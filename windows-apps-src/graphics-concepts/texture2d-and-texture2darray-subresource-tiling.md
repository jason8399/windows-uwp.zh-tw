---
title: "Texture2D 和 Texture2DArray 子資源拼接"
description: "這些表格顯示了 Texture2D 和 Texture2DArray 子資源拼接的方式。"
ms.assetid: 2DC14DFC-5299-44D9-895F-5A223D3FD530
keywords: "Texture2D 和 Texture2DArray 子資源拼接"
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 292adb2f06022fbb8fc063c49442cd69ccf64534
ms.sourcegitcommit: c80b9e6589a1ee29c5032a0b942e6a024c224ea7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/22/2017
---
# <a name="texture2d-and-texture2darray-subresource-tiling"></a>Texture2D 和 Texture2DArray 子資源拼接


這些表格顯示了 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) 和 [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526) 子資源拼接的方式。 這些表格中的數值並未計入結尾 mip 包裝。

## <a name="span-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spansubresources-with-multisample-counts-of-1"></a><span id="Subresources-with-multisample-counts-of-1"></span><span id="subresources-with-multisample-counts-of-1"></span><span id="SUBRESOURCES-WITH-MULTISAMPLE-COUNTS-OF-1"></span>多重採樣次數為 1 之子資源


下表顯示了 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) 和多重採樣次數為 1 之 [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526) 子資源拼接的方式。

| 位元數/像素 (1 個樣本/像素) | 磚的維度 (像素，寬 x 高) |
|-----------------------------|-------------------------------|
| 8                           | 256 x 256                       |
| 16                          | 256 x 128                       |
| 32                          | 128 x 128                       |
| 64                          | 128 x 64                        |
| 128                         | 64 x 64                         |
| BC1,4                       | 512 x 256                       |
| BC2,3,5,6,7                 | 256 x 256                       |

 

不受串流資源支援的格式位元計算包括了 96 bpp 格式、視訊格式、DXGI\_FORMAT\_R1\_UNORM、DXGI\_FORMAT\_R8G8\_B8G8\_UNORM，以及 DXGI\_FORMAT\_R8R8\_G8B8\_UNORM。

## <a name="span-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspansubresources-with-various-multisample-counts"></a><span id="Subresources-with-various-multisample-counts"></span><span id="subresources-with-various-multisample-counts"></span><span id="SUBRESOURCES-WITH-VARIOUS-MULTISAMPLE-COUNTS"></span>多重採樣次數超過 1 之子資源


下表顯示了 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) 和多重採樣次數超過 1 之 [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526) 子資源拼接的方式。

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


[串流資源區域的拼接方式](how-a-streaming-resource-s-area-is-tiled.md)

 

 



