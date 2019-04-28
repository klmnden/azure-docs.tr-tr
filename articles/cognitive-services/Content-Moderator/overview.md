---
title: Azure Content Moderator nedir?
titlesuffix: Azure Cognitive Services
description: Kullanıcıların oluşturduğu içeriği izlemek, işaretlemek, değerlendirmek ve uygunsuz malzemeleri filtrelemek için Content Moderator kullanmayı öğrenin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: overview
ms.date: 02/20/2019
ms.author: pafarley
ms.openlocfilehash: 440471acb6e122bf25ba21b0ab3b5a2f7d9b021d
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62129443"
---
# <a name="what-is-azure-content-moderator"></a>Azure Content Moderator nedir?

Azure Content Moderator API'si rahatsız edici, riskli veya istenmeyen olabilecek öğeleri tespit etme amacıyla metin, görüntü ve video içeriğinde denetim gerçekleştiren bir bilişsel hizmettir. Hizmet, böyle bir öğeyle karşılaştığında içeriğe uygun etiketler (bayraklar) ekler. Uygulamanız da bu bayraklı içeriği dikkate alarak düzenlemelere uygunluk ya da kullanıcılar için istenen ortamın oluşturulması amacıyla işlem gerçekleştirebilir. Farklı içerik bayraklarının anlamları hakkında daha fazla bilgi edinmek için [Content Moderator API'leri](#content-moderator-apis) bölümüne bakın.

## <a name="where-it-is-used"></a>Kullanıldığı yerler

Aşağıda bir yazılım geliştirme uzmanının veya ekibinin Content Moderator özelliklerinden faydalanmak isteyebileceği birkaç senaryo verilmiştir:

- Ürün kataloglarını ve kullanıcıların oluşturduğu içerikleri denetleyen çevrimiçi marketler
- Kullanıcıların oluşturduğu yapıtları ve sohbet odalarını denetleyen oyun şirketleri
- Kullanıcıların eklediği görüntüleri, metinleri ve videoları denetleyen sosyal mesajlaşma platformları
- İçerikleri için merkezi denetim uygulayan kurumsal medya şirketleri
- Öğrenciler ve eğitimciler için uygunsuz içeriği filtreleyen K-12 eğitim çözümü sağlayıcıları

## <a name="what-it-includes"></a>Neleri içerir

Content Moderator hizmeti hem REST çağrıları hem de .NET SDK'sı ile sunulan çeşitli web hizmeti API'lerinden oluşur. Ayrıca hizmete yardımcı olmak veya denetleme işlevinde ince ayar yapmak için insanlar tarafından gerçekleştirilen inceleme hizmeti de sunar.

![Denetleme API'lerini, Gözden Geçirme API'lerini ve insan inceleme aracını gösteren Content Moderator blok diyagramı](images/content-moderator-block-diagram.png)

### <a name="content-moderator-apis"></a>Content Moderator API'leri

Content Moderator hizmeti aşağıdaki senaryolara hitap eden API'lere sahiptir.

| API grubu | Açıklama |
| ------ | ----------- |
|[**Metin denetimi**](text-moderation-api.md)| Metin rahatsız edici içerik, cinsel açık veya müstehcen içerik, küfür ve kişisel verileri tarar.|
|[**Özel terim listeleri**](try-terms-list-api.md)| Metinleri yerleşik terimlere ek olarak özel terim listesine göre tarar. İçerik ilkelerinize göre içerik engellemek veya izin vermek için özel listeler kullanabilirsiniz.|  
|[**Görüntü denetimi**](image-moderation-api.md)| Görüntülerde yetişkinlere yönelik veya müstehcen görüntü taraması yapar, Optik Karakter Tanıyıcı (OCR) ile görüntüdeki metinleri tanır ve yüzleri algılar.|
|[**Özel görüntü listeleri**](try-image-list-api.md)| Görüntüleri özel görüntü listesine göre tarar. Özel görüntü listelerini kullanarak tekrar tekrar sınıflandırmak istemediğiniz yaygın içerik örneklerini filtreleyebilirsiniz.|
|[**Video denetimi**](video-moderation-api.md)| Videolarda yetişkinlere yönelik veya müstehcen içerik taraması yapar ve bu içeriklerle ilgili zaman işaretçilerini döndürür.|
|[**API'leri İnceleme**](try-review-api-job.md)| İnsan inceleme aracında döngüde insan araştırma iş akışları oluşturma ve otomatikleştirme için [İşler](try-review-api-job.md), [İncelemeler](try-review-api-review.md) ve [İş Akışı](try-review-api-workflow.md) işlemlerini kullanın. İş akışı API henüz .NET SDK'yı kullanılamıyor.|

### <a name="review-tool"></a>Gözden geçirme aracı

Content Moderator hizmet ayrıca web tabanlı içerir [gözden geçirme aracı](Review-Tool-User-Guide/human-in-the-loop.md), içeriği barındıran işlemek İnsan Moderatörler gözden geçirir. İnsan girişi hizmeti eğitmez ancak hizmetin ve insanlardan oluşan ekibin birlikte kullanılması, geliştiricilerin verimlilik ve doğruluk arasındaki doğru dengeyi yakalamasını sağlar. İnceleme aracını da kullanıcı dostu bir sağlayan çeşitli Content Moderator kaynaklar için ön uç.

![Content Moderator insan inceleme aracı giriş sayfası](images/homepage.PNG)

## <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği

Olarak tüm Bilişsel hizmetler, Content Moderator hizmeti kullanan geliştiricilerin Microsoft'un müşteri verilerini ilkelerinin bilmeniz gerekir. Bkz: [Bilişsel Hizmetler sayfasına](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) daha fazla bilgi için Microsoft Trust Center.

## <a name="next-steps"></a>Sonraki adımlar

' Ndaki yönergeleri takip ederek Content Moderator hizmetini kullanmaya başlama [deneyin Content Moderator Web'de](quick-start.md).