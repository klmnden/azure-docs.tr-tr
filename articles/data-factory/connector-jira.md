---
title: Azure Data Factory (Beta) kullanarak Jira veri kopyalama | Microsoft Docs
description: "Desteklenen havuz veri depolarına Jira bir Azure Data Factory ardışık düzeninde kopyalama etkinliği kullanarak verileri kopyalamak öğrenin."
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
ms.openlocfilehash: 98f160a17d459bd8bfcc299c3a10d3ec4f5f6106
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="copy-data-from-jira-using-azure-data-factory-beta"></a>Azure Data Factory (Beta) kullanarak Jira verilerini

Bu makalede kopya etkinliği Azure Data Factory'de Jira verileri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 kopyalama etkinliği](v1/data-factory-data-movement-activities.md).

> [!IMPORTANT]
> Bu şu anda Beta Bağlayıcıdır. Deneyin ve bize geri bildirimde bulunun. Üretim ortamında kullanmayın.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna Jira veri kopyalayabilirsiniz. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Azure Data Factory bağlantısını etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak sürücüyü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

.NET SDK'sı, Python SDK'sı, Azure PowerShell, REST API veya Azure Resource Manager şablonu kullanarak kopyalama etkinliği ile işlem hattı oluşturabilirsiniz. Bkz: [kopyalama etkinliği öğretici](quickstart-create-data-factory-dot-net.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

Aşağıdaki bölümler, belirli Data Factory varlıklarını Jira bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler Jira bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Jira** | Evet |
| ana bilgisayar | Jira hizmeti IP adresi veya ana bilgisayar adı. (örneğin, jira.example.com)  | Evet |
| port | İstemci bağlantılarını dinlemek için Jira sunucusunun kullandığı TCP bağlantı noktası. HTTP bağlanılıyorsa HTTPS veya 8080 bağlanma ise, varsayılan değer 443'ü kullanır.  | Hayır |
| kullanıcı adı | Jira hizmete erişmek için kullandığınız kullanıcı adı.  | Evet |
| password | Kullanıcı adı alanında sağlanan kullanıcı adı için karşılık gelen parola. Bu alan ADF içinde güvenli şekilde depolayın veya Azure anahtar kasası parolayı depolamak için bir SecureString olarak işaretlemek seçin ve veri kopyalama gerçekleştirirken buradan çekme-'dan daha fazla bilgi kopyalama etkinliği izin [anahtar kasasına kimlik bilgilerini saklamak](store-credentials-in-key-vault.md). | Evet |
| useEncryptedEndpoints | Veri kaynağı uç noktaları HTTPS kullanılarak şifrelenmiş olup olmadığını belirtir. Varsayılan değer true olur.  | Hayır |
| useHostVerification | SSL üzerinden bağlanırken sunucusunun ana bilgisayar adı ile eşleşmesi için sunucunun sertifikası ana bilgisayar adlarında istenip istenmeyeceğini belirtir. Varsayılan değer true olur.  | Hayır |
| usePeerVerification | SSL üzerinden bağlanırken sunucusunun kimliğini doğrulamak belirtir. Varsayılan değer true olur.  | Hayır |

**Örnek:**

```json
{
    "name": "JiraLinkedService",
    "properties": {
        "type": "Jira",
        "typeProperties": {
            "host" : "<host>",
            "port" : "<port>",
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

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde Jira veri kümesi tarafından desteklenen özellikler listesini sağlar.

Jira verileri kopyalamak için kümesine tür özelliği ayarlamak **JiraObject**. Ek bir türe özel özellik bu tür bir veri kümesi yok.

**Örnek**

```json
{
    "name": "JiraDataset",
    "properties": {
        "type": "JiraObject",
        "linkedServiceName": {
            "referenceName": "<Jira linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde Jira kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="jirasource-as-source"></a>Kaynak olarak JiraSource

Jira verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **JiraSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **JiraSource** | Evet |
| sorgu | Verileri okumak için özel SQL sorgusu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Evet |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromJira",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Jira input dataset name>",
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
                "type": "JiraSource",
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
