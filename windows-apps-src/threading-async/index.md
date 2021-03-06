---
ms.assetid: beac6333-655a-4bcf-9caf-bba15f715ea5
title: 執行緒和非同步程式設計
description: 執行緒和非同步程式設計可讓您的 app 在平行執行緒中以非同步方式完成工作。
ms.date: 05/14/2018
ms.topic: article
keywords: windows 10, uwp, asynchronous, threads, threading, 非同步, 執行緒
ms.localizationpriority: medium
ms.openlocfilehash: 22c151b90be30b39da7decd9a0ce3109e29b7fb7
ms.sourcegitcommit: f727b68e86a86c94eff00f67ed79a1c12666e7bc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "63813158"
---
# <a name="threading-and-async-programming"></a>執行緒和非同步程式設計
執行緒和非同步程式設計可讓您的 app 在平行執行緒中以非同步方式完成工作。

您的應用程式可以使用執行緒集區，以非同步方式完成平行執行緒中的工作。 執行緒集區管理一組執行緒，並且使用佇列將工作項目指派給可用的執行緒。 執行緒集區與 Windows 執行階段中可用來完成延伸工作但不會封鎖 UI 的非同步程式設計模式類似，不過執行緒集區提供的控制遠甚於非同步程式設計模式，您可以使用執行緒集區以並行方式完成多個工作項目。 您可以使用執行緒集區來處理以下工作：

-   提交工作項目、控制工作項目的優先順序，以及取消工作項目。

-   使用計時器和定期計時器排程工作項目。

-   預留重要工作項目所需的資源。

-   執行工作項目以回應具名事件和系統信號。

執行緒集區可以降低建立及摧毀執行緒的額外負荷，因此在管理執行緒更有效率。 這表示它可以存取多個 CPU 核心以最佳化執行緒，還可以在應用程式之間和在背景工作執行時平衡執行緒資源。 使用內建的執行緒集區可方便您專心撰寫完成工作的程式碼，而不是執行緒管理機制。

| 主題                                                                                                          | 說明                         |
|----------------------------------------------------------------------------------------------------------------|-------------------------------------|
| [非同步程式設計 (UWP 應用程式)](asynchronous-programming-universal-windows-platform-apps.md)              | 這個主題描述通用 Windows 平台 (UWP) 的非同步程式設計，以及它在 C#、Microsoft Visual Basic .NET、Visual C++ 元件延伸 (C++/CX) 以及 JavaScript 中的表示方式。 |
| [C++/CX 的非同步程式設計 (UWP 應用程式)](asynchronous-programming-in-cpp-universal-windows-platform-apps.md)| 本文描述透過使用在 ppltasks.h 內 <code>concurrency</code> 命名空間中定義的 <code>task</code> 類別，於 C++/CX 中運用非同步方法的建議方式。 |
| [使用執行緒集區的最佳做法](best-practices-for-using-the-thread-pool.md)                         | 本主題描述使用執行緒集區的最佳做法。 |
| [在 C# 或 Visual Basic 中呼叫非同步 API](call-asynchronous-apis-in-csharp-or-visual-basic.md)             | 通用 Windows 平台 (UWP) 包含許多非同步 API，可以確保即使 app 執行需要花一段時間才能完成的工作，還是能保持回應。 本主題討論如何在 C# 或 Microsoft Visual Basic 使用 UWP 的非同步方法。 |
| [建立定期工作項目](create-a-periodic-work-item.md)                                                   | 了解如何建立定期重複執行的工作項目。 |
| [將工作項目提交至執行緒集區](submit-a-work-item-to-the-thread-pool.md)                               | 了解如何透過將工作項目提交至執行緒集區，以使用個別的執行緒來執行工作。 |
| [使用計時器提交工作項目](use-a-timer-to-submit-a-work-item.md)                                       | 了解如何建立在計時器過後執行的工作項目。 |
