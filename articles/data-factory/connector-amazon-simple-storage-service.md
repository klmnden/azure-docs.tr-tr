---
title: Amazon basit depolama hizmeti (S3 için) Azure Data Factory kullanarak verileri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına Amazon Basit Depolama hizmetinden (S3 için) Azure Data Factory kullanarak veri kopyalama hakkında bilgi edinin.
services: data-factory
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: ca764c7e78f6ffe221386d1d320582e394d0a78a
ms.sourcegitcommit: 2c09af866f6cc3b2169e84100daea0aac9fc7fd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64875869"
---
# <a name="copy-data-from-amazon-simple-storage-service-using-azure-data-factory"></a>Amazon basit depolama hizmeti Azure Data Factory kullanarak veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
>
> * [Sürüm 1](v1/data-factory-amazon-simple-storage-service-connector.md)
> * [Geçerli sürüm](connector-amazon-simple-storage-service.md)

Bu makalede, Amazon Basit Depolama hizmetinden (Amazon S3) veri kopyalamak nasıl özetlenmektedir. Azure Data Factory hakkında bilgi edinmek için [giriş makalesi](introduction.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bu Amazon S3 Bağlayıcısı için aşağıdaki etkinlikleri desteklenir:

- [Kopyalama etkinliği](copy-activity-overview.md) ile [desteklenen kaynak/havuz Matrisi](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata activity](control-flow-get-metadata-activity.md)

Özellikle, bu Amazon S3 bağlayıcı kopyalama dosyaları gibi destekler- ya da dosyaları ayrıştırma [desteklenen dosya biçimleri ve codec sıkıştırma](supported-file-formats-and-compression-codecs.md). Kullandığı [AWS imza sürüm 4](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html) S3 isteklerine kimlik doğrulaması için.

>[!TIP]
>Bu Amazon S3 bağlayıcı veri kopyalamak için kullanabileceğiniz **herhangi S3 ile uyumlu depolama sağlayıcıları** örn [Google bulut depolama](connector-google-cloud-storage.md). Bağlı hizmet yapılandırmasında karşılık gelen hizmet URL'si belirtin.

## <a name="required-permissions"></a>Gerekli izinler

Verileri Amazon S3'ten kopyalamak için aşağıdaki izinleri verilmiş olan emin olun:

- **Kopyalama etkinliği yürütme için:**: `s3:GetObject` ve `s3:GetObjectVersion` Amazon S3 nesnesinin işlemleri için.
- **Data Factory GUI yazma**: `s3:ListAllMyBuckets` ve `s3:ListBucket` / `s3:GetBucketLocation` bağlantıyı test et ve göz atma ve Git gibi işlemler dosya yolları için Amazon S3 Demetini işlemleri için izinleri ayrıca gereklidir. Bu izin vermek istemiyorsanız, bağlı hizmet oluşturma sayfasındaki test bağlantısı atlayın ve doğrudan veri kümesi ayarlarında yolunu belirtin.

Amazon S3 izinlerin tam listesi hakkında daha fazla ayrıntı için bkz: [izinleri belirten bir ilke](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)] 

Aşağıdaki bölümler, Amazon S3 belirli Data Factory varlıkları tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Amazon S3'bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır **AmazonS3**. | Evet |
| accessKeyId | Gizli erişim anahtarı kimliği. |Evet |
| secretAccessKey | Gizli erişim anahtarı kendisi. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Evet |
| ServiceUrl | Bir S3 ile uyumlu depolama sağlayıcısı resmi Amazon S3 hizmeti dışındaki veri kopyalıyorsanız özel S3 uç noktasını belirtin. Örneğin, Google bulut depolama alanından verileri kopyalamak için belirtmeniz `https://storage.googleapis.com`. | Hayır |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel ağında bulunuyorsa), Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

>[!TIP]
>Resmi Amazon S3 hizmeti dışındaki bir S3 ile uyumlu depolama alanından veri kopyalıyorsanız özel S3 hizmeti URL'si belirtin.

>[!NOTE]
>Bu bağlayıcı, Amazon S3'ten veri kopyalamak IAM hesabının erişim anahtarlarını gerektirir. [Geçici güvenlik kimlik bilgisi](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) desteklenmiyor.
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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. 

