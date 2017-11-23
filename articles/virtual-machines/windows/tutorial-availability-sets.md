---
title: "Kullanılabilirlik kümeleri öğretici azure'da Windows sanal makineleri için | Microsoft Docs"
description: "Azure'da Windows sanal makineleri için kullanılabilirlik kümeleri hakkında bilgi edinin."
documentationcenter: 
services: virtual-machines-windows
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/05/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 2345b434a51b768793c2dea4587dc0a49ab35b70
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2017
---
# <a name="how-to-use-availability-sets"></a>Kullanılabilirlik kümeleri kullanma

Bu öğreticide, kullanılabilirlik ve kullanılabilirlik kümeleri adlı bir özelliği kullanarak azure'da sanal makine çözümlerinizi güvenilirliğini artırmak öğrenin. Kullanılabilirlik kümeleri, Azure üzerinde dağıttığınız sanal makineleri kümedeki birden çok yalıtılmış donanım düğümler arasında dağıtılır emin olun. Bu Azure içinde bir donanım veya yazılım hatası oluşursa, Vm'leriniz yalnızca bir alt kümesi etkilenmemesini sağlar yapmak ve çözümünüzün genel kullanılabilir ve çalışır durumda kalır. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Kullanılabilirlik kümesi oluşturma
> * Bir kullanılabilirlik kümesine bir VM oluşturma
> * Kullanılabilir VM boyutları denetleyin
> * Azure Danışmanı'nı denetleyin

Bu öğretici, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps).

## <a name="availability-set-overview"></a>Kullanılabilirlik kümesi'ne genel bakış

Bir kullanılabilirlik kümesi Azure'da Azure veri merkezi içinde dağıtıldığında içine yerleştirin VM kaynakları birbirinden yalıtılmış olduğundan emin olmak için kullanabileceğiniz bir mantıksal bir gruplandırma bir özelliktir. Azure birden çok fiziksel sunucuda çalıştırmak bir kullanılabilirlik kümesi içinde yerleştirin VM'ler sağlar, işlem rafları, depolama birimi ve ağ anahtarları. Bir donanım veya Azure yazılım hatası oluşursa, yalnızca bir alt kümesini Vm'leriniz etkilendi ve genel uygulamanızı kurma kalır ve müşterileriniz için kullanılabilir olmaya devam eder. Güvenilir bulut çözümleri oluşturmak istediğinizde kullanılabilirlik önemli bir özellik kümesidir.

Şimdi burada 4 ön uç web sunucusu ve bir veritabanı ana bilgisayar 2 arka uç VM kullanmak bir tipik VM tabanlı çözümünü göz önünde bulundurun. Azure ile Vm'leriniz dağıtmadan önce iki kullanılabilirlik kümesi tanımlamak istersiniz: bir kullanılabilirlik kümesi için web katmanı ve bir kullanılabilirlik veritabanı için kümesi. Ardından az VM'ye parametre kullanılabilirlik komutu oluşturun ve Azure otomatik olarak oluşturulan VM'ler sağlar belirleyebilirsiniz yeni bir VM oluştururken kullanılabilir kümesi yalıtılmış birden çok fiziksel donanım kaynaklarına arasında. Bir Web sunucusu veya veritabanı sunucusu sanal makineleri çalıştıran fiziksel donanımı bir sorun varsa, çünkü bunlar üzerinde farklı donanım diğer örneklerini Web sunucusu ve veritabanı VM'ler çalışır durumda bildirin.

Azure VM tabanlı güvenilir çözümlerinde dağıtmak istediğinizde kullanılabilirlik kümelerini kullanın.

## <a name="create-an-availability-set"></a>Kullanılabilirlik kümesi oluşturma

Kullanılabilirlik kümesi kullanarak oluşturabileceğiniz [yeni AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset). Bu örnekte, her iki güncelleştirme ve hata etki alanlarının sayısı ayarlarız *2* kullanılabilirlik adlandırılmış kümesi için *myAvailabilitySet* içinde *myResourceGroupAvailability* kaynak grubu.

Bir kaynak grubu oluşturun.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroupAvailability -Location EastUS
```

Kullanılarak ayarlanan bir yönetilen kullanılabilirlik Oluştur [yeni AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset) ile **- sku hizalı** parametresi.

```azurepowershell-interactive
New-AzureRmAvailabilitySet `
   -Location EastUS `
   -Name myAvailabilitySet `
   -ResourceGroupName myResourceGroupAvailability `
   -sku aligned `
   -PlatformFaultDomainCount 2 `
   -PlatformUpdateDomainCount 2
