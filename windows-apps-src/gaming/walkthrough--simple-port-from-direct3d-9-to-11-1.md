---
title: 逐步解說 - 從 DirectX3D 9 移植到 DirectX 11 和 UWP
description: 這個移植練習示範如何將簡單的轉譯架構從 Direct3D 9 移到 Direct3D 11 和通用 Windows 平台 (UWP)。
ms.assetid: d4467e1f-929b-a4b8-b233-e142a8714c96
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 遊戲, directx, 移植, direct3d 9, direct3d 11
ms.localizationpriority: medium
ms.openlocfilehash: 5d4aef73b9b28d631a492436ff90761541134220
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66367419"
---
# <a name="walkthrough-port-a-simple-direct3d-9-app-to-directx-11-and-universal-windows-platform-uwp"></a>逐步解說：簡單 Direct3D 9 應用程式移植到 DirectX 11 和通用 Windows 平台 (UWP)



這個移植練習示範如何將簡單的轉譯架構從 Direct3D 9 移到 Direct3D 11 和通用 Windows 平台 (UWP)。
## 
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主題</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md">初始化 Direct3D 11</a></p></td>
<td align="left"><p>示範如何將 Direct3D 9 初始化程式碼轉換成 Direct3D 11，包含如何取得 Direct3D 裝置的控制代碼與裝置內容，以及如何使用 DXGI 來設定交換鏈結。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="simple-port-from-direct3d-9-to-11-1-part-2--rendering.md">轉換轉譯架構</a></p></td>
<td align="left"><p>示範如何將簡單的轉譯架構從 Direct3D 9 轉換到 Direct3D 11，包含如何移植幾何緩衝區、如何編譯和載入 HLSL 著色器程式，以及如何在 Direct3D 11 中實作轉譯鏈結。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md">連接埠遊戲迴圈</a></p></td>
<td align="left"><p>說明如何為通用 Windows 平台 (UWP) 遊戲實作視窗，以及如何帶入遊戲迴圈，其中包含如何建置 <a href="https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView"><strong>IFrameworkView</strong></a> 以控制全螢幕的 <a href="https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow"><strong>CoreWindow</strong></a>。</p></td>
</tr>
</tbody>
</table>

 

本主題將逐步解說兩個執行相同基本圖形工作的程式碼路徑：顯示旋轉的頂點著色立方體。 在這兩個案例中，程式碼會涵蓋下列程序：

1.  建立 Direct3D 裝置和交換鏈結。
2.  建立頂點緩衝區與索引緩衝區，表示色彩豐富的立方體網格。
3.  建立將頂點轉換成螢幕空間的頂點著色器、混合色彩值的像素著色器、編譯著色器，以及將著色器載入為 Direct3D 資源。
4.  實作轉譯鏈結以及在螢幕上呈現繪製的立方體。
5.  建立視窗、開始主要迴圈與負責視窗訊息處理。

完成這個逐步解說之後，您應該就會熟悉以下的 Direct3D 9 和 Direct3D 11 基本差異：

-   裝置、裝置內容和圖形基礎結構的區隔。
-   編譯著色器的程序以及在執行階段載入著色器位元組程式碼。
-   如何為輸入組合語言 (IA) 階段設定每個頂點資料。
-   如何使用 [**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) 建立 CoreWindow 檢視。

請注意，為了簡單起見，本逐步解說使用 [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow)，而且不涵蓋 XAML 互通性。

## <a name="prerequisites"></a>先決條件


您應[為 UWP DirectX 遊戲開發準備開發環境](prepare-your-dev-environment-for-windows-store-directx-game-development.md)。 您不需要的範本，但您將需要 Microsoft Visual Studio 2015 載入這個逐步解說的程式碼範例。

如需進一步了解這個逐步解說中說明的 DirectX 11 和 UWP 程式設計概念，請瀏覽[移植概念和考量](porting-considerations.md)。

## <a name="related-topics"></a>相關主題

**Direct3D**

* [在 Direct3D 中撰寫 HLSL 著色器 9](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-writing-shaders-9)
* [DirectX 遊戲專案範本](user-interface.md)

**Microsoft 網上商店**

* [**Microsoft::WRL::ComPtr**](https://docs.microsoft.com/cpp/windows/comptr-class)
* [**物件控制代碼運算子 (^)** ](https://docs.microsoft.com/cpp/windows/handle-to-object-operator-hat-cpp-component-extensions)

