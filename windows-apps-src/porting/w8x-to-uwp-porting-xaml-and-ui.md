---
author: mcleblanc
description: 以宣告式 XAML 標記形式定義 UI 的做法可以極順利地從通用 8.1 app 轉譯至通用 Windows 平台 (UWP) app。
title: 將 Windows Runtime 8.x XAML 與 UI 移植到 UWP
ms.assetid: 78b86762-7359-474f-b1e3-c2d7cf9aa907
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 19a6ef29265c22d1bb02464a76ab20e487c67ce4
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.locfileid: "211097"
---
# <a name="porting-windows-runtime-8x-xaml-and-ui-to-uwp"></a>將 Windows Runtime 8.x XAML 與 UI 移植到 UWP

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

前一個主題是[疑難排解](w8x-to-uwp-troubleshooting.md)。

以宣告式 XAML 標記形式定義 UI 的做法可以極順利地從通用 8.1 應用程式轉譯至通用 Windows 平台 (UWP) 應用程式。 您會發現大部分標記都相容，雖然您可能需要對系統的「資源」索引鍵或您使用的自訂範本進行一些調整。 檢視模型中的命令式程式碼需要少許變更或可保持不變。 甚至還可以直接移植展示層中操作 UI 元素的許多或大多數命令式程式碼。

## <a name="imperative-code"></a>命令式程式碼

如果您只是想要到達專案建置階段，您可以將任何非必要程式碼標成註解或清除。 然後逐一查看問題，並參閱本節 (與上一個主題：[疑難排解](w8x-to-uwp-troubleshooting.md)) 中的下列主題，直到任何建置與執行階段問題都已解決，且您的移植已完成為止。

## <a name="adaptiveresponsive-ui"></a>調適型/回應式 UI

因為您的 app 可以在許多不同的裝置上執行，每個裝置都有其獨特的螢幕大小與解析度，因此您將會想要透過最少的步驟來完成您的 app 移植，並讓您的 UI 能夠在那些裝置上呈現最佳外觀。 您可以使用調適型 Visual State Manager 功能來動態偵測視窗大小及變更回應配置，Bookstore2 案例研究主題的[調適型 UI](w8x-to-uwp-case-study-bookstore2.md) 中已經提供此做法的範例。

## <a name="back-button-handling"></a>[返回] 按鈕處理

針對通用 8.1 應用程式，Windows 市集應用程式和 Windows Phone 市集應用程式對於您顯示的 UI，以及您為返回按鈕處理的事件有不同的方法。 但是，對於 Windows 10 應用程式，您可以在應用程式中使用單一方法。 在行動裝置上，按鈕會在裝置上以電容式按鈕的方式，或以殼層中的按鈕的方式提供您使用。 在傳統型裝置上，您可以在只要能夠於應用程式中進行反向瀏覽時，就在應用程式組建區塊中加入一個按鈕，而且這會顯示在視窗化應用程式的標題列中，或平板電腦模式的工作列中。 返回按鈕事件是所有裝置系列的一個通用概念，且以硬體或軟體方式實作的按鈕都可以引發相同的 [**BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893596) 事件。

下面的範例適用於所有裝置系列，且非常適合在可將相同處理套用至所有頁面以及您不需要確認瀏覽 (例如，提供未儲存變更的警告) 的情況下使用。

```csharp
   // app.xaml.cs

    protected override void OnLaunched(LaunchActivatedEventArgs e)
    {
        [...]

        Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += App_BackRequested;
        rootFrame.Navigated += RootFrame_Navigated;
    }

    private void RootFrame_Navigated(object sender, NavigationEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        // Note: On device families that have no title bar, setting AppViewBackButtonVisibility can safely execute 
        // but it will have no effect. Such device families provide back button UI for you.
        if (rootFrame.CanGoBack)
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Visible;
        }
        else
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Collapsed;
        }
    }

    private void App_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        if (rootFrame.CanGoBack)
        {
            rootFrame.GoBack();
        }
    }
```

也有適用於所有裝置系列，並以程式控制方式控制現有應用程式的單一方法。

```csharp
   Windows.UI.Xaml.Application.Current.Exit();
```

## <a name="charms"></a>常用鍵

您不需要變更任何與常用鍵整合的程式碼，但需要將一些 UI 新增到應用程式，以取代不屬於 Windows 10 殼層一部分的常用鍵列。 在 Windows 10 上執行的通用 8.1 應用程式，在應用程式的標題列中，會有由系統轉譯組件區塊所提供的取代 UI。

## <a name="controls-and-control-styles-and-templates"></a>控制項，以及控制項樣式和範本

