---
title: Azure şemaları, dağıtım sırası anlama
description: Her aşamanın ayrıntılarını ve bir şema geçtiği yaşam döngüsü hakkında bilgi edinin.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: c09fb26d8375e08281241aaed3f6f6e30acc755b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46955461"
---
# <a name="understand-the-deployment-sequence-in-azure-blueprints"></a>Azure şemaları, dağıtım sırası anlama

Blueprint kullandığı bir **sıralama sipariş** blueprint ataması işlerken kaynak oluşturma sırasını belirlemek için. Bu makalede kullanılan varsayılan sıralama düzeni, sırasını özelleştirmek nasıl ve özelleştirilmiş sırasını nasıl işlendiğini açıklar.

Kendi değerlerinizle değiştirmeniz gereken JSON örneklerde değişkenleri vardır:

- `{YourMG}` -Yönetim grubunuzun adıyla değiştirin

## <a name="default-sequencing-order"></a>Varsayılan sıralama düzeni

Blueprint yapıtları dağıtmanız siparişi için hiçbir yönergesi içerir veya yönergesiyse null aşağıdaki sırayla kullanılır:

- Abonelik düzeyi **rol ataması** yapıtları yapıt adına göre sıralanmış
- Abonelik düzeyi **ilke ataması** yapıtları yapıt adına göre sıralanmış
- Abonelik düzeyi **Azure Resource Manager şablonu** yapıtları yapıt adına göre sıralanmış
- **Kaynak grubu** (alt yapıtları dahil) yapıtları yer tutucu adına göre sıralanmış

Her **kaynak grubu** işlenir, yapıt dizisi sırayla yapıtlar için bu kaynak grubu içinde oluşturulması için kullanılır:

- Kaynak grubu alt **rol ataması** yapıtları yapıt adına göre sıralanmış
- Kaynak grubu alt **ilke ataması** yapıtları yapıt adına göre sıralanmış
- Kaynak grubu alt **Azure Resource Manager şablonu** yapıtları yapıt adına göre sıralanmış

## <a name="customizing-the-sequencing-order"></a>Sıralama sırasını özelleştirme

Büyük bir blueprint'i oluştururken, bir kaynak başka bir kaynağa ilişkisinde belirli bir sırayla oluşturulması için gerekli olabilir. Bu en yaygın kullanım desenini, birden çok Azure Resource Manager şablonları bir şema içerir andır. Blueprint işler bu tanımlanması için sıralama sırasını vererek.

Bu tanımlayarak gerçekleştirilir bir `dependsOn` JSON özelliği. Bu özellik yalnızca şema (için kaynak grupları) ve yapıt nesneleri destekler. `dependsOn` bir dizeyi belirli yapıt oluşturulmadan önce oluşturulması gereken yapıt adları dizisidir.

### <a name="example---blueprint-with-ordered-resource-group"></a>Örnek - şema ile sıralanmış kaynak grubu

Bu bir örnek bir özel sıralama düzeni için bir değer bildirerek tanımladığı bir kaynak grubu ile plandır `dependsOn`, birlikte standart bir kaynak grubu. Bu durumda, yapı adlı **assignPolicyTags** önce işlenen **sıralı-rg** kaynak grubu.
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

Bir Azure Resource Manager şablonuna bağlı bir örnek ilke yapıtı budur. Varsayılan sıralama ile bir ilke yapıtı önce Azure Resource Manager şablonu oluşturulması. Bu ilke yapıtı oluşturulması için Azure Resource Manager şablonu beklenecek sağlar.

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

Oluşturma işlemi sırasında topolojik bir sıralama blueprint yapıtlarını ve bağımlılık grafiği oluşturmak için kullanılır. Bu, birden çok kaynak grupları ve Yapılar arasındaki bağımlılık düzeyi desteklenen sağlar.

Bir bağımlılık şema veya varsayılan düzenini alter mıydı bir yapıt bildirirse, hiçbir değişiklik sıralama siparişin yapılır. Bu kaynak grubu 'standart-rg' alt rol atamasını olduğu abonelik düzeyi ilkesi veya kaynak grubu 'standart-rg' alt ilke ataması bağlı olduğu bir kaynak grubu örnekleridir. Her iki durumda da `dependsOn` varsayılan değiştirmiş mıydı sıralama sırası ve herhangi bir değişiklik yapılacak.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [blueprint yaşam döngüsü](lifecycle.md)
- Nasıl kullanılacağını anlamak [statik ve dinamik parametreleri](parameters.md)
- Öğrenin yapmak kullanım [blueprint kaynak kilitleme](resource-locking.md)
- Bilgi edinmek için nasıl [mevcut Atamaları Güncelleştir](../how-to/update-existing-assignments.md)
- İle bir blueprint ataması sırasında sorunları gidermek [genel sorun giderme](../troubleshoot/general.md)