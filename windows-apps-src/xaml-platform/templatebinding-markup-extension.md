---
author: jwmsft
description: 連結控制項範本中的屬性值和範本化控制項中的其他公開屬性值。 TemplateBinding 只能在 XAML 的 ControlTemplate 定義中使用。
title: TemplateBinding 標記延伸
ms.assetid: FDE71086-9D42-4287-89ED-8FBFCDF169DC
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: ca551ff53a0a91b5bc60263b6e282b95c32bf976
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.locfileid: "210491"
---
# <a name="templatebinding-markup-extension"></a>{TemplateBinding} 標記延伸

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

連結控制項範本中的屬性值和範本化控制項中的其他公開屬性值。 **TemplateBinding** 只能在 XAML 的 [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) 定義中使用。

## <a name="xaml-attribute-usage"></a>XAML 屬性用法

``` syntax
<object propertyName="{TemplateBinding sourceProperty}" .../>
```

## <a name="xaml-attribute-usage-for-setter-property-in-template-or-style"></a>XAML 屬性用法 (適用於範本或樣式中的 Setter 屬性)

``` syntax
<Setter Property="propertyName" Value="{TemplateBinding sourceProperty}" .../>
```

## <a name="xaml-values"></a>XAML 值

| 詞彙 | 說明 |
|------|-------------|
| propertyName | 正在 setter 語法中設定的屬性的名稱。 必須是相依性屬性。 |
| sourceProperty | 存在於範本化類型中其他相依性屬性的名稱。 |

## <a name="remarks"></a>備註

不論您是一個自訂控制項作者，還是要取代現有控制項的控制項範本，定義控制項範本的方法時，使用 **TemplateBinding** 是相當基礎的一個部分。 如需詳細資訊，請參閱[快速入門：控制項範本](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374)。

讓 *propertyName* 與 *targetProperty* 使用相同的屬性名稱是相當常見的做法。 在這個情況下，控制項可以在本身定義一個屬性，然後將該屬性轉送到它的其中一個元件部分的現有直覺式具名屬性。 例如，組合中併入 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 的控制項 (用來顯示控制項擁有的 **Text** 屬性) 可能會包含這個 XAML 以做為控制項範本的一部分： `<TextBlock Text="{TemplateBinding Text}" .... />`

用來做為來源屬性值和目標屬性值的類型必須相符。 當您正在使用 **TemplateBinding** 時，沒有機會可以引入轉換器。 這樣在剖析 XAML 時會因為無法比對值而產生錯誤。 如果您需要轉換器，可以針對範本繫結使用動詞語法，例如： `{Binding RelativeSource={RelativeSource TemplatedParent}, Converter="..." ...}`

嘗試在 XAML 中的 [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) 定義之外使用 **TemplateBinding** 會導致剖析器錯誤。

您可以針對範本化的父系值也會延遲為其他繫結的情況，使用 **TemplateBinding**。 **TemplateBinding** 的評估會等到所有必要的執行階段繫結都有值之後才進行。

**TemplateBinding** 一律是單向繫結。 內含的兩個屬性都必須是相依性屬性。

**TemplateBinding** 是一個標記延伸。 當有需要將屬性值逸出文字值或處理常式名稱時，通常就會實作標記延伸，而且這需求是全域性的，而不只是在特定類型或屬性放置類型轉換器。 XAML 的所有標記延伸會在屬性語法中使用「{」和「}」字元，這是慣例，XAML 處理器藉此來辨識必須處理屬性的標記延伸。

**注意**  在 Windows 執行階段 XAML 處理器實作中，沒有 **TemplateBinding** 的支援類別表示法。 **TemplateBinding** 僅限在 XAML 標記中使用。 並沒有一個直接的方式可以在程式碼中重現行為。

## <a name="related-topics"></a>相關主題

* [快速入門：控制項範本](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374)
* [深入了解資料繫結](https://msdn.microsoft.com/library/windows/apps/mt210946)
* [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391)
* [XAML 概觀](xaml-overview.md)
* [相依性屬性概觀](dependency-properties-overview.md)
 
