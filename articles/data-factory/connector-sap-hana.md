---
title: Azure Data Factory kullanarak SAP HANA veri kopyalama | Microsoft Docs
description: "Veri kopyalama etkinliği Azure Data Factory ardışık düzeninde kullanarak SAP HANA desteklenen havuz veri depolarına kopyalama öğrenin."
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
ms.date: 01/10/2018
ms.author: jingwang
ms.openlocfilehash: cb70b6fee5257a07dda673d6d0f6feb07ad66958
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-from-sap-hana-using-azure-data-factory"></a>Azure Data Factory kullanarak SAP HANA veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-sap-hana-connector.md)
> * [Sürüm 2 - Önizleme](connector-sap-hana.md)

Bu makalede kopya etkinliği Azure Data Factory'de bir SAP HANA veritabanından veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 SAP HANA Bağlayıcısı](v1/data-factory-sap-hana-connector.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

SAP HANA veritabanından veri tüm desteklenen havuz veri deposuna kopyalayabilirsiniz. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu SAP HANA bağlayıcı destekler:

- SAP HANA veritabanına herhangi bir sürümünden veri kopyalamayı.
- Verileri kopyalama **HANA bilgi modelleri** (gibi Analitik ve hesaplama görünümleri) ve **satır/sütun tabloları** SQL sorgularını kullanarak.
- Verileri kullanarak kopyalama **temel** veya **Windows** kimlik doğrulaması.

> [!NOTE]
> Verileri kopyalamak için **içine** SAP HANA veri deposuna, genel ODBC bağlayıcı kullanın. Bkz: [SAP HANA havuz](connector-odbc.md#sap-hana-sink) ayrıntılarla. SAP HANA bağlayıcı ve ODBC bağlayıcı için bağlantılı Hizmetleri farklı türde Not böylece yeniden kullanılamaz.

## <a name="prerequisites"></a>Önkoşullar

Bu SAP HANA bağlayıcıyı kullanmak için aktarmanız gerekir:

- Self-hosted tümleştirme çalışma zamanı ayarlayın. Bkz: [Self-hosted tümleştirmesi çalışma zamanı](create-self-hosted-integration-runtime.md) Ayrıntılar için makale.
- SAP HANA ODBC sürücüsü tümleştirmesi çalışma zamanı makineye yükleyin. SAP HANA ODBC sürücüsünü yükleyebilirsiniz [SAP yazılım İndirme Merkezi](https://support.sap.com/swdc). Arama anahtar sözcüğü ile **Windows için SAP HANA İSTEMCİSİ**.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, SAP HANA bağlayıcıya Data Factory varlıklarını belirli tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler, SAP HANA bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **SapHana** | Evet |
| sunucu | SAP HANA örneği bulunduğu sunucunun adıdır. Sunucunuz özelleştirilmiş bir bağlantı noktası kullanıyorsa belirtin `server:port`. | Evet |
| authenticationType | SAP HANA veritabanına bağlanmak için kullanılan kimlik doğrulama türü.<br/>İzin verilen değerler: **temel**, ve **Windows** | Evet |
| Kullanıcı adı | SAP sunucusuna erişimi olan kullanıcı adı. | Evet |
| password | Kullanıcının parolası. Bu alan bir SecureString işaretleyin. | Evet |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Bölümünde belirtildiği gibi bir Self-hosted tümleştirmesi çalışma zamanı gereklidir [Önkoşullar](#prerequisites). |Evet |

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

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için veri kümeleri makalesine bakın. Bu bölümde, SAP HANA veri kümesi tarafından desteklenen özellikler listesini sağlar.

SAP HANA verileri kopyalamak için kümesine tür özelliği ayarlamak **RelationalTable**. SAP HANA dataset için desteklenen hiçbir türüne özgü özellikleri varken RelationalTable yazın.

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

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde, SAP HANA kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="sap-hana-as-source"></a>SAP HANA kaynağı olarak

SAP HANA verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **RelationalSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **RelationalSource** | Evet |
| sorgu | SAP HANA örneğinden verileri okumak için SQL sorgusu belirtir. | Evet |

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

SAP HANA veri kopyalama işlemi sırasında aşağıdaki eşlemelerini SAP HANA veri türlerinden Azure Data Factory geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) nasıl kopyalama etkinliği kaynak şema ve veri türü için havuz eşlemeleri hakkında bilgi edinmek için.

| SAP HANA veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| ALPHANUM | Dize |
| BIGINT | Int64 |
| BLOB | Byte] |
| BOOLE DEĞERİ | Bayt |
| CLOB | Byte] |
| DATE | Tarih Saat |
| DECIMAL | Ondalık |
| ÇİFT | Bekar |
| INT | Int32 |
| NVARCHAR | Dize |
| GERÇEK | Bekar |
| SECONDDATE | Tarih Saat |
| SMALLINT | Int16 |
| ZAMAN | TimeSpan |
| TIMESTAMP | Tarih Saat |
| MİNİ TAMSAYI | Bayt |
| VARCHAR | Dize |

## <a name="known-limitations"></a>Bilinen sınırlamaları

SAP HANA veri kopyalama işlemi sırasında birkaç bilinen sınırlamalar vardır:

- En fazla 4000 Unicode karakter uzunluğunu NVARCHAR dizeleri kesiliyor
- SMALLDECIMAL desteklenmiyor
- VARBINARY desteklenmiyor
- Geçerli bir tarih olan 12/1899/30 arasında ile 9999/12/31


## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).