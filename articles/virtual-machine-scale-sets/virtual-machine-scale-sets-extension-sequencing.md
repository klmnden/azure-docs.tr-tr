---
title: Uzantı sıralama Azure sanal makine ölçek kümeleri ile kullanma | Microsoft Docs
description: Birden fazla uzantı, sanal makine ölçek kümesi dağıtırken uzantı sağlama sıra öğrenin.
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: drewm
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2019
ms.author: manayar
ms.openlocfilehash: 2e5dfda16c4828b3113fc50d4cffc79fe6ff19e8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60620181"
---
# <a name="sequence-extension-provisioning-in-virtual-machine-scale-sets"></a>Sıralı sanal makine ölçek kümesinde uzantı sağlama ayarlar
Azure sanal makine uzantıları, dağıtım sonrası yapılandırma ve yönetim, izleme, güvenlik ve daha fazlası gibi özellikler sunar. Üretim dağıtımları genellikle istenen sonuçları elde etmek için bir sanal makine örnekleri için yapılandırılmış birden fazla uzantı birleşimini kullanın.

Bir sanal makinede birden fazla uzantı kullanırken, aynı işletim sistemi kaynakları gerektiren uzantılar aynı anda bu kaynaklara almaya çalışırken olmayan sağlamak önemlidir. Bazı uzantılar, ortam ayarlarını ve gizli anahtarları gibi gerekli yapılandırmaları sağlamak için diğer uzantıları da bağlıdır. Doğru sıralama ve sıralama yerinde olmadan bağımlı uzantısı dağıtımları başarısız olabilir.

Bu makalede, sanal makine ölçek kümesi VM örnekleri için yapılandırılması için uzantıları nasıl sıra ayrıntıları.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, alışık olduğunuz varsayılır:
-   Azure sanal makine [uzantıları](../virtual-machines/extensions/overview.md)
-   [Değiştirme](virtual-machine-scale-sets-upgrade-scale-set.md) sanal makine ölçek kümeleri

## <a name="when-to-use-extension-sequencing"></a>Uzantı sıralama kullanıldığı durumlar
Uzantı sıralama içinde değil zorunlu ölçek için ayarlar ve herhangi bir sırada bir ölçek kümesi örneğinde belirtilmediği sürece, uzantıları sağlanabilir.

– ExtensionA ve modelde belirtilen ExtensionB – iki uzantıları, Ölçek kümesi modelinde varsa, örneğin, ardından aşağıdaki sağlama sıralarının ya da oluşabilir:
-   ExtensionA ExtensionB ->
-   ExtensionB ExtensionA ->

Uygulama Uzantısı genişletme B önce her zaman sağlanacak A gerektiriyorsa, bu makalede açıklandığı şekilde uzantı sıralama kullanmanız gerekir. Uzantı sıralama ile yalnızca bir sıralı artık meydana gelir:
-   ExtensionA - > ExtensionB

Tanımlanmış bir sağlama sırada belirtilmeyen herhangi bir uzantısı her zaman, öncesinde, sonra veya tanımlı bir dizi sırasında dahil olmak üzere sağlanabilir. Uzantı sıralama, yalnızca belirli bir uzantıya sonra başka bir özel uzantı sağlanacak belirtir. Modelde tanımlı başka bir uzantı sağlama etkilemez.

Örneğin, üç uzantılar – Uzantısı A, genişletme B ve modelde belirtilen uzantı C –, Ölçek kümesi modeline varsa ve sonra Uzantısı A sağlanacak uzantısı C ayarlayın, ardından aşağıdaki sağlama sıralarının ya da oluşabilir:
-   ExtensionA -> ExtensionC ExtensionB ->
-   ExtensionB -> ExtensionA ExtensionC ->
-   ExtensionA -> ExtensionB ExtensionC ->

Başka bir uzantı tanımlanmış uzantı dizisi yürütülürken sağlandığından emin olmak gerekirse, Ölçek kümesi modelinde tüm uzantıları sıralaması öneririz. Yukarıdaki örnekte, yalnızca bir sıralı gerçekleşebilir, sonra uzantı C sağlanacak genişletme B ayarlanabilir:
-   ExtensionA -> ExtensionC ExtensionB ->


## <a name="how-to-use-extension-sequencing"></a>Uzantı sıralama kullanma
Dizisi uzantısı hazırlanması, uzantı adları dizisi kabul "provisionAfterExtensions" özelliği eklemek için ölçek kümesi modelinde uzantısı tanımını güncelleştirmeniz gerekir. Özellik dizisi değeri belirtilen uzantılar ölçek kümesi modelinde tam olarak tanımlanması gerekir.

