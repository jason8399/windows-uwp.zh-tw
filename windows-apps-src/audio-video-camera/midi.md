---
ms.assetid: 9146212C-8480-4C16-B74C-D7F08C7086AF
description: 本文章示範如何列舉 MIDI (樂器數位介面) 裝置，並且從通用 Windows app 傳送及接收 MIDI 訊息。
title: MIDI
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 73806735401f53a73b1051f37c72119b45b574be
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318284"
---
# <a name="midi"></a>MIDI



本文章示範如何列舉 MIDI (樂器數位介面) 裝置，並且從通用 Windows app 傳送及接收 MIDI 訊息。 Windows 10 透過 USB （類別相容，以及最專屬驅動程式），透過藍牙 LE 的 MIDI 支援 MIDI (Windows 10 Anniversary Edition 和更新版本)，以及透過免費提供第三方產品、 透過乙太網路的 MIDI 和路由的 MIDI。

## <a name="enumerate-midi-devices"></a>列舉 MIDI 裝置

列舉和使用 MIDI 裝置之前，請將下列命名空間新增至您的專案。

[!code-cs[Using](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetUsing)]

將 [**ListBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox) 控制項新增至您的 XAML 頁面，讓使用者選取其中一個要附加至系統的 MIDI 輸入裝置。 新增另一個以列出 MIDI 輸出裝置。

