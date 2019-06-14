---
title: Azure Data Factory kullanarak bir REST kaynağı'ndan veri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına bir bulut veya şirket içi REST kaynaktan bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: jingwang
ms.openlocfilehash: ee47f464c59bd9deed98671f19cfcc6d2c3c1b39
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60546651"
---
# <a name="copy-data-from-a-rest-endpoint-by-using-azure-data-factory"></a>Azure Data Factory kullanarak bir REST uç noktasından veri kopyalama

Bu makalede, kopyalama etkinliği Azure Data Factory'de bir REST uç noktasından veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Makaleyi yapılar [Azure veri fabrikasında kopyalama etkinliği](copy-activity-overview.md), kopyalama etkinliği genel bir bakış sunar.

Bu REST Bağlayıcısı arasındaki fark [HTTP Bağlayıcısı](connector-http.md) ve [Web tablo Bağlayıcısı](connector-web-table.md) şunlardır:

- **REST'e bağlayıcı** özellikle RESTful API'lerinden; verileri kopyalama desteği 
- **HTTP Bağlayıcısı** örn herhangi bir HTTP uç noktasından veri almaya genel dosya indirilemedi. Bu REST bağlayıcı kullanılabilir olmadan önce desteklenen ancak daha az işlevsel REST'e bağlayıcı karşılaştırma olduğu RESTful API'den verileri kopyalamak için HTTP Bağlayıcısı'nı kullanmak için oluşabilir.
- **Web tablosu Bağlayıcısı** tablo bir HTML Web sayfası içeriği ayıklar.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bir REST kaynağı'ndan veri her desteklenen havuz veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği kaynak ve havuz olarak desteklediğini veri listesini depolar için bkz: [desteklenen veri depoları ve biçimler](copy-activity-overview.md#supported-data-stores-and-formats).

Özellikle, bu genel bir REST bağlayıcıyı destekler:

