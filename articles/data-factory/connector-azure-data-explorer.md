---
title: Ya da Azure veri Gezgini'nde Azure Data Factory kullanarak veri kopyalama
description: Bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak Azure Veri Gezgini gelen veya veri kopyalama hakkında bilgi edinin.
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
ms.date: 04/16/2019
ms.author: orspodek
ms.openlocfilehash: f501257903f3b7c621512f06d1c8c7109e22db1e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60394515"
---
# <a name="copy-data-to-or-from-azure-data-explorer-using-azure-data-factory"></a>Ya da Azure veri Gezgini'nde Azure Data Factory kullanarak veri kopyalama

Bu makalede, kopyalama etkinliği Azure Data Factory'de gelen veya giden veri kopyalamak için nasıl kullanılacağını özetlenmektedir [Azure Veri Gezgini](../data-explorer/data-explorer-overview.md). Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Azure veri Gezgini'ne herhangi bir desteklenen kaynak veri deposundan veri kopyalayabilirsiniz. Ayrıca, Azure veri Gezgini'nden desteklenen havuz veri deposuna veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak ve havuz desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md) tablo.

>[!NOTE]
>/ Azure Veri Gezgini üzerindeki şirket içinde barındırılan tümleştirme çalışma zamanını kullanarak şirket içi veri deposuna/deposundan veri kopyalamayı 3.14 sürümünden itibaren desteklenir.

Azure Veri Gezgini Bağlayıcısı'nı aşağıdakileri sağlar:

* Azure Active Directory (Azure AD) ile uygulama belirteci kimlik doğrulamasını kullanarak verileri kopyalama bir **hizmet sorumlusu**.
* Bir kaynak olarak KQL (Kusto) sorgusu kullanarak veri alın.
* Bir havuz olarak verileri hedef tabloya ekler.

## <a name="getting-started"></a>Başlarken

