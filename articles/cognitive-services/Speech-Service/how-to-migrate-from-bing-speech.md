---
title: Bing konuşma içeriğinden Azure konuşma Services'a geçme
titleSuffix: Azure Cognitive Services
description: Azure konuşma Hizmetleri için mevcut bir Bing konuşma abonelikten geçirmeyi öğrenin.
services: cognitive-services
author: wsturman
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: gracez
ms.openlocfilehash: 33907437ab330278bdf7b023f6a93bd96e78cbad
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561330"
---
# <a name="migrate-from-bing-speech-to-the-speech-service"></a>Konuşma hizmeti için Bing konuşma içeriğinden geçirme

Uygulamalarınızı konuşma hizmeti için Bing konuşma tanıma API'SİNDEN geçirmek için bu makaleyi kullanın.

Bu makalede, Bing konuşma API'leri ve konuşma Hizmetleri arasındaki farkları özetler ve uygulamalarınızı geçiş stratejileri önerir. Bing konuşma API'si abonelik anahtarınızı konuşma hizmeti sayesinde işe yaramaz; Yeni bir konuşma Hizmetleri aboneliği gerekir.

Konuşma Hizmetleri tek bir abonelik anahtarı aşağıdaki özelliklere erişim verir. Hepsinin kullanımı bağımsız bir şekilde ölçüldüğünden yalnızca kullandığınız hizmetler için ödeme yapmış olursunuz.

