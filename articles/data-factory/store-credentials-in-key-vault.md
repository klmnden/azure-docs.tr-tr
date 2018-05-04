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
ms.date: 04/25/2017
ms.author: jingwang
ms.openlocfilehash: 4a8c96bf9124feede2e5a28beb791636784dcad7
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="store-credential-in-azure-key-vault"></a>Azure anahtar kasası kimlik bilgisi deposu

Veri depoları ve içinde hesaplar için kimlik bilgilerini depolamak bir [Azure anahtar kasası](../key-vault/key-vault-whatis.md). Azure Data Factory veri deposu/işlem kullanan bir etkinliği yürütülürken kimlik bilgilerini alır.

Şu anda bu özelliği özel etkinlik dışındaki tüm etkinlik türlerini destekler. Bağlayıcı yapılandırması için özellikle "bağlantılı hizmet özellikleri" bölümüne bakın [her bağlayıcı konu](copy-activity-overview.md#supported-data-stores-and-formats) Ayrıntılar için.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [Data Factory version1 belgelerine](v1/data-factory-introduction.md).

## <a name="prerequisites"></a>Önkoşullar

Bu özellik, veri fabrikası hizmet kimliğini kullanır. Gelen nasıl çalıştığını öğrenin [veri fabrikası hizmet kimliği](data-factory-service-identity.md) ve ilişkili bir veri fabrikanıza olduğundan emin olun.

>[!TIP]
>Azure anahtar kasasında bir gizlilik oluşturduğunuzda, **ADF hizmet bağlı gizli bir özellik değerini (örn. bağlantı dizesi/parola/hizmet asıl anahtar/vb. için) ister put**. Örneğin, Azure depolama bağlantılı hizmeti, put `DefaultEndpointsProtocol=http;AccountName=myAccount;AccountKey=myKey;` AKV gizli sonra ADF; Dynamics bağlantılı hizmeti için başvuru "connectionString" alanında put `myPassword` AKV gizli "parola" alanı ADF'dan sonra başvuru. Desteklenen özelliği ayrıntıları her bağlayıcı/işlem makalesine bakın.

## <a name="steps"></a>Adımlar

Azure anahtar kasasında depolanan bir kimlik bilgisi başvurmak için aktarmanız gerekir:

1. **Veri Fabrikası hizmet kimliği alma** "Hizmeti kimliği uygulama Fabrikanızda birlikte oluşturulan kimliği" değerini kopyalayarak. UI yazma ADF kullanırsanız, hizmet kimliği kimliği üzerinde Azure anahtar kasası bağlı hizmet oluşturma penceresi gösterilir; Ayrıca alabilirsiniz Azure portalından başvurmak [almak veri fabrikası hizmet kimliği](data-factory-service-identity.md#retrieve-service-identity).
2. **Hizmet kimliği Azure anahtar Kasası'na erişim.** Anahtar kasasına erişim ilkeleri -> -> Ekle Yeni -> Bu hizmeti kimliği uygulama kimliği vermek için arama **almak** izin gizli izinleri açılır. Gizli anahtar kasasında erişmek bu belirlenen üreteci sağlar.
3. **Azure anahtar Kasası'na işaret eden bir bağlantılı hizmet oluşturun.** Başvurmak [Azure anahtar kasası bağlantılı hizmeti](#azure-key-vault-linked-service).
4. **Veri deposu bağlı hizmetini, hangi başvuru içinde karşılık gelen gizli depolanan anahtar kasası oluşturun.** Başvurmak [başvuru gizli anahtar kasasında depolanan](#reference-secret-stored-in-key-vault).

## <a name="azure-key-vault-linked-service"></a>Azure anahtar Kasası'bağlı hizmeti

Aşağıdaki özellikler, Azure anahtar kasası bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **AzureKeyVault**. | Evet |
| BaseUrl | Azure anahtar kasası URL'si belirtin. | Evet |

**UI yazma kullanma:**

Tıklatın **bağlantıları** -> **bağlı hizmetler** -> **+ yeni** "Azure anahtar kasası" için arama ->:

![Arama AKV](media/store-credentials-in-key-vault/search-akv.png)

Sağlanan Azure anahtar kasası kimlik bilgilerinizi depolandığı seçin. Yapabileceğiniz **Bağlantıyı Sına** , AKV emin olmak için bağlantı geçerlidir. 

![AKV yapılandırın](media/store-credentials-in-key-vault/configure-akv.png)

**JSON örnek:**

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

**UI yazma kullanma:**

Seçin **Azure anahtar kasası** veri deposu/işlem bağlantı oluşturulurken gizli alanlar için. Sağlanan Azure anahtar kasası bağlı hizmeti seçin ve sağlamak **gizli anahtar adı**. İsteğe bağlı olarak gizli bir sürümünü de sağlayabilir. 

![AKV gizli yapılandırma](media/store-credentials-in-key-vault/configure-akv-secret.png)

**JSON örnek: ("parola" bölümüne bakın)**

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