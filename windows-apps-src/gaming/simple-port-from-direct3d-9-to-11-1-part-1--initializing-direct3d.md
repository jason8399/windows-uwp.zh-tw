---
title: 初始化 Direct3D 11
description: 示範如何將 Direct3D 9 初始化程式碼轉換成 Direct3D 11，包含如何取得 Direct3D 裝置的控制代碼與裝置內容，以及如何使用 DXGI 來設定交換鏈結。
ms.assetid: 1bd5e8b7-fd9d-065c-9ff3-1a9b1c90da29
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 遊戲, direct3d 11, 初始設定, 移植, direct3d 9
ms.localizationpriority: medium
ms.openlocfilehash: c5a7f33ddbc6d70af5293b92165892c2098e452d
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368037"
---
# <a name="initialize-direct3d-11"></a>初始化 Direct3D 11



**摘要**

-   第 1 部分：初始化 Direct3D 11
-   [第 2 部分：轉換轉譯架構](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)
-   [第 3 部分：連接埠遊戲迴圈](simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md)


示範如何將 Direct3D 9 初始化程式碼轉換成 Direct3D 11，包含如何取得 Direct3D 裝置的控制代碼與裝置內容，以及如何使用 DXGI 來設定交換鏈結。 [將簡單的 Direct3D 9 app 移植到 DirectX 11 和通用 Windows 平台 (UWP)](walkthrough--simple-port-from-direct3d-9-to-11-1.md) 逐步解說的第一部分。

## <a name="initialize-the-direct3d-device"></a>初始化 Direct3D 裝置


