---
title: Veri kopyalama/Azure Cosmos Data Factory kullanarak DB'den | Microsoft Docs
description: "Desteklenen kaynak veri depolarına Azure Cosmos DB'de (veya) Cosmos DB verileri kopyalamak için Data Factory kullanarak desteklenen havuz depolarını öğrenin."
services: data-factory, cosmosdb
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: multiple
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2017
ms.author: jingwang
ms.openlocfilehash: 7d914684a0ee5598cee7972b78c3ec6296184466
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="copy-data-to-or-from-azure-cosmos-db-using-azure-data-factory"></a>Veri ya da Azure Cosmos Azure Data Factory kullanarak DB'den kopyalayın

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-azure-documentdb-connector.md)
> * [Sürüm 2 - Önizleme](connector-azure-cosmos-db.md)

Bu makalede kopya etkinliği Azure Data Factory'de ilk ve son Azure Cosmos DB (DocumentDB API) verileri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 içinde Azure Cosmos DB connnector](v1/data-factory-azure-documentdb-connector.md).

## <a name="supported-scenarios"></a>Desteklenen senaryolar

Verileri Azure Cosmos DB'den tüm desteklenen havuz veri deposuna kopyalamak ya da veri tüm desteklenen kaynak veri deposundan Azure Cosmos DB kopyalayın. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Azure Cosmos DB bağlayıcı destekler:

- Cosmos DB [DocumentDB API](https://docs.microsoft.com/en-us/azure/cosmos-db/documentdb-introduction).
- JSON belgeleri olarak alma/verme- ya da veri kopyalama/tablo dataset ör. SQL veritabanı, CSV dosyaları, vb.

Kopyalamak için belgeleri olarak-olduğu için/JSON dosyalarında veya başka bir Cosmos DB koleksiyonu bkz [içeri/dışarı aktarma JSON belgeleri](#importexport-json-documents).

## <a name="getting-started"></a>Başlarken
.NET SDK'sı, Python SDK'sı, Azure PowerShell, REST API veya Azure Resource Manager şablonu kullanarak kopyalama etkinliği ile işlem hattı oluşturabilirsiniz. Bkz: [kopyalama etkinliği öğretici](quickstart-create-data-factory-dot-net.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

Aşağıdaki bölümlerde Azure Cosmos DB Data Factory varlıklarını belirli tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler, Azure Cosmos DB bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **CosmosDb**. | Evet |
| connectionString |Azure Cosmos DB veritabanına bağlanmak için gereken bilgileri belirtin. Aşağıda örnek olarak bağlantı dizesinde veritabanı bilgisi belirtmek zorunda unutmayın. Bu alan bir SecureString işaretleyin. |Evet |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu özel bir ağda yer alıyorsa) Azure tümleştirmesi çalışma zamanı veya Self-hosted tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. |Hayır |

**Örnek:**

```json
{
    "name": "CosmosDbLinkedService",
    "properties": {
        "type": "CosmosDb",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
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

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için veri kümeleri makalesine bakın. Bu bölümde Azure Cosmos DB veri kümesi tarafından desteklenen özellikler listesini sağlar.

/ Azure Cosmos veritabanına veri kopyalamak için veri kümesi için tür özelliği ayarlamak **DocumentDbCollection**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak: **DocumentDbCollection** |Evet |
| collectionName |Cosmos DB belge koleksiyonunun adı. |Evet |

**Örnek:**

```json
{
    "name": "CosmosDbDataset",
    "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName":{
            "referenceName": "<Azure Cosmos DB linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "collectionName": "<collection name>"
        }
    }
}
```

### <a name="schema-by-data-factory"></a>Şema tarafından veri fabrikası

Azure Cosmos DB gibi şemasız veri depoları için kopyalama etkinliği şema aşağıdaki yollardan biriyle oluşturur. Bu nedenle, istediğiniz sürece [JSON belgeleri olarak içeri/dışarı aktarma-olan](#importexport-json-documents), veri yapılarını belirtmek için en iyi uygulamadır **yapısı** bölümü.

1. Verilerin yapısını kullanarak belirtirseniz **yapısı** özelliği veri kümesi tanımında Data Factory hizmetinin bu yapısı şema olarak geliştirir. Bir satır bir sütun için bir değer içermiyorsa, bu durumda, boş bir değer için sağlanır.
2. Verilerin yapısını kullanarak belirtmezseniz **yapısı** özelliği veri kümesi tanımında Data Factory hizmetinin oluşturur şema verileri ilk satırını kullanarak. Bu durumda, ilk satırın tam şema içermiyorsa, bazı sütunları kopyalama işlemi sonucunda eksik olacaktır.

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde Azure Cosmos DB kaynak ve havuz tarafından desteklenen özellikler listesini sağlar.

### <a name="azure-cosmos-db-as-source"></a>Kaynak olarak Azure Cosmos DB

Azure Cosmos Veritabanından veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **DocumentDbCollectionSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **DocumentDbCollectionSource** |Evet |
| sorgu |Verileri okumak için Cosmos DB sorgusunu belirtin.<br/><br/>Örnek:`SELECT c.BusinessEntityID, c.Name.First AS FirstName, c.Name.Middle AS MiddleName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Hayır <br/><br/>Belirtilmezse, yürütülen SQL deyimi:`select <columns defined in structure> from mycollection` |
| nestingSeparator |Belge iç içe geçmiş belirtmek için özel karakter ve nasıl flattern için sonuç kümesi.<br/><br/>Örneğin, bir Cosmos DB sorgu iç içe geçmiş bir sonuç döndürürse `"Name": {"First": "John"}`, kopyalama etkinliği tanımlar ve sütun adını "Name.First" "John" değeriyle nestedSeparator nokta olduğunda. |Hayır (varsayılan değer nokta `.`) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromCosmosDB",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Document DB input dataset name>",
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
                "type": "DocumentDbCollectionSource",
                "query": "SELECT c.BusinessEntityID, c.Name.First AS FirstName, c.Name.Middle AS MiddleName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\""
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-cosmos-db-as-sink"></a>Havuz olarak Azure Cosmos DB

Azure Cosmos Veritabanından veri kopyalamak için kopyalama etkinliği Havuz türü ayarlayın. **DocumentDbCollectionSink**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **DocumentDbCollectionSink** |Evet |
| nestingSeparator |Bir özel karakter iç içe geçmiş belge belirtmek için kaynak sütun adı gereklidir. <br/><br/>Örneğin, `Name.First` çıkış veri kümesinde bulunan yapısı Cosmos DB belgede aşağıdaki JSON yapısını oluşturur:`"Name": {"First": "[value maps to this column from source]"}` nestedSeparator nokta olduğunda. |Hayır (varsayılan değer nokta `.`) |
| writeBatchTimeout |İşlemin zaman aşımına uğramadan önce tamamlanması için bir süre bekleyin.<br/><br/>İzin verilen değerler: timespan. Örnek: "00: 30:00" (30 dakika). |Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToCosmosDB",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Document DB output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "DocumentDbCollectionSink"
            }
        }
    }
]
```

## <a name="importexport-json-documents"></a>İçeri/dışarı aktarma JSON belgeleri

Bu Cosmos DB Bağlayıcısı'nı kullanarak kolayca yapabilecekleriniz

* JSON belgeleri Cosmos DB, Azure Blob, Azure Data Lake Store ile Azure Data Factory ile desteklenen diğer dosya tabanlı depoları dahil olmak üzere çeşitli kaynaklardan aktarın.
* JSON belgeleri Cosmos DB collecton çeşitli dosya tabanlı depoları verin.
* Belgeleri iki Cosmos DB koleksiyonları arasında kopyalama-değil.

Bu tür şema belirsiz kopya elde etmek için:

- Cosmos DB veri kümeleri içinde "yapısı" bölümü belirtmeyin; ve kopyalama etkinliği Cosmos DB kaynak/havuz, "nestingSeparator" özelliği belirtmeyin.
- Alma / kümesindeki karşılık gelen dosya deposu, JSON dosyaları dışarı aktarma belirttiğinizde biçim türü "JsonFormat" ve yapılandırma "filePattern" düzgün (bkz [JSON biçimine](supported-file-formats-and-compression-codecs.md#json-format) Ayrıntılar için bölümüne), ardından "yapısı belirtmeyin "bölümünde ve rest biçimi ayarları atlayın.

## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).