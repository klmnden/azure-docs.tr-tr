---
title: PowerShell - döndürme TDE koruyucusuna - Azure SQL veritabanı | Microsoft Docs
description: Bir Azure SQL sunucusu için saydam veri şifrelemesi (TDE) koruyucu döndürme hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aliceku
ms.author: aliceku
ms.reviewer: vanto
manager: jhubbard
ms.date: 03/12/2019
ms.openlocfilehash: 760b292e75b4cc64b85eaf51ffad0521b721dabf
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60330865"
---
# <a name="rotate-the-transparent-data-encryption-tde-protector-using-powershell"></a>PowerShell kullanarak saydam veri şifrelemesi (TDE) koruyucu Döndür

Bu makalede, Azure Key vault'tan TDE koruyucusu kullanarak bir Azure SQL sunucusu için anahtar döndürme açıklanır. Bir Azure SQL server'ın TDE koruyucusu anlamına gelir, korunan sunucudaki veritabanlarını yeni bir asimetrik anahtar geçiş döndürme. Anahtar döndürme, çevrimiçi bir işlemdir ve yalnızca bu yalnızca şifresini çözer ve veritabanının veri şifreleme anahtarı, tüm veritabanını yeniden şifreler olduğundan tamamlanması birkaç saniye sürer.

Bu kılavuz, sunucuda TDE koruyucusuna döndürmek için iki seçenek açıklanır.

> [!NOTE]
> Duraklatılmış bir SQL veri ambarı, anahtar devirlerini önce sürdürüldü gerekir.
>

> [!IMPORTANT]
> **Silemediğini yapmak** önceki bir geçiş anahtarı.  Anahtarları uzatılabilir, bazı veriler hala eski veritabanı yedeklemeleri gibi önceki anahtarlarla şifrelenir. 
>

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure Resource Manager PowerShell modülü, Azure SQL veritabanı tarafından hala desteklenmektedir, ancak tüm gelecekteki geliştirme için Az.Sql modüldür. Bu cmdlet'ler için bkz. [Azurerm.SQL'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az modül ve AzureRm modülleri komutları için bağımsız değişkenler büyük ölçüde aynıdır.

- Bu nasıl yapılır kılavuzunda, zaten Azure Key vault'tan bir anahtar TDE koruyucusu olarak bir Azure SQL veritabanı veya veri ambarı için kullandığınızı varsayar. Bkz: [BYOK destekli saydam veri şifrelemesi](transparent-data-encryption-byok-azure-sql.md).
- Azure PowerShell yüklenmiş ve çalışıyor olması gerekir.
- [İsteğe bağlı ancak önerilen] Bir donanım güvenlik modülü (HSM) anahtar malzemesi için TDE koruyucusu oluşturun veya yerel anahtarı ilk depolamak ve anahtar malzemesi Azure anahtar Kasası'na içeri aktarın. İzleyin [bir donanım güvenlik modülü (HSM) ve anahtar kasası kullanmaya yönelik yönergeler](https://docs.microsoft.com/azure/key-vault/key-vault-get-started) daha fazla bilgi için.

## <a name="manual-key-rotation"></a>El ile anahtar döndürme

El ile anahtar döndürme kullanan [Ekle AzKeyVaultKey](/powershell/module/az.keyvault/Add-AzKeyVaultKey), [Ekle AzSqlServerKeyVaultKey](/powershell/module/az.sql/add-azsqlserverkeyvaultkey), ve [kümesi AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) cmdlet'leri eklemek için bir tamamen yeni anahtarı altında yeni bir anahtar adı veya hatta başka bir anahtar kasası olabilir. Bu yaklaşımı kullanarak, aynı anahtarı yüksek oranda kullanılabilir ve coğrafi-dr senaryoları desteklemek için farklı anahtar kasalarına eklenmesini destekler.

>[!NOTE]
>Birleşik anahtar adını ve anahtar kasası adı için 94 karakter uzunluğunda olabilir.

   ```powershell
   # Add a new key to Key Vault
   Add-AzKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName> `
   -Destination <HardwareOrSoftware>

   # Add the new key from Key Vault to the server
   Add-AzSqlServerKeyVaultKey `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>
  
   <# Set the key as the TDE protector for all resources under the server #>
   Set-AzSqlServerTransparentDataEncryptionProtector `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>
   ```

## <a name="option-2-manual-rotation"></a>2\. seçenek: El ile döndürme

Seçeneği kullandığı [Ekle AzKeyVaultKey](/powershell/module/az.keyvault/add-azkeyvaultkey), [Ekle AzSqlServerKeyVaultKey](/powershell/module/az.sql/add-azsqlserverkeyvaultkey), ve [kümesi AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) eklemek için cmdlet'leri bir tamamen Yeni anahtarı altında yeni bir anahtar adı veya hatta başka bir anahtar kasası olabilir. 

>[!NOTE]
>Birleşik anahtar adını ve anahtar kasası adı için 94 karakter uzunluğunda olabilir.
>

   ```powershell
   # Add a new key to Key Vault
   Add-AzKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName> `
   -Destination <HardwareOrSoftware>

   # Add the new key from Key Vault to the server
   Add-AzSqlServerKeyVaultKey `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>   
  
   <# Set the key as the TDE protector for all resources 
   under the server #>
   Set-AzSqlServerTransparentDataEncryptionProtector `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>
   ```
  
## <a name="other-useful-powershell-cmdlets"></a>Diğer kullanışlı PowerShell cmdlet'leri

- Microsoft tarafından yönetilen gelen TDE koruyucusuna BYOK moduna geçiş yapmak için kullanın [kümesi AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) cmdlet'i.

   ```powershell
   Set-AzSqlServerTransparentDataEncryptionProtector `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>
   ```

- İçin Microsoft tarafından yönetilen, BYOK modundan TDE koruyucusuna geçiş yapmak için kullanın [kümesi AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) cmdlet'i.

   ```powershell
   Set-AzSqlServerTransparentDataEncryptionProtector `
   -Type ServiceManaged `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName> 
   ``` 

## <a name="next-steps"></a>Sonraki adımlar

- Bir güvenlik riski olması durumunda, riskli olabilecek TDE koruyucusu kaldırma işlemleri gerçekleştirmeyi öğreneceksiniz: [Riskli olabilecek bir anahtarı Kaldır](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md) 

- TDE için kendi anahtarını Getir destek ve Azure anahtar kasası tümleştirme ile kullanmaya başlayın: [PowerShell kullanarak Key vault'tan kendi anahtarınızı kullanarak TDE Aç](transparent-data-encryption-byok-azure-sql-configure.md)
