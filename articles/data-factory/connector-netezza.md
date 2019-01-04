---
title: Azure Data Factory kullanarak Netezza'dan verileri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına Netezza'dan bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
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
ms.openlocfilehash: 676eac6853c8cead40cb702855090eac5e2ce7d8
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54025666"
---
# <a name="copy-data-from-netezza-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Netezza'dan verileri kopyalama 

Bu makalede, kopyalama etkinliği Azure Data Factory'de Netezza'dan verileri kopyalamak için nasıl kullanılacağını özetlenmektedir. Makaleyi yapılar [Azure veri fabrikasında kopyalama etkinliği](copy-activity-overview.md), kopyalama etkinliği genel bir bakış sunar.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna Netezza'dan verileri kopyalayabilirsiniz. Kopyalama etkinliği kaynak ve havuz olarak desteklediğini veri listesini depolar için bkz: [desteklenen veri depoları ve biçimler](copy-activity-overview.md#supported-data-stores-and-formats).

Azure Data Factory bağlantısını etkinleştirmek için yerleşik bir sürücü sağlar. Bu bağlayıcıyı kullanmak için herhangi bir sürücüsü el ile yüklemeniz gerekmez.

## <a name="get-started"></a>başlarken

.NET SDK, Python SDK'sı, Azure PowerShell, REST API veya bir Azure Resource Manager şablonu kullanarak kopyalama etkinliği kullanan bir işlem hattı oluşturabilirsiniz. Bkz: [kopyalama etkinliği Öğreticisi](quickstart-create-data-factory-dot-net.md) kopyalama etkinliği içeren işlem hattı oluşturma konusunda adım adım yönergeler için.

Aşağıdaki bölümler Netezza Bağlayıcısı özel olan Data Factory varlıkları tanımlamak için kullanabileceğiniz özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Aşağıdaki özellikler Netezza bağlı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** özelliği ayarlanmalıdır **Netezza**. | Evet |
| bağlantı dizesi | Netezza'ya bağlanma bir ODBC bağlantı dizesi. Bu alan olarak işaretlemek bir **SecureString** Data Factory'de güvenle depolamak için türü. Ayrıca [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz genel olarak erişilebilir olması durumunda) bir şirket içinde barındırılan tümleştirme çalışma zamanı veya Azure Integration Runtime seçebilirsiniz. Belirtilmezse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Hayır |

Bir bağlantı dizesi olan `Server=<server>;Port=<port>;Database=<database>;UID=<user name>;PWD=<password>`. Aşağıdaki tabloda ayarlayabilirsiniz daha fazla özellik açıklanmaktadır:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| SecurityLevel | Veri deposuna bağlantı için sürücüyü kullanır (SSL/TLS) güvenlik düzeyi. Örnek: `SecurityLevel=preferredSecured`. Desteklenen değerler şunlardır:<br/>- **Yalnızca güvenli** (**onlyUnSecured**): Sürücü SSL kullanmaz.<br/>- **Güvenli olmayan (preferredUnSecured) (varsayılan) tercih edilen**: Sunucu sunar, sürücü SSL kullanmaz. <br/>- **Güvenli (preferredSecured) tercih edilen**: Sunucu sunar, sürücü SSL kullanır. <br/>- **Yalnızca (onlySecured) güvenli**: Bir SSL bağlantısı yoksa sürücüsü bağlama değil. | Hayır |
| CASertifikaDosyası | Sunucu tarafından kullanılan SSL sertifikasının tam yolu. Örnek: `CaCertFile=<cert path>;`| SSL etkinleştirilmişse, Evet |

**Örnek**

```json
{
    "name": "NetezzaLinkedService",
    "properties": {
        "type": "Netezza",
        "typeProperties": {
            "connectionString": {
                 "type": "SecureString",
                 "value": "Server=<server>;Port=<port>;Database=<database>;UID=<user name>;PWD=<password>"
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

Bu bölümde Netezza veri kümesini destekleyen özelliklerin bir listesini sağlar.

Bölümleri ve veri kümeleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md). 

Netezza'dan verileri kopyalamak için ayarlanmış **türü** veri kümesine özelliği **NetezzaTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **NetezzaTable** | Evet |
| tableName | Tablonun adı. | Hayır (etkinlik kaynağı "sorgu" belirtilmişse) |

**Örnek**

```json
{
    "name": "NetezzaDataset",
    "properties": {
        "type": "NetezzaTable",
        "linkedServiceName": {
            "referenceName": "<Netezza linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bu bölümde Netezza kaynağının desteklediği özelliklerin bir listesini sağlar.

Bölümleri ve etkinlikleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md). 

### <a name="netezza-as-source"></a>Kaynak olarak Netezza

Netezza'dan verileri kopyalamak için ayarlanmış **kaynak** türü için kopyalama etkinliğindeki **NetezzaSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kopyalama etkinliği kaynak özelliği ayarlanmalıdır **NetezzaSource**. | Evet |
| sorgu | Verileri okumak için özel bir SQL sorgusu kullanın. Örnek: `"SELECT * FROM MyTable"` | Yok (veri kümesinde "TableName" değeri belirtilmişse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromNetezza",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Netezza input dataset name>",
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
                "type": "NetezzaSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar

Kopyalama etkinliği kaynak olarak destekler ve şu havuzlar Azure Data Factory'de veri depolarının listesi için bkz. [desteklenen veri depoları ve biçimler](copy-activity-overview.md#supported-data-stores-and-formats).
