---
author: mcleanbyron
ms.assetid: C09F4B7C-6324-4973-980A-A60035792EFC
description: "使用 Windows 市集提交 API 中的這個方法為登錄到您 Windows 開發人員中心帳戶的 App 建立新的附加元件提交。"
title: "使用 Windows 市集提交 API 建立附加元件提交"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: b7de4b00fb4d48b9f4c542437c38e0282e155a29

---

# 使用 Windows 市集提交 API 建立附加元件提交




使用 Windows 市集提交 API 中的這個方法為登錄到您 Windows 開發人員中心帳戶的 App 建立新的附加元件 (也稱為應用程式內產品或 IAP) 提交。 使用這個方法成功建立新提交之後，請[更新提交](update-an-add-on-submission.md)對提交的資料進行任何必要的變更，然後[認可提交](commit-an-add-on-submission.md)供擷取和發佈。

如需這個方法如何在使用 Windows 市集提交 API 建立附加元件提交的程序中進行的詳細資訊，請參閱[管理附加元件提交](manage-add-on-submissions.md)。

>**注意**&nbsp;&nbsp;這個方法會為現有的附加元件建立提交。 若要建立附加元件，請使用[建立附加元件](create-an-add-on.md)方法。

## 先決條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Windows 市集提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。
* 在您的開發人員中心帳戶中為 App 建立附加元件。 您可以在開發人員中心儀表板中進行，或者可以使用[建立附加元件](create-an-add-on.md)方法進行。

>**注意**&nbsp;&nbsp;這個方法僅供已被授權使用 Windows 市集提交 API 的 Windows 開發人員中心帳戶使用。 並非所有的帳戶都已啟用此權限。

## 要求

這個方法的語法如下。 請參閱下列各小節了解標頭和要求本文的使用範例和描述。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions``` |

<span/>
 

### 要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |

<span/>

### 要求參數

| 名稱        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | 字串 | 必要。 您想要建立提交之附加元件的市集識別碼。 市集識別碼可在開發人員中心儀表板上取得，並且包含在[建立附加元件](create-an-add-on.md)或[取得附加元件詳細資料](get-all-add-ons.md)要求的回應資料中。  |

<span/>

### 要求本文

不提供此方法的要求本文。

### 要求範例

下列範例示範如何為附加元件建立新的提交。

```
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions HTTP/1.1
Authorization: Bearer <your access token>
```

## 回應

下列範例示範成功呼叫這個方法的 JSON 回應本文。 回應本文包含新提交的相關資訊。 如需回應本文中各個值的詳細資訊，請參閱[附加元件提交資源](manage-add-on-submissions.md#add-on-submission-object)。

```json
{
  "id": "1152921504621243680",
  "contentType": "EMagazine",
  "keywords": [
    "books"
  ],
  "lifetime": "FiveDays",
  "listings": {
    "en": {
      "description": "English add-on description",
      "icon": {
        "fileName": "add-on-en-us-listing2.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (English)"
    },
    "ru": {
      "description": "Russian add-on description",
      "icon": {
        "fileName": "add-on-ru-listing.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (Russian)"
    }
  },
  "pricing": {
    "marketSpecificPricings": {
      "RU": "Tier3",
      "US": "Tier4",
    },
    "sales": [
      {
         "name": "Sale1",
         "basePriceId": "Free",
         "startDate": "2016-05-21T18:40:11.7369008Z",
         "endDate": "2016-05-22T18:40:11.7369008Z",
         "marketSpecificPricings": {
            "RU": "NotAvailable"
         }
      }
    ],
    "priceId": "Free"
  },
  "targetPublishDate": "2016-03-15T05:10:58.047Z",
  "targetPublishMode": "Immediate",
  "tag": "SampleTag",
  "visibility": "Public",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [
      {
        "code": "None",
        "details": "string"
      }
    ],
    "warnings": [
      {
        "code": "ListingOptOutWarning",
        "details": "You have removed listing language(s): []"
      }
    ],
    "certificationReports": [
      {
      }
    ]
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl",
  "friendlyName": "Submission 2"
}
```

## 錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述   |
|--------|------------------|
| 400  | 無法建立提交，因為要求無效。 |
| 409  | 無法建立提交，因為 App 的目前狀態，或 App 使用 [Windows 市集提交 API 目前不支援](create-and-manage-submissions-using-windows-store-services.md#not_supported)的開發人員中心儀表板功能。 |   

<span/>


## 相關主題

* [使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [管理附加元件提交](manage-add-on-submissions.md)
* [取得附加元件提交](get-an-add-on-submission.md)
* [認可附加元件提交](commit-an-add-on-submission.md)
* [更新附加元件提交](update-an-add-on-submission.md)
* [刪除附加元件提交](delete-an-add-on-submission.md)
* [取得附加元件提交的狀態](get-status-for-an-add-on-submission.md)



<!--HONumber=Aug16_HO5-->

