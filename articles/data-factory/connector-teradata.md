---
title: Azure Data Factory kullanarak Teradata veri kopyalama | Microsoft Docs
description: Olanak sağlayan Data Factory hizmetinin Teradata Bağlayıcısı hakkında bilgi edinin verilerini Teradata veritabanından veri depolarına havuzlarını Data Factory ile desteklenen.
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
ms.date: 02/07/2018
ms.author: jingwang
ms.openlocfilehash: 4360ff12a435afc4347fa97bba4506ccd81618aa
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34618989"
---
# <a name="copy-data-from-teradata-using-azure-data-factory"></a>Azure Data Factory kullanarak Teradata verilerini
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-onprem-teradata-connector.md)
> * [Sürüm 2 - Önizleme](connector-teradata.md)

Bu makalede kopya etkinliği Azure Data Factory'de bir Teradata veritabanından veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 Teradata Bağlayıcısı](v1/data-factory-onprem-teradata-connector.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna Teradata veritabanından veri kopyalayabilirsiniz. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Teradata bağlayıcı destekler:

- Teradata **sürüm 12 ve yukarıdaki**.
- Verileri kullanarak kopyalama **temel** veya **Windows** kimlik doğrulaması.

## <a name="prerequisites"></a>Önkoşullar

Bu Teradata bağlayıcıyı kullanmak için aktarmanız gerekir:

- Self-hosted tümleştirme çalışma zamanı ayarlayın. Bkz: [Self-hosted tümleştirmesi çalışma zamanı](create-self-hosted-integration-runtime.md) Ayrıntılar için makale.
- Yükleme [Teradata için .NET veri sağlayıcısı](http://go.microsoft.com/fwlink/?LinkId=278886) sürüm 14 veya yukarıdaki tümleştirme çalışma zamanı makinede.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, belirli Data Factory varlıklarını Teradata bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler Teradata bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Teradata** | Evet |
| sunucu | Teradata sunucunun adıdır. | Evet |
| authenticationType | Teradata veritabanına bağlanmak için kullanılan kimlik doğrulama türü.<br/>İzin verilen değerler: **temel**, ve **Windows**. | Evet |
| kullanıcı adı | Teradata veritabanına bağlanmak için kullanıcı adını belirtin. | Evet |
| password | Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). | Evet |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Bölümünde belirtildiği gibi bir Self-hosted tümleştirmesi çalışma zamanı gereklidir [Önkoşullar](#prerequisites). |Evet |

**Örnek:**

```json
{
    "name": "TeradataLinkedService",
    "properties": {
        "type": "Teradata",
        "typeProperties": {
            "server": "<server>",
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

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için veri kümeleri makalesine bakın. Bu bölümde Teradata veri kümesi tarafından desteklenen özellikler listesini sağlar.

Teradata verileri kopyalamak için kümesine tür özelliği ayarlamak **RelationalTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak: **RelationalTable** | Evet |
| tableName | Teradata veritabanına tablo adı. | ("Sorgu" etkinliği kaynağındaki belirtilmişse) yok |

**Örnek:**

```json
{
    "name": "TeradataDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<Teradata linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde Teradata kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="teradata-as-source"></a>Kaynak olarak Teradata

Teradata verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **RelationalSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **RelationalSource** | Evet |
| sorgu | Verileri okumak için özel SQL sorgusu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | (Veri kümesinde "tableName" belirtilmişse) yok |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromTeradata",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Teradata input dataset name>",
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

## <a name="data-type-mapping-for-teradata"></a>Teradata için eşleme veri türü

Teradata veri kopyalama işlemi sırasında aşağıdaki eşlemelerini Teradata veri türlerinden Azure Data Factory geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) nasıl kopyalama etkinliği kaynak şema ve veri türü için havuz eşlemeleri hakkında bilgi edinmek için.

| Teradata veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| BigInt |Int64 |
| Blob |Byte] |
| Bayt |Byte] |
| ByteInt |Int16 |
| char |Dize |
| CLOB |Dize |
| Tarih |DateTime |
| Ondalık |Ondalık |
| Çift |Çift |
| Grafiği |Dize |
| Tamsayı |Int32 |
| Aralık gün |TimeSpan |
| Saat gün aralığı |TimeSpan |
| Dakika gün aralığı |TimeSpan |
| İkinci gün aralığı |TimeSpan |
| Aralık saat |TimeSpan |
| Aralık saat dakika |TimeSpan |
| İkinci aralığı saate |TimeSpan |
| Aralık dakika |TimeSpan |
| İkinci için aralığı dakika |TimeSpan |
| Aralık ayı |Dize |
| Aralığı ikinci |TimeSpan |
| Aralığı yıl |Dize |
| Aralığı yıl ay için |Dize |
| Sayı |Çift |
| Period(Date) |Dize |
| Period(Time) |Dize |
| Süresi (saat dilimi ile) |Dize |
| Period(timestamp) |Dize |
| Süre (saat dilimi damgasıyla) |Dize |
| Tamsayı |Int16 |
| Zaman |TimeSpan |
| Saat dilimi süresiyle |Dize |
| Zaman damgası |DateTime |
| Saat dilimi zaman damgası |DateTimeOffset |
| VarByte |Byte] |
| VarChar |Dize |
| VarGraphic |Dize |
| Xml |Dize |


## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
