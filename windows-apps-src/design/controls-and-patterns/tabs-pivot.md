---
author: serenaz
Description: Tabs and pivots enable users to navigate between frequently accessed content.
title: 索引標籤與樞紐
ms.assetid: 556BC70D-CF5D-4295-A655-D58163CC1824
label: Tabs and pivots
template: detail.hbs
ms.author: sezhen
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 09e88fd070f7e7517e55d6f57563dd7608696770
ms.sourcegitcommit: 2470c6596d67e1f5ca26b44fad56a2f89773e9cc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
ms.locfileid: "1675475"
---
# <a name="pivot-and-tabs"></a>樞紐和索引標籤



Pivot 控制項和相關的索引標籤模式可用於瀏覽經常存取、不同的內容類別。 樞紐可允許在兩個或更多個內容窗格之間進行瀏覽，並依賴文字標頭來標註不同區段的內容。

> **重要 API**：[Pivot 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)

![樞紐控制項的範例](images/pivot_Hero_main.png)

索引標籤是樞紐的視覺變體，索引標籤會使用圖示和文字的組合或僅使用圖示來表達區段內容。 建置索引標籤時是使用 [Pivot](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx) 控制項來建置。 [樞紐範例](http://go.microsoft.com/fwlink/p/?LinkId=619903)示範如何將 Pivot 控制項自訂成索引標籤模式。

![索引標籤模式的範例](images/tabs.png) 

## <a name="the-pivot-control"></a>樞紐控制項

建置含有樞紐的應用程式時，有一些需要考量的主要設計變數。

- **標頭標籤。**  標頭可以有包含文字的圖示、只有圖示或只有文字。
- **標頭對齊。**  標頭可以靠左對齊或置中。
- **最上層或子層瀏覽。**  樞紐可以用於這兩種層級瀏覽中的任一種。 您也可以選擇將[瀏覽檢視](navigationview.md)做為主要層級，而將樞紐做為次要層級。
- **觸控手勢支援。**  對於支援觸控手勢的裝置，您可以使用兩個互動集合的其中一個在內容類別之間瀏覽：
    1. 點選索引標籤/樞紐標頭以瀏覽至該類別。
    2. 在內容區域上向左或向右撥動以瀏覽至相鄰的類別。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/Pivot">開啟應用程式並查看 Pivot 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

手機上的 Pivot 控制項

![樞紐範例](images/pivot_example.png)

「鬧鐘與時鐘」應用程式中的索引標籤模式。

![「鬧鐘與時鐘」中的索引標籤模式範例](images/tabs_alarms-and-clock.png)

## <a name="create-a-pivot-control"></a>建立 Pivot 控制項

[Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) 控制項內建了這一節中所述的基本功能。

這段 XAML 會建立包含 3 個內容區段的基本 Pivot 控制項。

```xaml
<Pivot x:Name="rootPivot" Title="Pivot Title">
    <PivotItem Header="Pivot Item 1">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 1."/>
    </PivotItem>
    <PivotItem Header="Pivot Item 2">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 2."/>
    </PivotItem>
    <PivotItem Header="Pivot Item 3">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 3."/>
    </PivotItem>
</Pivot>
```

### <a name="pivot-items"></a>Pivot 項目

Pivot 是一種 [ItemsControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx)，因此可以包含任何型別的項目集合。 所有新增到 Pivot 且不是明確為 [PivotItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivotitem.aspx) 的項目，都會隱含地包裝在 PivotItem 中。 因為 Pivot 通常是用來在頁面內容之間瀏覽，所以通常會直接使用 XAML UI 元素填入 [Items](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.items.aspx) 集合。 或者，您可以將 [ItemsSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) 屬性設為資料來源。 ItemsSource 中繫結的項目可以是任何型別，但如果它們不是明確為 PivotItems，則您必須定義 [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) 和 [HeaderTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.headertemplate.aspx) 來指定如何顯示這些項目。

您可以使用 [SelectedItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selecteditem.aspx) 屬性擷取或設定選取的項目。 使用 [SelectedIndex](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selectedindex.aspx) 屬性擷取或設定選取的項目。

### <a name="pivot-headers"></a>樞紐標頭

您可以使用 [LeftHeader](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.leftheader.aspx) 和 [RightHeader](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.rightheader.aspx) 屬性來將其他控制項新增到樞紐標頭。

例如，您可以在樞紐 RightHeader 中新增 [CommandBar](https://docs.microsoft.com/en-us/windows/uwp/controls-and-patterns/app-bars)。

![樞紐控制項右標頭中的命令列範例](images/PivotHeader.png)

```xaml
<Pivot>
    <Pivot.RightHeader>
        <CommandBar OverflowButtonVisibility="Collapsed" Background="Transparent">
                <AppBarButton Icon="Add"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Edit"/>
                <AppBarButton Icon="Delete"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Save"/>
        </CommandBar>
    </Pivot.RightHeader>
</Pivot>
```

### <a name="pivot-interaction"></a>樞紐互動

以下是該控制項主要的觸控手勢互動：

-   點選樞紐項目標頭可瀏覽到該標頭的區段內容。
-   在樞紐項目標頭上向左或向右撥動可瀏覽到相鄰的區段。
-   在區段內容上向左或向右撥動可瀏覽到相鄰的區段。

![在區段內容上向左撥動的範例](images/pivot_w_hand.png)

此控制項有兩種模式：

**靜止**

-   當所有的樞紐標頭大小符合允許的空間時，樞紐會靜止。
-   雖然樞紐本身不會移動，但點選樞紐標籤會瀏覽到對應的頁面。 使用中的樞紐會反白顯示。

**浮動切換**

-   當所有的樞紐標頭大小都不符合允許的空間時，樞紐會浮動切換。
-   點選樞紐標籤會瀏覽到對應的頁面，且使用中的樞紐標籤會浮動切換到第一個位置。
-   各樞紐項目會浮動循環切換，從最後一個接到第一個樞紐區段。

> **注意** 樞紐標頭不可在 [10 英呎環境](../devices/designing-for-tv.md)中浮動切換。 如果 App 會在 Xbox 上執行，請將 [IsHeaderItemsCarouselEnabled](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot.IsHeaderItemsCarouselEnabled) 屬性設為 **false**。

### <a name="pivot-focus"></a>樞紐焦點

根據預設，樞紐標頭上的鍵盤焦點會以底線呈現。

![預設焦點底線選取的標頭](images/pivot_focus_selectedHeader.png)

具有自訂樞紐且將底線併入其標頭選取項目視覺效果的 App 可以使用 [HeaderFocusVisualPlacement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.HeaderFocusVisualPlacement) 屬性變更預設。 在 `HeaderFocusVisualPlacement="ItemHeaders"` 時，會在整個標頭面板周圍繪製焦點。

![ItemsHeader 選項會在所有樞紐標頭周遭繪製焦點矩形](images/pivot_focus_headers.png)

## <a name="recommendations"></a>建議

- 根據螢幕大小對齊索引標籤/樞紐標頭。 對於寬度小於 720 epx 的螢幕，置中對齊通常比較好，而大多數情況是寬度大於 720 epx 的螢幕建議使用靠左對齊。
- 當使用浮動切換 (反覆) 模式時請避免使用超過 5 個標頭，因為循環超過 5 個標頭可能會混淆使用者。
- 當您的樞紐項目有個別的圖示時才使用索引標籤模式。
- 在樞紐項目標頭包含文字，以幫助使用者了解每個樞紐區段的意義。 圖示並非對於所有使用者都一目了然。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [樞紐範例](http://go.microsoft.com/fwlink/p/?LinkId=619903)：示範如何將 Pivot 控制項自訂成索引標籤模式。
- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)：以互動式格式查看所有 XAML 控制項。

## <a name="related-topics"></a>相關主題

- [Pivot 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)
- [瀏覽設計基本知識](../basics/navigation-basics.md)