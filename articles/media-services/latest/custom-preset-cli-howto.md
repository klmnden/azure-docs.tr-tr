---
title: Media Services v3 CLI - Azure kullanarak özel dönüştürme kodlama | Microsoft Docs
description: Bu konuda, Azure Media Services v3 CLI kullanarak özel bir dönüştürme kodlamak için nasıl kullanılacağı gösterilmektedir.
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
ms.openlocfilehash: 42b7c2d86525c428253137b424fe58bb61edba70
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65762019"
---
# <a name="how-to-encode-with-a-custom-transform---cli"></a>-CLI gibi özel bir dönüşüm ile kodlama

Azure Media Services ile kodlarken hızla biri olan sektördeki en iyi yöntemler üzerinde gösterildiği şekilde göre önerilen yerleşik hazır oluşturabileceğinize dair [akış dosyalarını](stream-files-cli-quickstart.md#create-a-transform-for-adaptive-bitrate-encoding) hızlı başlangıç. Ayrıca, özel bir senaryonuz ya da cihaz belirli gereksinimlerinizi hedeflemek için önceden de oluşturabilirsiniz.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Özel önayarların kullanılmasına oluştururken, aşağıdaki maddeler geçerlidir:

* Yükseklik ve genişlik AVC içeriğin tüm değerleri 4'ün katı olmalıdır.
* Azure Media Services v3 sürümünde, tüm kodlama bit hızlarına dönüştürme bit / saniye cinsindendir. Bu Önayarı kilobit/saniye birimi olarak kullanılan v2 Apı'lerimiz ile farklıdır. V2'de hızı (kilobit/saniye) 128 belirtilmemişse, örneğin, v3 sürümünde, 128000 için (bit/saniye) ayarlanır.

## <a name="prerequisites"></a>Önkoşullar 

[Bir Media Services hesabı oluşturma](create-account-cli-how-to.md). <br/>Kaynak grubu adı ve Media Services hesap adını hatırlamak emin olun. 

[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

## <a name="define-a-custom-preset"></a>Özel Önayar tanımlayın

Aşağıdaki örnek yeni bir dönüştürme istek gövdesi tanımlar. Bu dönüşüm kullanıldığında oluşturulan istiyoruz çıkışları birtakım tanımlarız. 

Bu örnekte biz ilk ses kodlama için bir AacAudio katmanı ve video kodlama için iki H264Video katmanları ekleyin. Böylece çıkış dosya adlarında kullanılabilir video katmanlar, biz etiketleri atayın. Ardından, küçük resimler de içerecek şekilde çıkış istiyoruz. Aşağıdaki örnekte biz görüntüleri giriş videosunun çözünürlüğünün % 50 ve üç zaman damgalarının - {% 25, % 50, 75} giriş videosunun uzunluğunu oluşturulan PNG biçiminde belirtin. Son olarak, biz çıkış dosyaları - bir video ve ses biçimi belirtin ve başka küçük resimleri için. Birden çok H264Layers sahibiz olduğundan, katman başına benzersiz adlar oluşturmak makroları gerekir. Biz kullanabilir bir `{Label}` veya `{Bitrate}` makrosu, bu örnek önceki gösterir.

Bu dönüşüm bir dosyaya kaydetmek için kullanacağız. Biz bu örnekte, dosya adı `customPreset.json`. 

```json
{
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

```

## <a name="create-a-new-transform"></a>Yeni dönüştürme oluşturma  

Bu örnekte, oluşturduğumuz bir **dönüştürme** daha önce tanımladığımız özel önayarın üzerinde temel alır. Dönüşüm oluştururken bir zaten mevcut değilse denetlemeniz gerekir. Dönüştürme varsa, yeniden kullanın. Aşağıdaki `show` komutu tarafından döndürülen `customTransformName` varsa dönüştürün:

```cli
az ams transform show -a amsaccount -g amsResourceGroup -n customTransformName
```

Aşağıdaki CLI komutunu (daha önce tanımlanan) özel önayarın üzerinde temel dönüşüm oluşturur. 

```cli
az ams transform create -a amsaccount -g amsResourceGroup -n customTransformName --description "Basic Transform using a custom encoding preset" --preset customPreset.json
```

Media Services'ın belirtilen video veya ses dönüştürme uygulamak bir iş dönüşüm altında göndermeniz gerekir. Dönüşüm altında bir iş göndermek nasıl oluşturulduğunu gösteren tam bir örnek için bkz. [hızlı başlangıç: Video dosyaları - CLI Stream](stream-files-cli-quickstart.md).

## <a name="see-also"></a>Ayrıca bkz.

[Azure CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
