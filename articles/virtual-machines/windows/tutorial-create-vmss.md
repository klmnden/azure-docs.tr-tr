---
title: Öğretici - Azure’da Windows için sanal makine ölçek kümesi oluşturma | Microsoft Docs
description: Bu öğreticide, bir sanal makine ölçek kümesini kullanarak Windows VM'leri üzerinde yüksek oranda kullanılabilir bir uygulama oluşturmak ve dağıtmak için Azure PowerShell kullanmayı öğreneceksiniz
services: virtual-machine-scale-sets
documentationcenter: ''
author: zr-msft
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: ''
ms.topic: tutorial
ms.date: 03/29/2018
ms.author: zarhoads
ms.custom: mvc
ms.openlocfilehash: 915b3d6aed2aa00e29916d2803f56b2172375f60
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49469569"
---
# <a name="tutorial-create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-windows-with-azure-powershell"></a>Öğretici: Azure PowerShell ile sanal makine ölçek kümesi oluşturma ve Windows üzerinde yüksek oranda kullanılabilir bir uygulama dağıtma
Sanal makine ölçek kümesi, birbiriyle aynı ve otomatik olarak ölçeklendirilen sanal makine kümesi dağıtmanızı ve yönetmenizi sağlar. Ölçek kümesi içindeki VM sayısını el ile ölçeklendirebilir veya CPU, bellek isteği ya da ağ trafiği gibi kaynak kullanımını temel alan otomatik ölçeklendirme kuralları tanımlayabilirsiniz. Bu öğreticide, Azure’da bir sanal makine ölçek kümesi dağıtılmaktadır. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Ölçeklenecek bir IIS sitesi tanımlamak için Özel Betik Uzantısı kullanma
> * Ölçek kümeniz için bir yük dengeleyici oluşturma
> * Sanal makine ölçek kümesi oluşturma
> * Ölçek kümesindeki örnek sayısını artırma veya azaltma
> * Otomatik ölçeklendirme kuralları oluşturma

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz, bu öğretici Azure PowerShell modülü 5.7.0 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.


## <a name="scale-set-overview"></a>Ölçek Kümesine genel bakış
Sanal makine ölçek kümesi, birbiriyle aynı ve otomatik olarak ölçeklendirilen sanal makine kümesi dağıtmanızı ve yönetmenizi sağlar. Ölçek kümesindeki VM’ler, bir veya daha fazla *yerleştirme grubu* şeklinde mantık hatası ve güncelleme etki alanlarında dağıtılır. Bu gruplar, [kullanılabilirlik kümeleri](tutorial-availability-sets.md) gibi benzer şekilde yapılandırılmış VM’lerdir.

VM’ler, ölçek kümesinde gerektiğinde oluşturulur. Ölçek kümesinde VM eklenmesi veya kaldırılması işlemlerinin nasıl ve ne zaman gerçekleştirileceğini denetlemek için otomatik ölçeklendirme kurallarını tanımlanır. Bu kurallar, CPU yükü, bellek kullanımı veya ağ trafiği gibi ölçümlere dayalı olarak tetiklenebilir.

Bir Azure platform görüntüsü kullanılırsa ölçek kümeleri 1.000 adede kadar VM'i destekler. Ciddi derecede yükleme veya VM özelleştirme gereksinimleri olan iş yükleri için [özel bir VM görüntüsü oluşturulması](tutorial-custom-images.md) iyi olabilir. Özel bir görüntü kullanırken ölçek kümesinde 300 adede kadar VM oluşturabilirsiniz.


## <a name="create-a-scale-set"></a>Ölçek kümesi oluşturma
[New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss) ile bir sanal makine ölçek kümesi oluşturun. Aşağıdaki örnek *myScaleSet* adına sahip olan ve *Windows Server 2016 Datacenter* platform görüntüsünü kullanan bir ölçek kümesi oluşturur. Sanal ağ, genel IP adresi ve yük dengeleyici için Azure ağ kaynakları otomatik olarak oluşturulur. İstendiğinde, ölçek kümesindeki sanal makine örnekleri için kendi istediğiniz yönetici kimlik bilgilerini sağlayın:

```azurepowershell-interactive
New-AzureRmVmss `
  -ResourceGroupName "myResourceGroupScaleSet" `
  -Location "EastUS" `
  -VMScaleSetName "myScaleSet" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicy "Automatic"
