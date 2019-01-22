---
title: Azure PowerShell ile Azure ölçek kümeleri için diskleri şifreleme | Microsoft Docs
description: Sanal makine örnekleri ve Windows sanal makine ölçek kümesi bağlı diskleri şifrelemek için Azure PowerShell kullanmayı öğrenin
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/30/2018
ms.author: cynthn
ms.openlocfilehash: 8beebfc0bd845fc7dbe8b1f1665aba7820c78767
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54432090"
---
# <a name="encrypt-os-and-attached-data-disks-in-a-virtual-machine-scale-set-with-azure-powershell-preview"></a>İşletim sistemi ve Azure PowerShell ile (Önizleme) bir sanal makine ölçek kümesi bağlı veri diskleri şifreleme

Dosya korumak ve endüstri standardında bir şifreleme teknolojisi kullanılarak, bekleme sırasında verileri korumak için Azure Disk şifrelemesi (ADE) sanal makine ölçek kümelerini destekler. Şifreleme, Windows ve Linux sanal makinesi için etkinleştirilebilir ölçek kümeleri. Daha fazla bilgi için [için Azure Disk şifrelemesi Windows ve Linux](../security/azure-security-disk-encryption.md).

> [!NOTE]
>  Sanal makine ölçek kümeleri için Azure disk şifrelemesi şu an Azure tüm ortak bölgelerde kullanılabilir genel Önizleme aşamasındadır.

Azure disk şifrelemesi desteklenmez:
- Ölçek kümeleri yönetilen diskler ile oluşturulan ve yerel (veya yönetilmeyen) disk ölçek kümeleri için desteklenmiyor.
- Windows ölçek kümesi işletim sistemi ve veri birimleri için. Devre dışı şifreleme, Windows ölçek kümesi için işletim sistemi ve veri birimleri için desteklenir.
- Linux ölçek kümelerinde veri birimleri için. İşletim sistemi disk şifreleme geçerli Önizleme Linux ölçek kümeleri için desteklenmez.

Geçerli Önizleme sürümünde, Ölçek kümesi VM yeniden görüntü oluşturma ve yükseltme işlemleri desteklenmiyor. Azure disk şifrelemesi için sanal makine ölçek kümeleri önizlemesi yalnızca test ortamlarında önerilir. Önizleme sürümünde, burada bir şifrelenmiş bir ölçek kümesindeki bir işletim sistemi görüntüsüne yükseltmeniz gerekebilir üretim ortamlarında disk şifrelemesi etkinleştirmeyin.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz, bu öğretici Azure PowerShell modülü 5.7.0 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/azurerm/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="register-for-disk-encryption-preview"></a>Disk şifreleme Önizleme için kaydolun

Önizleme sanal makine ölçek kümeleri için Azure disk şifrelemesi, kendi aboneliğinize kaydetmeniz gerektirir [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature). Yalnızca ilk kez disk şifreleme önizleme özelliğini kullandığınızda aşağıdaki adımları gerekir:

```azurepowershell-interactive
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```

Bu, kayıt isteği yaymak için 10 dakikaya kadar sürebilir. Kayıt durumunu denetleyebilirsiniz [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature). Zaman `RegistrationState` raporları *kayıtlı*, yeniden kaydetmeniz *Microsoft.Compute* sağlayıcısıyla [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider):

```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```

## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>Disk şifrelemesi için etkin bir Azure Key Vault oluşturma

