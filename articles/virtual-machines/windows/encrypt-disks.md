---
title: "Azure Windows VM disklerde şifrelemek | Microsoft Docs"
description: "Azure PowerShell kullanarak gelişmiş güvenlik için bir Windows VM sanal disklerde şifreleme"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/10/2017
ms.author: iainfou
ms.openlocfilehash: 98b42b252a601af090579e3939f3c7ab91c3803b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-encrypt-virtual-disks-on-a-windows-vm"></a>Bir Windows VM sanal disklerde şifreleme
Geliştirilmiş sanal makine (VM) güvenlik ve uyumluluk için Azure sanal diskleri şifrelenebilir. Diskleri, bir Azure anahtar kasası güvenli şifreleme anahtarları kullanılarak şifrelenir. Bu şifreleme anahtarları denetlemek ve bunların kullanılması denetleyebilirsiniz. Bu makalede Windows Azure PowerShell kullanarak bir VM sanal disklerde şifrelemek nasıl ayrıntılarını verir. Ayrıca [Azure CLI 2.0 kullanarak bir Linux VM şifrelemek](../linux/encrypt-disks.md).

## <a name="overview-of-disk-encryption"></a>Disk şifrelemesi'ne genel bakış
Windows sanal makineleri sanal disklerde Bitlocker kullanılarak bekleme sırasında şifrelenir. Azure sanal diskleri şifreleme ücretsizdir. Şifreleme anahtarları yazılım koruması'nı kullanarak Azure anahtar kasası içinde depolanır veya içe aktarmak ya da anahtarlarınızı FIPS 140-2 Düzey 2 standartları sertifikalı donanım güvenlik modüllerinde (HSM'ler) oluşturur. Bu şifreleme anahtarlarını şifrelemek ve şifresini çözmek, VM'ye bağlı sanal diskler için kullanılır. Bu şifreleme anahtarları denetimin korumak ve kullanımlarını denetleyebilirsiniz. Bir Azure Active Directory Hizmet sorumlusu VM'ler açma ve kapatma gücü gibi bu şifreleme anahtarları verme için güvenli bir mekanizma sağlar.

Bir VM şifreleme işlemi aşağıdaki gibidir:

1. Bir şifreleme anahtarının bir Azure anahtar kasası oluşturun.
2. Diskleri şifrelemek için kullanılabilmesi için şifreleme anahtarını yapılandırın.
3. Şifreleme anahtarı Azure anahtar Kasası'okumak için bir Azure Active Directory hizmet asıl uygun izinlerle birlikte oluşturun.
4. Kullanılacak Azure Active Directory hizmet asıl ve uygun şifreleme anahtarını belirterek, sanal diskler şifrelemek için komutu yürütün.
5. Azure Active Directory hizmet asıl gereken şifreleme anahtarını Azure anahtar Kasası'ister.
6. Sanal diskler sağlanan şifreleme anahtarı kullanılarak şifrelenir.

## <a name="encryption-process"></a>Şifreleme işlemi
Disk şifrelemesi aşağıdaki ek bileşenlerini dayanır:

* **Azure anahtar kasası** - şifreleme anahtarları ve gizli anahtarları disk şifreleme/şifre çözme işlemi için kullanılan korumak için kullanılan. 
  * Varsa, mevcut bir Azure anahtar kasası kullanabilirsiniz. Diskleri şifrelemek için bir anahtar kasası ayrılması gerekmez.
  * Yönetim sınırlarını ve anahtar görünürlük ayırmak için ayrılmış bir anahtar kasası oluşturabilirsiniz.
* **Azure Active Directory** -güvenli değişimi gerekli şifreleme anahtarlarını ve kimlik doğrulaması istenen eylemler için işler. 
  * Genellikle, uygulamanızı barındırmak için var olan bir Azure Active Directory örneğini kullanabilirsiniz.
  * Hizmet sorumlusu istemek ve uygun şifreleme anahtarları verilmesi için güvenli bir mekanizma sağlar. Azure Active Directory ile tümleşen gerçek uygulama geliştirme değil.

## <a name="requirements-and-limitations"></a>Gereksinimler ve sınırlamalar
Desteklenen senaryolar ve disk şifrelemesi için gereksinimleri:

