---
title: 移植遊戲迴圈
description: 說明如何為通用 Windows 平台 (UWP) 遊戲實作視窗，以及如何帶入遊戲迴圈，其中包含如何建置 IFrameworkView 以控制全螢幕的 CoreWindow。
ms.assetid: 070dd802-cb27-4672-12ba-a7f036ff495c
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 遊戲, 移植, 遊戲迴圈, direct3d 9, directx 11
ms.localizationpriority: medium
ms.openlocfilehash: 9b3a18d9ee63a2ecded07f8b779195d5274b6210
ms.sourcegitcommit: 734aa941dc675157c07bdeba5059cb76a5626b39
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/15/2019
ms.locfileid: "68141825"
---
# <a name="port-the-game-loop"></a>移植遊戲迴圈



**摘要**

-   [第 1 部分：初始化 Direct3D 11](simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md)
-   [第 2 部分：轉換轉譯架構](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)
-   第 3 部分：移植遊戲迴圈


說明如何為通用 Windows 平台 (UWP) 遊戲實作視窗，以及如何帶入遊戲迴圈，其中包含如何建置 [**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) 以控制全螢幕的 [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow)。 [將簡單的 Direct3D 9 app 移植到 DirectX 11 和 UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md) 逐步解說的第三部分。

## <a name="create-a-window"></a>建立視窗


若要使用 Direct3D 9 檢視區來設定桌面視窗，必須針對傳統型應用程式實作傳統視窗架構。 我們過去必須建立 HWND、設定視窗大小、提供視窗處理回呼、讓它變成可見，以及其他動作等等。

UWP 環境現在提供一個更簡單的系統。 使用 DirectX 的 Microsoft Store 遊戲會實作 [**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView)，而不是設定傳統視窗。 為了 DirectX app 與遊戲存在的這個介面，可直接在 app 容器內的 [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) 中執行。

> **附註**   Windows 提供的資源，例如來源應用程式物件的 managed 的指標和[ **CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow)。 請參閱[**物件控制代碼運算子 (^)** ](https://msdn.microsoft.com/library/windows/apps/yk97tc08.aspx)。

 

您的 「 主要 」 類別必須繼承自[ **IFrameworkView** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView)並實作五**IFrameworkView**方法：[**初始化**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.initialize)， [ **SetWindow**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow)， [**負載**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.load)， [**執行**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.run)，並[**解除初始化**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize)。 除了建立 **IFrameworkView** (這 (基本上) 將是您遊戲所在的位置)，您還需要實作 Factory 類別，以建立 **IFrameworkView** 的執行個體。 您的遊戲仍含有名稱為 **main()** 方法的可執行檔，但是所有的 main 都會使用 Factory 來建立 **IFrameworkView** 執行個體。

Main 函式

```cpp
//-----------------------------------------------------------------------------
// Required method for a DirectX-only app.
// The main function is only used to initialize the app's IFrameworkView class.
//-----------------------------------------------------------------------------
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}
```

IFrameworkView Factory

```cpp
//-----------------------------------------------------------------------------
// This class creates our IFrameworkView.
//-----------------------------------------------------------------------------
ref class Direct3DApplicationSource sealed : 
    Windows::ApplicationModel::Core::IFrameworkViewSource
{
public:
    virtual Windows::ApplicationModel::Core::IFrameworkView^ CreateView()
    {
        return ref new Cube11();
    };
};
```

## <a name="port-the-game-loop"></a>移植遊戲迴圈


讓我們看看來自 Direct3D 9 實作的遊戲迴圈。 這個程式碼存在於應用程式的 main 函式。 這個迴圈的每一個反覆項目都會處理視窗訊息或轉譯框架。

Direct3D 9 傳統型遊戲中的遊戲迴圈

```cpp
while(WM_QUIT != msg.message)
{
    // Process window events.
    // Use PeekMessage() so we can use idle time to render the scene. 
    bGotMsg = (PeekMessage(&msg, NULL, 0U, 0U, PM_REMOVE) != 0);

    if(bGotMsg)
    {
        // Translate and dispatch the message
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }
    else
    {
        // Render a new frame.
        // Render frames during idle time (when no messages are waiting).
        RenderFrame();
    }
}
```

在遊戲的 UWP 版本中，遊戲迴圈很類似 - 但更簡單：

遊戲迴圈會在 [**IFrameworkView::Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.run) 方法中執行 (而非 **main()** )，因為我們的遊戲會在 [**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) 類別中運作。

我們可以呼叫內建於應用程式視窗的 [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) 的 [**ProcessEvents**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.processevents) 方法，而不需實作訊息處理架構並呼叫 [**PeekMessage**](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-peekmessagea)。 遊戲迴圈不需要有分支和處理訊息 - 只需呼叫 **ProcessEvents** 並繼續。

Direct3D 11 Microsoft Store 遊戲中的遊戲迴圈

```cpp
// UWP apps should not exit. Use app lifecycle events instead.
while (true)
{
    // Process window events.
    auto dispatcher = CoreWindow::GetForCurrentThread()->Dispatcher;
    dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

    // Render a new frame.
    RenderFrame();
}
```

現在，我們的 UWP app 已設定與 DirectX 9 範例相同的基本圖形基礎架構，以及轉譯相同的彩色立方體。

## <a name="where-do-i-go-from-here"></a>接下來還可以做什麼？


將[移植 DirectX 11 常見問題集](directx-porting-faq.md)設定為書籤。

DirectX UWP 範本包含可靠的 Direct3D 裝置基礎結構，且已準備好與您的遊戲搭配使用。 如需挑選正確範本的指導方針，請參閱[從範本建立 DirectX 遊戲專案](user-interface.md)。

請造訪下列深入 Microsoft Store 遊戲開發文件：

-   [逐步解說： 簡單的 UWP 遊戲搭配 DirectX](tutorial--create-your-first-uwp-directx-game.md)
-   [適用於遊戲的音訊](working-with-audio-in-your-directx-game.md)
-   [適用於遊戲的移動尋找控制項](tutorial--adding-move-look-controls-to-your-directx-game.md)

 

 




