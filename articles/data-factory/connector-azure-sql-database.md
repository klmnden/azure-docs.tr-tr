---
title: Data Factory kullanarak Azure SQL veritabanı'ndan ya da veri kopyalama | Microsoft Docs
description: Data Factory kullanarak Azure SQL veritabanı için desteklenen kaynak veri depolarını veya desteklenen bir havuz veri depolarına SQL veritabanına veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: 231f44612b5e87afdf84f31d86c80be644fb4484
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65154335"
---
# <a name="copy-data-to-or-from-azure-sql-database-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure SQL veritabanı'ndan ya da veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you use:"]
> * [Sürüm 1](v1/data-factory-azure-sql-connector.md)
> * [Geçerli sürüm](connector-azure-sql-database.md)

Bu makalede, Azure SQL veritabanı ve veri kopyalamak nasıl özetlenmektedir. Azure Data Factory hakkında bilgi edinmek için [giriş makalesi](introduction.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bu Azure SQL Veritabanı Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Kopyalama etkinliği](copy-activity-overview.md) ile [desteklenen kaynak/havuz matris](copy-activity-overview.md) tablo
- [Veri akışı eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata activity](control-flow-get-metadata-activity.md)

Özellikle, bu Azure SQL Veritabanı Bağlayıcısı, bu işlevler destekler:

- SQL kimlik doğrulaması ve Azure Active Directory (Azure AD) uygulama belirteci kimlik doğrulamasını Azure kaynakları için bir hizmet sorumlusu veya yönetilen kimliklerle kullanarak verileri kopyalayın.
- Bir kaynak olarak bir SQL sorgusu veya saklı yordamı kullanarak veri alın.
- Bir havuz olarak verileri hedef tabloya veya kopyalama sırasında özel mantığı olan bir saklı yordam çağırma.

Azure SQL veritabanı [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-2017) artık desteklenmiyor. 

