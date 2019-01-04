---
title: Veri gönderip buralardan kullanarak Azure SQL veritabanı yönetilen Azure Data Factory örneği kopyalama | Microsoft Docs
description: Veri gönderip buralardan veri kullanarak Azure SQL veritabanı yönetilen Azure Data Factory örneği taşıma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: jingwang
ms.openlocfilehash: df8d337e7950400a86dcab14de4484f4811f43e2
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54025088"
---
# <a name="copy-data-to-and-from-azure-sql-database-managed-instance-using-azure-data-factory"></a>Azure SQL veritabanı yönetilen kullanarak Azure Data Factory örneği gelen ve giden veri kopyalama

Bu makalede, kopyalama etkinliği Azure Data Factory'de gelen ve Azure SQL veritabanı yönetilen örneği, verileri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Azure SQL veritabanı yönetilen örneği tüm desteklenen havuz veri deposuna veri kopyalamak ya da yönetilen örneği'ne herhangi bir desteklenen kaynak veri deposundan veri kopyalayın. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Azure SQL veritabanı yönetilen örneği bağlayıcı'yı destekler:

- Kullanarak verileri kopyalama **SQL** veya **Windows** kimlik doğrulaması.
- SQL sorgusu veya saklı yordamı kullanarak veri kaynağı olarak alınıyor.
- Havuz olarak veriler hedef tablonun veya kopyalama sırasında özel mantığı olan bir saklı yordam çağırma ekleniyor.

## <a name="prerequisites"></a>Önkoşullar

Azure SQL veritabanı yönetilen sanal ağda bulunan örneği verilerini kullanmak için şirket içinde barındırılan tümleştirme çalışma zamanı veritabanına erişebilen aynı VNET içindeki ayarlamanız gerekir. Bkz: [şirket içinde barındırılan tümleştirme çalışma zamanı](create-self-hosted-integration-runtime.md) makale Ayrıntılar için.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli Azure SQL veritabanı yönetilen örneği bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Azure SQL veritabanı yönetilen bağlı örneği hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **SqlServer** | Evet |
| bağlantı dizesi |SQL kimlik doğrulaması veya Windows kimlik doğrulaması kullanarak yönetilen örneği'ne bağlanmak üzere gereken bağlantı dizesi bilgilerini belirtin. Aşağıdaki örneğe bakın. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Evet |
| Kullanıcı adı |Windows kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. Örnek: **domainname\\username**. |Hayır |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Hayır |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Yönetilen Örneğinize aynı sanal AĞA şirket içinde barındırılan tümleştirme çalışma zamanı sağlama. |Evet |

>[!TIP]
>Hata olarak "UserErrorFailedToConnectToSqlServer" hata koduyla isabet ve gibi ileti "veritabanı için oturum sınırı xxx ve üst sınırına ulaşıldı.", ekleme `Pooling=false` bağlantı dizesi ve yeniden deneyin.

**Örnek 1: SQL kimlik doğrulaması kullanma**

```json
{
    "name": "SqlServerLinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek 2: Windows kimlik doğrulaması kullanma**

```json
{
    "name": "SqlServerLinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=True;"
            },
             "userName": "<domain\\username>",
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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için veri kümeleri makalesine bakın. Bu bölümde, Azure SQL veritabanı yönetilen örneği veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Verileri Azure BLOB'dan/Azure SQL veritabanı yönetilen örneği kopyalamak için dataset öğesinin type özelliği ayarlamak **SqlServerTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **SqlServerTable** | Evet |
| tableName |Tablo veya Görünüm veritabanı örneğinde başvuran bağlı hizmetin adı. | Kaynak, havuz için Evet Hayır |

**Örnek**

