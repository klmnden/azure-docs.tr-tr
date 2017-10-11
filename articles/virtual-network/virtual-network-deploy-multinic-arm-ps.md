---
title: "Birden çok NIC - Azure PowerShell ile bir VM oluşturma | Microsoft Docs"
description: "PowerShell kullanarak birden çok NIC ile VM oluşturmayı öğrenin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 88880483-8f9e-4eeb-b783-64b8613407d9
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f3a11afd8fbd6a5e6b94cf1ebee7ea20665421bd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-multiple-nics-using-powershell"></a>PowerShell kullanarak birden çok NIC ile VM oluşturma

> [!div class="op_single_selector"]
> * [PowerShell](virtual-network-deploy-multinic-arm-ps.md)
> * [Azure CLI](virtual-network-deploy-multinic-arm-cli.md)
> * [Şablon](virtual-network-deploy-multinic-arm-template.md)
> * [PowerShell (Klasik)](virtual-network-deploy-multinic-classic-ps.md)
> * [Azure CLI (Klasik)](virtual-network-deploy-multinic-classic-cli.md)

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../resource-manager-deployment-model.md).  Bu makale, Microsoft’un çoğu yeni dağıtım için [klasik dağıtım modeli](virtual-network-deploy-multinic-classic-ps.md) yerine önerdiği Resource Manager dağıtım modelini açıklamaktadır.
>

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Adlı bir kaynak grubu aşağıdaki adımları kullanın *IaaSStory* WEB sunucuları ve bir kaynak grubu için adlı *IaaSStory arka uç* DB sunucuları için.

## <a name="prerequisites"></a>Ön koşullar
DB sunucuları oluşturabilmeniz için önce oluşturmanız gerekir *IaaSStory* bu senaryo için gereken tüm kaynakların kaynak grubuyla. Bu kaynakları oluşturmak için aşağıdaki adımları tamamlayın:

