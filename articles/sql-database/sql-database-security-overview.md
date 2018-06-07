---
title: Azure SQL Veritabanı Güvenliğine Genel Bakış | Microsoft Belgeleri
description: Şirket içi SQL Server ve bulut arasındaki farklar de dahil, Azure SQL Database ve SQL Server güvenliği hakkında bilgi edinin.
services: sql-database
author: giladm
manager: craigg
ms.reviewer: carlrab
ms.service: sql-database
ms.custom: security
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: giladm
ms.openlocfilehash: a40ca715c15540bf7048fae8b5dde152890eb1c1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34648335"
---
# <a name="securing-your-sql-database"></a>SQL Veritabanınızı güvenli hale getirme

Bu makale, Azure SQL Veritabanı kullanan bir uygulamanın veri katmanının güvenliğini sağlamak için yapılması gereken temel işlemleri adım adım açıklamaktadır. Özellikle, bu makalede veri koruma, erişim ve proaktif izleme denetlemek için kaynaklar ile başlamanızı sağlar. 

Tüm SQL türlerindeki güvenlik özelliklerine eksiksiz bir genel bakış için bkz. [SQL Server Veritabanı Altyapısı ve Azure SQL Veritabanı için Güvenlik Merkezi](https://msdn.microsoft.com/library/bb510589). Ek bilgilere [Güvenlik ve Azure SQL Veritabanı teknik incelemesi](https://download.microsoft.com/download/A/C/3/AC305059-2B3F-4B08-9952-34CDCA8115A9/Security_and_Azure_SQL_Database_White_paper.pdf) (PDF) belgesinden de ulaşabilirsiniz.

## <a name="protect-data"></a>Veri koruma

### <a name="encryption"></a>Şifreleme
SQL veritabanı ile Hareket halindeki verileriniz için şifreleme sağlayarak, verilerinizi korur [Aktarım Katmanı Güvenliği](https://support.microsoft.com/kb/3135244), rest ile verileri için [saydam veri şifreleme](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)ve kullanılmakta olan verilerin [ Her zaman şifreli](https://msdn.microsoft.com/library/mt163865.aspx). 

> [!IMPORTANT]
>Veritabanına gelen ve veritabanından giden veriler "taşıma durumunda" olduğu Azure SQL Veritabanına yapılan tüm bağlantılar için şifreleme (SSL/TLS) gerekir. Uygulamanızın bağlantı dizesinde bağlantıyı şifrelemek için parametreler belirtmeniz gerekir ve *değil* (Bu işlem sizin için bağlantı dizenizi Azure portal dışında kopyalarsanız) sunucu sertifikası, aksi takdirde güvenmek için bağlantı sunucusunun kimliğini doğrulamaz ve "man-in--middle" saldırılarına açıktır. Örneğin ADO.NET sürücüsü için bu bağlantı dizesi parametreleri **Encrypt=True** ve **TrustServerCertificate=False** olacaktır. TLS ve bağlantı hakkında daha fazla bilgi için bkz: [TLS konuları](sql-database-connect-query.md#tls-considerations-for-sql-database-connectivity)

Verilerinizi şifrelemek için kullanabileceğiniz diğer yöntemler şunlardır:

* [Hücre düzeyinde şifreleme](https://msdn.microsoft.com/library/ms179331.aspx): Belirli sütunları hatta veri hücrelerini farklı şifreleme anahtarlarıyla şifrelemenizi sağlar.
* Donanım Güvenlik Modülüne veya şifreleme anahtarı hiyerarşinizi bir noktadan yönetmeye ihtiyacınız varsa [Azure VM'de SQL Server ile Azure Anahtar Kasası](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx)'nı kullanın.

### <a name="data-discovery--classification"></a>Veri bulma & sınıflandırma
Veri bulma & sınıflandırma (şu anda önizlemede) bulma, Sınıflandırma, etiketleme ve veritabanınızdaki hassas verileri korumak için Azure SQL veritabanına yerleşik gelişmiş özelliklerini sağlar. Keşfetmek ve utmost önemli verilerinizi sınıflandırmak (iş/Finans, sağlık, PII, vb.), kurumsal bilgi koruma stature bir bileşendirler rol oynayabilir. Altyapı olarak hizmet verebilir:

- (Denetim) izleme gibi çeşitli güvenlik senaryoları ve anormal hassas verilere erişimin uyarma.
- Çok hassas verileri içeren veritabanları erişimini denetleme ve güvenlik, sağlamlaştırma.
- Veri gizliliği standartlarını ve düzenleyici uyumluluk gereksinimleri karşılayan yardımcı olur.

Daha fazla bilgi için bkz: [SQL DB veri bulma & sınıflandırma ile çalışmaya başlama](sql-database-data-discovery-and-classification.md). 

## <a name="control-access"></a>Erişim denetimi
SQL Veritabanı veritabanınıza erişimi sınırlamak için güvenlik duvarı kurallarını, kullanıcıların kimliğini doğrulamak için kimlik doğrulama sistemlerini, rol tabanlı üyelikler ve izinler ile veri yetkilendirmeyi, satır düzeyi güvenliği ve dinamik veri maskelemeyi kullanarak verilerinizin güvenliğini sağlar. SQL Veritabanındaki erişim denetimi özelliklerinin kullanımı hakkında ayrıntılı bilgi için bkz. [Erişim denetimi](sql-database-control-access.md).

> [!IMPORTANT]
> Azure’daki veritabanlarının ve mantıksal sunucuların yönetilmesi, portal kullanıcısı hesabınıza atanan rollerle denetlenir. Bu makalede hakkında daha fazla bilgi için bkz: [Azure portalında rol tabanlı erişim denetimi](../role-based-access-control/overview.md).
>

### <a name="firewall-and-firewall-rules"></a>Güvenlik duvarı ve güvenlik duvarı kuralları
Güvenlik duvarları, verilerinizin korunmasına yardımcı olmak üzere [güvenlik duvarı](sql-database-firewall-configure.md) kurallarını kullanarak hangi bilgisayarların izinli olduğu belirtilene kadar veritabanı sunucunuza tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.

### <a name="authentication"></a>Kimlik Doğrulaması
SQL veritabanı kimlik doğrulaması, veritabanına bağlanırken kimliğinizi nasıl kanıtlayacağınızı belirtir. SQL Veritabanı iki kimlik doğrulaması türünü destekler:
* **SQL Kimlik Doğrulaması**: Kullanıcı adı ve parola kullanır. Veritabanınıza ait mantıksal sunucuyu oluşturduktan sonra kullanıcı adı ve parola belirleyerek "sunucu yöneticisi" oturum açma bilgisi oluşturdunuz. Bu kimlik bilgilerini kullanarak veritabanı sahibi veya "dbo" olarak sunucudaki tüm veritabanları için kimlik doğrulamasından geçebilirsiniz. 
* **Azure Active Directory Kimlik Doğrulaması**: Azure Active Directory tarafından yönetilen kimlikleri kullanır, yönetilen ve tümleşik etki alanları ile kullanılabilir. [Mümkün olduğunda](https://msdn.microsoft.com/library/ms144284.aspx) Active Directory kimlik doğrulamasını (tümleşik güvenlik) kullanın. Azure Active Directory Kimlik Doğrulamasını kullanmak istiyorsanız, Azure AD kullanıcılarını ve gruplarını yönetme izni olan "Azure AD yöneticisi" adlı başka bir sunucu yöneticisi daha oluşturmanız gerekir. Bu yönetici normal bir sunucu yöneticisinin gerçekleştirebileceği tüm işlemleri yapabilir. Azure Active Directory kimlik doğrulamasını etkinleştirme amacıyla Azure AD yöneticisi oluşturma adımları için bkz. [Azure Active Directory Kimlik Doğrulaması kullanarak SQL Veritabanına Bağlanma](sql-database-aad-authentication.md).

### <a name="authorization"></a>Yetkilendirme
Yetkilendirme, bir kullanıcının bir Azure SQL Veritabanında gerçekleştirebileceklerini tanımlar ve bu durum, kullanıcı hesabınızın veritabanı rolü üyelikleri ve nesne düzeyi izinleri ile denetlenir. En iyi uygulama olarak, kullanıcılarınıza gerekli olan en düşük ayrıcalıkları tanımanız gerekir. Bağlantı kurmak için kullandığınız sunucu yöneticisi hesabı, veritabanında tüm işlemleri gerçekleştirme yetkisi olan db_owner rolünün üyesidir. Bu hesabı şema yükseltmeleri ve diğer yönetimsel işlemlerde kullanmak üzere saklayın. Uygulamanızdan veritabanına, uygulamanızın ihtiyaç duyduğu en düşük ayrıcalıklarla bağlanmak için daha sınırlı izinlere sahip olan "ApplicationUser" hesabını kullanın.

### <a name="row-level-security"></a>Satır düzeyi güvenlik
Satır Düzeyi Güvenlik, müşterilerin bir veritabanı tablosundaki satırlara erişimi, sorguyu yürüten kullanıcının özelliklerine göre (grup üyeliği veya yürütme bağlamı) denetlemesini sağlar. Daha fazla bilgi için bkz. [Satır düzeyi güvenlik](https://msdn.microsoft.com/library/dn765131).

### <a name="dynamic-data-masking"></a>Dinamik veri maskeleme 
SQL veritabanı dinamik veri maskeleme ayrıcalıklı olmayan kullanıcılara maskeleyerek gizli verilerin açığa sınırlar. Dinamik veri maskeleme otomatik olarak Azure SQL veritabanındaki olası hassas verileri bulur ve bu alanlar, uygulama katmanı üzerinde en az etkiyle maskelemek için uygulanabilir öneriler sağlar. Bu özellik, hassas verileri belirlenen veritabanı alanlarına yapılan sorgunun sonuç kümesinde karartır ancak veritabanındaki veriler değişmez. Daha fazla bilgi için bkz: [SQL veritabanını dinamik veri maskeleme ile çalışmaya başlama](sql-database-dynamic-data-masking-get-started.md).

## <a name="proactive-monitoring"></a>Öngörülebilir izleme
SQL Veritabanı, denetim ve tehdit algılama özellikleriyle verilerinizi korur. 

### <a name="auditing"></a>Denetim
SQL Veritabanı Denetimi, veritabanı etkinliklerini izler ve veritabanı olaylarını Azure Depolama hesabınızdaki bir denetim günlüğüne kaydederek düzenlemelere uyumluluğu sağlamanıza yardımcı olur. Denetim sayesinde devam eden veritabanı etkinliklerini anlayabilir, ayrıca olası tehditleri veya kötüye kullanım ve güvenlik ihlali şüphelerini soruşturmak için geçmiş etkinlikleri analiz edebilir ve araştırabilirsiniz. Daha fazla bilgi için bkz. [SQL Veritabanı Denetimini kullanmaya başlayın](sql-database-auditing.md).  

### <a name="threat-detection"></a>Tehdit algılama
Tehdit algılama, denetleme, erişmek veya veritabanlarını yararlanmak için alışılmadık ve zararlı girişimlerini algılar Azure SQL veritabanı hizmete yerleşik güvenlik Intelligence ek katmanı sağlayarak tamamlar. Kuşkulu etkinlikler, olası güvenlik açıkları ve SQL ekleme saldırıları, yanı sıra hakkında anormal veritabanı erişimi desenleri uyarılırsınız. Tehdit algılama uyarıları, gelen görüntülenebilir [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) ve şüpheli etkinlik ayrıntılarını sağlayın ve eylem araştırmak ve tehdidi azaltmak nasıl öneririz. Tehdit algılama $15/sunucu/ay maliyetleri. İlk 60 gün boyunca ücretsizdir. Daha fazla bilgi için bkz. [SQL Veritabanı Tehdit Algılamayı kullanmaya başlayın](sql-database-threat-detection.md).
 
## <a name="compliance"></a>Uyumluluk
Yukarıdaki özelliklerine ve çeşitli güvenlik, Azure SQL veritabanı da gereksinimlerini uygulamanızı yardımcı olan işlevselliği ek olarak normal denetimleri katılan ve uyumluluk standartlarına çeşitli karşı sertifikalı. Daha fazla bilgi için günceli [SQL Veritabanı uyumluluk sertifikası](https://www.microsoft.com/trustcenter/compliance/complianceofferings/) listesine ulaşabileceğiniz [Microsoft Azure Güven Merkezi](https://azure.microsoft.com/support/trust-center/) sayfasına bakın.


## <a name="security-management"></a>Güvenlik yönetimi

SQL veritabanı, veri güvenliği veritabanı tarar ve Merkezi güvenlik Panoyu kullanarak sağlayarak yönetmenize yardımcı [SQL güvenlik açığı değerlendirmesi](sql-vulnerability-assessment.md).

**Güvenlik Açığı değerlendirmesi**: [SQL güvenlik açığı değerlendirmesi](sql-vulnerability-assessment.md) (halen) önizlemesidir kolay bir Azure SQL veritabanına bulmak, izlemek ve olası veritabanı düzeltebilirsiniz yardımcı olabilecek yerleşik aracı yapılandırmak güvenlik açıkları. Değerlendirme veritabanınızın üzerinde bir güvenlik açığı taraması yürütür ve güvenlik sorunlarını çözün ve veritabanı güvenliğini tıklatılabilir dahil olmak üzere, güvenlik durumu bakışını sunan bir rapor oluşturur. Değerlendirme rapor izni yapılandırmaları, özellik yapılandırmaları ve veritabanı ayarları için kabul edilebilir bir taban çizgisi ayarlayarak, ortamınız için özelleştirilebilir. Bu size yardımcı olabilir:

- Veritabanı tarama raporları gerektiren bir uyumluluk gereksinimini karşılar. 

- Veri gizliliği standartları karşılar. 

- Değişiklikleri izlemek zor olduğu bir dinamik veritabanı ortamı izleyin.

Daha fazla bilgi için bkz: [SQL güvenlik açığı değerlendirmesi](sql-vulnerability-assessment.md).

## <a name="next-steps"></a>Sonraki adımlar

- SQL Veritabanındaki erişim denetimi özelliklerinin kullanımı hakkında ayrıntılı bilgi için bkz. [Erişim denetimi](sql-database-control-access.md).
- Veritabanı denetim bir tartışma için bkz: [SQL veritabanı denetimi](sql-database-auditing.md).
- Tehdit algılama tartışma için bkz [SQL veritabanı tehdit algılama](sql-database-threat-detection.md).
