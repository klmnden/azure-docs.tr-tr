---
title: Azure Data Factory ile Cassandra ' veri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına Cassandra bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/07/2018
ms.author: jingwang
ms.openlocfilehash: 743dad6032547f8f535543413adff416efb56ac0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60640098"
---
# <a name="copy-data-from-cassandra-using-azure-data-factory"></a>Cassanra'dan Azure Data Factory kullanarak veri kopyalama
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](v1/data-factory-onprem-cassandra-connector.md)
> * [Geçerli sürüm](connector-cassandra.md)

Bu makalede, kopyalama etkinliği Azure Data Factory'de bir Cassandra veritabanına veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Cassandra veritabanı'ndan veri her desteklenen havuz veri deposuna kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Cassandra bağlayıcı'yı destekler:

- Cassandra **sürümleri 2.x ve 3.x**.
- Kullanarak verileri kopyalama **temel** veya **anonim** kimlik doğrulaması.

>[!NOTE]
>Şirket içinde barındırılan tümleştirme çalışma zamanı, Cassandra 3.x IR sürüm 3.7'den itibaren ve üstünde desteklenir üzerinde çalışan etkinlik için.

## <a name="prerequisites"></a>Önkoşullar

Genel olarak erişilebilir değil bir Cassandra veritabanından veri kopyalamak için şirket içinde barındırılan tümleştirme çalışma zamanını oluşturan gerekir. Bkz: [şirket içinde barındırılan tümleştirme çalışma zamanı](create-self-hosted-integration-runtime.md) daha fazla bilgi edinmek için makaleyi. Tümleştirme çalışma zamanı yerleşik bir Cassandra sürücü sağlar, bu nedenle herhangi bir sürücü / için Cassandra veri kopyalarken el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli Cassandra bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Cassandra bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Type özelliği ayarlanmalıdır: **Cassandra** |Evet |
| host |Bir veya daha fazla IP adresleri veya Cassandra sunucusunun ana bilgisayar adını.<br/>IP adreslerini veya aynı anda tüm sunuculara bağlanmak için ana bilgisayar adlarını virgülle ayrılmış listesini belirtin. |Evet |
| port |Cassandra sunucusunun istemci bağlantıları için dinlemek üzere kullandığı TCP bağlantı noktası. |Hayır (varsayılan değer 9042) |
| authenticationType | Cassandra veritabanına bağlanmak için kullanılan kimlik doğrulaması türü.<br/>İzin verilen değerler şunlardır: **Temel**, ve **anonim**. |Evet |
| username |Kullanıcı hesabının kullanıcı adını belirtin. |Evet, authenticationType temel olarak ayarlanmışsa. |
| password |Kullanıcı hesabı için parola belirtin. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Evet, authenticationType temel olarak ayarlanmışsa. |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz genel olarak erişilebilir değilse), şirket içinde barındırılan tümleştirme çalışma zamanı veya Azure Integration Runtime kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

>[!NOTE]
>Cassandra SSL kullanarak bağlantı şu anda desteklenmiyor.

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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için veri kümeleri makalesine bakın. Bu bölümde, Cassandra veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Cassandra verileri kopyalamak için dataset öğesinin type özelliği ayarlamak **CassandraTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **CassandraTable** | Evet |
| keySpace |Anahtar alanı veya Cassandra veritabanındaki şema adı. |Hayır ("CassandraSource" için "sorgu" belirtilmişse) |
| tableName |Cassandra veritabanındaki tablonun adı. |Hayır ("CassandraSource" için "sorgu" belirtilmişse) |

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


Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Cassandra kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="cassandra-as-source"></a>Kaynak olarak Cassandra

