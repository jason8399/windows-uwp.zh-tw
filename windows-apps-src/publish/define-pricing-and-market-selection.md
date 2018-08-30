---
author: jnHs
Description: The Microsoft Store reaches customers in over 200 countries and regions around the world.
title: 定義市場選取項目
ms.assetid: FBE7507B-DBF3-4FCB-8377-DB01660E75F8
ms.author: wdg-dev-content
ms.date: 06/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 市場, 國家, 地區
ms.localizationpriority: medium
ms.openlocfilehash: dd8cdb1f69a9a8a73700483f04d17f64de337347
ms.sourcegitcommit: 7efffcc715a4be26f0cf7f7e249653d8c356319b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/30/2018
ms.locfileid: "3114665"
---
# <a name="define-market-selection"></a>定義市場選取項目


Microsoft Store 可觸及全球 200 多個國家與地區的客戶。 您可以選擇提供應用程式的市場，且利用此選項依據市場或市場群組自訂許多[價格與可用性](set-app-pricing-and-availability.md)功能。

為了讓您的 app 適用於客戶世界各地的資訊，請參閱[全球化指導方針](../design/globalizing/guidelines-and-checklist-for-globalizing-your-app.md)，並[讓您的應用程式可當地語系化](../design/globalizing/prepare-your-app-for-localization.md)。

> [!NOTE]
> 雖然本主題是關於應用程式，但是附加元件提交的市場選擇使用相同的程序。

## <a name="markets"></a>市場

根據預設，我們會在所有可能的市場中，以基本價格提供您的應用程式，包括任何我們未來可能會新增的市場。

如果您想要的話，可以定義想要提供應用程式的特定市場。 若要這樣做，請選取 [**價格和可用性**] 頁面的 [**市場**] 區段中的 [**顯示選項**]。 這將會顯示 [**市場選取項目**] 快顯視窗，您可以在這裡選擇提供應用程式的市場。

根據預設，所有市場都會選取。 您可以取消選取個別市場以排除它們，或按一下 [**取消全選**]，然後新增您選擇的個別市場。 您可以在搜尋列中搜尋特定市場，也可以將 [**所有市場**] 下拉式清單變更為 [**Xbox 市場**]，如果您只想要檢視能夠銷售 Xbox 產品的市場。 完成後，按一下 [**確定**] 儲存您的選擇。

請注意，您在這裡選取的項目只會套用至新的取得項目；如果某人已在特定市場中擁有您的應用程式，而您之後移除該市場，該市場中已擁有應用程式的人仍可繼續使用，但是無法取得您提交的更新，而且該市場中沒有新客戶可取得您的應用程式。

> [!IMPORTANT]
> 遵循當地的任何法規要求是您的責任，即使這裡或 Windows 開發人員中心儀表板並未列出這些要求。

請記住，即使您選取全部的市場，當地法律限制或其他因素都可能造成某些 App 無法在部分國家/地區中列出。 此外，某些市場可能會有與年齡分級相關的特定需求。 如果您的 app 不符合這些需求，我們將無法在該市場中提供您的 app。 如需詳細資訊，請參閱[年齡分級](age-ratings.md)。

