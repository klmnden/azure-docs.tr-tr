---
title: Media Services v3 REST - Azure'ı kullanarak özel dönüştürme kodlama | Microsoft Docs
description: Bu konuda, Azure Media Services v3 REST kullanarak özel bir dönüştürme kodlamak için nasıl kullanılacağı gösterilmektedir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 05/14/2019
ms.author: juliako
ms.openlocfilehash: 30e22cb786e5dc2a667fe41ca8edf398cf0b7613
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65761804"
---
# <a name="how-to-encode-with-a-custom-transform---rest"></a>-REST gibi özel bir dönüşüm ile kodlama

Azure Media Services ile kodlarken hızla biri olan sektördeki en iyi yöntemler üzerinde gösterildiği şekilde göre önerilen yerleşik hazır oluşturabileceğinize dair [akış dosyalarını](stream-files-tutorial-with-rest.md#create-a-transform) öğretici. Ayrıca, özel bir senaryonuz ya da cihaz belirli gereksinimlerinizi hedeflemek için önceden de oluşturabilirsiniz.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Özel önayarların kullanılmasına oluştururken, aşağıdaki maddeler geçerlidir:

* Yükseklik ve genişlik AVC içeriğin tüm değerleri 4'ün katı olmalıdır.
* Azure Media Services v3 sürümünde, tüm kodlama bit hızlarına dönüştürme bit / saniye cinsindendir. Bu Önayarı kilobit/saniye birimi olarak kullanılan v2 Apı'lerimiz ile farklıdır. V2'de hızı (kilobit/saniye) 128 belirtilmemişse, örneğin, v3 sürümünde, 128000 için (bit/saniye) ayarlanır.

## <a name="prerequisites"></a>Önkoşullar 

- [Bir Media Services hesabı oluşturma](create-account-cli-how-to.md). <br/>Kaynak grubu adı ve Media Services hesap adını hatırlamak emin olun. 
- [Azure Media Services REST API çağrıları için Postman yapılandırma](media-rest-apis-with-postman.md).<br/>Konunun son adımı takip edin [Azure AD belirteci Al](media-rest-apis-with-postman.md#get-azure-ad-token). 

## <a name="define-a-custom-preset"></a>Özel Önayar tanımlayın

Aşağıdaki örnek yeni bir dönüştürme istek gövdesi tanımlar. Bu dönüşüm kullanıldığında oluşturulan istiyoruz çıkışları birtakım tanımlarız. 

Bu örnekte biz ilk ses kodlama için bir AacAudio katmanı ve video kodlama için iki H264Video katmanları ekleyin. Böylece çıkış dosya adlarında kullanılabilir video katmanlar, biz etiketleri atayın. Ardından, küçük resimler de içerecek şekilde çıkış istiyoruz. Aşağıdaki örnekte biz görüntüleri giriş videosunun çözünürlüğünün % 50 ve üç zaman damgalarının - {% 25, % 50, 75} giriş videosunun uzunluğunu oluşturulan PNG biçiminde belirtin. Son olarak, biz çıkış dosyaları - bir video ve ses biçimi belirtin ve başka küçük resimleri için. Birden çok H264Layers sahibiz olduğundan, katman başına benzersiz adlar oluşturmak makroları gerekir. Biz kullanabilir bir `{Label}` veya `{Bitrate}` makrosu, bu örnek önceki gösterir.

```json
{
    "properties": {
        "description": "Basic Transform using a custom encoding preset",
        "outputs": [
            {
                "onError": "StopProcessingJob",
                "relativePriority": "Normal",
                "preset": {
                    "@odata.type": "#Microsoft.Media.StandardEncoderPreset",
                    "codecs": [
                        {
                            "@odata.type": "#Microsoft.Media.AacAudio",
                            "channels": 2,
                            "samplingRate": 48000,
                            "bitrate": 128000,
                            "profile": "AacLc"
                        },
                        {
                            "@odata.type": "#Microsoft.Media.H264Video",
                            "keyFrameInterval": "PT2S",
                            "stretchMode": "AutoSize",
                            "sceneChangeDetection": false,
                            "complexity": "Balanced",
                            "layers": [
                                {
                                    "width": "1280",
                                    "height": "720",
                                    "label": "HD",
                                    "bitrate": 3400000,
                                    "maxBitrate": 3400000,
                                    "bFrames": 3,
                                    "slices": 0,
                                    "adaptiveBFrame": true,
                                    "profile": "Auto",
                                    "level": "auto",
                                    "bufferWindow": "PT5S",
                                    "referenceFrames": 3,
                                    "entropyMode": "Cabac"
                                },
                                {
                                    "width": "640",
                                    "height": "360",
                                    "label": "SD",
                                    "bitrate": 1000000,
                                    "maxBitrate": 1000000,
                                    "bFrames": 3,
                                    "slices": 0,
                                    "adaptiveBFrame": true,
                                    "profile": "Auto",
                                    "level": "auto",
                                    "bufferWindow": "PT5S",
                                    "referenceFrames": 3,
                                    "entropyMode": "Cabac"
                                }
                            ]
                        },
                        {
                            "@odata.type": "#Microsoft.Media.PngImage",
                            "stretchMode": "AutoSize",
                            "start": "25%",
                            "step": "25%",
                            "range": "80%",
                            "layers": [
                                {
                                    "width": "50%",
                                    "height": "50%"
                                }
                            ]
                        }
                    ],
                    "formats": [
                        {
                            "@odata.type": "#Microsoft.Media.Mp4Format",
                            "filenamePattern": "Video-{Basename}-{Label}-{Bitrate}{Extension}",
                            "outputFiles": []
                        },
                        {
                            "@odata.type": "#Microsoft.Media.PngFormat",
                            "filenamePattern": "Thumbnail-{Basename}-{Index}{Extension}"
                        }
                    ]
                }
            }
        ]
    }
}

```

## <a name="create-a-new-transform"></a>Yeni dönüştürme oluşturma  

Bu örnekte, oluşturduğumuz bir **dönüştürme** daha önce tanımladığımız özel önayarın üzerinde temel alır. Dönüşüm oluştururken ilk kullanmalısınız [alma](https://docs.microsoft.com/rest/api/media/transforms/get) biri zaten mevcut olup olmadığını denetlemek için. Dönüştürme varsa, yeniden kullanın. 

İndirdiğiniz Postman'ın koleksiyonda seçin **dönüşümler ve işler**->**oluşturma veya güncelleştirme dönüştürme**.

**PUT** HTTP istek yöntemine benzerdir:

```
PUT https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/transforms/:transformName?api-version={{api-version}}
```

Seçin **gövdesi** sekmesi ve Değiştir gövdesi json kodunu [daha önce tanımlanan](#define-a-custom-preset). Media Services'ın belirtilen video veya ses dönüştürme uygulamak bir iş dönüşüm altında göndermeniz gerekir.

**Gönder**’i seçin. 

Media Services'ın belirtilen video veya ses dönüştürme uygulamak bir iş dönüşüm altında göndermeniz gerekir. Dönüşüm altında bir iş göndermek nasıl oluşturulduğunu gösteren tam bir örnek için bkz. [Öğreticisi: Video dosyaları - REST Stream](stream-files-tutorial-with-rest.md).

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [diğer REST işlemlerini](https://docs.microsoft.com/rest/api/media/)
