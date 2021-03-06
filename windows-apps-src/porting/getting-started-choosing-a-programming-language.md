---
title: 選擇程式設計語言
ms.assetid: 6CA46432-BF03-4B20-9187-565B3503B497
description: 選擇程式設計語言
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 10673184b44f9c4c52e2ee5ad3cf4ba4a1238497
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493303"
---
# <a name="getting-started-choosing-a-programming-language"></a>開始使用：選擇程式設計語言

## <a name="choosing-a-programming-language"></a>選擇程式設計語言

在我們繼續進行之前，您必須知道開發通用 Windows 平台 (UWP) app 時可以選用的程式設計語言。 雖然本文中的逐步解說是使用 C#，但是您可以使用一或多種程式設計語言來開發 UWP app (請參閱[語言、工具及架構](https://docs.microsoft.com/previous-versions/windows/apps/dn465799(v=win.10)))。

您可以使用 C++、C#、Microsoft Visual Basic 以及 JavaScript 進行開發。 JavaScript 使用 HTML5 標記設定 UI 配置，而其他語言則使用稱為 *Extensible Application Markup Language (XAML)* 的標記語言來描述其 UI。

雖然在本文中，我們將焦點放在 C# 上，但是您可能想要探索其他語言提供的獨特優點。 例如，如果您應用程式最主要的考量是效能，特別是處理大量圖形，那麼 C++ 可能是正確的選擇。 Microsoft .NET 版本的 Visual Basic 非常適合 Visual Basic 應用程式開發人員。 JavaScript 搭配 HTML5 則適合具有網路開發背景的開發人員。 如需詳細資訊，請參閱下列各個主題：

-   [使用 c # 或 Visual Basic 建立 "Hello，World！" 應用程式](../get-started/create-a-hello-world-app-xaml-universal.md)
-   [使用 c + +/WinRT 建立 "Hello，World！" 應用程式](/windows/uwp/get-started/create-a-basic-windows-10-app-in-cppwinrt)
-   [使用 c + +/CX 建立 "Hello，World！" 應用程式](/windows/uwp/get-started/create-a-basic-windows-10-app-in-cpp)
-   [使用 JavaScript 建立 "Hello，World！" 應用程式](../get-started/create-a-hello-world-app-js-uwp.md)

**注意**   針對使用3D 圖形的應用程式，OpenGL 和 OpenGL ES 標準原本無法供 UWP 應用程式使用。 如果您不希望將您的 OpenGL ES 程式碼重新撰寫成 Microsoft DirectX，則您可能會想要了解 **Angle**。 Angle 是一個正在進行中的專案，設計目的是透過將 OpenGL API 呼叫轉譯成 DirectX API 呼叫，來將 OpenGL 轉換成 DirectX。 若要深入了解，請參閱下列文章：
-   [正切](https://bugs.chromium.org/p/angleproject/)
-   [使用 DirectX 建立您的第一個 UWP 應用程式](https://docs.microsoft.com/previous-versions/windows/apps/br229580(v=win.10))
-   [使用 DirectX 的 UWP 應用程式範例](https://docs.microsoft.com/samples/browse/?expanded=windows&products=windows-uwp&terms=directx)
-   [DirectX SDK 在哪裡？](https://docs.microsoft.com/windows/desktop/directx-sdk--august-2009-)

## <a name="giving-c-a-go"></a>試看看使用 C#

身為 iOS 開發人員，您習慣使用 Objective-C 和 Swift。 與兩者最接近的 Microsoft 程式設計語言是 C#。 對於大部分的開發人員及 app 而言，我們認為 C# 是最簡單且最快速學習和使用的語言，所以本文中的資訊及逐步解說會將焦點放在該語言。 若要深入了解 C#，請參閱下列主題：

-   [使用 c # 或 Visual Basic 建立您的第一個 UWP 應用程式](../get-started/create-a-hello-world-app-xaml-universal.md)
-   [使用 C 的 UWP 應用程式範例#](https://docs.microsoft.com/samples/browse/?expanded=windows&products=windows-uwp&languages=csharp)
-   [Visual C#](https://msdn.microsoft.com/library/kx37x362.aspx)

以下是使用 Objective-C 與 C# 撰寫的類別。 首先先顯示 Objective-C 的寫法，接著是 C# 的寫法。

```obj-c
// Objective-C header: SampleClass.h.

#import <Foundation/Foundation.h>

@interface SampleClass : NSObject {
    BOOL localVariable;
}

@property (nonatomic) BOOL localVariable;

-(int) addThis: (int) firstNumber andThis: (int) secondNumber;

@end
```

```obj-c
// Objective-C implementation.

#import "SampleClass.h"

@implementation SampleClass

@synthesize localVariable = _localVariable;

- (id)init {
    self = [super init];
    if (self) {
        localVariable = true;
    }
    return self;
}

-(int) addThis: (int) firstNumber andThis: (int) secondNumber {
    return firstNumber + secondNumber;
}

@end
```

```obj-c
// Objective-C usage.

SampleClass *mySampleClass = [[SampleClass alloc] init];
mySampleClass.localVariable = false;
int result = [mySampleClass addThis:1 andThis:2];
```

現在來看看 C# 版本。 和 Swift 一樣，您會看到標頭和實作不是位於個別檔案。

```csharp
// C# header and implementation.

using System;

namespace MyApp  // Defines this code' s scope.
{
    class SampleClass
    {
        private bool localVariable;

        public SampleClass() // Constructor.
        {
            localVariable = true;
        }

        public bool myLocalVariable // Property.
        {
            get
            {
                return localVariable;
            }
            set
            {
                localVariable = value; 
            }
        }

        public int AddTwoNumbers(int numberOne, int numberTwo)
        {
            return numberOne + numberTwo;
        }        
    }
}
```

```csharp
// C# usage.

SampleClass mySampleClass = new SampleClass();
mySampleClass.myLocalVariable = false;
int result = mySampleClass.AddTwoNumbers(1, 2);
```

C# 是個很容易學會的語言，而且內建許多構成 .NET 的支援類別和架構。 不用多少時間，您將會快快樂樂地撰寫程式碼，再也看不到註解！

## <a name="next-step"></a>後續步驟

[開始使用：使用 Visual Studio](getting-started-getting-around-in-visual-studio.md)
