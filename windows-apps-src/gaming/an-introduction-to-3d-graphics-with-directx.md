---
title: 適用於 DirectX 遊戲的基本 3D 圖形
description: 我們將示範如何使用 DirectX 程式設計來實作 3D 圖形的基本概念。
ms.assetid: 2989c91f-7b45-7377-4e83-9daa0325e92e
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, DirectX, 圖形
ms.localizationpriority: medium
ms.openlocfilehash: 556c5c74e5c8284e56047b4b8a9b2c2bf9c0155c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369068"
---
# <a name="basic-3d-graphics-for-directx-games"></a>適用於 DirectX 遊戲的基本 3D 圖形



我們將示範如何使用 DirectX 程式設計來實作 3D 圖形的基本概念。

**目標：** 了解如何撰寫 3D 圖形應用程式。

## <a name="prerequisites"></a>先決條件


我們假設您熟悉 C++。 您還需要圖形程式設計概念的基本經驗。

**若要完成的總時間：** 30 分鐘。

## <a name="where-to-go-from-here"></a>接下來該怎麼做


在這裡，我們討論如何開發使用 DirectX 的 3D 圖形和C++ \\Cx。 這個包含五個部分的教學課程將為您介紹 [Direct3D](https://docs.microsoft.com/windows/desktop/direct3d) API 及許多其他 DirectX 範例中也會用到的概念和程式碼。 這些部分彼此互相關聯，涵蓋範圍從為您的 UWP C++ app 設定 DirectX，到在基本型別上添加紋理和新增效果。

> **附註**  本教學課程會使用右手座標系統中資料行向量。 許多 DirectX 範例和 app 都使用左手座標系統搭配列向量。 如需更完整的圖形數學方案和支援左手座標系統搭配列向量的方案，請考慮使用 [DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/directxmath-portal)。 如需詳細資訊，請參閱[使用 DirectXMath 搭配 Direct3D](https://docs.microsoft.com/windows/desktop/dxmath/pg-xnamath-migration-d3dx)。

 

我們將示範如何：

-   使用 Windows 執行階段將 [Direct3D](https://docs.microsoft.com/windows/desktop/direct3d) 介面初始化
-   套用每一頂點著色器作業
-   設定幾何
-   將場景點陣化 (將 3D 場景平面化成 2D 投影)
-   消除隱藏的表面

> **注意**  

 

接著，我們會建立一個 Direct3D 裝置、交換鏈結和轉譯目標檢視，並將轉譯的影像呈現到顯示器。

[快速入門： 設定 DirectX 資源，並顯示影像](setting-up-directx-resources.md)

## <a name="related-topics"></a>相關主題


* [Direct3D 11 的圖形](https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)
* [DXGI](https://docs.microsoft.com/windows/desktop/direct3ddxgi/dx-graphics-dxgi)
* [HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl)

 

 




