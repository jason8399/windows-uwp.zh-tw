---
title: 在地圖上重疊顯示並排影像
description: 藉由使用磚來源，即可在地圖上重疊顯示協力廠商或自訂的並排影像。 您可以使用磚來源來重疊顯示專業資訊，例如氣象資料、人口資料或地震資料，或是使用磚來源完全取代預設的地圖。
ms.assetid: 066BD6E2-C22B-4F5B-AA94-5D6C86A09BDF
ms.date: 07/19/2018
ms.topic: article
keywords: windows 10, uwp, map, location, images, overlay, 地圖, 位置, 影像, 重疊
ms.localizationpriority: medium
ms.openlocfilehash: 501e28f88d07a85c1ded3ae880d1e679169ac36a
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260378"
---
# <a name="overlay-tiled-images-on-a-map"></a>在地圖上重疊顯示並排影像

藉由使用磚來源，即可在地圖上重疊顯示協力廠商或自訂的並排影像。 您可以使用磚來源來重疊顯示專業資訊，例如氣象資料、人口資料或地震資料，或是使用磚來源完全取代預設的地圖。

**提示**若要深入了解如何在應用程式中使用地圖，請到 Github 下載[通用 Windows 平台 (UWP) 地圖範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)。

<a id="tileintro" />

## <a name="tiled-image-overview"></a>並排影像概觀

