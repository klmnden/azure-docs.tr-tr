---
title: Azure Resource Manager dağıtım modları | Microsoft Docs
description: Azure Resource Manager ile tam veya artımlı dağıtım modu kullanıp kullanmayacağınızı açıklar.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/24/2019
ms.author: tomfitz
ms.openlocfilehash: 9120e5f283f8d8da8da2c80959a335965a643409
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2019
ms.locfileid: "54903902"
---
# <a name="azure-resource-manager-deployment-modes"></a>Azure Resource Manager dağıtım modları

Kaynaklarınızı dağıtırken dağıtım Artımlı güncelleştirme ya da tam güncelleştirme olduğunu belirtin.  Bu iki mod arasındaki başlıca fark, Resource Manager şablonunda olmayan mevcut kaynaklar kaynak grubunda nasıl işlediğini ' dir. Artımlı varsayılan moddur.

## <a name="incremental-and-complete-deployments"></a>Artımlı ve tam dağıtımları

Her iki mod için Resource Manager şablonunda belirtilen tüm kaynakları oluşturmaya çalışır. Kaynağın kaynak grubunda zaten mevcut ve ayarlarına aynıdır, işlem bu kaynak için alınır. Kaynak, bir kaynak için özellik değerlerini değiştirirseniz, bu yeni değerleri ile güncelleştirilir. Konum veya mevcut bir kaynak türünü güncelleştirmek çalışırsanız, dağıtım bir hata ile başarısız olur. Bunun yerine, yeni bir kaynak konumu ile dağıtın veya sizi gereken yazın.

Tam modda, Resource Manager **siler** kaynak grubunda var, ancak şablonda belirtilmeyen kaynakları. Şablonda belirtilen olan ancak çünkü dağıtılan kaynakları bir [koşul](resource-manager-templates-resources.md#condition) yanlış olarak değerlendirilir, silinmez.

Artımlı modda, Resource Manager **değişmeden kalır** kaynak grubunda var, ancak şablonda belirtilmeyen kaynakları. Artımlı modda bir kaynak dağıtarak, yalnızca güncelleştirmekte olanlara kaynak için tüm özellik değerlerini belirtin. Bazı özellikler belirtmezseniz, Resource Manager güncelleştirme bu değerlerin üzerine yazar olarak yorumlar.

## <a name="example-result"></a>Örnek sonucu

Artımlı ve tam modları arasındaki farkı anlamak için aşağıdaki senaryoyu göz önünde bulundurun.

**Kaynak grubu** içerir:

* Kaynak
* Kaynak B
* C kaynak

**Şablon** içerir:

* Kaynak
* Kaynak B
* Kaynak D

Dağıtılmış olduğunda **artımlı** modu, kaynak grubu vardır:

* Kaynak
* Kaynak B
* C kaynak
* Kaynak D

Dağıtılmış olduğunda **tam** modu, kaynak C silinir. Kaynak grubu vardır:

* Kaynak
* Kaynak B
* Kaynak D

## <a name="set-deployment-mode"></a>Dağıtım modunu ayarlama

PowerShell ile dağıtım yaparken, dağıtım modu ayarlamak için kullanın `Mode` parametresi.

```azurepowershell-interactive
New-AzureRmResourceGroupDeployment `
  -Mode Complete `
  -Name ExampleDeployment `
  -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json
```

Azure CLI ile dağıtım yaparken, dağıtım modu ayarlamak için kullanın `mode` parametresi.

```azurecli-interactive
az group deployment create \
  --name ExampleDeployment \
  --mode Complete \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS
```

Kullanırken bir [bağlantılı veya iç içe geçmiş şablon](resource-group-linked-templates.md), ayarlamalısınız `mode` özelliğini `Incremental`. Yalnızca kök düzeyinde şablonu tam dağıtım modunu destekler.

```json
"resources": [
  {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          <nested-template-or-external-template>
      }
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar

* Resource Manager şablonları oluşturma hakkında bilgi edinmek için [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
* Kaynakları dağıtma hakkında bilgi edinmek için [Azure Resource Manager şablonu ile uygulama dağıtma](resource-group-template-deploy.md).
* Bir kaynak sağlayıcısı işlemleri görüntülemek için bkz: [Azure REST API'si](/rest/api/).
