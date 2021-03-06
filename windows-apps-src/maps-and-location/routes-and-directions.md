---
title: 在地圖上顯示路線和路線指引
description: 要求路線和路線指引，並將它們顯示在您的 app 中。
ms.assetid: BBB4C23A-8F10-41D1-81EA-271BE01AED81
ms.date: 09/20/2017
ms.topic: article
keywords: Windows 10, uwp, 路線, 地圖, 位置, 路線指引
ms.localizationpriority: medium
ms.openlocfilehash: e9e464f9a3b49d3a94edbc8593df58e1e7c24515
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259333"
---
# <a name="display-routes-and-directions-on-a-map"></a>在地圖上顯示路線和路線指引



要求路線和路線指引，並將它們顯示在您的 app 中。

>[!Note]
>若要深入了解如何在應用程式中使用地圖，請下載[通用 Windows 平台 (UWP) 地圖範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)。
>如果地圖功能不是您 app 的核心功能，請考慮改為啟動 Windows 地圖 app。 您可以使用 `bingmaps:`、`ms-drive-to:` 和 `ms-walk-to:` URI 配置，將 Windows 地圖 app 啟動到特定的地圖和轉向建議路線。 如需詳細資訊，請參閱[啟動 Windows 地圖 app](https://docs.microsoft.com/windows/uwp/launch-resume/launch-maps-app)。

 
## <a name="an-intro-to-maproutefinder-results"></a>MapRouteFinder 結果簡介


以下說明路線與路線指引類別如何相關：

* [  **MapRouteFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinder) 類別擁有可取得路線與路線指引的方法。 這些方法會傳回 [**MapRouteFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinderResult)。

* [  **MapRouteFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinderResult) 包含一個 [**MapRoute**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute) 物件。 請透過 [MapRouteFinderResult**的**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinderresult.route)Route 屬性存取這個物件。

* [  **MapRoute**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute) 包含 [**MapRouteLeg**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteLeg) 物件的集合。 請透過 [MapRoute**的**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproute.legs)Legs 屬性存取這個集合。

* 每個 [**MapRouteLeg**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteLeg) 都包含 [**MapRouteManeuver**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteManeuver) 物件的集合。 請透過 [MapRouteLeg**的**](https://docs.microsoft.com/uwp/api/windows.services.maps.maprouteleg.maneuvers)Maneuvers 屬性存取這個集合。

您可以呼叫 [**MapRouteFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinder) 類別的方法來取得行車或步行路線和路線指引。 例如，[**GetDrivingRouteAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinder.getdrivingrouteasync) 或 [**GetWalkingRouteAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinder.getwalkingrouteasync)。

當您要求路線時，可以指定下列條件：

* 您可以只提供起點和終點，或是提供一系列的中繼點來計算路線。

    *停止*導航點會增加其他路線區段，而每一區段都有其自己的行程。 若要指定*停止*導航點，可使用任何的 [**GetDrivingRouteFromWaypointsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinder.getwalkingroutefromwaypointsasync) 多載。

    *透過*導航點定義*停止*導航點之間的中繼位置。 它們不會新增路線區段。  它們只是路線必須經過的導航點。 若要指定*經由*導航點，可使用任何的 [**GetDrivingRouteFromEnhancedWaypointsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinder.getdrivingroutefromenhancedwaypointsasync) 多載。

* 您可以指定最佳化 (例如：最短距離)。

* 您可以指定限制 (例如：避開高速公路)。

## <a name="display-directions"></a>顯示路線指引

[  **MapRouteFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinderResult) 物件包含您可以透過其 [**Route**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute) 屬性存取的 [**MapRoute**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinderresult.route) 物件。

計算的 [**MapRoute**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute) 擁有的屬性可提供行經路線的時間、路線長度，以及包含路線行程的 [**MapRouteLeg**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteLeg) 物件集合。 每個 **MapRouteLeg** 物件都包含 [**MapRouteManeuver**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteManeuver) 物件的集合。 **MapRouteManeuver** 物件包含您可以透過其 [**InstructionText**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutemaneuver.instructiontext) 屬性存取的路線指引。

>[!IMPORTANT]
>您必須指定地圖驗證金鑰，才能使用地圖服務。 如需詳細資訊，請參閱[要求地圖驗證金鑰](authentication-key.md)。

 

```csharp
using System;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void button_Click(object sender, RoutedEventArgs e)
{
   // Start at Microsoft in Redmond, Washington.
   BasicGeoposition startLocation = new BasicGeoposition() {Latitude=47.643,Longitude=-122.131};

   // End at the city of Seattle, Washington.
   BasicGeoposition endLocation = new BasicGeoposition() {Latitude = 47.604,Longitude= -122.329};

   // Get the route between the points.
   MapRouteFinderResult routeResult =
         await MapRouteFinder.GetDrivingRouteAsync(
         new Geopoint(startLocation),
         new Geopoint(endLocation),
         MapRouteOptimization.Time,
         MapRouteRestrictions.None);

   if (routeResult.Status == MapRouteFinderStatus.Success)
   {
      System.Text.StringBuilder routeInfo = new System.Text.StringBuilder();

      // Display summary info about the route.
      routeInfo.Append("Total estimated time (minutes) = ");
      routeInfo.Append(routeResult.Route.EstimatedDuration.TotalMinutes.ToString());
      routeInfo.Append("\nTotal length (kilometers) = ");
      routeInfo.Append((routeResult.Route.LengthInMeters / 1000).ToString());

      // Display the directions.
      routeInfo.Append("\n\nDIRECTIONS\n");

      foreach (MapRouteLeg leg in routeResult.Route.Legs)
      {
         foreach (MapRouteManeuver maneuver in leg.Maneuvers)
         {
            routeInfo.AppendLine(maneuver.InstructionText);
         }
      }

      // Load the text box.
      tbOutputText.Text = routeInfo.ToString();
   }
   else
   {
      tbOutputText.Text =
            "A problem occurred: " + routeResult.Status.ToString();
   }
}
```

這個範例會在 `tbOutputText` 文字方塊中顯示下列結果。

``` syntax
Total estimated time (minutes) = 18.4833333333333
Total length (kilometers) = 21.847

DIRECTIONS
Head north on 157th Ave NE.
Turn left onto 159th Ave NE.
Turn left onto NE 40th St.
Turn left onto WA-520 W.
Enter the freeway WA-520 from the right.
Keep left onto I-5 S/Portland.
Keep right and leave the freeway at exit 165A towards James St..
Turn right onto James St.
You have reached your destination.
```

## <a name="display-routes"></a>顯示路線


若要在 [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute) 上顯示 [**MapRoute**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)，請利用 [MapRoute**來建構**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapRouteView)MapRouteView。 接著，將 **MapRouteView** 新增到 [MapControl**的**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.routes)Routes 集合。

>[!IMPORTANT]
>您必須指定地圖驗證金鑰，才能使用地圖服務或地圖控制項。 如需詳細資訊，請參閱[要求地圖驗證金鑰](authentication-key.md)。

 

```csharp
using System;
using Windows.Devices.Geolocation;
using Windows.Services.Maps;
using Windows.UI;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Maps;
...
private async void ShowRouteOnMap()
{
   // Start at Microsoft in Redmond, Washington.
   BasicGeoposition startLocation = new BasicGeoposition() { Latitude = 47.643, Longitude = -122.131 };

   // End at the city of Seattle, Washington.
   BasicGeoposition endLocation = new BasicGeoposition() { Latitude = 47.604, Longitude = -122.329 };


   // Get the route between the points.
   MapRouteFinderResult routeResult =
         await MapRouteFinder.GetDrivingRouteAsync(
         new Geopoint(startLocation),
         new Geopoint(endLocation),
         MapRouteOptimization.Time,
         MapRouteRestrictions.None);

   if (routeResult.Status == MapRouteFinderStatus.Success)
   {
      // Use the route to initialize a MapRouteView.
      MapRouteView viewOfRoute = new MapRouteView(routeResult.Route);
      viewOfRoute.RouteColor = Colors.Yellow;
      viewOfRoute.OutlineColor = Colors.Black;

      // Add the new MapRouteView to the Routes collection
      // of the MapControl.
      MapWithRoute.Routes.Add(viewOfRoute);

      // Fit the MapControl to the route.
      await MapWithRoute.TrySetViewBoundsAsync(
            routeResult.Route.BoundingBox,
            null,
            Windows.UI.Xaml.Controls.Maps.MapAnimationKind.None);
   }
}
```

這個範例會在名為 [MapWithRoute**的**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)MapControl 上顯示下列項目。

![顯示路線的地圖控制項。](images/routeonmap.png)

以下是此範例的版本，其使用兩個*停止*個導航點之間的*經由*導航點：

```csharp
using System;
using Windows.Devices.Geolocation;
using Windows.Services.Maps;
using Windows.UI;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Maps;
...
private async void ShowRouteOnMap()
{
  Geolocator locator = new Geolocator();
  locator.DesiredAccuracyInMeters = 1;
  locator.PositionChanged += Locator_PositionChanged;

  BasicGeoposition point1 = new BasicGeoposition() { Latitude = 47.649693, Longitude = -122.144908 };
  BasicGeoposition point2 = new BasicGeoposition() { Latitude = 47.6205, Longitude = -122.3493 };
  BasicGeoposition point3 = new BasicGeoposition() { Latitude = 48.649693, Longitude = -122.144908 };

  // Get Driving Route from point A  to point B thru point C
  var path = new List<EnhancedWaypoint>();

  path.Add(new EnhancedWaypoint(new Geopoint(point1), WaypointKind.Stop));
  path.Add(new EnhancedWaypoint(new Geopoint(point2), WaypointKind.Via));
  path.Add(new EnhancedWaypoint(new Geopoint(point3), WaypointKind.Stop));

  MapRouteFinderResult routeResult =  await MapRouteFinder.GetDrivingRouteFromEnhancedWaypointsAsync(path);

  if (routeResult.Status == MapRouteFinderStatus.Success)
  {
      MapRouteView viewOfRoute = new MapRouteView(routeResult.Route);
      viewOfRoute.RouteColor = Colors.Yellow;
      viewOfRoute.OutlineColor = Colors.Black;

      myMap.Routes.Add(viewOfRoute);

      await myMap.TrySetViewBoundsAsync(
            routeResult.Route.BoundingBox,
            null,
            Windows.UI.Xaml.Controls.Maps.MapAnimationKind.None);
  }
}
```

## <a name="related-topics"></a>相關主題

* [Bing 地圖服務開發人員中心](https://www.bingmapsportal.com/)
* [UWP 地圖範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [地圖的設計指導方針](https://docs.microsoft.com/windows/uwp/maps-and-location/controls-map)
* [組建2015影片：在您的 Windows 應用程式中跨電話、平板電腦和 PC 運用地圖和位置](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 車流量應用程式範例](https://github.com/Microsoft/Windows-appsample-trafficapp)
