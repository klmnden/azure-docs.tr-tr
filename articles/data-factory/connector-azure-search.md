---
title: Azure Data Factory kullanarak verileri arama dizinine kopyalama | Microsoft Docs
description: Anında iletme veya bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak bir Azure search dizinine veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: jingwang
ms.openlocfilehash: faf0cab55ec0cef034638d218f2172f3676ff39b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66245103"
---
# <a name="copy-data-to-an-azure-search-index-using-azure-data-factory"></a>Azure Data Factory kullanarak bir Azure Search dizinine veri kopyalama

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-azure-search-connector.md)
> * [Geçerli sürüm](connector-azure-search.md)

Bu makalede, Azure Search dizinine veri kopyalamak için Azure veri fabrikasında kopyalama etkinliği kullanma açıklanmaktadır. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen kaynak veri deposundan verileri Azure Search dizinine kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli Azure Search bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Bağlı Azure Search hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **AzureSearch** | Evet |
| url | Azure Search hizmeti için URL. | Evet |
| key | Azure Search hizmeti için yönetici anahtarı. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel ağında bulunuyorsa), Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

> [!IMPORTANT]
> Azure Search'te Azure Search dizinine bir bulut veri deposundan veri kopyalamayı bağlı hizmeti, Azure tümleştirme çalışma zamanı connactVia açık bölgede ile başvurmanız gerekir. Bölge, Azure Search bulunan biri ayarlayın. Daha fazla bilgi [Azure Integration Runtime](concepts-integration-runtime.md#azure-integration-runtime).

**Örnek:**

```json
{
    "name": "AzureSearchLinkedService",
    "properties": {
        "type": "AzureSearch",
        "typeProperties": {
            "url": "https://<service>.search.windows.net",
            "key": {
                "type": "SecureString",
                "value": "<AdminKey>"
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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için veri kümeleri makalesine bakın. Bu bölümde, Azure Search veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Azure Search'e veri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **AzureSearchIndex** | Evet |
| indexName | Azure Search dizininin adı. Veri fabrikası, Dizin oluşturulmaz. Azure Search'te dizin varolmalıdır. | Evet |

**Örnek:**

```json
{
    "name": "AzureSearchIndexDataset",
    "properties": {
        "type": "AzureSearchIndex",
        "linkedServiceName": {
            "referenceName": "<Azure Search linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties" : {
            "indexName": "products"
        }
   }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Azure Search kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-search-as-sink"></a>Havuz olarak, Azure Search'ü

Azure Search'e veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **AzureSearchIndexSink**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **AzureSearchIndexSink** | Evet |
| writeBehavior | Bir belge dizinde zaten mevcut olduğunda değiştirin ya da birleştirme belirtir. Bkz: [WriteBehavior özelliği](#writebehavior-property).<br/><br/>İzin verilen değerler şunlardır: **Birleştirme** (varsayılan), ve **karşıya**. | Hayır |
| writeBatchSize | Arabellek boyutu writeBatchSize ulaştığında, verileri Azure Search dizinine yükler. Bkz: [WriteBatchSize özelliği](#writebatchsize-property) Ayrıntılar için.<br/><br/>İzin verilen değerler: 1 ila 1.000; tamsayı Varsayılan 1000'dir. | Hayır |

### <a name="writebehavior-property"></a>WriteBehavior özelliği

Veri yazarken AzureSearchSink upsert eder. Diğer bir deyişle, Azure Search dizin belge anahtarı zaten varsa belge yazarken, Azure Search çakışma özel durum yerine var olan bir belgeyi güncelleştirir.

AzureSearchSink (Azure Search SDK'sı kullanılarak) aşağıdaki iki upsert davranışları sağlar:

- **Birleştirme**: var olan bir yeni belge içindeki tüm sütunları birleştirin. Yeni belge null değere sahip sütunları için var olan bir değeri korunur.
- **Karşıya yükleme**: Var olan bir yeni belge değiştirir. Yeni belge içinde belirtilmeyen sütunlar için değer olup olmadığını null olmayan bir değer var olan bir belgeyi veya null olarak ayarlanır.

Varsayılan davranış **birleştirme**.

### <a name="writebatchsize-property"></a>WriteBatchSize özelliği

Azure arama hizmeti, toplu iş olarak belge yazma destekler. Bir toplu iş 1 ila 1.000 eylemleri içerebilir. Karşıya yükleme/birleştirme işlemi gerçekleştirmek için bir belge bir eylem gerçekleştirir.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToAzureSearch",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Search output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureSearchIndexSink",
                "writeBehavior": "Merge"
            }
        }
    }
]
```

### <a name="data-type-support"></a>Veri türü desteği

Aşağıdaki tabloda, bir Azure Search veri türü veya desteklenip desteklenmediğini belirtir.

| Azure Search veri türü | Azure Search'ü havuz desteklenen |
| ---------------------- | ------------------------------ |
| String | E |
| Int32 | E |
| Int64 | E |
| Double | E |
| Boolean | E |
| DataTimeOffset | E |
| String Array | N |
| GeographyPoint | N |

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
