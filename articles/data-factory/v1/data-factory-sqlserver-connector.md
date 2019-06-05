---
title: Verileri SQL Server'dan taşıma | Microsoft Docs
description: Veri gönderip buralardan şirket içi SQL Server veritabanı veya Azure Data Factory kullanarak Azure VM taşıma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 864ece28-93b5-4309-9873-b095bbe6fedd
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: be36f9ab881f2375b14ba0ea36038f9e840d199f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66156525"
---
# <a name="move-data-to-and-from-sql-server-on-premises-or-on-iaas-azure-vm-using-azure-data-factory"></a>Azure Data Factory kullanarak verileri ve SQL Server şirket içi veya ıaas (Azure VM) taşıma
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](data-factory-sqlserver-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-sql-server.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [V2'de SQL Server Bağlayıcısı](../connector-sql-server.md).

Bu makalede, kopyalama etkinliği Azure Data Factory'de veri gönderip buralardan veri bir şirket içi SQL Server veritabanını taşımak için nasıl kullanılacağı açıklanmaktadır. Yapılar [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği ile verileri taşıma genel bir bakış sunar.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="supported-scenarios"></a>Desteklenen senaryolar
Veri kopyalayabilirsiniz **SQL Server veritabanından** aşağıdaki verilere depolar:

[!INCLUDE [data-factory-supported-sink](../../../includes/data-factory-supported-sinks.md)]

Aşağıdaki veri depolarından veri kopyalayabilirsiniz **bir SQL Server veritabanına**:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

## <a name="supported-sql-server-versions"></a>Desteklenen SQL Server sürümleri
/ İçin aşağıdaki sürümleri örneği tarafından barındırılan şirket içi veya Azure Iaas SQL kimlik doğrulaması hem Windows kimlik doğrulaması kullanarak veri kopyalama bu SQL Server Bağlayıcısı desteği: SQL Server 2016, SQL Server 2014, SQL Server 2012, SQL Server 2008 R2, SQL Server 2008, SQL Server 2005

## <a name="enabling-connectivity"></a>Bağlantıyı etkinleştirme
Kavramlar ve barındırılan SQL Server şirket içi veya Azure Iaas (hizmet olarak altyapı-a-) Vm'leri bağlamak için gerekli adımları aynıdır. Her iki durumda da, veri yönetimi ağ geçidi bağlantısı için kullanmanız gerekir.

Bkz: [Bulut ve şirket içi konumlar arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makalenin veri yönetimi ağ geçidi ve ağ geçidini ayarlamadan adım adım yönergeleri hakkında bilgi edinin. Bir ağ geçidi örneğini ayarlama, SQL Server ile bağlanmak için bir önkoşuldur.

Ağ geçidi aynı şirket içi makine veya Bulut VM örneği üzerinde daha iyi performans için SQL Server yükleyebilirsiniz ancak bunları farklı makinelere yüklemeniz önerilir. SQL Server ve ağ geçidi, ayrı makinelerde sahip kaynak çekişmesini azaltır.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak veri gönderip buralardan veri bir şirket içi SQL Server veritabanına taşıyan kopyalama etkinliği ile işlem hattı oluşturabilirsiniz.

Bir işlem hattı oluşturmanın en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [Öğreticisi: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma hızlı bir kılavuz.

Ayrıca, bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portalında**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve  **REST API**. Bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

API'ler ve Araçlar kullanmanıza bakılmaksızın, bir havuz veri deposu için bir kaynak veri deposundan veri taşıyan bir işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma bir **veri fabrikası**. Veri fabrikası, bir veya daha fazla işlem hattı içerebilir.
2. Oluşturma **bağlı hizmetler** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar. Örneğin, verileri bir SQL Server veritabanından Azure blob depolama için kopyalıyorsanız kullanarak SQL Server veritabanı ve Azure depolama hesabınızı veri fabrikanıza bağlamak için iki bağlı hizmet oluşturursunuz. SQL Server veritabanına özel bağlı hizmeti özellikleri için bkz: [bağlı hizmeti özellikleri](#linked-service-properties) bölümü.
3. Oluşturma **veri kümeleri** kopyalama işleminin girdi ve çıktı verilerini göstermek için. Son adımda bahsedilen örnekte, SQL Server veritabanınızda girdi verilerini içeren SQL tablosunu belirtmek için bir veri kümesi oluşturun. Ve blob kapsayıcısını ve SQL Server veritabanından kopyalanan verileri tutan klasörün belirtmek için başka bir veri kümesi oluşturun. SQL Server veritabanı için özel veri kümesi özellikleri için bkz: [veri kümesi özellikleri](#dataset-properties) bölümü.
4. Oluşturma bir **işlem hattı** bir veri kümesini girdi ve çıktı olarak bir veri kümesini alan kopyalama etkinliği ile. Daha önce bahsedilen örnekte SqlSource bir kaynak ve BlobSink havuz olarak kopyalama etkinliği için kullanırsınız. Azure Blob depolama alanından SQL Server veritabanına kopyalanıyorsa, benzer şekilde, BlobSource ve SqlSink kopyalama etkinliği kullanırsınız. SQL Server veritabanına özel kopyalama etkinliği özellikleri için bkz: [kopyalama etkinliği özellikleri](#copy-activity-properties) bölümü. Bir kaynak veya havuz bir veri deposunu kullanma hakkında daha fazla ayrıntı için önceki bölümde veri deponuz için bağlantıya tıklayın.

Sihirbazı'nı kullandığınızda, bu Data Factory varlıklarını (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, bu Data Factory varlıkları JSON biçimini kullanarak tanımlayın. Veri gönderip buralardan şirket içi SQL Server veritabanına kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları ile örnekleri için bkz [JSON örnekler](#json-examples-for-copying-data-from-and-to-sql-server) bu makalenin.

Aşağıdaki bölümler, SQL Server Data Factory varlıklarını belirli tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri
Bağlı hizmet türü oluşturma **OnPremisesSqlServer** bir şirket içi SQL Server veritabanına bir veri fabrikasına bağlamak için. Aşağıdaki tabloda, şirket içi SQL Server bağlı hizmeti için özel JSON öğeleri için bir açıklama sağlar.

Aşağıdaki tabloda, SQL Server bağlı hizmeti için özel JSON öğeleri için bir açıklama sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliği ayarlanmalıdır: **OnPremisesSqlServer**. |Evet |
| connectionString |SQL kimlik doğrulaması veya Windows kimlik doğrulaması kullanarak şirket içi SQL Server veritabanına bağlanmak üzere gereken bağlantı dizesi bilgilerini belirtin. |Evet |
| gatewayName |Data Factory hizmetinin şirket içi SQL Server veritabanına bağlanmak için kullanması gereken ağ geçidi adı. |Evet |
| username |Windows kimlik doğrulamasını kullanıyorsanız kullanıcı adı belirtin. Örnek: **domainname\\username**. |Hayır |
| password |Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. |Hayır |

Kimlik bilgilerini kullanarak şifreleyebilirsiniz **yeni AzDataFactoryEncryptValue** cmdlet'i ve bunları aşağıdaki örnekte gösterildiği gibi bağlantı dizesini kullanın (**EncryptedCredential** özellik):

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```

### <a name="samples"></a>Örnekler
**SQL kimlik doğrulaması kullanmak için JSON**

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties":
    {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
**Windows kimlik doğrulaması kullanmak için JSON**

Veri Yönetimi ağ geçidi, şirket içi SQL Server veritabanına bağlanmak için belirtilen kullanıcı hesabı kimliğine bürünür.

```json
{
    "Name": " MyOnPremisesSQLDB",
    "Properties":
    {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
            "username": "<domain\\username>",
            "password": "<password>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bir veri kümesi türü kullanılan örneklerinde **SqlServerTable** bir SQL Server veritabanındaki bir tabloda temsil etmek için.

Bölümleri ve veri kümeleri tanımlamak için kullanılabilir özellikleri tam listesi için bkz [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler bir veri kümesi JSON İlkesi yapısı ve kullanılabilirlik gibi tüm veri kümesi türleri (SQL Server, Azure blob, Azure tablo, vs.) için benzer.

TypeProperties bölümünün her tür veri kümesi için farklıdır ve verilerin veri deposundaki konumu hakkında bilgi sağlar. **TypeProperties** türü için veri kümesi bölümünü **SqlServerTable** aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Tablo veya Görünüm bağlı hizmeti SQL Server veritabanı örneğindeki adını gösterir. |Evet |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bir SQL Server veritabanından veri taşıyorsanız, kaynak türü için kopyalama etkinliğindeki ayarladığınız **SqlSource**. Bir SQL Server veritabanına veri taşıyorsanız, benzer şekilde, Havuz türü için kopyalama etkinliğindeki ayarladığınız **SqlSink**. Bu bölümde SqlSource ve SqlSink tarafından desteklenen özelliklerin bir listesini sağlar.

Bölümleri & etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları oluşturma](data-factory-create-pipelines.md) makalesi. Ad, açıklama, girdi ve çıktı tabloları ve ilkeleri gibi özellikler, tüm etkinlik türleri için kullanılabilir.

> [!NOTE]
> Kopyalama etkinliği, tek bir girdi alır ve tek bir çıktı üretir.

Oysa etkinliğin typeProperties bölümündeki özellikler her etkinlik türü ile farklılık gösterir. Kopyalama etkinliği için kaynaklar ve havuzlar türlerine bağlı olarak farklılık gösterir.

### <a name="sqlsource"></a>SqlSource
Kopya etkinlikteki kaynak türünde olduğunda **SqlSource**, aşağıdaki özellikler kullanılabilir **typeProperties** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sqlReaderQuery |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin: seçin * MyTable öğesinden. Birden çok tablo giriş veri kümesi tarafından başvurulan veritabanından başvurabilir. Belirtilmezse, yürütülen SQL deyimi: MyTable arasından seçin. |Hayır |
| sqlReaderStoredProcedureName |Kaynak tablo verilerini okuyan saklı yordamın adı. |Saklı yordamın adı. Son SQL deyim bir SELECT deyimi saklı yordam içinde olmalıdır. |Hayır |
| storedProcedureParameters |Saklı yordamın parametreleri. |Ad/değer çiftleri. Adları ve parametreleri büyük küçük harfleri, adları ve saklı yordam parametreleri büyük küçük harfleri eşleşmelidir. |Hayır |

Varsa **sqlReaderQuery** belirtilen SqlSource için kopyalama etkinliği, verileri almak için SQL Server veritabanı kaynak karşı bu sorgu çalıştırır.

Alternatif olarak, bir saklı yordam belirterek belirtebileceğiniz **sqlReaderStoredProcedureName** ve **storedProcedureParameters** (saklı yordamın parametreleri sürerse).

SqlReaderQuery ya da sqlReaderStoredProcedureName belirtmezseniz yapı bölümünde tanımlanan sütunları SQL Server veritabanında çalıştırmak için bir select sorgusu oluşturmak için kullanılır. Veri kümesi tanımı yapısına sahip değilse, tüm sütunları tablodan seçilir.

> [!NOTE]
> Kullanırken **sqlReaderStoredProcedureName**, yine de için bir değer belirtmeniz gerekiyorsa **tableName** veri kümesi JSON özelliğinde. Ancak bu tabloya karşı gerçekleştirilen başka bir doğrulama vardır.

### <a name="sqlsink"></a>SqlSink
**SqlSink** aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| writeBatchTimeout |Toplu ekleme işlemi zaman aşımına uğramadan önce tamamlanması için bir süre bekleyin. |TimeSpan<br/><br/> Örnek: "00: 30:00" (30 dakika). |Hayır |
| writeBatchSize |Arabellek boyutu writeBatchSize ulaştığında veri SQL tablosuna ekler. |Tamsayı (satır sayısı) |Hayır (varsayılan: 10000) |
| sqlWriterCleanupScript |Belirli bir dilimin veri Temizlenen şekilde yürütmek kopyalama etkinliği için bir sorgu belirtin. Daha fazla bilgi için [tekrarlanabilir kopyalama](#repeatable-copy) bölümü. |Bir sorgu deyimi. |Hayır |
| sliceIdentifierColumnName |Kopyalama etkinliği'nin ne zaman yeniden çalıştırılacağını belirli bir dilimin verileri temizlemek için kullanılan otomatik dilim tanımlayıcısı ile doldurmak için sütun adı belirtin. Daha fazla bilgi için [tekrarlanabilir kopyalama](#repeatable-copy) bölümü. |Bir sütunun veri türüyle binary(32) sütun adı. |Hayır |
| sqlWriterStoredProcedureName |Kaynak verileri hedef tabloya örn upsert eder ya da kendi iş mantığınızı kullanarak dönüşüm nasıl uygulanacağını tanımlayan saklı yordamın adı. <br/><br/>Bu saklı yordamı olacaktır Not **toplu iş çağrılan**. Yalnızca bir kez çalışır ve hiçbir kaynak verileri ile/delete örn truncate yapmak için kullanma işlemi yapmak istiyorsanız `sqlWriterCleanupScript` özelliği. |Saklı yordamın adı. |Hayır |
| storedProcedureParameters |Saklı yordamın parametreleri. |Ad/değer çiftleri. Adları ve parametreleri büyük küçük harfleri, adları ve saklı yordam parametreleri büyük küçük harfleri eşleşmelidir. |Hayır |
| sqlWriterTableType |Saklı yordamda kullanılan tablo türü adı belirtin. Kopyalama etkinliği, taşınan veri bir geçici tablo bu tablo türü ile kullanılabilir hale getirir. Saklı yordam kodu daha sonra mevcut verilerle kopyalanan verileri birleştirebilirsiniz. |Bir tablo türü adı. |Hayır |


## <a name="json-examples-for-copying-data-from-and-to-sql-server"></a>Gelen ve SQL Server veri kopyalamak için JSON örnekleri
Aşağıdaki örnekler kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlamak [Azure portalında](data-factory-copy-activity-tutorial-using-azure-portal.md) veya [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Aşağıdaki örnekler, SQL Server ve Azure Blob Depolama ve veri kopyalamak nasıl gösterir. Ancak, veriler kopyalanabilir **doğrudan** herhangi birinden herhangi birine belirtilen havuzlarını kaynakları [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopyalama etkinliğini kullanarak Azure Data Factory'de.

## <a name="example-copy-data-from-sql-server-to-azure-blob"></a>Örnek: Verileri SQL Server'dan Azure Blob kopyalama
Aşağıdaki örnek, gösterir:

1. Bağlı hizmet türü [OnPremisesSqlServer](#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Girdi [veri kümesi](data-factory-create-datasets.md) türü [SqlServerTable](#dataset-properties).
4. Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. [İşlem hattı](data-factory-create-pipelines.md) kullanan kopyalama etkinlikli [SqlSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Örnek zaman serisi verilerini bir SQL Server tablosundan saatte bir Azure blobuna kopyalar. Bu örneklerde kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

İlk adım, veri yönetimi ağ geçidi'ni ayarlayın. Yönergeleri bulunan [Bulut ve şirket içi konumlar arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makalesi.

**SQL Server bağlı hizmeti**
```json
{
  "Name": "SqlServerLinkedService",
  "properties": {
    "type": "OnPremisesSqlServer",
    "typeProperties": {
      "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
      "gatewayName": "<gatewayname>"
    }
  }
}
```
**Azure Blob Depolama bağlı hizmeti**

```json
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
**SQL Server girdi veri kümesi**

Örnek, "MyTable" SQL Server'da bir tablo oluşturdunuz ve zaman serisi verileri için "timestampcolumn" adlı bir sütun içerdiği varsayılır. Tek bir veri kümesi kullanarak aynı veritabanı içinde birden çok tablolar üzerinden sorgulama yapabilirsiniz, ancak tek bir tabloya veri kümesinin tableName typeProperty için kullanılmalıdır.

"Dış" ayarını: "true" bildirir Data Factory hizmetinin veri kümesi dış veri fabrikasına ve veri fabrikasında bir etkinliği tarafından üretilen değil.

```json
{
  "name": "SqlServerInput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlServerLinkedService",
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
**Azure Blob çıktı veri kümesi**

Veriler her saat yeni bir bloba yazılır (Sıklık: saat, interval: 1). Blob için klasör yolu işlenmekte olan dilimin başlangıç zamanı temel alınarak dinamik olarak değerlendirilir. Yıl, ay, gün ve saat bölümlerini başlangıç zamanı klasör yolu kullanır.

```json
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
**Kopyalama etkinliği ile işlem hattı**

İşlem hattı, bu girdi ve çıktı veri kümelerini kullanmak için yapılandırıldığı ve saatte bir çalışacak şekilde zamanlanmış bir kopyalama etkinliği içeriyor. JSON tanımı, işlem hattındaki **kaynak** türü ayarlandığında **SqlSource** ve **havuz** türü ayarlandığında **BlobSink**. SQL sorgusu için belirtilen **SqlReaderQuery** özelliği veri kopyalamak için son bir saat içinde seçer.

```json
{
  "name":"SamplePipeline",
  "properties":{
    "start":"2016-06-01T18:00:00",
    "end":"2016-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[
      {
        "name": "SqlServertoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": " SqlServerInput"
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
Bu örnekte, **sqlReaderQuery** SqlSource için belirtilir. Kopyalama etkinliği, verileri almak için SQL Server veritabanı kaynak karşı bu sorguyu çalıştırır. Alternatif olarak, bir saklı yordam belirterek belirtebileceğiniz **sqlReaderStoredProcedureName** ve **storedProcedureParameters** (saklı yordamın parametreleri sürerse). Giriş veri kümesi tarafından başvurulan veritabanına birden fazla tablo sqlReaderQuery başvurabilirsiniz. Yalnızca veri kümesinin tableName typeProperty ayarlayın tabloya sınırlı değildir.

SqlReaderQuery veya sqlReaderStoredProcedureName belirtmezseniz yapı bölümünde tanımlanan sütunları SQL Server veritabanında çalıştırmak için bir select sorgusu oluşturmak için kullanılır. Veri kümesi tanımı yapısına sahip değilse, tüm sütunları tablodan seçilir.

Bkz: [Sql kaynağı](#sqlsource) bölümü ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) SqlSource ve BlobSink tarafından desteklenen özelliklerin listesi için.

## <a name="example-copy-data-from-azure-blob-to-sql-server"></a>Örnek: Verileri Azure Blobu'ndan SQL Server'a kopyalayın.
Aşağıdaki örnek, gösterir:

1. Türündeki bağlı hizmetin [OnPremisesSqlServer](#linked-service-properties).
2. Türündeki bağlı hizmetin [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Girdi [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).
5. [İşlem hattı](data-factory-create-pipelines.md) kullanan kopyalama etkinlikli [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) blobsource'u.

Zaman serisi verileri Azure SQL Server'a blob örnek kopya saatte tablo. Bu örneklerde kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

**SQL Server bağlı hizmeti**

```json
{
  "Name": "SqlServerLinkedService",
  "properties": {
    "type": "OnPremisesSqlServer",
    "typeProperties": {
      "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
      "gatewayName": "<gatewayname>"
    }
  }
}
```
**Azure Blob Depolama bağlı hizmeti**

```json
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
**Azure Blob girdi veri kümesi**

Veri alındığından yeni blobundan her saat (Sıklık: saat, interval: 1). Blob klasörü yolu ve dosya adı dinamik olarak değerlendirilir işlenmekte olan dilimin başlangıç zamanı temel alınarak. Klasör yolu yıl, ay ve gün kısmını başlangıç saati ve dosya adı başlangıç zamanı saat bölümünü kullanır. "dış": "true" ayarı, Data Factory hizmetinin veri kümesi dış veri fabrikasına ve veri fabrikasında bir etkinliği tarafından üretilen değil bildirir.

```json
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
**SQL Server çıktı veri kümesi**

Örnek verileri SQL Server'da "MyTable" adlı bir tabloya kopyalar. Blob CSV dosyasını içerecek şekilde beklediğiniz gibi SQL Server ile aynı sayıda sütun tablo oluşturun. Yeni satırlar saatte tablosuna eklenir.

```json
{
  "name": "SqlServerOutput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlServerLinkedService",
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
**Kopyalama etkinliği ile işlem hattı**

İşlem hattı, bu girdi ve çıktı veri kümelerini kullanmak için yapılandırıldığı ve saatte bir çalışacak şekilde zamanlanmış bir kopyalama etkinliği içeriyor. JSON tanımı, işlem hattındaki **kaynak** türü ayarlandığında **BlobSource** ve **havuz** türü ayarlandığında **SqlSink**.

```json
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
            "name": " SqlServerOutput "
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

## <a name="troubleshooting-connection-issues"></a>Bağlantı sorunlarını giderme
1. Uzak bağlantıları kabul etmek için SQL Server'ınızı yapılandırın. Başlatma **SQL Server Management Studio**, sağ **sunucu**, tıklatıp **özellikleri**. Seçin **bağlantıları** denetimi ve liste **sunucunun uzak bağlantılara izin vermek**.

    ![Uzak bağlantıları etkinleştir](./media/data-factory-sqlserver-connector/AllowRemoteConnections.png)

    Bkz: [uzaktan erişim sunucu yapılandırma seçeneğini yapılandırma](https://msdn.microsoft.com/library/ms191464.aspx) ayrıntılı adımlar için.
2. Başlatma **SQL Server Yapılandırma Yöneticisi**. Genişletin **SQL Server Ağ Yapılandırması** ve seçin, örneğin **MSSQLSERVER protokolleri**. Sağ bölmede protokolleri görmeniz gerekir. Sağ tıklayarak TCP/IP'yi etkinleştirin **TCP/IP'yi** tıklayıp **etkinleştirme**.

    ![TCP/IP'yi etkinleştirin](./media/data-factory-sqlserver-connector/EnableTCPProptocol.png)

    Bkz: [etkinleştirmek veya sunucu ağ protokolünü devre dışı](https://msdn.microsoft.com/library/ms191294.aspx) Ayrıntılar ve TCP/IP protokolünü etkinleştirme alternatif yolu.
3. Aynı pencerede çift **TCP/IP'yi** başlatmak için **TCP/IP özelliklerini** penceresi.
4. Geçiş **IP adresleri** sekmesi. Görmek için aşağı kaydırın **IPAll** bölümü. Not **TCP bağlantı noktası**(varsayılan değer **1433**).
5. Oluşturma bir **Windows Güvenlik Duvarı Kuralı** Bu bağlantı noktası üzerinden gelen trafiğe izin vermek için makinede.
6. **Bağlantıyı doğrulama**: Tam adı kullanılarak SQL Server'a bağlanmak için farklı bir makinede SQL Server Management Studio'yu kullanın. Örneğin: "\<makine\>.\< etki alanı\>. corp.\<şirket\>.com, 1433. "

   > [!IMPORTANT]
   > 
   > Bkz: [şirket içi kaynakları ve veri yönetimi ağ geçidi ile bulut arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) ayrıntılı bilgi için.
   > 
   > Bkz: [ağ geçidiyle ilgili sorunları giderme](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) bağlantı/ağ geçidi sorunlarını giderme ipuçları için ilgili sorunlar.


## <a name="identity-columns-in-the-target-database"></a>Hedef veritabanındaki Kimlik sütunları
Bu bölümde, bir kimlik sütunu hedef tabloyla kimlik sütunu ile bir kaynak tablosundan veri kopyalayan bir örnek sağlar.

**Kaynak Tablo:**

```sql
create table dbo.SourceTbl
(
    name varchar(100),
    age int
)
```
**Hedef Tablo:**

```sql
create table dbo.TargetTbl
(
    identifier int identity(1,1),
    name varchar(100),
    age int
)
```

Hedef tablo bir kimlik sütunu olduğuna dikkat edin.

**Kaynak veri kümesi JSON tanımı**

```json
{
    "name": "SampleSource",
    "properties": {
        "published": false,
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

```json
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "published": false,
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

Kaynak ve hedef tablonuz olarak farklı bir şemaya sahip olduğuna dikkat edin (hedef ek bir sütun kimliği içeriyor). Bu senaryoda, belirtmeniz gerekir. **yapısı** kimlik sütunu içermeyen hedef veri kümesi tanımında özelliği.

## <a name="invoke-stored-procedure-from-sql-sink"></a>SQL havuz saklı yordam çağırma
Bkz: [kopyalama etkinliğindeki SQL havuz için saklı yordam çağırma](data-factory-invoke-stored-procedure-from-copy-activity.md) makale için bir işlem hattının kopyalama etkinliği havuz SQL saklı bir yordam çağırma örneği.

## <a name="type-mapping-for-sql-server"></a>SQL server için tür eşlemesi
Belirtildiği gibi [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği, aşağıdaki 2 adımlı yaklaşım türleriyle havuz için kaynak türünden otomatik tür dönüştürmeleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürün
2. .NET türünden yerel havuz türüne dönüştürün

& SQL Server'dan veri taşıma, aşağıdaki eşlemeler SQL türü .NET türü ve bunun tersi de kullanılır.

Eşleme, aynı SQL Server veri türü eşlemesi ADO.NET için bir.

| SQL Server veritabanı altyapısı türü | .NET framework türü |
| --- | --- |
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
| sql_variant |Object * |
| metin |String, Char[] |
| time |TimeSpan |
| timestamp |Byte[] |
| tinyint |Byte |
| uniqueidentifier |Guid |
| varbinary |Byte[] |
| varchar |String, Char[] |
| xml |Xml |

## <a name="mapping-source-to-sink-columns"></a>Sütunları havuz için kaynak eşlemesi
Kaynak veri kümesindeki sütunları havuz veri kümesi sütunlara eşlemek için bkz: [Azure Data factory'de veri kümesi sütunlarını eşleme](data-factory-map-columns.md).

## <a name="repeatable-copy"></a>Tekrarlanabilir kopyalama
SQL Server veritabanına veri kopyalama, kopyalama etkinliği havuz tabloya verileri varsayılan olarak ekler. Bunun yerine bir UPSERT gerçekleştirmek için bkz: [Repeatable yazmak için SqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) makalesi.

İlişkisel veri kopyalama verileri depoladığında yinelenebilirliği istenmeyen sonuçlar önlemek için göz önünde bulundurun. Azure Data Factory'de bir dilim el ile çalıştırabilirsiniz. Bir hata oluştuğunda bir dilimi yeniden çalıştırmak için bir veri kümesi için yeniden deneme ilkesi de yapılandırabilirsiniz. Bir dilim her iki yolla yeniden çalıştırıldığında, aynı veri dilimi çalıştırılan kaç kez olursa olsun okuma emin olmanız gerekir. Bkz: [ilişkisel kaynaktan okumak Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Performans ve ayarlama
Bkz: [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md) veri taşıma (kopyalama etkinliği) Azure Data Factory ve bunu en iyi duruma getirmek için çeşitli yollar, performansı etkileyebilir anahtar Etkenler hakkında bilgi edinmek için.