- İçin **Parquet ve sınırlandırılmış metin biçimi**, başvurmak [Parquet ve sınırlandırılmış metin biçimi veri kümesi](#parquet-and-delimited-text-format-dataset) bölümü.
- Diğer biçimlerden için **ORC/Avro/JSON/ikili biçimi**, başvurmak [diğer biçim veri kümesi](#other-format-dataset) bölümü.

### <a name="parquet-and-delimited-text-format-dataset"></a>Parquet ve sınırlandırılmış metin biçimi veri kümesi

Verileri Amazon S3 kopyalamak için **Parquet veya sınırlandırılmış metin biçimi**, başvurmak [Parquet biçimi](format-parquet.md) ve [sınırlandırılmış metin biçimi](format-delimited-text.md) makale biçimi tabanlı veri kümesinde ve desteklenir Ayarlar. Amazon S3 altında aşağıdaki özellikler desteklenir `location` biçimi tabanlı bir veri kümesi ayarlarında:

| Özellik   | Açıklama                                                  | Gerekli |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | Type özelliği altında `location` kümesinde ayarlanmalıdır **AmazonS3Location**. | Evet      |
| bucketName | S3 demetini adı.                                          | Evet      |
| folderPath | Verilen demeti altında klasör yolu. Joker karakter filtresi klasörüne kullanmak istiyorsanız, bu ayar atlayın ve etkinliği kaynak ayarları belirtin. | Hayır       |
| fileName   | Belirli bir demet + folderPath altında dosya adı. Joker karakter filtresi dosyalarını kullanmak istiyorsanız, bu ayar atlayın ve etkinliği kaynak ayarları belirtin. | Hayır       |

> [!NOTE]
> **AmazonS3Object** sonraki bölümde bahsedilen Parquet/metin biçimine sahip tür veri kümesi olarak desteklenen hala-için geriye dönük uyumluluk, ancak arama/kopyalama/GetMetadata etkinliği eşleme veri akışı ile çalışmaz. İleride bu yeni modeli kullanmak için önerilir ve bu yeni tür oluşturma için kullanıcı Arabirimi geliştirme ADF geçti.

**Örnek:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<Amazon S3 linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AmazonS3Location",
                "bucketName": "bucketname",
                "folderPath": "folder/subfolder"
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

### <a name="other-format-dataset"></a>Diğer biçim veri kümesi

Verileri Amazon S3 kopyalamak için **ORC/Avro/JSON/ikili biçimi**, aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **AmazonS3Object** |Evet |
| bucketName | S3 demetini adı. Joker karakter filtresi desteklenmez. |GetMetadata etkinliği için Hayır kopyalama/arama etkinliği için Evet |
| key | **Adı veya joker karakter filtresi** altında belirtilen demetini S3 nesnesinin anahtarı. Yalnızca ne zaman "ön eki" özelliği belirtilmedi geçerlidir. <br/><br/>Joker karakter filtresi klasör bölümünü ve dosya adı bölümü için desteklenir. Joker karakterlere izin verilir: `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter).<br/>-Örnek 1: `"key": "rootfolder/subfolder/*.csv"`<br/>-Örnek 2: `"key": "rootfolder/subfolder/???20180427.txt"`<br/>Daha fazla örneğe bakın [klasör ve dosya filtreleme örnekler](#folder-and-file-filter-examples). Kullanım `^` joker karakter veya içinde bu kaçış karakteri, gerçek klasör/dosya adı varsa, kaçış için. |Hayır |
| prefix | S3 nesnesinin anahtarı için önek. Seçili bir nesne anahtarları bu öneki ile başlayın. Yalnızca "anahtarını" özelliği belirtilmedi uygulanır. |Hayır |
| version | S3 sürümü oluşturma etkinse, S3 nesnesinin sürümü. Belirtilmezse, en son sürüme getirildi. |Hayır |
| modifiedDatetimeStart | Dosya Filtresi özniteliğine dayanarak: Son değiştirme. Kendi son değiştirilme zamanı zaman aralığı içinde olduğunda dosyaları seçilir `modifiedDatetimeStart` ve `modifiedDatetimeEnd`. Zaman biçimi UTC saat diliminde uygulanan "2018-12-01T05:00:00Z". <br/><br/> Özellikler, hiçbir dosya öznitelik filtresi, veri kümesine uygulanacak anlamına NULL olabilir.  Zaman `modifiedDatetimeStart` datetime değerine sahip ancak `modifiedDatetimeEnd` NULL olduğu için daha büyük olan son değiştirilen özniteliği dosyaları geldiğini veya tarih saat değeri ile eşit seçilir.  Zaman `modifiedDatetimeEnd` datetime değerine sahip ancak `modifiedDatetimeStart` NULL ise, son değiştirilen özniteliği, tarih saat değeri seçilir daha az dosya anlamına gelir.| Hayır |
| modifiedDatetimeEnd | Dosya Filtresi özniteliğine dayanarak: Son değiştirme. Kendi son değiştirilme zamanı zaman aralığı içinde olduğunda dosyaları seçilir `modifiedDatetimeStart` ve `modifiedDatetimeEnd`. Zaman biçimi UTC saat diliminde uygulanan "2018-12-01T05:00:00Z". <br/><br/> Özellikler, hiçbir dosya öznitelik filtresi, veri kümesine uygulanacak anlamına NULL olabilir.  Zaman `modifiedDatetimeStart` datetime değerine sahip ancak `modifiedDatetimeEnd` NULL olduğu için daha büyük olan son değiştirilen özniteliği dosyaları geldiğini veya tarih saat değeri ile eşit seçilir.  Zaman `modifiedDatetimeEnd` datetime değerine sahip ancak `modifiedDatetimeStart` NULL ise, son değiştirilen özniteliği, tarih saat değeri seçilir daha az dosya anlamına gelir.| Hayır |
| format | İsterseniz **olarak dosya kopyalama-olan** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımları biçimi bölümünde atlayın.<br/><br/>Ayrıştırma veya belirli bir biçime sahip dosyaları oluşturmak istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** özelliği şu değerlerden biri olarak biçimine altında. Daha fazla bilgi için [metin biçimi](supported-file-formats-and-compression-codecs.md#text-format), [Json biçimine](supported-file-formats-and-compression-codecs.md#json-format), [Avro biçimi](supported-file-formats-and-compression-codecs.md#avro-format), [Orc biçimi](supported-file-formats-and-compression-codecs.md#orc-format), ve [Parquetbiçimi](supported-file-formats-and-compression-codecs.md#parquet-format) bölümler. |Hayır (yalnızca ikili kopya senaryosu için) |
| compression | Veri sıkıştırma düzeyi ve türünü belirtin. Daha fazla bilgi için [desteklenen dosya biçimleri ve codec sıkıştırma](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Desteklenen türler şunlardır: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**.<br/>Desteklenen düzeyleri şunlardır: **En iyi** ve **hızlı**. |Hayır |

>[!TIP]
>Bir klasör altındaki tüm dosyaları kopyalamak için belirtin **bucketName** demet için ve **önek** klasör bölümü için.<br>Belirli bir ada sahip tek bir dosya kopyalamak için belirtin **bucketName** demet için ve **anahtar** klasör bölümü artı dosya adı.<br>Bir alt klasörü altında bulunan dosyaları kopyalamak için belirtin **bucketName** demet için ve **anahtar** klasör bölümü artı joker karakter filtresi için.

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
            "modifiedDatetimeStart": "2018-12-01T05:00:00Z",
            "modifiedDatetimeEnd": "2018-12-01T06:00:00Z",
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

**Örnek: anahtarı ve sürüm (isteğe bağlı) kullanma**

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

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Amazon S3 kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="amazon-s3-as-source"></a>Amazon S3 kaynağı olarak

- Kopyalama için **Parquet ve sınırlandırılmış metin biçimi**, başvurmak [Parquet ve sınırlandırılmış metin biçimi kaynak](#parquet-and-delimited-text-format-source) bölümü.
- Kopyalama gibi diğer biçimlerinden **ORC/Avro/JSON/ikili biçimi**, başvurmak [başka bir biçim kaynağı](#other-format-source) bölümü.

#### <a name="parquet-and-delimited-text-format-source"></a>Parquet ve sınırlandırılmış metin biçimi kaynağı

Verileri Amazon S3 kopyalamak için **Parquet veya sınırlandırılmış metin biçimi**, başvurmak [Parquet biçimi](format-parquet.md) ve [sınırlandırılmış metin biçimi](format-delimited-text.md) makale biçimi tabanlı kopyalama etkinliği kaynağı ve desteklenen ayarlar. Amazon S3 altında aşağıdaki özellikler desteklenir `storeSettings` biçimi tabanlı kopyalama kaynak ayarları:

| Özellik                 | Açıklama                                                  | Gerekli                                                    |
| ------------------------ | ------------------------------------------------------------ | ----------------------------------------------------------- |
| type                     | Type özelliği altında `storeSettings` ayarlanmalıdır **AmazonS3ReadSetting**. | Evet                                                         |
| recursive                | Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. Özyinelemeli true ve havuz için ayarlandığında bir dosya tabanlı depolama, bir boş klasör veya alt klasör olduğunu unutmayın kopyalanır değil veya havuz oluşturulur. İzin verilen değerler **true** (varsayılan) ve **false**. | Hayır                                                          |
| prefix                   | Filtre kaynak nesneleri veri kümesine yapılandırılan belirli demeti altında S3 nesnesinin anahtarı için önek. Seçili bir nesne anahtarları bu öneki ile başlayın. <br>Yalnızca geçerli `wildcardFolderPath` ve `wildcardFileName` özellikler belirtilmedi. | Hayır                                                          |
| wildcardFolderPath       | Veri kümesi filtresi kaynak klasörleri için yapılandırılan belirli demeti altında joker karakterlerle klasör yolu. <br>Joker karakterlere izin verilir: `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter); kullanma `^` joker karakter veya içinde bu kaçış karakteri, gerçek bir klasör adı varsa, kaçış için. <br>Daha fazla örneklere bakın [klasör ve dosya filtreleme örnekler](#folder-and-file-filter-examples). | Hayır                                                          |
| wildcardFileName         | Belirli bir demet + folderPath/wildcardFolderPath filtre kaynak dosyalarının altında joker karakterler içeren dosya adı. <br>Joker karakterlere izin verilir: `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter); kullanma `^` joker karakter veya içinde bu kaçış karakteri, gerçek bir klasör adı varsa, kaçış için.  Daha fazla örneklere bakın [klasör ve dosya filtreleme örnekler](#folder-and-file-filter-examples). | Yanıt Evet ise `fileName` kümesindeki ve `prefix` belirtilmedi |
| modifiedDatetimeStart    | Dosya Filtresi özniteliğine dayanarak: Son değiştirme. Kendi son değiştirilme zamanı zaman aralığı içinde olduğunda dosyaları seçilir `modifiedDatetimeStart` ve `modifiedDatetimeEnd`. Zaman biçimi UTC saat diliminde uygulanan "2018-12-01T05:00:00Z". <br> Özellikler, hiçbir dosya öznitelik filtresi, veri kümesine uygulanacak anlamına NULL olabilir.  Zaman `modifiedDatetimeStart` datetime değerine sahip ancak `modifiedDatetimeEnd` NULL olduğu için daha büyük olan son değiştirilen özniteliği dosyaları geldiğini veya tarih saat değeri ile eşit seçilir.  Zaman `modifiedDatetimeEnd` datetime değerine sahip ancak `modifiedDatetimeStart` NULL ise, son değiştirilen özniteliği, tarih saat değeri seçilir daha az dosya anlamına gelir. | Hayır                                                          |
| modifiedDatetimeEnd      | Yukarıdakiyle aynı.                                               | Hayır                                                          |
| maxConcurrentConnections | Depolama deposu bağlanmayan bağlantılarının sayısı. Yalnızca veri deposuna eş zamanlı bağlantı sınırlandırmak istediğinizde bu seçeneği belirtin. | Hayır                                                          |

> [!NOTE]
> Parquet ve sınırlandırılmış metin biçimi **FileSystemSource** sonraki bölümde bahsedilen türü kopyalama etkinliği kaynağı olarak desteklenen hala-için geriye dönük uyumluluk içindir. İleride bu yeni modeli kullanmak için önerilir ve bu yeni tür oluşturma için kullanıcı Arabirimi geliştirme ADF geçti.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromAmazonS3",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Delimited text input dataset name>",
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
                "type": "DelimitedTextSource",
                "formatSettings":{
                    "type": "DelimitedTextReadSetting",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "AmazonS3ReadSetting",
                    "recursive": true,
                    "wildcardFolderPath": "myfolder*A",
                    "wildcardFileName": "*.csv"
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="other-format-source"></a>Başka bir biçim kaynağı

Verileri Amazon S3 kopyalamak için **ORC/Avro/JSON/ikili biçimi**, aşağıdaki özellikler kopyalama etkinliğinde desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **FileSystemSource** |Evet |
| recursive | Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. Özyinelemeli true ve havuz için ayarlandığında Not dosya tabanlı depolama, boş klasör/alt-folder havuz kopyalanan/oluşturulmuş olmaz.<br/>İzin verilen değerler: **true** (varsayılan), **false** | Hayır |
| maxConcurrentConnections | Eşzamanlı olarak veri deposuna bağlanmak için bağlantı sayısı. Yalnızca veri deposuna eş zamanlı bağlantı sınırlandırmak istediğinizde bu seçeneği belirtin. | Hayır |

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

### <a name="folder-and-file-filter-examples"></a>Klasör ve dosya filtresi örnekleri

Bu bölümde, sonuçta elde edilen davranışını klasör yolu ve dosya adı joker filtrelerle açıklanmaktadır.

| Demet | anahtar | özyinelemeli | Kaynak klasör yapısını ve filtre sonucunu (kalın dosyalarında alınır)|
|:--- |:--- |:--- |:--- |
| Demet | `Folder*/*` | false | Demet<br/>&nbsp;&nbsp;&nbsp;&nbsp;Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| Demet | `Folder*/*` | true | Demet<br/>&nbsp;&nbsp;&nbsp;&nbsp;Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| Demet | `Folder*/*.csv` | false | Demet<br/>&nbsp;&nbsp;&nbsp;&nbsp;Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| Demet | `Folder*/*.csv` | true | Demet<br/>&nbsp;&nbsp;&nbsp;&nbsp;Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
