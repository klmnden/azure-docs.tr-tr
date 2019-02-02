---
title: Azure anahtar Kasası'nda kimlik bilgileri Store | Microsoft Docs
description: Azure Data Factory çalışma zamanında otomatik olarak almak bir Azure anahtar Kasası'nda kullanılan veri depoları için kimlik bilgilerini depolamaya hakkında bilgi edinin.
services: data-factory
author: linda33wj
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: jingwang
ms.openlocfilehash: ff070adbda2a36261ca24eb0cc993ca22eada1c7
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55661250"
---
# <a name="store-credential-in-azure-key-vault"></a>Azure anahtar Kasası'nda kimlik bilgisi Store

Veri depoları ve işlemler, kimlik bilgilerini depolamak bir [Azure anahtar kasası](../key-vault/key-vault-whatis.md). Azure Data Factory, veri deposu/işlem kullanan bir etkinlik yürütülürken kimlik bilgilerini alır.

Şu anda özel etkinlik hariç tüm etkinlik türleri bu özelliği destekler. Bağlayıcı yapılandırması için özellikle, "bağlı hizmet özellikleri" bölümüne iade [her konu başlığı](copy-activity-overview.md#supported-data-stores-and-formats) Ayrıntılar için.

## <a name="prerequisites"></a>Önkoşullar

Bu özellik üzerinde veri fabrikası hizmet kimliği kullanır. Gelen nasıl çalıştığını öğrenin [veri fabrikası hizmet kimliği](data-factory-service-identity.md) ve ilişkili bir veri fabrikanıza olduğundan emin olun.

## <a name="steps"></a>Adımlar

Azure Key Vault'ta depolanan bir kimlik bilgisi başvuru yapmak için gerekir:

1. **Veri Fabrikası hizmet kimliği almak** "Hizmet kimliği uygulama fabrikanızı birlikte oluşturulan kimliği" değerini kopyalayarak. Kullanıcı Arabirimi geliştirme ADF kullanırsanız, hizmet kimliği Azure Key Vault bağlı hizmeti oluşturma penceresinde gösterilir; Ayrıca almak Azure Portalı'ndan başvurmak [almak, veri fabrikası hizmet kimliği](data-factory-service-identity.md#retrieve-service-identity).
2. **Azure Key Vault ile hizmet kimliği erişim verin.** Anahtar kasanıza erişim -> ilkeler -> Ekle Yeni -> Bu hizmeti kimliği uygulama kimliği vermek için arama **alma** izni gizli dizi izinleri açılır. Anahtar kasasındaki gizli erişmek belirlenen Bu fabrika sağlar.
3. **Azure anahtar Kasası'na işaret eden bir bağlı hizmet oluşturursunuz.** Başvurmak [Azure Key Vault bağlı hizmeti](#azure-key-vault-linked-service).
4. **Veri deposu bağlı hizmetini, hangi Başvurusu içinde karşılık gelen depolanan gizli anahtar kasası oluşturun.** Başvurmak [başvuru gizli anahtar kasasında depolanan](#reference-secret-stored-in-key-vault).

## <a name="azure-key-vault-linked-service"></a>Azure Key Vault bağlı hizmeti

Azure Key Vault bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **AzureKeyVault**. | Evet |
| baseUrl | Azure Key Vault URL'si belirtin. | Evet |

**Kullanıcı Arabirimi geliştirme kullanarak:**

Tıklayın **bağlantıları** -> **bağlı hizmetler** -> **+ yeni** -> "Azure Key Vault" için arama:

![Arama AKV](media/store-credentials-in-key-vault/search-akv.png)

Kimlik bilgilerinizi depolandığı sağlanan Azure anahtar Kasası'nı seçin. Yapabileceğiniz **Bağlantıyı Sına** , AKV emin olmak için bağlantı geçerlidir. 

![AKV yapılandırın](media/store-credentials-in-key-vault/configure-akv.png)

**JSON örneği:**

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

## <a name="reference-secret-stored-in-key-vault"></a>Anahtar Kasası'nda depolanan başvuru gizli

Bir alan bir anahtar kasası gizli dizi başvuran bağlı hizmette yapılandırdığınızda, aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Alan öğesinin type özelliği ayarlanmalıdır: **AzureKeyVaultSecret**. | Evet |
| secretName | Azure key vault'ta gizli dizi adı. | Evet |
| secretVersion | Azure key vault'ta gizli dizi sürümü.<br/>Belirtilmezse, her zaman en son sürümünü gizli anahtarı kullanır.<br/>Ardından belirtilmişse belirli bir sürümüne yapışması.| Hayır |
| Store | Kimlik bilgilerini saklamak için kullandığınız bir Azure Key Vault bağlı hizmetini ifade eder. | Evet |

**Kullanıcı Arabirimi geliştirme kullanarak:**

Seçin **Azure anahtar kasası** veri deposu/işlem bağlantısı oluşturulurken gizli alanlar için. Sağlanan Azure Key Vault bağlı hizmeti seçin ve sağlayan **gizli dizi adı**. İsteğe bağlı olarak bir gizli dizi sürümü de sağlayabilirsiniz. 

![AKV gizli yapılandırma](media/store-credentials-in-key-vault/configure-akv-secret.png)

**JSON örneği: ("parola" bölümüne bakın)**

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
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
