---
title: "Azure Media Kırpıcıyı kırpma işleri gönderme | Microsoft Docs"
description: "Azure Media Kırpıcıyı kırpma işleri göndermek için adımları"
services: media-services
keywords: "küçük; subclip; kodlama; ortam"
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 11/10/2017
ms.topic: article
ms.service: media-services
ms.openlocfilehash: d29889a4c972638f5d127e9c518aa85fbc19d861
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="submit-clipping-jobs-from-azure-media-clipper"></a>Azure Media Kırpıcıyı kırpma işlerini gönderme
Azure Media Kırpıcıyı gerektiren bir **submitSubclipCallback** kırpma iş gönderme işlemek için uygulanacak yöntemi. Bu işlev bir web hizmetine Kırpıcıyı çıktının bir HTTP POST uygulamak için kullanılır. Kodlama işinin gönderebileceği bu web hizmetidir. Kırpıcıyı çıktısını ya da bir medya Kodlayıcı işlenmiş işleri için hazır ya da dinamik bildirim filtresi çağrıları için REST API yükü kodlama standarttır. Media services hesap kimlik bilgilerini istemcinin tarayıcıda güvenli olmadığından bu geçiş modeli gereklidir.

Aşağıdaki sıralı diyagramı tarayıcı istemci, web hizmeti ve Azure Media Services arasında iş akışı gösterilmektedir: ![Azure medya Kırpıcıyı sıralı diyagramı](media/media-services-azure-media-clipper-submit-job/media-services-azure-media-clipper-sequence-diagram.PNG)

Yukarıdaki şemada, dört varlıklardır: son kullanıcının tarayıcı, web hizmetiniz, barındırma Kırpıcıyı kaynakları ve Azure Media Services CDN uç noktası. Son kullanıcı web sayfanıza gittiğinde, sayfa Kırpıcıyı JavaScript ve CSS kaynaklar barındırma CDN uç noktasından alır. Son kullanıcı kırpma iş veya dinamik bildirim filtresi oluşturma tarayıcısında çağrısından yapılandırır. Son kullanıcı iş veya filtresi oluşturma çağrı gönderdiğinde, tarayıcı dağıtmanız gerekir bir web hizmeti için iş yükü getirir. Bu web Hizmeti'nin sonuçta kırpma işi gönderir veya medya kullanarak Azure Media Services filtresi oluşturma çağrısı hizmetleri hesabı kimlik bilgileri.

Aşağıdaki kod örneği bir örnek gösterilmektedir **submitSubclipCallback** yöntemi. Bu yöntemde Medya Kodlayıcısı standart kodlama hazır HTTP POST uygulayın. POST başarılı olduysa (**sonuç**), **Promise** , aksi takdirde hata ayrıntılarla reddedildi çözümlenir.

```javascript
// Submit Subclip Callback
//
// Parameter:
// - subclip: object that represents the subclip (output contract).
//
// Returns: a Promise object that, when resolved, retuns true if the operation was accept in the back-end; otherwise, returns false.
var onSubmitSubclip = function (subclip) {
    var promise = new Promise(function (resolve, reject) {
        // TODO: perform the back-end AJAX request to submit the subclip job.
        var result = true;

        if (/* everything turned out fine */ result) {
            resolve(result);
        }
        else {
            reject(Error("error details"));
        }
    });

    return promise;
};

var subclipper = new subclipper({
    selector: '#root',
    restVersion: '2.0',
    submitSubclipCallback: onSubmitSubclip,
});
```
Medya Kodlayıcısı standart kodlama önceden işlenmiş işi için ya da dinamik bildirim filtreleri için REST API yükü iş gönderme çıkışıdır.

## <a name="submitting-encoding-job-to-create-video"></a>Video oluşturmak için kodlama işi gönderme
Çerçeve doğru bir video klip oluşturmak için bir Medya Kodlayıcısı standart kodlama işi gönderebilirsiniz. Videolar işlenen iş ürettiği kodlama, yeni bir MP4 dosyası parçalanmış.

Proje çıktı sözleşme işlenmiş kırpma için aşağıdaki özelliklere sahip bir JSON nesnesidir:

