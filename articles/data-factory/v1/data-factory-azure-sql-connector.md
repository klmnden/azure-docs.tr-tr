---
title: "Veri kopyalama/Azure SQL veritabanından | Microsoft Docs"
description: "İçin/Azure SQL veritabanından Azure Data Factory kullanarak verileri kopyalamak öğrenin."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 484f735b-8464-40ba-a9fc-820e6553159e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 7e57003582dc6190b79e1b4eea38ec4adc1c521c
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="copy-data-to-and-from-azure-sql-database-using-azure-data-factory"></a>Veri ve Azure SQL Azure Data Factory kullanarak veritabanından kopyalamak
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](data-factory-azure-sql-connector.md)
> * [Sürüm 2 - Önizleme](../connector-azure-sql-database.md)

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [V2 Azure SQL Veritabanı Bağlayıcısı](../connector-azure-sql-database.md).

Bu makalede kopya etkinliği Azure Data Factory için ve Azure SQL veritabanından veri taşımak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) kopyalama etkinliği ile veri taşıma için genel bir bakış sunar makalesi.  

## <a name="supported-scenarios"></a>Desteklenen senaryolar
Veri kopyalama **Azure SQL veritabanından** aşağıdaki veri depolar:

[!INCLUDE [data-factory-supported-sinks](../../../includes/data-factory-supported-sinks.md)]

Aşağıdaki veri depolarına verileri kopyalayabilirsiniz **Azure SQL veritabanı**:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

## <a name="supported-authentication-type"></a>Desteklenen kimlik doğrulama türü
Azure SQL Veritabanı Bağlayıcısı temel kimlik doğrulamasını destekler.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak bir Azure SQL veritabanından/gelen verileri taşır kopyalama etkinliği ile işlem hattı oluşturun.

