---
title: "Bir Azure sanal makine ölçek kümesi oluşturma | Microsoft Docs"
description: "Oluşturun ve Azure CLI, PowerShell, şablon veya Visual Studio ile ayarlanmış bir Linux veya Windows Azure sanal makine ölçek dağıtın."
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 07/21/2017
ms.author: adegeo
ms.openlocfilehash: 32af01aa545c541688128a7ae6bbb82a0e046f2d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-deploy-a-virtual-machine-scale-set"></a>Oluşturma ve bir sanal makine ölçek kümesini dağıtma
Sanal makine ölçek kümeleri dağıtmak ve bir küme olarak aynı sanal makineleri yönetmek için kolay hale getirir. Ölçek kümeleri ölçekte uygulamalar için yüksek oranda ölçeklenebilir ve özelleştirilebilir bilgi işlem katmanını sağlar ve Windows platform görüntüleri, Linux platform görüntüleri, özel resimler ve uzantıları destekler. Ölçek kümeleri hakkında daha fazla bilgi için bkz: [sanal makine ölçek kümeleri](virtual-machine-scale-sets-overview.md).

Bu öğreticide, bir sanal makine ölçek kümesi oluşturmak nasıl gösterilir **olmadan** Azure portalını kullanarak. Azure Portalı'nı kullanma hakkında daha fazla bilgi için bkz: [kümesinin nasıl bir sanal makine ölçek oluşturulacağı Azure portalıyla](virtual-machine-scale-sets-portal-create.md).

