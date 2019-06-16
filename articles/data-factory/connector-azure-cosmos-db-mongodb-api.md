---
title: Azure Cosmos DB API gelen veya MongoDB için Data Factory kullanarak verileri kopyalama | Microsoft Docs
description: Desteklenen kaynak veri depolarından ya da Azure Cosmos DB'nin API'sinden MongoDB için desteklenen havuz mağazalarının Data Factory kullanarak verileri kopyalama öğrenin.
services: data-factory, cosmosdb
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: multiple
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/20/2018
ms.author: jingwang
ms.openlocfilehash: 82418c03039219adedf45828d769d278a14499ff
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61259724"
---
# <a name="copy-data-to-or-from-azure-cosmos-dbs-api-for-mongodb-by-using-azure-data-factory"></a>Azure Cosmos DB API gelen veya MongoDB için Azure Data Factory kullanarak verileri kopyalama

Bu makalede, kopyalama etkinliği Azure Data Factory'de veri MongoDB için gelen ve Azure Cosmos DB'nin API'sini kopyalamak için nasıl kullanılacağını özetlenmektedir. Makaleyi yapılar [Azure veri fabrikasında kopyalama etkinliği](copy-activity-overview.md), kopyalama etkinliği genel bir bakış sunar.

>[!NOTE]
>Bu bağlayıcı yalnızca destek kopyalama veri gönderip buralardan veri Azure Cosmos DB'nin MongoDB API'si. SQL API'si için başvurmak [Cosmos DB SQL API Bağlayıcısı](connector-azure-cosmos-db.md). Diğer API türleri artık desteklenmez.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Verileri Azure Cosmos DB'nin API'SİNDEN MongoDB için hiçbir desteklenen havuz veri deposuna kopyalamak ya da veri herhangi bir desteklenen kaynak veri deposundan Azure Cosmos DB'nin API'si için MongoDB kopyalayın. Kopyalama etkinliği kaynak ve havuz olarak desteklediğini veri listesini depolar için bkz: [desteklenen veri depoları ve biçimler](copy-activity-overview.md#supported-data-stores-and-formats).

Azure Cosmos DB API için MongoDB bağlayıcısını kullanabilirsiniz:

- Verileri kaynak ve hedef [Azure Cosmos DB'nin MongoDB API'si](https://docs.microsoft.com/azure/cosmos-db/mongodb-introduction).
- Azure Cosmos DB yazma **Ekle** veya **upsert**.
- İçeri aktarma ve JSON belgeleri olarak dışarı aktarma- ya da ya da tablolu bir veri kümesine veri kopyalayın. SQL veritabanı ve bir CSV dosyası verilebilir. Kopyalamak için belgeler olarak-JSON dosyalarından veya için veya başka bir Azure Cosmos DB koleksiyonu, içeri aktarma bakın veya JSON belgelerini dışa sağlamaktır.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Azure Cosmos DB'nin MongoDB API'si için özel olan Data Factory varlıkları tanımlamak için kullanabileceğiniz özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Azure Cosmos DB API MongoDB bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** özelliği ayarlanmalıdır **CosmosDbMongoDbApi**. | Evet |
| connectionString |Bağlantı dizesi için MongoDB API'si için Azure Cosmos DB'nin belirtin. Bulabilirsiniz, Azure portalında -> Cosmos DB dikey pencerenize desenini ile birincil veya ikincil bağlantı dizesi -> `mongodb://<cosmosdb-name>:<password>@<cosmosdb-name>.documents.azure.com:10255/?ssl=true&replicaSet=globaldb`. <br/><br />Bu alan olarak işaretlemek bir **SecureString** Data Factory'de güvenle depolamak için türü. Ayrıca [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Evet |
| database | Erişmek istediğiniz veritabanının adı. | Evet |
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

Bölümleri ve veri kümeleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [veri kümeleri ve bağlı hizmetler](concepts-datasets-linked-services.md). MongoDB veri kümesi için Azure Cosmos DB API'si için aşağıdaki özellikleri destekler:

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
            "referenceName": "<Azure Cosmos DB's API for MongoDB linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "collectionName": "<collection name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bu bölümde, MongoDB kaynak ve havuz için Azure Cosmos DB'nin API'sini destekleyen özelliklerin bir listesini sağlar.

Bölümleri ve etkinlikleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md).

### <a name="azure-cosmos-dbs-api-for-mongodb-as-source"></a>Kaynak olarak MongoDB için Azure Cosmos DB API'si

Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kopyalama etkinliği kaynağı özelliği ayarlanmalıdır **CosmosDbMongoDbApiSource**. |Evet |
| filter | Sorgu işleçlerini kullanma seçimi filtresini belirtir. Bir koleksiyondaki tüm belgeleri döndürmek için bu parametreyi atlarsanız veya boş bir belge geçirin ({}). | Hayır |
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
                "referenceName": "<Azure Cosmos DB's API for MongoDB input dataset name>",
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

### <a name="azure-cosmos-dbs-api-for-mongodb-as-sink"></a>Havuz olarak MongoDB için Azure Cosmos DB API'si

Kopyalama etkinliği aşağıdaki özellikler desteklenir **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kopyalama etkinliği havuz özelliği ayarlanmalıdır **CosmosDbMongoDbApiSink**. |Evet |
| writeBehavior |Azure Cosmos DB'ye veri yazmak açıklar. İzin verilen değerler: **Ekle** ve **upsert**.<br/><br/>Davranışını **upsert** aynı Kimliğe sahip bir belge zaten varsa belge değiştirmek üzere; Aksi takdirde, belge ekleyin.<br /><br />**Not**: Bir kimliği, özgün belgenin veya sütun eşlemesi tarafından belirtilmezse veri fabrikası otomatik olarak bir belge için bir kimlik üretir. İçin emin olmanız gerekir, yani **upsert** beklendiği şekilde çalışması için belgeyi bir kimliği vardır. |Hayır<br />(varsayılan değer **Ekle**) |
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

Verileri Azure Cosmos DB'nin API'SİNDEN MongoDB için tablo havuza kopyalama veya tersine, [şema eşleme](copy-activity-schema-and-type-mapping.md#schema-mapping).

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
