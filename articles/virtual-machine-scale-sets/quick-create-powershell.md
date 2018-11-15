---
title: Hızlı Başlangıç - Azure PowerShell ile sanal makine ölçek kümesi oluşturma | Microsoft Docs
description: Azure PowerShell ile hızlıca sanal makine ölçek kümesi oluşturmayı öğrenin
services: virtual-machine-scale-sets
documentationcenter: ''
author: zr-msft
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.date: 11/08/18
ms.author: zarhoads
ms.openlocfilehash: 7c24375cd86700b3b4125447e1aa6dbc7507d8ba
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2018
ms.locfileid: "51515820"
---
# <a name="quickstart-create-a-virtual-machine-scale-set-with-azure-powershell"></a>Hızlı Başlangıç: Azure PowerShell ile sanal makine ölçek kümesi oluşturma
Bir sanal makine ölçek kümesi, bir grup özdeş, otomatik ölçeklendirme sanal makinelerin dağıtmanıza ve yönetmenize olanak tanır. Ölçek kümesi içindeki sanal makine sayısını el ile ölçeklendirebilir veya CPU, bellek talebi ya da ağ trafiği gibi kaynak kullanımını temel alan otomatik ölçeklendirme kuralları tanımlayabilirsiniz. Azure Load Balancer daha sonra ölçek kümesindeki sanal makine örneklerine trafiği dağıtır. Bu hızlı başlangıçta, Azure PowerShell ile bir sanal makine ölçek kümesi oluşturur ve örnek uygulama dağıtırsınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz, bu öğretici Azure PowerShell modülü 6.0.0 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `Connect-AzureRmAccount` Azure ile bir bağlantı oluşturmak için.


## <a name="create-a-scale-set"></a>Ölçek kümesi oluşturma
[New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss) ile bir sanal makine ölçek kümesi oluşturun. Aşağıdaki örnek *myScaleSet* adına sahip olan ve *Windows Server 2016 Datacenter* platform görüntüsünü kullanan bir ölçek kümesi oluşturur. Sanal ağ, genel IP adresi ve yük dengeleyici için Azure ağ kaynakları otomatik olarak oluşturulur. İstendiğinde ölçek kümesindeki VM örnekleri için yönetici kimlik bilgilerinizi kendi ayarlayabilirsiniz:

```azurepowershell-interactive
New-AzureRmVmss `
  -ResourceGroupName "myResourceGroup" `
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
Ölçek kümenizi test etmek için temel web uygulaması yükleyin. Sanal makine örneklerine IIS uygulamasını yükleyen bir betik indirip çalıştırmak için Azure Özel Betik Uzantısı kullanılır. Bu uzantı dağıtım sonrası yapılandırma, yazılım yükleme veya diğer yapılandırma/yönetim görevleri için kullanışlıdır. Daha fazla bilgi için bkz. [Özel Betik Uzantısı'na genel bakış](../virtual-machines/windows/extensions-customscript.md).

Temel bir IIS web sunucusu yüklemek için Özel Betik Uzantısı kullanın. IIS uygulamasını yükleyen Özel Betik Uzantısı'nı aşağıdaki şekilde uygulayın:

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

## <a name="allow-traffic-to-application"></a>Uygulamaya giden trafiğe izin verme

 Temel bir web uygulamasına erişim izni vermek için bir ağ güvenlik grubu oluşturma [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.compute/new-azurermnetworksecurityruleconfig) ve [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.compute/new-azurermnetworksecuritygroup). Daha fazla bilgi için [Azure sanal makine ölçek kümeleri için ağ](virtual-machine-scale-sets-networking.md).

 ```azurepowershell-interactive
 # Get information about the scale set
 $vmss = Get-AzureRmVmss `
             -ResourceGroupName "myResourceGroup" `
             -VMScaleSetName "myScaleSet"

 #Create a rule to allow traffic over port 80
 $nsgFrontendRule = New-AzureRmNetworkSecurityRuleConfig `
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
 $nsgFrontend = New-AzureRmNetworkSecurityGroup `
   -ResourceGroupName  "myResourceGroup" `
   -Location EastUS `
   -Name myFrontendNSG `
   -SecurityRules $nsgFrontendRule

 $vnet = Get-AzureRmVirtualNetwork `
   -ResourceGroupName  "myResourceGroup" `
   -Name myVnet

 $frontendSubnet = $vnet.Subnets[0]

 $frontendSubnetConfig = Set-AzureRmVirtualNetworkSubnetConfig `
   -VirtualNetwork $vnet `
   -Name mySubnet `
   -AddressPrefix $frontendSubnet.AddressPrefix `
   -NetworkSecurityGroup $nsgFrontend

 Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

 # Update the scale set and apply the Custom Script Extension to the VM instances
 Update-AzureRmVmss `
     -ResourceGroupName "myResourceGroup" `
     -Name "myScaleSet" `
     -VirtualMachineScaleSet $vmss
 ```

## <a name="test-your-scale-set"></a>Ölçek kümenizi test etme
Ölçek kümenizi çalışır halde görmek için bir web tarayıcısında örnek web uygulamasına erişin. İle yük dengeleyicinizin genel IP adresini alma [Get-Azurermpublicıpaddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). Aşağıdaki örnek, oluşturduğunuz IP adresini görüntüler. *myResourceGroup* kaynak grubu:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -ResourceGroupName "myResourceGroup" | Select IpAddress
```

Yük dengeleyicinin genel IP adresini bir web tarayıcısına girin. Aşağıdaki örnekte gösterildiği gibi yük dengeleyici trafiği VM örneklerinizden birine dağıtır:

![Çalışan IIS sitesi](./media/virtual-machine-scale-sets-create-powershell/running-iis-site.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekli değilse, [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanarak kaynak grubunu, ölçek kümesini ve tüm ilgili kaynakları şu şekilde kaldırabilirsiniz. `-Force` parametresi kaynakları ek bir komut istemi olmadan silmek istediğinizi onaylar. `-AsJob` parametresi işlemin tamamlanmasını beklemeden denetimi komut istemine döndürür.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name "myResourceGroup" -Force -AsJob
```


## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta basit bir ölçek kümesi oluşturdunuz ve Özel Betik Uzantısı'nı kullanarak VM örneklerine temel bir IIS web sunucusu yüklediniz. Daha fazla bilgi edinmek için Azure sanal makine ölçek kümelerinin nasıl oluşturulacağı ve yönetileceğine ilişkin öğreticiyle devam edin.

> [!div class="nextstepaction"]
> [Azure sanal makine ölçek kümeleri oluşturma ve yönetme](tutorial-create-and-manage-powershell.md)
