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
ms.topic: conceptual
ms.date: 07/02/2019
ms.author: jingwang
ms.openlocfilehash: 63f28c8b6eaceed12e1f76e9c0c5984e3b63b500
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561413"
---
# <a name="copy-data-from-teradata-using-azure-data-factory"></a>Teradata, Azure Data Factory kullanarak verileri kopyalama
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
>
> * [Sürüm 1](v1/data-factory-onprem-teradata-connector.md)
> * [Geçerli sürüm](connector-teradata.md)

Bu makalede, kopyalama etkinliği Azure Data Factory'de bir Teradata veritabanından veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Teradata veritabanından veri tüm desteklenen havuz veri deposuna kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Teradata bağlayıcı'yı destekler:

- Teradata **sürüm 14.10, 15.0, 15.10, 16,0, 16.10 ve 16.20**.
- Kullanarak verileri kopyalama **temel** veya **Windows** kimlik doğrulaması.
- Teradata kaynak paralel Kopyala. Bkz: [paralel Teradata kopyadan](#parallel-copy-from-teradata) ayrıntıları bölümü.

> [!NOTE]
>
> Azure Data Factory, yerleşik bir ODBC sürücüsü tarafından yetkilendirilmiş ve esnek bağlantı seçenekleri yanı sıra kullanıma hazır paralel kopyalama performansını artırmak üzere sunan şirket içinde barındırılan tümleştirme çalışma zamanı v3.18 beri Teradata bağlayıcı yükseltildi. Teradata hala olarak desteklenen tüm mevcut iş yüklerini önceki Teradata Bağlayıcısı'nı kullanarak .NET veri sağlayıcısı tarafından yetkilendirilmiş-olan ancak bundan sonra yeni bir tane kullanmak için önerilir. Yeni yolunu not alın, bağlantılı veri kümesi/service/kopyalama kaynağı farklı bir dizi gerektirir. Yapılandırma ayrıntıları ilgili bölümüne bakın.

## <a name="prerequisites"></a>Önkoşullar

Teradata, genel olarak erişilebilir durumda değilse, şirket içinde barındırılan tümleştirme çalışma zamanını oluşturan ayarlayın gerekir. Tümleştirme çalışma zamanı hakkında daha fazla bilgi için bkz. [şirket içinde barındırılan tümleştirme çalışma zamanı](create-self-hosted-integration-runtime.md). Tümleştirme çalışma zamanı 3.18 sürümünden başlayarak, yerleşik bir Teradata sürücü sağlar, bu nedenle herhangi bir sürücü el ile yüklemeniz gerekmez. Sürücü gerektirir "görsel C++ yeniden dağıtılabilir 2012 Update 4" şirket içinde barındırılan IR makinede indirmesine [burada](https://www.microsoft.com/en-sg/download/details.aspx?id=30679) , henüz yüklü değilse.

3\.18 düşük şirket içinde barındırılan IR sürümü için yüklemeniz gerekir. [Teradata için .NET veri sağlayıcısı](https://go.microsoft.com/fwlink/?LinkId=278886) 14 sürümü veya üzeri tümleştirme çalışma zamanı makinesinde. 

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli Teradata bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Bağlı Teradata hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Teradata** | Evet |
| connectionString | Teradata veritabanına bağlanmak için gereken bilgileri belirtir. Aşağıdaki örneklere bakın.<br/>Parola Azure anahtar kasası ve çekme koyabilirsiniz `password` yapılandırma bağlantı dizesini dışında. Başvurmak [kimlik bilgilerini Azure Key Vault'ta Store](store-credentials-in-key-vault.md) daha fazla ayrıntı içeren makalesi. | Evet |
| username | Teradata veritabanına bağlanmak için kullanıcı adı belirtin. Windows kimlik doğrulaması kullanılırken geçerlidir. | Hayır |
| password | Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. Ayrıca da tercih edebilirsiniz [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). <br>Windows kimlik doğrulaması kullanılırken uygulanır veya temel kimlik doğrulaması için parola anahtar Kasası'nda başvuruyor. | Hayır |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Belirtildiği gibi bir şirket içinde barındırılan tümleştirme çalışma zamanı gereklidir [önkoşulları](#prerequisites). |Evet |

**Örnek: temel kimlik doğrulaması kullanma**

```json
{
    "name": "TeradataLinkedService",
    "properties": {
        "type": "Teradata",
        "typeProperties": {
            "connectionString": "DBCName=<server>;Uid=<username>;Pwd=<password>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek: Windows kimlik doğrulaması kullanma**

```json
{
    "name": "TeradataLinkedService",
    "properties": {
        "type": "Teradata",
        "typeProperties": {
            "connectionString": "DBCName=<server>",
            "username": "<username>",
            "password": "<password>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

> [!NOTE]
>
> Aşağıdaki yük ile Teradata için .NET veri sağlayıcısı tarafından sunulan bağlı hizmet Teradata kullandıysanız, hala olarak desteklenmektedir-olan ancak bundan sonra yeni bir tane kullanmak için önerilir.

**Önceki yükü:**

```json
{
    "name": "TeradataLinkedService",
    "properties": {
        "type": "Teradata",
        "typeProperties": {
            "server": "<server>",
            "authenticationType": "<Basic/Windows>",
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

Teradata verileri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **TeradataTable** | Evet |
| database | Teradata veritabanı adı. | Hayır (etkinlik kaynağı "sorgu" belirtilmişse) |
| table | Teradata veritabanı tablosunun adı. | Hayır (etkinlik kaynağı "sorgu" belirtilmişse) |

**Örnek:**

```json
{
    "name": "TeradataDataset",
    "properties": {
        "type": "TeradataTable",
        "linkedServiceName": {
            "referenceName": "<Teradata linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

> [!NOTE]
>
> Aşağıdaki gibi "RelationalTable" türü veri kümesi kullanıyorsanız, hala olarak desteklenmektedir-olan ancak bundan sonra yeni bir tane kullanmak için önerilir.

**Önceki yükü:**

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

> [!TIP]
>
> Daha fazla bilgi [paralel Teradata kopyadan](#parallel-copy-from-teradata) bölümünü verimli bir şekilde veri bölümleme kullanılarak Teradata verileri yüklenemedi.

Teradata verileri kopyalamak için kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **TeradataSource** | Evet |
| query | Verileri okumak için özel bir SQL sorgusu kullanın. Örneğin: `"SELECT * FROM MyTable"`.<br>Bölümlenmiş yük etkinleştirdiğinizde, sorgunuzda karşılık gelen yerleşik bölüm parametreyle bağlamanız gerekir. Örneklere bakın [paralel Teradata kopyadan](#parallel-copy-from-teradata) bölümü. | Yok (veri kümesi tablosunda belirtilmişse) |
| partitionOptions | Veri bölümleme Teradata verileri yüklemek için kullanılan seçenekleri belirtir. <br>Değerler: izin ver: **Hiçbiri** (varsayılan), **karma** ve **DynamicRange**.<br>Bölüm seçeneği etkin olduğunda (değil ' None'), lütfen de yapılandırmanız **[ `parallelCopies` ](copy-activity-performance.md#parallel-copy)** kopyalama etkinliği örneğin 4 olarak ayarlama, belirleyen paralel derecede eşzamanlı olarak Teradata verileri yüklemek için Veritabanı. | Hayır |
| partitionSettings | Veri bölümleme için ayar grubu belirtin. <br>Bölüm seçenek olmadığı durumlarda uygulama `None`. | Hayır |
| partitionColumnName | Kaynak sütunun adını **Tamsayı türünde** kullanılacak aralığı için paralel bir kopya olarak bölümleyerek. Belirtilmezse, tablonun birincil anahtarı algılandı ve bölüm sütunu kullanılan otomatik olarak oluşturulacak. <br>Bölüm seçeneği olduğunda geçerli `Hash` veya `DynamicRange`. Kaynak verilerini almak için sorgu kullanın, bağlama `?AdfHashPartitionCondition` veya `?AdfRangePartitionColumnName` WHERE yan tümcesinde. Örnekte bakın [paralel Teradata kopyadan](#parallel-copy-from-teradata) bölümü. | Hayır |
| partitionUpperBound | Veri kopyalamak için bölüm sütunu en büyük değeri. <br>Bölüm seçeneği olduğunda geçerli `DynamicRange`. Kaynak verilerini almak için sorgu kullanın, bağlama `?AdfRangePartitionUpbound` WHERE yan tümcesinde. Örnekte bakın [paralel Teradata kopyadan](#parallel-copy-from-teradata) bölümü. | Hayır |
| partitionLowerBound | Veri kopyalamak için bölüm sütunu en küçük değeri. <br>Bölüm seçeneği olduğunda geçerli `DynamicRange`. Kaynak verilerini almak için sorgu kullanın, bağlama `?AdfRangePartitionLowbound` WHERE yan tümcesinde. Örnekte bakın [paralel Teradata kopyadan](#parallel-copy-from-teradata) bölümü. | Hayır |

> [!NOTE]
>
> "RelationalSource" türü kopyalama kaynağı kullandıysanız, hala olarak desteklenmektedir-olduğu halde yeni yerleşik paralel yüklerinin Teradata (bölüm seçenekleri) desteklemiyor. İleride bu yeni bir tane kullanmak için önerilir.

**Örnek: temel sorgu bölümü olmadan'ni kullanarak veri kopyalama**

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
                "type": "TeradataSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="parallel-copy-from-teradata"></a>Teradata paralel Kopyala

Veri Fabrikası Teradata Bağlayıcısı muhteşem bir performans ile paralel Teradata verileri kopyalamak için bölümleme yerleşik veri sağlar. Bulabilirsiniz Teradata kaynak kopyalama etkinliği verileri bölümleme Seçenekler ->:

![Bölüm seçenekleri](./media/connector-teradata/connector-teradata-partition-options.png)

Veri Fabrikası paralel sorgular bölümlenmiş kopyalama etkinleştirdiğinizde, Teradata kaynak bölümler tarafından verileri yüklemek için karşı çalışır. Paralel derece yapılandırılır ve gösterirken **[ `parallelCopies` ](copy-activity-performance.md#parallel-copy)** kopyalama etkinliğinde ayarlama. Örneğin, ayarlarsanız `parallelCopies` dört, eşzamanlı olarak veri fabrikası oluşturur ve dört sorgularını çalıştırır, Teradata veritabanından veri alınırken her bölümü, belirtilen bölüm seçenek ve ayarlar alınarak.

Teradata veritabanından özellikle büyük miktarda veri yüklediğinizde bölümleme verilerle paralel kopyasını etkinleştirmek için önerilir. Farklı senaryolar için önerilen yapılandırmaları şunlardır:

| Senaryo                                                     | Önerilen ayarları                                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Büyük tablosundan tam yük                                   | **Bölüm seçeneği**: Karma. <br><br/>Yürütme sırasında data Factory otomatik olarak PK sütun algılamak, onu karşı karma uygulama ve veri bölümleri tarafından kopyalayın. |
| Büyük miktarda veri özel sorgu kullanarak yükleme                 | **Bölüm seçeneği**: Karma.<br>**Sorgu**: `SELECT * FROM <TABLENAME> WHERE ?AdfHashPartitionCondition AND <your_additional_where_clause>`.<br>**Bölüm sütunu**: Uygula karma bölüm için kullanılan sütun belirtin. Belirtilmezse, ADF Teradata veri kümesinde belirtilen tablonun PK sütunu otomatik olarak algılar.<br><br>Yürütme, veri fabrikası değiştirme sırasında `?AdfHashPartitionCondition` karma bölüm mantığı ve Teradata Gönder. |
| Büyük boyutlu bir tamsayı sütunu aralık bölümleme eşit olarak dağıtılmış değerine sahip olan, özel bir sorgu kullanarak verileri yükleme | **Bölüm seçenekleri**: Dinamik aralık bölümü.<br>**Sorgu**: `SELECT * FROM <TABLENAME> WHERE ?AdfRangePartitionColumnName <= ?AdfRangePartitionUpbound AND ?AdfRangePartitionColumnName >= ?AdfRangePartitionLowbound AND <your_additional_where_clause>`.<br>**Bölüm sütunu**: Verileri bölümlemek için kullanılan sütun belirtin. Tamsayı veri türü ile sütun karşı bölümleyebilirsiniz.<br>**Bölüm üst sınır** ve **bölüm alt sınır**: Yalnızca alt ve üst aralık arasında veri almak için bölüm sütunu karşı filtrelemek isteyip istemediğinizi belirtin.<br><br>Yürütme, veri fabrikası değiştirme sırasında `?AdfRangePartitionColumnName`, `?AdfRangePartitionUpbound`, ve `?AdfRangePartitionLowbound` gerçek sütun adı ve değer aralıkları her bölüm ve Teradata için gönderin. <br>Örneğin, bölüm sütunu "ID" alt sınırı 1 ile 4 olarak paralel kopya kümesi ile 80'i olarak üst sınır olarak ayarlarsanız ADF veri alma [21, 40], kimliği [1,20] arasında 4 bölüm tarafından [41, 60] ve [61, 80]. |

**Örnek: Sorgu karma bölümü**

```json
"source": {
    "type": "TeradataSource",
    "query": "SELECT * FROM <TABLENAME> WHERE ?AdfHashPartitionCondition AND <your_additional_where_clause>",
    "partitionOption": "Hash",
    "partitionSettings": {
        "partitionColumnName": "<hash_partition_column_name>"
    }
}
```

**Örnek: Sorgu dinamik aralık bölümü**

```json
"source": {
    "type": "TeradataSource",
    "query": "SELECT * FROM <TABLENAME> WHERE ?AdfRangePartitionColumnName <= ?AdfRangePartitionUpbound AND ?AdfRangePartitionColumnName >= ?AdfRangePartitionLowbound AND <your_additional_where_clause>",
    "partitionOption": "DynamicRange",
    "partitionSettings": {
        "partitionColumnName": "<dynamic_range_partition_column_name>",
        "partitionUpperBound": "<upper_value_of_partition_column>",
        "partitionLowerBound": "<lower_value_of_partition_column>"
    }
}
```

## <a name="data-type-mapping-for-teradata"></a>Teradata için eşleme veri türü

Teradata veri kopyalama işlemi sırasında aşağıdaki eşlemeler Teradata veri türlerinden Azure veri fabrikası geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) eşlemelerini nasıl yapar? kopyalama etkinliği kaynak şema ve veri türü için havuz hakkında bilgi edinmek için.

| Teradata veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| BigInt |Int64 |
| Blob |Byte[] |
| Byte |Byte[] |
| ByteInt |Int16 |
| Char |String |
| Clob |String |
| Date |DateTime |
| Decimal |Decimal |
| Double |Double |
| Graphic |Desteklenmiyor. Kaynak Query'de açık tür dönüştürme uygulanır. |
| Integer |Int32 |
| Interval Day |Desteklenmiyor. Kaynak Query'de açık tür dönüştürme uygulanır. |
| Interval Day To Hour |Desteklenmiyor. Kaynak Query'de açık tür dönüştürme uygulanır. |
| Interval Day To Minute |Desteklenmiyor. Kaynak Query'de açık tür dönüştürme uygulanır. |
| Interval Day To Second |Desteklenmiyor. Kaynak Query'de açık tür dönüştürme uygulanır. |
| Interval Hour |Desteklenmiyor. Kaynak Query'de açık tür dönüştürme uygulanır. |
| Interval Hour To Minute |Desteklenmiyor. Kaynak Query'de açık tür dönüştürme uygulanır. |
| Interval Hour To Second |Desteklenmiyor. Kaynak Query'de açık tür dönüştürme uygulanır. |
| Interval Minute |Desteklenmiyor. Kaynak Query'de açık tür dönüştürme uygulanır. |
| Interval Minute To Second |Desteklenmiyor. Kaynak Query'de açık tür dönüştürme uygulanır. |
| Interval Month |Desteklenmiyor. Kaynak Query'de açık tür dönüştürme uygulanır. |
| Interval Second |Desteklenmiyor. Kaynak Query'de açık tür dönüştürme uygulanır. |
| Interval Year |Desteklenmiyor. Kaynak Query'de açık tür dönüştürme uygulanır. |
| Interval Year To Month |Desteklenmiyor. Kaynak Query'de açık tür dönüştürme uygulanır. |
| Sayı |Double |
| Süre (tarih) |Desteklenmiyor. Kaynak Query'de açık tür dönüştürme uygulanır. |
| Süre (saat) |Desteklenmiyor. Kaynak Query'de açık tür dönüştürme uygulanır. |
| Süresi (saat dilimiyle birlikte) |Desteklenmiyor. Kaynak Query'de açık tür dönüştürme uygulanır. |
| Süresi (zaman damgası) |Desteklenmiyor. Kaynak Query'de açık tür dönüştürme uygulanır. |
| Süre (saat dilimi ile zaman damgası) |Desteklenmiyor. Kaynak Query'de açık tür dönüştürme uygulanır. |
| Integer |Int16 |
| Time |TimeSpan |
| Time With Time Zone |TimeSpan |
| Timestamp |DateTime |
| Timestamp With Time Zone |DateTime |
| VarByte |Byte[] |
| VarChar |String |
| VarGraphic |Desteklenmiyor. Kaynak Query'de açık tür dönüştürme uygulanır. |
| Xml |Desteklenmiyor. Kaynak Query'de açık tür dönüştürme uygulanır. |


## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
