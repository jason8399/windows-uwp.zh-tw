---
author: mcleanbyron
description: 描述 Windows.Services.Store 命名空間中 Microsoft Store 產品的延伸 JSON 資料結構描述。
title: Microsoft Store 產品的資料結構描述
ms.author: mcleans
ms.date: 09/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, ExtendedJsonData, Microsoft Store 產品, 結構描述
ms.localizationpriority: medium
ms.openlocfilehash: 053391e78b4356e7e8042ba7864d99f1c95b2aad
ms.sourcegitcommit: 2470c6596d67e1f5ca26b44fad56a2f89773e9cc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
ms.locfileid: "1673825"
---
# <a name="data-schemas-for-store-products"></a>Microsoft Store 產品的資料結構描述

當您提交產品 (例如應用程式或附加元件) 至 Microsoft Store 時，Microsoft Store 會為產品及其授權維護一份完整的資料。 在您應用程式的程式碼中，您可以使用 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空間中的屬性，以程式設計方式存取此資料的某部分。 例如，您可以使用 [StoreProduct.Description](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.Description) 和 [StoreProduct.Price](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.Price) 屬性，擷取目前應用程式或目前應用程式之附加元件的描述和價格。

不過，Microsoft Store 中產品的大部分資料不會由 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空間的預先定義屬性公開。 若要在程式碼中存取產品的完整資料，您可以改用下列一般屬性：

* [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.ExtendedJsonData)
* [StoreSku.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.ExtendedJsonData)
* [StoreAvailability.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability.ExtendedJsonData)
*   [StoreCollectionData.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata.ExtendedJsonData)
*   [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData)
* [StoreLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense.ExtendedJsonData)
*   [StorePurchaseProperties.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storepurchaseproperties.ExtendedJsonData)

這些屬性會傳回對應物件完整資料的 JSON 格式字串。 本文提供這些屬性傳回之資料的完整結構描述。

> [!NOTE]
> Microsoft Store 中的產品依照 [product](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct)、[SKU](https://docs.microsoft.com/uwp/api/windows.services.store.storesku)、[availability](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability) 物件的階層分門別類。 如需詳細資訊，請參閱[產品、SKU 和可用性](in-app-purchases-and-trials.md#products-skus)。

## <a name="schema-for-storeproduct-storesku-storeavailability-and-storecollectiondata"></a>StoreProduct、StoreSku、StoreAvailability 和 StoreCollectionData 的結構描述

下列結構描述說明 [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.ExtendedJsonData) 傳回的 JSON 格式字串。 [StoreSku.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.ExtendedJsonData)、[StoreAvailability.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability.ExtendedJsonData) 和 [StoreCollectionData.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata.ExtendedJsonData) 屬性僅分別傳回 ```DisplaySkuAvailabilities\Sku```、```DisplaySkuAvailabilities\Availabilities``` 和 ```DisplaySkuAvailabilities\Sku\CollectionData``` 路徑底下定義的部分結構描述。

如需 [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.ExtendedJsonData) 所傳回 JSON 格式字串的範例，請參閱[本節](#product-example)。

[!code[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonData.json#L1-L729)]

<span id="product-example" />

### <a name="example"></a>範例

下例範例示範應用程式的 [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.ExtendedJsonData) 屬性傳回的 JSON 格式字串。

[!code[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonDataExample.json#L1-L268)]

## <a name="schema-for-storeapplicense-and-storelicense"></a>StoreAppLicense 和 StoreLicense 的結構描述

下列結構描述說明 [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData) 傳回的 JSON 格式字串。 [StoreLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense.ExtendedJsonData) 屬性僅傳回 ```productAddOns``` 路徑底下定義的部分結構描述。

如需 [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData) 所傳回 JSON 格式字串的範例，請參閱[本節](#license-example)。

[!code[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonData.json#L1-L80)]

<span id="license-example" />

### <a name="example"></a>範例

下例範例示範應用程式的 [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData) 屬性傳回的 JSON 格式字串。

[!code[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonDataExample.json#L1-L28)]

## <a name="schema-for-storepurchaseproperties"></a>StorePurchaseProperties 的結構描述

下列結構描述說明 [StorePurchaseProperties.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storepurchaseproperties.ExtendedJsonData) 傳回的 JSON 格式字串。

[!code[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StorePurchaseProperties.ExtendedJsonData.json#L1-L12)]

## <a name="related-topics"></a>相關主題

* [App 內購買和試用版](in-app-purchases-and-trials.md)
* [取得應用程式和附加元件的產品資訊](get-product-info-for-apps-and-add-ons.md)
* [取得 App 和附加元件的授權資訊](get-license-info-for-apps-and-add-ons.md)
* [啟用 App 和附加元件的 App 內購買](enable-in-app-purchases-of-apps-and-add-ons.md)
* [啟用消費性附加元件購買](enable-consumable-add-on-purchases.md)
* [實作您應用程式的試用版](implement-a-trial-version-of-your-app.md)