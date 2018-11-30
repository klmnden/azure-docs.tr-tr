---
title: Azure SQL Veritabanı Güvenliğine Genel Bakış | Microsoft Belgeleri
description: SQL Server şirket içi ve bulut arasındaki farklar dahil olmak üzere, Azure SQL veritabanı ve SQL Server güvenliği hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aliceku
ms.author: aliceku
ms.reviewer: vanto, carlrab, emlisa
manager: craigg
ms.date: 10/22/2018
ms.openlocfilehash: 5bb3dc0245371248b005d642debb5b60026b9f4c
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52635484"
---
# <a name="an-overview-of-azure-sql-database-security-capabilities"></a>Azure SQL veritabanı güvenlik özelliklerine genel bakış

Bu makale, Azure SQL Veritabanı kullanan bir uygulamanın veri katmanının güvenliğini sağlamak için yapılması gereken temel işlemleri adım adım açıklamaktadır. Özellikle, bu makalede, veri koruma, denetimi erişim ve öngörülebilir izleme kaynakları başlamanıza yardımcı olur.

Tüm SQL türlerindeki güvenlik özelliklerine eksiksiz bir genel bakış için bkz. [SQL Server Veritabanı Altyapısı ve Azure SQL Veritabanı için Güvenlik Merkezi](https://msdn.microsoft.com/library/bb510589). Ek bilgilere [Güvenlik ve Azure SQL Veritabanı teknik incelemesi](https://download.microsoft.com/download/A/C/3/AC305059-2B3F-4B08-9952-34CDCA8115A9/Security_and_Azure_SQL_Database_White_paper.pdf) (PDF) belgesinden de ulaşabilirsiniz.

## <a name="protect-data"></a>Veri koruma

### <a name="encryption"></a>Şifreleme

SQL Veritabanı, hareket halindeki verileriniz için [Aktarım Katmanı Güvenliği](https://support.microsoft.com/kb/3135244), bekleyen veriler için [Saydam Veri Şifrelemesi](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) ve kullanılan veriler için [Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx) kullanarak verilerinizi şifreler ve güvenliğini sağlar.

> [!IMPORTANT]
> Azure SQL veritabanı şifreleme (SSL/TLS), her zaman tüm bağlantılar için tüm veriler "taşıma durumunda" şifrelenir, veritabanı ve istemci arasında sağlar zorlar. Bu ayarı bağımsız olarak gerçekleşir **şifrele** veya **TrustServerCertificate** bağlantı dizesindeki.
>
> Uygulamanızın bağlantı dizesinde şifreli bir bağlantı belirttiğinizden emin olun ve *değil* sunucu sertifikasına güven için (Bu için ADO.NET sürücüsünü **Encrypt = True** ve  **TrustServerCertificate = False**). Bu, sunucu ve zorunlu şifreleme doğrulamak amacıyla uygulamanın zorlayarak Orta saldırı ortadaki uygulamanızdan önlemeye yardımcı olur. Bağlantı dizenizi Azure portalından elde doğru ayarları geçersiz olur.
>
> TLS ve bağlantı hakkında daha fazla bilgi için bkz. [TLS konuları](sql-database-connect-query.md#tls-considerations-for-sql-database-connectivity)

Verilerinizi şifrelemek için kullanabileceğiniz diğer yöntemler şunlardır:

- [Hücre düzeyinde şifreleme](https://msdn.microsoft.com/library/ms179331.aspx): Belirli sütunları hatta veri hücrelerini farklı şifreleme anahtarlarıyla şifrelemenizi sağlar.
-  Bir donanım güvenlik modülüne veya Getir bilgisayarınızı kendi anahtarını (BYOK) teknolojisi için saydam veri şifrelemesi gerekiyorsa, kullanmayı [Azure SQL saydam veri şifrelemesi: kendi anahtarını Getir Destek](transparent-data-encryption-byok-azure-sql.md).

### <a name="data-discovery--classification"></a>Veri bulma & sınıflandırma

Veri bulma & sınıflandırma (şu anda önizlemede), Azure SQL veritabanı'na bulma, Sınıflandırma, etiketleme ve veritabanlarınızı hassas verileri korumak için gelişmiş özellikler sunar. Bulma ve sınıflandırma büyük/küçük harfe duyarlı verilerinizi (iş/Finans, sağlık, PII, vb.), kurumsal bilgi koruma stature rol yürütebilirsiniz. Altyapı olarak hizmet eder:

- (Denetim) izleme gibi çeşitli güvenlik senaryoları ve anormal hassas verilere erişimi üzerinde uyarı.
- Erişimi denetleme ve güvenlik, sağlamlaştırma son derece hassas verileri içeren veritabanı.
- Veri gizliliği standartlarını ve yasal uyumluluk gereksinimlerini karşılamak yardımcı olur.

Daha fazla bilgi için [SQL DB veri bulma & sınıflandırma ile çalışmaya başlama](sql-database-data-discovery-and-classification.md).

## <a name="control-access"></a>Erişim denetimi

SQL Veritabanı veritabanınıza erişimi sınırlamak için güvenlik duvarı kurallarını, kullanıcıların kimliğini doğrulamak için kimlik doğrulama sistemlerini, rol tabanlı üyelikler ve izinler ile veri yetkilendirmeyi, satır düzeyi güvenliği ve dinamik veri maskelemeyi kullanarak verilerinizin güvenliğini sağlar. SQL Veritabanındaki erişim denetimi özelliklerinin kullanımı hakkında ayrıntılı bilgi için bkz. [Erişim denetimi](sql-database-control-access.md).

> [!IMPORTANT]
> Azure’daki veritabanlarının ve mantıksal sunucuların yönetilmesi, portal kullanıcısı hesabınıza atanan rollerle denetlenir. Bu makalede hakkında daha fazla bilgi için bkz. [Azure portalında rol tabanlı erişim denetimi](../role-based-access-control/overview.md). Güvenlik duvarı kuralları ile erişimi denetleme mu *değil* uygulamak **Azure SQL veritabanı yönetilen örneği**. Üzerinde lütfen şu makaleye bakın [yönetilen örneğe bağlanma](sql-database-managed-instance-connect-app.md) gereken ağ yapılandırması hakkında daha fazla bilgi.

### <a name="firewall-and-firewall-rules"></a>Güvenlik duvarı ve güvenlik duvarı kuralları

Güvenlik duvarları, verilerinizin korunmasına yardımcı olmak üzere [güvenlik duvarı](sql-database-firewall-configure.md) kurallarını kullanarak hangi bilgisayarların izinli olduğu belirtilene kadar veritabanı sunucunuza tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.

### <a name="authentication"></a>Kimlik Doğrulaması

SQL veritabanı kimlik doğrulaması, veritabanına bağlanırken kimliğinizi nasıl kanıtlayacağınızı belirtir. SQL Veritabanı iki kimlik doğrulaması türünü destekler:

- **SQL kimlik doğrulaması**

  Bu kimlik doğrulama yöntemi, bir kullanıcı adı ve parola kullanır. Veritabanınıza ait mantıksal sunucuyu oluşturduktan sonra kullanıcı adı ve parola belirleyerek "sunucu yöneticisi" oturum açma bilgisi oluşturdunuz. Bu kimlik bilgilerini kullanarak veritabanı sahibi veya "dbo" olarak sunucudaki tüm veritabanları için kimlik doğrulamasından geçebilirsiniz.

- **Azure Active Directory kimlik doğrulaması**

  Bu kimlik doğrulama yöntemi, Azure Active Directory tarafından yönetilen kimlikleri kullanır ve yönetilen ve tümleşik etki alanları için desteklenir. [Mümkün olduğunda](https://msdn.microsoft.com/library/ms144284.aspx) Active Directory kimlik doğrulamasını (tümleşik güvenlik) kullanın. Azure Active Directory Kimlik Doğrulamasını kullanmak istiyorsanız, Azure AD kullanıcılarını ve gruplarını yönetme izni olan "Azure AD yöneticisi" adlı başka bir sunucu yöneticisi daha oluşturmanız gerekir. Bu yönetici normal bir sunucu yöneticisinin gerçekleştirebileceği tüm işlemleri yapabilir. Azure Active Directory kimlik doğrulamasını etkinleştirme amacıyla Azure AD yöneticisi oluşturma adımları için bkz. [Azure Active Directory Kimlik Doğrulaması kullanarak SQL Veritabanına Bağlanma](sql-database-aad-authentication.md).

### <a name="authorization"></a>Yetkilendirme

Yetkilendirme, bir kullanıcının bir Azure SQL Veritabanında gerçekleştirebileceklerini tanımlar ve bu durum, kullanıcı hesabınızın veritabanı rolü üyelikleri ve nesne düzeyi izinleri ile denetlenir. En iyi uygulama olarak, kullanıcılarınıza gerekli olan en düşük ayrıcalıkları tanımanız gerekir. Bağlantı kurmak için kullandığınız sunucu yöneticisi hesabı, veritabanında tüm işlemleri gerçekleştirme yetkisi olan db_owner rolünün üyesidir. Bu hesabı şema yükseltmeleri ve diğer yönetimsel işlemlerde kullanmak üzere saklayın. Uygulamanızdan veritabanına, uygulamanızın ihtiyaç duyduğu en düşük ayrıcalıklarla bağlanmak için daha sınırlı izinlere sahip olan "ApplicationUser" hesabını kullanın.

### <a name="row-level-security"></a>Satır düzeyi güvenlik

Satır düzeyi güvenlik (örneğin, grup üyeliği veya yürütme bağlamı) Sorguyu yürüten kullanıcının özelliklerine dayanan bir veritabanı tablosundaki satırlara erişimi denetlemek müşterilerin sağlar. Daha fazla bilgi için bkz. [Satır düzeyi güvenlik](https://docs.microsoft.com/sql/relational-databases/security/row-level-security).

### <a name="dynamic-data-masking"></a>Dinamik veri maskeleme

SQL veritabanı dinamik veri maskeleme, hassas verilerin görünürlüğünü ayrıcalık sahibi ayrıcalıklı olmayan kullanıcılara gizleyerek sınırlar. Dinamik veri maskeleme otomatik olarak Azure SQL veritabanı'nda hassas olabilecek verileri keşfeder ve uygulama katmanı üzerinde en az etki ile bu alanlar maskelemek için gerçekleştirilebilecek öneriler sunar. Bu özellik, hassas verileri belirlenen veritabanı alanlarına yapılan sorgunun sonuç kümesinde karartır ancak veritabanındaki veriler değişmez. Daha fazla bilgi için [ile SQL veritabanı dinamik veri maskelemeyi kullanmaya başlayın](sql-database-dynamic-data-masking-get-started.md).

## <a name="proactive-monitoring"></a>Öngörülebilir izleme

SQL Veritabanı, denetim ve tehdit algılama özellikleriyle verilerinizi korur.

### <a name="auditing"></a>Denetim

SQL Veritabanı Denetimi, veritabanı etkinliklerini izler ve veritabanı olaylarını Azure Depolama hesabınızdaki bir denetim günlüğüne kaydederek düzenlemelere uyumluluğu sağlamanıza yardımcı olur. Denetim sayesinde devam eden veritabanı etkinliklerini anlayabilir, ayrıca olası tehditleri veya kötüye kullanım ve güvenlik ihlali şüphelerini soruşturmak için geçmiş etkinlikleri analiz edebilir ve araştırabilirsiniz. Daha fazla bilgi için bkz. [SQL Veritabanı Denetimini kullanmaya başlayın](sql-database-auditing.md).  

### <a name="threat-detection"></a>Tehdit algılama

Tehdit algılama, ek bir erişim veritabanı açıklıklarından yararlanmaya yönelik sıra dışı ve zararlı girişimlerini algılar, Azure SQL veritabanı hizmetine yerleşik güvenlik zekası katmanı ekleyerek denetim özelliklerini tamamlar. Şüpheli etkinlikler, potansiyel açıklar, hakkında uyarı almanız ve anormal veritabanı erişim modellerinin yanı sıra SQL ekleme saldırıları. Tehdit algılama uyarıları görüntülenebilir [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) şüpheli etkinlik ayrıntılarını sağlayın ve eylemi araştırmak ve tehdidi azaltmak için önerilir. Tehdit algılama, sunucu/15 ABD Doları/ay maliyeti. İlk 60 gün boyunca ücretsizdir. Daha fazla bilgi için bkz. [SQL Veritabanı Tehdit Algılamayı kullanmaya başlayın](sql-database-threat-detection.md).

## <a name="compliance"></a>Uyumluluk

Yukarıdaki özelliklerine ve uygulamanızı Azure SQL veritabanı çeşitli güvenlik gereksinimlerini karşılamasına yardımcı olan işlevselliği ek olarak, düzenli olarak denetimden geçmektedir ve karşı bir dizi uyumluluk standardı için belgelendirilmiştir. Daha fazla bilgi için günceli [SQL Veritabanı uyumluluk sertifikası](https://www.microsoft.com/trustcenter/compliance/complianceofferings) listesine ulaşabileceğiniz [Microsoft Azure Güven Merkezi](https://azure.microsoft.com/support/trust-center/) sayfasına bakın.

## <a name="security-management"></a>Güvenlik yönetimi

SQL veritabanı, veritabanı tarar ve Güvenlik Merkezi panosunu kullanma sağlayarak, veri güvenliği yönetmenize yardımcı olur [SQL güvenlik açığı değerlendirmesi](sql-vulnerability-assessment.md).

**[SQL güvenlik açığı değerlendirmesi](sql-vulnerability-assessment.md)**  bir kolayca Azure SQL veritabanında yerleşik keşfetmek, izlemek ve olası veritabanı güvenlik açıklarını düzeltin yardımcı olabilecek Aracı'nı yapılandırmak. Değerlendirme, bir güvenlik açığı taraması veritabanınızda yürütür ve güvenlik sorunlarını çözmek ve veritabanı güvenliğinizi artırmak için eyleme dönüştürülebilir adımlar da dahil olmak üzere güvenlik durumunuzu görünürlük sunan bir rapor oluşturur. Değerlendirme raporu, izni yapılandırmaları, özellik yapılandırmaları ve veritabanı ayarları için kabul edilebilir bir temel ayarlayarak, ortamınız için özelleştirilebilir. Bu, size yardımcı olabilir:

- Veritabanı tarama raporlarını gerektiren uyumluluk gereksinimlerini karşılayın.
- Veri gizliliği standartlarını karşılayın.
- Değişiklikleri izlemek zor olduğu bir dinamik veritabanı ortam izleyin.

Daha fazla bilgi için [SQL güvenlik açığı değerlendirmesi](sql-vulnerability-assessment.md).

## <a name="next-steps"></a>Sonraki adımlar

- SQL Veritabanındaki erişim denetimi özelliklerinin kullanımı hakkında ayrıntılı bilgi için bkz. [Erişim denetimi](sql-database-control-access.md).
- Veritabanı denetimi, bir açıklaması için bkz. [SQL veritabanı denetimi](sql-database-auditing.md).
- Tehdit algılama için bkz [SQL veritabanı tehdit algılama](sql-database-threat-detection.md).
