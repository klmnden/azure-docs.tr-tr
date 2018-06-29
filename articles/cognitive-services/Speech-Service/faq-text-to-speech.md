---
title: Azure üzerinde sık sorulan sorular konuşma metin hizmetine | Microsoft Docs
description: Burada, konuşma metin hakkında en popüler sorulan soruların yanıtları bulunur.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 06/11/2018
ms.author: panosper
ms.openlocfilehash: 4a29435c0ace79fc3a5d3a5a42a0e91bdbc8da5e
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37082833"
---
# <a name="text-to-speech-frequently-asked-questions"></a>Metin okuma sık sorulan sorular

Bu SSS sorularınızın yanıtlarını bulamazsanız, üzerinde özel konuşma hizmeti topluluğu isteyen deneyin [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) ve [UserVoice](https://cognitive.uservoice.com/)

## <a name="general"></a>Genel

**Soru**: standart ve özel sesli modelleri arasındaki fark nedir?

**Yanıt**: standart sesi modeller (paketini sesli yazı tipleri) veri ait Microsoft ile eğitilmiş ve bulutta zaten dağıtılmış. Özel sesli modelleri ortalama bir model uyum ve timbre ve Konuşmacı sesli stili göre ifade şekilde aktarmak için veya kullanıcı tarafından hazırlanan eğitim verileri temel alan bir tam yeni modeli eğitmek için verin. Bugün daha da fazla müşteriler kendi aracılarını için tür, bir, markalı bir ses istiyorsanız. Platform derleme özel sesli söz konusu doğru seçimdir.

**Soru**: nereden başlamalıyım standart sesi modelini kullanmak isterseniz?

**Yanıt**: 80'den fazla standart sesi modelleri üzerinde 45 dillerde HTTP istekleri üzerinden kullanılabilir. İlk almanız gereken bir [abonelik anahtarı](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/get-started). Önceden dağıtılan sesli modelleri REST çağrı yapmak için başvurun [burada ayrıntıları](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/rest-apis#text-to-speech).

**Soru**: özelleştirilmiş sesli modelini kullanmak istiyorsanız API standart sesi aynıdır?

**Yanıt**: oluşturulan ve dağıtılan özel sesli modeli sahip olduğunuzda, benzersiz bir uç noktası için modelinizin alırsınız. Uygulamalarınızda konuşmaya ses kullanmak için HTTP isteklerinde uç noktası belirtmeniz gerekir. Metin okuma hizmet için REST API aracılığıyla kullanılabilir aynı işlevselliği de özel uç noktası için kullanılabilir. Bkz: nasıl yapılır [oluşturma ve özel uç noktanızı kullanma](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/how-to-customize-voice-font#create-and-use-a-custom-endpoint).

**Soru**: özel sesli modelleri kendime oluşturmak için eğitim verileri hazırlamak gerekiyor mu?

**Yanıt**: eğitim verileri kendiniz için hazırlamanız gerekir. Konuşma veri koleksiyonu, bir özelleştirilmiş sesli model oluşturmak için gereklidir. Bu koleksiyon, konuşma kayıtları ses dosyalarının kümesi ve her ses dosyası transcription bir metin dosyasından oluşur. Dijital sesinizi sonucunu eğitim verilerinizi kalitesinden yoğun olarak kullanır. İyi bir TTS ses üretmek için kayıtları ile yüksek kaliteli durumu Mikrofon Sessiz bir odada yapılır önemlidir. Tutarlı birim, aralık ve açıklayıcı veren davranışların konuşma bile tutarlılık oranı, konuşma harika bir dijital ses oluşturmak için gerekli. Bir kayıt Studio'da kaydedilen sesi sahip olması gerektiğini öneririz.
Şu anda biz çevrimiçi kayıt destek sağlamaz veya herhangi bir kayıt studio önerimiz sahip. Biçim gereksinimini bkz [kayıtları ve dökümleri hazırlama](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/how-to-customize-voice-font#prepare-recordings-and-transcripts)
 
**Soru**: hangi betikleri özel sesli eğitim konuşma verilerini kaydetmek için kullanmalıyım? 

**Yanıt**: ses kaydetmeye için komut dosyalarını sınırlamaz. Konuşma kaydetmek için kendi komut dosyalarını kullanabilirsiniz. Yalnızca yeterli ses kapsamı konuşma verilerinizi olduğundan emin olun. Özel sesli eğitmek için 50 farklı cümleleri olabilir konuşma verilerin küçük bir birimle başlatabilirsiniz (konuşma hakkında 3-5 dakika). Daha fazla veri sağlarsanız, sesiniz fazla doğal olacaktır. 2000'den fazla cümleleri (yaklaşık konuşma 3-4 saat) kayıtlarını sağladığınızda tam sesli yazı tipi eğitmek başlatabilirsiniz. Yüksek kaliteli tam sesli almak için birden çok 6000 cümleleri (yaklaşık konuşma 8-10 saat) kayıtlarını hazırlamak gerekir.  
Komut dosyaları kayıt için hazırlanmanıza yardımcı olması için ek hizmetler sunuyoruz. Kişi [özel sesli müşteri desteği](mailto:customvoice@microsoft.com?subject=Inquiries%20about%20scripts%20generation%20for%20Custom%20Voice%20creation) sorgular için.

**Soru**: varsayılan değerinden daha yüksek eşzamanlılık ne ihtiyacım veya ne portalında sunulur?

**Yanıt**: 20 eş zamanlı istek artışlarla modelinizi yukarı ölçeklendirebilirsiniz. Kişi [özel sesli müşteri desteği](mailto:customvoice@microsoft.com?subject=Inquiries%20about%20scripts%20generation%20for%20Custom%20Voice%20creation) daha yüksek genişletilmesi sorgular için.

**Soru**: t my modeli indirebilir ve yerel olarak çalıştırma?

**Yanıt**: modelleri indirilir ve yerel olarak yürütülür.

## <a name="next-steps"></a>Sonraki adımlar

* [Sorun giderme](troubleshooting.md)
* [Sürüm notları](releasenotes.md)
