---
title: Veri taşıma/Azure tablosundan | Microsoft Docs
description: Öğesine/öğesinden Azure Data Factory kullanarak Azure Table Storage veri taşıma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 07b046b1-7884-4e57-a613-337292416319
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 996b1e5cbc477bf8a67a8cbb118961aaedf151fd
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34621515"
---
# <a name="move-data-to-and-from-azure-table-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Table gelen ve veri taşıma
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](data-factory-azure-table-connector.md)
> * [Sürüm 2 - Önizleme](../connector-azure-table-storage.md)

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [V2 Azure Table Storage Bağlayıcısı](../connector-azure-table-storage.md).

Bu makalede kopya etkinliği Azure Data Factory öğesine/öğesinden Azure Table Storage veri taşıma için nasıl kullanılacağı açıklanmaktadır. Derlemeler [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) kopyalama etkinliği ile veri taşıma için genel bir bakış sunar makalesi. 

Verileri Azure tablo depolaması tüm desteklenen kaynak veri deposundan veya Azure Table Storage tüm desteklenen havuz veri deposuna kopyalayabilirsiniz. Kaynakları veya havuzlarını olarak kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablo. 

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak Azure Table Storage/gruptan verileri taşır kopyalama etkinliği ile işlem hattı oluşturun.

