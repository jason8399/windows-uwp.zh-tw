---
author: mcleanbyron
description: "使用「Windows 市集提交 API」中的這個方法，來完成套件正式發行前小眾測試版提交的套件推出。"
title: "使用 Windows 市集提交 API 來完成套件正式發行前小眾測試版提交的套件推出"
translationtype: Human Translation
ms.sourcegitcommit: 9b76a11adfab838b21713cb384cdf31eada3286e
ms.openlocfilehash: 7e08aa793ee0f889995e8ec80554c6b6e41bd8b6

---

# 使用 Windows 市集提交 API 來完成套件正式發行前小眾測試版提交的套件推出


使用「Windows 市集提交 API」中的這個方法，來[完成](../publish/gradual-package-rollout.md#completing-the-rollout)套件正式發行前小眾測試版提交的套件推出。 如需有關使用「Windows 市集提交 API」來建立套件正式發行前小眾測試版提交的程序詳細資訊，請參閱[管理套件正式發行前小眾測試版提交](manage-flight-submissions.md)。


## 先決條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Windows 市集提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。
* 針對您開發人員中心帳戶中的 App 建立提交。 您可以在「開發人員中心」儀表板中執行此操作，或是使用[建立 App 提交](create-an-app-submission.md)方法來執行此操作。
* 啟用提交的漸進式套件推出。 您可以在[開發人員中心儀表板](../publish/gradual-package-rollout.md)中執行此操作，或是[使用 Windows 市集提交 API](manage-flight-submissions.md#manage-gradual-package-rollout) 來執行此操作。

>**注意**&nbsp;&nbsp;這個方法僅供用於已被授權使用「Windows 市集提交 API」的「Windows 開發人員中心」帳戶。 並非所有的帳戶都已啟用此權限。

## 要求

這個方法的語法如下。 如需使用方式範例及標頭和要求參數的描述，請參閱下列各節。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/finalizepackagerollout``` |

<span/>
 

### 要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |

<span/>

### 要求參數

| 名稱        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 字串 | 必要。 App 的「市集識別碼」，此 App 包含您想要完成其套件推出的套件正式發行前小眾測試版提交。 如需有關「市集識別碼」的詳細資訊，請參閱[檢視 App 身分識別詳細資料](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)。  |
| flightId | 字串 | 必要。 套件正式發行前小眾測試版的識別碼，此正式發行前小眾測試版包含您想要完成其套件推出的提交。 此識別碼可從「開發人員中心」儀表板中取得，而且包含在[建立套件正式發行前小眾測試版](create-a-flight.md)和[取得 App 的套件正式發行前小眾測試版](get-flights-for-an-app.md)要求的回應資料中。  |
| submissionId | 字串 | 必要。 您想要完成其套件推出之提交的識別碼。 此識別碼可從「開發人員中心」儀表板中取得，並且包含在[建立套件正式發行前小眾測試版提交](create-a-flight-submission.md)要求的回應資料中。  |

<span/>

### 要求主體

不提供此方法的要求主體。

### 要求範例

下列範例示範如何完成套件正式發行前小眾測試版提交的套件推出。

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243680/finalizepackagerollout HTTP/1.1
Authorization: Bearer <your access token>
```

## 回應

下列範例示範成功呼叫此方法時的 JSON 回應主體。 如需有關回應主體中各個值的詳細資訊，請參閱[套件推出資源](manage-flight-submissions.md#package-rollout-object)。

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 100,
    "packageRolloutStatus": "PackageRolloutComplete",
    "fallbackSubmissionId": "1212922684621243058"
}
```

## 錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述   |
|--------|------------------|
| 404  | 找不到套件正式發行前小眾測試版。 |
| 409  | 此代碼指出下列其中一個錯誤：<br/><br/><ul><li>提交狀態不是漸進式推出作業的有效狀態 (呼叫此方法之前，必須先發佈提交且 [packageRolloutStatus](manage-flight-submissions.md#package-rollout-object) 值必須設定為 **PackageRolloutInProgress**)。</li><li>提交不屬於指定的 App。</li><li>App 使用[目前 Windows 市集提交 API 不支援](create-and-manage-submissions-using-windows-store-services.md#not_supported)的「開發人員中心」儀表板功能。</li></ul> |   

<span/>


## 相關主題

* [漸進式套件推出](../publish/gradual-package-rollout.md)
* [使用 Windows 市集提交 API 管理套件正式發行前小眾測試版提交](manage-flight-submissions.md)
* [使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)



<!--HONumber=Nov16_HO1-->

