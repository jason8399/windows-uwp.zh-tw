---
title: 在 Marble Maze 範例中加入音訊
description: 本文件說明使用音訊時需要考量的重要做法，並指出 Marble Maze 如何運用這些做法。
ms.assetid: 77c23d0a-af6d-17b5-d69e-51d9885b0d44
ms.date: 10/18/2017
ms.topic: article
keywords: Windows 10, UWP, 音訊, 遊戲, 範例
ms.localizationpriority: medium
ms.openlocfilehash: 4cf803b61a280ff3ed803dc7972d7f7eb6993a64
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258552"
---
# <a name="adding-audio-to-the-marble-maze-sample"></a>在 Marble Maze 範例中加入音訊

本文件說明使用音訊時需要考量的重要做法，並指出 Marble Maze 如何運用這些做法。 Marble Maze 使用 [Microsoft 媒體基礎](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk)從檔案載入音訊資源，並使用 [XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-apis-portal) 來混合和播放音訊，以及將效果套用至音訊。

Marble Maze 會播放背景音樂，也會使用遊戲音效來表示遊戲事件，例如當彈珠碰到圍牆時。 實作的一個重點是Marble Maze 使用殘響或回音效果來模擬彈珠彈跳的音效。 殘響效果實作會讓回音在小房間裡更快到達聽者的位置、更大聲，房間較大時則會比較小聲，且較慢到達聽者的位置。

