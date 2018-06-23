---
title: Metin okuma hakkında | Microsoft Docs
description: Metin okuma özelliklerine genel bakış.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 84baf03c83bb63883b80982056cdf6e1e25b3fb7
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355631"
---
# <a name="about-the-text-to-speech-api"></a>Metin okuma hakkında API

**Metin okuma** konuşma hizmetinin (TTS) API doğal sesli konuşma giriş metni dönüştürür (olarak da bilinir *konuşma Birleştirici*).

Konuşma oluşturmak için uygulamanızın HTTP POST isteklerini konuşma hizmetine gönderir. Metin İnsan sesli konuşma, oluşturulan ve bir ses dosyası olarak döndürülür. Çeşitli sesler ve dilleri desteklenir.

Hangi konuşma Birleştirici benimsenen senaryolar şunlardır:

* *Erişilebilirlik artırma:* **metin okuma** teknolojisi, içerik sahipleri sağlar ve farklı şekillerde kişilere yanıt yayımcılar içeriklerini ile etkileşim. Görsel bozukluğu veya okuma zorluklar kişilerle içeriği işitsel olarak kullanmak geliştirebilmek için teşekkür ederiz. Ayrıca çıkış sesli gazetelerdeki veya arasında işe gidip gelmek veya kullanan mobil cihazlarda bloglar gibi metinsel içerikten kişiler için kolaylaştırır.

* *Çoklu senaryolarda yanıt:* **metin okuma** önemli bilgileri hızla ve rahat yürüten veya aksi halde kullanışlı dışında okuma ortamı almak amacıyla kişilerin sağlar. Gezinti, bu alandaki yaygın bir uygulamadır. 

* *Birden çok modlarıyla öğrenme geliştirme:* farklı kişiler farklı şekilde en iyi öğrenin. Çevrimiçi eğitim uzmanlar, ses ve metin birlikte sağlama bilgileri öğrenin ve korumak daha kolay hale getirmek yardımcı göstermiştir.

* *Sezgisel aracılarını ya da Yardımcıları teslim:* konuşma yeteneği akıllı sohbet bot veya sanal Yardımcısı'nın ayrılmaz bir parçası olabilir. Daha da fazla şirketler çekici müşteri, müşterileri için hizmet deneyimleri sağlamayı sohbet aracılarını geliştirmektedir. Ses (örneğin, telefon) işitsel olarak alınacak bot's yanıt vererek başka bir boyut ekler.

## <a name="voice-support"></a>Ses desteği

Microsoft **okuma** hizmeti birden fazla 75 sesleri birden fazla 45 dilleri ve yerel ayarlar sunar. Bu standart "sesli yazı tipleri" kullanmak için yalnızca hizmet REST API çağrısı zaman diğer birkaç parametrelerle sesli adı belirtmeniz gerekir. Desteklenen sesi Ayrıntılar için bkz [desteklenen diller](supported-languages.md). 

Uygulamanız için benzersiz bir sesli istiyorsanız oluşturabileceğiniz [özel sesli yazı tipleri](how-to-customize-voice-font.md) kendi konuşma örnekleri gelen.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi Al](https://azure.microsoft.com/try/cognitive-services/)
* [C# Konuşma tanıması bkz.](quickstart-csharp-windows.md)
