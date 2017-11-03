---
title: "Azure üst kaynak hataları | Microsoft Docs"
description: "Bir üst kaynağıyla çalışırken hatalarını çözümlemeyi açıklar."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 09/13/2017
ms.author: tomfitz
ms.openlocfilehash: e59147c4ac18f730f27b9d4aa9c008f219881065
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="resolve-errors-for-parent-resources"></a>Üst kaynaklar için hataları çözümleyin

Bu makalede, bir üst kaynağına bağımlı bir kaynak dağıtımı sırasında karşılaşabileceğiniz hatalar açıklanır.

## <a name="symptom"></a>Belirti

Başka bir kaynak için bir alt olan bir kaynak dağıtırken, aşağıdaki hata iletisini alabilirsiniz:

```
Code=ParentResourceNotFound;
Message=Can not perform requested operation on nested resource. Parent resource 'exampleserver' not found."
```

## <a name="cause"></a>Nedeni

Bir kaynak için başka bir kaynağı bir alt öğesi olduğunda üst kaynak alt kaynak oluşturmadan önce mevcut olması gerekir. Alt kaynağın adını üst adı içerir. Örneğin, bir SQL veritabanı olarak tanımlanabilir:

```json
{
  "type": "Microsoft.Sql/servers/databases",
  "name": "[concat(variables('databaseServerName'), '/', parameters('databaseName'))]",
  ...
```

Ancak, sunucu üzerinde bir bağımlılık belirtmezseniz, sunucu dağıtmış olan önce veritabanı dağıtımı başlayabilir.

## <a name="solution"></a>Çözüm

Bu hatayı gidermek için bir bağımlılık ekleyin.

```json
"dependsOn": [
    "[variables('databaseServerName')]"
]
```

Daha fazla bilgi için bkz: [dağıtma kaynakları Azure Resource Manager şablonlarındaki sırasını tanımlamak](resource-group-define-dependencies.md).