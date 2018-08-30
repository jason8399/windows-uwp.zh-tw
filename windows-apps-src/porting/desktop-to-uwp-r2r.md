---
author: rido-min
Description: This guide explains how to configure your Visual Studio Solution to optimize the application binaries with native images.
Search.Product: eADQiWindows 10XVcnh
title: 最佳化您的.NET 傳統型應用程式使用原生映像
ms.author: normesta
ms.date: 06/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10 中，編譯器的原生映像
ms.localizationpriority: medium
ms.openlocfilehash: d98b576fb51a8f9507802796ab359d0d00d21998
ms.sourcegitcommit: 7efffcc715a4be26f0cf7f7e249653d8c356319b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/30/2018
ms.locfileid: "3124927"
---
# <a name="optimize-your-net-desktop-apps-with-native-images"></a>最佳化您的.NET 傳統型應用程式使用原生映像

> [!NOTE]
> 正式發行前可能會進行大幅度修改之發行前版本產品的一些相關資訊。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。

您可以預先編譯您的二進位檔，以改善您的.NET Framework 應用程式的啟動時間。 您可以使用這項技術上您套件，並透過在 Windows 市集中發佈的大型應用程式。 在某些情況下，我們已經觀察到 20%的效能改進。 您可以深入了解[技術概觀](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/readytorun-overview.md)中這項技術。

我們已發行的預覽版本的原生映像編譯器為[NuGet 套件](https://www.nuget.org/packages/Microsoft.DotNet.Framework.NativeImageCompiler)。 您可以將此套件套用至任何.NET Framework 應用程式以.NET Framework 版本 4.6.2 為目標，或更新版本。 此套件新增 post 建置步驟，其中包含為您的應用程式所使用的所有二進位檔的原生承載。 當應用程式在.NET 4.7.2 和更新版本時執行舊版仍然會載入的 MSIL 程式碼時，將會載入這個最佳化的承載。

[.NET framework 4.7.2](https://blogs.msdn.microsoft.com/dotnet/2018/04/30/announcing-the-net-framework-4-7-2/)包含在[Windows 10 2018 年更新](https://blogs.windows.com/windowsexperience/2018/04/30/how-to-get-the-windows-10-april-2018-update/)。 您也可以在電腦上的執行 Windows 7 + 與 Windows Server 2008 R2 + 安裝此版本的.NET Framework。

> [!IMPORTANT]
> 如果您想要產生您的應用程式封裝的 Windows 應用程式封裝專案的原生映像，請務必將專案的目標平台最小版本設定為 Windows 年度更新版。

## <a name="how-to-produce-native-images"></a>如何產生原生映像

請依照下列指示來設定您的專案。

1. 設定目標架構為 4.6.2 或以上

2. 將目標平台設定為 x86 或 x64 

3. 新增 NuGet 套件。

4. 建立發行的組建。

## <a name="configure-the-target-framework-as-462-or-above"></a>設定目標架構為 4.6.2 或以上

若要設定您的專案目標.NET Framework 4.6.2 您將需要.NET Framework 4.6.2 開發工具或更新版本。 這些工具都是透過 Visual Studio 安裝程式在.NET 傳統型開發工作負載的選用元件為：

![安裝.NET 4.6.2 開發工具](images/desktop-to-uwp/install-4.6.2-devpack.png)

或者，您可以取得從.NET 開發人員套件：[https://www.microsoft.com/net/download/visual-studio-sdks](https://www.microsoft.com/net/download/visual-studio-sdks)

## <a name="configure-the-target-platform-as-x86-or-x64"></a>將目標平台設定為 x86 或 x64

原生映像編譯器會最佳化特定平台的程式碼。 若要使用它，您需要設定您的應用程式，例如 x86 或 x64 一個特定的平台為目標。

如果您有多個專案，在您的方案中，只有進入點專案 （最有可能會產生可執行檔檔案的專案） 必須進行編譯為 x86 或 x64。 即使在編譯為 AnyCPU 額外的二進位檔從主要專案參考來處理使用中主要專案中，指定之架構。

若要設定您的專案：

1. 您的解決方案，以滑鼠右鍵按一下，然後選取 [**組態管理員**。

2. 選取 **< 新.>** 在**平台**下拉式功能表中的專案，會產生可執行檔名稱旁邊。

3. 在 [**新專案平台**] 對話方塊中，請確定**複製設定，從**下拉式清單，設為 [**任何 CPU**。

![設定 x86](images/desktop-to-uwp/configure-x86.png)

重複執行此步驟的`Release/x64`如果您想要產生 x64 二進位檔案。

>[!IMPORTANT]
> 原生映像編譯器不支援 AnyCPU 組態。

## <a name="add-the-nuget-packages"></a>新增 NuGet 套件

原生映像編譯器會以的 NuGet 套件，您需要新增到 Visual Studio 專案產生的可執行檔提供。 這通常是您的 Windows Forms 或 WPF 專案。 使用此 PowerShell 命令執行此作業。

```PS
PM> Install-Package Microsoft.DotNet.Framework.NativeImageCompiler -Version 0.0.1-prerelease-00002  -PRE
```

> [!NOTE]
> 於 NuGet.org 中發行預覽封裝為未列出。 瀏覽 NuGet.org 或封裝管理員 UI 使用 Visual Studio 中，您將不會尋找他們。 不過，您可以從套件管理器主控台中，安裝它們，當您從不同的電腦還原。 我們要將套件完全存取時，我們都會發佈的第一個非預覽版本。

## <a name="create-a-release-build"></a>建立發行的組建

NuGet 套件設定專案以執行額外的工具的發行組建。 此工具將原生程式碼加入至相同的二進位檔。
若要驗證工具已處理的二進位檔，您可以檢閱組建輸出，以確定它包含例如此訊息：

```
Native image obj\x86\Release\\R2R\DesktopApp1.exe generated successfully.
```

## <a name="faq"></a>常見問題

**問:。 新的二進位檔是否作用於.NET Framework 4.7.2 機器？**

A. 最佳化的二進位檔而受益的改進功能與.NET Framework 4.7.2 執行時。 執行舊版的.NET framework 的用戶端會從之二進位檔載入非最佳化 MSIL 程式碼。

**問:。 如何提供意見反應或報告問題？**

A. 在 Visual Studio 2017 中使用的意見反應工具回報有關的問題。 [的詳細資訊](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017)。

**問:。 什麼是將原生映像新增到現有的二進位檔的影響？**

A. 最佳化的二進位檔包含 managed 和原生程式碼，讓更大的最終的檔案。

**問:。 我是否可以在釋出使用這項技術的二進位檔？**

A. 這個版本包含移 Live 授權，您可以立即使用。