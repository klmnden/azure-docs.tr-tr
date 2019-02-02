---
title: Ya da Azure veri Gezgini'nde Azure Data Factory kullanarak verileri kopyalama | Microsoft Docs
description: Bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak Azure Veri Gezgini gelen veya veri kopyalama hakkında bilgi edinin.
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
ms.date: 02/01/2019
ms.author: orspod
ms.openlocfilehash: f492878ffcb888560d2aed269608950927cebd43
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2019
ms.locfileid: "55570045"
---
# <a name="copy-data-to-or-from-azure-data-explorer-using-azure-data-factory"></a>Ya da Azure veri Gezgini'nde Azure Data Factory kullanarak veri kopyalama

Bu makalede, kopyalama etkinliği Azure Data Factory'de gelen veya giden veri kopyalamak için nasıl kullanılacağını özetlenmektedir [Azure Veri Gezgini](../data-explorer/data-explorer-overview.md). Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Azure veri Gezgini'ne herhangi bir desteklenen kaynak veri deposundan veri kopyalayabilirsiniz. Ayrıca, Azure veri Gezgini'nden desteklenen havuz veri deposuna veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak ve havuz desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md) tablo.

>[!NOTE]
>Şu anda veri gönderip buralardan veri Azure Veri Gezgini / için şirket içinde barındırılan tümleştirme çalışma zamanını kullanarak şirket içi veri deposuna kopyalama henüz desteklenmiyor.

Azure Veri Gezgini Bağlayıcısı'nı aşağıdakileri sağlar:

* Azure Active Directory (Azure AD) ile uygulama belirteci kimlik doğrulamasını kullanarak verileri kopyalama bir **hizmet sorumlusu**.
* Bir kaynak olarak KQL (Kusto) sorgusu kullanarak veri alın.
* Bir havuz olarak verileri hedef tabloya ekler.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli Azure Veri Gezgini bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Azure Veri Gezgini bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** özelliği ayarlanmalıdır **AzureDataExplorer** | Evet |
| endpoint | Uç nokta URL'si biçiminde Azure Veri Gezgini kümenin `https://<clusterName>.kusto.windows.net`. | Evet |
| veritabanı | Veritabanının adı. | Evet |
| kiracı | Kiracı bilgileri (etki alanı adı veya Kiracı kimliği), uygulamanızın bulunduğu altında belirtin. Bu, Azure portalının sağ üst köşedeki fareyle gelerek alın. | Evet |
| servicePrincipalId | Uygulamanın istemci kimliği belirtin. | Evet |
| serviceprincipalkey değerleri | Uygulama anahtarını belirtin. Bu alan olarak işaretlemek bir **SecureString** Data Factory'de güvenle depolamak için veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |

**Bağlı hizmeti özellikleri örneği:**

```json
{
    "name": "AzureDataExplorerLinkedService",
    "properties": {
        "type": "AzureDataExplorer",
        "typeProperties": {
            "endpoint": "https://<clusterName>.kusto.windows.net",
            "database": "<database name>",
            "tenant": "<tenant name/id e.g. microsoft.onmicrosoft.com>",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, Azure Veri Gezgini veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Azure veri Gezgini'ne veri kopyalamak için dataset öğesinin type özelliği ayarlamak **AzureDataExplorerTable**.

Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** özelliği ayarlanmalıdır **AzureDataExplorerTable** | Evet |
| tablo | Bağlı hizmet başvurduğu tablonun adı. | Havuz için Evet; Kaynak için Hayır |

**Veri kümesi özellikleri örneği**

```json
{
   "name": "AzureDataExplorerDataset",
    "properties": {
        "type": "AzureDataExplorerTable",
        "linkedServiceName": {
            "referenceName": "<Azure Data Explorer linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "table": "<table name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Azure Veri Gezgini kaynak ve havuz desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-data-explorer-as-source"></a>Kaynak olarak Azure Veri Gezgini

Azure veri Gezgini'nde verileri kopyalamak için ayarlanmış **türü** için kopyalama etkinliği kaynağı özelliğinde **AzureDataExplorerSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kopyalama etkinliği kaynağı özelliği ayarlanmalıdır: **AzureDataExplorerSource** | Evet |
| sorgu | Verileri okumak için özel KQL sorgu kullanın. | Evet |
| queryTimeout | Sorgu isteği önceki bekleme süresi zaman aşımına belirtin. Varsayılan değer: 10 dakikalık (00: 10:00); izin verilen en yüksek değer olan 1 saat (01: 00:00). | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromAzureDataExplorer",
        "type": "Copy",
        "typeProperties": {
            "source": {
                "type": "AzureDataExplorerSource",
                "query": "TestTable1 | take 10",
                "queryTimeout": "00:10:00"
            },
            "sink": {
                "type": "<sink type>"
            }
        },
        "inputs": [
            {
                "referenceName": "<Azure Data Explorer input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ]
    }
]
```

### <a name="azure-data-explorer-as-sink"></a>Havuz olarak Azure Veri Gezgini

Verileri Azure veri Gezgini'ne kopyalamak için kopyalama etkinliği Havuz türü özelliğini ayarlayın. **AzureDataExplorerSink**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kopyalama etkinliği havuz özelliği ayarlanmalıdır: **AzureDataExplorerSink** | Evet |
| ingestionMappingName | Adını [csv eşleme](/azure/kusto/management/mappings#csv-mapping) tablosunda. Kaynaktan Azure verileri araştırmak için sütunları eşlemek için de kopyalama etkinliği kullanabilirsiniz [sütun eşlemesi](copy-activity-schema-and-type-mapping.md). | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToAzureDataExplorer",
        "type": "Copy",
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureDataExplorerSink",
                "ingestionMappingName": "<optional csv mapping name>"
            }
        },
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Data Explorer output dataset name>",
                "type": "DatasetReference"
            }
        ]
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).