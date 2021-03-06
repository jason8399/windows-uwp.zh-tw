---
Description: 使用版面配置面板，在應用程式中排列與群組 UI 元素。
title: Windows 應用程式的版面配置面板
ms.date: 04/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e5a796f95efb418f7d70062ca2d73bbea7220d95
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234852"
---
# <a name="layout-panels"></a>版面配置面板

版面配置面板是允許您在應用程式中排列和群組 UI 元素的容器。 內建的 XAML 版面配置面板包含 [**RelativePanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RelativePanel)、[**StackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel)、[**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid)、[**VariableSizedWrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.VariableSizedWrapGrid) 及 [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas)。 我們將在此處說明每個面板，以及示範如何使用它來為 XAML UI 元素進行版面配置。

選擇版面配置面板時需要考量下列數個事項：
- 面板如何放置它的子元素。
- 面板如何調整它的子元素大小。
- 重疊的子元素如何彼此交疊 (圖層順序)。
- 建立所需版面配置之必要巢狀面板元素的個數與複雜性。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項資源庫</strong> 應用程式，請參閱 <a href="xamlcontrolsgallery:/item/RelativePanel">RelativePanel</a>、<a href="xamlcontrolsgallery:/item/StackPanel">StackPanel</a>、<a href="xamlcontrolsgallery:/item/Grid">Grid</a>、<a href="xamlcontrolsgallery:/item/VariableSizedWrapGrid">VariableSizedWrapGrid</a>、<a href="xamlcontrolsgallery:/item/Canvas">Canvas</a> 實際操作。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="panel-properties"></a>面板屬性

討論個別面板之前，先看看所有面板都會有的一些常見屬性。 

### <a name="panel-attached-properties"></a>面板附加屬性

大多數 XAML 版面配置面板都是使用附加屬性，以讓其子元素通知父面板應該如何將它們放置在 UI 中。 附加屬性使用的語法是 *AttachedPropertyProvider.PropertyName*。 如果您的面板會巢串於其他面板內，則 UI 元素上為父項指定版面配置特性的附加屬性只會由最接近的父項面板來解譯。

以下範例示範如何使用 XAML，在 Button 控制項上設定 [**Canvas.Left**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.left) 附加屬性。 這會通知父項 Canvas 應該將 Button 放置在從 Canvas 的左邊緣算起 50 個有效像素的位置。

```xaml
<Canvas>
  <Button Canvas.Left="50">Hello</Button>
</Canvas>
```

如需有關附加屬性的詳細資訊，請參閱[附加屬性概觀](../../xaml-platform/attached-properties-overview.md)。

### <a name="panel-borders"></a>面板框線

RelativePanel、StackPanel 及 Grid 面板會定義框線屬性，讓您能夠繪製面板周圍的框線，而不需在額外的 Border 元素中包裝它們。 這些框線屬性包含 **BorderBrush**、**BorderThickness**、**CornerRadius** 及 **Padding**。

以下是如何在 Grid 上設定框線屬性的範例。

```xaml
<Grid BorderBrush="Blue" BorderThickness="12" CornerRadius="12" Padding="12">
    <TextBlock Text="Hello World!"/>
</Grid>
```

![含有框線的方格](images/layout-panel-grid-border.png)

使用內建的框線屬性，可降低 XAML 元素計數，這樣可以提升應用程式的 UI 效能。 如需版面配置面板和 UI 效能的詳細資訊，請參閱[最佳化您的 XAML 版面配置](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-your-xaml-layout)。

## <a name="relativepanel"></a>RelativePanel

[**RelativePanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RelativePanel) 可讓您藉由為 UI 元素指定相對於其他元素和相對於面板的位置，為其進行版面配置。 元素預設會放置在面板的左上角。 您可以使用 RelativePanel 搭配 [**VisualStateManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualStateManager) 和 [**AdaptiveTrigger**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.AdaptiveTrigger)，針對不同的視窗大小重新排列 UI。

下表顯示的附加屬性可用來將元素與面板或其他元素對齊。

