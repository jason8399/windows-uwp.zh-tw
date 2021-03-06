---
ms.assetid: 7CC11888-8DC6-4FEE-ACED-9FA476B2125E
description: 使用 Microsoft Store 提交 API，以程式設計方式建立和管理向合作夥伴中心帳戶註冊的應用程式提交。
title: 建立及管理提交
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 提交 API
ms.localizationpriority: medium
ms.openlocfilehash: 38a59db4115332a374c96c8a4400dbaccff9cd82
ms.sourcegitcommit: 720413d2053c8d5c5b34d6873740be6e913a4857
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/25/2020
ms.locfileid: "88846818"
---
# <a name="create-and-manage-submissions"></a>建立及管理提交

使用 *Microsoft Store 提交 API* ，以程式設計方式查詢及建立您或組織合作夥伴中心帳戶的應用程式、附加元件和封裝航班的提交。 如果您的帳戶管理多個應用程式或附加元件，而且您想要自動化與最佳化這些資產的提交程序，這個 API 非常有用。 這個 API 使用 Azure Active Directory (Azure AD) 來驗證您 App 或服務的呼叫。

下列步驟說明使用 Microsoft Store 提交 API 的端對端處理程序︰

1.  請確定您已經完成所有[先決條件](#prerequisites)。
3.  在呼叫 Microsoft Store 提交 API 中的方法之前，請先[取得 Azure AD 存取權杖](#obtain-an-azure-ad-access-token)。 取得權杖之後，在權杖到期之前，您有 60 分鐘的時間可以使用這個權杖呼叫 Microsoft Store 提交 API。 權杖到期之後，您可以產生新的權杖。
4.  [呼叫 Microsoft Store 提交 API](#call-the-windows-store-submission-api)。

<span id="not_supported" />

> [!IMPORTANT]
> 如果您使用此 API 來建立應用程式、封裝航班或附加元件的提交，請務必使用 API 來進一步變更提交，而不是在合作夥伴中心中進行。 如果您使用合作夥伴中心來變更最初使用 API 所建立的提交，您將無法再使用 API 變更或認可該提交。 有時候提交可能會處於錯誤狀態，而無法繼續提交過程。 若發生這種情形，您必須刪除提交並建立新的提交。

> [!IMPORTANT]
> 您無法使用此 API 來發佈 [透過商務用 Microsoft Store 和教育用 Microsoft Store 大量購買](../publish/organizational-licensing.md) 的提交或直接向企業發佈 [LOB 應用程式](../publish/distribute-lob-apps-to-enterprises.md) 的提交。 在這兩種情況下，您都必須使用合作夥伴中心中的 [發佈提交]。

> [!NOTE]
> 此 API 無法搭配應用程式或使用強制性應用程式更新以及 Microsoft Store 管理的可消耗附加元件的附加元件來使用。 如果您將 Microsoft Store 提交 API 用於使用上述其中一個功能的 App 或附加元件，API 將會傳回 409 錯誤碼。 在此情況下，您必須使用合作夥伴中心來管理應用程式或附加元件的提交。

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-submission-api"></a>步驟 1︰完成使用 Microsoft Store 提交 API 的先決條件

開始撰寫程式碼以呼叫 Microsoft Store 提交 API 之前，請先確定您已完成下列先決條件。

* 您 (或您的組織) 必須具有 Azure AD 目錄，且您必須具備該目錄的[全域管理員](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) \(部分機器翻譯\) 權限。 如果您已經使用 Microsoft 365 或 Microsoft 的其他商務服務，則您已經有 Azure AD 目錄。 否則，您可以 [在合作夥伴中心中建立新的 Azure AD](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) ，而不需要額外收費。

* 您必須[將 Azure AD 應用程式與您的合作夥伴中心帳戶建立關聯](#associate-an-azure-ad-application-with-your-windows-partner-center-account) \(部分機器翻譯\)，並取得您的租用戶識別碼、用戶端識別碼及金鑰。 您需要這些值才能取得 Azure AD 存取權杖，而您將會在呼叫 Microsoft Store 提交 API 時使用該權杖。

* 準備您的 App 與 Microsoft Store 提交 API 搭配使用︰

  * 如果合作夥伴中心中還沒有您的應用程式，您必須 [在合作夥伴中心中保留應用程式的名稱，以建立您的應用程式](https://docs.microsoft.com/windows/uwp/publish/create-your-app-by-reserving-a-name)。 您無法使用 Microsoft Store 提交 API 在合作夥伴中心中建立應用程式;您必須使用合作夥伴中心來建立它，然後您可以使用 API 來存取應用程式，並以程式設計方式為它建立提交。 不過，您可以使用 API 以程式設計方式建立附加元件和套件正式發行前小眾測試版，再建立提交。

  * 您必須先 [針對合作夥伴中心中的應用程式建立一個提交](https://docs.microsoft.com/windows/uwp/publish/app-submissions)，包括回答 [年齡分級](https://docs.microsoft.com/windows/uwp/publish/age-ratings) 問卷，才能使用此 API 來建立指定應用程式的提交。 執行這個動作之後，您就能夠使用 API 以程式設計方式建立此 App 的新提交。 您不需要先建立附加元件提交或套件正式發行前小眾測試版提交，才能使用 API 進行這些類型的提交。

  * 如果您要建立或更新 App 提交，而且必須包含 App 套件，請[準備 App 套件](https://docs.microsoft.com/windows/uwp/publish/app-package-requirements)。

  * 如果您要建立或更新 App 提交，而且必須包含 Store 清單的螢幕擷取畫面或影像，請[準備 App 螢幕擷取畫面與影像](https://docs.microsoft.com/windows/uwp/publish/app-screenshots-and-images)。

  * 如果您要建立或更新附加元件提交，而且必須包含圖示，請[準備圖示](https://docs.microsoft.com/windows/uwp/publish/create-iap-descriptions)。

<span id="associate-an-azure-ad-application-with-your-windows-partner-center-account" />

### <a name="how-to-associate-an-azure-ad-application-with-your-partner-center-account"></a>如何將 Azure AD 應用程式與您的合作夥伴中心帳戶建立關聯

使用 Microsoft Store 提交 API 之前，您必須先將 Azure AD 應用程式與您的合作夥伴中心帳戶建立關聯，並取得應用程式的租使用者識別碼和用戶端識別碼，並產生金鑰。 Azure AD 應用程式代表您要呼叫 Microsoft Store 提交 API 的 App 或服務。 您需要租用戶識別碼、用戶端識別碼及金鑰，以取得您會傳遞給 API 的 Azure AD 存取權杖。

> [!NOTE]
> 您只需要執行此工作一次。 在您取得租用戶識別碼、用戶端識別碼及金鑰之後，您便可以在未來需要建立新的 Azure AD 存取權杖時重複加以使用。

1.  在合作夥伴中心中，[將您組織的合作夥伴中心帳戶與您組織的 Azure AD 目錄建立關聯](../publish/associate-azure-ad-with-partner-center.md) \(部分機器翻譯\)。

2.  接下來，在合作夥伴中心 [帳戶設定] 區段中的 [使用者] 頁面上[新增 Azure AD 應用程式](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) \(部分機器翻譯\)，此應用程式代表您將用來存取您合作夥伴中心帳戶之提交的應用程式或服務。 確定您將此應用程式指派為 [管理員] 角色。 如果該應用程式尚未存在於您的 Azure AD 目錄，您可以[在合作夥伴中心中建立新的 Azure AD 應用程式](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account) \(部分機器翻譯\)。  

3.  返回 [使用者] 頁面，按一下您 Azure AD 應用程式的名稱以移至應用程式設定，然後複製 [租用戶識別碼] 和 [用戶端識別碼] 值。

4. 按一下 [新增金鑰]。 在後續畫面上，複製 [金鑰] 值。 在您離開此頁面之後，便無法再次存取此資訊。 如需詳細資訊，請參閱[管理 Azure AD 應用程式的金鑰](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys) \(部分機器翻譯\)。

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>步驟 2:取得 Azure AD 存取權杖

在 Microsoft Store 提交 API 中呼叫任何方法之前，您必須先取得傳遞至 API 中每個方法的 **Authorization** 標頭的 Azure AD 存取權杖。 在您取得存取權杖之後，您有 60 分鐘的使用時間，之後其便會到期。 權杖到期之後，您可以重新整理權杖以便在後續呼叫 API 時繼續使用。

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

如需示範如何使用 C#、Java 或 Python 程式碼來取得存取權杖的範例，請參閱 Microsoft Store 提交 API [程式碼範例](#code-examples)。

<span id="call-the-windows-store-submission-api">

## <a name="step-3-use-the-microsoft-store-submission-api"></a>步驟 3：使用 Microsoft Store 提交 API

取得 Azure AD 存取權杖之後，您就可以呼叫 Microsoft Store 提交 API 中的方法。 API 包含許多方法，可以分成適用於 App、附加元件與套件正式發行前小眾測試版的案例。 若要建立或更新提交，您通常要以特定順序呼叫 Microsoft Store 提交 API 中的多個方法。 如需每個案例及每個方法之語法的相關資訊，請參閱下表中的文章。

> [!NOTE]
> 取得存取權杖之後，在權杖到期之前，您有 60 分鐘的時間可以呼叫 Microsoft Store 提交 API 中的方法。

| 狀況       | 描述                                                                 |
|---------------|----------------------------------------------------------------------|
| 應用程式 |  針對已向合作夥伴中心帳戶註冊的所有應用程式取出資料，並建立應用程式的提交。 如需這些方法的詳細資訊，請參閱下列文章： <ul><li>[取得應用程式資料](get-app-data.md)</li><li>[管理應用程式提交](manage-app-submissions.md)</li></ul> |
| 附加元件 | 取得、建立或刪除您 App 的附加元件，然後取得、建立或刪除附加元件的提交。 如需這些方法的詳細資訊，請參閱下列文章： <ul><li>[管理附加元件](manage-add-ons.md)</li><li>[管理附加元件提交](manage-add-on-submissions.md)</li></ul> |
| 套件正式發行前小眾測試版 | 取得、建立或刪除您 App 的套件正式發行前小眾測試版，然後取得、建立或刪除套件正式發行前小眾測試版的提交。 如需這些方法的詳細資訊，請參閱下列文章： <ul><li>[管理套件正式發行前小眾測試版](manage-flights.md)</li><li>[管理套件正式發行前小眾測試版提交](manage-flight-submissions.md)</li></ul> |

<span id="code-samples"/>

## <a name="code-examples"></a>程式碼範例

下列文章提供詳細的程式碼範例，示範如何以多個不同的程式設計語言使用 Microsoft Store 提交 API：

* [C# 範例：提交應用程式、附加元件與正式發行前小眾測試版](csharp-code-examples-for-the-windows-store-submission-api.md)
* [C# 範例：提交含遊戲選項與預告片的應用程式](csharp-code-examples-for-submissions-game-options-and-trailers.md)
* [Java 範例：提交應用程式、附加元件與正式發行前小眾測試版](java-code-examples-for-the-windows-store-submission-api.md)
* [Java 範例：提交含遊戲選項與預告片的應用程式](java-code-examples-for-submissions-game-options-and-trailers.md)
* [Python 範例：提交應用程式、附加元件與正式發行前小眾測試版](python-code-examples-for-the-windows-store-submission-api.md)
* [Python 範例：提交含遊戲選項與預告片的應用程式](python-code-examples-for-submissions-game-options-and-trailers.md)

## <a name="storebroker-powershell-module"></a>StoreBroker PowerShell 模組

另一種直接呼叫 Microsoft Store 提交 API 的方法，我們也提供了開放原始碼 PowerShell 模組，該模組在 API 上方實作命令列介面。 這個模組稱為 [StoreBroker](https://github.com/Microsoft/StoreBroker)。 您可以使用此模組從命令列來管理您的應用程式、正式發行前小眾測試版和附加元件提交，而不是直接直接呼叫 Microsoft Store 提交 API，也可以簡單瀏覽來源以檢視有關如何呼叫此 API 的更多範例。 StoreBroker 模組在 Microsoft 中積極地被用作為將眾多第一方應用程式提交至 Microsoft Store 的主要方式。

如需詳細資訊，請查看我們[在 GitHub 上的 StoreBroker 頁面](https://github.com/Microsoft/StoreBroker)。

## <a name="troubleshooting"></a>疑難排解

| 問題      | 解決方案                                          |
|---------------|---------------------------------------------|
| 從 PowerShell 呼叫 Microsoft Store 提交 API 之後，如果您使用 [ConvertFrom-Json](https://docs.microsoft.com/powershell/module/5.1/microsoft.powershell.utility/ConvertFrom-Json) Cmdlet 從 JSON 格式轉換為 PowerShell 物件，然後使用 [ConvertTo-Json](https://docs.microsoft.com/powershell/module/5.1/microsoft.powershell.utility/ConvertTo-Json) Cmdlet 轉換回 JSON 格式，API 的回應資料會損毀。 |  [ConvertTo-Json](https://docs.microsoft.com/powershell/module/5.1/microsoft.powershell.utility/ConvertTo-Json) Cmdlet 的 *-Depth* 參數預設是設為 2 層物件，這對於 Microsoft Store 提交 API 傳回的大部分 JSON 物件來說太淺。 當您呼叫 [ConvertTo-Json](https://docs.microsoft.com/powershell/module/5.1/microsoft.powershell.utility/ConvertTo-Json) Cmdlet 時，請將 *-Depth* 參數設為較大的數字，例如 20。 |

## <a name="additional-help"></a>其他說明

如果您有 Microsoft Store 提交 API 的相關問題，或需要使用這個 API 管理提交的協助，請使用下列資源︰

* 在我們的[論壇](https://social.msdn.microsoft.com/Forums/windowsapps/home?forum=wpsubmit)發問。
* 請流覽我們的 [支援頁面](https://developer.microsoft.com/windows/support) ，並要求合作夥伴中心的其中一個協助支援選項。 如果系統提示您選擇問題類型以及類別，請分別選擇 **App 提交和認證**以及**提交 App**。  

## <a name="related-topics"></a>相關主題

* [取得應用程式資料](get-app-data.md)
* [管理應用程式提交](manage-app-submissions.md)
* [管理附加元件](manage-add-ons.md)
* [管理附加元件提交](manage-add-on-submissions.md)
* [管理套件正式發行前小眾測試版](manage-flights.md)
* [管理套件正式發行前小眾測試版提交](manage-flight-submissions.md)
