---
title: Azure Data Factory kullanarak Search dizinine veri kopyalama | Microsoft Docs
description: "İtme veya kopyalama etkinliği Azure Data Factory ardışık düzeninde kullanarak bir Azure search dizinine veri kopyalama hakkında bilgi edinin."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/30/2017
ms.author: jingwang
ms.openlocfilehash: 2b6219dc509b1af8f196f056b489a31fa331acaf
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="copy-data-to-an-azure-search-index-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Search dizini için veri kopyalama

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-azure-search-connector.md)
> * [Sürüm 2 - Önizleme](connector-azure-search.md)

Bu makalede kopya etkinliği Azure Data Factory'de Azure Search dizinine verileri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 Azure Search Bağlayıcısı](v1/data-factory-azure-search-connector.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen kaynak veri deposundan verileri Azure Search dizinine kopyalayabilirsiniz. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

## <a name="getting-started"></a>Başlarken
.NET SDK'sı, Python SDK'sı, Azure PowerShell, REST API veya Azure Resource Manager şablonu kullanarak kopyalama etkinliği ile işlem hattı oluşturabilirsiniz. Bkz: [kopyalama etkinliği öğretici](quickstart-create-data-factory-dot-net.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

Aşağıdaki bölümler, Azure Search bağlayıcıya Data Factory varlıklarını belirli tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikleri, bağlantılı Azure Search hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **AzureSearch** | Evet |
| URL | Azure Search hizmeti için URL. | Evet |
| anahtar | Azure Search hizmeti için yönetici anahtarı. Bu alan bir SecureString işaretleyin. | Evet |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu özel bir ağda yer alıyorsa) Azure tümleştirmesi çalışma zamanı veya Self-hosted tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. |Hayır |

> [!IMPORTANT]
> Azure Search'te Azure Search dizinine bir bulut veri deposundan veri kopyalama hizmet bağlandığında, Azure tümleştirmesi çalışma zamanı connactVia açık bölgede ile başvurmanız gerekir. Bölge, Azure Search bulunduğu bir ayarlayın. ' Dan daha fazla bilgi edinin [Azure tümleştirmesi çalışma zamanı](concepts-integration-runtime.md#azure-integration-runtime).

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

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için veri kümeleri makalesine bakın. Bu bölümde Azure Search'te veri kümesi tarafından desteklenen özellikler listesini sağlar.

Azure Search verileri kopyalamak için kümesine tür özelliği ayarlamak **RelationalTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak: **AzureSearchIndex** | Evet |
| indexName | Azure Search dizini adı. Veri Fabrikası dizinini oluşturmaz. Azure Search'te dizin mevcut olması gerekir. | Evet |

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

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde Azure Search kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="azure-search-as-sink"></a>Havuz olarak Azure arama

Azure Search verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **AzureSearchIndexSink**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **AzureSearchIndexSink** | Evet |
| WriteBehavior | Birleştir veya bir belge dizinde zaten mevcut olduğunda Değiştir belirtir. Bkz: [WriteBehavior özelliği](#writebehavior-property).<br/><br/>İzin verilen değerler: **birleştirme** (varsayılan), ve **karşıya**. | Hayır |
| writeBatchSize | Arabellek boyutu writeBatchSize ulaştığında Azure Search dizinine veri yükler. Bkz: [WriteBatchSize özelliği](#writebatchsize-property) Ayrıntılar için.<br/><br/>İzin verilen değerler: tamsayı 1 için 1.000; Varsayılan 1000'dir. | Hayır |

### <a name="writebehavior-property"></a>WriteBehavior özelliği

Verileri yazarken AzureSearchSink upserts. Diğer bir deyişle, belge anahtarı Azure arama dizini zaten varsa bir belge yazarken, Azure Search çakışma özel durum atma yerine varolan bir belgeyi güncelleştirir.

AzureSearchSink (AzureSearch SDK kullanılarak) aşağıdaki iki upsert davranışlar sağlar:

- **Birleştirme**: yeni belgedeki tüm sütunları mevcut birleştirin. Yeni belge null değere sahip sütunlar için mevcut değeri korunur.
- **Karşıya yükleme**: varolan bir yeni belge değiştirir. Yeni belge içinde belirtilmeyen sütunlar için değer olup olmadığını değeri null olmayan mevcut belgede veya null olarak ayarlanır.

Varsayılan davranış **birleştirme**.

### <a name="writebatchsize-property"></a>WriteBatchSize özelliği

Azure Search Hizmeti yazma belgeleri toplu iş olarak destekler. Bir toplu iş için 1 1.000 eylemler içerebilir. Bir eylem karşıya yükleme/birleştirme işlemi gerçekleştirmek için bir belge işler.

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

| Azure arama veri türü | Azure arama havuzunda desteklenir |
| ---------------------- | ------------------------------ |
| Dize | E |
| Int32 | E |
| Int64 | E |
| Çift | E |
| Boole | E |
| DataTimeOffset | E |
| Dize dizisi | N |
| GeographyPoint | N |

## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
