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
ms.date: 05/14/2019
ms.openlocfilehash: 7916e9493a5d572f844bca23a1dd7806e5fbe572
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65790155"
---
# <a name="an-overview-of-azure-sql-database-security-capabilities"></a>Azure SQL veritabanı güvenlik özelliklerine genel bakış

Bu makalede, Azure SQL veritabanı'nı kullanarak bir uygulamayı veri katmanının güvenliğini sağlama hakkında temel bilgileri özetlenmektedir. Açıklanan güvenlik stratejisi aşağıdaki resimde gösterildiği gibi savunma katmanlı yaklaşımın izler ve içinde dışarıdan taşır:

![SQL güvenlik layer.png](media/sql-database-security-overview/sql-security-layer.png)

## <a name="network-security"></a>Ağ güvenliği

Microsoft Azure SQL veritabanı, Bulut ve kurumsal uygulamalar için ilişkisel veritabanı hizmetidir. IP adresi veya Azure sanal ağ trafiğini kaynak göre erişimi açıkça verilir kadar müşteri verilerini korumaya yardımcı olmak için güvenlik duvarları ağ erişimi veritabanı sunucusuna engeller.

### <a name="ip-firewall-rules"></a>IP güvenlik duvarı kuralları

IP güvenlik duvarı kurallarını her isteğin kaynak IP adresine göre veritabanlarına erişim verin. Daha fazla bilgi için [güvenlik duvarı kurallarını Azure SQL veritabanına genel bakış ve SQL veri ambarı](sql-database-firewall-configure.md).

### <a name="virtual-network-firewall-rules"></a>Sanal ağ güvenlik duvarı kuralları

[Sanal ağ hizmet uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md) Azure omurga üzerinden sanal ağ bağlantınızı genişletin ve trafiğin kaynaklandığı sanal ağ alt ağı tanımlamak Azure SQL veritabanı'nı etkinleştirin. Azure SQL veritabanına ulaşılamıyor trafiğine izin verecek şekilde SQL kullanmak [hizmet etiketleri](../virtual-network/security-overview.md) ağ güvenlik grupları üzerinden giden trafiğe izin verme.

[Sanal ağ kuralları](sql-database-vnet-service-endpoint-rule-overview.md) yalnızca bir sanal ağ içinde seçilen alt ağlar gönderildiği iletişimi kabul etmek Azure SQL veritabanı'nı etkinleştirin.

> [!NOTE]
> Güvenlik duvarı kuralları ile erişimi denetleme mu *değil* uygulamak **yönetilen örnek**. Ağ yapılandırması hakkında daha fazla bilgi için bkz gerekli [bir yönetilen örneğe bağlanma](sql-database-managed-instance-connect-app.md)

## <a name="access-management"></a>Erişim Yönetimi

> [!IMPORTANT]
> Veritabanları ve Azure içinde veritabanı sunucularını yönetme, portal kullanıcı hesabınızın rol atamaları tarafından denetlenir. Bu makalede hakkında daha fazla bilgi için bkz. [Azure portalında rol tabanlı erişim denetimi](../role-based-access-control/overview.md).

### <a name="authentication"></a>Kimlik Doğrulaması

Kimlik doğrulaması, kullanıcı kanıtlama işlemini kim iddia şeklindedir. Azure SQL veritabanı iki kimlik doğrulaması türünü destekler:

- **SQL kimlik doğrulaması**:

    SQL veritabanı kimlik doğrulaması başvuran bir kullanıcı kimlik doğrulaması için bağlanırken [Azure SQL veritabanı](sql-database-technical-overview.md) kullanıcı adı ve parola kullanarak. Veritabanı için veritabanı sunucusu oluşturma sırasında bir "Sunucu Yöneticisi" oturum açma kullanıcı adı ve parola belirtilmelidir. Bu kimlik bilgilerini kullanarak, "Sunucu Yöneticisi" o veritabanı sunucusundaki herhangi bir veritabanı için veritabanı sahibi olarak kimlik doğrulaması yapabilir. Bundan sonra ek SQL oturumları ve kullanıcıları sunucu yöneticisi tarafından kullanıcı adı ve parola kullanarak bağlanmasına olanak tanıyan oluşturulabilir.

