---
description: 在 Microsoft Store 分析 API 中使用此方法取得 Xbox Live 並行使用資料。
title: 取得 Xbox Live 並行使用資料
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10、uwp、Microsoft Store 服務、Microsoft Store 分析 API、Xbox Live 分析、並行使用
ms.localizationpriority: medium
ms.openlocfilehash: a1ceef92a533a230c2dca54a835578b56ceb809f
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321758"
---
# <a name="get-xbox-live-concurrent-usage-data"></a>取得 Xbox Live 並行使用資料


在 Microsoft Store 分析 API 中使用此方法可以取得有關在指定時間範圍內每分鐘、每小時或每天玩您的 [已啟用 Xbox Live 遊戲](https://docs.microsoft.com/gaming/xbox-live/index.md) 的平均客戶數量的即時使用量資料 (延遲 5-15 分鐘)。 這項資訊也會提供[Xbox 分析報告](../publish/xbox-analytics-report.md)在合作夥伴中心。

> [!IMPORTANT]
> 此方法僅支援 Xbox 遊戲，或使用 Xbox Live 服務的遊戲。 這些遊戲必須通盤了解[概念核准程序](../gaming/concept-approval.md)，包括 [Microsoft 合作夥伴](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md#microsoft-partners)發行的遊戲，以及透過  [ID@Xbox程式](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md#id)提交的遊戲。 此方法目前不支援透過 [Xbox Live 創作者計畫](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md) 發佈遊戲。

## <a name="prerequisites"></a>先決條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 在表單中的 Azure AD 存取權杖**持有人** &lt;*語彙基元*&gt;。 |


### <a name="request-parameters"></a>要求參數


| 參數        | 類型   |  描述      |  必要項  
|---------------|--------|---------------|------|
| applicationId | 字串 | 您想要擷取 Xbox Live 並行使用資料之遊戲的[ Store 識別碼](in-app-purchases-and-trials.md#store-ids)。  |  是  |
| metricType | 字串 | 指定要擷取之 Xbox Live 分析資料類型的字串。 對於此方法，請指定 **並行**值。  |  是  |
| startDate | date | 要擷取並行使用資料之日期範圍的開始日期。 有關預設行為，請參閱 *aggregationLevel* 說明。 |  否  |
| endDate | date | 要擷取並行使用資料之日期範圍的結束日期。 有關預設行為，請參閱 *aggregationLevel* 說明。 |  否  |
| aggregationLevel | 字串 | 指定要擷取彙總資料的時間範圍。 可以是下列其中一個字串：**分鐘**、**小時** 或  **天**。 如果沒有指定，則預設為 **day**。 <p/><p/>如果您不指定 *startDate* 或 *endDate*，回應主體預設如下： <ul><li>**分鐘**:可用的資料上次 60 的記錄。</li><li>**小時**:可用的資料最近的 24 個記錄。</li><li>**天**:可用的資料最後 7 的記錄。</li></ul><p/>下列彙總層級對可以傳回的記錄數量有大小限制。 如果要求的時間範圍太大，記錄將會被截斷。 <ul><li>**分鐘**:最多為 1440年記錄 （24 小時的資料）。</li><li>**小時**:最多 720 記錄 （30 天的資料）。</li><li>**天**:最多 60 記錄 （60 天的資料）。</li></ul>  |  否  |


### <a name="request-example"></a>要求範例

以下範例示範了用於已啟用 Xbox Live 遊戲取得並行使用資料的要求。 此要求會擷取 2018 年 2 月 1 日至 2018 年 2 月 2 日期間的每筆記錄資料。 將 *applicationId* 值取代為您的遊戲的 Store 識別碼。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=concurrency&aggregationLevel=hour&startDate=2018-02-01&endData=2018-02-02 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

回應主體包含物件的陣列，每個物件包含一組指定的分鐘、小時或天的並行使用資料。 每個物件包含下列值。

| 值      | 類型   | 描述                  |
|------------|--------|-------------------------------------------------------|
| Count      | 數字  | 在指定的分鐘、小時或天內玩已啟用 Xbox Live 的客戶的平均數量。 <p/><p/>**注意**&nbsp;&nbsp; 值為 0 表示在指定的時間間隔內沒有並行使用者，或者在指定的時間間隔內為遊戲收集並行使用者資料時發生錯誤。 |
| Date  | 字串 | 並行使用資料發生期間其詳列了分鐘、小時或天的日期。  |
| SeriesName | 字串    | 這總有 **UserConcurrency** 的值。 |


### <a name="response-example"></a>回應範例

下列範例示範使用於此要求的一個 JSON 回應主體範例，係依照記錄作資料彙整。

```json
[   {
        "Count": 418.0,
        "Date": "2018-02-02T04:42:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 418.0,
        "Date": "2018-02-02T04:43:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 415.0,
        "Date": "2018-02-02T04:44:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 412.0,
        "Date": "2018-02-02T04:45:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 414.0,
        "Date": "2018-02-02T04:46:13.65Z",
        "SeriesName": "UserConcurrency"
    }
]
```

## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務的存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得 Xbox Live 的分析資料](get-xbox-live-analytics.md)
* [取得 Xbox Live 的成就資料](get-xbox-live-achievements-data.md)
* [取得 Xbox Live 的健全狀況資料](get-xbox-live-health-data.md)
* [取得 Xbox Live 的遊戲中樞資料](get-xbox-live-game-hub-data.md)
* [取得 Xbox Live club 資料](get-xbox-live-club-data.md)
* [取得 Xbox Live 多人的資料](get-xbox-live-multiplayer-data.md)
