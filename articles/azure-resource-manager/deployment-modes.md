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
ms.date: 08/07/2018
ms.author: tomfitz
ms.openlocfilehash: 00800cb233776878e1fa330ce72cb067b8c698f3
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39635127"
---
# <a name="azure-resource-manager-deployment-modes"></a>Azure Resource Manager dağıtım modları
Kaynaklarınızı dağıtırken dağıtım Artımlı güncelleştirme ya da tam güncelleştirme olduğunu belirtin.  Bu iki mod arasındaki başlıca fark, Resource Manager şablonunda olmayan mevcut kaynaklar kaynak grubunda nasıl işlediğini ' dir.
Artımlı varsayılan moddur.

## <a name="incremental-and-complete-deployments"></a>Artımlı ve tam dağıtımları
Kaynak dağıtırken:

* Tam modda, Resource Manager **siler** kaynak grubunda var, ancak şablonda belirtilmeyen kaynakları. 
* Artımlı modda, Resource Manager **değişmeden kalır** kaynak grubunda var, ancak şablonda belirtilmeyen kaynakları.

Her iki mod için Resource Manager şablonunda belirtilen tüm kaynakları sağlamak çalışır. Kaynağın kaynak grubunda zaten mevcut ve ayarlarına aynıdır, işlemi hiçbir değişikliğe neden olur. Kaynak, bir kaynak ayarlarını değiştirirseniz, bu yeni ayarlar ile sağlanır. Dağıtım, konum veya mevcut bir kaynak türünü güncelleştirme çalışırsanız, bir hata ile başarısız olur. Bunun yerine, yeni bir kaynak konumu ile dağıtın veya sizi gereken yazın.

## <a name="example-result"></a>Örnek sonucu

Artımlı ve tam modları arasındaki farkı anlamak için aşağıdaki senaryoyu göz önünde bulundurun.

**Mevcut kaynak grubunu** içerir:

* Kaynak
* Kaynak B
* C kaynak

**Şablon** tanımlar:

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

```powershell
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

Dağıtım modu ayarlamak için bir [bağlantılı veya iç içe geçmiş şablon](resource-group-linked-templates.md), kullanın `mode` özelliği.

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

