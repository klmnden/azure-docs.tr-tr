---
title: Dil desteği - metin analizi API'si
titleSuffix: Azure Cognitive Services
description: "Doğal metin analizi API'si tarafından desteklenen dillerin listesi. Bu makalede her işlem için desteklenen dilleri açıklar: yaklaşım analizi, anahtar ifade ayıklama, dil algılama ve varlık tanıma."
services: cognitive-services
author: ashmaka
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: ashmaka
ms.openlocfilehash: 421d891ea6c580cfbdd7cfc40755b43e1d4b207e
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55871294"
---
# <a name="language-and-region-support-for-the-text-analytics-api"></a>Metin analizi API'si için dil ve bölge desteği

Bu makalede her işlem için desteklenen dilleri açıklar: yaklaşım analizi, anahtar ifade ayıklama ve dil algılama.

## <a name="language-detection"></a>Dil Algılama

Metin analizi API'si, en fazla 120 farklı dillerde algılayabilir. Dil algılama dilinin "betik" döndürür. Örneğin, tümcecik "I sahip bir köpek" döndürür `en` yerine `en-US`. Dil algılama yeteneği döndürdüğü Çince, yalnızca özel bir durum olduğu `zh_CHS` veya `zh_CHT` sağlanan metin verilen betiği belirleyebilirseniz. Burada belirli bir betik tanımlanamıyor Çince belge durumlarda, yalnızca döndüreceği `zh`.

## <a name="sentiment-analysis-key-phrase-extraction-and-entity-recognition"></a>Yaklaşım analizi, anahtar ifade ayıklama ve varlık tanıma

Yaklaşım analizi, anahtar ifade ayıklama ve varlık tanıma, desteklenen dillerin listesini Çözümleyicileri ek diller dil kurallarına uyum sağlamak için daraltılmış daha Seçici.

## <a name="language-list-and-status"></a>Dil listesini ve durumu

Dil desteği başlangıçta Mezun genel kullanıma (GA) durumuna birbirinden ve metin analizi hizmetinin genel Önizleme aşamasında kullanıma sunulma. Önizleme aşamasında için genel kullanıma sunulan metin analizi API'si geçişi sırasında bile kalmasına diller için mümkündür.

| Dil    | Dil kodu | Yaklaşım | Anahtar ifadeler | Varlık Tanıma |   Notlar  |
|:----------- |:-------------:|:---------:|:-----------:|:-----------:|:-----------:
| Danca      | `da`          | ✔ \*     | ✔           |             |     |
| Felemenkçe       | `nl`          | ✔ \*     | ✔          |             |     |
| Türkçe     | `en`          | ✔        | ✔           |  ✔ \*   |      |
| Fince     | `fi`          | ✔ \*     | ✔           |             |     |
| Fransızca       | `fr`          | ✔        | ✔           |             |     |
| Almanca       | `de`          | ✔ \*     | ✔           |            |     |
| Yunanca       | `el`          | ✔ \*     |             |            |     |
| İtalyanca     | `it`          | ✔ \*     | ✔           |             |     |
| Japonca    | `ja`          |          | ✔           |            |     |
| Korece      | `ko`          |          | ✔           |            |     |
| Norwegian  (Bokmål) | `no`          | ✔ \*     |  ✔          |             |     |
| Lehçe      | `pl`          | ✔ \*     |  ✔          |             |     |
| Portekizce (Portekiz) | `pt-PT`| ✔        |  ✔          |       |`pt` Ayrıca kabul edildi|
| Portekizce (Brezilya)   | `pt-BR`|          |  ✔   |         |     |
| Rusça     | `ru`          | ✔ \*     | ✔           |             |     |
| İspanyolca      | `es`          | ✔        | ✔           |   ✔ \*\*      |     |
| İsveççe     | `sv`          | ✔ \*     | ✔           |             |     |
| Türkçe     | `tr`          | ✔ \*     |             |             |  |

\* dil desteği Önizleme gösterir

\*\* Varlık ayıklama İspanyolca için kullanılabilir, yalnızca [(sürüm 2.1 Önizleme)](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634)

## <a name="see-also"></a>Ayrıca bkz.

[Bilişsel hizmetler belgeleri sayfası](https://docs.microsoft.com/azure/cognitive-services/)   
[Bilişsel Hizmetler Ürün sayfası](https://azure.microsoft.com/services/cognitive-services/)
