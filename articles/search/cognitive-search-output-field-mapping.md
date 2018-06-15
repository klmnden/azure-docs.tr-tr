---
title: Harita bilişsel arama zenginleştirilmiş Azure Search dizinlerini çıkış alanlara giriş alanlarının | Microsoft Docs
description: Ayıklayın ve kaynak veri alanları zenginleştirmek ve Azure Search dizini çıkış alanlarına eşleme.
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 67e4798070a73eebb8f61b0b260e3104e9ae6237
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33790954"
---
# <a name="how-to-map-enriched-fields-to-a-searchable-index"></a>Arama yapılabilir bir dizin zenginleştirilmiş alanlarını eşleme

Bu makalede, zenginleştirilmiş giriş alanları arama yapılabilir bir dizin çıkış alanlarına eşleme öğrenin. Bulduktan sonra [bir skillset tanımlanan](cognitive-search-defining-skillset.md), doğrudan değerleri search dizininizi belirli bir alana katkı yetenek çıktı alanlarının eşlemeniz gerekir. Alan eşlemelerini içerik zenginleştirilmiş belgelerden dizine taşımak için gereklidir.


## <a name="use-outputfieldmappings"></a>OutputFieldMappings kullanın
Alanları eşleme ekleyin `outputFieldMappings` aşağıda gösterildiği gibi dizin oluşturucu tanımı:

```http
PUT https://[servicename].search.windows.net/indexers/[indexer name]?api-version=2017-11-11-Preview
api-key: [admin key]
Content-Type: application/json
```

İstek gövdesini aşağıdaki gibi yapılandırılmış:

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
Her çıktı alan eşlemesi için başvurulan (targetFieldName) dizininde (sourceFieldName) zenginleştirilmiş alanının adı ve alan adını ayarlayın.

Bir sourceFieldName yolunda bir veya birden çok birden çok öğe temsil edebilir. Yukarıdaki örnekte ```/document/content/sentiment``` tek bir sayısal değer temsil ederken ```/document/content/organizations/*/description``` birkaç kuruluş açıklamaları temsil eder. Durumlarda çeşitli öğeleri olduğu bunlar "öğelerin her birini içeren bir diziye düzleştirilmiş olup". Daha fazla concretely için ```/document/content/organizations/*/description``` örnek, verileri *açıklamaları* alan uygulamamız görünecektir açıklamaları düz bir dizi gibi dizine önce:

```
 ["Microsoft is a company in Seattle","LinkedIn's office is in San Francisco"]
```
## <a name="next-steps"></a>Sonraki adımlar
Aranabilir alanlara zenginleştirilmiş alanlarınızı eşledikten sonra alan özniteliklerini aranabilir alanların her biri için ayarlayabileceğiniz [dizin tanımı bir parçası olarak](search-what-is-an-index.md).

Alan eşleme hakkında daha fazla bilgi için bkz: [alan eşlemelerini Azure Search'te dizin oluşturucular içinde](search-indexer-field-mappings.md).