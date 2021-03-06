---
title: 衰減和聚光燈係數
description: 全域照明方程式的擴散和聚光燈型光源元件內含描述光衰減及焦點圓錐的字詞。
ms.assetid: F61D4ACB-09AB-4087-9E2D-224E472D6196
keywords:
- 衰減和聚光燈係數
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8126ac8fa738a2b8a9680d215179fe23f77c5d44
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659293"
---
# <a name="attenuation-and-spotlight-factor"></a>衰減和聚光燈係數


全域照明方程式的擴散和聚光燈型光源元件內含描述光衰減及焦點圓錐的字詞。 以下將說明這些字詞。

## <a name="span-idattenuationspanspan-idattenuationspanspan-idattenuationspanattenuation"></a><span id="Attenuation"></span><span id="attenuation"></span><span id="ATTENUATION"></span>衰減


光線的衰減取決於光線的類型以及光線與頂點位置之間的距離。 若要計算衰減，請使用下列其中一個方程式。

Atten = 1 / (att0<sub>我</sub>+ att1<sub>我</sub> \* d + att2<sub>我</sub> \* d²)

其中：

| 參數        | 預設值 | 類型           | 描述                                     | 範圍          |
|------------------|---------------|----------------|-------------------------------------------------|----------------|
| att0<sub>i</sub> | 0.0           | 浮點數 | 定值衰減因數                     | 0 至 +infinity |
| att1<sub>i</sub> | 0.0           | 浮點數 | 線性衰減因數                       | 0 至 +infinity |
| att2<sub>i</sub> | 0.0           | 浮點數 | 二次方衰減因數                    | 0 至 +infinity |
| d                | 無           | 浮點數 | 頂點位置至光源位置的距離 | 無            |

 

-   若光線為定向光線，則 Atten = 1。
-   若光線與頂點之間的距離超過光線範圍，則 Atten = 0。

光線與頂點位置之間的距離一律為正值。

d = | L<sub>dir</sub> |

其中：

| 參數       | 預設值 | 類型                                             | 描述                                                 |
|-----------------|---------------|--------------------------------------------------|-------------------------------------------------------------|
| L<sub>dir</sub> | 無           | 3D 向量與 x、y 和 z 浮點值 | 從頂點位置至光線位置的方向向量 |

 

如果 d 大於光線範圍，Direct3D 不會進一步計算衰減，且不會從光線將效果套用至頂點。

衰減定值作為公式中的係數，您可對其進行簡單的調整來產生多種出現衰減曲線。 您可將 Attenuation1 設為 1.0，以建立不會衰減但仍受限於範圍的光線，或您可以嘗試使用不同的值，以取得各種衰減效果。

衰減的光線範圍上限不是 0.0。 若要防止光線在光線範圍時突然出現，應用程式可以增加光線範圍。 或者，應用程式可以設定衰減定值，使衰減因數的光線範圍接近 0.0。 衰減值會乘以光線色彩的紅、綠及藍元件，以將光線的強度擴充作為光線傳輸至頂點之距離的因數。

## <a name="span-idspotlight-factorspanspan-idspotlight-factorspanspan-idspotlight-factorspanspotlight-factor"></a><span id="Spotlight-Factor"></span><span id="spotlight-factor"></span><span id="SPOTLIGHT-FACTOR"></span>Spotlight 因素


以下方程式指定焦點因數。

![焦點因數的方程式](images/dx8light9.png)

| 參數         | 預設值 | 類型           | 描述                              | 範圍                    |
|-------------------|---------------|----------------|------------------------------------------|--------------------------|
| rho<sub>i</sub>   | 無           | 浮點數 | spotlight i 的 cosine(angle)            | 無                      |
| phi<sub>i</sub>   | 0.0           | 浮點數 | 焦點 i 在弧度的半影角度 | \[theta<sub>我</sub>，pi) |
| theta<sub>i</sub> | 0.0           | 浮點數 | 焦點 i 在弧度的本影角度    | \[0，pi)                 |
| falloff           | 0.0           | 浮點數 | Falloff 因數                           | (-infinity, +infinity)   |

 

其中：

rho = norm(L<sub>dcs</sub>)<sup>.</sup>norm(L<sub>dir</sub>)

和：

| 參數       | 預設值 | 類型                                             | 描述                                                 |
|-----------------|---------------|--------------------------------------------------|-------------------------------------------------------------|
| L<sub>dcs</sub> | 無           | 3D 向量與 x、y 和 z 浮點值 | 相機空間光線方向的負值         |
| L<sub>dir</sub> | 無           | 3D 向量與 x、y 和 z 浮點值 | 從頂點位置至光線位置的方向向量 |

 

計算光線衰減後，為了計算頂點的擴散和反射元件，Direct3D 還會考慮焦點效果 (如適用)、光線從表面反射的角度，以及目前材料的反射係數。 在[光類型](light-types.md)中，查看「焦點」。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[光源的數學運算](mathematics-of-lighting.md)

 

 




