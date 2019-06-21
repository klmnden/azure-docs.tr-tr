---
title: Azure kaynak bulunamadı hatası | Microsoft Docs
description: Bir kaynak bulunamadığında hataları gidermek nasıl açıklar.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: troubleshooting
ms.date: 06/06/2018
ms.author: tomfitz
ms.openlocfilehash: 9c999a70ffdbed0c954cfc960b5febdaff06e4ae
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67206195"
---
# <a name="resolve-not-found-errors-for-azure-resources"></a>Azure kaynakları için bulunamadı hataları çözün

Bu makalede, dağıtım sırasında bir kaynağı bulunamıyor görebileceğiniz hatalar açıklanır.

## <a name="symptom"></a>Belirti

Şablonunuz, çözümlenemeyen bir kaynağın adını içerdiğinde, benzer bir hata alırsınız:

```
Code=NotFound;
Message=Cannot find ServerFarm with name exampleplan.
```

Kullanırsanız [başvuru](resource-group-template-functions-resource.md#reference) veya [Listkeys'i](resource-group-template-functions-resource.md#listkeys) aşağıdaki hata iletisini çözümlenemeyen kaynağı olan işlevler:

```
Code=ResourceNotFound;
Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource
group {resource group name} was not found.
```

## <a name="cause"></a>Nedeni

Resource Manager kaynak özelliklerini almak gerekiyor, ancak aboneliğinizde kaynak tanımlanamıyor.

## <a name="solution-1---set-dependencies"></a>Çözüm 1 - kümesi bağımlılıkları

Şablon eksik kaynak dağıtmaya çalışıyorsanız, bir bağımlılık eklemek gerekli olup olmadığını denetleyin. Resource Manager dağıtım, mümkün olduğunda paralel olarak kaynakları oluşturarak iyileştirir. Bir kaynak başka bir kaynak sonra dağıtılması gerekiyorsa kullanmanız gerekir **dependsOn** şablonunuzdaki öğesi. Örneğin, bir web uygulaması dağıtım yaparken, App Service planı mevcut olması gerekir. Web uygulamasını App Service planı üzerinde bağlıdır belirtmediyseniz, Resource Manager, aynı anda hem kaynakları oluşturur. Web uygulamasında bir özellik ayarlama girişiminde bulunduğunuzda henüz yok çünkü App Service planı kaynak bulunamadığını belirten bir hatayla alın. Bu hata, web uygulamasında bağımlılık ayarlayarak engeller.

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

Ancak, gerekmeyen bağımlılıklarını ayarlama önlemek isteyebilirsiniz. Gereksiz bağımlılıkları varsa, paralel olarak dağıtılan birbirlerine bağımlı olmayan kaynakları engelleyerek dağıtım süresini uzatmak. Ayrıca, dağıtımı engelleyen döngüsel bağımlılıklar oluşturabilir. [Başvuru](resource-group-template-functions-resource.md#reference) işlevi ve [listesi *](resource-group-template-functions-resource.md#list) işlevleri bu kaynağa aynı şablonda dağıtılır ve onun adı (kaynak kimliği değil'tarafından başvurulan bu örtülü bir bağımlılık başvurulan kaynakta oluşturur ). Bu nedenle, bağımlılıkları belirtilenden daha fazla bağımlılıkları olabilir **dependsOn** özelliği. [ResourceId](resource-group-template-functions-resource.md#resourceid) işlevi değil ya da örtük bir bağımlılık oluşturmak kaynağın mevcut olduğunu doğrulayın. [Başvuru](resource-group-template-functions-resource.md#reference) işlevi ve [listesi *](resource-group-template-functions-resource.md#list) kaynağa kendi kaynak kimliği tarafından başvurulan işlevler dolaylı bir bağımlılığı oluşturmayın Örtük bir bağımlılık oluşturmak için aynı şablonda dağıtılan kaynağın adını geçirin.

Bağımlılık sorunlarını gördüğünüzde, kaynak dağıtım sıralamasını bir anlayış kazanmak gerekir. Dağıtım işlemlerini sırasını görüntülemek için:

1. Kaynak grubunuz için dağıtım geçmişini seçin.

   ![Dağıtım geçmişi seçin](./media/resource-manager-not-found-errors/select-deployment.png)

2. Bir dağıtım geçmişinden seçip **olayları**.

   ![Dağıtım olayları seçin](./media/resource-manager-not-found-errors/select-deployment-events.png)

3. Her kaynak için olaylar dizisini inceleyin. Her işlemin durumu dikkat edin. Örneğin, aşağıdaki görüntüde, paralel olarak dağıtılan üç depolama hesabı gösterilmektedir. Üç depolama hesabı aynı anda başlatılır dikkat edin.

   ![Paralel dağıtım](./media/resource-manager-not-found-errors/deployment-events-parallel.png)

   Sıradaki resimde, paralel olarak dağıtılan olmayan üç depolama hesaplarını gösterir. İlk Depolama hesabında ikinci depolama hesabına bağlıdır ve üçüncü depolama hesabının ikinci depolama hesabına bağlıdır. İlk Depolama hesabı başlatıldığından, kabul edilen ve sonraki başlatılmadan önce tamamlandı.

   ![sıralı dağıtım](./media/resource-manager-not-found-errors/deployment-events-sequence.png)

## <a name="solution-2---get-resource-from-different-resource-group"></a>Çözüm 2 - farklı bir kaynak grubundan kaynak alma

Farklı bir kaynak grubu için Dağıtılmış bir kaynak var, kullanın [ResourceId işlevi](resource-group-template-functions-resource.md#resourceid) kaynağın tam olarak nitelenmiş adını almak için.

```json
"properties": {
    "name": "[parameters('siteName')]",
    "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
}
```

## <a name="solution-3---check-reference-function"></a>Çözüm 3 - denetimi başvuru işlevi

İçeren bir ifade için konum [başvuru](resource-group-template-functions-resource.md#reference) işlevi. Sağladığınız değerler kaynak aynı şablonun, kaynak grubu ve abonelik olup göre değişir. Çifte denetim senaryonuz için gerekli parametre değerlerini sunuyorsunuz. Farklı kaynak grubunda kaynak ise tam kaynak kimliği sağlayın. Örneğin, başka bir kaynak grubuna bir depolama hesabında başvurmak için kullanın:

```json
"[reference(resourceId('exampleResourceGroup', 'Microsoft.Storage/storageAccounts', 'myStorage'), '2017-06-01')]"
```