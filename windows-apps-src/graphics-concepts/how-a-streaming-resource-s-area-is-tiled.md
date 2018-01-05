---
title: "串流資源區域的拼接方式"
description: "建立串流資源時，維度、格式項目大小及 Mipmap 數目和/或陣列配量 (如適用) 決定了備份整個表面區域所需的磚數目。"
ms.assetid: 3485FD8D-2A06-4B0A-8810-ECF37736F94B
keywords:
- "串流資源區域的拼接方式"
- "資源區域, 拼接"
- "大小表格, 資源, 拼接"
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 257522f8d00596aa6bb60a87277d4e1a5ccdcf89
ms.sourcegitcommit: c80b9e6589a1ee29c5032a0b942e6a024c224ea7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/22/2017
---
# <a name="how-a-streaming-resources-area-is-tiled"></a>串流資源區域的拼接方式


建立串流資源時，維度、格式項目大小及 Mipmap 數目和/或陣列配量 (如適用) 決定了備份整個表面區域所需的磚數目。 磚內像素/位元組的配置是由實作決定。 可放入磚的像素數目取決於格式項目大小，而且無論您是否使用標準 swizzle 都一樣。

特定表面大小和格式項目寬度所使用的磚數目會妥善定義，並且可根據下列區段中的表格預測。 對於包含 Mipmap 或表面維度無法填滿磚之案例的資源，會有些限制；請參閱[Mipmap 封裝](mipmap-packing.md)。

不同的串流資源可能指向相同的記憶體，但格式不同，只要應用程式不倚賴使用一種格式寫入記憶體，並使用另一種格式讀取的結果。 但應用程式可能會依賴使用一種格式寫入記憶體、另一種格式讀取的結果，如果格式屬於相同格式系列 (也就是說，它們具有相同的無類型上層格式)。 例如，DXGI\_FORMAT\_R8G8B8A8\_UNORM 和 DXGI\_FORMAT\_R8G8B8A8\_UINT 彼此相容，但與 DXGI\_FORMAT\_R16G16\_UNORM 不相容。

有一個例外是已妥善定義洩出資料從一種格式更名為另一種：如果磚的所有位元都只包含 0，該磚就可以搭配將這些記憶體內容解譯為 0 的任何格式使用 (無論記憶體配置為何)。 因此，磚可能會清除為 0x00 且其格式為 DXGI\_FORMAT\_R8\_UNORM，然後搭配 DXGI\_FORMAT\_R32G32\_FLOAT 這類格式使用，而且看起來內容仍為 (0.0f,0.0f)。

磚中資料的配置不是取決於磚在資源中的整體對應位置。 因此，例如磚可以一次在表面的不同位置重複使用，而且所有位置的行為一致。

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>本節內容


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
<td align="left"><p>[Texture2D 和 Texture2DArray 子資源拼接](texture2d-and-texture2darray-subresource-tiling.md)</p></td>
<td align="left"><p>這些表格顯示 [<strong>Texture2D</strong>](https://msdn.microsoft.com/library/windows/desktop/ff471525) 和 [<strong>Texture2DArray</strong>](https://msdn.microsoft.com/library/windows/desktop/ff471526) 子資源如何拼接。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Texture3D 子資源拼接](texture3d-subresource-tiling.md)</p></td>
<td align="left"><p>下表顯示 [<strong>Texture3D</strong>](https://msdn.microsoft.com/library/windows/desktop/ff471562) 子資源如何拼接。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[緩衝區拼接](buffer-tiling.md)</p></td>
<td align="left"><p>[緩衝區](introduction-to-buffers.md)資源分為 64KB 的磚，若大小不是 64 KB 的倍數，則最後一個磚會有某些空白空間。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Mipmap 封裝](mipmap-packing.md)</p></td>
<td align="left"><p>有些 mips 數量 (每一陣列配量) 可以封裝為部分磚數，根據串流資源的維度、格式、Mipmaps 數，以及陣列配量。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[建立串流資源](creating-streaming-resources.md)

 

 



