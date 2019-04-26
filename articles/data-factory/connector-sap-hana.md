---
title: Azure Data Factory kullanarak SAP HANA'dan veri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına SAP HANA'dan bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/07/2018
ms.author: jingwang
ms.openlocfilehash: cdd83c3ff9d34a5e8b7f2c164136ab82f498ffb5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60343775"
---
# <a name="copy-data-from-sap-hana-using-azure-data-factory"></a>Azure Data Factory kullanarak SAP HANA'dan veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-sap-hana-connector.md)
> * [Geçerli sürüm](connector-sap-hana.md)

Bu makalede, kopyalama etkinliği Azure Data Factory'de bir SAP HANA veritabanından veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

SAP HANA veritabanından veri tüm desteklenen havuz veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynakları/havuz desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu SAP HANA Bağlayıcısı destekler:

- SAP HANA veritabanı herhangi bir sürümünü kopyalama verileri.
- Veri kopyalama **HANA bilgi modellerini** (analitik görünümlere ve hesaplama görünümleri gibi) ve **satır/sütun tabloları** SQL sorgularını kullanarak.
- Kullanarak verileri kopyalama **temel** veya **Windows** kimlik doğrulaması.

> [!NOTE]
> Verileri kopyalamak için **içine** SAP HANA veri depolayın, genel ODBC Bağlayıcısı'nı kullanın. Bkz: [SAP HANA havuz](connector-odbc.md#sap-hana-sink) ayrıntılarla. Bu nedenle Not SAP HANA Bağlayıcısı ve ODBC Bağlayıcısı için bağlı hizmetler farklı bir türle yeniden kullanılamaz.

## <a name="prerequisites"></a>Önkoşullar

Bu SAP HANA Bağlayıcısı'nı kullanmak için gerekir:

- Şirket içinde barındırılan tümleştirme çalışma zamanını oluşturan ayarlayın. Bkz: [şirket içinde barındırılan tümleştirme çalışma zamanı](create-self-hosted-integration-runtime.md) makale Ayrıntılar için.
- Integration Runtime makine üzerinde SAP HANA ODBC sürücüsünü yükleyin. SAP HANA ODBC sürücüsünü yükleyebilirsiniz [SAP Software Download Center](https://support.sap.com/swdc). Anahtar sözcüğüyle arama **için SAP HANA istemci Windows**.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, SAP HANA Bağlayıcısı özel Data Factory varlıkları tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

SAP HANA bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **SapHana** | Evet |
| sunucu | SAP HANA örneği yer aldığı sunucunun adı. Sunucunuz özelleştirilmiş bir bağlantı noktası kullanıyorsa, belirtin `server:port`. | Evet |
| authenticationType | SAP HANA veritabanına bağlanmak için kullanılan kimlik doğrulaması türü.<br/>İzin verilen değerler şunlardır: **Temel**, ve **Windows** | Evet |
| Kullanıcı adı | SAP sunucusuna erişimi olan kullanıcı adı. | Evet |
| password | Kullanıcının parolası. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Belirtildiği gibi bir şirket içinde barındırılan tümleştirme çalışma zamanı gereklidir [önkoşulları](#prerequisites). |Evet |

**Örnek:**

```json
{
    "name": "SapHanaLinkedService",
    "properties": {
        "type": "SapHana",
        "typeProperties": {
            "server": "<server>:<port (optional)>",
            "authenticationType": "Basic",
            "userName": "<username>",
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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için veri kümeleri makalesine bakın. Bu bölümde, SAP HANA veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

SAP HANA'dan veri kopyalamak için dataset öğesinin type özelliği ayarlamak **RelationalTable**. SAP HANA veri kümesi için desteklenen hiçbir türe özgü özellikler varken RelationalTable yazın.

**Örnek:**

```json
{
    "name": "SAPHANADataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<SAP HANA linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, SAP HANA kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="sap-hana-as-source"></a>SAP HANA kaynağı olarak

SAP HANA'dan veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **RelationalSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **RelationalSource** | Evet |
| sorgu | SAP HANA örneği verileri okumak için SQL sorgusu belirtir. | Evet |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromSAPHANA",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP HANA input dataset name>",
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
                "query": "<SQL query for SAP HANA>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-sap-hana"></a>SAP HANA için veri türü eşlemesi

SAP HANA'dan veri kopyalama işlemi sırasında aşağıdaki eşlemeler SAP HANA veri türleri arasından Azure veri fabrikası geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) eşlemelerini nasıl yapar? kopyalama etkinliği kaynak şema ve veri türü için havuz hakkında bilgi edinmek için.

| SAP HANA veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| ALPHANUM | String |
| BIGINT | Int64 |
| BLOB | Byte[] |
| BOOLE DEĞERİ | Byte |
| CLOB | Byte[] |
| DATE | DateTime |
| DECIMAL | Decimal |
| ÇİFT | Single |
| INT | Int32 |
| NVARCHAR | String |
| GERÇEK | Single |
| SECONDDATE | DateTime |
| TAMSAYI | Int16 |
| TIME | TimeSpan |
| ZAMAN DAMGASI | DateTime |
| MİNİ TAMSAYI | Byte |
| VARCHAR | String |

## <a name="known-limitations"></a>Bilinen sınırlamalar

SAP HANA'dan veri kopyalama işlemi sırasında bazı bilinen sınırlamalar vardır:

- NVARCHAR dizeleri 4000 Unicode karakter maksimum uzunluğunu kesilir
- SMALLDECIMAL desteklenmez
- VARBINARY desteklenmez
- Geçerli tarihler 1899/12/30 arasında olan ve 9999/12/31


## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
