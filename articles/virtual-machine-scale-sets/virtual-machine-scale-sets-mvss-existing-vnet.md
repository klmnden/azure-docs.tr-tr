---
title: "Bir Azure ölçek kümesi şablonda mevcut bir sanal ağa başvuran | Microsoft Docs"
description: "Bir sanal ağ mevcut bir Azure sanal makine ölçek kümesi şablona eklemeyi öğrenin"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: negat
ms.openlocfilehash: 28117d467b491704aed8d45e5eba42530579dfa2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="add-reference-to-an-existing-virtual-network-in-an-azure-scale-set-template"></a>Bir Azure ölçek kümesi şablonda mevcut bir sanal ağı başvuru ekleyin

Bu makalede nasıl değiştirileceğini gösterir [minimum uygun ölçek kümesi şablonu](./virtual-machine-scale-sets-mvss-start.md) içine yeni bir tane oluşturmak yerine var olan bir sanal ağı dağıtmak için.

## <a name="change-the-template-definition"></a>Şablon tanımını değiştirin

Bizim minimum uygun ölçek kümesi şablon görülebilir [burada](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), ve mevcut bir sanal ağı kümesini dağıtmak için bizim şablon görülebilir [burada](https://raw.githubusercontent.com/gatneil/mvss/existing-vnet/azuredeploy.json). Bu şablon oluşturmak için kullanılan fark inceleyelim (`git diff minimum-viable-scale-set existing-vnet`) tarafından parça parça:

İlk olarak, eklediğimiz bir `subnetId` parametresi. Bu dize, ölçeği sanal makinelerine dağıtmak için önceden oluşturulmuş alt ağı tanımlamak için Ayarla izin vererek ölçek kümesi yapılandırması içine geçirilir. Bu dize biçiminde olmalıdır: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`. Örneği için ölçek dağıtmak için mevcut sanal ağda adıyla ayarlayın `myvnet`, alt ağ `mysubnet`, kaynak grubu `myrg`ve abonelik `00000000-0000-0000-0000-000000000000`, subnetId olacaktır: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`.

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

Ardından, biz sanal ağ kaynak silebilirsiniz `resources` dizi varolan bir sanal ağı kullanıyor ve yeni bir tane dağıtmanız gerekmez.

```diff
   "variables": {},
   "resources": [
-    {
-      "type": "Microsoft.Network/virtualNetworks",
-      "name": "myVnet",
-      "location": "[resourceGroup().location]",
-      "apiVersion": "2016-12-01",
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

Şablon dağıtılmadan önce bu yüzden sanal ağa kümesini from dependsOn yan tümcesi belirtmenize gerek yoktur sanal ağ zaten mevcut. Bu nedenle, bu satırları sil:

```diff
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "location": "[resourceGroup().location]",
       "apiVersion": "2016-04-30-preview",
-      "dependsOn": [
-        "Microsoft.Network/virtualNetworks/myVnet"
-      ],
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
```

Son olarak, biz geçirin `subnetId` parametresi kullanıcı tarafından ayarlanan (kullanmak yerine `resourceId` ne minimum uygun ölçek şablon kümesi olduğu aynı dağıtımda bir vnet kimliğini almak için değil).

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
