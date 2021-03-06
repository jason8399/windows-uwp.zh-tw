---
title: 附錄
description: 使用浮點規則、資料類型轉換、點陣化規則和材質區塊壓縮的技術詳細資料，來查看主題連結的表格。
ms.assetid: 461CCE6F-BAF0-4965-90A5-FD36B511F72C
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 41762bf24202d155ed7af288ff43ea898b43e46d
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88942768"
---
# <a name="appendices"></a>附錄

這幾節提供深入的技術詳細資料。

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
<td align="left"><p><a href="floating-point-rules.md">浮點規則</a></p></td>
<td align="left"><p>Direct3D 支援數個浮點表示法。 所有浮點數計算運作都在 IEEE 754 32 位元單精確度浮點數規則的定義子集下運作。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="data-type-conversion.md">資料型別轉換</a></p></td>
<td align="left"><p>下列章節說明 Direct3D 如何處理資料類型之間的轉換。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="rasterization-rules.md">點陣化規則</a></p></td>
<td align="left"><p>點陣化規則定義向量資料對應至點陣資料的方式。 點陣資料會貼齊至整數位置，該整數位置接著會進行揀選和裁剪 (以繪製像素數目下限)，而每個像素屬性會在傳遞至像素著色器之前進行插入 (從每個頂點屬性)。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-block-compression.md">紋理區塊壓縮</a></p></td>
<td align="left"><p>紋理的區塊壓縮 (BC) 支援在 Direct3D 11 中已進行擴充，內含 BC6H 和 BC7 演算法。 BC6H 支援高動態範圍色彩來源資料，而 BC7 使用較少的標準 RGB 來源資料成品，提供高於平均品質的壓縮。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[Direct3D 圖形學習指南](index.md)

 

 




