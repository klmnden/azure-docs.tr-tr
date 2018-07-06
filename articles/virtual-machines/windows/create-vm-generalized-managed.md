---
title: Azure'da yönetilen bir görüntüden VM oluşturma | Microsoft Docs
description: Genelleştirilmiş bir yönetilen görüntüsü Resource Manager dağıtım modelinde Azure PowerShell veya Azure portalını kullanarak bir Windows sanal makine oluşturun.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/27/2017
ms.author: cynthn
ms.openlocfilehash: 6baf784068b1fba0c35d2848b8d2dda4f1064a2d
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37867989"
---
# <a name="create-a-vm-from-a-managed-image"></a>Yönetilen bir görüntüden VM oluşturma

PowerShell veya Azure portalını kullanarak yönetilen bir sanal makine görüntüsünden birden çok VM oluşturabilirsiniz. Yönetilen bir VM görüntüsü işletim sistemi ve veri diskleri dahil olmak üzere, bir VM oluşturmak gerekli bilgileri içerir. Görüntüyü oluşturan hem işletim sistemi diskleri ve veri diskleri dahil olmak üzere, yönetilen disk olarak depolanmış VHD. 

Zaten sahip olması [yönetilen bir VM görüntüsünün oluşturulup](capture-image-resource.md) yeni bir VM oluşturmak için kullanılacak. 

## <a name="use-the-portal"></a>Portalı kullanma

1. [Azure portalı](https://portal.azure.com) açın.
2. Soldaki menüden **Tüm kaynaklar**'ı seçin. Kaynaklara göre sıralayabilirsiniz **türü** görüntülerinizi kolayca bulmak için.
3. Listeden kullanmak istediğiniz görüntüyü seçin. Görüntü **genel bakış** sayfası açılır.
4. Tıklayın **+ Oluştur VM** menüsünde.
5. Sanal makine bilgilerini girin. Burada girilen kullanıcı adı ve parola, sanal makinede oturum açarken kullanılır. İşlem tamamlandığında **Tamam**’a tıklayın. Mevcut bir kaynak grubunda yeni bir VM oluşturun veya seçin **Yeni Oluştur** VM depolamak için yeni bir kaynak grubu oluşturmak için.
6. VM için bir boyut seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. 
7. Altında **ayarları**, gerekli değişiklikleri yapın ve tıklayın **Tamam**. 
8. Özet sayfasında, listelenen görüntü adınızı görmelisiniz **özel görüntü**. Tıklayın **Tamam** sanal makine dağıtımını başlatın.


## <a name="use-powershell"></a>PowerShell kullanma

Basitleştirilmiş parametre kümesi kullanarak görüntüden bir VM oluşturmak için PowerShell kullanabilirsiniz [New-AzureRmVm](/powershell/module/azurerm.compute/new-azurermvm) cmdlet'i. Görüntü, VM oluşturmak için istediğiniz aynı kaynak grubunda olması gerekiyor.

Bu örnek AzureRM modülü 5.6.0 bir sürümü gerektirir veya üzeri. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps).

New-AzureRmVm adı, bir görüntüden bir VM oluşturmak için kaynak grubu ve görüntü adı sağlayın, ancak değerini kullanacağınız yalnızca gerektirir ayarlamanız Basitleştirilmiş parametre **-adı** parametre olarak tüm kaynakları adı olan otomatik olarak oluşturur. Bu örnekte, biz daha ayrıntılı adları her kaynak sağlar ancak onları otomatik olarak oluşturmasını cmdlet'i sağlar. Ayrıca, önceden sanal ağ gibi kaynakları oluşturabilir ve adı cmdlet'e geçirin. Bunları adlarına göre bulabilirsiniz, varolan kaynakları kullanır.

Aşağıdaki örnekte adlı bir VM oluşturur *Myımage*, *myResourceGroup* kaynak grubundan görüntüsünü *Myımage*. 


```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVMfromImage" `
    -ImageName "myImage" `
    -Location "East US" `
    -VirtualNetworkName "myImageVnet" `
    -SubnetName "myImageSubnet" `
    -SecurityGroupName "myImageNSG" `
    -PublicIpAddressName "myImagePIP" `
    -OpenPorts 3389
```



## <a name="next-steps"></a>Sonraki adımlar
Azure PowerShell ile yeni sanal makinenize yönetmek için bkz: [oluşturun ve Azure PowerShell modülü ile Windows Vm'leri yönetme](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

