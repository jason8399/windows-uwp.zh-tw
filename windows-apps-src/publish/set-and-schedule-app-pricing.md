---
Description: 選取應用程式的基本價格並排程價格變更。 您也可以針對特定市場自訂這些選項。
title: 設定與排程應用程價格
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 定價, App 定價, App 價格, 銷售應用程式, 價格變更, 自訂價格, 價格, 定價, 成本, 覆寫基本價格, 自由格式價格, 自由格式
ms.localizationpriority: medium
ms.openlocfilehash: 451a22ffef2d8062de7bf7d29d921db7197987b5
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210494"
---
# <a name="set-and-schedule-app-pricing"></a>設定與排程應用程價格

**價格和可用性**頁面的 [\[價格\]](set-app-pricing-and-availability.md) 區段可讓您選取應用程式的基本價格。 您也可以[排程價格變更](#schedule-price-changes)，表示應用程式的價格應變更的日期和時間。 此外，您還有選項可以選取新的價格區間，或輸入以市場當地貨幣表示的自由格式價格，來[覆寫特定市場的基本價格](#override-base-price-for-specific-markets)。

> [!NOTE]
> 雖然本主題是關於應用程式，但是附加元件提交的價格選擇使用相同的程序。 請注意，針對訂用帳戶[附加](../monetize/enable-subscription-add-ons-for-your-app.md)元件，您所選取的基本價格將無法增加（不論是藉由變更基本價格或藉由排程價格變更），不過它可能會降低。

## <a name="base-price"></a>基本價格

當您選取您的 App 的 **\[基本價格\]** 時，除非您覆寫任何市場的基本價格，否則會在所有銷售 App 的市場中使用該價格。

您可以將 **\[基本價格\]** 設定為 **\[免費\]** ，或者您可以選擇可用的價格區間，藉此設定您選擇散發應用程式的所有國家/地區的價格。 價格區間從 0.99 美元開始，其餘區間依次遞增提供 (1.09 美元、1.19 美元，依此類推)。 遞增值一般會隨著價格變高而增加。 

> [!NOTE]
> 這些價格區間也適用於附加元件。 

每個價格區間對於市集所提供 60 種以上的貨幣都有對應值。 我們使用這些值協助您在全球以可資比較的價格帶來銷售應用程式。 您可以選擇任何貨幣的基本價格，我們會針對不同市場自動使用不同的對應值。 請注意，我們有時可能會調整特定市場中的對應值以因應貨幣轉換率變更。

在 **\[價格\]** 區段中，按一下 **\[檢視轉換表\]** 查看所有貨幣的相對價格。 這也會顯示與每個價區間相關聯的識別碼，如果您使用 [Microsoft Store 提交 API](../monetize/manage-app-submissions.md#price-tiers) 輸入價格就會需要此識別碼。 您可以按一下 **\[下載\]** ，下載 .csv 檔案格式的價格區間表複本。

請記住，您選取的價格區間可以包含客戶必須支付的銷售或增值稅。 若要深入了解您的應用程式在選取市場中的稅務關連相關資訊，請參閱[付費應用程式的稅務詳細資料](tax-details-for-paid-apps.md)。 另外建議您檢閱[特定市場的定價考量](define-market-selection.md#price-considerations-for-specific-markets)。

> [!NOTE]
> 如果您在 [[可見度](choose-visibility-options.md#discoverability)] 區段的存放區中選擇 [**讓此產品可供使用但無法**探索] 下的 [**停止**取得] 選項，您將無法設定提交的定價（因為沒有人可以取得應用程式，除非他們使用促銷代碼來免費取得應用程式）。

## <a name="schedule-price-changes"></a>排程價格變更

如果您想要在特定日期和時間變更應用程式的基本價格，可以選擇性地排程一次或多次價格變更。 

> [!IMPORTANT]
> 價格變更只會對使用 Windows 10 裝置 (包括 Xbox) 的客戶顯示。 如果您先前發佈的應用程式支援較舊的 OS 版本，則價格變更不會套用到這些客戶。 對於使用 Windows 8 的客戶，應用程式一律以 [**基本價格**] 提供 (而非任何市場特定價格)，即使您排程額外的價格變更。 對於 Windows 8.1 的客戶和 Windows Phone 8.1 和更早版本，應用程式一律會在客戶市場的第一個定價層提供。

按一下 **\[排程價格變更\]** 查看價格變更選項。 選擇您想要使用的價格區間 (或輸入單一市場基本價格覆寫的自由格式價格)，然後選取日期、時間和時區。

您可以按一下 [**排程價格變更**]，視需要排程所需的後續變更。

> [!NOTE]
> 排程的價格變更與[銷售定價](put-apps-and-add-ons-on-sale.md)運作方式不同。 當您降價促銷 App 時，價格會在 Microsoft Store 中以刪除線顯示，而客戶便可以在您選取的期間用廉售價格購買該 App。 廉售期間結束後，廉售價格就不再適用，App 將以其基本價格 (或是您為該市場所指定的不同價格，如果適用的話) 提供。
>
> 有了排程的價格變更，您就可以調高或調低價格。 變更將在您指定的日期生效，但是不會在 Microsoft Store 中顯示為降價促銷，套用任何特殊格式；App 只會有新的基本價格。 


## <a name="override-base-price-for-specific-markets"></a>覆寫特定市場的基本價格

根據預設，您在上方選取的選項將適用於提供應用程式所在的所有市場。 您也可以選擇不同的價格區間，或輸入以市場當地貨幣表示的自由格式價格，選擇性變更一個或多個市場的價格。

> [!IMPORTANT]
> 如果您先前發佈的應用程式支援 Windows 8，則即使您為市場選取不同的價格，這些客戶一律會以其**基本價格**看到應用程式。

若要變更特定市場的價格，請按一下 **\[選取市場以進行基本價格覆寫\]** 。 **\[選擇市場\]** 快顯視窗將會出現，列出您選擇提供應用程式的所有市場。 (如果您在 **\[市場\]** 區段中排除任何市場，就不會有這些市場)。 

您可以一次覆寫一個市場的基本價格，或一併覆寫市場群組的基本價格。 這麼做之後，就可以覆寫其他市場 (或其他市場群組) 的基本價格，方法是再次選取 **\[選取市場以進行基本價格覆寫\]** ，然後重複以下所述程序。 若要覆寫您為市場 (或市場群組) 指定的定價，請按一下 **\[移除\]** 。


### <a name="override-the-base-price-for-a-single-market"></a>覆寫單一市場的基本價格

若只要變更一個市場的價格，請選取該市場，然後按一下 **\[建立\]** 。 您將會看見相同的 [**基本價格**] 和 [**排程價格變更**] 選項，如上所述，但您所做的選擇會是針對該特定市場。 由於您只有覆寫一個市場的基本價格，價格區間將會以該市場的當地貨幣來顯示。 您可以按一下 **\[檢視轉換表\]** 來查看所有貨幣的相對價格。 

覆寫單一市場的基本價格也提供選項讓您輸入您所選擇以市場當地貨幣表示的自由格式價格。 您可以隨意輸入任何價格 (介於下限和上限範圍內)，即使未對應至其中一個標準價格區間。 這個價格用僅適用於所選市場中的 Windows 10 (包括 Xbox) 客戶。 

> [!IMPORTANT]
> 如果您輸入自由格式價格，除非您提交新價格的更新，否則不會調整該價格 (即使轉換率變更)。 

### <a name="override-the-base-price-for-a-market-group"></a>覆寫市場群組的基本價格

若要覆寫多個市場的基本價格，您可以建立*市場群組*。 若要這樣做，請選取您想要包括的市場，然後選擇性輸入群組的名稱 (此名稱僅供參考，不會對客戶顯示)。當您完成時，按一下 **\[建立\]** 。 您將會看見相同的 **\[基本價格\]** 和 **\[排程價格變更\]** 選項，如上所述，但您所做的選擇會是針對該特定市場群組。 請注意，自由格式價格無法用於市場群組。您必須選取可用的價格區間。

若要變更市場群組中包含的市場，請按一下市場群組的名稱，並新增或移除您想要的任何市場，然後按一下 **\[確定\]** 儲存變更。 

> [!NOTE]
> 一個市場不可屬於 **\[定價\]** 區段中的多個市場群組。





