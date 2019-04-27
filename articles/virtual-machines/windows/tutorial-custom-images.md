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
ms.date: 11/30/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 74087a6d1ce00293c968837e72c636847081e39e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60785003"
---
# <a name="tutorial-create-a-custom-image-of-an-azure-vm-with-azure-powershell"></a>Öğretici: Azure PowerShell ile Azure VM'deki özel görüntüsünü oluşturma

Özel görüntüler market görüntüleri gibidir, ancak bunları kendiniz oluşturursunuz. Özel görüntüler, dağıtımları bootstrap ve birden çok VM arasında tutarlılık sağlamak için kullanılabilir. Bu öğreticide, kendi özel görüntünüzü PowerShell kullanarak bir Azure sanal makine oluşturun. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Sysprep ve VM’leri genelleştirme
> * Özel görüntü oluşturma
> * Özel görüntüden VM oluşturma
> * Aboneliğinizdeki tüm görüntüleri listeleme
> * Görüntü silme

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdaki adımlar, mevcut bir VM’yi alıp, yeni VM örnekleri oluşturmak için kullanabileceğiniz yeniden kullanılabilir bir özel görüntüye dönüştürmeyi ayrıntılı olarak açıklar.

Bu öğreticideki örneği tamamlamak için, mevcut bir sanal makinenizin olması gerekir. Gerekirse, bu [betik örneği](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) sizin için bir tane oluşturabilir. Bu öğreticide çalışırken, gerektiğinde kaynak grubu ve VM adlarını değiştirin.

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. 

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. İsterseniz [https://shell.azure.com/powershell](https://shell.azure.com/powershell) adresine giderek Cloud Shell'i ayrı bir tarayıcı sekmesinde de başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

## <a name="prepare-vm"></a>VM'yi hazırlama

Bir sanal makine görüntüsü oluşturmak için kaynak VM, Genelleştirme, serbest bırakılıyor ve Azure ile genelleştirilmiş olarak işaretleme hazırlamanız gerekir.

### <a name="generalize-the-windows-vm-using-sysprep"></a>Sysprep kullanarak Windows VM'sini genelleştirme

Sysprep diğer öğelerin yanı sıra tüm kişisel hesap bilgilerinizi kaldırır ve makineyi bir görüntü olarak kullanılacak şekilde hazırlar. Sysprep hakkında daha fazla ayrıntı için bkz: [Sysprep işlemini kullanma: Giriş](https://technet.microsoft.com/library/bb457073.aspx).


1. Sanal makineye bağlanın.
2. Yönetici olarak Komut İstemi penceresini açın. Dizinine *%windir%\system32\sysprep*ve ardından çalıştırın `sysprep.exe`.
3. **Sistem Hazırlama Aracı** iletişim kutusunda  **Sistem İlk Çalıştırma Deneyimi (OOBE) Moduna Gir**'i seçin ve **Genelleştir** onay kutusunun seçili olduğundan emin olun.
4. **Kapatma Seçenekleri**'nde **Kapat**'ı seçin ve **Tamam**'a tıklayın.
5. Sysprep tamamlandığında, sanal makineyi kapatır. **VM'yi yeniden başlatmayın**.

### <a name="deallocate-and-mark-the-vm-as-generalized"></a>VM’yi serbest bırakma ve genelleştirilmiş olarak işaretleme

Görüntü oluşturmak için, VM'nin serbest bırakılması ve Azure'da genelleştirilmiş olarak işaretlenmesi gerekir.

Kullanarak VM'yi [Stop-AzVM](https://docs.microsoft.com/powershell/module/az.compute/stop-azvm).

```azurepowershell-interactive
Stop-AzVM `
   -ResourceGroupName myResourceGroup `
   -Name myVM -Force
```

Sanal makinenin durumunu `-Generalized` kullanarak [Set-AzVm](https://docs.microsoft.com/powershell/module/az.compute/set-azvm). 
   
```azurepowershell-interactive
Set-AzVM `
   -ResourceGroupName myResourceGroup `
   -Name myVM -Generalized
```


## <a name="create-the-image"></a>Görüntü oluşturma

VM görüntüsünü kullanarak oluşturabileceğiniz artık [yeni AzImageConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azimageconfig) ve [yeni AzImage](https://docs.microsoft.com/powershell/module/az.compute/new-azimage). Aşağıdaki örnek, *myVM* adlı bir VM’den *myImage* adlı bir görüntü oluşturur.

Sanal makineyi alın. 

```azurepowershell-interactive
$vm = Get-AzVM `
   -Name myVM `
   -ResourceGroupName myResourceGroup
```

Görüntü yapılandırması oluşturun.

```azurepowershell-interactive
$image = New-AzImageConfig `
   -Location EastUS `
   -SourceVirtualMachineId $vm.ID 
```

Görüntü oluşturun.

```azurepowershell-interactive
New-AzImage `
   -Image $image `
   -ImageName myImage `
   -ResourceGroupName myResourceGroup
``` 

 
## <a name="create-vms-from-the-image"></a>Görüntüden VM oluşturma

Artık bir görüntünüz olduğuna göre, görüntüden bir veya daha fazla yeni VM oluşturabilirsiniz. Özel görüntüden VM oluşturma işlemi, Market görüntüsü kullanarak VM oluşturmaya benzer. Market görüntüsünü kullandığınızda, görüntü, görüntü sağlayıcısı, teklif, SKU ve sürüm hakkındaki bilgileri sağlamanız gerekir. Kullanarak Basitleştirilmiş parametre kümesi için [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) cmdlet'i, yalnızca gereken aynı kaynak grubunda olduğu sürece, özel görüntü adını sağlayın. 

Bu örnek adlı bir VM oluşturur *Myımage* gelen *Myımage* içinde görüntü *myResourceGroup*.


```azurepowershell-interactive
New-AzVm `
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
$images = Get-AzResource -ResourceType Microsoft.Compute/images 
$images.name
```

Görüntüyü silin. Bu örnek adlı görüntüyü siler *Myımage* gelen *myResourceGroup*.

```azurepowershell-interactive
Remove-AzImage `
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

Yüksek oranda kullanılabilir sanal makineleri oluşturma hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Yüksek oranda kullanılabilir VM’ler oluşturma](tutorial-availability-sets.md)



