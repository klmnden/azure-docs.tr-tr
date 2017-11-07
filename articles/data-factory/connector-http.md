---
title: "Azure Data Factory kullanarak HTTP kaynağından veri kopyalama | Microsoft Docs"
description: "Veri kopyalama etkinliği Azure Data Factory ardışık düzeninde kullanarak Bulut veya şirket içi bir HTTP kaynağından desteklenen havuz veri depolarına kopyalama öğrenin."
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
ms.date: 10/12/2017
ms.author: jingwang
ms.openlocfilehash: 65e3889c975edfad91d7e43baf0599ad98851341
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="copy-data-from-http-endpoint-using-azure-data-factory"></a>Azure Data Factory kullanarak HTTP uç noktasından veri kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-http-connector.md)
> * [Sürüm 2 - Önizleme](connector-http.md)

Bu makalede kopya etkinliği Azure Data Factory'de bir HTTP uç noktasından verileri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 HTTP Bağlayıcısı](v1/data-factory-http-connector.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna HTTP kaynaktan veri kopyalayabilirsiniz. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu HTTP bağlayıcı destekler:

- HTTP kullanarak HTTP/s uç noktasından verilerini alma **almak** veya **POST** yöntemi.
- Aşağıdaki kimlik doğrulamaları kullanarak verileri alınıyor: **anonim**, **temel**, **Özet**, **Windows**, ve  **ClientCertificate**.
- HTTP yanıt olarak kopyalama- ya da onunla ayrıştırma [desteklenen dosya biçimleri ve sıkıştırma codec](supported-file-formats-and-compression-codecs.md).

Bu bağlayıcı arasındaki farkı ve [Web tablo Bağlayıcısı](connector-web-table.md) ikinci web HTML sayfasından tablo içeriği ayıklamak için kullanılmasıdır.

## <a name="getting-started"></a>Başlarken
.NET SDK'sı, Python SDK'sı, Azure PowerShell, REST API veya Azure Resource Manager şablonu kullanarak kopyalama etkinliği ile işlem hattı oluşturabilirsiniz. Bkz: [kopyalama etkinliği öğretici](quickstart-create-data-factory-dot-net.md)) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

Aşağıdaki bölümler, Data Factory varlıklarını belirli HTTP bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikleri, bağlantılı HTTP hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **HttpServer**. | Evet |
| URL | Web sunucusu için temel URL | Evet |
| enableServerCertificateValidation | HTTP uç noktasına bağlanırken sunucu SSL sertifika doğrulamasını etkinleştirmek bu seçeneği belirtin. | Hayır, varsayılan değer true şeklindedir |
| authenticationType | Kimlik doğrulama türünü belirtir. İzin verilen değerler: **anonim**, **temel**, **Özet**, **Windows**, **ClientCertificate**. <br><br> Daha fazla özellikleri ve bu kimlik doğrulama türleri için JSON örnekleri bu tabloda aşağıdaki bölümlerde sırasıyla bakın. | Evet |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu özel bir ağda yer alıyorsa) Azure tümleştirmesi çalışma zamanı veya Self-hosted tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. |Hayır |

### <a name="using-basic-digest-or-windows-authentication"></a>Temel, Özet veya Windows kimlik doğrulaması kullanma

Kümesine "authenticationType" özelliği **temel**, **Özet**, veya **Windows**ve önceki açıklanan genel özellikleri birlikte aşağıdaki özellikleri belirtin Bölüm:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| Kullanıcı adı | HTTP uç noktasına erişmek için kullanıcı adı. | Evet |
| password | (Kullanıcı adı) kullanıcının parolası. Bu alan SecureString işaretleyin. | Evet |

**Örnek**

