---
Description: 以直覺且獨特的使用者互動體驗來建立 Windows 應用程式，其已針對觸控板優化，但在輸入裝置之間具有一致的功能。
title: 觸控板互動
ms.assetid: CEDEA30A-FE94-4553-A7FB-6C1FA44F06AB
label: Touchpad interactions
template: detail.hbs
keywords: 觸控板、PTP、觸控、指標、輸入、使用者互動
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ffc3ce96c7e8c2ad4a34aecd1ca85ff644bdef97
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234479"
---
# <a name="touchpad-design-guidelines"></a>觸控板設計指導方針


將您的應用程式設計成使用者可透過觸控板與其互動的應用程式。 觸控板結合了間接多點觸控輸入與指標裝置 (如滑鼠) 精確輸入。 這項結合讓觸控板既適用於觸控最佳化 UI，也適用於較小的生產力應用程式目標。

 

![觸控板](images/input-patterns/input-touchpad.jpg)


觸控板互動需要具備三個要件：

-   標準觸控板或 Windows 精確式觸控板。

    精確度 touchpads 已針對 Windows 應用程式裝置進行優化。 它們讓系統本質上就能處理特定層面的觸控板體驗 (例如手指追蹤和手掌偵測)，以便在不同的裝置上取得更一致的體驗。

-   觸控板上一或多根手指的直接接觸點。
-   觸控接觸點的移動 (或者沒有，根據時間閾值)。

觸控板感應器提供的輸入資料可以：

-   解譯為直接操作一或多個 UI 元素的實際手勢 (例如，移動瀏覽、旋轉、調整大小或移動)。 相反地，透過元素的屬性視窗或其他對話方塊與元素互動，則視為間接操作。
-   辨識為替代的輸入法，例如滑鼠或手寫筆。
-   用來補充或修改其他輸入法的層面，例如弄髒手寫筆繪製的筆跡筆觸。

觸控板結合了間接多點觸控輸入與指標裝置 (如滑鼠) 精確輸入。 這樣的結合讓觸控板適用於觸控最佳化 UI 以及一般較小目標的生產力應用程式和桌面環境兩者。 針對觸控輸入優化您的 Windows 應用程式設計，並根據預設取得觸控板支援。

因為觸控板支援整合的互動體驗，所以我們建議使用 [**PointerEntered**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered) 事件，在內建的觸控輸入支援以外，還提供滑鼠樣式的 UI 命令。 例如，使用 [上一頁] 和 [下一頁] 按鈕，讓使用者翻頁內容以及移動瀏覽內容。

本主題中討論的手勢和指導方針可以協助您確保您的應用程式使用最少的程式碼就能順暢地支援觸控板輸入。

## <a name="the-touchpad-language"></a>觸控板語言


一組可用於整個系統的簡單觸控板互動。 針對觸控及滑鼠輸入最佳化您的應用程式，而這個語言讓使用者可以立即熟悉您的應用程式，提高他們的自信，讓您的應用程式更易於學習及使用。

比起在標準觸控板上所做的動作，使用者能夠設定更多的精確式觸控板手勢與互動行為。 下列兩個影像顯示分別來自標準觸控板和精確式觸控板之 [設定] &gt; [裝置] &gt; [滑鼠和觸控板] 的不同觸控板設定頁面。

![標準觸控板設定](images/mouse-touchpad-settings-standard.png)

<sup>標準的 \\ 觸控板 \\ 設定</sup>

![Windows 精確式觸控板設定](images/mouse-touchpad-settings-ptp.png)

<sup>Windows \\ 精確的 \\ 觸控板 \\ 設定</sup>

