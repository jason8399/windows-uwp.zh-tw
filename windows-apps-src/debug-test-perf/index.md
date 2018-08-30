---
author: PatrickFarley
ms.assetid: 16976d00-1564-49fe-81ad-2568e25e9e41
title: 偵錯、測試及效能
description: 使用 Microsoft Visual Studio 偵錯並測試您的 app。 若要準備您的應用程式來進行 Windows 市集認證程序，請使用「Windows 應用程式認證套件」。
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 2d358d96a59128e3d272456a0fcf9f8535407d4f
ms.sourcegitcommit: 73ea31d42a9b352af38b5eb5d3c06504b50f6754
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/27/2017
ms.locfileid: "852793"
---
# <a name="debugging-testing-and-performance"></a>偵錯、測試及效能

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

使用 Microsoft Visual Studio 偵錯並測試您的 app。 若要準備您的應用程式來進行 Windows 市集認證程序，請使用「Windows 應用程式認證套件」。

| 主題 | 描述 |
|-------|-------------|
| [部署和偵錯 UWP app](deploying-and-debugging-uwp-apps.md) | 本文會引導您完成以各種部署和偵錯目標為目標的步驟。 |
| [處理程序生命週期管理 (PLM) 的測試與偵錯工具](testing-debugging-plm.md) | 用來偵錯和測試您 App 如何使用處理程序生命週期管理的工具及技術。 |
| [使用適用於 Windows 10 行動裝置版的 Microsoft 模擬器進行測試](test-with-the-emulator.md) | 使用「適用於 Windows 10 行動裝置版的 Microsoft 模擬器」隨附的工具來模擬與裝置的實際互動，以及測試應用程式的功能。 模擬器是模擬執行 Windows 10 之行動裝置的傳統型 app。 它提供虛擬化環境，讓您可以在其中針對 Windows app 進行偵錯與測試，而不需要擁有實體裝置。 它也提供隔離的環境供您的應用程式原型使用。 |
| [使用 Visual Studio 測試 Surface Hub App](test-surface-hub-apps-using-visual-studio.md) | Visual Studio 模擬器提供了可讓您設計、開發、偵錯和測試通用 Windows 平台 (UWP) app (包括針對 Microsoft Surface Hub 建置的應用程式) 的環境。 此模擬器不會使用與 Surface Hub 相同的使用者介面，但很適合以 Surface Hub 的畫面大小與解析度測試您應用程式的外觀和行為。 |
| [搶鮮版 (Beta) 測試](beta-testing.md) | **搶鮮版 (Beta) 測試**可讓您有機會根據您 App 開發小組以外的個人所提供的意見反應，改進您的 App，這些個人會在他們自己的裝置上試用您尚未發行的 App。 |
| [Windows Device Portal](device-portal.md) | Windows Device Portal 能讓您從遠端透過網路或 USB 連線來設定及管理您的裝置。 |
| [Windows 應用程式認證套件](windows-app-certification-kit.md) | 為了讓您的 App 能順利在 Windows 市集上發行或成為 Windows 認證，請在送出以進行認證之前，先在本機進行驗證和測試。 本主題示範如何安裝和執行 Windows 應用程式認證套件。 |
| [效能](performance-and-xaml-ui.md) | 使用者會期望其 app 保持回應性，並可自在地使用，而不會耗盡電池。 在技術上來說，效能是非功能的需求，但是將效能視為功能可協助您滿足使用者的期望。 指定目標和測量是主要因素。 決定您的效能關鍵案例是什麼；定義良好效能代表什麼意義。 然後在整個專案週期中及早並經常進行測量，以確保您能夠達成目標。 |
| [版本調適型應用程式](version-adaptive-apps.md) | 運用最新 API 及功能，而同時又盡可能深入最廣大的客戶群。 使用執行階段 API 檢查，以在執行階段配合應用程式執行所在 Windows 10 版本可用的功能，自動調整您的程式碼和 XAML。 |