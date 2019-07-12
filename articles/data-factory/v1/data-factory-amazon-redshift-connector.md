---
title: Azure Data Factory kullanarak verileri Amazon Redshift'ten taşıma | Microsoft Docs
description: Azure Data Factory kopyalama etkinliği'ni kullanarak, Amazon Redshift'ten veri taşımayı öğreneceksiniz.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 01d15078-58dc-455c-9d9d-98fbdf4ea51e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 3a1497211cc42c702537cbbdfea32ff71a400c7c
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67836679"
---
# <a name="move-data-from-amazon-redshift-using-azure-data-factory"></a>Gelen Amazon, Redshift Azure Data Factory ile veri taşıma
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](data-factory-amazon-redshift-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-amazon-redshift.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [Amazon Redshift Bağlayıcısı V2'de](../connector-amazon-redshift.md).

Bu makalede, Amazon Redshift'ten verileri taşımak için Azure Data Factory kopyalama etkinliği kullanmayı açıklar. Makaleyi yapılar [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği ile verileri taşıma genel bir bakış sunar.

Data Factory şu anda yalnızca Amazon Redshift verileri destekler bir [desteklenen havuz veri deposu](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Amazon Redshift diğer veri depolarından veri taşıma desteklenmiyor.

> [!TIP]
> Büyük miktarlarda verinin Amazon Redshift'ten kopyalama işlemi sırasında en iyi performansı elde etmek için yerleşik Redshift kullanmayı düşünün **kaldırma** Amazon basit depolama hizmeti (Amazon S3 için) üzerinden komutu. Ayrıntılar için bkz [kullanım verileri Amazon Redshift'ten kopyalamak için kaldırma](#use-unload-to-copy-data-from-amazon-redshift).

## <a name="prerequisites"></a>Önkoşullar
* Bir şirket içi veri deposuna veri taşıyorsanız, yükleme [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) bir şirket içi makine üzerinde. Şirket içi Makine IP adresi kullanarak, Amazon Redshift küme için bir ağ geçidi için erişim verin. Yönergeler için [kümeye erişim yetkisi](https://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html).
* Bir Azure veri deposuna veri taşımak için bkz [işlem IP adresi ve Microsoft Azure veri merkezleri tarafından kullanılan SQL aralıkları](https://www.microsoft.com/download/details.aspx?id=41653).

## <a name="getting-started"></a>Başlarken
Farklı araç ve API'leri kullanarak bir Amazon Redshift kaynaktan verileri taşımak için bir kopyalama etkinlikli bir işlem hattı oluşturabilirsiniz.

Bir işlem hattı oluşturmanın en kolay yolu, Azure Data Factory Kopyalama Sihirbazı'nı kullanmaktır. Hızlı bir kılavuz Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma hakkında bilgi için bkz: [Öğreticisi: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md).

Ayrıca, Visual Studio, Azure PowerShell veya diğer araçları kullanarak bir işlem hattı oluşturabilirsiniz. Azure Resource Manager şablonları, .NET API veya REST API işlem hattını oluşturmak için de kullanılabilir. Kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

API'ler ve Araçlar kullanmanıza bakılmaksızın, bir havuz veri deposu için bir kaynak veri deposundan veri taşıyan bir işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Giriş bağlantı ve veri depolarında veri fabrikanıza çıktı bağlı hizmetleri oluşturun.
2. Kopyalama işleminin girdi ve çıktı verilerini temsil eden veri kümeleri oluşturun.
3. Bir veri kümesini girdi ve çıktı olarak bir veri kümesini alan kopyalama etkinliği ile işlem hattı oluşturma.

Kopyalama Sihirbazı'nı kullandığınızda, bu Data Factory varlıkları için JSON tanımları otomatik olarak oluşturulur. Araç veya API'lerden (dışında .NET API'si) kullandığınızda, Data Factory varlıkları JSON biçimini kullanarak tanımlayın. JSON örneği: Azure Blob depolama alanına veri Amazon redshift'ten kopyalama, bir Amazon Redshift veri deposundan veri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları gösterir.

Aşağıdaki bölümlerde, Amazon Redshift için Data Factory varlıkları tanımlamak için kullanılan JSON özellikleri açıklanmaktadır.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Aşağıdaki tabloda, bir Amazon Redshift bağlı hizmeti için özel JSON öğelerinin açıklamaları verilmiştir.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| **type** |Bu özellik ayarlanmalıdır **AmazonRedshift**. |Evet |
| **Sunucu** |Amazon Redshift sunucusunun IP adresi veya ana bilgisayar adı. |Evet |
| **Bağlantı noktası** |Amazon Redshift sunucusunun istemci bağlantıları için dinlemek üzere kullandığı TCP bağlantı noktası sayısı. |Hayır (varsayılan değer 5439) |
| **Veritabanı** |Amazon Redshift veritabanına adı. |Evet |
| **Kullanıcı adı** |Veritabanına erişimi olan kullanıcı adı. |Evet |
| **Parola** |Kullanıcı hesabının parolası. |Evet |

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için kullanılabilir özellikler listesi için bkz. [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. **Yapısı**, **kullanılabilirlik**, ve **ilke** bölümlerde tüm veri kümesi türleri için benzerdir. Azure SQL, Azure Blob Depolama ve Azure tablo depolama, veri kümesi türleri içerir.

**TypeProperties** bölümünde her veri kümesi türü için farklıdır ve veri depolama konumu hakkında bilgi sağlar. **TypeProperties** türü için bir veri kümesi bölümünü **RelationalTable**, Amazon Redshift veri kümesi içeren, aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| **TableName** |Tabloda Amazon Redshift veritabanına başvuran bağlı hizmetin adı. |Hayır (varsa **sorgu** türünde bir kopyalama etkinliği özelliği **RelationalSource** belirtilir) |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için kullanılabilir özellikler listesi için bkz: [işlem hatları oluşturma](data-factory-create-pipelines.md) makalesi. **Adı**, **açıklama**, **girişleri** tablo **çıkarır** tablosunu ve **ilke** özellikleri Tüm etkinlik türleri için kullanılabilir. Kullanılabilir özellikler **typeProperties** bölümünde her bir etkinlik türü için farklılık gösterir. Kopyalama etkinliği için özellikleri veri kaynağı ve havuz türlerine bağlı olarak farklılık gösterir.

Kopyalama etkinliği kaynak türü olduğunda için **AmazonRedshiftSource**, aşağıdaki özellikler kullanılabilir **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| **query** | Verileri okumak için özel sorgu kullanın. |Hayır (varsa **tableName** özellik kümesinin belirtilen) |
| **redshiftUnloadSettings** | Redshift kullanırken özellik grubunu içeren **kaldırma** komutu. | Hayır |
| **s3LinkedServiceName** | Bir geçiş deposu olarak kullanılacak Amazon S3. Bağlı hizmet türü bir Azure Data Factory adını kullanarak belirtilen **AwsAccessKey**. | Kullanırken gereklidir **redshiftUnloadSettings** özelliği |
| **bucketName** | Geçici verileri depolamak için kullanılacak Amazon S3 demetini gösterir. Bu özellik sağlanmazsa, kopyalama etkinliği otomatik-bir demet oluşturur. | Kullanırken gereklidir **redshiftUnloadSettings** özelliği |

Alternatif olarak, **RelationalSource** Amazon Redshift, aşağıdaki özellik içeren bir tür **typeProperties** bölümü. Bu kaynak türü, Redshift desteklemiyor Not **kaldırma** komutu.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| **query** |Verileri okumak için özel sorgu kullanın. | Hayır (varsa **tableName** özellik kümesinin belirtilen) |

## <a name="use-unload-to-copy-data-from-amazon-redshift"></a>Verileri Amazon Redshift'ten kopyalamak için Kaldır'ı kullanın

Amazon Redshift [ **kaldırma** ](https://docs.aws.amazon.com/redshift/latest/dg/r_UNLOAD.html) Amazon S3 bir veya daha fazla dosyaları için bir sorgunun sonuçlarını komutu kaldırır. Bu komut tarafından Amazon Redshift'ten büyük veri kümelerini kopyalamak için önerilir.

**Örnek: Azure SQL veri ambarı veri Amazon redshift'ten kopyalama**

Bu örnek verileri Amazon Redshift'ten Azure SQL veri ambarı'na kopyalar. Redshift örnekte **kaldırma** komutu, verileri hazırlanmış kopya ve Microsoft PolyBase.

Bu örnek kullanım örneği için kopyalama etkinliği ilk Amazon redshift'ten verileri Amazon S3 için yapılandırılan bellekten **redshiftUnloadSettings** seçeneği. Ardından, verileri Amazon S3'ten belirtildiği gibi Azure Blob Depolama kopyalanır **stagingSettings** seçeneği. Son olarak, PolyBase, verileri SQL veri ambarı'na yükler. Tüm ara biçimlerin kopyalama etkinliği tarafından işlenir.

![SQL veri ambarı Amazon redshift'ten kopyalama iş akışı](media/data-factory-amazon-redshift-connector/redshift-to-sql-dw-copy-workflow.png)

```json
{
    "name": "CopyFromRedshiftToSQLDW",
    "type": "Copy",
    "typeProperties": {
        "source": {
            "type": "AmazonRedshiftSource",
            "query": "select * from MyTable",
            "redshiftUnloadSettings": {
                "s3LinkedServiceName":"MyAmazonS3StorageLinkedService",
                "bucketName": "bucketForUnload"
            }
        },
        "sink": {
            "type": "SqlDWSink",
            "allowPolyBase": true
        },
        "enableStaging": true,
        "stagingSettings": {
            "linkedServiceName": "MyAzureStorageLinkedService",
            "path": "adfstagingcopydata"
        },
        "cloudDataMovementUnits": 32
        .....
    }
}
```

## <a name="json-example-copy-data-from-amazon-redshift-to-azure-blob-storage"></a>JSON örneği: Azure Blob depolama alanına veri Amazon redshift'ten kopyalama
Bu örnek, bir Amazon Redshift veritabanındaki verileri Azure Blob depolama alanına veri kopyalamak gösterilmektedir. Verileri doğrudan herhangi kopyalanabilir [havuz desteklenen](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kullanarak kopyalama etkinliği.

Örnek, aşağıdaki data factory varlıklarını sahiptir:

* Bağlı hizmet türü [AmazonRedshift](#linked-service-properties)
* Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Girdi [veri kümesi](data-factory-create-datasets.md) türü [RelationalTable](#dataset-properties)
* Bir çıkış [veri kümesi](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
* A [işlem hattı](data-factory-create-pipelines.md) kullanan bir kopyalama etkinlikli [RelationalSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md##copy-activity-properties) özellikleri

Örnek verileri Amazon Redshift bir sorgu sonucunda Azure blobuna saatlik kopyalar. Örnekte kullanılan JSON özellikleri varlık tanımlarını bölümlerde açıklanmıştır.

**Amazon Redshift bağlı hizmeti**

```json
{
    "name": "AmazonRedshiftLinkedService",
    "properties":
    {
        "type": "AmazonRedshift",
        "typeProperties":
        {
            "server": "< The IP address or host name of the Amazon Redshift server >",
            "port": "<The number of the TCP port that the Amazon Redshift server uses to listen for client connections.>",
            "database": "<The database name of the Amazon Redshift database>",
            "username": "<username>",
            "password": "<password>"
        }
    }
}
```

**Azure Blob Depolama bağlı hizmeti**

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
**Amazon Redshift giriş veri kümesi**

**Dış** "Data Factory hizmetinin veri kümesini veri fabrikasına dış olduğunu bildirmek için true olarak" özelliğini ayarlayın. Bu özellik ayarı, veri kümesi veri fabrikasında bir etkinlik tarafından üretilen değil gösterir. Özelliği, işlem hattındaki bir etkinliğin oluşturulmuyor bir giriş veri kümesi üzerinde true olarak ayarlayın.

```json
{
    "name": "AmazonRedshiftInputDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "AmazonRedshiftLinkedService",
        "typeProperties": {
            "tableName": "<Table name>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

**Azure Blob çıktı veri kümesi**

Veriler yazılır yeni bir blob için saatte ayarlayarak **sıklığı** "Hour" özelliğini ve **aralığı** özelliği 1. **FolderPath** blob özelliği dinamik olarak değerlendirilir. Dilimin işlenmekte olan başlangıç zamanında özellik değeri temel alır. Yıl, ay, gün ve saat bölümlerini başlangıç zamanı klasör yolu kullanır.

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/fromamazonredshift/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Bir Azure Redshift kaynak (RelationalSource türünde) ve bir Azure Blob havuz ile bir işlem hattındaki kopyalama etkinliği**

İşlem hattının kopyalama etkinliği girdi ve çıktı veri kümelerini kullanmak üzere yapılandırılmış içerir. İşlem hattını saatte bir çalışacak şekilde zamanlanmış. İşlem hattının JSON tanımında **kaynak** türü ayarlandığında **RelationalSource** ve **havuz** türü ayarlandığında **BlobSink**. SQL sorgusu için belirtilen **sorgu** özellik, son bir saat kopyalanacak verileri seçer.

```json
{
    "name": "CopyAmazonRedshiftToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "AmazonRedshiftSource",
                        "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)",
                        "redshiftUnloadSettings": {
                            "s3LinkedServiceName":"myS3Storage",
                            "bucketName": "bucketForUnload"
                        }
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    },
                    "cloudDataMovementUnits": 32
                },
                "inputs": [
                    {
                        "name": "AmazonRedshiftInputDataset"
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
                "name": "AmazonRedshiftToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```
### <a name="type-mapping-for-amazon-redshift"></a>Amazon Redshift için tür eşlemesi
Belirtildiği gibi [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesi, kopyalama etkinliği Havuz türü için kaynak türünden otomatik tür dönüştürmeleri gerçekleştirir. İki aşamalı bir yaklaşım kullanarak türlerine dönüştürülür:

1. Yerel kaynak türünden bir .NET türüne dönüştürün
2. Bir .NET türünden bir yerel havuz türüne dönüştürün

Kopyalama etkinliği verileri bir Amazon Redshift türünden bir .NET türe dönüştürdüğünü şu eşlemeler kullanılır:

| Amazon Redshift türü | .NET türü |
| --- | --- |
| SMALLINT |Int16 |
| INTEGER |Int32 |
| BIGINT |Int64 |
| DECIMAL |Decimal |
| REAL |Single |
| DOUBLE PRECISION |Double |
| BOOLEAN |Dize |
| CHAR |Dize |
| VARCHAR |Dize |
| DATE |Datetime |
| TIMESTAMP |Datetime |
| TEXT |Dize |

## <a name="map-source-to-sink-columns"></a>Sütunları havuz için kaynak eşlemesi
Kaynak veri kümesindeki sütunları havuz veri kümesi sütunlara eşlemeyle ilgili bilgi edinmek için bkz: [Azure Data factory'de veri kümesi sütunlarını eşleme](data-factory-map-columns.md).

## <a name="repeatable-reads-from-relational-sources"></a>İlişkisel kaynaklar tekrarlanabilir okuma
İlişkisel veri deposundan veri kopyaladığınızda, Yinelenebilirlik istenmeyen sonuçlar önlemek için göz önünde bulundurun. Azure Data Factory'de bir dilim el ile çalıştırabilirsiniz. Yeniden deneme de yapılandırabilirsiniz **ilke** bir hata oluştuğunda bir dilimi yeniden çalıştırmak için bir veri kümesi için. Aynı veriler okunur, kaç kez ne olursa olsun dilimi yeniden çalıştırmak emin olun. Ayrıca dilim nasıl yeniden bağımsız olarak aynı verileri okuduğunuzdan emin olun. Daha fazla bilgi için [Repeatable okur ilişkisel kaynaklardan](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Performans ve ayar
Kopyalama etkinliği ve performansı iyileştirmek için yollar performansını etkileyen önemli faktörlerin öğrenin [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md).

## <a name="next-steps"></a>Sonraki adımlar
Kopyalama etkinliği ile işlem hattı oluşturmak için adım adım yönergeler için bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
