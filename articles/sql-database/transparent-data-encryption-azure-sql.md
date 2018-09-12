---
title: Azure SQL veritabanı ve veri ambarı için saydam veri şifrelemesi | Microsoft Docs
description: SQL veritabanı ve veri ambarı için saydam veri şifrelemesi genel bakış. Belge, aboneliğin avantajları ve hizmetle yönetilen şeffaf veri şifreleme ve kendi anahtarını Getir içeren yapılandırma seçeneklerini kapsar.
services: sql-database
author: becczhang
manager: craigg
ms.prod: ''
ms.reviewer: carlrab
ms.prod_service: sql-database, sql-data-warehouse
ms.service: sql-database
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.date: 07/09/2018
ms.author: aliceku
monikerRange: = azuresqldb-current || = azure-sqldw-latest || = sqlallproducts-allversions
ms.openlocfilehash: afc53fc1abce74b247ec2e25bc3e4845bc870860
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44380208"
---
# <a name="transparent-data-encryption-for-sql-database-and-data-warehouse"></a>SQL veritabanı ve veri ambarı için saydam veri şifrelemesi

Saydam veri şifrelemesi (TDE), Azure SQL veritabanı ve Azure veri ambarı, kötü amaçlı etkinlik tehditlerine karşı koruma yardımcı olur. Bu gerçek zamanlı şifreleme ve şifre çözme veritabanını, ilişkili yedeklemeler ve işlem günlük dosyaları bekleme sırasında uygulamada değişiklik gerektirmeden gerçekleştirir. Varsayılan olarak, tüm yeni dağıtılan Azure SQL veritabanları için TDE etkin. TDE, mantıksal şifrelemek için kullanılamaz **ana** SQL veritabanı.  **Ana** veritabanı kullanıcı veritabanlarında TDE işlemlerin gerçekleştirilmesi için gereken nesneleri içerir.

TDE, eski veritabanları veya Azure SQL veri ambarı için el ile etkinleştirilmesi gerekir.  

Saydam veri şifrelemesi tüm veritabanı depolama, veritabanı şifreleme anahtarı olarak adlandırılan bir simetrik anahtarı'nı kullanarak şifreler. Bu veritabanı şifreleme anahtarı saydam veri şifrelemesi koruyucu tarafından korunur. Koruyucu ya da bir hizmet tarafından yönetiliyor sertifika (hizmetle yönetilen şeffaf veri şifreleme) veya Azure anahtar Kasası'nda (kendi anahtarını Getir) depolanan bir asimetrik anahtar. Sunucu düzeyinde saydam veri şifrelemesi koruyucu ayarladığınız. 

Veritabanı başlatma sırasında şifrelenmiş veritabanı şifreleme anahtarını şifresi çözülür ve şifre çözme ve SQL Server veritabanı altyapısı işlemdeki veritabanı dosyalarını yeniden şifrelenmesi için kullanılır. Saydam veri şifrelemesi, gerçek zamanlı g/ç şifreleme ve şifre çözme veri sayfa düzeyinde gerçekleştirir. Belleğe okuyun ve sonra da yazılmadan önce şifrelenir her sayfanın şifresi çözülür diske. Saydam veri şifrelemesi genel bir açıklaması için bkz. [saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption).

