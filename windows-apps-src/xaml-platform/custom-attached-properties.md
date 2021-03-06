---
description: 說明如何將 XAML 附加屬性當作相依性屬性來實作，以及如何定義讓附加屬性可以在 XAML 中使用所需的存取子慣例。
title: 自訂附加屬性
ms.assetid: E9C0C57E-6098-4875-AA3E-9D7B36E160E0
ms.date: 07/18/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: f23d66acc9371fd7b23b6770a0c7be6d16f86be4
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340610"
---
# <a name="custom-attached-properties"></a>自訂附加屬性

「附加屬性」是一種 XAML 概念。 附加屬性通常會定義為特殊格式的相依性屬性。 這個主題說明如何將附加屬性當作相依性屬性來實作，以及如何定義讓附加屬性可以在 XAML 中使用所需的存取子慣例。

## <a name="prerequisites"></a>必要條件

我們假設您了解現有相依性屬性使用者對相依性屬性的觀點，而且也已經閱讀了[相依性屬性概觀](dependency-properties-overview.md)。 您必須也要閱讀[附加屬性概觀](attached-properties-overview.md)。 為了遵循這個主題中的範例，您也必須了解 XAML 並知道如何使用 C++、C# 或 Visual Basic 撰寫基本的 Windows 執行階段應用程式。

## <a name="scenarios-for-attached-properties"></a>附加屬性的案例

當定義類別以外的類別需要屬性設定機制的時候，就可以建立附加屬性。 最常見的案例是配置和服務支援。 現有配置屬性的範例包括 [**Canvas.ZIndex**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc190397(v=vs.95)) 和 [**Canvas.Top**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.top)。 在配置案例中，做為配置控制元素之子元素的元素，可以個別對它們的父元素表達配置需求，每個都會將父元素定義的屬性值設定為附加屬性。 Windows 執行階段 API 中服務支援案例的範例是一組 [**ScrollViewer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) 的附加屬性，如 [**ScrollViewer.IsZoomChainingEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.iszoomchainingenabled)。

> [!WARNING]
> Windows 執行階段 XAML 執行的現有限制是您無法建立自訂附加屬性的動畫。

## <a name="registering-a-custom-attached-property"></a>登錄自訂附加屬性

如果定義的附加屬性純粹只是在其他類型上使用，登錄屬性的類別不一定要衍生自 [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject)。 不過，如果您遵循讓附加屬性也是相依性屬的典型模式，則存取子的 target 參數需要使用 **DependencyObject**，如此一來，您就可以使用支援屬性儲存區。

藉由宣告類型為[**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty)的**公用** **靜態** **唯讀**屬性，將附加屬性定義為相依性屬性。 使用 [**RegisterAttached**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.registerattached) 方法的傳回值來定義這個屬性。 屬性名稱必須符合您指定為**system.windows.dependencyproperty.registerattached** *name*參數的附加屬性名稱，並將字串 "property" 新增至結尾。 這是根據代表的屬性命名相依性屬性識別碼的既定慣例。

定義自訂附加屬性與自訂相依性屬性的主要差異在於定義存取子或包裝函式的方式。 您不是使用[自訂相依性屬性](custom-dependency-properties.md)中所述的包裝函式技術，而是必須也提供靜態 **Get**_PropertyName_ 和 **Set**_PropertyName_ 方法做為附加屬性的存取子。 存取子主要是由 XAML 剖析器使用，不過任何其他呼叫者也可以使用它們在非 XAML 案例中設定值。

> [!IMPORTANT]
> 如果您未正確定義存取子，XAML 處理器就無法存取您的附加屬性，而且嘗試使用它的任何人都可能會收到 XAML 剖析器錯誤。 此外，設計和程式碼撰寫工具通常會依賴「\*屬性」慣例來命名識別碼，而當它們遇到參考元件中的自訂相依性屬性時。

## <a name="accessors"></a>存取子

**Get**_PropertyName_ 存取子的簽章必須是這個。

`public static` _valueType_ **Get**_PropertyName_ `(DependencyObject target)`

針對 Microsoft Visual Basic，它是這個。

`Public Shared Function Get`_PropertyName_`(ByVal target As DependencyObject) As `_valueType_`)`

*target* 物件可以是實作中更具體的類型，但是必須衍生自 [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject)。 *valueType* 傳回值也可以是實作中的更具體類型。 基本 **Object** 類型是可以被接受的，不過通常您會希望您的附加屬性強制執行類型安全技術。 建議的類型安全技術是在 getter 和 setter 簽章中使用類型。

**Set**_PropertyName_ 存取子的簽章必須是這個。

`public static void Set`_PropertyName_` (DependencyObject target , `_valueType_` value)`

針對 Visual Basic，它是這個。

