---
title: Şema eşleme kopyalama etkinliğindeki | Microsoft Docs
description: Kopyalama etkinliği Azure Data factory'de veri veri kopyalarken havuz şemaları ve veri türleri kaynak verilerden nasıl eşlendiğini hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/20/2018
ms.author: jingwang
ms.openlocfilehash: 99798b35419ec9574c99aaba42803fbeeb1555f1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60615635"
---
# <a name="schema-mapping-in-copy-activity"></a>Kopyalama etkinliğinde şema eşleme
Bu makalede Azure Data Factory kopyalama etkinliği, şema eşleme ve veri türü eşlemesi veri kaynağı verilerden nasıl yaptığını açıklar, veri kopyalama yürütün.

## <a name="column-mapping"></a>Sütun eşleme

Sütun eşlemesi şeklinde tablosal veri arasında veri kopyalama işlemi sırasında uygulanır. Varsayılan olarak, kopyalama etkinliği **sütun adlarına göre havuz için kaynak verileri eşleme**sürece [açık sütun eşlemesi](#explicit-column-mapping) yapılandırılır. Kopyalama etkinliği daha açık belirtmek gerekirse:

1. Veri kaynağından okumak ve kaynağı şemasını belirleme

    * Veri deposu/dosya biçimi, örneğin, veritabanları/dosya meta veri (Avro/ORC/Parquet/metin üst bilgisiyle), önceden tanımlı bir şema ile veri kaynakları için sorgu sonucunu veya dosya meta verilerinden kaynak şema ayıklanır.
    * Örneğin, Azure tablo/Cosmos DB, esnek şema ile veri kaynakları için kaynak şema sorgu sonuç algılanır. Veri kümesinde "yapı" yapılandırarak üzerine yazabilirsiniz.
    * Üst bilgi içermeyen metin dosyası için varsayılan sütun adları deseni "Prop_0", "Prop_1" oluşturulan... Veri kümesinde "yapı" yapılandırarak üzerine yazabilirsiniz.
    * Dynamics kaynağı için veri kümesi "yapı" bölümünde şema bilgileri sağlamanız gerekir.

2. Açık sütun eşleme, belirtilmişse uygulayın.

3. Havuz veri yazma

    * Önceden tanımlı bir şema ile veri depoları için sütunları aynı ada sahip veri yazılır.
    * Sabit olmayan veri depoları ve dosya biçimleri için sütun adları/meta veri kaynağı şemasını temel alan oluşturulur.

### <a name="explicit-column-mapping"></a>Açık sütun eşleme

Belirtebileceğiniz **Bunun amacı** içinde **typeProperties** bölümünü açık sütun eşleme için kopyalama etkinliği. Bu senaryoda, "yapı" bölümü, girdi ve çıktı veri kümeleri için gereklidir. Sütun eşleme destekler **eşleme tüm veya kaynak veri kümesindeki tüm sütunları havuz veri kümesi "yapı" içinde "yapısına" sütun alt kümesi**. Bir özel durumu hata koşulları şunlardır:

* Sorgu sonucu giriş veri kümesi "yapı" bölümünde belirtilen bir sütun adı yok. kaynak veri deposu.
* Havuz veri deposu (ile önceden tanımlı bir şeması varsa) çıkış veri kümesi "yapı" bölümünde belirtilen bir sütun adı yok.
* Daha az sütun veya daha fazla sütun "eşlemesinde belirtilen yapısını" havuz veri kümesi.
* Yinelenen eşleme.

#### <a name="explicit-column-mapping-example"></a>Açık sütun eşleme örneği

Bu örnekte, Giriş tablosunda bir yapıya sahiptir ve bir şirket içi SQL veritabanındaki bir tabloda işaret.

```json
{
    "name": "SqlServerInput",
    "properties": {
        "structure":
         [
            { "name": "UserId"},
            { "name": "Name"},
            { "name": "Group"}
         ],
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "SqlServerLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "SourceTable"
        }
    }
}
```

Bu örnekte, çıktı tablosu bir yapıya sahiptir ve Azure SQL veritabanı tablosuna işaret.

```json
{
    "name": "AzureSqlOutput",
    "properties": {
        "structure":
        [
            { "name": "MyUserId"},
            { "name": "MyName" },
            { "name": "MyGroup"}
        ],
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "AzureSqlLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "SinkTable"
        }
    }
}
```

Aşağıdaki JSON bir işlem hattında kopyalama etkinliği tanımlar. Kaynak havuzu sütuna eşlenmiş sütunlarından (**Bunun amacı**) kullanarak **translator** özelliği.

```json
{
    "name": "CopyActivity",
    "type": "Copy",
    "inputs": [
        {
            "referenceName": "SqlServerInput",
            "type": "DatasetReference"
        }
    ],
    "outputs": [
        {
            "referenceName": "AzureSqlOutput",
            "type": "DatasetReference"
        }
    ],
    "typeProperties":    {
        "source": { "type": "SqlSource" },
        "sink": { "type": "SqlSink" },
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

**Sütun eşlemesi akış:**

![Sütun eşleme akış](./media/copy-activity-schema-and-type-mapping/column-mapping-sample.png)

## <a name="schema-mapping"></a>Şema eşleme

Şema eşleme hiyerarşik biçimli verileri ve tablo şeklinde verileri, örneğin metin dosyası için MongoDB/REST Kopyala ve SQL bir kopyasından Azure Cosmos DB'nin MongoDB API'si için arasında veri kopyalama işlemi sırasında uygulanır. Kopyalama etkinliği aşağıdaki özellikler desteklenir `translator` bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği Çeviricisi öğesinin type özelliği ayarlanmalıdır: **TabularTranslator** | Evet |
| schemaMapping | Eşleme ilişki temsil eden anahtar-değer çiftleri koleksiyonu **yan havuz için kaynak taraftan**.<br/>- **Anahtar:** kaynak temsil eder. İçin **tablo kaynağı**, tanımlanan veri kümesi yapısı için; sütun adı belirtin **hiyerarşik kaynak**, ayıklayın ve eşlemek her bir alan için JSON yolu ifadesini belirtin.<br/>- **Değer:** havuz temsil eder. İçin **tablo havuz**, tanımlanan veri kümesi yapısı için; sütun adı belirtin **hiyerarşik havuz**, ayıklayın ve eşlemek her bir alan için JSON yolu ifadesini belirtin. <br/> Kök nesne altındaki alanlar için hiyerarşik veriler söz konusu olduğunda JSON yolu root $ ile başlar; tarafından seçilen dizinin içindeki alanlar için `collectionReference` özelliği, JSON yolu dizi öğeden başlar.  | Evet |
| collectionReference | Yineleme ve veri nesneleri ayıklamak istiyorsanız **bir dizi alanındaki** nesne başına daha fazla satır arası uygulamak için o dizinin JSON yolunu belirtmek için dönüştürme ve aynı deseni. Bu özellik yalnızca hiyerarşik veri kaynağı olduğunda desteklenir. | Hayır |

**Örnek: SQL Mongodb'den kopyalayın:**

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
    "name": "CopyFromMongoDBToSqlAzure",
    "type": "Copy",
    "typeProperties": {
        "source": {
            "type": "MongoDbV2Source"
        },
        "sink": {
            "type": "SqlSink"
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

Data Factory, aşağıdaki geçici veri türlerini destekler: Tür bilgilerini yapılandırırken değerleri belirtebilirsiniz [dataset yapısını](concepts-datasets-linked-services.md#dataset-structure) yapılandırma:

* Byte[]
* Boolean
* DateTime
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

### <a name="explicit-data-type-conversion"></a>Açık veri türü dönüşümü

Aynı sütunda farklı kaynak ve havuz sahip olduğunda sabit şema ile Örneğin, SQL Server/Oracle, veri kopyalama veri depolarıyla çalışırken açık tür dönüştürme kaynak tarafı içinde bildirilmesi gerekir:

* Dosya kaynağı için örneğin, CSV/Avro tür dönüştürme kaynak yapısı (kaynak tarafında sütun adı ve havuz yan türü) tam sütun listesiyle aracılığıyla bildirilmesi
* İlişkisel bir kaynak için (örneğin, SQL/Oracle), tür dönüştürme açık tür dönüştürme sorgu deyimi tarafından elde.

## <a name="when-to-specify-dataset-structure"></a>Veri kümesi "yapısı" belirtmek ne zaman

İçinde senaryoları, "yapı" veri kümesindeki gereklidir:

* Uygulama [açık veri türü dönüştürme](#explicit-data-type-conversion) (girdi veri kümesi) kopyalama sırasında dosya kaynakları için
* Uygulama [açık sütun eşlemesi](#explicit-column-mapping) (her ikisi de girdi ve çıktı veri kümesi) kopyalama sırasında
* Dynamics 365/CRM kaynağından kopyalama (giriş veri kümesi)
* Kaynak JSON dosyası olmadığında içi içe nesne olarak Cosmos DB'ye kopyalama (çıkış veri kümesi)

Senaryolarda, veri kümesi "yapı" önerilen:

* (Giriş veri kümesi) üst bilgisi olmayan metin dosyasından kopyalama. Açık sütun eşlemesi yapılandırmasını kaydetmek için karşılık gelen havuz sütunları içeren, hizalama metin dosyası sütun adlarını belirtebilirsiniz.
* Veri kopyalama ile esnek şemaya, örneğin, Azure tablo/Cosmos DB (girdi veri kümesi) depolar, etkinlik üzerinden let kopyalama yerine kopyalanan beklenen verileri (sütun) garanti etmek için üst satır her bir etkinlik çalıştırması sırasında dayalı şema çıkarsa.


## <a name="next-steps"></a>Sonraki adımlar
Bir kopyalama etkinliği makalelere bakın:

- [Kopyalama etkinliği'ne genel bakış](copy-activity-overview.md)
- [Kopyalama etkinliği hataya dayanıklılık](copy-activity-fault-tolerance.md)
- [Kopyalama etkinliği performansı](copy-activity-performance.md)
