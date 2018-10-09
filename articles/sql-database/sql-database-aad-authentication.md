---
title: Azure Active Directory kimlik doğrulama - Azure SQL | Microsoft Docs
description: Azure Active Directory, SQL veritabanı yönetilen örneği ve SQL veri ambarı ile kimlik doğrulaması için kullanma hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: data warehouse
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 10/05/2018
ms.openlocfilehash: 9d4ede9aee5261903a814cc2f29c788a2ba377f3
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2018
ms.locfileid: "48869778"
---
# <a name="use-azure-active-directory-authentication-for-authentication-with-sql"></a>SQL kimlik doğrulaması için Azure Active Directory kimlik doğrulaması kullanın.

Azure Active Directory kimlik doğrulama mekanizmasıdır Azure'a bağlanma bir [SQL veritabanı](sql-database-technical-overview.md) ve [SQL veri ambarı](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) kimlikleri Azure Active Directory (Azure AD) kullanarak. 

> [!NOTE]
> Bu konu başlığı, Azure SQL sunucusunun yanı sıra Azure SQL sunucusu üzerinde oluşturulmuş olan SQL Veritabanı ve SQL Veri Ambarı veritabanları için de geçerlidir. Kolaylık açısından, hem SQL Veritabanı hem de SQL Veri Ambarı için SQL Veritabanı terimi kullanılmaktadır.

Azure AD kimlik doğrulaması ile veritabanı kullanıcılarının kimliklerini ve diğer Microsoft Hizmetleri tek bir merkezi konumda merkezi olarak yönetebilir. Merkezi kimlik yönetimi, veritabanı kullanıcıları yönetmek için tek bir yerde sağlar ve izin yönetimini kolaylaştırır. Avantajları şunlardır:

- Bu SQL Server kimlik doğrulaması için bir alternatif sağlar.
- Yardımcı olur, kullanıcı kimliklerini çoğalan veritabanı sunucuları arasında durdurun.
- Tek bir yerde parola dönüşü sağlar.
- Müşteriler, dış (Azure AD) grupları kullanarak veritabanı izinlerini yönetebilir.
- Tümleşik Windows kimlik doğrulaması ve diğer Azure Active Directory tarafından desteklenen kimlik doğrulama etkinleştirerek bu parolaları ortadan kaldırabilir.
- Azure AD kimlik doğrulaması bağımsız veritabanı kullanıcılarını ve veritabanı düzeyinde kimlikleri doğrulamak için kullanır.
- Azure AD, SQL veritabanına bağlanan uygulamalar için belirteç tabanlı kimlik doğrulamasını destekler.
- Azure AD kimlik doğrulaması için bir yerel Azure Active Directory etki alanı eşitleme olmadan ADFS (etki alanı Federasyon) veya yerel kullanıcı/parola kimlik doğrulamasını destekler.  
- Azure AD Active Directory Evrensel multi-Factor Authentication (MFA) içeren kimlik doğrulaması kullanan SQL Server Management Studio bağlantılarından destekler.  MFA, güçlü kimlik doğrulaması ile çeşitli kolay doğrulama seçenekleri içerir — telefon araması, SMS mesajı, akıllı kartlar ve PIN ya da mobil uygulama bildirimi. Daha fazla bilgi için [SQL veritabanı ve SQL veri ambarı ile Azure AD MFA için SSMS desteği](sql-database-ssms-mfa-authentication.md).  

> [!NOTE]  
> Bir Azure Active Directory hesabı kullanarak bir Azure sanal makinesinde çalışan SQL Server için bağlanması desteklenmiyor. Bunun yerine bir etki alanı Active Directory hesabı kullanın.  

Yapılandırma adımları yapılandırmak ve Azure Active Directory kimlik doğrulamasını kullanmak için aşağıdaki yordamları içerir.

