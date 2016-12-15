---
title: "PowerShell kullanarak Sanal Makine Ölçek Kümesi Oluşturma | Microsoft Belgeleri"
description: "PowerShell kullanarak Sanal Makine Ölçek Kümesi oluşturma"
services: virtual-machine-scale-sets
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7bb03323-8bcc-4ee4-9a3e-144ca6d644e2
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/18/2016
ms.author: davidmu
translationtype: Human Translation
ms.sourcegitcommit: 550db52c2b77ad651b4edad2922faf0f951df617
ms.openlocfilehash: 5abaa31828e624f77b6a9efb4496327977b483e4


---
# <a name="create-a-windows-virtual-machine-scale-set-using-azure-powershell"></a>Azure PowerShell kullanarak bir Windows sanal makine ölçek kümesi oluşturma
Bu adımlar bir Azure sanal makine ölçek kümesi oluşturmaya yönelik boşluk doldurma yaklaşımını izlemektedir. Ölçek kümeleri hakkında daha fazla bilgi almak için bkz. [Sanal Makine Ölçek Kümeleri’ne Genel Bakış](virtual-machine-scale-sets-overview.md).

Bu makaledeki adımların uygulanması yaklaşık 30 dakika sürer.

## <a name="step-1-install-azure-powershell"></a>1. adım: Azure PowerShell'i yükleme
Azure PowerShell’in en son sürümünü yükleme, aboneliğinizi seçme ve hesabınızda oturum açma hakkında bilgi almak için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs).

## <a name="step-2-create-resources"></a>2. Adım: Kaynak oluşturma
Yeni ölçek kümeniz için gereken kaynakları oluşturun.

### <a name="resource-group"></a>Kaynak grubu
Bir kaynak grubunda bir sanal makine ölçek kümesi yer almalıdır.

1. Kaynak yerleştirebileceğiniz kullanılabilir konumların bir listesini alın:
   
        Get-AzureLocation | Sort Name | Select Name
2. Sizin için en uygun konumu seçin, **$locName** değerini konum adıyla değiştirin ve ardından değişkeni oluşturun:
   
        $locName = "location name from the list, such as Central US"
3. **$rgName** değerini yeni kaynak grubu için kullanmak istediğiniz adla değiştirin ve ardından değişkeni oluşturun: 
   
        $rgName = "resource group name"
4. Kaynak grubunu oluşturun:
   
        New-AzureRmResourceGroup -Name $rgName -Location $locName
   
    Bu örnektekine benzer bir şey görmeniz gerekir:
   
        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/########-####-####-####-############/resourceGroups/myrg1

### <a name="storage-account"></a>Depolama hesabı
Ölçeklendirme için kullanılan işletim sistemi diskini ve tanılama verilerini depolamak için sanal makine tarafından bir depolama hesabı kullanılır. Mümkün olduğunda, bir ölçek kümesi içinde oluşturulan her sanal makine için bir depolama hesabına sahip olunması en iyi uygulamadır. Bu mümkün değilse, her depolama hesabı için en fazla 20 VM planlayın. Bu makaledeki örnekte üç sanal makine için oluşturulan üç depolama hesabı gösterilmektedir.

1. **$saName** değerini depolama hesabının adıyla değiştirin. Adın benzersizliğini test edin. 
   
        $saName = "storage account name"
        Get-AzureRmStorageAccountNameAvailability $saName
   
    Yanıt **True** ise önerdiğiniz ad benzersizdir.
2. **$saType** değerini depolama hesabının türüyle değiştirin ve ardından değişkeni oluşturun:  
   
        $saType = "storage account type"
   
    Olası değerler şunlardır: Standard_LRS, Standard_GRS, Standard_RAGRS veya Premium_LRS.
