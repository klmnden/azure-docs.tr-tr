---
title: Dağıtım sırası sırası anlama
description: Her aşamanın ayrıntılarını ve bir şema geçtiği yaşam döngüsü hakkında bilgi edinin.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 11/12/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: bd12aabf0ca8f82261e6b3c677d7306ee46c4171
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53308626"
---
# <a name="understand-the-deployment-sequence-in-azure-blueprints"></a>Azure şemaları, dağıtım sırası anlama

Blueprint kullandığı bir **sıralama sipariş** blueprint ataması işlerken kaynak oluşturma sırasını belirlemek için. Bu makalede aşağıdaki kavramları açıklar:

- Kullanılan varsayılan sıralama düzeni
- Sırasını özelleştirme
- Özelleştirilmiş sırasını nasıl işlenir

Kendi değerlerinizle değiştirmeniz gereken JSON örneklerde değişkenleri vardır:

- `{YourMG}` - Yönetim grubunuzun adıyla değiştirin

## <a name="default-sequencing-order"></a>Varsayılan sıralama düzeni

Blueprint yapıtları dağıtmanız siparişi için hiçbir yönergesi içerir veya yönergesiyse null aşağıdaki sırayla kullanılır:

- Abonelik düzeyi **rol ataması** yapıtları yapıt adına göre sıralanmış
- Abonelik düzeyi **ilke ataması** yapıtları yapıt adına göre sıralanmış
- Abonelik düzeyi **Azure Resource Manager şablonu** yapıtları yapıt adına göre sıralanmış
- **Kaynak grubu** (alt yapıtları dahil) yapıtları yer tutucu adına göre sıralanmış

Her **kaynak grubu** yapıt dizisi sırayla yapıtlar için bu kaynak grubu içinde oluşturulması için kullanılır:

- Kaynak grubu alt **rol ataması** yapıtları yapıt adına göre sıralanmış
- Kaynak grubu alt **ilke ataması** yapıtları yapıt adına göre sıralanmış
- Kaynak grubu alt **Azure Resource Manager şablonu** yapıtları yapıt adına göre sıralanmış

## <a name="customizing-the-sequencing-order"></a>Sıralama sırasını özelleştirme

Büyük bir blueprint'i oluştururken, belirli bir sırayla oluşturulacak kaynakları için gerekli olabilir. Bu senaryoda en yaygın kullanım desenini, çeşitli Azure Resource Manager şablonları bir şema içerir andır. Blueprint tanımlanması için sıralama sırasını sağlayarak bu düzen işler.

Sıralama tanımlayarak gerçekleştirilir bir `dependsOn` JSON özelliği. Bu özellik yalnızca şema (için kaynak grupları) ve yapıt nesneleri destekler. `dependsOn` bir dizeyi belirli yapıt oluşturulmadan önce oluşturulması gereken yapıt adları dizisidir.

> [!NOTE]
> **Kaynak grubu** yapıtları Destek `dependsOn` özelliği hedefi olamaz, ancak bir `dependsOn` herhangi bir yapı türü tarafından.

### <a name="example---blueprint-with-ordered-resource-group"></a>Örnek - şema ile sıralanmış kaynak grubu

Bu örnek şema sahip bir özel sıralama düzeni için bir değer bildirerek tanımladığı bir kaynak grubu `dependsOn`, birlikte standart bir kaynak grubu. Bu durumda, yapı adlı **assignPolicyTags** önce işlenen **sıralı-rg** kaynak grubu. **Standart-rg** varsayılan sıralama düzeni işlenir.

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

## <a name="processing-the-customized-sequence"></a>Özelleştirilmiş sırası işleme

Oluşturma işlemi sırasında topolojik bir sıralama Blueprint yapıtları bağımlılık grafiği oluşturmak için kullanılır. Onay, her kaynak grupları ve Yapılar arasındaki bağımlılık düzeyini desteklenen emin olur.

Ardından bir yapıt bağımlılık varsayılan düzenini alter mıydı bildirirse, değişiklik yapılmaz. Bir abonelik düzeyi ilkesi olduğu kaynak grubu bir örnektir. Kaynak grubu 'standart-rg' alt rol atamasını olduğu kaynak grubu 'standart-rg' alt ilke ataması başka bir örnektir. Her iki durumda da `dependsOn` varsayılan değiştirmiş mıydı sıralama sırası ve herhangi bir değişiklik yapılacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Şema yaşam döngüsü](lifecycle.md) hakkında bilgi edinin
- [Statik ve dinamik parametreleri](parameters.md) kullanmayı anlayın
- [Şema kaynak kilitleme](resource-locking.md) özelliğini kullanmayı öğrenin
- [Var olan atamaları güncelleştirmeyi](../how-to/update-existing-assignments.md) öğrenin
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin