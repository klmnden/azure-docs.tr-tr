---
title: "Yönetilen disk kullanmak üzere bir Azure Resource Manager ölçek kümesi şablonu Dönüştür | Microsoft Docs"
description: "Ölçek kümesi şablon yönetilen disk ölçek kümesi şablona dönüştürebilirsiniz."
keywords: "Sanal makine ölçekleme kümeleri"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: madhana
editor: tysonn
tags: azure-resource-manager
ms.assetid: bc8c377a-8c3f-45b8-8b2d-acc2d6d0b1e8
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/18/2017
ms.author: negat
ms.openlocfilehash: 2f5cb85703888c5056611d466f508547ee72e44b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="convert-a-scale-set-template-to-a-managed-disk-scale-set-template"></a>Ölçek kümesi şablon yönetilen disk ölçek kümesi şablona dönüştürme

Yönetilen disk kullanmayan bir ölçek kümesi oluşturmak için Resource Manager şablonu ile müşteriler, yönetilen disk kullanacak şekilde değiştirmek isteyebilirsiniz. Bu makalede, örnek olarak bir çekme isteğinden kullanarak bunu gösterilmektedir [Azure hızlı başlangıç şablonlarını](https://github.com/Azure/azure-quickstart-templates), örnek Resource Manager şablonları için topluluk odaklı bir depo. Tam çekme isteği burada görülebilir: [https://github.com/Azure/azure-quickstart-templates/pull/2998](https://github.com/Azure/azure-quickstart-templates/pull/2998), ve açıklamalarının yanı sıra bazı aşağıda fark ilgili bölümleri:

## <a name="making-the-os-disks-managed"></a>Yönetilen işletim sistemi diskleri yapma

Depolama hesabı ve disk özelliklerine ilgili çeşitli değişkenler kaldırdınız aşağıdaki fark görebiliriz. Depolama hesabı türü gereklidir artık (Standard_LRS varsayılan değerdir), ancak biz istediğinizde, biz yine da belirtebilirsiniz. Yalnızca Standard_LRS ve Premium_LRS yönetilen disk ile desteklenir. Yeni depolama hesabı soneki, benzersiz bir dize dizisi ve sa sayısı eski şablonunda depolama hesabı adları oluşturmak için kullanılmıştır. Yönetilen disk depolama hesapları müşterinin adına otomatik olarak oluşturur. çünkü bu değişkenleri artık yeni şablona gerekli değildir. Yönetilen disk otomatik olarak adları temel alınan depolama blob kapsayıcıları ve diskleri olduğundan benzer şekilde, vhd kapsayıcı adı ve işletim sistemi disk adı artık gerekli değildir.

```diff
   "variables": {
-    "storageAccountType": "Standard_LRS",
     "namingInfix": "[toLower(substring(concat(parameters('vmssName'), uniqueString(resourceGroup().id)), 0, 9))]",
     "longNamingInfix": "[toLower(parameters('vmssName'))]",
-    "newStorageAccountSuffix": "[concat(variables('namingInfix'), 'sa')]",
-    "uniqueStringArray": [
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '0')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '1')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '2')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '3')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '4')))]"
-    ],
-    "saCount": "[length(variables('uniqueStringArray'))]",
-    "vhdContainerName": "[concat(variables('namingInfix'), 'vhd')]",
-    "osDiskName": "[concat(variables('namingInfix'), 'osdisk')]",
     "addressPrefix": "10.0.0.0/16",
     "subnetPrefix": "10.0.0.0/24",
     "virtualNetworkName": "[concat(variables('namingInfix'), 'vnet')]",
```


Aşağıdaki fark biz işlem güncelleştirilmiş görebiliriz 2016-04-30-ölçek kümesi ile yönetilen disk desteği için gerekli en erken sürümü olan önizlemeye API sürümü. Biz yine yönetilmeyen diskleri eski sözdizimi yeni API sürümüyle isterseniz kullanabilirsiniz olduğunu unutmayın. Diğer bir deyişle, biz yalnızca işlem güncelleştirirseniz API sürümü ve başka bir şey değişmez, şablon önceki gibi çalışmaya devam etmelidir.

```diff
@@ -86,7 +74,7 @@
       "version": "latest"
     },
     "imageReference": "[variables('osType')]",
-    "computeApiVersion": "2016-03-30",
+    "computeApiVersion": "2016-04-30-preview",
     "networkApiVersion": "2016-03-30",
     "storageApiVersion": "2015-06-15"
   },
```

Aşağıdaki fark biz depolama hesabı kaynağı kaynakları diziden tamamen kaldırdığınızı görebilirsiniz. Yönetilen disk bunları otomatik olarak bizim adımıza oluşturduğundan artık bunları ihtiyacımız var.

```diff
@@ -113,19 +101,6 @@
       }
     },
-    {
-      "type": "Microsoft.Storage/storageAccounts",
-      "name": "[concat(variables('uniqueStringArray')[copyIndex()], variables('newStorageAccountSuffix'))]",
-      "location": "[resourceGroup().location]",
-      "apiVersion": "[variables('storageApiVersion')]",
-      "copy": {
-        "name": "storageLoop",
-        "count": "[variables('saCount')]"
-      },
-      "properties": {
-        "accountType": "[variables('storageAccountType')]"
-      }
-    },
     {
       "type": "Microsoft.Network/publicIPAddresses",
       "name": "[variables('publicIPAddressName')]",
       "location": "[resourceGroup().location]",
```

Aşağıdaki fark biz kaldırma görebiliriz depolama hesapları oluşturma döngü ayarlamak ölçekten başvuran yan tümcesi bağlıdır. Eski şablonunda, bu depolama hesapları ölçek kümesi oluşturma başladı, ancak bu yan tümcesi artık yönetilen diskle gereklidir önce oluşturulan sağlama. Bu özellikler otomatik olarak başlık altında yönetilen disk tarafından işlenen olarak biz de vhd kapsayıcıları özelliği ve işletim sistemi disk adı özelliği kaldırın. Biz onlardan varsa, eklediğimiz `"managedDisk": { "storageAccountType": "Premium_LRS" }` biz premium OS diskleri istediyseniz "osDisk" yapılandırma. Yalnızca bir büyük harf olan VM'ler veya küçük'ın ' VM'yi sku premium diskleri kullanabilirsiniz.

```diff
@@ -183,7 +158,6 @@
       "location": "[resourceGroup().location]",
       "apiVersion": "[variables('computeApiVersion')]",
       "dependsOn": [
-        "storageLoop",
         "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
         "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
       ],
@@ -200,16 +174,8 @@
         "virtualMachineProfile": {
           "storageProfile": {
             "osDisk": {
-              "vhdContainers": [
-                "[concat('https://', variables('uniqueStringArray')[0], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[1], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[2], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[3], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[4], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]"
-              ],
-              "name": "[variables('osDiskName')]",
             },
             "imageReference": "[variables('imageReference')]"
           },

```

Yönetilen veya yönetilmeyen disk kullanıp kullanmayacağınızı için ölçek kümesi yapılandırmasında açık bir özellik yok. Ölçek kümesini kullanmak için depolama profilinde mevcut özelliklerine göre bilir. Bu nedenle, Ölçek kümesi depolama profilinde hakkı özellikleri sağlamak için şablon değiştirirken önemlidir.


## <a name="data-disks"></a>Veri diskleri

Yukarıdaki değişikliklerle ölçek kümesi kullanan yönetilen diskler işletim sistemi için ancak, veri diskleri disk? Veri diskleri eklemek için "storageProfile" altında "dataDisks" özelliği "osDisk" ile aynı düzeyde ekleyin. Her biri "(bir VM üzerinde veri disk başına benzersiz olması gerekir) LUN'un" özellikleri vardır, JSON nesnelerin bir listesini, özellik değeri "createOption" ("boş" şu anda yalnızca desteklenen seçenektir) ve "diskSizeGB" (gigabayt olarak; disk boyutu büyük olmalı 0 ile 1024'ten az) aşağıdaki örnekteki gibi: 

```
"dataDisks": [
  {
    "lun": "1",
    "createOption": "empty",
    "diskSizeGB": "1023"
  }
]
```

Belirtirseniz `n` diskleri bu dizideki her VM ölçek kümesi alır `n` veri diski. Ancak, bu veri diskleri ham aygıtların olduğunu unutmayın. Bunlar biçimlendirilmemiş. Bu, paritition, ekleme ve kullanmadan önce diskleri biçimlendirin müşteriye kadar olur. Biz de isteğe bağlı olarak, belirtebilirsiniz `"managedDisk": { "storageAccountType": "Premium_LRS" }` her veri diski nesnesinde bir premium veri diski gerektiğini belirtin. Yalnızca bir büyük harf olan VM'ler veya küçük'ın ' VM'yi sku premium diskleri kullanabilirsiniz.

Veri diskleri ölçek kümeleri ile kullanma hakkında daha fazla bilgi için bkz: [bu makalede](./virtual-machine-scale-sets-attached-disks.md).


## <a name="next-steps"></a>Sonraki adımlar
Örneğin Ölçek kümeleri kullanarak Resource Manager şablonları arama "vmss için" içinde [Azure hızlı başlangıç şablonlarını github deposuna](https://github.com/Azure/azure-quickstart-templates).

Genel bilgi için kullanıma [ölçek kümeleri için ana giriş sayfasının](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

