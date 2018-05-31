---
author: mijacobs
Description: App icon assets, which appear in a variety of forms throughout the Windows 10 operating system, are the calling cards for your Universal Windows Platform (UWP) app.
title: 磚和圖示資產
ms.assetid: D6CE21E5-2CFA-404F-8679-36AA522206C7
label: Tile and icon assets
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: cc614f7803e7546add8ecccb7cc20dc0250d0d22
ms.sourcegitcommit: eead3c00b27d9f66f79ec08c81a97e91dc1fdb3c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/18/2018
ms.locfileid: "1522787"
---
# <a name="guidelines-for-tile-and-icon-assets"></a>磚和圖示資產的指導方針

 


以各種形式出現在整個 Windows 10 作業系統的 App 圖示資產，好比通用 Windows 平台 (UWP) App 的名片。 這些指導方針詳細說明應用程式圖示資產出現在系統的何處，並提供如何建立最優美圖示的深入設計祕訣。

![Windows 10 開始畫面和磚](images/assetguidance01.jpg)

## <a name="adaptive-scaling"></a>彈性縮放比例


首先，簡短概述彈性縮放比例，以便更清楚了解縮放比例如何使用資產。 Windows 10 引進現有縮放比例模型的進化。 除了縮放比例向量內容之外，還有統一的縮放比例集合，跨各種不同的螢幕大小和顯示器解析度，針對 UI 元素提供一致的大小。 縮放比例也相容於 iOS 與 Android 等其他作業系統的縮放比例，讓您能夠更輕鬆地在這些平台之間共用資產。

Microsoft Store 會根據裝置的 DPI 部分來選取要下載的資產。 只會下載最符合裝置的資產。

## <a name="tile-elements"></a>磚元素


開始畫面磚的基本元件是由一個背板、一個圖示、一個商標列、邊界及一個 App 標題所構成：

![磚元素的分析](images/assetguidance02.png)

磚底部的商標列是顯示應用程式名稱、徽章及計數器 (如有使用) 的位置：

![磚中的商標列](images/assetguidance03.png)

商標列的高度是以顯示裝置上的縮放比例為基礎：

| 縮放比例 | 像素 |
|--------------|--------|
| 100%         | 32     |
| 125%         | 40     |
| 150%         | 48     |
| 200%         | 64     |
| 400%         | 128    |

 

系統會設定磚的邊界且無法加以修改。 大部分的內容都會顯示在邊界內部，如下列範例所示：

![磚邊界](images/assetguidance04.png)

邊界高度是以顯示裝置上的縮放比例為基礎：

| 縮放比例 | 像素 |
|--------------|--------|
| 100%         | 8      |
| 125%         | 10     |
| 150%         | 12     |
| 200%         | 16     |
| 400%         | 32     |

 

## <a name="tile-assets"></a>磚資產


每個磚資產都與它所在的磚等大小。 您可以資產的兩個不同的呈現方式在您的應用程式磚上打印商標：

1. 圖示或標誌置中，含有邊框間距。 這樣可讓背板色彩顯示：

![磚和背板](images/assetguidance05.png)

2.  跨頁的商標磚，不含邊框間距：

![磚會跨頁顯示](images/assetguidance06.png)

為了在裝置間保持一致性，每個磚大小 (小型、中型、寬形和大型) 都有自己的大小調整關聯性。 為了讓磚有一致的圖示放置方式，我們針對下列的磚大小建議了一些基本的邊框間距指導方針。 其中兩個紫色重疊交集的區域代表最理想的圖示擺設區域。 雖然圖示不會永遠剛好落在擺設區域內，但圖示視覺上的體積應該約等於提供的範例。

小型磚大小調整：

![小型磚大小調整範例](images/assetguidance07a.png)

中型磚大小調整：

![中型磚大小調整範例](images/assetguidance07b.png)

寬型磚大小調整：

![寬型磚大小調整範例](images/assetguidance07c.png)

大型磚大小調整：

![大型磚大小調整範例](images/assetguidance07d.png)

