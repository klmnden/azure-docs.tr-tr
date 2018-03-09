---
title: "Azure Active Directory kimlik doğrulama - Azure SQL (genel bakış) | Microsoft Docs"
description: "SQL veritabanı, yönetilen örneğini ve SQL Data Warehouse ile kimlik doğrulaması için Azure Active Directory kullanma hakkında bilgi edinin"
services: sql-database
author: GithubMirek
manager: johammer
ms.service: sql-database
ms.custom: security
ms.topic: article
ms.date: 03/07/2018
ms.author: mireks
ms.openlocfilehash: 9745fef33bf00e7249c1cb550c5164474bf5c3ee
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="use-azure-active-directory-authentication-for-authentication-with-sql-database-managed-instance-or-sql-data-warehouse"></a>SQL veritabanı, yönetilen örneği veya SQL Data Warehouse ile kimlik doğrulaması için Azure Active Directory kimlik doğrulaması kullanma
Azure Active Directory kimlik doğrulama mekanizmasıdır Microsoft Azure SQL veritabanına bağlanma bir ve [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) kimlikleri Azure Active Directory (Azure AD) kullanarak. Azure AD kimlik doğrulaması ile veritabanı kullanıcıları kimlikleri ve diğer Microsoft Hizmetleri tek bir merkezi konumda merkezi olarak yönetebilir. Merkezi kimlik yönetimi, veritabanı kullanıcıları yönetmek için tek bir yer sağlar ve izin Yönetimi basitleştirir. Avantajları şunlardır:

* SQL Server kimlik doğrulaması için bir alternatif sunar.
* Yardımcı veritabanı sunucuları arasında kullanıcı kimlikleri artışı durdurun.
* Tek bir yerde parola döndürme sağlar
* Müşteriler, dış (Azure AD) gruplarını kullanarak veritabanı izinlerini yönetebilir.
* Tümleşik Windows kimlik doğrulaması ve Azure Active Directory tarafından desteklenen kimlik doğrulama başka biçimlerde etkinleştirerek bunu depolanmasını parolaları ortadan kaldırabilirsiniz.
* Azure AD kimlik doğrulaması kapsanan veritabanı kullanıcıları veritabanı düzeyinde kimlikleri doğrulamak için kullanır.
* Azure AD, SQL veritabanına bağlanma uygulamalar için belirteç tabanlı kimlik doğrulamasını destekler.
* Azure AD kimlik doğrulaması, bir yerel Azure Active Directory etki alanı eşitleme olmadan için ADFS (etki alanı Federasyonu) veya yerel kullanıcı/parola kimlik doğrulamasını destekler.  
* Azure AD, Active Directory Evrensel çok faktörlü kimlik doğrulama (MFA) içeren kimlik doğrulaması kullanan SQL Server Management Studio bağlantılarını destekler.  MFA kolay doğrulama seçeneklerini aralıklı güçlü kimlik doğrulaması içerir — telefon araması, SMS mesajı, akıllı kartlar ve PIN ya da mobil uygulama bildirimi. Daha fazla bilgi için bkz: [SSMS desteklemek için SQL Database ve SQL Data Warehouse ile Azure AD MFA](sql-database-ssms-mfa-authentication.md).  

>  [!NOTE]  
>  Bir Azure Active Directory hesabı kullanarak bir Azure VM üzerinde çalışan SQL Server için bağlanması desteklenmiyor. Etki alanı Active Directory hesabı kullanın.  

Yapılandırma adımları yapılandırmak ve Azure Active Directory kimlik doğrulaması kullanmak için aşağıdaki yordamları içerir.

