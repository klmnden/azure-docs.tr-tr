---
title: Azure Data Factory kullanarak Azure SQL veri ambarı ve veri kopyalamak | Microsoft Docs
description: Data Factory kullanarak Azure SQL veri ambarı için desteklenen kaynak depolarını veya SQL veri ambarı'na desteklenen havuz depoları veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: jingwang
ms.openlocfilehash: e3fc5a3dc5dc40078ca3a4733f6a2ba11da450f1
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59681225"
---
# <a name="copy-data-to-or-from-azure-sql-data-warehouse-by-using-azure-data-factory"></a>Azure Data Factory kullanarak veya Azure SQL veri ambarı veri kopyalayın 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you're using:"]
> * [Version1](v1/data-factory-azure-sql-data-warehouse-connector.md)
> * [Geçerli sürüm](connector-azure-sql-data-warehouse.md)

Bu makalede, kopyalama etkinliği Azure Data Factory'de veya Azure SQL veri ambarı veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Yapılar [kopyalama etkinliğine genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Azure SQL veri ambarı ' veri her desteklenen havuz veri deposuna kopyalayabilirsiniz. Ve tüm desteklenen kaynak veri deposundan Azure SQL veri ambarı'na veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak ve havuz desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları ve biçimler](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Azure SQL veri ambarı Bağlayıcısı, bu işlevler destekler:

- SQL kimlik doğrulaması ve Azure Active Directory (Azure AD) uygulama belirteci kimlik doğrulamasını Azure kaynakları için bir hizmet sorumlusu veya yönetilen kimliklerle kullanarak verileri kopyalayın.
- Bir kaynak olarak bir SQL sorgusu veya saklı yordamı kullanarak veri alın.
- Bir havuz olarak, PolyBase ya da bir toplu kullanarak yük veriler ekleyin. Daha iyi bir kopyalama performansı için Polybase'i öneririz.

> [!IMPORTANT]
> PolyBase yalnızca SQL kimlik doğrulaması ancak Azure AD'ye kimlik doğrulaması desteklediğini unutmayın.

> [!IMPORTANT]
> Azure Data Factory Integration Runtime'ı kullanarak verileri kopyalama, yapılandırma bir [Azure SQL sunucusu güvenlik duvarı](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) Azure hizmetlerinin sunucunun erişebilmesi için.
> Şirket içinde barındırılan Integration runtime'ı kullanarak verileri kopyalama, Azure SQL sunucusu güvenlik duvarı uygun IP aralığı izin verecek şekilde yapılandırın. Azure SQL veritabanı'na bağlanmak için kullanılan bir makinenin IP bu aralığa dahildir.

## <a name="get-started"></a>başlarken

