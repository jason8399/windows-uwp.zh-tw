---
ms.assetid: EFCF84D0-2F4C-454D-97DA-249E9EAA806C
description: SystemMediaTransportControls 類別可讓您的 App 使用內建於 Windows 的系統媒體傳輸控制項，以及更新控制項顯示您 App 目前正在播放之媒體的相關中繼資料。
title: 系統媒體傳輸控制項的手動控制項
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0a4163105b934f5c1e2970fab9f51b76d69d1bd8
ms.sourcegitcommit: c95915f8a13736705eab74951a12b2cf528ea612
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876229"
---
# <a name="manual-control-of-the-system-media-transport-controls"></a>系統媒體傳輸控制項的手動控制項


從 Windows 10 版本 1607 開始，使用 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) 類別來播放媒體的 UWP App，預設將會自動與系統媒體傳輸控制項 (SMTC) 整合。 針對大部分的案例，這是與 SMTC 互動的建議方式。 如需自訂 SMTC 與 **MediaPlayer** 預設整合的詳細資訊，請參閱[與系統媒體傳輸控制項整合](integrate-with-systemmediatransportcontrols.md)。

在某幾個案例中，您可能需要實作 SMTC 的手動控制項。 這包括使用 [**MediaTimelineController**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaTimelineController) 來控制一或多個媒體播放程式之播放的案例。 或是會使用多個媒體播放程式，但針對 App 只想要單一 SMTC 執行個體的案例。 如果您是使用 [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) 來播放媒體，便必須手動控制 SMTC。

## <a name="set-up-transport-controls"></a>設定傳輸控制項
如果您是使用 **MediaPlayer** 來播放媒體，您可以透過存取 [**MediaPlayer.SystemMediaTransportControls**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.systemmediatransportcontrols) 屬性來取得 [**SystemMediaTransportControls**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControls) 類別的執行個體。 如果您要手動控制 SMTC，您應該停用由 **MediaPlayer** 所提供的自動整合，方法是將 [**CommandManager.IsEnabled**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled) 屬性設定為 false。

> [!NOTE] 
> 如果您透過將 [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled) 設定為 false 來停用 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) 的 [**MediaPlaybackCommandManager**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager)，它將會破壞 **MediaPlayer** 和由 **MediaPlayerElement** 所提供的 [**TransportControls**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.transportcontrols) 之間的連結，使內建傳輸控制項無法繼續自動控制播放器的播放。 您必須改為實作自己的控制項以控制 **MediaPlayer**。

[!code-cs[InitSMTCMediaPlayer](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetInitSMTCMediaPlayer)]

您也可以透過呼叫 [**GetForCurrentView**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.getforcurrentview) 以取得 [**SystemMediaTransportControls**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControls) 的執行個體。 如果您是使用 **MediaElement** 來播放媒體，您必須透過此方法來取得該物件。

[!code-cs[InitSMTCMediaElement](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetInitSMTCMediaElement)]

啟用您的 App 將會使用的按鈕，方法是設定 **SystemMediaTransportControls** 物件的相對應「is enabled」屬性，例如 [**IsPlayEnabled**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.isplayenabled)、[**IsPauseEnabled**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.ispauseenabled)、[**IsNextEnabled**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.isnextenabled) 及 [**IsPreviousEnabled**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.ispreviousenabled)。 請參閱 **SystemMediaTransportControls** 參考文件，以取得可用控制項的完整清單。

[!code-cs[EnableContols](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetEnableContols)]

註冊 [**ButtonPressed**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed) 事件的處理常式，以在使用者按下按鈕時接收通知。

[!code-cs[RegisterButtonPressed](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetRegisterButtonPressed)]

## <a name="handle-system-media-transport-controls-button-presses"></a>處理系統媒體傳輸控制項按鈕的按下動作

