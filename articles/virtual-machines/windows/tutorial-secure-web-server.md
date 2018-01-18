---
title: "SSL sertifikalarının Azure ile IIS güvenli | Microsoft Docs"
description: "IIS web sunucusuna Azure Windows VM üzerinde SSL sertifikalarıyla güvenli öğrenin"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/14/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 43f06422e1120f1c3b2a9d9d5d4be515213c0937
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
# <a name="secure-iis-web-server-with-ssl-certificates-on-a-windows-virtual-machine-in-azure"></a>IIS web sunucusu sanal bir makinede Windows Azure SSL sertifikalarıyla güvenli
Web sunucularının güvenliğini sağlamak için bir Güvenli Yuva Katmanı (SSL) sertifikası web trafiğini şifrelemek için kullanılabilir. Bu SSL sertifikalarını Azure anahtar kasası depolanabilir ve Azure'da Windows sanal makineleri (VM'ler) sertifikaların güvenli dağıtımlar izin. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure anahtar kasası oluşturma
> * Oluşturmak veya bir sertifika anahtar Kasası'na Karşıya Yükle
> * Bir VM oluşturun ve IIS web sunucusunu yükleme
> * Sertifika VM ekleme ve SSL bağlaması ile IIS yapılandırma

Bu öğretici, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps).


## <a name="overview"></a>Genel Bakış
Azure anahtar kasası, şifreleme anahtarlarını ve gizli anahtarları, bu tür sertifikalar veya parolaları koruma sağlar. Anahtar kasası, sertifika yönetimi işlemini kolaylaştırmaya yardımcı olur ve bu sertifikaları erişim anahtarlarını denetim kurmanızı sağlar. Anahtar kasası içinde otomatik olarak imzalanan bir sertifika oluşturun veya zaten sahip olduğunuz bir varolan, güvenilen bir sertifika yükleyin.

Sertifikaları içeren özel bir VM görüntüsü kullanarak yerine desteklenmiş, sertifikaları çalışan VM ekleme. Bu işlem en güncel sertifikaları dağıtımı sırasında bir web sunucusunda yüklü olan sağlar. Yenileme veya bir sertifikayı değiştirin, yeni bir özel VM görüntüsü oluşturmanız gerekmez. Ek sanal makineleri oluştururken en son sertifikaları otomatik olarak eklenmiş. Tüm işlem sırasında sertifikalar hiçbir zaman Azure platformu bırakın veya bir komut dosyası, komut satırı geçmiş veya şablonda sunulur.


