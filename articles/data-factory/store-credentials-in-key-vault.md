---
title: Azure anahtar kasası kimlik bilgilerini saklamak | Microsoft Docs
description: Veri depoları Azure Data Factory çalışma zamanında otomatik olarak almak bir Azure anahtar kasası kullanılan kimlik bilgilerini depolamak öğrenin.
services: data-factory
author: linda33wj
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2017
ms.author: jingwang
ms.openlocfilehash: 9a71a455ac4f406695edf722bc83604539eccaa9
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="store-credential-in-azure-key-vault"></a>Azure anahtar kasası kimlik bilgisi deposu

Veri depoları ve içinde hesaplar için kimlik bilgilerini depolamak bir [Azure anahtar kasası](../key-vault/key-vault-whatis.md). Azure Data Factory veri deposu/işlem kullanan bir etkinliği yürütülürken kimlik bilgilerini alır.

Şu anda bu özelliği özel etkinlik dışındaki tüm etkinlik türlerini destekler. Bağlayıcı yapılandırması için özellikle "bağlantılı hizmet özellikleri" bölümüne bakın [her bağlayıcı konu](copy-activity-overview.md#supported-data-stores-and-formats) Ayrıntılar için.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [Data Factory version1 belgelerine](v1/data-factory-introduction.md).

## <a name="prerequisites"></a>Önkoşullar

Bu özellik, veri fabrikası hizmet kimliğini kullanır. Gelen nasıl çalıştığını öğrenin [veri fabrikası hizmet kimliği](data-factory-service-identity.md) ve ilişkili bir veri fabrikanıza olduğundan emin olun.

## <a name="steps"></a>Adımlar

Azure anahtar kasasında depolanan bir kimlik bilgisi başvurmak için aktarmanız gerekir:

1. **[Veri Fabrikası hizmet kimliği alma](data-factory-service-identity.md#retrieve-service-identity)**  "Hizmeti kimliği uygulama Fabrikanızda birlikte oluşturulan kimliği" değerini kopyalayarak.
2. **Hizmet kimliği Azure anahtar Kasası'na erişim.** Anahtar kasasına erişim ilkeleri -> -> Ekle Yeni -> Bu hizmeti kimliği uygulama kimliği vermek için arama **almak** izin gizli izinleri açılır. Gizli anahtar kasasında erişmek bu belirlenen üreteci sağlar.
3. **Azure anahtar Kasası'na işaret eden bir bağlantılı hizmet oluşturun.** Başvurmak [Azure anahtar kasası bağlantılı hizmeti](#azure-key-vault-linked-service).
4. **Veri deposu bağlı hizmetini, hangi başvuru içinde karşılık gelen gizli depolanan anahtar kasası oluşturun.** Başvurmak [başvuru gizli anahtar kasasında depolanan](#reference-secret-stored-in-key-vault).

## <a name="azure-key-vault-linked-service"></a>Azure anahtar Kasası'bağlı hizmeti

Aşağıdaki özellikler, Azure anahtar kasası bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **AzureKeyVault**. | Evet |
| BaseUrl | Azure anahtar kasası URL'si belirtin. | Evet |

**Örnek:**

```json
{
    "name": "AzureKeyVaultLinkedService",
    "properties": {
        "type": "AzureKeyVault",
        "typeProperties": {
            "baseUrl": "https://<azureKeyVaultName>.vault.azure.net"
        }
    }
}
```

## <a name="reference-secret-stored-in-key-vault"></a>Başvuru gizli anahtar kasasında depolanan

Bir anahtar kasası gizlilik başvuran bağlantılı hizmetteki bir alan yapılandırırken aşağıdaki özellikleri desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Alanın türü özelliğini ayarlamak: **AzureKeyVaultSecret**. | Evet |
| secretName | Azure anahtar kasası gizliliği adı. | Evet |
| secretVersion | Azure anahtar kasası gizliliği sürümü.<br/>Belirtilmezse, her zaman en son sürümünü gizli anahtarı kullanır.<br/>Ardından belirtilirse, belirtilen sürüme sticks.| Hayır |
| mağaza | Kimlik bilgilerini saklamak için kullanacağınız bir Azure anahtar kasası bağlantılı hizmeti ifade eder. | Evet |

**Örnek: ("parola" bölümüne bakın)**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "typeProperties": {
            "deploymentType": "<>",
            "organizationName": "<>",
            "authenticationType": "<>",
            "username": "<>",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).