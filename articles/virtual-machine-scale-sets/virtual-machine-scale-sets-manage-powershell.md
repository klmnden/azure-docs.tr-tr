---
title: Sanal makine ölçek kümeleri Azure PowerShell ile yönetme | Microsoft Docs
description: Örneğini durdurmak ve başlatmak gibi nasıl sanal makine ölçek kümeleri, yönetme veya ölçeği değiştirmek, ortak Azure PowerShell cmdlet'lerini kapasitesini ayarlayın.
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
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
ms.author: cynthn
ms.openlocfilehash: 32bcc87cad23c8a9145e2104794701997fca8998
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54883271"
---
# <a name="manage-a-virtual-machine-scale-set-with-azure-powershell"></a>Bir sanal makine ölçek kümesini Azure PowerShell ile yönetme
Sanal makine ölçek kümesinin yaşam döngüsü boyunca bir veya daha fazla yönetim görevi çalıştırmanız gerekebilir. Ayrıca, çeşitli yaşam döngüsü görevlerini otomatikleştiren betikler oluşturmak isteyebilirsiniz. Bu makalede bu görevleri gerçekleştirmenize olanak tanıyan ortak Azure PowerShell cmdlet'lerini bazıları ayrıntılı olarak açıklanmaktadır.

Bu yönetim görevleri tamamlamak için en son Azure PowerShell modülü gerekir. Bilgi için [Azure PowerShell'i kullanmaya başlama](/powershell/azure/get-started-azureps). Bir sanal makine ölçek kümesi oluşturmak için ihtiyacınız varsa, [Azure PowerShell ile bir ölçek kümesi oluşturma](quick-create-powershell.md).


## <a name="view-information-about-a-scale-set"></a>Bir ölçek kümesi hakkındaki bilgileri görüntüleme
Bir ölçek kümesi hakkında genel bilgileri görüntülemek için kullanın [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss). Aşağıdaki örnekte adlı ölçek kümesi hakkında bilgi alır *myScaleSet* içinde *myResourceGroup* kaynak grubu. Şu şekilde kendi adlarınızı girin:

```powershell
Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```


## <a name="view-vms-in-a-scale-set"></a>Ölçek kümesindeki VM’leri görüntüleme
Bir ölçek kümesindeki sanal makine örneği listesini görüntülemek için kullanın [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm). Aşağıdaki örnekte adlı ölçek kümesi içinde tüm VM örnekleri listelenmiştir *myScaleSet* ve *myResourceGroup* kaynak grubu. Bu adları için kendi değerlerinizi sağlayın:

```powershell
Get-AzureRmVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```

Belirli bir sanal makine örneği hakkında ek bilgi görüntülemek için Ekle `-InstanceId` parametresi [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) ve görüntülemek için bir örnek belirtin. Aşağıdaki örnek, sanal makine örneği hakkında bilgi görüntüler *0* adlı ölçek kümesi içinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Şu şekilde kendi adlarınızı girin:

```powershell
Get-AzureRmVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="change-the-capacity-of-a-scale-set"></a>Ölçek kümesinin kapasitesini değiştirme
Yukarıdaki komutların ölçek kümenizi ve sanal makine örnekleri hakkında bilgi gösterdi. Artırabilir veya ölçek kümesindeki örneklerin sayısını azaltmak için kapasiteyi değiştirebilirsiniz. Ölçek kümesini otomatik olarak oluşturur veya gerekli VM sayısını kaldırır ve ardından uygulama trafiği almak için sanal makineleri yapılandırır.

İlk olarak, [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) ile bir ölçek kümesi nesnesi oluşturun, ardından `sku.capacity` için yeni bir değer belirtin. Kapasite değişikliğini uygulamak için [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) kullanın. Aşağıdaki örnek güncelleştirmeleri *myScaleSet* içinde *myResourceGroup* kapasitesi kaynak grubuna *5* örnekleri. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```powershell
# Get current scale set
$vmss = Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"

# Set and update the capacity of your scale set
$vmss.sku.capacity = 5
Update-AzureRmVmss -ResourceGroupName "myResourceGroup" -Name "myScaleSet" -VirtualMachineScaleSet $vmss
```

Ölçek kümenizin kapasitesinin güncelleştirilmesi birkaç dakika sürer. Bir ölçek kapasitesini azaltırsanız kimlikleri ilk önce kaldırılır en yüksek örnek ile Vm'leri ayarlayın.


## <a name="stop-and-start-vms-in-a-scale-set"></a>VM ölçek kümesindeki durdurup
Ölçek kümesindeki bir veya birden çok sanal makineyi durdurmak için [Stop-AzureRmVmss](/powershell/module/azurerm.compute/stop-azurermvmss) kullanın. `-InstanceId` parametresi, durdurulacak bir veya daha fazla sanal makine belirtmenize olanak sağlar. Örnek kimliği belirtmezseniz, ölçek kümesindeki tüm sanal makineler durdurulur. Birden çok VM durdurmak için her örnek kimliği virgül ile ayırın.

Aşağıdaki örnek örneği durdurur *0* adlı ölçek kümesi içinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```powershell
Stop-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```

Varsayılan olarak, durdurulan sanal makineler serbest bırakılır ve bunlar için işlem ücreti alınmaz. Durdurulan sanal makinenin sağlama durumunda kalmasını istiyorsanız, önceki komuta `-StayProvisioned` parametresini ekleyin. Sağlama durumunda tutulan durdurulmuş sanal makineler için normal işlem ücreti alınır.


### <a name="start-vms-in-a-scale-set"></a>Bir ölçek kümesindeki VM'lerin başlatma
Ölçek kümesindeki bir veya birden çok sanal makineyi başlatmak için [Start-AzureRmVmss](/powershell/module/azurerm.compute/start-azurermvmss) kullanın. `-InstanceId` parametresi, başlatılacak bir veya daha fazla sanal makine belirtmenize olanak sağlar. Örnek kimliği belirtmezseniz, ölçek kümesindeki tüm sanal makineler başlatılır. Birden çok VM başlatmak için her örnek kimliği virgül ile ayırın.

Aşağıdaki örnek bir örneğini başlatır *0* adlı ölçek kümesi içinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```powershell
Start-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="restart-vms-in-a-scale-set"></a>Bir ölçek kümesindeki Vm'leri yeniden başlatma
Ölçek kümesindeki bir veya birden çok sanal makineyi yeniden başlatmak için [Restart-AzureRmVmss](/powershell/module/azurerm.compute/restart-azurermvmss) kullanın. `-InstanceId` parametresi, yeniden başlatılacak bir veya daha fazla sanal makine belirtmenize olanak sağlar. Örnek kimliği belirtmezseniz, ölçek kümesindeki tüm sanal makineler yeniden başlatılır. Birden çok VM'yi yeniden başlatmak için her örnek kimliği virgül ile ayırın.

Aşağıdaki örnek örneğini yeniden başlatır *0* adlı ölçek kümesi içinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```powershell
Restart-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="remove-vms-from-a-scale-set"></a>VM ölçek kümesinden Kaldır
Bir ölçek kümesindeki bir veya daha fazla sanal makine kaldırmak için [Remove-AzureRmVmss](/powershell/module/azurerm.compute/remove-azurermvmss). `-InstanceId` Parametresi, kaldırmak için bir veya daha fazla sanal makine belirtmenize olanak sağlar. Örnek kimliği belirtmezseniz, Ölçek kümesindeki tüm sanal makineler kaldırılır. Birden çok VM kaldırmak için her örnek kimliği virgül ile ayırın.

Aşağıdaki örnek, örnek kaldırır *0* adlı ölçek kümesi içinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```powershell
Remove-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="next-steps"></a>Sonraki adımlar
Ölçek kümeleri için sık kullanılan diğer görevler nasıl [uygulama dağıtma](virtual-machine-scale-sets-deploy-app.md), ve [yükseltme sanal makine örnekleri](virtual-machine-scale-sets-upgrade-scale-set.md). Azure PowerShell'i de kullanabilirsiniz [otomatik ölçeklendirme kuralları yapılandırma](virtual-machine-scale-sets-autoscale-overview.md).