1. Gidin [şablon sayfasına](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Şablon sayfasındaki sağ tarafındaki **üst kaynak grubu**, tıklatın **Azure'a Dağıt**.
3. Gerekirse, parametre değerlerini değiştirin, sonra kaynak grubu dağıtmak için Azure Önizleme Portalı'ndaki adımları izleyin.

> [!IMPORTANT]
> Depolama hesabı adları benzersiz olduğundan emin olun. Azure üzerinde yinelenen depolama hesabı adları sahip olamaz.
> 

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-back-end-vms"></a>Arka uç VM'ler oluşturma
Arka uç VM'ler aşağıdaki kaynaklara oluşturulmasını bağlıdır:

* **Veri diskleri için depolama hesabı**. Daha iyi performans için veritabanı sunucularında veri diskler bir premium depolama hesabı gerektirir katı hal sürücüsü (SSD) teknolojisi kullanır. Premium depolama desteklemek için dağıtmanız Azure konumu emin olun.
* **NIC**. Her VM veritabanı erişimi için iki NIC gerekir ve yönetimi için bir tane.
* **Kullanılabilirlik kümesi**. Tüm veritabanı sunucuları, en az bir sanal makineleri çalışır durumda emin olmak için ayarlayın ve bakım sırasında çalıştıran tek bir kullanılabilirlik eklenir.  

### <a name="step-1---start-your-script"></a>1. adım - kodunuzu Başlat
Kullanılan tam PowerShell komut dosyası indirebilirsiniz [burada](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-ps.ps1). Ortamınızda çalışması için komut dosyasını değiştirmek için aşağıdaki adımları izleyin.

1. Yukarıdaki dağıtılmış varolan kaynak grubunuz temelinde değişkenleri aşağıdaki değerlerini değiştirmek [Önkoşullar](#Prerequisites).

    ```powershell
    $existingRGName        = "IaaSStory"
    $location              = "West US"
    $vnetName              = "WTestVNet"
    $backendSubnetName     = "BackEnd"
    $remoteAccessNSGName   = "NSG-RemoteAccess"
    $stdStorageAccountName = "wtestvnetstoragestd"
    ```

2. Arka uç dağıtımınız için kullanmak istediğiniz değerleri temel alarak aşağıdaki değişkenlerinin değerlerini değiştirin.

    ```powershell
    $backendRGName         = "IaaSStory-Backend"
    $prmStorageAccountName = "wtestvnetstorageprm"
    $avSetName             = "ASDB"
    $vmSize                = "Standard_DS3"
    $publisher             = "MicrosoftSQLServer"
    $offer                 = "SQL2014SP1-WS2012R2"
    $sku                   = "Standard"
    $version               = "latest"
    $vmNamePrefix          = "DB"
    $osDiskPrefix          = "osdiskdb"
    $dataDiskPrefix        = "datadisk"
    $diskSize               = "120"    
    $nicNamePrefix         = "NICDB"
    $ipAddressPrefix       = "192.168.2."
    $numberOfVMs           = 2
    ```
3. Dağıtımınız için gereken mevcut kaynakları alır.

    ```powershell
    $vnet                  = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $existingRGName
    $backendSubnet         = $vnet.Subnets|?{$_.Name -eq $backendSubnetName}
    $remoteAccessNSG       = Get-AzureRmNetworkSecurityGroup -Name $remoteAccessNSGName -ResourceGroupName $existingRGName
    $stdStorageAccount     = Get-AzureRmStorageAccount -Name $stdStorageAccountName -ResourceGroupName $existingRGName
    ```

### <a name="step-2---create-necessary-resources-for-your-vms"></a>2. adım - Vm'leriniz için gerekli kaynakları oluşturma
Yeni bir kaynak grubu, veri diskleri ve kullanılabilirlik tüm VM'ler için kümesi için bir depolama hesabı oluşturmak gerekir. Seçecek her VM için yerel yönetici hesabı kimlik bilgileri gerekir. Bu kaynakları oluşturmak için aşağıdaki adımları yürütün.

1. Yeni bir kaynak grubu oluşturun.

    ```powershell
    New-AzureRmResourceGroup -Name $backendRGName -Location $location
    ```
2. Yukarıda oluşturduğunuz kaynak grubunda yeni bir premium depolama hesabı oluşturun.

    ```powershell
    $prmStorageAccount = New-AzureRmStorageAccount -Name $prmStorageAccountName `
    -ResourceGroupName $backendRGName -Type Premium_LRS -Location $location
    ```
3. Yeni bir kullanılabilirlik kümesi oluşturun.

    ```powershell
    $avSet = New-AzureRmAvailabilitySet -Name $avSetName -ResourceGroupName $backendRGName -Location $location
    ```
4. Yerel yönetici her VM için kullanılacak hesap kimlik bilgilerini alın.

    ```powershell
    $cred = Get-Credential -Message "Type the name and password for the local administrator account."
    ```

### <a name="step-3---create-the-nics-and-back-end-vms"></a>Adım 3 - NIC ve arka uç VM'ler oluşturma
Ve gerekli NIC ve sanal makineleri döngüye içinde gibi birçok VM oluşturmak için bir döngü kullanmanız gerekir. NIC ve sanal makineleri oluşturmak için aşağıdaki adımları yürütün.

1. Başlangıç bir `for` değerine göre bir VM ve gerektiği pek çok, iki NIC oluşturmak için komutları yinelemek için döngü `$numberOfVMs` değişkeni.
   
    ```powershell
    for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){
    ```

2. Veritabanı erişimi için kullanılan NIC oluşturun.

    ```powershell
    $nic1Name = $nicNamePrefix + $suffixNumber + "-DA"
    $ipAddress1 = $ipAddressPrefix + ($suffixNumber + 3)
    $nic1 = New-AzureRmNetworkInterface -Name $nic1Name -ResourceGroupName $backendRGName `
    -Location $location -SubnetId $backendSubnet.Id -PrivateIpAddress $ipAddress1
    ```

3. Uzaktan erişim için kullanılan NIC oluşturun. Bu NIC için ilişkilendirilmiş bir NSG nasıl sahip dikkat edin.

    ```powershell
    $nic2Name = $nicNamePrefix + $suffixNumber + "-RA"
    $ipAddress2 = $ipAddressPrefix + (53 + $suffixNumber)
    $nic2 = New-AzureRmNetworkInterface -Name $nic2Name -ResourceGroupName $backendRGName `
    -Location $location -SubnetId $backendSubnet.Id -PrivateIpAddress $ipAddress2 `
    -NetworkSecurityGroupId $remoteAccessNSG.Id
    ```

4. Oluşturma `vmConfig` nesnesi.

    ```powershell
    $vmName = $vmNamePrefix + $suffixNumber
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avSet.Id
    ```

5. VM başına iki veri diski oluşturun. Veri diskleri daha önce oluşturduğunuz premium depolama hesabını olduğuna dikkat edin.

    ```powershell
    $dataDisk1Name = $vmName + "-" + $osDiskPrefix + "-1"
    $data1VhdUri = $prmStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $dataDisk1Name + ".vhd"
    Add-AzureRmVMDataDisk -VM $vmConfig -Name $dataDisk1Name -DiskSizeInGB $diskSize `
    -VhdUri $data1VhdUri -CreateOption empty -Lun 0

    $dataDisk2Name = $vmName + "-" + $dataDiskPrefix + "-2"
    $data2VhdUri = $prmStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $dataDisk2Name + ".vhd"
    Add-AzureRmVMDataDisk -VM $vmConfig -Name $dataDisk2Name -DiskSizeInGB $diskSize `
    -VhdUri $data2VhdUri -CreateOption empty -Lun 1
    ```

6. İşletim sistemi yapılandırması ve VM için kullanılacak resim.

    ```powershell
    $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName $publisher -Offer $offer -Skus $sku -Version $version
    ```

7. Yukarıda oluşturulan iki NIC ekleme `vmConfig` nesnesi.

    ```powershell
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic1.Id -Primary
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic2.Id
    ```

8. İşletim sistemi diski oluşturun ve VM oluşturun. Bildirim `}` bitiş `for` döngü.

    ```powershell
    $osDiskName = $vmName + "-" + $osDiskSuffix
    $osVhdUri = $stdStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $osDiskName + ".vhd"
    $vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $osDiskName -VhdUri $osVhdUri -CreateOption fromImage
    New-AzureRmVM -VM $vmConfig -ResourceGroupName $backendRGName -Location $location
    }
    ```

### <a name="step-4---run-the-script"></a>Adım 4 - komut dosyasını çalıştır
İndirilen ve gereksinimlerinize göre komut değiştirilen göre çalışma kendisinin komut dosyası arka uç veritabanı VM'ler ile birden çok NIC oluşturmak için.

1. Komut dosyanızı kaydedin ve çalıştırın **PowerShell** komut istemi veya **PowerShell ISE**. İlk çıkış gibi görürsünüz:

        ResourceGroupName : IaaSStory-Backend
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
            Actions  NotActions
            =======  ==========
                *                  

        ResourceId        : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory-Backend

2. Birkaç dakika sonra kimlik bilgileri istemi ve tıklatın doldurmak **Tamam**. Çıktı tek bir VM'ye temsil eder. Bildirim tüm işlemini tamamlamak için 8 dakika sürdü.

        ResourceGroupName            :
        Id                           :
        Name                         : DB2
        Type                         :
        Location                     :
        Tags                         :
        TagsText                     : null
        AvailabilitySetReference     : Microsoft.Azure.Management.Compute.Models.AvailabilitySetReference
        AvailabilitySetReferenceText :  {
                                    "ReferenceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/Microsoft.Compute/availabilitySets/ASDB"
                                    }
        Extensions                   :
        ExtensionsText               : null
        HardwareProfile              : Microsoft.Azure.Management.Compute.Models.HardwareProfile
        HardwareProfileText          : {
                                        "VirtualMachineSize": "Standard_DS3"
                                       }
        InstanceView                 :
        InstanceViewText             : null
        NetworkProfile               :
        NetworkProfileText           : null
        OSProfile                    :
        OSProfileText                : null
        Plan                         :
        PlanText                     : null
        ProvisioningState            :
        StorageProfile               : Microsoft.Azure.Management.Compute.Models.StorageProfile
        StorageProfileText           : {
                                         "DataDisks": [
                                           {
                                             "Lun": 0,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-1",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-1.vhd"
                                             }
                                           }
                                         ],
                                         "ImageReference": null,
                                         "OSDisk": null
                                       }
        DataDiskNames                : {DB2-datadisk-1}
        NetworkInterfaceIDs          :
        RequestId                    :
        StatusCode                   : 0

        ResourceGroupName            :
        Id                           :
        Name                         : DB2
        Type                         :
        Location                     :
        Tags                         :
        TagsText                     : null
        AvailabilitySetReference     : Microsoft.Azure.Management.Compute.Models.AvailabilitySetReference
        AvailabilitySetReferenceText : {
                                         "ReferenceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/
                                       Microsoft.Compute/availabilitySets/ASDB"
                                       }
        Extensions                   :
        ExtensionsText               : null
        HardwareProfile              : Microsoft.Azure.Management.Compute.Models.HardwareProfile
        HardwareProfileText          : {
                                         "VirtualMachineSize": "Standard_DS3"
                                       }
        InstanceView                 :
        InstanceViewText             : null
        NetworkProfile               :
        NetworkProfileText           : null
        OSProfile                    :
        OSProfileText                : null
        Plan                         :
        PlanText                     : null
        ProvisioningState            :
        StorageProfile               : Microsoft.Azure.Management.Compute.Models.StorageProfile
        StorageProfileText           : {
                                         "DataDisks": [
                                           {
                                             "Lun": 0,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-1",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-1.vhd"
                                             }
                                           },
                                           {
                                             "Lun": 1,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-2",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-2.vhd"
                                             }
                                           }
                                         ],
                                         "ImageReference": null,
                                         "OSDisk": null
                                       }
        DataDiskNames                : {DB2-datadisk-1, DB2-datadisk-2}
        NetworkInterfaceIDs          :
        RequestId                    :
        StatusCode                   : 0
        EndTime             : [Date] [Time]
        Error               :
        Output              :
        StartTime           : [Date] [Time]
        Status              : Succeeded
        TrackingOperationId : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        RequestId           : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        StatusCode          : OK
