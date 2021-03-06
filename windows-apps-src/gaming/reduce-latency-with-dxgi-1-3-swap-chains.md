---
title: 透過 DXGI 1.3 交換鏈結減少延遲
description: 使用 DXGI 1.3 可減少有效的框架延遲，方法是等候交換鏈結在適當時機發出訊號來開始轉譯新畫面。
ms.assetid: c99b97ed-a757-879f-3d55-7ed77133f6ce
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, games, latency, dxgi, swap chains, directx, 遊戲, 延遲, 交換鏈結
ms.localizationpriority: medium
ms.openlocfilehash: 27ecce9d95d3c2e852b049e3cac9579850022df9
ms.sourcegitcommit: d2aabe027a2fff8a624111a00864d8986711cae6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/07/2020
ms.locfileid: "82880858"
---
# <a name="reduce-latency-with-dxgi-13-swap-chains"></a>透過 DXGI 1.3 交換鏈結減少延遲

使用 DXGI 1.3 可減少有效的框架延遲，方法是等候交換鏈結在適當時機發出訊號來開始轉譯新畫面。 遊戲通常需要提供最低的延遲量，範圍可能從接收到玩家輸入時，到遊戲藉由更新顯示器來回應該輸入時。 本主題說明在 Direct3D 11.2 中開始時提供的技術，您可以使用該技術來將遊戲中有效的框架延遲降至最低。

## <a name="how-does-waiting-on-the-back-buffer-reduce-latency"></a>在背景緩衝區上等候如何減少延遲？

使用翻轉模型交換鏈結，背景緩衝區「翻轉」會在您的遊戲呼叫 [**IDXGISwapChain::Present**](/windows/win32/api/dxgi/nf-dxgi-idxgiswapchain-present) 時排入佇列。 當轉譯迴圈呼叫 Present() 時，系統會封鎖執行緒，直到它完成先前框架的顯示為止，在它實際顯示之前，清出空間以將新框架排入佇列。 這會導致遊戲繪製框架的時間和系統允許它顯示該框架的時間之間，產生額外的延遲。 在許多情況下，系統將會到達一個穩定的平衡，遊戲在它轉譯的時間和它呈現每個畫面的時間之間，一律會等候一個幾乎完全是額外存在的畫面。 最好是等到系統準備好接受新畫面，然後根據目前資料來轉譯該畫面，並立即將該畫面排入佇列。

使用[**\_DXGI 交換\_鏈\_旗\_標框架\_延遲\_可等候\_物件**](/windows/win32/api/dxgi/ne-dxgi-dxgi_swap_chain_flag)旗標來建立可等候交換鏈。 使用此方法建立的交換鏈結，可以在系統真正準備好接受新框架時通知您的轉譯迴圈。 這讓您的遊戲可以根據目前的資料進行轉譯，然後立即在目前佇列中放置結果。

## <a name="step-1-create-a-waitable-swap-chain"></a>步驟 1： 建立可等候交換鏈

當您呼叫[**CreateSwapChainForCoreWindow**](/windows/win32/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow)時，指定[**\_DXGI\_交換\_\_\_鏈旗\_標畫面\_延遲可等候物件**](/windows/win32/api/dxgi/ne-dxgi-dxgi_swap_chain_flag)旗標。

```cpp
swapChainDesc.Flags = DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT; // Enable GetFrameLatencyWaitableObject().
```

> [!NOTE]
> 相對於某些旗標，無法使用[**ResizeBuffers**](/windows/win32/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers)來新增或移除此旗標。 如果這個旗標的設定與建立交換鏈結時的不同，則 DXGI 會傳回錯誤碼。

```cpp
// If the swap chain already exists, resize it.
HRESULT hr = m_swapChain->ResizeBuffers(
    2, // Double-buffered swap chain.
    static_cast<UINT>(m_d3dRenderTargetSize.Width),
    static_cast<UINT>(m_d3dRenderTargetSize.Height),
    DXGI_FORMAT_B8G8R8A8_UNORM,
    DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT // Enable GetFrameLatencyWaitableObject().
    );
```

## <a name="step-2-set-the-frame-latency"></a>步驟 2： 設定框架延遲

使用 [**IDXGISwapChain2::SetMaximumFrameLatency**](/windows/win32/api/dxgi1_3/nf-dxgi1_3-idxgiswapchain2-setmaximumframelatency) API 來設定畫面延遲，而不是呼叫 [**IDXGIDevice1::SetMaximumFrameLatency**](/windows/win32/api/dxgi/nf-dxgi-idxgidevice1-setmaximumframelatency)。

