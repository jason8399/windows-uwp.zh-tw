---
title: 在 Windows 上進行 Android 開發的 PWA 方法
description: 開始使用 Windows 上的 PWA 方法來開發 Android 應用程式。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: windows 上的 android，pwa，android，cordova，ionic，phonegap，混合式 web 應用程式
ms.date: 04/28/2020
ms.openlocfilehash: c0ff9acf1d8e93e82f1db424d7a356c974988683
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255211"
---
# <a name="get-started-developing-a-pwa-or-hybrid-web-app-for-android"></a>開始開發 PWA 或適用于 Android 的混合式 web 應用程式

本指南將協助您開始在 Windows 上使用可用於 web 和跨裝置平臺（Android、iOS、Windows）的單一 HTML/CSS/JavaScript 程式碼基底，建立混合式 web 應用程式或漸進式 Web 應用程式（PWA）。

藉由使用正確的架構和元件，web 應用程式可以在 Android 裝置上運作，這種方式與原生應用程式非常類似。

## <a name="features-of-a-pwa-or-hybrid-web-app"></a>PWA 或混合式 web 應用程式的功能

有兩種主要類型的 web 應用程式可以安裝在 Android 裝置上。 主要差異在於您的應用程式程式碼是內嵌在應用程式套件（混合式）中，或裝載在網頁伺服器（pwa）上。

- **混合式 web 應用程式**：程式碼（HTML、JS、CSS）會封裝在 APK 中，並可透過 Google Play 商店散發。 查看引擎與使用者的網際網路瀏覽器隔離，沒有會話或快取共用。

- **漸進式 Web Apps （pwa）**：程式碼（HTML、JS、CSS）存在於網路上，不需要封裝為 APK。 系統會視需要使用服務工作者來下載和更新資源。 Chrome 瀏覽器會轉譯並顯示您的應用程式，但看起來會像原生，而不會包含一般的瀏覽器網址列等等。您可以使用瀏覽器來共用儲存體、快取和會話。 這基本上就像是在特殊模式中安裝 Chrome 瀏覽器的快捷方式。 Pwa 也可以使用受信任的 Web 活動列在 Google Play 商店中。

Pwa 和混合式 web 應用程式非常類似于原生 Android 應用程式，因為它們：

- 可以透過 App Store 安裝（Google Play 商店和/或 Microsoft Store）
- 具有原生裝置功能（例如相機、GPS、藍牙、通知及連絡人清單）的存取權
- 離線工作（沒有網際網路連線）

Pwa 也有幾個獨特的功能：

- 可以直接從 web 安裝在 Android 主畫面上（不含 App Store）
- 也可以[使用受信任的 Web 活動](https://css-tricks.com/how-to-get-a-progressive-web-app-into-the-google-play-store/)，透過 Google Play 商店另外安裝
- 可以透過 web 搜尋探索或透過 URL 連結進行共用
- 依賴服務背景[工作](https://developers.google.com/web/fundamentals/primers/service-workers)，以避免需要封裝原生程式碼

您不需要建立混合式應用程式或 PWA 的架構，但這份指南中會涵蓋一些常用的架構，包括 PhoneGap （含 Cordova）和 Ionic （使用「角度」或「回應」的 Cordova 或電容器）。

## <a name="apache-cordova"></a>Apache Cordova

[Apache Cordova](https://cordova.apache.org/)是一個開放原始碼架構，可以使用[外掛程式](https://cordova.apache.org/plugins/?platforms=cordova-android)來簡化您的 JavaScript 程式碼在原生[Web 視圖](https://developer.android.com/reference/android/webkit/WebView)和原生 Android 平臺之間的通訊。 這些外掛程式會公開可從您的程式碼呼叫的 JavaScript 端點，並用於呼叫原生 Android 裝置 Api。 一些範例 Cordova 外掛程式包括存取裝置服務，例如電池狀態、檔案存取、震動/環形音調等等。這些功能通常不會供 web 應用程式或瀏覽器使用。

有兩個常用的 Cordova 散發套件：

- [PhoneGap](https://phonegap.com/)

- [Ionic](https://ionicframework.com/)

## <a name="adobe-phonegap"></a>Adobe PhoneGap

[PhoneGap](https://phonegap.com/)： Adobe 支援 Cordova 的架構，它支援包含其他工具的 Cordova，例如[命令列](http://docs.phonegap.com/getting-started/1-install-phonegap/cli/)、傳統型[應用程式](https://phonegap.com/products#desktop-app-section)和[PhoneGap 組建](https://build.phonegap.com/)，這項服務可讓您將程式碼上傳至 adobe server，以建立原生應用程式，而不需要在本機電腦上安裝原生的 sdk。 這可讓您執行一些動作，例如使用您的 Windows 電腦建立 iOS 應用程式。

### <a name="install-phonegap"></a>安裝 PhoneGap

若要開始使用 PhoneGap 建立 PWA 或混合式 web 應用程式，您應該先安裝下列工具：

- 用於與 Ionic 生態系統互動的 node.js。 [下載 NodeJS For windows](https://nodejs.org/en/) ，或遵循使用適用于 Linux 的 Windows 子系統的[NodeJS 安裝指南](https://docs.microsoft.com/windows/nodejs/setup-on-wsl2)（WSL）。 如果您將使用多個專案和版本的 NodeJS，您可能會想要考慮使用[Node 版本管理員（nvm）](https://docs.microsoft.com/windows/nodejs/setup-on-wsl2#install-nvm-nodejs-and-npm) 。

在命令列中輸入下列命令，以安裝 PhoneGap：

```bash
npm install -g phonegap
```

若要建立新的 PhoneGap 專案，請遵循其步驟以[開始](https://phonegap.com/getstarted/)使用。 流覽 PhoneGap 檔的[PWA 功能](http://stage.docs.phonegap.com/tutorials/stockpile/911-pwa-features/)一節，瞭解如何將您的應用程式從混合式移至 PWA。  

## <a name="ionic"></a>Ionic

[Ionic](https://ionicframework.com/)是一種架構，可調整應用程式的使用者介面（UI），以符合每個平臺（Android、IOS、Windows）的設計語言。 Ionic 可讓您使用[角度](https://ionicframework.com/docs/developer-resources/guides/first-app-v4/intro)或[反應](https://ionicframework.com/react)。

> [!NOTE]
> 有新版本的 Ionic 會使用 Cordova 的替代方法，稱為[電容器](https://capacitor.ionicframework.com/)。 這種替代方式會使用容器，讓您的應用程式[更容易 web](https://ionicframework.com/blog/announcing-capacitor-1-0/)。

### <a name="get-started-with-ionic-by-installing-required-tools"></a>藉由安裝必要的工具來開始使用 Ionic

若要開始使用 Ionic 建立 PWA 或混合式 web 應用程式，您應該先安裝下列工具：

- 用於與 Ionic 生態系統互動的 node.js。 [下載 NodeJS For windows](https://nodejs.org/en/) ，或遵循使用適用于 Linux 的 Windows 子系統的[NodeJS 安裝指南](https://docs.microsoft.com/windows/nodejs/setup-on-wsl2)（WSL）。 如果您將使用多個專案和版本的 NodeJS，您可能會想要考慮使用[Node 版本管理員（nvm）](https://docs.microsoft.com/windows/nodejs/setup-on-wsl2#install-nvm-nodejs-and-npm) 。

- 撰寫程式碼的 VS Code。 [下載適用于 Windows 的 VS Code](https://code.visualstudio.com/)。 如果您想要使用 Linux 命令列來建立應用程式，您可能也會想要安裝[WSL 遠端延伸](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)模組。

- 使用您慣用的命令列介面（CLI）的 Windows 終端機。 [從 Microsoft Store 安裝 Windows 終端](https://www.microsoft.com/en-us/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab)機。

- 適用于版本控制的 Git。 [下載 Git](https://git-scm.com/downloads)。

## <a name="create-a-new-project-with-ionic-cordova-and-angular"></a>建立具有 Ionic Cordova 和角度的新專案

在命令列中輸入下列命令，以安裝 Ionic 和 Cordova：

```bash
npm install -g @ionic/cli cordova
```

藉由輸入命令，使用「索引標籤」應用程式範本來建立 Ionic 角度應用程式：

```bash
ionic start photo-gallery tabs
```

變更為應用程式資料夾：

```bash
cd photo-gallery
```

在您的網頁瀏覽器中執行應用程式：

```bash
ionic serve
```

如需詳細資訊，請參閱[Ionic Cordova 角度](https://ionicframework.com/docs/developer-resources/guides/first-app-v4/intro)檔。請流覽 Ionic 檔中的將[您的角度應用程式設為 pwa](https://ionicframework.com/docs/angular/pwa)一節，以瞭解如何將您的應用程式從混合式移至 pwa。

## <a name="create-a-new-project-with-ionic-capacitor-and-angular"></a>建立具有 Ionic 電容器和角度的新專案

在命令列中輸入下列命令，以安裝 Ionic 和 Cordova-Res：

```bash
npm install -g @ionic/cli native-run cordova-res
```

使用「索引標籤」應用程式範本建立 Ionic 角度應用程式，並輸入命令來新增電容器：

```bash
ionic start photo-gallery tabs --type=angular --capacitor
```

變更為應用程式資料夾：

```bash
cd photo-gallery
```

新增元件以將應用程式設為 PWA：

```bash
npm install @ionic/pwa-elements
```

將@ionic/pwa-elements下列內容新增至檔案， `src/main.ts`以進行匯入：

```typescript
import { defineCustomElements } from '@ionic/pwa-elements/loader';

// Call the element loader after the platform has been bootstrapped
defineCustomElements(window);
```

在您的網頁瀏覽器中執行應用程式：

```bash
ionic serve
```

如需詳細資訊，請參閱[Ionic 電容器角度](https://ionicframework.com/docs/angular/your-first-app)檔。請流覽 Ionic 檔中的將[您的角度應用程式設為 pwa](https://ionicframework.com/docs/angular/pwa)一節，以瞭解如何將您的應用程式從混合式移至 pwa。  

## <a name="create-a-new-project-with-ionic-and-react"></a>使用 Ionic 和回應建立新的專案

在命令列中輸入下列命令，以安裝 Ionic CLI：

```bash
npm install -g @ionic/cli
```

藉由輸入命令來建立新的專案，並做出反應：

```bash
ionic start myApp blank --type=react
```

變更為應用程式資料夾：

```bash
cd myApp
```

在您的網頁瀏覽器中執行應用程式：

```bash
ionic serve
```

如需詳細資訊，請參閱[Ionic 回應](https://ionicframework.com/docs/react/quickstart)檔。請流覽 Ionic 檔中的將[您的回應應用程式設為 pwa](https://ionicframework.com/docs/react/pwa)一節，以瞭解如何將您的應用程式從混合式移至 pwa。

## <a name="test-your-ionic-app-on-a-device-or-emulator"></a>在裝置或模擬器上測試您的 Ionic 應用程式

若要在 Android 裝置上測試您的 Ionic 應用程式，請插入您的裝置（[請確定它已先啟用開發](emulator.md#enable-your-device-for-development)），然後在命令列中輸入：

```bash
ionic cordova run android
```

若要在 Android 裝置模擬器上測試您的 Ionic 應用程式，您必須：

1. [安裝必要元件--JAVA 開發工具組（JDK）、Gradle 和 Android SDK](https://cordova.apache.org/docs/en/latest/guide/platforms/android/#installing-the-requirements)。

2. [建立 Android 虛擬裝置（AVD）](https://developer.android.com/studio/run/managing-avds.html)。

3. 輸入 Ionic 的命令，以建立您的應用程式並將其部署`ionic cordova emulate [<platform>] [options]`至模擬器：。 在此情況下，命令應該是：

```bash
ionic cordova emulate android --list
```

如需詳細資訊，請參閱 Ionic 檔中的[Cordova 模擬器](https://ionicframework.com/docs/cli/commands/cordova-emulate)。

## <a name="additional-resources"></a>其他資源

- [開發適用于 Android 的雙畫面應用程式，並取得 Surface 雙核裝置 SDK](https://docs.microsoft.com/dual-screen/android/)

- [新增 Windows Defender 排除專案以改善效能](defender-settings.md)

- [啟用虛擬化支援以改善模擬器效能](emulator.md#enable-virtualization-support)
