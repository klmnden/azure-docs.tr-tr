---
title: Bir Azure ölçek kümesi şablonunun var olan bir sanal ağda başvurusu | Microsoft Docs
description: Bir sanal ağ için mevcut bir Azure sanal makine ölçek kümesi şablonu eklemeyi öğrenin
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2019
ms.author: manayar
ms.openlocfilehash: 8b75b9898eb767866c0843594a82570cfb65d122
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64868948"
---
# <a name="add-reference-to-an-existing-virtual-network-in-an-azure-scale-set-template"></a>Bir Azure ölçek kümesi şablonunuzda bir sanal ağınız için başvuru ekleyin

Bu makalede nasıl değiştirileceğini gösterir [temel ölçek kümesi şablonunu](virtual-machine-scale-sets-mvss-start.md) içine yeni bir tane oluşturmak yerine var olan bir sanal ağı dağıtmak için.

## <a name="change-the-template-definition"></a>Şablon tanımı değiştirme

İçinde bir [önceki makalede](virtual-machine-scale-sets-mvss-start.md) temel ölçek kümesi şablonunun oluşturduğumuz. Şimdi daha önceki bu şablonu kullanın eder ve bir ölçek kümesi mevcut bir sanal ağa dağıtan bir şablon oluşturmak için değiştirin. 

İlk olarak, ekleme bir `subnetId` parametresi. Bu dize, içine sanal makineleri dağıtmak için önceden oluşturulmuş alt ağı belirlerken ölçek sağlayan ölçek kümesi yapılandırması uygulamasına geçirilir. Bu dize biçiminde olmalıdır: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`

Örneğin, Ölçek kümesi dağıtmaktır ada sahip mevcut bir sanal ağına ayarlayın `myvnet`, alt ağ `mysubnet`, kaynak grubu `myrg`ve abonelik `00000000-0000-0000-0000-000000000000`, Subnetıd olacaktır: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`.

```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "subnetId": {
+      "type": "string"
     }
   },
```

Ardından, sanal ağ kaynak grubundan silme `resources` dizi var olan bir sanal ağı kullanmayı ve yeni bir tane dağıtmanız gerekmez.

```diff
   "variables": {},
   "resources": [
-    {
-      "type": "Microsoft.Network/virtualNetworks",
-      "name": "myVnet",
-      "location": "[resourceGroup().location]",
-      "apiVersion": "2018-11-01",
-      "properties": {
-        "addressSpace": {
-          "addressPrefixes": [
-            "10.0.0.0/16"
-          ]
-        },
-        "subnets": [
-          {
-            "name": "mySubnet",
-            "properties": {
-              "addressPrefix": "10.0.0.0/16"
-            }
-          }
-        ]
-      }
-    },
```

Şablon dağıtılmadan önce bir ölçek kümesindeki sanal ağa from dependsOn tümcesi belirtmeniz gerekmez. Bu nedenle sanal ağ zaten mevcut. Aşağıdaki satırları silin:

```diff
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "location": "[resourceGroup().location]",
       "apiVersion": "2019-03-01",
-      "dependsOn": [
-        "Microsoft.Network/virtualNetworks/myVnet"
-      ],
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
```

Son olarak, geçirin `subnetId` parametresi kullanıcı tarafından ayarlanan (kullanmak yerine `resourceId` aynı dağıtımdaki sanal ağ Kimliğini almak için hangi şablonu temel uygun ölçek kümesi yapar).

```diff
                       "name": "myIpConfig",
                       "properties": {
                         "subnet": {
-                          "id": "[concat(resourceId('Microsoft.Network/virtualNetworks', 'myVnet'), '/subnets/mySubnet')]"
+                          "id": "[parameters('subnetId')]"
                         }
                       }
                     }
```




## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
