---
ms.assetid: 09BA9250-A476-4803-910E-52F0A51704B1
description: 本文說明如何使用 IMediaEncodingProperties 介面來設定相機預覽資料流及所擷取之相片和視訊的解析度和畫面播放速率。
title: 設定 MediaCapture 的格式、解析度和畫面播放速率
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 031f3f8f5b15f839348c05c1fd26b7711856d659
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74256247"
---
# <a name="set-format-resolution-and-frame-rate-for-mediacapture"></a>設定 MediaCapture 的格式、解析度和畫面播放速率



本文說明如何使用 [**IMediaEncodingProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.IMediaEncodingProperties) 介面來設定相機預覽資料流及所擷取之相片和視訊的解析度和畫面播放速率。 它也會說明如何確保預覽資料流的外觀比例符合所擷取的媒體。

相機設定檔提供更進階的方式來探索和設定相機的資料流屬性，但並非所有裝置都支援它們。 如需詳細資訊，請參閱[相機設定檔](camera-profiles.md)。

本文中的程式碼是採用 [CameraResolution 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraResolution)的程式碼。 您可以下載範例以查看實際使用的程式碼，或以此範例做為自己的 App 起點。

> [!NOTE] 
> 本文是以[使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)中討論的概念和程式碼為基礎，其中說明實作基本相片和視訊擷取的步驟。 建議您先熟悉該文中的基本媒體擷取模式，然後再移到更多進階的擷取案例。 本文章中的程式碼假設您的 app 已有正確初始化的 MediaCapture 執行個體。

## <a name="a-media-encoding-properties-helper-class"></a>媒體編碼屬性 Helper 類別

建立簡單的 Helper 類別來包裝 [**IMediaEncodingProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.IMediaEncodingProperties) 介面的功能，就能更輕易地選取一組符合特定準則的編碼屬性。 這個 Helper 類別因編碼屬性功能的下列行為而特別有用：