>[!TIP]
>Azure Veri Gezgini Bağlayıcısı'nı kullanarak bir kılavuz için bkz. [veri gönderip buralardan Azure Veri Gezgini'ni kullanarak Azure Data Factory kopyalama](../data-explorer/data-factory-load-data.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli Azure Veri Gezgini bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Azure Veri Gezgini bağlayıcı, hizmet sorumlusu kimlik doğrulaması kullanır. Bir hizmet sorumlusu almak ve izinleri vermek için aşağıdaki adımları izleyin:

1. Azure Active Directory (Azure AD) uygulama varlık kaydınızı [uygulamanızı Azure AD kiracısı ile kaydetmeniz](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant). Bağlı hizmetini tanımlamak için kullandığınız şu değerleri not edin:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. Hizmet sorumlusu uygun Azure veri Gezgini'nde izni. Başvurmak [veritabanı izinlerini yönetmek, Azure Veri Gezgini](../data-explorer/manage-database-permissions.md) ile yönetme izinleri gözden geçirme yanı sıra rolleri ve izinleri hakkında ayrıntılı bilgi. Genel olarak, için ihtiyacınız

    - **Kaynak olarak**, en az izni **veritabanının görüntüleyiciyi** veritabanınıza rol.
    - **Havuz olarak**, en az izni **veritabanı çıkışlara** veritabanınıza rol.

>[!NOTE]
>Yazmak için ADF UI'ı kullanırken, hizmet sorumlusu için daha yüksek ayrıcalıklı izni bağlı hizmeti veritabanlarında listeleme veya veri kümesi üzerinde tabloların listelendiği işlemleri gerektirebilir. Alternatif olarak, el ile veritabanı adı ve tablo adı giriş seçebilirsiniz. Hizmet sorumlusu veri okuma/yazma için uygun izni verilen sürece, Etkinlik yürütme works kopyalayın.

Azure Veri Gezgini bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** özelliği ayarlanmalıdır **AzureDataExplorer** | Evet |
| endpoint | Uç nokta URL'si biçiminde Azure Veri Gezgini kümenin `https://<clusterName>.<regionName>.kusto.windows.net`. | Evet |
| database | Veritabanının adı. | Evet |
| tenant | Kiracı bilgileri (etki alanı adı veya Kiracı kimliği), uygulamanızın bulunduğu altında belirtin. Bu, normal olarak tanıdığınız, "**yetkilisi kimliği**" içinde [Kusto bağlantı dizesi](https://docs.microsoft.com/azure/kusto/api/connection-strings/kusto#application-authentication-properties). Bu, Azure portalının sağ üst köşedeki fareyle gelerek alın. | Evet |
| servicePrincipalId | Uygulamanın istemci kimliği belirtin. Bu, normal olarak tanıdığınız, "**AAD uygulama istemci Kimliğini**" içinde [Kusto bağlantı dizesi](https://docs.microsoft.com/azure/kusto/api/connection-strings/kusto#application-authentication-properties). | Evet |
| serviceprincipalkey değerleri | Uygulama anahtarını belirtin. Bu, normal olarak tanıdığınız, "**AAD uygulama anahtarı**" içinde [Kusto bağlantı dizesi](https://docs.microsoft.com/azure/kusto/api/connection-strings/kusto#application-authentication-properties). Bu alan olarak işaretlemek bir **SecureString** Data Factory'de güvenle depolamak için veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |

**Bağlı hizmeti özellikleri örneği:**

```json
{
    "name": "AzureDataExplorerLinkedService",
    "properties": {
        "type": "AzureDataExplorer",
        "typeProperties": {
            "endpoint": "https://<clusterName>.<regionName>.kusto.windows.net ",
            "database": "<database name>",
            "tenant": "<tenant name/id e.g. microsoft.onmicrosoft.com>",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, Azure Veri Gezgini veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Azure veri Gezgini'ne veri kopyalamak için dataset öğesinin type özelliği ayarlamak **AzureDataExplorerTable**.

Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** özelliği ayarlanmalıdır **AzureDataExplorerTable** | Evet |
| table | Bağlı hizmet başvurduğu tablonun adı. | Havuz için Evet; Kaynak için Hayır |

**Veri kümesi özellikleri örneği**

```json
{
   "name": "AzureDataExplorerDataset",
    "properties": {
        "type": "AzureDataExplorerTable",
        "linkedServiceName": {
            "referenceName": "<Azure Data Explorer linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "table": "<table name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Azure Veri Gezgini kaynak ve havuz desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-data-explorer-as-source"></a>Kaynak olarak Azure Veri Gezgini

Azure veri Gezgini'nde verileri kopyalamak için ayarlanmış **türü** için kopyalama etkinliği kaynağı özelliğinde **AzureDataExplorerSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kopyalama etkinliği kaynağı özelliği ayarlanmalıdır: **AzureDataExplorerSource** | Evet |
| query | Verilen istek salt okunur bir [KQL biçimi](/azure/kusto/query/). Özel KQL sorgu referans olarak kullanın. | Evet |
| queryTimeout | Sorgu isteği önceki bekleme süresi zaman aşımına uğradı. Varsayılan değer: 10 dakikalık (00: 10:00); izin verilen en yüksek değer olan 1 saat (01: 00:00). | Hayır |

>[!NOTE]
>Varsayılan olarak Azure Veri Gezgini kaynak 500.000 kayıtları ya da 64 MB boyut sınırı vardır. Kesmeden tüm kayıtları almak için belirtebileceğiniz `set notruncation;` sorgunuzun başında. Başvurmak [sorgu sınırları](https://docs.microsoft.com/azure/kusto/concepts/querylimits) hakkında daha fazla bilgi.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromAzureDataExplorer",
        "type": "Copy",
        "typeProperties": {
            "source": {
                "type": "AzureDataExplorerSource",
                "query": "TestTable1 | take 10",
                "queryTimeout": "00:10:00"
            },
            "sink": {
                "type": "<sink type>"
            }
        },
        "inputs": [
            {
                "referenceName": "<Azure Data Explorer input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ]
    }
]
```

### <a name="azure-data-explorer-as-sink"></a>Havuz olarak Azure Veri Gezgini

Verileri Azure veri Gezgini'ne kopyalamak için kopyalama etkinliği Havuz türü özelliğini ayarlayın. **AzureDataExplorerSink**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kopyalama etkinliği havuz özelliği ayarlanmalıdır: **AzureDataExplorerSink** | Evet |
| ingestionMappingName | Önceden oluşturulmuş adını **[eşleme](/azure/kusto/management/mappings#csv-mapping)** Kusto tablosunda. Öğesine uygulanan Azure Veri Gezgini - kaynağından sütunlara eşlemek için **[tüm desteklenen depoları/biçimleri kaynak](copy-activity-overview.md#supported-data-stores-and-formats)** vb. dahil olmak üzere CSV/JSON/Avro biçimlendirir, kopyalama etkinliği kullanabilirsiniz [sütun eşleme](copy-activity-schema-and-type-mapping.md) (örtük olarak ada göre veya açıkça yapılandırılmış gibi) ve/veya Azure Veri Gezgini eşlemeleri. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToAzureDataExplorer",
        "type": "Copy",
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureDataExplorerSink",
                "ingestionMappingName": "<optional Azure Data Explorer mapping name>"
            }
        },
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Data Explorer output dataset name>",
                "type": "DatasetReference"
            }
        ]
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar

* Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).

* Daha fazla bilgi edinin [veri kopyalama Azure Data Factory tarafından Azure veri Gezgini'ne](/azure/data-explorer/data-factory-load-data).
