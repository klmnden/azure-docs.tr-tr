---
title: Azure üzerinde sık sorulan sorular için konuşma tanıma Hizmeti'ne metin
description: Konuşmayı metne dönüştürme ile ilgili en yaygın soruların yanıtları aşağıdadır.
services: cognitive-services
author: PanosPeriorellis
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 06/11/2018
ms.author: panosper
ms.openlocfilehash: 8d70c4a359c713d6c5f46423193e9c9e7e1f3baf
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39282866"
---
# <a name="text-to-speech-frequently-asked-questions"></a>Metin okuma hakkında sık sorulan sorular

Bu SSS'de sorularınızın yanıtlarını bulamazsanız, diğer destek seçenekleri denetleyin [burada](support.md).

## <a name="general"></a>Genel

**Soru**: standart ve özel ses modelleri arasındaki fark nedir?

**Yanıt**: Standart ses modeller (yani) Ses tipleri) ait verilerin Microsoft ile eğitilmiş ve bulut üzerinde zaten dağıtılmış. Özel ses modelleri ortalama bir model uyarlama ve timbre ve ifade şekilde konuşmacının ses stili göre aktarmak veya kullanıcı tarafından hazırlanan eğitim verileri temel alan tam yeni bir modeli eğitmek için kullanıcının verin. Günümüzde giderek daha fazla müşteriler kendi botlar için tür bir tane, markalı bir ses ister. Özel ses oluşturma platformu için doğru seçimdir.

**Soru**: nereden başlamalıyım standart ses modelini kullanmak isterseniz?

**Yanıt**: HTTP isteklerini 80'den fazla standart ses modelleri üzerinde 45 dilde kullanılabilir. İlk almanız gereken bir [abonelik anahtarı](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/get-started). Önceden dağıtılan ses modelleri için REST çağrılarını gerçekleştirme başvurun [burada ayrıntıları](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/rest-apis#text-to-speech).

**Soru**: özelleştirilmiş sesli model kullanmak isterseniz API standart sesi aynıdır?

**Yanıt**: oluşturulan ve dağıtılan özel sesli modeli kullandığınız zaman modeliniz için benzersiz bir uç nokta alırsınız. Uygulamalarınızda konuşma ses kullanmak için HTTP istek uç noktası'ni belirtmeniz gerekir. Metin okuma hizmetine için REST API aracılığıyla aynı işlevselliği, özel uç noktanız için de kullanılabilir. Bkz. nasıl [oluşturabilir ve özel uç noktanız](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/how-to-customize-voice-font#create-and-use-a-custom-endpoint).

**Soru**: özel sesli modelleri kendime oluşturmaya yönelik eğitim verilerini hazırlamak gerekiyor mu?

**Yanıt**: kendiniz eğitim verileri hazırlamanız gerekir. Konuşma verilerinin bir koleksiyonunu özelleştirilmiş sesli modeli oluşturmak için gereklidir. Bu koleksiyon, konuşma kayıtları, ses dosyaları ve her ses dosyasının transkripsiyonu, metin dosyası oluşur. Dijital sesinizi sonucunu yoğun eğitim verilerinizi kaliteye kullanır. İyi TTS sesli üretmek için kayıtları ile yüksek kaliteli ayakta Mikrofon Sessiz bir odada yapılır önemlidir. Tutarlı birim oranı, aralık ve konuşma ifadesel veren davranışların bile tutarlılık gibi konuşma harika bir dijital ses oluşturmak için gerekli. Ses kaydı Studio'da kayıtlı olmalıdır öneririz.
Şu anda biz kaydı çevrimiçi destek sağlamaz veya kayıt studio önerisi vardır. Biçim gereksinim için bkz: [kayıtları ve dökümler hazırlamayı öğrenin](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/how-to-customize-voice-font#prepare-recordings-and-transcripts)
 
**Soru**: hangi komut dosyaları için özel bir üslup eğitimi konuşma verilerini kaydetmek için kullanmalıyım? 

**Yanıt**: ses kaydı için komut dosyalarını sınırlamaz. Konuşma kaydetmek için kendi betiklerini kullanabilirsiniz. Yalnızca ses yeterli kapsamı konuşma verilerinizi olduğundan emin olun. Özel ses eğitmek için 50 farklı cümleler olabilir konuşma verilerin küçük bir birim başlayabilirsiniz (konuşma hakkında 3-5 dakika). Daha fazla veri sağlarsanız, daha doğal sesinizi olacaktır. Kayıt (yaklaşık konuşma 3-4 saat) 2000'den fazla tümcelerin sağladığınızda tam ses tipi eğitmek başlatabilirsiniz. Tam yüksek kaliteli sesli almak için kayıt (yaklaşık konuşma 8-10 saat) 6000'den fazla tümcelerin hazırlamanız gerekir.  
Kayıt için betikler hazırlanmanıza yardımcı olması için ek hizmetler sağlarız. İlgili kişi [özel sesli müşteri desteği](mailto:customvoice@microsoft.com?subject=Inquiries%20about%20scripts%20generation%20for%20Custom%20Voice%20creation) sorgular için.

**Soru**: portalda sunulan veya varsayılan değerinden daha yüksek Eş zamanlılık ihtiyacım olursa ne yapabilirim?

**Yanıt**: 20 eş zamanlı istek artışlarla modelinizde ölçekleme yapabilirsiniz. İlgili kişi [özel sesli müşteri desteği](mailto:customvoice@microsoft.com?subject=Inquiries%20about%20scripts%20generation%20for%20Custom%20Voice%20creation) için yüksek ölçeklendirme üzerinde sorgular.

**Soru**: Ben my modeli indirebilir ve yerel olarak çalıştırma?

**Yanıt**: modelleri indirilir ve yerel olarak yürütülür.

## <a name="next-steps"></a>Sonraki adımlar

* [Sorun giderme](troubleshooting.md)
* [Sürüm notları](releasenotes.md)
