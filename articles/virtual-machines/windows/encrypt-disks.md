---
title: Azure'daki Windows sanal diskleri şifreleme | Microsoft Docs
description: Azure PowerShell kullanarak gelişmiş güvenlik için bir Windows VM'de sanal diskleri şifreleme
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/30/2018
ms.author: cynthn
ms.openlocfilehash: 28bb4c9fd4827f534c5f7bac2bae15451e3ca16c
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50914711"
---
# <a name="how-to-encrypt-virtual-disks-on-a-windows-vm"></a>Bir Windows VM'de sanal diskleri şifreleme
Azure'da sanal diskler, sanal makine (VM) geliştirilmiş güvenlik ve uyumluluk için şifrelenebilir. Diskler, bir Azure Key Vault'ta güvenli şifreleme anahtarları kullanılarak şifrelenir. Bu şifreleme anahtarlarını denetlemek ve bunların kullanılması denetleyebilirsiniz. Bu makalede, Azure PowerShell kullanarak bir Windows VM'de sanal diskleri şifreleme işlemi açıklanmaktadır. Ayrıca [Azure CLI kullanarak Linux VM şifreleme](../linux/encrypt-disks.md).

## <a name="overview-of-disk-encryption"></a>Disk şifrelemesi'ne genel bakış

Windows vm'lerinde sanal diskler, Bitlocker kullanılarak, bekleme sırasında şifrelenir. Azure'da sanal diskler şifrelemek için ücret alınmaz. Yazılım koruması kullanarak Azure Key Vault şifreleme anahtarlarını depolanır veya anahtarlarınızı FIPS 140-2 seviye 2 standartlarıyla sertifikalandırılmış donanım güvenlik modüllerinde (HSM'ler) oluşturun veya içeri aktarın. Bu şifreleme anahtarlarını, şifreleme ve şifre çözme, sanal Makineye eklenmiş sanal diskler için kullanılır. Bu şifreleme anahtarları denetiminizde korumak ve kullanımlarını denetleyebilirsiniz. 

Bir VM şifreleme işlemi aşağıdaki gibidir:

1. Bir şifreleme anahtarı Azure anahtar Kasası'nda oluşturun.
1. Disk şifreleme için kullanılabilir olması için şifreleme anahtarını yapılandırın.
1. Sanal diskler için disk şifrelemeyi etkinleştirir.
1. Gerekli şifreleme anahtarları Azure Key Vault'tan istenir.
1. Sanal diskler belirtilen şifreleme anahtarı kullanılarak şifrelenir.

### <a name="azure-key-vault"></a>Azure Key Vault

Disk şifrelemesi kullanır [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault/key-vault-whatis). Key Vault, şifreleme anahtarlarını ve gizli disk şifreleme/şifre çözme işlemi için kullanılan korumak için kullanılır. 
  * Varsa, var olan bir Azure anahtar Kasası'nı kullanabilirsiniz. Disk şifreleme için bir Key Vault ayırmanız gerekmez.
  * Yönetim sınırları ve anahtar görünürlük ayırmak için ayrılmış bir anahtar kasası oluşturabilirsiniz.


## <a name="requirements-and-limitations"></a>Gereksinimler ve sınırlamalar

Desteklenen senaryolar ve disk şifrelemesi için gereksinimleri:

* Azure Market görüntülerini veya özel bir VHD görüntüsü yeni Windows vm'lerinde şifrelemeyi etkinleştirme.
* Azure'daki mevcut Windows vm'lerinde şifrelemeyi etkinleştirme.
* Depolama alanları kullanılarak yapılandırılan Windows Vm'leri üzerinde şifrelemeyi etkinleştirme.
* İşletim sistemi ve veri şifrelemeyi devre dışı bırakmak için Windows Vm'leri beraberinde getirir.
* Tüm kaynakları (örneğin, Key Vault, depolama hesabı ve sanal makine), aynı Azure bölgesindeki ve abonelikte olmalıdır.
* Standart katmanı Vm'lerini, A, D, DS, G ve GS serisi VM'ler gibi.

Disk şifrelemesi, şu anda aşağıdaki senaryolar desteklenmez:

* Temel katmanı Vm'lerini.
* Klasik dağıtım modeli kullanılarak oluşturulan VM'ler.
* Şifreleme anahtarları zaten şifrelenmiş sanal makinesinde güncelleştiriliyor.
* Şirket içi anahtar yönetimi hizmeti ile tümleştirme.

## <a name="create-azure-key-vault-and-keys"></a>Azure Key Vault ve anahtarlar oluşturma

Başlamadan önce Azure PowerShell modülünün en yeni sürümünün yüklü olduğundan emin olun. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview). Komut örneklerinde, tüm örnek parametreler kendi adları, konum ve anahtar değerlerini değiştirin. Aşağıdaki örnekler, bir kuralı kullanmak *myResourceGroup*, *myKeyVault*, *myVM*, vb.

İlk adım, bir Azure Key Vault, şifreleme anahtarlarını depolamak için oluşturmaktır. Azure Key Vault, anahtarları, gizli dizileri veya uygulamalarınızda ve hizmetlerinizde güvenli bir şekilde uygulamak izin parolaları depolayabilirsiniz. Sanal disk şifreleme için şifreleme veya şifrelerini çözme, sanal diskler için kullanılan bir şifreleme anahtarı depolamak için bir anahtar kasası oluşturun. 

