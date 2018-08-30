---
author: WilliamsJason
title: 裝置入口網站資料夾上傳 API 參考
description: 了解如何以程式設計方式存取資料夾上傳 API。
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: e1a2c7f0-0040-4ce7-94de-17224736e20b
ms.openlocfilehash: d071a0ff6d228608d7f6c7acdcfd88df38f2c390
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.locfileid: "210559"
---
# <a name="upload-a-folder-to-the-development-directory"></a>將資料夾上傳到開發目錄

**要求**

您可以將整個資料夾同時上傳到 DevelopmentFiles 的已知資料夾識別碼 (或是該資料夾內的的子資料夾)。

方法      | 要求 URI
:------     | :------
POST | /api/app/packagemanager/upload 
<br />
**URI 參數**

您可以在要求 URI 上指定下列其他參數：

URI 參數      | 說明
:------     | :-----
destinationFolder (必要) | 上傳資料夾的目的地資料夾名稱。 這個資料夾會放置在主機上的 d:\developmentfiles\LooseApps 底下。 這個資料夾名稱必須是 base64 編碼，因為它可能包含路徑分隔符號 (如果該資料夾是 LooseApps 下的子資料夾)。
<br />

**要求標頭**

- 無

**要求主體**

- 目錄內容的多部分合格 http 本文。

**回應**

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 成功
4XX | 錯誤碼
5XX | 錯誤碼
<br />
**可用裝置系列**

* Windows Xbox
