---
title: DB2 Azure Data Factory kullanarak verileri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına DB2'den bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: jingwang
ms.openlocfilehash: 797db8d0dd321676a3fa436a328a9981a3d3ca3b
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67312041"
---
# <a name="copy-data-from-db2-by-using-azure-data-factory"></a>Azure Data Factory kullanarak gelen DB2 veri kopyalama
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](v1/data-factory-onprem-db2-connector.md)
> * [Geçerli sürüm](connector-db2.md)

Bu makalede, kopyalama etkinliği Azure Data Factory'de DB2 veritabanından veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

DB2 veritabanından veri tüm desteklenen havuz veri deposuna kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu DB2 Bağlayıcısı aşağıdaki IBM DB2 platformlara ve sürümlere ile dağıtılmış ilişkisel veritabanı mimarisi (DRDA) SQL Erişim Yöneticisi (SQLAM) sürüm 9, 10 ve 11 destekler:

* IBM DB2 z/OS 11.1 için
* IBM DB2 z/OS 10.1 için
* IBM DB2 için i 7.3
* IBM DB2 için i 7.2
* IBM DB2 için i 7.1
* IBM DB2 LUW 11
* IBM DB2 LUW 10.5 için
* IBM DB2 LUW 10.1 için

> [!TIP]
> "Bir SQL deyimi yürütme isteğine karşılık gelen paket bulunamadı. bildiren bir hata iletisi alırsanız SQLSTATE 51002 SQLCODE =-805 = ", bu işletim sisteminde normal bir kullanıcı için gerekli paket oluşturulmaz nedenidir. DB2 sunucu türüne göre aşağıdaki yönergeleri izleyin:
> - DB2 için i (AS400): power kullanıcının oturum açan kullanıcı için kopyalama etkinliği kullanmadan önce koleksiyonu oluşturun. Komut: `create collection <username>`
> - DB2 z/OS veya LUW: kopyalama etkinliği bir kez çalışmak üzere yüksek ayrıcalıklı bir hesap - ileri kullanıcı veya yönetici paket yetkilileri ve bağlama BINDADD, verme YÜRÜTMEK için genel izinleri - kullanın ve gerekli paket kopyalama sırasında otomatik olarak oluşturulur. Ardından, sonraki kopyalama çalışmalarınız için normal kullanıcı için geçiş yapabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Genel olarak erişilebilir değil bir DB2 veritabanından veri kopyalama kullanmak için şirket içinde barındırılan tümleştirme çalışma zamanını oluşturan gerekir. Şirket içinde barındırılan tümleştirme çalışma zamanları hakkında bilgi edinmek için [şirket içinde barındırılan tümleştirme çalışma zamanı](create-self-hosted-integration-runtime.md) makalesi. Tümleştirme çalışma zamanı yerleşik bir DB2 sürücüsü sağlar, bu nedenle herhangi bir sürücü DB2'den veri kopyalama işlemi sırasında el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli DB2 Bağlayıcısı tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

DB2 bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Db2** | Evet |
| server |DB2 sunucusunun adı. Aşağıdaki örneğin virgül ile ayrılmış sunucu adı bağlantı noktası numarasını belirtebilirsiniz `server:port`. |Evet |
| database |DB2 veritabanı adı. |Evet |
| authenticationType |DB2 veritabanına bağlanmak için kullanılan kimlik doğrulaması türü.<br/>İzin verilen değeri şudur: **Temel**. |Evet |
| username |DB2 veritabanına bağlanmak için kullanıcı adı belirtin. |Evet |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz genel olarak erişilebilir değilse), şirket içinde barındırılan tümleştirme çalışma zamanı veya Azure Integration Runtime kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

**Örnek:**

```json
{
    "name": "Db2LinkedService",
    "properties": {
        "type": "Db2",
        "typeProperties": {
            "server": "<servername:port>",
            "database": "<dbname>",
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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için veri kümeleri makalesine bakın. Bu bölümde, DB2 veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

DB2'den veri kopyalamak için dataset öğesinin type özelliği ayarlamak **RelationalTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **RelationalTable** | Evet |
| tableName | DB2 veritabanı tablosunun adı. | Hayır (etkinlik kaynağı "sorgu" belirtilmişse) |

**Örnek**

```json
{
    "name": "DB2Dataset",
    "properties":
    {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<DB2 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, DB2 kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="db2-as-source"></a>DB2 kaynağı olarak

DB2'den veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **RelationalSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **RelationalSource** | Evet |
| query | Verileri okumak için özel bir SQL sorgusu kullanın. Örneğin: `"query": "SELECT * FROM \"DB2ADMIN\".\"Customers\""`. | Yok (veri kümesinde "TableName" değeri belirtilmişse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromDB2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<DB2 input dataset name>",
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
                "type": "RelationalSource",
                "query": "SELECT * FROM \"DB2ADMIN\".\"Customers\""
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-db2"></a>DB2 için eşleme veri türü

DB2'den veri kopyalama işlemi sırasında aşağıdaki eşlemeler DB2 veri türlerinden Azure veri fabrikası geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) eşlemelerini nasıl yapar? kopyalama etkinliği kaynak şema ve veri türü için havuz hakkında bilgi edinmek için.

| DB2 veritabanı türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| BigInt |Int64 |
| Binary |Byte[] |
| Blob |Byte[] |
| Char |String |
| Clob |String |
| Date |Datetime |
| DB2DynArray |String |
| DbClob |String |
| Decimal |Decimal |
| DecimalFloat |Decimal |
| Double |Double |
| Float |Double |
| Graphic |String |
| Integer |Int32 |
| LongVarBinary |Byte[] |
| LongVarChar |String |
| LongVarGraphic |String |
| Numeric |Decimal |
| Real |Single |
| Smallint |Int16 |
| Time |TimeSpan |
| Timestamp |Datetime |
| VarBinary |Byte[] |
| VarChar |String |
| VarGraphic |String |
| Xml |Byte[] |


## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