**警告**   [**VideoDeviceController. GetAvailableMediaStreamProperties**](https://docs.microsoft.com/uwp/api/windows.media.devices.videodevicecontroller.getavailablemediastreamproperties)方法會接受[**MediaStreamType**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaStreamType)列舉的成員，例如**VideoRecord**或**相片**，並傳回可傳達串流編碼設定的[**ImageEncodingProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties)或[**VideoEncodingProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.VideoEncodingProperties)物件清單，例如已捕捉的相片或影片的解析度。 呼叫 **GetAvailableMediaStreamProperties** 的結果可能包括 **ImageEncodingProperties** 或 **VideoEncodingProperties**，而不論指定的是哪一個 **MediaStreamType** 值。 基於這個原因，您應該一律檢查每個傳回值的類型，並將其轉換為適當的類型，然後才嘗試存取任何屬性值。

下列定義的 Helper 類別會處理 [**ImageEncodingProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties) 或 [**VideoEncodingProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.VideoEncodingProperties) 的類型檢查和轉換，如此一來，您的 app 程式碼就不需要區分這兩種類型。 此外，Helper 類別所公開的屬性適用於屬性的外觀比例、畫面播放速率 (僅適用於視訊編碼屬性)，以及更容易在 app UI 中顯示編碼屬性的易記名稱。

您必須在 Helper 類別的來源檔案中包含 [**Windows.Media.MediaProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties) 命名空間。

[!code-cs[MediaEncodingPropertiesUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetMediaEncodingPropertiesUsing)]

[!code-cs[StreamPropertiesHelper](./code/BasicMediaCaptureWin10/cs/StreamPropertiesHelper.cs#SnippetStreamPropertiesHelper)]

## <a name="determine-if-the-preview-and-capture-streams-are-independent"></a>判斷預覽資料流和擷取資料流是否為各自獨立

在某些裝置上，會針對預覽資料流和擷取資料流使用相同的硬體 pin。 在這些裝置上，設定其中一個的編碼屬性，也會設定另一個的編碼屬性。 在使用不同的硬體 pin 進行擷取和預覽的裝置上，可以為每個資料流個別設定屬性。 使用下列程式碼，來判斷預覽資料流和擷取資料流是否為各自獨立。 您應該調整 UI，根據這個測試的結果個別啟用或停用資料流設定。

[!code-cs[CheckIfStreamsAreIdentical](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCheckIfStreamsAreIdentical)]

## <a name="get-a-list-of-available-stream-properties"></a>取得可用的資料流屬性清單

針對擷取裝置取得可用的資料流屬性清單，方法是取得適用於 app 之 [MediaCapture**物件的**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.VideoDeviceController)VideoDeviceController[](capture-photos-and-video-with-mediacapture.md)，然後呼叫 [**GetAvailableMediaStreamProperties**](https://docs.microsoft.com/uwp/api/windows.media.devices.videodevicecontroller.getavailablemediastreamproperties) 並傳入其中一個 [**MediaStreamType**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaStreamType) 值 (**VideoPreview**、**VideoRecord** 或 **Photo**)。 這個範例使用 Linq 語法，針對每個從 **GetAvailableMediaStreamProperties** 傳回的 [**IMediaEncodingProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.IMediaEncodingProperties) 值，建立 **StreamPropertiesHelper** 物件的清單 (已定義於本文先前的內容中)。 這個範例會先使用 Linq 擴充方法，先根據解析度，然後根據畫面播放速率，來排序傳回的屬性。

如果您的 App 具有特定的解析度或畫面播放速率需求，您可以透過程式設計方式選取一組媒體編碼屬性。 典型的相機 app 將改為公開 UI 中可用屬性的清單，並允許使用者選取他們想要的設定。 **ComboBoxItem** 是在清單中，針對 **StreamPropertiesHelper** 物件清單中的每一個項目來建立。 內容已設定為 Helper 類別所傳回的易記名稱，且標籤已設定為 Helper 類別本身，如此一來，稍後便能使用它來擷取相關聯的編碼屬性。 每個 **ComboBoxItem** 會接著新增到傳遞給方法的 **ComboBox**。

[!code-cs[PopulateStreamPropertiesUI](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPopulateStreamPropertiesUI)]

## <a name="set-the-desired-stream-properties"></a>設定所需的串流屬性

請透過呼叫 [**SetMediaStreamPropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.media.devices.videodevicecontroller.setmediastreampropertiesasync)、傳入 **MediaStreamType** 值、指出是否應設定相片、視訊或預覽屬性，以告知視訊裝置控制器使用您所需的編碼屬性。 這個範例會在使用者於利用 **PopulateStreamPropertiesUI** Helper 方法填入的其中一個 **ComboBox** 物件中選取項目時，設定要求的編碼屬性 。

[!code-cs[PreviewSettingsChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPreviewSettingsChanged)]

[!code-cs[PhotoSettingsChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPhotoSettingsChanged)]

[!code-cs[VideoSettingsChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoSettingsChanged)]

## <a name="match-the-aspect-ratio-of-the-preview-and-capture-streams"></a>讓預覽串流與擷取串流的外觀比例相符

典型的相機 App 會提供 UI 讓使用者選取影片或相片擷取解析度，但是會以程式設計方式設定預覽解析度。 有幾個不同的策略可用來為您的 App 選取最佳預覽串流解析度：

-   選取可用的最高預覽解析度，讓 UI 架構執行任何必要的預覽縮放。

-   選取與擷取解析度最接近的預覽解析度，讓預覽以和最終擷取的媒體最接近的面貌呈現。

-   選取與 [**CaptureElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) 的大小最接近的預覽解析度，讓多餘的像素無法通過預覽串流管線。

**重要**   在某些裝置上，您可以為相機的預覽串流和 capture 串流設定不同的外觀比例。 因為這個不相符而引發的框架剪裁，可能會在擷取的媒體中顯示內容，而此內容不會顯示於預覽中，這會導致負面使用者經驗。 強烈建議您在小型容錯視窗內，針對預覽和擷取資料流使用相同的外觀比例。 啟用完全不同的解析度進行擷取和預覽是正常的，只要外觀比例非常接近即可。


為了確保相片或視訊擷取資料流符合預覽串流的外觀比例，這個範例會呼叫 [**VideoDeviceController.GetMediaStreamProperties**](https://docs.microsoft.com/uwp/api/windows.media.devices.videodevicecontroller.getmediastreamproperties) 並傳入 **VideoPreview** 列舉值，以要求預覽資料流目前的資料流屬性。 接著會定義一個小型外觀比例的容錯視窗，讓我們能夠包含未與預覽資料流完全相同的外觀比例 (儘管它們非常相近)。 接下來，會使用 Linq 擴充方法，僅選取外觀比例在已定義的預覽資料流容許範圍內的 **StreamPropertiesHelper** 物件。

[!code-cs[MatchPreviewAspectRatio](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetMatchPreviewAspectRatio)]

 

 




