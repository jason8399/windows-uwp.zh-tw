---
ms.assetid: D1F233EC-24B5-4F84-A92F-2030753E608E
description: 請在 Microsoft Store 集合 API 中使用這個方法，來取得與您 Azure AD 用戶端識別碼相關聯的應用程式中，某個客戶擁有的所有產品。 您可以把查詢範圍設定為特定產品，或是使用其他的篩選條件。
title: 查詢產品
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store collection API, view products, Microsoft Store 集合, 檢視產品
ms.localizationpriority: medium
ms.openlocfilehash: a5a986896ca3b2585590e11d17219106a9043204
ms.sourcegitcommit: 2747d9266e1678fca96d3822ce47499ca91a2c70
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/14/2020
ms.locfileid: "77252282"
---
# <a name="query-for-products"></a>查詢產品


請在 Microsoft Store 集合 API 中使用這個方法，來取得與您 Azure AD 用戶端識別碼相關聯的應用程式中，某個客戶擁有的所有產品。 您可以把查詢範圍設定為特定產品，或是使用其他的篩選條件。

這個方法是專門讓您的服務回應來自應用程式的訊息時所呼叫的方法。 您的服務不應該依排程定期輪詢所有的使用者。

## <a name="prerequisites"></a>必要條件


若要使用這個方法，您將需要：

* 一個具有對象 URI 值 `https://onestore.microsoft.com` 的 Azure AD 存取權杖。
* Microsoft Store 識別碼金鑰，代表您想要取得其產品之使用者的身分識別。

如需詳細資訊，請查看[管理服務的產品權利](view-and-grant-products-from-a-service.md)。

## <a name="request"></a>要求

### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                 |
|--------|-------------------------------------------------------------|
| POST   | ```https://collections.mp.microsoft.com/v6.0/collections/query``` |


### <a name="request-header"></a>要求的標頭

| 標頭         | 類型   | 描述                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Authorization  | string | 必要。 Azure AD 存取權杖的格式為**持有**人 &lt;*權杖*&gt;。                           |
| Host           | string | 其值必須設定為 **collections.mp.microsoft.com**。                                            |
| Content-Length | 數字 | 要求主體的長度。                                                                       |
| Content-Type   | string | 指定要求及回應類型。 目前唯一支援的值為 **application/json**。 |


### <a name="request-body"></a>要求本文

| 參數         | 類型         | 描述         | 必要項 |
|-------------------|--------------|---------------------|----------|
| beneficiaries     | 列出&lt;UserIdentity&gt; | UserIdentity 物件的清單，代表正在查詢產品的使用者。 如需詳細資訊，請參閱下表。    | 是      |
| continuationToken | string       | 如果有多組產品，回應主體會在達到頁面限制時傳回接續權杖。 為後續的呼叫提供該接續權杖，即可擷取剩餘的產品。       | 否       |
| maxPageSize       | 數字       | 為單一回應所傳回的產品數目上限。 預設及最大值為 100。                 | 否       |
| modifiedAfter     | datetime     | 如果已指定，該服務只會傳回在此日期之後修改過的產品。        | 否       |
| parentProductId   | string       | 如果已指定，該服務只會傳回對應到特定 App 的附加元件。      | 否       |
| productSkuIds     | list&lt;ProductSkuId&gt; | 如果已指定，該服務只會傳回適用於所提供產品/SKU 組的產品。 如需詳細資訊，請參閱下表。      | 否       |
| productTypes      | 列出&lt;字串&gt;       | 指定要在查詢結果中傳回的產品類型。 支援的產品類型包括**應用程式**、**耐用**、**遊戲**和**UnmanagedConsumable**。     | 是       |
| validityType      | string       | 設定為 **All** 時，會傳回某使用者的所有產品，包括已過期的項目。 設定為 **Valid** 時，只會傳回在當下有效的產品 (也就是該產品的狀態為使用中、開始日期 &lt; 目前時間，以及結束日期 &gt; 目前時間)。 | 否       |


UserIdentity 物件包含下列參數。

