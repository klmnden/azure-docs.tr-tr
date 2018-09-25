---
title: Azure'da Windows VM'ler şirket ilkeleri ile güvenlik zorlama | Microsoft Docs
description: Bir Azure Resource Manager Windows sanal makinesi için bir ilke uygulama
services: virtual-machines-windows
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 0b71ba54-01db-43ad-9bca-8ab358ae141b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: kasing
ms.openlocfilehash: 42b62c819fd3d26c6ea944f968e0d5956a7f055e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46987467"
---
# <a name="apply-policies-to-windows-vms-with-azure-resource-manager"></a>Azure Resource Manager ile Windows Vm'leri için ilkelerini uygula
İlkeleri kullanarak, bir kuruluşun çeşitli kurallar ve kuruluş genelinde kuralları zorunlu kılabilir. İstenen davranışı uygulanması, kuruluşun başarısı için katkıda bulunurken risk azaltmaya Yardım olabilir. Bu makalede, kuruluşunuzun sanal makineler için istediğiniz çalışma biçimini tanımlamak için Azure Resource Manager ilkeleri nasıl kullanabileceğinizi açıklar.

İlkeleri bir giriş için bkz [Azure İlkesi nedir?](../../azure-policy/azure-policy-introduction.md).

## <a name="permitted-virtual-machines"></a>İzin verilen sanal makineler
Sanal makineler, kuruluşunuz için bir uygulama ile uyumlu olmasını sağlamak için izin verilen işletim sistemleri kısıtlayabilirsiniz. Aşağıdaki ilke örnekte, yalnızca Windows Server 2012 R2 Datacenter oluşturulması için sanal makineleri izin ver:

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
                "MicrosoftWindowsServer"
              ]
            },
            {
              "field": "Microsoft.Compute/imageOffer",
              "in": [
                "WindowsServer"
              ]
            },
            {
              "field": "Microsoft.Compute/imageSku",
              "in": [
                "2012-R2-Datacenter"
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

Tüm Windows Server Datacenter görüntüsü izin vermek için önceki ilkesini değiştirmek için bir joker kullanın:

```json
{
  "field": "Microsoft.Compute/imageSku",
  "like": "*Datacenter"
}
```

Herhangi bir Windows Server 2012 R2 Datacenter veya daha yüksek görüntü izin vermek için önceki ilkesini değiştirmek için kullanın:

```json
{
  "anyOf": [
    {
      "field": "Microsoft.Compute/imageSku",
      "like": "2012-R2-Datacenter*"
    },
    {
      "field": "Microsoft.Compute/imageSku",
      "like": "2016-Datacenter*"
    }
  ]
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


## <a name="azure-hybrid-use-benefit"></a>Azure Hibrit Kullanım Teklifi

Bir şirket içi lisans varsa, sanal makinelerinizde lisans ücreti kaydedebilirsiniz. Lisans olmadığında seçeneği yasaklayabilme. Aşağıdaki ilke, Azure karma kullanım Avantajı'nın (AHUB) kullanımı engelliyor:

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "in":[ "Microsoft.Compute/virtualMachines","Microsoft.Compute/VirtualMachineScaleSets"]
            },
            {
                "field": "Microsoft.Compute/licenseType",
                "exists": true
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
* (Yukarıdaki örneklerde gösterildiği gibi) ilke kuralı tanımladıktan sonra ilke tanımı oluşturma ve bir kapsama atamak gerekir. Kapsam abonelik, kaynak grubu veya kaynak olabilir. İlkeleri atamak için bkz: [atamak ve kaynak ilkelerini yönetmek için Azure portalını kullanın](../../azure-policy/assign-policy-definition.md), [ilkeleri atamak için PowerShell kullanma](../../azure-policy/assign-policy-definition-ps.md), veya [ilkeleri atamak için Azure CLI'yı kullanmak](../../azure-policy/assign-policy-definition-cli.md).
* Kaynak ilkeleri bir giriş için bkz [Azure İlkesi nedir?](../../azure-policy/azure-policy-introduction.md).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](/azure/architecture/cloud-adoption-guide/subscription-governance).