---
title: Azure Backup kullanarak şifreli VM'ler için Key Vault anahtarını ve gizli anahtarı geri yükleme
description: Key Vault anahtar ve gizli dizi PowerShell kullanarak Azure Backup, geri yüklemeyi öğreneceksiniz
services: backup
author: geetha
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 08/28/2017
ms.author: geetha
ms.openlocfilehash: 13eb800cd64e0de736b1fdea308a03d8a8d0f046
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66127898"
---
# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a>Azure Backup kullanarak şifreli VM'ler için Key Vault anahtarını ve gizli anahtarı geri yükleme

Bu makalede, anahtar ve gizli anahtar kasasında mevcut değilse Azure VM yedeklemesi şifrelenmiş Azure Vm'lerini geri yükleme gerçekleştirmek için kullanma hakkında konuşuyor. Geri yüklenen VM için ayrı bir anahtar (anahtar şifreleme anahtarı) ve gizli dizi (BitLocker şifreleme anahtarı) kopyasını tutmak istiyorsanız şu adımları da kullanılabilir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

* **Şifrelenmiş Vm'leri yedekleme** - şifrelenmiş Azure Vm'lerini yedeklenir Azure Backup kullanarak. Makaleye bakın [yedekleme ve geri yükleme, Azure PowerShell kullanarak Vm'leri yönetme](backup-azure-vms-automation.md) şifrelenmiş Azure Vm'lerini yedekleme hakkında ayrıntılı bilgi için.
* **Azure Key Vault yapılandırma** – için anahtarları ve gizli anahtarları gereken geri yüklenmesi, anahtar kasası zaten mevcut olduğundan emin olun. Makaleye bakın [Azure anahtar kasası ile çalışmaya başlama](../key-vault/key-vault-get-started.md) anahtar kasası yönetimi hakkındaki ayrıntılar için.
* **Diski geri yükleme** -VM şifreli kullanmak için diskleri geri yüklemek için geri yükleme işi tetiklemesi olun [PowerShell adımları](backup-azure-vms-automation.md#restore-an-azure-vm). Bu iş, depolama hesabınızda anahtarlar ve gizli diziler için geri yüklenmesi şifrelenmiş sanal Makineyi içeren bir JSON dosyası oluşturur. olmasıdır.

## <a name="get-key-and-secret-from-azure-backup"></a>Azure yedeklemeden anahtar ve gizli dizi alma

> [!NOTE]
> Şifrelenmiş sanal makine için disk geri yüklendikten sonra emin olun:
> * $details belirtildiği gibi geri yükleme disk iş ayrıntıları ile doldurulmuş [PowerShell diskleri bölümüne geri yükleme adımları](backup-azure-vms-automation.md#restore-an-azure-vm)
> * VM yalnızca geri yüklenen disklerden oluşturulması **anahtar ve gizli dizi geri sonra anahtar kasasına**.

İş ayrıntılarını geri yüklenen diski özelliklerini sorgulayın.

```powershell
$properties = $details.properties
$storageAccountName = $properties["Target Storage Account Name"]
$containerName = $properties["Config Blob Container Name"]
$encryptedBlobName = $properties["Encryption Info Blob Name"]
```

Azure depolama bağlamını ayarlayın ve şifrelenmiş VM için anahtar ve gizli ayrıntılarını içeren JSON yapılandırma dosyası geri yükleyin.

```powershell
Set-AzCurrentStorageAccount -Name $storageaccountname -ResourceGroupName '<rg-name>'
$destination_path = 'C:\vmencryption_config.json'
Get-AzStorageBlobContent -Blob $encryptedBlobName -Container $containerName -Destination $destination_path
$encryptionObject = Get-Content -Path $destination_path  | ConvertFrom-Json
```

## <a name="restore-key"></a>Anahtarı geri yükle

Yukarıda belirtilen hedef yolu bir JSON dosyası oluşturulduktan sonra JSON'dan anahtar blob dosyası oluşturmak ve geri anahtarı (KEK) anahtar kasasını yerleştirmek için anahtar cmdlet'i geri yüklemek için akış.

```powershell
$keyDestination = 'C:\keyDetails.blob'
[io.file]::WriteAllBytes($keyDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyBackupData))
Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile $keyDestination
```

## <a name="restore-secret"></a>Gizli anahtarı geri yükleme

Gizli anahtar kasasında gizli dizi (BEK) geri put cmdlet'ini ayarlamak için yukarıdaki'gizli dizi adı ve değeri alın ve bu akışı oluşturulan JSON dosyası kullanın. Bu cmdlet'leri kullanın, **VM BEK ve KEK kullanılarak şifrelenir**.

**Windows VM'nizi BEK ve KEK kullanılarak şifrelendiyse, bu cmdlet'leri kullanın.**

```powershell
$secretdata = $encryptionObject.OsDiskKeyAndSecretDetails.SecretData
$Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
$secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
$Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = $encryptionObject.OsDiskKeyAndSecretDetails.KeyUrl;'MachineName' = 'vm-name'}
Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $Secret -ContentType  'Wrapped BEK' -Tags $Tags
```

**BEK ve KEK kullanarak Linux VM şifreli değilse bu cmdlet'leri kullanın.**

```powershell
$secretdata = $encryptionObject.OsDiskKeyAndSecretDetails.SecretData
$Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
$secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
$Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'LinuxPassPhraseFileName';'DiskEncryptionKeyEncryptionKeyURL' = $encryptionObject.OsDiskKeyAndSecretDetails.KeyUrl;'MachineName' = 'vm-name'}
Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $Secret -ContentType  'Wrapped BEK' -Tags $Tags
```

Gizli anahtar kasasında gizli dizi (BEK) geri put cmdlet'ini ayarlamak için yukarıdaki'gizli dizi adı ve değeri alın ve bu akışı oluşturulan JSON dosyası kullanın. Bu cmdlet'leri kullanın, **VM BEK kullanılarak şifrelenir** yalnızca.

```powershell
$secretDestination = 'C:\secret.blob'
[io.file]::WriteAllBytes($secretDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyVaultSecretBackupData))
Restore-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -InputFile $secretDestination -Verbose
  ```

> [!NOTE]
> * Değer $secretname $encryptionObject.OsDiskKeyAndSecretDetails.SecretUrl çıkışına başvuran ve sonra gizli metin kullanılarak edinilebilir / örn çıkış gizli URL'si için https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 ve gizli dizi adı B3284AAA-DAAA-4AAA-B393-60CAA848AAAA
> * Etiket DiskEncryptionKeyFileName gizli dizi adı aynı değeri.
>
>

## <a name="create-virtual-machine-from-restored-disk"></a>Geri yüklenen diskten sanal makine oluşturma

Azure VM Backup kullanarak şifreli VM'yi yedeklediyseniz, PowerShell cmdlet'leri, anahtar ve gizli arka anahtar Kasası'na geri Yardım bahsedilen. Bunları geri yükledikten sonra makaleyi başvuran [yedekleme ve geri yükleme, Azure PowerShell kullanarak Vm'leri yönetme](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) şifrelenmiş Vm'leri geri yüklenen diski, anahtar ve gizli dizi oluşturmak için.

## <a name="legacy-approach"></a>Eski bir yaklaşım

Yukarıda belirtilen bir yaklaşım için tüm kurtarma noktalarını işe yarar. Ancak, anahtar ve gizli bilgileri kurtarma noktasından alma daha eski bir yaklaşım olacaktır BEK ve KEK kullanılarak şifrelenmiş VM'ler için 11 Temmuz 2017'den daha eski kurtarma noktaları için geçerli. VM şifreli kullanmak için geri yükleme disk işi tamamlandıktan sonra [PowerShell adımları](backup-azure-vms-automation.md#restore-an-azure-vm), geçerli bir değerle $rp doldurulduğundan emin olun.

### <a name="restore-key"></a>Anahtarı geri yükle

Kurtarma noktasından anahtarı (KEK) bilgilerini alın ve akışı anahtar kasasında koymak için anahtar cmdlet'i geri yüklemek için aşağıdaki cmdlet'leri kullanın.

```powershell
$rp1 = Get-AzRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation 'C:\Users\downloads'
Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile 'C:\Users\downloads'
```

### <a name="restore-secret"></a>Gizli anahtarı geri yükleme

Kurtarma noktasından gizli (BEK) bilgi almak ve ayarlamak anahtar kasasında koymak için gizli cmdlet'i için akış için aşağıdaki cmdlet'leri kullanın.

```powershell
$secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
$secretdata = $rp1.KeyAndSecretDetails.SecretData
$Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
$Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://mykeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  'Wrapped BEK'
```

> [!NOTE]
> * $Secretname için değer $rp1 çıkışına başvurarak elde edilebilir. KeyAndSecretDetails.SecretUrl ve sonra gizli metin kullanarak / çıkış parolası URL'si örn, https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 ve gizli dizi adı B3284AAA-DAAA-4AAA-B393-60CAA848AAAA
> * Etiket DiskEncryptionKeyFileName gizli dizi adı aynı değeri.
> * DiskEncryptionKeyEncryptionKeyURL değeri elde edilebilir anahtar kasasından anahtarlar geri geri yükleme ve kullanma sonra [Get-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/get-azurekeyvaultkey) cmdlet'i
>
>

## <a name="next-steps"></a>Sonraki adımlar

Anahtar ve gizli arka anahtar Kasası'na geri yükledikten sonra makaleyi başvuran [yedekleme ve geri yükleme, Azure PowerShell kullanarak Vm'leri yönetme](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) şifrelenmiş Vm'leri geri yüklenen diski, anahtar ve gizli dizi oluşturmak için.
