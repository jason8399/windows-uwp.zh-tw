---
description: 與裝置本身及其感應器整合的程式碼牽涉到從使用者輸入和輸出到使用者。
title: 將 Windows Runtime 8.x 移植到適用於 I/O、裝置和 app 模型的 UWP
ms.assetid: bb13fb8f-bdec-46f5-8640-57fb0dd2d85b
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d30554c22fbd40a555f51b25011b128072a16165
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372236"
---
# <a name="porting-windows-runtime-8x-to-uwp-for-io-device-and-app-model"></a>將 Windows Runtime 8.x 移植到適用於 I/O、裝置和 app 模型的 UWP




前一個主題是[移植 XAML 與 UI](w8x-to-uwp-porting-xaml-and-ui.md)。

與裝置本身及其感應器整合的程式碼牽涉到從使用者輸入和輸出到使用者。 它也可能涉及資料處理。 但是這個程式碼通常不被認為是 UI 層*或資料層*。 這個程式碼包含與震動控制器、加速計、陀螺儀、麥克風和喇叭 (與語音辨識和合成交集)、(地理) 位置以及輸入形式 (例如觸控、滑鼠、鍵盤及手寫筆) 的整合。

## <a name="application-lifecycle-process-lifetime-management"></a>應用生命程式週期 (處理程序生命週期管理)


當應用程式變成非使用中，而系統即將引發暫停事件之前，通用 8.1 應用程式會有兩秒鐘的「防反彈空檔」。 使用這個防反彈空檔當成額外的暫停狀態時間並不安全，因為通用 Windows 平台 (UWP) 應用程式完全沒有防反彈空檔。當 應用程式變成非使用中時，隨即會引發暫停事件。

如需詳細資訊，請參閱[應用程式週期](https://docs.microsoft.com/windows/uwp/launch-resume/app-lifecycle)。

## <a name="background-audio"></a>背景音訊


針對[ **MediaElement.AudioCategory** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaelement.audiocategory)屬性， **ForegroundOnlyMedia**並**BackgroundCapableMedia**中被取代Windows 10 應用程式。 請改用 Windows Phone 市集應用程式模型。 如需詳細資訊，請參閱 [背景音效](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio)。

## <a name="detecting-the-platform-your-app-is-running-on"></a>偵測執行您 app 的平台


應用程式為目標的變更，與 Windows 10 的思考方式。 新的概念性模型是針對通用 Windows 平台 (UWP) 設計應用程式，然後在所有 Windows 裝置上執行。 接下來可以決定要啟用的特定裝置系列專屬功能。 如有需要，app 也有選項可供限制其特別針對一或多個裝置系列進行設計。 如需有哪些裝置系列以及如何決定要針對哪個裝置系列進行設計的詳細資訊，請參閱 [UWP app 指南](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)。

如果在您的通用 8.1 應用程式中有程式碼可偵測出其執行所在的作業系統，您可能必須根據邏輯原因來加以變更。 如果應用程式會不停傳送值卻不加以處理，您可能想要繼續收集作業系統資訊。

**附註**  建議，您使用作業系統或裝置系列來偵測功能是否存在。 若要判斷特定作業系統或裝置系列功能是否存在，識別目前的作業系統或裝置系列通常不是最佳的方式。 不要偵測作業系統或裝置系列 (與版本號碼)，而是要測試功能本身是否存在 (請參閱[條件式編譯與調適型程式碼](w8x-to-uwp-porting-to-a-uwp-project.md))。 如果您必須要求特定的作業系統或裝置系列，請務必將其當做最低的支援版本，而不是專門針對那一個版本來設計測試。

 

有數項建議技術可用來針對不同的裝置量身打造您的應用程式 UI。 繼續使用自動調整大小元素與動態配置面板。 在 XAML 標記中，繼續使用以有效像素 (前身為檢視像素) 為單位的大小，讓您的 UI 可隨不同的解析度與縮放比例調整 (請參閱[有效像素、檢視距離及縮放係數](w8x-to-uwp-porting-xaml-and-ui.md))。 還有使用 Visual State Manager 的調適型觸發程序與 Setter 讓您的 UI 可隨視窗大小調整 (請參閱 [UWP app 指南](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide))。

不過，如果有無法避免的情況，使您不得不偵測裝置系列時，才能那樣做。 在這個範例中，我們使用 [**AnalyticsVersionInfo**](https://docs.microsoft.com/uwp/api/Windows.System.Profile.AnalyticsVersionInfo) 類別來瀏覽到專為行動裝置系列量身訂做的適當頁面，然後確保其會切換回預設頁面。

```csharp
   if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Mobile")
        rootFrame.Navigate(typeof(MainPageMobile), e.Arguments);
    else
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
```

您的應用程式也能從作用中的資源選擇因素來判斷其正在哪個裝置系列上執行。 下列範例說明如何以命令方式來判斷，而 [**ResourceContext.QualifierValues**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues) 主題還會根據裝置系列因素，針對載入裝置系列特定資源中的類別說明較常見的使用案例。

```csharp
var qualifiers = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
string deviceFamilyName;
bool isDeviceFamilyNameKnown = qualifiers.TryGetValue("DeviceFamily", out deviceFamilyName);
```

另請參閱[條件式編譯與調適型程式碼](w8x-to-uwp-porting-to-a-uwp-project.md)。

## <a name="location"></a>Location


當宣告位置功能，在其應用程式封裝資訊清單上執行的 Windows 10 中的應用程式時，系統會提示使用者進行同意。 應用程式的 Windows Phone 市集應用程式或 Windows 10 應用程式是否為 true。 如果您的應用程式顯示其自有的自訂同意提示，或如果它提供切換開關，則建議您移除這些，使系統只會提示使用者一次。

 

 




