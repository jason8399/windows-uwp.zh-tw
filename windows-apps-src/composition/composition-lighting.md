---
author: daneuber
title: 組合光源
description: 組合光源 Api 可用來將 3D 的動態光源新增到您的應用程式。
ms.author: jimwalk
ms.date: 07/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e634b18fffc4f601f6512d6ceeed51efbe9c1886
ms.sourcegitcommit: 7efffcc715a4be26f0cf7f7e249653d8c356319b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/30/2018
ms.locfileid: "3116137"
---
# <a name="using-lights-in-windows-ui"></a>Windows UI 中使用光線

Windows.UI.Composition Api 可讓您建立即時動畫及效果。 組合光源可讓 3D 光源在 2D 應用程式中。 在本概觀中，我們會逐步說明如何設定組合光源，找出視覺效果，以接收每個光線，並使用效果定義為您的內容的材質的功能。

> [!NOTE]
> 若要讀取[XamlLight](/uwp/api/windows.ui.xaml.media.xamllight)物件如何套用[CompositionLights](/uwp/api/Windows.UI.Composition.CompositionLight)照亮 XAML Uielement，請參閱[XAML 光源](xaml-lighting.md)。

組合光源可讓您建立有趣藉由允許的 UI:

- 其他物件來啟用身歷其境的案例，例如音樂播放場景場景中的光線獨立的轉換。
- 配對光線的物件，使其在一起移動的能力獨立的其餘部分來啟用案例，例如 Fluent[顯色](/design/style/reveal.md)醒目提示的場景。
- 光線和整個場景轉換為建立的材質和深度群組。

組合光源支援三個主要的概念： **Light**、**目標**及**SceneLightingEffect**。

## <a name="light"></a>光源

[CompositionLight](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlight)可讓您建立各種不同的光線，並將它們放在座標空間中。 這些燈號目標視覺效果，您想要找出因為透過光源亮起。

### <a name="light-types"></a>光源類型

| 類型 | 描述 |
| --- | --- |
| [AmbientLight](/uwp/api/windows.ui.composition.ambientlight) | 光源發出出現非方向光線反映在場景中的所有項目。 |
| [DistantLight](/uwp/api/windows.ui.composition.distantlight) | 無限大型遠距離的光源會發出的光線針對朝單一方向。 像是陽光照射。 |
| [PointLight](/uwp/api/windows.ui.composition.pointlight) | 點的來源會在所有方向的光線發出的光線。 例如燈泡。 |
| [焦點](/uwp/api/windows.ui.composition.spotlight) | 光源發出之光線內錐和外錐。 例如手電筒。 |

## <a name="targets"></a>目標

當光線目標視覺效果 （新增到[目標](/uwp/api/windows.ui.composition.compositionlight.targets)清單），「 視覺效果和其所有子系已注意到，這個光源回應。 這可以是項目設定動畫效果的點狀光源方向的根目錄中的樹狀結構，移和下所有的視覺效果的 PointLight 來源回應一樣簡單。

**ExclusionsFromTargets**可讓您以類似的方式中加入目標移除的視覺效果或視覺效果的樹狀子目錄的光源。 子系根視覺項目會排除由樹狀結構中的不是如此一來亮起。

### <a name="sample-targets"></a>範例 （目標）

在下列範例中，我們使用 CompositionPointLight 來以 XAML TextBlock。

```cs
    _pointLight = _compositor.CreatePointLight();
    _pointLight.Color = Colors.White;
    _pointLight.CoordinateSpace = text; //set up co-ordinate space for offset
    _pointLight.Targets.Add(text); //target XAML TextBlock
```

藉由將動畫新增到點狀光源的位移，輕鬆地達成閃爍的效果。

```cs
_pointLight.Offset = new Vector3(-(float)TextBlock.ActualWidth, (float)TextBlock.ActualHeight / 2, (float)TextBlock.FontSize);
```

請參閱 WindowUIDevLabs 範例強大如深入了解在完整[文字 Shimmer](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2014393/TextShimmer)範例。

## <a name="restrictions"></a>限制

有數個將會透過 CompositionLight 亮起判斷哪些內容時，要考慮的因素。

概念 | 詳細資料
--- | ---
**環境光線** | 將非周遭環境光線新增到您的場景，將會關閉所有現有的光線。  非周遭環境光線不目標的項目將會顯示成黑色。  若要照亮周圍的視覺效果不的目標對象透過光源的自然的方式，使用環境光線與其他光線搭配使用。
**光線的數目** | 您可以使用任何兩個組合非周遭環境光線的任何組合來讓您的 UI 目標。 周遭環境光線不會受到限制。特別，點和遠距離的光源。
**存留期** | CompositionLight 可能會遇到存留期條件 (範例： 記憶體回收行程可能會回收光線物件，才能使用)。  我們建議讓您燈光的參考應用程式.onactivated 光線為來協助管理存留期的應用程式的成員。
**轉換** | 光線必須放在使用您的視覺結構中的效果，例如[透視轉換](/design/layout/3-d-perspective-effects.md)，來繪製正確的 UI 上的節點。
**目標和座標空間** | CoordinateSpace 是光線屬性必須設定中，所有的視覺空間。 CompositionLight.Targets 必須是在 CoordinateSpace 樹狀目錄中。

## <a name="lighting-properties"></a>光源屬性

根據使用的光線類型，光線可有衰減和空間的屬性。 並非所有光線類型均使用所有的屬性。

