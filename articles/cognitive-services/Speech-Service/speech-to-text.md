---
title: Konuşmayı metne dönüştürme
description: Konuşmayı metne dönüştürme API'si özelliklerine genel bakış.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 7cb0257a7302221f80bb90c0a6c3446cde07290a
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434135"
---
# <a name="about-the-speech-to-text-api"></a>Konuşmayı metne dönüştürme API'si

**Konuşmayı metne dönüştürme** API *dönüştürür* uygulamanızı görüntülemek için kullanıcı veya olarak alacak bir metne ses akışları giriş komutu. API'ler bir SDK'sı istemci kitaplığı (için desteklenen platformlar ve diller için) veya bir REST API ile kullanılabilir.

**Konuşmayı metne dönüştürme** API'si aşağıdaki özellikleri sunar:

- Konuşma tanıma teknolojisini Microsoft Gelişmiş — Cortana, Office ve diğer Microsoft ürünleri tarafından kullanılan aynı.

- Gerçek zamanlı sürekli tanıma. **Konuşmayı metne dönüştürme** kullanıcıların gerçek zamanlı olarak metne ses özelliği sağlar. Şu ana kadar tanınan bir kelimelerin Ara sonuçlar alma destekler. Hizmet, konuşma sonu otomatik olarak tanır. Kullanıcılar ayrıca büyük/küçük harf ve noktalama işaretleri, maskeleme küfür ve ters metin normalleştirme dahil olmak üzere ek biçimlendirme seçenekleri tercih edebilirsiniz.

- En iyi duruma getirilmiş **Konuşmayı metne dönüştürme** etkileşimli, konuşma, sonuçları ve dikte senaryoları. Tanınan sonuçları Lexical hem de görüntüleme formlarında döndürülür (sözcük sonuçlar için DetailedSpeechRecognitionResult örnekler ya da API bakın).

- Pek çok konuşulan dil ve diyalektler desteği. Her tanıma modunda desteklenen dillerin tam listesi için bkz. [desteklenen diller](language-support.md#speech-to-text).

- Ortamı ve konuşma yolu gibi kullanıcılarınızın özel etki alanı sözlük, uygulamanıza uyarlayabilirsiniz. Bu nedenle dil ve akustik modeller, özelleştirilmiş.

- Doğal dil anlama. İle tümleştirme yoluyla [Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS), türetilebilir amaç ve varlıkları konuşma içeriğinden. Kullanıcılar, uygulamanızın sözlük bilmeniz gerekmez, ancak kendi kelimelerinizle istediğini tanımlayabilirsiniz.

## <a name="api-capabilities"></a>API özellikleri

Çok sayıda yeteneklerini **Konuşmayı metne dönüştürme** - özellikle çevresinde özelleştirmesi - API REST kullanılabilir. Aşağıdaki tabloda, her API erişimi yöntemi yeteneklerini özetler. Özellikler ve API tam listesini consult Lütfen ayrıntıları [Swagger](https://swagger/service/11ed9226-335e-4d08-a623-4547014ba2cc#/)

| Kullanım örneği | REST | SDK’lar |
|-----|-----|-----|----|
| Bir komut (uzunluğu < 15 s); gibi kısa bir utterance özelliği geçici bir sonuç yok | Evet | Evet |
| Konuşmaların daha uzun bir utterance (> 15 sn) | Hayır | Evet |
| İsteğe bağlı Ara sonuçlarla Akış ses özelliği | Hayır | Evet |
| Konuşmacı ıntents aracılığıyla LUIS anlama | Yok\* | Evet |
| Doğruluk testi oluşturma | Evet | Hayır |
| Model uyarlama için veri kümelerini karşıya yükleme | Evet | Hayır |
| Konuşma modelleri Yönet & Oluştur | Evet | Hayır |
| Model dağıtımları yönetmek & Oluştur | Evet | Hayır |
| Abonelikleri Yönetme | Evet | Hayır |
| Model dağıtımları yönetmek & Oluştur | Evet | Hayır |
| Model dağıtımları yönetmek & Oluştur | Evet | Hayır |

> [!NOTE]
> REST API, API isteklerinin 5 saniye başına 25 sınırlayan azaltma uygular. İleti hearders sınırlarını size bildirir

\* *LUIS amaç ve varlıkları kullanarak ayrı bir LUIS aboneliği türetilebilir. Bu abonelikle SDK LUIS çağırmanızı ve konuşma döküm yanı sıra, varlık ve amacını sonuçlar sağlar. REST API ile LUIS çağırabilirsiniz kendiniz amaç ve varlıkları LUIS aboneliğinizle türetmek için.*

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [Hızlı Başlangıç: C# ' de Konuşma tanıma](quickstart-csharp-dotnet-windows.md)
* [Amaçlardan tutun C# ' de Konuşma tanıma öğrenin](how-to-recognize-intents-from-speech-csharp.md)
