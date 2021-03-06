---
Description: MakePri.exe 是一個命令列工具，您可以用來建立和傾印 PRI 檔案。 其做為 MSBuild 的一部分整合到 Microsoft Visual Studio 中，但用於手動建立套件或自訂組建系統也很有用。
title: 使用 MakePri.exe 來手動編譯資源
template: detail.hbs
ms.date: 10/23/2017
ms.topic: article
keywords: Windows 10, uwp, 資源, 影像, 資產, MRT, 限定詞
ms.localizationpriority: medium
ms.openlocfilehash: c23eced05d7539c869a40db1f2b641282b75f503
ms.sourcegitcommit: 963316e065cf36c17b6360c3f89fba93a1a94827
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/06/2020
ms.locfileid: "82868896"
---
# <a name="compile-resources-manually-with-makepriexe"></a>使用 MakePri.exe 來手動編譯資源

MakePri.exe 是一個命令列工具，您可以用來建立和傾印 PRI 檔案。 其做為 MSBuild 的一部分整合到 Microsoft Visual Studio 中，但用於手動建立套件或自訂組建系統也很有用。

> [!NOTE]
> 當您在安裝 Windows 軟體發展工具組時，檢查**UWP 受管理應用程式的 Windows SDK**選項時，會安裝 MakePri。 它會安裝到路徑`%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` （以及針對其他架構命名的資料夾中）。 例如： `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe` 。

## <a name="in-this-section"></a>本節內容
|主題|描述|
|-|-|
| [MakePri.exe 命令列選項](makepri-exe-command-options.md) | MakePri.exe 有一組命令 `createconfig`，`dump`，`new`、`resourcepack` 和 `versioned`。 本主題詳述命令列選項的使用。 |
| [MakePri.exe 設定檔](makepri-exe-configuration.md) | 本主題說明 MakePri.exe XML 設定檔的結構描述。 |
| [MakePri.exe 格式特定的索引子](makepri-exe-format-specific-indexers.md) | 本主題說明 MakePri.exe 用來產生資源索引的格式特定索引子。 |

## <a name="makepriexe-command-line-options"></a>MakePri.exe 命令列選項

MakePri.exe 有一組命令 `createconfig`，`dump`，`new`、`resourcepack` 和 `versioned`。 如需其使用的詳細資訊，請參閱 [MakePri.exe 命令列選項](makepri-exe-command-options.md)。

## <a name="makepriexe-configuration"></a>MakePri.exe 設定

PRI XML 設定檔會指出如何編製什麼資源的索引。 設定 XML 的結構描述在 [MakePri.exe 設定](makepri-exe-configuration.md)中描述。

## <a name="format-specific-indexers"></a>格式特定的索引子

MakePri.exe 通常搭配 `new`、`versioned` 和 `resourcepack` 選項使用。 在這些案例中，它會編制來源檔案的索引來產生資源的索引。 MakePri.exe 使用各種個別索引子來讀取不同來源的資源檔案或資源的容器。 最簡單的索引子是資料夾索引子，它會為資源如 `.jpg` 或 `.png` 影像編制資料夾內容的索引。 如需詳細資訊，請參閱 [MakePri.exe 格式特定的索引子](makepri-exe-format-specific-indexers.md)。

## <a name="makepriexe-warnings-and-error-messages"></a>MakePri.exe 警告和錯誤訊息

### <a name="resources-found-for-languages-languages-but-no-resources-found-for-default-languages-languages-change-the-default-language-or-qualify-resources-with-the-default-language"></a>找到語言「<語言」 > ' 的資源，但找不到預設語言的資源：「<語言 >」。 變更預設語言，或以預設語言限定資源。

當 MakePri 或 MSBuild 探索到標示了語言辨識符號之指定名稱資源的檔案或字串資源，但未找到預設語言的候選項時，就會顯示此警告。 在檔案和資料夾名稱中使用限定詞的程序會在[針對語言、縮放比例及其他限定詞量身打造您的資源](tailor-resources-lang-scale-contrast.md)中說明。 檔案或資料夾名稱中可能有語言名稱，但是找不到確切符合預設語言的任何資源。 例如，如果專案使用「en-US」做為預設語言，並且有一個名為「de/logo.png」的檔案，但是沒有任何檔案標示「en-US」的預設語言，就會出現此警告。 為移除此警告，檔案或字串資源應符合預設語言，或者應該變更預設語言。 若要變更預設語言，在 Visual Studio 中保持您的解決方案為開啟狀態下，開啟 `Package.appxmanifest`。 在 \[應用程式\] 索引標籤上，確認預設語言有適當地設定 (例如，「en」或「en-US」)。

### <a name="no-default-or-neutral-resource-given-for-resource-identifier-the-application-may-throw-an-exception-for-certain-user-configurations-when-retrieving-the-resources"></a>沒有為 '<resource identifier>' 提供預設或中性的資源。 在抓取資源時，應用程式可能會擲回特定使用者設定的例外狀況。

當 MakePri 或 MSBuild 發現的檔案或資源標示為不清楚資源的語言限定詞時，就會顯示此警告。 雖然有限定詞，但是並不保證特定資源候選項目可以因為該資源識別碼在執行階段而傳回。 如果找不到特定語言的資源候選項目、HomeRegion 或其他限定詞為預設值，或者始終與使用者的內容相符，將會顯示此警告。 在執行時間，針對特定使用者設定（例如使用者的語言喜好設定或首頁位置（**設定** > **時間 & 語言** > **區域 & 語言**），用來捕獲資源的 api 可能會擲回未預期的例外狀況。 為了移除此警告，應該提供預設資源，例如專案的預設語言或全球首頁區域 (homeregion-001) 中的資源。

## <a name="using-makepriexe-in-a-build-system"></a>在組建系統中使用 MakePri.exe

組建系統應該根據要建置專案的類型使用 MakePri.exe `new`、`versioned` 或 `resourcepack` 命令。 建立新的 PRI 檔案的組建系統應該使用 `new` 命令。 必須透過反覆運算確保內部位移之相容性的組建系統可以使用 `versioned` 命令。 必須建立包含資源額外變體的 PRI 檔案的組建系統，經過驗證以確保未加入任何新的資源到該變體時，應該使用 `resourcepack` 命令。

需要明確控制已編製索引之來源檔案的組建系統，可以使用 ResFiles 索引子而不是編製資料夾的索引。 組建系統也可以使用多個具有不同[格式特定索引子](makepri-exe-format-specific-indexers.md)的索引證來產生單一 PRI 檔案。

組建系統也可以使用 PRI 格式特定的索引子，從其他元件例如類別庫、組件、SDK 和 DLL，將預先建置的 PRI 檔案新增到 PRI。

當為其他元件、類別庫、組件、DLL 和 SDK 建置 PRI 檔案時，**initialPath** 設定應用來確保元件資源有自己的子資源地圖，不會與包含該地圖的 App 相衝突。

## <a name="related-topics"></a>相關主題
* [MakePri.exe 命令列選項](makepri-exe-command-options.md)
* [MakePri.exe 設定](makepri-exe-configuration.md)
* [MakePri.exe 格式特定的索引子](makepri-exe-format-specific-indexers.md)
* [針對語言、縮放比例及其他限定詞量身打造您的資源](tailor-resources-lang-scale-contrast.md)