```

## <a name="create-vms-inside-an-availability-set"></a>VM'ler içinde bir kullanılabilirlik kümesi oluştur

Sanal makineleri kullanılabilirlik donanım üzerinde doğru şekilde dağıtıldığından emin olmak için kümesini içinde oluşturulmalıdır. Kullanılabilirlik oluşturulduktan sonra kümesi için mevcut bir VM'yi ekleyemezsiniz. 

Bir konumda donanım birden çok güncelleştirme etki alanları ve hata etki alanları ayrılmıştır. Bir **güncelleştirme etki alanı** VM'ler ve aynı anda yeniden temel alınan fiziksel donanım oluşan bir gruptur. Sanal makineleri aynı **hata etki alanı** ortak bir güç kaynağı ve ağ anahtarı yanı sıra genel depolama paylaşın. 

Kullanarak bir VM yapılandırması oluşturduğunuzda [yeni AzureRMVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) kullandığınız `-AvailabilitySetId` parametresi kullanılabilirlik kümesi Kimliğini belirtin.

İle iki VM oluşturma [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) kullanılabilirlik kümesinde.

```azurepowershell-interactive
$availabilitySet = Get-AzureRmAvailabilitySet `
    -ResourceGroupName myResourceGroupAvailability `
    -Name myAvailabilitySet

$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupAvailability `
    -Location EastUS `
    -Name myVnet `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig
    
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

$nsg = New-AzureRmNetworkSecurityGroup `
    -Location eastus `
    -Name myNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupAvailability `
    -SecurityRules $nsgRuleRDP
    
# Apply the network security group to a subnet
Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name mySubnet `
    -NetworkSecurityGroup $nsg `
    -AddressPrefix 192.168.1.0/24

# Update the virtual network
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

for ($i=1; $i -le 2; $i++)
{
   $pip = New-AzureRmPublicIpAddress `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -Name "mypublicdns$(Get-Random)" `
        -AllocationMethod Static `
        -IdleTimeoutInMinutes 4

   $nic = New-AzureRmNetworkInterface `
        -Name myNic$i `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -SubnetId $vnet.Subnets[0].Id `
        -PublicIpAddressId $pip.Id `
        -NetworkSecurityGroupId $nsg.Id

   # Here is where we specify the availability set
   $vm = New-AzureRmVMConfig `
        -VMName myVM$i `
        -VMSize Standard_D1 `
        -AvailabilitySetId $availabilitySet.Id

   $vm = Set-AzureRmVMOperatingSystem `
        -ComputerName myVM$i `
        -Credential $cred `
        -VM $vm `
        -Windows `
        -EnableAutoUpdate `
        -ProvisionVMAgent
   $vm = Set-AzureRmVMSourceImage `
        -VM $vm `
        -PublisherName MicrosoftWindowsServer `
        -Offer WindowsServer `
        -Skus 2016-Datacenter `
        -Version latest
   $vm = Set-AzureRmVMOSDisk `
        -VM $vm `
        -Name myOsDisk$i `
        -DiskSizeInGB 128 `
        -CreateOption FromImage `
        -Caching ReadWrite
   $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
   New-AzureRmVM `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -VM $vm
}

```

Oluşturup her iki VM yapılandırmak için birkaç dakika sürer. Tamamlandığında, temel alınan donanım arasında dağıtılmış iki sanal makineye sahip olacaksınız. 

Kullanılabilirlik için kaynak gruplarını giderek portalda kümesini bakarsanız > myResourceGroupAvailability > myAvailabilitySet, sanal makineleri 2 arıza arasında nasıl dağıtıldığını bakın ve güncelleştirme etki alanı gerekir.

![Kullanılabilirlik portalda kümesi](./media/tutorial-availability-sets/fd-ud.png)

## <a name="check-for-available-vm-sizes"></a>Kullanılabilir VM boyutları denetle 

Daha fazla sanal makineleri daha sonra ayarlamak kullanılabilirlik ekleyebilirsiniz, ancak hangi VM boyutları donanımda kullanılabilir olduğunu bilmeniz gerekir. Kullanım [Get-AzureRMVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) tüm donanım üzerinde kullanılabilir boyutları küme için kullanılabilirlik kümesi listelemek için.

```azurepowershell-interactive
Get-AzureRmVMSize `
   -AvailabilitySetName myAvailabilitySet `
   -ResourceGroupName myResourceGroupAvailability  
```

## <a name="check-azure-advisor"></a>Azure Danışmanı'nı denetleyin 

Vm'leriniz kullanılabilirliğini artırmak için yollar hakkında daha fazla bilgi için Azure Danışmanı'nı da kullanabilirsiniz. Azure Danışmanı Azure dağıtımlarınızı iyileştirmek için en iyi uygulamaları izleyerek yardımcı olur. Kaynak yapılandırması ve kullanım telemetri analiz eder ve maliyet verimliliği, performans, yüksek kullanılabilirlik ve Azure kaynaklarınızın güvenliğini geliştirmenize yardımcı olabilir çözümleri önerir.

Oturum [Azure portal](https://portal.azure.com)seçin **daha fazla hizmet**ve türü **Danışmanı**. Advisor Pano seçili abonelik için kişiselleştirilmiş önerileri görüntüler. Daha fazla bilgi için bkz: [Azure Advisor ile çalışmaya başlama](../../advisor/advisor-get-started.md).


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Kullanılabilirlik kümesi oluşturma
> * Bir kullanılabilirlik kümesine bir VM oluşturma
> * Kullanılabilir VM boyutları denetleyin
> * Azure Danışmanı'nı denetleyin

Sanal makine ölçek kümeleri hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [VM ölçek kümesi oluşturma](tutorial-create-vmss.md)


