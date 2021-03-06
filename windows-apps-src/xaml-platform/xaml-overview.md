---
description: 為 Windows 執行階段應用程式開發人員物件介紹 XAML 語言和 XAML 概念，並說明在 XAML 中宣告物件和設定屬性的不同方式，因為它是用來建立 Windows 執行階段應用程式。
title: XAML 概觀
ms.assetid: 48041B37-F1A8-44A4-BB8E-1D4DE30E7823
ms.date: 07/18/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: 6d45e70afa5b0dc6e903dbd253c09042bb2046c2
ms.sourcegitcommit: f7ef7e894d7b7fc24483b4485605686abf8f2e93
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/30/2019
ms.locfileid: "71679216"
---
# <a name="xaml-overview"></a>XAML 概觀

本文介紹 XAML 語言和 XAML 概念給 Windows 執行階段應用程式開發人員的物件，並說明在 XAML 中用來建立 Windows 執行階段應用程式時，用來宣告物件和設定屬性的不同方式。

## <a name="what-is-xaml"></a>什麼是 XAML？

Extensible Application Markup Language (XAML) 是一種宣告式語言。 具體而言，XAML 可以初始化物件，並使用語言結構來設定物件的屬性，而該架構會顯示多個物件之間的階層式關聯性，以及支援類型擴充功能的支援類型慣例。 您可以利用宣告式 XAML 標記建立視覺上可以看到的 UI 元素。 您可以接著關聯可以回應事件的每個 XAML 檔案的個別程式碼後置檔案，然後操縱您原先在 XAML 中宣告的物件。

XAML 語言支援在開發程式中的不同工具和角色之間交換來源，例如在設計工具和互動式開發環境（IDE）之間交換 XAML 來源，或在主要開發人員和當地語系化之間交換能夠. 使用 XAML 做為交換格式時，設計師角色和開發人員角色可以各自保持獨立或合併在一起，而設計師和開發人員在 app 製作期間可以重複。

當您看見它們成為您 Windows 執行階段應用程式專案的一部分時，XAML 檔案是副檔名為 .xaml 的 XML 檔案。

## <a name="basic-xaml-syntax"></a>基本 XAML 語法

XAML 具備以 XML 為依據的基本語法。 根據定義，有效的 XAML 也必須是有效的 XML。 但 XAML 也具有已指派不同且更完整意義的語法概念，但根據 XML 1.0 規格，仍然會在 XML 中有效。 例如，XAML 支援「屬性元素語法」，其中的屬性值可以在元素內 (而不是當成屬性中的字串值或內容) 來設定。 如果是一般 XML，XAML 屬性元素就是其名稱內有一個點的元素，如此一來，它對純 XML 就是有效的，但不會有相同的意義。

## <a name="xaml-and-visual-studio"></a>XAML 和 Visual Studio

Microsoft Visual Studio 可以在 XAML 文字編輯器與更多圖形導向的 XAML 設計介面中，協助您產生有效的 XAML 語法。 當您使用 Visual Studio 為應用程式撰寫 XAML 時，請不要擔心每個擊鍵的語法太多。 IDE 會藉由提供自動完成提示來鼓勵有效的 XAML 語法，並在 Microsoft IntelliSense 清單和下拉式清單中顯示建議、在 [**工具箱**] 視窗中顯示 UI 元素程式庫，或其他技術。 如果這是您第一次使用 XAML，在參考或其他主題中描述 XAML 語法時，知道語法規則和有時候用來描述限制或選擇的術語可能還是很有用。 Xaml 語法的細微點包含在[xaml 語法指南](xaml-syntax-guide.md)的個別主題中。

## <a name="xaml-namespaces"></a>XAML 命名空間

在進行一般程式設計時，命名空間是一個組織的概念，可判斷如何解譯用於程式設計實體的識別碼。 透過使用命名空間，程式設計架構就可以區分使用者宣告的識別碼與架構宣告的識別碼、透過命名空間的限定性條件使識別碼意義清楚、強制執行範圍名稱的規則等等。 XAML 有自己的 XAML 命名空間概念，可達成 XAML 語言的這個目的。 這裡是 XAML 套用和延伸 XML 語言命名空間概念的方式：

