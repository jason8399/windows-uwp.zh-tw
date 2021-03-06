---
Description: 您可以使用 RichEditBox 控制項來輸入和編輯包含格式化文字、超連結及影像的 RTF 文件。 您可以藉由將 IsReadOnly 屬性設定成 true，使 RichEditBox 變成唯讀。
title: RichEditBox
ms.assetid: 4AFC0DFA-3B89-434D-9F86-4309CCFF7839
label: Rich edit box
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: cf2eebdc97a1052dd3f4a3e00dd3a911cb80bace
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970663"
---
# <a name="rich-edit-box"></a>Rich Edit 方塊

您可以使用 RichEditBox 控制項來輸入和編輯包含格式化文字、超連結及影像的 RTF 文件。 您可以藉由將 IsReadOnly 屬性設定成 **true**，使 RichEditBox 變成唯讀。

**取得 Windows UI 程式庫**

|  |  |
| - | - |
| ![WinUI 標誌](images/winui-logo-64x64.png) | Windows UI 程式庫 2.2 或更新版本中有這個控制項使用圓角的新範本。 如需詳細資訊，請參閱[圓角半徑](/windows/uwp/design/style/rounded-corner)。 WinUI 是 NuGet 套件，其中包含適用於 Windows 應用程式的新控制項和 UI 功能。 如需詳細資訊 (包括安裝指示)，請參閱 [Windows UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

> **平台 API**：[RichEditBox 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) \(英文\)、[Document 屬性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox.document) \(英文\)、[IsReadOnly 屬性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox.isreadonly) \(英文\)、[IsSpellCheckEnabled 屬性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox.isspellcheckenabled) \(英文\)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

使用 **RichEditBox** 來顯示和編輯文字檔案。 您不會透過您使用其他標準文字輸入方塊的方式，使用 RichEditBox 取得針對您應用程式的使用者輸入內容。 更確切地說，您會使用它來處理與應用程式分開的文字檔案。 您通常會將輸入到 RichEditBox 的文字儲存為 .rtf 檔案。
-   如果多行文字方塊的主要目的為建立唯讀文件 (例如部落格文章或電子郵件內容)，而且這些文件需要 RTF，則改為使用 [RTF 方塊](/windows/uwp/design/controls-and-patterns/rich-text-block)。
-   擷取只會消耗且不會再對使用者顯示的文字時，請使用純文字輸入控制項。
-   針對所有其他案例，請使用純文字輸入控制項。

如需如何選擇正確文字控制項的詳細資訊，請參閱[文字控制項](text-controls.md)文章。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡以<a href="xamlcontrolsgallery:/item/RichEditBox">開啟應用程式並查看 RichEditBox 的運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

此 Rich Edit 方塊已在其中開啟一個 RTF 文件。 格式和檔案的按鈕不屬於 Rich Edit 方塊的一部分，但您應該至少提供最小一組樣式按鈕，並實作其動作。

![包含已開啟文件的 RTF 方塊](images/rich-edit-box.png)

## <a name="create-a-rich-edit-box"></a>建立 Rich Edit 方塊

根據預設，RichEditBox 支援拼字檢查。 若要停用拼字檢查工具，請將 [IsSpellCheckEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox.isspellcheckenabled) 屬性設為 **false**。 如需詳細資訊，請參閱[拼字檢查的指導方針](text-controls.md)文章。

您可以使用 RichEditBox 的 [Document](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox.document) 屬性取得其內容。 RichEditBox 的內容是 [Windows.UI.Text.ITextDocument](https://docs.microsoft.com/windows/desktop/api/tom/nn-tom-itextdocument) 物件，與使用[Windows.UI.Xaml.Documents.Block](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.Block) 物件做為其內容的 RichTextBlock 控制項不同。 ITextDocument 介面提供的方法可載入文件並儲存為資料流、抓取文字範圍、取得使用中的選取項目、復原和重做變更、設定預設格式化屬性等等。

這個範例說明如何在 RichEditBox 中編輯、載入和儲存 RTF 格式 (rtf) 檔案。

```xaml
<RelativePanel Margin="20" HorizontalAlignment="Stretch">
    <RelativePanel.Resources>
        <Style TargetType="AppBarButton">
            <Setter Property="IsCompact" Value="True"/>
        </Style>
    </RelativePanel.Resources>
    <AppBarButton x:Name="openFileButton" Icon="OpenFile"
                  Click="OpenButton_Click" ToolTipService.ToolTip="Open file"/>
    <AppBarButton Icon="Save" Click="SaveButton_Click"
                  ToolTipService.ToolTip="Save file"
                  RelativePanel.RightOf="openFileButton" Margin="8,0,0,0"/>

    <AppBarButton Icon="Bold" Click="BoldButton_Click" ToolTipService.ToolTip="Bold"
                  RelativePanel.LeftOf="italicButton" Margin="0,0,8,0"/>
    <AppBarButton x:Name="italicButton" Icon="Italic" Click="ItalicButton_Click"
                  ToolTipService.ToolTip="Italic" RelativePanel.LeftOf="underlineButton" Margin="0,0,8,0"/>
    <AppBarButton x:Name="underlineButton" Icon="Underline" Click="UnderlineButton_Click"
                  ToolTipService.ToolTip="Underline" RelativePanel.AlignRightWithPanel="True"/>

    <RichEditBox x:Name="editor" Height="200" RelativePanel.Below="openFileButton"
                 RelativePanel.AlignLeftWithPanel="True" RelativePanel.AlignRightWithPanel="True"/>
</RelativePanel>
```


```csharp
private async void OpenButton_Click(object sender, RoutedEventArgs e)
{
    // Open a text file.
    Windows.Storage.Pickers.FileOpenPicker open =
        new Windows.Storage.Pickers.FileOpenPicker();
    open.SuggestedStartLocation =
        Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;
    open.FileTypeFilter.Add(".rtf");

    Windows.Storage.StorageFile file = await open.PickSingleFileAsync();

    if (file != null)
    {
        try
        {
            Windows.Storage.Streams.IRandomAccessStream randAccStream =
        await file.OpenAsync(Windows.Storage.FileAccessMode.Read);

            // Load the file into the Document property of the RichEditBox.
            editor.Document.LoadFromStream(Windows.UI.Text.TextSetOptions.FormatRtf, randAccStream);
        }
        catch (Exception)
        {
            ContentDialog errorDialog = new ContentDialog()
            {
                Title = "File open error",
                Content = "Sorry, I couldn't open the file.",
                PrimaryButtonText = "Ok"
            };

            await errorDialog.ShowAsync();
        }
    }
}

private async void SaveButton_Click(object sender, RoutedEventArgs e)
{
    Windows.Storage.Pickers.FileSavePicker savePicker = new Windows.Storage.Pickers.FileSavePicker();
    savePicker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;

    // Dropdown of file types the user can save the file as
    savePicker.FileTypeChoices.Add("Rich Text", new List<string>() { ".rtf" });

    // Default file name if the user does not type one in or select a file to replace
    savePicker.SuggestedFileName = "New Document";

    Windows.Storage.StorageFile file = await savePicker.PickSaveFileAsync();
    if (file != null)
    {
        // Prevent updates to the remote version of the file until we
        // finish making changes and call CompleteUpdatesAsync.
        Windows.Storage.CachedFileManager.DeferUpdates(file);
        // write to file
        Windows.Storage.Streams.IRandomAccessStream randAccStream =
            await file.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);

        editor.Document.SaveToStream(Windows.UI.Text.TextGetOptions.FormatRtf, randAccStream);

        // Let Windows know that we're finished changing the file so the
        // other app can update the remote version of the file.
        Windows.Storage.Provider.FileUpdateStatus status = await Windows.Storage.CachedFileManager.CompleteUpdatesAsync(file);
        if (status != Windows.Storage.Provider.FileUpdateStatus.Complete)
        {
            Windows.UI.Popups.MessageDialog errorBox =
                new Windows.UI.Popups.MessageDialog("File " + file.Name + " couldn't be saved.");
            await errorBox.ShowAsync();
        }
    }
}

private void BoldButton_Click(object sender, RoutedEventArgs e)
{
    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        Windows.UI.Text.ITextCharacterFormat charFormatting = selectedText.CharacterFormat;
        charFormatting.Bold = Windows.UI.Text.FormatEffect.Toggle;
        selectedText.CharacterFormat = charFormatting;
    }
}

private void ItalicButton_Click(object sender, RoutedEventArgs e)
{
    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        Windows.UI.Text.ITextCharacterFormat charFormatting = selectedText.CharacterFormat;
        charFormatting.Italic = Windows.UI.Text.FormatEffect.Toggle;
        selectedText.CharacterFormat = charFormatting;
    }
}

private void UnderlineButton_Click(object sender, RoutedEventArgs e)
{
    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        Windows.UI.Text.ITextCharacterFormat charFormatting = selectedText.CharacterFormat;
        if (charFormatting.Underline == Windows.UI.Text.UnderlineType.None)
        {
            charFormatting.Underline = Windows.UI.Text.UnderlineType.Single;
        }
        else {
            charFormatting.Underline = Windows.UI.Text.UnderlineType.None;
        }
        selectedText.CharacterFormat = charFormatting;
    }
}
```

## <a name="choose-the-right-keyboard-for-your-text-control"></a>選擇文字控制項的正確鍵盤

為協助使用者使用觸控式鍵盤或螢幕輸入面板 (SIP) 輸入資料，您可以設定文字控制項的輸入範圍，使其符合使用者要輸入的資料類型。 預設鍵盤配置通常適用於處理 RTF 文件。

如需如何使用輸入範圍的詳細資訊，請參閱[使用輸入範圍來變更觸控式鍵盤](https://docs.microsoft.com/windows/uwp/design/input/use-input-scope-to-change-the-touch-keyboard)。

## <a name="dos-and-donts"></a>可行與禁止事項

- 建立 RTF 方塊時，提供設定樣式按鈕並實作其動作。
- 使用與您的應用程式樣式一致的字型。
- 文字控制項的高度必須能夠容納一般輸入。
- 不要讓文字輸入控制項的高度隨著使用者輸入增加。
- 使用者只需要單行時，不要使用多行文字方塊。
- 使用純文字控制項就足夠時，不要使用 RTF 控制項。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) (英文) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

- [文字控制項](text-controls.md)
- [拼字檢查指導方針](text-controls.md)
- [新增搜尋](search.md)
- [文字輸入的指導方針](text-controls.md)
- [TextBox 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)
- [Windows.UI.Xaml.Controls PasswordBox 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)
