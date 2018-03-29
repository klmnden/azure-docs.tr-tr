---
title: DB2 veri Azure Data Factory kullanarak kopyalamak | Microsoft Docs
description: Desteklenen havuz veri depolarına DB2'den bir Azure Data Factory ardışık düzeninde kopyalama etkinliği kullanarak verileri kopyalamak öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2018
ms.author: jingwang
ms.openlocfilehash: 7713e1b6a74fd099206804133d2dc8140fe83a8d
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="copy-data-from-db2-by-using-azure-data-factory"></a>Azure Data Factory kullanarak DB2'den veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-onprem-db2-connector.md)
> * [Sürüm 2 - Önizleme](connector-db2.md)

Bu makalede kopya etkinliği Azure Data Factory'de DB2 veritabanından veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 DB2 Bağlayıcısı](v1/data-factory-onprem-db2-connector.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna DB2 veritabanından veri kopyalayabilirsiniz. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu DB2 Bağlayıcısı sürümleri ile dağıtılmış ilişkisel veritabanı mimarisi (DRDA) SQL Erişim Yöneticisi (SQLAM) sürüm 9, 10 ve 11 ve aşağıdaki IBM DB2 platformları destekler:

* IBM DB2 z/OS 11.1 için
* IBM DB2 z/OS 10.1 için
* IBM DB2 için i 7.2
* IBM DB2 için i 7.1
* IBM DB2 LUW 11
* IBM DB2 LUW 10.5 için
* IBM DB2 LUW 10.1 için

> [!TIP]
> "Bir SQL deyimi yürütme isteğine karşılık gelen paket bulunamadı. bildiren bir hata iletisi alırsanız SQLSTATE 51002 SQLCODE =-805 = ", gerekli paket oluşturulmadı, bu tür işletim sistemine normal bir kullanıcı için nedenidir. DB2 Sunucu türünüz göre aşağıdaki yönergeleri izleyin:
> - DB2 için i (AS400): güç kullanıcı oturum açma kullanıcısı için kopyalama etkinliği kullanmadan önce Koleksiyonu Oluştur olanak tanır. komut: `create collection <username>`
> - DB2 için z/OS veya LUW: kopyalama etkinliği çalışmak üzere yüksek ayrıcalıklı bir hesap - power user veya Yönetim Paketi yetkilileri ve bağlama, BINDADD, GRANT YÜRÜTMEK için genel izinleri - kullanın ve sonra gerekli paket kopyalama sırasında otomatik olarak oluşturulur. Daha sonra geri normal bir kullanıcı için sonraki kopyalama çalışmalarınız için geçiş yapabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Genel olarak erişilebilir değil bir DB2 veritabanından veri Kopyala kullanmak için Self-hosted tümleştirmesi çalışma zamanı ayarlamak gerekir. Kendini barındıran tümleştirme çalışma zamanları hakkında bilgi edinmek için [Self-hosted tümleştirmesi çalışma zamanı](create-self-hosted-integration-runtime.md) makalesi. Yerleşik bir DB2 sürücü tümleştirmesi çalışma zamanı sağlar, bu nedenle herhangi bir sürücüsü DB2'den veri kopyalama işlemi sırasında el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, belirli Data Factory varlıklarını DB2 Bağlayıcısı tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler DB2 bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Db2** | Evet |
| sunucu |DB2 sunucunun adıdır. |Evet |
| veritabanı |DB2 veritabanının adı. |Evet |
| authenticationType |DB2 veritabanına bağlanmak için kullanılan kimlik doğrulama türü.<br/>Değer izin verilen: **temel**. |Evet |
| kullanıcı adı |DB2 veritabanına bağlanmak için kullanıcı adını belirtin. |Evet |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). |Evet |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu genel olarak erişilebilir ise) Self-hosted tümleştirmesi çalışma zamanı veya Azure tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. |Hayır |

**Örnek:**

```json
{
    "name": "Db2LinkedService",
    "properties": {
        "type": "Db2",
        "typeProperties": {
            "server": "<servername>",
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

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için veri kümeleri makalesine bakın. Bu bölümde DB2 veri kümesi tarafından desteklenen özellikler listesini sağlar.

DB2'den verileri kopyalamak için kümesine tür özelliği ayarlamak **RelationalTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak: **RelationalTable** | Evet |
| tableName | DB2 veritabanında tablonun adı. | ("Sorgu" etkinliği kaynağındaki belirtilmişse) yok |

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

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde DB2 kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="db2-as-source"></a>Kaynak olarak DB2

DB2'den verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **RelationalSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **RelationalSource** | Evet |
| sorgu | Verileri okumak için özel SQL sorgusu kullanın. Örneğin: `"query": "SELECT * FROM \"DB2ADMIN\".\"Customers\""`. | (Veri kümesinde "tableName" belirtilmişse) yok |

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

## <a name="data-type-mapping-for-db2"></a>Eşleme DB2 için veri türü

DB2'den veri kopyalama işlemi sırasında aşağıdaki eşlemelerini DB2 veri türlerinden Azure Data Factory geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) nasıl kopyalama etkinliği kaynak şema ve veri türü için havuz eşlemeleri hakkında bilgi edinmek için.

| DB2 veritabanına türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| BigInt |Int64 |
| İkili |Byte] |
| Blob |Byte] |
| char |Dize |
| Clob |Dize |
| Tarih |Tarih saat |
| DB2DynArray |Dize |
| DbClob |Dize |
| Ondalık |Ondalık |
| DecimalFloat |Ondalık |
| Çift |Çift |
| Kayan nokta |Çift |
| Grafiği |Dize |
| Tamsayı |Int32 |
| LONGVARBINARY |Byte] |
| LongVarChar |Dize |
| LongVarGraphic |Dize |
| sayısal |Ondalık |
| Real |Bekar |
| Tamsayı |Int16 |
| Zaman |TimeSpan |
| Zaman damgası |DateTime |
| VarBinary |Byte] |
| VarChar |Dize |
| VarGraphic |Dize |
| Xml |Byte] |


## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).