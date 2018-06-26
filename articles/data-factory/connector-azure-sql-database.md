---
title: Veri Fabrikası kullanarak ya da Azure SQL veritabanından veri kopyalama | Microsoft Docs
description: Veri Fabrikası kullanarak Azure SQL veritabanı için desteklenen kaynak veri depolarını veya desteklenen havuz veri depolarına SQL veritabanından veri kopyalamak öğrenin.
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
ms.date: 05/05/2018
ms.author: jingwang
ms.openlocfilehash: 9b2acf622f33f5d1748c503ab4765b72c3d921e2
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751587"
---
# <a name="copy-data-to-or-from-azure-sql-database-by-using-azure-data-factory"></a>Azure Data Factory kullanarak veya Azure SQL veritabanından veri kopyalayın
> [!div class="op_single_selector" title1="Select the version of Data Factory service you use:"]
> * [Sürüm 1, GA](v1/data-factory-azure-sql-connector.md)
> * [Sürüm 2, Önizleme](connector-azure-sql-database.md)

Bu makalede kopya etkinliği Azure Data Factory'de ya da Azure SQL veritabanına veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) makalenin kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale, sürüm 2 Data Factory, şu anda önizlemede için geçerlidir. Data Factory hizmeti, genel olarak kullanılabilir (GA), 1 sürümünü kullanıyorsanız bkz [V1 Azure SQL Veritabanı Bağlayıcısı](v1/data-factory-azure-sql-connector.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna ya da Azure SQL veritabanına veri kopyalayabilirsiniz. Ve tüm desteklenen kaynak verileri depolama alanından Azure SQL veritabanına veri kopyalayabilirsiniz. Kaynakları veya havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları ve biçimleri](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Azure SQL veritabanı bağlayıcı bu işlevler destekler:

- Bir hizmet sorumlusu veya yönetilen hizmet kimliği (MSI) ile SQL kimlik doğrulaması ve Azure Active Directory (Azure AD) uygulama belirteci kimlik doğrulaması kullanarak veri kopyalayın.
- Bir kaynağı olarak bir SQL sorgusu veya saklı yordam kullanarak verileri alır.
- Bir havuz olarak verileri hedef tabloya veya kopyalama sırasında özel mantık olan bir saklı yordam çağırma.

> [!IMPORTANT]
> Azure veri fabrikası tümleştirmesi çalışma zamanı kullanarak verileri kopyalarsanız, yapılandırma bir [Azure SQL server Güvenlik Duvarı](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) böylece Azure Hizmetleri sunucu erişebilir.
> Kendini barındıran tümleştirmesi çalışma zamanı kullanarak verileri kopyalarsanız, uygun IP aralığını izin vermek için Azure SQL server güvenlik duvarını yapılandırın. Bu aralık, Azure SQL veritabanına bağlanmak için kullanılan makinenin IP içerir.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli bir Azure SQL Veritabanı Bağlayıcısı tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Bu özellikler Azure SQL veritabanı bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** özelliği ayarlanmalıdır **AzureSqlDatabase**. | Evet |
| connectionString | İçin Azure SQL veritabanı örneğine bağlanmak için gereken bilgileri belirtin **connectionString** özelliği. Bu alan olarak işaretlemek bir **SecureString** veri fabrikasında güvenli bir şekilde depolamak için veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). | Evet |
| servicePrincipalId | Uygulamanın istemci kimliği belirtin. | Evet, bir hizmet sorumlusu ile Azure AD kimlik doğrulaması kullandığınızda. |
| servicePrincipalKey | Uygulamanın anahtarını belirtin. Bu alan olarak işaretlemek bir **SecureString** veri fabrikasında güvenli bir şekilde depolamak için veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). | Evet, bir hizmet sorumlusu ile Azure AD kimlik doğrulaması kullandığınızda. |
| kiracı | Uygulamanızın bulunduğu altında Kiracı bilgileri (etki alanı adı veya Kiracı kimliği) belirtin. Bu, Azure portalının sağ üst köşedeki fare gelerek alın. | Evet, bir hizmet sorumlusu ile Azure AD kimlik doğrulaması kullandığınızda. |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Veri deposu özel bir ağda yer alıyorsa Azure tümleştirmesi çalışma zamanı veya bir kendi kendini barındıran tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. | Hayır |

Farklı kimlik doğrulama türleri için sırasıyla önkoşulları ve JSON örnekleri, aşağıdaki bölümlere bakın:

