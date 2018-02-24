---
title: "Azure sanal makine ölçek kümeleri şifrelemek diskleri | Microsoft Docs"
description: "Sanal makine ölçek kümeleri bağlı disklerin şifrelemek öğrenin."
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
ms.date: 01/26/2018
ms.author: iainfou
ms.openlocfilehash: dddcece9f7566961b256369330661e5dbd5d4665
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="encrypt-os-and-attached-data-disks-in-a-virtual-machine-scale-set"></a>İşletim sistemi ve sanal makine ölçek kümesindeki eklenen veri disklerini şifrele
Azure [sanal makine ölçek kümeleri](/azure/virtual-machine-scale-sets/) Azure disk şifrelemesi (ADE) destekler.  Windows için Azure disk şifrelemesi etkinleştirilebilir ve endüstri standart şifreleme teknolojisi kullanılarak bekleme sırasında verileri korumak ve ölçek korumak için sanal makine ölçek ayarlar Linux ayarlar. Daha fazla bilgi için Azure Disk şifrelemesi Windows ve Linux sanal makineleri okuyun.

> [!NOTE]
>  Sanal makine ölçek kümeleri için Azure disk şifrelemesi genel Önizleme, tüm Azure ortak bölgelerde kullanılabilir şu anda kullanılıyor.

Azure disk şifrelemesi desteklenir:
- Ölçek kümeleri yönetilen diskler ile oluşturulan ve yerel (veya yönetilmeyen) disk ölçek kümeleri için desteklenmiyor.
- işletim sistemi ve veri birimleri için Windows ölçek kümeleri. Devre dışı şifrelemesi, işletim sistemi ve veri birimleri Windows ölçek kümeleri için desteklenir.
- Linux ölçek kümeleri veri birimleri için. İşletim sistemi disk şifrelemesi Linux ölçek kümeleri için geçerli Önizleme'de desteklenmez.

Ölçek kümesi VM yeniden görüntü oluşturma ve yükseltme işlemleri geçerli Önizleme'de desteklenmez. Sanal makine ölçek kümeleri Önizleme için Azure disk şifrelemesi yalnızca sınama ortamlarında önerilir. Önizleme'de, disk şifreleme yere bir şifrelenmiş ölçek kümesindeki bir işletim sistemi görüntüsüne yükseltmeniz gerekebilir üretim ortamlarında etkinleştirmeyin.

## <a name="prerequisites"></a>Önkoşullar
En son sürümlerini yüklemek [Azure Powershell](https://github.com/Azure/azure-powershell/releases), şifreleme komutları içerir.

Sanal makine ölçek kümeleri Önizleme için Azure disk şifrelemesi aşağıdaki PowerShell komutlarını kullanarak aboneliğinizi otomatik olarak kaydetmek gerektirir: 

```powershell
Login-AzureRmAccount
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```

'Kayıtlı' durumu aşağıdaki komutu tarafından döndürülen kadar yaklaşık 10 dakika bekleyin: 

```powershell
Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```

## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>Disk şifrelemesi için etkin bir Azure anahtar kasası oluşturma
Yeni bir anahtar Kasası 'EnabledForDiskEncryption' erişim ilkesi ayarlayabilirsiniz ve ölçek aynı abonelikte ve bölgede oluşturun.

```powershell
$rgname="windatadiskencryptiontest"
$VaultName="encryptionvault321"

New-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $rgName -Location southcentralus -EnabledForDiskEncryption
``` 

Ya da disk şifrelemesi için ayarlama ölçek olarak aynı abonelikte ve bölgede var olan bir anahtar kasasını etkinleştirin.

```powershell
$VaultName="encryptionvault321"
Set-AzureRmKeyVaultAccessPolicy -VaultName $VaultName -EnabledForDiskEncryption
```

## <a name="enable-encryption"></a>Şifrelemeyi etkinleştir
Aşağıdaki komutlar bir veri diski aynı kaynak grubunda bir anahtar kasası kullanılarak ayarlanan bir çalışan ölçeğinde şifreleyin. Çalışan bir diskleri şifrelemek için şablonları kullanabilirsiniz [Windows ölçek kümesi](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox) veya [Linux ölçek kümesini](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox).

```powershell
$rgname="windatadiskencryptiontest"
$VmssName="nt1vm"
$DiskEncryptionKeyVaultUrl="https://encryptionvault321.vault.azure.net"
$KeyVaultResourceId="/subscriptions/0754ecc2-d80d-426a-902c-b83f4cfbdc95/resourceGroups/windatadiskencryptiontest/providers/Microsoft.KeyVault/vaults/encryptionvault321"

Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $VmssName `
    -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId –VolumeType Data
```

## <a name="check-encryption-progress"></a>Şifreleme ilerleme durumunu denetleme
Ölçek kümesini şifreleme durumunu göstermek için aşağıdaki komutları kullanın.

```powershell
$rgname="windatadiskencryptiontest"
$VmssName="nt1vm"
Get-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName

Get-AzureRmVmssVMDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName -InstanceId "4"
```

## <a name="disable-encryption"></a>Şifreleme devre dışı bırak
Aşağıdaki komutları kullanarak çalışan bir sanal makine ölçek şifreleme devre dışı bırakın. Çalışan bir şifreleme devre dışı bırakmak için şablonları kullanabilirsiniz [Windows ölçek kümesi](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-windows) veya [Linux ölçek kümesini](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-linux).

```powershell
$rgname="windatadiskencryptiontest"
$VmssName="nt1vm"
Disable-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName
```