面板對齊方式 | 同層級對齊方式 | 同層級位置
----------------|-------------------|-----------------
[**AlignTopWithPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.aligntopwithpanelproperty) | [**AlignTopWith**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.aligntopwithproperty) | [**Above**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel)  
[**AlignBottomWithPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.alignbottomwithpanelproperty) | [**AlignBottomWith**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.alignbottomwithproperty) | [**Below**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.belowproperty)  
[**AlignLeftWithPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel) | [**AlignLeftWith**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.getalignleftwith) | [**LeftOf**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.leftofproperty)  
[**AlignRightWithPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.alignrightwithpanelproperty) | [**AlignRightWith**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.alignrightwithproperty) | [**RightOf**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.setrightof)  
[**AlignHorizontalCenterWithPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.alignhorizontalcenterwithpanelproperty) | [**AlignHorizontalCenterWith**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.alignhorizontalcenterwithproperty) | &nbsp;   
[**AlignVerticalCenterWithPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.alignverticalcenterwithpanelproperty) | [**AlignVerticalCenterWith**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.alignverticalcenterwithproperty) | &nbsp;   

 
這個 XAML 示範如何在 RelativePanel 中排列元素。

```xaml
<RelativePanel BorderBrush="Gray" BorderThickness="1">
    <Rectangle x:Name="RedRect" Fill="Red" Height="44" Width="44"/>
    <Rectangle x:Name="BlueRect" Fill="Blue"
               Height="44" Width="88"
               RelativePanel.RightOf="RedRect" />

    <Rectangle x:Name="GreenRect" Fill="Green" 
               Height="44"
               RelativePanel.Below="RedRect" 
               RelativePanel.AlignLeftWith="RedRect" 
               RelativePanel.AlignRightWith="BlueRect"/>
    <Rectangle Fill="Orange"
               RelativePanel.Below="GreenRect" 
               RelativePanel.AlignLeftWith="BlueRect" 
               RelativePanel.AlignRightWithPanel="True"
               RelativePanel.AlignBottomWithPanel="True"/>
</RelativePanel>
```

結果看起來就像這樣。 

![相對路徑](images/layout-panel-relative-panel.png)

以下是調整矩形大小時應注意的一些事項：
- 已針對紅色矩形指定明確的大小：44x44。 此矩形會放置於面板左上角，這是預設位置。
- 已針對綠色矩形指定明確的高度：44。 它的左側會與紅色矩形對齊，而其右側會與藍色矩形對齊，這會決定它的寬度。
- 並未針對橘色矩形指定明確的大小。 它的左側會與藍色矩形對齊。 它的右邊和底部邊緣會與面板的邊緣對齊。 它的大小取決於這些對齊方式，而它將會在面板調整大小時調整其大小。

## <a name="stackpanel"></a>StackPanel

[**StackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) 將其子元素按水平或垂直方向排列到單行中。 StackPanel 通常用來排列頁面上 UI 的小型子區段。

您可以使用 [**Orientation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.stackpanel.orientation) 屬性來指定子元素的方向。 預設方向是 [**Vertical**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Orientation)。

下列 XAML 示範如何建立項目的垂直 StackPanel。

```xaml
<StackPanel>
    <Rectangle Fill="Red" Height="44"/>
    <Rectangle Fill="Blue" Height="44"/>
    <Rectangle Fill="Green" Height="44"/>
    <Rectangle Fill="Orange" Height="44"/>
</StackPanel>
```


結果看起來就像這樣。

![堆疊面板](images/layout-panel-stack-panel.png)

在 StackPanel 中，如果沒有明確設定子元素的大小，它會向兩邊延伸以填滿可用的寬度 (或如果 Orientation 是 **Horizontal**，則為填滿可用的高度)。 在這個範例中，未設定矩形的寬度。 矩形會展開以填滿 StackPanel 的完整寬度。

## <a name="grid"></a>方格

[**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) 面板支援可變式版面配置並允許您在多列與多欄版面配置中排列控制項。 使用 [**RowDefinitions**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid.rowdefinitions) 和 [**ColumnDefinitions**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid.columndefinitions) 屬性指定 Grid 的列與欄。

