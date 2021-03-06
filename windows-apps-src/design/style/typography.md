---
description: 了解如何在您的應用程式中使用印刷樣式，以協助使用者輕鬆了解內容。
title: Windows 應用程式中的印刷樣式
ms.date: 04/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 3a5b6df7a5d8333e0f4834c256a38fc912f8f51e
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970623"
---
# <a name="typography-in-windows-apps"></a>Windows 應用程式中的印刷樣式

![主角圖像](images/header-typography.svg)

作為語言的視覺表示，印刷格式的主要工作是溝通資訊。 其樣式絕對不能阻礙這項目標。 在這篇文章中，我們將會討論如何在您的 Windows 應用程式中設定印刷樣式，以協助使用者輕鬆且有效地了解內容。

## <a name="font"></a>字型

您應該在整個應用程式 UI 中使用單一字型，我們建議您維持 Windows 應用程式的預設字型 **Segoe UI**。 它設計成可維持不同大小和像素密度的最佳可讀性，並能提供輔助系統內容的乾淨、明亮、開放審美觀。

![Segoe UI 字型的範例文字](images/type/segoe-sample.svg)

若要顯示非英文語言或為應用程式選取不同字型，請參閱[語言](#languages)和[字型](#fonts)以了解我們對 Windows 應用程式的建議使用字型。

:::row:::
    :::column:::
![可行事項](images/do.svg)為 UI 選擇一種字型。
    :::column-end:::
    :::column:::
![禁止事項](images/dont.svg)請勿混合使用多個字型。
    :::column-end:::
:::row-end:::

## <a name="size-and-scaling"></a>大小和縮放比例

UWP 應用程式的字型大小會在所有裝置上自動縮放。 此縮放演算法可確保使用者在 10 英呎遠的 Surface Hub 上看到的 24 px 字型，就和在只有幾英吋遠的 5 吋手機上看到的 24 px 字型一樣清晰。

![不同裝置的檢視距離](images/type/scaling-chart.svg)

因為縮放比例系統的運作方式，您是以有效像素進行設計，而不是實際實體像素，而且您不應根據不同的螢幕大小或解析度來變更字型大小。

:::row:::
    :::column:::
![可行事項](images/do.svg)遵循 Windows [字體坡形](#type-ramp)大小調整。
    :::column-end:::
    :::column:::
![禁止事項](images/dont.svg)使用小於 12 像素的字型大小。
    :::column-end:::
:::row-end:::

## <a name="hierarchy"></a>階層

:::row:::
    :::column:::
使用者在掃描頁面時依賴視覺階層：標頭摘要顯示內容，本文提供更多詳細資料。 若要在應用程式中建立清楚的視覺階層，請遵循 Windows 字體坡形。
    :::column-end:::
    :::column:::
![文字區塊樣式](images/type/type-hierarchy.svg)
    :::column-end:::
:::row-end:::

### <a name="type-ramp"></a>字體坡形

Windows 字體坡形可在頁面的類型之間建立重要關係，協助使用者輕鬆閱讀內容。 所有大小都是有效像素，會針對在所有裝置上執行的 UWP 應用程式而最佳化。

![字體坡形](images/type/type-ramp.png)

### <a name="using-the-type-ramp"></a>使用字體坡形

:::row:::
    :::column:::
您可以存取 XAML [靜態資源](../controls-and-patterns/xaml-theme-resources.md#the-xaml-type-ramp)形式的字體坡形層級。 此樣式遵循 `*TextBlockStyle` 命名慣例。
    :::column-end:::
    :::column:::
![文字區塊樣式](images/type/text-block-type-ramp.svg)
    :::column-end:::
:::row-end:::

```XAML
<TextBlock Text="Header" Style="{StaticResource HeaderTextBlockStyle}"/>
<TextBlock Text="SubHeader" Style="{StaticResource SubheaderTextBlockStyle}"/>
<TextBlock Text="Title" Style="{StaticResource TitleTextBlockStyle}"/>
<TextBlock Text="SubTitle" Style="{StaticResource SubtitleTextBlockStyle}"/>
<TextBlock Text="Base" Style="{StaticResource BaseTextBlockStyle}"/>
<TextBlock Text="Body" Style="{StaticResource BodyTextBlockStyle}"/>
<TextBlock Text="Caption" Style="{StaticResource CaptionTextBlockStyle}"/>
```

:::row:::
    :::column:::
![可行事項](images/do.svg)讓大部分文字使用「Body」。

標題在空間有限時使用「基準」。
    :::column-end:::
    :::column:::
![禁止事項](images/dont.svg)對於主要動作或任何長字串，使用「Caption」。

如果文字需要自動換行，請使用「標題」或「副標題」。
    :::column-end:::
:::row-end:::

## <a name="alignment"></a>對應項目

預設 [TextAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.textalignment) 是 Left，而在大部分情況下，靠左和不齊右方法可提供一致的內容錨定與統一的配置。 對於 RTL 語言，請參閱[調整配置和字型以支援全球化](../globalizing/adjust-layout-and-fonts--and-support-rtl.md)。

![顯示靠左文字。](images/type/alignment.svg)

```xaml
<TextBlock TextAlignment="Left">
```

## <a name="character-count"></a>字元計數

:::row:::
    :::column:::
![可行事項](images/do.svg)每行維持 50–60 個字母以利閱讀。
    :::column-end:::
    :::column:::
![禁止事項](images/dont.svg)每行少於 20 個字元或超過 60 個字元會阻礙閱讀。
    :::column-end:::
:::row-end:::

## <a name="clipping-and-ellipses"></a>裁剪和省略符號

當文字量超過可用空間時，建議裁剪文字，這是大部分 [UWP 文字控制項](../controls-and-patterns/text-controls.md)的預設行為。

![顯示使用文字裁剪的裝置框架](images/type/clipping.svg)

```xaml
<TextBlock TextWrapping="WrapWholeWords" TextTrimming="Clip"/>
```

:::row:::
    :::column:::
![可行事項](images/do.svg)裁剪文字，如果啟用多行則換行。
    :::column-end:::
    :::column:::
![禁止事項](images/dont.svg)使用省略符號以避免視覺干擾。
    :::column-end:::
:::row-end:::

**注意**：如果容器未明確定義 (例如，沒有區別的背景色彩)，或是有連結可顯示更多文字，則請使用省略符號。

## <a name="languages"></a>語言 

Segoe UI 是我們的英文、歐洲語言、希臘文、希伯來文、亞美尼亞文、喬治亞文和阿拉伯文字型。 對於其他語言，請參閱下列建議。

### <a name="globalizinglocalizing-fonts"></a>將字型全球化/當地語系化

您可以使用 [LanguageFont 字型對應 API](https://docs.microsoft.com/uwp/api/Windows.Globalization.Fonts.LanguageFont)，以程式設計方式存取特定語言的建議字型系列、大小、粗細及樣式。 LanguageFont 物件會針對各種內容類別 (包括 UI 標頭、通知、內文，以及使用者可以編輯的文件內文字型)，提供正確字型資訊的存取權。 如需詳細資訊，請參閱[調整配置和字型以支援全球化](../globalizing/adjust-layout-and-fonts--and-support-rtl.md)。

### <a name="fonts-for-non-latin-languages"></a>非拉丁語言的字型

<table>
<thead>
<tr class="header">
<th align="left">字型家族</th>
<th align="left">Styles</th>
<th align="left">附註</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="font-family: Embrima;">Ebrima</td>
<td align="left">標準、粗體</td>
<td align="left">非洲字集 (衣索比亞文、西非書面文、歐斯馬雅文、提弗納文、范文) 的使用者介面字型。</td>
</tr>
<tr class="even">
<td style="font-family: Gadugi;">Gadugi</td>
<td align="left">標準、粗體</td>
<td align="left">北美洲字集 (加拿大語音節、卻洛奇文) 的使用者介面字型。</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Leelawadee UI;">Leelawadee UI</td>
<td align="left">標準、半細體、粗體</td>
<td align="left">東南亞字集 (布吉斯文、寮文、高棉文、泰文) 的使用者介面字型。</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Malgun Gothic;">Malgun Gothic</td>
<td align="left">標準</td>
<td align="left">韓文的使用者介面字型。</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft JhengHei UI;">Microsoft JhengHei UI</td>
<td align="left">標準、粗體、細體</td>
<td align="left">繁體中文的使用者介面字型。</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Microsoft YaHei UI;">Microsoft YaHei UI</td>
<td align="left">標準、粗體、細體</td>
<td align="left">簡體中文的使用者介面字型。</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Myanmar Text;">Myanmar Text</td>
<td align="left">標準</td>
<td align="left">緬甸文字集的後援字型。</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Nirmala UI;">Nirmala UI</td>
<td align="left">標準、半細體、粗體</td>
<td align="left">南亞字集 (孟加拉文、梵文字母、古吉拉特文、果魯穆奇文、坎那達文、馬來亞拉姆文、歐迪亞文、桑塔爾文、僧伽羅文、索拉僧平文、坦米爾文、特拉古文) 的使用者介面字型</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: SimSun;">SimSun</td>
<td align="left">標準</td>
<td align="left">傳統中文 UI 字型。 </td>
</tr>
<tr class="even">
<td align="left" style="font-family: Yu Gothic UI;">Yu Gothic UI</td>
<td align="left">細體、半細體、標準、半粗體、粗體</td>
<td align="left">日文的使用者介面字型。</td>
</tr>
</tbody>
</table>

## <a name="fonts"></a>字型

### <a name="sans-serif-fonts"></a>Sans-serif 字型

Sans-serif 字型是標題和 UI 元素的絕佳選擇。 

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">字型家族</th>
<th align="left">Styles</th>
<th align="left">附註</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left" style="font-family: Arial;">Arial</td>
<td align="left">標準、斜體、粗體、粗斜體、黑體</td>
<td align="left">支援歐洲與中東字集 (拉丁文、希臘文、斯拉夫文、阿拉伯文、亞美尼亞文及希伯來文) 黑色粗細僅支援歐洲字集。</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Calibri;">Calibri</td>
<td align="left">標準、斜體、粗體、粗斜體、細體、細斜體</td>
<td align="left">支援歐洲與中東字集 (拉丁文、希臘文、斯拉夫文、阿拉伯文及希伯來文)。 阿拉伯文只提供直書形式。</td>
</tr>
<td style="font-family: Consolas;">Consolas</td>
<td>標準、斜體、粗體、粗斜體</td>
<td>支援歐洲字集的固定寬度字型 (拉丁文、希臘文及斯拉夫文)。</td>
</tr>

<tr>
<td style="font-family: Segoe UI;">Segoe UI</td>
<td>標準、斜體、細斜體、黑斜體、粗體、粗斜體、細體、半細體、半粗體、黑體</td>
<td>歐洲與中東字集 (阿拉伯文、亞美尼亞文、斯拉夫文、喬治亞文、希臘文、希伯來文、拉丁文) 以及栗粟文字集的使用者介面字型。</td>
</tr>

<tr class="even">
<td style="font-family: Selawik;">Selawik</td>
<td align="left">標準、半細體、細體、粗體、半粗體</td>
<td align="left">在格律上與 Segoe UI 相容的開放原始碼字型，用於其他不想要與 Segoe UI 搭配之平台上的 App。 <a href="https://github.com/Microsoft/Selawik">從 GitHub 取得 Selawik。</a></td>
</tr>

</tbody>
</table>

### <a name="serif-fonts"></a>Serif 字型

Serif 字型適合呈現大量的文字。 

<table>
<thead>
<tr class="header">
<th align="left">字型家族</th>
<th align="left">Styles</th>
<th align="left">附註</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="font-family: Cambria;">Cambria</td>
<td align="left">標準</td>
<td align="left">支援歐洲字集的 Serif 字型 (拉丁文、希臘文、斯拉夫文)。</td>
</tr>
<tr class="even">
<td style="font-family: Courier New;">Courier New</td>
<td align="left">標準、斜體、粗體、粗斜體</td>
<td align="left">Serif 固定寬度字型支援歐洲與中東字集 (拉丁文、希臘文、斯拉夫文、阿拉伯文、亞美尼亞文及希伯來文)。</td>
</tr>
<tr class="odd">
<td style="font-family: Georgia;">喬治亞</td>
<td align="left">標準、斜體、粗體、粗斜體</td>
<td align="left">支援歐洲字集 (拉丁文、希臘文及斯拉夫文)。</td>
</tr>

<tr class="even">
<td style="font-family: Times New Roman;">Times New Roman</td>
<td align="left">標準、斜體、粗體、粗斜體</td>
<td align="left">支援歐洲字集的傳統字型 (拉丁文、希臘文、斯拉夫文、阿拉伯文、亞美尼亞文、希伯來文)。</td>
</tr>

</tbody>
</table>

### <a name="symbols-and-icons"></a>符號與圖示

<table>
<thead>
<tr class="header">
<th align="left">字型家族</th>
<th align="left">Styles</th>
<th align="left">附註</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Segoe MDL2 Assets</td>
<td align="left">標準</td>
<td align="left">App 圖示的使用者介面字型。 如需詳細資訊，請參閱 <a href="segoe-ui-symbol-font.md">Segoe MDL2 Assets 文章</a>。</td>
</tr>
<tr class="even">
<td align="left">Segoe UI Emoji</td>
<td align="left">標準</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Segoe UI Symbol</td>
<td align="left">標準</td>
<td align="left">符號的後援字型</td>
</tr>
</tbody>
</table>

## <a name="related-articles"></a>相關文章

* [文字控制項](../controls-and-patterns/text-controls.md)
* [XAML 佈景主題資源](../controls-and-patterns/xaml-theme-resources.md#the-xaml-type-ramp)
* [XAML 樣式](../controls-and-patterns/xaml-styles.md)
* [Microsoft Typography](https://docs.microsoft.com/typography/)
