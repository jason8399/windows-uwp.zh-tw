---
author: normesta
title: 地圖和位置概觀
description: 本節說明如何在您的 app 中顯示地圖、使用地圖服務、尋找位置，以及設定地理柵欄。 本節也示範如何將 Windows 地圖應用程式啟動到特定地圖、路線或一組轉向建議導航路線指引。
ms.assetid: F4C1F094-CF46-4B15-9D80-C1A26A314521
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, 地圖, 位置, 地圖服務
ms.openlocfilehash: 9f2c15c8d4bab5a764b8973c4eecb220ed6d8f38
ms.sourcegitcommit: 378382419f1fda4e4df76ffa9c8cea753d271e6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2017
ms.locfileid: "665334"
---
# <a name="maps-and-location-overview"></a>地圖和位置概觀


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本節說明如何在您的 app 中顯示地圖、使用地圖服務、尋找位置，以及設定地理柵欄。 本節也示範如何將 Windows 地圖 app 啟動到特定地圖、路線或一組轉向建議導航路線指引。

> **提示** 若要深入了解如何在應用程式中使用地圖和位置，請從 GitHub 的 [Windows-universal-samples 存放庫](http://go.microsoft.com/fwlink/p/?LinkId=619979)下載下列範例：
-   [通用 Windows 平台 (UWP) 地圖範例](http://go.microsoft.com/fwlink/p/?LinkId=619977)
-   [UWP 地理位置範例](http://go.microsoft.com/fwlink/p/?linkid=533278)

 

## <a name="display-maps"></a>顯示地圖


藉由來自 [**Windows.UI.Xaml.Controls.Maps**](https://msdn.microsoft.com/library/windows/apps/dn610751) 命名空間的 API，即可在您的 app 中以 2D、3D 或 Streetside 檢視顯示地圖。 您可以使用圖釘、影像、形狀或 XAML UI 元素，在地圖上標示興趣點 (POI)。 您也可以重疊顯示並排影像或完全取代地圖影像。

| 主題 | 說明 |
|-------|-------------|
| [要求地圖驗證金鑰](authentication-key.md) | 您的 App 必須經過驗證，才能使用 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 和 [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 命名空間中的地圖服務。 若要驗證您的 App，您必須指定地圖驗證金鑰。 本文說明如何從 [Bing 地圖服務開發人員中心](https://www.bingmapsportal.com/)要求地圖驗證金鑰，然後將它新增到您的應用程式。 |
| [顯示 2D、3D 和 Streetside 檢視的地圖](display-maps.md) | 藉由使用 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 類別，即可在您的 App 中顯示可自訂的地圖。 本主題也會介紹空照圖 3D 和街景檢視。 |
| [在地圖上顯示興趣點 (POI)](display-poi.md) | 藉由使用圖釘、影像、形狀及 XAML UI 元素，即可在地圖上新增興趣點 (POI)。 |
| [在地圖上重疊顯示並排影像](overlay-tiled-images.md) | 藉由使用磚來源，即可在地圖上重疊顯示協力廠商或自訂的並排影像。 您可以使用磚來源來重疊顯示專業資訊，例如氣象資料、人口資料或地震資料，或是使用磚來源完全取代預設的地圖。 |



## <a name="access-map-services"></a>存取地圖服務

藉由使用來自 [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 命名空間的 API，即可將路線、路線指引及地理編碼功能新增到您的 app。 您也可以將 [設定] app 直接啟動到適當的頁面，以協助使用者管理離線地圖。

| 主題 | 說明 |
|-----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [要求地圖驗證金鑰](authentication-key.md) | 您的 App 必須經過驗證，才能使用 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 和 [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 命名空間中的地圖服務。 若要驗證您的 App，您必須指定地圖驗證金鑰。 本文說明如何從 [Bing 地圖服務開發人員中心](https://www.bingmapsportal.com/)要求地圖驗證金鑰，然後將它新增到您的 App。 |
| [在地圖上顯示興趣點 (POI)](display-poi.md) | 藉由使用圖釘、影像、形狀及 XAML UI 元素，即可在地圖上新增興趣點 (POI)。 |
| [顯示路線和路線指引](routes-and-directions.md) | 要求路線和路線指引，並將它們顯示在您的 app 中。 |
| [執行地理編碼和反向地理編碼](geocoding.md) | 呼叫 [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 命名空間中 [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) 類別的方法，將地址轉換成地理位置 (地理編碼) 以及將地理位置轉換成地址 (反向地理編碼)。 |


## <a name="get-the-users-location"></a>取得使用者的位置

藉由使用來自 [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/br225603) 命名空間的 API，即可讓您的 app 取得使用者目前的位置，並在位置變更時收到通知。 這些 API 成員也常用在地圖 API 的參數中。 來自 [**Windows.Devices.Geolocation.Geofencing**](https://msdn.microsoft.com/library/windows/apps/dn263744) 命名空間的 API 可讓您的 App 在使用者進入或離開地理柵欄 (預先定義的地理區域) 時收到通知。

| 主題 | 說明 |
|-------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [要求地圖驗證金鑰](authentication-key.md) | 您的 App 必須經過驗證，才能使用 [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 和 [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 命名空間中的地圖服務。 若要驗證您的 App，您必須指定地圖驗證金鑰。 本文說明如何從 [Bing 地圖服務開發人員中心](https://www.bingmapsportal.com/)要求地圖驗證金鑰，然後將它新增到您的 App。 |
| [定位感知 App 的設計指導方針](guidelines-and-checklist-for-detecting-location.md) | 需要存取使用者位置之 App 的效能指導方針。 |
| [取得使用者的位置](get-location.md) | 取得使用者位置的存取權，並擷取位置。 |
| [地理柵欄設計指導方針](guidelines-for-geofencing.md) | 使用地理柵欄功能之 App 的效能指導方針。 |
| [設定地理柵欄](set-up-a-geofence.md) | 在您的 App 中設定地理柵欄，並了解如何在前景和背景中處理通知。 |

## <a name="launch-the-windows-maps-app"></a>啟動 Windows 地圖 App

如這裡所示，您的 app 可以啟動「Windows 地圖」app 以顯示特定的地圖和轉向建議導航路線指引。 請考慮使用「Windows 地圖」app 來提供地圖功能，而不要直接在您自己的 app 中提供該功能。 如需詳細資訊，請參閱[啟動 Windows 地圖 app](https://msdn.microsoft.com/library/windows/apps/mt228341)。

![Windows 地圖 app 的範例。](images/mapnyc.png)

## <a name="related-topics"></a>相關主題

* [UWP 地圖範例](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [UWP 地理位置範例](http://go.microsoft.com/fwlink/p/?linkid=533278)
* [Bing 地圖服務開發人員中心](https://www.bingmapsportal.com/)
* [取得目前的位置](get-location.md)
* [定位感知 app 的設計指導方針](guidelines-and-checklist-for-detecting-location.md)
* [地圖的設計指導方針](controls-map.md)
* [隱私權感知 app 的設計指導方針](https://msdn.microsoft.com/library/windows/apps/hh768223)
* [Build 2015 影片：跨手機、平板電腦和電腦運用 Windows app 中的地圖與位置功能](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 車流量 app 範例](http://go.microsoft.com/fwlink/p/?LinkId=619982)