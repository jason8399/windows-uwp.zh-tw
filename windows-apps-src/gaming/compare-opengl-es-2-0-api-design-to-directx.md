---
title: 計劃從 OpenGL ES 2.0 移植到 Direct3D
description: 如果您是從 iOS 或 Android 平台移植遊戲，可能已經在 OpenGL ES 2.0 投入了大量的心力。
ms.assetid: a31b8c5a-5577-4142-fc60-53217302ec3a
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, OpenGL, Direct3D
ms.localizationpriority: medium
ms.openlocfilehash: 44b851ef96b93974724ff4cf0b309d119dfd72d7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368958"
---
# <a name="plan-your-port-from-opengl-es-20-to-direct3d"></a>計劃從 OpenGL ES 2.0 移植到 Direct3D




**重要的 Api**

-   [Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)
-   [Visual C++](https://docs.microsoft.com/previous-versions/60k1461a(v=vs.140))

如果您是從 iOS 或 Android 平台移植遊戲，可能已經在 OpenGL ES 2.0 投入了大量的心力。 在準備將圖形管線程式碼基底移到 Direct3D 11 與 Windows 執行階段時，有幾件事在開始之前必須先行考量。

大部分的移植工作通常一開始是移動程式碼基底，以及對應兩個模型之間的共用 API 與模式。 如果您花些時間閱讀和探討這個主題，會發現這個程序變得更簡單。

從 OpenGL ES 2.0 將圖形移植到 Direct3D 11 時，有幾件事需要注意。

## <a name="notes-on-specific-opengl-es-20-providers"></a>關於特定 OpenGL ES 2.0 提供者的注意事項


本節的移植主題參考 OpenGL ES 2.0 規格的 Windows 實作，由 Khronos Group 所建立。 所有 OpenGL ES 2.0 程式碼範例都是使用 Visual Studio 2012 與基本 Windows C 語法開發。 如果您使用 Objective-C (iOS) 或 Java (Android) 程式碼基底，請注意所提供的 OpenGL ES 2.0 程式碼範例可能不是使用類似的 API 呼叫語法或參數。 這個指引盡可能嘗試不要加入不同平台的因素。

這份文件在 OpenGL ES 程式碼與參考只使用 2.0 規格 API。 如果您從 OpenGL ES 1.1 或 3.0 移植，雖然可能會對某些 OpenGL ES 2.0 程式碼範例與內容不甚熟悉，但本文內容仍然有用。

這些主題中的 Direct3D 11 範例使用 Microsoft Windows C++ 搭配元件延伸 (CX)。 如需詳細資訊，在此版本的C++語法，讀取[Visual C++ ](https://docs.microsoft.com/previous-versions/60k1461a(v=vs.140))，[元件 Extensions for Runtime Platforms](https://docs.microsoft.com/cpp/windows/component-extensions-for-runtime-platforms)，和[快速參考 (C++\\CX)](https://docs.microsoft.com/cpp/cppcx/quick-reference-c-cx).

## <a name="understand-your-hardware-requirements-and-resources"></a>了解您的硬體需求與資源


OpenGL ES 2.0 支援的圖形處理功能集大致上可對應至 Direct3D 9.1 提供的功能。 如果您想要利用 Direct3D 11 提供的進階功能，在規劃移植時請檢閱 [Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11) 文件， 或在完成一開始的工作時檢閱[從 DirectX 9 移植到通用 Windows 平台 (UWP)](porting-your-directx-9-game-to-windows-store.md)主題。

若要簡化一開始的移植工作，請使用 Visual Studio Direct3D 範本。 該範本提供已經設定好的基本轉譯器，且支援 UWP app 功能， 例如重建視窗變更的資源與 Direct3D 功能層級。

## <a name="understand-direct3d-feature-levels"></a>了解 Direct3D 功能層級


Direct3D 11 支援硬體 「 功能層級 」 9\_1 (Direct3D 9.1) 11\_1。 這些功能層級表示某些圖形功能與資源的可用性。 一般而言，大部分的 OpenGL ES 2.0 平台支援 Direct3D 9.1 (功能層級 9\_1) 的功能集。

## <a name="review-directx-graphics-features-and-apis"></a>檢閱 DirectX 圖形功能與 API


| API 系列                                                | 描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-----------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [DXGI](https://docs.microsoft.com/windows/desktop/direct3ddxgi/dx-graphics-dxgi)                     | DirectX Graphics Infrastructure (DXGI) 提供圖形硬體與 Direct3D 間的介面。 它使用 [**IDXGIAdapter**](https://docs.microsoft.com/windows/desktop/api/dxgi/nn-dxgi-idxgiadapter) 與 [**IDXGIDevice1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgidevice2) COM 介面設定裝置介面卡與硬體設定。 使用此基礎結構可建立和設定您的緩衝區與其他視窗資源。 尤其，會使用 [**IDXGIFactory2**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2) Factory 模式取得圖形資源，包含交換鏈結 (一組框架緩衝區)。 因為 DXGI 擁有交換鏈結，所以 [**IDXGISwapChain1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) 介面會用來將框架呈現在螢幕上。 |
| [Direct3D](https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)       | Direct3D 是一組 API，提供圖形介面的視覺呈現，可讓您用來繪製圖形。 11 版的功能大致與 OpenGL 4.3 相當。 （OpenGL ES 2.0，相反地，大致 DirectX9，到，和 OpenGL 2.0，但使用 OpenGL 3.0 整合著色器管線）。ID3D11Device1 和 ID3D11DeviceContext1 介面可以存取個別的資源和子，並呈現內容中，分別是完成大部分的苦差事。                                                                                                                                          |
| [Direct2D](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-portal)                      | Direct2D 提供一組 API，用於 GPU 加速的 2D 轉譯。 用途與 OpenVG 相似。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| [DirectWrite](https://docs.microsoft.com/windows/desktop/DirectWrite/direct-write-portal)            | DirectWrite 提供一組 API，用於 GPU 加速、高品質字型轉譯。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| [DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/directxmath-portal)                  | DirectXMath 提供一組 API 與巨集，用於處理共用的線性代數與三角函數類型、值與函式。 這些類型與函式的設計可完美搭配 Direct3D 與其著色器操作使用。                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| [DirectX HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-common-core) | Direct3D 著色器目前所使用的 HLSL 語法。 實作 Direct3D 著色器模型 5.0。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

 

## <a name="review-the-windows-runtime-apis-and-template-library"></a>檢閱 Windows 執行階段 API 與範本庫


Windows 執行階段 API 提供適用於 UWP app 的整體基礎結構。 您可以在[這裡](https://docs.microsoft.com/uwp/api/)檢閱。

用於移植圖形管線的主要 Windows 執行階段 API 包含：

-   [**Windows::UI::Core::CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow)
-   [**Windows::UI::Core::CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher)
-   [**Windows::ApplicationModel::Core::IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView)
-   [**Windows::ApplicationModel::Core::CoreApplicationView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView)

此外，Windows 執行階段 C++ 範本庫 (WRL) 提供編寫與使用 Windows 執行階段元件的低階方式。 適用於 UWP app 的 Direct3D 11 API 最好搭配此程式庫中的介面與類型使用， 例如智慧型指標 ([ComPtr](https://docs.microsoft.com/cpp/windows/comptr-class))。 如需 WRL 的詳細資訊，請閱讀 [Windows 執行階段 C++ 範本庫 (WRL)](https://docs.microsoft.com/cpp/windows/windows-runtime-cpp-template-library-wrl)。

## <a name="change-your-coordinate-system"></a>變更座標系統


在前期移植工作中，有一個可能造成混淆的不同點在於，從 OpenGL 的傳統右手座標系統變更為 Direct3D 的預設左手座標系統。 這項座標模型的變更影響遊戲的許多部分，從頂點緩衝區的設定和組態到許多矩陣數學函式。 要進行的兩項最重要變更為：

-   翻轉三角形頂點的順序，讓 Direct3D 從前面順時針轉動這些頂點。 例如，如果在 OpenGL 管線中以 0、1 與 2 指示頂點，請改以 0、2、1 順序傳遞至 Direct3D。
-   使用視圖矩陣將 z 方向的全局空間大小調整為 -1.0f，可有效率地轉換 z 軸座標的方向。 若要這樣做，請在視圖矩陣中翻轉位置 M31、M32 與 M33 的值符號 (當移植到 [**Matrix**](https://docs.microsoft.com/windows/desktop/direct3d9/matrix4x4) 類型時)。 如果 M34 不是 0，也翻轉其符號。

不過，Direct3D 可支援右手座標系統。 DirectXMath 提供數個函式，可同時在左手與右手座標系統上運作。 可用它們來保留您部分的原始網格資料與矩陣處理。 其中包括：

| DirectXMath 矩陣函式                                                   | 描述                                                                                                                 |
|-------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| [**XMMatrixLookAtLH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlookatlh)                               | 使用相機位置、向上方向與焦點建置左手座標系統的視圖矩陣。       |
| [**XMMatrixLookAtRH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlookatrh)                               | 使用相機位置、向上方向與焦點建置右手座標系統的視圖矩陣。      |
| [**XMMatrixLookToLH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlooktolh)                               | 使用相機位置、向上方向與相機方向建置左手座標系統的視圖矩陣。  |
| [**XMMatrixLookToRH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlooktorh)                               | 使用相機位置、向上方向與相機方向建置右手座標系統的視圖矩陣。 |
| [**XMMatrixOrthographicLH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographiclh)                   | 建置左手座標系統的正交投影矩陣。                                                 |
| [**XMMatrixOrthographicOffCenterLH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographicoffcenterlh) | 建置左手座標系統的自訂正交投影矩陣。                                           |
| [**XMMatrixOrthographicOffCenterRH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographicoffcenterrh) | 建置右手座標系統的自訂正交投影矩陣。                                          |
| [**XMMatrixOrthographicRH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographicrh)                   | 建置右手座標系統的正交投影矩陣。                                                |
| [**XMMatrixPerspectiveFovLH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectivefovlh)               | 根據視野範圍建置左手透視投影矩陣。                                                |
| [**XMMatrixPerspectiveFovRH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectivefovrh)               | 根據視野範圍建置右手透視投影矩陣。                                               |
| [**XMMatrixPerspectiveLH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectivelh)                     | 建置左手透視投影矩陣。                                                                         |
| [**XMMatrixPerspectiveOffCenterLH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectiveoffcenterlh)   | 建置自訂版本的左手透視投影矩陣。                                                     |
| [**XMMatrixPerspectiveOffCenterRH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectiveoffcenterrh)   | 建置自訂版本的右手透視投影矩陣。                                                    |
| [**XMMatrixPerspectiveRH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectiverh)                     | 建置右手透視投影矩陣。                                                                        |

 

## <a name="opengl-es20-to-direct3d-11-porting-frequently-asked-questions"></a>OpenGL ES2.0 到 Direct3D 11 移植常見問題集


-   問題：「 一般情況下，可以我搜尋特定字串或模式在我的 OpenGL 程式碼，並取代 Direct3D 對等項目嗎？ 」
-   回答：資料分割 OpenGL ES 2.0 與 Direct3D 11 來自不同的圖形管線模型世代。 雖然在概念與 API 上有些許類似，例如轉譯內容與著色器執行個體，您還是應該檢閱此指引與 Direct3D 11 參考，才能在重建管線時做出最好的選擇，而不是嘗試一對一的對應。 不過，如果您是從 GLSL 移植到 HLSL，為 GLSL 變數、內建與函式建立一組共用別名，不僅可讓移植變得更容易，還可讓您只維護一組著色器程式碼檔案。

 

 