屬性 | 描述
--- | ---
**色彩** | [色彩](/uwp/api/windows.ui.color)的光線。 光源值由[D3D](https://docs.microsoft.com/windows/uwp/graphics-concepts/light-properties) Diffuse、 Ambient，以及定義發出的色彩 Specular 定義的色彩。 光源使用的光線; 的 RGBA 值不會使用 alpha 色彩元件。
**Direction** | 光線的方向。 在其中光線指的方向被指定相對於其[CoordinateSpace](/uwp/api/windows.ui.composition.distantlight.coordinatespace)視覺效果。
**座標空間** | 每個視覺效果具有隱含的 3D 座標空間。 X 方向是從左到右。 Y 方向是從上到下。 Z 方向是從平面的點。 此座標的原始點視覺效果的左上角，且的單位是裝置獨立像素 (DIP)。 在這個座標中定義的位移。
**內錐和外錐體** | 聚光燈發出的光線錐體分兩部分︰明亮的內錐和外錐體。 組合可讓您可以控制內錐和外錐角度和色彩。
**Offset** | 光源相對於其座標空間視覺效果的位移。

> [!NOTE]
> 當多個光線叫用的相同的視覺效果，或當夠大，超過 1.0 光線的色彩值時，可能會因為鉗制光線色彩通道的變更光線的色彩。

### <a name="advanced-lighting-properties"></a>進階光源屬性

屬性 | 描述
--- | ---
**強度** | 控制光線的亮度。
**衰減** | 衰減控制光線強度如何隨著範圍屬性指定的最大距離減少。  常數，可以使用 Quadradic 和線性衰減屬性。

## <a name="getting-started-with-lighting"></a>開始使用光源

請依照下列新增光線這些一般步驟：

- 建立並將放置光源： 建立光源，並將它們放在指定的座標空間中。
- 識別為 [淺色物件： 目標在相關的視覺效果的光線。
- [選用]定義如何個別物件回應光線： 使用 SceneLightingEffect 搭配自訂顯示 SpriteVisual 光線反映 EffectBrush。 反映的預設值支援的光線 CoordinateSpace 的子系的光源。  使用 SceneLightingEffect 繪製視覺效果會覆寫該視覺效果的預設光源。

## <a name="scenelightingeffect"></a>SceneLightingEffect

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect)會用來修改預設光源套用至[SpriteVisual](/uwp/api/Windows.UI.Composition.SpriteVisual) [CompositionLight](/uwp/api/windows.ui.composition.compositionlight)目標的內容。

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect)經常用於材質建立。 SceneLightingEffect 是當您想要達到更複雜，例如啟用反映在影像上的屬性和/或目的深度錯覺效果提供一般的對應項目時，使用效果。 SceneLightingEffect 能夠藉由使用光線屬性，例如反射和擴散大量來自訂您的 UI。 您可以進一步自訂使用效果管線可讓您個別混合和撰寫您的內容使用不同的光源反應的其餘部分的光線效果。

> [!NOTE]
> 場景光源不會產生陰影。它會將焦點放在 2D 轉譯效果。  它不會納入考量 3D 光源案例，包括真實的光源模型，包括陰影。


屬性 | 描述
--- | ---
**一般的地圖** | NormalMaps 建立的紋理光線一般指向將會是更亮，一般指向立即播放將會較暗的效果。 若要新增您特定對象的視覺 NormalMap 使用載入 NormalMap 資產使用 LoadedImageSurface [CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) 。
**周遭環境** | 周遭環境的屬性大部分用來控制整體的色彩反映。
**反射光源** | 鏡面反射的物件，使其看起來光澤建立反白顯示。 您可以控制鏡面反射的層級以及發亮的層級。  這些屬性被操作來建立像 shinny 金屬或光面紙張材質效果。
**擴散** | Diffused 的反映 scatters 中所有方向的光線。
**反射率模型** | [反射率模型](/uwp/api/windows.ui.composition.effects.scenelightingeffectreflectancemodel)可讓您[Blinn 鏡面](https://docs.microsoft.com/visualstudio/designers/how-to-create-a-basic-phong-shader)和實際基礎 Blinn 鏡面之間進行選擇。  當您想要有壓縮反射強光光線時，您會選擇實際基礎 Blinn 鏡面。

### <a name="sample-scenelightingeffect"></a>範例 (SceneLightingEffect)

下列範例示範如何將一般對應新增至 SceneLightingEffect。

```cs
CompositionBrush CreateNormalMapBrush(ICompositionSurface normalMapImage)
{
    var colorSourceEffect = new ColorSourceEffect()
    {
        Color = Colors.White
    };
    var sceneLightingEffect = new SceneLightingEffect()
    {
        NormalMapSource = new CompositionEffectSourceParameter("NormalMap")
    };

    var compositeEffect = new ArithmeticCompositeEffect()
    {
        Source1 = colorSourceEffect,
        Source2 = sceneLightingEffect,
    };

    var factory = _compositor.CreateEffectFactory(sceneLightingEffect);

    var normalMapBrush = _compositor.CreateSurfaceBrush();
    normalMapBrush.Surface = normalMapImage;
    normalMapBrush.Stretch = CompositionStretch.Fill;

    var brush = factory.CreateBrush();
    brush.SetSourceParameter("NormalMap", normalMapBrush);

    return brush;
}
```

## <a name="related-articles"></a>相關文章

- [建立材質和視覺層中的燈號](https://blogs.windows.com/buildingapps/2017/08/04/creating-materials-lights-visual-layer/)
- [光源概觀](https://docs.microsoft.com/windows/uwp/graphics-concepts/lighting-overview)
- [CompositionCapabilities API](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositioncapabilities)
- [光源光源的數學計算](https://docs.microsoft.com/windows/uwp/graphics-concepts/mathematics-of-lighting)
- [SceneLightingEffect](https://docs.microsoft.com/uwp/api/windows.ui.composition.effects.scenelightingeffect)
- [WindowsUIDevLabs GitHub 存放庫](https://github.com/Microsoft/WindowsUIDevLabs)