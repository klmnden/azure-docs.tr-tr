---
title: Metin okuma hakkında
description: Metin okuma özelliklerine genel bakış.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: d111a9f852b849df15dbd056a7210fac82cee190
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39324264"
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

Microsoft **metin okuma** hizmeti, birden fazla 75 seslerle 45'den fazla dil ve yerel ayar sunar. Bu standart "ses tipi" kullanmak için yalnızca hizmetin REST API çağrısı ile diğer bazı parametreler ses adı belirtmeniz gerekir. Desteklenen sesi ayrıntıları için bkz: [desteklenen diller](https://docs.microsoft.com/azure/cognitive-services/speech-service/supported-languages#text-to-speech). 

Uygulamanız için benzersiz bir ses isterseniz oluşturabileceğiniz [özel ses tipi](how-to-customize-voice-font.md) gelen kendi konuşma örnekleri.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [REST API aracılığıyla konuşma sentezlemek öğrenin](how-to-text-to-speech.md)
