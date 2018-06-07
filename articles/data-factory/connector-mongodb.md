---
title: Azure Data Factory kullanarak MongoDB veri kopyalama | Microsoft Docs
description: Veri kopyalama etkinliği Azure Data Factory ardışık düzeninde kullanarak Mongo DB'den desteklenen havuz veri depolarına kopyalama öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/05/2018
ms.author: jingwang
ms.openlocfilehash: 24d641247ad9bb0b5e6199952cbde9cb56fcaea7
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34809303"
---
# <a name="copy-data-from-mongodb-using-azure-data-factory"></a>Azure Data Factory kullanarak MongoDB verilerini
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-on-premises-mongodb-connector.md)
> * [Sürüm 2 - Önizleme](connector-mongodb.md)

Bu makalede kopya etkinliği Azure Data Factory'de bir MongoDB veritabanından veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 MongoDB Bağlayıcısı](v1/data-factory-on-premises-mongodb-connector.md).


## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna MongoDB veritabanından veri kopyalayabilirsiniz. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu MongoDB bağlayıcı destekler:

- MongoDB **sürümleri 2.4, 2.6, 3.0, 3.2, 3.4 ve 3.6**.
- Verileri kullanarak kopyalama **temel** veya **anonim** kimlik doğrulaması.

## <a name="prerequisites"></a>Önkoşullar

Genel olarak erişilebilir değil bir MongoDB veritabanından veri kopyalamak için bir Self-hosted tümleştirmesi çalışma zamanı ayarlamanız gerekir. Bkz: [Self-hosted tümleştirmesi çalışma zamanı](create-self-hosted-integration-runtime.md) daha ayrıntılı bilgi için makalenin. Yerleşik bir MongoDB sürücü tümleştirmesi çalışma zamanı sağlar, bu nedenle herhangi bir sürücüsü adresinden veri kopyalama işlemi sırasında el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, belirli Data Factory varlıklarını MongoDB bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler MongoDB bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Type özelliği ayarlanmalıdır: **MongoDb** |Evet |
| sunucu |IP adresi veya ana bilgisayar MongoDB sunucunun adıdır. |Evet |
| port |İstemci bağlantılarını dinlemek için MongoDB sunucusunun kullandığı TCP bağlantı noktası. |Hayır (varsayılan olarak 27017) |
| databaseName |Erişmek istediğiniz MongoDB veritabanı adı. |Evet |
| authenticationType | MongoDB veritabanına bağlanmak için kullanılan kimlik doğrulama türü.<br/>İzin verilen değerler: **temel**, ve **anonim**. |Evet |
| kullanıcı adı |MongoDB erişmek için kullanıcı hesabı. |Evet (Temel kimlik doğrulaması kullanılıyorsa). |
| password |Kullanıcının parolası. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). |Evet (Temel kimlik doğrulaması kullanılıyorsa). |
| authSource |Kimlik doğrulaması için kimlik bilgilerinizi denetlemek için kullanmak istediğiniz MongoDB veritabanı adı. |Hayır. Temel kimlik doğrulaması için varsayılan yönetici hesabı ve databaseName özelliği kullanılarak belirtilen veritabanı kullanmaktır. |
| enableSsl | Sunucusuna bağlantılarda SSL kullanılarak şifrelenir olup olmadığını belirtir. Varsayılan değer false.  | Hayır |
| allowSelfSignedServerCert | Otomatik olarak imzalanan sertifikalar sunucudan izin verilip verilmeyeceğini belirtir. Varsayılan değer false.  | Hayır |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu genel olarak erişilebilir ise) Self-hosted tümleştirmesi çalışma zamanı veya Azure tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. |Hayır |

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

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için veri kümeleri makalesine bakın. Bu bölümde, MongoDB veri kümesi tarafından desteklenen özellikler listesini sağlar.

Adresinden verileri kopyalamak için kümesine tür özelliği ayarlamak **MongoDbCollection**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak: **MongoDbCollection** | Evet |
| collectionName |MongoDB veritabanı koleksiyonunda adı. |Evet |

**Örnek:**

