---
title: Bilişsel beceriler - Azure Search kullanım dışı
description: Bu sayfa, kullanım dışı olarak kabul edilen bilişsel arama yetenekleri listesini içeren ve yakın gelecekte desteklenmez.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: f773cd298c8faaac90b30d88a74e8ddcb51c3afa
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65021946"
---
# <a name="deprecated-cognitive-search-skills"></a>Kullanım dışı Bilişsel arama yetenekleri

Bu belgede, kullanım dışı olarak kabul edilir bilişsel yetenekleri açıklanmaktadır. İçeriği için aşağıdaki kılavuzu kullanın:

* Nitelik adı: Eşlendiği kullanım dışı bırakılacak beceri adını, @odata.type özniteliği.
* Son kullanılabilir API sürümü: Son sürümü Azure arama Genel API hangi becerilerini ilgili kullanım dışı yetenek içeren oluşturulan/güncelleştirilen olabilir.
* Destek sonu: Desteklenmeyen karşılık gelen beceri kabul edileceği sonra son günü. Daha önce oluşturduğunuz uzmanlık becerileri yine de çalışmaya devam etmelidir ancak kullanıcılar uzağa kullanım dışı bir yetenek geçirmek için önerilir.
* Öneriler: Desteklenen bir yetenek kullanmaya geçiş yolu ilet. Kullanıcıların destek almaya devam etmek için önerilere uyun sürümüne güncelleştirmeleri önerilir.

## <a name="microsoftskillstextnamedentityrecognitionskill"></a>Microsoft.Skills.Text.NamedEntityRecognitionSkill

### <a name="last-available-api-version"></a>Son kullanılabilir API sürümü

2019-05-06-Önizleme

### <a name="end-of-support"></a>Destek sonu

15 Şubat 2019

### <a name="recommendations"></a>Öneriler 

Kullanım [Microsoft.Skills.Text.EntityRecognitionSkill](cognitive-search-skill-entity-recognition.md) yerine. Bu, en yüksek kalitede NamedEntityRecognitionSkill işlevlerini sağlar. Ayrıca daha zengin bilgi karmaşık çıkış alanları içerir.

Öğesine geçirmeyi [varlık tanıma beceri](cognitive-search-skill-entity-recognition.md), bir veya daha fazla yetenek tanımı aşağıdaki değişiklikleri gerçekleştirmeniz gerekecektir. Beceri tanımı kullanarak güncelleştirebilirsiniz [güncelleştirme beceri kümesi API](https://docs.microsoft.com/rest/api/searchservice/update-skillset).

> [!NOTE]
> Şu anda, kavram olarak güvenilirlik puanı desteklenmiyor. `minimumPrecision` Parametresi var. `EntityRecognitionSkill` ve gelecekte kullanım için geriye dönük uyumluluk.

1. *(Gerekli)*  Değişiklik `@odata.type` gelen `"#Microsoft.Skills.Text.NamedEntityRecognitionSkill"` için `"#Microsoft.Skills.Text.EntityRecognitionSkill"`.

2. *(İsteğe bağlı)*  Yapıyorsanız, kullanım `entities` çıktı, kullanın `namedEntities` karmaşık koleksiyon çıkışı `EntityRecognitionSkill` yerine. Kullanabileceğiniz `targetName` beceriye eşlemek için bir ek açıklama tanımı adlı `entities`.

3. *(İsteğe bağlı)*  Açıkça belirtmezseniz `categories`, `EntityRecognitionSkill` farklı türde bir kategori tarafından desteklenen belirtilenlerin döndürebilir `NamedEntityRecognitionSkill`. Bu istenmeyen bir davranıştır, açıkça ayarlandığından emin olun `categories` parametresi `["Person", "Location", "Organization"]`.

    _Örnek geçiş tanımları_

    * Basit bir geçiş

        _(Önce) NamedEntityRecognition beceri tanımı_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
            "categories": [ "Person"],
            "defaultLanguageCode": "en",
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            }
            ]
        }
        ```
        _(Sonra) EntityRecognition beceri tanımı_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
            "categories": [ "Person"],
            "defaultLanguageCode": "en",
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            }
            ]
        }
        ```
    
    * Biraz karmaşık geçiş

        _(Önce) NamedEntityRecognition beceri tanımı_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
            "defaultLanguageCode": "en",
            "minimumPrecision": 0.1,
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            },
            {
                "name": "entities"
            }
            ]
        }
        ```
        _(Sonra) EntityRecognition beceri tanımı_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
            "categories": [ "Person", "Location", "Organization" ],
            "defaultLanguageCode": "en",
            "minimumPrecision": 0.1,
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            },
            {
                "name": "namedEntities",
                "targetName": "entities"
            }
            ]
        }
        ```

## <a name="see-also"></a>Ayrıca bkz.

+ [Önceden tanımlanmış beceriler](cognitive-search-predefined-skills.md)
+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)
+ [Varlık tanıma beceri](cognitive-search-skill-entity-recognition.md)
