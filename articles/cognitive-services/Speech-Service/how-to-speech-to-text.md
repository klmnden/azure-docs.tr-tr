---
title: Konuşmayı metne dönüştürme kullanın
description: Konuşmayı metne dönüştürme konuşma hizmeti kullanmayı öğrenin
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: v-jerkin
ms.openlocfilehash: 5e7916660275ab45c4556f1169f3e68fe1d3cb85
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39282305"
---
# <a name="use-speech-to-text-in-the-speech-service"></a>Konuşma hizmeti "için konuşma metin" kullanın

Kullanabileceğiniz **Konuşmayı metne dönüştürme** uygulamalarınızdaki iki farklı yolla.

| Yöntem | Açıklama |
|-|-|
| [SDK](speech-sdk.md) | C/C++, C# ve Java geliştiricileri için en basit yöntemi |
| [REST](rest-apis.md) | Bir HTTP POST isteği kullanarak kısa konuşma tanıma | 

## <a name="using-the-sdk"></a>SDK’yı kullanarak

[Speech SDK'sı](speech-sdk.md) kullanmanın en basit yolu sağlar **Konuşmayı metne dönüştürme** uygulamanızda tam işlevselliğe sahip.

1. Bir konuşma hizmeti abonelik anahtarı sağlayan bir konuşma fabrikası oluşturun ve [bölge](regions.md) veya bir yetkilendirme belirteci. Bu noktada tanıma dili veya kendi konuşma tanıma modelleri için özel bir uç nokta gibi seçeneklere de yapılandırabilirsiniz.

2. Bir tanıyıcı fabrikasından alın. Üç farklı türde tanıyıcıları kullanılabilir. Her tanıyıcı türü, bir dosyadan cihazınızın varsayılan mikrofon, bir ses akışı veya ses kullanabilirsiniz.

    Tanıyıcı | İşlev
    -|-
    Konuşma tanıyıcının|Konuşma transkripsiyonu metin sağlar
    Intent tanıyıcı|İle Konuşmacı amacı türetilen [LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/) tanıma sonra\*
    Çeviri tanıyıcı|Transcribed metni başka bir dile çevirir (bkz [konuşma çevirisi](how-to-translate-speech.md))

    \* *Amaç tanıma için ayrı bir LUIS abonelik anahtarı bir konuşma fabrikası için hedefi tanıyıcı oluştururken kullanmanız gerekir.*
    
4. Olayları zaman uyumsuz işlem için'kurmak isterseniz birbirine bağlayın. Geçici ve Nihai sonuç olduğunda tanıyıcı daha sonra olay işleyicileri çağırır. Aksi takdirde, uygulamanızı son transkripsiyonu sonuç alırsınız.

5. Tanıma başlatın.
   Tek tanıma, tanıma, komut veya sorgu gibi kullanılmaya `RecognizeAsync()`, tanınan ilk utterance döndürür.
   Uzun süreli tanıma, tanıma gibi kullanılmaya `StartContinuousRecognitionAsync()` ve zaman uyumsuz tanıma sonuçları için olayları oluşturan bağlayın.

### <a name="sdk-samples"></a>SDK örnekleri

En son örnekleri için bkz [Bilişsel hizmetler konuşma SDK örnek GitHub deposunda](https://aka.ms/csspeech/samples).

## <a name="using-the-rest-api"></a>REST API kullanarak

REST API, SDK tarafından desteklenen bir dil kullanıyorsanız, konuşma tanıma en basit yoludur. Hizmet uç noktası için bir HTTP POST isteği isteğin gövdesindeki tüm utterance geçirme olun. Tanınan metni içeren bir yanıt alırsınız.

> [!NOTE]
> REST API kullanırken, konuşma 15 saniye veya daha az sınırlı olmalıdır.

Daha fazla bilgi için **Konuşmayı metne dönüştürme** REST API için bkz: [REST API'leri](rest-apis.md#speech-to-text). Nasıl çalıştığını görmek için indirme [REST API örnekleri](https://github.com/Azure-Samples/SpeechToText-REST) github'dan.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
- [C++'ta konuşma tanıma nasıl](quickstart-cpp-windows.md)
- [C# dilinde konuşmayı algılama](quickstart-csharp-dotnet-windows.md)
- [Nasıl Java konuşma tanıma](quickstart-java-android.md)
