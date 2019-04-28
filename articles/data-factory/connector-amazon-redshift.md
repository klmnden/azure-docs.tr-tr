---
title: Azure Data Factory kullanarak Amazon Redshift'ten verileri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına Amazon Redshift'ten Azure Data Factory kullanarak veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/07/2018
ms.author: jingwang
ms.openlocfilehash: 9e1dde57dc1903e87704bd55fb0b942b7cc349e5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61262317"
---
# <a name="copy-data-from-amazon-redshift-using-azure-data-factory"></a>Azure Data Factory kullanarak Amazon Redshift'ten verileri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-amazon-redshift-connector.md)
> * [Geçerli sürüm](connector-amazon-redshift.md)


Bu makalede, kopyalama etkinliği Azure Data Factory'de veri bir Amazon Redshift'ten kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Amazon Redshift'ten verileri desteklenen havuz veri deposuna kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Amazon Redshift Bağlayıcısı sorgu veya yerleşik Redshift'i Kaldır Destek kullanarak Redshift'ten verileri alınırken destekler.

> [!TIP]
> Büyük miktarlarda verinin Redshift'ten kopyalama işlemi sırasında en iyi performansı elde etmek için yerleşik Redshift'i Kaldır aracılığıyla Amazon S3 kullanmayı düşünün. Bkz: [kullanım verileri Amazon Redshift'ten kopyalamak için kaldırma](#use-unload-to-copy-data-from-amazon-redshift) ayrıntıları bölümü.

## <a name="prerequisites"></a>Önkoşullar

* Kopyalamakta olduğunuz, şirket içi veriler için veri depolamak kullanarak [şirket içinde barındırılan tümleştirme çalışma zamanı](create-self-hosted-integration-runtime.md), Integration Runtime (makinenin IP adresini kullan) Amazon Redshift kümesine erişim. Bkz: [kümeye erişim yetkisi](https://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) yönergeler için.
* Bir Azure veri deposuna veri kopyalama olup [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653) işlem IP adresi ve Azure veri tarafından kullanılan SQL aralıkları ortalar.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Amazon Redshift Bağlayıcısı özel Data Factory varlıkları tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Amazon Redshift bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **AmazonRedshift** | Evet |
| sunucu |IP adresi veya ana bilgisayar adı Amazon Redshift sunucusunun. |Evet |
| port |Amazon Redshift sunucusunun istemci bağlantıları için dinlemek üzere kullandığı TCP bağlantı noktası sayısı. |Hayır, 5439 varsayılandır |
| veritabanı |Amazon Redshift veritabanının adı. |Evet |
| kullanıcı adı |Veritabanına erişimi olan kullanıcı adı. |Evet |
| password |Kullanıcı hesabı için parola. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel ağında bulunuyorsa), Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için veri kümeleri makalesine bakın. Bu bölümde, Amazon Redshift veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Verileri Amazon Redshift'ten kopyalamak için dataset öğesinin type özelliği ayarlamak **RelationalTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **RelationalTable** | Evet |
| tableName | Amazon Redshift veritabanındaki tablo adı. | Hayır (etkinlik kaynağı "sorgu" belirtilmişse) |

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

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Amazon Redshift kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="amazon-redshift-as-source"></a>Amazon Redshift kaynağı olarak

Verileri Amazon Redshift'ten kopyalamak için kopyalama etkinliği kaynak türünü ayarlayın. **AmazonRedshiftSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **AmazonRedshiftSource** | Evet |
| sorgu |Verileri okumak için özel sorgu kullanın. Örneğin: seçin * MyTable öğesinden. |Yok (veri kümesinde "TableName" değeri belirtilmişse) |
| redshiftUnloadSettings | Amazon Redshift kaldırma kullanırken özellik grubu. | Hayır |
| s3LinkedServiceName | Bir Amazon S3 How-to-edilecek bir geçiş deposu olarak kullanılan "AmazonS3" türündeki bağlı hizmetin adı belirterek başvuruyor. | UNLOAD kullanıyorsanız Evet |
| bucketName | Geçici verileri depolamak için S3 demetini gösterir. Sağlanmazsa, Data Factory hizmeti, otomatik olarak oluşturur.  | UNLOAD kullanıyorsanız Evet |

**Örnek: Amazon Redshift kaynakta Kaldır'ı kullanarak kopyalama etkinliği**

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

Verileri verimli bir şekilde bir sonraki bölümden Amazon redshift'ten kopyalamak için Kaldır'ı kullanma hakkında daha fazla bilgi edinin.

## <a name="use-unload-to-copy-data-from-amazon-redshift"></a>Verileri Amazon Redshift'ten kopyalamak için Kaldır'ı kullanın

[UNLOAD](https://docs.aws.amazon.com/redshift/latest/dg/r_UNLOAD.html) Amazon Redshift, Amazon basit depolama hizmeti (Amazon S3 için) bir veya daha fazla dosyaları için bir sorgunun sonuçlarını kaldırmak tarafından sağlanan bir mekanizma. Tarafından Amazon Redshift'ten büyük veri kümesi kopyalamak için önerilen yoldur.

**Örnek: veri Amazon Redshift'ten Azure SQL kaldırma, hazırlanmış kopya ve PolyBase kullanarak veri ambarı için kopyalama**

Bu örnek için kullanım örneği, Amazon S3 için Amazon redshift'ten kopyalama etkinliği bellekten veri "redshiftUnloadSettings" ve veri kopyalama Amazon S3'ndan Azure Blob için "stagingSettings" belirtildiği gibi yapılandırılmış olarak son verileri SQL veri yüklemek için PolyBase kullanın Ambarı. Tüm iç biçimi, kopyalama etkinliği tarafından düzgün bir şekilde ele alınır.

![SQL DW kopyalama iş akışı için Redshift](media/copy-data-from-amazon-redshift/redshift-to-sql-dw-copy-workflow.png)

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
            "dataIntegrationUnits": 32
        }
    }
]
```

## <a name="data-type-mapping-for-amazon-redshift"></a>Amazon Redshift için veri türü eşlemesi

Verileri Amazon Redshift'ten kopyalama yapılırken, aşağıdaki eşlemeler Amazon Redshift veri türlerinden Azure veri fabrikası geçici veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) eşlemelerini nasıl yapar? kopyalama etkinliği kaynak şema ve veri türü için havuz hakkında bilgi edinmek için.

| Amazon Redshift veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| BIGINT |Int64 |
| BOOLE DEĞERİ |String |
| CHAR |String |
| DATE |DateTime |
| DECIMAL |Decimal |
| ÇİFT DUYARLIK |Double |
| INTEGER |Int32 |
| GERÇEK |Single |
| TAMSAYI |Int16 |
| METİN |String |
| ZAMAN DAMGASI |DateTime |
| VARCHAR |String |

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