```json
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "HttpServer",
        "typeProperties":
        {
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

ClientCertificate kimlik doğrulamasını kullanmak için "authenticationType" özelliğini ayarlamak **ClientCertificate**ve önceki bölümde açıklanan genel özelliklerin yanı sıra aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| embeddedCertData | Base64 ile kodlanmış sertifika verileri. | Belirtin `embeddedCertData` veya `certThumbprint`. |
| Certthumbprınt | Sertifikanın parmak izi Self-hosted tümleştirmesi çalışma zamanı makinenizin sertifika deposunda yüklü. Yalnızca kendi kendini barındıran türü Integration zamanının içinde connectVia belirtildiğinde geçerlidir. | Belirtin `embeddedCertData` veya `certThumbprint`. |
| password | Sertifikayla ilişkili parola. Bu alan SecureString işaretleyin. | Hayır |

"Certthumbprınt" için kimlik doğrulaması kullanıyorsanız ve sertifika yerel bilgisayarın kişisel depoda yüklü Self-hosted tümleştirmesi çalışma zamanı için okuma izni vermeniz gerekir:

1. Microsoft Yönetim Konsolu (MMC) başlatın. Ekleme **sertifikaları** hedefleyen eklentisi **yerel bilgisayar**.
2. Genişletme **sertifikaları**, **kişisel**, tıklatıp **Sertifikalar**.
3. Kişisel deposundaki sertifikayı sağ tıklatın ve seçin **tüm görevler** -> **özel anahtarları Yönet...**
3. Üzerinde **güvenlik** sekmesinde, altında tümleştirmesi çalışma zamanı ana bilgisayar hizmeti (Dıahostservice) çalıştığı okuma erişimi sertifikayı kullanıcı hesabını ekleyin.

**Örnek 1: Certthumbprınt kullanma**

```json
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "HttpServer",
        "typeProperties":
        {
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
    "properties":
    {
        "type": "HttpServer",
        "typeProperties":
        {
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

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için veri kümeleri makalesine bakın. Bu bölümde HTTP veri kümesi tarafından desteklenen özellikler listesini sağlar.

HTTP veri kopyalamak için veri kümesi için tür özelliği ayarlamak **HttpFile**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak: **HttpFile** | Evet |
| relativeUrl | Verileri içeren kaynak için göreli bir URL. Bu özellik belirtilmemişse, bağlantılı hizmet tanımında belirtilen URL kullanılır. | Hayır |
| requestMethod | HTTP yöntemi.<br/>İzin verilen değerler **almak** (varsayılan) veya **Post**. | Hayır |
| additionalHeaders | Ek HTTP isteği üstbilgileri. | Hayır |
| RequestBody | HTTP istek gövdesi. | Hayır |
| Biçimi | İsterseniz **HTTP uç noktası olarak veri almak-olan** ve bir dosya tabanlı depolama alanına kopyalama ayrıştırma olmadan, her iki girdi ve çıktı veri kümesi tanımlarında Biçim bölümü atlayın.<br/><br/>HTTP yanıt içeriği kopyalama sırasında ayrıştırma istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** şu değerlerden biri biçimine altında özellik. Daha fazla bilgi için bkz: [Json biçimine](supported-file-formats-and-compression-codecs.md#json-format), [metin biçimi](supported-file-formats-and-compression-codecs.md#text-format), [Avro biçimi](supported-file-formats-and-compression-codecs.md#avro-format), [Orc biçimi](supported-file-formats-and-compression-codecs.md#orc-format), ve [Parquet biçimi](supported-file-formats-and-compression-codecs.md#parquet-format) bölümler. |Hayır |
| Sıkıştırma | Veri sıkıştırma düzeyini ve türünü belirtin. Daha fazla bilgi için bkz: [desteklenen dosya biçimleri ve sıkıştırma codec](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Desteklenen türler: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**.<br/>Desteklenen düzeyler: **Optimal** ve **en hızlı**. |Hayır |

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

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde HTTP kaynağı tarafından desteklenen özellikler listesini sağlar.

### <a name="http-as-source"></a>Kaynak olarak HTTP

HTTP veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **HttpSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **HttpSource** | Evet |
| httpRequestTimeout | Zaman aşımı (TimeSpan) için bir yanıt almak HTTP isteği. Bu zaman aşımı yanıt verileri okumak için zaman aşımına bir yanıt elde etmektir.<br/> Varsayılan değer: 00:01:40  | Hayır |

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
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).