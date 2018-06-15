---
title: Azure yığınında PowerShell kullanarak bir Windows sanal makine oluşturma | Microsoft Docs
description: Azure yığınında PowerShell ile Windows sanal makine oluşturun.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 7CA6C0AC-23B7-4007-BA32-7A950FD1F3B8
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 04/20/2018
ms.author: mabrigg
ms.custom: mvc
ms.openlocfilehash: 9f5752a969ff6a191ec60e175494316aea4abcaf
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32152128"
---
# <a name="quickstart-create-a-windows-server-virtual-machine-by-using-powershell-in-azure-stack"></a>Hızlı Başlangıç: Azure yığınında PowerShell kullanarak bir Windows Server sanal makine oluşturma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığın PowerShell kullanarak bir Windows Server 2016 sanal makine oluşturabilirsiniz. Bir sanal makine oluşturmak için bu makaledeki adımları izleyin. Bu makalede ayrıca adımlarını sağlar:

* Uzak bir istemci ile sanal makineye bağlanabilirsiniz.
* IIS web sunucusu yüklemek ve varsayılan giriş sayfasını görüntüleyin.
* Kaynakları temizlemek.

>[!NOTE]
 Bu makalede Azure yığın Geliştirme Seti veya bir Windows tabanlı bir dış istemci VPN üzerinden bağlandığı sırada açıklanan adımları çalıştırabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Azure yığın operatörünüze Azure yığın Market "Windows Server 2016" görüntü ekledi emin olun.

* Azure yığını, Azure PowerShell oluşturmak ve kaynakları yönetmek için belirli bir sürümünü gerektirir. PowerShell Azure yığını için yapılandırılmış yoksa adımlarını izleyin [yükleme](azure-stack-powershell-install.md) ve [yapılandırma](azure-stack-powershell-configure-user.md) PowerShell.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bir kaynak grubu hangi Azure yığına kaynakları dağıtılan yönetilen ve mantıksal bir kapsayıcısıdır. Geliştirme Seti veya Azure tümleşik yığını sistem, bir kaynak grubu oluşturmak için aşağıdaki kod bloğu çalıştırın. Bu belgedeki tüm değişkenler için değerler atanır, bu değerleri kullanın veya yeni değerler atayın.

```powershell
# Create variables to store the location and resource group names.
$location = "local"
$ResourceGroupName = "myResourceGroup"

New-AzureRmResourceGroup `
  -Name $ResourceGroupName `
  -Location $location
```

## <a name="create-storage-resources"></a>Depolama kaynaklarını oluşturun

Bir depolama hesabı ve Windows Server 2016 görüntüyü depolamak için bir depolama kapsayıcısı oluşturun.

```powershell
# Create variables to store the storage account name and the storage account SKU information
$StorageAccountName = "mystorageaccount"
$SkuName = "Standard_LRS"

# Create a new storage account
$StorageAccount = New-AzureRMStorageAccount `
  -Location $location `
  -ResourceGroupName $ResourceGroupName `
  -Type $SkuName `
  -Name $StorageAccountName

Set-AzureRmCurrentStorageAccount `
  -StorageAccountName $storageAccountName `
  -ResourceGroupName $resourceGroupName

# Create a storage container to store the virtual machine image
$containerName = 'osdisks'
$container = New-AzureStorageContainer `
  -Name $containerName `
  -Permission Blob
```

## <a name="create-networking-resources"></a>Ağ kaynakları oluşturma

Bir sanal ağ, alt ağ ve genel IP adresi oluşturun. Bu kaynaklar, sanal makine için ağ bağlantısı sağlamak için kullanılır.

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -Name MyVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -AllocationMethod Static `
  -IdleTimeoutInMinutes 4 `
  -Name "mypublicdns$(Get-Random)"
```

### <a name="create-a-network-security-group-and-a-network-security-group-rule"></a>Ağ güvenliği grubu ve ağ güvenliği grup kuralı oluşturma

Ağ güvenlik grubu, gelen ve giden kuralları kullanarak sanal makine güvenliğini sağlar. Gelen Uzak Masaüstü bağlantılarına izin verecek şekilde 3389 numaralı bağlantı noktası için bir gelen kuralı ve gelen web trafiği izin vermek için 80 numaralı bağlantı noktası için bir gelen kuralı oluşturma olanak sağlar.

```powershell
# Create an inbound network security group rule for port 3389
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

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNetworkSecurityGroupRuleWWW `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1001 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80 `
  -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRuleRDP,$nsgRuleWeb
