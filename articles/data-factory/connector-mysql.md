---
title: Mysql'i Azure Data Factory kullanarak verileri kopyalama | Microsoft Docs
description: MySQL connector bir havuz olarak desteklenen bir veri deposuna bir MySQL veritabanından veri kopyalamanıza olanak veren bir Azure Data Factory hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: jingwang
ms.openlocfilehash: e05e2f2d04aeb572307f8114ca80f148b3d50e3d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61370725"
---
# <a name="copy-data-from-mysql-using-azure-data-factory"></a>MySQL Azure Data Factory kullanarak verileri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-onprem-mysql-connector.md)
> * [Geçerli sürüm](connector-mysql.md)

Bu makalede, kopyalama etkinliği Azure Data Factory'de bir MySQL veritabanından veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna MySQL veritabanından veri kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Bu MySQL bağlayıcısını MySQL özellikle destekleyen **sürüm 5.6 ve 5.7**.

## <a name="prerequisites"></a>Önkoşullar

MySQL veritabanınızı genel olarak erişilebilir durumda değilse, şirket içinde barındırılan tümleştirme çalışma zamanını oluşturan ayarlayın gerekir. Şirket içinde barındırılan tümleştirme çalışma zamanları hakkında bilgi edinmek için [şirket içinde barındırılan tümleştirme çalışma zamanı](create-self-hosted-integration-runtime.md) makalesi. Tümleştirme çalışma zamanı 3.7 sürümünden itibaren yerleşik bir MySQL sürücüsünün sağlar, bu nedenle herhangi bir sürücü el ile yüklemeniz gerekmez.

3.7 düşük şirket içinde barındırılan IR sürümü için yüklemeniz gerekir. [MySQL Connector/Net için Microsoft Windows](https://dev.mysql.com/downloads/connector/net/) 6.6.5 ve tümleştirme çalışma zamanı makinesinde 6.10.7 arasında sürümü. Bu 32 bit sürücü IR 64 bit ile uyumlu değil

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler belirli Data Factory varlıkları için MySQL bağlayıcısını tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

MySQL bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **MySql** | Evet |
| connectionString | MySQL örneği için Azure veritabanına bağlanmak için gereken bilgileri belirtin.<br/>Bu alan, Data Factory'de güvenle depolamak için bir SecureString olarak işaretleyin. Parola Azure anahtar kasası ve çekme koyabilirsiniz `password` yapılandırma bağlantı dizesini dışında. Aşağıdaki örneklere bakın ve [kimlik bilgilerini Azure Key Vault'ta Store](store-credentials-in-key-vault.md) daha fazla ayrıntı içeren makalesi. | Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz genel olarak erişilebilir değilse), şirket içinde barındırılan tümleştirme çalışma zamanı veya Azure Integration Runtime kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

Bir bağlantı dizesi olan `Server=<server>;Port=<port>;Database=<database>;UID=<username>;PWD=<password>`. Daha fazla özellik durumunuz ayarlayabilirsiniz:

| Özellik | Açıklama | Seçenekler | Gerekli |
|:--- |:--- |:--- |:--- |
| SSLMode | Bu seçenek sürücü SSL şifreleme ve doğrulama Mysql'e bağlanırken kullanıp kullanmayacağını belirtir. Örneğin `SSLMode=<0/1/2/3/4>`| Devre dışı (0) / tercih edilen (1) **(varsayılan)** / gerekli (2) / VERIFY_CA (3) / VERIFY_IDENTITY (4) | Hayır |
| UseSystemTrustStore | Bu seçenek, bir CA sertifikası sistem güven deposu veya belirtilen bir PEM dosyası kullanılıp kullanılmayacağını belirtir. Örneğin `UseSystemTrustStore=<0/1>;`| (1) etkin / devre dışı (0) **(varsayılan)** | Hayır |

**Örnek:**

```json
{
    "name": "MySQLLinkedService",
    "properties": {
        "type": "MySql",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=<server>;Port=<port>;Database=<database>;UID=<username>;PWD=<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek: parola Azure Key Vault'ta depolama**

```json
{
    "name": "MySQLLinkedService",
    "properties": {
        "type": "MySql",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=<server>;Port=<port>;Database=<database>;UID=<username>;"
            },
            "password": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Aşağıdaki yük ile bağlantılı MySQL hizmeti kullanıyorsanız, hala olarak desteklenmektedir-olan ancak bundan sonra yeni bir tane kullanmak için önerilir.

**Önceki yükü:**

```json
{
    "name": "MySQLLinkedService",
    "properties": {
        "type": "MySql",
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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için veri kümeleri makalesine bakın. Bu bölümde, MySQL veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Mysql'i veri kopyalamak için dataset öğesinin type özelliği ayarlamak **RelationalTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **RelationalTable** | Evet |
| tableName | MySQL veritabanı tablosunun adı. | Hayır (etkinlik kaynağı "sorgu" belirtilmişse) |

**Örnek**

```json
{
    "name": "MySQLDataset",
    "properties":
    {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<MySQL linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, MySQL kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="mysql-as-source"></a>MySQL kaynak olarak

Mysql'i veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **RelationalSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **RelationalSource** | Evet |
| query | Verileri okumak için özel bir SQL sorgusu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Yok (veri kümesinde "TableName" değeri belirtilmişse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromMySQL",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<MySQL input dataset name>",
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
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-mysql"></a>MySQL için eşleme veri türü

Mysql'i veri kopyalama işlemi sırasında aşağıdaki eşlemeler MySQL veri türlerinden Azure veri fabrikası geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) eşlemelerini nasıl yapar? kopyalama etkinliği kaynak şema ve veri türü için havuz hakkında bilgi edinmek için.

| MySQL veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| `bigint` |`Int64` |
| `bigint unsigned` |`Decimal` |
| `bit(1)` |`Boolean` |
| `bit(M), M>1`|`Byte[]`|
| `blob` |`Byte[]` |
| `bool` |`Int16` |
| `char` |`String` |
| `date` |`Datetime` |
| `datetime` |`Datetime` |
| `decimal` |`Decimal, String` |
| `double` |`Double` |
| `double precision` |`Double` |
| `enum` |`String` |
| `float` |`Single` |
| `int` |`Int32` |
| `int unsigned` |`Int64`|
| `integer` |`Int32` |
| `integer unsigned` |`Int64` |
| `long varbinary` |`Byte[]` |
| `long varchar` |`String` |
| `longblob` |`Byte[]` |
| `longtext` |`String` |
| `mediumblob` |`Byte[]` |
| `mediumint` |`Int32` |
| `mediumint unsigned` |`Int64` |
| `mediumtext` |`String` |
| `numeric` |`Decimal` |
| `real` |`Double` |
| `set` |`String` |
| `smallint` |`Int16` |
| `smallint unsigned` |`Int32` |
| `text` |`String` |
| `time` |`TimeSpan` |
| `timestamp` |`Datetime` |
| `tinyblob` |`Byte[]` |
| `tinyint` |`Int16` |
| `tinyint unsigned` |`Int16` |
| `tinytext` |`String` |
| `varchar` |`String` |
| `year` |`Int` |

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
