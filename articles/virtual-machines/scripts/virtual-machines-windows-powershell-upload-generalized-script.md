---
title: "Azure PowerShell komut dosyası örneği için genelleştirilmiş bir VHD yüklemek | Microsoft Docs"
description: "Genelleştirilmiş bir VHD Azure'a yükleyin ve yönetilen diskleri ve resource manager dağıtım modelini kullanarak yeni bir VM oluşturmak için PowerShell örnek komut dosyası."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/02/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 9534ce2a32ac57a441535cfa26f2981b804182d1
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="sample-script-to-upload-a-vhd-to-azure-and-create-a-new-vm"></a>Bir VHD Azure'a yükleyin ve yeni bir VM oluşturmak için örnek komut dosyası

Bu komut dosyası genelleştirilmiş bir sanal makineden bir yerel .vhd dosyası alır ve Azure'a yükler, yönetilen Disk görüntüsü oluşturduğu ve kullandığı yeni bir VM oluşturmak için.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

```powershell
# Provide values for the variables
$resourceGroup = 'myResourceGroup'
$location = 'EastUS'
$storageaccount = 'mystorageaccount'
$storageType = 'Standard_LRS'
$containername = 'mycontainer'
$localPath = 'C:\Users\Public\Documents\Hyper-V\VHDs\generalized.vhd'
$vmName = 'myVM'
$imageName = 'myImage'
$vhdName = 'myUploadedVhd.vhd'
$diskSizeGB = '128'
$subnetName = 'mySubnet'
$vnetName = 'myVnet'
$ipName = 'myPip'
$nicName = 'myNic'
$nsgName = 'myNsg'
$ruleName = 'myRdpRule'
$vmName = 'myVM'
$computerName = 'myComputerName'
$vmSize = 'Standard_DS1_v2'

# Get the username and password to be used for the administrators account on the VM. 
# This is used when connecting to the VM using RDP.

$cred = Get-Credential

# Upload the VHD
New-AzureRmResourceGroup -Name $resourceGroup -Location $location
New-AzureRmStorageAccount -ResourceGroupName $resourceGroup -Name $storageAccount -Location $location `
    -SkuName $storageType -Kind "Storage"
$urlOfUploadedImageVhd = ('https://' + $storageaccount + '.blob.core.windows.net/' + $containername + '/' + $vhdName)
Add-AzureRmVhd -ResourceGroupName $resourceGroup -Destination $urlOfUploadedImageVhd `
    -LocalFilePath $localPath

# Note: Uploading the VHD may take awhile!

# Create a managed image from the uploaded VHD 
$imageConfig = New-AzureRmImageConfig -Location $location
$imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized `
    -BlobUri $urlOfUploadedImageVhd
$image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $resourceGroup -Image $imageConfig
 
# Create the networking resources
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroup -Location $location `
    -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
$pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $resourceGroup -Location $location `
    -AllocationMethod Dynamic
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $resourceGroup -Location $location `
    -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name $ruleName -Description 'Allow RDP' -Access Allow `
    -Protocol Tcp -Direction Inbound -Priority 110 -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Name $vnetName

# Start building the VM configuration
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize

# Set the VM image as source image for the new VM
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id

# Finish the VM configuration and add the NIC.
$vm = Set-AzureRmVMOSDisk -VM $vm  -DiskSizeInGB $diskSizeGB -CreateOption FromImage -Caching ReadWrite
$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id

# Create the VM
New-AzureRmVM -VM $vm -ResourceGroupName $resourceGroup -Location $location

# Verify that the VM was created
$vmList = Get-AzureRmVM -ResourceGroupName $resourceGroup
$vmList.Name


