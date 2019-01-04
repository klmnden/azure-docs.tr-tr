---
title: Data Factory kullanarak Azure Cosmos DB (MongoDB API'SİYLE) gelen veya veri kopyalama | Microsoft Docs
description: Veya Azure Cosmos DB'den (MongoDB API'SİYLE) desteklenen bir kaynak veri depolarından veri kopyalamak nasıl, Data Factory kullanarak desteklenen havuz mağazalarının öğrenin.
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
ms.date: 12/20/2018
ms.author: jingwang
ms.openlocfilehash: 3f60ffcde43bd1ee43b5dd7d1e86da3a8bf2c521
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2019
ms.locfileid: "54002119"
---
# <a name="copy-data-to-or-from-azure-cosmos-db-mongodb-api-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Cosmos DB (MongoDB API'SİYLE) gelen veya veri kopyalama

Bu makalede, kopyalama etkinliği Azure Data Factory'de gelen ve Azure Cosmos DB (MongoDB API'SİYLE) veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Makaleyi yapılar [Azure veri fabrikasında kopyalama etkinliği](copy-activity-overview.md), kopyalama etkinliği genel bir bakış sunar.

>[!NOTE]
>Bu bağlayıcı yalnızca destek veri gönderip buralardan veri Cosmos DB MongoDB API'si kopyalayın. SQL API'si için başvurmak [Cosmos DB SQL API Bağlayıcısı](connector-azure-cosmos-db.md). Diğer API türleri artık desteklenmez.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Azure Cosmos DB'den (MongoDB API'SİYLE) tüm desteklenen havuz veri deposuna veri kopyalamak ya da veri herhangi bir desteklenen kaynak veri deposundan Azure Cosmos DB'ye (MongoDB API'SİYLE) kopyalayın. Kopyalama etkinliği kaynak ve havuz olarak desteklediğini veri listesini depolar için bkz: [desteklenen veri depoları ve biçimler](copy-activity-overview.md#supported-data-stores-and-formats).

Azure Cosmos DB (MongoDB API'SİYLE) bağlayıcıya kullanabilirsiniz:

- Gelen ve Azure Cosmos DB veri kopyalama [MongoDB API'si](https://docs.microsoft.com/azure/cosmos-db/mongodb-introduction).
- Azure Cosmos DB yazma **Ekle** veya **upsert**.
- İçeri aktarma ve JSON belgeleri olarak dışarı aktarma- ya da ya da tablolu bir veri kümesine veri kopyalayın. SQL veritabanı ve bir CSV dosyası verilebilir. Kopyalamak için belgeler olarak-için veya JSON dosyaları veya için veya başka bir Azure Cosmos DB koleksiyonu, bkz: [JSON belgelerini içeri veya dışarı aktarmayı](#importexport-json-documents).

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Azure Cosmos DB (MongoDB API'SİYLE) için belirli bir Data Factory varlıkları tanımlamak için kullanabileceğiniz özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Azure Cosmos DB (MongoDB API'SİYLE) bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** özelliği ayarlanmalıdır **CosmosDbMongoDbApi**. | Evet |
| bağlantı dizesi |Azure Cosmos DB MongoDB API'si için bağlantı dizesini belirtin. Bulabilirsiniz, Azure portalında -> Cosmos DB dikey pencerenize desenini ile birincil veya ikincil bağlantı dizesi -> `mongodb://<cosmosdb-name>:<password>@<cosmosdb-name>.documents.azure.com:10255/?ssl=true&replicaSet=globaldb`. <br/><br />Bu alan olarak işaretlemek bir **SecureString** Data Factory'de güvenle depolamak için türü. Ayrıca [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Evet |
| veritabanı | Erişmek istediğiniz veritabanının adı. | Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel bir ağda yer alıyorsa) Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Bu özellik belirtilmezse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Hayır |

**Örnek**

```json
{
    "name": "CosmosDbMongoDBAPILinkedService",
    "properties": {
        "type": "CosmosDbMongoDbApi",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "mongodb://<cosmosdb-name>:<password>@<cosmosdb-name>.documents.azure.com:10255/?ssl=true&replicaSet=globaldb"
            },
            "database": "myDatabase"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [veri kümeleri ve bağlı hizmetler](concepts-datasets-linked-services.md). Azure Cosmos DB (MongoDB API'SİYLE) veri kümesi için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kümesinin özelliği ayarlanmalıdır **CosmosDbMongoDbApiCollection**. |Evet |
| collectionName |Azure Cosmos DB koleksiyonu adı. |Evet |

**Örnek**

```json
{
    "name": "CosmosDbMongoDBAPIDataset",
    "properties": {
        "type": "CosmosDbMongoDbApiCollection",
        "linkedServiceName":{
            "referenceName": "<Azure Cosmos DB MongoDB API linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "collectionName": "<collection name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bu bölümde, Azure Cosmos DB (MongoDB API'SİYLE) kaynak ve havuz destekleyen özelliklerin bir listesini sağlar.

Bölümleri ve etkinlikleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md).

### <a name="azure-cosmos-db-mongodb-api-as-source"></a>Azure Cosmos DB (MongoDB API'SİYLE) kaynak olarak

Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kopyalama etkinliği kaynağı özelliği ayarlanmalıdır **CosmosDbMongoDbApiSource**. |Evet |
| filtre | Sorgu işleçlerini kullanma seçimi filtresini belirtir. Bir koleksiyondaki tüm belgeleri döndürmek için bu parametreyi atlarsanız veya boş bir belge geçirin ({}). | Hayır |
| cursorMethods.project | Belgelerde yansıtma için döndürülecek alanları belirtir. Tüm alanları eşleşen belgeleri döndürmek için bu parametreyi atlayın. | Hayır |
| cursorMethods.sort | Sorgu eşleşen belgelerin döndüren sırasını belirtir. Başvurmak [cursor.sort()](https://docs.mongodb.com/manual/reference/method/cursor.sort/#cursor.sort). | Hayır |
| cursorMethods.limit | Sunucu belgeleri maksimum sayısını belirtir. Başvurmak [cursor.limit()](https://docs.mongodb.com/manual/reference/method/cursor.limit/#cursor.limit).  | Hayır | 
| cursorMethods.skip | Atlamak için belgeler ve sonuçları döndürmek MongoDB başladığı belirtir. Başvurmak [cursor.skip()](https://docs.mongodb.com/manual/reference/method/cursor.skip/#cursor.skip). | Hayır |
| batchSize | Her toplu iş yanıt MongoDB örneğine döndürmek için belge sayısını belirtir. Çoğu durumda, toplu iş boyutu değiştirme kullanıcı veya uygulamanın etkilemez. Cosmos DB sınırları her toplu işin aşamaz belgelerin boyutunun batchSize sayının toplamı olan boyutu, 40MB, bu değer kadar Azalt Belge boyutunuz büyük olması. | Hayır<br/>(varsayılan değer **100**) |

>[!TIP]
>BSON belgede tüketen ADF Destek **katı mod**. Filtre sorgunuzla Kabuk modu yerine katı modda olduğundan emin olun. Daha fazla açıklama şu yolda bulunabilir: [MongoDB el ile](https://docs.mongodb.com/manual/reference/mongodb-extended-json/index.html).

**Örnek**

```json
"activities":[
    {
        "name": "CopyFromCosmosDBMongoDBAPI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Cosmos DB MongoDB API input dataset name>",
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
                "type": "CosmosDbMongoDbApiSource",
                "filter": "{datetimeData: {$gte: ISODate(\"2018-12-11T00:00:00.000Z\"),$lt: ISODate(\"2018-12-12T00:00:00.000Z\")}, _id: ObjectId(\"5acd7c3d0000000000000000\") }",
                "cursorMethods": {
                    "project": "{ _id : 1, name : 1, age: 1, datetimeData: 1 }",
                    "sort": "{ age : 1 }",
                    "skip": 3,
                    "limit": 3
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-cosmos-db-mongodb-api-as-sink"></a>Azure Cosmos DB (MongoDB API'SİYLE) havuz olarak

Kopyalama etkinliği aşağıdaki özellikler desteklenir **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kopyalama etkinliği havuz özelliği ayarlanmalıdır **CosmosDbMongoDbApiSink**. |Evet |
| WriteBehavior |Azure Cosmos DB'ye veri yazmak açıklar. İzin verilen değerler: **Ekle** ve **upsert**.<br/><br/>Davranışını **upsert** aynı Kimliğe sahip bir belge zaten varsa belge değiştirmek üzere; Aksi takdirde, belge ekleyin.<br /><br />**Not**: Bir kimliği, özgün belgenin veya sütun eşlemesi tarafından belirtilmezse veri fabrikası otomatik olarak bir belge için bir kimlik üretir. İçin emin olmanız gerekir, yani **upsert** beklendiği şekilde çalışması için belgeyi bir kimliği vardır. |Hayır<br />(varsayılan değer **Ekle**) |
| writeBatchSize | **WriteBatchSize** özelliği, her toplu yazmak için belgeler boyutunu denetler. Değerini artırmayı deneyin **writeBatchSize** performansını ve değeri, azalan Belge boyutunuz büyük olması. |Hayır<br />(varsayılan değer **10.000**) |
| writeBatchTimeout | Batch için bekleme süresi, işlemin zaman aşımına uğramadan önce tamamlanmasını ekleyin. İzin verilen timespan değerdir. | Hayır<br/>(varsayılan değer **00:30:00** - 30 dakika) |

**Örnek**

```json
"activities":[
    {
        "name": "CopyToCosmosDBMongoDBAPI",
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
                "type": "CosmosDbMongoDbApiSink",
                "writeBehavior": "upsert"
            }
        }
    }
]
```

>[!TIP]
>JSON belgeleri olarak içeri aktarmak için-olan başvurmak için [içeri aktarma veya dışarı aktarma JSON belgelerini](#import-or-export-json-documents) bölümünde; tablo şeklinde veri kopyalamak için bkz [şema eşleme](#schema-mapping).

## <a name="import-or-export-json-documents"></a>JSON belgelerini içeri veya dışarı aktarma

Bu Azure Cosmos DB Bağlayıcısı için kolayca kullanabilirsiniz:

* JSON belgeleri için Azure Cosmos DB, Azure Blob Depolama, Azure Data Lake Store ve Azure Data Factory destekleyen diğer dosya tabanlı depoları da dahil olmak üzere çeşitli kaynaklardan içeri aktarın.
* JSON belgeleri bir Azure Cosmos DB koleksiyonundan çeşitli dosya tabanlı depoları dışarı aktarın.
* Belgeler iki Azure Cosmos DB koleksiyonları arasında kopyalama-olduğu.

Bu tür şemadan kopyalama elde etmek için "yapı" Atla (olarak da bilinir *şema*) bölümünde veri kümesini ve şema eşleme kopyalama etkinliğindeki.

## <a name="schema-mapping"></a>Şema eşleme

Verileri Cosmos DB MongoDB API'SİNDEN tablosal havuza kopyalama veya tersine, [şema eşleme](copy-activity-schema-and-type-mapping.md#schema-mapping).

Yazma Cosmos DB içine için özel olarak Cosmos DB doğru nesne Kimliğine sahip kaynak verilerinizden doldurmak emin olmak için örneğin, SQL veritabanı tablosunda bir "id" sütununuz ve MongoDB belge kimliği olarak Ekle/upsert, değerini kullanmak istediğiniz , MongoDB katı mod tanımına göre uygun şema eşlemesi ayarlamanız gerekir (`_id.$oid`) aşağıdaki gibi:

![MongoDB havuzunda eşleme kimliği](./media/connector-azure-cosmos-db-mongodb-api/map-id-in-mongodb-sink.png)

Kopyalamadan sonra BSON objectID altında Etkinlik yürütme havuzunda oluşturulur:

```json
{
    "_id": ObjectId("592e07800000000000000000")
}
``` 

## <a name="next-steps"></a>Sonraki adımlar

Kopyalama etkinliği kaynak olarak destekler ve şu havuzlar Azure Data Factory'de veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