以下是一些已針對觸控板進行最佳化且適用於執行一般工作的手勢範例。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">詞彙</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>三指點選</p></td>
<td align="left"><p>使用者喜好設定，可使用 <strong>Cortana</strong> 搜尋或顯示 [重要訊息中心]<strong></strong>。</p></td>
</tr>
<tr class="even">
<td align="left"><p>三指滑動</p></td>
<td align="left"><p>使用者喜好設定，可開啟虛擬桌面的 [工作檢視]、顯示桌面，或在開啟的應用程式之間切換。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>單指點選以進行主要動作</p></td>
<td align="left"><p>使用單指點選元素，叫用它的主要動作 (例如啟動應用程式或執行命令)。</p></td>
</tr>
<tr class="even">
<td align="left"><p>兩指點選以按一下滑鼠右鍵</p></td>
<td align="left"><p>使用兩指同時點選元素來選取它，並顯示內容命令。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>兩指滑動以進行移動瀏覽</p></td>
<td align="left"><p>滑動主要用於移動瀏覽互動，但也可以用來移動、繪圖以及書寫。</p></td>
</tr>
<tr class="even">
<td align="left"><p>捏合和伸展以進行縮放</p></td>
<td align="left"><p>捏合和伸展手勢通常用於調整大小和語意式縮放。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>單指按住並滑動以重新排列</p></td>
<td align="left"><p>拖曳元素。</p></td>
</tr>
<tr class="even">
<td align="left"><p>單指按住並滑動以選取文字</p></td>
<td align="left"><p>在可選取的文字內按住並滑動即可選取文字。 點兩下即可選取某個字。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>按左鍵/右鍵區域</p></td>
<td align="left"><p>模擬滑鼠裝置的左鍵和右鍵功能。</p></td>
</tr>
</tbody>
</table>

 

## <a name="hardware"></a>硬體


查詢滑鼠裝置的功能 ([**MouseCapabilities**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input.MouseCapabilities))，了解觸控板硬體可以直接存取應用程式 UI 的什麼層面。 建議提供同時適用於觸控與滑鼠輸入的 UI。

如需有關查詢裝置功能的詳細資訊，請參閱[識別輸入裝置](identify-input-devices.md)。

## <a name="visual-feedback"></a>視覺化回饋


-   偵測到觸控板游標時 (透過移動或暫留事件)，顯示滑鼠特定 UI，指示元素公開的功能。 如果觸控板游標有一段時間沒有移動，或者使用者起始觸控互動，讓觸控板 UI 逐漸淡出。 這可以讓 UI 保持整齊、不凌亂。
-   請勿為暫留回饋使用游標，元素提供的回饋已經足夠 (請參閱下方的＜游標＞一節)。
-   如果元素不支援互動 (例如靜態文字)，請勿顯示視覺化回饋。
-   請勿搭配觸控板互動使用焦點矩形。 請保留這些給鍵盤互動。
-   如果所有元素均代表相同的輸入目標，請同時顯示視覺化回饋。

如需有關視覺化回饋的詳細一般指導方針，請參閱[視覺化回饋的指導方針](https://docs.microsoft.com/windows/uwp/input-and-devices/guidelines-for-visualfeedback)。

## <a name="cursors"></a>資料指標


我們提供了一組可用於觸控板指標的標準游標。 它們可用來指示元素的主要動作。

每一個標準游標都有與其關聯之相對應的預設影像。 使用者或應用程式可以隨時取代與任何標準游標相關聯的預設影像。 UWP app 透過 [**PointerCursor**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointercursor) 函式指定游標影像。

如果您需要自訂滑鼠游標：

-   一律使用箭頭游標 (![箭頭游標](images/cursor-arrow.png)) 於可點選的元素。 請勿使用指向手型游標 (![指向手型游標](images/cursor-pointinghand.png)) 於連結或其他互動式元素。 請改為使用暫留效果 (描述如前)。
-   使用文字游標 (![文字游標](images/cursor-text.png)) 於可選取的文字。
-   使用移動游標 (![移動游標](images/cursor-move.png)) 於主要動作為移動時 (例如拖曳或裁剪時)。 對於主要動作為瀏覽的元素 (例如 [開始] 畫面磚)，請勿使用移動游標。
-   請使用水平、垂直及對角線調整游標 (![調整垂直大小游標](images/cursor-vertical.png), ![調整水平大小游標](images/cursor-horizontal.png), ![對角線調整游標 (左下右上)](images/cursor-diagonal2.png), ![對角線調整游標 (左上右下)](images/cursor-diagonal1.png)) 於物件可調整時。
-   使用握拳游標 (![握拳游標 (打開)](images/cursor-pan1.png), ![握拳游標 (握緊)](images/cursor-pan2.png)) 於固定畫布 (例如地圖) 內移動瀏覽內容時。

## <a name="related-articles"></a>相關文章

- [處理指標輸入](handle-pointer-input.md)
- [識別輸入裝置](identify-input-devices.md)

### <a name="samples"></a>範例

- [基本輸入範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [低延遲輸入範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [使用者互動模式範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [焦點視覺效果範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals) \(英文\)

### <a name="archive-samples"></a>封存範例

- [輸入：裝置功能範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [輸入：XAML 使用者輸入事件範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [XAML 捲軸、移動流覽和縮放範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [輸入：使用 GestureRecognizer 處理手勢與操作](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
