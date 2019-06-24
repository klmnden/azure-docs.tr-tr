---
title: Azure konuşma Hizmetleri nelerdir?
titleSuffix: Azure Cognitive Services
description: Azure konuşma Hizmetleri birleştirmesi konuşma metin, metin okuma ve konuşma çevirisi tek bir Azure aboneliği içinde oluşturulur. Konuşma, uygulamalar, Araçlar ve Speech SDK'sı, konuşma cihazları SDK veya REST API'leri ile cihazları eklemek kolaydır. Konuşma işlevselliği eklemek için var olan bir sohbet Robotu, metin okuma çevirisi uygulamasında dönüştürmek veya büyük çağrı merkezi veri birimleri özelliği.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: overview
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 4750937ee7ef6230ed4635d739a102a501b19a30
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341808"
---
# <a name="what-are-the-speech-services"></a>Konuşma Tanıma Hizmetleri nelerdir?

Azure konuşma konuşma metin okuma ve konuşma çevirisi birleştirmesi tek bir Azure aboneliği içine hizmetleridir. Konuşma kolaydır, uygulamaları, araçları ve cihazlarıyla etkinleştirme [Speech SDK'sı](speech-sdk-reference.md), [konuşma cihaz SDK'sı](https://aka.ms/sdsdk-quickstart), veya [REST API'leri](rest-apis.md).

> [!IMPORTANT]
> Bing konuşma API'si, Translator konuşma çevirisi ve özel konuşma tanıma, konuşma Hizmetleri yerini almıştır. Bkz: *nasıl yapılır kılavuzları > geçiş* geçiş yönergeleri için.

Bu özellikler Azure konuşma Hizmetleri ' hale getirir. Bağlantıları, her özellik için yaygın kullanım örnekleri hakkında daha fazla bilgi edinmek veya API Başvurusu betiğe gitmek için bu tabloyu kullanın.

| Hizmet | Özellik | Açıklama | SDK | REST |
|---------|---------|-------------|-----|------|
| [Konuşma metin](speech-to-text.md) | Konuşmayı Metne Dönüştürme | Konuşma metin, ses akışları gerçek zamanlı olarak, uygulamalar, Araçlar veya cihazları kullanma veya görüntüleme metne dönüştürür. Konuşma metin ile kullanmak [Language Understanding (LUIS)](https://docs.microsoft.com/azure/cognitive-services/luis/) transcribed konuşma tanıma ve sesli komutları üzerinde işlem yapma kullanıcı hedefleri türetmek için. | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| | [Batch Transkripsiyon](batch-transcription.md) | Batch transkripsiyonu, zaman uyumsuz metne dönüştürme konuşma transkripsiyonu büyük hacimdeki verileri sağlar. Bu özelleştirme ve model Yönetimi aynı uç noktasını kullanan bir REST tabanlı hizmetidir. | Hayır | [Evet](https://westus.cris.ai/swagger/ui/index) |
| | [Konuşma tanıma](conversation-transcription-service.md) | Gerçek zamanlı konuşma tanıma, konuşmacı tanıma ve diarization sağlar. Yüz yüze toplantılar konuşmacıları ayırt olanağı fotoğrafını için idealdir. | Evet | Hayır |
| | [Özel konuşma modelleri oluşturma](#customize-your-speech-experience) | Konuşma metin tanıma ve benzersiz bir ortamda transkripsiyonu için kullanıyorsanız, oluşturabilir ve adresi ortam gürültü veya sektöre özel sözlük özel akustik ve dil telaffuz modellerini eğitin. | Hayır | [Evet](https://westus.cris.ai/swagger/ui/index) |
| [Metin okuma](text-to-speech.md) | Metin okuma | Metin okuma, İnsan benzeri Sentezlenen konuşma kullanarak metin girişi dönüştürür [konuşma sentezi işaretleme dili (SSML'yi)](text-to-speech.md#speech-synthesis-markup-language-ssml). Standart seslerle ve sinir kişilerden daha fazlasını seçin (bkz [dil desteği](language-support.md)). | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| | [Özel ses oluşturma](#customize-your-speech-experience) | Özel ses tipi markanız veya ürün için benzersiz oluşturun. | Hayır | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Konuşma Çevirisi](speech-translation.md) | Konuşma çevirisi | Konuşma çevirisi, konuşma, uygulamalar, Araçlar ve cihazlar için gerçek zamanlı, çoklu dil çevirisi sağlar. Konuşma tanıma ve konuşma tanıma ve konuşma metin çevirisi için bu hizmeti kullanın. | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | Hayır |
| [Ses öncelikli sanal Yardımcıları](voice-first-virtual-assistants.md) | Ses öncelikli sanal Yardımcıları | Azure konuşma Hizmetleri kullanarak özel sanal Yardımcıları, geliştiricilere uygulamalarını ve deneyimler için doğal olarak, İnsan benzeri damıtarak konuşma bağlamında kullanılabilen arabirimlerini oluşturma olanağı sunun. Bot Framework'ün doğrudan satır konuşma kanal, ses, sesli etkileşim düşük gecikme süresi ve yüksek güvenilirlikle kullanıma sağlayan uyumlu bir bot için Eşgüdümlü, düzenlenmiş giriş noktası sunarak bu özelliklerini geliştirir. | [Evet](voice-first-virtual-assistants.md) | Hayır |

## <a name="news-and-updates"></a>Haberler ve güncelleştirmeler

İle Azure konuşma Hizmetleri yenilikleri öğrenin.

* Mayıs 2019 - belgeleri artık kullanılabilir [konuşma Transkripsiyonu](conversation-transcription-service.md), [çağrı merkezi Transkripsiyonu](call-center-transcription.md), ve [ses öncelikli sanal Yardımcıları](voice-first-virtual-assistants.md).
* Mayıs 2019
    * Konuşma SDK 1.5.1 yayımladı. Güncelleştirmeleri, yenilikleri ve bilinen sorunların tam listesi için bkz [sürüm notları](releasenotes.md).
    * Konuşma SDK 1.5.0 yayımladı. Güncelleştirmeleri, yenilikleri ve bilinen sorunların tam listesi için bkz [sürüm notları](releasenotes.md).
* Nisan 2019 - serbest konuşma SDK 1.4.0 C++ için metin okuma (Beta) desteğiyle C#ve Windows ve Linux üzerinde Java. Ayrıca, SDK'sı artık MP3 ve geçerli/Ogg ses biçimleri için C++ destekler ve C# Linux üzerinde. Güncelleştirmeleri, yenilikleri ve bilinen sorunların tam listesi için bkz [sürüm notları](releasenotes.md).
* Mart 2019 - belirli bir bölgede kullanılabilir sesleri tam bir listesini döndürür (TTS) okuma için yeni bir uç noktası artık kullanılabilir. Ayrıca, yeni bölgelere TTS için artık desteklenmektedir. Daha fazla bilgi için [metin okuma API Başvurusu (REST)](rest-text-to-speech.md).

## <a name="try-speech-services"></a>Konuşma Hizmetleri deneyin

Hızlı başlangıçlar, en popüler programlama dillerinden, her, kodu 10 dakikadan kısa bir süre içinde çalıştırmak için tasarlanmış sunuyoruz. Bu tablo her bir özellik için en popüler hızlı başlangıçları içerir. Ek diller ve platformlar keşfetmek için sol taraftaki gezinti kullanın.

| Konuşmayı metne dönüştürme (SDK) | Metin okuma (SDK) | Çeviri (SDK) |
|----------------------|----------------------|-------------------|
| [C#, .NET Core (Windows)](quickstart-csharp-dotnet-windows.md) | [C#, .NET Framework (Windows)](quickstart-text-to-speech-dotnet-windows.md) | [Java (Windows, Linux)](quickstart-translate-speech-java-jre.md) |
| [JavaScript (tarayıcı)](quickstart-js-browser.md) | [C++ (Windows)](quickstart-text-to-speech-cpp-windows.md) | [C#, .NET Core (Windows)](quickstart-translate-speech-dotnetcore-windows.md) |
| [Python (Windows, Linux, macOS)](quickstart-python.md) | [C++ (Linux)](quickstart-text-to-speech-cpp-linux.md) | [C#, .NET Framework (Windows)](quickstart-translate-speech-dotnetframework-windows.md) |
| [Java (Windows, Linux)](quickstart-java-jre.md) | | [C++ (Windows)](quickstart-translate-speech-cpp-windows.md) |

> [!NOTE]
> Konuşmayı metne ve metin okuma da REST uç noktaları ve ilişkili hızlı başlangıçlar vardır.

Konuşma hizmetleri kullanmak üzere ettikten sonra amaçlardan tutun LUIS ve Speech SDK'sı kullanarak konuşma tanıma öğretir öğreticimizi deneyin.

* [Öğretici: Amaçlardan tutun Speech SDK'sı ve LUIS ile Konuşma tanıma,C#](how-to-recognize-intents-from-speech-csharp.md)
* [Öğretici: Metinleri çevirin, duyguları çözümleyin ve çevrilmiş metin okuma, REST sentezlemek için bir Flask uygulaması derleme](https://docs.microsoft.com/azure/cognitive-services/translator/tutorial-build-flask-app-translation-synthesis?toc=%2fazure%2fcognitive-services%2fspeech-service%2ftoc.json&bc=%2fazure%2fcognitive-services%2fspeech-service%2fbreadcrumb%2ftoc.json&toc=%2Fen-us%2Fazure%2Fcognitive-services%2Fspeech-service%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)

## <a name="get-sample-code"></a>Örnek kodu alma

Örnek kodu Github'da Azure konuşma hizmetlerinin her biri için kullanılabilir. Bu örnekler bir dosya veya akıştan, sürekli ve tek tanıma, ses okuma ve özel modelleriyle çalışma gibi yaygın senaryoları kapsar. Bu bağlantıları, görüntülemek için kullanın. SDK'sını ve REST örnekleri:

* [Konuşma metin okuma ve konuşma çevirisi örnekleri (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
* [Batch transkripsiyonu örnekleri (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)
* [Metin okuma örnekleri (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)

## <a name="customize-your-speech-experience"></a>Konuşma deneyiminizi özelleştirin

Azure konuşma Hizmetleri yerleşik modelleri ile de çalışır, ancak daha fazla özelleştirme ve deneyimini ürün veya ortamı ayarlamak isteyebilirsiniz. Markanız için benzersiz bir ses tipleri için akustik model özelleştirme seçenekleri arasındadır. Özel bir model oluşturduktan sonra Azure konuşma Hizmetleri birini kullanabilirsiniz.

| Konuşma Hizmeti | Model | Açıklama |
|----------------|-------|-------------|
| Konuşmayı Metne Dönüştürme | [Akustik model](how-to-customize-acoustic-models.md) | Uygulamalar, Araçlar, özel akustik model oluşturma veya cihazlar özellikle bir araba ya da bir Fabrika katı, her biri belirli bir kaydı koşullar ortamları gibi kullanılır. Vurgulu konuşma, belirli bir arka plan gürültüleri veya belirli bir mikrofon kaydı için kullanarak örnek verilebilir. |
| | [Dil modeli](how-to-customize-language-model.md) | Alana özgü sözlük ve tıbbi terminolojisi ya da BT terminolojisinin gibi dil bilgisi dökümünün geliştirmek için bir özel dil modeli oluşturun. |
| | [Söyleniş modeli](how-to-customize-pronunciation.md) | Bir özel telaffuz modeliyle fonetik formu ve görüntüleme bir sözcük veya terimi tanımlayabilirsiniz. Ürün adları veya kısaltmalar gibi özelleştirilmiş koşullarını işlemek için kullanışlıdır. Başlamak için ihtiyacınız olan telaffuz dosya--basit .txt dosyası. |
| Metin Okuma | [Ses tipi](how-to-customize-voice-font.md) | Özel ses tipi markanız için tanınan, tür, tek bir ses oluşturmanıza imkan tanır. Yalnızca veri kullanmaya başlamak için az miktarda alır. Daha fazla veri sağlarsanız, daha doğal ve İnsan benzeri, ses tipi duyulacaktır. |

## <a name="reference-docs"></a>Başvuru belgeleri

* [Konuşma SDK'sı](speech-sdk-reference.md)
* [Konuşma cihaz SDK'sı](speech-devices-sdk.md)
* [REST API: Konuşma metin](rest-speech-to-text.md)
* [REST API: Metin okuma](rest-text-to-speech.md)
* [REST API: Batch tanıma ve özelleştirme](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir konuşma Hizmetleri abonelik anahtarı ücretsiz olarak edinin](get-started.md)
