---
title: Veri kopyalama/Azure Data Factory kullanarak Oracle | Microsoft Docs
description: "Veri Fabrikası kullanarak Oracle veritabanı için desteklenen kaynak depoları (veya) desteklenen havuz depoları için Oracle veri kopyalamak öğrenin."
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
ms.date: 11/01/2017
ms.author: jingwang
ms.openlocfilehash: f5d6cf07c52920a795c42e7f3578b1666a86d3c5
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="copy-data-from-and-to-oracle-using-azure-data-factory"></a>İlk ve son Azure Data Factory kullanarak Oracle veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-onprem-oracle-connector.md)
> * [Sürüm 2 - Önizleme](connector-oracle.md)

Bu makalede kopya etkinliği Azure Data Factory'de gelen ve bir Oracle veritabanına veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 Oracle Bağlayıcısı](v1/data-factory-onprem-oracle-connector.md).

## <a name="supported-scenarios"></a>Desteklenen senaryolar

Tüm desteklenen havuz veri deposuna Oracle veritabanından veri kopyalama veya tüm desteklenen kaynak veri deposundan Oracle veritabanına veri kopyalamak. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Oracle bağlayıcı Oracle veritabanı aşağıdaki sürümlerini destekler ve temel veya OID kimlik doğrulamaları destekler.

    - Oracle 12c R1 (12,1)
    - Oracle 11g R1, R2 (11.1, 11.2)
    - Oracle 10g R1, R2 (10,1, 10.2)
    - Oracle 9i R1, R2 (9.0.1, 9.2)
    - Oracle 8i R3 (8.1.7)

## <a name="prerequisites"></a>Ön koşullar

Başlangıç/bitiş genel olarak erişilebilir değil bir Oracle veritabanına veri kopyalamak için bir Self-hosted tümleştirmesi çalışma zamanı ayarlamanız gerekir. Bkz: [Self-hosted tümleştirmesi çalışma zamanı](create-self-hosted-integration-runtime.md) makale tümleştirmesi çalışma zamanı hakkında ayrıntılı bilgi için. Yerleşik bir Oracle sürücü tümleştirmesi çalışma zamanı sağlar, bu nedenle herhangi bir sürücüsü başlangıç/bitiş Oracle veri kopyalama işlemi sırasında el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken
.NET SDK'sı, Python SDK'sı, Azure PowerShell, REST API veya Azure Resource Manager şablonu kullanarak kopyalama etkinliği ile işlem hattı oluşturabilirsiniz. Bkz: [kopyalama etkinliği öğretici](quickstart-create-data-factory-dot-net.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

Aşağıdaki bölümler, Oracle bağlayıcıya Data Factory varlıklarını belirli tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler, Oracle bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Oracle** | Evet |
| connectionString | Oracle veritabanına bağlanmak için gereken bilgileri belirtin. Bu alan bir SecureString işaretleyin. | Evet |
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

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için veri kümeleri makalesine bakın. Bu bölümde, Oracle veri kümesi tarafından desteklenen özellikler listesini sağlar.

Veri kopyalama/Oracle için veri kümesi türü özelliğini ayarlayın **OracleTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak: **OracleTable** | Evet |
| tableName |Oracle veritabanında bağlantılı hizmet başvurduğu tablonun adı. | Evet |

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

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde, Oracle kaynak ve havuz tarafından desteklenen özellikler listesini sağlar.

### <a name="oracle-as-source"></a>Oracle kaynağı olarak

Oracle kaynağından veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **OracleSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **OracleSource** | Evet |
| oracleReaderQuery | Verileri okumak için özel SQL sorgusu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Hayır |

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

### <a name="oracle-as-sink"></a>Oracle havuz olarak

Oracle için veri kopyalamak için kopyalama etkinliği Havuz türü ayarlayın. **OracleSink**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **OracleSink** | Evet |
| writeBatchSize | Arabellek boyutu writeBatchSize ulaştığında veri SQL tablosuna ekler.<br/>İzin verilen değerler: tamsayı (satır sayısı). |Hayır (varsayılan değeri: 10000) |
| writeBatchTimeout | Toplu ekleme işlemi zaman aşımına uğramadan önce tamamlamak bir süre bekleyin.<br/>İzin verilen değerler: Timespan. Örnek: 00:30:00 (30 dakika). | Hayır |
| preCopyScript | Her çalışmasında Oracle veri yazmadan önce yürütmek kopyalama etkinliği için bir SQL sorgusunu belirtin. Önceden yüklenmiş veriyi temizlemek için bu özelliği kullanın. | Hayır |

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

/ İçin Oracle veri kopyalama işlemi sırasında aşağıdaki eşlemelerini Oracle veri türlerinden Azure Data Factory geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) nasıl kopyalama etkinliği kaynak şema ve veri türü için havuz eşlemeleri hakkında bilgi edinmek için.

| Oracle veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| BDOSYA |Byte] |
| BLOB |Byte] |
| CHAR |Dize |
| CLOB |Dize |
| TARİH |Tarih saat |
| KAYAN NOKTA |Ondalık, dize (varsa precision > 28) |
| TAMSAYI |Ondalık, dize (varsa precision > 28) |
| UZUN |Dize |
| UZUN HAM |Byte] |
| NCHAR |Dize |
| NCLOB |Dize |
| SAYI |Ondalık, dize (varsa precision > 28) |
| NVARCHAR2 |Dize |
| HAM |Byte] |
| SATIR KİMLİĞİ |Dize |
| ZAMAN DAMGASI |Tarih saat |
| YEREL SAAT DİLİMİ ZAMAN DAMGASI |Dize |
| SAAT DİLİMİ ZAMAN DAMGASI |Dize |
| İŞARETSİZ TAMSAYI |Sayı |
| VARCHAR2 |Dize |
| XML |Dize |

> [!NOTE]
> Veri türü ARALIĞI yıl ay ve ARALIĞI gün için ikinci desteklenmez.


## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).