Cassandra verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **CassandraSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **CassandraSource** | Evet |
| sorgu |Verileri okumak için özel sorgu kullanın. 92 SQL sorgusu veya CQL sorgusu. Bkz: [CQL başvuru](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>SQL sorgu kullanarak belirtmeniz **anahtar alanı name.table adı** sorgulamak istediğiniz tablosunu temsil edecek. |("TableName" ve "anahtar" alanı kümesindeki belirtilirse) yok. |
| consistencyLevel |Tutarlılık düzeyi, istemci uygulamasına veri döndürmeden önce kaç çoğaltmalar için Okuma isteği yanıtlamalıdır belirtir. Cassandra Okuma isteği karşılamak veriler için çoğaltmaları belirtilen sayısını denetler. Bkz: [veri tutarlılığını yapılandırma](https://docs.datastax.com/en/cassandra/2.1/cassandra/dml/dml_config_consistency_c.html) Ayrıntılar için.<br/><br/>İzin verilen değerler şunlardır: **BİR**, **iki**, **üç**, **çekirdek**, **tüm**, **LOCAL_QUORUM**, **EACH_QUORUM**, ve **LOCAL_ONE**. |Hayır (varsayılan değer `ONE`) |

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

Cassandra veri kopyalama işlemi sırasında aşağıdaki eşlemeler Cassandra veri türlerinden Azure veri fabrikası geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) eşlemelerini nasıl yapar? kopyalama etkinliği kaynak şema ve veri türü için havuz hakkında bilgi edinmek için.

| Cassandra veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| ASCII |String |
| BIGINT |Int64 |
| BLOB |Byte[] |
| BOOLEAN |Boolean |
| DECIMAL |Decimal |
| DOUBLE |Double |
| FLOAT |Single |
| INET |String |
| INT |Int32 |
| TEXT |String |
| TIMESTAMP |DateTime |
| TIMEUUID |Guid |
| UUID |Guid |
| VARCHAR |String |
| VARINT |Decimal |

> [!NOTE]
> Türler (harita, set, list, vb.), başvurmak için koleksiyon [iş Cassandra koleksiyon türlerini kullanarak sanal bir tablo](#work-with-collections-using-virtual-table) bölümü.
>
> Kullanıcı tanımlı türler desteklenmez.
>
> İkili bir sütunu ve dize sütunu uzunlukları uzunluğu 4000 ' büyük olamaz.
>

## <a name="work-with-collections-using-virtual-table"></a>Sanal tablosunu kullanarak collections ile çalışma

Azure Data Factory, bağlanma ve Cassandra veritabanınızdan veri kopyalamak için yerleşik bir ODBC sürücüsünü kullanır. Harita, küme ve liste gibi koleksiyon türleri için karşılık gelen sanal tablolarına veri sürücü renormalizes. Özellikle, bir tablo koleksiyonu sütunlar içeriyorsa, sürücü aşağıdaki sanal tablolar oluşturur:

* A **temel tablo**, aynı verileri toplama sütunları hariç gerçek tablosu içerir. Temel tablo adıyla temsil ettiği gerçek tablosu olarak kullanır.
* A **sanal tablo** her toplama sütunu için genişleyen iç içe geçmiş verileri. Gerçek bir ayırıcı tablonun adını kullanarak koleksiyonları temsil eden sanal tablolar adlı "*vt*" sütun adı.

Sanal tablolar normalleştirilmişlikten çıkarılmış verilere erişmek sürücüyü etkinleştirme gerçek tablodaki verileri bakın. Ayrıntılar için örnek bölümüne bakın. Cassandra koleksiyonları içeriğini sorgulama ve sanal tabloları birleştirme erişebilirsiniz.

### <a name="example"></a>Örnek

Örneğin, aşağıdaki "ExampleTable", "pk_int", değer adlı bir metin sütunu, bir liste sütunu, sütun eşleme ve ("StringSet" adlı) bir kümesi sütunu adlı bir tamsayı birincil anahtar sütunu içeren bir Cassandra veritabanı tablosu olur.

| pk_int | Değer | Liste | Eşleme | StringSet |
| --- | --- | --- | --- | --- |
| 1 |"örnek değeri 1" |["1", "2", "3"] |{"S1": "a", "S2": "b"} |{"A", "B", "C"} |
| 3 |"örnek value 3" |["100", "101", "102", "105"] |{"S1": "t"} |{"A", "E"} |

Sürücü bu tek tabloda temsil etmek için birden çok sanal tablolar oluşturur. Sanal tablolarındaki yabancı anahtar sütunları gerçek tablosundaki birincil anahtar sütunlarını başvuru ve sanal tablo satırı karşılık gelen gerçek tablosu satırı belirtmenize.

İlk sanal tablo "ExampleTable" adlı temel tablo aşağıdaki tabloda gösterilen şöyledir: 

| pk_int | Değer |
| --- | --- |
| 1 |"örnek değeri 1" |
| 3 |"örnek value 3" |

Temel tablo, bu tablodan atlanmış ve diğer sanal tablolarında genişletilmiş koleksiyonları dışında özgün veritabanı tablosunu aynı verileri içerir.

Aşağıdaki tablolar, liste ve eşleme StringSet sütundaki verileri normalleştirebilir sanal tabloları gösterir. "_Index" veya "_anahtarı" ile biten sütunları verilerin özgün liste veya harita içinde konumunu belirtin. Koleksiyon genişletilmiş verileri "_Değeri" ile biten sütunları içerir.

**Tablo "ExampleTable_vt_List":**

| pk_int | List_index | List_value |
| --- | --- | --- |
| 1 |0 |1\. |
| 1\. |1\. |2 |
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
| 3 |S1 |t |

**Tablo "ExampleTable_vt_StringSet":**

| pk_int | StringSet_value |
| --- | --- |
| 1 |A |
| 1 |B |
| 1 |C |
| 3 |A |
| 3 |E |

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
