---
title: Azure SQL veritabanı ve veri ambarı için saydam veri şifrelemesi | Microsoft Docs
description: SQL veritabanı ve veri ambarı için saydam veri şifrelemesi genel bakış. Belge, aboneliğin avantajları ve hizmetle yönetilen şeffaf veri şifreleme ve kendi anahtarını Getir içeren yapılandırma seçeneklerini kapsar.
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
ms.date: 01/22/2019
ms.openlocfilehash: b0b4a89aaf9b00b30e6b4759c8aa168f06d0d008
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54462479"
---
# <a name="transparent-data-encryption-for-sql-database-and-data-warehouse"></a>SQL veritabanı ve veri ambarı için saydam veri şifrelemesi

Saydam veri şifrelemesi (TDE), Azure SQL veritabanı, Azure SQL yönetilen örneği ve Azure veri ambarı, kötü amaçlı etkinlik tehditlerine karşı koruma yardımcı olur. Bu gerçek zamanlı şifreleme ve şifre çözme veritabanını, ilişkili yedeklemeler ve işlem günlük dosyaları bekleme sırasında uygulamada değişiklik gerektirmeden gerçekleştirir. Varsayılan olarak, tüm yeni dağıtılan Azure SQL veritabanları için TDE etkin. TDE, mantıksal şifrelemek için kullanılamaz **ana** SQL veritabanı.  **Ana** veritabanı kullanıcı veritabanlarında TDE işlemlerin gerçekleştirilmesi için gereken nesneleri içerir.

TDE Azure SQL yönetilen örnek için Azure SQL veritabanı veya Azure SQL veri ambarı eski veritabanlarını el ile etkinleştirilmesi gerekir.  

Saydam veri şifrelemesi tüm veritabanı depolama, veritabanı şifreleme anahtarı olarak adlandırılan bir simetrik anahtarı'nı kullanarak şifreler. Bu veritabanı şifreleme anahtarı saydam veri şifrelemesi koruyucu tarafından korunur. Koruyucu ya da bir hizmet tarafından yönetiliyor sertifika (hizmetle yönetilen şeffaf veri şifreleme) veya Azure anahtar Kasası'nda (kendi anahtarını Getir) depolanan bir asimetrik anahtar. Azure SQL veritabanı ve veri ambarı için sunucu düzeyinde ve örnek düzeyinde Azure SQL yönetilen örneği için saydam veri şifrelemesi koruyucu ayarlayın. Terim *sunucu* hem sunucusu ve örneği bu belge boyunca farklı belirtilmedikçe ifade eder.

Veritabanı başlatma sırasında şifrelenmiş veritabanı şifreleme anahtarını şifresi çözülür ve şifre çözme ve SQL Server veritabanı altyapısı işlemdeki veritabanı dosyalarını yeniden şifrelenmesi için kullanılır. Saydam veri şifrelemesi, gerçek zamanlı g/ç şifreleme ve şifre çözme veri sayfa düzeyinde gerçekleştirir. Belleğe okuyun ve sonra da yazılmadan önce şifrelenir her sayfanın şifresi çözülür diske. Saydam veri şifrelemesi genel bir açıklaması için bkz. [saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption).

Ayrıca bir Azure sanal makinesinde çalışan SQL Server Key vault'tan bir asimetrik anahtar kullanabilirsiniz. Yapılandırma adımları, SQL veritabanı ve SQL yönetilen örneği, asimetrik bir anahtar kullanarak farklıdır. Daha fazla bilgi için [Azure Key Vault (SQL Server) kullanılarak genişletilebilir anahtar yönetimi](https://docs.microsoft.com/sql/relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server).

## <a name="service-managed-transparent-data-encryption"></a>Hizmetle yönetilen şeffaf veri şifreleme

Azure'da varsayılan saydam veri şifrelemesi için veritabanı şifreleme anahtarını bir yerleşik bir sunucu sertifikası tarafından korunduğunu ayardır. Yerleşik bir sunucu sertifikası her sunucu için benzersizdir. Coğrafi çoğaltma ilişkisinde bir veritabanı olduğundan, birincil ve ikincil coğrafi çoğaltmalı veritabanı birincil veritabanının ana sunucu anahtarı tarafından korunur. İki veritabanı aynı sunucuya bağlandıysanız ayrıca yerleşik aynı sertifikayı paylaşırlar. Microsoft, bu sertifikalar otomatik olarak en az 90 günde döndürür.

Microsoft ayrıca sorunsuz bir şekilde taşır ve coğrafi çoğaltma için gerektiği şekilde anahtarları yönetir ve geri yükler.

