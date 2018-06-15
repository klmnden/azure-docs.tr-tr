---
title: Azure Media Kırpıcıyı varlıklar yük | Microsoft Docs
description: Azure Media Kırpıcıyı varlıklar yükleme için adımlar
services: media-services
keywords: küçük; subclip; kodlama; ortam
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 11/10/2017
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 6a479218ff8bd5addf4273b23c06380859e0ea08
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33788301"
---
# <a name="loading-assets-into-azure-media-clipper"></a>Azure Media Kırpıcıyı varlıklar yükleniyor
Varlıklar Azure medya Kırpıcıyı iki yöntemle yüklenebilir:
1. Statik olarak varlıklar kitaplıkta geçirme
2. Dinamik olarak API aracılığıyla varlıkların listesi oluşturma

## <a name="statically-load-videos-into-clipper"></a>Statik olarak videolar Kırpıcıyı yükleme
Statik olarak videolar varlık seçimi Masası'ndan videolar seçmeden klipleri oluşturmak son kullanıcıların olanak Kırpıcıyı yükleyin.

Bu durumda, bir statik kümesinin varlıklar için Kırpıcıyı geçirin. Her varlık, bir AMS varlık/filtre kimliği, ad, yayımlanan akış URL'sini içerir. Uygunsa, içerik koruma kimlik doğrulama belirteci veya küçük resim dizisi URL'leri geçirilmesi. Geçirilen, küçük resimleri arabirimine doldurulur. Varlık Kitaplığı statik ve küçük olduğu senaryolarda Kitaplığı'nda her varlık için varlık sözleşmenin iletebilirsiniz.

> [!NOTE]
> Varlıklar Kırpıcıyı yüklenirken, statik olarak, varlıklar eklenen **doğrudan zaman çizelgesine** ve **varlık bölmesinde değil çizilir**. İlk varlık çizelgesine eklenir ve varlıkları kalan zaman çizelgesi sağ tarafında Yığılmış).

Statik varlık kitaplığı yüklemek için kullanmak **yük** her varlık bir JSON temsili geçirmek için yöntem. Aşağıdaki kod örneği, bir varlık için JSON gösterimi gösterilmektedir.

```javascript
var assets = [
    {
      /* Required: represents the Azure Media Services asset Id when "type" === "asset"; otherwise, represents the dynamic manifest asset filter Id ("type" === "filter")  */
      "id": "my-asset-or-dynamic-manifest-asset-filter-id",
    
      /* Required: represents the asset name as shown in the Clipper interface */
      "name": "My Asset or Dynamic Manifest Asset Filter Name",
    
      /* Required: must be one of the following values: "asset" or "filter" */
      /* NOTE: "asset" type represents a rendered asset; "filter" type represents a dynamic manifest asset filter */
      "type": "asset",
    
      /* Required */
      "source": {
    
        /* Required: represents the asset streaming locator, the base Smooth Streaming URL */
        "src": "//amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest",
    
        /* Optional: default value "application/vnd.ms-sstr+xml" */
        "type": "application/vnd.ms-sstr+xml",
    
        /* Required: If the asset has content protection applied, then you must include an array with the different protection types along with the token to request the license/key; otherwise, provide an empty array */
        "protectionInfo": [{
            "type": "AES",
            "authenticationToken": "Bearer aes-token-placeholder"
          },
          {
            "type": "PlayReady",
            "authenticationToken": "Bearer playready-token-placeholder"
          },
          {
            "type": "Widevine",
            "authenticationToken": "Bearer widevine-token-placeholder"
          },
          {
            "type": "FairPlay",
            "certificateUrl": "//example/path/to/myfairplay.der",
            "authenticationToken": "Bearer fairplay-token-placeholder"
          }
        ]
      },
    
      /* Optional: array containing thumbnail URLs for the video. */
      /* NOTE: For the thumbnail URLs to work as expected in the Clipper timeline they must be evenly distributed across the video (based on the duration) and in chronological order within the array. */
      "thumbnails": [
        "//example/path/thumbnail_001.jpg",
        "//example/path/thumbnail_002.jpg",
        "//example/path/thumbnail_003.jpg",
        "//example/path/thumbnail_004.jpg",
        "//example/path/thumbnail_005.jpg"
        ]
    }
];

var subclipper = new subclipper({
    selector: '#root',
    restVersion: '2.0',
    submitSubclipCallback: onSubmitSubclip,
});
subclipper.ready(function () {
    subclipper.load(assets);
});

```

