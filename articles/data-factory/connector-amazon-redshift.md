---
title: Amazon Azure Data Factory kullanarak Redshift veri kopyalama | Microsoft Docs
description: "Verileri Amazon Redshift desteklenen havuz veri depoları için Azure Data Factory kullanarak kopyalama hakkında bilgi edinin."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2017
ms.author: jingwang
ms.openlocfilehash: dc8da80a89024d687a10b1539eeb1d90d218e4fb
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="copy-data-from-amazon-redshift-using-azure-data-factory"></a>Amazon Azure Data Factory kullanarak Redshift veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-amazon-redshift-connector.md)
> * [Sürüm 2 - Önizleme](connector-amazon-redshift.md)


Bu makalede kopya etkinliği Azure Data Factory'de bir Amazon Redshift verileri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1, Amazon Redshift connnector](v1/data-factory-amazon-redshift-connector.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Verileri Amazon Redshift tüm desteklenen havuz veri deposuna kopyalayabilirsiniz. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Amazon Redshift bağlayıcı sorgu veya yerleşik Redshift UNLOAD destek kullanarak Redshift veri alma destekler.

> [!TIP]
> Büyük miktarlarda verinin Redshift kopyalarken en iyi performansı elde etmek için yerleşik Redshift UNLOAD Amazon S3 aracılığıyla kullanmayı düşünün. Bkz [Amazon Redshift verileri kopyalamak için kullanım UNLOAD](#use-unload-to-copy-data-from-amazon-redshift) ayrıntıları bölümü.

## <a name="prerequisites"></a>Ön koşullar

* Kopyalama yapıyorsanız bir şirket içi veri veri depolamak kullanarak [Self-hosted tümleştirmesi çalışma zamanı](create-self-hosted-integration-runtime.md), tümleştirme çalışma zamanı (makinenin IP adresini kullan) Amazon Redshift kümesine erişimi verin. Bkz: [küme erişim yetkisi](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) yönergeler için.
* Bir Azure veri deposuna veri kopyalama olup [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653) işlem IP adresi ve Azure veriler tarafından kullanılan SQL aralıkları için merkezleri.

## <a name="getting-started"></a>Başlarken
.NET SDK'sı, Python SDK'sı, Azure PowerShell, REST API veya Azure Resource Manager şablonu kullanarak kopyalama etkinliği ile işlem hattı oluşturabilirsiniz. Bkz: [kopyalama etkinliği öğretici](quickstart-create-data-factory-dot-net.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

Aşağıdaki bölümler, Amazon Redshift bağlayıcıya Data Factory varlıklarını belirli tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler, Amazon Redshift bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **AmazonRedshift** | Evet |
| sunucu |IP adresi veya ana bilgisayar Amazon Redshift sunucunun adıdır. |Evet |
| port |İstemci bağlantılarını dinlemek için Amazon Redshift sunucunun kullandığı TCP bağlantı noktası numarası. |Hayır, varsayılan 5439 yöneliktir |
| veritabanı |Amazon Redshift veritabanının adı. |Evet |
| kullanıcı adı |Veritabanına erişimi olan kullanıcı adı. |Evet |
| password |Kullanıcı hesabının parolası. Bu alan bir SecureString işaretleyin. |Evet |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu özel bir ağda yer alıyorsa) Azure tümleştirmesi çalışma zamanı veya Self-hosted tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. |Hayır |

**Örnek:**

```json
{
    "name": "AmazonRedshiftLinkedService",
    "properties":
    {
        "type": "AmazonRedshift",
        "typeProperties":
        {
            "server": "<server name>",
            "database": "<database name>",
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için veri kümeleri makalesine bakın. Bu bölümde, Amazon Redshift veri kümesi tarafından desteklenen özellikler listesini sağlar.

Amazon Redshift verileri kopyalamak için kümesine tür özelliği ayarlamak **RelationalTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak: **RelationalTable** | Evet |
| tableName | Amazon Redshift tablo adı. | ("Sorgu" etkinliği kaynağındaki belirtilmişse) yok |

**Örnek**

```json
{
    "name": "AmazonRedshiftDataset",
    "properties":
    {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<Amazon Redshift linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde, Amazon Redshift kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="amazon-redshift-as-source"></a>Amazon Redshift kaynağı olarak

Amazon Redshift verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **AmazonRedshiftSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **AmazonRedshiftSource** | Evet |
| sorgu |Verileri okumak için özel sorgu kullanın. |SQL sorgu dizesi. Örneğin: seçin * from MyTable. |(Veri kümesinde "tableName" belirtilmişse) yok |
| redshiftUnloadSettings | Amazon Redshift UNLOAD kullanırken özellik grubu. | Hayır |
| s3LinkedServiceName | Bir Amazon S3 to-edilecek geçici bir depolama alanı olarak kullanılan "AmazonS3" türünün bir bağlantılı hizmet adı belirterek başvuruyor. | UNLOAD kullanıyorsanız Evet |
| bucketName | Geçici verileri depolamak için S3 demetini gösterir. Sağlanmazsa, Data Factory hizmeti bunu otomatik olarak oluşturur.  | UNLOAD kullanıyorsanız Evet |

**Örnek: Amazon Redshift kaynağında UNLOAD kullanarak kopyalama etkinliği**

```json
"source": {
    "type": "AmazonRedshiftSource",
    "query": "<SQL query>",
    "redshiftUnloadSettings": {
        "s3LinkedServiceName": {
            "referenceName": "<Amazon S3 linked service>",
            "type": "LinkedServiceReference"
        },
        "bucketName": "bucketForUnload"
    }
}
```

UNLOAD Amazon Redshift sonraki bölümünden verimli bir şekilde veri kopyalamak için nasıl kullanılacağı hakkında daha fazla bilgi edinin.

## <a name="use-unload-to-copy-data-from-amazon-redshift"></a>UNLOAD Amazon Redshift verileri kopyalamak için kullanın

[UNLOAD](http://docs.aws.amazon.com/redshift/latest/dg/r_UNLOAD.html) Amazon Basit Depolama Birimi Hizmeti (Amazon S3) üzerindeki bir veya daha fazla dosyalarına bir sorgunun sonuçlarını unload Amazon Redshift tarafından sağlanan bir mekanizması. Amazon tarafından Redshift büyük bir veri kümesinin kopyalamak için önerilen yoldur.

**Örnek: veri Amazon Redshift Azure SQL veri kaldırma, hazırlanmış Kopyala ve PolyBase kullanarak ambarına kopyalama**

Bu örnek için kullanım örneği, etkinlik bellekten verilerini Amazon Redshift Amazon S3 "redshiftUnloadSettings" ve verilerini Amazon S3 Azure Blob "stagingSettings" belirtildiği gibi yapılandırıldığı gibi son PolyBase verileri SQL veri yüklemek için kullanın Ambar. Tüm iç biçimi kopyalama etkinliği tarafından düzgün bir şekilde ele alınır.

![SQL DW kopyalama iş akışı için Redshift](media\copy-data-from-amazon-redshift\redshift-to-sql-dw-copy-workflow.png)

```json
"activities":[
    {
        "name": "CopyFromAmazonRedshiftToSQLDW",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "AmazonRedshiftDataset",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "AzureSQLDWDataset",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "AmazonRedshiftSource",
                "query": "select * from MyTable",
                "redshiftUnloadSettings": {
                    "s3LinkedServiceName": {
                        "referenceName": "AmazonS3LinkedService",
                        "type": "LinkedServiceReference"
                    },
                    "bucketName": "bucketForUnload"
                }
            },
            "sink": {
                "type": "SqlDWSink",
                "allowPolyBase": true
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": "AzureStorageLinkedService",
                "path": "adfstagingcopydata"
            },
            "cloudDataMovementUnits": 32
        }
    }
]
```

## <a name="data-type-mapping-for-amazon-redshift"></a>Amazon Redshift için veri türü eşlemesi

Amazon Redshift veri kopyalama işlemi sırasında aşağıdaki eşlemelerini Amazon Redshift veri türlerinden Azure Data Factory geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) nasıl kopyalama etkinliği kaynak şema ve veri türü için havuz eşlemeleri hakkında bilgi edinmek için.

| Amazon Redshift veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| BIGINT |Int64 |
| BOOLE DEĞERİ |Dize |
| CHAR |Dize |
| TARİH |Tarih Saat |
| ONDALIK |Ondalık |
| ÇİFT DUYARLIKLI |Çift |
| TAMSAYI |Int32 |
| GERÇEK |Bekar |
| TAMSAYI |Int16 |
| METİN |Dize |
| ZAMAN DAMGASI |Tarih Saat |
| VARCHAR |Dize |

## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
