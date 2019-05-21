---
title: Özel ilke tanımı oluşturma
description: Azure İlkesi, özel iş kurallarını uygulamak için özel bir ilke tanımı oluşturabilir.
author: DCtheGeek
ms.author: dacoulte
ms.date: 04/23/2019
ms.topic: tutorial
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: e38eb1315cde3400b70925059d4dd50475a47835
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65979661"
---
# <a name="tutorial-create-a-custom-policy-definition"></a>Öğretici: Özel ilke tanımı oluşturma

Özel bir ilke tanımı, müşterilerin Azure'ı kullanarak kendi kuralları tanımlamak olanak tanır. Genellikle, bu kurallar uygular:

- Güvenlik uygulamaları
- Maliyet yönetimi
- Kuruluşa özgü kurallar (örneğin, adlandırma veya konumlar)

Hangi iş sürücü özel bir ilke oluşturmak için adımları yeni bir özel ilke tanımlamak için aynıdır.

Özel bir ilke oluşturmadan önce kontrol [ilkesi örnekleri](../samples/index.md) gereksinimlerinizi zaten eşleşen bir ilke olup olmadığını görmek için.

Özel bir ilke oluşturmak için bir yaklaşım, aşağıdaki adımları izler:

> [!div class="checklist"]
> - İş gereksinimlerinizi belirleyin
> - Her gereksinim için bir Azure kaynak özelliği eşleme
> - Harita bir diğer ad özelliği
> - Kullanmak için hangi etkisi belirleme
> - İlke tanımı oluştur

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="identify-requirements"></a>Gereksinimleri belirleme

İlke tanımı oluşturmadan önce ilkenin amacı anlamak önemlidir. Bu öğreticide, bir ortak kuruluş güvenlik gereksinimi hedef olarak yer alan adımların göstermek üzere kullanacağız:

- Her Depolama hesabı için HTTPS etkinleştirilmiş olması gerekir
- Her Depolama hesabı için HTTP devre dışı bırakılmalıdır

Gereksinimlerinizi açıkça hem de tanımlamanız gerekir "" ve "olmayacak biçimde" kaynak durumları.

