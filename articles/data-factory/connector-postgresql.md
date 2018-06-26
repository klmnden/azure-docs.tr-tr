---
title: Azure Data Factory kullanarak PostgreSQL gelen veri kopyalama | Microsoft Docs
description: Desteklenen havuz veri depolarına PostgreSQL bir Azure Data Factory ardışık düzeninde kopyalama etkinliği kullanarak verileri kopyalamak öğrenin.
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
ms.date: 06/23/2018
ms.author: jingwang
ms.openlocfilehash: 436725e54e197e021f088e0fd0cd75fc944983a3
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751386"
---
# <a name="copy-data-from-postgresql-by-using-azure-data-factory"></a>Azure Data Factory kullanarak PostgreSQL verileri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-onprem-postgresql-connector.md)
> * [Sürüm 2 - Önizleme](connector-postgresql.md)

Bu makalede kopya etkinliği Azure Data Factory'de bir PostgreSQL veritabanından veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.


> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 PostgreSQL Bağlayıcısı](v1/data-factory-onprem-postgresql-connector.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna PostgreSQL veritabanından veri kopyalayabilirsiniz. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özel olarak, PostgreSQL bu PostgreSQL bağlayıcı destekler **sürüm 7.4 ve yukarıdaki**.

## <a name="prerequisites"></a>Önkoşullar

PostgreSQL veritabanınızı genel olarak erişilebilir durumda değilse, Self-hosted tümleştirmesi çalışma zamanı ayarlamanız gerekir. Kendini barındıran tümleştirme çalışma zamanları hakkında bilgi edinmek için [Self-hosted tümleştirmesi çalışma zamanı](create-self-hosted-integration-runtime.md) makalesi. Tümleştirme çalışma zamanı 3.7 sürümünden başlayarak yerleşik bir PostgreSQL sürücü sağlar, bu nedenle herhangi bir sürücüsü el ile yüklemeniz gerekmez.

Yüklemenize gerek 3.7 düşük Self-Hosted IR sürümü için [PostgreSQL için Ngpsql veri sağlayıcısı](http://go.microsoft.com/fwlink/?linkid=282716) 2.0.12 3.1.9 tümleştirmesi çalışma zamanı makinede arasındaki sürümüyle.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, belirli Data Factory varlıklarını PostgreSQL bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler PostgreSQL bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **PostgreSql** | Evet |
| connectionString | PostgreSQL için Azure veritabanına bağlanmak için bir ODBC bağlantı dizesi. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). | Evet |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu genel olarak erişilebilir ise) Self-hosted tümleştirmesi çalışma zamanı veya Azure tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. |Hayır |

Tipik bağlantı dizesi `Server=<server>;Database=<database>;Port=<port>;UID=<username>;Password=<Password>`. Daha fazla özellik durumunuz ayarlayabilirsiniz:

| Özellik | Açıklama | Seçenekler | Gerekli |
|:--- |:--- |:--- |:--- |:--- |
| EncryptionMethod (EM)| Sürücü yöntemi sürücü ve veritabanı sunucusu arasında gönderilen verileri şifrelemek için kullanır. Örneğin `ValidateServerCertificate=<0/1/6>;`| 0 (şifreleme) **(varsayılan)** / 1 (SSL) / 6 (RequestSSL) | Hayır |
| ValidateServerCertificate (VSC'yi) | Sürücü SSL şifrelemesi etkin olduğunda, veritabanı sunucusu tarafından gönderilen sertifikayı doğrulayıp doğrulamadığını belirler (şifreleme yöntemini = 1). Örneğin `ValidateServerCertificate=<0/1>;`| 0 (devre dışı) **(varsayılan)** / 1 (etkin) | Hayır |

**Örnek:**

```json
{
    "name": "PostgreSqlLinkedService",
    "properties": {
        "type": "PostgreSql",
        "typeProperties": {
            "connectionString": {
                 "type": "SecureString",
                 "value": "Server=<server>;Database=<database>;Port=<port>;UID=<username>;Password=<Password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Aşağıdaki yük ile bağlantılı PostgreSQL hizmeti kullanıyorsanız, hala olarak desteklenmektedir-ileride yeni bir kullanmak için önerilir, açıkken.

**Önceki yükü:**

```json
{
    "name": "PostgreSqlLinkedService",
    "properties": {
        "type": "PostgreSql",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
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

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için veri kümeleri makalesine bakın. Bu bölümde PostgreSQL veri kümesi tarafından desteklenen özellikler listesini sağlar.

PostgreSQL verileri kopyalamak için kümesine tür özelliği ayarlamak **RelationalTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak: **RelationalTable** | Evet |
| tableName | PostgreSQL veritabanında tablonun adı. | ("Sorgu" etkinliği kaynağındaki belirtilmişse) yok |

**Örnek**

```json
{
    "name": "PostgreSQLDataset",
    "properties":
    {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<PostgreSQL linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde PostgreSQL kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="postgresql-as-source"></a>Kaynak olarak PostgreSQL

PostgreSQL verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **RelationalSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **RelationalSource** | Evet |
| sorgu | Verileri okumak için özel SQL sorgusu kullanın. Örneğin: `"query": "SELECT * FROM \"MySchema\".\"MyTable\""`. | (Veri kümesinde "tableName" belirtilmişse) yok |

> [!NOTE]
> Şema ve tablo adları büyük/küçük harfe duyarlıdır. Bunları içine `""` (çift tırnak) sorgu.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromPostgreSQL",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<PostgreSQL input dataset name>",
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
                "type": "RelationalSource",
                "query": "SELECT * FROM \"MySchema\".\"MyTable\""
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-postgresql"></a>Eşleme PostgreSQL için veri türü

PostgreSQL veri kopyalama işlemi sırasında aşağıdaki eşlemelerini PostgreSQL veri türlerinden Azure Data Factory geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) nasıl kopyalama etkinliği kaynak şema ve veri türü için havuz eşlemeleri hakkında bilgi edinmek için.

| PostgreSQL veri türü | PostgresSQL diğer adlar | Veri Fabrikası geçici veri türü |
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
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
