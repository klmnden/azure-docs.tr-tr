---
title: Sanal makine ölçek kümeleri Azure PowerShell ile yönetme | Microsoft Docs
description: Sanal makine ölçek kümeleri örneğini durdurmak ve başlatmak nasıl gibi yönetmek veya ölçeği değiştirmek için ortak Azure PowerShell cmdlet'lerini kapasite ayarlayın.
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: d35fa77a-de96-4ccd-a332-eb181d1f4273
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2018
ms.author: iainfou
ms.openlocfilehash: 39cd7fa2232329716ba16abf92ba4a5f2cc15487
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34652789"
---
# <a name="manage-a-virtual-machine-scale-set-with-azure-powershell"></a>Azure PowerShell ile ayarlanmış bir sanal makine ölçek yönetme
Sanal makine ölçek kümesinin yaşam döngüsü boyunca bir veya daha fazla yönetim görevi çalıştırmanız gerekebilir. Ayrıca, çeşitli yaşam döngüsü görevlerini otomatikleştiren betikler oluşturmak isteyebilirsiniz. Bu makalede bu görevleri gerçekleştirmenize olanak sağlayan ortak Azure PowerShell cmdlet'lerini bazıları ayrıntılarını verir.

Bu yönetim görevleri tamamlamak için en son Azure PowerShell modülü gerekir. Bilgi için bkz: [Azure PowerShell ile çalışmaya başlama](/powershell/azure/get-started-azureps). Bir sanal makine ölçek kümesi oluşturmanız gerekiyorsa, yapabilecekleriniz [ölçeği Azure PowerShell ile Ayarla oluşturma](quick-create-powershell.md).


## <a name="view-information-about-a-scale-set"></a>Ölçek kümesi hakkında bilgi görüntüleyin
Ölçek kümesi hakkındaki genel bilgileri görüntülemek için kullanın [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss). Aşağıdaki örnek ölçeği adlandırılmış Ayarla hakkındaki bilgileri alır *myScaleSet* içinde *myResourceGroup* kaynak grubu. Aşağıdaki gibi kendi adlarınızı girin:

```powershell
Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```


## <a name="view-vms-in-a-scale-set"></a>Ölçek kümesindeki VM’leri görüntüleme
Ölçek kümesindeki VM örneği listesini görüntülemek için kullanın [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm). Aşağıdaki örnekte ölçeği adlandırılmış Ayarla tüm VM örnekleri listelenmiştir *myScaleSet* ve *myResourceGroup* kaynak grubu. Bu adları için kendi değerlerinizi girin:

```powershell
Get-AzureRmVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```

Belirli bir VM örneği hakkında ek bilgi görüntülemek için add `-InstanceId` parametresi [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) ve görüntülemek için bir örnek seçin. Aşağıdaki örnek görünümleri VM örneği hakkında bilgi *0* adlandırılmış kümesi ölçeğinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Aşağıdaki gibi kendi adlarınızı girin:

```powershell
Get-AzureRmVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="change-the-capacity-of-a-scale-set"></a>Ölçek kümesinin kapasitesini değiştirme
Yukarıdaki komutlar, Ölçek kümesi ve VM örnekleri hakkında bilgi gösterdi. Artırmak veya ölçek kümesindeki örneklerinin sayısını azaltmak için kapasite değiştirebilirsiniz. Ölçeği otomatik olarak ayarla oluşturur veya VM'ler gereken sayıda kaldırır ve sonra uygulama trafiği almaya VM'ler yapılandırır.

İlk olarak, [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) ile bir ölçek kümesi nesnesi oluşturun, ardından `sku.capacity` için yeni bir değer belirtin. Kapasite değişikliğini uygulamak için [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) kullanın. Aşağıdaki örnek güncelleştirmeleri *myScaleSet* içinde *myResourceGroup* kapasitesine kaynak grubuna *5* örnekleri. Kendi değerlerinizi aşağıdaki gibi belirtin:

```powershell
# Get current scale set
$vmss = Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"

