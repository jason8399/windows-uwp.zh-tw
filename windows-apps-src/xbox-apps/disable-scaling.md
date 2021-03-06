---
title: 如何關閉縮放比例
description: 關閉預設縮放比例的指示。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 6e68c1fc-a407-4c0b-b0f4-e445ccb72ff3
ms.localizationpriority: medium
ms.openlocfilehash: 44688ff40792ba2ee72cbd1d96bae1ac59834efa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604743"
---
# <a name="how-to-turn-off-scaling"></a>如何關閉縮放比例   
根據預設，若為 XAML，應用程式會調整為 200%，若為 HTML app 則為 150%。 您可以關閉預設縮放比例。 這會導致應用程式使用裝置的實際像素尺寸 (1910 x 1080 像素)。   
   
## <a name="html"></a>HTML   
您可以使用下列程式碼片段，選擇不使用縮放比例︰ 
   
```
var result = Windows.UI.ViewManagement.ApplicationViewScaling.trySetDisableLayoutScaling(true);
```

或者，您可以使用 Web 適用的方法：   

```   
@media (max-height: 1080px) {   
    @-ms-viewport {   
        height: 1080px;   
    }   
}   
```

## <a name="xaml"></a>XAML
您可以使用下列程式碼片段，選擇不使用縮放比例︰   
   
```
bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```
   
## <a name="directxc"></a>DirectX/C++   
DirectX/C++ 應用程式不會進行縮放。 自動縮放僅適用於 HTML 與 XAML 應用程式。  

## <a name="see-also"></a>請參閱
- [Xbox 的最佳作法](tailoring-for-xbox.md)
- [在 Xbox One UWP](index.md)
