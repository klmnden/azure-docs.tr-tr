---
title: "Azure sanal makine ölçek ayarlar disk şifrelemesi | Microsoft Docs"
description: "VM örnekleri ve sanal makine ölçek kümeleri bağlı disklerin şifrelemek için Azure PowerShell kullanmayı öğrenin"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/09/2018
ms.author: iainfou
ms.openlocfilehash: 856d4bc7dd636b3a2f3d072a10989cafd7efd6a6
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="encrypt-os-and-attached-data-disks-in-a-virtual-machine-scale-set"></a>İşletim sistemi ve sanal makine ölçek kümesindeki eklenen veri disklerini şifrele
Korumak ve rest endüstri standart şifreleme teknolojisi kullanarak verileri korumak için Azure Disk şifrelemesi (ADE) sanal makine ölçek kümesini destekler. Şifreleme, Windows ve Linux sanal makine için etkin olması ölçek kümeleri. Daha fazla bilgi için bkz: [için Azure Disk şifrelemesi Windows ve Linux](../security/azure-security-disk-encryption.md).

> [!NOTE]
>  Sanal makine ölçek kümeleri için Azure Disk şifrelemesi Önizleme, tüm Azure ortak bölgelerde kullanılabilir şu anda kullanılıyor. 
>
> Ölçek kümesi VM yeniden görüntü oluşturma ve yükseltme işlemleri geçerli Önizleme'de desteklenmez. Önizleme'de, Ölçek kümesi şifreleme yalnızca sınama ortamlarında önerilir. Önizleme'de, disk şifreleme yere bir işletim sistemi görüntüsüne yükseltmeniz gerekebilir üretim ortamlarında etkinleştirmeyin.

Azure disk şifrelemesi desteklenir:
- Ölçek kümeleri yönetilen diskler ile oluşturulan ve yerel (veya yönetilmeyen) disk ölçek kümeleri için desteklenmiyor.
- işletim sistemi ve veri birimleri için Windows ölçek kümeleri. Devre dışı şifrelemesi, işletim sistemi ve veri birimleri Windows ölçek kümeleri için desteklenir.
- Linux ölçek kümeleri veri birimleri için. İşletim sistemi disk şifrelemesi Linux ölçek kümeleri için geçerli Önizleme'de desteklenmez.


## <a name="prerequisites"></a>Önkoşullar
Bu makale Azure PowerShell modülü sürümü 5.3.0 gerektirir veya sonraki bir sürümü. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps).

Sanal makine ölçek için disk şifrelemesi'nin önizlemesi için Azure subsription ayarlar ile kayıt [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature): 

```powershell
Login-AzureRmAccount
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```

Yaklaşık 10 dakika kadar bekleyin *kayıtlı* durumu tarafından döndürülen [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature), ardından yeniden kaydettirin `Microsoft.Compute` sağlayıcısı: 

```powershell
Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```


## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>Disk şifrelemesi için etkin bir Azure anahtar kasası oluşturma
Azure anahtar kasası anahtarları, gizli ya da, uygulama ve hizmetlerinize güvenli bir şekilde uygulamak izin parolaları depolayabilirsiniz. Şifreleme anahtarları yazılım koruması'nı kullanarak Azure anahtar kasası içinde depolanır veya içe aktarmak ya da anahtarlarınızı FIPS 140-2 Düzey 2 standartları sertifikalı donanım güvenlik modüllerinde (HSM'ler) oluşturur. Bu şifreleme anahtarlarını şifrelemek ve şifresini çözmek, VM'ye bağlı sanal diskler için kullanılır. Bu şifreleme anahtarları denetimin korumak ve kullanımlarını denetleyebilirsiniz.

Bir anahtar kasası ile oluşturma [AzureRmKeyVault yeni](/powershell/module/azurerm.keyvault/new-azurermkeyvault). Disk şifrelemesi için kullanılacak anahtar kasası izin verecek şekilde ayarlamanız *EnabledForDiskEncryption* parametresi. Aşağıdaki örnek, ayrıca kaynak grubu adı, anahtar kasasının adı ve konumu için değişkenleri tanımlar. Kendi benzersiz anahtar kasası adı girin:

