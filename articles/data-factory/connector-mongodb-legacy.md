---
title: Azure Data Factory kullanarak Mongodb'deki verileri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına Mongo DB bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/20/2018
ms.author: jingwang
ms.openlocfilehash: 86dcd39ad7b9f1e207e9254ec72698db3998bbd6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61400483"
---
# <a name="copy-data-from-mongodb-using-azure-data-factory"></a>Azure Data Factory kullanarak MongoDB verilerini kopyalama
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](v1/data-factory-on-premises-mongodb-connector.md)
> * [Geçerli sürüm](connector-mongodb.md)

Bu makalede, kopyalama etkinliği Azure Data Factory'de bir MongoDB veritabanına veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

>[!IMPORTANT]
>ADF yayın daha iyi yerel MongoDB sağlayan yeni bir MongoDB bağlayıcısını Bu ODBC tabanlı bir uygulama karşılaştırma destekleyen, başvurmak [MongoDB bağlayıcısını](connector-mongodb.md) ilişkin ayrıntıları. Bu eski MongoDB bağlayıcısını olarak desteklenen tutulur-olan geriye dönük uyumluluk için tüm yeni iş yükleri için sırada, Lütfen yeni Bağlayıcısı'nı kullanın.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna verileri bir MongoDB veritabanından kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu MongoDB bağlayıcısını destekler:

- MongoDB **2.4, 2.6, 3.0, 3.2, 3.4 ve 3.6 sürümlerini**.
- Kullanarak verileri kopyalama **temel** veya **anonim** kimlik doğrulaması.

## <a name="prerequisites"></a>Önkoşullar

Genel olarak erişilebilir değil bir MongoDB veritabanına veri kopyalamak için şirket içinde barındırılan tümleştirme çalışma zamanını oluşturan gerekir. Bkz: [şirket içinde barındırılan tümleştirme çalışma zamanı](create-self-hosted-integration-runtime.md) daha fazla bilgi edinmek için makaleyi. Tümleştirme çalışma zamanı yerleşik bir MongoDB sürücü sağlar, bu nedenle herhangi bir sürücü Mongodb'deki verileri kopyalama işlemi sırasında el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli MongoDB bağlayıcısını tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

MongoDB bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Type özelliği ayarlanmalıdır: **MongoDb** |Evet |
| server |IP adresi veya ana bilgisayar adı MongoDB sunucusunun. |Evet |
| port |MongoDB sunucusunun istemci bağlantıları için dinlemek üzere kullandığı TCP bağlantı noktası. |Hayır (varsayılan değer 27017) |
| databaseName |Erişmek istediğiniz MongoDB veritabanının adı. |Evet |
| authenticationType | MongoDB veritabanına bağlanmak için kullanılan kimlik doğrulaması türü.<br/>İzin verilen değerler şunlardır: **Temel**, ve **anonim**. |Evet |
| username |MongoDB erişmek için kullanıcı hesabı'nı tıklatın. |Evet (Temel kimlik doğrulaması kullanılıyorsa). |
| password |Kullanıcının parolası. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Evet (Temel kimlik doğrulaması kullanılıyorsa). |
| authSource |Kimlik doğrulaması için kimlik bilgilerinizi denetlemek için kullanmak istediğiniz MongoDB veritabanının adı. |Hayır. Temel kimlik doğrulaması için yönetici hesabı ve databaseName özelliği kullanılarak belirtilen veritabanı kullanmak için varsayılandır. |
| enableSsl | Sunucusuna bağlantılarda SSL kullanarak şifrelenip şifrelenmeyeceğini belirtir. Varsayılan değer false'tur.  | Hayır |
| allowSelfSignedServerCert | Otomatik olarak imzalanan sertifikalar sunucudan izin verilip verilmeyeceğini belirtir. Varsayılan değer false'tur.  | Hayır |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz genel olarak erişilebilir değilse), şirket içinde barındırılan tümleştirme çalışma zamanı veya Azure Integration Runtime kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

**Örnek:**

