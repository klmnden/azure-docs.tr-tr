---
title: Konuşma hizmeti için Bing konuşma içeriğinden geçirme
titleSuffix: Azure Cognitive Services
description: Bing konuşma tanıma ve konuşma hizmeti arasındaki farklar Geliştirici açısından öğrenin ve konuşma hizmeti kullanmak için uygulamanızı geçişi.
services: cognitive-services
author: wsturman
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: gracez
ms.openlocfilehash: baf9b9cd9b3f57c1d708dd404d59c036df6c169f
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49466656"
---
# <a name="migrate-from-bing-speech-to-the-speech-service"></a>Konuşma hizmeti için Bing konuşma içeriğinden geçirme

Uygulamalarınızı konuşma hizmeti için Bing konuşma tanıma API'SİNDEN geçirmek için bu makaleyi kullanın.

Bu makalede, Bing konuşma API'leri ve konuşma hizmeti arasındaki farkları özetler ve uygulamalarınızı geçiş stratejileri önerir. Bing konuşma API'si abonelik anahtarınızı konuşma hizmeti tarafından kabul edilmez; Yeni bir konuşma tanıma hizmeti abonelik gerekir.

Tek bir Konuşma Tanıma Hizmeti abonelik anahtarıyla aşağıdaki özelliklerin tümüne erişebilirsiniz. Hepsinin kullanımı bağımsız bir şekilde ölçüldüğünden yalnızca kullandığınız hizmetler için ödeme yapmış olursunuz.

