---
title: TDE sertifikasını geçirme - Azure SQL Veritabanı Yönetilen Örneği | Microsoft Docs
description: Saydam Veri Şifrelemesi ile veritabanının Veritabanı Şifreleme Anahtarı’nı koruyan sertifikayı Azure SQL Yönetilen örneğine geçirme
keywords: sql veritabanı öğreticisi, sql veritabanı yönetilen örneği, TDE sertifikasını geçirme
services: sql-database
author: MladjoA
ms.reviewer: carlrab, jovanpop
ms.service: sql-database
ms.custom: managed instance
ms.topic: tutorial
ms.date: 07/16/2018
ms.author: mlandzic
manager: craigg
ms.openlocfilehash: 042d89017db898102deafc9156cf847a08c92227
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39074708"
---
# <a name="migrate-certificate-of-tde-protected-database-to-azure-sql-managed-instance"></a>TDE korumalı veritabanının sertifikasını Azure SQL Yönetilen Örneği’ne geçirme

[Saydam Veri Şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption) ile korunan veritabanı yerel geri yükleme seçeneği kullanılarak Azure SQL Yönetilen Örneği’ne geçirildiğinde, veritabanı geri yüklenmeden önce ilgili sertifikanın şirket içinden veya IaaS SQL Server’dan geçirilmesi gerekir. Bu makale, sertifikanın Azure SQL Veritabanı Yönetilen Örneği’ne el ile geçiş işleminde size yol gösterir:

> [!div class="checklist"]
> * Sertifikayı Kişisel Bilgi Değişimi (.pfx) dosyası olarak dışarı aktarma
> * Sertifikayı dosyadan base-64 dizesine ayıklama
> * PowerShell cmdlet’ini kullanarak bunu karşıya yükleme

