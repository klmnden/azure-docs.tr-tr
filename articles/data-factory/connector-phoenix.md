---
title: Phoenix Azure Data Factory kullanarak verileri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına Phoenix bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
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
ms.date: 09/19/2018
ms.author: jingwang
ms.openlocfilehash: 78e432bf526ad270ae8543ad1be40727ed560d4b
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46367908"
---
# <a name="copy-data-from-phoenix-using-azure-data-factory"></a>Phoenix Azure Data Factory kullanarak verileri kopyalama 

Bu makalede, kopyalama etkinliği Azure Data Factory'de Phoenix verileri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Phoenix tüm desteklenen havuz veri deposuna veri kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Azure Data Factory bağlantısını etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak herhangi bir sürücü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli Phoenix bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Phoenix bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Phoenix** | Evet |
| konak | Phoenix sunucusunun IP adresi veya ana bilgisayar adı. (diğer bir deyişle, 192.168.222.160)  | Evet |
| port | Phoenix sunucusunun istemci bağlantıları için dinlemek üzere kullandığı TCP bağlantı noktası. 8765 varsayılan değerdir. Azure Hdınsights bağlarsanız, bağlantı noktası 443 belirtin. | Hayır |
| httpPath | Phoenix sunucuya karşılık gelen kısmi URL. (diğer bir deyişle, /gateway/sandbox/phoenix/version). Belirtin `/hbasephoenix0` Hdınsights küme kullanıyorsanız.  | Hayır |
| authenticationType | Phoenix sunucuya bağlanmak için kullanılan kimlik doğrulama mekanizması. <br/>İzin verilen değerler: **anonim**, **UsernameAndPassword**, **WindowsAzureHDInsightService** | Evet |
| kullanıcı adı | Phoenix sunucuya bağlanmak için kullanılan kullanıcı adı.  | Hayır |
| password | Kullanıcı adına karşılık gelen parola. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Hayır |
| enableSsl | Sunucusuna bağlantılarda SSL kullanarak şifrelenip şifrelenmeyeceğini belirtir. Varsayılan değer false'tur.  | Hayır |
| trustedCertPath | SSL üzerinden bağlanırken sunucu doğrulamak için güvenilen CA sertifikalarını içeren .pem dosyasının tam yolu. Bu özellik yalnızca şirket içinde barındırılan IR üzerinde SSL kullanılarak, ayarlanabilir Varsayılan değer IR ile yüklü cacerts.pem dosyasıdır  | Hayır |
| useSystemTrustStore | Bir CA sertifikası sistem güven deposu veya belirtilen bir PEM dosyası kullanılıp kullanılmayacağını belirtir. Varsayılan değer false'tur.  | Hayır |
| allowHostNameCNMismatch | SSL üzerinden bağlanırken sunucu ana bilgisayar adını eşleştirmek için bir CA tarafından verilen SSL sertifika adı gerekip gerekmediğini belirtir. Varsayılan değer false'tur.  | Hayır |
| allowSelfSignedServerCert | Otomatik olarak imzalanan sertifikalar sunucudan izin verilip verilmeyeceğini belirtir. Varsayılan değer false'tur.  | Hayır |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz genel olarak erişilebilir değilse), şirket içinde barındırılan tümleştirme çalışma zamanı veya Azure Integration Runtime kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

>[!NOTE]
>Yapışkan oturumu örneğin HDInsight kümenizi desteklemiyorsa, açıkça http yolu ayarını sonunda düğüm dizinini ekleyin, örneğin belirtin `/hbasephoenix0` yerine `/hbasephoenix`.

**Örnek:**

```json
{
    "name": "PhoenixLinkedService",
    "properties": {
        "type": "Phoenix",
        "typeProperties": {
            "host" : "<cluster>.azurehdinsight.net",
            "port" : "443",
            "httpPath" : "/hbasephoenix0",
            "authenticationType" : "WindowsAzureHDInsightService",
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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, Phoenix veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Phoenix verileri kopyalamak için dataset öğesinin type özelliği ayarlamak **PhoenixObject**. Ek bir türe özel özellik bu tür bir veri kümesi yok.

**Örnek**

```json
{
    "name": "PhoenixDataset",
    "properties": {
        "type": "PhoenixObject",
        "linkedServiceName": {
            "referenceName": "<Phoenix linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Phoenix kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="phoenixsource-as-source"></a>Kaynak olarak PhoenixSource

Phoenix verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **PhoenixSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **PhoenixSource** | Evet |
| sorgu | Verileri okumak için özel bir SQL sorgusu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Evet |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromPhoenix",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Phoenix input dataset name>",
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
                "type": "PhoenixSource",
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
