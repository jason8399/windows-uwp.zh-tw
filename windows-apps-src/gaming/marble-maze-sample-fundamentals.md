---
title: Marble Maze 範例基礎觀念
description: 本文件說明 Marble Maze 專案的基本特性，例如，它在 Windows 執行階段環境中如何使用 Visual C++、如何建立和建構它，以及如何建置它。
ms.assetid: 73329b29-62e3-1b36-01db-b7744ee5b4c3
ms.date: 08/22/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, 樣本, directx, 基礎
ms.localizationpriority: medium
ms.openlocfilehash: ff39abadc82cc3e0a5d0296ed499baa3b85f2714
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258491"
---
# <a name="marble-maze-sample-fundamentals"></a>Marble Maze 範例基礎觀念




本主題描述 Marble Maze 專案的基本特性&mdash;例如，它在 Windows 執行階段環境中如何使用 Visual C++、如何建立和建構它，以及如何建置它。 本主題也描述程式碼中採用的幾種慣例。

> [!NOTE]
> 與本文件對應的範例程式碼可以在 [DirectX Marble Maze 遊戲範例](https://github.com/microsoft/Windows-appsample-marble-maze)中找到。

以下是本文件所討論在規劃和開發通用 Windows 平台 (UWP) 遊戲時的一些重點。

-   在 Visual Studio 中使用**DirectX 11 應用程式（ C++通用 Windows-/cx）** 範本來建立 directx UWP 遊戲。
-   Windows 執行階段提供類別和介面，讓您以更現代的物件導向方式來開發 UWP app。
-   使用物件參考搭配 hat （^）符號來管理 Windows 執行階段變數的存留期、 [Microsoft：： WRL：： ComPtr](https://docs.microsoft.com/cpp/windows/comptr-class)來管理 COM 物件的存留期，以及[std：： shared\_ptr](https://docs.microsoft.com/cpp/standard-library/shared-ptr-class)或[std：： unique\_ptr](https://docs.microsoft.com/cpp/standard-library/unique-ptr-class)來管理所有其他堆積配置C++物件的存留期。
-   在大多數情況下，使用例外狀況處理 (而不是結果程式碼) 來處理意外的錯誤。
-   使用 [SAL 註釋](https://docs.microsoft.com/visualstudio/code-quality/using-sal-annotations-to-reduce-c-cpp-code-defects)並搭配程式碼分析工具，協助找出應用程式中的錯誤。

## <a name="creating-the-visual-studio-project"></a>建立 Visual Studio 專案


如果您已經下載並解壓縮範例，則可以在 Visual Studio 中開啟 **MarbleMaze_VS2017.sln** 檔案 (在 **C++** 資料夾中)，程式碼會隨即顯示出來。

我們建立 Marble Maze 的 Visual Studio 專案時，是以現有的專案為基礎。 不過，如果您還沒有提供 DirectX UWP 遊戲所需基本功能的現有專案，建議您根據 Visual Studio **directx 11 應用程式（通用C++Windows/cx）** 範本來建立專案，因為它會提供基本的工作3d 應用程式。 若要這樣做，請執行下列步驟：

1. 在 Visual Studio 2019 中，選取 [檔案] **> 新增 > 專案**。

2. 在 [**建立新專案**] 視窗中，選取 [ **DirectX 11 應用程式（ C++通用 Windows-/cx）** ]。 如果您沒有看到此選項，表示您可能未安裝必要元件&mdash;請參閱透過[新增或移除工作負載和元件來修改 Visual Studio 2019](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) ，以取得如何安裝其他元件的相關資訊。

![新專案](images/vs2019-marble-maze-sample-fundamentals-1.png)

3. 選取 **[下一步]** ，然後輸入**專案名稱**、要儲存之檔案的**位置**和**方案名稱**，然後選取 [**建立**]。



[ **DirectX 11 應用程式（通用 Windows- C++/cx）** ] 範本中的一個重要專案設定是 **/ZW**選項，可讓程式使用 Windows 執行階段的語言擴充功能。 當您使用 Visual Studio 範本時，這個選項預設為啟用。 請參閱[設定編譯器選項](https://docs.microsoft.com/cpp/build/reference/setting-compiler-options)，以取得如何在 Visual Studio 中設定編譯器選項的詳細資訊。

> **注意**   **/ZW**選項與 **/clr**之類的選項不相容。 如果使用 **/clr**，這表示您無法在同一個 Visual C++ 專案中，同時以 .NET Framework 與 Windows 執行階段為目標。

 

您從 Microsoft Store 取得的每個 UWP 應用程式都是以應用程式套件的形式呈現。 App 套件包含套件資訊清單，內含 App 的相關資訊。 例如，您可以指定應用程式的功能 (也就是對受保護系統資源或使用者資料的必要存取權)。 如果您認為應用程式需要特定的功能，請使用封裝資訊清單來宣告所需的功能。 資訊清單也可讓您指定專案屬性，例如支援的裝置旋轉、影像填滿和啟動顯示畫面。 您可以在您的專案中開啟 **Package.appxmanifest** 來編輯資訊清單。 如需有關 App 套件的詳細資訊，請參閱[封裝 App](https://docs.microsoft.com/windows/uwp/packaging/index)。

##  <a name="building-deploying-and-running-the-game"></a>建置、部署及執行遊戲

在 Visual Studio 頂端的下拉式功能表中，綠色播放按鈕的左側，選取您的部署組態。 建議將它設定為鎖定您裝置架構的 **\[偵錯\]** (32 位元則為 **x86**，64 位元則為 **x64**)，並設定成您的 **\[本機電腦\]** 。 您也可以在 **\[遠端電腦\]** 上測試，也可在透過 USB 連接的 **\[裝置\]** 上測試。 然後按一下綠色播放按鈕來建置並部署到您的裝置。

![偵錯; x64; 本機電腦](images/marble-maze-sample-fundamentals-2.png)

###  <a name="controlling-the-game"></a>控制遊戲

您可以使用觸控、加速計、Xbox One 控制器或滑鼠來控制 Marble Maze。

-   使用控制器的方向鍵來變更現用功能表項目。
-   使用觸控、控制器的 A 或 Start 按鍵或是滑鼠來選擇功能表項目。
-   使用觸控、加速計、左搖桿或滑鼠使迷宮傾斜。
-   使用觸控、控制器的 A 或 Start 按鍵或是滑鼠來關閉功能表，例如計分排行榜。
-   使用控制器的 Start 按鍵或鍵盤的 P 鍵來暫停或繼續遊戲。
-   使用控制器的 [返回] 按鈕或鍵盤的 Home 鍵來重新啟動遊戲。
-   當計分排行榜出現時，使用控制器的 Back 按鍵或鍵盤的 Home 鍵可清除所有分數。

##  <a name="code-conventions"></a>程式碼慣例


Windows 執行階段是一個程式設計介面，您可以使用它來建立只能在特定應用程式環境中執行的 UWP App。 這類應用程式會使用授權的函式、資料類型和裝置，並從 Microsoft Store 散發。 Windows 執行階段在最低層級包含「應用程式二進位介面」(ABI)。 ABI 是讓 Windows 執行階段 API 可供多種程式設計語言 (例如 JavaScript、.NET 語言和 Visual C++) 存取的低階二進位合約。

若要從 JavaScript 和 .NET 呼叫 Windows 執行階段 API，這些語言需要每個語言環境特有的投射。 當您從 JavaScript 或 .NET 呼叫 Windows 執行階段 API 時，就會叫用投射，而投射再呼叫基礎 ABI 函式。 雖然您可以在 C++ 中直接呼叫 ABI 函式，但 Microsoft 也為 C++ 提供投射，因為它們可讓 Windows 執行階段 API 的使用變得更為輕鬆，但不會降低效能。 Microsoft 也為 Visual C++ 提供專門支援 Windows 執行階段投射的語言擴充功能。 這些語言擴充功能有很多都類似 C++/CLI 語言的語法。 不過，原生應用程式使用此語法來以 Windows 執行階段為目標，而不是以 Common Language Runtime (CLR) 為目標。 物件參考或 ^ 修飾詞是這個新語法的重要部分，因為它能夠透過參考計數的功能來自動刪除執行階段物件。 若沒有其他元件參考 Windows 執行階段物件 (例如，離開範圍或將所有參考設為 [nullptr](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref))，執行階段就會刪除該物件，而不是呼叫 [AddRef](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-release) 和 **Release** 等方法來管理該物件的存留期。 另一個使用 Visual C++ 來建立 UWP app 的重要部分就是 **ref new** 關鍵字。 請使用 **ref new** (而不使用 **new**) 來建立計算參考次數的 Windows 執行階段物件。 如需詳細資訊，請參閱[型別系統 (C++/CX)](https://docs.microsoft.com/cpp/cppcx/type-system-c-cx)。

> [!IMPORTANT]
> 當您建立 Windows 執行階段物件或建立 Windows 執行階段元件時，您只需要使用 **^** 和 **ref new**。 當您撰寫的核心應用程式碼不使用 Windows 執行階段時，您可以使用標準 C++ 語法。

Marble Maze 使用 **^** 並搭配 **Microsoft::WRL::ComPtr** 來管理堆積配置的物件，並使記憶體流失情況降到最低。 建議您使用 ^ 來管理 Windows 執行階段變數的存留期， **ComPtr**以管理 COM 變數的存留期（例如當您使用 DirectX 時），並使用**std：： shared\_ptr**或**std：： unique\_ptr**來管理其他所有堆積配置C++物件的存留期。

 

如需 C++ UWP app 可用的語言擴充功能的詳細資訊，請參閱 [Visual C++ 語言參考 (C++/CX)](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx)。

###  <a name="error-handling"></a>錯誤處理

Marble Maze 使用例外狀況處理做為處理意外錯誤的主要方法。 遊戲程式碼習慣上使用記錄或錯誤碼 (例如 **HRESULT** 值) 來指出錯誤，但例外狀況處理有兩大優點。 首先，可讓程式碼易於閱讀和維護。 從程式碼的角度來說，例外狀況處理可更有效率地將錯誤傳播到可處理該錯誤的常式。 使用錯誤碼通常需要由每一個函式明確地傳播錯誤。 第二個優點是您可以將 Visual Studio 偵錯工具設為在例外狀況發生時中斷，以立即在錯誤的位置和上下文停止。 Windows 執行階段也廣泛使用例外狀況處理。 因此，藉由在程式碼中使用例外狀況處理，您可以將所有錯誤處理結合到一個模型中。

建議您在錯誤處理模型中採用下列慣例：

-   使用例外狀況來傳達意外錯誤。
-   不要使用例外狀況來控制程式碼的流程。
-   只攔截您可以安全處理和復原的例外狀況。 否則，不要攔截例外狀況，請讓 app 終止。
-   當您呼叫 DirectX 常式傳回 **HRESULT** 時，請使用 **DX::ThrowIfFailed** 函式。 此函式是在 [DirectXHelper.h](https://github.com/Microsoft/Windows-appsample-marble-maze/blob/master/C%2B%2B/Shared/DirectXHelper.h) 中定義。 如果提供的 **HRESULT** 是錯誤碼，**ThrowIfFailed** 會擲回例外狀況。 例如， **E\_指標**會導致**ThrowIfFailed**擲回[Platform：： NullReferenceException](https://docs.microsoft.com/cpp/cppcx/platform-nullreferenceexception-class)。

    當您使用 **ThrowIfFailed** 時，請將 DirectX 呼叫寫在單獨一行，以改善程式碼的可讀性，如下列範例所示。

    ```cpp
    // Identify the physical adapter (GPU or card) this device is running on.
    ComPtr<IDXGIAdapter> dxgiAdapter;
    DX::ThrowIfFailed(
        dxgiDevice->GetAdapter(&dxgiAdapter)
        );
    ```

-   雖然我們建議您避免使用 **HRESULT** 來處理意外錯誤，但更要避免使用例外狀況處理來控制程式碼的流程。 因此，需要控制程式碼的流程時，最好使用 **HRESULT** 傳回值。

###  <a name="sal-annotations"></a>SAL 註釋

使用 SAL 註釋並搭配程式碼分析工具，協助找出應用程式中的錯誤。

您可以使用 Microsoft 原始程式碼註釋語言 (SAL) 來註釋或描述函式如何使用參數。 SAL 註釋也可描述傳回值。 SAL 註釋可搭配 C/C++ 程式碼分析工具來尋找 C 和 C++原始程式碼中可能的瑕疵。 這個工具所報告的常見程式碼錯誤包括：緩衝區滿溢、未初始化的記憶體、Null 指標取值以及記憶體和資源流失。

考慮使用 **BasicLoader.h** 中宣告的 [BasicLoader::LoadMesh](https://github.com/Microsoft/Windows-appsample-marble-maze/blob/e62d68a85499e208d591d2caefbd9df62af86809/C%2B%2B/Shared/BasicLoader.h) 方法。 這個方法使用 `_In_` 指定 *filename* 是輸入參數 (因此只能讀取)、使用 `_Out_` 指定 *vertexBuffer* 和 *indexBuffer* 是輸出參數 (因此只能寫入)，以及使用 `_Out_opt_` 指定 *vertexCount* 和 *indexCount* 是選擇性輸出參數 (可能寫入)。 因為 *vertexCount* 和 *indexCount* 是選擇性輸出參數，所以允許為 **nullptr**。 C/C++ 程式碼分析工具會檢查這個方法的呼叫，以確定傳遞的參數符合這些準則。

```cpp
void LoadMesh(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    );
```

若要對 App 執行程式碼分析，請在功能表列上選擇 **\[建置\] > \[針對方案執行程式碼分析\]** 。 如需程式碼分析的詳細資訊，請參閱[使用程式碼分析進行 C/C++ 程式碼品質分析](https://docs.microsoft.com/visualstudio/code-quality/analyzing-c-cpp-code-quality-by-using-code-analysis)。

可用註釋的完整清單定義在 sal.h 中。 如需詳細資訊，請參閱 [SAL 註釋](https://docs.microsoft.com/cpp/c-runtime-library/sal-annotations)。

## <a name="next-steps"></a>後續步驟


如需如何建構 Marble Maze 應用程式碼的詳細資訊，以及 DirectX UWP app 的結構與傳統型應用程式有何不同，請參閱 [Marble Maze 應用程式結構](marble-maze-application-structure.md)。

## <a name="related-topics"></a>相關主題


* [大理石迷宮應用程式結構](marble-maze-application-structure.md)
* [開發大理石迷宮，和 DirectX 中C++的 UWP 遊戲](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 




