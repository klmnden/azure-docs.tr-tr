---
title: Azure Data Factory kullanarak Google Bigquery'den verileri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına Google Bigquery'den bir data factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
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
ms.date: 11/05/2018
ms.author: jingwang
ms.openlocfilehash: ca12c7a3fe8a5ade8cf0e4ce00977bdcc9a300a6
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51007663"
---
# <a name="copy-data-from-google-bigquery-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Google Bigquery'den verileri kopyalama

Bu makalede, kopyalama etkinliği Azure Data Factory'de Google Bigquery'den verileri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliğine genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Google Bigquery'den verileri hiçbir desteklenen havuz veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak ve havuz desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Veri Fabrikası bağlantısını etkinleştirmek için yerleşik bir sürücü sağlar. Bu nedenle, bu bağlayıcıyı kullanmak üzere bir sürücüyü el ile yüklemeniz gerekmez.

>[!NOTE]
>Bu, Google BigQuery Bağlayıcısı BigQuery API'ler üzerinde oluşturulmuştur. Dikkat edin, maksimum oran gelen istekleri ve, proje başına temelinde uygun kotalar zorlar BigQuery sınırları bakın [kotalar ve limitler - API istekleri](https://cloud.google.com/bigquery/quotas#api_requests). Hesap için çok fazla eş zamanlı istek tetiklemez emin olun.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Google BigQuery Bağlayıcısı özel Data Factory varlıkları tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Google BigQuery bağlı hizmeti için aşağıdaki özellikleri desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır **GoogleBigQuery**. | Evet |
| Proje | Sorgulamanın yapılacağı varsayılan BigQuery projenin proje kimliği.  | Evet |
| additionalProjects | Genel proje kimliklerinin virgülle ayrılmış bir listesini, erişimi BigQuery yansıtıyor.  | Hayır |
| requestGoogleDriveScope | Google drive'a erişim istenip istenmeyeceğini belirtir. Google Drive erişimine BigQuery veri Google Drive verilerle birleştirmek birleştirilmiş tablolar için destek sağlar. Varsayılan değer **false**.  | Hayır |
| authenticationType | Kimlik doğrulaması için kullanılan OAuth 2.0 kimlik doğrulama mekanizması. Yalnızca şirket içinde barındırılan tümleştirme çalışma zamanını ServiceAuthentication kullanılabilir. <br/>İzin verilen değerler **UserAuthentication** ve **ServiceAuthentication**. Daha fazla özellik ve bu kimlik doğrulama türleri için JSON örnekleri bu tabloda aşağıdaki bölümlere sırasıyla bakın. | Evet |

### <a name="using-user-authentication"></a>Kullanıcı kimlik doğrulaması kullanma

"AuthenticationType" özelliği ayarlanmış **UserAuthentication**, önceki bölümde açıklanan genel özellikleri ile birlikte aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| ClientID | Yenileme belirteci oluşturmak için kullanılan uygulama kimliği. | Hayır |
| ClientSecret | Yenileme belirteci oluşturmak için kullanılan uygulama gizli anahtarı. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Hayır |
| refreshToken | Google BigQuery erişim yetkisi vermek için kullanılan alınan yenileme belirteci. Birinden almayı öğrenme [alma OAuth 2.0 erişim belirteçleri](https://developers.google.com/identity/protocols/OAuth2WebServer#obtainingaccesstokens) ve [Bu topluluk blogu](https://jpd.ms/getting-your-bigquery-refresh-token-for-azure-datafactory-f884ff815a59). Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Hayır |

**Örnek:**

```json
{
    "name": "GoogleBigQueryLinkedService",
    "properties": {
        "type": "GoogleBigQuery",
        "typeProperties": {
            "project" : "<project ID>",
            "additionalProjects" : "<additional project IDs>",
            "requestGoogleDriveScope" : true,
            "authenticationType" : "UserAuthentication",
            "clientId": "<id of the application used to generate the refresh token>",
            "clientSecret": {
                "type": "SecureString",
                "value":"<secret of the application used to generate the refresh token>"
            },
            "refreshToken": {
                 "type": "SecureString",
                 "value": "<refresh token>"
            }
        }
    }
}
```

### <a name="using-service-authentication"></a>Hizmet kimlik doğrulaması kullanma

"AuthenticationType" özelliği ayarlanmış **ServiceAuthentication**, önceki bölümde açıklanan genel özellikleri ile birlikte aşağıdaki özellikleri belirtin. Bu kimlik doğrulama türü, yalnızca şirket içinde barındırılan tümleştirme çalışma zamanı üzerinde kullanılabilir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| e-posta | ServiceAuthentication için kullanılan hizmet hesabı e-posta kimliği. Yalnızca şirket içinde barındırılan tümleştirme çalışma zamanı üzerinde kullanılabilir.  | Hayır |
| keyFilePath | Hizmet hesabı e-posta adresi kimliğini doğrulamak için kullanılan .p12 anahtar dosyasının tam yolu. | Hayır |
| trustedCertPath | SSL üzerinden bağlanırken sunucu doğrulamak için kullanılan güvenilir CA sertifikaları içeren bir .pem dosyasının tam yolu. Yalnızca şirket içinde barındırılan tümleştirme çalışma zamanını SSL kullandığınızda, bu özelliği ayarlayabilirsiniz. Varsayılan değer tümleştirme çalışma zamanının yüklü cacerts.pem dosyasıdır.  | Hayır |
| useSystemTrustStore | Sistem güven deposundan veya bir belirtilen .pem dosyasından bir CA sertifikası kullanılıp kullanılmayacağını belirtir. Varsayılan değer **false**.  | Hayır |

**Örnek:**

```json
{
    "name": "GoogleBigQueryLinkedService",
    "properties": {
        "type": "GoogleBigQuery",
        "typeProperties": {
            "project" : "<project id>",
            "requestGoogleDriveScope" : true,
            "authenticationType" : "ServiceAuthentication",
            "email": "<email>",
            "keyFilePath": "<.p12 key path on the IR machine>"
        },
        "connectVia": {
            "referenceName": "<name of Self-hosted Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
} 
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, Google BigQuery veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Google Bigquery'den verileri kopyalamak için dataset öğesinin type özelliği ayarlamak **GoogleBigQueryObject**. Ek bir türe özel özellik bu tür bir veri kümesi yok.

**Örnek**

```json
{
    "name": "GoogleBigQueryDataset",
    "properties": {
        "type": "GoogleBigQueryObject",
        "linkedServiceName": {
            "referenceName": "<GoogleBigQuery linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Google BigQuery kaynak türüne göre desteklenen özelliklerin bir listesini sağlar.

### <a name="googlebigquerysource-as-a-source-type"></a>Bir kaynak türü olarak GoogleBigQuerySource

Google Bigquery'den verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **GoogleBigQuerySource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır **GoogleBigQuerySource**. | Evet |
| sorgu | Verileri okumak için özel bir SQL sorgusu kullanın. `"SELECT * FROM MyTable"` bunun bir örneğidir. | Evet |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromGoogleBigQuery",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<GoogleBigQuery input dataset name>",
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
                "type": "GoogleBigQuerySource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar
Veri fabrikasında kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
