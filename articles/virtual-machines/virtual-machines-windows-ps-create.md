---
title: "PowerShell kullanarak bir VM oluşturma | Microsoft Belgeleri"
description: "Kolayca Windows Server çalıştıran yeni bir VM oluşturmak için Azure PowerShell ve Azure Resource Manager kullanın."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 14fe9ca9-e228-4d3b-a5d8-3101e9478f6e
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/21/2016
ms.author: davidmu
translationtype: Human Translation
ms.sourcegitcommit: edeee13457c1098eb1b44efaa97e9a84d29e88e7
ms.openlocfilehash: 12903dc79ac6349da9f4897cdb0db5cb62f67b22


---
# <a name="create-a-windows-vm-using-resource-manager-and-powershell"></a>Resource Manager ve PowerShell kullanarak Windows VM oluşturma
Bu makalede, [Resource Manager](../azure-resource-manager/resource-group-overview.md) ve PowerShell kullanarak Windows Server çalıştıran bir Azure Virtual Machine’i ve ihtiyacı olan kaynakları nasıl hızlı bir şekilde oluşturacağınız gösterilir. 

Bu makaledeki tüm adımlar bir sanal makine oluşturmak için gereklidir ve bu adımların tamamlanması yaklaşık 30 dakika sürer. Komutlardaki örnek parametre değerlerini, ortamınız için anlamlı olan adlarla değiştirin.

## <a name="step-1-install-azure-powershell"></a>1. adım: Azure PowerShell'i yükleme
Azure PowerShell’in en son sürümünü yükleme, aboneliğinizi seçme ve hesabınızda oturum açma hakkında bilgi almak için bkz. [Azure PowerShell’i yükleme ve yapılandırma](../powershell-install-configure.md).

## <a name="step-2-create-a-resource-group"></a>2. adım: bir kaynak grubu oluşturma
Kaynak grubu tüm kaynakları içermelidir, bu nedenle önce bu grubu oluşturalım.  

1. Kaynakların oluşturulabileceği kullanılabilir konumların bir listesini alın.
   
    ```powershell
    Get-AzureRmLocation | sort Location | Select Location
    ```
2. Kaynakların konumunu belirleyin. Bu komut, konumu **centralus** olarak belirler.
   
    ```powershell
    $location = "centralus"
    ```
3. Bir kaynak grubu oluşturun. Bu komut, belirlediğiniz konumda **myResourceGroup** adlı kaynak grubunu oluşturur.
   
    ```powershell
    $myResourceGroup = "myResourceGroup"
    New-AzureRmResourceGroup -Name $myResourceGroup -Location $location
    ```

## <a name="step-3-create-a-storage-account"></a>3. adım: Depolama hesabı oluşturma
[Depolama hesabı](../storage/storage-introduction.md) oluşturduğunuz sanal makine tarafından kullanılan sanal sabit diski depolamak için gereklidir. Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir.

1. Depolama hesap adının benzersiz olup olmadığını test edin. Bu komut, **myStorageAccount** adını test eder.
   
    ```powershell
    $myStorageAccountName = "mystorageaccount"
    Get-AzureRmStorageAccountNameAvailability $myStorageAccountName
    ```
   
    Bu komut **True** döndürürse, önerilen adınız Azure içinde benzersizdir. 
2. Şimdi depolama hesabını oluşturun.
   
    ```powershell    
    $myStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $myResourceGroup `
        -Name $myStorageAccountName -SkuName "Standard_LRS" -Kind "Storage" -Location $location
    ```

## <a name="step-4-create-a-virtual-network"></a>4. adım: Sanal ağ oluşturma
Tüm sanal makineler bir [sanal ağın](../virtual-network/virtual-networks-overview.md) parçasıdır.

1. Sanal ağ için bir alt ağ oluşturun. Bu komut, 10.0.0.0/24 adres ön ekini içerecek şekilde **mySubnet** adlı bir alt ağ oluşturur.
   
    ```powershell
    $mySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -AddressPrefix 10.0.0.0/24
    ```
2. Şimdi sanal ağı oluşturalım. Bu komut, oluşturduğunuz alt ağı ve **10.0.0.0/16** adres ön ekini kullanan, **myVnet** adlı bir sanal ağ oluşturur.
   
    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName $myResourceGroup `
        -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $mySubnet
    ```

## <a name="step-5-create-a-public-ip-address-and-network-interface"></a>5. adım: Genel IP adresi ve ağ arabirimi oluşturma
Sanal makinenin sanal ağda iletişimini etkinleştirmeniz için, [genel IP adresi](../virtual-network/virtual-network-ip-addresses-overview-arm.md) ve ağ arabirimi gereklidir.

1. Genel IP adresini oluşturun. Bu komut, **Dinamik** ayırma yöntemiyle **myPublicIp** adlı bir genel IP adresi oluşturur.
   
    ```powershell
    $myPublicIp = New-AzureRmPublicIpAddress -Name "myPublicIp" -ResourceGroupName $myResourceGroup `
        -Location $location -AllocationMethod Dynamic
    ```
