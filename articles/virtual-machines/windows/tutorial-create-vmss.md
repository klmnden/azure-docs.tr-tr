---
title: Öğretici - Azure’da Windows için sanal makine ölçek kümesi oluşturma | Microsoft Docs
description: Bu öğreticide, bir sanal makine ölçek kümesini kullanarak Windows VM'leri üzerinde yüksek oranda kullanılabilir bir uygulama oluşturmak ve dağıtmak için Azure PowerShell kullanmayı öğreneceksiniz
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: ''
ms.topic: tutorial
ms.date: 11/30/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 6766dc2b4d99d51e2832a054969697e4e330dd86
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60785168"
---
# <a name="tutorial-create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-windows-with-azure-powershell"></a>Öğretici: Bir sanal makine ölçek kümesi oluşturma ve Azure PowerShell ile Windows üzerinde yüksek oranda kullanılabilir bir uygulama dağıtma
Bir sanal makine ölçek kümesi, bir grup özdeş, otomatik ölçeklendirme sanal makinelerin dağıtmanıza ve yönetmenize olanak tanır. Ölçek el ile kümesindeki VM sayısını ölçeklendirebilirsiniz. Ayrıca, CPU, bellek isteği ya da ağ trafiği gibi kaynak kullanımını temel alan otomatik ölçeklendirme kuralları tanımlayabilirsiniz. Bu öğreticide, azure'da bir sanal makine ölçek kümesini dağıtmak ve bilgi edinmek için nasıl:

> [!div class="checklist"]
> * Ölçeklenecek bir IIS sitesi tanımlamak için Özel Betik Uzantısı kullanma
> * Ölçek kümeniz için bir yük dengeleyici oluşturma
> * Sanal makine ölçek kümesi oluşturma
> * Ölçek kümesindeki örnek sayısını artırma veya azaltma
> * Otomatik ölçeklendirme kuralları oluşturma

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. 

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. İsterseniz [https://shell.azure.com/powershell](https://shell.azure.com/powershell) adresine giderek Cloud Shell'i ayrı bir tarayıcı sekmesinde de başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

## <a name="scale-set-overview"></a>Ölçek Kümesine genel bakış
Bir sanal makine ölçek kümesi, bir grup özdeş, otomatik ölçeklendirme sanal makinelerin dağıtmanıza ve yönetmenize olanak tanır. Ölçek kümesindeki VM’ler, bir veya daha fazla *yerleştirme grubu* şeklinde mantık hatası ve güncelleme etki alanlarında dağıtılır. Yerleştirme grupları şuna benzer şekilde yapılandırılmış Vm'lerdir gruplarıdır [kullanılabilirlik kümeleri](tutorial-availability-sets.md).

VM’ler, ölçek kümesinde gerektiğinde oluşturulur. Ölçek kümesinde VM eklenmesi veya kaldırılması işlemlerinin nasıl ve ne zaman gerçekleştirileceğini denetlemek için otomatik ölçeklendirme kurallarını tanımlanır. Bu kurallar, CPU yükü, bellek kullanımı veya ağ trafiği gibi ölçümlere dayalı olarak tetiklenebilir.

Bir Azure platform görüntüsü kullanılırsa ölçek kümeleri 1.000 adede kadar VM'i destekler. Ciddi derecede yükleme veya VM özelleştirme gereksinimleri olan iş yükleri için [özel bir VM görüntüsü oluşturulması](tutorial-custom-images.md) iyi olabilir. Özel bir görüntü kullanırken ölçek kümesinde 300 adede kadar VM oluşturabilirsiniz.


## <a name="create-a-scale-set"></a>Ölçek kümesi oluşturma
Bir sanal makine ölçek kümesi oluşturma [yeni AzVmss](https://docs.microsoft.com/powershell/module/az.compute/new-azvmss). Aşağıdaki örnek *myScaleSet* adına sahip olan ve *Windows Server 2016 Datacenter* platform görüntüsünü kullanan bir ölçek kümesi oluşturur. Sanal ağ, genel IP adresi ve yük dengeleyici için Azure ağ kaynakları otomatik olarak oluşturulur. İstendiğinde ölçek kümesindeki VM örnekleri için yönetici kimlik bilgilerinizi kendi ayarlayabilirsiniz:

```azurepowershell-interactive
New-AzVmss `
  -ResourceGroupName "myResourceGroupScaleSet" `
  -Location "EastUS" `
  -VMScaleSetName "myScaleSet" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicyMode "Automatic"
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
$vmss = Get-AzVmss `
  -ResourceGroupName "myResourceGroupScaleSet" `
  -VMScaleSetName "myScaleSet"

# Use Custom Script Extension to install IIS and configure basic website
Add-AzVmssExtension -VirtualMachineScaleSet $vmss `
  -Name "customScript" `
  -Publisher "Microsoft.Compute" `
  -Type "CustomScriptExtension" `
  -TypeHandlerVersion 1.8 `
  -Setting $publicSettings

# Update the scale set and apply the Custom Script Extension to the VM instances
Update-AzVmss `
  -ResourceGroupName "myResourceGroupScaleSet" `
  -Name "myScaleSet" `
  -VirtualMachineScaleSet $vmss
```

## <a name="allow-traffic-to-application"></a>Uygulamaya giden trafiğe izin verme

Temel bir web uygulamasına erişim izni vermek için bir ağ güvenlik grubu oluşturma [yeni AzNetworkSecurityRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecurityruleconfig) ve [yeni AzNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecuritygroup). Daha fazla bilgi için [Azure sanal makine ölçek kümeleri için ağ](../../virtual-machine-scale-sets/virtual-machine-scale-sets-networking.md).

```azurepowershell-interactive
# Get information about the scale set
$vmss = Get-AzVmss `
  -ResourceGroupName "myResourceGroupScaleSet" `
  -VMScaleSetName "myScaleSet"

#Create a rule to allow traffic over port 80
$nsgFrontendRule = New-AzNetworkSecurityRuleConfig `
  -Name myFrontendNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 200 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80 `
  -Access Allow

#Create a network security group and associate it with the rule
$nsgFrontend = New-AzNetworkSecurityGroup `
  -ResourceGroupName  "myResourceGroupScaleSet" `
  -Location EastUS `
  -Name myFrontendNSG `
  -SecurityRules $nsgFrontendRule

$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName  "myResourceGroupScaleSet" `
  -Name myVnet

$frontendSubnet = $vnet.Subnets[0]

$frontendSubnetConfig = Set-AzVirtualNetworkSubnetConfig `
  -VirtualNetwork $vnet `
  -Name mySubnet `
  -AddressPrefix $frontendSubnet.AddressPrefix `
  -NetworkSecurityGroup $nsgFrontend

Set-AzVirtualNetwork -VirtualNetwork $vnet

# Update the scale set and apply the Custom Script Extension to the VM instances
Update-AzVmss `
  -ResourceGroupName "myResourceGroupScaleSet" `
  -Name "myScaleSet" `
  -VirtualMachineScaleSet $vmss
```

## <a name="test-your-scale-set"></a>Ölçek kümenizi test etme
Ölçek kümenizi, ile yük dengeleyicinizin genel IP adresini alın görmek için [Get-AzPublicIPAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress). Aşağıdaki örnek IP adresini görüntüler *Mypublicıp* ölçek kümesinin bir parçası olarak oluşturulan:

```azurepowershell-interactive
Get-AzPublicIPAddress `
  -ResourceGroupName "myResourceGroupScaleSet" `
  -Name "myPublicIPAddress" | select IpAddress
```

Genel IP adresini bir web tarayıcısına girin. Web uygulaması, yük dengeleyicinin trafiği dağıttığı VM’nin ana bilgisayar adı ile birlikte görüntülenir:

![Çalışan IIS sitesi](./media/tutorial-create-vmss/running-iis-site.png)

Ölçek kümesini çalışırken görmek için web tarayıcınızı yenilemeye zorlayarak yük dengeleyicinin trafiği, uygulamanızı çalıştıran üç VM’ye dağıtmasını görebilirsiniz.


## <a name="management-tasks"></a>Yönetim görevleri
Ölçek kümesinin yaşam döngüsü boyunca bir veya daha fazla yönetim görevi çalıştırmanız gerekebilir. Ayrıca, çeşitli yaşam döngüsü görevlerini otomatikleştiren betikler oluşturmak isteyebilirsiniz. Azure PowerShell tüm bunları hızlıca yapabilmenizi sağlar. Aşağıda birkaç yaygın görev açıklanmaktadır.

### <a name="view-vms-in-a-scale-set"></a>Ölçek kümesindeki VM’leri görüntüleme
Bir ölçek kümesindeki sanal makine örneklerinin listesini görüntülemek için kullanın [Get-AzVmssVM](https://docs.microsoft.com/powershell/module/az.compute/get-azvmssvm) gibi:

```azurepowershell-interactive
Get-AzVmssVM `
  -ResourceGroupName "myResourceGroupScaleSet" `
  -VMScaleSetName "myScaleSet"
```

Aşağıdaki örnek çıkış, ölçek kümesindeki iki sanal makine örneğini gösterir:

```powershell
ResourceGroupName                 Name Location             Sku InstanceID ProvisioningState
-----------------                 ---- --------             --- ---------- -----------------
MYRESOURCEGROUPSCALESET   myScaleSet_0   eastus Standard_DS1_v2          0         Succeeded
MYRESOURCEGROUPSCALESET   myScaleSet_1   eastus Standard_DS1_v2          1         Succeeded
```

Belirli bir sanal makine örneği hakkında ek bilgi görüntülemek için Ekle `-InstanceId` parametresi [Get-AzVmssVM](https://docs.microsoft.com/powershell/module/az.compute/get-azvmssvm). Aşağıdaki örnekte, *1* sanal makine örneğiyle ilgili bilgiler görüntülenir:

```azurepowershell-interactive
Get-AzVmssVM `
  -ResourceGroupName "myResourceGroupScaleSet" `
  -VMScaleSetName "myScaleSet" `
  -InstanceId "1"
```


### <a name="increase-or-decrease-vm-instances"></a>VM örneklerinin sayısını artırma veya azaltma
Şu anda kullandığınız bir ölçek kümesindeki örneklerin sayısını görmek için kullanın [Get-AzVmss](https://docs.microsoft.com/powershell/module/az.compute/get-azvmss) üzerinde bir sorgu *sku.capacity*:

```azurepowershell-interactive
Get-AzVmss -ResourceGroupName "myResourceGroupScaleSet" `
  -VMScaleSetName "myScaleSet" | `
  Select -ExpandProperty Sku
```

Daha sonra elle artırabilir veya ölçek kümesi sanal makinelerin sayısını azaltmak [güncelleştirme AzVmss](https://docs.microsoft.com/powershell/module/az.compute/update-azvmss). Aşağıdaki örnek, ölçek kümenizdeki VM'lerin sayısını *3* olarak ayarlar:

```azurepowershell-interactive
# Get current scale set
$scaleset = Get-AzVmss `
  -ResourceGroupName "myResourceGroupScaleSet" `
  -VMScaleSetName "myScaleSet"

# Set and update the capacity of your scale set
$scaleset.sku.capacity = 3
Update-AzVmss -ResourceGroupName "myResourceGroupScaleSet" `
    -Name "myScaleSet" `
    -VirtualMachineScaleSet $scaleset
```

Ölçek kümenizde belirtilen sayıda örneğin güncelleştirilmesi birkaç dakika sürer.


### <a name="configure-autoscale-rules"></a>Otomatik ölçeklendirme kuralları yapılandırma
Ölçek kümenizdeki örneklerin sayısını el ile ölçeklendirmek yerine otomatik ölçeklendirme kuralları tanımlarsınız. Bu kurallar, ölçek kümenizdeki örnekleri izler ve tanımladığınız ölçüm ve eşiklere göre müdahale eder. Aşağıdaki örnek, ortalama CPU yükü 5 dakika süresince %60’dan yüksek olursa örnek sayısını bir tane artırır. Ortalama CPU yükü daha sonra 5 dakika süresince %30’dan düşük olursa örnek sayısı bir tane azaltılır:

```azurepowershell-interactive
# Define your scale set information
$mySubscriptionId = (Get-AzSubscription)[0].Id
$myResourceGroup = "myResourceGroupScaleSet"
$myScaleSet = "myScaleSet"
$myLocation = "East US"
$myScaleSetId = (Get-AzVmss -ResourceGroupName $myResourceGroup -VMScaleSetName $myScaleSet).Id 

# Create a scale up rule to increase the number instances after 60% average CPU usage exceeded for a 5-minute period
$myRuleScaleUp = New-AzAutoscaleRule `
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
$myRuleScaleDown = New-AzAutoscaleRule `
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
$myScaleProfile = New-AzAutoscaleProfile `
  -DefaultCapacity 2  `
  -MaximumCapacity 10 `
  -MinimumCapacity 2 `
  -Rule $myRuleScaleUp,$myRuleScaleDown `
  -Name "autoprofile"

# Apply the autoscale rules
Add-AzAutoscaleSetting `
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
