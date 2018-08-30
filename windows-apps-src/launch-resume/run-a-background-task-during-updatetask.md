---
author: TylerMSFT
title: 在更新 UWP app 時執行背景工作
description: 了解如何在更新通用 Windows 平台 (UWP) 市集應用程式時執行背景工作。
ms.author: twhitney
ms.date: 04/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp、 更新、 背景工作、 updatetask，背景工作
ms.localizationpriority: medium
ms.openlocfilehash: fcba2cb736f86cebc6d2664e2ec3b557d47c86d7
ms.sourcegitcommit: 7efffcc715a4be26f0cf7f7e249653d8c356319b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/30/2018
ms.locfileid: "3112980"
---
# <a name="run-a-background-task-when-your-uwp-app-is-updated"></a>在更新 UWP app 時執行背景工作

了解如何撰寫背景工作會在您的通用 Windows 平台 (UWP) 市集應用程式會更新後執行。

使用者在裝置已安裝的應用程式安裝更新之後，作業系統會叫用更新工作背景工作。 這可讓您的應用程式執行例如初始化新的推播通知通道，更新資料庫結構描述，以及等等，在使用者啟動已更新的 app 之前的初始設定工作。

啟動背景工作使用[ServicingComplete](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)觸發程序，因為在此情況下您的應用程式必須至少執行一次之前都會更新一次若要註冊之背景工作進行啟用的**的不同之處更新工作ServicingComplete**觸發程序。  更新工作不登錄，因此應用程式，永遠不會執行之前，但可升級時，仍然會觸發其更新工作。

## <a name="step-1-create-the-background-task-class"></a>步驟 1： 建立背景工作類別

做為使用其他類型的背景工作，您更新工作背景工作以元件形式實作 Windows 執行階段。 若要建立這個元件，請依照步驟**建立背景工作類別**的區段中[建立及註冊跨處理序背景工作](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)。 步驟包括︰

- 將 Windows 執行階段元件專案新增至您的方案。
- 從您的應用程式中建立參考的元件。
- 在元件中建立的公用、 密封類別會實作[**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)。
- 實作[**Run**](https://msdn.microsoft.com/library/windows/apps/br224811)方法，即會執行更新工作時，會呼叫的必要的進入點。 如果您要從您的背景工作的非同步呼叫，[建立及註冊跨處理序背景工作](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)會說明如何在您**執行**的方法使用延遲。

您不需要使用 「 更新 」 工作登錄此背景工作 （「 登錄背景工作執行 」 一節中**建立及註冊跨處理序背景工作**主題）。 這是使用更新工作，因為您不需要將任何程式碼新增到您的應用程式登錄工作，而且應用程式沒有至少一次，再更新，以註冊背景工作執行的主要原因。

下列範例程式碼會顯示在 C# 中的更新工作背景工作類別的基本起點。 背景工作類別本身及背景工作專案中的所有其他類別-必須是**公用**和**密封**。 您的背景工作類別必須是衍生自**IBackgroundTask** ，並使用如下所示的簽章中有公用**run （）** 方法：

```cs
using Windows.ApplicationModel.Background;

namespace BackgroundTasks
{
    public sealed class UpdateTask : IBackgroundTask
    {
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // your app migration/update code here
        }
    }
}
```

## <a name="step-2-declare-your-background-task-in-the-package-manifest"></a>步驟 2： 宣告您的背景工作在套件資訊清單中

在 Visual Studio 方案總管中，以滑鼠右鍵按一下 [ **Package.appxmanifest** ，按一下 [**檢視程式碼**到套件資訊清單。 新增下列項目`<Extensions>`XML，若要宣告您的更新工作：

```XML
<Package ...>
    ...
  <Applications>  
    <Application ...>  
        ...
      <Extensions>  
        <Extension Category="windows.updateTask"  EntryPoint="BackgroundTasks.UpdateTask">  
        </Extension>  
      </Extensions>

    </Application>  
  </Applications>  
</Package>
```

在上述 XML 中，確認`EntryPoint`屬性設為 namespace.class 您更新工作類別的名稱。 名稱會區分大小寫。

## <a name="step-3-debugtest-your-update-task"></a>步驟 3： 偵錯/測試您的更新工作

請確定您有您的應用程式部署到您的電腦，因此並更新的項目。

在您的背景工作的 run （） 方法中設定中斷點。

![設定中斷點](images/run-func-breakpoint.png)

接下來，在 [方案總管中，以滑鼠右鍵按一下您的應用程式專案 （不背景工作專案），然後按一下 \ [**內容**。 在 [應用程式屬性] 視窗中，按一下左側的 [**偵錯**然後選取 [**不啟動，但在我的程式碼啟動時進行偵錯**：

![設定偵錯設定](images/do-not-launch-but-debug.png)

接下來，若要確保 UpdateTask 會觸發，增加套件的版本號碼。 在 [方案總管] 中按兩下以開啟封裝設計工具中，您的應用程式的**Package.appxmanifest**檔案，然後更新的**組建**編號：

![更新版本](images/bump-version.png)

現在，在 Visual Studio 2017 當您按下 F5 時，您的應用程式將會更新，系統便會啟用您 UpdateTask 的元件在背景中。 背景處理程序將會自動附加偵錯工具。 您將會取得中斷點，您可以逐步執行您的更新程式碼邏輯。

當背景工作完成時，您可以在相同的偵錯工作階段中啟動前景應用程式從 Windows [開始] 功能表。 偵錯工具將會自動再次附加，在前景處理序中，這次，您可以逐步執行您的應用程式的邏輯。

> [!NOTE]
> Visual Studio 2015 使用者： 上述步驟適用於 Visual Studio 2017。 如果您使用 Visual Studio 2015，您可以使用相同的技術來觸發程序和 UpdateTask，除了 Visual Studio 會附加到它的測試。 在 VS 2015 的替代程序是設定為其進入點，設定 UpdateTask [ApplicationTrigger](https://docs.microsoft.com/windows/uwp/launch-resume/trigger-background-task-from-app)與觸發程序直接從前景應用程式執行。

## <a name="see-also"></a>請參閱

[建立及註冊跨處理序的背景工作](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)