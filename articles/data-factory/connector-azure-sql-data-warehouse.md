---
title: Azure Data Factory kullanarak ve Azure SQL veri ambarından veri kopyalamak | Microsoft Docs
description: Veri Fabrikası kullanarak Azure SQL Data Warehouse için desteklenen kaynak depoları veya SQL Data Warehouse desteklenen havuz depolarına veri kopyalamak öğrenin.
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
ms.date: 05/28/2018
ms.author: jingwang
ms.openlocfilehash: 5a7ee7862e102093efa2c203eac2497b025af4e5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36337824"
---
#  <a name="copy-data-to-or-from-azure-sql-data-warehouse-by-using-azure-data-factory"></a>Azure Data Factory kullanarak veya Azure SQL veri ambarından veri kopyalayın 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you're using:"]
> * [Sürüm 1: GA](v1/data-factory-azure-sql-data-warehouse-connector.md)
> * [Sürüm 2: Önizleme](connector-azure-sql-data-warehouse.md)

Bu makalede kopya etkinliği Azure Data Factory'de veya Azure SQL veri ambarından veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale, sürüm 2 Data Factory, şu anda önizlemede için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 Azure SQL Data Warehouse Bağlayıcısı](v1/data-factory-azure-sql-data-warehouse-connector.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Azure SQL veri ambarından veri tüm desteklenen havuz veri deposuna kopyalayabilirsiniz. Ve tüm desteklenen kaynak veri deposundan Azure SQL Data Warehouse'a veri kopyalayabilirsiniz. Kaynakları veya havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları ve biçimleri](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Azure SQL Data Warehouse Bağlayıcısı bu işlevler destekler:

- Bir hizmet sorumlusu veya yönetilen hizmet kimliği (MSI) ile SQL kimlik doğrulaması ve Azure Active Directory (Azure AD) uygulama belirteci kimlik doğrulaması kullanarak veri kopyalayın.
- Bir kaynağı olarak bir SQL sorgusu veya saklı yordam kullanarak verileri alır.
- Bir havuz PolyBase ya da bir toplu kullanarak yük veri ekleyin. Daha iyi kopyalama performans PolyBase öneririz.

> [!IMPORTANT]
> PolyBase yalnızca SQL kimlik doğrulaması ancak değil Azure AD kimlik doğrulaması desteklediğini unutmayın.

> [!IMPORTANT]
> Azure veri fabrikası tümleştirmesi çalışma zamanı kullanarak verileri kopyalarsanız, yapılandırma bir [Azure SQL server Güvenlik Duvarı](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) Azure hizmetlerinin sunucunun erişebilmesi için.
> Kendini barındıran tümleştirmesi çalışma zamanı kullanarak verileri kopyalarsanız, uygun IP aralığını izin vermek için Azure SQL server güvenlik duvarını yapılandırın. Bu aralık, Azure SQL veritabanına bağlanmak için kullanılan makinenin IP içerir.

## <a name="get-started"></a>başlarken

> [!TIP]
> En iyi performansı elde etmek için Azure SQL Data Warehouse'a veri yüklemek için PolyBase kullanın. [Kullanım Azure SQL Data Warehouse'a veri yüklemek için PolyBase](#use-polybase-to-load-data-into-azure-sql-data-warehouse) bölümde ayrıntılar bulunur. Kullanım örneği ile bir anlatım için bkz: [1 TB altında 15 dakika Azure Data Factory ile Azure SQL Data Warehouse'a veri yükleme](load-azure-sql-data-warehouse.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli bir Azure SQL Data Warehouse Bağlayıcısı tanımlama özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler, Azure SQL Data Warehouse bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlamak **AzureSqlDW**. | Evet |
| connectionString | İçin Azure SQL Data Warehouse örneğine bağlanmak için gereken bilgileri belirtin **connectionString** özelliği. Bu alan olarak işaretlemek bir **SecureString** veri fabrikasında güvenli bir şekilde depolamak için veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). | Evet |
| servicePrincipalId | Uygulamanın istemci kimliği belirtin. | Evet, bir hizmet sorumlusu ile Azure AD kimlik doğrulaması kullandığınızda. |
| servicePrincipalKey | Uygulamanın anahtarını belirtin. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). | Evet, bir hizmet sorumlusu ile Azure AD kimlik doğrulaması kullandığınızda. |
| kiracı | Uygulamanızın bulunduğu altında Kiracı bilgileri (etki alanı adı veya Kiracı kimliği) belirtin. Azure portalının sağ üst köşedeki fare gelerek alabilir. | Evet, bir hizmet sorumlusu ile Azure AD kimlik doğrulaması kullandığınızda. |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu özel bir ağda yer alıyorsa) Azure tümleştirmesi çalışma zamanı veya bir kendi kendini barındıran tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. | Hayır |

