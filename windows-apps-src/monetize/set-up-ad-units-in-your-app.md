---
author: mcleanbyron
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: 了解在提交 app 到 Microsoft Store 之前，如何從 Windows 開發人員中心儀表板新增應用程式識別碼和廣告單元識別碼。
title: 在您的應用程式中設定廣告單元
ms.author: mcleans
ms.date: 10/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 廣告, 廣告單元, 測試
ms.localizationpriority: medium
ms.openlocfilehash: 6473d571ed44f60f8001b8a565d70c58d43407d1
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
ms.locfileid: "1654657"
---
# <a name="set-up-ad-units-in-your-app"></a>在您的應用程式中設定廣告單元

通用 Windows 平台 (UWP) app 中的每個控制項都有對應的*廣告單元*，是由我們的服務用來提供廣告給控制項。 每個廣告單元包含*廣告單元識別碼*和*應用程式識別碼*，您必須將其指派給應用程式中的 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx)、[InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 或 [NativeAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.aspx)。

我們提供[測試廣告單元值](#test-ad-units)，您可以在測試期間用來確認您的應用程式可以顯示測試廣告。 這些測試值只能在您應用程式的測試版本中使用。 如果您嘗試在應用程式發佈後使用測試值，您的實際應用程式將不會收到廣告。

完成測試 UWP app 且準備好提交至 Windows 開發人員中心時，您必須從 Windows 開發人員中心儀表板的 [\[應用程式內廣告\]](../publish/in-app-ads.md) 頁面[建立即時廣告單元](#live-ad-units)，然後更新您的 App 程式碼來使用此廣告單元的應用程式識別碼和廣告單元識別碼值。

<span id="test-ad-units" />

## <a name="test-ad-units"></a>測試廣告單元

當您開發您的應用程式時，使用本節中的測試應用程式識別碼和廣告單元識別碼值，以查看您的應用程式於測試期間呈現廣告的方式。

### <a name="banner-ads-using-the-adcontrol-class"></a>橫幅廣告 (使用 AdControl 類別)

* 廣告單元識別碼： ```test```
* 應用程式識別碼：  ```3f83fe91-d6be-434d-a0ae-7351c5a997f1```

    > [!IMPORTANT]
    > **AdControl** 即時廣告的大小是由 **Width** 和 **Height** 屬性定義。 為獲得最佳結果，請確定程式碼中的 **Width** 和 **Height** 屬性是[橫幅廣告支援的廣告大小](supported-ad-sizes-for-banner-ads.md)其中之一。 **Width** 和 **Height** 屬性不會隨即時廣告的大小變更。

### <a name="interstitial-ads-and-native-ads"></a>插播式廣告以及原生廣告

* 廣告單元識別碼： ```test```
* 應用程式識別碼：  ```d25517cb-12d4-4699-8bdc-52040c712cab```

<span id="live-ad-units" />

## <a name="live-ad-units"></a>即時廣告單元

若要從開發人員中心儀表板取得即時廣告單元並在您的應用程式中使用：

1.  在儀表板中的 **\[應用程式內廣告\]** 頁面，[建立廣告單元](../publish/in-app-ads.md#create-ad-unit)。 請務必指定您的應用程式中正在使用的正確廣告控制項的廣告單元類型。
    > [!NOTE]
    > 您可以選擇為廣告單元啟用廣告流量分配，方法是在 [\[流量分配設定\]](../publish/in-app-ads.md#mediation) 區段進行設定。 廣告流量分配可讓您獲得最大的廣告收益並充分發揮應用程式促銷功能，透過顯示來自多個廣告網路的廣告，包括其他付費廣告網路的廣告，以及 Microsoft 應用程式促銷活動的廣告。 根據預設，我們會使用機器學習演算法自動設定廣告流量分配，來協助您的 app 在支援的所有市場，將您的廣告收入最大化，但是您可以選擇手動設定廣告流量分配。

2.  建立新的廣告單元之後，在**獲利**&gt;**應用程式中廣告**頁面中擷取可用的廣告單元表格中廣告單元的**應用程式識別碼**和**廣告單元識別碼**。
    > [!NOTE]
    > 測試廣告單元和即時 UWP 廣告單元的應用程式識別碼值有不同的格式。 測試應用程式識別碼值為 GUID。 當您在儀表板中建立即時 UWP 廣告單元時，廣告單元的應用程式識別碼值一律符合您應用程式的 Microsoft Store 識別碼 (Microsoft Store 識別碼值範例類似於 9NBLGGH4R315)。

3.  在您的應用程式代碼中，指派**應用程式識別碼**和**廣告單元識別碼**值：

    * 如果您的 App 顯示橫幅廣告，請將這些值指派給 [AdControl](https://msdn.microsoft.com/library/mt313154.aspx) 物件的 [ApplicationId](https://msdn.microsoft.com/library/mt313174.aspx) 和 [AdUnitId](https://msdn.microsoft.com/library/mt313171.aspx) 屬性。 如需詳細資訊，請參閱 [XAML 和 .NET 中的 AdControl](../monetize/adcontrol-in-xaml-and--net.md) 和 [HTML 5 和 JavaScript 中的 AdControl](../monetize/adcontrol-in-html-5-and-javascript.md)。

    * 如果您的 App 顯示插播式廣告，請將這些值傳遞給 [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx) 物件的 [RequestAd](https://msdn.microsoft.com/library/mt313192.aspx) 方法。 如需詳細資訊，請參閱[插播式廣告](../monetize/interstitial-ads.md)。

    * 如果您的 app 顯示原生廣告，請將這些值指派給 [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.nativeadsmanager.aspx) 建構函式的 *applicationId* 和 *adUnitId* 參數。 如需詳細資訊，請參閱[原生廣告](../monetize/native-ads.md)。

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>管理您應用程式中多個廣告控制項的廣告單元

您可以在單一 App 中使用多個橫幅、插播式廣告及原生廣告控制項。 在本案例中，我們建議您將不同的廣告單元指派給每個控制項。 針對每個控制項使用不同的廣告單元，可讓您為每個控制項個別[進行流量分配設定](../publish/in-app-ads.md#mediation)，並取得不同的[報告資料](../publish/advertising-performance-report.md)。 這也可讓我們的服務更能最佳化提供您的 App 的廣告。

> [!IMPORTANT]
> 每個廣告單元只能在一個 app 中使用。 如果您在多個 app 中使用廣告單元，不會提供廣告給該廣告單元。

## <a name="related-topics"></a>相關主題

* [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md)
* [HTML 5 和 Javascript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)
* [插播式廣告](interstitial-ads.md)
* [原生廣告](native-ads.md)


 

 