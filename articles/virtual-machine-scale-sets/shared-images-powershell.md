---
title: Paylaşılan bir ölçek kümesi Azure'da oluşturmak için VM görüntüleri kullan | Microsoft Docs
description: Azure'da sanal makine ölçek kümeleri dağıtmak için kullanılacak paylaşılan VM görüntüleri oluşturmak için Azure PowerShell kullanmayı öğrenin.
services: virtual-machine-scale-sets
documentationcenter: ''
author: axayjo
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2019
ms.author: akjosh; cynthn
ms.custom: ''
ms.openlocfilehash: 055242c3118ce9d972d55cdc6a21bf623679a0c1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66242043"
---
# <a name="create-and-use-shared-images-for-virtual-machine-scale-sets-with-the-azure-powershell"></a>Oluşturma ve Azure PowerShell ile sanal makine ölçek kümeleri için paylaşılan görüntülerini kullanma

Ölçek kümesi oluşturduğunuzda, sanal makine örnekleri dağıtılırken kullanılacak bir görüntü belirtirsiniz. Paylaşılan görüntü Galerisi hizmeti kuruluşunuz genelinde paylaşımı özel görüntü büyük ölçüde kolaylaştırır. Özel görüntüler market görüntüleri gibidir, ancak bunları kendiniz oluşturursunuz. Özel görüntüler, uygulamaları, uygulama yapılandırmalarını ve diğer işletim sistemi yapılandırmalarını önceden yükleme gibi yapılandırmaları önyüklemek için kullanılabilir. 

Paylaşılan görüntü Galerisi özel VM görüntülerinizi başkalarıyla kuruluşunuzdaki içinde veya bölgeler, bir AAD kiracısı arasında paylaşmanıza olanak tanır. Paylaşmak istediğiniz hangi görüntüleri seçin, bunları bulunan ve kendileriyle paylaşmak istediğiniz yapmak istediğiniz hangi bölgeleri. Paylaşılan görüntülerini mantıksal olarak gruplayabilirsiniz, böylece birden fazla galeri oluşturabilirsiniz. 

Galeri tam rol tabanlı erişim denetimi (RBAC) sağlayan bir üst düzey bir kaynaktır. Görüntüleri tutulan olabilir ve farklı bir Azure bölgesi kümesi için her görüntü sürümü çoğaltmak seçebilirsiniz. Galeri, yalnızca yönetilen görüntüleri ile çalışır. 

Paylaşılan görüntü Galerisi özelliği, birden çok kaynak türü vardır. Biz kullanarak veya kaldırılacak bunlar bu makaledeki oluşturma:

| Resource | Açıklama|
|----------|------------|
| **Yönetilen bir görüntü** | Bu, tek başına kullanılan veya oluşturmak için kullanılan bir temel görüntü, bir **görüntü sürümü** bir görüntü galerisinde. Yönetilen bir görüntü genelleştirilmiş sanal makinelerinden oluşturulur. Yönetilen bir görüntü, birden çok sanal makine sağlamak için kullanılabilir ve artık paylaşılan görüntü sürümlerini oluşturmak için kullanılan VHD özel türüdür. |
| **Görüntü Galerisi** | Azure Marketi gibi bir **görüntü Galerisi** yönetmek ve görüntüler, ancak kimlerin erişebildiğini siz denetlersiniz paylaşımı için bir depodur. |
| **Görüntü tanımı** | Görüntüleri bir galeri içindeki tanımlanır ve görüntü ve dahili olarak kullanma gereksinimleri hakkında bilgi Yürüt. Bu, görüntünün Windows veya Linux, sürüm notları ve minimum ve maksimum bellek gereksinimleri olup olmadığını içerir. Görüntü türü bir tanımıdır. |
| **Görüntü sürümü** | Bir **görüntü sürümü** bir galeri kullanırken bir VM oluşturmak için kullanın. Görüntünün birden çok sürümü, ortamınız için gerektiği şekilde olabilir. Yönetilen bir görüntü kullanırken gibi bir **görüntü sürümü** bir VM oluşturmak için görüntü sürümü sanal makine için yeni bir disk oluşturmak için kullanılır. Yansıma sürümü birden çok kez kullanılabilir. |

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [updated-for-az.md](../../includes/updated-for-az.md)]

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdaki adımlarda, mevcut bir VM'yi alıp yeni VM örnekleri oluşturmak için kullanabileceğiniz yeniden kullanılabilir bir özel görüntüye dönüştürme işlemi ayrıntılı olarak açıklanmıştır.

Bu makalede örneği tamamlamak için var olan yönetilen bir görüntü olması gerekir. İzleyebileceğiniz [Öğreticisi: Oluşturma ve Azure PowerShell ile sanal makine ölçek kümeleri için özel görüntü kullanma](tutorial-use-custom-image-powershell.md) gerekirse oluşturmak için. Yönetilen bir görüntü veri diski varsa, veri disk boyutu 1 TB'den fazla olamaz.

Makale üzerinden geçmeden değiştirdiğinizde kaynak grubu ve VM adlarını gerektiğinde.


[!INCLUDE [virtual-machines-common-shared-images-ps](../../includes/virtual-machines-common-shared-images-powershell.md)]

