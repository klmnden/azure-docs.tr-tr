---
title: "Azure yığınında PowerShell kullanarak bir Windows sanal makine oluşturma | Microsoft Docs"
description: "Azure yığınında PowerShell ile Windows sanal makine oluşturun."
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 09/25/2017
ms.author: sngun
ms.custom: mvc
ms.openlocfilehash: 42e126ffefd75669d90ea3ff1d3939028f71159c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-windows-virtual-machine-by-using-powershell-in-azure-stack"></a>Azure yığınında PowerShell kullanarak bir Windows sanal makine oluşturma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

PowerShell kullanarak Azure yığınında bir Windows Server 2016 sanal makine oluşturmak için bu kılavuzu ayrıntıları. Bu makalede Azure yığın Geliştirme Setinden veya Windows tabanlı bir dış istemci VPN üzerinden bağlandığı sırada açıklanan adımları çalıştırabilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar 

* Azure yığın operatörünüze Azure yığın Market "Windows Server 2016" görüntü ekledi emin olun.  

* Azure yığını, Azure PowerShell oluşturmak ve kaynakları yönetmek için belirli bir sürümünü gerektirir. PowerShell Azure yığını için yapılandırılmış yoksa adımlarını izleyin [yükleme](azure-stack-powershell-install.md) ve [yapılandırma](azure-stack-powershell-configure-user.md) PowerShell.    

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bir kaynak grubu hangi Azure yığına kaynakları dağıtılan yönetilen ve mantıksal bir kapsayıcısıdır. Geliştirme Seti veya Azure tümleşik yığını sistem, bir kaynak grubu oluşturmak için aşağıdaki kod bloğu çalıştırın. Bu belgedeki tüm değişkenleri için değerleri atamış olduğunuz, bunları ya da farklı bir değer atamak gibi kullanabilirsiniz.  

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

Sanal makine yapılandırması oluşturun. Yapılandırma, bir sanal makine görüntüsü, boyutu, kimlik bilgileri gibi sanal makine dağıtımı sırasında kullanılan ayarları içerir.

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

#Create the virtual machine.
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
 
Sanal makineyle bir Uzak Masaüstü oturumu oluşturmak için aşağıdaki komutu kullanın. IP adresi, sanal makinenize Publicıpaddress ile değiştirin. İstendiğinde, kullanıcı adı ve sanal makine oluştururken kullandığınız parolayı girin.

```powershell
mstsc /v <publicIpAddress>
```

## <a name="install-iis-via-powershell"></a>PowerShell kullanarak IIS yükleme

Azure VM’de oturum açtıktan sonra tek bir PowerShell satırı kullanarak IIS yükleyebilir ve web trafiğine izin vermek üzere yerel güvenlik duvarı kuralını etkinleştirebilirsiniz. Bir PowerShell istemi açın ve şu komutu çalıştırın:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a>IIS karşılama sayfasını görüntüleme

Sanal makinenizde İnternet’ten IIS yüklenmiş ve bağlantı noktası 80 açık olduğunda, varsayılan IIS karşılama sayfasını görüntülemek için seçtiğiniz bir web tarayıcısını kullanabilirsiniz. Varsayılan sayfayı ziyaret etmek için yukarıda belgelediğiniz *publicIpAddress* değerini kullandığınızdan emin olun. 

![Varsayılan IIS sitesi](./media/azure-stack-quick-create-vm-windows-powershell/default-iis-website.png) 


## <a name="delete-the-virtual-machine"></a>Sanal makineyi silin

Artık gerekli olduğunda, sanal makineyi ve ilgili kaynaklarını içeren kaynak grubunu kaldırmak için aşağıdaki komutu kullanın:

```powershell
Remove-AzureRmResourceGroup `
  -Name $ResourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar

Bu Hızlı Başlangıç, basit bir Windows sanal makine dağıttıktan sonra. Azure yığın sanal makineler hakkında daha fazla bilgi için devam [Azure yığınında sanal makineler için ilgili önemli noktalar](azure-stack-vm-considerations.md).

