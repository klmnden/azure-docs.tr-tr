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
ms.date: 06/14/2019
ms.author: jingwang
ms.openlocfilehash: 5dbd739070b1f66fe5ff04f319a3818269d0bdaa
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67449602"
---
# <a name="copy-data-to-or-from-azure-sql-database-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure SQL veritabanı'ndan ya da veri kopyalama
> [!div class="op_single_selector" title1="Azure Data Factory, kullanmakta olduğunuz sürümünü seçin:"]
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

- SQL kimlik doğrulaması ve Azure Active Directory (Azure AD) uygulama belirteci kimlik doğrulamasını Azure kaynakları için bir hizmet sorumlusu veya yönetilen kimliklerle kullanarak veri kopyalama.
- Bir kaynak olarak bir SQL sorgusu veya saklı yordam kullanarak verileri alınıyor.
- Bir havuz olarak veriler hedef tablonun veya kopyalama sırasında özel mantığı olan bir saklı yordam çağırma ekleniyor.

>[!NOTE]
>Azure SQL veritabanı [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=azuresqldb-current) bu bağlayıcı tarafından artık desteklenmiyor. Geçici olarak çözmek için kullanabileceğiniz bir [genel ODBC Bağlayıcısı](connector-odbc.md) ve bir şirket içinde barındırılan tümleştirme çalışma zamanı aracılığıyla SQL Server ODBC sürücüsü. İzleyin [bu kılavuz](https://docs.microsoft.com/sql/connect/odbc/using-always-encrypted-with-the-odbc-driver?view=azuresqldb-current) ODBC sürücüsünü yükleme ve bağlantı dizesi yapılandırmaları ile.

> [!IMPORTANT]
> Azure Data Factory Integration runtime'ı kullanarak verileri kopyalama, yapılandırma bir [Azure SQL sunucusu güvenlik duvarı](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) Azure hizmetlerinin sunucunun erişebilmesi için.
> Şirket içinde barındırılan Integration runtime'ı kullanarak verileri kopyalama, Azure SQL sunucusu güvenlik duvarı uygun IP aralığı izin verecek şekilde yapılandırın. Azure SQL veritabanı'na bağlanmak için kullanılan bir makinenin IP bu aralığa dahildir.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Azure Data Factory varlıklarını belirli bir Azure SQL Veritabanı Bağlayıcısı tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Bu özellikler bir Azure SQL veritabanı bağlı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** özelliği ayarlanmalıdır **AzureSqlDatabase**. | Evet |
| connectionString | İçin Azure SQL veritabanı örneğine bağlanmak için gereken bilgileri belirtin **connectionString** özelliği. <br/>Bu alanı olarak işaretleyin **SecureString** güvenli bir şekilde Azure Data Factory'de depolamak için. Ayrıca, bir parola veya hizmet sorumlusu anahtarı Azure anahtar Kasası'nda koyabilirsiniz. SQL kimlik doğrulaması etkinleştirilmişse çeker `password` yapılandırma bağlantı dizesini dışında. Daha fazla bilgi için aşağıdaki tablonun JSON örneğe bakın ve [kimlik bilgilerini Azure Key Vault'ta Store](store-credentials-in-key-vault.md). | Evet |
| servicePrincipalId | Uygulamanın istemci kimliği belirtin. | Evet, bir hizmet sorumlusu ile Azure AD kimlik doğrulaması kullandığınızda |
| serviceprincipalkey değerleri | Uygulama anahtarını belirtin. Bu alanı olarak işaretleyin **SecureString** güvenli bir şekilde Azure Data Factory'de depolamak için veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet, bir hizmet sorumlusu ile Azure AD kimlik doğrulaması kullandığınızda |
| tenant | Kiracı bilgileri gibi etki alanı adı belirtin veya Kiracı kimliği, uygulamanızın bulunduğu. Bu, Azure portalının sağ üst köşedeki fare gelerek alın. | Evet, bir hizmet sorumlusu ile Azure AD kimlik doğrulaması kullandığınızda |
| connectVia | Bu [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlamak için kullanılır. Data store özel bir ağda yer alıyorsa, Azure tümleştirme çalışma zamanı veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. | Hayır |

Farklı kimlik doğrulama türleri için sırasıyla önkoşulları ve JSON örnekleri aşağıdaki bölümlere bakın:

- [SQL kimlik doğrulaması](#sql-authentication)
- [Azure AD uygulama belirteci kimlik doğrulaması: Hizmet sorumlusu](#service-principal-authentication)
- [Azure AD uygulama belirteci kimlik doğrulaması: Azure kaynakları için yönetilen kimlikleri](#managed-identity)

>[!TIP]
>"Veritabanı için oturum sınırı xxx ve ulaşıldı gibi", "UserErrorFailedToConnectToSqlServer" hata koduyla bir hata ve bir ileti ulaşırsanız ekleme `Pooling=false` bağlantı dizesi ve yeniden deneyin.

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

**Azure Key vault'ta parola** 

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

1. [Bir Azure Active Directory uygulaması oluşturma](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application) Azure portalından. Uygulama adı ve bağlı hizmetini tanımlamak aşağıdaki değerleri not edin:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. [Bir Azure Active Directory Yöneticisi sağlama](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server) zaten yapmadıysanız Azure Portal'da Azure SQL sunucunuzun. Azure AD Yöneticisi, Azure AD kullanıcısı veya Azure AD grubu olması gerekir, ancak bir hizmet sorumlusu olamaz. Bu adım, sonraki adımda, bir Azure AD kimlik bağımsız veritabanı kullanıcısı için hizmet sorumlusu oluşturmak için kullanabilirsiniz, böylece gerçekleştirilir.

3. [Kapsanan veritabanı kullanıcıları oluşturma](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities) hizmet sorumlusu için. Veritabanından ya da en az bir Azure AD kimlik ile SQL Server Management Studio gibi araçları kullanarak verileri kopyalamak istediğiniz herhangi bir kullanıcı ALTER izni. Aşağıdaki T-SQL çalıştırın: 
  
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

4. SQL kullanıcıları veya diğerleri için normalde yaptığınız gibi hizmet sorumlusu gereken izinleri verin. Aşağıdaki kodu çalıştırın. Daha fazla seçenek için bkz: [bu belgeyi](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017).

    ```sql
    EXEC sp_addrolemember [role name], [your application name];
    ```

5. Azure Data Factory'de bir Azure SQL veritabanı bağlı hizmeti yapılandırın.


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

Yönetilen kimlik doğrulaması kullanmak için aşağıdaki adımları izleyin.

1. [Bir Azure Active Directory Yöneticisi sağlama](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server) zaten yapmadıysanız Azure Portal'da Azure SQL sunucunuzun. Azure AD Yöneticisi, bir Azure AD kullanıcısı veya Azure AD grubu olabilir. Yönetilen kimlik Yönetici rolü grubuyla sağlıyorsa, 3 ve 4. adımları atlayın. Yönetici veritabanına tam erişime sahip.

2. [Kapsanan veritabanı kullanıcıları oluşturma](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities) için Azure Data Factory yönetilen kimliği. Veritabanından ya da en az bir Azure AD kimlik ile SQL Server Management Studio gibi araçları kullanarak verileri kopyalamak istediğiniz herhangi bir kullanıcı ALTER izni. Aşağıdaki T-SQL çalıştırın: 
  
    ```sql
    CREATE USER [your Data Factory name] FROM EXTERNAL PROVIDER;
    ```

3. Data Factory yönetilen kimliği aynı izinleri SQL kullanıcılar ve diğerleri için her zamanki verin. Aşağıdaki kodu çalıştırın. Daha fazla seçenek için bkz: [bu belgeyi](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017).

    ```sql
    EXEC sp_addrolemember [role name], [your Data Factory name];
    ```

4. Azure Data Factory'de bir Azure SQL veritabanı bağlı hizmeti yapılandırın.

**Örnek**

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

Bölümleri ve veri kümeleri tanımlamak mevcut özelliklerin tam listesi için bkz: [veri kümeleri](https://docs.microsoft.com/azure/data-factory/concepts-datasets-linked-services). Bu bölümde, Azure SQL veritabanı veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

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

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md). Bu bölümde, Azure SQL veritabanı kaynak ve havuz desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-sql-database-as-the-source"></a>Kaynağı olarak Azure SQL veritabanı

Azure SQL veritabanı'ndan veri kopyalamak için ayarlanmış **türü** için kopyalama etkinliği kaynağı özelliğinde **SqlSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| türü | **Türü** kopyalama etkinliği kaynağı özelliği ayarlanmalıdır **SqlSource**. | Evet |
| sqlReaderQuery | Bu özellik, verileri okumak için özel bir SQL sorgusu kullanır. `select * from MyTable` bunun bir örneğidir. | Hayır |
| sqlReaderStoredProcedureName | Kaynak tablo verilerini okuyan saklı yordamın adı. Son SQL deyim bir SELECT deyimi saklı yordam içinde olmalıdır. | Hayır |
| storedProcedureParameters | Saklı yordamın parametreleri.<br/>İzin verilen değerler, ad veya değer çiftleridir. Adları ve parametreleri büyük küçük harfleri, adları ve saklı yordam parametreleri büyük küçük harfleri eşleşmelidir. | Hayır |

**Dikkat edilecek noktalar:**

- Varsa **sqlReaderQuery** için belirtilen **SqlSource**, kopyalama etkinliği, verileri almak için Azure SQL veritabanı kaynak karşı bu sorgu çalıştırır. Belirterek bir saklı yordam belirtebilirsiniz **sqlReaderStoredProcedureName** ve **storedProcedureParameters** saklı yordamın kullandığı parametreler varsa.
- Ya da belirtmezseniz **sqlReaderQuery** veya **sqlReaderStoredProcedureName**, sütunları veri kümesi JSON "yapı" bölümünde tanımlanan bir sorgu oluşturmak için kullanılır. Sorgu `select column1, column2 from mytable` Azure SQL veritabanına karşı çalışır. Veri kümesi tanımı "yapı" yoksa, tüm sütunları tablodan seçilir.

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

> [!TIP]
> Desteklenen yazma davranışları, yapılandırmaları ve en iyi uygulamaları hakkında daha fazla bilgi [açısından en iyisi, Azure SQL veritabanı'na veri yükleme](#best-practice-for-loading-data-into-azure-sql-database).

Azure SQL veritabanı'na veri kopyalamak için ayarlanmış **türü** özelliği kopyalama etkinliğindeki havuz için **SqlSink**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| türü | **Türü** kopyalama etkinliği havuz özelliği ayarlanmalıdır **SqlSink**. | Evet |
| writeBatchSize | SQL tablosuna eklenecek satır sayısı *toplu iş başına*.<br/> İzin verilen değer **tamsayı** (satır sayısı). Varsayılan olarak, Azure Data Factory satır boyutuna göre uygun toplu iş boyutu dinamik olarak belirler. | Hayır |
| writeBatchTimeout | Batch için bekleme süresi, işlemin zaman aşımına uğramadan önce tamamlanmasını ekleyin.<br/> İzin verilen değer **timespan**. Bir örnek verilmiştir "00: 30:00" (30 dakika). | Hayır |
| preCopyScript | Azure SQL veritabanı'na veri yazılmadan önce çalıştırmak kopyalama etkinliği için bir SQL sorgusunu belirtin. Ayrıca, kopya çalıştırma başına yalnızca bir kez çağrılır. Önceden yüklenmiş ve verileri temizlemek için bu özelliği kullanın. | Hayır |
| sqlWriterStoredProcedureName | Kaynak verileri hedef tabloya uygulanacağını tanımlayan saklı yordamın adı. <br/>Bu saklı yordam *toplu iş çağrılan*. Yalnızca bir kez çalıştırın ve kaynak verileri, örneğin, silmek veya kesmek için hiçbir şey işlemleri için `preCopyScript` özelliği. | Hayır |
| storedProcedureParameters |Saklı yordamın parametreleri.<br/>Ad ve değer çiftlerini izin verilen değerler. Adları ve parametreleri büyük küçük harfleri, adları ve saklı yordam parametreleri büyük küçük harfleri eşleşmelidir. | Hayır |
| sqlWriterTableType | Saklı yordam, kullanılacak bir tablo türü adı belirtin. Kopyalama etkinliği, Taşınmakta olan veriler geçici tablo bu tablo türü ile kullanılabilir hale getirir. Saklı yordam kodu daha sonra kopyalanan verileri mevcut verilerle birleştirebilirsiniz. | Hayır |

**Örnek 1: Veri ekleme**

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

**Örnek 2: Kopyalama sırasında bir saklı yordam çağırma**

Daha ayrıntılı bilgi edinin [SQL havuz saklı bir yordam çağırma](#invoke-a-stored-procedure-from-a-sql-sink).

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

## <a name="best-practice-for-loading-data-into-azure-sql-database"></a>Azure SQL veritabanı'na veri yüklemeye yönelik en iyi uygulama

Azure SQL veritabanı'na veri kopyalama, farklı yazma davranışını gerektirebilir:

- [Append](#append-data): Kaynak verilerimi yalnızca yeni kayıtları sahiptir.
- [Upsert](#upsert-data): Insertler ve updateler hem kaynak verilerimi sahiptir.
- [Üzerine](#overwrite-the-entire-table): Tüm boyut tablosu her zaman yeniden yüklemek istiyorsunuz.
- [Özel mantığı ile yazma](#write-data-with-custom-logic): Son ekleme ve hedef tabloya önce ek işleme ihtiyacım var.

Azure Data Factory ve en iyi yapılandırma konusunda ilgili bölümlere bakın.

### <a name="append-data"></a>Veri ekleme

Veri ekleme Bu Azure SQL veritabanı havuz bağlayıcının varsayılan davranıştır. Azure Data Factory tablonuza verimli bir şekilde yazmak için bir toplu ekleme yapar. Kaynak yapılandırma ve buna göre havuz olarak kopyalama etkinliği.

### <a name="upsert-data"></a>Verileri upsert etme

**1. seçenek:** Büyük miktarda veri kopyalayın, bir upsert yapmak için aşağıdaki yaklaşımı kullanın olduğunda: 

- İlk olarak, bir [veritabanı kapsamlı geçici tablo](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql?view=azuresqldb-current#database-scoped-global-temporary-tables-azure-sql-database) tüm kayıtları kopyalama etkinliği'ni kullanarak toplu yükleme için. Operations veritabanı kapsamlı geçici tablolar karşı oturumunuz açık olmadığından, saniyeler içinde milyonlarca kayıt yükleyebilirsiniz.
- Bir saklı yordam etkinliği çalıştırma uygulamak için Azure Data Factory'de bir [birleştirme](https://docs.microsoft.com/sql/t-sql/statements/merge-transact-sql?view=azuresqldb-current) veya INSERT/UPDATE deyimi. Tüm gerçekleştirmek için kaynak güncelleştirmeleri veya tek bir işlem olarak ekler geçici tabloyu kullanın. Bu şekilde, gidiş dönüş ve günlük işlemleri sayısı azaltılır. Saklı yordam etkinliği sonunda geçici tablo için bir sonraki upsert döngüyü hazır olmasını kesilebilir.

Örneğin, Azure Data Factory'de bir işlem hattı oluşturabilirsiniz bir **kopyalama etkinliği** ile zincirleme bir **saklı yordam etkinliği**. Eski veri kaynak Mağazası'ndan Azure SQL veritabanı geçici tabloya, örneğin, kopyalar **##UpsertTempTable**, tablo adı veri kümesinde olarak. Ardından ikinci bir geçici tablo kaynak verileri hedef tabloya birleştirebilir ve geçici tablosunu temizlemek için bir saklı yordam çağırır.

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

Yapılandırabileceğiniz **preCopyScript** kopyalama etkinliği havuz özelliği. Bu durumda, çalıştırılan her kopyalama etkinliği için ilk betiği Azure Data Factory çalıştırır. Daha sonra verileri eklemek için bir kopyasını çalıştırır. Örneğin, en son verileriyle tüm tablo üzerine yazmak için bir komut dosyası toplu önce tüm kayıtları silin, kaynaktan yeni veri yükleme belirtin.

### <a name="write-data-with-custom-logic"></a>Özel mantığı ile veri yazma

Veri özel mantığı yazmak için de açıklananlara benzer adımlarla [Upsert veri](#upsert-data) bölümü. Uygulamak, ihtiyacınız olduğunda ek önce kaynak verinin son ekleme hedef tabloya, büyük ölçekli işleme bunu ikisinden birini yapabilirsiniz:

- Veritabanı kapsamlı geçici tabloya yükleyin ve ardından bir saklı yordam çağırma. 
- Kopyalama sırasında bir saklı yordam çağırma.

## <a name="invoke-a-stored-procedure-from-a-sql-sink"></a> SQL havuz saklı bir yordam çağırma

Azure SQL veritabanı'na veri kopyalama sırasında da yapılandırabilir ve ek parametreler ile kullanıcı tanımlı saklı yordam çağırma.

> [!TIP]
> Bir saklı yordam çağırma, büyük ölçekli bir kopya için önermemekteyiz bir toplu işlem'i kullanarak, satır satır yerine verileri işler. Daha fazla bilgi [açısından en iyisi, Azure SQL veritabanı'na veri yükleme](#best-practice-for-loading-data-into-azure-sql-database).

Yerleşik kopyalama mekanizmaları amaca hizmet yoksa, bir saklı yordamı kullanabilirsiniz. Son eklenen kaynak verileri hedef tabloya önce ek işleme uygulamak istediğinizde bir örnektir. Sütunları Birleştir, ek değerleri bakın ve birden fazla tabloya yerleştirmek istediğinizde, ek işleme örnek verilebilir.

Aşağıdaki örnek, Azure SQL veritabanındaki bir tabloya bir upsert yapmak için bir saklı yordam kullanmayı gösterir. Varsayımında girdi verilerini ve havuz **pazarlama** her tablo üç sütun vardır: **Profileıd**, **durumu**, ve **kategori**. Temel upsert yapmak **Profileıd** sütun ve yalnızca belirli bir kategori için uygulayın.

**Çıktı veri kümesi:** Aşağıdaki saklı yordamı kodda gösterildiği gibi "tableName" depolanmış yordamınızdaki aynı tabloda tür parametre adı şöyledir:

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
    [State] [varchar](256) NOT NULL,
    [Category] [varchar](256) NOT NULL
)
```

Saklı yordam özellik yararlanır [tablo değerli parametre](https://msdn.microsoft.com/library/bb675163.aspx).

## <a name="mapping-data-flow-properties"></a>Eşleme veri akışı özellikleri

Ayrıntıları öğrenin [kaynak dönüştürme](data-flow-source.md) ve [havuz dönüştürme](data-flow-sink.md) veri akışı eşlemesi içinde.

## <a name="data-type-mapping-for-azure-sql-database"></a>Azure SQL veritabanı için veri türü eşlemesi

Verileri Azure SQL veritabanı ya da kopyalandığında, aşağıdaki eşlemeler Azure SQL veritabanı veri türleri arasından Azure veri fabrikası geçici veri türleri için kullanılır. Kopyalama etkinliği havuz için kaynak şema ve veri türü eşlemelerini nasıl bilgi edinmek için [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md).

| Azure SQL veritabanı veri türü | Azure veri fabrikası geçici veri türü |
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
| tinyint |Byte |
| uniqueidentifier |Guid |
| varbinary |Byte[] |
| varchar |String, Char[] |
| xml |Xml |

>[!NOTE]
> Eşleme için ondalık geçiş türü veri türleri için Azure Data Factory şu anda 28 kadar duyarlık destekler. Duyarlık 28'den büyük verilerle varsa, SQL sorgusu içindeki bir dizeye dönüştürmeyi düşünün.

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları ve biçimler](copy-activity-overview.md##supported-data-stores-and-formats).
