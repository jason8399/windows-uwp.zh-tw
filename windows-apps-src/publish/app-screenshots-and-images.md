---
author: jnHs
Description: You can select the screenshots, logos, and other art assets (such as trailers and promotional images) to include in your app's Store listing.
title: 應用程式螢幕擷取畫面、影像及預告片
ms.assetid: D216DD2B-F43D-4D26-82EE-0CD34DB929D8
ms.author: wdg-dev-content
ms.date: 4/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 預告片, 影片, 螢幕擷取畫面, 影像, 圖示, Store 清單, Store 清單影像
ms.localizationpriority: high
ms.openlocfilehash: e0ac8e01aab07e68e0a4f22160cb58e558b4dc42
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/30/2018
ms.locfileid: "1817779"
---
# <a name="app-screenshots-images-and-trailers"></a>應用程式螢幕擷取畫面、影像及預告片

完善設計的影像是您在Microsoft Store中向潛在客戶呈現您的應用程式的主要方式。

您可以提供[螢幕擷取畫面](#screenshots)、[標誌](#store-logos)及其他美工圖案資產 (例如[預告片](#trailers)和[促銷影像](##additional-art-assets))，加入您應用程式的Microsoft Store清單中。 有些是必要的，有些則是選擇性的（雖然為了最佳的Microsoft Store顯示效果，包含一些選擇性影像是重要的）。 

在[應用程式提交程序](app-submissions.md)中，您會在[Microsoft Store清單](create-app-store-listings.md)步驟中提供這些美工圖案資產。 請注意，根據客戶的作業系統和其他因素而定，在Microsoft Store中使用的影像以及顯示影像的方式可能有所差異。

Microsoft Store可能也會使用您的應用程式磚以及應用程式套件中包含的其他影像。 在您提交應用程式之前，執行 [Windows 應用程式認證套件](../debug-test-perf/windows-app-certification-kit.md)，以判斷您是否缺少任何必要影像。 如需關於這些影像的指導方針和建議，請參閱[磚和圖示資產](../design/shell/tiles-and-notifications/app-assets.md)。

## <a name="screenshots"></a>螢幕擷取畫面

螢幕擷取畫面是客戶可在您應用程式的 Store 清單中看見的應用程式影像。

您可以選擇提供您應用程式支援的不同裝置系列的螢幕擷取畫面，如此一來，當客戶在該類型裝置上檢視您應用程式的 Store 清單時，就會出現適當的螢幕擷取畫面。 

您的提交程序只需要一個螢幕擷取畫面 (適用於任何裝置系列)，但是您可以提供數個螢幕擷取畫面；最多 9 個桌面螢幕擷取畫面，以及其他裝置系列最多 8 個螢幕擷取畫面。 我們建議您針對應用程式所支援的每個裝置系列提供至少四個螢幕擷取畫面，如此人們就可查看應用程式在其裝置類型上如何顯示。 (不包含您應用程式不支援裝置系列的螢幕擷取畫面)。請注意，也會針對 Surface Hub 裝置顯示**桌面**螢幕擷取畫面。

> [!NOTE]
> Microsoft Visual Studio 提供[可協助您擷取螢幕擷取畫面的工具](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-in-the-simulator#BKMK_Capture_a_screenshot_of_your_app_for_submission_to_the_Microsoft_Store)。

每個螢幕擷取畫面都必須是橫向或直向的 .png 檔案，而且檔案大小不能超過 50 MB。

大小需求是根據裝置系列而有所不同：
- 桌上型電腦︰1366 x 768 像素或更高。 支援 4K 影像 (3840 x 2160)。 (也會顯示給 Surface Hub 裝置上的客戶)。
- 行動裝置：影像必須是下列其中一個格式：1080 x 1920、1920 x 1080、768 x 1280、1280 x 768、720 x 1280、1280 x 720、800 x 480 或 480 x 800 像素。
- Xbox：3480 x 2160 像素或更小。 支援 4K 影像 (3840 x 2160)。
- 全像攝影：1268 x 720 像素或更高。 支援 4K 影像 (3840 x 2160)。

為獲得最佳顯示效果，建立螢幕擷取畫面時務必記住下列指導方針：
- 將重要的視覺效果和文字保持在影像頂端 3/4。 文字重疊可能會出現在底部 1/4。 
- 不要加入其他的標誌、圖示或行銷訊息到螢幕擷取畫面中。
- 不要使用非常淺或非常深的色彩，或高對比的線條，否則可能會干擾文字重疊的易讀性。

您也可以提供描述每個螢幕擷取畫面的簡短標題 (不超過 200 個字元)。

> [!TIP]
> 螢幕擷取畫面會依序顯示在您的清單中。 上傳螢幕擷取畫面之後，您可以進行拖放將它們重新排列。 

請注意，如果您為[多種語言](supported-languages.md)建立Microsoft Store清單，則對於每個語言您都會有一個 [**Microsoft Store清單**] 頁面。 您必須針對每種語言個別上傳影像 (即使您使用相同影像)，並提供每種語言使用的標題。


## <a name="store-logos"></a>Microsoft Store 標誌

您可以上傳 Microsoft Store 標誌，為 Microsoft Store 增添自訂的顯示效果。 我們建議您提供這些影像，為您的應用程式支援的所有裝置與 OS 版本上提供最佳的 Store 清單顯示。 請注意，如果您的應用程式可用於 Xbox 客戶，則需要其中一些影像。

您可以提供 .png 檔案的影像 (不可大於 50 MB)，這些影像應遵循下列指導方針。

### <a name="916-poster-art-720-x-1080-or-1440-x-2160-pixels"></a>9:16 海報美工圖案 (720 x 1080 或 1440 x 2160 像素)

這是用於 Windows 10 和 Xbox 裝置客戶的主要標誌影像，因此我們**強烈建議**提供這個影像以確保正確顯示。 如果未提供此項目，您的清單可能不美觀，當客戶瀏覽 Microsoft Store 時也無法與他們看到的其他清單保持一致。 此影像也會用於搜尋結果或編輯精選集錦。

此影像應包含您的應用程式名稱，而且影像上的任何文字都應符合可存取易讀需求 (4.51 對比率)。 請注意，文字重疊會出現此影像的下方四分之一處，因此請不要在此處加入文字或重要圖案。

> [!NOTE]
> 如果您的應用程式要提供給 Xbox 上的客戶，此影像為**必要**且必須包含產品的標題。 標題必須顯示在影像的上方四分之三處，因為文字重疊可能會出現在影像的下方四分之一處。

### <a name="11-box-art-1080-x-1080-or-2160-x-2160-pixels"></a>1:1 方塊美工圖案 (1080 x 1080 或 2160 x 2160 像素)

此影像會出現在 Windows 10 (包括 Xbox) 的 Microsoft Store 頁面，而如果您未提供 **9:16 海報美工圖案**，此影像也會做為您的主要標誌。 這個影像也應該包含您應用程式的名稱。 文字重疊會出現此影像的下方四分之一處，因此請不要在此處加入文字或重要圖案。 請務必在這個影像中加入您應用程式的名稱。 

> [!NOTE]
> 如果您的應用程式要提供給 Xbox 上的客戶，此影像為**必要**且必須包含產品的標題。 標題必須顯示在影像的上方四分之三處，因為文字重疊可能會出現在影像的下方四分之一處。

### <a name="11-app-tile-icon-300-x-300-pixels"></a>1:1 應用程式磚圖示 (300 x 300 像素)

必須要有此影像，才能在 Windows Phone 8.1 與較舊版本上正確顯示。 如果您的應用程式支援 Windows Phone 8.1 或較舊版本但您未提供這個影像，這些客戶將會在應用程式清單中看到空白圖示。 (這也適用於使用 Windows 10 的客戶，如果您的應用程式只有以 Windows Phone 8.1 或更舊版本為目標的套件)。如果您的提交內容*僅*包含 UWP 套件，則不需要提供這個影像。 (請注意，如果您提交的內容同時包含 Windows Phone 8.x 套件和 UWP 套件，而且您提供此影像，在某些 Microsoft Store 配置中它可能會用在 Windows 10 上。 若要避免此情況，您可以建立適用於您應用程式支援之 Windows Phone 版本的[平台專屬清單](create-platform-specific-store-listings.md)，並且僅在那裡包含應用程式磚圖示)。

您也可以選擇性地為 Windows 10 (包含 Xbox) 客戶顯示清單時，防止 Microsoft Store 使用您應用程式套件中的標誌影像，指定讓 Microsoft Store 只使用您上傳的影像。 如此您更能為 Windows 10 (包含 Xbox) 的客戶，在整個 Microsoft Store 中掌控各種不同顯示的應用程式外觀。

若只要將上傳的影像用於在 Windows 10 (包含 Xbox) 的 Microsoft Store 中顯示，請勾選 **\[針對 Windows 10 客戶顯示上傳的標誌影像，而不是我的套件中的影像\]** 方塊。 (如果未勾選此方塊，則會使用來自您應用程式套件中的影像)。

勾選此方塊時，就會出現稱為 **\[上傳的 Microsoft Store 標誌\]** 的新區段。 在此處可上傳 3 個影像，包括 300 x 300「應用程式磚圖示」大小 (如果您核取此方塊，提供該影像的欄位就會移至此區段中)。 如果您使用此選項，建議您三種影像大小都提供：300 x 300、150 x 150 及 71 x 71 像素。 不過，只需要 300 x 300 大小。


<span id="promotional-images" />

## <a name="additional-art-assets"></a>其他美工圖案資產

此區段讓您提供圖稿，幫助您更有效率地在 Microsoft Store 中顯示產品。 我們建議您提供這些影像，建立更具吸引力的 Store 清單。

> [!TIP]
> 如果您想要將[預告影片](#trailers)包含在 Store 清單中，尤其建議使用 **16:9 超級主角美工圖案**。如果未包含，預告片不會顯示在清單頂端。

若要新增這些影像，請選取 **\[其他美工圖案資產\]** 區段中的 **\[顯示詳細資料\]**。



### <a name="windows-10-and-xbox-image-169-super-hero-art"></a>Windows 10 和 Xbox 影像 (16:9 超級主角美工圖案)

**16:9 超級主角美工圖案 (1920 x 1080 或 3840 x 2160 像素)** 影像用於所有 Windows 10 裝置類型 (包括 Xbox) 的 Microsoft Store 中各種不同的配置。 我們建議您提供此影像，無論您的應用程式是以哪個作業系統版本或裝置類型為目標。

如果您的清單包含[預告影片](#trailers)，*需要*這個影像以正確顯示。 對於 Windows 10 1607 版或更新版本 (包括 Xbox) 的客戶，它做為您的 Store 清單頂端的主要影像 (或在任何預告片播放完畢之後出現)。 它也可能在整個 Microsoft Store 的促銷配置中用於展示您的應用程式。 請注意，此影像不能包含產品的標題或其他文字。

以下提供一些您在設計影像時應記住的秘訣：

- 影像必須是 .png 且是 1920 x 1080 像素或 3840 x 2160 像素。
- 選取與應用程式相關的動態影像，以促進辨識和區分。 避免使用圖庫影像或通用視覺效果。
- 不要在影像中包含文字。
- 避免在影像的底部三分之一處放置關鍵視覺元素 (因為在某些版面配置中，我們可能會在這個部分套用漸層)。
- 將最重要的詳細資料放置在影像中心 (因為在某些版面配置中，我們可能會裁剪映像)。
- 將空白空間縮至最小。
- 避免顯示您的應用程式 UI，而且請勿使用任何裝置特定的圖像。
- 避免政治性和國家/地區的佈景主題、旗標或宗教象徵。
- 請勿包含敏感性手勢、裸露、賭博、貨幣、藥品、菸草或酒類的影像。
- 請勿使用武器指向觀看者或過度暴力和血腥。

提供此影像可讓我們考慮將您的應用程式納入精選促銷商機，但不保證一定會展示您的應用程式。 如需詳細資訊，請參閱[讓您的應用程式更容易宣傳](make-your-app-easier-to-promote.md)。


### <a name="xbox-images"></a>Xbox 影像

如果您將應用程式發行至 Xbox，則需要這些影像以便正確顯示。 

您可以上傳 3 種不同的大小：
- **品牌主要美工圖案，584 x 800 像素**︰必須包含產品的標題。 此影像上需要有商標列。 請將標題和所有重要圖像放在影像的上方四分之三處，因為底部四分之一處可能會出現重疊。
- **主角美工圖案，1920 x 1080 像素**︰必須包含產品的標題。 請將標題和所有重要圖像放在影像的上方四分之三處，因為底部四分之一處可能會出現重疊。
- **精選促銷正方形美工圖案，1080 x 1080 像素**：*不能*包括產品的標題。

> [!NOTE]
> 為確保 Xbox 上的最佳顯示，您必須也在[Microsoft Store標誌](#store-logos) 區段中提供 **9:16（720 x 1080 或 1440 x 2160 像素）** 影像。


### <a name="holographic-image"></a>全像影像

如果您的應用程式支援全像攝影裝置系列，才會使用 **2:1 (2400 x 1200)** 影像。 如果支援，我們建議提供此影像。


<span id="optional-promotional-images" />

### <a name="images-only-for-windows-8x-andor-windows-phone-8x"></a>僅適用於 Windows 8.x 和/或 Windows Phone 8.x 的影像 

如果您的應用程式支援舊版作業系統版本 (Windows 8.x 和/或 Windows Phone 8.x)，則必須提供這些影像，我們才能考慮將您的應用程式納入促銷配置中 (但不保證您的應用程式會是精選應用程式)。 如果您的應用程式不支援這些舊版的作業系統，您可以略過本節。 (本節先前稱為「**選用宣傳影像**」。)

**若是 Windows Phone 8.1 和較舊版本**，有兩種影像大小可用於促銷配置：**1000 x 800 像素 (5:4)** 和 **358 x 358 像素 (1:1)**。 如果您的應用程式是在 Windows Phone 8.1 或較舊版本上執行，建議您基於促銷考量來提供這兩個大小的影像。  

> [!TIP]
> 請務必在[Microsoft Store標誌](#store-logos)區段中提供 300 x 300 應用程式磚圖示，針對任何支援 Windows Phone 8.1 或較舊版本的任何提交。 這樣可確保您的應用程式在Microsoft Store中不會顯示空白圖示。  

**若是 Windows 8.1 和較舊版本**，某些促銷配置可能使用 **414 x 180** 像素大小的影像。 如果您的應用程式是在 Windows 8.1 或較舊版本上執行，建議您基於促銷考量來提供這個大小的影像。


## <a name="trailers"></a>預告片

預告片是短片，讓客戶能夠看見產品的實際使用情形，幫助他們更了解產品。 它們會在應用程式的Microsoft Store清單頂端顯示 (只要您在[宣傳影像](#promotional-images)區段中包含 **1920 x 1080 像素影像 (16:9)**)。 

預告片是以[平滑串流](http://www.iis.net/downloads/microsoft/smooth-streaming)編碼，會根據可用的頻寬和 CPU 資源對用戶端提供即時的視訊串流品質。

> [!NOTE]
> 預告片只在 Windows 10 版本 1607 或更新版本 (包括 Xbox) 上對客戶顯示。

### <a name="upload-trailers"></a>上傳預告片

您最多可以新增 15 支預告片至 Store 清單。 每支預告片務必符合下列全部的[需求](#trailer-requirements)。

對於您提供的每支預告片，您必須上傳視訊檔案 (.mp4 或 .mov)、縮圖影像和標題。

> [!IMPORTANT]
> 當使用預告片時，您還必須在[宣傳影像](#promotional-images) 區段中提供 **1920 x 1080 像素影像 (16:9)**，讓您的預告片顯示在Microsoft Store清單頂端。 在您的預告片播放完畢後，會顯示此影像。

依照這些建議讓您的預告片生效：
- 預告片應品質良好且符合最短長度 (建議 60 秒以內或小於 2 GB)。 
- 針對每支預告片使用不同的縮圖，讓客戶了解其獨特性。
- 由於某些 Microsoft Store 配置可能會稍微裁剪預告片的頂端和底部，因此務必確定重要資訊出現在畫面的正中央。
- 畫面播放速率和解析度應該符合來源材料。 例如，以 720p60 拍攝的內容應經過編碼，並以 720p60 上傳。 

另外也須遵循下列需求。

**若要新增預告片至清單：**
1. 在指定的方塊中上傳預告片 [**視訊檔案**]。 下拉式方塊也會顯示，方便您重複使用已上傳的預告片 (可能是其他語言的Microsoft Store清單)。
2. 上傳預告片之後，您將需要上傳與它搭配的 [**縮圖影像**]。 這必須是 1920 x 1080 像素的 .png 檔案，通常是取自預告片的靜態影像。
3. 按一下鉛筆圖示新增預告片的 [**標題**] (255 個字元以內)。
4. 如果您想要新增更多預告片至清單中，按一下 [**新增預告片**] 並重複上列步驟。

> [!TIP]
> 如果您已建立多種語言的 Microsoft Store 清單，您可以選取 **\[從現有預告片中選擇\]** 以重複使用已上傳的預告片。 您不需要為每一種語個別上傳預告片。

若要從清單中移除預告片，請按一下其檔案名稱旁的 **\[X\]**。 您可以選擇只要將它從目前使用的 Microsoft Store 清單中移除，或是從產品的所有 Microsoft Store 清單中移除 (所有語言中)。


### <a name="trailer-requirements"></a>預告片需求

提供預告片時，務必遵循下列需求：

- 視訊格式必須是 MOV 或 MP4。 
- 視訊長度不應超過 60 秒。
- 預告片的檔案大小不應超過 2 GB。 
- 視訊解析度必須是 1920 x 1080 像素或 3840 x 2160 像素。
- 縮圖必須是解析度為 1920 x 1080 像素或 3840 x 2160 像素的 PNG 檔案。
- 標題不可超過 255 個字元。 
- 不要在您的預告片中包含年齡分級。

就像 Store 清單頁面上的其他欄位一樣，預告片必須通過認證，才能發行至 Microsoft Store。 確定您的預告片遵循 [Microsoft Store 原則](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx)。

根據檔案類型，可能有其他需求。

#### <a name="mov"></a>MOV

<table>
<tr>
<td>

**視訊：**

<ul>
<li>1080p ProRes (HQ，適用的話)</li>
<li>原生畫面播放速率；慣用 29.97 FPS</li>
</ul>
</td>
<td>

**音訊：**

<ul>
<li>須為立體聲</li>
<li>建議音訊等級：-16 LKFS/LUFS</li>
</ul> 
</td>
</tr>
</table>

#### <a name="mp4"></a>MP4

<table>
<tr>
<td>

**視訊：**

<ul>
<li>轉碼器︰H.264</li>
<li>漸進式掃描 (無交錯)</li>
<li>高調明確</li>
<li>2 個連續 B 畫面格</li>
<li>封閉式 GOP。 一半畫面播放速率的 GOP</li>
<li>CABAC</li>
<li>每秒 50 MB </li>
<li>色彩空間︰4.2.0</li>
</ul>
</td>
<td>

**音訊：**

<ul>
<li>轉碼器：AAC-LC</li>
<li>通道：立體聲或環繞音效</li>
<li>取樣率︰48 KHz</li>
<li>音訊位元速率︰立體聲為每秒 384 KB，環繞音效為每秒 512 KB</li>
</ul>
</td>
</tr>
</table>

如為 H.264 夾層檔案，我們的建議如下：
- 容器：MP4
- 無編輯清單 (否則可能喪失 AV 同步)
- 檔案前面 Moov atom (快速開始)