Tam yönetilen hizmet kullanılarak hem TDE korumalı veritabanının hem de ilgili sertifikanın sorunsuz geçişini sağlamaya yönelik alternatif bir seçenek için bkz. [Azure Veritabanı Geçiş Hizmeti'ni kullanarak şirket içi veritabanınızı Yönetilen Örneğe geçirme](../dms/tutorial-sql-server-to-managed-instance.md).

> [!IMPORTANT]
> Azure SQL Yönetilen Örneği için Saydam Veri Şifrelemesi hizmetle yönetilen modda çalışır. Geçirilen sertifika yalnızca TDE korumalı veritabanını geri yüklemek için kullanılır. Geri yükleme işlemi yapıldıktan hemen sonra, geçirilen sertifika farklı, sistem tarafından yönetilen bir sertifikayla değiştirilir.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları tamamlayabilmeniz için şu önkoşullar gereklidir:

- Şirket içi sunucuya veya dosya olarak dışarı aktarılan sertifikaya erişimi olan başka bir bilgisayara yüklenmiş [Pvk2Pfx](https://docs.microsoft.com/windows-hardware/drivers/devtest/pvk2pfx) komut satırı aracı. Pvk2Pfx aracı, tek başına kendi içinde bir komut satırı ortamı olan [Enterprise Windows Driver Kit](https://docs.microsoft.com/windows-hardware/drivers/download-the-wdk)'in bir parçasıdır.
- [Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell) sürüm 5.0 veya üstü yüklenmiş olmalıdır.
- AzureRM PowerShell modülü [yüklenmiş ve güncelleştirilmiş](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) olmalıdır.\[AzureRM.Sql modülü](https://www.powershellgallery.com/packages/AzureRM.Sql) sürüm 4.10.0 veya üstü.
- PowerShell modülünü yüklemek/güncelleştirmek için PowerShell’de şu komutları çalıştırın:

   ```powershell
   Install-Module -Name AzureRM.Sql
   Update-Module -Name AzureRM.Sql
   ```

## <a name="export-tde-certificate-to-a-personal-information-exchange-pfx-file"></a>TDE sertifikasını Kişisel Bilgi Değişimi (.pfx) dosyasında dışarı aktarma

Sertifika doğrudan kaynak SQL Server’dan veya sertifika depolama alanından (burada tutuluyorsa) dışarı aktarılabilir.

### <a name="export-certificate-from-the-source-sql-server"></a>Sertifikayı kaynak SQL Server’dan dışarı aktarma

Sertifikayı SQL Server Management Studio ile dışarı aktarmak ve bunu pfx biçimine dönüştürmek için aşağıdaki adımları kullanın. Bu adımlarda sertifikanın dosya adları ve yolları için *TDE_Cert* ve *full_path* genel adları kullanılır. Bunlar gerçek adlarla değiştirilmelidir.

1. SSMS’de yeni bir sorgu penceresi açın ve kaynak SQL Server’a bağlanın.
2. TDE korumalı veritabanlarını listelemek ve geçirilecek veritabanının şifrelemesini koruyan sertifikanın adını almak için şu betiği kullanın:

   ```sql
   USE master
   GO
   SELECT db.name as [database_name], cer.name as [certificate_name]
   FROM sys.dm_database_encryption_keys dek
   LEFT JOIN sys.certificates cer
   ON dek.encryptor_thumbprint = cer.thumbprint
   INNER JOIN sys.databases db
   ON dek.database_id = db.database_id
   WHERE dek.encryption_state = 3
   ```

   ![TDE sertifikalarının listesi](./media/sql-database-managed-instance-migrate-tde-certificate/onprem-certificate-list.png)

3. Sertifikayı, ortak ve özel anahtar bilgilerinin tutulduğu dosya çiftine (.cer ve .pvk) dışarı aktarmak için şu betiği yürütün:

   ```sql
   USE master
   GO
   BACKUP CERTIFICATE TDE_Cert
   TO FILE = 'c:\full_path\TDE_Cert.cer'
   WITH PRIVATE KEY (
     FILE = 'c:\full_path\TDE_Cert.pvk',
     ENCRYPTION BY PASSWORD = '<SomeStrongPassword>'
   )
   ```

   ![yedek TDE sertifikası](./media/sql-database-managed-instance-migrate-tde-certificate/backup-onprem-certificate.png)

4. Pvk2Pfx aracını kullanarak, sertifika bilgilerini yeni oluşturulmuş dosya çiftinden Kişisel Bilgi Değişimi (.pfx) dosyasına kopyalamak için PowerShell konsolunu kullanın:

   ```powershell
   .\pvk2pfx -pvk c:/full_path/TDE_Cert.pvk  -pi "<SomeStrongPassword>" -spc c:/full_path/TDE_Cert.cer -pfx c:/full_path/TDE_Cert.pfx
   ```

### <a name="export-certificate-from-certificate-store"></a>Sertifika depolama alanından sertifikayı dışarı aktarma

Sertifika SQL Server’ın yerel makine sertifika depolama alanında tutuluyorsa şu adımlar kullanılarak dışarı aktarılabilir:

1. PowerShell konsolunu açın ve Microsoft Yönetim Konsolu’nun Sertifikalar ek bileşenini açmak için şu komutu yürütün:

   ```powershell
   certlm
   ```

2. Sertifikalar MMC ek bileşeninde, sertifikaların listesini görmek için Kişisel -> Sertifikalar yolunu genişletin

3. Sertifikaya sağ tıklayın ve Dışarı Aktar... seçeneğine tıklayın

4. Sertifikayı ve özel anahtarı Kişisel Bilgi Değişimi biçiminde dışarı aktarmak için sihirbazı izleyin

## <a name="extract-certificate-from-file-to-base-64-string"></a>Sertifikayı dosyadan base-64 dizesine ayıklama

PowerShell’de şu betiği yürütün ve çıkış olarak base-64 kodlamalı sertifikayı alın:

```powershell
$fileContentBytes = Get-Content 'C:/full_path/TDE_Cert.pfx' -Encoding Byte
$base64EncodedCert = [System.Convert]::ToBase64String($fileContentBytes)
echo $base64EncodedCert
```

## <a name="upload-certificate-to-azure-sql-managed-instance-using-azure-powershell-cmdlet"></a>Azure PowerShell cmdlet’ini kullanarak sertifikayı Azure SQL Yönetilen Örneği’ne yükleyin

1. PowerShell’deki hazırlık adımlarını başlatın:

   ```powershell
   # Import the module into the PowerShell session
   Import-Module AzureRM
   # Connect to Azure with an interactive dialog for sign-in
   Connect-AzureRmAccount
   # List subscriptions available and copy id of the subscription target Managed Instance belongs to
   Get-AzureRmSubscription
   # Set subscription for the session
   Select-AzureRmSubscription Guid_Subscription_Id
   ```

2. Tüm hazırlık adımları tamamladıktan sonra, base-64 kodlamalı sertifikayı hedef Yönetilen Örneğe yüklemek için şu komutları çalıştırın:

   ```powershell
   $privateBlob = "<base-64-encoded-certificate-string>"
   $securePrivateBlob = $privateBlob  | ConvertTo-SecureString -AsPlainText -Force
   $password = "SomeStrongPassword"
   $securePassword = $password | ConvertTo-SecureString -AsPlainText -Force
   Add-AzureRmSqlManagedInstanceTransparentDataEncryptionCertificate -ResourceGroupName "<ResourceGroupName>" -ManagedInstanceName "<ManagedInstanceName>" -PrivateBlob $securePrivateBlob -Password $securePassword
   ```

Sertifikaya artık belirtilen Yönetilen Örnekten ulaşılabilir ve buna karşılık gelen TDE korumalı veritabanının yedeği başarılı bir şekilde geri yüklenebilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Saydam Veri Şifrelemesi ile veritabanının şifreleme anahtarını koruyarak sertifikayı şirket içi veya IaaS SQL Server’dan Azure SQL Yönetilen Örneği’ne geçirmeyi öğrendiniz.

Azure SQL Veritabanı Yönetilen Örneği’nde veritabanı yedeğini geri yüklemeyi öğrenmek için bkz. [Veritabanı yedeğini Azure SQL Veritabanı Yönetilen Örneği’ne geri yükleme](sql-database-managed-instance-restore-from-backup-tutorial.md).