* [Konuşmayı metne dönüştürme](speech-to-text.md)
* [Özel konuşmayı metne dönüştürme](https://cris.ai)
* [Metin okuma](text-to-speech.md)
* [Özel metin okuma sesleri](how-to-customize-voice-font.md)
* [Konuşma çevirisi ](speech-translation.md) ([Metin çevirisi](../translator/translator-info-overview.md) özelliğini kapsamaz)

[Speech SDK'sı](speech-sdk.md) Bing konuşma istemci kitaplıkları için bir işlev yerini almaktadır, ancak farklı bir API kullanır.

## <a name="comparison-of-features"></a>Özelliklerin karşılaştırması

Konuşma hizmeti büyük ölçüde özelliği, platform ve aşağıdaki farklarla birlikte Bing Konuşma ile programlama dili eşlik altındadır.

Özellik | Bing Konuşma | Konuşma Hizmeti | Ayrıntılar
-|-|-|-
C++ SDK'SI | : heavy_minus_sign: | : heavy_check_mark: | Konuşma hizmeti, Windows ve Linux destekler.
Java SDK | : heavy_check_mark: | : heavy_check_mark: | Konuşma hizmeti, Android ve konuşma cihazları destekler
C# SDK’sı | : heavy_check_mark: | : heavy_check_mark: | Konuşma hizmeti, Windows 10, UWP ve .NET Standard 2.0 destekler.
Sürekli konuşma tanıma | 10 dakika | Sınırsız (SDK ile) | Bing konuşma hem konuşma hizmeti WebSockets protokolleri, çağrı başına 10 dakikaya kadar destekler. Ancak, Speech SDK'sı otomatik olarak zaman aşımından üzerinde yeniden veya kesin.
Kısmi veya Ara sonuçlar | : heavy_check_mark: | : heavy_check_mark: | WebSockets protokolü veya SDK'sı ile
Özel konuşma modelleri | : heavy_check_mark: | : heavy_check_mark: | Bing konuşma ayrı bir özel konuşma aboneliği gerektirir
Özel ses tipi | : heavy_check_mark: | : heavy_check_mark: | Bing konuşma ayrı bir özel sesli aboneliği gerektirir
24 kHz sesleri | : heavy_minus_sign: | : heavy_check_mark: 
Konuşma niyeti tanıma | Ayrı LUIS API'si çağrısı gerektirir | (SDK ile) tümleştirilmiş |  Konuşma hizmeti sayesinde bir LUIS anahtar kullanılabilir.
Basit niyeti tanıma | : heavy_minus_sign: | : heavy_check_mark: 
Batch transkripsiyonu uzun ses dosyası | : heavy_minus_sign: | : heavy_check_mark:
Tanıma modu | Uç noktası URI'si aracılığıyla el ile | Automatic | Tanıma modu konuşma hizmeti kullanılamıyor
Uç nokta konumu | Genel | Bölgesel | Bölgesel uç noktaları, gecikme süresini artırın. Konuşma hizmeti için göz önünde bulundurarak bir genel uç noktası altındadır.
REST API'leri | : heavy_check_mark: | : heavy_check_mark: | Konuşma hizmeti REST API'si, Bing konuşma (farklı uç noktası) ile uyumludur. REST API'leri, metin okuma ve sınırlı konuşma metin işlevleri destekler.
WebSockets protokolleri | : heavy_check_mark: | : heavy_check_mark: | Konuşma hizmeti WebSockets API'si, Bing konuşma (farklı uç noktası) ile uyumludur. Konuşma, kodu basitleştirmek için SDK mümkünse geçirin.
Hizmetten hizmete API çağrıları | : heavy_check_mark: | : heavy_minus_sign: | Bing konuşma C# hizmet kitaplığı aracılığıyla sağlanmalıdır. 
Açık kaynak SDK'sı | : heavy_check_mark: | : heavy_minus_sign: |

Konuşma hizmeti, zamana dayalı bir fiyatlandırma modeli (işlem tabanlı bir modeli yerine) kullanır. Ayrıntılar için [Konuşma Tanıma Hizmeti fiyatlandırmasını](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) inceleyin.

## <a name="migration-strategies"></a>Geçiş stratejileri

Siz veya Kurumunuz Bing konuşma API'si kullanan uygulamaları geliştirme veya üretim varsa, bunları konuşma hizmeti olabildiğince çabuk kullanacak şekilde güncelleştirmeniz gerekir. Bkz: [konuşma hizmeti belgeleri](index.yml) kullanılabilir SDK'lar, kod örnekleri ve öğreticiler.

Konuşma hizmeti [REST API'leri](rest-apis.md) Bing konuşma API'leri ile uyumludur. Bing konuşma REST API kullanıyorsanız, yalnızca REST uç noktasını değiştirmek ve bir konuşma tanıma hizmeti abonelik anahtarı için anahtar gerekir.

Konuşma hizmeti WebSockets protokolleri, Bing konuşma tarafından Kullanılanlar ile de uyumludur. Yeni geliştirme hedef WebSockets kullanmak yerine konuşma hizmeti SDK'sı ve SDK için mevcut kodu geçirmenizi öneriyoruz öneririz. Ancak, olarak yalnızca bir değişiklik uç noktasını ve güncelleştirilmiş bir anahtarı WebSockets üzerinden Bing konuşma kullanan mevcut kodu REST API'lerle gerektirir.

Belirli bir programlama dili için Bing konuşma istemcisi kitaplığını kullanıyorsanız, geçiş [Speech SDK'sı](speech-sdk.md) API farklı olduğundan, uygulamanıza değişiklik yapılmasını gerektirir. Speech SDK'sı, kodunuzun daha basit yeni özelliklere erişim sağlarken de yapabilirsiniz.

Şu anda C# (Windows 10, UWP, .NET Standard), Java (cihazlar Android ve özel), Objective C (iOS), (Windows ve Linux) C++ ve JavaScript Speech SDK'sı destekler. Tüm platformlarda API'leri çoklu platform geliştirme hızlandırma benzerdir.

Konuşma hizmeti genel bir uç noktası şu anda sunmaz. Uygulamanızı verimli bir şekilde tüm trafik için tek bir bölgesel uç nokta kullanarak çalışır belirlemeniz gerekir. Aksi durumda, coğrafi konum en verimli uç nokta belirlemek için kullanın. Kullandığınız her bölgede ayrı bir konuşma tanıma hizmeti aboneliği gerekir.

Uygulamanızı uzun süreli bağlantılar kullanır ve kullanılabilir bir SDK'ları kullanamazsınız, WebsSockets bağlantı ve uygun zamanlarda yeniden bağlanmayı tarafından 10 dakikalık zaman aşımı sınırı yönetme kullanabilirsiniz.

Konuşma SDK'sını kullanmaya başlamak için:

1. İndirme [konuşma SDK](speech-sdk.md).
1. Konuşma hizmeti üzerinden iş [hızlı başlangıç kılavuzları](quickstart-csharp-dotnet-windows.md), [öğreticiler](how-to-recognize-intents-from-speech-csharp.md)bakın [kod örnekleri](samples.md) deneyimi yeni API'ler ile almak için.
1. Konuşma hizmeti ve API'leri kullanmak için uygulamanızı güncelleştirin.

## <a name="support"></a>Destek

Bing konuşma Müşteriler müşteri desteğine başvurun açarak bir [destek bileti](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). Destek gereksinimi gerektiriyorsa de bize başvurabilirsiniz bir [teknik destek planı](https://azure.microsoft.com/support/plans/).

Konuşma hizmeti, SDK ve API desteği için konuşma hizmeti ziyaret [destek sayfasını](support.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma hizmetini ücretsiz deneyin](get-started.md)
* [Hızlı Başlangıç: bir UWP uygulamasında Speech SDK'sı kullanarak konuşma tanıma](quickstart-csharp-uwp.md)

## <a name="see-also"></a>Ayrıca bkz.
* [Konuşma hizmeti sürüm notları](releasenotes.md)
* [Konuşma hizmeti nedir](overview.md)
* [Konuşma hizmeti ve SDK Belgeleri](speech-sdk.md#get-the-sdk)
