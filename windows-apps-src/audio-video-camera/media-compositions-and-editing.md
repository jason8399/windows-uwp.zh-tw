---
ms.assetid: C4DB495D-1F91-40EF-A55C-5CABBF3269A2
description: Windows.Media.Editing 命名空間中的 API 可讓您快速開發 app，讓使用者從音訊和視訊來源檔案建立媒體組合。
title: 媒體組合和編輯
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 6a0b21ac4c2bc1a2278757cdaa542be39c01f481
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318258"
---
# <a name="media-compositions-and-editing"></a>媒體組合和編輯



本文向您說明如何使用 [**Windows.Media.Editing**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing) 命名空間中的 API 來快速開發 app，讓使用者從音訊和視訊來源檔案建立媒體組合。 架構的功能包括能夠以程式設計的方式一起新增多個視訊剪輯、新增視訊與影像重疊、新增背景音訊，以及套用音訊與視訊效果。 建立之後，媒體組合可以轉譯為一般媒體檔案來播放或共用，但是組合也可以序列化至磁碟和從磁碟還原序列化，允許使用者載入和修改他們之前建立的組合。 這項功能是以方便使用的 Windows 執行階段介面提供，相較於低階 [Microsoft 媒體基礎](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk) API時，大幅減少執行這些工作所需之程式碼的數量和複雜度。

## <a name="create-a-new-media-composition"></a>建立新的媒體組合

