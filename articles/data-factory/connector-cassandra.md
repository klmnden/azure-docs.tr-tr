---
title: Azure Data Factory kullanarak Cassandra veri kopyalama | Microsoft Docs
description: Desteklenen havuz veri depolarına Cassandra bir Azure Data Factory ardışık düzeninde kopyalama etkinliği kullanarak verileri kopyalamak öğrenin.
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
ms.date: 06/07/2018
ms.author: jingwang
ms.openlocfilehash: 94312edaa97a5d9a7502eed4c0551151ce2a06cc
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35235286"
---
# <a name="copy-data-from-cassandra-using-azure-data-factory"></a>Azure Data Factory kullanarak Cassandra verilerini
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-onprem-cassandra-connector.md)
> * [Sürüm 2 - Önizleme](connector-cassandra.md)

Bu makalede kopya etkinliği Azure Data Factory'de Cassandra veritabanından veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.


> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 Cassandra Bağlayıcısı](v1/data-factory-onprem-cassandra-connector.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna Cassandra veritabanından veri kopyalayabilirsiniz. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Cassandra bağlayıcı destekler:

- Cassandra **sürümleri 2.x ve 3.x**.
- Verileri kullanarak kopyalama **temel** veya **anonim** kimlik doğrulaması.

>[!NOTE]
>Self-hosted tümleştirme çalışma zamanı, 3.x IR sürüm 3.7'den itibaren ve üstünde desteklenir Cassandra çalışan etkinliği.

## <a name="prerequisites"></a>Önkoşullar

Genel olarak erişilebilir değil Cassandra veritabanından veri kopyalamak için bir Self-hosted tümleştirmesi çalışma zamanı ayarlamanız gerekir. Bkz: [Self-hosted tümleştirmesi çalışma zamanı](create-self-hosted-integration-runtime.md) daha ayrıntılı bilgi için makalenin. Yerleşik bir Cassandra sürücü tümleştirmesi çalışma zamanı sağlar, bu nedenle herhangi bir sürücüsü başlangıç/bitiş Cassandra veri kopyalama işlemi sırasında el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, belirli Data Factory varlıklarını Cassandra bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler Cassandra bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Type özelliği ayarlanmalıdır: **Cassandra** |Evet |
| konak |Bir veya daha fazla IP adresleri veya ana bilgisayar adlarını Cassandra sunucuları.<br/>IP adreslerini veya aynı anda tüm sunucularına bağlanmak için ana bilgisayar adlarını virgülle ayrılmış listesini belirtin. |Evet |
| port |İstemci bağlantılarını dinlemek için Cassandra sunucusunun kullandığı TCP bağlantı noktası. |Hayır (varsayılan olarak 9042) |
| authenticationType | Cassandra veritabanına bağlanmak için kullanılan kimlik doğrulama türü.<br/>İzin verilen değerler: **temel**, ve **anonim**. |Evet |
| kullanıcı adı |Kullanıcı hesabının kullanıcı adını belirtin. |Evet, authenticationType temel olarak ayarlanmışsa. |
| password |Kullanıcı hesabı için parola belirtin. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). |Evet, authenticationType temel olarak ayarlanmışsa. |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu genel olarak erişilebilir ise) Self-hosted tümleştirmesi çalışma zamanı veya Azure tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. |Hayır |

>[!NOTE]
>Şu anda Cassandra SSL kullanarak bağlantı desteklenmiyor.

**Örnek:**