```json
{
  /* Required */
  "name": "My New Subclip Output Name",

  /* Required: string specifying the format of the Json file.
   */
  /* NOTE: The REST API version, currently 2.0 is supported
   */
  "restVersion": "2.0"

  /* Required: array containing all the input Ids (both "asset" or "filter" type) used in the subclipper timeline;
  it must contain at least one item. */
  /* NOTE: when "output"."type" === "job", all items must match the "inputsIds"[i]."type" === "asset" condition;
  this array can be used in the back-end to get the input assets for the subclipping job with the Media Encoder Standard (MES) processor. */
  /* NOTE: when "output"."type" === "filter", there must be a single item in the array ("inputsIds".length === 1)
  that can be used in the back-end to get the source for the dynamic manifest filter (create or update). */
  "inputsIds": [{
    /* Required */
    "id": "my-asset-id-1",

    /* Required: must be one of the following values: "asset" or "filter" */
    "type": "asset"
  }, {
    /* Required */
    "id": "my-asset-id-2",

    /* Required: must be one of the following values: "asset" or "filter" */
    "type": "asset"
  }],

  /* Required */
  "output": {

    /* Required: must be one of the following values: "job" or "filter" */
    "type": "job",

    /* Required if "type" === "job" */
    /* NOTE: This is the preset for the Media Encoder Standard (MES) processor that can be used in the back-end to sumit the subclip job.
    The encoding profile ("Codecs" property) depends on the "singleBitrateMp4Profile" and "multiBitrateMp4Profile" option parameters
    specified when creating the widget instance. */
    /* REFERENCE: https://docs.microsoft.com/azure/media-services/media-services-advanced-encoding-with-mes */
    "job": {
      "Version": 1.0,
      "Codecs": [{
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": true,
          "H264Layers": [{
            "Profile": "Auto",
            "Level": "auto",
            "Bitrate": 6750,
            "MaxBitrate": 6750,
            "BufferWindow": "00:00:05",
            "Width": 1920,
            "Height": 1080,
            "BFrames": 3,
            "ReferenceFrames": 3,
            "AdaptiveBFrame": true,
            "Type": "H264Layer",
            "FrameRate": "0/1"
          }],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [{
        "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
        "Format": {
          "Type": "MP4Format"
        }
      }],
      "Sources": [{
        "AssetId": "my-asset-id-1",
        "StartTime": "0.00:01:15.000",
        "Duration": "0.00:00:25.000"
      }, {
        "AssetId": "my-asset-id-2",
        "StartTime": "0.00:20:04.000",
        "Duration": "0.00:33:07.500"
      }]
    }
  }
}
```

Kodlama işi gerçekleştirmek göndermek için Medya Kodlayıcısı standart ile ilişkili kodlama işinin hazır. Kodlama gönderme hakkında bilgi için bu makalenin işleri görmek [.NET SDK'sı](https://docs.microsoft.com/en-us/azure/media-services/media-services-dotnet-encode-with-media-encoder-standard) veya [REST API](https://docs.microsoft.com/en-us/azure/media-services/media-services-rest-encode-asset).

## <a name="quickly-creating-video-clips-without-encoding"></a>Video klip kodlamadan hızlı oluşturma
Bir kodlama işi oluşturmak için bir alternatif, dinamik bildirim filtreleri oluşturmak için Azure Media Kırpıcıyı kullanabilirsiniz. Filtreler kodlama gerektirmez ve yeni bir varlık oluşturulmaz gibi hızlı bir şekilde oluşturulabilir. Bir JSON nesnesi aşağıdaki özelliklere sahip bir filtre kırpma için çıktı sözleşmedir:

```json
{
  /* Required: Filter name (Alphanumeric characters and hyphens only, no whitespace) */
  "name": "FilterName",

  /* Required: string specifying the format of the Json file.
   */
  /* NOTE: This subclipper version always returns '2.0' in this field.
   */
  "restVersion": "2.0"

  /* Required: array containing all the input Ids (both "asset" or "filter" type) used in the subclipper timeline;
  it must contain at least one item. */
  /* NOTE: when "output"."type" === "job", all items must match the "inputsIds"[i]."type" === "asset" condition;
  this array can be used in the back-end to get the input assets for the subclipping job with the Media Encoder Standard (MES) processor. */
  /* NOTE: when "output"."type" === "filter", there must be a single item in the array ("inputsIds".length === 1)
  that can be used in the back-end to get the source for the dynamic manifest filter (create or update). */
  "inputsIds": [{
    /* Required */
    "id": "my-asset-id",

    /* Required: must be one of the following values: "asset" or "filter" */
    "type": "asset"
  }],

  /* Required */
  "output": {

    /* Required: must be one of the following values: "job" or "filter" */
    "type": "filter",

    /* Required if "type" === "filter" */
    /* REFERENCE: https://docs.microsoft.com/rest/api/media/operations/assetfilter */
    "filter": {
      "PresentationTimeRange": {
        "StartTimestamp": "340000000",
        "EndTimestamp": "580000000",
        "Timescale": "10000000"
      },
      "Tracks": [{
        "PropertyConditions": [{
          "Property": "Type",
          "Value": "video",
          "Operator": "Equal"
        }]
      }, {
        "PropertyConditions": [{
          "Property": "Type",
          "Value": "audio",
          "Operator": "Equal"
        }, {
          "Property": "Name",
          "Value": "audio-track-1",
          "Operator": "Equal"
        }]
      }, {
        "PropertyConditions": [{
          "Property": "Type",
          "Value": "text",
          "Operator": "Equal"
        }, {
          "Property": "Language",
          "Value": "en",
          "Operator": "Equal"
        }]
      }]
    }
  }
}
```

İlişkili filtre yükü kullanarak dinamik bildirim filtresi oluşturmak için REST çağrısı göndermek için Gönder [REST API](https://docs.microsoft.com/en-us/azure/media-services/media-services-rest-dynamic-manifest).