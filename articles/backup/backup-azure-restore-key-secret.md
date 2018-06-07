---
title: Anahtar kasası anahtarı ve gizli Azure Yedekleme kullanılarak şifrelenmiş VM'ler için geri yükleme
description: Anahtar kasası anahtarı ve gizli Azure PowerShell kullanarak yedekleme geri yüklemeyi öğrenin
services: backup
author: JPallavi
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 08/28/2017
ms.author: pajosh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b703b4511f9fefb48546b23feaa33ca7da34da1f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34606112"
---
# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a>Anahtar kasası anahtarı ve gizli Azure Yedekleme kullanılarak şifrelenmiş VM'ler için geri yükleme
Bu makalede, anahtarı ve gizli anahtar kasasına yoksa Azure VM Backup şifrelenmiş Azure VM'ler, geri yükleme gerçekleştirmek için kullanma hakkında alınmaktadır. Geri yüklenen VM için anahtar (anahtar şifreleme anahtarı) ve gizli (BitLocker şifreleme anahtarı) ayrı bir kopyasını korumak istiyorsanız, aşağıdaki adımları da kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar
* **Yedekleme şifrelenmiş VM'ler** - şifrelenmiş Azure VM'ler yedeklenmiş Azure Yedekleme'yi kullanarak. Makaleyi başvurmak [yönetmek yedekleme ve geri yükleme Azure PowerShell kullanarak VM'lerin](backup-azure-vms-automation.md) şifrelenmiş Azure sanal makineleri yedekleme hakkında ayrıntılar için.
* **Azure anahtar kasası yapılandırma** – için anahtarları ve gizli anahtarları gereken geri yüklenmesi, anahtar kasası zaten mevcut olduğundan emin olun. Makaleyi başvuran [Azure anahtar kasası ile çalışmaya başlama](../key-vault/key-vault-get-started.md) anahtar kasası yönetimi hakkında ayrıntılar için.
* **Diski geri** -şifrelenmiş VM kullanma diskleri geri yüklemek için geri yükleme işi tetiklemesi olun [PowerShell adımları](backup-azure-vms-automation.md#restore-an-azure-vm). Bu durum, bu iş, anahtarları ve gizli anahtarları geri yüklenmesi şifrelenmiş VM içeren depolama hesabınızdaki bir JSON dosyası oluşturur. çünkü.

## <a name="get-key-and-secret-from-azure-backup"></a>Azure yedeklemeden anahtarı ve gizli anahtarı alma

> [!NOTE]
> Disk şifrelenmiş VM için geri yüklendikten sonra emin olun:
> 1. $details bölümünde belirtildiği gibi geri yükleme disk iş ayrıntılarla doldurulmuş [PowerShell adımları geri yükleme disk bölümü](backup-azure-vms-automation.md#restore-an-azure-vm)
> 2. VM yalnızca geri yüklenen disklerden oluşturulmalıdır **anahtarı ve gizli geri sonra anahtar Kasası'na**.
>
>

İş ayrıntılarını geri yüklenen disk özelliklerini sorgu.

```
PS C:\> $properties = $details.properties
PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
PS C:\> $containerName = $properties["Config Blob Container Name"]
PS C:\> $encryptedBlobName = $properties["Encryption Info Blob Name"]
```

Azure depolama bağlamını ayarlayın ve şifrelenmiş VM için anahtarı ve gizli bilgi içeren JSON yapılandırma dosyası geri yükleme.

```
PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName '<rg-name>'
PS C:\> $destination_path = 'C:\vmencryption_config.json'
PS C:\> Get-AzureStorageBlobContent -Blob $encryptedBlobName -Container $containerName -Destination $destination_path
PS C:\> $encryptionObject = Get-Content -Path $destination_path  | ConvertFrom-Json
```

## <a name="restore-key"></a>Anahtarı geri yükle
JSON dosyasının yukarıda belirtilen hedef yolu oluşturulduktan sonra geri anahtarı (KEK) anahtar kasasını yerleştirmek için anahtar cmdlet geri yüklemek için akış ve JSON öğesinden anahtar blob dosyası oluşturur.

```
PS C:\> $keyDestination = 'C:\keyDetails.blob'
PS C:\> [io.file]::WriteAllBytes($keyDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyBackupData))
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile $keyDestination
```

## <a name="restore-secret"></a>Gizli anahtarı geri yükleme
Anahtar kasasına gizli anahtarı (BEK) geri almaya gizli cmdlet ayarlamak için gizli adını ve değerini almak ve bunu akış için yukarıda oluşturulan JSON dosyasını kullanın. **VM BEK ve KEK kullanılarak şifrelenir bu cmdlet'leri kullanın.**

```
PS C:\> $secretdata = $encryptionObject.OsDiskKeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = $encryptionObject.OsDiskKeyAndSecretDetails.KeyUrl;'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $Secret -ContentType  'Wrapped BEK' -Tags $Tags
```

VM ise **BEK yalnızca kullanılarak şifrelenmiş**, JSON öğesinden gizli blob dosyası oluşturma ve anahtar kasasına gizli (BEK) yerleştirilecek gizli cmdlet geri yüklemek için akış.

```
PS C:\> $secretDestination = 'C:\secret.blob'
PS C:\> [io.file]::WriteAllBytes($secretDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyVaultSecretBackupData))
PS C:\> Restore-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -InputFile $secretDestination -Verbose
```

> [!NOTE]
> 1. Değer $secretname elde edilebilir $encryptionObject.OsDiskKeyAndSecretDetails.SecretUrl çıkışına başvuran ve sonra gizli metin kullanarak / Örneğin çıkış gizli URL için https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 ve gizli anahtar adı B3284AAA-DAAA-4AAA-B393-60CAA848AAAA
> 2. Etiket DiskEncryptionKeyFileName gizli adıyla aynı değeridir.
>
>

## <a name="create-virtual-machine-from-restored-disk"></a>Geri yüklenen disk, sanal makine oluşturma
Azure VM Backup kullanılarak şifrelenmiş VM yedeklediyseniz, PowerShell cmdlet'leri anahtarı ve gizli geri anahtar Kasası'na geri Yardım bahsedilen. Bunları geri yükledikten sonra makaleyi başvuran [yönetmek yedekleme ve geri yükleme Azure PowerShell kullanarak VM'lerin](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) şifrelenmiş VM'ler geri yüklenen disk, anahtarı ve gizli anahtarı oluşturmak için.

## <a name="legacy-approach"></a>Eski yaklaşımı
Yukarıda belirtilen bir yaklaşım için tüm kurtarma noktalarının çalışır. Ancak, daha eski bir yaklaşım anahtarı ve gizli bilgileri kurtarma noktasından alma olacaktır 11 Temmuz 2017 BEK ve KEK kullanılarak şifrelenmiş VM'ler için daha eski kurtarma noktaları için geçerlidir. Geri Yükleme disk işi tamamlandığında VM şifrelenmiş kullanmak için [PowerShell adımları](backup-azure-vms-automation.md#restore-an-azure-vm), $rp geçerli bir değerle doldurulur emin olun.

### <a name="restore-key"></a>Anahtarı geri yükle
Kurtarma noktasından anahtarı (KEK) bilgilerini almak ve bunu geri anahtar kasasını yerleştirmek için anahtar cmdlet geri yüklemek için akış için aşağıdaki cmdlet'leri kullanın.

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation 'C:\Users\downloads'
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile 'C:\Users\downloads'
```

### <a name="restore-secret"></a>Gizli anahtarı geri yükleme
Kurtarma noktasından gizli (BEK) bilgilerini alamadı ve onu geri anahtar kasasını yerleştirmek için gizli cmdlet ayarlamak için akış için aşağıdaki cmdlet'leri kullanın.

```
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://mykeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  'Wrapped BEK'
```

> [!NOTE]
> 1. $Secretname için değer $rp1 çıkışına bakarak elde edilebilir. KeyAndSecretDetails.SecretUrl ve sonra gizli metni kullanarak örneğin URL çıkış gizli olduğu https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 ve gizli anahtar adı B3284AAA-DAAA-4AAA-B393-60CAA848AAAA
> 2. Etiket DiskEncryptionKeyFileName gizli adıyla aynı değeridir.
> 3. DiskEncryptionKeyEncryptionKeyURL değeri elde edilebilir anahtar Kasası'nı kullanarak ve geri anahtarları geri yükleniyor sonra [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx) cmdlet'i
>
>

## <a name="next-steps"></a>Sonraki adımlar
Anahtar Kasası'na anahtarı ve gizli geri geri yükledikten sonra makaleyi başvuran [yönetmek yedekleme ve geri yükleme Azure PowerShell kullanarak VM'lerin](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) şifrelenmiş VM'ler geri yüklenen disk, anahtarı ve gizli anahtarını oluşturmak için.