在這個範例中，圖示對磚而言過大：

![磚的圖示過大](images/assetguidance08a.png)

在這個範例中，圖示對磚而言過小：

![磚的圖示過小](images/assetguidance08b.png)

下列邊框間距比例對水平或垂直方向的圖示而言是最佳的。

對於小型磚，將圖示寬度和高度限制為磚大小的 66%：

![小型磚大小調整比例](images/assetguidance09.png)

對於中型磚，將圖示寬度限制為磚大小的 66%，將高度限制為磚大小的 50%。 這樣可以防止商標列中的元素重疊：

![中型磚大小調整比例](images/assetguidance10.png)

對於寬型磚，將圖示寬度限制為磚大小的 66%，將高度限制為磚大小的 50%。 這樣可以防止商標列中的元素重疊：

![寬型磚大小調整比例](images/assetguidance11.png)

對於大型磚，將圖示寬度限制為磚大小的 66%，將高度限制為磚大小的 50%：

![大型磚大小比例](images/assetguidance12.png)

有些圖示是水平或垂直方向，但其他圖示卻有著更複雜的圖形，使其無法恰好符合目標尺寸。 看起來像置中的圖示可能偏向另一側。 在這個案例中，假如圖示的視覺大小與恰好符合的圖示相同，則部分圖示可能會超出建議的擺設區域：

![三個置中的圖示](images/assetguidance13.png)

對於跨頁資產，請考量在磚的邊界和邊緣內互動的元素。 將邊界維持在至少是磚高或磚寬的 16%。 如為最小型磚大小，這個百分比表示邊界寬度的兩倍：

![含邊界的跨頁磚](images/assetguidance14.png)

在這個範例中，邊界太擠了：

![邊界過小的跨頁磚](images/assetguidance15.png)

## <a name="tile-assets-in-list-views"></a>清單檢視中的磚資產


磚也會出現在清單檢視中。 對於出現在清單檢視的磚資產的大小調整指導方針，與前述的磚資產稍有不同。 本節詳細說明這些大小調整特性。

![清單檢視中的磚資產](images/assetguidance16.png)

將圖示寬度和高度限制為磚大小的 75%：

![清單檢視磚的圖示大小調整](images/assetguidance17.png)

對於垂直和水平圖示格式，將寬度和高度限制為磚大小的 75%：

![清單檢視磚的圖示大小調整](images/assetguidance18.png)

對於重要商標元素的跨頁圖檔，請維持至少 12.5% 的邊界：

![清單檢視磚中的跨頁圖檔](images/assetguidance19.png)

在這個範例中，圖示對磚而言過大：

![對磚而言過大的圖示](images/assetguidance20a.png)

在這個範例中，圖示對磚而言過小：

![對磚而言過小的圖示](images/assetguidance20b.png)

## <a name="target-based-assets"></a>以目標為基礎的資產


以目標為基礎的資產適用於圖示和磚，會顯示在 Windows 工作列、工作檢視、ALT + TAB 及貼齊小幫手上，以及開始畫面磚的右下角。 您不需要將邊框間距新增至這些資產；如果需要，Windows 會新增邊框間距。 這些資產至少需要 16 個像素的擺設區域。 以下是這些資產出現在 Windows 工作列上之圖示中的範例：

![Windows 工作列中的資產](images/assetguidance21.png)

雖然這些 UI 在彩色背板上方預設會使用以目標為基礎的資產，但您也可以使用以目標為基礎的無背板資產。 建立無背板資產的時機，就是當它們可能會顯示在各種背景色彩時：

![無背板和有背板資產](images/assetguidance22.png)

這些是以目標為基礎的資產的大小建議 (縮放比例 100%)：

![以目標為基礎的資產的大小調整 (縮放比例 100%)](images/assetguidance23.png)

## <a name="splash-screen-assets"></a>啟動顯示畫面資產


