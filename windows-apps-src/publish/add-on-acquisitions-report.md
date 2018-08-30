---
author: jnHs
Description: The Add-on acquisitions report in the Windows Dev Center dashboard lets you see how many add-ons you've sold, along with demographic and platform details.
title: 附加元件下載數報告
ms.assetid: F2DF9188-0A98-4AC3-81C0-3E2C37B15582
ms.author: wdg-dev-content
ms.date: 05/24/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 附加元件銷售, 附加元件下載數, iap 銷售, 應用程式內產品, iap, 附加元件
ms.localizationpriority: medium
ms.openlocfilehash: 019bb410e6ac65f9951f06052c78f40e9a5f32e2
ms.sourcegitcommit: 7efffcc715a4be26f0cf7f7e249653d8c356319b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/30/2018
ms.locfileid: "3114485"
---
# <a name="add-on-acquisitions-report"></a>附加元件下載數報告


在 Windows 開發人員中心儀表板的**附加元件下載數**報告可讓您查看有多個附加元件，您所銷售、 人數統計資料及平台詳細資訊，並顯示 Windows 10 （包括 Xbox） 客戶的轉換資訊。 您也可以檢視的最後一個小時或 seventy 兩個小時期間接近即時的下載數資料。

您可以在儀表板中檢視此資料，或是[下載報告](download-analytic-reports.md)來離線檢視。 或者，您可透過程式設計方式使用 [Microsoft Store 分析 REST API](../monetize/access-analytics-data-using-windows-store-services.md) 中的[取得附加元件下載數](../monetize/get-in-app-acquisitions.md)方法來擷取此資料。

在此報告中，附加元件下載數表示客戶已經向您購買附加元件（或取得而不需付費，如果您免費提供附加元件）。 同一位客戶購買多個相同的消耗型附加元件會當作個別的附加元件下載數來計算。

> [!IMPORTANT]
> **附加元件下載數**報告不包含有關退款、解除安裝、信用卡退款等相關資料。若要評估您的應用程式收益，請造訪[支付摘要](payout-summary.md)。 在 **\[保留\]** 區段中，按一下 **\[下載保留的交易\]** 連結。


## <a name="apply-filters"></a>套用篩選

在頁面頂端附近，您可以選取您想要顯示資料的時間週期。 預設選項是 **30D**（30 天），但您可以選擇在 3、6 或 12 個月期間顯示資料，或指定自訂的日期範圍。 您也可以選取**1 H**或**72h**近乎即時顯示下載數資料的一小時或 seventy 兩個小時。**附加元件下載數**圖表的 [**附加元件每日**] 索引標籤，以及**市場**圖表的 [**下載**] 索引標籤，僅適用於這些時間期間。 

您也可以展開 **\[篩選條件\]**，依特定附加元件、市場和/或裝置類型篩選此頁面上的所有資料。

-   **附加元件**：預設篩選條件是 **\[所有附加元件\]**，但您可以限制資料為 App 的一或多個附加元件。
-   **市場**：預設篩選條件是 **\[所有市場\]**，但您可以限制資料為一或多個市場中的下載數。
-   **裝置類型**：預設設定為 **\[所有裝置\]**。 如果您只想要顯示來自特定裝置類型 (例如個人電腦、主機或平板電腦) 的下載數資料，您可以在此處選擇特定的類型。

下列所有圖表中的資訊將反映您選取的日期範圍和任何篩選條件。 某些區段也可讓您套用其他篩選。


## <a name="add-on-acquisitions"></a>附加元件下載數

**\[附加元件下載數\]** 圖表會顯示在選取時段內，每日或每週取得您附加元件的數目  (當您利用 **\[套用篩選\]** 顯示較長期間的資料時，下載數資料將會以週為單位分組)。

您也可以透過選取 **\[Add-on cumulative\]** (附加元件累積)，看到您 app 生命週期內的下載數。 這會顯示所有下載數的累計總數 (從您 App 第一次發行開始)。

您可以選擇性地依據附加元件下載是來自用戶端或 web 架構市集，和（或）依據 OS 版本來篩選結果。


## <a name="customer-demographic"></a>客戶人口統計資料

**\[客戶人口統計資料\]** 圖表顯示取得您附加元件對象的人數統計資訊。 您可以依特定年齡層和依性別來查看取得對象在所選時段內的下載數。

> [!NOTE]
> 部分客戶選擇不分享此資訊。 如果我們無法判斷年齡層或性別，即會將該次下載數分類為 **\[未知\]**。


## <a name="markets"></a>市場

**市場**圖表會依您 App 上市的每個市場顯示所選時段內的附加元件下載總數。 

您可以使用視覺**地圖**形式檢視這些資料，或切換設定，以**表格**形式檢視資料。 表格形式一次顯示五個市場，依字母排序或依最高/最低下載數或安裝數排序。 您也可以下載資料，一起檢視所有市場資訊。


## <a name="add-on-page-views-and-conversions-by-campaign-id"></a>附加元件頁面檢視數與轉換數 (依行銷活動識別碼)

**附加元件頁面檢視數與轉換數 (依行銷活動識別碼)** 圖表顯示在所選時段內，每個行銷活動識別碼的附加元件轉換 (下載) 總數，協助您追蹤每個[自訂促銷活動](create-a-custom-app-promotion-campaign.md)的 Windows 10 (包括 Xbox) 客戶轉換數和頁面檢視數。 此圖表僅顯示附加元件轉換。

> [!NOTE]
> 客戶可能透過按一下並非由您建立的自訂行銷活動，而連到您的應用程式清單。 我們將工作階段中的每個頁面檢視都加上客戶首度進入市集的行銷活動識別碼。 接著，對於 24 小時內的所有下載，我們會將轉換歸至該行銷活動識別碼。 因此，您可能會看到比您行銷活動識別碼轉換總計更多的轉換總計，以及可能會有頁面檢視為零的轉換或附加元件轉換。 


## <a name="conversions-breakdown-by-campaign-id"></a>轉換明細分析 (依行銷活動識別碼)

**轉換明細分析 (依行銷活動識別碼)** 圖表可讓您追蹤每個[自訂促銷活動](create-a-custom-app-promotion-campaign.md)的 Windows 10 客戶轉換數和頁面檢視數。 應用程式和附加元件轉換數都是依據每個行銷活動識別碼顯示。

在這個圖表，一個*頁面檢視*表示一位客戶檢視應用程式的市集清單。 *轉換*表示客戶已經新獲得您 App 或附加元件的授權 (不論是付費或免費取得)。

請牢記這些頁面檢視數與轉換數並非不重複客戶的計數。 


## <a name="top-add-ons"></a>熱門附加元件

**\[熱門附加元件\]** 圖表會顯示所選時段內每個附加元件的下載總數，因此您可以看到哪些是最熱門的附加元件。 



 

 