---
description: 在 Microsoft Store 分析 API 中使用此方法，以針對特定日期範圍與其他選擇性篩選器，取得應用程式的依通道彙總轉換資料。
title: 依通路取得應用程式轉換
ms.date: 08/04/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 服務, Microsoft Store 分析 API, 應用程式轉換, 通道
ms.localizationpriority: medium
ms.openlocfilehash: 58eda007ad82431734028231e8f5ae02cc704fab
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493533"
---
# <a name="get-app-conversions-by-channel"></a>依通路取得應用程式轉換

在 Microsoft Store 分析 API 中使用此方法，以針對特定日期範圍與其他選擇性篩選器，取得應用程式的依通道彙總轉換。

* *轉換*表示客戶 (使用 Microsoft 帳戶登入) 已經新獲得您 App 的授權 (不論是付費或免費取得)。
* *通道*是客戶到達您應用程式清單頁面的方法 (例如，透過 Microsoft Store 或[自訂應用程式促銷活動](../publish/create-a-custom-app-promotion-campaign.md))。

您也可以在合作夥伴中心的 [[收購] 報告](../publish/acquisitions-report.md#app-page-views-and-conversions-by-channel)中取得這項資訊。

## <a name="prerequisites"></a>必要條件


若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您有 60 分鐘的使用時間，之後其便會到期。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/appchannelconversions``` |


### <a name="request-header"></a>要求標頭

| 頁首        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  說明      |  必要  
|---------------|--------|---------------|------|
| applicationId | 字串 | 您想要擷取轉換資料之應用程式的[ Store 識別碼](in-app-purchases-and-trials.md#store-ids)。 舉例來說，Store 識別碼可以是「9WZDNCRFJ3Q8」。 |  是  |
| startDate | date | 要擷取轉換資料之日期範圍的開始日期。 預設是 1/1/2016。 |  否  |
| endDate | date | 要擷取轉換資料之日期範圍的結束日期。 預設值是目前的日期。 |  否  |
| top | int | 在要求中傳回的資料列數目。 如果未指定，最大值和預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。 |  否  |
| skip | int | 在查詢中要略過的資料列數目。 使用此參數來瀏覽大型資料集。 例如，top=10000 且 skip=0 將擷取前 10000 個資料列的資料，top=10000 且 skip=10000 將擷取下 10000 個資料列的資料，以此類推。 |  否  |
| filter | 字串  | 一或多個篩選回應本文的陳述式。 每個陳述式都可以使用 **eq** 或 **ne** 運算子，而陳述式之間可以使用 **and** 或 **or** 來結合。 您可以在篩選陳述式中指定下列字串。 如需說明，請參閱本文的[轉換值](#conversion-values)一節。 <ul><li><strong>applicationName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>deviceType</strong></li><li><strong>細分</strong></li></ul><p>以下是 *filter* 參數範例：<em>filter=deviceType eq 'PC'</em>。</p> | 否   |
| aggregationLevel | 字串 | 指定要擷取彙總資料的時間範圍。 可以是下列其中一個字串：<strong>day</strong>、<strong>week</strong> 或 <strong>month</strong>。 如果沒有指定，則預設為 <strong>天</strong>。 | 否 |
| orderby | 字串 | 對每個轉換的結果資料值做出排序的陳述式。 語法為 <em>orderby=field [order],field [order],...</em>，其中 <em>field</em> 參數可以是下列其中一個字串︰<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>deviceType</strong></li><li><strong>細分</strong></li></ul><p><em>order</em> 參數為選擇性，並可以是 <strong>asc</strong> 或 <strong>desc</strong>，以指定每個欄位的遞增或遞減順序。 預設值是<strong>asc</strong>。</p><p>以下是範例<em>orderby</em>字串： <em>orderby = date，市</em></p> |  否  |
| groupby | 字串 | 將資料彙總僅套用至指定欄位的陳述式。 您可以指定下列欄位：<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>deviceType</strong></li><li><strong>細分</strong></li></ul><p>傳回的資料列將包含 <em>groupby</em> 參數中指定的欄位，以及下列項目：</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>conversionCount</strong></li><li><strong>clickCount</strong></li></ul><p><em>groupby</em> 參數可以搭配 <em>aggregationLevel</em> 參數使用。 例如： <em>groupby = ageGroup、市場 &amp; aggregationLevel = 周</em></p> |  否  |


### <a name="request-example"></a>要求範例

下列範例示範數個取得應用程式轉換資料的要求。 將 *applicationId* 值取代為您 App 的 Store 識別碼。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appchannelconversions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=2/1/2017&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appchannelconversions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=4/31/2017&skip=0&filter=market eq 'US'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應本文

| 值      | 類型   | 描述                  |
|------------|--------|-------------------------------------------------------|
| 值      | array  | 物件陣列，內含應用程式的彙總轉換資料。 如需有關每個物件中資料的詳細資訊，請參閱下方的[轉換值](#conversion-values)一節。                                                                                                                      |
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 10，但是查詢卻有超過 10 個資料列的轉換資料，就會傳回此值。 |
| TotalCount | int    | 查詢之資料結果的資料列總數。      |


### <a name="conversion-values"></a>轉換值

*Value* 陣列中的物件包含下列值。

| 值               | 類型   | 描述                           |
|---------------------|--------|-------------------------------------------|
| date                | 字串 | 轉換資料之日期範圍中的第一個日期。 如果要求指定單一天數，此值便會是該日期。 如果要求指定一週、一個月或其他日期範圍，此值便會是該日期範圍的第一個日期。 |
| applicationId       | 字串 | 您正在擷取轉換資料之應用程式的[ Store 識別碼](in-app-purchases-and-trials.md#store-ids)。     |
| applicationName     | 字串 | 您正在擷取轉換資料之應用程式的顯示名稱。        |
| appType          | 字串 |  您正在擷取轉換資料之應用程式的產品類型。 對於此方法，唯一支援的值是 **App**。            |
| customCampaignId           | 字串 |  與應用程式相關聯之[自訂應用程式促銷活動](../publish/create-a-custom-app-promotion-campaign.md)的識別碼字串。   |
| referrerUriDomain           | 字串 |  指定搭配已啟動之自訂應用程式促銷活動的應用程式清單所在的網域。   |
| channelType           | 字串 |  下列其中一個字串，指定轉換的通道︰<ul><li><strong>CustomCampaignId</strong></li><li><strong>Store Traffic</strong></li><li><strong>其他</strong></li></ul>    |
| storeClient         | 字串 | 發生轉換之 Microsoft Store 的版本。 目前唯一支援的值為 **SFC**。    |
| deviceType          | 字串 | 下列其中一個字串：<ul><li><strong>PC</strong></li><li><strong>來電</strong></li><li><strong>主控台-Xbox One</strong></li><li><strong>主控台-Xbox 系列 X</strong></li><li><strong>IoT</strong></li><li><strong>全息影像</strong></li><li><strong>Unknown</strong></li></ul>            |
| market              | 字串 | 發生轉換之市場的 ISO 3166 國家/地區碼。    |
| clickCount              | number  |     客戶點按您應用程式清單連結的數目。      |           
| conversionCount            | number  |   客戶轉換數。         |          


### <a name="response-example"></a>回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "date": "2016-01-01",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso App",
      "appType": "App",
      "customCampaignId": "",
      "referrerUriDomain": "Universal Client Store",
      "channelType": "Store Traffic",
      "storeClient": "SFC",
      "deviceType": "PC",
      "market": "US",
      "clickCount": 7,
      "conversionCount": 0
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>相關主題

* [下載數報告](../publish/acquisitions-report.md)
* [使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得應用程式下載數](get-app-acquisitions.md)