### <a name="template-deployment"></a>Şablon dağıtımı
Aşağıdaki örnek, uzantıları sırayla sağlanır, Ölçek kümesi – ExtensionA ExtensionB ve ExtensionC – üç uzantıları sahip olduğu bir şablon tanımlar:
-   ExtensionA -> ExtensionB ExtensionC ->

```json
"virtualMachineProfile": {
  "extensionProfile": {
    "extensions": [
      {
        "name": "ExtensionA",
        "properties": {
          "publisher": "ExtensionA.Publisher",
          "settings": {},
          "typeHandlerVersion": "1.0",
          "autoUpgradeMinorVersion": true,
          "type": "ExtensionA"
        }
      },
      {
        "name": "ExtensionB",
        "properties": {
          "provisionAfterExtensions": [
            "ExtensionA"
          ],
          "publisher": "ExtensionB.Publisher",
          "settings": {},
          "typeHandlerVersion": "2.0",
          "autoUpgradeMinorVersion": true,
          "type": "ExtensionB"
        }
      }, 
      {
        "name": "ExtensionC",
        "properties": {
          "provisionAfterExtensions": [
            "ExtensionB"
          ],
          "publisher": "ExtensionC.Publisher",
          "settings": {},
          "typeHandlerVersion": "3.0",
          "autoUpgradeMinorVersion": true,
          "type": "ExtensionC"                   
        }
      }
    ]
  }
}
```

' % S'özelliğinin "provisionAfterExtensions" uzantı adları dizisi kabul olduğundan, yukarıdaki örnek ExtensionC ExtensionB ExtensionA sonra sağlanır, ancak sıralama yok ExtensionB ExtensionA arasında gerekli şekilde değiştirilebilir. Aşağıdaki şablonu, bu senaryo elde etmek için kullanılabilir:

```json
"virtualMachineProfile": {
  "extensionProfile": {
    "extensions": [
      {
        "name": "ExtensionA",
        "properties": {
          "publisher": "ExtensionA.Publisher",
          "settings": {},
          "typeHandlerVersion": "1.0",
          "autoUpgradeMinorVersion": true,
          "type": "ExtensionA"
        }
      },
      {
        "name": "ExtensionB",
        "properties": {
          "publisher": "ExtensionB.Publisher",
          "settings": {},
          "typeHandlerVersion": "2.0",
          "autoUpgradeMinorVersion": true,
          "type": "ExtensionB"
        }
      }, 
      {
        "name": "ExtensionC",
        "properties": {
          "provisionAfterExtensions": [
            "ExtensionA","ExtensionB"
          ],
          "publisher": "ExtensionC.Publisher",
          "settings": {},
          "typeHandlerVersion": "3.0",
          "autoUpgradeMinorVersion": true,
          "type": "ExtensionC"                   
        }
      }
    ]
  }
}
```

### <a name="rest-api"></a>REST API
Aşağıdaki örnek, bir ölçek kümesi modeline ExtensionC adlı yeni bir uzantı ekler. ExtensionC, Ölçek kümesi modelinde zaten tanımlamış ExtensionA ve ExtensionB, bağımlılıkları vardır.

```
PUT on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/extensions/ExtensionC?api-version=2018-10-01`
```

```json
{ 
  "name": "ExtensionC",
  "properties": {
    "provisionAfterExtensions": [
      "ExtensionA","ExtensionB"
    ],
    "publisher": "ExtensionC.Publisher",
    "settings": {},
    "typeHandlerVersion": "3.0",
    "autoUpgradeMinorVersion": true,
    "type": "ExtensionC" 
  }                  
}
```

ExtensionC daha önce tanımlanan ölçek modeli ve bağımlılıklarını eklemek istediğiniz kümesini şimdi, yürütebilirsiniz bir `PATCH` zaten dağıtılmış bir uzantının özelliklerini düzenlemek için.

```
PATCH on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/extensions/ExtensionC?api-version=2018-10-01`
```

```json
{ 
  "properties": {
    "provisionAfterExtensions": [
      "ExtensionA","ExtensionB"
    ]
  }                  
}
```
Mevcut ölçek kümesi örneklerine değişiklikler sonraki uygulanan [yükseltme](virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model).

