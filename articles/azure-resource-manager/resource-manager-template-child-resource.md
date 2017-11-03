---
title: "Alt kaynak Azure şablonda tanımlamak | Microsoft Docs"
description: "Kaynak türü ve alt kaynağın adını bir Azure Resource Manager şablonu nasıl ayarlanacağını gösterir"
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: tomfitz
ms.openlocfilehash: 5b6ce5526f354008eb4a697deec737876f22391f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="set-name-and-type-for-child-resource-in-resource-manager-template"></a>Alt kaynak için ad ve tür Resource Manager şablonunda ayarlayın
Bir şablon oluştururken sık üst kaynak ilişkili bir alt kaynak eklemeniz gerekir. Örneğin, şablonunuza bir SQL server ve veritabanı içerebilir. SQL server üst kaynak ve alt kaynak veritabanıdır. 

Alt öğe kaynak türünü biçimdedir:`{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`

Alt kaynak adının biçimi şöyledir:`{parent-resource-name}/{child-resource-name}`

Ancak, tür ve ad bu farklı içindeki üst kaynak olup olmadığını iç içe yerleştirilmiş, veya kendi en üst düzeyinde dayanan bir şablonu belirtin. Bu konu, her iki yaklaşımın nasıl ele alınacağını gösterir.

Bir kaynağa tam bir başvuru oluşturulurken, tür ve ad bölümlerinin birleştirmek için sipariş iki yalnızca bir birleşimini değil.  Bunun yerine, sonra ad alanı, bir dizi kullanın *türü/adı* en az belirli bir çiftlerinden en belirli:

```json
{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]*
```

Örneğin:

`Microsoft.Compute/virtualMachines/myVM/extensions/myExt`doğru `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` doğru değil

## <a name="nested-child-resource"></a>İç içe alt kaynak
Alt kaynak tanımlamak için en kolay yolu üst kaynak içindeki iç içe olmaktır. Aşağıdaki örnek, içinde bir SQL Server iç içe bir SQL veritabanı gösterir.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  ...
  "resources": [
    {
      "name": "exampledatabase",
      "type": "databases",
      "apiVersion": "2014-04-01",
      ...
    }
  ]
}
```

Alt kaynağı için tür ayarlamak `databases` ancak kendi tam kaynak türü `Microsoft.Sql/servers/databases`. Sunmaz `Microsoft.Sql/servers/` üst kaynak türünden varsayıldığından. Alt kaynak adı ayarlamak `exampledatabase` ancak üst adı tam adını içerir. Sunmaz `exampleserver` üst kaynak varsayıldığından.

## <a name="top-level-child-resource"></a>Üst düzey alt kaynak
En üst düzeyinde alt kaynak tanımlayabilirsiniz. Üst kaynak şablonun aynı dağıtılmamışsa, ya da varsa bu yaklaşımı kullanabilir kullanmak istediğiniz `copy` birden çok alt kaynaklarını oluşturun. Bu yaklaşımda, tam kaynak türü sağlayın ve üst kaynak adı alt kaynak adında gerekir.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  "resources": [ 
  ],
  ...
},
{
  "name": "exampleserver/exampledatabase",
  "type": "Microsoft.Sql/servers/databases",
  "apiVersion": "2014-04-01",
  ...
}
```

Aynı düzeydeki şablonda tanımlanan olsa bile veritabanı sunucusuna alt kaynaktır.

## <a name="next-steps"></a>Sonraki adımlar
* Şablonlarının nasıl oluşturulacağı hakkında daha fazla önerileri için bkz: [en iyi uygulamalar Azure Resource Manager şablonları oluşturmak için](resource-manager-template-best-practices.md).
* Birden çok alt kaynakları oluşturma örneği için bkz: [Azure Resource Manager şablonları kaynaklarında birden çok örneğini dağıtmak](resource-group-create-multiple.md).
