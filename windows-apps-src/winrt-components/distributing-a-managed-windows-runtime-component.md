---
title: 發佈 Managed Windows 執行階段元件
description: 您可以透過檔案複製來發佈自己的 Windows 執行階段元件。
ms.assetid: 80262992-89FC-42FC-8298-5AABF58F8212
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1d306c02d4fd99acaa49ec59230181ac0a20c9f3
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690390"
---
# <a name="distributing-a-managed-windows-runtime-component"></a>發佈 Managed Windows 執行階段元件

您可以透過檔案複製來發佈自己的 Windows 執行階段元件。 不過，如果元件包含許多檔案，使用者就必須等待冗長的安裝過程。 此外，放置檔案時發生的錯誤或參考設定失敗都可能會造成他們的問題。 您可以將複雜元件封裝成 Visual Studio 擴充功能 SDK，方便安裝與使用。 使用者只需為整個封裝設定一個參考。 他們可以使用 [**擴充功能和更新**] 對話方塊輕鬆地尋找並安裝元件，如[尋找和使用 Visual Studio 延伸](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2015)模組中所述。

## <a name="planning-a-distributable-windows-runtime-component"></a>規劃可發佈的 Windows 執行階段元件

為二進位檔案 (例如您的 .winmd 檔案) 選擇唯一的名稱。 建議您遵循下列格式，以確保名稱的唯一性：

``` syntax
company.product.purpose.extension
For example: Microsoft.Cpp.Build.dll
```

您的二進位檔案將安裝於 app 封裝中，可能會與其他開發人員的二進位檔案放在一起。 請參閱 how [to：建立軟體發展工具組](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)中的「延伸模組 sdk」。

請先考慮元件的複雜性，再決定其發佈方式。 符合下列條件時，建議使用擴充功能 SDK 或類似的封裝管理員：

-   您的元件包含多個檔案。
-   您為多個平台 (例如 x86 和 ARM) 提供不同的元件版本。
-   您同時提供元件的偵錯與發行版本。
-   您的元件包含只能在設計階段使用的檔案和組件。

如果符合上述多個條件，擴充功能 SDK 就特別有用。

