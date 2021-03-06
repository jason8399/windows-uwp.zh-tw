---
ms.assetid: E1943DCE-833F-48AE-8402-CD48765B24FC
title: 最佳化暫停/繼續
description: 建立將處理程序生命週期系統更有效利用以在暫停或終止之後有效率地繼續執行的「通用 Windows 平台」(UWP) 應用程式。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 610b6237071c9d7435ca167c1a89b4ef7c40b333
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "71339570"
---
# <a name="optimize-suspendresume"></a>最佳化暫停/繼續


建立將處理程序生命週期系統更有效利用以在暫停或終止之後有效率地繼續執行的「通用 Windows 平台」(UWP) 應用程式。

## <a name="launch"></a>啟動

在暫停/終止之後重新啟用 App 時，請檢查看看是否已經過很長的時間。 如果是這樣，請考慮回到 App 的主登陸頁面，而不是顯示使用者的失效資料。 這樣也可以讓啟動變得更快。

在啟用期間，請一律檢查事件引數參數的 PreviousExecutionState (例如針對已啟動的啟用，請檢查 LaunchActivatedEventArgs.PreviousExecutionState)。 如果值是 ClosedByUser 或 NotRunning，請勿浪費時間還原先前儲存的狀態。 在此情況下，正確的做法是提供全新體驗，這將會讓啟動變得更快。

請勿急著還原先前儲存的狀態，而是考慮記錄該狀態，只在需要時才將它還原。 例如，假設您的 App 先前被暫停、儲存了 3 個頁面的狀態，然後被終止。 在重新啟動時，如果您決定讓使用者回到第 3 頁，就請勿急著還原前 2 個頁面的狀態。 取而代之的是，保留這個狀態，而只在您了解需要它時才使用它。

## <a name="while-running"></a>執行時

最佳做法是不要等待暫停事件而又保留大量狀態。 取而代之的是，您的應用程式應該在其執行時，以遞增方式保留較少量的狀態。 對於在暫停期間可能沒有足夠時間來嘗試立即儲存所有項目的大型 App 來說，這特別重要。

不過，您需要在 App 執行時的增量儲存與效能之間，尋找一個良好的平衡點。 一個理想的取捨方式是以增量方式記錄已經變更 (因此需要儲存) 的資料，然後使用暫停事件來實際儲存該資料 (比儲存所有資料或檢查 App 的整個狀態來決定要儲存哪些資料還要快)。

請勿使用視窗 Activated 或 VisibilityChanged 事件來決定何時儲存狀態。 當使用者切換離開您的 App 時，視窗會停用，但系統會先稍候一段時間 (約 10 秒)，然後才會暫停 App。 這是為了在使用者快速切換回 App 時，提供較快的回應體驗。 執行暫停邏輯之前，請先等候暫停事件。

## <a name="suspend"></a>暫停

在暫停期間，請減少您 App 的磁碟使用量。 如果您的 App 在暫停時使用較少的記憶體，整體系統的回應速度將會較快，並且將終止的暫停 App (包括您的 App) 也會較少。 不過，請在這項技巧與敏捷的繼續執行需求之間取得平衡：不要因為過度減少磁碟使用量，而導致在 App 將大量資料重新載入到記憶體中時，大幅拖慢繼續執行的速度。

針對受管理的 App，系統將會在 App 的暫停處理常式完成之後，執行記憶體回收階段。 請務必透過釋出可在 App 暫停時協助減少其磁碟使用量的物件參考，善加利用這項功能。

在理想的情況下，您的 App 會在 1 秒內隨著暫停邏輯結束。 暫停的速度越快越好，這會讓系統的其他 App 和部分能夠提供較敏捷的使用者體驗。 如有必要，您的暫停邏輯在傳統型裝置上最多可花費 5 秒鐘的時間，或在行動裝置上最多可花費 10 秒鐘的時間。 如果超過這些時間，您的 App 將會突然終止。 您不會希望發生這種情況，因為如果發生這種情況，當使用者切換回您的 App 時，將會啟動新的處理程序，而這與繼續執行暫停的 App 相比，在體驗上將會感覺速度變慢許多。

## <a name="resume"></a>繼續

大多數 App 在繼續執行時並不需要執行任何特別的動作，因此您將不會處理此事件。 有些 Apps 會使用繼續執行來還原在暫停期間關閉的連線，或是重新整理可能已失效的資料。 請將您的 App 設計成依需求起始這些活動，而不是急著執行這類工作。 這可在使用者切換回暫停的 App 時，提供較快的體驗，並確保您只執行使用者真正需要的工作。

## <a name="avoid-unnecessary-termination"></a>避免不必要的終止

UWP 處理程序生命週期系統可能會因為許多原因而暫停或終止 app。 這個處理程序的設計目的，是要讓 app 快速回到暫停或終止前的狀態。 最好的結果是使用者不會察覺 app 曾經停止執行過。 您的 UWP app 可利用下列技巧，協助系統簡化 app 生命週期中的轉換。

