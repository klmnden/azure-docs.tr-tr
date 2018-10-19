---
title: Azure İlkesi tanım yapısı
description: Kaynak ilke tanımı hangi etkili olması için zaman ilkelerin hiçbiri uygulanmaz ve açıklayarak, kuruluşunuzdaki kaynaklar için kuralları oluşturmak için Azure İlkesi tarafından nasıl kullanıldığını açıklar.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: f5906a93e92691cb6046fb04a9fd83f3484e17b8
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49427408"
---
# <a name="azure-policy-definition-structure"></a>Azure İlkesi tanım yapısı

Azure İlkesi tarafından kullanılan kaynak ilke tanımı hangi etkili olması için zaman ilkelerin hiçbiri uygulanmaz ve açıklayarak, kuruluşunuzdaki kaynaklar için kuralları kurmanızı sağlar. Kuralları tanımlayarak daha kolayca kaynaklarınızı yönetmek ve maliyetleri denetleyebilirsiniz. Örneğin, yalnızca belirli türlerdeki sanal makinelere izin verildiğini belirtebilirsiniz. Veya tüm kaynakların belirli bir etikete sahip gerektirebilir. İlkeleri, tüm alt kaynaklar tarafından devralınır. Bu nedenle, bir kaynak grubu için bir ilke uygulanırsa, bu kaynak grubundaki tüm kaynaklar için geçerlidir.

