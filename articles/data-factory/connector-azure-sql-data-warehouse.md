---
title: Veri kopyalama/Data Factory kullanarak Azure SQL veri ambarından | Microsoft Docs
description: Azure SQL Data Warehouse için desteklenen kaynak depoları (veya) SQL veri ambarından veri kopyalamak için desteklenen havuz depoları Data Factory kullanarak öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2018
ms.author: jingwang
ms.openlocfilehash: 0bc24fb0206455c723acf5e6f4b82d82002f727c
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="copy-data-to-or-from-azure-sql-data-warehouse-by-using-azure-data-factory"></a>Azure Data Factory kullanarak veya Azure SQL veri ambarından veri kopyalayın
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-azure-sql-data-warehouse-connector.md)
> * [Sürüm 2 - Önizleme](connector-azure-sql-data-warehouse.md)

Bu makalede kopya etkinliği Azure Data Factory'de ve bir Azure SQL veri ambarından veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 Azure SQL Data Warehouse Bağlayıcısı](v1/data-factory-azure-sql-data-warehouse-connector.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna başlangıç/bitiş Azure SQL Data Warehouse verilerini kopyalayabilir veya verileri Azure SQL Data Warehouse için tüm desteklenen kaynak veri deposundan kopyalayın. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Azure SQL Data Warehouse Bağlayıcısı destekler:

- Verileri kullanarak kopyalama **SQL kimlik doğrulaması**, ve **Azure Active Directory Uygulama belirteci kimlik doğrulaması** hizmet sorumlusu veya yönetilen hizmet kimliği (MSI).
- SQL sorgusu veya saklı yordam kullanarak veri kaynağı olarak alınıyor.
- Havuz, verileri kullanarak yükleme olarak **PolyBase** veya toplu ekleme. Eski olan **önerilen** daha iyi kopyalama performans.

> [!IMPORTANT]
> Not PolyBase, SQL authentcation ancak değil Azure Active Directory kimlik doğrulaması yalnızca destekler.

> [!IMPORTANT]
> Azure tümleştirmesi çalışma zamanı kullanarak verileri kopyalarsanız, yapılandırma [Azure SQL Server Güvenlik Duvarı](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) için [Azure hizmetlerinin sunucuya erişimine izin](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure). Self-hosted tümleştirme çalışma zamanı kullanarak verileri kopyalarsanız, Azure SQL veritabanına bağlanmak için kullanılan makinenin IP de dahil olmak üzere uygun IP aralığını izin vermek için Azure SQL Server Güvenlik Duvarı'nı yapılandırın.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler için Azure SQL Data Warehouse Bağlayıcısı Data Factory varlıklarını belirli tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler, Azure SQL Data Warehouse bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **AzureSqlDW** | Evet |
| connectionString |ConnectionString özelliği için Azure SQL Data Warehouse örneğine bağlanmak için gereken bilgileri belirtin. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). |Evet |
| servicePrincipalId | Uygulamanın istemci kimliği belirtin. | AAD kimlik doğrulama ile hizmet sorumlusu kullanırken, Evet. |
| servicePrincipalKey | Uygulamanın anahtarını belirtin. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). | AAD kimlik doğrulama ile hizmet sorumlusu kullanırken, Evet. |
| kiracı | Uygulamanızın bulunduğu altında Kiracı bilgileri (etki alanı adı veya Kiracı kimliği) belirtin. Azure portalının sağ üst köşedeki fare gelerek alabilir. | AAD kimlik doğrulama ile hizmet sorumlusu kullanırken, Evet. |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu özel bir ağda yer alıyorsa) Azure tümleştirmesi çalışma zamanı veya Self-hosted tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. |Hayır |

Farklı kimlik doğrulama türleri için sırasıyla önkoşulları ve JSON örnekleri aşağıdaki bölümlere bakın:

