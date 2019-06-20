---
title: Data Factory kullanarak verileri Amazon Basit Depolama hizmetinden taşıma | Microsoft Docs
description: Verileri Amazon Basit Depolama hizmetinden (S3 için) Azure Data Factory kullanarak taşıma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 636d3179-eba8-4841-bcb4-3563f6822a26
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 1f5064cece32cfc38f149816961e5156ff20974a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60335343"
---
# <a name="move-data-from-amazon-simple-storage-service-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Amazon Basit Depolama hizmetinden veri taşıma
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](data-factory-amazon-simple-storage-service-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-amazon-simple-storage-service.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [V2 Amazon S3 bağlayıcısında](../connector-amazon-simple-storage-service.md).

Bu makalede, Amazon Basit Depolama hizmetinden (S3 için) verileri taşımak için Azure Data Factory kopyalama etkinliği kullanmayı açıklar. Yapılar [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği ile verileri taşıma genel bir bakış sunar.

Verileri Amazon S3'ten herhangi bir desteklenen havuz veri deposuna kopyalayabilirsiniz. Havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablo. Data Factory şu anda yalnızca verileri Amazon S3 diğer veri depolarına destekler, ancak diğer veriler veri taşıma değil Amazon S3 olarak depolar.

## <a name="required-permissions"></a>Gerekli izinler
Verileri Amazon S3'ten kopyalamak için aşağıdaki izinleri verilmiş olan emin olun:

* `s3:GetObject` ve `s3:GetObjectVersion` Amazon S3 nesnesinin işlemleri için.
* `s3:ListBucket` Amazon S3 Demetini işlemleri için. Data Factory Kopyalama Sihirbazı'nı kullanıyorsanız `s3:ListAllMyBuckets` de gereklidir.

Amazon S3 izinlerin tam listesi hakkında daha fazla ayrıntı için bkz: [izinleri belirten bir ilke](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).

## <a name="getting-started"></a>Başlarken
Farklı araçlar veya API'leri kullanarak bir Amazon S3 kaynaktan veri taşıyan kopyalama etkinliği ile işlem hattı oluşturabilirsiniz.

Bir işlem hattı oluşturmanın en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Hızlı bir kılavuz için bkz. [Öğreticisi: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md).

Ayrıca, bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portalında**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve  **REST API**. Kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Araç veya API'lerden kullanmanıza bakılmaksızın, bir havuz veri deposu için bir kaynak veri deposundan veri taşıyan bir işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma **bağlı hizmetler** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar.
2. Oluşturma **veri kümeleri** kopyalama işleminin girdi ve çıktı verilerini göstermek için.
3. Oluşturma bir **işlem hattı** bir veri kümesini girdi ve çıktı olarak bir veri kümesini alan kopyalama etkinliği ile.

Sihirbazı'nı kullandığınızda, bu Data Factory varlıklarını (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araç veya API'lerden (dışında .NET API'si) kullandığınızda, bu Data Factory varlıkları JSON biçimini kullanarak tanımlayın. Bir Amazon S3 veri deposundan veri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları ile bir örnek için bkz. [JSON örneği: Verileri Amazon S3'ten Azure Blob kopyalama](#json-example-copy-data-from-amazon-s3-to-azure-blob-storage) bu makalenin.

> [!NOTE]
> Kopyalama etkinliği için desteklenen dosya ve sıkıştırma biçimleri hakkında daha fazla ayrıntı için bkz: [dosya ve sıkıştırma biçimleri Azure Data factory'de](data-factory-supported-file-and-compression-formats.md).

Aşağıdaki bölümler, Amazon S3 belirli Data Factory varlıkları tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri
Bağlı hizmet, bir veri deposuna bir veri fabrikasına bağlar. Bağlı hizmet türü oluşturma **AwsAccessKey** veri fabrikanıza Amazon S3 veri deponuza bağlamak için. Aşağıdaki tabloda, Amazon S3 JSON öğelerinin belirli bir açıklama sağlar (AwsAccessKey) bağlı hizmeti.

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| accessKeyID |Gizli erişim anahtarı kimliği. |string |Evet |
| secretAccessKey |Gizli erişim anahtarı kendisi. |Şifrelenmiş gizli dize |Evet |

>[!NOTE]
>Bu bağlayıcı, Amazon S3'ten veri kopyalamak IAM hesabının erişim anahtarlarını gerektirir. [Geçici güvenlik kimlik bilgisi](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) desteklenmiyor.
>

Örnek aşağıda verilmiştir:

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Girdi verilerini Azure Blob Depolama alanında temsil eden bir veri kümesi belirtmek için veri kümesine öğesinin type özelliği ayarlamak **AmazonS3**. Ayarlama **linkedServiceName** özelliği adı Amazon S3 veri kümesine bağlı hizmeti. Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md). 

Tüm veri kümesi türleri (örneğin, SQL veritabanı, Azure blob ve Azure tablo) için yapı, kullanılabilirlik ve ilke gibi bölümlere benzerdir. **TypeProperties** bölümünde her veri kümesi türü için farklıdır ve verilerin veri deposundaki konumu hakkında bilgi sağlar. **TypeProperties** türü için bir veri kümesi bölümünü **AmazonS3** (Amazon S3 veri kümesi içeren) aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| bucketName |S3 demetini adı. |String |Evet |
| key |S3 nesnesinin anahtarı. |String |Hayır |
| prefix |S3 nesnesinin anahtarı için önek. Seçili bir nesne anahtarları bu öneki ile başlayın. Yalnızca anahtar boş olduğunda geçerlidir. |String |Hayır |
| version |S3 sürümü oluşturma etkinse, S3 nesnesinin sürümü. |String |Hayır |
| format | Şu biçim türlerini destekler: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** özelliği şu değerlerden biri olarak biçimine altında. Daha fazla bilgi için [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [JSON biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquet biçimi ](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. <br><br> Dosyaları olarak kopyalamak istiyorsanız-olan dosya temelli deposu arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımları biçimi bölümünde atlayın. |Hayır | |
| compression | Veri sıkıştırma düzeyi ve türünü belirtin. Desteklenen türler şunlardır: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**. Desteklenen düzeyleri şunlardır: **En iyi** ve **hızlı**. Daha fazla bilgi için [dosya ve sıkıştırma biçimleri Azure Data factory'de](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır | |


> [!NOTE]
> **bucketName + anahtar** konumu S3 nesnesinin burada demetine S3 nesneleri için kök kapsayıcı ve anahtardır S3 nesnesinin tam yolunu belirtir.

### <a name="sample-dataset-with-prefix"></a>Örnek veri kümesiyle ön eki

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "prefix": "testFolder/test",
            "bucketName": "testbucket",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
### <a name="sample-dataset-with-version"></a>Örnek veri kümesiyle (sürüm)

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "key": "testFolder/test.orc",
            "bucketName": "testbucket",
            "version": "XXXXXXXXXczm0CJajYkHf0_k6LhBmkcL",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

### <a name="dynamic-paths-for-s3"></a>S3 için dinamik yolları
Önceki örnekte sabit değerleri için kullandığı **anahtarı** ve **bucketName** Amazon S3 veri kümesi özellikleri.

```json
"key": "testFolder/test.orc",
"bucketName": "testbucket",
```

Data Factory, bu özellikler zamanında dinamik olarak, sistem değişkenlerini SliceStart gibi hesaplamak olabilir.

```json
"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"
```

Aynı yapabileceğiniz **önek** bir Amazon S3 veri kümesi özelliği. Desteklenen işlevler ve değişkenler listesi için bkz. [Data Factory işlevleri ve sistem değişkenleri](data-factory-functions-variables.md).

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [komut zincirleri oluşturma](data-factory-create-pipelines.md). Ad, açıklama, girdi ve çıktı tabloları ve ilkeleri gibi özellikler, tüm etkinlik türleri için kullanılabilir. Bulunan özelliklerin **typeProperties** etkinlik bölümünü her etkinlik türü ile farklılık gösterir. Kopyalama etkinliği için özellikler, kaynaklar ve havuzlar türlerine bağlı olarak farklılık gösterir. Bir kopyalama etkinliği kaynağı türünde olduğunda **FileSystemSource** (Amazon S3 içeren), aşağıdaki özellikler kullanılabilir **typeProperties** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| recursive |Yinelemeli olarak S3 listesinde olup olmadığını belirtir dizinin altındaki nesneleri. |true/false |Hayır |

## <a name="json-example-copy-data-from-amazon-s3-to-azure-blob-storage"></a>JSON örneği: Verileri Amazon S3'ten Azure Blob depolama alanına kopyalayın.
Bu örnek verileri Amazon S3'ten bir Azure Blob depolama alanına kopyalamak nasıl gösterir. Ancak, veriler doğrudan kopyalanabilir [herhangi bir desteklenen havuzlarını](data-factory-data-movement-activities.md#supported-data-stores-and-formats) veri fabrikasında kopyalama etkinliği kullanarak.

Örnek, aşağıdaki Data Factory varlıkları için JSON tanımları sağlar. Verileri Amazon S3'ten kullanarak Blob depolama alanına kopyalamak için bir işlem hattı oluşturmak için bu tanımları kullanabilirsiniz [Azure portalında](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), veya [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).   

* Bağlı hizmet türü [AwsAccessKey](#linked-service-properties).
* Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Girdi [veri kümesi](data-factory-create-datasets.md) türü [AmazonS3](#dataset-properties).
* Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* A [işlem hattı](data-factory-create-pipelines.md) kullanan kopyalama etkinlikli [FileSystemSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Örnek verileri Amazon S3'ten Azure blobuna saatte kopyalar. Bu örneklerde kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

### <a name="amazon-s3-linked-service"></a>Amazon S3'bağlı hizmeti

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a>Azure Storage bağlı hizmeti

```json
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

### <a name="amazon-s3-input-dataset"></a>Amazon S3 giriş veri kümesi

Ayarı **"dış": true** Data Factory hizmetinin veri kümesini veri fabrikasına dış olduğunu bildirir. Bu özellik, işlem hattındaki bir etkinliğin oluşturulmuyor bir giriş veri kümesi üzerinde true olarak ayarlayın.

```json
    {
        "name": "AmazonS3InputDataset",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "AmazonS3LinkedService",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }
```


### <a name="azure-blob-output-dataset"></a>Azure Blob çıktı veri kümesi

Veriler her saat yeni bir bloba yazılır (Sıklık: saat, interval: 1). Blob için klasör yolu işlenmekte olan dilimin başlangıç zamanı temel alınarak dinamik olarak değerlendirilir. Yıl, ay, gün ve saat bölümlerini başlangıç zamanı klasör yolu kullanır.

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/fromamazons3/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
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
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```


### <a name="copy-activity-in-a-pipeline-with-an-amazon-s3-source-and-a-blob-sink"></a>Amazon S3 kaynağı ve havuz blob ile bir işlem hattındaki kopyalama etkinliği

İşlem hattının giriş ve çıkış veri kümelerini kullanmak için yapılandırıldığı ve saatte bir çalışacak şekilde zamanlanmış bir kopyalama etkinliği içeriyor. JSON tanımı, işlem hattındaki **kaynak** türü ayarlandığında **FileSystemSource**, ve **havuz** türü ayarlandığında **BlobSink**.

```json
{
    "name": "CopyAmazonS3ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "FileSystemSource",
                        "recursive": true
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "AmazonS3InputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutputDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "AmazonS3ToBlob"
            }
        ],
        "start": "2014-08-08T18:00:00Z",
        "end": "2014-08-08T19:00:00Z"
    }
}
```
> [!NOTE]
> Sütunları havuz veri kümesi için bir kaynak veri kümesindeki sütunları eşlemek için bkz: [Azure Data factory'de veri kümesi sütunlarını eşleme](data-factory-map-columns.md).


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

* Veri taşıma (kopyalama etkinliği) Data Factory ve bunu en iyi duruma getirmek için çeşitli yollar, performansı etkileyebilir anahtar Etkenler hakkında bilgi edinmek için [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md).

* Kopyalama etkinliği ile işlem hattı oluşturmak için adım adım yönergeler için bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
