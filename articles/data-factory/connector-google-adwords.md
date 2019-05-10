---
title: Azure Data Factory (Önizleme) kullanarak Google AdWords verileri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına Google AdWords bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: jingwang
ms.openlocfilehash: 0f68627e2db3c08049f0273045906057526bd6aa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61094927"
---
# <a name="copy-data-from-google-adwords-using-azure-data-factory-preview"></a>Google AdWords Azure Data Factory (Önizleme) kullanarak verileri kopyalama

Bu makalede, kopyalama etkinliği Azure Data Factory'de Google AdWords verileri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

> [!IMPORTANT]
> Bu bağlayıcı, şu anda Önizleme aşamasındadır. Deneyin ve geri bildirim sağlayın. Çözümünüzde bir önizleme bağlayıcısı bağımlılığı olmasını istiyorsanız lütfen [Azure desteğine](https://azure.microsoft.com/support/) başvurun.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Google AdWords tüm desteklenen havuz veri deposuna veri kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Azure Data Factory bağlantısını etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak herhangi bir sürücü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler belirli Data Factory varlıkları Google AdWords bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Google AdWords bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **GoogleAdWords** | Evet |
| clientCustomerID | Rapor verileri getirmek istediğiniz AdWords hesabını istemci müşteri kimliği.  | Evet |
| developerToken | Geliştirici belirteç AdWords API'ye erişim için kullandığınız yönetici hesabı ile ilişkili.  Bu alanı ADF içinde güvenli bir şekilde depolayın veya Azure anahtar Kasası'nda parolayı depolamak için bir SecureString olarak işaretleyin ve veri kopyalama gerçekleştirirken buradan kopyalama etkinliği çekme - daha fazla bilgi ADF seçebileceğiniz [anahtar Kasası'nda kimlik bilgileri Store](store-credentials-in-key-vault.md). | Evet |
| authenticationType | Kimlik doğrulaması için kullanılan OAuth 2.0 kimlik doğrulama mekanizması. ServiceAuthentication yalnızca şirket içinde barındırılan IR üzerinde kullanılabilir <br/>İzin verilen değerler şunlardır: **ServiceAuthentication**, **UserAuthentication** | Evet |
| refreshToken | İçin UserAuthentication AdWords erişim yetkisi vermek için Google'dan alınan yenileme belirteci. Bu alanı ADF içinde güvenli bir şekilde depolayın veya Azure anahtar Kasası'nda parolayı depolamak için bir SecureString olarak işaretleyin ve veri kopyalama gerçekleştirirken buradan kopyalama etkinliği çekme - daha fazla bilgi ADF seçebileceğiniz [anahtar Kasası'nda kimlik bilgileri Store](store-credentials-in-key-vault.md). | Hayır |
| clientId | Yenileme belirteci almak için kullanılan google uygulama istemci kimliği. Bu alanı ADF içinde güvenli bir şekilde depolayın veya Azure anahtar Kasası'nda parolayı depolamak için bir SecureString olarak işaretleyin ve veri kopyalama gerçekleştirirken buradan kopyalama etkinliği çekme - daha fazla bilgi ADF seçebileceğiniz [anahtar Kasası'nda kimlik bilgileri Store](store-credentials-in-key-vault.md). | Hayır |
| clientSecret | Yenileme belirteci almak için kullanılan google uygulama istemci gizli bilgisi. Bu alanı ADF içinde güvenli bir şekilde depolayın veya Azure anahtar Kasası'nda parolayı depolamak için bir SecureString olarak işaretleyin ve veri kopyalama gerçekleştirirken buradan kopyalama etkinliği çekme - daha fazla bilgi ADF seçebileceğiniz [anahtar Kasası'nda kimlik bilgileri Store](store-credentials-in-key-vault.md). | Hayır |
| email | ServiceAuthentication için kullanılır ve yalnızca şirket içinde barındırılan IR üzerinde kullanılabilir hizmet hesabı e-posta kimliği  | Hayır |
| keyFilePath | Hizmet hesabı e-posta adresi kimliğini doğrulamak için kullanılır ve yalnızca şirket içinde barındırılan IR üzerinde kullanılabilir .p12 anahtar dosyasının tam yolu  | Hayır |
| trustedCertPath | SSL üzerinden bağlanırken sunucu doğrulamak için güvenilen CA sertifikalarını içeren .pem dosyasının tam yolu. Bu özellik yalnızca şirket içinde barındırılan IR üzerinde SSL kullanılarak, ayarlanabilir Varsayılan değer IR ile yüklü cacerts.pem dosyasıdır  | Hayır |
| useSystemTrustStore | Bir CA sertifikası sistem güven deposu veya belirtilen bir PEM dosyası kullanılıp kullanılmayacağını belirtir. Varsayılan değer false'tur.  | Hayır |

**Örnek:**

```json
{
    "name": "GoogleAdWordsLinkedService",
    "properties": {
        "type": "GoogleAdWords",
        "typeProperties": {
            "clientCustomerID" : "<clientCustomerID>",
            "developerToken": {
                "type": "SecureString",
                "value": "<developerToken>"
            },
            "authenticationType" : "ServiceAuthentication",
            "refreshToken": {
                "type": "SecureString",
                "value": "<refreshToken>"
            },
            "clientId": {
                "type": "SecureString",
                "value": "<clientId>"
            },
            "clientSecret": {
                "type": "SecureString",
                "value": "<clientSecret>"
            },
            "email" : "<email>",
            "keyFilePath" : "<keyFilePath>",
            "trustedCertPath" : "<trustedCertPath>",
            "useSystemTrustStore" : true,
        }
    }
}

```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, Google AdWords veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Google AdWords verileri kopyalamak için dataset öğesinin type özelliği ayarlamak **GoogleAdWordsObject**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **GoogleAdWordsObject** | Evet |
| tableName | Tablonun adı. | Hayır (etkinlik kaynağı "sorgu" belirtilmişse) |

**Örnek**

```json
{
    "name": "GoogleAdWordsDataset",
    "properties": {
        "type": "GoogleAdWordsObject",
        "linkedServiceName": {
            "referenceName": "<GoogleAdWords linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}

```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Google AdWords kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="google-adwords-as-source"></a>Kaynak olarak Google AdWords

Google AdWords verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **GoogleAdWordsSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **GoogleAdWordsSource** | Evet |
| query | Verileri okumak için özel bir SQL sorgusu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Yok (veri kümesinde "TableName" değeri belirtilmişse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromGoogleAdWords",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<GoogleAdWords input dataset name>",
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
                "type": "GoogleAdWordsSource",
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
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
