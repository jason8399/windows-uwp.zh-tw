---
ms.assetid: b556a245-6359-4ddc-a4bd-76f9873ab694
description: 在 Microsoft Store 分析 API 中使用此方法，以取得 App 中錯誤的堆疊追蹤。
title: 取得應用程式中錯誤的堆疊追蹤
ms.date: 06/05/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 服務, Microsoft Store 分析 API, 堆疊追蹤, 錯誤
ms.localizationpriority: medium
ms.openlocfilehash: ceffc7622f756eb17c8475208852e013df814554
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627453"
---
# <a name="get-the-stack-trace-for-an-error-in-your-app"></a>取得應用程式中錯誤的堆疊追蹤

在 Microsoft Store 分析 API 中使用此方法，以取得 App 中錯誤的堆疊追蹤。 這個方法只可以下載最近 30 天發生之 App 錯誤的堆疊追蹤。 堆疊追蹤中也會有**失敗**一節[健康情況報告](../publish/health-report.md)在合作夥伴中心。

使用這個方法之前，您必須先使用[取得 App 中錯誤的詳細資料](get-details-for-an-error-in-your-app.md)方法來擷取與您想要擷取堆疊追蹤的錯誤相關聯之 CAB 檔案的識別碼。

## <a name="prerequisites"></a>必要條件


若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。
* 取得與您想要擷取堆疊追蹤的錯誤相關聯之 CAB 檔案的識別碼。 若要取得此識別碼，請使用[取得 App 中錯誤的詳細資料](get-details-for-an-error-in-your-app.md)方法以擷取您的 App 中特定錯誤的詳細資料，並在該方法的回應主體中使用 **cabId** 值。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/stacktrace``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 在表單中的 Azure AD 存取權杖**持有人** &lt;*語彙基元*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  描述      |  必要  |
|---------------|--------|---------------|------|
| applicationId | 字串 | 您想要取得堆疊追蹤之 App 的市集識別碼。 存放區識別碼位於[應用程式識別碼 頁面上](../publish/view-app-identity-details.md)在合作夥伴中心。 舉例來說，市集識別碼可以是「9WZDNCRFJ3Q8」。 |  是  |
| cabId | 字串 | 與您想要擷取堆疊追蹤的錯誤相關聯之 CAB 檔案的唯一識別碼。 若要取得此識別碼，請使用[取得 App 中錯誤的詳細資料](get-details-for-an-error-in-your-app.md)方法以擷取您的 App 中特定錯誤的詳細資料，並在該方法的回應主體中使用 **cabId** 值。 |  是  |

 
### <a name="request-example"></a>要求範例

下列範例示範如何使用此方法取得堆疊追蹤。 將 *applicationId* 值以您應用程式的市集識別碼取代。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/stacktrace?applicationId=9NBLGGGZ5QDR&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應主體

| 值      | 類型    | 描述                  |
|------------|---------|--------------------------------|
| 值      | 陣列   | 物件的陣列，每個物件都包含一個堆疊追蹤資料框架。 如需有關每個物件中資料的詳細資訊，請參閱下方的[堆疊追蹤值](#stack-trace-values)一節。 |
| @nextLink  | 字串  | 如果還有其他資料頁面，此字串會包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 10，但是查詢卻有超過 10 個資料列的錯誤，就會傳回此值。 |
| TotalCount | 整數 | 查詢之資料結果的資料列總數。          |


### <a name="stack-trace-values"></a>堆疊追蹤值

*Value* 陣列中的元素包含下列值。

| 值           | 類型    | 描述      |
|-----------------|---------|----------------|
| level            | 字串  |  此元素在呼叫堆疊中所代表的框架數目。  |
| image   | 字串  |   可執行檔名稱或程式庫映像，包含在此堆疊框架中呼叫的函式。           |
| function | 字串  |  在此堆疊框架中呼叫的函式名稱。 只有當您的 App 包含可執行檔或程式庫的符號時才能使用。              |
| offset     | 字串  |  相對於函式開始之目前指示的位元組位移。      |


### <a name="response-example"></a>回應範例

下列範例示範這個要求的一個範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "level": "0",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.MainPage.DoWork",
      "offset": "0x25C"
    }
    {
      "level": "1",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.MainPage.Initialize",
      "offset": "0x26"
    }
    {
      "level": "2",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.Start",
      "offset": "0x66"
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}

```

## <a name="related-topics"></a>相關主題

* [健康情況報告](../publish/health-report.md)
* [使用 Microsoft Store 服務的存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得錯誤報告資料](get-error-reporting-data.md)
* [取得您的應用程式中的錯誤詳細資料](get-details-for-an-error-in-your-app.md)
* [下載您的應用程式中發生錯誤的 CAB 檔案](download-the-cab-file-for-an-error-in-your-app.md)
