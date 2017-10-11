---
title: "Medya Kodlayıcısı standart - Azure ile video kırpma | Microsoft Docs"
description: "Bu makalede, Medya Kodlayıcısı standart ile video kırpma gösterilmektedir."
services: media-services
documentationcenter: 
author: anilmur
manager: cfowler
editor: 
ms.assetid: 7628f674-2005-4531-8b61-d7a4f53e46ba
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/09/2017
ms.author: anilmur;juliako;
ms.openlocfilehash: 60d0ce14a271fcbe698559da95ca011cb888b221
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="crop-videos-with-media-encoder-standard"></a>Media Encoder Standard ile videoları kırpma
Giriş video kırpma için Medya Kodlayıcısı standart (MES) kullanabilirsiniz. Kırpma video çerçevesinde dikdörtgen penceresinin seçilmesi ve yalnızca o penceresi içinde piksel kodlama işlemidir. Aşağıdaki diyagramda, işlem göstermeye yardımcı olur.

![Bir video kırpma](./media/media-services-crop-video/media-services-crop-video01.png)

Yalnızca 4:3 penceresi veya 1440 x 1080 piksel içeren etkin video böylece çözünürlük 1920 x 1080 piksel (16:9 en boy oranını) olsa da, sol ve sağ tarafta, siyah çubukları (sütun kutuları) sahip bir video giriş olarak olduğunu varsayalım. 1440 x 1080 bölge kodlamak ve MES kırpma veya siyah çubuklar düzenlemek için kullanın.

Kodlama hazır kırpma parametreleri özgün giriş videoya uygulamak için MES kırpma bir ön-işleme, aşamadır. Bir sonraki aşama kodlamadır ve genişliği/yüksekliğinin ayarlarının uygulanacağı *önceden işlenen* video ve özgün video uygulanmaz. Önceden belirlenmiş ayarınız tasarlarken, aşağıdakileri yapmanız gerekir: (a) özgün giriş video dayalı kırpma parametreleri seçin ve (b) seçin, kırpılmış video dayalı ayarları kodlayın. Eşleşmiyorsa, kırpılmış video ayarlar kodlamak, beklediğiniz gibi çıkış olmaz.

[Aşağıdaki](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) konu ile MES bir kodlama işi oluşturma ve kodlama görev için önceden belirlenmiş bir özel belirtme gösterir. 

## <a name="creating-a-custom-preset"></a>Özel bir ön ayarı oluşturma
Aşağıdaki çizimde gösterilen örnekte:

1. Özgün giriş 1920 x 1080 olabilir
2. Giriş çerçevede ortalanmış 1440 x 1080, çıktısı kırpılmış gerekir
3. Bu (1920 – 1440) X uzaklığını anlamına gelir / 2 = 240 ve bir Y uzaklığı sıfır
4. Genişlik ve yükseklik kırpma dikdörtgenin 1440 ve 1080, sırasıyla olan
5. Kodla aşamasında sor üç katmanı oluşturmak için bu çözümleri 1440 x 1080 960 x 720 ve 480 x 360, sırasıyla olan

### <a name="json-preset"></a>JSON hazır
    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "Crop": {
                "X": 240,
                "Y": 0,
                "Width": 1440,
                "Height": 1080
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1440,
              "Height": 1080,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1250,
              "MaxBitrate": 1250,
              "BufferWindow": "00:00:05",
              "Width": 480,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            }
          ],
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
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


## <a name="restrictions-on-cropping"></a>Kırpma kısıtlamaları
Kırpma özelliği el ile olması amaçlanmıştır. Giriş videonuzun, ilgi çerçevelerini seçin, bu belirli video, vb. için ayarlanmış kodlama hazır belirlemek için kırpma dikdörtgeninin için uzaklık belirlemek için imleç Konumlandır sağlar uygun bir düzenleme aracı yüklemek için gerekir. Bu özellik gibi şeyleri etkinleştirmek üzere tasarlanmamıştır: otomatik algılama ve video giriş siyah Mektup Kutusu/posta kutusu kenarlıkları kaldırılması.

Aşağıdaki kısıtlamalar kırpma özelliği için geçerli. Bunlar karşılanmazsa görev kodla başarısız veya beklenmeyen bir çıktı oluşturmak.

1. Ortak ordinates ve kırpma dikdörtgen boyutunu içinde giriş video uyması gerekir
2. Yukarıda belirtildiği gibi genişliğini ve yüksekliğini kodla ayarlarında kırpılmış videoya karşılık gerekir
3. Kırpma yatay modunda yakalanan videolar için geçerlidir (yani dikey veya dikey modunda smartphone ile kaydedilen videolar uygulanamaz tutulan)
4. Kare piksel ile yakalanan aşamalı video ile en iyi şekilde çalışır

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a>Sonraki adım
AMS tarafından sunulan harika özellikler hakkında bilgi edinmenize yardımcı olmak üzere Azure Media Services'i öğrenme yolları bakın.  

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]
