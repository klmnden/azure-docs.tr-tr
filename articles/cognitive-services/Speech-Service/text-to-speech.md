---
title: Okuma-konuşma hizmeti hakkında
titleSuffix: Azure Cognitive Services
description: 45'den fazla dil ve yerel ayar birden fazla 75 seslerle metin okuma API'si sunar. Standart ses tipi olarak kullanmak için yalnızca konuşma hizmeti çağırdığınızda, diğer birkaç parametrelerle ses adı belirtmeniz gerekir.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 0836ae4a9041db27cfed35dd0f1fc0df6e541aff
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55859343"
---
# <a name="about-the-text-to-speech-api"></a>Metin okuma API'si hakkında

**Metin okuma** API (TTS) giriş metni doğal görünen konuşmaya dönüştürür (olarak da adlandırılan *konuşma sentezi*).

Konuşmanın oluşturulacağı uygulamanızı metin okuma API'si için HTTP POST isteği gönderir. Metni konuşmaya İnsan görünen, oluşturulan ve bir ses dosyası olarak döndürülür. Seslerle ve dilleri çeşitli desteklenir.

İçinde hangi konuşma sentezi benimseniyor senaryolar şunlardır:

* *Erişilebilirliği iyileştirme:* **metin okuma** teknolojisi, içerik sahiplerinin sağlar ve farklı şekillerde kişilere yanıt yayımcılar kendi içerikleriyle etkileşim. Görsel bozukluğu veya okuma zorluklarla kişilerle işitsel içeriği kullanamayabilir olması için teşekkür ederiz. Ayrıca çıktı ses gazete veya arasında işe gidip gelmek veya uygulama çalışırken, mobil cihazlarda blogları gibi metinsel içeriği yararlanabilmek kişi için kolaylaştırır.

* *Çoklu senaryolarda yanıt:* **metin okuma** önemli bilgileri hızla ve rahat bir şekilde gidiş veya aksi halde kullanışlı dışında okuma ortam devralarak yapmasını sağlar. Gezinti, bu alandaki yaygın bir uygulamadır.

* *Öğrenme birden çok modları ile geliştirme:* Farklı kişilere iyi farklı şekillerde öğrenin. Çevrimiçi öğrenme uzmanlar, ses ve metin birlikte sağlama bilgilerini öğrenin ve korumak daha kolay hale getirmek yardımcı olduğunu göstermiştir.

* *Sezgisel robotlar veya Yardımcıları teslim:* Konuşma yeteneği bütünleyici bir akıllı sohbet Robotu ya da sanal bir yardımcı olabilir. Giderek daha çok şirketlerin müşterileri için ilgi çekici bir müşteri hizmeti deneyimleri sağlamak sohbet bot geliştirmeye. Ses, başka bir boyut (örneğin, telefon) işitsel alınacak botun yanıtlar vererek ekler.

## <a name="voice-support"></a>Ses desteği

Microsoft **metin okuma** hizmeti, birden fazla 75 seslerle 45'den fazla dil ve yerel ayar sunar. Bu standart "ses tipi" kullanmak için yalnızca hizmetin REST API çağrısı ile diğer bazı parametreler ses adı belirtmeniz gerekir. Desteklenen diller, yerel ayarlar ve sesler hakkında daha fazla bilgi için bkz. [desteklenen diller](language-support.md#text-to-speech).

> [!IMPORTANT]
> Maliyetleri için standart, özel ve sinir sesleri farklılık gösterir. Daha fazla bilgi için [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="neural-voices"></a>Sinir sesleri

Sinir metin okuma, sohbet robotları ve sanal Yardımcıları ile etkileşim daha doğal yapmak ve ilgi çekici, e-kitapları gibi dijital metinleri audiobooks dönüştürmek ve içi navigasyon sistemleri geliştirmek için kullanılabilir. Yapay ZEKA sistemlerle etkileşim kurduğunuzda İnsan benzeri doğal prosody ve sözcük Temizle articulation sinir TTS dinleme yorulma önemli ölçüde azalttı. Sinir sesleri hakkında daha fazla bilgi için bkz: [desteklenen diller](language-support.md#text-to-speech).

### <a name="custom-voices"></a>Özel ses

Metin okuma ses özelleştirme markanız için tanınan, tür, tek bir ses oluşturmanıza olanak sağlar: bir *ses tipi.* Ses tipi oluşturmak için studio kaydını yapabilir ve ilişkili betikler eğitim verileri olarak karşıya yükleyin. Hizmet, ardından kaydınız için ayarlanmış bir benzersiz ses modeli oluşturur. Kendi ses tipi konuşma sentezlemek için kullanabilirsiniz. Daha fazla bilgi için [özel ses tipi](how-to-customize-voice-font.md).

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

* [Serbest konuşma Hizmetleri aboneliği edinin](https://azure.microsoft.com/try/cognitive-services/)
* [Hızlı Başlangıç: Metin okuma, Python Dönüştür](quickstart-python-text-to-speech.md)
* [Hızlı Başlangıç: Metin okuma, .NET Core Dönüştür](quickstart-dotnet-text-to-speech.md)
* [REST API başvurusu](rest-apis.md)
