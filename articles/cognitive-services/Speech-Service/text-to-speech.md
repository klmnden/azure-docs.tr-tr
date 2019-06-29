---
title: Azure konuşma Hizmetleri ile metin okuma
titleSuffix: Azure Cognitive Services
description: Azure konuşma Services'dan metin okuma, uygulamalar, Araçlar veya cihazları doğal insan benzeri Sentezlenen konuşmaya metni dönüştürmek sağlayan bir hizmettir. Standart ve sinir ses arasından seçin veya kendi özel sesli benzersiz ürün veya marka oluşturun. 75'ten fazla standart sesleri 45'den fazla dil ve yerel ayar kullanılabilir ve 5 sinir sesleri 4 dil ve yerel ayarlar kullanılabilir.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: erhopf
ms.openlocfilehash: da7259585ad66ac9b58131ce834d82e7b3d4bcf2
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67466957"
---
# <a name="what-is-text-to-speech"></a>Metin okuma nedir?

Azure konuşma Services'dan metin okuma, uygulamalar, Araçlar veya cihazları doğal insan benzeri Sentezlenen konuşmaya metni dönüştürmek sağlayan bir hizmettir. Standart ve sinir ses arasından seçin veya kendi özel sesli benzersiz ürün veya marka oluşturun. 75'ten fazla standart sesleri 45'den fazla dil ve yerel ayar kullanılabilir ve 5 sinir sesleri 4 dil ve yerel ayarlar kullanılabilir. Tam bir listesi için bkz [desteklenen diller](language-support.md#text-to-speech).

Metin okuma teknolojisi, içerik oluşturucuların kullanıcılarının ile farklı yöntemle etkileşim kurmasına imkan tanır. Metin okuma, kullanımı içeriklerle etkileşim için bir seçenek ile kullanıcılara sağlayarak erişilebilirlik artırabilir. Metin okuma, kullanıcı bir görsel engelliler, bir öğrenme engelli veya sürüş Gezinti bilgilerini gerektiriyor olsun, var olan bir deneyim artırabilir. Metin okuma da bir değerli ses robotlar ve sanal Yardımcıları eklentisidir.


