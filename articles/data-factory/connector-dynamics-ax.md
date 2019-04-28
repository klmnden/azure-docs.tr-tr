---
title: Azure Data Factory (Önizleme) kullanarak Dynamics AX'ten veri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına Dynamics AX'ten bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: jingwang
ms.openlocfilehash: 05bd4fdd220b47b11dfed9857dbc8dbe25b236df
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61347788"
---
# <a name="copy-data-from-dynamics-ax-by-using-azure-data-factory-preview"></a>Azure Data Factory (Önizleme) kullanarak Dynamics AX'ten veri kopyalama

Bu makalede, kopyalama etkinliği Azure Data Factory'de Dynamics AX kaynaktan veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Makaleyi yapılar [Azure veri fabrikasında kopyalama etkinliği](copy-activity-overview.md), kopyalama etkinliği genel bir bakış sunar.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Dynamics AX'ten tüm desteklenen havuz veri deposuna veri kopyalayabilirsiniz. Kopyalama etkinliği kaynak ve havuz olarak desteklediğini veri listesini depolar için bkz: [desteklenen veri depoları ve biçimler](copy-activity-overview.md#supported-data-stores-and-formats).

Özellikle, bu Dynamics AX bağlayıcı Dynamics AX kullanarak veri kopyalamayı destekler **OData Protokolü** ile **hizmet sorumlusu kimlik doğrulaması**.

>[!TIP]
>Verileri kopyalamak için bu bağlayıcıyı kullanabilirsiniz **Dynamics 365 Finans ve operasyon**. Dynamics 365 için 's başvuran [OData desteği](https://docs.microsoft.com/dynamics365/unified-operations/dev-itpro/data-entities/odata) ve [kimlik doğrulama yöntemi](https://docs.microsoft.com/dynamics365/unified-operations/dev-itpro/data-entities/services-home-page#authentication).

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Dynamics AX bağlayıcıya özgü Data Factory varlıkları tanımlamak için kullanabileceğiniz özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="prerequisites"></a>Önkoşullar

Hizmet sorumlusu kimlik doğrulaması kullanmak için aşağıdaki adımları izleyin:

1. Azure Active Directory (Azure AD) uygulama varlık kaydınızı [uygulamanızı Azure AD kiracısı ile kaydetmeniz](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant). Bağlı hizmetini tanımlamak için kullandığınız şu değerleri not edin:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. Dynamics AX gidin ve bu hizmet, Dynamics AX erişmek için uygun asıl izni verin.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Dynamics AX bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** özelliği ayarlanmalıdır **DynamicsAX**. |Evet |
| url | Dynamics AX (veya Dynamics 365 Finans ve operasyon) örneği OData uç noktası. |Evet |
| servicePrincipalId | Uygulamanın istemci kimliği belirtin. | Evet |
| serviceprincipalkey değerleri | Uygulama anahtarını belirtin. Bu alan olarak işaretlemek bir **SecureString** Data Factory'de güvenle depolamak için veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| kiracı | Kiracı bilgileri (etki alanı adı veya Kiracı kimliği), uygulamanızın bulunduğu altında belirtin. Bu, Azure portalının sağ üst köşedeki fare gelerek alın. | Evet |
| aadResourceId | İçin yetkilendirmesi AAD kaynağı belirtin. Örneğin, Dynamics URL'nizi ise `https://sampledynamics.sandbox.operations.dynamics.com/data/`, karşılık gelen AAD genellikle kaynaktır `https://sampledynamics.sandbox.operations.dynamics.com`. | Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel bir ağda yer alıyorsa) Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı seçebilirsiniz. Belirtilmezse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Hayır |

**Örnek**

```json
{
    "name": "DynamicsAXLinkedService",
    "properties": {
        "type": "DynamicsAX",
        "typeProperties": {
            "url": "<Dynamics AX instance OData endpoint>",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "aadResourceId": "<AAD resource, e.g. https://sampledynamics.sandbox.operations.dynamics.com>"
        }
    },
    "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
    }
}

```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bu bölümde, Dynamics AX veri kümesini destekleyen özelliklerin bir listesini sağlar.

Bölümleri ve veri kümeleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [veri kümeleri ve bağlı hizmetler](concepts-datasets-linked-services.md). 

Dynamics AX'ten veri kopyalamak için ayarlanmış **türü** veri kümesine özelliği **DynamicsAXResource**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kümesinin özelliği ayarlanmalıdır **DynamicsAXResource**. | Evet |
| path | Dynamics AX OData varlık yolu. | Evet |

**Örnek**

```json
{
    "name": "DynamicsAXResourceDataset",
    "properties": {
        "type": "DynamicsAXResource",
        "typeProperties": {
            "path": "<entity path e.g. dd04tentitySet>"
        },
        "linkedServiceName": {
            "referenceName": "<Dynamics AX linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bu bölümde, Dynamics AX kaynağının desteklediği özelliklerin bir listesini sağlar.

Bölümleri ve etkinlikleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md). 

### <a name="dynamics-ax-as-source"></a>Dynamics AX kaynağı olarak

Dynamics AX'ten veri kopyalamak için ayarlanmış **kaynak** türü için kopyalama etkinliğindeki **DynamicsAXSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kopyalama etkinliği kaynak özelliği ayarlanmalıdır **DynamicsAXSource**. | Evet |
| sorgu | Verileri filtreleme için OData sorgu seçenekleri. Örnek: `"?$select=Name,Description&$top=5"`.<br/><br/>**Not**: Bağlayıcı verileri birleşik URL'den kopyalar: `[URL specified in linked service]/[path specified in dataset][query specified in copy activity source]`. Daha fazla bilgi için [OData URL'si bileşenleri](https://www.odata.org/documentation/odata-version-3-0/url-conventions/). | Hayır |

**Örnek**

```json
"activities":[
    {
        "name": "CopyFromDynamicsAX",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Dynamics AX input dataset name>",
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
                "type": "DynamicsAXSource",
                "query": "$top=10"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar

Kopyalama etkinliği kaynak olarak destekler ve şu havuzlar Azure Data Factory'de veri depolarının listesi için bkz. [desteklenen veri depoları ve biçimler](copy-activity-overview.md##supported-data-stores-and-formats).
