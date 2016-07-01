<properties
    pageTitle="PowerShell kullanarak bir VM oluşturma | Microsoft Azure"
    description="Kolayca Windows Server çalıştıran yeni bir VM oluşturmak için Azure PowerShell ve Azure Resource Manager kullanın."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/02/2016"
    ms.author="davidmu"/>

# Resource Manager ve PowerShell kullanarak Windows VM oluşturma

Bu makalede, [Resource Manager](../resource-group-overview.md) ve PowerShell kullanarak Windows Server çalıştıran bir Azure Virtual Machine’i ve ihtiyacı olan kaynakları nasıl hızlı bir şekilde oluşturacağınız gösterilir. 

Bu makaledeki tüm adımlar bir sanal makine oluşturmak için gereklidir ve bu adımların tamamlanması yaklaşık 30 dakika sürer.

## 1. adım: Azure PowerShell'i yükleme

Azure PowerShell’in en son sürümünü yükleme, kullanmak istediğiniz aboneliği seçime ve Azure hesabınızda oturum açma hakkında bilgi almak için bkz. [Azure PowerShell’i yükleme ve yapılandırma](../powershell-install-configure.md)
        
## 2. adım: bir kaynak grubu oluşturma

Önce, bir kaynak grubu oluşturun.

1. Kaynakların oluşturulabileceği kullanılabilir konumların bir listesini alın.

        Get-AzureLocation | sort Name | Select Name
        
    Şuna benzer bir şey görmeniz gerekir:
    
        Name
        ----
        Australia East
        Australia Southeast
        Brazil South
        Central India
        Central US
        East Asia
        East US
        East US 2
        Japan East
        Japan West
        North Central US
        North Europe
        South Central US
        South India
        Southeast Asia
        West Europe
        West India
        West US

2. **$locName** değerini listeden bir konumla değiştirin. Değişkeni oluşturun.

        $locName = "Central US"
        
3.  **$rgName** değerini yeni kaynak grubu adıyla değiştirin. Değişkeni ve kaynak grubunu oluşturun.

        $rgName = "mygroup1"
        New-AzureRmResourceGroup -Name $rgName -Location $locName
    
## 3. adım: Depolama hesabı oluşturma

[Depolama hesabı](../storage/storage-introduction.md) oluşturduğunuz sanal makine tarafından kullanılan sanal sabit diski depolamak için gereklidir.

1. **$stName** değerini depolama hesabı adıyla değiştirin. Adın benzersizliğini test edin.

        $stName = "mystorage1"
        Test-AzureName -Storage $stName

    Bu komut **False** döndürürse, önerilen adınız Azure içinde benzersizdir. Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir.
    
2. Şimdi depolama hesabını oluşturmak üzere komutu çalıştırın.
    
        $storageAcc = New-AzureRmStorageAccount -ResourceGroupName $rgName -Name $stName -Type "Standard_LRS" -Location $locName
        
## 4. adım: Sanal ağ oluşturma

Tüm sanal makineler bir [sanal ağın](../virtual-network/virtual-networks-overview.md) parçasıdır.

1. **$subnetName** değerini alt ağ adıyla değiştirin. Değişkeni ve alt ağı oluşturun.
        
        $subnetName = "mysubnet1"
        $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
        
2. **$vnetName** değerini sanal ağ adıyla değiştirin. Değişkeni ve alt ağ ile birlikte sanal ağı oluşturun.

        $vnetName = "myvnet1"
        $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
        
    Uygulamanız ve ortamınız için anlamlı değerleri kullanmanız gerekir.
        
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
        
    Parola 8-123 karakter uzunluğunda olmalıdır ve şunlardan en az 3’ünü içermelidir: bir küçük harf karakter, bir büyük harf karakter, bir sayı ve bir özel karakter. 
        
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
        
6. **$blobPath** değerini sanal sabit diskin kullanacağı depolamadaki yol ve dosya adıyla değiştirin. Sanal sabit disk dosyası genellikle bir kapsayıcıda depolanır, örneğin, **vhds/WindowsVMosDisk.vhd**. Değişkenleri oluşturun.

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

- Dağıtım ile ilgili sorunlar varsa, bir sonraki adım [Azure Portal’daki kaynak grubu dağıtımı sorunlarını giderme](../resource-manager-troubleshoot-deployments-portal.md)’ye bakmak için olacaktır
- [Azure Resource Manager ve PowerShell kullanarak sanal makineleri yönetme](virtual-machines-windows-ps-manage.md) gözden geçirerek henüz oluşturduğunuz sanal makineyi yönetmeyi öğrenin.
- [Bir Resource Manager şablonu ile Windows sanal makine oluşturma](virtual-machines-windows-ps-template.md)’daki bilgileri kullanarak sanal makine oluşturmak için şablon kullanma avantajından yararlanın



<!--HONumber=Jun16_HO2-->


