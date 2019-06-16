---
title: Azure PowerShell ile paylaşılan VM görüntüleri oluşturma | Microsoft Docs
description: Azure'da bir paylaşılan bir sanal makine görüntüsü oluşturmak için Azure PowerShell kullanmayı öğrenin
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/06/2019
ms.author: cynthn
ms.custom: ''
ms.openlocfilehash: c58faa7104a3ca2c740a1d1e35ca5bfd47c3a9fd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66241194"
---
# <a name="create-a-shared-image-gallery-with-azure-powershell"></a>Azure PowerShell ile bir paylaşılan görüntü Galerisi oluşturma 

A [paylaşılan görüntü Galerisi](shared-image-galleries.md) kuruluşunuz genelinde paylaşımı özel görüntü basitleştirir. Özel görüntüler market görüntüleri gibidir, ancak bunları kendiniz oluşturursunuz. Özel görüntüler, uygulamaları, uygulama yapılandırmalarını ve diğer işletim sistemi yapılandırmalarını önceden yükleme gibi dağıtım görevlerini bootstrap için kullanılabilir. 

Paylaşılan görüntü Galerisi özel VM görüntülerinizi başkalarıyla kuruluşunuzdaki içinde veya bölgeler, bir AAD kiracısı arasında paylaşmanıza olanak tanır. Paylaşmak istediğiniz hangi görüntüleri seçin, bunları bulunan ve kendileriyle paylaşmak istediğiniz yapmak istediğiniz hangi bölgeleri. Paylaşılan görüntülerini mantıksal olarak gruplayabilirsiniz, böylece birden fazla galeri oluşturabilirsiniz. 

Galeri tam rol tabanlı erişim denetimi (RBAC) sağlayan bir üst düzey bir kaynaktır. Görüntüleri tutulan olabilir ve farklı bir Azure bölgesi kümesi için her görüntü sürümü çoğaltmak seçebilirsiniz. Galeri, yalnızca yönetilen görüntüleri ile çalışır.

Paylaşılan görüntü Galerisi özelliği, birden çok kaynak türü vardır. Biz kullanarak veya kaldırılacak bunlar bu makaledeki oluşturma:

| Resource | Açıklama|
|----------|------------|
| **Yönetilen bir görüntü** | Bu, tek başına kullanılan veya oluşturmak için kullanılan bir temel görüntü, bir **görüntü sürümü** bir görüntü galerisinde. Yönetilen bir görüntü genelleştirilmiş sanal makinelerinden oluşturulur. Yönetilen bir görüntü, birden çok sanal makine sağlamak için kullanılabilir ve artık paylaşılan görüntü sürümlerini oluşturmak için kullanılan VHD özel türüdür. |
| **Görüntü Galerisi** | Azure Marketi gibi bir **görüntü Galerisi** yönetmek ve görüntüler, ancak kimlerin erişebildiğini siz denetlersiniz paylaşımı için bir depodur. |
| **Görüntü tanımı** | Görüntüleri bir galeri içindeki tanımlanır ve görüntü ve dahili olarak kullanma gereksinimleri hakkında bilgi Yürüt. Bu, görüntünün Windows veya Linux, sürüm notları ve minimum ve maksimum bellek gereksinimleri olup olmadığını içerir. Görüntü türü bir tanımıdır. |
| **Görüntü sürümü** | Bir **görüntü sürümü** bir galeri kullanırken bir VM oluşturmak için kullanın. Görüntünün birden çok sürümü, ortamınız için gerektiği şekilde olabilir. Yönetilen bir görüntü kullanırken gibi bir **görüntü sürümü** bir VM oluşturmak için görüntü sürümü sanal makine için yeni bir disk oluşturmak için kullanılır. Yansıma sürümü birden çok kez kullanılabilir. |

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede örneği tamamlamak için var olan yönetilen bir görüntü olması gerekir. İzleyebileceğiniz [Öğreticisi: Azure PowerShell ile Azure VM'deki özel görüntüsünü oluşturma](tutorial-custom-images.md) gerekirse oluşturmak için. Yönetilen bir görüntü veri diski varsa, veri disk boyutu 1 TB'den fazla olamaz.

Bu makalede, üzerinden geçmeden değiştirdiğinizde kaynak grubu ve VM adlarını gerektiğinde.

[!INCLUDE [virtual-machines-common-shared-images-powershell](../../../includes/virtual-machines-common-shared-images-powershell.md)]

 
## <a name="create-vms-from-an-image"></a>Bir görüntüden VM oluşturma

Görüntü sürümü tamamlandıktan sonra bir veya daha fazla yeni VM'ler oluşturabilirsiniz. Kullanarak [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) cmdlet'i. 

Bu örnek adlı bir VM oluşturur *Myımage*, *myResourceGroup* içinde *Orta Güney ABD* veri merkezi.


```azurepowershell-interactive
$resourceGroup = "myResourceGroup"
$location = "South Central US"
$vmName = "myVMfromImage"

# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a resource group
New-AzResourceGroup -Name $resourceGroup -Location $location

# Network pieces
$subnetConfig = New-AzVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24
$vnet = New-AzVirtualNetwork -ResourceGroupName $resourceGroup -Location $location `
  -Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig
$pip = New-AzPublicIpAddress -ResourceGroupName $resourceGroup -Location $location `
  -Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4
$nsgRuleRDP = New-AzNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
  -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389 -Access Allow
$nsg = New-AzNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location $location `
  -Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP
$nic = New-AzNetworkInterface -Name myNic -ResourceGroupName $resourceGroup -Location $location `
  -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration using $imageVersion.Id to specify the shared image
$vmConfig = New-AzVMConfig -VMName $vmName -VMSize Standard_D1_v2 | `
Set-AzVMOperatingSystem -Windows -ComputerName $vmName -Credential $cred | `
Set-AzVMSourceImage -Id $imageVersion.Id | `
Add-AzVMNetworkInterface -Id $nic.Id

# Create a virtual machine
New-AzVM -ResourceGroupName $resourceGroup -Location $location -VM $vmConfig
```

[!INCLUDE [virtual-machines-common-gallery-list-ps](../../../includes/virtual-machines-common-gallery-list-ps.md)]

[!INCLUDE [virtual-machines-common-shared-images-update-delete-ps](../../../includes/virtual-machines-common-shared-images-update-delete-ps.md)]

## <a name="next-steps"></a>Sonraki adımlar
[Azure Görüntü Oluşturucu (Önizleme)](image-builder-overview.md) bile bunu güncelleştirmek için kullanabilirsiniz, görüntü sürümü oluşturma otomatikleştirilmesine yardımcı olur ve [varolan bir görüntü sürümü yeni bir görüntü sürümü oluşturma](image-builder-gallery-update-image-version.md). 

Paylaşılan görüntü Galerisi kaynak şablonlarını kullanarak da oluşturabilirsiniz. Çeşitli Azure hızlı başlangıç şablonları mevcuttur: 

- [Paylaşılan bir görüntü Galerisi oluşturma](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [Paylaşılan bir görüntü galerisinde bir görüntü tanımı oluşturun](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [Paylaşılan bir görüntü galerisinde görüntü sürümü oluşturma](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [Resmi sürümden bir VM oluşturma](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)

Paylaşılan resim galerileri hakkında daha fazla bilgi için bkz: [genel bakış](shared-image-galleries.md). Sorun yaşarsanız bkz [sorun giderme paylaşılan resim galerileri](troubleshooting-shared-images.md).