* [Konuşmayı metne dönüştürme](speech-to-text.md)
* [Özel konuşmayı metne dönüştürme](https://cris.ai)
* [Metin okuma](text-to-speech.md)
* [Özel metin okuma sesleri](how-to-customize-voice-font.md)
* [Konuşma çevirisi ](speech-translation.md) ([Metin çevirisi](../translator/translator-info-overview.md) özelliğini kapsamaz)

[Speech SDK'sı](speech-sdk.md) Bing konuşma istemci kitaplıkları için bir işlev yerini almaktadır, ancak farklı bir API kullanır.

## <a name="comparison-of-features"></a>Özelliklerin karşılaştırması

Konuşma Hizmetleri ile aşağıdaki farklar Bing konuşma, büyük ölçüde benzer.

Özellik | Bing Konuşma | Konuşma Hizmetleri | Ayrıntılar
-|-|-|-
C++ SDK | : heavy_minus_sign: | :heavy_check_mark: | Konuşma Hizmetleri, Windows ve Linux destekler.
Java SDK | :heavy_check_mark: | :heavy_check_mark: | Konuşma Hizmetleri, Android ve konuşma cihazları destekler.
C# SDK’sı | :heavy_check_mark: | :heavy_check_mark: | Konuşma Hizmetleri, Windows 10 Evrensel Windows Platformu (UWP) ve .NET Standard 2.0 destekler.
Sürekli konuşma tanıma | 10 dakika | Sınırsız (SDK ile) | Bing konuşma hem konuşma Hizmetleri WebSockets protokolleri, çağrı başına 10 dakikaya kadar destekler. Ancak, Speech SDK'sı otomatik olarak zaman aşımından üzerinde yeniden veya kesin.
Kısmi veya Ara sonuçlar | :heavy_check_mark: | :heavy_check_mark: | WebSockets protokolü veya SDK'sı ile.
Özel konuşma modelleri | :heavy_check_mark: | :heavy_check_mark: | Bing konuşma ayrı bir özel konuşma aboneliği gerektirir.
Özel ses tipi | :heavy_check_mark: | :heavy_check_mark: | Bing konuşma ayrı bir özel sesli aboneliği gerektirir.
24 kHz sesleri | : heavy_minus_sign: | :heavy_check_mark:
Konuşma niyeti tanıma | Ayrı LUIS API'si çağrısı gerektirir | (SDK ile) tümleştirilmiş |  Konuşma hizmeti sayesinde bir LUIS anahtarı kullanabilirsiniz.
Basit niyeti tanıma | : heavy_minus_sign: | :heavy_check_mark:
Batch transkripsiyonu uzun ses dosyası | : heavy_minus_sign: | :heavy_check_mark:
Tanıma modu | Uç noktası URI'si aracılığıyla el ile | Otomatik | Tanıma modu konuşma hizmeti kullanılabilir değil.
Uç nokta konumu | Genel | Bölgesel | Bölgesel uç noktaları, gecikme süresini artırın.
REST API'leri | :heavy_check_mark: | :heavy_check_mark: | Konuşma Hizmetleri REST API'leri, Bing konuşma (farklı uç noktası) ile uyumludur. REST API'leri, metin okuma ve sınırlı konuşma metin işlevleri destekler.
WebSockets protokolleri | :heavy_check_mark: | :heavy_check_mark: | Konuşma Hizmetleri WebSockets API'si, Bing konuşma (farklı uç noktası) ile uyumludur. Mümkünse, konuşma SDK kodunuzu basitleştirerek geçirin.
Hizmetten hizmete API çağrıları | :heavy_check_mark: | : heavy_minus_sign: | Bing konuşma C# hizmet kitaplığı aracılığıyla sağlanmalıdır.
Açık kaynak SDK'sı | :heavy_check_mark: | : heavy_minus_sign: |

Konuşma Hizmetleri zamana dayalı bir fiyatlandırma modeli (işlem tabanlı bir modeli yerine) kullanın. Bkz: [konuşma Hizmetleri fiyatlandırması](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) Ayrıntılar için.

## <a name="migration-strategies"></a>Geçiş stratejileri

Siz veya Kurumunuz Bing konuşma API'si kullanan uygulamaları geliştirme veya üretim varsa, bunları konuşma Hizmetleri olabildiğince çabuk kullanacak şekilde güncelleştirmeniz gerekir. Bkz: [konuşma Hizmetleri belgeleri](index.yml) kullanılabilir SDK'lar, kod örnekleri ve öğreticiler.

Konuşma Hizmetleri [REST API'leri](rest-apis.md) Bing konuşma API'leri ile uyumludur. Bing konuşma REST API kullanıyorsanız, yalnızca REST uç noktasını değiştirmek ve bir konuşma Hizmetleri abonelik anahtarı için anahtar gerekir.

Konuşma Hizmetleri WebSockets protokolleri, Bing konuşma tarafından Kullanılanlar ile de uyumludur. Yeni geliştirme için WebSockets yerine Speech SDK'sı kullanmanızı öneririz. SDK için mevcut kodu geçirmek için iyi bir fikirdir. Ancak, olarak yalnızca bir değişiklik uç noktasını ve güncelleştirilmiş bir anahtarı WebSockets üzerinden Bing konuşma kullanan mevcut kodu REST API'lerle gerektirir.

Belirli bir programlama dili için Bing konuşma istemcisi kitaplığını kullanıyorsanız, geçiş [Speech SDK'sı](speech-sdk.md) API farklı olduğundan, uygulamanıza değişiklikler gerektirir. Speech SDK'sı, kodunuzu daha basit, yeni özelliklere erişim sağlarken de yapabilirsiniz.

Şu anda Speech SDK'sı destekler C# ([burada ayrıntıları](https://aka.ms/csspeech)), Java (cihazlar Android ve özel), Objective C (iOS) C++ (Windows ve Linux) ve JavaScript. Tüm platformlarda API'leri çoklu platform geliştirme hızlandırma benzerdir.

Konuşma hizmetleri genel bir uç noktası sunmamaktadır. Uygulamanızı verimli bir şekilde, bölgesel tek bir uç nokta tüm trafik için kullandığında işlevleri, belirleyin. Aksi durumda, coğrafi konum en verimli uç nokta belirlemek için kullanın. Kullandığınız her bölgede ayrı bir konuşma Hizmetleri aboneliği gerekir.

Uygulamanızı uzun süreli bağlantılar kullanır ve kullanılabilir bir SDK'ları kullanamazsınız, WebSockets bağlantı kullanabilirsiniz. 10 dakikalık zaman aşımı sınırı, uygun zamanlarda yeniden bağlanmayı tarafından yönetin.

Konuşma SDK'sını kullanmaya başlamak için:

1. İndirme [konuşma SDK](speech-sdk.md).
1. Konuşma Hizmetleri aracılığıyla iş [hızlı başlangıç kılavuzları](quickstart-csharp-dotnet-windows.md) ve [öğreticiler](how-to-recognize-intents-from-speech-csharp.md). Ayrıca bakmak [kod örnekleri](samples.md) deneyimi yeni API'ler ile almak için.
1. Konuşma hizmetleri kullanmak üzere uygulamanızı güncelleştirin.

## <a name="support"></a>Destek

Bing konuşma Müşteriler müşteri desteğine başvurun açarak bir [destek bileti](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). Destek gereksinimi gerektiriyorsa de bize başvurabilirsiniz bir [teknik destek planınız](https://azure.microsoft.com/support/plans/).

Konuşma hizmeti, SDK ve API desteği için konuşma hizmetleri ziyaret [destek sayfasını](support.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma Hizmetleri ücretsiz olarak deneyin](get-started.md)
* [Hızlı Başlangıç: Bir UWP uygulamasında Speech SDK'sı kullanarak konuşma tanıma](quickstart-csharp-uwp.md)

## <a name="see-also"></a>Ayrıca bkz.
* [Konuşma Hizmetleri sürüm notları](releasenotes.md)
* [Konuşma hizmeti nedir](overview.md)
* [Konuşma Hizmetleri ve Speech SDK'sı belgeleri](speech-sdk.md#get-the-sdk)
