---
Description: 使用事件檢視器對「Microsoft 進行測驗」的事件和錯誤進行疑難排解。
title: 使用事件檢視器對「Microsoft 進行測驗」進行疑難排解。
ms.assetid: 9218e542-f520-4616-98fc-b113d5a08e0f
ms.date: 10/06/2017
ms.topic: article
keywords: windows 10, uwp, 教育
ms.localizationpriority: medium
ms.openlocfilehash: 2f4bdcf45c7dd37dd540a666d99b5fa2fd2d49f8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598473"
---
# <a name="troubleshoot-microsoft-take-a-test-with-the-event-viewer"></a>使用事件檢視器對「Microsoft 進行測驗」進行疑難排解

您可以使用事件檢視器來見識「進行測驗」的事件和錯誤。 在收到鎖定要求之後、裝置成功註冊、成功套用鎖定原則等情況下，「進行測驗」會記錄事件。

若要啟用以「事件檢視器」檢視事件：
1. 開啟 `Event Viewer`
2. 瀏覽至 `Applications and Services Logs > Microsoft > Windows > Management-SecureAssessment`
3. 以滑鼠右鍵按一下`Operational`，然後選取 `Enable Log`

若要儲存事件記錄檔︰
1. 以滑鼠右鍵按一下 `Operational`
2. 按一下 `Save All Events As…`