> [!IMPORTANT]
> Yeni oluşturulan tüm SQL veritabanları, varsayılan olarak hizmetle yönetilen şeffaf veri şifreleme kullanılarak şifrelenir. Varsayılan olarak Azure SQL yönetilen örnek veritabanları, Mayıs 2017'den önce oluşturulan mevcut SQL veritabanları ve SQL veritabanlarını geri yükleme, coğrafi çoğaltma ve veritabanı kopyalama oluşturulan şifrelenmez.

## <a name="bring-your-own-key"></a>Kendi anahtarını Getir

Kendi anahtarınızı getirin desteği ile saydam Veri Şifreleme anahtarlarınızın denetimini ve bunları erişebilen denetimi sürebilir ve ne zaman. Key Vault, Azure bulut tabanlı dış anahtar yönetimi sistemi olduğundan, ilk anahtar yönetimi hizmeti olan saydam veri şifreleme için kendi anahtarını Getir desteği ile tümleştirilmiştir. Kendi anahtarını Getir desteği sayesinde, veritabanı şifreleme anahtarını Key Vault'ta depolanan bir asimetrik anahtar korunur. Asimetrik anahtar, anahtar kasası hiçbir zaman ayrılmaz. Sunucu bir Key Vault için izinlere sahip sonra sunucu temel anahtar işlemi istekleri için Key Vault gönderir. Sunucu düzeyinde ve tüm asimetrik anahtar kümesi *şifrelenmiş* sunucu altındaki veritabanları devralır.

Kendi anahtarınızı getirin desteği ile anahtar döndürme ve anahtar kasası izinleri gibi önemli yönetim görevlerinin denetler. Ayrıca anahtarlarını silin ve tüm şifreleme anahtarlarını denetim/raporlamayı etkinleştirmek. Key Vault anahtar merkezi yönetimi sağlar ve sıkı bir şekilde izlenen donanım güvenlik modüllerini kullanır. Key Vault, anahtarları ve yasal uyumluluk gereklerini karşılamak amacıyla veri yönetimi ayrımı yükseltir. Key Vault hakkında daha fazla bilgi için bkz: [Key Vault belgeleri sayfasını](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault).

Azure SQL veritabanı, SQL yönetilen örneği ve veri ambarı için kendi anahtarını Getir desteği ile saydam veri şifrelemesi hakkında daha fazla bilgi için bkz: [kendi anahtarınızı getirin desteği ile saydam veri şifrelemesi](transparent-data-encryption-byok-azure-sql.md).

Kendi anahtarınızı getirin desteği ile saydam veri şifrelemesi'ni kullanmaya başlamak için nasıl yapılır Kılavuzu [PowerShell kullanarak Key vault'tan kendi anahtarınızı saydam veri şifrelemesini açma](transparent-data-encryption-byok-azure-sql-configure.md).

## <a name="move-a-transparent-data-encryption-protected-database"></a>Saydam veri şifrelemesi ile korunan bir veritabanını taşıma

Azure'da işlem veritabanları şifresini gerek yoktur. Kaynak veritabanında veya birincil veritabanında saydam veri şifrelemesini şeffaf bir şekilde hedef devralınır. Dahil edilen işlemi içerir:

- Coğrafi Geri Yükleme
- Self Servis-belirli bir noktaya geri yükleme
- Silinen bir veritabanını geri yükleme
- Etkin coğrafi çoğaltma
- Bir veritabanı kopyası oluşturma
- Yedekleme dosyasını Azure SQL yönetilen örneğine geri yükleme

> [!IMPORTANT]
> Şifreleme için kullanılan sertifika erişilebilir olmadığından el ile yalnızca kopya yedekleme TDE yönetilen hizmeti tarafından şifrelenmiş bir veritabanının Azure SQL yönetilen örneği'nde, izin verilmiyor. Bu veritabanı türünde başka bir yönetilen örnek'e taşımak için noktası içinde belirli bir geri yükleme özelliği kullanın.

Saydam veri şifrelemesi ile korunan veritabanını dışarı aktarma, dışa aktarılan veritabanının içeriğini şifreli değil. Dışarı aktarılan bu içerik, şifrelenmemiş BACPAC dosyalarında depolanır. BACPAC dosyalarını uygun şekilde koruyun ve yeni veritabanının içeri aktarma tamamlandıktan sonra saydam veri şifrelemesini etkinleştirme emin olun.

Örneğin, bir şirket içi SQL Server örneğinden BACPAC dosyasına dışarı aktarılır, yeni veritabanının içeri aktarılan içeriği otomatik olarak şifrelenir değil. Şirket içi SQL Server örneğine BACPAC dosyasına dışarı aktarılır, benzer şekilde, yeni veritabanı da otomatik olarak şifrelenir değil.

