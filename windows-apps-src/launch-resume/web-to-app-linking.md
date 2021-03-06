---
title: 使用 App URI 處理常式啟用網站的應用程式
description: 透過支援 [網站的應用程式] 功能，讓使用者持續使用您的 App。
keywords: 深層連結 Windows
ms.date: 08/25/2017
ms.topic: article
ms.assetid: 260cf387-88be-4a3d-93bc-7e4560f90abc
ms.localizationpriority: medium
ms.openlocfilehash: 5807cdc19e4b38c8cc8fa4ca45c4ef47e79b7742
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/11/2019
ms.locfileid: "72282256"
---
# <a name="enable-apps-for-websites-using-app-uri-handlers"></a>使用 App URI 處理常式啟用網站的應用程式

\[網站的應用程式\] 將您的應用程式關聯至網站，以便當某人開啟您網站的連結時，會啟動您的應用程式而不是開啟瀏覽器。 如果未安裝您的應用程式，則會像往常一樣在瀏覽器中開啟您的網站。 只有經過驗證的內容擁有者可以註冊連結，因此使用者可以信任這項體驗。 使用者可以移至 [設定] > [應用程式] > [網站的應用程式]，檢查他們所有已註冊的網站至應用程式連結。

若要啟用網站至應用程式連結，您將需要：
- 在資訊清單檔案中識別應用程式將會處理的 URI
- 定義您的應用程式與您的網站之間關聯的 JSON 檔案。 將應用程式套件系列名稱，放在與應用程式資訊清單宣告的同一個主機根目錄。
- 在應用程式中處理啟用。

> [!Note]
> 從 Windows 10 建立者更新開始，在 Microsoft Edge 中按一下支援的連結將會啟動對應的應用程式。 在其他瀏覽器中按一下支援的連結（例如 Internet Explorer 等），將會讓您保持流覽體驗。

## <a name="register-to-handle-http-and-https-links-in-the-app-manifest"></a>登錄以處理應用程式資訊清單中的 http 和 https 連結

您的應用程式需要識別將會處理的網站 URI。 若要執行此動作，請將 **Windows.appUriHandler** 擴充功能註冊新增至應用程式的資訊清單檔案 **Package.appxmanifest** 中。

例如，若您網站的網址為 “msn.com”，您必須在應用程式資訊清單放入下列項目︰

```xml
<Applications>
  <Application ... >
      ...
      <Extensions>
         <uap3:Extension Category="windows.appUriHandler">
          <uap3:AppUriHandler>
            <uap3:Host Name="msn.com" />
          </uap3:AppUriHandler>
        </uap3:Extension>
      </Extensions>
  </Application>
</Applications>
```

上述的宣告會登錄您的應用程式要從指定的主機來處理連結。 如果您的網站有多個位址（例如： m.example.com、www\.example.com 和 example.com），請在 `<uap3:AppUriHandler>` 中為每個位址新增個別的 `<uap3:Host Name=... />` 專案。

## <a name="associate-your-app-and-website-with-a-json-file"></a>使用 JSON 檔案將應用程式與網站關聯

若要確保只有您的應用程式才能開啟您網站上的內容，請將您的應用程式套件系列名稱放入位在網頁伺服器根目錄中，或位在網域的已知目錄中的 JSON 檔案。 這表示您的網站同意所列出的應用程式開啟您網站上的內容。 您可以在應用程式資訊清單設計工具的 \[套件\] 區段中，找到套件系列名稱。

>[!Important]
> JSON 檔案不應該有.json 後置檔案。

建立名為 **windows-app-web-link** 的 JSON 檔案 (不含.json 副檔名)，並提供您的應用程式套件系列名稱。 例如：

``` JSON
[{
  "packageFamilyName": "Your app's package family name, e.g MyApp_9jmtgj1pbbz6e",
  "paths": [ "*" ],
  "excludePaths" : [ "/news/*", "/blog/*" ]
 }]
```

Windows 會讓 https 連線至您的網站，並會在網頁伺服器上尋找對應的 JSON 檔案。

### <a name="wildcards"></a>萬用字元

上面的 JSON 檔案範例示範萬用字元的用法。 萬用字元可讓您以較少行的程式碼，支援各種不同的連結。 網站至應用程式連結可在 JSON 檔案中支援兩種類型的萬用字元︰

| **模糊** | **描述**               |
|--------------|-------------------------------|
| **\***       | 代表任何子字串      |
| **?**        | 代表單一字元 |

例如 `"excludePaths" : [ "/news/*", "/blog/*" ]`，在上述範例中，您的應用程式將會支援以您的網站位址開頭的所有路徑（例如，msn.com），**但**`/news/` 和 `/blog/`底下的路徑除外。 將支援**msn.com/weather.html** ，但不會**msn.com/news/topnews.html**。

### <a name="multiple-apps"></a>多個應用程式

如果您想要連結到網站的 app 有兩個時，請在 **windows-app-web-link** JSON 檔案中，列出這兩個應用程式套件系列名稱。 即可支援這兩個應用程式。 如果兩者均已安裝，還會顯示選項讓使用者從中選擇預設連結。 如果他們稍後想要變更預設連結，可以在 **\[設定\] &gt; \[網站的應用程式\]** 變更。 開發人員也可以隨時變更 JSON 檔案並查看同一天的變更，但僅限更新後的八天內。

``` JSON
[{
  "packageFamilyName": "Your apps's package family name, e.g MyApp_9jmtgj1pbbz6e",
  "paths": [ "*" ],
  "excludePaths" : [ "/news/*", "/blog/*" ]
 },
 {
  "packageFamilyName": "Your second app's package family name, for example, MyApp2_8jmtgj2pbbz6e",
  "paths": [ "/example/*", "/links/*" ]
 }]
```

