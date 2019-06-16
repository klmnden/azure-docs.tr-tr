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
ms.date: 04/26/2019
ms.author: cynthn
ms.openlocfilehash: a582a4787a4b215d82dcbff60be8853793f92c32
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66728360"
---
# <a name="encrypt-os-and-attached-data-disks-in-a-virtual-machine-scale-set-with-azure-powershell"></a>İşletim sistemi ve Azure PowerShell ile bir sanal makine ölçek kümesi bağlı veri diskleri şifreleme

Dosya korumak ve endüstri standardında bir şifreleme teknolojisi kullanılarak, bekleme sırasında verileri korumak için Azure Disk şifrelemesi (ADE) sanal makine ölçek kümelerini destekler. Şifreleme, Windows ve Linux sanal makinesi için etkinleştirilebilir ölçek kümeleri. Daha fazla bilgi için [için Azure Disk şifrelemesi Windows ve Linux](../security/azure-security-disk-encryption.md).

Azure disk şifrelemesi desteklenmez:
- Ölçek kümeleri yönetilen diskler ile oluşturulan ve yerel (veya yönetilmeyen) disk ölçek kümeleri için desteklenmiyor.
- Windows ölçek kümesi işletim sistemi ve veri birimleri için. Devre dışı şifreleme, Windows ölçek kümesi için işletim sistemi ve veri birimleri için desteklenir.
- Linux ölçek kümelerinde veri birimleri için. İşletim sistemi disk şifreleme Linux ölçek kümeleri için şu anda desteklenmiyor.

