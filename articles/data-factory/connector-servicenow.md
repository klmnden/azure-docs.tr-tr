---
title: Azure Data Factory kullanarak ServiceNow veri kopyalama | Microsoft Docs
description: Desteklenen havuz veri depolarına ServiceNow bir Azure Data Factory ardışık düzeninde kopyalama etkinliği kullanarak verileri kopyalamak öğrenin.
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
ms.date: 05/28/2018
ms.author: jingwang
ms.openlocfilehash: 94c94d949874dcf3b48a7ecf2d8ec63da056aec5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36334284"
---
# <a name="copy-data-from-servicenow-using-azure-data-factory"></a>Servicenow'ı kullanarak Azure Data Factory kopyalama verileri

Bu makalede kopya etkinliği Azure Data Factory'de ServiceNow verileri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 kopyalama etkinliği](v1/data-factory-data-movement-activities.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

ServiceNow verileri herhangi bir desteklenen havuz veri deposuna kopyalayabilirsiniz. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Azure Data Factory bağlantısını etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak sürücüyü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, belirli Data Factory varlıklarını ServiceNow bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler ServiceNow bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **ServiceNow** | Evet |
| endpoint | ServiceNow sunucusu uç noktası (`http://<instance>.service-now.com`).  | Evet |
| authenticationType | Kullanılacak kimlik doğrulama türü. <br/>İzin verilen değerler: **temel**, **OAuth2** | Evet |
| kullanıcı adı | ServiceNow temel ve OAuth2 kimlik doğrulama için sunucusuna bağlanmak için kullanılan kullanıcı adı.  | Evet |
| password | Temel ve OAuth2 kimlik doğrulama için kullanıcı adı için karşılık gelen parola. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). | Evet |
| istemci kimliği | OAuth2 kimlik doğrulaması için istemci kimliği.  | Hayır |
| clientSecret | OAuth2 kimlik doğrulaması için istemci gizli anahtarı. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). | Hayır |
| useEncryptedEndpoints | Veri kaynağı uç noktaları HTTPS kullanılarak şifrelenmiş olup olmadığını belirtir. Varsayılan değer true olur.  | Hayır |
| useHostVerification | SSL üzerinden bağlanırken sunucusunun ana bilgisayar adı ile eşleşmesi için sunucunun sertifikası ana bilgisayar adlarında istenip istenmeyeceğini belirtir. Varsayılan değer true olur.  | Hayır |
| usePeerVerification | SSL üzerinden bağlanırken sunucusunun kimliğini doğrulamak belirtir. Varsayılan değer true olur.  | Hayır |

**Örnek:**

```json
{
    "name": "ServiceNowLinkedService",
    "properties": {
        "type": "ServiceNow",
        "typeProperties": {
            "endpoint" : "http://<instance>.service-now.com",
            "authenticationType" : "Basic",
            "username" : "<username>",
            "password": {
                 "type": "SecureString",
                 "value": "<password>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde ServiceNow veri kümesi tarafından desteklenen özellikler listesini sağlar.

ServiceNow verileri kopyalamak için kümesine tür özelliği ayarlamak **ServiceNowObject**. Ek bir türe özel özellik bu tür bir veri kümesi yok.

**Örnek**

```json
{
    "name": "ServiceNowDataset",
    "properties": {
        "type": "ServiceNowObject",
        "linkedServiceName": {
            "referenceName": "<ServiceNow linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde ServiceNow kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="servicenow-as-source"></a>Kaynak olarak ServiceNow

ServiceNow verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **ServiceNowSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **ServiceNowSource** | Evet |
| sorgu | Verileri okumak için özel SQL sorgusu kullanın. Örneğin: `"SELECT * FROM Actual.alm_asset"`. | Evet |

Şema ve sütun ServiceNow için sorguda belirtirken aşağıdakileri unutmayın:

- **Şema:** şeması olarak belirtmek `Actual` veya `Display` adresinden parametresi olarak bakabilirsiniz ServiceNow sorgusunda `sysparm_display_value` true veya false çağrılırken olarak [ServiceNow restful API'lerini](https://developer.servicenow.com/app.do#!/rest_api_doc?v=jakarta&id=r_AggregateAPI-GET). 
- **Sütun:** altında gerçek değer sütun adı `Actual` şeması `[columne name]_value`sırada altındaki görüntüleme değeri için `Display` şeması `[columne name]_display_value`. Not sütun adı sorguda kullanılan şema eşlenmesi gerekir.

**Örnek Sorgu:** 
 `SELECT col_value FROM Actual.alm_asset` veya `SELECT col_display_value FROM Display.alm_asset`

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromServiceNow",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ServiceNow input dataset name>",
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
                "type": "ServiceNowSource",
                "query": "SELECT * FROM Actual.alm_asset"
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
