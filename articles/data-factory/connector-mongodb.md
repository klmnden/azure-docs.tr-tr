---
title: Azure Data Factory kullanarak Mongodb'deki verileri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına Mongo DB bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: WenJason
manager: digimobile
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
origin.date: 12/20/2018
ms.date: 04/22/2019
ms.author: v-jay
ms.openlocfilehash: ca6040bb74839f30a2f1b13297f6037f05240c67
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61400449"
---
# <a name="copy-data-from-mongodb-using-azure-data-factory"></a>Azure Data Factory kullanarak MongoDB verilerini kopyalama

Bu makalede, kopyalama etkinliği Azure Data Factory'de bir MongoDB veritabanına veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

>[!IMPORTANT]
>ADF sürümü bu yeni MongoDB bağlayıcısını daha iyi yerel MongoDB destek sağlar. Çözümünüzdeki olarak desteklenen önceki MongoDB bağlayıcısını kullanıyorsanız-olan geriye dönük uyumluluk için başvurmak [MongoDB bağlayıcısını (eski)](connector-mongodb-legacy.md) makale.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna verileri bir MongoDB veritabanından kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu MongoDB bağlayıcısını destekler **3.4 kadar sürümleri**.

## <a name="prerequisites"></a>Önkoşullar

Genel olarak erişilebilir değil bir MongoDB veritabanına veri kopyalamak için şirket içinde barındırılan tümleştirme çalışma zamanını oluşturan gerekir. Bkz: [şirket içinde barındırılan tümleştirme çalışma zamanı](create-self-hosted-integration-runtime.md) daha fazla bilgi edinmek için makaleyi.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli MongoDB bağlayıcısını tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

MongoDB bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Type özelliği ayarlanmalıdır: **MongoDbV2** |Evet |
| connectionString |Örneğin, MongoDB bağlantı dizesini belirtin `mongodb://[username:password@]host[:port][/[database][?options]]`. Başvurmak [MongoDB bağlantı dizesi el ile](https://docs.mongodb.com/manual/reference/connection-string/) daha fazla ayrıntı için. <br/><br />Bu alan olarak işaretlemek bir **SecureString** Data Factory'de güvenle depolamak için türü. Ayrıca [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Evet |
| veritabanı | Erişmek istediğiniz veritabanının adı. | Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz genel olarak erişilebilir değilse), şirket içinde barındırılan tümleştirme çalışma zamanı veya Azure Integration Runtime kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

**Örnek:**

```json
{
    "name": "MongoDBLinkedService",
    "properties": {
        "type": "MongoDbV2",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "mongodb://[username:password@]host[:port][/[database][?options]]"
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

Bölümleri ve veri kümeleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [veri kümeleri ve bağlı hizmetler](concepts-datasets-linked-services.md). MongoDB veri kümesi için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **MongoDbV2Collection** | Evet |
| collectionName |MongoDB veritabanındaki koleksiyonun adı. |Evet |

**Örnek:**

```json
{
    "name": "MongoDbDataset",
    "properties": {
        "type": "MongoDbV2Collection",
        "linkedServiceName": {
            "referenceName": "<MongoDB linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "collectionName": "<Collection name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, MongoDB kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="mongodb-as-source"></a>Kaynak olarak MongoDB

Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **MongoDbV2Source** | Evet |
| filtre | Sorgu işleçlerini kullanma seçimi filtresini belirtir. Bir koleksiyondaki tüm belgeleri döndürmek için bu parametreyi atlarsanız veya boş bir belge geçirin ({}). | Hayır |
| cursorMethods.project | Belgelerde yansıtma için döndürülecek alanları belirtir. Tüm alanları eşleşen belgeleri döndürmek için bu parametreyi atlayın. | Hayır |
| cursorMethods.sort | Sorgu eşleşen belgelerin döndüren sırasını belirtir. Başvurmak [cursor.sort()](https://docs.mongodb.com/manual/reference/method/cursor.sort/#cursor.sort). | Hayır |
| cursorMethods.limit | Sunucu belgeleri maksimum sayısını belirtir. Başvurmak [cursor.limit()](https://docs.mongodb.com/manual/reference/method/cursor.limit/#cursor.limit).  | Hayır |
| cursorMethods.skip | Atlamak için belgeler ve sonuçları döndürmek MongoDB başladığı belirtir. Başvurmak [cursor.skip()](https://docs.mongodb.com/manual/reference/method/cursor.skip/#cursor.skip). | Hayır |
| batchSize | Her toplu iş yanıt MongoDB örneğine döndürmek için belge sayısını belirtir. Çoğu durumda, toplu iş boyutu değiştirme kullanıcı veya uygulamanın etkilemez. Cosmos DB sınırları her toplu işin aşamaz belgelerin boyutunun batchSize sayının toplamı olan boyutu, 40MB, bu değer kadar Azalt Belge boyutunuz büyük olması. | Hayır<br/>(varsayılan değer **100**) |

>[!TIP]
>BSON belgede tüketen ADF Destek **katı mod**. Filtre sorgunuzla Kabuk modu yerine katı modda olduğundan emin olun. Daha fazla açıklama şu yolda bulunabilir: [MongoDB el ile](https://docs.mongodb.com/manual/reference/mongodb-extended-json/index.html).

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromMongoDB",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<MongoDB input dataset name>",
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
                "type": "MongoDbV2Source",
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

## <a name="export-json-documents-as-is"></a>JSON belgeleri olarak dışarı aktar-olup

JSON belgeleri olarak dışarı aktarmak için bu MongoDB bağlayıcısını kullanabilirsiniz-çeşitli dosya tabanlı depoları veya Azure Cosmos DB için MongoDB koleksiyondan kullanmaktır. Bu tür şemadan kopyalama elde etmek için "yapı" Atla (olarak da bilinir *şema*) bölümünde veri kümesini ve şema eşleme kopyalama etkinliğindeki.

## <a name="schema-mapping"></a>Şema eşleme

Tablo havuza Mongodb'deki verileri kopyalamak için başvurmak [şema eşleme](copy-activity-schema-and-type-mapping.md#schema-mapping).

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
