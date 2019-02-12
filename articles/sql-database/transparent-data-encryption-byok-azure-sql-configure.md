---
title: "PowerShell ve CLI: Azure Key Vault ile SQL TDE'nin - enable - kendi anahtarınızı - Azure SQL veritabanı getirin | Microsoft Docs"
description: Bir Azure SQL veritabanı ve veri ambarı bekleyen şifreleme için saydam veri şifrelemesi (TDE) kullanmaya başlamak için yapılandırmayı öğrenin PowerShell veya CLI kullanarak.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aliceku
ms.author: aliceku
ms.reviewer: vanto
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: b6abfa24d790554de239171ffef1899cdc135d26
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2019
ms.locfileid: "55996262"
---
# <a name="powershell-and-cli-enable-transparent-data-encryption-with-customer-managed-key-from-azure-key-vault"></a>PowerShell ve CLI: Azure Key vault'tan müşteri tarafından yönetilen anahtarla saydam veri şifrelemesini etkinleştirme

Bu makalede Azure Key vault'tan bir anahtar için saydam veri şifrelemesi (TDE) bir SQL veritabanı veya veri ambarı kullanma konusunda yol göstermektedir. Azure Key Vault tümleştirmesi - Getir bilgisayarınızı kendi anahtarını (BYOK) desteği ile TDE hakkında daha fazla bilgi edinmek için [müşteri tarafından yönetilen anahtarları Azure Key vault'taki TDE](transparent-data-encryption-byok-azure-sql.md). 

## <a name="prerequisites-for-powershell"></a>PowerShell için Önkoşullar

