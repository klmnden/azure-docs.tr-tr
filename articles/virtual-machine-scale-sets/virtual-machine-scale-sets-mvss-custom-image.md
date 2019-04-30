---
title: Bir Azure ölçek kümesi şablonunun özel görüntüsünü başvurusu | Microsoft Docs
description: Özel bir görüntü için mevcut bir Azure sanal makine ölçek kümesi şablonu eklemeyi öğrenin
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
origin.date: 05/10/2017
ms.date: 11/30/2018
ms.author: v-junlch
ms.openlocfilehash: 2e3c8177a32082c251be74e597a18730ae1c9d37
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62108386"
---
# <a name="add-a-custom-image-to-an-azure-scale-set-template"></a>Özel bir görüntü için bir Azure ölçek kümesi şablonu Ekle

Bu makalede nasıl değiştirileceğini gösterir [en düşük uygun ölçek kümesi şablonunu](./virtual-machine-scale-sets-mvss-start.md) özel görüntüden dağıtılacak.

## <a name="change-the-template-definition"></a>Şablon tanımı değiştirme

En düşük uygun ölçek kümesi şablonunun görülebilir [burada](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), ve özel bir görüntüden dağıtma ölçek kümesi için şablon görülebilir [burada](https://raw.githubusercontent.com/gatneil/mvss/custom-image/azuredeploy.json). Bu şablon oluşturmak için kullanılan fark inceleyelim (`git diff minimum-viable-scale-set custom-image`) parça parça:

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
+      "apiVersion": "2016-04-30-preview",
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
       "apiVersion": "2016-04-30-preview",
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

<!-- Update_Description: update metedata properties -->