```

### <a name="create-a-network-card-for-the-virtual-machine"></a>Sanal makine için bir ağ kartı oluşturma

Ağ kartı, sanal makineyi bir alt ağa, ağ güvenliği grubuna ve genel IP adresine bağlar.

```powershell
# Create a virtual network card and associate it with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
  -Name myNic `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id `
  -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Sanal makine yapılandırması oluşturun. Bu yapılandırma, sanal makine dağıtımı sırasında kullanılan ayarları içerir. Örneğin: kimlik bilgileri, boyut ve sanal makine görüntüsü.

```powershell
# Define a credential object to store the username and password for the virtual machine
$UserName='demouser'
$Password='Password@123'| ConvertTo-SecureString -Force -AsPlainText
$Credential=New-Object PSCredential($UserName,$Password)

# Create the virtual machine configuration object
$VmName = "VirtualMachinelatest"
$VmSize = "Standard_A1"
$VirtualMachine = New-AzureRmVMConfig `
  -VMName $VmName `
  -VMSize $VmSize

$VirtualMachine = Set-AzureRmVMOperatingSystem `
  -VM $VirtualMachine `
  -Windows `
  -ComputerName "MainComputer" `
  -Credential $Credential

$VirtualMachine = Set-AzureRmVMSourceImage `
  -VM $VirtualMachine `
  -PublisherName "MicrosoftWindowsServer" `
  -Offer "WindowsServer" `
  -Skus "2016-Datacenter" `
  -Version "latest"

$osDiskName = "OsDisk"
$osDiskUri = '{0}vhds/{1}-{2}.vhd' -f `
  $StorageAccount.PrimaryEndpoints.Blob.ToString(),`
  $vmName.ToLower(), `
  $osDiskName

# Sets the operating system disk properties on a virtual machine.
$VirtualMachine = Set-AzureRmVMOSDisk `
  -VM $VirtualMachine `
  -Name $osDiskName `
  -VhdUri $OsDiskUri `
  -CreateOption FromImage | `
  Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create the virtual machine.
New-AzureRmVM `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -VM $VirtualMachine
```

## <a name="connect-to-the-virtual-machine"></a>Sanal makineye bağlanma

Uzak önceki adımda oluşturduğunuz sanal makinede, ortak IP adresi gerekir. Sanal makinenin ortak IP adresini almak için aşağıdaki komutu çalıştırın:

```powershell
Get-AzureRmPublicIpAddress `
  -ResourceGroupName $ResourceGroupName | Select IpAddress
```

Sanal makineyle bir Uzak Masaüstü oturumu oluşturmak için aşağıdaki komutu kullanın. IP adresini, sanal makinenizin publicIPAddress değeriyle değiştirin. İstendiğinde, kullanıcı adı ve sanal makine oluştururken kullandığınız parolayı girin.

```powershell
mstsc /v <publicIpAddress>
```

## <a name="install-iis-via-powershell"></a>PowerShell kullanarak IIS yükleme

Azure VM’de oturum açtıktan sonra tek bir PowerShell satırı kullanarak IIS yükleyebilir ve web trafiğine izin vermek üzere yerel güvenlik duvarı kuralını etkinleştirebilirsiniz. Bir PowerShell istemi açın ve şu komutu çalıştırın:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a>IIS karşılama sayfasını görüntüleme

IIS yüklü olan ve bağlantı noktası 80 üzerinde VM açık ile varsayılan IIS Karşılama sayfasını görüntülemek için tercih ettiğiniz bir web tarayıcısı kullanabilirsiniz. Kullanım *Publicıpaddress* varsayılan sayfasını ziyaret etmek için önceki bölümde belgelenen.

![Varsayılan IIS sitesi](./media/azure-stack-quick-create-vm-windows-powershell/default-iis-website.png)

## <a name="delete-the-virtual-machine"></a>Sanal makineyi silin

Artık gerekli olduğunda, sanal makineyi ve ilgili kaynaklarını içeren kaynak grubunu kaldırmak için aşağıdaki komutu kullanın:

```powershell
Remove-AzureRmResourceGroup `
  -Name $ResourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç basit Windows sanal makine dağıttıktan sonra. Azure yığın sanal makineler hakkında daha fazla bilgi için devam [Azure yığınında sanal makineler için ilgili önemli noktalar](azure-stack-vm-considerations.md).
