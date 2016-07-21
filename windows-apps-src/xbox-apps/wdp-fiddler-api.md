---
author: WilliamsJason
title: "Device Portal Fiddler API 參考"
description: "了解如何以程式設計方式啟用/停用 Fiddler 追蹤。"
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: eeb3bc5c4843fe86c54930315d4e112166664e45
ms.openlocfilehash: 435a00eaf9c1f0d8e0c0043229c2adc80638ace3

---

# Fiddler 設定 API 參考   
您可以使用這個 REST API，啟用和停用開發套件的 Fiddler 網路追蹤。

## 啟用 Fiddler 追蹤

**要求**

您可以使用下列要求，啟用開發套件的 Fiddler 追蹤。  請注意，裝置必須重新啟動才會生效。

方法      | 要求 URI
:------     | :-----
POST | /ext/fiddler
<br />
**URI 參數**

您可以在要求 URI 上指定下列其他參數：

| URI 參數      | 描述     | 
| ------------------ |-----------------|
| proxyAddress       | 執行 Fiddler 之裝置的 IP 位址或主機名稱 |
| proxyPort          | Fiddler 用來監視流量的連接埠。 預設值為 8888 |
| updateCert (可省略)| 布林值，指出是否有提供根 Fiddler 憑證。 如果從未在此開發套件上設定 Fiddler，或是在曾其他主機上設定 Fiddler，則這個值必須為 true。  |
<br>

**要求標頭**

- 無

**要求主體**

- 若 updateCert 為 false 或未提供，則為無。 否則，多部分合格的 http 本文包含 FiddlerRoot.cer 檔案。

**回應**   

- 無  

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
204 | 已接受啟用 Fiddler 的要求。 Fiddler 將在裝置下次重新啟動時啟用。
4XX | 錯誤碼
5XX | 錯誤碼

## 停用在開發套件上的 Fiddler 追蹤

**要求**

您可以使用下列要求，停用裝置上的 Fiddler 追蹤。 請注意，裝置必須重新啟動才會生效。

方法      | 要求 URI
:------     | :-----
DELETE | /ext/fiddler
<br />
**URI 參數**

- 無

**要求標頭**

- 無

**要求主體**   

- 無

**回應**   

- 無 

**狀態碼**

此 API 具有下列預期狀態碼。

HTTP 狀態碼      | 描述
:------     | :-----
204 | 停用 Fiddler 追蹤的要求成功。 追蹤將在裝置下次重新啟動時停用。
4XX | 錯誤碼
5XX | 錯誤碼

<br />
**可用裝置系列**

* Windows Xbox

## 另請參閱
- [在 Xbox 上針對 UWP 設定 Fiddler](uwp-fiddler.md)




<!--HONumber=Jul16_HO1-->