Bir işlem hattı oluşturmak için en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [öğretici: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma Hızlı Kılavuz.

Bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**, ve **REST API**. Bkz: [kopyalama etkinliği öğretici](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için. 

Araçlar ya da API'leri kullanıp bir havuz veri deposu için bir kaynak veri deposundan verileri taşır bir ardışık düzen oluşturmak için aşağıdaki adımları gerçekleştirin: 

1. Oluşturma **bağlantılı Hizmetleri** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar.
2. Oluşturma **veri kümeleri** kopyalama işlemi için girdi ve çıktı verilerini temsil etmek için. 
3. Oluşturma bir **ardışık düzen** bir giriş olarak bir veri kümesi ve bir veri kümesini çıktı olarak alan kopyalama etkinliği ile. 

Sihirbazı'nı kullandığınızda, bu Data Factory varlıkları (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, JSON biçimini kullanarak bu Data Factory varlıklarını tanımlayın.  Veri kopyalama/Azure tablo depolaması için kullanılan Data Factory varlıkları için JSON tanımlarıyla örnekleri için bkz: [JSON örnekler](#json-examples) bu makalenin. 

Aşağıdaki bölümler, Azure Table Storage Data Factory varlıklarını belirli tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar: 

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Bağlı hizmetler Azure blob depolama bir Azure data factory'ye bağlamak için kullanabileceğiniz iki tür vardır. Bunlar: **AzureStorage** bağlantılı hizmeti ve **AzureStorageSas** bağlı hizmeti. Azure Storage bağlı hizmeti Azure Storage data factory ile genel erişim sağlar. Azure depolama SAS (paylaşılan erişim imzası) bağlı ise hizmeti Azure Storage veri fabrikası kısıtlanmış/zaman sınırlı erişimi olan sağlar. Bu iki bağlı hizmet arasındaki herhangi bir fark vardır. Gereksinimlerinize uygun bağlı hizmeti seçin. Aşağıdaki bölümler bu iki bağlı hizmet üzerinde daha fazla ayrıntı sağlar.

[!INCLUDE [data-factory-azure-storage-linked-services](../../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümler & özellikleri veri kümeleri tanımlamak için kullanılabilir tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler yapısı, kullanılabilirlik ve bir veri kümesi JSON İlkesi gibi tüm veri türleri (Azure SQL, Azure blob, Azure tablo, vs.) için benzer.

TypeProperties bölümü dataset her tür için farklıdır ve verilerin veri deposunda konumu hakkında bilgi sağlar. **TypeProperties** veri kümesi için bir bölüm türü **AzureTable** aşağıdaki özelliklere sahiptir.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlantılı hizmet başvurduğu Azure tablo veritabanı örneğinde tablonun adı. |Evet. Bir tableName bir azureTableSourceQuery belirtildiğinde, tablodaki tüm kayıtları hedefe kopyalanır. Bir azureTableSourceQuery de belirtilirse, sorgu karşılayan tablodaki kayıtları hedefe kopyalanır. |

### <a name="schema-by-data-factory"></a>Şema tarafından veri fabrikası
Azure Table gibi şemasız veri depoları için Data Factory hizmetinin şema aşağıdaki yollardan biriyle oluşturur:

1. Verilerin yapısını kullanarak belirtirseniz **yapısı** özelliği veri kümesi tanımında Data Factory hizmetinin bu yapısı şema olarak geliştirir. Bu durumda, bir satır bir sütun için bir değer içermiyorsa, bir null değer için sağlanır.
2. Kullanarak verilerin yapısını belirtmezseniz **yapısı** Data Factory veri kümesi tanımı özelliğinde verileri ilk satırını kullanarak şema oluşturur. Bu durumda, ilk satırın tam şema içermiyorsa, bazı sütunları kopyalama işlemi sonucunda eksik.

Bu nedenle, şemasız veri kaynakları için en iyi uygulama verileri kullanarak yapısı belirtmektir **yapısı** özelliği.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümler & özellikleri etkinlikleri tanımlamak için kullanılabilir tam listesi için bkz: [oluşturma ardışık düzen](data-factory-create-pipelines.md) makalesi. Ad, açıklama, giriş ve çıkış veri kümeleri ve ilkeleri gibi özellikler etkinlikleri tüm türleri için kullanılabilir.

Etkinliğin typeProperties bölümündeki özellikler diğer yandan her etkinlik türü ile değişir. Kopya etkinliği için bunlar türlerini kaynakları ve havuzlarını bağlı olarak farklılık gösterir.

**AzureTableSource** typeProperties bölümünde aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| azureTableSourceQuery |Verileri okumak için özel sorgu kullanın. |Azure tablo sorgu dizesi. Sonraki bölümdeki örneklere bakın. |Hayır. Bir tableName bir azureTableSourceQuery belirtildiğinde, tablodaki tüm kayıtları hedefe kopyalanır. Bir azureTableSourceQuery de belirtilirse, sorgu karşılayan tablodaki kayıtları hedefe kopyalanır. |
| azureTableSourceIgnoreTableNotFound |Swallow tablosunun özel durum var olup olmadığını gösterir. |TRUE<br/>FALSE |Hayır |

### <a name="azuretablesourcequery-examples"></a>azureTableSourceQuery örnekleri
Azure tablo sütunu dize türü ise:

```JSON
azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', SliceStart)"
```

Azure tablo sütunu datetime türü ise:

```JSON
"azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', SliceStart, SliceEnd)"
```

**AzureTableSink** typeProperties bölümünde aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| azureTableDefaultPartitionKeyValue |Havuzu tarafından kullanılan varsayılan bölüm anahtar değeri. |Bir dize değeri. |Hayır |
| azureTablePartitionKeyName |Değerleri bölüm anahtarları olarak kullanılan sütun adını belirtin. Belirtilmezse, AzureTableDefaultPartitionKeyValue bölüm anahtarı olarak kullanılır. |Sütun adı. |Hayır |
| azureTableRowKeyName |Sütun değerleri satır anahtarı olarak kullanılan sütun adını belirtin. Belirtilmezse, her satır için bir GUID kullanın. |Sütun adı. |Hayır |
| azureTableInsertType |Azure tabloya veri ekleme modu.<br/><br/>Bu özellik, var olan satırları eşleşen bölüm ve satır anahtarlarla çıkış tablosuna birleştirilmiş veya değiştirilmesi değerlerine sahip olup olmadığını denetler. <br/><br/>Bu ayarları (birleştirme ve Değiştir) nasıl çalıştığı hakkında bilgi edinmek için [ekleme veya birleştirme varlık](https://msdn.microsoft.com/library/azure/hh452241.aspx) ve [ekleme veya değiştirme varlık](https://msdn.microsoft.com/library/azure/hh452242.aspx) Konular. <br/><br> Bu ayar, tablo düzeyinde değil satır düzeyinde uygulanır ve girdide var olmayan satır çıkış tablosuna hiçbiri seçeneği siler. |Merge (varsayılan)<br/>Değiştir |Hayır |
| writeBatchSize |WriteBatchSize veya writeBatchTimeout gelindiğinde veri Azure tablosuna ekler. |Tamsayı (satır sayısı) |Hayır (varsayılan: 10000) |
| writeBatchTimeout |WriteBatchSize veya writeBatchTimeout gelindiğinde veri Azure tablosuna ekler. |TimeSpan<br/><br/>Örnek: "00: 20:00" (20 dakika) |Hayır (depolama İstemcisi varsayılan zaman aşımı için varsayılan değer 90 saniye) |

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName
Bir kaynak sütun azureTablePartitionKeyName hedef sütun kullanmadan önce Çeviricisi JSON özelliğini kullanarak bir hedef sütun eşleme.

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
Aşağıdaki örnekleri kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) veya [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bunlar ve Azure Table Storage ve Azure Blob veritabanına veri kopyalamak nasıl gösterir. Ancak, veriler kopyalanabilir **doğrudan** herhangi birinden herhangi birine desteklenen kaynakları iç havuzlar. Daha fazla bilgi için "desteklenen veri depoları ve biçimleri" bölümüne bakın [Kopyala etkinliğini kullanarak veri taşıma](data-factory-data-movement-activities.md).

## <a name="example-copy-data-from-azure-table-to-azure-blob"></a>Örnek: verileri Azure tablodan Azure Blob kopyalama
Aşağıdaki örnek gösterilmektedir:

1. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) (Tablo & blob için kullanılır).
2. Bir giriş [dataset](data-factory-create-datasets.md) türü [AzureTable](#dataset-properties).
3. Bir çıkış [dataset](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. [Ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [AzureTableSource](#activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Örnek bir blob için bir Azure tablosunda varsayılan bölümü saatte ait verileri kopyalar. Bu örnekler kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

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
Azure Data Factory iki tür Azure Storage bağlı hizmeti destekler: **AzureStorage** ve **AzureStorageSas**. İlk biri için hesap anahtarı içeren bağlantı dizesini belirtin ve daha sonra biri için paylaşılan erişim imzası (SAS) URI'sini belirtin. Bkz: [bağlı hizmetler](#linked-service-properties) ayrıntıları bölümü.  

**Azure Tablo girdi veri kümesi:**

Örnek Azure tablosunda tablo "MyTable" oluşturdunuz varsayılır.

"Dış" ayarı: "true" bildirir Data Factory hizmetinin veri kümesi data factory dış ve veri fabrikasında bir etkinlik tarafından üretilen değil.

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

**AzureTableSource ve BlobSink ardışık düzeninde etkinlik kopyalayın:**

Ardışık Düzen giriş ve çıkış veri kümeleri kullanmak üzere yapılandırıldığı ve saatte çalışacak şekilde zamanlanır kopyalama etkinliği içerir. JSON tanımını düzenindeki **kaynak** türü ayarlanmış **AzureTableSource** ve **havuz** türü ayarlanmış **BlobSink**. Belirtilen SQL sorgusu **AzureTableSourceQuery** özelliği seçer verileri varsayılan bölümünden saatte kopyalamak için.

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

## <a name="example-copy-data-from-azure-blob-to-azure-table"></a>Örnek: verileri Azure Blob'tan Azure tablosuna kopyalayın
Aşağıdaki örnek gösterilmektedir:

1. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) (Tablo & blob için kullanılır)
2. Bir giriş [dataset](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
3. Bir çıkış [dataset](data-factory-create-datasets.md) türü [AzureTable](#dataset-properties).
4. [Ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) ve [AzureTableSink](#copy-activity-properties).

Zaman serisi için Azure verileri Azure blob örnek kopyaları saatlik tablo. Bu örnekler kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

**(İçin Azure Table & Blob) azure depolama bağlı hizmeti:**

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

Azure Data Factory iki tür Azure Storage bağlı hizmeti destekler: **AzureStorage** ve **AzureStorageSas**. İlk biri için hesap anahtarı içeren bağlantı dizesini belirtin ve daha sonra biri için paylaşılan erişim imzası (SAS) URI'sini belirtin. Bkz: [bağlı hizmetler](#linked-service-properties) ayrıntıları bölümü.

**Azure Blob girdi veri kümesi:**

Veri toplanma yeni blob üzerinden saatte (sıklığı: saat, aralığı: 1). Blob klasör yolu ve dosya adı dinamik olarak değerlendirilir işleniyor dilim başlangıç zamanı temel alınarak. Klasör yolu yıl, ay ve gün kısmını başlangıç saati ve dosya adı başlangıç zamanı saat bölümünü kullanır. "dış": "true" ayarı, veri kümesi data factory dış ve veri fabrikasında bir etkinlik tarafından üretilen değil Data Factory hizmetinin bildirir.

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

**Azure tablo çıkış dataset:**

Örnek verileri Azure tablosunda "MyTable" adlı bir tablo kopyalar. Blob CSV dosyasında içerecek şekilde beklendiği gibi aynı sayıda sütuna bir Azure tablosu oluşturun. Yeni satırlar tabloya saatte eklenir.

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

**BlobSource ve AzureTableSink ardışık düzeninde etkinlik kopyalayın:**

Ardışık Düzen giriş ve çıkış veri kümeleri kullanmak üzere yapılandırıldığı ve saatte çalışacak şekilde zamanlanır kopyalama etkinliği içerir. JSON tanımını düzenindeki **kaynak** türü ayarlanmış **BlobSource** ve **havuz** türü ayarlanmış **AzureTableSink**.

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
## <a name="type-mapping-for-azure-table"></a>Tür eşlemesi için Azure tablo
Bölümünde belirtildiği gibi [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makale, kopyalama etkinliği, aşağıdaki iki aşamalı yaklaşımı türleriyle havuz için kaynak türünden otomatik tür dönüşümleri gerçekleştirir.

1. Yerel kaynak türlerinden .NET türüne dönüştürün
2. .NET türünden yerel havuz türüne dönüştürün

Taşınırken veri & Azure tablosundan aşağıdaki [Azure tablo hizmeti tarafından tanımlanan eşlemeler](https://msdn.microsoft.com/library/azure/dd179338.aspx) Azure tablo OData türleri .NET türü ve tersi yönde kullanılır.

| OData veri türü | .NET türü | Ayrıntılar |
| --- | --- | --- |
| Edm.Binary |Byte] |Bir bayt dizisi en çok 64 KB. |
| Edm.Boolean |bool |Bir Boole değeri. |
| Edm.DateTime |DateTime |Eşgüdümlü Evrensel Saat (UTC) olarak ifade edilen bir 64-bit değeri. Desteklenen tarih/saat aralığı 1 Ocak 1601 gece 12:00 gece ' başlar (C.E.) UTC. Aralık 9999 31 Aralık sona erer. |
| Edm.Double |double |Bir 64-bit kayan noktalı değeri. |
| Edm.Guid |Guid |128-bit bir genel benzersiz tanımlayıcısı. |
| Edm.Int32 |Int32 |Bir 32 bit tamsayı. |
| Edm.Int64 |Int64 |64 bitlik bir tamsayı. |
| Edm.String |Dize |UTF-16 kodlu değer. Dize değerlerini en çok 64 KB olabilir. |

### <a name="type-conversion-sample"></a>Tür dönüştürme örnek
Aşağıdaki örnek, verileri Azure Blob'tan Azure tablosuna tür dönüşümleri ile kopyalamak için ' dir.

Blob veri kümesi CSV biçiminde olduğunu ve üç sütun içeren varsayalım. Bunlardan biri, haftanın günü için kısaltılmış Fransızca adları kullanarak bir özel datetime biçimiyle datetime bir sütundur.

Blob kaynağı dataset sütunlar için tür tanımları birlikte aşağıdaki gibi tanımlayın.

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
Tür eşlemesi için .NET türü Azure tablo OData türünden verildiğinde, aşağıdaki şema ile Azure tablosunda tablo tanımlarsınız.

**Azure tablo şemasını:**

| Sütun adı | Tür |
| --- | --- |
| Kullanıcı Kimliği |Edm.Int64 |
| ad |Edm.String |
| lastlogindate |Edm.DateTime |

Ardından, Azure Table dataset aşağıdaki gibi tanımlayın. Tür bilgilerini temel alınan veri deposunda zaten belirtilen türü bilgilerle "yapısı" bölümünde belirtmek gerekmez.

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

Bu durumda, veri fabrikası dönüşümleri Datetime alanın veri Blobundan Azure tablosuna taşırken "fr-fr" kültürü kullanarak özel datetime biçim de dahil olmak üzere otomatik olarak yazın.

> [!NOTE]
> Kaynak veri kümesi sütunlarından havuz kümesinden sütunlara eşlemek için bkz [Azure Data Factory veri kümesi sütunlarında eşleme](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Performans ve ayarlama
Bu veri taşıma (kopyalama etkinliği) Azure Data Factory ve onu en iyi duruma getirmek için çeşitli yollar etkisi performansını anahtar Etkenler hakkında bilgi için bkz [kopya etkinliği performansının & ayarlama Kılavuzu](data-factory-copy-activity-performance.md).
