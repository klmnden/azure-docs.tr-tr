---
title: "Sanal makine ölçek kümeleri Azure PowerShell ile yönetme | Microsoft Docs"
description: "Sanal makine ölçek kümeleri örneğini durdurmak ve başlatmak nasıl gibi yönetmek veya ölçeği değiştirmek için ortak Azure PowerShell cmdlet'lerini kapasite ayarlayın."
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: d35fa77a-de96-4ccd-a332-eb181d1f4273
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: iainfou
ms.openlocfilehash: a661aa5a555dacac5c94c3feb8c6b88bb5033f83
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2017
---
# <a name="manage-a-virtual-machine-scale-set-with-azure-powershell"></a>Azure PowerShell ile ayarlanmış bir sanal makine ölçek yönetme
Bir sanal makine ölçek kümesi yaşam döngüsü boyunca, bir veya daha fazla yönetim görevleri çalıştırmanız gerekebilir. Ayrıca, çeşitli yaşam döngüsü görevleri otomatikleştiren komut dosyaları oluşturmak isteyebilirsiniz. Bu makalede bu görevleri gerçekleştirmenize olanak sağlayan ortak Azure PowerShell cmdlet'lerini bazıları ayrıntılarını verir.

Bu yönetim görevleri tamamlamak için en son Azure PowerShell modülü gerekir. Yükleme ve en son sürümünü kullanma hakkında daha fazla bilgi için bkz: [Azure PowerShell ile çalışmaya başlama](/powershell/azure/get-started-azureps). Bir sanal makine ölçek kümesi oluşturmanız gerekiyorsa, yapabilecekleriniz [ölçeği Azure portalında Ayarla oluşturma](virtual-machine-scale-sets-portal-create.md).


## <a name="view-information-about-a-scale-set"></a>Ölçek kümesi hakkında bilgi görüntüleyin
Ölçek kümesi hakkındaki genel bilgileri görüntülemek için kullanın [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss). Aşağıdaki örnek ölçeği adlandırılmış Ayarla hakkındaki bilgileri alır *myScaleSet* içinde *myResourceGroup* kaynak grubu. Aşağıdaki gibi kendi adlarınızı girin:

```powershell
Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```


## <a name="view-vms-in-a-scale-set"></a>Görünüm VM ölçek kümesindeki
Ölçek kümesindeki VM örneği listesini görüntülemek için kullanın [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm). Aşağıdaki örnek ölçeği adlandırılmış Ayarla tüm VM örnekleri listesi *myScaleSet* ve *myResourceGroup* kaynak grubu. Bu adları için kendi değerlerinizi girin:

```powershell
Get-AzureRmVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```

Belirli bir VM örneği hakkında ek bilgi görüntülemek için add `-InstanceId` parametresi [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) ve görüntülemek için bir örnek seçin. Aşağıdaki örnek görünümleri VM örneği hakkında bilgi *0* adlandırılmış kümesi ölçeğinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Aşağıdaki gibi kendi adlarınızı girin:

```powershell
Get-AzureRmVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="change-the-capacity-of-a-scale-set"></a>Ölçek kümesi kapasitesi değiştirme
Yukarıdaki komutlar, Ölçek kümesi ve VM örnekleri hakkında bilgi gösterdi. Artırmak veya ölçek kümesindeki örneklerinin sayısını azaltmak için kapasite değiştirebilirsiniz. Ölçeği otomatik olarak ayarla oluşturur veya VM'ler gereken sayıda kaldırır ve sonra uygulama trafiği almaya VM'ler yapılandırır.

İlk olarak, bir ölçek kümesi nesnesi oluşturun [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss), için yeni bir değer belirtin `sku.capacity`. Kapasite değişikliği uygulamak için kullanmak [güncelleştirme AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss). Aşağıdaki örnek güncelleştirmeleri *myScaleSet* içinde *myResourceGroup* kapasitesine kaynak grubuna *5* örnekleri. Kendi değerlerinizi aşağıdaki gibi belirtin:

```powershell
# Get current scale set
$vmss = Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"

