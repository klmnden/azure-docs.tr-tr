---
title: Azure Content Moderator nedir?
titlesuffix: Azure Cognitive Services
description: Kullanıcıların oluşturduğu içeriği izlemek, işaretlemek, değerlendirmek ve uygunsuz malzemeleri filtrelemek için Content Moderator kullanmayı öğrenin.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: overview
ms.date: 10/05/2018
ms.author: sajagtap
ms.openlocfilehash: 5756e8fb451b073c68271359848ab27373ad85ed
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2018
ms.locfileid: "49309561"
---
# <a name="what-is-content-moderator"></a>Content Moderator nedir?

İçerik denetleme rahatsız edici, istenmeyen veya riskli olabilecek malzemeleri tespit etme amacıyla metin, görüntü veya video içeriğinin izlenmesidir. İşaretlenen içerik gizlenebilir veya düzenlemelere uygunluk ya da kullanıcılar için istenen ortamın oluşturulması amacıyla işlem gerçekleştirilebilir.

## <a name="where-it-is-used"></a>Kullanıldığı yerler

Aşağıdaki liste, Content Moderator'ın kullanılabileceği bazı örnek senaryoları göstermektedir:

- Ürün kataloglarını ve kullanıcıların oluşturduğu içeriği denetleyen çevrimiçi marketler
- Kullanıcıların oluşturduğu yapıtları ve sohbet odalarını denetleyen oyun şirketleri
- Kullanıcıların eklediği görüntüleri, metinleri ve videoları denetleyen sosyal mesajlaşma platformları
- İçerikleri için merkezi içerik denetimi uygulayan kurumsal medya şirketleri
- Öğrenciler ve eğitimciler için uygunsuz veya rahatsız edici içeriği filtreleyen K-12 eğitim çözümü sağlayıcılar

## <a name="what-it-includes"></a>Neleri içerir

Content Moderator; görüntü, metin ve video denetlemeye yardımcı olan birkaç Web hizmeti API'sinden ve döngüde insan araştıran yerleşik bir inceleme aracından oluşur.

![Content Moderator engelleme şeması](images/content-moderator-block-diagram.png)

### <a name="apis"></a>API'ler

Content Moderator hizmeti aşağıdaki API'leri içerir:
  - [**Metin denetimi API'si**](text-moderation-api.md): Metinde olabilecek küfürlü, müstehcen, davetkar, rahatsız edici ve kişisel bilgileri (PII) taramak için bu API'yi kullanın.
  - [**Özel terim listesi API'si**](try-terms-list-api.md): Yerleşik terimlere ek olarak özel bir listedeki terimler ile eşleştirme için bu API'yi kullanın. Bu listeleri, ilkelerinize bağlı olarak içeriğe izin vermek ya da vermemek için kullanın.  
  - [**Görüntü denetimi API'si**](image-moderation-api.md): Görüntülerde yetişkinlere yönelik veya müstehcen görüntü taramak, Optik Karakter Tanıyıcı (OCR) ile görüntüde metin tanımak ve yüz algılamak için bu API'yi kullanın.
  - [**Özel görüntü listesi API'si**](try-image-list-api.md): Özel bir görüntü listesiyle, yeniden sınıflandırmanız gerekmeyen önceden tanımlı içerikle eşleştirme için bu API'yi kullanın.
  - [**Video denetimi API'si**](video-moderation-api.md): Yetişkinlere yönelik veya müstehcen olabilecek videoları taramak için bu API'yi kullanın.
  - [**Gözden geçirme API'leri**](try-review-api-job.md): İnceleme aracında döngüde insan araştırma iş akışları oluşturma ve otomatikleştirme için [İşler](try-review-api-job.md), [İncelemeler](try-review-api-review.md) ve [İş Akışı](try-review-api-workflow.md) işlemlerini kullanın.

### <a name="human-review-tool"></a>İnsan inceleme aracı

Content Moderator aboneliğinize yerleşik [insan inceleme aracı](Review-Tool-User-Guide/human-in-the-loop.md) dahildir. İnsan denetimcilerinizin karar alabilmesi amacıyla metinlerin, görüntülerin ve videoların incelemelerini oluşturmak için daha önce değinilen İnceleme API'sini kullanın.

![Content Moderator video inceleme aracı](images/video-review-default-view.png)

## <a name="next-steps"></a>Sonraki adımlar

Content Moderator'ı kullanmaya başlamak için [Hızlı Başlangıç](quick-start.md)'ı kullanın.
