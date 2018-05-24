---
title: Hızlı Başlangıç - Azure PowerShell ile Linux VM Oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta Azure PowerShell’i kullanarak Linux sanal makinesi oluşturmayı öğrenirsiniz
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/24/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: e910ced35738fcba27a8a1d7f2f010b2cd42e55d
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-create-a-linux-virtual-machine-in-azure-with-powershell"></a>Hızlı başlangıç: PowerShell ile Azure'da Linux sanal makinesi oluşturma

Azure PowerShell modülü, PowerShell komut satırından veya betik içinden Azure kaynakları oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıçta Azure PowerShell modülünü kullanarak Azure’da Ubuntu çalıştıran bir Linux sanal makinesinin (VM) nasıl dağıtılacağı gösterilir. VM’ye SSH oluşturup NGINX web sunucusunu yükleyerek VM’nizin çalıştığını görebilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz, bu öğretici Azure PowerShell modülü 5.7.0 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.

Son olarak, Windows kullanıcı profilinizin *.ssh* dizininde *id_rsa.pub* adlı bir ortak SSH anahtarının depolanması gerekir. SSH anahtarlarını oluşturma ve kullanma hakkında ayrıntılı bilgi için bkz. [Azure için SSH anahtarları oluşturma](ssh-from-windows.md).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) ile yeni bir Azure kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır:

```azurepowershell-interactive
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

## <a name="create-virtual-network-resources"></a>Sanal ağ kaynakları oluşturma

Bir sanal ağ, alt ağ ve genel IP adresi oluşturun. Bu kaynaklar, VM'ye ağ bağlantısı sağlamak ve VM'yi İnternet'e bağlamak için kullanılır:

```azurepowershell-interactive
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" -Location "EastUS" `
-Name "myVNET" -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName "myResourceGroup" -Location "EastUS" `
-AllocationMethod Static -IdleTimeoutInMinutes 4 -Name "mypublicdns$(Get-Random)"
```

Azure Ağ Güvenlik Grubu ve trafik kuralı oluşturun. Ağ güvenlik Grubu, gelen ve giden kurallarını kullanarak VM'nin güvenliğini sağlar. Aşağıdaki örnekte, TCP bağlantı noktası 22 için SSH bağlantılarına izin veren bir gelen kuralı oluşturulur. Gelen web trafiğine izin vermek amacıyla, TCP bağlantı noktası 80 için de bir gelen kuralı oluşturulur.

```azurepowershell-interactive
# Create an inbound network security group rule for port 22
$nsgRuleSSH = New-AzureRmNetworkSecurityRuleConfig -Name "myNetworkSecurityGroupRuleSSH"  -Protocol "Tcp" `
-Direction "Inbound" -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 22 -Access "Allow"

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig -Name "myNetworkSecurityGroupRuleWWW"  -Protocol "Tcp" `
-Direction "Inbound" -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 80 -Access "Allow"

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" -Location "EastUS" `
-Name "myNetworkSecurityGroup" -SecurityRules $nsgRuleSSH,$nsgRuleWeb
```

[New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) komutuyla bir sanal ağ arabirim kartı (NIC) oluşturun. Sanal NIC, VM'yi bir alt ağa, Ağ Güvenlik Grubu'na ve genel IP adresine bağlar.

```azurepowershell-interactive
# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name "myNic" -ResourceGroupName "myResourceGroup" -Location "EastUS" `
-SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Sanal makine yapılandırması, VM dağıtımı sırasında kullanılan VM görüntüsü, boyutu ve kimlik doğrulama seçenekleri gibi ayarları içerir. SSH kimlik bilgilerini, işletim sistemi bilgilerini ve VM boyutunu aşağıda gösterildiği gibi tanımlayın:

```azurepowershell-interactive
# Define a credential object
$securePassword = ConvertTo-SecureString ' ' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("azureuser", $securePassword)

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_D1" | `
Set-AzureRmVMOperatingSystem -Linux -ComputerName "myVM" -Credential $cred -DisablePasswordAuthentication | `
Set-AzureRmVMSourceImage -PublisherName "Canonical" -Offer "UbuntuServer" -Skus "16.04-LTS" -Version "latest" | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Configure SSH Keys
$sshPublicKey = Get-Content "$env:USERPROFILE\.ssh\id_rsa.pub"
Add-AzureRmVMSshPublicKey -VM $vmconfig -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"
```

Şimdi, [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) ile oluşturmak için önceki yapılandırma tanımlarını birleştirin:

```azurepowershell-interactive
New-AzureRmVM -ResourceGroupName "myResourceGroup" -Location eastus -VM $vmConfig
```

## <a name="connect-to-virtual-machine"></a>Sanal makineye bağlanma

Dağıtım tamamlandıktan sonra SSH VM'ye bağlanır. VM'nizin çalışmasını görmek için, NGINX web sunucusu yüklenir.

VM'nin genel IP adresini görmek için [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) cmdlet'ini kullanın:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -ResourceGroupName "myResourceGroup" | Select "IpAddress"
```

VM'ye bağlanmak için bir SSH istemcisi kullanın. Web tarayıcısından Azure Cloud Shell'i kullanabileceğiniz gibi, Windows kullanıcısıysanız [Putty](ssh-from-windows.md)'yi veya [Linux için Windows Alt Sistemi](/windows/wsl/install-win10)'ni de kullanabilirsiniz. VM'nizin genel IP adresini sağlayın:

```bash
ssh azureuser@IpAddress
```

Sorulduğunda, kullanıcı adı *azureuser* olarak girilmelidir. SSH anahtarlarınızla birlikte bir parola kullanılıyorsa, istendiğinde bu parolayı girmelisiniz.

## <a name="install-web-server"></a>Web sunucusunu yükleme

Sanal makinenizin çalıştığını görmek için NGINX web sunucusunu yükleyin. Paket kaynaklarını güncelleştirmek ve en son NGINX paketini yüklemek için SSH oturumunuzdan aşağıdaki komutları çalıştırın:

```bash
# update packages
sudo apt-get -y update

# install NGINX
sudo apt-get -y install nginx
```

İşiniz bittiğinde SSH oturumundan çıkın (`exit`)

## <a name="view-the-web-server-in-action"></a>Web sunucusunun çalıştığını görme

NGINX yüklendiğine ve VM’nizde İnternet üzerinden 80 numaralı bağlantı noktası açık olduğuna göre, varsayılan NGINX karşılama sayfasını görüntülemek için, seçtiğiniz bir web tarayıcısını kullanın. VM’nizin önceki bir adımda edinilen genel IP adresini kullanın. Aşağıdaki örnekte varsayılan NGINX web sitesi gösterilir:

![Varsayılan NGINX sitesi](./media/quick-create-cli/nginx.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) cmdlet’ini kullanarak kaynak grubunu, VM’yi ve tüm ilgili kaynakları kaldırabilirsiniz.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name "myResourceGroup"
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta basit bir sanal makine dağıttınız, bir Ağ Güvenlik Grubu ve kuralı oluşturdunuz ve temel bir web sunucusu yüklediniz. Azure sanal makineleri hakkında daha fazla bilgi için Linux VM’lerine yönelik öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Linux sanal makine öğreticileri](./tutorial-manage-vm.md)