Bunun tek istisnası, için ve bir SQL veritabanından dışarı andır. Yeni veritabanı saydam veri şifrelemesi etkin ancak BACPAC dosyasına şifreli değildir.

## <a name="manage-transparent-data-encryption-in-the-azure-portal"></a>Azure portalında saydam veri şifrelemesini yönetin

Azure portalı üzerinden saydam veri şifrelemesini yapılandırmak için Azure sahibi, katkıda bulunan veya SQL Güvenlik Yöneticisi bağlanması gerekir.

Veritabanı düzeyinde saydam veri şifrelemesi açıp kapatın. Bir veritabanı saydam veri şifrelemesini etkinleştirmek için Git [Azure portalında](https://portal.azure.com) ve Azure Yöneticisi veya Katılımcısı hesabınızla oturum açın. Saydam veri şifrelemesi ayarları kullanıcı veritabanınıza altında bulun. Varsayılan olarak hizmetle yönetilen şeffaf veri şifrelemesi kullanılır. Saydam veri şifreleme sertifikasını içeren bir veritabanı sunucusu için otomatik olarak oluşturulur. Azure SQL yönetilen örneği için bir veritabanında saydam veri şifrelemesi açıp kapatmak için T-SQL kullanın.

![Hizmetle yönetilen şeffaf veri şifreleme](./media/transparent-data-encryption-azure-sql/service-managed-tde.png)  

Sunucu düzeyinde saydam veri şifrelemesi ana anahtarı olarak da bilinen saydam veri şifrelemesi koruyucu ayarladığınız. Kendi anahtarını Getir destekli saydam veri şifrelemesi kullanma ve veritabanlarınızı Key vault'tan bir anahtar ile koruma için saydam veri şifrelemesi ayarları sunucunuz altında açın.

![Kendi anahtarınızı getirin desteği ile saydam veri şifrelemesi](./media/transparent-data-encryption-azure-sql/tde-byok-support.png)

## <a name="manage-transparent-data-encryption-by-using-powershell"></a>Saydam veri şifrelemesi, PowerShell kullanarak yönetme

Saydam veri şifrelemesi PowerShell üzerinden yapılandırmak için Azure sahibi, katkıda bulunan veya SQL Güvenlik Yöneticisi bağlanması gerekir.

### <a name="cmdlets-for-azure-sql-database-and-data-warehouse"></a>Azure SQL veritabanı ve veri ambarı için cmdlet'leri

Azure SQL veritabanı ve veri ambarı için aşağıdaki cmdlet'leri kullanın:

| Cmdlet | Açıklama |
| --- | --- |
| [Set-AzureRmSqlDatabaseTransparentDataEncryption](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabasetransparentdataencryption) |Etkinleştirir veya bir veritabanı için saydam veri şifrelemesi devre dışı bırakır|
| [Get-AzureRmSqlDatabaseTransparentDataEncryption](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryption) |Bir veritabanı için saydam veri şifreleme durumunu alır |
| [Get-AzureRmSqlDatabaseTransparentDataEncryptionActivity](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryptionactivity) |Bir veritabanı şifreleme ilerleme durumunu denetler |
| [Add-AzureRmSqlServerKeyVaultKey](https://docs.microsoft.com/powershell/module/azurerm.sql/add-azurermsqlserverkeyvaultkey) |Bir Key Vault anahtarı bir SQL Server örneğine ekler. |
| [Get-AzureRmSqlServerKeyVaultKey](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlserverkeyvaultkey) |Bir Azure SQL veritabanı sunucusu için anahtar kasası anahtarlarını alır  |
| [Set-AzureRmSqlServerTransparentDataEncryptionProtector](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) |Saydam veri şifrelemesi koruyucu bir SQL Server örneği için ayarlar |
| [Get-AzureRmSqlServerTransparentDataEncryptionProtector](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlservertransparentdataencryptionprotector) |Saydam veri şifrelemesi koruyucu alır |
| [Remove-AzureRmSqlServerKeyVaultKey](https://docs.microsoft.com/powershell/module/azurerm.sql/remove-azurermsqlserverkeyvaultkey) |Bir Key Vault anahtarı bir SQL Server örneğinden kaldırır. |
|  | |

> [!IMPORTANT]
> Azure SQL yönetilen örnek için T-SQL kullanma [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-azure-sql-database) saydam veri şifrelemesi açıp bir veritabanı düzeyinde etkinleştirmek ve denetlemek için komut [PowerShell betik örneği](transparent-data-encryption-byok-azure-sql-configure.md) saydam verileri yönetmek için Örnek düzeyinde şifreleme sağlar.

## <a name="manage-transparent-data-encryption-by-using-transact-sql"></a>Saydam veri şifrelemesi, Transact-SQL kullanarak yönetme

Bir yönetici ya da üyesi olan bir oturum açma kullanarak veritabanına bağlanma **dbmanager** asıl veritabanı rolü.

| Komut | Açıklama |
| --- | --- |
| [ALTER DATABASE (Azure SQL veritabanı)](https://docs.microsoft.com/sql/t-sql/statements/alter-database-azure-sql-database) | Şifreleme Kapat AYARLAMAK şifreler veya bir veritabanı şifresini çözer. |
| [sys.dm_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql) |Şifreleme anahtarlarını bir veritabanı ve onun ilişkili veritabanı şifreleme durumu hakkında bilgi döndürür |
| [sys.dm_pdw_nodes_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql) |Her bir veri şifreleme durumunu hakkında bilgi verir ambarı düğümü ve onun ilişkili veritabanı şifreleme anahtarları |
|  | |

Transact-SQL kullanarak Key Vault'tan bir anahtar için saydam veri şifrelemesi koruyucu geçemezsiniz. PowerShell veya Azure portalını kullanın.

## <a name="manage-transparent-data-encryption-by-using-the-rest-api"></a>Saydam veri şifrelemesi, REST API kullanarak yönetme

REST API aracılığıyla saydam veri şifrelemesini yapılandırmak için Azure sahibi, katkıda bulunan veya SQL Güvenlik Yöneticisi bağlanması gerekir.
Azure SQL veritabanı ve veri ambarı için aşağıdaki komutları kümesini kullanın:

| Komut | Açıklama |
| --- | --- |
|[Oluşturma veya güncelleştirme sunucusu](https://docs.microsoft.com/rest/api/sql/servers/createorupdate)|Azure Active Directory kimliği (anahtar Kasası'na erişim vermek için kullanılır) bir SQL Server örneğine ekler|
|[Sunucu anahtarı oluşturma veya güncelleştirme](https://docs.microsoft.com/rest/api/sql/serverkeys/createorupdate)|Bir Key Vault anahtarı bir SQL Server örneğine ekler.|
|[Sunucu anahtarını Sil](https://docs.microsoft.com/rest/api/sql/serverkeys/delete)|Bir Key Vault anahtarı bir SQL Server örneğinden kaldırır.|
|[Sunucu anahtarları alma](https://docs.microsoft.com/rest/api/sql/serverkeys/get)|Bir SQL Server örneğinden belirli bir Key Vault anahtarı alır|
|[Sunucu tarafından Server anahtarlarını Listele](https://docs.microsoft.com/rest/api/sql/serverkeys/listbyserver)|SQL Server örneği için anahtar kasası anahtarlarını alır |
|[Şifreleme koruyucusu güncelle](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/createorupdate)|Saydam veri şifrelemesi koruyucu bir SQL Server örneği için ayarlar|
|[Şifreleme koruyucusunu alın](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/get)|Saydam veri şifrelemesi koruyucu bir SQL Server örneğini alır.|
|[Sunucu şifreleme Koruyucularıyla listesi](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/listbyserver)|Saydam veri şifrelemesi koruyucu bir SQL Server örneğini alır. |
|[Saydam veri şifreleme yapılandırması güncelle](https://docs.microsoft.com/rest/api/sql/transparentdataencryptions/createorupdate)|Etkinleştirir veya bir veritabanı için saydam veri şifrelemesi devre dışı bırakır|
|[Saydam veri şifrelemesi yapılandırmasını alma](https://docs.microsoft.com/rest/api/sql/transparentdataencryptions/get)|Bir veritabanı için saydam veri şifreleme yapılandırması alır|
|[Liste saydam veri şifreleme yapılandırma sonuçları](https://docs.microsoft.com/rest/api/sql/transparentdataencryptionactivities/listbyconfiguration)|Bir veritabanı için şifreleme sonucunu alır|

## <a name="next-steps"></a>Sonraki adımlar

- Saydam veri şifrelemesi genel bir açıklaması için bkz. [saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption).
- Azure SQL veritabanı, Azure SQL yönetilen örneği ve veri ambarı için kendi anahtarını Getir desteği ile saydam veri şifrelemesi hakkında daha fazla bilgi için bkz: [kendi anahtarınızı getirin desteği ile saydam veri şifrelemesi](transparent-data-encryption-byok-azure-sql.md).
- Kendi anahtarınızı getirin desteği ile saydam veri şifrelemesi'ni kullanmaya başlamak için nasıl yapılır Kılavuzu [PowerShell kullanarak Key vault'tan kendi anahtarınızı saydam veri şifrelemesini açma](transparent-data-encryption-byok-azure-sql-configure.md).
- Key Vault hakkında daha fazla bilgi için bkz. [Key Vault belgeleri sayfasını](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault).