* Yeni Windows sanal makineleri Azure Market görüntülerini veya özel VHD görüntüsü üzerinde şifrelemeyi etkinleştirme.
* Var olan Windows sanal makineleri Azure üzerinde şifrelemeyi etkinleştirme.
* Depolama alanları kullanılarak yapılandırılan Windows VM'ler üzerinde şifrelemeyi etkinleştirme.
* İşletim sistemi ve veri şifreleme devre dışı bırakma Windows VM'ler için sürücüleri.
* Tüm kaynaklar (örneğin, anahtar kasası, depolama hesabı ve VM) aynı Azure bölgesinde ve abonelik olmalıdır.
* Standart katmanı VM'ler D, DS, G ve GS serisi VM'ler gibi.

Disk şifrelemesi aşağıdaki senaryolarda şu anda desteklenmiyor:

* Temel katman VM'ler.
* Klasik dağıtım modeli kullanılarak oluşturulan VM'ler.
* Şifreleme anahtarları zaten şifrelenmiş VM'de güncelleştiriliyor.
* Şirket içi anahtar yönetimi hizmeti ile tümleştirme.

## <a name="create-azure-key-vault-and-keys"></a>Azure anahtar kasası ve anahtarları oluşturma
Başlamadan önce Azure PowerShell modülünün en yeni sürümünün yüklü olduğundan emin olun. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview). Komut örnekleri tüm örnek parametreleri kendi adları, konum ve anahtar değerlerini değiştirin. Aşağıdaki örnekler, bir kuralı kullanmak *myResourceGroup*, *myKeyVault*, *myVM*, vb.

İlk adım, şifreleme anahtarlarını depolamak için bir Azure anahtar kasası oluşturmaktır. Azure anahtar kasası anahtarları, gizli ya da, uygulama ve hizmetlerinize güvenli bir şekilde uygulamak izin parolaları depolayabilirsiniz. Sanal disk şifreleme için şifreleme veya şifrelerini çözme, sanal diskler için kullanılan bir şifreleme anahtarı depolamak için bir anahtar kasası oluşturun. 

