---
title: Mipmap 封裝
description: 有些 mips 數量 (每一陣列配量) 可以封裝為部分磚數，根據串流資源的維度、格式、Mipmaps 數，以及陣列配量。
ms.assetid: 906C3CAC-4E84-4947-B508-06788551BE85
keywords:
- Mipmap 封裝
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8b2733da1f843062a1fa7f2b4a7969326523d54e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608103"
---
# <a name="mipmap-packing"></a>Mipmap 封裝


有些 mips 數量 (每一陣列配量) 可以封裝為部分磚數，根據串流資源的維度、格式、Mipmaps 數，以及陣列配量。

根據串流資源支援的[階層](streaming-resources-features-tiers.md)，具特定維度的 Mipmaps 未依照標準磚外形，且與彼此以對應用程式不透明的方式全部彼此封裝一起。 較高階層的支援更加保證何種表面維度類型符合標準磚外形 (應用程式可因此個別對應)。

實作之間會有變化的是 - 假設串流資源的維度、格式、Mipmaps 數量和陣列配量 - 有些 Mips 的 M 數(每一陣列配量) 可以封裝成一些 N 個磚數。 當您取得裝置的資源並排資訊，驅動程式會向應用程式報告 M 和 N 各是多少 (介於表面的其他標準詳細資料之中，不會因硬體廠商的不同而有所不同)。 已封裝的 Mips 的磚組合仍是 64 KB，可以個別對應到磚集區中的不同位置。

但是動態磚的像素形狀，以及 Mipmaps 如何跨過磚組合進行調整因硬體廠商而異，因複雜度過高而無法公開。 因此，應用程式需要每次對應到指定封裝的所有磚，或是都不對應。 否則，不定義存取串流資源的行為。

對於陣列式表面，已封裝的 mips 組合以及儲存那些 mips 封裝磚數 (前面描述的 M 和 N)，會針對各陣列配量單獨套用。

用於複製磚的專用 API 無法存取封裝的 mips。 要從封裝的 mips 複製資料或複製資料到封裝的 mips 的應用程式，可以使用所有非串流特定資源 API 複製和轉譯到表面。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[資料流資源的區域會並排顯示方式](how-a-streaming-resource-s-area-is-tiled.md)

 

 




