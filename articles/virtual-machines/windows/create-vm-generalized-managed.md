---
title: Azure yönetilen bir görüntüden VM oluşturma | Microsoft Docs
description: Azure PowerShell veya Azure portalında Resource Manager dağıtım modelinde kullanılarak genelleştirilmiş bir yönetilen görüntüsünden bir Windows sanal makine oluşturun.
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
ms.openlocfilehash: 1d543bd9590664e74cff70cf55e8f7bd42f2c6f0
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
ms.locfileid: "30239064"
---
# <a name="create-a-vm-from-a-managed-image"></a>Yönetilen bir görüntüden bir VM oluşturma

Birden çok VM PowerShell veya Azure portalını kullanarak yönetilen bir VM görüntüsünü oluşturabilirsiniz. Yönetilen bir VM görüntüsü, işletim sistemi ve veri diskleri de dahil olmak üzere, bir VM oluşturmak gerekli bilgileri içerir. İşletim sistemi ve tüm veri disklerle hem de dahil olmak üzere görüntüyü oluşturan, yönetilen diskleri olarak depolanan VHD'ler. 

Zaten olmasına gerek [yönetilen bir VM görüntüsü oluşturulan](capture-image-resource.md) yeni VM oluşturmak için kullanılacak. 

## <a name="use-the-portal"></a>Portalı kullanma

1. [Azure portalı](https://portal.azure.com) açın.
2. Soldaki menüden **Tüm kaynaklar**'ı seçin. Kaynaklar tarafından sıralayabilirsiniz **türü** görüntülerinizi kolayca bulmak için.
3. Listeden kullanmak istediğiniz görüntüyü seçin. Görüntü **genel bakış** sayfası açılır.
4. Tıklatın **+ Oluştur VM** menüsünde.
5. Sanal makine bilgilerini girin. Burada girilen kullanıcı adı ve parola, sanal makinede oturum açarken kullanılır. İşlem tamamlandığında **Tamam**’a tıklayın. Var olan bir Resrouce grubuna yeni bir VM oluşturun veya seçin **Yeni Oluştur** VM depolamak için yeni bir kaynak grubu oluşturmak için.
6. VM için bir boyut seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. 
7. Altında **ayarları**, gerekli değişiklikleri yapın ve tıklayın **Tamam**. 
8. Özet sayfasında, görüntü adı için listelenen görmelisiniz **özel görüntü**. Tıklatın **Tamam** sanal makine dağıtımı başlatmak için.


## <a name="use-powershell"></a>PowerShell kullanma

Bir VM için Basitleştirilmiş parametresini kullanarak bir görüntü oluşturmak için PowerShell kullanma [New-AzureRmVm](/powershell/module/azurerm.compute/new-azurermvm) cmdlet'i. Görüntü VM'yi oluşturmak istediğiniz aynı kaynak grubunda olması gerekir.

Bu örnek, Modül sürümü 5.6.0 AzureRM gerektirir veya sonraki bir sürümü. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps).

Basitleştirilmiş parametre adı, bir görüntüden bir VM oluşturmak için kaynak grubu ve görüntü adı sağlayın, ancak değerini kullanacak yeni-AzureRmVm yalnızca gerektirir kümesi **-ad** parametre adı olarak tüm kaynakları olan otomatik olarak oluşturur. Bu örnekte, biz her kaynağın ayrıntılı adları sağlar ancak onları otomatik olarak oluşturmasını cmdlet'i sağlar. Ayrıca, önceden sanal ağ gibi kaynakları oluşturun ve adı cmdlet'e geçirin. Bu onları adına göre bulursanız, var olan kaynakları kullanır.

Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myVMFromImage*, *myResourceGroup* kaynak grubundan adlı görüntü *myImage*. 


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
Yeni sanal makinenizi Azure PowerShell ile yönetmek için bkz: [oluşturma ve Azure PowerShell modülü ile Windows sanal makineleri yönetme](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

