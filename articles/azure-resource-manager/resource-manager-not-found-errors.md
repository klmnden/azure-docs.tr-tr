---
title: Azure kaynak bulunamadı hatası | Microsoft Docs
description: Bir kaynak bulunamadığında hatalarını çözümlemeyi açıklar.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 06/06/2018
ms.author: tomfitz
ms.openlocfilehash: 494526ae2084053f23bb3a096ac7d089c47a731a
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34823444"
---
# <a name="resolve-not-found-errors-for-azure-resources"></a>Azure kaynakları için bulunamadı hatalarını çözümleme

Bu makalede, dağıtım sırasında bir kaynağın ne zaman bulunamıyor görebileceğiniz hatalar açıklanır.

## <a name="symptom"></a>Belirti

Şablonunuzu çözümlenemeyen kaynağın adını içerdiğinde, benzer bir hata alırsınız:

```
Code=NotFound;
Message=Cannot find ServerFarm with name exampleplan.
```

Kullanırsanız [başvuru](resource-group-template-functions-resource.md#reference) veya [listKeys](resource-group-template-functions-resource.md#listkeys) işlevleri bir kaynakla aşağıdaki hata iletisini çözümlenemiyor:

```
Code=ResourceNotFound;
Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource
group {resource group name} was not found.
```

## <a name="cause"></a>Nedeni

Resource Manager kaynak özelliklerini almak gerekiyor, ancak aboneliğinizde kaynak tanımlanamıyor.

## <a name="solution-1---set-dependencies"></a>Çözüm 1 - kümesi bağımlılıkları

Şablon eksik kaynak dağıtmak çalışıyorsanız, bir bağımlılık eklemeniz gerekip gerekmediğini denetleyin. Resource Manager kaynakları paralel olarak, mümkün olduğunda oluşturarak dağıtım en iyi duruma getirir. Bir kaynak sonra başka bir kaynak dağıtılmalıdır, kullanmanıza gerek **dependsOn** şablonunuzda öğesi. Örneğin, bir web uygulaması dağıtımını yaparken, uygulama hizmeti planı bulunması gerekir. Web uygulaması uygulama hizmeti plan üzerinde bağlıdır belirtmediyseniz, Resource Manager aynı anda hem kaynakları oluşturur. Henüz web uygulamasında bir özellik Ayarla girişiminde bulunulduğunda var olmadığı için uygulama hizmeti planı kaynak bulunamıyor, belirten bir hata alırsınız. Bu hata, web uygulamasında bağımlılık ayarlayarak engeller.

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

Ancak, artık gerekmeyen bağımlılıklarını ayarlama önlemek istiyor. Gereksiz bağımlılıkları varsa, paralel olarak dağıtılan birbirlerine bağımlı olmayan kaynakları engelleyerek dağıtım süresini uzatmak. Ayrıca, dağıtım engelleme döngüsel bağımlılıklar oluşturabilir. [Başvuru](resource-group-template-functions-resource.md#reference) işlevi ve [listesi *](resource-group-template-functions-resource.md#listkeys-listsecrets-and-list) işlevleri bu kaynağa aynı şablonunda dağıtılır ve adıyla (değil kaynak kimliği başvurulan bu dolaylı bir bağımlılığı başvurulan kaynakta oluşturur ). Bu nedenle, bağımlılıkları, belirtilenden daha fazla bağımlılıkları olabilir **dependsOn** özelliği. [ResourceId](resource-group-template-functions-resource.md#resourceid) işlevi değil ya da dolaylı bir bağımlılığı oluşturmak kaynak var olduğunu doğrulayın. [Başvuru](resource-group-template-functions-resource.md#reference) işlevi ve [listesi *](resource-group-template-functions-resource.md#listkeys-listsecrets-and-list) işlevleri kaynak kaynak kimliğini tarafından başvurulan dolaylı bir bağımlılığı oluştur Örtük bir bağımlılık oluşturmak için aynı şablonu dağıtılan kaynağın adını geçirin.

Bağımlılık sorunlarını gördüğünüzde, kaynak dağıtım sırasını kavramanıza gerekir. Dağıtım işlemlerini sırasını görüntülemek için:

1. Dağıtım geçmişi kaynak grubunuz için seçin.

   ![Dağıtım geçmişi seçin](./media/resource-manager-not-found-errors/select-deployment.png)

2. Bir dağıtım geçmişinden ve seçin **olayları**.

   ![Dağıtım olaylarını seçin](./media/resource-manager-not-found-errors/select-deployment-events.png)

3. Her kaynak için olayların sırası inceleyin. Her işlemin durumunu dikkat edin. Örneğin, aşağıdaki resimde, paralel olarak dağıtılan üç depolama hesaplarını gösterir. Üç depolama hesapları aynı anda başlatılır dikkat edin.

   ![Paralel dağıtım](./media/resource-manager-not-found-errors/deployment-events-parallel.png)

   Sonraki resmi paralel olarak dağıtılan olmayan üç depolama hesaplarını gösterir. İlk Depolama hesabında ikinci depolama hesabı bağlıdır ve ikinci depolama hesabında üçüncü depolama hesabına bağlıdır. İlk Depolama hesabı başlatıldı, kabul ve sonraki başlatılmadan önce tamamlandı.

   ![sıralı dağıtım](./media/resource-manager-not-found-errors/deployment-events-sequence.png)

## <a name="solution-2---get-resource-from-different-resource-group"></a>Çözüm 2 - farklı bir kaynak grubundan kaynak Al

İçin dağıtılan olandan farklı bir kaynak grubundaki kaynak mevcut olduğunda kullanın [ResourceId işlevi](resource-group-template-functions-resource.md#resourceid) kaynak tam adını almak için.

```json
"properties": {
    "name": "[parameters('siteName')]",
    "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
}
```

## <a name="solution-3---check-reference-function"></a>Çözüm 3 - onay başvuru işlevi

İçeren bir ifade Ara [başvuru](resource-group-template-functions-resource.md#reference) işlevi. Sağladığınız değerleri kaynak aynı şablonu, kaynak grubu ve abonelik olup göre farklılık gösterir. Senaryonuz için gerekli parametre değerleri sağlayarak kontrol edin. Kaynak farklı bir kaynak grubu içinde ise, tam kaynak kimliğini sağlayın Örneğin, bir depolama hesabı başka bir kaynak grubuna başvurmak için kullanın:

```json
"[reference(resourceId('exampleResourceGroup', 'Microsoft.Storage/storageAccounts', 'myStorage'), '2017-06-01')]"
```