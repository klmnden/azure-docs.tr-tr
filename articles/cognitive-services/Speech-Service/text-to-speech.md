---
title: Metin okuma hakkında
description: Metin okuma özelliklerine genel bakış.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: erhopf
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: erhopf
ms.openlocfilehash: bc60eed63fb40c42fc4331edb01e15f850bf6ecb
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166092"
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

Çok sayıda yeteneklerini **metin okuma** API'si — özellikle çevresinde özelleştirmesi — REST da kullanılabilir. Aşağıdaki tabloda, her API erişimi yöntemi yeteneklerini özetler. Özellikler ve API ayrıntıları tam listesi için lütfen başvurun [Swagger](https://swagger/service/11ed9226-335e-4d08-a623-4547014ba2cc#/),

| Kullanım örneği | REST | SDK’lar |
|-----|-----|-----|----|
| Ses uyarlama için veri kümelerini karşıya yükleme | Evet | Hayır |
| Ses yazı tipi modelleri Yönet & Oluştur | Evet | Hayır |
| Ses yazı tipi dağıtımları yönetmek & Oluştur | Evet | Hayır |
| Ses yazı tipi testleri yönetmek & Oluştur| Evet | Hayır |
| Abonelikleri Yönetme | Evet | Hayır |

> [!NOTE]
> API, API isteklerinin 5 saniye başına 25 sınırlayan azaltma uygular. İleti hearders sınırlarını bilgilendirecektir.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [REST API aracılığıyla konuşma sentezlemek öğrenin](how-to-text-to-speech.md)
