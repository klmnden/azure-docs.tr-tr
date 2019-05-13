---
title: HDFS Azure Data Factory kullanarak verileri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına bir bulut veya şirket içi HDFS kaynaktan bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: c528f37c8970380678a318ec2d63babd37f89501
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65228043"
---
# <a name="copy-data-from-hdfs-using-azure-data-factory"></a>Hdfs Azure Data Factory kullanarak veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-hdfs-connector.md)
> * [Geçerli sürüm](connector-hdfs.md)

Bu makalede, HDFS sunucusundan veri kopyalamak nasıl özetlenmektedir. Azure Data Factory hakkında bilgi edinmek için [giriş makalesi](introduction.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bu HDFS Bağlayıcısı için aşağıdaki etkinlikleri desteklenir:

- [Kopyalama etkinliği](copy-activity-overview.md) ile [desteklenen kaynak/havuz Matrisi](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)

Özellikle, bu HDFS bağlayıcı'yı destekler:

- Dosyalar kopyalanıyor kullanarak **Windows** (Kerberos) veya **anonim** kimlik doğrulaması.
- Dosyalar kopyalanıyor kullanarak **webhdfs** protokolü veya **yerleşik DistCp** destekler.
- Dosyaları olarak kopyalama- ya da ayrıştırma/oluşturma dosyalarıyla [desteklenen dosya biçimleri ve codec sıkıştırma](supported-file-formats-and-compression-codecs.md).

## <a name="prerequisites"></a>Önkoşullar

Genel olarak erişilebilir değil bir HDFS veri kopyalamak için şirket içinde barındırılan tümleştirme çalışma zamanını oluşturan gerekir. Bkz: [şirket içinde barındırılan tümleştirme çalışma zamanı](concepts-integration-runtime.md) daha fazla bilgi edinmek için makaleyi.

> [!NOTE]
> Emin Integration Runtime için erişebileceği **tüm** [ad sunucusu düğümü]: [ad, düğüm bağlantı noktası] ve [veri düğümü sunucuları]: [veri düğümü bağlantı noktası] Hadoop kümesi. 50070 varsayılan [ad düğümü bağlantı noktası] ve [veri düğümü bağlantı noktası] 50075 varsayılandır.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli HDFS'ye tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

HDFS bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Hdfs**. | Evet |
| url |HDFS URL'si |Evet |
| authenticationType | İzin verilen değerler şunlardır: **Anonim**, veya **Windows**. <br><br> Kullanılacak **Kerberos kimlik doğrulaması** HDFS bağlayıcısının başvurmak [Bu bölümde](#use-kerberos-authentication-for-hdfs-connector) şirket içi ortamınızı uygun şekilde ayarlamak için. |Evet |
| userName |Kullanıcı adı için Windows kimlik doğrulaması. Kerberos kimlik doğrulaması için belirtin `<username>@<domain>.com`. |Evet (Windows kimlik doğrulaması için) |
| password |Windows kimlik doğrulaması için parola. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Evet (Windows kimlik doğrulaması için) |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz genel olarak erişilebilir değilse), şirket içinde barındırılan tümleştirme çalışma zamanı veya Azure Integration Runtime kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

**Örnek: Anonim kimlik doğrulamasını kullanma**

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "authenticationType": "Anonymous",
            "userName": "hadoop"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek: Windows kimlik doğrulaması kullanma**

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "authenticationType": "Windows",
            "userName": "<username>@<domain>.com (for Kerberos auth)",
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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. 

- İçin **Parquet ve sınırlandırılmış metin biçimi**, başvurmak [Parquet ve sınırlandırılmış metin biçimi veri kümesi](#parquet-and-delimited-text-format-dataset) bölümü.
- Diğer biçimlerden için **ORC/Avro/JSON/ikili biçimi**, başvurmak [diğer biçim veri kümesi](#other-format-dataset) bölümü.

### <a name="parquet-and-delimited-text-format-dataset"></a>Parquet ve sınırlandırılmış metin biçimi veri kümesi

HDFS verileri kopyalamak için **Parquet veya sınırlandırılmış metin biçimi**, başvurmak [Parquet biçimi](format-parquet.md) ve [sınırlandırılmış metin biçimi](format-delimited-text.md) makale biçimi tabanlı veri kümesinde ve desteklenen Ayarlar. Aşağıdaki özellikler altında HDFS için desteklenen `location` biçimi tabanlı bir veri kümesi ayarlarında:

| Özellik   | Açıklama                                                  | Gerekli |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | Type özelliği altında `location` kümesinde ayarlanmalıdır **HdfsLocation**. | Evet      |
| folderPath | Klasör yolu. Joker karakter filtresi klasörüne kullanmak istiyorsanız, bu ayar atlayın ve etkinliği kaynak ayarları belirtin. | Hayır       |
| fileName   | Verilen folderPath altında dosya adı. Joker karakter filtresi dosyalarını kullanmak istiyorsanız, bu ayar atlayın ve etkinliği kaynak ayarları belirtin. | Hayır       |

> [!NOTE]
> **Dosya paylaşımını** sonraki bölümde bahsedilen Parquet/metin biçimine sahip tür veri kümesi olarak desteklenen hala-kopyalama/arama etkinliği için geriye dönük uyumluluk içindir. İleride bu yeni modeli kullanmak için önerilir ve bu yeni tür oluşturma için kullanıcı Arabirimi geliştirme ADF geçti.

**Örnek:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<HDFS linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "HdfsLocation",
                "folderPath": "root/folder/subfolder"
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

HDFS verileri kopyalamak için **ORC/Avro/JSON/ikili biçimi**, aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **Dosya Paylaşımı** |Evet |
| folderPath | Klasör yolu. Joker karakter filtresi desteklenir, joker karakterlere izin şunlardır: `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter); kullanma `^` joker karakter veya içinde bu kaçış karakteri, gerçek dosya adı varsa, kaçış için. <br/><br/>Örnekler: rootfolder/alt/daha fazla örneklere bakın [klasör ve dosya filtreleme örnekler](#folder-and-file-filter-examples). |Evet |
| fileName |  **Adı veya joker karakter filtresi** belirtilen "folderPath" altında dosyaları için. Bu özellik için bir değer belirtmezseniz, klasördeki tüm dosyaları için veri kümesini işaret eder. <br/><br/>Filtre için joker karakterlere izin verilir: `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter).<br/>-Örnek 1: `"fileName": "*.csv"`<br/>-Örnek 2: `"fileName": "???20180427.txt"`<br/>Kullanım `^` joker karakter veya içinde bu kaçış karakteri, gerçek bir klasör adı varsa, kaçış için. |Hayır |
| modifiedDatetimeStart | Dosya Filtresi özniteliğine dayanarak: Son değiştirme. Kendi son değiştirilme zamanı zaman aralığı içinde olduğunda dosyaları seçilir `modifiedDatetimeStart` ve `modifiedDatetimeEnd`. Zaman biçimi UTC saat diliminde uygulanan "2018-12-01T05:00:00Z". <br/><br/> Veri taşıma genel performansını filtre devasa miktarda bir dosyaları dosya istediğinizde, bu ayarı etkinleştirerek etkilenecek dikkat edin. <br/><br/> Özellikler, hiçbir dosya öznitelik filtresi, veri kümesine uygulanacak anlamına NULL olabilir.  Zaman `modifiedDatetimeStart` datetime değerine sahip ancak `modifiedDatetimeEnd` NULL olduğu için daha büyük olan son değiştirilen özniteliği dosyaları geldiğini veya tarih saat değeri ile eşit seçilir.  Zaman `modifiedDatetimeEnd` datetime değerine sahip ancak `modifiedDatetimeStart` NULL ise, son değiştirilen özniteliği, tarih saat değeri seçilir daha az dosya anlamına gelir.| Hayır |
| modifiedDatetimeEnd | Dosya Filtresi özniteliğine dayanarak: Son değiştirme. Kendi son değiştirilme zamanı zaman aralığı içinde olduğunda dosyaları seçilir `modifiedDatetimeStart` ve `modifiedDatetimeEnd`. Zaman biçimi UTC saat diliminde uygulanan "2018-12-01T05:00:00Z". <br/><br/> Veri taşıma genel performansını filtre devasa miktarda bir dosyaları dosya istediğinizde, bu ayarı etkinleştirerek etkilenecek dikkat edin. <br/><br/> Özellikler, hiçbir dosya öznitelik filtresi, veri kümesine uygulanacak anlamına NULL olabilir.  Zaman `modifiedDatetimeStart` datetime değerine sahip ancak `modifiedDatetimeEnd` NULL olduğu için daha büyük olan son değiştirilen özniteliği dosyaları geldiğini veya tarih saat değeri ile eşit seçilir.  Zaman `modifiedDatetimeEnd` datetime değerine sahip ancak `modifiedDatetimeStart` NULL ise, son değiştirilen özniteliği, tarih saat değeri seçilir daha az dosya anlamına gelir.| Hayır |
| format | İsterseniz **olarak dosya kopyalama-olan** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımları biçimi bölümünde atlayın.<br/><br/>Belirli bir biçime sahip dosyalarını ayrıştırmak istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** özelliği şu değerlerden biri olarak biçimine altında. Daha fazla bilgi için [metin biçimi](supported-file-formats-and-compression-codecs.md#text-format), [Json biçimine](supported-file-formats-and-compression-codecs.md#json-format), [Avro biçimi](supported-file-formats-and-compression-codecs.md#avro-format), [Orc biçimi](supported-file-formats-and-compression-codecs.md#orc-format), ve [Parquetbiçimi](supported-file-formats-and-compression-codecs.md#parquet-format) bölümler. |Hayır (yalnızca ikili kopya senaryosu için) |
| compression | Veri sıkıştırma düzeyi ve türünü belirtin. Daha fazla bilgi için [desteklenen dosya biçimleri ve codec sıkıştırma](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Desteklenen türler şunlardır: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**.<br/>Desteklenen düzeyleri şunlardır: **En iyi** ve **hızlı**. |Hayır |

>[!TIP]
>Bir klasör altındaki tüm dosyaları kopyalamak için belirtin **folderPath** yalnızca.<br>Belirli bir ada sahip tek bir dosya kopyalamak için belirtin **folderPath** klasör bölümüyle ve **fileName** dosya adına sahip.<br>Bir alt klasörü altında bulunan dosyaları kopyalamak için belirtin **folderPath** klasör bölümüyle ve **fileName** joker karakter Filtresi ile.

**Örnek:**

```json
{
    "name": "HDFSDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<HDFS linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "folder/subfolder/",
            "fileName": "*",
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

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, HDFS kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="hdfs-as-source"></a>Kaynak olarak HDFS

- Kopyalama için **Parquet ve sınırlandırılmış metin biçimi**, başvurmak [Parquet ve sınırlandırılmış metin biçimi kaynak](#parquet-and-delimited-text-format-source) bölümü.
- Kopyalama gibi diğer biçimlerinden **ORC/Avro/JSON/ikili biçimi**, başvurmak [başka bir biçim kaynağı](#other-format-source) bölümü.

#### <a name="parquet-and-delimited-text-format-source"></a>Parquet ve sınırlandırılmış metin biçimi kaynağı

HDFS verileri kopyalamak için **Parquet veya sınırlandırılmış metin biçimi**, başvurmak [Parquet biçimi](format-parquet.md) ve [sınırlandırılmış metin biçimi](format-delimited-text.md) makale biçimi tabanlı kopyalama etkinliği kaynağı ve desteklenen ayarlar. Aşağıdaki özellikler altında HDFS için desteklenen `storeSettings` biçimi tabanlı kopyalama kaynak ayarları:

| Özellik                 | Açıklama                                                  | Gerekli                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | Type özelliği altında `storeSettings` ayarlanmalıdır **HdfsReadSetting**. | Evet                                           |
| recursive                | Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. Özyinelemeli true ve havuz için ayarlandığında bir dosya tabanlı depolama, bir boş klasör veya alt klasör olduğunu unutmayın kopyalanır değil veya havuz oluşturulur. İzin verilen değerler **true** (varsayılan) ve **false**. | Hayır                                            |
| wildcardFolderPath       | Kaynak klasörleri filtrelemek için joker karakter içeren klasör yolu. <br>Joker karakterlere izin verilir: `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter); kullanma `^` joker karakter veya içinde bu kaçış karakteri, gerçek bir klasör adı varsa, kaçış için. <br>Daha fazla örneklere bakın [klasör ve dosya filtreleme örnekler](#folder-and-file-filter-examples). | Hayır                                            |
| wildcardFileName         | Joker karakter filtresi kaynak dosyalarının belirli folderPath/wildcardFolderPath altında içeren dosya adı. <br>Joker karakterlere izin verilir: `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter); kullanma `^` joker karakter veya içinde bu kaçış karakteri, gerçek bir klasör adı varsa, kaçış için.  Daha fazla örneklere bakın [klasör ve dosya filtreleme örnekler](#folder-and-file-filter-examples). | Yanıt Evet ise `fileName` kümesinde belirtilmedi |
| modifiedDatetimeStart    | Dosya Filtresi özniteliğine dayanarak: Son değiştirme. Kendi son değiştirilme zamanı zaman aralığı içinde olduğunda dosyaları seçilir `modifiedDatetimeStart` ve `modifiedDatetimeEnd`. Zaman biçimi UTC saat diliminde uygulanan "2018-12-01T05:00:00Z". <br> Özellikler, hiçbir dosya öznitelik filtresi, veri kümesine uygulanacak anlamına NULL olabilir.  Zaman `modifiedDatetimeStart` datetime değerine sahip ancak `modifiedDatetimeEnd` NULL olduğu için daha büyük olan son değiştirilen özniteliği dosyaları geldiğini veya tarih saat değeri ile eşit seçilir.  Zaman `modifiedDatetimeEnd` datetime değerine sahip ancak `modifiedDatetimeStart` NULL ise, son değiştirilen özniteliği, tarih saat değeri seçilir daha az dosya anlamına gelir. | Hayır                                            |
| modifiedDatetimeEnd      | Yukarıdakiyle aynı.                                               | Hayır                                            |
| maxConcurrentConnections | Depolama deposu bağlanmayan bağlantılarının sayısı. Yalnızca veri deposuna eş zamanlı bağlantı sınırlandırmak istediğinizde bu seçeneği belirtin. | Hayır                                            |

> [!NOTE]
> Parquet ve sınırlandırılmış metin biçimi **FileSystemSource** sonraki bölümde bahsedilen türü kopyalama etkinliği kaynağı olarak desteklenen hala-geriye dönük uyumluluğu içindir. İleride bu yeni modeli kullanmak için önerilir ve bu yeni tür oluşturma için kullanıcı Arabirimi geliştirme ADF geçti.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromHDFS",
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
                    "type": "HdfsReadSetting",
                    "recursive": true
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

HDFS verileri kopyalamak için **ORC/Avro/JSON/ikili biçimi**, aşağıdaki özellikleri kopyalama etkinliğinde desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **HdfsSource** |Evet |
| recursive | Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. Özyinelemeli true ve havuz için ayarlandığında Not dosya tabanlı depolama, boş klasör/alt-folder havuz kopyalanan/oluşturulmuş olmaz.<br/>İzin verilen değerler: **true** (varsayılan), **false** | Hayır |
| distcpSettings | HDFS DistCp kullanırken özellik grubu. | Hayır |
| resourceManagerEndpoint | Yarn Kaynak Yöneticisi uç noktası | DistCp kullanıyorsanız Evet |
| tempScriptPath | Geçici DistCp komut dosyasını depolamak için kullanılan bir klasör yolu. Komut dosyası, Data Factory tarafından oluşturulur ve kopyalama işi tamamlandıktan sonra kaldırılır. | DistCp kullanıyorsanız Evet |
| distcpOptions | DistCp komutu için sağlanan ek seçenekler. | Hayır |
| maxConcurrentConnections | Depolama deposu bağlanmayan bağlantılarının sayısı. Yalnızca veri deposuna eş zamanlı bağlantı sınırlandırmak istediğinizde bu seçeneği belirtin. | Hayır |

**Örnek: HDFS kaynak kopyalama etkinliğindeki DistCp kullanma**

```json
"source": {
    "type": "HdfsSource",
    "distcpSettings": {
        "resourceManagerEndpoint": "resourcemanagerendpoint:8088",
        "tempScriptPath": "/usr/hadoop/tempscript",
        "distcpOptions": "-m 100"
    }
}
```

Verileri HDFS sonraki bölümünden verimli bir şekilde kopyalamak için DistCp kullanma konusunda daha fazla bilgi edinin.

### <a name="folder-and-file-filter-examples"></a>Klasör ve dosya filtresi örnekleri

Bu bölümde, sonuçta elde edilen davranışını klasör yolu ve dosya adı joker filtrelerle açıklanmaktadır.

| folderPath | fileName             | recursive | Kaynak klasör yapısını ve filtre sonucunu (dosyalar **kalın** alınır) |
| :--------- | :------------------- | :-------- | :----------------------------------------------------------- |
| `Folder*`  | (boş, varsayılan kullanın) | false     | Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*`  | (boş, varsayılan kullanın) | true      | Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*`  | `*.csv`              | false     | Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*`  | `*.csv`              | true      | Klasörüdür<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

## <a name="use-distcp-to-copy-data-from-hdfs"></a>HDFS veri kopyalamak için DistCp kullanma

[DistCp](https://hadoop.apache.org/docs/current3/hadoop-distcp/DistCp.html) bir Hadoop kümesinde dağıtılmış kopyalamayı yapmak için bir Hadoop yerel komut satırı aracıdır. Distcp komutu çalıştırdığınızda, ilk kopyalanması, Hadoop kümesine birkaç harita işleri oluşturmak için tüm dosyaları listelenir ve her bir harita iş ikili kopyalama havuz kaynağından ne yapacağını.

Kopyalama etkinliği destek dosyaları olarak kopyalamak için DistCp kullanma-Azure Blob içinde olduğu (dahil olmak üzere [kopyalama aşamalı](copy-activity-performance.md) veya Azure Data Lake Store, bu durumda, tam olarak şirket içinde barındırılan tümleştirme çalışma zamanını çalıştırmak yerine kümenizin power yararlanabilir . Özellikle, kümenizin çok güçlü ise daha iyi kopyalama aktarım hızı sağlar. Azure Data factory'de yapılandırmanıza bağlı olarak, kopyalama etkinliği otomatik olarak distcp komutu oluşturun, Hadoop kümenizi gönderin ve kopya durumunu izleyin.

### <a name="prerequisites"></a>Önkoşullar

Kopyalamak için DistCp kullanma dosyaları olarak-olan HDFS (hazırlanmış kopya dahil) Azure Blob veya Azure Data Lake Store, Hadoop kümenizi gereksinimleri karşıladığından emin olun:

1. MapReduce ve Yarn hizmetler etkinleştirilir.
2. Yarn sürümüdür 2.5 veya üstü.
3. HDFS sunucusuna, hedef veri deposu ile - Azure Blob veya Azure Data Lake Store tümleştirilir:

    - Azure Blob dosya sistemi, Hadoop 2.7 beri yerel olarak desteklenir. Yalnızca Hadoop env yapılandırmada jar yolunu belirtmeniz gerekir.
    - Hadoop 3.0.0-alpha1 ' itibaren Azure Data Lake Store dosya sistemi ile paketlenmiştir. Hadoop kümenizi Bu sürümden daha düşükse, ilgili ADLS el ile almanız gerekir kümeden halinde paketler (azure-datalake-store.jar) jar [burada](https://hadoop.apache.org/releases.html)ve Hadoop env yapılandırmada jar yolunu belirtin.

4. HDFS geçici bir klasörde hazırlayın. Bu geçici klasörü DistCp Kabuk betiği KB düzeyinde alanı kaplar şekilde depolamak için kullanılır.
5. Emin olun, HDFS bağlı hizmette sağlanan kullanıcı hesabı için izne sahip bir); Yarn uygulama gönderme (b) bir alt klasör oluşturun, yukarıda geçici klasörü altındaki dosyaları okuma/yazma iznine sahip.

### <a name="configurations"></a>Yapılandırmalar

DistCp bkz ilgili yapılandırmaları ve örneklerde [kaynağı olarak HDFS](#hdfs-as-source) bölümü.

## <a name="use-kerberos-authentication-for-hdfs-connector"></a>HDFS Bağlayıcısı için Kerberos kimlik doğrulaması kullan

HDFS bağlayıcısında Kerberos kimlik doğrulamasını kullanmak için şirket içi ortamı ayarlamak için iki seçenek vardır. Bir Olayınıza daha iyi uyduğunu seçebilirsiniz.
* 1. seçenek: [Şirket içinde barındırılan tümleştirme çalışma zamanı makine Kerberos bölgesinde katılın](#kerberos-join-realm)
* 2. seçenek: [Windows etki alanı ve Kerberos alanı arasında karşılıklı güven etkinleştir](#kerberos-mutual-trust)

### <a name="kerberos-join-realm"></a>1. seçenek: Şirket içinde barındırılan tümleştirme çalışma zamanı makine Kerberos bölgesinde katılın

#### <a name="requirements"></a>Gereksinimler

* Şirket içinde barındırılan tümleştirme çalışma zamanı makine Kerberos alanı katılmak gereken ve herhangi bir Windows etki alanına katılma işlemi gerçekleştirilemiyor.

#### <a name="how-to-configure"></a>Yapılandırma

**Şirket içinde barındırılan tümleştirme çalışma zamanı makinesinde:**

1.  Çalıştırma **Ksetup** Kerberos KDC sunucu ve bölgeyi yapılandırmak için yardımcı program.

    Kerberos alanı bir Windows etki alanından farklı olduğundan makine bir çalışma grubunun üyesi yapılandırılmalıdır. Bu, Kerberos alanı ayarlama ve şu şekilde bir KDC sunucu ekleyerek gerçekleştirilebilir. Değiştirin *REALM.COM* gerektiği gibi ilgili kendi bölge ile.

            C:> Ksetup /setdomain REALM.COM
            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>

    **Yeniden** 2 Bu komutları yürütmeden sonra makine.

2.  Yapılandırmayı doğrulamak **Ksetup** komutu. Çıkış, gibi olmalıdır:

            C:> Ksetup
            default realm = REALM.COM (external)
            REALM.com:
                kdc = <your_kdc_server_address>

**Azure Data Factory'de:**

* HDFS bağlayıcısını kullanarak yapılandırma **Windows kimlik doğrulaması** Kerberos asıl adı ve HDFS veri kaynağına bağlanmak için parola. Denetleme [HDFS bağlı hizmeti özellikleri](#linked-service-properties) yapılandırma ayrıntıları bölümü.

### <a name="kerberos-mutual-trust"></a>2. seçenek: Windows etki alanı ve Kerberos alanı arasında karşılıklı güven etkinleştir

#### <a name="requirements"></a>Gereksinimler

*   Şirket içinde barındırılan tümleştirme çalışma zamanı bilgisayarın Windows etki alanına Katıl gerekir.
*   Etki alanı denetleyicisinin ayarlarını güncelleştirmek için izne ihtiyacınız var.

#### <a name="how-to-configure"></a>Yapılandırma

> [!NOTE]
> REALM.COM ve AD.COM aşağıdaki öğreticide kendi ilgili bölge ve etki alanı denetleyicisi gerektiği gibi değiştirin.

**KDC sunucusunda:**

1. KDC yapılandırmayı Düzenle **krb5.conf** KDC izin vermek için dosyanın Windows aşağıdaki yapılandırma şablonuna başvuran etki alanı güven. Varsayılan olarak, şu yapılandırma konumdadır **/etc/krb5.conf**.

           [logging]
            default = FILE:/var/log/krb5libs.log
            kdc = FILE:/var/log/krb5kdc.log
            admin_server = FILE:/var/log/kadmind.log
            
           [libdefaults]
            default_realm = REALM.COM
            dns_lookup_realm = false
            dns_lookup_kdc = false
            ticket_lifetime = 24h
            renew_lifetime = 7d
            forwardable = true
            
           [realms]
            REALM.COM = {
             kdc = node.REALM.COM
             admin_server = node.REALM.COM
            }
           AD.COM = {
            kdc = windc.ad.com
            admin_server = windc.ad.com
           }
            
           [domain_realm]
            .REALM.COM = REALM.COM
            REALM.COM = REALM.COM
            .ad.com = AD.COM
            ad.com = AD.COM
            
           [capaths]
            AD.COM = {
             REALM.COM = .
            }

   **Yeniden** yapılandırma sonrasında KDC Hizmeti.

2. Adlı bir asıl hazırlama **krbtgt/REALM.COM\@AD.COM** KDC sunucusunda aşağıdaki komutla:

           Kadmin> addprinc krbtgt/REALM.COM@AD.COM

3. İçinde **hadoop.security.auth_to_local** HDFS hizmet yapılandırma dosyasında, ekleme `RULE:[1:$1@$0](.*\@AD.COM)s/\@.*//`.

**Etki alanı denetleyicisinde:**

1.  Aşağıdaki komutu çalıştırın **Ksetup** komutları bölge giriş eklemek için:

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

2.  Kerberos alanı için Windows etki alanı güven oluşturun. [parola] olan asıl parola **krbtgt/REALM.COM\@AD.COM**.

            C:> netdom trust REALM.COM /Domain: AD.COM /add /realm /passwordt:[password]

3.  Kerberos kullanılan şifreleme algoritması seçin.

    1. Sunucu Yöneticisi'ne gidin > Grup İlkesi Yönetimi > etki alanı > Grup İlkesi nesneleri > Varsayılan veya etkin bir etki alanı ilkesi ve düzenleyin.

    2. İçinde **Grup İlkesi Yönetimi Düzenleyicisi** açılan pencere, bilgisayar yapılandırması gidin > ilkeler > Windows Ayarları > Güvenlik Ayarları > yerel ilkeler > güvenlik seçeneklerini ve yapılandırma **ağ Güvenlik: Kerberos'ta izin verilen şifreleme türlerini yapılandır**.

    3. Seçin, kullanmak istediğiniz şifreleme algoritması için KDC bağlanın. Genellikle, yalnızca tüm seçenekleri seçebilirsiniz.

        ![Kerberos için şifreleme türlerini yapılandırma](media/connector-hdfs/config-encryption-types-for-kerberos.png)

    4. Kullanım **Ksetup** belirli bölge üzerinde kullanılacak şifreleme algoritmasını belirtmek için komutu.

                C:> ksetup /SetEncTypeAttr REALM.COM DES-CBC-CRC DES-CBC-MD5 RC4-HMAC-MD5 AES128-CTS-HMAC-SHA1-96 AES256-CTS-HMAC-SHA1-96

4.  Windows etki alanında Kerberos asıl kullanmak için etki alanı hesabını ve Kerberos asıl arasındaki eşleme oluşturun.

    1. Yönetimsel Araçları başlatmak > **Active Directory Kullanıcıları ve Bilgisayarları**.

    2. Tıklayarak Gelişmiş özellikleri yapılandırma **görünümü** > **Gelişmiş Özellikler**.

    3. Hangi eşlemeleri oluşturmak istiyorsanız ve görüntülemek için sağ hesabını bulun **adı eşlemeleri** > tıklatın **Kerberos adları** sekmesi.

    4. Sorumlu bölge ekleyin.

        ![Güvenlik kimliğine eşleyin](media/connector-hdfs/map-security-identity.png)

**Şirket içinde barındırılan tümleştirme çalışma zamanı makinesinde:**

* Aşağıdaki komutu çalıştırın **Ksetup** bölge giriş eklemek için komutları.

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

**Azure Data Factory'de:**

* HDFS bağlayıcısını kullanarak yapılandırma **Windows kimlik doğrulaması** etki alanı hesabı veya Kerberos asıl HDFS veri kaynağına bağlanmak için birlikte. Denetleme [HDFS bağlı hizmeti özellikleri](#linked-service-properties) yapılandırma ayrıntıları bölümü.


## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
