---
title: Metin analizi API'si, desteklenen diller
titleSuffix: Azure Cognitive Services
description: Metin analizi API'si işlemleri için listesi sunulmuştur ve önizleme dil desteği. Yaklaşım analizi, anahtar ifade ayıklama ve dil algılama için geçerlidir.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.technology: text-analytics
ms.topic: conceptual
ms.date: 09/12/2018
ms.author: ashmaka
ms.openlocfilehash: 984947903a91837910493618bd7abc2cdfe0da71
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45603252"
---
# <a name="supported-languages-in-the-text-analytics-cognitive-service"></a>Metin analizi Bilişsel hizmetinde desteklenen diller

Bu makalede her işlem için desteklenen dilleri açıklar: yaklaşım analizi, anahtar ifade ayıklama ve dil algılama.

## <a name="language-detection"></a>Dil Algılama

Metin analizi API'si, en fazla 120 farklı dillerde algılayabilir. Dil algılama dilinin "betik" döndürür. Örneğin, tümcecik "I sahip bir köpek" döndürür `en` yerine `en-US`. Dil algılama yeteneği döndürdüğü Çince, yalnızca özel bir durum olduğu `zh_CHS` veya `zh_CHT` sağlanan metin verilen betiği belirleyebilirseniz. Burada belirli bir betik tanımlanamıyor Çince belge durumlarda, yalnızca döndüreceği `zh`.

## <a name="sentiment-analysis-key-phrase-extraction-and-entity-linking"></a>Yaklaşım analizi, anahtar ifade ayıklama ve varlık bağlama

Yaklaşım analizi, anahtar ifade ayıklama ve varlık bağlama, desteklenen dillerin listesini Çözümleyicileri ek diller dil kurallarına uyum sağlamak için daraltılmış daha Seçici.

## <a name="language-list-and-status"></a>Dil listesini ve durumu

Dil desteği başlangıçta Mezun genel kullanıma (GA) durumuna birbirinden ve metin analizi hizmetinin genel Önizleme aşamasında kullanıma sunulma. Önizleme aşamasında için genel kullanıma sunulan metin analizi API'si geçişi sırasında bile kalmasına diller için mümkündür.

| Dil    | Dil kodu | Yaklaşım | Anahtar ifadeler | Varlık Bağlama |   Notlar  |
|:----------- |:-------------:|:---------:|:-----------:|:-----------:|:-----------:
| Danca      | `da`          | ✔ \*     | ✔           |             |     |
| Hollanda dili       | `nl`          | ✔ \*     | ✔          |             |     |
| Türkçe     | `en`          | ✔        | ✔           |  ✔ \*   |      |
| Fince     | `fi`          | ✔ \*     | ✔           |             |     |
| Fransızca      | `fr`          | ✔        | ✔           |             |     |
| Almanca      | `de`          | ✔ \*     | ✔           |            |     |
| Yunanca       | `el`          | ✔ \*     |             |            |     |
| İtalyanca     | `it`          | ✔ \*     | ✔           |             |     |
| Japonca    | `ja`          |          | ✔           |            |     |
| Kore dili      | `ko`          |          | ✔           |            |     |
| Norveççe (Bokmal) | `no`          | ✔ \*     |  ✔          |             |     |
| Lehçe      | `pl`          | ✔ \*     |  ✔          |             |     |
| Portekizce (Portekiz) | `pt-PT`| ✔        |  ✔          |       |`pt` Ayrıca kabul edildi|
| Portekizce (Brezilya)   | `pt-BR`|          |  ✔   |         |     |
| Rusça     | `ru`          | ✔ \*     | ✔           |             |     |
| İspanyolca     | `es`          | ✔        | ✔           |     |     |
| İsveç dili     | `sv`          | ✔ \*     | ✔           |             |     |
| Türkçe     | `tr`          | ✔ \*     |             |             |     |

\* dil desteği Önizleme gösterir

## <a name="see-also"></a>Ayrıca bkz.

[Bilişsel hizmetler belgeleri sayfası](https://docs.microsoft.com/azure/cognitive-services/)   
[Bilişsel Hizmetler Ürün sayfası](https://azure.microsoft.com/services/cognitive-services/)