Ayrıca bir Azure sanal makinesinde çalışan SQL Server Key vault'tan bir asimetrik anahtar kullanabilirsiniz. Yapılandırma adımları, SQL veritabanı'nda asimetrik bir anahtar kullanarak farklıdır. Daha fazla bilgi için [Azure Key Vault (SQL Server) kullanılarak genişletilebilir anahtar yönetimi](https://docs.microsoft.com/sql/relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server).

## <a name="service-managed-transparent-data-encryption"></a>Hizmetle yönetilen şeffaf veri şifreleme

Azure'da varsayılan saydam veri şifrelemesi için veritabanı şifreleme anahtarını bir yerleşik bir sunucu sertifikası tarafından korunduğunu ayardır. Yerleşik bir sunucu sertifikası her sunucu için benzersizdir. Coğrafi çoğaltma ilişkisinde bir veritabanı olduğundan, birincil ve ikincil coğrafi çoğaltmalı veritabanı birincil veritabanının ana sunucu anahtarı tarafından korunur. İki veritabanı aynı sunucuya bağlandıysanız yerleşik aynı sertifikayı paylaşırlar. Microsoft, bu sertifikalar otomatik olarak en az 90 günde döndürür.

Microsoft ayrıca sorunsuz bir şekilde taşır ve coğrafi çoğaltma için gerektiği şekilde anahtarları yönetir ve geri yükler. 

> [!IMPORTANT]
> Yeni oluşturulan tüm SQL veritabanları, varsayılan olarak hizmetle yönetilen şeffaf veri şifreleme kullanılarak şifrelenir. Var olan veritabanlarını Mayıs 2017 önce ve geri yükleme, coğrafi çoğaltma ve veritabanı kopyalama oluşturulan veritabanları, varsayılan olarak şifrelenmiş değildir.
>

## <a name="bring-your-own-key"></a>Kendi anahtarını Getir

Kendi anahtarınızı getirin desteği ile saydam Veri Şifreleme anahtarlarınızın denetimini ve bunları erişebilen denetimi sürebilir ve ne zaman. Key Vault, Azure bulut tabanlı dış anahtar yönetimi sistemi olduğundan, ilk anahtar yönetimi hizmeti olan kendi anahtarınızı getirin desteği ile saydam veri şifrelemesi tümleştirilmiştir. Kendi anahtarını Getir desteği sayesinde, veritabanı şifreleme anahtarını Key Vault'ta depolanan bir asimetrik anahtar korunur. Asimetrik anahtar, anahtar kasası hiçbir zaman ayrılmaz. Sunucu bir anahtar kasası için izinlere sahip sonra sunucu temel anahtar işlemi istekleri için Key Vault gönderir. Asimetrik anahtar sunucu düzeyinde ayarlamak ve bu sunucu kapsamındaki tüm veritabanları, devralır.

Kendi anahtarını Getir destekle, artık anahtar döndürme ve anahtar kasası izinleri gibi önemli yönetim görevlerinin kontrol edebilirsiniz. Ayrıca anahtarlarını silin ve tüm şifreleme anahtarlarını denetim/raporlamayı etkinleştirmek. Key Vault anahtar merkezi yönetimi sağlar ve sıkı bir şekilde izlenen donanım güvenlik modüllerini kullanır. Key Vault, anahtarları ve yasal uyumluluk gereklerini karşılamak amacıyla veri yönetimi ayrımı yükseltir. Key Vault hakkında daha fazla bilgi için bkz: [Key Vault belgeleri sayfasını](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault).

SQL veritabanı ve veri ambarı için kendi anahtarını Getir desteği ile saydam veri şifrelemesi hakkında daha fazla bilgi için bkz: [kendi anahtarınızı getirin desteği ile saydam veri şifrelemesi](transparent-data-encryption-byok-azure-sql.md).

Kendi anahtarınızı getirin desteği ile saydam veri şifrelemesi'ni kullanmaya başlamak için nasıl yapılır Kılavuzu [PowerShell kullanarak Key vault'tan kendi anahtarınızı saydam veri şifrelemesini açma](transparent-data-encryption-byok-azure-sql-configure.md).

## <a name="move-a-transparent-data-encryption-protected-database"></a>Saydam veri şifrelemesi ile korunan bir veritabanını taşıma

Azure'da işlem veritabanları şifresini gerek yoktur. Kaynak veritabanında veya birincil veritabanında saydam veri şifrelemesini şeffaf bir şekilde hedef devralınır. Dahil edilen işlemi içerir:
- Coğrafi geri yükleme.
- Self Servis-belirli bir noktaya geri yükleme.
- Silinen bir veritabanını geri yükleme.
- Etkin coğrafi çoğaltma.
- Veritabanı kopyasını oluşturma.

Saydam veri şifrelemesi ile korunan veritabanını dışarı aktarma, dışa aktarılan veritabanının içeriğini şifreli değil. Dışarı aktarılan bu içerik, şifrelenmemiş BACPAC dosyalarında depolanır. BACPAC dosyalarını uygun şekilde koruyun ve yeni veritabanının içeri aktarma tamamlandıktan sonra saydam veri şifrelemesini etkinleştirme emin olun.

