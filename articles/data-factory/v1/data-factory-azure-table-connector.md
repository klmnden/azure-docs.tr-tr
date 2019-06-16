---
title: Veri gönderip buralardan Azure tablo Taşı | Microsoft Docs
description: Azure Data Factory kullanarak Azure tablo depolama içine/dışına veri taşıma konusunda bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 07b046b1-7884-4e57-a613-337292416319
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: aed341c50332b424a1149c129629cd451a4e5133
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66146923"
---
# <a name="move-data-to-and-from-azure-table-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure tablo gelen ve giden veri taşıma
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](data-factory-azure-table-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-azure-table-storage.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [V2'de Azure tablo depolama Bağlayıcısı](../connector-azure-table-storage.md).

Bu makalede, verileri Azure tablo depolama içine/dışına taşımak için Azure Data Factory'de kopyalama etkinliği kullanmayı açıklar. Yapılar [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği ile verileri taşıma genel bir bakış sunar. 

Tüm Azure tablo depolama için desteklenen kaynak veri deposundan veya herhangi bir desteklenen havuz veri deposu için Azure tablo Depolama'dan veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak ve havuz desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablo. 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak bir Azure tablo depolama içine/dışına veri taşıyan kopyalama etkinliği ile işlem hattı oluşturabilirsiniz.

Bir işlem hattı oluşturmanın en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [Öğreticisi: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma hızlı bir kılavuz.

Ayrıca, bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portalında**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve  **REST API**. Bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için. 

API'ler ve Araçlar kullanmanıza bakılmaksızın, bir havuz veri deposu için bir kaynak veri deposundan veri taşıyan bir işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirin: 

1. Oluşturma **bağlı hizmetler** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar.
2. Oluşturma **veri kümeleri** kopyalama işleminin girdi ve çıktı verilerini göstermek için. 
3. Oluşturma bir **işlem hattı** bir veri kümesini girdi ve çıktı olarak bir veri kümesini alan kopyalama etkinliği ile. 

Sihirbazı'nı kullandığınızda, bu Data Factory varlıklarını (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, bu Data Factory varlıkları JSON biçimini kullanarak tanımlayın. Bir Azure tablo depolama içine/dışına veri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları ile örnekleri için bkz [JSON örnekler](#json-examples) bu makalenin.

Aşağıdaki bölümler Azure tablo depolama için belirli Data Factory varlıkları tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar: 

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri
Bağlı hizmetler Azure blob depolama, bir Azure data factory'ye bağlamak için kullanabileceğiniz iki tür vardır. Bunlar: **AzureStorage** bağlı hizmet ve **AzureStorageSas** bağlı hizmeti. Azure depolama bağlı hizmeti Azure depolama data factory ile küresel erişim sağlar. Azure depolama SAS (paylaşılan erişim imzası) bağlı ise hizmet kısıtlı/süresi sınırlı erişimi olan data factory Azure depolama sağlar. Bu iki bağlı hizmet arasında başka hiçbir fark yoktur. Gereksinimlerinize uyan bağlı hizmeti seçin. Aşağıdaki bölümlerde, bu iki bağlı hizmet üzerinde daha fazla ayrıntı sağlar.

[!INCLUDE [data-factory-azure-storage-linked-services](../../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümleri ve veri kümeleri tanımlamak için kullanılabilir özellikleri tam listesi için bkz [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler bir veri kümesi JSON İlkesi yapısı ve kullanılabilirlik gibi tüm veri kümesi türleri (Azure SQL, Azure blob, Azure tablo, vs.) için benzer.

TypeProperties bölümünün her tür veri kümesi için farklıdır ve verilerin veri deposundaki konumu hakkında bilgi sağlar. **TypeProperties** türü için veri kümesi bölümünü **AzureTable** aşağıdaki özelliklere sahiptir.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlı hizmeti Azure tablo veritabanı örneğinde tablonun adını gösterir. |Evet. Bir tableName bir azureTableSourceQuery belirtildiğinde, tablodaki tüm kayıtları hedefe kopyalanır. Sorguyu karşılayan bir tablodaki kayıtları bir azureTableSourceQuery de belirtilirse, hedefe kopyalanamadı. |

### <a name="schema-by-data-factory"></a>Veri fabrikası tarafından şeması
Azure tablo gibi şemasız veri depoları için Data Factory hizmetinin şemayı aşağıdaki yollardan biriyle algılar:

1. Verilerin yapısını kullanarak belirtirseniz **yapısı** veri kümesi tanımında, Data Factory hizmetinin özelliği, şema olarak bu yapı geliştirir. Bu durumda, bir satır bir sütun için bir değer içermiyorsa null değeri için sağlanır.
2. Kullanarak verilerin yapısını belirtmezseniz **yapısı** özelliği, Data Factory veri kümesi tanımında, verilerin ilk satırı kullanarak şemayı algılar. Bu durumda, ilk satır, tam şema içermiyorsa, bazı sütunları kopyalama işleminin sonucunda eksik.

Bu nedenle, şemasız veri kaynakları için veri kullanımının yapısı belirtmek için en iyi yöntem olacaktır **yapısı** özelliği.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümleri & etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları oluşturma](data-factory-create-pipelines.md) makalesi. Ad, açıklama, girdi ve çıktı veri kümeleri ve ilkeleri gibi özellikler, tüm etkinlik türleri için kullanılabilir.

Etkinliğin typeProperties bölümündeki özellikler, diğer yandan her etkinlik türü ile değişir. Kopyalama etkinliği için kaynaklar ve havuzlar türlerine bağlı olarak farklılık gösterir.

**AzureTableSource** typeProperties bölümünde aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| azureTableSourceQuery |Verileri okumak için özel sorgu kullanın. |Azure tablo sorgu dizesi. Sonraki bölümdeki örneklere bakın. |Hayır. Bir tableName bir azureTableSourceQuery belirtildiğinde, tablodaki tüm kayıtları hedefe kopyalanır. Sorguyu karşılayan bir tablodaki kayıtları bir azureTableSourceQuery de belirtilirse, hedefe kopyalanamadı. |
| azureTableSourceIgnoreTableNotFound |Swallow özel durum tablosunun mevcut olup olmadığını gösterir. |TRUE<br/>FALSE |Hayır |

### <a name="azuretablesourcequery-examples"></a>azureTableSourceQuery örnekleri
Azure tablo sütunu dize türü ise:

```JSON
azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', SliceStart)"
```

Azure tablo sütunu datetime türünde ise:

```JSON
"azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', SliceStart, SliceEnd)"
```

**AzureTableSink** typeProperties bölümünde aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| azureTableDefaultPartitionKeyValue |Havuz tarafından kullanılan varsayılan bölüm anahtarı değeri. |Bir dize değeri. |Hayır |
| azureTablePartitionKeyName |Değerleri bölüm anahtarı olarak kullanılan sütun adını belirtin. Belirtilmezse, AzureTableDefaultPartitionKeyValue bölüm anahtarı olarak kullanılır. |Sütun adı. |Hayır |
| azureTableRowKeyName |Sütun değerleri satır anahtarı olarak kullanılan sütun adını belirtin. Belirtilmezse, her satır için bir GUID kullanın. |Sütun adı. |Hayır |
| azureTableInsertType |Azure tabloya veri eklemek için modu.<br/><br/>Bu özellik, var olan satır bölüm ve satır anahtarları eşleşen çıktı tablosu değiştirildi veya birleştirilmiş değerlerine sahip olup olmadığını denetler. <br/><br/>Bu ayarları (birleştirme ve Değiştir) nasıl çalıştığı hakkında bilgi edinmek için bkz. [Ekle ya da birleştirme varlık](https://msdn.microsoft.com/library/azure/hh452241.aspx) ve [Ekle veya Değiştir varlık](https://msdn.microsoft.com/library/azure/hh452242.aspx) konuları. <br/><br> Bu ayar, tablo düzeyinde değil satır düzeyinde uygulanır ve her iki seçeneği giriş bulunmayan çıkış tablosundaki satırları siler. |(Varsayılan) birleştirme<br/>Değiştir |Hayır |
| writeBatchSize |WriteBatchSize veya writeBatchTimeout isabet edildiğinde verileri Azure tablosuna ekler. |Tamsayı (satır sayısı) |Hayır (varsayılan: 10000) |
| writeBatchTimeout |WriteBatchSize veya writeBatchTimeout isabet edildiğinde verileri Azure tablosuna ekler. |TimeSpan<br/><br/>Örnek: "00: 20:00" (20 dakika) |Hayır (depolama istemci varsayılan zaman aşımı süresi için varsayılan değer 90 saniye) |

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName
Bir kaynak sütun azureTablePartitionKeyName hedef sütunun kullanabilmeniz için önce JSON özelliği translator'ı kullanarak bir hedef sütun eşleyin.

Aşağıdaki örnekte, kaynak sütunu DivisionID hedef sütuna eşlendi: DivisionID.  

```JSON
"translator": {
    "type": "TabularTranslator",
    "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
}
```
DivisionID bölüm anahtarı olarak belirtilir.

```JSON
"sink": {
    "type": "AzureTableSink",
    "azureTablePartitionKeyName": "DivisionID",
    "writeBatchSize": 100,
    "writeBatchTimeout": "01:00:00"
}
```
## <a name="json-examples"></a>JSON örnekleri
Aşağıdaki örnekler kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlamak [Azure portalında](data-factory-copy-activity-tutorial-using-azure-portal.md) veya [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bunlar, Azure tablo depolama ve Azure Blob veritabanı ve veri kopyalamak nasıl gösterir. Ancak, veriler kopyalanabilir **doğrudan** herhangi birinden herhangi birine desteklenen kaynakları başlatır. Daha fazla bilgi için bkz: "desteklenen veri depoları ve biçimler" bölümündeki [kopyalama etkinliğiyle veri taşıma](data-factory-data-movement-activities.md).

## <a name="example-copy-data-from-azure-table-to-azure-blob"></a>Örnek: Azure Blob için veri Azure tablodan kopyalama
Aşağıdaki örnek, gösterir:

1. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) (tablo ve blob için kullanılır).
2. Girdi [veri kümesi](data-factory-create-datasets.md) türü [AzureTable](#dataset-properties).
3. Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. [İşlem hattı](data-factory-create-pipelines.md) kopyalama etkinlikli AzureTableSource kullanır ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Örnek, bir blob için bir Azure tablosu varsayılan bölümün saatte ait verileri kopyalar. Bu örneklerde kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

**Azure depolama bağlı hizmeti:**

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
Azure Data Factory, Azure depolama bağlı hizmetlerini iki türlerini destekler: **AzureStorage** ve **AzureStorageSas**. Birinci hesap anahtarı içeren bağlantı dizesini belirtin ve daha sonra biri için paylaşılan erişim imzası (SAS) URI belirtin. Bkz: [bağlı hizmetler](#linked-service-properties) ayrıntıları bölümü.  

**Azure tablosu giriş veri kümesi:**

Örnek, bir tablo "MyTable" Azure tablosunda oluşturmuş olduğunuzu varsayar.

"Dış" ayarını: "true" bildirir Data Factory hizmetinin veri kümesi dış veri fabrikasına ve veri fabrikasında bir etkinliği tarafından üretilen değil.

```JSON
{
  "name": "AzureTableInput",
  "properties": {
    "type": "AzureTable",
    "linkedServiceName": "StorageLinkedService",
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

**Azure Blob çıktı veri kümesi:**

Veriler her saat yeni bir bloba yazılır (Sıklık: saat, interval: 1). Blob için klasör yolu işlenmekte olan dilimin başlangıç zamanı temel alınarak dinamik olarak değerlendirilir. Yıl, ay, gün ve saat bölümlerini başlangıç zamanı klasör yolu kullanır.

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

**AzureTableSource ve BlobSink ile bir işlem hattındaki kopyalama etkinliği:**

İşlem hattının giriş ve çıkış veri kümelerini kullanmak için yapılandırıldığı ve saatte bir çalışacak şekilde zamanlanmış bir kopyalama etkinliği içeriyor. JSON tanımı, işlem hattındaki **kaynak** türü ayarlandığında **AzureTableSource** ve **havuz** türü ayarlandığında **BlobSink**. Belirtilen SQL sorgusu **AzureTableSourceQuery** özelliği seçen veri varsayılan bölümünden saatte kopyalanacak.

```JSON
{
    "name":"SamplePipeline",
    "properties":{
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[
            {
                "name": "AzureTabletoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": "AzureTableInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "AzureTableSource",
                        "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
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

## <a name="example-copy-data-from-azure-blob-to-azure-table"></a>Örnek: Verileri Azure Blobundan Azure Tablo kopyalama
Aşağıdaki örnek, gösterir:

1. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) (tablo ve blob için kullanılır)
2. Girdi [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
3. Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [AzureTable](#dataset-properties).
4. [İşlem hattı](data-factory-create-pipelines.md) kullanan kopyalama etkinlikli [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) ve [AzureTableSink](#copy-activity-properties).

Zaman serisi verileri Azure için bir Azure blob örnek kopya saatlik tablo. Bu örneklerde kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

**(Azure tablosu ve Blob için için) Azure depolama bağlı hizmeti:**

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

Azure Data Factory, Azure depolama bağlı hizmetlerini iki türlerini destekler: **AzureStorage** ve **AzureStorageSas**. Birinci hesap anahtarı içeren bağlantı dizesini belirtin ve daha sonra biri için paylaşılan erişim imzası (SAS) URI belirtin. Bkz: [bağlı hizmetler](#linked-service-properties) ayrıntıları bölümü.

**Azure Blob girdi veri kümesi:**

Veri alındığından yeni blobundan her saat (Sıklık: saat, interval: 1). Blob klasörü yolu ve dosya adı dinamik olarak değerlendirilir işlenmekte olan dilimin başlangıç zamanı temel alınarak. Klasör yolu yıl, ay ve gün kısmını başlangıç saati ve dosya adı başlangıç zamanı saat bölümünü kullanır. "dış": "true" ayarı, Data Factory hizmetinin veri kümesi dış veri fabrikasına ve veri fabrikasında bir etkinliği tarafından üretilen değil bildirir.

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

**Azure tablo çıktı veri kümesi:**

Örnek verileri Azure tablo "MyTable" adlı bir tabloya kopyalar. Blob CSV dosyasını içerecek şekilde beklediğiniz gibi bir Azure tablosu ile aynı sayıda sütun oluşturun. Yeni satırlar saatte tablosuna eklenir.

```JSON
{
  "name": "AzureTableOutput",
  "properties": {
    "type": "AzureTable",
    "linkedServiceName": "StorageLinkedService",
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

**BlobSource ve AzureTableSink ile bir işlem hattındaki kopyalama etkinliği:**

İşlem hattının giriş ve çıkış veri kümelerini kullanmak için yapılandırıldığı ve saatte bir çalışacak şekilde zamanlanmış bir kopyalama etkinliği içeriyor. JSON tanımı, işlem hattındaki **kaynak** türü ayarlandığında **BlobSource** ve **havuz** türü ayarlandığında **AzureTableSink**.

```JSON
{
  "name":"SamplePipeline",
  "properties":{
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[
      {
        "name": "AzureBlobtoTable",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureTableOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "AzureTableSink",
            "writeBatchSize": 100,
            "writeBatchTimeout": "01:00:00"
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
## <a name="type-mapping-for-azure-table"></a>Azure tablosu için tür eşlemesi
Belirtildiği gibi [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği, aşağıdaki iki adımlı yaklaşım türleriyle havuz için kaynak türünden otomatik tür dönüştürmeleri gerçekleştirir.

1. Yerel kaynak türlerinden .NET türüne dönüştürün
2. .NET türünden yerel havuz türüne dönüştürün

Taşınırken veri & Azure tablosundan aşağıdaki [Azure tablo hizmeti tarafından tanımlanan eşlemeler](https://msdn.microsoft.com/library/azure/dd179338.aspx) .NET türüne ve Azure tablo OData türlerinden kullanılır.

| OData veri türü | .NET türü | Ayrıntılar |
| --- | --- | --- |
| Edm.Binary |byte[] |Bir bayt dizisi en fazla 64 KB. |
| Edm.Boolean |bool |Bir Boole değeri. |
| Edm.DateTime |DateTime |Eşgüdümlü Evrensel Saat (UTC) olarak ifade edilen bir 64-bit değeri. Desteklenen tarih/saat aralığı 1 Ocak 1601 M.S. 12:00 gece ' başlar (C.E.), UTC. Aralık 9999 31 Aralık sona erer. |
| Edm.Double |double |Bir 64-bit kayan nokta değeri. |
| Edm.Guid |Guid |128 bit genel benzersiz tanımlayıcı. |
| Edm.Int32 |Int32 |Bir 32 bit tamsayı. |
| Edm.Int64 |Int64 |Bir 64-bit tamsayı. |
| Edm.String |String |UTF-16 kodlu bir değer. Dize değerleri, en fazla 64 KB olabilir. |

### <a name="type-conversion-sample"></a>Tür dönüştürme örnek
Aşağıdaki örnek, verileri Azure Blob'tan Azure tablo ile tür dönüştürmeleri kopyalamak için ' dir.

Blob veri kümesi CSV biçimi ve üç sütun içeren varsayalım. Datetime sütunlarındaki haftanın günü için Fransızca kısaltılmış kullanarak özel bir tarih saat biçiminde, bunlardan biridir.

Sütunları için tür tanımları gibi birlikte Blob kaynak veri kümesi tanımlayın.

```JSON
{
    "name": " AzureBlobInput",
    "properties":
    {
        "structure":
        [
            { "name": "userid", "type": "Int64"},
            { "name": "name", "type": "String"},
            { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
        ],
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder",
            "fileName":"myfile.csv",
            "format":
            {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "external": true,
        "availability":
        {
            "frequency": "Hour",
            "interval": 1,
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
Tür eşlemesine Azure tablo OData türünden .NET türü için göz önünde bulundurulduğunda, aşağıdaki şemayla Azure tablosunda tablo tanımlarsınız.

**Azure tablo şeması:**

| Sütun adı | Type |
| --- | --- |
| userid |Edm.Int64 |
| name |Edm.String |
| lastlogindate |Edm.DateTime |

Ardından, Azure tablosu veri kümesi şu şekilde tanımlayın. Tür bilgilerini temel alınan veri deposunda zaten belirtilmiş olduğundan "yapı" bölümü ile tür bilgilerini belirtmek gerekmez.

```JSON
{
  "name": "AzureTableOutput",
  "properties": {
    "type": "AzureTable",
    "linkedServiceName": "StorageLinkedService",
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

Bu durumda, Data Factory Datetime alanı verileri BLOB'dan Azure tablo taşırken "fr-fr" kültür kullanılarak özel bir tarih/saat biçimi de dahil olmak üzere dönüştürmeleri otomatik olarak yazın.

> [!NOTE]
> Kaynak veri kümesindeki sütunları havuz veri kümesi sütunlara eşlemek için bkz: [Azure Data factory'de veri kümesi sütunlarını eşleme](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Performans ve ayarlama
Veri taşıma (kopyalama etkinliği) Azure Data Factory ve bunu en iyi duruma getirmek için çeşitli yollar, performansı etkileyebilir anahtar Etkenler hakkında bilgi edinmek için [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md).
