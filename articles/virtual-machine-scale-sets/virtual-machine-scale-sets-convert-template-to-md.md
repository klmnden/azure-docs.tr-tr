---
title: Yönetilen disk kullanmak üzere bir Azure Resource Manager ölçek kümesi şablonu dönüştürme | Microsoft Docs
description: Bir ölçek kümesi şablonunun bir yönetilen disk ölçüm kümesi şablonuna dönüştürme.
keywords: sanal makine ölçek kümeleri
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: bc8c377a-8c3f-45b8-8b2d-acc2d6d0b1e8
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/18/2017
ms.author: manayar
ms.openlocfilehash: b2d1738b85799079b3af7ab39c5cb1799a38d382
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60731746"
---
# <a name="convert-a-scale-set-template-to-a-managed-disk-scale-set-template"></a>Bir ölçek kümesi şablonunun bir yönetilen disk ölçüm kümesi şablonuna dönüştürme

Yönetilen disk kullanmayan bir ölçek kümesi oluşturmak için Resource Manager şablonu ile müşteriler, yönetilen disk kullanacak şekilde değiştirmek isteyebilirsiniz. Bu makalede, bir çekme isteğinde bir örnek olarak kullanarak, yönetilen diskleri kullanma gösterilmektedir [Azure hızlı başlangıç şablonları](https://github.com/Azure/azure-quickstart-templates), Resource Manager şablonları için topluluk odaklı bir depo. Tam bir çekme isteği burada görülebilir: [ https://github.com/Azure/azure-quickstart-templates/pull/2998 ](https://github.com/Azure/azure-quickstart-templates/pull/2998), ve açıklamalar ile birlikte aşağıda fark ilgili bölümleri şunlardır:

## <a name="making-the-os-disks-managed"></a>Yönetilen işletim sistemi diskleri yapma

Depolama hesabı ve disk özelliklerine ilgili çeşitli değişkenleri aşağıdaki Farklarda kaldırılır. Depolama hesabı türü gereklidir artık (Standard_LRS, varsayılan), ancak isterseniz belirtebilirsiniz. Yalnızca Standard_LRS ve Premium_LRS yönetilen disk ile desteklenir. Yeni depolama hesabı soneki, benzersiz bir dize dizisi ve sa sayısını eski şablonda depolama hesabı adları oluşturmak için kullanılan. Yönetilen disk depolama hesapları müşteri adına otomatik olarak oluşturur. çünkü bu değişkenler artık yeni şablonu gereklidir. Temel alınan depolama blob kapsayıcılarına ve diskler yönetilen disk adları benzer şekilde, vhd kapsayıcı adı ve işletim sistemi disk adı artık gerekli değildir.

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


Aşağıdaki Farklarda API Sürüm 2016-04-30-preview için ölçek kümeleri ile yönetilen disk desteği için gerekli en erken sürümü olan güncelleştirilmiş işlem. İsterseniz, yeni API sürümü eski sözdizimi ile yönetilmeyen diskler kullanabilirsiniz. Yalnızca işlem API sürümünü güncelleştirin ve başka bir şey değişmez, şablonu önceki gibi çalışmaya devam etmesi gerekir.

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

Aşağıdaki Farklarda depolama hesabı kaynağı kaynak diziden tamamen kaldırılır. Yönetilen disk bunları otomatik olarak oluşturduğundan kaynak artık gerekli değildir.

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

Aşağıdaki Farklarda kaldırıyoruz olduğunu görebiliriz depolama hesapları oluştururken döngüye ölçek gelen başvuran yan tümcesi bağlıdır. Eski şablonunda, bu depolama hesaplarının ölçek kümesi oluşturma başladı, ancak bu yan tümce artık yönetilen diskler sayesinde gereklidir önce oluşturulan sağlama. Bu özellikler tarafından yönetilen disk başlık altında otomatik olarak işlenir gibi vhd kapsayıcıları özelliği de, işletim sistemi disk adı özelliği ile birlikte kaldırılır. Ekleyebilirsiniz `"managedDisk": { "storageAccountType": "Premium_LRS" }` premium işletim sistemi diskleri istediyseniz "osDisk" yapılandırma. Yalnızca VM içeren bir büyük harf veya küçük 's' VM sku premium diskler kullanabilirsiniz.

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

Ölçek kümesi yapılandırması için yönetilen veya yönetilmeyen disk kullanmayı açık özellik yok. Ölçek kümesi kullanmak için depolama profilinde bulunan özelliklerine göre bilir. Bu nedenle, Ölçek kümesinin depolama profilinde hakkı özellikleri sağlamak için bir şablonu değiştirirken önemlidir.


## <a name="data-disks"></a>Veri diskleri

Yukarıdaki değişiklikler ile ölçek kümesi yönetilen diskleri kullan işletim sistemi için veri diskleri ancak disk? Veri diskleri eklemek için "osDisk" ile aynı düzeyde "Datadisks" altındaki "Storageprofile" özelliğini ekleyin. Özelliğinin değeri her biri "(hangi veri diski üzerinde bir VM başına benzersiz olması gerekir) lun" özellikleri vardır, JSON nesneleri listesidir "createOption" ("boş" şu anda desteklenen tek seçenek olan) ve "diskSizeGB" (gigabayt olarak; disk boyutunu değerinden büyük olmalıdır 0 ile 1024'ten küçük) aşağıdaki örnekte olduğu gibi:

```
"dataDisks": [
  {
    "lun": "1",
    "createOption": "empty",
    "diskSizeGB": "1023"
  }
]
```

Belirtirseniz `n` diskleri dizideki her bir VM ölçek kümesi alır `n` veri diskleri. Ancak, bu veri diskleri, ham cihaz olduğunu unutmayın. Bunlar biçimlendirilmemiş. Bu, müşterinin, bölümü eklemek ve kullanmadan önce biçimlendirmek için en fazla olur. İsteğe bağlı olarak da belirtebilirsiniz `"managedDisk": { "storageAccountType": "Premium_LRS" }` her veri diski nesnede premium veri diski gerektiğini belirtin. Yalnızca VM içeren bir büyük harf veya küçük 's' VM sku premium diskler kullanabilirsiniz.

Veri disklerini ölçek kümeleri ile kullanma hakkında daha fazla bilgi için bkz: [bu makalede](./virtual-machine-scale-sets-attached-disks.md).


## <a name="next-steps"></a>Sonraki adımlar
Örneğin Resource Manager şablonları kullanarak ölçek kümeleri, arama "vmss için" içinde [Azure hızlı başlangıç şablonları GitHub deposunda](https://github.com/Azure/azure-quickstart-templates).

Genel bilgiler için kullanıma [ölçek kümeleri için ana giriş sayfası](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

