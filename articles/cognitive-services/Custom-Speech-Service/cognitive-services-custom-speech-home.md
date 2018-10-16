---
title: Azure'da özel konuşma hizmeti genel bakış | Microsoft Docs
description: Özel Konuşma Tanıma Hizmeti, kullanıcıların konuşmayı metne dönüştürme transkripsiyonu için konuşma modellerini özelleştirmesine olanak sağlayan bulut tabanlı bir hizmettir.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/07/2017
ms.author: panosper
ms.openlocfilehash: 97eee2b6440dbbf740ad5fa856bd518facabbfef
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49342305"
---
# <a name="what-is-custom-speech-service"></a>Özel Konuşma Tanıma Hizmeti nedir?

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

Özel Konuşma Tanıma Hizmeti, kullanıcıların Konuşmayı Metne dönüştürme transkripsiyonu için konuşma modellerini özelleştirmesine olanak sağlayan bulut tabanlı bir hizmettir.
Özel Konuşma Tanıma Hizmetini kullanmak için [Özel Konuşma Tanıma Hizmeti Portalı](https://cris.ai)’na bakın.

Özel Konuşma Tanıma Hizmeti, uygulamanıza ve kullanıcınıza göre uyarlanmış, özelleştirilmiş dil modelleri ve akustik modeller oluşturmanıza olanak sağlar. Özel konuşma ve/veya metin verilerinizi Özel Konuşma Tanıma Hizmetine yükleyerek, Microsoft’un mevcut yeni konuşma modelleriyle birlikte kullanılabilecek özel modeller oluşturabilirsiniz.

Örneğin, bir cep telefonu, tablet veya PC uygulamasına sesli etkileşim ekliyorsanız, uygulamanız için özel olarak tasarlanmış, konuşmayı metne dönüştürme uç noktası oluşturmak için Microsoft’un akustik modeliyle birleştirilebilen özel bir dil modeli oluşturabilirsiniz. Uygulamanız, belirli bir ortamda veya belirli bir kullanıcı popülasyonu tarafından kullanılmak üzere tasarlanmışsa, bu hizmetle özel bir akustik model oluşturabilir ve dağıtabilirsiniz.


## <a name="how-do-speech-recognition-systems-work"></a>Konuşma tanıma sistemleri nasıl çalışır?
Konuşma tanıma sistemleri, birlikte çalışan birçok bileşenden oluşur. En önemli bileşenlerin ikisi, akustik model ve dil modelidir.

Akustik model, kısa ses parçalarını belirli bir dildeki çeşitli fonemlerden veya ses birimlerinden birine göre etiketleyen bir sınıflandırıcıdır. Örneğin, İngilizcedeki “speech” (konuşma) sözcüğü, “s p iy ch” şeklinde dört fonemden oluşur. Bu sınıflandırmalar, saniyede yaklaşık 100 kez gerçekleştirilir.

Dil modeli, sözcük dizileri üzerine bir olasılık dağılımıdır. Dil modeli sistemin, söyleniş biçimi birbirine benzeyen söz dizileri arasından seçim yapmasına yardımcı olur. Sistem bu seçimi, söz dizisinin kullanılma olasılığına göre yapar. Örneğin, “konuşma tanıma” ile “konuş vatanıma” ifadelerinin söylenişi birbirine benzer, ancak birinci hipotezin gerçekleşme olasılığı çok daha yüksek olduğundan dil modeli, birinci modele daha yüksek bir puan atar.

Akustik ve dil modelleri, eğitim verilerinden öğrenilen istatistiksel modellerdir. Sonuç olarak, uygulamalarda kullanıldığında karşılaştıkları konuşma, eğitim sırasında gözlemlenen verilere benzer olduğunda en yüksek performansı gösterir. Microsoft Konuşmayı Metne Dönüştürme altyapısındaki akustik ve dil modelleri, devasa bir konuşma ve metin koleksiyonu üzerinde eğitilmiş olup web’de sesle arama yaparak veya metin iletilerini bir arkadaşa dikte ederek akıllı telefonunuzda, tabletinizde veya PC’nizde Cortana ile etkileşim kurma gibi en yaygın kullanım senaryoları için en iyi performansı sunar.

## <a name="why-use-the-custom-speech-service"></a>Özel Konuşma Tanıma Hizmeti neden kullanılmalıdır?
Microsoft Konuşmayı Metne Dönüştürme altyapısı birinci sınıf olsa da, yukarıda açıklanan senaryolara yöneliktir. Ancak uygulamanıza yönelik sesli sorguların belirli sözlük öğelerini (ürün adları veya gündelik konuşmada nadiren kullanılan terimler) içermesini bekliyorsanız, dil modelini özelleştirerek daha iyi bir performans elde edebilirsiniz.

Örneğin, MSDN’de sesli arama gerçekleştirmeye yönelik bir uygulama oluşturuyor olsaydınız “nesne odaklı”, “ad alanı” veya “dot net” gibi terimler, normal ses tanıma uygulamalarına kıyasla daha sık görülürdü. Dil modelinin özelleştirilmesi, sistemin bunu öğrenmesine olanak tanır.

## <a name="next-steps"></a>Sonraki adımlar

Özel konuşma hizmeti kullanma hakkında daha fazla bilgi için bkz. [özel konuşma hizmeti portalı](https://cris.ai).

* [Kullanmaya Başlama](cognitive-services-custom-speech-get-started.md)
* [SSS](cognitive-services-custom-speech-faq.md)
* [Sözlük](cognitive-services-custom-speech-glossary.md)
