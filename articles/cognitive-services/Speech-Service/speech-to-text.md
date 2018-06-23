---
title: Metin konuşma hakkında | Microsoft Docs
description: Konuşma metin API özelliklerine genel bakış.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 2eee1c6f9158f128ed5ffe575f8f498f1d3eb5e9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355690"
---
# <a name="about-the-speech-to-text-api"></a>Konuşma metin API hakkında

**Metin konuşma** API *transcribes* , uygulamanızın görüntülenen kullanıcıya veya olarak görev metne ses akışları giriş komutu. API'ler, bir SDK istemci kitaplığı (için desteklenen platformlar ve diller) veya bir REST API kullanılabilir.

**Metin konuşma** API aşağıdaki özellikleri sunar:

- Konuşma tanıma teknolojisi Microsoft'tan Gelişmiş — aynı Cortana, Office ve diğer Microsoft ürünleri tarafından kullanılır.

- Gerçek zamanlı sürekli tanıma. **Metin konuşma** kullanıcıların gerçek zamanlı metne ses transcribe olanak sağlar. Ayrıca, o ana kadarki tanınan sözcüklerin Ara sonuçların alma destekler. Hizmet, konuşma sonu otomatik olarak tanır. Kullanıcılar, büyük/küçük harf ve noktalama işaretleri, maskeleme uygunsuz metin ve metin normalleştirme dahil olmak üzere ek biçimlendirme seçenekleri de seçebilirsiniz.

- En iyi duruma getirilmiş **metin konuşma** etkileşimli, konuşma için sonuçları ve yazdırma senaryoları. 

- Birden çok Portekizce birçok konuşulan dilde desteği. Desteklenen dillerin her tanıma modunda tam listesi için bkz: [desteklenen diller](supported-languages.md#speech-to-text).

- Özelleştirilmiş dil ve uygulamanızı konuşarak, ortam ve özel sözlük kullanıcılarınızın şekilde uyarlamak için akustik modeller.

- Doğal dil anlama. İle tümleştirme yoluyla [dil anlama](https://docs.microsoft.com/azure/cognitive-services/luis/) (HALUK), türetilen hedefleri ve varlıkları konuşma. Kullanıcılar, uygulamanızın sözlük bilmek zorunda değilsiniz ancak kendi sözcükleri istediklerini tanımlayabilirsiniz.

## <a name="api-capabilities"></a>API özellikleri

Bazı yetenekleri **metin konuşma** API REST kullanılabilir değildir. Aşağıdaki tabloda API'sine erişim her yöntem yeteneklerini özetler.

| Kullanım örneği | REST | SDK’lar |
|-----|-----|-----|----|
| Bir komutu (Uzunluk < 15 s); gibi kısa bir utterance transcribe Ara sonuç yok | Evet | Evet |
| Uzun utterance (> 15 s) transcribe | Hayır | Evet |
| İsteğe bağlı Ara sonuçlarla ses akışını transcribe | Hayır | Evet |
| Konuşmacı hedefleri HALUK aracılığıyla anlama | yok\* | Evet |

\* *HALUK hedefleri ve varlıkları ayrı bir HALUK abonelik kullanarak elde edilebilir. Bu abonelik, SDK HALUK sizin için çağırın ve konuşma transcriptions yanı sıra varlık ve amacı sonuçları sağlar. REST API'si ile HALUK çağırabilirsiniz kendinize hedefleri ve varlıkları HALUK aboneliğinizle türetilir.*

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi Al](https://azure.microsoft.com/try/cognitive-services/)
* [C# Konuşma tanıması bkz.](quickstart-csharp-windows.md)
