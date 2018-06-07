---
title: Azure Data Factory (beta) kullanarak Impala veri kopyalama | Microsoft Docs
description: Desteklenen havuz veri depolarına Impala veri fabrikası ardışık düzeninde kopyalama etkinliği kullanarak verileri kopyalamak öğrenin.
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
ms.date: 02/07/2018
ms.author: jingwang
ms.openlocfilehash: 73987d03cb96fa421d193504fe6eaf6c3b5ddb18
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34618778"
---
# <a name="copy-data-from-impala-by-using-azure-data-factory-beta"></a>Azure Data Factory (beta) kullanarak Impala verileri kopyalama

Bu makalede kopya etkinliği Azure Data Factory'de Impala verileri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir, veri fabrikası 1 sürümünü kullanıyorsanız bkz [kopyalama etkinliğini sürüm 1](v1/data-factory-data-movement-activities.md).

> [!IMPORTANT]
> Bu şu anda beta Bağlayıcıdır. Deneyin ve geri bildirim sağlayın. Üretim ortamında kullanmayın.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna Impala veri kopyalayabilirsiniz. Kaynakları veya havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

 Veri Fabrikası bağlantısını etkinleştirmek için yerleşik bir sürücü sağlar. Bu nedenle, bu bağlayıcıyı kullanmak için bir sürücüyü el ile yüklemeniz gerekmez.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

Aşağıdaki bölümler, belirli Data Factory varlıklarını Impala bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Hizmeti Impala bağlı aşağıdaki özellikleri desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlamak **Impala**. | Evet |
| konak | (Diğer bir deyişle, 192.168.222.160) Impala sunucusunun IP adresi veya ana bilgisayar adı.  | Evet |
| port | İstemci bağlantılarını dinlemek için Impala sunucusunun kullandığı TCP bağlantı noktası. 21050 varsayılan değerdir.  | Hayır |
| authenticationType | Kullanılacak kimlik doğrulama türü. <br/>İzin verilen değerler **anonim**, **SASLUsername**, ve **UsernameAndPassword**. | Evet |
| kullanıcı adı | Impala sunucuya erişmek için kullanılan kullanıcı adı. Varsayılan değer anonim olduğunda SASLUsername kullanın.  | Hayır |
| password | UsernameAndPassword kullandığınızda, kullanıcı adına karşılık gelen parola. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). | Hayır |
| enableSsl | Sunucusuna bağlantılarda SSL kullanarak şifrelenir olup olmadığını belirtir. Varsayılan değer **false**.  | Hayır |
| trustedCertPath | Sunucu SSL üzerinden bağlandığında doğrulamak için kullanılan güvenilir CA sertifikaları içeren .pem dosyasının tam yolu. Bu özellik, yalnızca SSL Self-hosted tümleştirmesi Çalışma Zamanı Modülü kullandığınızda ayarlayabilirsiniz. Varsayılan değer ile tümleştirme çalışma zamanının yüklü cacerts.pem dosyasıdır.  | Hayır |
| useSystemTrustStore | Bir CA sertifikası sistem güven deposundan veya belirtilen PEM dosyası kullanılıp kullanılmayacağını belirtir. Varsayılan değer **false**.  | Hayır |
| allowHostNameCNMismatch | SSL üzerinden bağlandığında, sunucunun ana bilgisayar adını eşleştirmek için bir CA tarafından verilen SSL sertifika adı istenip istenmeyeceğini belirtir. Varsayılan değer **false**.  | Hayır |
| allowSelfSignedServerCert | Otomatik olarak imzalanan sertifikalar sunucudan izin verilip verilmeyeceğini belirtir. Varsayılan değer **false**.  | Hayır |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu genel olarak erişilebilir ise) Self-hosted tümleştirmesi çalışma zamanı veya Azure tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. |Hayır |

**Örnek:**

```json
{
    "name": "ImpalaLinkedService",
    "properties": {
        "type": "Impala",
        "typeProperties": {
            "host" : "<host>",
            "port" : "<port>",
            "authenticationType" : "UsernameAndPassword",
            "username" : "<username>",
            "password": {
                 "type": "SecureString",
                 "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, Impala veri kümesi tarafından desteklenen özellikler listesini sağlar.

Impala verileri kopyalamak için kümesine tür özelliği ayarlamak **ImpalaObject**. Ek bir türe özel özellik bu tür bir veri kümesi yok.

**Örnek**

```json
{
    "name": "ImpalaDataset",
    "properties": {
        "type": "ImpalaObject",
        "linkedServiceName": {
            "referenceName": "<Impala linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde Impala kaynak türü tarafından desteklenen özellikler listesini sağlar.

### <a name="impala-as-a-source-type"></a>Bir kaynak türü olarak ımpala

Impala verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **ImpalaSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak **ImpalaSource**. | Evet |
| sorgu | Verileri okumak için özel SQL sorgusu kullanın. `"SELECT * FROM MyTable"` bunun bir örneğidir. | Evet |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromImpala",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Impala input dataset name>",
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
                "type": "ImpalaSource",
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
Kaynakları ve havuzlarını Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
