---
title: Dil desteği - metin analizi API'si
titleSuffix: Azure Cognitive Services
description: "Doğal metin analizi API'si tarafından desteklenen dillerin listesi. Bu makalede her işlem için desteklenen dilleri açıklar: yaklaşım analizi, anahtar ifade ayıklama, dil algılama ve varlık tanıma."
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 06/18/2019
ms.author: aahi
ms.openlocfilehash: 704a1193eb47f9346900c6c8a003122c30c8ab44
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203964"
---
# <a name="language-and-region-support-for-the-text-analytics-api"></a>Metin analizi API'si için dil ve bölge desteği

Bu makalede her işlem için desteklenen dilleri açıklar: yaklaşım analizi, anahtar ifade ayıklama, dil algılama ve adlandırılmış varlık tanıma.

## <a name="language-detection"></a>Dil Algılama

Metin analizi API'si, çok çeşitli diller, çeşitleri, diyalektler ve bölge/kültürel bazı diller algılayabilir.  Dil algılama dilinin "betik" döndürür. Örneğin, tümcecik "I sahip bir köpek" döndürür `en` yerine `en-US`. Dil algılama yeteneği döndürdüğü Çince, yalnızca özel bir durum olduğu `zh_CHS` veya `zh_CHT` sağlanan metin verilen betiği belirleyebilirseniz. Burada belirli bir betik tanımlanamıyor Çince belge durumlarda, yalnızca döndüreceği `zh`.

Biz bu özelliğin dillerin tam listesini yayımlama, ancak çok çeşitli diller, çeşitleri, diyalektler ve bölge/kültürel bazı diller algılayabilir. 

Daha az sık kullanılan bir dille ifade içeriğiniz varsa, bir kod döndürür görmek için dil algılama deneyebilirsiniz. Yanıt algılanamayan diller için `unknown`.

## <a name="sentiment-analysis-key-phrase-extraction-and-named-entity-recognition"></a>Yaklaşım analizi, anahtar ifade ayıklama ve adlandırılmış varlık tanıma

Yaklaşım analizi, anahtar ifade ayıklama ve varlık tanıma, desteklenen dillerin listesini Çözümleyicileri ek diller dil kurallarına uyum sağlamak için daraltılmış daha Seçici. Eksiksiz bir listesi için destek [varlık türleri](how-tos/text-analytics-how-to-entity-linking.md#supported-types-for-named-entity-recognition) şu anda aşağıdaki diller için sınırlıdır: 
* Türkçe
* Çince (Basitleştirilmiş)
* Fransızca
* Almanca
* İspanyolca

Yalnızca `Person`, `Location` ve `Organization` adlandırılmış varlık, diğer diller için döndürülür.

## <a name="language-list-and-status"></a>Dil listesini ve durumu

Dil desteği başlangıçta Mezun genel kullanıma (GA) durumuna birbirinden ve metin analizi hizmetinin genel Önizleme aşamasında kullanıma sunulma. Önizleme aşamasında için genel kullanıma sunulan metin analizi API'si geçişi sırasında bile kalmasına diller için mümkündür.

| Dil    | Dil kodu | Yaklaşım | Anahtar ifadeler | Adlandırılmış Varlık Tanıma |   Notlar  |
|:----------- |:-------------:|:---------:|:-----------:|:-----------:|:-----------:
| Arapça      | `ar`          |           |             | ✔ \*                     | |
| Çekçe       | `cs`          |           |             | ✔ \*                     | |
| Çince (Basitleştirilmiş) | `zh-CN`|           |             | ✔ \*        |    |
| Danca      | `da`          | ✔ \*     | ✔           | ✔ \*            |     |
| Felemenkçe       | `nl`          | ✔ \*     | ✔          |  ✔ \*           |     |
| Türkçe     | `en`          | ✔        | ✔           |  ✔ \*\*     |      |
| Fince     | `fi`          | ✔ \*     | ✔           |  ✔ \*           |     |
| Fransızca      | `fr`          | ✔        | ✔           |  ✔ \*           |     |
| Almanca      | `de`          | ✔ \*     | ✔           |  ✔ \*          |     |
| Yunanca       | `el`          | ✔ \*     |             |            |     |
| Macarca   | `hu`          |           |             |  ✔ \*          |     | 
| İtalyanca     | `it`          | ✔ \*     | ✔           |  ✔ \*           |     |
| Japonca    | `ja`          |          | ✔           |  ✔ \*          |     |
| Korece      | `ko`          |          | ✔           |  ✔ \*          |     |
| Norwegian  (Bokmål) | `no`  | ✔ \*     |  ✔          | ✔ \*            |     |
| Lehçe      | `pl`          | ✔ \*     |  ✔          |  ✔ \*           |     |
| Portekizce (Portekiz) | `pt-PT`| ✔        |  ✔          | ✔ \*      |`pt` Ayrıca kabul edildi|
| Portekizce (Brezilya)   | `pt-BR`|          |  ✔   |  ✔ \*       |     |
| Rusça     | `ru`          | ✔ \*     | ✔           |  ✔ \*           |     |
| İspanyolca     | `es`          | ✔        |            |   ✔ \*\*      |     | 
| İsveççe     | `sv`          | ✔ \*     | ✔           |   ✔ \*          |     |
| Türkçe     | `tr`          | ✔ \*     |             |   ✔ \*          |  |

\* Dil desteği Önizleme aşamasındadır.

\*\* [Adlandırılmış varlık tanıma](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-ner) ve [varlık bağlama](how-tos/text-analytics-how-to-entity-linking.md#entity-linking) her ikisi de bu dil için kullanılabilir.    

## <a name="see-also"></a>Ayrıca bkz.

[Bilişsel hizmetler belgeleri sayfası](https://docs.microsoft.com/azure/cognitive-services/)   
[Bilişsel Hizmetler Ürün sayfası](https://azure.microsoft.com/services/cognitive-services/)
