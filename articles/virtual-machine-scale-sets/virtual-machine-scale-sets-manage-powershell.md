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
ms.openlocfilehash: a6474320fd8b1545d61320cd43e155ab077ba310
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64683536"
---
# <a name="manage-a-virtual-machine-scale-set-with-azure-powershell"></a>Bir sanal makine ölçek kümesini Azure PowerShell ile yönetme

Sanal makine ölçek kümesinin yaşam döngüsü boyunca bir veya daha fazla yönetim görevi çalıştırmanız gerekebilir. Ayrıca, çeşitli yaşam döngüsü görevlerini otomatikleştiren betikler oluşturmak isteyebilirsiniz. Bu makalede bu görevleri gerçekleştirmenize olanak tanıyan ortak Azure PowerShell cmdlet'lerini bazıları ayrıntılı olarak açıklanmaktadır.

Bir sanal makine ölçek kümesi oluşturmak için ihtiyacınız varsa, [Azure PowerShell ile bir ölçek kümesi oluşturma](quick-create-powershell.md).

[!INCLUDE [updated-for-az.md](../../includes/updated-for-az.md)]

## <a name="view-information-about-a-scale-set"></a>Bir ölçek kümesi hakkındaki bilgileri görüntüleme
Bir ölçek kümesi hakkında genel bilgileri görüntülemek için kullanın [Get-AzVmss](/powershell/module/az.compute/get-azvmss). Aşağıdaki örnekte adlı ölçek kümesi hakkında bilgi alır *myScaleSet* içinde *myResourceGroup* kaynak grubu. Şu şekilde kendi adlarınızı girin:

```powershell
Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```


## <a name="view-vms-in-a-scale-set"></a>Ölçek kümesindeki VM’leri görüntüleme
Bir ölçek kümesindeki sanal makine örneği listesini görüntülemek için kullanın [Get-AzVmssVM](/powershell/module/az.compute/get-azvmssvm). Aşağıdaki örnekte adlı ölçek kümesi içinde tüm VM örnekleri listelenmiştir *myScaleSet* ve *myResourceGroup* kaynak grubu. Bu adları için kendi değerlerinizi sağlayın:

```powershell
Get-AzVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```

Belirli bir sanal makine örneği hakkında ek bilgi görüntülemek için Ekle `-InstanceId` parametresi [Get-AzVmssVM](/powershell/module/az.compute/get-azvmssvm) ve görüntülemek için bir örnek belirtin. Aşağıdaki örnek, sanal makine örneği hakkında bilgi görüntüler *0* adlı ölçek kümesi içinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Şu şekilde kendi adlarınızı girin:

```powershell
Get-AzVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="change-the-capacity-of-a-scale-set"></a>Ölçek kümesinin kapasitesini değiştirme
Yukarıdaki komutların ölçek kümenizi ve sanal makine örnekleri hakkında bilgi gösterdi. Artırabilir veya ölçek kümesindeki örneklerin sayısını azaltmak için kapasiteyi değiştirebilirsiniz. Ölçek kümesini otomatik olarak oluşturur veya gerekli VM sayısını kaldırır ve ardından uygulama trafiği almak için sanal makineleri yapılandırır.

İlk olarak, bir ölçek kümesi nesnesi oluşturma [Get-AzVmss](/powershell/module/az.compute/get-azvmss), için yeni bir değer belirtmezseniz `sku.capacity`. Kapasite değişikliğini uygulamak için [güncelleştirme AzVmss](/powershell/module/az.compute/update-azvmss). Aşağıdaki örnek güncelleştirmeleri *myScaleSet* içinde *myResourceGroup* kapasitesi kaynak grubuna *5* örnekleri. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```powershell
# Get current scale set
$vmss = Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"

# Set and update the capacity of your scale set
$vmss.sku.capacity = 5
Update-AzVmss -ResourceGroupName "myResourceGroup" -Name "myScaleSet" -VirtualMachineScaleSet $vmss
```

Ölçek kümenizin kapasitesinin güncelleştirilmesi birkaç dakika sürer. Bir ölçek kapasitesini azaltırsanız kimlikleri ilk önce kaldırılır en yüksek örnek ile Vm'leri ayarlayın.


## <a name="stop-and-start-vms-in-a-scale-set"></a>VM ölçek kümesindeki durdurup
Bir ölçek kümesindeki bir veya daha fazla sanal makineleri durdurmak için kullanın [Stop-AzVmss](/powershell/module/az.compute/stop-azvmss). `-InstanceId` parametresi, durdurulacak bir veya daha fazla sanal makine belirtmenize olanak sağlar. Örnek kimliği belirtmezseniz, ölçek kümesindeki tüm sanal makineler durdurulur. Birden çok VM durdurmak için her örnek kimliği virgül ile ayırın.

Aşağıdaki örnek örneği durdurur *0* adlı ölçek kümesi içinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```powershell
Stop-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```

Varsayılan olarak, durdurulan sanal makineler serbest bırakılır ve bunlar için işlem ücreti alınmaz. Durdurulan sanal makinenin sağlama durumunda kalmasını istiyorsanız, önceki komuta `-StayProvisioned` parametresini ekleyin. Sağlama durumunda tutulan durdurulmuş sanal makineler için normal işlem ücreti alınır.


### <a name="start-vms-in-a-scale-set"></a>Bir ölçek kümesindeki VM'lerin başlatma
Bir ölçek kümesindeki bir veya daha fazla sanal makine başlatmak için kullanın [başlangıç AzVmss](/powershell/module/az.compute/start-azvmss). `-InstanceId` parametresi, başlatılacak bir veya daha fazla sanal makine belirtmenize olanak sağlar. Örnek kimliği belirtmezseniz, ölçek kümesindeki tüm sanal makineler başlatılır. Birden çok VM başlatmak için her örnek kimliği virgül ile ayırın.

Aşağıdaki örnek bir örneğini başlatır *0* adlı ölçek kümesi içinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```powershell
Start-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="restart-vms-in-a-scale-set"></a>Bir ölçek kümesindeki Vm'leri yeniden başlatma
Bir ölçek kümesindeki bir veya daha fazla sanal makine yeniden başlatmak için kullanmak [yeniden AzVmss](/powershell/module/az.compute/restart-azvmss). `-InstanceId` parametresi, yeniden başlatılacak bir veya daha fazla sanal makine belirtmenize olanak sağlar. Örnek kimliği belirtmezseniz, ölçek kümesindeki tüm sanal makineler yeniden başlatılır. Birden çok VM'yi yeniden başlatmak için her örnek kimliği virgül ile ayırın.

Aşağıdaki örnek örneğini yeniden başlatır *0* adlı ölçek kümesi içinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```powershell
Restart-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="remove-vms-from-a-scale-set"></a>VM ölçek kümesinden Kaldır
Bir ölçek kümesindeki bir veya daha fazla sanal makine kaldırmak için [Remove-AzVmss](/powershell/module/az.compute/remove-azvmss). `-InstanceId` Parametresi, kaldırmak için bir veya daha fazla sanal makine belirtmenize olanak sağlar. Örnek kimliği belirtmezseniz, Ölçek kümesindeki tüm sanal makineler kaldırılır. Birden çok VM kaldırmak için her örnek kimliği virgül ile ayırın.

Aşağıdaki örnek, örnek kaldırır *0* adlı ölçek kümesi içinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```powershell
Remove-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="next-steps"></a>Sonraki adımlar
Ölçek kümeleri için sık kullanılan diğer görevler nasıl [uygulama dağıtma](virtual-machine-scale-sets-deploy-app.md), ve [yükseltme sanal makine örnekleri](virtual-machine-scale-sets-upgrade-scale-set.md). Azure PowerShell'i de kullanabilirsiniz [otomatik ölçeklendirme kuralları yapılandırma](virtual-machine-scale-sets-autoscale-overview.md).