> [!IMPORTANT]
> Azure Data Factory Integration Runtime'ı kullanarak verileri kopyalama, yapılandırma bir [Azure SQL sunucusu güvenlik duvarı](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) Azure Hizmetleri sunucusuna erişebilmesi için.
> Şirket içinde barındırılan Integration runtime'ı kullanarak verileri kopyalama, Azure SQL sunucusu güvenlik duvarı uygun IP aralığı izin verecek şekilde yapılandırın. Azure SQL veritabanı'na bağlanmak için kullanılan bir makinenin IP bu aralığa dahildir.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli bir Azure SQL Veritabanı Bağlayıcısı tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Bu özellikler bir Azure SQL veritabanı bağlı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** özelliği ayarlanmalıdır **AzureSqlDatabase**. | Evet |
| connectionString | İçin Azure SQL veritabanı örneğine bağlanmak için gereken bilgileri belirtin **connectionString** özelliği. <br/>Bu alan, Data Factory'de güvenle depolamak için bir SecureString olarak işaretleyin. Parola/hizmet sorumlusu anahtarı Azure anahtar Kasası'nda koyabilirsiniz ve SQL kimlik doğrulaması çekme ise `password` yapılandırma bağlantı dizesini dışında. Aşağıdaki JSON örneği görmek ve [kimlik bilgilerini Azure Key Vault'ta Store](store-credentials-in-key-vault.md) daha fazla ayrıntı içeren makalesi. | Evet |
| servicePrincipalId | Uygulamanın istemci kimliği belirtin. | Evet, bir hizmet sorumlusu ile Azure AD kimlik doğrulaması kullandığınızda. |
| serviceprincipalkey değerleri | Uygulama anahtarını belirtin. Bu alan olarak işaretlemek bir **SecureString** Data Factory'de güvenle depolamak için veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet, bir hizmet sorumlusu ile Azure AD kimlik doğrulaması kullandığınızda. |
| tenant | Kiracı bilgileri (etki alanı adı veya Kiracı kimliği), uygulamanızın bulunduğu altında belirtin. Bu, Azure portalının sağ üst köşedeki fare gelerek alın. | Evet, bir hizmet sorumlusu ile Azure AD kimlik doğrulaması kullandığınızda. |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Data store özel bir ağda yer alıyorsa, Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. | Hayır |

Farklı kimlik doğrulama türleri için sırasıyla önkoşulları ve JSON örnekleri aşağıdaki bölümlere bakın:

- [SQL kimlik doğrulaması](#sql-authentication)
- [Azure AD uygulama belirteci kimlik doğrulaması: Hizmet sorumlusu](#service-principal-authentication)
- [Azure AD uygulama belirteci kimlik doğrulaması: Azure kaynakları için yönetilen kimlikleri](#managed-identity)

>[!TIP]
>Hata olarak "UserErrorFailedToConnectToSqlServer" hata koduyla isabet ve gibi ileti "veritabanı için oturum sınırı xxx ve üst sınırına ulaşıldı.", ekleme `Pooling=false` bağlantı dizesi ve yeniden deneyin.

### <a name="sql-authentication"></a>SQL kimlik doğrulaması

#### <a name="linked-service-example-that-uses-sql-authentication"></a>SQL kimlik doğrulamasını kullanan bağlı hizmet örneği

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

**Azure Key vault'ta parolası:** 

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
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

### <a name="service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması

Bir hizmet sorumlusu tabanlı Azure AD uygulama belirteci kimlik doğrulamasını kullanmak için aşağıdaki adımları izleyin:

1. **[Bir Azure Active Directory uygulaması oluşturma](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application)**  Azure portalından. Uygulama adı ve bağlı hizmetini tanımlamak aşağıdaki değerleri not edin:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. **[Bir Azure Active Directory Yöneticisi sağlama](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)**  zaten yapmadıysanız Azure Portal'da Azure SQL sunucunuzun. Azure AD Yöneticisi, Azure AD kullanıcısı veya Azure AD grubu olması gerekir, ancak bir hizmet sorumlusu olamaz. Bu adım, sonraki adımda, bir Azure AD kimlik bağımsız veritabanı kullanıcısı için hizmet sorumlusu oluşturmak için kullanabilirsiniz, böylece gerçekleştirilir.

3. **[Bağımsız veritabanı kullanıcılarını oluşturun](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)**  hizmet sorumlusu için. Veritabanından ya da en az bir Azure AD kimlik ile SSMS gibi araçları kullanarak verileri kopyalamak istediğiniz herhangi bir kullanıcı ALTER izni. Aşağıdaki T-SQL çalıştırın: 
  
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

4. **Hizmet sorumlusuna gerekli izinleri vermek** SQL kullanıcıları veya diğerleri için normalde yaptığınız gibi. Aşağıdaki kodu çalıştırın ya da daha fazla seçenek için başvuru [burada](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017).

    ```sql
    EXEC sp_addrolemember [role name], [your application name];
    ```

5. **Bir Azure SQL veritabanı bağlı hizmeti yapılandırma** Azure Data factory'de.


#### <a name="linked-service-example-that-uses-service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulamasını kullanan bağlı hizmet örneği

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;Connection Timeout=30"
            },
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identity"></a> Azure kaynaklarında kimlik doğrulaması için yönetilen kimlik

Veri Fabrikası ile ilişkilendirilmiş bir [yönetilen Azure kaynakları için kimliği](data-factory-service-identity.md) , belirli veri üretecini temsil eder. Bu yönetilen kimlik Azure SQL veritabanı kimlik doğrulaması için kullanabilirsiniz. Belirtilen Üreteç erişebilir ve veri kopyalama ya da veritabanı sunucunuza bu kimliği kullanarak.

Yönetilen kimlik doğrulaması kullanmak için aşağıdaki adımları izleyin:

1. **[Bir Azure Active Directory Yöneticisi sağlama](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)**  zaten yapmadıysanız Azure Portal'da Azure SQL sunucunuzun. Azure AD Yöneticisi, Azure AD kullanıcısı veya Azure AD grubu olabilir. Yönetilen kimlik Yönetici rolü grubuyla sağlıyorsa, 3 ve 4. adımları atlayın. Yönetici veritabanında tam erişiminiz olacaktır.

2. **[Bağımsız veritabanı kullanıcılarını oluşturun](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)**  Data Factory yönetilen kimliği için. Veritabanından ya da en az bir Azure AD kimlik ile SSMS gibi araçları kullanarak verileri kopyalamak istediğiniz herhangi bir kullanıcı ALTER izni. Aşağıdaki T-SQL çalıştırın: 
  
    ```sql
    CREATE USER [your Data Factory name] FROM EXTERNAL PROVIDER;
    ```

3. **Data Factory yönetilen kimliği gerekli izinleri vermek** SQL kullanıcılar ve diğerleri için normalde yaptığınız gibi. Aşağıdaki kodu çalıştırın ya da daha fazla seçenek için başvuru [burada](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017).

    ```sql
    EXEC sp_addrolemember [role name], [your Data Factory name];
    ```

4. **Bir Azure SQL veritabanı bağlı hizmeti yapılandırma** Azure Data factory'de.

**Örnek:**

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;Connection Timeout=30"
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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](https://docs.microsoft.com/azure/data-factory/concepts-datasets-linked-services) makalesi. Bu bölümde, Azure SQL veritabanı veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Ya da Azure SQL veritabanına veri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kümesinin özelliği ayarlanmalıdır **AzureSqlTable**. | Evet |
| tableName | Tablo veya Görünüm başvuran bağlı hizmetin Azure SQL veritabanı örneğinde adı. | Kaynak, havuz için Evet Hayır |

#### <a name="dataset-properties-example"></a>Veri kümesi özellikleri örneği

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
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Azure SQL veritabanı kaynak ve havuz desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-sql-database-as-the-source"></a>Kaynağı olarak Azure SQL veritabanı

Azure SQL veritabanı'ndan veri kopyalamak için ayarlanmış **türü** kopyalama etkinliği kaynak özelliğinde **SqlSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kopyalama etkinliği kaynak özelliği ayarlanmalıdır **SqlSource**. | Evet |
| sqlReaderQuery | Verileri okumak için özel bir SQL sorgusu kullanın. Örnek: `select * from MyTable`. | Hayır |
| sqlReaderStoredProcedureName | Kaynak tablo verilerini okuyan saklı yordamın adı. Son SQL deyim bir SELECT deyimi saklı yordam içinde olmalıdır. | Hayır |
| storedProcedureParameters | Saklı yordamın parametreleri.<br/>İzin verilen değerler, ad veya değer çiftleridir. Adları ve parametreleri büyük küçük harfleri, adları ve saklı yordam parametreleri büyük küçük harfleri eşleşmelidir. | Hayır |

### <a name="points-to-note"></a>Dikkat edilecek noktaları

- Varsa **sqlReaderQuery** için belirtilen **SqlSource**, kopyalama etkinliği, verileri almak için Azure SQL veritabanı kaynak karşı bu sorgu çalıştırır. Veya bir saklı yordam belirtebilirsiniz. Belirtin **sqlReaderStoredProcedureName** ve **storedProcedureParameters** saklı yordamın kullandığı parametreler varsa.
- Ya da belirtmezseniz **sqlReaderQuery** veya **sqlReaderStoredProcedureName**, tanımlanan sütunları **yapısı** JSON veri kümesi bölümünü alışkın olduğunuz bir sorgu oluşturun. `select column1, column2 from mytable` Azure SQL veritabanına karşı çalışır. Veri kümesi tanımı yoksa **yapısı**, tablodan tüm sütun seçilmedi.

#### <a name="sql-query-example"></a>SQL sorgusu örneği

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

#### <a name="stored-procedure-example"></a>Saklı yordam örneği

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

### <a name="stored-procedure-definition"></a>Saklı yordam tanımında

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

### <a name="azure-sql-database-as-the-sink"></a>Azure SQL veritabanı havuz olarak

Azure SQL veritabanı'na veri kopyalamak için ayarlanmış **türü** özelliği kopyalama etkinliğindeki havuz için **SqlSink**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kopyalama etkinliği havuz özelliği ayarlanmalıdır **SqlSink**. | Evet |
| writeBatchSize | SQL tablosuna ekler için satır sayısı **toplu iş başına**.<br/> İzin verilen değer **tamsayı** (satır sayısı). Varsayılan olarak, Data Factory dinamik olarak satır boyutuna göre uygun toplu iş boyutu belirler. | Hayır |
| writeBatchTimeout | Batch için bekleme süresi, işlemin zaman aşımına uğramadan önce tamamlanmasını ekleyin.<br/> İzin verilen değer **timespan**. Örnek: "00: 30:00" (30 dakika). | Hayır |
| preCopyScript | Azure SQL veritabanı'na veri yazılmadan önce çalıştırmak kopyalama etkinliği için bir SQL sorgusunu belirtin. Bu yalnızca bir kez çalıştır kopyalama çağrılır. Önceden yüklenmiş ve verileri temizlemek için bu özelliği kullanın. | Hayır |
| sqlWriterStoredProcedureName | Kaynak verileri hedef tabloya uygulanacağını tanımlayan saklı yordamın adı. Upsert eder misiniz veya tarafından dönüştürmek için bir örnek verilmiştir, kendi iş mantığınızı kullanarak. <br/><br/>Bu saklı yordam **toplu iş çağrılan**. Yalnızca bir kez çalıştırın ve kaynak verilerle ilgisi yoktur işlemleri için `preCopyScript` özelliği. Örnek işlemler silmeden ve kesin. | Hayır |
| storedProcedureParameters |Saklı yordamın parametreleri.<br/>Ad ve değer çiftlerini izin verilen değerler. Adları ve parametreleri büyük küçük harfleri, adları ve saklı yordam parametreleri büyük küçük harfleri eşleşmelidir. | Hayır |
| sqlWriterTableType | Saklı yordam, kullanılacak bir tablo türü adı belirtin. Kopyalama etkinliği, Taşınmakta olan veriler geçici tablo bu tablo türü ile kullanılabilir hale getirir. Saklı yordam kodu daha sonra mevcut verilerle kopyalanan verileri birleştirebilirsiniz. | Hayır |

> [!TIP]
> Azure SQL veritabanı'na veri kopyalama, kopyalama etkinliği havuz tablo için verileri varsayılan olarak ekler. Upsert ya da ek iş mantığı için saklı yordamı kullanın. **SqlSink**. Daha ayrıntılı bilgi edinin [SQL havuz saklı yordam çağırma](#invoking-stored-procedure-for-sql-sink).

#### <a name="append-data-example"></a>Örnek veri ekleme

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

#### <a name="invoke-a-stored-procedure-during-copy-for-upsert-example"></a>Upsert örneğin kopyalama sırasında bir saklı yordam çağırma

Daha ayrıntılı bilgi edinin [SQL havuz saklı yordam çağırma](#invoking-stored-procedure-for-sql-sink).

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

## <a name="identity-columns-in-the-target-database"></a>Hedef veritabanındaki Kimlik sütunları

Bu bölümde bir IDENTITY sütunu hedef tabloda bir kimlik sütunu olmayan bir kaynak tablosundan veri kopyalamak nasıl gösterir.

#### <a name="source-table"></a>Kaynak tablosu

```sql
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```

#### <a name="destination-table"></a>Hedef Tablo

```sql
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```

> [!NOTE]
> Hedef tablo bir kimlik sütunu var.

#### <a name="source-dataset-json-definition"></a>Kaynak veri kümesi JSON tanımı

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

#### <a name="destination-dataset-json-definition"></a>Hedef veri kümesi JSON tanımı

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

> [!NOTE]
> Kaynak ve hedef tablonuz farklı bir şeması vardır. 

Hedef bir kimliğe sahip başka bir sütuna sahiptir. Bu senaryoda, belirtmelisiniz **yapısı** kimlik sütunu içermeyen hedef veri kümesi tanımında özelliği.

## <a name="invoking-stored-procedure-for-sql-sink"></a> SQL havuz saklı yordam çağırma

Azure SQL veritabanı'na veri kopyalama, yapılandırın ve bir kullanıcı tarafından belirtilen saklı yordam ek parametrelerle çağırın.

Yerleşik kopyalama mekanizmaları amaca hizmet yoksa, bir saklı yordamı kullanabilirsiniz. Genellikle, upsert, INSERT ve update kullanıldıklarından veya ek işlem son eklenen kaynak verileri hedef tabloya önce yapılmalıdır. Bazı ek işleme örnekleri ek değerler ve birden fazla tablosuna eklenirken Ara birleştirme sütunlarıdır.

Aşağıdaki örnek, Azure SQL veritabanındaki bir tabloya bir upsert yapmak için bir saklı yordam kullanmayı gösterir. Varsayılır, giriş veri ve havuz **pazarlama** her tablo üç sütun vardır: **Profileıd**, **durumu**, ve **kategori**. Temel upsert yapmak **Profileıd** sütun ve yalnızca belirli bir kategori için uygulayın.

**Çıktı veri kümesi:** "tableName" depolanmış yordamınızdaki (saklı yordam betiği aşağıya bakın) aynı tabloda tür parametre adı olmalıdır.

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

Veritabanınızda, aynı ada sahip bir tablo türü tanımlayan **sqlWriterTableType**. Tablo türü şemasını girişinizi tarafından döndürülen şema olarak aynı olmalıdır.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL,
    [Category] [varchar](256) NOT NULL
)
```

Saklı yordam özellik yararlanır [Table-Valued parametreleri](https://msdn.microsoft.com/library/bb675163.aspx).

## <a name="mapping-data-flow-properties"></a>Veri akışı özellikleri eşleme

Ayrıntıları öğrenin [kaynak dönüştürme](data-flow-source.md) ve [havuz dönüştürme](data-flow-sink.md) eşleme veri akışı olarak.

## <a name="data-type-mapping-for-azure-sql-database"></a>Azure SQL veritabanı için veri türü eşlemesi

Şu eşlemeler ya da Azure SQL veritabanına veri kopyalama, Azure SQL veritabanı veri türleri arasından Azure veri fabrikası geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) kopyalama etkinliği havuz için kaynak şema ve veri türü eşlemelerini nasıl öğrenin.

| Azure SQL veritabanı veri türü | Veri Fabrikası geçici veri türü |
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
| FILESTREAM özniteliğini (varbinary(max)) |Byte[] |
| Float |Double |
| image |Byte[] |
| int |Int32 |
| money |Decimal |
| nchar |String, Char[] |
| ntext |String, Char[] |
| Numeric |Decimal |
| nvarchar |String, Char[] |
| real |Single |
| rowVersion |Byte[] |
| smalldatetime |DateTime |
| smallint |Int16 |
| küçük para |Decimal |
| sql_variant |Object |
| metin |String, Char[] |
| time |TimeSpan |
| timestamp |Byte[] |
| tinyint |Byte |
| uniqueidentifier |Guid |
| Varbinary |Byte[] |
| varchar |String, Char[] |
| xml |Xml |

>[!NOTE]
> Ondalık geçiş türü için veri türü eşlemeleri için ADF desteklemekte duyarlık 28 kadar. 28'den büyük olan hassas verileriniz varsa, SQL sorgusunda dizeye dönüştürmek için göz önünde bulundurun.

## <a name="next-steps"></a>Sonraki adımlar
Azure veri fabrikasında kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları ve biçimler](copy-activity-overview.md##supported-data-stores-and-formats).
