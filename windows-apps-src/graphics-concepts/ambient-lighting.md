---
title: 環境光源
description: 環境光線為場景提供定值光線。
ms.assetid: C34FA65A-3634-4A4B-B183-4CDA89F4DC95
keywords:
- 環境光源
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ac958a93fcafbb33a9025196b49398e2e3269e55
ms.sourcegitcommit: 82edc63a5b3623abce1d5e70d8e200a58dec673c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/20/2019
ms.locfileid: "58291836"
---
# <a name="ambient-lighting"></a>環境光源

環境光線為場景提供定值光線。 它同等地照亮所有物件端點，因為其不取決於任何其他照明因素，例如頂點標準、光源方向、光源位置、範圍或衰減。 環境光線於所有方向皆為定值，且為物件的所有像素以相同方式上色。 加以計算很快速，但是會讓物件看起來很平板且不真實。

環境光線是最快速的照明類型，但產生的結果最不逼真。 Direct3D 包含單一全域環境光線屬性，您可加以使用而不需建立任何光線。 或者，您可以將任何光線物件設為提供環境光線。

場景的環境光線由下列方程式描述。

環境光源 = Cₐ\*\[Gₐ + sum (Atten<sub>我</sub>\*點<sub>我</sub>\*L<sub>ai</sub>)\]

其中：

| 參數         | 預設值 | 類型          | 描述                                                                                                       |
|-------------------|---------------|---------------|-------------------------------------------------------------------------------------------------------------------|
| Cₐ                | (0,0,0,0)     | D3DCOLORVALUE | 材料環境色彩                                                                                            |
| Gₐ                | (0,0,0,0)     | D3DCOLORVALUE | 全域環境色彩                                                                                              |
| Atten<sub>i</sub> | (0,0,0,0)     | D3DCOLORVALUE | 第 i 個光線的光衰減。 參閱[衰減和聚光燈係數](attenuation-and-spotlight-factor.md)。 |
| Spot<sub>i</sub>  | (0,0,0,0)     | D3DVECTOR     | 第 i 個光線的聚光燈係數。 參閱[衰減和聚光燈係數](attenuation-and-spotlight-factor.md)。  |
| 總和               | N/A           | N/A           | 環境光線的加總                                                                                          |
| L<sub>ai</sub>    | (0,0,0,0)     | D3DVECTOR     | 第 i 個光線的環境光線色彩                                                                              |

 

Cₐ 的值為︰

-   頂點 color1 如果 AMBIENTMATERIALSOURCE = D3DMCS\_COLOR1，與第一個頂點色彩頂點宣告中提供。
-   頂點色 2，如果 AMBIENTMATERIALSOURCE = D3DMCS\_頂點宣告中提供色 2，與第二個頂點的色彩。
-   材料環境色彩。

**附註**  如果使用任一 AMBIENTMATERIALSOURCE 選項時，而且未提供的端點色彩，則材料的環境色彩會在使用。

 

若要使用材料環境色彩，請使用下方範例程式碼所示的 SetMaterial。

Gₐ 是全域環境色彩。 它會設定使用 Graphicsdevice (D3DRS\_AMBIENT)。 Direct3D 場景中有一個全域環境色彩。 此參數與 Direct3D 光線物件無關聯。

L<sub>ai</sub> 是場景中第 i 個光線的環境色彩。 每個 Direct3D 光線都有一組屬性，其中一項是環境色彩。 sum(L<sub>ai</sub>) 這項字詞是場景中所有環境色彩的加總。

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>範例


在此範例中，物件使用場景環境光線和材料環境色彩上色。

```cpp
#define GRAY_COLOR  0x00bfbfbf

Ambient.r = 0.75f;
Ambient.g = 0.0f;
Ambient.b = 0.0f;
Ambient.a = 0.0f;
```

根據方程式，為物件端點產生的色彩是材質色彩和淺色的組合。

下方兩個圖例顯示材質色彩 (灰色)，以及淺色 (鮮紅)。

![灰球體圖例](images/amb1.jpg)![紅色球體圖例](images/lightred.jpg)

下圖顯示產生的場景。 場景中唯一的物件為球體。 環境光線使用相同色彩照亮所有物件端點。 其不取決於頂點標準或光線方向。 如此一來，球體看起來像是 2D 圓形，因為物件表面周圍的陰影並無差別。

![使用環境光線的球體圖例](images/lighta.jpg)

若要讓物件看起來更逼真，請在環境光線之外套用光源擴散或反射光源。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[光源的數學運算](mathematics-of-lighting.md)

 

 