Azure Key Vault, anahtarları, gizli dizileri veya uygulamalarınızda ve hizmetlerinizde güvenli bir şekilde uygulamak izin parolaları depolayabilirsiniz. Yazılım koruması kullanarak Azure Key Vault şifreleme anahtarlarını depolanır veya anahtarlarınızı FIPS 140-2 seviye 2 standartlarıyla sertifikalandırılmış donanım güvenlik modüllerinde (HSM'ler) oluşturun veya içeri aktarın. Bu şifreleme anahtarlarını, şifreleme ve şifre çözme, sanal Makineye eklenmiş sanal diskler için kullanılır. Bu şifreleme anahtarları denetiminizde korumak ve kullanımlarını denetleyebilirsiniz.

Key Vault ile oluşturma [yeni-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault). Disk şifrelemesi için kullanılacak anahtar kasası izin verecek şekilde ayarlanmış *EnabledForDiskEncryption* parametresi. Aşağıdaki örnek, kaynak grubu adı, anahtar kasası adı ve konumu için değişkenleri ayrıca tanımlar. Kendi benzersiz Key Vault adı sağlayın:

```azurepowershell-interactive
$rgName="myResourceGroup"
$vaultName="myuniquekeyvault"
$location = "EastUS"

New-AzureRmResourceGroup -Name $rgName -Location $location
New-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $rgName -Location $location -EnabledForDiskEncryption
```

### <a name="use-an-existing-key-vault"></a>Var olan bir Key Vault kullanma

Bu adım yalnızca, disk şifrelemesi ile kullanmak istediğiniz bir anahtar kasası varsa gereklidir. Bir Key Vault önceki bölümde oluşturduğunuz bu adımı atlayın.

Disk şifrelemesi ile ölçek olarak aynı abonelik ve bölgede var olan bir anahtar Kasası'nı etkinleştirebilirsiniz [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/AzureRM.KeyVault/Set-AzureRmKeyVaultAccessPolicy). Varolan anahtar kasanıza adını tanımlayın *$vaultName* değişkeni aşağıdaki gibi:

```azurepowershell-interactive
$vaultName="myexistingkeyvault"
Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -EnabledForDiskEncryption
```

## <a name="create-a-scale-set"></a>Ölçek kümesi oluşturma

İlk olarak, VM örnekleri için [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) ile bir yönetici kullanıcı adı ve parola ayarlayın:

```azurepowershell-interactive
$cred = Get-Credential
```

Bu adımda [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss) ile bir sanal makine ölçek kümesi oluşturun. Tek tek sanal makine örneklerine trafiği dağıtmak için bir yük dengeleyici de oluşturulur. Yük dengeleyici hem TCP bağlantı noktası 80 üzerinden trafiği dağıtmak hem de TCP bağlantı noktası 3389 üzerinden uzak masaüstü trafiğine hem de TCP bağlantı noktası 5985 üzerinden PowerShell uzaktan iletişimine olanak tanımak için kurallar içerir:

```azurepowershell-interactive
$vmssName="myScaleSet"

New-AzureRmVmss `
    -ResourceGroupName $rgName `
    -VMScaleSetName $vmssName `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -PublicIpAddressName "myPublicIPAddress" `
    -LoadBalancerName "myLoadBalancer" `
    -UpgradePolicy "Automatic" `
    -Credential $cred
```

## <a name="enable-encryption"></a>Şifrelemeyi etkinleştir

Bir ölçek kümesindeki sanal makine örnekleri şifrelemek için önce bazı bilgiler ile Key Vault URI'si ve kaynak kimliğinde Al [Get-AzureRmKeyVault](/powershell/module/AzureRM.KeyVault/Get-AzureRmKeyVault). Bu değişkenler ardından ile şifreleme işlemi başlatmak için kullanılan [Set-AzureRmVmssDiskEncryptionExtension](/powershell/module/AzureRM.Compute/Set-AzureRmVmssDiskEncryptionExtension):

```azurepowershell-interactive
$diskEncryptionKeyVaultUrl=(Get-AzureRmKeyVault -ResourceGroupName $rgName -Name $vaultName).VaultUri
$keyVaultResourceId=(Get-AzureRmKeyVault -ResourceGroupName $rgName -Name $vaultName).ResourceId

Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $vmssName `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId –VolumeType "All"
```

İstendiğinde, yazın *y* disk şifreleme işlemi ölçek kümesi sanal Makinesinin devam etmek için örnekler.

## <a name="check-encryption-progress"></a>Şifreleme ilerleme durumunu denetleme

Disk şifreleme durumunu denetlemek için kullanmak [Get-AzureRmVmssDiskEncryption](/powershell/module/AzureRM.Compute/Get-AzureRmVmssDiskEncryption):

```azurepowershell-interactive
Get-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $vmssName
```

Sanal makine örnekleri şifrelediğinizde *EncryptionSummary* kod raporları *ProvisioningState ve başarılı* aşağıdaki örnek çıktıda gösterildiği gibi:

```powershell
ResourceGroupName            : myResourceGroup
VmScaleSetName               : myScaleSet
EncryptionSettings           :
  KeyVaultURL                : https://myuniquekeyvault.vault.azure.net/
  KeyEncryptionKeyURL        :
  KeyVaultResourceId         : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myuniquekeyvault
  KekVaultResourceId         :
  KeyEncryptionAlgorithm     :
  VolumeType                 : All
  EncryptionOperation        : EnableEncryption
EncryptionSummary[0]         :
  Code                       : ProvisioningState/succeeded
  Count                      : 2
EncryptionEnabled            : True
EncryptionExtensionInstalled : True
```

## <a name="disable-encryption"></a>Şifrelemeyi devre dışı bırakma

Artık şifrelenmiş VM örnekleri diskleri kullanmak istiyorsanız, şifrelemeyi devre dışı bırakabilirsiniz [Disable-AzureRmVmssDiskEncryption](/powershell/module/AzureRM.Compute/Disable-AzureRmVmssDiskEncryption) gibi:

```azurepowershell-interactive
Disable-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $vmssName
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure PowerShell bir sanal makine ölçek kümesi şifrelemek için kullanılır. Ayrıca [Azure CLI](virtual-machine-scale-sets-encrypt-disks-cli.md) veya şablonları [Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox) veya [Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox).
