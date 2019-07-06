---
title: Konuşma çevirisi ile Azure konuşma Hizmetleri
titlesuffix: Azure Cognitive Services
description: Konuşma Hizmetleri, uygulamaları, araçları ve cihazlar için uçtan uca, gerçek zamanlı, çok dilli konuşma çevirisi eklemenize olanak sağlar. Aynı API hem konuşma okuma ve konuşma metin çevirisi için kullanılabilir.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 20e47b6d3243bb0cccbc42ab0ab904b72922e98b
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67604792"
---
# <a name="what-is-speech-translation"></a>Konuşma çevirisi nedir?

Konuşma çevirisi Azure konuşma Hizmetleri, gerçek zamanlı, çok dilli konuşma okuma ve konuşma metin çevirisi ses akışları sağlar. Konuşma SDK ile uygulamalarınızı, araçları ve cihazlarınızı kaynak döküm ve çeviri çıkışları sağlanan ses erişiminiz. Geçici transkripsiyonu ve çeviri sonuçları konuşma algılanır ve Sentezlenen konuşmaya finaller sonuçları dönüştürülebilir olarak döndürülür.

Microsoft'un çeviri altyapısı tarafından iki farklı yaklaşım desteklenir: istatistiksel makine çevirisi (SMT) ve sinirsel makine çevirisi (NMT). SMT verilen birkaç sözcük bağlamında en iyi olası çevirileri tahmin etmek için Gelişmiş istatistiksel çözümlemesini kullanır. NMT ile sinir ağları daha doğru doğal görünen çevirileri kelimeleri için tam cümlelerden bağlamında kullanarak sağlamak için kullanılır.

Bugün, Microsoft çeviri için en popüler diller NMT kullanır. Tüm [konuşma konuşma çevirisi için kullanılabilen dilleri](language-support.md#speech-translation) NMT tarafından desteklenir. Konuşma metin çeviri dili çifti bağlı olarak SMT veya NMT kullanabilir. Hedef Dil NMT tarafından desteklenen tüm çeviri NMT destekli olur. Hedef Dil NMT tarafından desteklenmiyor, çeviri NMT ve SMT İngilizce olarak bir "Özet" arasında iki dilleri kullanarak, bir karma değildir.

## <a name="core-features"></a>Temel özellikleri

REST API'leri ve Speech SDK'sı kullanılabilen özellikleri şunlardır:

| Kullanım örneği | SDK | REST |
|----------|-----|------|
| Tanıma sonuçları konuşma metin çevirisi. | Evet | Hayır |
| Konuşma konuşma çevirisi. | Evet | Hayır |
| Geçici algılama ve çeviri sonuçları. | Evet | Hayır |

## <a name="get-started-with-speech-translation"></a>Konuşma çevirisi ile çalışmaya başlama

Hızlı başlangıçlar, kodu 10 dakikadan kısa bir süre içinde çalıştırmak için tasarlanmış sunuyoruz. Bu tablo, konuşma çevirisi hızlı başlangıçlar dile göre düzenlenmiş bir listesini içerir.

| Hızlı Başlangıç | Platform | API başvurusu |
|------------|----------|---------------|
| [C#, .NET Core](quickstart-translate-speech-dotnetcore-windows.md) | Windows | [Göz atma](https://aka.ms/csspeech/csharpref) |
| [C#, .NET Framework](quickstart-translate-speech-dotnetframework-windows.md) | Windows | [Göz atma](https://aka.ms/csspeech/csharpref) |
| [C#, UWP](quickstart-translate-speech-uwp.md) | Windows | [Göz atma](https://aka.ms/csspeech/csharpref) |
| [C++](quickstart-translate-speech-cpp-windows.md) | Windows | [Göz atma](https://aka.ms/csspeech/cppref)|
| [Java](quickstart-translate-speech-java-jre.md) | Windows, Linux, macOS | [Göz atma](https://aka.ms/csspeech/javaref) |

## <a name="sample-code"></a>Örnek kod

Speech SDK'sı için örnek kod, Github'da kullanılabilir. Bu örnekler bir dosya veya akıştan, sürekli ve tek tanıma/çevirisi, ses okuma ve özel modelleriyle çalışma gibi yaygın senaryoları kapsar.

* [Konuşmayı metne ve çeviri örnekleri (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)

## <a name="migration-guides"></a>Geçiş kılavuzları

> [!WARNING]
> Translator konuşma çevirisi, 15 Ekim 2019 üzerinde kullanımdan.

Translator konuşma çevirisi, uygulamalar, Araçlar veya ürünleri kullanıyorsa kılavuzları konuşma Hizmetleri'ne geçirmenize yardımcı olması için oluşturduk.

* [Translator konuşma API'sini konuşma Services'a geçme](how-to-migrate-from-translator-speech-api.md)

## <a name="reference-docs"></a>Başvuru belgeleri

* [Konuşma SDK'sı](speech-sdk-reference.md)
* [Konuşma cihaz SDK'sı](speech-devices-sdk.md)
* [REST API: Konuşma metin](rest-speech-to-text.md)
* [REST API: Metin okuma](rest-text-to-speech.md)
* [REST API: Batch tanıma ve özelleştirme](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Sonraki adımlar

* [Bir konuşma Hizmetleri abonelik anahtarı ücretsiz olarak edinin](get-started.md)
* [Konuşma SDK'sı Al](speech-sdk.md)