`Public Shared Sub Set`_PropertyName_` (ByVal target As DependencyObject, ByVal value As `_valueType_`)`

*target* 物件可以是實作中更具體的類型，但是必須衍生自 [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject)。 *value* 物件及它的 *valueType* 可以是實作中的更具體類型。 請記住，這個方法的值是 XAML 處理器在標記中遇到附加屬性時，來自於 XAML 處理器的輸入。 您使用的類型必須取得類型轉換或現有標記延伸的支援，這樣才能從屬性值建立適當的類型 (最終只是字串)。 基本 **Object** 類型是可以接受的，但是通常您會想要更進一步的類型安全性。 若要這樣做，請將類型強制放到存取子中。

> [!NOTE]
> 您也可以定義附加屬性，其中預期的用法是透過屬性元素語法。 在該情況下，您不需要輸入值的轉換，但必須確保您想要的值可以在 XAML 中建構。 [**VisualStateManager. system.windows.visualstatemanager.visualstategroups**](https://docs.microsoft.com/dotnet/api/system.windows.visualstatemanager)是現有附加屬性的範例，其僅支援屬性專案使用方式。

## <a name="code-example"></a>程式碼範例

這個範例示範自訂附加屬性的相依性屬性登錄 (使用 [**RegisterAttached**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.registerattached) 方法) 以及 **Get** 和 **Set** 存取子。 在範例中，附加屬性名稱是 `IsMovable`。 所以，存取子必須命名為 `GetIsMovable` 和 `SetIsMovable`。 附加屬性的擁有者是名為 `GameService` 的服務類別，這個類別沒有自己的 UI；其用途只是在使用 **GameService.IsMovable** 附加屬性時提供附加屬性服務。

在/Cx 中C++定義附加屬性有點複雜。 您必須決定如何切分標頭與程式碼檔案。 另外，您應該將識別碼公開為只包含一個 **get** 存取子的屬性，原因請參閱[自訂相依性屬性](custom-dependency-properties.md)中的討論。 在C++/cx 中，您必須明確地定義此屬性欄位關聯性，而不是依賴 .net **readonly** keywording 和簡單屬性的隱含支援。 當應用程式第一次啟動，但在載入任何需要附加屬性的 XAML 頁面之前，您也需要在只執行一次的協助程式函式內執行附加屬性的登錄。 一般會針對任何和全部相依性或附加屬性呼叫屬性登錄協助程式函式的位置，是從 app.xaml 檔案程式碼中的 **App** / [**Application**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.-ctor) 建構函式內呼叫。

```csharp
public class GameService : DependencyObject
{
    public static readonly DependencyProperty IsMovableProperty = 
    DependencyProperty.RegisterAttached(
      "IsMovable",
      typeof(Boolean),
      typeof(GameService),
      new PropertyMetadata(false)
    );
    public static void SetIsMovable(UIElement element, Boolean value)
    {
        element.SetValue(IsMovableProperty, value);
    }
    public static Boolean GetIsMovable(UIElement element)
    {
        return (Boolean)element.GetValue(IsMovableProperty);
    }
}
```

```vb
Public Class GameService
    Inherits DependencyObject

    Public Shared ReadOnly IsMovableProperty As DependencyProperty = 
        DependencyProperty.RegisterAttached("IsMovable",  
        GetType(Boolean), 
        GetType(GameService), 
        New PropertyMetadata(False))

    Public Shared Sub SetIsMovable(ByRef element As UIElement, value As Boolean)
        element.SetValue(IsMovableProperty, value)
    End Sub

    Public Shared Function GetIsMovable(ByRef element As UIElement) As Boolean
        GetIsMovable = CBool(element.GetValue(IsMovableProperty))
    End Function
End Class
```

```cppwinrt
// GameService.idl
namespace UserAndCustomControls
{
    [default_interface]
    runtimeclass GameService : Windows.UI.Xaml.DependencyObject
    {
        GameService();
        static Windows.UI.Xaml.DependencyProperty IsMovableProperty{ get; };
        static Boolean GetIsMovable(Windows.UI.Xaml.DependencyObject target);
        static void SetIsMovable(Windows.UI.Xaml.DependencyObject target, Boolean value);
    }
}

// GameService.h
...
    static Windows::UI::Xaml::DependencyProperty IsMovableProperty() { return m_IsMovableProperty; }
    static bool GetIsMovable(Windows::UI::Xaml::DependencyObject const& target) { return winrt::unbox_value<bool>(target.GetValue(m_IsMovableProperty)); }
    static void SetIsMovable(Windows::UI::Xaml::DependencyObject const& target, bool value) { target.SetValue(m_IsMovableProperty, winrt::box_value(value)); }

private:
    static Windows::UI::Xaml::DependencyProperty m_IsMovableProperty;
...

// GameService.cpp
...
Windows::UI::Xaml::DependencyProperty GameService::m_IsMovableProperty =
    Windows::UI::Xaml::DependencyProperty::RegisterAttached(
        L"IsMovable",
        winrt::xaml_typename<bool>(),
        winrt::xaml_typename<UserAndCustomControls::GameService>(),
        Windows::UI::Xaml::PropertyMetadata{ winrt::box_value(false) }
);
...
```

```cpp
// GameService.h
#pragma once

#include "pch.h"
//namespace WUX = Windows::UI::Xaml;

namespace UserAndCustomControls {
    public ref class GameService sealed : public WUX::DependencyObject {
    private:
        static WUX::DependencyProperty^ _IsMovableProperty;
    public:
        GameService::GameService();
        void GameService::RegisterDependencyProperties();
        static property WUX::DependencyProperty^ IsMovableProperty
        {
            WUX::DependencyProperty^ get() {
                return _IsMovableProperty;
            }
        };
        static bool GameService::GetIsMovable(WUX::UIElement^ element) {
            return (bool)element->GetValue(_IsMovableProperty);
        };
        static void GameService::SetIsMovable(WUX::UIElement^ element, bool value) {
            element->SetValue(_IsMovableProperty,value);
        }
    };
}

// GameService.cpp
#include "pch.h"
#include "GameService.h"

using namespace UserAndCustomControls;

using namespace Platform;
using namespace Windows::Foundation;
using namespace Windows::Foundation::Collections;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::UI::Xaml::Data;
using namespace Windows::UI::Xaml::Documents;
using namespace Windows::UI::Xaml::Input;
using namespace Windows::UI::Xaml::Interop;
using namespace Windows::UI::Xaml::Media;

GameService::GameService() {};

GameService::RegisterDependencyProperties() {
    DependencyProperty^ GameService::_IsMovableProperty = DependencyProperty::RegisterAttached(
         "IsMovable", Platform::Boolean::typeid, GameService::typeid, ref new PropertyMetadata(false));
}
```

## <a name="setting-your-custom-attached-property-from-xaml-markup"></a>從 XAML 標記設定自訂附加屬性

> [!NOTE]
> 如果您使用C++的是/WinRT，請跳至下一節（以命令式[方式設定自訂C++附加屬性的/WinRT](#setting-your-custom-attached-property-imperatively-with-cwinrt)）。

定義您的附加屬性並將它的支援成員包含為自訂類型的一部分之後，您接著必須讓 XAML 可以使用定義。 若要這樣做，您必須對應將要參考包含相關類別程式碼命名空間的 XAML 命名空間。 在已經將附加屬性定義為程式庫一部分的情況中，您必須包含這個程式庫，讓它成為應用程式之應用程式套件的一部分。

XAML 的 XML 命名空間對應通常會放置在 XAML 頁面的根元素中。 例如，針對包含前面程式碼片段中所示之附加屬性定義的命名空間 `GameService` 中名為 `UserAndCustomControls` 的類別，其對應看起來就會像這樣。

```xaml
<UserControl
  xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
  xmlns:uc="using:UserAndCustomControls"
  ... >
```

使用對應即可在符合您目標定義的任何元素上設定您的 `GameService.IsMovable` 附加屬性，包括 Windows 執行階段定義的現有類型。

```xaml
<Image uc:GameService.IsMovable="True" .../>
```

如果您在某個元素上設定屬性且該元素也在同一個對應的 XML 命名空間中，仍然需要在附加屬性名稱上包含前置詞。 這是因為前置詞會限定擁有者類型。 您不能假設附加屬性的屬性一定位於與包含屬性的元素相同的 XML 命名空間內；不過，根據一般的 XML 規則，屬性可以從元素繼承命名空間。 例如，如果您在自訂類型的 `GameService.IsMovable` 上設定 `ImageWithLabelControl` (未顯示定義)，而且即使這兩者是在對應到相同前置詞的同一個程式碼命名空間中定義的，XAML 仍然會是這個。

```xaml
<uc:ImageWithLabelControl uc:GameService.IsMovable="True" .../>
```

> [!NOTE]
> 如果您要撰寫具有C++/CX 的 xaml UI，則每當 XAML 頁面使用該類型時，您都必須包含定義附加屬性之自訂類型的標頭。 每個 XAML 頁面都有相關聯的程式碼後置標頭（.xaml）。 這是您應該在其中包含附加屬性之擁有者類型定義的標頭（使用 **\#包含**）。

## <a name="setting-your-custom-attached-property-imperatively-with-cwinrt"></a>以命令方式使用C++/WinRT 設定自訂附加屬性

如果您使用C++/WinRT，則可以從命令式程式碼（而不是從 XAML 標記）存取自訂附加屬性。 下列程式碼顯示如何。

```xaml
<Image x:Name="gameServiceImage"/>
```

```cppwinrt
// MainPage.h
...
#include "GameService.h"
...

// MainPage.cpp
...
MainPage::MainPage()
{
    InitializeComponent();

    GameService::SetIsMovable(gameServiceImage(), true);
}
...
```

## <a name="value-type-of-a-custom-attached-property"></a>自訂附加屬性的值類型

做為自訂附加屬性值類型的類型會影響使用方式、定義或同時影響兩者。 附加屬性的值類型會在數個位置宣告：在 **Get** 與 **Set** 存取子兩個方法的簽章中，以及做為RegisterAttached[**呼叫的**propertyType](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.registerattached) 參數。

最常見的附加屬性 (自訂或其他) 值類型是簡單字串。 這是因為附加屬性通常是用於 XAML 屬性，而將字串當作值類型能讓屬性變得更為精簡。 使用原生轉換為字串方法的其他基本類型 (例如整數、雙精度浮點數或列舉值) 也是附加屬性常用的值類型。 您可以使用其他值類型—不支援原生字串轉換的值類型—當作附加屬性值。 不過，這就取決於您要選擇使用或實作它：

- 您可以讓附加屬性保持原狀，但是附加屬性只能支援附加屬性是屬性元素且值被宣告為物件元素的使用方法。 在這種情況下，屬性類型不一定要支援使用 XAML 作為物件元素。 若為現有的 Windows 執行階段參考類別，請檢查 XAML 語法來確保類型支援 XAML 物件元素使用方法。
- 您可以讓附加屬性保持原狀，但是透過 XAML 參考技術 (如 **Binding** 或可以使用字串表示的 **StaticResource**) 僅在使用屬性時使用它。

## <a name="more-about-the-canvasleft-example"></a>進一步了解 **Canvas.Left** 範例

在前面的附加屬性用法範例中，我們示範了設定 [**Canvas.Left**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.left) 附加屬性的不同方法。 但是，這對於 [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) 與您物件的互動方式有何改變，以及在何時發生？ 我們將進一步探討這個特定的範例，因為如果您實作附加屬性，看看當典型的附加屬性擁有者類別在其他物件上發現它的附加屬性值時，會對這些值做哪些其他處理，將是一件有趣的事。

[  **Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) 的主要功能是成為 UI 中的絕對位置配置容器。 **Canvas** 的子項會儲存在基底類別定義的屬性 [**Children**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.panel.children) 中。 在所有的面板中，**Canvas** 是唯一使用絕對位置的面板。 當新增屬性而屬性可能只與 [Canvas**和它們身為**UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) 子元素的特定 **UIElement** 案例相關時，會將常見的UIElement 類型的物件模型塞得臃腫不堪。 將 **Canvas** 的配置控制項屬性定義成任何 **UIElement** 都可以使用的附加屬性可以讓物件模型保持簡潔。

為了能夠成為實際的面板，[**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) 具有會覆寫架構層級的 [**Measure**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) 和 [**Arrange**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) 方法的行為。 這是 **Canvas** 實際檢查其子項是否有附加屬性值的位置。 **Measure** 和 **Arrange** 模式都有部分是迴圈，會逐一查看任何內容，而面板具有 [**Children**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.panel.children) 屬性，可以明確指出哪個應該視為面板子項。 因此，**Canvas** 配置行為會逐一查看這些子項，並且在每個子項進行靜態的 [**Canvas.GetLeft**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas.getleft) 和 [**Canvas.GetTop**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas.gettop) 呼叫，以查看那些附加屬性是否包含非預設值 (預設值為 0)。 然後，會使用這些值並根據每個子項提供的特定值，以賦予每個子項在 **Canvas** 可用配置空間中的絕對位置，然後使用 **Arrange** 來進行認可。

程式碼看起來像這個虛擬程式。

```syntax
protected override Size ArrangeOverride(Size finalSize)
{
    foreach (UIElement child in Children)
    {
        double x = (double) Canvas.GetLeft(child);
        double y = (double) Canvas.GetTop(child);
        child.Arrange(new Rect(new Point(x, y), child.DesiredSize));
    }
    return base.ArrangeOverride(finalSize); 
    // real Canvas has more sophisticated sizing
}
```

> [!NOTE]
> 如需面板如何工作的詳細資訊，請參閱[XAML 自訂面板總覽](https://docs.microsoft.com/windows/uwp/layout/custom-panels-overview)。

## <a name="related-topics"></a>相關主題

* [**System.windows.dependencyproperty.registerattached**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.registerattached)
* [附加屬性概觀](attached-properties-overview.md)
* [自訂相依性屬性](custom-dependency-properties.md)
* [XAML 概觀](xaml-overview.md)
