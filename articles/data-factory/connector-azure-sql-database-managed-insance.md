---
title: Azure Data Factory kullanarak Azure SQL veritabanı yönetilen örneği gelen ve giden veri kopyalama | Microsoft Docs
description: Azure Data Factory kullanarak Azure SQL veritabanı yönetilen örneği gelen ve giden veri taşımayı öğreneceksiniz.
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
ms.openlocfilehash: 9208ceeb760bba97c12b23a1b6e5bdff7efc9020
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67274824"
---
# <a name="copy-data-to-and-from-azure-sql-database-managed-instance-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure SQL veritabanı yönetilen örneği gelen ve giden veri kopyalama

Bu makalede, kopyalama etkinliği Azure Data Factory ve Azure SQL veritabanı yönetilen örnek veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliğine genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Verileri Azure SQL veritabanı yönetilen örneği tüm desteklenen havuz veri deposuna kopyalayabilirsiniz. Ayrıca yönetilen örneğe herhangi bir desteklenen kaynak veri deposundan veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Azure SQL veritabanı yönetilen örneği bağlayıcı'yı destekler:

- SQL kimlik doğrulaması kullanarak veri kopyalama.
- Bir kaynak olarak bir SQL sorgusu veya saklı yordamı kullanarak verileri alınıyor.
- Bir havuz olarak veriler hedef tablonun veya kopyalama sırasında özel mantığı olan bir saklı yordam çağırma ekleniyor.

