---
Description: 使用 ListView 或 GridView 控制項來顯示和操縱一組資料，例如影像圖庫或一組電子郵件訊息。
title: 清單檢視和方格檢視
label: List view and grid view
template: detail.hbs
ms.date: 11/04/2019
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f8532ba0-5510-4686-9fcf-87fd7c643e7b
pm-contact: predavid
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: c130505ec79ca83698fd79df26464969afe79c36
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "80893468"
---
# <a name="list-view-and-grid-view"></a>清單檢視和方格檢視

大部分應用程式都會操縱和顯示資料組 (例如影像圖庫) 或一組電子郵件訊息。 XAML UI 架構提供 ListView 和 GridView 控制項，讓您輕鬆顯示和操縱應用程式中的資料。  

> **重要 API**：[ListView 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) \(英文\)、[GridView 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview) \(英文\)、[ItemsSource 屬性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) \(英文\)、[Items 屬性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) \(英文\)

> [!NOTE]
> ListView 和 GridView 都是衍生自 [ListViewBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase) 類別，因此它們具有相同功能，但會以不同方式顯示資料。 在本文中，當我們討論清單檢視時，除非另外指定，否則該資訊同時適用於 ListView 和 GridView 控制項。 我們可能會參考像是 ListView 或 ListViewItem 等類別，但對於對應的方格對等項目 (GridView 或 GridViewItem)，可使用 *Grid* 來取代 *List* 首碼。 

ListView 和 GridView 提供許多處理集合的優點。 它們很容易實作，且提供基本 UI、互動性和捲動功能，而且可輕易自訂。 ListView 和 GridView 可繫結至現有的動態資料來源，或是 XAML 本身/程式碼後置中提供的硬式編碼資料。 

這兩個控制項在許多使用案例中都具有操作彈性，但在用於所有項目都應具有相同的基本結構和外觀，以及相同的互動行為 (也就是在按下時都應執行相同的動作，例如開啟連結、導覽等等) 的集合時，整體效果最好。


## <a name="differences-between-listview-and-gridview"></a>ListView 與 GridView 之間的差異

### <a name="listview"></a>ListView
ListView 會在單一欄中以垂直堆疊的方式顯示資料。 ListView 較適用於以文字為焦點的項目，以及應從上往下讀取 (也就是依字母順序排列) 的集合。 ListView 的一些常見使用案例包括訊息清單和搜尋結果清單。 需要以多個資料欄或類似資料表格式顯示的集合_不_應該使用 ListView，但應該改用 [DataGrid](https://docs.microsoft.com/windows/communitytoolkit/controls/datagrid) 查看。

![具有分組資料的清單檢視](images/listview-grouped-example-resized-final.png)

### <a name="gridview"></a>GridView
GridView 會在可垂直捲動的列和欄中顯示項目集合。 資料會以水平方向進行堆疊，直到它填滿該欄為止，然後繼續進行下一列。 GridView 較適用於以影像為焦點的項目，以及可並列檢視或未依特定順序排序的集合。 GridView 的常見使用案例為相片或產品資源庫。

![內容庫範例](images/gridview-simple-example-final.png)

## <a name="which-collection-control-should-you-use-a-comparison-with-itemsrepeater"></a>您應使用哪個集合控制項？ 與 ItemsRepeater 的比較

ListView 和 GridView 是立即可用的控制項，可用來顯示任何集合，且具有本身的內建 UI 和 UX。 [ItemsRepeater](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/items-repeater) 控制項也可用來顯示集合，但最初的設計宗旨是要作為建置組塊，用以建立符合個人確切 UI 需求的自訂控制項。 影響您最終使用的控制項最重要的差異應是：

-   ListView 和 GridView 是功能豐富的控制項，需要進行的自訂極少，但卻提供齊備的功能。 ItemsRepeater 是供您自行建立版面配置控制項的建置組塊，並沒有相同的內建特性和功能 - 您必須實作任何必要的功能或互動。
-   如果您具有無法使用 ListView 或 GridView 來建立的高度自訂 UI，或是您的資料來源針對每個項目需要高度不同的行為，則應使用 ItemsRepeater。


若要深入了解 ItemsRepeater，請閱讀其[指引](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/items-repeater)和 [API 文件](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2)頁面。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡以開啟應用程式並查看 <a href="xamlcontrolsgallery:/item/ListView">ListView</a> 或 <a href="xamlcontrolsgallery:/item/GridView">GridView</a> 的運作情形。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-listview-or-gridview"></a>建立 ListView 或 GridView

ListView 和 GridView 皆屬於 [ItemsControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol) 類型，因此可包含任何類型的項目集合。 ListView 或 GridView 的 [Items](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) 集合中必須要有項目，才能在畫面上顯示任何內容。 若要填入檢視，您可以直接將項目新增至 [Items](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) 集合，或將 [ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 屬性設為資料來源。 

> [!IMPORTANT]
> 您可以使用 Items 或 ItemsSource 來填入清單，但無法同時使用這兩者。 如果您設定 ItemsSource 屬性並在 XAML 中新增項目，新增的項目將會被略過。 如果您設定 ItemsSource 屬性並將項目新增到程式碼的 Items 集合，則會擲出例外狀況。

為了方便說明，本文的許多範例都會直接填入 `Items` 集合。 不過，清單中較常見的項目是來自動態來源，例如，來自線上資料庫的書籍清單。 您可以使用 `ItemsSource` 屬性來達到此目的。 

### <a name="add-items-to-a-listview-or-gridview"></a>將項目新增至 ListView 或 GridView

您可以使用 XAML 將項目新增至 ListView 或 GridView 的 [Items](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) 集合，或使用程式碼產生相同的結果。 如果您只有少量不會變更且很容易定義的項目，或是如果您在執行階段於程式碼中產生項目，則通常會透過 XAML 新增項目。 

<u>方法 1：將新增項目至 Items 集合</u>
#### <a name="option-1-add-items-through-xaml"></a>選項 1：透過 XAML 新增項目
```xml
<!-- No corresponding C# code is needed for this example. -->

<ListView x:Name="Fruits"> 
   <x:String>Apricot</x:String> 
   <x:String>Banana</x:String> 
   <x:String>Cherry</x:String> 
   <x:String>Orange</x:String> 
   <x:String>Strawberry</x:String> 
</ListView>  
```


#### <a name="option-2-add-items-through-c"></a>選項 2：透過 C# 新增項目

##### <a name="c-code"></a>C# 程式碼：
```csharp
// Create a new ListView and add content. 
ListView Fruits = new ListView(); 
Fruits.Items.Add("Apricot"); 
Fruits.Items.Add("Banana"); 
Fruits.Items.Add("Cherry"); 
Fruits.Items.Add("Orange"); 
Fruits.Items.Add("Strawberry");
 
// Add the ListView to a parent container in the visual tree (that you created in the corresponding XAML file).
FruitsPanel.Children.Add(Fruits); 
```

##### <a name="corresponding-xaml-code"></a>對應的 XAML 程式碼：
```xml
<StackPanel Name="FruitsPanel"></StackPanel>
```
上述兩個選項會產生相同的 ListView，如下所示：

![簡單的清單檢視](images/listview-basic-code-example2.png)
<br/>
<u>方法 2：藉由設定 ItemsSource 來新增項目</u>

您通常會使用 ListView 或 GridView 顯示來自資料庫或網際網路等來源的資料。 若要從資料來源填入 ListView/GridView，您應將其 [ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 屬性設定為資料項目的集合。 如果您的 ListView 或 GridView 將會存放自訂類別物件，此方法會有更好的效用，如下列範例所示。

#### <a name="option-1-set-itemssource-in-c"></a>選項 1：在 C# 中設定 ItemsSource
在這裡，清單檢視的 ItemsSource 會在程式碼中直接設定為集合的執行個體。 

##### <a name="c-code"></a>C# 程式碼：
```csharp 
// Class defintion should be provided within the namespace being used, outside of any other classes.

this.InitializeComponent();

// Instead of adding hard coded items to an ObservableCollection as shown below, 
//the data could be pulled asynchronously from a database or the internet.
ObservableCollection<Contact> Contacts = new ObservableCollection<Contact>();

// Contact objects are created by providing a first name, last name, and company for the Contact constructor.
// They are then added to the ObservableCollection Contacts.
Contacts.Add(new Contact("John", "Doe", "ABC Printers"));
Contacts.Add(new Contact("Jane", "Doe", "XYZ Refridgerators"));
Contacts.Add(new Contact("Santa", "Claus", "North Pole Toy Factory Inc."));

// Create a new ListView (or GridView) for the UI, add content by setting ItemsSource
ListView ContactsLV = new ListView();
ContactsLV.ItemsSource = Contacts;

// Add the ListView to a parent container in the visual tree (that you created in the corresponding XAML file)
ContactPanel.Children.Add(ContactsLV);
```

##### <a name="xaml-code"></a>XAML 程式碼：
```xml
<StackPanel x:Name="ContactPanel"></StackPanel>
```

#### <a name="option-2-set-itemssource-in-xaml"></a>選項 2：在 XAML 中設定 ItemsSource
您也可以將 ItemsSource 屬性繫結至 XAML 中的集合。 在此，ItemsSource 會繫結至名為 `Contacts` 的公用屬性，此屬性可公開頁面的私用資料集合 `_contacts`。

**XAML**
```xml
<ListView x:Name="ContactsLV" ItemsSource="{x:Bind Contacts}"/>
```

**C#**
```csharp
// Class defintion should be provided within the namespace being used, outside of any other classes.
// These two declarations belong outside of the main page class.
private ObservableCollection<Contact> _contacts = new ObservableCollection<Contact>();

public ObservableCollection<Contact> Contacts
{
    get { return this._contacts; }
}

// This method should be defined within your main page class.
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    // Instead of hard coded items, the data could be pulled 
    // asynchronously from a database or the internet.
    Contacts.Add(new Contact("John", "Doe", "ABC Printers"));
    Contacts.Add(new Contact("Jane", "Doe", "XYZ Refridgerators"));
    Contacts.Add(new Contact("Santa", "Claus", "North Pole Toy Factory Inc."));
}
```

上述兩個選項會產生相同的 ListView，如下所示。 ListView 只會以字串形式顯示每個項目，因為我們並未提供資料範本。

![設定了 ItemsSource 的簡單清單檢視](images/listview-basic-code-example-final.png)

> [!IMPORTANT]
> 在未定義資料範本的情況下，自訂類別物件將只會以其字串值形式出現在 ListView 中 (如果它們具有已定義的 [ToString()](https://docs.microsoft.com/uwp/api/windows.foundation.istringable.tostring) 方法)。

 下一節將詳細說明如何在 ListView 或 GridView 中正確呈現簡單和自訂類別項目。

如需資料繫結的詳細資訊，請參閱[資料繫結概觀](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-quickstart)。

> [!NOTE]
> 如果您需要在 ListView 中顯示分組資料，就必須繫結至 [CollectionViewSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource)。 CollectionViewSource 會做為 XAML 中集合類別的 Proxy，並啟用貨幣和群組支援。 如需詳細資訊，請參閱 [CollectionViewSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource)。

## <a name="customizing-the-look-of-items-with-a-datatemplate"></a>使用 DataTemplate 自訂項目的外觀

ListView 或 GridView 中的資料範本會定義項目/資料的視覺化方式。 根據預設，資料項目會在 ListView 中以字串形式顯示所繫結的資料物件。 您可以將 [DisplayMemberPath](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.displaymemberpath) 設定為該屬性，以顯示資料項目之特定屬性的字串表示法。

但是，您通常會想要以更多樣化的表示方式來顯示資料。 為了明確指定項目在 ListView/GridView 中的顯示方式，您必須建立 [DataTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate)。 在 DataTemplate 中的 XAML 會定義用來顯示個別項目之控制項的配置和外觀。 配置中的控制項可以繫結至資料物件的屬性，或以內嵌方式定義靜態內容。 

> [!NOTE]
> 當您在 DataTemplate中使用 [x:Bind markup extension](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) 時，必須在 DataTemplate 上指定 DataType (`x:DataType`)。

#### <a name="simple-listview-data-template"></a>簡單的 ListView 資料範本
在此範例中，資料項目是一個簡單字串。 DataTemplate 會以內嵌方式定義於 ListView 定義內，以將影像新增至字串左側，並以藍綠色顯示字串。 這是使用前述的方法 1 和選項 1 建立的相同 ListView。

**XAML**
```XML
<!--No corresponding C# code is needed for this example.-->
<ListView x:Name="FruitsList">
                <ListView.ItemTemplate>
                    <DataTemplate x:DataType="x:String">
                        <Grid>
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="47"/>
                                <ColumnDefinition/>
                            </Grid.ColumnDefinitions>
                            <Image Source="Assets/placeholder.png" Width="32" Height="32"
                                HorizontalAlignment="Left" VerticalAlignment="Center"/>
                            <TextBlock Text="{x:Bind}" Foreground="Teal" FontSize="14" 
                                Grid.Column="1" VerticalAlignment="Center"/>
                        </Grid>
                    </DataTemplate>
                </ListView.ItemTemplate>
                <x:String>Apricot</x:String>
                <x:String>Banana</x:String>
                <x:String>Cherry</x:String>
                <x:String>Orange</x:String>
                <x:String>Strawberry</x:String>
            </ListView>

```

以下是使用此資料範本在 ListView 中顯示資料項目的情形：

![使用資料範本的 ListView 項目](images/listview-w-datatemplate1-final.png)

#### <a name="listview-data-template-for-custom-class-objects"></a>自訂類別物件的 ListView 資料範本
在此範例中，資料項目是一個 Contact 物件。 DataTemplate 會以內嵌方式定義於 ListView 定義內，以將連絡人影像新增至連絡人名稱和公司左側。 此 ListView 是使用前述的方法 2 和選項 2 建立的。
```xml
<ListView x:Name="ContactsLV" ItemsSource="{x:Bind Contacts}">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Contact">
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="*"/>
                    <RowDefinition Height="*"/>
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto"/>
                    <ColumnDefinition Width="*"/>
                </Grid.ColumnDefinitions>
                <Image Grid.Column="0" Grid.RowSpan="2" Source="Assets/grey-placeholder.png" Width="32"
                    Height="32" HorizontalAlignment="Center" VerticalAlignment="Center"></Image>
                <TextBlock Grid.Column="1" Text="{x:Bind Name}" Margin="12,6,0,0" 
                    Style="{ThemeResource BaseTextBlockStyle}"/>
                <TextBlock  Grid.Column="1" Grid.Row="1" Text="{x:Bind Company}" Margin="12,0,0,6" 
                    Style="{ThemeResource BodyTextBlockStyle}"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

以下是使用此資料範本在 ListView 中顯示資料項目的情形：

![使用資料範本的 ListView 自訂類別項目](images/listview-customclass-datatemplate-final.png)

資料範本是您定義 ListView 外觀的主要方式。 如果您的清單含有大量項目，這些範本可能也會對效能產生顯著的影響。  

您的資料範本可以內嵌方式定義於 ListView/GridView 定義內 (如上所示)，或個別定義於 [資源] 區段中。 如果定義於 ListView/GridView 本身以外，則必須為 DataTemplate 指定 [x:Key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute) 屬性，並將其指派給使用該索引鍵的 ListView 或 GridView 的 [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) 屬性。

如需詳細資訊，以及如何使用資料範本和項目容器來在清單或方格中定義項目外觀的範例，請參閱[項目容器與範本](item-containers-templates.md)。 

## <a name="change-the-layout-of-items"></a>變更項目的配置

當您將項目新增至 ListView 或 GridView 時，控制項會在項目容器中將每個項目自動換行，接著配置所有項目容器。 這些項目容器的配置方式取決於控制項的 [ItemsPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel)。  
- 根據預設，**ListView** 會使用 [ItemsStackPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemsstackpanel) \(英文\)，其會產生垂直清單，如下所示。

![簡單的清單檢視](images/listview-simple.png)

- **GridView** 會使用 [ItemsWrapGrid](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemswrapgrid) \(英文\)，其會以水平方向新增項目，然後以垂直方向進行換行和捲動，如下所示。

![簡單的方格檢視](images/gridview-simple.png)

您可以藉由調整項目面板上的屬性來修改項目配置，或者可以使用另一個面板來取代預設面板。

> [!NOTE]
> 如果您變更 ItemsPanel，切記不可停用虛擬化。 **ItemsStackPanel** 和 **ItemsWrapGrid** 都支援虛擬化，因此可放心使用這兩者。 如果您使用任何其他面板，您可能會停用虛擬化，並拖慢清單檢視的效能。 如需詳細資訊，請參閱[效能](https://docs.microsoft.com/windows/uwp/debug-test-perf/performance-and-xaml-ui)下方的清單檢視文章。 

這個範例示範如何藉由變更 **ItemsStackPanel** 的 [Orientation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemsstackpanel.orientation) \(英文\) 屬性，讓 **ListView** 能夠在水平清單中列出其項目容器。
因為清單檢視預設是垂直捲動，所以您也需要在清單檢視的內部 [ScrollViewer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer) 上調整一些屬性，讓它能夠水平捲動。
- 將 [ScrollViewer.HorizontalScrollMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode) 設為 **Enabled** 或 **Auto**
- 將 [ScrollViewer.HorizontalScrollBarVisibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility) 設為 **Auto** 
- 將 [ScrollViewer.VerticalScrollMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollmode) 設為 **Disabled** 
- 將 [ScrollViewer.VerticalScrollBarVisibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility) 設為 **Hidden** 

> [!IMPORTANT]
> 這些範例所顯示的清單檢視寬度不受限制，因此不會顯示水平捲軸。 如果您執行這個程式碼，可在 ListView 上設定 `Width="180"` 來顯示捲軸。

**XAML**
```xml
<ListView Height="60" 
          ScrollViewer.HorizontalScrollMode="Enabled" 
          ScrollViewer.HorizontalScrollBarVisibility="Auto"
          ScrollViewer.VerticalScrollMode="Disabled"
          ScrollViewer.VerticalScrollBarVisibility="Hidden">
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsStackPanel Orientation="Horizontal"/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
    <x:String>Apricot</x:String>
    <x:String>Banana</x:String>
    <x:String>Cherry</x:String>
    <x:String>Orange</x:String>
    <x:String>Strawberry</x:String>
</ListView>
```

產生的清單外觀如下。

![水平清單檢視](images/listview-horizontal2-final.png)

 在下一個範例中，**ListView** 會使用 **ItemsWrapGrid** 而不是 **ItemsStackPanel**，在垂直換行清單中配置項目。 
 
> [!IMPORTANT]
> 清單檢視的高度必須受到限制，才能強制控制項在容器中換行。

**XAML**
```xml
<ListView Height="100"
          ScrollViewer.HorizontalScrollMode="Enabled" 
          ScrollViewer.HorizontalScrollBarVisibility="Auto"
          ScrollViewer.VerticalScrollMode="Disabled"
          ScrollViewer.VerticalScrollBarVisibility="Hidden">
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
    <x:String>Apricot</x:String>
    <x:String>Banana</x:String>
    <x:String>Cherry</x:String>
    <x:String>Orange</x:String>
    <x:String>Strawberry</x:String>
</ListView>
```

產生的清單外觀如下。

![使用方格配置的清單檢視](images/listview-itemswrapgrid2-final.png)

如果您在清單檢視中顯示分組資料，ItemsPanel 會決定配置項目群組的方式，而不是配置個別項目的方式。例如，如果使用先前所示的水平 ItemsStackPanel 來顯示分組資料，群組會呈現水平排列，但是每個群組中的項目仍然會以垂直方向進行堆疊，如下所示。

![分組的水平清單檢視](images/listview-horizontal-groups.png)

## <a name="item-selection-and-interaction"></a>項目選取與互動

您可以從各種方式進行選擇，讓使用者可以與清單檢視互動。 根據預設，使用者可以選取單一項目。 您可以變更 [SelectionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode) 屬性，以啟用多重選取或停用選取。 您可以設定 [IsItemClickEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled) 屬性，讓使用者可按一下項目來叫用動作 (例如按鈕)，而不需選取項目。

> **注意**&nbsp;&nbsp;ListView 和 GridView 都會針對其 SelectionMode 屬性使用 [ListViewSelectionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewselectionmode) \(英文\) 列舉。 根據預設，IsItemClickEnabled 是 **False**，因此您只能設定它來啟用按一下模式。

下表顯示使用者可以與清單檢視互動的方式，以及您可以回應互動的方式。

若要啟用此互動︰ | 使用這些設定： | 處理這個事件︰ | 使用此屬性來取得選取的項目：
----------------------------|---------------------|--------------------|--------------------------------------------
沒有互動 | [SelectionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode) = **None**，[IsItemClickEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled) = **False** | 不適用 | 不適用 
單一選取 | SelectionMode = **Single**，IsItemClickEnabled = **False** | [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) \(英文\) | [SelectedItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem)，[SelectedIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex)  
多重選取 | SelectionMode = **Multiple**，IsItemClickEnabled = **False** | [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) \(英文\) | [SelectedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selecteditems) \(英文\)  
延伸選取 | SelectionMode = **Extended**，IsItemClickEnabled = **False** | [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) \(英文\) | [SelectedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selecteditems) \(英文\)  
按一下 | SelectionMode = **None**，IsItemClickEnabled = **True** | [ItemClick](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.itemclick) \(英文\) | 不適用 

> **注意**&nbsp;&nbsp;從 Windows 10 開始，您可以在同時將 SelectionMode 設定為 Single、Multiple 或 Extended 的情況下，啟用 IsItemClickEnabled 來引發 ItemClick 事件。 如果您執行此動作，就會先引發 ItemClick 事件，接著引發 SelectionChanged 事件。 在某些情況下，假設您瀏覽到 ItemClick 事件處理常式的另一個頁面，就不會引發 SelectionChanged 事件且不會選取該項目。

您可以在 XAML 或程式碼中設定這些屬性，如下所示。

**XAML**
```xaml
<ListView x:Name="myListView" SelectionMode="Multiple"/>

<GridView x:Name="myGridView" SelectionMode="None" IsItemClickEnabled="True"/> 
```

**C#**
```csharp
myListView.SelectionMode = ListViewSelectionMode.Multiple; 

myGridView.SelectionMode = ListViewSelectionMode.None;
myGridView.IsItemClickEnabled = true;
```

### <a name="read-only"></a>唯讀

您可以將 SelectionMode 屬性設為 **ListViewSelectionMode.None** 來停用項目選取。 這會將控制項放置於唯讀模式中，以用來顯示資料，但不會與它互動。 控制項本身並未停用，只會停用選取的項目。

### <a name="single-selection"></a>單一選取

下表說明當 SelectionMode 為 **Single** 時的鍵盤、滑鼠及觸控互動。

輔助按鍵 | 互動
-------------|------------
無 | <li>使用者可以使用空格鍵、按一下滑鼠或觸控點選來選取單一項目。</li>
CTRL | <li>使用者可以使用空格鍵、按一下滑鼠或觸控點選來取消選取單一項目。</li><li>使用者可以使用方向鍵來移動各自獨立的選取焦點。</li>

當 SelectionMode 是 **Single** 時，您可以從 [SelectedItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) \(英文\) 屬性取得選取的資料項目。 您可以使用 [SelectedIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex) 屬性來取得選取項目集合中的索引。 如果未選取任何項目，SelectedItem 即為 **null** 且 SelectedIndex 是 -1。 
 
如果您嘗試將不在 **Items** 集合中的項目設定為 **SelectedItem**，則此作業會遭到忽略，而且 SelectedItem 為**null**。 不過，如果您嘗試將 **SelectedIndex** 設為不在清單中 **Items** 範圍內的索引，即會發生 **System.ArgumentException** 例外狀況。 

### <a name="multiple-selection"></a>多重選取

下表說明當 SelectionMode 為 **Multiple** 時的鍵盤、滑鼠及觸控互動。

輔助按鍵 | 互動
-------------|------------
無 | <li>使用者可以使用空格鍵、按一下滑鼠或觸控點選來選取多個項目，以便在焦點項目上切換選取項目。</li><li>使用者可以使用方向鍵來移動各自獨立的選取焦點。</li>
Shift | <li>使用者可以選取多個連續項目，方法是按一下或點選選取範圍中的第一個項目，然後按一下或點選選取範圍中的最後一個項目。</li><li>使用者可以使用方向鍵來建立連續的選取範圍，選取範圍的第一個項目是按下 Shift 鍵時所選取的項目。</li>

### <a name="extended-selection"></a>延伸選取

下表說明當 SelectionMode 為 **Extended** 時的鍵盤、滑鼠及觸控互動。

輔助按鍵 | 互動
-------------|------------
無 | <li>此行為與**單一**選取相同。</li>
CTRL | <li>使用者可以使用空格鍵、按一下滑鼠或觸控點選來選取多個項目，以便在焦點項目上切換選取項目。</li><li>使用者可以使用方向鍵來移動各自獨立的選取焦點。</li>
Shift | <li>使用者可以選取多個連續項目，方法是按一下或點選選取範圍中的第一個項目，然後按一下或點選選取範圍中的最後一個項目。</li><li>使用者可以使用方向鍵來建立連續的選取範圍，選取範圍的第一個項目是按下 Shift 鍵時所選取的項目。</li>

當 SelectionMode 是 **Multiple** 或 **Extended** 時，您可以從 [SelectedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selecteditems) \(英文\) 屬性取得選取的資料項目。 

**SelectedIndex**、**SelectedItem** 及 **SelectedItems** 屬性會同步處理。 例如，如果您將 SelectedIndex 設為 -1，SelectedItem 就會設為 **null** 且 SelectedItems 是空的；如果您將 SelectedItem 設為 **null**，SelectedIndex 即會設為 -1 且 SelectedItems 是空的。

在多重選取模式中，**SelectedItem** 會包含第一個選取的項目，而 **Selectedindex** 會包含第一個選取的項目索引。 

### <a name="respond-to-selection-changes"></a>回應選取項目變更

若要回應清單檢視中的選取變更，請處理 [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) 事件。 在事件處理常式程式碼中，您可以從 [SelectionChangedEventArgs.AddedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems) 屬性取得所選項目的清單。 您可以取得已從 [SelectionChangedEventArgs.RemovedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.removeditems) 屬性中取消選取的任何項目。 除非使用者長按 Shift 鍵來選取某個範圍的項目，否則 AddedItems 和 RemovedItems 集合最多只會包含 1 個項目。

這個範例示範如何處理 **SelectionChanged** 事件，以及存取各種不同的項目集合。

**XAML**
```xml
<StackPanel HorizontalAlignment="Right">
    <ListView x:Name="listView1" SelectionMode="Multiple" 
              SelectionChanged="ListView1_SelectionChanged">
        <x:String>Apricot</x:String>
        <x:String>Banana</x:String>
        <x:String>Cherry</x:String>
        <x:String>Orange</x:String>
        <x:String>Strawberry</x:String>
    </ListView>
    <TextBlock x:Name="selectedItem"/>
    <TextBlock x:Name="selectedIndex"/>
    <TextBlock x:Name="selectedItemCount"/>
    <TextBlock x:Name="addedItems"/>
    <TextBlock x:Name="removedItems"/>
</StackPanel> 
```

**C#**
```csharp
private void ListView1_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (listView1.SelectedItem != null)
    {
        selectedItem.Text = 
            "Selected item: " + listView1.SelectedItem.ToString();
    }
    else
    {
        selectedItem.Text = 
            "Selected item: null";
    }
    selectedIndex.Text = 
        "Selected index: " + listView1.SelectedIndex.ToString();
    selectedItemCount.Text = 
        "Items selected: " + listView1.SelectedItems.Count.ToString();
    addedItems.Text = 
        "Added: " + e.AddedItems.Count.ToString();
    removedItems.Text = 
        "Removed: " + e.RemovedItems.Count.ToString();
}
```

### <a name="click-mode"></a>按一下模式

您可以變更清單檢視，讓使用者按一下項目像是在按一下按鈕，而不用選取項目。 例如，當您的使用者按一下清單或方格中的項目來使應用程式瀏覽到新頁面時，這會非常實用。 若要啟用此行為︰
- 將 **SelectionMode** 設為 **None**。
- 將 **IsItemClickEnabled** 設為 **true**。
- 處理 **ItemClick** 事件，在使用者按一下項目時執行一些動作。

以下是具有可點選項目的清單檢視。 ItemClick 事件處理常式中的程式碼會瀏覽到新頁面。

**XAML**
```xml
<ListView SelectionMode="None"
          IsItemClickEnabled="True" 
          ItemClick="ListView1_ItemClick">
    <x:String>Page 1</x:String>
    <x:String>Page 2</x:String>
    <x:String>Page 3</x:String>
    <x:String>Page 4</x:String>
    <x:String>Page 5</x:String>
</ListView>
```

**C#**
```csharp
private void ListView1_ItemClick(object sender, ItemClickEventArgs e)
{
    switch (e.ClickedItem.ToString())
    {
        case "Page 1":
            this.Frame.Navigate(typeof(Page1));
            break;

        case "Page 2":
            this.Frame.Navigate(typeof(Page2));
            break;

        case "Page 3":
            this.Frame.Navigate(typeof(Page3));
            break;

        case "Page 4":
            this.Frame.Navigate(typeof(Page4));
            break;

        case "Page 5":
            this.Frame.Navigate(typeof(Page5));
            break;

        default:
            break;
    }
}
```

### <a name="select-a-range-of-items-programmatically"></a>以程式設計的方式選取某個範圍的項目

您有時需要以程式設計方式操縱清單檢視的項目選取。 例如，您可能會提供 [全選]  按鈕，讓使用者選取清單中的所有項目。 在此情況下，從 SelectedItems 集合逐一新增和移除項目通常是不太有效率的。 每個項目變更都會導致 SelectionChanged 事件的發生，當您直接使用項目而不是使用索引值時，即會取消項目的虛擬化。

[SelectAll](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectall)、[SelectRange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectrange) 與 [DeselectRange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.deselectrange) 方法提供比使用 SelectedItems 屬性更有效率的方式來修改選取項目。 這些方法會使用項目索引的範圍來選取或取消選取。 由於只使用索引，因此，已虛擬化的項目仍會維持虛擬化狀態。 指定範圍中的所有項目都會選取 (或取消選取)，而無論其原始選取狀態為何。 針對這些方法的每一個呼叫，SelectionChanged 事件只會發生一次。

> [!IMPORTANT]
> 只有在 SelectionMode 屬性設為 Multiple 或 Extended 時，才能呼叫這些方法。 如果您在 SelectionMode 為 Single 或 None 時呼叫 SelectRange，即會擲回例外狀況。

當您使用索引範圍選取項目時，請使用 [SelectedRanges](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectedranges) 屬性來取得清單中的所有選取範圍。

如果 ItemsSource 實作 [IItemsRangeInfo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.iitemsrangeinfo) \(英文\)，且您使用這些方法來修改選取項目，系統將不會在 SelectionChangedEventArgs 中設定 **AddedItems** 和 **RemovedItems** 屬性。 設定這些屬性需要將項目物件取消虛擬化。 請改用 **SelectedRanges** 屬性來取得項目。

您可以呼叫 SelectAll 方法來選取集合中的所有項目。 不過，沒有對應的方法來取消選取所有項目。 您可以呼叫 DeselectRange 並傳遞 [FirstIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.itemindexrange.firstindex) 值為 0 和 [Length](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.itemindexrange.length) 值等於集合中項目數目的 [ItemIndexRange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.itemindexrange) 來取消所有項目。 這會連同選取所有項目的選項一起顯示在下列範例中。

**XAML**
```xml
<StackPanel Width="160">
    <Button Content="Select all" Click="SelectAllButton_Click"/>
    <Button Content="Deselect all" Click="DeselectAllButton_Click"/>
    <ListView x:Name="listView1" SelectionMode="Multiple">
        <x:String>Apricot</x:String>
        <x:String>Banana</x:String>
        <x:String>Cherry</x:String>
        <x:String>Orange</x:String>
        <x:String>Strawberry</x:String>
    </ListView>
</StackPanel>
```

**C#**
```csharp
private void SelectAllButton_Click(object sender, RoutedEventArgs e)
{
    if (listView1.SelectionMode == ListViewSelectionMode.Multiple ||
        listView1.SelectionMode == ListViewSelectionMode.Extended)
    {
        listView1.SelectAll();
    }
}

private void DeselectAllButton_Click(object sender, RoutedEventArgs e)
{
    if (listView1.SelectionMode == ListViewSelectionMode.Multiple ||
        listView1.SelectionMode == ListViewSelectionMode.Extended)
    {
        listView1.DeselectRange(new ItemIndexRange(0, (uint)listView1.Items.Count));
    }
}
```

如需如何變更選取項目外觀的詳細資訊，請參閱[項目容器與範本](item-containers-templates.md)。

### <a name="drag-and-drop"></a>拖放

ListView 和 GridView 控制項支援在項目本身內部，以及在本身和其他 ListView 與 GridView 控制項之間進行拖放。 如需有關實作拖放模式的詳細資訊，請參閱[拖放](../input/drag-and-drop.md)。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML ListView 和 GridView 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView) \(英文\) - 示範 ListView 和 GridView 控制項。
- [XAML 拖放範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlDragAndDrop) \(英文\) - 示範搭配 ListView 控制項的拖放。
- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) (英文) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

- [清單](lists.md)
- [項目容器與範本](item-containers-templates.md)
- [拖放功能](../input/drag-and-drop.md)
