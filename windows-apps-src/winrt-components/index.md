---
title: Windows 執行階段元件
description: Windows 執行階段元件是可讓您具現化並從任何語言 (包括 C#、Visual Basic、JavaScript 和 C++) 使用的獨立物件。
ms.assetid: 55887622-828b-4318-87f2-25592268f7c1
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: db49dfc613b6f7ca2afdb6cc7bdb6b6407751402
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493503"
---
# <a name="windows-runtime-components"></a>Windows 執行階段元件

Windows 執行階段元件是可讓您撰寫、參考並搭配任何 Windows 執行階段語言 (包括 C#、C++/WinRT、Visual Basic、JavaScript 和 C++/CX) 使用的獨立軟體模組。 您可以使用 Visual Studio 來建立可在通用 Windows 平台 (UWP) 應用程式中使用的 Windows 執行階段元件。

> [!NOTE]
> 針對 C++ 開發人員，我們建議您將 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 用於新的應用程式。 C++/WinRT 是完全標準現代的 Windows 執行階段 (WinRT) API 的 C++17 語言投影，僅實作為標頭檔案式程式庫，以及設計用來提供您現代化 Windows API 的第一級存取。 若要了解如何使用 C++/WinRT 建立 Windows 執行階段元件，請參閱[使用 C++/WinRT 的 Windows 執行階段元件](/windows/uwp/winrt-components/create-a-windows-runtime-component-in-cppwinrt)。

| 主題 | 說明 |
|-------|-------------|
| [使用 C++/WinRT 的 Windows 執行階段元件](/windows/uwp/winrt-components/create-a-windows-runtime-component-in-cppwinrt) | 本主題說明如何使用 C++/WinRT 來建立及使用 Windows 執行階段元件，此元件是可使用任何 Windows 執行階段語言所建立通用 Windows 應用程式呼叫的元件。 |
| [Windows 執行階段元件與 C++/CX](creating-windows-runtime-components-in-cpp.md) | 本主題說明如何使用 C++/CX 來建立 Windows 執行階段元件，此元件是可使用任何 Windows 執行階段語言所建立通用 Windows 應用程式呼叫的元件。 |
| [建立 C++/CX Windows 執行階段元件，並從 JavaScript 或 C# 呼叫該元件的逐步解說](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md) | 本逐步解說示範如何建立可從 JavaScript、C# 或 Visual Basic 呼叫的基本 Windows 執行階段元件 DLL。 開始本逐步解說之前，請確定您了解一些概念，例如：抽象二進位介面 (ABI)、ref 類別，以及讓 ref 類別更容易使用的 Visual C++ 元件擴充功能。 如需詳細資訊，請參閱[在 C++ 中建立 Windows 執行階段元件](creating-windows-runtime-components-in-cpp.md)和 [Visual C++ 語言參考 (C++/CX)](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx)。 |
| [使用 C# 和 Visual Basic 建立 Windows 執行階段元件](creating-windows-runtime-components-in-csharp-and-visual-basic.md) | 您可以使用 Managed 程式碼自行建立封裝在 Windows 執行階段元件中的 Windows 執行階段類型。 您可以在通用 Windows 平台 (UWP) app 中，將元件與 C++、JavaScript、Visual Basic 或 C# 搭配使用。 本主題將概述建立元件的規則，並就某些層面討論 .NET 對於 Windows 執行階段的支援。 一般而言，該支援依設計應可讓 .NET 程式設計人員清楚理解。 但是，當您建立要與 JavaScript 或 C++ 搭配使用的元件時，必須了解這些語言對於 Windows 執行階段的支援方式有何差異。 |
| [建立 C# 或 Visual Basic Windows 執行階段元件，並從 JavaScript 呼叫該元件的逐步解說](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) | 本逐步解說示範如何使用 .NET 搭配 Visual Basic 或 C#，建立您自己的 Windows 執行階段類型 (封裝於 Windows 執行階段元件中)，以及如何從使用 JavaScript 為 Windows 建置的通用 Windows 應用程式呼叫此元件。 |
| [在 Windows 執行階段元件中引發事件](raising-events-in-windows-runtime-components.md) | 如果您的 Windows 執行階段元件會在背景執行緒 (背景工作執行緒) 上引發使用者定義的委派類型事件，而您希望 JavaScript 能夠接收該事件，您可以透過下列其中一種方式來實作和 (或) 引發此事件： | 
| [側載 UWP 應用程式的代理 Windows 執行階段元件](brokered-windows-runtime-components-for-side-loaded-windows-store-apps.md) | 本主題討論 Windows 10 更新和更新版本所支援的企業導向功能，該功能允許方便觸控的 .NET 應用程式可使用負責重要業務關鍵作業的現有程式碼。 |
