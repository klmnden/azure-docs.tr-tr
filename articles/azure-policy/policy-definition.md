---
title: Azure İlkesi tanım yapısı
description: Kaynak ilke tanımı zaman İlkesi uygulandığında ve hangi etkili olabilmesi için açıklayarak, kuruluşunuzda kaynaklar için kuralları oluşturmak için Azure ilke tarafından nasıl kullanıldığını açıklar.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/24/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: 2f756d65fa167b3812772088aec7232d08b04b9f
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36937341"
---
# <a name="azure-policy-definition-structure"></a>Azure İlkesi tanım yapısı

Azure ilke tarafından kullanılan kaynak ilke tanımı zaman İlkesi uygulandığında ve hangi etkili olabilmesi için açıklayarak, kuruluşunuzdaki kaynakların kuralları kurmanızı sağlar. Kuralları tanımlayarak, daha kolay kaynaklarınızı yönetmek ve maliyetleri denetleyebilirsiniz. Örneğin, sanal makineler yalnızca belirli türdeki izin verildiğini belirtebilirsiniz. Veya, tüm kaynakların belirli bir etikete sahip olması gerekir. İlkeler tüm alt kaynaklar tarafından devralınır. Bu nedenle, bir kaynak grubu için bir ilke uygulandığında, bu kaynak grubundaki tüm kaynaklar için geçerlidir.