- **Azure Active Directory kimlik doğrulaması**:

    Azure Active Directory kimlik doğrulama mekanizmasıdır bağlanma bir [Azure SQL veritabanı](sql-database-technical-overview.md) ve [SQL veri ambarı](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) kimlikleri Azure Active Directory (Azure AD) kullanarak. Azure AD kimlik doğrulaması, yöneticilerin kimlikleri ve diğer Microsoft Hizmetleri tek bir merkezi konumda yanı sıra veritabanı kullanıcı izinleri merkezi olarak yönetmenize olanak sağlar. Bu, parola depolama küçültme içerir ve merkezi parola dönüşü ilkeleri sağlar.

     Sunucu Yöneticisi olarak adlandırılan **Active Directory Yöneticisi** SQL veritabanı ile Azure AD kimlik doğrulamasını kullanmak için oluşturulması gerekir. Daha fazla bilgi için [SQL veritabanı tarafından Azure Active Directory kimlik doğrulaması kullanarak bağlanma](sql-database-aad-authentication.md). Azure AD kimlik doğrulaması, hem yönetilen hem de Federasyon hesaplarını destekler. Federasyon hesapları Windows kullanıcıları ve grupları Azure AD ile birleştirilmiş bir müşteri etki alanı desteği.

    Ek Azure AD kimlik doğrulama seçenekleri kullanılabilir [Active Directory Evrensel kimlik doğrulaması için SQL Server Management Studio](sql-database-ssms-mfa-authentication.md) bağlantılar dahil olmak üzere [multi-Factor Authentication](../active-directory/authentication/concept-mfa-howitworks.md) ve [ Koşullu erişim](sql-database-conditional-access.md).

> [!IMPORTANT]
> Veritabanları ve Azure içinde sunucularını yönetme, portal kullanıcı hesabınızın rol atamaları tarafından denetlenir. Bu makalede hakkında daha fazla bilgi için bkz. [Azure portalında rol tabanlı erişim denetimi](../role-based-access-control/overview.md). Güvenlik duvarı kuralları ile erişimi denetleme mu *değil* uygulamak **yönetilen örnek**. Üzerinde lütfen şu makaleye bakın [bir yönetilen örneğe bağlanma](sql-database-managed-instance-connect-app.md) gereken ağ yapılandırması hakkında daha fazla bilgi.

## <a name="authorization"></a>Yetkilendirme

Yetkilendirme, bir Azure SQL veritabanındaki bir kullanıcıya atanan izinleri belirtir ve kullanıcı yapmak için verileceğini belirler. İzinleri, kullanıcı hesapları eklenerek denetlenir [veritabanı rolleri](/sql/relational-databases/security/authentication-access/database-level-roles) ve bu rolleri veya belirli kullanıcı verme veritabanı düzeyindeki izinleri atama [nesne düzeyi izinleri](/sql/relational-databases/security/permissions-database-engine). Daha fazla bilgi için [oturumlar ve kullanıcılar](sql-database-manage-logins.md)

En iyi uygulama, gerektiğinde özel roller oluşturun. Kullanıcılar kendi iş işlevi gerçekleştirmek için gereken en az ayrıcalıkla rolüne ekleyin. Doğrudan kullanıcılara izinleri atamayın. Sunucu yönetici hesabı kapsamlı izinlere sahiptir ve yalnızca birkaç yönetim görevlerini kullanıcılara verilecek yerleşik db_owner rolünün üyesidir. Azure SQL veritabanı uygulamaları için kullanıyor [EXECUTE AS](/sql/t-sql/statements/execute-as-clause-transact-sql) çağrılan modülü yürütme bağlamı belirtin veya [uygulama rolleri](/sql/relational-databases/security/authentication-access/application-roles) sınırlı izinlere sahip. Bu uygulama, veritabanına bağlanan bir uygulama uygulama tarafından gereken en az ayrıcalığa sahip olmasını sağlar. Bu en iyi uygulamaları izleyerek, görevlerin ayrılmasını destekler.

### <a name="row-level-security"></a>Satır düzeyi güvenlik

