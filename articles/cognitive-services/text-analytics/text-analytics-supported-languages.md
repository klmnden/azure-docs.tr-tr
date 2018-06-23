---
title: Metin analizi API - Azure Bilişsel hizmetler desteklenen diller | Microsoft Docs
description: Genel olarak kullanılabilir listesi ve önizleme dil metin analizi API işlemleri için destekler. Düşünceleri analiz, anahtar tümcecik ayıklama ve dil algılama için geçerlidir.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.technology: text-analytics
ms.topic: conceptual
ms.date: 05/02/2018
ms.author: ashmaka
ms.openlocfilehash: 2d341cfaf261bea6367bb55dd5d322f419e22d34
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355186"
---
# <a name="supported-languages-in-the-text-analytics-api"></a>Metin analizi API'sindeki desteklenen diller

Bu makale, her işlem için desteklenen hangi dilleri açıklar: düşünceleri analiz, anahtar tümcecik ayıklama ve dil algılama.

## <a name="language-detection"></a>Dil Algılama

Metin analizi API en fazla 120 farklı dillerde algılayabilir. Dil algılama "komut dosyası" dilinin döndürür. Örneğin, tümcecik "Sahibim bir köpek" döndürür `en` yerine `en-US`. Yalnızca özel Çince dil algılama yeteneği burada döndürür durumdur `zh_CHS` veya `zh_CHT` sağlanan metin verilen komut dosyası belirleyebilirseniz. Burada belirli bir betik belirlenemedi Çince bir belge için durumlarda, yalnızca döndürür `zh`.

## <a name="sentiment-analysis-key-phrase-extraction-and-entity-linking"></a>Düşünceleri çözümleme, anahtar tümcecik ayıklama ve varlık bağlama

Düşünceleri analiz, anahtar tümcecik ayıklama ve varlık bağlama, desteklenen dillerin listesi çözümleyiciler ek dilleri dil kurallarına uygun hale getirmek için Gelişmiş daha Seçici içindir.

## <a name="language-list-and-status"></a>Dil listesi ve durumu

Dil desteği başlangıçta genel olarak kullanılabilir (GA) durumuna birbirinden ve genel metin Analytics hizmeti, Mezun önizlemede alınır. Metin analizi API geçişleri genel olarak kullanılabilir sırasında bile önizlemede kalmasına diller için mümkündür.

| Dil    | Dil kodu | Yaklaşım | Anahtar ifadeler | Varlık Bağlama |   Notlar  |
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
| Kore dili      | `ko`          |          | ✔           |            |     |
| Norveççe (Bokmål) | `no`          | ✔ \*     |  ✔          |             |     |
| Lehçe      | `pl`          | ✔ \*     |  ✔          |             |     |
| Portekizce (Portekiz) | `pt-PT`| ✔        |  ✔          |       |`pt` Ayrıca kabul edildi|
| Portekizce (Brezilya)   | `pt-BR`|          |  ✔   |         |     |
| Rusça     | `ru`          | ✔ \*     | ✔           |             |     |
| İspanyolca      | `es`          | ✔        | ✔           |     |     |
| İsveç dili     | `sv`          | ✔ \*     | ✔           |             |     |
| Türkçe     | `tr`          | ✔ \*     |             |             |     |

\* dil desteği önizlemede gösterir

## <a name="see-also"></a>Ayrıca bkz.

[Bilişsel hizmetler belge sayfası](https://docs.microsoft.com/azure/cognitive-services/)   
[Bilişsel hizmetler ürün sayfası](https://azure.microsoft.com/services/cognitive-services/)