在 Direct3D 9 中，我們透過呼叫 [**IDirect3D9::CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3d9-createdevice) 來建立 Direct3D 裝置的控制代碼。 我們一開始先取得 [**IDirect3D9 interface**](https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3d9) 的指標，並指定一些參數來控制 Direct3D 裝置與交換鏈結的設定。 在這樣做之前，我們已呼叫 [**GetDeviceCaps**](https://docs.microsoft.com/windows/desktop/api/wingdi/nf-wingdi-getdevicecaps)，確定我們沒有要求裝置執行某些它無法執行的動作。

Direct3D 9

```cpp
UINT32 AdapterOrdinal = 0;
D3DDEVTYPE DeviceType = D3DDEVTYPE_HAL;
D3DCAPS9 caps;
m_pD3D->GetDeviceCaps(AdapterOrdinal, DeviceType, &caps); // caps bits

D3DPRESENT_PARAMETERS params;
ZeroMemory(&params, sizeof(D3DPRESENT_PARAMETERS));

// Swap chain parameters:
params.hDeviceWindow = m_hWnd;
params.AutoDepthStencilFormat = D3DFMT_D24X8;
params.BackBufferFormat = D3DFMT_X8R8G8B8;
params.MultiSampleQuality = D3DMULTISAMPLE_NONE;
params.MultiSampleType = D3DMULTISAMPLE_NONE;
params.SwapEffect = D3DSWAPEFFECT_DISCARD;
params.Windowed = true;
params.PresentationInterval = 0;
params.BackBufferCount = 2;
params.BackBufferWidth = 0;
params.BackBufferHeight = 0;
params.EnableAutoDepthStencil = true;
params.Flags = 2;

m_pD3D->CreateDevice(
    0,
    D3DDEVTYPE_HAL,
    m_hWnd,
    64,
    &params,
    &m_pd3dDevice
    );
```

在 Direct3D 11 中，裝置內容與圖形基礎結構被視為與裝置本身不同。 初始化分成數個步驟。

首先，我們會建立裝置。 我們取得裝置支援的功能層級清單 - 這會告知大部分我們需要了解有關 GPU 的事項。 此外，不需要建立只為存取 Direct3D 的介面， 而是應該使用 [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 核心 API。 這個 API 可以為我們提供裝置的控制代碼與裝置的直接內容。 裝置內容是用來設定管線狀態和產生轉譯命令。

建立 Direct3D 11 裝置與內容之後，我們可以利用 COM 指標功能來取得包含額外功能的最新介面版本 (一律建議您取得最新版本)。

> **附註**   D3D\_功能\_層級\_9\_1 （其對應至著色器模型 2.0） 是您的 Microsoft Store 遊戲，才可支援的最低層級。 (您的遊戲 ARM 套件將會失敗憑證，如果您不支援 9\_1。)如果您的遊戲也包含轉譯路徑著色器模型 3 的功能，則您應該在包含 D3D\_功能\_層級\_9\_3 陣列中的。

 

Direct3D 11

```cpp
// This flag adds support for surfaces with a different color channel 
// ordering than the API default. It is required for compatibility with
// Direct2D.
UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;

#if defined(_DEBUG)
// If the project is in a debug build, enable debugging via SDK Layers.
creationFlags |= D3D11_CREATE_DEVICE_DEBUG;
#endif

// This example only uses feature level 9.1.
D3D_FEATURE_LEVEL featureLevels[] = 
{
    D3D_FEATURE_LEVEL_9_1
};

// Create the Direct3D 11 API device object and a corresponding context.
ComPtr<ID3D11Device> device;
ComPtr<ID3D11DeviceContext> context;
D3D11CreateDevice(
    nullptr, // Specify nullptr to use the default adapter.
    D3D_DRIVER_TYPE_HARDWARE,
    nullptr,
    creationFlags,
    featureLevels,
    ARRAYSIZE(featureLevels),
    D3D11_SDK_VERSION, // UWP apps must set this to D3D11_SDK_VERSION.
    &device, // Returns the Direct3D device created.
    nullptr,
    &context // Returns the device immediate context.
    );

// Store pointers to the Direct3D 11.2 API device and immediate context.
device.As(&m_d3dDevice);

context.As(&m_d3dContext);
```

## <a name="create-a-swap-chain"></a>建立交換鏈結


Direct3D 11 包含稱為 DirectX Graphics Infrastructure (DXGI) 的裝置 API。 DXGI 介面讓我們能夠 (舉例來說) 控制如何設定交換鏈結和設定共用裝置。 在初始化 Direct3D 的這個步驟中，我們將使用 DXGI 來建立交換鏈結。 由於我們已建立裝置，因此可以遵循介面鏈結回到 DXGI 介面卡。

Direct3D 裝置會實作 DXGI 的 COM 介面。 首先，我們需要取得該介面，並使用它來要求裝載裝置的 DXGI 介面卡。 接著，使用 DXGI 介面卡來建立 DXGI Factory。

> **附註**  這些是 COM 介面，因此您第一次的回應可能會使用[ **QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))。 您應該改用 [**Microsoft::WRL::ComPtr**](https://docs.microsoft.com/cpp/windows/comptr-class) 智慧型指標。 接著，只需呼叫 [**As()** ](https://docs.microsoft.com/previous-versions/br230426(v=vs.140)) 方法，提供正確介面類型的空 COM 指標。

 

**Direct3D 11**

```cpp
ComPtr<IDXGIDevice2> dxgiDevice;
m_d3dDevice.As(&dxgiDevice);

// Then, the adapter hosting the device;
ComPtr<IDXGIAdapter> dxgiAdapter;
dxgiDevice->GetAdapter(&dxgiAdapter);

// Then, the factory that created the adapter interface:
ComPtr<IDXGIFactory2> dxgiFactory;
dxgiAdapter->GetParent(
    __uuidof(IDXGIFactory2),
    &dxgiFactory
    );
```

現在，我們已經有 DXGI Factory，可以使用它來建立交換鏈結。 讓我們來定義交換鏈結參數。 我們需要指定介面的格式;我們將選擇[ **DXGI\_格式\_B8G8R8A8\_UNORM** ](https://docs.microsoft.com/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)因為它是與 Direct2D 相容。 我們關閉顯示縮放比例、多重取樣及立體著色運算，因為這個範例中並未用到它們。 由於我們是直接在 CoreWindow 中執行，所以能夠讓寬度與高度保留為 0 的設定，並自動取得全螢幕值。

> **附註**  永遠組*SDKVersion*參數，以 D3D11\_SDK\_UWP 應用程式的版本。

 

**Direct3D 11**

```cpp
ComPtr<IDXGISwapChain1> swapChain;
dxgiFactory->CreateSwapChainForCoreWindow(
    m_d3dDevice.Get(),
    reinterpret_cast<IUnknown*>(window),
    &swapChainDesc,
    nullptr,
    &swapChain
    );
swapChain.As(&m_swapChain);
```

若要確保我們不比實際顯示畫面可以更常轉譯，我們設定 1 」 和 「 使用畫面格延遲[ **DXGI\_交換\_效果\_FLIP\_循序**](https://docs.microsoft.com/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_effect). 這樣就能節省電源，而且這是一項市集憑證需求；我們將在這個逐步解說的第二部分中深入了解如何在螢幕上顯示。

> **附註**  您可以使用多執行緒處理 (例如[ **ThreadPool** ](https://docs.microsoft.com/uwp/api/Windows.System.Threading)工作項目) 呈現執行緒封鎖時，請繼續執行其他工作。

 

**Direct3D 11**

```cpp
dxgiDevice->SetMaximumFrameLatency(1);
```

現在，我們可以設定背景緩衝區來進行轉譯。

## <a name="configure-the-back-buffer-as-a-render-target"></a>設定背景緩衝區做為轉譯目標。


首先，我們必須取得背景緩衝區的控制代碼 （請注意，背景緩衝區屬於 DXGI 交換鏈結，而在 DirectX 9 它之擁有者為 Direct3D 裝置。）然後我們告訴她能夠使用它做為呈現目標建立呈現目標的 Direct3D 裝置*檢視*使用背景緩衝區。

**Direct3D 11**

```cpp
ComPtr<ID3D11Texture2D> backBuffer;
m_swapChain->GetBuffer(
    0,
    __uuidof(ID3D11Texture2D),
    &backBuffer
    );

// Create a render target view on the back buffer.
m_d3dDevice->CreateRenderTargetView(
    backBuffer.Get(),
    nullptr,
    &m_renderTargetView
    );
```

現在裝置內容已能播放。 我們使用裝置內容介面來告知 Direct3D，使用最新建立的轉譯目標檢視。 我們將擷取背景緩衝區的寬度與高度，如此便能將整個視窗的目標設定為我們的檢視區。 請注意，背景緩衝區是附加到交換鏈結，因此，如果視窗大小變更 (例如，使用者將遊戲視窗拖曳到另一部監視器)，背景緩衝區將需重新調整大小，而部分設定將需重新執行。

**Direct3D 11**

```cpp
D3D11_TEXTURE2D_DESC backBufferDesc = {0};
backBuffer->GetDesc(&backBufferDesc);

CD3D11_VIEWPORT viewport(
    0.0f,
    0.0f,
    static_cast<float>(backBufferDesc.Width),
    static_cast<float>(backBufferDesc.Height)
    );

m_d3dContext->RSSetViewports(1, &viewport);
```

現在我們有裝置控制代碼與全螢幕轉譯目標，所以已經準備好載入與繪製幾何圖形。 若要繼續[第 2 部分：轉譯](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)。

 

 




