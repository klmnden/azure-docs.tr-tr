---
title: Azure PowerShell ile Sanal Makine Ölçek Kümesi Oluşturma | Microsoft Belgeleri
description: Azure PowerShell ile hızlıca sanal makine ölçek kümesi oluşturmayı öğrenin
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: get-started-article
ms.date: 12/19/2017
ms.author: iainfou
ms.openlocfilehash: c5ca90ae303d62c4ad453971f84da78866b0c599
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2018
---
# <a name="create-a-virtual-machine-scale-set-with-azure-powershell"></a>Azure PowerShell ile Sanal Makine Ölçek Kümesi oluşturma
Sanal makine ölçek kümesi, birbiriyle aynı ve otomatik olarak ölçeklendirilen sanal makine kümesi dağıtmanızı ve yönetmenizi sağlar. Ölçek kümesi içindeki VM sayısını el ile ölçeklendirebilir veya CPU, bellek isteği ya da ağ trafiği gibi kaynak kullanımını temel alan otomatik ölçeklendirme kuralları tanımlayabilirsiniz. Bu başlangıç makalesinde Azure PowerShell ile bir sanal makine ölçek kümesi oluşturacaksınız. Ölçek kümesi oluşturmak için [Azure CLI 2.0](virtual-machine-scale-sets-create-cli.md) veya [Azure portalı](virtual-machine-scale-sets-create-portal.md) da kullanabilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici, Azure PowerShell modülü 4.4.1 veya sonraki bir sürümü gerektirir. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.


## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Ölçek kümesi oluşturabilmek için [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) ile bir kaynak grubu oluşturun. Aşağıdaki örnek *EastUS* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName "myResourceGroup" -Location "EastUS"
```


## <a name="create-load-balancer"></a>Yük dengeleyici oluşturma
Ölçek kümesindeki VM örnekleri bir yük dengeleyiciye bağlıdır. Azure yük dengeleyici, gelen trafiği iyi durumdaki VM'ler arasında dağıtmak için yüksek kullanılabilirlik sağlayan bir 4. Katman (TCP, UDP) yük dengeleyicidir. Yük dengeleyici durum araştırması, her VM'deki belirli bir bağlantı noktasını izler ve trafiği yalnızca çalışır durumdaki VM'lere yönlendirir.

Bir sanal ağ ile genel IP adresine sahip olan ve 80 numaralı bağlantı noktasına gelen web trafiğini dağıtan bir yük dengeleyici oluşturun. Bu kaynakları oluşturmak için aşağıdaki PowerShell cmdlet'lerini kopyalayıp yapıştırın:

```azurepowershell-interactive
# Create a virtual network subnet
$subnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name "mySubnet" `
  -AddressPrefix 10.0.0.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName "myResourceGroup" `
  -Name "myVnet" `
  -Location "EastUS" `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $subnet

# Create a public IP address
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName "myResourceGroup" `
  -Location "EastUS" `
  -AllocationMethod Static `
  -Name "myPublicIP"

# Create a frontend and backend IP pool
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name "myFrontEndPool" `
  -PublicIpAddress $publicIP
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "myBackEndPool"

# Create a Network Address Translation (NAT) pool
$inboundNATPool = New-AzureRmLoadBalancerInboundNatPoolConfig `
  -Name "myRDPRule" `
  -FrontendIpConfigurationId $frontendIP.Id `
  -Protocol TCP `
  -FrontendPortRangeStart 50001 `
  -FrontendPortRangeEnd 50010 `
  -BackendPort 3389

# Create the load balancer
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName "myResourceGroup" `
  -Name "myLoadBalancer" `
  -Location "EastUS" `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool `
  -InboundNatPool $inboundNATPool

# Create a load balancer health probe on port 80
Add-AzureRmLoadBalancerProbeConfig -Name "myHealthProbe" `
  -LoadBalancer $lb `
  -Protocol TCP `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2

# Create a load balancer rule to distribute traffic on port 80
Add-AzureRmLoadBalancerRuleConfig `
  -Name "myLoadBalancerRule" `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol TCP `
  -FrontendPort 80 `
  -BackendPort 80

# Update the load balancer configuration
Set-AzureRmLoadBalancer -LoadBalancer $lb

# Create IP address configurations
$ipConfig = New-AzureRmVmssIpConfig `
  -Name "myIPConfig" `
  -LoadBalancerBackendAddressPoolsId $lb.BackendAddressPools[0].Id `
  -LoadBalancerInboundNatPoolsId $inboundNATPool.Id `
  -SubnetId $vnet.Subnets[0].Id
```


