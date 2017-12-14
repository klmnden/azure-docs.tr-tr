---
title: "Azure'da ölçeklenebilir bir uygulama için bir VM ve depolama hesabı oluşturma | Microsoft Docs"
description: "Azure blob storage kullanarak ölçeklenebilir bir uygulamayı çalıştırmak için kullanılacak bir VM dağıtmayı öğrenin"
services: storage
documentationcenter: 
author: georgewallace
manager: jeconnoc
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 12/12/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: 011812f5e32537321301dad0c654bca341b3606d
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="create-a-virtual-machine-and-storage-account-for-a-scalable-application"></a>Bir sanal makine ve ölçeklenebilir bir uygulama için depolama hesabı oluşturma

Bu öğretici bir dizi birini bir parçasıdır. Bu öğretici, yükler ve büyük miktarlarda bir Azure depolama hesabıyla rastgele veri indirme bir uygulama dağıttığınız gösterir. İşlemi tamamladığınızda, karşıya yükleme ve büyük miktarlarda verinin bir depolama hesabına indirme bir sanal makinede çalışan bir konsol uygulaması sahip.

Bölümünde bir dizi öğrenin nasıl yapılır:

> [!div class="checklist"]
> * Depolama hesabı oluşturma
> * Sanal makine oluşturma
> * Bir özel betik uzantısı yapılandırma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) ile yeni bir Azure kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma
 
Örnek bir Azure depolama hesabındaki bir blob kapsayıcısını 50 büyük dosyaları karşıya yükleme. Bir depolama hesabı depolamak ve Azure storage veri nesnelerinizi erişmek için benzersiz bir ad alanı sağlar. Bir depolama hesabı kullanılarak oluşturulan kaynak grubu oluşturun [New-AzureRmStorageAccount](/powershell/module/AzureRM.Storage/New-AzureRmStorageAccount) komutu.

Aşağıdaki komutta, kendi gördüğünüz Blob storage hesabı için genel olarak benzersiz bir ad yerine `<blob_storage_account>` yer tutucu.

```powershell-interactive
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName myResourceGroup `
  -Name "<blob_storage_account>" `
  -Location EastUS `
  -SkuName Standard_LRS `
  -Kind Storage `
  -EnableEncryptionService Blob
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Sanal makine yapılandırması oluşturun. Bu yapılandırma, sanal makineyi dağıtırken kullanılan sanal makine görüntüsü, boyutu ve kimlik doğrulama yapılandırması gibi ayarları içerir. Bu adımı çalıştırırken kimlik bilgileri istenir. Girdiğiniz değerler, sanal makinenin kullanıcı adı ve parolası olarak yapılandırılır.

[New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) ile sanal makineyi oluşturun.

```azurepowershell-interactive
# Variables for common values
$resourceGroup = "myResourceGroup"
$location = "eastus"
$vmName = "myVM"

# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Location $location `
  -Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName $resourceGroup -Location $location `
  -Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4

# Create a virtual network card and associate with public IP address
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName $resourceGroup -Location $location `
  -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS14_v2 | `
    Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
    Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer `
    -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create a virtual machine
New-AzureRmVM -ResourceGroupName $resourceGroup -Location $location -VM $vmConfig

Write-host "Your public IP address is $($pip.IpAddress)"
```

## <a name="deploy-configuration"></a>Yapılandırma dağıtma

Bu öğretici için sanal makinede yüklü olması gereken önkoşulları vardır. Özel betik uzantısı, aşağıdaki görevleri tamamlar bir PowerShell betiğini çalıştırmak için kullanılır:

> [!div class="checklist"]
> * .NET core 2.0 yükleyin
> * Yükleme chocolatey
> * GİT'i yükleyin
> * Örnek depoyu kopyalama
> * NuGet paketlerini geri yüklemek
> * Rastgele verilerle 50 1 GB dosyaları oluşturur

Sanal makinenin yapılandırmasını sonlandırmak için aşağıdaki cmdlet'i çalıştırın. Bu adımı tamamlamak için 5-15 dakika sürer.

```azurepowershell-interactive
# Start a CustomScript extension to use a simple PowerShell script to instal .NET core, dependancies, and pre-create the files to upload.
Set-AzureRMVMCustomScriptExtension -ResourceGroupName myResourceGroup `
    -VMName myVM `
    -Location EastUS `
    -FileUri https://raw.githubusercontent.com/azure-samples/storage-dotnet-perf-scale-app/master/setup_env.ps1 `
    -Run 'setup_env.ps1' `
    -Name DemoScriptExtension
```

## <a name="next-steps"></a>Sonraki adımlar

Bir depolama hesabı oluşturma, bir sanal makine dağıtımı ve gerekli önkoşulların nasıl gibi ile sanal makine yapılandırma hakkında öğrenilen bölümünde bir dizi:

> [!div class="checklist"]
> * Depolama hesabı oluşturma
> * Sanal makine oluşturma
> * Bir özel betik uzantısı yapılandırma

Büyük miktarlarda verinin üstel yeniden deneme ve paralellik kullanarak bir depolama hesabına yüklemek için seri iki kısmına ilerleyin.

> [!div class="nextstepaction"]
> [Büyük miktarda depolama hesabı paralel büyük dosyaları karşıya yükleme](storage-blob-scalable-app-upload-files.md)