- [SQL kimlik doğrulaması kullanma](#using-sql-authentication)
- [AAD uygulama belirteci kimlik doğrulaması - hizmet sorumlusunu kullanarak](#using-service-principal-authentication)
- [AAD uygulama belirteci kimlik doğrulaması - yönetilen hizmet kimliği kullanma](#using-managed-service-identity-authentication)

### <a name="using-sql-authentication"></a>SQL kimlik doğrulaması kullanma

**SQL kimlik doğrulaması kullanarak bağlantılı hizmet örneği:**

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

### <a name="using-service-principal-authentication"></a>Hizmet asıl kimlik doğrulaması kullanma

Hizmet asıl tabanlı AAD uygulama belirteci kimlik doğrulaması kullanmak için aşağıdaki adımları izleyin:

1. **[Bir Azure Active Directory uygulaması Azure portalından oluşturmak](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application).**  Uygulama adı ve bağlantılı hizmet tanımlamak üzere kullanabileceğiniz aşağıdaki değerleri not edin:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. **[Azure Active Directory yönetici sağlamak](../sql-database/sql-database-aad-authentication-configure.md#create-an-azure-ad-administrator-for-azure-sql-server) ** bunu yapmadıysanız Azure Portal'da Azure SQL Server. AAD yönetici bir AAD kullanıcı veya AAD grup olabilir. MSI grubuyla Yönetici rolü izni varsa, yönetici DB tam erişim yaptığınız gibi adım 3 ve 4 aşağıda atlayın.

3. **Kapsanan veritabanı kullanıcı için hizmet sorumlusu oluşturmak**, bağlayarak veri ambarından/SSMS gibi araçları kullanarak veri kopyalamak istediğiniz bir AAD ile kimlik en az olması ALTER herhangi bir kullanıcı izni ve aşağıdaki T-SQL yürütme . Kapsanan veritabanı kullanıcıdan hakkında daha fazla bilgi edinin [burada](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities).
    
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

4. **Hizmet sorumlusu gerekli izinleri vermek** , SQL kullanıcılar için Örneğin aşağıda çalıştırarak her zamanki gibi:

    ```sql
    EXEC sp_addrolemember '[your application name]', 'readonlyuser';
    ```

5. ADF içinde bir Azure SQL Data Warehouse bağlı hizmetini yapılandırın.


**Hizmet asıl kimlik doğrulaması kullanarak bağlantılı hizmet örneği:**

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

### <a name="using-managed-service-identity-authentication"></a>Yönetilen hizmet kimlik doğrulama kullanılarak

Data factory ile ilişkilendirilebilir bir [yönetilen hizmet kimliği (MSI)](data-factory-service-identity.md), bu belirli veri fabrikası temsil eder. Bu hizmet kimliği, verilerinizi veri ambarına / erişim ve kopyalama verilere atanmış bu Fabrika sağlayan, Azure SQL Data Warehouse kimlik doğrulaması için kullanabilirsiniz.

Tabanlı AAD uygulama belirteci kimlik doğrulaması MSI kullanmak için aşağıdaki adımları izleyin:

1. **Azure AD'de bir grup oluşturun ve MSI Fabrika grubunun bir üyesi yapın**.

    a. Azure Portalı'ndan veri fabrikası hizmet kimliği bulunamıyor. Veri fabrikanızın -> Özellikler kopyalama -> **hizmet kimlik kimliği**.

    b. Yükleme [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) Modülü'nü kullanarak oturum `Connect-AzureAD` komut ve bir grup oluşturun ve veri fabrikası MSI üyesi olarak eklemek için aşağıdaki komutları çalıştırın.
    ```powershell
    $Group = New-AzureADGroup -DisplayName "<your group name>" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId "<your data factory service identity ID>"
    ```

2. **[Azure Active Directory yönetici sağlamak](../sql-database/sql-database-aad-authentication-configure.md#create-an-azure-ad-administrator-for-azure-sql-server) ** bunu yapmadıysanız Azure Portal'da Azure SQL Server.

3. **AAD grubu için kapsanan veritabanı kullanıcısı oluşturmak**, bağlayarak veri ambarından/SSMS gibi araçları kullanarak veri kopyalamak istediğiniz bir AAD ile kimlik en az olması ALTER herhangi bir kullanıcı izni ve aşağıdaki T-SQL yürütme. Kapsanan veritabanı kullanıcıdan hakkında daha fazla bilgi edinin [burada](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities).
    
    ```sql
    CREATE USER [your AAD group name] FROM EXTERNAL PROVIDER;
    ```

4. **AAD Grup gerekli izinleri vermek** , SQL kullanıcılar için Örneğin aşağıda çalıştırarak her zamanki gibi:

    ```sql
    EXEC sp_addrolemember '[your AAD group name]', 'readonlyuser';
    ```

5. ADF içinde bir Azure SQL Data Warehouse bağlı hizmetini yapılandırın.

**MSI kimlik doğrulaması kullanarak bağlantılı hizmet örneği:**

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

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için veri kümeleri makalesine bakın. Bu bölümde Azure SQL veri ambarı veri kümesi tarafından desteklenen özellikler listesini sağlar.

/ Azure SQL Data warehouse'a veri kopyalamak için veri kümesi için tür özelliği ayarlamak **AzureSqlDWTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak: **AzureSqlDWTable** | Evet |
| tableName |Tablo veya Görünüm başvuran bağlı hizmetin Azure SQL Data Warehouse örneğinde adı. | Evet |

**Örnek:**

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

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde Azure SQL Data Warehouse kaynak ve havuz tarafından desteklenen özellikler listesini sağlar.

### <a name="azure-sql-data-warehouse-as-source"></a>Kaynak olarak Azure SQL veri ambarı

Azure SQL veri ambarından veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **SqlDWSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **SqlDWSource** | Evet |
| sqlReaderQuery |Verileri okumak için özel SQL sorgusu kullanın. Örnek: `select * from MyTable`. |Hayır |
| sqlReaderStoredProcedureName |Kaynak tablodan veri okuyan saklı yordamın adı. Son SQL deyimi SELECT deyimi içinde saklı yordamı olması gerekir. |Hayır |
| storedProcedureParameters |Saklı yordam parametreleri.<br/>İzin verilen değerler: ad/değer çiftleri. Adları ve büyük/küçük harf parametrelerinin adlarını ve saklı yordam parametreleri büyük/küçük harf eşleşmelidir. |Hayır |

**Dikkat edilecek noktalar:**

- Varsa **sqlReaderQuery** belirtilen SqlSource için kopyalama etkinliği veri almak için Azure SQL Data Warehouse kaynağında bu sorguyu çalıştırır. Alternatif olarak, bir saklı yordam belirterek belirleyebileceğiniz **sqlReaderStoredProcedureName** ve **storedProcedureParameters** (saklı yordam parametreleri alıyorsa).
- "SqlReaderQuery" veya "sqlReaderStoredProcedureName" belirtmezseniz, JSON veri kümesi'nin "yapısı" bölümünde tanımlanan sütunları bir sorgu oluşturmak için kullanılır (`select column1, column2 from mytable`) Azure SQL veri ambarına karşı çalıştırmak için. Veri kümesi tanımı "yapısı" yoksa, tüm sütunları tablodan seçilir.
- Kullandığınızda **sqlReaderStoredProcedureName**, yine de bir kukla belirtmeniz gerekiyorsa **tableName** JSON veri kümesi bir özellik.

**Örnek: SQL sorgusu kullanma**

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

**Örnek: saklı yordam kullanma**

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

### <a name="azure-sql-data-warehouse-as-sink"></a>Havuz olarak Azure SQL veri ambarı

Azure SQL Data Warehouse'a veri kopyalamak için kopyalama etkinliği Havuz türü ayarlayın. **SqlDWSink**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopya etkinliği havuz tür özelliği ayarlamak: **SqlDWSink** | Evet |
| Bulunan'allowpolybase |BULKINSERT mekanizması yerine PolyBase (varsa) kullanılıp kullanılmayacağını belirtir. <br/><br/> **PolyBase kullanarak SQL Data Warehouse'a veri yükleme için önerilen yoldur.** Bkz [kullanım Azure SQL Data Warehouse'a veri yüklemek için PolyBase](#use-polybase-to-load-data-into-azure-sql-data-warehouse) kısıtlamaları ve ayrıntıları bölümü.<br/><br/>İzin verilen değerler: **True** (varsayılan), ve **False**.  |Hayır |
| polyBaseSettings |Bir grup olabilir özellik belirtilen **Bulunan'allowpolybase** özelliği ayarlanmış **doğru**. |Hayır |
| rejectValue |Sayı veya yüzde değeri sorgu başarısız önce reddedilemiyor satır belirtir.<br/><br/>PolyBase'nın reddetme seçenekleri hakkında daha fazla bilgi **bağımsız değişkenleri** bölümünü [CREATE dış TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) konu. <br/><br/>İzin verilen değerler: 0 (varsayılan), 1, 2... |Hayır |
| rejectType |RejectValue seçeneği bir hazır değer veya bir yüzde belirtilen belirtir.<br/><br/>İzin verilen değerler: **değeri** (varsayılan), ve **yüzde**. |Hayır |
| Havuzu'na ilişkin |PolyBase reddedilen satırları yüzdesini yeniden hesaplar önce almak için satır sayısını belirler.<br/><br/>İzin verilen değerler: 1, 2... |Evet, varsa **rejectType** olan **yüzdesi** |
| useTypeDefault |PolyBase metin dosyasından veri aldığında sınırlandırılmış metin dosyaları eksik değerleri nasıl ele alınacağını belirtir.<br/><br/>Bağımsız değişkenler bölümünde bu özelliği hakkında daha fazla bilgi [oluşturma EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).<br/><br/>İzin verilen değerler: **True**, **False** (varsayılan). |Hayır |
| writeBatchSize |Arabellek boyutu writeBatchSize ulaştığında veri SQL tablosuna ekler. Yalnızca PolyBase değil kullanıldığında geçerlidir.<br/><br/>İzin verilen değerler: tamsayı (satır sayısı). |Hayır (varsayılan değeri: 10000) |
| writeBatchTimeout |Toplu ekleme işlemi zaman aşımına uğramadan önce tamamlamak bir süre bekleyin. Yalnızca PolyBase değil kullanıldığında geçerlidir.<br/><br/>İzin verilen değerler: timespan. Örnek: "00: 30:00" (30 dakika). |Hayır |
| preCopyScript |Her çalıştırma sırasında Azure SQL Data Warehouse'a veri yazmadan önce yürütmek kopyalama etkinliği için bir SQL sorgusunu belirtin. Önceden yüklenmiş veriyi temizlemek için bu özelliği kullanın. |Hayır |(#repeatability sırasında-kopyalama). |Sorgu bildirimi. |Hayır |

**Örnek:**

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

PolyBase SQL Data Warehouse sonraki bölümünden verimli bir şekilde yüklemek için nasıl kullanılacağı hakkında daha fazla bilgi edinin.

## <a name="use-polybase-to-load-data-into-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse'a veri yüklemek için Polybase'i kullanın

Kullanarak ** [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) ** büyük miktarda veri yüksek işleme ile Azure SQL Data Warehouse'a veri yükleme etkili bir yoldur. PolyBase yerine varsayılan BULKINSERT mekanizmasını kullanarak büyük kazanç verimliliği de görebilirsiniz. Bkz: [kopyalama performans başvuru numarası](copy-activity-performance.md#performance-reference) ayrıntılı karşılaştırması ile. Kullanım örneği ile bir anlatım için bkz: [1 TB altında 15 dakika Azure Data Factory ile Azure SQL Data Warehouse'a veri yükleme](connector-azure-sql-data-warehouse.md).

* Veri kaynağınızı ise **Azure Blob veya Azure Data Lake Store**ve biçimini PolyBase ile uyumlu ise, doğrudan Azure SQL veri Polybase'i kullanarak ambarına kopyalayabilirsiniz. Bkz: ** [Polybase'i kullanarak doğrudan kopyalama](#direct-copy-using-polybase) ** ayrıntılarla.
* Kaynak veri deposu ve biçim başlangıçta desteklenmiyor, PolyBase tarafından kullanabileceğiniz ** [Polybase'i kullanarak kopyalama hazırlanan](#staged-copy-using-polybase) ** yerine özellik. Ayrıca, daha iyi verim otomatik olarak veri PolyBase uyumlu biçimine dönüştürmek için kullanılan ve Azure Blob depolama alanına veri depolayarak sağlar. Ardından verileri SQL Data Warehouse'a veri yükler.

> [!IMPORTANT]
> Not PolyBase, Azure SQL veri ambarı SQL authentcation ancak değil Azure Active Directory kimlik doğrulaması yalnızca destekler.

### <a name="direct-copy-using-polybase"></a>Polybase'i kullanarak doğrudan kopyalama

SQL veri ambarı PolyBase doğrudan desteği Azure Blob ve Azure Data Lake Store (hizmet sorumlusu kullanarak) kaynak olarak ve belirli dosya biçimi gereksinimleriyle. Veri kaynağınızı Bu bölümde açıklanan ölçütlerini karşılıyorsa, kaynak veri deposundan doğrudan Azure SQL veri Polybase'i kullanarak ambarına kopyalayabilirsiniz. Aksi takdirde, kullanabileceğiniz [Polybase'i kullanarak kopyalama hazırlanan](#staged-copy-using-polybase).

> [!TIP]
> Verimli bir şekilde SQL veri ambarı için verileri Data Lake Deposu'ndan veri kopyalamak için'dan daha fazla bilgi edinin [Azure Data Factory sağlar, bu, daha kolay ve Data Lake Store ile SQL veri ambarı kullanırken verilerden Öngörüler ortaya çıkarmak uygun bile](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/).

Gereksinimler karşılanmazsa, Azure Data Factory ayarları denetler ve veri taşıma için BULKINSERT mekanizması için otomatik olarak geri döner.

1. **Kaynak bağlantılı hizmeti** türüdür: **AzureStorage** veya **AzureDataLakeStore** hizmet asıl kimlik doğrulamasına sahip.
2. **Girdi veri kümesi** türüdür: **AzureBlob** veya **AzureDataLakeStoreFile**ve altında yazın biçimi `type` özellikleri **OrcFormat **, **ParquetFormat**, veya **TextFormat** aşağıdaki yapılandırmalara sahip:

   1. `rowDelimiter` olmalıdır **\n**.
   2. `nullValue` ayarlanmış **boş dize** (""), veya `treatEmptyAsNull` ayarlanır **doğru**.
   3. `encodingName` ayarlanmış **utf-8**, olduğu **varsayılan** değeri.
   4. `escapeChar`, `quoteChar`, `firstRowAsHeader`, ve `skipLineCount` belirtilmedi.
   5. `compression` olabilir **sıkıştırma yok**, **GZip**, veya **Deflate**.

    ```json
    "typeProperties": {
       "folderPath": "<blobpath>",
       "format": {
           "type": "TextFormat",
           "columnDelimiter": "<any delimiter>",
           "rowDelimiter": "\n",
           "nullValue": "",
           "encodingName": "utf-8"
       },
       "compression": {
           "type": "GZip",
           "level": "Optimal"
       }
    },
    ```

3. Yoktur hiçbir `skipHeaderLineCount` altında ayarı **BlobSource** veya **AzureDataLakeStore** işlem hattının kopyalama etkinliği için.
4. Var olan hiçbir `sliceIdentifierColumnName` altında ayarlama **SqlDWSink** işlem hattının kopyalama etkinliği için. (Tüm veriler güncelleştirilir veya hiçbir şey bir tek çalıştırmada güncelleştirildiğinde PolyBase garanti eder. Elde etmek için **Yinelenebilirlik**, kullanabileceğinizi `sqlWriterCleanupScript`).

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

### <a name="staged-copy-using-polybase"></a>Polybase'i kullanarak hazırlanmış kopyalama

Veri kaynağınızı önceki bölümde sunulan ölçütlere uyan değil, bir geçici hazırlama Azure Blob (Premium depolama olamaz) depolama aracılığıyla veri kopyalamayı etkinleştirebilirsiniz. Bu durumda, Azure Data Factory otomatik olarak dönüşümleri verileri PolyBase, veri biçimi gereksinimlerini karşılamak sonra verileri SQL Data Warehouse'a veri yüklemek için Polybase'i kullanın ve ardından geçici verilerinizi Blob depolama biriminden temizleyin gerçekleştirir. Bkz: [hazırlanan kopyalama](copy-activity-performance.md#staged-copy) nasıl hazırlama Azure Blob üzerinden veri kopyalama genel çalıştığı hakkında bilgi.

Bu özelliği kullanmak için oluşturma bir [Azure depolama bağlantılı hizmeti](connector-azure-blob-storage.md#linked-service-properties) Ara blob depolama alanına sahip Azure depolama hesabı anlamına gelir, sonra belirtin `enableStaging` ve `stagingSettings` kopyalama gösterildiği gibi etkinliğin özellikleri Aşağıdaki kod:

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

## <a name="best-practices-when-using-polybase"></a>PolyBase kullanırken en iyi uygulamalar

Aşağıdaki bölümlerde bölümünde belirtildiği olanlara ek en iyi yöntemleri sağlamak [en iyi uygulamalar Azure SQL Data Warehouse için](../sql-data-warehouse/sql-data-warehouse-best-practices.md).

### <a name="required-database-permission"></a>Gerekli veritabanı izni

PolyBase kullanmak için SQL Data Warehouse'a veri yükleme için kullanılan kullanıcı gerektiriyor ["Denetim" izni](https://msdn.microsoft.com/library/ms191291.aspx) hedef veritabanı üzerinde. Elde etmenin bir yolu, "db_owner" rolünün bir üyesi o kullanıcı eklemektir. Aşağıdaki kullanarak bunu öğrenin [Bu bölümde](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization).

### <a name="row-size-and-data-type-limitation"></a>Satır boyutu ve verileri sınırlama yazın

Polybase yükleri sınırlı yükleme satırları hem küçük **1 MB** ve VARCHR(MAX), NVARCHAR(MAX) veya VARBINARY(MAX) yüklenemiyor. Başvurmak [burada](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).

Kaynak verileri satır boyutu 1 MB'tan fazla ile varsa, kaynak tabloları dikey Burada bunların her biri en büyük satır boyutu sınırı aşmadığından birkaç küçük parçalara bölmek isteyebilirsiniz. Daha küçük tabloları sonra PolyBase kullanarak yüklenebilir ve Azure SQL Data Warehouse'da birleştirmeniz.

### <a name="sql-data-warehouse-resource-class"></a>SQL veri ambarı kaynak sınıfı

Olası en iyi verime ulaşmak için daha büyük bir kaynak sınıfı PolyBase aracılığıyla SQL veri ambarında verileri yüklemek için kullanılan kullanıcı atama göz önünde bulundurun.

### <a name="tablename-in-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse tableName

Aşağıdaki tabloda belirleme konusunda örnekler **tableName** dataset JSON çeşitli şema ve tablo adı için bir özellik.

| DB şeması | Tablo adı | tableName JSON özelliği |
| --- | --- | --- |
| dbo |MyTable |MyTable ya da dbo. MyTable ya da [dbo]. [MyTable] |
| dbo1 |MyTable |dbo1. MyTable veya [dbo1]. [MyTable] |
| dbo |My.Table |[My.Table] veya [dbo]. [My.Table] |
| dbo1 |My.Table |[dbo1]. [My.Table] |

Aşağıdaki hata görürseniz, tableName özelliği için belirtilen değer ile ilgili bir sorun olabilir. TableName JSON özellik değerlerini belirtmek doğru bir şekilde için tabloya bakın.

```
Type=System.Data.SqlClient.SqlException,Message=Invalid object name 'stg.Account_test'.,Source=.Net SqlClient Data Provider
```

### <a name="columns-with-default-values"></a>Varsayılan değerleri içeren sütun

Şu anda, veri fabrikası'nda PolyBase özelliği yalnızca aynı sayıda sütun hedef tabloda olduğu gibi kabul eder. Dört sütun içeren bir tablo varsa ve bunlardan birini varsayılan bir değerle tanımlanan söyleyin. Giriş verisi hala dört sütun içermelidir. 3-sütun girdi veri kümesi sağlayan aşağıdaki iletiye benzer bir hata verir:

```
All columns of the table must be specified in the INSERT BULK statement.
```

NULL değer, varsayılan değeri özel bir biçimidir. Sütun null ise girdi verilerini (blob) Bu sütun için boş olabilir (giriş kümesinden eksik olamaz). PolyBase, Azure SQL Data Warehouse bunları NULL ekler.

## <a name="data-type-mapping-for-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse için veri türü eşlemesi

Başlangıç/bitiş Azure SQL Data Warehouse veri kopyalama işlemi sırasında aşağıdaki eşlemelerini Azure SQL Data Warehouse veri türlerinden Azure Data Factory geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) nasıl kopyalama etkinliği kaynak şema ve veri türü için havuz eşlemeleri hakkında bilgi edinmek için.

| Azure SQL veri ambarı veri türü | Veri Fabrikası geçici veri türü |
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
| Kayan nokta |Çift |
| görüntü |Byte] |
| Int |Int32 |
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
| Metin |Dize, Char] |
| time |TimeSpan |
| timestamp |Byte] |
| Mini tamsayı |Bayt |
| benzersiz tanımlayıcı |Guid |
| varbinary |Byte] |
| varchar |Dize, Char] |
| xml |Xml |

## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).