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
ms.date: 03/12/2019
ms.openlocfilehash: c42c6175512105de38a29be260c370851e152137
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60330882"
---
# <a name="powershell-and-cli-enable-transparent-data-encryption-with-customer-managed-key-from-azure-key-vault"></a>PowerShell ve CLI: Azure Key vault'tan müşteri tarafından yönetilen anahtarla saydam veri şifrelemesini etkinleştirme

Bu makalede Azure Key vault'tan bir anahtar için saydam veri şifrelemesi (TDE) bir SQL veritabanı veya veri ambarı kullanma konusunda yol göstermektedir. Azure Key Vault tümleştirmesi - Getir bilgisayarınızı kendi anahtarını (BYOK) desteği ile TDE hakkında daha fazla bilgi edinmek için [müşteri tarafından yönetilen anahtarları Azure Key vault'taki TDE](transparent-data-encryption-byok-azure-sql.md). 

## <a name="prerequisites-for-powershell"></a>PowerShell için Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure Resource Manager PowerShell modülü, Azure SQL veritabanı tarafından hala desteklenmektedir, ancak tüm gelecekteki geliştirme için Az.Sql modüldür. Bu cmdlet'ler için bkz. [Azurerm.SQL'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az modül ve AzureRm modülleri komutları için bağımsız değişkenler büyük ölçüde aynıdır.