> **請注意**  針對複雜元件，NuGet 套件管理系統提供延伸模組 sdk 的開放原始碼替代方案。 與擴充功能 SDK 一樣，NuGet 可讓您建立封裝來簡化複雜元件的安裝作業。 如需 NuGet 封裝和 Visual Studio 擴充功能 Sdk 的比較，請參閱[使用 Nuget 與擴充功能 sdk 來新增參考](https://docs.microsoft.com/visualstudio/ide/adding-references-using-nuget-versus-an-extension-sdk?view=vs-2015)。

## <a name="distribution-by-file-copy"></a>藉由檔案複製發佈

如果元件是由單一 .winmd 檔案組成，或是由 .winmd 檔案和資源索引 (.pri) 檔案組成，您只需讓使用者複製 .winmd 檔案即可。 使用者可以將檔案放在專案中的任何地方、使用 [加入現有項目] 對話方塊來將 .winmd 檔案加入專案，然後使用 [參考管理員] 對話方塊來建立參考。 如果您包含 .pri 檔案或 .xml 檔案，請告知使用者將這些檔案與 .winmd 檔案放在一起。

> **請注意**，當您建立 Windows 執行階段元件時，即使您的專案未包含任何資源，Visual Studio 一律會產生一個 pri 檔案  。 如果您有元件的測試應用程式，您可以藉由檢查 bin 中的應用程式套件內容，\\debug\\AppX 資料夾來判斷是否使用 pri 檔案。 如果來自元件的 .pri 檔案並未出現在其中，則您就不需發佈該元件。 或者，您也可以使用 [MakePRI.exe](https://docs.microsoft.com/previous-versions/windows/apps/jj552945(v=win.10)) 工具，傾印來自於 Windows 執行階段元件專案的資源檔。 例如，您可以在 Visual Studio 命令提示字元視窗中輸入： makepri dump /if MyComponent.pri /of MyComponent.pri.xml 如需 .pri 檔案的詳細資訊，請參閱[資源管理系統 (Windows)](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10))。

## <a name="distribution-by-extension-sdk"></a>藉由擴充功能 SDK 發佈

複雜元件通常包括 Windows 資源，但請參閱上一節中關於偵測空白 .pri 檔案的注意事項。

**建立擴充功能 SDK**

1.  確定您已安裝 Visual Studio SDK。 您可以從 [Visual Studio 下載](https://visualstudio.microsoft.com/downloads/download-visual-studio-vs)頁面下載 Visual Studio SDK。
2.  使用 VSIX 專案範本建立新專案。 您可以在 Visual C# 或 Visual Basic 底下的 [擴充性] 分類中找到此範本。 此範本是隨著 Visual Studio SDK 一起安裝。 ([逐步解說：使用 C# 或 Visual Basic 建立 SDK](https://docs.microsoft.com/visualstudio/extensibility/walkthrough-creating-an-sdk-using-csharp-or-visual-basic?view=vs-2015) 或[逐步解說：使用 C++ 建立 SDK](https://docs.microsoft.com/visualstudio/extensibility/walkthrough-creating-an-sdk-using-cpp?view=vs-2015) 會透過非常簡單的案例，來示範此範本的使用方式。 )
3.  判斷 SDK 的資料夾結構。 資料夾結構的開頭位於 VSIX 專案的根層級，並且包含 **References**、**Redist** 及 **DesignTime** 資料夾。

    -   **References** 是二進位檔案的位置，您的使用者可以針對這些檔案進行程式設計。 擴充功能 SDK 會在使用者的 Visual Studio 專案中建立這些檔案的參考。
    -   **Redist** 是必須與二進位檔案一同發佈之其他檔案的位置，位於使用您元件所建立的 app 中。
    -   **DesignTime** 是只有當開發人員在建立會用到您元件的 app 時所使用之檔案的位置。

    您可以在上述每一個資料夾中建立組態資料夾。 可以使用的名稱包含 debug、retail 和 CommonConfiguration。 CommonConfiguration 資料夾是用來存放零售或偵錯組建所使用的相同檔案。 如果您只發佈元件的零售組建，就能將所有檔案放在 CommonConfiguration 中，然後省略另外兩個資料夾。

    在每個組態資料夾中，您可以針對平台特定的檔案提供架構資料夾。 如果您針對所有平台使用相同的檔案，則可提供名為 neutral 的單一資料夾。 您可以在[如何：建立軟體發展工具組](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)中找到資料夾結構的詳細資料，包括其他架構資料夾名稱。 (該文章將討論平台 SDK 和擴充功能 SDK。 您可能發現摺疊關於平台 SDK 的章節，可有效避免混淆。 )

4.  建立 SDK 資訊清單檔案。 資訊清單會指定名稱和版本資訊、SDK 支援的架構、.NET 版本，以及 Visual Studio 使用 SDK 之方式的其他相關資訊。 如需詳細資訊和範例，請參閱[做法：建立軟體開發套件](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)。
5.  建置和發佈擴充功能 SDK。 如需深入資訊，包括當地語系化和簽署 VSIX 封裝，請參閱[Vsix 部署](https://docs.microsoft.com/visualstudio/misc/how-to-manually-package-an-extension-vsix-deployment?view=vs-2015)。

## <a name="related-topics"></a>相關主題

* [建立軟體發展工具組](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)
* [NuGet 套件管理系統](https://github.com/NuGet/Home)
* [資源管理系統（Windows）](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10))
* [尋找和使用 Visual Studio 延伸模組](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2015)
* [MakePRI .exe 命令選項](https://docs.microsoft.com/previous-versions/windows/apps/jj552945(v=win.10))