Azure ilke tarafından kullanılan şema şurada bulunabilir: [https://schema.management.azure.com/schemas/2016-12-01/policyDefinition.json](https://schema.management.azure.com/schemas/2016-12-01/policyDefinition.json)

Bir ilke tanımı oluşturmak için JSON kullanın. İlke tanımı için öğeleri içerir:

- modu
- parametreler
- Görünen ad
- açıklama
- ilke kuralı
  - mantıksal değerlendirme
  - Etkisi

Örneğin, aşağıdaki JSON kaynakları dağıtıldığı sınırlar bir ilke gösterir:

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

Tüm Azure ilkesi örnekleri altındadır [ilkesi örnekleri](json-samples.md).

## <a name="mode"></a>Mod

**Modu** hangi kaynak türlerinin için bir ilke değerlendirilir belirler. Desteklenen modları şunlardır:

- `all`: kaynak grupları ve tüm kaynak türleri değerlendir
- `indexed`: yalnızca etiketlerini ve konumunu destekleyen kaynak türleri değerlendir

Ayarlamanızı öneririz **modu** için `all` çoğu durumda. Portal kullanımı oluşturulan tüm ilke tanımları `all` modu. PowerShell veya Azure CLI kullanıyorsanız, belirtebilirsiniz **modu** parametresi el ile. İlke tanımı içermiyorsa bir **modu** varsayılan değer `all` Azure PowerShell ve çok `null` Azure CLI'da olduğu eşdeğer `indexed`, için geriye dönük uyumluluk.

`indexed` ilkeleri oluşturma etiketler veya konumları zorunlu kılacak olduğunda kullanılmalıdır. Bu gerekli değildir ancak etiketleri ve konumları olarak uyumluluk sonuçlarını uyumlu olmayan göstermesini desteklemeyen kaynakları engeller. Bunun tek istisnası **kaynak grupları**. Konum veya bir kaynak grubu üzerinde etiketleri zorlamak için çalışıyorsunuz ilkeleri ayarlamalıdır **modu** için `all` ve özellikle hedef `Microsoft.Resources/subscriptions/resourceGroup` türü. Bir örnek için bkz: [kaynak grubu etiketleri zorunlu](scripts/enforce-tag-rg.md).

## <a name="parameters"></a>Parametreler

Parametreleri ilke tanımları sayısını azaltarak ilke yönetimini basitleştirmeye yardımcı olur. Formdaki – alanları gibi parametreleri düşünün `name`, `address`, `city`, `state`. Bu parametrelerin her zaman aynı kalır, ancak bunların değerleri değiştirmek tek tek formu dolduran üzerinde temel. Parametreler, ilkeleri oluştururken aynı şekilde çalışır. Bir ilke tanımı'nda parametreleri dahil ederek, farklı değerler kullanarak farklı senaryolar için bu ilkeyi yeniden kullanabilirsiniz.

Örneğin, kaynakları dağıtıldığı konumları sınırlamak bir kaynak özelliği için bir ilke tanımlayabilirsiniz. Bu durumda, ilke oluşturduğunuzda, aşağıdaki parametreleri bildirin:

```json
"parameters": {
    "allowedLocations": {
        "type": "array",
        "metadata": {
            "description": "The list of allowed locations for resources.",
            "displayName": "Allowed locations",
            "strongType": "location"
        }
    }
}
```

Bir parametrenin türü string veya dizi olabilir. Meta veri özelliği, Azure portalı gibi araçlar için kullanıcı dostu bilgileri görüntülemek için kullanılır.

Meta veri özelliği içinde kullandığınız **strongType** Azure portalı içinde seçeneklerini çoklu seçim listesi temin etmek için.  İzin verilen değerler için **strongType** şu anda içerir:

- `"location"`
- `"resourceTypes"`
- `"storageSkus"`
- `"vmSKUs"`
- `"existingResourceGroups"`
- `"omsWorkspace"`

İlke kuralı aşağıdaki söz dizimini parametrelerle başvurusu:

```json
{
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

## <a name="definition-location"></a>Tanım konumu

Bir girişim veya ilke tanımı oluşturulurken tanımı konumu belirtmeniz önemlidir.

Tanım konumu için Initiative veya ilke tanımı atanabilir kapsamı belirler. Konum, bir yönetim grubu veya abonelik belirtilebilir.

> [!NOTE]
> Bu ilke tanımı birden fazla abonelik uygulamayı planlıyorsanız, konumun Initiative ya da ilkeye atayacaktır abonelikleri içeren yönetim grubu olması gerekir.

## <a name="display-name-and-description"></a>Görünen ad ve açıklama

Kullanabileceğiniz **displayName** ve **açıklama** ilke tanımı tanımlamak ve kullanıldığında için bağlamı sağlar.

## <a name="policy-rule"></a>İlke kuralı

İlke kuralı oluşan **varsa** ve **sonra** engeller. İçinde **varsa** bloğu, ilke zaman zorlanır belirten bir veya daha fazla koşullarını tanımlayın. Mantıksal işleçler tam olarak bu senaryo için bir ilke tanımlamak için bu koşullar uygulayabilirsiniz.

İçinde **sonra** bloğu olur etkisi tanımladığınız zaman **varsa** koşullar yerine.

```json
{
    "if": {
        <condition> | <logical operator>
    },
    "then": {
        "effect": "deny | audit | append | auditIfNotExists | deployIfNotExists"
    }
}
```

### <a name="logical-operators"></a>Mantıksal işleçler

Desteklenen mantıksal işleçler şunlardır:

- `"not": {condition  or operator}`
- `"allOf": [{condition or operator},{condition or operator}]`
- `"anyOf": [{condition or operator},{condition or operator}]`

**Değil** sözdizimi koşul sonucunu tersine çevirir. **Tümü** sözdizimi (benzer şekilde mantıksal **ve** işlemi) tüm koşulların doğru olmasını gerektirir. **Herhangi** sözdizimi (benzer şekilde mantıksal **veya** işlemi) doğru olması için bir veya birden çok koşul gerektirir.

Mantıksal işleçler yerleştirebilirsiniz. Aşağıdaki örnekte gösterildiği bir **değil** içinde iç içe işlemi bir **tümü** işlemi.

```json
"if": {
    "allOf": [{
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

Bir koşulu değerlendirir olup bir **alan** belirli kriterlere uyan. Desteklenen koşullar şunlardır:

- `"equals": "value"`
- `"notEquals": "value"`
- `"like": "value"`
- `"notLike": "value"`
- `"match": "value"`
- `"notMatch": "value"`
- `"contains": "value"`
- `"notContains": "value"`
- `"in": ["value1","value2"]`
- `"notIn": ["value1","value2"]`
- `"containsKey": "keyName"`
- `"notContainsKey": "keyName"`
- `"exists": "bool"`

Kullanırken **gibi** ve **notLike** koşullar, bir joker karakter sağlayabilir `*` değeri.
Değer birden fazla joker karakter içermemesi gerekir `*`.

Kullanırken **eşleşen** ve **notMatch** koşullar sağlamak `#` bir basamak temsil etmek için `?` bir harf ve o gerçek karakteri temsil etmesi için başka bir karakter. Örnekler için bkz: [birden çok adı desenleri izin](scripts/allow-multiple-name-patterns.md).

### <a name="fields"></a>Alanlar

Koşullar alanlar kullanılarak oluşturulur. Bir alan kaynağının durumu tanımlamak için kullanılan kaynak istek yükünde özelliklerini temsil eder.  

Aşağıdaki alanları desteklenir:

- `name`
- `fullName`
  - Kaynak tam adını döndürür. Bir kaynağın tam adı, tüm üst kaynak adları (örneğin "myServer/Veritabanım") tarafından $a kaynak adıdır.
- `kind`
- `type`
- `location`
- `tags`
- `tags.tagName`
- `tags[tagName]`
  - Nokta içeren etiket adları bu köşeli ayraç sözdizimini destekler
- özellik diğer adlar - bir listesi için bkz: [diğer adlar](#aliases).

### <a name="alternative-accessors"></a>Alternatif erişimciler

**Alan** olan ilke kurallarında kullanılan birincil erişimcisi. Doğrudan değerlendiriliyor kaynak olup olmadığını denetler. Ancak, diğer bir erişimci İlkesi destekler **kaynak**.

```json
"source": "action",
"equals": "Microsoft.Compute/virtualMachines/write"
```

**Kaynak** yalnızca bir değeri destekleyen **eylem**. Eylem şu anda değerlendiriliyor isteğinin yetkilendirme eylem döndürür. Yetkilendirme Eylemler yetkilendirme bölümünde açığa [etkinlik günlüğü](../monitoring-and-diagnostics/monitoring-activity-log-schema.md).

İlke arka planda var olan kaynakların değerlendirirken, ayarlar **eylem** için bir `/write` kaynağın türünü yetkilendirme eylem.

### <a name="effect"></a>Etki

İlke etkili aşağıdaki türlerini destekler:

- **Reddetme**: Denetim günlüğüne bir olay oluşturur ve isteği başarısız olur
- **Denetim**: Denetim günlüğüne bir uyarı olayı oluşturur ama isteği başarısız değil
- **Append**: alanları dizi tanımlanmış isteğe ekler
- **AuditIfNotExists**: kaynak yoksa, denetim sağlar
- **DeployIfNotExists**: zaten yoksa, bir kaynak dağıtır. Şu anda bu etkiyi yalnızca yerleşik ilkeler aracılığıyla desteklenir.

İçin **sona**, aşağıdaki ayrıntıları sağlamanız gerekir:

```json
"effect": "append",
"details": [{
    "field": "field name",
    "value": "value of the field"
}]
```

Değer bir dize veya bir JSON biçimi nesnesi olabilir.

İle **AuditIfNotExists** ve **DeployIfNotExists** ilişkili bir kaynak varlığını değerlendirin ve bu kaynak mevcut değil, bir kural ve karşılık gelen bir efekt uygulayın. Örneğin, bir Ağ İzleyicisi için tüm sanal ağları dağıtılır gerektirebilir.
Bir sanal makine uzantısı değil dağıtıldığında denetim bir örnek için bkz: [uzantısı yoksa, Denetim](scripts/audit-ext-not-exist.md).

Değerlendirme, özellikleri ve örnekler, sırasını her efekti ilgili tam Ayrıntılar için bkz: [anlama İlkesi etkileri](policy-effects.md).

## <a name="aliases"></a>Diğer adlar

Bir kaynak türü için belirli özelliklere erişmek için özellik diğer adlar kullanın. Diğer adlar hangi değerleri veya koşullara bir özelliği bir kaynak için izin verilen kısıtlamak etkinleştirin. Her bir diğer ad belirli kaynak türlerine yönelik farklı API sürümleri yollarında eşler. İlke değerlendirmesi sırasında ilke altyapısı bu API sürümü için özellik yolu alır.

Diğer adlar listesini her zaman artıyor. Hangi diğer adların şu anda Azure ilke tarafından desteklenen bulmak için aşağıdaki yöntemlerden birini kullanın:

- Azure PowerShell

  ```azurepowershell-interactive
  # Login first with Connect-AzureRmAccount if not using Cloud Shell

  $azContext = Get-AzureRmContext
  $azProfile = [Microsoft.Azure.Commands.Common.Authentication.Abstractions.AzureRmProfileProvider]::Instance.Profile
  $profileClient = New-Object -TypeName Microsoft.Azure.Commands.ResourceManager.Common.RMProfileClient -ArgumentList ($azProfile)
  $token = $profileClient.AcquireAccessToken($azContext.Subscription.TenantId)
  $authHeader = @{
      'Content-Type'='application/json'
      'Authorization'='Bearer ' + $token.AccessToken
  }

  # Invoke the REST API
  $response = Invoke-RestMethod -Uri 'https://management.azure.com/providers/?api-version=2017-08-01&$expand=resourceTypes/aliases' -Method Get -Headers $authHeader

  # Create an Array List to hold discovered aliases
  $aliases = New-Object System.Collections.ArrayList

  foreach ($ns in $response.value) {
      foreach ($rT in $ns.resourceTypes) {
          if ($rT.aliases) {
              foreach ($obj in $rT.aliases) {
                  $alias = [PSCustomObject]@{
                      Namespace       = $ns.namespace
                      resourceType    = $rT.resourceType
                      alias           = $obj.name
                  }
                  $aliases.Add($alias) | Out-Null
              }
          }
      }
  }

  # Output the list, sort, and format. You can customize with Where-Object to limit as desired.
  $aliases | Sort-Object -Property Namespace, resourceType, alias | Format-Table
  ```

- Azure CLI

  ```azurecli-interactive
  # Login first with az login if not using Cloud Shell

  # Get Azure Policy aliases for a specific Namespace
  az provider show --namespace Microsoft.Automation --expand "resourceTypes/aliases" --query "resourceTypes[].aliases[].name"
  ```

- REST API / ARMClient

  ```http
  GET https://management.azure.com/providers/?api-version=2017-08-01&$expand=resourceTypes/aliases
  ```

## <a name="initiatives"></a>Girişimler

Girişimleri, bir grup için tek bir öğe olarak çalışmak için atamalarını ve yönetimini basitleştirmek için birkaç ilgili ilke tanımları gruplamanıza olanak sağlar. Örneğin, tek bir girişim içindeki tüm ilgili etiketleme ilke tanımları gruplandırabilirsiniz. Her ilke tek tek atamak yerine Initiative uygulayın.

Aşağıdaki örnekte nasıl iki etiket işlemek için bir girişimi oluşturulacağı gösterilmektedir: `costCenter` ve `productName`. Varsayılan etiket değeri uygulamak için iki yerleşik ilkeleri kullanır.

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
        "policyDefinitions": [{
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

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İlkesi örnekleri](json-samples.md) sayfasındaki diğer örnekleri inceleyin.
