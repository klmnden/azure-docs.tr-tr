---
title: İfade listeleri - konuşma Hizmetleri
titlesuffix: Azure Cognitive Services
description: Bir ifade listesini kullanarak konuşma hizmetleri sağlamak öğrenin `PhraseListGrammar` konuşma metin tanıma Sonuçları iyileştirmek için nesne.
services: cognitive-services
author: rhurey
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 5/02/2019
ms.author: rhurey
ms.openlocfilehash: 576d3c4a70c8870a31bc352b9f7723d2c2e69854
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025812"
---
# <a name="phrase-lists-for-speech-to-text"></a>Konuşma metin için ifade listeleri

Bir ifade listesi konuşma Hizmetleri sağlayarak, konuşma tanıma doğruluğunu geliştirebilir. İfade listeleri, bir kişinin adını veya belirli bir konuma gibi ses verilerinde bilinen ifadeleri tanımlamak için kullanılır.

Örneğin, bir komut varsa, "Taşı" ve "konuşulan, Atla" olası hedefi "Taşımak için Atla" bir giriş ekleyebilirsiniz. Bir ifade ekleme olasılığı artar, ne zaman ses tanınır "Taşıma doğru" yerine "Taşımak için Atla" tanınır.

Tek sözcükleri veya tümcecikleri tam tümcecik listesine eklenebilir. Ses tam bir eşleşme içeriyorsa bir deyim listesindeki bir girdinin tanıma sırasında kullanılır. Önceki örnekten, ifade listesi "Taşımak için Atla" ve yakalanan deyimi içeriyorsa oluşturma "doğru yavaş taşıma" adıdır, ardından "Yavaş taşımak için Atla" tanıma sonuç olacaktır.

## <a name="how-to-use-phrase-lists"></a>İfade listeleri kullanma

Aşağıdaki örnekler kullanarak bir ifade listesini oluşturmak nasıl çalışılacağını `PhraseListGrammar` nesne.

```C++
auto phraselist = PhraseListGrammar::FromRecognizer(recognizer);
phraselist->AddPhrase("Move to Ward");
phraselist->AddPhrase("Move to Bill");
phraselist->AddPhrase("Move to Ted");
```

```cs
PhraseListGrammar phraseList = PhraseListGrammar.FromRecognizer(recognizer);
phraseList.AddPhrase("Move to Ward");
phraseList.AddPhrase("Move to Bill");
phraseList.AddPhrase("Move to Ted");
```

```Python
phrase_list_grammar = speechsdk.PhraseListGrammar.from_recognizer(reco)
phrase_list_grammar.addPhrase("Move to Ward")
phrase_list_grammar.addPhrase("Move to Bill")
phrase_list_grammar.addPhrase("Move to Ted")
```

```JavaScript
var phraseListGrammar = SpeechSDK.PhraseListGrammar.fromRecognizer(reco);
phraseListGrammar.addPhrase("Move to Ward");
phraseListGrammar.addPhrase("Move to Bill");
phraseListGrammar.addPhrase("Move to Ted");
```

```Java
PhraseListGrammar phraseListGrammar = PhraseListGrammar.fromRecognizer(recognizer);
phraseListGrammar.addPhrase("Move to Ward");
phraseListGrammar.addPhrase("Move to Bill");
phraseListGrammar.addPhrase("Move to Ted");
```

>[!Note]
> Konuşma hizmeti konuşma eşleştirmek için kullanacağı tümcecik listeler maksimum sayısı 1024'tür.

İlişkilendirilmiş ifadeler de temizleyebilirsiniz `PhraseListGrammar` çağıran clear() tarafından.

```C++
phraselist->Clear();
```

```cs
phraseList.Clear();
```

```Python
phrase_list_grammar.clear()
```

```JavaScript
phraseListGrammar.clear();
```

```Java
phraseListGrammar.clear();
```

> [!NOTE]
> Değişikliklerini bir `PhraseListGrammar` Al etkileyen sonraki tanıma ya da bir konuşma hizmetleri yeniden bağlanma aşağıdaki nesne.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma SDK başvuru belgeleri](speech-sdk.md)
