---
title: Şema eşleme kopyalama etkinliğindeki | Microsoft Docs
description: Kopyalama etkinliği Azure Data factory'de veri veri kopyalarken havuz şemaları ve veri türleri kaynak verilerden nasıl eşlendiğini hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: 9108f83e854b51720c64c5a74a828543cc5e7688
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64875812"
---
# <a name="schema-mapping-in-copy-activity"></a>Kopyalama etkinliğinde şema eşleme

Bu makalede Azure Data Factory kopyalama etkinliği, şema eşleme ve veri türü eşlemesi veri kaynağı verilerden nasıl yaptığını açıklar, veri kopyalama yürütün.

## <a name="schema-mapping"></a>Şema eşleme

Sütun eşlemesi, havuz kaynaktan veri kopyalama işlemi sırasında uygulanır. Varsayılan olarak, kopyalama etkinliği **sütun adlarına göre havuz için kaynak verileri eşleme**. Belirtebileceğiniz [açık eşleme](#explicit-mapping) sütun eşlemesi gereksinimlerinize göre özelleştirmek için. Kopyalama etkinliği daha açık belirtmek gerekirse:

1. Veri kaynağından okumak ve kaynağı şemasını belirleme
2. Ada göre sütunları eşlemek için varsayılan sütun eşlemesi'ı kullanın veya açık sütun eşlemesi belirtilmişse uygulayın.
3. Havuz veri yazma

### <a name="explicit-mapping"></a>Açık bir eşleme

Belirtebileceğiniz sütunları kopyalama etkinliği eşlemek için -> `translator`  ->  `mappings` özelliği. Aşağıdaki örnek, verileri Azure SQL veritabanı'na sınırlandırılmış metin kopyalamak için bir işlem hattındaki kopyalama etkinliği tanımlar.

```json
{
    "name": "CopyActivity",
    "type": "Copy",
    "inputs": [{
        "referenceName": "DelimitedTextInput",
        "type": "DatasetReference"
    }],
    "outputs": [{
        "referenceName": "AzureSqlOutput",
        "type": "DatasetReference"
    }],
    "typeProperties": {
        "source": { "type": "DelimitedTextSource" },
        "sink": { "type": "SqlSink" },
        "translator": {
            "type": "TabularTranslator",
            "mappings": [
                {
                    "source": {
                        "name": "UserId",
                        "type": "Guid"
                    },
                    "sink": {
                        "name": "MyUserId"
                    }
                }, 
                {
                    "source": {
                        "name": "Name",
                        "type": "String"
                    },
                    "sink": {
                        "name": "MyName"
                    }
                }, 
                {
                    "source": {
                        "name": "Group",
                        "type": "String"
                    },
                    "sink": {
                        "name": "MyGroup"
                    }
                }
            ]
        }
    }
}
```

Aşağıdaki özellikler altında desteklenen `translator`  ->  `mappings` Nesne -> `source` ve `sink`:

| Özellik | Açıklama                                                  | Gerekli |
| -------- | ------------------------------------------------------------ | -------- |
| name     | Kaynak veya havuz sütunun adı.                           | Evet      |
| ordinal  | Sütun dizini. 1 ile başlayın. <br>Uygula ve sınırlandırılmış metin üst bilgi satırı olmadan kullanarak gereklidir. | Hayır       |
| path     | Ayıklanacak veya eşleme her bir alan için JSON yolu ifadesini. Hiyerarşik veriler için MongoDB/REST örn uygulayın.<br>Kök nesne altındaki alanlar için root $ ile JSON yolunu başlatır; tarafından seçilen dizinin içindeki alanlar için `collectionReference` özelliği, JSON yolu dizi öğeden başlar. | Hayır       |
| type     | Veri Fabrikası geçici sütunun veri türünü kaynak veya havuz. | Hayır       |
| culture  | Kaynak veya havuz sütun kültür. <br>Türü olduğunda geçerli `Datetime` veya `Datetimeoffset`. Varsayılan değer: `en-us`. | Hayır       |
| format   | Biçim türü olduğunda kullanılacak dize `Datetime` veya `Datetimeoffset`. Başvurmak [özel tarih ve saat biçim dizeleri](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings) datetime biçimine üzerinde. | Hayır       |

Aşağıdaki özellikler altında desteklenen `translator`  ->  `mappings` nesneyle yanı sıra `source` ve `sink`:

| Özellik            | Açıklama                                                  | Gerekli |
| ------------------- | ------------------------------------------------------------ | -------- |
| collectionReference | Yalnızca hiyerarşik veri örn MongoDB/REST kaynağı olduğunda desteklenir.<br>Yineleme ve veri nesneleri ayıklamak istiyorsanız **bir dizi alanındaki** nesne başına daha fazla satır arası uygulamak için o dizinin JSON yolunu belirtmek için dönüştürme ve aynı deseni. | Hayır       |

### <a name="alternative-column-mapping"></a>Alternatif sütun eşleme

Kopyalama belirtebilirsiniz etkinlik -> `translator`  ->  `columnMappings` şeklinde tablosal verileri arasında eşleme için. Bu durumda, "yapı" bölümü, girdi ve çıktı veri kümeleri için gereklidir. Sütun eşleme destekler **eşleme tüm veya kaynak veri kümesindeki tüm sütunları havuz veri kümesi "yapı" içinde "yapısına" sütun alt kümesi**. Bir özel durumu hata koşulları şunlardır:

* Sorgu sonucu giriş veri kümesi "yapı" bölümünde belirtilen bir sütun adı yok. kaynak veri deposu.
* Havuz veri deposu (ile önceden tanımlı bir şeması varsa) çıkış veri kümesi "yapı" bölümünde belirtilen bir sütun adı yok.
* Daha az sütun veya daha fazla sütun "eşlemesinde belirtilen yapısını" havuz veri kümesi.
* Yinelenen eşleme.

Aşağıdaki örnekte, giriş veri kümesi bir yapıya sahiptir ve bir tabloya bir şirket içi Oracle veritabanına işaret eder.

```json
{
    "name": "OracleDataset",
    "properties": {
        "structure":
         [
            { "name": "UserId"},
            { "name": "Name"},
            { "name": "Group"}
         ],
        "type": "OracleTable",
        "linkedServiceName": {
            "referenceName": "OracleLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "SourceTable"
        }
    }
}
```

Bu örnekte, çıktı veri kümesi bir yapıya sahiptir ve Salesfoce tablosuna işaret.

```json
{
    "name": "SalesforceDataset",
    "properties": {
        "structure":
        [
            { "name": "MyUserId"},
            { "name": "MyName" },
            { "name": "MyGroup"}
        ],
        "type": "SalesforceObject",
        "linkedServiceName": {
            "referenceName": "SalesforceLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "SinkTable"
        }
    }
}
```

Aşağıdaki JSON bir işlem hattında kopyalama etkinliği tanımlar. Sütunları kullanarak havuz sütuna eşlenmiş kaynağından **translator** -> **Bunun amacı** özelliği.

```json
{
    "name": "CopyActivity",
    "type": "Copy",
    "inputs": [
        {
            "referenceName": "OracleDataset",
            "type": "DatasetReference"
        }
    ],
    "outputs": [
        {
            "referenceName": "SalesforceDataset",
            "type": "DatasetReference"
        }
    ],
    "typeProperties":    {
        "source": { "type": "OracleSource" },
        "sink": { "type": "SalesforceSink" },
        "translator":
        {
            "type": "TabularTranslator",
            "columnMappings":
            {
                "UserId": "MyUserId",
                "Group": "MyGroup",
                "Name": "MyName"
            }
        }
    }
}
```

Söz dizimini kullanıyorsanız `"columnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"` sütun eşlemesi belirtmek için hala olarak desteklenmektedir-olduğu.

### <a name="alternative-schema-mapping"></a>Alternatif bir şema eşleme

Kopyalama belirtebilirsiniz etkinlik -> `translator`  ->  `schemaMapping` hiyerarşik biçimli verileri ve tablo şeklinde verileri arasında eşleme için örneğin MongoDB/geri KALANINDAN metin dosyası ve Azure Cosmos DB API Oracle Kopyala MongoDB için kopyalayın. Kopyalama etkinliği aşağıdaki özellikler desteklenir `translator` bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği Çeviricisi öğesinin type özelliği ayarlanmalıdır: **TabularTranslator** | Evet |
| schemaMapping | Eşleme ilişki temsil eden anahtar-değer çiftleri koleksiyonu **yan havuz için kaynak taraftan**.<br/>- **Anahtar:** kaynak temsil eder. İçin **tablo kaynağı**, tanımlanan veri kümesi yapısı için; sütun adı belirtin **hiyerarşik kaynak**, ayıklayın ve eşlemek her bir alan için JSON yolu ifadesini belirtin.<br>- **Değer:** havuz temsil eder. İçin **tablo havuz**, tanımlanan veri kümesi yapısı için; sütun adı belirtin **hiyerarşik havuz**, ayıklayın ve eşlemek her bir alan için JSON yolu ifadesini belirtin. <br>Kök nesne altındaki alanlar için hiyerarşik veriler söz konusu olduğunda JSON yolu root $ ile başlar; tarafından seçilen dizinin içindeki alanlar için `collectionReference` özelliği, JSON yolu dizi öğeden başlar.  | Evet |
| collectionReference | Yineleme ve veri nesneleri ayıklamak istiyorsanız **bir dizi alanındaki** nesne başına daha fazla satır arası uygulamak için o dizinin JSON yolunu belirtmek için dönüştürme ve aynı deseni. Bu özellik yalnızca hiyerarşik veri kaynağı olduğunda desteklenir. | Hayır |

**Örnek: Oracle'dan Mongodb'den kopyalayın:**

Örneğin, aşağıdaki içerikle MongoDB belge varsa:

```json
{
    "id": {
        "$oid": "592e07800000000000000000"
    },
    "number": "01",
    "date": "20170122",
    "orders": [
        {
            "prod": "p1",
            "price": 23
        },
        {
            "prod": "p2",
            "price": 13
        },
        {
            "prod": "p3",
            "price": 231
        }
    ],
    "city": [ { "name": "Seattle" } ]
}
```

ve bunu bir Azure SQL tablosuna aşağıdaki biçimde dizi içindeki verileri düzleştirme tarafından kopyalamak istediğiniz *(order_pd ve order_price)* ve ortak kök bilgiyle katılın arası *(sayı, tarih ve şehir)* :

| orderNumber | orderDate | order_pd | order_price | city |
| --- | --- | --- | --- | --- |
| 01 | 20170122 | P1 | 23 | Seattle |
| 01 | 20170122 | P2 | 13 | Seattle |
| 01 | 20170122 | P3 | 231 | Seattle |

Şema eşleme kuralının aşağıdaki kopyalama etkinliği JSON örneği yapılandırın:

```json
{
    "name": "CopyFromMongoDBToOracle",
    "type": "Copy",
    "typeProperties": {
        "source": {
            "type": "MongoDbV2Source"
        },
        "sink": {
            "type": "OracleSink"
        },
        "translator": {
            "type": "TabularTranslator",
            "schemaMapping": {
                "orderNumber": "$.number",
                "orderDate": "$.date",
                "order_pd": "prod",
                "order_price": "price",
                "city": " $.city[0].name"
            },
            "collectionReference":  "$.orders"
        }
    }
}
```

## <a name="data-type-mapping"></a>Veri türü eşlemesi

Kopyalama etkinliği kaynak türleri için aşağıdaki 2 adımlı yaklaşım ile eşleme türleri havuz gerçekleştirir:

1. Azure veri fabrikası geçici veri türleri için yerel kaynak türlerinden dönüştürme
2. Azure Data Factory geçici veri türlerinden yerel havuz türüne dönüştürün

Her bir bağlayıcı konuda "Veri eşleme türü" bölümündeki geçiş türü için yerel bir tür arasındaki eşlemeyi bulabilirsiniz.

### <a name="supported-data-types"></a>Desteklenen veri türleri

Data Factory, aşağıdaki geçici veri türlerini destekler: Tür bilgilerini yapılandırırken değerleri belirtebilirsiniz [dataset yapısını](concepts-datasets-linked-services.md#dataset-structure-or-schema) yapılandırma:

* Byte[]
* Boolean
* Datetime
* Datetimeoffset
* Decimal
* Double
* Guid
* Int16
* Int32
* Int64
* Single
* String
* Timespan

## <a name="next-steps"></a>Sonraki adımlar
Bir kopyalama etkinliği makalelere bakın:

- [Kopyalama etkinliği'ne genel bakış](copy-activity-overview.md)
