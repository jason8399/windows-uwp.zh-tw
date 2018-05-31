---
author: laurenhughes
ms.assetid: 4C59D5AC-58F7-4863-A884-E9E54228A5AD
title: 列舉和查詢檔案和資料夾
description: 存取位於資料夾、媒體櫃、裝置或網路位置中的檔案和資料夾。 您也可以建構檔案和資料夾查詢，來查詢位置中的檔案和資料夾。
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 04cd59c454f0cd1561f35125f394c68bc5753002
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
ms.locfileid: "1652857"
---
# <a name="enumerate-and-query-files-and-folders"></a>列舉和查詢檔案和資料夾




存取位於資料夾、媒體櫃、裝置或網路位置中的檔案和資料夾。 您也可以建構檔案和資料夾查詢來查詢位置中的檔案和資料夾。

如需如何儲存通用 Windows 平台應用程式資料的指導方針，請參閱 [ApplicationData](https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.aspx) 類別。

> [!NOTE]
> 另請參閱[資料夾列舉範例](http://go.microsoft.com/fwlink/p/?linkid=619993)(英文)。

 
## <a name="prerequisites"></a>先決條件

-   **了解通用 Windows 平台 (UWP) App 的非同步程式設計**

    您可以參閱[在 C# 或 Visual Basic 中呼叫非同步 API](https://msdn.microsoft.com/library/windows/apps/mt187337)，以了解如何使用 C# 或 Visual Basic 撰寫非同步的 app。 若要了解如何使用 C++ 撰寫非同步的 App，請參閱 [C++ 的非同步程式設計](https://msdn.microsoft.com/library/windows/apps/mt187334)。

-   **位置的存取權限**

    例如，這些範例中的程式碼需要 **picturesLibrary** 功能，但是您的位置可能需要其他功能或完全不需要功能。 若要深入了解，請參閱[檔案存取權限](file-access-permissions.md)。

## <a name="enumerate-files-and-folders-in-a-location"></a>列舉位置中的檔案和資料夾

> [!NOTE]
> 請記得宣告 **picturesLibrary** 功能。

在這個範例中，我們會先使用 [**StorageFolder.GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br227276) 方法來取得 [**PicturesLibrary**](https://msdn.microsoft.com/library/windows/apps/br227156) 的根資料夾中 (不在子資料夾) 的所有檔案，並列出每個檔案的名稱。 接下來，我們會使用 [**GetFoldersAsync**](https://msdn.microsoft.com/library/windows/apps/br227280) 方法來取得 **PicturesLibrary** 中的所有子資料夾，並列出每個子資料夾的名稱。

<!--BUGBUG: IAsyncOperation<IVectorView<StorageFolder^>^>^  causes build to flake out-->
> [!div class="tabbedCodeSnippets"]
> ```cpp
> //#include <ppltasks.h>
> //#include <string>
> //#include <memory>
> using namespace Windows::Storage;
> using namespace Platform::Collections;
> using namespace concurrency;
> using namespace std;
>
> // Be sure to specify the Pictures Folder capability in the appxmanifext file.
> StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary;
>
> // Use a shared_ptr so that the string stays in memory
> // until the last task is complete.
> auto outputString = make_shared<wstring>();
> *outputString += L"Files:\n";
>
> // Get a read-only vector of the file objects
> // and pass it to the continuation.
> create_task(picturesFolder->GetFilesAsync())        
>    // outputString is captured by value, which creates a copy
>    // of the shared_ptr and increments its reference count.
>    .then ([outputString] (IVectorView\<StorageFile^>^ files)
>    {        
>        for ( unsigned int i = 0 ; i < files->Size; i++)
>        {
>            *outputString += files->GetAt(i)->Name->Data();
>            *outputString += L"\n";
>       }
>    })
>    // We need to explicitly state the return type
>    // here: -> IAsyncOperation<...>
>    .then([picturesFolder]() -> IAsyncOperation\<IVectorView\<StorageFolder^>^>^
>    {
>        return picturesFolder->GetFoldersAsync();
>    })
>    // Capture "this" to access m_OutputTextBlock from within the lambda.
>    .then([this, outputString](IVectorView/<StorageFolder^>^ folders)
>    {        
>        *outputString += L"Folders:\n";
>
>        for ( unsigned int i = 0; i < folders->Size; i++)
>        {
>           *outputString += folders->GetAt(i)->Name->Data();
>           *outputString += L"\n";
>        }
>
>        // Assume m_OutputTextBlock is a TextBlock defined in the XAML.
>        m_OutputTextBlock->Text = ref new String((*outputString).c_str());
>     });
> ```
> ```cs
> StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
> StringBuilder outputText = new StringBuilder();
>
> IReadOnlyList<StorageFile> fileList = await picturesFolder.GetFilesAsync();
>
> outputText.AppendLine("Files:");
> foreach (StorageFile file in fileList)
> {
>     outputText.Append(file.Name + "\n");
> }
>
> IReadOnlyList<StorageFolder> folderList = await picturesFolder.GetFoldersAsync();
>            
> outputText.AppendLine("Folders:");
> foreach (StorageFolder folder in folderList)
> {
>     outputText.Append(folder.DisplayName + "\n");
> }
> ```
> ```vb
> Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
> Dim outputText As New StringBuilder
>
> Dim fileList As IReadOnlyList(Of StorageFile) =
>     Await picturesFolder.GetFilesAsync()
>
> outputText.AppendLine("Files:")
> For Each file As StorageFile In fileList
>
>     outputText.Append(file.Name & vbLf)
>
> Next file
>
> Dim folderList As IReadOnlyList(Of StorageFolder) =
>     Await picturesFolder.GetFoldersAsync()
>
> outputText.AppendLine("Folders:")
> For Each folder As StorageFolder In folderList
>
>     outputText.Append(folder.DisplayName & vbLf)
>
> Next folder
> ```


> [!NOTE]
> 在 C# 或 Visual Basic 中，請務必在您使用 **await** 運算子的任何方法的方法宣告中放置 **async** 關鍵字。
 

或者，您可以使用 [**GetItemsAsync**](https://msdn.microsoft.com/library/windows/apps/br227286) 方法取得特定位置中的所有項目 (檔案與子資料夾)。 下列範例使用 **GetItemsAsync** 方法取得 [**PicturesLibrary**](https://msdn.microsoft.com/library/windows/apps/br227156) 的根資料夾中 (不在子資料夾) 的所有檔案與子資料夾。 接著範例會列出每個檔案或子資料夾的名稱。 如果項目是子資料夾，範例會將 `"folder"` 附加到名稱。

> [!div class="tabbedCodeSnippets"]
> ```cpp
> // See previous example for comments, namespace and #include info.
> StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary;
> auto outputString = make_shared<wstring>();
>
> create_task(picturesFolder->GetItemsAsync())        
>     .then ([this, outputString] (IVectorView<IStorageItem^>^ items)
> {        
>     for ( unsigned int i = 0 ; i < items->Size; i++)
>     {
>         *outputString += items->GetAt(i)->Name->Data();
>         if(items->GetAt(i)->IsOfType(StorageItemTypes::Folder))
>         {
>             *outputString += L"  folder\n";
>         }
>         else
>         {
>             *outputString += L"\n";
>         }
>         m_OutputTextBlock->Text = ref new String((*outputString).c_str());
>     }
> });
> ```
> ```cs
> StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
> StringBuilder outputText = new StringBuilder();
>
> IReadOnlyList<IStorageItem> itemsList = await picturesFolder.GetItemsAsync();
>
> foreach (var item in itemsList)
> {
>     if (item is StorageFolder)
>     {
>         outputText.Append(item.Name + " folder\n");
>
>     }
>     else
>     {
>         outputText.Append(item.Name + "\n");
>     }
> }
> ```
> ```vb
> Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
> Dim outputText As New StringBuilder
>
> Dim itemsList As IReadOnlyList(Of IStorageItem) =
>     Await picturesFolder.GetItemsAsync()
>
> For Each item In itemsList
>
>     If TypeOf item Is StorageFolder Then
>
>         outputText.Append(item.Name & " folder" & vbLf)
>
>     Else
>
>         outputText.Append(item.Name & vbLf)
>
>     End If
>
> Next item
> ```

## <a name="query-files-in-a-location-and-enumerate-matching-files"></a>查詢位置中的檔案並列舉相符的檔案

在這個範例中，我們會查詢依月份群組的 [**PicturesLibrary**](https://msdn.microsoft.com/library/windows/apps/br227156) 中的所有檔案，這次範例遞迴到子資料夾。 首先，我們會呼叫 [**StorageFolder.CreateFolderQuery**](https://msdn.microsoft.com/library/windows/apps/br227262) 並將 [**CommonFolderQuery.GroupByMonth**](https://msdn.microsoft.com/library/windows/apps/br207957) 值傳遞到方法。 我們會得到 [**StorageFolderQueryResult**](https://msdn.microsoft.com/library/windows/apps/br208066) 物件。

接著我們會呼叫 [**StorageFolderQueryResult.GetFoldersAsync**](https://msdn.microsoft.com/library/windows/apps/br208074)，它會傳回代表虛擬資料夾的 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) 物件。 在這個案例中，我們依月份分組，讓每個虛擬資料夾代表相同月份的檔案群組。

> [!div class="tabbedCodeSnippets"]
> ```cpp
> //#include <ppltasks.h>
> //#include <string>
> //#include <memory>
> using namespace Windows::Storage;
> using namespace Windows::Storage::Search;
> using namespace concurrency;
> using namespace Platform::Collections;
> using namespace Windows::Foundation::Collections;
> using namespace std;
>
> StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary;
>
> StorageFolderQueryResult^ queryResult =
>     picturesFolder->CreateFolderQuery(CommonFolderQuery::GroupByMonth);
>
> // Use shared_ptr so that outputString remains in memory
> // until the task completes, which is after the function goes out of scope.
> auto outputString = std::make_shared<wstring>();
>
> create_task( queryResult->GetFoldersAsync()).then([this, outputString] (IVectorView<StorageFolder^>^ view)
> {        
>     for ( unsigned int i = 0; i < view->Size; i++)
>     {
>         create_task(view->GetAt(i)->GetFilesAsync()).then([this, i, view, outputString](IVectorView<StorageFile^>^ files)
>         {
>             *outputString += view->GetAt(i)->Name->Data();
>             *outputString += L"(";
>             *outputString += to_wstring(files->Size);
>             *outputString += L")\r\n";
>             for (unsigned int j = 0; j < files->Size; j++)
>             {
>                 *outputString += L"     ";
>                 *outputString += files->GetAt(j)->Name->Data();
>                 *outputString += L"\r\n";
>             }
>         }).then([this, outputString]()
>         {
>             m_OutputTextBlock->Text = ref new String((*outputString).c_str());
>         });
>     }    
> });
> ```
> ```cs
> StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
>
> StorageFolderQueryResult queryResult =
>     picturesFolder.CreateFolderQuery(CommonFolderQuery.GroupByMonth);
>         
> IReadOnlyList<StorageFolder> folderList =
>     await queryResult.GetFoldersAsync();
>
> StringBuilder outputText = new StringBuilder();
>
> foreach (StorageFolder folder in folderList)
> {
>     IReadOnlyList<StorageFile> fileList = await folder.GetFilesAsync();
>
>     // Print the month and number of files in this group.
>     outputText.AppendLine(folder.Name + " (" + fileList.Count + ")");
>
>     foreach (StorageFile file in fileList)
>     {
>         // Print the name of the file.
>         outputText.AppendLine("   " + file.Name);
>     }
> }
> ```
> ```vb
> Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
> Dim outputText As New StringBuilder
>
> Dim queryResult As StorageFolderQueryResult =
>     picturesFolder.CreateFolderQuery(CommonFolderQuery.GroupByMonth)
>
> Dim folderList As IReadOnlyList(Of StorageFolder) =
>     Await queryResult.GetFoldersAsync()
>
> For Each folder As StorageFolder In folderList
>
>     Dim fileList As IReadOnlyList(Of StorageFile) =
>         Await folder.GetFilesAsync()
>
>     ' Print the month and number of files in this group.
>     outputText.AppendLine(folder.Name & " (" & fileList.Count & ")")
>
>     For Each file As StorageFile In fileList
>
>         ' Print the name of the file.
>         outputText.AppendLine("   " & file.Name)
>
>     Next file
>
> Next folder
> ```

範例的輸出結果看起來和下面類似。

``` syntax
July ‎2015 (2)
   MyImage3.png
   MyImage4.png
‎December ‎2014 (2)
   MyImage1.png
   MyImage2.png
```