Konuşma sentezi işaretleme dili (SSML'yi), bir XML-tabanlı işaretleme dili yararlanarak metin okuma hizmeti kullanan geliştiriciler nasıl giriş metni belirtebilirsiniz Sentezlenen konuşmaya dönüştürülür. SSML'yi ile aralık, Söyleniş, oranı, birim ve daha fazlası gibi ayarlayabilirsiniz. Daha fazla bilgi için [SSML'yi](#speech-synthesis-markup-language-ssml).

### <a name="standard-voices"></a>Standart sesler

Standart sesleri istatistiksel parametrik sentezi ve/veya birleştirme sentezi teknikleri kullanılarak oluşturulur. Bu ses oldukça anlaşılır ve ses doğal ' dir. Çok çeşitli sesli seçenekleri ile 45'den fazla dilde konuşmak uygulamalarınızı kolayca etkinleştirebilirsiniz. Bu ses kısaltmalar, harflendirme genişletmeleri, tarih/saat ınterpretations polyphones ve daha fazlası için destek dahil olmak üzere telaffuz yüksek doğruluk sağlar. Standart kullanıcıların içeriğinizi kullanımı etkileşime girmesine izin vererek erişilebilirlik uygulamaları ve hizmetleri geliştirmek için kullanın.

### <a name="neural-voices"></a>Sinir sesleri

Sinir sesleri stres ve konuşulan dili ve konuşma birimlerinin bir bilgisayar ses synthesizing tonlama desenlerle eşleşen geleneksel metin okuma sistemlere sınırlarını üstesinden gelmek için derin sinir ağları kullanın. Standart metinden konuşmaya prosody ayrı dil analizi ve içinde muffled ses birleştirme sonuçlanabilir bağımsız modelleri tarafından yönetilir akustik tahmin adımları halinde ayırır. Bizim sinir özelliği bir daha esnektir, doğal görünen ses sonuçlanır prosody öngörü ve ses sentezi eşzamanlı olarak yapar.

Sinir sesleri etkileşimleri sohbet robotları ve sanal Yardımcıları ile daha doğal yapmasına ve ilgi çekici, e-kitapları gibi dijital metinleri audiobooks dönüştürmek ve içi navigasyon sistemleri geliştirir kullanılabilir. Yapay ZEKA sistemlerle etkileşim kurduğunuzda İnsan benzeri doğal prosody ve sözcük Temizle articulation sinir sesleri dinleme yorulma ciddi ölçüde düşürün.

Farklı stillerde, nötr gibi ve neşeli sinir kişilerden daha fazlasını destekler. Örneğin, Jessa (en-US) ses cheerfully, normal, mutlu konuşma için optimize edilmiştir konuşabilirsiniz. Sesi, aralık, gibi ses çıkış ayarlama ve kullanma hızlandırmak [konuşma sentezi biçimlendirme dili](speech-synthesis-markup.md). Kullanılabilir seslerini tam bir listesi için bkz. [desteklenen diller](language-support.md#text-to-speech).

Sinir sesleri avantajları hakkında daha fazla bilgi için bkz: [yardımcı kişi gibi konuşun makineler Microsoft'un yeni sinir metin okuma hizmeti](https://azure.microsoft.com/blog/microsoft-s-new-neural-text-to-speech-service-helps-machines-speak-like-people/).

### <a name="custom-voices"></a>Özel ses

Ses özelleştirme markanız için tanınan, tür, tek bir ses oluşturmanıza olanak sağlar. Özel ses tipi oluşturmak için studio kaydını yapabilir ve ilişkili betikler eğitim verileri olarak karşıya yükleyin. Hizmet, ardından kaydınız için ayarlanmış bir benzersiz ses modeli oluşturur. Bu özel ses tipi konuşma sentezlemek için kullanabilirsiniz. Daha fazla bilgi için [özel seslerle](how-to-customize-voice-font.md).

## <a name="speech-synthesis-markup-language-ssml"></a>Konuşma Sentezi Biçimlendirme Dili (SSML)

Konuşma sentezi işaretleme dili (SSML'yi), geliştiricilerin nasıl Giriş bir metin belirtmenize olanak tanıyan bir XML-tabanlı işaretleme dili Sentezlenen konuşmaya metin okuma hizmeti kullanılarak dönüştürülür ' dir. Düz metne kıyasla oranı, birim ve diğer metin okuma çıkış Konuşmayı SSML'yi geliştiricilerin aralık, Söyleniş, ince ayar olanak tanır. Bir süre sonra duraklatma veya bir soru işareti ile bir cümle sona erdiğinde doğru tonlama kullanma gibi normal noktalama, otomatik olarak işlenir.

Metin okuma hizmetine gönderilen tüm metin girişi SSML'yi yapılandırılmış olmalıdır. Daha fazla bilgi için [konuşma sentezi biçimlendirme dili](speech-synthesis-markup.md).

### <a name="pricing-note"></a>Not fiyatlandırması

Metin okuma hizmeti kullanılırken, noktalama işaretleri dahil olmak üzere konuşma, dönüştürülen her karakter için faturalandırılırsınız. SSML'yi belge Faturalanabilir olmamasına karşın, metin okuma, Fonem ve sıklıkta konuşulur, gibi nasıl dönüştürülür ayarlamak için kullanılan isteğe bağlı öğeler Faturalanabilir karakter olarak sayılır. Faturalanabilir nedir listesi aşağıda verilmiştir:

* Metin okuma hizmetine istek SSML'yi gövdesinde geçirilen metin
* İstek gövdesi SSML'yi biçiminde metin alanı içindeki tüm biçimlendirme dışında `<speak>` ve `<voice>` etiketleri
* Harf, noktalama, boşluk, sekme, biçimlendirme ve tüm boşluk karakterlerinin
* Unicode olarak tanımlanan her kod noktası

Ayrıntılı bilgi için bkz. [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

> [!IMPORTANT]
> Her Çince, Japonca ve Korece dil karakter, faturalandırma için iki karakter olarak sayılır.

## <a name="core-features"></a>Temel özellikleri

Bu tabloda metin okuma için temel özellikleri listelenmektedir:

| Kullanım örneği | SDK | REST |
|----------|-----|------|
| Metni konuşmaya dönüştürün. | Evet | Evet |
| Veri kümeleri için ses uyarlama karşıya yükleyin. | Hayır | Evet\* |
| Oluşturun ve ses yazı tipi modelleri yönetin. | Hayır | Evet\* |
| Oluşturun ve ses yazı tipi dağıtımları yönetin. | Hayır | Evet\* |
| Oluşturun ve ses yazı tipi testleri yönetin. | Hayır | Evet\* |
| Aboneliklerini yönetin. | Hayır | Evet\* |

\* *Bu hizmetler cris.ai uç noktayı kullanarak kullanılabilir. Bkz: [Swagger başvuru](https://westus.cris.ai/swagger/ui/index). Bu özel üslup eğitimi ve yönetim API'leri sınırları istekleri 5 saniye başına 25 konuşma sentezi API'nin, azaltma uygular izin verdiğini olarak en yüksek saniye başına 200 istek azaltma uygular. Azaltma ortaya çıktığında, ileti üstbilgileri bildirilir.*

## <a name="get-started-with-text-to-speech"></a>Metin okuma ile çalışmaya başlama

Hızlı başlangıçlar, kodu 10 dakikadan kısa bir süre içinde çalıştırmak için tasarlanmış sunuyoruz. Bu tablo, metin okuma Hızlı başlangıçlar dile göre düzenlenmiş bir listesini içerir.

### <a name="sdk-quickstarts"></a>SDK hızlı başlangıçları

| Hızlı Başlangıç (SDK) | Platform | API başvurusu |
|------------|----------|---------------|
| [C#, .NET Core](quickstart-text-to-speech-dotnetcore.md) | Windows | [Göz atma](https://aka.ms/csspeech/csharpref) |
| [C#, .NET Framework](quickstart-text-to-speech-dotnet-windows.md) | Windows | [Göz atma](https://aka.ms/csspeech/csharpref) |
| [C#, UWP](quickstart-text-to-speech-csharp-uwp.md) | Windows | [Göz atma](https://aka.ms/csspeech/csharpref) |
| [C#, Unity](quickstart-text-to-speech-csharp-unity.md) | Windows, Android | [Göz atma](https://aka.ms/csspeech/csharpref) |
| [C++](quickstart-text-to-speech-cpp-windows.md) | Windows | [Göz atma](https://aka.ms/csspeech/cppref) |
| [C++](quickstart-text-to-speech-cpp-linux.md) | Linux | [Göz atma](https://aka.ms/csspeech/cppref) |

### <a name="rest-quickstarts"></a>DİĞER hızlı başlangıçlar

| Hızlı Başlangıç (REST) | Platform | API başvurusu |
|------------|----------|---------------|
| [C#, .NET Core](quickstart-dotnet-text-to-speech.md) | Windows, macOS, Linux | [Göz atma](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Node.js](quickstart-nodejs-text-to-speech.md) | Pencere, macOS, Linux | [Göz atma](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Python](quickstart-python-text-to-speech.md) | Pencere, macOS, Linux | [Göz atma](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |

## <a name="sample-code"></a>Örnek kod

Metin okuma için örnek kod, Github'da kullanılabilir. Bu örnekler, en popüler programlama dillerinden metinden konuşmaya dönüştürme kapsar.

* [Metin okuma örnekleri (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
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
