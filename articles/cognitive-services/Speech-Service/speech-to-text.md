---
title: Konuşma metin ile Azure konuşma Hizmetleri
titleSuffix: Azure Cognitive Services
description: Konuşma metin Azure konuşma Hizmetleri, olarak da bilinen Konuşmayı metne dönüştürme, etkinleştirir, uygulamalar, Araçlar veya cihazları kullanabilen bir metne ses akışları gerçek zamanlı döküm, görüntüleyin ve komut giriş olarak üzerinde işlem gerçekleştir. Bu hizmet Microsoft Cortana ve Office ürünleri için kullanan ve metin okuma ve çeviri ile sorunsuz çalışır aynı tanıma teknolojisini tarafından desteklenmektedir.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 9e6bc1264e668ba5c6593ce36e721f54e685c391
ms.sourcegitcommit: e43ea344c52b3a99235660960c1e747b9d6c990e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "59008501"
---
# <a name="what-is-speech-to-text"></a>Konuşmayı metne nedir?

Konuşma metin Azure konuşma Hizmetleri, olarak da bilinen Konuşmayı metne dönüştürme, etkinleştirir, uygulamalar, Araçlar veya cihazları kullanabilen bir metne ses akışları gerçek zamanlı döküm, görüntüleyin ve komut giriş olarak üzerinde işlem gerçekleştir. Bu hizmet Microsoft Cortana ve Office ürünleri için kullanan ve metin okuma ve çeviri ile sorunsuz çalışır aynı tanıma teknolojisini tarafından desteklenmektedir.  Konuşmayı metne diller tam bir listesi için bkz. [desteklenen diller](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/language-support#speech-to-text).

Varsayılan olarak, konuşma metin hizmetini Evrensel dil modelini kullanır. Bu model, Microsoft'a ait verileri kullanarak eğitim ve olduğu buluta dağıtılabilir. İçin en iyi damıtarak konuşma bağlamında kullanılabilen ve dikte senaryoları. Konuşma metin tanıma ve benzersiz bir ortamda transkripsiyonu için kullanıyorsanız, oluşturabilir ve adresi ortam gürültü veya sektöre özel sözlük özel akustik ve dil telaffuz modellerini eğitin. 

Kolayca mikrofondan gelen sesi yakalama, bir akıştan okumak veya REST API'leri ve Speech SDK'sı ile depolama biriminden ses dosyaları erişim. Speech SDK'sı, konuşma tanıma için WAV/PCM 16-bit, 16 kHz tek kanal ses destekler. Kullanarak ek ses biçimleri desteklenmektedir [konuşma metin REST uç noktasını](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) veya [batch transkripsiyonu hizmeti](https://docs.microsoft.com/azure/cognitive-services/speech-service/batch-transcription#supported-formats).

## <a name="core-features"></a>Temel özellikleri

REST API'leri ve Speech SDK'sı kullanılabilen özellikleri şunlardır:

| Kullanım örneği | SDK | REST |
|----------|-----|------|
| Kısa konuşma konuşmaların (< 15 saniye). Yalnızca son transkripsiyonu sonucu destekler. | Evet | Evet |
| Uzun konuşma ve ses akışını sürekli transkripsiyonu (> 15 saniye). Geçici ve son tanıma sonuçları destekler. | Evet | Hayır |
| Intents tanıma sonuçları ile türetilen [LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/what-is-luis). | Evet | Yok\* |
| Ses dosyaları, döküm zaman uyumsuz olarak toplu olarak. | Hayır | Evet\** |
| Oluşturun ve konuşma modelleri yönetin. | Hayır | Evet\** |
| Oluşturun ve özel model dağıtımı yönetin. | Hayır | Evet\** |
| Özel modelleri karşı temel model doğruluğunu ölçmek için doğruluk testi oluşturun. | Hayır | Evet\** |
| Aboneliklerini yönetin. | Hayır | Evet\** |

\* *LUIS amaç ve varlıkları kullanarak ayrı bir LUIS aboneliği türetilebilir. Bu abonelikle SDK LUIS çağırmanızı ve varlık ve hedefi sonuçlar sağlar. REST API ile LUIS çağırabilirsiniz kendiniz amaç ve varlıkları LUIS aboneliğinizle türetmek için.*

\** *Bu hizmetler cris.ai uç noktayı kullanarak kullanılabilir. Bkz: [Swagger başvuru](https://westus.cris.ai/swagger/ui/index).*

## <a name="get-started-with-speech-to-text"></a>Konuşmayı metne dönüştürme ile çalışmaya başlama

Hızlı başlangıçlar, en popüler programlama dillerinden, her, kodu 10 dakikadan kısa bir süre içinde çalıştırmak için tasarlanmış sunuyoruz. Bu tablo, dil tarafından düzenlenir ve Speech SDK'sı hızlı başlangıç kılavuzları tam bir listesini içerir.

| Hızlı Başlangıç | Platform | API başvurusu |
|------------|----------|---------------|
| [C#, .NET Core](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-csharp-dotnetcore-windows) | Windows | [Göz at](https://aka.ms/csspeech/csharpref) |
| [C#, .NET Framework](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-csharp-dotnet-windows) | Windows | [Göz at](https://aka.ms/csspeech/csharpref) |
| [C#, UWP](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-csharp-uwp) | Windows | [Göz at](https://aka.ms/csspeech/csharpref) |
| [C++](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-cpp-windows) | Windows | [Göz at](https://aka.ms/csspeech/cppref)|
| [C++](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-cpp-linux) | Linux | [Göz at](https://aka.ms/csspeech/cppref) |
| [Java](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-java-android) | Android | [Göz at](https://aka.ms/csspeech/javaref) |
| [Java](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-java-jre) | Windows, Linux | [Göz at](https://aka.ms/csspeech/javaref) |
| [JavaScript tarayıcı](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-js-browser) | Tarayıcı, Windows, Linux, macOS | [Göz at](https://aka.ms/AA434tv) |
| [JavaScript, Node.js](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-js-node) | Windows, Linux, macOS | [Göz at](https://aka.ms/AA434tv) |
| [Objective-C](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-objectivec-ios) | iOS | [Göz at](https://aka.ms/csspeech/objectivecref) |
| [Python](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-python) | Windows, Linux, macOS | [Göz at](https://aka.ms/AA434tr)  |

Konuşmayı metne REST hizmeti kullanmayı tercih ederseniz bkz [REST API'leri](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis).

## <a name="tutorials-and-sample-code"></a>Öğreticiler ve örnek kod

Konuşma hizmetleri kullanmak üzere ettikten sonra amaçlardan tutun LUIS ve Speech SDK'sı kullanarak konuşma tanıma öğretir öğreticimizi deneyin.

* [Öğretici: Amaçlardan tutun Speech SDK'sı ve LUIS ile Konuşma tanıma,C#](how-to-recognize-intents-from-speech-csharp.md)

Speech SDK'sı için örnek kod, Github'da kullanılabilir. Bu örnekler bir dosya veya akıştan, sürekli ve tek tanıma, ses okuma ve özel modelleriyle çalışma gibi yaygın senaryoları kapsar.

* [Konuşmayı metne örnekleri (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
* [Batch transkripsiyonu örnekleri (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)

## <a name="customization"></a>Özelleştirme

Evrensel modeli konuşma Hizmetleri tarafından kullanılan ek olarak, özel akustik ve dil telaffuz modelleri belirli deneyiminizi oluşturabilirsiniz. Özelleştirme seçeneklerinin bir listesi aşağıda verilmiştir:

| Model | Açıklama |
|-------|-------------|
| [Akustik model](how-to-customize-acoustic-models.md) | Özel akustik model oluşturma, uygulama, araçları veya cihazları belirli bir ortam gibi bir araba ya da belirli bir kaydı koşullarla Fabrika kullanılması durumunda yararlıdır. Örnek olarak aksanlı konuşma, belirli arka plan görüntüleri veya kayıt için belirli bir mikrofonun kullanılması verilebilir. |
| [Dil modeli](how-to-customize-language-model.md) | Sektöre özel sözlük ve tıbbi terminolojisi ya da BT terminolojisinin gibi dil bilgisi dökümünün geliştirmek için bir özel dil modeli oluşturun. |
| [Söyleniş modeli](how-to-customize-pronunciation.md) | Bir özel telaffuz modeliyle fonetik formu ve görüntüleme bir sözcük veya terimi tanımlayabilirsiniz. Ürün adları veya kısaltmalar gibi özelleştirilmiş koşullarını işlemek için kullanışlıdır. Başlamak için ihtiyacınız olan telaffuz dosya--basit .txt dosyası. |

> [!NOTE]
> Özelleştirme seçenekleri farklı dil/bölge tarafından (bkz [desteklenen diller](supported-languages.md)).

## <a name="migration-guides"></a>Geçiş kılavuzları

> [!WARNING]
> Bing konuşma 15 Ekim 2019 üzerinde kullanımdan.

Uygulamalar, Araçlar ya da ürünler Bing konuşma API'leri veya özel konuşma tanıma kullanıyorsanız, konuşma Hizmetleri'ne geçirmenize yardımcı olacak Kılavuzlar oluşturduk.

* [Bing konuşma içeriğinden konuşma Services'a geçme](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-migrate-from-bing-speech)
* [Özel konuşma içeriğinden konuşma Services'a geçme](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-migrate-from-custom-speech-service)

## <a name="reference-docs"></a>Başvuru belgeleri

* [Konuşma SDK'sı](speech-sdk-reference.md)
* [Konuşma Cihazları SDK’sı](speech-devices-sdk.md)
* [REST API: Konuşmayı metne dönüştürme](rest-speech-to-text.md)
* [REST API: Metin okuma](rest-text-to-speech.md)
* [REST API: Batch tanıma ve özelleştirme](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Sonraki adımlar

* [Bir konuşma Hizmetleri abonelik anahtarı ücretsiz olarak edinin](get-started.md)
* [Konuşma SDK'sı Al](speech-sdk.md)