1. Oluşturma ve Azure AD doldurabilirsiniz.
2. İsteğe bağlı: İlişkilendirmek veya şu anda Azure aboneliğinizle ilişkili olan active directory değiştirin.
3. Yönetilen örnek, Azure SQL veritabanı sunucusu için bir Azure Active Directory Yöneticisi oluşturma veya [Azure SQL veri ambarı](https://azure.microsoft.com/services/sql-data-warehouse/).
4. İstemci bilgisayarlarınızın yapılandırın.
5. Azure AD kimlikleri için eşlenmiş veritabanında bağımsız veritabanı kullanıcılarını oluşturun.
6. Azure AD kimlikleri kullanarak veritabanınıza bağlanın.

> [!NOTE]
> Oluşturup Azure AD'ye doldurun ve sonra Azure SQL veritabanı yönetilen örneği ve SQL veri ambarı ile Azure AD yapılandırma konusunda bilgi almak için bkz: [yapılandırma Azure AD ile Azure SQL veritabanı](sql-database-aad-authentication-configure.md).

## <a name="trust-architecture"></a>Güven mimarisi

Aşağıdaki üst düzey diyagramda Azure SQL veritabanı ile Azure AD kimlik doğrulamasını kullanarak çözüm mimarisi özetler. SQL veri ambarı'na aynı kavramlar geçerlidir. Azure AD yerel kullanıcı parolası desteklemek için yalnızca bulut bölümü ve Azure AD/Azure SQL veritabanı sayılır. Federe kimlik doğrulaması (veya Windows kimlik bilgileri için kullanıcı/parola) desteklemek için ADFS bloğu ile iletişim gereklidir. Oklar, iletişimin yollarla gösterir.

![aad kimlik doğrulama diyagramı][1]

Aşağıdaki diyagramda, federasyon güven ve bir belirteç göndererek bir veritabanına bağlanmak bir istemci izin veren barındırma ilişkileri gösterir. Belirteç bir Azure AD tarafından doğrulanır ve veritabanı tarafından güvenilir. Müşteri 1'yerel kullanıcıların bir Azure Active Directory veya Azure AD ile Federasyon kullanıcıları temsil edebilir. Müşteri 2 içeri aktarılan kullanıcılar dahil olmak üzere, olası bir çözüm temsil eder; Bu örnekte bir Azure Active Directory ile eşitlenmekte olan AD FS ile Federasyon Azure Active Directory geldiğini. Azure AD kimlik doğrulamasını kullanarak bir veritabanına erişimi barındırma aboneliğin Azure AD ile ilişkili olduğunu gerektiğini anlamak önemlidir. Aynı abonelikte, Azure SQL veritabanı veya SQL veri ambarı barındıran SQL Server'ı oluşturmak için kullanılmalıdır.

![Abonelik ilişkileri][2]

## <a name="administrator-structure"></a>Yönetici yapısı

Azure AD kimlik doğrulamasını kullanırken, SQL veritabanı sunucusu ve yönetilen örnek için iki yönetici hesabı vardır. Özgün SQL Server Yöneticisi ve Azure AD Yöneticisi. SQL veri ambarı'na aynı kavramlar geçerlidir. Yalnızca bir Azure AD hesabına göre yönetici bir kullanıcı veritabanına ilk yer alan Azure AD veritabanı kullanıcısı oluşturabilirsiniz. Azure AD yönetici oturum açma, bir Azure AD kullanıcısı veya Azure AD grubu olabilir. Yönetici grubu hesabı, birden çok Azure AD Yöneticisi SQL Server örneği için etkinleştirme, herhangi bir grup üyesi tarafından kullanılabilir. Grup hesabı yönetici olarak kullanarak, yönetilebilirlik, merkezi olarak ekleyebilir ve kullanıcıları veya SQL veritabanı izinleri değiştirmeden Azure AD'de grubu üyelerini kaldırma olanak tanıyarak geliştirir. Herhangi bir anda yalnızca bir Azure AD Yöneticisi (kullanıcı veya grup) yapılandırılabilir.

![Yönetici yapısı][3]

## <a name="permissions"></a>İzinler

Yeni kullanıcılar oluşturmak için olmalıdır `ALTER ANY USER` veritabanı. `ALTER ANY USER` Herhangi bir veritabanı kullanıcısı için izin verilebilir. `ALTER ANY USER` İzni server yönetici hesabı ve veritabanı kullanıcılarıyla tarafından tutulan ayrıca `CONTROL ON DATABASE` veya `ALTER ON DATABASE` bu veritabanı için ve üyeleri tarafından izni `db_owner` veritabanı rolü.

Azure SQL veritabanı yönetilen örneği veya SQL veri ambarı bağımsız veritabanı kullanıcısı oluşturmak için veritabanı veya örnek bir Azure AD kimlik kullanarak bağlanmanız gerekir. İlk bağımsız veritabanı kullanıcısı oluşturmak için (veritabanı sahibi olan) bir Azure AD Yöneticisi'ni kullanarak veritabanına bağlanmalısınız. Bu gösterilmiştir [yapılandırma ve SQL veritabanı veya SQL veri ambarı ile Azure Active Directory kimlik doğrulamasını yönetmek](sql-database-aad-authentication-configure.md). Azure AD Yöneticisi, Azure SQL veritabanı veya SQL veri ambarı sunucusu için oluşturulduysa, herhangi bir Azure AD kimlik doğrulaması yalnızca mümkündür. Azure Active Directory Yöneticisi sunucudan kaldırdıysanız, oluşturulan mevcut Azure Active Directory Kullanıcıları daha önce SQL Server içinde artık, Azure Active Directory kimlik bilgilerini kullanarak veritabanına bağlanabilirsiniz.

## <a name="azure-ad-features-and-limitations"></a>Azure AD özellikler ve sınırlamalar

Azure AD aşağıdaki üyeleri, Azure SQL server veya SQL veri ambarı sağlanabilir:

- Yerel üyeleri: yönetilen etki alanında veya bir müşteri etki alanında Azure AD'de oluşturulan bir üyesi. Daha fazla bilgi için [kendi etki alanı adınızı Azure AD'ye ekleme](../active-directory/active-directory-domains-add-azure-portal.md).
- Federasyon etki alanı üyesi: federe bir etki alanı ile Azure AD'de oluşturulan bir üyesi. Daha fazla bilgi için [Microsoft Azure artık Windows Server Active Directory ile Federasyonu destekliyor](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/).
- Yerel veya Federasyon etki alanına üye olan içeri aktarılan üyelerinden diğer Azure AD'nin.
- Active Directory grupları güvenlik grupları oluşturulur.

