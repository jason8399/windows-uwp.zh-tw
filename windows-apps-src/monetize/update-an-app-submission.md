---
author: mcleanbyron
ms.assetid: E8751EBF-AE0F-4107-80A1-23C186453B1C
description: "使用 Windows 市集提交 API 中的這個方法，更新現有的 App 提交。"
title: "使用 Windows 市集提交 API 更新 App 提交"
translationtype: Human Translation
ms.sourcegitcommit: 178b70db1583790c174d65e060c8bce6e4f69243
ms.openlocfilehash: ad1c565f1ec84127b2ac689cc7cb23d2b39764ef

---

# 使用 Windows 市集提交 API 更新 App 提交




使用 Windows 市集提交 API 中的這個方法，更新現有的 App 提交。 使用這個方法成功更新提交之後，您必須針對擷取和發佈[認可提交](commit-an-app-submission.md)。

如需這個方法如何在使用 Windows 市集提交 API 建立 App 提交的程序中進行的詳細資訊，請參閱[管理 App 提交](manage-app-submissions.md)。

## 先決條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Windows 市集提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。
* 針對您開發人員中心帳戶中的 App 建立提交。 您可以在開發人員中心儀表板中進行，或者可以使用[建立 App 提交](create-an-app-submission.md)方法進行。

>**注意**&nbsp;&nbsp;這個方法僅供已被授權使用 Windows 市集提交 API 的 Windows 開發人員中心帳戶使用。 並非所有的帳戶都已啟用此權限。

## 要求

