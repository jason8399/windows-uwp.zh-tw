---
Description: 介紹與 Windows 應用程式相關的協助工具概念。
ms.assetid: C89D79C2-B830-493D-B020-F3FF8EB5FFDD
title: 協助工具
label: Accessibility
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0e410094f738860e71dadb960fccbdbc59306050
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234145"
---
# <a name="accessibility"></a>協助工具  

協助工具是關於建立體驗，讓在各種環境中使用技術的人員可以利用您的 Windows 應用程式，並以各種需求和體驗來處理您的 UI。 根據法律的規定，某些情況必須提供協助工具。 不過無論法律的規定為何，最好能解決無障礙問題，這樣應用程式便能吸引更多的使用者。

> 您的應用程式的協助工具也有 Microsoft Store 的聲明！

| 文章 | 說明 |
|---------|-------------|
| [協助工具概觀](accessibility-overview.md) | 本文概述與 Windows 應用程式的協助工具案例相關的概念和技術。 |
| [設計通用軟體](designing-inclusive-software.md) | 瞭解適用于 Windows 10 之 Windows 應用程式的進化式內含性設計。  在設計和建置通用軟體時考量提供無障礙功能。 |
| [開發全人 Windows 應用程式](developing-inclusive-windows-apps.md) | 本文是開發可存取的 Windows 應用程式的藍圖。 |
| [協助工具測試](accessibility-testing.md) | 要遵循的測試程式，以確保您的 Windows 應用程式可供存取。 |
| [市集中的協助工具](accessibility-in-the-store.md) | 說明在 Microsoft Store 中將 Windows 應用程式宣告為可存取的需求。 |
| [協助工具檢查清單](accessibility-checklist.md) | 提供檢查清單以協助您確保 Windows 應用程式可供存取。 |
| [公開基本的協助工具資訊](basic-accessibility-information.md) | 基本協助工具資訊通常分類為名稱、角色以及值。 本主題描述的程式碼可協助您的 app 公開輔助技術所需的基本資訊。 |
| [鍵盤協助工具](keyboard-accessibility.md) | 如果 app 未能提供適切的鍵盤功能操作，盲眼或行動不便的使用者將難以使用 app，或者根本無法使用。 |
| [螢幕讀取器和硬體系統按鈕](system-button-narration.md) | 螢幕讀者（例如 [[朗讀](https://support.microsoft.com/en-us/help/22798/windows-10-complete-guide-to-narrator)程式]）必須能夠辨識並處理硬體系統按鈕事件，並將其狀態傳達給使用者。 在某些情況下，螢幕閱讀程式可能需要以獨佔方式處理按鈕事件，而不是讓它們向上反升至其他處理常式。 |
| [地標和標題](landmarks-and-headings.md) | 地標和標題定義可協助輔助技術 (例如螢幕助讀程式) 使用者有效率進行瀏覽的使用者介面區段。 |
| [高對比佈景主題](high-contrast-themes.md) | 說明當高對比主題為作用中時，確保您的 Windows 應用程式可供使用的必要步驟。 |
| [協助工具文字的需求](accessible-text-requirements.md) | 本主題說明 app 中文字的協助工具最佳做法，方法是確保色彩和背景能夠滿足必要的對比率。 本主題也會討論 Windows 應用程式中的文字元素可以擁有的 Microsoft UI 自動化角色，以及圖形中文字的最佳作法。 |
| [協助工具應避免的做法](practices-to-avoid.md) | 如果您想要建立可存取的 Windows 應用程式，請列出要避免的作法。 |
| [自訂自動化對等](custom-automation-peers.md) | 說明使用者介面自動化的自動化對等觀念，以及如何提供自訂 UI 類別的自動化支援。 |
| [控制項模式和介面](control-patterns-and-interfaces.md) | 本文列出 Microsoft 使用者介面自動化控制項模式、用戶端用來存取控制項模式的類別，以及介面提供者用來實作控制項模式的類別。 |

## <a name="related-topics"></a>相關主題  
* [**Windows. UI. Xaml. Automation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation) 
* [開始使用朗讀程式](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator)
