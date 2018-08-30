---
title: 串流資源建立參數
description: 可以建立為串流資源的 Direct3D 資源類型上有一些限制。
ms.assetid: 6FC5AD93-6F47-479E-947C-895C99B427BC
keywords:
- 串流資源建立參數
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: f2a17f69ae0353bb7682a1dbfa48d5909f48d4aa
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
ms.locfileid: "1652987"
---
# <a name="streaming-resource-creation-parameters"></a>串流資源建立參數


可以建立為串流資源的 Direct3D 資源類型上有一些限制。

<span id="Supported-Resource-Type"></span><span id="supported-resource-type"></span><span id="SUPPORTED-RESOURCE-TYPE"></span>**支援的資源類型**  
Texture2D\[Array\] (包括 TextureCube\[Array\]，這是 Texture2D\[Array\] 的變量) 或緩衝區。

**不支援：**Texture1D\[Array\]。

<span id="Supported-Resource-Usage"></span><span id="supported-resource-usage"></span><span id="SUPPORTED-RESOURCE-USAGE"></span>**支援的資源使用方式**  
預設使用方式。

**不支援：**動態、預備環境，或不可變。

<span id="Supported-Resource-Misc-Flags"></span><span id="supported-resource-misc-flags"></span><span id="SUPPORTED-RESOURCE-MISC-FLAGS"></span>**支援的資源其他旗標**  
Tiled；也就是 streaming（依定義）、texture cube、draw indirect arguments、buffer allow raw views、structured buffer、resource clamp 或 generate mips。

**不支援：**shared、shared keyed mutex、GDI compatible、shared NT handle、restricted content、restrict shared resource、restrict shared resource driver、guarded 或 tile pool。

<span id="Supported-Bind-Flags"></span><span id="supported-bind-flags"></span><span id="SUPPORTED-BIND-FLAGS"></span>**支援的繫結旗標**  
繫結為 shader resource、render target、depth stencil 或 unordered access。

**不支援：**繫結為 constant buffer、vertex buffer (支援繫結區塊式緩衝區做為 SRV/UAV/RTV)、index buffer、stream output、decoder 或 video encoder。

<span id="Supported-Formats"></span><span id="supported-formats"></span><span id="SUPPORTED-FORMATS"></span>**支援的格式**  
特定設定的所有格式 (不管它是區塊式)，但有一些例外。

<span id="Supported-Sample-Description--Multisample-count--quality-"></span><span id="supported-sample-description--multisample-count--quality-"></span><span id="SUPPORTED-SAMPLE-DESCRIPTION--MULTISAMPLE-COUNT--QUALITY-"></span>**支援的樣本描述（多重樣本計數、品質）**  
特定設定的所有描述都會受支援 (不管它是區塊式)，但有一些例外。

<span id="Supported-Width-Height-MipLevels-ArraySize"></span><span id="supported-width-height-miplevels-arraysize"></span><span id="SUPPORTED-WIDTH-HEIGHT-MIPLEVELS-ARRAYSIZE"></span>**支援的寬度/高度/MipLevels/ArraySize**  
Direct3D 支援完整範圍。 串流資源沒有加諸到非串流資源的總記憶體大小限制。 串流資源只受到整體虛擬位址空間限制。 請參閱[串流資源可使用的位址空間](address-space-available-for-streaming-resources.md)。

磚集區記憶體的初始內容未定義。

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
<td align="left"><p><a href="address-space-available-for-streaming-resources.md">串流資源可使用的位址空間</a></p></td>
<td align="left"><p>本章節指定虛擬位址空間，可供串流資源使用。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[建立串流資源](creating-streaming-resources.md)

 

 



