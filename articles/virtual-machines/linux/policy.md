---
title: Azure'daki Linux vm'lerinde ilkeleri ile güvenlik zorlama | Microsoft Docs
description: Bir Azure Resource Manager Linux sanal makinesi için bir ilke uygulama
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
ms.openlocfilehash: 8ad1bf371c5d5dbcbf3657ad69eace2003a8dda9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61473867"
---
# <a name="apply-policies-to-linux-vms-with-azure-resource-manager"></a>Azure Resource Manager ile Linux Vm'leri için ilkelerini uygula
İlkeleri kullanarak, bir kuruluşun çeşitli kurallar ve kuruluş genelinde kuralları zorunlu kılabilir. İstenen davranışı uygulanması, kuruluşun başarısı için katkıda bulunurken risk azaltmaya Yardım olabilir. Bu makalede, kuruluşunuzun sanal makineler için istediğiniz çalışma biçimini tanımlamak için Azure Resource Manager ilkeleri nasıl kullanabileceğinizi açıklar.

İlkeleri bir giriş için bkz [Azure İlkesi nedir?](../../governance/policy/overview.md).

## <a name="permitted-virtual-machines"></a>İzin verilen sanal makineler
Sanal makineler, kuruluşunuz için bir uygulama ile uyumlu olmasını sağlamak için izin verilen işletim sistemleri kısıtlayabilirsiniz. Aşağıdaki ilke örnekte, yalnızca Ubuntu 14.04.2-LTS oluşturulması için sanal makineler sağlar.

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

Joker karakterli bir Ubuntu LTS görüntüsüne izin vermek için önceki ilkesini değiştirmek için kullanın: 

```json
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*LTS"
}
```

İlke alanlar hakkında daha fazla bilgi için bkz: [ilke diğer adlar](../../governance/policy/concepts/definition-structure.md#aliases).

## <a name="managed-disks"></a>Yönetilen diskler

Yönetilen diskler kullanımını zorunlu kılmak için şu ilkeyi kullanın:

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

## <a name="images-for-virtual-machines"></a>Sanal makine görüntüleri

Güvenlik nedenleriyle, özel görüntüler yalnızca onaylanan ortamınıza dağıtılan gerektirebilir. Onaylanan görüntüleri içeren kaynak grubu ya da belirtebilir veya özel görüntüleri onaylandı.

Aşağıdaki örnek, görüntüleri bir onaylı kaynak grubundan gerektirir:

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

Aşağıdaki örnek, onaylanan kimliklerini belirtir:

```json
{
    "field": "Microsoft.Compute/imageId",
    "in": ["{imageId1}","{imageId2}"]
}
```

## <a name="virtual-machine-extensions"></a>Sanal makine uzantıları

Belirli türde bir uzantısı kullanımını yasaklayabilme isteyebilirsiniz. Örneğin, bir uzantı belirli özel sanal makine görüntüleri ile uyumlu olmayabilir. Aşağıdaki örnek, belirli bir uzantıya engellemek gösterilmektedir. Hangi uzantısı engellenip engellenmeyeceğini belirlemek için yayımcı ve türü kullanır.

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
* (Yukarıdaki örneklerde gösterildiği gibi) ilke kuralı tanımladıktan sonra ilke tanımı oluşturma ve bir kapsama atamak gerekir. Kapsam abonelik, kaynak grubu veya kaynak olabilir. İlkeleri atamak için bkz: [atamak ve kaynak ilkelerini yönetmek için Azure portalını kullanın](../../governance/policy/assign-policy-portal.md), [ilkeleri atamak için PowerShell kullanma](../../governance/policy/assign-policy-powershell.md), veya [ilkeleri atamak için Azure CLI'yı kullanmak](../../governance/policy/assign-policy-azurecli.md).
* Kaynak ilkeleri bir giriş için bkz [Azure İlkesi nedir?](../../governance/policy/overview.md).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](/azure/architecture/cloud-adoption-guide/subscription-governance).