當使用者將 app 移到背景或是當系統進入低電源狀態時，可以暫停 app。 暫停應用程式時，它會引發暫停事件並且有多達 5 秒的時間可儲存其資料。 如果應用程式的暫停事件處理常式未在 5 秒鐘內完成，系統會假設應用程式已停止回應並終止它。 當使用者切換到終止的應用程式時，該應用程式必須再度經歷長時間的啟動處理程序，而無法立即載入到記憶體中。

### <a name="serialize-only-when-necessary"></a>只在必要時進行序列化

許多應用程式會在暫停時序列化它們的所有資料。 不過，如果只需要儲存少量的應用程式設定資料，應該使用 [**LocalSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings) 存放區，而非序列化資料。 請針對大量資料和非設定資料使用序列化。

當您序列化資料時，如果資料沒有變更，應該避免重新序列化。 這不但會使應用程式花費額外的時間序列化和儲存資料，而且再次啟用應用程式時，還需要花費額外的時間讀取和還原序列化資料。 相反的，我們建議應用程式判斷其狀態是否真的改變，若是如此，則只在變更的資料進行序列化和還原序列化。 確保執行這項工作的最佳方法是在資料變更後定期於背景序列化資料。 當您使用這項技巧時，需在暫停時序列化的所有資料早已儲存，因此應用程式不需要執行任何工作就能快速暫停。

### <a name="serializing-data-in-c-and-visual-basic"></a>在 C# 與 Visual Basic 中序列化資料

.NET 應用程式可用的序列化技術選項是 [**System.Xml.Serialization.XmlSerializer**](https://docs.microsoft.com/dotnet/api/system.xml.serialization.xmlserializer)、[**System.Runtime.Serialization.DataContractSerializer**](https://docs.microsoft.com/dotnet/api/system.runtime.serialization.datacontractserializer) 以及 [**System.Runtime.Serialization.Json.DataContractJsonSerializer**](https://docs.microsoft.com/dotnet/api/system.runtime.serialization.json.datacontractjsonserializer) 類別。

從效能的觀點來看，我們建議您使用 [**XmlSerializer**](https://docs.microsoft.com/dotnet/api/system.xml.serialization.xmlserializer) 類別。 **XmlSerializer** 有最低的序列化和還原序列化時間，並可維持最低的磁碟使用量。 **XmlSerializer** 對 .NET Framework 的依賴性較少；這表示相較於其他序列化技術，使用 **XmlSerializer** 時需要載入 app 的模組較少。

[**DataContractSerializer**](https://docs.microsoft.com/dotnet/api/system.runtime.serialization.datacontractserializer) 讓序列化自訂類別的工作變得更容易，不過對於效能的影響比 **XmlSerializer** 還大。 如果您需要較佳的效能，請考慮調換使用。 一般而言，您不應該載入一個以上的序列化程式，而且應該優先使用 **XmlSerializer**，除非需要其他序列化程式的功能。

### <a name="reduce-memory-footprint"></a>減少記憶體使用量

系統會嘗試在記憶體中盡可能保留最多暫停的應用程式，使用者才能在應用程式之間快速且可靠地切換。 在系統的記憶體中暫停和保留應用程式時，可以將它快速地放置到前景，以利使用者與其互動，而不必顯示啟動顯示畫面或是執行冗長的載入操作。 如果沒有足夠的資源將應用程式保留在記憶體中，就會終止應用程式。 基於下列兩個理由，記憶體管理非常重要：

-   在暫停時盡可能釋放最多的記憶體，可以將應用程式因為暫停時資源不足而終止的機率降至最低。
-   減少應用程式使用的整體磁碟使用量，就能降低其他應用程式在暫停時被終止的機率。

### <a name="release-resources"></a>釋放資源

某些物件 (例如檔案與裝置) 會佔據大量的記憶體。 建議在暫停期間，應用程式應釋放這些物件的控制代碼，並在需要時重新建立控制代碼。 這個時候也適合清除在應用程式繼續時將不再有效的任何快取。 XAML 架構在必要時會代替您為 C# 與 Visual Basic 應用程式執行的一個額外步驟，就是記憶體回收。 這樣可確保系統釋放應用程式碼中不會繼續被參考的所有物件。

## <a name="resume-quickly"></a>快速繼續

當使用者將應用程式移到前景或是當系統離開低電源狀態時，就可以繼續暫停的應用程式。 當應用程式從暫停的狀態繼續時，它會從暫停它時的位置繼續。 因為應用程式資料是儲存在記憶體中，所以即使應用程式暫停了很長一段時間也不會遺失。

大部分的應用程式都不需要處理 [**Resuming**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.resuming) 事件。 繼續 app 時，變數與物件的狀態會與暫停 app 時所具有的狀態完全相同。 只有在您需要為暫停 app 與繼續 app 之間的這段時間內可能已經變更的資料或物件進行更新時，才處理 **Resuming** 事件，例如：內容 (如更新摘要資料)、網路連線，或是如果您需要重新取得裝置的存取 (如網路攝影機)。

## <a name="related-topics"></a>相關主題

* [應用程式暫停和繼續執行的指導方針](https://docs.microsoft.com/windows/uwp/launch-resume/index)
 

 