- Kullanarak bir REST uç noktasından veri alma **alma** veya **POST** yöntemleri.
- Aşağıdaki kimlik doğrulamaları birini kullanarak verileri alınıyor: **Anonim**, **temel**, **AAD hizmet sorumlusu**, ve **kimliklerini Azure kaynakları için yönetilen**.
- **[Sayfalandırma](#pagination-support)**  REST API'leri de.
- REST JSON yanıtı kopyalama [olarak-olan](#export-json-response-as-is) veya kullanarak ayrıştırmayı [şema eşleme](copy-activity-schema-and-type-mapping.md#schema-mapping). Yalnızca yanıt yükünde **JSON** desteklenir.

> [!TIP]
> Data Factory REST bağlayıcısını yapılandırabilmeniz için önce veri alma isteği test etmek için API belirtimine üstbilgi ve gövde gereksinimleri hakkında bilgi edinin. Doğrulamak için Postman veya bir web tarayıcısı gibi araçları kullanabilirsiniz.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, REST bağlayıcıya özgü Data Factory varlıkları tanımlamak için kullanabileceğiniz özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Aşağıdaki özellikleri bağlantılı REST hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** özelliği ayarlanmalıdır **RestService**. | Evet |
| url | REST hizmeti temel URL'si. | Evet |
| enableServerCertificateValidation | Uç noktasına bağlanırken sunucu tarafı SSL sertifikasını doğrulamak belirtir. | Hayır<br /> (varsayılan değer **true**) |
| authenticationType | REST hizmete bağlanmak için kullanılan kimlik doğrulaması türü. İzin verilen değerler **anonim**, **temel**, **AadServicePrincipal** ve **ManagedServiceIdentity**. Daha fazla özellikler ve örnekler üzerinde aşağıdaki karşılık gelen bölümlere sırasıyla bakın. | Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel bir ağda yer alıyorsa) Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirme çalışma zamanı bu özelliği kullanır. |Hayır |

### <a name="use-basic-authentication"></a>Temel kimlik doğrulaması kullan

Ayarlama **authenticationType** özelliğini **temel**. Önceki bölümde açıklanan genel özelliklerine ek olarak aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| userName adı | REST uç noktasına erişmek için kullanılacak kullanıcı adı. | Evet |
| password | Kullanıcının parolasını ( **kullanıcıadı** değeri). Bu alan olarak işaretlemek bir **SecureString** Data Factory'de güvenle depolamak için türü. Ayrıca [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |

**Örnek**

```json
{
    "name": "RESTLinkedService",
    "properties": {
        "type": "RestService",
        "typeProperties": {
            "authenticationType": "Basic",
            "url" : "<REST endpoint>",
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

### <a name="use-aad-service-principal-authentication"></a>AAD hizmet sorumlusu kimlik doğrulaması kullanma

Ayarlama **authenticationType** özelliğini **AadServicePrincipal**. Önceki bölümde açıklanan genel özelliklerine ek olarak aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| servicePrincipalId | Azure Active Directory Uygulama istemci kimliği belirtin. | Evet |
| servicePrincipalKey | Azure Active Directory Uygulama anahtarını belirtin. Bu alan olarak işaretlemek bir **SecureString** Data Factory'de güvenle depolamak için veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| tek | Kiracı bilgileri (etki alanı adı veya Kiracı kimliği), uygulamanızın bulunduğu altında belirtin. Bu, Azure portalının sağ üst köşedeki fare gelerek alın. | Evet |
| aadResourceId | Belirtmek istediğiniz yetkilendirme için örneğin AAD kaynak `https://management.core.windows.net`.| Evet |

**Örnek**

```json
{
    "name": "RESTLinkedService",
    "properties": {
        "type": "RestService",
        "typeProperties": {
            "url": "<REST endpoint e.g. https://www.example.com/>",
            "authenticationType": "AadServicePrincipal",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "value": "<service principal key>",
                "type": "SecureString"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "aadResourceId": "<AAD resource URL e.g. https://management.core.windows.net>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identity"></a> Azure kaynakları ile kimlik doğrulaması için yönetilen kimlikleri kullanmak

Ayarlama **authenticationType** özelliğini **ManagedServiceIdentity**. Önceki bölümde açıklanan genel özelliklerine ek olarak aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| aadResourceId | Belirtmek istediğiniz yetkilendirme için örneğin AAD kaynak `https://management.core.windows.net`.| Evet |

**Örnek**

```json
{
    "name": "RESTLinkedService",
    "properties": {
        "type": "RestService",
        "typeProperties": {
            "url": "<REST endpoint e.g. https://www.example.com/>",
            "authenticationType": "ManagedServiceIdentity",
            "aadResourceId": "<AAD resource URL e.g. https://management.core.windows.net>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bu bölümde REST veri kümesini destekleyen özelliklerin bir listesini sağlar. 

Bölümleri ve veri kümeleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [veri kümeleri ve bağlı hizmetler](concepts-datasets-linked-services.md). 

Verileri geri KALANINDAN kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kümesinin özelliği ayarlanmalıdır **RestResource**. | Evet |
| relativeUrl | Verileri içeren kaynak için göreli bir URL. Bu özellik belirtilmezse bağlı hizmet tanımında belirtilen URL kullanılır. | Hayır |
| requestMethod | HTTP yöntemi. İzin verilen değerler **alma** (varsayılan) ve **Post**. | Hayır |
| additionalHeaders | Ek HTTP isteği üstbilgileri. | Hayır |
| Includesearchresults: true | HTTP isteğinin gövdesi. | Hayır |
| paginationRules | Sonraki sayfa istekleri oluşturmak için sayfalandırma kurallar. Başvurmak [sayfalandırma Destek](#pagination-support) ayrıntıları bölümü. | Hayır |

**Örnek 1: Get yöntemi ile sayfalandırma kullanma**

```json
{
    "name": "RESTDataset",
    "properties": {
        "type": "RestResource",
        "linkedServiceName": {
            "referenceName": "<REST linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "relativeUrl": "<relative url>",
            "additionalHeaders": {
                "x-user-defined": "helloworld"
            },
            "paginationRules": {
                "AbsoluteUrl": "$.paging.next"
            }
        }
    }
}
```

**Örnek 2: Post yöntemini kullanma**

```json
{
    "name": "RESTDataset",
    "properties": {
        "type": "RestResource",
        "linkedServiceName": {
            "referenceName": "<REST linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "relativeUrl": "<relative url>",
            "requestMethod": "Post",
            "requestBody": "<body for POST REST request>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bu bölümde REST kaynağının desteklediği özelliklerin bir listesini sağlar.

Bölümleri ve etkinlikleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md). 

### <a name="rest-as-source"></a>Kaynak olarak tutun

Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kopyalama etkinliği kaynağı özelliği ayarlanmalıdır **RestSource**. | Evet |
| httpRequestTimeout | Zaman aşımı ( **TimeSpan** değeri) bir yanıt almak HTTP isteği için. Bu değer, yanıt verileri okumak için zaman aşımını değil bir yanıt almak için zaman aşımı olur. Varsayılan değer **00:01:40**.  | Hayır |
| requestInterval | Sonraki sayfa isteği göndermeden önce beklenecek süre. Varsayılan değer **00:00:01** |  Hayır |

**Örnek**

```json
"activities":[
    {
        "name": "CopyFromREST",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<REST input dataset name>",
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
                "type": "RestSource",
                "httpRequestTimeout": "00:01:00"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="pagination-support"></a>Sayfalandırma desteği

Normalde, REST API, makul sayıda altında tek bir istek, yanıt yükü boyutunu sınırlamak; while büyük miktarda veri, döndürülecek sonuç birden çok sayfalarına böler ve sonucun sonraki sayfaya ulaşmak için art arda istekler göndermek çağıranlar gerektirir. Genellikle, tek bir sayfaya dinamik ve önceki sayfaya yanıttan döndürülen bilgileri tarafından oluşturulan isteğidir.

Bu genel bir REST Bağlayıcısı aşağıdaki sayfalandırma desenleri destekler: 

* Sonraki isteğin mutlak veya göreli URL'si özellik değeri geçerli bir yanıt gövdesindeki =
* Sonraki isteğin mutlak veya göreli URL'si geçerli yanıt üst bilgilerinde de üst bilgi değeri =
* Sonraki isteğin sorgu parametresi özellik değeri geçerli bir yanıt gövdesindeki =
* Sonraki isteğin sorgu parametresi geçerli yanıt üst bilgilerinde de üst bilgi değeri =
* Sonraki isteğin başlık özellik değeri geçerli bir yanıt gövdesindeki =
* Sonraki isteğin başlığı üst bilgi değeri geçerli yanıt üst bilgilerinde =

**Sayfalandırma kuralları** kümesindeki bir veya daha fazla büyük/küçük harfe anahtar-değer çiftleri içeren bir sözlük olarak tanımlanır. Yapılandırma, ikinci sayfasından başlatma isteği oluşturmak için kullanılır. Bağlayıcı 204 (içerik yok) HTTP durum kodu alır ya da herhangi bir "paginationRules" JSONPath ifade null döndürür yineleme durdurur.

**Anahtarları desteklenen** sayfalandırma kuralları:

| Anahtar | Açıklama |
|:--- |:--- |
| AbsoluteUrl | Sonraki istek için URL'yi belirtir. Bu olabilir **mutlak bir URL ya da göreli URL**. |
| QueryParameters. *request_query_parameter* veya QueryParameters ['request_query_parameter'] | "request_query_parameter" kullanıcı-sonraki HTTP isteği URL'si bir sorgu parametresi adlarında başvuran tanımlanır. |
| Üstbilgileri. *request_header* veya üstbilgisi ['request_header'] | "request_header" kullanıcı-sonraki HTTP isteği bir üst bilgi adı başvuran tanımlanır. |

**Desteklenen değerleri** sayfalandırma kuralları:

| Değer | Açıklama |
|:--- |:--- |
| Üstbilgileri. *response_header* veya üstbilgisi ['response_header'] | "response_header" kullanıcı-başvuran bir üst bilgi adı değerini sonraki istek için kullanılacak geçerli HTTP yanıtı içinde tanımlanır. |
| "(Yanıt gövdesi kökünü temsil eden) $" ile başlayan bir JSONPath ifadesi | Yanıt gövdesi yalnızca bir JSON nesnesi içermesi gerekir. JSONPath ifade sonraki istek için kullanılan tek bir İlkel değer döndürmelidir. |

**Örnek:**

Facebook Graph API'si aşağıdaki yapısında, büyük/küçük harf sonraki sayfanın URL'sini temsil edilmiştir yanıt verir ***paging.next***:

```json
{
    "data": [
        {
            "created_time": "2017-12-12T14:12:20+0000",
            "name": "album1",
            "id": "1809938745705498_1809939942372045"
        },
        {
            "created_time": "2017-12-12T14:14:03+0000",
            "name": "album2",
            "id": "1809938745705498_1809941802371859"
        },
        {
            "created_time": "2017-12-12T14:14:11+0000",
            "name": "album3",
            "id": "1809938745705498_1809941879038518"
        }
    ],
    "paging": {
        "cursors": {
            "after": "MTAxNTExOTQ1MjAwNzI5NDE=",
            "before": "NDMyNzQyODI3OTQw"
        },
        "previous": "https://graph.facebook.com/me/albums?limit=25&before=NDMyNzQyODI3OTQw",
        "next": "https://graph.facebook.com/me/albums?limit=25&after=MTAxNTExOTQ1MjAwNzI5NDE="
    }
}
```

Karşılık gelen REST veri kümesi yapılandırma özellikle `paginationRules` aşağıdaki gibidir:

```json
{
    "name": "MyFacebookAlbums",
    "properties": {
            "type": "RestResource",
            "typeProperties": {
                "relativeUrl": "albums",
                "paginationRules": {
                    "AbsoluteUrl": "$.paging.next"
                }
            },
            "linkedServiceName": {
                "referenceName": "MyRestService",
                "type": "LinkedServiceReference"
            }
    }
}
```

## <a name="export-json-response-as-is"></a>JSON yanıtı olarak dışarı aktar-olup

Bu REST bağlayıcı olarak REST API JSON yanıt vermek için kullanabileceğiniz-çeşitli dosya tabanlı depoladığı. Bu tür şemadan kopyalama elde etmek için "yapı" Atla (olarak da bilinir *şema*) bölümünde veri kümesini ve şema eşleme kopyalama etkinliğindeki.

## <a name="schema-mapping"></a>Şema eşleme

Tablo havuz için REST uç noktasından veri kopyalamak için başvurmak [şema eşleme](copy-activity-schema-and-type-mapping.md#schema-mapping).

## <a name="next-steps"></a>Sonraki adımlar

Kopyalama etkinliği kaynak olarak destekler ve şu havuzlar Azure Data Factory'de veri depolarının listesi için bkz. [desteklenen veri depoları ve biçimler](copy-activity-overview.md#supported-data-stores-and-formats).
