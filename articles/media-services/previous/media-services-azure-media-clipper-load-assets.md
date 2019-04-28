---
title: Azure Media Clipper'a varlık yükleme | Microsoft Docs
description: Azure Media Clipper'a varlık yükleme adımları
services: media-services
keywords: clip;subclip;encoding;media
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 03/14/2019
ms.topic: article
ms.service: media-services
ms.openlocfilehash: ec8cd06be78bbd8df0bca390696e736c3a6ee075
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61465890"
---
# <a name="loading-assets-into-azure-media-clipper"></a>Azure Media Clipper'a varlık yükleme  

Varlıklar Azure Media Clipper'a iki yöntemle yüklenebilir:
1. Varlıkların kitaplığa statik olarak geçirme
2. Dinamik olarak bir API aracılığıyla varlıkların listesi oluşturma

## <a name="statically-load-videos-into-clipper"></a>Statik olarak videoları Clipper'a varlık yükleme
Statik olarak video klipleri varlık seçimi panelinden videoları seçmeden oluşturmak son kullanıcıların olanak Clipper'ı yükleyin.

Bu durumda, Clipper için statik varlıklar kümesi içinde geçirin. Her varlık, bir AMS varlığı/filtre kimliği, adı, yayımlanan akış URL'sini içerir. Uygunsa, içerik koruma kimlik doğrulama belirteci veya küçük bir dizi URL'leri de geçirilebilir. Geçirilen, küçük resimleri arabirimine doldurulur. Varlık kitaplığa statik ve küçük olduğu senaryolarda, kitaplıktaki her varlık için varlık sözleşmede geçirebilirsiniz.

> [!NOTE]
> Statik olarak Clipper'a varlık yükleme, varlıkları eklenir **doğrudan zaman çizelgesine** ve **varlık bölmesinde değil işlenen**. İlk varlık zaman çizelgesine eklenir ve varlıkları geri kalanı zaman çizelgesi sağ tarafında Yığılmış).

Bir statik varlık kitaplığını yüklemek için kullanmak **yük** her varlık bir JSON temsili geçirmek için yöntemi. Aşağıdaki kod örneği bir varlık için JSON gösterimi gösterilmektedir.

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
> Load() yöntemini çağırmak yukarıdaki örnekte gösterildiği gibi ready(handler) yöntemiyle zinciri önerilir. Yukarıdaki örnekte, varlıkları yüklemeden önce pencere öğesi hazır olduğunu garanti eder.

> [!NOTE]
> Küçük resim URL'leri, bunlar eşit olarak dağıtılması gerekir (süresine göre) video arasında Clipper zaman çizelgesindeki beklendiği gibi çalıştığını ve dizi içinde kronolojik sırada. 'Medya Kodlayıcısı standart' işlemci ile görüntüleri oluşturmak için aşağıdaki JSON önceden oluşturulmuş kod parçacığını bir örnek başvuru olarak kullanabilirsiniz:

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

## <a name="dynamically-load-videos-in-clipper"></a>Dinamik olarak Clipper videoları yükleme
Dinamik olarak videoları videoları karşı kırpmak için varlık seçim paneli seçmek son kullanıcıların olanak Clipper'ı yükleyin.

Alternatif olarak, bir geri çağırma aracılığıyla dinamik olarak varlıkları yükleyebilirsiniz. Burada varlıklarını dinamik olarak oluşturulan ya da büyük bir kitaplıktır senaryolarda geri yüklemeniz gerekir. Varlık dinamik olarak yüklemek için isteğe bağlı onLoadAssets geri çağırma işlevi uygulamalıdır. Bu işlev, başlatma sırasında Clipper uygulamasına geçirilir. Çözümlenen varlıklar, statik olarak yüklenen varlıklar olarak aynı sözleşmeye uymalıdır. Aşağıdaki kod örneği, yöntem imzası, beklenen giriş ve beklenen çıktıyı gösterir.

> [!NOTE]
> Dinamik olarak Clipper'a varlık yükleme olduğunda, varlıklar olarak işlenir **varlık seçim paneli**.

```javascript
// Video Assets Pane Callback
    //
    // Filter Parameters:
    // - search: string value term that will be used in the back-end to filter assets by name.
    // - skip: int value used for pagination in the back-end that allows skipping a number of assets in the response.
    // - take: int value used for pagination in the back-end that allows defining the number of assets to include in the response.
    // - type: ('filter', 'asset') value that will be used in the back-end to filter assets by type.
    //
    // Returns: a Promise object that, when resolved, returns an object containing an array of assets (input contract)
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
