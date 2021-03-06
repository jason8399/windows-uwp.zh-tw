---
title: 建立著色器及繪製基本型別
description: 以下我們將示範如何使用 HLSL 來源檔案，以編譯及建立您可以接著用來在顯示器上繪製基本型別的著色器。
ms.assetid: 91113bbe-96c9-4ef9-6482-39f1ff1a70f4
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, 著色器, 原始物件, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: fecce6237d08f9ffa89bc7503412a357b17c641d
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368951"
---
# <a name="create-shaders-and-drawing-primitives"></a>建立著色器及繪製基本型別



以下我們將示範如何使用 HLSL 來源檔案，以編譯及建立您可以接著用來在顯示器上繪製基本型別的著色器。

我們會使用頂點和像素著色器來建立及繪製一個黃色三角形。 在建立 Direct3D 裝置、交換鏈結及轉譯目標檢視之後，我們會從磁碟上的二進位著色器物件檔案讀取資料。

**目標：** 若要建立著色器，以及繪製基本項目。

## <a name="prerequisites"></a>先決條件


我們假設您熟悉 C++。 您還需要圖形程式設計概念的基本經驗。

我們也假設您已經看過[快速入門：設定 DirectX 資源及顯示影像](setting-up-directx-resources.md)。

**完成時間：** 20 分鐘的時間。

## <a name="instructions"></a>指示

### <a name="1-compiling-hlsl-source-files"></a>1.編譯 HLSL 原始程式檔