>[!NOTE]
>Azure Resource Manager kaynakları hakkında daha fazla bilgi için bkz: [Azure Resource Manager ve klasik dağıtım](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Bir ölçek kümesi oluşturmak için Azure CLI 2.0 veya Azure PowerShell kullanıyorsanız, önce aboneliğinize imzalamanız gerekir.

Yüklemek, ayarlayın ve Azure CLI veya PowerShell ile Azure oturumu hakkında daha fazla bilgi için bkz: [Azure CLI 2.0 ile çalışmaya başlama](/cli/azure/get-started-with-azure-cli.md) veya [Azure PowerShell cmdlet'leri kullanmaya başlama](/powershell/azure/overview).

```azurecli
az login
```

```powershell
Login-AzureRmAccount
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

İlk sanal makine ölçek kümesi ile ilişkili bir kaynak grubu oluşturmanız gerekir.

```azurecli
az group create --location westus2 --name MyResourceGroup1
```

```powershell
New-AzureRmResourceGroup -Location westus2 -Name MyResourceGroup1
```

## <a name="create-from-azure-cli"></a>Azure CLI üzerinden oluşturma

Azure CLI ile en az çaba ile ayarlanmış bir sanal makine ölçek oluşturabilirsiniz. Varsayılan değerleri atlarsanız, bunlar sizin için sağlanır. Örneğin, herhangi bir sanal ağ bilgi belirtmezseniz, bir sanal ağ sizin için oluşturulur. Aşağıdaki bölümleri atlarsanız, bunlar sizin için oluşturulur: 
- Bir yük dengeleyici
- Bir sanal ağ
- Bir ortak IP adresi

Sanal makine ölçek kümesi üzerinde kullanmak istediğiniz sanal makine görüntüsü seçerken, birkaç seçeneğiniz vardır:

- URN  
Bir kaynak tanımlayıcısı:  
**Win2012R2Datacenter**

- URN diğer adı  
Bir URN kolay adı:  
**MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest**

- Özel kaynak kimliği  
Bir Azure kaynağı yolu:  
**/Subscriptions/Subscription-guid/resourceGroups/MyResourceGroup/providers/Microsoft.COMPUTE/images/MyImage**

- Web kaynağı  
Bir HTTP URI yolu:  
**http://contoso.BLOB.Core.Windows.NET/vhds/osdiskimage.vhd**

>[!TIP]
>Kullanılabilir görüntülerle listesini almak `az vm image list`.

Bir sanal makine ölçek kümesi oluşturmak için aşağıdakileri belirtmeniz gerekir:

- Kaynak grubu 
- Ad
- İşletim sistemi görüntüsü
- Kimlik doğrulama bilgileri 
 
Aşağıdaki örnek, (Bu adım birkaç dakika sürebilir) temel sanal makine ölçek kümesi oluşturur.

```azurecli
az vmss create --resource-group MyResourceGroup1 --name MyScaleSet --image UbuntuLTS --authentication-type password --admin-username azureuser --admin-password P@ssw0rd!
```

Komut bittikten sonra oluşturulan ayarlayın, sanal makine ölçek şimdi sahip olur. Sanal makinenin IP adresi için bağlanabilmesi yapmanız gerekebilir. Aşağıdaki komutla bir çok sayıda sanal makine (IP adresi dahil) hakkında farklı bilgi elde edebilirsiniz. 

```azurecli
az vmss list-instance-connection-info --resource-group MyResourceGroup1 --name MyScaleSet
```

## <a name="create-from-powershell"></a>Powershell'den oluşturma

PowerShell Azure CLI kullanmak çok daha karmaşıktır. Azure CLI ağla ilgili kaynaklar (örneğin, yük Dengeleyiciler, IP adresleri ve sanal ağları) için varsayılan sağlarken, PowerShell desteklemez. PowerShell ile bir görüntü başvuran bir biraz daha çok karmaşıktır. Aşağıdaki cmdlet ile görüntüleri alabilirsiniz:

1. Get-AzureRMVMImagePublisher
2. Get-AzureRMVMImageOffer
3. Get-AzureRmVMImageSku

Cmdlet'leri iş sırayla yöneltilen. İşte bir örnek için tüm görüntüleri alma **Batı ABD 2** bölge adına sahip bir yayımcı ile **microsoft** da.

```powershell
Get-AzureRMVMImagePublisher -Location WestUS2 | Where-Object PublisherName -Like *microsoft* | Get-AzureRMVMImageOffer | Get-AzureRmVMImageSku | Select-Object PublisherName, Offer, Skus
```

```
PublisherName              Offer                    Skus
-------------              -----                    ----
microsoft-ads              linux-data-science-vm    linuxdsvm
microsoft-ads              standard-data-science-vm standard-data-science-vm
MicrosoftAzureSiteRecovery Process-Server           Windows-2012-R2-Datacenter
MicrosoftBizTalkServer     BizTalk-Server           2013-R2-Enterprise
MicrosoftBizTalkServer     BizTalk-Server           2013-R2-Standard
MicrosoftBizTalkServer     BizTalk-Server           2016-Developer
MicrosoftBizTalkServer     BizTalk-Server           2016-Enterprise
...
```

Bir sanal makine ölçek kümesi oluşturmak için iş akışı aşağıdaki gibidir:

1. Ölçek kümesi hakkında bilgi içeren bir yapılandırma nesnesi oluşturun.
2. Temel işletim sistemi görüntüsü başvuru.
3. İşletim sistemi ayarlarını yapılandırın: kimlik doğrulaması, VM adı ön ekini ve kullanıcı/geçirin.
4. Ağ yapılandırın.
5. Ölçek kümesi oluşturun.

Bu örnek, Windows Server 2016 sahip bir bilgisayar için temel bir iki örnekli ölçek oluşturur.

```powershell
# Resource group name from above
$rg = "MyResourceGroup1"
$location = "WestUS2"

# Create a config object
$vmssConfig = New-AzureRmVmssConfig -Location $location -SkuCapacity 2 -SkuName Standard_A0  -UpgradePolicyMode Automatic

# Reference a virtual machine image from the gallery
Set-AzureRmVmssStorageProfile $vmssConfig -ImageReferencePublisher MicrosoftWindowsServer -ImageReferenceOffer WindowsServer -ImageReferenceSku 2016-Datacenter -ImageReferenceVersion latest

# Set up information for authenticating with the virtual machine
Set-AzureRmVmssOsProfile $vmssConfig -AdminUsername azureuser -AdminPassword P@ssw0rd! -ComputerNamePrefix myvmssvm

# Create the virtual network resources

## Basics
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name "my-subnet" -AddressPrefix 10.0.0.0/24
$vnet = New-AzureRmVirtualNetwork -Name "my-network" -ResourceGroupName $rg -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $subnet

## Load balancer
$publicIP = New-AzureRmPublicIpAddress -Name "PublicIP" -ResourceGroupName $rg -Location $location -AllocationMethod Static -DomainNameLabel "myuniquedomain"
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontend" -PublicIpAddress $publicIP
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"
$probe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2
$inboundNATRule1= New-AzureRmLoadBalancerRuleConfig -Name "webserver" -FrontendIpConfiguration $frontendIP -Protocol Tcp -FrontendPort 80 -BackendPort 80 -IdleTimeoutInMinutes 15 -Probe $probe -BackendAddressPool $backendPool
$inboundNATPool1 = New-AzureRmLoadBalancerInboundNatPoolConfig -Name "RDP" -FrontendIpConfigurationId $frontendIP.Id -Protocol TCP -FrontendPortRangeStart 53380 -FrontendPortRangeEnd 53390 -BackendPort 3389

New-AzureRmLoadBalancer -ResourceGroupName $rg -Name "LB1" -Location $location -FrontendIpConfiguration $frontendIP -LoadBalancingRule $inboundNATRule1 -InboundNatPool $inboundNATPool1 -BackendAddressPool $backendPool -Probe $probe

## IP address config
$ipConfig = New-AzureRmVmssIpConfig -Name "my-ipaddress" -LoadBalancerBackendAddressPoolsId $backendPool.Id -SubnetId $vnet.Subnets[0].Id -LoadBalancerInboundNatPoolsId $inboundNATPool1.Id

# Attach the virtual network to the IP object
Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmssConfig -Name "network-config" -Primary $true -IPConfiguration $ipConfig

# Create the scale set with the config object (this step might take a few minutes)
New-AzureRmVmss -ResourceGroupName $rg -Name "MyScaleSet1" -VirtualMachineScaleSet $vmssConfig
```

### <a name="using-a-custom-virtual-machine-image"></a>Bir özel sanal makine görüntüsü kullanma
Galeriden bir sanal makine görüntüsü başvuran yerine kendi özel görüntünüzü kümeden bir ölçek oluşturuyorsanız _kümesi AzureRmVmssStorageProfile_ komutu şuna benzeyebilir:
```PowerShell
Set-AzureRmVmssStorageProfile -OsDiskCreateOption FromImage -ManagedDisk PremiumLRS -OsDiskCaching "None" -OsDiskOsType Linux -ImageReferenceId (Get-AzureRmImage -ImageName $VMImage -ResourceGroupName $rg).id
```

## <a name="create-from-a-template"></a>Şablondan oluşturma

Bir Azure Resource Manager şablonu kullanarak bir sanal makine ölçek dağıtabilirsiniz. Kendi şablonunuzu oluşturun ya da birinden kullanmak [şablonu deposu](https://azure.microsoft.com/resources/templates/?term=vmss). Bu şablonları Azure aboneliğinize doğrudan dağıtılabilir.

>[!NOTE]
>Kendi şablonunuzu oluşturmak için JSON metin dosyası oluşturun. Oluşturma ve bir şablonu özelleştirme hakkında genel bilgi için bkz: [Azure Resource Manager şablonları](../azure-resource-manager/resource-group-authoring-templates.md).

Örnek şablon kullanılabilir [github'da](https://github.com/gatneil/mvss/tree/minimum-viable-scale-set). Oluşturun ve bu örneği kullanmak hakkında daha fazla bilgi için bkz: [en uygun ölçek kümesini](.\virtual-machine-scale-sets-mvss-start.md).

## <a name="create-from-visual-studio"></a>Visual Studio'dan oluşturma

Visual Studio ile bir Azure kaynak grubu projesi oluşturun ve şablon için bir sanal makine ölçek kümesi ekleyin. GitHub veya Azure Web uygulama Galerisi almak isteyip istemediğinizi seçebilirsiniz. Bir dağıtım PowerShell komut dosyası ayrıca sizin için oluşturulur. Daha fazla bilgi için bkz: [kümesinin Visual Studio ile nasıl bir sanal makine ölçek oluşturulacağı](virtual-machine-scale-sets-vs-create.md).

## <a name="create-from-the-azure-portal"></a>Azure portalından oluşturmak

Azure portal, hızlı bir şekilde bir ölçek kümesi oluşturmak için kolay bir yol sağlar. Daha fazla bilgi için bkz: [kümesinin nasıl bir sanal makine ölçek oluşturulacağı Azure portalıyla](virtual-machine-scale-sets-portal-create.md).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek [veri diskleri](virtual-machine-scale-sets-attached-disks.md).

Bilgi edinmek için nasıl [uygulamalarınızı yönetmek](virtual-machine-scale-sets-deploy-app.md).
