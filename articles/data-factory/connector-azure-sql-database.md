---
title: "Veri kopyalama/Azure SQL Data Factory kullanarak veritabanından | Microsoft Docs"
description: "Azure SQL veritabanı için desteklenen kaynak veri depoları (veya) SQL veritabanına veri kopyalamak desteklenen havuz veri depolarına Data Factory kullanarak öğrenin."
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
ms.date: 08/30/2017
ms.author: jingwang
ms.openlocfilehash: a5d2994eb1203274454fc31c3ee9bf7a21562f75
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="copy-data-to-or-from-azure-sql-database-by-using-azure-data-factory"></a>Azure Data Factory kullanarak veya Azure SQL veritabanından veri kopyalayın
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-azure-sql-connector.md)
> * [Sürüm 2 - Önizleme](connector-azure-sql-database.md)

Bu makalede kopya etkinliği Azure Data Factory'de gelen ve bir Azure SQL veritabanına veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 Azure SQL Veritabanı Bağlayıcısı](v1/data-factory-azure-sql-connector.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna başlangıç/bitiş Azure SQL veritabanına veri kopyalamak ya da veri tüm desteklenen kaynak veri deposundan Azure SQL veritabanına kopyalamak. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Azure SQL veritabanı bağlayıcı destekler:

- SQL kimlik doğrulaması kullanarak veri kopyalama.
- SQL sorgusu veya saklı yordam kullanarak veri kaynağı olarak alınıyor.
- Havuz veri hedef tablo ya da kopyalama sırasında özel mantık olan bir saklı yordam çağırma sonuna ekleme.

## <a name="getting-started"></a>Başlarken
.NET SDK'sı, Python SDK'sı, Azure PowerShell, REST API veya Azure Resource Manager şablonu kullanarak kopyalama etkinliği ile işlem hattı oluşturabilirsiniz. Bkz: [kopyalama etkinliği öğretici](quickstart-create-data-factory-dot-net.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

Aşağıdaki bölümler, Azure SQL veritabanı bağlayıcıya Data Factory varlıklarını belirli tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler, Azure SQL veritabanı bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **AzureSqlDatabase** | Evet |
| connectionString |ConnectionString özelliği için Azure SQL veritabanı örneğine bağlanmak için gereken bilgileri belirtin. Yalnızca temel kimlik doğrulama desteklenir. Bu alan bir SecureString işaretleyin. |Evet |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu özel bir ağda yer alıyorsa) Azure tümleştirmesi çalışma zamanı veya Self-hosted tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. |Hayır |

> [!IMPORTANT]
> Yapılandırma [Azure SQL veritabanı Güvenlik Duvarı](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) veritabanı sunucusuna [Azure hizmetlerinin sunucuya erişimine izin](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure). Dış Azure data factory ile şirket içi veri kaynaklarından kendini barındıran tümleştirmesi çalışma zamanı dahil olmak üzere Azure SQL veritabanına veri kopyalıyorsanız, ayrıca, Azure SQL veri gönderme makine için uygun IP adresi aralığı yapılandırın Veritabanı.

**Örnek:**

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
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

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için veri kümeleri makalesine bakın. Bu bölümde Azure SQL veritabanı veri kümesi tarafından desteklenen özellikler listesini sağlar.

/ Azure SQL veritabanına veri kopyalamak için veri kümesi için tür özelliği ayarlamak **AzureSqlTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak: **AzureSqlTable** | Evet |
| tableName |Tablo veya Görünüm bağlantılı hizmetinin Azure SQL veritabanı örneğinde başvurduğu adı. | Evet |

**Örnek:**

