---
title: Azure Data Factory (Beta) kullanarak QuickBooks veri kopyalama | Microsoft Docs
description: "Veri kopyalama etkinliği Azure Data Factory ardışık düzeninde kullanarak QuickBooks desteklenen havuz veri depolarına kopyalama öğrenin."
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
ms.date: 11/30/2017
ms.author: jingwang
ms.openlocfilehash: 9c3a725d0d0c5091a280c3fb99279757f1e014f1
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="copy-data-from-quickbooks-using-azure-data-factory-beta"></a>Azure Data Factory (Beta) kullanarak QuickBooks verilerini

Bu makalede kopya etkinliği Azure Data Factory'de QuickBooks verileri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 kopyalama etkinliği](v1/data-factory-data-movement-activities.md).

> [!IMPORTANT]
> Bu şu anda Beta Bağlayıcıdır. Deneyin ve bize geri bildirimde bulunun. Üretim ortamında kullanmayın.

## <a name="supported-capabilities"></a>Desteklenen özellikler

QuickBooks verileri herhangi bir desteklenen havuz veri deposuna kopyalayabilirsiniz. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Azure Data Factory bağlantısını etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak sürücüyü el ile yüklemeniz gerekmez.

Şu anda bu bağlayıcı 17 Temmuz 2017 önce oluşturulan uygulamaların bir geliştirici hesabını sahip gerek yani 1, 0a, destekleyebilir.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

Aşağıdaki bölümler, belirli Data Factory varlıklarını QuickBooks bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler QuickBooks bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **QuickBooks** | Evet |
| endpoint | QuickBooks sunucusu uç noktası. (diğer bir deyişle, quickbooks.api.intuit.com)  | Evet |
| Companyıd | Yetkilendirmek için QuickBooks şirket şirket kimliği.  | Evet |
| accessToken | OAuth 1.0 kimlik doğrulaması için erişim belirteci. Bu alan ADF içinde güvenli şekilde depolayın veya Azure anahtar kasası parolayı depolamak için bir SecureString olarak işaretlemek seçin ve veri kopyalama gerçekleştirirken buradan çekme-'dan daha fazla bilgi kopyalama etkinliği izin [anahtar kasasına kimlik bilgilerini saklamak](store-credentials-in-key-vault.md). | Evet |
| accessTokenSecret | Erişim belirteci gizli anahtarı OAuth 1.0 kimlik doğrulaması için. Bu alan ADF içinde güvenli şekilde depolayın veya Azure anahtar kasası parolayı depolamak için bir SecureString olarak işaretlemek seçin ve veri kopyalama gerçekleştirirken buradan çekme-'dan daha fazla bilgi kopyalama etkinliği izin [anahtar kasasına kimlik bilgilerini saklamak](store-credentials-in-key-vault.md). | Evet |
| useEncryptedEndpoints | Veri kaynağı uç noktaları HTTPS kullanılarak şifrelenmiş olup olmadığını belirtir. Varsayılan değer true olur.  | Hayır |

**Örnek:**

```json
{
    "name": "QuickBooksLinkedService",
    "properties": {
        "type": "QuickBooks",
        "typeProperties": {
            "endpoint" : "quickbooks.api.intuit.com",
            "companyId" : "<companyId>",
            "accessToken": {
                 "type": "SecureString",
                 "value": "<accessToken>"
            },
            "accessTokenSecret": {
                 "type": "SecureString",
                 "value": "<accessTokenSecret>"
            },
            "useEncryptedEndpoints" : true
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde QuickBooks veri kümesi tarafından desteklenen özellikler listesini sağlar.

QuickBooks verileri kopyalamak için kümesine tür özelliği ayarlamak **QuickBooksObject**. Ek bir türe özel özellik bu tür bir veri kümesi yok.

**Örnek**

```json
{
    "name": "QuickBooksDataset",
    "properties": {
        "type": "QuickBooksObject",
        "linkedServiceName": {
            "referenceName": "<QuickBooks linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde QuickBooks kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="quickbookssource-as-source"></a>Kaynak olarak QuickBooksSource

QuickBooks verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **QuickBooksSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **QuickBooksSource** | Evet |
| sorgu | Verileri okumak için özel SQL sorgusu kullanın. Örneğin: `"SELECT * FROM "Bill" WHERE Id = '123'"`. | Evet |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromQuickBooks",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<QuickBooks input dataset name>",
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
                "type": "QuickBooksSource",
                "query": "SELECT * FROM \"Bill\" WHERE Id = '123' "
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
