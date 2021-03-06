---
title: BC6H 格式
description: BC6H 格式為一種設計用來支援來源資料中高動態範圍 (HDR) 色彩空間的紋理壓縮格式。
ms.assetid: 6781D967-9262-4EE7-B354-7A6D0EA0498E
keywords:
- BC6H 格式
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 50c8fa623130412688f14307fa46540c81f38554
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370478"
---
# <a name="bc6h-format"></a>BC6H 格式


BC6H 格式為一種設計用來支援來源資料中高動態範圍 (HDR) 色彩空間的紋理壓縮格式。

## <a name="span-idabout-bc6h-dxgi-format-bc6hspanspan-idabout-bc6h-dxgi-format-bc6hspanspan-idabout-bc6h-dxgi-format-bc6hspanabout-bc6hdxgiformatbc6h"></a><span id="About-BC6H-DXGI-FORMAT-BC6H"></span><span id="about-bc6h-dxgi-format-bc6h"></span><span id="ABOUT-BC6H-DXGI-FORMAT-BC6H"></span>關於 BC6H/DXGI\_格式\_BC6H


BC6H 格式針對使用了三個 HDR 色彩通道的影像，透過賦予每個值為 (16:16:16) 的色彩通道一個 16 位元值，提供高品質的壓縮。 目前仍不支援 Alpha 色板。

BC6H 由下列的 DXGI\_格式的列舉值：

-   **DXGI\_格式\_BC6H\_TYPELESS**。
-   **DXGI\_格式\_BC6H\_UF16**。 此 BC6H 格式在 16 位元浮點數的色彩通道數值中，不會使用正負號位元。
-   **DXGI\_FORMAT\_BC6H\_SF16**. 此 BC6H 格式在 16 位元浮點數的色彩通道數值中，使用正負號位元。

**附註**  浮動點格式，色頻的 16 位元通常稱為 「 半"浮點格式。 此格式以下列位元配置︰
|                       |                                                 |
|-----------------------|-------------------------------------------------|
| UF16 (不帶正負號的浮點數) | 5 個指數位元 + 11 個尾數位元              |
| SF16 (帶正負號的浮點數)   | 1 個正負號位元 + 5 個指數位元 + 10 個尾數位元 |

 

 