這個方法的語法如下。 請參閱下列各小節了解標頭和要求本文的使用範例和描述。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| PUT   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}  ``` |

<span/>
 

### 要求標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |

<span/>

### 要求參數

| 名稱        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 字串 | 必要。 您想要更新提交之 App 的市集識別碼。 如需有關市集識別碼的詳細資訊，請參閱[檢視 App 身分識別詳細資料](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)。  |
| submissionId | 字串 | 必要。 要更新之提交的識別碼。 識別碼可在開發人員中心儀表板上取得，並且包含在[建立 App 提交](create-an-app-submission.md)要求的回應資料中。  |

<span/>

### 要求本文

要求本文包含下列參數。

| 值      | 類型   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applicationCategory           | 字串  |   指定 App [類別和/或子類別](https://msdn.microsoft.com/windows/uwp/publish/category-and-subcategory-table)的字串。 類別與子類別會使用底線 '_' 字元結合為單一字串，例如 **BooksAndReference_EReader**。      |  
| pricing           |  物件  | 此物件包含 App 的定價資訊。 如需詳細資訊，請參閱[定價資源](manage-app-submissions.md#pricing-object)一節。       |   
| visibility           |  字串  |  App 的可見度。 這可以是下列其中一個值： <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>       |   
| targetPublishMode           | 字串  | 提交的發佈模式。 這可以是下列其中一個值： <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | 字串  | 如果將 *targetPublishMode* 設為 SpecificDate，則為 ISO 8601 格式的提交發佈日期。  |  
| listings           |   物件  |  機碼和值組的字典，其中每個機碼都是國家/地區代碼，而每個值都是[清單資源](manage-app-submissions.md#listing-object)物件，其中包含 App 的清單資訊。       |   
| hardwarePreferences           |  array  |   定義 App [硬體喜好設定](https://msdn.microsoft.com/windows/uwp/publish/enter-app-properties#hardware_preferences)的字串陣列。 這可以是下列其中一個值： <ul><li>Touch</li><li>Keyboard</li><li>Mouse</li><li>Camera</li><li>NfcHce</li><li>Nfc</li><li>BluetoothLE</li><li>Telephony</li></ul>     |   
| automaticBackupEnabled           |  布林值  |   指出 Windows 是否可以在自動備份至 OneDrive 時包含您 App 的資料。 如需詳細資訊，請參閱[應用程式宣告](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)。   |   
| canInstallOnRemovableMedia           |  布林值  |   指出客戶是否可以將您的 App 安裝到抽取式存放裝置。 如需詳細資訊，請參閱[應用程式宣告](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)。     |   
| isGameDvrEnabled           |  布林值 |   指出是否已針對 App 啟用遊戲 DVR。    |   
| hasExternalInAppProducts           |     布林值          |   指出您的 App 是否允許使用者在 Windows 市集商務系統外部進行購買。 如需詳細資訊，請參閱[應用程式宣告](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)。     |   
| meetAccessibilityGuidelines           |    布林值           |  指出您的 App 是否已經過測試，符合協助工具指導方針。 如需詳細資訊，請參閱[應用程式宣告](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)。      |   
| notesForCertification           |  字串  |   包含您 App 的[認證注意事項](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification)。    |    
| applicationPackages           |   array  | 包含可提供關於提交中每個套件之詳細資料的物件。 如需詳細資訊，請參閱[應用程式套件](manage-app-submissions.md#application-package-object)一節。 在呼叫這個方法以更新 App 提交時，要求本文中只需要這些物件的 *fileName*、*fileStatus*、*minimumDirectXVersion* 和 *minimumSystemRam* 值。 其他值均是由開發人員中心所填入。   |    
| enterpriseLicensing           |  字串  |  其中一個[企業授權值](#enterprise-licensing)，可指出 App 適用的企業授權行為。  |    
| allowMicrosftDecideAppAvailabilityToFutureDeviceFamilies           |  布林值   |  指出是否允許 Microsoft [讓 App 可供未來的 Windows 10 裝置系列使用](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families)。    |    
| allowTargetFutureDeviceFamilies           | 布林值   |  指出是否允許您的 App [以未來的 Windows 10 裝置系列為目標](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families)。     |    

<span/>

### 要求範例

下列範例示範如何更新 App 提交。

```json
PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621230023 HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2"
  },
  "visibility": "Public",
  "targetPublishMode": "Manual",
  "targetPublishDate": "1601-01-01T00:00:00Z",
  "listings": {
    "en-us": {
      "baseListing": {
        "copyrightAndTrademarkInfo": "",
        "keywords": [],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [],
        "releaseNotes": "",
        "images": [
          {
            "fileName": "contoso.png",
            "fileStatus": "Uploaded",
            "id": "1152921504672272757",
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "ApiTestApp For Devbox"
      },
      "platformOverrides": {}
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
  "hasExternalInAppProducts": false,
  "meetAccessibilityGuidelines": true,
  "notesForCertification": "",
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "PendingUpload",
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  }
}
```

## 回應

下列範例示範成功呼叫這個方法的 JSON 回應本文。 回應本文包含更新提交的相關資訊。 如需回應本文中各個值的詳細資訊，請參閱 [App 提交資源](manage-app-submissions.md#app-submission-object)。

```json
{
  "id": "1152921504621243540",
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2"
  },
  "visibility": "Public",
  "targetPublishMode": "Manual",
  "targetPublishDate": "1601-01-01T00:00:00Z",
  "listings": {
    "en-us": {
      "baseListing": {
        "copyrightAndTrademarkInfo": "",
        "keywords": [],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [],
        "releaseNotes": "",
        "images": [
          {
            "fileName": "contoso.png",
            "fileStatus": "Uploaded",
            "id": "1152921504672272757",
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "ApiTestApp For Devbox"
      },
      "platformOverrides": {}
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
  "hasExternalInAppProducts": false,
  "meetAccessibilityGuidelines": true,
  "notesForCertification": "",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/387a9ea8-a412-43a9-8fb3-a38d03eb483d?sv=2014-02-14&sr=b&sig=sdd12JmoaT6BhvC%2BZUrwRweA%2Fkvj%2BEBCY09C2SZZowg%3D&se=2016-06-17T18:32:26Z&sp=rwl",
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "PendingUpload",
      "id": "1152921504620138797",
      "version": "1.0.0.0",
      "architecture": "ARM",
      "languages": [
        "en-US"
      ],
      "capabilities": [
        "ID_RESOLUTION_HD720P",
        "ID_RESOLUTION_WVGA",
        "ID_RESOLUTION_WXGA"
      ],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None",
      "targetDeviceFamilies": [
        "Windows.Mobile min version 10.0.10240.0"
      ]
    }
  ],
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "friendlyName": "Submission 2"
}
```

## 錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述   |
|--------|------------------|
| 400  | 無法更新提交，因為要求無效。 |
| 409  | 無法更新提交，因為 App 的目前狀態，或 App 是使用 [Windows 市集提交 API 目前不支援](create-and-manage-submissions-using-windows-store-services.md#not_supported)的開發人員中心儀表板功能。 |   

<span/>


## 相關主題

* [使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [取得 App 提交](get-an-app-submission.md)
* [建立 App 提交](create-an-app-submission.md)
* [認可 App 提交](commit-an-app-submission.md)
* [刪除 App 提交](delete-an-app-submission.md)
* [取得 App 提交的狀態](get-status-for-an-app-submission.md)



<!--HONumber=Aug16_HO5-->

