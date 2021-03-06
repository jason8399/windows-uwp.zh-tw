---
ms.assetid: 370f2c14-4f1e-47b3-9197-24205ab255a3
description: 本文列出適用於 UWP app 的相機功能，以及示範如何使用它們的操作說明文章的連結。
title: 相機
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d7d7fcdeb3740ac4c6851170796243392676d1d1
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74254294"
---
# <a name="camera"></a>相機

本節提供建立通用 Windows 平台 (UWP) app 的指導方針，這類 app 會使用相機或麥克風來擷取相片、視訊或音訊。

## <a name="use-the-windows-built-in-camera-ui"></a>使用 Windows 內建的相機 UI

| 主題 | 描述 |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [使用 Windows 內建攝影機 UI 來捕捉相片和影片](capture-photos-and-video-with-cameracaptureui.md) | 示範如何使用 [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) 類別，透過 Windows 內建的相機 UI 來擷取相片或視訊。 如果您只想讓使用者能夠擷取相片或影片，並將結果傳回給您的 app，這會是最快速且最簡單的方法。  |

## <a name="basic-mediacapture-tasks"></a>基本的 MediaCapture 工作

| 主題 | 描述 |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [顯示相機預覽](simple-camera-preview-access.md) | 示範如何在通用 Windows 平台 (UWP) app 中的 XAML 頁面上快速顯示相機預覽資料流。 |
| [具有 MediaCapture 的基本相片、影片和音訊捕獲](basic-photo-video-and-audio-capture-with-MediaCapture.md) | 示範使用 [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) 類別來擷取相片和視訊的最簡單方式。 **MediaCapture** 類別會公開一組健全的 API，可提供擷取管線的低階控制權，並啟用進階擷取案例，但本文的目的是協助您快速且輕鬆地新增對 app 的基本媒體擷取。 |
| [行動裝置的相機 UI 功能](camera-ui-features-for-mobile-devices.md) | 示範如何利用只會出現在行動裝置上的特殊相機 UI 功能。  |
                                                                                                               
## <a name="advanced-mediacapture-tasks"></a>進階的 MediaCapture 工作   
                                                                                                               
| 主題                                                                                             | 描述                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [使用 MediaCapture 處理裝置和螢幕方向](handle-device-orientation-with-mediacapture.md) | 示範如何在使用協助程式類別擷取相片和視訊時處理裝置方向。 | 
| [使用相機設定檔探索並選取攝影機功能](camera-profiles.md) | 示範如何使用相機設定檔來探索和管理不同視訊擷取裝置的功能。 這其中包括如下的工作：選取支援特定解析度或畫面播放速率的設定檔、選取支援可同時存取多台相機的設定檔，以及選取支援 HDR 的設定檔。 |
| [設定 MediaCapture 的格式、解析度和畫面播放速率](set-media-encoding-properties.md) | 示範如何使用 [**IMediaEncodingProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.IMediaEncodingProperties) 介面，來設定相機預覽資料流及所擷取相片和視訊的解析度和畫面播放速率。 它也會說明如何確保預覽資料流的外觀比例符合所擷取的媒體。 |
| [HDR 和低光線相片捕獲](high-dynamic-range-hdr-photo-capture.md) | 示範如何使用 [**AdvancedPhotoCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) 類別，來擷取高動態範圍 (HDR) 相片和弱光相片。 |
| [手動相機控制項的相片和影片捕獲](capture-device-controls-for-photo-and-video-capture.md) | 示範如何使用手動裝置控制項來啟用美化的相片和視訊擷取案例，包括光學防手震和平滑變焦。 |
| [手動拍攝影片的相機控制項](capture-device-controls-for-video-capture.md) | 示範如何使用手動裝置控制項來啟用美化的視訊擷取案例，包括 HDR 視訊和曝光優先順序。  |
| [影片捕捉的視頻穩定效果](effects-for-video-capture.md) | 示範如何使用影像防震效果。  |
| [MediaCapture 的場景分析](scene-analysis-for-media-capture.md) | 示範如何使用 [**SceneAnalysisEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.SceneAnalysisEffect) 和 [**FaceDetectionEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.FaceDetectionEffect) 來分析媒體擷取預覽資料流的內容。  |
| [使用 VariablePhotoSequence 來捕捉相片序列](variable-photo-sequence.md) | 示範如何擷取可變相片序列，讓您以快速連續的方式擷取多個影像畫面，並針對每個畫面使用不同焦點、閃光燈、ISO、曝光度及曝光補償設定進行設定。  |
| [使用 MediaFrameReader 處理媒體框架](process-media-frames-with-mediaframereader.md) | 示範如何使用 [**MediaFrameReader**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) 搭配 [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture)，從一或多個可用來源取得媒體畫面。來源包括色彩、深度及紅外線相機、音訊裝置，甚至是自訂畫面來源 (例如，能產生骨骼追蹤畫面的來源)。 此功能是針對要讓執行媒體畫面即時處理的 App 使用所設計，例如虛擬實境及深度感知相機 App。  |
| [取得預覽畫面](get-a-preview-frame.md) | 示範如何從媒體擷取預覽資料流取得單一預覽畫面。  |                                                                                                   


## <a name="uwp-app-samples-for-camera"></a>相機的 UWP app 範例

* [相機臉部偵測範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFaceDetection)
* [相機預覽框架範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraGetPreviewFrame)
* [相機 HDR 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraAdvancedCapture)
* [數位相機手動控制項範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraManualControls)
* [相機設定檔範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraProfile)
* [相機解析度範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraResolution)
* [相機入門套件](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraStarterKit)
* [相機視頻穩定範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraVideoStabilization)

## <a name="related-topics"></a>相關主題

* [音訊、視訊和相機](index.md)
 

 




