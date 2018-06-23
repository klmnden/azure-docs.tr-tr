---
title: Veri kopyalama/Azure Data Factory kullanarak SQL Server | Microsoft Docs
description: Azure Data Factory kullanarak Azure VM'deki veya şirket içi SQL Server veritabanı için/gelen verileri taşıma hakkında bilgi edinin.
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
ms.date: 06/22/2018
ms.author: jingwang
ms.openlocfilehash: cca35d75f6d5560a621d377ae544eeba41434962
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36316388"
---
# <a name="copy-data-to-and-from-sql-server-using-azure-data-factory"></a>Azure Data Factory kullanarak SQL Server gelen ve giden veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-sqlserver-connector.md)
> * [Sürüm 2 - Önizleme](connector-sql-server.md)

Bu makalede kopya etkinliği Azure Data Factory'de gelen ve bir SQL Server veritabanına veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 SQL Server connector](v1/data-factory-sqlserver-connector.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna / SQL Server veritabanına veri kopyalamak ya da tüm desteklenen kaynak veri deposundan SQL Server veritabanına veri kopyalamak. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu SQL Server bağlayıcısını destekler:

- SQL Server 2016, 2014, 2012, 2008 R2, 2008 ve 2005 sürüm
- Verileri kullanarak kopyalama **SQL** veya **Windows** kimlik doğrulaması.
- SQL sorgusu veya saklı yordam kullanarak veri kaynağı olarak alınıyor.
- Havuz veri hedef tablo ya da kopyalama sırasında özel mantık olan bir saklı yordam çağırma sonuna ekleme.

## <a name="prerequisites"></a>Önkoşullar

Genel olarak erişilebilir değil bir SQL Server veritabanından veri Kopyala kullanmak için Self-hosted tümleştirmesi çalışma zamanı ayarlamak gerekir. Bkz: [Self-hosted tümleştirmesi çalışma zamanı](create-self-hosted-integration-runtime.md) Ayrıntılar için makale. Yerleşik bir SQL Server veritabanı sürücü tümleştirmesi çalışma zamanı sağlar, bu nedenle herhangi bir sürücüsü / SQL Server veritabanına veri kopyalama işlemi sırasında el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, SQL Server veritabanı bağlayıcıya Data Factory varlıklarını belirli tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler, SQL Server bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **SqlServer** | Evet |
| connectionString |SQL kimlik doğrulaması veya Windows kimlik doğrulamasını kullanarak SQL Server veritabanına bağlanmak için gereken connectionString bilgilerini belirtin. Aşağıdaki örneğe bakın ve eklediğiniz daha fazla özellik örneğin AlwaysOn içerecek şekilde zenginleştirmek. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). |Evet |
| Kullanıcı adı |Windows kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. Örnek: **domainname\\kullanıcıadı**. |Hayır |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). |Hayır |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu genel olarak erişilebilir ise) Self-hosted tümleştirmesi çalışma zamanı veya Azure tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. |Hayır |

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

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için veri kümeleri makalesine bakın. Bu bölümde, SQL Server veri kümesi tarafından desteklenen özellikler listesini sağlar.

/ SQL Server veritabanına veri kopyalamak için veri kümesi için tür özelliği ayarlamak **SqlServerTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak: **SqlServerTable** | Evet |
| tableName |Tablo veya Görünüm hizmeti bağlı SQL Server veritabanı örneğinde başvurduğu adı. | Evet |

**Örnek:**

