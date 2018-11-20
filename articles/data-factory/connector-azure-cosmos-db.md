---
title: Data Factory kullanarak Azure Cosmos DB'den ya da veri kopyalama | Microsoft Docs
description: Desteklenen kaynak veri depolarından ya da Azure Cosmos DB'den desteklenen havuz mağazalarının Data Factory kullanarak verileri kopyalama öğrenin.
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
ms.date: 11/19/2018
ms.author: jingwang
ms.openlocfilehash: c10a933f371bfc84b863413134f2fdf5ff9c0e34
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52161846"
---
# <a name="copy-data-to-or-from-azure-cosmos-db-by-using-azure-data-factory"></a>Azure Data Factory kullanarak veya Azure Cosmos DB'den verileri kopyalayın

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-azure-documentdb-connector.md)
> * [Geçerli sürüm](connector-azure-cosmos-db.md)

Bu makalede, kopyalama etkinliği Azure Data Factory'de gelen ve Azure Cosmos DB (SQL API) veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Makaleyi yapılar [Azure veri fabrikasında kopyalama etkinliği](copy-activity-overview.md), kopyalama etkinliği genel bir bakış sunar.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Azure Cosmos DB'den tüm desteklenen havuz veri deposuna veri kopyalamak ya da Azure Cosmos DB için herhangi bir desteklenen kaynak veri deposundan veri kopyalayın. Kopyalama etkinliği kaynak ve havuz olarak desteklediğini veri listesini depolar için bkz: [desteklenen veri depoları ve biçimler](copy-activity-overview.md#supported-data-stores-and-formats).

Azure Cosmos DB Bağlayıcısı kullanabilirsiniz:

- Gelen ve Azure Cosmos DB veri kopyalama [SQL API](https://docs.microsoft.com/azure/cosmos-db/documentdb-introduction).
- Azure Cosmos DB yazma **Ekle** veya **upsert**.
- İçeri aktarma ve JSON belgeleri olarak dışarı aktarma- ya da ya da tablolu bir veri kümesine veri kopyalayın. SQL veritabanı ve bir CSV dosyası verilebilir. Kopyalamak için belgeler olarak-için veya JSON dosyaları veya için veya başka bir Azure Cosmos DB koleksiyonu, bkz: [JSON belgelerini içeri veya dışarı aktarmayı](#importexport-json-documents).

Veri Fabrikası ile tümleştirilir [Azure Cosmos DB toplu Yürütücü Kitaplığı](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started) Azure Cosmos DB'ye yazdığınızda, en iyi performansı sağlamak için.

> [!TIP]
> [Veri geçişi video](https://youtu.be/5-SRNiC_qOU) Azure Cosmos DB için Azure Blob depolamadan veri kopyalama adımlarında size rehberlik yapacaktır. Video ayrıca genel olarak Azure Cosmos DB'ye veri almak için performans ayarlama konuları açıklanır.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Azure Cosmos DB'ye özel olan Data Factory varlıkları tanımlamak için kullanabileceğiniz özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Azure Cosmos DB bağlı hizmetini aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** özelliği ayarlanmalıdır **CosmosDb**. | Evet |
| bağlantı dizesi |Azure Cosmos DB veritabanına bağlanmak için gereken bilgileri belirtin.<br /><br />**Not**: veritabanı bilgisi bağlantı dizesinde aşağıdaki örneklerde gösterildiği gibi belirtmeniz gerekir. Bu alan olarak işaretlemek bir **SecureString** Data Factory'de güvenle depolamak için türü. Ayrıca [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel bir ağda yer alıyorsa) Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Bu özellik belirtilmezse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Hayır |

**Örnek**

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

Bu bölümde, Azure Cosmos DB veri kümesini destekleyen özelliklerin bir listesini sağlar. 

Bölümleri ve veri kümeleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [veri kümeleri ve bağlı hizmetler](concepts-datasets-linked-services.md). 

Ya da Azure Cosmos DB'ye veri kopyalamak için ayarlanmış **türü** veri kümesine özelliği **DocumentDbCollection**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kümesinin özelliği ayarlanmalıdır **DocumentDbCollection**. |Evet |
| collectionName |Azure Cosmos DB belge koleksiyonu adı. |Evet |

**Örnek**

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

Azure Cosmos DB gibi şemasız veri depoları için kopyalama etkinliği aşağıdaki listede açıklandığı şekilde şemayı algılar. İstediğiniz sürece [içeri veya dışarı aktarma JSON belgeleri olarak-olan](#import-or-export-json-documents), veri yapısını belirlemek için en iyi uygulamadır **yapısı** bölümü.

* Verilerin yapısını kullanarak belirtirseniz **yapısı** özelliği Data Factory veri kümesi tanımında, bu yapı şema olarak geliştirir. 

    Bir satır bir sütun için bir değer içermiyorsa, bir null değer sütun değeri için sağlanır.
* Kullanarak verilerin yapısını belirtmezseniz **yapısı** özelliği veri kümesi tanımında, Data Factory hizmetinin çıkarsar şema verilerin ilk satırı kullanarak. 

    İlk satır, tam şema içermiyorsa, bazı sütunları kopyalama işleminin sonucunda eksik olacaktır.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bu bölümde, Azure Cosmos DB kaynak ve havuz destekleyen özelliklerin bir listesini sağlar.

Bölümleri ve etkinlikleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md).

### <a name="azure-cosmos-db-as-source"></a>Kaynak olarak Azure Cosmos DB

Azure Cosmos DB'den verileri kopyalamak için ayarlanmış **kaynak** türü için kopyalama etkinliğindeki **DocumentDbCollectionSource**. 

Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kopyalama etkinliği kaynağı özelliği ayarlanmalıdır **DocumentDbCollectionSource**. |Evet |
| sorgu |Verileri okumak için Azure Cosmos DB sorgusu belirtin.<br/><br/>Örnek:<br /> `SELECT c.BusinessEntityID, c.Name.First AS FirstName, c.Name.Middle AS MiddleName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Hayır <br/><br/>Belirtilmemişse, bu SQL deyimi yürütülür: `select <columns defined in structure> from mycollection` |
| nestingSeparator |Belge iç içe geçmiş gösteren özel bir karakter ve sonuç kümesini düzleştirmek öğrenin.<br/><br/>Örneğin, bir Azure Cosmos DB sorgusu sonuç döndürürse `"Name": {"First": "John"}`, kopyalama etkinliği tanımlayan sütun adı olarak `Name.First`, "John" değerine sahip olduğunda **nestedSeparator** değer **.** (nokta). |Hayır<br />(varsayılan değer **.** (nokta)) |

**Örnek**

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

Azure Cosmos DB'ye veri kopyalamak için ayarlanmış **havuz** türü için kopyalama etkinliğindeki **DocumentDbCollectionSink**. 

Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kopyalama etkinliği havuz özelliği ayarlanmalıdır **DocumentDbCollectionSink**. |Evet |
| WriteBehavior |Azure Cosmos DB'ye veri yazmak açıklar. İzin verilen değerler: **Ekle** ve **upsert**.<br/><br/>Davranışını **upsert** aynı Kimliğe sahip bir belge zaten varsa belge değiştirmek üzere; Aksi takdirde, belge ekleyin.<br /><br />**Not**: Data Factory bir kimliği, özgün belgenin veya sütun eşlemesi tarafından belirtilmezse bir belge için bir kimliği otomatik olarak oluşturur. İçin emin olmanız gerekir, yani **upsert** beklendiği şekilde çalışması için belgeyi bir kimliği vardır. |Hayır<br />(varsayılan değer **Ekle**) |
| writeBatchSize | Veri fabrikasının kullandığı [Azure Cosmos DB toplu Yürütücü Kitaplığı](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started) Azure Cosmos DB'ye veri yazmak için. **WriteBatchSize** özellik kitaplığa sağladığımız belgeleri boyutunu denetler. Değerini artırmayı deneyin **writeBatchSize** performansı ve değeri, azalan artırmak için belgenizin durdurulmasını büyük boyut - ipuçlarına bakın. |Hayır<br />(varsayılan değer **10.000**) |
| nestingSeparator |Bir özel karakter **kaynak** iç içe geçmiş bir belge gerekli olmadığını gösteren bir sütun adı. <br/><br/>Örneğin, `Name.First` çıkış veri kümesinde, Azure Cosmos DB aşağıdaki JSON yapısında yapısı oluşturur ne zaman belge **nestedSeparator** olduğu **.** (nokta): `"Name": {"First": "[value maps to this column from source]"}`  |Hayır<br />(varsayılan değer **.** (nokta)) |

>[!TIP]
>Cosmos DB tek isteğin boyutunu 2 MB ile sınırlar. İstek boyutu formüldür tek belge boyutu = * yazma toplu iş boyutu. Hata bildiren ulaşırsanız **"istek boyutu çok büyük."** , **azaltmak `writeBatchSize` değer** kopyalama havuz yapılandırması.

**Örnek**

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

## <a name="import-or-export-json-documents"></a>JSON belgelerini içeri veya dışarı aktarma

Bu Azure Cosmos DB Bağlayıcısı için kolayca kullanabilirsiniz:

* JSON belgeleri için Azure Cosmos DB, Azure Blob Depolama, Azure Data Lake Store ve Azure Data Factory destekleyen diğer dosya tabanlı depoları da dahil olmak üzere çeşitli kaynaklardan içeri aktarın.
* JSON belgeleri bir Azure Cosmos DB koleksiyonundan çeşitli dosya tabanlı depoları dışarı aktarın.
* Belgeler iki Azure Cosmos DB koleksiyonları arasında kopyalama-olduğu.

Şemadan kopyalama elde etmek için:

* Veri kopyalama aracını kullandığınızda, seçin **Dışarı Aktar-JSON dosyaları veya Cosmos DB koleksiyonu** seçeneği.
* Etkinlik yazma kullandığınızda, belirtmeyin **yapısı** (olarak da adlandırılan *şema*) bölümünde Azure Cosmos DB veri kümesi. Ayrıca, belirtmeyin **nestingSeparator** Azure Cosmos DB kaynak veya havuz olarak kopyalama etkinliği özelliği. Alınacak veya JSON dosyalarını dışarı aktarma, veri kümesi karşılık gelen dosyasında depolamak, belirtin **biçimi** olarak yazın **JsonFormat** ve yapılandırma **filePattern** olarak açıklanan [JSON biçimine](supported-file-formats-and-compression-codecs.md#json-format) bölümü. Ardından, belirtmeyin **yapısı** bölümünde ve biçimi ayarları geri kalanını atlayın.

## <a name="next-steps"></a>Sonraki adımlar

Kopyalama etkinliği kaynak olarak destekler ve şu havuzlar Azure Data Factory'de veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