- XAML 會使用保留的 XML 屬性 **xmlns** 來進行命名空間宣告。 這個屬性值通常是一個統一資源識別元 (URI)，它是繼承自 XML 的慣例。
- XAML 在宣告中使用前置詞來宣告非預設命名空間，並在元素和屬性中使用前置詞來參考該命名空間。
- XAML 具備預設命名空間的概念，這是在用法或宣告中沒有任何前置詞存在時使用的命名空間。 預設的命名空間可針對每個 XAML 程式設計架構，以不同方式來定義。
- XAML 檔案中的命名空間定義是從父元素繼承或建構到子元素。 例如，如果您在 XAML 檔案的根項目中定義命名空間，則該檔案內的所有元素都會繼承該命名空間定義。 如果進一步深入頁面的元素會重新定義命名空間，則該元素的子系會繼承新定義。
- 元素的屬性則是繼承元素的命名空間。 很少會在 XAML 屬性上看到前置詞。

XAML 檔案幾乎永遠在它的根元素中宣告預設的 XAML 命名空間。 預設 XAML 命名空間定義您可以宣告但不必使用前置詞來限定的元素。 對於典型的 Windows 執行階段 app 專案，這個預設命名空間包含適用於 Windows 執行階段且可用於 UI 定義的所有內建 XAML 詞彙：預設控制項、文字元素、XAML 圖形和動畫、資料繫結與樣式支援類型等。 您針對 Windows 執行階段應用程式所撰寫的多數 XAML 從而能在參考常見的 UI 元素時，避免使用 XAML 命名空間和前置詞。

以下程式碼片段顯示應用程式初始頁面的範本建立 <xref:Windows.UI.Xaml.Controls.Page> 根目錄（僅顯示開頭標記，並簡化）。 它宣告預設的命名空間，以及 **x** 命名空間 (我們將在下段加以說明)。

```xml
<Page
    x:Class="Application1.BlankPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
>
```

## <a name="the-xaml-language-xaml-namespace"></a>XAML 語言 XAML 命名空間

有一個幾乎會在每個 Windows 執行階段 XAML 檔案中宣告的特定 XAML 命名空間，就是 XAML 語言命名空間。 這個命名空間包含 XAML 語言規格所定義的元素和概念。 依照慣例，XAML 語言 XAML 命名空間會對應至前置詞 "x"。 Windows 執行階段應用程式專案的預設專案與檔案範本，永遠會將預設的 XAML 命名空間 (無前置詞，只有 `xmlns=`) 與 XAML 語言 XAML 命名空間 (前置詞 "x") 定義為根元素的一部分。

"x" 前置詞/XAML 語言 XAML 命名空間包含數個您經常用在 XAML 中的程式設計建構。 以下是最常用的建構：

| 詞彙 | 描述 |
|------|-------------|
| [x：Key](x-key-attribute.md) | 為 XAML <xref:Windows.UI.Xaml.ResourceDictionary>中的每個資源設定唯一的使用者定義索引鍵。 索引鍵的權杖字串是 **StaticResource** 標記延伸的引數，而您稍後可以使用這個索引鍵，從位於 app XAML 任一處的其他 XAML 用法中抓取 XAML 資源。 |
| [x：Class](x-class-attribute.md) | 指定類別的程式碼命名空間和程式碼類別名稱，該類別提供 XAML 頁面的程式碼後置。 這會在您建置應用程式時，為組建動作所建立或加入的類別命名。 這些組建動作支援 XAML 標記編譯器，並且可以在編譯應用程式時組合您的標記和程式碼後置。 您必須擁有這類類別，才能支援 XAML 頁面的程式碼後置。 <xref:Windows.UI.Xaml.Window.Content%2A?displayProperty=nameWithType> 預設 Windows 執行階段啟用模型。 |
| [x：Name](x-name-attribute.md) | 為處理完 XAML 中定義的物件元素後而存在執行階段程式碼中的執行個體，指定執行階段物件名稱。 您可以將在 XAML 中設定 **x:Name** 想像成在程式碼中宣告具名變數。 之後您就會了解，當您的 XAML 載入為 Windows 執行階段 app 的元件時所發生的狀況。 <br/><div class="alert">**請注意**，<xref:Windows.UI.Xaml.FrameworkElement.Name%2A> 在架構中是類似的屬性，但並非所有元素都支援它。 每當該專案類型不支援**FrameworkElement.Name**時，請使用**x：Name**進行元素識別。 |
| [x：Uid](x-uid-directive.md) | 識別應該為某些屬性值使用當地語系化資源的元素。 如需如何使用 **x:Uid** 的詳細資訊，請參閱[快速入門：翻譯 UI 資源](/previous-versions/windows/apps/hh965329(v=win.10))。 |
| [XAML 內建資料類型](xaml-intrinsic-data-types.md) \(部分機器翻譯\) | 這些類型可以針對屬性或資源要求的簡單值類型指定值。 這些內建類型會對應到通常是針對每種程式設計語言內建定義所定義的簡單值類型。 例如，您可能需要代表要在 <xref:Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames> storyboarded 視覺狀態中使用之**true**布林值的物件。 對於 XAML 中的該值，您會使用**x:Boolean**內建類型做為物件元素，如下所示： <code>&lt;x:Boolean&gt;True&lt;/x:Boolean&gt;</code> |

