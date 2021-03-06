---
ms.assetid: E2B73380-D673-48C6-9026-96976D745017
title: 開始使用常用控制項
description: 查看常見通用 Windows 平臺 (UWP) 控制項及其對等 iOS 控制項的相關主題連結清單。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 86debeae4a4d8d3e3fb1a4084a3cf1ebef10f9d6
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88943067"
---
# <a name="getting-started-common-controls"></a>開始使用：常用控制項


## <a name="common-controls-list"></a>常用控制項清單

在上一個小節中，您只使用了兩個控制項：按鈕和文字區塊。 當然還有許多控制項可供您使用。 以下是一些您會在 app 及其 iOS 對等項目中使用的常用控制項。 這裡依字母順序列出 iOS 控制項，旁邊是最相似的通用 Windows 平台 (UWP) 控制項。

UWP 控制項的好處是它們可以感應正在執行的所在裝置類型，並適當地變更其外觀和功能。 例如，如果您的專案使用 [**DatePicker**](https://docs.microsoft.com/previous-versions/windows/apps/br211681(v=win.10)) 控制項，它會有足夠的智慧自行最佳化，以便在桌上型電腦與手機 (舉例) 上有不同的外觀和行為。 您不需要執行任何動作：控制項會在執行階段自我調整。

| iOS 控制項 (類別/通訊協定) | 對等 UWP 控制項 |
|------------------------------|--------------------------------------|
| 活動指示器 (**UIActivityIndicatorView**) | [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) <br/> 另請參閱[快速入門：新增進度控制項](https://docs.microsoft.com/previous-versions/windows/apps/hh780651(v=win.10)) |
| 廣告橫幅檢視 (**ADBannerView**) 和廣告檢視委派 (**ADBannerViewDelegate**) | [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) <br/> 另請參閱[在您的應用程式中顯示廣告](../monetize/display-ads-in-your-app.md) |
| 按鈕 (UIButton) | [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) <br/> 另請參閱[快速入門：新增按鈕控制項](https://docs.microsoft.com/previous-versions/windows/apps/jj153346(v=win.10)) |
| 日期選擇器 (UIDatePicker) | [DatePicker](https://docs.microsoft.com/previous-versions/windows/apps/br211681(v=win.10)) |
| 影像檢視 (UIImageView) | [映像](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) <br/> 另請參閱 [Image 和 ImageBrush](https://docs.microsoft.com/windows/uwp/controls-and-patterns/images-imagebrushes) |
| 標籤 (UILabel) | [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) <br/> 另請參閱[快速入門：顯示文字](https://docs.microsoft.com/previous-versions/windows/apps/hh700392(v=win.10)) |
| 地圖檢視 (MKMapView) 和地圖檢視委派 (MKMapViewDelegate) | 查看[適用于 UWP 應用程式的 Bing 地圖](https://msdn.microsoft.com/library/hh846481)服務 |
| 瀏覽控制項 (UINavigationController) 和瀏覽控制項委派 (UINavigationControllerDelegate) | [Frame](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) <br/> 另請參閱[瀏覽](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) |
| 頁面控制項 (UIPageControl) | [頁面](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) <br/> 另請參閱[瀏覽](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) |
| 選擇器檢視 (UIPickerView) 和選擇器檢視委派 (UIPickerViewDelegate) | [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) <br/> 另請參閱[新增下拉式方塊與清單方塊](https://docs.microsoft.com/previous-versions/windows/apps/hh780616(v=win.10)) |
| 進度列 (UIProgressView) | [進度列](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressBar) <br/> 另請參閱[快速入門：新增進度控制項](https://docs.microsoft.com/previous-versions/windows/apps/hh780651(v=win.10)) |
| 捲動檢視 (UIScrollView) 和捲動檢視委派 (UIScrollViewDelegate) | [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) <br/>  另請參閱[Extensible Application Markup Language (XAML) 捲動、移動瀏覽和縮放範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample%20(Windows%208)) |
| 搜尋列 (UISearchBar) 和搜尋列委派 (UISearchBarDelegate) | 請參閱[將搜尋新增到應用程式](https://docs.microsoft.com/previous-versions/windows/apps/jj130767(v=win.10)) <br/>  另請參閱[快速入門：將搜尋新增到應用程式](https://docs.microsoft.com/previous-versions/windows/apps/hh868180(v=win.10)) |
| 分段控制項 (UISegmentedControl) | 無 |
| 滑桿 (UISlider) | [滑桿](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) <br/>  另請參閱[如何新增滑桿](https://docs.microsoft.com/previous-versions/windows/apps/hh868197(v=win.10)) |
| 分割檢視控制項 (UISplitViewController) 和分割檢視控制項委派 (UISplitViewControllerDelegate) | 無 |
| 開關 (UISwitch) | [ToggleSwitch](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToggleSwitch) <br/>  另請參閱[如何新增切換開關](https://docs.microsoft.com/previous-versions/windows/apps/hh868198(v=win.10)) |
| 索引標籤列控制項 (UITabBarController) 和索引標籤列控制項委派 (UITabBarControllerDelegate) | 無 |
| 表格檢視控制項 (UITableViewController)、表格檢視 (UITableView)、表格檢視委派 (UITableViewDelegate)、表格儲存格 (UITableViewCell) | [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) <br/>  另請參閱[快速入門：新增 ListView 和 GridView 控制項](https://docs.microsoft.com/previous-versions/windows/apps/hh780650(v=win.10)) |
| 文字欄位 (UITextField) 和文字欄位委派 (UITextFieldDelegate) | [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) <br/>  另請參閱[顯示和編輯文字](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/text-controls) |
| 文字檢視 (UITextView) 和文字檢視委派 (UITextViewDelegate) | [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) <br/>  另請參閱[快速入門：顯示文字](https://docs.microsoft.com/previous-versions/windows/apps/hh700392(v=win.10)) |
| 檢視 (UIView) 和檢視控制項 (UIViewController) | [頁面](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) <br/>  另請參閱[瀏覽](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) |
| 網頁檢視 (UIWebView) 和網頁檢視委派 (UIWebViewDelegate) | [WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) <br/>  另請參閱 [XAML WebView 控制項範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20WebView%20control%20sample%20(Windows%208)) |
| 視窗 (UIWindow) | [Frame](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) <br/>  另請參閱[瀏覽](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) |

如果想知道更多控制項，請參閱[控制項清單](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/)。

**注意**   如需使用 JavaScript 和 HTML 之 UWP 應用程式的控制項清單，請參閱[控制項清單](https://docs.microsoft.com/previous-versions/windows/apps/hh465453(v=win.10))。

### <a name="next-step"></a>後續步驟

[消費者入門：導覽](getting-started-navigation.md)

## <a name="related-topics"></a>相關主題

* [Build 2014：XAML UI 和控制項呢？](https://channel9.msdn.com/Events/Build/2014/2-516)
* [Build 2014：使用通用的 XAML UI 架構開發應用程式](https://channel9.msdn.com/Events/Build/2014/2-507)
* [Build 2014：使用 Visual Studio 建置 XAML 交集的應用程式](https://channel9.msdn.com/Events/Build/2014/3-591)