BC6H 格式可用於 [Texture2D](https://docs.microsoft.com/windows/desktop/direct3d10/d3d10-graphics-reference-resource-structures) (包括陣列)、Texture3D，或 TextureCube (包括陣列) 紋理資源。 同樣地，此格式適用於任何與這些資源建立關聯的 Mipmap 表面。

BC6H 使用固定的 16 位元組 (128 位元) 區塊大小，以及固定之 4x4 材質的磚大小。 與之前的 BC 格式一樣，大於支援之磚大小 (4x4) 的紋理影像，程式會使用多重區塊對其進行壓縮。 此定址身分識別也適用於 3D 影像、Mipmap、立方體貼圖，以及紋理陣列。 所有影像磚都必須格式相同。

BC6H 格式的注意事項︰

-   BC6H 支援浮點數反正規化，但不支援 INF (無限大) 和 NAN (不是數字)。 例外狀況是帶正負號的 BC6H 模式 (DXGI\_格式\_BC6H\_SF16)，可支援-INF （無限大的負數）。 這項對 -INF 的支援只不過是格式本身的變形，編碼器並未明確支援此種格式。 一般而言，編碼器遇到值為 INF (正或負) 或 NAN 的輸入資料時，編碼器應在壓縮前將資料轉換為其允許的最大非 INF 表示值，並將 NAN 對應至 0。
-   BC6H 不支援 Alpha 色板。
-   BC6H 解碼器在執行紋理篩選之前，會先執行解壓縮。
-   BC6H 解壓縮必須在位元上正確無誤，即硬體必須傳回與此份文件中描述的解碼器完全相同的結果。

## <a name="span-idbc6h-implementationspanspan-idbc6h-implementationspanspan-idbc6h-implementationspanbc6h-implementation"></a><span id="BC6H-implementation"></span><span id="bc6h-implementation"></span><span id="BC6H-IMPLEMENTATION"></span>BC6H 實作


BC6H 區塊由模式位元、壓縮端點、壓縮索引，以及選用的分割索引組成。 此格式指定了 14 種不同的模式。

端點色彩以 RGB 三元組儲存。 BC6H 透過一條通過數個使用者設定色彩端點的估計線，以決定色彩調色盤。 此外，根據模式的不同，磚可分割成兩個區域或視為單一區域。其中，分割成兩個區域的磚，針對每個區域皆會有其個別的色彩端點組。 BC6H 在每個材質上儲存一個調色盤索引。

在兩個區域的案例中，總共會有 32 種可能的分割。

## <a name="span-iddecoding-the-bc6h-formatspanspan-iddecoding-the-bc6h-formatspanspan-iddecoding-the-bc6h-formatspandecoding-the-bc6h-format"></a><span id="Decoding-the-BC6H-format"></span><span id="decoding-the-bc6h-format"></span><span id="DECODING-THE-BC6H-FORMAT"></span>解碼 BC6H 格式


下列虛擬程式碼描述於 16 位元組 BC6H 區塊中，解壓縮位於 (x,y) 點之像素的步驟。

``` syntax
decompress_bc6h(x, y, block)
{
    mode = extract_mode(block);
    endpoints;
    index;
    
    if(mode.type == ONE)
    {
        endpoints = extract_compressed_endpoints(mode, block);
        index = extract_index_ONE(x, y, block);
    }
    else //mode.type == TWO
    {
        partition = extract_partition(block);
        region = get_region(partition, x, y);
        endpoints = extract_compressed_endpoints(mode, region, block);
        index = extract_index_TWO(x, y, partition, block);
    }
    
    unquantize(endpoints);
    color = interpolate(index, endpoints);
    finish_unquantize(color);
}
```

下表包含了 BC6H 區塊中 14 種可能格式個別的位元計數及其數值。

| 模式 | 分割索引 | Partition | 色彩端點                  | 模式位元      |
|------|-------------------|-----------|----------------------------------|----------------|
| 1    | 46 個位元           | 5 個位元    | 75 個位元 (10.555, 10.555, 10.555) | 2 個位元 (00)    |
| 2    | 46 個位元           | 5 個位元    | 75 個位元 (7666, 7666, 7666)       | 2 個位元 (01)    |
| 3    | 46 個位元           | 5 個位元    | 72 個位元 (11.555, 11.444, 11.444) | 5 個位元 (00010) |
| 4    | 46 個位元           | 5 個位元    | 72 個位元 (11.444, 11.555, 11.444) | 5 個位元 (00110) |
| 5    | 46 個位元           | 5 個位元    | 72 個位元 (11.444, 11.444, 11.555) | 5 個位元 (01010) |
| 6    | 46 個位元           | 5 個位元    | 72 個位元 (9555, 9555, 9555)       | 5 個位元 (01110) |
| 7    | 46 個位元           | 5 個位元    | 72 個位元 (8666, 8555, 8555)       | 5 個位元 (10010) |
| 8    | 46 個位元           | 5 個位元    | 72 個位元 (8555, 8666, 8555)       | 5 個位元 (10110) |
| 9    | 46 個位元           | 5 個位元    | 72 個位元 (8555, 8555, 8666)       | 5 個位元 (11010) |
| 10   | 46 個位元           | 5 個位元    | 72 個位元 (6666, 6666, 6666)       | 5 個位元 (11110) |
| 11   | 63 個位元           | 0 個位元    | 60 個位元 (10.10, 10.10, 10.10)    | 5 個位元 (00011) |
| 12   | 63 個位元           | 0 個位元    | 60 個位元 (11.9, 11.9, 11.9)       | 5 個位元 (00111) |
| 13   | 63 個位元           | 0 個位元    | 60 個位元 (12.8, 12.8, 12.8)       | 5 個位元 (01011) |
| 14   | 63 個位元           | 0 個位元    | 60 個位元 (16.4, 16.4, 16.4)       | 5 個位元 (01111) |

 

此表格中的每個格式皆可利用其模式位元進行識別。 前 10 個模式使用於分割為兩個區域的磚，並且其模式位元欄位之長度可為二或五。 這些區塊也包含了壓縮色彩端點 (72 或 75 個位元)、分割 (5 個位元)，以及分割索引 (46 個位元) 的欄位。

針對壓縮色彩端點，上表中的數值標示了儲存 RGB 色彩端點的精確度，及每個色彩值所使用到的位元數。 例如：模式 3 指定了色彩端點精確度為 11，以及儲存轉換過後紅色、藍色，及綠色色彩端點差異值所需要的位元數。(分別為 5, 4, 4。) 模式 10 則並未使用 delta 壓縮，而是明確的儲存所有四個色彩端點。

最後四個區塊模式，則適用於單一區域的磚，其模式欄位為 5 個位元。 這些區塊都具備了端點 (60 個位元) 及壓縮索引 (63 個位元) 的欄位。 模式 11 (與模式 10 類似) 並未使用 delta 壓縮，而是將兩個色彩端點都明確儲存。

模式 10011、10111、11011，以及 11111 (未顯示) 目前則作為保留用途使用。 請勿將這些運用在您的編碼器中。 如果硬體接收到了明確指定為這些模式當中其中一種的區塊，最後解壓縮的結果區塊必須在除 Alpha 色板之外全部的通道中包含所有的 0。

針對 BC6H，無論模式為何，Alpha 色板都必須傳回 1.0。

### <a name="span-idbc6h-partition-setspanspan-idbc6h-partition-setspanspan-idbc6h-partition-setspanbc6h-partition-set"></a><span id="BC6H-partition-set"></span><span id="bc6h-partition-set"></span><span id="BC6H-PARTITION-SET"></span>BC6H 分割集

兩個區域的磚共有 32 種可能的分割組合，其定義如下表。 每個 4x4 的區塊代表了單一圖形。

![BC6H 分割組合列表](images/bc6h-partition-sets.png)

在此表格中的分割組合，以粗體及底線標示的項目，即為其子集 1 固定索引的位置 (以少於其母集 1 個位元表示)。 子集 0 的固定索引一律都是索引 0，因為分割一律會進行排列，使得索引 0 一律會位於子集 0。 分割的順序為：由左上至右下，由左移至右，再由上至下。

## <a name="span-idbc6h-compressed-endpoint-formatspanspan-idbc6h-compressed-endpoint-formatspanspan-idbc6h-compressed-endpoint-formatspanbc6h-compressed-endpoint-format"></a><span id="BC6H-compressed-endpoint-format"></span><span id="bc6h-compressed-endpoint-format"></span><span id="BC6H-COMPRESSED-ENDPOINT-FORMAT"></span>BC6H 壓縮的端點格式


![BC6H 壓縮端點格式的位元欄位](images/bc6h-headers-med.png)

此表格將壓縮端點的位元欄位以端點格式的功能呈現。每一欄都指定了一種編碼方式，每一列都指定了一個位元欄位。 利用此種方法，兩個區域的磚會使用到 82 個位元，單一區域的磚則會使用到 65 個位元。 例如前, 5 個位元，其中一個區域\[16 4\]編碼 （特別是最右側資料行） 上方是位元 m\[4:0\]，接下來的 10 個位元是位元 rw\[9:0\]等等使用最後的 6 位元包含 bw\[10:15\]。

上述表格中的欄位名稱定義如下︰

| 欄位 | 變數          |
|-------|-------------------|
| m     | mode              |
| d     | 圖形索引       |
| rw    | endpt\[0\].A\[0\] |
| rx    | endpt\[0\].B\[0\] |
| ry    | endpt\[1\].A\[0\] |
| rz    | endpt\[1\].B\[0\] |
| gw    | endpt\[0\].A\[1\] |
| gx    | endpt\[0\].B\[1\] |
| gy    | endpt\[1\]。A\[1\] |
| gz    | endpt\[1\].B\[1\] |
| bw    | endpt\[0\].A\[2\] |
| bx    | endpt\[0\].B\[2\] |
| 由    | endpt\[1\].A\[2\] |
| bz    | endpt\[1\].B\[2\] |

 

Endpt\[我\]，其中是 0 或 1，分別是指第 0 個或第 1 組端點。
## <a name="span-idsign-extension-for-endpoint-valuesspanspan-idsign-extension-for-endpoint-valuesspanspan-idsign-extension-for-endpoint-valuesspansign-extension-for-endpoint-values"></a><span id="Sign-extension-for-endpoint-values"></span><span id="sign-extension-for-endpoint-values"></span><span id="SIGN-EXTENSION-FOR-ENDPOINT-VALUES"></span>端點值的正負號擴充


針對兩個區域的磚，總共有四個端點數值可進行正負號擴充。 Endpt\[0\]。已簽署，只有格式是帶正負號的格式，只有當端點已轉換，或如果格式為帶正負號的格式，則會簽署其他端點。 下方的程式碼示範了將兩個區域端點數值的正負號擴充的演算法。

``` syntax
static void sign_extend_two_region(Pattern &p, IntEndpts endpts[NREGIONS_TWO])
{
    for (int i=0; i<NCHANNELS; ++i)
    {
      if (BC6H::FORMAT == SIGNED_F16)
        endpts[0].A[i] = SIGN_EXTEND(endpts[0].A[i], p.chan[i].prec);
      if (p.transformed || BC6H::FORMAT == SIGNED_F16)
      {
        endpts[0].B[i] = SIGN_EXTEND(endpts[0].B[i], p.chan[i].delta[0]);
        endpts[1].A[i] = SIGN_EXTEND(endpts[1].A[i], p.chan[i].delta[1]);
        endpts[1].B[i] = SIGN_EXTEND(endpts[1].B[i], p.chan[i].delta[2]);
      }
    }
}
```

其中一個區域圖格，行為是一樣的只能搭配 endpt\[1\]移除。

``` syntax
static void sign_extend_one_region(Pattern &p, IntEndpts endpts[NREGIONS_ONE])
{
    for (int i=0; i<NCHANNELS; ++i)
    {
    if (BC6H::FORMAT == SIGNED_F16)
        endpts[0].A[i] = SIGN_EXTEND(endpts[0].A[i], p.chan[i].prec);
    if (p.transformed || BC6H::FORMAT == SIGNED_F16) 
        endpts[0].B[i] = SIGN_EXTEND(endpts[0].B[i], p.chan[i].delta[0]);
    }
}
```

## <a name="span-idtransform-inversion-for-endpoint-valuesspanspan-idtransform-inversion-for-endpoint-valuesspanspan-idtransform-inversion-for-endpoint-valuesspantransform-inversion-for-endpoint-values"></a><span id="Transform-inversion-for-endpoint-values"></span><span id="transform-inversion-for-endpoint-values"></span><span id="TRANSFORM-INVERSION-FOR-ENDPOINT-VALUES"></span>轉換的端點值的反轉


針對兩個區域的圖格，轉換會套用的編碼方式，在 endpt 加入基底值的差異反向\[0\]。A 到其他三個共 9 項加入作業。 在下列影像中，基底值表示為「A0」，並且有著最高的浮點數精確度。 「A1」、「B0」，以及「B1」都是從錨點值計算而來的差異值，並且都以較低的精確度表示。 (A0 對應至 endpt\[0\]。A，B0 對應至 endpt\[0\]。B，A1 對應至 endpt\[1\]。A，而 B1 對應至 endpt\[1\]。B)

![端點數值反向轉換的計算](images/bc6h-transform-inverse.png)

針對單一區域的磚，由於只有一個 delta 位移，因此只會有 3 個新增運算。

解壓縮程式必須確定，結果的反向轉換不會溢位的有效位數 endpt\[0\]。 一旦溢位發生，反向轉換的結果值必須包裝於相同數量的位元中。 如果 A0 的精確度為「p」個位元，其轉換演算法為︰

`B0 = (B0 + A0) & ((1 << p) - 1)`

針對帶正負號的格式，delta 計算的結果也必須要進行正負號擴充。 若正負號擴充運算意圖擴充兩種符號，而 0 代表正向，1 代表負向時，則 0 的正負號擴充會負責處理上述限制。 同樣的，在上述限制之後，只有數值為 1 (負向) 的才需要進行正負號擴充。

## <a name="span-idunquantization-of-color-endpointsspanspan-idunquantization-of-color-endpointsspanspan-idunquantization-of-color-endpointsspanunquantization-of-color-endpoints"></a><span id="Unquantization-of-color-endpoints"></span><span id="unquantization-of-color-endpoints"></span><span id="UNQUANTIZATION-OF-COLOR-ENDPOINTS"></span>Unquantization 的色彩端點


若給予未壓縮的端點，下一個步驟即是對色彩端點進行初步的取消量化。 這牽涉到三個步驟︰

-   取消量化色彩調色盤
-   插補調色盤
-   完成取消量化

將取消量化的程序分成兩個部分 (插補調色盤前的取消量化色彩調色盤，及插補之後的完成取消量化)，相較於插補調色盤前就進行完整的取消量化程序，可以減少需要進行的乘法運算次數。

下方的程式碼示範了擷取原始 16 位元色彩的估計值，並且利用提供的權數值將 6 個額外的色彩值新增至調色盤中的程序。 每個通道都會執行相同的作業。

``` syntax
int aWeight3[] = {0, 9, 18, 27, 37, 46, 55, 64};
int aWeight4[] = {0, 4, 9, 13, 17, 21, 26, 30, 34, 38, 43, 47, 51, 55, 60, 64};

// c1, c2: endpoints of a component
void generate_palette_unquantized(UINT8 uNumIndices, int c1, int c2, int prec, UINT16 palette[NINDICES])
{
    int* aWeights;
    if(uNumIndices == 8)
        aWeights = aWeight3;
    else  // uNumIndices == 16
        aWeights = aWeight4;

    int a = unquantize(c1, prec); 
    int b = unquantize(c2, prec);

    // interpolate
    for(int i = 0; i < uNumIndices; ++i)
        palette[i] = finish_unquantize((a * (64 - aWeights[i]) + b * aWeights[i] + 32) >> 6);
}
```

下一個程式碼範例示範了插補的程序，請觀察下列過程︰

-   由於 **unquantize** 函式 (請參閱下方程式碼) 的色彩值完整範圍是從 -32768 到 65535，插補器即透過 17 位元帶正負號的算術進行實作。
-   在插補之後, 的值會傳遞給**完成\_unquantize**函數 （第三個範例，這一節中說明），這會套用最後的調整。
-   所有硬體解壓縮程式在利用這些函式時，都必須傳回在位元上正確無誤的結果。

``` syntax
int unquantize(int comp, int uBitsPerComp)
{
    int unq, s = 0;
    switch(BC6H::FORMAT)
    {
    case UNSIGNED_F16:
        if(uBitsPerComp >= 15)
            unq = comp;
        else if(comp == 0)
            unq = 0;
        else if(comp == ((1 << uBitsPerComp) - 1))
            unq = 0xFFFF;
        else
            unq = ((comp << 16) + 0x8000) >> uBitsPerComp;
        break;

    case SIGNED_F16:
        if(uBitsPerComp >= 16)
            unq = comp;
        else
        {
            if(comp < 0)
            {
                s = 1;
                comp = -comp;
            }

            if(comp == 0)
                unq = 0;
            else if(comp >= ((1 << (uBitsPerComp - 1)) - 1))
                unq = 0x7FFF;
            else
                unq = ((comp << 15) + 0x4000) >> (uBitsPerComp-1);

            if(s)
                unq = -unq;
        }
        break;
    }
    return unq;
}
```

**完成\_unquantize**調色盤內插補點之後呼叫。 **unquantize** 函式會針對帶正負號及不帶正負號的對象，分別以 31/32 及 31/64 延遲其縮放。 為了在插補調色盤完成之後使最終數值落入一半的有效範圍 (-0x7BFF ~ 0x7BFF)，並且減少需要進行的乘法運算次數，此行為是必要的。 **完成\_unquantize**套用最後的調整，並傳回**unsigned short**值，取得重新解譯成**一半**。

``` syntax
unsigned short finish_unquantize(int comp)
{
    if(BC6H::FORMAT == UNSIGNED_F16)
    {
        comp = (comp * 31) >> 6;                                         // scale the magnitude by 31/64
        return (unsigned short) comp;
    }
    else // (BC6H::FORMAT == SIGNED_F16)
    {
        comp = (comp < 0) ? -(((-comp) * 31) >> 5) : (comp * 31) >> 5;   // scale the magnitude by 31/32
        int s = 0;
        if(comp < 0)
        {
            s = 0x8000;
            comp = -comp;
        }
        return (unsigned short) (s | comp);
    }
}
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[紋理區塊壓縮](texture-block-compression.md)

 

 




