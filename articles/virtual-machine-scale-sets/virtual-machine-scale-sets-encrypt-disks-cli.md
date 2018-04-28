---
title: Azure sanal makine ölçek kümeleri şifrelemek diskleri | Microsoft Docs
description: Sanal makine ölçek kümeleri bağlı disklerin şifrelemek öğrenin.
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: iainfou
ms.openlocfilehash: 570764ad5d657a8b1efa2425423a89ddc518451c
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
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

Uçtan uca toplu iş dosyası örneği Linux ölçek kümesi veri disk şifrelemesi için bulunabilir [burada](https://gist.githubusercontent.com/ejarvi/7766dad1475d5f7078544ffbb449f29b/raw/03e5d990b798f62cf188706221ba6c0c7c2efb3f/enable-linux-vmss.bat).  Bu örnek, bir kaynak grubu, Linux ölçek kümesi oluşturur, 5 GB veri diski bağlar ve sanal makine ölçek kümesi şifreler.

## <a name="prerequisites"></a>Önkoşullar
En son sürümlerini yüklemek [Azure Powershell](https://github.com/Azure/azure-powershell/releases) veya [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), şifreleme komutları içerir.

Sanal makine ölçek kümeleri Önizleme için Azure disk şifrelemesi aşağıdaki PowerShell komutlarını kullanarak aboneliğinizi otomatik olarak kaydetmek gerektirir: 

```powershell
Connect-AzureRmAccount
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```

'Kayıtlı' durumu aşağıdaki komutu tarafından döndürülen kadar yaklaşık 10 dakika bekleyin: 

```powershell
Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```

## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>Disk şifrelemesi için etkin bir Azure anahtar kasası oluşturma
Yeni bir anahtar Kasası 'EnabledForDiskEncryption' erişim ilkesi ayarlayabilirsiniz ve ölçek aynı abonelikte ve bölgede oluşturun.

```azurecli
rgname="linuxdatadiskencryptiontest"
VaultName="encryptionvault321"

az keyvault create --name $VaultName --resource-group $rgname --enabled-for-disk-encryption
```

Ya da disk şifrelemesi için ayarlama ölçek olarak aynı abonelikte ve bölgede var olan bir anahtar kasasını etkinleştirin.

```azurecli
VaultName="encryptionvault321"
az keyvault update --name $VaultName --enabled-for-disk-encryption
```

## <a name="enable-encryption"></a>Şifrelemeyi etkinleştir

Aşağıdaki komutlar bir veri diski aynı kaynak grubunda bir anahtar kasası kullanılarak ayarlanan bir çalışan ölçeğinde şifreleyin. Çalışan bir diskleri şifrelemek için şablonları kullanabilirsiniz [Windows ölçek kümesi](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox) veya [Linux ölçek kümesini](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox).

```azurecli
ResourceGroup="linuxdatadiskencryptiontest"
VmssName="nt1vm"
EncryptionKeyVaultUrl="https://encryptionvaultlinuxsf.vault.azure.net"
VaultResourceId="/subscriptions/0754ecc2-d80d-426a-902c-b83f4cfbdc95/resourceGroups/linuxdatadiskencryptiontest/providers/Microsoft.KeyVault/vaults/encryptionvaultlinuxsf"

az vmss encryption enable -g $ResourceGroup -n $VmssName --disk-encryption-keyvault $VaultResourceId --volume-type DATA
az vmss update-instances -g $ResourceGroup -n $VmssName --instance-ids *
```

## <a name="check-encryption-progress"></a>Şifreleme ilerleme durumunu denetleme

Ölçek kümesini şifreleme durumunu göstermek için aşağıdaki komutları kullanın.

```azurecli
ResourceGroup="linuxdatadiskencryptiontest"
VmssName="nt1vm"

az vmss encryption show -g $ResourceGroup -n $VmssName
```

## <a name="disable-encryption"></a>Şifreleme devre dışı bırak
Aşağıdaki komutları kullanarak çalışan bir sanal makine ölçek şifreleme devre dışı bırakın. Çalışan bir şifreleme devre dışı bırakmak için şablonları kullanabilirsiniz [Windows VM ölçek kümesi](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-windows) veya [Linux VM ölçek kümesi](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-linux).

```azurecli
ResourceGroup="linuxdatadiskencryptiontest"
VmssName="nt1vm"

az vmss encryption disable -g $ResourceGroup -n $VmssName
```