- Bir Azure aboneliğiniz varsa ve söz konusu abonelik yönetici olmanız gerekir.
- [İsteğe bağlı ancak önerilen] Bir donanım güvenlik modülü (HSM) veya TDE koruyucusuna anahtar malzemesi yerel bir kopyasını oluşturmak için depolama yerel anahtar vardır.
- Azure PowerShell yüklenmiş ve çalışıyor olması gerekir. 
- TDE için kullanılacak bir Azure Key Vault ve anahtarı oluşturun.
  - [Key vault'tan PowerShell yönergeleri](../key-vault/key-vault-overview.md)
  - [Bir donanım güvenlik modülü (HSM) ve anahtar Kasası'nı kullanma yönergeleri](../key-vault/key-vault-hsm-protected-keys.md)
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
   $server = Set-AzSqlServer `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -AssignIdentity
   ```

Bir sunucu oluşturuyorsanız, kullanın [yeni AzSqlServer](/powershell/module/az.sql/new-azsqlserver) etiket cmdlet'iyle-kimlik sunucusu oluşturulurken bir Azure AD kimlik eklemek için:

   ```powershell
   $server = New-AzSqlServer `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -Location <RegionName> `
   -ServerName <LogicalServerName> `
   -ServerVersion "12.0" `
   -SqlAdministratorCredentials <PSCredential> `
   -AssignIdentity 
   ```

## <a name="step-2-grant-key-vault-permissions-to-your-server"></a>2. Adım Key Vault sunucunuza izinler

Kullanım [kümesi AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) cmdlet'ini sunucu erişim anahtarı kasaya bir anahtarından TDE için kullanmadan önce.

   ```powershell
   Set-AzKeyVaultAccessPolicy  `
   -VaultName <KeyVaultName> `
   -ObjectId $server.Identity.PrincipalId `
   -PermissionsToKeys get, wrapKey, unwrapKey
   ```

## <a name="step-3-add-the-key-vault-key-to-the-server-and-set-the-tde-protector"></a>3. Adım Key Vault anahtarı sunucuya ekleyin ve sonucunda TDE koruyucusuna ayarlayın

- Kullanım [Ekle AzSqlServerKeyVaultKey](/powershell/module/az.sql/add-azsqlserverkeyvaultkey) cmdlet'ini anahtar Key Vault'tan sunucuya ekleyin.
- Kullanım [kümesi AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) cmdlet'i tüm sunucu kaynaklarını için TDE koruyucusu olarak anahtarını ayarlayın.
- Kullanım [Get-AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/get-azsqlservertransparentdataencryptionprotector) cmdlet'ini TDE koruyucusuna beklendiği gibi yapılandırıldığını onaylayın.

> [!Note]
> Birleşik anahtar adını ve anahtar kasası adı için 94 karakter uzunluğunda olabilir.
> 

>[!Tip]
>Bir örnek Keyıd Key vault'tan: https://contosokeyvault.vault.azure.net/keys/Key1/1a1a2b2b3c3c4d4d5e5e6f6f7g7g8h8h
>

   ```powershell
   <# Add the key from Key Vault to the server #>
   Add-AzSqlServerKeyVaultKey `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -KeyId <KeyVaultKeyId>

   <# Set the key as the TDE protector for all resources under the server #>
   Set-AzSqlServerTransparentDataEncryptionProtector `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> 

   <# To confirm that the TDE protector was configured as intended: #>
   Get-AzSqlServerTransparentDataEncryptionProtector `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> 
   ```

## <a name="step-4-turn-on-tde"></a>4. Adım. TDE'yi etkinleştirmek 

Kullanım [kümesi AzSqlDatabaseTransparentDataEncryption](/powershell/module/az.sql/set-azsqldatabasetransparentdataencryption) TDE'yi etkinleştirmek için cmdlet'i.

   ```powershell
   Set-AzSqlDatabaseTransparentDataEncryption `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -DatabaseName <DatabaseName> `
   -State "Enabled"
   ```

Artık veritabanı veya veri ambarını TDE şifreleme anahtarı anahtar Kasası'nda etkinleştirilen sahiptir.

## <a name="step-5-check-the-encryption-state-and-encryption-activity"></a>5. Adım. Şifreleme etkinlik ve şifreleme durumunu denetle

Kullanım [Get-AzSqlDatabaseTransparentDataEncryption](/powershell/module/az.sql/get-azsqldatabasetransparentdataencryption) şifreleme durumunu almak için ve [Get-AzSqlDatabaseTransparentDataEncryptionActivity](/powershell/module/az.sql/get-azsqldatabasetransparentdataencryptionactivity) bir veritabanı şifreleme ilerleme durumunu denetlemek için veya veri ambarı.

   ```powershell
   # Get the encryption state
   Get-AzSqlDatabaseTransparentDataEncryption `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -DatabaseName <DatabaseName> `

   <# Check the encryption progress for a database or data warehouse #>
   Get-AzSqlDatabaseTransparentDataEncryptionActivity `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -DatabaseName <DatabaseName>  
   ```

## <a name="other-useful-powershell-cmdlets"></a>Diğer kullanışlı PowerShell cmdlet'leri

- Kullanım [kümesi AzSqlDatabaseTransparentDataEncryption](/powershell/module/az.sql/set-azsqldatabasetransparentdataencryption) TDE'yi etkinleştirmek için cmdlet'i.

   ```powershell
   Set-AzSqlDatabaseTransparentDataEncryption `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -DatabaseName <DatabaseName> `
   -State "Disabled”
   ```
 
- Kullanım [Get-AzSqlServerKeyVaultKey](/powershell/module/az.sql/get-azsqlserverkeyvaultkey) sunucuya eklenmiş Key Vault anahtar listesini döndürmek için cmdlet'i.

   ```powershell
   <# KeyId is an optional parameter, to return a specific key version #>
   Get-AzSqlServerKeyVaultKey `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName>
   ```
 
- Kullanım [Remove-AzSqlServerKeyVaultKey](/powershell/module/az.sql/remove-azsqlserverkeyvaultkey) bir Key Vault anahtarı sunucudan kaldırmak için.

   ```powershell
   <# The key set as the TDE Protector cannot be removed. #>
   Remove-AzSqlServerKeyVaultKey `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName>   
   ```
 
## <a name="troubleshooting"></a>Sorun giderme

Bir sorun oluşursa aşağıdakileri denetleyin:
- Anahtar kasası bulunamıyor, doğru aboneliği kullanarak işiniz emin olun [Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription) cmdlet'i.

   ```powershell
   Get-AzSubscription `
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
- Komut satırı arabirimi 2.0 veya sonraki bir sürümü. En son sürümünü yükleyin ve Azure aboneliğinize bağlanmak için bkz: [yükleme ve yapılandırma Azure platformlar arası komut satırı arabirimi 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). 
- TDE için kullanılacak bir Azure Key Vault ve anahtarı oluşturun.
  - [CLI 2.0 kullanarak Key Vault yönetme](../key-vault/key-vault-manage-with-cli2.md)
  - [Bir donanım güvenlik modülü (HSM) ve anahtar Kasası'nı kullanma yönergeleri](../key-vault/key-vault-hsm-protected-keys.md)
    - Anahtar kasası TDE için kullanılacak özelliğine sahip olmalıdır:
  - [Geçici silme](../key-vault/key-vault-ovw-soft-delete.md)
  - [Key Vault geçici silmeyi CLI ile kullanma](../key-vault/key-vault-soft-delete-cli.md) 
- Anahtar TDE için kullanılacak aşağıdaki özniteliklere sahip olmanız gerekir:
   - Sona erme tarihi
   - Devre dışı değil
   - Şunları gerçekleştirmek *alma*, *anahtarı sarmalama*, *anahtarı kaydırma* işlemleri
   
## <a name="step-1-create-a-server-with-an-azure-ad-identity"></a>1. Adım Bir Azure AD kimlik ile bir sunucu oluşturma
      cli
      # create server (with identity) and database
      az sql server create --name <servername> --resource-group <rgname>  --location <location> --admin-user <user> --admin-password <password> --assign-identity
      az sql db create --name <dbname> --server <servername> --resource-group <rgname>  
 
 
>[!Tip]
>"Principalıd" sunucu oluşturmaktan tutun, sonraki adımda anahtar kasası izinlerini atamak için kullanılan nesne kimliği
>
 
## <a name="step-2-grant-key-vault-permissions-to-the-logical-sql-server"></a>2. Adım Key Vault mantıksal sql sunucusu izinleri verme
      cli
      # create key vault, key and grant permission
       az keyvault create --name <kvname> --resource-group <rgname> --location <location> --enable-soft-delete true
       az keyvault key create --name <keyname> --vault-name <kvname> --protection software
       az keyvault set-policy --name <kvname>  --object-id <objectid> --resource-group <rgname> --key-permissions wrapKey unwrapKey get 


>[!Tip]
>Örneğin, anahtar URI'si veya yeni bir anahtar için bir sonraki adımda, Keyıd bulundurun: https://contosokeyvault.vault.azure.net/keys/Key1/1a1a2b2b3c3c4d4d5e5e6f6f7g7g8h8h
>
 
       
## <a name="step-3-add-the-key-vault-key-to-the-server-and-set-the-tde-protector"></a>3. Adım Key Vault anahtarı sunucuya ekleyin ve sonucunda TDE koruyucusuna ayarlayın
  
     cli
     # add server key and update encryption protector
     az sql server key create --server <servername> --resource-group <rgname> --kid <keyID>
     az sql server tde-key set --server <servername> --server-key-type AzureKeyVault  --resource-group <rgname> --kid <keyID>

        
  > [!Note]
> Birleşik anahtar adını ve anahtar kasası adı için 94 karakter uzunluğunda olabilir.
> 

  
## <a name="step-4-turn-on-tde"></a>4. Adım. TDE'yi etkinleştirmek 
      cli
      # enable encryption
      az sql db tde set --database <dbname> --server <servername> --resource-group <rgname> --status Enabled 
      

Veritabanını veya veri ambarı artık etkinleştirilmiş olan bir müşteri tarafından yönetilen bir şifreleme anahtarı Azure Key vault'taki TDE sahiptir.

## <a name="step-5-check-the-encryption-state-and-encryption-activity"></a>5. Adım. Şifreleme etkinlik ve şifreleme durumunu denetle

     cli
      # get encryption scan progress
      az sql db tde list-activity --database <dbname> --server <servername> --resource-group <rgname>  

      # get whether encryption is on or off
      az sql db tde show --database <dbname> --server <servername> --resource-group <rgname> 

## <a name="sql-cli-references"></a>SQL CLI başvuruları

https://docs.microsoft.com/cli/azure/sql 

https://docs.microsoft.com/cli/azure/sql/server/key 

https://docs.microsoft.com/cli/azure/sql/server/tde-key 

https://docs.microsoft.com/cli/azure/sql/db/tde 