若要為使用者提供最佳的使用體驗，請使用排除路徑，務必從 JSON 檔案的支援路徑中排除僅供線上存取的內容。

排除路徑都會先行檢查，如果有相符項目，將會使用瀏覽器開啟對應的頁面，而不是使用指定的應用程式。 在上述範例中，'/news/\*' 包含該路徑下的任何頁面，而 '/news\*' （沒有正斜線軌跡 ' news '）則包含「新聞\*」下的任何路徑，例如 ' newslocal/'、' newsinternational/' 等等。

## <a name="handle-links-on-activation-to-link-to-content"></a>處理啟用以連結到內容的連結

在 app 的 Visual Studio 方案中，瀏覽到 **App.xaml.cs**，並在 **OnActivated()** 中對連結的內容新增處理。 在下列範例中，在應用程式中開啟的頁面取決於 URI 路徑︰

``` CS
protected override void OnActivated(IActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame == null)
    {
        ...
    }

    // Check ActivationKind, Parse URI, and Navigate user to content
    Type deepLinkPageType = typeof(MainPage);
    if (e.Kind == ActivationKind.Protocol)
    {
        var protocolArgs = (ProtocolActivatedEventArgs)e;        
        switch (protocolArgs.Uri.AbsolutePath)
        {
            case "/":
                break;
            case "/index.html":
                break;
            case "/sports.html":
                deepLinkPageType = typeof(SportsPage);
                break;
            case "/technology.html":
                deepLinkPageType = typeof(TechnologyPage);
                break;
            case "/business.html":
                deepLinkPageType = typeof(BusinessPage);
                break;
            case "/science.html":
                deepLinkPageType = typeof(SciencePage);
                break;
        }
    }

    if (rootFrame.Content == null)
    {
        // Default navigation
        rootFrame.Navigate(deepLinkPageType, e);
    }

    // Ensure the current window is active
    Window.Current.Activate();
}
```

**重要：** 務必要以 `if (rootFrame.Content == null)` 取代最終 的`rootFrame.Navigate(deepLinkPageType, e);` 邏輯，如上述範例中所示。

## <a name="test-it-out-local-validation-tool"></a>測試︰本機驗證工具

您可以執行主機登錄驗證工具來測試您的應用程式和網站設定，該工具可在下列位置取得︰

% windir%\\system32\\**AppHostRegistrationVerifier**

執行此工具時，可利用下列參數來測試應用程式與網站的設定：

**AppHostRegistrationVerifier .exe** *主機名稱 packagefamilyname filepath*

-   主機名稱：您的網站（例如，microsoft.com）
-   套件系列名稱 (PFN)：您的應用程式PFN
-   檔案路徑：本機驗證的 JSON 檔案（例如 C：\\SomeFolder\\windows-app-web 連結）

如果此工具未傳回任何項目，則會在上傳檔案時對該檔案進行驗證。 如果有錯誤碼，表示它無法運作。

您可以啟用下列登錄機碼，強制在本機驗證中對側載應用程式進行路徑比對：

`HKCU\Software\Classes\LocalSettings\Software\Microsoft\Windows\CurrentVersion\
AppModel\SystemAppData\YourApp\AppUriHandlers`

Keyname： `ForceValidation` 值： `1`

## <a name="test-it-web-validation"></a>測試︰Web 驗證

關閉您的應用程式，驗證當您按一下連結時會啟用該應用程式。 接著在您的網站中，複製其中一個支援路徑的網址。 例如，如果您的網站位址是 "msn.com"，且其中一個支援路徑為 "path1"，則您會使用 `http://msn.com/path1`

確認您的應用程式已經關閉。 按下 **Windows 鍵 + R** 以開啟 **\[執行\]** 對話方塊並在視窗中貼上連結。 您的應用程式應要啟動，而不是網頁瀏覽器。

此外，您可以使用 [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) API，測試從另一個應用程式來啟動您的應用程式。 您同樣可以使用這個 API，在手機上測試。

如果您想要依照通訊協定啟用邏輯，在 **OnActivated** 事件處理常式中設定中斷點。

## <a name="appurihandlers-tips"></a>AppUriHandlers 祕訣︰

- 請務必只指定您的應用程式能夠處理的連結。
- 列出所有您將會支援的主機。  請注意，www\.example.com 和 example.com 是不同的主機。
- 使用者可以在 \[設定\] 中，選擇他們想要哪個應用程式來處理網站。
- 您的 JSON 檔案必須上傳到 https 伺服器。
- 如果您需要變更所要支援的路徑，您可以重新發佈 JSON 檔案，而不需要重新發佈您的應用程式。 使用者能在 1-8 天內查看所做的變更。
- 所有使用 AppUriHandlers 側載的應用程式，在安裝時都會有該主機的驗證連結。 您不需要將 JSON 檔案上傳，也能測試該功能。
- 只要您的應用程式是利用 [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) 啟動的 UWP 應用程式，或使用 [ShellExecuteEx](https://docs.microsoft.com/windows/desktop/api/shellapi/nf-shellapi-shellexecuteexa) 啟動的 Windows 傳統型應用程式，此功能都能運作。 如果 URL 對應到已登錄的應用程式 URI 處理常式，會啟動該應用程式，而不是瀏覽器。

## <a name="see-also"></a>請參閱

[網站至應用程式範例專案](https://github.com/project-rome/AppUriHandlers/tree/master/NarwhalFacts)
[windows.protocol 註冊](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-protocol)
[處理 URI 啟用](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)
[關聯啟動範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AssociationLaunching)示範如何使用 LaunchUriAsync() API。