當某個啟用的按鈕被按下時，系統傳輸控制項會引發 [**ButtonPressed**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed) 事件。 傳入事件處理常式的 [**SystemMediaTransportControlsButtonPressedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControlsButtonPressedEventArgs) 的 [**Button**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrolsbuttonpressedeventargs.button) 屬性，是 [**SystemMediaTransportControlsButton**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControlsButton) 列舉的成員，該列舉表示已按下哪個已啟用的按鈕。

若要從 [**ButtonPressed**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed) 事件處理常式更新 UI 執行緒上的物件 (如 [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) 物件)，您必須透過 [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) 為呼叫進行封送處理。 這是因為 **ButtonPressed** 事件處理常式不是從 UI 執行緒呼叫，因此如果您嘗試直接修改 UI 將會擲回例外狀況。

[!code-cs[SystemMediaTransportControlsButtonPressed](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsButtonPressed)]

## <a name="update-the-system-media-transport-controls-with-the-current-media-status"></a>以目前的媒體狀態更新系統媒體傳輸控制項

當媒體狀態已變更時，您應該通知 [**SystemMediaTransportControls**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControls)，以便讓系統可以更新控制項以反映目前狀態。 若要這樣做，請從 [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) 的 [**CurrentStateChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaelement.currentstatechanged) 事件 (當媒體狀態變更時引發)，將 [**PlaybackStatus**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.playbackstatus) 屬性設為適當的 [**MediaPlaybackStatus**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaPlaybackStatus) 值。

[!code-cs[SystemMediaTransportControlsStateChange](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsStateChange)]

## <a name="update-the-system-media-transport-controls-with-media-info-and-thumbnails"></a>以媒體資訊和縮圖更新系統媒體傳輸控制項

您可以使用 [**SystemMediaTransportControlsDisplayUpdater**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControlsDisplayUpdater) 類別來更新傳輸控制項顯示的媒體資訊，例如，目前播放媒體項目的歌曲標題或專輯封面。 使用 [**SystemMediaTransportControls.DisplayUpdater**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.displayupdater) 屬性取得這個類別的執行個體。 對於典型的案例，傳遞中繼資料的建議方式是呼叫 [**CopyFromFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.copyfromfileasync)，傳入目前播放媒體檔案。 顯示更新程式會自動從檔案擷取中繼資料和縮圖影像。

呼叫 [**Update**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.update) 會造成系統媒體傳輸控制項使用新的中繼資料與縮圖更新它的 UI。

[!code-cs[SystemMediaTransportControlsUpdater](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsUpdater)]

如果您的案例需要，您可以手動更新系統媒體傳輸控制項顯示的中繼資料，方法是設定 [**DisplayUpdater**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.displayupdater) 類別公開之 [**MusicProperties**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.musicproperties)、[**ImageProperties**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.imageproperties) 或 [**VideoProperties**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.videoproperties) 物件的值。