```json
{
    "name": "AzureSQLDbDataset",
    "properties":
    {
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "<Azure SQL Database linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde Azure SQL veritabanı kaynak ve havuz tarafından desteklenen özellikler listesini sağlar.

### <a name="azure-sql-database-as-source"></a>Kaynak olarak Azure SQL veritabanı

Azure SQL veritabanından veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **SqlSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **SqlSource** | Evet |
| sqlReaderQuery |Verileri okumak için özel SQL sorgusu kullanın. Örnek: `select * from MyTable`. |Hayır |
| sqlReaderStoredProcedureName |Kaynak tablodan veri okuyan saklı yordamın adı. Son SQL deyimi SELECT deyimi içinde saklı yordamı olması gerekir. |Hayır |
| storedProcedureParameters |Saklı yordam parametreleri.<br/>İzin verilen değerler: ad/değer çiftleri. Adları ve büyük/küçük harf parametrelerinin adlarını ve saklı yordam parametreleri büyük/küçük harf eşleşmelidir. |Hayır |

**Dikkat edilecek noktalar:**

- Varsa **sqlReaderQuery** belirtilen SqlSource için kopyalama etkinliği veri almak için Azure SQL veritabanı kaynağında bu sorguyu çalıştırır. Alternatif olarak, bir saklı yordam belirterek belirleyebileceğiniz **sqlReaderStoredProcedureName** ve **storedProcedureParameters** (saklı yordam parametreleri alıyorsa).
- "SqlReaderQuery" veya "sqlReaderStoredProcedureName" belirtmezseniz, JSON veri kümesi'nin "yapısı" bölümünde tanımlanan sütunları bir sorgu oluşturmak için kullanılır (`select column1, column2 from mytable`) Azure SQL veritabanına karşı çalıştırmak için. Veri kümesi tanımı "yapısı" yoksa, tüm sütunları tablodan seçilir.
- Kullandığınızda **sqlReaderStoredProcedureName**, yine de bir kukla belirtmeniz gerekiyorsa **tableName** JSON veri kümesi bir özellik.

**Örnek: SQL sorgusu kullanma**

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL Database input dataset name>",
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
                "type": "SqlSource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**Örnek: saklı yordam kullanma**

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL Database input dataset name>",
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
                "type": "SqlSource",
                "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
                "storedProcedureParameters": {
                    "stringData": { "value": "str3" },
                    "identifier": { "value": "$$Text.Format('{0:yyyy}', <datetime parameter>)", "type": "Int"}
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**Saklı yordam tanımı:**

```sql
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
     select *
     from dbo.UnitTestSrcTable
     where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="azure-sql-database-as-sink"></a>Havuz olarak Azure SQL veritabanı

Azure SQL veritabanına veri kopyalamak için kopyalama etkinliği Havuz türü ayarlayın. **SqlSink**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopya etkinliği havuz tür özelliği ayarlamak: **SqlSink** | Evet |
| writeBatchSize |Arabellek boyutu writeBatchSize ulaştığında veri SQL tablosuna ekler.<br/>İzin verilen değerler: tamsayı (satır sayısı). |Hayır (varsayılan değeri: 10000) |
| writeBatchTimeout |Toplu ekleme işlemi zaman aşımına uğramadan önce tamamlamak bir süre bekleyin.<br/>İzin verilen değerler: timespan. Örnek: "00: 30:00" (30 dakika). |Hayır |
| sqlWriterStoredProcedureName |Saklı yordam adı hedef tabloda bu upserts (güncelleştirmeler/ekler) verileri. |Hayır |
| storedProcedureParameters |Saklı yordam parametreleri.<br/>İzin verilen değerler: ad/değer çiftleri. Adları ve büyük/küçük harf parametrelerinin adlarını ve saklı yordam parametreleri büyük/küçük harf eşleşmelidir. |Hayır |
| sqlWriterTableType |Saklı yordam, kullanılacak bir tablo türü adı belirtin. Kopyalama etkinliği taşınan veri geçici bir tablo bu tablo türü ile kullanılabilir hale getirir. Saklı yordam kodu ardından var olan verilerle kopyalanan verileri birleştirebilirsiniz. |Hayır |
| preCopyScript |Verileri Azure SQL veritabanına her çalışmasında yazmadan önce yürütmek kopyalama etkinliği için bir SQL sorgusunu belirtin. Önceden yüklenmiş veriyi temizlemek için bu özelliği kullanın. |Hayır |

> [!TIP]
> Azure SQL Database'e veri kopyalama, kopyalama etkinliği verileri varsayılan olarak havuz tabloya ekler. UPSERT veya ek iş mantığı gerçekleştirmek için SqlSink içinde saklı yordamı kullanın. Daha fazla ayrıntıyı öğrenin [SQL havuz için saklı yordam çağırma](#invoking-stored-procedure-for-sql-sink).

**Örnek 1: veri ekleme**

```json
"activities":[
    {
        "name": "CopyToAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure SQL Database output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "writeBatchSize": 100000
            }
        }
    }
]
```

**Örnek 2: upsert kopyalama sırasında bir saklı yordamı çağırma**

