---
title: 回應式設計的螢幕大小與中斷點
description: 與其針對 Windows 10 生態系統的許多裝置將 UI 最佳化，我們建議設計幾個重要的寬度類別 (稱為「中斷點」)。
ms.date: 08/30/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 2e385f6b9977eead6aed52215080588e4f9d8c27
ms.sourcegitcommit: cc645386b996f6e59f1ee27583dcd4310f8fb2a6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/01/2020
ms.locfileid: "84262769"
---
#  <a name="screen-sizes-and-breakpoints"></a>螢幕大小與中斷點

Windows 應用程式可在執行 Windows，包括手機、平板電腦、桌機、電視，以及更多的任何裝置上執行。 Windows 10 生態系統跨越大量裝置目標與螢幕大小，與其針對每個裝置最佳化您的 UI，我們建議設計幾個重要的寬度類別 (也稱為「中斷點」) ： 
- 小型 (小於 640 像素)
- 中型 (641 - 1007 像素)
- 大型 (1008 和更高像素)

> [!TIP]
> 當針對特定中斷點進行設計時，請針對您應用程式，而不是螢幕大小，可以使用的螢幕空間量 (應用程式的視窗) 進行設計。 當應用程式以全螢幕執行時，應用程式視窗會與螢幕大小相同，但當應用程式不是全螢幕時，視窗則小於螢幕。

## <a name="breakpoints"></a>中斷點
下表描述不同大小的類別和中斷點。

![回應式設計中斷點](images/breakpoints/size-classes.svg)

<table>
<thead>
<tr class="header">
<th align="left">大小類別</th>
<th align="left">中斷點</th>
<th align="left">一般螢幕大小 (對角線)</th>
<th align="left">裝置</th>
<th align="left">視窗大小</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td style="vertical-align:top;">小型</td>
<td style="vertical-align:top;">640px 或更少</td>
<td style="vertical-align:top;">4&quot; 到 6&quot;；20&quot; 到 65&quot;</td>
<td style="vertical-align:top;">手機、電視</td>
<td style="vertical-align:top;">320x569、360x640、480x854</td>
</tr>
<tr class="odd">
<td style="vertical-align:top;">中</td>
<td style="vertical-align:top;">641px 到 1007px</td>
<td style="vertical-align:top;">7&quot; 到 12&quot;</td>
<td style="vertical-align:top;">平板手機、平板電腦</td>
<td style="vertical-align:top;">960x540</td>
</tr>
<tr class="even">
<td style="vertical-align:top;">大型</td>
<td style="vertical-align:top;">1008px 或更大像素</td>
<td style="vertical-align:top;">13&quot; 及更大</td>
<td style="vertical-align:top;">電腦、膝上型電腦、Surface Hub</td>
<td style="vertical-align:top;">1024x640、1366x768、1920x1080</td>
</tr>
</tbody>
</table>

## <a name="why-are-tvs-considered-small"></a>為何電視被認定為「小型」？ 

大部分電視實體都相當大 (40 到 65 英吋是常見) 和高解析度 (HD 或 4k)，設計從 10 英呎遠處觀看的 1080P 電視，和設計坐在桌前一英呎遠觀看 1080p 監視器，是大不同的。 當考慮距離，電視的 1080 像素更像是更近的 540 像素監視器。

UWP 的有效像素系統會自動考量您觀看的距離。 當指定控制項的大小或中斷點範圍時，您實際上是在使用「有效」像素。 例如，您為 1080 像素及更高像素建立回應式程式碼時，1080 監視器會使用該程式碼，但 1080p 電視不會，因為 1080p 電視雖有 1080 實體像素，但只有 540 個有效像素。 在這部分，設計電視與設計手機很雷同。

## <a name="effective-pixels-and-scale-factor"></a>有效像素與縮放比例

UWP 應用程式會自動調整您的 UI 以確保應用程式在所有 Windows 10 裝置上可辨識。 Windows 會根據 DPI (每英吋點數) 以及裝置的檢視距離，自動縮放每個螢幕的比例。 使用者可以移至 [設定]   >  [顯示]   >  [縮放與版面配置]  設定頁面，覆寫預設值。 


## <a name="general-recommendations"></a>一般建議

### <a name="small"></a>小型
- 設定左右視窗邊界為 12px，以在應用程式視窗左右邊緣建立視覺區隔。
- 將[應用程式列](../controls-and-patterns/app-bars.md)固定在視窗底部以改善存取性。
- 一次使用 1 個欄/區域。
- 使用圖示來表示搜尋 (不顯示搜尋方塊)。
- 將[瀏覽窗格](../controls-and-patterns/navigationview.md)以重疊模式放置，以節省螢幕空間。
- 如果您使用[主要詳細資料模式](../controls-and-patterns/master-details.md)，請使用堆疊展示模式，以節省螢幕空間。

### <a name="medium"></a>中
- 設定左右視窗邊界為 24px，以在應用程式視窗左右邊緣建立視覺區隔。
- 將[應用程式列](../controls-and-patterns/app-bars.md)之類的命令元素放置在應用程式視窗的頂端。
- 最多使用 2 個欄/區域。
- 顯示搜尋方塊。
- 將[瀏覽窗格](../controls-and-patterns/navigationview.md)以窄條模式放置，以便讓圖示帶狀線永遠顯示。
- 請考慮進一步自訂[電視體驗](https://docs.microsoft.com/windows/uwp/design/devices/designing-for-tv?redirectedfrom=MSDN)。

### <a name="large"></a>大型
- 設定左右視窗邊界為 24px，以在應用程式視窗左右邊緣建立視覺區隔。
- 將[應用程式列](../controls-and-patterns/app-bars.md)之類的命令元素放置在應用程式視窗的頂端。
- 最多使用 3 個欄/區域。
- 顯示搜尋方塊。
- 將[瀏覽窗格](../controls-and-patterns/navigationview.md)以停駐模式放置，以便讓它永遠顯示。
