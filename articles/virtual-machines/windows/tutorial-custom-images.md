---
title: Öğretici - Azure PowerShell ile özel VM görüntüleri oluşturma | Microsoft Docs
description: Bu öğreticide, Azure PowerShell kullanarak Azure’da özel sanal makine görüntüsü oluşturmayı öğrenirsiniz
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 4d8e8dabf9d6977393158ad716c8e8f3dc8d1512
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51710499"
---
# <a name="tutorial-create-a-custom-image-of-an-azure-vm-with-azure-powershell"></a>Öğretici: Azure PowerShell ile bir Azure VM'nin özel görüntüsünü oluşturma

Özel görüntüler market görüntüleri gibidir, ancak bunları kendiniz oluşturursunuz. Özel görüntüler, uygulamaları, uygulama yapılandırmalarını ve diğer işletim sistemi yapılandırmalarını önceden yükleme gibi yapılandırmaları önyüklemek için kullanılabilir. Bu öğreticide, bir Azure sanal makinesine ait kendi özel görüntünüzü oluşturursunuz. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Sysprep ve VM’leri genelleştirme
> * Özel görüntü oluşturma
> * Özel görüntüden VM oluşturma
> * Aboneliğinizdeki tüm görüntüleri listeleme
> * Görüntü silme

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdaki adımlar, mevcut bir VM’yi alıp, yeni VM örnekleri oluşturmak için kullanabileceğiniz yeniden kullanılabilir bir özel görüntüye dönüştürmeyi ayrıntılı olarak açıklar.

Bu öğreticideki örneği tamamlamak için, mevcut bir sanal makinenizin olması gerekir. Gerekirse, bu [betik örneği](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) sizin için bir tane oluşturabilir. Bu öğreticide çalışırken, gerektiğinde kaynak grubu ve VM adlarını değiştirin.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici, AzureRM modülü 5.7.0 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps).

## <a name="prepare-vm"></a>VM'yi hazırlama

Sanal makinenin görüntüsünü oluşturmak için, VM'yi genelleştirerek, serbest bırakarak ve ardından kaynak VM'yi Azure'da genelleştirilmiş olarak işaretleyerek VM’yi hazırlamanız gerekir.

### <a name="generalize-the-windows-vm-using-sysprep"></a>Sysprep kullanarak Windows VM'sini genelleştirme

Sysprep diğer öğelerin yanı sıra tüm kişisel hesap bilgilerinizi kaldırır ve makineyi bir görüntü olarak kullanılacak şekilde hazırlar. Sysprep hakkındaki ayrıntılar için bkz. [Sysprep İşlemini Kullanma: Giriş](https://technet.microsoft.com/library/bb457073.aspx).


1. Sanal makineye bağlanın.
2. Yönetici olarak Komut İstemi penceresini açın. *%windir%\system32\sysprep* dizinine geçin ve *sysprep.exe*'yi çalıştırın.
3. **Sistem Hazırlama Aracı** iletişim kutusunda  *Sistem İlk Çalıştırma Deneyimi (OOBE) Moduna Gir*'i seçin ve *Genelleştir* onay kutusunun seçili olduğundan emin olun.
4. **Kapatma Seçenekleri**'nde *Kapat*'ı seçin ve **Tamam**'a tıklayın.
5. Sysprep tamamlandığında, sanal makineyi kapatır. **VM'yi yeniden başlatmayın**.

### <a name="deallocate-and-mark-the-vm-as-generalized"></a>VM’yi serbest bırakma ve genelleştirilmiş olarak işaretleme

Görüntü oluşturmak için, VM'nin serbest bırakılması ve Azure'da genelleştirilmiş olarak işaretlenmesi gerekir.

[Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) komutunu kullanarak VM'yi serbest bırakın.

```azurepowershell-interactive
Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM -Force
```

[Set-AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm) komutunu kullanarak sanal makinenin durumunu `-Generalized` olarak ayarlayın. 
   
```azurepowershell-interactive
Set-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM -Generalized
```


## <a name="create-the-image"></a>Görüntü oluşturma

Şimdi [New-AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) ve [New-AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage) komutlarını kullanarak sanal makinenin görüntüsünü oluşturabilirsiniz. Aşağıdaki örnek, *myVM* adlı bir VM’den *myImage* adlı bir görüntü oluşturur.

Sanal makineyi alın. 

```azurepowershell-interactive
$vm = Get-AzureRmVM -Name myVM -ResourceGroupName myResourceGroup
```

Görüntü yapılandırması oluşturun.

```azurepowershell-interactive
$image = New-AzureRmImageConfig -Location EastUS -SourceVirtualMachineId $vm.ID 
```

Görüntü oluşturun.

```azurepowershell-interactive
New-AzureRmImage -Image $image -ImageName myImage -ResourceGroupName myResourceGroup
``` 

 
## <a name="create-vms-from-the-image"></a>Görüntüden VM oluşturma

Artık bir görüntünüz olduğuna göre, görüntüden bir veya daha fazla yeni VM oluşturabilirsiniz. Özel görüntüden VM oluşturma işlemi, Market görüntüsü kullanarak VM oluşturmaya benzer. Market görüntüsünü kullandığınızda, görüntü, görüntü sağlayıcısı, teklif, SKU ve sürüm hakkındaki bilgileri sağlamanız gerekir. [New-AzureRMVM]() cmdlet'inin basitleştirilmiş parametre kümesini kullanarak, aynı kaynak grubunda yer aldığı sürece özel görüntünün yalnızca adını sağlamanız yeterli olur. 

Bu örnekte *myResourceGroup* içindeki *myImage* görüntüsünden *myVMfromImage* adlı VM'yi oluşturursunuz.


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

Burada, yaygın görüntü yönetimi görevlerini ve PowerShell kullanarak bunların nasıl tamamlanacağını gösteren bazı örnekler verilmiştir.

Tüm görüntüleri ada göre listeleyin.

```azurepowershell-interactive
$images = Get-AzureRMResource -ResourceType Microsoft.Compute/images 
$images.name
```

Görüntüyü silin. Bu örnek adlı görüntüyü siler *Myımage* gelen *myResourceGroup*.

```azurepowershell-interactive
Remove-AzureRmImage `
    -ImageName myImage `
    -ResourceGroupName myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, özel bir VM görüntüsü oluşturdunuz. Şunları öğrendiniz:

> [!div class="checklist"]
> * Sysprep ve VM’leri genelleştirme
> * Özel görüntü oluşturma
> * Özel görüntüden VM oluşturma
> * Aboneliğinizdeki tüm görüntüleri listeleme
> * Görüntü silme

Yüksek oranda kullanılabilir sanal makineler hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Yüksek oranda kullanılabilir VM’ler oluşturma](tutorial-availability-sets.md)