在 Windows 10 上執行的通用 8.1 應用程式將保留與控制項相關的 8.1 外觀和行為。 但是，當您將應用程式移植到 Windows 10 應用程式時，會在外觀和行為上產生一些需注意的差異。 適用於 Windows 10 應用程式的控制項架構和設計基本上不會變更，因此，變更大部分都會與[設計語言](#design-language-in-windows-10)、簡化，以及可用性的改進有關。

**注意** PointerOver 視覺狀態在 Windows 10 應用程式和 Windows 市集應用程式的自訂樣式/範本中是相關的，但在 Windows Phone 市集應用程式中則不相關。 基於這個原因 (而且因為 Windows 10 app 支援的系統資源索引鍵的緣故)，我們建議您在將 app 移植到 Windows 10 時，重複使用 Windows 市集應用程式中的自訂樣式/範本。
如果您想要確定您的自訂樣式/範本使用最新的一組視覺狀態，並且能夠從您對預設樣式/範本所進行的效能改進中獲益，請編輯新 Windows 10 預設範本的複本，然後在該複本中重新套用自訂項目。 效能改進的其中一個範例是，已移除先前括住 **ContentPresenter** 的任何 **Border** 或面板，而且子元素現在會呈現框線。

以下是控制項變更的一些更具體的範例。

| 控制項名稱 | 變更 |
|--------------|--------|
| **AppBar**   | 如果您使用的是 **AppBar** 控制項 (建議改用 [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927))，則預設在 Windows 10 應用程式中不會隱藏該控制項。 您可以使用 [**AppBar.ClosedDisplayMode**](https://msdn.microsoft.com/library/windows/apps/dn633872) 屬性加以控制。 |
| **AppBar**、[**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) | 在 Windows 10 應用程式中，**AppBar** 和 [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) 都有一個 [**查看更多**] 按鈕 (省略符號)。 |
| [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) | 在 Windows 市集應用程式中，永遠可以看到 [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) 的次要命令。 在 Windows Phone 市集應用程式以及 Windows 10 應用程式中，不會顯示這些命令，直到命令列開啟為止。 |
| [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) | 針對 Windows Phone 市集應用程式，[**CommandBar.IsSticky**](https://msdn.microsoft.com/library/windows/apps/hh701944) 的值不會影響命令列是否會消失關閉。 針對 Windows 10 應用程式，如果將 **IsSticky** 設定為 True，則 **CommandBar** 會略過消失關閉手勢。 |
| [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) | 在 Windows 10 應用程式中，[**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) 不會處理 [**EdgeGesture.Completed**](https://msdn.microsoft.com/library/windows/apps/hh701622) 事件，也不會處理 [**UIElement.RightTapped**](https://msdn.microsoft.com/library/windows/apps/br208984) 事件。 它不會回應點選也不會回應向上撥動。 您仍然可以選擇處理這些事件，並設定 [**IsOpen**](https://msdn.microsoft.com/library/windows/apps/hh701939)。 |
| [**DatePicker**](https://msdn.microsoft.com/library/windows/apps/dn298584)、[**TimePicker**](https://msdn.microsoft.com/library/windows/apps/dn299280) | 檢閱在視覺變更為 [**DatePicker**](https://msdn.microsoft.com/library/windows/apps/dn298584) 和 [**TimePicker**](https://msdn.microsoft.com/library/windows/apps/dn299280) 之後您應用程式的外觀。 針對在行動裝置上執行的 Windows 10 應用程式，這些控制項便不再瀏覽到選取頁面，而是改用會消失關閉的快顯視窗。 |
| [**DatePicker**](https://msdn.microsoft.com/library/windows/apps/dn298584)、[**TimePicker**](https://msdn.microsoft.com/library/windows/apps/dn299280) | 在 Windows 10 應用程式中，您無法將 [**DatePicker**](https://msdn.microsoft.com/library/windows/apps/dn298584) 或 [**TimePicker**](https://msdn.microsoft.com/library/windows/apps/dn299280) 放在飛出視窗內。 如果您希望在快顯類型控制項中顯示這些控制項，則您可以使用 [**DatePickerFlyout**](https://msdn.microsoft.com/library/windows/apps/dn625013) 和 [**TimePickerFlyout**](https://msdn.microsoft.com/library/windows/apps/dn608313)。 |
| **GridView**、**ListView** | 針對 **GridView**/**ListView**，請參閱 [GridView 和 ListView 變更](#gridview-and-listview-changes)。 |
| [**中心**](https://msdn.microsoft.com/library/windows/apps/dn251843) | 在 Windows Phone 市集應用程式中，[**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843) 控制項會從最後一個區段迴繞到第一個區段。 在 Windows 市集應用程式和 Windows 10 應用程式中，中樞區段不會迴繞。 |
| [**中樞**](https://msdn.microsoft.com/library/windows/apps/dn251843) | 在 Windows Phone 市集應用程式中，[**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843) 控制項的背景影像不會以相對於中樞區段的視差移動。 在 Windows 市集應用程式和 Windows 10 應用程式中，將不會使用視差。 |
| [**中樞**](https://msdn.microsoft.com/library/windows/apps/dn251843)  | 在通用 8.1 app 中，[**HubSection.IsHeaderInteractive**](https://msdn.microsoft.com/library/windows/apps/dn251917) 屬性會讓區段標頭—和旁邊呈現的 ＞ 形箭號字符—變成互動式。 在 Windows 10 應用程式中，標頭旁邊有一個互動式的 [查看更多] 預示性，但標頭本身不是互動式。 **IsHeaderInteractive** 仍然會決定互動是否會引發 [**Hub.SectionHeaderClick**](https://msdn.microsoft.com/library/windows/apps/dn251953) 事件。 |
| **MessageDialog** | 如果您使用的是 **MessageDialog**，則請考慮改用更有彈性的 [**ContentDialog**](https://msdn.microsoft.com/library/windows/apps/dn633972)。 另請參閱 [XAML UI 基本知識](http://go.microsoft.com/fwlink/p/?linkid=619992)範例。 |
| **ListPickerFlyout**、**PickerFlyout**  | **ListPickerFlyout** 和 **PickerFlyout** 對於 Windows 10 應用程式已過時。 如需單一選擇飛出視窗，請使用 [**MenuFlyout**](https://msdn.microsoft.com/library/windows/apps/dn299030)；如需更複雜的體驗，請使用 [**Flyout**](https://msdn.microsoft.com/library/windows/apps/dn279496)。 |
| [**PasswordBox**](https://msdn.microsoft.com/library/windows/apps/br227519) | [**PasswordBox.IsPasswordRevealButtonEnabled**](https://msdn.microsoft.com/library/windows/apps/hh702579) 屬性在 Windows 10 應用程式中已過時，因此設定該屬性沒有任何作用。 請改用預設為 **Peek** 的 [**PasswordBox.PasswordRevealMode**](https://msdn.microsoft.com/library/windows/apps/dn890867) (其中會顯示一個眼睛字符，就像在 Windows 市集應用程式中一樣)。 另請參閱[密碼方塊的指導方針](https://msdn.microsoft.com/library/windows/apps/dn596103)。 |
| [**樞紐分析**](https://msdn.microsoft.com/library/windows/apps/dn608241) | [**Pivot**](https://msdn.microsoft.com/library/windows/apps/dn608241) 控制項現在是通用的，不再受限於只能在行動裝置上使用。 |
| [**SearchBox**](https://msdn.microsoft.com/library/windows/apps/dn252771) | 雖然 [**SearchBox**](https://msdn.microsoft.com/library/windows/apps/dn252803) 可在通用裝置系列中實作，但在行動裝置上無法使用所有功能。 請參閱 [SearchBox 已取代為 AutoSuggestBox](#searchbox-deprecated-in-favor-of-autosuggestbox)。 |
| **SemanticZoom** | 針對 **SemanticZoom**，請參閱 [SemanticZoom 變更](#semanticzoom-changes)。 |
| [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527)  | [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527) 的某些預設屬性已經變更。 [**HorizontalScrollMode**](https://msdn.microsoft.com/library/windows/apps/br209549) 為 **Auto**、[**VerticalScrollMode**](https://msdn.microsoft.com/library/windows/apps/br209589) 為 **Auto**，而 [**ZoomMode**](https://msdn.microsoft.com/library/windows/apps/br209601) 為 **Disabled**。 如果新的預設值不適用於您的 app，則您可以在樣式上進行變更，或當做控制項本身的本機值變更。  |
| [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) | 在 Windows 市集應用程式中，[**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 預設關閉拼字檢查。 在 Windows Phone 市集應用程式和 Windows 10 應用程式中，它預設為開啟。 |
| [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) | [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 的預設字型大小已從 11 變更為 15。 |
| [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) | [**TextBox.TextReadingOrder**](https://msdn.microsoft.com/library/windows/apps/dn252859) 的預設值已從 **Default** 變更為 **DetectFromContent**。 如果不適當，則使用 **UseFlowDirection**。 **Default** 已過時。 |
| 各種 | 輔色會套用到 Windows Phone 市集應用程式和 Windows 10 應用程式，但不會套用到 Windows 市集應用程式。  |

如需 UWP app 控制項的詳細資訊，請參閱[依功能分類的控制項](https://msdn.microsoft.com/library/windows/apps/mt185405)、[控制項清單](https://msdn.microsoft.com/library/windows/apps/mt185406)，以及[控制項的指導方針](https://msdn.microsoft.com/library/windows/apps/dn611856)。

##  <a name="design-language-in-windows-10"></a>Windows 10 中的設計語言

通用 8.1 應用程式和 Windows 10 應用程式之間的設計語言，有一些細微但卻很重要的差異。 如需所有詳細資訊，請參閱[設計](http://dev.windows.com/design)。 儘管設計語言會變更，但我們的設計原則仍會保持一致：留意細節，但為了簡單起見，儘量將重點放在內容不是組件區塊、將視覺元素降至最低，並保留數位網域的驗證；使用視覺層次，特別是使用印刷格式；設計格線；以及使用流暢的動畫讓您的體驗變得更生動。

## <a name="effective-pixels-viewing-distance-and-scale-factors"></a>有效的像素、檢視距離及縮放比例

檢視像素以前是從裝置的實際實體大小和解析度抽取 UI 元素大小與配置的方式。 檢視像素現在已經進化為有效像素，以下將說明此術語、它的意義及其提供的額外價值。

「解析度」一詞是指像素密度 (而非一般認為的像素計數) 的度量。 「有效解析度」係指在特定檢視距離與裝置之實體像素大小 (像素密度為實體像素大小的倒數) 的差異下，構成影像或字符的實體像素解析成視覺的方式。 有效解析度以使用者為中心，是建置體驗時很好的衡量標準。 藉由了解所有因素及控制 UI 元素的大小，您就能讓使用者享有良好的體驗。

不同的裝置有不同數量的有效像素範圍，範圍從適用於最小型裝置的 320 epx 到適用於中等大小之監視器的 1024 epx，還有遠遠超過此寬度的更高像素。 您只需一如往常繼續使用自動調整大小元素與動態配置面板。 在某些情況下，也會將 XAML 標記中的 UI 元素屬性設定為固定大小。 縮放比例會根據您的應用程式是在何種裝置上執行，以及使用者所做的顯示設定，自動套用到該應用程式。 此外，該縮放比例還可用來讓大小固定的任何 UI 元素持續在各種不同螢幕大小上，為使用者呈現大約是固定大小的觸控 (及讀取) 目標。 和動態配置一起使用，您的 UI 將不只是在不同裝置上進行光學縮放。 它會改為執行必要的動作，以便將適當數量的內容放到可用空間中。

為使您的應用程式在所有顯示器上都能有最佳體驗，建議您在某個大小範圍內個別建立適用於各個特定縮放比例的點陣圖資產。 提供 100%、200% 及 400% 的縮放比例 (並以此順序做為其優先順序)，可讓您在大部分情況下利用所有的中繼縮放係數獲得絕佳的結果。

**注意** 如果您無法建立多種大小的資產 (無論原因為何)，請建立縮放比例為 100% 的資產。 在 Microsoft Visual Studio 中，UWP app 的預設專案範本只會提供一種大小的商標資產 (磚影像和標誌)，但其縮放比例不是 100%。 在為您自己的應用程式製作資產時，請遵循本節中的指導方針，提供 100%、 200%及 400% 的大小，並使用資產套件。

如果您有複雜的圖檔，您可以用更多大小來提供您的資產。 如果您開始使用向量藝術，則使用任何縮放比例來產生高品質的資產，相對來說就容易許多。

不建議您嘗試支援所有縮放比例，但 Windows 10 應用程式的完整縮放比例清單為 100%、125%、150%、200%、250%、300% 及 400%。 若有提供，市集將會為每個裝置挑選大小正確的資產，同時只會下載那些資產。 市集會根據裝置的 DPI 來選取要下載的資產。 您能以像是 140% 與 220% 的縮放比例重複使用 Windows 市集應用程式的資產，但您的應用程式將以新的縮放比例執行，因此必然會稍微縮放點陣圖比例。 在各種裝置測試您的應用程式，看看您是否對結果滿意。

您可能會重複使用來自 Windows 市集應用程式的 XAML 標記，其中的常值維度值會在標記中使用 (可能用於圖形大小或其他元素，也可能用於印刷格式)。 但在某些情況下，較大的縮放比例會用於 Windows 10 應用程式適用的裝置，而非通用 8.1 應用程式適用的裝置 (例如，先前使用 140%，現在會使用 150%，而先前使用 180%，現在會使用 200%)。 因此，如果您發現這些常值目前在 Windows 10 上太大，則請嘗試將它們乘以 0.8。 如需詳細資訊，請參閱[適用於 UWP App 的回應式設計入門](https://msdn.microsoft.com/library/windows/apps/dn958435)。

## <a name="gridview-and-listview-changes"></a>GridView 和 ListView 變更

已針對 [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705) 的預設樣式 setter 進行數個變更，使控制項可垂直捲動 (而不是水平捲動，如同它先前預設的動作一樣)。 如果您曾編輯過專案中預設樣式的複本，則您的複本將不會擁有這些變更，因此您需要進行手動更新。 以下是變更的清單。

-   適用於 [**ScrollViewer.HorizontalScrollBarVisibility**](https://msdn.microsoft.com/library/windows/apps/br209547) 的 setter 已從 **Auto** 變更為 **Disabled**。
-   適用於 [**ScrollViewer.VerticalScrollBarVisibility**](https://msdn.microsoft.com/library/windows/apps/br209587) 的 setter 已從 **Disabled** 變更為 **Auto**。
-   適用於 [**ScrollViewer.HorizontalScrollMode**](https://msdn.microsoft.com/library/windows/apps/br209549) 的 setter 已從 **Enabled** 變更為 **Disabled**。
-   適用於 [**ScrollViewer.VerticalScrollMode**](https://msdn.microsoft.com/library/windows/apps/br209589) 的 setter 已從 **Disabled** 變更為 **Enabled**。
-   在適用於 [**ItemsPanel**](https://msdn.microsoft.com/library/windows/apps/br242826) 的 setter 中，[**ItemsWrapGrid.Orientation**](https://msdn.microsoft.com/library/windows/apps/dn298907) 的值已從 **Vertical** 變更為 **Horizontal**。

如果這最後一個變更 (變更為 **Orientation**) 看似矛盾，則請記住，我們正在討論的是自動換行方格。 水平方向的自動換行方格 (新值) 類似橫書的書寫系統，並在頁面結尾向下分隔設定到下一行。 這類文字的頁面可以垂直捲動。 相反地，垂直方向的自動換行方格 (先前的值) 類似直書的書寫系統，因此可以水平捲動。

以下是 Windows 10 中已變更或不支援的 [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705) 和 [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) 部分。

-   Windows 10 應用程式不支援 [**IsSwipeEnabled**](https://msdn.microsoft.com/library/windows/apps/hh702518) 屬性 (僅適用於 Windows 市集應用程式)： API 仍然存在，但設定它不會產生任何作用。 支援所有先前的選取手勢，但向下撥動 (不支援此手勢，因為資料會顯示它是無法探索的) 和按一下滑鼠右鍵 (其保留來顯示操作功能表) 除外。
-   Windows 10 應用程式不支援 [**ReorderMode**](https://msdn.microsoft.com/library/windows/apps/dn625099) 屬性 (僅適用於 Windows Phone 市集應用程式)： API 仍然存在，但設定它不會產生任何作用。 相反地，在您的 **GridView** 或 **ListView** 上，將 [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/br208912) 和 [**CanReorderItems**](https://msdn.microsoft.com/library/windows/apps/br242882) 設定為 True，之後使用者就能使用按住不放 (或按一下並拖曳) 手勢重新排序。
-   針對 Windows 10 進行開發時，在您的項目容器樣式中，同時針對 [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) 和 [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705) 使用 [**ListViewItemPresenter**](https://msdn.microsoft.com/library/windows/apps/dn298500) 而不是 [**GridViewItemPresenter**](https://msdn.microsoft.com/library/windows/apps/dn279298)。 如果您編輯預設項目容器樣式的複本，則可取得正確的類型。
-   Windows 10 應用程式的選取視覺效果已經變更。 如果您將 [**SelectionMode**](https://msdn.microsoft.com/library/windows/apps/br242915) 設定為 **Multiple**，則根據預設，會為每個項目呈現一個核取方塊。 **ListView** 項目的預設設定表示核取方塊已內嵌配置於項目旁邊，因此，其餘項目所佔用的空間會稍微降低並移動。 針對 **GridView** 項目，核取方塊預設會重疊於項目上方。 但在任一種情況下，您可以控制核取方塊 (使用 [**CheckMode**](https://msdn.microsoft.com/library/windows/apps/dn913923) 屬性) 的配置 (內嵌或重疊)，以及它們是否會在 [**ListViewItemPresenter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.listviewitempresenter.aspx) 元素上完全顯示 (以及 [**SelectionCheckMarkVisualEnabled**](https://msdn.microsoft.com/library/windows/apps/dn298541) 屬性)，此元素位於項目容器樣式內部，如下列範例所示。
-   在 Windows 10 中，每個項目在 UI 虛擬化期間會引發 [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/dn298914) 事件兩次：一次用於回收，一次用於重複使用。 如果 [**InRecycleQueue**](https://msdn.microsoft.com/library/windows/apps/dn279443) 的值是 **true**，且您沒有特殊回收工作需要執行，您可以立即結束事件處理常式，以確保在重複使用相同項目時可以重新進入此處理常式 (屆時 **InRecycleQueue** 將會是 **false**)。

```xml
<Style x:Key="CustomItemContainerStyle" TargetType="ListViewItem|GridViewItem">
    ...
    <Setter.Value>
        <ControlTemplate TargetType="ListViewItem|GridViewItem">
            <ListViewItemPresenter CheckMode="Inline|Overlay" ... />
        </ControlTemplate>
    </Setter.Value>
    ...
</Style>
```

![具有內嵌核取方塊的 ListViewItemPresenter](images/w8x-to-uwp-case-studies/ui-listviewbase-cb-inline.jpg)

具有內嵌核取方塊的 ListViewItemPresenter

![具有重疊核取方塊的 ListViewItemPresenter](images/w8x-to-uwp-case-studies/ui-listviewbase-cb-overlay.jpg)

具有重疊核取方塊的 ListViewItemPresenter

-   移除用以選取的向下撥動和使用滑鼠右鍵按一下手勢之後 (基於上述因素)，互動模式即已變更，而其中一個結果是 [**ItemClick**](https://msdn.microsoft.com/library/windows/apps/br242904) 和 [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) 事件不再互斥。 針對 Windows 10 應用程式檢閱您的案例，並決定是否要採用「選項」或「叫用」互動模型。 如需詳細資訊，請參與[如何變更互動模式](https://msdn.microsoft.com/library/windows/apps/xaml/hh780625)。
-   對於您用來設定 [**ListViewItemPresenter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.listviewitempresenter.aspx) 樣式的屬性有一些變更。 新的屬性是 [**CheckBoxBrush**](https://msdn.microsoft.com/library/windows/apps/dn913905)、[**PressedBackground**](https://msdn.microsoft.com/library/windows/apps/dn913931)、[**SelectedPressedBackground**](https://msdn.microsoft.com/library/windows/apps/dn913937) 及 [**FocusSecondaryBorderBrush**](https://msdn.microsoft.com/library/windows/apps/dn898370)。 要針對 Windows 10 應用程式略過的屬性是 [**Padding**](https://msdn.microsoft.com/library/windows/apps/dn424775) (改用 [**ContentMargin**](https://msdn.microsoft.com/library/windows/apps/dn424773))、[**CheckHintBrush**](https://msdn.microsoft.com/library/windows/apps/dn298504)、[**CheckSelectingBrush**](https://msdn.microsoft.com/library/windows/apps/dn298506)、[**PointerOverBackgroundMargin**](https://msdn.microsoft.com/library/windows/apps/dn424778)、[**ReorderHintOffset**](https://msdn.microsoft.com/library/windows/apps/dn298528)、[**SelectedBorderThickness**](https://msdn.microsoft.com/library/windows/apps/dn298533) 及 [**SelectedPointerOverBorderBrush**](https://msdn.microsoft.com/library/windows/apps/dn298539)。

下表說明 [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/br242919) 和 [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/hh738501) 控制項範本中視覺狀態和視覺狀態群組的變更。

| 8.1                 |                         | Windows 10        |                     |
|---------------------|-------------------------|-------------------|---------------------|
| CommonStates        |                         | CommonStates      |                     |
|                     | Normal                  |                   | Normal              |
|                     | PointerOver             |                   | PointerOver         |
|                     | Pressed                 |                   | Pressed             |
|                     | PointerOverPressed      |                   | [無法使用]       |
|                     | Disabled                |                   | [無法使用]       |
|                     | [無法使用]           |                   | PointerOverSelected |
|                     | [無法使用]           |                   | Selected            |
|                     | [無法使用]           |                   | PressedSelected     |
| [無法使用]       |                         | DisabledStates    |                     |
|                     | [無法使用]           |                   | Disabled            |
|                     | [無法使用]           |                   | 已啟用             |
| SelectionHintStates |                         | [無法使用]     |                     |
|                     | VerticalSelectionHint   |                   | [無法使用]       |
|                     | HorizontalSelectionHint |                   | [無法使用]       |
|                     | NoSelectionHint         |                   | [無法使用]       |
| [無法使用]       |                         | MultiSelectStates |                     |
|                     | [無法使用]           |                   | MultiSelectDisabled |
|                     | [無法使用]           |                   | MultiSelectEnabled  |
| SelectionStates     |                         | [無法使用]     |                     |
|                     | Unselecting             |                   | [無法使用]       |
|                     | Unselected              |                   | [無法使用]       |
|                     | UnselectedPointerOver   |                   | [無法使用]       |
|                     | UnselectedSwiping       |                   | [無法使用]       |
|                     | Selecting               |                   | [無法使用]       |
|                     | Selected                |                   | [無法使用]       |
|                     | SelectedSwiping         |                   | [無法使用]       |
|                     | SelectedUnfocused       |                   | [無法使用]       |

如果您擁有自訂的 [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/br242919) 或 [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/hh738501) 控制項範本，則可按照上述變更來檢閱範本。 建議您一開始先編輯新預設範本的複本，然後將您的自訂重新套用到其中。 如果您基於任何因素而無法執行該動作，且需要編輯現有的範本，則以下提供一些您或許可用來執行該動作的一般指導方針。

-   新增新的 MultiSelectStates 視覺狀態群組。
-   新增新的 MultiSelectDisabled 視覺狀態。
-   新增新的 MultiSelectEnabled 視覺狀態。
-   新增新的 DisabledStates 視覺狀態群組。
-   新增新的 Enabled 視覺狀態。
-   在 CommonStates 視覺狀態群組中，移除 PointerOverPressed 視覺狀態。
-   將 Disabled 視覺狀態移至 DisabledStates 視覺狀態群組。
-   新增新的 PointerOverSelected 視覺狀態。
-   新增新的 PressedSelected 視覺狀態。
-   移除 SelectedHintStates 視覺狀態群組。
-   在 SelectionStates 視覺狀態群組中，將 Selected 的視覺狀態移至 CommonStates 視覺狀態群組。
-   移除整個 SelectionStates 視覺狀態群組。

## <a name="localization-and-globalization"></a>當地語系化和全球化

您可以在 UWP app 專案中重複使用通用 8.1 專案的 Resources.resw 檔案。 完成複製檔案之後，請將該檔案新增到專案，並將 [**建置動作**] 設定為 [**PRIResource**] 以及將 [**複製到輸出目錄**] 設為 [**不要複製**]。 [**ResourceContext.QualifierValues**](https://msdn.microsoft.com/library/windows/apps/br206071) 主題說明如何根據裝置系列資源選擇因素載入裝置系列特定資源。

## <a name="play-to"></a>播放至

[**Windows.Media.PlayTo**](https://msdn.microsoft.com/library/windows/apps/br207025) 命名空間中的 API 對於 Windows 10 應用程式已過時，並由 [**Windows.Media.Casting**](https://msdn.microsoft.com/library/windows/apps/dn972568) API 取而代之。

## <a name="resource-keys-and-textblock-style-sizes"></a>資源索引鍵和 TextBlock 樣式大小

適用於 Windows 10 的設計語言已進化許多，因此變更了某些系統樣式。 在某些情況下，您會想要重新瀏覽檢視的視覺設計，使其與變更的樣式屬性彼此協調。

在其他情況下，不再支援資源索引鍵。 Visual Studio 中的 XAML 標記編輯器會醒目提示無法解析的資源索引鍵參考。 例如，XAML 標記編輯器會在樣式索引鍵 `ListViewItemTextBlockStyle` 的參考加上紅色波浪底線。 如果未修正該參考，當您嘗試將應用程式部署到模擬器或裝置上時，它將會立即終止。 因此，請務必注意 XAML 標記的正確性。 您會發現 Visual Studio 是攔截這類問題的絕佳工具。

對於仍然支援的索引鍵，設計語言的變更表示某些樣式設定的屬性已經變更。 例如，`TitleTextBlockStyle` 在 Windows 市集應用程式中將 **FontSize** 設定為 14.667px，並在 Windows Phone 市集應用程式中設定為 18.14px。 但是相同的樣式在 Windows 10 應用程式中，會將 **FontSize** 設定為更大的 24px。 檢閱您的設計和配置，並在適當的位置使用適當的樣式。 如需詳細資訊，請參閱[字型的指導方針](https://msdn.microsoft.com/library/windows/apps/hh700394.aspx)和[設計 UWP app](http://dev.windows.com/design)。

以下是不再支援的完整索引鍵清單：

-   CheckBoxAndRadioButtonMinWidthSize
-   CheckBoxAndRadioButtonTextPaddingThickness
-   ComboBoxFlyoutListPlaceholderTextOpacity
-   ComboBoxFlyoutListPlaceholderTextThemeMargin
-   ComboBoxHighlightedBackgroundThemeBrush
-   ComboBoxHighlightedBorderThemeBrush
-   ComboBoxHighlightedForegroundThemeBrush
-   ComboBoxInlinePlaceholderTextForegroundThemeBrush
-   ComboBoxInlinePlaceholderTextThemeFontWeight
-   ComboBoxItemDisabledThemeOpacity
-   ComboBoxItemHighContrastBackgroundThemeMargin
-   ComboBoxItemMinHeightThemeSize
-   ComboBoxPlaceholderTextBlockStyle
-   ComboBoxPlaceholderTextThemeMargin
-   CommandBarBackgroundThemeBrush
-   CommandBarForegroundThemeBrush
-   ContentDialogButton1HostPadding
-   ContentDialogButton2HostPadding
-   ContentDialogButtonsMinHeight
-   ContentDialogContentLandscapeWidth
-   ContentDialogContentMinHeight
-   ContentDialogDimmingColor
-   ContentDialogTitleMinHeight
-   ControlContextualInfoTextBlockStyle
-   ControlHeaderContentPresenterStyle
-   ControlHeaderTextBlockStyle
-   FlyoutContentPanelLandscapeThemeMargin
-   FlyoutContentPanelPortraitThemeMargin
-   GrabberMargin
-   GridViewItemMargin
-   GridViewItemPlaceholderBackgroundThemeBrush
-   GroupHeaderTextBlockStyle
-   HeaderContentPresenterStyle
-   HighContrastBlack
-   HighContrastWhite
-   HubHeaderCharacterSpacing
-   HubHeaderFontSize
-   HubHeaderMarginThickness
-   HubSectionHeaderCharacterSpacing
-   HubSectionHeaderFontSize
-   HubSectionHeaderMarginThickness
-   HubSectionMarginThickness
-   InlineWindowPlayPauseMargin
-   ItemTemplate
-   LeftFullWindowMargin
-   LeftMargin
-   ListViewEmptyStaticTextBlockStyle
-   ListViewItemContentTextBlockStyle
-   ListViewItemContentTranslateX
-   ListViewItemMargin
-   ListViewItemMultiselectCheckBoxMargin
-   ListViewItemSubheaderTextBlockStyle
-   ListViewItemTextBlockStyle
-   MediaControlPanelAudioThemeBrush
-   MediaControlPanelPhoneVideoThemeBrush
-   MediaControlPanelVideoThemeBrush
-   MediaControlPanelVideoThemeColor
-   MediaControlPlayPauseThemeBrush
-   MediaControlTimeRowThemeBrush
-   MediaControlTimeRowThemeColor
-   MediaDownloadProgressIndicatorThemeBrush
-   MediaErrorBackgroundThemeBrush
-   MediaTextThemeBrush
-   MenuFlyoutBackgroundThemeBrush
-   MenuFlyoutBorderThemeBrush
-   MenuFlyoutLandscapeThemePadding
-   MenuFlyoutLeftLandscapeBorderThemeThickness
-   MenuFlyoutPortraitBorderThemeThickness
-   MenuFlyoutPortraitThemePadding
-   MenuFlyoutRightLandscapeBorderThemeThickness
-   MessageDialogContentStyle
-   MessageDialogTitleStyle
-   MinimalWindowMargin
-   PasswordBoxCheckBoxThemeMargin
-   PhoneAccentBrush
-   PhoneBackgroundBrush
-   PhoneBackgroundColor
-   PhoneBaseBlackColor
-   PhoneBaseHighColor
-   PhoneBaseLowColor
-   PhoneBaseLowSolidColor
-   PhoneBaseMediumHighColor
-   PhoneBaseMediumMidColor
-   PhoneBaseMediumMidSolidColor
-   PhoneBaseMidColor
-   PhoneBaseWhiteColor
-   PhoneBorderThickness
-   PhoneButtonBasePressedForegroundBrush
-   PhoneButtonContentPadding
-   PhoneButtonFontWeight
-   PhoneButtonMinHeight
-   PhoneButtonMinWidth
-   PhoneChromeBrush
-   PhoneChromeColor
-   PhoneControlBackgroundColor
-   PhoneControlDisabledColor
-   PhoneControlForegroundColor
-   PhoneDisabledBrush
-   PhoneDisabledColor
-   PhoneFontFamilyLight
-   PhoneFontFamilySemiBold
-   PhoneForegroundBrush
-   PhoneForegroundColor
-   PhoneHighContrastSelectedBackgroundThemeBrush
-   PhoneHighContrastSelectedForegroundThemeBrush
-   PhoneImagePlaceholderColor
-   PhoneLowBrush
-   PhoneMidBrush
-   PhonePageBackgroundColor
-   PhonePivotLockedTranslation
-   PhonePivotUnselectedItemOpacity
-   PhoneRadioCheckBoxBorderBrush
-   PhoneRadioCheckBoxBrush
-   PhoneRadioCheckBoxCheckBrush
-   PhoneRadioCheckBoxPressedBrush
-   PhoneStrokeThickness
-   PhoneTextHighColor
-   PhoneTextLowColor
-   PhoneTextMidColor
-   PhoneTextOverAccentColor
-   PhoneTouchTargetLargeOverhang
-   PhoneTouchTargetOverhang
-   PivotHeaderItemPadding
-   PlaceholderContentPresenterStyle
-   ProgressBarHighContrastAccentBarThemeBrush
-   ProgressBarIndeterminateRectagleThemeSize
-   ProgressBarRectangleStyle
-   ProgressRingActiveBackgroundOpacity
-   ProgressRingElipseThemeMargin
-   ProgressRingElipseThemeSize
-   ProgressRingTextForegroundThemeBrush
-   ProgressRingTextThemeMargin
-   ProgressRingThemeSize
-   RichEditBoxTextThemeMargin
-   RightFullWindowMargin
-   RightMargin
-   ScrollBarMinThemeHeight
-   ScrollBarMinThemeWidth
-   ScrollBarPanningThumbThemeHeight
-   ScrollBarPanningThumbThemeWidth
-   SliderThumbDisabledBorderThemeBrush
-   SliderTrackBorderThemeBrush
-   SliderTrackDisabledBorderThemeBrush
-   TextBoxBackgroundColor
-   TextBoxBorderColor
-   TextBoxDisabledHeaderForegroundThemeBrush
-   TextBoxFocusedBackgroundThemeBrush
-   TextBoxForegroundColor
-   TextBoxPlaceholderColor
-   TextControlHeaderMarginThemeThickness
-   TextControlHeaderMinHeightSize
-   TextStyleExtraExtraLargeFontSize
-   TextStyleExtraLargePlusFontSize
-   TextStyleMediumFontSize
-   TextStyleSmallFontSize
-   TimeRemainingElementMargin

## <a name="searchbox-deprecated-in-favor-of-autosuggestbox"></a>SearchBox 已取代為 AutoSuggestBox

雖然 [**SearchBox**](https://msdn.microsoft.com/library/windows/apps/dn252803) 可在通用裝置系列中實作，但在行動裝置上無法使用所有功能。 請將 [**AutoSuggestBox**](https://msdn.microsoft.com/library/windows/apps/dn633874) 用在您的通用搜尋經驗。 以下說明在一般情況下如何透過 **AutoSuggestBox** 實作搜尋經驗。

使用者開始輸入後即會引發 **TextChanged** 事件，原因為 **UserInput**。 接著，您可以填入建議清單，並設定 [**AutoSuggestBox**](https://msdn.microsoft.com/library/windows/apps/dn633874) 的 **ItemsSource**。 使用者瀏覽清單時會引發 **SuggestionChosen** 事件 (且如果您已設定 **TextMemberDisplayPath**，文字方塊將會自動填入指定的屬性)。 當使用者以 Enter 鍵提交選擇時，會引發 **QuerySubmitted** 事件，此時您可以採取該建議動作 (在此情況下，很可能是瀏覽至其他含有指定內容之詳細資料的其他頁面)。 請注意，**SearchBoxQuerySubmittedEventArgs** 的 **LinguisticDetails** 和 **Language** 屬性已不再受支援 (有對等的 API 可支援該功能)。 **KeyModifiers** 也不再受支援。

[**AutoSuggestBox**](https://msdn.microsoft.com/library/windows/apps/dn633874) 也具有輸入法編輯器 (IME) 的支援。 而且，如果您想要顯示「尋找」圖示，您也可以這麼做 (與該圖示互動將會引發 **QuerySubmitted** 事件)。

```xml
   <AutoSuggestBox ... >
        <AutoSuggestBox.QueryIcon>
            <SymbolIcon Symbol="Find"/>
        </AutoSuggestBox.QueryIcon>
    </AutoSuggestBox>
```

另請參閱 [AutoSuggestBox 移植範例](http://go.microsoft.com/fwlink/p/?linkid=619996)。

## <a name="semanticzoom-changes"></a>SemanticZoom 變更

適用於 [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) 的縮放手勢已併入 Windows Phone 模型中，也就是可以點選或按一下群組標題 (因此，在傳統型電腦上，就不再顯示可用來進行縮放的減號按鈕能供性)。 我們現在可以在所有裝置上免費取得相同且一致的行為。 與 Windows Phone 模型不同的一個外觀差異是，放大檢視 (捷徑清單) 會取代縮小檢視，而不是與它重疊。 基於這個因素，您可以從縮小檢視中移除任何半透明的背景。

在 Windows Phone 市集應用程式中，縮小檢視會展開至螢幕的大小。 在 Windows 市集應用程式和 Windows 10 應用程式中，縮小檢視的大小限制為 **SemanticZoom** 控制項的界限。

在 Windows Phone 市集應用程式中，如果縮小檢視在其背景具有任何透明度，則縮小檢視 (z 順序) 背後的內容會穿透顯示。 在 Windows 市集應用程式和 Windows 10 應用程式中，看不到縮小檢視背後的任何內容。

在 Windows 市集應用程式中，當應用程式停用後和重新啟用時，會關閉縮小檢視 (若有顯示)，並改為顯示放大檢視。 在 Windows Phone 市集應用程式和 Windows 10 應用程式中，縮小檢視將會維持在顯示狀態。

在 Windows Phone 市集應用程式和 Windows 10 應用程式中，按下返回按鈕時，會關閉縮小檢視。 對於 Windows 市集應用程式，沒有內建的返回按鈕處理，因此此問題不適用。

## <a name="settings"></a>設定

Windows 執行階段 8.x **SettingsPane** 類別不適用於 Windows 10。 除了建置設定頁面，您應該改為提供使用者從您的 app 存取它的方式。 我們建議您在最上層公開這個應用程式設定頁面，做為瀏覽窗格中最後一個釘選的項目，但此處是一組完整的選項。

-   瀏覽窗格。 設定應為選項瀏覽清單的最後一個項目，而且釘選到底部。
-   Appbar/工具列 (在索引標籤檢視或樞紐分析配置內)。 設定應為 appbar 或工具列功能表飛出視窗中的最後一個項目。 不建議將設定做為瀏覽內的最上層項目的其中之一。
-   中樞。 設定應位於功能表飛出視窗內 (可位於應用程式列功能表或中樞配置內的工具列功能表中)。

也不建議將設定隱藏在主控制項/詳細資料控制項窗格中。

設定頁面應填滿應用程式的整個視窗，而且設定頁面也應是關於和意見反應的所在位置。 如需設定頁面的設計指導方針，請參閱[應用程式設定的指導方針](https://msdn.microsoft.com/library/windows/apps/hh770544)。

## <a name="text"></a>文字

文字 (或印刷樣式) 是 UWP app 中的一個重要層面，在移植時，您可以重新檢閱檢視的視覺設計，以確保它們不會與新的設計語言產生違和感。 請使用這些插圖說明來找出可用的通用 Windows 平台 (UWP) **TextBlock** 系統樣式。 尋找與您使用的 Windows Phone Silverlight 樣式相對應的樣式。 您也可以選擇建立自己的通用樣式，然後從 Windows Phone Silverlight 系統樣式將屬性複製到這些通用樣式中。

![適用於 Windows 10 app 的系統 textblock 樣式](images/label-uwp10stylegallery.png) <br/>適用於 Windows 10 app 的系統 TextBlock 樣式

在 Windows 市集應用程式和 Windows Phone 市集應用程式中，預設字型系列為 Global User Interface。 在 Windows 10 應用程式中，預設字型系列為 Segoe UI。 因此，您應用程式中的字型標準可能會看起來不一樣。 如果您想要重新產生 8.1 文字的外觀，您可以使用像是 [**LineHeight**](https://msdn.microsoft.com/library/windows/apps/br209671) 和 [**LineStackingStrategy**](https://msdn.microsoft.com/library/windows/apps/br244362) 這樣的屬性設定您自己的標準。

在 Windows 市集應用程式和 Windows Phone 市集應用程式中，文字的預設語言設定為組建的語言或 en-us。 在 Windows 10 應用程式中，預設語言設為常見的應用程式語言 (字型遞補)。 您可以明確地設定 [**FrameworkElement.Language**](https://msdn.microsoft.com/library/windows/apps/hh702066)，但是如果您未設定該屬性的值，將盡情享受更好的字型遞補行為。

如需詳細資訊，請參閱[字型的指導方針](https://msdn.microsoft.com/library/windows/apps/hh700394.aspx)和[設計 UWP app](http://go.microsoft.com/fwlink/p/?LinkID=533896)。 另請參閱上述的[控制項](#controls-and-control-styles-and-templates)一節，以了解文字控制項的變更。

## <a name="theme-changes"></a>佈景主題變更

針對通用 8.1 應用程式，預設佈景主題預設是深色。 Windows 10 裝置的預設佈景主題已經變更，但是您可以透過在 App.xaml 中宣告要求的佈景主題來控制所使用的佈景主題。 例如，若要在所有裝置上使用深色佈景主題，可將 `RequestedTheme="Dark"` 新增到根應用程式元素。

## <a name="tiles-and-toasts"></a>磚和快顯通知

對於磚和快顯通知，您目前正在使用的範本會繼續在您的 Windows 10 應用程式中運作。 但是有全新適合的範本可供您使用，在[通知、磚、快顯通知以及徽章](https://msdn.microsoft.com/library/windows/apps/mt185606)中有詳細說明。

先前在桌上型電腦上，快顯通知是一個暫時的訊息。 它會消失，錯過或略過之後也無法再取得。 在 Windows Phone 上，如果快顯通知略過或暫時關閉，則會進入重要訊息中心。 現在，重要訊息中心不再只限於行動裝置系列。

若要傳送快顯通知，不需要再宣告功能。

## <a name="window-size"></a>視窗大小

針對通用 8.1 應用程式，可以使用 [**ApplicationView**](https://msdn.microsoft.com/library/windows/apps/dn391667) 應用程式資訊清單元素來宣告最小的視窗寬度。 在 UWP app 中，您可以使用命令式程式碼來指定最小大小 (寬度與高度)。 預設的最小大小是 500x320epx，這也是可接受的最小大小下限。 可接受的最小大小上限是 500x500epx。

```csharp
   Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetPreferredMinSize
        (new Size { Width = 500, Height = 500 });
```

下一個主題是 [I/O、裝置與 app 模型的移植](w8x-to-uwp-input-and-sensors.md)。
