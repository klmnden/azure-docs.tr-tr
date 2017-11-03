---
title: "Azure yığınında PowerShell kullanarak bir Linux sanal makine oluşturma | Microsoft Docs"
description: "Azure yığınında PowerShell ile Linux sanal makine oluşturun."
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
ms.openlocfilehash: 579246a2f5aefda0d48cea235d74f196cd814331
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-linux-virtual-machine-by-using-powershell-in-azure-stack"></a>Azure yığınında PowerShell kullanarak bir Linux sanal makine oluşturma 

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Azure PowerShell oluşturmak ve kaynak Azure yığınından bir komut satırı veya komut dosyalarını yönetmek için kullanılır.  PowerShell kullanarak Azure yığınında Ubuntu server çalıştıran bir sanal makine oluşturmak için bu kılavuzu ayrıntıları.

## <a name="prerequisites"></a>Ön koşullar 

* Azure yığın operatörünüze Azure yığın Market "Ubuntu Server 16.04 LTS" görüntü ekledi emin olun.  

* Azure yığını, Azure PowerShell oluşturmak ve kaynakları yönetmek için belirli bir sürümünü gerektirir. PowerShell Azure yığını için yapılandırılmış yoksa, oturum [Geliştirme Seti](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), veya kullanıyorsanız Windows tabanlı bir dış istemcinin [VPN üzerinden bağlı](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) ve adımlarını izleyin [ Yükleme](azure-stack-powershell-install.md) ve [yapılandırma](azure-stack-powershell-configure-user.md) PowerShell.    

* Genel SSH anahtarı adı id_rsa.pub ile Windows kullanıcı profiliniz .ssh dizininin oluşturulması gerekir. SSH anahtarları oluşturma hakkında ayrıntılı bilgi için bkz: [oluşturma SSH anahtarları Windows'da](../../virtual-machines/linux/ssh-from-windows.md).  

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

Bir depolama hesabı ve Ubuntu Server 16.04 LTS görüntüyü depolamak için bir depolama kapsayıcısı oluşturun.

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

Ağ güvenlik grubu, gelen ve giden kuralları kullanarak sanal makine güvenliğini sağlar. Gelen Uzak Masaüstü bağlantılarına izin verecek şekilde 3389 numaralı bağlantı noktası için bir gelen kuralı ve gelen web trafiği izin vermek için 80 numaralı bağlantı noktası için bir gelen kuralı oluşturalım.

```powershell
# Create an inbound network security group rule for port 22
$nsgRuleSSH = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleSSH  -Protocol Tcp `
-Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 22 -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleWWW  -Protocol Tcp `
-Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 80 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName myResourceGroup -Location local `
-Name myNetworkSecurityGroup -SecurityRules $nsgRuleSSH,$nsgRuleWeb
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
Sanal makine yapılandırması oluşturun. Bu yapılandırma, sanal makineyi dağıtırken kullanılan sanal makine görüntüsü, boyutu ve kimlik doğrulama yapılandırması gibi ayarları içerir.

```powershell
# Define a credential object.
$UserName='demouser'
$securePassword = ConvertTo-SecureString ' ' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ($UserName, $securePassword)

# Create the virtual machine configuration object
$VmName = "VirtualMachinelatest"
$VmSize = "Standard_D1"
$VirtualMachine = New-AzureRmVMConfig `
  -VMName $VmName `
  -VMSize $VmSize 

$VirtualMachine = Set-AzureRmVMOperatingSystem `
  -VM $VirtualMachine `
  -Linux `
  -ComputerName "MainComputer" `
  -Credential $cred 

$VirtualMachine = Set-AzureRmVMSourceImage `
  -VM $VirtualMachine `
  -PublisherName "Canonical" `
  -Offer "UbuntuServer" `
  -Skus "16.04-LTS" `
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

# Configure SSH Keys
$sshPublicKey = Get-Content "$env:USERPROFILE\.ssh\id_rsa.pub"

# Adds the SSH Key to the virtual machine
Add-AzureRmVMSshPublicKey -VM $VirtualMachine `
 -KeyData $sshPublicKey `
 -Path "/home/azureuser/.ssh/authorized_keys"

#Create the virtual machine.
New-AzureRmVM `
  -ResourceGroupName $ResourceGroupName `
 -Location $location `
  -VM $VirtualMachine 
```

## <a name="connect-to-the-virtual-machine"></a>Sanal makineye bağlanma

Dağıtım tamamlandıktan sonra sanal makine ile bir SSH bağlantısı oluşturun. Sanal makinenin genel IP adresini döndürmek için [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress?view=azurermps-4.3.1) komutunu kullanın.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

SSH yüklü bir sistemden sanal makineye bağlanmak için aşağıdaki komutu kullanın. Windows üzerinde çalışıyorsanız, kullanabileceğiniz [Putty](http://www.putty.org/) bağlantı oluşturmak için.

```
ssh <Public IP Address>
```

İstendiğinde, oturum açma kullanıcı azureuser adıdır. SSH anahtarları oluşturulurken bir şifre girildiyse, bu şifreyi de girmeniz gerekir.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekli olduğunda, kullanabileceğiniz [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup?view=azurermps-4.3.1) VM, kaynak grubunu kaldırmak için komut ve tüm ilgili kaynaklar:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu Hızlı Başlangıç, basit Linux sanal makine dağıttıktan sonra. Azure yığın sanal makineler hakkında daha fazla bilgi için devam [Azure yığınında sanal makineler için ilgili önemli noktalar](azure-stack-vm-considerations.md).
