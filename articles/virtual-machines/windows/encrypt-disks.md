---
title: Azure'daki Windows sanal diskleri şifreleme | Microsoft Docs
description: Azure PowerShell kullanarak gelişmiş güvenlikten yararlanmaya başlamak için bir Windows VM'de sanal diskleri şifreleme
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
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
ms.openlocfilehash: fadbb4668dbaed46cc30841d2b04a92ea41cd5a1
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67718672"
---
# <a name="encrypt-virtual-disks-on-a-windows-vm"></a>Bir Windows VM'de sanal diskleri şifreleme
Azure'da sanal diskler, sanal makine (VM) geliştirilmiş güvenlik ve uyumluluk için şifrelenebilir. Diskler, bir Azure Key Vault'ta güvenli şifreleme anahtarları kullanılarak şifrelenir. Bu şifreleme anahtarlarını denetlemek ve bunların kullanılması denetleyebilirsiniz. Bu makalede, Azure PowerShell kullanarak bir Windows VM'de sanal diskleri şifreleme açıklar. Ayrıca [Azure CLI kullanarak Linux VM şifreleme](../linux/encrypt-disks.md).

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="overview-of-disk-encryption"></a>Disk şifrelemesi'ne genel bakış
Windows sanal makineleri sanal disklerde BitLocker'ı kullanarak bekleme durumundayken şifrelenir. Azure'da sanal diskler şifrelemek için ücret alınmaz. Yazılım Koruması'nı kullanarak bir Azure Key Vault'ta depolanan şifreleme anahtarları veya anahtarlarınızı FIPS 140-2 seviye 2 standartlarıyla sertifikalandırılmış donanım güvenlik modüllerinde (HSM'ler) oluşturun veya içeri aktarın. Şifreleme anahtarlarını, şifreleme ve şifre çözme, sanal Makineye eklenmiş sanal diskler için kullanılır. Bu şifreleme anahtarları denetiminizde korumak ve kullanımlarını denetleyebilirsiniz. 

Bir VM şifreleme işlemi aşağıdaki gibidir:

1. Bir şifreleme anahtarı Azure anahtar Kasası'nda oluşturun.
1. Disk şifreleme için kullanılabilir olması için şifreleme anahtarını yapılandırın.
1. Sanal diskler için disk şifrelemeyi etkinleştirir.
1. Gerekli şifreleme anahtarları Azure Key Vault'tan istenir.
1. Sanal diskler belirtilen şifreleme anahtarı kullanılarak şifrelenir.


## <a name="requirements-and-limitations"></a>Gereksinimler ve sınırlamalar

Desteklenen senaryolar ve disk şifrelemesi için gereksinimleri:

* Azure Market görüntülerini veya özel VHD görüntülerinizi yeni Windows vm'lerinde şifrelemeyi etkinleştirme.
* Azure'daki mevcut Windows vm'lerinde şifrelemeyi etkinleştirme.
* Depolama alanları kullanılarak yapılandırılan Windows Vm'leri üzerinde şifrelemeyi etkinleştirme.
* İşletim sistemi ve veri şifrelemeyi devre dışı bırakmak için Windows Vm'leri beraberinde getirir.
* Standart katmanı Vm'lerini, A, D, DS, G ve GS serisi VM'ler gibi.

    > [!NOTE]
    > (Key Vault, depolama hesabı ve VM dahil) tüm kaynaklar aynı Azure bölgesindeki ve abonelikte olmalıdır.

Disk şifrelemesi, aşağıdaki senaryolarda şu anda desteklenmemektedir:

* Temel katmanı Vm'lerini.
* Klasik dağıtım modeli kullanılarak oluşturulan VM'ler.
* Şifreleme anahtarları zaten şifrelenmiş sanal makinesinde güncelleştiriliyor.
* Şirket içi anahtar yönetimi hizmeti ile tümleştirme.


## <a name="create-an-azure-key-vault-and-keys"></a>Bir Azure Key Vault ve anahtarlar oluşturma
Başlamadan önce Azure PowerShell modülünün en son sürümünün yüklendiğinden emin olun. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview). Aşağıdaki komut örneklerinde, tüm örnek parametrelerini kendi adları, konum ve anahtar değerlerini gibi Değiştir *myResourceGroup*, *myKeyVault*, *myVM*, ve diğerleri.

İlk adım, bir Azure Key Vault, şifreleme anahtarlarını depolamak için oluşturmaktır. Azure Key Vault, anahtarları, gizli dizileri veya uygulamalarınızda ve hizmetlerinizde güvenli bir şekilde uygulamak izin parolaları depolayabilirsiniz. Sanal disk şifreleme için şifreleme veya şifrelerini çözme, sanal diskler için kullanılan bir şifreleme anahtarı depolamak için bir Key Vault oluşturacaksınız. 

