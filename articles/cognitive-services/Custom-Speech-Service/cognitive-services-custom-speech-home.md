---
title: Azure'da özel konuşma hizmeti genel bakış | Microsoft Docs
description: Özel konuşma hizmeti kullanıcıların konuşma modelleri için metne dönüştürme konuşma transkripsiyonu özelleştirme olanak sağlayan bir bulut tabanlı bir hizmettir.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/07/2017
ms.author: panosper
ROBOTS: NOINDEX
ms.openlocfilehash: da88989753069f7ba8ca2c2e2806a648f3df4e3c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46948375"
---
# <a name="what-is-custom-speech-service"></a>Özel Konuşma Tanıma Hizmeti nedir?

Özel konuşma hizmeti, kullanıcıların konuşma modelleri için metne dönüştürme konuşma transkripsiyonu özelleştirme olanağı sağlayan bir bulut tabanlı bir hizmettir.
Özel konuşma hizmeti kullanmak için başvurmak [özel konuşma hizmeti portalı](https://cris.ai).

Özel konuşma hizmeti, özel dil modelleri ve uygulamanız ve kullanıcılarınız için özel akustik modeller oluşturmanızı sağlar. Özel konuşma hizmeti için özel konuşma tanıma ve/veya metin veri yükleyerek, Microsoft'un mevcut durumu resim konuşma modelleri ile birlikte kullanılabilecek özel modeller oluşturabilirsiniz.

Örneğin, bir cep telefonu, tablet veya PC uygulamaya sesli etkileşim ekliyorsanız, uygulamanız için özellikle tasarlanmış bir metne dönüştürme konuşma uç noktası oluşturmak için akustik model Microsoft'un birlikte bir özel dil modeli oluşturabilirsiniz. Uygulamanız için belirli bir kullanıcı bir popülasyon kullanılarak veya belirli bir ortamda tasarlanmışsa, oluşturabilir ve bu hizmeti ile özel akustik model dağıtma.


## <a name="how-do-speech-recognition-systems-work"></a>Konuşma tanıma sistemleri nasıl çalışır?
Konuşma tanıma sistemleri birlikte çalışan birkaç bileşenden oluşur. En önemli bileşenleri akustik model ve dil modeli ikisidir.

Akustik model bir dizi Fonem ya da ses birimi, belirli bir dilde birine kısa ses parçalarını etiketleyen bir sınıflandırıcıdır. Örneğin, "Okuma" sözcük "s p IY ch" dört Fonem oluşur. Bu sınıflandırmalar, saniyede yaklaşık 100 kez gerçekleştirilir.

Dil modeli, sözcük dizileri üzerine bir olasılık dağılımıdır. Dil modeli sistemin, söyleniş biçimi birbirine benzeyen söz dizileri arasından seçim yapmasına yardımcı olur. Sistem bu seçimi, söz dizisinin kullanılma olasılığına göre yapar. Örneğin, “konuşma tanıma” ile “konuş vatanıma” ifadelerinin söylenişi birbirine benzer, ancak birinci hipotezin gerçekleşme olasılığı çok daha yüksek olduğundan dil modeli, birinci modele daha yüksek bir puan atar.

Akustik ve dil modelleri, eğitim verileri öğrenilen istatistiksel modelleridir. Uygulamalarında kullanıldığında karşılaştıkları konuşma benzer veriler eğitim sırasında gözlemlenen olduğunda sonuç olarak, bunlar en iyi şekilde çalışır. Microsoft konuşma metin altyapısındaki akustik ve dil modellerini metin ve konuşma devasa bir koleksiyon üzerinde geliştirilen ve durumu resim performans, akıllı üzerinde Cortana ile etkileşim kurma gibi yaygın kullanım senaryoları için telefon, tablet veya PC, Web'de sesli arama veya kısa mesaj bir arkadaşınıza dikte.

## <a name="why-use-the-custom-speech-service"></a>Özel konuşma hizmeti neden kullanmalısınız?
Microsoft Konuşmayı metne dönüştürme altyapısı birinci sınıf olsa da, yukarıda açıklanan senaryolardan doğru yöneliktir. Ancak, sesli sorguların belirli sözlük öğelerini ürün adları veya tipik konuşma dilinde nadiren gerçekleşen terminolojisinin içerecek şekilde uygulamanıza bekliyorsanız, dil modelini özelleştirerek geliştirilmiş performans elde edebilirsiniz.

Örneğin, MSDN’de sesli arama gerçekleştirmeye yönelik bir uygulama oluşturuyor olsaydınız “nesne odaklı”, “ad alanı” veya “dot net” gibi terimler, normal ses tanıma uygulamalarına kıyasla daha sık görülürdü. Dil modelinin özelleştirilmesi, sistemin bunu öğrenmesine olanak tanır.

## <a name="next-steps"></a>Sonraki adımlar

Özel konuşma hizmeti kullanma hakkında daha fazla bilgi için bkz. [özel konuşma hizmeti portalı] (https://cris.ai).

* [Kullanmaya Başlama](cognitive-services-custom-speech-get-started.md)
* [SSS](cognitive-services-custom-speech-faq.md)
* [Sözlük](cognitive-services-custom-speech-glossary.md)