XAML 語言 XAML 命名空間中的其他程式設計建構雖然存在，但比較少見。

## <a name="mapping-custom-types-to-xaml-namespaces"></a>將自訂類型對應到 XAML 命名空間

就語言方面來說，XAML 的一個最重要概念就是能夠輕易地擴充適用於 Windows 執行階段應用程式的 XAML 詞彙。 您可以在應用程式的程式設計語言中定義自己的自訂類型，然後在 XAML 標記中參考您的自訂類型。 透過自訂類型進行擴充的支援，基本上是內建於 XAML 語言的運作方式。 架構或應用程式開發人員會負責建立 XAML 參考的支援物件。 架構和應用程式開發人員都不會受到其詞彙中的物件所代表或超出基本 XAML 語法規則的規格。 （XAML 語言 XAML 命名空間型別應該做些什麼，但是 Windows 執行階段提供所有必要的支援）。

如果您針對 Windows 執行階段核心程式庫和中繼資料以外的程式庫隨附的類型使用 XAML，就必須宣告並對應含有前置詞的 XAML 命名空間。 請在元素用法中使用該前置詞，以參考您的程式庫中已定義的類型。 您通常會在根元素以及其他 XAML 命名空間定義中，將前置詞對應宣告為 **xmlns** 屬性。

若要建立您自己的命名空間定義來參考自訂類型，首先要指定關鍵字 **xmlns:** ，接著是您想要的前置詞。 該屬性的值必須包含關鍵字 **using:** 做為值的第一個部分。 此值的其餘部分為字串語彙基元，依名稱參考特定程式碼支援命名空間 (其中包含您自訂的類型)。

前置詞定義了標記語彙基元，這個標記語彙基元是用來參考該 XAML 檔案中標記剩餘部分的那個 XAML 命名空間。 冒號字元 (:) 是放在前置詞以及 XAML 命名空間內要參考的實體之間。

例如，將前置詞 `myTypes` 對應至命名空間的屬性語法 `myCompany.myTypes` 為： `    xmlns:myTypes="using:myCompany.myTypes"`，而代表元素的使用方式為： `<myTypes:CustomButton/>`

如需針對自訂類型對應 XAML 命名空間的詳細資訊，包含適用於 Visual C++ 元件延伸 (C++/CX) 的特殊考量，請參閱 [XAML 命名空間與命名空間對應](xaml-namespaces-and-namespace-mapping.md)。

## <a name="other-xaml-namespaces"></a>其他 XAML 命名空間

