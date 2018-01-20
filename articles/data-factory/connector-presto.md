---
title: Azure Data Factory (Beta) kullanarak Presto veri kopyalama | Microsoft Docs
description: "Desteklenen havuz veri depolarına Presto bir Azure Data Factory ardışık düzeninde kopyalama etkinliği kullanarak verileri kopyalamak öğrenin."
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
ms.openlocfilehash: 440a644e0b43fd42a446ec3e1a6a8d32a0b247cd
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="copy-data-from-presto-using-azure-data-factory-beta"></a>Azure Data Factory (Beta) kullanarak Presto verilerini

Bu makalede kopya etkinliği Azure Data Factory'de Presto verileri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 kopyalama etkinliği](v1/data-factory-data-movement-activities.md).

> [!IMPORTANT]
> Bu şu anda Beta Bağlayıcıdır. Deneyin ve bize geri bildirimde bulunun. Üretim ortamında kullanmayın.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Tüm desteklenen havuz veri deposuna Presto veri kopyalayabilirsiniz. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Azure Data Factory bağlantısını etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak sürücüyü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

Aşağıdaki bölümler, belirli Data Factory varlıklarını Presto bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler Presto bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Presto** | Evet |
| konak | Presto sunucusunun IP adresi veya ana bilgisayar adı. (i.e. 192.168.222.160)  | Evet |
| serverVersion | Presto sunucu sürümü. (i.e. 0.148-t)  | Evet |
| katalog | Sunucusuna yönelik tüm istekleri için katalog bağlamı.  | Evet |
| port | İstemci bağlantılarını dinlemek için Presto sunucusunun kullandığı TCP bağlantı noktası. Varsayılan değer 8080'dir.  | Hayır |
| authenticationType | Presto sunucuya bağlanmak için kullanılan kimlik doğrulama mekanizması. <br/>İzin verilen değerler: **anonim**, **LDAP** | Evet |
| kullanıcı adı | Presto sunucuya bağlanmak için kullanılan kullanıcı adı.  | Hayır |
| password | Kullanıcı adına karşılık gelen parola. Bu alan ADF içinde güvenli şekilde depolayın veya Azure anahtar kasası parolayı depolamak için bir SecureString olarak işaretlemek seçin ve veri kopyalama gerçekleştirirken buradan çekme-'dan daha fazla bilgi kopyalama etkinliği izin [anahtar kasasına kimlik bilgilerini saklamak](store-credentials-in-key-vault.md). | Hayır |
| enableSsl | Sunucusuna bağlantılarda SSL kullanılarak şifrelenir olup olmadığını belirtir. Varsayılan değer false.  | Hayır |
| trustedCertPath | Sunucu SSL üzerinden bağlanırken doğrulamak için güvenilen CA sertifikaları içeren .pem dosyasının tam yolu. Bu özellik yalnızca SSL üzerinde kendini barındıran IR kullanırken ayarlanabilir Varsayılan değer ile IR yüklü cacerts.pem dosyasıdır  | Hayır |
| useSystemTrustStore | Bir CA sertifikası sistem güven deposundan veya belirtilen PEM dosyası kullanılıp kullanılmayacağını belirtir. Varsayılan değer false.  | Hayır |
| allowHostNameCNMismatch | SSL üzerinden bağlanırken sunucusunun ana bilgisayar adı ile eşleşmesi için CA tarafından verilen SSL sertifika adı istenip istenmeyeceğini belirtir. Varsayılan değer false.  | Hayır |
| allowSelfSignedServerCert | Otomatik olarak imzalanan sertifikalar sunucudan izin verilip verilmeyeceğini belirtir. Varsayılan değer false.  | Hayır |
| timeZoneID | Bağlantı tarafından kullanılan yerel saat dilimi. Bu seçenek için geçerli değerler IANA saat dilimi veritabanında belirtilir. Sistem saat dilimi varsayılan değerdir.  | Hayır |

**Örnek:**

```json
{
    "name": "PrestoLinkedService",
    "properties": {
        "type": "Presto",
        "typeProperties": {
            "host" : "<host>",
            "serverVersion" : "0.148-t",
            "catalog" : "<catalog>",
            "port" : "<port>",
            "authenticationType" : "Anonymous",
            "username" : "<username>",
            "password": {
                 "type": "SecureString",
                 "value": "<password>"
            },
            "timeZoneID" : "Europe/Berlin"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde Presto veri kümesi tarafından desteklenen özellikler listesini sağlar.

Presto verileri kopyalamak için kümesine tür özelliği ayarlamak **PrestoObject**. Ek bir türe özel özellik bu tür bir veri kümesi yok.

**Örnek**

```json
{
    "name": "PrestoDataset",
    "properties": {
        "type": "PrestoObject",
        "linkedServiceName": {
            "referenceName": "<Presto linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde Presto kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="prestosource-as-source"></a>Kaynak olarak PrestoSource

Presto verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **PrestoSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **PrestoSource** | Evet |
| sorgu | Verileri okumak için özel SQL sorgusu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Evet |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromPresto",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Presto input dataset name>",
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
                "type": "PrestoSource",
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
