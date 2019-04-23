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
ms.date: 02/13/2019
ms.author: aahi
ms.openlocfilehash: 4f1ce8fd44a501f594f3093789d1ef03e664d018
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60008503"
---
# <a name="language-and-region-support-for-the-text-analytics-api"></a>Metin analizi API'si için dil ve bölge desteği

Bu makalede her işlem için desteklenen dilleri açıklar: yaklaşım analizi, anahtar ifade ayıklama, dil algılama ve adlandırılmış varlık tanıma.

## <a name="language-detection"></a>Dil Algılama

Metin analizi API'si, en fazla 120 farklı dillerde algılayabilir. Dil algılama dilinin "betik" döndürür. Örneğin, tümcecik "I sahip bir köpek" döndürür `en` yerine `en-US`. Dil algılama yeteneği döndürdüğü Çince, yalnızca özel bir durum olduğu `zh_CHS` veya `zh_CHT` sağlanan metin verilen betiği belirleyebilirseniz. Burada belirli bir betik tanımlanamıyor Çince belge durumlarda, yalnızca döndüreceği `zh`.

## <a name="sentiment-analysis-key-phrase-extraction-and-named-entity-recognition"></a>Yaklaşım analizi, anahtar ifade ayıklama ve adlandırılmış varlık tanıma

Yaklaşım analizi, anahtar ifade ayıklama ve varlık tanıma, desteklenen dillerin listesini Çözümleyicileri ek diller dil kurallarına uyum sağlamak için daraltılmış daha Seçici.

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
| Fransızca       | `fr`          | ✔        | ✔           |  ✔ \*           |     |
| Almanca       | `de`          | ✔ \*     | ✔           |  ✔ \*          |     |
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
| İspanyolca      | `es`          | ✔        |            |   ✔ \*\*      |     | 
| İsveççe     | `sv`          | ✔ \*     | ✔           |   ✔ \*          |     |
| Türkçe     | `tr`          | ✔ \*     |             |   ✔ \*          |  |

\* Dil desteği Önizleme aşamasındadır.

\*\* Adlandırılmış varlık tanıma ve [varlık bağlama](how-tos/text-analytics-how-to-entity-linking.md) her ikisi de bu dil için kullanılabilir.    

## <a name="see-also"></a>Ayrıca bkz.

[Bilişsel hizmetler belgeleri sayfası](https://docs.microsoft.com/azure/cognitive-services/)   
[Bilişsel Hizmetler Ürün sayfası](https://azure.microsoft.com/services/cognitive-services/)
