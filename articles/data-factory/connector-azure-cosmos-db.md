---
title: Veri kopyalama/Data Factory kullanarak Azure Cosmos DB | Microsoft Docs
description: Cosmos DB'den (veya) Azure Cosmos DB için desteklenen kaynak veri depolarından veri kopyalamak nasıl Data Factory ile desteklenen havuz mağazalarının öğrenin.
services: data-factory, cosmosdb
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: multiple
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/28/2018
ms.author: jingwang
ms.openlocfilehash: 6c0921a466864bf2b07711cfcd1eac397c5ced83
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39325362"
---
# <a name="copy-data-to-or-from-azure-cosmos-db-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Cosmos DB için veya veri kopyalama

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-azure-documentdb-connector.md)
> * [Geçerli sürüm](connector-azure-cosmos-db.md)

Bu makalede, kopyalama etkinliği Azure Data Factory'de gelen ve Azure Cosmos DB (SQL API) veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Azure Cosmos DB'den tüm desteklenen havuz veri deposuna veri kopyalamak ya da Azure Cosmos DB için herhangi bir desteklenen kaynak veri deposundan veri kopyalayın. Kopyalama etkinliği tarafından kaynakları/havuz desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Azure Cosmos DB Bağlayıcısı destekler:

- Cosmos DB [SQL API](https://docs.microsoft.com/azure/cosmos-db/documentdb-introduction).
- JSON belgeleri olarak alma/verme- ya da veri kopyalama/tablo dataset ör. SQL veritabanı, CSV dosyaları, vb.

Kopyalamak için belgeler olarak-olduğu için/JSON dosyaları veya başka bir Cosmos DB koleksiyonu bkz [içeri/dışarı aktarma JSON belgelerini](#importexport-json-documents).

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli Azure Cosmos DB'ye tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Azure Cosmos DB bağlı hizmetini aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **CosmosDb**. | Evet |
| bağlantı dizesi |Azure Cosmos DB veritabanına bağlanmak için gereken bilgileri belirtin. Veritabanı bilgileri, örnek olarak aşağıdaki bağlantı dizesinde belirtmek zorunda olmadığını unutmayın. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel ağında bulunuyorsa), Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için veri kümeleri makalesine bakın. Bu bölümde, Azure Cosmos DB veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Verileri Azure BLOB'dan/Azure Cosmos DB kopyalamak için dataset öğesinin type özelliği ayarlamak **DocumentDbCollection**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **DocumentDbCollection** |Evet |
| CollectionName |Cosmos DB belge koleksiyonu adı. |Evet |

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

### <a name="schema-by-data-factory"></a>Veri fabrikası tarafından şeması

Azure Cosmos DB gibi şemasız veri depoları için kopyalama etkinliği şemayı aşağıdaki yollardan biriyle algılar. Bu nedenle, istediğiniz sürece [JSON belgeleri olarak içeri/dışarı aktarma-olan](#importexport-json-documents), veri yapısını belirlemek için en iyi uygulamadır **yapısı** bölümü.

*. Verilerin yapısını kullanarak belirtirseniz **yapısı** veri kümesi tanımında, Data Factory hizmetinin özelliği, şema olarak bu yapı geliştirir. Bir satır bir sütun için bir değer içermiyorsa, bu durumda, bir null değer için sağlanır.
*. Veri yapısını kullanarak belirtmezseniz **yapısı** özelliği veri kümesi tanımında, Data Factory hizmetinin çıkarsar şema verilerin ilk satırı kullanarak. Bu durumda, ilk satır, tam şema içermiyorsa, bazı sütunları kopyalama işleminin sonucunda eksik olacaktır.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Azure Cosmos DB kaynak ve havuz desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-cosmos-db-as-source"></a>Kaynak olarak Azure Cosmos DB

Azure Cosmos DB'den verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **DocumentDbCollectionSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **DocumentDbCollectionSource** |Evet |
| sorgu |Verileri okumak için Cosmos DB sorgusu belirtin.<br/><br/>Örnek: `SELECT c.BusinessEntityID, c.Name.First AS FirstName, c.Name.Middle AS MiddleName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Hayır <br/><br/>Belirtilmezse, yürütülen SQL deyimi: `select <columns defined in structure> from mycollection` |
| nestingSeparator |Belge iç içe geçmiş belirtmek için özel bir karakter ve nasıl flattern için sonuç kümesi.<br/><br/>Örneğin, bir Cosmos DB sorgu bir sonuç döndürürse `"Name": {"First": "John"}`, kopyalama etkinliği tanımlar sütun adı "Name.First" değeri "John" ile nestedSeparator nokta olduğunda. |Hayır (varsayılan değer nokta `.`) |

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

Azure Cosmos DB'ye veri kopyalamak için kopyalama etkinliğine de Havuz türü ayarlayın. **DocumentDbCollectionSink**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuz öğesinin type özelliği ayarlanmalıdır: **DocumentDbCollectionSink** |Evet |
| WriteBehavior |Cosmos DB'ye veri yazmak açıklanmaktadır. İzin verilen değerler: `insert` ve `upsert`.<br/>Davranışını **upsert** aynı kimliğe sahip bir belge zaten mevcutsa; belge değiştirmek üzere yapılması hali ekleyin Aksi takdirde. Not) özgün belgeye veya sütunu eşleyerek belirtilmezse ADF belge için bir kimliği otomatik olarak üretir, upsert çalışır; böylece beklendiği gibi belgenizi emin olmak ihtiyacınız olduğu anlamına gelir "id" olur. |Hayır, varsayılan olan Ekle |
| writeBatchSize | Veri Fabrikası kullanımı [Cosmos DB toplu Yürütücü](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started) Cosmos DB'ye veri yazmak için. "writeBatchSize" her zaman kitaplığa sağladığımız belgeleri boyutunu denetler. Artış deneyebilirsiniz writeBatchSize performansını artırmak için. |Hayır |
| nestingSeparator |Bir iç içe geçmiş belge belirtmek için kaynak sütun adı özel karakterler gereklidir. <br/><br/>Örneğin, `Name.First` çıkış veri kümesinde, Cosmos DB belgesini aşağıdaki JSON yapısında yapısı oluşturur:`"Name": {"First": "[value maps to this column from source]"}` nestedSeparator nokta olduğunda. |Hayır (varsayılan değer nokta `.`) |

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
                "type": "DocumentDbCollectionSink",
                "writeBehavior": "upsert"
            }
        }
    }
]
```

## <a name="importexport-json-documents"></a>İçeri/dışarı aktarma JSON belgeleri

Bu Cosmos DB Bağlayıcısı'nı kullanarak, kolayca yapabilecekleriniz

* JSON belgelerini Cosmos DB, Azure Blob, Azure Data Lake Store ve Azure Data Factory tarafından desteklenen diğer dosya tabanlı depoları gibi çeşitli kaynaklardan veri aktarın.
* JSON belgelerini, çeşitli dosya tabanlı depoları Cosmos DB collecton ' dışarı aktarın.
* Belgeler iki Cosmos DB koleksiyonları arasında kopyalama-olduğu.

Bu tür şemadan kopyalama elde etmek için:

* Kopya veri aracını kullanırken, kontrol **"Dışarı Aktar-JSON dosyaları veya Cosmos DB koleksiyonu"** seçeneği.
* Ne zaman etkinlik yazma, kullanarak belirtme "yapı" (şema olarak da bilinir) bölümünde Cosmos DB veri kümelerindeki ya da "nestingSeparator" özelliği Cosmos DB üzerinde kaynak/havuz kopyalama etkinliği. Ne zaman karşılık gelen dosya depolama kümesindeki JSON dosyaları dışa aktarma / içeri uygulama aktarılırken biçim türünü belirtin "JsonFormat" ve yapılandırma "filePattern" düzgün (bkz [JSON biçimine](supported-file-formats-and-compression-codecs.md#json-format) ayrıntıları bölümü), "yapı belirtmeyin "(şema olarak da bilinir) bölümünde ve rest biçimi ayarları atlayın.

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
