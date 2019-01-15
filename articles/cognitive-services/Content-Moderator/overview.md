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
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: f7fef00cfff9295036d7545470f86e27314e6451
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2019
ms.locfileid: "54258707"
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

| Eylem | Açıklama |
| ------ | ----------- |
|[**Metin denetimi**](text-moderation-api.md)| Metinlerde rahatsız edici içerik, cinsel veya müstehcen içerik, küfür ve kişisel bilgi taraması gerçekleştirir.|
|[**Özel terim listeleri**](try-terms-list-api.md)| Metinleri yerleşik terimlere ek olarak özel terim listesine göre tarar. İçerik ilkelerinize göre içerik engellemek veya izin vermek için özel listeler kullanabilirsiniz.|  
|[**Görüntü denetimi**](image-moderation-api.md)| Görüntülerde yetişkinlere yönelik veya müstehcen görüntü taraması yapar, Optik Karakter Tanıyıcı (OCR) ile görüntüdeki metinleri tanır ve yüzleri algılar.|
|[**Özel görüntü listeleri**](try-image-list-api.md)| Görüntüleri özel görüntü listesine göre tarar. Özel görüntü listelerini kullanarak tekrar tekrar sınıflandırmak istemediğiniz yaygın içerik örneklerini filtreleyebilirsiniz.|
|[**Video denetimi**](video-moderation-api.md)| Videolarda yetişkinlere yönelik veya müstehcen içerik taraması yapar ve bu içeriklerle ilgili zaman işaretçilerini döndürür.|
|[**İnceleme**](try-review-api-job.md)| İnsan inceleme aracında döngüde insan araştırma iş akışları oluşturma ve otomatikleştirme için [İşler](try-review-api-job.md), [İncelemeler](try-review-api-review.md) ve [İş Akışı](try-review-api-workflow.md) işlemlerini kullanın. İş Akışı API'si henüz .NET SDK üzerinden kullanılabilir durumda değildir.|

### <a name="human-review-tool"></a>İnsan inceleme aracı

Content Moderator hizmeti, web tabanlı [insan inceleme aracını](Review-Tool-User-Guide/human-in-the-loop.md) da içerir. 

![Content Moderator insan inceleme aracı giriş sayfası](images/homepage.PNG)

İnceleme APı'lerini kullanarak belirlediğiniz filtrelere göre metin, görüntü ve video içeriğinde inceleme gerçekleştirecek bir ekip oluşturabilirsiniz. Ardından insan denetiminden geçirerek son kararları belirleyebilirsiniz. İnsan girişi hizmeti eğitmez ancak hizmetin ve insanlardan oluşan ekibin birlikte kullanılması, geliştiricilerin verimlilik ve doğruluk arasındaki doğru dengeyi yakalamasını sağlar.

## <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği
Olarak tüm Bilişsel hizmetler, Content Moderator hizmeti kullanan geliştiricilerin Microsoft'un müşteri verilerini ilkelerinin bilmeniz gerekir. Bkz: [Bilişsel Hizmetler sayfasına](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) daha fazla bilgi için Microsoft Trust Center.

## <a name="next-steps"></a>Sonraki adımlar

' Ndaki yönergeleri takip ederek Content Moderator hizmetini kullanmaya başlama [deneyin Content Moderator Web'de](quick-start.md).
