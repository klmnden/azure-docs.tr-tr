---
title: "Azure anahtar kasası kimlik bilgilerini saklamak | Microsoft Docs"
description: "Veri depoları Azure Data Factory çalışma zamanında otomatik olarak almak bir Azure anahtar kasası kullanılan kimlik bilgilerini depolamak öğrenin."
services: data-factory
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2017
ms.author: jingwang
ms.openlocfilehash: 145c2bc0556010389e78e523fde6fd4b9063f930
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="store-credential-in-azure-key-vault"></a>Azure anahtar kasası kimlik bilgisi deposu

Veri depolarında kimlik bilgilerini depolamak bir [Azure anahtar kasası](../key-vault/key-vault-whatis.md). Azure Data Factory veri deposunu kullanan bir etkinliği yürütülürken kimlik bilgilerini alır.

Şu anda [Dynamics Bağlayıcısı](connector-dynamics-crm-office-365.md), [Salesforce bağlayıcı](connector-salesforce.md) ve birkaç bu özelliği yeni bağlayıcılar desteği etkinleştirin. Daha fazla daha sonra gelen bekler. Her bağlayıcı konu ayrıntıları kontrol edebilirsiniz. Bu özelliği destekleyen gizli alanlar için açıklama bildiren bir notta görürsünüz "*Bu alan ADF içinde güvenli şekilde depolayın veya Azure anahtar Kasası ' parola depolamak ve buradan kopyalama etkinliklere çekme izin için SecureString olarak işaretlemek seçebilirsiniz veri kopyalama - gerçekleştirirken anahtar kasasında depolama kimlik bilgileri'dan daha fazla bilgi edinin.* "

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [Data Factory version1 belgelerine](v1/data-factory-introduction.md).

## <a name="prerequisites"></a>Ön koşullar

Bu özellik, veri fabrikası hizmet kimliğini kullanır. Gelen nasıl çalıştığını öğrenin [veri fabrikası hizmet kimliği](data-factory-service-identity.md) ve ilişkili bir veri fabrikanıza olduğundan emin olun.

## <a name="steps"></a>Adımlar

Azure anahtar kasasında depolanan bir kimlik bilgisi başvurmak için aktarmanız gerekir:

1. [Veri Fabrikası hizmet kimliği alma](data-factory-service-identity.md#retrieve-service-identity) "Hizmeti kimliği uygulama Fabrikanızda birlikte oluşturulan kimliği" değerini kopyalayarak.
2. Hizmet kimliği Azure anahtar Kasası'na erişim. Anahtar kasasına erişim denetimi -> -> Ekle -> Bu hizmeti kimliği uygulama kimliği en az eklemek için arama **okuyucu** izni. Gizli anahtar kasasında erişmek bu belirlenen üreteci sağlar.
3. Azure anahtar Kasası'na işaret eden bir bağlantılı hizmet oluşturun. Başvurmak [Azure anahtar kasası bağlantılı hizmeti](#azure-key-vault-linked-service).
4. Veri deposu bağlı hizmetini, hangi başvuru içinde karşılık gelen gizli depolanan anahtar kasası oluşturun. Başvurmak [anahtar kasasında depolanan kimlik bilgileri başvuru](#reference-credential-stored-in-key-vault).

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

## <a name="reference-credential-stored-in-key-vault"></a>Anahtar kasasında depolanan başvuru kimlik bilgisi

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