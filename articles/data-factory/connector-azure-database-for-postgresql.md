---
title: Azure Data Factory kullanarak PostgreSQL için Azure veritabanı'ndan veri kopyalama | Microsoft Docs
description: Verileri Azure veritabanı'ndan PostgreSQL için desteklenen bir havuz veri depolarına bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: jingwang
ms.openlocfilehash: c105c64b65d50b9d6bf6477e47d08f415b3a9a31
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54018730"
---
# <a name="copy-data-from-azure-database-for-postgresql-using-azure-data-factory"></a>Azure Data Factory kullanarak PostgreSQL için Azure veritabanı'ndan veri kopyalama 

Bu makalede, kopyalama etkinliği Azure Data Factory'de PostgreSQL için Azure veritabanı'ndan veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna PostgreSQL için Azure veritabanı'ndan veri kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Azure Data Factory bağlantısını etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak herhangi bir sürücü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli PostgreSQL bağlayıcı için Azure veritabanı'na tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Aşağıdaki özellikler için Azure veritabanı, PostgreSQL bağlı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **AzurePostgreSql** | Evet |
| bağlantı dizesi | PostgreSQL için Azure veritabanı'na bağlanmak için bir ODBC bağlantı dizesi. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel ağında bulunuyorsa), Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

Bir bağlantı dizesi olan `Server=<server>.postgres.database.azure.com;Database=<database>;Port=<port>;UID=<username>;Password=<Password>`. Daha fazla özellik durumunuz ayarlayabilirsiniz:

| Özellik | Açıklama | Seçenekler | Gerekli |
|:--- |:--- |:--- |:--- |:--- |
| EncryptionMethod (EM)| Sürücü yöntemi, sürücü ve veritabanı sunucusu arasında gönderilen verileri şifrelemek için kullanır. Örneğin `ValidateServerCertificate=<0/1/6>;`| 0 (şifreleme) **(varsayılan)** / 1 (SSL) / 6 (RequestSSL) | Hayır |
| ValidateServerCertificate (VSC) | Sürücü SSL şifrelemesi etkin olduğunda veritabanı sunucusu tarafından gönderilen sertifikayı doğrulayıp doğrulamadığını belirler (şifreleme yöntemini = 1). Örneğin `ValidateServerCertificate=<0/1>;`| 0 (devre dışı) **(varsayılan)** / 1 (etkin) | Hayır |

**Örnek:**

```json
{
    "name": "AzurePostgreSqlLinkedService",
    "properties": {
        "type": "AzurePostgreSql",
        "typeProperties": {
            "connectionString": {
                 "type": "SecureString",
                 "value": "Server=<server>.postgres.database.azure.com;Database=<database>;Port=<port>;UID=<username>;Password=<Password>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, PostgreSQL veri kümesi için Azure veritabanı tarafından desteklenen özelliklerin bir listesini sağlar.

PostgreSQL için Azure veritabanı'ndan veri kopyalamak için dataset öğesinin type özelliği ayarlamak **AzurePostgreSqlTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **AzurePostgreSqlTable** | Evet |
| tableName | Tablonun adı. | Hayır (etkinlik kaynağı "sorgu" belirtilmişse) |

**Örnek**

```json
{
    "name": "AzurePostgreSqlDataset",
    "properties": {
        "type": "AzurePostgreSqlTable",
        "linkedServiceName": {
            "referenceName": "<AzurePostgreSql linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, PostgreSQL kaynağı için Azure veritabanı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-database-for-postgresql-as-source"></a>Kaynak olarak PostgreSql için Azure veritabanı

PostgreSQL için Azure veritabanı'ndan veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **AzurePostgreSqlSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **AzurePostgreSqlSource** | Evet |
| sorgu | Verileri okumak için özel bir SQL sorgusu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Yok (veri kümesinde "TableName" değeri belirtilmişse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromAzurePostgreSql",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<AzurePostgreSql input dataset name>",
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
                "type": "AzurePostgreSqlSource",
                "query": "<custom query e.g. SELECT * FROM MyTable>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı için veri türü eşlemesi

PostgreSQL için Azure veritabanı'ndan veri kopyalama yapılırken, aşağıdaki eşlemeler PostgreSQL veri türlerinden Azure veri fabrikası geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) eşlemelerini nasıl yapar? kopyalama etkinliği kaynak şema ve veri türü için havuz hakkında bilgi edinmek için.

| PostgreSQL veri türü | PostgresSQL diğer adları | Veri Fabrikası geçici veri türü |
|:--- |:--- |:--- |
| `abstime` |&nbsp; |`String` |
| `bigint` | `int8` | `Int64` |
| `bigserial` | `serial8` | `Int64` |
| `bit [1]` |&nbsp; | `Boolean` |
| `bit [(n)], n>1` |&nbsp; | `Byte[]` |
| `bit varying [(n)]` | `varbit` |`Byte[]` |
| `boolean` | `bool` | `Boolean` |
| `box` |&nbsp; | `String` |
| `bytea` |&nbsp; | `Byte[], String` |
| `character [(n)]` | `char [(n)]` | `String` |
| `character varying [(n)]` | `varchar [(n)]` | `String` |
| `cid` |&nbsp; | `Int32` |
| `cidr` |&nbsp; | `String` |
| `circle` |&nbsp; |` String` |
| `date` |&nbsp; |`Datetime` |
| `daterange` |&nbsp; |`String` |
| `double precision` |`float8` |`Double` |
| `inet` |&nbsp; |`String` |
| `intarray` |&nbsp; |`String` |
| `int4range` |&nbsp; |`String` |
| `int8range` |&nbsp; |`String` |
| `integer` | `int, int4` |`Int32` |
| `interval [fields] [(p)]` | | `String` |
| `json` |&nbsp; | `String` |
| `jsonb` |&nbsp; | `Byte[]` |
| `line` |&nbsp; | `Byte[], String` |
| `lseg` |&nbsp; | `String` |
| `macaddr` |&nbsp; | `String` |
| `money` |&nbsp; | `String` |
| `numeric [(p, s)]`|`decimal [(p, s)]` |`String` |
| `numrange` |&nbsp; |`String` |
| `oid` |&nbsp; |`Int32` |
| `path` |&nbsp; |`String` |
| `pg_lsn` |&nbsp; |`Int64` |
| `point` |&nbsp; |`String` |
| `polygon` |&nbsp; |`String` |
| `real` |`float4` |`Single` |
| `smallint` |`int2` |`Int16` |
| `smallserial` |`serial2` |`Int16` |
| `serial` |`serial4` |`Int32` |
| `text` |&nbsp; |`String` |
| `timewithtimezone` |&nbsp; |`String` |
| `timewithouttimezone` |&nbsp; |`String` |
| `timestampwithtimezone` |&nbsp; |`String` |
| `xid` |&nbsp; |`Int32` |

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
