---
author: mtoepke
title: 圖形診斷工具
description: 了解如何取得與使用圖形診斷功能，包括 Visual Studio 中的圖形偵錯、圖形畫面格分析，以及 GPU 使用量。
ms.assetid: 629ea462-18ed-a333-07e9-cc87ea2dcd93
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 遊戲, 圖形, 診斷工具, directx
ms.openlocfilehash: 076020d88889a9cc8b417befa2dd54b41d688e5e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.locfileid: "209208"
---
# <a name="graphics-diagnostics-tools"></a>圖形診斷工具


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

隨著 Windows 10 的推出，圖形診斷工具目前已在 Windows 內做為選用功能來提供。 若要使用執行階段與 Visual Studio 中所提供的圖形診斷功能來開發 DirectX App 或遊戲，請安裝「圖形工具」選用功能：

1.  移至 **\[設定\]**，依序選取 **\[系統\]**、**\[應用程式與功能\]**，然後按一下 **\[管理選用功能\]**。
2.  按一下 **\[新增功能\]**   
3.  在 **\[選用功能\]** 清單中，選取 **\[圖形工具\]**，然後按一下 **\[安裝\]**。

圖形診斷功能包括在 DirectX 執行階段中建立 Direct3D 偵錯裝置 (透過 Direct3D SDK 層) 的能力，再加上圖形偵錯、圖形畫面格分析與 GPU 使用量。

-   圖形偵錯可讓您追蹤您應用程式所進行的 Direct3D 呼叫。 然後您就能重新執行這些呼叫、檢查參數、偵錯與實驗著色器，並且將圖形資產視覺化，以診斷轉譯問題。 記錄可在 Windows 電腦、模擬器或裝置上執行，並在不同的硬體上播放。
-   Visual Studio 中的圖形畫面格分析會在圖形偵錯記錄上執行，並收集 Direct3D 繪製呼叫的基準計時。 然後它會透過修改各種圖形設定，來執行一連串的實驗，並產生計時結果表格。 您可以使用此資料了解您應用程式中的圖形效能問題並檢閱各項實驗的結果，以找出可能改善效能的環節。
-   Visual Studio 中的 GPU 使用量可讓您即時監視 GPU 使用狀況。 它會收集並分析由 CPU 與 GPU 所處理的工作負載的計時資料， 讓您可以判斷瓶頸發生位置。

## <a name="related-topics"></a>相關主題


[Visual Studio 中的圖形診斷概觀](http://go.microsoft.com/fwlink/p/?LinkID=526382)

 

 