3. Hesabı oluşturun:
   
        New-AzureRmStorageAccount -Name $saName -ResourceGroupName $rgName –Type $saType -Location $locName
   
    Bu örnektekine benzer bir şey görmeniz gerekir:
   
        ResourceGroupName   : myrg1
        StorageAccountName  : myst1
        Id                  : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microsoft
                              .Storage/storageAccounts/myst1
        Location            : centralus
        AccountType         : StandardLRS
        CreationTime        : 3/15/2016 4:51:52 PM
        CustomDomain        :
        LastGeoFailoverTime :
        PrimaryEndpoints    : Microsoft.Azure.Management.Storage.Models.Endpoints
        PrimaryLocation     : centralus
        ProvisioningState   : Succeeded
        SecondaryEndpoints  :
        SecondaryLocation   :
        StatusOfPrimary     : Available
        StatusOfSecondary   :
        Tags                : {}
        Context             : Microsoft.WindowsAzure.Commands.Common.Storage.AzureStorageContext
4. Üç depolama hesabı oluşturmak için 1 ile 4 arasındaki adımları tekrarlayın: örneğin myst1, myst2 ve myst3.

### <a name="virtual-network"></a>Sanal ağ
Ölçek kümesindeki sanal makineler için bir sanal ağ gereklidir.

1. **$subnetName** değerini sanal ağdaki alt ağ için kullanmak istediğiniz adla değiştirin ve ardından değişkeni oluşturun: 
   
        $subnetName = "subnet name"
2. Alt ağ yapılandırmasını oluşturun:
   
        $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
   
    Adres ön eki sanal ağınızdakinden farklı olabilir.
3. **$netName** değerini sanal ağ için kullanmak istediğiniz adla değiştirin ve ardından değişkeni oluşturun: 
   
        $netName = "virtual network name"
4. Sanal ağı oluşturun:
   
        $vnet = New-AzureRmVirtualNetwork -Name $netName -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="configuration-of-the-scale-set"></a>Ölçek kümesinin yapılandırması
Ölçek kümesi yapılandırması için gereken tüm kaynaklara sahip olduğunuza göre artık ölçek kümesini oluşturabiliriz.  

1. **$ipName** değerini IP yapılandırması için kullanmak istediğiniz adla değiştirin ve ardından değişkeni oluşturun: 
   
        $ipName = "IP configuration name"
2. IP yapılandırmasını oluşturun:
   
        $ipConfig = New-AzureRmVmssIpConfig -Name $ipName -LoadBalancerBackendAddressPoolsId $null -SubnetId $vnet.Subnets[0].Id
3. **$vmssConfig** değerini ölçek kümesi yapılandırması için kullanmak istediğiniz adla değiştirin ve ardından değişkeni oluşturun:   
   
        $vmssConfig = "Scale set configuration name"
4. Ölçek kümesi yapılandırmasını oluşturun:
   
        $vmss = New-AzureRmVmssConfig -Location $locName -SkuCapacity 3 -SkuName "Standard_A0" -UpgradePolicyMode "manual"
   
    Bu örnekte üç sanal makine ile oluşturulan bir ölçek kümesi gösterilmektedir. Ölçek kümelerinin kapasitesi hakkında daha fazla bilgi için bkz. [Sanal Makine Ölçek Kümelerine Genel Bakış](virtual-machine-scale-sets-overview.md). Bu adım ayrıca kümedeki sanal makinelerin boyutunu ayarlamayı (SkuName olarak adlandırılır) içerir. İhtiyaçlarınıza uygun bir boyut bulmak için [Sanal makine boyutları](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) bölümüne bakın.
5. Ağ arabirimi yapılandırmasını ölçek kümesi yapılandırmasına ekleyin:
   
        Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmss -Name $vmssConfig -Primary $true -IPConfiguration $ipConfig
   
    Bu örnektekine benzer bir şey görmeniz gerekir:
   
        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     :
        OverProvision         :
        Id                    :
        Name                  :
        Type                  :
        Location              : Central US
        Tags                  :

#### <a name="operating-system-profile"></a>İşletim sistemi profili
1. **$computerName** değerini kullanmak istediğiniz bilgisayar adıyla değiştirin ve ardından değişkeni oluşturun: 
   
        $computerName = "computer name prefix"
2. **$adminName** değerini sanal makineler üzerindeki yönetici hesabının adıyla değiştirin ve ardından değişkeni oluşturun:
   
        $adminName = "administrator account name"
3. **$adminPassword** değerini hesap parolasıyla değiştirin ve ardından değişkeni oluşturun:
   
        $adminPassword = "password for administrator accounts"
