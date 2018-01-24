---
title: "Veri Fabrikası kullanarak Amazon Basit Depolama hizmetinden veri taşıma | Microsoft Docs"
description: "Azure Data Factory kullanarak Amazon Basit Depolama hizmetinden (S3) veri taşıma hakkında bilgi edinin."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 636d3179-eba8-4841-bcb4-3563f6822a26
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: fb2b534955a2cd0e1294df5425550ac6958ff3c2
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="move-data-from-amazon-simple-storage-service-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Amazon Basit Depolama hizmetinden veri taşıma
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](data-factory-amazon-simple-storage-service-connector.md)
> * [Sürüm 2 - Önizleme](../connector-amazon-simple-storage-service.md)

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [V2 Amazon S3 Bağlayıcısı](../connector-amazon-simple-storage-service.md).

Bu makalede kopya etkinliği Azure Data Factory'de verileri Amazon Basit Depolama hizmetinden (S3) taşımak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) kopyalama etkinliği ile veri taşıma için genel bir bakış sunar makalesi.

Verileri Amazon S3'ten herhangi desteklenen havuz veri deposuna kopyalayabilirsiniz. Veri depoları havuzlarını kopyalama etkinliği tarafından desteklenen bir listesi için bkz: [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablo. Veri Fabrikası şu anda yalnızca taşıma Amazon S3 verileri diğer veri depolarına destekler, ancak verileri diğer veriler taşıma değil Amazon S3 depolar.

## <a name="required-permissions"></a>Gerekli izinler
Amazon S3'ten verileri kopyalamak için aşağıdaki izinleri verilmiş olan emin olun:

* `s3:GetObject`ve `s3:GetObjectVersion` Amazon S3 nesne işlemleri için.
* `s3:ListBucket`Amazon S3 Demetini işlemleri için. Data Factory Kopyalama Sihirbazı'nı kullanıyorsanız `s3:ListAllMyBuckets` de gereklidir.

Amazon S3 izinlerin tam listesi hakkında daha fazla ayrıntı için bkz: [belirleyen izinleri bir ilke](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).

## <a name="getting-started"></a>Başlarken
Farklı araçlar veya API'lerini kullanarak bir Amazon S3 kaynaktan verileri taşır kopyalama etkinliği ile işlem hattı oluşturun.

Bir işlem hattı oluşturmak için en kolay yolu kullanmaktır **Kopyalama Sihirbazı'nı**. Hızlı bir kılavuz için bkz: [öğretici: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md).

Bir işlem hattı oluşturmak için aşağıdaki araçları kullanabilirsiniz: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**, ve **REST API**. Kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Araçları veya API'ler kullanıp bir havuz veri deposu için bir kaynak veri deposundan verileri taşır bir ardışık düzen oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Oluşturma **bağlantılı Hizmetleri** girdi ve çıktı verilerini bağlamak için veri fabrikanıza depolar.
2. Oluşturma **veri kümeleri** kopyalama işlemi için girdi ve çıktı verilerini temsil etmek için.
3. Oluşturma bir **ardışık düzen** bir giriş olarak bir veri kümesi ve bir veri kümesini çıktı olarak alan kopyalama etkinliği ile.

