---
title: Azure Data Factory (Beta) kullanarak Google BigQuery veri kopyalama | Microsoft Docs
description: "Veri kopyalama etkinliği Azure Data Factory ardışık düzeninde kullanarak Google BigQuery desteklenen havuz veri depolarına kopyalama öğrenin."
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
ms.openlocfilehash: 00962b1bb32ff096712d36c07620505e72667380
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="copy-data-from-google-bigquery-using-azure-data-factory-beta"></a>Google Azure Data Factory (Beta) kullanarak BigQuery verilerini

Bu makalede kopya etkinliği Azure Data Factory'de Google BigQuery verileri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 kopyalama etkinliği](v1/data-factory-data-movement-activities.md).

> [!IMPORTANT]
> Bu şu anda Beta Bağlayıcıdır. Deneyin ve bize geri bildirimde bulunun. Üretim ortamında kullanmayın.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Google BigQuery verileri herhangi bir desteklenen havuz veri deposuna kopyalayabilirsiniz. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Azure Data Factory bağlantısını etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak sürücüyü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

.NET SDK'sı, Python SDK'sı, Azure PowerShell, REST API veya Azure Resource Manager şablonu kullanarak kopyalama etkinliği ile işlem hattı oluşturabilirsiniz. Bkz: [kopyalama etkinliği öğretici](quickstart-create-data-factory-dot-net.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

Aşağıdaki bölümler, belirli Data Factory varlıklarını Google BigQuery bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Google BigQuery bağlantılı hizmetinin aşağıdaki özellikleri desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **GoogleBigQuery** | Evet |
| Proje | Varsayılan BigQuery projeye sorgusu.  | Evet |
| additionalProjects | Ortak virgülle ayrılmış bir listesini BigQuery erişimi projeleri.  | Hayır |
| requestGoogleDriveScope | Google sürücüye erişim istenip istenmeyeceğini belirtir. Google sürücü erişimine Google sürücüsünden verilerle BigQuery verileri birleştirmek birleştirilmiş tablolar için destek sağlar. Varsayılan değer false.  | Hayır |
| authenticationType | Kimlik doğrulaması için kullanılan OAuth 2.0 kimlik doğrulama mekanizması. ServiceAuthentication yalnızca kendi kendini barındıran IR üzerinde kullanılabilir <br/>İzin verilen değerler: **ServiceAuthentication**, **UserAuthentication** | Evet |
| refreshToken | UserAuthentication için BigQuery erişim yetkisi vermek için Google elde yenileme belirteci. Bu alan ADF içinde güvenli şekilde depolayın veya Azure anahtar kasası parolayı depolamak için bir SecureString olarak işaretlemek seçin ve veri kopyalama gerçekleştirirken buradan çekme-'dan daha fazla bilgi kopyalama etkinliği izin [anahtar kasasına kimlik bilgilerini saklamak](store-credentials-in-key-vault.md). | Hayır |
| e-posta | ServiceAuthentication için kullanılır ve yalnızca kendi kendini barındıran IR üzerinde kullanılabilir hizmet hesabı e-posta kimliği  | Hayır |
| keyFilePath | Hizmet hesabı e-posta adresi kimliğini doğrulamak için kullanılır ve yalnızca kendi kendini barındıran IR üzerinde kullanılabilir .p12 anahtar dosyasının tam yolu  | Hayır |
| trustedCertPath | Sunucu SSL üzerinden bağlanırken doğrulamak için güvenilen CA sertifikaları içeren .pem dosyasının tam yolu. Bu özellik yalnızca SSL üzerinde kendini barındıran IR kullanırken ayarlanabilir Varsayılan değer ile IR yüklü cacerts.pem dosyasıdır  | Hayır |
| useSystemTrustStore | Bir CA sertifikası sistem güven deposundan veya belirtilen PEM dosyası kullanılıp kullanılmayacağını belirtir. Varsayılan değer false.  | Hayır |

**Örnek:**

```json
{
    "name": "GoogleBigQueryLinkedService",
    "properties": {
        "type": "GoogleBigQuery",
        "typeProperties": {
            "project" : "<project>",
            "additionalProjects" : "<additionalProjects>",
            "requestGoogleDriveScope" : true,
            "authenticationType" : "UserAuthentication",
            "refreshToken": {
                 "type": "SecureString",
                 "value": "<refreshToken>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, Google BigQuery veri kümesi tarafından desteklenen özellikler listesini sağlar.

Google BigQuery verileri kopyalamak için kümesine tür özelliği ayarlamak **GoogleBigQueryObject**. Ek bir türe özel özellik bu tür bir veri kümesi yok.

**Örnek**

```json
{
    "name": "GoogleBigQueryDataset",
    "properties": {
        "type": "GoogleBigQueryObject",
        "linkedServiceName": {
            "referenceName": "<GoogleBigQuery linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Etkinlik özellikleri Kopyala

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde, Google BigQuery kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="googlebigquerysource-as-source"></a>Kaynak olarak GoogleBigQuerySource

Google BigQuery verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **GoogleBigQuerySource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **GoogleBigQuerySource** | Evet |
| sorgu | Verileri okumak için özel SQL sorgusu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Evet |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromGoogleBigQuery",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<GoogleBigQuery input dataset name>",
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
                "type": "GoogleBigQuerySource",
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
