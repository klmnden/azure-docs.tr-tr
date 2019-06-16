---
title: Media Encoder Standard - Azure ile videoları kırpma | Microsoft Docs
description: Bu makalede, Media Encoder Standard ile videoları kırpma gösterilmektedir.
services: media-services
documentationcenter: ''
author: anilmur
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/18/2019
ms.author: anilmur;juliako;
ms.openlocfilehash: 9a81050fca935f688f2ff58cb04a148bf676f04b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61217216"
---
# <a name="crop-videos-with-media-encoder-standard"></a>Media Encoder Standard ile videoları kırpma  

Giriş video kırpmak için medya Kodlayıcı standart (MES) kullanabilirsiniz. Kırpma video çerçevesinde dikdörtgen bir pencere seçmek ve yalnızca söz konusu pencerede piksel kodlama işlemidir. Aşağıdaki diyagram işlemin açıklanmasına yardımcı olur.

![Videoyu Kırp](./media/media-services-crop-video/media-services-crop-video01.png)

Yalnızca 4:3 pencere veya 1440 x 1080 piksel içeren etkin video, giriş olarak bir çözünürlük 1920 x 1080 piksel (en boy oranı 16:9), ancak, sağ ve sol siyah çubukları (pillar kutuları) sahip bir video olduğunu varsayalım. Kırpma veya siyah çubuklar düzenlemek için MES kullanma ve 1440 x 1080 bölge kodlayın.

Özgün giriş video kodlama Önayarı kırpma parametrelerinde uygulamak için bir ön işleme aşamayı MES kırpma ifade eder. Kodlama sonraki bir aşamada ve genişliği/yüksekliğinin ayarlarının uygulanacağı *ön işlemden* video ve özgün video uygulanmaz. Önceden ayarlanmış tasarlarken aşağıdakileri yapmanız gerekir: (a) özgün giriş video temel alarak kırpma parametreler ve (b) seçin, ayarlarına kırpılmış video kodlayın. Eşleşmemesi durumunda, ayarları kırpılmış video kodlayın, beklediğiniz gibi çıkış olmayacak.

[Aşağıdaki](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) konu MES ile bir kodlama işi oluşturma ve özel bir kodlama görevi için önceden belirlemek nasıl gösterir. 

## <a name="creating-a-custom-preset"></a>Özel Önayar oluşturma
Aşağıdaki diyagramda gösterilen örnekte:

1. 1920 x 1080 özgün giriştir
2. Bir çıktısına giriş çerçevede ortalanır 1440 x 1080, kırpılmış gerekir
3. Bu, bir X uzaklığı (1920 – 1440) anlamına gelir / 2 = 240 ve bir Y sıfır uzaklığı
4. Kırpma dikdörtgenin yüksekliğini ve genişliğini 1440 ve 1080, sırasıyla
5. Kodla aşama sor üç katmanı oluşturmak için çözünürlük 1440 x 1080 960 x 720 ve 480 x 360, sırasıyla

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


## <a name="restrictions-on-cropping"></a>Kırpma ile ilgili kısıtlamalar
Kırpma özelliği, el ile olacak şekilde tasarlanmıştır. Girdi videonuzun ilgi çerçeve seçin, bu belirli bir video, vb. için ayarlanan kodlama Önayarı belirlemek için kırpma dikdörtgenini için uzaklık belirlemek için imleci yerleştirin sağlayan uygun bir düzenleme aracı yüklemek gerekecektir. Bu özellik gibi şeyleri etkinleştirmek için tasarlanmamıştır: otomatik algılama ve kaldırma girişinizi video siyah sinemaskop/posta kutusu kenarlıklarının.

Kırpma özelliği aşağıdaki kısıtlamalar uygulanır. Bunlar karşılanmazsa görev kodla başarısız veya beklenmeyen bir çıktı üretir.

1. Kırpma dikdörtgenini boyutunu ve ortak ordinates içinde giriş videosunun uyması gerekir
2. Yukarıda belirtildiği gibi kırpılmış videoyu karşılık olarak genişliğini ve yüksekliğini kodla ayarlarında sahip
3. Yatay modda yakalanan videoları kırpma uygulanır (yani dikey ya da dikey modda bir akıllı ile kaydedilen videoları uygulanamaz tutulan)
4. Yakalanan kare piksel aşamalı video ile en iyi şekilde çalışır.

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a>Sonraki adım
AMS tarafından sunulan harika özellikler hakkında bilgi edinmenize yardımcı olacak Azure Media Services'i öğrenme yolları bakın.  

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]
