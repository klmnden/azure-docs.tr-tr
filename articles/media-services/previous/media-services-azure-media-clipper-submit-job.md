---
title: Azure Media Clipper'ı kırpma işlerini gönderme | Microsoft Docs
description: Azure Media Clipper'ı kırpma işlerini gönderme adımları
services: media-services
keywords: clip;subclip;encoding;media
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 03/14/2019
ms.topic: article
ms.service: media-services
ms.openlocfilehash: f0dc6879ccbb22dbebd57de98e4610cd593318db
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61242856"
---
# <a name="submit-clipping-jobs-from-azure-media-clipper"></a>Azure Media Clipper'ı kırpma işlerini gönderme 

Azure Media Clipper'ı gerektiren bir **submitSubclipCallback** kırpma iş gönderme işlemek için uygulanması gereken yöntemini. Bu işlev, bir HTTP POST Clipper çıktının bir web hizmeti uygulamak için kullanılır. Kodlama işinin gönderebileceği bu web hizmetidir. Clipper çıktısını ya da bir medya Kodlayıcı işlenen işleri için hazır ya da dinamik bildirim filtresi çağrıları için REST API yükü kodlama standarttır. Doğrudan bu model, medya hizmetleri hesabı kimlik bilgileri istemcinin tarayıcıda güvenli olmadığı için gereklidir.

Aşağıdaki sıralama diyagramı tarayıcı istemcisi, web hizmeti ve Azure Media Services arasında iş akışı gösterilmektedir: ![Azure Media Clipper sıralı diyagram](media/media-services-azure-media-clipper-submit-job/media-services-azure-media-clipper-sequence-diagram.PNG)

Yukarıdaki diyagramda, dört varlıklardır: son kullanıcının tarayıcı, web hizmetiniz, barındırma Clipper kaynakları ve Azure Media Services CDN uç noktası. Son kullanıcının web sayfasına gittiğinde, sayfanın Clipper JavaScript ve CSS kaynakları barındıran bir CDN uç noktasından alır. Son kullanıcı iş kırpma veya tarayıcıları dinamik bildirim filtresi oluşturma çağrısından yapılandırır. Son kullanıcı iş veya filtre oluşturma çağrı gönderdiğinde, tarayıcı dağıtmalısınız bir web hizmeti için iş yükü getirir. Bu web hizmetini sonuçta kırpma işi gönderir veya hesap kimlik bilgilerini kullanarak medya dosyalarınızı Azure Media Services filtresi oluşturma çağrısına Hizmetleri.

Aşağıdaki kod örneği bir örnek göstermektedir **submitSubclipCallback** yöntemi. Bu yöntemde, Media Encoder Standard kodlama Önayarı, HTTP POST uygulayın. POST başarılı olduysa (**sonucu**), **Promise** , aksi takdirde, bu hata ayrıntılarıyla reddedildi çözümlenir.

```javascript
// Submit Subclip Callback
//
// Parameter:
// - subclip: object that represents the subclip (output contract).
//
// Returns: a Promise object that, when resolved, returns true if the operation was accept in the back-end; otherwise, returns false.
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
İş gönderme çıktısını işlenen iş için Media Encoder Standard kodlama Önayarı ya da dinamik bildirim filtreleri için REST API yükü var.

## <a name="submitting-encoding-job-to-create-video"></a>Görüntü oluşturmak için kodlama işi gönderiliyor
Çerçeve doğru video klip oluşturma, Media Encoder Standard bir kodlama iş gönderebilirsiniz. Videoları işlenen iş üretim kodlama, yeni bir MP4 dosyasını parçalanmış.

Proje çıkış sözleşme işlenmiş kırpma için aşağıdaki özelliklere sahip bir JSON nesnesidir:

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
    /* NOTE: This is the preset for the Media Encoder Standard (MES) processor that can be used in the back-end to submit the subclip job.
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

Kodlama işi gerçekleştirmek göndermek için Media Encoder Standard ile ilişkili kodlama işi hazır. Kodlama gönderme hakkında bilgi için bu makalenin işleri görmek [.NET SDK'sı](https://docs.microsoft.com/azure/media-services/media-services-dotnet-encode-with-media-encoder-standard) veya [REST API](https://docs.microsoft.com/azure/media-services/media-services-rest-encode-asset).

## <a name="quickly-creating-video-clips-without-encoding"></a>Hızlı bir şekilde kodlama içermeyen video klip oluşturma
Bir kodlama işi oluşturmaya alternatif, dinamik bildirim filtreleri oluşturmak için Azure Media Clipper'ı kullanabilirsiniz. Filtreler kodlama gerektirmeyen ve hızlı bir şekilde yeni bir varlık değil oluşturulurken oluşturulabilir. Çıkış sözleşmenin bir filtre kırpma için aşağıdaki özelliklere sahip bir JSON nesnesidir:

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

İlişkilendirilen filtre yükü kullanarak dinamik bildirim filtresi oluşturmak için REST çağrısı göndermek için Gönder [REST API](https://docs.microsoft.com/azure/media-services/media-services-rest-dynamic-manifest).