Daha fazla ayrıntıyı öğrenin [SQL havuz için saklı yordam çağırma](#invoking-stored-procedure-for-sql-sink).

```json
"activities":[
    {
        "name": "CopyToAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure SQL Database output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
                "sqlWriterTableType": "CopyTestTableType",
                "storedProcedureParameters": {
                    "identifier": { "value": "1", "type": "Int" },
                    "stringData": { "value": "str1" }
                }
            }
        }
    }
]
```

## <a name="identity-columns-in-the-target-database"></a>Hedef veritabanında kimlik sütunu

Bu bölüm veri kaynak tablodaki bir kimlik sütunu olmadan bir kimlik sütunu içeren bir hedef tablo kopyalamak için bir örnek verilmektedir.

**Kaynak tablosu:**

```sql
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```

**Hedef Tablo:**

```sql
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```

Hedef tabloda bir kimlik sütunu olduğuna dikkat edin.

**Kaynak veri kümesi JSON tanımı**

```json
{
    "name": "SampleSource",
    "properties": {
        "type": " AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "TestIdentitySQL",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "SourceTbl"
        }
    }
}
```

**Hedef veri kümesi JSON tanımı**

```json
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "TestIdentitySQL",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "TargetTbl"
        }
    }
}
```

Kaynak ve hedef tablosu farklı şema sahip dikkat edin (hedef kimliğine sahip başka bir sütuna sahip). Bu senaryoda, belirtmeniz gerekir. **yapısı** kimlik sütunu içermeyen hedef veri kümesi tanımında özelliği.

## <a name="invoke-stored-procedure-from-sql-sink"></a>SQL havuz depolanan yordamı çağırma

Depolanan verileri Azure SQL veritabanına bir kullanıcı tarafından belirtilen kopyalarken yordamı yapılandırılabilir ve ek parametrelerle çağrılır.

Yerleşik kopyalama mekanizmaları amacı sunmaz bir saklı yordam kullanılabilir. Upsert (INSERT + güncelleştirme) veya (sütunları, ek değerleri, birden çok tablo, vb. ekleme bakarak birleştirme) ek işleme kaynak verilerin hedef tabloda son ekleme önce yapılması gerektiğinde genellikle kullanılır.

Aşağıdaki örnek, Azure SQL veritabanındaki bir tabloya bir upsert yapmak için bir saklı yordam kullanmayı gösterir. Giriş verilerinin ve "Pazarlama" havuz tablo varsayılarak üç sütun vardır: Profileıd, durumu ve kategori. Upsert "Profileıd" sütununa dayalı gerçekleştirin ve yalnızca belirli bir kategorideki için geçerlidir.

**Çıktı veri kümesi**

```json
{
    "name": "AzureSQLDbDataset",
    "properties":
    {
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "<Azure SQL Database linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "Marketing"
        }
    }
}
```

"SqlSink" bölümü kopyalama etkinliği aşağıdaki gibi tanımlayın.

```json
"sink": {
    "type": "SqlSink",
    "SqlWriterTableType": "MarketingType",
    "SqlWriterStoredProcedureName": "spOverwriteMarketing",
    "storedProcedureParameters": {
        "category": {
            "value": "ProductA"
        }
    }
}
```

Veritabanınızda, "SqlWriterStoredProcedureName" adıyla aynı saklı yordamını tanımlayın. Bu, çıkış tablosuna belirtilen kaynak ve birleştirme giriş verilerini işler. Saklı yordam parametresinin adı "kümesinde tanımlanan tableName" ile aynı olmalıdır dikkat edin.

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @category varchar(256)
AS
BEGIN
  MERGE [dbo].[Marketing] AS target
  USING @Marketing AS source
  ON (target.ProfileID = source.ProfileID and target.Category = @category)
  WHEN MATCHED THEN
      UPDATE SET State = source.State
  WHEN NOT MATCHED THEN
      INSERT (ProfileID, State, Category)
      VALUES (source.ProfileID, source.State, source.Category)
END
```

Veritabanınızda, aynı ada sahip bir tablo türü sqlWriterTableType tanımlayın. Tablo türü şeması giriş verilerinizi tarafından döndürülen şema ile aynı olması gerektiğini unutmayın.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL，
    [Category] [varchar](256) NOT NULL，
)
```

Saklı yordam özellik yararlandığı [tablo değerli parametreleri](https://msdn.microsoft.com/library/bb675163.aspx).

## <a name="data-type-mapping-for-azure-sql-database"></a>Azure SQL veritabanı için veri türü eşlemesi

Başlangıç/bitiş Azure SQL veritabanı veri kopyalama işlemi sırasında aşağıdaki eşlemelerini Azure SQL veritabanı veri türlerinden Azure Data Factory geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) nasıl kopyalama etkinliği kaynak şema ve veri türü için havuz eşlemeleri hakkında bilgi edinmek için.

| Azure SQL veritabanı veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| bigint |Int64 |
| İkili |Byte] |
| bit |Boole değeri |
| char |Dize, Char] |
| Tarih |Tarih saat |
| Tarih saat |Tarih saat |
| datetime2 |Tarih saat |
| Datetimeoffset |DateTimeOffset |
| Ondalık |Ondalık |
| FILESTREAM özniteliği (varbinary(max)) |Byte] |
| Kayan nokta |Çift |
| Görüntü |Byte] |
| Int |Int32 |
| para |Ondalık |
| nchar |Dize, Char] |
| ntext |Dize, Char] |
| sayısal |Ondalık |
| nvarchar |Dize, Char] |
| Gerçek |Tek |
| rowVersion |Byte] |
| smalldatetime |Tarih saat |
| tamsayı |Int16 |
| küçük para |Ondalık |
| sql_variant |Nesne * |
| Metin |Dize, Char] |
| time |TimeSpan |
| timestamp |Byte] |
| Mini tamsayı |Bayt |
| benzersiz tanımlayıcı |GUID |
| varbinary |Byte] |
| varchar |Dize, Char] |
| xml |XML |

## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).