Yönetilen örneği ile ilişkili azure AD sınırlamaları:

- Yalnızca Azure AD Yöneticisi, veritabanları oluşturabilir, Azure AD kullanıcıları için tek bir veritabanı kapsamlı ve bu izne sahip değil
- Veritabanı sahibi:
  - Azure AD sorumlusu ' % s'veritabanı (YETKİLENDİRME üzerinde veritabanı ALTER) sahipliğini değiştiremezsiniz ve sahibi olarak ayarlanamaz.
  - Azure AD Yöneticisi tarafından oluşturulan veritabanları için herhangi bir sahiplik ayarlanır (owner_sid sys.sysdatabases alanıdır 0x1).
- SQL Aracısı, Azure AD sorumlusu kullanarak oturum açıldığında yönetilemez.
- Azure AD Yöneticisi EXECUTE AS kullanarak bürünülemez.
- DAC bağlantı ile Azure AD sorumlusu desteklenmiyor.

Bu sistem İşlevler, Azure AD sorumlusu altında yürütüldüğünde NULL değerler döndürür:

- `SUSER_ID()`
- `SUSER_NAME(<admin ID>)`
- `SUSER_SNAME(<admin SID>)`
- `SUSER_ID(<admin name>)`
- `SUSER_SID(<admin name>)`

## <a name="connecting-using-azure-ad-identities"></a>Azure AD kimlikleri kullanarak bağlanma

Azure Active Directory kimlik doğrulaması, Azure AD kimlikleri kullanarak bir veritabanına bağlanma aşağıdaki yöntemleri destekler:

- Tümleşik Windows kimlik doğrulaması kullanma
- Azure AD sorumlusu adı ve parola kullanarak
- Uygulama belirteci kimlik doğrulamasını kullanma

