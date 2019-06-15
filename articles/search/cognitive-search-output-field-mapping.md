---
title: Çıktı alanlarını - Azure Search için giriş alanlarını eşleme bilişsel arama zenginleştirilmiş
description: Çıkarın ve kaynak veri alanları zenginleştirin ve Azure Search dizini çıkış alanlarına eşleme.
manager: pablocas
author: luiscabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 506acee6cd9cd3c50e10f1c45768230564eeaaf1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65022087"
---
# <a name="how-to-map-enriched-fields-to-a-searchable-index"></a>Arama yapılabilir bir dizin zenginleştirilmiş alanlarını eşleme

Bu makalede, zenginleştirilmiş giriş alanlarını çıkış arama yapılabilir bir dizin alanlarına eşleme öğrenin. Yapılandırmasını tamamladıktan [bir beceri kümesi tanımlanan](cognitive-search-defining-skillset.md), doğrudan arama dizininizdeki belirli bir alandaki değerlere katkıda bulunan tüm beceri çıktı alanlarını eşlemeniz gerekir. Alan eşlemelerini dizine zenginleştirilmiş belgelerden içerik taşımak için gerekli değildir.


## <a name="use-outputfieldmappings"></a>OutputFieldMappings kullanın
Alanları eşleme ekleyin `outputFieldMappings` aşağıda gösterildiği gibi dizin oluşturucu tanımı:

```http
PUT https://[servicename].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
api-key: [admin key]
Content-Type: application/json
```

İstek gövdesi aşağıdaki gibi yapılandırılır:

```json
{
    "name": "myIndexer",
    "dataSourceName": "myDataSource",
    "targetIndexName": "myIndex",
    "skillsetName": "myFirstSkillSet",
    "fieldMappings": [
        {
            "sourceFieldName": "metadata_storage_path",
            "targetFieldName": "id",
            "mappingFunction": {
                "name": "base64Encode"
            }
        }
    ],
    "outputFieldMappings": [
        {
            "sourceFieldName": "/document/content/organizations/*/description",
            "targetFieldName": "descriptions"
        },
        {
            "sourceFieldName": "/document/content/organizations",
            "targetFieldName": "orgNames"
        },
        {
            "sourceFieldName": "/document/content/sentiment",
            "targetFieldName": "sentiment"
        }
    ]
}
```
Her çıkış alan eşlemesi için başvurulan ' % s'dizininde (targetFieldName) olarak zenginleştirilmiş alanı (sourceFieldName) adını ve alan adını ayarlayın.

Bir sourceFieldName yolunda bir öğe veya birden çok öğe temsil edebilir. Yukarıdaki örnekte ```/document/content/sentiment``` tek sayısal bir değeri temsil ederken ```/document/content/organizations/*/description``` birçok kuruluş açıklamaları temsil eder. Durumlarda birkaç öğe olduğunda, bunlar "öğelerin her birini içeren bir diziye düzleştirilir". Daha fazla concretely için ```/document/content/organizations/*/description``` örnek, verileri *açıklamaları* dizine eklenmeden önce alan düz bir açıklamaları dizisi gibi görünür:

```
 ["Microsoft is a company in Seattle","LinkedIn's office is in San Francisco"]
```
## <a name="next-steps"></a>Sonraki adımlar
Aranabilir alanlara zenginleştirilmiş alanlarınızı eşledikten sonra alan öznitelikleri aranabilir alanların her biri için ayarlayabileceğiniz [dizin tanımını bir parçası olarak](search-what-is-an-index.md).

Alan eşleme hakkında daha fazla bilgi için bkz. [alan eşlemeleri Azure Search dizin oluşturucularında](search-indexer-field-mappings.md).
