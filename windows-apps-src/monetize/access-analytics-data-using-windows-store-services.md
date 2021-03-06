---
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: 使用 Microsoft Store 分析 API，以程式設計方式取得已向您或您組織的 Windows 合作夥伴中心帳戶註冊之應用程式的分析資料。
title: 使用 Microsoft Store 服務存取分析資料
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10 , UWP, Microsoft Store 服務, Microsoft Store 分析 API
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 51b0180d6b550cda082b5ee10530194824a48e33
ms.sourcegitcommit: 720413d2053c8d5c5b34d6873740be6e913a4857
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/25/2020
ms.locfileid: "88846798"
---
# <a name="access-analytics-data-using-store-services"></a>使用 Microsoft Store 服務存取分析資料

使用 *Microsoft Store 分析 API* ，以程式設計方式抓取向您或您組織的 Windows 合作夥伴中心帳戶註冊之應用程式的分析資料。 這個 API 可讓您擷取應用程式和附加元件 (也稱為應用程式內產品或 IAP) 下載數、錯誤、應用程式評分與評論的資料。 這個 API 使用 Azure Active Directory (Azure AD) 來驗證您 App 或服務的呼叫。

下列步驟說明端對端的程序：

1.  請確定您已經完成所有[先決條件](#prerequisites)。
2.  在呼叫 Microsoft Store 分析 API 中的方法之前，請先[取得 Azure AD 存取權杖](#obtain-an-azure-ad-access-token)。 取得權杖之後，在權杖到期之前，您有 60 分鐘的時間可以使用這個權杖呼叫 Microsoft Store 分析 API。 權杖到期之後，您可以產生新的權杖。
3.  [呼叫 Microsoft Store 分析 API](#call-the-windows-store-analytics-api)。

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-analytics-api"></a>步驟 1：完成使用 Microsoft Store 分析 API 的先決條件

開始撰寫程式碼以呼叫 Microsoft Store 分析 API 之前，請先確定您已完成下列先決條件。

* 您 (或您的組織) 必須具有 Azure AD 目錄，且您必須具備該目錄的[全域管理員](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) \(部分機器翻譯\) 權限。 如果您已經使用 Microsoft 365 或 Microsoft 的其他商務服務，則您已經有 Azure AD 目錄。 否則，您可以 [在合作夥伴中心中建立新的 Azure AD](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) ，而不需要額外收費。

* 您必須將 Azure AD 應用程式與您的合作夥伴中心帳戶建立關聯、取出應用程式的租使用者識別碼和用戶端識別碼，並產生金鑰。 Azure AD 應用程式代表您要呼叫 Microsoft Store 分析 API 的應用程式或服務。 您需要租用戶識別碼、用戶端識別碼及金鑰，以取得您會傳遞給 API 的 Azure AD 存取權杖。
    > [!NOTE]
    > 您只需要執行此工作一次。 在您取得租用戶識別碼、用戶端識別碼及金鑰之後，您便可以在未來需要建立新的 Azure AD 存取權杖時重複加以使用。

若要將 Azure AD 應用程式與您的合作夥伴中心帳戶建立關聯，並取出所需的值：

1.  在合作夥伴中心中，[將您組織的合作夥伴中心帳戶與您組織的 Azure AD 目錄建立關聯](../publish/associate-azure-ad-with-partner-center.md) \(部分機器翻譯\)。

2.  接下來，在合作夥伴中心的 [**帳戶設定**] 區段中的 [**使用者**] 頁面上，新增代表您將用來存取合作夥伴中心帳戶分析資料之應用程式或服務的[Azure AD 應用程式](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account)。 確定您將此應用程式指派為 [管理員] 角色。 如果該應用程式尚未存在於您的 Azure AD 目錄，您可以[在合作夥伴中心中建立新的 Azure AD 應用程式](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account) \(部分機器翻譯\)。

3.  返回 [使用者] 頁面，按一下您 Azure AD 應用程式的名稱以移至應用程式設定，然後複製 [租用戶識別碼] 和 [用戶端識別碼] 值。

4. 按一下 [新增金鑰]。 在後續畫面上，複製 [金鑰] 值。 在您離開此頁面之後，便無法再次存取此資訊。 如需詳細資訊，請參閱[管理 Azure AD 應用程式的金鑰](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys) \(部分機器翻譯\)。

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>步驟 2:取得 Azure AD 存取權杖

在 Microsoft Store 分析 API 中呼叫任何方法之前，您必須先取得傳遞至 API 中每個方法的 **Authorization** 標頭的 Azure AD 存取權杖。 在您取得存取權杖之後，您有 60 分鐘的使用時間，之後其便會到期。 權杖到期之後，您可以重新整理權杖以便在後續呼叫 API 時繼續使用。

若要取得存取權杖，請按照[使用用戶端認證的服務對服務呼叫](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/)中的指示，將 HTTP POST 傳送至 ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` 端點。 以下是範例要求。

```json
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

針對 POST URI 中的 *租使用者 \_ 識別碼* 值，以及 *客戶 \_ 端識別碼* 和用戶端 * \_ 密碼* 參數，指定您的應用程式的租使用者識別碼、用戶端識別碼和金鑰，並從上一節的合作夥伴中心中取出。 針對 *resource* 參數，您必須指定 ```https://manage.devcenter.microsoft.com```。

存取權杖到期之後，您可以按照[這裡](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)的指示，重新整理權杖。

<span id="call-the-windows-store-analytics-api" />

## <a name="step-3-call-the-microsoft-store-analytics-api"></a>步驟 3：呼叫 Microsoft Store 分析 API

有了 Azure AD 存取權杖之後，就可以呼叫 Microsoft Store 分析 API。 您必須將存取權杖傳送給每個方法的 **Authorization** 標頭。

### <a name="methods-for-uwp-apps-and-games"></a>UWP 應用程式和遊戲的方法
下列方法適用于應用程式和遊戲的取得和附加元件的收購： 

* [取得遊戲和應用程式的銷售資料](acquisitions-data.md)
* [取得遊戲和應用程式的其他銷售資料](add-on-acquisitions-data.md)

### <a name="methods-for-uwp-apps"></a>適用於 UWP app 的方法 

下列分析方法適用于合作夥伴中心中的 UWP 應用程式。

| 狀況       | 方法      |
|---------------|--------------------|
| 收購、轉換、安裝和使用方式 |  <ul><li> (舊版) [取得應用程式收購](get-app-acquisitions.md)</li><li>取得 (舊版) 的[應用程式取得漏斗資料](get-acquisition-funnel-data.md)</li><li>[依通路取得應用程式轉換](get-app-conversions-by-channel.md)</li><li>[取得附加元件下載數](get-in-app-acquisitions.md)</li><li>[取得訂閱附加元件下載數](get-subscription-acquisitions.md)</li><li>[依通路取得附加元件轉換](get-add-on-conversions-by-channel.md)</li><li>[取得應用程式安裝數](get-app-installs.md)</li><li>[取得每日應用程式使用量](get-app-usage-daily.md)</li><li>[取得每月應用程式使用量](get-app-usage-monthly.md)</li></ul> |
| App 錯誤 | <ul><li>[取得錯誤報告資料](get-error-reporting-data.md)</li><li>[取得應用程式中錯誤的詳細資料](get-details-for-an-error-in-your-app.md)</li><li>[取得應用程式中錯誤的堆疊追蹤](get-the-stack-trace-for-an-error-in-your-app.md)</li><li>[下載應用程式中錯誤的 CAB 檔案](download-the-cab-file-for-an-error-in-your-app.md)</li></ul> |
| 深入解析 | <ul><li>[取得應用程式的深入解析資料](get-insights-data-for-your-app.md)</li></ul>  |
| 評分與評論 | <ul><li>[取得應用程式評分](get-app-ratings.md)</li><li>[取得應用程式評論](get-app-reviews.md)</li></ul> |
| 應用程式內廣告與廣告行銷活動 | <ul><li>[取得廣告績效資料](get-ad-performance-data.md)</li><li>[取得廣告活動績效資料](get-ad-campaign-performance-data.md)</li></ul> |

### <a name="methods-for-desktop-applications"></a>傳統型應用程式的方法

下列分析方法可供隸屬於 [Windows 傳統型應用程式計畫](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)的開發人員帳戶使用。

| 狀況       | 方法      |
|---------------|--------------------|
| 安裝數 |  <ul><li>[取得傳統型應用程式安裝](get-desktop-app-installs.md)</li></ul> |
| 區塊 |  <ul><li>[取得傳統型應用程式的升級區塊](get-desktop-block-data.md)</li><li>[取得傳統型應用程式的升級區塊詳細資料](get-desktop-block-data-details.md)</li></ul> |
| 應用程式錯誤 |  <ul><li>[取得傳統型應用程式的錯誤報告資料](get-desktop-application-error-reporting-data.md)</li><li>[取得傳統型應用程式中錯誤的詳細資料](get-details-for-an-error-in-your-desktop-application.md)</li><li>[取得傳統型應用程式中錯誤的堆疊追蹤](get-the-stack-trace-for-an-error-in-your-desktop-application.md)</li><li>[下載傳統型應用程式中錯誤的 CAB 檔案](download-the-cab-file-for-an-error-in-your-desktop-application.md)</li></ul> |
| 深入解析 | <ul><li>[取得傳統型應用程式的深入解析資料](get-insights-data-for-your-desktop-app.md)</li></ul>  |

### <a name="methods-for-xbox-live-services"></a>適用於 Xbox Live 服務的方法

下列其他方法可供使用 [Xbox Live 服務](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md)的遊戲開發人員帳戶使用。

| 狀況       | 方法      |
|---------------|--------------------|
| 一般分析 |  <ul><li>[取得 Xbox Live 分析資料](get-xbox-live-analytics.md)</li><li>[取得 Xbox Live 成就資料](get-xbox-live-achievements-data.md)</li><li>[取得 Xbox Live 並行使用資料](get-xbox-live-concurrent-usage-data.md)</li></ul> |
| 健康情況分析 |  <ul><li>[取得 Xbox Live 健康情況資料](get-xbox-live-health-data.md)</li></ul> |
| 社群分析 |  <ul><li>[取得 Xbox Live 遊戲中心資料](get-xbox-live-game-hub-data.md)</li><li>[取得 Xbox Live 俱樂部資料](get-xbox-live-club-data.md)</li><li>[取得 Xbox Live 多人遊戲資料](get-xbox-live-multiplayer-data.md)</li></ul>  |

### <a name="methods-for-hardware-and-drivers"></a>硬體與驅動程式的方法

屬於 [Windows 硬體儀表板程式](https://docs.microsoft.com/windows-hardware/drivers/dashboard/get-started-with-the-hardware-dashboard) 的開發人員帳戶，可以存取一組額外的方法來取得硬體和驅動程式的分析資料。 如需詳細資訊，請參閱 [硬體儀表板 API](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-api)。

## <a name="code-example"></a>程式碼範例

下列程式碼範例示範如何取得 Azure AD 存取權杖，並從 C# 主控台應用程式呼叫 Microsoft Store 分析 API。 若要使用此程式碼範例，請針對您的案例將 *tenantId*、*clientId*、*clientSecret* 和 *appID* 變數指定為適當的值。 此範例需要 Newtonsoft 的 [Json.NET package](https://www.newtonsoft.com/json)，以還原序列化 Microsoft Store 分析 API 傳回的 JSON 資料。

> [!div class="tabbedCodeSnippets"]
[!code-csharp[AnalyticsApi](./code/StoreServicesExamples_Analytics/cs/Program.cs#AnalyticsApiExample)]

## <a name="error-responses"></a>錯誤回應

Microsoft Store 分析 API 會以包含錯誤碼和訊息的 JSON 物件，傳回錯誤回應。 下列範例示範由無效的參數造成的錯誤回應。

```json
{
    "code":"BadRequest",
    "data":[],
    "details":[],
    "innererror":{
        "code":"InvalidQueryParameters",
        "data":[
            "top parameter cannot be more than 10000"
        ],
        "details":[],
        "message":"One or More Query Parameters has invalid values.",
        "source":"AnalyticsAPI"
    },
    "message":"The calling client sent a bad request to the service.",
    "source":"AnalyticsAPI"
}
```
