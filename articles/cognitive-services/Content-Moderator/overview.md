---
title: Content Moderator nedir?
titlesuffix: Azure Cognitive Services
description: Kullanıcıların oluşturduğu içeriği izlemek, işaretlemek, değerlendirmek ve uygunsuz içeriği filtrelemek için Content Moderator kullanmayı öğrenin.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: overview
ms.date: 06/15/2017
ms.author: sajagtap
ms.openlocfilehash: e109376f47d921fb18d7bb9a6252e80315419ec0
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47226050"
---
# <a name="what-is-content-moderator"></a>Content Moderator nedir?

İçerik denetleme; rahatsız edici olabilecek, istenmeyen ve riskli içeriği izleme işlemidir. Denetlenen içerik görüntü, metin veya video olabilir.

## <a name="where-it-is-used"></a>Kullanıldığı yerler

Aşağıdaki liste, Content Moderator'ın kullanıldığı bazı örnek senaryoları göstermektedir:

- Ürün kataloglarını ve kullanıcıların oluşturduğu içeriği denetleyen çevrimiçi marketler
- Kullanıcıların oluşturduğu yapıtları ve sohbet odalarını denetleyen oyun şirketleri
- Kullanıcıların eklediği görüntüleri, metinleri ve videoları denetleyen sosyal mesajlaşma platformları
- İçerikleri için merkezi içerik denetimi uygulayan kurumsal medya şirketleri
- Öğrenciler ve eğitimciler için kötü olabilecek veya rahatsız edici içeriği filtreleyen K-12 eğitim çözümü sağlayıcılar

## <a name="what-it-includes"></a>Neleri içerir

Content Moderator; görüntü, metin ve video denetlemeye yardımcı olan birkaç Web hizmeti API'sinden ve döngüde insan araştıran yerleşik bir inceleme aracından oluşur.

![Content Moderator engelleme şeması](images/content-moderator-block-diagram.png)

## <a name="apis"></a>API'ler

Content Moderator aşağıdaki API'leri içerir:
  - [**Metin denetimi API'si**](text-moderation-api.md): Metinde olabilecek küfürlü, müstehcen, davetkar, rahatsız edici ve kişisel bilgileri (PII) taramak için bu API'yi kullanın.
  - [**Özel terim listesi API'si**](try-terms-list-api.md): Yerleşik terimlere ek olarak özel bir listedeki terimler ile eşleştirme için bu API'yi kullanın. Bu listeleri, ilkelerinize bağlı olarak içeriğe izin vermek ya da vermemek için kullanın.  
  - [**Görüntü denetimi API'si**](image-moderation-api.md): Görüntülerde yetişkinlere yönelik veya müstehcen görüntü taramak, Optik Karakter Tanıyıcı (OCR) ile görüntüde metin tanımak ve yüz algılamak için bu API'yi kullanın.
  - [**Özel görüntü listesi API'si**](try-image-list-api.md): Özel bir görüntü listesiyle, yeniden sınıflandırmanız gerekmeyen önceden tanımlı içerikle eşleştirme için bu API'yi kullanın.
  - [**Video denetimi API'si**](video-moderation-api.md): Yetişkinlere yönelik veya müstehcen olabilecek videoları taramak için bu API'yi kullanın.
  - [**Gözden geçirme API'leri**](try-review-api-job.md): İnceleme aracında döngüde insan araştırma iş akışları oluşturma ve otomatikleştirme için [İşler](try-review-api-job.md), [İncelemeler](try-review-api-review.md) ve [İş Akışı](try-review-api-workflow.md) işlemlerini kullanın.

## <a name="human-review-tool"></a>İnsan inceleme aracı

Content Moderator aboneliğinize yerleşik [insan inceleme aracı](Review-Tool-User-Guide/human-in-the-loop.md) dahildir. İnsan denetimcilerinizin karar alabilmesi amacıyla metinlerin, görüntülerin ve videoların incelemelerini oluşturmak için daha önce değinilen İnceleme API'sini kullanın.

![Content Moderator video inceleme aracı](images/video-review-default-view.png)

## <a name="next-steps"></a>Sonraki adımlar

Content Moderator'ı kullanmaya başlamak için [Hızlı Başlangıç](quick-start.md)'ı kullanın.
