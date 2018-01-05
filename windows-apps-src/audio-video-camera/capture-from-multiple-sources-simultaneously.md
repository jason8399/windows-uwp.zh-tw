---
author: drewbatgit
ms.assetid: 
description: "本文示範如何從多個來源同步擷取視訊到單一檔案，內含多個內嵌視訊播放軌。"
title: "使用 MediaFrameSourceGroup 從多個來源擷取"
ms.author: drewbat
ms.date: 09/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 擷取, 視訊"
ms.localizationpriority: medium
ms.openlocfilehash: 56996cf9edb5aef317421e5ca8eef05153328bf9
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="capture-from-multiple-sources-using-mediaframesourcegroup"></a>使用 MediaFrameSourceGroup 從多個來源擷取

本文示範如何從多個來源同步擷取視訊到單一檔案，內含多個內嵌視訊播放軌。 從 RS3 開始，您可以為單一 **[MediaEncodingProfile](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile)** 指定多個 **[VideoStreamDescriptor](https://docs.microsoft.com/uwp/api/windows.media.core.videostreamdescriptor)** 物件。 這可讓您同時將多個串流編碼至單一檔案。 以此操作編碼的視訊串流必須包含在單一 **[MediaFrameSourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup)** 中，指定可在同一時間使用的目前裝置上的一組相機。 

如需使用 **[MediaFrameSourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup)** 搭配 **[MediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader)** 類別以啟用運用多個相機的即時電腦視覺情景相關資訊，請參閱[使用 MediaFrameReader 處理媒體畫面](process-media-frames-with-mediaframereader.md)。

本文章的其餘部分將逐步引導您從兩部彩色相機錄製影片到包含多個視訊播放軌的單一檔案。

## <a name="find-available-sensor-groups"></a>尋找可用的感應器群組
**MediaFrameSourceGroup** 代表可同時存取的畫面來源集合，通常是相機。 每個裝置的可用畫面來源群組集都不同，因此此範例的第一個步驟就是取得可用畫面來源群組的清單，並找到包含此例所需相機的群組，在此例中這需要兩部彩色相機。

**[MediaFrameSourceGroup.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup#Windows_Media_Capture_Frames_MediaFrameSourceGroup_FindAllAsync)** 方法會傳回目前裝置上可用的所有來源群組。 每個傳回的 **MediaFrameSourceGroup** 都有 **[MediaFrameSourceInfo](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo)** 物件的清單，描述群組中的每個畫面來源。 Linq 查詢用來尋找包含兩部彩色相機的來源群組，一個在前端面板上，一個在背面。 傳回匿名物件，包含每部彩色相機的選定 **MediaFrameSourceGroup** 和 **MediaFrameSourceInfo**。 不使用 Linq 語法，您可以改為循環處理每個群組以及每個 **MediaFrameSourceInfo** 來尋找符合您需求的群組。

請注意，並非每個裝置都包含有兩部彩色相機的來源群組，因此您應該要檢查以確認有找到來源群組，然後再擷取視訊。

[!code-cs[MultiRecordFindSensorGroups](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordFindSensorGroups)]

## <a name="initialize-the-mediacapture-object"></a>初始化 MediaCapture 物件
**[MediaCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture)** 類別是 UWP app 中用於大部分音訊、視訊和相片擷取作業的主要類別。 透過呼叫 **[InitializeAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture#Windows_Media_Capture_MediaCapture_InitializeAsync)**、傳遞 **[MediaCaptureInitializationSettings](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings)** 包含初始化參數的物件，來初始化物件。 在此範例中，指定的唯一設定是 **[SourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings#Windows_Media_Capture_MediaCaptureInitializationSettings_SourceGroup)** 屬性，設為上一個程式碼範例中擷取的 **MediaFrameSourceGroup**。

如需您可搭配 **MediaCapture** 執行的其他作業以及擷取媒體的其他 UWP app 功能的相關資訊，請參閱[相機](camera.md)。

[!code-cs[MultiRecordInitMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordInitMediaCapture)]

## <a name="create-a-mediaencodingprofile"></a>建立 MediaEncodingProfile
**[MediaEncodingProfile](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile)** 類別會告知媒體擷取管線在將擷取到的音訊和視訊寫入檔案時，應該如何編碼。 對於一般擷取和轉碼案例，此類別提供一組靜態方法來建立常見的設定檔，例如 **[CreateAvi](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateAvi_Windows_Media_MediaProperties_VideoEncodingQuality_)** 和 **[CreateMp3](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateMp3_Windows_Media_MediaProperties_AudioEncodingQuality_)**。 針對此範例，將使用 Mpeg4 容器和 H264 視訊編碼來手動建立編碼設定檔。 使用 **[VideoEncodingProperties](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.videoencodingproperties)** 物件來指定視訊編碼設定。 針對本案例中使用的每部彩色相機，會設定 **VideoStreamDescriptor** 物件。 使用 **VideoEncodingProperties** 物件建構描述元，指定編碼。 **VideoStreamDescriptor** 的 **[Label](https://docs.microsoft.com/uwp/api/windows.media.core.videostreamdescriptor#Windows_Media_Core_VideoStreamDescriptor_Label)** 屬性必須設成要擷取至串流的媒體畫面來源的識別碼。 擷取管線透過此方法得知哪一個串流描述元與編碼屬性應用於每個相機。 畫面來源的識別碼由 **[MediaFrameSourceInfo](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo)** 物件公開，選取 **MediaFrameSourceGroup** 時可在上一區段找到此物件。


從 RS3 開始，您可以透過呼叫 **[SetVideoTracks](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile#Windows_Media_MediaProperties_MediaEncodingProfile_SetVideoTracks_Windows_Foundation_Collections_IIterable_Windows_Media_Core_VideoStreamDescriptor__)**，在 **MediaEncodingProfile** 上設定多個編碼屬性。 您可以呼叫 **[GetVideoTracks](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile#Windows_Media_MediaProperties_MediaEncodingProfile_GetVideoTracks)** 來擷取視訊串流描述元的清單。 請注意，如果您設定 **[Video](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile#Windows_Media_MediaProperties_MediaEncodingProfile_Video)** 屬性 (用於儲存單一串流描述元)，則您透過呼叫 **SetVideoTracks** 設定的描述元清單會更換為包含您所指定之單一描述元的清單。


[!code-cs[MultiRecordMediaEncodingProfile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordMediaEncodingProfile)]

## <a name="record-using-the-multi-stream-mediaencodingprofile"></a>使用多串流 MediaEncodingProfile 錄製
此範例的最後一個步驟是透過呼叫 **[StartRecordToStorageFileAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture#Windows_Media_Capture_MediaCapture_StartRecordToStorageFileAsync_Windows_Media_MediaProperties_MediaEncodingProfile_Windows_Storage_IStorageFile_)**、傳遞 **StorageFile** 至所擷取媒體的寫入目的，以及在前一個程式碼範例中建立的 **MediaEncodingProfile**，來起始視訊擷取功能。 等候幾秒後，呼叫 **[StopRecordAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture#Windows_Media_Capture_MediaCapture_StopRecordAsync)** 來停止錄製。

[!code-cs[MultiRecordToFile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordToFile)]

作業完成時，會建立視訊檔案，其中包含從每部相機擷取的視訊，並編碼為檔案中的個別串流。 如需播放包含多個視訊播放軌之媒體檔案的相關資訊，請參閱[媒體項目、播放清單與曲目](media-playback-with-mediasource.md)。

## <a name="related-topics"></a>相關主題

* [相機](camera.md)
* [使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [使用 MediaFrameReader 處理媒體畫面](process-media-frames-with-mediaframereader.md)
* [媒體項目、播放清單與曲目](media-playback-with-mediasource.md)


 

 



