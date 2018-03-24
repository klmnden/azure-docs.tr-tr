---
title: Azure Data Factory kullanarak için ve Oracle veri kopyalama | Microsoft Docs
description: Veri Fabrikası kullanarak bir Oracle veritabanına desteklenen kaynak depoları veya desteklenen havuz depoları için Oracle veri kopyalamak öğrenin.
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
ms.date: 02/07/2018
ms.author: jingwang
ms.openlocfilehash: aa96356b01d63aa21c55f1b2e6998e65f9d617f6
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="copy-data-from-and-to-oracle-by-using-azure-data-factory"></a>Azure Data Factory kullanarak ilk ve son Oracle veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel kullanıma sunuldu](v1/data-factory-onprem-oracle-connector.md)
> * [Sürüm 2 - Önizleme](connector-oracle.md)

Bu makalede kopyalama etkinliği Azure Data Factory'de gelen ve bir Oracle veritabanına veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir, veri fabrikası 1 sürümünü kullanıyorsanız bkz [Oracle Bağlayıcısı sürüm 1](v1/data-factory-onprem-oracle-connector.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bir Oracle veritabanından veri tüm desteklenen havuz veri deposuna kopyalayabilirsiniz. Ayrıca, tüm desteklenen kaynak veri deposundan Oracle veritabanına veri kopyalayabilirsiniz. Kaynakları veya havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Oracle bağlayıcı bir Oracle veritabanına aşağıdaki sürümlerini destekler. Ayrıca, temel veya OID kimlik doğrulamaları destekler:

- Oracle 12c R1 (12.1)
- Oracle 11g R1, R2 (11.1, 11.2)
- Oracle 10g R1, R2 (10.1, 10.2)
- Oracle 9i R1, R2 (9.0.1, 9.2)
- Oracle 8i R3 (8.1.7)

## <a name="prerequisites"></a>Önkoşullar

Gelen ve genel olarak erişilebilir değil bir Oracle veritabanına veri kopyalamak için bir Self-hosted tümleştirmesi çalışma zamanı ayarlamanız gerekir. Tümleştirme çalışma zamanı hakkında daha fazla bilgi için bkz: [Self-hosted tümleştirmesi çalışma zamanı](create-self-hosted-integration-runtime.md). Tümleştirme çalışma zamanı yerleşik bir Oracle sürücü sağlar. Bu nedenle, ilk ve son Oracle veri kopyaladığınızda, bir sürücüyü el ile yüklemeniz gerekmez.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Oracle bağlayıcıya Data Factory varlıklarını belirli tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler Oracle bağlantılı hizmeti için desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlamak **Oracle**. | Evet |
| connectionString | Oracle veritabanına bağlanmak için gereken bilgileri belirtir. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md).<br><br>**Bağlantı türü desteklenen**: kullanabileceğiniz **Oracle SID** veya **Oracle hizmet adı** veritabanınızı tanımlamak için:<br>-SID kullanıyorsanız: `Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;`<br>-Hizmet adı kullanıyorsanız: `Host=<host>;Port=<port>;ServiceName=<sid>;User Id=<username>;Password=<password>;` | Evet |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu genel olarak erişilebilir ise) Self-hosted tümleştirmesi çalışma zamanı veya Azure tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. |Hayır |

**Örnek:**

```json
{
    "name": "OracleLinkedService",
    "properties": {
        "type": "Oracle",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;"
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

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, Oracle veri kümesi tarafından desteklenen özellikler listesini sağlar.

Gelen ve Oracle veri kopyalamak için kümesine tür özelliği ayarlamak **OracleTable**. Aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak **OracleTable**. | Evet |
| tableName |Bağlantılı hizmet başvurduğu Oracle veritabanı tablosunun adı. | Evet |

**Örnek:**

```json
{
    "name": "OracleDataset",
    "properties":
    {
        "type": "OracleTable",
        "linkedServiceName": {
            "referenceName": "<Oracle linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde Oracle kaynak ve havuz tarafından desteklenen özellikler listesini sağlar.

### <a name="oracle-as-a-source-type"></a>Oracle bir kaynak türü

Oracle kaynağından veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **OracleSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak **OracleSource**. | Evet |
| oracleReaderQuery | Verileri okumak için özel SQL sorgusu kullanın. `"SELECT * FROM MyTable"` bunun bir örneğidir. | Hayır |

"OracleReaderQuery" belirtmezseniz, veri kümesi'nin "yapısı" bölümünde tanımlanan sütunları bir sorgu oluşturmak için kullanılır (`select column1, column2 from mytable`) Oracle veritabanına karşı çalıştırmak için. Veri kümesi tanımı "yapısı" yoksa, tüm sütunları tablodan seçilir.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromOracle",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Oracle input dataset name>",
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
                "type": "OracleSource",
                "oracleReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="oracle-as-a-sink-type"></a>Havuz türü olarak Oracle

Oracle için veri kopyalamak için kopyalama etkinliği Havuz türü ayarlayın. **OracleSink**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **havuz** bölümü.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopya etkinliği havuz tür özelliği ayarlamak **OracleSink**. | Evet |
| writeBatchSize | Arabellek boyutu writeBatchSize ulaştığında veri SQL tablosuna ekler.<br/>Tamsayı (satır sayısı) izin verilen değerler. |Hayır (varsayılan değeri: 10.000) |
| writeBatchTimeout | Toplu ekleme işlemi zaman aşımına uğramadan önce tamamlamak bir süre bekleyin.<br/>Timespan izin verilen değerler. 00:30:00 (30 dakika) örneğidir. | Hayır |
| preCopyScript | Her çalışmasında Oracle veri yazmadan önce yürütmek kopyalama etkinliği için bir SQL sorgusunu belirtin. Önceden yüklenen verileri temizlemek için bu özelliği kullanın. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToOracle",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Oracle output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "OracleSink"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-oracle"></a>Eşleme için Oracle veri türü

İlk ve son Oracle veri kopyaladığınızda, aşağıdaki eşlemelerini Oracle veri türlerinden Data Factory geçici veri türleri için kullanılır. Nasıl kopyalama etkinliği kaynak şema ve veri türü için havuz eşlemeleri hakkında bilgi edinmek için [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md).

| Oracle veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| BFILE |Byte] |
| BLOB |Byte]<br/>(yalnızca Oracle 10 g desteklenir ve üzeri) |
| CHAR |Dize |
| CLOB |Dize |
| DATE |DateTime |
| KAYAN NOKTA |Ondalık, dize (varsa precision > 28) |
| TAMSAYI |Ondalık, dize (varsa precision > 28) |
| UZUN |Dize |
| LONG RAW |Byte] |
| NCHAR |Dize |
| NCLOB |Dize |
| SAYI |Ondalık, dize (varsa precision > 28) |
| NVARCHAR2 |Dize |
| RAW |Byte] |
| SATIR KİMLİĞİ |Dize |
| TIMESTAMP |DateTime |
| YEREL SAAT DİLİMİ ZAMAN DAMGASI |Dize |
| SAAT DİLİMİ ZAMAN DAMGASI |Dize |
| İŞARETSİZ TAMSAYI |Sayı |
| VARCHAR2 |Dize |
| XML |Dize |

> [!NOTE]
> Veri türleri ARALIĞI Yıl Bitiş ayı ve ARALIĞI gün için ikinci desteklenmiyor.


## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).