```json
{
    "name": "SQLServerDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<Managed Instance linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Azure SQL veritabanı yönetilen örneği'nın kaynak ve havuz desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-sql-database-managed-instance-as-source"></a>Azure SQL veritabanı yönetilen örneği kaynağı olarak

Verileri Azure SQL veritabanı yönetilen örneği kopyalamak için kopyalama etkinliği kaynak türünü ayarlayın. **SqlSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **SqlSource** | Evet |
| sqlReaderQuery |Verileri okumak için özel bir SQL sorgusu kullanın. Örnek: `select * from MyTable`. |Hayır |
| sqlReaderStoredProcedureName |Kaynak tablo verilerini okuyan saklı yordamın adı. Son SQL deyim bir SELECT deyimi saklı yordam içinde olmalıdır. |Hayır |
| storedProcedureParameters |Saklı yordamın parametreleri.<br/>İzin verilen değerler: ad/değer çiftleri. Adları ve parametreleri büyük küçük harfleri, adları ve saklı yordam parametreleri büyük küçük harfleri eşleşmelidir. |Hayır |

**Dikkat edilecek noktaları**

- Varsa **sqlReaderQuery** belirtilen SqlSource için kopyalama etkinliği çalıştığında bu sorgu veri almak için yönetilen örneği kaynağında. Alternatif olarak, bir saklı yordam belirterek belirtebileceğiniz **sqlReaderStoredProcedureName** ve **storedProcedureParameters** (saklı yordamın parametreleri sürerse).
- "SqlReaderQuery" veya "sqlReaderStoredProcedureName" özelliğini belirtmezseniz, sütunları veri kümesi JSON "yapı" bölümünde tanımlanan bir sorgu oluşturmak için kullanılır (`select column1, column2 from mytable`) yönetilen örneğine karşı çalıştırılacak. Veri kümesi tanımı "yapı" yoksa, tüm sütunları tablodan seçilir.

**Örnek: Bir SQL sorgusunu kullanarak**

```json
"activities":[
    {
        "name": "CopyFromSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Managed Instance input dataset name>",
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

**Örnek: Bir saklı yordam kullanma**

```json
"activities":[
    {
        "name": "CopyFromSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Managed Instance input dataset name>",
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

**Saklı yordam tanımında**

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

### <a name="azure-sql-database-managed-instance-as-sink"></a>Azure SQL veritabanı yönetilen örneği havuz olarak

Azure SQL veritabanı yönetilen örneği için verileri kopyalamak için kopyalama etkinliğine de Havuz türü ayarlayın. **SqlSink**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuz öğesinin type özelliği ayarlanmalıdır: **SqlSink** | Evet |
| writeBatchSize |Arabellek boyutu writeBatchSize ulaştığında veri SQL tablosuna ekler.<br/>İzin verilen değerler: tamsayı (satır sayısı). |Hayır (varsayılan: 10000) |
| writeBatchTimeout |Toplu ekleme işlemi zaman aşımına uğramadan önce tamamlanması için bir süre bekleyin.<br/>İzin verilen değerler: TimeSpan değeri. Örnek: "00: 30:00" (30 dakika). |Hayır |
| preCopyScript |Yönetilen örneğe veri yazılmadan önce yürütmek kopyalama etkinliği için bir SQL sorgusunu belirtin. Bu yalnızca bir kez çalıştır kopyalama çağrılır. Önceden yüklenmiş ve verileri temizlemek için bu özelliği kullanabilirsiniz. |Hayır |
| sqlWriterStoredProcedureName |Kaynak verileri hedef tabloya örn upsert eder ya da kendi iş mantığınızı kullanarak dönüşüm nasıl uygulanacağını tanımlayan saklı yordamın adı. <br/><br/>Bu saklı yordamı olacaktır Not **toplu iş çağrılan**. Yalnızca bir kez çalışır ve hiçbir kaynak verileri ile/delete örn truncate yapmak için kullanma işlemi yapmak istiyorsanız `preCopyScript` özelliği. |Hayır |
| storedProcedureParameters |Saklı yordamın parametreleri.<br/>İzin verilen değerler: ad/değer çiftleri. Adları ve parametreleri büyük küçük harfleri, adları ve saklı yordam parametreleri büyük küçük harfleri eşleşmelidir. |Hayır |
| sqlWriterTableType |Saklı yordam, kullanılacak bir tablo türü adı belirtin. Kopyalama etkinliği, taşınan veri bir geçici tablo bu tablo türü ile kullanılabilir hale getirir. Saklı yordam kodu daha sonra mevcut verilerle kopyalanan verileri birleştirebilirsiniz. |Hayır |

> [!TIP]
> Azure SQL veritabanı yönetilen örneği için verileri kopyalama, kopyalama etkinliği havuz tabloya verileri varsayılan olarak ekler. UPSERT ya da ek iş mantığını gerçekleştirmek için SqlSink içinde saklı yordamı kullanın. Daha ayrıntılı bilgi edinin [SQL havuz için saklı yordam çağırma](#invoking-stored-procedure-for-sql-sink).

**Örnek 1: Veri ekleme**

```json
"activities":[
    {
        "name": "CopyToSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Managed Instance output dataset name>",
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

**Örnek 2: Upsert için kopyalama sırasında bir saklı yordam çağırma**

Daha ayrıntılı bilgi edinin [SQL havuz için saklı yordam çağırma](#invoking-stored-procedure-for-sql-sink).

```json
"activities":[
    {
        "name": "CopyToSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Managed Instance output dataset name>",
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

## <a name="identity-columns-in-the-target-database"></a>Hedef veritabanındaki Kimlik sütunları

Bu bölümde, bir kimlik sütunu hedef tabloyla kimlik sütunu ile bir kaynak tablosundan veri kopyalayan bir örnek sağlar.

**Kaynak tablosu**

```sql
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```

**Hedef Tablo**

```sql
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```

Hedef tablo bir kimlik sütunu olduğuna dikkat edin.

**Kaynak veri kümesi JSON tanımı**

```json
{
    "name": "SampleSource",
    "properties": {
        "type": " SqlServerTable",
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
        "type": "SqlServerTable",
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

Kaynak ve hedef tablonuz olarak farklı bir şemaya sahip olduğuna dikkat edin (hedef ek bir sütun kimliği içeriyor). Bu senaryoda, belirtmeniz gerekir. **yapısı** kimlik sütunu içermeyen hedef veri kümesi tanımında özelliği.

## <a name="invoking-stored-procedure-for-sql-sink"></a> SQL havuz saklı yordam çağırma

Verileri Azure SQL veritabanı yönetilen örneği, bir kullanıcı tarafından belirtilen depolanan kopyalarken yordamı yapılandırılabilir ve ek parametrelerle çağrıldı.

Yerleşik kopyalama mekanizmaları amacına hizmet etmediğini bir saklı yordam kullanılabilir. Upsert (Ekle + güncelleştirme) ya da ek işleme (ek değerler, birden çok tablo, vb. içine Arama sütunları, birleştirme) kaynak verilerin hedef tablodaki son ekleme önce yapılması gerektiğinde genellikle kullanılır.

Aşağıdaki örnek, yönetilen örneği ' nde bir tabloya bir upsert yapmak için bir saklı yordam kullanmayı gösterir. Girdi verilerini ve "Pazarlama" havuz tablo üç sütun vardır: Profileıd, durum ve kategorisi. Upsert "Profileıd" sütunu temel alarak gerçekleştirin ve yalnızca belirli bir kategori için geçerlidir.

**Çıktı veri kümesi**

```json
{
    "name": "SQLServerDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<Managed Instance linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "Marketing"
        }
    }
}
```

SqlSink bölümü gibi kopyalama etkinliği tanımlayın.

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

Veritabanınızda, aynı ada sahip bir saklı yordam SqlWriterStoredProcedureName tanımlayın. Bu, çıkış tabloya belirtilen kaynak ve birleştirme gelen giriş verilerinin işler. Tablo türünde saklı yordam parametre adı veri kümesinde tanımlanan "tableName" ile aynı olması gerekir.

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

Veritabanınızda, aynı ada sahip bir tablo türü sqlWriterTableType tanımlayın. Tablo türü şemasını girişinizi tarafından döndürülen şema olarak aynı olması gerektiğini unutmayın.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL，
    [Category] [varchar](256) NOT NULL，
)
```

Saklı yordam özellik yararlanır [Table-Valued parametreleri](https://msdn.microsoft.com/library/bb675163.aspx).

>[!NOTE]
>Para/küçük para veri türüne çağrılıyor saklı yordam tarafından yazarsanız, değerleri yuvarlatılmış. Karşılık gelen veri türünü TVP azaltmak için para/küçük para yerine ondalık olarak belirtin. 

## <a name="data-type-mapping-for-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği için veri türü eşlemesi

/ İçin Azure SQL veritabanı yönetilen örneği'nın verilerini kopyalarken şu eşlemeler Yönetilen örnek veri türlerinden Azure veri fabrikası geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) eşlemelerini nasıl yapar? kopyalama etkinliği kaynak şema ve veri türü için havuz hakkında bilgi edinmek için.

| Azure SQL veritabanı yönetilen örneği veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| bigint |Int64 |
| İkili |Bayt] |
| Bit |Boole |
| Char |Dize, Char] |
| date |DateTime |
| Tarih saat |DateTime |
| datetime2 |DateTime |
| Datetimeoffset |DateTimeOffset |
| Ondalık |Ondalık |
| FILESTREAM özniteliğini (varbinary(max)) |Bayt] |
| Kayan |çift |
| image |Bayt] |
| int |Int32 |
| para |Ondalık |
| nchar |Dize, Char] |
| ntext |Dize, Char] |
| Sayısal |Ondalık |
| nvarchar |Dize, Char] |
| Gerçek |Tek |
| rowVersion |Bayt] |
| smalldatetime |DateTime |
| smallint |Int16 |
| küçük para |Ondalık |
| sql_variant |Nesne * |
| metin |Dize, Char] |
| time |Zaman aralığı |
| timestamp |Bayt] |
| tinyint |Int16 |
| benzersiz tanımlayıcı |Guid |
| varbinary |Bayt] |
| varchar |Dize, Char] |
| xml |Xml |

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
