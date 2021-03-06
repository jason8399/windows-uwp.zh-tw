---
title: 移植著色器物件
description: 從 OpenGL ES 2.0 移植簡單的轉譯器時，第一個步驟是在 Direct3D 11 中設定對等的頂點和片段著色器物件，以及確定主程式可以在著色器物件編譯完成之後與這些物件通訊。
ms.assetid: 0383b774-bc1b-910e-8eb6-cc969b3dcc08
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, games, port, shader, direct3d, opengl, 遊戲, 連接埠, 著色器, 移植
ms.localizationpriority: medium
ms.openlocfilehash: b800a32149011376e1d97e0da44d32c733ddfb93
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368229"
---
# <a name="port-the-shader-objects"></a>移植著色器物件




**重要的 Api**

-   [**ID3D11Device**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11device)
-   [**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)

從 OpenGL ES 2.0 移植簡單的轉譯器時，第一個步驟是在 Direct3D 11 中設定對等的頂點和片段著色器物件，以及確定主程式可以在著色器物件編譯完成之後與這些物件通訊。

> **附註**  您建立新的 Direct3D 專案嗎？ 如果沒有，請依照[建立適用於通用 Windows 平台 (UWP) 的新 DirectX 11 專案](user-interface.md)中的指示執行。 這個逐步解說假設您已建立可用來繪製到螢幕的 DXGI 與 Direct3D 資源，而這些是在範本中提供。

 

與 OpenGL ES 2.0 非常類似，在 Direct3D 中編譯好的著色器必須和繪圖內容產生關聯。 但是，Direct3D 本身並未提供著色器程式物件的概念；而是必須由您直接將著色器指派給 [**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)。 這個步驟遵循可用來建立和繫結著色器物件的 OpenGL ES 2.0 程序，並為您提供 Direct3D 中相對應的 API 行為。

<a name="instructions"></a>指示
------------

### <a name="step-1-compile-the-shaders"></a>步驟 1：編譯著色器

在這個簡單的 OpenGL ES 2.0 範例中，著色器會以文字檔形式儲存，並當成字串資料載入，以進行執行階段編譯。

OpenGL ES 2.0:編譯著色器

``` syntax
GLuint __cdecl CompileShader (GLenum shaderType, const char *shaderSrcStr)
// shaderType can be GL_VERTEX_SHADER or GL_FRAGMENT_SHADER. Returns 0 if compilation fails.
{
  GLuint shaderHandle;
  GLint compiledShaderHandle;
   
  // Create an empty shader object.
  shaderHandle = glCreateShader(shaderType);

  if (shaderHandle == 0)
  return 0;

  // Load the GLSL shader source as a string value. You could obtain it from
  // from reading a text file or hardcoded.
  glShaderSource(shaderHandle, 1, &shaderSrcStr, NULL);
   
  // Compile the shader.
  glCompileShader(shaderHandle);

  // Check the compile status
  glGetShaderiv(shaderHandle, GL_COMPILE_STATUS, &compiledShaderHandle);

  if (!compiledShaderHandle) // error in compilation occurred
  {
    // Handle any errors here.
              
    glDeleteShader(shaderHandle);
    return 0;
  }

  return shaderHandle;

}
```

在 Direct3D 中，著色器不會在執行階段期間編譯；它們一律是在編譯程式的剩餘部分時編譯成 CSO 檔案。 當您使用 Microsoft Visual Studio 編譯應用程式時，HLSL 檔案會編譯成應用程式必須載入的 CSO (.cso) 檔案。 封裝應用程式時，請務必將這些 CSO 檔案包含在內！

> **附註**  下列範例會載入著色器並以非同步方式使用編譯**自動**關鍵字和 lambda 語法。 ReadDataAsync() 是針對 CSO 檔案中讀取來做為位元組資料陣列 (fileData) 的範本所實作的方法。

 

Direct3D 11:編譯著色器

``` syntax
auto loadVSTask = DX::ReadDataAsync(m_projectDir + "SimpleVertexShader.cso");
auto loadPSTask = DX::ReadDataAsync(m_projectDir + "SimplePixelShader.cso");

auto createVSTask = loadVSTask.then([this](Platform::Array<byte>^ fileData) {

m_d3dDevice->CreateVertexShader(
  fileData->Data,
  fileData->Length,
  nullptr,
  &m_vertexShader);

auto createPSTask = loadPSTask.then([this](Platform::Array<byte>^ fileData) {
  m_d3dDevice->CreatePixelShader(
    fileData->Data,
    fileData->Length,
    nullptr,
    &m_pixelShader;
};
```

### <a name="step-2-create-and-load-the-vertex-and-fragment-pixel-shaders"></a>步驟 2：建立和載入頂點，以及片段 （像素） 著色器