4. İşletim sistemi profilini oluşturun:
   
        Set-AzureRmVmssOsProfile -VirtualMachineScaleSet $vmss -ComputerNamePrefix $computerName -AdminUsername $adminName -AdminPassword $adminPassword

#### <a name="storage-profile"></a>Depolama profili
1. **$storageProfile** değerini depolama profili için kullanmak istediğiniz adla değiştirin ve ardından değişkeni oluşturun:  
   
        $storageProfile = "storage profile name"
2. Kullanılacak görüntüyü tanımlayan değişkenleri oluşturun:  
   
        $imagePublisher = "MicrosoftWindowsServer"
        $imageOffer = "WindowsServer"
        $imageSku = "2012-R2-Datacenter"
   
    Kullanılacak diğer görüntüler hakkında bilgi almak için [Windows PowerShell ve Azure CLI ile Azure sanal makine görüntülerinde gezinme ve seçme](../virtual-machines/virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
3. **$vhdContainers** değerini, "https://mystorage.blob.core.windows.net/vhds" gibi sanal sabit disklerin depolandığı yolları içeren bir listeyle değiştirin ve ardından değişkeni oluşturun:
   
        $vhdContainers = @("https://myst1.blob.core.windows.net/vhds","https://myst2.blob.core.windows.net/vhds","https://myst3.blob.core.windows.net/vhds")
4. Depolama profilini oluşturun:
   
        Set-AzureRmVmssStorageProfile -VirtualMachineScaleSet $vmss -ImageReferencePublisher $imagePublisher -ImageReferenceOffer $imageOffer -ImageReferenceSku $imageSku -ImageReferenceVersion "latest" -Name $storageProfile -VhdContainer $vhdContainers -OsDiskCreateOption "FromImage" -OsDiskCaching "None"  

### <a name="virtual-machine-scale-set"></a>Sanal makine ölçek kümesi
Son olarak, ölçek kümesini oluşturabilirsiniz.

1. **$vmssName** değerini sanal makine ölçek kümesinin adıyla değiştirin ve ardından değişkeni oluşturun:
   
        $vmssName = "scale set name"
2. Ölçek kümesini oluşturun:
   
        New-AzureRmVmss -ResourceGroupName $rgName -Name $vmssName -VirtualMachineScaleSet $vmss
   
    Başarılı dağıtımı gösteren bu örnek gibi bir şey görmeniz gerekir:
   
        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     : Updating
        OverProvision         :
        Id                    : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microso
                                ft.Compute/virtualMachineScaleSets/myvmss1
        Name                  : myvmss1
        Type                  : Microsoft.Compute/virtualMachineScaleSets
        Location              : centralus
        Tags                  :

## <a name="step-3-explore-resources"></a>3. Adım: Kaynakları keşfetme
Oluşturduğunuz sanal makine ölçek kümesini keşfetmek için aşağıdaki kaynakları kullanın:

* Azure portalı - Portal kullanılarak sınırlı miktarda bilgiye ulaşılabilir.
* [Azure Kaynak Gezgini](https://resources.azure.com/) - Bu araç, ölçek kümenizin mevcut durumunu araştırmak için en iyi yöntemdir.
* Azure PowerShell - Bilgileri almak için şu komutu kullanın:
  
        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
  
        Or 
  
        Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

## <a name="next-steps"></a>Sonraki adımlar
* [Bir Sanal Makine Ölçek Kümesindeki sanal makineleri yönetme](virtual-machine-scale-sets-windows-manage.md) bölümündeki bilgileri kullanarak yeni oluşturduğunuz ölçek kümesini yönetin
* [Otomatik ölçeklendirme ve sanal makine ölçek kümeleri](virtual-machine-scale-sets-autoscale-overview.md) bölümündeki bilgileri kullanarak ölçek kümenizin otomatik ölçeklendirmesini ayarlamayı düşünün
* [Sanal Makine Ölçek kümeleri ile dikey otomatik ölçeklendirme](virtual-machine-scale-sets-vertical-scale-reprovision.md) bölümünü gözden geçirerek dikey ölçeklendirme hakkında daha fazla bilgi edinin




<!--HONumber=Dec16_HO1-->


