---
author: mcleanbyron
ms.assetid: A4C6098B-6CB9-4FAF-B2EA-50B03D027FF1
description: 在 Microsoft Store 針對性優惠 API 中使用此方法，在目前的應用程式的內容中，取得適用於目前使用者的針對性優惠。
title: 取得針對性優惠
ms.author: mcleans
ms.date: 10/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10 uwp, Microsoft Store 服務, Microsoft Store 針對性優惠 API, 取得針對性優惠
ms.localizationpriority: medium
ms.openlocfilehash: 188561fb5dc6dda318a0b2aa7f1d91aa04c32548
ms.sourcegitcommit: 1773bec0f46906d7b4d71451ba03f47017a87fec
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/17/2018
ms.locfileid: "1661888"
---
# <a name="get-targeted-offers"></a>取得針對性優惠

使用此方法，根據使用者是不是針對性優惠客戶區隔的一部分，取得適用於目前使用者的針對性優惠。 如需詳細資訊，請參閱[使用 Microsoft Store 服務管理針對性優惠](manage-targeted-offers-using-windows-store-services.md)。

## <a name="prerequisites"></a>先決條件

若要使用此方式，您需要先為您的 app 目前已登入的使用者[取得 Microsoft 帳戶權杖](manage-targeted-offers-using-windows-store-services.md#obtain-a-microsoft-account-token)。 您必須在這個方法的 ```Authorization``` 要求標頭中傳遞這個權杖。 Microsoft Store 會使用此權杖，為目前使用者取得針對性優惠。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述  |
|---------------|--------|--------------|
| 授權 | 字串 | 必要。 您的 app 目前已登入的使用者的 Microsoft 帳戶權杖，格式為 **bearer**&lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

這個方法沒有 URI 或查詢參數。

### <a name="request-example"></a>要求範例

```syntax
GET https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user HTTP/1.1
Authorization: Bearer <Microsoft Account token>
```

## <a name="response"></a>回應

這個方法會傳回 JSON 格式化回應主體，包含有下列欄位的物件陣列。 陣列中的每個物件代表適用於特定客戶區隔中特定使用者的針對性優惠。

| 欄位      | 類型   | 描述         |
|------------|--------|------------------|
| offers      | array  | 附加元件的產品識別碼陣列，這些附加元件與適用於目前使用者的針對性優惠相關聯。 這些產品識別碼是在 Windows 開發人員中心儀表板中 app 的 **\[針對性優惠\]** 頁面中指定。            |
| trackingId  | 字串 | 您可選擇使用 GUID 來追蹤您自己代碼或服務內的針對性優惠。 |


### <a name="example"></a>範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
[
  {
    "offers": [
      "10x gold coins",
      "100x gold coins"
    ],
    "trackingId": "5de5dd29-6dce-4e68-b45e-d8ee6c2cd203"
  }
]
```

## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務管理針對性優惠](manage-targeted-offers-using-windows-store-services.md)

 

 