```json
{
    "name": "CassandraLinkedService",
    "properties": {
        "type": "Cassandra",
        "typeProperties": {
            "host": "<host>",
            "authenticationType": "Basic",
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için veri kümeleri makalesine bakın. Bu bölümde Cassandra veri kümesi tarafından desteklenen özellikler listesini sağlar.

Cassandra verileri kopyalamak için kümesine tür özelliği ayarlamak **CassandraTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak: **CassandraTable** | Evet |
| keyspace |Keyspace veya Cassandra veritabanındaki şema adı. |("CassandraSource" için "sorgu" belirtilmişse) yok |
| tableName |Cassandra veritabanı tablosunun adı. |("CassandraSource" için "sorgu" belirtilmişse) yok |

**Örnek:**

```json
{
    "name": "CassandraDataset",
    "properties": {
        "type": "CassandraTable",
        "linkedServiceName": {
            "referenceName": "<Cassandra linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "keySpace": "<keyspace name>",
            "tableName": "<table name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri


Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde Cassandra kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="cassandra-as-source"></a>Kaynak olarak Cassandra

Cassandra verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **CassandraSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **CassandraSource** | Evet |
| sorgu |Verileri okumak için özel sorgu kullanın. |SQL-92 sorgusu veya CQL sorgusu. Bkz: [CQL başvuru](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>SQL sorgusu kullanırken belirtin **keyspace name.table adı** sorgulamak istediğiniz tabloyu temsil etmek için. |("TableName" ve "keyspace" kümesindeki belirtilirse) yok. |
| consistencyLevel |Tutarlılık düzeyi kaç çoğaltmaları Okuma isteği için veri istemci uygulamasına geri dönmeden önce yanıt vermesi gereken belirtir. Cassandra Okuma isteği karşılamak veriler için çoğaltmaları belirtilen sayısını denetler. Bkz: [veri tutarlılığını yapılandırma](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) Ayrıntılar için.<br/><br/>İzin verilen değerler: **bir**, **iki**, **üç**, **çekirdek**, **tüm**, **LOCAL_ Çekirdek**, **EACH_QUORUM**, ve **LOCAL_ONE**. |Hayır (varsayılan değer `ONE`) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromCassandra",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Cassandra input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "CassandraSource",
                "query": "select id, firstname, lastname from mykeyspace.mytable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-cassandra"></a>Eşleme Cassandra için veri türü

Cassandra veri kopyalama işlemi sırasında aşağıdaki eşlemelerini Cassandra veri türlerinden Azure Data Factory geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) nasıl kopyalama etkinliği kaynak şema ve veri türü için havuz eşlemeleri hakkında bilgi edinmek için.

| Cassandra veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| ASCII |Dize |
| BIGINT |Int64 |
| BLOB |Byte] |
| BOOLE DEĞERİ |Boole |
| ONDALIK |Ondalık |
| ÇİFT |Çift |
| KAYAN NOKTA |Tek |
| INET |Dize |
| INT |Int32 |
| METİN |Dize |
| ZAMAN DAMGASI |DateTime |
| TIMEUUID |Guid |
| UUID |Guid |
| VARCHAR |Dize |
| VARINT |Ondalık |

> [!NOTE]
> Türleri (harita, kümesi, liste, vb.), başvurmak için koleksiyonu [iş Cassandra koleksiyon türlerini sanal tablosunu kullanarak](#work-with-collections-using-virtual-table) bölümü.
>
> Kullanıcı tanımlı türler desteklenmez.
>
> İkili sütun ve dize sütunu uzunluklarını uzunluğu 4000'den büyük olamaz.
>

## <a name="work-with-collections-using-virtual-table"></a>Sanal tablosunu kullanarak koleksiyonları ile çalışma

Azure Data Factory bağlanmak ve Cassandra veritabanınızdan verileri kopyalamak için yerleşik bir ODBC sürücüsü kullanır. Harita, kümesi ve listesi dahil olmak üzere koleksiyon türü için karşılık gelen sanal tablolara veri sürücüsü renormalizes. Özellikle, bir tablo tüm koleksiyon sütunları içeriyorsa, sürücü aşağıdaki sanal tablolar oluşturur:

* A **temel tablo**, koleksiyon sütunları hariç gerçek tablosu olarak aynı verileri içerir. Temel tablo aynı adını temsil ettiği gerçek tablo olarak kullanır.
* A **sanal tablo** her koleksiyon sütun için hangi genişletir iç içe veri. Koleksiyonları temsil eden sanal tablolar ayırıcı gerçek tablosunun adı kullanılarak adlandırılmış "*vt*" ve sütunun adı.

Sanal tablolar Normalleştirilmemiş verilere erişmek sürücüyü etkinleştirme gerçek tablodaki verileri bakın. Ayrıntılar için örnek bölümüne bakın. Sorgulamak ve sanal tabloları birleştirme Cassandra koleksiyonların içeriğe erişebilir.

### <a name="example"></a>Örnek

Örneğin, aşağıdaki "ExampleTable", "pk_int", değer adlı bir text sütunu, bir liste sütunu, sütun eşleme ve ("StringSet" olarak adlandırılır) bir sütunu Ayarla adlı bir tamsayı birincil anahtar sütunu içeren bir Cassandra veritabanı tablosu verilmiştir.

| pk_int | Değer | Liste | Eşleme | StringSet |
| --- | --- | --- | --- | --- |
| 1 |"örnek değeri 1" |["1", "2", "3"] |{"S1": "a", "S2": "b"} |{"A", "B", "C"} |
| 3 |"örnek value 3" |["100", "101", "102", "105"] |{"S1": "t"} |{"A", "E"} |

Sürücü bu tek tablo göstermek için birden çok sanal tablo oluşturur. Sanal tablolardaki yabancı anahtar sütunları gerçek tablosundaki birincil anahtar sütunlarının başvuru ve sanal tablo satırı karşılık gelen gerçek tablo satırı belirtmenize.

İlk sanal tablonun "ExampleTable" adlı temel tablo aşağıdaki tabloda gösterilen şöyledir: 

| pk_int | Değer |
| --- | --- |
| 1 |"örnek değeri 1" |
| 3 |"örnek value 3" |

Temel tablo özgün veritabanı tablosunun bu tablodan atlanmış ve diğer sanal tablolarda genişletilmiş koleksiyonları dışında aynı verileri içerir.

Aşağıdaki tablolarda listesi, eşleme ve StringSet sütunları verilerden renormalize sanal tablolar gösterilmektedir. Yalnızca "_index" veya "_anahtar" ile biten adlarını sütunlarla özgün listesi veya eşlemesi içinde veri konumunu belirtin. Yalnızca "_value" ile biten adlarını sütunlarla koleksiyondan genişletilmiş verileri içerir.

**Tablo "ExampleTable_vt_List":**

| pk_int | List_index | List_value |
| --- | --- | --- |
| 1 |0 |1 |
| 1 |1 |2 |
| 1 |2 |3 |
| 3 |0 |100 |
| 3 |1 |101 |
| 3 |2 |102 |
| 3 |3 |103 |

**Tablo "ExampleTable_vt_Map":**

| pk_int | Map_key | Map_value |
| --- | --- | --- |
| 1 |S1 |A |
| 1 |S2 |b |
| 3 |S1 |T |

**Tablo "ExampleTable_vt_StringSet":**

| pk_int | StringSet_value |
| --- | --- |
| 1 |A |
| 1 |B |
| 1 |C |
| 3 |A |
| 3 |E |

## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
