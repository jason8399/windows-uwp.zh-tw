---
author: jnHs
Description: Learn how your app's packages are made available to your customers, and how to manage specific package scenarios.
title: 應用程式套件管理指導方針
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.author: wdg-dev-content
ms.date: 03/28/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9b0b6315b1177138c3ede7834e2dbc792ee106dd
ms.sourcegitcommit: 7efffcc715a4be26f0cf7f7e249653d8c356319b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/30/2018
ms.locfileid: "3118307"
---
# <a name="guidance-for-app-package-management"></a>應用程式套件管理指導方針

了解您的應用程式套件如何提供給您的客戶，以及如何管理特定套件案例。

-   [作業系統版本和套件發佈](#os-versions-and-package-distribution)
-   [將適用於 Windows 10 的套件新增至先前發佈的 app](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [維護 Windows Phone 8.1 的套件相容性](#maintaining-package-compatibility-for-windows-phone-81)
-   [從市集移除 App](#removing-an-app-from-the-store)
-   [移除先前支援之裝置系列的套件](#removing-packages-for-a-previously-supported-device-family)


## <a name="os-versions-and-package-distribution"></a>作業系統版本和套件發佈

不同的作業系統上可以執行不同類型的套件。 如果有一個以上的套件可以在客戶的裝置上執行，則 Microsoft Store 將提供最佳絕配。

一般而言，較新的作業系統版本可以針對相同的裝置系列，執行以之前作業系統版本為目標的套件。 不過，客戶只會取得那些套件，如果 app 不包含以其目前的作業系統版本為目標的套件。

例如，Windows 10 裝置可以執行所有先前支援的作業系統版本 (每個裝置系列)。 Windows 10 桌面裝置可以執行針對 Windows 8.1 或 Windows 8 建置的 app；Windows 10 行動裝置可以執行針對 Windows Phone 8.1、Windows Phone 8 和 Windows Phone 7.x 所建置的 app。 

下列範例針對包含以不同作業系統版本為目標之套件的應用程式，說明各種的案例 (除非您套件的特定限制不允許它們在此處所列的每一個作業系統版本/裝置類型上執行，例如，套件的架構必須適用於裝置)。 

### <a name="example-app-1"></a>範例 app 1

| 套件的目標作業系統 | 將取得此套件的作業系統 |
|-------------------------------------|----------------------------------------------|
| Windows 8.1                         | Windows 10 桌上型電腦裝置、Windows 8.1      |
| Windows Phone 8.1                   | Windows 10 行動裝置、Windows Phone 8.1 |
| Windows Phone 8                     | Windows Phone 8                              |
| Windows Phone 7.1                   | Windows Phone 7.x                            |

在 app 範例 1，app 還沒有特別針對 Windows 10 裝置建置的通用 Windows 平台 (UWP) 套件，但 Windows 10 的客戶仍可取得 app。 這些客戶會根據其裝置類型取得最佳的套件。

### <a name="example-app-2"></a>範例 app 2

| 套件的目標作業系統  | 將取得此套件的作業系統 |
|--------------------------------------|----------------------------------------------|
| Windows 10 (通用裝置系列) | Windows 10 (所有裝置系列)             |
| Windows 8.1                          | Windows 8.1                                  |
| Windows Phone 8.1                    | Windows Phone 8.1                            |
| Windows Phone 7.1                    | Windows Phone 7.x、Windows Phone 8           |

在範例 app 2 中，沒有可在 Windows 8 上執行的套件。 在所有其他作業系統版本上執行的客戶可以取得應用程式。 在 Windows 10 上的所有客戶將取得相同的套件。

### <a name="example-app-3"></a>範例 app 3

| 套件的目標作業系統 | 將取得此套件的作業系統                  |
|-------------------------------------|---------------------------------------------------------------|
| Windows 10 (桌面裝置系列)  | Windows 10 桌面裝置                                    |
| Windows Phone 8                     | Windows 10 行動裝置、Windows Phone 8、Windows Phone 8.1 |

在範例 app 3 中，因為沒有以行動裝置系列為目標的 UWP 套件，所以 Windows 10 行動裝置的客戶將會取得 Windows Phone 8 套件。 如果此應用程式後來新增以行動裝置系列 (或通用裝置系列) 為目標的套件，則該套用即可供 Windows 10 行動裝置的客戶，而非 Windows Phone 8 套件的客戶使用。

也請注意，這個範例 app 不包含任何可在 Windows Phone 7.x 上執行的套件。

### <a name="example-app-4"></a>範例 app 4

| 套件的目標作業系統  | 將取得此套件的作業系統 |
|--------------------------------------|----------------------------------------------|
| Windows 10 (通用裝置系列) | Windows 10 (所有裝置系列)             |

在範例 app 4 中，任何執行 Windows 10 的裝置均可取得 app，但舊版作業系統的客戶無法使用該 app。 因為 UWP 套件以通用裝置系列為目標，它可供任何 Windows 10 的裝置 （每個您的[裝置系列可用性選取項目](device-family-availability.md)）。


## <a name="removing-an-app-from-the-store"></a>從 Microsoft Store 移除 App

有時，您可能想要停止為客戶提供某個 app，有效地「取消發佈」該 app。 若要執行此動作，請按一下 [**應用程式概觀**] 頁面上的 [**停止提供應用程式**]。 在您確認想要停止提供該應用程式之後，該應用程式在數小時內便無法在 Microsoft Store 中讓別人看見，而所有的新客戶都將無法取得它 (除非他們有[促銷碼](generate-promotional-codes.md)且使用 Windows 10 裝置)。

> [!IMPORTANT]
> 此選項將會覆寫您已在提交中選取的所有[可見度](choose-visibility-options.md#discoverability)設定。 

這個選項具有以下選項的相同效果：如果您建立提交並選擇 **\[在 Microsoft Store 推出此產品，但不供搜尋\]** 與 **\[停止取得\]** 選項。 不過，它不需要您建立新的提交。

請注意，任何已擁有此 app 的客戶仍可使用它並可再次下載 (甚至可以在您於稍後提交新的套件時取得更新)。

停止提供該 app 之後，您仍能在儀表板中看見它。 如果您決定再次為客戶提供該 app，就可以按一下 [App 概觀] 頁面上的 **\[提供 App\]**。 當您確認之後，該 app 即可在數小時內提供給新的客戶使用 (除非受到您在最新提交中的設定所限制)。

> [!NOTE]
> 如果您想保持應用程式的可用性，但不想繼續提供給使用特定作業系統版本的新客戶，您可以建立新的提交，並針對您想要在其上防止新取得的作業系統版本移除所有套件。 例如，如果您先前已有適用於 Windows Phone 8.1 及 Windows 10 的套件，而您不想持續提供該應用程式給 Windows Phone 8.1 上的客戶，則可從提交中移除所有 Windows Phone 8.1 套件。 發佈更新之後，就不會有任何 Windows Phone 8.1 上的新客戶能夠擷取該應用程式 (但已經擁有該應用程式的客戶仍能繼續使用)。 不過，應用程式仍然可供 Windows 10 上的新客戶使用。


## <a name="removing-packages-for-a-previously-supported-device-family"></a>移除先前支援之裝置系列的套件

如果您要移除之特定[裝置系列](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview)，您的 app 先前支援，系統將提示您確認這是您意圖，您可以在 [**套件**\] 頁面上儲存變更之前的所有套件。

當您發佈移除所有套件，可在您的 app 先前支援的裝置系列上執行的提交時，新客戶將無法取得該裝置系列上的 app。 此後您還是可以發佈其他更新，以再次針對該裝置系列提供套件。

請注意，即使您移除支援特定裝置系列的所有套件，任何已安裝該 app 的現有客戶仍能夠使用它，且他們會取得您之後所提供的任何更新。


<a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>

## <a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>將適用於 Windows 10 的套件新增至先前發佈的 app

如果您在市集中有一個以 Windows 8.x 和/或 Windows Phone 8.x 為目標的 App，但是想要更新該 App 使之適用於 Windows 10 時，請在進行 [[套件](upload-app-packages.md)] 步驟時建立新的提交並新增 UWP .appxupload 套件。 您的應用程式通過認證程序後，已經擁有您的 app，而且現在位於 Windows 10 的客戶將會取得您的 UWP 套件更新從 microsoft Store。 Windows 10 上的客戶也可以透過全新取得的方式獲取 UWP 套件。

> [!NOTE]
> 一旦 Windows 10 客戶取得您的 UWP 套件後，您無法讓該客戶回復到使用任何之前作業系統版本的套件。 

請注意，您的 Windows 10 套件版本號碼必須高於您包含的 Windows 8、Windows 8.1 和/或 Windows Phone 8.1 套件 (或是適用於您先前發佈之作業系統版本的套件) 的版本號碼。 如需詳細資訊，請參閱[套件版本編號](package-version-numbering.md)。

如需針對 Microsoft Store 封裝 UWP 應用程式的詳細資訊，請參閱[封裝應用程式](../packaging/index.md)。

> [!IMPORTANT]
> 請記住，如果您提供以通用裝置系列為目標的套件，那麼已經在任何舊版作業系統 (Windows Phone 8、Windows 8.1 等) 中擁有您的應用程式而後升級至 Windows 10 的客戶，將會進行更新以取得 Windows 10 套件。
> 
> 發生這種情況即使您已排除特定裝置系列的[裝置系列可用性](device-family-availability.md)步驟中的 [您的提交，因為一節，僅適用於全新取得的情況。 如果您不想每一位先前客戶取得您的通用 Windows 10 套件，請務必更新您的 appx 資訊清單中的 [**TargetDeviceFamily**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) 元素，只包含您想要支援的特定裝置系列。
> 
> 例如，假設您想要讓您 Windows 8 和 Windows 8.1 的客戶已升級至 Windows 10 desktop 裝置，取得您新的 UWP 應用程式，但您想要的任何 Windows Phone 客戶現在將您的套件先前的 Windows 10 行動裝置版裝置可 available （目標為 Windows Phone 8 或 Windows Phone 8.1）。 若要這樣做，您將需要更新[**TargetDeviceFamily**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily)您 appx 資訊清單中包含僅**Windows.Desktop** （適用於傳統型裝置系列），而不是將它做為**Windows.Universal**值 （適用於通用裝置系列）該 Microsoft Visual Studio 預設包含在資訊清單中。 請勿提交任何針對通用或行動裝置系列的 UWP 套件 (**Windows.Universal** 或 **Windows.Universal**)。 如此一來，您的 Windows 10 行動裝置版客戶將不會取得任何 UWP 套件。


## <a name="maintaining-package-compatibility-for-windows-phone-81"></a>維護 Windows Phone 8.1 的套件相容性

在更新先前針對 Windows Phone 8.1 發佈的 app 時，會套用某些套件類型的特定要求：

-   在 app 擁有已發佈的 Windows Phone 8.1 套件後，所有後續的更新也必須包含 Windows Phone 8.1 套件。
-   在 app 擁有已發佈的 Windows Phone 8.1 XAP 後，後續的更新必須具有 Windows Phone 8.1 XAP、Windows Phone 8.1 appx 或 Windows Phone 8.1 appxbundle。
-   當 app 擁有已發佈的 Windows Phone 8.1 .appx 時，後續的更新必須具有 Windows Phone 8.1 .appx 或 Windows Phone 8.1 .appxbundle。 換句話說，不允許 Windows Phone 8.1 XAP。 這也適用於包含 Windows Phone 8.1.appx 的 .appxupload。
-   在 app 擁有已發佈的 Windows Phone 8.1 .appxbundle 後，後續的更新必須具有 Windows Phone 8.1 .appxbundle。 換句話說，不允許 Windows Phone 8.1 XAP 或 Windows Phone 8.1 .appx。 這也適用於包含 Windows Phone 8.1 .appxbundle 的 .appxupload。

未遵循這些規則可能會導致套件上傳錯誤，使您無法完成提交。