### <a name="azure-powershell"></a>Azure PowerShell
Kullanım [Ekle AzVmssExtension](/powershell/module/az.compute/add-azvmssextension) ölçek için uygulama durumunu uzantısı eklemek için cmdlet kümesi model tanımı. Uzantı sıralama Az PowerShell 1.2.0 veya üstü gerektirir.

Aşağıdaki örnek ekler [uygulama Uzantısın](virtual-machine-scale-sets-health-extension.md) için `extensionProfile` bir ölçek kümesi modelinde bir Windows tabanlı ölçek kümesi. Uygulama durumunu uzantısı sağladıktan sonra sağlanacak [özel betik uzantısı](../virtual-machines/extensions/custom-script-windows.md), Ölçek kümesindeki zaten tanımlı.

```azurepowershell-interactive
# Define the scale set variables
$vmScaleSetName = "myVMScaleSet"
$vmScaleSetResourceGroup = "myVMScaleSetResourceGroup"

# Define the Application Health extension properties
$publicConfig = @{"protocol" = "http"; "port" = 80; "requestPath" = "/healthEndpoint"};
$extensionName = "myHealthExtension"
$extensionType = "ApplicationHealthWindows"
$publisher = "Microsoft.ManagedServices"

# Get the scale set object
$vmScaleSet = Get-AzVmss `
  -ResourceGroupName $vmScaleSetResourceGroup `
  -VMScaleSetName $vmScaleSetName

# Add the Application Health extension to the scale set model
Add-AzVmssExtension -VirtualMachineScaleSet $vmScaleSet `
  -Name $extensionName `
  -Publisher $publisher `
  -Setting $publicConfig `
  -Type $extensionType `
  -TypeHandlerVersion "1.0" `
  -ProvisionAfterExtension "CustomScriptExtension" `
  -AutoUpgradeMinorVersion $True

# Update the scale set
Update-AzVmss -ResourceGroupName $vmScaleSetResourceGroup `
  -Name $vmScaleSetName `
  -VirtualMachineScaleSet $vmScaleSet
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Kullanım [az vmss uzantı kümesi](/cli/azure/vmss/extension#az-vmss-extension-set) için uygulama durumunu uzantısı eklemek için ölçek kümesi modeli tanımı. Uzantı sıralama 2.0.55 Azure CLI'ın veya üstü gerektirir.

Aşağıdaki örnek ekler [uygulama Uzantısın](virtual-machine-scale-sets-health-extension.md) ölçek kümesi modelinde bir Windows tabanlı ölçek kümesi. Uygulama durumunu uzantısı sağladıktan sonra sağlanacak [özel betik uzantısı](../virtual-machines/extensions/custom-script-windows.md), Ölçek kümesindeki zaten tanımlı.

```azurecli-interactive
az vmss extension set \
  --name ApplicationHealthWindows \
  --publisher Microsoft.ManagedServices \
  --version 1.0 \
  --resource-group <myVMScaleSetResourceGroup> \
  --vmss-name <myVMScaleSet> \
  --provision-after-extensions CustomScriptExtension \
  --settings ./extension.json
```


## <a name="troubleshoot"></a>Sorun giderme

### <a name="not-able-to-add-extension-with-dependencies"></a>Bağımlılıkları olan uzantısı eklemek karşılaştırılamıyor?
1. Ölçek kümesi modelinde provisionAfterExtensions içinde belirtilen uzantılar tanımlandığından emin olun.
2. Tanıtılan döngüsel bağımlılık emin olun. Örneğin, aşağıdaki sırayı izin verilmiyor: ExtensionA -> ExtensionB ExtensionC -> ExtensionA ->
3. Bağımlılıklar, aldığınız herhangi bir uzantısı bir "ayarlar" özelliği "Özellikler" uzantısı altında olduğundan emin olun. Örneğin, ExtentionB sonra ExtensionA sağlanması gerekiyorsa, ExtensionA ExtensionA "Özellikler" altındaki "ayarlar" alanında olması gerekir. Uzantı, gerekli tüm ayarları kılmaz boş "ayarlar" özelliğini belirtebilirsiniz.

### <a name="not-able-to-remove-extensions"></a>Uzantıları kaldırmak karşılaştırılamıyor?
Kaldırılmakta olan uzantıları için herhangi bir uzantısı provisionAfterExtensions altında listelenmeyen emin olun.

## <a name="next-steps"></a>Sonraki adımlar
Bilgi edinmek için nasıl [uygulamanızı dağıtmak](virtual-machine-scale-sets-deploy-app.md) üzerinde sanal makine ölçek kümeleri.