```

Tüm ölçek kümesi kaynaklarının ve VM'lerin oluşturulup yapılandırılması birkaç dakika sürer.


## <a name="deploy-sample-application"></a>Örnek uygulama dağıtma
Ölçek kümenizi test etmek için temel web uygulaması yükleyin. Sanal makine örneklerine IIS uygulamasını yükleyen bir betik indirip çalıştırmak için Azure Özel Betik Uzantısı kullanılır. Bu uzantı dağıtım sonrası yapılandırma, yazılım yükleme veya diğer yapılandırma/yönetim görevleri için kullanışlıdır. Daha fazla bilgi için bkz. [Özel Betik Uzantısı'na genel bakış](extensions-customscript.md).

Temel bir IIS web sunucusu yüklemek için Özel Betik Uzantısı kullanın. IIS uygulamasını yükleyen Özel Betik Uzantısı'nı aşağıdaki şekilde uygulayın:

```azurepowershell-interactive
# Define the script for your Custom Script Extension to run
$publicSettings = @{
    "fileUris" = (,"https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate-iis.ps1");
    "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1"
}

# Get information about the scale set
$vmss = Get-AzureRmVmss `
            -ResourceGroupName "myResourceGroupScaleSet" `
            -VMScaleSetName "myScaleSet"

# Use Custom Script Extension to install IIS and configure basic website
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $publicSettings

# Update the scale set and apply the Custom Script Extension to the VM instances
Update-AzureRmVmss `
    -ResourceGroupName "myResourceGroupScaleSet" `
    -Name "myScaleSet" `
    -VirtualMachineScaleSet $vmss
```


## <a name="test-your-scale-set"></a>Ölçek kümenizi test etme
Ölçek kümenizi çalışır halde görmek için [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) ile yük dengeleyicinizin genel IP adresini alın. Aşağıdaki örnek, ölçek kümesinin bir parçası olarak oluşturulan *myPublicIP* için IP adresini alır:

```azurepowershell-interactive
Get-AzureRmPublicIPAddress `
    -ResourceGroupName "myResourceGroupScaleSet" `
    -Name "myPublicIPAddress" | select IpAddress
```

Genel IP adresini bir web tarayıcısına girin. Web uygulaması, yük dengeleyicinin trafiği dağıttığı VM’nin ana bilgisayar adı ile birlikte görüntülenir:

![Çalışan IIS sitesi](./media/tutorial-create-vmss/running-iis-site.png)

Ölçek kümesini çalışırken görmek için web tarayıcınızı yenilemeye zorlayarak yük dengeleyicinin trafiği, uygulamanızı çalıştıran üç VM’ye dağıtmasını görebilirsiniz.


## <a name="management-tasks"></a>Yönetim görevleri
Ölçek kümesinin yaşam döngüsü boyunca bir veya daha fazla yönetim görevi çalıştırmanız gerekebilir. Ayrıca, çeşitli yaşam döngüsü görevlerini otomatikleştiren betikler oluşturmak isteyebilirsiniz. Azure PowerShell tüm bunları hızlıca yapabilmenizi sağlar. Aşağıda birkaç yaygın görev açıklanmaktadır.

### <a name="view-vms-in-a-scale-set"></a>Ölçek kümesindeki VM’leri görüntüleme
Bir ölçek kümesindeki sanal makine örneklerinin listesini görüntülemek için [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) komutunu şu şekilde kullanın:

```azurepowershell-interactive
Get-AzureRmVmssVM -ResourceGroupName "myResourceGroupScaleSet" -VMScaleSetName "myScaleSet"
```

Aşağıdaki örnek çıkış, ölçek kümesindeki iki sanal makine örneğini gösterir:

```powershell
ResourceGroupName                 Name Location             Sku InstanceID ProvisioningState
-----------------                 ---- --------             --- ---------- -----------------
MYRESOURCEGROUPSCALESET   myScaleSet_0   eastus Standard_DS1_v2          0         Succeeded
MYRESOURCEGROUPSCALESET   myScaleSet_1   eastus Standard_DS1_v2          1         Succeeded
```

Belirli bir sanal makine örneği hakkında ek bilgi görüntülemek için [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) komutuna `-InstanceId` parametresini ekleyin. Aşağıdaki örnekte, *1* sanal makine örneğiyle ilgili bilgiler görüntülenir:

```azurepowershell-interactive
Get-AzureRmVmssVM -ResourceGroupName "myResourceGroupScaleSet" -VMScaleSetName "myScaleSet" -InstanceId "1"
```


