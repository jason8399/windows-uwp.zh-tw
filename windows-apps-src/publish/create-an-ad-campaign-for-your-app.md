---
Description: 您可以在合作夥伴中心建立廣告行銷活動，以協助推廣您的應用程式並擴大應用程式的使用者群。
title: 為您的應用程式建立行銷活動
ms.assetid: 10D94929-92C4-4379-AA5F-6FEF879F2463
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, 廣告, 行銷活動, 促銷
ms.localizationpriority: medium
ms.openlocfilehash: aa5c3c160d3bb69a2ba478606a3c3e04e935088d
ms.sourcegitcommit: 71f9013c41fc1038a9d6c770cea4c5e481c23fbc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/20/2020
ms.locfileid: "77507062"
---
# <a name="create-an-ad-campaign-for-your-app"></a>為您的應用程式建立行銷活動

>[!WARNING]
> 從2020年6月1日起，適用于 Windows UWP 應用程式的 Microsoft Ad 營收平臺將會關閉。 [深入了解](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

您可以在[合作夥伴中心](https://partner.microsoft.com/dashboard)建立廣告行銷活動，以協助推廣您的應用程式並擴大其使用者群。 根據預設，我們會根據您在合作夥伴中心內的應用程式設定，為您的廣告選擇目標物件，但您可以選擇性地定義您自己的物件。 您也可以使用一組預設的廣告範本或上傳自己的廣告設計。 如需廣告行銷活動的詳細資訊，請參閱[廣告行銷活動的常見問題](common-questions.md)。

您可以只針對已通過[應用程式認證程序](the-app-certification-process.md)最後發佈階段的應用程式建立廣告行銷活動。

> [!NOTE]
> 檔集的這一節說明如何在合作夥伴中心建立廣告行銷活動。 以程式設計方式建立和管理 ad 活動的其他活動選項包括[Vungle](https://vungle.com/)和[Microsoft Store 促銷 API](../monetize/run-ad-campaigns-using-windows-store-services.md)。

## <a name="instructions"></a>指示

以下說明如何建立廣告活動來促銷 App。

1.  從 [[合作夥伴中心](https://partner.microsoft.com/dashboard)] 的左側導覽功能表中，展開 [**吸引**]，然後選取 [**廣告活動**]。
2.  選取 **\[建立廣告活動\]** (或者如果您已經建立廣告活動，請選取 **\[新廣告活動\]** )。
3.  在下一頁的 **\[目標類型\]** 區段中，選擇其中一項︰
    * **增加 app 安裝次數**。 如果您的廣告活動是要吸引人安裝您的 app，請選取此選項。
    * **增加與 app 的互動**。 如果您的廣告活動是要吸引客戶增加 App 使用量，請選取此選項。 當您選取此選項時，您可以將廣告行銷活動的目標設定為您所定義的特定[客戶區隔](create-customer-segments.md)。

4.  選取您想要透過這個廣告活動宣傳的 app。 請注意，應用程式必須已經在市集中。
5.  檢閱 **\[廣告活動名稱\]** 欄位中提供的廣告活動名稱，以，並視需要進行變更。
6.  在 **\[廣告活動類型\]** 底下，選擇下列其中一個選項：
    * **付費廣告**：這些廣告會在任何符合您應用程式的裝置與類別中執行。 對於 2017 年 1 月 9 日後建立的新行銷活動，這些廣告也會出現在 MSN.com、Outlook.com、Skype 和其他 Microsoft 進階內容中。 將應用程式與 Microsoft 進階內容做為目標的應用程式促銷活動稱為 *「通用」* 行銷活動。
    * **社群廣告 (免費)** ︰這些廣告會在其他開發人員發行的應用程式中執行，而這些開發人員也會建立社群廣告行銷活動。 您必須先在 **\[獲利\]**  ->  **\[應用程式內廣告\]** 頁面中選擇在您的應用程式中顯示社群廣告，才能選取此選項。 如需詳細資訊，請參閱[關於社群廣告](about-community-ads.md)。
    * **自家廣告 (免費)** ：這些廣告只會在您的應用程式 (符合廣告應用程式的裝置類型) 中執行。 自家廣告完全免費。 如需詳細資訊，請參閱[關於自家廣告](about-house-ads.md)。

7.  對於付費廣告活動，請確認 **\[廣告活動持續期間\]** （將支出廣告活動預算的期間）。 預設選項是 **\[每月\]** ，這表示每個月重複支出您的廣告活動預算，直到停止廣告活動為止。 如果您有 Premium 帳戶，您也可以選擇 **\[自訂\]** 來指定支出廣告活動預算的自訂日期和時間範圍。 如需有關 Premium 帳戶的詳細資訊，請參閱[廣告行銷活動的常見問題](common-questions.md#how-can-i-increase-the-maximum-monthly-budget-amount-allowed-for-my-ad-campaign)。

8.  確認您的預算和付款資訊 (如果您正在建立自家行銷活動或社群行銷活動，就不會顯示這些選項，因為這些行銷活動是免費的)。
    * 在 **\[預算\]** 底下，使用滑桿設定您想要每個月花費在執行此廣告的金額（或總預算，如果您已選取自訂廣告活動持續期間）。

        每月預算是依建立廣告活動的月份按比例分配。 換句話說，如果您在行事曆月份的中途建立廣告活動，您將需要支付該月份每月預算的一半。

    * 按一下 **\[新增付款方式\]** 並填寫您的帳戶詳細資料，以指定廣告行銷活動的付款方式。 如果您已經提供付費方式，需要進行更新時可以選取 **\[選擇其他付款方式\]** 。 付款方法的帳單位址的國家/地區必須符合與您的開發人員帳戶相關聯的國家/地區。

    * 如果您收到 Microsoft 代表提供的優惠券並用來支付廣告活動費用，請按一下 **[Use a coupon]** (使用優待券)、輸入優待券代碼，然後按一下 **[套用]** 以將優待券套用至廣告活動。

    當您完成時，請按一下 **\[儲存並前往下一步\]** ，繼續進行 **\[對象\]** 步驟。 此步驟不適用於房屋廣告活動，因為它們只會在您自己的應用程式中執行。

9.  在 **\[對象\]** 頁面上，我們將會顯示建議用於您行銷活動的對象設定。 您可以選擇性地調整這項資訊：
    * **國家/地區**：最多選擇您想要顯示廣告的 5 個國家或地區。 如需支援的國家或地區清單，請參閱[廣告行銷活動的常見問題](common-questions.md#where-will-my-ad-appear)。

    * **裝置**：選擇您想要顯示這些廣告的裝置類型。 只有您應用程式支援的裝置類型才會顯示。

    * **Surface**：選擇 **\[通用\]** ，讓您的廣告出現在應用程式中，以及 MSN.com、Outlook.com、Skype 和其他 Microsoft 進階內容中。 選擇 **\[App\]** ，如果您只想讓您的廣告出現在應用程式中。

    * **作業系統**：選擇應該會顯示您廣告的作業系統。 只有您 app 支援的作業系統才會顯示。

    * **性別**：選擇是否要依性別限制廣告對象。

    * **年齡範圍**：選取期望對象的年齡範圍。

    此區段也會顯示 **\[預估觸及\]** 圖表。 此圖表使用您目前的客群選取，顯示您可以預期觸及的對象，以所選市場中所有啟用廣告之 Windows app 使用者的百分比顯示。

10.  如果您選擇 **\[增加與 App 的互動\]** 做為您的行銷活動目標，您可以選取其中一個客戶區隔做為目標。 只有此區隔中包含的客戶，才能看到使用此行銷活動建立的廣告。 每一廣告活動只能選取一個區隔。 如需區隔的相關資訊，請參閱[建立客戶區隔](create-customer-segments.md)。 當您完成時，請按一下 **\[儲存並前往下一步\]** ，繼續進行 **\[廣告設計\]** 步驟。 此步驟不適用於房屋廣告活動，因為它們只會在您自己的應用程式中執行。

11.  在 **\[廣告設計\]** 頁面，選擇下列其中一個選項：
    * **自動產生**。 這是預設選項，可讓您從我們的預設範本建立廣告。 您可以進行選擇以自訂您的廣告內容，然後我們會根據您的選項進行廣告預覽（當您進行選取時更新自動）。
        * 在 **\[語言\]** 下拉式清單中選取廣告的語言。 Microsoft Store 徽章的文字會以您選取的語言顯示。
        * 若要新增額外的標語文字到廣告中，在 **\[自訂標語\]** 欄位中輸入文字。
            > [!NOTE]
            > 您在此輸入的文字必須當地語系化成選取的語言。 如果文字未符合 [Bing 廣告原則](https://advertise.bingads.microsoft.com/bing-ads-policies)，將會拒絕自訂標語。 如需樣式與禁止內容的指導方針，請參閱此頁面。
        * 若要進一步自訂廣告，請展開 **\[自訂廣告設計/看見所有廣告大小\]** 並選擇下列任一項︰
            * **背景色彩**。 從可用的選項中選擇。
            * **影像**。 選擇其中一個可用影像（取自您的應用程式市集清單）。
            * **顯示我的應用程式評分**。 如果您想要顯示應用程式的評分，請選取此核取方塊。
            * **顯示我的應用程式是免費的**。 如果您的應用程式在所有選取的市集中是免費的，您會有選取此核取方塊的選項。
            * **喚起行動**。 如果您選擇 **\[增加與 App 的互動\]** 做為您的行銷活動目標，您可以將您行銷活動的 [喚起行動] 按鈕設定為 **\[開啟\]** 、 **\[播放\]** 、 **\[朗讀\]** 、 **\[聆聽\]** 或 **\[購買\]** 。  

    * **自訂**。 選擇此選項以使用您自己的廣告設計。 請注意，如果您稍早選取了客戶區段，則必須使用自訂 creatives。 您可以針對每個可用的廣告尺寸上傳不同的檔案。 檔案必須符合下列需求與指導方針：
        * 每個檔案都必須是小於或等於 40 KB 的 .png 或 .jpg 檔案。
        * 您的廣告設計必須符合 [Microsoft 創意接受原則](https://about.ads.microsoft.com/solutions/ad-products/display-advertising/creative-acceptance-policies)中指定的需求。
        * 您廣告設計中的內容必須與宣傳的應用程式相關。 與應用程式無關的廣告設計將不會發佈給其他應用程式中的廣告。
        * 您廣告設計中的所有內容都應該清晰易懂。 例如，內容不應該模糊不清、像素不足或延展。

12.  如果您有 [Premium 帳戶](common-questions.md#how-can-i-increase-the-maximum-monthly-budget-amount-allowed-for-my-ad-campaign)，您可以使用 **\[連結網址\]** 方塊來控制客戶按一下您的廣告時會發生的事。
    * 如果方塊保留空白，當客戶按一下您的廣告時，會顯示您的應用程式市集清單。
    * 如果您使用 [調整]、[Kochava]、[微調] 或 [Vungle] 來測量應用程式的安裝分析，請輸入您的安裝追蹤 URL。 當您儲存活動時，系統會驗證該追蹤 URL 以確保它會解析至您應用程式在Microsoft Store 中的清單頁面。 如需有關使用這些服務進行安裝追蹤的詳細資訊，請參閱[調整](https://docs.adjust.com/en/)、 [Kochava](https://support.kochava.com/)、[微調](https://help.tune.com/hasoffers/)和[Vungle](https://support.vungle.com/hc/en-us)檔。
    * 如果您選擇 **\[增加與 App 的互動\]** 做為您的行銷活動目標，您可以指定[深層連結 URI](../launch-resume/handle-uri-activation.md)，將選取區隔中的客戶重新導向到您應用程式內的指定頁面。
    * 如果您指定了任何不是您應用程式說明頁面或應用程式內頁面的目的地，您的行銷活動將會自動暫停。

13.  最後，按一下 **\[檢閱\]** 確認您的廣告活動設定與預算和付款資訊 (如果它是付費廣告活動)。 按一下 **\[確認\]** ，您的廣告通常會在幾個小時內開始出現在裝置上。

## <a name="review-ad-campaign-performance"></a>檢閱廣告行銷活動成效

若要了解行銷活動執行情況，請返回 **\[廣告活動\]** 頁面。 選取 **\[區段篩選器\]** ，依 **\[日期\]** 、 **\[行銷活動目標\]** 、 **\[應用程式名稱\]** 、 **\[行銷活動類型\]** 或 **\[狀態\]** 來設定報告中內容的範圍。 除了查看您行銷活動的 **\[曝光數\]** 、 **\[點擊次數\]** 、 **\[轉換\]** 與 **\[花費\]** 等資訊之外，您可以使用報告來 **\[暫停\]** 或 **\[繼續\]** 行銷活動。 如需詳細資訊，請參閱[廣告活動報告](promote-your-app-report.md)。

若要編輯行銷活動，請選取清單中的名稱。
