---
title: Veri taşıma/Azure Cosmos DB'den | Microsoft Docs
description: Bilgi nasıl veri taşıma/Azure Data Factory kullanarak Azure Cosmos DB koleksiyonundan
services: data-factory, cosmosdb
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: c9297b71-1bb4-4b29-ba3c-4cf1f5575fac
ms.service: multiple
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: d0897f73ed1a321c8287729eaba775a625f51e4d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34620995"
---
# <a name="move-data-to-and-from-azure-cosmos-db-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Cosmos DB gelen ve veri taşıma
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](data-factory-azure-documentdb-connector.md)
> * [Sürüm 2 - Önizleme](../connector-azure-cosmos-db.md)

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [V2 Azure Cosmos DB Bağlayıcısı](../connector-azure-cosmos-db.md).

Bu makalede kopya etkinliği Azure Data Factory'de öğesine/öğesinden Azure Cosmos DB (SQL API) verileri taşımak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) kopyalama etkinliği ile veri taşıma için genel bir bakış sunar makalesi. 

Verileri Azure Cosmos DB'de tüm desteklenen kaynak veri deposundan veya Azure Cosmos DB tüm desteklenen havuz veri deposuna kopyalayabilirsiniz. Kaynakları veya havuzlarını olarak kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablo. 

> [!IMPORTANT]
> Azure Cosmos DB Bağlayıcısı, yalnızca SQL API destekler.