Azure Key Vault sağlayıcısının ile Azure aboneliğinizde etkinleştirme [Register-AzResourceProvider](https://docs.microsoft.com/powershell/module/az.resources/register-azresourceprovider), sonra bir kaynak grubu oluşturun [yeni AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup). Aşağıdaki örnek, bir kaynak grubu adı oluşturur *myResourceGroup* içinde *Doğu ABD* konumu:

```azurepowershell-interactive
$rgName = "myResourceGroup"
$location = "East US"

Register-AzResourceProvider -ProviderNamespace "Microsoft.KeyVault"
New-AzResourceGroup -Location $location -Name $rgName
```

Azure Key Vault şifreleme anahtarlarını ve depolama alanı ve VM gibi ilişkili işlem kaynakları tutan aynı bölgede olması gerekir. Bir Azure Key Vault ile oluşturma [yeni AzKeyVault](https://docs.microsoft.com/powershell/module/az.keyvault/new-azkeyvault) ve anahtar kasası disk şifrelemesi ile kullanmak için etkinleştirin. İçin benzersiz bir anahtar kasası adı belirtmeniz *keyVaultName* gibi:

```azurepowershell-interactive
$keyVaultName = "myKeyVault$(Get-Random)"
New-AzKeyVault -Location $location `
    -ResourceGroupName $rgName `
    -VaultName $keyVaultName `
    -EnabledForDiskEncryption
```

Şifreleme anahtarlarını, yazılım veya donanım güvenlik modeli (HSM) koruması kullanarak depolayabilirsiniz.  Standart bir Key Vault, yazılım korumalı anahtarlar yalnızca depolar. HSM kullanmanın ek bir ücret ödemeden anahtar kasası premium gerektirir. Anahtar kasası premium oluşturmak için önceki adımda ekleme *- Sku "Premium"* parametresi. Aşağıdaki örnek, standart bir Key Vault oluşturduğumuz bu yana yazılım korumalı anahtarlar kullanır. 

Her iki koruma modeli için Azure platformu sanal disklerin şifresini çözmek için VM önyükleme yaptığında, şifreleme anahtarlarını istemek için erişim verilmesi gerekir. Bir şifreleme anahtarı ile anahtar Kasası'nda oluşturma [Add-AzureKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyvault/add-azkeyvaultkey). Aşağıdaki örnekte adlı bir anahtar oluşturur *myKey*:

```azurepowershell-interactive
Add-AzKeyVaultKey -VaultName $keyVaultName `
    -Name "myKey" `
    -Destination "Software"
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma
Şifreleme işlemi test etmek için bir VM oluşturma [New-AzVm](https://docs.microsoft.com/powershell/module/az.compute/new-azvm). Aşağıdaki örnekte adlı bir VM oluşturur *myVM* kullanarak bir *Windows Server 2016 Datacenter* görüntü. Kimlik bilgileri istendiğinde, VM'niz için kullanılacak parola ve kullanıcı adı girin:

```azurepowershell-interactive
$cred = Get-Credential

New-AzVm `
    -ResourceGroupName $rgName `
    -Name "myVM" `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -Credential $cred
```


## <a name="encrypt-a-virtual-machine"></a>Bir sanal makinesi şifreleme
Şifreleme ile sanal makinenize [kümesi AzVMDiskEncryptionExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmdiskencryptionextension) kullanarak Azure Key Vault anahtarı. Aşağıdaki örnekte tüm önemli bilgileri alır. ardından adlı VM şifreler *myVM*:

```azurepowershell-interactive
$keyVault = Get-AzKeyVault -VaultName $keyVaultName -ResourceGroupName $rgName;
$diskEncryptionKeyVaultUrl = $keyVault.VaultUri;
$keyVaultResourceId = $keyVault.ResourceId;
$keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $keyVaultName -Name myKey).Key.kid;

Set-AzVMDiskEncryptionExtension -ResourceGroupName $rgName `
    -VMName "myVM" `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl `
    -DiskEncryptionKeyVaultId $keyVaultResourceId `
    -KeyEncryptionKeyUrl $keyEncryptionKeyUrl `
    -KeyEncryptionKeyVaultId $keyVaultResourceId
```

VM şifreleme ile devam etmek için istemi kabul edin. İşlem sırasında yeniden başlatır. VM yeniden başlattı ve şifreleme işlemi tamamlandıktan sonra şifreleme durumunu gözden [Get-AzVmDiskEncryptionStatus](https://docs.microsoft.com/powershell/module/az.compute/get-azvmdiskencryptionstatus):

```azurepowershell-interactive
Get-AzVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName "myVM"
```

Çıktı aşağıdaki örneğe benzer:

```azurepowershell-interactive
OsVolumeEncrypted          : Encrypted
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OsVolume: Encrypted, DataVolumes: Encrypted
```

## <a name="next-steps"></a>Sonraki adımlar
* Bir Azure anahtar Kasası'nı yönetme hakkında daha fazla bilgi için bkz. [sanal makineler için bir anahtar kasası ayarlama](key-vault-setup.md).
* Şifrelenmiş özel bir VM'yi Azure'a yüklemek için hazırlama gibi disk şifrelemesi hakkında daha fazla bilgi için bkz. [Azure Disk şifrelemesi](../../security/azure-security-disk-encryption.md).
