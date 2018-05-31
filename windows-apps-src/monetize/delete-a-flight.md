---
author: mcleanbyron
ms.assetid: AD80F9B3-CED0-40BD-A199-AB81CDAE466C
description: 使用 Microsoft Store 提交 API 中的這個方法為註冊到您 Windows 開發人員中心帳戶的 App 刪除套件正式發行前小眾測試版。
title: 刪除套件正式發行前小眾測試版
ms.author: mcleans
ms.date: 04/17/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, Microsoft Store 提交 API, 刪除正式發行前小眾測試版
ms.localizationpriority: medium
ms.openlocfilehash: d907d87929a64178b3ebd3d169e30b04f6577eb4
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/30/2018
ms.locfileid: "1815503"
---
# <a name="delete-a-package-flight"></a>刪除套件正式發行前小眾測試版

使用 Microsoft Store 提交 API 中的這個方法為註冊到您 Windows 開發人員中心帳戶的 App 刪除套件正式發行前小眾測試版。


## <a name="prerequisites"></a>先決條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Microsoft Store 提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求

這個方法的語法如下。 請參閱下列各小節了解標頭和要求主體的使用範例和描述。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}``` |


### <a name="request-header"></a>要求標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 名稱        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 字串 | 必要。 包含您想要刪除套件正式發行前小眾測試版之 App 的 Store 識別碼。 App 的 Store 識別碼可在開發人員中心儀表板上取得。  |
| flightId | 字串 | 必要。 要刪除的套件正式發行前小眾測試版識別碼。 識別碼可從[建立套件正式發行前小眾測試版](create-a-flight.md)和[取得 App 套件正式發行前小眾測試版](get-flights-for-an-app.md)要求的回應資料中取得。 對於開發人員中心儀表板中所建立的正式發行前小眾測試版，這個 ID 也適用於儀表板中正式發行前小眾測試版頁面的 URL。  |


### <a name="request-body"></a>要求本文

不提供此方法的要求主體。


### <a name="request-example"></a>要求範例

下列範例示範如何刪除套件正式發行前小眾測試版。

```
DELETE https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

如果成功，此方法會傳回空白回應主體。

## <a name="error-codes"></a>錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述                                                                                                                                                                           |
|--------|------------------|
| 400  | 要求參數無效。 |
| 404  | 找不到指定的套件正式發行前小眾測試版。  |
| 409  | 找到指定的套件正式發行前小眾測試版，但無法以其目前的狀態刪除，或 App 使用 [Microsoft Store 提交 API 目前不支援](create-and-manage-submissions-using-windows-store-services.md#not_supported)的開發人員中心儀表板功能。 |   


## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [建立套件正式發行前小眾測試版](create-a-flight.md)
* [取得套件正式發行前小眾測試版](get-a-flight.md)