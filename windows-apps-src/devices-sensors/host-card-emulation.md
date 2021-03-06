---
ms.assetid: 26834A51-512B-485B-84C8-ABF713787588
title: 建立 NFC 智慧卡應用程式
description: Windows Phone 8.1 使用以 SIM 卡為基礎的安全元素來支援 NFC 卡模擬應用程式，但該模型需要安全的付款應用程式才能與行動網路運算子 (MNO) 緊密結合。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c06611f1694ed45180409c200e7958ef83c76319
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684795"
---
# <a name="create-an-nfc-smart-card-app"></a>建立 NFC 智慧卡應用程式


**重要**  本主題僅適用于 Windows 10 行動裝置版。

Windows Phone 8.1 使用以 SIM 卡為基礎的安全元素來支援 NFC 卡模擬應用程式，但該模型需要安全的付款應用程式才能與行動網路運算子 (MNO) 緊密結合。 這會限制其他未結合 MNO 的商家或開發人員所提供的各種可能付款解決方案， 在 Windows 10 行動裝置版中，我們已導入新的卡片模擬技術，稱為主機卡模擬 (HCE)。 HCE 技術可讓您的應用程式直接與 NFC 卡讀卡機進行通訊。 本主題說明主機卡模擬 (HCE) 在 Windows 10 行動裝置版裝置上的運作方式，以及如何開發 HCE 應用程式，讓您的客戶可以透過他們的手機而不是實體卡片存取您的服務，而不需要使用 MNO 共同作業。

## <a name="what-you-need-to-develop-an-hce-app"></a>開發 HCE 應用程式所需的項目


