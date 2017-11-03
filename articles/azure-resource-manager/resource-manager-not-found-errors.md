---
title: "Azure kaynak bulunamadı hatası | Microsoft Docs"
description: "Bir kaynak bulunamadığında hatalarını çözümlemeyi açıklar."
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
ms.openlocfilehash: c76c965c43ca8217faa9488c01975ce09a21daaf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="resolve-not-found-errors-for-azure-resources"></a>Azure kaynakları için bulunamadı hatalarını çözümleme

Bu makalede, bir kaynak dağıtım sırasında bulunamadığında karşılaşabileceğiniz hatalar açıklanır. 

## <a name="symptom"></a>Belirti

Şablonunuzu çözümlenemeyen kaynağın adını içerdiğinde, benzer bir hata alırsınız:

```
Code=NotFound;
Message=Cannot find ServerFarm with name exampleplan.
```

Kullanmayı denerseniz, [başvuru](resource-group-template-functions-resource.md#reference) veya [listKeys](resource-group-template-functions-resource.md#listkeys) işlevleri bir kaynakla aşağıdaki hata iletisini çözümlenemiyor:

```
Code=ResourceNotFound;
Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource
group {resource group name} was not found.
```

## <a name="cause"></a>Nedeni

Resource Manager kaynak özelliklerini almak gerekiyor, ancak aboneliğinizde kaynak tanımlanamıyor.

## <a name="solution"></a>Çözüm

### <a name="solution-1"></a>Çözüm 1

Şablon eksik kaynak dağıtmak çalışıyorsunuz, bir bağımlılık eklemeniz gerekip gerekmediğini denetleyin. Resource Manager kaynakları paralel olarak, mümkün olduğunda oluşturarak dağıtım en iyi duruma getirir. Bir kaynak sonra başka bir kaynak dağıtılmalıdır, kullanmanıza gerek **dependsOn** öğesi şablonunuzda diğer kaynak üzerinde bir bağımlılık oluşturun. Örneğin, bir web uygulaması dağıtımını yaparken, uygulama hizmeti planı bulunması gerekir. Web uygulaması uygulama hizmeti plan üzerinde bağlıdır belirtmediyseniz, Resource Manager aynı anda hem de kaynaklar oluşturur. Bunu henüz web uygulamasında bir özellik Ayarla girişiminde bulunulduğunda var olmadığından uygulama hizmeti planı kaynağın bulunamadığını bildiren bir hata alırsınız. Bu hata, web uygulamasında bağımlılık ayarlayarak engeller.

```json
{
  "apiVersion": "2015-08-01",
  "type": "Microsoft.Web/sites",
  "dependsOn": [
    "[variables('hostingPlanName')]"
  ],
  ...
}
```

Bağımlılık hatalarında sorun giderme hakkında daha fazla bilgi için bkz: [denetleyin dağıtım sırası](resource-manager-troubleshoot-tips.md#check-deployment-sequence).

### <a name="solution-2"></a>Çözüm 2

İçin dağıtılan olandan farklı bir kaynak grubundaki kaynak mevcut olduğunda kullanın [ResourceId işlevi](resource-group-template-functions-resource.md#resourceid) kaynak tam adını almak için.

```json
"properties": {
    "name": "[parameters('siteName')]",
    "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
}
```

### <a name="solution-3"></a>Çözüm 3

İçeren bir ifade Ara [başvuru](resource-group-template-functions-resource.md#reference) işlevi. Sağladığınız değerleri kaynak aynı şablonu, kaynak grubu ve abonelik olup göre farklılık gösterir. Senaryonuz için gerekli parametre değerlerini sağlayarak kontrol edin. Kaynak farklı bir kaynak grubu içinde ise, tam kaynak kimliğini sağlayın Örneğin, bir depolama hesabı başka bir kaynak grubuna başvurmak için kullanın:

```json
"[reference(resourceId('exampleResourceGroup', 'Microsoft.Storage/storageAccounts', 'myStorage'), '2017-06-01')]"
```