1. Oluşturma ve Azure AD doldurabilirsiniz.
2. İsteğe bağlı: İlişkilendirmek veya şu anda Azure aboneliğinizle ilişkili olan active directory değiştirin.
3. Azure SQL veritabanı sunucusu, yönetilen örneği için bir Azure Active Directory Yöneticisi oluşturmak veya [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
4. İstemci bilgisayarları yapılandırın.
5. Azure AD kimlikleri eşlenen veritabanınızdaki kapsanan veritabanı kullanıcıları oluşturun.
6. Veritabanınız için Azure AD kimlikleri kullanarak bağlanın.

> [!NOTE]
> Oluşturma ve Azure AD doldurmak ve ardından Azure SQL Database, yönetilen örneğini ve SQL Data Warehouse ile Azure AD'yi yapılandırma konusunda bilgi almak için bkz: [yapılandırma Azure AD ile Azure SQL veritabanı](sql-database-aad-authentication-configure.md).
>

## <a name="trust-architecture"></a>Güven mimarisi
Aşağıdaki üst düzey diyagramı Azure SQL veritabanı ile Azure AD kimlik doğrulaması kullanarak çözüm mimarisi özetler. Aynı kavramlar SQL veri ambarı'na uygulanır. Azure AD yerel kullanıcı parolası desteklemek için yalnızca bulut bölümünün ve Azure AD/Azure SQL veritabanı olarak kabul. Federe kimlik doğrulaması (veya kullanıcı/parola için Windows kimlik bilgileri) desteklemek için ADFS bloğu ile iletişim gereklidir. Oklar, iletişimin yollarıyla gösterir.

![aad kimlik doğrulama diyagramı][1]

Aşağıdaki diyagramda, federasyon güven ve bir belirteç göndererek bir veritabanına bağlanmak istemci olanak barındırma ilişkileri gösterir. Belirtecin bir Azure AD tarafından kimlik doğrulaması ve veritabanı tarafından güvenilir. Müşteri 1, yerel kullanıcıları olan bir Azure Active Directory veya bir Azure AD ile Federasyon kullanıcıları temsil edebilir. Müşteri 2 içeri aktarılan kullanıcılar dahil olmak üzere olası bir çözüm temsil eder; Bu örnekte, bir Federasyon Azure Active Directory'den Azure Active Directory ile eşitlenmesini ADFS ile geliyor. Azure AD kimlik doğrulaması kullanarak bir veritabanına erişimi barındırma abonelik Azure AD ile ilişkili olmasını gerektirir anlamak önemlidir. Aynı abonelik, Azure SQL Database veya SQL veri ambarı barındıran SQL Server oluşturmak için kullanılmalıdır.

![Abonelik ilişkisi][2]

## <a name="administrator-structure"></a>Yönetici yapısı
Azure AD kimlik doğrulaması kullanırken, SQL veritabanı sunucusu ve yönetilen örneği iki yönetici hesapları vardır; Özgün SQL Server Yöneticisi ve Azure AD Yöneticisi. Aynı kavramlar SQL veri ambarı'na uygulanır. Yalnızca bir Azure AD hesabına göre yönetici kullanıcı veritabanında ilk yer alan Azure AD veritabanı kullanıcı oluşturabilirsiniz. Azure AD yönetici oturum açma, bir Azure AD kullanıcısının veya bir Azure AD grubu olabilir. Yönetici grubu hesabı olduğunda, SQL Server örneği için birden çok Azure AD yöneticilerin sağlayarak, herhangi bir grup üyesi tarafından kullanılabilir. Grup hesabı yönetici olarak kullanarak, merkezi olarak ekleyip Grup üyeleri Azure AD'de kullanıcıları veya SQL veritabanı izinleri değiştirmeden izin vererek yönetilebilirliği geliştirir. Herhangi bir anda yalnızca bir Azure AD Yöneticisi (kullanıcı veya grup) yapılandırılabilir.

![Yönetici yapısı][3]

## <a name="permissions"></a>İzinler
Yeni kullanıcılar oluşturmak için ihtiyacınız `ALTER ANY USER` veritabanındaki izni. `ALTER ANY USER` İznine herhangi bir veritabanı kullanıcı sahip. `ALTER ANY USER` İzni server yönetici hesabı ve veritabanı kullanıcılarla tarafından tutulan de `CONTROL ON DATABASE` veya `ALTER ON DATABASE` izni bu veritabanı için ve üyeleri tarafından `db_owner` veritabanı rolü.

Azure SQL Database, yönetilen örneği veya SQL Data Warehouse bir kapsanan veritabanı kullanıcısı oluşturmak için veritabanı veya bir Azure AD kimlik kullanarak örneğini bağlanmanız gerekir. İlk kapsanan veritabanı kullanıcı oluşturmak için (veritabanı sahibi) Azure AD yönetici kullanarak veritabanına bağlanmalısınız. Bu, gösterilmiştir [yapılandırma ve SQL Database veya SQL Data Warehouse ile Azure Active Directory kimlik doğrulamasını yönetmek](sql-database-aad-authentication-configure.md). Azure AD yönetim Azure SQL Database veya SQL veri ambarı sunucusu için oluşturulduysa, herhangi bir Azure AD kimlik doğrulaması yalnızca mümkündür. Azure Active Directory Yöneticisi sunucudan kaldırdıysanız, oluşturulan mevcut Azure Active Directory Kullanıcıları daha önce SQL Server içinde artık Azure Active Directory kimlik bilgilerini kullanarak veritabanına bağlanabilir.

## <a name="azure-ad-features-and-limitations"></a>Azure AD özellikleri ve sınırlamaları
Azure AD aşağıdaki üyeleri, Azure SQL server veya SQL Data Warehouse sağlanabilir:

- Yerel üyeleri: Azure AD'de yönetilen etki alanında veya bir müşteri etki alanında oluşturulan üyesi. Daha fazla bilgi için bkz: [kendi etki alanı adını Azure AD'ye ekleme](../active-directory/active-directory-domains-add-azure-portal.md).
- Federasyon etki alanı üyeleri: federe bir etki alanı ile Azure AD'de oluşturulan üyesi. Daha fazla bilgi için bkz: [Microsoft Azure artık Windows Server Active Directory ile Federasyon destekler](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/).
- Yerel veya Federasyon etki alanına üye olan alınan üyelerinden diğer Azure AD.
- Active Directory grupları güvenlik grupları oluşturuldu.

