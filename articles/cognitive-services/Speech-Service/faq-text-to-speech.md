---
title: Azure metin okuma hizmeti hakkında sık sorulan sorular
titleSuffix: Azure Cognitive Services
description: Metin okuma hizmetle ilgili en yaygın soruların yanıtlarını alın.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/11/2018
ms.author: panosper
ms.openlocfilehash: fd8362748c39389139e8384d0bad7e84d20128a4
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66751615"
---
# <a name="text-to-speech-frequently-asked-questions"></a>Metin okuma hakkında sık sorulan sorular

Bu SSS'de sorularınızın yanıtlarını bulamazsanız, kullanıma [diğer destek seçenekleri](support.md).

## <a name="general"></a>Genel

**S: Bir standart ses modeli ve özel ses model arasındaki fark nedir?**

**A**: Ses standart modele (olarak da adlandırılan bir *ses tipi*) Microsoft'a ait verileri kullanılarak geliştirilen ve bulut üzerinde zaten dağıtılmış. Ortalama bir model uyarlama ve timbre ve konuşmacının ses stili ifade aktarmak veya kullanıcı tarafından hazırlanan eğitim verileri temel alan tam, yeni bir modeli eğitmek için özel sesli modeli kullanabilirsiniz. Günümüzde giderek daha fazla müşteriler kendi botlar için tür bir tane, markalı bir ses ister. Özel ses oluşturma platformu için bu seçeneği doğru seçimdir.

**S: Standart ses modelini kullanmak istiyorsanız nereden başlamalıyım?**

**A**: HTTP isteklerini 80'den fazla standart ses modelleri üzerinde 45 dilde kullanılabilir. İlk olarak, alın bir [abonelik anahtarı](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started). Predeployed ses modelleri için REST çağrıları yapmak için bkz: [REST API](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis).

**S: API, özelleştirilmiş sesli model kullanmak isterseniz standart ses için kullanılan bir ile aynı mıdır?**

**A**: Özel ses modeli oluşturulup dağıtıldığında, modelinizi için benzersiz bir uç noktası alın. Uygulamalarınızda konuşma ses kullanmak için uç noktası, HTTP isteklerini belirtmeniz gerekir. Metin okuma hizmeti REST API'si kullanılabilir olan aynı işlevleri özel uç noktanız için kullanılabilir. Bilgi edinmek için nasıl [oluşturabilir ve özel uç noktanız](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-customize-voice-font#create-and-use-a-custom-voice-endpoint).

**S: Verilerimi kendi özel sesli modelleri oluşturmak için eğitim verileri hazırlamanız gerekiyor mu?**

**A**: Evet, eğitim verileri kendiniz için bir özel sesli modeli hazırlamanız gerekir.

Konuşma verilerinin bir koleksiyonunu özelleştirilmiş sesli modeli oluşturmak için gereklidir. Bu koleksiyon, bir dizi konuşma kayıtlarını ses dosyalarının ve her ses dosyasının transkripsiyonu, metin dosyası oluşur. Dijital sesinizi sonucunu yoğun eğitim verilerinizi kaliteye kullanır. İyi bir metin okuma ses üretmek için bir yüksek kaliteli ayakta Mikrofon Sessiz bir odada kayıtları yapılan önemlidir. Tutarlı bir birim oranı konuşma ve aralık ve konuşma ifadesel veren davranışların bile tutarlılık gibi harika bir dijital ses oluşturmak için gereklidir. Ses kaydı Studio'da kaydı önemle öneririz.

Şu anda, biz çevrimiçi kaydı desteklemek veya kayıt studio önerisi sahip kullanmayın. Biçim gereksinim için bkz: [kayıtları ve dökümler hazırlama](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-custom-voice-create-voice).

**S: Özel bir üslup eğitimi konuşma verilerini kaydetmek için hangi betikleri kullanmalıyım?**

**A**: Ses kaydı için komut dosyalarını sınırlandırırız yok. Konuşma kaydetmek için kendi betiklerini kullanabilirsiniz. Yalnızca konuşma verilerinizi yeterli fonetik kapsamı olduğundan emin olun. Özel ses eğitmek için küçük bir 50 farklı cümlelerden (yaklaşık konuşma 3-5 dakika) olabilecek konuşma veri hacmi ile başlayabilirsiniz. Daha fazla veri sağlarsanız, daha doğal sesinizi olacaktır. Kayıt (yaklaşık konuşma 3-4 saat) 2. 000'den fazla tümcelerin sağladığınızda tam ses tipi eğitmek başlatabilirsiniz. Yüksek kaliteli, tam ses almak için 6000'den fazla cümlelerden (yaklaşık konuşma 8-10 saat) kayıtlarını hazırlamanız gerekir.

Kayıt için betikler hazırlanmanıza yardımcı olması için ek hizmetler sağlarız. İlgili kişi [özel sesli müşteri desteği](mailto:customvoice@microsoft.com?subject=Inquiries%20about%20scripts%20generation%20for%20Custom%20Voice%20creation) sorgular için.

**S: Varsayılan değer veya Portalı'nda sunulan daha yüksek eşzamanlılık ne gerekiyor?**

**A**: Modelinizi 20 eş zamanlı istek artışlarla ölçeklendirme yapabilir. İlgili kişi [özel sesli müşteri desteği](mailto:customvoice@microsoft.com?subject=Inquiries%20about%20scripts%20generation%20for%20Custom%20Voice%20creation) için yüksek ölçeklendirme hakkında sorgular.

**S: Ben my modeli indirebilir ve yerel olarak çalıştırma?**

**A**: Modelleri indirilir ve yerel olarak yürütülür.

**S: İsteklerim kısıtlanan?**

**A**: REST API istekleri 5 saniye başına 25 sınırlar. Ayrıntılar için sayfalarımızın bulunabilir [metin okuma](text-to-speech.md). 

## <a name="next-steps"></a>Sonraki adımlar

* [Sorun giderme](troubleshooting.md)
* [Sürüm notları](releasenotes.md)
