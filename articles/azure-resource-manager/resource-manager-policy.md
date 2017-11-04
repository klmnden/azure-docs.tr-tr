---
title: Azure kaynak ilkeleri | Microsoft Docs
description: "Azure Resource Manager ilkeleri tutarlı kaynak özelliklerini dağıtımı sırasında ayarlandığından emin olmak için nasıl kullanılacağını açıklar. Abonelik veya kaynak gruplarının ilkeleri uygulanabilir."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: abde0f73-c0fe-4e6d-a1ee-32a6fce52a2d
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/09/2017
ms.author: tomfitz
ms.openlocfilehash: 2e0d2e9830639209a22e9b62b0679d31854150e4
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="resource-policy-overview"></a>Kaynak ilkesine genel bakış
Kaynak ilkeleri, kuruluşunuzdaki kaynakların kuralları oluşturmak etkinleştirin. Kuralları tanımlayarak, daha kolay kaynaklarınızı yönetmek ve maliyetleri denetleyebilirsiniz. Örneğin, sanal makineler yalnızca belirli türdeki izin verildiğini belirtebilirsiniz. Veya, tüm kaynakların belirli bir etikete sahip olması gerekir. İlkeler tüm alt kaynaklar tarafından devralınır. Bu nedenle, bir kaynak grubu için bir ilke uygulandığında, bu kaynak grubundaki tüm kaynaklar için geçerlidir.

İlkeleri hakkında anlamak için iki kavram vardır:

* ilke tanımı - zaman İlkesi uygulandığında ve hangi eylemin yapılacağını açıklar
* ilke ataması - (abonelik veya kaynak grubu) kapsam için ilke tanımı Uygula

Bu konu, ilke tanımı odaklanır. İlke ataması hakkında daha fazla bilgi için bkz: [atamak ve kaynak ilkelerini yönetmek için kullanım Azure portal](resource-manager-policy-portal.md) veya [atayın ve komut dosyası aracılığıyla ilkelerini yönetme](resource-manager-policy-create-assign.md).

İlkeleri oluşturma ve (PUT ve işlemleri düzeltme eki) kaynakları güncelleştirme değerlendirilir.

## <a name="how-is-it-different-from-rbac"></a>Nasıl RBAC farklı mı?
İlke ve rol tabanlı erişim denetimi (RBAC) arasındaki bazı farklar vardır. RBAC odaklanır **kullanıcı** farklı kapsamlar eylemlerini. Örneğin, bu kaynak grubuna değişiklik yapabilmek için bir kaynak grubu istenilen kapsamda katılımcı rolü eklenir. İlke odaklanır **kaynak** dağıtımı sırasında özellikler. Örneğin, ilkeler aracılığıyla sağlanabilir kaynak türleri kontrol edebilirsiniz. Ya da kaynaklar sağlanabilir konumları kısıtlayabilirsiniz. RBAC, bir varsayılan izin ilkedir ve açık sistem engelle. 

İlkelerini kullanmak için RBAC kimliğinin doğrulanması gerekir. Özellikle, hesabınızı gerekir:

* `Microsoft.Authorization/policydefinitions/write`ilke tanımlamak için izni
* `Microsoft.Authorization/policyassignments/write`bir ilke atama izni 

Bu izinleri dahil değildir **katkıda bulunan** rol.

## <a name="built-in-policies"></a>Yerleşik ilkeleri

Azure tanımlamak zorunda ilkeleri sayısını azaltabilir bazı yerleşik ilke tanımları sağlar. İlke tanımları ile devam etmeden önce yerleşik bir ilke zaten ihtiyaç duyduğunuz tanımı sağlayıp sağlamadığını göz önünde bulundurmalısınız. Yerleşik ilke tanımları şunlardır:

* İzin verilen konumların
* İzin verilen kaynak türleri
* Depolama hesabı SKU'ları izin
* Sanal makine SKU'ları izin
* Etiket ve varsayılan değer Uygula
* Etiket ve değer zorla
* Kaynak türleri izin verilmiyor
* SQL Server sürüm 12.0 gerektirir
* Depolama hesabı şifreleme iste

Herhangi bir aracılığıyla bu ilkeleri atayabilirsiniz [portal](resource-manager-policy-portal.md), [PowerShell](resource-manager-policy-create-assign.md#powershell), veya [Azure CLI](resource-manager-policy-create-assign.md#azure-cli).

## <a name="policy-definition-structure"></a>İlke tanımı yapısı
Bir ilke tanımı oluşturmak için JSON kullanın. İlke tanımı için öğeleri içerir:

* Modu
* parametreler
* Görünen ad
* açıklama
* İlke kuralı
  * mantıksal değerlendirme
  * Etkisi

Aşağıdaki örnek, kaynakları dağıtıldığı sınırlar bir ilke gösterir:

```json
{
  "properties": {
    "mode": "all",
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
    "policyRule": {
      "if": {
        "not": {
          "field": "location",
          "in": "[parameters('allowedLocations')]"
        }
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

## <a name="mode"></a>Modu

Ayarlamanızı öneririz `mode` için `all`. Ayarladığınızda bu **tüm**, kaynak grupları ve tüm kaynak türleri için ilke değerlendirilir. Portal kullanır **tüm** tüm ilkeler. PowerShell veya Azure CLI kullanıyorsanız, belirtmek zorunda `mode` parametre ve ayarlamak **tüm**.
 
Daha önce ilke etiketlerini ve konumunu desteklenen kaynak türleri üzerinde değerlendirildi. `indexed` Modu bu davranış devam eder. Kullanırsanız **tüm** modu, ilkeleri etiketlerini ve konumunu desteklemeyen kaynak türleri de değerlendirilir. [Sanal ağ alt](https://github.com/Azure/azure-policy-samples/tree/master/samples/Network/enforce-nsg-on-subnet) yeni eklenen türü örneğidir. Ayrıca, kaynak grupları modu ayarlandığında değerlendirilir **tüm**. Örneğin, [etiketleri bir kaynak grubu üzerinde zorunlu](https://github.com/Azure/azure-policy-samples/tree/master/samples/ResourceGroup/enforce-resourceGroup-tags). 

## <a name="parameters"></a>Parametreler
Parametrelerini kullanarak ilke tanımları sayısını azaltarak ilke yönetimini basitleştirmeye yardımcı olur. Bir kaynak özelliği (örneğin kaynaklar dağıtıldığı konumları sınırlama) için bir ilke tanımlayın ve tanımında parametreleri içerir. Ardından, bu ilke tanımı farklı senaryolar için (örneğin, bir dizi bir abonelik için konumları belirtme) farklı değerler geçirerek ne zaman yeniden ilke atama.

İlke tanımları oluşturduğunuzda parametreleri bildirin.

```json
"parameters": {
  "allowedLocations": {
    "type": "array",
    "metadata": {
      "description": "The list of allowed locations for resources.",
      "displayName": "Allowed locations"
    }
  }
}
```

Bir parametrenin türü string veya dizi olabilir. Meta veri özelliği, Azure portal gibi araçlar için kullanıcı dostu bilgileri görüntülemek için kullanılır. 

İlke kuralı aşağıdaki söz dizimini parametrelerle başvurusu: 

```json
{ 
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

## <a name="display-name-and-description"></a>Görünen ad ve açıklama

Kullandığınız **displayName** ve **açıklama** ilke tanımı tanımlamak ve kullanıldığında için bağlamı sağlar.

## <a name="policy-rule"></a>İlke kuralı

İlke kuralı oluşan **varsa** ve **sonra** engeller. İçinde **varsa** bloğu, ilke zaman zorlanır belirten bir veya daha fazla koşullarını tanımlayın. Mantıksal işleçler tam olarak bu senaryo için bir ilke tanımlamak için bu koşullar uygulayabilirsiniz. İçinde **sonra** bloğu olur etkisi tanımladığınız zaman **varsa** koşullar yerine.

```json
{
  "if": {
    <condition> | <logical operator>
  },
  "then": {
    "effect": "deny | audit | append"
  }
}
```

### <a name="logical-operators"></a>Mantıksal işleçler
Desteklenen mantıksal işleçler şunlardır:

* `"not": {condition  or operator}`
* `"allOf": [{condition or operator},{condition or operator}]`
* `"anyOf": [{condition or operator},{condition or operator}]`

**Değil** sözdizimi koşul sonucunu tersine çevirir. **Tümü** sözdizimi (benzer şekilde mantıksal **ve** işlemi) tüm koşulların doğru olmasını gerektirir. **Herhangi** sözdizimi (benzer şekilde mantıksal **veya** işlemi) doğru olması için bir veya birden çok koşul gerektirir.

Mantıksal işleçler yerleştirebilirsiniz. Aşağıdaki örnekte gösterildiği bir **değil** içinde iç içe işlemi bir **tümü** işlemi. 

```json
"if": {
  "allOf": [
    {
      "not": {
        "field": "tags",
        "containsKey": "application"
      }
    },
    {
      "field": "type",
      "equals": "Microsoft.Storage/storageAccounts"
    }
  ]
},
```

### <a name="conditions"></a>Koşullar
Koşulu değerlendirir olup bir **alan** belirli kriterlere uyan. Desteklenen koşullar şunlardır:

* `"equals": "value"`
* `"like": "value"`
* `"match": "value"`
* `"contains": "value"`
* `"in": ["value1","value2"]`
* `"containsKey": "keyName"`
* `"exists": "bool"`

Kullanırken **gibi** koşulu, bir joker (*) değer sağlayabilir.

Kullanırken **eşleşen** koşul, sağlayın `#` bir basamak temsil etmek için `?` bir harf ve o gerçek karakteri temsil etmesi için başka bir karakter. Örnekler için bkz: [adları ve metin için kaynak ilkelerini uygulamak](resource-manager-policy-naming-convention.md).

### <a name="fields"></a>Alanları
Koşullar alanlar kullanılarak oluşturulur. Bir alan kaynağının durumu tanımlamak için kullanılan kaynak istek yükünde özelliklerini temsil eder.  

Aşağıdaki alanları desteklenir:

* `name`
* `kind`
* `type`
* `location`
* `tags`
* `tags.*` 
* özellik diğer adlar - bir listesi için bkz: [diğer adlar](#aliases).

### <a name="effect"></a>Etki
İlke etkili - üç tür destekler `deny`, `audit`, `append`, `AuditIfNotExists`, ve `DeployIfNotExists`. 

* **Reddetme** Denetim günlüğüne bir olay oluşturur ve isteği başarısız olur
* **Denetim** Denetim günlüğüne bir uyarı olayı oluşturur ama isteği başarısız değil
* **Append** alanları dizi tanımlanmış isteğe ekler 
* **AuditIfNotExists** -kaynak yoksa, denetimi etkinleştirme
* **DeployIfNotExists** -zaten yoksa, bir kaynak dağıtın. Şu anda bu etkiyi yalnızca yerleşik ilkeler aracılığıyla desteklenir.

İçin **sona**, aşağıdaki ayrıntıları sağlamanız gerekir:

```json
"effect": "append",
"details": [
  {
    "field": "field name",
    "value": "value of the field"
  }
]
```

Değer bir dize veya bir JSON biçimi nesnesi olabilir. 

İle **AuditIfNotExists** ve **DeployIfNotExists**, bir alt kaynak varlığını değerlendirmek ve bu kaynak mevcut değil, bir kural uygulayın. Örneğin, bir Ağ İzleyicisi için tüm sanal ağları dağıtılır gerektirebilir.

Bir sanal makine uzantısı değil dağıtıldığında denetim bir örnek için bkz: [denetim VM uzantıları](https://github.com/Azure/azure-policy-samples/blob/master/samples/Compute/audit-vm-extension/azurepolicy.json).

## <a name="aliases"></a>Diğer adlar

Bir kaynak türü için belirli özelliklere erişmek için özellik diğer adlar kullanın. Diğer adlar hangi değerleri veya koşullara bir özelliği bir kaynak için izin verilen kısıtlamak etkinleştirin. Her bir diğer ad belirli kaynak türlerine yönelik farklı API sürümleri yollarında eşler. İlke değerlendirmesi sırasında ilke altyapısı bu API sürümü için özellik yolu alır.

**Microsoft.Cache/Redis**

| Diğer ad | Açıklama |
| ----- | ----------- |
| Microsoft.Cache/Redis/enableNonSslPort | Ayarlanmış olup olmadığını ssl olmayan Redis bağlantı noktası (6379) etkindir. |
| Microsoft.Cache/Redis/shardCount | Premium küme önbelleği üzerinde oluşturulacak parça sayısını ayarlayın.  |
| Microsoft.Cache/Redis/sku.capacity | Dağıtmak için Redis önbelleği boyutunu ayarlayın.  |
| Microsoft.Cache/Redis/sku.family | Kullanmak için SKU ailesi ayarlayın. |
| Microsoft.Cache/Redis/sku.name | Dağıtmak için Redis önbelleği türünü ayarlayın. |

**Microsoft.Cdn/profiles**

| Diğer ad | Açıklama |
| ----- | ----------- |
| Microsoft.CDN/profiles/sku.name | Fiyatlandırma katmanı adını ayarlayın. |

**Microsoft.Compute/disks**

| Diğer ad | Açıklama |
| ----- | ----------- |
| Microsoft.Compute/imageOffer | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü teklif ayarlayın. |
| Microsoft.Compute/imagePublisher | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü yayımcısına ayarlayın. |
| Microsoft.Compute/imageSku | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü SKU'su ayarlayın. |
| Microsoft.Compute/imageVersion | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü sürümünü ayarlayın. |


**Microsoft.Compute/virtualMachines**

| Diğer ad | Açıklama |
| ----- | ----------- |
| Microsoft.Compute/imageId | Sanal makine oluşturmak için kullanılan görüntü tanıtıcısı ayarlayın. |
| Microsoft.Compute/imageOffer | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü teklif ayarlayın. |
| Microsoft.Compute/imagePublisher | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü yayımcısına ayarlayın. |
| Microsoft.Compute/imageSku | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü SKU'su ayarlayın. |
| Microsoft.Compute/imageVersion | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü sürümünü ayarlayın. |
| Microsoft.Compute/licenseType | Görüntü veya disk içi lisanslı ayarlanır. Bu değer yalnızca Windows Server işletim sistemi içeren görüntüler için kullanılır.  |
| Microsoft.Compute/virtualMachines/imageOffer | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü teklif ayarlayın. |
| Microsoft.Compute/virtualMachines/imagePublisher | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü yayımcısına ayarlayın. |
| Microsoft.Compute/virtualMachines/imageSku | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü SKU'su ayarlayın. |
| Microsoft.Compute/virtualMachines/imageVersion | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü sürümünü ayarlayın. |
| Microsoft.Compute/virtualMachines/osDisk.Uri | Vhd URI ayarlayın. |
| Microsoft.Compute/virtualMachines/sku.name | Sanal makine boyutunu ayarlayın. |

**Microsoft.Compute/virtualMachines/extensions**

| Diğer ad | Açıklama |
| ----- | ----------- |
| Microsoft.Compute/virtualMachines/extensions/publisher | Uzantının yayımcının adını ayarlayın. |
| Microsoft.Compute/virtualMachines/extensions/type | Uzantı türü olarak ayarlayın. |
| Microsoft.Compute/virtualMachines/extensions/typeHandlerVersion | Uzantı sürümü ayarlayın. |

**Microsoft.Compute/virtualMachineScaleSets**

| Diğer ad | Açıklama |
| ----- | ----------- |
| Microsoft.Compute/imageId | Sanal makine oluşturmak için kullanılan görüntü tanıtıcısı ayarlayın. |
| Microsoft.Compute/imageOffer | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü teklif ayarlayın. |
| Microsoft.Compute/imagePublisher | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü yayımcısına ayarlayın. |
| Microsoft.Compute/imageSku | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü SKU'su ayarlayın. |
| Microsoft.Compute/imageVersion | Platform görüntü veya sanal makine oluşturmak için kullanılan Market görüntüsü sürümünü ayarlayın. |
| Microsoft.Compute/licenseType | Görüntü veya disk içi lisanslı ayarlanır. Bu değer yalnızca Windows Server işletim sistemi içeren görüntüler için kullanılır. |
| Microsoft.Compute/VirtualMachineScaleSets/computerNamePrefix | Tüm sanal makineler için bilgisayar adı öneki ölçek kümesinde ayarlayın. |
| Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl | Kullanıcı görüntüsü blob URI'si ayarlayın. |
| Microsoft.Compute/VirtualMachineScaleSets/osdisk.vhdContainers | Ölçek kümesi için işletim sistemi disklerini depolamak için kullanılan kapsayıcı URL'lerini ayarlayın. |
| Microsoft.Compute/VirtualMachineScaleSets/sku.name | Ölçek kümesindeki sanal makinelerin boyutunu ayarlayın. |
| Microsoft.Compute/VirtualMachineScaleSets/sku.tier | Sanal makine ölçek kümesindeki kümesi. |
  
**Microsoft.Network/applicationGateways**

| Diğer ad | Açıklama |
| ----- | ----------- |
| Microsoft.Network/applicationGateways/sku.name | Ağ geçidi boyutunu ayarlayın. |

**Microsoft.Network/virtualNetworkGateways**

| Diğer ad | Açıklama |
| ----- | ----------- |
| Microsoft.Network/virtualNetworkGateways/gatewayType | Bu sanal ağ geçidi türünü ayarlayın. |
| Microsoft.Network/virtualNetworkGateways/sku.name | Ağ geçidi SKU adına ayarlayın. |

**Microsoft.Sql/servers**

| Diğer ad | Açıklama |
| ----- | ----------- |
| Microsoft.Sql/servers/version | Sunucu sürümü ayarlayın. |

**Microsoft.Sql/databases**

| Diğer ad | Açıklama |
| ----- | ----------- |
| Microsoft.Sql/servers/databases/edition | Veritabanı sürümünü ayarlayın. |
| Microsoft.Sql/servers/databases/elasticPoolName | Esnek havuz veritabanı adını ayarlayın. |
| Microsoft.Sql/servers/databases/requestedServiceObjectiveId | Yapılandırılan hizmet düzeyi hedefi kimliği veritabanının ayarlayın. |
| Microsoft.Sql/servers/databases/requestedServiceObjectiveName | Yapılandırılan hizmet düzeyi hedefi veritabanının adını ayarlayın.  |

**Microsoft.Sql/elasticpools**

| Diğer ad | Açıklama |
| ----- | ----------- |
| sunucuları/elasticpools | Microsoft.Sql/servers/elasticPools/dtu | Veritabanı esnek havuz için toplam paylaşılan DTU ayarlayın. |
| sunucuları/elasticpools | Microsoft.Sql/servers/elasticPools/edition | Esnek havuz sürümünü ayarlayın. |

**Microsoft.Storage/storageAccounts**

| Diğer ad | Açıklama |
| ----- | ----------- |
| Microsoft.Storage/storageAccounts/accessTier | Faturalama için kullanılan erişim katmanı ayarlayın. |
| Microsoft.Storage/storageAccounts/accountType | SKU adına ayarlayın. |
| Microsoft.Storage/storageAccounts/enableBlobEncryption | Blob depolama hizmetinde depolanan gibi hizmet verileri şifreler gerekip gerekmediğini belirleyin. |
| Microsoft.Storage/storageAccounts/enableFileEncryption | Dosya depolama hizmetinde depolanan gibi hizmet verileri şifreler gerekip gerekmediğini belirleyin. |
| Microsoft.Storage/storageAccounts/sku.name | SKU adına ayarlayın. |
| Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly | Yalnızca depolama hizmeti https trafiğine izin verecek şekilde ayarlayın. |

## <a name="policy-sets"></a>İlke ayarlar

İlke kümeleri, birkaç ilgili ilke tanımları gruplamanıza olanak sağlar. Grup için tek bir öğe olarak çalışmak için ilke kümesi atama ve yönetimini basitleştirir. Örneğin, tek bir ilke kümesi içindeki tüm ilgili etiketleme ilkeler gruplandırabilirsiniz. Tek tek her ilke atama yerine, ilke kümesini Uygula.
 
Aşağıdaki örnekte iki etiket (costCenter ve productName) işlemek için ayarlanmış bir ilkesinin nasıl oluşturulacağını gösterir. Varsayılan etiket değeri uygulama ve etiket değeri zorlama için iki yerleşik ilkeleri kullanır. İlke kümesi, iki parametre, costCenterValue ve yeniden kullanılırlığı productNameValue bildirir. Birden çok kez farklı parametrelerle iki yerleşik ilke tanımları başvurur. Her parametre için tagName için gösterildiği gibi ya da sabit bir değer sağlayabilir veya tagValue için gösterildiği gibi bir parametre ilkesinden ayarlayın.

```json
{
    "properties": {
        "displayName": "Billing Tags Policy",
        "policyType": "Custom",
        "description": "Specify cost Center tag and product name tag",
        "parameters": {
            "costCenterValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for Cost Center tag"
                }
            },
            "productNameValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for product Name tag"
                }
            }
        },
        "policyDefinitions": [
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            }
        ]
    },
    "id": "/subscriptions/<subscription-id>/providers/Microsoft.Authorization/policySetDefinitions/billingTagsPolicy",
    "type": "Microsoft.Authorization/policySetDefinitions",
    "name": "billingTagsPolicy"
}
```

Kümesiyle ilke Ekle **yeni AzureRMPolicySetDefinition** PowerShell komutu.

REST işlemleri için **2017-06-01-Önizleme** API sürümü, aşağıdaki örnekte gösterildiği gibi:

```
PUT /subscriptions/<subId>/providers/Microsoft.Authorization/policySetDefinitions/billingTagsPolicySet?api-version=2017-06-01-preview
```

## <a name="next-steps"></a>Sonraki adımlar
* İlke kuralı tanımladıktan sonra bir kapsama atayın. Portal üzerinden ilkeler atamak için bkz: [atamak ve kaynak ilkelerini yönetmek için kullanım Azure portal](resource-manager-policy-portal.md). REST API'si, PowerShell veya Azure CLI aracılığıyla ilkeleri atamak için bkz: [atayın ve komut dosyası aracılığıyla ilkelerini yönetme](resource-manager-policy-create-assign.md).
* Örneğin bkz: ilkeleri, [Azure kaynak İlkesi GitHub deposunu](https://github.com/Azure/azure-policy-samples).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](resource-manager-subscription-governance.md).
* İlke şema yayımlanma [http://schema.management.azure.com/schemas/2016-12-01/policyDefinition.json](http://schema.management.azure.com/schemas/2016-12-01/policyDefinition.json). 

