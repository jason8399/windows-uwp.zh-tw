---
title: 設定條碼掃描器
description: 了解如何設定預期的應用程式的條碼掃描器。
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: 8466c56ef73a1c38c67e28cf52de7f380e6c563a
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321578"
---
# <a name="configure-a-barcode-scanner"></a>設定條碼掃描器

條碼掃描器可以幾種不同模式設定。  請務必針對其用途妥善設定條碼掃描器。

許多條碼掃描器可在**鍵盤 Wedge** 模式中設定，其可使條碼掃描器顯示為 Windows 鍵盤。  這可讓您掃描條碼到不是條碼掃描器感知的應用程式，例如記事本。  當您在此模式中掃描條碼時，從條碼掃描器解碼的資料會插入到您的插入點，宛如您使用鍵盤輸入資料一般。  若要進一步從您的 UWP 應用程式控制條碼掃描器，您必須在非鍵盤 Wedge 模式中設定。

## <a name="usb-barcode-scanner"></a>USB 條碼掃描器
USB 連接的條碼掃描器必須在 **HID POS 掃描器**模式中設定，才能搭配 Windows 內含的條碼掃描器驅動程式使用。 此驅動程式會實作**HID 點的銷售資料使用量資料表**規格發行至[USB HID](https://www.usb.org/hid)。  如需啟用 **HID POS 掃描器**模式的指示操作，請參閱條碼掃描器的文件，或連絡條碼掃描器的製造商。  一旦設定為 **HID POS 掃描器**，您的條碼掃描器會在 **POS 條碼掃描器**節點下的 \[裝置管理員\] 中顯示為 **POS HID 條碼掃描器**。

您的條碼掃描器製造商也可能會有廠商特定驅動程式，其使用 **HID POS 掃描器**以外的模式支援 UWP 條碼掃描器 API。  如果您已安裝的製造商提供驅動程式與 UWP 條碼掃描器 Api 相容，您可能會看到底下所列的廠商特定裝置**POS 條碼掃描器**裝置管理員 中。

## <a name="bluetooth-barcode-scanner"></a>藍牙條碼掃描器
藍牙連接的掃描器必須在**序列埠通訊協定 - 簡單序列介面 (SPP SSI)** 模式中設定，以搭配 UWP 條碼掃描器 API 使用。  如需啟用 **SPP-SSI 模式**的指示操作，請參閱條碼掃描器的文件，或連絡條碼掃描器的製造商。

您可以使用您的藍芽條碼掃描器之前您必須將使用**設定 > 裝置 > 藍芽 & 其他裝置 > 新增藍芽或其他裝置**。

您可以起始，並控制配對的處理程序使用[Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.enumeration)命名空間。  請參閱[配對裝置](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices)如需詳細資訊。

[!INCLUDE [feedback](./includes/pos-feedback.md)]