Bir işlem hattı oluşturmak için en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [öğretici: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma Hızlı Kılavuz.

Bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**, ve **REST API**. Bkz: [kopyalama etkinliği öğretici](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için. 

Araçlar ya da API'leri kullanıp bir havuz veri deposu için bir kaynak veri deposundan verileri taşır bir ardışık düzen oluşturmak için aşağıdaki adımları gerçekleştirin: 

1. Oluşturma bir **veri fabrikası**. Veri Fabrikası bir veya daha fazla ardışık düzen içerebilir. 
2. Oluşturma **bağlantılı Hizmetleri** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar. Örneğin, verileri Azure blob depolama alanından Azure SQL veritabanına kopyalıyorsanız Azure storage hesabını ve Azure SQL veritabanını veri fabrikanıza bağlamak için iki bağlı hizmet oluşturun. Azure SQL veritabanına özel bağlantılı hizmet özellikleri için bkz: [bağlantılı hizmet özellikleri](#linked-service-properties) bölümü. 
3. Oluşturma **veri kümeleri** kopyalama işlemi için girdi ve çıktı verilerini temsil etmek için. Son adımda bahsedilen örnekte blob kapsayıcısı ve giriş verilerini içeren klasörü belirtmek için bir veri kümesi oluşturun. Ve blob depolama biriminden kopyalanan verilerini tutan Azure SQL veritabanında SQL tablosu belirtmek için başka bir veri kümesi oluşturun. Azure Data Lake Store için özel veri kümesi özellikleri için bkz: [veri kümesi özellikleri](#dataset-properties) bölümü.
4. Oluşturma bir **ardışık düzen** bir giriş olarak bir veri kümesi ve bir veri kümesini çıktı olarak alan kopyalama etkinliği ile. Daha önce bahsedilen örnekte BlobSource bir kaynak ve SqlSink havuzu olarak kopya etkinliği için kullanırsınız. Azure Blob depolama alanına Azure SQL veritabanından kopyalıyorsanız benzer şekilde, SqlSource ve BlobSink kopyalama etkinliği kullanırsınız. Azure SQL veritabanına belirli kopyalama etkinliği özellikleri için bkz: [kopyalama etkinliği özellikleri](#copy-activity-properties) bölümü. Bir veri deposu bir kaynak veya bir havuz nasıl kullanılacağı hakkında daha fazla bilgi için önceki bölümde, veri deposu için bağlantıya tıklayın.

Sihirbazı'nı kullandığınızda, bu Data Factory varlıkları (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, JSON biçimini kullanarak bu Data Factory varlıklarını tanımlayın.  / Bir Azure SQL veritabanından veri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımlarıyla örnekleri için bkz: [JSON örnekler](#json-examples-for-copying-data-to-and-from-sql-database) bu makalenin. 

Aşağıdaki bölümler, Azure SQL veritabanını Data Factory varlıklarını belirli tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar: 

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Bir Azure SQL Hizmeti Bağlantıları data factory'nizi Azure SQL veritabanına bağlı. Aşağıdaki tabloda, JSON öğeleri Azure SQL bağlı hizmeti için belirli bir açıklamasını sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalıdır: **AzureSqlDatabase** |Evet |
| connectionString |ConnectionString özelliği için Azure SQL veritabanı örneğine bağlanmak için gereken bilgileri belirtin. Yalnızca temel kimlik doğrulama desteklenir. |Evet |

> [!IMPORTANT]
> Yapılandırma [Azure SQL veritabanı Güvenlik Duvarı](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) veritabanı sunucusuna [Azure hizmetlerinin sunucuya erişimine izin](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure). Ayrıca, dış Azure veri fabrikası ağ geçidi ile şirket içi veri kaynaklarından dahil olmak üzere Azure SQL veritabanına veri kopyalıyorsanız, Azure SQL veritabanına veri gönderme makine için uygun IP adresi aralığı yapılandırın.

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bir Azure SQL veritabanındaki giriş veya çıkış verileri temsil etmek için bir veri kümesi belirtmek için veri kümesine tür özelliği ayarlayın: **AzureSqlTable**. Ayarlama **linkedServiceName** özellik kümesinin adı ile Azure SQL bağlı hizmeti.  

Bölümler & özellikleri veri kümeleri tanımlamak için kullanılabilir tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler yapısı, kullanılabilirlik ve bir veri kümesi JSON İlkesi gibi tüm veri türleri (Azure SQL, Azure blob, Azure tablo, vs.) için benzer.

TypeProperties bölümü dataset her tür için farklıdır ve verilerin veri deposunda konumu hakkında bilgi sağlar. **TypeProperties** veri kümesi için bir bölüm türü **AzureSqlTable** aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Tablo veya Görünüm bağlantılı hizmetinin Azure SQL veritabanı örneğinde başvurduğu adı. |Evet |

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala
Bölümler & özellikleri etkinlikleri tanımlamak için kullanılabilir tam listesi için bkz: [oluşturma ardışık düzen](data-factory-create-pipelines.md) makalesi. Ad, açıklama, giriş ve çıkış tabloları ve ilke gibi özellikler etkinlikleri tüm türleri için kullanılabilir.

> [!NOTE]
> Kopyalama etkinliği yalnızca bir girdi alır ve tek bir çıktı üretir.

Bulunan özellikler **typeProperties** etkinlik bölümünü her etkinlik türü ile değişir. Kopya etkinliği için bunlar türlerini kaynakları ve havuzlarını bağlı olarak farklılık gösterir.

Bir Azure SQL veritabanından veri taşıyorsanız, kaynak türü için kopyalama etkinliğinde ayarladığınız **SqlSource**. Bir Azure SQL veritabanına veri taşıyorsanız, benzer şekilde, Havuz türü için kopyalama etkinliğinde ayarladığınız **SqlSink**. Bu bölümde SqlSource ve SqlSink tarafından desteklenen özellikler listesini sağlar.

### <a name="sqlsource"></a>SqlSource
Kopyalama etkinliğinde kaynak türü olduğunda **SqlSource**, aşağıdaki özellikler mevcuttur **typeProperties** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sqlReaderQuery |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örnek: `select * from MyTable`. |Hayır |
| sqlReaderStoredProcedureName |Kaynak tablodan veri okuyan saklı yordamın adı. |Saklı yordam adı. Son SQL deyimi SELECT deyimi içinde saklı yordamı olması gerekir. |Hayır |
| storedProcedureParameters |Saklı yordam parametreleri. |Ad/değer çiftleri. Adları ve büyük/küçük harf parametrelerinin adlarını ve saklı yordam parametreleri büyük/küçük harf eşleşmelidir. |Hayır |

Varsa **sqlReaderQuery** belirtilen SqlSource için kopyalama etkinliği veri almak için Azure SQL veritabanı kaynağında bu sorguyu çalıştırır. Alternatif olarak, bir saklı yordam belirterek belirleyebileceğiniz **sqlReaderStoredProcedureName** ve **storedProcedureParameters** (saklı yordam parametreleri alıyorsa).

SqlReaderQuery veya sqlReaderStoredProcedureName belirtmezseniz, JSON veri kümesi yapısı bölümünde tanımlanan sütunları bir sorgu oluşturmak için kullanılır (`select column1, column2 from mytable`) Azure SQL veritabanına karşı çalıştırmak için. Veri kümesi tanımı yapısına sahip değilse, tüm sütunları tablodan seçilir.

> [!NOTE]
> Kullandığınızda **sqlReaderStoredProcedureName**, yine de için bir değer belirtmeniz gerekiyorsa **tableName** JSON veri kümesi bir özellik. Yine de bu tabloya karşı gerçekleştirilen başka doğrulama vardır.
>
>

### <a name="sqlsource-example"></a>SqlSource örneği

```JSON
"source": {
    "type": "SqlSource",
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

### <a name="sqlsink"></a>SqlSink
**SqlSink** aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| writeBatchTimeout |Toplu ekleme işlemi zaman aşımına uğramadan önce tamamlamak bir süre bekleyin. |TimeSpan<br/><br/> Örnek: "00: 30:00" (30 dakika). |Hayır |
| writeBatchSize |Arabellek boyutu writeBatchSize ulaştığında veri SQL tablosuna ekler. |Tamsayı (satır sayısı) |Hayır (varsayılan: 10000) |
| sqlWriterCleanupScript |Belirli bir dilimle verilerinin temizlenmesini şekilde yürütmek kopyalama etkinliği için bir sorgu belirtin. Daha fazla bilgi için bkz: [yinelenebilir kopyalama](#repeatable-copy). |Sorgu bildirimi. |Hayır |
| Sliceıdentifiercolumnname |Ne zaman yeniden çalıştırılacağını belirli bir dilim verileri temizlemek için kullanılan otomatik dilim tanımlayıcı doldurmak kopyalama etkinliği için bir sütun adı belirtin. Daha fazla bilgi için bkz: [yinelenebilir kopyalama](#repeatable-copy). |Binary(32) veri türüne sahip bir sütunun sütun adı. |Hayır |
| sqlWriterStoredProcedureName |Saklı yordam adı hedef tabloda bu upserts (güncelleştirmeler/ekler) verileri. |Saklı yordam adı. |Hayır |
| storedProcedureParameters |Saklı yordam parametreleri. |Ad/değer çiftleri. Adları ve büyük/küçük harf parametrelerinin adlarını ve saklı yordam parametreleri büyük/küçük harf eşleşmelidir. |Hayır |
| sqlWriterTableType |Saklı yordam, kullanılacak bir tablo türü adı belirtin. Kopyalama etkinliği taşınan veri geçici bir tablo bu tablo türü ile kullanılabilir hale getirir. Saklı yordam kodu ardından var olan verilerle kopyalanan verileri birleştirebilirsiniz. |Bir tablo türü adı. |Hayır |

#### <a name="sqlsink-example"></a>SqlSink örneği

```JSON
"sink": {
    "type": "SqlSink",
    "writeBatchSize": 1000000,
    "writeBatchTimeout": "00:05:00",
    "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
    "sqlWriterTableType": "CopyTestTableType",
    "storedProcedureParameters": {
        "identifier": { "value": "1", "type": "Int" },
        "stringData": { "value": "str1" },
        "decimalData": { "value": "1", "type": "Decimal" }
    }
}
```

## <a name="json-examples-for-copying-data-to-and-from-sql-database"></a>JSON örnekleri ve SQL veritabanından veri kopyalama
Aşağıdaki örnekleri kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) veya [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bunlar ve Azure SQL Database ve Azure Blob depolama alanından veri kopyalamak nasıl gösterir. Ancak, veriler kopyalanabilir **doğrudan** herhangi birinden herhangi birine belirtildiği havuzlarını kaynakları [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopya etkinliği Azure Data Factory kullanarak.

### <a name="example-copy-data-from-azure-sql-database-to-azure-blob"></a>Örnek: verileri Azure SQL veritabanından Azure Blob kopyalama
Aynı aşağıdaki Data Factory varlıklarını tanımlar:

1. Bağlı hizmet türü [AzureSqlDatabase](#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bir giriş [dataset](data-factory-create-datasets.md) türü [AzureSqlTable](#dataset-properties).
4. Bir çıkış [dataset](data-factory-create-datasets.md) türü [Azure Blob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [SqlSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Örnek time series verilerini (saatlik, günlük, vb.) Azure SQL veritabanındaki bir tablo için bir blob saatte kopyalar. Bu örnekler kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.  

**Azure SQL Database hizmeti bağlı:**

```JSON
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
Bkz: [Azure SQL bağlı hizmeti](#linked-service) bu bağlantılı hizmet tarafından desteklenen özelliklerin listesi için bölüm.

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
Bkz: [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) bu bağlantılı hizmet tarafından desteklenen özelliklerin listesi için makale.


**Azure SQL girdi veri kümesi:**

Örnek, Azure SQL tablosu "MyTable" oluşturulur ve zaman serisi veri için "timestampcolumn" adlı bir sütun içerdiği varsayar.

"Dış" ayarı: "true" bildirir Azure Data Factory hizmetinin veri kümesi data factory dış ve veri fabrikasında bir etkinlik tarafından üretilen değil.

```JSON
{
  "name": "AzureSqlInput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
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

Bkz: [Azure SQL veri kümesi türü özellikleri](#dataset) bu dataset türü tarafından desteklenen özelliklerin listesi için bölüm.  

**Azure Blob dataset çıktı:**

Veri her saat yeni bir bloba yazılır (sıklığı: saat, aralığı: 1). Blob klasör yolu dinamik işlenmekte olan dilim başlangıç zamanı temel alınarak değerlendirilir. Klasör yolu yıl, ay, gün ve saat bölümleri başlangıç saatini kullanır.

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
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
Bkz: [Azure Blob veri kümesi türü özellikleri](data-factory-azure-blob-connector.md#dataset-properties) bu dataset türü tarafından desteklenen özelliklerin listesi için bölüm.  

**SQL kaynak ve Blob havuz sahip işlem hattı kopyalama etkinliğinde:**

Ardışık Düzen giriş ve çıkış veri kümeleri kullanmak üzere yapılandırıldığı ve saatte çalışacak şekilde zamanlanır kopyalama etkinliği içerir. JSON tanımını düzenindeki **kaynak** türü ayarlanmış **SqlSource** ve **havuz** türü ayarlanmış **BlobSink**. SQL sorgusu için belirtilen **SqlReaderQuery** özelliği veri kopyalamak için son bir saat içindeki seçer.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "AzureSQLtoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureSQLInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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
Örnekte, **sqlReaderQuery** SqlSource için belirtilir. Kopyalama etkinliği bu sorguyu veri almak için Azure SQL veritabanı kaynak karşı çalışır. Alternatif olarak, bir saklı yordam belirterek belirleyebileceğiniz **sqlReaderStoredProcedureName** ve **storedProcedureParameters** (saklı yordam parametreleri alıyorsa).

SqlReaderQuery veya sqlReaderStoredProcedureName belirtmezseniz JSON veri kümesi yapısı bölümünde tanımlanan sütunları Azure SQL veritabanına karşı çalıştırmak için bir sorgu oluşturmak için kullanılır. Örneğin: `select column1, column2 from mytable`. Veri kümesi tanımı yapısına sahip değilse, tüm sütunları tablodan seçilir.

Bkz: [Sql kaynağı](#sqlsource) bölüm ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) SqlSource ve BlobSink tarafından desteklenen özelliklerin listesi için.

### <a name="example-copy-data-from-azure-blob-to-azure-sql-database"></a>Örnek: verileri Azure Blob'tan Azure SQL veritabanına kopyalamak
Örnek aşağıdaki Data Factory varlıklarını tanımlar:  

1. Bağlı hizmet türü [AzureSqlDatabase](#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bir giriş [dataset](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Bir çıkış [dataset](data-factory-create-datasets.md) türü [AzureSqlTable](#dataset-properties).
5. A [ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) ve [SqlSink](#copy-activity-properties).

Zaman serisi Azure SQL tablosuna (saatlik, günlük, vb.) verileri Azure blob örnek kopyaları saatte veritabanı. Bu örnekler kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

**Azure SQL bağlı hizmeti:**

```JSON
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
Bkz: [Azure SQL bağlı hizmeti](#linked-service) bu bağlantılı hizmet tarafından desteklenen özelliklerin listesi için bölüm.

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
Bkz: [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) bu bağlantılı hizmet tarafından desteklenen özelliklerin listesi için makale.


**Azure Blob girdi veri kümesi:**

Veri toplanma yeni blob üzerinden saatte (sıklığı: saat, aralığı: 1). Blob klasör yolu ve dosya adı dinamik olarak değerlendirilir işleniyor dilim başlangıç zamanı temel alınarak. Klasör yolu yıl, ay ve gün kısmını başlangıç saati ve dosya adı başlangıç zamanı saat bölümünü kullanır. "dış": "true" ayarı Bu tablo veri fabrikası dış ve veri fabrikasında bir etkinlik tarafından üretilen değil Data Factory hizmetinin sizi bilgilendirir.

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
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
Bkz: [Azure Blob veri kümesi türü özellikleri](data-factory-azure-blob-connector.md#dataset-properties) bu dataset türü tarafından desteklenen özelliklerin listesi için bölüm.

**Azure SQL veritabanı veri kümesini çıktı:**

Örnek verileri Azure SQL "MyTable" adlı bir tablo kopyalar. Blob CSV dosyasında içerecek şekilde beklediğiniz gibi aynı sayıda sütun ile Azure SQL tablosu oluşturun. Yeni satırlar tabloya saatte eklenir.

```JSON
{
  "name": "AzureSqlOutput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
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
Bkz: [Azure SQL veri kümesi türü özellikleri](#dataset) bu dataset türü tarafından desteklenen özelliklerin listesi için bölüm.

**Blob kaynağı ve SQL havuz sahip işlem hattı kopyalama etkinliğinde:**

Ardışık Düzen giriş ve çıkış veri kümeleri kullanmak üzere yapılandırıldığı ve saatte çalışacak şekilde zamanlanır kopyalama etkinliği içerir. JSON tanımını düzenindeki **kaynak** türü ayarlanmış **BlobSource** ve **havuz** türü ayarlanmış **SqlSink**.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQL",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource",
            "blobColumnSeparators": ","
          },
          "sink": {
            "type": "SqlSink"
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
Bkz: [Sql havuz](#sqlsink) bölüm ve [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) SqlSink ve BlobSource tarafından desteklenen özelliklerin listesi için.

## <a name="identity-columns-in-the-target-database"></a>Hedef veritabanında kimlik sütunu
Bu bölüm veri kaynak tablodaki bir kimlik sütunu olmadan bir kimlik sütunu içeren bir hedef tablo kopyalamak için bir örnek verilmektedir.

**Kaynak tablosu:**

```SQL
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```
**Hedef Tablo:**

```SQL
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```
Hedef tabloda bir kimlik sütunu olduğuna dikkat edin.

**Kaynak veri kümesi JSON tanımı**

```JSON
{
    "name": "SampleSource",
    "properties": {
        "type": " SqlServerTable",
        "linkedServiceName": "TestIdentitySQL",
        "typeProperties": {
            "tableName": "SourceTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```
**Hedef veri kümesi JSON tanımı**

```JSON
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "type": "AzureSqlTable",
        "linkedServiceName": "TestIdentitySQLSource",
        "typeProperties": {
            "tableName": "TargetTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": false,
        "policy": {}
    }    
}
```

Kaynak ve hedef tablosu farklı şema sahip dikkat edin (hedef kimliğine sahip başka bir sütuna sahip). Bu senaryoda, belirtmeniz gerekir. **yapısı** kimlik sütunu içermeyen hedef veri kümesi tanımında özelliği.

## <a name="invoke-stored-procedure-from-sql-sink"></a>SQL havuz depolanan yordamı çağırma
Kopyalama etkinliği ardışık SQL havuzunda bir saklı yordam çağırma bir örnek için bkz: [kopyalama etkinliği SQL havuz için saklı yordam çağırma](data-factory-invoke-stored-procedure-from-copy-activity.md) makalesi. 

## <a name="type-mapping-for-azure-sql-database"></a>Tür eşlemesi için Azure SQL veritabanı
Bölümünde belirtildiği gibi [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makale kopyalama etkinliği aşağıdaki 2 adımlı yaklaşımı türleriyle havuz için kaynak türünden otomatik tür dönüşümleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürün
2. .NET türünden yerel havuz türüne dönüştürün

Veri taşımak ve Azure SQL veritabanından olduğunda, aşağıdaki eşlemelerini SQL türü .NET türü ve tersi yönde kullanılır. ADO.NET için SQL Server veri türü eşlemesi aynı eşlemedir.

| SQL Server veritabanı altyapısı türü | .NET framework türü |
| --- | --- |
| bigint |Int64 |
| İkili |Byte] |
| bit |Boole değeri |
| char |Dize, Char] |
| Tarih |Tarih saat |
| Tarih saat |Tarih saat |
| datetime2 |Tarih saat |
| Datetimeoffset |DateTimeOffset |
| Ondalık |Ondalık |
| FILESTREAM özniteliği (varbinary(max)) |Byte] |
| Kayan nokta |Çift |
| Görüntü |Byte] |
| Int |Int32 |
| para |Ondalık |
| nchar |Dize, Char] |
| ntext |Dize, Char] |
| sayısal |Ondalık |
| nvarchar |Dize, Char] |
| Gerçek |Tek |
| rowVersion |Byte] |
| smalldatetime |Tarih saat |
| tamsayı |Int16 |
| küçük para |Ondalık |
| sql_variant |Nesne * |
| Metin |Dize, Char] |
| time |TimeSpan |
| timestamp |Byte] |
| Mini tamsayı |Bayt |
| benzersiz tanımlayıcı |GUID |
| varbinary |Byte] |
| varchar |Dize, Char] |
| xml |XML |