Satır düzeyi güvenlik (örneğin, grup üyeliği veya yürütme bağlamı) Sorguyu yürüten kullanıcının özelliklerine dayanan bir veritabanı tablosundaki satırlara erişimi denetlemek müşterilerin sağlar. Satır düzeyi güvenlik, özel etiket temel güvenlik kavramları uygulamak için de kullanılabilir. Daha fazla bilgi için bkz. [Satır düzeyi güvenlik](/sql/relational-databases/security/row-level-security).

![azure-database-rls.png](media/sql-database-security-overview/azure-database-rls.png)

## <a name="threat-protection"></a>Tehdit koruması

SQL veritabanı denetim ve tehdit algılama özellikleri sağlayarak müşteri verilerini korur.

### <a name="sql-auditing-in-azure-monitor-logs-and-event-hubs"></a>Azure İzleyici günlüklerine ve Event Hubs SQL denetimi

SQL veritabanı denetimi veritabanı etkinliklerini izler ve veritabanı olaylarını bir denetim kaydı tarafından güvenlik standartlarıyla uyumluluğu sürdürmesine yardımcı olur, müşteriye ait Azure depolama hesabında oturum. Denetim, kullanıcıların devam eden veritabanı etkinliklerini izleme yanı sıra analiz ve olası tehditleri veya kötüye kullanım ve güvenlik ihlallerini tespit edildiğinde alınan önlemlerin belirlemek için geçmiş etkinlikleri araştırın sağlar. Daha fazla bilgi için bkz: ile çalışmaya başlama [SQL Database Auditing](sql-database-auditing.md).  

### <a name="advanced-threat-protection"></a>Gelişmiş Tehdit Koruması

Gelişmiş tehdit koruması, olağan dışı davranış ve veritabanı açıklıklarından yararlanmaya yönelik erişim zararlı olabilecek girişimleri algılamak için SQL Server günlüklerini analiz ediyor. Uyarılar, SQL ekleme, olası veri habersiz kalabilir ve deneme yanılma saldırıları zorla veya anomalileri erişim desenleri ayrıcalık yükseltmeleri ve ihlal edilen kimlik bilgilerini yakalamak için kullanmak gibi şüpheli etkinlikler için oluşturulur. Gelen uyarılar görüntülendiğinde [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/)burada kuşkulu etkinliklerin Ayrıntılar verilmiştir ve öneriler için daha fazla araştırma tehdidi azaltmak için Eylemler ile birlikte verilir. Gelişmiş tehdit koruması, ek bir ücret karşılığında sunucu başına etkinleştirilebilir. Daha fazla bilgi için [SQL veritabanı Gelişmiş tehdit koruması ile çalışmaya başlama](sql-database-threat-detection.md).

![azure-database-td.jpg](media/sql-database-security-overview/azure-database-td.jpg)

## <a name="information-protection-and-encryption"></a>Information protection ve şifreleme

### <a name="transport-layer-security-tls-encryption-in-transit"></a>Aktarım Katmanı Güvenliği TLS (şifreleme yolda)

