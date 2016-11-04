---
title: PowerShell kullanarak bir VM oluşturma | Microsoft Docs
description: Kolayca Windows Server çalıştıran yeni bir VM oluşturmak için Azure PowerShell ve Azure Resource Manager kullanın.
services: virtual-machines-windows
documentationcenter: ''
author: davidmu1
manager: timlt
editor: ''
tags: azure-resource-manager

ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/27/2016
ms.author: davidmu

---
# Resource Manager ve PowerShell kullanarak Windows VM oluşturma
Bu makalede, [Resource Manager](../resource-group-overview.md) ve PowerShell kullanarak Windows Server çalıştıran bir Azure Virtual Machine’i ve ihtiyacı olan kaynakları nasıl hızlı bir şekilde oluşturacağınız gösterilir. 

Bu makaledeki tüm adımlar bir sanal makine oluşturmak için gereklidir ve bu adımların tamamlanması yaklaşık 30 dakika sürer.

## 1. adım: Azure PowerShell'i yükleme
Azure PowerShell’in en son sürümünü yükleme, aboneliğinizi seçme ve hesabınızda oturum açma hakkında bilgi almak için bkz. [Azure PowerShell’i yükleme ve yapılandırma](../powershell-install-configure.md).

## 2. adım: bir kaynak grubu oluşturma
Önce, bir kaynak grubu oluşturun.

1. Kaynakların oluşturulabileceği kullanılabilir konumların bir listesini alın.
   
        Get-AzureRmLocation | sort Location | Select Location
   
    Bu örnektekine benzer bir şey görmeniz gerekir:
   
        Location
        --------
        australiaeast
        australiasoutheast
        brazilsouth
        canadacentral
        canadaeast
        centralindia
        centralus
        eastasia
        eastus
        eastus2
        japaneast
        japanwest
        northcentralus
        northeurope
        southcentralus
        southeastasia
        southindia
        westeurope
        westindia
        westus
2. **$locName** değerini listeden bir konumla değiştirin. Değişkeni oluşturun.
   
        $locName = "centralus"
3. **$rgName** değerini yeni kaynak grubu adıyla değiştirin. Değişkeni ve kaynak grubunu oluşturun.
   
       $rgName = "mygroup1"
       New-AzureRmResourceGroup -Name $rgName -Location $locName

## 3. adım: Depolama hesabı oluşturma
[Depolama hesabı](../storage/storage-introduction.md) oluşturduğunuz sanal makine tarafından kullanılan sanal sabit diski depolamak için gereklidir.

1. **$stName** değerini depolama hesabı adıyla değiştirin. Adın benzersizliğini test edin.
   
        $stName = "mystorage1"
        Get-AzureRmStorageAccountNameAvailability $stName
   
    Bu komut **True** döndürürse, önerilen adınız Azure içinde benzersizdir. Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir.
2. Şimdi depolama hesabını oluşturmak üzere komutu çalıştırın.
   
        $storageAcc = New-AzureRmStorageAccount -ResourceGroupName $rgName -Name $stName -SkuName "Standard_LRS" -Kind "Storage" -Location $locName

## 4. adım: Sanal ağ oluşturma
Tüm sanal makineler bir [sanal ağın](../virtual-network/virtual-networks-overview.md) parçasıdır.

1. **$subnetName** değerini alt ağ adıyla değiştirin. Değişkeni ve alt ağı oluşturun.
   
        $subnetName = "mysubnet1"
        $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
2. **$vnetName** değerini sanal ağ adıyla değiştirin. Değişkeni ve alt ağ ile birlikte sanal ağı oluşturun.
   
        $vnetName = "myvnet1"
        $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
   
    Uygulamanız ve ortamınız için anlamlı değerleri kullanın.

## 5. adım: Genel IP adresi ve ağ arabirimi oluşturma
Sanal makinenin sanal ağda iletişimini etkinleştirmeniz için, [genel IP adresi](../virtual-network/virtual-network-ip-addresses-overview-arm.md) ve ağ arabirimi gereklidir.