>[!NOTE]
>Azure SQL veritabanı yönetilen örneği **[Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=azuresqldb-mi-current)** bu bağlayıcı tarafından artık desteklenmiyor. Geçici olarak çözmek için kullanabileceğiniz [genel ODBC Bağlayıcısı](connector-odbc.md) hem de şirket içinde barındırılan tümleştirme çalışma zamanı aracılığıyla SQL Server ODBC sürücüsü. İzleyin [bu kılavuz](https://docs.microsoft.com/sql/connect/odbc/using-always-encrypted-with-the-odbc-driver?view=azuresqldb-mi-current) ODBC sürücüsünü yükleme ve bağlantı dizesi yapılandırmaları ile.

>[!NOTE]
>Bu bağlayıcı tarafından ve kısa süre sonra etkinleştirmek için plan, hizmet sorumlusu ve yönetilen kimlik doğrulamaları şu anda desteklenmez. Şimdilik geçici çözümü için Azure SQL Veritabanı Bağlayıcısı ve el ile yönetilen örneğinizin sunucu belirtmek seçebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Azure SQL veritabanı yönetilen örneğine erişmek için  **[genel uç nokta](../sql-database/sql-database-managed-instance-public-endpoint-securely.md)** , ADF tarafından yönetilen Azure IR kullanabilirsiniz. Yalnızca genel uç noktasını etkinleştirmek, ancak izleyerek veritabanınızı için bağlanan APM verisiyle ADF sağlamak için ağ güvenlik grubu genel uç nokta trafiğine de izin verin emin [bu kılavuz](../sql-database/sql-database-managed-instance-public-endpoint-configure.md).

Azure SQL veritabanı yönetilen örneğine erişmek için **özel uç nokta**ayarlayın, bir [barındırılan tümleştirme çalışma zamanını](create-self-hosted-integration-runtime.md) veritabanına erişebilir. Şirket içinde barındırılan tümleştirme çalışma zamanının yönetilen Örneğinize aynı sanal ağda sağlarsanız, tümleştirme çalışma zamanını makinenize yönetilen Örneğinize değerinden farklı bir alt ağda olduğundan emin olun. Yönetilen Örneğinize farklı bir sanal ağ, şirket içinde barındırılan tümleştirme çalışma zamanı sağlama, bir sanal ağ eşlemesi veya sanal ağ bağlantısı için sanal ağ kullanabilirsiniz. Daha fazla bilgi için [uygulamanızı Azure SQL veritabanı yönetilen örneği bağlamak](../sql-database/sql-database-managed-instance-connect-app.md).

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler belirli Data Factory varlıkları için Azure SQL veritabanı yönetilen örneği bağlayıcıyı tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Azure SQL veritabanı yönetilen bağlı örneği hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır **SqlServer**. | Evet. |
| connectionString |Bu özellik, SQL kimlik doğrulaması kullanarak yönetilen örneğe bağlanmak için gereken bağlantı dizesi bilgilerini belirtir. Daha fazla bilgi için aşağıdaki örneklere bakın. <br/>Bu alan, Data Factory'de güvenle depolamak için bir SecureString olarak işaretleyin. Parola Azure anahtar Kasası'nda koyabilirsiniz ve SQL kimlik doğrulaması çekme ise `password` yapılandırma bağlantı dizesini dışında. Aşağıdaki JSON örneği görmek ve [kimlik bilgilerini Azure Key Vault'ta Store](store-credentials-in-key-vault.md) daha fazla ayrıntı içeren makalesi. |Evet. |
| connectVia | Bu [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlamak için kullanılır. (Yönetilen Örneğinize genel uç noktaya sahip ve ADF erişmek izin verirse), şirket içinde barındırılan tümleştirme çalışma zamanı veya Azure Integration Runtime kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Evet. |

**Örnek 1: SQL kimlik doğrulamasını kullan** 1433 varsayılan bağlantı noktasıdır. SQL yönetilen örnek genel bir uç nokta ile kullanıyorsanız, bağlantı noktası 3342 açıkça belirtin.

```json
{
    "name": "AzureSqlMILinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<hostname,port>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek 2: Azure anahtar Kasası'nda parolayla SQL kimlik doğrulamasını kullan** 1433 varsayılan bağlantı noktasıdır. SQL yönetilen örnek genel bir uç nokta ile kullanıyorsanız, bağlantı noktası 3342 açıkça belirtin.

```json
{
    "name": "AzureSqlMILinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<hostname,port>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;"
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

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak üzere kullanılmak üzere mevcut özelliklerin tam listesi için veri kümeleri makalesine bakın. Bu bölümde, Azure SQL veritabanı yönetilen örneği veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Azure SQL veritabanı yönetilen örneği ve veri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır **SqlServerTable**. | Evet. |
| tableName |Bu özellik, tablo veya Görünüm bağlı hizmetini ifade eder veritabanı örneğinde adıdır. | Kaynağı yok. Havuz için Evet. |

**Örnek**

```json
{
    "name": "AzureSqlMIDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<Managed Instance linked service name>",
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

Bölümleri ve etkinlikleri tanımlamak üzere kullanılmak üzere mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Azure SQL veritabanı yönetilen örneği'nın kaynak ve havuz desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-sql-database-managed-instance-as-a-source"></a>Azure SQL veritabanı yönetilen örneği bir kaynak olarak

Verileri Azure SQL veritabanı yönetilen örneği kopyalamak için kopyalama etkinliği kaynak türünü ayarlayın. **SqlSource**. Kopyalama etkinliği kaynak bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır **SqlSource**. | Evet. |
| sqlReaderQuery |Bu özellik, verileri okumak için özel bir SQL sorgusu kullanır. `select * from MyTable` bunun bir örneğidir. |Hayır. |
| sqlReaderStoredProcedureName |Bu özellik kaynak tablosundan veri okuyan saklı yordamı adıdır. Son SQL deyim bir SELECT deyimi saklı yordam içinde olmalıdır. |Hayır. |
| storedProcedureParameters |Bu parametreler için saklı yordam kullanılabilir.<br/>İzin verilen değerler, ad veya değer çiftleridir. Adları ve parametreleri büyük küçük harfleri, adları ve saklı yordam parametreleri büyük küçük harfleri eşleşmelidir. |Hayır. |

Aşağıdaki noktalara dikkat edin:

- Varsa **sqlReaderQuery** için belirtilen **SqlSource**, kopyalama etkinliği bu sorgu veri almak için yönetilen örnek kaynak karşı çalışır. Belirterek bir saklı yordam belirtebilirsiniz **sqlReaderStoredProcedureName** ve **storedProcedureParameters** saklı yordamın kullandığı parametreler varsa.
- Ya da belirtmezseniz **sqlReaderQuery** veya **sqlReaderStoredProcedureName** özelliği, bir sorgu oluşturmak için kullanılan JSON veri kümesi "yapı" bölümünde tanımlanan sütunları. Sorgu `select column1, column2 from mytable` yönetilen örneğine karşı çalışır. Veri kümesi tanımı "yapı" yoksa, tüm sütunları tablodan seçilir.

**Örnek: Bir SQL sorgusu kullanın**

```json
"activities":[
    {
        "name": "CopyFromAzureSqlMI",
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
        "name": "CopyFromAzureSqlMI",
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

### <a name="azure-sql-database-managed-instance-as-a-sink"></a>Azure SQL veritabanı yönetilen örneği bir havuz olarak

> [!TIP]
> Desteklenen yazma davranışları, yapılandırmaları ve gelen en iyi uygulama hakkında daha fazla bilgi edinin [açısından en iyisi, Azure SQL veritabanı yönetilen örneği ile veri yükleme](#best-practice-for-loading-data-into-azure-sql-database-managed-instance).

Azure SQL veritabanı yönetilen örneği için verileri kopyalamak için kopyalama etkinliğine de Havuz türü ayarlayın. **SqlSink**. Kopyalama etkinliği havuz bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuz öğesinin type özelliği ayarlanmalıdır **SqlSink**. | Evet. |
| writeBatchSize |SQL tablosuna ekler için satır sayısı **toplu iş başına**.<br/>Tamsayılar için satır sayısı izin verilen değerler. Varsayılan olarak, Data Factory dinamik olarak satır boyutuna göre uygun toplu iş boyutu belirler.  |Hayır |
| writeBatchTimeout |Bu özellik toplu ekleme işlemi zaman aşımına uğramadan önce tamamlanması için bekleme süresini belirtir.<br/>Zaman aralığı için izin verilen değerler. Bir örnek verilmiştir "00: 30:00," 30 dakika olduğu. |Hayır. |
| preCopyScript |Bu özellik, yönetilen örneğe veri yazılmadan önce yürütmek kopyalama etkinliği için bir SQL sorgusu belirtir. Ayrıca, kopya çalıştırma başına yalnızca bir kez çağrılır. Önceden verileri temizlemek için bu özelliği kullanabilirsiniz. |Hayır. |
| sqlWriterStoredProcedureName |Bu ad, kaynak verileri hedef tabloya uygulanacağını tanımlayan bir saklı yordam içindir. <br/>Bu saklı yordam *toplu iş çağrılan*. Yalnızca bir kez çalıştırır ve kaynak verileri, örneğin, silmek veya kesmek için hiçbir şey sahip bir işlem yapmak için `preCopyScript` özelliği. |Hayır. |
| storedProcedureParameters |Bu parametreler, saklı yordam için kullanılır.<br/>İzin verilen değerler, ad veya değer çiftleridir. Adları ve parametreleri büyük küçük harfleri, adları ve saklı yordam parametreleri büyük küçük harfleri eşleşmelidir. |Hayır. |
| sqlWriterTableType |Bu özellik saklı yordamda kullanılan tablo türü adı belirtir. Kopyalama etkinliği, taşınan veri bir geçici tablo bu tablo türü ile kullanılabilir hale getirir. Saklı yordam kodu daha sonra kopyalanan verileri mevcut verilerle birleştirebilirsiniz. |Hayır. |

**Örnek 1: veri ekleme**

```json
"activities":[
    {
        "name": "CopyToAzureSqlMI",
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

**Örnek 2: Kopyalama sırasında bir saklı yordam çağırma**

Daha ayrıntılı bilgi edinin [SQL havuz saklı bir yordam çağırma](#invoking-stored-procedure-for-sql-sink).

```json
"activities":[
    {
        "name": "CopyToAzureSqlMI",
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

## <a name="best-practice-for-loading-data-into-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği ile veri yükleme için en iyi uygulama

Verileri Azure SQL veritabanı yönetilen örneğine kopyaladığınızda, farklı yazma davranışını gerektirebilir:

- **[Append](#append-data)** : kaynak Verilerimin yalnızca yeni kayıtları; var
- **[Upsert](#upsert-data)** : kaynak Verilerimin hem eklemeleri ve güncelleştirmeleri; var
- **[Üzerine](#overwrite-entire-table)** : Tüm boyut tablosunu her zaman yeniden yüklemek istiyorsanız;
- **[Özel mantığı ile yazma](#write-data-with-custom-logic)** : Son ekleme ve hedef tabloya önce ek işleme ihtiyacım var.

Başvurmak sırasıyla bölümler ADF ve en iyi yapılandırma.

### <a name="append-data"></a>Veri ekleme

Bu bu Azure SQL veritabanı yönetilen örneği havuz bağlayıcının varsayılan davranıştır ve ADF yapın **toplu ekleme** tablonuza verimli bir şekilde yazmak için. Yalnızca kaynak yapılandırabilir ve buna göre havuz olarak kopyalama etkinliği.

### <a name="upsert-data"></a>Verileri upsert etme

**Ben seçeneği** (özellikle büyük veri kopyalamak için olduğunda önerilen): **en yüksek performanslı yaklaşım** upsert yapmak için aşağıda verilmiştir: 

- İlk olarak, yararlanan bir [geçici tablo](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql?view=sql-server-2017#temporary-tables) kopyalama etkinliğini kullanarak tüm kayıtları toplu yükleme için. Geçici tablolara yönelik işlemleri günlüğe kaydedilmez gibi saniyeler içinde milyonlarca kayıt yükleyebilirsiniz.
- Yürütme uygulamak için ADF içinde bir saklı yordam etkinliği bir [birleştirme](https://docs.microsoft.com/sql/t-sql/statements/merge-transact-sql?view=azuresqldb-current) (veya ekleme/güncelleştirme) ifadesi ve geçici tablo tüm gerçekleştirmek için kaynak güncelleştirmeleri veya tek bir işlem ekler gidiş-dönüş miktarını azaltmayı ve günlük işlemlerini kullanın. Saklı yordam etkinliğine sonunda geçici tablo için bir sonraki upsert döngüyü hazır olmasını kesilebilir. 

Örneğin, Azure Data Factory'de bir işlem hattı oluşturabilirsiniz bir **kopyalama etkinliği** ile zincirleme bir **saklı yordam etkinliği** başarılı. Önceki veri kaynağı Mağazası'ndan geçici bir tabloya deyin kopyalar " **##UpsertTempTable**" tablo adı veri kümesinde, ardından ikinci bir depolanan geçici tablo kaynak verileri hedef tabloya birleştirme ve temizleme yordamı çağıran geçici tablo.

![Upsert](./media/connector-azure-sql-database/azure-sql-database-upsert.png)

Veritabanında bir saklı yordam için yukarıdaki saklı yordam etkinliği işaret edilen aşağıdaki gibi birleştirme mantığıyla tanımlayın. Hedef varsayılarak **pazarlama** üç sütunlar içeren tablo: **Profileıd**, **durumu**, ve **kategori**, temel upsert yapıp **Profileıd** sütun.

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

**Seçenek II:** alternatif olarak, seçebileceğiniz [kopyalama etkinliği içinde saklı yordamı çağırma](#invoking-stored-procedure-for-sql-sink), bu yaklaşım, toplu yararlanarak yerine kaynak tablodaki her satır için yürütüldüğünde Not sırasında varsayılan yaklaşım Ekle Kopyalama etkinliği, bu nedenle, büyük ölçekli upsert için sığmıyor.

### <a name="overwrite-entire-table"></a>Tüm tablo üzerine yaz

Yapılandırabileceğiniz **preCopyScript** özelliği kopyalama etkinliğindeki havuz, bu durumda her kopyalama etkinliği için çalıştırma ADF betik ilk olarak, daha sonra verileri eklemek için kopyalama işleminin çalıştırılma yürütür. Örneğin, en son verileriyle tüm tablo üzerine yazmak için önce toplu yeni veri kaynağından yükleme önce tüm kayıtları silmek için bir betik belirtebilirsiniz.

### <a name="write-data-with-custom-logic"></a>Özel mantığı ile veri yazma

Bölümünde anlatıldığı gibi benzer [Upsert veri](#upsert-data) bölümü, son eklenen kaynak verileri hedef tabloya önce ek işleme uygulamak, ihtiyacınız olduğunda şunları yapabilirsiniz bir) büyük ölçekli, geçici bir tabloya yüklemek sonra bir saklı çağırma yordam veya b) kopyalama sırasında bir saklı yordam çağırma.

## <a name="invoking-stored-procedure-for-sql-sink"></a> SQL havuz saklı bir yordam çağırma

Verileri Azure SQL veritabanı yönetilen örneğine kopyaladığınızda, yapılandırın ve bir kullanıcı tarafından belirtilen saklı yordam ek parametrelerle çağırın.

> [!TIP]
> Saklı yordam çağırma verileri-satır büyük ölçekli kopyalama için önerilen değil, toplu işlem yerine işler. Daha fazla bilgi [açısından en iyisi, Azure SQL veritabanı yönetilen örneği ile veri yükleme](#best-practice-for-loading-data-into-azure-sql-database-managed-instance).

Yerleşik kopyalama mekanizmaları, amaca yönelik olmayan bir saklı yordam kullanabilirsiniz örneğin son eklenen kaynak verileri hedef tabloya önce ek işlem uygulayın. Bazı ek işleme örnekleri ek değerler ve birden fazla tablosuna eklenirken Ara birleştirme sütunlarıdır.

Aşağıdaki örnek, bir saklı yordamı SQL Server veritabanındaki bir tabloya bir upsert yapmak için nasıl kullanılacağını gösterir. Varsayılır, giriş veri ve havuz **pazarlama** her tablo üç sütun vardır: **Profileıd**, **durumu**, ve **kategori**. Temel upsert yapmak **Profileıd** sütun ve yalnızca belirli bir kategori için uygulayın.

**Çıktı veri kümesi:** "tableName" depolanmış yordamınızdaki (saklı yordam betiği aşağıya bakın) aynı tabloda tür parametre adı olmalıdır.

```json
{
    "name": "AzureSqlMIDataset",
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

Tanımlama **SQL havuz** gibi kopyalama etkinliği bölümü.

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

Veritabanınızda, aynı ada sahip bir saklı yordam tanımlamak **SqlWriterStoredProcedureName**. Belirtilen kaynak gelen giriş verilerinin işler ve çıkış tablosuna birleştirir. Tablo türünde saklı yordam parametre adı aynı olmalıdır **tableName** kümesinde tanımlanan.

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

Veritabanınızda, aynı ada sahip bir tablo türü sqlWriterTableType tanımlayın. Tablo türü şemasını girişinizi tarafından döndürülen şema ile aynıdır.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL，
    [Category] [varchar](256) NOT NULL
)
```

Saklı yordam özellik yararlanır [tablo değerli parametre](https://msdn.microsoft.com/library/bb675163.aspx).

## <a name="data-type-mapping-for-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği için veri türü eşlemesi

Azure SQL veritabanı yönetilen örneği gelen ve giden veriler kopyalandığında, aşağıdaki eşlemeler Azure SQL veritabanı yönetilen örneği veri türlerinden Azure veri fabrikası geçici veri türleri için kullanılır. Kopyalama etkinliği havuz için kaynak şema ve veri türü eşlemelerini nasıl bilgi edinmek için [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md).

| Azure SQL veritabanı yönetilen örneği veri türü | Azure veri fabrikası geçici veri türü |
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

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
