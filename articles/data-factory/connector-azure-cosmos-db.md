---
title: Data Factory kullanarak Azure Cosmos DB'den (SQL API'si) ya da veri kopyalama | Microsoft Docs
description: Azure Cosmos DB'den (SQL API'si) ya da desteklenen kaynak veri depolarından veri kopyalamak desteklenen havuz mağazalarının Data Factory kullanarak öğrenin.
services: data-factory, cosmosdb
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: multiple
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: jingwang
ms.openlocfilehash: eca5e4cc96996c35e7c2181746cdb3de2e5a602c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61259526"
---
# <a name="copy-data-to-or-from-azure-cosmos-db-sql-api-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Cosmos DB'den (SQL API'si) ya da veri kopyalama

> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](v1/data-factory-azure-documentdb-connector.md)
> * [Geçerli sürüm](connector-azure-cosmos-db.md)

Bu makalede, kopyalama etkinliği Azure Data Factory'de gelen ve Azure Cosmos DB (SQL API) veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Makaleyi yapılar [Azure veri fabrikasında kopyalama etkinliği](copy-activity-overview.md), kopyalama etkinliği genel bir bakış sunar.

>[!NOTE]
>Bu bağlayıcı yalnızca destek veri gönderip buralardan veri Cosmos DB SQL API kopyalayın. MongoDB API'si için başvurmak [Azure Cosmos DB'nin MongoDB API'si için bağlayıcı](connector-azure-cosmos-db-mongodb-api.md). Diğer API türleri artık desteklenmez.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Azure Cosmos DB'den (SQL API) tüm desteklenen havuz veri deposuna veri kopyalamak ya da veri herhangi bir desteklenen kaynak veri deposundan Azure Cosmos DB'ye (SQL API) kopyalayın. Kopyalama etkinliği kaynak ve havuz olarak desteklediğini veri listesini depolar için bkz: [desteklenen veri depoları ve biçimler](copy-activity-overview.md#supported-data-stores-and-formats).

Azure Cosmos DB (SQL API) bağlayıcıya kullanabilirsiniz:

- Gelen ve Azure Cosmos DB veri kopyalama [SQL API](https://docs.microsoft.com/azure/cosmos-db/documentdb-introduction).
- Azure Cosmos DB yazma **Ekle** veya **upsert**.
- İçeri aktarma ve JSON belgeleri olarak dışarı aktarma- ya da ya da tablolu bir veri kümesine veri kopyalayın. SQL veritabanı ve bir CSV dosyası verilebilir. Kopyalamak için belgeler olarak-JSON dosyalarından veya için veya başka bir Azure Cosmos DB koleksiyonu, içeri aktarma bakın veya JSON belgelerini dışa sağlamaktır.

Veri Fabrikası ile tümleştirilir [Azure Cosmos DB toplu Yürütücü Kitaplığı](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started) Azure Cosmos DB'ye yazdığınızda, en iyi performansı sağlamak için.

> [!TIP]
> [Veri geçişi video](https://youtu.be/5-SRNiC_qOU) Azure Cosmos DB için Azure Blob depolamadan veri kopyalama adımlarında size rehberlik yapacaktır. Video ayrıca genel olarak Azure Cosmos DB'ye veri almak için performans ayarlama konuları açıklanır.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Azure Cosmos DB'ye (SQL API) belirli bir Data Factory varlıkları tanımlamak için kullanabileceğiniz özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Azure Cosmos DB (SQL API) bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** özelliği ayarlanmalıdır **CosmosDb**. | Evet |
| connectionString |Azure Cosmos DB veritabanına bağlanmak için gereken bilgileri belirtin.<br />**Not**: Aşağıdaki örneklerde gösterildiği gibi bağlantı dizesinde veritabanı bilgileri belirtmeniz gerekir. <br/>Bu alan, Data Factory'de güvenle depolamak için bir SecureString olarak işaretleyin. Hesap anahtarı Azure Key Vault ve çekme koyabilirsiniz `accountKey` yapılandırma bağlantı dizesini dışında. Aşağıdaki örneklere bakın ve [kimlik bilgilerini Azure Key Vault'ta Store](store-credentials-in-key-vault.md) daha fazla ayrıntı içeren makalesi. |Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel bir ağda yer alıyorsa) Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Bu özellik belirtilmezse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Hayır |

**Örnek**

