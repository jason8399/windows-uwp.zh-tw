---
Description: 瞭解如何接收您的應用程式、附加元件（應用程式內產品）和廣告收益的付款。
title: 取得付款
ms.assetid: 37D1EF45-C4A8-4849-8819-3D4A4898215C
ms.date: 05/29/2020
ms.topic: article
keywords: windows 10, uwp, 付款, 應用程式銷售, 應用程式收益, 支出, Microsoft Store 費用, 支付保留, 百分比
ms.localizationpriority: medium
ms.openlocfilehash: 0d42677aeda694e2fc8924cee1832b62d98b15e5
ms.sourcegitcommit: a937963ce63a14c254420926661b9b68be28a8ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/12/2020
ms.locfileid: "84746768"
---
# <a name="getting-paid"></a>取得付款
以下是有關接收您的應用程式、附加元件和廣告收益之付款的一些重要資訊。

> [!IMPORTANT]
> 您必須先[設定您的付款帳戶，並填寫必要的稅務形式](setting-up-your-payout-account-and-tax-forms.md)，才可以從 Microsoft Store 中的應用程式銷售額獲得金錢。

> [!NOTE]
> 如果您要尋找有關支出的支援，包括設定付款帳戶、遺失支出、在保存支出或任何其他專案，請在[這裡](https://developer.microsoft.com/windows/support)聯絡支援。

## <a name="store-fee"></a>市集費用

當您[註冊開發人員帳戶](https://developer.microsoft.com/store/register)時，會接受[應用程式開發人員合約](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)。 本合約說明當您在 Microsoft Store 銷售 App 時您與 Microsoft 之間的關係，其中包含 Microsoft 針對每筆銷售收取的 Microsoft Store 費用。

費用在[應用程式開發人員合約](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)中有正式的定義。 如果您有任何問題，請一律檢閱該文件。

Microsoft Store 費用適用於 Windows 市集收取的所有 App 銷售金額，包括附加元件。


## <a name="price-tiers"></a>價格區間

所選取的價格區間會在您選擇散佈應用程式的所有國家/地區設定[銷售價格](set-and-schedule-app-pricing.md#base-price)。 您也可以使用其他定價功能，例如[為不同市場的選擇不同的價格](set-and-schedule-app-pricing.md#override-base-price-for-specific-markets)或[降價促銷應用程式](put-apps-and-add-ons-on-sale.md)。

您可以免費提供應用程式，或是挑選一個客戶必須支付才能取得應用程式的價格。 價格區間從 .99 美元開始，逐額遞增 (1.09 美元、1.19 美元等)。 價格區間之間的遞增值會隨著價格變高而增加。

> [!NOTE] 
> 這些價格區間也適用於您從應用程式內提供的所有附加元件。

每個價格區間對於市集所提供的貨幣都有對應值。 我們使用這些值協助您在全球以可資比較的價格帶來銷售應用程式。 不過，由於貨幣匯率隨時都在變化，因此確切的銷售金額可能會隨貨幣種類的不同而有些微差異。 每月計算一次匯率。 根據您的交易發生的時間，會套用適當的匯率。 匯率和其實際的日期範圍會分別列在 exchangeRate 和 exchangeRateDate 資料行的支出報告中。

您也有選項可以輸入您選擇以特定市場當地貨幣表示的自由格式價格。 這樣做時，除非您提交新價格的更新，否則不會調整價格 (即使轉換率變更)。 

請記住，您所選取的價格可能會包含客戶必須支付的銷售或增值稅。 如需詳細資訊，請參閱[付費 app 的稅務詳細資料](tax-details-for-paid-apps.md)。


## <a name="payout-reporting"></a>付款報告

您可以存取有關付款資訊的詳細資料，並在[合作夥伴中心](https://partner.microsoft.com/dashboard)的**支出摘要**中下載報告。 如需此處所顯示的詳細資訊，以及我們將您所賺取金額分類的方式，請參閱[支付摘要](payout-summary.md)。


## <a name="payout-timeframe"></a>支付時間範圍

付款是每月進行一次 (假設已符合適用的付款門檻，而您尚未進行保留支付，如下所述)。 我們通常會在某個指定月份，於該月份的 15 日之前傳送任何付款。 請注意，付款通常需要 3 到 10 個額外工作天，才能送達您的支付帳戶。 如需詳細資訊，請參閱[付款門檻、方法和時間範圍](payment-thresholds-methods-and-timeframes.md)。


##  <a name="payout-hold-status"></a>支付保留狀態

我們預設會每個月傳送付款，如上所述。 不過，您可以選擇讓程式保持支出，這會導致我們無法傳送付款給您的帳戶。 如果您選擇保留支付，我們會繼續記錄任何您賺到的收入，並在 [**支付摘要**] 中提供詳細資料。 不過，除非您移除保留，否則我們不會將任何付款傳送到您的帳戶。

若要保留您的付款，請移至**開發人員設定**。 在 [支付**和稅務**] 底下的 [付款**和稅務設定檔指派**] 區段中，找出您想要保留付款的程式。 按一下 [**保留我的付款**] 核取方塊，以保留此計畫的付款。 您隨時都可以變更您的支付保留狀態，但是請注意您決定將會影響後續的每月支付。 例如，如果您想要保留年四月的支付，請務必在三月底之前將支付保留狀態設為 [**開啟**]。

當您將 [付款保留] 狀態設為 [**開啟**] 之後，此程式的所有支出都會保留，直到您將滑杆切換回 [**關閉**] 為止。 這樣做時，在下一個每月支付週期將會包含您 (假設已符合適用的付款門檻)。 例如，如果您已經保留支付，但想要產生六月的支付，則請確定在五月底之前將支付保留狀態切換為[**關閉**]。

> [!NOTE]
> 您的付款**保存狀態**會個別套用至每個程式（Microsoft Store、廣告、Azure Marketplace 等）。 如果您想要保存所有程式的款項，您必須個別支付每個程式的費用。


 

 