2. Ağ arabirimini oluşturun. Bu komut, **myNIC** adlı bir ağ arabirimi oluşturur.
   
    ```powershell
    $myNIC = New-AzureRmNetworkInterface -Name "myNIC" -ResourceGroupName $myResourceGroup `
        -Location $location -SubnetId $myVnet.Subnets[0].Id -PublicIpAddressId $myPublicIp.Id
    ```

## <a name="step-6-create-a-virtual-machine"></a>6. adım: Sanal makine oluşturma
Tüm parçaları yerinde olduğuna göre, şimdi sanal makine oluşturma vakti.

1. Sanal makine için yönetici hesap adı ve parolasını ayarlamak için bu komutu çalıştırın.

    ```powershell
    $cred = Get-Credential -Message "Type the name and password of the local administrator account."
    ```
   
    Parola 12-123 karakter uzunluğunda olmalıdır ve en az şunları içermelidir: bir küçük harf karakter, bir büyük harf karakter, bir sayı ve bir özel karakter. 
2. Sanal makine için yapılandırma nesnesini oluşturun. Bu komut, VM’nin adını ve boyutunu belirleyen **myVmConfig** adlı bir yapılandırma nesnesi oluşturur.
   
    ```powershell
    $myVm = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS1_v2"
    ```
   
    Sanal makine için kullanılabilir boyutların listesi için bkz. [Azure’da sanal makineler için boyutlar](virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
3. VM için işletim sistemi ayarlarını yapılandırın. Bu komut; bilgisayar adını, işletim sistemi türünü ve VM için hesap kimlik bilgilerini ayarlar.
   
    ```powershell
    $myVM = Set-AzureRmVMOperatingSystem -VM $myVM -Windows -ComputerName "myVM" -Credential $cred `
        -ProvisionVMAgent -EnableAutoUpdate
    ```
4. VM’yi sağlamak için kullanılacak görüntüyü tanımlayın. Bu komut, VM için kullanılacak Windows Server görüntüsünü tanımlar. 
   
    ```powershell
    $myVM = Set-AzureRmVMSourceImage -VM $myVM -PublisherName "MicrosoftWindowsServer" `
        -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
    ```
   
    Kullanılacak görüntüleri seçme hakkında daha fazla bilgi için bkz. [PowerShell veya CLI ile Azure’da Windows sanal makine görüntülerine erişme ve bu görüntüleri seçme](virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
5. Oluşturduğunuz ağ arabirimini yapılandırmaya ekleyin.
   
    ```powershell
    $myVM = Add-AzureRmVMNetworkInterface -VM $myVM -Id $myNIC.Id
    ```
6. VM sabit diskinin konumunu ve adını tanımlayın. Sanal sabit disk dosyası, bir kapsayıcıda depolanır. Bu komut, oluşturduğunuz depolama hesabındaki **vhds/myOsDisk1.vhd** adlı kapsayıcıda diski oluşturur.
   
    ```powershell
    $blobPath = "vhds/myOsDisk1.vhd"
    $osDiskUri = $myStorageAccount.PrimaryEndpoints.Blob.ToString() + $blobPath
    ```
7. İşletim sistemi disk bilgilerini VM yapılandırmasına ekleyin. **$diskName** değerini işletim sistemi diski adıyla değiştirin. Değişkeni oluşturun ve yapılandırmaya disk bilgilerini ekleyin.
   
    ```powershell
    $myVM = Set-AzureRmVMOSDisk -VM $myVM -Name "myOsDisk1" -VhdUri $osDiskUri -CreateOption fromImage
    ```
8. Son olarak, sanal makineyi oluşturun.
   
    ```powershell
    New-AzureRmVM -ResourceGroupName $myResourceGroup -Location $location -VM $myVM
    ```

## <a name="next-steps"></a>Sonraki Adımlar
* Dağıtım ile ilgili sorunlar varsa, bir sonraki adım [Azure portalındaki kaynak grubu dağıtımı sorunlarını giderme](../resource-manager-troubleshoot-deployments-portal.md)’ye bakmak için olacaktır
* [Azure Resource Manager ve PowerShell kullanarak sanal makineleri yönetme](virtual-machines-windows-ps-manage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) gözden geçirerek, oluşturduğunuz sanal makineyi yönetmeyi öğrenin.
* [Bir Resource Manager şablonu ile Windows sanal makine oluşturma](virtual-machines-windows-ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)’daki bilgileri kullanarak sanal makine oluşturmak için şablon kullanma avantajından yararlanın




<!--HONumber=Nov16_HO5-->


