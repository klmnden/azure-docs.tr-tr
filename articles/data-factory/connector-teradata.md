---
title: Teradata Azure Data Factory kullanarak verileri kopyalama | Microsoft Docs
description: Data Factory hizmetinin sağlayan Teradata Connector hakkında havuz Data Factory tarafından desteklenen veri depolarının Teradata veritabanından veri kopyalayın öğrenin.
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
ms.openlocfilehash: 37e7281af87a8cfc57aae95411eb2d4cce9eef65
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51228071"
---
# <a name="copy-data-from-teradata-using-azure-data-factory"></a>Teradata, Azure Data Factory kullanarak verileri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-onprem-teradata-connector.md)
> * [Geçerli sürüm](connector-teradata.md)

Bu makalede, kopyalama etkinliği Azure Data Factory'de bir Teradata veritabanından veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Teradata veritabanından veri tüm desteklenen havuz veri deposuna kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Teradata bağlayıcı'yı destekler:

- Teradata **sürüm 12 ve yukarıdaki**.
- Kullanarak verileri kopyalama **temel** veya **Windows** kimlik doğrulaması.

## <a name="prerequisites"></a>Önkoşullar

Bu Teradata bağlayıcıyı kullanmak için yapmanız:

- Şirket içinde barındırılan tümleştirme çalışma zamanını oluşturan ayarlayın. Bkz: [şirket içinde barındırılan tümleştirme çalışma zamanı](create-self-hosted-integration-runtime.md) makale Ayrıntılar için.
- Yükleme [Teradata için .NET veri sağlayıcısı](https://go.microsoft.com/fwlink/?LinkId=278886) 14 sürümü veya üzeri tümleştirme çalışma zamanı makinesinde.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli Teradata bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Bağlı Teradata hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Teradata** | Evet |
| sunucu | Teradata sunucusunun adı. | Evet |
| authenticationType | Teradata veritabanına bağlanmak için kullanılan kimlik doğrulaması türü.<br/>İzin verilen değerler: **temel**, ve **Windows**. | Evet |
| kullanıcı adı | Teradata veritabanına bağlanmak için kullanıcı adı belirtin. | Evet |
| password | Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Belirtildiği gibi bir şirket içinde barındırılan tümleştirme çalışma zamanı gereklidir [önkoşulları](#prerequisites). |Evet |

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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için veri kümeleri makalesine bakın. Bu bölümde, Teradata veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Teradata verileri kopyalamak için dataset öğesinin type özelliği ayarlamak **RelationalTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **RelationalTable** | Evet |
| tableName | Teradata veritabanı tablosunun adı. | Hayır (etkinlik kaynağı "sorgu" belirtilmişse) |

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

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Teradata kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="teradata-as-source"></a>Teradata kaynağı olarak

Teradata verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **RelationalSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **RelationalSource** | Evet |
| sorgu | Verileri okumak için özel bir SQL sorgusu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Yok (veri kümesinde "TableName" değeri belirtilmişse) |

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

Teradata veri kopyalama işlemi sırasında aşağıdaki eşlemeler Teradata veri türlerinden Azure veri fabrikası geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) eşlemelerini nasıl yapar? kopyalama etkinliği kaynak şema ve veri türü için havuz hakkında bilgi edinmek için.

| Teradata veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| BigInt |Int64 |
| Blob |Bayt] |
| Bayt |Bayt] |
| ByteInt |Int16 |
| Char |Dize |
| CLOB |Dize |
| Tarih |DateTime |
| Ondalık |Ondalık |
| çift |çift |
| Grafiği |Dize |
| Tamsayı |Int32 |
| Gün aralığı |Zaman aralığı |
| Saat gün aralığı |Zaman aralığı |
| Dakika gün aralığı |Zaman aralığı |
| İkinci gün aralığı |Zaman aralığı |
| Saat aralığı |Zaman aralığı |
| Aralığı saat dakika |Zaman aralığı |
| İkinci saat aralığı |Zaman aralığı |
| Aralık dakika |Zaman aralığı |
| İkinci aralık dakika |Zaman aralığı |
| Aralık ayı |Dize |
| Aralık ikinci |Zaman aralığı |
| Aralığı yıl |Dize |
| Yıl ay aralığı |Dize |
| Sayı |çift |
| Period(Date) |Dize |
| Period(Time) |Dize |
| Süresi (saat dilimiyle birlikte) |Dize |
| Period(timestamp) |Dize |
| Süre (saat dilimi ile zaman damgası) |Dize |
| Tamsayı |Int16 |
| Zaman |Zaman aralığı |
| Saat dilimi ile zaman |Dize |
| Zaman damgası |DateTime |
| Saat dilimi ile zaman damgası |DateTimeOffset |
| VarByte |Bayt] |
| VarChar |Dize |
| VarGraphic |Dize |
| Xml |Dize |


## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