[!code-cs[SystemMediaTransportControlsUpdaterManual](./code/SMTCWin10/cs/MainPage.xaml.cs#SystemMediaTransportControlsUpdaterManual)]

> [!Note]
> 應用程式應設定[SystemMediaTransportControlsDisplayUpdater](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.type#Windows_Media_SystemMediaTransportControlsDisplayUpdater_Type
)屬性的值，即使它們未提供系統媒體傳輸控制項所要顯示的其他媒體中繼資料也一樣。 此值可協助系統正確地處理您的媒體內容，包括防止螢幕保護裝置在播放期間啟用。


## <a name="update-the-system-media-transport-controls-timeline-properties"></a>更新系統媒體傳輸控制項時間軸屬性

系統傳輸控制項會顯示目前播放媒體項目之時間軸的相關資訊，包括媒體項目的目前播放位置、開始時間及結束時間。 若要更新系統傳輸控制項時間軸屬性，請建立新的 [**SystemMediaTransportControlsTimelineProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControlsTimelineProperties) 物件。 設定物件的屬性以反映正在播放的媒體項目的目前狀態。 呼叫 [**SystemMediaTransportControls.UpdateTimelineProperties**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.updatetimelineproperties) 讓控制項更新時間軸。

[!code-cs[UpdateTimelineProperties](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetUpdateTimelineProperties)]

-   您必須提供 [**StartTime**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.starttime)、[**EndTime**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.endtime) 和 [**Position**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.playbackpositionchangerequested) 的值，系統控制項才能為您正在播放的項目顯示時間軸。

-   [**MinSeekTime**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.minseektime)和[**MaxSeekTime**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.maxseektime)可讓您指定時間軸內使用者可以搜尋的範圍。 典型的案例是讓內容提供者在他們的媒體中包含廣告中斷。

    您必須設定 [**MinSeekTime**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.minseektime) 和 [**MaxSeekTime**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.maxseektime)，才會引發 [**PositionChangeRequest**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.playbackpositionchangerequested)。

-   建議您讓系統控制項與您的媒體播放保持同步，方法是在播放期間大約每 5 秒更新這些屬性，以及每當播放的狀態變更時更新，例如暫停或搜尋新的位置。

## <a name="respond-to-player-property-changes"></a>回應播放程式屬性變更

有一組系統傳輸控制項屬性與媒體播放程式本身的目前狀態相關，而不是與正在播放的媒體項目狀態相關。 每個屬性與使用者調整相關聯控制項時所引發的事件相符。 這些屬性與事件包括：

| 屬性                                                                  | Event - 事件                                                                                                   |
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| [**AutoRepeatMode**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.autorepeatmode) | [**AutoRepeatModeChangeRequested**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.autorepeatmodechangerequested) |
| [**PlaybackRate**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.playbackrate)     | [**PlaybackRateChangeRequested**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.playbackratechangerequested)     |
| [**ShuffleEnabled**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.shuffleenabled) | [**ShuffleEnabledChangeRequested**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.shuffleenabledchangerequested) |

 
若要處理使用者與其中一個控制項的互動，請先註冊相關聯事件的處理常式。

[!code-cs[RegisterPlaybackChangedHandler](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetRegisterPlaybackChangedHandler)]

在事件處理常式中，請先確定要求的值是在有效且預期的範圍內。 如果是的話，設定 [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) 的對應屬性，然後設定 [**SystemMediaTransportControls**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControls) 物件的對應屬性。

[!code-cs[PlaybackChangedHandler](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetPlaybackChangedHandler)]

-   為了引發其中一個播放程式屬性事件，您必須設定屬性的初始值。 例如，[**PlaybackRateChangeRequested**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.playbackratechangerequested) 不會引發，直到您設定 [**PlaybackRate**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.playbackrate) 屬性的值至少一次之後。

## <a name="use-the-system-media-transport-controls-for-background-audio"></a>針對背景音訊使用系統媒體傳輸控制項

如果您沒有使用由 **MediaPlayer** 所提供的自動 SMTC 整合，您必須手動與 SMTC 整合以啟用背景音訊。 您的 App 至少必須將 [**IsPlayEnabled**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.isplayenabled) 和 [**IsPauseEnabled**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.ispauseenabled) 設定成 true 來啟用播放和暫停按鈕。 您的 App 還必須處理 [**ButtonPressed**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed) 事件。 如果您的 App 沒有符合這些需求，音訊播放將會在 App 移至背景時停止。

針對背景音訊使用新的單處理程序模型的 App，應該能透過呼叫 [**GetForCurrentView**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.getforcurrentview) 取得 [**SystemMediaTransportControls**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControls) 的執行個體。 針對背景音訊使用舊版的雙處理程序模型 的 App，必須使用 [**BackgroundMediaPlayer.Current.SystemMediaTransportControls**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.systemmediatransportcontrols) 以取得從背景處理程序存取 SMTC 的權限。

如需有關在背景播放音訊的詳細資訊，請參閱[在背景播放媒體](background-audio.md)。

## <a name="related-topics"></a>相關主題
* [媒體播放](media-playback.md)
* [與系統媒體傳輸控制整合](integrate-with-systemmediatransportcontrols.md) 
* [系統媒體 Tranport 範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls) 

 