Örneğin, bir şirket içi SQL Server örneğinden BACPAC dosyasına dışarı aktarılır, yeni veritabanının içeri aktarılan içeriği otomatik olarak şifrelenir değil. Şirket içi SQL Server örneğine BACPAC dosyasına dışarı aktarılır, benzer şekilde, yeni veritabanı da otomatik olarak şifrelenir değil.

Bunun tek istisnası, için ve bir SQL veritabanından dışarı andır. Yeni veritabanı saydam veri şifrelemesi etkin ancak BACPAC dosyasına şifreli değildir.

## <a name="manage-transparent-data-encryption-in-the-azure-portal"></a>Azure portalında saydam veri şifrelemesini yönetin

Azure portalı üzerinden saydam veri şifrelemesini yapılandırmak için Azure sahibi, katkıda bulunan veya SQL Güvenlik Yöneticisi bağlanması gerekir. 

Saydam veri şifrelemesi veritabanı düzeyinde ayarlarsınız. Bir veritabanı saydam veri şifrelemesini etkinleştirmek için Git [Azure portalında](https://portal.azure.com) ve Azure Yöneticisi veya Katılımcısı hesabınızla oturum açın. Saydam veri şifrelemesi ayarları kullanıcı veritabanınıza altında bulun. Varsayılan olarak hizmetle yönetilen şeffaf veri şifrelemesi kullanılır. Saydam veri şifreleme sertifikasını içeren bir veritabanı sunucusu için otomatik olarak oluşturulur. 

![Hizmetle yönetilen şeffaf veri şifreleme](./media/transparent-data-encryption-azure-sql/service-managed-tde.png)  

Sunucu düzeyinde saydam veri şifrelemesi ana anahtarı olarak da bilinen saydam veri şifrelemesi koruyucu ayarladığınız. Kendi anahtarını Getir destekli saydam veri şifrelemesi kullanma ve veritabanlarınızı Key vault'tan bir anahtar ile koruma için saydam veri şifrelemesi ayarları sunucunuz altında bakın. 

![Kendi anahtarınızı getirin desteği ile saydam veri şifrelemesi](./media/transparent-data-encryption-azure-sql/tde-byok-support.png) 

## <a name="manage-transparent-data-encryption-by-using-powershell"></a>Saydam veri şifrelemesi, PowerShell kullanarak yönetme

Saydam veri şifrelemesi PowerShell üzerinden yapılandırmak için Azure sahibi, katkıda bulunan veya SQL Güvenlik Yöneticisi bağlanması gerekir. 

| Cmdlet | Açıklama |
| --- | --- |
| [Set-AzureRmSqlDatabaseTransparentDataEncryption](/powershell/module/azurerm.sql/set-azurermsqldatabasetransparentdataencryption) |Etkinleştirir veya bir veritabanı için saydam veri şifrelemesi devre dışı bırakır|
| [Get-Azure-Rm-Sql-Database-Transparent-Data-Encryption](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryption) |Bir veritabanı için saydam veri şifreleme durumunu alır |
| [Get-Azure-Rm-Sql-Database-Transparent-Data-Encryption-Activity](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryptionactivity) |Bir veritabanı şifreleme ilerleme durumunu denetler |
| [AzureRmSqlServerKeyVaultKey ekleyin](/powershell/module/azurerm.sql/add-azurermsqlserverkeyvaultkey) |Bir Key Vault anahtarı bir SQL Server örneğine ekler. |
| [Get-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/get-azurermsqlserverkeyvaultkey) |Bir SQL server örneği için anahtar kasası anahtarlarını alır  |
| [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) |Saydam veri şifrelemesi koruyucu bir SQL Server örneği için ayarlar |
| [Get-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/get-azurermsqlservertransparentdataencryptionprotector) |Saydam veri şifrelemesi koruyucu alır |
| [Remove-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/remove-azurermsqlserverkeyvaultkey) |Bir Key Vault anahtarı bir SQL Server örneğinden kaldırır. |
|  | |

## <a name="manage-transparent-data-encryption-by-using-transact-sql"></a>Saydam veri şifrelemesi, Transact-SQL kullanarak yönetme

Bir yönetici ya da üyesi olan bir oturum açma kullanarak veritabanına bağlanma **dbmanager** asıl veritabanı rolü.

| Komut | Açıklama |
| --- | --- |
| [ALTER DATABASE (Azure SQL veritabanı)](/sql/t-sql/statements/alter-database-azure-sql-database) | Şifreleme Kapat AYARLAMAK şifreler veya bir veritabanı şifresini çözer. |
| [sys.dm_database_encryption_keys](/sql/relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql) |Şifreleme anahtarlarını bir veritabanı ve onun ilişkili veritabanı şifreleme durumu hakkında bilgi döndürür |
| [sys.dm_pdw_nodes_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql) |Her bir veri şifreleme durumunu hakkında bilgi verir ambarı düğümü ve onun ilişkili veritabanı şifreleme anahtarları | 
|  | |

Transact-SQL kullanarak Key Vault'tan bir anahtar için saydam veri şifrelemesi koruyucu geçemezsiniz. PowerShell veya Azure portalını kullanın.

## <a name="manage-transparent-data-encryption-by-using-the-rest-api"></a>Saydam veri şifrelemesi, REST API kullanarak yönetme
 
REST API aracılığıyla saydam veri şifrelemesini yapılandırmak için Azure sahibi, katkıda bulunan veya SQL Güvenlik Yöneticisi bağlanması gerekir. 

| Komut | Açıklama |
| --- | --- |
|[Oluşturma veya güncelleştirme sunucusu](/rest/api/sql/servers/createorupdate)|Azure Active Directory kimliği (anahtar Kasası'na erişim vermek için kullanılır) bir SQL Server örneğine ekler|
|[Sunucu anahtarı oluşturma veya güncelleştirme](/rest/api/sql/serverkeys/createorupdate)|Bir Key Vault anahtarı bir SQL Server örneğine ekler.|
|[Sunucu anahtarını Sil](/rest/api/sql/serverkeys/delete)|Bir Key Vault anahtarı bir SQL Server örneğinden kaldırır.|
|[Sunucu anahtarları alma](/rest/api/sql/serverkeys/get)|Bir SQL Server örneğinden belirli bir Key Vault anahtarı alır|
|[Sunucu tarafından Server anahtarlarını Listele](/rest/api/sql/serverkeys/listbyserver)|SQL Server örneği için anahtar kasası anahtarlarını alır |
|[Şifreleme koruyucusu güncelle](/rest/api/sql/encryptionprotectors/createorupdate)|Saydam veri şifrelemesi koruyucu bir SQL Server örneği için ayarlar|
|[Şifreleme koruyucusunu alın](/rest/api/sql/encryptionprotectors/get)|Saydam veri şifrelemesi koruyucu bir SQL Server örneğini alır.|
|[Sunucu şifreleme Koruyucularıyla listesi](/rest/api/sql/encryptionprotectors/listbyserver)|Saydam veri şifrelemesi koruyucu bir SQL Server örneğini alır. |
|[Saydam veri şifreleme yapılandırması güncelle](/rest/api/sql/transparentdataencryptions/createorupdate)|Etkinleştirir veya bir veritabanı için saydam veri şifrelemesi devre dışı bırakır|
|[Saydam veri şifrelemesi yapılandırmasını alma](/rest/api/sql/transparentdataencryptions/get)|Bir veritabanı için saydam veri şifreleme yapılandırması alır|
|[Liste saydam veri şifreleme yapılandırma sonuçları](/rest/api/sql/transparentdataencryptionactivities/ListByConfiguration)|Bir veritabanı için şifreleme sonucunu alır|

## <a name="next-steps"></a>Sonraki adımlar

- Saydam veri şifrelemesi genel bir açıklaması için bkz. [saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption).
- SQL veritabanı ve veri ambarı için kendi anahtarını Getir desteği ile saydam veri şifrelemesi hakkında daha fazla bilgi için bkz: [kendi anahtarınızı getirin desteği ile saydam veri şifrelemesi](transparent-data-encryption-byok-azure-sql.md).
- Kendi anahtarınızı getirin desteği ile saydam veri şifrelemesi'ni kullanmaya başlamak için nasıl yapılır Kılavuzu [PowerShell kullanarak Key vault'tan kendi anahtarınızı saydam veri şifrelemesini açma](transparent-data-encryption-byok-azure-sql-configure.md).
- Key Vault hakkında daha fazla bilgi için bkz. [Key Vault belgeleri sayfasını](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault).
