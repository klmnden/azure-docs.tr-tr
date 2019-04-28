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
origin.date: 06/27/2017
ms.date: 11/30/2018
ms.author: v-junlch
ms.openlocfilehash: 1dcb97a94bd5790edc2e40acf890bb47baec7a4b
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62108036"
---
# <a name="add-reference-to-an-existing-virtual-network-in-an-azure-scale-set-template"></a>Bir Azure ölçek kümesi şablonunuzda bir sanal ağınız için başvuru ekleyin

Bu makalede nasıl değiştirileceğini gösterir [en düşük uygun ölçek kümesi şablonunu](./virtual-machine-scale-sets-mvss-start.md) içine yeni bir tane oluşturmak yerine var olan bir sanal ağı dağıtmak için.

## <a name="change-the-template-definition"></a>Şablon tanımı değiştirme

En düşük uygun ölçek kümesi şablonunun görülebilir [burada](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), ve ölçek kümesini mevcut bir sanal ağa dağıtmak için şablon görülebilir [burada](https://raw.githubusercontent.com/gatneil/mvss/existing-vnet/azuredeploy.json). Bu şablon oluşturmak için kullanılan fark inceleyelim (`git diff minimum-viable-scale-set existing-vnet`) parça parça:

İlk olarak, ekleme bir `subnetId` parametresi. Bu dize, içine sanal makineleri dağıtmak için önceden oluşturulmuş alt ağı belirlerken ölçek sağlayan ölçek kümesi yapılandırması uygulamasına geçirilir. Bu dize biçiminde olmalıdır: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`. Örneğin, Ölçek kümesi dağıtmaktır ada sahip mevcut bir sanal ağına ayarlayın `myvnet`, alt ağ `mysubnet`, kaynak grubu `myrg`ve abonelik `00000000-0000-0000-0000-000000000000`, Subnetıd olacaktır: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`.

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

Şablon dağıtılmadan önce bir ölçek kümesindeki sanal ağa from dependsOn tümcesi belirtmeniz gerekmez. Bu nedenle sanal ağ zaten mevcut. Aşağıdaki satırları silin:

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

Son olarak, geçirin `subnetId` parametresi kullanıcı tarafından ayarlanan (kullanmak yerine `resourceId` aynı dağıtımdaki sanal ağ Kimliğini almak için hangi şablon en düşük uygun ölçek kümesi yapar).

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

<!-- Update_Description: update metedata properties -->