Microsoft Visual Studio 使用 [fxc.exe](https://docs.microsoft.com/windows/desktop/direct3dtools/fxc) HLSL 程式碼編譯器，將 .hlsl 來源檔案 (SimpleVertexShader.hlsl 和 SimplePixelShader.hlsl) 編譯成 .cso 二進位著色器物件檔案 (SimpleVertexShader.cso 和 SimplePixelShader.cso)。 如需 HLSL 程式碼編譯器的詳細資訊，請參閱＜效果編譯器工具＞。 如需有關編譯著色器程式碼的詳細資訊，請參閱[編譯著色器](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-part1)。

以下是 SimpleVertexShader.hlsl 中的程式碼：

```hlsl
struct VertexShaderInput
{
    DirectX::XMFLOAT2 pos : POSITION;
};

struct PixelShaderInput
{
    float4 pos : SV_POSITION;
};

PixelShaderInput SimpleVertexShader(VertexShaderInput input)
{
    PixelShaderInput vertexShaderOutput;

    // For this lesson, set the vertex depth value to 0.5, so it is guaranteed to be drawn.
    vertexShaderOutput.pos = float4(input.pos, 0.5f, 1.0f);

    return vertexShaderOutput;
}
```

以下是 SimplePixelShader.hlsl 中的程式碼：

```hlsl
struct PixelShaderInput
{
    float4 pos : SV_POSITION;
};

float4 SimplePixelShader(PixelShaderInput input) : SV_TARGET
{
    // Draw the entire triangle yellow.
    return float4(1.0f, 1.0f, 0.0f, 1.0f);
}
```

### <a name="2-reading-data-from-disk"></a>2.從磁碟讀取資料

我們可以使用 DirectX 11 App (通用 Windows) 範本中 DirectXHelper.h 的 DX::ReadDataAsync 功能，以非同步方式讀取磁碟上的檔案資料。

### <a name="3-creating-vertex-and-pixel-shaders"></a>3.建立頂點和像素著色器

我們會從 SimpleVertexShader.cso 檔案讀取資料，並將資料指派給 *vertexShaderBytecode* 位元組陣列。 我們會使用該位元組陣列呼叫 [**ID3D11Device::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) 來建立頂點著色器 ([**ID3D11VertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11vertexshader))。 我們會在 SimpleVertexShader.hlsl 來源中將頂點深度值設定為 0.5，以確保會繪製出三角形。 我們會填入的陣列[ **D3D11\_輸入\_項目\_DESC** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_input_element_desc)結構描述 頂點著色器程式碼的版面配置，然後呼叫[**ID3D11Device::CreateInputLayout** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createinputlayout)建立配置。 陣列有一個定義頂點位置的配置元素。 我們會從 SimplePixelShader.cso 檔案讀取資料，並將資料指派給 *pixelShaderBytecode* 位元組陣列。 我們會使用該位元組陣列呼叫 [**ID3D11Device::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) 來建立像素著色器 ([**ID3D11PixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11pixelshader))。 我們會在 SimplePixelShader.hlsl 來源中將像素值設定為 (1,1,1,1)，以讓三角形呈黃色。 您可以變更這個值來變更色彩。

我們會建立定義簡單三角形的頂點和索引緩衝區。 若要這樣做，我們先定義三角形，接下來描述頂點和索引緩衝區 ([**D3D11\_緩衝區\_DESC** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_buffer_desc)並[ **D3D11\_SUBRESOURCE\_資料**](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data)) 使用三角形定義，以及最後呼叫[ **ID3D11Device::CreateBuffer** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer)一次，每個緩衝區。

```cpp
        auto loadVSTask = DX::ReadDataAsync(L"SimpleVertexShader.cso");
        auto loadPSTask = DX::ReadDataAsync(L"SimplePixelShader.cso");
        
        // Load the raw vertex shader bytecode from disk and create a vertex shader with it.
        auto createVSTask = loadVSTask.then([this](const std::vector<byte>& vertexShaderBytecode) {


          ComPtr<ID3D11VertexShader> vertexShader;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateVertexShader(
                  vertexShaderBytecode->Data,
                  vertexShaderBytecode->Length,
                  nullptr,
                  &vertexShader
                  )
              );

          // Create an input layout that matches the layout defined in the vertex shader code.
          // For this lesson, this is simply a DirectX::XMFLOAT2 vector defining the vertex position.
          const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
          {
              { "POSITION", 0, DXGI_FORMAT_R32G32_FLOAT, 0, 0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
          };

          ComPtr<ID3D11InputLayout> inputLayout;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateInputLayout(
                  basicVertexLayoutDesc,
                  ARRAYSIZE(basicVertexLayoutDesc),
                  vertexShaderBytecode->Data,
                  vertexShaderBytecode->Length,
                  &inputLayout
                  )
              );
        });
        
        // Load the raw pixel shader bytecode from disk and create a pixel shader with it.
        auto createPSTask = loadPSTask.then([this](const std::vector<byte>& pixelShaderBytecode) {
          ComPtr<ID3D11PixelShader> pixelShader;
          DX::ThrowIfFailed(
              m_d3dDevice->CreatePixelShader(
                  pixelShaderBytecode->Data,
                  pixelShaderBytecode->Length,
                  nullptr,
                  &pixelShader
                  )
              );
        });

        // Create vertex and index buffers that define a simple triangle.
        auto createTriangleTask = (createPSTask && createVSTask).then([this] () {

          DirectX::XMFLOAT2 triangleVertices[] =
          {
              float2(-0.5f, -0.5f),
              float2( 0.0f,  0.5f),
              float2( 0.5f, -0.5f),
          };

          unsigned short triangleIndices[] =
          {
              0, 1, 2,
          };

          D3D11_BUFFER_DESC vertexBufferDesc = {0};
          vertexBufferDesc.ByteWidth = sizeof(float2) * ARRAYSIZE(triangleVertices);
          vertexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          vertexBufferDesc.BindFlags = D3D11_BIND_VERTEX_BUFFER;
          vertexBufferDesc.CPUAccessFlags = 0;
          vertexBufferDesc.MiscFlags = 0;
          vertexBufferDesc.StructureByteStride = 0;

          D3D11_SUBRESOURCE_DATA vertexBufferData;
          vertexBufferData.pSysMem = triangleVertices;
          vertexBufferData.SysMemPitch = 0;
          vertexBufferData.SysMemSlicePitch = 0;

          ComPtr<ID3D11Buffer> vertexBuffer;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateBuffer(
                  &vertexBufferDesc,
                  &vertexBufferData,
                  &vertexBuffer
                  )
              );

          D3D11_BUFFER_DESC indexBufferDesc;
          indexBufferDesc.ByteWidth = sizeof(unsigned short) * ARRAYSIZE(triangleIndices);
          indexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          indexBufferDesc.BindFlags = D3D11_BIND_INDEX_BUFFER;
          indexBufferDesc.CPUAccessFlags = 0;
          indexBufferDesc.MiscFlags = 0;
          indexBufferDesc.StructureByteStride = 0;

          D3D11_SUBRESOURCE_DATA indexBufferData;
          indexBufferData.pSysMem = triangleIndices;
          indexBufferData.SysMemPitch = 0;
          indexBufferData.SysMemSlicePitch = 0;

          ComPtr<ID3D11Buffer> indexBuffer;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateBuffer(
                  &indexBufferDesc,
                  &indexBufferData,
                  &indexBuffer
                  )
              );
        });
```

我們會使用頂點和像素著色器、頂點著色器配置及頂點和索引緩衝區，以繪製黃色三角形。

### <a name="4-drawing-the-triangle-and-presenting-the-rendered-image"></a>4.繪製三角形和呈現所呈現的影像

我們會進入一個無限迴圈來不斷轉譯並顯示場景。 我們會呼叫 [**ID3D11DeviceContext::OMSetRenderTargets**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets)，以將轉譯目標指定為輸出目標。 我們會呼叫 [**ID3D11DeviceContext::ClearRenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-clearrendertargetview) 搭配 { 0.071f, 0.04f, 0.561f, 1.0f }，以將轉譯目標清除成純藍色。

在無限迴圈中，我們會在藍色表面上繪製一個黃色三角形。

**若要繪製的黃色三角形**

1.  首先，我們會呼叫 [**ID3D11DeviceContext::IASetInputLayout**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout) 來描述頂點緩衝區資料如何串流處理到輸入組合器階段。
2.  接著，我們會呼叫 [**ID3D11DeviceContext::IASetVertexBuffers**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers) 和 [**ID3D11DeviceContext::IASetIndexBuffer**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer) 來將頂點和索引緩衝區繫結到輸入組合階段。
3.  接下來，我們會呼叫[ **ID3D11DeviceContext::IASetPrimitiveTopology** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetprimitivetopology)具有[ **D3D11\_基本\_拓樸\_TRIANGLESTRIP** ](https://docs.microsoft.com/previous-versions/windows/desktop/legacy/ff476189(v=vs.85))值來指定輸入組譯工具階段解譯為三角形區域的頂點資料。
4.  接著，我們會呼叫 [**ID3D11DeviceContext::VSSetShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetshader) 以使用頂點著色器程式碼起始頂點著色階段，並且呼叫 [**ID3D11DeviceContext::PSSetShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader) 以使用像素著色器程式碼起始像素著色階段。
5.  最後，我們會呼叫 [**ID3D11DeviceContext::DrawIndexed**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed) 來繪製三角形並將它提交給轉譯管線。

我們會呼叫 [**IDXGISwapChain::Present**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present)，以將轉譯的影像呈現到視窗。

```cpp
            // Specify the render target we created as the output target.
            m_d3dDeviceContext->OMSetRenderTargets(
                1,
                m_renderTargetView.GetAddressOf(),
                nullptr // Use no depth stencil.
                );

            // Clear the render target to a solid color.
            const float clearColor[4] = { 0.071f, 0.04f, 0.561f, 1.0f };
            m_d3dDeviceContext->ClearRenderTargetView(
                m_renderTargetView.Get(),
                clearColor
                );

            m_d3dDeviceContext->IASetInputLayout(inputLayout.Get());

            // Set the vertex and index buffers, and specify the way they define geometry.
            UINT stride = sizeof(float2);
            UINT offset = 0;
            m_d3dDeviceContext->IASetVertexBuffers(
                0,
                1,
                vertexBuffer.GetAddressOf(),
                &stride,
                &offset
                );

            m_d3dDeviceContext->IASetIndexBuffer(
                indexBuffer.Get(),
                DXGI_FORMAT_R16_UINT,
                0
                );

            m_d3dDeviceContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

            // Set the vertex and pixel shader stage state.
            m_d3dDeviceContext->VSSetShader(
                vertexShader.Get(),
                nullptr,
                0
                );

            m_d3dDeviceContext->PSSetShader(
                pixelShader.Get(),
                nullptr,
                0
                );

            // Draw the cube.
            m_d3dDeviceContext->DrawIndexed(
                ARRAYSIZE(triangleIndices),
                0,
                0
                );

            // Present the rendered image to the window.  Because the maximum frame latency is set to 1,
            // the render loop will generally be throttled to the screen refresh rate, typically around
            // 60 Hz, by sleeping the application on Present until the screen is refreshed.
            DX::ThrowIfFailed(
                m_swapChain->Present(1, 0)
                );
```

## <a name="summary-and-next-steps"></a>摘要與後續步驟


我們使用頂點和像素著色器建立及繪製了一個黃色三角形。

接著，我們會建立一個環繞移動的 3D 立方體，並將光源效果套用到該立方體。

[使用深度和原始物件的效果](using-depth-and-effects-on-primitives.md)

 

 




