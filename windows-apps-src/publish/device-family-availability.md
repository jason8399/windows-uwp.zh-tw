---
Description: 成功上傳套件之後，您會看到一個依排名順序顯示將會提供哪些套件給特定 Windows 10 裝置系列 (以及舊版 OS，如果適用的話) 的表格。
title: 裝置系列可用性
ms.date: 03/21/2019
ms.topic: article
keywords: windows 10, uwp, 套件, 上傳, 裝置系列可用性
ms.localizationpriority: medium
ms.openlocfilehash: 088fb859ae38e608182b22b94300b9c27063054e
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67320023"
---
# <a name="device-family-availability"></a>裝置系列可用性

在 **\[套件\]** 頁面成功上傳套件之後， **\[裝置系列可用性\]** 區段會顯示一個依排名順序顯示將會提供哪些套件給特定 Windows 10 裝置系列 (以及舊版 OS，如果適用的話) 的表格。 本區段也可讓您選擇是否要針對特定 Windows 10 裝置系列的客戶提供提交。

> [!NOTE]
> 如果您尚未上傳套件， **\[裝置系列可用性\]** 區段將會顯示 Windows 10 裝置系列，並有核取方塊讓您表示是否要將提交提供給那些裝置系列上的客戶。 表格會在您上傳一個或多個套件之後出現。

此區段也包含核取方塊，您可以用來表示是否要允許 Microsoft 將 App 提供給任何未來的 Windows 10 裝置系列。 我們建議您保持選取此方塊，讓您的 App 在新裝置系列引入時可供更多潛在客戶使用。


## <a name="choosing-which-device-families-to-support"></a>選擇支援的裝置系列

如果您上傳以一個個別裝置系列為目標的套件，我們會核取此方塊，以便將這些套件提供給使用該裝置類型的新客戶。 例如，如果套件的目標設為 Windows.Desktop，就會核取該套件的 **\[Windows 10 Desktop\]** 方塊 (而且您無法核取其他裝置系列的方塊)。

目標設為 Windows.Universal 裝置系列的套件可以在任何 Windows 10 裝置 (包括 Xbox One) 上執行。 根據預設，我們會在 Xbox *以外*的所有裝置類型上，讓這些套件可供新客戶使用。

如果您不希望將您的提交提供給該裝置類型上的使用者，您可以取消選取任何 Windows 10 裝置系列的方塊。 如果未選取裝置系列的方塊，該裝置類型上的新客戶就無法取得 App (雖然已經有 App 的客戶仍然可以使用，且取得您所提交的任何更新)。

