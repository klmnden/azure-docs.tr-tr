---
title: Dağıtım sırası sırası anlama
description: Her aşamanın ayrıntılarını ve şema tanımını geçtiği yaşam döngüsü hakkında bilgi edinin.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/25/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 8451b858717e1a3e66214f66db624ee41f6da375
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58434815"
---
# <a name="understand-the-deployment-sequence-in-azure-blueprints"></a>Azure şemaları, dağıtım sırası anlama

Blueprint kullandığı bir **sıralama sipariş** şema tanımını atama işleme sırasında kaynak oluşturma sırasını belirlemek için. Bu makalede aşağıdaki kavramları açıklar:

- Kullanılan varsayılan sıralama düzeni
- Sırasını özelleştirme
- Özelleştirilmiş sırasını nasıl işlenir

Kendi değerlerinizle değiştirmeniz gereken JSON örneklerde değişkenleri vardır:

- `{YourMG}` - Yönetim grubunuzun adıyla değiştirin

## <a name="default-sequencing-order"></a>Varsayılan sıralama düzeni

Yönergesiyse null ya da hiçbir yönergesi yapıtları dağıtmanız sipariş için şema tanımını içeren aşağıdaki sırayla kullanılır:

- Abonelik düzeyi **rol ataması** yapıtları yapıt adına göre sıralanmış
- Abonelik düzeyi **ilke ataması** yapıtları yapıt adına göre sıralanmış
- Abonelik düzeyi **Azure Resource Manager şablonu** yapıtları yapıt adına göre sıralanmış
- **Kaynak grubu** (alt yapıtları dahil) yapıtları yer tutucu adına göre sıralanmış

Her **kaynak grubu** yapıt dizisi sırayla yapıtlar için bu kaynak grubu içinde oluşturulması için kullanılır:

- Kaynak grubu alt **rol ataması** yapıtları yapıt adına göre sıralanmış
- Kaynak grubu alt **ilke ataması** yapıtları yapıt adına göre sıralanmış
- Kaynak grubu alt **Azure Resource Manager şablonu** yapıtları yapıt adına göre sıralanmış

## <a name="customizing-the-sequencing-order"></a>Sıralama sırasını özelleştirme

Büyük şema tanımları oluştururken, belirli bir sırayla oluşturulacak kaynakları için gerekli olabilir. Bu senaryoda en yaygın kullanım desenini, çeşitli Azure Resource Manager şablonları bir şema tanımını içeren andır. Blueprint tanımlanması için sıralama sırasını sağlayarak bu düzen işler.

Sıralama tanımlayarak gerçekleştirilir bir `dependsOn` JSON özelliği. Şema tanımını kaynak grupları ve yapıt nesneleri için bu özelliği destekler. `dependsOn` bir dizeyi belirli yapıt oluşturulmadan önce oluşturulması gereken yapıt adları dizisidir.

### <a name="example---ordered-resource-group"></a>Örnek - kaynak grubuna sıralı

Bu örnek şema tanımını bir özel sıralama düzeni için bir değer bildirerek tanımladığı bir kaynak grubuna sahip `dependsOn`, birlikte standart bir kaynak grubu. Bu durumda, yapı adlı **assignPolicyTags** önce işlenen **sıralı-rg** kaynak grubu.
**Standart-rg** varsayılan sıralama düzeni işlenir.

```json
{
    "properties": {
        "description": "Example blueprint with custom sequencing order",
        "resourceGroups": {
            "ordered-rg": {
                "dependsOn": [
                    "assignPolicyTags"
                ],
                "metadata": {
                    "description": "Resource Group that waits for 'assignPolicyTags' creation"
                }
            },
            "standard-rg": {
                "metadata": {
                    "description": "Resource Group that follows the standard sequence ordering"
                }
            }
        },
        "targetScope": "subscription"
    },
    "id": "/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/mySequencedBlueprint",
    "type": "Microsoft.Blueprint/blueprints",
    "name": "mySequencedBlueprint"
}
```

### <a name="example---artifact-with-custom-order"></a>Örnek - özel düzen yapıtı

Bu örnek, bir Azure Resource Manager şablonuna bağımlı bir ilke yapıtı içindir. Varsayılan sıralama ile bir ilke yapıtı önce Azure Resource Manager şablonu oluşturulması. Bu sıralama oluşturulması için Azure Resource Manager şablonu beklenecek ilke yapıtı sağlar.

```json
{
    "properties": {
        "displayName": "Assigns an identifying tag",
        "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
        "resourceGroup": "standard-rg",
        "dependsOn": [
            "customTemplate"
        ]
    },
    "kind": "policyAssignment",
    "id": "/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/mySequencedBlueprint/artifacts/assignPolicyTags",
    "type": "Microsoft.Blueprint/artifacts",
    "name": "assignPolicyTags"
}
```

### <a name="example---subscription-level-template-artifact-depending-on-a-resource-group"></a>Örnek - abonelik düzeyinde şablonu yapıt bir kaynak grubu bağlı

Bu örnek, bir kaynak grubu üzerinde bağımlı abonelik düzeyinde dağıtılan bir Resource Manager şablonu içindir. Varsayılan sıralama, abonelik düzeyinde yapıtları tüm kaynak grupları ve bu kaynak gruplarındaki alt yapıtları önce oluşturulması. Kaynak grubu, şema tanımını şöyle tanımlanır:

```json
"resourceGroups": {
    "wait-for-me": {
        "metadata": {
            "description": "Resource Group that is deployed prior to the subscription level template artifact"
        }
    }
}
```

Abonelik düzeyi şablon yapıt bağlı olarak **bekleme-için-bana** kaynak grubunu şu şekilde tanımlanır:

```json
{
    "properties": {
        "template": {
            ...
        },
        "parameters": {
            ...
        },
        "dependsOn": ["wait-for-me"],
        "displayName": "SubLevelTemplate",
        "description": ""
    },
    "kind": "template",
    "id": "/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/mySequencedBlueprint/artifacts/subtemplateWaitForRG",
    "type": "Microsoft.Blueprint/blueprints/artifacts",
    "name": "subtemplateWaitForRG"
}
```

## <a name="processing-the-customized-sequence"></a>Özelleştirilmiş sırası işleme

Oluşturma işlemi sırasında topolojik bir sıralama Blueprint yapıtları bağımlılık grafiği oluşturmak için kullanılır. Onay, her kaynak grupları ve Yapılar arasındaki bağımlılık düzeyini desteklenen emin olur.

Ardından bir yapıt bağımlılık varsayılan düzenini alter mıydı bildirirse, değişiklik yapılmaz. Bir abonelik düzeyi ilkesi olduğu kaynak grubu bir örnektir. Kaynak grubu 'standart-rg' alt rol atamasını olduğu kaynak grubu 'standart-rg' alt ilke ataması başka bir örnektir. Her iki durumda da `dependsOn` varsayılan değiştirmiş mıydı sıralama sırası ve herhangi bir değişiklik yapılacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Şema yaşam döngüsü](lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](parameters.md) kullanımını anlayın.
- [Şema kaynak kilitleme](resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../how-to/update-existing-assignments.md) öğrenin.
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin.