---
description: 使用本節的 Java 程式碼範例，深入了解如何使用 Microsoft Store 提交 API 來提交遊戲選項和預告片。
title: Java 範例 - 提交含遊戲選項與預告片的應用程式
ms.date: 07/10/2017
ms.topic: article
keywords: Windows 10, uwp, Microsoft Store 提交 API, 程式碼範例, 遊戲選項, 預告片, 進階清單, java
ms.localizationpriority: medium
ms.openlocfilehash: 93c629d509aaf3f92c3010e18077270339c7d332
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334296"
---
# <a name="java-sample-app-submission-with-game-options-and-trailers"></a>Java 範例：提交含遊戲選項與預告片的應用程式

本文提供 java 程式碼範例，示範如何使用 [Microsoft Store 提交 API](create-and-manage-submissions-using-windows-store-services.md)來執行這些任務：

* 取得 Azure AD 存取權杖以便用於 Microsoft Store 提交 API。
* 建立應用程式提交
* 設定 App 提交的 Store 清單資料，包括[遊戲](manage-app-submissions.md#gaming-options-object)和[預告片](manage-app-submissions.md#trailer-object)進階清單選項。
* 上傳包含 App 提交的套件、清單影像和預告片檔案的 ZIP 檔案。
* 認可 App 提交。

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>建立應用程式提交

`CreateAndSubmitSubmissionExample` 類別實作 `main` 程式，此程式會呼叫其他範例方法，以使用 Microsoft Store 提交 API 來建立並認可包含遊戲選項和預告片的 App 提交。 若要自行調整這個程式碼：

* 將 `tenantId` 變數指派給應用程式的租用戶識別碼，並指派 `clientId` 和 `clientSecret` 變數給應用程式的用戶端識別碼和金鑰。 如需詳細資訊，請參閱[如何與您的合作夥伴中心帳戶相關聯的 Azure AD 應用程式](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account)
* 將 `applicationId` 變數指派給您要建立提交之應用程式的[ Store 識別碼](in-app-purchases-and-trials.md#store-ids)。

> [!div class="tabbedCodeSnippets"]
[!code-java[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/java/CreateAndSubmitSubmissionExample.java#L1-L313)]

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>取得 Azure AD 存取權杖

`DevCenterAccessTokenClient` 類別定義協助程式方法，使用您的 `tenantId`、`clientId` 和 `clientSecret` 值來建立 Azure AD 存取權杖以搭配 Microsoft Store 提交 API 使用。

> [!div class="tabbedCodeSnippets"]
[!code[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/java/DevCenterAccessTokenClient.java#L1-L69)]

<span id="utilities" />

## <a name="helper-methods-to-invoke-the-submission-api-and-upload-submission-files"></a>叫用提交 API 並上傳提交檔案的協助程式方法

`DevCenterClient` 類別定義協助程式方法，叫用 Microsoft Store 提交 API 中的各種不同方法，並上傳包含 App 提交的套件、清單影像和預告片檔案的 ZIP 檔案。

> [!div class="tabbedCodeSnippets"]
[!code-java[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/java/DevCenterClient.java#L1-L224)]

## <a name="related-topics"></a>相關主題

* [建立和管理使用 Microsoft Store 服務的提交內容](create-and-manage-submissions-using-windows-store-services.md)