```powershell
$rgName="myResourceGroup"
$vaultName="myuniquekeyvault"
$location = "EastUS"

New-AzureRmResourceGroup -Name $rgName -Location $location
New-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $rgName -Location $location -EnabledForDiskEncryption
```


### <a name="use-an-existing-key-vault"></a>Var olan bir anahtar kasası kullanın
Bu adım yalnızca olan disk şifrelemesi ile kullanmak istediğiniz bir anahtar kasası varsa gerekli. Bir anahtar kasası önceki bölümde oluşturduysanız, bu adımı atlayın.

Disk şifrelemesi ile ayarlamak ölçek olarak aynı abonelikte ve bölgede var olan bir anahtar kasasına etkinleştirebilirsiniz [kümesi AzureRmKeyVaultAccessPolicy](/powershell/module/AzureRM.KeyVault/Set-AzureRmKeyVaultAccessPolicy). Varolan anahtar kasanızı adını tanımlamak *$vaultName* şekilde değişkeni:

```powershell
$vaultName="myexistingkeyvault"
Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -EnabledForDiskEncryption
```


## <a name="create-a-scale-set"></a>Ölçek kümesi oluşturma
İlk olarak, bir yönetici kullanıcı adı ve parola ile VM örnekleri için ayarlanmış [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

Bu adımda [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss) ile bir sanal makine ölçek kümesi oluşturun. Tekil VM örnekleriyle trafiğini dağıtmak için bir yük dengeleyici da oluşturulur. Yük Dengeleyici olarak, TCP bağlantı noktası 80 üzerinde trafiği dağıtmak için TCP bağlantı noktası 3389 üzerinde Uzak Masaüstü trafik ve PowerShell uzaktan iletişimini TCP bağlantı noktası 5985 izin kurallarını içerir:

```powershell
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
Ölçek kümesindeki VM örnekleri şifrelemek için ilk anahtarı kasa URI'sini ve kaynak kimliği ile ilgili bazı bilgi edinmek [Get-AzureRmKeyVault](/powershell/module/AzureRM.KeyVault/Get-AzureRmKeyVault). Bu değişkenleri ile şifreleme işlemini başlatmak için kullanılan [kümesi AzureRmVmssDiskEncryptionExtension](/powershell/module/AzureRM.Compute/Set-AzureRmVmssDiskEncryptionExtension):

```powershell
$diskEncryptionKeyVaultUrl=(Get-AzureRmKeyVault -ResourceGroupName $rgName -Name $vaultName).VaultUri
$keyVaultResourceId=(Get-AzureRmKeyVault -ResourceGroupName $rgName -Name $vaultName).ResourceId

Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $vmssName `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId –VolumeType "All"
```

İstendiğinde yazın *y* ölçekte disk şifreleme işlem kümesi VM devam etmek için örnekleri.


## <a name="check-encryption-progress"></a>Şifreleme ilerleme durumunu denetleme
Disk şifrelemesi durumunu denetlemek için kullanın [Get-AzureRmVmssDiskEncryption](/powershell/module/AzureRM.Compute/Get-AzureRmVmssDiskEncryption):

```powershell
Get-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $vmssName
```

VM örnekleri şifreli olduğunda *EncryptionSummary* kod raporları *ProvisioningState ve başarılı* aşağıdaki örnek çıktıda görüldüğü gibi:

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


## <a name="disable-encryption"></a>Şifreleme devre dışı bırak
Artık şifrelenmiş VM örnekleri diskleri kullanmak istiyorsanız, şifreleme ile devre dışı bırakabilirsiniz [devre dışı bırakma AzureRmVmssDiskEncryption](/powershell/module/AzureRM.Compute/Disable-AzureRmVmssDiskEncryption) gibi:

```powershell
Disable-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $vmssName
```


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure PowerShell bir sanal makine ölçek kümesi şifrelemek için kullanılır. Aynı zamanda [Azure CLI 2.0](virtual-machine-scale-sets-encrypt-disks-cli.md) için ya da şablon [Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox) veya [Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox).
