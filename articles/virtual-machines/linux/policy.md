---
title: Linux sanal makineleri Azure üzerinde ilkeleri ile güvenlik zorlama | Microsoft Docs
description: Bir Azure Resource Manager Linux sanal makine için bir ilke uygulama
services: virtual-machines-linux
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 06778ab4-f8ff-4eed-ae10-26a276fc3faa
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: singhkay
ms.openlocfilehash: fa6c95c3986a398bdb4593235116b305a80616fb
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34653802"
---
# <a name="apply-policies-to-linux-vms-with-azure-resource-manager"></a>Azure Resource Manager ile Linux VM'ler için geçerlidir
İlkeleri kullanarak, bir kuruluşun çeşitli kuralları ve kuruluş genelinde kuralları zorunlu kılabilir. İstenen davranışı zorlama kuruluşun başarısı için katkıda bulunan sırasında risk azaltılmasına yardımcı olur. Bu makalede, kuruluşunuzun sanal makineler için istenen davranışı tanımlamak için Azure Resource Manager ilkelerini nasıl kullanabileceğinizi açıklar.

İlkeleri giriş için bkz: [Azure ilke nedir?](../../azure-policy/azure-policy-introduction.md).

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

İlke alanlarını hakkında daha fazla bilgi için bkz: [İlkesi diğer adlar](../../azure-policy/policy-definition.md#aliases).

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
* (Yukarıdaki örneklerde gösterildiği gibi) bir ilke kuralı tanımladıktan sonra ilke tanımı oluşturun ve bir kapsama atamanız gerekir. Kapsamı bir abonelik, kaynak grubu veya kaynak olabilir. İlkeler atamak için bkz: [atamak ve kaynak ilkelerini yönetmek için kullanım Azure portal](../../azure-policy/assign-policy-definition.md), [kullanım ilkeleri atamak için PowerShell](../../azure-policy/assign-policy-definition-ps.md), veya [ilkeler atamak için kullan Azure CLI](../../azure-policy/assign-policy-definition-cli.md).
* Kaynak ilkelerini giriş için bkz: [Azure ilke nedir?](../../azure-policy/azure-policy-introduction.md).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](/azure/architecture/cloud-adoption-guide/subscription-governance).
