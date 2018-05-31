---
author: mcleblanc
ms.assetid: A9D54DEC-CD1B-4043-ADE4-32CD4977D1BF
title: 資料繫結概觀
description: 本主題示範如何在通用 Windows 平台 (UWP) app 中將控制項 (或其他 UI 元素) 繫結到單一項目，或將項目的控制項繫結到項目集合。
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: fd9b9b4f1c6ddd48db8801d714568ccb011ea7e5
ms.sourcegitcommit: 4b522af988273946414a04fbbd1d7fde40f8ba5e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2018
ms.locfileid: "1494045"
---
<a name="data-binding-overview"></a>資料繫結概觀
=====================



本主題說明如何在通用 Windows 平台 (UWP) 應用程式中將控制項 (或其他 UI 元素) 繫結到單一項目，或將項目控制項繫結到項目集合。 此外，我們還會說明如何控制項目的呈現、根據選擇來實作詳細資料檢視、以及轉換資料以供顯示。 如需詳細資訊，請參閱[深入了解資料繫結](data-binding-in-depth.md)。

<a name="prerequisites"></a>先決條件
-------------------------------------------------------------------------------------------------------------

這個主題假設您知道如何建立基本的 UWP app。 如需建立第一個 UWP 應用程式的指示，請參閱 [Windows 應用程式入門](https://developer.microsoft.com/windows/getstarted)。

<a name="create-the-project"></a>建立專案
---------------------------------------------------------------------------------------------------------------------------------

建立新的 **\[空白應用程式 (Windows 通用)\]** 專案。 將它命名為「快速入門」。

<a name="binding-to-a-single-item"></a>繫結到單一項目
---------------------------------------------------------------------------------------------------------------------------------------------------------

每個繫結是由繫結目標和繫結來源所組成。 通常，目標是控制項或其他 UI 元素的屬性，來源是類別執行個體 (資料模型或檢視模型) 的屬性。 這個範例示範如何將控制項繫結到單一項目。 目標是 **TextBlock** 的 **Text** 屬性。 來源是一個簡單類別 **Recording** 的執行個體，代表音訊錄製。 讓我們先看一下這個類別。

將新類別加入專案、將它命名為 Recording.cs (如果使用的是下面提供的 C#、C++ 程式碼片段)，然後在其中加入下列程式碼。

> [!div class="tabbedCodeSnippets"]
```csharp
namespace Quickstart
{
    public class Recording
    {
        public string ArtistName { get; set; }
        public string CompositionName { get; set; }
        public DateTime ReleaseDateTime { get; set; }
        public Recording()
        {
            this.ArtistName = "Wolfgang Amadeus Mozart";
            this.CompositionName = "Andante in C for Piano";
            this.ReleaseDateTime = new DateTime(1761, 1, 1);
        }
        public string OneLineSummary
        {
            get
            {
                return $"{this.CompositionName} by {this.ArtistName}, released: "
                    + this.ReleaseDateTime.ToString("d");
            }
        }
    }
    public class RecordingViewModel
    {
        private Recording defaultRecording = new Recording();
        public Recording DefaultRecording { get { return this.defaultRecording; } }
    }
}
```
```cpp
    #include <sstream>
    namespace Quickstart
    {
        public ref class Recording sealed
        {
        private:
            Platform::String^ artistName;
            Platform::String^ compositionName;
            Windows::Globalization::Calendar^ releaseDateTime;
        public:
            Recording(Platform::String^ artistName, Platform::String^ compositionName,
                Windows::Globalization::Calendar^ releaseDateTime) :
                artistName{ artistName },
                compositionName{ compositionName },
                releaseDateTime{ releaseDateTime } {}
            property Platform::String^ ArtistName
            {
                Platform::String^ get() { return this->artistName; }
            }
            property Platform::String^ CompositionName
            {
                Platform::String^ get() { return this->compositionName; }
            }
            property Windows::Globalization::Calendar^ ReleaseDateTime
            {
                Windows::Globalization::Calendar^ get() { return this->releaseDateTime; }
            }
            property Platform::String^ OneLineSummary
            {
                Platform::String^ get()
                {
                    std::wstringstream wstringstream;
                    wstringstream << this->CompositionName->Data();
                    wstringstream << L" by " << this->ArtistName->Data();
                    wstringstream << L", released: " << this->ReleaseDateTime->MonthAsNumericString()->Data();
                    wstringstream << L"/" << this->ReleaseDateTime->DayAsString()->Data();
                    wstringstream << L"/" << this->ReleaseDateTime->YearAsString()->Data();
                    return ref new Platform::String(wstringstream.str().c_str());
                }
            }
        };
        public ref class RecordingViewModel sealed
        {
        private:
            Recording^ defaultRecording;
        public:
            RecordingViewModel()
            {
                Windows::Globalization::Calendar^ releaseDateTime = ref new Windows::Globalization::Calendar();
                releaseDateTime->Month = 1;
                releaseDateTime->Day = 1;
                releaseDateTime->Year = 1761;
                this->defaultRecording = ref new Recording{ L"Wolfgang Amadeus Mozart", L"Andante in C for Piano", releaseDateTime };
            }
            property Recording^ DefaultRecording
            {
                Recording^ get() { return this->defaultRecording; };
            }
        };
    }
```

接著，從代表標記頁面的類別中公開繫結來源類別。 作法是將 **RecordingViewModel** 類型的屬性加入到 **MainPage**。

> [!div class="tabbedCodeSnippets"]
```csharp
    namespace Quickstart
    {
        public sealed partial class MainPage : Page
        {
            public MainPage()
            {
                this.InitializeComponent();
                this.ViewModel = new RecordingViewModel();
            }
            public RecordingViewModel ViewModel { get; set; }
        }
    }
```
```cpp
    namespace Quickstart
    {
        public ref class MainPage sealed
        {
        private:
            RecordingViewModel^ viewModel;
        public:
            MainPage()
            {
                InitializeComponent();
                this->viewModel = ref new RecordingViewModel();
            }
            property RecordingViewModel^ ViewModel
            {
                RecordingViewModel^ get() { return this->viewModel; };
            }
        };
    }
```

最後一步是將 **TextBlock** 繫結到 **ViewModel.DefaultRecording.OneLiner** 屬性。

```xml
    <Page x:Class="Quickstart.MainPage" ... >
        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <TextBlock Text="{x:Bind ViewModel.DefaultRecording.OneLineSummary}"
            HorizontalAlignment="Center"
            VerticalAlignment="Center"/>
        </Grid>
    </Page>
```

結果如下。

![繫結文字方塊](images/xaml-databinding0.png)

<a name="binding-to-a-collection-of-items"></a>繫結到項目集合
------------------------------------------------------------------------------------------------------------------

常見的一個情況是繫結到商業物件的集合。 在 C# 和 Visual Basic 中，[**ObservableCollection&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/xaml/ms668604.aspx) 泛型類別是適用於資料繫結的集合選擇，因為它實作 [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.componentmodel.inotifypropertychanged.aspx) 和 [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) 介面。 當加入或移除項目，或清單本身的屬性變更時，這些介面提供變更通知給繫結。 如果您希望繫結控制項隨著集合中物件屬性的變更一起更新，那麼商業物件也應該實作 **INotifyPropertyChanged**。 如需詳細資訊，請參閱[深入了解資料繫結](data-binding-in-depth.md)。

下一個範例將 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 繫結到 `Recording` 物件的集合。 首先讓我們將集合加入到檢視模型。 將這些新成員加入到 **RecordingViewModel** 類別。

> [!div class="tabbedCodeSnippets"]
```csharp
    public class RecordingViewModel
    {
        ...
        private ObservableCollection<Recording> recordings = new ObservableCollection<Recording>();
        public ObservableCollection<Recording> Recordings { get { return this.recordings; } }
        public RecordingViewModel()
        {
            this.recordings.Add(new Recording() { ArtistName = "Johann Sebastian Bach",
            CompositionName = "Mass in B minor", ReleaseDateTime = new DateTime(1748, 7, 8) });
            this.recordings.Add(new Recording() { ArtistName = "Ludwig van Beethoven",
            CompositionName = "Third Symphony", ReleaseDateTime = new DateTime(1805, 2, 11) });
            this.recordings.Add(new Recording() { ArtistName = "George Frideric Handel",
            CompositionName = "Serse", ReleaseDateTime = new DateTime(1737, 12, 3) });
        }
    }
```
```cpp
    public ref class RecordingViewModel sealed
    {
    private:
        ...
        Windows::Foundation::Collections::IVector<Recording^>^ recordings;
    public:
        RecordingViewModel()
        {
            ...
            releaseDateTime = ref new Windows::Globalization::Calendar();
            releaseDateTime->Month = 7;
            releaseDateTime->Day = 8;
            releaseDateTime->Year = 1748;
            Recording^ recording = ref new Recording{ L"Johann Sebastian Bach", L"Mass in B minor", releaseDateTime };
            this->Recordings->Append(recording);
            releaseDateTime = ref new Windows::Globalization::Calendar();
            releaseDateTime->Month = 2;
            releaseDateTime->Day = 11;
            releaseDateTime->Year = 1805;
            recording = ref new Recording{ L"Ludwig van Beethoven", L"Third Symphony", releaseDateTime };
            this->Recordings->Append(recording);
            releaseDateTime = ref new Windows::Globalization::Calendar();
            releaseDateTime->Month = 12;
            releaseDateTime->Day = 3;
            releaseDateTime->Year = 1737;
            recording = ref new Recording{ L"George Frideric Handel", L"Serse", releaseDateTime };
            this->Recordings->Append(recording);
        }
        ...
        property Windows::Foundation::Collections::IVector<Recording^>^ Recordings
        {
            Windows::Foundation::Collections::IVector<Recording^>^ get()
            {
                if (this->recordings == nullptr)
                {
                    this->recordings = ref new Platform::Collections::Vector<Recording^>();
                }
                return this->recordings;
            };
        }
    };
```

然後將 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 繫結到 **ViewModel.Recordings** 屬性。

```xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <ListView ItemsSource="{x:Bind ViewModel.Recordings}"
        HorizontalAlignment="Center" VerticalAlignment="Center"/>
    </Grid>
</Page>
```

我們還未提供資料範本給 **Recording** 類別，因此 UI 架構所能做的只是針對 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 中的每個項目，呼叫 [**ToString**](https://msdn.microsoft.com/library/windows/apps/system.object.tostring.aspx)。 **ToString** 的預設實作是傳回類型名稱。

![繫結清單檢視](images/xaml-databinding1.png)

若要解決這個問題，我們可以覆寫 [**ToString**](https://msdn.microsoft.com/library/windows/apps/system.object.tostring.aspx) 來傳回 **OneLineSummary** 的值，不然就是提供資料範本。 資料範本選項較常用，也可說是較有彈性。 您可以使用內容控制項的 [**ContentTemplate**](https://msdn.microsoft.com/library/windows/apps/BR209369) 屬性或項目控制項的 [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242830) 屬性來指定資料範本。 以下是為 **Recording** 設計資料範本的兩種方式，同時提供結果的插圖。

```xml
    <ListView ItemsSource="{x:Bind ViewModel.Recordings}"
        HorizontalAlignment="Center" VerticalAlignment="Center">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="local:Recording">
                <TextBlock Text="{x:Bind OneLineSummary}"/>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
```

![繫結清單檢視](images/xaml-databinding2.png)

```xml
    <ListView ItemsSource="{x:Bind ViewModel.Recordings}"
    HorizontalAlignment="Center" VerticalAlignment="Center">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="local:Recording">
                <StackPanel Orientation="Horizontal" Margin="6">
                    <SymbolIcon Symbol="Audio" Margin="0,0,12,0"/>
                    <StackPanel>
                        <TextBlock Text="{x:Bind ArtistName}" FontWeight="Bold"/>
                        <TextBlock Text="{x:Bind CompositionName}"/>
                    </StackPanel>
                </StackPanel>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
```

![繫結清單檢視](images/xaml-databinding3.png)

如需 XAML 語法的詳細資訊，請參閱[使用 XAML 建立 UI](https://msdn.microsoft.com/library/windows/apps/Mt228349)。 如需控制項配置的詳細資訊，請參閱[使用 XAML 定義配置](https://msdn.microsoft.com/library/windows/apps/Mt228350)。

<a name="adding-a-details-view"></a>新增詳細資料檢視
-----------------------------------------------------------------------------------------------------

您可以選擇在 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 項目中顯示 **Recording** 物件的所有詳細資料。 但這會佔用大量空間。 相反地，您可以在項目中顯示剛好足夠識別它的資料，然後當使用者做出選擇時，您可以在另一個稱為詳細資料檢視的 UI 中，顯示選定項目的所有詳細資料。 這種安排也稱為主要/詳細資料檢視，或清單/詳細資料檢視。

有兩種作法。 您可以將詳細資料檢視繫結到 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 的 [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/BR209770) 屬性。 或者，您可以使用 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833)：將 **ListView** 和詳細資料檢視都繫結到 **CollectionViewSource** (將為您處理目前選取的項目)。 以下顯示這兩種技巧，且兩者的結果相同，如圖所示。

> [!NOTE]
> 本主題到目前為止，我們只使用 [{x:Bind} 標記延伸](https://msdn.microsoft.com/library/windows/apps/Mt204783)，但以下我們將說明的兩種技巧需要更有彈性 (但效能較低) 的 [{Binding} 標記延伸](https://msdn.microsoft.com/library/windows/apps/Mt204782)。

首先是 [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/BR209770) 技術。 如果您使用的是 Visual C++ 元件延伸 (C++/CX)，則由於我們將使用 [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782)，因此您需要將 [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) 屬性新增到 **Recording** 類別。

```cpp
    [Windows::UI::Xaml::Data::Bindable]
    public ref class Recording sealed
    {
        ...
    };
```

其他只需變更標記。

```xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
            <ListView x:Name="recordingsListView" ItemsSource="{x:Bind ViewModel.Recordings}">
                <ListView.ItemTemplate>
                    <DataTemplate x:DataType="local:Recording">
                        <StackPanel Orientation="Horizontal" Margin="6">
                            <SymbolIcon Symbol="Audio" Margin="0,0,12,0"/>
                            <StackPanel>
                                <TextBlock Text="{x:Bind CompositionName}"/>
                            </StackPanel>
                        </StackPanel>
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
            <StackPanel DataContext="{Binding SelectedItem, ElementName=recordingsListView}"
            Margin="0,24,0,0">
                <TextBlock Text="{Binding ArtistName}"/>
                <TextBlock Text="{Binding CompositionName}"/>
                <TextBlock Text="{Binding ReleaseDateTime}"/>
            </StackPanel>
        </StackPanel>
    </Grid>
</Page>
```

使用 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) 技術時，請先新增 **CollectionViewSource** 做為頁面資源。

```xml
    <Page.Resources>
        <CollectionViewSource x:Name="RecordingsCollection" Source="{x:Bind ViewModel.Recordings}"/>
    </Page.Resources>
```

然後，將 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) (不再需要命名) 和詳細資料檢視上的繫結調整為使用 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833)。 請注意，將詳細資料檢視直接繫結到 **CollectionViewSource** 時，就意味著您想要繫結至在集合本身找不到路徑之繫結中的目前項目。 不需要指定 **CurrentItem** 屬性做為繫結的路徑 (但如果情況模稜兩可，您可以這樣做)。

```xml
    ...

    <ListView ItemsSource="{Binding Source={StaticResource RecordingsCollection}}">

    ...

    <StackPanel DataContext="{Binding Source={StaticResource RecordingsCollection}}" ...>
    ...
```

以下是各種情況的相同結果。

![繫結清單檢視](images/xaml-databinding4.png)

<a name="formatting-or-converting-data-values-for-display"></a>格式化或轉換資料值以供顯示
--------------------------------------------------------------------------------------------------------------------------------------------

以上呈現的結果有一個小問題。 **ReleaseDateTime** 屬性不只是日期，而且還是 [**DateTime**](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetime.aspx)，結果顯示多餘的精確度。 一種解決辦法是將字串屬性加入到 **Recording** 類別，以傳回 `this.ReleaseDateTime.ToString("d")`。 將該屬性命名為 **ReleaseDate** 表示只傳回日期，而不是日期和時間。 命名為 **ReleaseDateAsString** 進一步表示傳回字串。

更有彈性的解決辦法是使用所謂的「值轉換器」。 以下是如何撰寫您自己的值轉換器的範例。 將下列程式碼加入到 Recording.cs 原始程式碼檔。

```csharp
public class StringFormatter : Windows.UI.Xaml.Data.IValueConverter
{
    // This converts the value object to the string to display.
    // This will work with most simple types.
    public object Convert(object value, Type targetType,
        object parameter, string language)
    {
        // Retrieve the format string and use it to format the value.
        string formatString = parameter as string;
        if (!string.IsNullOrEmpty(formatString))
        {
            return string.Format(formatString, value);
        }

        // If the format string is null or empty, simply
        // call ToString() on the value.
        return value.ToString();
    }

    // No need to implement converting back on a one-way binding
    public object ConvertBack(object value, Type targetType,
        object parameter, string language)
    {
        throw new NotImplementedException();
    }
}
```

現在我們可以新增 **StringFormatter** 的執行個體做為頁面資源，然後用在我們的繫結中。 我們從標記中將格式字串傳遞至轉換器，發揮最高的格式化彈性。

```xml
    <Page.Resources>
        <local:StringFormatter x:Key="StringFormatterValueConverter"/>
    </Page.Resources>
    ...

    <TextBlock Text="{Binding ReleaseDateTime,
        Converter={StaticResource StringFormatterValueConverter},
        ConverterParameter=Released: \{0:d\}}"/>

    ...
```

結果如下。

![顯示自訂格式的日期](images/xaml-databinding5.png)

> [!NOTE]
> 從 Windows 10 版本 1607 開始，XAML 架構針對可見度轉換器提供了內建布林值。 轉換器會將 **true** 對應至 **Visible** 列舉值，並將 **false** 對應至 **Collapsed**，這樣您就可以將 Visibility 屬性繫結至布林值而不用建立轉換器。 若要使用內建轉換器，您 App 的最低目標 SDK 版本必須為 14393 或更新版本。 當您的 App 是以舊版 Windows10 為目標時，您就無法使用它。 如需目標版本的相關詳細資訊，請參閱[版本調適型程式碼](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)。

## <a name="see-also"></a>另請參閱
- [資料繫結](index.md)