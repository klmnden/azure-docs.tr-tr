---
author: rothja
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 10/26/2018
ms.author: jroth
ms.openlocfilehash: 22f16a7382cb0fe1f3fe2a6ef5e7c00a6989623c
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188321"
---
## <a name="next-steps"></a>Sonraki adımlar

Azure anahtar kasası tümleştirmeyi etkinleştirdikten sonra SQL sanal makinenizde SQL Server şifrelemeyi etkinleştirebilirsiniz. İlk olarak, anahtar kasanıza içinde asimetrik bir anahtar ve SQL Server içinde bir simetrik anahtar, sanal makinenizde oluşturmanız gerekecektir. Ardından, veritabanları ve yedeklemeler için şifrelemeyi etkinleştirmek için T-SQL deyimlerini yürütmek mümkün olacaktır.

Çeşitli avantajlarından yararlanabilirsiniz şifreleme biçimi vardır:

* [Saydam Veri Şifrelemesi (TDE)](https://msdn.microsoft.com/library/bb934049.aspx)
* [Şifrelenmiş yedekleme](https://msdn.microsoft.com/library/dn449489.aspx)
* [Sütun düzeyinde şifrelemeyi (CLE)](https://msdn.microsoft.com/library/ms173744.aspx)

Bu alanların her biri için aşağıdaki Transact-SQL betikleri örnekler sunar.

### <a name="prerequisites-for-examples"></a>Örnekler için Önkoşullar

Her örnek üzerinde iki önkoşulları dayanır: bir asimetrik anahtar, anahtar Kasası'ndaki adlı **CONTOSO_KEY** AKV tümleştirme özelliği tarafından oluşturulan bir kimlik bilgisi adı verilen ve **Azure_EKM_TDE_cred**. Aşağıdaki Transact-SQL komutlarını örnekleri çalıştırmak için şu önkoşulların ayarlayın.

``` sql
USE master;
GO

--create credential
--The <<SECRET>> here requires the <Application ID> (without hyphens) and <Secret> to be passed together without a space between them.
CREATE CREDENTIAL sysadmin_ekm_cred
    WITH IDENTITY = 'keytestvault', --keyvault
    SECRET = '<<SECRET>>'
FOR CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov;


--Map the credential to a SQL login that has sysadmin permissions. This allows the SQL login to access the key vault when creating the asymmetric key in the next step.
ALTER LOGIN [SQL_Login]
ADD CREDENTIAL sysadmin_ekm_cred;


CREATE ASYMMETRIC KEY CONTOSO_KEY
FROM PROVIDER [AzureKeyVault_EKM_Prov]
WITH PROVIDER_KEY_NAME = 'KeyName_in_KeyVault',  --The key name here requires the key we created in the key vault
CREATION_DISPOSITION = OPEN_EXISTING;
```

### <a name="transparent-data-encryption-tde"></a>Saydam Veri Şifrelemesi (TDE)

1. TDE için veritabanı altyapısı tarafından kullanılacak bir SQL Server oturumu oluşturun ve ardından kimlik bilgisi ekleyin.

   ``` sql
   USE master;
   -- Create a SQL Server login associated with the asymmetric key
   -- for the Database engine to use when it loads a database
   -- encrypted by TDE.
   CREATE LOGIN EKM_Login
   FROM ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter the TDE Login to add the credential for use by the
   -- Database Engine to access the key vault
   ALTER LOGIN EKM_Login
   ADD CREDENTIAL Azure_EKM_cred;
   GO
   ```

1. TDE için kullanılacak veritabanı şifreleme anahtarı oluşturun.

   ``` sql
   USE ContosoDatabase;
   GO

   CREATE DATABASE ENCRYPTION KEY 
   WITH ALGORITHM = AES_128 
   ENCRYPTION BY SERVER ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter the database to enable transparent data encryption.
   ALTER DATABASE ContosoDatabase
   SET ENCRYPTION ON;
   GO
   ```

### <a name="encrypted-backups"></a>Şifrelenmiş yedekleme

1. Yedeklemeleri şifrelemek için veritabanı altyapısı tarafından kullanılacak bir SQL Server oturumu oluşturun ve kimlik bilgisi ekleyin.

   ``` sql
   USE master;
   -- Create a SQL Server login associated with the asymmetric key
   -- for the Database engine to use when it is encrypting the backup.
   CREATE LOGIN EKM_Login
   FROM ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter the Encrypted Backup Login to add the credential for use by
   -- the Database Engine to access the key vault
   ALTER LOGIN EKM_Login
   ADD CREDENTIAL Azure_EKM_cred ;
   GO
   ```

1. Yedekleme veritabanı belirtme şifreleme anahtar kasasında depolanan asimetrik anahtarla.

   ``` sql
   USE master;
   BACKUP DATABASE [DATABASE_TO_BACKUP]
   TO DISK = N'[PATH TO BACKUP FILE]'
   WITH FORMAT, INIT, SKIP, NOREWIND, NOUNLOAD,
   ENCRYPTION(ALGORITHM = AES_256, SERVER ASYMMETRIC KEY = [CONTOSO_KEY]);
   GO
   ```

### <a name="column-level-encryption-cle"></a>Sütun düzeyinde şifrelemeyi (CLE)

Bu betik, key vault'ta asimetrik anahtar tarafından korunan bir simetrik anahtar oluşturur ve sonra veritabanındaki verileri şifrelemek için simetrik anahtarı kullanır.

``` sql
CREATE SYMMETRIC KEY DATA_ENCRYPTION_KEY
WITH ALGORITHM=AES_256
ENCRYPTION BY ASYMMETRIC KEY CONTOSO_KEY;

DECLARE @DATA VARBINARY(MAX);

--Open the symmetric key for use in this session
OPEN SYMMETRIC KEY DATA_ENCRYPTION_KEY
DECRYPTION BY ASYMMETRIC KEY CONTOSO_KEY;

--Encrypt syntax
SELECT @DATA = ENCRYPTBYKEY(KEY_GUID('DATA_ENCRYPTION_KEY'), CONVERT(VARBINARY,'Plain text data to encrypt'));

-- Decrypt syntax
SELECT CONVERT(VARCHAR, DECRYPTBYKEY(@DATA));

--Close the symmetric key
CLOSE SYMMETRIC KEY DATA_ENCRYPTION_KEY;
```

## <a name="additional-resources"></a>Ek kaynaklar

Bu şifreleme özelliklerini kullanma hakkında daha fazla bilgi için bkz. [EKM kullanarak SQL Server şifreleme özellikleri ile](https://msdn.microsoft.com/library/dn198405.aspx#UsesOfEKM).

Bu makaledeki adımlarda, Azure sanal makinesinde çalışan SQL Server zaten sahip olduğunuzu varsaymaktadır unutmayın. Aksi takdirde bkz [azure'da bir SQL Server sanal makinesi sağlama](../articles/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md). Azure Vm'lerinde SQL Server çalıştıran diğer yönergeler için bkz. [SQL Server Azure sanal makinelerine genel bakış](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md).
