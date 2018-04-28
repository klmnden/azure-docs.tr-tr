---
title: Azure Data Factory kullanarak Spark'tan veri kopyalama | Microsoft Docs
description: Veri kopyalama etkinliği Azure Data Factory ardışık düzeninde kullanarak Spark'tan desteklenen havuz veri depolarına kopyalama öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2018
ms.author: jingwang
ms.openlocfilehash: 4089fe636ad25f97fe78f0bd10553b93d768321d
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="copy-data-from-spark-using-azure-data-factory"></a>Azure Data Factory kullanarak Spark'tan veri kopyalama 

Bu makalede kopya etkinliği Azure Data Factory'de Spark'tan veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 kopyalama etkinliği](v1/data-factory-data-movement-activities.md).


## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna Spark'tan veri kopyalayabilirsiniz. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Azure Data Factory bağlantısını etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak sürücüyü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

Aşağıdaki bölümler, belirli Data Factory varlıklarını Spark bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikleri, bağlantılı Spark hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Spark** | Evet |
| konak | Spark sunucusunun IP adresi veya ana bilgisayar adı  | Evet |
| port | İstemci bağlantılarını dinlemek için Spark sunucusunun kullandığı TCP bağlantı noktası. Azure Hdınsights bağlanıyorsanız, bağlantı noktası 443'tür belirtin. | Evet |
| Sunucu türü | Spark sunucu türü. <br/>İzin verilen değerler: **SharkServer**, **SharkServer2**, **SparkThriftServer** | Hayır |
| thriftTransportProtocol | Thrift katmanda kullanılacak Aktarım Protokolü. <br/>İzin verilen değerler: **ikili**, **SASL**, **HTTP** | Hayır |
| authenticationType | Spark sunucuya erişmek için kullanılan kimlik doğrulama yöntemi. <br/>İzin verilen değerler: **anonim**, **kullanıcıadı**, **UsernameAndPassword**, **WindowsAzureHDInsightService** | Evet |
| kullanıcı adı | Spark sunucusuna erişmek için kullandığınız kullanıcı adı.  | Hayır |
| password | Kullanıcıya karşılık gelen parola. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). | Hayır |
| httpPath | Spark sunucuya karşılık gelen kısmi URL'si.  | Hayır |
| enableSsl | Sunucusuna bağlantılarda SSL kullanılarak şifrelenir olup olmadığını belirtir. Varsayılan değer false.  | Hayır |
| trustedCertPath | Sunucu SSL üzerinden bağlanırken doğrulamak için güvenilen CA sertifikaları içeren .pem dosyasının tam yolu. Bu özellik yalnızca SSL üzerinde kendini barındıran IR kullanırken ayarlanabilir Varsayılan değer ile IR yüklü cacerts.pem dosyasıdır  | Hayır |
| useSystemTrustStore | Bir CA sertifikası sistem güven deposundan veya belirtilen PEM dosyası kullanılıp kullanılmayacağını belirtir. Varsayılan değer false.  | Hayır |
| allowHostNameCNMismatch | SSL üzerinden bağlanırken sunucusunun ana bilgisayar adı ile eşleşmesi için CA tarafından verilen SSL sertifika adı istenip istenmeyeceğini belirtir. Varsayılan değer false.  | Hayır |
| allowSelfSignedServerCert | Otomatik olarak imzalanan sertifikalar sunucudan izin verilip verilmeyeceğini belirtir. Varsayılan değer false.  | Hayır |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu genel olarak erişilebilir ise) Self-hosted tümleştirmesi çalışma zamanı veya Azure tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. |Hayır |

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

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, Spark veri kümesi tarafından desteklenen özellikler listesini sağlar.

Spark'tan veri kopyalamak için veri kümesi için tür özelliği ayarlamak **SparkObject**. Ek bir türe özel özellik bu tür bir veri kümesi yok.

**Örnek**

```json
{
    "name": "SparkDataset",
    "properties": {
        "type": "SparkObject",
        "linkedServiceName": {
            "referenceName": "<Spark linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde, Spark kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="sparksource-as-source"></a>Kaynak olarak SparkSource

Spark'tan veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **SparkSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **SparkSource** | Evet |
| sorgu | Verileri okumak için özel SQL sorgusu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Evet |

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
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
