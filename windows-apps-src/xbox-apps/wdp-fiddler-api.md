---
title: Device Portal Fiddler API 參考
description: 了解如何以程式設計方式啟用/停用 Fiddler 追蹤。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: e7d4225e-ac2c-41dc-aca7-9b1a95ec590b
ms.localizationpriority: medium
ms.openlocfilehash: 4cbdae1084f96901e90f8237d71bd59bf2d4c592
ms.sourcegitcommit: 681c1e3836d2a51cd3b31d824ece344281932bcd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2019
ms.locfileid: "59240016"
---
# <a name="fiddler-settings-api-reference"></a>Fiddler 設定 API 參考   
您可以使用這個 REST API，啟用和停用開發套件的 Fiddler 網路追蹤。

## <a name="determine-if-fiddler-tracing-is-enabled"></a>判斷 Fiddler 追蹤是否啟用

**要求**

您可以使用下列要求，檢查以查看裝置上是否啟用 Fiddler 追蹤。

方法      | 要求 URI
:------     | :-----
GET | /ext/fiddler


**URI 參數**

- None

**要求標頭**

- None

**要求本文**   

- None

**回應**   

- JSON bool 屬性 IsProxyEnabled，指定 proxy 是否啟用。

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
200 | 成功
4XX | 錯誤碼
5XX | 錯誤碼

## <a name="enable-fiddler-tracing"></a>啟用 Fiddler 追蹤

**要求**

您可以使用下列要求，啟用開發套件的 Fiddler 追蹤。  請注意，裝置必須重新啟動才會生效。

方法      | 要求 URI
:------     | :-----
POST | /ext/fiddler

**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數      | 描述     | 
| ------------------ |-----------------|
| proxyAddress       | 執行 Fiddler 之裝置的 IP 位址或主機名稱 |
| proxyPort          | Fiddler 用來監視流量的連接埠。 預設值為 8888 |
| updateCert (可省略)| 布林值，指出是否有提供根 Fiddler 憑證。 如果從未在此開發套件上設定 Fiddler，或是在曾其他主機上設定 Fiddler，則這個值必須為 true。  |


**要求標頭**

- None

**要求本文**

- 若 updateCert 為 false 或未提供，則為無。 否則，多部分合格的 http 本文包含 FiddlerRoot.cer 檔案。

**回應**   

- None  

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
204 | 已接受啟用 Fiddler 的要求。 Fiddler 將在裝置下次重新啟動時啟用。
4XX | 錯誤碼
5XX | 錯誤碼

## <a name="disable-fiddler-tracing-on-the-devkit"></a>停用在開發套件上的 Fiddler 追蹤

**要求**

您可以使用下列要求，停用裝置上的 Fiddler 追蹤。 請注意，裝置必須重新啟動才會生效。

方法      | 要求 URI
:------     | :-----
DELETE | /ext/fiddler

**URI 參數**

- None

**要求標頭**

- None

**要求本文**   

- None

**回應**   

- None 

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
204 | 停用 Fiddler 追蹤的要求成功。 追蹤將在裝置下次重新啟動時停用。
4XX | 錯誤碼
5XX | 錯誤碼


**可用裝置系列**

* Windows Xbox

## <a name="see-also"></a>另請參閱
- [在 Xbox 上針對 UWP 設定 Fiddler](uwp-fiddler.md)

