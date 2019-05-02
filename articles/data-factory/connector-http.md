---
title: Azure Data Factory kullanarak bir HTTP kaynaktan veri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına Bulut veya şirket içi bir HTTP kaynağından bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
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
ms.openlocfilehash: f25b0f2c7b5e3148bae778c4b50a3f0bd0c148da
ms.sourcegitcommit: 2c09af866f6cc3b2169e84100daea0aac9fc7fd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64875939"
---
# <a name="copy-data-from-an-http-endpoint-by-using-azure-data-factory"></a>Azure Data Factory kullanarak bir HTTP uç noktasından veri kopyalama

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-http-connector.md)
> * [Geçerli sürüm](connector-http.md)

Bu makalede, kopyalama etkinliği Azure Data Factory'de bir HTTP uç noktasından veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Makaleyi yapılar [Azure veri fabrikasında kopyalama etkinliği](copy-activity-overview.md), kopyalama etkinliği genel bir bakış sunar.

Bu HTTP Bağlayıcısı arasındaki fark [REST'e bağlayıcı](connector-rest.md) ve [Web tablo Bağlayıcısı](connector-web-table.md) şunlardır:

- **REST'e bağlayıcı** özellikle RESTful API'lerinden; verileri kopyalama desteği 
- **HTTP Bağlayıcısı** örn herhangi bir HTTP uç noktasından veri almaya genel dosya indirilemedi. REST'e bağlayıcı kullanılabilir olmadan önce desteklenen ancak daha az işlevsel REST'e bağlayıcı karşılaştırma olduğu RESTful API'den verileri kopyalamak için HTTP Bağlayıcısı'nı kullanmak için oluşabilir.
- **Web tablosu Bağlayıcısı** tablo bir HTML Web sayfası içeriği ayıklar.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bir HTTP kaynağından tüm desteklenen havuz veri deposuna veri kopyalayabilirsiniz. Kopyalama etkinliği kaynak ve havuz olarak desteklediğini veri listesini depolar için bkz: [desteklenen veri depoları ve biçimler](copy-activity-overview.md#supported-data-stores-and-formats).

Bu HTTP Bağlayıcısı için kullanabilirsiniz:

- HTTP kullanarak bir HTTP/S uç noktasından veri **alma** veya **POST** yöntemleri.
- Aşağıdaki kimlik doğrulama kullanarak veri: **Anonim**, **temel**, **Özet**, **Windows**, veya **ClientCertificate**.
- HTTP yanıt olarak kopyalama- ya da kullanarak ayrıştırmayı [desteklenen dosya biçimleri ve codec sıkıştırma](supported-file-formats-and-compression-codecs.md).

> [!TIP]
> Data Factory'de HTTP bağlayıcısını yapılandırabilmeniz için önce bir HTTP isteği için veri alma test etmek için API belirtimine üstbilgi ve gövde gereksinimleri hakkında bilgi edinin. Doğrulamak için Postman veya bir web tarayıcısı gibi araçları kullanabilirsiniz.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler için HTTP Bağlayıcısı özel olan Data Factory varlıkları tanımlamak için kullanabileceğiniz özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

HTTP bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** özelliği ayarlanmalıdır **HttpServer**. | Evet |
| url | Web sunucusuna temel URL'si. | Evet |
| enableServerCertificateValidation | Bir HTTP uç noktasına bağlanırken sunucu SSL sertifika doğrulamasını etkinleştirmek bu seçeneği belirtin. HTTPS sunucunuzun otomatik olarak imzalanan bir sertifika kullanıyorsa, bu özelliği ayarlayın **false**. | Hayır<br /> (varsayılan değer **true**) |
| authenticationType | Kimlik doğrulama türünü belirtir. İzin verilen değerler **anonim**, **temel**, **Özet**, **Windows**, ve **ClientCertificate**. <br><br> Daha fazla özellik ve bu kimlik doğrulama türleri için JSON örnekleri için bu tablodan sonraki bölümlere bakın. | Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel bir ağda yer alıyorsa) Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirme çalışma zamanı bu özelliği kullanır. |Hayır |

### <a name="using-basic-digest-or-windows-authentication"></a>Temel, Özet veya Windows kimlik doğrulamasını kullanma

Ayarlama **authenticationType** özelliğini **temel**, **Özet**, veya **Windows**. Önceki bölümde açıklanan genel özelliklerine ek olarak aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| Kullanıcı adı | HTTP uç noktasına erişmek için kullanılacak kullanıcı adı. | Evet |
| password | Kullanıcının parolasını ( **kullanıcıadı** değeri). Bu alan olarak işaretlemek bir **SecureString** Data Factory'de güvenle depolamak için türü. Ayrıca [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |

**Örnek**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "Basic",
            "url" : "<HTTP endpoint>",
            "userName": "<user name>",
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

### <a name="using-clientcertificate-authentication"></a>ClientCertificate kimlik doğrulaması kullanma

ClientCertificate kimlik doğrulaması kullanacak şekilde ayarlama **authenticationType** özelliğini **ClientCertificate**. Önceki bölümde açıklanan genel özelliklerine ek olarak aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| embeddedCertData | Sertifikayı Base64 ile kodlanmış veriler. | Seçeneklerinden birini belirtin **embeddedCertData** veya **Certthumbprınt**. |
| Certthumbprınt | Şirket içinde barındırılan tümleştirme çalışma zamanı makinenizin sertifika deposunda yüklü sertifika parmak izi. Yalnızca şirket içinde barındırılan tümleştirme çalışma zamanı türü olarak belirtildiğinde geçerlidir **connectVia** özelliği. | Seçeneklerinden birini belirtin **embeddedCertData** veya **Certthumbprınt**. |
| password | Sertifikayla ilişkili parola. Bu alan olarak işaretlemek bir **SecureString** Data Factory'de güvenle depolamak için türü. Ayrıca [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Hayır |

Kullanırsanız **Certthumbprınt** yerel bilgisayarın kişisel depoda kimlik doğrulaması ve sertifika yüklü için okuma izni ver izinleri için şirket içinde barındırılan tümleştirme çalışma zamanı:

1. Microsoft Yönetim Konsolu (MMC) açın. Ekleme **sertifikaları** hedefleyen eklentisini **yerel bilgisayar**.
2. Genişletin **sertifikaları** > **kişisel**ve ardından **sertifikaları**.
3. Kişisel deposundan sertifikaya sağ tıklayın ve ardından **tüm görevler** > **özel anahtarları Yönet**.
3. Üzerinde **güvenlik** sekmesinde, altında Integration Runtime konak hizmeti (Dıahostservice) çalıştığı, sertifika okuma erişimi olan kullanıcı hesabı ekleyin.

**Örnek 1: Certthumbprınt kullanma**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "<HTTP endpoint>",
            "certThumbprint": "<thumbprint of certificate>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek 2: EmbeddedCertData kullanma**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "<HTTP endpoint>",
            "embeddedCertData": "<Base64-encoded cert data>",
            "password": {
                "type": "SecureString",
                "value": "password of cert"
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

HTTP veri kopyalamak için **Parquet veya sınırlandırılmış metin biçimi**, başvurmak [Parquet biçimi](format-parquet.md) ve [sınırlandırılmış metin biçimi](format-delimited-text.md) makale biçimi tabanlı veri kümesinde ve desteklenir Ayarlar. Aşağıdaki özellikler HTTP altında desteklenir `location` biçimi tabanlı bir veri kümesi ayarlarında:

| Özellik    | Açıklama                                                  | Gerekli |
| ----------- | ------------------------------------------------------------ | -------- |
| type        | Type özelliği altında `location` kümesinde ayarlanmalıdır **HttpServerLocation**. | Evet      |
| relativeUrl | Verileri içeren kaynak için göreli bir URL.       | Hayır       |

> [!NOTE]
> Desteklenen HTTP isteği yükü boyutu yaklaşık 500 KB'dir. Web uç noktanıza geçirmek istediğiniz yükü boyutu 500 KB boyutundan büyükse, yükü daha küçük öbekler halinde toplu işleme göz önünde bulundurun.

> [!NOTE]
> **HttpFile** sonraki bölümde bahsedilen Parquet/metin biçimine sahip tür veri kümesi olarak desteklenen hala-kopyalama/arama etkinliği için geriye dönük uyumluluk içindir. İleride bu yeni modeli kullanmak için önerilir ve bu yeni tür oluşturma için kullanıcı Arabirimi geliştirme ADF geçti.

**Örnek:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<HTTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "HttpServerLocation",
                "relativeUrl": "<relative url>"
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

HTTP veri kopyalamak için **ORC/Avro/JSON/ikili biçimi**, aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kümesinin özelliği ayarlanmalıdır **HttpFile**. | Evet |
| relativeUrl | Verileri içeren kaynak için göreli bir URL. Bu özellik belirtilmezse bağlı hizmet tanımında belirtilen URL kullanılır. | Hayır |
| requestMethod | HTTP yöntemi. İzin verilen değerler **alma** (varsayılan) ve **Post**. | Hayır |
| additionalHeaders | Ek HTTP isteği üstbilgileri. | Hayır |
| Includesearchresults: true | HTTP isteğinin gövdesi. | Hayır |
| biçim | HTTP uç noktası olarak veri almak istiyorsanız-ayrıştırma olmadan ve verilerin bir dosya tabanlı depolama alanına kopyalayın atla **biçimi** girdi ve çıktı veri kümesi tanımları bölümünde.<br/><br/>Kopyalama sırasında HTTP yanıt içeriği ayrıştırılamıyor istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, ve **ParquetFormat**. Altında **biçimi**ayarlayın **türü** özelliğini şu değerlerden biri. Daha fazla bilgi için [JSON biçimine](supported-file-formats-and-compression-codecs.md#json-format), [metin biçimi](supported-file-formats-and-compression-codecs.md#text-format), [Avro biçimi](supported-file-formats-and-compression-codecs.md#avro-format), [Orc biçimi](supported-file-formats-and-compression-codecs.md#orc-format), ve [Parquetbiçimi](supported-file-formats-and-compression-codecs.md#parquet-format). |Hayır |
| Sıkıştırma | Veri sıkıştırma düzeyi ve türünü belirtin. Daha fazla bilgi için [desteklenen dosya biçimleri ve codec sıkıştırma](supported-file-formats-and-compression-codecs.md#compression-support).<br/><br/>Desteklenen türler: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**.<br/>Desteklenen düzeyler:  **En iyi** ve **hızlı**. |Hayır |

> [!NOTE]
> Desteklenen HTTP isteği yükü boyutu yaklaşık 500 KB'dir. Web uç noktanıza geçirmek istediğiniz yükü boyutu 500 KB boyutundan büyükse, yükü daha küçük öbekler halinde toplu işleme göz önünde bulundurun.

**Örnek 1: Get yöntemi (varsayılan) kullanma**

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "HttpFile",
        "linkedServiceName": {
            "referenceName": "<HTTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "relativeUrl": "<relative url>",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        }
    }
}
```

**Örnek 2: Post yöntemini kullanma**

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "HttpFile",
        "linkedServiceName": {
            "referenceName": "<HTTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "relativeUrl": "<relative url>",
            "requestMethod": "Post",
            "requestBody": "<body for POST HTTP request>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bu bölümde, HTTP kaynağı desteklediği özelliklerin bir listesini sağlar.

Bölümleri ve etkinlikleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md). 

### <a name="http-as-source"></a>HTTP kaynağı olarak

- Kopyalama için **Parquet ve sınırlandırılmış metin biçimi**, başvurmak [Parquet ve sınırlandırılmış metin biçimi kaynak](#parquet-and-delimited-text-format-source) bölümü.
- Kopyalama gibi diğer biçimlerinden **ORC/Avro/JSON/ikili biçimi**, başvurmak [başka bir biçim kaynağı](#other-format-source) bölümü.

#### <a name="parquet-and-delimited-text-format-source"></a>Parquet ve sınırlandırılmış metin biçimi kaynağı

HTTP veri kopyalamak için **Parquet veya sınırlandırılmış metin biçimi**, başvurmak [Parquet biçimi](format-parquet.md) ve [sınırlandırılmış metin biçimi](format-delimited-text.md) makale biçimi tabanlı kopyalama etkinliği kaynağı ve desteklenen ayarlar. Aşağıdaki özellikler HTTP altında desteklenir `storeSettings` biçimi tabanlı kopyalama kaynak ayarları:

| Özellik                 | Açıklama                                                  | Gerekli |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | Type özelliği altında `storeSettings` ayarlanmalıdır **HttpReadSetting**. | Evet      |
| requestMethod            | HTTP yöntemi. <br>İzin verilen değerler **alma** (varsayılan) ve **Post**. | Hayır       |
| addtionalHeaders         | Ek HTTP isteği üstbilgileri.                             | Hayır       |
| Includesearchresults: true              | HTTP isteğinin gövdesi.                               | Hayır       |
| RequestTimeout           | Zaman aşımı ( **TimeSpan** değeri) bir yanıt almak HTTP isteği için. Bu değer, yanıt verileri okumak için zaman aşımını değil bir yanıt almak için zaman aşımı olur. Varsayılan değer **00:01:40**. | Hayır       |
| MaxConcurrentConnections | Depolama deposu bağlanmayan bağlantılarının sayısı. Yalnızca veri deposuna eş zamanlı bağlantı sınırlandırmak istediğinizde bu seçeneği belirtin. | Hayır       |

> [!NOTE]
> Parquet ve sınırlandırılmış metin biçimi **HttpSource** sonraki bölümde bahsedilen türü kopyalama etkinliği kaynağı olarak desteklenen hala-için geriye dönük uyumluluk içindir. İleride bu yeni modeli kullanmak için önerilir ve bu yeni tür oluşturma için kullanıcı Arabirimi geliştirme ADF geçti.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromHTTP",
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
                    "type": "HttpReadSetting",
                    "requestMethod": "Post",
                    "additionalHeaders": "<header key: header value>\n<header key: header value>\n",
                    "requestBody": "<body for POST HTTP request>"
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

HTTP veri kopyalamak için **ORC/Avro/JSON/ikili biçimi**, aşağıdaki özellikler kopyalama etkinliğinde desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kopyalama etkinliği kaynağı özelliği ayarlanmalıdır **HttpSource**. | Evet |
| httpRequestTimeout | Zaman aşımı ( **TimeSpan** değeri) bir yanıt almak HTTP isteği için. Bu değer, yanıt verileri okumak için zaman aşımını değil bir yanıt almak için zaman aşımı olur. Varsayılan değer **00:01:40**.  | Hayır |

**Örnek**

```json
"activities":[
    {
        "name": "CopyFromHTTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<HTTP input dataset name>",
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
                "type": "HttpSource",
                "httpRequestTimeout": "00:01:00"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```


## <a name="next-steps"></a>Sonraki adımlar

Kopyalama etkinliği kaynak olarak destekler ve şu havuzlar Azure Data Factory'de veri depolarının listesi için bkz. [desteklenen veri depoları ve biçimler](copy-activity-overview.md#supported-data-stores-and-formats).
