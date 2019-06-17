---
title: Azure Data Factory kullanarak Spark'tan veri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına Spark'tan bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
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
ms.openlocfilehash: 370da046e5a964d91b668ea80730b8d331065d29
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60322881"
---
# <a name="copy-data-from-spark-using-azure-data-factory"></a>Azure Data Factory kullanarak spark'tan veri kopyalama 

Bu makalede, kopyalama etkinliği Azure Data Factory'de Spark'tan veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna Spark'tan veri kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Azure Data Factory bağlantısını etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak herhangi bir sürücü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler belirli Data Factory varlıkları için Spark Bağlayıcısı tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Bağlı bir Spark hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Spark** | Evet |
| konak | Spark sunucusunun IP adresi veya ana bilgisayar adı  | Evet |
| port | Spark sunucusunun istemci bağlantıları için dinlemek üzere kullandığı TCP bağlantı noktası. Azure Hdınsights bağlarsanız, bağlantı noktası 443 belirtin. | Evet |
| Sunucu türü | Spark sunucusu türü. <br/>İzin verilen değerler şunlardır: **SharkServer**, **SharkServer2**, **SparkThriftServer** | Hayır |
| thriftTransportProtocol | Thrift katmanda kullanılacak taşıma protokol. <br/>İzin verilen değerler şunlardır: **İkili**, **SASL**, **HTTP** | Hayır |
| authenticationType | Spark sunucuya erişmek için kullanılan kimlik doğrulama yöntemi. <br/>İzin verilen değerler şunlardır: **Anonim**, **kullanıcıadı**, **UsernameAndPassword**, **WindowsAzureHDInsightService** | Evet |
| kullanıcı adı | Spark sunucusuna erişmek için kullandığınız kullanıcı adı.  | Hayır |
| password | Kullanıcıya karşılık gelen parola. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Hayır |
| httpPath | Spark sunucuya karşılık gelen kısmi URL.  | Hayır |
| enableSsl | Sunucusuna bağlantılarda SSL kullanarak şifrelenip şifrelenmeyeceğini belirtir. Varsayılan değer false'tur.  | Hayır |
| trustedCertPath | SSL üzerinden bağlanırken sunucu doğrulamak için güvenilen CA sertifikalarını içeren .pem dosyasının tam yolu. Bu özellik yalnızca şirket içinde barındırılan IR üzerinde SSL kullanılarak, ayarlanabilir Varsayılan değer IR ile yüklü cacerts.pem dosyasıdır  | Hayır |
| useSystemTrustStore | Bir CA sertifikası sistem güven deposu veya belirtilen bir PEM dosyası kullanılıp kullanılmayacağını belirtir. Varsayılan değer false'tur.  | Hayır |
| allowHostNameCNMismatch | SSL üzerinden bağlanırken sunucu ana bilgisayar adını eşleştirmek için bir CA tarafından verilen SSL sertifika adı gerekip gerekmediğini belirtir. Varsayılan değer false'tur.  | Hayır |
| allowSelfSignedServerCert | Otomatik olarak imzalanan sertifikalar sunucudan izin verilip verilmeyeceğini belirtir. Varsayılan değer false'tur.  | Hayır |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz genel olarak erişilebilir değilse), şirket içinde barındırılan tümleştirme çalışma zamanı veya Azure Integration Runtime kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

**Örnek:**

```json
{
    "name": "SparkLinkedService",
    "properties": {
        "type": "Spark",
        "typeProperties": {
            "host" : "<cluster>.azurehdinsight.net",
            "port" : "<port>",
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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, Spark veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Spark'tan veri kopyalamak için dataset öğesinin type özelliği ayarlamak **SparkObject**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **SparkObject** | Evet |
| tableName | Tablonun adı. | Hayır (etkinlik kaynağı "sorgu" belirtilmişse) |

**Örnek**

```json
{
    "name": "SparkDataset",
    "properties": {
        "type": "SparkObject",
        "linkedServiceName": {
            "referenceName": "<Spark linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Spark kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="spark-as-source"></a>Kaynak olarak spark

Spark'tan veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **SparkSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **SparkSource** | Evet |
| query | Verileri okumak için özel bir SQL sorgusu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Yok (veri kümesinde "TableName" değeri belirtilmişse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromSpark",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Spark input dataset name>",
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
                "type": "SparkSource",
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
