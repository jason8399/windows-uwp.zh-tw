---
ms.assetid: 70667353-152B-4B18-92C1-0178298052D4
title: 用來設定格式的 Epson ESC/POS
description: 了解如何使用 ESC/POS 命令語言，針對您的服務點印表機來格式化文字，例如粗體和雙倍大小字元。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0731f551afaa2420451521b186515255c3724c36
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370140"
---
# <a name="epson-escpos-with-formatting"></a>用來設定格式的 Epson ESC/POS


**重要的 Api**

-   [**PointofService 印表機**](https://docs.microsoft.com/uwp/api/Windows.Devices.PointOfService)
-   [**Windows.Devices.PointOfService**](https://docs.microsoft.com/uwp/api/Windows.Devices.PointOfService)

了解如何使用 ESC/POS 命令語言，針對您的服務點印表機來格式化文字，例如粗體和雙倍大小字元。

## <a name="escpos-usage"></a>ESC/POS 使用方式

Windows 服務點提供使用各種不同印表機的方式，包括數台 Epson TM 系列的印表機 (如需支援印表機的完整清單，請參閱 [PointofService 印表機](https://docs.microsoft.com/uwp/api/Windows.Devices.PointOfService)頁面)。 Windows 支援透過 ESC/POS 印表機控制語言進行列印，可提供有效且正常運作的命令來與您的印表機通訊。

ESC/POS 是 Epson 建立來在範圍廣泛之 POS 印表機系統上使用的命令系統，目標是藉由提供通用的適用性來避免不相容的命令集。 大部分新型印表機均支援 ESC/POS。

所有開頭為 ESC 字元 (ASCII 27、HEX 1B) 或 GS (ASCII 29、HEX 1D) 的命令，後面緊接著另一個用來指定命令的字元。 一般文字都會直接傳送到印表機，並以分行符號來分隔。

[  **Windows PointOfService API**](https://docs.microsoft.com/uwp/api/Windows.Devices.PointOfService) 會透過 **Print()** 或 **PrintLine()** 方法來提供大部分的功能。 不過，若要取得特定的格式設定或傳送指定的命令，您必須使用 ESC/POS 命令，以字串形式建置並傳送到印表機。

## <a name="example-using-bold-and-double-size-characters"></a>使用粗體和雙倍大小字元的範例

下列範例示範如何使用 ESC/POS 命令來列印粗體和雙倍大小的字元。 請注意，每個命令都會以字串形式建置，然後插入 printJob 呼叫。

```csharp
// … prior plumbing code removed for brevity
// this code assumed you've already created a receipt print job (printJob)
// and also that you've already checked the PosPrinter Capabilities to
// verify that the printer supports Bold and DoubleHighDoubleWide print modes

const string ESC = "\u001B";
const string GS = "\u001D";
const string InitializePrinter = ESC + "@";
const string BoldOn = ESC + "E" + "\u0001";
const string BoldOff = ESC + "E" + "\0";
const string DoubleOn = GS + "!" + "\u0011";  // 2x sized text (double-high + double-wide)
const string DoubleOff = GS + "!" + "\0";

printJob.Print(InitializePrinter);
printJob.PrintLine("Here is some normal text.");
printJob.PrintLine(BoldOn + "Here is some bold text." + BoldOff);
printJob.PrintLine(DoubleOn + "Here is some large text." + DoubleOff);

printJob.ExecuteAsync();
```

如需 ESC/POS (包括可用的命令) 的詳細資訊，請查閱 [Epson ESC/POS 常見問題集](https://content.epson.de/fileadmin/content/files/RSD/downloads/escpos.pdf)。 如需 [**Windows.Devices.PointOfService**](https://docs.microsoft.com/uwp/api/Windows.Devices.PointOfService) 和所有可用功能的詳細資訊，請參閱 MSDN 上的 [PointofService 印表機](https://docs.microsoft.com/uwp/api/Windows.Devices.PointOfService)。