# Set and update the capacity of your scale set
$vmss.sku.capacity = 5
Update-AzureRmVmss -ResourceGroupName "myResourceGroup" -Name "myScaleSet" -VirtualMachineScaleSet $vmss 
```

Kapasite, Ölçek güncelleştirilmesi birkaç dakika bir ayarlarsanız. Bir ölçek kapasitesini azaltırsanız kimlikleri ilk kaldırılır en yüksek örnek VM'lerin ayarlayın.


## <a name="stop-and-start-vms-in-a-scale-set"></a>Durdurun ve ölçek kümesindeki sanal makineleri Başlat
Ölçek kümesindeki bir veya daha fazla sanal makineleri durdurmak için kullanma [Stop-AzureRmVmss](/powershell/module/azurerm.compute/stop-azurermvmss). `-InstanceId` Parametresi durdurmak için bir veya daha fazla VM belirtmenize olanak verir. Bir örnek kimliği belirtmezseniz, Ölçek kümesindeki tüm VM'ler durdurulur. Birden çok VM durdurmak için her örnek kimliği virgül ile ayırın.

Aşağıdaki örnek örneği durdurur *0* adlandırılmış kümesi ölçeğinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Değerlerinizi aşağıdaki gibi belirtin:

```powershell
Stop-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```

Varsayılan olarak, durdurulmuş VM'ler serbest ve bilgi işlem ücretleri değil. VM durdurulduğunda sağlanan bir durumda kalır, eklemek isterseniz, `-StayProvisioned` yukarıdaki komut parametresi. Sağlanan kalır durdurulmuş VM'ler normal bilgi işlem ücretleri.


### <a name="start-vms-in-a-scale-set"></a>Ölçek kümesindeki sanal makineleri Başlat
Ölçek kümesindeki bir veya daha fazla VM başlatmak için kullanmak [başlangıç AzureRmVmss](/powershell/module/azurerm.compute/start-azurermvmss). `-InstanceId` Parametresi başlatmak için bir veya daha fazla VM belirtmenize olanak verir. Bir örnek kimliği belirtmezseniz, Ölçek kümesindeki tüm VM'ler başlatılır. Birden çok VM başlatmak için her örnek kimliği virgül ile ayırın.

Aşağıdaki örnekte bir örneğini başlatır *0* adlandırılmış kümesi ölçeğinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Değerlerinizi aşağıdaki gibi belirtin:

```powershell
Start-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="restart-vms-in-a-scale-set"></a>Ölçek kümesindeki sanal makineleri yeniden başlatın
Bir veya daha fazla VM ölçek kümesindeki yeniden başlatmak için kullanın [Retart AzureRmVmss](/powershell/module/azurerm.compute/restart-azurermvmss). `-InstanceId` Parametresi yeniden başlatmak için bir veya daha fazla VM belirtmenize olanak verir. Bir örnek kimliği belirtmezseniz, Ölçek grubundaki tüm sanal makineleri yeniden başlatılır. Birden çok VM yeniden başlatmak için her örnek kimliği virgül ile ayırın.

Aşağıdaki örnek örneği yeniden *0* adlandırılmış kümesi ölçeğinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Değerlerinizi aşağıdaki gibi belirtin:

```powershell
Restart-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="remove-vms-from-a-scale-set"></a>Ölçek kümesindeki sanal makineleri Kaldır
Bir veya daha fazla VM ölçek kümesindeki kaldırmak için kullanın [Kaldır AzureRmVmss](/powershell/module/azurerm.compute/remove-azurermvmss). `-InstanceId` Parametresi kaldırmak için bir veya daha fazla VM belirtmenize olanak verir. Bir örnek kimliği belirtmezseniz, Ölçek kümesindeki tüm VM'ler kaldırılır. Birden çok VM kaldırmak için her örnek kimliği virgül ile ayırın.

Aşağıdaki örnek, örnek kaldırır *0* adlandırılmış kümesi ölçeğinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Değerlerinizi aşağıdaki gibi belirtin:

```powershell
Remove-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="next-steps"></a>Sonraki adımlar
Diğer ortak görevler ölçek kümeleri için nasıl [bir uygulamayı dağıtmak](virtual-machine-scale-sets-deploy-app.md), ve [yükseltme VM örnekleri](virtual-machine-scale-sets-upgrade-scale-set.md). Azure PowerShell de kullanabilirsiniz [otomatik ölçeklendirme kurallarını yapılandırma](virtual-machine-scale-sets-autoscale-overview.md).