如果您的 App 支援那些裝置，除非有特定原因必須限制可以取得這些 App 的 Windows 10 裝置類型，建議您讓所有的方塊保持選取狀態。 例如，如果已知您的 App 無法在 [Surface Hub](https://developer.microsoft.com/windows/surfacehub) 和/或 [Microsoft HoloLens](https://developer.microsoft.com/mixed-reality) 上提供良好體驗，您可以取消選取 **\[Windows 10 團隊版\]** 和/或 **\[Windows 10 全像攝影版\]** 方塊。 如此即可防止任何新客戶在這些裝置上取得該 App。 如果您後來決定要提供給那些客戶，則可以建立新的提交並核取該方塊。

<span id="xbox" />

Windows.Universal 套件唯一不會預設核取的 Windows 10 裝置系列是 **\[Windows 10 Xbox\]** 。 如果您的 App 不是遊戲 (或者是遊戲，但您已啟用 [Xbox Live 創作者計畫](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators)或完成[概念核准](../gaming/concept-approval.md)程序)，且您的提交包含使用 Windows 10 SDK 版本 14393 或更新版本編譯的中性和/或 x64 UWP 套件，您可以選取 **\[Windows 10 Xbox\]** 方塊來提供 App 給 Xbox One 上的客戶。

> [!IMPORTANT]
> 為了讓您的 App 在 Xbox 裝置上啟動，您必須包含使用 Windows SDK 版本 14393 或更新版本編譯的中性或 x64 套件。 不過，如果您選取 **\[Windows 10 Xbox\]** ，您可用於 Xbox 的最高版本套件 (也就是以 Xbox 或通用裝置系列為目標的中性或 x64 套件) 將一律提供給 Xbox 上的客戶，即使套件是以舊版 SDK 編譯。 基於這個原因，確保適用於 Xbox 的最高版本套件是以 Windows SDK 版本 14393 或更高版本進行編譯非常重要。 如果不是，您會看到錯誤訊息，指出 Xbox 客戶不能啟動您的 App。 
> 
> 若要解決這個錯誤，您可以執行下列其中一項：
> - 以使用 Windows SDK 版本 14393 或更高版本編譯的套件取代適用的套件。
> - 如果您已經有支援 Xbox 且以 Windows SDK 版本 14393 或更高版本編譯的套件，請增加其版本號碼，這樣才能是提交中最高版本的套件。
> - 取消選取 [Windows 10 Xbox]  方塊。
>   
> 如果您仍然無法解決問題，請連絡支援服務。

如果您提交適用於 Windows 10 IoT 核心版的 UWP app，在上傳套件之後您不應變更預設選項。針對 Windows 10 IoT 沒有不同的核取方塊。 如需發佈 IoT 核心版 UWP app 的詳細資訊，請參閱[適用於 IoT 核心版 UWP app 的 Microsoft Store 支援](https://docs.microsoft.com/windows/iot-core/commercialize-your-device/installingandservicing)。

如果您先前已發行的應用程式的提交包含可以在執行的封裝**Windows 8/8.1**並**Windows Phone 8.x 和更早版本**，那些套件將可供在這些作業系統上的客戶版本。 若要停止將您的 App 提供給這些客戶，請從您的提交移除對應的套件。

> [!IMPORTANT]
> 若要完全避免特定的 Windows 10 裝置系列開始您的提交，更新[ **TargetDeviceFamily** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily)您想要為目標裝置系列程式資訊清單中的項目支援 （亦即，Windows.Mobile 或 Windows.Desktop），而不是保留為 Windows.Universal 值 （適用於通用裝置系列中），Microsoft Visual Studio 包含資訊清單中的預設值。

請務必注意您在 **\[裝置系列可用性\]** 區段所做的選擇僅會套用至新擷取。 已經有您的 App 的任何人都能繼續使用並取得您提交的任何更新，即使您在這裡移除其裝置系列也一樣。 甚至會套用至在升級至 Windows 10 之前就取得您的 App 的客戶。 比方說，如果您有已發行的應用程式與 Windows Phone 8.1，您新增為目標的 Windows.Universal 裝置系列的 Windows 10 (UWP) 套件，Windows 10 行動裝置的客戶有您的 Windows Phone 8.1 套件會提供更新給這個 Windows10 (UWP) 套件，即使您已取消核取方塊**Windows 10 行動裝置版**。

如需裝置系列的詳細資訊，請參閱[**裝置系列概觀**](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview)。


## <a name="understanding-ranking"></a>了解排行

除了讓您指出哪些 Windows 10 裝置系列可以下載您的提交申請**裝置系列可用性**區段會顯示將會提供給不同的裝置系列的特定套件。 如果您有多個套件可在特定裝置系列上執行，資料表會根據套件的版本號碼，指出提供套件的順序。 如需有關市集如何根據版本號碼排序套件的詳細資訊，請參閱[套件版本編號](package-version-numbering.md)。 

例如，假設您有兩個套件：Package_A.appxupload 和 Package_B.appxupload。 針對特定的裝置系列，如果 Package_A.Appxupload 排序為 1 而 Package_B.Appxupload 排序為 2，則表示當該裝置類型上的使用者取得您的 App 時，市集會先嘗試傳遞 Package_A.Appxupload。 如果客戶的裝置無法執行 Package_A.appxupload，則市集會提供 Package_B.appxupload。 如果客戶的裝置無法執行的任何裝置家族的封裝 (例如，如果**MinVersion**應用程式支援大於客戶的裝置上的版本) 則客戶將無法下載上的應用程式該裝置。

> [!NOTE]
> 判斷哪一個套件，以提供特定的客戶時，不會考慮.xap 套件 （適用於先前發佈的應用程式） 中的版本號碼。 基於這個原因，如果您有多個同樣順位的 .xap 套件，您會看到星號而非號碼，而客戶可能會收到任一套件。 若要將客戶從一個 .xap 套件更新為較新的套件，請務必在新的提交中移除較舊的 .xap。

