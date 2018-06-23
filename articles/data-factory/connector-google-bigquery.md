---
title: Azure Data Factory kullanarak Google BigQuery veri kopyalama | Microsoft Docs
description: Desteklenen havuz veri depolarına Google BigQuery veri fabrikası ardışık düzeninde kopyalama etkinliği kullanarak verileri kopyalamak öğrenin.
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
ms.date: 02/12/2018
ms.author: jingwang
ms.openlocfilehash: e88d8046e31a43f444193dff91895b474be70d91
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36332346"
---
# <a name="copy-data-from-google-bigquery-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Google BigQuery veri kopyalama

Bu makalede kopya etkinliği Azure Data Factory'de Google BigQuery verileri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir, Data Factory hizmetinin 1 sürümünü kullanıyorsanız bkz [kopyalama etkinliğini sürüm 1](v1/data-factory-data-movement-activities.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Google BigQuery verileri herhangi bir desteklenen havuz veri deposuna kopyalayabilirsiniz. Kaynakları veya havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

 Veri Fabrikası bağlantısını etkinleştirmek için yerleşik bir sürücü sağlar. Bu nedenle, bu bağlayıcıyı kullanmak için bir sürücüyü el ile yüklemeniz gerekmez.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, belirli Data Factory varlıklarını Google BigQuery bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Google BigQuery bağlantılı hizmetinin aşağıdaki özellikleri desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlamak **GoogleBigQuery**. | Evet |
| proje | Sorgu varsayılan BigQuery projeye proje kimliği.  | Evet |
| additionalProjects | Ortak proje kimliklerinin virgülle ayrılmış bir listesini BigQuery erişimi projeleri.  | Hayır |
| requestGoogleDriveScope | Google sürücüye erişim istenip istenmeyeceğini belirtir. Google sürücü erişimine Google sürücüsünden verilerle BigQuery verileri birleştirmek birleştirilmiş tablolar için destek sağlar. Varsayılan değer **false**.  | Hayır |
| authenticationType | Kimlik doğrulaması için kullanılan OAuth 2.0 kimlik doğrulama mekanizması. ServiceAuthentication yalnızca Self-hosted tümleştirme çalışma üzerinde kullanılabilir. <br/>İzin verilen değerler **UserAuthentication** ve **ServiceAuthentication**. Daha fazla özellikleri ve bu kimlik doğrulama türleri için JSON örnekleri bu tabloda aşağıdaki bölümlerde sırasıyla bakın. | Evet |

### <a name="using-user-authentication"></a>Kullanıcı kimlik doğrulaması kullanma

Kümesine "authenticationType" özelliği **UserAuthentication**ve önceki bölümde açıklanan genel özellikleri birlikte aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| istemci kimliği | Yenileme belirteci üretmek için kullanılan uygulama kimliği. | Hayır |
| clientSecret | Yenileme belirtecini oluşturmak için kullanılan uygulama gizli anahtarı. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). | Hayır |
| refreshToken | Google BigQuery erişim yetkisi vermek için kullanılan alınan yenileme belirteci. Bir alma hakkında bilgi [alma OAuth 2.0 erişim belirteçleri](https://developers.google.com/identity/protocols/OAuth2WebServer#obtainingaccesstokens) ve [Bu topluluk blog](https://jpd.ms/getting-your-bigquery-refresh-token-for-azure-datafactory-f884ff815a59). Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). | Hayır |

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

Kümesine "authenticationType" özelliği **ServiceAuthentication**ve önceki bölümde açıklanan genel özellikleri birlikte aşağıdaki özellikleri belirtin. Bu kimlik doğrulama türü yalnızca Self-hosted tümleştirme çalışma üzerinde kullanılabilir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| e-posta | ServiceAuthentication için kullanılan hizmet hesabı e-posta kimliği. Yalnızca Self-hosted tümleştirme çalışma üzerinde kullanılabilir.  | Hayır |
| keyFilePath | Hizmet hesabı e-posta adresini doğrulamak için kullanılan .p12 anahtar dosyasının tam yolu. | Hayır |
| trustedCertPath | Sunucu SSL üzerinden bağlandığında doğrulamak için kullanılan güvenilir CA sertifikaları içeren .pem dosyasının tam yolu. Bu özellik, yalnızca SSL Self-hosted tümleştirmesi Çalışma Zamanı Modülü kullandığınızda ayarlayabilirsiniz. Varsayılan değer ile tümleştirme çalışma zamanının yüklü cacerts.pem dosyasıdır.  | Hayır |
| useSystemTrustStore | Bir CA sertifikası sistem güven deposundan veya belirtilen .pem dosyasını kullanılıp kullanılmayacağını belirtir. Varsayılan değer **false**.  | Hayır |

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

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, Google BigQuery veri kümesi tarafından desteklenen özellikler listesini sağlar.

Google BigQuery verileri kopyalamak için kümesine tür özelliği ayarlamak **GoogleBigQueryObject**. Ek bir türe özel özellik bu tür bir veri kümesi yok.

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

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde Google BigQuery kaynak türü tarafından desteklenen özellikler listesini sağlar.

### <a name="googlebigquerysource-as-a-source-type"></a>Bir kaynak türü olarak GoogleBigQuerySource

Google BigQuery verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **GoogleBigQuerySource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak **GoogleBigQuerySource**. | Evet |
| sorgu | Verileri okumak için özel SQL sorgusu kullanın. `"SELECT * FROM MyTable"` bunun bir örneğidir. | Evet |

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
Kaynakları ve havuzlarını Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
