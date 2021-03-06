---
ms.assetid: 374D1983-60E0-4E18-ABBB-04775BAA0F0D
title: 從您的 app 掃描
description: 在此處了解如何使用平台、送紙器或自動設定的掃描來源，來掃描 app 的內容。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1a314d0acdc3df1e0b53b1d78445b6ab1b71bf92
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369740"
---
# <a name="scan-from-your-app"></a>從您的 app 掃描


**重要的 Api**

-   [**Windows.Devices.Scanners**](https://docs.microsoft.com/uwp/api/Windows.Devices.Scanners)
-   [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation)
-   [**DeviceClass**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceClass)

在此處了解如何使用平台、送紙器或自動設定的掃描來源，來掃描 app 的內容。

**重要**   [ **Windows.Devices.Scanners** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Scanners) Api 屬於桌面[裝置系列](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)。 應用程式可以使用這些 Api，只會針對 Windows 10 桌面版本。

如果要從 app 掃描，您必須先透過宣告新的 [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 物件並取得 [**DeviceClass**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceClass) 類型以列出可用的掃描器。 只有在本機連同 WIA 驅動程式一起安裝的掃描器會列出，並且可供您的 app 使用。

在您的 app 列出可用的掃描器後，即可依據掃描器類型使用自動設定的掃描設定值，或僅使用可用的平台或送紙器掃描來源進行掃描。 如果要使用自動設定的設定，必須啟用掃描器的自動設定，而且不得同時配備平台與送紙器掃描器。 如需詳細資訊，請參閱[自動設定的掃描](https://docs.microsoft.com/windows-hardware/drivers/image/auto-configured-scanning)。

## <a name="enumerate-available-scanners"></a>列舉可用的掃描器

Windows 不會自動偵測掃描器。 您必須執行此步驟，應用程式才能和掃描器通訊。 在此範例中，掃描器裝置列舉是使用 [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) 命名空間完成的。

1.  首先，使用陳述式將這些新增至您的類別定義檔案。

``` csharp
    using Windows.Devices.Enumeration;
    using Windows.Devices.Scanners;
```

2.  接著實作裝置監控程式以開始列舉掃描器。 如需詳細資訊，請參閱[列舉裝置](enumerate-devices.md)。

```csharp
    void InitDeviceWatcher()
    {
       // Create a Device Watcher class for type Image Scanner for enumerating scanners
       scannerWatcher = DeviceInformation.CreateWatcher(DeviceClass.ImageScanner);

       scannerWatcher.Added += OnScannerAdded;
       scannerWatcher.Removed += OnScannerRemoved;
       scannerWatcher.EnumerationCompleted += OnScannerEnumerationComplete;
    }
```

3.  建立在新增掃描器時使用的事件處理常式。

```csharp
    private async void OnScannerAdded(DeviceWatcher sender,  DeviceInformation deviceInfo)
    {
       await
       MainPage.Current.Dispatcher.RunAsync(
             Windows.UI.Core.CoreDispatcherPriority.Normal,
             () =>
             {
                MainPage.Current.NotifyUser(String.Format("Scanner with device id {0} has been added", deviceInfo.Id), NotifyType.StatusMessage);

                // search the device list for a device with a matching device id
                ScannerDataItem match = FindInList(deviceInfo.Id);

                // If we found a match then mark it as verified and return
                if (match != null)
                {
                   match.Matched = true;
                   return;
                }

                // Add the new element to the end of the list of devices
                AppendToList(deviceInfo);
             }
       );
    }
```

## <a name="scan"></a>Scan

1.  **取得 ImageScanner 物件**

對於每個 [**ImageScannerScanSource**](https://docs.microsoft.com/uwp/api/Windows.Devices.Scanners.ImageScannerScanSource) 列舉類型，無論其為 **Default**、**AutoConfigured**、**Flatbed** 或 **Feeder**，您都必須先呼叫 [**ImageScanner.FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.scanners.imagescanner.fromidasync) 方法以建立一個 [**ImageScanner**](https://docs.microsoft.com/uwp/api/Windows.Devices.Scanners.ImageScanner) 物件，就像這樣。

 ```csharp
    ImageScanner myScanner = await ImageScanner.FromIdAsync(deviceId);
 ```

2.  **只要掃描**

為使用預設值掃描，您的應用程式需要 [**Windows.Devices.Scanners**](https://docs.microsoft.com/uwp/api/Windows.Devices.Scanners) 命名空間才能選取掃描器並從該來源掃描。 掃描設定不會變更。 可能的掃描器有自動設定、平台或送紙器。 此類型的掃描最有可能產生成功的掃描作業，即使是從錯誤的來源 (例如平台而非送紙器) 掃描也一樣。

**附註**  如果使用者將送紙器中掃描的文件，掃描器會從平台移改為掃描。 如果使用者嘗試從空白的送紙器掃描，掃瞄工作將不會產生任何掃描的檔案。
 
```csharp
    var result = await myScanner.ScanFilesToFolderAsync(ImageScannerScanSource.Default,
        folder).AsTask(cancellationToken.Token, progress);
```

3.  **從自動設定的掃描、 平台或送紙器來源**

您可以使用裝置的[自動設定的掃描](https://docs.microsoft.com/windows-hardware/drivers/image/auto-configured-scanning)，以最佳的掃描設定執行掃描。 使用此選項時，裝置本身可依據正在掃描的內容來判斷最佳的掃描設定，例如色彩模式與掃描解析度。 裝置會在執行階段為每個新的掃描工作選取掃描設定。

**附註**  並非所有的掃描器會支援此功能，因此應用程式必須確認掃描器是否支援此功能，才能使用此設定。

在此範例中，app 會先檢查掃描器是否可以自動設定，然後再進行掃描。 如果要指定平台或送紙器掃描器，只要將 **AutoConfigured** 取代成 **Flatbed** 或 **Feeder**。

```csharp
    if (myScanner.IsScanSourceSupported(ImageScannerScanSource.AutoConfigured))
    {
        ...
        // Scan API call to start scanning with Auto-Configured settings.
        var result = await myScanner.ScanFilesToFolderAsync(
            ImageScannerScanSource.AutoConfigured, folder).AsTask(cancellationToken.Token, progress);
        ...
    }
```

## <a name="preview-the-scan"></a>預覽掃描

在掃描到資料夾之前，您可以新增預覽掃描的程式碼。 在下列範例中，app 會檢查 **Flatbed** 掃描器是否支援預覽，然後再預覽掃描。

```csharp
if (myScanner.IsPreviewSupported(ImageScannerScanSource.Flatbed))
{
    rootPage.NotifyUser("Scanning", NotifyType.StatusMessage);
                // Scan API call to get preview from the flatbed.
                var result = await myScanner.ScanPreviewToStreamAsync(
                    ImageScannerScanSource.Flatbed, stream);
```

## <a name="cancel-the-scan"></a>取消掃描

您可以讓使用者在掃描中途取消掃描工作，就像這樣。

```csharp
void CancelScanning()
{
    if (ModelDataContext.ScenarioRunning)
    {
        if (cancellationToken != null)
        {
            cancellationToken.Cancel();
        }                
        DisplayImage.Source = null;
        ModelDataContext.ScenarioRunning = false;
        ModelDataContext.ClearFileList();
    }
}
```

## <a name="scan-with-progress"></a>掃描時顯示進度

1.  建立 **System.Threading.CancellationTokenSource** 物件。

```csharp
cancellationToken = new CancellationTokenSource();
```

2.  設定進度事件處理常式，以取得掃描的進度。

```csharp
    rootPage.NotifyUser("Scanning", NotifyType.StatusMessage);
    var progress = new Progress<UInt32>(ScanProgress);
```

## <a name="scanning-to-the-pictures-library"></a>掃描到圖片媒體櫃

使用者可使用 [**FolderPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FolderPicker) 類別動態地掃描至任一資料夾，但您必須在資訊清單中宣告「圖片庫」  功能，以允許使用者掃描至該資料夾。 如需 app 功能的詳細資訊，請參閱 [App 功能宣告](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)。