## <a name="create-an-azure-key-vault"></a>Azure anahtar kasası oluşturma
Bir anahtar kasası ve sertifikaları oluşturmadan önce bir kaynak grubuyla oluşturmanız [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroupSecureWeb* içinde *Doğu ABD* konumu:

```powershell
$resourceGroup = "myResourceGroupSecureWeb"
$location = "East US"
New-AzureRmResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

Ardından, bir anahtar kasası ile oluşturma [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault). Her anahtar kasası benzersiz bir ad gerektirir ve tüm küçük olmalıdır. Değiştir `<mykeyvault>` aşağıdaki örnekte kendi benzersiz bir anahtar kasası ad ile:

```powershell
$keyvaultName="<mykeyvault>"
New-AzureRmKeyVault -VaultName $keyvaultName `
    -ResourceGroup $resourceGroup `
    -Location $location `
    -EnabledForDeployment
```

## <a name="generate-a-certificate-and-store-in-key-vault"></a>Bir sertifika oluşturmak ve anahtar kasasına depolamak
Üretim kullanımı için güvenilen bir sağlayıcı tarafından imzalanmış geçerli bir sertifika almanız gerekir [alma AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/import-azurekeyvaultcertificate). Bu öğretici için aşağıdaki örnek, otomatik olarak imzalanan bir sertifika ile nasıl oluşturabileceğiniz gösterir [Ekle AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/add-azurekeyvaultcertificate) varsayılan sertifika ilkesinden kullanan [ AzureKeyVaultCertificatePolicy yeni](/powershell/module/azurerm.keyvault/new-azurekeyvaultcertificatepolicy). 

```powershell
$policy = New-AzureKeyVaultCertificatePolicy `
    -SubjectName "CN=www.contoso.com" `
    -SecretContentType "application/x-pkcs12" `
    -IssuerName Self `
    -ValidityInMonths 12

Add-AzureKeyVaultCertificate `
    -VaultName $keyvaultName `
    -Name "mycert" `
    -CertificatePolicy $policy 
```


## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma
Bir yönetici ile VM için kullanıcı adı ve parola ayarlayın [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

VM oluşturmak üzere, şimdi [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Aşağıdaki örnek gereken sanal ağ bileşenleri, işletim sistemi yapılandırması ve adlı bir VM oluşturur *myVM*:

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -Name "myVnet" `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$publicIP = New-AzureRmPublicIpAddress `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4 `
    -Name "myPublicIP"

# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name "myNetworkSecurityGroupRuleRDP"  `
    -Protocol "Tcp" `
    -Direction "Inbound" `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

# Create an inbound network security group rule for port 443
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig `
    -Name "myNetworkSecurityGroupRuleWWW"  `
    -Protocol "Tcp" `
    -Direction "Inbound" `
    -Priority 1001 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 443 `
    -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -Name "myNetworkSecurityGroup" `
    -SecurityRules $nsgRuleRDP,$nsgRuleWeb

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
    -Name "myNic" `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIP.Id `
    -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS2" | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName "myVM" -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName "MicrosoftWindowsServer" `
    -Offer "WindowsServer" -Skus "2016-Datacenter" -Version "latest" | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create virtual machine
New-AzureRmVM -ResourceGroupName $resourceGroup -Location $location -VM $vmConfig

# Use the Custom Script Extension to install IIS
Set-AzureRmVMExtension -ResourceGroupName $resourceGroup `
    -ExtensionName "IIS" `
    -VMName "myVM" `
    -Location $location `
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server -IncludeManagementTools"}'
```

Oluşturulacak VM için birkaç dakika sürer. Son adımda Azure özel betik uzantısının IIS web sunucusu ile yüklemek için kullanır. [kümesi AzureRmVmExtension](/powershell/module/azurerm.compute/set-azurermvmextension).


## <a name="add-a-certificate-to-vm-from-key-vault"></a>Bir sertifika VM'ye anahtar Kasası'nı ekleyin.
Sertifika için bir VM anahtar Kasası'nı eklemek için sertifikayla kimliği elde [Get-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/get-azurekeyvaultsecret). VM ile sertifika eklemek [Ekle AzureRmVMSecret](/powershell/module/azurerm.compute/add-azurermvmsecret):

```powershell
$certURL=(Get-AzureKeyVaultSecret -VaultName $keyvaultName -Name "mycert").id

$vm=Get-AzureRMVM -ResourceGroupName $resourceGroup -Name "myVM"
$vaultId=(Get-AzureRmKeyVault -ResourceGroupName $resourceGroup -VaultName $keyVaultName).ResourceId
$vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $vaultId -CertificateStore "My" -CertificateUrl $certURL

Update-AzureRmVM -ResourceGroupName $resourceGroup -VM $vm
```


## <a name="configure-iis-to-use-the-certificate"></a>Sertifikayı kullanmak üzere IIS'yi yapılandırmak
Özel betik uzantısı'yla yeniden kullanmak [kümesi AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) IIS yapılandırmasını güncelleştirmek için. Bu güncelleştirme için IIS anahtar Kasası'nı eklenen sertifika uygular ve web bağlama yapılandırır:

```powershell
$PublicSettings = '{
    "fileUris":["https://raw.githubusercontent.com/iainfoulds/azure-samples/master/secure-iis.ps1"],
    "commandToExecute":"powershell -ExecutionPolicy Unrestricted -File secure-iis.ps1"
}'

Set-AzureRmVMExtension -ResourceGroupName $resourceGroup `
    -ExtensionName "IIS" `
    -VMName "myVM" `
    -Location $location `
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -SettingString $publicSettings
```


### <a name="test-the-secure-web-app"></a>Güvenli web uygulamayı test etme
VM ile genel IP adresi elde [Get-AzureRmPublicIPAddress](/powershell/resourcemanager/azurerm.network/get-azurermpublicipaddress). Aşağıdaki örnek IP adresi alacağı `myPublicIP` daha önce oluşturduğunuz:

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName $resourceGroup -Name "myPublicIP" | select "IpAddress"
```

Bir web tarayıcısı açın ve girin artık `https://<myPublicIP>` adres çubuğundaki. Kendinden imzalı bir sertifika kullanıyorsa uyarı güvenlik kabul etmeyi seçin **ayrıntıları** ve ardından **Web sayfasına gidin**:

![Web tarayıcısı güvenlik uyarısını kabul edin](./media/tutorial-secure-web-server/browser-warning.png)

Güvenli, IIS Web sitesi sonra aşağıdaki örnekte olduğu gibi görüntülenir:

![Görünüm çalıştırma güvenli IIS sitesi](./media/tutorial-secure-web-server/secured-iis.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure anahtar kasasında depolanan bir SSL sertifikası ile bir IIS web sunucusu güvenli. Şunları öğrendiniz:

> [!div class="checklist"]
> * Azure anahtar kasası oluşturma
> * Oluşturmak veya bir sertifika anahtar Kasası'na Karşıya Yükle
> * Bir VM oluşturun ve IIS web sunucusunu yükleme
> * Sertifika VM ekleme ve SSL bağlaması ile IIS yapılandırma

Önceden derlenmiş sanal makine kod örnekleri görmek için bu bağlantıyı izleyin.

> [!div class="nextstepaction"]
> [Windows sanal makine kod örnekleri](./powershell-samples.md)
