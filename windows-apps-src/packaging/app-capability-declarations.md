---
ms.assetid: 25B18BA5-E584-4537-9F19-BB2C8C52DFE1
title: 應用程式功能宣告
description: 必須在 Windows 應用程式的封裝資訊清單中宣告功能，才能存取特定的 API 或資源 (如圖片、音樂)，或是相機或麥克風等裝置。
ms.date: 02/10/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 0f5f3e13c1b272c88cdc679c2aec73e59bd1e13d
ms.sourcegitcommit: af0030e95cf24e670bb91fa59c3c1217e5e8ab02
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/10/2020
ms.locfileid: "84663960"
---
# <a name="app-capability-declarations"></a>應用程式功能宣告

必須在 Windows 應用程式的[封裝資訊清單](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest)中宣告功能，才能存取特定 Windows 10 API 或資源 (如圖片、音樂)，或是相機或麥克風等裝置。 UWP 應用程式以及其他類型的傳統型應用程式 (封裝在 Windows 10 的 MSIX 或 AppX 套件中) 會使用功能。

您可以藉由在 app 的[套件資訊清單](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest)中宣告功能，來要求存取特定資源或 API。 您可以使用 Visual Studio 中的[資訊清單設計工具](/windows/msix/package/packaging-uwp-apps#configure-an-app-package)來宣告一般的功能，也可以手動新增。 如需詳細資訊，請參閱[如何在套件資訊清單中指定功能](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-specify-capabilities-in-a-package-manifest)。 請務必了解，當客戶從市集取得您的 app 時，會得知該 app 宣告的所有功能。 因此，請避免宣告您的 app 不需要的功能。

部分功能提供 app 存取*敏感資源*的權限。 這些資源會視為敏感資源是因為它們可以存取使用者的個人資料或使用者必須付費才能使用。 受設定 app 管理的隱私權設定可讓使用者動態控制敏感資源的存取權。 因此，您的 app 不會假設敏感資源可隨時使用這點非常重要。 如需存取敏感資源的詳細資訊，請參閱[隱私權感知 app 的指導方針](https://docs.microsoft.com/windows/uwp/security/index)。 提供應用程式「敏感資源」存取權的功能，於功能案例旁邊會加註星號 (\*)。

有數種類型的功能。

- [一般用途功能](#general-use-capabilities)，適用於大部分常見應用程式案例。
- [裝置功能](#device-capabilities)，可以讓您的應用程式存取周邊和內部裝置。
- [受限功能](#restricted-capabilities)，需要取得 Microsoft Store 提交核准，且/或通常只有 Microsoft 和特定合作夥伴可以使用。
- [自訂功能](#custom-capabilities)。

## <a name="general-use-capabilities"></a>一般用途功能

一般用途功能是使用應用程式封裝資訊清單中的 **Capability** 元素來指定。 這些功能適用於大部分常見的應用程式案例。

> [!NOTE]
> 在封裝資訊清單中的 **Capabilities** 節點底下，所有的 **Capability** 元素都必須放在任何 [CustomCapability](#custom-capabilities) 和 [DeviceCapability](#device-capabilities) 元素之前。

| 功能案例 | 功能使用方式 |
|---------------------|------------------|
| **音樂**\* | **musicLibrary** 功能提供對使用者 [音樂] 媒體櫃的程式化存取，可讓應用程式列舉及存取媒體櫃中的所有檔案，而不需使用者互動。 這個功能通常用於點唱機 app，這類 app 需要存取整個 [音樂] 媒體櫃。<br /><br />[檔案選擇器](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)提供健全的 UI 機制，讓使用者可開啟要供 app 使用的檔案。 只有當您 app 的案例需要以程式設計方式進行存取，而您無法使用檔案選擇器來實現時，才需宣告 **musicLibrary** 功能。<br /><br /> 在您的 app 套件資訊清單中宣告 **musicLibrary** 功能時，它必須包含 **uap** 命名空間，如下所示。 <br /><br />```<Capabilities><uap:Capability Name="musicLibrary"/></Capabilities>```
| **圖片**\* | **picturesLibrary** 功能提供對使用者 [圖片] 庫的程式化存取，可讓應用程式列舉及存取媒體櫃中的所有檔案，而不需使用者互動。 這個功能通常用於相片 app，這類 app 需要存取整個 [圖片] 媒體櫃。<br /><br />[檔案選擇器](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)提供健全的 UI 機制，讓使用者可開啟要供 app 使用的檔案。 只有當您 app 的案例需要以程式設計方式進行存取，而您無法使用檔案選擇器來實現時，才需宣告 **picturesLibrary** 功能。<br /><br />在您的 app 套件資訊清單中宣告 **picturesLibrary** 功能時，它必須包含 **uap** 命名空間，如下所示。<br /><br />```<Capabilities><uap:Capability Name="picturesLibrary"/></Capabilities>```
| **視訊**\* | **videosLibrary** 功能提供以程式設計方式存取使用者的 [影片]，允許 app 在沒有使用者互動的情況下，列舉和存取媒體櫃中的所有檔案。 這個功能通常用於電影播放 app，這類 app 需要存取整個 [影片] 媒體櫃。<br /><br />[檔案選擇器](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)提供健全的 UI 機制，讓使用者可開啟要供 app 使用的檔案。 只有當您 app 的案例需要以程式設計方式進行存取，而您無法使用檔案選擇器來實現時，才需宣告 **videosLibrary** 功能。<br /><br />在您的 app 套件資訊清單中宣告 **videosLibrary** 功能時，它必須包含 **uap** 命名空間，如下所示。<br /><br />```<Capabilities><uap:Capability Name="videosLibrary"/></Capabilities>```
| **卸除式存放裝置** | **removableStorage** 功能提供以程式設計方式存取抽取式存放裝置 \(例如 USB 快閃磁碟機和外接式硬碟\) 中的檔案 \(已篩選出套件資訊清單中宣告的檔案類型關聯\) 的功能。 例如，如果文件閱讀程式 app 宣告 .doc 檔案類型關聯，則可以開啟抽取式存放裝置上的 .doc 檔案，但不能開啟其他類型的檔案。 宣告這個功能時請小心，因為使用者可能會在他們的抽取式存放裝置上包含各種不同資訊，而且會預期您的 app 會針對以程式設計方式存取抽取式存放裝置上所有宣告的檔案類型提供有效的調整。<br /><br />使用者會預期您的 app 會處理宣告的所有檔案關聯。 因此，請勿宣告您的 app 無法負責處理的檔案關聯。 [檔案選擇器](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)提供健全的 UI 機制，讓使用者可開啟要供 app 使用的檔案。<br /><br />只有當您 app 的案例需要以程式設計方式進行存取，而您無法使用[檔案選擇器](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)來實現時，才需宣告 **removableStorage** 功能。<br /><br />在您的 app 套件資訊清單中宣告 **removableStorage** 功能時，它必須包含 **uap** 命名空間，如下所示。<br /><br />```<Capabilities><uap:Capability Name="removableStorage"/></Capabilities>```
| **網際網路和公用網路**\* | 有兩個功能可以提供不同等級的網際網路和公用網路存取權。<br /><br />**internetClient** 功能指出 app 可接收從網際網路傳入的資料。 無法做為伺服器。 不具區域網路存取權。<br />**internetClientServer** 功能指出 app 可接收從網際網路傳入的資料。 可以做為伺服器。 不具區域網路存取權。<br /><br />大部分含有 Web 服務元件的 app 都將使用 **internetClient**。 啟用點對點 (P2P) 案例的 app (此 app 需要接聽連入的網路連線) 應該使用 **internetClientServer**。 **internetClientServer** 功能包含 **internetClient** 功能提供的存取權，因此指定 **internetClientServer** 就不需要指定 **internetClient**。
| **家用與工作場所網路**\* | **privateNetworkClientServer** 功能可以透過防火牆，提供家用與工作場所網路的對內和對外存取。 這個功能通常用於透過區域網路 (LAN) 通訊的遊戲，或在各種本機裝置上共用資料的 app。 如果 app 指定 **musicLibrary**、**picturesLibrary** 或 **videosLibrary**，則您不需使用這個功能來存取家用群組中對應的媒體櫃。 在 Windows 上，這個功能不提供網際網路的存取權。
| **約會** | **appointments** 功能可提供存取使用者的約會存放區。 這個功能提供從已同步網路帳戶取得之約會和其他寫入約會存放區之 app 的讀取權限。 有了這個功能，您的 app 就可以建立新的行事曆，以及將約會寫入建立的行事曆中。<br /><br />在您的 app 套件資訊清單中宣告 **appointments** 功能時，它必須包含 **uap** 命名空間，如下所示。<br /><br />```<Capabilities><uap:Capability Name="appointments"/></Capabilities>```
| **連絡人**\* | **contacts** 功能可存取來自各個連絡人存放區的連絡人彙總檢視。 這個功能提供 app 有限的存取權 (套用網路許可規則)，讓 app 存取與各種網路同步的連絡人和本機連絡人存放區。<br /><br />在您的 app 套件資訊清單中宣告 **contacts** 功能時，它必須包含 **uap** 命名空間，如下所示。<br /><br />```<Capabilities><uap:Capability Name="contacts"/></Capabilities>```
| **程式碼產生** | **codeGeneration** 功能讓 App 能夠存取下列函式，以提供 JIT 功能給 App。<br /><br />[**VirtualProtectFromApp**](https://docs.microsoft.com/windows/desktop/api/memoryapi/nf-memoryapi-virtualprotectfromapp)<br />[**CreateFileMappingFromApp**](https://docs.microsoft.com/windows/desktop/api/memoryapi/nf-memoryapi-createfilemappingfromapp)<br />[**OpenFileMappingFromApp**](https://docs.microsoft.com/windows/desktop/api/memoryapi/nf-memoryapi-openfilemappingfromapp)<br />[**MapViewOfFileFromApp**](https://docs.microsoft.com/windows/desktop/api/memoryapi/nf-memoryapi-mapviewoffilefromapp)
| **AllJoyn** | **allJoyn** 功能讓網路上具備 AllJoyn 功能的 app 和裝置能夠進行探索且與彼此互動。<br /><br />存取 [**Windows.Devices.AllJoyn**](https://docs.microsoft.com/uwp/api/Windows.Devices.AllJoyn) 命名空間中之 API 的所有 app 都必須使用這個功能。
| **通話** | **phoneCall** 功能讓 app 能夠存取裝置上的所有電話線路，並執行下列功能。<ul><li>在不提示使用者的情況下，透過該電話線路撥打電話，並顯示系統撥號程式。</li><li>存取與線路相關的中繼資料。</li><li>存取與線路相關的觸發程序。</li><li>讓使用者選取的垃圾電話篩選 app 能夠設定和檢查封鎖清單及通話來源資訊。</li></ul>在您的 app 套件資訊清單中宣告 **phoneCall** 功能時，它必須包含 **uap** 命名空間，如下所示。<br /><br />```<Capabilities><uap:Capability Name="phoneCall"/></Capabilities>```<br /><br /><b>phoneCallHistoryPublic</b> 功能讓應用程式能夠讀取行動電話和該裝置上的一些 VoIP 通話歷程記錄資訊。 這個功能也可讓應用程式寫入 VoIP 通話歷程記錄項目。 要存取 <a href="https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Calls.PhoneCallHistoryStore"><b>PhoneCallHistoryStore</b></a> 類別的所有成員，必須要有這個功能。
| **錄製的通話資料夾**\* | **recordedCallsFolder** 裝置功能讓 app 能夠存取錄製的通話資料夾。<br /><br />在您的 app 套件資訊清單中宣告 **recordedCallsFolder** 功能時，它必須包含 **mobile** 命名空間，如下所示。<br /><br />```<Capabilities><mobile:Capability Name="recordedCallsFolder"/></Capabilities>```
| **使用者帳戶資訊**\* | **userAccountInformation** 功能讓 app 能夠存取使用者的名稱和圖片。<br /><br />您需要具備這個功能，才能存取 [**Windows.System.UserProfile**](https://docs.microsoft.com/uwp/api/Windows.System.UserProfile) 命名空間中的某些 API。<br /><br />在您的 app 套件資訊清單中宣告 **userAccountInformation** 功能時，它必須包含 **uap** 命名空間，如下所示。<br /><br />```<Capabilities><uap:Capability Name="userAccountInformation"/></Capabilities>```
| **VoIP 通話** | **voipCall** 功能讓應用程式能夠存取 [**Windows.ApplicationModel.Calls**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Calls) 命名空間中的 VoIP 通話 API。<br /><br />在您的 app 套件資訊清單中宣告 **voipCall** 功能時，它必須包含 **uap** 命名空間，如下所示。<br /><br />```<Capabilities><uap:Capability Name="voipCall"/></Capabilities>```
| **3D 物件** | **objects3D** 功能讓 app 能以程式設計的方式存取 3D 物件檔案。 這項功能通常用於必須存取整個 3D 物件庫的 3D 應用程式和遊戲。<br /><br />需要具備這個功能，才能使用 [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage) 命名空間中的 API 存取包含 3D 物件的資料夾。<br /><br />在您的 app 套件資訊清單中宣告 **objects3D** 功能時，它必須包含 **uap** 命名空間，如下所示。<br /><br />```<Capabilities><uap:Capability Name="objects3D"/></Capabilities>```
| **讀取已封鎖訊息**\* | **blockedChatMessages** 功能讓 app 能夠讀取已由垃圾郵件篩選 app 封鎖的簡訊和多媒體簡訊訊息。<br /><br />需要具備這個功能，才能使用 [**Windows.ApplicationModel.Chat**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Chat) 命名空間中的 API 存取已封鎖的訊息。<br /><br />在您的 app 套件資訊清單中宣告 **blockedChatMessages** 功能時，它必須包含 **uap** 命名空間，如下所示。<br /><br />```<Capabilities><uap:Capability Name="blockedChatMessages"/></Capabilities>```
| **自訂裝置** | 在達到一些額外的要求時，**lowLevelDevices** 功能可讓應用程式存取自訂裝置。 這項功能不應與 **lowLevel** 裝置功能混淆，後者可讓您存取 GPIO、I2C、SPI、PWM 裝置。<br /><br /> 如果您開發的自訂驅動程式會公開 [裝置介面](https://docs.microsoft.com/windows-hardware/drivers/install/device-interface-classes)，而且您想要開啟此裝置的控制代碼並傳送 IOCTL，您必須： <ul><li>啟用應用程式資訊清單中的 **lowLevelDevices** 功能：```<Capabilities><iot:Capability Name="lowLevelDevices"/></Capabilities>```</li><li>啟用 [embedded 模式](https://docs.microsoft.com/windows/iot-core/develop-your-app/EmbeddedMode)</li><li>在您的 [INF](https://docs.microsoft.com/previous-versions/windows/desktop/deviceaccess/register-the-device-interface-class-as-privileged) 中，或在驅動程式中呼叫 [WdfDeviceAssignInterfaceProperty()](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdevice/nf-wdfdevice-wdfdeviceassigninterfaceproperty)，將裝置介面標示為 [restricted](https://docs.microsoft.com/windows-hardware/drivers/install/devpkey-deviceinterface-restricted)。</ul>然後，您可以使用 [**Windows.Devices.Custom.CustomDevice**](https://docs.microsoft.com/uwp/api/Windows.Devices.Custom.CustomDevice) 開啟裝置的控制代碼。 如需詳細資訊，請參閱[適用於內部裝置的 UWP 裝置應用程式](https://docs.microsoft.com/windows-hardware/drivers/devapps/uwp-device-apps-for-specialized-devices)。
| **IoT 系統管理** | **systemManagement** 功能讓 app 具備基本的系統管理權限，例如關機或重新啟動、地區設定和時區。<br /><br />需要具備這個功能，才能存取 [**Windows.System**](https://docs.microsoft.com/uwp/api/Windows.System) 命名空間中的某些 API。<br /><br />在您的 app 套件資訊清單中宣告 **systemManagement** 功能時，它必須包含 **iot** 命名空間，如下所示。<br /><br />```<Capabilities><iot:Capability Name="systemManagement"/></Capabilities>```
| **背景媒體播放** | **backgroundMediaPlayback** 功能會變更媒體特定 API (例如 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) 和 [**AudioGraph**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph) 類別) 的行為，以便在您的 app 位於背景時啟用媒體播放。 所有作用中的音訊資料流將不再設為靜音，但在 app 轉換到背景時仍可繼續聽到聲音。 此外，進行播放時，將會自動延伸應用程式存留期。
| **遠端系統** | **remoteSystem** 功能讓 app 能夠存取與使用者 Microsoft 帳戶相關聯的裝置清單。 需要存取裝置清單，才能執行任何保留在裝置上的操作。 需要具備這個功能，才能存取下列各項的所有成員。<ul><li>[Windows.System.RemoteSystems](https://docs.microsoft.com/uwp/api/windows.system.remotesystems) 命名空間</li><li>[Windows.System.RemoteLauncher](https://docs.microsoft.com/uwp/api/Windows.System.RemoteLauncher) 類別</li><li>[AppServiceConnection.OpenRemoteAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.openremoteasync) 方法</li></ul> |
| **空間感知** | **spatialPerception** 功能提供空間對應資料的程式設計存取，並提供有關使用者附近空間的應用程式指定區域中表面的混合實境應用程式資訊。  只有在應用程式明確使用這些表面網格時，才要宣告 spatialPerception 功能，因為混合實境應用程式根據使用者頭部姿勢執行全像轉譯時並不需要此功能。 |
| **全域媒體控制** | **globalMediaControl** 功能可讓應用程式存取整個系統中與 [**SystemMediaTransportControls**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControls) 整合的播放會話，以提供播放資訊並允許遠端控制。 需要具備這個功能，才能使用 [**Windows.Media.Control**](https://docs.microsoft.com/uwp/api/windows.media.control) 命名空間中的某些 API。 這項功能是在 [uap7:Capability](/uwp/schemas/appxpackage/uapmanifestschema/element-uap7-capability) 元素中定義。  |

## <a name="device-capabilities"></a>裝置功能

裝置功能可以讓您的 app 存取周邊和內部裝置。 裝置功能是使用應用程式套件資訊清單中的 **DeviceCapability** 元素來指定。 這個元素可能需要其他子元素，而且您必須手動將一些裝置功能新增到套件資訊清單。 如需詳細資訊，請參閱[如何在套件資訊清單中指定裝置功能](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-specify-device-capabilities-in-a-package-manifest)和 [**DeviceCapability 結構描述參考**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-devicecapability)。

> [!NOTE]
> 封裝資訊清單中的 **Capabilities** 元素下，您可以有多個 **DeviceCapability** 元素。 所有 **DeviceCapability** 元素都必須在任何 **Capability** 和 [CustomCapability](#custom-capabilities) 元素之後。

| 功能案例 | 功能使用方式 |
|---------------------|------------------|
| **位置**\* | **location** 功能提供存取位置功能，您可以透過專用硬體 (例如電腦中的 GPS 感應器) 來擷取這類功能，或是從可用的網路資訊加以衍生。 當使用者從設定常用鍵停用定位服務時，app 必須能夠處理這種狀況。 |
| **麥克風** | **microphone** 功能提供存取麥克風的音訊饋送，讓 app 能夠從連接的麥克風錄音。 當使用者從設定常用鍵停用麥克風時，app 必須能夠處理這種狀況。 |
| **接近** | **proximity** 功能可以讓多個近接的裝置彼此通訊。 這個功能通常用於輕鬆的多人遊戲，以及交換資訊的 app 中。 裝置會試著使用提供最佳連線的通訊技術，例如藍牙、Wi-Fi 以及網際網路。 這個功能只能用於起始裝置間的通訊。 |
| **網路攝影機** | **webcam** 功能提供存取內建相機或外接式網路攝影機的視訊饋送，讓 app 能夠擷取相片和影片。 在 Windows 上，當使用者從設定常用鍵停用相機時，app 必須能夠處理這種狀況。<br/>**webcam** 功能只授與對視訊串流的存取權。 若要同時授與對音訊串流的存取權，必須新增 **microphone** 功能。 |
| **USB** | **usb** 裝置功能可讓您存取[針對 USB 裝置更新應用程式資訊清單套件](https://msdn.microsoft.com/library/windows/hardware/dn303351(v=vs.85).aspx)中所述的 API。 |
| **人性化介面裝置 (HID)** | **humaninterfacedevice** 裝置功能可讓您存取[如何指定 HID 的裝置功能](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-specify-device-capabilities-for-hid)中的 API。 |
| **服務點 (POS)** | **pointOfService** 裝置功能可讓您存取 [**Windows.Devices.PointOfService**](https://docs.microsoft.com/uwp/api/Windows.Devices.PointOfService) 命名空間中的 API。 這個命名空間可以讓您的 app 存取服務指標 (POS) 條碼掃描器和磁條讀取器。 這個命名空間提供一個廠商中性介面，可讓您從 UWP 應用程式存取來自各個製造商的 POS 裝置。 |
| **Bluetooth** | **bluetooth** 裝置功能讓 app 能夠與兩個都透過 Generic Attribute (GATT) 或 Classic Basic Rate (RFCOMM) 通訊協定且配對好的藍牙裝置通訊。<br/>需要具備這個功能，才能使用 [**Windows.Devices.Bluetooth**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth) 命名空間中的某些 API。 |
| **Wi-Fi 網路功能** | **wiFiControl** 裝置功能讓 app 能夠掃描並連線到 Wi-Fi 網路。<br/>需要具備這個功能，才能使用 [**Windows.Devices.WiFi**](https://docs.microsoft.com/uwp/api/Windows.Devices.WiFi) 命名空間中的某些 API。 |
| **無線電狀態** | **radios** 裝置功能讓 app 能夠在 Wi-Fi 和藍牙無線電之間切換。<br/>需要具備這個功能，才能使用 [**Windows.Devices.Radios**](https://docs.microsoft.com/uwp/api/Windows.Devices.Radios) 命名空間中的 API。  |
| **光碟片** | **optical** 裝置功能讓 app 能夠存取光碟機 (例如 CD、DVD 及藍光) 上的功能。<br/>需要具備這個功能，才能使用 [**Windows.Devices.Custom**](https://docs.microsoft.com/uwp/api/Windows.Devices.Custom) 命名空間中的某些 API。 |
| **動作活動** | **activity** 裝置功能讓 app 能夠偵測裝置目前的動作。<br/>需要具備這個功能，才能使用 [**Windows.Devices.Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors) 命名空間中的某些 API。 |
| **序列通訊** | **Serialcommunication** 裝置功能提供 Windows.Devices.SerialCommunication 命名空間中 API 的存取權，可讓 Windows 應用程式與公開序列埠或序列埠部分抽象概念的裝置進行通訊。 需要具備這個功能，才能使用 [**Windows.Devices.SerialCommnication**](https://docs.microsoft.com/uwp/api/windows.devices.serialcommunication) 命名空間中的 API。 |
| **眼動追蹤儀** | 相容眼動追蹤裝置已連接時，**gazeInput** 功能可讓應用程式偵測使用者在應用程式範圍內注視的位置。 需要具備這個功能，才能使用 [**Windows.Devices.Input.Preview**](https://docs.microsoft.com/uwp/api/windows.devices.input.preview) 命名空間中的某些 API。 |
| **GPIO、I2C、SPI、PWM** | **lowLevel** 裝置功能可讓您存取 GPIO、I2C、SPI、PWM 裝置。 若要在下列命名空間中使用 API，這項功能是必要的：[**Windows.Devices.Gpio**](https://docs.microsoft.com/uwp/api/windows.devices.gpio)、[**Windows.Devices.I2c**](https://docs.microsoft.com/uwp/api/windows.devices.i2c)、[**Windows.Devices.Spi**](https://docs.microsoft.com/uwp/api/windows.devices.spi)、[**Windows.Devices.Pwm**](https://docs.microsoft.com/uwp/api/windows.devices.pwm)。<br /><br />```<Capabilities><DeviceCapability Name="lowLevel"/></Capabilities>``` |


<span id="special-and-restricted-capabilities" />

## <a name="restricted-capabilities"></a>受限功能

如果您的應用程式宣告任何受限制的功能，您必須在[應用程式提交程序](../publish/app-submissions.md)期間提供資訊，才能獲准將應用程式發佈至 Microsoft Store。 您可以在提交的 [提交選項](../publish/manage-submission-options.md#restricted-capabilities) 頁面上提供這項資訊，說明您的應用程式如何使用它所宣告的每個受限功能。

> [!IMPORTANT]
> 受限功能適用於非常特殊的情況。 這些功能的用法受到高度限制，而且受其他市集上架原則和審查規定的規範。 請注意，您可以在不需要收到任何核准的情形下側載有宣告受限功能的應用程式。 只有在將這些應用程式提交到 Microsoft Store 時才需要核准。

除非您的應用程式真正需要，否則請務必不要宣告這些受限功能。 這類功能在某些情況下是必要且適當的，例如具備雙因素驗證的銀行系統，使用者需提供含數位憑證的智慧卡來確認身分識別。 其他應用程式主要可能是針對企業客戶所設計，而且可能需要存取公司資源，若使用者沒有網域認證，便無法存取這類公司資源。

若要宣告受限功能，請修改您的[應用程式封裝資訊清單](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest)來源檔案 (`Package.appxmanifest`)。 新增 **xmlns:rescap** XML 命名空間宣告，並在宣告受限功能時使用 **rescap** 前置詞。 例如，以下示範如何宣告 **appCaptureSettings** 功能。

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package
    ...
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    IgnorableNamespaces="... rescap">
...
<Capabilities>
    <rescap:Capability Name="appCaptureSettings"/>
</Capabilities>
</Package>
```

> [!NOTE]
> 在封裝資訊清單中的 **Capabilities** 節點底下，所有的受限功能元素都必須放在任何 [CustomCapability](#custom-capabilities) 和 [DeviceCapability](#device-capabilities) 元素之前。

### <a name="restricted-capability-approval-process"></a>受限功能核准程序

在過去，我們要求您連絡支援服務以取得使用功能的核准。 我們現在可讓您在[合作夥伴中心](https://partner.microsoft.com/dashboard)中提供這項資訊，作為[提交程序](../publish/app-submissions.md)的一部分。

當您上傳要提交的套件時，我們將偵測是否有宣告受限功能。 如果我們這樣做，您將需要在[提交選項](../publish/manage-submission-options.md#restricted-capabilities)頁面提供有關您的產品如何使用每項功能的詳細資訊。 請務必提供詳細資料以協助我們了解您的產品需要宣告該功能的原因。 請注意，應用程式提交可能需要更多時間來完成認證程序。

認證過程中，我們的測試人員將會檢閱您所提供的資訊，以判斷是否核准您提交的應用程式使用該功能。 請注意，應用程式提交可能需要更多時間來完成認證程序。 如果我們核准您使用此功能，您的應用程式將繼續進行認證程序的其餘部分。 當您提交應用程式的更新時，通常不必重複功能核准程序 (除非您宣告其他功能)。

如果我們不核准您使用此功能，您的提交無法通過認證，而且我們將會在認證報告中提供意見反應。 然後，您可以選擇建立新的提交和上傳不宣告此功能的套件，或 (如果適用) 解決有關您使用此功能的任何問題，並在新提交中要求核准。

> [!NOTE]
> 如果您的提交使用合作夥伴中心中的開發沙箱 (例如，與 Xbox Live 整合的任何遊戲案例)，您必須預先要求核准，而不是在 [提交選項] 頁面提供資訊。 若要這樣做，請瀏覽 [Windows 開發人員支援頁面](https://developer.microsoft.com/windows/support)。 [開發人員支援主題] 請選取 [儀表板問題]，[問題類型] 請選取 [應用程式提交]，[子類別] 請選取 [其他]。 然後描述您使用此功能的方式，以及您的產品為何需要它。 如果您未提供所有必要資訊，您的要求將會遭到拒絕。 您可能還需要提供更多資訊。 請注意，此程序通常會需要 5 個工作天或更長，所以請事前提交您的要求。
>
> 不論您是否使用開發沙箱，如果您想要在開始提交之前先確認能獲准使用受限功能，也可以利用這種方式來要求核准 (而不是在提交期間提供這項資訊)。

<span id="restricted-and-special-use-capability-list" />

### <a name="restricted-capability-list"></a>受限功能清單

下表列出受限制的功能。 您可以依照上述的程序，為提交到 Microsoft Store 的應用程式中的這些功能要求核准。

> [!IMPORTANT]
> 除非在非常特定與有限的情況中，否則提交到 Microsoft Store 的應用程式，某些受限制的功能幾乎不會獲得核准。 下表說明這些功能。 如果您想要透過 Microsoft Store 散發您的應用程式，建議您不要在應用程式中宣告這些功能。

| 功能案例 | 功能使用方式 |
|---------------------|------------------|
| **企業** | Windows 網域認證可以讓使用者使用自己的認證登入遠端資源，如同使用者提供自己的使用者名稱和密碼一樣。 **enterpriseAuthentication** 功能通常是在連線到企業內伺服器的特定業務應用程式中使用。 <br /><br />您不需要針對網際網路上的一般通訊使用此功能。<br /><br />**enterpriseAuthentication** 功能的目的在於支援常見的特定業務應用程式。 請勿在不需要存取公司資源的 app 中宣告此功能。 [檔案選擇器](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)提供健全的 UI 機制，能夠讓使用者開啟網路共用上要供 app 使用的檔案。 只有當您應用程式的案例需要以程式設計方式進行存取，而您無法使用**檔案選擇器**來實現時，才需宣告 **enterpriseAuthentication** 功能。<br /><br />在您的 app 套件資訊清單中宣告 **enterpriseAuthentication** 功能時，它必須包含 **uap** 命名空間，如下所示。<br /><br />```<Capabilities><uap:Capability Name="enterpriseAuthentication"/></Capabilities>```<br /><br />**enterpriseDataPolicy** 功能可讓應用程式在使用「Windows 資訊保護」原則 (例如：行動裝置管理和行動應用程式管理系統) 管理應用程式時，安全地分開處理企業資料。  如下宣告此受限功能： <br /><br />```<Capabilities><rescap:Capability Name="enterpriseDataPolicy"/></Capabilities>```<br /><br />若要使用下列類別的所有成員，必須要有這個功能。<ul><li><a href="https://docs.microsoft.com/uwp/api/Windows.Security.EnterpriseData.FileProtectionManager">FileProtectionManager</a></li><li><a href="https://docs.microsoft.com/uwp/api/Windows.Security.EnterpriseData.DataProtectionManager">DataProtectionManager</a></li><li><a href="https://docs.microsoft.com/uwp/api/Windows.Security.EnterpriseData.ProtectionPolicyManager">ProtectionPolicyManager</a></li></ul> |
| **共用的使用者憑證** | **sharedUserCertificates** 功能可讓應用程式在「共用使用者」存放區中新增和存取軟體和硬體型憑證，例如儲存在智慧卡的憑證。 這個功能通常用於需要使用智慧卡進行身分驗證的金融或企業 app。<br /><br />在您的 app 套件資訊清單中宣告 **sharedUserCertificates** 功能時，它必須包含 **uap** 命名空間，如下所示。<br /><br />```<Capabilities><uap:Capability Name="sharedUserCertificates"/></Capabilities>``` |
|**文件**\* | **documentsLibrary** 功能提供以程式設計方式存取使用者的 [文件] 程式庫 (並篩選出封裝資訊清單所宣告的檔案類型關聯)。 例如，如果文書處理應用程式宣告了 .doc 檔案類型關聯，它就可以開啟使用者 [文件] 程式庫中的 .doc 檔案。 <br /><br />只有在應用程式以程式設計方式存取 Documents 程式庫，而「不需」使用者介入時，才需要 **documentsLibrary** 功能。 如果使用者使用選擇器 API 進行選擇，您的應用程式<b>不</b>需要 **documentsLibrary** 功能即可存取 Documents 程式庫。 一般而言，應用程式應該使用下列其中一個選擇器 API 來允許使用者選擇其檔案的位置： <ul><li>[FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 可開啟現有的檔案。</li><li>[FileSavePicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker) 可儲存新檔案。</li><li>[FolderPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.folderpicker) 可選擇要開啟/儲存其他檔案的資料夾。</li></ul>使用這些 API 可讓使用者選擇最適合的位置，例如雲端同步處理的帳戶 (例如 OneDrive)。 使用者使用這些 API 挑選檔案或資料夾之後，您的應用程式就可以接著使用 [FutureAccessList](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) API 來存取該位置。 此 API 可讓您的應用程式在未來存取檔案或資料夾，而不會要求使用者再次加以選擇。<br /><br />在現有工作流程假設檔案會在 [文件] 程式庫中 (例如，與現有的傳統型應用程式交互操作) 的情況，或您不想讓使用者選擇位置的情況下，您可以為應用程式宣告 **documentsLibrary** 功能。 如果您的應用程式使用 **documentsLibrary** 功能，建議您也允許使用者手動挑選位置。<br /><br />在您的 app 套件資訊清單中宣告 **documentsLibrary** 功能時，它必須包含 **uap** 命名空間，如下所示。<br /><br />```<Capabilities><uap:Capability Name="documentsLibrary"/></Capabilities>``` |
| **遊戲 DVR 設定** | **appCaptureSettings** 受限制的功能讓 app 能夠控制「遊戲 DVR」的使用者設定。<br /><br />需要具備這個功能，才能使用 [**Windows.Media.Capture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture) 命名空間中的某些 API。 <br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。  |
| **行動電話通訊** | **cellularDeviceControl** 受限制的功能讓 app 能夠控制行動電話通訊裝置。<br /><br />**cellularDeviceIdentity** 功能讓 app 能夠存取行動電話通訊識別資料。<br /><br />**cellularMessaging** 功能讓 app 能夠使用簡訊和 RCS。<br /><br />需要具備這些功能，才能使用 [**Windows.Devices.Sms**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sms) 命名空間中的某些 API。  |
| **裝置解除鎖定** | **deviceUnlock** 受限制的功能讓 app 能夠解除鎖定裝置，以供開發人員和企業側載案例使用。<br /><br /> 我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **雙 SIM 卡磚** | **dualSimTiles** 受限制的功能讓應用程式能夠在配備多張 SIM 卡的裝置上建立其他應用程式清單項目。<br /><br />需要具備這個功能，才能使用 [**Windows.UI.StartScreen**](https://docs.microsoft.com/uwp/api/Windows.UI.StartScreen) 命名空間中的某些 API。 |
| **企業共用存放裝置** | **enterpriseDeviceLockdown** 受限制的功能讓 app 使用裝置鎖定 API，以及存取企業共用存放裝置資料夾。 <br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **系統輸入導入** | **inputInjectionBrokered** 受限功能讓應用程式能夠以程式設計方式，將各種不同形式的輸入 (例如，HID、觸控、手寫筆、鍵盤、滑鼠) 導入系統中。 這個功能通常用於可控制系統的共同作業應用程式。<br /><br />在電腦上，從具備此功能的應用程式導入的輸入源，只能透過位於相同應用程式容器的處理程序來接收。<br /><br />```<Capabilities><rescap:Capability Name="inputInjectionBrokered" /></Capabilities>``` |
| **觀察輸入**\* | **inputObservation** 受限制的功能讓 app 能夠觀察系統所接收到的各種不同形式的原始輸入 (例如，HID、觸控、手寫筆、鍵盤或滑鼠)，而不論其最終目的地為何。<br /><br />這項功能及其相關的 API 僅供選定的 Microsoft 合作夥伴使用。  |
| **隱藏輸入** | **inputSuppression** 受限制的功能讓 app 能夠隱藏系統所接收到的各種不同形式的原始輸入 (例如，HID、觸控、手寫筆、鍵盤或滑鼠)。<br /><br />這項功能及其相關的 API 僅供選定的 Microsoft 合作夥伴使用。 |
| **VPN 應用程式** | **networkingVpnProvider** 受限制的功能讓 app 能夠具備 VPN 功能的完整存取權，包括管理連線的能力，以及提供 VPN 外掛程式功能。<br /><br />需要具備這個功能，才能使用 [**Windows.Networking.Vpn**](https://docs.microsoft.com/uwp/api/Windows.Networking.Vpn) 命名空間中的某些 API。 |
| **其他應用程式管理** | **packageManagement** 受限制的功能讓 app 能夠直接管理其他 app。<br /><br />**packageQuery** 裝置功能讓 app 能夠收集其他 app 的相關資訊。<br /><br />需要具備這個功能，才能使用存取 [**PackageManager**](https://docs.microsoft.com/uwp/api/Windows.Management.Deployment.PackageManager) 類別中的某些方法和屬性。 |
| **畫面投影** | **screenDuplication** 受限制的功能讓 app 能將畫面投影到其他裝置。<br /><br />需要具備這個功能，才能使用 DirectX 命名空間中的 API。 <br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **使用者主體名稱** | **userPrincipalName** 受限功能可讓應用程式存取目前使用者的使用者主要名稱 (UPN)。<br /><br />需要具備這個功能，才能呼叫 [**GetUserNameEx**](https://docs.microsoft.com/windows/desktop/api/secext/nf-secext-getusernameexa) 函式。 <br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **電子錢包** | **walletSystem** 受限制的功能讓 app 能完整存取儲存式電子錢包卡。<br /><br />需要具備這個功能，才能使用 [**Windows.ApplicationModel.Wallet.System**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Wallet.System) 命名空間中的 API。 <br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **位置歷程記錄** | **locationHistory** 受限制的功能讓 app 能夠存取裝置的位置歷程記錄。<br /><br />需要具備這個功能，才能使用 [**Windows.Devices.Geolocation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation) 命名空間中的 API。
| **應用程式關閉確認** | **confirmAppClose** 受限制的功能讓 app 能夠關閉本身和它們自己的視窗，並延遲關閉它們的 app。<br /><br />Windows 10 版本 1703 (組建 10.0.15063) 及更新版本的應用程式可能會要求此功能。 在先前的 Windows 10 版本中，此功能為私用，並且導致應用程式安裝失敗而顯示「此應用程式無法授權要求的功能」錯誤訊息。 |
| **通話記錄**\* | **phoneCallHistory** 受限制的功能讓 app 能夠讀取通訊記錄並刪除記錄中的項目。<br /><br />需要具備這個功能，才能使用 [**Windows.ApplicationModel.Chat**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Chat) 命名空間中的 API。 <br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **系統層級的約會存取** | **appointmentsSystem** 受限制的功能讓 app 能夠讀取和修改使用者行事曆上的所有約會。<br /><br />需要具備這個功能，才能使用 [**Windows.ApplicationModel.Appointment**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments) 命名空間中的 API。 <br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **系統層級的聊天訊息存取**\* | **chatSystem** 受限制的功能讓 app 能夠讀取和寫入所有簡訊和多媒體簡訊訊息。<br />需要具備這個功能，才能使用 [**Windows.ApplicationModel.Chat**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Chat) 命名空間中的 API。 <br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **系統層級的連絡人存取** | **contactsSystem** 受限制的功能讓 app 能夠讀取已指定為受限制或敏感的連絡人資訊，並修改現有的連絡人資訊。<br /><br />需要具備這個功能，才能使用 [**Windows.ApplicationModel.Chat**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Chat) 命名空間中的 API。 <br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **電子郵件存取** | **email** 受限制的功能讓 app 能夠讀取、分級，以及傳送電子郵件給使用者。<br /><br />需要具備這個功能，才能使用 [**Windows.ApplicationModel.Email**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Email) 命名空間中的 API。 <br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **系統層級的電子郵件存取**| **emailSystem** 受限制的功能讓 app 能夠讀取、分級，以及傳送受限制或敏感的電子郵件給使用者。 <br /><br />需要具備這個功能，才能使用 [**Windows.ApplicationModel.Email**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Email) 命名空間中的 API。 <br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **系統層級的通話記錄存取** | **phoneCallHistorySystem** 受限制的功能讓 app 能夠透過變更現有項目及撰寫新項目來完整修改通話記錄。<br /><br />需要具備這個功能，才能使用 [**Windows.ApplicationModel.Calls**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Calls) 命名空間中的 API。 <br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **傳送簡訊**\* | **smsSend** 受限制的功能讓 app 能夠傳送簡訊和多媒體簡訊訊息。<br /><br />需要具備這個功能，才能使用 [**Windows.ApplicationModel.Chat**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Chat) 命名空間中的 API。 |
| **所有使用者資料的系統層級存取** | **userDataSystem** 受限制的功能讓 app 能夠存取使用者資料系統資料存放區。 <br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **Store 預覽功能** | **previewStore** 受限制的功能讓 app 能夠擷取並購買 app 內產品的 SKU。<br /><br />需要具備這個功能，才能使用 [**Windows.ApplicationModel.Store.Preview**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.Preview) 命名空間中的特定 API。 |
| **第一次登入設定** | **firstSignInSettings** 受限制的功能讓 app 能夠存取使用者首次登入其裝置時所設定的使用者設定。 |
| **Windows Teams 體驗** | **teamEditionExperience** 受限制的功能讓 app 能夠存取控制 Windows 小組工作階段各個體驗層面的內部 API。 Windows 小組工作階段極有可能會在小組裝置 (如 Microsoft Surface Hub) 上執行。 <br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **遠端解除鎖定** | **remotePassportAuthentication** 受限制的功能讓 app 能夠存取可以用來解除遠端電腦鎖定的認證。 <br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **預覽組合** | **previewUiComposition** 受限制的功能讓 app 能夠預覽其使用者介面的 [**Windows.UI.Composition**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition) 命名空間，以便他們可以在完成 API 之前提供意見反應。 如需詳細資訊，請連絡 wincomposition@microsoft.com。 |
| **安全性評定鎖定** | **secureAssessment** 受限制的功能讓 app 能夠將 Windows 鎖定為單一 app 模式以進行安全性評定。 <br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **連線管理員佈建** | **networkConnectionManagerProvisioning** 受限制的功能讓 app 能夠定義可將裝置連線到 WWAN 和 WLAN 介面的原則。 電信業者會建立使用這個功能的 app，以便管理連線至其行動網路的裝置。 |
| **行動數據方案佈建** | **networkDataPlanProvisioning** 受限制的功能讓 app 能夠收集裝置上行動數據方案的相關資訊，並讀取網路使用量。 電信業者會建立使用這個功能的 app，以便將其客戶的實際數據使用量整合到 OS 數據使用量設定。 |
| **軟體授權** | **slapiQueryLicenseValue** 受限制的功能讓 app 能夠查詢軟體授權原則。 <br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **延伸執行** | **extendedBackgroundTaskTime** 受限功能可防止背景工作因執行時間限制而遭到取消或終止。 但仍會受制於所有其他記憶體和能源使用量的限制。 這項功能可以透過電池使用量或隱私權背景應用程式設定進行限制。 請注意：消費者和系統管理員仍然可以透過群組原則設定控制背景工作。<br /><br />**extendedExecutionBackgroundAudio** 受限制的功能讓 app 能夠在背景執行時也能播放音訊。<br /><br />**extendedExecutionCritical** 受限制的功能讓 app 能夠開始嚴重延伸執行工作階段。<br /><br />**extendedExecutionUnconstrained** 受限制的功能讓 app 能夠開始不受限制的延伸執行工作階段。 <br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。<br /><br />如需有關在應用程式暫止時使用延伸執行延期的詳細資訊，請參閱[透過延長執行延後應用程式暫停](https://docs.microsoft.com/windows/uwp/launch-resume/run-minimized-with-extended-execution)。 |
| **行動裝置管理** | **deviceManagementDmAccount** 受限制的功能讓 app 能夠佈建和設定 [電信業者開放行動裝置聯盟 - 裝置管理 (MO OMA-DM)] 帳戶。<br /><br />**deviceManagementFoundation** 受限制的功能讓 app 能夠基本存取裝置上的行動裝置管理 (MDM) 設定服務提供者 (CSP) 基礎結構。 請注意，需有其他功能才能存取特定 CSP。<br /><br />**deviceManagementWapSecurityPolicies** 受限制的功能讓 app 能夠設定無線應用通訊協定 (WAP) 服務，例如 MMs、服務指示/服務載入 (SI/SL) 及開放行動裝置聯盟 - 用戶端佈建 (OMA-CP)。<br /><br />**deviceManagementEmailAccount** 受限制的功能讓 app 能夠由電信業者建立，以便在他們佈建給使用者的裝置上新增和管理電子郵件帳戶。 |
| **套件原則控制項** | **packagePolicySystem** 受限制的功能讓 app 能夠掌控裝置上所安裝之 app 的相關系統原則。<br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **遊戲清單** | **gameList** 受限制的功能讓 app 能夠取得一份系統上已安裝的知名遊戲清單。<br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **Xbox 配件** | **xboxAccessoryManagement** 受限制的功能讓 app 能夠直接管理符合 Xbox 硬體規格的 Xbox 裝置。<br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **附屬應用程式的語音辨識** | **cortanaSpeechAccessory** 受限制的功能讓 app 能夠叫用並傳遞命令到 Cortana。<br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **配件管理** | **accessoryManager** 受限制的功能讓 app 能夠註冊為配件專屬 App，並選擇加入特定 app 通知，以便將它們轉送到配件並向使用者顯示。 |
| **驅動程式存取** | **interopServices** 受限制的功能讓 app 能夠直接與驅動程式互動。 <br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **前景觀察** | **inputForegroundObservation** 受限制的功能讓前景的 app 能夠攔截鍵盤輸入，並繞過所有非 app 鍵盤輸入的處理。 此功能無法攔截 SAS 組合。 需要具備這個功能，才能存取 [**KeyboardDeliveryInterceptor**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.KeyboardDeliveryInterceptor) 類別的成員。
| **OEM 和 MO 合作夥伴應用程式** | **oemDeployment** 受限制的功能讓 Microsoft 合作夥伴建立的 app 可以在裝置上安裝新的 app 和查詢目前安裝的 app。<br /><br />**oemPublicDirectory** 受限制的功能讓 Microsoft 合作夥伴建立的 app 可以存取共用 app 資料夾。 我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **應用程式授權** | **appLicensing** 受限制的功能可允許 app 在不需要授權的情況下執行。 如果您在資訊清單中宣告此功能，您就無法將 app 提交至市集。 <br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **位置系統**| **locationSystem** 受限制的功能可允許 app 執行特定需要特殊權限的位置設定，例如設定裝置的預設位置。 <br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **使用者資料帳戶提供者**| **userDataAccountsProvider** 受限制的功能讓 app 能夠完全管理電子郵件、行事曆和連絡人帳戶。 |
| **手寫筆工作區** | **previewPenWorkspace** 功能讓 app 能夠存取 Windows.ApplicationModel.Preview.Notes 命名空間，此命名空間要裝載於手寫筆工作區內部，以做為記住動作處理常式。 |
| **第二個驗證因素** | **secondaryAuthenticationFactor** 功能讓 app 能夠透過傳送儲存於鄰近隨附驗證裝置上的密碼來解除鎖定電腦。 例如，隨附的健身手環可以用來解除鎖定電腦。 需要具備這個功能，才能存取 Windows.Security.Authentication.Identity.Provider 命名空間中的 API。<br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **Store 授權管理**| **storeLicenseManagement** 功能讓 Microsoft 合作夥伴中樞應用程式能夠管理裝置上的市集授權。 需要具備這個功能，才能存取 Windows.ApplicationModel.Store.LicenseManagement 命名空間中的 API。 |
| **使用者系統識別碼**| **userSystemId** 功能讓 app 能夠取得使用者特定的系統識別碼。 這個識別碼可唯一識別特定系統上目前的使用者，也可以用來使應用程式間的資訊互相關聯。 需要具備這個功能，才能存取 Windows.System.Profile.SystemIdentification 類別中的 GetUserSpecificSystemId API。 |
| **目標式內容**| **targetedContent** 功能提供應用程式擷取及使用 [**Windows.Services.TargetedContent**](https://docs.microsoft.com/uwp/api/windows.services.targetedcontent) 命名空間提供之目標訂閱內容的能力。<br /><br />必須具備這項功能，才能使用 **Windows.System.Profile.SystemIdentification** 命名空間中的某些 API。 |
| **使用者介面自動化**| **uiAutomation** 功能可讓 UI 自動化用戶端 (例如朗讀程式) 連接至 UI 自動化伺服器或提供者。<br /><br />必須具備這項功能，才能使用 **Windows.Xbox.Media.Capture.Broadcaster** 命名空間中的某些 API。 |
|**遊戲列服務**| **gameBarServices** 僅限於第一方 Microsoft Store 可更新收件匣的 UWA。<br /><br />必須具備這項功能，才能使用 [**Windows.Media.Capture.GameBarsSrvices**](https://docs.microsoft.com/uwp/api/windows.media.capture.gamebarservices) 類別。<br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
|**應用程式擷取服務**| **appCaptureServices** 功能僅限於與 Microsoft 具有契約關係之對象。 這些關係會根據由 Xbox 服務及 bizdev 支援推動的合作夥伴協議進行授與。<br /><br />必須具備這項功能，才能使用 [**Windows.Media.Capture.AppCaptureServices**](https://docs.microsoft.com/uwp/api/windows.media.capture.appcaptureservices) 類別。 |
|**應用程式廣播服務**| **appBroadcastServices** 功能僅限於與 Microsoft 具有契約關係之對象。 這些關係會根據由 Xbox 服務支援推動的合作夥伴協議進行授與。<br /> <br />必須具備這項功能，才能使用 [**Windows.Media.capture.AppBroadcastServices**](https://docs.microsoft.com/uwp/api/windows.media.capture.appbroadcastservices) 類別。<br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
|**音訊裝置設定**| **audioDeviceConfiguration** 功能可以讓應用程式查詢、設定、啟用、停用音訊驅動程式公開的音訊效果。 <br /> <br />必須具備這項功能，才能使用 [**Windows.Media.Devices.AudioDeviceModulesManager**](https://docs.microsoft.com/uwp/api/windows.media.devices.audiodevicemodulesmanager) 類別。<br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 這是因為 **AudioDeviceModulesManager** 可讓應用程式存取特定系統上所有的音訊效果。 而音訊效果有可能會用於造成裝置上音訊效能的負面影響。 |
| **背景媒體錄影** | **backgroundMediaRecording** 功能會變更媒體特定 API (例如 [**MediaCapture**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture) 和 [**AudioGraph**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph) 類別) 的行為，以便在您的應用程式位於背景時啟用媒體錄影。 |
|**預覽 Ink 工作區**| **previewInkWorkspace** 功能可讓應用程式存取裝載在 Ink 工作區的預覽 Ink 命名空間。 一般而言，OEM 會使用這項功能來取代裝置上的白板應用程式。<br /> <br />必須具備這項功能，才能使用 [**Windows.ApplicationModel.Preview.InkWorkspace**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.preview.inkworkspace) 命名空間中的 API。 |
|**開始畫面管理**| **startScreenManagement** 功能可讓應用程式直接將磚釘選至開始畫面。 應用程式也可以在背景進行釘選。 不具備 **startScreenManagement** 功能並不會封鎖任何 API。然而，使用 **startScreenManagement** 表示命令介面不會在任何應用程式使用釘選 API 時顯示任何 UI。 |
|**Cortana 權限**| **cortanaPermissions** 功能可讓應用程式列舉使用者授與裝置上 Cortana 的權限。 此功能同時也允許應用程式授與或撤銷裝置上 Cortana 的權限。 請注意，使用 **cortanaPermissions** 需要裝置在授與權限前顯示法律聲明文字。 因此，應用程式應負起責任，告知使用者修改權限可能造成的法律後果。<br /> <br /><br />必須具備這項功能，才能取得 **HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Search** 登錄設定的讀取權限。<br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
|**所有應用程式模組**| **allAppMods** 功能可讓應用程式存取所有應用程式的 AppMods 資料夾。 模組管理公用程式使用 **allAppMods** 在使用該模組的遊戲或應用程式之外管理模組。 |
|**展開的資源**| **expandedResources** 功能可讓應用程式存取遊戲模式資源。 在 Xbox 以及符合足夠標準的電腦上，遊戲模式資源代表可供應用程式獨佔使用的保留 CPU 核心子集。 在 Xbox 上，應用程式也有至少 4 GB 記憶體分割的獨佔使用權。<br /><br />必須具備這項功能，才能取得如上方所定義之 CPU 和記憶體資源的獨佔使用權。 |
|**受保護的應用程式**| **protectedApp** 功能授與應用程式能夠被 Microsoft Store 載入受保護程序的能力。 當應用程式內嵌至 Microsoft Store 時，Microsoft Store 會在執行檔中加入一個 blob。 Microsoft Store 也會使用 Microsoft 金鑰在執行檔上頁面簽署。 由於 blob 需要 Microsoft 的簽章，程序載入器會檢查此 blob，而非這個功能強迫受保護的程序。<br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
|**遊戲監視器**| **gameMonitor** 功能會讓系統使用主動式監視來偵測應用程式的遊戲作弊行為。<br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
|**應用程式診斷**| **appDiagnostics** 功能可讓應用程式取得任何其他正在執行中之 UWP 應用程式的診斷資訊 (例如：套件資訊、記憶體使用量、帳戶名稱)。 傳回的資訊包含了正在執行該應用程式的網域/機器帳戶名稱。若呼叫的應用程式是以系統管理員權限啟動的，則該應用程式還可以擷取機器上所有帳戶正在執行的所有應用程式清單。 <br /><br />必須具備這項功能，才能使用 [**Windows.System.AppDiagnosticInfo**](https://docs.microsoft.com/uwp/api/windows.system.appdiagnosticinfo)、**Windows.System.AppDiagnosticInfo.RequestAppDiagnosticInfoAsync**、[**Windows.ApplicationModel.AppInfo**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appinfo) 類別。 |
| **裝置入口網站提供者** | **devicePortalProvider** 功能可讓應用程式呼叫 **Windows.System.Diagnostics.DevicePortal** API，並在開發人員模式下做為診斷工具的[網頁伺服器](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-plugin)。<br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **企業雲端單一登入** | **enterpriseCloudSSO** 功能可讓應用程式搭配 Web 檢視託管控制項內部的 Azure Active Director (AAD) 資源使用單一登入。 |
| **自動接受 VoIP 通話** | **backgroundVoIP** 功能可讓您自動接收和接聽 VoIP 來電，而不需要使用者明確接受通話。 使用這項功能的應用程式獲得攝影機及麥克風的完整控制權，並且可以在背景使用這些資源。<br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 大部分的開發人員都不會獲得核准使用這項功能。 |
| **保留 VoIP 通話的資源** | **oneProcessVoIP** 功能可讓您在單一進程應用程式中，保留 VoIP 通話所需的 CPU 和記憶體資源。<br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 大部分的開發人員都不會獲得核准使用這項功能。 |
| **開發模式網路** | 呼叫 C++/CX UWP 應用程式或 C++ Windows 執行階段元件中的 OpenFile Win32 API 時，**developmentModeNetwork** 功能可讓應用程式使用已登入使用者的認證存取網路路徑。 <br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **廣泛的檔案系統存取** | **broadFileSystemAccess** 功能可讓應用程式在沒有任何其他檔案選擇器樣式提示的情況下，於執行階段期間取得和目前正在執行應用程式的使用者同樣的檔案系統存取權。 請務必注意，存取使用者已使用 FilePicker 或 FolderPicker 選擇的檔案時，不需要這項功能。<br/><br/>這項功能適用於 [Windows.Storage](https://docs.microsoft.com/uwp/api/windows.storage) API。 由於使用者隨時都可以在 [設定] 中授與或拒絕權限，您應該確定應用程式針對那些變更具備復原性。 在 2018 年 4 月更新中，該權限的預設值為 [開啟]。 在 2018 年 10 月更新中，預設值為 [關閉]。 同樣重要的是，請勿宣告任何使用這項功能的特殊資料夾功能，例如 [文件]、[圖片] 或 [影片]。 您可以藉由將 **broadFileSystemAccess** 新增至資訊清單，在您的應用程式中啟用這項功能。 如需範例，請參閱[檔案存取權限](/windows/uwp/files/file-access-permissions)一文。<br/><br/>**注意：** Xbox 不支援這項功能。 |
| **系統韌體和 BIOS** | **smbios** 功能讓應用程式能夠存取 BIOS 資料和系統韌體資料。 |
| **完全信任權限層級** | **runFullTrust** 受限功能可讓應用程式在使用者電腦上以完全信任的權限層級執行。 必須具備這項功能，才能使用 [FullTrustProcessLauncher](https://docs.microsoft.com/uwp/api/windows.applicationmodel.fulltrustprocesslauncher) API。<br /><br />任何以 appx 或 msix 套件形式提供的傳統型應用程式 (如[傳統型橋接器](https://developer.microsoft.com/windows/bridges/desktop)) 都需要這項功能，而且當使用 Desktop App Converter (DAC) 或 Visual Studio 封裝這些應用程式時，它會自動顯示在您的資訊清單中。 |
| **提升權限** | **allowElevation** 受限功能可讓 Microsoft 合作夥伴和企業所建立的應用程式，保留在啟動時或在應用程式存留期期間需要自動提升權限的現有傳統型功能。<br/><br/>我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 只有在企業透過商務用 Microsoft Store 將營運應用程式部署到其私人存放區的情況下，才會核准此功能。  |
| **Windows Teams 裝置認證** | **teamEditionDeviceCredential** 受限功能可讓應用程式在執行 Windows 10 1703 版或更新版本的 Surface Hub 裝置上，存取要求裝置帳號認證的 API。<br/><br/>我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **Windows Teams 應用程式檢視** | **teamEditionView** 受限功能可讓應用程式存取 API，以便在執行 Windows 10 版本 1703 或更新版本的 Surface Hub 裝置上裝載應用程式檢視。<br/><br/>我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **相機處理延伸模組** | **cameraProcessingExtension** 受限功能可讓應用程式處理從相機擷取的影像，而不需要直接控制相機。<br /><br />需要具備這個功能，才能呼叫 [Windows.Devices.PointOfService.Provider](/uwp/api/windows.devices.pointofservice.provider) 命名空間中的 API。<br /><br />任何人都可能要求存取這個功能以進行市集提交。 |
| **資料使用方式管理** | **networkDataUsageManagement** 受限功能可讓應用程式收集網路資料使用資訊。<br /><br />需要有這項功能，才能呼叫 [GetAttributedNetworkUsageAsync](/uwp/api/windows.networking.connectivity.connectionprofile.getattributednetworkusageasync)。<br /><br />任何人都可能要求存取這個功能以進行市集提交。 |
| **管理電話線連線能力** | **phoneLineTransportManagement** 功能可讓應用程式管理負責電話線連線的系統裝置。<br /><br />需要具備這個功能，才能使用 [Windows.ApplicationModel.Calls](https://docs.microsoft.com/uwp/api/windows.applicationmodel.calls) 命名空間中的 PhoneLineTransportDevice API。 |
| **未虛擬化的資源** | **unvirtualizedResources** 受限功能可讓您的應用程式在其封裝資訊清單中宣告 [RegistryWriteVirtualization](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-registrywritevirtualization) 和 [FileSystemWriteVirtualization](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-filesystemwritevirtualization) 元素，以停用登錄和檔案系統的虛擬化。 這些宣告會防止系統將任何對 HKEY_CURRENT_USER 或對使用者 AppData 資料夾的寫入分別虛擬化。 當您的應用程式需要其他應用程式讀取或寫入與您的應用程式相同的登錄或檔案系統項目時，這會很有用。<br /><br />這項功能是針對由 Microsoft 和我們的合作夥伴發佈的幾種特定傳統型電腦遊戲所設計。 其並不適合用於其他案例，因為可能會危害系統完全解除安裝的能力。 |
| **可修改的應用程式** | **modifiableApp** 受限功能可讓您的應用程式在其封裝資訊清單中宣告 [windows.mutablePackageDirectories](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-package-extension) 延伸模組。 這使得您可以為您的應用程式預期有修改後檔案或新增檔案的資料夾提供名稱。 OS 會建立此資料夾，並讓您的應用程式使用此資料夾中的檔案，而不是使用 (或額外再使用) 應用程式原本安裝的檔案。<br /><br />這項功能是針對由 Microsoft 和我們的合作夥伴發佈的幾種特定傳統型電腦遊戲所設計。 因為此功能允許未簽署的程式碼執行，故不會核准其用在其他案例。 |
| **套件寫入重新導向相容性填充碼** | **packageWriteRedirectionCompatibilityShim** 受限功能會將您的應用程式設定為在每個使用者的位置建立所有新檔案。 系統會將為了寫入而開啟的任何預先存在檔案都先複製到每個使用者的位置，然後修改這些位置中的檔案。 這項功能適用於可在其安裝資料夾中建立或修改檔案的應用程式。<br /><br />這項功能是針對由 Microsoft 和我們的合作夥伴發佈的幾種特定傳統型電腦遊戲所設計。 不過，在某些情況下，它也可能適用於其他應用程式。 |
| **自訂安裝動作** | **customInstallActions** 受限功能可讓您的應用程式在其封裝資訊清單中宣告 [windows.customInstall](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-package-extension) 延伸模組，讓它可以指定一或多個與您的應用程式一起執行的額外安裝程式檔案 (.exe 或 .msi)。 這可讓您為任何標準部署案例指定自訂動作：安裝、更新、修復或解除安裝。 例如，這適用於搭售協力廠商可轉散發元件的應用程式。<br /><br />這項功能是針對由 Microsoft 和我們的合作夥伴發佈的幾種特定傳統型電腦遊戲所設計。 不會核准其用在其他案例。 |
| **封裝的服務** | **packagedServices** 受限功能可讓 Microsoft 合作夥伴和企業所建立的應用程式在其封裝資訊清單中宣告 [windows.service](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-extension) 延伸模組，使其可以與應用程式一起安裝一或多個服務。 這些服務可以設定為在本機服務、網路服務或本機系統帳戶下執行。 本機服務和網路服務服務只需要 **packagedServices** 功能。 本機系統服務需要 **packagedServices** 和 **localSystemServices** 功能。<br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。  |
| **本機系統服務** | **localSystemServices** 受限功能可讓 Microsoft 合作夥伴和企業所建立的應用程式與應用程式一起安裝一或多個本機系統服務 (也就是，您的應用程式可以將服務的 StartAccount 宣告為 LocalSystem)。 此案例也需要 **packagesServices** 功能。 <br /><br />我們不建議您在提交至 Microsoft Store 的應用程式中宣告這項功能。 在大部分情況下，不會核准此功能的使用。 |
| **背景空間感知** | **backgroundSpatialPerception** 受限功能可讓應用程式在背景執行時仍能存取使用者的頭、手、動作控制器和其他追蹤物件的移動。 |

## <a name="custom-capabilities"></a>自訂功能

前面的[受限功能](#restricted-capabilities)一節描述的功能核准程序，也是您用來要求核准使用自訂功能的程序。 [embedded SIM](/uwp/api/windows.networking.networkoperators.esim) API 是需要自訂功能的 API 範例。 如果您只想要以開發人員模式在本機執行應用程式，則不需要自訂功能。 但是若要將您的應用程式發佈至 Microsoft Store 或在開發人員模式之外執行，您就需要自訂功能。

如果您有 Windows 技術支援專案經理 (TAM)，則可以與 TAM 合作要求存取權。 您可以在[聯絡 Microsoft TAM](/windows-hardware/drivers/mobilebroadband/testing-your-desktop-cosa-apn-database-submission#contact-your-microsoft-tam) 找到更多詳細資料。

若要宣告自訂功能，請修改您的[應用程式封裝資訊清單](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest)來源檔案 (`Package.appxmanifest`)。 新增 **xmlns:uap4** XML 命名空間宣告，並在宣告自訂功能時使用 **uap4** 前置詞。 以下是範例。

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package
    ...
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4">
...
<Capabilities>
    <uap4:CustomCapability Name="CompanyName.customCapabilityName_PublisherID"/>
</Capabilities>
</Package>
```

> [!NOTE]
> 在封裝資訊清單中的 **Capabilities** 節點底下，所有的 **CustomCapability** 元素都必須放在任何 **Capability** 元素之前、任何 [DeviceCapability](#device-capabilities) 元素之前。

## <a name="related-topics"></a>相關主題

* [提交選項](../publish/manage-submission-options.md)
* [如何在封裝資訊清單中指定功能](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-specify-capabilities-in-a-package-manifest)
* [如何在封裝資訊清單中指定裝置功能](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-specify-device-capabilities-in-a-package-manifest)
