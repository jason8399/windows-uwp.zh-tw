---
title: 緩衝區簡介
description: 緩衝資源是一個完整輸入的資料集合，由元素所組成。
ms.assetid: 494FDF57-0FBE-434C-B568-06F977B40263
keywords:
- 緩衝區簡介
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: deeae0cc66a7e75da2e44c0d2aba2a9ed459b824
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602573"
---
# <a name="introduction-to-buffers"></a>緩衝區簡介


緩衝資源是一個完整輸入的資料集合，由元素所組成。 緩衝區儲存資料，例如*頂點緩衝區*中的紋理座標、*索引緩衝區*中的索引、*常數緩衝區*中的著色器常數資料、位置向量、法向向量或裝置狀態。

緩衝元素由 1 到 4 個元件組成。 緩衝元素可以包含封裝資料值 (像是 R8G8B8A8 面值)、單一 8 位整數，或四個 32 位元浮點數值。

建立緩衝區做為未結構化的資源。 因為未建立，緩衝不能包含任何 mipmap 層次，當讀取時無法篩選，而且無法多重採樣。

## <a name="span-idbuffertypesspanspan-idbuffertypesspanspan-idbuffertypesspanbuffer-types"></a><span id="Buffer_Types"></span><span id="buffer_types"></span><span id="BUFFER_TYPES"></span>緩衝區類型


以下是 Direct3D 11 支援的緩衝區資源類型。

-   [頂點緩衝區](#vertex-buffer)
-   [索引緩衝區](#index-buffer)
-   [常數緩衝區](#shader-constant-buffer)

### <a name="span-idvertexbufferspanspan-idvertexbufferspanspan-idvertexbufferspanspan-idvertex-bufferspanvertex-buffer"></a><span id="Vertex_Buffer"></span><span id="vertex_buffer"></span><span id="VERTEX_BUFFER"></span><span id="vertex-buffer"></span>頂點緩衝區

頂點緩衝區包含用來定義您的幾何頂點資料。 頂點資料包括位置座標、色彩資料、紋理座標資料、一般資料等等。

最簡單的頂點緩衝區範例是僅包含位置資料者。 它可以像下圖一樣視覺化。

![包含位置資料的頂點緩衝區圖例](images/d3d10-resources-single-element-vb2.png)

頂點緩衝區經常包含可完整指定 3D 頂點所需的所有資料。 例如，可能會包含每個頂點位置、一般及紋理座標的頂點緩衝區。 此資料通常組織成每個頂點元素集，如下圖所示。

![頂點緩區的圖例，其包含位置、一般和紋理資料](images/d3d10-vertex-buffer-element.png)

這個頂點緩衝區包含每個頂點資料，而每個頂點儲存三個元素 (位置、一般和紋理座標)。 位置與一般座標通常會使用三個 32 位元浮點數來指定，紋理座標則會使用兩個 32 位元浮點數來指定。

若要存取頂點緩衝區的資料，您需要知道所存取的頂點，再加上以下額外緩衝區參數︰

-   位移 - 從緩衝區的起始到第一個頂點資料的位元組數目。
-   BaseVertexLocation - 從位移到適當繪製呼叫所使用的第一個頂點的位元組數目。

建立頂點緩衝區之前，您需要定義其配置。 建立輸入配置物件之後，您將它繫結至[輸入組合語言 (IA) 階段](input-assembler-stage--ia-.md)。

### <a name="span-idindexbufferspanspan-idindexbufferspanspan-idindexbufferspanspan-idindex-bufferspanindex-buffer"></a><span id="Index_Buffer"></span><span id="index_buffer"></span><span id="INDEX_BUFFER"></span><span id="index-buffer"></span>索引緩衝區

索引緩衝包含整數位移至頂點緩衝區，可用於更有效率地呈現基本類型。 索引緩衝區包含連續的 16 位元或 32 位元索引集，而每個索引用來識別頂點緩衝區中的頂點。 索引緩衝區可以視覺化，如下圖所示。

![索引緩衝區的圖例](images/d3d10-index-buffer.png)

儲存在索引緩衝區中的循序索引，使用以下參數尋找︰

-   Offset - 索引緩衝的基底位址的位元組數目。
-   StartIndexLocation - 指定基底位址和位移的第一個索引緩衝元素。 代表呈現的第一個索引的起始位置。
-   IndexCount - 要呈現的索引數目。

索引緩衝區的開頭 = 索引緩衝區基底位址 + 位移 （位元組） + StartIndexLocation \* ElementSize （位元組）;

在這個運算中，ElementSize 是各索引緩衝元素的大小，不是兩個就是四個位元組。

### <a name="span-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshader-constant-bufferspanconstant-buffer"></a><span id="Shader_Constant_Buffer"></span><span id="shader_constant_buffer"></span><span id="SHADER_CONSTANT_BUFFER"></span><span id="shader-constant-buffer"></span>常數緩衝區

常數緩衝區可讓您有效率地提供著色器常數資料至管線。 您可以使用常數緩衝區來儲存資料流輸出階段的結果。 概念上，常數緩衝區看起來很像單一元素頂點緩衝區，如下圖所示。

![著色器常數緩衝區的圖例](images/d3d10-shader-resource-buffer.png)

每個元素會儲存 1 到 4 個元件常數，取決於儲存的資料格式。

常數緩衝區可以只使用無法與任何其他繫結旗標結合的單一繫結旗標。

若要讀取著色器常數緩衝區，請使用 HLSL 載入功能。 每個著色器階段允許最多 15 個著色器常數緩衝區，而每個緩衝區可保留最多 4096 個常數。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[頂點和索引緩衝區](vertex-and-index-buffers.md)

 

 