> [!NOTE]
> 對於目標為 Windows 8 或 Windows 8.1 的應用程式，某些個別市場會將其視為一個單獨的「世界其他地方」市場。 如需詳細資訊，請參閱[針對 Windows 8.x 的「世界其他地方」市場](#rest-of-world-markets-for-windows-8x)。

您也會看到一個核取方塊，讓您指定是否要在市集未來可能新增的市場中提供您的應用程式。 若您讓此方塊保持核取狀態，而我們之後新增市場，則在這些市場中會針對您的應用程式使用您的提交項目的基本價格和一般可用性日期。 如果您不希望這種情況發生，可以取消核取此方塊，如此我們就不會將您的應用程式列在未來任何市場中 (不過您之後可以隨時自行新增)。
 

## <a name="microsoft-store-consumer-markets"></a>Microsoft 市集消費者市場

您可以選擇在下列一或多個市集中列出您的應用程式 (或附加元件)。 標示星號的市場支援 Xbox One; 上的 Microsoft 網上商店您會看到**Xbox** **市場選取項目**快顯視窗中其名稱旁。


<table>
  
  <tr>
    <td>阿富汗</td>
    <td>奧蘭島</td>
    <td>阿爾巴尼亞</td>
    <td>阿爾及利亞</td>
  </tr>
  <tr>
    <td>美屬薩摩亞</td>
    <td>安道爾</td>
    <td>安哥拉</td>
    <td>安圭拉</td>
  </tr>
  <tr>
    <td>南極大陸</td>
    <td>安地卡及巴布達</td>
    <td>阿根廷</td>
    <td>亞美尼亞</td>
  </tr>
  <tr>
    <td>阿路巴</td>
    <td>澳大利亞</td>
    <td>奧地利</td>
    <td>亞塞拜然</td>
  </tr>
  <tr>
    <td>巴哈馬</td>
    <td>巴林</td>
    <td>孟加拉</td>
    <td>巴貝多</td>
  </tr>
  <tr>
    <td>白俄羅斯</td>
    <td>比利時</td>
    <td>貝里斯</td>
    <td>貝南</td>
  </tr>
  <tr>
    <td>百慕達</td>
    <td>不丹</td>
    <td>玻利維亞</td>
    <td>波奈</td>
  </tr>
  <tr>
    <td>波士尼亞與赫塞哥維納</td>
    <td>波札那</td>
    <td>布威島</td>
    <td>巴西</td>
  </tr>
  <tr>
    <td>英屬印度洋領土</td>
    <td>英屬維爾京群島</td>
    <td>汶萊</td>
    <td>保加利亞</td>
  </tr>
  <tr>
    <td>布吉納法索</td>
    <td>蒲隆地</td>
    <td>維德角</td>
    <td>柬埔寨</td>
  </tr>
  <tr>
    <td>喀麥隆</td>
    <td>加拿大</td>
    <td>開曼群島</td>
    <td>中非共和國</td>
  </tr>
  <tr>
    <td>查德</td>
    <td>智利</td>
    <td>中國</td>
    <td>聖誕島</td>
  </tr>
  <tr>
    <td>可可斯群島</td>
    <td>哥倫比亞 *</td>
    <td>葛摩</td>
    <td>剛果共和國</td>
  </tr>
  <tr>
    <td>剛果民主共和國 (DRC)</td>
    <td>柯克群島</td>
    <td>哥斯大黎加</td>
    <td>科特迪瓦 (Côte d’Ivoire)</td>
  </tr>
  <tr>
    <td>克羅埃西亞</td>
    <td>古拉梳</td>
    <td>賽普勒斯</td>
    <td>捷克共和國 *</td>
  </tr>
  <tr>
    <td>丹麥 *</td>
    <td>吉布地</td>
    <td>多米尼克</td>
    <td>多明尼加共和國</td>
  </tr>
  <tr>
    <td>厄瓜多</td>
    <td>埃及</td>
    <td>薩爾瓦多</td>
    <td>赤道幾內亞</td>
  </tr>
  <tr>
    <td>厄利垂亞</td>
    <td>愛沙尼亞</td>
    <td>衣索比亞</td>
    <td>福克蘭群島</td>
  </tr>
  <tr>
    <td>法羅群島</td>
    <td>斐濟群島</td>
    <td>芬蘭 *</td>
    <td>法國 *</td>
  </tr>
  <tr>
    <td>法屬圭亞那</td>
    <td>法屬玻里尼西亞</td>
    <td>法國南方領土及南極陸地</td>
    <td>加彭</td>
  </tr>
  <tr>
    <td>甘比亞</td>
    <td>喬治亞</td>
    <td>德國 *</td>
    <td>迦納</td>
  </tr>
  <tr>
    <td>直布羅陀</td>
    <td>希臘 *</td>
    <td>格陵蘭</td>
    <td>格瑞那達</td>
  </tr>
  <tr>
    <td>哥德普洛</td>
    <td>關島</td>
    <td>瓜地馬拉</td>
    <td>根息</td>
  </tr>
  <tr>
    <td>幾內亞</td>
    <td>幾內亞比索</td>
    <td>蓋亞納</td>
    <td>海地</td>
  </tr>
  <tr>
    <td>赫德島及麥當勞群島</td>
    <td>宏都拉斯</td>
    <td>香港特別行政區 *</td>
    <td>匈牙利 *</td>
  </tr>
  <tr>
    <td>冰島</td>
    <td>印度 *</td>
    <td>印尼</td>
    <td>伊拉克</td>
  </tr>
  <tr>
    <td>愛爾蘭 *</td>
    <td>曼城島</td>
    <td>以色列 *</td>
    <td>義大利 *</td>
  </tr>
  <tr>
    <td>牙買加</td>
    <td>日本 *</td>
    <td>澤西島</td>
    <td>約旦</td>
  </tr>
  <tr>
    <td>哈薩克</td>
    <td>肯亞</td>
    <td>吉里巴斯</td>
    <td>韓國 *</td>
  </tr>
  <tr>
    <td>科威特</td>
    <td>吉爾吉斯</td>
    <td>寮國</td>
    <td>拉脫維亞</td>
  </tr>
  <tr>
    <td>黎巴嫩</td>
    <td>賴索托</td>
    <td>賴比瑞亞</td>
    <td>利比亞</td>
  </tr>
  <tr>
    <td>列支敦斯登</td>
    <td>立陶宛</td>
    <td>盧森堡</td>
    <td>澳門特別行政區</td>
  </tr>
  <tr>
    <td>馬其頓 (FYRO)</td>
    <td>馬達加斯加</td>
    <td>馬拉威</td>
    <td>馬來西亞</td>
  </tr>
  <tr>
    <td>馬爾地夫</td>
    <td>馬利</td>
    <td>馬爾他</td>
    <td>馬紹爾群島</td>
  </tr>
  <tr>
    <td>馬丁尼克</td>
    <td>茅利塔尼亞</td>
    <td>模里西斯</td>
    <td>馬約特島</td>
  </tr>
  <tr>
    <td>墨西哥 *</td>
    <td>密克羅尼西亞</td>
    <td>摩爾多瓦</td>
    <td>摩納哥</td>
  </tr>
  <tr>
    <td>蒙古</td>
    <td>蒙特內哥羅</td>
    <td>蒙特色拉特島</td>
    <td>摩洛哥</td>
  </tr>
  <tr>
    <td>莫三比克</td>
    <td>緬甸</td>
    <td>納米比亞</td>
    <td>諾魯</td>
  </tr>
  <tr>
    <td>尼泊爾</td>
    <td>荷蘭 *</td>
    <td>新喀里多尼亞群島</td>
    <td>紐西蘭 *</td>
  </tr>
  <tr>
    <td>尼加拉瓜</td>
    <td>尼日</td>
    <td>奈及利亞</td>
    <td>紐威島</td>
  </tr>
  <tr>
    <td>諾福克島</td>
    <td>北馬里安納群島</td>
    <td>挪威 *</td>
    <td>阿曼</td>
  </tr>
  <tr>
    <td>巴基斯坦</td>
    <td>帛琉</td>
    <td>巴勒斯坦民族權力機構</td>
    <td>巴拿馬</td>
  </tr>
  <tr>
    <td>巴布亞紐幾內亞</td>
    <td>巴拉圭</td>
    <td>秘魯</td>
    <td>菲律賓</td>
  </tr>
  <tr>
    <td>皮特康群島</td>
    <td>波蘭 *</td>
    <td>葡萄牙 *</td>
    <td>卡達</td>
  </tr>
  <tr>
    <td>留尼旺</td>
    <td>羅馬尼亞</td>
    <td>俄羅斯 *</td>
    <td>盧安達</td>
  </tr>
  <tr>
    <td>聖巴瑟米</td>
    <td>聖赫勒拿、亞森欣、特里斯坦達庫尼亞群島</td>
    <td>聖克里斯多福及尼維斯</td>
    <td>聖露西亞</td>
  </tr>
  <tr>
    <td>法屬聖馬丁</td>
    <td>聖匹島</td>
    <td>聖文森及格瑞那丁</td>
    <td>薩摩亞</td>
  </tr>
  <tr>
    <td>聖馬利諾</td>
    <td>聖多美普林西比</td>
    <td>沙烏地阿拉伯 *</td>
    <td>塞內加爾</td>
  </tr>
  <tr>
    <td>賽爾維亞</td>
    <td>塞席爾</td>
    <td>獅子山</td>
    <td>新加坡 *</td>
  </tr>
  <tr>
    <td>荷屬聖馬丁</td>
    <td>斯洛伐克 *</td>
    <td>斯洛維尼亞</td>
    <td>索羅門群島</td>
  </tr>
  <tr>
    <td>索馬利亞</td>
    <td>南非 *</td>
    <td>南喬治亞及南三明治群島</td>
    <td>西班牙 *</td>
  </tr>
  <tr>
    <td>斯里蘭卡</td>
    <td>蘇利南</td>
    <td>冷岸及央棉</td>
    <td>史瓦濟蘭</td>
  </tr>
  <tr>
    <td>瑞典 *</td>
    <td>瑞士 *</td>
    <td>台灣 *</td>
    <td>塔吉克</td>
  </tr>
  <tr>
    <td>坦尚尼亞</td>
    <td>泰國</td>
    <td>東帝汶</td>
    <td>多哥</td>
  </tr>
  <tr>
    <td>托克勞群島</td>
    <td>東加</td>
    <td>千里達及托巴哥</td>
    <td>突尼西亞</td>
  </tr>
  <tr>
    <td>土耳其 *</td>
    <td>土庫曼</td>
    <td>土克斯及開科斯群島</td>
    <td>吐瓦魯</td>
  </tr>
  <tr>
    <td>美國外島</td>
    <td>美屬維爾京群島</td>
    <td>烏干達</td>
    <td>烏克蘭</td>
  </tr>
  <tr>
    <td>阿拉伯聯合大公國 *</td>
    <td>英國 *</td>
    <td>美國 *</td>
    <td>烏拉圭</td>
  </tr>
  <tr>
    <td>烏茲別克</td>
    <td>萬那杜</td>
    <td>梵蒂岡</td>
    <td>委內瑞拉</td>
  </tr>
  <tr>
    <td>越南</td>
    <td>瓦利斯及福杜納</td>
    <td>西撒哈拉 (爭議)</td>
    <td>葉門</td>
  </tr>
  <tr>
    <td>尚比亞</td>
    <td>辛巴威</td>
    <td></td>
    <td></td>
  </tr>
</table>


## <a name="price-considerations-for-specific-markets"></a>特定市場的定價考量

像是禮品卡和電信業者帳單等付款方式，有助於提高付費 app 和 app 內購買項目的銷售量。 由於使用這類付款方式的成本較高，因此在使用下表所列付款方式的國家/區域中，計算付費 app 與 app 內購買交易的應付「App 收益」時，「市集費用」會再從「淨收入」扣除「商業拓展調整」。 如果您 app 上市的國家/地區適用「商業拓展調整」，則您制定市場定價策略時，應將它納入考量因素。 您可在[應用程式開發人員合約](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)中，找到「商業拓展調整」的詳細資料。

從生效日期開始，針對特定「國家/地區」與「付款方式」處理的所有交易都將套用「商業拓展調整」。 這項資訊將會每月更新；「商業拓展調整」在新的國家/地區與付款方式生效後的三十 (30) 天內，就會列出該國家/地區與付款方式。

&nbsp;

| 國家/地區       | 付款方式  | 商業拓展調整 | 生效日期 |
|----------------------|-----------------|-------------------------------|----------------|
| 阿根廷            | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 澳大利亞            | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 奧地利              | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 比利時              | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 巴西               | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 加拿大               | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 智利                | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 中國                | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 哥倫比亞             | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 捷克共和國       | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 丹麥              | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 芬蘭              | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 法國               | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 德國              | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 希臘               | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 香港            | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 匈牙利              | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 印度                | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 愛爾蘭              | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 以色列               | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 義大利                | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 日本                | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 墨西哥               | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 荷蘭          | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 紐西蘭          | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 波蘭               | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 葡萄牙             | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 俄羅斯               | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 沙烏地阿拉伯         | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 新加坡            | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 斯洛伐克             | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 南非         | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 南韓          | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 西班牙                | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 瑞典               | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 瑞士          | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 台灣               | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 土耳其               | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 阿拉伯聯合大公國 | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 英國       | 禮品卡       | 2.24%                         | 2016 年 3 月     |
| 美國        | 禮品卡       | 2.24%                         | 2016 年 3 月     |

 

## <a name="rest-of-world-markets-for-windows-8x"></a>針對 Windows 8.x 的「世界其他地方」市場

如果您的應用程式內容包含套件目標為 Windows 8.x，請務必注意市場的數字會被視為單一 「 世界其他地方 」 市場的客戶在 Windows 上使用 microsoft Store 8.x，即使它們會顯示為 Windows 開發人員中心中的個別市場儀表板 (而不是較舊版本的市集儀表板，其中有已將所有這些市場的其中一個的 「 世界其他地方 」 市場選項)。

如果提交您 app 時保留預設的選項，則您不需要擔心這些事，且所有可能的市場都將能使用您的 app。 然而，如果您想要排除某些市場，請記住：就算您只排除「世界其他地方」市場中的一個市場，「世界其他地方」市場上使用 Windows 8 或 Windows 8.1 的客戶都將無法使用您的 app。

所謂 Windows 8.x「世界其他地方」的市場包括：


<table>
  <tr>
    <td>阿富汗</td>
    <td>奧蘭島</td>
    <td>阿爾巴尼亞</td>
    <td>美屬薩摩亞</td>
  </tr>
  <tr>
    <td>安道爾</td>
    <td>安哥拉</td>
    <td>安圭拉</td>
    <td>南極大陸</td>
  </tr>
  <tr>
    <td>安地卡及巴布達</td>
    <td>亞美尼亞</td>
    <td>阿路巴</td>
    <td>亞塞拜然</td>
  </tr>
  <tr>
    <td>巴哈馬</td>
    <td>孟加拉</td>
    <td>巴貝多</td>
    <td>白俄羅斯</td>
  </tr>
  <tr>
    <td>貝里斯</td>
    <td>貝南</td>
    <td>百慕達</td>
    <td>不丹</td>
  </tr>
  <tr>
    <td>玻利維亞</td>
    <td>波奈</td>
    <td>波士尼亞與赫塞哥維納</td>
    <td>波札那</td>
  </tr>
  <tr>
    <td>布威島</td>
    <td>英屬印度洋領土</td>
    <td>英屬維爾京群島</td>
    <td>汶萊</td>
  </tr>
  <tr>
    <td>布吉納法索</td>
    <td>蒲隆地</td>
    <td>維德角</td>
    <td>柬埔寨</td>
  </tr>
  <tr>
    <td>喀麥隆</td>
    <td>開曼群島</td>
    <td>中非共和國</td>
    <td>查德</td>
  </tr>
  <tr>
    <td>聖誕島</td>
    <td>可可斯群島</td>
    <td>葛摩</td>
    <td>剛果共和國</td>
  </tr>
  <tr>
    <td>剛果民主共和國 (DRC)</td>
    <td>柯克群島</td>
    <td>科特迪瓦 (Côte d’Ivoire)</td>
    <td>古拉梳</td>
  </tr>
  <tr>
    <td>吉布地</td>
    <td>多米尼克</td>
    <td>多明尼加共和國</td>
    <td>厄瓜多</td>
  </tr>
  <tr>
    <td>薩爾瓦多</td>
    <td>赤道幾內亞</td>
    <td>厄利垂亞</td>
    <td>衣索比亞</td>
  </tr>
  <tr>
    <td>福克蘭群島</td>
    <td>法羅群島</td>
    <td>斐濟群島</td>
    <td>法屬圭亞那</td>
  </tr>
  <tr>
    <td>法屬玻里尼西亞</td>
    <td>法國南方領土及南極陸地</td>
    <td>加彭</td>
    <td>甘比亞</td>
  </tr>
  <tr>
    <td>喬治亞</td>
    <td>迦納</td>
    <td>直布羅陀</td>
    <td>格陵蘭</td>
  </tr>
  <tr>
    <td>格瑞那達</td>
    <td>哥德普洛</td>
    <td>關島</td>
    <td>瓜地馬拉</td>
  </tr>
  <tr>
    <td>根息</td>
    <td>幾內亞</td>
    <td>幾內亞比索</td>
    <td>蓋亞納</td>
  </tr>
  <tr>
    <td>海地</td>
    <td>赫德島及麥當勞群島</td>
    <td>宏都拉斯</td>
    <td>冰島</td>
  </tr>
  <tr>
    <td>曼城島</td>
    <td>牙買加</td>
    <td>澤西島</td>
    <td>肯亞</td>
  </tr>
  <tr>
    <td>吉里巴斯</td>
    <td>吉爾吉斯</td>
    <td>寮國</td>
    <td>賴索托</td>
  </tr>
  <tr>
    <td>賴比瑞亞</td>
    <td>列支敦斯登</td>
    <td>澳門特別行政區</td>
    <td>馬其頓 (FYRO)</td>
  </tr>
  <tr>
    <td>馬達加斯加</td>
    <td>馬拉威</td>
    <td>馬爾地夫</td>
    <td>馬利</td>
  </tr>
  <tr>
    <td>馬紹爾群島</td>
    <td>馬丁尼克</td>
    <td>茅利塔尼亞</td>
    <td>模里西斯</td>
  </tr>
  <tr>
    <td>馬約特島</td>
    <td>密克羅尼西亞</td>
    <td>摩爾多瓦</td>
    <td>摩納哥</td>
  </tr>
  <tr>
    <td>蒙古</td>
    <td>蒙特內哥羅</td>
    <td>蒙特色拉特島</td>
    <td>摩洛哥</td>
  </tr>
  <tr>
    <td>莫三比克</td>
    <td>緬甸</td>
    <td>納米比亞</td>
    <td>諾魯</td>
  </tr>
  <tr>
    <td>尼泊爾</td>
    <td>新喀里多尼亞群島</td>
    <td>尼加拉瓜</td>
    <td>尼日</td>
  </tr>
  <tr>
    <td>奈及利亞</td>
    <td>紐威島</td>
    <td>諾福克島</td>
    <td>北馬里安納群島</td>
  </tr>
  <tr>
    <td>帛琉</td>
    <td>巴勒斯坦民族權力機構</td>
    <td>巴拿馬</td>
    <td>巴布亞紐幾內亞</td>
  </tr>
  <tr>
    <td>巴拉圭</td>
    <td>皮特康群島</td>
    <td>留尼旺</td>
    <td>盧安達</td>
  </tr>
  <tr>
    <td>聖巴瑟米</td>
    <td>聖赫勒拿、亞森欣、特里斯坦達庫尼亞群島</td>
    <td>聖克里斯多福及尼維斯</td>
    <td>聖露西亞</td>
  </tr>
  <tr>
    <td>法屬聖馬丁</td>
    <td>聖匹島</td>
    <td>聖文森及格瑞那丁</td>
    <td>薩摩亞</td>
  </tr>
  <tr>
    <td>聖馬利諾</td>
    <td>聖多美普林西比</td>
    <td>塞內加爾</td>
    <td>塞席爾</td>
  </tr>
  <tr>
    <td>獅子山</td>
    <td>荷屬聖馬丁</td>
    <td>索羅門群島</td>
    <td>索馬利亞</td>
  </tr>
  <tr>
    <td>南喬治亞及南三明治群島</td>
    <td>蘇利南</td>
    <td>冷岸及央棉</td>
    <td>史瓦濟蘭</td>
  </tr>
  <tr>
    <td>塔吉克</td>
    <td>坦尚尼亞</td>
    <td>東帝汶</td>
    <td>多哥</td>
  </tr>
  <tr>
    <td>托克勞群島</td>
    <td>東加</td>
    <td>土庫曼</td>
    <td>土克斯及開科斯群島</td>
  </tr>
  <tr>
    <td>吐瓦魯</td>
    <td>烏干達</td>
    <td>美國外島</td>
    <td>美屬維爾京群島</td>
  </tr>
  <tr>
    <td>烏茲別克</td>
    <td>梵蒂岡</td>
    <td>委內瑞拉</td>
    <td>越南</td>
  </tr>
  <tr>
    <td>萬那杜</td>
    <td>瓦利斯及福杜納</td>
    <td>葉門</td>
    <td>尚比亞</td>
  </tr>
  <tr>
    <td>辛巴威</td>
    <td>&nbsp;</td>
    <td>&nbsp;</td>
    <td>&nbsp;</td>
  </tr>
</table>

> [!NOTE]
> 如需您可以註冊開發人員帳戶的國家和地區清單，請參閱[帳戶類型、位置和費用](account-types-locations-and-fees.md)。