Nokia Maps 和「Bing 地圖服務」之類的地圖服務都是將地圖切成正方形磚以便快速抓取和顯示。 這些磚的大小是 256 像素 x 256 像素，並以多個詳細層級預先轉譯。 許多協力廠商服務也會提供切成磚的地圖式資料。 您可以使用磚來源來抓取協力廠商磚，或建立您自己的自訂磚，然後在 [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 中顯示的地圖上重疊顯示這些磚。

**重要**   當您使用磚來源時，您不需要撰寫程式碼來要求或放置個別的磚。 [  **MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 會在需要磚時要求磚。 每個要求都會指定 X 和 Y 座標，以及個別磚的縮放比例。 您只需指定要用來抓取 **UriFormatString** 屬性中的磚的 URI 格式或檔案名稱。 也就是說，您需在基底 URI 或檔案名稱中插入可置換參數，以指出要將 X 和 Y 座標傳遞到哪裡，以及每個磚的縮放比例。

以下是 [**HttpMapTileDataSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) 的 [**UriFormatString**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource) 屬性範例，示範 X 和 Y 座標及縮放比例的可置換參數。

```syntax
http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}
```

(X 和 Y 座標代表具指定詳細層級之世界地圖內個別磚的位置。 磚編號系統從地圖左上角的 {0, 0} 開始。 例如，位於 {1, 2} 的磚是在磚格線之第三列的第二欄中。)

如需有關地圖服務所使用之磚系統的詳細資訊，請參閱 [Bing 地圖服務磚系統](https://docs.microsoft.com/bingmaps/articles/bing-maps-tile-system?redirectedfrom=MSDN)。

### <a name="overlay-tiles-from-a-tile-source"></a>重疊顯示來自磚來源的磚

藉由使用 [**MapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileDataSource)，即可在地圖上重疊顯示來自磚來源的並排影像。

1.  將繼承自 [**MapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileDataSource) 的三個磚資料來源類別其中之一具現化。

    -   [**HttpMapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource)
    -   [**LocalMapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource)
    -   [**CustomMapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.CustomMapTileDataSource)

    藉由在基底 URI 或檔案名稱中插入可置換參數，設定要用來要求磚的 **UriFormatString**。

    下列範例會將 [**HttpMapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource) 具現化。 這個範例會在 [HttpMapTileDataSource**的建構函式中指定**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring)UriFormatString 的值。

    ```csharp
        HttpMapTileDataSource dataSource = new HttpMapTileDataSource(
          "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");
    ```

2.  將 [**MapTileSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource) 具現化並設定。 將您在上一個步驟中設定的 [**MapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileDataSource) 指定為 [MapTileSource**的**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilesource.datasource)DataSource。

    下列範例會在 [**MapTileSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilesource.datasource) 的建構函式中指定 [**DataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource)。

    ```csharp
        MapTileSource tileSource = new MapTileSource(dataSource);
    ```

    您可以使用 [**MapTileSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource) 的屬性來限制顯示磚的條件。

    -   藉由提供 [**Bounds**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilesource.bounds) 屬性的值，只顯示特定地理區域內的磚。
    -   藉由提供 [**ZoomLevelRange**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilesource.zoomlevelrange) 屬性的值，只以特定詳細層級顯示磚。

    您也可以依選擇設定 [**MapTileSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource) 的其他屬性來影響磚的載入或顯示，例如 [**Layer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilesource.layer)、[**AllowOverstretch**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilesource.allowoverstretch)、[**IsRetryEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilesource.isretryenabled) 及 [**IsTransparencyEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilesource.istransparencyenabled)。

3.  將 [**MapTileSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource) 新增到 [**MapControl**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.tilesources) 的 [**TileSources**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 集合。

    ```csharp
         MapControl1.TileSources.Add(tileSource);
    ```

## <a name="overlay-tiles-from-a-web-service"></a>重疊顯示來自 Web 服務的磚


藉由使用 [**HttpMapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource)，即可重疊顯示抓取自 Web 服務的並排影像。

1.  將 [**HttpMapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource) 具現化。
2.  指定 Web 服務預期做為 [**UriFormatString**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) 屬性值的 URI 格式。 若要建立這個值，請在基底 URI 中插入可置換參數。 例如，在下列程式碼範例中，**UriFormatString** 的值是：

    ``` syntax
    http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}
    ```

    Web 服務必須支援包含可置換參數 {x}、{y} 及 {zoomlevel} 的 URI。 大部分 Web 服務 (例如 Nokia、Bing 及 Google) 都支援此格式的 URI。 如果 Web 服務需要 [**UriFormatString**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) 屬性所無法提供的額外引數，您就必須建立自訂 URI。 您可以處理 [**UriRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.urirequested) 事件來建立和傳回自訂 URI。 如需詳細資訊，請參閱本主題中稍後的[提供自訂 URI](#customuri) 一節。

3.  然後，依照先前[並排影像概觀](#tileintro)中所述的其餘步驟進行。

下列範例會在北美地圖上重疊顯示來自一個虛構 Web 服務的磚。 [  **UriFormatString**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) 的值是在 [**HttpMapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource) 的建構函式中指定。 在這個範例中，只有在選擇性 [**Bounds**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilesource.bounds) 屬性所指定的地理界限內，才會顯示磚。

```csharp
private void AddHttpMapTileSource()
{
    // Create the bounding box in which the tiles are displayed.
    // This example represents North America.
    BasicGeoposition northWestCorner =
        new BasicGeoposition() { Latitude = 48.38544, Longitude = -124.667360 };
    BasicGeoposition southEastCorner =
        new BasicGeoposition() { Latitude = 25.26954, Longitude = -80.30182 };
    GeoboundingBox boundingBox = new GeoboundingBox(northWestCorner, southEastCorner);

    // Create an HTTP data source.
    // This example retrieves tiles from a fictitious web service.
    HttpMapTileDataSource dataSource = new HttpMapTileDataSource(
        "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");

    // Optionally, add custom HTTP headers if the web service requires them.
    dataSource.AdditionalRequestHeaders.Add("header name", "header value");

    // Create a tile source and add it to the Map control.
    MapTileSource tileSource = new MapTileSource(dataSource);
    tileSource.Bounds = boundingBox;
    MapControl1.TileSources.Add(tileSource);
}
```

```cppwinrt
...
#include <winrt/Windows.Devices.Geolocation.h>
#include <winrt/Windows.UI.Xaml.Controls.Maps.h>
...
void MainPage::AddHttpMapTileSource()
{
    Windows::Devices::Geolocation::BasicGeoposition northWest{ 48.38544, -124.667360 };
    Windows::Devices::Geolocation::BasicGeoposition southEast{ 25.26954, -80.30182 };
    Windows::Devices::Geolocation::GeoboundingBox boundingBox{ northWest, southEast };

    Windows::UI::Xaml::Controls::Maps::HttpMapTileDataSource dataSource{
        L"http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}" };

    dataSource.AdditionalRequestHeaders().Insert(L"header name", L"header value");

    Windows::UI::Xaml::Controls::Maps::MapTileSource tileSource{ dataSource };
    tileSource.Bounds(boundingBox);

    MapControl1().TileSources().Append(tileSource);
}
...
```

```cpp
void MainPage::AddHttpMapTileSource()
{
    BasicGeoposition northWest = { 48.38544, -124.667360 };
    BasicGeoposition southEast = { 25.26954, -80.30182 };
    GeoboundingBox^ boundingBox = ref new GeoboundingBox(northWest, southEast);

    auto dataSource = ref new Windows::UI::Xaml::Controls::Maps::HttpMapTileDataSource(
        "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");

    dataSource->AdditionalRequestHeaders->Insert("header name", "header value");

    auto tileSource = ref new Windows::UI::Xaml::Controls::Maps::MapTileSource(dataSource);
    tileSource->Bounds = boundingBox;

    this->MapControl1->TileSources->Append(tileSource);
}
```

## <a name="overlay-tiles-from-local-storage"></a>重疊顯示來自本機存放區的磚


藉由使用 [**LocalMapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource)，即可重疊顯示在本機存放區中儲存為檔案的並排影像。 通常您會將這些檔案與您的 app 封裝在一起並散布。

1.  將 [**LocalMapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource) 具現化。
2.  指定檔案名稱的格式做為 [**UriFormatString**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.uriformatstring) 屬性的值。 若要建立這個值，請在基底檔案名稱中插入可置換參數。 例如，在下列程式碼範例中，[**UriFormatString**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) 的值是：

    ``` syntax
        Tile_{zoomlevel}_{x}_{y}.png
    ```

    如果檔案名稱格式需要 [**UriFormatString**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.uriformatstring) 屬性所無法提供的額外引數，您就必須建立自訂 URI。 您可以處理 [**UriRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.urirequested) 事件來建立和傳回自訂 URI。 如需詳細資訊，請參閱本主題中稍後的[提供自訂 URI](#customuri) 一節。

3.  然後，依照先前[並排影像概觀](#tileintro)中所述的其餘步驟進行。

您可以使用下列通訊協定和位置以從本機存放區載入磚：

| Uri | 其他資訊 |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| ms-appx:/// | 指向 app 安裝資料夾的根目錄。 |
|  | 這是 [Package.InstalledLocation](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package.installedlocation) 屬性所參考的位置。 |
| ms-appdata:///local | 指向 app 本機存放區的根目錄。 |
|  | 這是 [ApplicationData.LocalFolder](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder) 屬性所參考的位置。 |
| ms-appdata:///temp | 指向 app 的暫存資料夾。 |
|  | 這是 [ApplicationData.TemporaryFolder](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.temporaryfolder) 屬性所參考的位置。 |

 

下列範例會使用 `ms-appx:///` 通訊協定來載入在 app 安裝資料夾中儲存為檔案的磚。 [  **UriFormatString**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.uriformatstring) 的值是在 [**LocalMapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource) 的建構函式中指定。 在這個範例中，只有當地圖的縮放比例是在選擇性 [**ZoomLevelRange**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilesource.zoomlevelrange) 屬性指定的範圍內時，才會顯示磚。

```csharp
        void AddLocalMapTileSource()
        {
            // Specify the range of zoom levels
            // at which the overlaid tiles are displayed.
            MapZoomLevelRange range;
            range.Min = 11;
            range.Max = 20;

            // Create a local data source.
            LocalMapTileDataSource dataSource = new LocalMapTileDataSource(
                "ms-appx:///TileSourceAssets/Tile_{zoomlevel}_{x}_{y}.png");

            // Create a tile source and add it to the Map control.
            MapTileSource tileSource = new MapTileSource(dataSource);
            tileSource.ZoomLevelRange = range;
            MapControl1.TileSources.Add(tileSource);
        }
```

<a id="customuri" />

## <a name="provide-a-custom-uri"></a>提供自訂 URI

如果 [**HttpMapTileDataSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) 的 [**UriFormatString**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource) 屬性或 [**LocalMapTileDataSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.uriformatstring) 的 [**UriFormatString**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource) 屬性所提供的可置換參數不足以擷取您的磚，您就必須建立自訂 URI。 為 **UriRequested** 事件提供自訂處理常式以建立及傳回自訂 URI。 將會針對每個個別磚引發 **UriRequested** 事件。

1.  在 **UriRequested** 事件的自訂處理常式中，將必要的自訂引數與 [**MapTileUriRequestedEventArgs**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptileurirequestedeventargs.x) 的 [**X**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptileurirequestedeventargs.y)、[**Y**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptileurirequestedeventargs.zoomlevel) 及 [**ZoomLevel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileUriRequestedEventArgs) 屬性結合，以建立自訂 URI。
2.  傳回 [**MapTileUriRequest**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptileurirequest.uri) (包含在 [**MapTileUriRequestedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileUriRequest) 的 [**Request**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptileurirequestedeventargs.request) 屬性中) 之 [**Uri**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileUriRequestedEventArgs) 屬性中的自訂 URI。

下列範例示範如何為 **UriRequested** 事件建立自訂處理常式來提供自訂 URI。 同時也示範當您需要以非同步方式執行工作來建立自訂 URI 時，要如何實作延遲模式。

```csharp
using Windows.UI.Xaml.Controls.Maps;
using System.Threading.Tasks;
...
            var httpTileDataSource = new HttpMapTileDataSource();
            // Attach a handler for the UriRequested event.
            httpTileDataSource.UriRequested += HandleUriRequestAsync;
            MapTileSource httpTileSource = new MapTileSource(httpTileDataSource);
            MapControl1.TileSources.Add(httpTileSource);
...
        // Handle the UriRequested event.
        private async void HandleUriRequestAsync(HttpMapTileDataSource sender,
            MapTileUriRequestedEventArgs args)
        {
            // Get a deferral to do something asynchronously.
            // Omit this line if you don't have to do something asynchronously.
            var deferral = args.Request.GetDeferral();

            // Get the custom Uri.
            var uri = await GetCustomUriAsync(args.X, args.Y, args.ZoomLevel);

            // Specify the Uri in the Uri property of the MapTileUriRequest.
            args.Request.Uri = uri;

            // Notify the app that the custom Uri is ready.
            // Omit this line also if you don't have to do something asynchronously.
            deferral.Complete();
        }

        // Create the custom Uri.
        private async Task<Uri> GetCustomUriAsync(int x, int y, int zoomLevel)
        {
            // Do something asynchronously to create and return the custom Uri.        }
        }
```

## <a name="overlay-tiles-from-a-custom-source"></a>重疊顯示來自自訂來源的磚

藉由使用 [**CustomMapTileDataSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.CustomMapTileDataSource)，即可重疊顯示自訂磚。 您可以透過程式設計方式在記憶體中即時建立磚，或撰寫您自己的程式碼，以從另一個來源載入現有的磚。

若要建立或載入自訂磚，請為 [**BitmapRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.custommaptiledatasource.bitmaprequested) 事件提供自訂處理常式。 將會針對每個個別磚引發 **BitmapRequested** 事件。

1.  在 [**BitmapRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.custommaptiledatasource.bitmaprequested) 事件的自訂處理常式中，將必要的自訂引數與 [**MapTileBitmapRequestedEventArgs**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.x) 的 [**X**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.y)、[**Y**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.zoomlevel) 及 [**ZoomLevel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileBitmapRequestedEventArgs) 屬性結合，以建立或抓取自訂磚。
2.  傳回 [**MapTileBitmapRequest**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequest.pixeldata) (包含在 [**MapTileBitmapRequestedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileBitmapRequest) 的 [**Request**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.request) 屬性中) 之 [**PixelData**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileBitmapRequestedEventArgs) 屬性中的自訂磚。 **PixelData** 屬性的類型為 [**IRandomAccessStreamReference**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IRandomAccessStreamReference)。

下列範例示範如何為 **BitmapRequested** 事件建立自訂處理常式來提供自訂磚。 這個範例會建立局部不透明的相同紅色磚。 這個範例會忽略 [**MapTileBitmapRequestedEventArgs**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.x) 的 [**X**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.y)、[**Y**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.zoomlevel) 及 [**ZoomLevel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileBitmapRequestedEventArgs) 屬性。 雖然這不是真實世界的範例，但是此範例示範了如何在記憶體中即時建立自訂磚。 此範例同時也示範當您需要以非同步方式執行工作來建立自訂磚時，要如何實作延遲模式。

```csharp
using Windows.UI.Xaml.Controls.Maps;
using Windows.Storage.Streams;
using System.Threading.Tasks;
...
        CustomMapTileDataSource customDataSource = new CustomMapTileDataSource();
        // Attach a handler for the BitmapRequested event.
        customDataSource.BitmapRequested += customDataSource_BitmapRequestedAsync;
        customTileSource = new MapTileSource(customDataSource);
        MapControl1.TileSources.Add(customTileSource);
...
        // Handle the BitmapRequested event.
        private async void customDataSource_BitmapRequestedAsync(
            CustomMapTileDataSource sender,
            MapTileBitmapRequestedEventArgs args)
        {
            var deferral = args.Request.GetDeferral();
            args.Request.PixelData = await CreateBitmapAsStreamAsync();
            deferral.Complete();
        }

        // Create the custom tiles.
        // This example creates red tiles that are partially opaque.
        private async Task<RandomAccessStreamReference> CreateBitmapAsStreamAsync()
        {
            int pixelHeight = 256;
            int pixelWidth = 256;
            int bpp = 4;

            byte[] bytes = new byte[pixelHeight * pixelWidth * bpp];

            for (int y = 0; y < pixelHeight; y++)
            {
                for (int x = 0; x < pixelWidth; x++)
                {
                    int pixelIndex = y * pixelWidth + x;
                    int byteIndex = pixelIndex * bpp;

                    // Set the current pixel bytes.
                    bytes[byteIndex] = 0xff;        // Red
                    bytes[byteIndex + 1] = 0x00;    // Green
                    bytes[byteIndex + 2] = 0x00;    // Blue
                    bytes[byteIndex + 3] = 0x80;    // Alpha (0xff = fully opaque)
                }
            }

            // Create RandomAccessStream from byte array.
            InMemoryRandomAccessStream randomAccessStream =
                new InMemoryRandomAccessStream();
            IOutputStream outputStream = randomAccessStream.GetOutputStreamAt(0);
            DataWriter writer = new DataWriter(outputStream);
            writer.WriteBytes(bytes);
            await writer.StoreAsync();
            await writer.FlushAsync();
            return RandomAccessStreamReference.CreateFromStream(randomAccessStream);
        }
```

```cppwinrt
...
#include <winrt/Windows.Storage.Streams.h>
...
Windows::Foundation::IAsyncOperation<Windows::Storage::Streams::InMemoryRandomAccessStream> MainPage::CustomRandomAccessStream()
{
    constexpr int pixelHeight{ 256 };
    constexpr int pixelWidth{ 256 };
    constexpr int bpp{ 4 };

    std::array<uint8_t, pixelHeight * pixelWidth * bpp> bytes;

    for (int y = 0; y < pixelHeight; y++)
    {
        for (int x = 0; x < pixelWidth; x++)
        {
            int pixelIndex{ y * pixelWidth + x };
            int byteIndex{ pixelIndex * bpp };

            // Set the current pixel bytes.
            bytes[byteIndex] = (byte)(std::rand() % 256);        // Red
            bytes[byteIndex + 1] = (byte)(std::rand() % 256);    // Green
            bytes[byteIndex + 2] = (byte)(std::rand() % 256);    // Blue
            bytes[byteIndex + 3] = (byte)((std::rand() % 56) + 200);    // Alpha (0xff = fully opaque)
        }
    }

    // Create RandomAccessStream from byte array.
    Windows::Storage::Streams::InMemoryRandomAccessStream randomAccessStream;
    Windows::Storage::Streams::IOutputStream outputStream{ randomAccessStream.GetOutputStreamAt(0) };
    Windows::Storage::Streams::DataWriter writer{ outputStream };
    writer.WriteBytes(bytes);

    co_await writer.StoreAsync();
    co_await writer.FlushAsync();

    co_return randomAccessStream;
}
...
```

```cpp
InMemoryRandomAccessStream^ TileSources::CustomRandomAccessStream::get()
{
    int pixelHeight = 256;
    int pixelWidth = 256;
    int bpp = 4;

    Array<byte>^ bytes = ref new Array<byte>(pixelHeight * pixelWidth * bpp);

    for (int y = 0; y < pixelHeight; y++)
    {
        for (int x = 0; x < pixelWidth; x++)
        {
            int pixelIndex = y * pixelWidth + x;
            int byteIndex = pixelIndex * bpp;

            // Set the current pixel bytes.
            bytes[byteIndex] = (byte)(std::rand() % 256);        // Red
            bytes[byteIndex + 1] = (byte)(std::rand() % 256);    // Green
            bytes[byteIndex + 2] = (byte)(std::rand() % 256);    // Blue
            bytes[byteIndex + 3] = (byte)((std::rand() % 56) + 200);    // Alpha (0xff = fully opaque)
        }
    }

    // Create RandomAccessStream from byte array.
    InMemoryRandomAccessStream^ randomAccessStream = ref new InMemoryRandomAccessStream();
    IOutputStream^ outputStream = randomAccessStream->GetOutputStreamAt(0);
    DataWriter^ writer = ref new DataWriter(outputStream);
    writer->WriteBytes(bytes);

    create_task(writer->StoreAsync()).then([writer](unsigned int)
    {
        create_task(writer->FlushAsync());
    });

    return randomAccessStream;
}
```

## <a name="replace-the-default-map"></a>取代預設地圖

完全以協力廠商磚或自訂磚取代預設地圖：

-   指定 [**MapTileLayer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileLayer).**BackgroundReplacement** 做為 [**MapTileSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilesource.layer) 的 [**Layer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource) 屬性值。
-   指定 [**MapStyle**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapStyle).**None** 做為 [**MapControl**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.style) 的 [**Style**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 屬性值。

## <a name="related-topics"></a>相關主題

* [Bing 地圖服務開發人員中心](https://www.bingmapsportal.com/)
* [UWP 地圖範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [地圖的設計指導方針](https://docs.microsoft.com/windows/uwp/maps-and-location/controls-map)
* [組建2015影片：在您的 Windows 應用程式中跨電話、平板電腦和 PC 運用地圖和位置](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP 車流量應用程式範例](https://github.com/Microsoft/Windows-appsample-trafficapp)