- Bir Azure aboneliğiniz varsa ve söz konusu abonelik yönetici olmanız gerekir.
- [İsteğe bağlı ancak önerilen] Bir donanım güvenlik modülü (HSM) veya TDE koruyucusuna anahtar malzemesi yerel bir kopyasını oluşturmak için depolama yerel anahtar vardır.
- Azure PowerShell sürümü 4.2.0 olmalıdır veya üzerinin yüklü ve çalışıyor. 
- TDE için kullanılacak bir Azure Key Vault ve anahtarı oluşturun.
   - [Key vault'tan PowerShell yönergeleri](../key-vault/key-vault-get-started.md)
   - [Bir donanım güvenlik modülü (HSM) ve anahtar Kasası'nı kullanma yönergeleri](../key-vault/key-vault-get-started.md#HSM)
 - Anahtar kasası TDE için kullanılacak özelliğine sahip olmalıdır:
   - [Geçici silme](../key-vault/key-vault-ovw-soft-delete.md)
   - [Key Vault geçici silmeyi PowerShell ile kullanma](../key-vault/key-vault-soft-delete-powershell.md) 
- Anahtar TDE için kullanılacak aşağıdaki özniteliklere sahip olmanız gerekir:
   - Sona erme tarihi
   - Devre dışı değil
   - Şunları gerçekleştirmek *alma*, *anahtarı sarmalama*, *anahtarı kaydırma* işlemleri

## <a name="step-1-assign-an-azure-ad-identity-to-your-server"></a>1. Adım Bir Azure AD kimlik sunucunuza atama 

Mevcut bir sunucu varsa sunucunuza bir Azure AD kimlik eklemek için aşağıdakileri kullanın:

   ```powershell
   $server = Set-AzureRmSqlServer `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -AssignIdentity
   ```

Bir sunucu oluşturuyorsanız, kullanın [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) etiket cmdlet'iyle-kimlik sunucusu oluşturulurken bir Azure AD kimlik eklemek için:

   ```powershell
   $server = New-AzureRmSqlServer `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -Location <RegionName> `
   -ServerName <LogicalServerName> `
   -ServerVersion "12.0" `
   -SqlAdministratorCredentials <PSCredential> `
   -AssignIdentity 
   ```

## <a name="step-2-grant-key-vault-permissions-to-your-server"></a>2. Adım Key Vault sunucunuza izinler

Kullanım [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) cmdlet'ini sunucu erişim anahtarı kasaya bir anahtarından TDE için kullanmadan önce.

   ```powershell
   Set-AzureRmKeyVaultAccessPolicy  `
   -VaultName <KeyVaultName> `
   -ObjectId $server.Identity.PrincipalId `
   -PermissionsToKeys get, wrapKey, unwrapKey
   ```

## <a name="step-3-add-the-key-vault-key-to-the-server-and-set-the-tde-protector"></a>3. Adım Key Vault anahtarı sunucuya ekleyin ve sonucunda TDE koruyucusuna ayarlayın

- Kullanım [Ekle AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/add-azurermsqlserverkeyvaultkey) cmdlet'ini anahtar Key Vault'tan sunucuya ekleyin.
- Kullanım [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) cmdlet'i tüm sunucu kaynaklarını için TDE koruyucusu olarak anahtarını ayarlayın.
- Kullanım [Get-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/get-azurermsqlservertransparentdataencryptionprotector) cmdlet'ini TDE koruyucusuna beklendiği gibi yapılandırıldığını onaylayın.

> [!Note]
> Birleşik anahtar adını ve anahtar kasası adı için 94 karakter uzunluğunda olabilir.
> 

>[!Tip]
>Bir örnek Keyıd Key vault'tan: https://contosokeyvault.vault.azure.net/keys/Key1/1a1a2b2b3c3c4d4d5e5e6f6f7g7g8h8h
>

   ```powershell
   <# Add the key from Key Vault to the server #>
   Add-AzureRmSqlServerKeyVaultKey `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -KeyId <KeyVaultKeyId>

   <# Set the key as the TDE protector for all resources under the server #>
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> 

   <# To confirm that the TDE protector was configured as intended: #>
   Get-AzureRmSqlServerTransparentDataEncryptionProtector `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> 
   ```

## <a name="step-4-turn-on-tde"></a>4. Adım. TDE'yi etkinleştirmek 

Kullanım [kümesi AzureRMSqlDatabaseTransparentDataEncryption](/powershell/module/azurerm.sql/set-azurermsqldatabasetransparentdataencryption) TDE'yi etkinleştirmek için cmdlet'i.

   ```powershell
   Set-AzureRMSqlDatabaseTransparentDataEncryption `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -DatabaseName <DatabaseName> `
   -State "Enabled"
   ```

Artık veritabanı veya veri ambarını TDE şifreleme anahtarı anahtar Kasası'nda etkinleştirilen sahiptir.

## <a name="step-5-check-the-encryption-state-and-encryption-activity"></a>5. Adım. Şifreleme etkinlik ve şifreleme durumunu denetle

Kullanım [Get-AzureRMSqlDatabaseTransparentDataEncryption](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryption) şifreleme durumunu almak için ve [Get-AzureRMSqlDatabaseTransparentDataEncryptionActivity](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryptionactivity) şifreleme ilerleme durumunu denetlemek için bir veritabanını veya veri ambarı.

   ```powershell
   # Get the encryption state
   Get-AzureRMSqlDatabaseTransparentDataEncryption `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -DatabaseName <DatabaseName> `

   <# Check the encryption progress for a database or data warehouse #>
   Get-AzureRMSqlDatabaseTransparentDataEncryptionActivity `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -DatabaseName <DatabaseName>  
   ```

## <a name="other-useful-powershell-cmdlets"></a>Diğer kullanışlı PowerShell cmdlet'leri

- Kullanım [kümesi AzureRMSqlDatabaseTransparentDataEncryption](/powershell/module/azurerm.sql/set-azurermsqldatabasetransparentdataencryption) TDE'yi etkinleştirmek için cmdlet'i.

   ```powershell
   Set-AzureRMSqlDatabaseTransparentDataEncryption `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -DatabaseName <DatabaseName> `
   -State "Disabled”
   ```
 
- Kullanım [Get-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/get-azurermsqlserverkeyvaultkey) sunucuya eklenmiş Key Vault anahtar listesini döndürmek için cmdlet'i.

   ```powershell
   <# KeyId is an optional parameter, to return a specific key version #>
   Get-AzureRmSqlServerKeyVaultKey `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName>
   ```
 
- Kullanım [Remove-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/remove-azurermsqlserverkeyvaultkey) bir Key Vault anahtarı sunucudan kaldırmak için.

   ```powershell
   <# The key set as the TDE Protector cannot be removed. #>
   Remove-AzureRmSqlServerKeyVaultKey `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName>   
   ```
 
## <a name="troubleshooting"></a>Sorun giderme

Bir sorun oluşursa aşağıdakileri denetleyin:
- Anahtar kasası bulunamıyor, doğru aboneliği kullanarak işiniz emin olun [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription) cmdlet'i.

   ```powershell
   Get-AzureRmSubscription `
   -SubscriptionId <SubscriptionId>
   ```

- Yeni anahtar sunucuya eklenemez ya da yeni anahtarı TDE koruyucusu olarak güncelleştirilemiyor, aşağıdakileri denetleyin:
   - Anahtar sona erme tarihi sahip olmamalıdır
   - Anahtarı olmalıdır *alma*, *anahtarı sarmalama*, ve *anahtarı kaydırma* etkin işlemler.

## <a name="next-steps"></a>Sonraki adımlar

- Güvenlik gereksinimlerine uymak için TDE koruyucusu sunucusunun döndürme hakkında bilgi edinin: [Saydam veri şifrelemesi koruyucu PowerShell kullanarak döndürme](transparent-data-encryption-byok-azure-sql-key-rotation.md).
- Bir güvenlik riski olması durumunda, riskli olabilecek TDE koruyucusu kaldırma işlemleri gerçekleştirmeyi öğreneceksiniz: [Riskli olabilecek bir anahtarı Kaldır](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md). 

## <a name="prerequisites-for-cli"></a>CLI için Önkoşullar

- Bir Azure aboneliğiniz varsa ve söz konusu abonelik yönetici olmanız gerekir.
- [İsteğe bağlı ancak önerilen] Bir donanım güvenlik modülü (HSM) veya TDE koruyucusuna anahtar malzemesi yerel bir kopyasını oluşturmak için depolama yerel anahtar vardır.
- Komut satırı arabirimi 2.0 veya sonraki bir sürümü. En son sürümünü yükleyin ve Azure aboneliğinize bağlanmak için bkz: [yükleme ve yapılandırma Azure platformlar arası komut satırı arabirimi 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). 
- TDE için kullanılacak bir Azure Key Vault ve anahtarı oluşturun.
   - [CLI 2.0 kullanarak Key Vault yönetme](../key-vault/key-vault-manage-with-cli2.md)
   - [Bir donanım güvenlik modülü (HSM) ve anahtar Kasası'nı kullanma yönergeleri](../key-vault/key-vault-get-started.md#HSM)
 - Anahtar kasası TDE için kullanılacak özelliğine sahip olmalıdır:
   - [Geçici silme](../key-vault/key-vault-ovw-soft-delete.md)
   - [Key Vault geçici silmeyi CLI ile kullanma](../key-vault/key-vault-soft-delete-cli.md) 
- Anahtar TDE için kullanılacak aşağıdaki özniteliklere sahip olmanız gerekir:
   - Sona erme tarihi
   - Devre dışı değil
   - Şunları gerçekleştirmek *alma*, *anahtarı sarmalama*, *anahtarı kaydırma* işlemleri
   
## <a name="step-1-create-a-server-and-assign-an-azure-ad-identity-to-your-server"></a>1. Adım Bir sunucu oluşturmak ve bir Azure AD kimlik sunucunuza atama
      cli
      # create server (with identity) and database
      az sql server create -n "ServerName" -g "ResourceGroupName" -l "westus" -u "cloudsa" -p "YourFavoritePassWord99@34" -i 
      az sql db create -n "DatabaseName" -g "ResourceGroupName" -s "ServerName" 
      

 
## <a name="step-2-grant-key-vault-permissions-to-your-server"></a>2. Adım Key Vault sunucunuza izinler
      cli
      # create key vault, key and grant permission
      az keyvault create -n "VaultName" -g "ResourceGroupName" 
      az keyvault key create -n myKey -p software --vault-name "VaultName" 
      az keyvault set-policy -n "VaultName" --object-id "ServerIdentityObjectId" -g "ResourceGroupName" --key-permissions wrapKey unwrapKey get list 
      

 
## <a name="step-3-add-the-key-vault-key-to-the-server-and-set-the-tde-protector"></a>3. Adım Key Vault anahtarı sunucuya ekleyin ve sonucunda TDE koruyucusuna ayarlayın
  
     cli
     # add server key and update encryption protector
      az sql server key create -g "ResourceGroupName" -s "ServerName" -t "AzureKeyVault" -u "FullVersionedKeyUri 
      az sql server tde-key update -g "ResourceGroupName" -s "ServerName" -t AzureKeyVault -u "FullVersionedKeyUri" 
      
  
  > [!Note]
> Birleşik anahtar adını ve anahtar kasası adı için 94 karakter uzunluğunda olabilir.
> 

>[!Tip]
>Bir örnek Keyıd Key vault'tan: https://contosokeyvault.vault.azure.net/keys/Key1/1a1a2b2b3c3c4d4d5e5e6f6f7g7g8h8h
>
  
## <a name="step-4-turn-on-tde"></a>4. Adım. TDE'yi etkinleştirmek 
      cli
      # enable encryption
      az sql db tde create -n "DatabaseName" -g "ResourceGroupName" -s "ServerName" --status Enabled 
      

Artık veritabanı veya veri ambarını TDE şifreleme anahtarı anahtar Kasası'nda etkinleştirilen sahiptir.

## <a name="step-5-check-the-encryption-state-and-encryption-activity"></a>5. Adım. Şifreleme etkinlik ve şifreleme durumunu denetle

     cli
      # get encryption scan progress
      az sql db tde show-activity -n "DatabaseName" -g "ResourceGroupName" -s "ServerName" 

      # get whether encryption is on or off
      az sql db tde show-configuration -n "DatabaseName" -g "ResourceGroupName" -s "ServerName" 

## <a name="sql-cli-references"></a>SQL CLI başvuruları

https://docs.microsoft.com/cli/azure/sql?view=azure-cli-latest 

https://docs.microsoft.com/cli/azure/sql/server/key?view=azure-cli-latest 

https://docs.microsoft.com/cli/azure/sql/server/tde-key?view=azure-cli-latest 

https://docs.microsoft.com/cli/azure/sql/db/tde?view=azure-cli-latest 