> [!TIP]
> En iyi performansı elde etmek için Azure SQL veri ambarı'na veri yüklemek için PolyBase kullanın. [Azure SQL veri ambarı'na veri yüklemek için PolyBase kullanma](#use-polybase-to-load-data-into-azure-sql-data-warehouse) Ayrıntılar bölümünde bulunur. Kullanım örneği ile bir kılavuz için bkz. [1 TB 15 dakikadan daha kısa Azure Data Factory ile Azure SQL Data Warehouse'a veri yükleme](load-azure-sql-data-warehouse.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli bir Azure SQL veri ambarı Bağlayıcısı tanımlayan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Bir Azure SQL veri ambarı bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır **AzureSqlDW**. | Evet |
| connectionString | İçin Azure SQL veri ambarı örneğine bağlanmak için gereken bilgileri belirtin **connectionString** özelliği. <br/>Bu alan, Data Factory'de güvenle depolamak için bir SecureString olarak işaretleyin. Parola/hizmet sorumlusu anahtarı Azure anahtar Kasası'nda koyabilirsiniz ve SQL kimlik doğrulaması çekme ise `password` yapılandırma bağlantı dizesini dışında. Aşağıdaki JSON örneği görmek ve [kimlik bilgilerini Azure Key Vault'ta Store](store-credentials-in-key-vault.md) daha fazla ayrıntı içeren makalesi. | Evet |
| servicePrincipalId | Uygulamanın istemci kimliği belirtin. | Evet, bir hizmet sorumlusu ile Azure AD kimlik doğrulaması kullandığınızda. |
| serviceprincipalkey değerleri | Uygulama anahtarını belirtin. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet, bir hizmet sorumlusu ile Azure AD kimlik doğrulaması kullandığınızda. |
| tenant | Kiracı bilgileri (etki alanı adı veya Kiracı kimliği), uygulamanızın bulunduğu altında belirtin. Azure portalının sağ üst köşedeki fare getirerek geri alabilirsiniz. | Evet, bir hizmet sorumlusu ile Azure AD kimlik doğrulaması kullandığınızda. |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel bir ağda yer alıyorsa) Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. | Hayır |

Farklı kimlik doğrulama türleri için sırasıyla önkoşulları ve JSON örnekleri aşağıdaki bölümlere bakın:

- [SQL kimlik doğrulaması](#sql-authentication)
- Azure AD uygulama belirteci kimlik doğrulaması: [Hizmet sorumlusu](#service-principal-authentication)
- Azure AD uygulama belirteci kimlik doğrulaması: [Azure kaynakları için yönetilen kimlikler](#managed-identity)

>[!TIP]
>Hata olarak "UserErrorFailedToConnectToSqlServer" hata koduyla isabet ve gibi ileti "veritabanı için oturum sınırı xxx ve üst sınırına ulaşıldı.", ekleme `Pooling=false` bağlantı dizesi ve yeniden deneyin.

### <a name="sql-authentication"></a>SQL kimlik doğrulaması

#### <a name="linked-service-example-that-uses-sql-authentication"></a>SQL kimlik doğrulamasını kullanan bağlı hizmet örneği

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
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
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
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

Hizmet sorumlusu tabanlı Azure AD uygulama belirteci kimlik doğrulamasını kullanmak için aşağıdaki adımları izleyin:

1. **[Bir Azure Active Directory uygulaması oluşturma](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application)**  Azure portalından. Uygulama adı ve bağlı hizmetini tanımlamak aşağıdaki değerleri not edin:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. **[Bir Azure Active Directory Yöneticisi sağlama](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)**  zaten yapmadıysanız Azure Portal'da Azure SQL sunucunuzun. Azure AD Yöneticisi, Azure AD kullanıcısı veya Azure AD grubu olabilir. Yönetilen kimlik Yönetici rolü grubuyla sağlıyorsa, 3 ve 4. adımları atlayın. Yönetici veritabanında tam erişiminiz olacaktır.

3. **[Bağımsız veritabanı kullanıcılarını oluşturun](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)**  hizmet sorumlusu için. Veri ambarından ya da en az bir Azure AD kimlik ile SSMS gibi araçları kullanarak verileri kopyalamak istediğiniz herhangi bir kullanıcı ALTER izni. Aşağıdaki T-SQL çalıştırın:
    
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

4. **Hizmet sorumlusuna gerekli izinleri vermek** SQL kullanıcıları veya diğerleri için normalde yaptığınız gibi. Aşağıdaki kodu çalıştırın ya da daha fazla seçenek için başvuru [burada](https://docs.microsoft.com/en-us/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017).

    ```sql
    EXEC sp_addrolemember [role name], [your application name];
    ```

5. **Bir Azure SQL veri ambarı bağlı hizmetini yapılandırma** Azure Data factory'de.


#### <a name="linked-service-example-that-uses-service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulamasını kullanan bağlı hizmet örneği

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
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

Veri Fabrikası ile ilişkilendirilmiş bir [yönetilen Azure kaynakları için kimliği](data-factory-service-identity.md) , belirli üretecini temsil eder. Bu yönetilen kimlik Azure SQL veri ambarı kimlik doğrulaması için kullanabilirsiniz. Belirtilen Üreteç erişebilir ve bu kimlik kullanarak veri kopyalama ya da verilerinize ambarı.

Yönetilen kimlik doğrulaması kullanmak için aşağıdaki adımları izleyin:

1. **[Bir Azure Active Directory Yöneticisi sağlama](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)**  zaten yapmadıysanız Azure Portal'da Azure SQL sunucunuzun. Azure AD Yöneticisi, Azure AD kullanıcısı veya Azure AD grubu olabilir. Yönetilen kimlik Yönetici rolü grubuyla sağlıyorsa, 3 ve 4. adımları atlayın. Yönetici veritabanında tam erişiminiz olacaktır.

2. **[Bağımsız veritabanı kullanıcılarını oluşturun](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)**  Data Factory yönetilen kimliği için. Veri ambarından ya da en az bir Azure AD kimlik ile SSMS gibi araçları kullanarak verileri kopyalamak istediğiniz herhangi bir kullanıcı ALTER izni. Aşağıdaki T-SQL çalıştırın. 
    
    ```sql
    CREATE USER [your Data Factory name] FROM EXTERNAL PROVIDER;
    ```

3. **Data Factory yönetilen kimliği gerekli izinleri vermek** SQL kullanıcılar ve diğerleri için normalde yaptığınız gibi. Aşağıdaki kodu çalıştırın ya da daha fazla seçenek için başvuru [burada](https://docs.microsoft.com/en-us/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017).

    ```sql
    EXEC sp_addrolemember [role name], [your Data Factory name];
    ```

5. **Bir Azure SQL veri ambarı bağlı hizmetini yapılandırma** Azure Data factory'de.

**Örnek:**

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](https://docs.microsoft.com/azure/data-factory/concepts-datasets-linked-services) makalesi. Bu bölümde, Azure SQL veri ambarı veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Gelen veya Azure SQL veri ambarı veri kopyalamak için ayarlanmış **türü** veri kümesine özelliği **AzureSqlDWTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **type** kümesinin özelliği ayarlanmalıdır **AzureSqlDWTable**. | Evet |
| tableName | Tablo veya Görünüm başvuran bağlı hizmetin Azure SQL veri ambarı örneğinde adı. | Kaynak, havuz için Evet Hayır |

#### <a name="dataset-properties-example"></a>Veri kümesi özellikleri örneği

```json
{
    "name": "AzureSQLDWDataset",
    "properties":
    {
        "type": "AzureSqlDWTable",
        "linkedServiceName": {
            "referenceName": "<Azure SQL Data Warehouse linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Azure SQL veri ambarı kaynak ve havuz desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-sql-data-warehouse-as-the-source"></a>Kaynağı olarak Azure SQL veri ambarı

Verileri Azure SQL Data Warehouse'dan veri kopyalamak için ayarlanmış **türü** kopyalama etkinliği kaynak özelliğinde **SqlDWSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **type** kopyalama etkinliği kaynak özelliği ayarlanmalıdır **SqlDWSource**. | Evet |
| sqlReaderQuery | Verileri okumak için özel bir SQL sorgusu kullanın. Örnek: `select * from MyTable`. | Hayır |
| sqlReaderStoredProcedureName | Kaynak tablo verilerini okuyan saklı yordamın adı. Son SQL deyim bir SELECT deyimi saklı yordam içinde olmalıdır. | Hayır |
| storedProcedureParameters | Saklı yordamın parametreleri.<br/>İzin verilen değerler, ad veya değer çiftleridir. Adları ve parametreleri büyük küçük harfleri, adları ve saklı yordam parametreleri büyük küçük harfleri eşleşmelidir. | Hayır |

### <a name="points-to-note"></a>Dikkat edilecek noktaları

- Varsa **sqlReaderQuery** için belirtilen **SqlSource**, kopyalama etkinliği, verileri almak için Azure SQL veri ambarı kaynağına karşı bu sorgu çalıştırır. Veya bir saklı yordam belirtebilirsiniz. Belirtin **sqlReaderStoredProcedureName** ve **storedProcedureParameters** saklı yordamın kullandığı parametreler varsa.
- Ya da belirtmezseniz **sqlReaderQuery** veya **sqlReaderStoredProcedureName**, tanımlanan sütunları **yapısı** JSON veri kümesi bölümünü alışkın olduğunuz bir sorgu oluşturun. `select column1, column2 from mytable` Azure SQL veri ambarı karşı çalışır. Veri kümesi tanımı yoksa **yapısı**, tablodan tüm sütun seçilmedi.

#### <a name="sql-query-example"></a>SQL sorgusu örneği

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDW",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL DW input dataset name>",
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
                "type": "SqlDWSource",
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
        "name": "CopyFromAzureSQLDW",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL DW input dataset name>",
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
                "type": "SqlDWSource",
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

### <a name="azure-sql-data-warehouse-as-sink"></a> Havuz olarak Azure SQL veri ambarı

Azure SQL veri ambarı'na veri kopyalamak için kopyalama etkinliği Havuz türü ayarlayın. **SqlDWSink**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **type** kopyalama etkinliği havuz özelliği ayarlanmalıdır **SqlDWSink**. | Evet |
| allowPolyBase | PolyBase, uygun olduğunda yerine BULKINSERT mekanizması kullanılıp kullanılmayacağını belirtir. <br/><br/> PolyBase kullanarak SQL Data Warehouse'a veri yükleme öneririz. Bkz: [Azure SQL veri ambarı'na veri yüklemek için PolyBase kullanma](#use-polybase-to-load-data-into-azure-sql-data-warehouse) kısıtlamaları ve ayrıntıları bölümü.<br/><br/>İzin verilen değerler **True** ve **False** (varsayılan).  | Hayır |
| polyBaseSettings | Bir grup olabilir özellik belirtilen **Bulunan'allowpolybase** özelliği **true**. | Hayır |
| rejectValue | Sayı veya sorgu başarısız olmadan önce reddedilemiyor satırları yüzdesini belirtir.<br/><br/>Bağımsız değişkenler bölümünde PolyBase'nın reddetme seçenekleri hakkında daha fazla bilgi [CREATE EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx). <br/><br/>İzin verilen değerler: 0 (varsayılan), 1, 2, vs. |Hayır |
| rejectType | Belirtir olup olmadığını **rejectValue** seçenektir değişmez değer veya bir yüzdesi.<br/><br/>İzin verilen değerler **değer** (varsayılan) ve **yüzdesi**. | Hayır |
| rejectSampleValue | Reddedilen satırların yüzdesi PolyBase yeniden hesaplar önce almak için satır sayısını belirler.<br/><br/>İzin verilen değerler: 1, 2, vs. | Evet, varsa **rejectType** olduğu **yüzdesi**. |
| useTypeDefault | PolyBase metin dosyasından veri aldığında sınırlandırılmış metin dosyaları eksik değerleri nasıl ele alınacağını belirtir.<br/><br/>Bağımsız değişkenler bölümünden bu özellik hakkında daha fazla bilgi [oluşturma EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).<br/><br/>İzin verilen değerler **True** ve **False** (varsayılan). | Hayır |
| writeBatchSize | Arabellek boyutu ulaştığında veri SQL tablosuna ekler **writeBatchSize**. Yalnızca PolyBase ne zaman kullanılmaz geçerlidir.<br/><br/>İzin verilen değer **tamsayı** (satır sayısı). | Hayır. Varsayılan 10000'dir. |
| writeBatchTimeout | Toplu ekleme işlemi zaman aşımına uğramadan önce tamamlanması için bir süre bekleyin. Yalnızca PolyBase ne zaman kullanılmaz geçerlidir.<br/><br/>İzin verilen değer **timespan**. Örnek: "00: 30:00" (30 dakika). | Hayır |
| preCopyScript | Her bir çalıştırmada Azure SQL Data Warehouse'a veri yazılmadan önce çalıştırmak kopyalama etkinliği için bir SQL sorgusunu belirtin. Önceden yüklenmiş ve verileri temizlemek için bu özelliği kullanın. | Hayır |

#### <a name="sql-data-warehouse-sink-example"></a>SQL veri ambarı havuzu örnek

```json
"sink": {
    "type": "SqlDWSink",
    "allowPolyBase": true,
    "polyBaseSettings":
    {
        "rejectType": "percentage",
        "rejectValue": 10.0,
        "rejectSampleValue": 100,
        "useTypeDefault": true
    }
}
```

SQL veri ambarı'nda bir sonraki bölüm verimli bir şekilde yüklemek için PolyBase kullanma hakkında daha fazla bilgi edinin.

## <a name="use-polybase-to-load-data-into-azure-sql-data-warehouse"></a>Azure SQL veri ambarı'na veri yüklemek için PolyBase kullanma

Kullanarak [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) büyük miktarda veri, yüksek aktarım hızı ile Azure SQL Data Warehouse'a yüklemek için etkili bir yoludur. Yerine varsayılan BULKINSERT mekanizmasını PolyBase kullanarak büyük bir kazanç aktarım hızının görürsünüz. Bkz: [Performans başvurusu](copy-activity-performance.md#performance-reference) ayrıntılı bir karşılaştırması için. Kullanım örneği ile bir kılavuz için bkz. [1 TB'ı Azure SQL Data Warehouse'a veri yükleme](https://docs.microsoft.com/azure/data-factory/v1/data-factory-load-sql-data-warehouse).

* Kaynak verilerinizi Azure Blob Depolama veya Azure Data Lake Store ve biçimi ise kopyalama doğrudan PolyBase kullanarak Azure SQL veri ambarı PolyBase ile uyumludur. Ayrıntılar için bkz  **[kopyalama PolyBase kullanarak doğrudan](#direct-copy-by-using-polybase)**.
* Kaynak veri deposu ve biçim başlangıçta desteklenmez, PolyBase tarafından kullanın **[kopyalama PolyBase kullanarak hazırlanmış](#staged-copy-by-using-polybase)** bunun yerine özellik. Hazırlanmış kopya özelliğini de daha iyi aktarım hızı sağlar. Otomatik olarak verileri PolyBase ile uyumlu biçimine dönüştürür. Ve verileri Azure Blob Depolama alanında depolar. Ardından verileri SQL veri ambarı'na yükler.

### <a name="direct-copy-by-using-polybase"></a>PolyBase kullanarak doğrudan Kopyala

SQL veri ambarı PolyBase, Azure Blob ve Azure Data Lake Store doğrudan destekler. Bu hizmet sorumlusu kaynağı olarak kullanır ve belirli dosya biçimi gereksinimleri vardır. Veri kaynağınızı Bu bölümde açıklanan ölçütlere uyuyorsa, doğrudan kaynak veri deposundan Azure SQL veri ambarı'na kopyalamak için PolyBase kullanın. Aksi takdirde kullanın [kopyalama PolyBase kullanarak hazırlanmış](#staged-copy-by-using-polybase).

> [!TIP]
> Verileri verimli bir şekilde SQL veri ambarı'na Data Lake Store ' kopyalamak için daha fazla bilgi [Azure Data Factory sağlar, bu, hatta daha kolay ve verileri SQL veri ambarı ile Data Lake Store kullanırken açığa çıkarmak kullanışlı](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/).

Gereksinimleri karşılanmadığı takdirde, Azure Data Factory ayarları denetler ve veri taşıma BULKINSERT mekanizması için otomatik olarak geri döner.

1. **Kaynak bağlı hizmet** türü, Azure Blob Depolama (**AzureBLobStorage**/**AzureStorage**) ile **hesap anahtarı kimlik doğrulaması**  veya Azure Data Lake depolama Gen1 (**birlikte AzureDataLakeStore**) ile **hizmet sorumlusu kimlik doğrulaması**.
2. **Girdi veri kümesi** türü **AzureBlob** veya **AzureDataLakeStoreFile**. Biçim türü altında `type` özellikleri **OrcFormat**, **ParquetFormat**, veya **TextFormat**, aşağıdaki yapılandırmaları ile:

   1. `fileName` joker karakter filtresi içermiyor.
   2. `rowDelimiter` olmalıdır **\n**.
   3. `nullValue` ya da ayarlanmış **boş dize** ("") veya varsayılan olarak sola ve `treatEmptyAsNull` varsayılan sola veya ayarlamak true.
   4. `encodingName` ayarlanır **utf-8**, varsayılan değer olan.
   5. `escapeChar`, `quoteChar` ve `skipLineCount` belirtilmeyen. PolyBase destek Atla olarak yapılandırılan üst bilgi satırı `firstRowAsHeader` ADF içinde.
   6. `compression` olabilir **sıkıştırma**, **GZip**, veya **Deflate**.

      ```json
      "typeProperties": {
        "folderPath": "<blobpath>",
        "format": {
            "type": "TextFormat",
            "columnDelimiter": "<any delimiter>",
            "rowDelimiter": "\n",
            "nullValue": "",
            "encodingName": "utf-8",
            "firstRowAsHeader": <any>
        },
        "compression": {
            "type": "GZip",
            "level": "Optimal"
        }
      },
      ```

```json
"activities":[
    {
        "name": "CopyFromAzureBlobToSQLDataWarehouseViaPolyBase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "BlobDataset",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "AzureSQLDWDataset",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "SqlDWSink",
                "allowPolyBase": true
            }
        }
    }
]
```

### <a name="staged-copy-by-using-polybase"></a>PolyBase kullanarak hazırlanmış Kopyala

Veri kaynağınızı önceki bölümde ölçütleri karşılamayan, veri kopyalama işlemini bir geçici hazırlama Azure Blob Depolama örneği etkinleştirin. Azure Premium depolama olamaz. Bu durumda, Azure Data Factory otomatik olarak PolyBase veri biçim gereksinimlerini karşılamak için veriler üzerinde dönüştürmeler çalıştırır. Ardından verileri SQL Data Warehouse'a veri yüklemek için PolyBase kullanır. Son olarak, blob depolama alanından, geçici verileri temizler. Bkz: [kopyalama aşamalı](copy-activity-performance.md#staged-copy) hazırlama bir Azure Blob Depolama örneği aracılığıyla veri kopyalama hakkında ayrıntılar için.

Bu özelliği kullanmak için oluşturun bir [Azure depolama bağlı hizmeti](connector-azure-blob-storage.md#linked-service-properties) geçici blob depolama ile Azure depolama hesabına başvuruyor. Ardından belirtin `enableStaging` ve `stagingSettings` özellikleri aşağıdaki kodda gösterildiği gibi kopyalama etkinliği için:

```json
"activities":[
    {
        "name": "CopyFromSQLServerToSQLDataWarehouseViaPolyBase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "SQLServerDataset",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "AzureSQLDWDataset",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
            },
            "sink": {
                "type": "SqlDWSink",
                "allowPolyBase": true
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": {
                    "referenceName": "MyStagingBlob",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
]
```

## <a name="best-practices-for-using-polybase"></a>PolyBase kullanmak için en iyi uygulamalar

Aşağıdaki bölümlerde belirtildiği ek olarak en iyi [en iyi uygulamalar için Azure SQL veri ambarı](../sql-data-warehouse/sql-data-warehouse-best-practices.md).

### <a name="required-database-permission"></a>Gerekli database izni

PolyBase kullanmak için SQL Data Warehouse'a veri yükleyen kullanıcının olmalıdır ["CONTROL" izni](https://msdn.microsoft.com/library/ms191291.aspx) hedef veritabanında. Elde etmenin bir yolu olan bir üye olarak kullanıcı eklemek için **db_owner** rol. Bunu yapmak öğrenin [SQL veri ambarı genel bakış](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization).

### <a name="row-size-and-data-type-limits"></a>Satır boyutu ve veri sınırları yazın

PolyBase yükleri satır 1 MB'tan küçük sınırlıdır. Bunlar VARCHR(MAX), NVARCHAR(MAX) veya VARBINARY(MAX) yüklenemiyor. Daha fazla bilgi için [SQL veri ambarı hizmet kapasite sınırları](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).

Veri kaynağınızı satırları 1 MB'den büyük olduğunda, dikey olarak kaynak tablolarda birkaç küçük parçalara bölmek isteyebilirsiniz. Her satırın en büyük boyut sınırı aşmadığından emin olun. Daha küçük tablolar PolyBase kullanılarak yüklenen ve birlikte Azure SQL veri ambarı'nda birleştirildi.

### <a name="sql-data-warehouse-resource-class"></a>SQL veri ambarı kaynak sınıfı

Olası en iyi verimi elde etmek için PolyBase aracılığıyla SQL veri ambarı'na veri yükleyen kullanıcının daha büyük bir kaynak sınıfına atayın.

### <a name="tablename-in-azure-sql-data-warehouse"></a>**tableName** Azure SQL veri ambarı

Aşağıdaki tabloda belirtmek örnekler verilmektedir **tableName** JSON veri kümesi özelliği. Bu, çeşitli birleşimlerini şema ve tablo adları gösterir.

| DB şema | Tablo adı | **tableName** JSON özelliği |
| --- | --- | --- |
| dbo | MyTable | MyTable ya da dbo. MyTable veya [dbo]. [MyTable] |
| dbo1 | MyTable | dbo1. MyTable veya [dbo1]. [MyTable] |
| dbo | My.Table | [My.Table] veya [dbo]. [My.Table] |
| dbo1 | My.Table | [dbo1]. [My.Table] |

Aşağıdaki hatayı görürseniz, sorunu için belirtilen değer olabilir **tableName** özelliği. Doğru şekilde değerlerini belirtmek için önceki tabloya bakın **tableName** JSON özelliği.

```
Type=System.Data.SqlClient.SqlException,Message=Invalid object name 'stg.Account_test'.,Source=.Net SqlClient Data Provider
```

### <a name="columns-with-default-values"></a>Varsayılan değeri olan sütunları

Şu anda, Data Factory, PolyBase özelliği yalnızca aynı sayıda sütun hedef tabloda olduğu gibi kabul eder. Bunlardan biri, varsayılan bir değerle tanımlandığı dört sütunlarını içeren bir tablo buna bir örnektir. Giriş verilerini hala dört sütun olması gerekir. Üç sütunlu giriş veri kümesi aşağıdaki iletiye benzer bir hata verir:

```
All columns of the table must be specified in the INSERT BULK statement.
```

NULL değerini varsayılan değer özel bir biçimidir. Sütun null ise, söz konusu sütun için BLOB giriş veri boş olabilir. Ancak, giriş veri kümesinden eksik olamaz. PolyBase, Azure SQL veri ambarı'nda eksik değerler için NULL ekler.

## <a name="data-type-mapping-for-azure-sql-data-warehouse"></a>Azure SQL veri ambarı için veri türü eşlemesi

Veya Azure SQL veri ambarı veri kopyalayın, aşağıdaki eşlemeler Azure SQL veri ambarı veri türlerinden Azure veri fabrikası geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) kopyalama etkinliği havuz için kaynak şema ve veri türü eşlemelerini nasıl öğrenin.

| Azure SQL veri ambarı veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| bigint | Int64 |
| binary | Byte[] |
| bit | Boolean |
| char | String, Char[] |
| date | DateTime |
| Datetime | DateTime |
| datetime2 | DateTime |
| Datetimeoffset | DateTimeOffset |
| Decimal | Decimal |
| FILESTREAM özniteliğini (varbinary(max)) | Byte[] |
| Float | Double |
| image | Byte[] |
| int | Int32 |
| money | Decimal |
| nchar | String, Char[] |
| ntext | String, Char[] |
| Sayısal | Decimal |
| nvarchar | String, Char[] |
| real | Single |
| rowVersion | Byte[] |
| smalldatetime | DateTime |
| smallint | Int16 |
| küçük para | Decimal |
| sql_variant | Object |
| metin | String, Char[] |
| time | TimeSpan |
| timestamp | Byte[] |
| tinyint | Byte |
| uniqueidentifier | Guid |
| varbinary | Byte[] |
| varchar | String, Char[] |
| xml | Xml |

## <a name="next-steps"></a>Sonraki adımlar
Azure veri fabrikasında kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları ve biçimler](copy-activity-overview.md##supported-data-stores-and-formats).
