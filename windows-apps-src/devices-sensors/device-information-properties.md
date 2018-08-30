---
author: muhsinking
ms.assetid: 4A4C2802-E674-4C04-8A6D-D7C1BBF1BD20
title: 裝置資訊屬性
description: 每個裝置都具有相關聯的 DeviceInformation 屬性，您可以在需要特定資訊或正在建置裝置選取器時使用。
ms.author: mukin
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: c8fe51fd98f70e6f920a7421a9932e69bba11377
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2018
ms.locfileid: "959244"
---
# <a name="device-information-properties"></a>裝置資訊屬性



**重要 API**

- [**Windows.Devices.Enumeration**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration)

每個裝置都具有相關聯的 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 屬性，您可以在需要特定資訊或正在建置裝置選取器時使用。 這些屬性可以指定為 AQS 篩選器來限制您正在列舉的裝置，以便尋找具有指定特性的裝置。 您也可以使用這些屬性，來指定要針對每個裝置傳回的資訊。 這可讓您指定要傳回給應用程式的裝置資訊。

如需在裝置選取器中使用 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 屬性的詳細資訊，請參閱[建置裝置選取器](build-a-device-selector.md)。 本主題說明如何要求資訊屬性，同時也會列出一些常見的屬性及其用途。

[**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 物件是由一個身分識別 ([**DeviceInformation.Id**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.id))、一個類型 ([**DeviceInformation.Kind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.kind.aspx)) 及一個屬性包 ([**DeviceInformation.Properties**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.properties.aspx)) 所組成。 **DeviceInformation** 物件的所有其他屬性均衍生自 **Properties** 屬性包。 例如，[**Name**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.name) 是衍生自 **System.ItemNameDisplay**。 這表示屬性包一律包含判斷其他屬性所需的資訊。

## <a name="requesting-properties"></a>正在要求屬性

[**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 物件擁有一些基本屬性，例如 [**Id**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.id) 和 [**Kind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.kind.aspx)，但是大部分的屬性會儲存在 [**Properties**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.properties.aspx) 底下的屬性包中。 因此，屬性包會包含用來從屬性包找出屬性的屬性。 例如，使用 [System.ItemNameDisplay](https://msdn.microsoft.com/library/windows/desktop/Bb760770) 來找出 [**Name**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.name) 屬性。 這是具有使用者易記名稱的通用和已知屬性的案例。 Windows 提供數個使用者易記的名稱，讓查詢屬性更加容易。

當您正在要求屬性時，並不會受限於具有使用者易記名稱的通用屬性。 您可以指定基本的 GUID 和屬性識別碼 (PID) 來要求任何可用的屬性，甚至是個別裝置或驅動程式所提供的自訂屬性。 指定自訂屬性的格式是 "`{GUID} PID`"。 例如："`{744e3bed-3684-4e16-9f8a-07953a8bf2ab} 7`"。 

> [!Note]
> 您可以找到屬性 Guid 的清單中的裝置驅動程式裝置屬性索引鍵的標頭檔。

某些屬性在所有 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind) 物件中是通用的，但大部分都是特定類型獨有的。 下列區段會列出一些按個別 **DeviceInformationKind** 排序的通用屬性。 如需不同類型如何彼此關聯的詳細資訊，請參閱 **DeviceInformationKind**。

## <a name="deviceinterface-properties"></a>DeviceInterface 屬性

**DeviceInterface** 是預設值，並且是 app 案例中最常使用的 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind) 物件。 除非裝置 API 指出不同的特定 **DeviceInformationKind**，否則這是您應該使用的物件類型。

| 名稱                                  | 類型    | 說明                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **System.Devices.ContainerId**        | GUID    | **DeviceInformationKind.DeviceContainer** 的身分識別，其中包含涵蓋這個 **DeviceInterface** 的 **Device**。 您可以將此值連同 **DeviceInformationKind.DeviceContainer** 傳遞至 [**CreateFromIdAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.createfromidasync)，以尋找適當的容器。                                                                                    |
| **System.Devices.InterfaceClassGuid** | GUID    | 此介面代表的介面類別 GUID。                                                                                                                                                                                                                                                                                                                                                       |
| **System.Devices.DeviceInstanceId**   | 字串  | 父項 **DeviceInformationKind.Device** 的身分識別。 您可以將此值連同 **DeviceInformationKind.Device** 傳遞至 [**CreateFromIdAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.createfromidasync)，以尋找適當的裝置。                                                                                                                                                                   |
| **System.Devices.InterfaceEnabled**   | 布林值 | 表示介面是否已啟用。 [**DeviceInformation.IsEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.isenabled) 衍生自此屬性。                                                                                                                                                                                                                                                           |
| **System.Devices.GlyphIcon**          | 字串  | 字符的圖示路徑。                                                                                                                                                                                                                                                                                                                                                                                  |
| **System.Devices.IsDefault**          | 布林值 | 表示這是否是 **System.Devices.InterfaceClassGuid** 的預設裝置。 這通常用於印表機。 由於有多個音訊預設值，因此這不適用於音訊。 使用 [**GetDefaultAudioRenderId**](https://msdn.microsoft.com/library/windows/apps/BR226819) 或 [**GetDefaultAudioCaptureId**](https://msdn.microsoft.com/library/windows/apps/BR226818) 來取得音訊預設值。 |
| **System.Devices.Icon**               | 字串  | 圖示路徑。                                                                                                                                                                                                                                                                                                                                                                                                |
| **System.ItemNameDisplay**            | 字串  | 裝置物件的最佳顯示名稱。                                                                                                                                                                                                                                                                                                                                                              |

 

## <a name="device-properties"></a>裝置屬性

| 名稱                                  | 類型       | 說明                                                                                                                                                                                                                                                                              |
|---------------------------------------|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **System.Devices.ClassGuid**          | GUID       | 裝置安裝期間使用的裝置類別。 如需詳細資訊，請參閱[裝置安裝類別](https://msdn.microsoft.com/library/windows/hardware/Ff541509)。                                                                                                                                                            |
| **System.Devices.CompatibleIds**      | String\[\] | 裝置的相容識別碼。 Windows 判斷要安裝在裝置上的最佳驅動程式時會用到這些資訊。 如需詳細資訊，請參閱[相容識別碼](https://msdn.microsoft.com/library/windows/hardware/Ff539950)。                                                                                                |
| **System.Devices.ContainerId**        | GUID       | 包含此裝置之 **DeviceInformationKind.DeviceContainer** 的身分識別。 您可以將此值連同 **DeviceInformationKind.DeviceContainer** 傳遞至 [**CreateFromIdAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.createfromidasync)，以尋找適當的容器。          |
| **System.Devices.DeviceCapabilities** | UInt32     | 在 **CfgMgr32.h** 中定義的 CM\_DEVCAP\_X 功能旗標的位元 OR。 如需詳細資訊，請參閱 [**DEVPKEY\_Device\_Capabilities**](https://msdn.microsoft.com/library/windows/hardware/Ff542373)。                                                                                             |
| **System.Devices.DeviceHasProblem**   | 布林值    | 裝置目前出現問題且很可能無法正常運作。 這可能是因為過期、遺漏或不正確的驅動程式所導致。                                                                                                                                                |
| **System.Devices.DeviceInstanceId**   | 字串     | 裝置的身分識別。 這也會是 [**DeviceInformation.Id**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.id) 的值。                                                                                                                                                                       |
| **System.Devices.DeviceManufacturer** | 字串     | 裝置的製造商。                                                                                                                                                                                                                                                          |
| **System.Devices.HardwareIds**        | String\[\] | 裝置的硬體識別碼。 Windows 在判斷要安裝的最佳驅動程式時會用到這些識別碼。 裝置廠商可以使用這個屬性，從他們的 app 中識別他們的裝置。 如需詳細資訊，請參閱[硬體識別碼](https://msdn.microsoft.com/library/windows/hardware/Ff546152)。                                         |
| **System.Devices.Parent**             | 字串     | 父項裝置的 [**DeviceInformation.Id**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.id)。 這是指連線父項，不是 **DeviceContainer** 父項。                                                                                                                                 |
| **System.Devices.Present**            | 布林值    | 表示裝置目前是否存在並可以使用。                                                                                                                                                                                                                         |
| **System.ItemNameDisplay**            | 字串     | 此裝置物件的最佳顯示名稱。 在此案例中，這不一定是最佳的使用者名稱。 藉由參考相關聯的 **DeviceContainer** 或 **DeviceInterface** 的 **System.ItemNameDisplay**，就能找到更有可能的使用者易記名稱候選項目。 |

 

## <a name="devicecontainer-properties"></a>DeviceContainer 屬性

| 名稱                              | 類型       | 說明                                                                                                                                                        |
|-----------------------------------|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **System.Devices.Category**       | String\[\] | 裝置所屬類別的說明清單。 這份清單會以單數類別的形式提供。 例如，「顯示器」、「電話」或「音訊裝置」。  |
| **System.Devices.CategoryIds**    | String\[\] | 包含此裝置所屬類別的清單。 例如，**Audio.Headphone**、**Display.Monitor** 或 **Input.Gaming**。                                  |
| **System.Devices.CategoryPlural** | String\[\] | 裝置所屬類別的說明清單。 這份清單會以複數類別的形式提供。 例如，「顯示器」、「電話」或「音訊裝置」。 |
| **System.Devices.CompatibleIds**  | String\[\] | 所有子項 **DeviceInformationKind.Device** 物件的相容識別碼集合。                                                                       |
| **System.Devices.Connected**      | 布林值    | 表示裝置目前是否與系統連線。                                                                                          |
| **System.Devices.GlyphIcon**      | 字串     | 字符的圖示路徑。                                                                                                                                           |
| **System.Devices.HardwareIds**    | String\[\] | 所有子項 **DeviceInformationKind.Device** 物件的硬體識別碼集合。                                                                         |
| **System.Devices.Icon**           | 字串     | 圖示路徑。                                                                                                                                                         |
| **System.Devices.LocalMachine**   | 布林值    | 如果這個 **DeviceContainer** 代表系統本身則為 **true**；如果裝置不在系統內則為 **false**。                                              |
| **System.Devices.Manufacturer**   | 字串     | 裝置的製造商。                                                                                                                                    |
| **System.Devices.ModelName**      | 字串     | 裝置容器的型號名稱。                                                                                                                                |
| **System.Devices.Paired**         | 布林值    | 表示目前已與系統配對的任何子項 **DeviceInformationKind.Device** 物件是無線還是網路裝置。             |
| **System.ItemNameDisplay**        | 字串     | 此裝置的最佳顯示名稱。                                                                                                                             |

 

## <a name="deviceinterfaceclass-properties"></a>DeviceInterfaceClass 屬性

| 名稱                       | 類型   | 說明                            |
|----------------------------|--------|----------------------------------------|
| **System.ItemNameDisplay** | 字串 | 此裝置的最佳顯示名稱。 |

 

## <a name="associationendpoint-properties"></a>AssociationEndpoint 屬性

| 名稱                                  | 類型       | 說明                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|---------------------------------------|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **System.Devices.Aep.AepId**          | 字串     | 此裝置的身分識別。 這也會是 [**DeviceInformation.Id**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.id) 的值。                                                                                                                                                                                                                                                                                                                                                                        |
| **System.Devices.Aep.CanPair**        | 布林值    | 表示裝置是否可與系統配對。 [**DeviceInformationPairing.CanPair**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationpairing.canpair.aspx) 衍生自此屬性。                                                                                                                                                                                                                                                                                                       |
| **System.Devices.Aep.Category**       | String\[\] | 裝置的所屬類別。 例如，印表機或相機。                                                                                                                                                                                                                                                                                                                                                                                                             |
| **System.Devices.Aep.ContainerId**    | GUID       | 父項 **AssociationEndpointContainer** 物件的識別碼。                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **System.Devices.Aep.DeviceAddress**  | 字串     | 裝置的位址。 如果裝置為網路裝置，則這會是 IP 位址。                                                                                                                                                                                                                                                                                                                                                                                                  |
| **System.Devices.Aep.IsConnected**    | 布林值    | 表示裝置目前是否與系統連線。                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **System.Devices.Aep.IsPaired**       | 布林值    | 表示裝置目前是否已配對。 [**DeviceInformationPairing.IsPaired**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationpairing.ispaired.aspx) 衍生自此屬性。                                                                                                                                                                                                                                                                                                                      |
| **System.Devices.Aep.IsPresent**      | 布林值    | 指出裝置目前是否存在，這表示該裝置存在且可透過網路或無線通訊協定來探索。 一旦裝置與系統配對後，系統便會快取裝置。 之後，查詢 **AssociationEndpoint** 物件時，系統便會自動探索裝置。 因此，您不能依賴只使用查詢來探索裝置，以指出裝置目前是否可供使用。 這就是為什麼這個屬性很重要的原因。 |
| **System.Devices.Aep.Manufacturer**   | 字串     | 裝置的製造商。                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| **System.Devices.Aep.ModelId**        | GUID       | 裝置的型號識別碼。                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| **System.Devices.Aep.ModelName**      | 字串     | 裝置的型號名稱。                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **System.Devices.Aep.ProtocolId**     | GUID       | 表示用來探索此 **AssocationEndpoint** 裝置的通訊協定。                                                                                                                                                                                                                                                                                                                                                                                                            |
| **System.Devices.Aep.SignalStrength** | Int32      | 裝置的訊號強度。 這個屬性僅適用於某些通訊協定。                                                                                                                                                                                                                                                                                                                                                                                                |
| **System.ItemNameDisplay**            | 字串     | 裝置的最佳顯示名稱。                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

 

## <a name="associationendpointcontainer-properties"></a>AssociationEndpointContainer 屬性

| 名稱                                                | 類型       | 說明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|-----------------------------------------------------|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **System.Devices.AepContainer.Categories**          | String\[\] | 裝置的所屬類別。 例如，印表機或相機。                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| **System.Devices.AepContainer.Children**            | String\[\] | 屬於這個容器之 **AssocationEndpoint** 物件的識別碼集合。                                                                                                                                                                                                                                                                                                                                                                                                                                |
| **System.Devices.AepContainer.CanPair**             | 布林值    | 表示其中一個子項 **AssociationEndpoint** 裝置是否可與系統配對。 [**DeviceInformationPairing.CanPair**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationpairing.canpair.aspx) 衍生自此屬性。                                                                                                                                                                                                                                                                                                       |
| **System.Devices.AepContainer.ContainerId**         | GUID       | 此裝置的身分識別。 這也會是 [**DeviceInformation.Id**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.id) 的值 (但採用 GUID 形式)。                                                                                                                                                                                                                                                                                                                                                                                            |
| **System.Devices.AepContainer.IsPaired**            | 布林值    | 表示目前是否已與其中一個子項 **AssociationEndpoint** 裝置配對。 [**DeviceInformationPairing.IsPaired**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationpairing.ispaired.aspx) 衍生自此屬性。                                                                                                                                                                                                                                                                                                                      |
| **System.Devices.AepContainer.IsPresent**           | 布林值    | 指出其中一個子項 **AssociationEndpoint** 裝置目前是否存在，這表示該裝置存在且可透過網路或無線通訊協定來探索。 一旦裝置與系統配對後，系統便會快取裝置。 之後，查詢 **AssociationEndpoint** 物件時，系統便會自動探索裝置。 因此，您不能依賴只使用查詢來探索裝置，以指出裝置目前是否可供使用。 這就是為什麼這個屬性很重要的原因。 |
| **System.Devices.AepContainer.Manufacturer**        | 字串     | 裝置的製造商。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| **System.Devices.AepContainer.ModelIds**            | String\[\] | 裝置的型號識別碼清單。 每個型號都是採用字串形式的 GUID。                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| **System.Devices.AepContainer.ModelName**           | 字串     | 裝置的型號名稱。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| **System.Devices.AepContainer.ProtocolIds**         | GUID\[\]   | 協助建置 **AssociationEndpointContainer** 物件的通訊協定識別碼清單。 請記住，**AssociationEndpointContainer** 裝置的建立方式是，針對同一個實體裝置收集所有透過不同通訊協定探索到的 **AssociationEndpoint** 裝置來建立。                                                                                                                                                                                                                           |
| **System.Devices.AepContainer.SupportedUriSchemes** | String\[\] | 此裝置支援的傳播 URI 配置清單。                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| **System.Devices.AepContainer.SupportsAudio**       | 布林值    | 表示此裝置是否支援音效傳播。                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| **System.Devices.AepContainer.SupportsImages**      | 布林值    | 表示此裝置是否支援影像傳播。                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| **System.Devices.AepContainer.SupportsVideo**       | 布林值    | 表示此裝置是否支援視訊傳播。                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| **System.ItemNameDisplay**                          | 字串     | 裝置的最佳顯示名稱。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |

 

## <a name="associationendpointservice-properties"></a>AssociationEndpointService 屬性

| 名稱                                            | 類型    | 說明                                                                                                      |
|-------------------------------------------------|---------|------------------------------------------------------------------------------------------------------------------|
| **System.Devices.AepService.AepId**             | 字串  | 父項 **AssociationEndpoint** 物件的識別碼。                                                     |
| **System.Devices.AepService.ContainerId**       | GUID    | 父項 **AssociationEndpointContainer** 物件的識別碼。                                            |
| **System.Devices.AepService.ParentAepIsPaired** | 布林值 | 表示父項 **AssociationEndpoint** 物件是否已與系統配對。                           |
| **System.Devices.AepService.ProtocolId**        | GUID    | 用來探索此裝置的通訊協定身分識別。                                                           |
| **System.Devices.AepService.ServiceClassId**    | GUID    | 此裝置所代表的服務身分識別。                                                             |
| **System.Devices.AeoService.ServiceId**         | 字串  | 此服務的身分識別。 這也會是 [**DeviceInformation.Id**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.id) 的值。 |
| **System.ItemNameDisplay**                      | 字串  | 服務的最佳顯示名稱。                                                                           |

 

 

 