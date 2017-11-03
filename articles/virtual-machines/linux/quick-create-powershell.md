---
title: "Azure Hızlı Başlangıç - VM PowerShell oluşturma | Microsoft Docs"
description: "PowerShell ile Linux sanal makinesi oluşturmayı hızlı bir şekilde öğrenin"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/13/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: fdd83f2386055fa9fac1ad50f4b01bf4419342b5
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2017
---
# <a name="create-a-linux-virtual-machine-with-powershell"></a>PowerShell ile Linux sanal makinesi oluşturma

Azure PowerShell modülü, PowerShell komut satırından veya betik içinden Azure kaynakları oluşturmak ve yönetmek için kullanılır. Bu kılavuzda Ubuntu sunucusu çalıştıran bir sanal makineyi Azure PowerShell modülü kullanarak dağıtma işleminin ayrıntıları verilmektedir. Sunucu dağıtıldıktan sonra bir SSH bağlantısı oluşturulur ve bir NGINX web sunucusu yüklenir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu hızlı başlangıç, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-azurerm-ps).

Son olarak, Windows kullanıcı profilinizin *.ssh* dizininde *id_rsa.pub* adlı bir ortak SSH anahtarının depolanması gerekir. Azure için SSH anahtarları oluşturma hakkında ayrıntılı bilgi için bkz. [Azure için SSH anahtarları oluşturma](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="log-in-to-azure"></a>Azure'da oturum açma

`Login-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) ile yeni bir Azure kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location eastus
```

## <a name="create-networking-resources"></a>Ağ kaynakları oluşturma

Bir sanal ağ, alt ağ ve genel IP adresi oluşturun. Bu kaynaklar, sanal makineye ağ bağlantısı sağlamak ve sanal makineyi İnternet'e bağlamak için kullanılır.

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName myResourceGroup -Location eastus `
-Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup -Location eastus `
-AllocationMethod Static -IdleTimeoutInMinutes 4 -Name "mypublicdns$(Get-Random)"
```

Bir ağ güvenliği grubu ve bir ağ güvenliği grup kuralı oluşturun. Ağ güvenliği grubu, gelen ve giden kuralları kullanarak sanal makinenin güvenliğini sağlar. Bu durumda, bağlantı noktası 22 için gelen SSH bağlantılarına izin veren bir gelen kuralı oluşturulur. Ayrıca, gelen web trafiği sağlayan bağlantı noktası 80 için bir gelen kuralı oluşturmak istiyoruz.

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
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName myResourceGroup -Location eastus `
-Name myNetworkSecurityGroup -SecurityRules $nsgRuleSSH,$nsgRuleWeb
```

Sanal makine için [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) ile bir ağ kartı oluşturun. Ağ kartı, sanal makineyi bir alt ağa, ağ güvenliği grubuna ve genel IP adresine bağlar.

```powershell
# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName myResourceGroup -Location eastus `
-SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

Sanal makine yapılandırması oluşturun. Bu yapılandırma, sanal makineyi dağıtırken kullanılan sanal makine görüntüsü, boyutu ve kimlik doğrulama yapılandırması gibi ayarları içerir.

```powershell
# Define a credential object
$securePassword = ConvertTo-SecureString ' ' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("azureuser", $securePassword)

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_D1 | `
Set-AzureRmVMOperatingSystem -Linux -ComputerName myVM -Credential $cred -DisablePasswordAuthentication | `
Set-AzureRmVMSourceImage -PublisherName Canonical -Offer UbuntuServer -Skus 16.04-LTS -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Configure SSH Keys
$sshPublicKey = Get-Content "$env:USERPROFILE\.ssh\id_rsa.pub"
Add-AzureRmVMSshPublicKey -VM $vmconfig -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"
```

[New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) ile sanal makineyi oluşturun.

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroup -Location eastus -VM $vmConfig
```

## <a name="connect-to-virtual-machine"></a>Sanal makineye bağlanma

Dağıtım tamamlandıktan sonra sanal makine ile bir SSH bağlantısı oluşturun.

Sanal makinenin genel IP adresini döndürmek için [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) komutunu kullanın.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

SSH yüklü bir sistemden sanal makineye bağlanmak için aşağıdaki komutu kullanın. Windows üzerinde çalışıyorsanız, bağlantı oluşturmak için [Putty](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-ssh-from-windows?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-private-key-for-putty) kullanılabilir. 

```bash 
ssh <Public IP Address>
```

Sorulduğunda, kullanıcı adı *azureuser* olarak girilmelidir. SSH anahtarları oluşturulurken bir şifre girildiyse, bu şifreyi de girmeniz gerekir.


## <a name="install-nginx"></a>NGINX yükleme

Paket kaynaklarını güncelleştirmek ve en son NGINX paketini yüklemek için aşağıdaki komutları kullanın. 

```bash 
# update package source
sudo apt-get -y update

# install NGINX
sudo apt-get -y install nginx
```

## <a name="view-the-ngix-welcome-page"></a>NGIX karşılama sayfasını görüntüleme

NGINX yüklendiğine ve VM’nizde İnternet üzerinden 80 numaralı bağlantı noktası açık olduğuna göre, varsayılan NGINX karşılama sayfasını görüntülemek için, seçtiğiniz bir web tarayıcısını kullanabilirsiniz. Varsayılan sayfayı ziyaret etmek için yukarıda belgelediğiniz genel IP adresini kullandığınızdan emin olun. 

![Varsayılan NGINX sitesi](./media/quick-create-cli/nginx.png) 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanarak kaynak grubunu, VM’yi ve tüm ilgili kaynakları kaldırabilirsiniz.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta basit bir sanal makine ve bir ağ güvenlik grubu kuralı dağıtıp, bir web sunucusu yüklediniz. Azure sanal makineleri hakkında daha fazla bilgi için Linux VM’lerine yönelik öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Linux sanal makine öğreticileri](./tutorial-manage-vm.md)