Farklı kimlik doğrulama türleri için sırasıyla önkoşulları ve JSON örnekleri, aşağıdaki bölümlere bakın:

- [SQL kimlik doğrulaması](#sql-authentication)
- Azure AD uygulama belirteci kimlik doğrulaması: [hizmet sorumlusu](#service-principal-authentication)
- Azure AD uygulama belirteci kimlik doğrulaması: [yönetilen hizmet kimliği](#managed-service-identity-authentication)

### <a name="sql-authentication"></a>SQL kimlik doğrulaması

#### <a name="linked-service-example-that-uses-sql-authentication"></a>SQL kimlik doğrulaması kullanan bağlantılı hizmet örneği

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

### <a name="service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması

Hizmet sorumlusu tabanlı Azure AD uygulama belirteci kimlik doğrulaması kullanmak için aşağıdaki adımları izleyin:

1. **[Azure Active Directory Uygulama oluşturma](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application)**  Azure portalından. Uygulama adı ve bağlantılı hizmet tanımlayan aşağıdaki değerleri not edin:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. **[Azure Active Directory yönetici sağlamak](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)**  zaten yapmadıysanız Azure Portal'da Azure SQL sunucunuzun. Azure AD Yöneticisi bir Azure AD kullanıcısının veya Azure AD grup olabilir. MSI grubuyla Yönetici rolü izni varsa, 3 ve 4 numaralı adımları atlayın. Yönetici veritabanına tam erişime sahip.

3. **[Kapsanan veritabanı kullanıcıları oluşturun](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)**  hizmet sorumlusu için. Veri ambarı veya en az bir Azure AD kimlik ile SSMS gibi araçları kullanarak veri kopyalamak istediğiniz bağlanmak ALTER herhangi bir kullanıcı izni. Aşağıdaki T-SQL çalıştırın:
    
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

4. **Hizmet sorumlusu gerekli izinleri vermek** SQL kullanıcılar veya başkaları için normalde yaptığınız gibi. Aşağıdaki kodu çalıştırın:

    ```sql
    EXEC sp_addrolemember [role name], [your application name];
    ```

5. **Bir Azure SQL Data Warehouse bağlantılı hizmeti yapılandırma** Azure veri fabrikası'nda.


#### <a name="linked-service-example-that-uses-service-principal-authentication"></a>Hizmet asıl kimlik doğrulaması kullanan bağlantılı hizmet örneği

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

### <a name="managed-service-identity-authentication"></a>Yönetilen hizmet kimlik doğrulama

Data factory ile ilişkili bir [yönetilen hizmet kimliği](data-factory-service-identity.md) , belirli üretecini temsil eder. Bu hizmet kimliği Azure SQL Data Warehouse kimlik doğrulaması için kullanabilirsiniz. Bu kimliği kullanarak veya bu verilerinizi kopya veri ambarı ve belirlenen Fabrika erişebilirsiniz.

> [!IMPORTANT]
> PolyBase MSI kimlik doğrulaması için şu anda desteklenmediğini unutmayın.

MSI tabanlı Azure AD uygulama belirteci kimlik doğrulaması kullanmak için aşağıdaki adımları izleyin:

1. **Azure AD'de bir grup oluşturun.** MSI Fabrika grubunun bir üyesi yapın.

    a. Veri Fabrikası hizmet kimliği Azure portalından bulun. Veri fabrikasının gidin **özellikleri**. Hizmet kimlik kimliği kopyalayın.

    b. Yükleme [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) modülü. Kullanarak oturum `Connect-AzureAD` komutu. Bir grup oluşturun ve veri fabrikası MSI üyesi olarak eklemek için aşağıdaki komutları çalıştırın.
    ```powershell
    $Group = New-AzureADGroup -DisplayName "<your group name>" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId "<your data factory service identity ID>"
    ```

2. **[Azure Active Directory yönetici sağlamak](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)**  zaten yapmadıysanız Azure Portal'da Azure SQL sunucunuzun.

3. **[Kapsanan veritabanı kullanıcıları oluşturun](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)**  Azure AD grubu için. Veri ambarı veya en az bir Azure AD kimlik ile SSMS gibi araçları kullanarak veri kopyalamak istediğiniz bağlanmak ALTER herhangi bir kullanıcı izni. Aşağıdaki T-SQL çalıştırın. 
    
    ```sql
    CREATE USER [your Azure AD group name] FROM EXTERNAL PROVIDER;
    ```

4. **Azure AD grubundaki gerekli izinleri vermek** SQL kullanıcılar ve başkaları için normalde yaptığınız gibi. Örneğin, aşağıdaki kodu çalıştırın.

    ```sql
    EXEC sp_addrolemember [role name], [your Azure AD group name];
    ```

5. **Bir Azure SQL Data Warehouse bağlantılı hizmeti yapılandırma** Azure veri fabrikası'nda.

#### <a name="linked-service-example-that-uses-msi-authentication"></a>MSI kimlik doğrulaması kullanan bağlantılı hizmet örneği

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

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [veri kümeleri](https://docs.microsoft.com/en-us/azure/data-factory/concepts-datasets-linked-services) makalesi. Bu bölümde Azure SQL veri ambarı veri kümesi tarafından desteklenen özellikler listesini sağlar.

Gelen veya Azure SQL Data Warehouse veri kopyalamak için ayarlanmış **türü** dataset özelliğinin **AzureSqlDWTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** dataset özelliğinin ayarlanması gerekir **AzureSqlDWTable**. | Evet |
| tableName | Tablo veya Görünüm başvuran bağlı hizmetin Azure SQL Data Warehouse örneğinde adı. | Evet |

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

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde Azure SQL Data Warehouse kaynak ve havuz tarafından desteklenen özellikler listesini sağlar.

### <a name="azure-sql-data-warehouse-as-the-source"></a>Kaynak olarak Azure SQL veri ambarı

Azure SQL veri ambarından veri kopyalamak için ayarlanmış **türü** kopyalama etkinliği kaynağına özelliğinde **SqlDWSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kopyalama etkinliği kaynak özelliği ayarlanmalıdır **SqlDWSource**. | Evet |
| sqlReaderQuery | Verileri okumak için özel SQL sorgusu kullanın. Örnek: `select * from MyTable`. | Hayır |
| sqlReaderStoredProcedureName | Kaynak tablodan veri okuyan saklı yordamın adı. Son SQL deyimi SELECT deyimi içinde saklı yordamı olması gerekir. | Hayır |
| storedProcedureParameters | Saklı yordam parametreleri.<br/>İzin verilen değerler ad veya değer çiftleridir. Adları ve büyük/küçük harf parametrelerinin adlarını ve saklı yordam parametreleri büyük/küçük harf eşleşmelidir. | Hayır |

### <a name="points-to-note"></a>Dikkat edilecek noktaları

- Varsa **sqlReaderQuery** için belirtilen **SqlSource**, kopyalama etkinliği bu sorguyu veri almak için Azure SQL Data Warehouse kaynağına karşı çalışır. Veya bir saklı yordam belirtebilirsiniz. Belirtin **sqlReaderStoredProcedureName** ve **storedProcedureParameters** saklı yordamın kullandığı parametreler varsa.
- Ya da belirtmezseniz **sqlReaderQuery** veya **sqlReaderStoredProcedureName**, tanımlanan sütunları **yapısı** bölüm JSON veri kümesi için kullanılan bir sorgu oluşturun. `select column1, column2 from mytable` Azure SQL veri ambarına karşı çalışır. Veri kümesi tanımı yoksa **yapısı**, tüm sütunlar tablosundan seçilir.
- Kullandığınızda **sqlReaderStoredProcedureName**, yine de bir kukla belirtmeniz gerekiyorsa **tableName** JSON veri kümesi bir özellik.

#### <a name="sql-query-example"></a>SQL sorgu örneği

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

### <a name="azure-sql-data-warehouse-as-sink"></a> Havuz olarak Azure SQL veri ambarı

Azure SQL Data Warehouse'a veri kopyalamak için kopyalama etkinliği için Havuz türü ayarlayın. **SqlDWSink**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kopyalama etkinliği havuz özelliği ayarlanmalıdır **SqlDWSink**. | Evet |
| Bulunan'allowpolybase | PolyBase, uygun olduğunda, yerine BULKINSERT mekanizması kullanılıp kullanılmayacağını belirtir. <br/><br/> PolyBase kullanarak SQL Data Warehouse'a veri yükleme öneririz. Bkz [kullanım Azure SQL Data Warehouse'a veri yüklemek için PolyBase](#use-polybase-to-load-data-into-azure-sql-data-warehouse) kısıtlamaları ve ayrıntıları bölümü.<br/><br/>İzin verilen değerler **True** ve **False** (varsayılan).  | Hayır |
| polyBaseSettings | Bir grup olabilir özellik belirtilen **Bulunan'allowpolybase** özelliği ayarlanmış **doğru**. | Hayır |
| rejectValue | Sayı veya yüzde değeri sorgu başarısız önce reddedilemiyor satır belirtir.<br/><br/>Bağımsız değişkenler bölümünde PolyBase'nın reddetme seçenekleri hakkında daha fazla bilgi [CREATE dış TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx). <br/><br/>0 (varsayılan), 1, 2, izin verilen değerler vs. |Hayır |
| rejectType | Belirtir olup olmadığını **rejectValue** seçenektir değişmez değer veya bir yüzde.<br/><br/>İzin verilen değerler **değeri** (varsayılan) ve **yüzde**. | Hayır |
| Havuzu'na ilişkin | PolyBase reddedilen satırları yüzdesini yeniden hesaplar önce almak için satır sayısını belirler.<br/><br/>İzin verilen değerler: 1, 2, vs. | Evet, varsa **rejectType** olan **yüzde**. |
| useTypeDefault | PolyBase metin dosyasından veri aldığında sınırlandırılmış metin dosyaları eksik değerleri nasıl ele alınacağını belirtir.<br/><br/>Bağımsız değişkenler bölümünde bu özelliği hakkında daha fazla bilgi [oluşturma EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).<br/><br/>İzin verilen değerler **True** ve **False** (varsayılan). | Hayır |
| writeBatchSize | Arabellek boyutu ulaştığında veri SQL tablosuna ekler **writeBatchSize**. Yalnızca PolyBase kullanıldığında değilse geçerlidir.<br/><br/>İzin verilen değer **tamsayı** (satır sayısı). | Hayır. 10000 varsayılandır. |
| writeBatchTimeout | Toplu ekleme işlemi zaman aşımına uğramadan önce tamamlamak bir süre bekleyin. Yalnızca PolyBase kullanıldığında değilse geçerlidir.<br/><br/>İzin verilen değer **timespan**. Örnek: "00: 30:00" (30 dakika). | Hayır |
| preCopyScript | Her çalıştırma sırasında Azure SQL Data Warehouse'a veri yazmadan önce çalıştırmak kopyalama etkinliği için bir SQL sorgusunu belirtin. Önceden yüklenen veriyi temizlemek için bu özelliği kullanın. | Hayır | (#repeatability sırasında-kopyalama). | Sorgu bildirimi. | Hayır |

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

SQL veri ambarı sonraki bölümde verimli bir şekilde yüklemek için PolyBase kullanma hakkında daha fazla bilgi edinin.

## <a name="use-polybase-to-load-data-into-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse'a veri yüklemek için Polybase'i kullanın

Kullanarak [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) büyük miktarda veri yüksek işleme ile Azure SQL Data Warehouse'a veri yüklemek için etkili bir yoldur. PolyBase yerine varsayılan BULKINSERT mekanizmasını kullanarak iş çıkarma büyük kazanç görürsünüz. Bkz: [Performans başvurusu](copy-activity-performance.md#performance-reference) ayrıntılı bir karşılaştırması için. Kullanım örneği ile bir anlatım için bkz: [1 TB Azure SQL Data Warehouse'a veri yükleme](https://docs.microsoft.com/en-us/azure/data-factory/v1/data-factory-load-sql-data-warehouse).

* Kaynak verileriniz Azure Blob storage veya Azure Data Lake Store hem biçiminde ise PolyBase, PolyBase kullanarak kopyalama doğrudan Azure SQL veri ambarı ile uyumludur. Ayrıntılar için bkz  **[doğrudan kopyalama PolyBase kullanarak](#direct-copy-by-using-polybase)**.
* Kaynak veri deposu ve biçim başlangıçta desteklenmez, PolyBase tarafından kullanın **[kopyalama PolyBase kullanarak hazırlanmış](#staged-copy-by-using-polybase)** yerine özelliği. Hazırlanan kopyalama özelliğini de daha iyi verimlilik sağlar. Otomatik olarak veri PolyBase uyumlu biçime dönüştürür. Ve verileri Azure Blob Depolama alanında depolar. Ardından verileri SQL Data Warehouse'a veri yükler.

> [!IMPORTANT]
> PolyBase MSI tabanlı Azure AD uygulama belirteci kimlik doğrulaması için şu anda desteklenmediğini unutmayın.

### <a name="direct-copy-by-using-polybase"></a>PolyBase kullanarak doğrudan Kopyala

SQL veri ambarı PolyBase, Azure Blob ve Azure Data Lake Store doğrudan destekler. Bir kaynak olarak hizmet sorumlusu kullanır ve belirli dosya biçimi gereksinimleri vardır. Veri kaynağınızı Bu bölümde açıklanan ölçütlerini karşılıyorsa, doğrudan Azure SQL Data Warehouse için kaynak veri deposundan kopyalamak için Polybase'i kullanın. Aksi takdirde kullanın [kopyalama PolyBase kullanarak hazırlanmış](#staged-copy-by-using-polybase).

> [!TIP]
> Verileri SQL Data Warehouse için verimli bir şekilde Data Lake Deposu'ndan veri kopyalamak için'dan daha fazla bilgi edinin [Azure Data Factory sağlar, bu, daha kolay ve Data Lake Store ile SQL veri ambarı kullanırken verilerden Öngörüler ortaya çıkarmak uygun bile](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/).

Gereksinimleri karşılanmadığı takdirde Azure Data Factory ayarları denetler ve veri taşıma için BULKINSERT mekanizması için otomatik olarak geri döner.

1. **Kaynak bağlantılı hizmeti** türü **AzureStorage** veya **AzureDataLakeStore** hizmet asıl kimlik doğrulamasına sahip.
2. **Girdi veri kümesi** türü **AzureBlob** veya **AzureDataLakeStoreFile**. Biçim türü'nün altında `type` özellikleri **OrcFormat**, **ParquetFormat**, veya **TextFormat**, aşağıdaki yapılandırmaları ile:

   1. `rowDelimiter` olmalıdır **\n**.
   2. `nullValue` ayarlanmış **boş dize** (""), veya `treatEmptyAsNull` ayarlanır **doğru**.
   3. `encodingName` ayarlanmış **utf-8**, varsayılan değer olan.
   4. `escapeChar`, `quoteChar`, `firstRowAsHeader`, ve `skipLineCount` belirtilen değil.
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
4. Yoktur hiçbir `sliceIdentifierColumnName` altında ayarı **SqlDWSink** işlem hattının kopyalama etkinliği için. PolyBase tüm verileri güncelleştirilir veya hiçbir şey bir tek çalıştırmada güncelleştirildiğinde güvence altına alır. Elde etmek için **Yinelenebilirlik**, kullanmak `sqlWriterCleanupScript`.

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

Veri kaynağınızı önceki bölümdeki ölçütlerle eşleşmeyen verileri geçici hazırlama Azure Blob Depolama örneği kopyalama etkinleştirin. Azure Premium Storage olamaz. Bu durumda, Azure Data Factory PolyBase veri biçimi gereksinimlerini karşılamak için veri dönüşümleri otomatik olarak çalıştırılır. Ardından SQL Data Warehouse'a veri yüklemek için PolyBase kullanır. Son olarak, blob depolama biriminden, geçici verileri temizler. Bkz: [kopyalama hazırlanan](copy-activity-performance.md#staged-copy) bir hazırlama Azure Blob Depolama örneği aracılığıyla veri kopyalama hakkında ayrıntılar için.

Bu özelliği kullanmak için oluşturma bir [Azure depolama bağlantılı hizmeti](connector-azure-blob-storage.md#linked-service-properties) Ara blob storage ile Azure depolama hesabı için başvuruyor. Ardından belirtin `enableStaging` ve `stagingSettings` kopyalama aşağıdaki kodda gösterildiği gibi etkinliği özellikleri:

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

## <a name="best-practices-for-using-polybase"></a>Polybase'i kullanarak için en iyi uygulamalar

Aşağıdaki bölümlerde belirtilen listelenenlere en iyi yöntemleri sağlamak [en iyi uygulamalar Azure SQL Data Warehouse için](../sql-data-warehouse/sql-data-warehouse-best-practices.md).

### <a name="required-database-permission"></a>Gerekli veritabanı izni

PolyBase kullanmak için SQL Data Warehouse'a veri yükleyen kullanıcının olmalıdır ["Denetim" izni](https://msdn.microsoft.com/library/ms191291.aspx) hedef veritabanı üzerinde. Elde etmenin bir yolu olan bir üye olarak kullanıcı eklemek için **db_owner** rol. Bunu yapmak öğrenin [SQL Data Warehouse genel bakış](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization).

### <a name="row-size-and-data-type-limits"></a>Satır boyutu ve veri sınırları yazın

PolyBase yükleri satır 1 MB'den küçük sınırlıdır. Bunlar VARCHR(MAX), NVARCHAR(MAX) veya VARBINARY(MAX) yüklenemiyor. Daha fazla bilgi için bkz: [SQL veri ambarı hizmet kapasite sınırları](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).

Veri kaynağınızı 1 MB'tan fazla satır olduğunda, dikey olarak kaynak tabloları birkaç küçük parçalara bölmek isteyebilirsiniz. Her satırın en büyük boyut sınırı aşmadığından emin olun. Daha küçük tabloları PolyBase kullanılarak yüklenen ve Azure SQL Data Warehouse'da birlikte birleştirilir.

### <a name="sql-data-warehouse-resource-class"></a>SQL veri ambarı kaynak sınıfı

Olası en iyi performans elde etmek için verileri SQL Data Warehouse PolyBase aracılığıyla yükler kullanıcı daha büyük bir kaynak sınıfı atayın.

### <a name="tablename-in-azure-sql-data-warehouse"></a>**tableName** Azure SQL veri ambarı

Aşağıdaki tabloda nasıl belirleneceğini örnekleri verir **tableName** JSON veri kümesi bir özellik. Şema ve tablo adlarının birkaç birleşimlerini gösterir.

| DB şeması | Tablo adı | **tableName** JSON özelliği |
| --- | --- | --- |
| dbo | MyTable | MyTable ya da dbo. MyTable ya da [dbo]. [MyTable] |
| dbo1 | MyTable | dbo1. MyTable veya [dbo1]. [MyTable] |
| dbo | My.Table | [My.Table] veya [dbo]. [My.Table] |
| dbo1 | My.Table | [dbo1]. [My.Table] |

Aşağıdaki hatayla karşılaşırsanız, sorun için belirtilen değer olabilir **tableName** özelliği. Değerlerini belirtmek doğru bir şekilde için önceki tabloya bakın **tableName** JSON özelliği.

```
Type=System.Data.SqlClient.SqlException,Message=Invalid object name 'stg.Account_test'.,Source=.Net SqlClient Data Provider
```

### <a name="columns-with-default-values"></a>Varsayılan değerleri içeren sütun

Şu anda, veri fabrikası'nda PolyBase özelliği yalnızca aynı sayıda sütun hedef tabloda olduğu gibi kabul eder. Bunlardan birini varsayılan bir değerle tanımlandığı dört sütun içeren bir tablo örneğidir. Giriş verisi hala dört sütun olmalıdır. Üç sütun girdi veri kümesi aşağıdaki iletiye benzer bir hata verir:

```
All columns of the table must be specified in the INSERT BULK statement.
```

NULL değeri, varsayılan değeri özel bir biçimidir. Sütunu null atanabilir ise, bu sütun için blob girdi verileri boş olabilir. Ancak giriş kümesinden eksik olamaz. PolyBase, Azure SQL Data Warehouse eksik değerleri NULL ekler.

## <a name="data-type-mapping-for-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse için veri türü eşlemesi

Gelen veya Azure SQL Data Warehouse veri kopyaladığınızda, aşağıdaki eşlemelerini Azure SQL Data Warehouse veri türlerinden Azure Data Factory geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) kopyalama etkinliği kaynak şema ve veri türü için havuz nasıl eşlendiğini öğrenin.

| Azure SQL veri ambarı veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| bigint | Int64 |
| İkili | Byte] |
| bit | Boole |
| char | Dize, Char] |
| tarih | DateTime |
| Tarih saat | DateTime |
| datetime2 | DateTime |
| Datetimeoffset | DateTimeOffset |
| Ondalık | Ondalık |
| FILESTREAM özniteliği (varbinary(max)) | Byte] |
| Kayan | çift |
| image | Byte] |
| int | Int32 |
| para | Ondalık |
| nchar | Dize, Char] |
| ntext | Dize, Char] |
| sayısal | Ondalık |
| nvarchar | Dize, Char] |
| Gerçek | Tek |
| rowVersion | Byte] |
| smalldatetime | DateTime |
| tamsayı | Int16 |
| küçük para | Ondalık |
| sql_variant | Nesne * |
| metin | Dize, Char] |
| time | TimeSpan |
| timestamp | Byte] |
| Mini tamsayı | Bayt |
| benzersiz tanımlayıcı | Guid |
| varbinary | Byte] |
| varchar | Dize, Char] |
| xml | Xml |

## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları ve biçimleri](copy-activity-overview.md##supported-data-stores-and-formats).