Azure Key Vault sağlayıcısının ile Azure aboneliğinizde etkinleştir [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider), bir kaynak grubu oluşturup [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Aşağıdaki örnek, bir kaynak grubu adı oluşturur *myResourceGroup* içinde *Doğu ABD* konumu:

```azurepowershell-interactive
$rgName = "myResourceGroup"
$location = "East US"

Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"
New-AzureRmResourceGroup -Location $location -Name $rgName
```

Azure Key Vault şifreleme anahtarlarını ve depolama alanı ve VM gibi ilişkili işlem kaynakları içeren aynı bölgede bulunmalıdır. Bir Azure Key Vault ile oluşturma [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) ve anahtar kasası disk şifrelemesi ile kullanmak için etkinleştirin. İçin benzersiz bir anahtar kasası adı belirtmeniz *keyVaultName* gibi:

```azurepowershell-interactive
$keyVaultName = "myKeyVault$(Get-Random)"
New-AzureRmKeyVault -Location $location `
    -ResourceGroupName $rgName `
    -VaultName $keyVaultName `
    -EnabledForDiskEncryption
```

Yazılım veya donanım güvenlik modeli (HSM) koruması kullanarak şifreleme anahtarlarını depolayabilirsiniz. Bir HSM kullanıldığında, anahtar kasası premium gerektirir. Bir premium yazılım korumalı anahtarlar depolayan standart Key Vault yerine Key Vault oluşturma için ek bir maliyet yoktur. Anahtar kasası premium oluşturmak için önceki adımda ekleme *- Sku "Premium"* parametreleri. Aşağıdaki örnek, standart bir Key Vault oluşturduğumuz bu yana yazılım korumalı anahtarlar kullanır. 

Her iki koruma modeli için Azure platformu sanal disklerin şifresini çözmek için VM önyükleme yaptığında, şifreleme anahtarlarını istemek için erişim verilmesi gerekir. Bir şifreleme anahtarı ile anahtar Kasası'nda oluşturma [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey). Aşağıdaki örnekte adlı bir anahtar oluşturur *myKey*:

```azurepowershell-interactive
Add-AzureKeyVaultKey -VaultName $keyVaultName `
    -Name "myKey" `
    -Destination "Software"
```

## <a name="create-virtual-machine"></a>Sanal makine oluşturma
Şifreleme işlemi test etmek için bir VM oluşturma [New-AzureRmVm](/powershell/module/azurerm.compute/new-azurermvm). Aşağıdaki örnekte adlı bir VM oluşturur *myVM* kullanarak bir *Windows Server 2016 Datacenter* görüntü. Kimlik bilgileri istendiğinde, VM'niz için kullanılacak parola ve kullanıcı adı girin:

```azurepowershell-interactive
$cred = Get-Credential

New-AzureRmVm `
    -ResourceGroupName $rgName `
    -Name "myVM" `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -Credential $cred
```


## <a name="encrypt-virtual-machine"></a>Sanal makinesi şifreleme
Sanal disklerini şifrelemek için tüm önceki bileşenleri bir araya:

1. Azure Active Directory Hizmet sorumlusu ve parolayı belirtin.
2. Şifrelenmiş diskleriniz için meta verileri depolamak için Key Vault belirtin.
3. Şifreleme anahtarları gerçek şifrelemeyi ve şifre çözme için kullanılacağını belirtin.
4. İşletim sistemi diski, veri disklerini veya tüm şifrelemek isteyip istemediğinizi belirtin.

Şifreleme ile sanal makinenize [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) Azure Key Vault anahtarını ve Azure Active Directory Hizmet sorumlusu kimlik bilgilerini kullanarak. Aşağıdaki örnekte tüm önemli bilgileri alır. ardından adlı VM şifreler *myVM*:

```azurepowershell-interactive
$keyVault = Get-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $rgName;
$diskEncryptionKeyVaultUrl = $keyVault.VaultUri;
$keyVaultResourceId = $keyVault.ResourceId;
$keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $keyVaultName -Name myKey).Key.kid;

Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgName `
    -VMName "myVM" `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl `
    -DiskEncryptionKeyVaultId $keyVaultResourceId `
    -KeyEncryptionKeyUrl $keyEncryptionKeyUrl `
    -KeyEncryptionKeyVaultId $keyVaultResourceId
```

VM şifreleme ile devam etmek için istemi kabul edin. İşlem sırasında yeniden başlatır. VM yeniden başlattı ve şifreleme işlemi tamamlandıktan sonra şifreleme durumunu gözden [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus):

```azurepowershell-interactive
Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName "myVM"
```

Çıktı aşağıdaki örneğe benzer:

```azurepowershell-interactive
OsVolumeEncrypted          : Encrypted
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OsVolume: Encrypted, DataVolumes: Encrypted
```

## <a name="next-steps"></a>Sonraki adımlar
* Azure anahtar Kasası'nı yönetme hakkında daha fazla bilgi için bkz. [sanal makineler için anahtar kasası ayarlama](key-vault-setup.md).
* Şifrelenmiş özel bir VM'yi Azure'a yüklemek için hazırlama gibi disk şifrelemesi hakkında daha fazla bilgi için bkz. [Azure Disk şifrelemesi](../../security/azure-security-disk-encryption.md).
