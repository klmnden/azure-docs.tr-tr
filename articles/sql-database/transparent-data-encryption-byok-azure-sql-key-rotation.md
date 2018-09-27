---
title: PowerShell - döndürme TDE koruyucusuna - Azure SQL veritabanı | Microsoft Docs
description: Bir Azure SQL sunucusu için saydam veri şifrelemesi (TDE) koruyucu döndürme hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: becczhang
ms.author: ryzhang26
ms.reviewer: vanto
manager: jhubbard
ms.date: 08/07/2017
ms.openlocfilehash: 365cb020bb6b2786a89a98e008d1b62c23c1c0e7
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166597"
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

- Bu nasıl yapılır kılavuzunda, zaten Azure Key vault'tan bir anahtar TDE koruyucusu olarak bir Azure SQL veritabanı veya veri ambarı için kullandığınızı varsayar. Bkz: [BYOK destekli saydam veri şifrelemesi](transparent-data-encryption-byok-azure-sql.md).
- Azure PowerShell sürüm 3.7.0 olmalıdır veya üzerinin yüklü ve çalışıyor. 
- [İsteğe bağlı ancak önerilen] Bir donanım güvenlik modülü (HSM) anahtar malzemesi için TDE koruyucusu oluşturun veya yerel anahtarı ilk depolamak ve anahtar malzemesi Azure anahtar Kasası'na içeri aktarın. İzleyin [bir donanım güvenlik modülü (HSM) ve anahtar kasası kullanmaya yönelik yönergeler](https://docs.microsoft.com/azure/key-vault/key-vault-get-started) daha fazla bilgi için.

## <a name="option-1-auto-rotation"></a>1. seçenek: Otomatik döndürme

Yeni bir sürümü mevcut TDE koruyucusu anahtarı anahtar Kasası'nda aynı anahtar adını ve anahtar kasası altında oluşturur. 24 saat içinde bu yeni sürümü kullanarak Azure SQL hizmetini başlatır. 

Kullanarak koruyucusu TDE yeni bir sürümünü oluşturmak için [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey) cmdlet:

   ```powershell
   Add-AzureKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName> `
   -Destination <HardwareOrSoftware>
   ```

## <a name="option-2-manual-rotation"></a>2. seçenek: El ile döndürme

Seçenek kullandığı [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey), [Ekle AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/add-azurermsqlserverkeyvaultkey), ve [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) eklemek için cmdlet'leri Yeni bir anahtar adı veya hatta başka bir anahtar kasası altında olabilecek tamamen yeni bir anahtar. 

>[!NOTE]
>Birleşik anahtar adını ve anahtar kasası adı için 94 karakter uzunluğunda olabilir.
>

   ```powershell
   # Add a new key to Key Vault
   Add-AzureKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName> `
   -Destination <HardwareOrSoftware>

   # Add the new key from Key Vault to the server
   Add-AzureRmSqlServerKeyVaultKey `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>   
  
   <# Set the key as the TDE protector for all resources 
   under the server #>
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>
   ```
  
## <a name="other-useful-powershell-cmdlets"></a>Diğer kullanışlı PowerShell cmdlet'leri

- Microsoft tarafından yönetilen gelen TDE koruyucusuna BYOK moduna geçiş yapmak için kullanın [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) cmdlet'i.

   ```powershell
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>
   ```

- İçin Microsoft tarafından yönetilen, BYOK modundan TDE koruyucusuna geçiş yapmak için kullanın [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) cmdlet'i.

   ```powershell
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -Type ServiceManaged `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName> 
   ``` 

## <a name="next-steps"></a>Sonraki adımlar

- Bir güvenlik riski olması durumunda, riskli olabilecek TDE koruyucusu kaldırmayı öğrenin: [riskli olabilecek bir anahtarı Kaldır](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md) 

- Destek TDE için kendi anahtarını Getir ile çalışmaya başlama: [TDE PowerShell kullanarak Key vault'tan kendi anahtarınızı kullanarak Aç](transparent-data-encryption-byok-azure-sql-configure.md)