Azure aboneliğinizle içinde Azure anahtar kasası sağlayıcısını etkinleştir [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider), sahip bir kaynak grubu oluşturma [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Aşağıdaki örnek, bir kaynak grubu adı oluşturur *myResourceGroup* içinde *Doğu ABD* konumu:

```powershell
$rgName = "myResourceGroup"
$location = "East US"

Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"
New-AzureRmResourceGroup -Location $location -Name $rgName
```

Şifreleme anahtarlarını ve depolama ve VM gibi ilişkili işlem kaynakları içeren Azure anahtar kasası aynı bölgede bulunmalıdır. Bir Azure anahtar kasası ile oluşturma [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) ve anahtar kasası disk şifrelemesi ile kullanmak için etkinleştirin. İçin benzersiz bir anahtar kasası adı belirtmeniz *keyVaultName* gibi:

```powershell
$keyVaultName = "myUniqueKeyVaultName"
New-AzureRmKeyVault -Location $location `
    -ResourceGroupName $rgName `
    -VaultName $keyVaultName `
    -EnabledForDiskEncryption
```

Yazılım veya donanım güvenlik modeli (HSM) koruması kullanarak şifreleme anahtarlarını depolayabilirsiniz. HSM kullanmanın premium anahtar kasası gerektirir. Premium yazılım korumalı anahtarlar depolar standart anahtar kasası yerine anahtar kasası oluşturmak için ek bir maliyet yoktur. Premium anahtar kasası oluşturmak için önceki adımda ekleyin *- Sku "Premium"* parametreleri. Aşağıdaki örnek, standart bir anahtar kasası oluşturduğumuz bu yana yazılım korumalı anahtarlar kullanır. 

Her iki koruma modeli için Azure platformu VM sanal diskleri şifresini çözmek için önyüklendiğinde şifreleme anahtarları istemek için erişim verilmesi gerekir. Anahtar kasası ile bir şifreleme anahtarı oluşturmak [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey). Aşağıdaki örnek adlı bir anahtar oluşturur *myKey*:

```powershell
Add-AzureKeyVaultKey -VaultName $keyVaultName `
    -Name "myKey" `
    -Destination "Software"
```


## <a name="create-the-azure-active-directory-service-principal"></a>Azure Active Directory Hizmet sorumlusu oluşturma
Sanal diskler şifrelenmiş veya şifresi, kimlik doğrulama ve şifreleme anahtarlarının anahtar Kasası'ndan değişimi işlemek için bir hesap belirtin. Bu hesap, bir Azure Active Directory hizmet asıl adına VM uygun şifreleme anahtarları istemek Azure platformu sağlar. Birçok kuruluş Azure Active Directory dizinleri ayrılmış olsa, aboneliğinizde varsayılan Azure Active Directory örneği kullanılabilir.

Azure Active Directory ile bir hizmet sorumlusu oluşturma [yeni AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal). Güvenli bir parola belirtmek için izleyin [parola ilkeleri ve kısıtlamaları Azure Active Directory'de](../../active-directory/active-directory-passwords-policy.md):

```powershell
$appName = "My App"
$securePassword = "P@ssword!"
$app = New-AzureRmADApplication -DisplayName $appName `
    -HomePage "https://myapp.contoso.com" `
    -IdentifierUris "https://contoso.com/myapp" `
    -Password $securePassword
New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
```

Başarılı bir şekilde şifrelemek veya sanal diskleri şifresini çözmek için anahtar kasasında depolanan şifreleme anahtarı üzerindeki izinleri anahtarları okumak için Azure Active Directory Hizmet Asıl izin verecek şekilde ayarlanması gerekir. Anahtar kasası ile üzerindeki izinleri ayarla [kümesi AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy):

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyvaultName `
    -ServicePrincipalName $app.ApplicationId `
    -PermissionsToKeys "WrapKey" `
    -PermissionsToSecrets "Set"
```


## <a name="create-virtual-machine"></a>Sanal makine oluşturma
Şifreleme işlemi sınamak için bir VM oluşturalım. Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myVM* kullanarak bir *Windows Server 2016 Datacenter* görüntü:

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

$vnet = New-AzureRmVirtualNetwork -ResourceGroupName $rgName `
    -Location $location `
    -Name myVnet `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

$pip = New-AzureRmPublicIpAddress -ResourceGroupName $rgName `
    -Location $location `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4 `
    -Name "mypublicdns$(Get-Random)"

$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName `
    -Location $location `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleRDP

$nic = New-AzureRmNetworkInterface -Name myNic `
    -ResourceGroupName $rgName `
    -Location $location `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $pip.Id `
    -NetworkSecurityGroupId $nsg.Id

$cred = Get-Credential

$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize Standard_D1 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer -Skus 2016-Datacenter -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vmConfig
```


## <a name="encrypt-virtual-machine"></a>Sanal makine şifrele
Sanal diskler şifrelemek için birlikte tüm önceki bileşenleri getirin:

1. Azure Active Directory hizmet asıl ve parola belirtin.
2. Şifrelenmiş diskleriniz için meta verileri depolamak için anahtar kasası belirtin.
3. Gerçek şifreleme ve şifre çözme için kullanılacak şifreleme anahtarları belirtin.
4. İşletim sistemi diski, veri diskleri veya tüm şifrelemek isteyip istemediğinizi belirtin.

VM ile şifrelemek [kümesi AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) Azure anahtar kasası anahtar ve Azure Active Directory hizmet asıl kimlik bilgilerini kullanarak. Aşağıdaki örnek anahtar bilgilerini alır sonra adlı VM şifreler *myVM*:

```powershell
$keyVault = Get-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $rgName;
$diskEncryptionKeyVaultUrl = $keyVault.VaultUri;
$keyVaultResourceId = $keyVault.ResourceId;
$keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $keyVaultName -Name myKey).Key.kid;

Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgName `
    -VMName $vmName `
    -AadClientID $app.ApplicationId `
    -AadClientSecret $securePassword `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl `
    -DiskEncryptionKeyVaultId $keyVaultResourceId `
    -KeyEncryptionKeyUrl $keyEncryptionKeyUrl `
    -KeyEncryptionKeyVaultId $keyVaultResourceId
```

VM şifreleme ile devam etmek için isteğini kabul edin. VM, işlem sırasında yeniden başlatır. VM yeniden başlattı ve şifreleme işlemi tamamlandıktan sonra şifreleme durumunu gözden [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus):

```powershell
Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName $vmName
```

Çıktı aşağıdaki örneğe benzer:

```powershell
OsVolumeEncrypted          : Encrypted
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OsVolume: Encrypted, DataVolumes: Encrypted
```

## <a name="next-steps"></a>Sonraki adımlar
* Azure anahtar kasası yönetme hakkında daha fazla bilgi için bkz: [sanal makineler için anahtar kasasını oluşturup](key-vault-setup.md).
* Azure için karşıya yüklemek için şifrelenmiş bir özel VM hazırlama gibi disk şifrelemesi hakkında daha fazla bilgi için bkz: [Azure Disk şifrelemesi](../../security/azure-security-disk-encryption.md).
