---
Description: 使用淡化動畫將項目帶入檢視或帶出檢視。 兩個常見的淡化動畫為淡入和淡出。
title: 淡化動畫
ms.assetid: 975E5EE3-EFBE-4159-8D10-3C94143DD07F
label: Motion--fades
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6b937ba4174d01096a09a98f7efd4e7e3dce9632
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970303"
---
# <a name="fade-animations"></a>淡化動畫



使用淡化動畫將項目帶入檢視或帶出檢視。 兩個常見的淡化動畫為淡入和淡出。

> **重要 Api**: [**FadeInThemeAnimation 類別**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation)、[**FadeOutThemeAnimation 類別**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation)


## <a name="dos-and-donts"></a>可行與禁止事項


-   當您的應用程式在不相關或包含大量文字元素之間轉換時，請先使用淡出再接著使用淡入。 這樣能讓傳出物件完全消失，然後才看到傳入物件。
-   如果元素的大小維持一致，而且您希望使用者感覺它們看起來是相同的項目時，請在傳出元素上淡入一或多個傳入元素。 一旦傳入完成後，就可以移除傳出項目。 這個選項是在傳入項目可以完全覆蓋傳出項目時才可行。
-   請避免使用淡入/淡出動畫在清單中新增或刪除項目。 請改用專為該目的建立的清單動畫。
-   請避免使用淡入/淡出動畫變更頁面的全部內容。 請改用專為該目的建立的頁面轉換動畫。
-   淡出是移除元素的一種微妙方式。
## <a name="related-articles"></a>相關文章

* [動畫概觀](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [讓淡入/淡出產生動畫效果](https://docs.microsoft.com/previous-versions/windows/apps/jj649429(v=win.10))
* [快速入門：使用動畫庫讓 UI 產生動畫效果](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**FadeInThemeAnimation 類別**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation)
* [**FadeOutThemeAnimation 類別**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation)

 

 




