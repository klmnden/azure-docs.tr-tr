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
ms.date: 07/03/2019
ms.author: pafarley
ms.openlocfilehash: d814a943bc8dc789abe84b33583714beb998c0ef
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67607011"
---
# <a name="what-is-azure-content-moderator"></a>Azure Content Moderator nedir?

Azure Content Moderator API'si rahatsız edici, riskli veya istenmeyen olabilecek öğeleri tespit etme amacıyla metin, görüntü ve video içeriğinde denetim gerçekleştiren bir bilişsel hizmettir. Hizmet, böyle bir öğeyle karşılaştığında içeriğe uygun etiketler (bayraklar) ekler. Uygulamanız da bu bayraklı içeriği dikkate alarak düzenlemelere uygunluk ya da kullanıcılar için istenen ortamın oluşturulması amacıyla işlem gerçekleştirebilir. Bkz: [yönetim API'leri](#moderation-apis) ne farklı içerik bayraklar hakkında daha fazla bilgi edinmek için bölüm belirtin.

## <a name="where-it-is-used"></a>Kullanıldığı yerler

Aşağıda bir yazılım geliştirme uzmanının veya ekibinin Content Moderator özelliklerinden faydalanmak isteyebileceği birkaç senaryo verilmiştir:

- Ürün katalogları ve diğer kullanıcı tarafından oluşturulan içerikleri Orta çevrimiçi marketlerdir.
- Kullanıcı tarafından oluşturulan oyun yapıtlar ve sohbet odaları Orta oyun şirketler.
- Görüntü, metin ve bunların kullanıcılar tarafından eklenen videoları Orta sosyal Mesajlaşma platformlar.
- Kendi içerik için merkezi yönetimini uygulamak Kurumsal medya şirketlerinin.
- Öğrenciler ve eğitimciler için uygun olmayan içerikleri filtreleme K-12 eğitim çözüm sağlayıcıları.

> [!NOTE]
> Geçersiz alt istismarıyla kategorisinde olabilecek resimleri algılama için Content Moderator'ı kullanamazsınız. Ancak, tam kuruluşlar kullanabilir [PhotoDNA bulut hizmeti](https://www.microsoft.com/photodna "Microsoft PhotoDNA bulut hizmeti") ekranına bu içerik türü için.

## <a name="what-it-includes"></a>Neleri içerir

Content Moderator hizmeti hem REST çağrıları hem de .NET SDK'sı ile sunulan çeşitli web hizmeti API'lerinden oluşur. Ayrıca hizmete yardımcı olmak veya denetleme işlevinde ince ayar yapmak için insanlar tarafından gerçekleştirilen inceleme hizmeti de sunar.

## <a name="moderation-apis"></a>Yönetim API’leri

Content Moderator hizmeti, büyük olasılıkla uygun veya uygunsuz malzeme için içerik denetimi API'leri içerir.

![Content Moderator denetimi API'leri için Blok Diyagramı](images/content-moderator-mod-api.png)

Aşağıdaki tabloda, yönetim API'lerini farklı türleri açıklanmaktadır.

| API grubu | Açıklama |
| ------ | ----------- |
|[**Metin denetimi**](text-moderation-api.md)| Metin rahatsız edici içerik, cinsel açık veya müstehcen içerik, küfür ve kişisel verileri tarar.|
|[**Özel terim listeleri**](try-terms-list-api.md)| Metinleri yerleşik terimlere ek olarak özel terim listesine göre tarar. İçerik ilkelerinize göre içerik engellemek veya izin vermek için özel listeler kullanabilirsiniz.|  
|[**Görüntü denetimi**](image-moderation-api.md)| Görüntülerde yetişkinlere yönelik veya müstehcen görüntü taraması yapar, Optik Karakter Tanıyıcı (OCR) ile görüntüdeki metinleri tanır ve yüzleri algılar.|
|[**Özel görüntü listeleri**](try-image-list-api.md)| Görüntüleri özel görüntü listesine göre tarar. Özel görüntü listelerini kullanarak tekrar tekrar sınıflandırmak istemediğiniz yaygın içerik örneklerini filtreleyebilirsiniz.|
|[**Video denetimi**](video-moderation-api.md)| Videolarda yetişkinlere yönelik veya müstehcen içerik taraması yapar ve bu içeriklerle ilgili zaman işaretçilerini döndürür.|

## <a name="review-apis"></a>API’leri inceleme

İnceleme API'lerini denetimi işlem hattınızı İnsan geçirenlerle tümleştirmenize olanak tanır. Kullanım [işleri](review-api.md#jobs), [incelemeleri](review-api.md#reviews), ve [iş akışı](review-api.md#workflows) oluşturmak ve ile İnsan içinde--döngüsü iş akışlarını otomatikleştirmek için operations [gözden geçirme aracı](#the-review-tool) () Aşağıda).

> [!NOTE]
> İş akışı API .NET SDK'yı henüz kullanılamıyor, ancak REST uç noktası ile kullanılabilir.

![Content Moderator için blok diyagramını API'leri gözden geçirin](images/content-moderator-rev-api.png)

## <a name="the-review-tool"></a>Gözden geçirme aracı

Content Moderator hizmet ayrıca web tabanlı içerir [gözden geçirme aracı](Review-Tool-User-Guide/human-in-the-loop.md), içeriği barındıran işlemek İnsan Moderatörler gözden geçirir. İnsan girişi hizmeti eğitmez ancak hizmetin ve insanlardan oluşan ekibin birlikte kullanılması, geliştiricilerin verimlilik ve doğruluk arasındaki doğru dengeyi yakalamasını sağlar. Gözden geçirme Aracı kullanıcı dostu bir ön uç çeşitli Content Moderator kaynaklar için de sağlar.

![Content Moderator insan inceleme aracı giriş sayfası](images/homepage.PNG)

## <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği

Olarak tüm Bilişsel hizmetler, Content Moderator hizmeti kullanan geliştiricilerin Microsoft'un müşteri verilerini ilkelerinin bilmeniz gerekir. Bkz: [Bilişsel Hizmetler sayfasına](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) daha fazla bilgi için Microsoft Trust Center.

## <a name="next-steps"></a>Sonraki adımlar

' Ndaki yönergeleri takip ederek Content Moderator hizmetini kullanmaya başlama [deneyin Content Moderator Web'de](quick-start.md).