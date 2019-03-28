---
title: Azure konuşma Hizmetleri ile metin okuma
titleSuffix: Azure Cognitive Services
description: Azure konuşma Services'dan metin okuma, uygulamalar, Araçlar veya cihazları doğal insan benzeri Sentezlenen konuşmaya metni dönüştürmek sağlayan bir REST tabanlı bir hizmettir. Standart ve sinir ses arasından seçin veya kendi özel sesli benzersiz ürün veya marka oluşturun. 75'ten fazla standart sesleri 45'den fazla dil ve yerel ayar kullanılabilir ve 5 sinir sesleri 4 dil ve yerel ayarlar kullanılabilir.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/19/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 1edc2587ef8680f60082bf6271b73d30184f331b
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58521253"
---
# <a name="what-is-text-to-speech"></a>Metin okuma nedir?

Azure konuşma Services'dan metin okuma, uygulamalar, Araçlar veya cihazları doğal insan benzeri Sentezlenen konuşmaya metni dönüştürmek sağlayan bir REST tabanlı bir hizmettir. Standart ve sinir ses arasından seçin veya kendi özel sesli benzersiz ürün veya marka oluşturun. 75'ten fazla standart sesleri 45'den fazla dil ve yerel ayar kullanılabilir ve 5 sinir sesleri 4 dil ve yerel ayarlar kullanılabilir. Tam bir listesi için bkz [desteklenen diller](language-support.md#text-to-speech).

Metin okuma teknolojisi, içerik oluşturucuların kullanıcılarının ile farklı yöntemle etkileşim kurmasına imkan tanır. Metin okuma, kullanımı içeriklerle etkileşim için bir seçenek ile kullanıcılara sağlayarak erişilebilirlik artırabilir. Metin okuma, kullanıcı bir görsel engelliler, bir öğrenme engelli veya sürüş Gezinti bilgilerini gerektiriyor olsun, var olan bir deneyim artırabilir. Metin okuma da bir değerli ses robotlar ve sanal Yardımcıları eklentisidir.

### <a name="neural-voices"></a>Sinir sesleri

Sinir sesleri etkileşimleri sohbet robotları ve sanal Yardımcıları ile daha doğal yapmasına ve ilgi çekici, e-kitapları gibi dijital metinleri audiobooks dönüştürmek ve içi navigasyon sistemleri geliştirir kullanılabilir. Yapay ZEKA sistemlerle etkileşim kurduğunuzda İnsan benzeri doğal prosody ve sözcük Temizle articulation sinir sesleri dinleme yorulma ciddi ölçüde düşürün. Sinir sesleri hakkında daha fazla bilgi için bkz: [desteklenen diller](language-support.md#text-to-speech).

### <a name="custom-voices"></a>Özel ses

Ses özelleştirme markanız için tanınan, tür, tek bir ses oluşturmanıza olanak sağlar. Özel ses tipi oluşturmak için studio kaydını yapabilir ve ilişkili betikler eğitim verileri olarak karşıya yükleyin. Hizmet, ardından kaydınız için ayarlanmış bir benzersiz ses modeli oluşturur. Bu özel ses tipi konuşma sentezlemek için kullanabilirsiniz. Daha fazla bilgi için [özel seslerle](how-to-customize-voice-font.md).

## <a name="core-features"></a>Temel özellikleri

Bu tabloda metin okuma için temel özellikleri listelenmektedir:

| Kullanım örneği | SDK | REST |
|----------|-----|------|
| Metni konuşmaya dönüştürün. | Hayır | Evet |
| Veri kümeleri için ses uyarlama karşıya yükleyin. | Hayır | Evet\* |
| Oluşturun ve ses yazı tipi modelleri yönetin. | Hayır | Evet\* |
| Oluşturun ve ses yazı tipi dağıtımları yönetin. | Hayır | Evet\* |
| Oluşturun ve ses yazı tipi testleri yönetin. | Hayır | Evet\* |
| Aboneliklerini yönetin. | Hayır | Evet\* |

\* *Bu hizmetler cris.ai uç noktayı kullanarak kullanılabilir. Bkz: [Swagger başvuru](https://westus.cris.ai/swagger/ui/index).*

> [!NOTE]
> Metin okuma uç nokta istekleri 5 saniye başına 25 sınırlayan azaltma uygular. Azaltma ortaya çıktığında, ileti üstbilgileri bildirilir.

## <a name="get-started-with-text-to-speech"></a>Metin okuma ile çalışmaya başlama

Hızlı başlangıçlar, kodu 10 dakikadan kısa bir süre içinde çalıştırmak için tasarlanmış sunuyoruz. Bu tablo, metin okuma Hızlı başlangıçlar dile göre düzenlenmiş bir listesini içerir.

| Hızlı Başlangıç | Platform | API başvurusu |
|------------|----------|---------------|
| [C#, .NET Core](quickstart-dotnet-text-to-speech.md) | Windows, macOS, Linux | [Göz atma](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Node.js](quickstart-nodejs-text-to-speech.md) | Pencere, macOS, Linux | [Göz atma](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Python](quickstart-python-text-to-speech.md) | Pencere, macOS, Linux | [Göz atma](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |

## <a name="sample-code"></a>Örnek kod

Metin okuma için örnek kod, Github'da kullanılabilir. Bu örnekler, en popüler programlama dillerinden metinden konuşmaya dönüştürme kapsar.

* [Metin okuma örnekleri (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)

## <a name="reference-docs"></a>Başvuru belgeleri

* [Konuşma SDK'sı](speech-sdk-reference.md)
* [Konuşma cihaz SDK'sı](speech-devices-sdk.md)
* [REST API: Konuşma metin](rest-speech-to-text.md)
* [REST API: Metin okuma](rest-text-to-speech.md)
* [REST API: Batch tanıma ve özelleştirme](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Sonraki adımlar

* [Serbest konuşma Hizmetleri aboneliği edinin](get-started.md)
* [Özel ses tipi olarak oluşturma](how-to-customize-voice-font.md)
