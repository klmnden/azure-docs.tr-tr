---
title: Azure Data Factory kullanarak Sybase'den veri kopyalama | Microsoft Docs
description: "Kopya etkinliği Azure Data Factory ardışık düzeninde kullanarak verileri Sybase'den desteklenen havuz veri depolarına kopyalamak öğrenin."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2018
ms.author: jingwang
ms.openlocfilehash: 88d71510c1d966c3250891eb9a430503959a91ba
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="copy-data-from-sybase-using-azure-data-factory"></a>Azure Data Factory kullanarak Sybase'den veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-onprem-sybase-connector.md)
> * [Sürüm 2 - Önizleme](connector-sybase.md)

Bu makalede kopya etkinliği Azure Data Factory'de bir Sybase veritabanından veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 Sybase Bağlayıcısı](v1/data-factory-onprem-sybase-connector.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna Sybase veritabanından veri kopyalayabilirsiniz. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Sybase bağlayıcı destekler:

- SAP Sybase SQL her yerden (ASA) **sürüm 16 ve yukarıdaki**; IQ ve ana desteklenmez.
- Verileri kullanarak kopyalama **temel** veya **Windows** kimlik doğrulaması.

## <a name="prerequisites"></a>Önkoşullar

Bu Sybase bağlayıcıyı kullanmak için aktarmanız gerekir:

- Self-hosted tümleştirme çalışma zamanı ayarlayın. Bkz: [Self-hosted tümleştirmesi çalışma zamanı](create-self-hosted-integration-runtime.md) Ayrıntılar için makale.
- Yükleme [Sybase iAnywhere.Data.SQLAnywhere için veri sağlayıcı](http://go.microsoft.com/fwlink/?linkid=324846) 16 veya yukarıdaki tümleştirme çalışma zamanı makinede.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Sybase bağlayıcıya Data Factory varlıklarını belirli tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler, Sybase bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Sybase** | Evet |
| sunucu | Sybase sunucunun adıdır. |Evet |
| veritabanı | Sybase veritabanının adı. |Evet |
| authenticationType | Sybase veritabanına bağlanmak için kullanılan kimlik doğrulama türü.<br/>İzin verilen değerler: **temel**, ve **Windows**. |Evet |
| kullanıcı adı | Sybase veritabanına bağlanmak için kullanıcı adını belirtin. |Evet |
| password | Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. Bu alan bir SecureString işaretleyin. |Evet |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Bölümünde belirtildiği gibi bir Self-hosted tümleştirmesi çalışma zamanı gereklidir [Önkoşullar](#prerequisites). |Evet |

**Örnek:**

```json
{
    "name": "SybaseLinkedService",
    "properties": {
        "type": "Sybase",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
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

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için veri kümeleri makalesine bakın. Bu bölümde, Sybase veri kümesi tarafından desteklenen özellikler listesini sağlar.

Verileri Sybase'den kopyalamak için kümesine tür özelliği ayarlamak **RelationalTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak: **RelationalTable** | Evet |
| tableName | Sybase veritabanı tablosunun adı. | ("Sorgu" etkinliği kaynağındaki belirtilmişse) yok |

**Örnek**

```json
{
    "name": "SybaseDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<Sybase linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde, Sybase kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="sybase-as-source"></a>Sybase kaynağı olarak

Verileri Sybase'den kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **RelationalSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **RelationalSource** | Evet |
| sorgu | Verileri okumak için özel SQL sorgusu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | (Veri kümesinde "tableName" belirtilmişse) yok |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromSybase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Sybase input dataset name>",
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
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-sybase"></a>Sybase için eşleme veri türü

Verileri Sybase'den kopyalarken, aşağıdaki eşlemelerini Sybase veri türlerinden Azure Data Factory geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) nasıl kopyalama etkinliği kaynak şema ve veri türü için havuz eşlemeleri hakkında bilgi edinmek için.

Sybase T-SQL türlerini destekler. Bir eşleme tablosu için SQL türlerinden Azure Data Factory geçici veri türleri için bkz: [Azure SQL Veritabanı Bağlayıcısı - veri türü eşlemesi](connector-azure-sql-database.md#data-type-mapping-for-azure-sql-database) bölümü.


## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).