---
ms.assetid: 5BD650D2-AA26-4DE9-8243-374FDB7D932B
description: 在 Microsoft Store 提交 API 中使用這個方法，來建立應用程式已向 PartnerCenter 帳戶的附加元件。
title: 建立附加元件
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 提交 API, 建立附加元件, 應用程式內產品, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 00eb1a865631ce51cfa065d27ed00b44c66a6757
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371261"
---
# <a name="create-an-add-on"></a>建立附加元件

在 Microsoft Store 提交 API 中使用這個方法，來建立應用程式已向您的合作夥伴中心帳戶的附加元件 （也稱為應用程式內產品或 IAP）。

> [!NOTE]
> 這個方法會建立一個附加元件但不含任何提交。 若要為附加元件建立提交，請參閱[管理附加元件提交](manage-add-on-submissions.md)中的方法。

## <a name="prerequisites"></a>必要條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Microsoft Store 提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求

這個方法的語法如下。 請參閱下列各小節了解標頭和要求本文的使用範例和描述。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | `https://manage.devcenter.microsoft.com/v1.0/my/inappproducts` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 在表單中的 Azure AD 存取權杖**持有人** &lt;*語彙基元*&gt;。 |


### <a name="request-body"></a>要求本文

要求本文包含下列參數。

|  參數  |  類型  |  描述  |  必要項  |
|------|------|------|------|
|  applicationIds  |  陣列  |  此陣列包含此附加元件相關聯之 App 的市集識別碼。 此陣列只支援一個項目。   |  是  |
|  productId  |  字串  |  附加元件的產品識別碼。 這是可在程式碼中用來參考附加元件的識別碼。 如需詳細資訊，請參閱[設定您的產品類型和產品識別碼](https://docs.microsoft.com/windows/uwp/publish/set-your-iap-product-id)。  |  是  |
|  productType  |  字串  |  附加元件的產品類型。 支援下列值：**永久性**並**可取用**。  |  是  |


### <a name="request-example"></a>要求範例

下列範例示範如何為 App 建立新的消耗性附加元件。

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q...
Content-Type: application/json
{
    "applicationIds": [  "9NBLGGH4R315"  ],
    "productId": "my-new-add-on",
    "productType": "Consumable",
}
```

## <a name="response"></a>回應

下列範例示範成功呼叫此方法時的 JSON 回應主體。 如需回應本文中各個值的詳細資訊，請參閱[附加元件資源](manage-add-ons.md#add-on-object)。

```json
{
  "applications": {
    "value": [
      {
        "id": "9NBLGGH4R315",
        "resourceLocation": "applications/9NBLGGH4R315"
      }
    ],
    "totalCount": 1
  },
  "id": "9NBLGGH4TNMP",
  "productId": "my-new-add-on",
  "productType": "Consumable",
}
```

## <a name="error-codes"></a>錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述                                                                                                                                                                           |
|--------|------------------|
| 400  | 要求無效。 |
| 409  | 無法建立附加元件，因為其目前的狀態，或附加元件會使用合作夥伴中心功能都十分[目前不支援 Microsoft Store 提交 API](create-and-manage-submissions-using-windows-store-services.md#not_supported)。 |   


## <a name="related-topics"></a>相關主題

* [建立和管理使用 Microsoft Store 服務的提交內容](create-and-manage-submissions-using-windows-store-services.md)
* [管理附加元件提交](manage-add-on-submissions.md)
* [取得所有附加元件](get-all-add-ons.md)
* [取得附加元件](get-an-add-on.md)
* [刪除附加元件](delete-an-add-on.md)
