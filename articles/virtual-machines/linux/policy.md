---
title: "Linux sanal makineleri Azure üzerinde ilkeleri ile güvenlik zorlama | Microsoft Docs"
description: "Bir Azure Resource Manager Linux sanal makine için bir ilke uygulama"
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 06778ab4-f8ff-4eed-ae10-26a276fc3faa
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: singhkay
ms.openlocfilehash: 58eaab4fa03afc1e6a5e38bef691cce62a921ea9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="apply-policies-to-linux-vms-with-azure-resource-manager"></a>Azure Resource Manager ile Linux VM'ler için geçerlidir
İlkeleri kullanarak, bir kuruluşun çeşitli kuralları ve kuruluş genelinde kuralları zorunlu kılabilir. İstenen davranışı zorlama kuruluşun başarısı için katkıda bulunan sırasında risk azaltılmasına yardımcı olur. Bu makalede, kuruluşunuzun sanal makineler için istenen davranışı tanımlamak için Azure Resource Manager ilkelerini nasıl kullanabileceğinizi açıklar.

İlkeleri giriş için bkz: [kaynakları yönetmek ve erişimi denetlemek için ilke kullanma](../../azure-resource-manager/resource-manager-policy.md).

## <a name="permitted-virtual-machines"></a>İzin verilen sanal makineler
Sanal makineler, kuruluşunuz için bir uygulama ile uyumlu olduğundan emin olmak için izin verilen işletim sistemleri sınırlandırabilirsiniz. Aşağıdaki ilke örnekte yalnızca Ubuntu 14.04.2-LTS oluşturulması için sanal makineleri izin verin.

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "in": [
          "Microsoft.Compute/disks",
          "Microsoft.Compute/virtualMachines",
          "Microsoft.Compute/VirtualMachineScaleSets"
        ]
      },
      {
        "not": {
          "allOf": [
            {
              "field": "Microsoft.Compute/imagePublisher",
              "in": [
                "Canonical"
              ]
            },
            {
              "field": "Microsoft.Compute/imageOffer",
              "in": [
                "UbuntuServer"
              ]
            },
            {
              "field": "Microsoft.Compute/imageSku",
              "in": [
                "14.04.2-LTS"
              ]
            },
            {
              "field": "Microsoft.Compute/imageVersion",
              "in": [
                "latest"
              ]
            }
          ]
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

Herhangi bir Ubuntu LTS görüntü izin vermek için önceki ilkeyi değiştirmek için joker kullanın: 

```json
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*LTS"
}
```

İlke alanlarını hakkında daha fazla bilgi için bkz: [İlkesi diğer adlar](../../azure-resource-manager/resource-manager-policy.md#aliases).

## <a name="managed-disks"></a>Yönetilen diskler

Yönetilen disklerin gerektirmek için aşağıdaki ilke kullanın:

```json
{
  "if": {
    "anyOf": [
      {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/virtualMachines"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/osDisk.uri",
            "exists": true
          }
        ]
      },
      {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/VirtualMachineScaleSets"
          },
          {
            "anyOf": [
              {
                "field": "Microsoft.Compute/VirtualMachineScaleSets/osDisk.vhdContainers",
                "exists": true
              },
              {
                "field": "Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl",
                "exists": true
              }
            ]
          }
        ]
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="images-for-virtual-machines"></a>Sanal makineler için görüntüleri

Güvenlik nedenleriyle, yalnızca onaylanan özel resimler ortamınızda dağıtılan gerektirebilir. Özel resimler onaylanmış veya onaylanan görüntüleri içeren kaynak grubu ya da belirtebilirsiniz.

Aşağıdaki örnek, onaylanan kaynak grubu görüntülerden gerektirir:

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "in": [
                    "Microsoft.Compute/virtualMachines",
                    "Microsoft.Compute/VirtualMachineScaleSets"
                ]
            },
            {
                "not": {
                    "field": "Microsoft.Compute/imageId",
                    "contains": "resourceGroups/CustomImage"
                }
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
} 
```

Aşağıdaki örnek, onaylanan resim kimlikleri belirtir:

```json
{
    "field": "Microsoft.Compute/imageId",
    "in": ["{imageId1}","{imageId2}"]
}
```

## <a name="virtual-machine-extensions"></a>Sanal makine uzantıları

Belirli türde bir uzantıları kullanımını yasaklamaz isteyebilirsiniz. Örneğin, bir uzantı belirli özel bir sanal makine görüntüleri ile uyumlu olmayabilir. Aşağıdaki örnek, belirli bir uzantıya engelleme gösterilmektedir. Yayımcı ve türüne engellemek için hangi uzantısı belirlemek için kullanır.

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.Compute/virtualMachines/extensions"
            },
            {
                "field": "Microsoft.Compute/virtualMachines/extensions/publisher",
                "equals": "Microsoft.Compute"
            },
            {
                "field": "Microsoft.Compute/virtualMachines/extensions/type",
                "equals": "{extension-type}"

      }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```


## <a name="next-steps"></a>Sonraki adımlar
* (Yukarıdaki örneklerde gösterildiği gibi) bir ilke kuralı tanımladıktan sonra ilke tanımı oluşturun ve bir kapsama atamanız gerekir. Kapsamı bir abonelik, kaynak grubu veya kaynak olabilir. Portal üzerinden ilkeler atamak için bkz: [atamak ve kaynak ilkelerini yönetmek için kullanım Azure portal](../../azure-resource-manager/resource-manager-policy-portal.md). REST API'si, PowerShell veya Azure CLI aracılığıyla ilkeleri atamak için bkz: [atayın ve komut dosyası aracılığıyla ilkelerini yönetme](../../azure-resource-manager/resource-manager-policy-create-assign.md).
* Kaynak ilkelerini giriş için bkz: [kaynak ilkesine genel bakış](../../azure-resource-manager/resource-manager-policy.md).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](../../azure-resource-manager/resource-manager-subscription-governance.md).
