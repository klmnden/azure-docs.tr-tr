---
title: Amazon basit depolama Azure Data Factory kullanarak hizmetinden veri kopyalama | Microsoft Docs
description: Verileri Amazon Basit Depolama hizmetinden (S3) desteklenen havuz veri depoları için Azure Data Factory kullanarak kopyalama hakkında bilgi edinin.
services: data-factory
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 05/25/2018
ms.author: jingwang
ms.openlocfilehash: 3635e8bf1d9ba4061da5b8f416a3b755f7064000
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37045645"
---
# <a name="copy-data-from-amazon-simple-storage-service-using-azure-data-factory"></a>Amazon basit depolama Azure Data Factory kullanarak hizmetinden veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-amazon-simple-storage-service-connector.md)
> * [Geçerli sürüm](connector-amazon-simple-storage-service.md)

Bu makalede kopya etkinliği Azure Data Factory'de Amazon S3'ten verileri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Verileri Amazon S3 tüm desteklenen havuz veri deposuna kopyalayabilirsiniz. Kaynakları veya havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Amazon S3 bağlayıcı kopyalama dosyaları olarak destekler- ya da dosyalarıyla ayrıştırma [desteklenen dosya biçimleri ve sıkıştırma codec](supported-file-formats-and-compression-codecs.md).

## <a name="required-permissions"></a>Gerekli izinler

Amazon S3'ten verileri kopyalamak için aşağıdaki izinleri verilmiş olan emin olun:

- `s3:GetObject` ve `s3:GetObjectVersion` Amazon S3 nesne işlemleri için.
- `s3:ListBucket` veya `s3:GetBucketLocation` Amazon S3 Demetini işlemleri için. Data Factory Kopyalama Sihirbazı'nı kullanıyorsanız `s3:ListAllMyBuckets` de gereklidir.

Amazon S3 izinlerin tam listesi hakkında daha fazla ayrıntı için bkz: [belirleyen izinleri bir ilke](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)] 

Aşağıdaki bölümler, Amazon S3 Data Factory varlıklarını belirli tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler, Amazon S3 bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlamak **AmazonS3**. | Evet |
| accessKeyId | Gizli erişim anahtarı kimliği. |Evet |
| secretAccessKey | Gizli erişim anahtar kendisi. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). |Evet |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu özel bir ağda yer alıyorsa) Azure tümleştirmesi çalışma zamanı veya Self-hosted tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. |Hayır |

>[!NOTE]
>Bu bağlayıcı, Amazon S3'ten verileri kopyalamak IAM hesabı için erişim anahtarlarını gerektirir. [Geçici güvenlik kimlik bilgisi](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) desteklenmiyor.
>

Örnek aşağıda verilmiştir:

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AmazonS3",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": {
                    "type": "SecureString",
                    "value": "<secret access key>"
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

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için veri kümeleri makalesine bakın. Bu bölümde, Amazon S3 dataset tarafından desteklenen özellikler listesini sağlar.

Amazon S3'ten verileri kopyalamak için kümesine tür özelliği ayarlamak **AmazonS3Object**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak: **AmazonS3Object** |Evet |
| bucketName | S3 demetini adı. Joker karakter filtresi desteklenmiyor. |Evet |
| anahtar | **Adı veya joker karakter filtresini** belirtilen demetini altındaki S3 nesne anahtarının. Yalnızca "öneki" özelliği olmayan belirtildiğinde geçerlidir. <br/><br/>Joker karakter filtresi, yalnızca dosya adı bölümü ancak klasör bölümü için desteklenir. Joker karakterler izin verilir: `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (eşleşen sıfır veya tek bir karakter).<br/>-Örnek 1: `"key": "rootfolder/subfolder/*.csv"`<br/>-Örnek 2: `"key": "rootfolder/subfolder/???20180427.txt"`<br/>Kullanım `^` , gerçek dosya adı joker karakter ya da bu kaçış karakteri içinde varsa kaçınmak için. |Hayır |
| önek | S3 nesne anahtarı için önek. Seçilen nesneler, anahtarları Bu önek ile başlatın. Yalnızca "anahtar" özelliği belirtilmediğinde geçerlidir. |Hayır |
| sürüm | S3 sürüm etkinleştirilirse S3 nesne sürümü. |Hayır |
| Biçimi | İsterseniz **olarak dosyaları kopyalama-olduğu** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımlarında Biçim bölümü atlayın.<br/><br/>Ayrıştırma veya belirli bir biçime sahip dosyaları oluşturmak istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** şu değerlerden biri biçimine altında özellik. Daha fazla bilgi için bkz: [metin biçimi](supported-file-formats-and-compression-codecs.md#text-format), [Json biçimine](supported-file-formats-and-compression-codecs.md#json-format), [Avro biçimi](supported-file-formats-and-compression-codecs.md#avro-format), [Orc biçimi](supported-file-formats-and-compression-codecs.md#orc-format), ve [Parquet biçimi](supported-file-formats-and-compression-codecs.md#parquet-format) bölümler. |Hayır (yalnızca ikili kopyalama senaryosu) |
| Sıkıştırma | Veri sıkıştırma düzeyini ve türünü belirtin. Daha fazla bilgi için bkz: [desteklenen dosya biçimleri ve sıkıştırma codec](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Desteklenen türler: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**.<br/>Desteklenen düzeyler: **Optimal** ve **en hızlı**. |Hayır |

>[!TIP]
>Bir klasörü altındaki tüm dosyaları kopyalamak için belirtin **bucketName** sepet ve **önek** klasör bölümü için.<br>Belirli bir ada sahip tek bir dosya kopyalamak için belirtin **bucketName** sepet ve **anahtar** klasör bölümü artı dosya adı.<br>Bir klasör altındaki dosyalar kümesini kopyalamak için belirtin **bucketName** sepet ve **anahtar** klasör bölümü artı joker karakter filtresi için.

**Örnek: önek kullanma**

```json
{
    "name": "AmazonS3Dataset",
    "properties": {
        "type": "AmazonS3Object",
        "linkedServiceName": {
            "referenceName": "<Amazon S3 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "bucketName": "testbucket",
            "prefix": "testFolder/test",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

**Örnek: anahtar ve sürüm (isteğe bağlı) kullanma**

```json
{
    "name": "AmazonS3Dataset",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": {
            "referenceName": "<Amazon S3 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "bucketName": "testbucket",
            "key": "testFolder/testfile.csv.gz",
            "version": "XXXXXXXXXczm0CJajYkHf0_k6LhBmkcL",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde, Amazon S3 kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="amazon-s3-as-source"></a>Kaynak olarak Amazon S3

Amazon S3'ten verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **FileSystemSource** (Amazon S3 içerir). Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **FileSystemSource** |Evet |
| özyinelemeli | Belirtilen klasörün alt klasörleri ya da yalnızca verileri özyinelemeli olarak okunur olup olmadığını gösterir. Özyinelemeli true ve havuz için ayarlandığında Not dosya tabanlı depolama, boş klasör/alt-folder havuz kopyalanır ve oluşturulan olmaz.<br/>İzin verilen değerler: **true** (varsayılan), **false** | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromAmazonS3",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Amazon S3 input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "FileSystemSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```
## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
