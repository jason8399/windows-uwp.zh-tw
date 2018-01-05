---
author: stevewhims
Description: "本節顯示如何製作、封裝及使用您的 App 字串、影像和檔案資源。"
title: "App 資源和資源管理系統"
label: Intro
template: detail.hbs
ms.author: stwhi
ms.date: 10/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 資源, 影像, 資產, MRT, 限定詞"
localizationpriority: medium
ms.openlocfilehash: 38a131704bacbffdf89636aa70b405aa30861d27
ms.sourcegitcommit: d0c93d734639bd31f264424ae5b6fead903a951d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/03/2017
---
# <a name="app-resources-and-the-resource-management-system"></a>App 資源和資源管理系統
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

本節顯示如何製作、封裝及使用您的 App 字串、影像和檔案資源。 例如，您可能會封裝檔案以及包含遊戲層級定義的休閒遊戲，並且載入執行階段的檔案。 我們也會向您顯示單獨維護您的 App 邏輯的資源，如何可讓您輕鬆針對不同地區設定、裝置顯示、協助工具設定以及其他使用者和電腦內容，當地語系化及自訂您的 App。 諸如字串和影像的資源通常需要存在於多個語言、縮放比例和對比變體中。 對於這類資源，您有[資源管理系統](resource-management-system.md)的支援。

App 資源有兩種類型。
- 檔案資源是在磁碟上儲存為檔案的資源。 檔案資源可以包含點陣圖影像、XAML、XML、HTML 或任何其他類型的資料。
- 內嵌的資源是內嵌在一些包含資源檔案的資源。 最常見的範例是內嵌於資源檔案中的字串資源 (.resw 或 .resjson)。

如需有關將您的 App 當地語系化的價值主張的詳細資訊，請參閱[全球化和當地語系化](../globalizing/globalizing-portal.md)。

| 文章 | 說明 |
|---------|-------------|
| [資源管理系統](resource-management-system.md) | 建置期間，資源管理系統會建立所有不同變體 (使用您的 App 封裝) 的資源的索引。 在執行階段，系統會偵測生效的使用者和電腦設定，並載入這些設定的最佳相符項的資源。 |
| [資源管理系統如何比對和選擇資源](how-rms-matches-and-chooses-resources.md) | 當要求資源時，也許有幾個某程度符合目前資源內容的候選項目。 資源管理系統將會分析所有的候選項目，並判斷要傳回的最佳候選項目。 本主題將詳細說明該程序，並且提供範例。 |
| [資源管理系統如何比對語言標記](how-rms-matches-lang-tags.md) | 上一個主題 ([資源管理系統如何比對和選擇資源](how-rms-matches-and-chooses-resources.md)) 主要綜觀限定詞比對。 本主題著重於語言標記比對的細節。 |
| [針對語言、縮放比例、高對比及其他限定詞量身打造您的資源](tailor-resources-lang-scale-contrast.md) | 本主題說明資源限定詞的一般概念、其使用方式，以及每個限定詞名稱的用途。 |
| [將 UI 及應用程式套件資訊清單中的字串當地語系化](localize-strings-ui-manifest.md) | 如果希望 App 支援不同的顯示語言，而且您的程式碼、XAML 標記或應用程式套件資訊清單也含有字串常值時，請將這些字串移入資源檔案 (.resw)。 您可以接著針對 App 支援的每一種語言建立該資源檔案的翻譯複本。 |
| [載入針對縮放比例、佈景主題、高對比及其他設定量身打造的影像和資產](images-tailored-for-scale-theme-contrast.md) | 您的 App 可以載入包含針對顯示縮放比例、佈景主題、高對比及其他執行階段內容量身打造之影像的影像資源檔案。 |
| [對語言、縮放比例及高對比的磚和快顯通知支援](tile-toast-language-scale-contrast.md) | 您的磚和快顯通知可以載入針對顯示語言、顯示縮放比例、高對比及其他執行階段內容量身打造的字串與影像。 |
| [URI 配置](uri-schemes.md) | 有幾個可供您用來參考您的應用程式套件、您的 App 資料的資料夾或雲端之檔案的 URI (統一資源識別項) 配置。 您也可以使用 URI 配置參考從您的 App 的檔案資源 (.resw) 載入的字串。 |
| [手動以 MakePri.exe 編譯資源](compile-resources-manually-with-makepri.md) | MakePri.exe 是一個命令列工具，您可以用來建立和傾印 PRI 檔案。 其做為 MSBuild 的一部分整合到 Microsoft Visual Studio 中，但用於手動建立套件或自訂組建系統也很有用。 |
| [MakePri.exe 命令列選項](makepri-exe-command-options.md) | MakePri.exe 有一組命令 `createconfig`，`dump`，`new`、`resourcepack` 和 `versioned`。 本主題詳述命令列選項的使用。 |
| [MakePri.exe 設定檔](makepri-exe-configuration.md) | 本主題說明 MakePri.exe XML 設定檔的結構描述。 |
| [MakePri.exe 格式特定的索引子](makepri-exe-format-specific-indexers.md) | 本主題說明 MakePri.exe 用來產生資源索引的格式特定索引子。 |
| [在舊版應用程式或遊戲中使用 Windows 10 資源管理系統](using-mrt-for-converted-desktop-apps-and-games.md) | 將您的 .NET 或 Win32 應用程式或遊戲封裝成 AppX 套件，即可利用「資源管理系統」載入為執行階段內容量身打造的 App 資源。 這個深入主題說明技術。 |

另請參閱，原本針對 Windows 8.x 建立的文件，這些文件仍然適用於通用 Windows 平台 (UWP) app 和 Windows 10。

-   [App 資源與當地語系化](https://msdn.microsoft.com/library/windows/apps/xaml/hh710212.aspx)