---
title: Azure Data Factory kullanarak HTTP kaynağından veri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına Bulut veya şirket içi bir HTTP kaynağından bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/24/2018
ms.author: jingwang
ms.openlocfilehash: 5afb2fccd5c7b8ca306079941837d854c0b21349
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43091726"
---
# <a name="copy-data-from-http-endpoint-using-azure-data-factory"></a>Azure Data Factory kullanarak HTTP uç noktasından veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-http-connector.md)
> * [Geçerli sürüm](connector-http.md)

Bu makalede, kopyalama etkinliği Azure Data Factory'de bir HTTP uç noktasından veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna HTTP kaynaktan veri kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu HTTP Bağlayıcısı destekler:

- HTTP kullanarak HTTP/s uç noktasından veri alma **alma** veya **POST** yöntemi.
- Aşağıdaki kimlik doğrulama kullanarak veri alma: **anonim**, **temel**, **Özet**, **Windows**, ve  **ClientCertificate**.
- HTTP yanıt olarak kopyalama- ya da onunla ayrıştırma [desteklenen dosya biçimleri ve codec sıkıştırma](supported-file-formats-and-compression-codecs.md).

Bu bağlayıcı arasındaki farkı ve [Web tablo Bağlayıcısı](connector-web-table.md) ikinci HTML web sayfasından tablo içeriğini ayıklamak için kullanılmasıdır.

>[!TIP]
>HTTP isteği için HTTP Bağlayıcısı ADF'de yapılandırmadan önce alınırken veri test etmek için başlık ve gövde gereksinimleri API Belirtimi öğrenin ve doğrulamak için Postman veya web tarayıcısı gibi araçları kullanın.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler belirli Data Factory varlıkları için HTTP Bağlayıcısı tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

HTTP bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **HttpServer**. | Evet |
| url | Web sunucusu için temel URL | Evet |
| enableServerCertificateValidation | HTTP uç noktasına bağlanırken sunucu SSL sertifika doğrulamasını etkinleştirmek bu seçeneği belirtin. HTTPS sunucunuzun otomatik olarak imzalanan bir sertifika kullandığında, bunu false olarak ayarlayın. | Hayır, varsayılan değer true şeklindedir |
| authenticationType | Kimlik doğrulama türünü belirtir. İzin verilen değerler: **anonim**, **temel**, **Özet**, **Windows**, **ClientCertificate**. <br><br> Daha fazla özellik ve bu kimlik doğrulama türleri için JSON örnekleri bu tabloda aşağıdaki bölümlere sırasıyla bakın. | Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel ağında bulunuyorsa), Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

### <a name="using-basic-digest-or-windows-authentication"></a>Temel, Özet veya Windows kimlik doğrulamasını kullanma

"AuthenticationType" özelliği ayarlanmış **temel**, **Özet**, veya **Windows**, önceki açıklanan genel özellikleri ile birlikte aşağıdaki özellikleri belirtin Bölüm:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| Kullanıcı adı | HTTP uç noktasına erişmek için kullanıcı adı. | Evet |
| password | (Kullanıcı adı) kullanıcı parolası. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |

**Örnek**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "Basic",
            "url" : "<HTTP endpoint>",
            "userName": "<username>",
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

ClientCertificate kimlik doğrulaması kullanmak için "authenticationType" özelliğini ayarlamak **ClientCertificate**, önceki bölümde açıklanan genel özellikleri ile birlikte aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| embeddedCertData | Base64 olarak kodlanmış sertifika verileri. | Seçeneklerinden birini belirtin `embeddedCertData` veya `certThumbprint`. |
| Certthumbprınt | Şirket içinde barındırılan tümleştirme çalışma zamanı makinenizin sertifika deposunda yüklü sertifika parmak izi. Yalnızca şirket içinde barındırılan tümleştirme çalışma zamanının türünü connectVia belirtildiğinde geçerlidir. | Seçeneklerinden birini belirtin `embeddedCertData` veya `certThumbprint`. |
| password | Sertifikayla ilişkili parola. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Hayır |

"Certthumbprınt" kimlik doğrulaması için kullanmak ve sertifikayı yerel bilgisayarın kişisel depoda yüklü değilse şirket içinde barındırılan tümleştirme çalışma zamanının okuma izni gerekir:

1. Microsoft Yönetim Konsolu (MMC) başlatın. Ekleme **sertifikaları** hedefleyen eklentisini **yerel bilgisayar**.
2. Genişletin **sertifikaları**, **kişisel**, tıklatıp **sertifikaları**.
3. Kişisel deposundan sertifikayı sağ tıklatın ve seçin **tüm görevler** -> **özel anahtarları Yönet...**
3. Üzerinde **güvenlik** sekmesinde, altında Integration Runtime konak hizmeti (Dıahostservice) çalıştığı okuma erişimi ile sertifikayı kullanıcı hesabı ekleyin.

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

**Örnek 2: embeddedCertData kullanma**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "<HTTP endpoint>",
            "embeddedCertData": "<base64 encoded cert data>",
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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için veri kümeleri makalesine bakın. Bu bölümde, HTTP veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

HTTP veri kopyalamak için dataset öğesinin type özelliği ayarlamak **HttpFile**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **HttpFile** | Evet |
| relativeUrl | Verileri içeren kaynak için göreli bir URL. Bu özellik belirtilmemişse, yalnızca bağlı hizmet tanımında belirtilen URL kullanılır. | Hayır |
| requestMethod | HTTP yöntemi.<br/>İzin verilen değerler **alma** (varsayılan) veya **Post**. | Hayır |
| additionalHeaders | Ek HTTP isteği üstbilgileri. | Hayır |
| Includesearchresults: true | HTTP istek gövdesi. | Hayır |
| Biçim | İsterseniz **HTTP uç noktasından veri alma-olan** ve bir dosya tabanlı depolama alanına kopyalama ayrıştırma olmadan hem girdi ve çıktı veri kümesi tanımları biçimi bölümüne atlayın.<br/><br/>Kopyalama sırasında HTTP yanıt içeriği ayrıştırılamıyor istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** özelliği şu değerlerden biri olarak biçimine altında. Daha fazla bilgi için [Json biçimine](supported-file-formats-and-compression-codecs.md#json-format), [metin biçimi](supported-file-formats-and-compression-codecs.md#text-format), [Avro biçimi](supported-file-formats-and-compression-codecs.md#avro-format), [Orc biçimi](supported-file-formats-and-compression-codecs.md#orc-format), ve [Parquetbiçimi](supported-file-formats-and-compression-codecs.md#parquet-format) bölümler. |Hayır |
| Sıkıştırma | Veri sıkıştırma düzeyi ve türünü belirtin. Daha fazla bilgi için [desteklenen dosya biçimleri ve codec sıkıştırma](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Desteklenen türler: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**.<br/>Desteklenen düzeyler: **Optimal** ve **en hızlı**. |Hayır |

>[!NOTE]
>Desteklenen HTTP isteği yükü boyutu yaklaşık 500 KB'dir. Web uç noktanıza geçirmek istediğiniz yükü boyutu bu değerden daha büyük ise, batch için daha küçük öbeklere göz önünde bulundurun.

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

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, HTTP kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="http-as-source"></a>HTTP kaynağı olarak

HTTP veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **HttpSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **HttpSource** | Evet |
| httpRequestTimeout | Zaman aşımı süresi (zaman) HTTP isteği bir yanıt alın. Yanıt verileri okumak için zaman aşımını değil bir yanıt almak için zaman aşımı olan.<br/> Varsayılan değer: 00:01:40  | Hayır |

**Örnek:**

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
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
