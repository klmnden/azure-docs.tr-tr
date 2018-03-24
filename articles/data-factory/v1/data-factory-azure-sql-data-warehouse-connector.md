---
title: Veri kopyalama/Azure SQL veri ambarından | Microsoft Docs
description: Azure Data Factory kullanarak Azure SQL Data Warehouse öğesine/öğesinden veri kopyalama öğrenin
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: d90fa9bd-4b79-458a-8d40-e896835cfd4a
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 709a178d99a34adb9c77086e55270fe41ed84551
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="copy-data-to-and-from-azure-sql-data-warehouse-using-azure-data-factory"></a>İçin ve Azure Data Factory kullanarak Azure SQL veri ambarından veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](data-factory-azure-sql-data-warehouse-connector.md)
> * [Sürüm 2 - Önizleme](../connector-azure-sql-data-warehouse.md)

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [V2 Azure SQL Data Warehouse Bağlayıcısı](../connector-azure-sql-data-warehouse.md).

Bu makalede kopya etkinliği Azure Data Factory'de Azure SQL Data Warehouse grafikten veri taşımak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) kopyalama etkinliği ile veri taşıma için genel bir bakış sunar makalesi.  

> [!TIP]
> En iyi performansı elde etmek için Azure SQL Data Warehouse'a veri yüklemek için PolyBase kullanın. [Kullanım Azure SQL Data Warehouse'a veri yüklemek için PolyBase](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) bölümde ayrıntılar bulunur. Kullanım örneği ile bir anlatım için bkz: [1 TB altında 15 dakika Azure Data Factory ile Azure SQL Data Warehouse'a veri yükleme](data-factory-load-sql-data-warehouse.md).

## <a name="supported-scenarios"></a>Desteklenen senaryolar
Veri kopyalama **Azure SQL veri ambarından** aşağıdaki veri depolar:

[!INCLUDE [data-factory-supported-sinks](../../../includes/data-factory-supported-sinks.md)]

Aşağıdaki veri depolarına verileri kopyalayabilirsiniz **Azure SQL Data Warehouse için**:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

> [!TIP]
> Tablo hedef deposunda mevcut değilse veri SQL Server veya Azure SQL veritabanından Azure SQL Data Warehouse için kopyalarken, veri fabrikası otomatik olarak tablo SQL veri ambarı'nda kaynak veri deposunda tablonun şeması kullanarak oluşturabilirsiniz. Bkz: [Otomatik Tablo oluşturma](#auto-table-creation) Ayrıntılar için.

## <a name="supported-authentication-type"></a>Desteklenen kimlik doğrulama türü
Azure SQL Data Warehouse Bağlayıcısı destek temel kimlik doğrulaması.

## <a name="getting-started"></a>Başlarken
Verileri farklı araçlar/API'lerini kullanarak Azure SQL Data Warehouse denetleyicisinden taşır kopyalama etkinliği ile işlem hattı oluşturun.

Öğesine/öğesinden Azure SQL veri ambarı verileri kopyalayan bir işlem hattı oluşturmak için en kolay yolu, veri kopyalama Sihirbazı'nı kullanmaktır. Bkz: [öğretici: Data Factory ile SQL veri ambarına veri yükleme](../../sql-data-warehouse/sql-data-warehouse-load-with-data-factory.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma Hızlı Kılavuz.

Bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**, ve **REST API**. Bkz: [kopyalama etkinliği öğretici](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

Araçlar ya da API'leri kullanıp bir havuz veri deposu için bir kaynak veri deposundan verileri taşır bir ardışık düzen oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma bir **veri fabrikası**. Veri Fabrikası bir veya daha fazla ardışık düzen içerebilir. 
2. Oluşturma **bağlantılı Hizmetleri** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar. Örneğin, verileri Azure blob depolama alanından Azure SQL veri ambarı'na kopyalıyorsanız Azure depolama hesabı ve Azure SQL veri ambarı veri fabrikanıza bağlamak için iki bağlı hizmet oluşturun. Azure SQL Data Warehouse için özel bağlantılı hizmet özellikleri için bkz: [bağlantılı hizmet özellikleri](#linked-service-properties) bölümü. 
3. Oluşturma **veri kümeleri** kopyalama işlemi için girdi ve çıktı verilerini temsil etmek için. Son adımda bahsedilen örnekte blob kapsayıcısı ve giriş verilerini içeren klasörü belirtmek için bir veri kümesi oluşturun. Ve blob depolama biriminden kopyalanan verilerini tutan Azure SQL data warehouse'da tablo belirtmek için başka bir veri kümesi oluşturun. Azure SQL Data Warehouse için özel veri kümesi özellikleri için bkz: [veri kümesi özellikleri](#dataset-properties) bölümü.
4. Oluşturma bir **ardışık düzen** bir giriş olarak bir veri kümesi ve bir veri kümesini çıktı olarak alan kopyalama etkinliği ile. Daha önce bahsedilen örnekte BlobSource bir kaynak ve SqlDWSink havuzu olarak kopya etkinliği için kullanırsınız. Azure Blob depolama alanına Azure SQL veri ambarından kopyalıyorsanız benzer şekilde, SqlDWSource ve BlobSink kopyalama etkinliği kullanırsınız. Azure SQL Data Warehouse için belirli kopyalama etkinliği özellikleri için bkz: [kopyalama etkinliği özellikleri](#copy-activity-properties) bölümü. Bir veri deposu bir kaynak veya bir havuz nasıl kullanılacağı hakkında daha fazla bilgi için önceki bölümde, veri deposu için bağlantıya tıklayın.

Sihirbazı'nı kullandığınızda, bu Data Factory varlıkları (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, JSON biçimini kullanarak bu Data Factory varlıklarını tanımlayın.  / Azure SQL Data Warehouse veri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımlarıyla örnekleri için bkz: [JSON örnekler](#json-examples-for-copying-data-to-and-from-sql-data-warehouse) bu makalenin.

Aşağıdaki bölümler, Azure SQL Data Warehouse için Data Factory varlıklarını belirli tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Aşağıdaki tabloda, Azure SQL Data Warehouse bağlantılı hizmete özgü JSON öğelerini açıklamasını sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalıdır: **AzureSqlDW** |Evet |
| connectionString |ConnectionString özelliği için Azure SQL Data Warehouse örneğine bağlanmak için gereken bilgileri belirtin. Yalnızca temel kimlik doğrulama desteklenir. |Evet |

> [!IMPORTANT]
> Yapılandırma [Azure SQL veritabanı Güvenlik Duvarı](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) ve veritabanı sunucusuna [Azure hizmetlerinin sunucuya erişimine izin](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure). Ayrıca, dış Azure veri fabrikası ağ geçidi ile şirket içi veri kaynaklarından dahil olmak üzere Azure SQL Data Warehouse'a veri kopyalıyorsanız, verileri Azure SQL Data Warehouse için gönderme makine için uygun IP adresi aralığı yapılandırın.

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümler & özellikleri veri kümeleri tanımlamak için kullanılabilir tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler yapısı, kullanılabilirlik ve bir veri kümesi JSON İlkesi gibi tüm veri türleri (Azure SQL, Azure blob, Azure tablo, vs.) için benzer.

TypeProperties bölümü dataset her tür için farklıdır ve verilerin veri deposunda konumu hakkında bilgi sağlar. **TypeProperties** veri kümesi için bir bölüm türü **AzureSqlDWTable** aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Tablo veya Görünüm başvuran bağlı hizmetin Azure SQL veri ambarı veritabanındaki adı. |Evet |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümler & özellikleri etkinlikleri tanımlamak için kullanılabilir tam listesi için bkz: [oluşturma ardışık düzen](data-factory-create-pipelines.md) makalesi. Ad, açıklama, giriş ve çıkış tabloları ve ilke gibi özellikler etkinlikleri tüm türleri için kullanılabilir.

> [!NOTE]
> Kopyalama etkinliği yalnızca bir girdi alır ve tek bir çıktı üretir.

Oysa etkinliğin typeProperties bölümündeki özellikler her etkinlik türü ile farklılık gösterir. Kopya etkinliği için bunlar türlerini kaynakları ve havuzlarını bağlı olarak farklılık gösterir.

### <a name="sqldwsource"></a>SqlDWSource
Kaynak türü olduğunda **SqlDWSource**, aşağıdaki özellikler mevcuttur **typeProperties** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sqlReaderQuery |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin: seçin * from MyTable. |Hayır |
| sqlReaderStoredProcedureName |Kaynak tablodan veri okuyan saklı yordamın adı. |Saklı yordam adı. Son SQL deyimi SELECT deyimi içinde saklı yordamı olması gerekir. |Hayır |
| storedProcedureParameters |Saklı yordam parametreleri. |Ad/değer çiftleri. Adları ve büyük/küçük harf parametrelerinin adlarını ve saklı yordam parametreleri büyük/küçük harf eşleşmelidir. |Hayır |

Varsa **sqlReaderQuery** belirtilen SqlDWSource için kopyalama etkinliği veri almak için Azure SQL Data Warehouse kaynağında bu sorguyu çalıştırır.

Alternatif olarak, bir saklı yordam belirterek belirleyebileceğiniz **sqlReaderStoredProcedureName** ve **storedProcedureParameters** (saklı yordam parametreleri alıyorsa).

SqlReaderQuery veya sqlReaderStoredProcedureName belirtmezseniz JSON veri kümesi yapısı bölümünde tanımlanan sütunları Azure SQL veri ambarına karşı çalıştırmak için bir sorgu oluşturmak için kullanılır. Örnek: `select column1, column2 from mytable`. Veri kümesi tanımı yapısına sahip değilse, tüm sütunları tablodan seçilir.

#### <a name="sqldwsource-example"></a>SqlDWSource örneği

```JSON
"source": {
    "type": "SqlDWSource",
    "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
    "storedProcedureParameters": {
        "stringData": { "value": "str3" },
        "identifier": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
    }
}
```
**Saklı yordam tanımı:**

```SQL
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

### <a name="sqldwsink"></a>SqlDWSink
**SqlDWSink** aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sqlWriterCleanupScript |Belirli bir dilimle verilerinin temizlenmesini şekilde yürütmek kopyalama etkinliği için bir sorgu belirtin. Ayrıntılar için bkz [Yinelenebilirlik bölüm](#repeatability-during-copy). |Sorgu bildirimi. |Hayır |
| allowPolyBase |BULKINSERT mekanizması yerine PolyBase (varsa) kullanılıp kullanılmayacağını belirtir. <br/><br/> **PolyBase kullanarak SQL Data Warehouse'a veri yükleme için önerilen yoldur.** Bkz [kullanım Azure SQL Data Warehouse'a veri yüklemek için PolyBase](#use-polybase-to-load-data-into-azure-sql-data-warehouse) kısıtlamaları ve ayrıntıları bölümü. |True <br/>False (varsayılan) |Hayır |
| polyBaseSettings |Bir grup olabilir özellik belirtilen **Bulunan'allowpolybase** özelliği ayarlanmış **doğru**. |&nbsp; |Hayır |
| rejectValue |Sayı veya yüzde değeri sorgu başarısız önce reddedilemiyor satır belirtir. <br/><br/>PolyBase'nın reddetme seçenekleri hakkında daha fazla bilgi **bağımsız değişkenleri** bölümünü [CREATE dış TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) konu. |0 (varsayılan), 1, 2... |Hayır |
| rejectType |RejectValue seçeneği bir hazır değer veya bir yüzde belirtilen belirtir. |Değer (varsayılan), yüzde |Hayır |
| rejectSampleValue |PolyBase reddedilen satırları yüzdesini yeniden hesaplar önce almak için satır sayısını belirler. |1, 2, … |Evet, varsa **rejectType** olan **yüzdesi** |
| useTypeDefault |PolyBase metin dosyasından veri aldığında sınırlandırılmış metin dosyaları eksik değerleri nasıl ele alınacağını belirtir.<br/><br/>Bağımsız değişkenler bölümünde bu özelliği hakkında daha fazla bilgi [oluşturma EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx). |TRUE, False (varsayılan) |Hayır |
| writeBatchSize |Arabellek boyutu writeBatchSize ulaştığında veri SQL tablosuna ekler. |Tamsayı (satır sayısı) |Hayır (varsayılan: 10000) |
| writeBatchTimeout |Toplu ekleme işlemi zaman aşımına uğramadan önce tamamlamak bir süre bekleyin. |TimeSpan<br/><br/> Örnek: "00: 30:00" (30 dakika). |Hayır |

#### <a name="sqldwsink-example"></a>SqlDWSink örneği

```JSON
"sink": {
    "type": "SqlDWSink",
    "allowPolyBase": true
}
```

## <a name="use-polybase-to-load-data-into-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse'a veri yüklemek için Polybase'i kullanın
Kullanarak **[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide)** büyük miktarda veri yüksek işleme ile Azure SQL Data Warehouse'a veri yükleme etkili bir yoldur. PolyBase yerine varsayılan BULKINSERT mekanizmasını kullanarak büyük kazanç verimliliği de görebilirsiniz. Bkz: [kopyalama performans başvuru numarası](data-factory-copy-activity-performance.md#performance-reference) ayrıntılı karşılaştırması ile. Kullanım örneği ile bir anlatım için bkz: [1 TB altında 15 dakika Azure Data Factory ile Azure SQL Data Warehouse'a veri yükleme](data-factory-load-sql-data-warehouse.md).

* Veri kaynağınızı ise **Azure Blob veya Azure Data Lake Store**ve biçimini PolyBase ile uyumlu ise, doğrudan Azure SQL veri Polybase'i kullanarak ambarına kopyalayabilirsiniz. Bkz: **[Polybase'i kullanarak doğrudan kopyalama](#direct-copy-using-polybase)** ayrıntılarla.
* Kaynak veri deposu ve biçim başlangıçta desteklenmiyor, PolyBase tarafından kullanabileceğiniz **[Polybase'i kullanarak kopyalama hazırlanan](#staged-copy-using-polybase)** yerine özellik. Ayrıca, daha iyi verim otomatik olarak veri PolyBase uyumlu biçimine dönüştürmek için kullanılan ve Azure Blob depolama alanına veri depolayarak sağlar. Ardından verileri SQL Data Warehouse'a veri yükler.

Ayarlama `allowPolyBase` özelliğine **true** Azure SQL Data Warehouse'a veri kopyalamak için PolyBase kullanmak Azure Data Factory için aşağıdaki örnekte gösterildiği gibi. Bulunan'allowpolybase true olarak ayarladığınızda, kullanarak PolyBase belirli özelliklerini belirtebilirsiniz `polyBaseSettings` özellik grubu. bkz: [SqlDWSink](#SqlDWSink) polyBaseSettings ile kullanabileceğiniz özellikleri hakkında ayrıntılı bilgi için bölüm.

```JSON
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

### <a name="direct-copy-using-polybase"></a>Polybase'i kullanarak doğrudan kopyalama
SQL veri ambarı PolyBase doğrudan desteği Azure Blob ve Azure Data Lake Store (hizmet sorumlusu kullanarak) kaynak olarak ve belirli dosya biçimi gereksinimleriyle. Veri kaynağınızı Bu bölümde açıklanan ölçütlerini karşılıyorsa, kaynak veri deposundan doğrudan Azure SQL veri Polybase'i kullanarak ambarına kopyalayabilirsiniz. Aksi takdirde, kullanabileceğiniz [Polybase'i kullanarak kopyalama hazırlanan](#staged-copy-using-polybase).

> [!TIP]
> Verimli bir şekilde SQL veri ambarı için verileri Data Lake Deposu'ndan veri kopyalamak için'dan daha fazla bilgi edinin [Azure Data Factory sağlar, bu, daha kolay ve Data Lake Store ile SQL veri ambarı kullanırken verilerden Öngörüler ortaya çıkarmak uygun bile](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/).

Gereksinimler karşılanmazsa, Azure Data Factory ayarları denetler ve veri taşıma için BULKINSERT mekanizması için otomatik olarak geri döner.

1. **Kaynak bağlantılı hizmeti** türüdür: **AzureStorage** veya **AzureDataLakeStore hizmet asıl kimlik doğrulaması ile**.  
2. **Girdi veri kümesi** türüdür: **AzureBlob** veya **AzureDataLakeStore**ve altında yazın biçimi `type` özellikleri **OrcFormat**, **ParquetFormat**, veya **TextFormat** aşağıdaki yapılandırmalara sahip:

   1. `rowDelimiter` olmalıdır **\n**.
   2. `nullValue` ayarlanmış **boş dize** (""), veya `treatEmptyAsNull` ayarlanır **doğru**.
   3. `encodingName` ayarlanmış **utf-8**, olduğu **varsayılan** değeri.
   4. `escapeChar`, `quoteChar`, `firstRowAsHeader`, ve `skipLineCount` belirtilmedi.
   5. `compression` olabilir **sıkıştırma yok**, **GZip**, veya **Deflate**.

    ```JSON
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
5. Yoktur hiçbir `columnMapping` ilişkili kopya kullanılan etkinlik.

### <a name="staged-copy-using-polybase"></a>Polybase'i kullanarak hazırlanmış kopyalama
Veri kaynağınızı önceki bölümde sunulan ölçütlere uyan değil, bir geçici hazırlama Azure Blob (Premium depolama olamaz) depolama aracılığıyla veri kopyalamayı etkinleştirebilirsiniz. Bu durumda, Azure Data Factory otomatik olarak dönüşümleri PolyBase veri biçimi gereksinimlerini karşılamak ve ardından verileri SQL veri ambarında ve son temizlemenin yüklemek için Polybase'i kullanın veri, geçici veriler Blob depolama alanından gerçekleştirir. Bkz: [hazırlanan kopyalama](data-factory-copy-activity-performance.md#staged-copy) nasıl hazırlama Azure Blob üzerinden veri kopyalama genel çalıştığı hakkında bilgi.

> [!NOTE]
> Ne zaman Azure SQL Data Polybase'i kullanarak Warehouse'a bir şirket içi veri kopyalama verileri depolamak ve hazırlama, veri yönetimi ağ geçidi sürümü 2.4 ise, JRE (Java Çalışma zamanı ortamı) ağ geçidi makinenizde veri kaynağınızı dönüştürmek için kullanılan gereklidir doğru biçime. Bu bağımlılık önlemek için ağ geçidinizi en son yükseltmeniz önerilir.
>

Bu özelliği kullanmak için oluşturma bir [Azure depolama bağlantılı hizmeti](data-factory-azure-blob-connector.md#azure-storage-linked-service) Ara blob depolama alanına sahip Azure depolama hesabı anlamına gelir, sonra belirtin `enableStaging` ve `stagingSettings` kopyalama gösterildiği gibi etkinliğin özellikleri Aşağıdaki kod:

```json
"activities":[  
{
    "name": "Sample copy activity from SQL Server to SQL Data Warehouse via PolyBase",
    "type": "Copy",
    "inputs": [{ "name": "OnpremisesSQLServerInput" }],
    "outputs": [{ "name": "AzureSQLDWOutput" }],
    "typeProperties": {
        "source": {
            "type": "SqlSource",
        },
        "sink": {
            "type": "SqlDwSink",
            "allowPolyBase": true
        },
        "enableStaging": true,
        "stagingSettings": {
            "linkedServiceName": "MyStagingBlob"
        }
    }
}
]
```

## <a name="best-practices-when-using-polybase"></a>PolyBase kullanırken en iyi uygulamalar
Aşağıdaki bölümlerde bölümünde belirtildiği olanlara ek en iyi yöntemleri sağlamak [en iyi uygulamalar Azure SQL Data Warehouse için](../../sql-data-warehouse/sql-data-warehouse-best-practices.md).

### <a name="required-database-permission"></a>Gerekli veritabanı izni
PolyBase kullanmak için SQL Data Warehouse'a veri yükleme için kullanılan kullanıcı gerektiriyor ["Denetim" izni](https://msdn.microsoft.com/library/ms191291.aspx) hedef veritabanı üzerinde. Elde etmenin bir yolu, "db_owner" rolünün bir üyesi o kullanıcı eklemektir. Aşağıdaki kullanarak bunu öğrenin [Bu bölümde](../../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization).

### <a name="row-size-and-data-type-limitation"></a>Satır boyutu ve verileri sınırlama yazın
Polybase yükleri sınırlı yükleme satırları hem küçük **1 MB** ve VARCHR(MAX), NVARCHAR(MAX) veya VARBINARY(MAX) yüklenemiyor. Başvurmak [burada](../../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).

Kaynak verileri satır boyutu 1 MB'tan fazla ile varsa, kaynak tabloları dikey Burada bunların her biri en büyük satır boyutu sınırı aşmadığından birkaç küçük parçalara bölmek isteyebilirsiniz. Daha küçük tabloları sonra PolyBase kullanarak yüklenebilir ve Azure SQL Data Warehouse'da birleştirmeniz.

### <a name="sql-data-warehouse-resource-class"></a>SQL veri ambarı kaynak sınıfı
Olası en iyi verime ulaşmak için PolyBase aracılığıyla SQL veri ambarında verileri yüklemek için kullanılan kullanıcı büyük kaynak sınıfı atamak için göz önünde bulundurun. Aşağıdaki kullanarak bunu öğrenin [kullanıcı kaynak sınıfı örneğini değiştirmek](../../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md).

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

## <a name="auto-table-creation"></a>Otomatik Tablo oluşturma
Azure SQL Data Warehouse için SQL Server veya Azure SQL veritabanına veri kopyalamak için kopyalama Sihirbazı'nı kullanıyorsanız ve kaynak tablosunda karşılık gelen tablosu hedef deposunda mevcut değil, veri fabrikası otomatik olarak tablo veri ambarında u tarafından oluşturabilirsiniz Kaynak tablo şemasını kaydolur.

Veri Fabrikası aynı tablo adı kaynak veri deposundaki ile hedef deposunda bir tablo oluşturur. Sütun veri türleri aşağıdaki tür eşlemesi göre seçilir. Gerekli olursa, kaynak ve hedef depoları arasında uyumsuzlukları düzeltmek için tür dönüşümleri gerçekleştirir. Ayrıca, hepsini bir kez Tablo dağıtım kullanır.

| Kaynak SQL veritabanı sütun türü | Hedef SQL DW sütun türü (boyutu kısıtlaması) |
| --- | --- |
| Int | Int |
| BigInt | BigInt |
| Tamsayı | Tamsayı |
| TinyInt | TinyInt |
| bit | bit |
| Ondalık | Ondalık |
| sayısal | Ondalık |
| Kayan nokta | Kayan nokta |
| para | para |
| Real | Real |
| Küçük para | Küçük para |
| İkili | İkili |
| varbinary | Varbinary (en fazla 8000) |
| Tarih | Tarih |
| DateTime | DateTime |
| DateTime2 | DateTime2 |
| Zaman | Zaman |
| DateTimeOffset | DateTimeOffset |
| SmallDateTime | SmallDateTime |
| Metin | VARCHAR (en fazla 8000) |
| NText | NVarChar (en fazla 4000) |
| Görüntü | VarBinary (en fazla 8000) |
| UniqueIdentifier | UniqueIdentifier |
| char | char |
| NChar | NChar |
| VarChar | VarChar (en fazla 8000) |
| NVarChar | NVarChar (en fazla 4000) |
| Xml | VARCHAR (en fazla 8000) |

[!INCLUDE [data-factory-type-repeatability-for-sql-sources](../../../includes/data-factory-type-repeatability-for-sql-sources.md)]

## <a name="type-mapping-for-azure-sql-data-warehouse"></a>Tür eşlemesi için Azure SQL veri ambarı
Bölümünde belirtildiği gibi [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makale, kopya etkinliği aşağıdaki 2 adımlı yaklaşımı türleriyle havuz için kaynak türünden otomatik tür dönüşümleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürün
2. .NET türünden yerel havuz türüne dönüştürün

& Azure SQL veri ambarından veri taşıma olduğunda, aşağıdaki eşlemelerini SQL türü .NET türü ve tersi yönde kullanılır.

Eşleme aynı [ADO.NET için SQL Server veri türü eşlemesi](https://msdn.microsoft.com/library/cc716729.aspx).

| SQL Server veritabanı altyapısı türü | .NET framework türü |
| --- | --- |
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
| Gerçek |Bekar |
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

Ayrıca havuz dataset kopyalama etkinliği tanımında sütunları kaynak kümesinden sütunları eşleyebilirsiniz. Ayrıntılar için bkz [Azure Data Factory veri kümesi sütunlarında eşleme](data-factory-map-columns.md).

## <a name="json-examples-for-copying-data-to-and-from-sql-data-warehouse"></a>JSON örnekleri ve SQL veri ambarından veri kopyalama
Aşağıdaki örnekleri kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) veya [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bunlar ve Azure SQL Data Warehouse ve Azure Blob Storage veri kopyalamak nasıl gösterir. Ancak, veriler kopyalanabilir **doğrudan** herhangi birinden herhangi birine belirtildiği havuzlarını kaynakları [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopya etkinliği Azure Data Factory kullanarak.

### <a name="example-copy-data-from-azure-sql-data-warehouse-to-azure-blob"></a>Örnek: Verilerini Azure SQL Data Warehouse Azure Blob
Örnek aşağıdaki Data Factory varlıklarını tanımlar:

1. Bağlı hizmet türü [AzureSqlDW](#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bir giriş [dataset](data-factory-create-datasets.md) türü [AzureSqlDWTable](#dataset-properties).
4. Bir çıkış [dataset](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [SqlDWSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Örnek zaman serisi (saatlik, günlük, vs.) verileri Azure SQL veri ambarı veritabanındaki bir tablodan bir blobu saatte kopyalar. Bu örnekler kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

**Azure SQL Data Warehouse bağlantılı hizmeti:**

```JSON
{
  "name": "AzureSqlDWLinkedService",
  "properties": {
    "type": "AzureSqlDW",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
**Azure Blob storage bağlı hizmeti:**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Azure SQL veri ambarı veri kümesi giriş:**

Örnek, Azure SQL Data Warehouse'da bir tablo "MyTable" oluşturulur ve zaman serisi veri için "timestampcolumn" adlı bir sütun içerdiği varsayar.

"Dış" ayarı: "true" bildirir Data Factory hizmetinin veri kümesi data factory dış ve veri fabrikasında bir etkinlik tarafından üretilen değil.

```JSON
{
  "name": "AzureSqlDWInput",
  "properties": {
    "type": "AzureSqlDWTable",
    "linkedServiceName": "AzureSqlDWLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```
**Azure Blob dataset çıktı:**

Veri her saat yeni bir bloba yazılır (sıklığı: saat, aralığı: 1). Blob klasör yolu dinamik işlenmekte olan dilim başlangıç zamanı temel alınarak değerlendirilir. Klasör yolu yıl, ay, gün ve saat bölümleri başlangıç saatini kullanır.

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

**SqlDWSource ve BlobSink ardışık düzeninde etkinlik kopyalayın:**

Ardışık Düzen giriş ve çıkış veri kümeleri kullanmak üzere yapılandırıldığı ve saatte çalışacak şekilde zamanlanır kopyalama etkinliği içerir. JSON tanımını düzenindeki **kaynak** türü ayarlanmış **SqlDWSource** ve **havuz** türü ayarlanmış **BlobSink**. SQL sorgusu için belirtilen **SqlReaderQuery** özelliği veri kopyalamak için son bir saat içindeki seçer.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "AzureSQLDWtoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureSqlDWInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlDWSource",
            "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
     ]
   }
}
```
> [!NOTE]
> Örnekte, **sqlReaderQuery** SqlDWSource için belirtilir. Kopyalama etkinliği bu sorguyu veri almak için Azure SQL Data Warehouse kaynağına karşı çalışır.
>
> Alternatif olarak, bir saklı yordam belirterek belirleyebileceğiniz **sqlReaderStoredProcedureName** ve **storedProcedureParameters** (saklı yordam parametreleri alıyorsa).
>
> SqlReaderQuery veya sqlReaderStoredProcedureName belirtmezseniz JSON veri kümesi yapısı bölümünde tanımlanan sütunları Azure SQL veri ambarına karşı çalıştırmak için bir sorgu (select column1, column2 mytable gelen) oluşturmak için kullanılır. Veri kümesi tanımı yapısına sahip değilse, tüm sütunları tablodan seçilir.
>
>

### <a name="example-copy-data-from-azure-blob-to-azure-sql-data-warehouse"></a>Örnek: verileri Azure Blob'tan Azure SQL veri ambarı'na kopyalama
Örnek aşağıdaki Data Factory varlıklarını tanımlar:

1. Bağlı hizmet türü [AzureSqlDW](#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bir giriş [dataset](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Bir çıkış [dataset](data-factory-create-datasets.md) türü [AzureSqlDWTable](#dataset-properties).
5. A [ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) ve [SqlDWSink](#copy-activity-properties).

Zaman serisi Azure SQL Data Warehouse tablosuna verilerden (saatlik, günlük, vb.) Azure blob örnek kopyaları saatte veritabanı. Bu örnekler kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

**Azure SQL Data Warehouse bağlantılı hizmeti:**

```JSON
{
  "name": "AzureSqlDWLinkedService",
  "properties": {
    "type": "AzureSqlDW",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
**Azure Blob storage bağlı hizmeti:**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Azure Blob girdi veri kümesi:**

Veri toplanma yeni blob üzerinden saatte (sıklığı: saat, aralığı: 1). Blob klasör yolu ve dosya adı dinamik olarak değerlendirilir işleniyor dilim başlangıç zamanı temel alınarak. Klasör yolu yıl, ay ve gün kısmını başlangıç saati ve dosya adı başlangıç zamanı saat bölümünü kullanır. "dış": "true" ayarı Bu tablo veri fabrikası dış ve veri fabrikasında bir etkinlik tarafından üretilen değil Data Factory hizmetinin sizi bilgilendirir.

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": "\n"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```
**Azure SQL veri ambarı veri kümesi çıkışını:**

Örnek verileri Azure SQL Data Warehouse "MyTable" adlı bir tablo kopyalar. Blob CSV dosyasında içerecek şekilde beklediğiniz gibi tablo Azure SQL Data Warehouse ile aynı sayıda sütun oluşturun. Yeni satırlar tabloya saatte eklenir.

```JSON
{
  "name": "AzureSqlDWOutput",
  "properties": {
    "type": "AzureSqlDWTable",
    "linkedServiceName": "AzureSqlDWLinkedService",
    "typeProperties": {
      "tableName": "MyOutputTable"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
**BlobSource ve SqlDWSink ardışık düzeninde etkinlik kopyalayın:**

Ardışık Düzen giriş ve çıkış veri kümeleri kullanmak üzere yapılandırıldığı ve saatte çalışacak şekilde zamanlanır kopyalama etkinliği içerir. JSON tanımını düzenindeki **kaynak** türü ayarlanmış **BlobSource** ve **havuz** türü ayarlanmış **SqlDWSink**.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQLDW",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlDWOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource",
            "blobColumnSeparators": ","
          },
          "sink": {
            "type": "SqlDWSink",
            "allowPolyBase": true
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
      ]
   }
}
```
İçin bkz: bkz [1 TB altında 15 dakika Azure Data Factory ile Azure SQL Data Warehouse'a veri yükleme](data-factory-load-sql-data-warehouse.md) ve [Azure Data Factory ile veri yükleme](../../sql-data-warehouse/sql-data-warehouse-get-started-load-with-azure-data-factory.md) makale Azure SQL Data Warehouse belgelerinde.

## <a name="performance-and-tuning"></a>Performans ve ayarlama
Bkz: [kopya etkinliği performansının & ayarlama Kılavuzu](data-factory-copy-activity-performance.md) bu veri taşıma (kopyalama etkinliği) Azure Data Factory ve onu en iyi duruma getirmek için çeşitli yollar etkisi performansını anahtar Etkenler hakkında bilgi edinmek için.
