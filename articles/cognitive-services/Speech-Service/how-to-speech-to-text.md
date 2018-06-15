---
title: Konuşarak metin | Microsoft Docs
description: Konuşma metin okuma hizmetinde kullanmayı öğrenin
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 878a31992415b1f8688afcfb186fcd94ce2567b4
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "35356229"
---
# <a name="use-speech-to-text-in-the-speech-service"></a>"Metne konuşma" konuşma Hizmeti'yle

Kullanabileceğiniz **metin konuşma** iki farklı yolla uygulamalarınızda.

| Yöntem | Açıklama |
|-|-|
| [SDK](speech-sdk.md) | C/C++, C# ve Java * geliştiriciler için en basit yöntemi |
| [REST](rest-apis.md) | Bir HTTP POST isteği kullanarak kısa utterances tanı | 

\* *Java SDK'sı parçası olan [konuşma aygıtları SDK](speech-devices-sdk.md).*

## <a name="using-the-sdk"></a>SDK’yı kullanarak

[Konuşma SDK](speech-sdk.md) kullanmanın en basit yolu sağlar **metin konuşma** uygulamanızda tam işlevsellikle.

1. Bir konuşma hizmeti abonelik anahtar veya bir yetki belirteci sağlayan bir konuşma fabrikası oluşturun. Bu noktada tanıma dili veya kendi konuşma tanıma modelleri için özel bir uç noktası gibi seçenekler de yapılandırabilirsiniz.

2. Bir tanıyıcı fabrikası'ndan alın. Üç farklı türde tanıyıcıları kullanılabilir. Her tanıyıcı türü, bir dosyadan aygıtınızın varsayılan mikrofon, ses akışı ya da ses kullanabilirsiniz.

    Tanıyıcı | İşlev
    -|-
    Konuşma tanıyıcı|Konuşma metin transcription sağlar
    Hedefi tanıyıcı|Konuşmacı hedefi aracılığıyla türetilen [HALUK](https://docs.microsoft.com/azure/cognitive-services/luis/) tanıma sonra\*
    Çeviri tanıyıcı|Başka bir dilde transcribed metne çevirir (bkz [konuşma çeviri](how-to-translate-speech.md))

    \* *Hedefi tanıma için ayrı bir HALUK abonelik anahtarı bir konuşma fabrikası için hedefi tanıyıcı oluştururken kullanmanız gerekir.*
    
4. Zaman uyumsuz işlem olayları isterseniz bağlayın. Ara ve nihai sonuçları olduğunda tanıyıcı daha sonra olay işleyicileri çağırır. Aksi durumda, uygulamanızın son transcription sonucu alırsınız.

5. Tanıma başlatın.

### <a name="sdk-samples"></a>SDK örnekleri

En son örnekleri için bkz [Bilişsel hizmetler konuşma SDK örnek GitHub deposunu](https://aka.ms/csspeech/samples).

## <a name="using-the-rest-api"></a>REST API kullanarak

REST API, SDK'sı tarafından desteklenen bir dil kullanmıyorsanız konuşma tanımak için basit bir yoldur. İstek gövdesinde tüm utterance geçirme Hizmeti uç noktası için bir HTTP POST isteği olun. Tanınan metni içeren bir yanıtı alırsınız.

> [!NOTE]
> Utterances REST API kullanırken 15 saniye veya daha az sınırlıdır.


Daha fazla bilgi için **metin konuşma** REST API için bkz: [REST API'leri](rest-apis.md#speech-to-text). Eylem görmek için indirme [REST API örnekleri](https://github.com/Azure-Samples/SpeechToText-REST) github'dan.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma deneme aboneliğinizi Al](https://azure.microsoft.com/try/cognitive-services/)
- [C# Konuşma tanımasını nasıl](quickstart-csharp-windows.md)
