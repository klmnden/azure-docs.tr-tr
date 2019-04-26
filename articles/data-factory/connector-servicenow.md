---
title: ServiceNow'ı Azure Data Factory kullanarak verileri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına ServiceNow bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
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
ms.openlocfilehash: 234b78a97c2663121d0d585154695887a58b9522
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60203423"
---
# <a name="copy-data-from-servicenow-using-azure-data-factory"></a>Servicenow'ı Azure Data Factory kullanarak verileri kopyalama

Bu makalede, kopyalama etkinliği Azure Data Factory'de ServiceNow verileri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Servicenow'ı tüm desteklenen havuz veri deposuna veri kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Azure Data Factory bağlantısını etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak herhangi bir sürücü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli ServiceNow bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Servicenow'ı bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **ServiceNow** | Evet |
| endpoint | ServiceNow sunucu uç noktası (`http://<instance>.service-now.com`).  | Evet |
| authenticationType | Kullanılacak kimlik doğrulaması türü. <br/>İzin verilen değerler şunlardır: **Temel**, **OAuth2** | Evet |
| kullanıcı adı | Temel ve OAuth2 kimlik doğrulaması için ServiceNow sunucusuna bağlanmak için kullanılan kullanıcı adı.  | Evet |
| password | Temel ve OAuth2 kimlik doğrulaması için kullanıcı adına karşılık gelen parola. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| ClientID | OAuth2 kimlik doğrulaması için istemci kimliği.  | Hayır |
| ClientSecret | OAuth2 kimlik doğrulaması için istemci gizli anahtarı. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Hayır |
| useEncryptedEndpoints | Veri kaynağı uç noktaları HTTPS kullanılarak şifrelenmiş olup olmadığını belirtir. Varsayılan değer true olur.  | Hayır |
| useHostVerification | Ana bilgisayar adı sunucunun sertifikasında SSL üzerinden bağlanırken sunucu ana bilgisayar adıyla eşleşmesi gerekip gerekmediğini belirtir. Varsayılan değer true olur.  | Hayır |
| usePeerVerification | SSL üzerinden bağlanırken sunucu kimliğinin doğrulanıp doğrulanmayacağını belirtir. Varsayılan değer true olur.  | Hayır |

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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, ServiceNow veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Servicenow'ı verileri kopyalamak için dataset öğesinin type özelliği ayarlamak **ServiceNowObject**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **ServiceNowObject** | Evet |
| tableName | Tablonun adı. | Hayır (etkinlik kaynağı "sorgu" belirtilmişse) |

**Örnek**

```json
{
    "name": "ServiceNowDataset",
    "properties": {
        "type": "ServiceNowObject",
        "linkedServiceName": {
            "referenceName": "<ServiceNow linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, ServiceNow kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="servicenow-as-source"></a>Kaynak olarak ServiceNow

Servicenow'ı verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **ServiceNowSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **ServiceNowSource** | Evet |
| sorgu | Verileri okumak için özel bir SQL sorgusu kullanın. Örneğin: `"SELECT * FROM Actual.alm_asset"`. | Yok (veri kümesinde "TableName" değeri belirtilmişse) |

Şema ve sütun için ServiceNow sorguda belirtirken, aşağıdakilere dikkat edin ve **başvurmak [performans ipuçları](#performance-tips) kopyalama performans olduğu çıkarımında üzerinde**.

- **Şema:** şeması olarak belirtmek `Actual` veya `Display` adresinden parametresi olarak bakabilirsiniz ServiceNow sorgusunda `sysparm_display_value` true veya false çağırırken olarak [ServiceNow restful API'leri](https://developer.servicenow.com/app.do#!/rest_api_doc?v=jakarta&id=r_AggregateAPI-GET). 
- **Sütun:** altında gerçek değer için sütun adı `Actual` şeması `[column name]_value`sırada altında görüntüleme değeri için `Display` şeması `[column name]_display_value`. Not sütun adı, sorguda kullanılan şema eşlemesine gerekir.

**Örnek Sorgu:**
`SELECT col_value FROM Actual.alm_asset` VEYA 
`SELECT col_display_value FROM Display.alm_asset`

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
## <a name="performance-tips"></a>Performans ipuçları

### <a name="schema-to-use"></a>Kullanılacak şema

Servicenow'ı olan 2 farklı şemalar, biri **"Gerçek"** gerçek veri döndürür, diğeri ise **"Görüntüler"** veri görüntüleme değerleri döndürür. 

Sorgunuzdaki bir filtreniz varsa, daha iyi performans kopyalama "Gerçek" şema kullanın. "Gerçek" şemasına göre sorgulama yapıldığında, servicenow'ı yerel olarak destekleyen filtre "Display" şema sorgulanırken ADF verilerin tümünü alamadı ve dahili olarak Filtre Uygula filtrelenmiş sonuç kümesi yalnızca döndürülecek veri getirilirken.

### <a name="index"></a>Dizin oluşturma

ServiceNow tablo dizin sorgu performansını artırmak, başvurmak yardımcı [tablo dizin oluşturma](https://docs.servicenow.com/bundle/geneva-servicenow-platform/page/administer/table_administration/task/t_CreateCustomIndex.html).

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
