---
title: Müşteri tarafından yönetilen anahtarlar powershell'den Azure depolama şifrelemesi için yapılandırma
description: PowerShell, Azure depolama şifrelemesi için müşteri tarafından yönetilen anahtarlar yapılandırmak için kullanmayı öğrenin. Müşteri tarafından yönetilen anahtarlar oluşturun, döndürme, devre dışı bırakın ve erişim denetimleri iptal olanak sağlar.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/16/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: be876b370cd476bee2af7d90a9f0433fd80de3b4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65233684"
---
# <a name="configure-customer-managed-keys-for-azure-storage-encryption-from-powershell"></a>Müşteri tarafından yönetilen anahtarlar powershell'den Azure depolama şifrelemesi için yapılandırma

[!INCLUDE [storage-encryption-configure-keys-include](../../../includes/storage-encryption-configure-keys-include.md)]

Bu makalede PowerShell kullanılarak müşteri tarafından yönetilen anahtarlarla anahtar kasası yapılandırma gösterilmektedir.

## <a name="assign-an-identity-to-the-storage-account"></a>Depolama hesabı için bir kimlik atama

Müşteri tarafından yönetilen anahtarları depolama hesabınız için etkinleştirmek için önce sistem tarafından atanan bir yönetilen kimlik depolama hesabına atayın. Bu yönetilen kimlik, anahtar kasasına erişmek için depolama hesabı izinleri vermek için kullanın.

PowerShell kullanarak yönetilen bir kimlik atanacak çağrı [kümesi AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount). Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```powershell
$storageAccount = Set-AzStorageAccount -ResourceGroupName <resource_group> `
    -Name <storage-account> `
    -AssignIdentity
```

Sistem tarafından atanan yönetilen kimlikleri PowerShell ile yapılandırma hakkında daha fazla bilgi için bkz. [yapılandırma kimliklerini Azure VM'de PowerShell kullanarak Azure kaynakları için yönetilen](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md).

## <a name="create-a-new-key-vault"></a>Yeni key vault oluşturma

PowerShell kullanarak yeni bir anahtar kasası oluşturmak için arama [yeni AzKeyVault](/powershell/module/az.keyvault/new-azkeyvault). Azure depolama şifrelemesi etkin, iki anahtar koruma ayarı bulunmalıdır, müşteri tarafından yönetilen anahtarları depolamak için kullandığınız bir anahtar kasası **geçici silme** ve **yapmak değil Temizleme**. 

Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın. 

```powershell
$keyVault = New-AzKeyVault -Name <key-vault> `
    -ResourceGroupName <resource_group> `
    -Location <location> `
    -EnableSoftDelete `
    -EnablePurgeProtection
```

## <a name="configure-the-key-vault-access-policy"></a>Anahtar kasası erişim ilkesini yapılandırma

Ardından, depolama hesabına erişmek için izinlere sahip olacak şekilde anahtar kasası için erişim ilkesi yapılandırın. Bu adımda, önceden atanmış depolama hesabına yönetilen bir kimlik kullanmanız gerekir.

Anahtar kasası erişim ilkesini ayarlamak için çağrı [kümesi AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy). Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirin ve önceki örneklerde tanımlanan değişkenleri kullanmayı unutmayın.

```powershell
Set-AzKeyVaultAccessPolicy `
    -VaultName $keyVault.VaultName `
    -ObjectId $storageAccount.Identity.PrincipalId `
    -PermissionsToKeys wrapkey,unwrapkey,get,recover
```

## <a name="create-a-new-key"></a>Yeni anahtar oluştur

Ardından, anahtar Kasası'nda yeni bir anahtar oluşturun. Yeni bir anahtar oluşturmak için arama [Ekle AzKeyVaultKey](/powershell/module/az.keyvault/add-azkeyvaultkey). Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirin ve önceki örneklerde tanımlanan değişkenleri kullanmayı unutmayın.

```powershell
$key = Add-AzKeyVaultKey -VaultName $keyVault.VaultName -Name <key> -Destination 'Software'
```

## <a name="configure-encryption-with-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlarla şifreleme yapılandırma

Varsayılan olarak, Microsoft tarafından yönetilen anahtarlar Azure depolama şifrelemesi kullanır. Bu adımda, müşteri tarafından yönetilen anahtarlar kullanın ve depolama hesabıyla ilişkilendirmek için anahtarı belirtmek için Azure depolama hesabınızı yapılandırın.

Çağrı [kümesi AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) depolama hesabının şifrelemesini güncelleştirilecek. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirin ve önceki örneklerde tanımlanan değişkenleri kullanmayı unutmayın.

```powershell
Set-AzStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName `
    -AccountName $storageAccount.StorageAccountName `
    -KeyvaultEncryption `
    -KeyName $key.Name `
    -KeyVersion $key.Version `
    -KeyVaultUri $keyVault.VaultUri
```

## <a name="update-the-key-version"></a>Anahtar sürümü güncelleştir

Bir anahtarın yeni bir sürümünü oluşturduğunuzda, yeni sürümü kullanmak için depolama hesabının güncelleştirilmesi gerekir. İlk olarak, çağrı [Get-AzKeyVaultKey](/powershell/module/az.keyvault/get-azkeyvaultkey) anahtar en son sürümünü almak için. Ardından çağırın [kümesi AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) önceki bölümde gösterildiği gibi anahtarın yeni sürümü kullanmak için depolama hesabının şifreleme ayarlarını güncelleştirmek için.

## <a name="next-steps"></a>Sonraki adımlar

- [Bekleyen veri için Azure depolama şifrelemesi](storage-service-encryption.md) 
- [Azure anahtar kasası nedir](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)?