### <a name="additional-considerations"></a>Diğer konular

- Yönetilebilirlik geliştirmek için adanmış bir Azure AD sağlama öneririz yönetici olarak grup.   
- Yalnızca bir Azure AD Yöneticisi (kullanıcı veya grup) herhangi bir zamanda bir Azure SQL veritabanı sunucusu, yönetilen örneği veya Azure SQL veri ambarı için yapılandırılabilir.
- Yalnızca SQL Server için Azure AD Yöneticisi, başlangıçta Azure SQL veritabanı sunucusu, yönetilen örneği veya Azure Active Directory hesabını kullanarak Azure SQL veri ambarı bağlanabilirsiniz. Active Directory Yöneticisi sonraki Azure AD'yi yapılandırabilirsiniz veritabanı kullanıcılar.   
- 30 saniye olarak bağlantı zaman aşımı ayarını öneririz.   
- SQL Server 2016 Management Studio ve Visual Studio 2015 (sürüm 14.0.60311.1April 2016 veya üzeri) için SQL Server veri araçları, Azure Active Directory kimlik doğrulamasını destekler. (Azure AD kimlik doğrulaması tarafından desteklenen **SqlServer için .NET Framework veri sağlayıcısı**; en az .NET Framework 4.6 sürümü). Bu nedenle en son sürümleri bu araçlar ve veri katmanı uygulamaları (DAC ve. BACPAC), Azure AD kimlik doğrulaması kullanabilirsiniz.   
- [ODBC sürüm 13.1](https://www.microsoft.com/download/details.aspx?id=53339) ancak Azure Active Directory kimlik doğrulamasını destekleyen `bcp.exe` daha eski bir ODBC sağlayıcısını kullandığından, Azure Active Directory kimlik doğrulaması kullanılarak bağlanılamıyor.   
- `sqlcmd` Azure Active Directory kimlik doğrulaması başına 13.1 kullanılabilir sürümüyle destekler [İndirme Merkezi](http://go.microsoft.com/fwlink/?LinkID=825643).
- SQL Server veri araçları, Visual Studio 2015 için en az bir veri Araçları (sürüm 14.0.60311.1) Nisan 2016 sürümünü gerektirir. Şu anda Azure AD kullanıcılarının SSDT nesne Gezgini'nde gösterilmez. Geçici çözüm olarak, kullanıcılar görüntülemek [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).   
- [SQL Server için Microsoft JDBC sürücüsü 6.0](https://www.microsoft.com/download/details.aspx?id=11774) destekleyen Azure AD kimlik doğrulaması. Ayrıca bkz [bağlantı özelliklerini ayarlama](https://msdn.microsoft.com/library/ms378988.aspx).   
- PolyBase, Azure AD kimlik doğrulamasını kullanarak kimlik doğrulaması yapamaz.   
- Azure AD kimlik doğrulaması SQL veritabanı için Azure portal tarafından desteklenen **veritabanını içeri aktar** ve **veritabanı dışarı aktarma** dikey pencereleri. İçeri ve dışarı aktarma Azure AD kimlik doğrulamasını kullanarak PowerShell komutunu da desteklenir.   
- Azure AD kimlik doğrulaması, CLI kullanarak SQL veritabanı yönetilen örneği ve SQL veri ambarı için desteklenir. Daha fazla bilgi için [yapılandırma ve SQL veritabanı veya SQL veri ambarı ile Azure Active Directory kimlik doğrulamasını yönetmek](sql-database-aad-authentication-configure.md) ve [SQL Server - az sql server](https://docs.microsoft.com/cli/azure/sql/server).

## <a name="next-steps"></a>Sonraki adımlar

- Oluşturup Azure AD'ye doldurun ve ardından Azure AD ile Azure SQL veritabanı veya Azure SQL veri ambarı yapılandırma konusunda bilgi almak için bkz: [yapılandırma ve SQL veritabanı, yönetilen örneği veya SQL veri ambarı ile Azure Active Directory kimlik doğrulamasını Yönet ](sql-database-aad-authentication-configure.md).
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