OpenGL ES 2.0 具備著色器「程式」的概念，而這是做為在 CPU 上執行的主程式與在 GPU 上執行的著色器之間的介面。 著色器會經過編譯 (或從編譯過的來源載入)，並與在 GPU 上啟用執行的程式產生關聯。

OpenGL ES 2.0:頂點和片段著色器載入陰影程式

``` syntax
GLuint __cdecl LoadShaderProgram (const char *vertShaderSrcStr, const char *fragShaderSrcStr)
{
  GLuint programObject, vertexShaderHandle, fragmentShaderHandle;
  GLint linkStatusCode;

  // Load the vertex shader and compile it to an internal executable format.
  vertexShaderHandle = CompileShader(GL_VERTEX_SHADER, vertShaderSrcStr);
  if (vertexShaderHandle == 0)
  {
    glDeleteShader(vertexShaderHandle);
    return 0;
  }

   // Load the fragment/pixel shader and compile it to an internal executable format.
  fragmentShaderHandle = CompileShader(GL_FRAGMENT_SHADER, fragShaderSrcStr);
  if (fragmentShaderHandle == 0)
  {
    glDeleteShader(fragmentShaderHandle);
    return 0;
  }

  // Create the program object proper.
  programObject = glCreateProgram();
   
  if (programObject == 0)    return 0;

  // Attach the compiled shaders
  glAttachShader(programObject, vertexShaderHandle);
  glAttachShader(programObject, fragmentShaderHandle);

  // Compile the shaders into binary executables in memory and link them to the program object..
  glLinkProgram(programObject);

  // Check the project object link status and determine if the program is available.
  glGetProgramiv(programObject, GL_LINK_STATUS, &linkStatusCode);

  if (!linkStatusCode) // if link status <> 0
  {
    // Linking failed; delete the program object and return a failure code (0).

    glDeleteProgram (programObject);
    return 0;
  }

  // Deallocate the unused shader resources. The actual executables are part of the program object.
  glDeleteShader(vertexShaderHandle);
  glDeleteShader(fragmentShaderHandle);

  return programObject;
}

// ...

glUseProgram(renderer->programObject);
```

Direct3D 沒有著色器程式物件的概念； 而是會在呼叫 [**ID3D11Device**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11device) 介面 (例如 [**ID3D11Device::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) 或 [**ID3D11Device::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader)) 上的其中一個著色器建立方法時建立著色器。 為了針對目前繪製內容設定著色器，我們使用一組著色器方法，提供著色器給對應的 [**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)，例如，適用於頂點著色器的 [**ID3D11DeviceContext::VSSetShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetshader)，或是適用於片段著色器的 [**ID3D11DeviceContext::PSSetShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader)。

Direct3D 11:設定著色器圖形裝置的繪圖內容。

``` syntax
m_d3dContext->VSSetShader(
  m_vertexShader.Get(),
  nullptr,
  0);

m_d3dContext->PSSetShader(
  m_pixelShader.Get(),
  nullptr,
  0);
```

### <a name="step-3-define-the-data-to-supply-to-the-shaders"></a>步驟 3：定義提供給著色器資料

我們在 OpenGL ES 2.0 範例中提供了一個 **uniform**，以針對著色器管線進行宣告：

-   **u\_mvpMatrix**： 表示採用此模型的最終模型-檢視-投影轉換矩陣的 4x4 陣列的浮點數協調 cube，並將其轉換成 2D 投影座標進行掃描轉換。

此外，有兩個適用於頂點資料的 **attribute** 值：

-   **\_位置**: 4 浮點向量中供模型座標的頂點。
-   **\_色彩**:4 浮點向量中的供 RGBA 色彩與頂點相關聯的值。

Open GL ES 2.0:GLSL uniforms 和屬性的定義

``` syntax
uniform mat4 u_mvpMatrix;
attribute vec4 a_position;
attribute vec4 a_color;
```

在這個案例中，對應的主程式變數會定義為轉譯器物件上的欄位 (請參閱中的標頭[如何： 連接埠簡單的 OpenGL ES 2.0 轉譯器為 Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)。)一旦我們所做的我們需要在其中主要的程式會提供這些值，通常我們之前繪製呼叫的將著色器 」 管線的記憶體中指定的位置：

OpenGL ES 2.0:標示的統一和屬性資料的位置

``` syntax

// Inform the shader of the attribute locations
loc = glGetAttribLocation(renderer->programObject, "a_position");
glVertexAttribPointer(loc, 3, GL_FLOAT, GL_FALSE, 
    sizeof(Vertex), 0);
glEnableVertexAttribArray(loc);

loc = glGetAttribLocation(renderer->programObject, "a_color");
glVertexAttribPointer(loc, 4, GL_FLOAT, GL_FALSE, 
    sizeof(Vertex), (GLvoid*) (sizeof(float) * 3));
glEnableVertexAttribArray(loc);


// Inform the shader program of the uniform location
renderer->mvpLoc = glGetUniformLocation(renderer->programObject, "u_mvpMatrix");
```

