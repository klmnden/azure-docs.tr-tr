---
title: Dil anlama (HALUK) sınırları | Microsoft Docs
titleSuffix: Azure
description: Bu makale HALUK bilinen sınırlamaları içerir.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 52bda6a13422ce8f759c40bd454a6b15e92d7a5d
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110305"
---
# <a name="luis-boundaries"></a>HALUK sınırları
HALUK birkaç sınır alanlar vardır. İlk [modeli sınır](#model-boundaries), amacı, varlıkları ve HALUK özelliklerinde denetler. İkinci alanı [kota sınırları](#key-limits) anahtar türüne göre. Sınırları üçüncü bir alandır [klavye birleşimi](#keyboard-controls) HALUK Web sitesi denetleme. Dördüncü bir alandır [world bölge eşleme](luis-reference-regions.md) Web sitesi geliştirme HALUK HALUK arasındaki [endpoint](luis-glossary.md#endpoint) API'leri. 


## <a name="model-boundaries"></a>Model sınırları

|Alan|Sınır|
|--|:--|--|
| [Uygulama adı][luis-get-started-create-app] | * Max varsayılan karakter |
| [Toplu test etme][batch-testing]| 10 veri kümeleri, veri kümesi başına 1000 utterances|
| **[Bileşik](./luis-concept-entity-types.md)|100 ile en fazla 10 alt öğeleri |
| Açık listesi | uygulama başına 50|
| **[Hiyerarşik](./luis-concept-entity-types.md) |100 ile en fazla 10 alt öğeleri |
| [Hedefleri][intents]|uygulama başına 500<br>[Gönderme tabanlı](https://github.com/Microsoft/botbuilder-tools/tree/master/Dispatch) uygulamasına sahip karşılık gelen 500 dağıtma kaynakları|
| [Liste varlıklar](./luis-concept-entity-types.md) | Üst: 50, alt: 20.000 öğeleri. Kurallı adı * varsayılan karakter maks. Eş anlamlıları uzunluk sınırlaması vardır. |
| [Desenler](luis-concept-patterns.md)|uygulama başına 500 desenleri.<br>Desen uzunluğu en fazla 400 karakter olabilir.<br>Deseni başına 3 Pattern.any varlıklar<br>En fazla 2 iç içe geçmiş isteğe bağlı metinleri deseninde|
| [Pattern.Any](./luis-concept-entity-types.md)|uygulama başına 100 deseni başına 3 pattern.any varlıklar |
| [Tümcecik listesi][phrase-list]|10 tümcecik listeleri, liste başına 5.000 öğeleri|
| [Önceden oluşturulmuş varlıklar](./Pre-builtEntities.md) | sınır|
| [Normal ifade varlıkları](./luis-concept-entity-types.md)|20 varlıklar<br>500 karakter maks. Normal ifade varlık deseni|
| [Roller](luis-concept-roles.md)|uygulama başına 300 rolleri. Varlık başına 10 rolleri|
| **[Basit](./luis-concept-entity-types.md)| 100 varlık|
| [utterance][utterances] | 500 karakter|
| [Utterances][utterances] | uygulama başına 15.000|
| [Sürüm adı][luis-how-to-manage-versions] | alfasayısal ve süre sınırlı 10 karakter (.) |

* Varsayılan karakter en çok 50 karakter olabilir. 

** Basit, hiyerarşik ve bileşik varlıkları toplam sayısı 100 aşamaz. Hiyerarşik varlıklar, bileşik varlıklar, basit varlıkları ve hiyerarşik alt varlıkları toplam hata sayısı 330 aşamaz. 

## <a name="intent-and-entity-naming"></a>Amacı ve varlık adlandırma
Amacı ve varlık adları şu karakterleri kullanmayın:

|Karakter|Ad|
|--|--|
|`{`|Sol süslü ayraç|
|`}`|Sağ süslü ayraç|
|`[`|Sol ayraç|
|`]`|Sağ köşeli ayraç|
|`\`|Ters eğik çizgi|

## <a name="key-limits"></a>Anahtar sınırları
Geliştirme anahtarının, yazma ve uç noktası için farklı sınırları vardır. HALUK hizmet uç noktası anahtarı yalnızca son nokta sorguları için geçerlidir.

|Anahtar|Yazma|Uç Nokta|Amaç|
|--|--|--|--|
|Yazma Başlatıcı|1 milyon/ay, 5/saniye|1 bin/ay, 5/saniye|HALUK uygulamanızı yazma|
|[Abonelik] [ pricing] - F0 - ücretsiz katmanı |geçersiz|10 bin/ay, 5/saniye|HALUK uç noktanızı sorgulama|
|[Abonelik] [ pricing] - S0 - temel katmanı|geçersiz|50/saniye|HALUK uç noktanızı sorgulama|
|[Düşünceleri analiz tümleştirme](publishapp.md#enable-sentiment-analysis)|geçersiz|ücret ödemeden|Anahtar tümcecik veri ayıklama düşünceleri bilgilerini ekleme |
|Konuşma tümleştirme|geçersiz|$5.50 ABD Doları/1 bin uç nokta istekleri|Metin utterance konuşulan utterance dönüştürmek ve HALUK sonuçları Döndür|

## <a name="keyboard-controls"></a>Klavye denetimleri

|Klavye girişi | Açıklama | 
|--|--|
|Denetim + E|belirteçleri ve utterances listesi üzerinde varlıklar arasında geçiş yapar|

## <a name="website-sign-in-time-period"></a>Web sitesi oturum zaman diliminde

Oturum açma erişim içindir **60 dakika**. Bu süre sonra bu hatayı alırsınız. Yeniden oturum açmanız gerekir.

[luis-get-started-create-app]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app
[batch-testing]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-test#batch-testing
[intents]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-intent
[phrase-list]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-feature
[utterances]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-utterance
[luis-how-to-manage-versions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-manage-versions
[pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
<!-- TBD: fix this link -->
[speech-to-intent-pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