Yönetilen örneği ile ilişkili azure AD sınırlamaları:
- Yalnızca Azure AD yönetim veritabanları oluşturabilir, Azure AD kullanıcıları için tek bir DB kapsamlı ve bu izne sahip değil
- Veritabanı sahipliği:
  - Azure AD asıl veritabanı (YETKİLENDİRME ON veritabanı ALTER) sahipliğini değiştiremez ve sahibi olarak ayarlanamaz.
  - Azure AD yönetici tarafından oluşturulan veritabanları için herhangi bir sahiplik ayarlayın (owner_sid sys.sysdatabases alanıdır 0x1)
- SQL Aracısı, Azure AD ilkeleri kullanarak oturum açıldığında yönetilemez. 
- EXECUTE AS kullanarak Azure AD yönetim özellikleri alınamaz
- DAC bağlantı ile Azure AD ilkeleri desteklenmiyor. 

Bu sistem İşlevler altında Azure AD sorumluları çalıştırıldığında NULL değerleri döndürür:
- `SUSER_ID()`
- `SUSER_NAME(<admin ID>)`
- `SUSER_SNAME(<admin SID>)`
- `SUSER_ID(<admin name>)`
- `SUSER_SID(<admin name>)`

## <a name="connecting-using-azure-ad-identities"></a>Azure AD kimlikleri kullanarak bağlanma

Azure Active Directory kimlik doğrulaması kullanarak Azure AD kimlikleri bir veritabanına bağlanmak için aşağıdaki yöntemler destekler:

* Tümleşik Windows kimlik doğrulaması kullanma
* Bir Azure AD asıl adı ve parola kullanarak
* Uygulama belirteci kimlik doğrulaması kullanma

### <a name="additional-considerations"></a>Diğer konular

