---
Description: 查看 Microsoft Store 所指派給您應用程式之唯一身分識別的相關詳細資料，並取得應用程式商店清單的連結。
title: 檢視應用程式身分識別詳細資料
ms.assetid: 86F05A79-EFBC-4705-9A71-3A056323AC65
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 07c2d3308d204d37e246a9a56c0a7203a1340dc0
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210424"
---
# <a name="view-app-identity-details"></a>檢視應用程式身分識別詳細資料


您可以透過 [**應用程式識別**] 頁面上的 Microsoft Store，來查看指派給應用程式之唯一身分識別的相關詳細資料。 您也可以在此頁面上取得應用程式商店清單的連結。

若要尋找這項資訊，請瀏覽到其中一個 app，然後在左方導覽功能表中展開 [應用程式管理]。 選取 **\[應用程式身分識別\]** 可檢視這些詳細資料。


## <a name="values-to-include-in-your-app-package-manifest"></a>app 套件資訊清單中包含的值

您的封裝資訊清單中必須包含下列值。 如果您[使用 Microsoft Visual Studio 建立套件](/windows/msix/package/packaging-uwp-apps)，並且使用與您開發人員帳戶關聯的相同 Microsoft 帳戶進行簽署，這些詳細資料就會自動包含在內。 如果您手動建立套件，您就必須加入下列這些項目：

-   **封裝/身分識別/名稱**
-   **封裝/身分識別/發行者**
-   **封裝/屬性/PublisherDisplayName**

如需詳細資訊，請參閱[套件資訊清單結構描述參考](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)中的 [**Identity**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)。

這些元素共同宣告您的 app 身分識別，建立所有其套件所屬的「套件系列」。 個別套件會有額外的詳細資料，例如架構和版本。


## <a name="additional-values-for-package-family"></a>套件系列的其他值

下列值為參考您 app 套件系列的其他值，但是沒有包含在資訊清單中。

-   **套件系列名稱（PFN）** ：這個值會與特定 Windows Api 搭配使用。
-   **封裝 SID**：您將需要此值，才能將 WNS 通知傳送到您的應用程式。 如需詳細資訊，請參閱 [Windows 推播通知服務 (WNS) 概觀](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)。


## <a name="link-to-your-apps-listing"></a>App 清單連結

您可以分享連至您 app 頁面的直接連結，以協助您的客戶在市集中找到該 app。 這個連結的格式為 **`https://www.microsoft.com/store/apps/<your app's Store ID>`** 。 當客戶按一下此連結時，會開啟您 App 的網頁清單頁面。 在 Windows 裝置上，市集應用程式也將啟動並顯示您的應用程式清單。

您 App 的**市集識別碼**也會在此區段中顯示。 這個市集識別碼可用來[產生市集徽章](https://developer.microsoft.com/store/badges)，或識別您的 App。

**市集通訊協定連結**可以用來直接連接至您在市集中的應用程式，而不開啟瀏覽器，例如當您從應用程式連結時。 如需詳細資訊，請參閱[應用程式的連結](link-to-your-app.md)。



 

 