# Set and update the capacity of your scale set
$vmss.sku.capacity = 5
Update-AzureRmVmss -ResourceGroupName "myResourceGroup" -Name "myScaleSet" -VirtualMachineScaleSet $vmss
```

Ölçek kümenizin kapasitesinin güncelleştirilmesi birkaç dakika sürer. Bir ölçek kapasitesini azaltırsanız kimlikleri ilk kaldırılır en yüksek örnek VM'lerin ayarlayın.


## <a name="stop-and-start-vms-in-a-scale-set"></a>Durdurun ve ölçek kümesindeki sanal makineleri Başlat
Ölçek kümesindeki bir veya birden çok sanal makineyi durdurmak için [Stop-AzureRmVmss](/powershell/module/azurerm.compute/stop-azurermvmss) kullanın. `-InstanceId` parametresi, durdurulacak bir veya daha fazla sanal makine belirtmenize olanak sağlar. Örnek kimliği belirtmezseniz, ölçek kümesindeki tüm sanal makineler durdurulur. Birden çok VM durdurmak için her örnek kimliği virgül ile ayırın.

Aşağıdaki örnek örneği durdurur *0* adlandırılmış kümesi ölçeğinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Kendi değerlerinizi aşağıdaki gibi belirtin:

```powershell
Stop-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```

Varsayılan olarak, durdurulan sanal makineler serbest bırakılır ve bunlar için işlem ücreti alınmaz. Durdurulan sanal makinenin sağlama durumunda kalmasını istiyorsanız, önceki komuta `-StayProvisioned` parametresini ekleyin. Sağlama durumunda tutulan durdurulmuş sanal makineler için normal işlem ücreti alınır.


### <a name="start-vms-in-a-scale-set"></a>Ölçek kümesindeki sanal makineleri Başlat
Ölçek kümesindeki bir veya birden çok sanal makineyi başlatmak için [Start-AzureRmVmss](/powershell/module/azurerm.compute/start-azurermvmss) kullanın. `-InstanceId` parametresi, başlatılacak bir veya daha fazla sanal makine belirtmenize olanak sağlar. Örnek kimliği belirtmezseniz, ölçek kümesindeki tüm sanal makineler başlatılır. Birden çok VM başlatmak için her örnek kimliği virgül ile ayırın.

Aşağıdaki örnekte bir örneğini başlatır *0* adlandırılmış kümesi ölçeğinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Kendi değerlerinizi aşağıdaki gibi belirtin:

```powershell
Start-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="restart-vms-in-a-scale-set"></a>Ölçek kümesindeki sanal makineleri yeniden başlatın
Ölçek kümesindeki bir veya birden çok sanal makineyi yeniden başlatmak için [Restart-AzureRmVmss](/powershell/module/azurerm.compute/restart-azurermvmss) kullanın. `-InstanceId` parametresi, yeniden başlatılacak bir veya daha fazla sanal makine belirtmenize olanak sağlar. Örnek kimliği belirtmezseniz, ölçek kümesindeki tüm sanal makineler yeniden başlatılır. Birden çok VM yeniden başlatmak için her örnek kimliği virgül ile ayırın.

Aşağıdaki örnek örneği yeniden *0* adlandırılmış kümesi ölçeğinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Kendi değerlerinizi aşağıdaki gibi belirtin:

```powershell
Restart-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="remove-vms-from-a-scale-set"></a>Ölçek kümesindeki sanal makineleri Kaldır
Bir veya daha fazla VM ölçek kümesindeki kaldırmak için kullanın [Kaldır AzureRmVmss](/powershell/module/azurerm.compute/remove-azurermvmss). `-InstanceId` Parametresi kaldırmak için bir veya daha fazla VM belirtmenize olanak verir. Bir örnek kimliği belirtmezseniz, Ölçek kümesindeki tüm VM'ler kaldırılır. Birden çok VM kaldırmak için her örnek kimliği virgül ile ayırın.

Aşağıdaki örnek, örnek kaldırır *0* adlandırılmış kümesi ölçeğinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Kendi değerlerinizi aşağıdaki gibi belirtin:

```powershell
Remove-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="next-steps"></a>Sonraki adımlar
Diğer ortak görevler ölçek kümeleri için nasıl [bir uygulamayı dağıtmak](virtual-machine-scale-sets-deploy-app.md), ve [yükseltme VM örnekleri](virtual-machine-scale-sets-upgrade-scale-set.md). Azure PowerShell de kullanabilirsiniz [otomatik ölçeklendirme kurallarını yapılandırma](virtual-machine-scale-sets-autoscale-overview.md).