根據預設，會將適用於可等候交換鏈結的畫面延遲設為 1，這會盡可能產生最少的延遲，但也會降低 CPU-GPU 平行處理原則。 如果您需要提高 CPU-GPU 平行處理原則以達到 60 FPS (也就是說，如果 CPU 和 GPU 在框架處理轉譯工作時所花費的時間各少於 16.7 毫秒，但它們相加的總和大於 16.7 毫秒)，請將框架延遲設為 2。 這允許 GPU 處理在前一個框架期間由 CPU 排入佇列的工作，同時允許 CPU 獨立為目前框架提交轉譯命令。

```cpp
// Swapchains created with the DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT flag use their
// own per-swapchain latency setting instead of the one associated with the DXGI device. The
// default per-swapchain latency is 1, which ensures that DXGI does not queue more than one frame
// at a time. This both reduces latency and ensures that the application will only render after
// each VSync, minimizing power consumption.
//DX::ThrowIfFailed(
//    swapChain2->SetMaximumFrameLatency(1)
//    );
```

## <a name="step-3-get-the-waitable-object-from-the-swap-chain"></a>步驟 3： 從交換鏈取得可等候物件

呼叫 [**IDXGISwapChain2::GetFrameLatencyWaitableObject**](/windows/win32/api/dxgi1_3/nf-dxgi1_3-idxgiswapchain2-getframelatencywaitableobject) 以擷取等候控制代碼。 等候控制代碼是指向可等候物件的指標。 儲存此控制代碼，以供您的轉譯迴圈使用。

```cpp
// Get the frame latency waitable object, which is used by the WaitOnSwapChain method. This
// requires that swap chain be created with the DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT
// flag.
m_frameLatencyWaitableObject = swapChain2->GetFrameLatencyWaitableObject();
```

## <a name="step-4-wait-before-rendering-each-frame"></a>步驟 4： 在轉譯每個畫面格之前等候

您的轉譯迴圈在開始轉譯每個框架之前，應該先等候交換鏈結透過可等候的物件發出訊號。 這包含第一個使用交換鏈結轉譯的畫面。 使用 [**WaitForSingleObjectEx**](/windows/win32/api/synchapi/nf-synchapi-waitforsingleobjectex)，提供步驟 2 中抓取的等候控制代碼，為每個畫面的開始發出訊號。

下列範例顯示來自 DirectXLatency 範例的轉譯迴圈：

```cpp
while (!m_windowClosed)
{
    if (m_windowVisible)
    {
        // Block this thread until the swap chain is finished presenting. Note that it is
        // important to call this before the first Present in order to minimize the latency
        // of the swap chain.
        m_deviceResources->WaitOnSwapChain();

        // Process any UI events in the queue.
        CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

        // Update app state in response to any UI events that occurred.
        m_main->Update();

        // Render the scene.
        m_main->Render();

        // Present the scene.
        m_deviceResources->Present();
    }
    else
    {
        // The window is hidden. Block until a UI event occurs.
        CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
    }
}
```

下列範例顯示來自 DirectXLatency 範例的 WaitForSingleObjectEx 呼叫：

```cpp
// Block the current thread until the swap chain has finished presenting.
void DX::DeviceResources::WaitOnSwapChain()
{
    DWORD result = WaitForSingleObjectEx(
        m_frameLatencyWaitableObject,
        1000, // 1 second timeout (shouldn't ever occur)
        true
        );
}
```

## <a name="what-should-my-game-do-while-it-waits-for-the-swap-chain-to-present"></a>當我的遊戲在等候交換鏈結出現時應該做些什麼？

如果您的遊戲不含任何會在轉譯迴圈上封鎖的工作，讓它等候交換鏈結出現就非常有利，因為能夠節省電力，這在行動裝置上特別重要。 否則，當您的遊戲在等候交換鏈結出現時，您可以使用多執行緒處理來完成工作。 以下只是一些您的遊戲可以完成的工作：

-   處理網路事件
-   更新 AI
-   以 CPU 為基礎的物理計算
-   轉譯延遲的內容 (在支援的裝置上)
-   載入資產

如需 Windows 中多執行緒程式設計的詳細資訊，請參閱下列相關主題。

## <a name="related-topics"></a>相關主題

* [DirectX 延遲範例應用程式](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/DirectX%20latency%20sample)
* [**IDXGISwapChain2::GetFrameLatencyWaitableObject**](/windows/win32/api/dxgi1_3/nf-dxgi1_3-idxgiswapchain2-getframelatencywaitableobject)
* [**WaitForSingleObjectEx**](/windows/win32/api/synchapi/nf-synchapi-waitforsingleobjectex)
* [**Windows.System.Threading**](/uwp/api/Windows.System.Threading)
* [C + + 中的非同步程式設計](/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps)
* [進程和執行緒](/windows/win32/procthread/processes-and-threads)
* [參與](/windows/win32/sync/synchronization)
* [使用事件物件 (Windows)](/windows/win32/sync/using-event-objects)