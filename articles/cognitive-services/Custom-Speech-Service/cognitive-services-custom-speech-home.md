---
title: Azure üzerinde özel konuşma hizmetine genel bakış | Microsoft Docs
description: Özel konuşma hizmet konuşma modelleri konuşma metin transcription için özelleştirmek kullanıcıların sağlayan bir bulut tabanlı bir hizmettir.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/07/2017
ms.author: panosper
ms.openlocfilehash: a6139283a555f8f97371c02f9d1f3d53dc6f15d3
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352901"
---
# <a name="what-is-custom-speech-service"></a>Özel konuşma hizmeti nedir?

Özel konuşma hizmet kullanıcıların konuşma modelleri konuşma metin transcription için özelleştirme olanağı sağlayan bir bulut tabanlı bir hizmettir.
Özel konuşma hizmetini kullanmak için başvurmak [özel konuşma hizmet portalı](https://cris.ai).

Özel konuşma hizmeti özelleştirilmiş dil modelleri ve uygulamanızı ve kullanıcılarınız için özel olarak hazırlanmış akustik modelleri oluşturmanıza olanak sağlar. Belirli konuşma ve/veya metin veri özel konuşma hizmet yükleyerek, Microsoft'un mevcut durumu resim konuşma modelleri ile birlikte kullanılabilecek özel modelleri oluşturabilirsiniz.

Örneğin, cep telefonu, tablet veya PC uygulama sesli etkileşim ekliyorsanız, uygulamanız için özellikle tasarlanmış bir konuşma metin uç noktası oluşturmak için Microsoft'un akustik modeli ile birleştirilmiş özel dil modeli oluşturabilirsiniz. Uygulamanızı belirli bir ortamda veya belirli kullanıcı nüfusu tarafından kullanılmak üzere tasarlanmış, oluşturun ve bu hizmeti ile özel bir akustik modeli dağıtın.


## <a name="how-do-speech-recognition-systems-work"></a>Konuşma tanıma sistemleri nasıl çalışır?
Konuşma tanıma sistemleri birlikte çalışan pek çok bileşenden oluşur. En önemli bileşenleri akustik modeli ve dil modeli ikisidir.

Ses kısa parçalarını Fonem ya da belirli bir dilde ses birim sayısını birine etiketler bir sınıflandırıcı akustik modelidir. Örneğin, "Okuma" word dört Fonem "s p IY ch" oluşur. Bu sınıflandırmalar, saniyede yaklaşık 100 kez gerçekleştirilir.

Dil modeli, sözcük dizileri üzerine bir olasılık dağılımıdır. Dil modeli sistemin, söyleniş biçimi birbirine benzeyen söz dizileri arasından seçim yapmasına yardımcı olur. Sistem bu seçimi, söz dizisinin kullanılma olasılığına göre yapar. Örneğin, “konuşma tanıma” ile “konuş vatanıma” ifadelerinin söylenişi birbirine benzer, ancak birinci hipotezin gerçekleşme olasılığı çok daha yüksek olduğundan dil modeli, birinci modele daha yüksek bir puan atar.

Kullanım ve dil modelleri eğitim verileri öğrenilen istatistiksel modelleri ' dir. Uygulamalarda kullanıldığında karşılaştıkları konuşma benzer veriler eğitim sırasında gözlenen olduğunda sonuç olarak, bunlar en iyi şekilde çalışır. Microsoft konuşma metin altyapısındaki kullanım ve dil modelleri muazzam koleksiyonunu konuşma ve metin üzerinde eğitilmiş ve üzerinde Akıllı Cortana ile etkileşim gibi en yaygın kullanım senaryoları için durumu resim performans sağlar telefon, tablet veya PC, web tarafından sesli arama veya metin iletileri bir arkadaşınıza dikte.

## <a name="why-use-the-custom-speech-service"></a>Özel konuşma hizmeti neden kullanılır?
Microsoft konuşma metin altyapısı dünya çapındaki olsa da, yukarıda açıklanan senaryoları doğru yöneliktir. Ancak, ürün adlarını veya nadiren tipik konuşma içinde ortaya terimler gibi belirli sözlük öğelerini içermek üzere uygulamanıza sesli sorguları bekliyorsanız dil modeli özelleştirerek performansı elde edebilirsiniz muhtemelen doğrudur.

Örneğin, MSDN’de sesli arama gerçekleştirmeye yönelik bir uygulama oluşturuyor olsaydınız “nesne odaklı”, “ad alanı” veya “dot net” gibi terimler, normal ses tanıma uygulamalarına kıyasla daha sık görülürdü. Dil modelinin özelleştirilmesi, sistemin bunu öğrenmesine olanak tanır.

## <a name="next-steps"></a>Sonraki adımlar

Özel konuşma hizmetini kullanma hakkında daha fazla bilgi için bkz. [özel konuşma hizmet portalı] (https://cris.ai).

* [Kullanmaya Başlama](cognitive-services-custom-speech-get-started.md)
* [SSS](cognitive-services-custom-speech-faq.md)
* [Sözlük](cognitive-services-custom-speech-glossary.md)
