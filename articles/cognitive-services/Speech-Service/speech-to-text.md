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
ms.openlocfilehash: ba6710c8b5b8de1c63fa6778ea3853ab52365254
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39325345"
---
# <a name="about-the-speech-to-text-api"></a>Konuşmayı metne dönüştürme API'si

**Konuşmayı metne dönüştürme** API *dönüştürür* uygulamanızı görüntülemek için kullanıcı veya olarak alacak bir metne ses akışları giriş komutu. API'ler bir SDK'sı istemci kitaplığı (için desteklenen platformlar ve diller için) veya bir REST API ile kullanılabilir.

**Konuşmayı metne dönüştürme** API'si aşağıdaki özellikleri sunar:

- Konuşma tanıma teknolojisini Microsoft Gelişmiş — Cortana, Office ve diğer Microsoft ürünleri tarafından kullanılan aynı.

- Gerçek zamanlı sürekli tanıma. **Konuşmayı metne dönüştürme** kullanıcıların gerçek zamanlı olarak metne ses özelliği sağlar. Şu ana kadar tanınan bir kelimelerin Ara sonuçlar alma destekler. Hizmet, konuşma sonu otomatik olarak tanır. Kullanıcılar ayrıca büyük/küçük harf ve noktalama işaretleri, maskeleme küfür ve ters metin normalleştirme dahil olmak üzere ek biçimlendirme seçenekleri tercih edebilirsiniz.

- En iyi duruma getirilmiş **Konuşmayı metne dönüştürme** etkileşimli, konuşma, sonuçları ve dikte senaryoları. 

- Pek çok konuşulan dil ve diyalektler desteği. Her tanıma modunda desteklenen dillerin tam listesi için bkz. [desteklenen diller](supported-languages.md#speech-to-text).

- Ortamı ve konuşma yolu gibi kullanıcılarınızın özel etki alanı sözlük, uygulamanıza uyarlayabilirsiniz. Bu nedenle dil ve akustik modeller, özelleştirilmiş.

- Doğal dil anlama. İle tümleştirme yoluyla [Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS), türetilebilir amaç ve varlıkları konuşma içeriğinden. Kullanıcılar, uygulamanızın sözlük bilmeniz gerekmez, ancak kendi kelimelerinizle istediğini tanımlayabilirsiniz.

## <a name="api-capabilities"></a>API özellikleri

Bazı özelliklerini **Konuşmayı metne dönüştürme** API'si, REST kullanılabilir değildir. Aşağıdaki tabloda, her API erişimi yöntemi yeteneklerini özetler.

| Kullanım örneği | REST | SDK’lar |
|-----|-----|-----|----|
| Bir komut (uzunluğu < 15 s); gibi kısa bir utterance özelliği geçici bir sonuç yok | Evet | Evet |
| Konuşmaların daha uzun bir utterance (> 15 sn) | Hayır | Evet |
| İsteğe bağlı Ara sonuçlarla Akış ses özelliği | Hayır | Evet |
| Konuşmacı ıntents aracılığıyla LUIS anlama | Yok\* | Evet |

\* *LUIS amaç ve varlıkları kullanarak ayrı bir LUIS aboneliği türetilebilir. Bu abonelikle SDK LUIS çağırmanızı ve konuşma döküm yanı sıra, varlık ve amacını sonuçlar sağlar. REST API ile LUIS çağırabilirsiniz kendiniz amaç ve varlıkları LUIS aboneliğinizle türetmek için.*

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [Hızlı Başlangıç: C# ' de Konuşma tanıma](quickstart-csharp-dotnet-windows.md)
* [Amaçlardan tutun C# ' de Konuşma tanıma öğrenin](how-to-recognize-intents-from-speech-csharp.md)