## <a name="create-a-scale-set"></a>Ölçek kümesi oluşturma
Bu adımda [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss) ile bir sanal makine ölçek kümesi oluşturun. Aşağıdaki örnek *myScaleSet* adına sahip olan ve *Windows Server 2016 Datacenter* platform görüntüsünü kullanan bir ölçek kümesi oluşturur. *vmssConfig* nesnesi Doğu ABD konumunda 2 VM örneği oluşturur ve kimlik bilgilerini *adminUsername* ve *securePassword* değişkenleri ile belirtir. Kendi kimlik bilgilerinizi sağlayıp ölçek kümesi oluşturmak için aşağıdaki adımları uygulayın:

```azurepowershell-interactive
# Provide your own secure password for use with the VM instances
$securePassword = "P@ssword!"
$adminUsername = "azureuser"

# Create a config object
$vmssConfig = New-AzureRmVmssConfig `
    -Location "EastUS" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic

# Reference a virtual machine image from the gallery
Set-AzureRmVmssStorageProfile $vmssConfig `
  -ImageReferencePublisher "MicrosoftWindowsServer" `
  -ImageReferenceOffer "WindowsServer" `
  -ImageReferenceSku "2016-Datacenter" `
  -ImageReferenceVersion "latest"

# Set up information for authenticating with the virtual machine
Set-AzureRmVmssOsProfile $vmssConfig `
  -AdminUsername $adminUsername `
  -AdminPassword $securePassword `
  -ComputerNamePrefix "myVM"

# Attach the virtual network to the config object
Add-AzureRmVmssNetworkInterfaceConfiguration `
  -VirtualMachineScaleSet $vmssConfig `
  -Name "network-config" `
  -Primary $true `
  -IPConfiguration $ipConfig

# Create the scale set with the config object (this step might take a few minutes)
New-AzureRmVmss `
  -ResourceGroupName "myResourceGroup" `
  -Name "myScaleSet" `
  -VirtualMachineScaleSet $vmssConfig
```

Tüm ölçek kümesi kaynaklarının ve VM'lerin oluşturulup yapılandırılması birkaç dakika sürer.


## <a name="install-iis-webserver"></a>IIS web sunucusunu yükleme
Ölçek kümenizi test etmek için Özel Betik Uzantısı'nı kullanarak VM örneklerine IIS uygulamasını yükleyen bir betik indirip çalıştırın. Bu uzantı dağıtım sonrası yapılandırma, yazılım yükleme veya diğer yapılandırma/yönetim görevleri için kullanışlıdır. Daha fazla bilgi için bkz. [Özel Betik Uzantısı'na genel bakış](../virtual-machines/windows/extensions-customscript.md).

IIS uygulamasını yükleyen Özel Betik Uzantısı'nı aşağıdaki şekilde uygulayın:

```azurepowershell-interactive
# Define the script for your Custom Script Extension to run
$publicSettings = @{
    "fileUris" = (,"https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate-iis.ps1");
    "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1"
}

# Get information about the scale set
$vmss = Get-AzureRmVmss `
            -ResourceGroupName "myResourceGroup" `
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
    -ResourceGroupName "myResourceGroup" `
    -Name "myScaleSet" `
    -VirtualMachineScaleSet $vmss
```


## <a name="test-your-web-server"></a>Web sunucunuzu test etme
Web sunucunuzu çalışır halde görmek için [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) ile yük dengeleyicinizin genel IP adresini alın. Aşağıdaki örnek *myResourceGroup* kaynak grubunda oluşturulan IP adresini alır:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -ResourceGroupName "myResourceGroup" | Select IpAddress
```

Yük dengeleyicinin genel IP adresini bir web tarayıcısına girin. Aşağıdaki örnekte gösterildiği gibi yük dengeleyici trafiği VM örneklerinizden birine dağıtır:

![Çalışan IIS sitesi](./media/virtual-machine-scale-sets-create-powershell/running-iis-site.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekli değilse, [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanarak kaynak grubunu, ölçek kümesini ve tüm ilgili kaynakları şu şekilde kaldırabilirsiniz:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name "myResourceGroup"
```


## <a name="next-steps"></a>Sonraki adımlar
Bu başlangıç makalesinde basit bir ölçek kümesi oluşturdunuz ve Özel Betik Uzantısı'nı kullanarak VM örneklerine basit bir IIS web sunucusu yüklediniz. Daha fazla ölçeklenebilirlik ve otomasyon için ölçek kümenizi aşağıdaki nasıl yapılır makaleleriyle genişletin:

- [Uygulamanızı sanal makine ölçek kümelerine dağıtma](virtual-machine-scale-sets-deploy-app.md)
- [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md), [Azure CLI](virtual-machine-scale-sets-autoscale-cli.md) veya [Azure portalı](virtual-machine-scale-sets-autoscale-portal.md) ile otomatik ölçeklendirme
- [Ölçek kümesi VM örnekleriniz için otomatik işletim sistemi yükseltmelerini kullanma](virtual-machine-scale-sets-automatic-upgrade.md)