> [!NOTE]
> Load() yöntemini çağırmak önceki örnekte gösterildiği gibi ready(handler) yöntemiyle zinciri önerilir. Önceki örnekte varlıklar yüklemeden önce pencere öğesi hazır olduğunu güvence altına alır.

> [!NOTE]
> Küçük resim bunlar eşit şekilde dağıtılması gerekir (süreye göre göre) video arasında Kırpıcıyı çizelgesinde beklendiği şekilde çalışması için URL'leri ve dizi içinde kronolojik sırada. 'Medya Kodlayıcısı standart' işlemci ile görüntüleri oluşturmak için aşağıdaki JSON hazır parçacığını bir örnek başvuru olarak kullanabilirsiniz:

```json
{
  "Start": "0",
  "Step": "00:00:05",
  "Range": "100%",
  "Type": "PngImage",
  "PngLayers": [
    {
      "Type": "PngLayer",
      "Width": 48,
      "Height": 26
    }
  ]
}
```

## <a name="dynamically-load-videos-in-clipper"></a>Dinamik olarak Kırpıcıyı videoları yükleme
Dinamik olarak videolar karşı küçük için varlık seçimi panelinden videolar seçmek son kullanıcıların olanak Kırpıcıyı yükleyin.

Alternatif olarak, bir geri çağırma aracılığıyla dinamik olarak varlıklar yükleyebilirsiniz. Varlıklar dinamik olarak oluşturulur veya kitaplık büyük olduğu senaryolarda geri yüklemeniz gerekir. Varlık dinamik olarak yüklemek için isteğe bağlı onLoadAssets geri çağırma işlevi uygulamalıdır. Bu işlev, başlatma sırasında Kırpıcıyı içine geçirilir. Çözümlenen varlıklar statik olarak yüklenen varlıkların aynı sözleşmeye uymanız gerekir. Aşağıdaki kod örneği, yöntem imzası, beklenen giriş ve beklenen çıktı gösterilmektedir.

> [!NOTE]
> Varlıklar Kırpıcıyı yüklenirken, dinamik olarak, varlıklar içinde işlenir **varlık seçim bölmesi**.

```javascript
// Video Assets Pane Callback
    //
    // Filter Parameters:
    // - search: string value term that will be used in the back-end to filter assets by name.
    // - skip: int value used for pagination in the back-end that allows skipping a number of assets in the response.
    // - take: int value used for pagination in the back-end that allows defining the number of assets to include in the response.
    // - type: ('filter', 'asset') value that will be used in the back-end to filter assets by type.
    //
    // Returns: a Promise object that, when resolved, retuns an object containing an array of assets (input contract)
    //          that satisfies the filter parameters, plus optionally the total types of files available:
    // {
    //  total: 100,
    //  assets: [{...}],
    // }
    var onLoadAssets = function (search, skip, take, type) {
        var promise = new Promise(function (resolve, reject) {
            // TODO: implement the getAssetsFromBackend method to get the assets from the back-end using the filter parameters (search, skip, take, type).
            getAssetsFromBackend(search, skip, take, type)
                .then(function (assets) {
                    resolve({
                        total: assets.length,
                        assets: assets
                    });
                }).catch(function () {
                    reject(Error("error details"));
                });
        });
    
        return promise;
    };

    // Create widget instance:
    // - using a root element selector
    // - enabling the Video Assets panel by registering a callback in the 'assetsPanelLoaderCallback' option parameter.
    var widget = new subclipper({
        selector: '#root',

        // Enable the Video Assets panel in the widget to automatically load assets (input contract)
        assetsPanelLoaderCallback: onLoadAssets
    });

    // ...
    // The widget will automatically invoke the 'assetsPanelLoaderCallback' callback with the filter parameters specified by the user 
    // and load assets returned by the Promise into the Video Assets panel.
    // ...
```