---
title: "Windows Azure için sanal makine ölçek kümeleri oluşturma | Microsoft Docs"
description: "Windows sanal makine ölçek kümesini kullanarak sanal makineleri üzerinde yüksek oranda kullanılabilir bir uygulama oluşturun ve dağıtın"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: 
ms.topic: article
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: d8f161af7753d2cd93a8683e8a93128144b86079
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-windows"></a>Bir sanal makine ölçek kümesi oluşturma ve yüksek oranda kullanılabilir bir Windows uygulamasını dağıtma
Bir sanal makine ölçek kümesini dağıtmak ve aynı, otomatik ölçeklendirme sanal makineler kümesi yönetmenize olanak sağlar. Ölçek kümesindeki VM'lerin sayısını elle ölçeklendirme ya da CPU, bellek isteğe bağlı veya ağ trafiğini gibi kaynak kullanımına bağlı olarak otomatik ölçeklendirme kurallarını tanımlayabilirsiniz. Bu öğreticide, Azure üzerinde ayarlanmış bir sanal makine ölçek dağıtın. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir IIS sitesi ölçeklendirme tanımlamak için özel betik uzantısı kullanın
> * Ölçek kümesi için bir yük dengeleyici oluşturma
> * Bir sanal makine ölçek kümesi oluşturma
> * Artırma veya azaltma ölçek kümesindeki örnek sayısı
> * Otomatik ölçeklendirme kuralları oluşturma

Bu öğretici, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps).


## <a name="scale-set-overview"></a>Ölçek kümesi'ne genel bakış
Bir sanal makine ölçek kümesini dağıtmak ve aynı, otomatik ölçeklendirme sanal makineler kümesi yönetmenize olanak sağlar. VM ölçek kümesindeki bir veya daha mantığı arıza ve güncelleştirme alanlarında dağıtılır *yerleştirme grupları*. Benzer şekilde yapılandırılmış sanal makineleri, benzer şekilde, bunlar gruplarıdır [kullanılabilirlik kümeleri](tutorial-availability-sets.md).

VM ölçek kümesindeki gerektiği şekilde oluşturulur. Nasıl ve ne zaman VM'ler eklendiğinde veya kaldırıldığında ölçek kümesi denetlemek için otomatik ölçeklendirme kurallarını tanımlayın. Bu kurallar temel alınarak ölçümleri CPU yükünü, bellek kullanımı veya ağ trafiğini gibi tetikleyebilir.

Bir Azure platform görüntüsü kullandığınızda ölçek 1.000 VM'ler kadar destek ayarlar. Önemli yükleme veya VM özelleştirme gereksinimleri ile iş yükleri için istediğiniz [özel bir VM görüntüsü oluşturma](tutorial-custom-images.md). Özel görüntü kullanırken ayarlayın bir ölçek kadar 300 VM'ler oluşturabilirsiniz.


