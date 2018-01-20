---
title: Azure Data Factory (Beta) kullanarak Xero veri kopyalama | Microsoft Docs
description: "Desteklenen havuz veri depolarına Xero bir Azure Data Factory ardışık düzeninde kopyalama etkinliği kullanarak verileri kopyalamak öğrenin."
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
ms.openlocfilehash: aa81f9d163da8d9236470c0b797f5430163ed39d
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="copy-data-from-xero-using-azure-data-factory-beta"></a>Azure Data Factory (Beta) kullanarak Xero verilerini

Bu makalede kopya etkinliği Azure Data Factory'de Xero verileri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 kopyalama etkinliği](v1/data-factory-data-movement-activities.md).

> [!IMPORTANT]
> Bu şu anda Beta Bağlayıcıdır. Deneyin ve geri bildirim sağlayın. Üretim ortamında kullanmayın.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna Xero veri kopyalayabilirsiniz. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Azure Data Factory bağlantısını etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak sürücüyü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

Aşağıdaki bölümler, belirli Data Factory varlıklarını Xero bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler Xero bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Xero** | Evet |
| konak | Xero sunucusu uç noktası. (that is, api.xero.com)  | Evet |
| consumerKey | Xero uygulama ile ilişkili tüketici anahtarı. Bu alan veri fabrikasında güvenli şekilde depolayın veya Azure anahtar kasası parolayı depolamak için bir SecureString olarak işaretlemek seçin ve veri kopyalama gerçekleştirirken buradan çekme-'dan daha fazla bilgi kopyalama etkinliği izin [anahtar kasasıkimlikbilgilerinisaklamak](store-credentials-in-key-vault.md). | Evet |
| privateKey | Xero özel uygulamanız için oluşturulan .pem dosyasından özel anahtarı. .Pem dosyasını UNIX satır endings(\n) dahil olmak üzere, tüm metni içerir. Bu alan veri fabrikasında güvenli şekilde depolayın veya Azure anahtar kasası parolayı depolamak için bir SecureString olarak işaretlemek seçin ve veri kopyalama gerçekleştirirken buradan çekme-'dan daha fazla bilgi kopyalama etkinliği izin [anahtar kasasıkimlikbilgilerinisaklamak](store-credentials-in-key-vault.md). | Evet |
| useEncryptedEndpoints | Veri kaynağı uç noktaları HTTPS kullanılarak şifrelenmiş olup olmadığını belirtir. Varsayılan değer true olur.  | Hayır |
| useHostVerification | SSL üzerinden bağlanırken sunucusunun ana bilgisayar adı ile eşleşmesi için ana bilgisayar adı sunucunun sertifikasında gerekip gerekmediğini belirtir. Varsayılan değer true olur.  | Hayır |
| usePeerVerification | SSL üzerinden bağlanırken sunucusunun kimliğini doğrulamak belirtir. Varsayılan değer true olur.  | Hayır |

**Örnek:**

```json
{
    "name": "XeroLinkedService",
    "properties": {
        "type": "Xero",
        "typeProperties": {
            "host" : "api.xero.com",
            "consumerKey": {
                 "type": "SecureString",
                 "value": "<consumerKey>"
            },
            "privateKey": {
                 "type": "SecureString",
                 "value": "<privateKey>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde Xero veri kümesi tarafından desteklenen özellikler listesini sağlar.

Xero verileri kopyalamak için kümesine tür özelliği ayarlamak **XeroObject**. Ek bir türe özel özellik bu tür bir veri kümesi yok.

**Örnek**

```json
{
    "name": "XeroDataset",
    "properties": {
        "type": "XeroObject",
        "linkedServiceName": {
            "referenceName": "<Xero linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde Xero kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="xerosource-as-source"></a>Kaynak olarak XeroSource

Xero verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **XeroSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **XeroSource** | Evet |
| sorgu | Verileri okumak için özel SQL sorgusu kullanın. Örneğin: `"SELECT * FROM Contacts"`. | Evet |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromXero",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Xero input dataset name>",
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
                "type": "XeroSource",
                "query": "SELECT * FROM Contacts"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar
Kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