| 參數            | 類型   |  描述      | 必要項 |
|----------------------|--------|----------------|----------|
| identityType         | string | 指定字串值 **b2b**。    | 是      |
| identityValue        | string | [Microsoft Store 識別碼金鑰](view-and-grant-products-from-a-service.md#step-4)，代表您想要為其查詢產品之使用者的身分識別。  | 是      |
| localTicketReference | string | 所傳回產品的要求識別碼。 回應主體中的已傳回項目將會有相符的 *localTicketReference*。 建議您使用相同值做為在 Microsoft Store 識別碼索引鍵中宣告的 *userId*。 | 是      |


ProductSkuId 物件包含下列參數。

| 參數 | 類型   | 描述          | 必要項 |
|-----------|--------|----------------------|----------|
| productId | string | Microsoft Store 目錄中[產品](in-app-purchases-and-trials.md#store-ids)的 [Store 識別碼](in-app-purchases-and-trials.md#products-skus-and-availabilities)。 例如，產品的 Store 識別碼是 9NBLGGH42CFD。 | 是      |
| skuID     | string | Microsoft Store 目錄中產品 [SKU](in-app-purchases-and-trials.md#store-ids) 的 [Store 識別碼](in-app-purchases-and-trials.md#products-skus-and-availabilities)。 例如，SKU 的 Store 識別碼是 0010。       | 是      |


### <a name="request-example"></a>要求的範例

```syntax
POST https://collections.mp.microsoft.com/v6.0/collections/query HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q…….
Host: collections.mp.microsoft.com
Content-Length: 2531
Content-Type: application/json

{
  "maxPageSize": 100,
  "beneficiaries": [
    {
      "localTicketReference": "1055521810674918",
      "identityValue": "eyJ0eXAiOiJ……",
      "identityType": "b2b"
    }
  ],
  "modifiedAfter": "\/Date(-62135568000000)\/",
  "productSkuIds": [
    {
      "productId": "9NBLGGH5WVP6",
      "skuId": "0010"
    }
  ],
  "productTypes": [
    "UnmanagedConsumable"
  ],
  "validityType": "All"
}
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應本文

| 參數         | 類型                     | 描述          | 必要項 |
|-------------------|--------------------------|-----------------------|----------|
| continuationToken | string                   | 如果有多組產品，會在達到頁面限制時傳回此權杖。 您可以為後續的呼叫指定此接續權杖，來擷取剩餘的產品。 | 否       |
| 項目             | CollectionItemContractV6 | 特定使用者的產品陣列。 如需詳細資訊，請參閱下表。        | 否       |


CollectionItemContractV6 物件包含下列參數。

| 參數            | 類型               | 描述            | 必要項 |
|----------------------|--------------------|-------------------------|----------|
| acquiredDate         | datetime           | 使用者取得該項目的日期。                  | 是      |
| campaignId           | string             | 在購買此項目時所提供的行銷活動識別碼。                  | 否       |
| devOfferId           | string             | 來自 App 內購買的優惠識別碼。              | 否       |
| endDate              | datetime           | 項目的結束日期。              | 是      |
| fulfillmentData      | string             | N/A         | 否       |
| inAppOfferToken      | string             | 在合作夥伴中心指派給專案的開發人員指定產品識別碼字串。 範例產品識別碼為*product123*。 | 否       |
| itemId               | string             | 能將此集合項目與使用者擁有的其他項目區別的識別碼。 每個產品的此識別碼都是唯一的。   | 是      |
| localTicketReference | string             | 要求本文中先前所提供 *localTicketReference* 的識別碼。                  | 是      |
| modifiedDate         | datetime           | 此項目上次修改的日期。              | 是      |
| orderId              | string             | 如果存在，則取得此項目的訂單識別碼。              | 否       |
| orderLineItemId      | string             | 如果存在，則取得此項目之特定訂單的明細項目。              | 否       |
| ownershipType        | string             | 字串為 *「OwnedByBeneficiary」* 。   | 是      |
| productId            | string             | Microsoft Store 目錄中[產品](in-app-purchases-and-trials.md#store-ids)的 [ Store 識別碼](in-app-purchases-and-trials.md#products-skus-and-availabilities)。 例如，產品的 Store 識別碼是 9NBLGGH42CFD。          | 是      |
| productType          | string             | 下列其中一個產品類型：**Application**、**Durable** 及 **UnmanagedConsumable**。        | 是      |
| purchasedCountry     | string             | N/A   | 否       |
| purchaser            | IdentityContractV6 | 如果存在，則代表項目購買者的身分識別。 請在下方參閱此物件的詳細資料。        | 否       |
| quantity             | 數字             | 項目的數量。 目前此值永遠為 1。      | 否       |
| skuId                | string             | Microsoft Store 目錄中[產品 SKU](in-app-purchases-and-trials.md#store-ids)的 [Store 識別碼](in-app-purchases-and-trials.md#products-skus-and-availabilities)。 例如，SKU 的 Store 識別碼是 0010。     | 是      |
| skuType              | string             | SKU 的類型。 可能的值包括 **Trial**、**Full** 及 **Rental**。        | 是      |
| startDate            | datetime           | 項目開始生效的日期。       | 是      |
| status               | string             | 項目的狀態。 可能的值包括 **Active**、**Expired**、**Revoked** 及 **Banned**。    | 是      |
| tags                 | string             | N/A    | 是      |
| transactionId        | guid               | 因購買此項目而產生的交易識別碼。 可用來回報某項目已完成。      | 是      |


IdentityContractV6 物件包含下列參數。

| 參數     | 類型   | 描述                                                                        | 必要項 |
|---------------|--------|------------------------------------------------------------------------------------|----------|
| identityType  | string | 包含 *「pub」* 值。                                                      | 是      |
| identityValue | string | 來自特定 Microsoft Store 識別碼金鑰之 *publisherUserId* 的字串值。 | 是      |


### <a name="response-example"></a>回應範例

```syntax
HTTP/1.1 200 OK
Content-Length: 7241
Content-Type: application/json
MS-CorrelationId: 699681ce-662c-4841-920a-f2269b2b4e6c
MS-RequestId: a9988cf9-652b-4791-beba-b0e732121a12
MS-CV: xu2HW6SrSkyfHyFh.0.1
MS-ServerId: 020022359
Date: Tue, 22 Sep 2015 20:28:18 GMT

{
  "items" : [
    {
      "acquiredDate" : "2015-09-22T19:22:51.2068724+00:00",
      "devOfferId" : "f9587c53-540a-498b-a281-8a349491ed47",
      "endDate" : "9999-12-31T23:59:59.9999999+00:00",
      "fulfillmentData" : [],
      "inAppOfferToken" : "consumable2",
      "itemId" : "4b8fbb13127a41f299270ea668681c1d",
      "localTicketReference" : "1055521810674918",
      "modifiedDate" : "2015-09-22T19:22:51.2513155+00:00",
      "orderId" : "4ba5960d-4ec6-4a81-ac20-aafce02ddf31",
      "ownershipType" : "OwnedByBeneficiary",
      "productId" : "9NBLGGH5WVP6",
      "productType" : "UnmanagedConsumable",
      "purchaser" : {
        "identityType" : "pub",
        "identityValue" : "user123"
      },
      "skuId" : "0010",
      "skuType" : "Full",
      "startDate" : "2015-09-22T19:22:51.2068724+00:00",
      "status" : "Active",
      "tags" : [],
      "transactionId" : "4ba5960d-4ec6-4a81-ac20-aafce02ddf31"
    }
  ]
}
```

## <a name="related-topics"></a>相關主題

* [管理服務的產品權利](view-and-grant-products-from-a-service.md)
* [報告可耗用的產品](report-consumable-products-as-fulfilled.md)
* [授與免費產品](grant-free-products.md)
* [更新 Microsoft Store 識別碼金鑰](renew-a-windows-store-id-key.md)