Sihirbazı'nı kullandığınızda, bu Data Factory varlıkları (bağlı hizmetler, veri kümeleri ve işlem hattı) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçları veya API'ler (dışında .NET API'si) kullandığınızda, JSON biçimini kullanarak bu Data Factory varlıklarını tanımlayın. Bir Amazon S3 veri deposundan verileri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları içeren bir örnek için bkz: [JSON örnek: veri kopyalama Amazon S3'ten Azure Blob](#json-example-copy-data-from-amazon-s3-to-azure-blob-storage) bu makalenin.

> [!NOTE]
> Kopyalama etkinliği için desteklenen dosya ve sıkıştırma biçimleri hakkında daha fazla ayrıntı için bkz: [Azure Data Factory dosya ve sıkıştırma biçimlerde](data-factory-supported-file-and-compression-formats.md).

Aşağıdaki bölümler, Amazon S3 Data Factory varlıklarını belirli tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Bağlı hizmet, veri fabrikası için bir veri deposu bağlar. Bağlı hizmet türü oluşturma **AwsAccessKey** veri fabrikanıza Amazon S3 veri deponuza bağlamak için. Aşağıdaki tabloda, Amazon S3 JSON öğeleri belirli bir açıklamasını sağlar (AwsAccessKey) bağlı hizmeti.

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| accessKeyID |Gizli erişim anahtarı kimliği. |dize |Evet |
| secretAccessKey |Gizli erişim anahtar kendisi. |Şifrelenmiş gizli dize |Evet |

>[!NOTE]
>Bu bağlayıcı, Amazon S3'ten verileri kopyalamak IAM hesabı için erişim anahtarlarını gerektirir. [Geçici güvenlik kimlik bilgisi](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) desteklenmiyor.
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
Girdi verileri Azure Blob Depolama alanında temsil etmek üzere bir veri kümesi belirtmek için veri kümesine tür özelliği ayarlayın **AmazonS3**. Ayarlama **linkedServiceName** Amazon S3 adını dataset özelliğinin bağlı hizmeti. Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [veri kümeleri oluşturma](data-factory-create-datasets.md). 

Bölümler yapısı, kullanılabilirlik ve ilke gibi tüm veri türleri (örneğin, SQL database, Azure blob ve Azure tablo) benzer. **TypeProperties** bölüm veri kümesi her tür için farklıdır ve verilerin veri deposunda konumu hakkında bilgi sağlar. **TypeProperties** bir veri kümesi için bir bölüm türü **AmazonS3** (Amazon S3 dataset içerir) aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| bucketName |S3 demetini adı. |Dize |Evet |
| anahtar |S3 nesne anahtarı. |Dize |Hayır |
| önek |S3 nesne anahtarı için önek. Seçilen nesneler, anahtarları Bu önek ile başlatın. Yalnızca anahtar boş olduğunda geçerlidir. |Dize |Hayır |
| sürüm |S3 sürüm etkinleştirilirse S3 nesne sürümü. |Dize |Hayır |
| Biçimi | Şu biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** şu değerlerden biri biçimine altında özellik. Daha fazla bilgi için bkz: [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [JSON biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquet biçimi ](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. <br><br> Dosyaları olarak kopyalamak istiyorsanız-olan dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımlarında Biçim bölümü atlayın. |Hayır | |
| Sıkıştırma | Veri sıkıştırma düzeyini ve türünü belirtin. Desteklenen türler: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**. Desteklenen düzeyler: **Optimal** ve **en hızlı**. Daha fazla bilgi için bkz: [Azure Data Factory dosya ve sıkıştırma biçimlerde](data-factory-supported-file-and-compression-formats.md#compression-support). |Hayır | |


> [!NOTE]
> **bucketName + tuşu** burada demet S3 nesneleri için kök kapsayıcı ve anahtarıdır S3 nesnenin tam yolunun S3 nesnenin konumunu belirtir.

### <a name="sample-dataset-with-prefix"></a>Örnek veri kümesi önekiyle

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
### <a name="sample-dataset-with-version"></a>Örnek veri kümesi (sürümüyle)

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

### <a name="dynamic-paths-for-s3"></a>S3 için dinamik yollar
Önceki örnekte sabit değerleri kullanan **anahtar** ve **bucketName** Amazon S3 dataset özelliklerinde.

```json
"key": "testFolder/test.orc",
"bucketName": "testbucket",
```

Veri Fabrikası SliceStart gibi sistem değişkenleri kullanarak çalışma zamanında dinamik olarak bu özellikleri hesaplama olabilir.

```json
"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"
```

Aynı yapabileceğiniz **önek** bir Amazon S3 dataset özelliğinin. Desteklenen işlevleri ve değişkenler listesi için bkz: [Data Factory işlevler ve sistem değişkenleri](data-factory-functions-variables.md).

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen oluşturma](data-factory-create-pipelines.md). Ad, açıklama, giriş ve çıkış tabloları ve ilkeleri gibi özellikler etkinlikleri tüm türleri için kullanılabilir. Kullanılabilir özellikler **typeProperties** bölüm etkinliğin her etkinlik türü ile değişir. Kopya etkinliği için özellikler türlerini kaynakları ve havuzlarını bağlı olarak farklılık gösterir. Kopyalama etkinliği kaynağında türü olduğunda **FileSystemSource** (içeren Amazon S3), aşağıdaki özellikler kullanılabilir **typeProperties** bölümü:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| Özyinelemeli |Özyinelemeli S3 listesinde olup olmadığını belirtir dizini altındaki nesneleri. |true/false |Hayır |

## <a name="json-example-copy-data-from-amazon-s3-to-azure-blob-storage"></a>JSON örnek: veri kopyalama Amazon S3'ten Azure Blob depolama alanına
Bu örnek, Amazon S3'ten bir Azure Blob depolama alanına veri kopyalama gösterilmektedir. Ancak, verileri doğrudan kopyalanabilir [desteklenen havuzlarını hiçbirini](data-factory-data-movement-activities.md#supported-data-stores-and-formats) Data Factory kopyalama etkinliği kullanarak.

Örnek aşağıdaki Data Factory varlıkları için JSON tanımları sağlar. Bu tanımları verileri Amazon S3'ten kullanarak Blob depolama alanına kopyalamak için bir işlem hattı oluşturmak için kullanabileceğiniz [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), veya [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).   

* Bağlı hizmet türü [AwsAccessKey](#linked-service-properties).
* Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Bir giriş [dataset](data-factory-create-datasets.md) türü [AmazonS3](#dataset-properties).
* Bir çıkış [dataset](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* A [ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [FileSystemSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Örnek verileri Amazon S3'ten saatte bir Azure blob kopyalar. Bu örnekler kullanılan JSON özellikleri örnekleri aşağıdaki bölümlerde açıklanmıştır.

### <a name="amazon-s3-linked-service"></a>Amazon S3 bağlı hizmet

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

### <a name="amazon-s3-input-dataset"></a>Amazon S3 girdi veri kümesi

Ayarı **"dış": true** Data Factory hizmetinin veri kümesi data factory dış olduğunu bildirir. Bu özellik, bir işlem hattında etkinlik tarafından üretilen olmayan bir giriş veri kümesi üzerinde true olarak ayarlayın.

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

Veri her saat yeni bir bloba yazılır (sıklığı: saat, aralığı: 1). Blob klasör yolu dinamik işlenmekte olan dilim başlangıç zamanı temel alınarak değerlendirilir. Klasör yolu yıl, ay, gün ve saatleri bölümlerini başlangıç saatini kullanır.

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


### <a name="copy-activity-in-a-pipeline-with-an-amazon-s3-source-and-a-blob-sink"></a>Bir Amazon S3 kaynak ve blob havuz sahip işlem hattı kopyalama etkinliği

Ardışık Düzen giriş ve çıkış veri kümeleri kullanmak üzere yapılandırıldığı ve saatte çalışacak şekilde zamanlanır kopyalama etkinliği içerir. JSON tanımını düzenindeki **kaynak** türü ayarlanmış **FileSystemSource**, ve **havuz** türü ayarlanmış **BlobSink**.

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
> Bir havuz veri kümesinden sütunlara kaynak kümesinden sütunları eşlemek için bkz: [Azure Data Factory veri kümesi sütunlarında eşleme](data-factory-map-columns.md).


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

* Bu veri fabrikası ve onu en iyi duruma getirmek için çeşitli yollar veri taşıma (kopyalama etkinliği) etkisi performansını anahtar Etkenler hakkında bilgi için bkz [kopyalama etkinliği performans ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md).

* Kopyalama etkinliği ile işlem hattı oluşturmak için adım adım yönergeler için bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
