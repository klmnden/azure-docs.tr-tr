---
title: "Azure PowerShell ile özel VM görüntüleri oluşturma | Microsoft Docs"
description: "Öğretici - Azure PowerShell kullanarak özel bir VM görüntüsü oluşturun."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/07/2017
ms.author: cynthn
ms.custom: mvc
<<<<<<< HEAD
ms.openlocfilehash: cee283268057a407003a38f8db5af8cac151439f
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: HT
=======
ms.openlocfilehash: 7001e5df235d65c531b9102f879bde9693c4f853
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="create-a-custom-image-of-an-azure-vm-using-powershell"></a>Özel bir Azure PowerShell kullanarak bir VM görüntüsü oluşturma

Özel resimler gibi Market görüntülerini olsa da, bunları kendiniz oluşturmanız. Özel resimler, uygulamalar, uygulama yapılandırmaları ve diğer işletim sistemi yapılandırmalarını önceden gibi önyükleme yapılandırmaları için kullanılabilir. Bu öğreticide, kendi özel görüntünüzü bir Azure sanal makine oluşturun. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Sysprep ve VM'ler generalize
> * Özel görüntü oluşturma
> * Özel bir görüntüden bir VM oluşturma
> * Aboneliğinizdeki tüm görüntüleri listesi
> * Bir görüntü Sil

Bu öğretici, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps).

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdaki adımlar mevcut bir VM'yi almak ve yeni VM örnekleri oluşturmak için kullanabileceğiniz yeniden kullanılabilir bir özel görüntü açmak nasıl ayrıntılı olarak açıklanmaktadır.

Örneğin bu öğreticiyi tamamlamak için var olan bir sanal makine olması gerekir. Gerekirse, bu [komut dosyası örneği](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) sizin için bir tane oluşturabilirsiniz. Çalışma öğretici aracılığıyla değiştirdiğinizde VM ve kaynak grubu adları gerektiğinde.

## <a name="prepare-vm"></a>VM hazırlama

Bir sanal makine görüntüsünü oluşturmak için VM genelleme, serbest bırakma ve Azure'da genelleştirilmiş gibi VM kaynak işaretleme VM hazırlamanız gerekir.

### <a name="generalize-the-windows-vm-using-sysprep"></a>Sysprep kullanarak Windows VM generalize

Sysprep tüm kişisel hesap bilgilerinize, başka şeylerin kaldırır ve bir görüntü olarak kullanılacak makine hazırlar. Sysprep hakkında daha fazla ayrıntı için bkz: [kullanım Sysprep nasıl: Giriş](http://technet.microsoft.com/library/bb457073.aspx).


1. Sanal makineye bağlanın.
2. Bir yönetici olarak komut istemi penceresi açın. Dizinine değiştirin *%windir%\system32\sysprep*ve ardından çalıştırın *sysprep.exe*.
3. İçinde **Sistem Hazırlama aracı** iletişim kutusunda *girin sistem Out-of-Box deneyimi (OOBE)*, emin olun *Generalize* onay kutusu seçilidir.
4. İçinde **kapatma seçenekleri**seçin *kapatma* ve ardından **Tamam**.
5. Sysprep tamamlandığında, sanal makineyi kapatır. **VM yeniden başlatma**.

### <a name="deallocate-and-mark-the-vm-as-generalized"></a>Deallocate ve VM genelleştirilmiş olarak işaretle

Bir görüntü oluşturmak için VM serbest ve Azure'da genelleştirilmiş olarak işaretlenen gerekir.

Kullanarak VM serbest [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM -Force
```

Sanal makine durumunu ayarlamak `-Generalized` kullanarak [Set-AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm). 
   
```powershell
Set-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM -Generalized
```


## <a name="create-the-image"></a>Görüntü oluşturma

VM görüntüsü kullanarak oluşturabileceğiniz artık [yeni AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) ve [yeni AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage). Aşağıdaki örnek adlı bir resim oluşturur *myImage* adlı bir VM'den *myVM*.

Sanal makine Al. 

```powershell
$vm = Get-AzureRmVM -Name myVM -ResourceGroupName myResourceGroup
```

Görüntü yapılandırmasını oluşturun.

```powershell
$image = New-AzureRmImageConfig -Location EastUS -SourceVirtualMachineId $vm.ID 
```

Görüntü oluşturma.

```powershell
New-AzureRmImage -Image $image -ImageName myImage -ResourceGroupName myResourceGroup
``` 

 
## <a name="create-vms-from-the-image"></a>Sanal makineleri görüntüden oluşturma

Bir görüntü sahip olduğunuza göre bir veya daha fazla yeni VM'ler görüntüden oluşturabilirsiniz. Özel bir görüntüden bir VM oluşturma, Market görüntüsünü kullanan bir VM oluşturmak için çok benzer. Market görüntüsünü kullandığınızda, görüntü, görüntü sağlayıcısı, teklif, SKU ve sürümü hakkında bilgi sağlamak için gerekir. Özel bir görüntü ile yalnızca özel görüntü kaynak kimliği sağlamanız gerekir. 

Aşağıdaki komut dosyasında bir değişken oluşturuyoruz *$image* özel görüntü kullanma hakkında bilgi depolamak için [Get-AzureRmImage](/powershell/module/azurerm.compute/get-azurermimage) ve ardından kullanırız [kümesi AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) ve Kimliğini kullanarak belirtin *$image* değişken yeni oluşturduğumuz. 

Komut dosyası adlı bir VM oluşturur *myVMfromImage* bizim Özel görüntüden yeni bir kaynak grubu adında *myResourceGroupFromImage* içinde *Batı ABD* konumu.


```powershell
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

New-AzureRmResourceGroup -Name myResourceGroupFromImage -Location EastUS

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name MYvNET `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

$pip = New-AzureRmPublicIpAddress `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name "mypublicdns$(Get-Random)" `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4

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
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleRDP

$nic = New-AzureRmNetworkInterface `
    -Name myNic `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $pip.Id `
    -NetworkSecurityGroupId $nsg.Id

$vmConfig = New-AzureRmVMConfig `
    -VMName myVMfromImage `
    -VMSize Standard_D1 | Set-AzureRmVMOperatingSystem -Windows `
        -ComputerName myComputer `
        -Credential $cred 

# Here is where we create a variable to store information about the image 
$image = Get-AzureRmImage `
    -ImageName myImage `
    -ResourceGroupName myResourceGroup

# Here is where we specify that we want to create the VM from and image and provide the image ID
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -Id $image.Id

$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id

New-AzureRmVM `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -VM $vmConfig
```

## <a name="image-management"></a>Görüntü Yönetimi 

İşte bazı örnekler genel yönetim görüntü görevleri ve bunları tamamlamak nasıl PowerShell kullanarak.

Tüm görüntüleri ada göre listeler.

```powershell
$images = Find-AzureRMResource -ResourceType Microsoft.Compute/images 
$images.name
```

Görüntüyü silin. Bu örnek adlı görüntü siler *myOldImage* gelen *myResourceGroup*.

```powershell
Remove-AzureRmImage `
    -ImageName myOldImage `
    -ResourceGroupName myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, özel bir VM görüntüsü oluşturuldu. Şunları öğrendiniz:

> [!div class="checklist"]
> * Sysprep ve VM'ler generalize
> * Özel görüntü oluşturma
> * Özel bir görüntüden bir VM oluşturma
> * Aboneliğinizdeki tüm görüntüleri listesi
> * Bir görüntü Sil

Nasıl yüksek oranda kullanılabilir sanal makineler hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Yüksek oranda kullanılabilir VM’ler oluşturma](tutorial-availability-sets.md)



