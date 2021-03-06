---
ms.assetid: cf0d2709-21a1-4d56-9341-d4897e405f5d
title: XAML/C# 錯誤處理的逐步解說
description: '遵循本逐步解說，瞭解如何在 XAML/c # 應用程式中攔截及處理來自 AdControl 的錯誤。'
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, 廣告, 廣告, 錯誤處理, XAML, c#
ms.localizationpriority: medium
ms.openlocfilehash: 4526f44c1a38af79886a7404eb932416a4414f77
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043500"
---
# <a name="error-handling-in-xamlc-walkthrough"></a>XAML/C# 錯誤處理的逐步解說

>[!WARNING]
> 從2020年6月1日起，適用于 Windows UWP 應用程式的 Microsoft Ad 營收平臺將會關閉。 [深入了解](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

此逐步解說示範如何在您的 App 中抓取廣告相關錯誤。 此逐步解說示範使用 [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 來顯示橫幅廣告，但它的一般概念也適用於插播式廣告和原生廣告。

這些範例假設您有包含 **AdControl** 的 XAML/C# app。 如需示範如何將 **AdControl** 新增到 app 的逐步指示，請參閱 [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md)。 

1.  在您的 MainPage.xaml 檔案中，找出 **AdControl** 的定義。 該程式碼看起來像這樣：
    ``` xml
    <UI:AdControl
      ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
      AdUnitId="test"
      HorizontalAlignment="Left"
      Height="250"
      Margin="10,10,0,0"
      VerticalAlignment="Top"
      Width="300" />
    ```

2.   在 **Width** 屬性後方，但在結尾標記的前方，指派錯誤事件處理常式的名稱給 [ErrorOccurred](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.erroroccurred) 事件。 在這個逐步解說中，錯誤事件處理常式的名稱為 **OnAdError**。
    ``` xml
    <UI:AdControl
      ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
      AdUnitId="test"
      HorizontalAlignment="Left"
      Height="250"
      Margin="10,10,0,0"
      VerticalAlignment="Top"
      Width="300"
      ErrorOccurred="OnAdError"/>
    ```

3.  若要在執行階段產生錯誤，請建立具有不同應用程式識別碼的第二個 **AdControl**。 因為在 app 中，所有 **AdControl** 物件都必須使用相同的應用程式識別碼，所以建立具有不同應用程式識別碼的其他 **AdControl** 將會擲回錯誤。

    在 MainPage.xaml 中第一個 **AdControl** 的後方定義第二個 **AdControl**，然後將 [ApplicationId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) 屬性設為零 (“0”)。
    ``` xml
    <UI:AdControl
        ApplicationId="0"
        AdUnitId="test"
        HorizontalAlignment="Left"
        Height="250"
        Margin="10,265,0,0"
        VerticalAlignment="Top"
        Width="300"
        ErrorOccurred="OnAdError" />
    ```

4.  在 MainPage.xaml.cs 中，將以下 **OnAdError** 事件處理常式新增到 **MainPage** 類別。 這個事件處理常式會寫入資訊到 Visual Studio **Output** 視窗。
    ``` csharp
    private void OnAdError(object sender, AdErrorEventArgs e)
    {
        System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name +
            "): " + e.ErrorMessage + " ErrorCode: " + e.ErrorCode.ToString());
    }
    ```

4.  建置並執行專案。 執行 app 之後，您將會看到下面與 Visual Studio 之 **Output** 視窗中的訊息類似的訊息。
    ```json
    AdControl error (): MicrosoftAdvertising.Shared.AdException: all ad requests must use the same application ID within a single application (0, d25517cb-12d4-4699-8bdc-52040c712cab) ErrorCode: ClientConfiguration
    ```

## <a name="related-topics"></a>相關主題

* [GitHub 上的廣告範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
