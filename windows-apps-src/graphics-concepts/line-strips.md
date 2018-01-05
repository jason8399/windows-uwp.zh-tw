---
title: "帶狀線"
description: "帶狀線是包含連接的帶狀線的基本類型。 您的應用程式可使用帶狀線建立未閉合之多邊形。 封閉的多邊形是用線段將其最後一個頂點連接到第一個頂點的多邊形。"
ms.assetid: 6E8C58E1-B463-44FD-A69F-81CCBF25D856
keywords: "帶狀線"
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7eec406faf1f695a473154dd23322bcdf9189049
ms.sourcegitcommit: c80b9e6589a1ee29c5032a0b942e6a024c224ea7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/22/2017
---
# <a name="line-strips"></a>帶狀線


帶狀線是包含連接的帶狀線的基本類型。 您的應用程式可使用帶狀線建立未閉合之多邊形。 封閉的多邊形是用線段將其最後一個頂點連接到第一個頂點的多邊形。 如果您的應用程式讓多邊形以帶狀線為主，不保證頂點為共面。

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>範例


下圖顯示呈現的帶狀線。

![帶狀線的圖例](images/linstrip.gif)

下列程式碼顯示如何建立這個帶狀線的頂點。

```
struct CUSTOMVERTEX
{
    float x,y,z;
};

CUSTOMVERTEX Vertices[] = 
{
    {-5.0, -5.0, 0.0},
    { 0.0,  5.0, 0.0},
    { 5.0, -5.0, 0.0},
    {10.0,  5.0, 0.0},
    {15.0, -5.0, 0.0},
    {20.0,  5.0, 0.0}
};
```

下列程式碼範例顯示如何在 Direct3D 中呈現帶狀線。

```
//
// It is assumed that d3dDevice is a valid
// pointer to an IDirect3DDevice interface.
//
d3dDevice->DrawPrimitive( D3DPT_LINESTRIP, 0, 5 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[基本型別](primitives.md)

 

 