您可以提供啟動顯示畫面影像，做為影像檔案的直接路徑或做為資源。 藉由使用資源參考，您可以提供不同縮放比例的影像，讓 Windows 可以為裝置和螢幕解析度選擇最佳大小。 您也可以提供協助工具的高對比影像和當地語系化的影像，以符合不同的 UI 語言。

如果您在文字編輯器中開啟 "Package.appxmanifest"，[**SplashScreen**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-splashscreen) 元素即會顯示為 [**VisualElements**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements) 元素的子項。 資訊清單檔案中的預設啟動顯示畫面標記在文字編輯器中看起來像這樣：

```XML
<uap:SplashScreen Image="Assets\SplashScreen.png" /></code></pre></td>
</tr>
</tbody>
</table>
```

無論使用何種裝置，啟動顯示畫面資產都會置中：

![啟動顯示畫面資產的大小調整](images/assetguidance27.png)

## <a name="high-contrast-assets"></a>高對比資產


高對比模式使用不同的資產集合，高對比白色使用白色背景和黑色文字，高對比黑色使用黑色背景和白色文字。 如果您沒有為應用程式提供高對比資產，將使用標準資產。

如果您應用程式的標準資產在黑白背景上的呈現是可接受的視覺體驗，那麼您的應用程式在高對比模式時看起來應該至少是令人滿意的。 如果您的標準資產在黑白背景上的呈現是無法接受的視覺體驗，請考慮具體包含高對比資產。 這些範例示範高對比資產的兩種類型：

![高對比比例資產範例](images/assetguidance28.png)

如果您決定提供高對比資產，您需要包含兩組 — 黑底白字和白底黑字。 在您的套件中包含這些資產時，您可以為黑底白字資產建立一個「對比 - 黑色」資料夾，為白底黑字資產建立一個「對比 - 白色」資料夾。

## <a name="asset-size-tables"></a>資產大小表格


我們強烈建議您起碼為 100、200 及 400 的縮放比例提供資產。 針對所有縮放比例提供資產將可提供最佳的使用者體驗。

<br/>

<table>
<thead>
<tr><th colspan="3">小型磚 (Square71x71Logo)</th></tr>
</thead>
<tbody>
<tr>
    <td width="20%">100% 縮放</td>
    <td width="20%">71x71</td>
    <td>Square71x71Logo.scale-100.png</td>
</tr>
<tr>
    <td>125% 縮放</td>
    <td>89x89</td>
    <td>Square71x71Logo.scale-125.png</td>
</tr>
<tr>
    <td>150% 縮放</td>
    <td>107x107</td>
    <td>Square71x71Logo.scale-150.png</td>
</tr>
<tr>
    <td>200% 縮放</td>
    <td>142x142</td>
    <td>Square71x71Logo.scale-200.png</td>
</tr>
<tr>
    <td>400% 縮放</td>
    <td>284x284</td>
    <td>Square71x71Logo.scale-400.png</td>
</tr>
</tbody>
</table>

<br/>

<table>
<thead>
<tr><th colspan="3">中型磚 (Square150x150Logo)</th></tr>
</thead>
<tbody>
<tr>
    <td width="20%">100% 縮放</td>
    <td width="20%">150x150</td>
    <td>Square150x150Logo.scale-100.png</td>
</tr>
<tr>
    <td>125% 縮放</td>
    <td>188x188</td>
    <td>Square150x150Logo.scale-125.png</td>
</tr>
<tr>
    <td>150% 縮放</td>
    <td>225x225</td>
    <td>Square150x150Logo.scale-150.png</td>
</tr>
<tr>
    <td>200% 縮放</td>
    <td>300x300</td>
    <td>Square150x150Logo.scale-200.png</td>
</tr>
<tr>
    <td>400% 縮放</td>
    <td>600x600</td>
    <td>Square150x150Logo.scale-400.png</td>
</tr>
</tbody>
</table>

<br/>

<table>
<thead>
<tr><th colspan="3">寬形磚 (Wide310x150Logo)</th></tr>
</thead>
<tbody>
<tr>
    <td width="20%">100% 縮放</td>
    <td width="20%">310x150</td>
    <td>Wide310x150Logo.scale-100.png</td>