## <a name="map-source-to-sink-columns"></a>Kaynak havuzu sütunları eşleme
Havuz dataset sütunlara kaynak kümesindeki eşleme sütunları hakkında bilgi edinmek için [Azure Data Factory veri kümesi sütunlarında eşleme](data-factory-map-columns.md).

## <a name="repeatable-copy"></a>Yinelenebilir kopyalama
SQL Server veritabanına veri kopyalama, kopyalama etkinliği verileri varsayılan olarak havuz tabloya ekler. Bunun yerine bir UPSERT gerçekleştirmek için bkz: [Repeatable yazmak için SqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) makalesi. 

İlişkisel veri kopyalama verileri depoladığında, Yinelenebilirlik istenmeyen sonuçları önlemek için göz önünde bulundurun. Azure Data Factory'de bir dilim el ile çalıştırabilirsiniz. Bir hata oluştuğunda bir dilimi yeniden çalıştırmak için bir veri kümesi için yeniden deneme ilkesi de yapılandırabilirsiniz. Bir dilim iki yolla yeniden çalıştırıldığında, aynı veri dilimi çalıştırmak kaç kez geçtiğinden bağımsız okuduğunuzdan emin olmanız gerekir. Bkz: [ilişkisel kaynaktan okumak Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Performans ve ayarlama
Bkz: [kopya etkinliği performansının & ayarlama Kılavuzu](data-factory-copy-activity-performance.md) bu veri taşıma (kopyalama etkinliği) Azure Data Factory ve onu en iyi duruma getirmek için çeşitli yollar etkisi performansını anahtar Etkenler hakkında bilgi edinmek için.
