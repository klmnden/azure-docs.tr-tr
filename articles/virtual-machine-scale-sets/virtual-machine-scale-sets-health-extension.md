---
title: Uygulama durumunu uzantısını Azure sanal makine ölçek kümeleri ile kullanma | Microsoft Docs
description: Sanal makine ölçek kümeleri üzerinde dağıtılan uygulamalarınızı durumunu izlemek için uygulama durumunu uzantısını kullanmayı öğrenin.
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
ms.openlocfilehash: d1cff1011e190e5fbb2874657cbdfbdc68bde0c0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60619833"
---
# <a name="using-application-health-extension-with-virtual-machine-scale-sets"></a>Uygulama durumunu uzantısı ile sanal makine ölçek kümeleri
Uygulamanızın sistem durumu izleme, yönetme ve yükseltme dağıtımınız için önemli bir sinyaldir. Azure sanal makine ölçek kümeleri için destek sağlar [toplu yükseltmeler](virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model) dahil olmak üzere [otomatik işletim sistemi görüntüsü yükseltme](virtual-machine-scale-sets-automatic-upgrade.md), dağıtımınızı yükseltmek için tek tek örneklerini sistem durumu izlemeye güvenin .

Bu makalede, sanal makine ölçek kümeleri üzerinde dağıtılan uygulamalarınızı durumunu izlemek için uygulama durumunu uzantıyı nasıl kullanabileceğiniz açıklanır.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, aşina olduğunuzu varsayar:
-   Azure sanal makine [uzantıları](../virtual-machines/extensions/overview.md)
-   [Değiştirme](virtual-machine-scale-sets-upgrade-scale-set.md) sanal makine ölçek kümeleri

## <a name="when-to-use-the-application-health-extension"></a>Uygulama durumunu uzantıyı kullanmak ne zaman
Uygulama durumunu uzantısı, sanal makine ölçek kümesi örneği ve sistem durumu VM ölçek kümesi örneğinin içinde raporlarda içinde dağıtılır. Bir uygulama uç noktaya yoklama ve uygulamanın bu örneği üzerinde durumunu güncelleştirmek için uzantısını yapılandırabilirsiniz. Bu örneği durumu örneği yükseltme işlemleri için uygun olup olmadığını belirlemek için Azure tarafından denetlenir.

Uzantı raporları sistem bir VM içinde uzantı, dış araştırmalarının uygulama sistem durumu Araştırmalarının gibi durumlarda kullanılabilir (özel Azure Load Balancer kullanan [araştırmaları](../load-balancer/load-balancer-custom-probe-overview.md)) kullanılamaz.

## <a name="extension-schema"></a>Uzantı şeması

Uygulama durumunu Uzantı Şeması aşağıdaki JSON'u göstermektedir. Uzantı "tcp" veya "http" istek ile ilişkili bir bağlantı noktası veya istek yolu en az gerektirir.

```json
{
  "type": "extensions",
  "name": "HealthExtension",
  "apiVersion": "2018-10-01",
  "location": "<location>",  
  "properties": {
    "publisher": "Microsoft.ManagedServices",
    "type": "< ApplicationHealthLinux or ApplicationHealthWindows>",
    "autoUpgradeMinorVersion": true,
    "typeHandlerVersion": "1.0",
    "settings": {
      "protocol": "<protocol>",
      "port": "<port>",
      "requestPath": "</requestPath>"
    }
  }
}  
```

### <a name="property-values"></a>Özellik değerleri

| Ad | Değer / örnek | Veri Türü
| ---- | ---- | ---- 
| apiVersion | `2018-10-01` | date |
| Yayımcı | `Microsoft.ManagedServices` | string |
| type | `ApplicationHealthLinux` (Linux), `ApplicationHealthWindows` (Windows) | string |
| typeHandlerVersion | `1.0` | int |

### <a name="settings"></a>Ayarlar

| Ad | Değer / örnek | Veri Türü
| ---- | ---- | ----
| protokol | `http` veya `tcp` | string |
| port | Protokol olduğunda isteğe bağlı `http`zorunlu Protokolü `tcp` | int |
| requestPath | Protokol olduğunda zorunlu `http`, protokol olduğunda izin verilmiyor `tcp` | string |

## <a name="deploy-the-application-health-extension"></a>Uygulama durumunu uzantısı dağıtma
Ölçek kümenizi uzantısı, aşağıdaki örneklerde açıklandığı ayarlar uygulama durumunu dağıtma birden çok yolu vardır.

### <a name="rest-api"></a>REST API

Aşağıdaki örnek uygulama durumunu uzantısıyla (adı myHealthExtension) ölçek kümesi modelinde bir Windows tabanlı ölçek kümesinin extensionprofile öğesine ekler.

```
PUT on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/extensions/myHealthExtension?api-version=2018-10-01`
```

```json
{
  "name": "myHealthExtension",
  "properties": {
    "publisher": " Microsoft.ManagedServices",
    "type": "< ApplicationHealthWindows>",
    "autoUpgradeMinorVersion": true,
    "typeHandlerVersion": "1.0",
    "settings": {
      "protocol": "<protocol>",
      "port": "<port>",
      "requestPath": "</requestPath>"
    }
  }
}
```
Kullanım `PATCH` zaten dağıtılmış bir uzantı düzenlenecek.

### <a name="azure-powershell"></a>Azure PowerShell

Kullanım [Ekle AzVmssExtension](/powershell/module/az.compute/add-azvmssextension) ölçek için uygulama durumunu uzantısı eklemek için cmdlet kümesi model tanımı.

Aşağıdaki örnek uygulama durumunu uzantısını ekler `extensionProfile` ölçek kümesi modelinde bir Windows tabanlı ölçek kümesi. Örneğin, yeni Az PowerShell modülü kullanır.

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
  -AutoUpgradeMinorVersion $True

# Update the scale set
Update-AzVmss -ResourceGroupName $vmScaleSetResourceGroup `
  -Name $vmScaleSetName `
  -VirtualMachineScaleSet $vmScaleSet
```


### <a name="azure-cli-20"></a>Azure CLI 2.0

Kullanım [az vmss uzantı kümesi](/cli/azure/vmss/extension#az-vmss-extension-set) için uygulama durumunu uzantısı eklemek için ölçek kümesi modeli tanımı.

Aşağıdaki örnek, Ölçek kümesi modeline Windows tabanlı ölçek kümesinin uygulama durumunu uzantısını ekler.

```azurecli-interactive
az vmss extension set \
  --name ApplicationHealthWindows \
  --publisher Microsoft.ManagedServices \
  --version 1.0 \
  --resource-group <myVMScaleSetResourceGroup> \
  --vmss-name <myVMScaleSet> \
  --settings ./extension.json
```


## <a name="troubleshoot"></a>Sorun giderme
Uzantı yürütme çıkış dosyaları aşağıdaki dizinlerde bulunan kaydedilir:

```Windows
C:\WindowsAzure\Logs\Plugins\Microsoft.ManagedServices.ApplicationHealthWindows\<version>\
```

```Linux
/var/lib/waagent/apphealth
```

Günlükler uygulama sistem durumu da düzenli aralıklarla yakalayın.

## <a name="next-steps"></a>Sonraki adımlar
Bilgi edinmek için nasıl [uygulamanızı dağıtmak](virtual-machine-scale-sets-deploy-app.md) üzerinde sanal makine ölçek kümeleri.
