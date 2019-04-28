---
title: Azure Özel Görüntü İşleme nedir?
titlesuffix: Azure Cognitive Services
description: Özel Görüntü İşleme Hizmeti'ni kullanarak Azure bulutunda özel görüntü sınıflandırıcıları oluşturmayı öğrenin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: overview
ms.date: 03/21/2019
ms.author: pafarley
ms.openlocfilehash: 50935aca20af931eec63717921ef7a73267d2373
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60996651"
---
# <a name="what-is-azure-custom-vision"></a>Azure Özel Görüntü İşleme nedir?

Azure özel görüntü işleme oluşturmanızı, dağıtmanızı ve kendi resim sınıflandırıcıları geliştirmeye olanak sağlayan bir bilişsel bir hizmettir. Etiketleri geçerli bir yapay ZEKA hizmeti bir görüntü sınıflandırıcıdır (hangi temsil _sınıfları_) Görüntülere visual özelliklerine göre. Farklı [görüntü işleme](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home) hizmeti, özel görüntü işleme uygulamak için etiket belirlemenize olanak sağlar.

## <a name="what-it-does"></a>Ne yapar?

Özel görüntü işleme hizmeti, görüntüleri etiketleri uygulamak için bir makine öğrenimi algoritması kullanır. Geliştirici olarak size, özellik ve söz konusu özellikleri eksik görüntü gruplarını göndermeniz gerekir. Görüntüleri kendiniz gönderme zamanında etiketleyin. Ardından algoritması için bu verileri eğitir ve kendisini aynı görüntülerin test ederek kendi doğruluğu hesaplar. Algoritma eğitildi sonra test, yeniden eğitme ve sonunda yeni görüntüleri, uygulamanızın ihtiyaçlarına göre sınıflandırmak için kullanın. İsterseniz modeli çevrimdışı kullanmak üzere dışarı aktarabilirsiniz.

### <a name="classification-and-object-detection"></a>Sınıflandırma ve nesne algılama

Özel Görüntü İşleme hizmeti iki bölüme ayrılabilir. **Görüntü sınıflandırma** yansımaya bir veya daha fazla etiket uygular. **Nesne algılama** benzer, ancak aynı zamanda koordinatları görüntüde uygulanmış etiketi bulunduğu döndürür.

### <a name="optimization"></a>İyileştirme

Özel görüntü işleme hizmeti, görüntüleri arasındaki başlıca farklar hızlı bir şekilde tanımak için optimize edilmiştir. Bu, prototip oluşturma başlamak için az miktarda veriniz modelinizi sağlar. 50 etikete göre genellikle iyi bir başlangıç görüntüleridir. Ancak bu durumda hizmet, görüntülerdeki küçük farklılıkları algılamak (kalite denetimi senaryolarında küçük çatlakların veya eziklerin algılanması gibi) için en uygun durumda olmayacaktır.

Ayrıca, özel görüntü işleme algoritması belirli malzemeyi görüntülerle için optimize edilmiş çeşitli çeşitleri aralarından seçim yapabileceğiniz&mdash;yer işareti veya perakende öğeleri. Bu konuda daha fazla bilgi için [Sınıflandırıcı oluşturma](getting-started-build-a-classifier.md) belgesine bakın.

## <a name="what-it-includes"></a>Neleri içerir

Özel Görüntü İşleme Hizmeti, yerel SDK'lara ek olarak [Özel Görüntü İşleme giriş sayfası](https://customvision.ai/) üzerinden web tabanlı arabirim aracılığıyla da sunulmaktadır. Oluşturun, test edin ve her iki arabirim üzerinden bir model eğitip veya her ikisini de birlikte kullanın.

![Chrome tarayıcı penceresinde Özel Görüntü İşleme giriş sayfası](media/browser-home.png)

## <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği

Olarak tüm Bilişsel hizmetler Custom Vision Service'i kullanan geliştiricilerin Microsoft'un müşteri verilerini ilkelerinin bilmeniz gerekir. Bkz: [Bilişsel Hizmetler sayfasına](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) daha fazla bilgi için Microsoft Trust Center.

## <a name="next-steps"></a>Sonraki adımlar

İzleyin [sınıflandırıcı oluşturma](getting-started-build-a-classifier.md) özel görüntü işleme web üzerinde kullanmaya başlamak için yol ya da tamamlamak bir [görüntü sınıflandırma Öğreticisi](csharp-tutorial.md) temel senaryo koda uygulanması.
