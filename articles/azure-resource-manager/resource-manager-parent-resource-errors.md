---
title: Azure ana kaynak hataları | Microsoft Docs
description: Bir üst kaynak çalışırken hataları gidermek nasıl açıklar.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 08/01/2018
ms.author: tomfitz
ms.openlocfilehash: 19642138105f3de0e2bda3f0f80e12cf74a35709
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61059999"
---
# <a name="resolve-errors-for-parent-resources"></a>Üst kaynaklar için hataları çözümleyin

Bu makalede, bir üst kaynağına bağımlı bir kaynağa dağıtırken alabilirsiniz hataları açıklanır.

## <a name="symptom"></a>Belirti

Bir alt başka bir kaynak için bir kaynak dağıtım yaparken, aşağıdaki hata iletisini alabilirsiniz:

```
Code=ParentResourceNotFound;
Message=Can not perform requested operation on nested resource. Parent resource 'exampleserver' not found."
```

## <a name="cause"></a>Nedeni

Bir kaynak başka bir kaynak alt, üst kaynak alt kaynak oluşturmadan önce mevcut olması gerekir. Alt kaynak adı üst kaynağı ile bağlantıyı tanımlar. Alt kaynak adı şu biçimdedir `<parent-resource-name>/<child-resource-name>`. Örneğin, bir SQL veritabanı olarak tanımlanabilir:

```json
{
  "type": "Microsoft.Sql/servers/databases",
  "name": "[concat(variables('databaseServerName'), '/', parameters('databaseName'))]",
  ...
```

Hem sunucu hem de veritabanında aynı şablonu dağıtın, ancak sunucu üzerinde bir bağımlılık belirtmezseniz, sunucuya dağıtıldığını önce veritabanı dağıtımı başlayabilir. 

Üst kaynak zaten varsa ve aynı şablonda dağıtılan değil, Resource Manager ile üst alt kaynak ilişkilendirilemiyor olduğunda bu hatayı alırsınız. Bu hata alt kaynak doğru biçimde değil veya alt kaynak kaynak grubu üst kaynak için farklı bir kaynak grubuna dağıtılan zaman da gerçekleşebilir.

## <a name="solution"></a>Çözüm

Üst ve alt kaynakları aynı şablon dağıtıldığında bu hatayı gidermek için bir bağımlılık içerir.

```json
"dependsOn": [
    "[variables('databaseServerName')]"
]
```

Üst kaynak daha önce farklı bir şablona dağıtıldığında bu hatayı gidermek için bir bağımlılık ayarlamanız gerekmez. Bunun yerine, alt aynı kaynak grubuna dağıtın ve üst kaynak adını sağlayın.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "sqlServerName": {
            "type": "string"
        },
        "databaseName": {
            "type": "string"
        }
    },
    "resources": [
        {
            "apiVersion": "2014-04-01",
            "type": "Microsoft.Sql/servers/databases",
            "location": "[resourceGroup().location]",
            "name": "[concat(parameters('sqlServerName'), '/', parameters('databaseName'))]",
            "properties": {
                "collation": "SQL_Latin1_General_CP1_CI_AS",
                "edition": "Basic"
            }
        }
    ],
    "outputs": {}
}
```

Daha fazla bilgi için [tanımlamak için Azure Resource Manager şablonlarında kaynak dağıtmaya](resource-group-define-dependencies.md).