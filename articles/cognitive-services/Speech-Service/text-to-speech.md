---
title: Metin okuma hakkında - konuşma Hizmetleri
titleSuffix: Azure Cognitive Services
description: 45'den fazla dil ve yerel ayar birden fazla 75 seslerle metin okuma API'si sunar. Standart ses tipi olarak kullanmak için yalnızca konuşma hizmeti çağırdığınızda, diğer birkaç parametrelerle ses adı belirtmeniz gerekir.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 42e9a7e02bbe7efeab4ea0d8ee5d9876b68a7565
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53100659"
---
# <a name="about-the-text-to-speech-api"></a>Metin okuma hakkında API'si

**Metin okuma** konuşma hizmeti API'sini (TTS) giriş metni doğal görünen konuşmaya dönüştürür (olarak da adlandırılan *konuşma sentezi*).

Konuşmanın oluşturulacağı uygulamanızı konuşma hizmeti için HTTP POST isteği gönderir. Metni konuşmaya İnsan görünen, oluşturulan ve bir ses dosyası olarak döndürülür. Seslerle ve dilleri çeşitli desteklenir.

İçinde hangi konuşma sentezi benimseniyor senaryolar şunlardır:

* *Erişilebilirliği iyileştirme:* **metin okuma** teknolojisi, içerik sahiplerinin sağlar ve farklı şekillerde kişilere yanıt yayımcılar kendi içerikleriyle etkileşim. Görsel bozukluğu veya okuma zorluklarla kişilerle işitsel içeriği kullanamayabilir olması için teşekkür ederiz. Ayrıca çıktı ses gazete veya arasında işe gidip gelmek veya uygulama çalışırken, mobil cihazlarda blogları gibi metinsel içeriği yararlanabilmek kişi için kolaylaştırır.

* *Çoklu senaryolarda yanıt:* **metin okuma** önemli bilgileri hızla ve rahat bir şekilde gidiş veya aksi halde kullanışlı dışında okuma ortam devralarak yapmasını sağlar. Gezinti, bu alandaki yaygın bir uygulamadır.

* *Öğrenme birden çok modları ile geliştirme:* farklı kişilere iyi farklı şekillerde öğrenin. Çevrimiçi öğrenme uzmanlar, ses ve metin birlikte sağlama bilgilerini öğrenin ve korumak daha kolay hale getirmek yardımcı olduğunu göstermiştir.

* *Sezgisel robotlar veya Yardımcıları teslim:* konuşma yeteneği bütünleyici bir akıllı sohbet Robotu ya da sanal bir yardımcı olabilir. Giderek daha çok şirketlerin müşterileri için ilgi çekici bir müşteri hizmeti deneyimleri sağlamak sohbet bot geliştirmeye. Ses, başka bir boyut (örneğin, telefon) işitsel alınacak botun yanıtlar vererek ekler.

## <a name="voice-support"></a>Ses desteği

Microsoft **metin okuma** hizmeti, birden fazla 75 seslerle 45'den fazla dil ve yerel ayar sunar. Bu standart "ses tipi" kullanmak için yalnızca hizmetin REST API çağrısı ile diğer bazı parametreler ses adı belirtmeniz gerekir. Desteklenen sesi ayrıntıları için bkz: [desteklenen diller](language-support.md#text-to-speech).

Uygulamanız için benzersiz bir ses isterseniz oluşturabileceğiniz [özel ses tipi](how-to-customize-voice-font.md) gelen kendi konuşma örnekleri.

## <a name="api-capabilities"></a>API özellikleri

Çok sayıda yeteneklerini **metin okuma** API, özellikle özelleştirmesi, REST kullanılabilir. Aşağıdaki tabloda, her API erişimi yöntemi yeteneklerini özetler. Özellikler ve API ayrıntıları tam listesi için bkz: [Swagger başvuru](https://westus.cris.ai/swagger/ui/index).

| Kullanım örneği | REST | SDK’lar |
|-----|-----|-----|----|
| Ses uyarlama için veri kümelerini karşıya yükleme | Evet | Hayır |
| Ses yazı tipi modelleri Yönet & Oluştur | Evet | Hayır |
| Ses yazı tipi dağıtımları yönetmek & Oluştur | Evet | Hayır |
| Ses yazı tipi testleri yönetmek & Oluştur| Evet | Hayır |
| Abonelikleri Yönetme | Evet | Hayır |

> [!NOTE]
> API, API isteklerinin 5 saniye başına 25 sınırlayan azaltma uygular. İleti üst sınırları bilgilendirecektir.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [REST API aracılığıyla konuşma sentezlemek öğrenin](how-to-text-to-speech.md)
