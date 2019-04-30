---
title: Azure Data Factory kullanarak müşteri için/SAP buluta veri kopyalama | Microsoft Docs
description: Desteklenen kaynak veri depolarından arasında desteklenen havuz veri depolarına müşteri için SAP Cloud (veya) için SAP Cloud müşteri için Data Factory kullanarak verileri kopyalama öğrenin.
services: data-factory
documentationcenter: ''
author: WenJason
manager: digimobile
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
origin.date: 04/17/2018
ms.date: 04/22/2019
ms.author: v-jay
ms.openlocfilehash: e4625b934f9e1cf98254f3dee59f9c26e8e16fb5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60578717"
---
# <a name="copy-data-from-sap-cloud-for-customer-c4c-using-azure-data-factory"></a>SAP Cloud (C4C) müşteri için Azure Data Factory kullanarak verileri kopyalama

Bu makalede, kopyalama etkinliği Azure Data Factory'de veri / için SAP Cloud (C4C) müşteri için kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Veri SAP Buluttan müşteri için tüm desteklenen havuz veri deposuna kopyalamak ya da veri herhangi bir desteklenen kaynak veri deposundan müşteri için SAP Cloud kopyalayın. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu bağlayıcı, satış, hizmet için SAP Cloud ve sosyal Engagement çözümleri için SAP Cloud SAP bulut dahil olmak üzere müşteri için SAP Cloud / için veri kopyalamak Azure Data Factory sağlar.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli müşteri Bağlayıcısı için SAP Cloud tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Aşağıdaki özellikler için SAP Cloud bağlı müşteri hizmetleri için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **SapCloudForCustomer**. | Evet |
| url | SAP C4C OData hizmeti URL'si. | Evet |
| kullanıcı adı | İçin SAP C4C bağlanmak için kullanıcı adı belirtin. | Evet |
| password | Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. | Kaynak, havuz için Evet Hayır |

>[!IMPORTANT]
>Müşteri için SAP buluta verileri açıkça kopyalamak için [Azure IR oluşturma](create-azure-integration-runtime.md#create-azure-ir) aşağıdaki örnekteki gibi bağlantılı hizmetteki ilişkilendirme yanı sıra müşteri için SAP bulut yakın bir konum:

**Örnek:**

```json
{
    "name": "SAPC4CLinkedService",
    "properties": {
        "type": "SapCloudForCustomer",
        "typeProperties": {
            "url": "https://<tenantname>.crm.ondemand.cn/sap/c4c/odata/v1/c4codata/" ,
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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, müşteri veri kümesi için SAP bulut tarafından desteklenen özelliklerin bir listesini sağlar.

Müşteri için SAP Buluttan veri kopyalamak için dataset öğesinin type özelliği ayarlamak **SapCloudForCustomerResource**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **SapCloudForCustomerResource** |Evet |
| path | SAP C4C OData varlık yolu belirtin. |Evet |

**Örnek:**

```json
{
    "name": "SAPC4CDataset",
    "properties": {
        "type": "SapCloudForCustomerResource",
        "typeProperties": {
            "path": "<path e.g. LeadCollection>"
        },
        "linkedServiceName": {
            "referenceName": "<SAP C4C linked service>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, müşteri kaynağı için SAP bulut tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="sap-c4c-as-source"></a>SAP C4C kaynağı olarak

Müşteri için SAP Buluttan veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **SapCloudForCustomerSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **SapCloudForCustomerSource**  | Evet |
| sorgu | Verileri okumak için özel OData sorgusu belirtin. | Hayır |

Belirli bir gün için verileri almak için örnek sorgu: `"query": "$filter=CreatedOn ge datetimeoffset'2017-07-31T10:02:06.4202620Z' and CreatedOn le datetimeoffset'2017-08-01T10:02:06.4202620Z'"`

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromSAPC4C",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP C4C input dataset>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SapCloudForCustomerSource",
                "query": "<custom query e.g. $top=10>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="sap-c4c-as-sink"></a>SAP C4C havuz olarak

Müşteri için SAP Cloud veri kopyalamak için kopyalama etkinliğine de Havuz türü ayarlayın. **SapCloudForCustomerSink**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **SapCloudForCustomerSink**  | Evet |
| writeBehavior | İşlemi yazma davranışını. "Ekle", "Güncelleştir" olabilir. | Hayır. Varsayılan "ekleme". |
| writeBatchSize | Yazma işlemi toplu iş boyutu. Toplu iş boyutu, en iyi performansı elde etmek için farklı bir tablo veya sunucusu için farklı olabilir. | Hayır. Varsayılan olarak 10. |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToSapC4c",
        "type": "Copy",
        "inputs": [{
            "type": "DatasetReference",
            "referenceName": "<dataset type>"
        }],
        "outputs": [{
            "type": "DatasetReference",
            "referenceName": "SapC4cDataset"
        }],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SapCloudForCustomerSink",
                "writeBehavior": "Insert",
                "writeBatchSize": 30
            },
            "parallelCopies": 10,
            "dataIntegrationUnits": 4,
            "enableSkipIncompatibleRow": true,
            "redirectIncompatibleRowSettings": {
                "linkedServiceName": {
                    "referenceName": "ErrorLogBlobLinkedService",
                    "type": "LinkedServiceReference"
                },
                "path": "incompatiblerows"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-sap-cloud-for-customer"></a>Müşteri için SAP bulut için veri türü eşlemesi

Müşteri için SAP Buluttan veri kopyalama yapılırken, aşağıdaki eşlemeler SAP Buluttan müşteri veri türleri için Azure Data Factory geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) eşlemelerini nasıl yapar? kopyalama etkinliği kaynak şema ve veri türü için havuz hakkında bilgi edinmek için.

| SAP C4C OData veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| Edm.Binary | Byte[] |
| Edm.Boolean | Bool |
| Edm.Byte | Byte[] |
| Edm.DateTime | DateTime |
| Edm.Decimal | Decimal |
| Edm.Double | Double |
| Edm.Single | Single |
| Edm.Guid | Guid |
| Edm.Int16 | Int16 |
| Edm.Int32 | Int32 |
| Edm.Int64 | Int64 |
| Edm.SByte | Int16 |
| Edm.String | String |
| Edm.Time | TimeSpan |
| Edm.DateTimeOffset | DateTimeOffset |


## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
