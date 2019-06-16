---
title: İnsan tarafından İnceleme - Content Moderator ile video denetimi
titlesuffix: Azure Cognitive Services
description: Uygunsuz içeriği Orta için makine destekli video denetimi ve insan tarafından İnceleme araçları kullanın
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 04/30/2019
ms.author: sajagtap
ms.openlocfilehash: a6c467d3153400815e37a5d461766140abd1fa32
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65228121"
---
# <a name="video-moderation-with-human-review"></a>İnsan tarafından İnceleme ile video denetimi

Kullanım Content Moderator'ın makine destekli [video denetimi](video-moderation-api.md) ve [insan tarafından İnceleme aracı](Review-Tool-User-Guide/human-in-the-loop.md) Orta videoları ve dökümleri için yetişkin (açık) ve en iyi sonuçları almak için (müstehcen) müstehcen içerik işletmenizi.

## <a name="video-trained-classifier-preview"></a>Video eğitimli sınıflandırıcı (Önizleme)

Makine destekli video sınıflandırma ya da görüntü eğitilen modelleri veya video eğitilen modelleri ile elde edilir. Görüntü eğitimli video sınıflandırıcılar farklı olarak, Microsoft'un yetişkinlere yönelik ve müstehcen video sınıflandırıcı videolardaki eğitildi. Bu yöntem, daha iyi eşleşmenin kalitesini sonuçlanır.

## <a name="shot-detection"></a>Kare algılama

Sınıflandırma ayrıntılarını alırken ek video zeka videoları analiz daha fazla esneklik yardımcı olur. Çerçeve çıktısı yerine, Microsoft'un video denetimi hizmeti çok görüntüsü düzeyi bilgileri sağlar. Şimdi, görüntüsü ve çerçeve düzeyindeki videolarınızı analiz seçeneğiniz de vardır.

## <a name="key-frame-detection"></a>Anahtar kare algılama

Çerçeve düzenli aralıklarla çıktısı, yerine video denetimi hizmeti tanımlar ve yalnızca büyük olasılıkla tamamlandı (iyi) çerçeve çıkarır. Bu özellik, çerçeve düzeyi yetişkinlere yönelik ve müstehcen analizi için etkin çerçeve oluşturma sağlar.

Aşağıdaki Ayıkla kısmi yanıt olası görüntüleri, anahtar çerçeveler ve yetişkinlere yönelik ve müstehcen puanları gösterilmektedir:

```json
"fragments":[  
  {  
    "start":0,
    "duration":18000
  },
  {  
    "start":18000,
    "duration":3600,
    "interval":3600,
    "events":[  
      [  
        {  
          "reviewRecommended":false,
          "adultScore":0.00001,
          "racyScore":0.03077,
          "index":5,
          "timestamp":18000,
          "shotIndex":0
        }
      ]
    ]
  },
  {  
    "start":18386372,
    "duration":119149,
    "interval":119149,
    "events":[  
      [  
        {  
          "reviewRecommended":true,
          "adultScore":0.00000,
          "racyScore":0.91902,
          "index":5085,
          "timestamp":18386372,
          "shotIndex":62
        }
      ]
    ]
```

## <a name="visualization-for-human-reviews"></a>İncelemelere için görselleştirme

Daha fazla bilgi için ince durumlarda, işletmeler, video, çerçeveler ve makine atanan etiketleri işlemeye yönelik bir insan tarafından İnceleme çözümü gerekir. Videolar ve çerçeveleri gözden geçirme İnsan Moderatörler öngörüleri eksiksiz bir görünümünü elde, etiketleri değiştirmek ve kararlarına gönderin.

![Video gözden geçirme aracı varsayılan görünüm](images/video-review-default-view.png)

## <a name="player-view-for-video-level-review"></a>Video düzeyinde gözden geçirme için Player görüntüle

Video düzeyinde ikili kararları olası yetişkinlere yönelik ve müstehcen çerçeveleri gösteren bir video oynatıcı görünümü ile yapılabilir. Video Sahne incelemek için çeşitli hızı seçenekleri ile İnsan gözden geçirenler gidin. Bunlar, etiketler kapatarak kararlarına onaylayın.

![Video gözden geçirme aracı player görüntüle](images/video-review-player-view.PNG)

## <a name="frames-view-for-detailed-reviews"></a>Çerçeve görünüm ayrıntılı incelemeleri

Çerçeve çerçeve analizi için ayrıntılı bir video İnceleme çerçeve tabanlı bir görünümle mümkün olur. İnsan gözden geçirenler, gözden geçirin ve bir veya daha fazla kareyi seçin ve etiketleri kararlarına onaylamak için Değiştir. İsteğe bağlı bir sonraki adım, rahatsız edici çerçeveleri veya içerik Redaksiyon olacaktır.

![Video gözden geçirme aracı çerçeveleri görüntülemek](images/video-review-frames-view-apply-tags.PNG)

## <a name="transcript-moderation"></a>Transkripti denetleme

Videoları genellikle ses üzerinden denetimi de uygunsuz konuşma gereken sahiptir. Konuşmayı metne dönüştürün ve gözden geçirme aracı içindeki metin denetimi için döküm göndermek için Content Moderator'ın gözden geçirme API kullanmak için Azure Media Indexer hizmeti kullanın.

![Video gözden geçirme aracı döküm görüntüle](images/video-review-transcript-view.png)

## <a name="next-steps"></a>Sonraki adımlar

- Kullanmaya başlama [video denetimi hızlı](video-moderation-api.md).
- Nasıl oluşturacağınızı öğrenin [video incelemeleri](video-reviews-quickstart-dotnet.md) İnsan gözden geçirenler, denetlenen çıktısından için.
- Ekleme [video deşifre metni inceler](video-transcript-reviews-quickstart-dotnet.md) video, incelemeleri için.
- Ayrıntılı öğretici nasıl geliştirebileceğinizi kullanıma bir [tamamlamak video denetimi çözümü](video-transcript-moderation-review-tutorial-dotnet.md).