SQL veritabanı ile Hareket halindeki verileri şifreleyerek müşteri verilerini korur [Aktarım Katmanı Güvenliği](https://support.microsoft.com/help/3135244/tls-1-2-support-for-microsoft-sql-server).

SQL Server şifreleme (SSL/TLS), her zaman tüm bağlantılar için zorlar. Bu, tüm veriler "taşıma durumunda" ayarından bağımsız olarak sunucu ve istemci arasında şifrelenir sağlar **şifrele** veya **TrustServerCertificate** bağlantı dizesindeki.

Bir en iyi uygulama olarak, uygulamanızın bağlantı dizesi, öneri şifreli bir bağlantı belirtin ve _**değil**_ sunucu sertifikasına güven. Bu zorlar sunucu sertifikasını doğrulamak için uygulamanızı ve bu nedenle uygulamanızın Orta tür saldırıların ortadaki karşı savunmasız olan engeller.

Örneğin ADO.NET sürücüsü kullanılırken bu aracılığıyla gerçekleştirilir **Encrypt = True** ve **TrustServerCertificate = False**. Bağlantı dizenizi Azure portalından elde doğru ayarları geçersiz olur.

> [!IMPORTANT]
> Bazı Microsoft olmayan sürücüler değil varsayılan olarak TLS kullanın veya TLS daha eski bir sürümünü kullanan unutmayın (< 1.2) çalışabilmesi için. Bu durumda SQL Server hala veritabanınıza bağlanmak sağlar. Ancak, özellikle hassas verileri depoladığınızda, söz konusu sürücüleri ve SQL veritabanı'na bağlanmak için uygulama izin verme, güvenlik riskleri değerlendirmeniz önerilir. 
>
> TLS ve bağlantı hakkında daha fazla bilgi için bkz: [TLS konuları](sql-database-connect-query.md#tls-considerations-for-sql-database-connectivity)

### <a name="transparent-data-encryption-encryption-at-rest"></a>Saydam veri şifrelemesi (şifreleme bekleyen)

[Azure SQL veritabanı için saydam veri şifrelemesi (TDE)](transparent-data-encryption-azure-sql.md) ham dosyalara veya yedekleri çevrimdışı ya da yetkisiz erişimden bekleyen verilerin korunmasına yardımcı olmak için güvenlik katmanı ekler. Veri Merkezi hırsızlık veya donanım ya da disk sürücüleri ve yedekleme bantlarını gibi medya güvenli şekilde elden yaygın senaryolar şunlardır. TDE, uygulama geliştiricilerin mevcut uygulamaları herhangi bir değişiklik gerektirmeyen bir AES şifreleme algoritmasını kullanarak tüm veritabanını şifreler.

Azure'da yeni oluşturulan tüm SQL veritabanları, varsayılan olarak şifrelenir ve veritabanı şifreleme anahtarını bir yerleşik bir sunucu sertifikası tarafından korunur.  Sertifika Bakım ve döndürme hizmet tarafından yönetilen ve kullanıcı müdahalesi gerektirir. Şifreleme anahtarları denetiminizde yapılacak tercih eden müşteriler anahtarları yönetebilir [Azure anahtar kasası](../key-vault/key-vault-secure-your-key-vault.md).

### <a name="key-management-with-azure-key-vault"></a>Azure Key Vault ile anahtar yönetimi

[Kendi anahtarını Getir](transparent-data-encryption-byok-azure-sql.md) (BYOK) desteği [saydam veri şifrelemesi](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE) izin verir, müşterilerin anahtar yönetimi ve döndürme kullanarak sahipliğini [Azure anahtar kasası](../key-vault/key-vault-secure-your-key-vault.md), Azure'un bulut tabanlı Dış anahtar yönetimi sistemi. Anahtar kasası veritabanı erişimi iptal edilirse bir veritabanı kullanılamaz şifresi ve belleğe okuyun. Azure Key Vault merkezi anahtar yönetimi platformu sağlayan, sıkı bir şekilde izlenen donanım güvenlik modülleri (HSM'ler) yararlanır ve güvenlik gereksinimlerini karşılamanıza yardımcı olmak üzere anahtar yönetimi ve veri arasında görevler ayrımı sağlar.

### <a name="always-encrypted-encryption-in-use"></a>(Şifreleme kullanımda) her zaman şifreli

![azure-database-ae.png](media/sql-database-security-overview/azure-database-ae.png)

[Her zaman şifreli](/sql/relational-databases/security/encryption/always-encrypted-database-engine) erişimden belirli veritabanı sütunlarda depolanan hassas verileri korumak için tasarlanan bir özelliktir (örneğin, kredi kartı numaraları, Ulusal Kimlik numaraları veya veri çubuğunda bir _bilmeniz gereken_ olarak). Bu, Veritabanı Yöneticileri veya yönetim görevlerini gerçekleştirebilirsiniz, ancak şifreli sütunlarda belirli verilere erişim gerektiren iş veritabanına erişmek için yetkiniz ayrıcalıklı diğer kullanıcıları içerir. Veriler her zaman şifrelenir, şifrelenmiş veriler için şifreleme anahtarına erişim ile istemci uygulamalar tarafından işlenmek üzere şifresi çözülür anlamına gelir.  Şifreleme anahtarını SQL hiçbir zaman sunulur ve olabilir ya da depolanan [Windows sertifika Store](sql-database-always-encrypted.md) veya [Azure anahtar kasası](sql-database-always-encrypted-azure-key-vault.md).

### <a name="dynamic-data-masking"></a>Dinamik veri maskeleme

![azure-database-ddm.png](media/sql-database-security-overview/azure-database-ddm.png)

SQL veritabanı dinamik veri maskeleme, hassas verilerin görünürlüğünü ayrıcalık sahibi ayrıcalıklı olmayan kullanıcılara gizleyerek sınırlar. Dinamik veri maskeleme otomatik olarak Azure SQL veritabanı'nda hassas olabilecek verileri keşfeder ve uygulama katmanı üzerinde en az etki ile bu alanlar maskelemek için gerçekleştirilebilecek öneriler sunar. Bu özellik, hassas verileri belirlenen veritabanı alanlarına yapılan sorgunun sonuç kümesinde karartır ancak veritabanındaki veriler değişmez. Daha fazla bilgi için [ile SQL veritabanı dinamik veri maskelemeyi kullanmaya başlayın](sql-database-dynamic-data-masking-get-started.md).

## <a name="security-management"></a>Güvenlik yönetimi

### <a name="vulnerability-assessment"></a>Güvenlik açığı değerlendirmesi

[Güvenlik Açığı değerlendirmesi](sql-vulnerability-assessment.md) bir kolayca bulmak, izlemek ve proaktif olarak genel veritabanı güvenliği artırmak amacıyla olası veritabanı güvenlik açıklarını düzeltin yardımcı hizmetini yapılandırmak. Güvenlik Açığı değerlendirmesi (VA) Gelişmiş SQL güvenlik özellikleri için birleştirilmiş bir pakettir gelişmiş veri güvenliği (REKLAM) sunan bir parçasıdır. Güvenlik Açığı değerlendirmesi, erişilebilir ve merkezi SQL REKLAM portalı üzerinden yönetilebilir.

### <a name="data-discovery--classification"></a>Veri bulma ve sınıflandırma

Veri bulma & sınıflandırma (şu anda önizlemede), Azure SQL veritabanı'na bulma, Sınıflandırma, etiketleme ve veritabanlarınızı hassas verileri korumak için gelişmiş özellikler sunar. Rol, bulma ve Sınıflandırma, büyük/küçük harfe duyarlı verilerinizi (iş/finansal, sağlık hizmetleri, kişisel verileri, vb.), kurumsal bilgi koruma stature oynatabilirsiniz. Altyapı olarak hizmet eder:

- (Denetim) izleme gibi çeşitli güvenlik senaryoları ve anormal hassas verilere erişimi üzerinde uyarı.
- Erişimi denetleme ve güvenlik, sağlamlaştırma son derece hassas verileri içeren veritabanı.
- Veri gizliliği standartlarını ve yasal uyumluluk gereksinimlerini karşılamak yardımcı olur.

Daha fazla bilgi için [veri bulma & sınıflandırma ile çalışmaya başlama](sql-database-data-discovery-and-classification.md).

### <a name="compliance"></a>Uyumluluk

Yukarıdaki özelliklerine ve uygulamanızı Azure SQL veritabanı çeşitli güvenlik gereksinimlerini karşılamasına yardımcı olan işlevselliği ek olarak, düzenli olarak denetimden geçmektedir ve karşı bir dizi uyumluluk standardı için belgelendirilmiştir. Daha fazla bilgi için [Microsoft Azure Trust Center](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) burada bulabilirsiniz SQL veritabanı uyumluluk sertifikaları en güncel listesi.

## <a name="next-steps"></a>Sonraki adımlar

- SQL Veritabanındaki erişim denetimi özelliklerinin kullanımı hakkında ayrıntılı bilgi için bkz. [Erişim denetimi](sql-database-control-access.md).
- Veritabanı denetimi, bir açıklaması için bkz. [SQL veritabanı denetimi](sql-database-auditing.md).
- Tehdit algılama için bkz [SQL veritabanı tehdit algılama](sql-database-threat-detection.md).
