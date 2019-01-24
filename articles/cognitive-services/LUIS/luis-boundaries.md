---
title: Sınırlar
titleSuffix: Language Understanding - Azure Cognitive Services
description: Bu makale, Azure Bilişsel hizmetler Language Understanding (LUIS) bilinen sınırları içerir. LUIS, birden fazla sınır alanlara sahip değildir. Model sınır amacı, varlıkları ve LUIS özellikleri denetler. Kota sınırları, anahtar türüne göre. Klavye birleşimi LUIS Web sitesini denetler.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 01/22/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 7f8f4848b7181ad3df7ad4fa009ff284de381b75
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54820419"
---
# <a name="boundaries-for-your-luis-model-and-keys"></a>LUIS modeline ve anahtarlar için sınırlar
LUIS, birden fazla sınır alanlara sahip değildir. İlk [modeli sınır](#model-boundaries), amacı, varlıkları ve LUIS özellikleri denetler. İkinci alanı [kota sınırları](#key-limits) anahtar türüne göre. Üçüncü bir sınırları alanıdır [klavye birleşimi](#keyboard-controls) LUIS Web sitesi denetleme. Dördüncü alan [dünya bölge eşleme](luis-reference-regions.md) LUIS ile Web sitesi geliştirme LUIS arasındaki [uç nokta](luis-glossary.md#endpoint) API'leri. 


## <a name="model-boundaries"></a>Model sınırları


|Alan|Sınır|
|--|:--|--|
| [Uygulama adı][luis-get-started-create-app] | * Max varsayılan karakter |
| [Toplu test etme][batch-testing]| 10 veri kümeleri, veri kümesi başına 1000 konuşma|
| Açık listesi | uygulama başına 50|
| [Hedefleri][intents]|uygulama başına 500<br>[Gönderim tabanlı](https://aka.ms/dispatch-tool) uygulamanın karşılık gelen 500 gönderme kaynakları var.|
| [Varlıklar listesi](./luis-concept-entity-types.md) | Üst öğe: 50, alt: 20.000 öğeleri. Kurallı ad * varsayılan karakter maks. Eş anlamlı değerleri herhangi bir uzunluk sınırlaması vardır. |
| [Makine öğrenilen varlıklar](./luis-concept-entity-types.md):<br> Bileşik<br>  Hiyerarşik<br> Basit|100 <br>Makine öğrenilen varlıklar (Basit, hiyerarşik ve bileşik varlıklar) toplam sayısı 100 aşamaz. Bileşik ve hiyerarşik varlıkları 10'dan fazla alt öğeleri olamaz.  |
| [Desenleri](luis-concept-patterns.md)|uygulama başına 500 desenleri.<br>Deseni en fazla uzunluğu 400 karakter olabilir.<br>Deseni başına 3 Pattern.any varlıklar<br>En fazla 2 iç içe geçmiş isteğe bağlı metni deseninde|
| [Pattern.Any](./luis-concept-entity-types.md)|uygulama başına 100 deseni başına 3 pattern.any varlıklar |
| [İfade listesi][phrase-list]|10 tümcecik listeler, liste başına 5.000 öğeleri|
| [Önceden oluşturulmuş varlıklar](./luis-prebuilt-entities.md) | bir sınır yoktur|
| [Normal ifade varlıkları](./luis-concept-entity-types.md)|20 varlıklar<br>Maksimum 500 karakter. Normal ifade varlık deseni|
| [Roller](luis-concept-roles.md)|uygulama başına 300 roller. Varlık başına 10 rolü|
| [Utterance][utterances] | 500 karakter|
| [Konuşma][utterances] | uygulama başına 15.000|
| [Sürümleri](luis-concept-version.md)| bir sınır yoktur |
| [Sürüm adı][luis-how-to-manage-versions] | alfasayısal ve süre sınırlı 10 karakter (.) |

* Varsayılan karakter en fazla 50 karakterdir. 

## <a name="intent-and-entity-naming"></a>Amacı ve varlık adlandırma
Amacı ve varlık adları şu karakterleri kullanmayın:

|Karakter|Ad|
|--|--|
|`{`|Sol küme ayracı|
|`}`|Sağa süslü ayraç|
|`[`|Sol köşeli ayraç|
|`]`|Sağ köşeli ayraç|
|`\`|Ters eğik çizgi|

## <a name="key-usage"></a>Anahtar kullanımı

Dil anlama, ayrı anahtarları, yazma için bir tür ve tahmin uç noktası'nı sorgulamak için bir türü vardır. Anahtar türleri arasındaki farklar hakkında daha fazla bilgi için bkz. [LUIS yazma ve sorgu tahmin uç nokta anahtarlarını](luis-concept-keys.md).

## <a name="key-limits"></a>Anahtar sınırları

Yazma anahtar yazma ve uç noktası için farklı sınırlara sahiptir. LUIS hizmet uç noktası anahtarı yalnızca uç nokta sorgular için geçerlidir.


|Anahtar|Yazma|Uç Nokta|Amaç|
|--|--|--|--|
|Yazma dil anlama/başlangıç|1 milyon/ay, 5/saniye|1 bin/ay, 5/saniye|LUIS uygulamanızı yazma|
|Language Understanding'i [abonelik] [ pricing] - F0 - ücretsiz katmanı |geçersiz|10 bin/ay, 5/saniye|LUIS uç noktanızı sorgulama|
|Language Understanding'i [abonelik] [ pricing] - S0 - temel katman|geçersiz|50/saniye|LUIS uç noktanızı sorgulama|
|Bilişsel hizmet [abonelik] [ pricing] - S0 - standart katman|geçersiz|50/saniye|LUIS uç noktanızı sorgulama|
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
