---
ms.assetid: D06AA3F5-CED6-446E-94E8-713D98B13CAA
title: 建置裝置選取器
description: 建置裝置選取器可讓您在列舉裝置時限制要搜尋的裝置。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 67d83a66687bb8719dc374a2a8a3e30eaac82c71
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684829"
---
# <a name="build-a-device-selector"></a>建置裝置選取器



**重要 API**

- [**Windows. 列舉**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration)

建置裝置選取器可讓您在列舉裝置時限制要搜尋的裝置。 這讓您能夠只獲得相關結果，並提高系統的效能。 在大部分情況下，您可以從裝置堆疊中取得裝置選取器。 例如，您可以在透過 USB 找到的裝置中使用 [**GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.usb.usbdevice.getdeviceselector)。 這些裝置選取器會傳回進階查詢語法 (AQS) 字串。 如果您不熟悉 AQS 格式，您可以在[透過程式設計方式使用進階查詢語法](https://docs.microsoft.com/windows/desktop/search/-search-3x-advancedquerysyntax)中閱讀更多內容。

## <a name="building-the-filter-string"></a>建置篩選字串

有一些情況是，您需要列舉裝置，但提供的裝置選取器不適用您的案例。 裝置選取器為 AQS 篩選字串，其中包含下列資訊。 在建立篩選字串之前，您需要知道您想要列舉之裝置相關資訊中的某些重要部分。

-   您感興趣之裝置的 [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)。 如需 **DeviceInformationKind** 如何影響列舉裝置的詳細資訊，請參閱[列舉裝置](enumerate-devices.md)。
-   建置 AQS 篩選字串的方式，如本主題所述。
-   您感興趣的屬性。 可用的屬性將取決於 [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)。 如需詳細資訊，請參閱[裝置資訊屬性](device-information-properties.md)。
-   您用來進行查詢的通訊協定。 唯有當您是透過無線或有線網路搜尋裝置時，才需要執行此動作。 如需執行此動作的詳細資訊，請參閱[列舉網路上的裝置](enumerate-devices-over-a-network.md)。

使用 [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) API 時，您經常會將裝置選取器與您感興趣的裝置類型組合在一起。 裝置類型的可用清單是由 [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) 列舉所定義。 這個規格組合可協助您限制可供您感興趣之裝置使用的裝置。 如果未指定 **DeviceInformationKind**，或您使用的方法不提供 **DeviceInformationKind** 參數，則預設類型是 **DeviceInterface**。

[  **Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) API 會使用標準 AQS 語法，但未支援所有的運算子。 如需可在建構篩選字串時使用的屬性清單，請參閱[裝置資訊屬性](device-information-properties.md)。

**注意  ：** 當您 AQS 篩選字串時，不能使用 `{GUID} PID` 格式所定義的自訂屬性。 這是因為屬性類型是衍生自已知的屬性名稱。

 

下表列出 AQS 運算子及其支援的參數類型。

| 運算子                       | 支援的類型                                                             |
|--------------------------------|-----------------------------------------------------------------------------|
| **COP\_相等**                 | 字串、布林值、GUID、UInt16、UInt32                                       |
| **COP\_NOTEQUAL**              | 字串、布林值、GUID、UInt16、UInt32                                       |
| **COP\_LESSTHAN**              | UInt16、UInt32                                                              |
| **COP\_GREATERTHAN**           | UInt16、UInt32                                                              |
| **COP\_LESSTHANOREQUAL**       | UInt16、UInt32                                                              |
| **COP\_GREATERTHANOREQUAL**    | UInt16、UInt32                                                              |
| **COP\_值\_包含**       | 字串、字串陣列、布林值陣列、GUID 陣列、UInt16 陣列、UInt32 陣列 |
| **COP\_值\_NOTCONTAINS**    | 字串、字串陣列、布林值陣列、GUID 陣列、UInt16 陣列、UInt32 陣列 |
| **COP\_值\_STARTSWITH**     | 字串                                                                      |
| **COP\_值\_ENDSWITH**       | 字串                                                                      |
| **COP\_DOSWILDCARDS**          | 不受支援                                                               |
| **COP\_WORD\_相等**           | 不受支援                                                               |
| **COP\_WORD\_STARTSWITH**      | 不受支援                                                               |
| **COP\_應用程式\_特定** | 不受支援                                                               |


> **提示**  您可以為**COP\_相等**或**COP\_NOTEQUAL**指定**Null** 。 這會轉譯為沒有值或值不存在的屬性。 在 AQS 中，您可以使用空的括弧 \[\]來指定**Null** 。

> **重要**  使用**COP\_值\_CONTAINS**和**COP\_值\_NOTCONTAINS**運算子時，它們的行為會與字串和字串陣列不同。 如果是字串，系統會執行不區分大小寫的搜尋，以確認裝置是否包含做為子字串的指定字串。 如果是字串陣列，系統不會搜尋子字串。 在字串陣列中，系統會搜尋陣列以確認它是否包含整個指定字串。 您無法搜尋字串陣列來確認陣列中的元素是否包含子字串。

如果您無法建立單一 AQS 篩選字串以適當包含您的結果範圍，則可在收到結果之後進行篩選。 不過，如果您選擇這樣做，建議您在將初始 AQS 篩選字串提供給 [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) API 時，盡可能限制該字串所產生的結果。 這將有助於改善應用程式的效能。

## <a name="aqs-string-examples"></a>AQS 字串範例

下列範例說明如何使用 AQS 語法來限制您要列舉的裝置。 所有的這些篩選字串都會與 [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) 配對，以建立完整篩選器。 如果未指定類型，請記住預設的類型是 **DeviceInterface**。

當這個篩選器與 **DeviceInterface** 的 [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) 搭配時，它會列舉包含音訊擷取介面類別且目前已啟用的所有物件。 **=** 會轉譯為**COP\_EQUALS**。

``` syntax
System.Devices.InterfaceClassGuid:="{2eef81be-33fa-4800-9670-1cd474972c3f}" AND
System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True
```

當這個篩選器與 **Device** 的 [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) 搭配時，它會列舉包含至少一個 GenCdRom 硬體識別碼的所有物件。 **~~** 會轉譯為**COP\_值\_包含**。

``` syntax
System.Devices.HardwareIds:~~"GenCdRom"
```

當這個篩選器與 **DeviceContainer** 的 [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) 搭配時，它會列舉型號名稱包含子字串 Microsoft 的所有物件。 **~~** 會轉譯為**COP\_值\_包含**。

``` syntax
System.Devices.ModelName:~~"Microsoft"
```

當這個篩選器與 **DeviceInterface** 的 [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) 搭配時，它會列舉名稱以子字串 Microsoft 開頭的所有物件。 **~&lt;** 會轉譯為**COP\_STARTSWITH**。

``` syntax
System.ItemNameDisplay:~<"Microsoft"
```

當這個篩選器與 **Device** 的 [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) 搭配時，它會列舉包含 **System.Devices.IpAddress** 屬性集的所有物件。 **&lt;&gt;\[\]** 會轉譯成與**Null**值結合的**COP\_NOTEQUALS** 。

``` syntax
System.Devices.IpAddress:<>[]
```

當這個篩選器與 **Device** 的[**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) 搭配時，它會列舉不包含 **System.Devices.IpAddress** 屬性集的所有物件。 **=\[\]** 轉譯為**COP\_等於**與**Null**值結合。

``` syntax
System.Devices.IpAddress:=[]
```

 

 