## <a name="create-an-app-to-scale"></a>Ölçeklendirmek için uygulama oluşturma
Ölçek kümesini oluşturmadan önce bir kaynak grubuyla oluşturmanız [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroupAutomate* içinde *EastUS* konumu:

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupScaleSet -Location EastUS
```

Bir önceki öğreticide öğrenilen nasıl [otomatikleştirmek VM Yapılandırması](tutorial-automate-vm-deployment.md) özel betik uzantısı kullanma. Ölçek kümesi yapılandırması oluşturun, sonra IIS yüklemesi ve yapılandırması için bir özel betik uzantısı Uygula:

```powershell
# Create a config object
$vmssConfig = New-AzureRmVmssConfig `
    -Location EastUS `
    -SkuCapacity 2 `
    -SkuName Standard_DS2 `
    -UpgradePolicyMode Automatic

# Define the script for your Custom Script Extension to run
$publicSettings = @{
    "fileUris" = (,"https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate-iis.ps1");
    "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1"
}

# Use Custom Script Extension to install IIS and configure basic website
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $publicSettings
```

## <a name="create-scale-load-balancer"></a>Ölçek yük dengeleyici oluşturma
Bir Azure yük dengeleyici gelen trafiği sağlıklı VM'ler arasında dağıtarak yüksek kullanılabilirlik sağlayan bir katman 4 (TCP, UDP) yük dengeleyicidir. Bir yük dengeleyici durum araştırması her VM üzerinde belirli bir bağlantı noktasını izler ve yalnızca trafiği işletimsel bir VM dağıtır. Daha fazla bilgi için sonraki öğretici bakın [nasıl yükleneceğini Windows sanal makineleri Bakiye](tutorial-load-balancer.md).

Bir ortak IP adresi ve bağlantı noktası 80 üzerinde web trafiği dağıtan bir yük dengeleyicisi oluşturun:

```powershell
# Create a public IP address
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupScaleSet `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIP

# Create a frontend and backend IP pool
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name myFrontEndPool `
  -PublicIpAddress $publicIP
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool

# Create the load balancer
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroupScaleSet `
  -Name myLoadBalancer `
  -Location EastUS `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool

# Create a load balancer health probe on port 80
Add-AzureRmLoadBalancerProbeConfig -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2

# Create a load balancer rule to distribute traffic on port 80
Add-AzureRmLoadBalancerRuleConfig `
  -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80

# Update the load balancer configuration
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="create-a-scale-set"></a>Bir ölçek kümesi oluşturma
Şimdi bir sanal makine ölçek kümesi oluşturmak [yeni AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvm). Aşağıdaki örnek, ölçeği adlandırılmış Ayarla oluşturur *myScaleSet*:

```powershell
# Reference a virtual machine image from the gallery
Set-AzureRmVmssStorageProfile $vmssConfig `
  -ImageReferencePublisher MicrosoftWindowsServer `
  -ImageReferenceOffer WindowsServer `
  -ImageReferenceSku 2016-Datacenter `
  -ImageReferenceVersion latest

# Set up information for authenticating with the virtual machine
Set-AzureRmVmssOsProfile $vmssConfig `
  -AdminUsername azureuser `
  -AdminPassword P@ssword! `
  -ComputerNamePrefix myVM

# Create the virtual network resources
$subnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name "mySubnet" `
  -AddressPrefix 10.0.0.0/24
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName "myResourceGroupScaleSet" `
  -Name "myVnet" `
  -Location "EastUS" `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $subnet
$ipConfig = New-AzureRmVmssIpConfig `
  -Name "myIPConfig" `
  -LoadBalancerBackendAddressPoolsId $lb.BackendAddressPools[0].Id `
  -SubnetId $vnet.Subnets[0].Id

# Attach the virtual network to the config object
Add-AzureRmVmssNetworkInterfaceConfiguration `
  -VirtualMachineScaleSet $vmssConfig `
  -Name "network-config" `
  -Primary $true `
  -IPConfiguration $ipConfig

# Create the scale set with the config object (this step might take a few minutes)
New-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -Name myScaleSet `
  -VirtualMachineScaleSet $vmssConfig
```

Oluşturun ve tüm sanal makineleri ve ölçek kümesi kaynakları yapılandırmak için birkaç dakika sürer.


## <a name="test-your-app"></a>Uygulamanızı test etme
Eylem, IIS Web sitesinin görmek için yük dengeleyici ile genel IP adresi elde [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). Aşağıdaki örnek IP adresi alacağı *myPublicIP* ölçek kümesinin bir parçası olarak oluşturulan:

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupScaleSet -Name myPublicIP | select IpAddress
```

Ortak IP adresini bir web tarayıcısına girin. Ana bilgisayar trafiği için yük dengeleyici dağıtılmış VM adını dahil olmak üzere uygulama gösterilir:

![Çalışan IIS sitesi](./media/tutorial-create-vmss/running-iis-site.png)

Eylem kümesini görmek için zorla uygulamanızı çalıştıran tüm VM'ler arasında trafiği dağıtmak yük dengeleyici görmek için web tarayıcınızın yenileme.


## <a name="management-tasks"></a>Yönetim görevleri
Ölçek kümesini yaşam döngüsü boyunca, bir veya daha fazla yönetim görevleri çalıştırmanız gerekebilir. Ayrıca, çeşitli yaşam döngüsü görevleri otomatikleştiren komut dosyaları oluşturmak isteyebilirsiniz. Azure PowerShell, bu görevleri gerçekleştirmek için hızlı bir yoludur. Birkaç ortak görevler şunlardır.

### <a name="view-vms-in-a-scale-set"></a>Görünüm VM ölçek kümesindeki
Ölçek kümesinde çalışan sanal makineler listesini görüntülemek için kullanın [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) gibi:

```powershell
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -VMScaleSetName myScaleSet

# Loop through the instanaces in your scale set
for ($i=1; $i -le ($scaleset.Sku.Capacity - 1); $i++) {
    Get-AzureRmVmssVM -ResourceGroupName myResourceGroupScaleSet `
      -VMScaleSetName myScaleSet `
      -InstanceId $i
}
```


### <a name="increase-or-decrease-vm-instances"></a>Artırma veya azaltma VM örnekleri
Ölçek kümesindeki şu anda sahip örneklerinin sayısını görmek için [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) ve sorgulayın *sku.capacity*:

```powershell
Get-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -VMScaleSetName myScaleSet | `
    Select -ExpandProperty Sku
```

Daha sonra el ile artırabilir veya sanal makine ölçek kümesi sayısını azaltmak [güncelleştirme AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss). Aşağıdaki örnek VM'lerin sayısını ayarlamak, Ölçek ayarlar *5*:

```powershell
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -VMScaleSetName myScaleSet

# Set and update the capacity of your scale set
$scaleset.sku.capacity = 5
Update-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -Name myScaleSet `
    -VirtualMachineScaleSet $scaleset
```

Bir örneği, Ölçek belirtilen sayısı güncelleştirilmesi birkaç dakika sürer ayarlarsanız.


### <a name="configure-autoscale-rules"></a>Otomatik ölçeklendirme kurallarını yapılandırma
Örnek sayısı, Ölçek el ile ölçeklendirme ayarlamak yerine otomatik ölçeklendirme kurallarını tanımlayın. Bu kurallar, Ölçek kümesinin örneklerini izlemek ve buna göre ölçümleri ve tanımladığınız eşikleri göre yanıt. Ortalama CPU yükü 5 dakikalık bir süre içinde % 60'den büyük olduğunda aşağıdaki örnekte, biri tarafından örneklerinin sayısını olarak ölçeklendirir. Ortalama CPU yükünü daha sonra 5 dakikalık bir süre içinde % 30 düşerse örnekleri içinde bir örneği tarafından ölçeklenir:

```powershell
# Define your scale set information
$mySubscriptionId = (Get-AzureRmSubscription).Id
$myResourceGroup = "myResourceGroupScaleSet"
$myScaleSet = "myScaleSet"
$myLocation = "East US"

# Create a scale up rule to increase the number instances after 60% average CPU usage exceeded for a 5 minute period
$myRuleScaleUp = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -Operator GreaterThan `
  -MetricStatistic Average `
  -Threshold 60 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:05:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Increase `
  -ScaleActionValue 1

# Create a scale down rule to decrease the number of instances after 30% average CPU usage over a 5 minute period
$myRuleScaleDown = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
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
  -Rules $myRuleScaleUp,$myRuleScaleDown `
  -Name "autoprofile"

# Apply the autoscale rules
Add-AzureRmAutoscaleSetting `
  -Location $myLocation `
  -Name "autosetting" `
  -ResourceGroup $myResourceGroup `
  -TargetResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -AutoscaleProfiles $myScaleProfile
```

Daha fazla tasarım otomatik ölçeklendirme, kullanımı hakkında bilgi için [otomatik ölçeklendirme en iyi yöntemler](/azure/architecture/best-practices/auto-scaling).


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir sanal makine ölçek kümesi oluşturuldu. Şunları öğrendiniz:

> [!div class="checklist"]
> * Bir IIS sitesi ölçeklendirme tanımlamak için özel betik uzantısı kullanın
> * Ölçek kümesi için bir yük dengeleyici oluşturma
> * Bir sanal makine ölçek kümesi oluşturma
> * Artırma veya azaltma ölçek kümesindeki örnek sayısı
> * Otomatik ölçeklendirme kuralları oluşturma

Yük Dengeleme sanal makineler için kavramları hakkında daha fazla bilgi için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Sanal makinelerin yük dengelemesini](tutorial-load-balancer.md)