Veri olarak kopyalamak için-olduğu için/JSON dosyalarında veya başka bir Cosmos DB koleksiyonu, bkz: [içeri/dışarı aktarma JSON belgeleri](#importexport-json-documents).

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'lerini kullanarak verileri öğesine/öğesinden Azure Cosmos DB taşır kopyalama etkinliği ile işlem hattı oluşturun.

Bir işlem hattı oluşturmak için en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Bkz: [öğretici: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) veri kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma Hızlı Kılavuz.

Bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**, ve **REST API**. Bkz: [kopyalama etkinliği öğretici](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için. 

Araçlar ya da API'leri kullanıp bir havuz veri deposu için bir kaynak veri deposundan verileri taşır bir ardışık düzen oluşturmak için aşağıdaki adımları gerçekleştirin: 

1. Oluşturma **bağlantılı Hizmetleri** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar.
2. Oluşturma **veri kümeleri** kopyalama işlemi için girdi ve çıktı verilerini temsil etmek için. 
3. Oluşturma bir **ardışık düzen** bir giriş olarak bir veri kümesi ve bir veri kümesini çıktı olarak alan kopyalama etkinliği ile. 

Sihirbazı'nı kullandığınızda, bu Data Factory varlıkları (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar/API'leri (dışında .NET API'si) kullandığınızda, JSON biçimini kullanarak bu Data Factory varlıklarını tanımlayın.  / Cosmos DB'den veri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımlarıyla örnekleri için bkz: [JSON örnekler](#json-examples) bu makalenin. 

Aşağıdaki bölümler Cosmos DB Data Factory varlıklarını belirli tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar: 

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Aşağıdaki tabloda Azure Cosmos DB bağlantılı hizmete özgü JSON öğelerini açıklamasını sağlar.

| **Özellik** | **Açıklama** | **Gerekli** |
| --- | --- | --- |
| type |Type özelliği ayarlanmalıdır: **DocumentDb** |Evet |
| connectionString |Azure Cosmos DB veritabanına bağlanmak için gereken bilgileri belirtin. |Evet |

Örnek:

```JSON
{
  "name": "CosmosDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Bölümler & özellikleri veri kümeleri tanımlamak için kullanılabilir tam bir listesi için lütfen [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bölümler yapısı, kullanılabilirlik ve bir veri kümesi JSON İlkesi gibi tüm veri türleri (Azure SQL, Azure blob, Azure tablo, vs.) için benzer.

TypeProperties bölümü dataset her tür için farklıdır ve verilerin veri deposunda konumu hakkında bilgi sağlar. TypeProperties bölüm türü veri kümesi için **DocumentDbCollection** aşağıdaki özelliklere sahiptir.

| **Özellik** | **Açıklama** | **Gerekli** |
| --- | --- | --- |
| collectionName |Cosmos DB belge koleksiyonunun adı. |Evet |

Örnek:

```JSON
{
  "name": "PersonCosmosDbTable",
  "properties": {
    "type": "DocumentDbCollection",
    "linkedServiceName": "CosmosDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
### <a name="schema-by-data-factory"></a>Şema tarafından veri fabrikası
Azure Cosmos DB gibi şemasız veri depoları için Data Factory hizmetinin şema aşağıdaki yollardan biriyle oluşturur:  

1. Verilerin yapısını kullanarak belirtirseniz **yapısı** özelliği veri kümesi tanımında Data Factory hizmetinin bu yapısı şema olarak geliştirir. Bir satır bir sütun için bir değer içermiyorsa, bu durumda, boş bir değer için sağlanır.
2. Verilerin yapısını kullanarak belirtmezseniz **yapısı** özelliği veri kümesi tanımında Data Factory hizmetinin oluşturur şema verileri ilk satırını kullanarak. Bu durumda, ilk satırın tam şema içermiyorsa, bazı sütunları kopyalama işlemi sonucunda eksik olacaktır.

Bu nedenle, şemasız veri kaynakları için en iyi uygulama verileri kullanarak yapısı belirtmektir **yapısı** özelliği.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümler & özellikleri etkinlikleri tanımlamak için kullanılabilir tam bir listesi için lütfen [oluşturma ardışık düzen](data-factory-create-pipelines.md) makalesi. Ad, açıklama, giriş ve çıkış tabloları ve ilke gibi özellikler etkinlikleri tüm türleri için kullanılabilir.

> [!NOTE]
> Kopyalama etkinliği yalnızca bir girdi alır ve tek bir çıktı üretir.

Etkinliğin typeProperties bölümündeki özellikler diğer yandan her etkinlik türü ile farklı ve kopyalama etkinliği durumunda bunlar türlerini kaynakları ve havuzlarını bağlı olarak farklılık gösterir.

Kaynak türü olduğunda kopyalama etkinliği durumunda **DocumentDbCollectionSource** aşağıdaki özellikler mevcuttur **typeProperties** bölümü:

| **Özellik** | **Açıklama** | **İzin verilen değerler** | **Gerekli** |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için sorgu belirtin. |Sorgu dizesindeki Azure Cosmos DB tarafından desteklenir. <br/><br/>Örnek: `SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Hayır <br/><br/>Belirtilmezse, yürütülen SQL deyimi: `select <columns defined in structure> from mycollection` |
| nestingSeparator |Belge iç içe geçmiş belirtmek için özel karakter |Herhangi bir karakter. <br/><br/>Azure Cosmos DB iç içe geçmiş yapılar burada izin verilen bir NoSQL JSON belgeleri için deposudur. Azure Data Factory sağlar hiyerarşisi olan nestingSeparator aracılığıyla belirtmek kullanıcı "." Yukarıdaki örneklerde. Ayırıcı olmadan kopyalama etkinliği üç alt öğeleri "Ad" nesnesiyle ilk olarak, oluşturacak Orta ve son "Name.First", "Name.Middle" ve "Name.Last" Tablo tanımında göre. |Hayır |

**DocumentDbCollectionSink** aşağıdaki özellikleri destekler:

| **Özellik** | **Açıklama** | **İzin verilen değerler** | **Gerekli** |
| --- | --- | --- | --- |
| nestingSeparator |Bir özel karakter iç içe geçmiş belge belirtmek için kaynak sütun adı gereklidir. <br/><br/>Örneğin yukarıdaki: `Name.First` çıktıda tablo Cosmos DB belgede aşağıdaki JSON yapısını oluşturur:<br/><br/>"Name": {<br/>    "İlk": "John"<br/>}, |İç içe geçme düzeylerini ayırmak için kullanılan karakterdir.<br/><br/>Varsayılan değer `.` (nokta). |İç içe geçme düzeylerini ayırmak için kullanılan karakterdir. <br/><br/>Varsayılan değer `.` (nokta). |
| writeBatchSize |Azure Cosmos DB hizmeti belgeleri oluşturmak için paralel isteklerin sayısı.<br/><br/>Bu özelliği kullanarak verileri için/Cosmos DB kopyalama işlemi sırasında performans ince ayar yapabilirsiniz. Daha fazla paralel isteklerine Cosmos DB gönderildiğinden writeBatchSize artırdığınızda daha iyi bir performans düşüklüğü görebilir. Ancak, azaltma önlemek gerekir, hata iletisi atabilirsiniz: "oranıdır büyük istek".<br/><br/>Azaltmayı bir dizi etkene, belgeler, belgeleri koşullarını sayısı boyutunu dahil olmak üzere, hedef koleksiyon, vb. İlkesi dizin tarafından belirlenir. Kopyalama işlemleri için en çok kullanılabilir verimlilik sağlamak için daha iyi bir koleksiyonunu (örneğin S3) kullanabilirsiniz (2.500 istek birimleri/saniye). |Tamsayı |Hayır (varsayılan: 5) |
| writeBatchTimeout |İşlemin zaman aşımına uğramadan önce tamamlanması için bir süre bekleyin. |TimeSpan<br/><br/> Örnek: "00: 30:00" (30 dakika). |Hayır |

## <a name="importexport-json-documents"></a>İçeri/dışarı aktarma JSON belgeleri
Bu Cosmos DB Bağlayıcısı'nı kullanarak kolayca yapabilecekleriniz

* JSON belgeleri Cosmos DB, Azure Blob, Azure Data Lake, şirket içi dosya sistemi veya Azure Data Factory ile desteklenen diğer dosya tabanlı depoları gibi çeşitli kaynaklardan aktarın.
* JSON belgeleri Cosmos DB collecton çeşitli dosya tabanlı depoları verin.
* İki Cosmos DB koleksiyon olarak arasında veri geçirme-değil.

Bu tür şema belirsiz kopya elde etmek için 
* Kopyalama Sihirbazı'nı kullanırken denetleyin **"dışa-JSON dosyaları ya da Cosmos DB koleksiyonu"** seçeneği.
* Ne zaman, JSON düzenleme kullanarak belirtme "yapısı" bölümü Cosmos DB veri kümeleri içinde ya da "nestingSeparator" özelliği Cosmos DB kaynak/havuz kopyalama etkinliği. Gelen içeri / dışarı aktarma için JSON dosyaları için dosya depolama kümesinde "JsonFormat", yapılandırma "filePattern" biçim türünü belirtin ve rest biçimi ayarları Atla, bakın [JSON biçimine](data-factory-supported-file-and-compression-formats.md#json-format) ayrıntıları bölümü.

## <a name="json-examples"></a>JSON örnekleri
Aşağıdaki örnekleri kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) veya [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Bunlar ve Azure Cosmos DB ve Azure Blob Storage veri kopyalamak nasıl gösterir. Ancak, veriler kopyalanabilir **doğrudan** herhangi birinden herhangi birine belirtildiği havuzlarını kaynakları [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopya etkinliği Azure Data Factory kullanarak.

## <a name="example-copy-data-from-azure-cosmos-db-to-azure-blob"></a>Örnek: verileri Azure Cosmos DB'den Azure Blob kopyalama
Aşağıdaki örneği gösterilmektedir:

1. Bağlı hizmet türü [DocumentDb](#linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bir giriş [dataset](data-factory-create-datasets.md) türü [DocumentDbCollection](#dataset-properties).
4. Bir çıkış [dataset](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [DocumentDbCollectionSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Örnek verileri Azure Blob Azure Cosmos DB'de kopyalar. Bu örnekler kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

**Azure Cosmos bağlı DB hizmeti:**

```JSON
{
  "name": "CosmosDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
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
**Azure belge DB girdi veri kümesi:**

Örnek adlı bir koleksiyon olduğu varsayılır **kişi** Azure Cosmos DB veritabanında.

"Dış" ayarı: "true" ve tablo data factory dış ve veri fabrikasında bir etkinlik tarafından üretilen değil externalData ilke bilgilerini Azure Data Factory hizmetinin belirtme.

```JSON
{
  "name": "PersonCosmosDbTable",
  "properties": {
    "type": "DocumentDbCollection",
    "linkedServiceName": "CosmosDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Azure Blob dataset çıktı:**

Veri yolu ile her saat saat ayrıntı belirli datetime yansıtma blob için yeni bir blob kopyalanır.

```JSON
{
  "name": "PersonBlobTableOut",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "docdb",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "nullValue": "NULL"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
Örnek JSON belgesi Cosmos DB veritabanında kişi koleksiyonundaki:

```JSON
{
  "PersonId": 2,
  "Name": {
    "First": "Jane",
    "Middle": "",
    "Last": "Doe"
  }
}
```
Cosmos DB SQL söz dizimi gibi hiyerarşik JSON belgelerini kullanarak belgelerin sorgulanmasını destekler.

Örnek: 

```sql
SELECT Person.PersonId, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person
```

Aşağıdaki işlem hattı verileri Azure Cosmos DB veritabanı kişi koleksiyonunda bir Azure blob kopyalar. Veri kümeleri parçası kopyalama etkinliği giriş ve çıkış belirtildi.  

```JSON
{
  "name": "DocDbToBlobPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "DocumentDbCollectionSource",
            "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
            "nestingSeparator": "."
          },
          "sink": {
            "type": "BlobSink",
            "blobWriterAddHeader": true,
            "writeBatchSize": 1000,
            "writeBatchTimeout": "00:00:59"
          }
        },
        "inputs": [
          {
            "name": "PersonCosmosDbTable"
          }
        ],
        "outputs": [
          {
            "name": "PersonBlobTableOut"
          }
        ],
        "policy": {
          "concurrency": 1
        },
        "name": "CopyFromDocDbToBlob"
      }
    ],
    "start": "2015-04-01T00:00:00Z",
    "end": "2015-04-02T00:00:00Z"
  }
}
```
## <a name="example-copy-data-from-azure-blob-to-azure-cosmos-db"></a>Örnek: verileri Azure Blob'tan Azure Cosmos DB kopyalayın 
Aşağıdaki örneği gösterilmektedir:

1. Bağlı hizmet türü [DocumentDb](#azure-documentdb-linked-service-properties).
2. Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bir giriş [dataset](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Bir çıkış [dataset](data-factory-create-datasets.md) türü [DocumentDbCollection](#azure-documentdb-dataset-type-properties).
5. A [ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) ve [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).

Örnek verileri Azure'dan kopyalar blob Azure Cosmos DB'de. Bu örnekler kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

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
**Azure Cosmos bağlı DB hizmeti:**

```JSON
{
  "name": "CosmosDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```
**Azure Blob girdi veri kümesi:**

```JSON
{
  "name": "PersonBlobTableIn",
  "properties": {
    "structure": [
      {
        "name": "Id",
        "type": "Int"
      },
      {
        "name": "FirstName",
        "type": "String"
      },
      {
        "name": "MiddleName",
        "type": "String"
      },
      {
        "name": "LastName",
        "type": "String"
      }
    ],
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "fileName": "input.csv",
      "folderPath": "docdb",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "nullValue": "NULL"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
**Azure Cosmos DB çıkış dataset:**

Örnek verileri "Kişi" adlı bir koleksiyon için kopyalar.

```JSON
{
  "name": "PersonCosmosDbTableOut",
  "properties": {
    "structure": [
      {
        "name": "Id",
        "type": "Int"
      },
      {
        "name": "Name.First",
        "type": "String"
      },
      {
        "name": "Name.Middle",
        "type": "String"
      },
      {
        "name": "Name.Last",
        "type": "String"
      }
    ],
    "type": "DocumentDbCollection",
    "linkedServiceName": "CosmosDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
Aşağıdaki işlem hattı verileri Azure Blob'tan Cosmos DB kişi koleksiyonunda kopyalar. Veri kümeleri parçası kopyalama etkinliği giriş ve çıkış belirtildi.

```JSON
{
  "name": "BlobToDocDbPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "DocumentDbCollectionSink",
            "nestingSeparator": ".",
            "writeBatchSize": 2,
            "writeBatchTimeout": "00:00:00"
          }
          "translator": {
              "type": "TabularTranslator",
              "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, Title: Title, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
          }
        },
        "inputs": [
          {
            "name": "PersonBlobTableIn"
          }
        ],
        "outputs": [
          {
            "name": "PersonCosmosDbTableOut"
          }
        ],
        "policy": {
          "concurrency": 1
        },
        "name": "CopyFromBlobToDocDb"
      }
    ],
    "start": "2015-04-14T00:00:00Z",
    "end": "2015-04-15T00:00:00Z"
  }
}
```
Örnek blob giriş olarak ise

```
1,John,,Doe
```
Ardından çıktı Cosmos DB'de JSON olarak olacaktır:

```JSON
{
  "Id": 1,
  "Name": {
    "First": "John",
    "Middle": null,
    "Last": "Doe"
  },
  "id": "a5e8595c-62ec-4554-a118-3940f4ff70b6"
}
```
Azure Cosmos DB iç içe geçmiş yapılar burada izin verilen bir NoSQL JSON belgeleri için deposudur. Azure Data Factory sağlar hiyerarşisi aracılığıyla belirtmek kullanıcı **nestingSeparator**, olduğu "." Bu örnekte. Ayırıcı olmadan kopyalama etkinliği üç alt öğeleri "Ad" nesnesiyle ilk olarak, oluşturacak Orta ve son "Name.First", "Name.Middle" ve "Name.Last" Tablo tanımında göre.

## <a name="appendix"></a>Ek
1. **Soru:** kopyalama etkinliği desteği güncelleştirmesi mevcut kayıtların mu?

    **Yanıt:** No
2. **Soru:** kayıtları kopyalanan nasıl bir kopya ile Azure Cosmos DB anlaşma için bir yeniden deneme zaten mu?

    **Yanıt:** kayıtları "ID" alanına sahipseniz ve aynı Kimliğe sahip bir kayıt eklemek kopyalama işlemi çalışırsa, kopyalama işlemi bir hata oluşturur.  
3. **Soru:** Data Factory desteklemiyor [aralığı veya karma tabanlı veri bölümlendirme](../../cosmos-db/sql-api-partition-data.md)?

    **Yanıt:** No
4. **Soru:** bir tablo için birden fazla Azure Cosmos DB koleksiyonu belirtin?

    **Yanıt:** No Şu anda yalnızca bir koleksiyon belirtilebilir.

## <a name="performance-and-tuning"></a>Performans ve ayarlama
Bkz: [kopya etkinliği performansının & ayarlama Kılavuzu](data-factory-copy-activity-performance.md) bu veri taşıma (kopyalama etkinliği) Azure Data Factory ve onu en iyi duruma getirmek için çeşitli yollar etkisi performansını anahtar Etkenler hakkında bilgi edinmek için.