您經常會看到定義前置詞的 XAML 檔案 "d" (用於設計人員命名空間) 與 "mc" (用於標記相容性)。 一般而言，這些是用於基礎結構支援，或在設計階段工具中啟用案例。 如需詳細資訊，請參閱 [XAML 命名空間主題的＜其他 XAML 命名空間＞一節](xaml-namespaces-and-namespace-mapping.md#other-XAML-namespaces)。

## <a name="markup-extensions"></a>標記延伸

標記延伸是 Windows 執行階段 XAML 實作中經常用到的 XAML 語言概念。 標記延伸通常代表某種「捷徑」，可讓 XAML 檔案能夠存取某個值或行為，而不只是根據支援類型來宣告元素。 部分標記延伸若其目標為簡化語法或不同 XAML 檔案間的建構，就可以使用純字串或額外巢狀元素來設定屬性。

在 XAML 屬性語法中，大括號 "{" 與 "}" 表示 XAML 標記延伸用法。 這個用法會指示 XAML 處理將處理屬性值的一般處理逸出為常值字串或直接的字串可轉換值。 相反地，XAML 剖析器會呼叫可提供適用於該特定標記延伸的行為的程式碼，而該程式碼可以提供 XAML 剖析器所需的替代物件或行為結果。 標記延伸可以有引數 (之後緊接著標記延伸名稱)，而且也必須包含在大括號內。 通常，經過評估的標記延伸會提供物件傳回值。 在剖析期間，該傳回值會插入物件樹中的位置，而此為標記延伸用法之前在來源 XAML 中的所在位置。

Windows 執行階段 XAML 支援下列標記延伸，它們定義在預設的 XAML 命名空間下，並且可被 Windows 執行階段 XAML 剖析器了解：

- [{x:Bind}](x-bind-markup-extension.md)：支援資料系結，這會在執行時間執行特殊用途的程式碼（在編譯時期產生），以延遲屬性評估。 這個標記延伸可支援大範圍的引數。
- [{Binding}](binding-markup-extension.md)：支援資料繫結，執行一般用途的執行階段物件檢查，將屬性評估延後到執行階段。 這個標記延伸可支援大範圍的引數。
- [{StaticResource}](staticresource-markup-extension.md)：支援參考在 <xref:Windows.UI.Xaml.ResourceDictionary>中定義的資源值。 這些資源可以位於不同的 XAML 檔案中，但最終必須能夠讓 XAML 剖析器在載入時找到。 `{StaticResource}` 使用方式的引數會識別 <xref:Windows.UI.Xaml.ResourceDictionary>中金鑰資源的索引鍵（名稱）。
- [{ThemeResource}](themeresource-markup-extension.md)：類似 [{StaticResource}](staticresource-markup-extension.md)，但可回應執行階段佈景主題變更。 {ThemeResource} 經常出現在 Windows 執行階段預設 XAML 範本中，因為大部分的這類範本其設計目的是為了提供相容性給在 app 執行時切換佈景主題的使用者。
- [{TemplateBinding}](templatebinding-markup-extension.md)：[{Binding}](binding-markup-extension.md) 的一種特殊情況，可支援 XAML 中的控制項範本及其在執行階段的最終用法。
- [{RelativeSource}](relativesource-markup-extension.md)：啟用特定格式的範本繫結，其中的值是來自範本化的父系。
- [{CustomResource}](customresource-markup-extension.md)：用於進階資源查詢案例。

Windows 執行階段也支援 [{x:Null} 標記延伸](x-null-markup-extension.md)。 您可以在 XAML 中使用這個標記延伸，將 [**Nullable**](/dotnet/api/system.nullable-1) 值設定為 **null**。 例如，您可以在 <xref:Windows.UI.Xaml.Controls.CheckBox>的控制項範本中使用這個，這會將**null**視為不確定的檢查狀態（觸發「不定」視覺狀態）。

標記延伸通常會從應用程式的物件圖形的其他部分傳回現有的實例，或將值延遲到執行時間。 因為您可以使用標記延伸做為屬性值，這也是典型的用法，所以您通常會看到標記延伸為參考類型屬性提供值，這些參考類型屬性可能需要其他屬性元素語法。

例如，以下是從 <xref:Windows.UI.Xaml.ResourceDictionary>參考可重複使用之 <xref:Windows.UI.Xaml.Style> 的語法： `<Button Style="{StaticResource SearchButtonStyle}"/>`。 <xref:Windows.UI.Xaml.Style> 是參考型別，而不是簡單的值，因此，如果沒有 `{StaticResource}` 使用方式，您就需要 `<Button.Style>` 屬性專案和其中的 `<Style>` 定義來設定 <xref:Windows.UI.Xaml.FrameworkElement.Style%2A?displayProperty=nameWithType> 屬性。

透過使用標記延伸，可以在 XAML 中設定的每一個屬性可能都可以在屬性語法中設定。 即使屬性不支援直接物件具現化的屬性語法，您還是可以使用屬性語法提供屬性的參考值。 或者，您可以啟用延遲一般需求的特定行為，讓值類型或最新建立的參考類型填入 XAML 屬性。

為了說明，下一個 XAML 範例會使用屬性語法來設定 <xref:Windows.UI.Xaml.Controls.Border> 之 <xref:Windows.UI.Xaml.FrameworkElement.Style%2A?displayProperty=nameWithType> 屬性的值。 <xref:Windows.UI.Xaml.FrameworkElement.Style%2A?displayProperty=nameWithType> 屬性會接受 <xref:Windows.UI.Xaml.Style?displayProperty=nameWithType> 類別的實例，這是預設無法使用屬性語法字串來建立的參考型別。 但是，在此案例中，屬性參考特定的標記延伸 [StaticResource](staticresource-markup-extension.md)。 在處理該標記延伸時，它會傳回對 **Style** 元素的參考，該元素是稍早在資源字典中定義為索引鍵資源的元素。

```xml
<Canvas.Resources>
  <Style TargetType="Border" x:Key="PageBackground">
    <Setter Property="BorderBrush" Value="Blue"/>
    <Setter Property="BorderThickness" Value="5"/>
  </Style>
</Canvas.Resources>
...
<Border Style="{StaticResource PageBackground}">
  ...
</Border>
```

您可以巢狀化標記延伸。 最內部的標記延伸會優先評估。

由於使用標記延伸，因此針對屬性中的文字 "{" 值，您需要使用特殊語法。 如需詳細資訊，請參閱 [XAML 語法指南](xaml-syntax-guide.md)。

## <a name="events"></a>事件

XAML 是物件與物件屬性的宣告式語言，但也包含將事件處理常式附加到標記物件的語法。 XAML 事件語法可以接著透過 Windows 執行階段的程式撰寫模型，整合 XAML 宣告的事件。 您可以在處理事件的物件中指定事件名稱做為屬性名稱。 對於屬性值，您可以指定在程式碼定義的事件處理函式名稱。 XAML 處理器會使用這個名稱在載入的物件樹中建立委派表示法，並將指定的處理常式新增到內部處理常式清單。 幾乎所有的 Windows 執行階段應用程式都是由標記與程式碼後置來源所定義。

以下是一個簡單範例。 <xref:Windows.UI.Xaml.Controls.Button> 類別支援名為 <xref:Windows.UI.Xaml.Controls.Primitives.ButtonBase.Click>的事件。 您可以為 **Click** 撰寫一個處理常式，執行在使用者按一下 **Button** 後應該叫用的程式碼。 在 XAML 中，指定 **Click** 做為 **Button** 上的屬性。 針對屬性值提供一個字串，此字串是您處理常式的方法名稱。

```xml
<Button Click="showUpdatesButton_Click">Show updates</Button>
```

當您編譯時，現在編譯器會預期在程式碼後置檔案中以及在 XAML 頁面之 `showUpdatesButton_Click`x:Class[ 值所宣告的命名空間中，將定義一個名為 ](x-class-attribute.md) 的方法。 此外，該方法必須滿足 <xref:Windows.UI.Xaml.Controls.Primitives.ButtonBase.Click> 事件的委派合約。 例如：

```csharp
namespace App1
{
    public sealed partial class MainPage: Page {
        ...
        private void showUpdatesButton_Click (object sender, RoutedEventArgs e) {
            //your code
        }
    }
}
```

```vb
' Namespace included at project level
Public NotInheritable Class MainPage
    Inherits Page
        ...
        Private Sub showUpdatesButton_Click (sender As Object, e As RoutedEventArgs e)
            ' your code
        End Sub
    ...
End Class
```

```cppwinrt
namespace winrt::App1::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        ...
        void showUpdatesButton_Click(Windows::Foundation::IInspectable const&, Windows::UI::Xaml::RoutedEventArgs const&);
    };
}
```

```cpp
// .h
namespace App1
{
    public ref class MainPage sealed {
        ...
    private:
        void showUpdatesButton_Click(Object^ sender, RoutedEventArgs^ e);
    };
}
```

在專案中，XAML 會寫成 .xaml 檔案，您可以使用偏好的語言 (C#、Visual Basic、C++/CX) 來撰寫程式碼後置檔案。 當某個 XAML 檔案以標記編譯成專案建置動作的一部分時，是透過將命名空間與類別指定為 XAML 頁面根元素的 [x:Class](x-class-attribute.md) 屬性，以識別每個 XAML 頁面的 XAML 程式碼後置檔案位置。 如需這些機制在 XAML 中如何運作與它們和程式撰寫模型及 app 模型的關聯的相關資訊，請參閱[事件與路由事件概觀](events-and-routed-events-overview.md)。

> [!NOTE]
> C++/Cx 有兩個程式碼後置檔案：一個是標頭（.xaml .h），另一個則是執行（.xaml）。 實作會參考標頭，技術上來說，它是代表程式碼後置連線進入點的標頭。

## <a name="resource-dictionaries"></a>資源字典

建立 <xref:Windows.UI.Xaml.ResourceDictionary> 是常見的工作，通常是藉由撰寫資源字典做為 XAML 頁面或個別 XAML 檔案的區域來完成。 資源字典及其使用方式是一個大範圍的概念，不在本主題的討論範圍內。 如需詳細資訊，請參閱 [ResourceDictionary 與 XAML 資源參考](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)。

## <a name="xaml-and-xml"></a>XAML 與 XML

XAML 語言基本上是以 XML 語言為基礎。 但 XAML 大幅擴充了 XML。 具體來說，由於它與支援類型概念的關係，因此處理結構描述概念的方式相當不同，並且會新增語言元素，像是附加的成員與標記延伸。 **xml:lang** 在 XAML 中有效，但會影響執行階段而非剖析行為，並且通常會產生別名到架構層級屬性。 如需詳細資訊，請參閱<xref:Windows.UI.Xaml.FrameworkElement.Language%2A?displayProperty=nameWithType>。 **xml:base** 在標記中有效，但剖析器會忽略它。 **xml:space** 有效，但只與 [XAML 與空格](xaml-and-whitespace.md)主題中描述的案例有關。 **encoding** 屬性在 XAML 中有效。 僅支援 UTF-8 與 UTF-16 編碼。 不支援 UTF-32 編碼。

###  <a name="case-sensitivity-in-xaml"></a>XAML 中的區分大小寫功能

XAML 會區分大小寫。 這是 XAML 以 XML 做為基礎的另一個結果，XML 會區分大小寫。 XAML 元素的名稱與屬性會區分大小寫。 屬性值可能會區分大小寫；這取決於特定屬性的屬性值如何處理。 例如，如果屬性值宣告列舉的某個成員名稱，轉換成員名稱字串類型以傳回列舉成員值的內建行為不會區分大小寫。 相反地，**Name** 屬性的值與用在以 **Name** 屬性宣告的名稱為基礎的物件的公用程式方法，會以區分大小寫的方式來處理名稱字串。

## <a name="xaml-namescopes"></a>XAML 命名範圍

XAML 語言定義 XAML 命名範圍的概念。 XAML 命名範圍概念會影響 XAML 處理器應如何處理套用到 XAML 元素的 **x:Name** 或 **Name** 的值，尤其是需要名稱做為唯一識別碼的範圍。 XAML 命名範圍在不同的主題中有更詳細的說明；請參閱 [XAML 命名範圍](xaml-namescopes.md)。

## <a name="the-role-of-xaml-in-the-development-process"></a>XAML 在開發程序中的角色

XAML 在應用程式開發程序中扮演了數個重要的角色。

- 如果您是使用 C#、Visual Basic 或 C++/CX 進行程式設計，XAML 是宣告應用程式 UI 以及該 UI 中元素的主要格式。 通常您的專案至少會有一個 XAML 檔案來表示應用程式最初顯示 UI 的頁面隱喻。 其他的 XAML 檔案可以為瀏覽 UI 宣告其他頁面。 其他 XAML 檔案可以宣告資源，如範本或樣式。
- 您使用 XAML 格式來宣告套用到應用程式中控制項和 UI 的樣式和範本。
- 為了將現有控制項範本化，或如果您定義的控制項會提供預設範本做為控制項套件的一部分，您可能會使用樣式和範本。 當您使用它來定義樣式和範本時，相關的 XAML 通常會宣告為具有 <xref:Windows.UI.Xaml.ResourceDictionary> 根的離散 XAML 檔案。
- XAML 是設計工具支援的常見格式，可以在不同的設計工具應用程式之間建立應用程式 UI 和交換 UI 設計。 最特別的是，應用程式的 XAML 可以在不同的 XAML 設計工具 (或工具內的設計視窗) 之間交換使用。
- 有數種其他技術也可以在 XAML 中定義基本 UI。 和 Windows Presentation Foundation (WPF) XAML 與 Microsoft Silverlight XAML 類似，Windows 執行階段的 XAML 會使用其共用預設 XAML 命名空間的相同 URI。 Windows 執行階段的 XAML 詞彙與 Silverlight 也會使用的 UI XAML 詞彙大幅重疊，而 WPF 使用的 UI XAML 詞彙重疊程度則較低。 因此，XAML 會為 UI 升級原先定義前導技術 (也使用 XAML) 的有效移轉路徑。
- XAML 定義 UI 的視覺外觀，而關聯的程式碼後置檔案定義邏輯。 您不用變更程式碼後置中的邏輯就可以調整 UI 設計。 XAML 簡化了設計人員與開發人員之間的工作流程。
- 因為支援 XAML 語言的視覺化設計工具及設計表面的豐富性，所以 XAML 也支援早期開發階段中的快速 UI 原型設計。

視您在開發程序中的角色而定，您不一定會與 XAML 有很多的互動。 您與 XAML 檔案互動的程度也取決於您所使用的開發環境、是否使用互動式設計環境功能 (如工具箱和屬性編輯器)，以及您 Windows 執行階段應用程式的範圍和用途。 儘管如此，在應用程式的開發期間，您會使用文字或 XML 編輯器在元素層級編輯 XAML 檔案。 使用這項資訊，您可以在文字或 XML 表示法中放心編輯 XAML，並在您的 Windows 執行階段應用程式的工具、標記編譯作業或執行階段使用 XAML 時，維持該 XAML 檔案宣告的有效性。

## <a name="optimize-your-xaml-for-load-performance"></a>針對載入效能將您的 XAML 最佳化

以下是使用效能最佳做法定義 XAML 中 UI 元素的一些祕訣。 這些祕訣中有許多與使用 XAML 資源相關，但是基於方便性，所以在一般 XAML 概觀這裡列出。 如需有關 XAML 資源的詳細資訊，請參閱 [ResourceDictionary 與 XAML 資源參考](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)。 如需更多有關效能的提示，包含特意示範一些您應該在 XAML 中避免的不良效能做法的 XAML，請參閱[最佳化您的 XAML 標記](../debug-test-perf/optimize-xaml-loading.md)。

- 如果您經常在 XAML 中使用相同的色彩筆刷，請將 <xref:Windows.UI.Xaml.Media.SolidColorBrush> 定義為資源，而不是每次使用已命名的色彩做為屬性值。
- 如果您在多個 UI 頁面上使用相同的資源，請考慮在 <xref:Windows.UI.Xaml.Application.Resources%2A> 中定義它，而不是在每個頁面上。 相反地，如果只有一個頁面使用某個資源，就不要在 **Application.Resources** 中定義該資源，而是只針對需要使用的頁面定義。 這在設計 app 期間用來分解 XAML 非常好用，同時對於剖析 XAML 期間的效能非常有助益。
- 針對您應用程式封裝的資源檢查未使用的資源 (具有索引鍵，但是在使用它的應用程式中並沒有 [StaticResource](staticresource-markup-extension.md) 參考的資源)。 請先從您的 XAML 中移除這些資源，然後再發行您的應用程式。
- 如果您使用的是提供設計資源（<xref:Windows.UI.Xaml.ResourceDictionary.MergedDictionaries%2A>）的個別 XAML 檔案，請考慮從這些檔案批註或移除未使用的資源。 即使您在一個以上的應用程式中使用共用的 XAML 起點或是使用共用的 XAML 起點來為所有應用程式提供常見資源，每次仍是由您的應用程式封裝 XAML 資源，而且可能必須載入它們。
- 不要定義組合項目中不需要的 UI 元素，盡可能使用預設的控制項範本 (這些範本已經針對載入效能進行測試與驗證)。
- 使用 <xref:Windows.UI.Xaml.Controls.Border> 的容器，而不是刻意 overdraws UI 元素。 基本上，請不要多次繪製同一個像素。 如需過度繪製的詳細資訊，以及如何進行測試，請參閱 <xref:Windows.UI.Xaml.DebugSettings.IsOverdrawHeatMapEnabled?displayProperty=nameWithType>。
- 使用 <xref:Windows.UI.Xaml.Controls.ListView> 或 <xref:Windows.UI.Xaml.Controls.GridView>的預設專案範本;這些是特殊的展示**者**邏輯，可解決建立大量清單專案的視覺化樹狀結構時的效能問題。

## <a name="debug-xaml"></a>Debug XAML

因為 XAML 是一種標記語言，所以在 Microsoft Visual Studio 內一些偵錯的典型策略就無法使用。 例如，您無法在 XAML 檔案內設定中斷點。 不過，仍處於開發應用程式階段時，還是有其他技術可以協助您偵錯 UI 定義或其他 XAML 標記的問題。

當 XAML 檔案有問題的時候，最典型的結果是有些系統或您的應用程式會擲回 XAML 剖析例外狀況。 出現 XAML 剖析例外狀況時，XAML 剖析器載入的 XAML 無法建立有效的物件樹狀結構。 在有些情況下，像是 XAML 呈現載入為根 Visual 的應用程式第一個「頁面」時，XAML 剖析例外狀況是不可復原的。

XAML 通常是在 IDE 內 (如 Visual Studio) 以及它的其中一個 XAML 設計表面內進行編輯。 Visual Studio 通常可在您編輯的時候提供設計階段驗證及 XAML 來源的錯誤檢查。 例如，當您輸入錯誤的屬性值時，它會立即在 XAML 文字編輯器中顯示「波浪線」，您甚至不用等 XAML 編譯傳送，就能看到 UI 定義中有錯誤。

一旦 app 實際執行之後，如果設計階段未偵測到任何 XAML 剖析錯誤，通用語言執行階段 (CLR) 就會將它們報告為 [**XamlParseException**](https://docs.microsoft.com/dotnet/api/Windows.UI.Xaml.markup.xamlparseexception?view=dotnet-uwp-10.0)。 如需可在執行階段 **XamlParseException** 執行哪些動作的詳細資訊，請參閱 [C# 或 Visual Basic 中 Windows 執行階段 app 的例外狀況處理](https://docs.microsoft.com/previous-versions/windows/apps/dn532194(v=win.10))。

> [!NOTE]
> 針對程式碼C++使用/cx 的應用程式不會取得特定的[**XamlParseException**](https://docs.microsoft.com/dotnet/api/Windows.UI.Xaml.markup.xamlparseexception?view=dotnet-uwp-10.0)。 但是，例外狀況中的訊息可清楚說明錯誤來源是與 XAML 相關，而且其中包含內容資訊，例如 XAML 檔案中的行號，就像 **XamlParseException** 一樣。

如需偵錯 Windows 執行階段 app 的詳細資訊，請參閱[啟動偵錯工作階段](/visualstudio/debugger/start-a-debugging-session-for-a-store-app-in-visual-studio-vb-csharp-cpp-and-xaml?view=vs-2015)。