```json
{
     "name":  "MongoDbDataset",
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

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde, MongoDB kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="mongodb-as-source"></a>Kaynak olarak MongoDB

Adresinden verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **MongoDbSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **MongoDbSource** | Evet |
| sorgu |Verileri okumak için özel SQL-92 sorgu kullanın. Örneğin: seçin * from MyTable. |(Veri kümesinde "collectionName" belirtilmişse) yok |

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

## <a name="schema-by-data-factory"></a>Şema tarafından veri fabrikası

Azure Data Factory hizmeti kullanarak MongoDB koleksiyonundan şema oluşturur **en son 100 belgeler** koleksiyondaki. Bu 100 belgeleri tam şema içermiyorsa kopyalama işlemi sırasında bazı sütunları yoksayılabilir.

## <a name="data-type-mapping-for-mongodb"></a>MongoDB için eşleme veri türü

Veri adresinden kopyalarken, aşağıdaki eşlemelerini MongoDB veri türlerinden Azure Data Factory geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) nasıl kopyalama etkinliği kaynak şema ve veri türü için havuz eşlemeleri hakkında bilgi edinmek için.

| MongoDB veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| İkili |Byte] |
| Boole |Boole |
| Tarih |DateTime |
| NumberDouble |Çift |
| NumberInt |Int32 |
| NumberLong |Int64 |
| ObjectID |Dize |
| Dize |Dize |
| UUID |Guid |
| Nesne |Renormalized içine iç içe geçmiş ayırıcı olarak "_" sütunlarla düzleştirme |

> [!NOTE]
> Sanal tablolar kullanarak dizileri desteği hakkında bilgi edinmek için bkz [sanal tablolar kullanarak karmaşık türleri için destek](#support-for-complex-types-using-virtual-tables) bölümü.
>
> Şu anda, aşağıdaki MongoDB veri türleri desteklenmiyor: DBPointer, JavaScript, en fazla/dak anahtar, normal ifade, simge, zaman damgası, tanımlanmamış.

## <a name="support-for-complex-types-using-virtual-tables"></a>Sanal tablolar kullanarak karmaşık türleri için destek

Azure Data Factory bağlanmak ve MongoDB veritabanınızdan verileri kopyalamak için yerleşik bir ODBC sürücüsü kullanır. Belgeler arasında farklı türleriyle dizileri veya nesneleri gibi karmaşık türler için sürücü veri karşılık gelen sanal tablolara yeniden normalleştirir. Özellikle, bir tablo bu tür sütunlar içeriyorsa, sürücü aşağıdaki sanal tablolar oluşturur:

* A **temel tablo**, karmaşık tür sütunları hariç gerçek tablosu olarak aynı verileri içerir. Temel tablo aynı adını temsil ettiği gerçek tablo olarak kullanır.
* A **sanal tablo** her karmaşık tür sütun için hangi genişletir iç içe veri. Sanal tablolar gerçek tablo, ayırıcı "_" ve dizi veya nesne adı adını kullanarak adlandırılır.

Sanal tablolar Normalleştirilmemiş verilere erişmek sürücüyü etkinleştirme gerçek tablodaki verileri bakın. Sorgulamak ve sanal tabloları birleştirme MongoDB dizi içeriğe erişebilir.

### <a name="example"></a>Örnek

Örneğin, burada ExampleTable MongoDB tabloda, her hücrede – faturaları nesnelerinin bir dizisi olan bir sütun ve bir dizi skaler türler – derecelendirmeleri bir sütunla olur.

| _ıd | Müşteri adı | Faturalar | Hizmet Düzeyi | Derecelendirme |
| --- | --- | --- | --- | --- |
| 1111 |ABC |[{invoice_id: "123" öğesi: "toaster", fiyat: "456", indirim: "0.2"}, {invoice_id: "124" öğesi: "fırın", fiyat: "1235" indirim: "0.2"}] |Gümüş |[5,6] |
| 2222 |XYZ |[{invoice_id: "135" öğesi: "fridge", fiyat: "12543", indirim: "0,0"}] |Altın |[1,2] |

Sürücü bu tek tablo göstermek için birden çok sanal tablo oluşturur. İlk sanal "örnekte gösterilen ExampleTable" adlı temel tablo tablodur. Temel tablo özgün tablonun tüm verileri içerir, ancak diziler verilerden çıkarıldı ve sanal tablolarda genişletilir.

| _ıd | Müşteri adı | Hizmet Düzeyi |
| --- | --- | --- |
| 1111 |ABC |Gümüş |
| 2222 |XYZ |Altın |

Aşağıdaki tablolarda örnek özgün dizilerde temsil eden sanal tablolar gösterilmektedir. Bu tablolar aşağıdakileri içerir:

* (Aracılığıyla _ıd sütun) özgün dizi satır karşılık gelen geri özgün birincil anahtar sütunu başvurusu
* Göstergesidir özgün dizinin içindeki verilerin konumu
* Dizi içinde her öğe için genişletilmiş veri

**Tablo "ExampleTable_Invoices":**

| _ıd | ExampleTable_Invoices_dim1_idx | invoice_id | Öğesi | price | İndirim |
| --- | --- | --- | --- | --- | --- |
| 1111 |0 |123 |toaster |456 |0.2 |
| 1111 |1 |124 |Fırın |1235 |0.2 |
| 2222 |0 |135 |fridge |12543 |0.0 |

**Tablo "ExampleTable_Ratings":**

| _ıd | ExampleTable_Ratings_dim1_idx | ExampleTable_Ratings |
| --- | --- | --- |
| 1111 |0 |5 |
| 1111 |1 |6 |
| 2222 |0 |1 |
| 2222 |1 |2 |


## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
