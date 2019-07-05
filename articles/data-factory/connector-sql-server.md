---
title: Azure Data Factory kullanarak verileri ve SQL Server kopyalamak | Microsoft Docs
description: Azure Data Factory kullanarak verileri ve şirket içi SQL Server veritabanından veya Azure VM'deki taşıma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/13/2019
ms.author: jingwang
ms.openlocfilehash: a6767c7c8931898c44fd748dbe4299b8ed23eb9c
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443285"
---
# <a name="copy-data-to-and-from-sql-server-by-using-azure-data-factory"></a>Azure Data Factory kullanarak verileri ve SQL Server kopyalamak
> [!div class="op_single_selector" title1="Azure Data Factory, kullanmakta olduğunuz sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-sqlserver-connector.md)
> * [Geçerli sürüm](connector-sql-server.md)

Bu makalede, kopyalama etkinliği Azure Data Factory'de gelen ve SQL Server veritabanına veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Kaynak ve hedef SQL Server veritabanı herhangi bir desteklenen havuz veri deposuna veri kopyalayabilirsiniz. Veya bir SQL Server veritabanı herhangi bir desteklenen kaynak veri deposundan veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak ve havuz desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu SQL Server connector'ı destekler:

- SQL Server sürümleri 2016, 2014, 2012, 2008 R2, 2008 ve 2005.
- SQL veya Windows kimlik doğrulaması kullanarak veri kopyalama.
- Bir kaynak olarak bir SQL sorgusu veya saklı yordam kullanarak verileri alınıyor.
- Bir havuz olarak veriler hedef tablonun veya kopyalama sırasında özel mantığı olan bir saklı yordam çağırma ekleniyor.

>[!NOTE]
>SQL Server [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-2017) bu bağlayıcı tarafından artık desteklenmiyor. Geçici olarak çözmek için kullanabileceğiniz bir [genel ODBC Bağlayıcısı](connector-odbc.md) ve bir SQL Server ODBC sürücüsü. İzleyin [bu kılavuz](https://docs.microsoft.com/sql/connect/odbc/using-always-encrypted-with-the-odbc-driver?view=sql-server-2017) ODBC sürücüsünü yükleme ve bağlantı dizesi yapılandırmaları ile.

## <a name="prerequisites"></a>Önkoşullar

Genel olarak erişilebilir olmayan SQL Server veritabanından veri kopyalama kullanmak için bir şirket içinde barındırılan tümleştirme çalışma zamanını oluşturan gerekir. Daha fazla bilgi için [barındırılan tümleştirme çalışma zamanını](create-self-hosted-integration-runtime.md). Tümleştirme çalışma zamanı, yerleşik bir SQL Server veritabanı sürücüsünü sağlar. Herhangi bir sürücüsü ya da SQL Server veritabanına veri kopyalama, el ile yüklemeniz gerekmez.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli SQL Server veritabanı bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Aşağıdaki özellikler, SQL Server bağlı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır **SqlServer**. | Evet |
| connectionString |Belirtin **connectionString** SQL kimlik doğrulaması veya Windows kimlik doğrulaması kullanarak SQL Server veritabanına bağlanmak için gerekli olan bilgileri. Aşağıdaki örneklere bakın.<br/>Bu alanı olarak işaretleyin **SecureString** güvenli bir şekilde Azure Data Factory'de depolamak için. Bu gibi durumlarda, bir parola da Azure anahtar Kasası'nda koyabilirsiniz. SQL kimlik doğrulaması etkinleştirilmişse çeker `password` yapılandırma bağlantı dizesini dışında. Daha fazla bilgi için aşağıdaki tablonun JSON örneğe bakın ve [kimlik bilgilerini Azure Key Vault'ta Store](store-credentials-in-key-vault.md). |Evet |
| userName |Windows kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. Bir örnek **domainname\\username**. |Hayır |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için bir parola belirtin. Bu alanı olarak işaretleyin **SecureString** güvenli bir şekilde Azure Data Factory'de depolamak için. Veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Hayır |
| connectVia | Bu [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlamak için kullanılır. Data store genel olarak erişilebilir olması durumunda, şirket içinde barındırılan tümleştirme çalışma zamanı veya Azure tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Hayır |

>[!TIP]
>"Veritabanı için oturum sınırı xxx ve ulaşıldı gibi", "UserErrorFailedToConnectToSqlServer" hata koduyla bir hata ve bir ileti ulaşırsanız ekleme `Pooling=false` bağlantı dizesi ve yeniden deneyin.

**Örnek 1: SQL kimlik doğrulaması kullan**

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

**Örnek 2: Azure anahtar Kasası'nda bir parola ile SQL kimlik doğrulaması kullan**

```json
{
    "name": "SqlServerLinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;"
            },
            "password": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek 3: Windows kimlik doğrulaması kullan**

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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için veri kümeleri makalesine bakın. Bu bölümde, SQL Server veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Gelen ve SQL Server veritabanına veri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır **SqlServerTable**. | Evet |
| tableName |Bu özellik, tablo veya Görünüm başvuran bağlı hizmetin SQL Server veritabanı örneğinde adıdır. | Kaynak, havuz için Evet Hayır |

**Örnek**

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
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak üzere kullanılmak üzere mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, SQL Server kaynak ve havuz desteklenen özelliklerin bir listesini sağlar.

### <a name="sql-server-as-a-source"></a>Bir kaynak olarak SQL Server

SQL Server'dan veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **SqlSource**. Kopyalama etkinliği kaynak bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır **SqlSource**. | Evet |
| sqlReaderQuery |Verileri okumak için özel bir SQL sorgusu kullanın. `select * from MyTable` bunun bir örneğidir. |Hayır |
| sqlReaderStoredProcedureName |Bu özellik kaynak tablosundan veri okuyan saklı yordamı adıdır. Son SQL deyim bir SELECT deyimi saklı yordam içinde olmalıdır. |Hayır |
| storedProcedureParameters |Bu parametreler için saklı yordam kullanılabilir.<br/>İzin verilen değerler, ad veya değer çiftleridir. Adları ve parametreleri büyük küçük harfleri, adları ve saklı yordam parametreleri büyük küçük harfleri eşleşmelidir. |Hayır |

**Dikkat edilecek noktalar:**

- Varsa **sqlReaderQuery** için belirtilen **SqlSource**, kopyalama etkinliği, verileri almak için SQL Server Kaynak karşı bu sorgu çalıştırır. Belirterek bir saklı yordam belirtebilirsiniz **sqlReaderStoredProcedureName** ve **storedProcedureParameters** saklı yordamın kullandığı parametreler varsa.
- Ya da belirtmezseniz **sqlReaderQuery** veya **sqlReaderStoredProcedureName**, sütunları veri kümesi JSON "yapı" bölümünde tanımlanan bir sorgu oluşturmak için kullanılır. Sorgu `select column1, column2 from mytable` SQL Sunucusu'na karşı çalışır. Veri kümesi tanımı "yapı" yoksa, tüm sütunları tablodan seçilir.

**Örnek: SQL sorgusu kullanın**

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

**Örnek: Bir saklı yordam kullanma**

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

### <a name="sql-server-as-a-sink"></a>Bir havuz olarak SQL Server

> [!TIP]
> Desteklenen yazma davranışları, yapılandırmaları ve en iyi uygulamaları hakkında daha fazla bilgi [açısından en iyisi verileri SQL Server'a yüklemeye](#best-practice-for-loading-data-into-sql-server).

SQL Server veri kopyalamak için kopyalama etkinliğine de Havuz türü ayarlayın. **SqlSink**. Kopyalama etkinliği havuz bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuz öğesinin type özelliği ayarlanmalıdır **SqlSink**. | Evet |
| writeBatchSize |SQL tablosuna eklenecek satır sayısı *toplu iş başına*.<br/>Tamsayılar için satır sayısı izin verilen değerler. Varsayılan olarak, Azure Data Factory satır boyutuna göre uygun toplu iş boyutu dinamik olarak belirler. |Hayır |
| writeBatchTimeout |Bu özellik toplu ekleme işlemi zaman aşımına uğramadan önce tamamlanması için bekleme süresini belirtir.<br/>Zaman aralığı için izin verilen değerler. Bir örnek verilmiştir "00: 30:00" 30 dakikadır. |Hayır |
| preCopyScript |Bu özellik, SQL Server'daki verileri yazmadan önce çalıştırmak kopyalama etkinliği için bir SQL sorgusu belirtir. Ayrıca, kopya çalıştırma başına yalnızca bir kez çağrılır. Önceden yüklenmiş ve verileri temizlemek için bu özelliği kullanabilirsiniz. |Hayır |
| sqlWriterStoredProcedureName |Bu ad, kaynak verileri hedef tabloya uygulanacağını tanımlayan bir saklı yordam içindir.<br/>Bu saklı yordam *toplu iş çağrılan*. Yalnızca bir kez çalıştırır ve kaynak verileri, örneğin, silmek veya kesmek için hiçbir şey sahip bir işlem yapmak için `preCopyScript` özelliği. |Hayır |
| storedProcedureParameters |Bu parametreler, saklı yordam için kullanılır.<br/>İzin verilen değerler, ad veya değer çiftleridir. Adları ve parametreleri büyük küçük harfleri, adları ve saklı yordam parametreleri büyük küçük harfleri eşleşmelidir. |Hayır |
| sqlWriterTableType |Bu özellik saklı yordamda kullanılan tablo türü adı belirtir. Kopyalama etkinliği, taşınan veri bir geçici tablo bu tablo türü ile kullanılabilir hale getirir. Saklı yordam kodu daha sonra kopyalanan verileri mevcut verilerle birleştirebilirsiniz. |Hayır |

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

**Örnek 2: Kopyalama sırasında bir saklı yordam çağırma**

Daha ayrıntılı bilgi edinin [SQL havuz saklı bir yordam çağırma](#invoke-a-stored-procedure-from-a-sql-sink).

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

## <a name="best-practice-for-loading-data-into-sql-server"></a>SQL Server'a veri yüklemeye yönelik en iyi uygulama

SQL Server'daki verileri kopyaladığınızda, farklı yazma davranışını gerektirebilir:

- [Append](#append-data): Kaynak verilerimi yalnızca yeni kayıtları sahiptir.
- [Upsert](#upsert-data): Insertler ve updateler hem kaynak verilerimi sahiptir.
- [Üzerine](#overwrite-the-entire-table): Tüm boyut tablosuna her zaman yeniden yüklemek istiyorsunuz.
- [Özel mantığı ile yazma](#write-data-with-custom-logic): Son ekleme ve hedef tabloya önce ek işleme ihtiyacım var.

Azure Data Factory ve en iyi uygulamaları nasıl yapılandıracağınızı öğrenmek için ilgili bölümlere bakın.

### <a name="append-data"></a>Veri ekleme

Veri ekleme Bu SQL Server havuz bağlayıcının varsayılan davranışıdır. Azure Data Factory tablonuza verimli bir şekilde yazmak için bir toplu ekleme yapar. Kaynak yapılandırma ve buna göre havuz olarak kopyalama etkinliği.

### <a name="upsert-data"></a>Verileri upsert etme

**1. seçenek:** Büyük miktarda veri kopyalayın, bir upsert yapmak için aşağıdaki yaklaşımı kullanın olduğunda: 

- İlk olarak, bir [geçici tablo](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql?view=sql-server-2017#temporary-tables) tüm kayıtları kopyalama etkinliği'ni kullanarak toplu yükleme için. Geçici tablolara yönelik işlemler oturumunuz açık olmadığından, saniyeler içinde milyonlarca kayıt yükleyebilirsiniz.
- Bir saklı yordam etkinliği çalıştırma uygulamak için Azure Data Factory'de bir [birleştirme](https://docs.microsoft.com/sql/t-sql/statements/merge-transact-sql?view=azuresqldb-current) veya INSERT/UPDATE deyimi. Tüm gerçekleştirmek için bir kaynağı güncelleştirir veya tek bir işlem olarak ekler geçici tabloyu kullanın. Bu şekilde, gidiş dönüş ve günlük işlemleri sayısı azaltılır. Saklı yordam etkinliği sonunda geçici tablo için bir sonraki upsert döngüyü hazır olmasını kesilebilir.

Örneğin, Azure Data Factory'de bir işlem hattı oluşturabilirsiniz bir **kopyalama etkinliği** ile zincirleme bir **saklı yordam etkinliği**. Önceki veri kaynağı deponuzdan veritabanı geçici bir tabloya, örneğin, kopyalar **##UpsertTempTable**, tablo adı veri kümesinde olarak. Ardından ikinci bir geçici tablo kaynak verileri hedef tabloya birleştirebilir ve geçici tablosunu temizlemek için bir saklı yordam çağırır.

![Upsert](./media/connector-azure-sql-database/azure-sql-database-upsert.png)

Veritabanında bir saklı yordam için önceki saklı yordam etkinliği işaret edilen aşağıdaki örnekte olduğu gibi birleştirme mantığıyla tanımlayın. Hedef olduğunu varsayın **pazarlama** üç sütunlar içeren tablo: **Profileıd**, **durumu**, ve **kategori**. Temel upsert yapmak **Profileıd** sütun.

```sql
CREATE PROCEDURE [dbo].[spMergeData]
AS
BEGIN
    MERGE TargetTable AS target
    USING ##UpsertTempTable AS source
    ON (target.[ProfileID] = source.[ProfileID])
    WHEN MATCHED THEN
        UPDATE SET State = source.State
    WHEN NOT matched THEN
        INSERT ([ProfileID], [State], [Category])
      VALUES (source.ProfileID, source.State, source.Category);
    
    TRUNCATE TABLE ##UpsertTempTable
END
```

**2. seçenek:** Ayrıca da tercih edebilirsiniz [kopyalama etkinliği içinde saklı bir yordam çağırma](#invoke-a-stored-procedure-from-a-sql-sink). Bu yaklaşım, büyük ölçekli upsert için uygun değilse, kopyalama etkinliğinde, varsayılan olarak toplu ekleme kullanmak yerine kaynak tablodaki her satır çalıştırır.

### <a name="overwrite-the-entire-table"></a>Tüm tablo üzerine yaz

Yapılandırabileceğiniz **preCopyScript** bir kopyalama etkinliği havuzu özelliği. Bu durumda, çalıştırılan her kopyalama etkinliği için ilk betiği Azure Data Factory çalıştırır. Daha sonra verileri eklemek için bir kopyasını çalıştırır. Örneğin, en son verileriyle tüm tablo üzerine yazmak için bir komut dosyası toplu önce tüm kayıtları silin, kaynaktan yeni veri yükleme belirtin.

### <a name="write-data-with-custom-logic"></a>Özel mantığı ile veri yazma

Veri özel mantığı yazmak için de açıklananlara benzer adımlarla [Upsert veri](#upsert-data) bölümü. Uygulamak, ihtiyacınız olduğunda ek önce kaynak verinin son ekleme hedef tabloya, büyük ölçekli işleme bunu ikisinden birini yapabilirsiniz: 

- Geçici bir tabloya yükleyin ve ardından bir saklı yordam çağırma. 
- Kopyalama sırasında bir saklı yordam çağırma.

## <a name="invoke-a-stored-procedure-from-a-sql-sink"></a> SQL havuz saklı bir yordam çağırma

SQL Server veritabanına veri kopyalama sırasında da yapılandırabilir ve ek parametreler ile kullanıcı tanımlı saklı yordam çağırma.

> [!TIP]
> Bir saklı yordam çağırma, büyük ölçekli bir kopya için önermemekteyiz bir toplu işlem'i kullanarak, satır satır yerine verileri işler. Daha fazla bilgi [açısından en iyisi verileri SQL Server'a yüklemeye](#best-practice-for-loading-data-into-sql-server).

Yerleşik kopyalama mekanizmaları amaca hizmet yoksa, bir saklı yordamı kullanabilirsiniz. Son eklenen kaynak verileri hedef tabloya önce ek işleme uygulamak istediğinizde bir örnektir. Sütunları Birleştir, ek değerleri bakın ve birden fazla tabloya veri eklemek istediğiniz zaman ek işleme örnek verilebilir.

Aşağıdaki örnek, bir saklı yordamı SQL Server veritabanındaki bir tabloya bir upsert yapmak için nasıl kullanılacağını gösterir. Varsayımında girdi verilerini ve havuz **pazarlama** her tablo üç sütun vardır: **Profileıd**, **durumu**, ve **kategori**. Temel upsert yapmak **Profileıd** sütun ve yalnızca belirli bir kategori için uygulayın.

**Çıktı veri kümesi:** Aşağıdaki saklı yordamı kodda gösterildiği gibi "tableName" depolanmış yordamınızdaki aynı tabloda tür parametre adı şöyledir:

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

Tanımlama **SQL havuz** gibi kopyalama etkinliği bölümünde:

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

Veritabanınızda, aynı ada sahip bir saklı yordam tanımlamak **SqlWriterStoredProcedureName**. Belirtilen kaynak gelen giriş verilerinin işler ve çıkış tablosuna birleştirir. Tablo türünde saklı yordam parametre adı aynı olan **tableName** kümesinde tanımlanan.

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
      VALUES (source.ProfileID, source.State, source.Category);
END
```

Veritabanınızda, aynı ada sahip bir tablo türü tanımlayan **sqlWriterTableType**. Tablo türü şemasını girişinizi tarafından döndürülen şema ile aynıdır.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL，
    [Category] [varchar](256) NOT NULL
)
```

Saklı yordam özellik yararlanır [tablo değerli parametre](https://msdn.microsoft.com/library/bb675163.aspx).

## <a name="data-type-mapping-for-sql-server"></a>SQL Server için veri türü eşlemesi

Gelen ve SQL Server verileri kopyaladığınızda, aşağıdaki eşlemeler SQL Server veri türleri arasından Azure veri fabrikası geçici veri türleri için kullanılır. Kopyalama etkinliği havuz için kaynak şema ve veri türü eşlemelerini nasıl bilgi edinmek için [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md).

| SQL Server veri türü | Azure veri fabrikası geçici veri türü |
|:--- |:--- |
| bigint |Int64 |
| binary |Byte[] |
| bit |Boolean |
| char |String, Char[] |
| date |DateTime |
| Datetime |DateTime |
| datetime2 |DateTime |
| Datetimeoffset |DateTimeOffset |
| Decimal |Decimal |
| FILESTREAM attribute (varbinary(max)) |Byte[] |
| Float |Double |
| image |Byte[] |
| int |Int32 |
| money |Decimal |
| nchar |String, Char[] |
| ntext |String, Char[] |
| numeric |Decimal |
| nvarchar |String, Char[] |
| real |Single |
| rowversion |Byte[] |
| smalldatetime |DateTime |
| smallint |Int16 |
| smallmoney |Decimal |
| sql_variant |Object |
| text |String, Char[] |
| time |TimeSpan |
| timestamp |Byte[] |
| tinyint |Int16 |
| uniqueidentifier |Guid |
| varbinary |Byte[] |
| varchar |String, Char[] |
| xml |Xml |

>[!NOTE]
> Eşleme için ondalık geçiş türü veri türleri için Azure Data Factory şu anda 28 kadar duyarlık destekler. 28'den büyük duyarlılık gerektiren varsa, bir SQL sorgusu içindeki bir dizeye dönüştürmeyi düşünün.

## <a name="troubleshoot-connection-issues"></a>Bağlantı sorunlarını giderme

1. SQL Server örneğinizi uzak bağlantıları kabul edecek şekilde yapılandırın. Başlangıç **SQL Server Management Studio**, sağ **sunucu**seçip **özellikleri**. Seçin **bağlantıları** listesi ve select **bu sunucuya uzak bağlantılara izin vermek** onay kutusu.

    ![Uzak bağlantıları etkinleştir](media/copy-data-to-from-sql-server/AllowRemoteConnections.png)

    Ayrıntılı adımlar için bkz. [uzaktan erişim sunucu yapılandırma seçeneğini yapılandırma](https://msdn.microsoft.com/library/ms191464.aspx).

2. Başlangıç **SQL Server Yapılandırma Yöneticisi**. Genişletin **SQL Server Ağ Yapılandırması** ve seçin, örneğin **MSSQLSERVER protokolleri**. Protokolleri, sağ bölmede görünür. Sağ tıklayarak TCP/IP'yi etkinleştirin **TCP/IP'yi** seçerek **etkinleştirme**.

    ![TCP/IP'yi etkinleştirin](./media/copy-data-to-from-sql-server/EnableTCPProptocol.png)

    Daha fazla bilgi ve TCP/IP protokolünü etkinleştirme diğer yolları için bkz. [etkinleştirmek veya devre dışı sunucu ağ protokolünü](https://msdn.microsoft.com/library/ms191294.aspx).

3. Aynı pencerede çift **TCP/IP'yi** başlatmak için **TCP/IP özelliklerini** penceresi.
4. Geçiş **IP adresleri** sekmesi. Görmek için aşağı kaydırın **IPAll** bölümü. Not **TCP bağlantı noktası**. Varsayılan değer **1433**.
5. Oluşturma bir **Windows Güvenlik Duvarı Kuralı** Bu bağlantı noktası üzerinden gelen trafiğe izin vermek için makinede. 
6. **Bağlantıyı doğrulama**: Tam olarak nitelenmiş adını kullanarak SQL sunucusuna bağlanmak için farklı bir makinede SQL Server Management Studio'yu kullanın. `"<machine>.<domain>.corp.<company>.com,1433"` bunun bir örneğidir.

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
