---
title: xDeferLoadStrategy 屬性
description: xDeferLoadStrategy 會延遲建立元素及其子系而縮短啟動時間，但記憶體使用量會略為增加。 每個受影響的元素會增加約 600 個位元組的記憶體使用量。
ms.assetid: E763898E-13FF-4412-B502-B54DBFE2D4E4
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f445b186c42095bfdf6d10fa7960b78691ced792
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371097"
---
# <a name="xdeferloadstrategy-attribute"></a>x:DeferLoadStrategy 屬性

> [!IMPORTANT]
> 從 Windows 10，版本 1703 (Creators Update) 開始，**x:DeferLoadStrategy** 已由 [**x:Load 屬性**](x-load-attribute.md) 取代。 使用 `x:Load="False"` 與使用 `x:DeferLoadStrategy="Lazy"` 具有相同的效果，但在需要的時候可以提供您取消載入 UI 的能力。 請參閱 [x:Load 屬性](x-load-attribute.md)。

您可以使用 **x:DeferLoadStrategy="Lazy"** 來為您的 XAML 應用程式最佳化啟動或建立樹狀結構的效能。 當您使用 **x:DeferLoadStrategy="Lazy"** 時，元素及其子元素的建立將會延後，進而達到減少啟動時間和記憶體消耗量的效果。 這在降低不常顯示或只在特定條件下需要顯示之元素的成本時很有用。 當從程式碼或 VisualStateManager 參考元素時，元素將會具現化。

然而，XAML 架構將會針對每個受影響的元素增加大約 600 位元組的記憶體使用量，作為追蹤延遲元素使用。 延遲的元素樹狀結構愈大，節省的啟動時間就愈多，但代價是記憶體使用量較大。 因此，過度使用此屬性可能導致效能降低。

## <a name="xaml-attribute-usage"></a>XAML 屬性用法

``` syntax
<object x:DeferLoadStrategy="Lazy" .../>
```

## <a name="remarks"></a>備註

**x:DeferLoadStrategy** 的使用限制如下：

- 您必須定義[X:name](x-name-attribute.md) 讓項目，與必須設法稍後尋找項目。
- 您只能延遲 [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) 或 [**FlyoutBase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase) 衍生的類型。
- 您無法延遲 [**Page**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page)、[**UserControls**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol) 或 [**DataTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate) 中的根元素。
- 您無法延遲 [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) 中的元素。
- 您無法延遲隨 [**XamlReader.Load**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.xamlreader.load) 載入的鬆散 XAML。
- 移動父元素將會清除任何尚未辨識的元素。

有幾種不同的方式可用來辨識延遲的元素：

- 使用您在元素上定義的名稱呼叫 [**FindName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.findname)。
- 使用您在元素上定義的名稱呼叫 [**GetTemplateChild**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.gettemplatechild)。
- 在 [**VisualState**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState) 中，使用以延遲的元素為目標的 [**Setter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Setter) 或 **Storyboard** 動畫。
- 在 **Storyboard** 中以任何延遲的元素做為目標。
- 使用以延遲的元素為目標的繫結。

> 注意：一旦啟動項目的具現化，則會建立在 UI 執行緒，因此可能會導致多如果太多一次建立 UI。

以任何前述方法建立延遲的元素後，將會發生幾件事：

- 引發元素的 [**Loaded**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loaded) 事件。
- 元素的任何繫結會受到評估。
- 如果您已登錄而會接收包含延遲元素之屬性的屬性變更通知，通知將會引發。

您可以內嵌延遲的元素，但必須從最外層元素開始辨識這些元素。  如果您嘗試在辨識父元素之前辨識子元素，將會引發例外狀況。

一般而言，我們建議您延後不會出現在第一個畫面中的元素。 在尋找要延遲的候選項目時，建議您尋找以摺疊的 [**Visibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) 建立的元素。 此外，使用者互動所觸發的 UI 也是您尋找延遲元素的理想之處。

請留意 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 中的延遲元素，因為它雖然可減少您的啟動時間，但也可能隨著您所建立的項目而導致移動瀏覽效能下降。 如果您想要提升移動瀏覽效能，請參閱 [{x:Bind} 標記延伸](x-bind-markup-extension.md)和 [x:Phase 屬性](x-phase-attribute.md)文件。

如果 [x:Phase 屬性](x-phase-attribute.md)結合 **x:DeferLoadStrategy** 使用，則當元素或元素樹狀結構實現之後，繫結會向上套用且包含目前階段。 針對 **x:Phase** 指定的階段不會影響或控制元素的延遲。 當清單項目被視為移動瀏覽的一部分回收時，實現的元素會與其他作用中元素的行為相同，而已編譯的繫結 ( **{x:Bind}** 繫結) 會使用相同的規則處理 (包括階段處理)。

一般會建議您在之前或之後測量您應用程式的執行效能，以確保您得到想要的效能。

## <a name="example"></a>範例

```xml
<Grid x:Name="DeferredGrid" x:DeferLoadStrategy="Lazy">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition Width="Auto" />
    </Grid.ColumnDefinitions>

    <Rectangle Height="100" Width="100" Fill="#F65314" Margin="0,0,4,4" />
    <Rectangle Height="100" Width="100" Fill="#7CBB00" Grid.Column="1" Margin="4,0,0,4" />
    <Rectangle Height="100" Width="100" Fill="#00A1F1" Grid.Row="1" Margin="0,4,4,0" />
    <Rectangle Height="100" Width="100" Fill="#FFBB00" Grid.Row="1" Grid.Column="1" Margin="4,4,0,0" />
</Grid>
<Button x:Name="RealizeElements" Content="Realize Elements" Click="RealizeElements_Click"/>
```

```csharp
private void RealizeElements_Click(object sender, RoutedEventArgs e)
{
    // This will realize the deferred grid.
    this.FindName("DeferredGrid");
}
```