### <a name="increase-or-decrease-vm-instances"></a>VM örneklerinin sayısını artırma veya azaltma
Ölçek kümesinde şu anda yer alan örneklerin sayısını görmek için [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) komutunu kullanarak *sku.capacity* üzerinde bir sorgu çalıştırın:

```azurepowershell-interactive
Get-AzureRmVmss -ResourceGroupName "myResourceGroupScaleSet" `
    -VMScaleSetName "myScaleSet" | `
    Select -ExpandProperty Sku
```

Ardından [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) ile ölçek kümesindeki sanal makinelerin sayısını elle artırabilir veya azaltabilirsiniz. Aşağıdaki örnek, ölçek kümenizdeki VM'lerin sayısını *3* olarak ayarlar:

```azurepowershell-interactive
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName "myResourceGroupScaleSet" `
  -VMScaleSetName "myScaleSet"

# Set and update the capacity of your scale set
$scaleset.sku.capacity = 3
Update-AzureRmVmss -ResourceGroupName "myResourceGroupScaleSet" `
    -Name "myScaleSet" `
    -VirtualMachineScaleSet $scaleset
```

Ölçek kümenizde belirtilen sayıda örneğin güncelleştirilmesi birkaç dakika sürer.


### <a name="configure-autoscale-rules"></a>Otomatik ölçeklendirme kuralları yapılandırma
Ölçek kümenizdeki örneklerin sayısını el ile ölçeklendirmek yerine otomatik ölçeklendirme kuralları tanımlarsınız. Bu kurallar, ölçek kümenizdeki örnekleri izler ve tanımladığınız ölçüm ve eşiklere göre müdahale eder. Aşağıdaki örnek, ortalama CPU yükü 5 dakika süresince %60’dan yüksek olursa örnek sayısını bir tane artırır. Ortalama CPU yükü daha sonra 5 dakika süresince %30’dan düşük olursa örnek sayısı bir tane azaltılır:

```azurepowershell-interactive
# Define your scale set information
$mySubscriptionId = (Get-AzureRmSubscription)[0].Id
$myResourceGroup = "myResourceGroupScaleSet"
$myScaleSet = "myScaleSet"
$myLocation = "East US"
$myScaleSetId = (Get-AzureRmVmss -ResourceGroupName $myResourceGroup -VMScaleSetName $myScaleSet).Id 

# Create a scale up rule to increase the number instances after 60% average CPU usage exceeded for a 5-minute period
$myRuleScaleUp = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId $myScaleSetId `
  -Operator GreaterThan `
  -MetricStatistic Average `
  -Threshold 60 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:05:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Increase `
  -ScaleActionValue 1

# Create a scale down rule to decrease the number of instances after 30% average CPU usage over a 5-minute period
$myRuleScaleDown = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId $myScaleSetId `
  -Operator LessThan `
  -MetricStatistic Average `
  -Threshold 30 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:05:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Decrease `
  -ScaleActionValue 1

# Create a scale profile with your scale up and scale down rules
$myScaleProfile = New-AzureRmAutoscaleProfile `
  -DefaultCapacity 2  `
  -MaximumCapacity 10 `
  -MinimumCapacity 2 `
  -Rule $myRuleScaleUp,$myRuleScaleDown `
  -Name "autoprofile"

# Apply the autoscale rules
Add-AzureRmAutoscaleSetting `
  -Location $myLocation `
  -Name "autosetting" `
  -ResourceGroup $myResourceGroup `
  -TargetResourceId $myScaleSetId `
  -AutoscaleProfile $myScaleProfile
```

Otomatik ölçeklendirme kullanımıyla ilgili diğer tasarım bilgileri için [otomatik ölçeklendirmeye yönelik en iyi yöntemlere](/azure/architecture/best-practices/auto-scaling) bakın.


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir sanal makine ölçek kümesi oluşturdunuz. Şunları öğrendiniz:

> [!div class="checklist"]
> * Ölçeklenecek bir IIS sitesi tanımlamak için Özel Betik Uzantısı kullanma
> * Ölçek kümeniz için bir yük dengeleyici oluşturma
> * Sanal makine ölçek kümesi oluşturma
> * Ölçek kümesindeki örnek sayısını artırma veya azaltma
> * Otomatik ölçeklendirme kuralları oluşturma

Sanal makinelere ilişkin yük dengeleme kavramları hakkında daha fazla bilgi edinmek için sıradaki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Sanal makinelerde yük dengeleme](tutorial-load-balancer.md)