* Yönetilebilirlik geliştirmek için ayrılmış bir Azure AD sağlamak önerilen yönetici olarak grup.   
* Yalnızca bir Azure AD Yöneticisi (kullanıcı veya grup) herhangi bir zamanda bir Azure SQL veritabanı sunucusu, yönetilen örneği veya Azure SQL Data Warehouse için yapılandırılabilir.   
* Yalnızca SQL Server için Azure AD yönetici başlangıçta Azure SQL veritabanı sunucusu, yönetilen örneği veya Azure SQL Data Warehouse bir Azure Active Directory hesabı kullanarak bağlanabilir. Active Directory Yöneticisi sonraki Azure AD yapılandırabilirsiniz veritabanı kullanıcılar.   
* 30 saniye olarak bağlantı zaman aşımı ayarını öneririz.   
* SQL Server 2016 Management Studio'yu ve SQL Server veri araçları Visual Studio 2015 (2016 veya sonraki sürüm 14.0.60311.1April) için Azure Active Directory kimlik doğrulamasını destekler. (Azure AD kimlik doğrulaması tarafından desteklenen **SqlServer için .NET Framework veri sağlayıcısı**; en az sürüm .NET Framework 4.6). Bu nedenle en son sürümleri bu araçlar ve veri katmanı uygulamaları (DAC ve. BACPAC) Azure AD kimlik doğrulaması kullanabilirsiniz.   
* [ODBC sürüm 13,1](https://www.microsoft.com/download/details.aspx?id=53339) Azure Active Directory kimlik doğrulaması ancak destekler `bcp.exe` daha eski bir ODBC sağlayıcısını kullandığından Azure Active Directory kimlik doğrulaması kullanarak bağlanamıyor.   
* `sqlcmd` Azure Active Directory kimlik doğrulaması başına 13,1 kullanılabilir sürümüyle destekleyen [Yükleme Merkezi'nden](http://go.microsoft.com/fwlink/?LinkID=825643).   
* SQL Server veri araçları Visual Studio 2015 için en az bir veri Araçları (sürüm 14.0.60311.1) Nisan 2016 sürümünü gerektirir. Şu anda Azure AD kullanıcılarının SSDT nesne Gezgini'nde gösterilmez. Geçici bir çözüm olarak, kullanıcılar görüntülemek [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).   
* [SQL Server için Microsoft JDBC sürücüsü 6.0](https://www.microsoft.com/download/details.aspx?id=11774) destekleyen Azure AD kimlik doğrulama. Ayrıca bkz [bağlantı özelliklerini ayarlama](https://msdn.microsoft.com/library/ms378988.aspx).   
* PolyBase, Azure AD kimlik doğrulama kullanarak kimlik doğrulaması yapamaz.   
* Azure AD kimlik doğrulaması SQL veritabanı için Azure portal tarafından desteklenen **alma veritabanı** ve **verme veritabanı** dikey pencereleri. İçeri ve dışarı aktarma Azure AD kimlik doğrulaması kullanarak PowerShell komutunu da desteklenir.   
* Azure AD kimlik doğrulaması, CLI kullanarak SQL veritabanı, yönetilen örneğini ve SQL Data Warehouse için desteklenir. Daha fazla bilgi için bkz: [yapılandırma ve SQL Database veya SQL Data Warehouse ile Azure Active Directory kimlik doğrulamasını yönetmek](sql-database-aad-authentication-configure.md) ve [SQL Server - az sql server](https://docs.microsoft.com/cli/azure/sql/server).

## <a name="next-steps"></a>Sonraki adımlar
- Oluşturma ve Azure AD doldurmak ve ardından Azure SQL Database veya Azure SQL Data Warehouse ile Azure AD'yi yapılandırma konusunda bilgi almak için bkz: [yapılandırma ve SQL Database, yönetilen örneği veya SQL Data Warehouse ile Azure Active Directory kimlik doğrulaması yönetme ](sql-database-aad-authentication-configure.md).
- SQL Veritabanında erişim ve denetime genel bakış için bkz. [SQL Veritabanında erişim ve denetim](sql-database-control-access.md).
- SQL Veritabanındaki oturum açma bilgileri, kullanıcılar ve veritabanı rollerine genel bakış için bkz. [Oturum açma bilgileri, kullanıcılar ve veritabanı rolleri](sql-database-manage-logins.md).
- Veritabanı sorumluları hakkında daha fazla bilgi için bkz. [Sorumlular](https://msdn.microsoft.com/library/ms181127.aspx).
- Veritabanı rolleri hakkında daha fazla bilgi için bkz. [Veritabanı rolleri](https://msdn.microsoft.com/library/ms189121.aspx).
- SQL Veritabanındaki güvenlik duvarı kuralları hakkında daha fazla bilgi için bkz. [SQL Veritabanı güvenlik duvarı kuralları](sql-database-firewall-configure.md).

<!--Image references-->

[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[9]: ./media/sql-database-aad-authentication/9ad-settings.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/11connect-using-int-auth.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db.png

