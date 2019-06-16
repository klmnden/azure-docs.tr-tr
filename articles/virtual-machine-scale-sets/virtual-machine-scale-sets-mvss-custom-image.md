---
title: Bir Azure ölçek kümesi şablonunun özel görüntüsünü başvurusu | Microsoft Docs
description: Özel bir görüntü için mevcut bir Azure sanal makine ölçek kümesi şablonu eklemeyi öğrenin
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: drewm
editor: ''
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2018
ms.author: manayar
ms.openlocfilehash: 2415d0dc2b9a2c4229d9910b42eb8ec9309ac7a7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64869108"
---
# <a name="add-a-custom-image-to-an-azure-scale-set-template"></a>Özel bir görüntü için bir Azure ölçek kümesi şablonu Ekle

Bu makalede nasıl değiştirileceğini gösterir [temel ölçek kümesi şablonunu](virtual-machine-scale-sets-mvss-start.md) özel görüntüden dağıtılacak.

## <a name="change-the-template-definition"></a>Şablon tanımı değiştirme
İçinde bir [önceki makalede](virtual-machine-scale-sets-mvss-start.md) temel ölçek kümesi şablonunun oluşturduğumuz. Şimdi daha önceki bu şablonu kullanın eder ve bir ölçek kümesi özel bir görüntüden dağıtan bir şablon oluşturmak için değiştirin.  

### <a name="creating-a-managed-disk-image"></a>Bir yönetilen disk görüntüsü oluşturma

Özel bir yönetilen disk görüntüsü zaten varsa (bir kaynak türü `Microsoft.Compute/images`), sonra da bu bölümü atlayabilirsiniz.

İlk olarak, ekleme bir `sourceImageVhdUri` genelleştirilmiş dağıtmak için özel görüntü içeren bir Azure depolama blob URI'si olan parametre.


```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "sourceImageVhdUri": {
+      "type": "string",
+      "metadata": {
+        "description": "The source of the generalized blob containing the custom image"
+      }
     }
   },
   "variables": {},
```

Ardından, bir kaynak türü ekleyin `Microsoft.Compute/images`, yönetilen disk görüntüsü URI'da bulunan genelleştirilmiş blob dayalı olduğu `sourceImageVhdUri`. Bu görüntü, bunu kullanan ölçek kümesi ile aynı bölgede olması gerekir. Görüntü özelliklerinde işletim sistemi türü, blob konumunu belirtin (gelen `sourceImageVhdUri` parametresi) ve depolama hesabı türü:

```diff
   "resources": [
     {
+      "type": "Microsoft.Compute/images",
+      "apiVersion": "2019-03-01",
+      "name": "myCustomImage",
+      "location": "[resourceGroup().location]",
+      "properties": {
+        "storageProfile": {
+          "osDisk": {
+            "osType": "Linux",
+            "osState": "Generalized",
+            "blobUri": "[parameters('sourceImageVhdUri')]",
+            "storageAccountType": "Standard_LRS"
+          }
+        }
+      }
+    },
+    {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "myVnet",
       "location": "[resourceGroup().location]",

```

Kaynak ölçek kümesi, ekleme bir `dependsOn` ölçek kümesi bu görüntüden dağıtmayı denemeden önce görüntünün emin olmak için özel görüntü başvuran yan tümcesi oluşturulur:

```diff
       "location": "[resourceGroup().location]",
       "apiVersion": "2019-03-01-preview",
       "dependsOn": [
-        "Microsoft.Network/virtualNetworks/myVnet"
+        "Microsoft.Network/virtualNetworks/myVnet",
+        "Microsoft.Compute/images/myCustomImage"
       ],
       "sku": {
         "name": "Standard_A1",

```

### <a name="changing-scale-set-properties-to-use-the-managed-disk-image"></a>Ölçeği değiştirmek yönetilen disk görüntüsü kullanmak için özellikleri ayarlama

İçinde `imageReference` ölçek kümesi `storageProfile`, yayımcı belirtmek yerine, teklif, sku ve bir platform görüntüsü sürümü belirtin `id` , `Microsoft.Compute/images` kaynak:

```diff
         "virtualMachineProfile": {
           "storageProfile": {
             "imageReference": {
-              "publisher": "Canonical",
-              "offer": "UbuntuServer",
-              "sku": "16.04-LTS",
-              "version": "latest"
+              "id": "[resourceId('Microsoft.Compute/images', 'myCustomImage')]"
             }
           },
           "osProfile": {
```

Bu örnekte kullanın `resourceId` aynı şablonda oluşturulan görüntünün kaynak Kimliğini almak için işlevi. Yönetilen disk görüntüsü önceden oluşturduysanız, bunun yerine o yansıma Kimliğini sağlamanız gerekir. Bu kimliği biçiminde olmalıdır: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Compute/images/<image-name>`.


## <a name="next-steps"></a>Sonraki Adımlar

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
