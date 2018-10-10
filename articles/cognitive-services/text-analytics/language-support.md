---
title: Dil desteği - metin analizi API'si
titleSuffix: Azure Cognitive Services
description: "Doğal metin analizi API'si tarafından desteklenen dillerin listesi. Bu makalede her işlem için desteklenen dilleri açıklar: yaklaşım analizi, anahtar ifade ayıklama, dil algılama ve varlık tanıma."
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: ashmaka
ms.openlocfilehash: 9d09115d3d55d5ba44d35cb0b2009d800ab42a2b
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2018
ms.locfileid: "48886660"
---
# <a name="language-and-region-support-for-the-text-analytics-api"></a>Metin analizi API'si için dil ve bölge desteği

Bu makalede her işlem için desteklenen dilleri açıklar: yaklaşım analizi, anahtar ifade ayıklama ve dil algılama.

## <a name="language-detection"></a>Dil Algılama

Metin analizi API'si, en fazla 120 farklı dillerde algılayabilir. Dil algılama dilinin "betik" döndürür. Örneğin, tümcecik "I sahip bir köpek" döndürür `en` yerine `en-US`. Dil algılama yeteneği döndürdüğü Çince, yalnızca özel bir durum olduğu `zh_CHS` veya `zh_CHT` sağlanan metin verilen betiği belirleyebilirseniz. Burada belirli bir betik tanımlanamıyor Çince belge durumlarda, yalnızca döndüreceği `zh`.

## <a name="sentiment-analysis-key-phrase-extraction-and-entity-recognition"></a>Yaklaşım analizi, anahtar ifade ayıklama ve varlık tanıma

Yaklaşım analizi, anahtar ifade ayıklama ve varlık tanıma, desteklenen dillerin listesini Çözümleyicileri ek diller dil kurallarına uyum sağlamak için daraltılmış daha Seçici.

## <a name="language-list-and-status"></a>Dil listesini ve durumu

Dil desteği başlangıçta Mezun genel kullanıma (GA) durumuna birbirinden ve metin analizi hizmetinin genel Önizleme aşamasında kullanıma sunulma. Önizleme aşamasında için genel kullanıma sunulan metin analizi API'si geçişi sırasında bile kalmasına diller için mümkündür.

| Dil    | Dil kodu | Yaklaşım | Anahtar ifadeler | Varlık tanıma |   Notlar  |
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
| Türkçe     | `tr`          | ✔ \*     |             |             |  |

\* dil desteği Önizleme gösterir

## <a name="see-also"></a>Ayrıca bkz.

[Bilişsel hizmetler belgeleri sayfası](https://docs.microsoft.com/azure/cognitive-services/)   
[Bilişsel Hizmetler Ürün sayfası](https://azure.microsoft.com/services/cognitive-services/)