Beklenen kaynak durumunu tanımladığımız, ancak biz istediğimiz tanımlanmamış henüz ile uyumlu olmayan kaynakları yaptık. Azure İlkesi, bir dizi destekler [etkileri](../concepts/effects.md). Bu öğretici için iş kuralları ile uyumlu değilse kaynaklarının oluşturulmasını önleyen olarak size yönelik iş gereksinimini tanımlarsınız. Bu hedefe ulaşmak için kullanacağız [Reddet](../concepts/effects.md#deny) efekt. İlkeyi belirli atamalar için askıya alma seçeneği de istiyoruz. Bu nedenle, kullanacağız [devre dışı bırakılmış](../concepts/effects.md#disabled) efekt ve uygulanması bir [parametre](../concepts/definition-structure.md#parameters) ilke tanımı'ndaki.

## <a name="determine-resource-properties"></a>Kaynak özelliklerini belirleme

İş gereksinimi temel alan, Azure İlkesi ile denetlemek için Azure kaynak depolama hesabı anlamına gelmektedir. Ancak biz özelliklerini ilke tanımında kullanmak üzere bilmiyorum. Azure İlkesi, bu nedenle bu kaynakta mevcut olan özelliklerin anlamanız gerekir kaynak JSON temsili karşı değerlendirir.

Bir Azure kaynak özelliklerini belirlemek için birçok yolu vardır. Bu öğretici için her şu konuları inceleyeceğiz:

- Resource Manager şablonları
  - Mevcut kaynak dışarı aktarma
  - Oluşturma deneyimi
  - Hızlı Başlangıç şablonları (GitHub)
  - Şablon başvuru belgeleri
- Azure Resource Manager

### <a name="resource-manager-templates"></a>Resource Manager şablonları

Bakmak için birkaç şekilde bir [Resource Manager şablonu](../../../azure-resource-manager/resource-manager-tutorial-create-encrypted-storage-accounts.md) yönetmek için aradığınız özellik içerir.

#### <a name="existing-resource-in-the-portal"></a>Portalda mevcut kaynağı

Özellikleri bulmak için en basit yolu, mevcut bir kaynağı aynı türde aramaktır. Uygulamak istediğiniz ayarı ile yapılandırılmış kaynaklara karşı Karşılaştırılacak değer de sağlar.
Bakmak **şablonu dışarı aktarma** sayfa (altında **ayarları**), belirli bir kaynak için Azure portalında.

![Şablon sayfasında, var olan kaynak dışarı aktarma](../media/create-custom-policy-definition/export-template.png)

Bunu yapmak için bir depolama hesabı şu örneğe benzer bir şablon gösterir:

```json
...
"resources": [{
    "comments": "Generalized from resource: '/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount'.",
    "type": "Microsoft.Storage/storageAccounts",
    "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
    },
    "kind": "Storage",
    "name": "[parameters('storageAccounts_mystorageaccount_name')]",
    "apiVersion": "2018-07-01",
    "location": "westus",
    "tags": {
        "ms-resource-usage": "azure-cloud-shell"
    },
    "scale": null,
    "properties": {
        "networkAcls": {
            "bypass": "AzureServices",
            "virtualNetworkRules": [],
            "ipRules": [],
            "defaultAction": "Allow"
        },
        "supportsHttpsTrafficOnly": false,
        "encryption": {
            "services": {
                "file": {
                    "enabled": true
                },
                "blob": {
                    "enabled": true
                }
            },
            "keySource": "Microsoft.Storage"
        }
    },
    "dependsOn": []
}]
...
```

Altında **özellikleri** adlı bir değer **supportsHttpsTrafficOnly** kümesine **false**. Bu özellik, bekliyoruz özelliği olabilir görülüyor. Ayrıca, **türü** kaynak **Microsoft.Storage/storageAccounts**. Türü bu tür kaynak yalnızca ilkeye sınırlamak olanak tanır.

#### <a name="create-a-resource-in-the-portal"></a>Portalda kaynak oluştur

Portal üzerinden başka bir yolu kaynak oluşturma deneyimidir. Bir depolama hesabı altındaki bir seçenek portal üzerinden oluşturulurken **Gelişmiş** sekmesi **gerekli güvenlik aktarımı**. Bu özelliğin _devre dışı bırakılmış_ ve _etkin_ seçenekleri. Bu seçeneği, büyük olasılıkla istiyoruz özelliği olduğunu doğrular, ek metin bilgi simgesine sahip. Ancak, portalda özellik adı bu ekranda bize değil.

Üzerinde **gözden geçir + Oluştur** sekmesi, bir bağlantı olduğu için sayfanın alt kısmındaki **Otomasyon için bir şablonunu indirebilirsiniz**. Bağlantı seçildiğinde, biz yapılandırılmış kaynak oluşturan şablonu açar. Bu durumda, iki temel bilgi parçasını bakın:

```json
...
"supportsHttpsTrafficOnly": {
    "type": "bool"
}
...
"properties": {
    "accessTier": "[parameters('accessTier')]",
    "supportsHttpsTrafficOnly": "[parameters('supportsHttpsTrafficOnly')]"
}
...
```

Bu bilgileri bize özellik türü gösterir ve ayrıca onaylar **supportsHttpsTrafficOnly** biz için sağlanmıyorsa özelliğidir.

#### <a name="quickstart-templates-on-github"></a>Hızlı Başlangıç şablonları GitHub üzerinde

[Azure hızlı başlangıç şablonları](https://github.com/Azure/azure-quickstart-templates) Github'da yüzlerce yerleşik farklı kaynaklar için Resource Manager şablonu vardır. Bu şablonlar, aradığınız kaynak özelliği bulmak için harika bir yol olabilir. Bazı özellikler aradığınız ne gibi görünüyor ancak başka bir denetim.

#### <a name="resource-reference-docs"></a>Kaynak başvuru belgeleri

Doğrulanacak **supportsHttpsTrafficOnly** olan özellik düzeltmek için Resource Manager şablon başvurusu için denetleyin [depolama hesabı kaynağı](/azure/templates/microsoft.storage/2018-07-01/storageaccounts) depolama sağlayıcısı.
Özellikleri nesnesi geçerli parametrelerin bir listesi vardır. Seçme [StorageAccountPropertiesCreateParameters nesne](/azure/templates/microsoft.storage/2018-07-01/storageaccounts#storageaccountpropertiescreateparameters-object) bağlantı kabul edilebilir özelliklerinin bir tablo gösterir. **supportsHttpsTrafficOnly** mevcut olduğundan ve hangi iş gereksinimlerini karşılamak üzere arıyoruz açıklama eşleşir.

### <a name="azure-resource-explorer"></a>Azure Resource Manager

Aracılığıyla, Azure kaynaklarını keşfetmek için başka bir yolu ise [Azure kaynak Gezgini](https://resources.azure.com) (Önizleme). Bu araç, aboneliğinizi bağlamında kullanır, bu nedenle Web sitesine Azure kimlik bilgilerinizle kimlik doğrulaması yapmanız gerekir. Kimlik doğrulandıktan sonra sağlayıcıları, abonelikler, kaynak grupları ve kaynaklara göz atabilirsiniz.

Bir depolama hesabı kaynağı bulun ve özelliklerini arayın. Görüyoruz **supportsHttpsTrafficOnly** özelliği de burada. Seçme **belgeleri** sekmesinde görüyoruz description özelliği ne referans belgeler daha önce bulduk eşleşir.

## <a name="find-the-property-alias"></a>Özelliği diğer adı bulunamadı

Kaynak özelliği belirledik, ancak bu özelliğe eşlemek ihtiyacımız bir [diğer](../concepts/definition-structure.md#aliases).

Bir Azure kaynağı için diğer adlar belirlemek için birkaç yolu vardır. Bu öğretici için her şu konuları inceleyeceğiz:

- Azure CLI'si
- Azure PowerShell
- Azure Kaynak Grafı

### <a name="azure-cli"></a>Azure CLI'si

Azure CLI'de `az provider` kaynak diğer adları aramak için kullanılan komut grubu. İçin filtreleyeceğiz **Microsoft.Storage** ad alanı temel aldık Azure kaynak hakkında daha önce ayrıntıları.

```azurecli-interactive
# Login first with az login if not using Cloud Shell

# Get Azure Policy aliases for type Microsoft.Storage
az provider show --namespace Microsoft.Storage --expand "resourceTypes/aliases" --query "resourceTypes[].aliases[].name"
```

Sonuçlarda adlı depolama hesabı tarafından desteklenen bir diğer ad görüyoruz **supportsHttpsTrafficOnly**. Bu diğer adı bu varlığı bizim iş gereksinimlerini zorlamak için ilke yazabiliriz anlamına gelir!

### <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell'de `Get-AzPolicyAlias` cmdlet'i, kaynak diğer adları aramak için kullanılır. İçin filtreleyeceğiz **Microsoft.Storage** ad alanı temel aldık Azure kaynak hakkında daha önce ayrıntıları.

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Use Get-AzPolicyAlias to list aliases for Microsoft.Storage
(Get-AzPolicyAlias -NamespaceMatch 'Microsoft.Storage').Aliases
```

Azure CLI gibi adlı depolama hesabı tarafından desteklenen bir diğer ad sonuçlarını göster **supportsHttpsTrafficOnly**.

### <a name="azure-resource-graph"></a>Azure Kaynak Grafı

[Azure Kaynak Grafiği](../../resource-graph/overview.md) önizlemede yeni bir hizmettir. Bu, Azure kaynaklarını özelliklerini bulmak için başka bir yöntem sağlar. Aşağıda, bir tek bir depolama hesabı kaynak grafiği ile arama için örnek sorgu verilmiştir:

```kusto
where type=~'microsoft.storage/storageaccounts'
| limit 1
```

```azurecli-interactive
az graph query -q "where type=~'microsoft.storage/storageaccounts' | limit 1"
```

```azurepowershell-interactive
Search-AzGraph -Query "where type=~'microsoft.storage/storageaccounts' | limit 1"
```

Sonuçlar şu Resource Manager şablonları ve Azure kaynak Gezgini üzerinden gördükleri için benzer görünür. Ancak, Azure kaynak Graph sonuçları da içerebilir [diğer](../concepts/definition-structure.md#aliases) tarafından ayrıntıları _planlanması_ _diğer adlar_ dizisi:

```kusto
where type=~'microsoft.storage/storageaccounts'
| limit 1
| project aliases
```

```azurecli-interactive
az graph query -q "where type=~'microsoft.storage/storageaccounts' | limit 1 | project aliases"
```

```azurepowershell-interactive
Search-AzGraph -Query "where type=~'microsoft.storage/storageaccounts' | limit 1 | project aliases"
```

Diğer adlar için bir depolama hesabından örnek çıktı aşağıdaki gibidir:

```json
"aliases": {
    "Microsoft.Storage/storageAccounts/accessTier": null,
    "Microsoft.Storage/storageAccounts/accountType": "Standard_LRS",
    "Microsoft.Storage/storageAccounts/enableBlobEncryption": true,
    "Microsoft.Storage/storageAccounts/enableFileEncryption": true,
    "Microsoft.Storage/storageAccounts/encryption": {
        "keySource": "Microsoft.Storage",
        "services": {
            "blob": {
                "enabled": true,
                "lastEnabledTime": "2018-06-04T17:59:14.4970000Z"
            },
            "file": {
                "enabled": true,
                "lastEnabledTime": "2018-06-04T17:59:14.4970000Z"
            }
        }
    },
    "Microsoft.Storage/storageAccounts/encryption.keySource": "Microsoft.Storage",
    "Microsoft.Storage/storageAccounts/encryption.keyvaultproperties.keyname": null,
    "Microsoft.Storage/storageAccounts/encryption.keyvaultproperties.keyvaulturi": null,
    "Microsoft.Storage/storageAccounts/encryption.keyvaultproperties.keyversion": null,
    "Microsoft.Storage/storageAccounts/encryption.services": {
        "blob": {
            "enabled": true,
            "lastEnabledTime": "2018-06-04T17:59:14.4970000Z"
        },
        "file": {
            "enabled": true,
            "lastEnabledTime": "2018-06-04T17:59:14.4970000Z"
        }
    },
    "Microsoft.Storage/storageAccounts/encryption.services.blob": {
        "enabled": true,
        "lastEnabledTime": "2018-06-04T17:59:14.4970000Z"
    },
    "Microsoft.Storage/storageAccounts/encryption.services.blob.enabled": true,
    "Microsoft.Storage/storageAccounts/encryption.services.file": {
        "enabled": true,
        "lastEnabledTime": "2018-06-04T17:59:14.4970000Z"
    },
    "Microsoft.Storage/storageAccounts/encryption.services.file.enabled": true,
    "Microsoft.Storage/storageAccounts/networkAcls": {
        "bypass": "AzureServices",
        "defaultAction": "Allow",
        "ipRules": [],
        "virtualNetworkRules": []
    },
    "Microsoft.Storage/storageAccounts/networkAcls.bypass": "AzureServices",
    "Microsoft.Storage/storageAccounts/networkAcls.defaultAction": "Allow",
    "Microsoft.Storage/storageAccounts/networkAcls.ipRules": [],
    "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]": [],
    "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].action": [],
    "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].value": [],
    "Microsoft.Storage/storageAccounts/networkAcls.virtualNetworkRules": [],
    "Microsoft.Storage/storageAccounts/networkAcls.virtualNetworkRules[*]": [],
    "Microsoft.Storage/storageAccounts/networkAcls.virtualNetworkRules[*].action": [],
    "Microsoft.Storage/storageAccounts/networkAcls.virtualNetworkRules[*].id": [],
    "Microsoft.Storage/storageAccounts/networkAcls.virtualNetworkRules[*].state": [],
    "Microsoft.Storage/storageAccounts/primaryEndpoints": {
        "blob": "https://mystorageaccount.blob.core.windows.net/",
        "file": "https://mystorageaccount.file.core.windows.net/",
        "queue": "https://mystorageaccount.queue.core.windows.net/",
        "table": "https://mystorageaccount.table.core.windows.net/"
    },
    "Microsoft.Storage/storageAccounts/primaryEndpoints.blob": "https://mystorageaccount.blob.core.windows.net/",
    "Microsoft.Storage/storageAccounts/primaryEndpoints.file": "https://mystorageaccount.file.core.windows.net/",
    "Microsoft.Storage/storageAccounts/primaryEndpoints.queue": "https://mystorageaccount.queue.core.windows.net/",
    "Microsoft.Storage/storageAccounts/primaryEndpoints.table": "https://mystorageaccount.table.core.windows.net/",
    "Microsoft.Storage/storageAccounts/primaryEndpoints.web": null,
    "Microsoft.Storage/storageAccounts/primaryLocation": "eastus2",
    "Microsoft.Storage/storageAccounts/provisioningState": "Succeeded",
    "Microsoft.Storage/storageAccounts/sku.name": "Standard_LRS",
    "Microsoft.Storage/storageAccounts/sku.tier": "Standard",
    "Microsoft.Storage/storageAccounts/statusOfPrimary": "available",
    "Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly": false
}
```

Azure Kaynak Grafiği (Önizleme) aracılığıyla kullanılabilir [Cloud Shell](https://shell.azure.com), kaynaklarınızı özelliklerini keşfetmek için hızlı ve kolay bir yolunu kolaylaştırır.

## <a name="determine-the-effect-to-use"></a>Kullanılacak etkisini belirlemek

Uyumlu olmayan kaynaklarınızla yapmanız gerekenler karar ne ilk başta değerlendirilecek karar olarak neredeyse da önemlidir. Uyumlu olmayan bir kaynağa her olası yanıt olarak adlandırılan bir [etkisi](../concepts/effects.md).
Uyumlu olmayan kaynak açtıysa engellenen etkili denetimler veri eklenmiş veya bir dağıtım için kaynak geri uyumlu duruma koymak için ilişkili.

Bizim örneğimizde, reddetme, uyumlu olmayan kaynakları Azure ortamımızda oluşturulan istemediğiniz gibi istiyoruz etkisidir. Denetim Reddet olarak ayarlamadan önce bir ilkenin etkisi nedir belirlemek bir ilke etkisi için iyi bir ilk seçimdir. Daha kolay atama temelinde geçerli değiştirmek için bir efekti parametre haline getirmek için yoludur. Bkz: [parametreleri](#parameters) aşağıda hakkında ayrıntılar için.

## <a name="compose-the-definition"></a>Tanımı oluştur

Artık özellik ayrıntılarını ve yönetmek planlıyoruz için diğer ad sahibiz. Ardından, biz ilke kuralı oluşturmak. Henüz ilke dili ile ilgili bilgi sahibi değilseniz, başvuru [İlkesi tanım yapısı](../concepts/definition-structure.md) ilke tanımı yapısı öğrenmek için. Bir ilke tanımı göründüğünü boş bir şablon şöyledir:

```json
{
    "properties": {
        "displayName": "<displayName>",
        "description": "<description>",
        "mode": "<mode>",
        "parameters": {
                <parameters>
        },
        "policyRule": {
            "if": {
                <rule>
            },
            "then": {
                "effect": "<effect>"
            }
        }
    }
}
```

### <a name="metadata"></a>Meta veriler

İlk üç ilke meta verilerini bileşenlerdir. Kural ne oluşturuyoruz bildiğiniz için değerler sağlamak bu bileşenlerin kolaydır. [Modu](../concepts/definition-structure.md#mode) öncelikle etiketleri ve kaynak konumu hakkında. Değerlendirme etiketleri destekleyen kaynaklara sınırlamak gerekmez bu yana kullanacağız _tüm_ değerini **modu**.

```json
"displayName": "Deny storage accounts not using only HTTPS",
"description": "Deny storage accounts not using only HTTPS. Checks the supportsHttpsTrafficOnly property on StorageAccounts.",
"mode": "all",
```

### <a name="parameters"></a>Parametreler

Değerlendirme değiştirmek için size bir parametre kullanmadı karşın, biz bir parametre değiştirilmesine izin ver için kullanmak istediğiniz **etkisi** sorun giderme. Tanımlama olasılığınız bir **effectType** parametresi ve yalnızca sınırlamak **Reddet** ve **devre dışı bırakılmış**. Bu iki seçenek bizim iş gereksinimlerini karşılamak. Tamamlanmış parametreler blok şu örnekteki gibi görünür:

```json
"parameters": {
    "effectType": {
        "type": "string",
        "defaultValue": "Deny",
        "allowedValues": [
            "Deny",
            "Disabled"
        ],
        "metadata": {
            "displayName": "Effect",
            "description": "Enable or disable the execution of the policy"
        }
    }
},
```

### <a name="policy-rule"></a>İlke kuralı

Oluşturma [ilke kuralı](../concepts/definition-structure.md#policy-rule) bizim özel bir ilke tanımı oluşturmanın son adımdır. Biz, test etmek için iki deyim tespit ettik:

- Depolama hesabı **türü** olduğu **Microsoft.Storage/storageAccounts**
- Depolama hesabı **supportsHttpsTrafficOnly** değil **true**

Her iki ifade true olarak ihtiyacımız beri kullanacağız **tümü** [mantıksal işleç](../concepts/definition-structure.md#logical-operators). Biz de ileteceksiniz **effectType** statik bildirimi yapmak yerine etkili olması için parametre. Bizim tamamlanmış kuralı şu örnekteki gibi görünür:

```json
"if": {
    "allOf": [
        {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
        },
        {
            "field": "Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly",
            "notEquals": "true"
        }
    ]
},
"then": {
    "effect": "[parameters('effectType')]"
}
```

### <a name="completed-definition"></a>Tamamlanma tanımı

Tüm üç bölümden tanımlanan ilke ile tamamlanmış bizim tanımı aşağıda verilmiştir:

```json
{
    "properties": {
        "displayName": "Deny storage accounts not using only HTTPS",
        "description": "Deny storage accounts not using only HTTPS. Checks the supportsHttpsTrafficOnly property on StorageAccounts.",
        "mode": "all",
        "parameters": {
            "effectType": {
                "type": "string",
                "defaultValue": "Deny",
                "allowedValues": [
                    "Deny",
                    "Disabled"
                ],
                "metadata": {
                    "displayName": "Effect",
                    "description": "Enable or disable the execution of the policy"
                }
            }
        },
        "policyRule": {
            "if": {
                "allOf": [
                    {
                        "field": "type",
                        "equals": "Microsoft.Storage/storageAccounts"
                    },
                    {
                        "field": "Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly",
                        "notEquals": "true"
                    }
                ]
            },
            "then": {
                "effect": "[parameters('effectType')]"
            }
        }
    }
}
```

Tamamlanma tanımı, yeni bir ilke oluşturmak için kullanılabilir. Portal ve her bir SDK (Azure CLI, Azure PowerShell ve REST API'si) tanımı farklı şekillerde kabul edin, böylece her doğru kullanım doğrulamak için komutları gözden geçirin. Ardından, depolama hesaplarınızın güvenliğini yönetmek için uygun kaynaklara parametreli efekt kullanarak atayın.

## <a name="review"></a>İncele

Bu öğreticide, aşağıdaki görevleri başarıyla gerçekleştirdiniz:

> [!div class="checklist"]
> - İş gereksinimlerinizi tanımlanan
> - Her gereksinim, bir Azure kaynak özellikle eşleniyor.
> - Özellik için bir diğer ad eşlendi
> - Kullanılacak etkisini belirler
> - İlke tanımı oluşur

## <a name="next-steps"></a>Sonraki adımlar

Ardından, bir ilkesi oluşturma ve atama için özel bir ilke tanımınız kullanın:

> [!div class="nextstepaction"]
> [Bir ilke tanımı oluşturma ve atama](../how-to/programmatically-create.md#create-and-assign-a-policy-definition)