## <a name="create-a-scale-set-from-the-shared-image-version"></a>Ölçek kümesi paylaşılan görüntü sürümü oluşturma

Bir sanal makine ölçek kümesi oluşturma [yeni AzVmss](/powershell/module/az.compute/new-azvmss). Aşağıdaki örnek, Ölçek kümesi yeni görüntü sürümü oluşturur *Orta Güney ABD* veri merkezi. İstendiğinde ölçek kümesindeki VM örnekleri için yönetici kendi kimlik bilgilerini ayarlayın:


```azurepowershell-interactive
# Define variables
$resourceGroupName = "myVMSSRG"
$scaleSetName = "myScaleSet"
$location = "South Central US"
$cred = Get-Credential

# Create a resource group
New-AzResourceGroup -ResourceGroupName $resourceGroupName -Location $location

# Create a netowrking pieces
$subnet = New-AzVirtualNetworkSubnetConfig `
  -Name "mySubnet" `
  -AddressPrefix 10.0.0.0/24
$vnet = New-AzVirtualNetwork `
  -ResourceGroupName $resourceGroupName `
  -Name "myVnet" `
  -Location $location `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $subnet
$publicIP = New-AzPublicIpAddress `
  -ResourceGroupName $resourceGroupName `
  -Location $location `
  -AllocationMethod Static `
  -Name "myPublicIP"
$frontendIP = New-AzLoadBalancerFrontendIpConfig `
  -Name "myFrontEndPool" `
  -PublicIpAddress $publicIP
$backendPool = New-AzLoadBalancerBackendAddressPoolConfig -Name "myBackEndPool"
$inboundNATPool = New-AzLoadBalancerInboundNatPoolConfig `
  -Name "myRDPRule" `
  -FrontendIpConfigurationId $frontendIP.Id `
  -Protocol TCP `
  -FrontendPortRangeStart 50001 `
  -FrontendPortRangeEnd 50010 `
  -BackendPort 3389
# Create the load balancer and health probe
$lb = New-AzLoadBalancer `
  -ResourceGroupName $resourceGroupName `
  -Name "myLoadBalancer" `
  -Location $location `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool `
  -InboundNatPool $inboundNATPool
Add-AzLoadBalancerProbeConfig -Name "myHealthProbe" `
  -LoadBalancer $lb `
  -Protocol TCP `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
Add-AzLoadBalancerRuleConfig `
  -Name "myLoadBalancerRule" `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol TCP `
  -FrontendPort 80 `
  -BackendPort 80 `
  -Probe (Get-AzLoadBalancerProbeConfig -Name "myHealthProbe" -LoadBalancer $lb)
Set-AzLoadBalancer -LoadBalancer $lb

# Create IP address configurations
$ipConfig = New-AzVmssIpConfig `
  -Name "myIPConfig" `
  -LoadBalancerBackendAddressPoolsId $lb.BackendAddressPools[0].Id `
  -LoadBalancerInboundNatPoolsId $inboundNATPool.Id `
  -SubnetId $vnet.Subnets[0].Id

# Create a configuration 
$vmssConfig = New-AzVmssConfig `
    -Location $location `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode "Automatic"

# Reference the image version
Set-AzVmssStorageProfile $vmssConfig `
  -OsDiskCreateOption "FromImage" `
  -Id $imageVersion.Id

# Complete the configuration
Set-AzVmssOsProfile $vmssConfig `
  -AdminUsername $cred.UserName `
  -AdminPassword $cred.Password `
  -ComputerNamePrefix "myVM"
Add-AzVmssNetworkInterfaceConfiguration `
  -VirtualMachineScaleSet $vmssConfig `
  -Name "network-config" `
  -Primary $true `
  -IPConfiguration $ipConfig

# Create the scale set 
New-AzVmss `
  -ResourceGroupName $resourceGroupName `
  -Name $scaleSetName `
  -VirtualMachineScaleSet $vmssConfig
```

Tüm ölçek kümesi kaynaklarının ve VM'lerin oluşturulup yapılandırılması birkaç dakika sürer.

[!INCLUDE [virtual-machines-common-gallery-list-ps](../../includes/virtual-machines-common-gallery-list-ps.md)]

[!INCLUDE [virtual-machines-common-shared-images-update-delete-ps](../../includes/virtual-machines-common-shared-images-update-delete-ps.md)]


## <a name="next-steps"></a>Sonraki adımlar

Paylaşılan görüntü Galerisi kaynak şablonlarını kullanarak da oluşturabilirsiniz. Çeşitli Azure hızlı başlangıç şablonları mevcuttur: 

- [Paylaşılan bir görüntü Galerisi oluşturma](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [Paylaşılan bir görüntü galerisinde bir görüntü tanımı oluşturun](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [Paylaşılan bir görüntü galerisinde görüntü sürümü oluşturma](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [Resmi sürümden bir VM oluşturma](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)

Paylaşılan resim galerileri hakkında daha fazla bilgi için bkz: [genel bakış](shared-image-galleries.md). Sorun yaşarsanız bkz [sorun giderme paylaşılan resim galerileri](troubleshooting-shared-images.md).
