---
author: PatrickFarley
title: 已連線的應用程式與裝置 (專案 Rome)
description: 本節說明如何使用遠端系統平台來探索遠端裝置、啟動遠端裝置上的 app，以及與遠端裝置上的 app 服務通訊。
ms.author: pafarley
ms.date: 06/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，連接裝置、 遠端系統、 rome 的 project rome
ms.assetid: 7f39d080-1fff-478c-8c51-526472c1326a
ms.localizationpriority: medium
ms.openlocfilehash: d3efb7e094ce1464028dadaa14c6f0bfb3f3b214
ms.sourcegitcommit: 7efffcc715a4be26f0cf7f7e249653d8c356319b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/30/2018
ms.locfileid: "3127515"
---
# <a name="connected-apps-and-devices-project-rome"></a>已連線的應用程式與裝置 (專案 Rome)

本節說明如何使用專案 Rome 跨裝置與平台與 App 連線。 了解如何探索遠端裝置、啟動遠端裝置上的 App，以及與遠端裝置上的 App 服務通訊。

大部分的使用者擁有多部裝置，而且經常是在一部裝置上開始活動，卻在另一部上完成。 若要做到這一點，App 需要跨越裝置與平台。

Windows 10 (版本 1607) 中引進的[遠端系統 API](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)，可讓您撰寫 App，允許使用者在一部裝置上啟動工作，並在另一部上完成。 工作仍是中心焦點，而且使用者能在最方便使用的裝置上執行工作。 例如，使用者可能會在車內聆聽手機上的收音機，但當到家時，可能會想要換到和家庭立體聲系統連結的 Xbox One 上播放。

您也可以將專案 Rome 用於隨附裝置或遠端控制案例。 使用 App 服務訊息傳送 API，在兩部裝置之間建立 App 通道，來傳送和接收自訂訊息。 例如，您可以為手機撰寫可控制電視播放的 App，或撰寫配對 App 來為您透過另一個 App 上觀看的電視節目角色提供相關資訊。  

近端可以透過藍牙和無線來與裝置連線，或使用裝置使用者的 Microsoft 帳戶 (MSA) 透過雲端從遠端連結。

請參閱[遠端系統 UWP 範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems )，以取得如何探索遠端系統、啟動遠端系統上的 App，以及使用 App 服務在兩個系統上執行的 App 之間傳送訊息的範例。

如需一般有關專案 Rome 的詳細資訊，包括跨平台整合的資源，請移至 [aka.ms/project-rome](https://aka.ms/project-rome)。

| 主題 | 說明 |
|-------|-------------|
| [啟動遠端裝置上的應用程式](launch-a-remote-app.md) | 了解如何啟動遠端裝置上的 App。 本主題涵蓋最簡單的使用案例和初步安裝。  |
| [探索遠端裝置](discover-remote-devices.md)  | 了解如何探索能夠連線的裝置。 |
| [與遠端應用程式服務通訊](communicate-with-a-remote-app-service.md) | 了解如何與遠端裝置上的 app 互動。 |
| [透過遠端工作階段連接裝置](remote-sessions.md) | 將裝置加入遠端工作階段，以建立跨多部裝置的共用體驗。 |
| [繼續使用者活動，甚至是在各個裝置之間](useractivities.md)| 協助使用者繼續使用什麼程式執行作業您的應用程式，甚至是在多個裝置之間。|
| [使用者活動的最佳做法](useractivities-best-practices.md)| 了解建立和更新使用者活動的建議的做法。|