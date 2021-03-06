---
title: Texture2D 和 Texture2DArray 子資源拼貼
description: 這些表格顯示了 Texture2D 和 Texture2DArray 子資源拼接的方式。
ms.assetid: 2DC14DFC-5299-44D9-895F-5A223D3FD530
keywords:
- Texture2D 和 Texture2DArray 子資源拼貼
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2e193ab7bce31c1f13cb40f04902922c6ff21056
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370908"
---
# <a name="texture2d-and-texture2darray-subresource-tiling"></a>Texture2D 和 Texture2DArray 子資源拼貼


這些表格顯示了 [**Texture2D**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2d) 和 [**Texture2DArray**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2darray) 子資源拼接的方式。 這些表格中的數值並未計入結尾 mip 包裝。

## <a name="span-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spansubresources-with-multisample-counts-of-1"></a><span id="Subresources-with-multisample-counts-of-1"></span><span id="subresources-with-multisample-counts-of-1"></span><span id="SUBRESOURCES-WITH-MULTISAMPLE-COUNTS-OF-1"></span>多重取樣計數 1 與子


下表顯示了 [**Texture2D**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2d) 和多重採樣次數為 1 之 [**Texture2DArray**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2darray) 子資源拼接的方式。

| 位元數/像素 (1 個樣本/像素) | 磚的維度 (像素，寬 x 高) |
|-----------------------------|-------------------------------|
| 8                           | 256 x 256                       |
| 16                          | 256 x 128                       |
| 32                          | 128 x 128                       |
| 64                          | 128 x 64                        |
| 128                         | 64x64                         |
| BC1,4                       | 512 x 256                       |
| BC2,3,5,6,7                 | 256 x 256                       |

 

不支援使用資料流資源的格式元計數是 96 bpp 格式的視訊格式，DXGI\_格式\_R1\_UNORM、 DXGI\_格式\_R8G8\_B8G8\_UNORM，和 DXGI\_格式\_R8R8\_G8B8\_UNORM。

## <a name="span-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspansubresources-with-various-multisample-counts"></a><span id="Subresources-with-various-multisample-counts"></span><span id="subresources-with-various-multisample-counts"></span><span id="SUBRESOURCES-WITH-VARIOUS-MULTISAMPLE-COUNTS"></span>子以各種不同的多重取樣計數


下表顯示了 [**Texture2D**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2d) 和多重採樣次數超過 1 之 [**Texture2DArray**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2darray) 子資源拼接的方式。

| 位元數/像素 (1 個樣本/像素) | 磚的維度 (像素，寬 x 高) |
|-----------------------------|-------------------------------|
| 1                           | 1 x 1                           |
| 2                           | 2 x 1                           |
| 4                           | 2 x 2                           |
| 8                           | 4 x 2                           |
| 16                          | 4 x 4                           |

 

只有樣本數 1 及 4 必須 (並且允許) 受到串流資源支援。 串流資源目前不支援 2、8，和 16，即使其仍會顯示。

實作上可以針對非串流資源選擇支援 2、8，和 (或) 16 樣本的多重採樣消除鋸齒 (MSAA) 模式，即使串流資源目前並不支援。

樣本數大於 1 的串流資源無法使用 128 bpp 格式。

支援的樣本數量和格式限制，乃是因為硬體規格的不一致。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[資料流資源的區域會並排顯示方式](how-a-streaming-resource-s-area-is-tiled.md)

 

 




