---
title: Azure kavramsal tasarımlar işlevleri
description: Azure şemaları tanımları ve atamaları ile kullanmak için işlevleri açıklar.
author: DCtheGeek
ms.author: dacoulte
ms.date: 04/15/2019
ms.topic: reference
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: dc72113a8f5ed978d64d35c43e94dc9e19e4cdb1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65209425"
---
# <a name="functions-for-use-with-azure-blueprints"></a>İşlevler Azure şemaları ile kullanmak için

Azure şemalar şema tanımını daha dinamik yapmadan işlevler sağlar. Bu işlevler, şema tanımları ile kullanmak içindir ve blueprint yapıtları. Resource Manager şablonu yapıt tam bir blueprint parametresi aracılığıyla dinamik değer alma yanı sıra, Resource Manager işlevleri destekler.

Aşağıdaki işlevler desteklenir:

- [artifacts](#artifacts)
- [concat](#concat)
- [parameters](#parameters)
- [resourceGroup](#resourcegroup)
- [resourceGroups](#resourcegroups)
- [subscription](#subscription)

## <a name="artifacts"></a>artifacts

`artifacts(artifactName)`

Bir nesnenin özellikleri ile blueprint yapıtları çıkışlarının doldurulup döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| artifactName |Evet |string |Blueprint yapıtı adı. |

### <a name="return-value"></a>Dönüş değeri

Çıkış özellikleri bir nesne. **Çıkarır** özellikleri başvurulan blueprint yapıtı türüne bağımlı. Tüm türleri, biçimi izleyin:

```json
{
  "outputs": {collectionOfOutputProperties}
}
```

#### <a name="policy-assignment-artifact"></a>İlke atama yapıtındaki

```json
{
    "outputs": {
        "policyAssignmentId": "{resourceId-of-policy-assignment}",
        "policyAssignmentName": "{name-of-policy-assignment}",
        "policyDefinitionId": "{resourceId-of-policy-definition}",
    }
}
```

#### <a name="resource-manager-template-artifact"></a>Resource Manager şablonu yapıt

**Çıkarır** döndürülen nesne özellikleri Resource Manager şablonunda tanımlanır ve dağıtımı tarafından döndürülen.

#### <a name="role-assignment-artifact"></a>Rol atama yapıtındaki

```json
{
    "outputs": {
        "roleAssignmentId": "{resourceId-of-role-assignment}",
        "roleDefinitionId": "{resourceId-of-role-definition}",
        "principalId": "{principalId-role-is-being-assigned-to}",
    }
}
```

### <a name="example"></a>Örnek

Bir Resource Manager şablonu yapıt kimliği ile _myTemplateArtifact_ özelliği içeren aşağıdaki örnek çıktı:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    ...
    "outputs": {
        "myArray": {
            "type": "array",
            "value": ["first", "second"]
        },
        "myString": {
            "type": "string",
            "value": "my string value"
        },
        "myObject": {
            "type": "object",
            "value": {
                "myProperty": "my value",
                "anotherProperty": true
            }
        }
    }
}
```

Veri alma ilişkin bazı örnekler _myTemplateArtifact_ örnek:

| İfade | Tür | Değer |
|:---|:---|:---|
|`[artifacts("myTemplateArtifact").outputs.myArray]` | Array | \["önce", "saniye"\] |
|`[artifacts("myTemplateArtifact").outputs.myArray[0]]` | String | "first" |
|`[artifacts("myTemplateArtifact").outputs.myString]` | String | "Benim dize değeri" |
|`[artifacts("myTemplateArtifact").outputs.myObject]` | Object | {"myproperty": "my value", "anotherProperty": true} |
|`[artifacts("myTemplateArtifact").outputs.myObject.myProperty]` | String | "Benim value" |
|`[artifacts("myTemplateArtifact").outputs.myObject.anotherProperty]` | Bool | True |

## <a name="concat"></a>concat

`concat(string1, string2, string3, ...)`

Birden çok dize değerleri birleştirir ve birleştirilmiş dizeyi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| string1 |Evet |string |Birleştirme için ilk değer. |
| Ek bağımsız değişkenler |Hayır |string |Sıralı birleştirme için ek değerler |

### <a name="return-value"></a>Dönüş değeri

Bitişik değer dizesi.

### <a name="remarks"></a>Açıklamalar

Azure şema işlevi yalnızca dizeler ile çalışır Azure Resource Manager şablonu işlevini farklıdır.

### <a name="example"></a>Örnek

`concat(parameters('organizationName'), '-vm')`

## <a name="parameters"></a>parametreler

`parameters(parameterName)`

Blueprint parametresinin değeri döndürür. Belirtilen parametre adı, şema tanımını veya blueprint yapıtları tanımlanmalıdır.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| parameterName |Evet |string |Döndürülecek parametrenin adı. |

### <a name="return-value"></a>Dönüş değeri

Belirtilen şema veya şema yapıt parametre değeri.

### <a name="remarks"></a>Açıklamalar

Azure şema işlevi yalnızca şema parametreleri ile çalışan Azure Resource Manager şablonu işlevden farklıdır.

### <a name="example"></a>Örnek

Parametresini tanımlama _principalIds_ şema tanımı içinde:

```json
{
    "type": "Microsoft.Blueprint/blueprints",
    "properties": {
        ...
        "parameters": {
            "principalIds": {
                "type": "array",
                "metadata": {
                    "displayName": "Principal IDs",
                    "description": "This is a blueprint parameter that any artifact can reference. We'll display these descriptions for you in the info bubble. Supply principal IDs for the users,groups, or service principals for the RBAC assignment.",
                    "strongType": "PrincipalId"
                }
            }
        },
        ...
    }
}
```

Ardından _principalIds_ bağımsız değişkeni olarak `parameters()` blueprint yapıtı içinde:

```json
{
    "type": "Microsoft.Blueprint/blueprints/artifacts",
    "kind": "roleAssignment",
    ...
    "properties": {
        "roleDefinitionId": "/providers/Microsoft.Authorization/roleDefinitions/8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
        "principalIds": "[parameters('principalIds')]",
        ...
    }
}
```

## <a name="resourcegroup"></a>resourceGroup

`resourceGroup()`

Geçerli kaynak grubunu temsil eden bir nesne döndürür.

### <a name="return-value"></a>Dönüş değeri

Döndürülen nesne aşağıdaki biçimdedir:

```json
{
  "name": "{resourceGroupName}",
  "location": "{resourceGroupLocation}",
}
```

### <a name="remarks"></a>Açıklamalar

Azure şema işlevi Azure Resource Manager şablonu işlevini farklıdır. `resourceGroup()` Abonelik düzeyi yapıt veya şema tanımı işlevi kullanılamaz. Yalnızca bir kaynak grubu yapıt parçası olan blueprint yapıtları içinde kullanılabilir.

Yaygın `resourceGroup()` işlev, aynı konumda kaynak grubu yapıt kaynakları oluşturmak için.

### <a name="example"></a>Örnek

Kaynak grubunun konumu kullanmak üzere veya şema tanımı ayarlayın veya başka bir yapıt için konumu olarak bir ataması sırasında şema Tanımınızda bir kaynak grubu yer tutucu nesne bildirme. Bu örnekte, _NetworkingPlaceholder_ kaynak grubu yer tutucu adıdır.

```json
{
    "type": "Microsoft.Blueprint/blueprints",
    "properties": {
        ...
        "resourceGroups": {
            "NetworkingPlaceholder": {
                "location": "eastus"
            }
        }
    }
}
```

Ardından `resourceGroup()` bağlamında, bir kaynak grubu yer tutucu nesne hedefleyen bir blueprint yapıtı işlevi. Bu örnekte, şablon yapıt içine dağıtılır _NetworkingPlaceholder_ kaynak grubu ve parametre sağlar _resourceLocation_ ile dinamik olarak doldurulmuş  _NetworkingPlaceholder_ şablona bir kaynak grubu konumu. Konumunu _NetworkingPlaceholder_ kaynak grubu şema tanımında statik olarak tanımlanan veya yüklenmiş atama sırasında dinamik olarak tanımlanır. Her iki durumda da, şablon yapıt bu bilgileri bir parametre olarak sağlanır ve kaynakları doğru konuma dağıtmak için kullanır.

```json
{
  "type": "Microsoft.Blueprint/blueprints/artifacts",
  "kind": "template",
  "properties": {
      "template": {
        ...
      },
      "resourceGroup": "NetworkingPlaceholder",
      ...
      "parameters": {
        "resourceLocation": {
          "value": "[resourceGroup().location]"
        }
      }
  }
}
```

## <a name="resourcegroups"></a>resourceGroups

`resourceGroups(placeholderName)`

Belirtilen kaynak grubu yapıt temsil eden bir nesne döndürür. Farklı `resourceGroup()`yapıtın bağlamı gerektirir, bu işlev, içeriği o kaynak grubunun içinde değil belirli bir kaynak grubu yer tutucu özelliklerini almak için kullanılır.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| placeholderName |Evet |string |Döndürülecek kaynak grubu yapıt yer tutucu adı. |

### <a name="return-value"></a>Dönüş değeri

Döndürülen nesne aşağıdaki biçimdedir:

```json
{
  "name": "{resourceGroupName}",
  "location": "{resourceGroupLocation}",
}
```

### <a name="example"></a>Örnek

Kaynak grubunun konumu kullanmak üzere veya şema tanımı ayarlayın veya başka bir yapıt için konumu olarak bir ataması sırasında şema Tanımınızda bir kaynak grubu yer tutucu nesne bildirme. Bu örnekte, _NetworkingPlaceholder_ kaynak grubu yer tutucu adıdır.

```json
{
    "type": "Microsoft.Blueprint/blueprints",
    "properties": {
        ...
        "resourceGroups": {
            "NetworkingPlaceholder": {
                "location": "eastus"
            }
        }
    }
}
```

Ardından `resourceGroups()` kaynak grubu yer tutucu nesnesine bir başvuru almak için herhangi bir blueprint yapıtı bağlamından işlevi. Bu örnekte, şablon yapı dışında dağıtılan _NetworkingPlaceholder_ kaynak grubu ve parametre sağlar _artifactLocation_ ile dinamik olarak doldurulmuş  _NetworkingPlaceholder_ şablona bir kaynak grubu konumu. Konumunu _NetworkingPlaceholder_ kaynak grubu şema tanımında statik olarak tanımlanan veya yüklenmiş atama sırasında dinamik olarak tanımlanır. Her iki durumda da, şablon yapıt bu bilgileri bir parametre olarak sağlanır ve kaynakları doğru konuma dağıtmak için kullanır.

```json
{
  "kind": "template",
  "properties": {
      "template": {
          ...
      },
      ...
      "parameters": {
        "artifactLocation": {
          "value": "[resourceGroups('NetworkingPlaceholder').location]"
        }
      }
  },
  "type": "Microsoft.Blueprint/blueprints/artifacts",
  "name": "myTemplate"
}
```

## <a name="subscription"></a>subscription

`subscription()`

Abonelik için geçerli şema atamasını ayrıntılarını döndürür.

### <a name="return-value"></a>Dönüş değeri

Döndürülen nesne aşağıdaki biçimdedir:

```json
{
    "id": "/subscriptions/{subscriptionId}",
    "subscriptionId": "{subscriptionId}",
    "tenantId": "{tenantId}",
    "displayName": "{name-of-subscription}"
}
```

### <a name="example"></a>Örnek

Aboneliğin görünen adını kullanın ve `concat()` işlevi parametre olarak geçirilen bir adlandırma kuralı oluşturmak için _resourceName_ şablon yapıya.

```json
{
  "kind": "template",
  "properties": {
      "template": {
          ...
      },
      ...
      "parameters": {
        "resourceName": {
          "value": "[concat(subscription().displayName, '-vm')]"
        }
      }
  },
  "type": "Microsoft.Blueprint/blueprints/artifacts",
  "name": "myTemplate"
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Şema yaşam döngüsü](../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../how-to/update-existing-assignments.md) öğrenin.
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin.