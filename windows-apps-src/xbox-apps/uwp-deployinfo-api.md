---
title: 裝置入口網站部署資訊 API 參考
description: 瞭解如何使用 Xbox 裝置入口網站 REST API deployinfo 來要求一或多個已安裝套件的部署資訊。
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 5260125625ced6c258a683bcfb9b552e57d07f06
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88942998"
---
# <a name="requests-deployment-information-for-one-or-more-installed-packages"></a>對於一或多個安裝的套件要求部署資訊。

**要求**

方法      | 要求 URI
:------     | :------
POST | /ext/app/deployinfo
<br />
**URI 參數**

 - 無

**要求標頭**

- 無

**要求本文**

下列格式的 JSON 陣列：

* DeployInfo
  * PackageFullName - 我們正在要求相關資訊的套件名稱。
  * OverlayFolder - 如果使用這項功能選用的路徑重疊資料夾路徑。

###<a name="response"></a>回應

**回應主體**

下列格式的 JSON 陣列 (部分欄位是選用的)：

* DeployInfo
  * PackageFullName - 我們正在接收相關資訊的套件名稱。
  * DeployType - 部署的類型。
  * DeployPathOrSpecifiers - 鬆散部署的部署路徑，或封裝部署的已安裝規範。
  * DeployDrive - 針對適用的部署類型，部署套件到磁碟機。
  * DeploySizeInBytes - 針對適用的部署類型，套件的大小 (以位元組為單位)。
  * OverlayFolder - 支援此功能之部署的重疊資料夾。

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | Success
4XX | 錯誤碼
5XX | 錯誤碼
<br />

**可用裝置系列**

* Windows Xbox