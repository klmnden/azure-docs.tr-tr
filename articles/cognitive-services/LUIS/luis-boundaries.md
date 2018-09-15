---
title: Sınırları ve sınırları için Language Understanding (LUIS)
titleSuffix: Azure Cognitive Services
description: Bu makale, Azure Bilişsel hizmetler Language Understanding (LUIS) bilinen sınırları içerir. LUIS, birden fazla sınır alanlara sahip değildir. Model sınır amacı, varlıkları ve LUIS özellikleri denetler. Kota sınırları, anahtar türüne göre. Klavye birleşimi LUIS Web sitesini denetler.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 215f8305c19f0b12a8b240abb16a30f0ce852502
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2018
ms.locfileid: "45629024"
---
# <a name="luis-boundaries"></a>LUIS sınırları
LUIS, birden fazla sınır alanlara sahip değildir. İlk [modeli sınır](#model-boundaries), amacı, varlıkları ve LUIS özellikleri denetler. İkinci alanı [kota sınırları](#key-limits) anahtar türüne göre. Üçüncü bir sınırları alanıdır [klavye birleşimi](#keyboard-controls) LUIS Web sitesi denetleme. Dördüncü alan [dünya bölge eşleme](luis-reference-regions.md) LUIS ile Web sitesi geliştirme LUIS arasındaki [uç nokta](luis-glossary.md#endpoint) API'leri. 


## <a name="model-boundaries"></a>Model sınırları

|Alan|Sınır|
|--|:--|--|
| [Uygulama adı][luis-get-started-create-app] | * Max varsayılan karakter |
| [Toplu test etme][batch-testing]| 10 veri kümeleri, veri kümesi başına 1000 konuşma|
| **[Bileşik](./luis-concept-entity-types.md)|100 ile en fazla 10 alt öğeleri |
| Açık listesi | uygulama başına 50|
| **[Hiyerarşik](./luis-concept-entity-types.md) |100 ile en fazla 10 alt öğeleri |
| [Hedefleri][intents]|uygulama başına 500<br>[Gönderim tabanlı](https://aka.ms/dispatch-tool) uygulamanın karşılık gelen 500 gönderme kaynakları var.|
| [Varlıklar listesi](./luis-concept-entity-types.md) | Üst: 50, alt: 20.000 öğeleri. Kurallı ad * varsayılan karakter maks. Eş anlamlı değerleri herhangi bir uzunluk sınırlaması vardır. |
| [Desenleri](luis-concept-patterns.md)|uygulama başına 500 desenleri.<br>Deseni en fazla uzunluğu 400 karakter olabilir.<br>Deseni başına 3 Pattern.any varlıklar<br>En fazla 2 iç içe geçmiş isteğe bağlı metni deseninde|
| [Pattern.Any](./luis-concept-entity-types.md)|uygulama başına 100 deseni başına 3 pattern.any varlıklar |
| [İfade listesi][phrase-list]|10 tümcecik listeler, liste başına 5.000 öğeleri|
| [Önceden oluşturulmuş varlıklar](./luis-prebuilt-entities.md) | bir sınır yoktur|
| [Normal ifade varlıkları](./luis-concept-entity-types.md)|20 varlıklar<br>Maksimum 500 karakter. Normal ifade varlık deseni|
| [Roller](luis-concept-roles.md)|uygulama başına 300 roller. Varlık başına 10 rolü|
| **[Basit](./luis-concept-entity-types.md)| 100 varlık|
| [Utterance][utterances] | 500 karakter|
| [Konuşma][utterances] | uygulama başına 15.000|
| [Sürümleri](luis-concept-version.md)| bir sınır yoktur |
| [Sürüm adı][luis-how-to-manage-versions] | alfasayısal ve süre sınırlı 10 karakter (.) |

* Varsayılan karakter en fazla 50 karakterdir. 

** Basit, hiyerarşik ve bileşik varlıkların toplam sayısı 100 aşamaz. Toplam sayısını, hiyerarşik varlıklar, bileşik varlıklar, basit varlıkları ve hiyerarşik alt varlıklar 330 aşamaz. 

## <a name="intent-and-entity-naming"></a>Amacı ve varlık adlandırma
Amacı ve varlık adları şu karakterleri kullanmayın:

|Karakter|Ad|
|--|--|
|`{`|Sol küme ayracı|
|`}`|Sağa süslü ayraç|
|`[`|Sol köşeli ayraç|
|`]`|Sağ köşeli ayraç|
|`\`|Ters eğik çizgi|

## <a name="key-limits"></a>Anahtar sınırları
Yazma anahtar yazma ve uç noktası için farklı sınırlara sahiptir. LUIS hizmet uç noktası anahtarı yalnızca uç nokta sorgular için geçerlidir.

|Anahtar|Yazma|Uç Nokta|Amaç|
|--|--|--|--|
|Yazma/başlangıç|1 milyon/ay, 5/saniye|1 bin/ay, 5/saniye|LUIS uygulamanızı yazma|
|[Abonelik] [ pricing] - F0 - ücretsiz katmanı |geçersiz|10 bin/ay, 5/saniye|LUIS uç noktanızı sorgulama|
|[Abonelik] [ pricing] - S0 - temel katman|geçersiz|50/saniye|LUIS uç noktanızı sorgulama|
|[Yaklaşım analizi tümleştirme](luis-how-to-publish-app.md#enable-sentiment-analysis)|geçersiz|Ücretsiz|Anahtar ifade veri ayıklama gibi yaklaşım bilgileri ekleme |
|Konuşma tümleştirme|geçersiz|5.50 ABD Doları/1 bin uç nokta istekleri|Konuşulan utterance dönüştürmek için metin utterance ve LUIS sonuçlar döndürebilir.|

## <a name="keyboard-controls"></a>Klavye denetimleri

|Klavye girdisi | Açıklama | 
|--|--|
|Denetim + E|belirteçler ve konuşma listesindeki varlıklar arasında geçiş yapar|

## <a name="website-sign-in-time-period"></a>Web sitesi oturum zaman aralığı

Oturum açma erişiminizi içindir **60 dakika**. Bu süre sonra bu hatayı alırsınız. Yeniden oturum açmanız gerekir.

[luis-get-started-create-app]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app
[batch-testing]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-test#batch-testing
[intents]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-intent
[phrase-list]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-feature
[utterances]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-utterance
[luis-how-to-manage-versions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-manage-versions
[pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
<!-- TBD: fix this link -->
[speech-to-intent-pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