[!code-xml[MidiListBoxes](./code/MIDIWin10/cs/MainPage.xaml#SnippetMidiListBoxes)]

[  **FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) 方法 [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 類別是用來列舉 Windows 可辨識的許多不同類型的裝置。 若要指定您只想要方法可尋找 MIDI 輸入裝置，請使用 [**MidiInPort.GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.midi.midiinport.getdeviceselector) 傳回的選取器字串。 **FindAllAsync** 會傳回 [**DeviceInformationCollection**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationCollection)，包含向系統登錄之每個 MIDI 輸入裝置的 **DeviceInformation**。 如果傳回的集合不包含任何項目，則沒有可用的 MIDI 輸入裝置。 如果集合中有項目，循環顯示 **DeviceInformation** 物件並且將每個裝置的名稱新增至 MIDI 輸入裝置 **ListBox**。

[!code-cs[EnumerateMidiInputDevices](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetEnumerateMidiInputDevices)]

列舉 MIDI 輸出裝置的運作方式與列舉輸入裝置的方式完全相同，不同的是您應該指定在呼叫 **FindAllAsync** 時 [**MidiOutPort.GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.midi.midioutport.getdeviceselector) 傳回的選取器字串。

[!code-cs[EnumerateMidiOutputDevices](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetEnumerateMidiOutputDevices)]



## <a name="create-a-device-watcher-helper-class"></a>建立裝置監控程式協助程式類別

[  **Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) 命名空間提供 [**DeviceWatcher**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)，它可以在系統中新增或移除裝置時，或裝置的資訊更新時，通知您的 app。 因為已啟用 MIDI 的 app 通常會想要輸入和輸出裝置，這個範例會建立實作 **DeviceWatcher** 的協助程式類別，以便相同的程式碼可以用於 MIDI 輸入和 MIDI 輸出裝置，而不需要重複。

將新的類別新增至您的專案做為裝置監控程式。 在此範例中，類別名為 **MyMidiDeviceWatcher**。 在本節中的其餘程式碼是用來實作協助程式類別。

將部分成員變數新增至類別：

-   [  **DeviceWatcher**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) 物件，該物件會監視裝置變更。
-   裝置選取器字串，它將針對一個執行個體包含 MIDI 輸入連接埠選取器字串，針對另一個執行個體包含 MIDI 輸出連接埠選取器字串。
-   [  **ListBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox) 控制項，該控制項會填入可用裝置的名稱。
-   [  **CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher)，這是從 UI 執行緒以外的執行緒更新 UI 的必要項目。

[!code-cs[WatcherVariables](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherVariables)]

新增 [**DeviceInformationCollection**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationCollection) 屬性，該屬性是用來存取來自協助程式類別外部的目前裝置清單。

[!code-cs[DeclareDeviceInformationCollection](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetDeclareDeviceInformationCollection)]

在類別建構函式中，呼叫者傳入 MIDI 裝置選取器字串，用來列出裝置的 **ListBox**，和更新 UI 所需的 **Dispatcher**。

呼叫 [**DeviceInformation.CreateWatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher) 以建立 **DeviceWatcher** 類別的新執行個體，傳入 MIDI 裝置選取器字串。

針對監控程式的事件處理常式登錄處理常式。

[!code-cs[WatcherConstructor](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherConstructor)]

**DeviceWatcher** 具有下列事件：

-   [**新增**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added) -新的裝置新增至系統時引發。
-   [**移除**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.removed) -從系統移除裝置時所引發。
-   [**更新**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated) -引發時更新現有的裝置相關聯的資訊。
-   [**EnumerationCompleted** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.enumerationcompleted) -監看員已完成要求的裝置類型的列舉型別時引發。

在每個事件的事件處理常式中，會呼叫協助程式方法 **UpdateDevices**，以使用目前的裝置清單更新 **ListBox**。 因為 **UpdateDevices** 更新 UI 元素與這些事件處理常式不是在 UI 執行緒上呼叫，每個呼叫必須包裝於對 [**RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) 的呼叫中，這會使指定的程式碼在 UI 執行緒上執行。

[!code-cs[WatcherEventHandlers](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherEventHandlers)]

**UpdateDevices** 協助程式方法會呼叫 [**DeviceInformation.FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)，以及使用本文先前所述的已傳回裝置的名稱更新 **ListBox**。

[!code-cs[WatcherUpdateDevices](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherUpdateDevices)]

使用 **DeviceWatcher** 物件的 [**Start**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.start) 方法，新增方法以啟動監控程式，以及使用 [**Stop**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.stop) 方法，停止監控程式。

[!code-cs[WatcherStopStart](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherStopStart)]

提供解構函式以取消註冊監控程式事件處理常式，並且將裝置監控程式設為 Null。

[!code-cs[WatcherDestructor](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherDestructor)]

## <a name="create-midi-ports-to-send-and-receive-messages"></a>建立 MIDI 連接埠以傳送和接收訊息

在您的頁面背後的程式碼中，宣告成員變數來保存 **MyMidiDeviceWatcher** 協助程式類別的兩個執行個體，一個用於輸入裝置，另一個用於輸出裝置。

[!code-cs[DeclareDeviceWatchers](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetDeclareDeviceWatchers)]

建立監控程式協助程式類別的新執行個體，傳入裝置選取器字串、要填入的 **ListBox** 和可以透過頁面的 **Dispatcher** 屬性存取的 **CoreDispatcher** 物件。 然後，呼叫方法以啟動每個物件的 **DeviceWatcher**。

在啟動每個 **DeviceWatcher** 之後不久，它會完成列舉連接到系統的目前裝置，並且引發其 [**EnumerationCompleted**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.enumerationcompleted) 事件，這會導致每個 **ListBox** 以目前的 MIDI 裝置進行更新。

[!code-cs[StartWatchers](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetStartWatchers)]

當使用者在 MIDI 輸入 **ListBox** 中選取項目時，會引發 [**SelectionChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) 事件。 在這個事件的處理常式中，存取協助程式類別的 **DeviceInformationCollection** 屬性以取得目前的裝置清單。 如果清單中有項目，選取具有對應至 **ListBox** 控制項的 [**SelectedIndex**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex) 之索引的 **DeviceInformation** 物件。

建立 [**MidiInPort**](https://docs.microsoft.com/uwp/api/Windows.Devices.Midi.MidiInPort) 物件，代表選取的輸入裝置，方法是呼叫 [**MidiInPort.FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.midi.midiinport.fromidasync)，傳入選取的裝置的 [**Id**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) 屬性。

登錄 [**MessageReceived**](https://docs.microsoft.com/uwp/api/windows.devices.midi.midiinport.messagereceived) 事件的處理常式，它會在每當透過指定的裝置接收 MIDI 訊息時引發。

[!code-cs[DeclareMidiPorts](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetDeclareMidiPorts)]

[!code-cs[InPortSelectionChanged](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetInPortSelectionChanged)]

當呼叫 **MessageReceived** 處理常式時，訊息會包含在 **MidiMessageReceivedEventArgs** 的 [**Message**](https://docs.microsoft.com/uwp/api/Windows.Devices.Midi.MidiMessageReceivedEventArgs) 屬性中。 訊息物件的 [**Type**](https://docs.microsoft.com/uwp/api/windows.devices.midi.imidimessage.type) 是來自 [**MidiMessageType**](https://docs.microsoft.com/uwp/api/Windows.Devices.Midi.MidiMessageType) 列舉的值，指出已收到之訊息的類型。 訊息的資料取決於訊息類型。 這個範例會查看訊息是否為訊息的附註，若是如此，會輸出 midi 通道、附註和訊息的速度。

[!code-cs[MessageReceived](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetMessageReceived)]

輸出裝置 **ListBox** 的 [**SelectionChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) 處理常式的運作方式與輸入裝置的處理常式運作方式相同，不同的是不會註冊事件處理常式。

[!code-cs[OutPortSelectionChanged](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetOutPortSelectionChanged)]

建立輸出裝置之後，您可以傳送訊息，方法是針對您要傳送的訊息類型建立新的 [**IMidiMessage**](https://docs.microsoft.com/uwp/api/Windows.Devices.Midi.IMidiMessage)。 在此範例中，訊息是 [**NoteOnMessage**](https://docs.microsoft.com/uwp/api/Windows.Devices.Midi.MidiNoteOnMessage)。 會呼叫 [**IMidiOutPort**](https://docs.microsoft.com/uwp/api/Windows.Devices.Midi.IMidiOutPort) 物件的 [**SendMessage**](https://docs.microsoft.com/uwp/api/windows.devices.midi.imidioutport.sendmessage) 方法以傳送訊息。

[!code-cs[SendMessage](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetSendMessage)]

當您的 app 停用時，請務必清除您的 app 資源。 取消註冊事件處理常式並且將 MIDI 輸入連接埠和輸出連接埠物件設為 Null。 停止裝置監控程式並且將它們設為 Null。

[!code-cs[CleanUp](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetCleanUp)]

## <a name="using-the-built-in-windows-general-midi-synth"></a>使用內建的 Windows General MIDI 合成

當您使用上述的技術列舉輸出 MIDI 裝置時，您的 app 將會探索名為「Microsoft GS Wavetable Synth」的 MIDI 裝置。 這是您可以用來從 app 播放的內建 General MIDI 合成器。 不過，除非您已經在專案中包含內建合成的 SDK 擴充功能，否則嘗試在此裝置建立 MIDI 輸出將會失敗。

**若要在應用程式專案中包含一般的 MIDI 合成 SDK 延伸模組**

1.  在 \[方案總管\] 中您的專案底下，以滑鼠右鍵按一下 \[參考\]，然後選取 \[加入參考\]。   
2.  展開 \[Universal Windows\] 節點。 
3.  選取 [**擴充功能**]。
4.  從擴充功能清單選取 [Microsoft General MIDI DLS for Universal Windows Apps]  。
    > [!NOTE] 
    > 如果擴充功能有多個版本，請務必選取符合您 app 之目標的版本。 您可以在專案的 [屬性]、[應用程式]  索引標籤上查看設為 app 目標的 SDK 版本。

 

 




