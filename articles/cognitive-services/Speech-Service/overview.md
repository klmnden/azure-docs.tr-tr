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
ms.date: 03/13/2019
ms.author: erhopf
ms.openlocfilehash: 06b2a5211c720d50a2f14e5fa56fa296cb80d41f
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58519638"
---
# <a name="what-are-the-speech-services"></a>Konuşma Tanıma Hizmetleri nelerdir?

Azure konuşma konuşma metin okuma ve konuşma çevirisi birleştirmesi tek bir Azure aboneliği içine hizmetleridir. Konuşma kolaydır, uygulamaları, araçları ve cihazlarıyla etkinleştirme [Speech SDK'sı](speech-sdk-reference.md), [konuşma cihaz SDK'sı](speech-devices-sdk-qsg.md), veya [REST API'leri](rest-apis.md).

> [!IMPORTANT]
> Bing konuşma API'si, Translator konuşma çevirisi ve özel konuşma tanıma, konuşma Hizmetleri yerini almıştır. Bkz: *nasıl yapılır kılavuzları > geçiş* geçiş yönergeleri için.

Bu özellikler Azure konuşma Hizmetleri ' hale getirir. Bağlantıları, her özellik için yaygın kullanım örnekleri hakkında daha fazla bilgi edinmek veya API Başvurusu betiğe gitmek için bu tabloyu kullanın.

| Hizmet | Özellik | Açıklama | SDK | REST |
|---------|---------|-------------|-----|------|
| [Konuşma metin](speech-to-text.md) | Konuşmayı metne dönüştürme | Konuşma metin, ses akışları gerçek zamanlı olarak, uygulamalar, Araçlar veya cihazları kullanma veya görüntüleme metne dönüştürür. Konuşma metin ile kullanmak [Language Understanding (LUIS)](https://docs.microsoft.com/azure/cognitive-services/luis/) transcribed konuşma tanıma ve sesli komutları üzerinde işlem yapma kullanıcı hedefleri türetmek için. | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| | [Batch Transkripsiyon](batch-transcription.md) | Batch transkripsiyonu, zaman uyumsuz metne dönüştürme konuşma transkripsiyonu büyük hacimdeki verileri sağlar. Bu özelleştirme ve model Yönetimi aynı uç noktasını kullanan bir REST tabanlı hizmetidir. | Hayır | [Evet](https://westus.cris.ai/swagger/ui/index) |
| | [Özelleştirme](#customize-your-speech-experience) | Konuşma metin tanıma ve benzersiz bir ortamda transkripsiyonu için kullanıyorsanız, oluşturabilir ve adresi ortam gürültü veya sektöre özel sözlük özel akustik ve dil telaffuz modellerini eğitin. | Hayır | [Evet](https://westus.cris.ai/swagger/ui/index) |
| [Metin okuma](text-to-speech.md) | Metin okuma | Metin okuma, giriş metni İnsan benzeri Sentezlenen konuşmaya dönüştürür. Standart seslerle ve sinir kişilerden daha fazlasını seçin (bkz [dil desteği](language-support.md)). | Hayır | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| | [Özelleştirme](#customize-your-speech-experience) | Özel ses tipi markanız veya ürün için benzersiz oluşturun. | Hayır | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Konuşma Çevirisi](speech-translation.md) | Konuşma çevirisi | Konuşma çevirisi, konuşma, uygulamalar, Araçlar ve cihazlar için gerçek zamanlı, çoklu dil çevirisi sağlar. Konuşma tanıma ve konuşma tanıma ve konuşma metin çevirisi için bu hizmeti kullanın. | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | Hayır |

## <a name="news-and-updates"></a>Haberler ve güncelleştirmeler

İle Azure konuşma Hizmetleri yenilikleri öğrenin.

* Mart 2019 - belirli bir bölgede kullanılabilir sesleri tam bir listesini döndürür metin okuma için yeni bir uç noktası artık kullanılabilir. Daha fazla bilgi için [metin okuma API Başvurusu (REST)](rest-text-to-speech.md).
* Şubat 2019 - serbest konuşma SDK 1.3.0 desteğiyle [Unity (beta)](quickstart-csharp-unity.md). İçin destek eklendi `AudioInput` ses akış kaynağı seçmenize olanak sağlayan sınıf. Yenilikleri ve bilinen sorunların tam listesi için bkz: [sürüm notları](releasenotes.md).
* Aralık 2018'e - serbest konuşma SDK 1.2.0 desteğiyle [Python](quickstart-python.md) ve [Node.js](quickstart-js-node.md), Ubuntu LTS 18.04 yanı sıra. Daha fazla bilgi için [sürüm notları](releasenotes.md).

## <a name="try-speech-services"></a>Konuşma Hizmetleri deneyin

Hızlı başlangıçlar, en popüler programlama dillerinden, her, kodu 10 dakikadan kısa bir süre içinde çalıştırmak için tasarlanmış sunuyoruz. Bu tablo her bir özellik için en popüler hızlı başlangıçları içerir. Ek diller ve platformlar keşfetmek için sol taraftaki gezinti kullanın.

| Konuşmayı metne dönüştürme (SDK) | Çeviri (SDK) | Metin okuma (REST) |
|-------------------|-------------------|-----------------------|
| [C#, .NET Core (Windows)](quickstart-csharp-dotnet-windows.md) | [Java (Windows, Linux)](quickstart-translate-speech-java-jre.md) | [Python (Windows, Linux, macOS)](quickstart-python-text-to-speech.md) |
| [JavaScript (tarayıcı)](quickstart-js-browser.md) | [C#, .NET Core (Windows)](quickstart-translate-speech-dotnetcore-windows.md) | [C#, .NET Core (Windows, Linux, macOS)](quickstart-dotnet-text-to-speech.md) |
| [Python (Windows, Linux, macOS)](quickstart-python.md) | [C#, .NET Framework (Windows)](quickstart-translate-speech-dotnetframework-windows.md) | [Node.js (Windows, Linux, macOS)](quickstart-nodejs-text-to-speech.md) |
| [Java (Windows, Linux)](quickstart-java-jre.md) | [C++ (Windows)](quickstart-translate-speech-cpp-windows.md) | |

Konuşma hizmetleri kullanmak üzere ettikten sonra amaçlardan tutun LUIS ve Speech SDK'sı kullanarak konuşma tanıma öğretir öğreticimizi deneyin.

* [Öğretici: Amaçlardan tutun Speech SDK'sı ve LUIS ile Konuşma tanıma,C#](how-to-recognize-intents-from-speech-csharp.md)

## <a name="get-sample-code"></a>Örnek kodu alma

Örnek kodu Github'da Azure konuşma hizmetlerinin her biri için kullanılabilir. Bu örnekler bir dosya veya akıştan, sürekli ve tek tanıma, ses okuma ve özel modelleriyle çalışma gibi yaygın senaryoları kapsar. Bu bağlantıları, görüntülemek için kullanın. SDK'sını ve REST örnekleri:

* [Konuşma metin ve konuşma çevirisi örnekleri (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
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
