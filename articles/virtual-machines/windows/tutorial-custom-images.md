---
title: Azure PowerShell ile özel VM görüntüleri oluşturma | Microsoft Docs
description: Öğretici - Azure PowerShell kullanarak özel bir VM görüntüsü oluşturun.
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
ms.date: 03/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 443f47b98ea063c6fe1f0b3517c00b6cf3692161
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-custom-image-of-an-azure-vm-using-powershell"></a>Özel bir Azure PowerShell kullanarak bir VM görüntüsü oluşturma

Özel görüntüler market görüntüleri gibidir, ancak bunları kendiniz oluşturursunuz. Özel görüntüler, uygulamaları, uygulama yapılandırmalarını ve diğer işletim sistemi yapılandırmalarını önceden yükleme gibi yapılandırmaları önyüklemek için kullanılabilir. Bu öğreticide, bir Azure sanal makinesine ait kendi özel görüntünüzü oluşturursunuz. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Sysprep ve VM'ler generalize
> * Özel görüntü oluşturma
> * Özel görüntüden VM oluşturma
> * Aboneliğinizdeki tüm görüntüleri listeleme
> * Görüntü silme


## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdaki adımlar, mevcut bir VM’yi alıp, yeni VM örnekleri oluşturmak için kullanabileceğiniz yeniden kullanılabilir bir özel görüntüye dönüştürmeyi ayrıntılı olarak açıklar.

Bu öğreticideki örneği tamamlamak için, mevcut bir sanal makinenizin olması gerekir. Gerekirse, bu [betik örneği](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) sizin için bir tane oluşturabilir. Bu öğreticide çalışırken, gerektiğinde kaynak grubu ve VM adlarını değiştirin.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Yükleme ve yerel olarak PowerShell kullanma seçerseniz, Bu öğretici Modül sürümü 5.6.0 AzureRM gerektirir veya sonraki bir sürümü. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps).

## <a name="prepare-vm"></a>VM hazırlama

Bir sanal makine görüntüsünü oluşturmak için VM genelleme, serbest bırakma ve Azure'da genelleştirilmiş gibi VM kaynak işaretleme VM hazırlamanız gerekir.

### <a name="generalize-the-windows-vm-using-sysprep"></a>Sysprep kullanarak Windows VM generalize

Sysprep tüm kişisel hesap bilgilerinize, başka şeylerin kaldırır ve bir görüntü olarak kullanılacak makine hazırlar. Sysprep hakkında daha fazla ayrıntı için bkz: [kullanım Sysprep nasıl: Giriş](http://technet.microsoft.com/library/bb457073.aspx).


1. Sanal makineye bağlanın.
2. Bir yönetici olarak komut istemi penceresi açın. Dizinine değiştirin *%windir%\system32\sysprep*ve ardından çalıştırın *sysprep.exe*.
3. İçinde **Sistem Hazırlama aracı** iletişim kutusunda *girin sistem Out-of-Box deneyimi (OOBE)*, emin olun *Generalize* onay kutusu seçilidir.
4. İçinde **kapatma seçenekleri**seçin *kapatma* ve ardından **Tamam**.
5. Sysprep tamamlandığında, sanal makineyi kapatır. **VM yeniden başlatma**.

### <a name="deallocate-and-mark-the-vm-as-generalized"></a>VM’yi serbest bırakma ve genelleştirilmiş olarak işaretleme

Bir görüntü oluşturmak için VM serbest ve Azure'da genelleştirilmiş olarak işaretlenen gerekir.

Kullanarak VM serbest [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).

```azurepowershell-interactive
Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM -Force
```

Sanal makine durumunu ayarlamak `-Generalized` kullanarak [Set-AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm). 
   
```azurepowershell-interactive
Set-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM -Generalized
```


## <a name="create-the-image"></a>Görüntü oluşturma

VM görüntüsü kullanarak oluşturabileceğiniz artık [yeni AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) ve [yeni AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage). Aşağıdaki örnek, *myVM* adlı bir VM’den *myImage* adlı bir görüntü oluşturur.

Sanal makine Al. 

```azurepowershell-interactive
$vm = Get-AzureRmVM -Name myVM -ResourceGroupName myResourceGroup
```

Görüntü yapılandırmasını oluşturun.

```azurepowershell-interactive
$image = New-AzureRmImageConfig -Location EastUS -SourceVirtualMachineId $vm.ID 
```

Görüntü oluşturma.

```azurepowershell-interactive
New-AzureRmImage -Image $image -ImageName myImage -ResourceGroupName myResourceGroup
``` 

 
## <a name="create-vms-from-the-image"></a>Görüntüden VM oluşturma

Bir görüntü sahip olduğunuza göre bir veya daha fazla yeni VM'ler görüntüden oluşturabilirsiniz. Özel bir görüntüden bir VM oluşturma, Market görüntüsünü kullanan bir VM oluşturmak için çok benzer. Market görüntüsünü kullandığınızda, görüntü, görüntü sağlayıcısı, teklif, SKU ve sürümü hakkında bilgi sağlamak için gerekir. Basitleştirilmiş parametresini kullanarak ayarlamak için [New-AzureRMVM]() cmdlet'i, yalnızca aynı kaynak grubunda olduğu sürece özel görüntü adı sağlamanız gereken. 

Bu örnek, adlandırılmış bir VM'nin oluşturur *myVMfromImage* gelen *myImage*, *myResourceGroup*.


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

## <a name="image-management"></a>Görüntü yönetimi 

İşte bazı örnekler genel yönetim görüntü görevleri ve bunları tamamlamak nasıl PowerShell kullanarak.

Tüm görüntüleri ada göre listeler.

```azurepowershell-interactive
$images = Find-AzureRMResource -ResourceType Microsoft.Compute/images 
$images.name
```

Görüntüyü silin. Bu örnek, *myResourceGroup* kaynak grubundan *myOldImage* adlı görüntüyü siler.

```azurepowershell-interactive
Remove-AzureRmImage `
    -ImageName myOldImage `
    -ResourceGroupName myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, özel bir VM görüntüsü oluşturdunuz. Şunları öğrendiniz:

> [!div class="checklist"]
> * Sysprep ve VM'ler generalize
> * Özel görüntü oluşturma
> * Özel görüntüden VM oluşturma
> * Aboneliğinizdeki tüm görüntüleri listeleme
> * Görüntü silme

Nasıl yüksek oranda kullanılabilir sanal makineler hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Yüksek oranda kullanılabilir VM’ler oluşturma](tutorial-availability-sets.md)