使用 [**Grid.Column**](https://docs.microsoft.com/dotnet/api/system.windows.controls.grid.column) 和 [**Grid.Row**](https://docs.microsoft.com/dotnet/api/system.windows.controls.grid.row) 附加屬性，將物件放置在 Grid 的特定儲存格中。

使用 [**Grid.RowSpan**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms605035(v=vs.95)) 和 [**Grid.ColumnSpan**](https://docs.microsoft.com/dotnet/api/system.windows.controls.grid.columnspan) 附加屬性將內容延伸到多個列與欄。

這個 XAML 範例示範如何建立具有兩列兩欄的方格。

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition/>
        <RowDefinition Height="44"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <Rectangle Fill="Red" Width="44"/>
    <Rectangle Fill="Blue" Grid.Row="1"/>
    <Rectangle Fill="Green" Grid.Column="1"/>
    <Rectangle Fill="Orange" Grid.Row="1" Grid.Column="1"/>
</Grid>
```


結果看起來就像這樣。

![方格](images/layout-panel-grid.png)

在這個範例中，調整大小的運作方式如下： 
- 第二列具有明確的高度：44 個有效像素。 根據預設，第一列的高度會填滿所有遺留下來的空間。
- 第一欄的寬度會設定為 **Auto**，因此會是其子系所需的寬度。 在此案例中，寬度必須為 44 個有效像素，才能容納紅色矩形的寬度。
- 矩形上沒有任何其他的大小限制，因此，每一個都會向兩邊延伸以填滿其所在的方格儲存格。

您可以使用 **Auto** 或星號調整來分配欄內或列內的空間。 您可以使用自動調整大小，讓 UI 元素調整大小以符合它們的內容或父容器。 您也可以使用自動調整大小搭配方格的列與欄。 若要使用自動調整大小，請將 UI 元素的 Height 和/或 Width 設定為 **Auto**。

您使用等比例調整大小 (亦稱為「星號調整」)，按照權重比例，將可用的空間分配給方格的列和欄。 在 XAML 中，星號值的表示方法為 \* (加權星號調整則為 *n*\* )。 例如，若要在 2 欄的版面配置中，將某一欄的寬度設定為第二欄的 5 倍，請使用 "5\*" 與 "\*" 來表示 [**ColumnDefinition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ColumnDefinition) \(英文\) 元素中的 [**Width**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.columndefinition.width) \(英文\) 屬性。

這個範例會在具有 4 欄的 [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) 中結合固定、自動和等比例調整大小。

&nbsp;|&nbsp;|&nbsp;
------|------|------
Column_1 | **Auto** | 會調整欄的大小以容納其內容。
Column_2 | * | 計算 Auto 欄之後，這個欄會分配到一部分的剩餘寬度。 Column_2 會是 Column_4 的一半寬度。
Column_3 | **44** | 此欄寬度為 44 個像素。
Column_4 | **2**\* | 計算 Auto 欄之後，這個欄會分配到一部分的剩餘寬度。 Column_4 會是 Column_2 的兩倍寬度。

預設欄的寬度是 "*"，因此不需要為第二欄明確設定這個值。

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition/>
        <ColumnDefinition Width="44"/>
        <ColumnDefinition Width="2*"/>
    </Grid.ColumnDefinitions>
    <TextBlock Text="Column 1 sizes to its conent." FontSize="24"/>
</Grid>
```

在 Visual Studio XAML 設計工具中，結果看起來就像這樣。

![Visual Studio 設計工具中的 4 欄格線](images/xaml-layout-grid-in-designer.png)

## <a name="variablesizedwrapgrid"></a>VariableSizedWrapGrid

[**VariableSizedWrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.VariableSizedWrapGrid) 是方格樣式的版面配置面板，其中列或欄在達到 [**MaximumRowsOrColumns**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid.maximumrowsorcolumns) 值時會自動換行到新列或新欄。 

[  **Orientation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid.orientation) 屬性指定方格是否會在換行之前將項目新增到列或欄。 預設方向是 **Vertical**，這表示方格會從上到下新增項目，直到該欄滿了為止然後換行到新欄。 當值為 **Horizontal** 時，方格會從左到右新增項目，然後換行到新列。

儲存格尺寸是透過 [**ItemHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid.itemheight) 和 [**ItemWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid.itemwidth) 來指定。 每個儲存格都是相同的大小。 如果未指定 ItemHeight 或 ItemWidth，則第一個儲存格會調整大小以符合其內容，而其他的每一個儲存格都是第一個儲存格的大小。

您可以使用 [**VariableSizedWrapGrid.ColumnSpan**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid) 和 [**VariableSizedWrapGrid.RowSpan**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid.getrowspan) 附加屬性，來指定子元素應該填滿多少個相鄰的儲存格。

以下示範如何在 XAML 中使用 VariableSizedWrapGrid。

```xaml
<VariableSizedWrapGrid MaximumRowsOrColumns="3" ItemHeight="44" ItemWidth="44">
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" 
               VariableSizedWrapGrid.RowSpan="2"/>
    <Rectangle Fill="Green" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
    <Rectangle Fill="Orange" 
               VariableSizedWrapGrid.RowSpan="2" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
</VariableSizedWrapGrid>
```


結果看起來就像這樣。

![可變動的大小換行方格](images/layout-panel-variable-size-wrap-grid.png)

在這個範例中，每個欄中的列數上限為 3。 由於藍色矩形會橫跨 2 列，因此第一欄只會包含 2 個項目 (紅色和藍色矩形)。 綠色矩形接著會換行到下一欄的頂端。

## <a name="canvas"></a>畫布

[**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) 面板使用固定座標點定位子元素，且不支援可變式版面配置。 您可以在每個元素上設定 [**Canvas.Left**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.left) 和 [**Canvas.Top**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.top) 附加屬性，以指定個別子元素上的點。 在版面配置的 [Arrange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) 階段，父項 Canvas 會從其子系讀取這些附加屬性值。

Canvas 中的物件可以重疊，將某一個物件繪製於另一個物件上方。 根據預設，Canvas 會以宣告子物件的順序來呈現它們，因此最後一個子系會呈現在最上方 (每個元素的預設 z 索引為 0)。 這等同於其他內建的面板。 但是，Canvas 也支援 [**Canvas.ZIndex**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc190397(v=vs.95)) 附加屬性，您可以在每一個子元素上設定它們。 您可以在程式碼中設定這個屬性，在執行階段期間變更元素的繪製順序。 含有最高 Canvas.ZIndex 值的元素是最後繪製的，因此會繪製於共用相同空間或以任何方式重疊的任何其他元素上。 請注意，因為會採用 Alpha 值 (透明度)，所以如果最上層元素的 Alpha 值不是最大的，即使元素重疊，重疊區域顯示的內容也可能會混合。

Canvas 不會針對其子項進行任何調整大小的動作。 每個元素都必須指定其大小。

以下是 XAML 中的 Canvas 範例。

```xaml
<Canvas Width="120" Height="120">
    <Rectangle Fill="Red" Height="44" Width="44"/>
    <Rectangle Fill="Blue" Height="44" Width="44" Canvas.Left="20" Canvas.Top="20"/>
    <Rectangle Fill="Green" Height="44" Width="44" Canvas.Left="40" Canvas.Top="40"/>
    <Rectangle Fill="Orange" Height="44" Width="44" Canvas.Left="60" Canvas.Top="60"/>
</Canvas>
```

結果看起來就像這樣。

![畫布](images/layout-panel-canvas.png)

請謹慎使用 Canvas 面板。 雖然在某些情況下，能夠精確控制 UI 元素的位置是非常方便的，但是，固定位置的版面配置面板會導致 UI 區域較無法適應整體應用程式視窗大小變更。 當裝置方向變更、分割應用程式視窗、變更監視器，以及一些其他使用者案例，都可能需要調整應用程式視窗的大小。

## <a name="panels-for-itemscontrol"></a>ItemsControl 的面板

有數個具有特殊用途的面板，只能用來做為 [**ItemsPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel)，以顯示 [**ItemsControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) 中的項目。 這些是 [**ItemsStackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsStackPanel)、[**ItemsWrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid)、[**VirtualizingStackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.VirtualizingStackPanel) 及 [**WrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WrapGrid)。 您無法針對一般 UI 版面配置使用這些面板。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以互動式格式查看所有 XAML 控制項。
