---
title: Windows sanal makineleri Azure üzerinde ilkeleri ile güvenlik zorlama | Microsoft Docs
description: Bir Azure Kaynak Yöneticisi'ni Windows sanal makine için bir ilke uygulama
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
ms.openlocfilehash: b6a42e1a0b0256a6b19220958f98940764273a2d
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114393"
---
# <a name="apply-policies-to-windows-vms-with-azure-resource-manager"></a>Azure Resource Manager ile Windows sanal makineleri için geçerlidir
İlkeleri kullanarak, bir kuruluşun çeşitli kuralları ve kuruluş genelinde kuralları zorunlu kılabilir. İstenen davranışı zorlama kuruluşun başarısı için katkıda bulunan sırasında risk azaltılmasına yardımcı olur. Bu makalede, kuruluşunuzun sanal makineler için istenen davranışı tanımlamak için Azure Resource Manager ilkelerini nasıl kullanabileceğinizi açıklar.

İlkeleri giriş için bkz: [Azure ilke nedir?](../../azure-policy/azure-policy-introduction.md).

## <a name="permitted-virtual-machines"></a>İzin verilen sanal makineler
Sanal makineler, kuruluşunuz için bir uygulama ile uyumlu olduğundan emin olmak için izin verilen işletim sistemleri sınırlandırabilirsiniz. Aşağıdaki ilke örnekte, yalnızca Windows Server 2012 R2 Datacenter oluşturulması için sanal makineleri izin ver:

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

Herhangi bir Windows Server Datacenter görüntü izin vermek için önceki ilkeyi değiştirmek için joker kullanın:

```json
{
  "field": "Microsoft.Compute/imageSku",
  "like": "*Datacenter"
}
```

Herhangi bir Windows Server 2012 R2 Datacenter veya daha yüksek görüntü izin vermek için önceki ilkeyi değiştirmek için herhangi kullanın:

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


## <a name="azure-hybrid-use-benefit"></a>Azure Hibrit Kullanım Teklifi

Bir şirket içi lisansı olduğunda, sanal makinelere lisans ücret kaydedebilirsiniz. Lisansına sahip olmadığınız durumlarda seçeneği yasaklamaz. Aşağıdaki ilke kullanım Azure karma kullanımı Avantajı (AHUB) engelliyor:

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
* (Yukarıdaki örneklerde gösterildiği gibi) bir ilke kuralı tanımladıktan sonra ilke tanımı oluşturun ve bir kapsama atamanız gerekir. Kapsamı bir abonelik, kaynak grubu veya kaynak olabilir. İlkeler atamak için bkz: [atamak ve kaynak ilkelerini yönetmek için kullanım Azure portal](../../azure-policy/assign-policy-definition.md), [kullanım ilkeleri atamak için PowerShell](../../azure-policy/assign-policy-definition-ps.md), veya [ilkeler atamak için kullan Azure CLI](../../azure-policy/assign-policy-definition-cli.md).
* Kaynak ilkelerini giriş için bkz: [Azure ilke nedir?](../../azure-policy/azure-policy-introduction.md).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](/azure/architecture/cloud-adoption-guide/subscription-governance).