- [SQL kimlik doğrulaması](#sql-authentication)
- [Azure AD uygulama belirteci kimlik doğrulaması: hizmet sorumlusu](#service-principal-authentication)
- [Azure AD uygulama belirteç kimlik doğrulama: Yönetilen hizmet kimliği](#managed-service-identity-authentication)

### <a name="sql-authentication"></a>SQL kimlik doğrulaması

#### <a name="linked-service-example-that-uses-sql-authentication"></a>SQL kimlik doğrulaması kullanan bağlantılı hizmet örneği

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

### <a name="service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması

Bir hizmet asıl tabanlı Azure AD uygulama belirteci kimlik doğrulaması kullanmak için aşağıdaki adımları izleyin:

1. **[Azure Active Directory Uygulama oluşturma](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application)**  Azure portalından. Uygulama adı ve bağlantılı hizmet tanımlayan aşağıdaki değerleri not edin:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. **[Azure Active Directory yönetici sağlamak](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)**  zaten yapmadıysanız Azure Portal'da Azure SQL sunucunuzun. Azure AD Yöneticisi bir Azure AD kullanıcısının veya Azure AD grubundaki olmalı, ancak bir hizmet sorumlusu olamaz. Sonraki adımda, bir kapsanan veritabanı kullanıcı için hizmet sorumlusu oluşturmak için bir Azure AD kimlik kullanabilmeniz için bu adım gerçekleştirilir.

3. **[Kapsanan veritabanı kullanıcıları oluşturun](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)**  hizmet sorumlusu için. Veritabanından veya en az bir Azure AD kimlik ile SSMS gibi araçları kullanarak veri kopyalamak istediğiniz bağlanmak ALTER herhangi bir kullanıcı izni. Aşağıdaki T-SQL çalıştırın: 
    
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

4. **Hizmet sorumlusu gerekli izinleri vermek** SQL kullanıcılar veya başkaları için normalde yaptığınız gibi. Aşağıdaki kodu çalıştırın:

    ```sql
    EXEC sp_addrolemember [role name], [your application name];
    ```

5. **Bir Azure SQL veritabanı bağlantılı hizmet yapılandırma** Azure veri fabrikası'nda.


#### <a name="linked-service-example-that-uses-service-principal-authentication"></a>Hizmet asıl kimlik doğrulaması kullanan bağlantılı hizmet örneği

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

### <a name="managed-service-identity-authentication"></a>Yönetilen hizmet kimlik doğrulama

Data factory ile ilişkili bir [yönetilen hizmet kimliği](data-factory-service-identity.md) , belirli veri üretecini temsil eder. Bu hizmet kimliği Azure SQL veritabanı kimlik doğrulaması için kullanabilirsiniz. Belirtilen Üreteç erişebilir ve veri kopyalama veritabanınıza ya da bu kimliği kullanarak.

MSI tabanlı Azure AD uygulama belirteci kimlik doğrulaması kullanmak için aşağıdaki adımları izleyin:

1. **Azure AD'de bir grup oluşturun.** MSI Fabrika grubunun bir üyesi yapın.

    a. Veri Fabrikası hizmet kimliği Azure portalından bulun. Veri fabrikasının gidin **özellikleri**. Hizmet kimlik kimliği kopyalayın.

    b. Yükleme [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) modülü. Kullanarak oturum `Connect-AzureAD` komutu. Bir grup oluşturun ve veri fabrikası MSI üyesi olarak eklemek için aşağıdaki komutları çalıştırın.
    ```powershell
    $Group = New-AzureADGroup -DisplayName "<your group name>" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId "<your data factory service identity ID>"
    ```

2. **[Azure Active Directory yönetici sağlamak](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)**  zaten yapmadıysanız Azure Portal'da Azure SQL sunucunuzun. Azure AD Yöneticisi bir Azure AD kullanıcısının veya Azure AD grup olabilir. MSI grubuyla Yönetici rolü izni varsa, 3 ve 4 numaralı adımları atlayın. Yönetici veritabanına tam erişime sahip.

3. **[Kapsanan veritabanı kullanıcıları oluşturun](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)**  Azure AD grubu için. Veritabanından veya en az bir Azure AD kimlik ile SSMS gibi araçları kullanarak veri kopyalamak istediğiniz bağlanmak ALTER herhangi bir kullanıcı izni. Aşağıdaki T-SQL çalıştırın: 
    
    ```sql
    CREATE USER [your AAD group name] FROM EXTERNAL PROVIDER;
    ```

4. **Azure AD grubundaki gerekli izinleri vermek** SQL kullanıcılar ve başkaları için normalde yaptığınız gibi. Örneğin, aşağıdaki kodu çalıştırın:

    ```sql
    EXEC sp_addrolemember [role name], [your AAD group name];
    ```

5. **Bir Azure SQL veritabanı bağlantılı hizmet yapılandırma** Azure veri fabrikası'nda.

#### <a name="linked-service-example-that-uses-msi-authentication"></a>MSI kimlik doğrulaması kullanan bağlantılı hizmet örneği

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

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [veri kümeleri](https://docs.microsoft.com/en-us/azure/data-factory/concepts-datasets-linked-services) makalesi. Bu bölümde Azure SQL veritabanı veri kümesi tarafından desteklenen özellikler listesini sağlar.

Gelen veya Azure SQL veritabanına veri kopyalamak için ayarlanmış **türü** dataset özelliğinin **AzureSqlTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** dataset özelliğinin ayarlanması gerekir **AzureSqlTable**. | Evet |
| tableName | Tablo veya Görünüm başvuran bağlı hizmetin Azure SQL veritabanı örneğinde adı. | Evet |

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
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde Azure SQL veritabanı kaynak ve havuz tarafından desteklenen özellikler listesini sağlar.

### <a name="azure-sql-database-as-the-source"></a>Kaynak olarak Azure SQL veritabanı

Azure SQL veritabanından veri kopyalamak için ayarlanmış **türü** kopyalama etkinliği kaynağına özelliğinde **SqlSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kopyalama etkinliği kaynak özelliği ayarlanmalıdır **SqlSource**. | Evet |
| sqlReaderQuery | Verileri okumak için özel SQL sorgusu kullanın. Örnek: `select * from MyTable`. | Hayır |
| sqlReaderStoredProcedureName | Kaynak tablodan veri okuyan saklı yordamın adı. Son SQL deyimi SELECT deyimi içinde saklı yordamı olması gerekir. | Hayır |
| storedProcedureParameters | Saklı yordam parametreleri.<br/>İzin verilen değerler ad veya değer çiftleridir. Adları ve büyük/küçük harf parametrelerinin adlarını ve saklı yordam parametreleri büyük/küçük harf eşleşmelidir. | Hayır |

### <a name="points-to-note"></a>Dikkat edilecek noktaları

- Varsa **sqlReaderQuery** için belirtilen **SqlSource**, kopyalama etkinliği veri almak için Azure SQL veritabanı kaynağında bu sorguyu çalıştırır. Veya bir saklı yordam belirtebilirsiniz. Belirtin **sqlReaderStoredProcedureName** ve **storedProcedureParameters** saklı yordamın kullandığı parametreler varsa.
- Ya da belirtmezseniz **sqlReaderQuery** veya **sqlReaderStoredProcedureName**, tanımlanan sütunları **yapısı** bölüm JSON veri kümesi için kullanılan bir sorgu oluşturun. `select column1, column2 from mytable` Azure SQL veritabanına karşı çalışır. Veri kümesi tanımı yoksa **yapısı**, tüm sütunlar tablosundan seçilir.
- Kullandığınızda **sqlReaderStoredProcedureName**, yine de bir kukla belirtmeniz gerekiyorsa **tableName** JSON veri kümesi bir özellik.

#### <a name="sql-query-example"></a>SQL sorgu örneği

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

### <a name="stored-procedure-definition"></a>Saklı yordam tanımı

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

### <a name="azure-sql-database-as-the-sink"></a>Havuz olarak Azure SQL veritabanı

Azure SQL veritabanına veri kopyalamak için ayarlanmış **türü** kopyalama etkinliğinde havuz için **SqlSink**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kopyalama etkinliği havuz özelliği ayarlanmalıdır **SqlSink**. | Evet |
| writeBatchSize | Arabellek boyutu ulaştığında veri SQL tablosuna ekler **writeBatchSize**.<br/> İzin verilen değer **tamsayı** (satır sayısı). | Hayır. 10000 varsayılandır. |
| writeBatchTimeout | Toplu işlemi için bekleme süresi işlemi zaman aşımına uğramadan önce son ekleyin.<br/> İzin verilen değer **timespan**. Örnek: "00: 30:00" (30 dakika). | Hayır |
| preCopyScript | Azure SQL veritabanına veri yazmadan önce çalıştırmak kopyalama etkinliği için bir SQL sorgusunu belirtin. Bu yalnızca bir kez çalıştır kopya başına çağrılır. Önceden yüklenen veriyi temizlemek için bu özelliği kullanın. | Hayır |
| sqlWriterStoredProcedureName | Kaynak verileri hedef tabloya uygulamak nasıl tanımlar saklı yordamın adı. Bir örnektir upserts yapın veya tarafından dönüştürmek için kendi iş mantığı kullanarak. <br/><br/>Bu saklı yordam **yığın başına çağrılan**. Yalnızca bir kez çalıştır ve kaynak verileri ile ilgisi yoktur işlemleri için `preCopyScript` özelliği. Örnek işlemler silmeden ve kesmek. | Hayır |
| storedProcedureParameters |Saklı yordam parametreleri.<br/>İzin verilen değerler ad ve değer çiftleridir. Adları ve büyük/küçük harf parametrelerinin adlarını ve saklı yordam parametreleri büyük/küçük harf eşleşmelidir. | Hayır |
| sqlWriterTableType | Saklı yordam, kullanılacak bir tablo türü adı belirtin. Kopyalama etkinliği taşınan veri geçici bir tablo bu tablo türü ile kullanılabilir hale getirir. Saklı yordam kodu ardından var olan verilerle kopyalanan verileri birleştirebilirsiniz. | Hayır |

> [!TIP]
> Azure SQL Database'e veri kopyalama, kopyalama etkinliği varsayılan olarak havuz tabloya veri ekler. Upsert veya ek iş mantığı yapmak için saklı yordamda kullanın **SqlSink**. Daha fazla ayrıntıyı öğrenin [SQL havuzu saklı yordam çağırma](#invoking-stored-procedure-for-sql-sink).

#### <a name="append-data-example"></a>Veri örneği ekleme

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

#### <a name="invoke-a-stored-procedure-during-copy-for-upsert-example"></a>Upsert örneğin kopyalama sırasında bir saklı yordamı çağırma

Daha fazla ayrıntıyı öğrenin [SQL havuzu saklı yordam çağırma](#invoking-stored-procedure-for-sql-sink).

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

Bu bölümde bir kimlik sütunu olmayan bir kaynak tablo verileri bir kimlik sütunu hedef tabloyla kopyalamak nasıl gösterir.

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
> Hedef tabloda bir kimlik sütunu yok.

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
> Kaynak ve hedef tablosu farklı şemalara sahip. 

Hedef bir kimliğe sahip başka bir sütuna sahip. Bu senaryoda, belirtmelisiniz **yapısı** kimlik sütunu içermeyen hedef veri kümesi tanımında özelliği.

## <a name="invoking-stored-procedure-for-sql-sink"></a> SQL havuz depolanan yordamı çağırma

Azure SQL veritabanına veri kopyaladığınızda, yapılandırmak ve bir kullanıcı tarafından belirtilen saklı yordam ek parametrelerle çağırın.

Yerleşik kopyalama mekanizmaları amaca hizmet verme sırasında saklı yordamı kullanabilirsiniz. Genellikle bir upsert, ekleme ve güncelleştirme olduğunda kullanıldıklarından veya ek işleme hedef tablonun son ekleme kaynak verilerin önce yapılmalıdır. Birleştirme sütunları, ek değerler ve birden fazla tablosuna Ara bazı ek işleme örnektir.

Aşağıdaki örnek, Azure SQL veritabanındaki bir tabloya bir upsert yapmak için bir saklı yordam kullanmayı gösterir. Varsayın veri ve havuz giriş **pazarlama** tablosu her üç sütuna sahip: **Profileıd**, **durumu**, ve **kategori**. Temel upsert yapmak **Profileıd** sütun ve yalnızca belirli bir kategorideki için geçerlidir.

#### <a name="output-dataset"></a>Çıktı veri kümesi

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

Tanımlamak **SqlSink** kopyalama etkinliği bölümünde:

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

Aynı ada sahip saklı yordam veritabanınızdaki tanımlamak **SqlWriterStoredProcedureName**. Belirtilen kaynak gelen giriş verilerinin işler ve çıkış tablosuna birleştirir. Saklı yordam parametresinin adı aynı olmalıdır **tableName** kümesinde tanımlanan.

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

Aynı ada sahip bir tablo türü veritabanınızdaki tanımlamak **sqlWriterTableType**. Tablo türü şeması giriş verilerinizi tarafından döndürülen şema ile aynı olmalıdır.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL,
    [Category] [varchar](256) NOT NULL,
)
```

Saklı yordam özellik yararlandığı [tablo değerli parametreleri](https://msdn.microsoft.com/library/bb675163.aspx).

## <a name="data-type-mapping-for-azure-sql-database"></a>Azure SQL veritabanı için veri türü eşlemesi

Aşağıdaki eşlemelerini ya da Azure SQL veritabanına veri kopyaladığınızda, Azure SQL veritabanı veri türlerinden Azure Data Factory geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) kopyalama etkinliği kaynak şema ve veri türü için havuz nasıl eşlendiğini öğrenin.

| Azure SQL veritabanı veri türü | Veri Fabrikası geçici veri türü |
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
| Mini tamsayı |Bayt |
| benzersiz tanımlayıcı |Guid |
| varbinary |Byte] |
| varchar |Dize, Char] |
| xml |Xml |

## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları ve biçimleri](copy-activity-overview.md##supported-data-stores-and-formats).
