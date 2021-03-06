---
description: 除了為您的應用程式建立將在 Windows 應用程式中執行的廣告行銷活動之外，您也可以使用其他管道促銷您的應用程式。
title: 建立自訂應用程式促銷活動
ms.assetid: 7C9BF73E-B811-4FC7-B1DD-4A0C2E17E95D
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, 自訂, 應用程式, 促銷, 行銷活動
ms.localizationpriority: medium
ms.openlocfilehash: 407a34294155e688e672db392c262e1607c01a39
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57653733"
---
# <a name="create-a-custom-app-promotion-campaign"></a>建立自訂應用程式促銷活動

除了為您的 app 建立將在 Windows app 中執行的[廣告活動](create-an-ad-campaign-for-your-app.md)之外，您也可以使用其他管道促銷您的 app。 例如，您可以使用第三方 app 行銷提供者促銷您的 app，或可以在社交媒體網站上張貼 app 的連結。 這些活動稱為*自訂行銷活動*。

如果為您的應用程式執行自訂行銷活動，可以追蹤每個行銷活動相關的效能，方法是為每個自訂行銷活動建立不同的 URL，其中每個 URL 包含不同的*行銷活動識別碼*。 當執行 Windows 10 的客戶按一下 URL，其中包含的活動識別碼時，Microsoft 關聯對應之自訂活動中按一下，並提供這項資料給您在[合作夥伴中心](https://partner.microsoft.com/dashboard)。

> [!IMPORTANT]
> 客戶只會追蹤這項資料在 Windows 10 上。 使用其他作業系統的客戶仍可以透過連結連到您的 app 清單，但不包含有關這些客戶的活動相關資料。

與自訂行銷活動關聯的資料類型主要有兩個：應用程式 Store 清單的 *「頁面檢視次數」*，以及 *「轉換」*。 「轉換」即是指客戶從包含自訂行銷活動識別碼的 URL，檢視您應用程式的市集清單頁面，並且取得您的應用程式。 如需轉換的詳細資訊，請參閱本主題中的[了解應用程式下載數如何符合轉換的資格](#understanding-how-acquisitions-qualify-as-conversions)。

您可以以下列方式擷取您的 app 的自訂行銷活動效能資料：

* 您可以檢視有關頁面檢視和應用程式或附加元件，從轉換的資料**應用程式頁面檢視和活動識別碼的轉換**並**總行銷活動轉換**圖表會在中[併購報表](acquisitions-report.md)。
* 如果您的 App 是通用 Windows 平台 (UWP) app，您可以使用 Windows SDK 中的 API，以程式設計方式擷取導致轉換的自訂行銷活動識別碼。

## <a name="example-custom-campaign-scenario"></a>自訂行銷活動案例的範例

請考慮已經完成建置新遊戲並想要對為其現有的遊戲玩家促銷的遊戲開發人員。 她在她的 Facebook 頁面上張貼新遊戲發行的通知，包含遊戲的 Store 清單。 她的許多玩家也在 Twitter 上關注她，所以她也推文公告遊戲的 Store 清單。

為了追蹤每個這些促銷活動管道的成功度，開發人員會為遊戲的 Store 清單建立兩個 URL 變數：

* 她將會張貼到她的 Facebook 頁面的 URL 包含自訂的活動識別碼 `my-facebook-campaign`

* 她將會張貼到 Twitter 的 URL 包含自訂的活動識別碼 `my-twitter-campaign`

當她的 Facebook 和 Twitter 追隨者按一下 URL 時，Microsoft 會追蹤與對應的自訂行銷活動關聯的每個點按。 後續符合資格的遊戲購置和任何附加元件購買會與自訂行銷活動產生關聯，並且回報為轉換。

<span id="conversions" />

## <a name="understanding-how-acquisitions-qualify-as-conversions"></a>了解下載數如何符合轉換的資格

自訂行銷活動 *「轉換」* 即是指客戶透過按下您藉由自訂行銷活動促銷的 URL，並且取得您的應用程式。 有不同的案例，轉換為符合資格**應用程式頁面檢視和活動識別碼的轉換**並**總行銷活動轉換**圖表會在中[取得報表](acquisitions-report.md)以及的轉換成合格[以程式設計方式擷取活動識別碼](#programmatically)。

### <a name="qualifying-conversions-in-the-acquisitions-report"></a>合格的併購報表中的轉換

對於開發人員中心儀表板上的[下載數報告](acquisitions-report.md)的**應用程式頁面檢視數與轉換數 (依行銷活動識別碼)** 和**行銷活動轉換總計**圖表，下列案例符合轉換的資格：

* 客戶 *(具備或不具備已辨識的 Microsoft 帳戶)* 按一下包含自訂行銷活動識別碼的 App URL，並重新導向至 App 的市集清單頁面。 然後，同一個客戶在首次按下包含自訂行銷活動識別碼的 Microsoft Store URL 的 24 小時內取得應用程式。

* 若客戶取得應用程式的裝置不同於他們用來按下包含自訂行銷活動識別碼之 URL 的裝置，只有在當該客戶登入時與按一下 URL 時使用相同的 Microsoft 帳戶，才會計算轉換。

> [!NOTE]
> 針對已計入為透過自訂行銷活動轉換的 App 下載數，該客戶在該 App 中任何附加元件的購買，也都會計為透過同一個自訂行銷活動進行的轉換。

### <a name="qualifying-conversions-when-programmatically-retrieving-the-campaign-id"></a>以程式設計方式擷取行銷活動識別碼時的合格轉換

若要在以程式設計方式擷取與 App 關聯的行銷活動識別碼時符合轉換的資格，必須符合下列條件：

* 執行的裝置上**Windows 10 版本 1607，或更新版本**:客戶 （無論登入可辨識的 Microsoft 帳戶） 按一下包含自訂的活動識別碼和重新導向至應用程式的存放區 清單頁面的 URL。 客戶因為按一下 URL 而檢視市集清單時取得 App。

* 執行的裝置上**Windows 10 版本 1511，或更早版本**:客戶 （必須登入可辨識的 Microsoft 帳戶） 按一下包含自訂的活動識別碼和重新導向至應用程式的存放區 清單頁面的 URL。 客戶因為按一下 URL 而檢視市集清單時取得 App。 在這些版本的 Windows 10 中，使用者必須使用已辨識的 Microsoft 帳戶登入，才能在以程式設計方式擷取行銷活動識別碼時讓下載符合轉換資格。

> [!NOTE]
> 若客戶離開市集清單頁面，但在 24 小時之內返回該頁面 (在同一部裝置上，或在使用相同的 Microsoft 帳戶登入時，使用不同的裝置)，並且取得 App，這**將**符合[下載數報告](acquisitions-report.md)的**應用程式頁面檢視數與轉換數 (依行銷活動識別碼)** 和**行銷活動轉換總計**圖表的轉換資格。 然而，這**不會**計為您透過程式設計方式擷取行銷活動識別碼所達成之轉換。

## <a name="embed-a-custom-campaign-id-to-your-apps-microsoft-store-page-url"></a>內嵌自訂行銷活動識別碼到您應用程式的 Microsoft Store 頁面 URL

若要使用自訂行銷活動識別碼為您的應用程式建立 Microsoft Store 頁面 URL：

1.  為您的自訂行銷活動建立識別碼字串。 此字串可以包含最多 100 個字元，不過我們建議您定義容易識別的簡短行銷活動識別碼。

   > [!NOTE]
   > 當其他開發人員檢視自己的 App [下載數報告](acquisitions-report.md)時，可能會看到此行銷活動識別碼字串。 當客戶按下您的自訂行銷活動識別碼進入到市集之中，並在同一個工作階段中購買了另一位開發人員的應用程式，因此將該轉換歸至您的行銷活動識別碼時，可能會發生這種狀況。 該開發人員會看到他們自己的應用程式當中，有多少轉換是來自於初次按下您的行銷活動識別碼，包括行銷活動識別碼名稱，但是他們不會看到有多少使用者是透過您的行銷活動識別碼而購買到您應用程式 (或任何其他開發人員的應用程式) 的相關資料。

2.  以 HTML 或通訊協定格式取得應用程式的 Store 清單。

    * 若要任何作業系統的客戶使用瀏覽器瀏覽到您應用程式的 web 架構市集清單，請使用 HTML URL。 在 Windows 裝置上，市集應用程式也將啟動並顯示您的應用程式清單。 此 URL 的格式為 **`https://www.microsoft.com/store/apps/*your app ID*`**。 例如，Skype 的 HTML URL 為 `https://www.microsoft.com/store/apps/9wzdncrfj364`。 您可以在 [App 身分識別](view-app-identity-details.md#link-to-your-apps-listing)頁面上找到這個 URL。

    * 若您想要從已安裝 UWP app 的裝置或電腦上執行的其他 Windows 應用程式進行促銷，或者您知道客戶使用支援 Microsoft Store 的裝置時，請使用通訊協定格式。 此連結會直接移至您的應用程式市集清單，而不會開啟瀏覽器。 此 URL 的格式為 **`ms-windows-store://pdp/?PRODUCTID=*your app id*`**。 例如，Skype 的通訊協定 URL 為 `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364`。

3.  附加以下字串到您的 app 的 URL 的結尾：

    * 針對 HTML 格式 URL，附加 **`?cid=*my custom campaign ID*`**。 例如，如果 Skype 導入了以值的活動識別碼**自訂\_行銷活動**、 新的 URL，包括的活動識別碼為： `https://www.microsoft.com/store/apps/skype/9wzdncrfj364?cid=custom\_campaign`。

    * 針對通訊協定格式 URL，附加 **`&cid=*my custom campaign ID*`**。 例如，如果 Skype 導入了以值的活動識別碼**自訂\_行銷活動**、 新的通訊協定 URL，包括的活動識別碼為： `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364&cid=custom\_campaign`。

<span id="programmatically" />

## <a name="programmatically-retrieve-the-custom-campaign-id-for-an-app"></a>以程式設計方式擷取 app 的自訂行銷活動識別碼

如果您的 App 是 UWP app，您可以使用 Windows SDK 中的 API 以程式設計方式擷取與 App 取得相關聯的自訂行銷活動識別碼。 這些 API 使許多分析和創造營收案例得以實現。 例如，您可以了解目前的使用者在透過您的 Facebook 行銷活動發現您的 app 後是否取得它，然後據此自訂 app 經驗。 或者，如果您使用第三方 app 行銷提供者，您可以將資料傳回提供者。

只有在客戶按一下包含行銷活動識別碼的 URL，檢視應用程式的 Microsoft Store 頁面，接著取得您的應用程式但未離開 Store 清單頁面時，這些 API 才會傳回行銷活動識別碼字串。 如果使用者離開頁面，但稍後又返回並取得 App，那麼使用這些 API 時就[不符合轉換資格](#conversions)。

您要使用的 API 會有所不同，取決於 App 針對的 Windows 10 目標版本：

* Windows 10 版本 1607，或更新版本：使用[ **StoreContext** ](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext)類別**Windows.Services.Store**命名空間。 使用此 API 時，您可以擷取任何[合格下載](#conversions)的自訂行銷活動識別碼，不論使用者是否使用已辨識的 Microsoft 帳戶登入。

* Windows 10 版本 1511，或更早版本：使用[ **CurrentApp** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp)類別**Windows.ApplicationModel.Store**命名空間。 使用此 API 時，您只能在使用者使用已辨識的 Microsoft 帳戶登入情況下擷取[合格下載](#conversions)的自訂行銷活動識別碼。

> [!NOTE]
> 雖然 **Windows.ApplicationModel.Store** 命名空間適用於所有 Windows 10 版本，如果您的 App 以 Windows 10 1607 版或更新版本為目標，還是建議您使用 **Windows.Services.Store** 命名空間中的 API。 如需有關這些命名空間之間差異的詳細資訊，請參閱 [App 內購買和試用版](../monetize/in-app-purchases-and-trials.md#choose-namespace)。 下列程式碼範例說明如何建構程式碼，在相同專案中使用這兩個 API。

### <a name="code-example"></a>程式碼範例

下列程式碼範例說明如何擷取自訂行銷活動識別碼。 這個範例透過[版本調適型程式碼](../debug-test-perf/version-adaptive-code.md)使用 **Windows.Services.Store** 和 **Windows.ApplicationModel.Store** 命名空間中的這兩組 API。 您的程式碼可以依照此程序，在任何版本的 Windows 10 上執行。 若要使用此代碼，專案的目標作業系統版本必須是 **Windows 10 Anniversary Edition (10.0 版；組建 14394)** 或更新版本，雖然最小作業系統版本可以是較早的版本。

``` csharp
// This example assumes the code file has using statements for
// System.Linq, System.Threading.Tasks, Windows.Data.Json,
// and Windows.Services.Store.
public async Task<string> GetCampaignId()
{
    // Use APIs in the Windows.Services.Store namespace if they are available
    // (the app is running on a device with Windows 10, version 1607, or later).
    if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent(
         "Windows.Services.Store.StoreContext"))
    {
        StoreContext context = StoreContext.GetDefault();

        // Try to get the campaign ID for users with a recognized Microsoft account.
        StoreProductResult result = await context.GetStoreProductForCurrentAppAsync();
        if (result.Product != null)
        {
            StoreSku sku = result.Product.Skus.FirstOrDefault(s => s.IsInUserCollection);

            if (sku != null)
            {
                return sku.CollectionData.CampaignId;
            }
        }

        // Try to get the campaign ID from the license data for users without a
        // recognized Microsoft account.
        StoreAppLicense license = await context.GetAppLicenseAsync();
        JsonObject json = JsonObject.Parse(license.ExtendedJsonData);
        if (json.ContainsKey("customPolicyField1"))
        {
            return json["customPolicyField1"].GetString();
        }

        // No campaign ID was found.
        return String.Empty;
    }
    // Fall back to using APIs in the Windows.ApplicationModel.Store namespace instead
    // (the app is running on a device with Windows 10, version 1577, or earlier).
    else
    {
#if DEBUG
        return await Windows.ApplicationModel.Store.CurrentAppSimulator.GetAppPurchaseCampaignIdAsync();
#else
        return await Windows.ApplicationModel.Store.CurrentApp.GetAppPurchaseCampaignIdAsync() ;
#endif
    }
}
```

此程式碼會進行下列作業：

1. 首先，檢查 **Windows.Services.Store** 命名空間中的 [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 類別是否適用於目前的裝置 (這表示裝置正在執行 Windows 10 1607 版或更新版本)。 如果是這樣，請繼續使用此類別。

2. 接下來，在目前的使用者擁有已辨識的 Microsoft 帳戶情況下嘗試取得自訂行銷活動識別碼。 為此，程式碼會取得表示目前 App SKU 的 [**StoreSku**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreSku) 物件，然後存取 [**CampaignId**](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata.CampaignId) 屬性以擷取行銷活動識別碼 (如果有的話)。
3. 程式碼接著在目前使用者沒有已辨識的 Microsoft 帳戶情況下嘗試擷取行銷活動識別碼。 在此情況下，行銷活動識別碼是內嵌在 App 授權中。 程式碼會使用 [**GetAppLicenseAsync**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetAppLicenseAsync) 方法擷取授權，然後對名稱為 *customPolicyField1* 的索引鍵值進行授權的 JSON 內容剖析。 這個值包含行銷活動識別碼。

4. 如果 **Windows.Services.Store** 命名空間中的 [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 類別無法使用，程式碼就會回復為使用 **Windows.ApplicationModel.Store** 命名空間中的 [**GetAppPurchaseCampaignIdAsync**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp#Windows_ApplicationModel_Store_CurrentApp_GetAppPurchaseCampaignIdAsync) 方法，以擷取自訂行銷活動識別碼 (這個命名空間適用於所有 Windows 10 版本，包括 1511 版和更早版本)。 請注意，使用此方法時，您只能在使用者擁有已辨識的 Microsoft 帳戶情況下擷取[合格下載](#conversions)的自訂行銷活動識別碼。

### <a name="specify-the-campaign-id-in-the-proxy-file-for-the-windowsapplicationmodelstore-namespace"></a>在 Proxy 檔案中為 Windows.ApplicationModel.Store 命名空間指定行銷活動識別碼

**Windows.ApplicationModel.Store** 命名空間包含 [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator)，這是為了在提交 App 至市集之前測試程式碼而模擬市集作業的特殊類別。 此類別會從[名為 Windows.StoreProxy.xml 的本機檔案](../monetize/in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#using-the-windowsstoreproxyxml-file-with-currentappsimulator)擷取資料。 上一個程式碼範例說明如何在專案的偵錯及非偵錯程式碼中同時使用 **CurrentApp** 和 **CurrentAppSimulator**。 若要在偵錯環境中測試此程式碼，請將 **AppPurchaseCampaignId** 項目新增至開發電腦上的 WindowsStoreProxy.xml 檔案，如下列範例所示。 執行 app 時，[**GetAppPurchaseCampaignIdAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator.GetAppPurchaseCampaignIdAsync) 方法一律會傳回這個值。

``` xml
<CurrentApp>
    ...
    <AppPurchaseCampaignId>your custom campaign ID</AppPurchaseCampaignId>
</CurrentApp>
```

**Windows.Services.Store** 命名空間不提供您可在測試期間用來模擬授權資訊的類別。 您必須改為將應用程式發行到「市集」，並將該應用程式下載到您的開發裝置，才能使用它的授權進行測試。 如需詳細資訊，請參閱 [App 內購買和試用版](../monetize/in-app-purchases-and-trials.md#testing)。

## <a name="test-your-custom-campaign"></a>測試您的自訂行銷活動

促銷自訂行銷活動 URL 之前，建議您執行下列動作來測試自訂行銷活動：

1.  在您要用於測試的裝置上登入 Microsoft 帳戶。

2.  按一下您的自訂行銷活動 URL。 請確定您被引導至您的應用程式頁面，然後關閉 UWP app 或瀏覽器頁面。

3.  按一下 URL 數次，在每次瀏覽到您的應用程式頁面後關閉 UWP app 或瀏覽器頁面。 在對應用程式頁面的**其中一次**瀏覽中，取得您的應用程式以產生轉換。 計算您按一下 URL 的總次數。

4. 確認預期的頁面檢視和轉換是否出現在**應用程式頁面檢視和活動識別碼的轉換**並**總行銷活動轉換**圖表會在中[取得報表](acquisitions-report.md)，並測試您的應用程式程式碼，以確認它是否可以成功地擷取使用上面所述的 Api 的活動 ID。
