---
ms.assetid: 24C5F796-5FB8-4B5D-B428-C3154B3098BD
description: 使用 Microsoft Store 提交 API 中的這個方法，來更新現有的套件正式發行前小眾測試版提交。
title: 更新套件正式發行前小眾測試版提交
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 提交 API, 正式發行前小眾測試版提交, 更新
ms.localizationpriority: medium
ms.openlocfilehash: a06f341584c88be06e4f8c23a3b86bec9d1cec28
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360897"
---
# <a name="update-a-package-flight-submission"></a>更新套件正式發行前小眾測試版提交


使用 Microsoft Store 提交 API 中的這個方法，來更新現有的套件正式發行前小眾測試版提交。 使用這個方法成功更新提交之後，您必須針對擷取和發佈[認可提交](commit-a-flight-submission.md)。

如需這個方法如何在使用 Microsoft Store 提交 API 建立套件正式發行前小眾測試版提交的程序中進行的詳細資訊，請參閱[管理套件正式發行前小眾測試版提交](manage-flight-submissions.md)。

## <a name="prerequisites"></a>先決條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Microsoft Store 提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。
* 建立封裝飛行的送出您的應用程式的其中一個。 您可以在合作夥伴中心，或您可以使用[建立套件飛行提交](create-a-flight-submission.md)方法。

## <a name="request"></a>要求

這個方法的語法如下。 請參閱下列各小節了解標頭和要求本文的使用範例和描述。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 在表單中的 Azure AD 存取權杖**持有人** &lt;*語彙基元*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 名稱        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 字串 | 必要。 您想要更新套件正式發行前小眾測試版提交之 App 的市集識別碼。 如需有關市集識別碼的詳細資訊，請參閱[檢視應用程式身分識別詳細資料](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details)。  |
| flightId | 字串 | 必要。 您要更新提交之套件正式發行前小眾測試版的識別碼。 識別碼可從[建立套件正式發行前小眾測試版](create-a-flight.md)和[取得 App 套件正式發行前小眾測試版](get-flights-for-an-app.md)要求的回應資料中取得。 在合作夥伴中心建立的航班，此識別碼也會提供在合作夥伴中心 [飛行] 頁面的 url。  |
| submissionId | 字串 | 必要。 要更新之提交的識別碼。 在[建立套件正式發行前小眾測試版提交](create-a-flight-submission.md)要求的回應資料中有提供此識別碼。 提交在合作夥伴中心所建立，此識別碼也會提供在合作夥伴中心內的 [提交] 頁面的 url。  |


### <a name="request-body"></a>要求本文

要求本文包含下列參數。

| 值      | 類型   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightPackages           | 陣列  | 包含可提供關於提交中每個套件之詳細資料的物件。 如需回應主體中各個值的詳細資訊，請參閱[正式發行前小眾測試版資源](manage-flight-submissions.md#flight-package-object)。 在呼叫這個方法以更新 App 提交時，要求主體中只需要這些物件的 *fileName*、*fileStatus*、*minimumDirectXVersion* 和 *minimumSystemRam* 值。 合作夥伴中心會填入其他值。 |
| packageDeliveryOptions    | 物件  | 包含提交的漸進式套件推出和強制更新設定。 如需詳細資訊，請參閱[套件交付選項物件](manage-flight-submissions.md#package-delivery-options-object)。  |
| targetPublishMode           | 字串  | 提交的發佈模式。 這可以是下列其中一個值： <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | 字串  | 如果將 *targetPublishMode* 設為 SpecificDate，則為 ISO 8601 格式的提交發佈日期。  |
| notesForCertification           | 字串  |  為認證測試人員提供其他資訊，例如測試帳戶認證，以及存取和確認功能的步驟。 如需詳細資訊，請參閱[認證注意事項](https://docs.microsoft.com/windows/uwp/publish/notes-for-certification)。 |


### <a name="request-example"></a>要求範例

下列範例示範如何為 App 更新套件正式發行前小眾測試版提交。

```json
PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649 HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0.0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

## <a name="response"></a>回應

下列範例示範成功呼叫此方法時的 JSON 回應主體。 回應主體包含更新提交的相關資訊。 如需回應本文中各個值的詳細資訊，請參閱[套件正式發行前小眾測試版提交資源](manage-flight-submissions.md#flight-submission-object)。

```json
{
  "id": "1152921504621243649",
  "flightId": "cd2e368a-0da5-4026-9f34-0e7934bc6f23",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "id": "",
      "version": "1.0.0.0",
      "languages": ["en-us"],
      "capabilities": [],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0.0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/8b389577-5d5e-4cbe-a744-1ff2e97a9eb8?sv=2014-02-14&sr=b&sig=wgMCQPjPDkuuxNLkeG35rfHaMToebCxBNMPw7WABdXU%3D&se=2016-06-17T21:29:44Z&sp=rwl",
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

## <a name="error-codes"></a>錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述   |
|--------|------------------|
| 400  | 無法更新套件正式發行前小眾測試版提交，因為要求無效。 |
| 409  | 無法更新封裝飛行提交，因為應用程式中的目前狀態，或應用程式使用的合作夥伴中心功能[目前不支援 Microsoft Store 提交 API](create-and-manage-submissions-using-windows-store-services.md#not_supported)。 |   


## <a name="related-topics"></a>相關主題

* [建立和管理使用 Microsoft Store 服務的提交內容](create-and-manage-submissions-using-windows-store-services.md)
* [管理封裝飛行提交](manage-flight-submissions.md)
* [取得封裝飛行提交](get-a-flight-submission.md)
* [建立套件飛行提交](create-a-flight-submission.md)
* [認可套件飛行提交](commit-a-flight-submission.md)
* [刪除套件飛行提交](delete-a-flight-submission.md)
* [取得封裝飛行提交狀態](get-status-for-a-flight-submission.md)