1. **$ipName** değerini genel IP adresi adıyla değiştirin. Değişkeni ve genel IP adresini oluşturun.
   
        $ipName = "myIPaddress1"
        $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
2. **$nicName** değerini ağ arabirimi adıyla değiştirin. Değişkeni ve ağ arabirimini oluşturun.
   
        $nicName = "mynic1"
        $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id

## 6. adım: Sanal makine oluşturma
Tüm parçaları yerinde olduğuna göre, şimdi sanal makine oluşturma vakti.

1. Sanal makine için yönetici hesap adı ve parolasını ayarlamak için komutu çalıştırın.
   
        $cred = Get-Credential -Message "Type the name and password of the local administrator account."
   
    Parola 12-123 karakter uzunluğunda olmalıdır ve en az şunları içermelidir: bir küçük harf karakter, bir büyük harf karakter, bir sayı ve bir özel karakter. 
2. **$vmName** değerini sanal makine adıyla değiştirin. Değişkeni ve sanal makine yapılandırmasını oluşturun.
   
        $vmName = "myvm1"
        $vm = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A1"
   
    Sanal makine için kullanılabilir boyutların listesi için bkz. [Azure’da sanal makineler için boyutlar](virtual-machines-windows-sizes.md)
3. **$compName** değerini sanal makine bilgisayar adıyla değiştirin. Değişkeni oluşturun ve yapılandırmaya işletim sistemi bilgilerini ekleyin.
   
        $compName = "myvm1"
        $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $compName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
4. Sanal makine sağlamak için kullanılacak görüntüyü tanımlayın. 
   
        $vm = Set-AzureRmVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
   
    Kullanılacak görüntüleri seçme hakkında daha fazla bilgi için bkz. [PowerShell veya CLI ile Azure’da Windows sanal makine görüntülerine erişin ve seçin](virtual-machines-windows-cli-ps-findimage.md).
5. Oluşturduğunuz ağ arabirimini yapılandırmaya ekleyin.
   
        $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
6. **$blobPath** değerini sanal sabit diskteki depolamanın yol ve dosya adıyla değiştirin. Sanal sabit disk dosyası genellikle bir kapsayıcıda depolanır, örneğin, **vhds/WindowsVMosDisk.vhd**. Değişkenleri oluşturun.
   
        $blobPath = "vhds/WindowsVMosDisk.vhd"
        $osDiskUri = $storageAcc.PrimaryEndpoints.Blob.ToString() + $blobPath
7. **$diskName** değerini işletim sistemi diski adıyla değiştirin. Değişkeni oluşturun ve yapılandırmaya disk bilgilerini ekleyin.
   
        $diskName = "windowsvmosdisk"
        $vm = Set-AzureRmVMOSDisk -VM $vm -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage
8. Son olarak, sanal makineyi oluşturun.
   
        New-AzureRmVM -ResourceGroupName $rgName -Location $locName -VM $vm
   
    Kaynak grubunu ve Azure portaldaki tüm kaynaklarını ve PowerShell penceresinde başarı durumunu görmelisiniz:
   
        RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
        ---------  -------------------  ----------  ------------
                                  True          OK  OK

## Sonraki Adımlar
* Dağıtım ile ilgili sorunlar varsa, bir sonraki adım [Azure portalındaki kaynak grubu dağıtımı sorunlarını giderme](../resource-manager-troubleshoot-deployments-portal.md)’ye bakmak için olacaktır
* [Azure Resource Manager ve PowerShell kullanarak sanal makineleri yönetme](virtual-machines-windows-ps-manage.md) gözden geçirerek, oluşturduğunuz sanal makineyi yönetmeyi öğrenin.
* [Bir Resource Manager şablonu ile Windows sanal makine oluşturma](virtual-machines-windows-ps-template.md)’daki bilgileri kullanarak sanal makine oluşturmak için şablon kullanma avantajından yararlanın

<!--HONumber=Sep16_HO5-->