```json
{
    "name": "SQLServerDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<SQL Server linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde, SQL Server kaynak ve havuz tarafından desteklenen özellikler listesini sağlar.

### <a name="sql-server-as-source"></a>SQL Server kaynak olarak

SQL Server'dan veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **SqlSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **SqlSource** | Evet |
| sqlReaderQuery |Verileri okumak için özel SQL sorgusu kullanın. Örnek: `select * from MyTable`. |Hayır |
| sqlReaderStoredProcedureName |Kaynak tablodan veri okuyan saklı yordamın adı. Son SQL deyimi SELECT deyimi içinde saklı yordamı olması gerekir. |Hayır |
| storedProcedureParameters |Saklı yordam parametreleri.<br/>İzin verilen değerler: ad/değer çiftleri. Adları ve büyük/küçük harf parametrelerinin adlarını ve saklı yordam parametreleri büyük/küçük harf eşleşmelidir. |Hayır |

**Dikkat edilecek noktalar:**

- Varsa **sqlReaderQuery** belirtilen SqlSource için kopyalama etkinliği veri almak için SQL Server Kaynak karşı bu sorguyu çalıştırır. Alternatif olarak, bir saklı yordam belirterek belirleyebileceğiniz **sqlReaderStoredProcedureName** ve **storedProcedureParameters** (saklı yordam parametreleri alıyorsa).
- "SqlReaderQuery" veya "sqlReaderStoredProcedureName" belirtmezseniz, JSON veri kümesi'nin "yapısı" bölümünde tanımlanan sütunları bir sorgu oluşturmak için kullanılır (`select column1, column2 from mytable`) SQL Server karşı çalıştırmak için. Veri kümesi tanımı "yapısı" yoksa, tüm sütunları tablodan seçilir.
- Kullandığınızda **sqlReaderStoredProcedureName**, yine de bir kukla belirtmeniz gerekiyorsa **tableName** JSON veri kümesi bir özellik.

**Örnek: SQL sorgusu kullanma**

```json
"activities":[
    {
        "name": "CopyFromSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SQL Server input dataset name>",
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
        "name": "CopyFromSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SQL Server input dataset name>",
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

### <a name="sql-server-as-sink"></a>Havuz olarak SQL Server

SQL Server veri kopyalamak için kopyalama etkinliği Havuz türü ayarlayın. **SqlSink**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopya etkinliği havuz tür özelliği ayarlamak: **SqlSink** | Evet |
| writeBatchSize |Arabellek boyutu writeBatchSize ulaştığında veri SQL tablosuna ekler.<br/>İzin verilen değerler: tamsayı (satır sayısı). |Hayır (varsayılan: 10000) |
| writeBatchTimeout |Toplu ekleme işlemi zaman aşımına uğramadan önce tamamlamak bir süre bekleyin.<br/>İzin verilen değerler: timespan. Örnek: "00: 30:00" (30 dakika). |Hayır |
| preCopyScript |SQL Server'a veri yazmadan önce yürütmek kopyalama etkinliği için bir SQL sorgusunu belirtin. Bu yalnızca bir kez çalıştır kopya başına çağrılır. Önceden yüklenmiş veriyi temizlemek için bu özelliği kullanın. |Hayır |
| sqlWriterStoredProcedureName |Kaynak verileri hedef tabloya örneğin upserts veya kendi iş mantığı kullanarak dönüşüm uygulamak nasıl tanımlar saklı yordamın adı. <br/><br/>Bu saklı yordam olacaktır Not **yığın başına çağrılan**. Yalnızca bir kez çalışır ve kaynak verilerle örneğin silme/kesmek yapmak için kullanmak için hiçbir şey olan işlemi yapmak istiyorsanız `preCopyScript` özelliği. |Hayır |
| storedProcedureParameters |Saklı yordam parametreleri.<br/>İzin verilen değerler: ad/değer çiftleri. Adları ve büyük/küçük harf parametrelerinin adlarını ve saklı yordam parametreleri büyük/küçük harf eşleşmelidir. |Hayır |
| sqlWriterTableType |Saklı yordam, kullanılacak bir tablo türü adı belirtin. Kopyalama etkinliği taşınan veri geçici bir tablo bu tablo türü ile kullanılabilir hale getirir. Saklı yordam kodu ardından var olan verilerle kopyalanan verileri birleştirebilirsiniz. |Hayır |

> [!TIP]
> SQL Server veri kopyalama, kopyalama etkinliği varsayılan olarak havuz tabloya veri ekler. UPSERT veya ek iş mantığı gerçekleştirmek için SqlSink içinde saklı yordamı kullanın. Daha fazla ayrıntıyı öğrenin [SQL havuz için saklı yordam çağırma](#invoking-stored-procedure-for-sql-sink).

**Örnek 1: veri ekleme**

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
                "referenceName": "<SQL Server output dataset name>",
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
                "referenceName": "<SQL Server output dataset name>",
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

Bu bölümde kimlik sütunu içeren bir kaynak tablo verileri bir kimlik sütunu hedef tabloyla kopyalayan bir örnek sağlar.

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

Kaynak ve hedef tablosu farklı şema sahip dikkat edin (hedef kimliğine sahip başka bir sütuna sahip). Bu senaryoda, belirtmeniz gerekir. **yapısı** kimlik sütunu içermeyen hedef veri kümesi tanımında özelliği.

## <a name="invoking-stored-procedure-for-sql-sink"></a> SQL havuz depolanan yordamı çağırma

SQL Server veritabanına veri kopyalama işlemi sırasında bir kullanıcı saklı yordam yapılandırılabilir ve ek parametrelerle çağrılan belirtti.

Yerleşik kopyalama mekanizmaları amacı sunmaz bir saklı yordam kullanılabilir. Upsert (INSERT + güncelleştirme) veya (sütunları, ek değerleri, birden çok tablo, vb. ekleme bakarak birleştirme) ek işleme kaynak verilerin hedef tabloda son ekleme önce yapılması gerektiğinde genellikle kullanılır.

Aşağıdaki örnek, SQL Server veritabanındaki bir tabloya bir upsert yapmak için bir saklı yordam kullanmayı gösterir. Giriş verilerinin ve "Pazarlama" havuz tablo varsayılarak üç sütun vardır: Profileıd, durumu ve kategori. Upsert "Profileıd" sütununa dayalı gerçekleştirin ve yalnızca belirli bir kategorideki için geçerlidir.

**Çıktı veri kümesi**

```json
{
    "name": "SQLServerDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<SQL Server linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "Marketing"
        }
    }
}
```

SqlSink bölüm kopyalama etkinliği aşağıdaki gibi tanımlayın.

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

Veritabanınızda, aynı ada sahip saklı yordam SqlWriterStoredProcedureName tanımlayın. Bu, çıkış tablosuna belirtilen kaynak ve birleştirme giriş verilerini işler. Saklı yordam parametresinin adı "kümesinde tanımlanan tableName" ile aynı olmalıdır dikkat edin.

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

## <a name="data-type-mapping-for-sql-server"></a>Eşleme için SQL server veri türü

SQL Server başlangıç/bitiş veri kopyalama işlemi sırasında aşağıdaki eşlemelerini SQL Server veri türlerinden Azure Data Factory geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) nasıl kopyalama etkinliği kaynak şema ve veri türü için havuz eşlemeleri hakkında bilgi edinmek için.

| SQL Server veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| bigint |Int64 |
| İkili |Byte] |
| bit |Boole |
| char |Dize, Char] |
| tarih |DateTime |
| Tarih saat |DateTime |
| datetime2 |DateTime |
| Datetimeoffset |DateTimeOffset |
| Ondalık |Ondalık |
| FILESTREAM özniteliği (varbinary(max)) |Byte] |
| Kayan |çift |
| image |Byte] |
| int |Int32 |
| para |Ondalık |
| nchar |Dize, Char] |
| ntext |Dize, Char] |
| sayısal |Ondalık |
| nvarchar |Dize, Char] |
| Gerçek |Tek |
| rowVersion |Byte] |
| smalldatetime |DateTime |
| tamsayı |Int16 |
| küçük para |Ondalık |
| sql_variant |Nesne * |
| metin |Dize, Char] |
| time |TimeSpan |
| timestamp |Byte] |
| Mini tamsayı |Int16 |
| benzersiz tanımlayıcı |Guid |
| varbinary |Byte] |
| varchar |Dize, Char] |
| xml |Xml |

