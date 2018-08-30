---
author: TerryWarwick
title: 服務點硬體的支援
description: 本文包含各個服務點裝置類別的硬體支援的相關資訊
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ecb2468497115c9595f6fd17ab61b30caed507ab
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832092"
---
# <a name="supported-point-of-service-peripherals"></a>支援的服務點週邊設備

## <a name="barcode-scanner"></a>條碼掃描器
| 連線能力 | 支援 |
| -------------|-------------|
| USB          | <p>Windows 包含連接 USB 之條碼掃描器的預設類別驅動程式，而此預設類別驅動程式以 [USB.org](http://www.usb.org/developers/hidpage/)所定義的 HID POS 掃描器使用表格 (8c) 規格為基礎。請查看下列表格，以了解已知相容裝置的清單。  請參閱條碼掃描器的手冊，或連絡製造商以決定如何在 **\[USB.HID.POS 掃描器\]** 模式下進行設定。 </p><p>Windows 也支援實作供應商特定驅動程式，以支援不支援 USB.HID.POS 掃描器標準的其他條碼掃描器。 請與條碼掃描器製造商連絡，以了解供應商特定驅動程式可用性。</p><p>條碼掃描器製造商如需有關建立自訂條碼掃描器驅動程式的資訊，請參閱[條碼掃描器驅動程式設計指南](https://aka.ms/pointofservice-drv)</p> |
| 藍牙    | <p>Windows 支援序列埠通訊協定 - 簡單序列介面 (SPP SSI) 藍牙條碼掃描器。 請查看下列表格，以了解已知相容裝置的清單。 請參閱條碼掃描器的手冊，或連絡製造商以決定如何在 **\[SPP-SSI\]** 模式下進行設定。</p> |
| 網路攝影機       | <p>從 Windows 10 版本 1803 開始，您可以從通用 Windows 應用程式透過標準攝影機鏡頭讀取條碼。 建議您使用支援自動對焦的相機，並且最低解析度為 1920 x 1440。  如果條碼列印夠大的話，部分解析度較低的相機可讀取標準條碼。  元素較細的條碼可能需要解析度較高的相機。</p>| 
|


### <a name="compatible-barcode-scanners"></a>相容的條碼掃描器
| 分類 | 連線能力 | 製造商/機型 |
|--------------|-----------|-----------|
| **1D 手持掃描器** | **USB** |Honeywell Voyager 1200g<br/>Honeywell Voyager 1202g<br/>Honeywell Voyager 1202-bf<br/>Honeywell Voyager 145Xg (可升級)|
| **1D 手持掃描器** | **藍牙** |通訊端行動裝置 CHS 7Ci<br/> 通訊端行動裝置 CHS 7Di<br/> 通訊端行動裝置 CHS 7Mi<br/> 通訊端行動裝置 CHS 7Pi<br/>通訊行動裝置 DuraScan D700<br/> 通訊行動裝置 DuraScan D730<br/>通訊端行動裝置 SocketScan S800 (先前稱為 CHS 8Ci) <br/>|
|**2D 手持掃描器** | **USB** |Code Reader™ 950<br/>Code Reader™ 1021<br/>Code Reader™ 1421<br/> Honeywell Granit 198Xi<br/>Honeywell Granit 191Xi<br/>Honeywell Xenon 1900g<br/>Honeywell Xenon 1902g<br/>Honeywell Xenon 1902g-bf<br/>Honeywell Xenon 1900h<br/>Honeywell Xenon 1902h<br/>Honeywell Voyager 145Xg (可升級)<br/>Honeywell Voyager 1602g<br/>Intermec SG20<br/>Zebra DS2278<br/>Zebra DS8108 ¹<hr><small>¹ 最小值韌體需要 016 (2018.01.18)。 可使用 [123Scan](http://www.zebra.com/123Scan) 升級</small>|
|**2D 手持掃描器** | **藍牙** |通訊端行動裝置 SocketScan S850 (先前稱為 CHS 8Qi)|
| **簡報掃描器** | **USB** |Code Reader™ 5000<br/>Honeywell Genesis 7580g<br/>Honeywell Orbit 7190g|
| **平台式掃描器** | **USB** |Honeywell Stratos 2700|
| **掃描引擎** | **USB** | Honeywell N5680<br/>Honeywell N3680|
| **Windows 行動裝置版裝置**| **內建** |Bluebird EF400<br/>Bluebird EF500<br/>Bluebird EF500R<br/>Honeywell CT50<br/>Honeywell D75e<br/>Janam XT2<br/>Panasonic FZ-E1<br/>Panasonic FZ-F1<br/>PointMobile PM80<br/>Zebra TC700j|
| **Windows 行動裝置版裝置**| **自訂** | HP Elite X3 (含 Barcode Scanner Jacket) |


## <a name="cash-drawer"></a>收銀機
| 連網 | 支援 |
| -------------|-------------|
| 網路/藍牙 | <p> 視收銀機裝置的功能而定，可以透過網路或藍牙直接與收銀機進行連線。 </p><p>APG 收銀機：NetPRO、BluePRO</p> |
| DK 連接埠 | <p> 沒有網路或藍牙功能的收銀機也可以透過受支援收據印表機的 DK 連接埠或 Star Micronics DK-AirCash 配件進行連線。 </p>
| OPOS    | <p> 透過製造商提供的 OPOS 服務物件支援任何 OPOS 相容收銀機。 根據裝置製造商的安裝指示，安裝 OPOS 驅動程式。 </p> |


## <a name="customer-display-linedisplay"></a>客戶螢幕 (LineDisplay)
透過製造商提供的 OPOS 服務物件支援任何 OPOS 相容行顯示器。 根據裝置製造商的安裝指示，安裝 OPOS 驅動程式。

## <a name="magnetic-stripe-reader"></a>磁條讀取器
Windows 根據廠商識別碼和產品識別碼 (VID/PID)，為 Magtek 和 IDTech 的下列磁條讀取器提供支援。

| 製造商 |    型號 |  零件編號 |
|--------------|-----------|--------------|
| IDTech | SecureMag (VID:0ACD PID:2010) | IDRE-3x5xxxx |
| | MiniMag (VID:0ACD PID:0500) |   IDMB-3x5xxxx |
| Magtek | MagneSafe (VID:0801 PID:0011) |  210730xx |
| | Dynamag (VID:0801 PID:0002) |   210401xx |

 Windows 支援其他廠商特定驅動程式的實作，以便支援其他磁條讀取器。 請向磁條讀取器製造商洽詢可用性。 磁條讀取器製造商如需有關建立自訂磁條讀取器驅動程式的資訊，請參閱[磁條讀取器驅動程式設計指南](https://aka.ms/pointofservice-drv)

## <a name="receipt-printer-posprinter"></a>收據印表機 (POSPrinter)
| 連線能力 | 支援 |
| -------------|-------------|
| 網路和藍牙 | <p>Windows 支援使用 Epson ESC/POS 印表機控制語言的網路及藍牙連線收據印表機。  使用 POSPrinter API 可自動對下列印表機進行探索。 提供 ESC/POS 模擬的附加收據印表機也適用，但必須使用[頻外配對](https://aka.ms/pointofservice-oobpairing)程序來產生關聯。</p><p>注意：票印站及存根記錄站無法透過這種方式來支援。</p> |
| OPOS    | <p> 透過 OPOS 服務物件支援任何 OPOS 相容收據印表機。 根據裝置製造商的安裝指示，安裝 OPOS 驅動程式。 </p> |

### <a name="stationary-receipt-printers-networkbluetooth"></a>固定式收據印表機 (網路/藍牙)
| 製造商 |    型號 |
|--------------|-----------|
| Epson |   TM-T88V、TM-T70、TM-T20、TM-U220 |

### <a name="mobile-receipt-printers-bluetooth"></a>行動收據印表機 (藍牙)
| 製造商 |    型號 |
|--------------|-----------|
| Epson |   Mobilink P20 (TM-P20)、Mobilink P60 (TM-P60)、Mobilink P80 (TM-P80) |