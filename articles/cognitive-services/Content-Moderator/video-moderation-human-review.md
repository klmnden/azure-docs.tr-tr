---
title: Azure içerik aracı - Video denetleme | Microsoft Docs
description: Uygunsuz içeriği Orta için makine destekli video denetleme ve İnsan gözden geçirme Araçları'nı kullanın
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/20/2018
ms.author: sajagtap
ms.openlocfilehash: fb26c9af55381c80a3f520b1a0068d8f72c91061
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351532"
---
# <a name="video-moderation"></a>Video denetimi

Kullanım içerik denetleyicinin makine destekli [video yönetimini](video-moderation-api.md) ve [İnsan gözden geçirme aracı](Review-Tool-User-Guide/human-in-the-loop.md) Orta videolar ve dökümleri yetişkin (açık) için ve saldırganlardan (müstehcen) içerik için en iyi sonuçları almak için işinizi.

## <a name="video-trained-classifier"></a>Video eğitilmiş sınıflandırıcı

Makine destekli video sınıflandırma ya da görüntü eğitilmiş modeller veya video eğitilmiş modeller ile elde edilir. Görüntüyü eğitim almış video sınıflandırıcı, Microsoft'un yetişkin ve saldırganlardan video sınıflandırıcı ile video eğitildi. Bu yöntem daha iyi bir eşleşme kalitesi sonuçlanır.

## <a name="shot-detection"></a>Kare algılama

Sınıflandırma Ayrıntılar alırken ek video gösterimi videolar çözümleme daha fazla esneklik yardımcı olur. Yalnızca çerçeve çıktısı yerine, Microsoft'un video denetleme hizmeti çok görüntüsü düzeyi bilgileri sağlar. Şimdi, videolarınızı görüntüsü ve çerçeve düzeyindeki çözümlemek için seçeneğiniz de vardır.
 
## <a name="key-frame-detection"></a>Anahtar çerçeve algılama

Çerçeve düzenli aralıklarla çıktısı, yerine video denetleme hizmeti tanımlar ve yalnızca büyük olasılıkla tam (iyi) çerçeveleri çıkarır. Bu özellik etkili çerçeve oluşturma çerçeve düzeyi yetişkin ve saldırganlardan analiz için sağlar.

Aşağıdaki Ayıkla bir kısmi yanıt olası görüntüleri, anahtar çerçeveler ve yetişkin ve saldırganlardan puanları gösterir:

    "fragments": [
    {
      "start": 0,
      "duration": 18000
    },
    {
      "start": 18000,
      "duration": 3600,
      "interval": 3600,
      "events": [
        [
          {
            "reviewRecommended": false,
            "adultScore": 0.00001,
            "racyScore": 0.03077,
            "index": 5,
            "timestamp": 18000,
            "shotIndex": 0
          }
        ]
      ]
    },
    {
      "start": 18386372,
      "duration": 119149,
      "interval": 119149,
      "events": [
        [
          {
            "reviewRecommended": true,
            "adultScore": 0.00000,
            "racyScore": 0.91902,
            "index": 5085,
            "timestamp": 18386372,
            "shotIndex": 62
          }
        ]
      ]


## <a name="visualization-for-human-reviews"></a>İnsan incelemeler için görselleştirme

Daha fazla bilgi için nuanced durumlarda, işletmelerin video, çerçeve ve makine atanmış etiketleri işlemeye yönelik İnsan gözden geçirme çözüm gerekir. Videolar ve çerçeveleri gözden geçirme İnsan araburucu öngörüleri eksiksiz bir görünümünü elde, etiketleri değiştirmek ve kararlarına gönderin.

![Video gözden geçirme aracı varsayılan görünüm](images/video-review-default-view.png)

## <a name="player-view-for-video-level-review"></a>Video düzeyi gözden geçirme için Player görünümü

Video düzeyi ikili kararları olası yetişkin ve saldırganlardan çerçeveleri gösteren bir video oynatıcı görünümü ile mümkün hale getirilir. İnsan gözden geçirenler video planda incelemek için çeşitli hızı seçenekleri ile gidin. Bunlar, kararlarına etiketler değiştirerek onaylayın.

![Video gözden geçirme aracı player görünümü](images/video-review-player-view.PNG)

## <a name="frames-view-for-detailed-reviews"></a>Çerçeve görünüm için ayrıntılı incelemeleri

Kare kare çözümleme için ayrıntılı bir video incelemesi çerçeve tabanlı bir görünümle mümkün hale getirilir. İnsan gözden geçirenler gözden geçirin ve bir veya daha fazla çerçevelerini seçin ve kararlarına onaylamak için etiketler Değiştir. İsteğe bağlı bir sonraki adımı Redaksiyon rahatsız edici çerçeveleri veya içeriği oluşturur.

![Video gözden geçirme aracı çerçeve Görünüm](images/video-review-frames-view-apply-tags.PNG)

## <a name="transcript-moderation"></a>Dökümü denetleme

Videolar genellikle sesli üzerinden yönetimini de rahatsız edici konuşma gereken vardır. Konuşma metne dönüştürmek ve gözden geçirme Aracı'nı içinde metin denetleme dökümü göndermek için içerik denetleyicinin gözden geçirme API kullanmak için Azure Media Indexer hizmeti kullanın.

![Video gözden geçirme aracı dökümü görünümü](images/video-review-transcript-view.png)

## <a name="next-steps"></a>Sonraki adımlar

Kullanmaya başlama [video yönetimini quickstart](video-moderation-api.md). 

Nasıl oluşturulacağını öğrenin [video incelemeleri](video-reviews-quickstart-dotnet.md) , aracılı çıktısından İnsan gözden geçirenlerin için.

Ekleme [video dökümü incelemeleri](video-transcript-reviews-quickstart-dotnet.md) video incelemelere.

Geliştirme konusunda ayrıntılı öğretici kullanıma bir [tamamlamak video yönetimini çözüm](video-transcript-moderation-review-tutorial-dotnet.md). 