```json
{
    "name": "CosmosDbSQLAPILinkedService",
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

**Örnek: hesap anahtarı Azure Key Vault'ta depolama**

```json
{
    "name": "CosmosDbSQLAPILinkedService",
    "properties": {
        "type": "CosmosDb",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "AccountEndpoint=<EndpointUrl>;Database=<Database>"
            },
            "accountKey": { 
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

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bu bölümde, Azure Cosmos DB (SQL API) veri kümesini destekleyen özelliklerin bir listesini sağlar. 

Bölümleri ve veri kümeleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [veri kümeleri ve bağlı hizmetler](concepts-datasets-linked-services.md). 

Azure Cosmos DB'ye (SQL API'si) ya da veri kopyalamak için ayarlanmış **türü** veri kümesine özelliği **DocumentDbCollection**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kümesinin özelliği ayarlanmalıdır **DocumentDbCollection**. |Evet |
| collectionName |Azure Cosmos DB belge koleksiyonu adı. |Evet |

**Örnek**

```json
{
    "name": "CosmosDbSQLAPIDataset",
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

Bu bölümde, Azure Cosmos DB (SQL API) kaynak ve havuz destekleyen özelliklerin bir listesini sağlar.

Bölümleri ve etkinlikleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md).

### <a name="azure-cosmos-db-sql-api-as-source"></a>Azure Cosmos DB (SQL API) kaynak olarak

Azure Cosmos DB'den (SQL API) veri kopyalamak için ayarlanmış **kaynak** kopyalama etkinliğindeki türü **DocumentDbCollectionSource**. 

Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kopyalama etkinliği kaynağı özelliği ayarlanmalıdır **DocumentDbCollectionSource**. |Evet |
| query |Verileri okumak için Azure Cosmos DB sorgusu belirtin.<br/><br/>Örnek:<br /> `SELECT c.BusinessEntityID, c.Name.First AS FirstName, c.Name.Middle AS MiddleName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Hayır <br/><br/>Belirtilmemişse, bu SQL deyimi yürütülür: `select <columns defined in structure> from mycollection` |
| nestingSeparator |Belge iç içe geçmiş gösteren özel bir karakter ve sonuç kümesini düzleştirmek öğrenin.<br/><br/>Örneğin, bir Azure Cosmos DB sorgusu sonuç döndürürse `"Name": {"First": "John"}`, kopyalama etkinliği tanımlayan sütun adı olarak `Name.First`, "John" değerine sahip olduğunda **nestedSeparator** değer **.** (nokta). |Hayır<br />(varsayılan değer **.** (nokta)) |

**Örnek**

```json
"activities":[
    {
        "name": "CopyFromCosmosDBSQLAPI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Cosmos DB SQL API input dataset name>",
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

### <a name="azure-cosmos-db-sql-api-as-sink"></a>Havuz olarak Azure Cosmos DB (SQL API)

Azure Cosmos DB'ye (SQL API) veri kopyalamak için ayarlanmış **havuz** türü için kopyalama etkinliğindeki **DocumentDbCollectionSink**. 

Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kopyalama etkinliği havuz özelliği ayarlanmalıdır **DocumentDbCollectionSink**. |Evet |
| writeBehavior |Azure Cosmos DB'ye veri yazmak açıklar. İzin verilen değerler: **Ekle** ve **upsert**.<br/><br/>Davranışını **upsert** aynı Kimliğe sahip bir belge zaten varsa belge değiştirmek üzere; Aksi takdirde, belge ekleyin.<br /><br />**Not**: Bir kimliği, özgün belgenin veya sütun eşlemesi tarafından belirtilmezse veri fabrikası otomatik olarak bir belge için bir kimlik üretir. İçin emin olmanız gerekir, yani **upsert** beklendiği şekilde çalışması için belgeyi bir kimliği vardır. |Hayır<br />(varsayılan değer **Ekle**) |
| writeBatchSize | Veri fabrikasının kullandığı [Azure Cosmos DB toplu Yürütücü Kitaplığı](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started) Azure Cosmos DB'ye veri yazmak için. **WriteBatchSize** özelliği sağlayan ADF kitaplığa belgeleri boyutunu denetler. Değerini artırmayı deneyin **writeBatchSize** performansı ve değeri, azalan artırmak için belgenizin durdurulmasını büyük boyut - ipuçlarına bakın. |Hayır<br />(varsayılan değer **10.000**) |
| nestingSeparator |Bir özel karakter **kaynak** iç içe geçmiş bir belge gerekli olmadığını gösteren bir sütun adı. <br/><br/>Örneğin, `Name.First` çıkış veri kümesinde, Azure Cosmos DB aşağıdaki JSON yapısında yapısı oluşturur ne zaman belge **nestedSeparator** olduğu **.** (nokta): `"Name": {"First": "[value maps to this column from source]"}`  |Hayır<br />(varsayılan değer **.** (nokta)) |

>[!TIP]
>Cosmos DB tek isteğin boyutunu 2 MB ile sınırlar. İstek boyutu formüldür tek belge boyutu = * yazma toplu iş boyutu. Hata bildiren ulaşırsanız **"istek boyutu çok büyük."** , **azaltmak `writeBatchSize` değer** kopyalama havuz yapılandırması.

**Örnek**

```json
"activities":[
    {
        "name": "CopyToCosmosDBSQLAPI",
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

Bu Azure Cosmos DB (SQL API) bağlayıcıya bir kolayca kullanabilirsiniz:

* JSON belgeleri için Azure Cosmos DB, Azure Blob Depolama, Azure Data Lake Store ve Azure Data Factory destekleyen diğer dosya tabanlı depoları da dahil olmak üzere çeşitli kaynaklardan içeri aktarın.
* JSON belgeleri bir Azure Cosmos DB koleksiyonundan çeşitli dosya tabanlı depoları dışarı aktarın.
* Belgeler iki Azure Cosmos DB koleksiyonları arasında kopyalama-olduğu.

Şemadan kopyalama elde etmek için:

* Veri kopyalama aracını kullandığınızda, seçin **Dışarı Aktar-JSON dosyaları veya Cosmos DB koleksiyonu** seçeneği.
* Etkinlik yazma kullandığınızda, belirtmeyin **yapısı** (olarak da adlandırılan *şema*) bölümünde Azure Cosmos DB veri kümesi. Ayrıca, belirtmeyin **nestingSeparator** Azure Cosmos DB kaynak veya havuz olarak kopyalama etkinliği özelliği. Alınacak veya JSON dosyalarını dışarı aktarma, veri kümesi karşılık gelen dosyasında depolamak, belirtin **biçimi** olarak yazın **JsonFormat** ve yapılandırma **filePattern** olarak açıklanan [JSON biçimine](supported-file-formats-and-compression-codecs.md#json-format) bölümü. Ardından, belirtmeyin **yapısı** bölümünde ve biçimi ayarları geri kalanını atlayın.

## <a name="next-steps"></a>Sonraki adımlar

Kopyalama etkinliği kaynak olarak destekler ve şu havuzlar Azure Data Factory'de veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