Azure İlkesi tarafından kullanılan şema burada bulunabilir: [https://schema.management.azure.com/schemas/2018-05-01/policyDefinition.json](https://schema.management.azure.com/schemas/2018-05-01/policyDefinition.json)

Bir ilke tanımı oluşturmak için JSON kullanın. İlke tanımı yönelik öğeleri içerir:

- mode
- parametreler
- Görünen ad
- açıklama
- ilke kuralı
  - mantıksal değerlendirme
  - Etkin

Örneğin, aşağıdaki JSON kaynakların dağıtıldığı sınırlayan ilke gösterilmektedir:

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

Tüm Azure ilkesi örnekleri altındadır [ilkesi örnekleri](../samples/index.md).

## <a name="mode"></a>Mod

**Modu** hangi kaynak türlerinin bir ilke değerlendirileceğini belirler. İlişkin desteklenen modları şunlardır:

- `all`: kaynak grupları ve tüm kaynak türleri değerlendirin
- `indexed`: etiketlerini ve konumunu destekleyen kaynak türleri yalnızca değerlendirme

Ayarlamanızı öneririz **modu** için `all` çoğu durumda. Portalı kullanarak oluşturulan tüm ilke tanımlarını `all` modu. PowerShell veya Azure CLI kullanıyorsanız, belirtebilmeniz için **modu** parametresi el ile. İlke tanımı içermiyorsa bir **modu** varsayılan değer `all` Azure PowerShell ve çok `null` Azure CLI'da olduğu eşdeğer `indexed`, için geriye dönük uyumluluk.

`indexed` ilkeleri oluşturma etiketleri veya konumları zorlar olduğunda kullanılmalıdır. Bu gerekli değildir, ancak etiketleri ve konumları olarak uyumluluk sonuçları uyumlu olmayan gösteren gelen desteklemeyen kaynakları engeller. Bir özel durum **kaynak grupları**. Konum veya bir kaynak grubu etiketleri uygulamak çalıştığınız ilkeleri ayarlamalıdır **modu** için `all` ve özellikle hedef `Microsoft.Resources/subscriptions/resourceGroup` türü. Bir örnek için bkz. [kaynak grubu etiketleri zorunlu](../samples/enforce-tag-rg.md).

## <a name="parameters"></a>Parametreler

Parametreleri, ilke tanımlarının sayısını azaltarak ilke yönetiminizi basitleştirmeye yardımcı olur. Düşünme – form üzerinde alanları gibi parametrelerin `name`, `address`, `city`, `state`. Bu parametreleri her zaman aynı kalır, tek tek formu dolduran üzerinde tabanlı ancak bunların değerlerini değiştirin.
Parametreler, ilkeleri oluştururken aynı şekilde çalışır. İlke tanımında parametreler ekleyerek, farklı değerler kullanarak farklı senaryolar için bu ilkeyi yeniden kullanabilirsiniz.

> [!NOTE]
> Yalnızca parametre tanımı için bir ilke veya girişim tanımı ilke veya girişim ilk oluşturma sırasında yapılandırılabilir. Parametre tanımı daha sonra değiştirilemez.
> Bu, ilke veya girişim mevcut atamaları dolaylı olarak geçersiz yapılmasını önler.

Örneğin, kaynakların dağıtıldığı konumları sınırlamak bir kaynak özelliği için bir ilke tanımlayabilirsiniz. Bu durumda, ilkenizi oluşturduğunuzda aşağıdaki parametreleri bildirmek:

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

Bir parametrenin türü dize veya dizi olabilir. Meta veri özelliği, Azure portalı gibi araçları için kullanıcı dostu bilgileri görüntülemek için kullanılır.

Meta veri özelliği içinde kullanabileceğiniz **strongType** çoklu seçim listesini Azure portalındaki seçenekleri sağlamak için. İzin verilen değerler için **strongType** şu anda içerir:

- `"location"`
- `"resourceTypes"`
- `"storageSkus"`
- `"vmSKUs"`
- `"existingResourceGroups"`
- `"omsWorkspace"`

Aşağıdaki parametrelerle ilke kuralında başvuru `parameters` dağıtım değer işlev sözdizimi:

```json
{
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

## <a name="definition-location"></a>Tanım konumu

Bir girişim veya tanımını oluşturulurken tanım konumunu belirtmek önemlidir.

Tanım konumu için girişim veya tanımını atanabilir kapsamı belirler. Konum, bir yönetim grubu veya abonelik belirtilebilir.

> [!NOTE]
> Bu ilke tanımı için birden çok abonelik uygulamayı planlıyorsanız, konumu için girişim veya atar abonelikleri içeren yönetim grubu olması gerekir.

## <a name="display-name-and-description"></a>Görünen ad ve açıklama

Kullanabileceğiniz **displayName** ve **açıklama** ilke tanımını ve kullanıldığında için bağlam sağlar.

## <a name="policy-rule"></a>İlke kuralı

İlke kuralı oluşan **varsa** ve **ardından** engeller. İçinde **varsa** bloğu ne zaman ilkelerin hiçbiri uygulanmaz belirten bir veya daha fazla koşulları tanımlayın. Senaryo ilkesi için tam olarak tanımlamak için bu koşullar için mantıksal işleçler uygulayabilirsiniz.

İçinde **ardından** bloğu gerçekleşen etkisi tanımladığınız zaman **varsa** koşullar yerine.

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

**Değil** söz dizimi koşul sonucunu tersine çevirir. **Tümü** sözdizimi (benzer şekilde mantıksal **ve** işlemi) tüm koşulların true olmasını gerektirir. **Herhangi** sözdizimi (benzer şekilde mantıksal **veya** işlemi) bir veya daha fazla koşul true olmasını gerektirir.

Mantıksal işleçler iç içe yerleştirebilirsiniz. Aşağıdaki örnekte gösterildiği bir **değil** içinde iç içe işlem bir **tümü** işlemi.

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

Bir koşulu değerlendirir olup olmadığını bir **alan** belirli kriterlere uyan. Desteklenen koşullar şunlardır:

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

Kullanırken **gibi** ve **notLike** koşullar, joker karakter sağlayabilir `*` değerindeki değişikliği belirtir.
Değer, birden fazla joker karakter içermemelidir `*`.

Kullanırken **eşleşen** ve **notMatch** koşulları sağlayan `#` bir basamak gösterecek `?` bir harfi için `.` tüm karakterleri ve diğer herhangi bir karakterle eşleştirmek için Bu gerçek bir karakteri temsil eder. Örnekler için bkz [birden çok adı desenlerinin izin](../samples/allow-multiple-name-patterns.md).

### <a name="fields"></a>Alanlar

Koşullara alanları kullanılarak oluşturulur. Bir alan kaynak durumunu tanımlamak için kullanılan kaynak isteği yükü özelliklerinde temsil eder.

Aşağıdaki alanları desteklenir:

- `name`
- `fullName`
  - Kaynağın tam adını döndürür. Bir kaynağın tam adını, tüm üst kaynak adları (örneğin "myServer/Veritabanım") tarafından başına kaynak addır.
- `kind`
- `type`
- `location`
- `tags`
- `tags.<tagName>`
  - Burada **\<tagName\>** doğrulamak için bir koşul için etiket adıdır.
  - Örnek: `tags.CostCenter` burada **CostCenter** etiketin adı.
- `tags[<tagName>]`
  - Tane nokta içeren etiket adları bu parantez sözdizimini destekler.
  - Burada **\<tagName\>** doğrulamak için bir koşul için etiket adıdır.
  - Örnek: `tags[Acct.CostCenter]` burada **Acct.CostCenter** etiketin adı.
- özellik diğer adları - bir listesi için bkz [diğer adlar](#aliases).

### <a name="effect"></a>Etki

İlke etkisi aşağıdaki türlerini destekler:

- **Reddetme**: Denetim günlüğüne bir olay oluşturur ve istek başarısız olur
- **Denetim**: denetim günlüğünde uyarı olayı oluşturur, ancak istek başarısız olmaz
- **Append**: dizi alanları isteği ekler
- **AuditIfNotExists**: bir kaynak mevcut değilse denetim sağlar
- **Deployıfnotexists**: zaten yoksa, bir kaynak dağıtır.

İçin **ekleme**, aşağıdaki ayrıntıları sağlamanız gerekir:

```json
"effect": "append",
"details": [{
    "field": "field name",
    "value": "value of the field"
}]
```

Değer bir dize veya bir JSON biçimi nesnesi olabilir.

İle **AuditIfNotExists** ve **Deployıfnotexists** ilişkili bir kaynak varlığı değerlendirin ve bu kaynak mevcut değil, bir kural ve karşılık gelen bir efekt uygulayın. Örneğin, tüm sanal ağları için Ağ İzleyicisi dağıtıldığını gerektirebilir. Sanal makine uzantısı olmayan dağıtıldığında denetim örneği için bkz: [uzantı mevcut değilse denetim](../samples/audit-ext-not-exist.md).

Değerlendirme, özellikler ve örnekler de sırasını her etkisi hakkında tüm ayrıntılar için bkz. [anlama ilke etkileri](effects.md).

### <a name="policy-functions"></a>İlke işlevleri

Bir alt kümesini [Resource Manager şablonu işlevleri](../../../azure-resource-manager/resource-group-template-functions.md) bir ilke kuralı içinde kullanılabilir. Şu anda desteklenen işlevler şunlardır:

- [parametreler](../../../azure-resource-manager/resource-group-template-functions-deployment.md#parameters)
- [concat](../../../azure-resource-manager/resource-group-template-functions-array.md#concat)
- [resourceGroup](../../../azure-resource-manager/resource-group-template-functions-resource.md#resourcegroup)
- [aboneliği](../../../azure-resource-manager/resource-group-template-functions-resource.md#subscription)

Ayrıca, `field` işlevi ilke kuralları için kullanılabilir. İle kullanmak için öncelikle bu işlev, **AuditIfNotExists** ve **Deployıfnotexists** değerlendirilen kaynağı başvurusu alanları. Buna örnek olarak görülebilir [Deployıfnotexists örnek](effects.md#deployifnotexists-example).

#### <a name="policy-function-examples"></a>İlke işlevi örnekleri

Bu ilke kuralı örnekte `resourceGroup` almak için kaynak işlevi **adı** özelliği bir araya geldiğinde, `concat` oluşturmak için dizi ve nesne işlevi bir `like` başlatmak için kaynak adı zorlar durumu kaynak grubu adı ile.

```json
{
    "if": {
        "not": {
            "field": "name",
            "like": "[concat(resourceGroup().name,'*')]"
        }
    },
    "then": {
        "effect": "deny"
    }
}
```

Bu ilke kuralı örnekte `resourceGroup` almak için kaynak işlevi **etiketleri** özelliği dizi değerinin **CostCenter** bir kaynak grubuna etiket ve eklenecek **CostCenter**  yeni kaynak etiketi.

```json
{
    "if": {
        "field": "tags.CostCenter",
        "exists": "false"
    },
    "then": {
        "effect": "append",
        "details": [{
            "field": "tags.CostCenter",
            "value": "[resourceGroup().tags.CostCenter]"
        }]
    }
}
```

## <a name="aliases"></a>Diğer adlar

Bir kaynak türü için belirli özelliklerine erişmek için özelliği diğer adları kullanın. Diğer adlar, kaynak üzerinde bir özellik için hangi değerleri veya koşullara izin sınırlamak sağlar. Belirtilen kaynak türü için farklı bir API sürümlerinde yolları her diğer adın eşlenir. İlke değerlendirmesi sırasında ilke altyapısı bu API sürümü için özellik yolu alır.

Diğer adlar listesini her zaman artmaktadır. Hangi diğer adlar şu anda Azure İlkesi tarafından desteklenen bulmak için aşağıdaki yöntemlerden birini kullanın:

- Azure PowerShell

  ```azurepowershell-interactive
  # Login first with Connect-AzureRmAccount if not using Cloud Shell

  # Use Get-AzureRmPolicyAlias to list available providers
  Get-AzureRmPolicyAlias -ListAvailable

  # Use Get-AzureRmPolicyAlias to list aliases for a Namespace (such as Azure Automation -- Microsoft.Automation)
  Get-AzureRmPolicyAlias -NamespaceMatch 'automation'
  ```

- Azure CLI

  ```azurecli-interactive
  # Login first with az login if not using Cloud Shell

  # List namespaces
  az provider list --query [*].namespace

  # Get Azure Policy aliases for a specific Namespace (such as Azure Automation -- Microsoft.Automation)
  az provider show --namespace Microsoft.Automation --expand "resourceTypes/aliases" --query "resourceTypes[].aliases[].name"
  ```

- REST API / ARMClient

  ```http
  GET https://management.azure.com/providers/?api-version=2017-08-01&$expand=resourceTypes/aliases
  ```

### <a name="understanding-the--alias"></a>[*] Diğer anlama

Birkaç kullanılabilir diğer adlarına sahip bir 'normal' bir ad ve başka görüntülenen bir sürümüne sahip **[\*]** eklediniz. Örneğin:

- `Microsoft.Storage/storageAccounts/networkAcls.ipRules`
- `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]`

İlk örnek tüm dizinin değerlendirmek için kullanılan burada **[\*]** diğer dizinin her öğesi değerlendirir.

Örneğin bir ilke kuralı bakalım. Bu ilke olacak **Reddet** ipRules yapılandırılmış olan bir depolama hesabı ve **hiçbiri** ipRules "127.0.0.1" değerine sahip.

```json
"policyRule": {
    "if": {
        "allOf": [{
                "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules",
                "exists": "true"
            },
            {
                "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].value",
                "notEquals": "127.0.0.1"
            }
        ]
    },
    "then": {
        "effect": "deny",
    }
}
```

**İpRules** dizi şu şekildedir örneğin:

```json
"ipRules": [{
        "value": "127.0.0.1",
        "action": "Allow"
    },
    {
        "value": "192.168.1.1",
        "action": "Allow"
    }
]
```

Bu örnek nasıl işleneceğini aşağıda verilmiştir:

- `networkAcls.ipRules` -Dizisi null olmayan olup olmadığını denetleyin. Değerlendirme devam eder, true olarak değerlendirilir.

  ```json
  {
    "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules",
    "exists": "true"
  }
  ```

- `networkAcls.ipRules[*].value` -Her denetler _değer_ özelliğinde **ipRules** dizisi.

  ```json
  {
    "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].value",
    "notEquals": "127.0.0.1"
  }
  ```

  - Her öğe bir dizi olarak işlenir.

    - "127.0.0.1"! = "127.0.0.1" false değerlendirir.
    - "127.0.0.1"! = "192.168.1.1", true değerlendirilir.
    - En az bir _değer_ özelliğinde **ipRules** dizi değerlendirme sona erecek yanlış değerlendirilir.

Koşul false olarak değerlendirildi **Reddet** etkisi olmayan tetiklenir.

## <a name="initiatives"></a>Girişimler

Girişim atamaları ve yönetim grubu tek bir öğe olarak çalışmak için basitleştirmek için çeşitli ilgili ilke tanımlarını olanak sağlar. Örneğin, tek bir girişim etiketleme ilgili tüm ilke tanımlarını gruplandırabilirsiniz. Tek tek her ilke atamak yerine, girişim uygulayın.

Aşağıdaki örnek iki etiketi işlemeye yönelik bir girişim oluşturma işlemini gösterir: `costCenter` ve `productName`. Varsayılan etiket değeri uygulamak için iki yerleşik ilkeleri kullanır.

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

- Gözden geçirme örneklere [Azure ilkesi örnekleri](../samples/index.md)
- Gözden geçirme [ilke etkilerini anlama](effects.md)
- Anlamak için nasıl [programlı olarak ilkeler oluşturma](../how-to/programmatically-create.md)
- Bilgi edinmek için nasıl [uyumluluk verilerini al](../how-to/getting-compliance-data.md)
- Bulma nasıl [uyumlu olmayan kaynakları Düzelt](../how-to/remediate-resources.md)
- [Kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../../management-groups/overview.md) bölümünde yönetim gruplarını gözden geçirebilirsiniz
