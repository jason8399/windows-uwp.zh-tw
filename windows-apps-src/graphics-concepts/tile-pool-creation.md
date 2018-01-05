---
title: "建立磚集區"
description: "應用程式可以在每一個 Direct3D 裝置建立一個或多個磚集區。 每個磚集區的大小總和限制為 Direct3D 11 的資源大小上限，也就是約為圖形處理器 (GPU) RAM 的 1/4。"
ms.assetid: BD51EDD3-4AD3-4733-B014-DD77B9D743BB
keywords: "建立磚集區"
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4336d38bca354da3c30cfe2d7e4b092cff15af83
ms.sourcegitcommit: c80b9e6589a1ee29c5032a0b942e6a024c224ea7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/22/2017
---
# <a name="tile-pool-creation"></a>建立磚集區


應用程式可以在每一個 Direct3D 裝置建立一個或多個磚集區。 每個磚集區的大小總和限制為 Direct3D 11 的資源大小上限，也就是約為圖形處理器 (GPU) RAM 的 1/4。

磚集區由 64 KB 磚所組成，但作業系統 (顯示驅動程式) 會在幕後將整個集區作為一或多個配置來管理，但應用程式看不到分解過程。 串流資源藉由指向磚集區中的磚來定義內容。 您能藉由將磚指向**NULL**，以取消對應串流資源中的磚。 這類非對應的磚有讀取或寫入行為的規則；請參閱[磚集區資源的危險追蹤](hazard-tracking-versus-tile-pool-resources.md) (英文)。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[對應至磚集區中](mappings-are-into-a-tile-pool.md)

 

 