</tr>
<tr>
    <td>125% 縮放</td>
    <td>388x188</td>
    <td>Wide310x150Logo.scale-125.png</td>
</tr>
<tr>
    <td>150% 縮放</td>
    <td>465 x 225</td>
    <td>Wide310x150Logo.scale-150.png</td>
</tr>
<tr>
    <td>200% 縮放</td>
    <td>620x300</td>
    <td>Wide310x150Logo.scale-200.png</td>
</tr>
<tr>
    <td>400% 縮放</td>
    <td>1240x600</td>
    <td>Wide310x150Logo.scale-400.png</td>
</tr>
</tbody>
</table>

<br/>

<table>
<thead>
<tr><th colspan="3">大型磚 (Square310x310Logo)</th></tr>
</thead>
<tbody>
<tr>
    <td width="20%">100% 縮放</td>
    <td width="20%">310x310</td>
    <td>Square310x310Logo.scale-100.png</td>
</tr>
<tr>
    <td>125% 縮放</td>
    <td>388x388</td>
    <td>Square310x310Logo.scale-125.png</td>
</tr>
<tr>
    <td>150% 縮放</td>
    <td>465x465</td>
    <td>Square310x310Logo.scale-150.png</td>
</tr>
<tr>
    <td>200% 縮放</td>
    <td>620x620</td>
    <td>Square310x310Logo.scale-200.png</td>
</tr>
<tr>
    <td>400% 縮放</td>
    <td>1240x1240</td>
    <td>Square310x310Logo.scale-400.png</td>
</tr>
</tbody>
</table>

<br/>

<table>
<thead>
<tr><th colspan="3">應用程式清單圖示 (Square44x44Logo)</th></tr>
</thead>
<tbody>
<tr>
    <td width="20%">100% 縮放</td>
    <td width="20%">44x44</td>
    <td>Square44x44Logo.scale-100.png</td>
</tr>
<tr>
    <td>125% 縮放</td>
    <td>55x55</td>
    <td>Square44x44Logo.scale-125.png</td>
</tr>
<tr>
    <td>150% 縮放</td>
    <td>66x66</td>
    <td>Square44x44Logo.scale-150.png</td>
</tr>
<tr>
    <td>200% 縮放</td>
    <td>88x88</td>
    <td>Square44x44Logo.scale-200.png</td>
</tr>
<tr>
    <td>400% 縮放</td>
    <td>176x176</td>
    <td>Square44x44Logo.scale-400.png</td>
</tr>
</tbody>
</table>

<br/>

<table>
<thead>
<tr><th colspan="3">啟動顯示畫面 (SplashScreen)</th></tr>
</thead>
<tbody>
<tr>
    <td width="20%">100% 縮放</td>
    <td width="20%">620x300</td>
    <td>SplashScreen.scale-100.png</td>
</tr>
<tr>
    <td>125% 縮放</td>
    <td>775x375</td>
    <td>SplashScreen.scale-125.png</td>
</tr>
<tr>
    <td>150% 縮放</td>
    <td>930x450</td>
    <td>SplashScreen.scale-150.png</td>
</tr>
<tr>
    <td>200% 縮放</td>
    <td>1240x600</td>
    <td>SplashScreen.scale-200.png</td>
</tr>
<tr>
    <td>400% 縮放</td>
    <td>2480x1200</td>
    <td>SplashScreen.scale-400.png</td>
</tr>
</tbody>
</table>

<br/>
 

**以目標為基礎的資產**

以目標為基礎的資產用於多個縮放比例。 以目標為基礎的資產的元素名稱是 **Square44x44Logo**。 我們強烈建議您起碼提交下列資產：

16x16、24x24、32x32、48x48、256 x 256

下表列出所有以目標為基礎的資產大小及對應的檔案名稱範例：