## <a name="troubleshooting-connection-issues"></a>Bağlantı sorunlarını giderme

1. Uzak bağlantıları kabul etmek için SQL Server'ınızı yapılandırın. Başlatma **SQL Server Management Studio**, sağ **server**, tıklatıp **özellikleri**. Seçin **bağlantıları** listesi ve onay **sunucusuna uzaktan bağlantılara izin**.

    ![Uzak bağlantıları etkinleştir](media/copy-data-to-from-sql-server/AllowRemoteConnections.png)

    Bkz: [uzaktan erişim sunucu yapılandırma seçeneğini yapılandırma](https://msdn.microsoft.com/library/ms191464.aspx) ayrıntılı adımlar için.

2. Başlatma **SQL Server Configuration Manager**. Genişletme **SQL Server Ağ Yapılandırması** ve seçin, örneğin **MSSQLSERVER protokolleri**. Sağ bölmede protokolleri görmeniz gerekir. Sağ tıklayarak TCP/IP'yi etkinleştirin **TCP/IP'yi** tıklatıp **etkinleştirmek**.

    ![TCP/IP'yi etkinleştirin](./media/copy-data-to-from-sql-server/EnableTCPProptocol.png)

    Bkz: [etkinleştirmek veya devre dışı bir sunucu ağ protokolü](https://msdn.microsoft.com/library/ms191294.aspx) ayrıntı ve TCP/IP protokolünün etkinleştirilmesi alternatif yolu.

3. Aynı pencerede çift **TCP/IP'yi** başlatmak için **TCP/IP özelliklerini** penceresi.
4. Geçiş **IP adreslerini** sekmesi. Kaydırma görmek için **IPAll** bölümü. Aşağı Not ** TCP bağlantı noktası ** (varsayılan değer **1433**).
5. Oluşturma bir **Windows Güvenlik Duvarı Kuralı** Bu bağlantı noktası üzerinden gelen trafiğe izin vermek için makinede.  
6. **Bağlantıyı doğrulama**: tam olarak nitelenmiş adını kullanarak SQL Server'a bağlanmak için farklı bir makineden SQL Server Management Studio kullanın. Örneğin: `"<machine>.<domain>.corp.<company>.com,1433"`.


## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