[  **MediaComposition**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaComposition) 類別是適用於所有媒體剪輯的容器，這些媒體剪輯組成組合，負責轉譯最終組合、將組合載入和儲存到光碟，以及提供組合的預覽串流，讓使用者可以在 UI 中檢視。 若要在您的 app 中使用 **MediaComposition**，請包含 [**Windows.Media.Editing**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing) 命名空間以及 [**Windows.Media.Core**](https://docs.microsoft.com/uwp/api/Windows.Media.Core) 命名空間，該命名空間提供您需要的相關 API。

[!code-cs[Namespace1](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetNamespace1)]


將會從您的程式碼的多個點存取 **MediaComposition** 物件，因此通常您會宣告其中的成員變數以儲存它。

[!code-cs[DeclareMediaComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetDeclareMediaComposition)]

**MediaComposition** 的建構函式沒有引數。

[!code-cs[MediaCompositionConstructor](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetMediaCompositionConstructor)]

## <a name="add-media-clips-to-a-composition"></a>將媒體剪輯新增至組合

媒體組合通常包含一或多個視訊剪輯。 您可以使用 [**FileOpenPicker**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-fileopenpicker) 以允許使用者選取視訊檔案。 一旦選取檔案，建立新的 [**MediaClip**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaClip) 物件以包含視訊剪輯，方法是呼叫 [**MediaClip.CreateFromFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.createfromfileasync)。 接著您將剪輯新增至 **MediaComposition** 物件的 [**Clips**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.clips) 清單。

[!code-cs[PickFileAndAddClip](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetPickFileAndAddClip)]

-   媒體剪輯會以在 [**Clips**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.clips) 清單中顯示的相同順序出現在 **MediaComposition** 中。

-   **MediaClip** 只能併入組合一次。 嘗試新增已由組合使用的 **MediaClip** 會導致錯誤。 若要在組合中重複使用視訊剪輯多次，請呼叫 [**Clone**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.clone) 以建立稍後可以新增至組合的新 **MediaClip** 物件。

-   通用 Windows app 沒有權限可以存取整個檔案系統。 [  **StorageApplicationPermissions**](https://docs.microsoft.com/uwp/api/Windows.Storage.AccessCache.StorageApplicationPermissions) 類別的 [**FutureAccessList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) 屬性可以讓您的 app 儲存檔案的記錄，該檔案已由使用者選取，所以您可以保留存取檔案的權限。 **FutureAccessList** 具有最多的 1000 個項目，所以您的 app 需要管理清單以確定不會變滿。 如果您計劃支援載入和修改之前建立的組合，這特別重要。

-   **MediaComposition** 支援 MP4 格式的視訊剪輯。

-   如果視訊檔案包含多個內嵌的曲目，您可以藉由設定 [**SelectedEmbeddedAudioTrackIndex**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.selectedembeddedaudiotrackindex) 屬性，選取要在組合中使用哪個曲目。

-   建立單一色彩填滿整個框架的 **MediaClip**，方法是呼叫 [**CreateFromColor**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.createfromcolor) 以及指定色彩和剪輯的持續時間。

-   從影像檔建立 **MediaClip**，方法是呼叫 [**CreateFromImageFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.createfromimagefileasync) 以及指定影像檔和剪輯的持續時間。

-   從 [**IDirect3DSurface**](https://docs.microsoft.com/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) 建立 **MediaClip**，方法是呼叫 [**CreateFromSurface**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.createfromsurface) 以及指定表面和剪輯的持續時間。

## <a name="preview-the-composition-in-a-mediaelement"></a>預覽 MediaElement 中的組合

若要讓使用者檢視媒體組合，請將 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) 新增至定義您 UI 的 XAML 檔案。

[!code-xml[MediaElement](./code/MediaEditing/cs/MainPage.xaml#SnippetMediaElement)]

宣告類型 [**MediaStreamSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaStreamSource) 的成員變數。


[!code-cs[DeclareMediaStreamSource](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetDeclareMediaStreamSource)]

呼叫 **MediaComposition** 物件的 [**GeneratePreviewMediaStreamSource**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.generatepreviewmediastreamsource) 方法來建立組合的 **MediaStreamSource**。 透過呼叫 Factory 方法 [**CreateFromMediaStreamSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediastreamsource)，並將它指派給 **MediaPlayerElement** 的 [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) 參數，來建立 [**MediaSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource) 物件。 現在可以在 UI 中檢視組合。


[!code-cs[UpdateMediaElementSource](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetUpdateMediaElementSource)]

-   **MediaComposition** 必須包含至少一個媒體剪輯，然後才能呼叫 [**GeneratePreviewMediaStreamSource**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.generatepreviewmediastreamsource)，否則傳回的物件將會是 Null。

-   **MediaElement** 時間軸不會自動更新以反映組合中的變更。 建議您呼叫兩者**GeneratePreviewMediaStreamSource**並設定**MediaPlayerElement** **來源**每次您進行的變更集的屬性撰寫，而且想来更新 UI。

當使用者離開頁面以釋放相關聯的資源時，建議您將 **MediaStreamSource** 物件和 **MediaPlayerElement** 的 [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaelement.source) 屬性設為 Null。

[!code-cs[OnNavigatedFrom](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

## <a name="render-the-composition-to-a-video-file"></a>將組合轉譯為視訊檔案

若要將媒體組合轉譯為一般視訊檔案，以便共用及在其他裝置上檢視，您需要使用 [**Windows.Media.Transcoding**](https://docs.microsoft.com/uwp/api/Windows.Media.Transcoding) 命名空間的 API。 若要更新非同步作業進度的 UI，您也需要 [**Windows.UI.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core) 命名空間的 API。

[!code-cs[Namespace2](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetNamespace2)]

在允許使用者使用 [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker) 選取輸出檔案之後，藉由呼叫 **MediaComposition** 物件的 [**RenderToFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.rendertofileasync)，將組合轉譯為選取的檔案。 下列範例中的其他程式碼只是遵循處理 [**AsyncOperationWithProgress**](https://docs.microsoft.com/previous-versions/br205807(v=vs.85)) 的模式。

[!code-cs[RenderCompositionToFile](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetRenderCompositionToFile)]

-   [  **MediaTrimmingPreference**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaTrimmingPreference) 可讓您設定轉碼作業速度與修剪相鄰媒體剪輯的精確度的優先順序。 **Fast** 會讓轉碼速度較快而修剪精確度較低，**Precise** 會讓轉碼較慢而修剪精確度較高。

## <a name="trim-a-video-clip"></a>修剪視訊剪輯

修剪組合中視訊剪輯的持續時間，方法是設定 [**MediaClip**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaClip) 物件的 [**TrimTimeFromStart**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.trimtimefromstart) 屬性、[**TrimTimeFromEnd**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.trimtimefromend) 屬性，或兩者同時設定。

[!code-cs[TrimClipBeforeCurrentPosition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetTrimClipBeforeCurrentPosition)]

-   您可以使用任何您想要的 UI 讓使用者指定開始和結束修剪值。 上述範例使用與 **MediaPlayerElement** 相關聯之 [**MediaPlaybackSession**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackSession) 的 [**Position**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.position) 屬性，藉由檢查 [**StartTimeInComposition**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.starttimeincomposition) 和 [**EndTimeInComposition**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.endtimeincomposition)，先決定要在組合中的目前位置播放哪個 **MediaClip**。 然後再次使用 **Position** 和 **StartTimeInComposition** 屬性，計算從剪輯開頭修剪的時間量。 **FirstOrDefault** 方法是 **System.Linq** 命名空間的擴充方法，可簡化從清單中選取項目的程式碼。
-   **MediaClip** 物件的 [**OriginalDuration**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.originalduration) 屬性可讓您知道未套用任何剪輯的媒體剪輯的持續時間。
-   [  **TrimmedDuration**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.trimmedduration) 屬性可讓您知道套用修剪之後的媒體剪輯的持續時間。
-   指定大於媒體剪輯原始持續時間的修剪值不會擲回錯誤。 不過，如果組合只包含單一剪輯，並且藉由指定大的修剪值而修剪為零長度，[**GeneratePreviewMediaStreamSource**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.generatepreviewmediastreamsource) 的後續呼叫會傳回 Null，如同組合沒有任何剪輯。

## <a name="add-a-background-audio-track-to-a-composition"></a>將背景曲目新增至組合

若要將背景曲目新增至組合，載入音訊檔案然後建立 [**BackgroundAudioTrack**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.BackgroundAudioTrack) 物件，方法是呼叫 Factory 方法 [**BackgroundAudioTrack.CreateFromFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.editing.backgroundaudiotrack.createfromfileasync)。 接著將 **BackgroundAudioTrack** 新增至組合的 [**BackgroundAudioTracks**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.backgroundaudiotracks) 屬性。

[!code-cs[AddBackgroundAudioTrack](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddBackgroundAudioTrack)]

-   A **MediaComposition**支援背景音訊資料軌，格式如下：MP3，WAV FLAC

-   背景曲目

-   如同使用視訊檔案，您應該使用 [**StorageApplicationPermissions**](https://docs.microsoft.com/uwp/api/Windows.Storage.AccessCache.StorageApplicationPermissions) 類別來保留組合中檔案的存取權。

-   如同使用 **MediaClip**，**BackgroundAudioTrack** 只能併入組合一次。 嘗試新增已由組合使用的 **BackgroundAudioTrack** 會導致錯誤。 若要在組合中重複使用曲目多次，請呼叫 [**Clone**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.clone) 以建立稍後可以新增至組合的新 **MediaClip** 物件。

-   根據預設，背景曲目會在組合的開頭開始播放。 如果有多個背景曲目存在，所有曲目都會在組合的開頭開始播放。 若要讓背景曲目在其他時間開始播放，請將 [**Delay**](https://docs.microsoft.com/uwp/api/windows.media.editing.backgroundaudiotrack.delay) 屬性設為想要的時間起始位移。

## <a name="add-an-overlay-to-a-composition"></a>將重疊新增至組合

重疊可讓您在組合中每個視訊層上方堆疊多個視訊層。 組合可以包含多個重疊層，每一個可以包含多個重疊。 將 **MediaClip** 傳遞到其建構函式，以建立 [**MediaOverlay**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaOverlay) 物件。 設定重疊的位置和不透明度，然後建立新的 [**MediaOverlayLayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaOverlayLayer)，並將 **MediaOverlay** 新增至其 [**Overlays**](https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgioutput2-supportsoverlays) 清單。 最後，將 **MediaOverlayLayer** 新增至組合的 [**OverlayLayers**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.overlaylayers) 清單。

[!code-cs[AddOverlay](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddOverlay)]

-   視訊層內的重疊是根據其包含層之 **Overlays** 清單的順序進行 z 排序。 清單中具有較高索引的項目會轉譯在較低索引項目的上方。 組合中的重疊層也是如此。 在組合的 **OverlayLayers** 清單中具有較高索引的視訊層會轉譯在較低索引視訊層的上方。

-   因為重疊是堆疊在彼此上方，而不是順序播放，所以所有重疊預設會在組合的開頭開始播放。 若要讓重疊在其他時間開始播放，請將 [**Delay**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaoverlay.delay) 屬性設為想要的時間起始位移。

## <a name="add-effects-to-a-media-clip"></a>將效果新增至媒體剪輯

組合中的每個 **MediaClip** 都有一份可以加入多個效果的音訊與視訊效果清單。 效果必須分別實作 [**IAudioEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.IAudioEffectDefinition) 和 [**IVideoEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.IVideoEffectDefinition)。 下列範例會使用目前的 **MediaPlayerElement** 位置以選擇目前檢視的 **MediaClip**，然後建立 [**VideoStabilizationEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.VideoStabilizationEffectDefinition) 的新執行個體，並將它附加到媒體剪輯的 [**VideoEffectDefinitions**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.videoeffectdefinitions) 清單。

[!code-cs[AddVideoEffect](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddVideoEffect)]

## <a name="save-a-composition-to-a-file"></a>將組合儲存至檔案

媒體組合稍後可以序列化至檔案以進行修改。 挑選輸出檔案，然後呼叫 [**MediaComposition**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaComposition) 方法 [**SaveAsync**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.saveasync) 以儲存組合。

[!code-cs[SaveComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetSaveComposition)]

## <a name="load-a-composition-from-a-file"></a>從檔案載入組合

可以從檔案還原序列化媒體組合，讓使用者檢視和修改組合。 挑選組合檔案，然後呼叫 [**MediaComposition**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaComposition) 方法 [**LoadAsync**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.loadasync) 以載入組合。

[!code-cs[OpenComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetOpenComposition)]

-   如果組合中的媒體檔案不是在您 app 可存取的位置，且不是在 app 的 [**StorageApplicationPermissions**](https://docs.microsoft.com/uwp/api/Windows.Storage.AccessCache.StorageApplicationPermissions) 類別 [**FutureAccessList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) 屬性中，則在載入組合時會擲回錯誤。

 

 




