---
description: 在 XAML 標記中，指定 x:Bind 的預設模式。
title: xDefaultBindMode 屬性
ms.date: 02/08/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: c8917b09f04206a5466797f48414defeb35baf5e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57647603"
---
# <a name="xdefaultbindmode-attribute"></a>x:DefaultBindMode 屬性

在 XAML 標記中，指定 x:Bind 的預設模式。

**x:DefaultBindMode** 屬性是在 Windows 10 (版本 1607，年度更新版) SDK 版本 14393 中開始提供。

## <a name="xaml-attribute-usage"></a>XAML 屬性用法

``` syntax
<object x:DefaultBindMode="OneTime \| OneWay \| TwoWay" .../>
```

## <a name="remarks"></a>備註

[x:bind](x-bind-markup-extension.md)預設模式為 **OneTime**。 這是基於效能考量所選擇，因為使用 **OneWay** 會導致產生更多程式碼來連結和處理變更偵測。 您可以使用 **x:DefaultBindMode** 變更標記樹狀結構特定片段的 x:Bind 預設模式。 指定的模式會在該元素及其子系上，套用任何未明確指定模式做為繫結之一部分的 x:Bind 運算式。

## <a name="related-topics"></a>相關主題

* [x： 繫結標記延伸](x-bind-markup-extension.md)