若要開發適用于 Windows 10 行動裝置版的 HCE 型卡模擬應用程式，您必須設定開發環境。 您可以安裝 Microsoft Visual Studio 2015，包括 Windows 開發人員工具和具有 NFC 模擬支援的 Windows 10 行動裝置模擬器，藉以進行設定。 如需開始設定的詳細資訊，請參閱[開始設定](https://docs.microsoft.com/windows/uwp/get-started/get-set-up)

或者，如果您想要使用實際的 Windows 10 行動裝置（而不是隨附的 Windows 10 行動模擬器）進行測試，您也需要下列專案。

-   具有 NFC HCE 支援的 Windows 10 行動裝置。 Lumia 730、830、640 和 640 XL 目前提供硬體來支援 NFC HCE 應用程式。
-   支援 ISO/IEC 14443-4 和 ISO/IEC 7816-4 通訊協定的讀卡機終端機

Windows 10 Mobile 會執行提供下列功能的 HCE 服務。

-   應用程式可以登錄它們想要模擬之卡片的小程式識別碼 (AID)。
-   根據外部讀卡機卡片選取和使用者喜好設定，對於某一個已登錄的應用程式進行應用程式通訊協定資料單位 (APDU) 命令和回應組的衝突解決和路由。
-   處理應用程式的事件和通知做為使用者動作的結果。

Windows 10 支援模擬以 ISO DEP （ISO-IEC 14443-4）為基礎的智慧卡，並使用 ISO IEC 7816-4 規格中所定義的 Apdu 進行通訊。 Windows 10 支援 ISO/IEC 14443-4 輸入 HCE 應用程式的技術。 根據預設，會將類型 B、類型 F 及非 ISO-DEP (例如 MIFARE) 技術路由傳送到 SIM 卡。

只有具備「卡片模擬」功能的 Windows 10 行動裝置才會啟用。 其他版本的 Windows 10 無法使用以 SIM 為基礎和以 HCE 為基礎的卡片模擬。

下圖顯示以 HCE 和 SIM 為基礎的卡片模擬支援架構。

![HCE 和 SIM 卡模擬的架構](./images/nfc-architecture.png)

## <a name="app-selection-and-aid-routing"></a>應用程式選取和 AID 路由

若要開發 HCE 應用程式，您必須瞭解 Windows 10 Mobile 裝置如何將輔助應用程式路由傳送至特定的 app，因為使用者可以安裝多個不同的 HCE 應用程式。 每個應用程式都可登錄多個以 HCE 和 SIM 卡為基礎的卡片。 使用 SIM 型的舊版 Windows Phone 8.1 應用程式，只要使用者選擇 [SIM 卡] 選項作為 [NFC 設定] 功能表中的預設付款卡，就會繼續在 Windows 10 行動裝置版上執行。 此選項為首次開啟裝置時的預設設定。

當使用者將其 Windows 10 行動裝置應用程式分到終端機時，資料會自動路由傳送至裝置上安裝的適當 app。 這個路由是以小程式識別碼 (AID) 為根據，這類識別碼是 5-16 位元組的識別碼 。 輕觸期間，外部終端機將傳輸 SELECT 命令 APDU 來指定 AID，它就像所有後續要路由傳送的 APDU 命令一樣。 後續的 SELECT 命令將再次變更路由。 根據應用程式登錄的 AID 和使用者設定，會將 APDU 流量路由傳送到特定的應用程式，這將會傳送回應 APDU。 請注意，終端機可能想要在同一個輕觸期間，與數種不同的應用程式進行通訊。 因此，您必須確定應用程式的背景工作會在停用時儘快結束，以便為另一個應用程式的背景工作產生更多空間來回應 APDU。 我們將在本主題稍後討論背景工作。

HCE 應用程式必須利用它們可處理的特殊 AID 來登錄自己，讓它們能夠接收 AID的 APDU。 應用程式會使用 AID 群組來宣告 AID。 AID 群組在概念上相當於個別的實體卡。 例如，某一張信用卡是使用一個 AID 群組來宣告，而第二張來自其他銀行的信用卡則是利用不同的第二個 AUD 群組來宣告，儘管這兩張信用卡具備同一個 AID 也一樣。

## <a name="conflict-resolution-for-payment-aid-groups"></a>針對付款 AID 群組的衝突解決

當應用程式登錄實體卡片 (AID 群組) 時，可將 AID 群組類別宣告為 [付款] 或 [其他]。 儘管在任何指定期間內可登錄多個付款 AID 群組，但針對「輕觸以付款」一次只能啟用一個付款 AID 群組，該群組將會由使用者選取。 這種行為存在是因為使用者預期能夠控制自行選擇要使用單一付款、信用卡或轉帳卡，如此一來，當他們將其裝置輕觸終端機時，就不會用到其他不想使用的卡片來支付。

不過，已登錄為 [其他] 的多個 AID 群組可以在不需使用者互動的情況下同時啟用。 這種行為之所以存在，是因為使用者透過點選手機使用其他類型的卡片 (例如，忠誠卡、折價券或大眾運輸卡) 時，應該不須要花費任何心力或收到任何提示便可以使用。

已登錄為 [付款] 的所有 AID 群組會出現在 [NFC 設定] 頁面的卡片清單中，使用者可在其中選取預設付款卡。 選取預設付款卡之後，已登錄這個付款 AID 群組的預設付款卡就會成為預設付款應用程式。 預設付款應用程式可以在沒有使用者互動的情況下，啟用或停用它們的任何 AID 群組。 如果使用者拒絕預設付款應用程式的提示，則目前的預設付款應用程式 (如果有的話) 就會繼續保持為預設值。 下列螢幕擷取畫面會顯示 [NFC 設定] 頁面。

![[NFC 設定] 頁面的螢幕擷取畫面](./images/nfc-settings.png)

以上述的範例螢幕擷取畫面為例，如果使用者將預設付款卡變更為另一張未向「HCE Application 1」登錄的卡片，則系統會建立確認提示來取得該使用者的同意。 不過，如果使用者將預設付款卡變更為另一張已向「HCE Application 1」登錄的卡片，則系統不會為使用者建立確認提示，因為「HCE Application 1」已經是預設的付款 app。

## <a name="conflict-resolution-for-non-payment-aid-groups"></a>針對非付款 AID 群組的衝突解決

分類為 [其他] 的非付款卡不會出現在 [NFC設定] 頁面中。

您的應用程式可以使用與付款 AID 群組的相同方式，來建立、登錄並啟用非付款 AID 群組。 主要差異在於非付款 AID 群組已將模擬類別設定為 [其他] 而不是 [付款]。 向系統登錄 AID 群組之後，您需要啟用 AID 群組來接收 NFC 流量。 當您嘗試啟用非付款 AID 群組來接收流量時，除非與其他應用程式已經登錄於系統中的其中一個 AID 發生衝突，否則系統不會提示使用者進行確認。 如果發生衝突，若使用者選擇啟用最新登錄的 AID 群組，系統將提示使用者關於哪一張卡片及其相關聯應用程式即將停用的相關資訊。

**與 SIM 型 NFC 應用程式共存**

在 Windows 10 行動裝置版中，系統會設定用來在控制器層進行路由決策的 NFC 控制器路由表。 此表格包含下列項目的路由資訊。

-   個別的 AID 路由。
-   以通訊協定為基礎的路由 (ISO-DEP)。
-   以技術為基礎的路由 (NFC-A/B/F)。

當外部讀卡機傳送「SELECT AID」命令時，NFC 控制器會先檢查路由表中的 AID 路由來尋找相符項目。 如果沒有相符項目，將使用以通訊協定為基礎的路由做為 ISO-DEP (14443-4-A) 流量的預設路由。 對於任何其他非 ISO-DEP 流量，將使用以技術基礎的路由。

Windows 10 行動裝置版在 NFC 設定頁面中提供功能表選項「SIM 卡」，以繼續使用舊版 Windows Phone 8.1 SIM 型應用程式，而不會向系統註冊其輔助。 如果使用者選取 [SIM 卡] 做為預設付款卡，則 ISO-DEP 路由會設定為 UICC，針對下拉式功能表中的所有其他選取項目，ISO-DEP 路由會指向到主機。

當裝置第一次使用 Windows 10 Mobile 開機時，ISO DEP 路由會設定為具有 SE 已啟用 SIM 卡的裝置的「SIM 卡」。 當使用者安裝已啟用 HCE 的應用程式，且該應用程式會啟用任何 HCE AID 群組登錄時，ISO-DEP 路由將會指向主機。 以 SIM卡為基礎的新應用程式需針對要在控制器路由表中填入的特定 AID 路由，在 SIM 卡中依序登錄 AID。

## <a name="creating-an-hce-based-app"></a>建立以 HCE 為基礎的應用程式

您的 HCE 應用程式具有兩個部分。

-   適用於使用者互動的主要前景應用程式。
-   由系統觸發來處理指定 AID 之 APDU 的背景工作。

由於對於載入背景工作以回應 NFC 輕觸效能需求非常嚴謹，因此建議您在 C++/CX 原生程式碼 (包括任何相依性、資源或您相依的程式庫) 而非 C# 或 Managed 程式碼中實作整個背景工作。 儘管 C# 和 Managed 程式碼通常能夠順利執行，但還是會產生額外負荷，例如，載入 .NET CLR，而您可以使用 C++/CX 進行撰寫來避免。
## <a name="create-and-register-your-background-task"></a>建立並登錄您的背景工作

您必須在 HCE 應用程式中建立背景工作，來處理和回應系統要路由傳送到它的 APDU。 在第一次啟動您的應用程式期間，前景會登錄實作 [**IBackgroundTaskRegistration**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskRegistration) 介面的 HCE 背景工作，如下列程式碼所示。

```csharp
var taskBuilder = new BackgroundTaskBuilder();
taskBuilder.Name = bgTaskName;
taskBuilder.TaskEntryPoint = taskEntryPoint;
taskBuilder.SetTrigger(new SmartCardTrigger(SmartCardTriggerType.EmulatorHostApplicationActivated));
bgTask = taskBuilder.Register();
```

請注意，工作觸發程序已設定為 [**SmartCardTriggerType**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardTriggerType)。 **EmulatorHostApplicationActivated**。 這表示每當要解析應用程式的作業系統收到 SELECT AID 命令 APDU 時，您的背景工作即會啟動。

## <a name="receive-and-respond-to-apdus"></a>接收和回應 APDU

如果有 APDU 的目標是您的應用程式時，系統將會啟動您的背景工作。 您的背景工作將會接收透過 [**SmartCardEmulatorApduReceivedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardEmulatorApduReceivedEventArgs) 物件的 [**CommandApdu**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardemulatorapdureceivedeventargs.commandapdu) 屬性傳遞的 APDU，並使用相同物件的 [**TryRespondAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardemulatorapdureceivedeventargs.tryrespondwithcryptogramsasync) 方法來回應 APDU。 基於效能考量，請考慮讓您的背景工作保持為輕量型作業。 例如，在完成所有處理時，立即回應 APDU，並結束您的背景工作。 由於 NFC 交易的性質，使用者通常只會在讀卡機上持有其裝置一段極短暫的時間。 您的背景工作將繼續接收來自讀卡機的流量，直到您的連線停用為止，在這種情況下，您將會接收到 [**SmartCardEmulatorConnectionDeactivatedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardEmulatorConnectionDeactivatedEventArgs) 物件。 您的連線會因為下列原因 (如 [**SmartCardEmulatorConnectionDeactivatedEventArgs.Reason**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardemulatorconnectiondeactivatedeventargs.reason) 屬性中所示) 而停用。

-   如果連線是使用 **ConnectionLost** 值來停用，就表示使用者已將他們的裝置抽離讀卡機。 如果您的應用程式需要使用者輕觸終端機較長的時間，您可能要考慮透過回饋來提示他們。 您應該快速終止背景工作 (藉由完成您的延遲)，以確保使用者再次輕觸它時，它不會因等待前一個背景工作結束而延遲。
-   如果連線是使用 **ConnectionRedirected** 值來停用，就表示終端機所傳送的新 SELECT AID 命令 APDU 並導向至不同的 AID。 在此情況下，您的應用程式應該立即結束背景工作 (藉由完成您的延遲)，以便讓另一個背景工作能夠執行。

背景工作也應該在 [**IBackgroundTaskInstance interface**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) 上針對 [**Canceled event**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.canceled) 進行登錄，然後同樣地快速結束背景工作 (藉由完成您的延遲)，因為這個事件會在您的背景工作完成它時由系統所引發。 以下是示範 HCE 應用程式背景工作的程式碼。

```csharp
void BgTask::Run(
    IBackgroundTaskInstance^ taskInstance)
{
    m_triggerDetails = static_cast<SmartCardTriggerDetails^>(taskInstance->TriggerDetails);
    if (m_triggerDetails == nullptr)
    {
        // May be not a smart card event that triggered us
        return;
    }

    m_emulator = m_triggerDetails->Emulator;
    m_taskInstance = taskInstance;

    switch (m_triggerDetails->TriggerType)
    {
    case SmartCardTriggerType::EmulatorHostApplicationActivated:
        HandleHceActivation();
        break;

    case SmartCardTriggerType::EmulatorAppletIdGroupRegistrationChanged:
        HandleRegistrationChange();
        break;

    default:
        break;
    }
}

void BgTask::HandleHceActivation()
{
 try
 {
        auto lock = m_srwLock.LockShared();
        // Take a deferral to keep this background task alive even after this "Run" method returns
        // You must complete this deferal immediately after you have done processing the current transaction
        m_deferral = m_taskInstance->GetDeferral();

        DebugLog(L"*** HCE Activation Background Task Started ***");

        // Set up a handler for if the background task is cancelled, we must immediately complete our deferral
        m_taskInstance->Canceled += ref new Windows::ApplicationModel::Background::BackgroundTaskCanceledEventHandler(
            [this](
            IBackgroundTaskInstance^ sender,
            BackgroundTaskCancellationReason reason)
        {
            DebugLog(L"Cancelled");
            DebugLog(reason.ToString()->Data());
            EndTask();
        });

        if (Windows::Phone::System::SystemProtection::ScreenLocked)
        {
            auto denyIfLocked = Windows::Storage::ApplicationData::Current->RoamingSettings->Values->Lookup("DenyIfPhoneLocked");
            if (denyIfLocked != nullptr && (bool)denyIfLocked == true)
            {
                // The phone is locked, and our current user setting is to deny transactions while locked so let the user know
                // Denied
                DoLaunch(Denied, L"Phone was locked at the time of tap");

                // We still need to respond to APDUs in a timely manner, even though we will just return failure
                m_fDenyTransactions = true;
            }
        }
        else
        {
            m_fDenyTransactions = false;
        }

        m_emulator->ApduReceived += ref new TypedEventHandler<SmartCardEmulator^, SmartCardEmulatorApduReceivedEventArgs^>(
            this, &BgTask::ApduReceived);

        m_emulator->ConnectionDeactivated += ref new TypedEventHandler<SmartCardEmulator^, SmartCardEmulatorConnectionDeactivatedEventArgs^>(
                [this](
                SmartCardEmulator^ emulator,
                SmartCardEmulatorConnectionDeactivatedEventArgs^ eventArgs)
            {
                DebugLog(L"Connection deactivated");
                EndTask();
            });

  m_emulator->Start();
        DebugLog(L"Emulator started");
 }
 catch (Exception^ e)
 {
        DebugLog(("Exception in Run: " + e->ToString())->Data());
        EndTask();
 }
}
```
## <a name="create-and-register-aid-groups"></a>建立並登錄 AID 群組

當您的應用程式在佈建卡片時首次啟動期間，您將會建立 AID 群組並向系統登錄。 系統會根據登錄的 AID 和使用者設定，來判斷外部讀卡機想要交談並據以路由傳送 APDU 的應用程式。

大部分的付款卡都會針對同一個 AID (其為 PPSE AID) 以及其他付款網路卡特定的 AID 進行登錄。 每個 AID 群組都代表一張卡片，而且當使用者啟用該卡片時，群組中的所有 AID 都會啟用。 同樣地，當使用者會停用卡片時，群組中的所有 AID 都會停用。

若要登錄 AID 群組，您需要建立 [**SmartCardAppletIdGroup**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardAppletIdGroup) 物件並設定其屬性，以反映這是以 HCE 為基礎的付款卡。 對使用者而言，您的顯示名稱應該具備描述性，因為它將會在 NFC 設定功能表以及使用者提示中顯示。 針對 HCE 付款卡，應該將 [**SmartCardEmulationCategory**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationcategory) 屬性設定為 **Payment**，而且應該將 [**SmartCardEmulationType**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationtype) 屬性設定為 **Host**。

```csharp
public static byte[] AID_PPSE =
        {
            // File name "2PAY.SYS.DDF01" (14 bytes)
            (byte)'2', (byte)'P', (byte)'A', (byte)'Y',
            (byte)'.', (byte)'S', (byte)'Y', (byte)'S',
            (byte)'.', (byte)'D', (byte)'D', (byte)'F', (byte)'0', (byte)'1'
        };

var appletIdGroup = new SmartCardAppletIdGroup(
                        "Example DisplayName",
                                new List<IBuffer> {AID_PPSE.AsBuffer()},
                                SmartCardEmulationCategory.Payment,
                                SmartCardEmulationType.Host);
```

針對非付款 HCE 卡，應該將 [**SmartCardEmulationCategory**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationcategory) 屬性設定為 **Other**，而且應該將 [**SmartCardEmulationType**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationtype) 屬性設定為 **Host**。

```csharp
public static byte[] AID_OTHER =
        {
            (byte)'1', (byte)'2', (byte)'3', (byte)'4',
            (byte)'5', (byte)'6', (byte)'7', (byte)'8',
            (byte)'O', (byte)'T', (byte)'H', (byte)'E', (byte)'R'
        };

var appletIdGroup = new SmartCardAppletIdGroup(
                        "Example DisplayName",
                                new List<IBuffer> {AID_OTHER.AsBuffer()},
                                SmartCardEmulationCategory.Other,
                                SmartCardEmulationType.Host);
```

您最多可以針對每個 AID 群組包含 9 個 AID (每個長度為 5-16 位元組)。

使用 [**RegisterAppletIdGroupAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardemulator.registerappletidgroupasync) 方法來向系統登錄您的 AID 群組，這樣將會傳回 [**SmartCardAppletIdGroupRegistration**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) 物件。 根據預設，會將登錄物件的 [**ActivationPolicy**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) 屬性設定為 **Disabled**。 這表示即使您的 AID 已向系統登錄，但它們仍未啟用且將不會接收流量。

```csharp
reg = await SmartCardEmulator.RegisterAppletIdGroupAsync(appletIdGroup);
```

您可以使用 [**SmartCardAppletIdGroupRegistration**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) 類別的 [**RequestActivationPolicyChangeAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) 方法來啟用登錄的卡片 (AID 群組)，如下所示。 由於系統上一次只能啟用單一付款卡，因此將付款 AID 群組的 [**ActivationPolicy**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) 設定為 **Enabled**，與設定預設付款卡相同。 系統將提示使用者允許此卡片做為預設付款卡，不論是否已經選取預設付款卡。 如果您的應用程式已經是預設付款應用程式，而且只會在它自己的 AID 群組之間變更，則這個論點並不正確。 您最多可以針對每個應用程式登錄 10 個 AID 群組。

```csharp
reg.RequestActivationPolicyChangeAsync(AppletIdGroupActivationPolicy.Enabled);
```

您可以查詢應用程式已向作業系統登錄的 AID 群組，並使用 [**GetAppletIdGroupRegistrationsAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardemulator.getappletidgroupregistrationsasync) 方法來檢查它們的啟用原則。

當您將付款卡的啟用原則從 **Disabled** 變更為 **Enabled** 時，唯有在您的應用程式不是預設付款應用程式的情況下，使用者才會收到提示。 如果發生 AID 衝突，使用者將會在您將非付款卡的啟用原則從 **Disabled** 變更為 **Enabled** 時收到提示。

```csharp
var registrations = await SmartCardEmulator.GetAppletIdGroupRegistrationsAsync();
    foreach (var registration in registrations)
    {
registration.RequestActivationPolicyChangeAsync (AppletIdGroupActivationPolicy.Enabled);
    }
```

**啟用原則變更時的事件通知**

在您的背景工作中，您可以登錄以在其中一個 AID 登錄的啟用原則在應用程式外部發生變更時接收到相關事件。 例如，使用者可能透過 NFC 設定功能表來變更預設付款應用程式，從您的某一張卡片變更為其他應用程式裝載的另一張卡片。 如果您的應用程式需要了解這個對於內部設定的變更 (例如更新動態磚)，您可以接收關於此變更的事件通知，並在應用程式中據以採取動作。

```csharp
var taskBuilder = new BackgroundTaskBuilder();
taskBuilder.Name = bgTaskName;
taskBuilder.TaskEntryPoint = taskEntryPoint;
taskBuilder.SetTrigger(new SmartCardTrigger(SmartCardTriggerType.EmulatorAppletIdGroupRegistrationChanged));
bgTask = taskBuilder.Register();
```

## <a name="foreground-override-behavior"></a>前景覆寫行為

您可以在您的應用程式仍在前景中執行時，將任一個 AID 群組登錄的 [**ActivationPolicy**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) 變更為 **ForegroundOverride**，而使用者將不會收到提示。 當使用者在您的應用程式仍於前景中執行時，使用他們的裝置輕觸終端機時，即使該使用者未選取您的任何一張付款卡做為預設付款卡，仍會將流量路由傳送到您的應用程式。 當您將卡片的啟用原則變更為 **ForegroundOverride** 時，這個變更只會短暫存在，直到您的應用程式離開前景為止，而且它將不會變更使用者目前設定的預設付款卡。 您可以從前景應用程式中變更付款或非付款卡的 **ActivationPolicy**，如下所示。 請注意，[**RequestActivationPolicyChangeAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) 方法只能從前景應用程式呼叫，無法從背景工作呼叫。

```csharp
reg.RequestActivationPolicyChangeAsync(AppletIdGroupActivationPolicy.ForegroundOverride);
```

此外，您可以登錄由單一 0 長度的 AID 所組成的 AID 群組，這將導致系統路由傳送所有 APDU (無論 AID 為何)，這也包含接收到 SELECT AID 命令之前傳送的任何命令 APDU。 不過，這類 AID 群組僅能在您的應用程式於前景執行時運作，因為它只能設定為 **ForegroundOverride** 且無法永久啟用。 此外，此機制可針對 [**SmartCardEmulationType**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardEmulationType) 列舉的 **Host** 和 **UICC** 值進行運作，以便將所有流量路由傳送到 HCE 背景工作或 SIM 卡。

```csharp
public static byte[] AID_Foreground =
        {};

var appletIdGroup = new SmartCardAppletIdGroup(
                        "Example DisplayName",
                                new List<IBuffer> {AID_Foreground.AsBuffer()},
                                SmartCardEmulationCategory.Other,
                                SmartCardEmulationType.Host);
reg = await SmartCardEmulator.RegisterAppletIdGroupAsync(appletIdGroup);
reg.RequestActivationPolicyChangeAsync(AppletIdGroupActivationPolicy.ForegroundOverride);
```

## <a name="check-for-nfc-and-hce-support"></a>檢查 NFC 和 HCE 支援

您的應用程式應該檢查裝置是否具備 NFC 硬體、支援卡片模擬功能，以及支援主機卡模擬，然後再將這類功能提供給使用者。

NFC 智慧卡模擬功能只會在 Windows 10 行動裝置版上啟用，因此嘗試在任何其他版本的 Windows 10 中使用智慧卡模擬器 Api 將會導致錯誤。 您可以在下列程式碼片段中查看智慧卡 API 支援。

```csharp
Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.Devices.SmartCards.SmartCardEmulator");
```

您可以藉由檢查 [**SmartCardEmulator.GetDefaultAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardemulator.getdefaultasync) 方法是否會傳回 Null 來進行額外檢查，以查看裝置是否具備某種形式卡片模擬功能的 NFC 硬體。 若是如此，則裝置上不支援任何 NFC 卡片模擬。

```csharp
var smartcardemulator = await SmartCardEmulator.GetDefaultAsync();<
```

只有在目前啟動的裝置上 (例如，Lumia 730、830、640 及 640 XL) 才支援以 HCE 和 AID 為基礎的 UICC 路由。 執行 Windows 10 行動裝置版和之後的任何新支援 NFC 的裝置，都應該支援 HCE。 您的應用程式可以檢查是否有 HCE 支援，如下所示。

```csharp
Smartcardemulator.IsHostCardEmulationSupported();
```

## <a name="lock-screen-and-screen-off-behavior"></a>鎖定畫面和螢幕關閉行為

Windows 10 Mobile 具有裝置層級的卡模擬設定，可由行動操作員或裝置製造商設定。 除非 MO 或 OEM 覆寫這些值，否則預設會停用 [輕觸支付] 切換，並將 [裝置層級的啟用原則] 設定為 [自動]。

您的應用程式可以在裝置層級上查詢 [**EnablementPolicy**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardEmulatorEnablementPolicy) 的值，並根據應用程式在每個狀態中所需的行為，針對每個案例採取動作。

```csharp
SmartCardEmulator emulator = await SmartCardEmulator.GetDefaultAsync();

switch (emulator.EnablementPolicy)
{
case Never:
// you can take the user to the NFC settings to turn "tap and pay" on
await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings-nfctransactions:"));
break;

 case Always:
return "Card emulation always on";

 case ScreenOn:
 return "Card emulation on only when screen is on";

 case ScreenUnlocked:
 return "Card emulation on only when screen unlocked";
}
```

即使手機已鎖定，您的應用程式背景工作還是會啟動，和/或唯有當外部讀卡機選取某個 AID 來解析您的應用程式時，螢幕才會關閉。 您可以在背景工作中回應來自讀卡機的命令，但是，如果您需要任何來自使用者的輸入，或者需要為使用者顯示訊息，則可使用相同引數來啟動前景應用程式。 您的背景工作會使用下列行為來啟動前景應用程式。

-   在裝置鎖定畫面下方 (只有在使用者解除鎖定裝置之後，才會看到您的前景應用程式)
-   在裝置鎖定畫面上方 (在使用者關閉您的應用程式之後，裝置仍會處於鎖定狀態)

```csharp
        if (Windows::Phone::System::SystemProtection::ScreenLocked)
        {
            // Launch above the lock with some arguments
            var result = await eventDetails.TryLaunchSelfAsync("app-specific arguments", SmartCardLaunchBehavior.AboveLock);
        }
```

## <a name="aid-registration-and-other-updates-for-sim-based-apps"></a>AID 登錄以及適用於以 SIM 為基礎之應用程式的其他更新

使用 SIM 卡做為安全元素的卡片模擬應用程式可向 Windows 服務進行登錄，以宣告 SIM 卡上支援的 AID。 此登錄非常類似以 HCE 為基礎的應用程式登錄。 唯一的差別是 [**SmartCardEmulationType**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardEmulationType)，您應該針對以 SIM 卡為基礎的應用程式將它設定為 Uicc。 登錄付款卡之後，也會將該卡片的顯示名稱填入 NFC 設定功能表中。

```csharp
var appletIdGroup = new SmartCardAppletIdGroup(
                        "Example DisplayName",
                                new List<IBuffer> {AID_PPSE.AsBuffer()},
                                SmartCardEmulationCategory.Payment,
                                SmartCardEmulationType.Uicc);
```

<b>重要</b>   Windows Phone 8.1 中舊版的二進位 sms 攔截支援已移除，並以 Windows 10 行動裝置版新的更廣泛 sms 支援取代，但任何依賴此版本的舊版 Windows Phone 8.1 應用程式都必須更新，才能使用新的 Windows 10 行動裝置版 SMS api。
