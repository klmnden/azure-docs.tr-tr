---
title: Azure Data Factory (Beta) kullanarak Marketo'dan veri kopyalama | Microsoft Docs
description: "Veri kopyalama etkinliği Azure Data Factory ardışık düzeninde kullanarak Marketo'dan desteklenen havuz veri depolarına kopyalama öğrenin."
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
ms.date: 12/15/2017
ms.author: jingwang
ms.openlocfilehash: dc4e8b8682e686abc77f43427615c7069585f327
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="copy-data-from-marketo-using-azure-data-factory-beta"></a>Azure Data Factory (Beta) kullanarak Marketo'dan veri kopyalama

Bu makalede kopya etkinliği Azure Data Factory'de Marketo'dan verileri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 kopyalama etkinliği](v1/data-factory-data-movement-activities.md).

> [!IMPORTANT]
> Bu şu anda Beta Bağlayıcıdır. Deneyin ve bize geri bildirimde bulunun. Üretim ortamında kullanmayın.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Veriler Marketo'dan tüm desteklenen havuz veri deposuna kopyalayabilirsiniz. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Azure Data Factory bağlantısını etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak sürücüyü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

Aşağıdaki bölümler, belirli Data Factory varlıklarını Marketo bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler Marketo bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Marketo** | Evet |
| endpoint | Marketo sunucusu uç noktası. (i.e. 123-ABC-321.mktorest.com)  | Evet |
| clientId | İstemci kimliği Marketo hizmetinizin.  | Evet |
| clientSecret | Marketo hizmetinizi istemci gizli anahtarı. Bu alan ADF içinde güvenli şekilde depolayın veya Azure anahtar kasası parolayı depolamak için bir SecureString olarak işaretle ve veri kopyalama gerçekleştirirken etkinliklere çekme buradan kopyalama-'dan daha fazla bilgi edinin ADF izin seçebilirsiniz [anahtar kasasına kimlik bilgilerini saklamak](store-credentials-in-key-vault.md). | Evet |
| useEncryptedEndpoints | Veri kaynağı uç noktaları HTTPS kullanılarak şifrelenmiş olup olmadığını belirtir. Varsayılan değer true olur.  | Hayır |
| useHostVerification | SSL üzerinden bağlanırken sunucusunun ana bilgisayar adı ile eşleşmesi için sunucunun sertifikası ana bilgisayar adlarında istenip istenmeyeceğini belirtir. Varsayılan değer true olur.  | Hayır |
| usePeerVerification | SSL üzerinden bağlanırken sunucusunun kimliğini doğrulamak belirtir. Varsayılan değer true olur.  | Hayır |

**Örnek:**

```json
{
    "name": "MarketoLinkedService",
    "properties": {
        "type": "Marketo",
        "typeProperties": {
            "endpoint" : "123-ABC-321.mktorest.com",
            "clientId" : "<clientId>",
            "clientSecret": {
                 "type": "SecureString",
                 "value": "<clientSecret>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde Marketo veri kümesi tarafından desteklenen özellikler listesini sağlar.

Marketo'dan verileri kopyalamak için kümesine tür özelliği ayarlamak **MarketoObject**. Ek bir türe özel özellik bu tür bir veri kümesi yok.

**Örnek**

```json
{
    "name": "MarketoDataset",
    "properties": {
        "type": "MarketoObject",
        "linkedServiceName": {
            "referenceName": "<Marketo linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde Marketo kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="marketosource-as-source"></a>Kaynak olarak MarketoSource

Marketo'dan verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **MarketoSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **MarketoSource** | Evet |
| sorgu | Verileri okumak için özel SQL sorgusu kullanın. Örneğin: `"SELECT * FROM Activitiy_Types"`. | Evet |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromMarketo",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Marketo input dataset name>",
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
                "type": "MarketoSource",
                "query": "SELECT top 1000 * FROM Activitiy_Types"
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