| 資產大小 | 檔案名稱範例                  |
|------------|------------------------------------|
| 16x16\*    | Square44x44Logo.targetsize-16.png  |
| 24x24\*    | Square44x44Logo.targetsize-24.png  |
| 32x32\*    | Square44x44Logo.targetsize-32.png  |
| 48x48\*    | Square44x44Logo.targetsize-48.png  |
| 256x256\*  | Square44x44Logo.targetsize-256.png |
| 20x20      | Square44x44Logo.targetsize-20.png  |
| 30x30      | Square44x44Logo.targetsize-30.png  |
| 36x36      | Square44x44Logo.targetsize-36.png  |
| 40x40      | Square44x44Logo.targetsize-40.png  |
| 60x60      | Square44x44Logo.targetsize-60.png  |
| 64x64      | Square44x44Logo.targetsize-64.png  |
| 72x72      | Square44x44Logo.targetsize-72.png  |
| 80x80      | Square44x44Logo.targetsize-80.png  |
| 96x96      | Square44x44Logo.targetsize-96.png  |

 

\* 提交這些資產大小做為基準線

## <a name="asset-types"></a>資產類型


此處列出所有資產類型、資產用途和建議的檔案名稱。

**磚資產**

-   置中的資產一般使用於 [開始] 畫面上，以展示您的應用程式。
-   檔案名稱格式：[Square\Wide]\*x\*Logo.scale-\*.png
-   受影響的應用程式：每個 UWP 應用程式
-   用途：
    -   預設的 [開始] 畫面磚 (桌面和行動)
    -   重要訊息中心 (桌面和行動)
    -   工作切換器 (行動)
    -   共用選擇器 (行動)
    -   選擇器 (行動)
    -   市集

**含有背板的可調式清單資產**

-   這些資產用於要求縮放比例的介面。 資產可能使用系統提供的背板，或附帶本身的背景色彩 (如果應用程式有包含的話)。
-   檔案名稱格式：Square44x44Logo.scale-\*.png
-   受影響的應用程式：每個 UWP 應用程式
-   用途：
    -   [開始] 畫面的所有 App 清單 (桌面)
    -   [開始] 畫面的最常用清單 (桌面)
    -   工作管理員 (桌面)
    -   Cortana 搜尋結果
    -   [開始] 畫面的所有 App 清單 (行動)
    -   設定

**含有背板的目標大小清單資產**

-   這些是固定不調整的資產大小。 主要用於傳統體驗。 系統會檢查資產。
-   檔案名稱格式：Square44x44Logo.targetsize-\*.png
-   受影響的應用程式：每個 UWP 應用程式
-   用途：
    -   [開始] 畫面的捷徑清單 (桌面)
    -   [開始] 畫面的右下角的磚 (桌面)
    -   捷徑 (桌面)
    -   控制台 (桌面)

**不含背板的目標大小清單資產**

-   這些是系統不會提供背板或調整的資產。
-   檔案名稱格式：Square44x44Logo.targetsize-\*\_altform-unplated.png
-   受影響的應用程式：每個 UWP 應用程式
-   用途：
    -   工作列和工作列縮圖 (桌面)
    -   工作列捷徑清單
    -   工作檢視
    -   ALT+TAB

**副檔名資產**

-   這些是副檔名特定的資產。 它們顯示在檔案總管中的 Win32 樣式檔案關聯圖示旁，而且必須不限佈景主題。 大小調整在桌面和行動平台上各不相同。
-   檔案名稱格式：\*LogoExtensions.targetsize-\*.png
-   受影響的應用程式：音樂、影片、相片、Microsoft Edge、Microsoft Office
-   用途：
    -   檔案總管
    -   Cortana
    -   不同的 UI 介面 (桌面)

**啟動顯示畫面**

-   在您的應用程式啟動顯示畫面上出現的資產。 在桌面和行動平台上自動調整。
-   檔案名稱格式：SplashScreen.scale-*.png
-   受影響的應用程式：每個 UWP 應用程式
-   用途：
    -   應用程式的啟動顯示畫面