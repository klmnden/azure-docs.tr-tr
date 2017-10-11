## <a name="next-steps"></a>Sonraki adımlar

Azure anahtar kasası tümleştirmeyi etkinleştirdikten sonra SQL VM üzerinde SQL Server şifrelemeyi etkinleştirebilirsiniz. İlk olarak, anahtar kasanızı içinde bir asimetrik anahtar ve SQL Server içinde bir simetrik anahtar kendi VM'nizi oluşturmanız gerekir. Ardından, veritabanları ve yedeklemeleri şifrelemeyi etkinleştirmek için T-SQL deyimlerini yürütmek mümkün olacaktır.

Birkaç forms özelliklerden yararlanabilirsiniz şifreleme vardır:

* [Saydam veri şifreleme (TDE)](https://msdn.microsoft.com/library/bb934049.aspx)
* [Şifreli yedekleme](https://msdn.microsoft.com/library/dn449489.aspx)
* [Sütun düzeyinde şifreleme (Temizle)](https://msdn.microsoft.com/library/ms173744.aspx)

Bu alanların her biri için aşağıdaki Transact-SQL betikleri örnekler sağlar.

### <a name="prerequisites-for-examples"></a>Örnekler için Önkoşullar

Bulunan iki önkoşul tabanlı her örnek: bir asimetrik anahtar, anahtar Kasası'ndan adlı **CONTOSO_KEY** AKV tümleştirme özelliği tarafından oluşturulan bir kimlik bilgisi adı verilen ve **Azure_EKM_TDE_cred**. Aşağıdaki Transact-SQL komutlarıyla örnekleri çalıştırmak için bu Önkoşullar ayarlayın.

``` sql
USE master;
GO

sp_configure [show advanced options], 1;
GO
RECONFIGURE;
GO

-- Enable EKM provider
sp_configure [EKM provider enabled], 1;
GO
RECONFIGURE;

--create provider
CREATE CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov
FROM FILE = 'C:\Program Files\SQL Server Connector for Microsoft Azure Key Vault\Microsoft.AzureKeyVaultService.EKM.dll';
GO

--create credential
CREATE CREDENTIAL sysadmin_ekm_cred
    WITH IDENTITY = 'keytestvault', --keyvault
    SECRET = '<<SECRET>>'
FOR CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov;


--must have sysadmin
ALTER LOGIN [TDE_Login]
ADD CREDENTIAL sysadmin_ekm_cred;


CREATE ASYMMETRIC KEY CONTOSO_KEY
FROM PROVIDER [AzureKeyVault_EKM_Prov]
WITH PROVIDER_KEY_NAME = 'keytestvault',  --key name
CREATION_DISPOSITION = OPEN_EXISTING;
```

### <a name="transparent-data-encryption-tde"></a>Saydam veri şifreleme (TDE)

1. TDE için veritabanı altyapısı tarafından kullanılacak bir SQL Server oturumu oluşturun, sonra kimlik bilgisi ekleyin.

   ``` sql
   USE master;
   -- Create a SQL Server login associated with the asymmetric key
   -- for the Database engine to use when it loads a database
   -- encrypted by TDE.
   CREATE LOGIN TDE_Login
   FROM ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter the TDE Login to add the credential for use by the
   -- Database Engine to access the key vault
   ALTER LOGIN TDE_Login
   ADD CREDENTIAL Azure_EKM_TDE_cred;
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

### <a name="encrypted-backups"></a>Şifreli yedekleme

1. Yedeklemeleri şifrelemek için veritabanı motoru tarafından kullanılmak üzere bir SQL Server oturumu oluşturun ve kimlik bilgisi ekleyin.

   ``` sql
   USE master;
   -- Create a SQL Server login associated with the asymmetric key
   -- for the Database engine to use when it is encrypting the backup.
   CREATE LOGIN Backup_Login
   FROM ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter the Encrypted Backup Login to add the credential for use by
   -- the Database Engine to access the key vault
   ALTER LOGIN Backup_Login
   ADD CREDENTIAL Azure_EKM_Backup_cred ;
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

### <a name="column-level-encryption-cle"></a>Sütun düzeyinde şifreleme (Temizle)

Bu komut dosyası anahtar kasasında asimetrik anahtar tarafından korunan bir simetrik anahtar oluşturur ve veritabanındaki verileri şifrelemek için simetrik anahtar kullanır.

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

Bu şifreleme özelliklerinin nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [kullanarak EKM SQL Server şifreleme özellikleriyle](https://msdn.microsoft.com/library/dn198405.aspx#UsesOfEKM).

Bu makaledeki adımları zaten bir Azure sanal makinede çalışan SQL Server olduğunu varsayalım unutmayın. Aksi takdirde bkz [bir SQL Server sanal makinesi sağlama](../articles/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md). Azure Vm'lerinde SQL Server çalıştıran diğer yönergeler için bkz: [Azure sanal makinelere genel bakış SQL Server'da](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md).