> [!NOTE]
> 與本文件對應的範例程式碼可以在 [DirectX Marble Maze 遊戲範例](https://github.com/microsoft/Windows-appsample-marble-maze)中找到。

以下是本文件所討論在遊戲中使用音訊時的一些重點：

- 考慮使用媒體基礎來解碼音訊資產，以及使用 XAudio2 來播放音效。 不過，如果您已經有適用於通用 Windows 平台 (UWP) App 的音訊資產載入機制，則可直接使用。

- 音訊圖包含每個作用中音效的一個來源音、零或多個副混音，以及一個主控音。 來源音可以送入副混音和/或主控音。 副混音可以送入其他副混音或主控音。

- 如果您的背景音樂檔很大，請考慮將音樂串流處理到較小的緩衝區，以減少記憶體用量。

- 如果合適的話，當應用程式失去焦點或看不見或暫停執行時，應暫停音訊播放。 等到應用程式重新取得焦點、變成可見或繼續執行時，就繼續播放。

- 設定音訊分類以反映每個音效的角色。 例如，您通常會使用**AudioCategory\_GameMedia**做為遊戲背景音訊和**AudioCategory\_GameEffects**以取得音效效果。

- 透過釋放和重建所有音訊資源和介面，以處理裝置變更，包括耳機。

- 當需要將磁碟空間最小化和串流處理成本降到最低時，應考慮是否壓縮音訊檔。 或者，您也可以維持不壓縮音訊，以加速載入音訊。

## <a name="introducing-xaudio2-and-microsoft-media-foundation"></a>介紹 XAudio2 和 Microsoft 媒體基礎

XAudio2 是專門支援遊戲音訊的 Windows 低階音訊程式庫。 這個程式庫提供遊戲的數位訊號處理 (DSP) 和音訊圖引擎。 XAudio2 支援運算趨勢，例如 SIMD 浮點架構和 HD 音訊，所以能超越其之前的產品 (DirectSound 和 XAudio)。 它也支援現在的遊戲中更複雜的音效處理需求。

[XAudio2 主要概念](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-key-concepts)文件說明使用 XAudio2 的主要概念。 簡單來說，這些概念包括：

- [IXAudio2](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) 介面是 XAudio2 引擎的核心。 Marble Maze 使用此介面來建立音效，以及在輸出裝置變更或故障時接收通知。

- **聲音**會處理、調整及播放音訊資料。

- **來源音**是音訊頻道的集合 (單聲道、5.1 等)，代表一個音訊資料流。 在 XAudio2 中，來源音是音訊處理的起點。 一般而言，會從外部來源 (例如檔案或網路) 載入音效資料，然後傳送至來源音。 Marble Maze 使用[媒體基礎](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk)從檔案載入音效資料。 本文件稍後會介紹媒體基礎。

- **副混音**會處理音訊資料。 此處理可能包括變更音訊資料流，或將多個資料流合而為一。 Marble Maze 使用副混音來建立殘響效果。

- **主控音**會合併來源音和副混音的資料，然後將該資料傳送至音訊硬體。

- **音訊圖**包含每個主動音效的一個來源音、零或多個副混音，以及唯一的一個主控音。

- **回呼**會告知用戶端程式碼，音效或引擎物件發生某些事件。 「回呼」可讓您在 XAudio2 結束使用緩衝區後重複使用記憶體、當音訊裝置變更時做出反應 (例如，插上耳機或拔除耳機時) 等等。 本文件稍後的[處理耳機和裝置變更](#handling-headphones-and-device-changes)會說明 Marble Maze 如何使用此機制來處理裝置變更。

Marble Maze 使用兩個音訊引擎 (亦即兩個 [IXAudio2](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) 物件) 來處理音訊。 一個引擎處理背景音樂，另一個引擎處理遊戲音效。

Marble Maze 也必須為每個引擎建立一個主控音。 前面說過，主引擎會將音訊資料流合併為一個資料流，然後將該資料流傳送至音訊硬體。 背景音樂資料流 (來源音) 會將資料輸出至一個主控音和兩個副混音。 副混音則會表現殘響效果。

「媒體基礎」是支援許多音訊和視訊格式的多媒體程式庫。 XAudio2 和媒體基礎相輔相成。 Marble Maze 會使用媒體基礎從檔案載入音訊資產，並使用 XAudio2 播放音訊。 您不一定要使用媒體基礎來載入音訊資產。 如果您已經有適用於通用 Windows 平台 (UWP) App 的音訊資產載入機制，就可以使用它。 [音訊、視訊和相機](../audio-video-camera/index.md)討論實作 UWP app 中的音訊的幾個方法。

如需 XAudio2 的詳細資訊，請參閱[程式設計指南](https://docs.microsoft.com/windows/desktop/xaudio2/programming-guide)。 如需媒體基礎的詳細資訊，請參閱 [Microsoft 媒體基礎](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk)。

## <a name="initializing-audio-resources"></a>初始化音訊資源

Marble Maze 使用 Windows Media 音訊 (.wma) 檔案儲存背景音樂，而使用 WAV (.wav) 檔案儲存遊戲音效。 媒體基礎支援這些格式。 雖然 XAudio2 原本就支援 .wav 檔案格式，但遊戲必須手動分析檔案格式，以填入適當的 XAudio2 資料結構。 Marble Maze 使用媒體基礎來更輕鬆地使用 .wav 檔案。 如需媒體基礎支援的完整媒體格式清單，請參閱[媒體基礎中支援的媒體格式](https://docs.microsoft.com/windows/desktop/medfound/supported-media-formats-in-media-foundation)。 Marble Maze 既不使用個別的設計階段和執行階段音訊格式，也不使用 XAudio2 ADPCM 壓縮支援。 如需 XAudio2 中 ADPCM 壓縮的詳細資訊，請參閱 [ADPCM 概觀](https://docs.microsoft.com/windows/desktop/xaudio2/adpcm-overview)。

從 **MarbleMazeMain::LoadDeferredResources** 呼叫的 **Audio::CreateResources** 方法會從檔案載入音訊資料流、初始化 XAudio2 引擎物件，以及建立來源音、副混音和主控音。

### <a name="creating-the-xaudio2-engines"></a>建立 XAudio2 引擎

前面說過，Marble Maze 會建立一個 [IXAudio2](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) 物件來代表它所使用的每個音訊引擎。 若要建立音訊引擎，請呼叫 [XAudio2Create](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create) 方法。 下列範例顯示 Marble Maze 如何建立處理背景音樂的音訊引擎。

```cpp
// In Audio.h
class Audio
{
private:
    IXAudio2*                   m_musicEngine;
// ...
}

// In Audio.cpp
void Audio::CreateResources()
{
    try
    {
        // ...
        DX::ThrowIfFailed(
            XAudio2Create(&m_musicEngine)
            );
        // ...
    }
    // ...
}
```

Marble Maze 會執行類似的步驟，建立可用來播放遊戲音效的音訊引擎。

在 UWP app 中使用 [IXAudio2](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) 介面的方式，相較於在傳統型應用程式中有兩點不同。 首先，在呼叫 [XAudio2Create](https://docs.microsoft.com/windows/desktop/api/combaseapi/nf-combaseapi-coinitializeex) 之前，不需要呼叫 [CoInitializeEx](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create)。 此外，**IXAudio2** 不再支援裝置列舉。 如需如何列舉音訊裝置的相關資訊，請參閱[列舉裝置](https://docs.microsoft.com/previous-versions/windows/apps/hh464977(v=win.10))。

### <a name="creating-the-mastering-voices"></a>建立主控音

下列範例顯示 **Audio::CreateResources** 方法如何使用 [IXAudio2::CreateMasteringVoice](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createmasteringvoice) 方法建立背景音樂的主控音。 在此範例中， **m\_musicMasteringVoice**是[IXAudio2MasteringVoice](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2masteringvoice)物件。 我們指定兩個輸入通道；這可簡化殘響效果的邏輯。 

我們指定 48000 做為輸入的取樣率。 我們選擇此取樣率是因為它可在音訊品質與所需的 CPU 處理之間取得平衡。 更高的取樣率需要更多的 CPU 處理，但品質不會明顯提升。 

最後，我們指定 **AudioCategory_GameMedia** 做為音訊串流類別，以便使用者可以一邊玩遊戲，一邊聆聽來自不同應用程式的音樂。 播放音樂應用程式時，Windows 會 mutes **AudioCategory\_GameMedia**選項所建立的任何聲音。 使用者仍然會聽到遊戲音效，因為它們是由**AudioCategory\_GameEffects**選項所建立。 如需有關音訊類別的詳細資訊，請參閱[音訊\_串流\_類別目錄](https://docs.microsoft.com/windows/desktop/api/audiosessiontypes/ne-audiosessiontypes-_audio_stream_category)。

```cpp
// This sample plays the equivalent of background music, which we tag on the  
// mastering voice as AudioCategory_GameMedia. In ordinary usage, if we were  
// playing the music track with no effects, we could route it entirely through 
// Media Foundation. Here, we are using XAudio2 to apply a reverb effect to the 
// music, so we use Media Foundation to decode the data then we feed it through 
// the XAudio2 pipeline as a separate Mastering Voice, so that we can tag it 
// as Game Media. We default the mastering voice to 2 channels to simplify  
// the reverb logic.
DX::ThrowIfFailed(
    m_musicEngine->CreateMasteringVoice(
        &m_musicMasteringVoice,
        2,
        48000,
        0,
        nullptr,
        nullptr,
        AudioCategory_GameMedia
        )
);
```

**音訊：：若是**方法會執行類似的步驟來建立遊戲音效的「主控語音」，不同之處在于它會針對*StreamCategory*參數指定**AudioCategory\_GameEffects** ，這是預設值。

### <a name="creating-the-reverb-effect"></a>建立殘響效果

對於每個音效，您可以使用 XAudio2 建立一序列的效果來處理音訊。 這類序列稱為「效果鏈」。 當您想要將一或多個效果套用至音效時，請使用效果鏈。 效果鏈可能有破壞性，也就是鏈結中的每個效果都可以覆寫音訊緩衝區。 這個屬性很重要，因為 XAudio2 不保證會以無聲方式初始化輸出緩衝區。 效果物件在 XAudio2 中是以跨平台音訊處理物件 (XAPO) 來代表。 如需 XAPO 的詳細資訊，請參閱 [XAPO 概觀](https://docs.microsoft.com/windows/desktop/xaudio2/xapo-overview)。

當您建立效果鏈時，請依照下列步驟執行：

1. 建立效果物件。

2. 在[XAUDIO2\_效果](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_descriptor)中填入效果資料\_描述元結構。

3. 在[XAUDIO2\_效果](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_chain)中填入資料，\_鏈結構。

4. 將效果鏈套用至音效。

5. 填入效果參數結構，並將其套用至效果。

6. 適時地停用或啟用效果。

**Audio** 類別會定義 **CreateReverb** 方法，以建立實作殘響的效果鏈。 這個方法會呼叫 [XAudio2CreateReverb](https://docs.microsoft.com/windows/desktop/api/xaudio2fx/nf-xaudio2fx-xaudio2createreverb) 方法來建立 **ComPtr&lt;IUnknown&gt;** 物件 **soundEffectXAPO**，做為殘響效果的副混音。

```cpp
Microsoft::WRL::ComPtr<IUnknown> soundEffectXAPO;

DX::ThrowIfFailed(
    XAudio2CreateReverb(&soundEffectXAPO)
    );
```

[XAUDIO2\_效果\_描述](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_descriptor)元結構包含在效果鏈中使用之 XAPO 的相關資訊，例如輸出通道的目標數目。 **音訊：： CreateReverb**方法會建立**XAUDIO2\_效果，\_描述**元物件**soundEffectdescriptor**、設定為停用狀態、使用兩個輸出通道，以及**soundEffectXAPO**用於回音效果的參考。 **soundEffectdescriptor** 最初為停用狀態，因為在效果開始修改遊戲音效之前，遊戲必須先設定參數。 Marble Maze 使用兩個輸出通道來簡化殘響效果的邏輯。

```cpp
soundEffectdescriptor.InitialState = false;
soundEffectdescriptor.OutputChannels = 2;
soundEffectdescriptor.pEffect = soundEffectXAPO.Get();
```

如果效果鏈有多個效果，則每個效果都需要一個物件。 [XAUDIO2\_效果\_鏈](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_chain)結構會保存 XAUDIO2 的陣列， [\_效果會\_影響](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_descriptor)參與效果的描述元物件。 下列範例顯示 **Audio::CreateReverb** 方法如何指定一個效果來實作殘響。

```cpp
XAUDIO2_EFFECT_CHAIN soundEffectChain;

// ...

soundEffectChain.EffectCount = 1;
soundEffectChain.pEffectDescriptors = &soundEffectdescriptor;
```

**Audio::CreateReverb** 方法會呼叫 [IXAudio2::CreateSubmixVoice](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createsubmixvoice) 方法來建立效果的副混音。 它會針對*pEffectChain*參數指定[XAUDIO2\_效果\_鏈](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_chain)物件**soundEffectChain**，以將效果鏈與語音產生關聯。 Marble Maze 還會指定兩個輸出通道和 48 千赫的取樣率。

```cpp
DX::ThrowIfFailed(
    engine->CreateSubmixVoice(newSubmix, 2, 48000, 0, 0, nullptr, &soundEffectChain)
    );
```

> [!TIP]
> 祕訣：如果您想要將現有的效果鏈附加至現有的副混音，或想要取代目前的效果鏈，請使用 [IXAudio2Voice::SetEffectChain](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-seteffectchain) 方法。

**Audio::CreateReverb** 方法會呼叫 [IXAudio2Voice::SetEffectParameters](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-seteffectparameters) 來設定與效果相關聯的其他參數。 這個方法使用效果特有的參數結構。 [XAUDIO2FX\_的「回音」\_PARAMETERS](https://docs.microsoft.com/windows/desktop/api/xaudio2fx/ns-xaudio2fx-xaudio2fx_reverb_parameters)物件**m_reverbParametersSmall**（其中包含「回音」的效果參數）會在「**音訊：： Initialize** 」方法中初始化，因為每個「回音」效果都會共用相同的參數。 下列範例顯示 **Audio::Initialize** 方法如何初始化近距離殘響的殘響參數。

```cpp
m_reverbParametersSmall.ReflectionsDelay = XAUDIO2FX_REVERB_DEFAULT_REFLECTIONS_DELAY;
m_reverbParametersSmall.ReverbDelay = XAUDIO2FX_REVERB_DEFAULT_REVERB_DELAY;
m_reverbParametersSmall.RearDelay = XAUDIO2FX_REVERB_DEFAULT_REAR_DELAY;
m_reverbParametersSmall.PositionLeft = XAUDIO2FX_REVERB_DEFAULT_POSITION;
m_reverbParametersSmall.PositionRight = XAUDIO2FX_REVERB_DEFAULT_POSITION;
m_reverbParametersSmall.PositionMatrixLeft = XAUDIO2FX_REVERB_DEFAULT_POSITION_MATRIX;
m_reverbParametersSmall.PositionMatrixRight = XAUDIO2FX_REVERB_DEFAULT_POSITION_MATRIX;
m_reverbParametersSmall.EarlyDiffusion = 4;
m_reverbParametersSmall.LateDiffusion = 15;
m_reverbParametersSmall.LowEQGain = XAUDIO2FX_REVERB_DEFAULT_LOW_EQ_GAIN;
m_reverbParametersSmall.LowEQCutoff = XAUDIO2FX_REVERB_DEFAULT_LOW_EQ_CUTOFF;
m_reverbParametersSmall.HighEQGain = XAUDIO2FX_REVERB_DEFAULT_HIGH_EQ_GAIN;
m_reverbParametersSmall.HighEQCutoff = XAUDIO2FX_REVERB_DEFAULT_HIGH_EQ_CUTOFF;
m_reverbParametersSmall.RoomFilterFreq = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_FREQ;
m_reverbParametersSmall.RoomFilterMain = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_MAIN;
m_reverbParametersSmall.RoomFilterHF = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_HF;
m_reverbParametersSmall.ReflectionsGain = XAUDIO2FX_REVERB_DEFAULT_REFLECTIONS_GAIN;
m_reverbParametersSmall.ReverbGain = XAUDIO2FX_REVERB_DEFAULT_REVERB_GAIN;
m_reverbParametersSmall.DecayTime = XAUDIO2FX_REVERB_DEFAULT_DECAY_TIME;
m_reverbParametersSmall.Density = XAUDIO2FX_REVERB_DEFAULT_DENSITY;
m_reverbParametersSmall.RoomSize = XAUDIO2FX_REVERB_DEFAULT_ROOM_SIZE;
m_reverbParametersSmall.WetDryMix = XAUDIO2FX_REVERB_DEFAULT_WET_DRY_MIX;
m_reverbParametersSmall.DisableLateField = TRUE;
```

這個範例在大部分殘響參數中使用預設值，不過，它將 **DisableLateField** 設為 TRUE 來指定近距離殘響、將 **EarlyDiffusion** 設為 4 來模擬平坦近距離平面、將 **LateDiffusion** 設為 15 來模擬極度擴散遠距離平面。 平坦近距離平面會使回音傳遞得更快、更大聲，而擴散遠距離平面則會使回音變小聲、傳遞得較慢。 您可以實驗殘響值來取得適合遊戲的理想效果，或使用 **ReverbConvertI3DL2ToNative** 方法來採用業界標準的 I3DL2 (Interactive 3D Audio Rendering Guidelines Level 2.0) 參數。

以下範例顯示 **Audio::CreateReverb** 如何設定殘響參數。 **newSubmix** 是 [IXAudio2SubmixVoice](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2submixvoice)** 物件。 **參數**是[XAUDIO2FX 的\_回音\_parameters](https://docs.microsoft.com/windows/desktop/api/xaudio2fx/ns-xaudio2fx-xaudio2fx_reverb_parameters)* 物件。

```cpp
DX::ThrowIfFailed(
    (*newSubmix)->SetEffectParameters(0, parameters, sizeof(m_reverbParametersSmall))
    );
```

透過使用 **IXAudio2Voice::EnableEffect** 啟用效果 (如果設定 [enableEffect](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-enableeffect) 旗標)，來完成 **Audio::CreateReverb** 方法。 它也會使用 [IXAudio2Voice::SetVolume](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-setvolume) 設定其磁碟區，以及使用[IXAudio2Voice::SetOutputMatrix](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-setoutputmatrix) 輸出矩陣。 這個部分會將音量設為最大 (1.0)，並將左右輸入和左右輸出喇叭的音量矩陣指定為靜音。 我們這樣做是因為其他程式碼稍後會在兩個殘響之間淡入與淡出 (模擬從靠近牆面變成身處於較大的空間裡)，或必要時將兩個殘響變成靜音。 之後，當殘響路徑解除靜音時，遊戲就會設定 {1.0f、0.0f、0.0f、1.0f} 的矩陣，將左殘響輸出傳送至主控音的左輸入，而將右殘響輸出傳送至主控音的右輸入。

```cpp
if (enableEffect)
{
    DX::ThrowIfFailed(
        (*newSubmix)->EnableEffect(0)
        );    
}

DX::ThrowIfFailed(
    (*newSubmix)->SetVolume (1.0f)
    );

float outputMatrix[4] = {0, 0, 0, 0};
DX::ThrowIfFailed(
    (*newSubmix)->SetOutputMatrix(masteringVoice, 2, 2, outputMatrix)
    );
```

Marble Maze 會呼叫 **Audio::CreateReverb** 方法四次：其中兩次用於播放背景音樂，另外兩次用於播放遊戲音效。 下列內容會顯示 Marble Maze 如何呼叫 **CreateReverb** 方法來播放背景音樂。

```cpp
CreateReverb(
    m_musicEngine, 
    m_musicMasteringVoice, 
    &m_reverbParametersSmall, 
    &m_musicReverbVoiceSmallRoom, 
    true
    );
CreateReverb(
    m_musicEngine, 
    m_musicMasteringVoice, 
    &m_reverbParametersLarge, 
    &m_musicReverbVoiceLargeRoom, 
    true
    );
```

如需 XAudio2 適用效果來源的清單，請參閱 [XAudio2 音訊效果](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-audio-effects)。

### <a name="loading-audio-data-from-file"></a>從檔案載入音訊資料

Marble Maze 會定義 **MediaStreamer** 類別，以使用媒體基礎從檔案載入音訊資源。 Marble Maze 使用一個 **MediaStreamer** 物件來載入每個音訊檔。

Marble Maze 會呼叫 **MediaStreamer::Initialize** 方法來初始化每個音訊資料流。 以下內容顯示 **Audio::CreateResources** 方法如何呼叫 **MediaStreamer::Initialize** 來初始化背景音樂的音訊資料流：

```cpp
// Media Foundation is a convenient way to get both file I/O and format decode for 
// audio assets. You can replace the streamer in this sample with your own file I/O 
// and decode routines.
m_musicStreamer.Initialize(L"Media\\Audio\\background.wma");
```

**MediaStreamer::Initialize** 方法首先會呼叫 [MFStartup](https://docs.microsoft.com/windows/desktop/api/mfapi/nf-mfapi-mfstartup) 方法來初始化媒體基礎。 **MF_VERSION** 是在**mfapi.h** 中定義的巨集，並且是我們指定為要使用的媒體基礎版本。

```cpp
DX::ThrowIfFailed(
    MFStartup(MF_VERSION)
    );
```

**MediaStreamer::Initialize** 接著會呼叫 [MFCreateSourceReaderFromURL](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-mfcreatesourcereaderfromurl) 來建立 [IMFSourceReader](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nn-mfreadwrite-imfsourcereader) 物件。 **IMFSourceReader** 物件 **m_reader** 會從 **url** 指定的檔案中讀取媒體資料。

```cpp
DX::ThrowIfFailed(
    MFCreateSourceReaderFromURL(url, nullptr, &m_reader)
    );
```

**MediaStreamer::Initialize** 方法接著使用 [MFCreateMediaType](https://docs.microsoft.com/windows/desktop/api/mfobjects/nn-mfobjects-imfmediatype) 建立 [IMFMediaType](https://docs.microsoft.com/windows/desktop/api/mfapi/nf-mfapi-mfcreatemediatype) 物件來描述音訊串流的格式。 音訊格式有兩種類型：「主類型」及「子類型」。 主類型定義媒體的整體格式，例如視訊、音訊、指令碼等。 子類型定義 PCM、ADPCM 或 WMA 等格式。

**MediaStreamer：： Initialize**方法會使用[IMFAttributes：： SetGUID](https://docs.microsoft.com/windows/desktop/api/mfobjects/nf-mfobjects-imfattributes-setguid)方法，將主要類型（[MF_MT_MAJOR_TYPE](https://docs.microsoft.com/windows/desktop/medfound/mf-mt-major-type-attribute)）指定為音訊（**MFMediaType\_音訊**）和次要類型（[MF_MT_SUBTYPE](https://docs.microsoft.com/windows/desktop/medfound/mf-mt-subtype-attribute)），做為未壓縮的 pcm 音訊（**MFAudioFormat\_PCM**）。 **MF_MT_MAJOR_TYPE** 和 **MF_MT_SUBTYPE** 是[媒體基礎屬性](https://docs.microsoft.com/windows/desktop/medfound/media-foundation-attributes)。 **MFMediaType_Audio** 和 **MFAudioFormat_PCM** 是類型和子類型 GUID；如需詳細資訊，請參閱[音訊媒體類型](https://docs.microsoft.com/windows/desktop/medfound/audio-media-types)。 [IMFSourceReader::SetCurrentMediaType](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentmediatype) 方法會將媒體類型與資料流讀取器產生關聯。

```cpp
// Set the decoded output format as PCM. 
// XAudio2 on Windows can process PCM and ADPCM-encoded buffers. 
// When this sample uses Media Foundation, it always decodes into PCM.

DX::ThrowIfFailed(
    MFCreateMediaType(&mediaType)
    );

DX::ThrowIfFailed(
    mediaType->SetGUID(MF_MT_MAJOR_TYPE, MFMediaType_Audio)
    );

DX::ThrowIfFailed(
    mediaType->SetGUID(MF_MT_SUBTYPE, MFAudioFormat_PCM)
    );

DX::ThrowIfFailed(
    m_reader->SetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, 0, mediaType.Get())
    );
```

**MediaStreamer::Initialize** 方法接著會使用 [IMFSourceReader::GetCurrentMediaType](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getcurrentmediatype) 從媒體基礎取得完整的輸出媒體格式，然後呼叫 [MFCreateWaveFormatExFromMFMediaType](https://docs.microsoft.com/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype) 方法，將媒體基礎音訊媒體類型轉換為 [WAVEFORMATEX](https://docs.microsoft.com/windows/desktop/api/mmreg/ns-mmreg-twaveformatex) 結構。 **WAVEFORMATEX** 結構會定義波形音訊資料的格式。 Marble Maze 使用此結構來建立來源音，並將低通篩選器套用至彈珠滾動音效。

```cpp
// Get the complete WAVEFORMAT from the Media Type.
DX::ThrowIfFailed(
    m_reader->GetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, &outputMediaType)
    );

uint32 formatSize = 0;
WAVEFORMATEX* waveFormat;
DX::ThrowIfFailed(
    MFCreateWaveFormatExFromMFMediaType(outputMediaType.Get(), &waveFormat, &formatSize)
    );
CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
CoTaskMemFree(waveFormat);
```

> [!IMPORTANT]
> [MFCreateWaveFormatExFromMFMediaType](https://docs.microsoft.com/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype) 方法使用 **CoTaskMemAlloc** 來配置 [WAVEFORMATEX](https://docs.microsoft.com/windows/desktop/api/mmreg/ns-mmreg-twaveformatex) 物件。 因此，當此物件使用完畢時，請記得呼叫 **CoTaskMemFree**。

 

**MediaStreamer：： Initialize**方法會計算資料流程的長度（ **m\_maxStreamLengthInBytes**，以位元組為單位）來完成。 為了計算長度，它會呼叫 [IMFSourceReader::GetPresentationAttribute](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getpresentationattribute) 方法來取得音訊串流的持續期間 (以 100 奈秒為單位)，將持續期間分段，然後乘以平均資料傳輸率 (每秒位元組數)。 Marble Maze 之後會使用這個值來配置用於保存每個遊戲音效的緩衝區。

```cpp
// Get the total length of the stream, in bytes.
PROPVARIANT var;
DX::ThrowIfFailed(
    m_reader->
        GetPresentationAttribute(MF_SOURCE_READER_MEDIASOURCE, MF_PD_DURATION, &var)
    );

// duration is in 100ns units; convert to seconds, and round up
// to the nearest whole byte.
ULONGLONG duration = var.uhVal.QuadPart;
m_maxStreamLengthInBytes =
    static_cast<unsigned int>(
        ((duration * static_cast<ULONGLONG>(m_waveFormat.nAvgBytesPerSec)) + 10000000)
        / 10000000
        );
```

### <a name="creating-the-source-voices"></a>建立來源音

Marble Maze 會建立 XAudio2 來源音來播放每個遊戲音效和來源音中的音樂。 **Audio** 類別會定義 [IXAudio2SourceVoice](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2sourcevoice) 物件來播放背景音樂，也會定義 **SoundEffectData** 物件陣列來保存遊戲音效。 **SoundEffectData** 結構會保存效果的 **IXAudio2SourceVoice** 物件，並且定義與效果相關的其他資料，例如音訊緩衝區。 **Audio.h** 會定義 **SoundEvent** 列舉。 Marble Maze 使用這個列舉來識別每個遊戲音效。 **Audio** 類別也會使用這個列舉來編製 **SoundEffectData** 物件陣列的索引。

```cpp
enum SoundEvent
{
    RollingEvent        = 0,
    FallingEvent        = 1,
    CollisionEvent      = 2,
    CheckpointEvent     = 3,
    MenuChangeEvent     = 4,
    MenuSelectedEvent   = 5,
    LastSoundEvent,
};
```

下表顯示以下項目之間的關係：值、含有相關聯音效資料的檔案，以及每個音效所代表意義的簡短描述。 音訊檔案位於 **\\Media\\音訊** 資料夾中。

| SoundEvent 值  | 檔案名稱      | 描述                                              |
|-------------------|----------------|----------------------------------------------------------|
| RollingEvent      | MarbleRoll.wav | 彈珠滾動時播放。                              |
| FallingEvent      | MarbleFall.wav | 彈珠從迷宮掉落時播放。               |
| CollisionEvent    | MarbleHit.wav  | 彈珠碰撞到迷宮時播放。           |
| CheckpointEvent   | Checkpoint.wav | 彈珠通過關卡時播放。         |
| MenuChangeEvent   | MenuChange.wav | 使用者變更目前的選單項目時播放。 |
| MenuSelectedEvent | MenuSelect.wav | 使用者選取選單項目時播放。           |

 

下列範例顯示 **Audio::CreateResources** 方法如何建立背景音樂的來源音。 [XAUDIO2\_傳送\_描述](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_send_descriptor)元結構會定義來自另一個聲音的目標目的地語音，並指定是否應該使用篩選準則。 Marble Maze 會呼叫 **Audio::SetSoundEffectFilter** 方法來使用篩選器，以在彈珠滾動時變更音效。 [XAUDIO2\_VOICE\_](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_voice_sends)傳送結構會定義一組用來從單一輸出語音接收資料的聲音。 Marble Maze 會將來源音的資料傳送至主控音 (音效播放的「原始音」或未修飾部分) 及兩個副混音 (實作音效播放的「效果音」或迴響部分)。

[IXAudio2::CreateSourceVoice](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createsourcevoice) 方法會建立並設定來源音。 這個方法採用 [WAVEFORMATEX](https://docs.microsoft.com/windows/desktop/api/mmreg/ns-mmreg-twaveformatex) 結構，而此結構會定義傳送至音效的音訊緩衝區格式。 如前所述，Marble Maze 使用 PCM 格式。

```cpp
XAUDIO2_SEND_DESCRIPTOR descriptors[3];
descriptors[0].pOutputVoice = m_musicMasteringVoice;
descriptors[0].Flags = 0;
descriptors[1].pOutputVoice = m_musicReverbVoiceSmallRoom;
descriptors[1].Flags = 0;
descriptors[2].pOutputVoice = m_musicReverbVoiceLargeRoom;
descriptors[2].Flags = 0;
XAUDIO2_VOICE_SENDS sends = {0};
sends.SendCount = 3;
sends.pSends = descriptors;
WAVEFORMATEX& waveFormat = m_musicStreamer.GetOutputWaveFormatEx();

DX::ThrowIfFailed(
    m_musicEngine->CreateSourceVoice(&m_musicSourceVoice, &waveFormat, 0, 1.0f, &m_voiceContext, &sends, nullptr)
    );

DX::ThrowIfFailed(
    m_musicMasteringVoice->SetVolume(0.4f)
    );
```

## <a name="playing-background-music"></a>播放背景音樂


來源音最初建立時為停止狀態。 Marble Maze 會在遊戲迴圈中啟動背景音樂。 第一次呼叫  **MarbleMazeMain::Update** 時，會呼叫 **Audio::Start** 來啟動背景音樂。

```cpp
if (!m_audio.m_isAudioStarted)
{
    m_audio.Start();
}
```

**Audio::Start** 方法會呼叫 [IXAudio2SourceVoice::Start](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start) 來開始處理背景音樂的來源音。

```cpp
void Audio::Start()
{     
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    HRESULT hr = m_musicSourceVoice->Start(0);

    if SUCCEEDED(hr) {
        m_isAudioStarted = true;
    }
    else
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

來源音會將該音訊資料傳遞至音訊圖的下一個階段。 就 Marble Maze 而言，下一個階段包含將兩個殘響效果套用至音訊的兩個副混音。 一個副混音套用附近遠距離殘響，第二個則套用遠方遠距離殘響。

每個副混音在最終混音中所貢獻的量，取決於空間的大小和形狀。 當彈珠接近牆面或在較小空間時，近距離殘響貢獻較多，當彈珠在較大空間時，遠距離殘響貢獻較多。 這種技術可在彈珠穿越迷宮時產生更真實的回音效果。 若要進一步了解 Marble Maze 如何實作此效果，請參閱 Marble Maze 原始程式碼中的 **Audio::SetRoomSize** 和 **Physics::CalculateCurrentRoomSize**。

> [!NOTE]
> 在大多數空間大小幾乎相同的遊戲中，您可以使用更基本的殘響模式。 例如，您可以為所有空間使用一個殘響設定，也可以為每個空間建立預先定義的殘響設定。

**Audio::CreateResources** 方法會使用媒體基礎來載入背景音樂。 不過，來源音目前沒有要使用的音訊資料。 此外，因為背景音樂會循環播放，來源音必須定期更新資料，音樂才能繼續播放。

為了讓來源音持續填入資料，遊戲迴圈會隨每個畫面更新音訊緩衝區。 **MarbleMazeMain::Render** 方法會呼叫 **Audio::Render** 來處理背景音樂音訊緩衝區。 **音訊**類別定義三個音訊緩衝區的陣列， **m\_audioBuffers**。 每個緩衝區保存 64 KB (65536 個位元組) 的資料。 迴圈會從媒體基礎物件讀取資料，並將該資料寫入來源音，直到來源音有三個排入佇列的緩衝區為止。

> [!CAUTION]
> 雖然 Marble Maze 使用 64 KB 緩衝區保存音樂資料，但您可能需要使用更大或更小的緩衝區。 此數量視您遊戲的需求而定。

```cpp
// This sample processes audio buffers during the render cycle of the application.
// As long as the sample maintains a high-enough frame rate, this approach should
// not glitch audio. In game code, it is best for audio buffers to be processed
// on a separate thread that is not synced to the main render loop of the game.
void Audio::Render()
{
    if (m_engineExperiencedCriticalError)
    {
        m_engineExperiencedCriticalError = false;
        ReleaseResources();
        Initialize();
        CreateResources();
        Start();
        if (m_engineExperiencedCriticalError)
        {
            return;
        }
    }

    try
    {
        bool streamComplete;
        XAUDIO2_VOICE_STATE state;
        uint32 bufferLength;
        XAUDIO2_BUFFER buf = {0};

        // Use MediaStreamer to stream the buffers.
        m_musicSourceVoice->GetState(&state);
        while (state.BuffersQueued <= MAX_BUFFER_COUNT - 1)
        {
            streamComplete = m_musicStreamer.GetNextBuffer(
                m_audioBuffers[m_currentBuffer],
                STREAMING_BUFFER_SIZE,
                &bufferLength
                );

            if (bufferLength > 0)
            {
                buf.AudioBytes = bufferLength;
                buf.pAudioData = m_audioBuffers[m_currentBuffer];
                buf.Flags = (streamComplete) ? XAUDIO2_END_OF_STREAM : 0;
                buf.pContext = 0;
                DX::ThrowIfFailed(
                    m_musicSourceVoice->SubmitSourceBuffer(&buf)
                    );

                m_currentBuffer++;
                m_currentBuffer %= MAX_BUFFER_COUNT;
            }

            if (streamComplete)
            {
                // Loop the stream.
                m_musicStreamer.Restart();
                break;
            }

            m_musicSourceVoice->GetState(&state);
        }
    }
    catch (...)
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

迴圈也會處理媒體基礎物件何時到達資料流結尾。 在此情況下，它會呼叫 [IMFSourceReader::SetCurrentPosition](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentposition) 方法來重設音訊來源的位置。

```cpp
void MediaStreamer::Restart()
{
    if (m_reader == nullptr)
    {
        return;
    }

    PROPVARIANT var = {0};
    var.vt = VT_I8;

    DX::ThrowIfFailed(
        m_reader->SetCurrentPosition(GUID_NULL, var)
        );
}
```

若要針對單一緩衝區（或完全載入記憶體的整個音效）執行音訊迴圈，您可以在初始化音效時，將[XAUDIO2_BUFFER](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_buffer)：： LoopCount 欄位設定為**XAUDIO2\_迴圈\_無限**。 Marble Maze 採用這種技術來播放彈珠的滾動音效。

```cpp
if (sound == RollingEvent)
{
    m_soundEffects[sound].m_audioBuffer.LoopCount = XAUDIO2_LOOP_INFINITE;
}
```

不過，在背景音樂方面，Marble Maze 會直接管理緩衝區，所以可以更準確地控制所使用的記憶體數量。 當音樂檔很大時，您可以將音樂資料串流處理至較小的緩衝區。 這樣做有助於平衡記憶體大小與遊戲處理和串流化音訊資料的次數。

> [!TIP]
> 如果您遊戲的畫面撥放速率較低或有變化，則在主要執行緒處理音訊可能會造成音訊發生無法預期的中斷或突然播放，因為已緩衝處理的音訊資料不足以供音訊引擎使用。 如果遊戲容易受到此問題所影響，請考慮在不執行轉譯的獨立執行緒中處理音訊。 這個方法在具有多顆處理器的電腦上特別有用，因為遊戲可以使用閒置的處理器。

## <a name="reacting-to-game-events"></a>回應遊戲事件

**Audio** 類別提供 **PlaySoundEffect**、**IsSoundEffectStarted**、**StopSoundEffect**、**SetSoundEffectVolume**、**SetSoundEffectPitch** 和 **SetSoundEffectFilter** 等方法，讓遊戲控制何時播放和停止音效，以及控制音量和音調等音效屬性。 例如，如果彈珠從迷宮掉落，**MarbleMazeMain::Update** 會呼叫 **Audio::PlaySoundEffect** 方法來播放 **FallingEvent** 音效。

```cpp
m_audio.PlaySoundEffect(FallingEvent);
```

**Audio::PlaySoundEffect** 方法會呼叫 [IXAudio2SourceVoice::Start](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start) 方法來開始播放音效。 **IXAudio2SourceVoice::Start** 方法一旦呼叫過，就不會再度啟動。 **Audio::PlaySoundEffect** 接著會執行某些音效的自訂邏輯。

```cpp
void Audio::PlaySoundEffect(SoundEvent sound)
{
    XAUDIO2_BUFFER buf = {0};
    XAUDIO2_VOICE_STATE state = {0};

    if (m_engineExperiencedCriticalError)
    {
        // If there's an error, then we'll recreate the engine on the next
        // render pass.
        return;
    }

    SoundEffectData* soundEffect = &m_soundEffects[sound];
    HRESULT hr = soundEffect->m_soundEffectSourceVoice->Start();

    if FAILED(hr)
    {
        m_engineExperiencedCriticalError = true;
        return;
    }

    // For one-off voices, submit a new buffer if there's none queued up,
    // and allow up to two collisions to be queued up. 
    if (sound != RollingEvent)
    {
        XAUDIO2_VOICE_STATE state = {0};

        soundEffect->m_soundEffectSourceVoice->
            GetState(&state, XAUDIO2_VOICE_NOSAMPLESPLAYED);

        if (state.BuffersQueued == 0)
        {
            soundEffect->m_soundEffectSourceVoice->
                SubmitSourceBuffer(&soundEffect->m_audioBuffer);
        }
        else if (state.BuffersQueued < 2 && sound == CollisionEvent)
        {
            soundEffect->m_soundEffectSourceVoice->
                SubmitSourceBuffer(&soundEffect->m_audioBuffer);
        }

        // For the menu clicks, we want to stop the voice and replay the click
        // right away.
        // Note that stopping and then flushing could cause a glitch due to the
        // waveform not being at a zero-crossing, but due to the nature of the 
        // sound (fast and 'clicky'), we don't mind.
        if (state.BuffersQueued > 0 && sound == MenuChangeEvent)
        {
            soundEffect->m_soundEffectSourceVoice->Stop();
            soundEffect->m_soundEffectSourceVoice->FlushSourceBuffers();

            soundEffect->m_soundEffectSourceVoice->
                SubmitSourceBuffer(&soundEffect->m_audioBuffer);

            soundEffect->m_soundEffectSourceVoice->Start();
        }
    }

    m_soundEffects[sound].m_soundEffectStarted = true;
}
```

對於滾動以外的音效，**Audio::PlaySoundEffect** 方法會呼叫 [IXAudio2SourceVoice::GetState](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-getstate) 來決定來源音播放的緩衝區數目。 如果沒有作用中的緩衝區，它會呼叫 [IXAudio2SourceVoice::SubmitSourceBuffer](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-submitsourcebuffer)，將音效的音訊資料新增至聲音的輸入佇列。 **Audio::PlaySoundEffect** 方法也能夠連續播放兩次碰撞音效。 例如，當彈珠碰撞到迷宮角落時，就會出現這種情形。

如前文所述，音訊類別會在初始化滾動事件的音效時，使用**XAUDIO2\_迴圈\_無限**旗標。 第一次因為此事件而呼叫 **Audio::PlaySoundEffect** 時，就會開始循環播放音效。 為了簡化滾動音效的播放邏輯，Marble Maze 會變成靜音，而非停止音效。 當彈珠速度改變時，Marble Maze 也會隨之變更音效的音調和音量，以產生更真實的效果。 以下程式碼顯示 **MarbleMazeMain::Update** 方法如何隨著彈珠速度的改變來更新音調和音量，以及如何在彈珠停止時將音量設定為零以變成靜音。

```cpp
// Play the roll sound only if the marble is actually rolling.
if (ci.isRollingOnFloor && volume > 0)
{
    if (!m_audio.IsSoundEffectStarted(RollingEvent))
    {
        m_audio.PlaySoundEffect(RollingEvent);
    }

    // Update the volume and pitch by the velocity.
    m_audio.SetSoundEffectVolume(RollingEvent, volume);
    m_audio.SetSoundEffectPitch(RollingEvent, pitch);

    // The rolling sound has at most 8000Hz sounds, so we linearly
    // ramp up the low-pass filter the faster we go.
    // We also reduce the Q-value of the filter, starting with a
    // relatively broad cutoff and get progressively tighter.
    m_audio.SetSoundEffectFilter(
        RollingEvent,
        600.0f + 8000.0f * volume,
        XAUDIO2_MAX_FILTER_ONEOVERQ - volume*volume
        );
}
else
{
    m_audio.SetSoundEffectVolume(RollingEvent, 0);
}
```

## <a name="reacting-to-suspend-and-resume-events"></a>回應暫停和繼續事件

[Marble Maze 應用程式結構](marble-maze-application-structure.md) 描述 Marble Maze 支援如何暫停和繼續。 當遊戲暫停時，遊戲會暫停音訊。 當遊戲繼續時，遊戲會從先前暫停的地方繼續播放音訊。 我們這樣做是為了遵循「除非必要，否則絕不使用資源」的最佳做法。

遊戲暫停時會呼叫 **Audio::SuspendAudio** 方法。 這個方法會呼叫 [IXAudio2::StopEngine](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-stopengine) 方法來停止所有音訊。 雖然 **IXAudio2::StopEngine** 會立即停止所有音訊輸出，但會保留音訊圖及其效果參數 (例如，彈珠反彈時套用的殘響效果)。

```cpp
// Uses the IXAudio2::StopEngine method to stop all audio immediately.  
// It leaves the audio graph untouched, which preserves all effect parameters   
// and effect histories (like reverb effects) voice states, pending buffers,  
// cursor positions and so on. 
// When the engines are restarted, the resulting audio will sound as if it had  
// never been stopped except for the period of silence. 
void Audio::SuspendAudio()
{
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    if (m_isAudioStarted)
    {
        m_musicEngine->StopEngine();
        m_soundEffectEngine->StopEngine();
    }

    m_isAudioStarted = false;
}
```

遊戲繼續時會呼叫 **Audio::ResumeAudio** 方法。 這個方法會使用 [IXAudio2::StartEngine](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-startengine) 方法來重新啟動音訊。 由於呼叫 [IXAudio2::StopEngine](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-stopengine) 會保留音訊圖及其效果參數，音訊輸出會從先前暫停的地方繼續。

```cpp
// Restarts the audio streams. A call to this method must match a previous call
// to SuspendAudio. This method causes audio to continue where it left off.
// If there is a problem with the restart, the m_engineExperiencedCriticalError
// flag is set. The next call to Render will recreate all the resources and
// reset the audio pipeline.
void Audio::ResumeAudio()
{
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    HRESULT hr = m_musicEngine->StartEngine();
    HRESULT hr2 = m_soundEffectEngine->StartEngine();

    if (FAILED(hr) || FAILED(hr2))
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

## <a name="handling-headphones-and-device-changes"></a>處理耳機和裝置變更

Marble Maze 使用引擎回呼來處理 XAudio2 引擎失敗情形，例如當音訊裝置變更時。 裝置變更的原因可能是因為玩家插上耳機或拔除耳機。 建議您實作引擎回呼來處理裝置變更。 否則，當使用者插入耳機或拔出耳機時，遊戲會停止播放音效，直到重新啟動為止。

**Audio.h** 會定義 **AudioEngineCallbacks** 類別。 這個類別實作 [IXAudio2EngineCallback](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2enginecallback) 介面。

```cpp
class AudioEngineCallbacks: public IXAudio2EngineCallback
{
private:
    Audio* m_audio;

public :
    AudioEngineCallbacks(){};
    void Initialize(Audio* audio);

    // Called by XAudio2 just before an audio processing pass begins.
    void _stdcall OnProcessingPassStart(){};

    // Called just after an audio processing pass ends.
    void  _stdcall OnProcessingPassEnd(){};

    // Called when a critical system error causes XAudio2
    // to be closed and restarted. The error code is given in Error.
    void  _stdcall OnCriticalError(HRESULT Error);
};
```

[IXAudio2EngineCallback](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2enginecallback) 介面可以在發生音訊處理事件及引擎遇到嚴重錯誤時通知您的程式碼。 為了註冊回呼，Marble Maze 在建立音樂引擎的 [IXAudio2](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-registerforcallbacks) 物件之後會呼叫 **Audio::CreateResources** 中的 [IXAudio2::RegisterForCallbacks](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) 方法。

```cpp
m_musicEngineCallback.Initialize(this);
m_musicEngine->RegisterForCallbacks(&m_musicEngineCallback);
```

Marble Maze 不需要在音訊處理開始或結束時收到通知。 因此，它實作的 [IXAudio2EngineCallback::OnProcessingPassStart](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2enginecallback-onprocessingpassstart) 和 [IXAudio2EngineCallback::OnProcessingPassEnd](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2enginecallback-onprocessingpassend) 方法沒有作用。 若為[IXAudio2EngineCallback：： OnCriticalError](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2enginecallback-oncriticalerror)方法，大理石迷宮會呼叫**SetEngineExperiencedCriticalError**方法，其會設定**m\_engineExperiencedCriticalError**旗標。

```cpp
// Audio.cpp

// Called when a critical system error causes XAudio2 
// to be closed and restarted. The error code is given in Error. 
void  _stdcall AudioEngineCallbacks::OnCriticalError(HRESULT Error)
{
    m_audio->SetEngineExperiencedCriticalError();
}
```

```cpp
// Audio.h (Audio class)

// This flag can be used to tell when the audio system 
// is experiencing critial errors.
// XAudio2 gives a critical error when the user unplugs
// the headphones and a new speaker configuration is generated.
void SetEngineExperiencedCriticalError()
{
    m_engineExperiencedCriticalError = true;
}
```

發生嚴重錯誤時，音訊處理會停止，而且對 XAudio2 的其他所有呼叫都會失敗。 為了從這種情況中復原，您必須釋放 XAudio2 執行個體並建立新的執行個體。 從遊戲迴圈每個畫面格呼叫的**音訊：： Render**方法，會先檢查**m\_engineExperiencedCriticalError**旗標。 如果有設定此旗標，則會加以清除、釋放目前的 XAudio2 執行個體、初始化資源，然後啟動背景音樂。

```cpp
if (m_engineExperiencedCriticalError)
{
    m_engineExperiencedCriticalError = false;
    ReleaseResources();
    Initialize();
    CreateResources();
    Start();
    if (m_engineExperiencedCriticalError)
    {
        return;
    }
}
```

當沒有可用的音訊裝置時，大理石迷宮也會使用**m\_engineExperiencedCriticalError**旗標來防止呼叫 XAudio2。 例如，有設定此旗標時，**MarbleMazeMain::Update** 方法便不會處理滾動或碰撞事件的音訊。 應用程式會視需要在每個畫面格嘗試修復音訊引擎;不過，如果電腦沒有音訊裝置，或耳機未插上，而且沒有其他可用的音訊裝置，就一定會設定**m\_engineExperiencedCriticalError**旗標。

> [!CAUTION]
> 請勿在引擎回呼的主體中執行封鎖操作。 這樣做可能會造成效能問題。 Marble Maze 會在 **OnCriticalError** 回呼中設定旗標，之後再於一般音訊處理階段中處理錯誤。 如需 XAudio2 回呼的詳細資訊，請參閱 [XAudio2 回呼](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-callbacks)。

## <a name="conclusion"></a>結論

這會摘要 Marble Maze 遊戲範例！ 雖然它相對是個簡單遊戲，但包含數個重要部分，可移至任何 UWP DirectX 遊戲，並且是在製作您自己的遊戲時可遵循的很好範例。

現在您已經完成下列事項，請嘗試稍微修補原始程式碼並查看其變化。 或查看[使用 DirectX 建立簡單 UWP 遊戲](tutorial--create-your-first-uwp-directx-game.md)，這是另一個 UWP DirectX 遊戲範例。

準備好進一步了解 DirectX 嗎？ 然後在 [DirectX 程式設計](directx-programming.md)查看我們的指南。

如果您有興趣在 UWP 上開發遊戲，請參閱[遊戲程式設計](index.md)的文件。

## <a name="related-topics"></a>相關主題

* [將輸入和互動性新增至大理石迷宮範例](adding-input-and-interactivity-to-the-marble-maze-sample.md)
* [開發大理石迷宮，和 DirectX 中C++的 UWP 遊戲](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)