```json
{
    "name": "MongoDBLinkedService",
    "properties": {
        "type": "MongoDb",
        "typeProperties": {
            "server": "<server name>",
            "databaseName": "<database name>",
            "authenticationType": "Basic",
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

Bölümleri ve veri kümeleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [veri kümeleri ve bağlı hizmetler](concepts-datasets-linked-services.md). MongoDB veri kümesi için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **MongoDbCollection** | Evet |
| collectionName |MongoDB veritabanındaki koleksiyonun adı. |Evet |

**Örnek:**

```json
{
    "name": "MongoDbDataset",
    "properties": {
        "type": "MongoDbCollection",
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
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **MongoDbSource** | Evet |
| query |Verileri okumak için özel SQL 92 sorgu kullanın. Örneğin: seçin * MyTable öğesinden. |Yok (veri kümesinde "collectionName" belirtilmişse) |

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
                "type": "MongoDbSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

> [!TIP]
> Ne zaman dikkat DateTime biçimine SQL sorgusunu belirtin. Örneğin: `SELECT * FROM Account WHERE LastModifiedDate >= '2018-06-01' AND LastModifiedDate < '2018-06-02'` veya parametresini kullanma `SELECT * FROM Account WHERE LastModifiedDate >= '@{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-dd HH:mm:ss')}' AND LastModifiedDate < '@{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-dd HH:mm:ss')}'`

## <a name="schema-by-data-factory"></a>Veri fabrikası tarafından şeması

Azure Data Factory hizmeti kullanarak bir MongoDB koleksiyonu şema çıkarsar **en son 100 belgede** koleksiyondaki. 100 bu belgeler, tam şema içermiyorsa, kopyalama işlemi sırasında bazı sütunları yoksayılabilir.

## <a name="data-type-mapping-for-mongodb"></a>MongoDB için eşleme veri türü

Mongodb'deki verileri kopyalama, aşağıdaki eşlemeler MongoDB veri türlerinden Azure veri fabrikası geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) eşlemelerini nasıl yapar? kopyalama etkinliği kaynak şema ve veri türü için havuz hakkında bilgi edinmek için.

| MongoDB veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| Binary |Byte[] |
| Boolean |Boolean |
| Tarih |DateTime |
| NumberDouble |Double |
| NumberInt |Int32 |
| NumberLong |Int64 |
| ObjectID |String |
| Dize |String |
| UUID |Guid |
| Object |Renormalized içine sütunları içeren iç içe geçmiş ayırıcı olarak "_" düzleştirme |

> [!NOTE]
> Sanal tablolar kullanarak dizileri için destek hakkında bilgi edinmek için başvurmak [sanal tabloları kullanarak karmaşık türler için destek](#support-for-complex-types-using-virtual-tables) bölümü.
>
> Şu anda aşağıdaki MongoDB veri türleri desteklenmez: Normal ifade, sembol, zaman damgası, tanımlanmamış DBPointer, JavaScript, en yüksek/dak anahtar.

## <a name="support-for-complex-types-using-virtual-tables"></a>Sanal tablolar'ı kullanarak karmaşık türler için destek

Azure Data Factory, bağlanma ve MongoDB veritabanından veri kopyalamak için yerleşik bir ODBC sürücüsünü kullanır. Belgeler arasında farklı tür ile diziler veya nesneler gibi karmaşık türler için sürücü veri karşılık gelen sanal tablolarına yeniden normalleştirir. Özellikle, bir tablo bu tür sütunlar içeriyorsa, sürücü aşağıdaki sanal tablolar oluşturur:

* A **temel tablo**, karmaşık tür sütunları hariç gerçek tablosu olarak aynı verileri içerir. Temel tablo adıyla temsil ettiği gerçek tablosu olarak kullanır.
* A **sanal tablo** her bir karmaşık türü sütun için genişleyen iç içe veri. Sanal tablolar, gerçek tablosu, "_" ayırıcı ve dizi veya nesne adı adını kullanarak yeniden adlandırılır.

Sanal tablolar normalleştirilmişlikten çıkarılmış verilere erişmek sürücüyü etkinleştirme gerçek tablodaki verileri bakın. MongoDB diziler içeriğini sorgulama ve sanal tabloları birleştirme erişebilirsiniz.

### <a name="example"></a>Örnek

Örneğin, burada ExampleTable her hücredeki – fatura, nesneleri içeren bir dizi içeren bir sütun ve skaler türler – derecelendirmeleri bir dizi içeren bir sütun bir MongoDB tablodur.

| _id | Müşteri Adı | Faturalar | Hizmet Düzeyi | Derecelendirme |
| --- | --- | --- | --- | --- |
| 1111 |ABC |[{invoice_id: "123" öğesi: "toaster", price: "456" indirim: "0.2"}, {invoice_id: "124" öğesi: "fırın", price: "1235" indirim: "0.2"}] |Gümüş |[5,6] |
| 2222 |XYZ |[{invoice_id: "135" öğesi: "fridge", price: "12543" indirim: "0.0"}] |Altın |[1,2] |

Sürücü bu tek tabloda temsil etmek için birden çok sanal tablolar oluşturur. İlk sanal "örnekte gösterilen ExampleTable" adlı temel tablo tablosudur. Temel tablo özgün tablonun tüm verileri içerir, ancak dizileri verilerden çıkarıldı ve sanal tablolarında genişletilir.

| _id | Müşteri Adı | Hizmet Düzeyi |
| --- | --- | --- |
| 1111 |ABC |Gümüş |
| 2222 |XYZ |Altın |

Aşağıdaki tablolar, özgün diziler örnekte temsil eden sanal tablolar gösterir. Bu tablolar arasında aşağıdakiler yer alır:

* Özgün birincil anahtar sütunu satır (aracılığıyla _kimliği sütun) özgün dizinin karşılık gelen bir başvuru dön
* Verileri özgün dizi içinde konumunu bir göstergesi
* Dizideki her öğe için genişletilmiş verileri

**Tablo "ExampleTable_Invoices":**

| _id | ExampleTable_Invoices_dim1_idx | invoice_id | Öğesi | price | İndirim |
| --- | --- | --- | --- | --- | --- |
| 1111 |0 |123 |toaster |456 |0.2 |
| 1111 |1 |124 |Fırın |1235 |0.2 |
| 2222 |0 |135 |fridge |12543 |0.0 |

**Tablo "ExampleTable_Ratings":**

| _id | ExampleTable_Ratings_dim1_idx | ExampleTable_Ratings |
| --- | --- | --- |
| 1111 |0 |5 |
| 1111 |1 |6 |
| 2222 |0 |1 |
| 2222 |1 |2 |

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
