---
author: jwmsft
description: 在 XAML 標記中，為屬性指定 null 值。
title: xNull 標記延伸
ms.assetid: E6A4038E-4ADA-4E82-9824-582FC16AB037
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: a367594cab5e1f29ce6c5f45ee869b025c4bf47e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.locfileid: "210553"
---
# <a name="xnull-markup-extension"></a>{x:Null} 標記延伸

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

在 XAML 標記中，為屬性指定 **null** 值。

## <a name="xaml-attribute-usage"></a>XAML 屬性用法

``` syntax
<object property="{x:Null}" .../>
```

## <a name="remarks"></a>備註

**null** 是 C# 與 C++ 的 null 參考關鍵字。 Microsoft Visual Basicnull 的 null 參考關鍵字為 **Nothing**。

不同相依性屬性的初始預設值可能會不同，且不一定是 **null**。 此外，許多相依性屬性因為內部實作的緣故，不接受以 **null** 做為值 (無論是透過標記或程式碼)。 在這種情況下，以 **{x:Null}** 設定 XAML 屬性值可能導致發生剖析器例外狀況。

一些 Windows 執行階段類型，是可為 null 的類型。 在可為 null 的類型尚未使用 **null** 做為預設值的情況下，您可以使用 **{x:Null}**，在 XAML 中設為 **null** 值。 如果使用的是 Visual C++ 元件延伸 (C++/CX)，可為 null 的類型會以 [**Platform::IBox<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/jj606120.aspx) 來表示。 如果使用的是 Microsoft .NET 語言，可為 null 的類型會以 [**Nullable<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx) 來表示。

## <a name="related-topics"></a>相關主題

* [**Nullable<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx)
* [**IReference<T>**](https://msdn.microsoft.com/library/windows/apps/br225864)
 