[!INCLUDE [updated-for-az.md](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>Disk şifrelemesi için etkin bir Azure Key Vault oluşturma

Azure Key Vault, anahtarları, gizli dizileri veya uygulamalarınızda ve hizmetlerinizde güvenli bir şekilde uygulamak izin parolaları depolayabilirsiniz. Yazılım koruması kullanarak Azure Key Vault şifreleme anahtarlarını depolanır veya anahtarlarınızı FIPS 140-2 seviye 2 standartlarıyla sertifikalandırılmış donanım güvenlik modüllerinde (HSM'ler) oluşturun veya içeri aktarın. Bu şifreleme anahtarlarını, şifreleme ve şifre çözme, sanal Makineye eklenmiş sanal diskler için kullanılır. Bu şifreleme anahtarları denetiminizde korumak ve kullanımlarını denetleyebilirsiniz.

Key Vault ile oluşturma [AzKeyVault yeni](/powershell/module/az.keyvault/new-azkeyvault). Disk şifrelemesi için kullanılacak anahtar kasası izin verecek şekilde ayarlanmış *EnabledForDiskEncryption* parametresi. Aşağıdaki örnek, kaynak grubu adı, anahtar kasası adı ve konumu için değişkenleri ayrıca tanımlar. Kendi benzersiz Key Vault adı sağlayın:

```azurepowershell-interactive
$rgName="myResourceGroup"
$vaultName="myuniquekeyvault"
$location = "EastUS"

New-AzResourceGroup -Name $rgName -Location $location
New-AzKeyVault -VaultName $vaultName -ResourceGroupName $rgName -Location $location -EnabledForDiskEncryption
```

### <a name="use-an-existing-key-vault"></a>Var olan bir Key Vault kullanma

Bu adım yalnızca, disk şifrelemesi ile kullanmak istediğiniz bir anahtar kasası varsa gereklidir. Bir Key Vault önceki bölümde oluşturduğunuz bu adımı atlayın.

Disk şifrelemesi ile ölçek olarak aynı abonelik ve bölgede var olan bir anahtar Kasası'nı etkinleştirebilirsiniz [kümesi AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/Set-AzKeyVaultAccessPolicy). Varolan anahtar kasanıza adını tanımlayın *$vaultName* değişkeni aşağıdaki gibi:


```azurepowershell-interactive
$vaultName="myexistingkeyvault"
Set-AzKeyVaultAccessPolicy -VaultName $vaultName -EnabledForDiskEncryption
```

## <a name="create-a-scale-set"></a>Ölçek kümesi oluşturma

İlk olarak, VM örnekleri için [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) ile bir yönetici kullanıcı adı ve parola ayarlayın:

```azurepowershell-interactive
$cred = Get-Credential
```

Artık bir sanal makine ölçek kümesi oluşturma [yeni AzVmss](/powershell/module/az.compute/new-azvmss). Tek tek sanal makine örneklerine trafiği dağıtmak için bir yük dengeleyici de oluşturulur. Yük dengeleyici hem TCP bağlantı noktası 80 üzerinden trafiği dağıtmak hem de TCP bağlantı noktası 3389 üzerinden uzak masaüstü trafiğine hem de TCP bağlantı noktası 5985 üzerinden PowerShell uzaktan iletişimine olanak tanımak için kurallar içerir:

```azurepowershell-interactive
$vmssName="myScaleSet"

New-AzVmss `
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

## <a name="enable-encryption"></a>Şifrelemeyi etkinleştirme

Bir ölçek kümesindeki sanal makine örnekleri şifrelemek için önce bazı bilgiler ile Key Vault URI'si ve kaynak kimliğinde Al [Get-AzKeyVault](/powershell/module/az.keyvault/Get-AzKeyVault). Bu değişkenler ardından ile şifreleme işlemi başlatmak için kullanılan [kümesi AzVmssDiskEncryptionExtension](/powershell/module/az.compute/Set-AzVmssDiskEncryptionExtension):


```azurepowershell-interactive
$diskEncryptionKeyVaultUrl=(Get-AzKeyVault -ResourceGroupName $rgName -Name $vaultName).VaultUri
$keyVaultResourceId=(Get-AzKeyVault -ResourceGroupName $rgName -Name $vaultName).ResourceId

Set-AzVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $vmssName `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId –VolumeType "All"
```

İstendiğinde, yazın *y* disk şifreleme işlemi ölçek kümesi sanal Makinesinin devam etmek için örnekler.

### <a name="enable-encryption-using-kek-to-wrap-the-key"></a>Şifreleme anahtarı sarmalama için KEK kullanarak etkinleştirin

Ek güvenlik için sanal makine ölçek kümesi şifrelerken anahtar şifreleme anahtarı kullanabilirsiniz.

```azurepowershell-interactive
$diskEncryptionKeyVaultUrl=(Get-AzKeyVault -ResourceGroupName $rgName -Name $vaultName).VaultUri
$keyVaultResourceId=(Get-AzKeyVault -ResourceGroupName $rgName -Name $vaultName).ResourceId
$keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $vaultName -Name $keyEncryptionKeyName).Key.kid;

Set-AzVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $vmssName `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId `
    -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $keyVaultResourceId –VolumeType "All"
```

> [!NOTE]
>  Disk şifreleme keyvault parametresinin değeri söz diziminin tam tanımlayıcı dize şöyledir:</br>
/ subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br></br>
> Tam URI gibi KEK anahtar şifreleme anahtarı parametresinin değeri için sözdizimi aşağıdaki gibidir:</br>
https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id]

## <a name="check-encryption-progress"></a>Şifreleme ilerleme durumunu denetleme

Disk şifreleme durumunu denetlemek için kullanmak [Get-AzVmssDiskEncryption](/powershell/module/az.compute/Get-AzVmssDiskEncryption):


```azurepowershell-interactive
Get-AzVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $vmssName
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

Artık şifrelenmiş VM örnekleri diskleri kullanmak istiyorsanız, şifrelemeyi devre dışı bırakabilirsiniz [devre dışı bırak AzVmssDiskEncryption](/powershell/module/az.compute/Disable-AzVmssDiskEncryption) gibi:


```azurepowershell-interactive
Disable-AzVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $vmssName
```

## <a name="next-steps"></a>Sonraki adımlar

- Bu makalede, Azure PowerShell bir sanal makine ölçek kümesi şifrelemek için kullanılır. Ayrıca [Azure CLI](virtual-machine-scale-sets-encrypt-disks-cli.md) veya şablonları [Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox) veya [Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox).
- Sahip olmasını isterseniz, Azure Disk şifrelemesi uygulanan başka bir uzantı sağlandıktan sonra kullanabileceğiniz [uzantı sıralama](virtual-machine-scale-sets-extension-sequencing.md). Kullanabileceğiniz [bu örnekleri](../security/azure-security-disk-encryption-extension-sequencing.md#sample-azure-templates) kullanmaya başlamak için.