```


<!-- 
[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-iis/create-windows-vm-iis.ps1 "Create VM IIS")] -->

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyası dağıtımı oluşturmak için aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her öğe.

| Komut                                                                                                             | Notlar                                                                                                                                                                                |
|---------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Yeni-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)                           | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur.                                                                                                                          |
| [Yeni-AzureRmStorageAccount](/powershell/module/azurerm.resources/new-azurermstorageaccount)                         | Bir depolama hesabı oluşturur.                                                                                                                                                           |
| [Ekleme AzureRmVhd](/powershell/module/azurerm.resources/add-azurermvhd)                                               | Bir sanal sabit diskten bir şirket içi sanal makine için Azure içinde bulut depolama hesabındaki blob karşıya yükleme.                                                                       |
| [AzureRmImageConfig yeni](/powershell/module/azurerm.resources/new-azurermimageconfig)                               | Yapılandırılabilir Resim nesnesi oluşturur.                                                                                                                                                 |
| [Set-AzureRmImageOsDisk](/powershell/module/azurerm.resources/set-azurermimageosdisk)                               | İşletim sistemi görüntüsü nesne üzerinde disk özelliklerini ayarlar.                                                                                                                        |
| [AzureRmImage yeni](/powershell/module/azurerm.resources/new-azurermimage)                                           | Yeni bir görüntü oluşturur.                                                                                                                                                                 |
| [AzureRmVirtualNetworkSubnetConfig yeni](/powershell/module/azurerm.resources/new-azurermvirtualnetworksubnetconfig) | Bir alt ağ yapılandırması oluşturur. Bu yapılandırma sanal ağ oluşturma işlemine kullanılır.                                                                                |
| [Yeni-AzureRmVirtualNetwork](/powershell/module/azurerm.resources/new-azurermvirtualnetwork)                         | Sanal ağ oluşturur.                                                                                                                                                           |
| [AzureRmPublicIpAddress yeni](/powershell/module/azurerm.resources/new-azurermpublicipaddress)                       | Bir ortak IP adresi oluşturur.                                                                                                                                                         |
| [AzureRmNetworkInterface yeni](/powershell/module/azurerm.resources/new-azurermnetworkinterface)                     | Bir ağ arabirimi oluşturur.                                                                                                                                                         |
| [AzureRmNetworkSecurityRuleConfig yeni](/powershell/module/azurerm.resources/new-azurermnetworksecurityruleconfig)   | Bir ağ güvenlik grubu kural yapılandırması oluşturur. Bu yapılandırma, NSG oluşturulduğunda bir NSG kuralı oluşturmak için kullanılır.                                                       |
| [AzureRmNetworkSecurityGroup yeni](/powershell/module/azurerm.resources/new-azurermnetworksecuritygroup)             | Bir ağ güvenlik grubu oluşturur.                                                                                                                                                    |
| [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.resources/get-azurermvirtualnetwork)                         | Bir sanal ağ, bir kaynak grubunda alır.                                                                                                                                          |
| [AzureRmVMConfig yeni](/powershell/module/azurerm.resources/new-azurermvmconfig)                                     | Bir VM yapılandırması oluşturur. Bu yapılandırma VM adı, işletim sistemi ve yönetici kimlik bilgileri gibi bilgileri içerir. Yapılandırma VM oluşturma sırasında kullanılır. |
| [Set-AzureRmVMSourceImage](/powershell/module/azurerm.resources/set-azurermvmsourceimage)                           | Bir sanal makine için bir görüntü belirtir.                                                                                                                                            |
| [Set-AzureRmVMOSDisk](/powershell/module/azurerm.resources/set-azurermvmosdisk)                                     | İşletim sistemi, bir sanal makineye disk özelliklerini ayarlar.                                                                                                                      |
| [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.resources/set-azurermvmoperatingsystem)                   | İşletim sistemi, bir sanal makineye disk özelliklerini ayarlar.                                                                                                                      |
| [Ekleme AzureRmVMNetworkInterface](/powershell/module/azurerm.resources/add-azurermvmnetworkinterface)                 | Bir ağ arabirimi bir sanal makineye ekler.                                                                                                                                       |
| [Yeni-AzureRmVM](/powershell/module/azurerm.resources/new-azurermvm)                                                 | Bir sanal makine oluşturun.                                                                                                                                                            |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup)                     | Bir kaynak grubu ve içerdiği tüm kaynaklar kaldırır.                                                                                                                         |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Ek sanal makine PowerShell komut dosyası örnekleri bulunabilir [Azure Windows VM belgelerine](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
