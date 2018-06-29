---
title: Şema eşleme kopyalama etkinliğinde | Microsoft Docs
description: Kopya etkinliği Azure Data factory'de veri kopyalama işlemi sırasında veri havuzu için şemaları ve verileri türlerinden kaynak verileri nasıl eşlendiğini hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: jingwang
ms.openlocfilehash: 338df0e258f66b6639e59a4fe31b6cfb6c283dd3
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37045536"
---
# <a name="schema-mapping-in-copy-activity"></a>Şema eşleme kopyalama etkinliğinde
Şema eşleme ve veri türü eşlemesi veri kaynağı verilerden Azure Data Factory kopyalama etkinliği nasıl yapar bu makalede veri kopyalamayı yapılırken.

## <a name="column-mapping"></a>Sütun eşlemesi

Varsayılan olarak, kopya etkinliği **eşleme sütun adlarına göre havuz için kaynak verilerini**sürece [açık sütun eşlemesi](#explicit-column-mapping) yapılandırılır. Kopyalama etkinliği daha açık belirtmek gerekirse:

1. Veri kaynağından okumak ve kaynak şema belirleme

    * Veri deposu/dosya biçimi, örneğin, veritabanları/dosyalarıyla, meta veriler (Avro/ORC/Parquet/metin üstbilgiyle), önceden tanımlanmış şemasıyla veri kaynakları için kaynak şema sorgu sonucu veya dosya meta verilerden ayıklanır.
    * Esnek şeması, örneğin, Azure tablo/Cosmos DB veri kaynakları için kaynak şema sorgusu sonucundan algılanır. Veri kümesi "yapı" sağlayarak üzerine yazabilirsiniz.
    * Üst bilgi içermeyen metin dosyası için varsayılan sütun adları desenle "Prop_0", "Prop_1" üretilir... Veri kümesi "yapı" sağlayarak üzerine yazabilirsiniz.
    * Dynamics kaynağı için veri kümesi "yapısı" bölümünde şema bilgileri girmeniz gerekir.

2. Açık sütun eşlemesi belirtilmişse uygulayın.

3. Havuz için veri yazma

    * Önceden tanımlı bir şeması ile veri depoları için aynı ada sahip sütun için veri yazılır.
    * Veri depoları sabit şemasına olmadan ve dosya biçimleri için sütun adları/meta veri kaynağının şemasını temel alan oluşturulur.

### <a name="explicit-column-mapping"></a>Açık sütun eşlemesi

Belirleyebileceğiniz **columnMappings** içinde **typeProperties** açık sütun eşlemesi yapmak için kopyalama etkinliği bölümü. Bu senaryoda, "yapısı" bölümü, giriş ve çıkış veri kümeleri için gereklidir. Sütun eşlemesi destekler **eşleme tüm veya "yapısı" havuz kümesindeki tüm sütunlara "yapısı" kaynak veri kümesinde sütun alt kümesini**. Bir özel durum neden hata koşulları şunlardır:

* Sorgu sonucu girdi veri kümesi "yapısı" bölümünde belirtilen bir sütun adı yok kaynak veri deposu.
* Havuz veri deposu (şemasıyla önceden tanımlanmış değilse) çıkış veri kümesi "yapısı" bölümünde belirtilen bir sütun adı yok.
* Daha az sütun veya "eşlemede belirtilen yapısını" havuz veri kümesi'den daha fazla sütun.
* Yinelenen eşleme.

#### <a name="explicit-column-mapping-example"></a>Açık sütun eşleme örneği

Bu örnekte, Giriş tablosunda bir yapı ve bir şirket içi SQL veritabanındaki bir tablo işaret.

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

Bu örnek, çıktı tablosu yapısının ve bir Azure SQL veritabanındaki bir tablo işaret.

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

Aşağıdaki JSON ardışık düzeninde kopyalama etkinliği tanımlar. Kaynak havuzu sütuna eşlenen sütunlarından (**columnMappings**) kullanarak **Çeviricisi** özelliği.

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

Söz dizimi kullanıyorsanız `"columnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"` sütun eşlemesi belirtmek için hala olarak desteklenmektedir-değil.

**Sütun eşlemesi akışı:**

![Sütun eşlemesi akışı](./media/copy-activity-schema-and-type-mapping/column-mapping-sample.png)

## <a name="data-type-mapping"></a>Veri türü eşlemesi

Kopya etkinliği aşağıdaki 2 adımlı yaklaşımda eşleme türler havuz için kaynak türleri gerçekleştirir:

1. Azure Data Factory geçici veri türleri için yerel kaynak türlerinden Dönüştür
2. Azure Data Factory geçici veri türlerinden yerel havuz türüne dönüştürün

Her bağlayıcı konu "Veri eşleme türü" bölümünde geçiş türü için yerel tür arasında eşleme bulabilirsiniz.

### <a name="supported-data-types"></a>Desteklenen veri türleri

Veri fabrikası aşağıdaki geçici veri türlerini destekler: tür bilgileri sağlanırken altındaki değerler belirtebilirsiniz [veri kümesi yapısı](concepts-datasets-linked-services.md#dataset-structure) yapılandırma:

* Byte]
* Boole
* Tarih saat
* Datetimeoffset
* Ondalık
* çift
* Guid
* Int16
* Int32
* Int64
* Tek
* Dize
* Timespan

### <a name="explicit-data-type-conversion"></a>Açık veri türü dönüşümü

Kaynak ve havuz olduğunda farklı türü aynı sütunda, verileri veri kopyalama sabit şemasıyla, örneğin, SQL Server/Oracle, depoladığında kaynak tarafta açık tür dönüşümü bildirilmesi gerekir:

* Dosya kaynağını, örneğin, CSV/Avro, tür dönüştürme kaynak yapısı (kaynak tarafı sütun adı ve havuz yan türü) tam sütun listesiyle aracılığıyla bildirilmesi
* (Örneğin, SQL/Oracle) ilişkisel kaynağı için tür dönüştürme sorgu deyimi içinde açık tür atama tarafından elde.

## <a name="when-to-specify-dataset-structure"></a>Veri kümesi "yapısı" belirtmek ne zaman

İçinde senaryolar "yapısı" kümesindeki gereklidir:

* Uygulama [açık veri türü dönüştürme](#explicit-data-type-conversion) dosya kaynaklarını kopyalama (girdi veri kümesi) sırasında için
* Uygulama [açık sütun eşlemesi](#explicit-column-mapping) (her ikisi de giriş ve çıkış dataset) kopyalama sırasında
* Dynamics 365 / CRM kaynağından (girdi veri kümesi) kopyalama
* Kaynağı JSON dosyaları (çıktı veri kümesi) olmadığında Cosmos DB iç içe nesne olarak kopyalama

Senaryolar, veri kümesi "yapısında" önerilen:

* Üstbilgi (girdi veri kümesi) olmadan metin dosyasından kopyalama. Metin dosyası sütunlarla açık sütun eşlemesi sağlama kaydetmek için karşılık gelen havuz, hizalama için sütun adlarını belirtebilirsiniz.
* Verileri kopyalama esnek şeması ile Örneğin, Azure tablo/Cosmos DB (girdi veri kümesi) depolar, üzerinden yerine let kopyalama kopyalanmasını beklenen verileri (sütunları) güvence altına almak için etkinlik her etkinlik sırasında üzerinde üst satırlara göre şema olarak Infer.


## <a name="next-steps"></a>Sonraki adımlar
Bir kopyalama etkinliği makalelere bakın:

- [Kopya etkinliği genel bakış](copy-activity-overview.md)
- [Etkinlik hata toleransı kopyalayın](copy-activity-fault-tolerance.md)
- [Kopya etkinliği performansının](copy-activity-performance.md)
