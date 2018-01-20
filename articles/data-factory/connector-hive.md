---
title: "Azure Data Factory kullanarak kovanından veri kopyalama | Microsoft Docs"
description: "Veri kopyalama etkinliği Azure Data Factory ardışık düzeninde kullanarak kovanından desteklenen havuz veri depolarına kopyalama öğrenin."
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
ms.openlocfilehash: 8842adcc00a1230f252411d64c22d497faeec5b2
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="copy-data-from-hive-using-azure-data-factory"></a>Azure Data Factory kullanarak kovanından veri kopyalama 

Bu makalede kopya etkinliği Azure Data Factory'de kovanından verileri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 kopyalama etkinliği](v1/data-factory-data-movement-activities.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna kovanından veri kopyalayabilirsiniz. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Azure Data Factory bağlantısını etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak sürücüyü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli Hive bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler Hive bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **yığını** | Evet |
| konak | IP adresi veya ana bilgisayar adı (yalnızca serviceDiscoveryMode etkin olduğunda) birden çok ana bilgisayar için ';' ile ayrılmış Hive sunucu.  | Evet |
| port | Hive sunucusunun istemci bağlantılarını dinlemek için kullandığı TCP bağlantı noktası.  | Hayır |
| serverType | Hive sunucu türü. <br/>İzin verilen değerler: **HiveServer1**, **HiveServer2**, **HiveThriftServer** | Hayır |
| thriftTransportProtocol | Thrift katmanda kullanılacak Aktarım Protokolü. <br/>İzin verilen değerler: **ikili**, **SASL**, ** HTTP ** | Hayır |
| authenticationType | Hive sunucuya erişmek için kullanılan kimlik doğrulama yöntemi. <br/>İzin verilen değerler: **anonim**, **kullanıcıadı**, **UsernameAndPassword**, **WindowsAzureHDInsightService** | Evet |
| serviceDiscoveryMode | ZooKeeper hizmet yanlış kullanmayan belirtmek için true.  | Hayır |
| zooKeeperNameSpace | Hangi altında Hive Server 2 düğümler eklenir, ZooKeeper ad.  | Hayır |
| useNativeQuery | Sürücünün yerel HiveQL sorgularını kullanır veya bunları HiveQL eşdeğer bir formda dönüştürür olup olmadığını belirtir.  | Hayır |
| kullanıcı adı | Hive sunucusuna erişmek için kullandığınız kullanıcı adı.  | Hayır |
| password | Bu alan ADF, içinde güvenli bir şekilde depolamak veya Azure anahtar kasası Parolada depolamak ve veri kopyalama - gerçekleştirirken buradan kopyalama etkinliği çekme izin vermek için bir SecureString olarak işaretlemek seçebilirsiniz kullanıcı adı alanında sağlanan kullanıcı adı için karşılık gelen parola ö Daha fazla kaydırmayı [anahtar kasasına kimlik bilgilerini saklamak](store-credentials-in-key-vault.md). | Hayır |
| httpPath | Hive sunucuya karşılık gelen kısmi URL'si.  | Hayır |
| enableSsl | Sunucusuna bağlantılarda SSL kullanılarak şifrelenir olup olmadığını belirtir. Varsayılan değer false.  | Hayır |
| trustedCertPath | Sunucu SSL üzerinden bağlanırken doğrulamak için güvenilen CA sertifikaları içeren .pem dosyasının tam yolu. Bu özellik yalnızca SSL üzerinde kendini barındıran IR kullanırken ayarlanabilir Varsayılan değer ile IR yüklü cacerts.pem dosyasıdır  | Hayır |
| useSystemTrustStore | Bir CA sertifikası sistem güven deposundan veya belirtilen PEM dosyası kullanılıp kullanılmayacağını belirtir. Varsayılan değer false.  | Hayır |
| allowHostNameCNMismatch | SSL üzerinden bağlanırken sunucusunun ana bilgisayar adı ile eşleşmesi için CA tarafından verilen SSL sertifika adı istenip istenmeyeceğini belirtir. Varsayılan değer false.  | Hayır |
| allowSelfSignedServerCert | Otomatik olarak imzalanan sertifikalar sunucudan izin verilip verilmeyeceğini belirtir. Varsayılan değer false.  | Hayır |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu genel olarak erişilebilir ise) Self-hosted tümleştirmesi çalışma zamanı veya Azure tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. |Hayır |

**Örnek:**

```json
{
    "name": "HiveLinkedService",
    "properties": {
        "type": "Hive",
        "typeProperties": {
            "host" : "<cluster>.azurehdinsight.net",
            "port" : "<port>",
            "authenticationType" : "WindowsAzureHDInsightService",
            "username" : "<username>",
            "password": {
                 "type": "SecureString",
                 "value": "<password>"
            },
            "httpPath" : "gateway/sandbox/spark"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, Hive veri kümesi tarafından desteklenen özellikler listesini sağlar.

Kovanından verileri kopyalamak için kümesine tür özelliği ayarlamak **HiveObject**. Ek bir türe özel özellik bu tür bir veri kümesi yok.

**Örnek**

```json
{
    "name": "HiveDataset",
    "properties": {
        "type": "HiveObject",
        "linkedServiceName": {
            "referenceName": "<Hive linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde, Hive kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="hivesource-as-source"></a>Kaynak olarak HiveSource

Kovanından verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **HiveSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **HiveSource** | Evet |
| sorgu | Verileri okumak için özel SQL sorgusu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Evet |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromHive",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Hive input dataset name>",
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
                "type": "HiveSource",
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
