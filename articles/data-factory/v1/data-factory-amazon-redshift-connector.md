---
title: "Azure Data Factory kullanarak Amazon Redshift veri taşıma | Microsoft Docs"
description: "Azure Data Factory kopyalama etkinliği kullanarak Amazon Redshift veri taşıma konusunda bilgi edinin."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 01d15078-58dc-455c-9d9d-98fbdf4ea51e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2017
ms.author: jingwang
robots: noindex
ms.openlocfilehash: d423304c84bd03477f5e9ee2edb4763e2ae8d5b5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="move-data-from-amazon-redshift-using-azure-data-factory"></a>Veri öğesinden Amazon, Redshift Azure Data Factory kullanarak Taşı
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](data-factory-amazon-redshift-connector.md)
> * [Sürüm 2 - Önizleme](../connector-amazon-redshift.md)

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [V2 Amazon Redshift Bağlayıcısı](../connector-amazon-redshift.md).

Bu makalede kopya etkinliği Azure Data Factory'de Amazon Redshift verileri taşımak için nasıl kullanılacağı açıklanmaktadır. Makaleyi derlemeler [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) kopyalama etkinliği ile veri taşıma için genel bir bakış sunar makalesi. 

Veri Fabrikası şu anda yalnızca taşıma verileri Amazon Redshift destekleyen bir [desteklenen havuz veri deposu](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Verileri Amazon Redshift diğer veri depolarına taşıma desteklenmiyor.

> [!TIP]
> Büyük miktarlarda verinin Amazon Redshift kopyalarken en iyi performansı elde etmek için yerleşik Redshift kullanmayı **UNLOAD** Amazon Basit Depolama Birimi Hizmeti (Amazon S3) aracılığıyla komutu. Ayrıntılar için bkz [Amazon Redshift verileri kopyalamak için kullanım UNLOAD](#use-unload-to-copy-data-from-amazon-redshift).

## <a name="prerequisites"></a>Ön koşullar
* Bir şirket içi veri deposuna veri taşıyorsanız, yükleme [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) , şirket içi makinede. Şirket içi makineyi IP adresini kullanarak Amazon Redshift küme için bir ağ geçidi için erişim verin. Yönergeler için bkz: [küme erişim yetkisi](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html).
* Bir Azure veri deposuna veri taşımak için bkz: [işlem IP adresi ve Microsoft Azure veri merkezleri tarafından kullanılan SQL aralıkları](https://www.microsoft.com/download/details.aspx?id=41653).

## <a name="getting-started"></a>Başlarken
Farklı araçlar ve API'ler kullanarak bir Amazon Redshift kaynaktan veri taşımak için kopyalama etkinliği ile işlem hattı oluşturun.

Bir işlem hattı oluşturmak için en kolay yolu, Azure Data Factory Kopyalama Sihirbazı'nı kullanmaktır. Hızlı bir anlatım Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma hakkında bilgi için bkz: [öğretici: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md).

Azure portalı, Visual Studio, Azure PowerShell veya diğer araçları kullanarak, bir işlem hattı de oluşturabilirsiniz. Azure Resource Manager şablonları, .NET API veya REST API ardışık düzen oluşturmak için de kullanılabilir. Kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

Araçlar ya da API'leri kullanıp bir havuz veri deposu için bir kaynak veri deposundan verileri taşır bir ardışık düzen oluşturmak için aşağıdaki adımları gerçekleştirin: 

1. Veri depoları, veri fabrikası için çıkış ve giriş bağlantısı bağlantılı Hizmetleri oluşturun.
2. Kopyalama işlemi için girdi ve çıktı verilerini temsil edecek veri kümeleri oluşturma. 
3. Bir giriş olarak bir veri kümesi ve bir veri kümesini çıktı olarak alan kopyalama etkinliği ile işlem hattı oluşturacaksınız. 

Kopyalama Sihirbazı'nı kullandığınızda, bu Data Factory varlıkları için JSON tanımları otomatik olarak oluşturulur. Araçları veya API'ler (dışında .NET API'si) kullandığınızda, JSON biçimini kullanarak Data Factory varlıklarını tanımlayın. [JSON örnek: veri kopyalama Amazon Redshift Azure Blob depolama alanına](#json-example-copy-data-from-amazon-redshift-to-azure-blob) bir Amazon Redshift veri deposundan verileri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları gösterir.

Aşağıdaki bölümlerde, Amazon Redshift için Data Factory varlıklarını tanımlamak için kullanılan JSON özellikleri açıklanmaktadır.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki tabloda bir Amazon bağlı Redshift hizmete özgü JSON öğeleri için açıklamalar sağlanır.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| **türü** |Bu özelliği ayarlamak **AmazonRedshift**. |Evet |
| **Sunucu** |Amazon Redshift sunucusunun IP adresi veya ana bilgisayar adı. |Evet |
| **bağlantı noktası** |İstemci bağlantılarını dinlemek için Amazon Redshift sunucunun kullandığı TCP bağlantı noktası numarası. |Hayır (varsayılan olarak 5439) |
| **Veritabanı** |Amazon Redshift veritabanının adı. |Evet |
| **Kullanıcı adı** |Veritabanına erişimi olan kullanıcı adı. |Evet |
| **Parola** |Kullanıcı hesabının parolası. |Evet |

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümelerini tanımlamak için kullanılabilir özelliklerin listesi için bkz [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. **Yapısı**, **kullanılabilirlik**, ve **İlkesi** bölümleri tüm veri kümesi türleri için benzer. Azure SQL, Azure Blob Depolama ve Azure Table depolama veri kümesi türleri örneklerindendir.

**TypeProperties** bölüm veri kümesi her tür için farklıdır ve verilerin depolama konumu hakkında bilgi sağlar. **TypeProperties** bir veri kümesi için bir bölüm türü **RelationalTable**, Amazon Redshift veri kümesini içeren aşağıdaki özelliklere sahip:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| **tableName** |Amazon Redshift veritabanında bağlantılı hizmet başvurduğu tablonun adı. |Hayır (varsa **sorgu** türü kopyalama etkinliği özelliğinin **RelationalSource** belirtilir) |

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala

Bölümleri ve etkinlikleri tanımlamak için kullanılabilir özelliklerin listesi için bkz [oluşturma ardışık düzen](data-factory-create-pipelines.md) makalesi. **Adı**, **açıklama**, **girişleri** tablo **çıkarır** tablo ve **İlkesi** özellikleri Etkinlikler tüm türleri için kullanılabilir. Kullanılabilir özellikler **typeProperties** bölüm her etkinlik türü için farklılık gösterir. Kopya etkinliği için özellikleri, veri kaynaklarının ve havuzlarını türlerine bağlı olarak farklılık gösterir.

Kopyalama kaynağı türü olduğunda etkinliği için **AmazonRedshiftSource**, aşağıdaki özellikler mevcuttur **typeProperties** bölümü:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| **Sorgu** | Verileri okumak için özel sorgu kullanın. |Hayır (varsa **tableName** özellik kümesinin belirtilen) |
| **redshiftUnloadSettings** | Özellik grubu Redshift kullanırken içeren **UNLOAD** komutu. | Hayır |
| **s3LinkedServiceName** | Bir geçiş deposu olarak kullanmak için Amazon S3. Bağlantılı hizmet türü Azure Data Factory adını kullanarak belirtilen **AwsAccessKey**. | Kullanırken gerekli **redshiftUnloadSettings** özelliği |
| **bucketName** | Geçici verileri depolamak için kullanmak üzere Amazon S3 demetini gösterir. Bu özellik sağlanmazsa, kopyalama etkinliği otomatik-kova oluşturur. | Kullanırken gerekli **redshiftUnloadSettings** özelliği |

Alternatif olarak, kullanabileceğiniz **RelationalSource** Amazon Redshift aşağıdaki özelliğinde içerir türü **typeProperties** bölümü. Bu kaynak türü Redshift desteklemiyor Not **UNLOAD** komutu.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| **Sorgu** |Verileri okumak için özel sorgu kullanın. | Hayır (varsa **tableName** özellik kümesinin belirtilen) |

## <a name="use-unload-to-copy-data-from-amazon-redshift"></a>UNLOAD Amazon Redshift verileri kopyalamak için kullanın

Amazon Redshift [ **UNLOAD** ](http://docs.aws.amazon.com/redshift/latest/dg/r_UNLOAD.html) komutu Amazon S3 üzerindeki bir veya daha fazla dosyalarına bir sorgunun sonuçlarını kaldırır. Bu komut, Amazon tarafından Redshift büyük veri kümelerini kopyalamak için önerilir.

**Örnek: Verilerini Amazon Redshift Azure SQL veri ambarı**

Bu örnek verileri Amazon Redshift Azure SQL veri ambarı'na kopyalar. Örnek Redshift kullanır **UNLOAD** komutu, veri hazırlanmış Kopyala ve Microsoft PolyBase.

Bu örnek kullanım durumu için kopyalama etkinliği ilk Amazon Redshift verileri Amazon S3 için yapılandırılan bellekten **redshiftUnloadSettings** seçeneği. Ardından, verileri Amazon S3'ten belirtildiği gibi Azure Blob Depolama kopyalanır **stagingSettings** seçeneği. Son olarak, PolyBase verileri SQL Data Warehouse'a veri yükler. Tüm geçiş biçimlerinin kopyalama etkinliği tarafından işlenir.

![SQL veri ambarı Amazon Redshift alanından kopyalama iş akışı](media\data-factory-amazon-redshift-connector\redshift-to-sql-dw-copy-workflow.png)

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

## <a name="json-example-copy-data-from-amazon-redshift-to-azure-blob-storage"></a>JSON örnek: veri kopyalama Amazon Redshift Azure Blob depolama alanına
Bu örnek, verileri Azure Blob depolama alanına bir Amazon Redshift veritabanından kopyalamak gösterilmiştir. Verileri doğrudan herhangi kopyalanabilir [havuz desteklenen](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kullanarak kopyalama etkinliği.  

Örnek aşağıdaki data factory varlıklarını sahiptir:

* Bağlı hizmet türü [AmazonRedshift](#linked-service-properties)
* Bağlı hizmet türü [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Bir giriş [dataset](data-factory-create-datasets.md) türü [RelationalTable](#dataset-properties)
* Bir çıkış [dataset](data-factory-create-datasets.md) türü [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
* A [ardışık düzen](data-factory-create-pipelines.md) kullanan kopyalama etkinliği ile [RelationalSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md##copy-activity-properties) özellikleri

Örnek verileri Amazon Redshift bir sorgu sonucunda bir Azure blob saatlik kopyalar. Aşağıdaki örnekte kullanılan JSON özellikleri varlık tanımları izleyen bölümlerde açıklanmıştır.

**Amazon Redshift bağlı hizmet**

```json
{
    "name": "AmazonRedshiftLinkedService",
    "properties":
    {
        "type": "AmazonRedshift",
        "typeProperties":
        {
            "server": "< The IP address or host name of the Amazon Redshift server >",
            "port": <The number of the TCP port that the Amazon Redshift server uses to listen for client connections.>,
            "database": "<The database name of the Amazon Redshift database>",
            "username": "<username>",
            "password": "<password>"
        }
    }
}
```

**Azure Blob storage bağlı hizmeti**

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
**Amazon Redshift girdi veri kümesi**

**Dış** özelliği ayarlanmış Data Factory hizmetinin veri kümesi data factory dış olduğunu bildirmek için "true". Bu özellik ayarı veri kümesi, veri fabrikası'nda bir etkinlik tarafından oluşturulmuyor gösterir. Özelliği, bir işlem hattında etkinlik tarafından üretilen olmayan bir giriş veri kümesi üzerinde true olarak ayarlayın.

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

**Azure Blob dataset çıktı**

Veri yazıldığı için yeni bir blob saatte ayarlayarak **sıklığı** "Saat" özelliğine ve **aralığı** özelliği 1. **FolderPath** özelliği blob için dinamik olarak değerlendirilir. Özellik değeri işleniyor dilim başlangıç saatine bağlıdır. Klasör yolu yıl, ay, gün ve saatleri bölümlerini başlangıç saatini kullanır.

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

**Kopya etkinliği Azure Redshift kaynak (RelationalSource türünde) ve bir Azure Blob havuz ile ardışık düzeninde**

Ardışık Düzen giriş ve çıkış veri kümeleri kullanmak üzere yapılandırılmış bir kopyalama etkinliği içerir. Ardışık Düzen saatte çalışacak şekilde zamanlanır. Ardışık düzeni için JSON tanımında **kaynak** türü ayarlanmış **RelationalSource** ve **havuz** türü ayarlanmış **BlobSink**. SQL sorgusu için belirtilen **sorgu** özelliği son bir saat kopyalamak için verileri seçer.

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
### <a name="type-mapping-for-amazon-redshift"></a>Tür eşlemesi için Amazon Redshift
Bölümünde belirtildiği gibi [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makale, kopyalama etkinliği türü havuz için kaynak türünden otomatik tür dönüşümleri gerçekleştirir. İki aşamalı bir yaklaşım kullanarak türlerine dönüştürülür:

1. Yerel kaynak türünden bir .NET türüne dönüştürün
2. Bir .NET türünden bir yerel havuz türüne dönüştürün

Kopya etkinliği bir Amazon Redshift türünden bir .NET türü veri dönüştürdüğünde aşağıdaki eşlemelerini kullanılır:

| Amazon Redshift türü | .NET türü |
| --- | --- |
| TAMSAYI |Int16 |
| TAMSAYI |Int32 |
| BIGINT |Int64 |
| ONDALIK |Ondalık |
| GERÇEK |Tek |
| ÇİFT DUYARLIKLI |Çift |
| BOOLE DEĞERİ |Dize |
| CHAR |Dize |
| VARCHAR |Dize |
| TARİH |Tarih saat |
| ZAMAN DAMGASI |Tarih saat |
| METİN |Dize |

## <a name="map-source-to-sink-columns"></a>Kaynak havuzu sütunları eşleme
Havuz dataset sütunlara kaynak veri kümesinde sütun eşleme hakkında bilgi edinmek için [Azure Data Factory veri kümesi sütunlarında eşleme](data-factory-map-columns.md).

## <a name="repeatable-reads-from-relational-sources"></a>İlişkisel kaynaklardan yinelenebilir okuma
İlişkisel veri deposundan verileri kopyaladığınızda, Yinelenebilirlik istenmeyen sonuçları önlemek için göz önünde bulundurun. Azure Data Factory'de bir dilim el ile çalıştırabilirsiniz. Yeniden deneme de yapılandırabilirsiniz **İlkesi** bir hata oluştuğunda bir dilimi yeniden çalıştırmak bir veri kümesi için. Aynı veri okuma, kaç kez olsun dilimi yeniden çalıştırmak emin olun. Ayrıca aynı veri dilimi nasıl yeniden bağımsız olarak okuduğunuzdan emin olun. Daha fazla bilgi için bkz: [Repeatable okur ilişkisel kaynaklardan](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Performans ve ayar
Kopyalama etkinliği ve performansı iyileştirmek için yollar performansını etkileyen anahtar Etkenler hakkında bilgi edinin [kopya etkinliği performansının ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md). 

## <a name="next-steps"></a>Sonraki adımlar
Kopyalama etkinliği ile işlem hattı oluşturmak için adım adım yönergeler için bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
