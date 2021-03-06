---
title: 相機條碼掃描器系統需求
description: 本文列出從 UWP app 使用相機條碼掃描器的需求。
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: 4eed59e302bc34e93d21adef794de02427f2933e
ms.sourcegitcommit: 0dec04de501a3db6b22dfd4a320fc09b5c4a21b5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/04/2019
ms.locfileid: "70243276"
---
# <a name="camera-barcode-scanner-system-requirements"></a>相機條碼掃描器系統需求
從 Windows 10 版本 1803 開始，您可以從通用 Windows 應用程式透過標準攝影機鏡頭讀取條碼。

## <a name="supported-windows-editions"></a>支援的 Windows 版本
- Windows 10 專業版 S 模式
- Windows 10 專業版
- Windows 10 企業版
- Windows 10 IoT 核心版


## <a name="webcam-requirements"></a>網路攝影機需求
| Category      | 建議           | 註解 |
| ------------- | ------------------------ | -------- |
| 對焦         | 自動對焦               | 不建議使用固定焦點 |
| 解析度    | 1920 x 1440 或更高    | 我們正解析度為 1920 x 1440 或更高的數位相機的最佳使用體驗。  如果條碼列印夠大的話，部分解析度較低的相機可讀取標準條碼。 元素較細的條碼可能需要解析度較高的相機。 |
|

## <a name="see-also"></a>另請參閱

### <a name="samples"></a>範例

- [條碼掃描器範例](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