Direct3D 沒有相同意義的「屬性」或 "uniform" 概念 (或者至少它不會共用此語法)； 而是提供以 Direct3D 子資源表示的常數緩衝區 -- 在主程式與著色器程式之間共用的資源。 這其中的部分子資源 (例如，頂點位置與色彩) 會使用 HLSL 語意來說明。 如需關於常數緩衝區與 HLSL 語意 (當它們與 OpenGL ES 2.0 概念相關時) 的詳細資訊，請參閱[移植框架緩衝區物件、Uniform 及屬性](porting-uniforms-and-attributes.md)。

將這個程序移至 Direct3D 時，我們會將 Uniform 轉換成 Direct3D 常數緩衝區 (cbuffer)，並為它指派暫存器，以使用 **register** HLSL 語意進行查詢。 這兩個頂點屬性會被處理成著色器管線階段的輸入元素，同時也會被指派可通知著色器的 [HLSL 語意](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dcl-usage---ps) (POSITION 與 COLOR0)。 像素著色器會採用 SV\_SV 的位置，\_前置詞表示它產生 GPU 的系統值。 （在此情況下，它會掃描轉換期間所產生的像素位置。）VertexShaderInput 和 PixelShaderInput 未宣告為常數緩衝處理，因為前者會用來定義頂點緩衝區 (請參閱[連接埠的端點緩衝區和資料](port-the-vertex-buffers-and-data-config.md))，而且後者的資料會產生的結果在管線中，在此情況下是端點著色器的上一個階段。

Direct3D:HLSL 常數緩衝區和頂點資料定義

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};

// Per-vertex data used as input to the vertex shader.
struct VertexShaderInput
{
  float4 pos : POSITION;
  float4 color : COLOR0;
};

// Per-vertex color data passed through the pixel shader.
struct PixelShaderInput
{
  float4 pos : SV_POSITION;
  float3 color : COLOR0;
};
```

如需關於移植到常數緩衝區與 HLSL 語意的應用程式詳細資訊，請參閱[移植框架緩衝區物件、Uniform 及屬性](porting-uniforms-and-attributes.md)。

這裡提供使用常數或頂點緩衝區傳送到著色器管線的資料配置結構。

Direct3D 11:宣告常數和頂點緩衝區配置

``` syntax
// Constant buffer used to send MVP matrices to the vertex shader.
struct ModelViewProjectionConstantBuffer
{
  DirectX::XMFLOAT4X4 modelViewProjection;
};

// Used to send per-vertex data to the vertex shader.
struct VertexPositionColor
{
  DirectX::XMFLOAT4 pos;
  DirectX::XMFLOAT4 color;
};
```

使用 DirectXMath XM\*您常數的類型會緩衝項目，因為它們提供適當的封裝和內容的對齊方式傳送至著色器管線時。 如果您使用標準的 Windows 平台浮點數類型與陣列，就必須自行執行封裝與對齊。

要繫結常數緩衝區，建立 版面配置描述作為[ **CD3D11\_緩衝區\_DESC** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-cd3d11_buffer_desc)結構，並將它傳遞給[ **ID3DDevice::CreateBuffer**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer)。 接著，在您的轉譯方法中，先將常數緩衝區傳送到 [**ID3D11DeviceContext::UpdateSubresource**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-updatesubresource)，再進行繪製。

Direct3D 11:常數緩衝區繫結

``` syntax
CD3D11_BUFFER_DESC constantBufferDesc(sizeof(ModelViewProjectionConstantBuffer), D3D11_BIND_CONSTANT_BUFFER);

m_d3dDevice->CreateBuffer(
  &constantBufferDesc,
  nullptr,
  &m_constantBuffer);

// ...

// Only update shader resources that have changed since the last frame.
m_d3dContext->UpdateSubresource(
  m_constantBuffer.Get(),
  0,
  NULL,
  &m_constantBufferData,
  0,
  0);
```

頂點緩衝區是以類似的方式建立和更新，而且會在下一個步驟[移植頂點緩衝區與資料](port-the-vertex-buffers-and-data-config.md)中討論。

<a name="next-step"></a>後續步驟
---------

[連接埠的端點緩衝區和資料](port-the-vertex-buffers-and-data-config.md)
## <a name="related-topics"></a>相關主題


[如何： 連接埠到 Direct3D 11 中的簡單的 OpenGL ES 2.0 轉譯器](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)

[連接埠的端點緩衝區和資料](port-the-vertex-buffers-and-data-config.md)

[GLSL 的連接埠](port-the-glsl.md)

[描繪至螢幕](draw-to-the-screen.md)

 

 




