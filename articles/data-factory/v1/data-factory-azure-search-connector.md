---
title: Data Factory kullanarak Search dizinine veri gönderme | Microsoft Docs
description: Azure Data Factory kullanarak Azure Search dizinine veri gönderme hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: f8d46e1e-5c37-4408-80fb-c54be532a4ab
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: f68e1077ebc26245b25eae3b0310db74b6d1357e
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37046454"
---
# <a name="push-data-to-an-azure-search-index-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Search dizini veri göndermek
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](data-factory-azure-search-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-azure-search.md)

> [!NOTE]
> Bu makale, veri fabrikası 1 sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [V2 Azure Search Bağlayıcısı](../connector-azure-search.md).

Bu makalede, Azure Search dizini için desteklenen kaynak veri deposundan veri göndermek için kopyalama etkinliği kullanmayı açıklar. Desteklenen kaynak veri depoları, kaynağı sütununda listelenen [desteklenen kaynakları ve havuzlarını](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablo. Bu makalede derlemeler [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) desteklenen veri deposu birleşimleri kopyalama etkinliği ile veri taşıma için genel bir bakış sunan makalesi.

## <a name="enabling-connectivity"></a>Bağlantıyı etkinleştirme
Veri Fabrikası hizmetine bağlanın bir şirket içi veri deposuna izin vermek için şirket içi ortamınızda veri yönetimi ağ geçidi yükleyin. Ağ geçidi ana bilgisayar veri kaynağını depolamak aynı makine üzerindeki veya veri depolama alanı kaynakları için rekabete önlemek için ayrı bir makine yükleyebilirsiniz.

Veri Yönetimi ağ geçidi şirket içi veri kaynaklarına güvenli ve yönetilen bir şekilde bulut hizmetlerine bağlar. Bkz: [şirket içi ve bulut arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makale veri yönetimi ağ geçidi hakkında ayrıntılı bilgi için.

## <a name="getting-started"></a>Başlarken
Veri kaynağı veri deposundan farklı araçlar/API'lerini kullanarak Azure Search dizinine iter kopyalama etkinliği ile işlem hattı oluşturun.

Bir işlem hattı oluşturmak için en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [öğretici: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma Hızlı Kılavuz.

Bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu** , **.NET API**, ve **REST API**. Bkz: [kopyalama etkinliği öğretici](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için. 

Araçlar ya da API'leri kullanıp bir havuz veri deposu için bir kaynak veri deposundan verileri taşır bir ardışık düzen oluşturmak için aşağıdaki adımları gerçekleştirin: 

1. Oluşturma **bağlantılı Hizmetleri** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar.
2. Oluşturma **veri kümeleri** kopyalama işlemi için girdi ve çıktı verilerini temsil etmek için. 
3. Oluşturma bir **ardışık düzen** bir giriş olarak bir veri kümesi ve bir veri kümesini çıktı olarak alan kopyalama etkinliği ile. 

Sihirbazı'nı kullandığınızda, bu Data Factory varlıkları (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, JSON biçimini kullanarak bu Data Factory varlıklarını tanımlayın.  Azure Search dizinine veri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları içeren bir örnek için bkz: [JSON örnek: veri kopyalama şirket içi SQL Server'dan Azure Search dizinine](#json-example-copy-data-from-on-premises-sql-server-to-azure-search-index) bu makalenin. 

Aşağıdaki bölümler, Azure Search dizinine Data Factory varlıklarını belirli tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki tabloda Azure Search bağlantılı hizmete özgü JSON öğeleri için açıklamalar sağlanır.

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| type | Type özelliği ayarlanmalıdır: **AzureSearch**. | Evet |
| url | Azure Search hizmeti için URL. | Evet |
| anahtar | Azure Search hizmeti için yönetici anahtarı. | Evet |

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümelerini tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler yapısı, kullanılabilirlik ve bir veri kümesi JSON İlkesi gibi tüm veri kümesi türleri için benzerdir. **TypeProperties** bölümü, her veri kümesi türü için farklıdır. TypeProperties bölüm türü veri kümesi için **AzureSearchIndex** aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| type | Type özelliği ayarlamak **AzureSearchIndex**.| Evet |
| indexName | Azure Search dizini adı. Veri Fabrikası dizinini oluşturmaz. Azure Search'te dizin mevcut olması gerekir. | Evet |


## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümleri ve etkinlikleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [ardışık düzen oluşturma](data-factory-create-pipelines.md) makalesi. Ad, açıklama, giriş ve çıkış tabloları ve çeşitli ilkeleri gibi özellikler etkinlikleri tüm türleri için kullanılabilir. Oysa typeProperties bölümündeki özellikler her etkinlik türü ile farklılık gösterir. Kopya etkinliği için bunlar türlerini kaynakları ve havuzlarını bağlı olarak farklılık gösterir.

Kopyalama Havuz türü olduğunda etkinliği için **AzureSearchIndexSink**, typeProperties bölümünde aşağıdaki özellikler kullanılabilir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| -------- | ----------- | -------------- | -------- |
| WriteBehavior | Birleştir veya bir belge dizinde zaten mevcut olduğunda Değiştir belirtir. Bkz: [WriteBehavior özelliği](#writebehavior-property).| (Varsayılan) birleştirme<br/>Karşıya Yükle| Hayır |
| WriteBatchSize | Arabellek boyutu writeBatchSize ulaştığında Azure Search dizinine veri yükler. Bkz: [WriteBatchSize özelliği](#writebatchsize-property) Ayrıntılar için. | 1 için 1.000. Varsayılan değer 1000'dir. | Hayır |

### <a name="writebehavior-property"></a>WriteBehavior özelliği
Verileri yazarken AzureSearchSink upserts. Diğer bir deyişle, belge anahtarı Azure arama dizini zaten varsa bir belge yazarken, Azure Search çakışma özel durum atma yerine varolan bir belgeyi güncelleştirir.

AzureSearchSink (AzureSearch SDK kullanılarak) aşağıdaki iki upsert davranışlar sağlar:

- **Birleştirme**: yeni belgedeki tüm sütunları mevcut birleştirin. Yeni belge null değere sahip sütunlar için mevcut değeri korunur.
- **Karşıya yükleme**: varolan bir yeni belge değiştirir. Yeni belge içinde belirtilmeyen sütunlar için değer olup olmadığını değeri null olmayan mevcut belgede veya null olarak ayarlanır.

Varsayılan davranış **birleştirme**.

### <a name="writebatchsize-property"></a>WriteBatchSize özelliği
Azure Search Hizmeti yazma belgeleri toplu iş olarak destekler. Bir toplu iş için 1 1.000 eylemler içerebilir. Bir eylem karşıya yükleme/birleştirme işlemi gerçekleştirmek için bir belge işler.

### <a name="data-type-support"></a>Veri türü desteği
Aşağıdaki tabloda, bir Azure Search veri türü veya desteklenip desteklenmediğini belirtir.

| Azure arama veri türü | Azure arama havuzunda desteklenir |
| ---------------------- | ------------------------------ |
| Dize | E |
| Int32 | E |
| Int64 | E |
| çift | E |
| Boole | E |
| DataTimeOffset | E |
| Dize dizisi | N |
| GeographyPoint | N |

## <a name="json-example-copy-data-from-on-premises-sql-server-to-azure-search-index"></a>JSON örnek: veri kopyalama şirket içi SQL Server'dan Azure Search dizini

Aşağıdaki örnek gösterilmektedir:

1.  Bağlı hizmet türü [AzureSearch](#linked-service-properties).
2.  Bağlı hizmet türü [OnPremisesSqlServer](data-factory-sqlserver-connector.md#linked-service-properties).
3.  Bir giriş [dataset](data-factory-create-datasets.md) türü [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).
4.  Bir çıkış [dataset](data-factory-create-datasets.md) türü [AzureSearchIndex](#dataset-properties).
4.  A [ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [SqlSource](data-factory-sqlserver-connector.md#copy-activity-properties) ve [AzureSearchIndexSink](#copy-activity-properties).

Örnek zaman serisi veri bir şirket içi SQL Server veritabanından bir Azure Search dizini saatlik kopyalar. Bu örnekte kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

İlk adım olarak, veri yönetimi ağ geçidi, şirket içi makinenizde ayarlayın. Yönergeler bulunan [Bulut ve şirket içi konumlara arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) makalesi.

**Azure arama bağlantılı hizmeti:**

```JSON
{
    "name": "AzureSearchLinkedService",
    "properties": {
        "type": "AzureSearch",
        "typeProperties": {
            "url": "https://<service>.search.windows.net",
            "key": "<AdminKey>"
        }
    }
}
```

**SQL Server bağlantılı hizmeti**

```JSON
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

**SQL Server girdi veri kümesi**

Örnek SQL Server'da bir tablo "MyTable" oluşturulur ve zaman serisi veri için "timestampcolumn" adlı bir sütun içerdiği varsayar. Tek bir veri kümesini kullanarak aynı veritabanı içinde birden çok tablo üzerinde sorgulayabilir, ancak tek bir tablo için veri kümesi'nin tableName typeProperty kullanılmalıdır.

"Dış" ayarı: "true" bildirir Data Factory hizmetinin veri kümesi data factory dış ve veri fabrikasında bir etkinlik tarafından üretilen değil.

```JSON
{
  "name": "SqlServerDataset",
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

**Azure Search'te veri kümesini çıktı:**

Örnek verileri adlı bir Azure Search dizini kopyalar **ürünleri**. Veri Fabrikası dizinini oluşturmaz. Örnek test etmek için bu ada sahip bir dizin oluşturun. İle aynı sayıda sütun girdi veri kümesi olduğu gibi Azure Search dizini oluşturma. Yeni girişler için Azure Search dizini saatte eklenir.

```JSON
{
    "name": "AzureSearchIndexDataset",
    "properties": {
        "type": "AzureSearchIndex",
        "linkedServiceName": "AzureSearchLinkedService",
        "typeProperties" : {
            "indexName": "products",
        },
        "availability": {
            "frequency": "Minute",
            "interval": 15
        }
   }
}
```

**SQL kaynak ve Azure Search dizini havuz ile ardışık düzeninde etkinlik kopyalayın:**

Ardışık Düzen giriş ve çıkış veri kümeleri kullanmak üzere yapılandırıldığı ve saatte çalışacak şekilde zamanlanır kopyalama etkinliği içerir. JSON tanımını düzenindeki **kaynak** türü ayarlanmış **SqlSource** ve **havuz** türü ayarlanmış **AzureSearchIndexSink**. SQL sorgusu için belirtilen **SqlReaderQuery** özelliği veri kopyalamak için son bir saat içindeki seçer.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "SqlServertoAzureSearchIndex",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": " SqlServerInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSearchIndexDataset"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "AzureSearchIndexSink"
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

Azure Search bir bulut veri deposundan veri kopyalıyorsanız `executionLocation` özelliği gereklidir. Kopyalama etkinliği altında gerekli değişiklik aşağıdaki JSON parçacığı gösterir `typeProperties` bir örnek olarak. Denetleme [bulut veri depoları arasında veri kopyalama](data-factory-data-movement-activities.md#global) desteklenen değerler ve daha fazla ayrıntı için bölüm.

```JSON
"typeProperties": {
  "source": {
    "type": "BlobSource"
  },
  "sink": {
    "type": "AzureSearchIndexSink"
  },
  "executionLocation": "West US"
}
```


## <a name="copy-from-a-cloud-source"></a>Bir bulut kaynaktan kopyalama
Azure Search bir bulut veri deposundan veri kopyalıyorsanız `executionLocation` özelliği gereklidir. Kopyalama etkinliği altında gerekli değişiklik aşağıdaki JSON parçacığı gösterir `typeProperties` bir örnek olarak. Denetleme [bulut veri depoları arasında veri kopyalama](data-factory-data-movement-activities.md#global) desteklenen değerler ve daha fazla ayrıntı için bölüm.

```JSON
"typeProperties": {
  "source": {
    "type": "BlobSource"
  },
  "sink": {
    "type": "AzureSearchIndexSink"
  },
  "executionLocation": "West US"
}
```

Ayrıca havuz dataset kopyalama etkinliği tanımında sütunları kaynak kümesinden sütunları eşleyebilirsiniz. Ayrıntılar için bkz [Azure Data Factory veri kümesi sütunlarında eşleme](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Performans ve ayar  
Bkz: [kopyalama etkinliği performans ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md) etkisi performans veri hareketlerini (kopyalama etkinliği) ve bunu iyileştirmek için çeşitli yollar hakkında önemli faktör öğrenin.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

* [Kopya etkinliği öğretici](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak için adım adım yönergeler için.
