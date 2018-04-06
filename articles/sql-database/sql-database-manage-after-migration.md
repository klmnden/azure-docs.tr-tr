---
title: Geçişten sonra - Azure SQL veritabanını yönetme | Microsoft Docs
description: Azure SQL veritabanı geçiş sonrasında veritabanınızı yönetmeyi öğrenin.
services: sql-database
author: joesackmsft
manager: craigg
ms.service: sql-database
ms.custom: migrate
ms.topic: article
ms.date: 03/16/2018
ms.author: josack
ms.suite: sql
ms.prod_service: sql-database
ms.component: migration
ms.openlocfilehash: 96bc75e15c99897414fad8c138c8a34ef790af21
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="new-dba-in-the-cloud--managing-your-database-in-azure-sql-database"></a>Yeni DBA bulutta – Azure SQL veritabanı, veritabanınızı yönetme

Geleneksel kendi kendine yönetilen bir PaaS ortamı için kendi kendine kontrollü ortam göründüğü biraz zorlamayı taşıma ilk. Uygulama geliştiricisi veya DBA olarak platformun uygulamanızı kullanılabilir, tutmanıza yardımcı olacak kullanıcı, güvenli ve esnek - çekirdek özellikler her zaman bilmek isteyebilirsiniz. Tam olarak bunu yapmak için bu makalede amaçlar. Makaleyi temellerini kaynakları düzenler ve en iyi SQL veritabanı'nın temel işlevleri yönetmek ve uygulamanızın verimli bir şekilde çalışmaya devam etmesi ve bulutta en iyi sonuçlar elde etmek için nasıl kullanılacağı hakkında bazı yönergeler verir. Bu makale için tipik İzleyici olanlar olacaktır kimin: 
- Kullanıcıların uygulamaları için Azure SQL DB'ye, uygulamaları Modernizing – geçişini değerlendiriliyor.
- Kullanıcıların uygulamaları – devam eden geçiş senaryosu geçiş sürecinde olduğundan.
- En son Azure SQL DB – bulutta yeni DBA geçiş tamamladınız.

Bu makalede Azure SQL DB temel özelliklerinden bazıları kolayca yararlanabilirsiniz bir platform anlatılmaktadır. Aşağıda verilmiştir:- 
- İş devamlılığı ve olağanüstü durum kurtarma (BCDR)
- Güvenlik ve uyumluluk
- Akıllı veritabanı izleme ve Bakım
- Veri taşıma


## <a name="business-continuity-and-disaster-recovery-bcdr"></a>İş devamlılığı ve olağanüstü durum kurtarma (BCDR)
İş devamlılığı ve olağanüstü durum kurtarma yeteneklerini işletmenizin bir olağanüstü durumda her zamanki gibi devam etmek etkinleştirin. Olağanüstü durum veritabanı düzeyi olay olabilir (örneğin, birisi yanlışlıkla önemli bir tabloyu bırakır) veya bir veri merkezi düzeyi olay (Bölgesel catastrophe, örneğin bir tsunami). 

### <a name="how-do-i-create-and-manage-backups-on-sql-database"></a>Nasıl oluşturun ve SQL veritabanı yedeklemeleri yönetme?
Azure SQL DB üzerinde yedeklemeleri oluşturmayın ve zorunda değilsiniz olmasıdır. Artık zamanlama, alma ve yedeklemeleri yönetme hakkında endişelenmeniz gerekir böylece SQL veritabanı, sizin için otomatik olarak veritabanlarını yedekler. Platform tam yedekleme haftada yedekleme birkaç saatte ve bir günlük 5 olağanüstü durum kurtarma etkin olduğundan emin olmak üzere dakikada bir ve en az veri kaybı yedekleme değişiklikleri alır. Bir veritabanı oluşturur oluşturmaz ilk tam yedeklemede olur. Bu yedeklemeler "Bekletme dönemi" olarak adlandırılan belirli bir süre için kullanılabilen ve seçtiğiniz performans katmanına göre değişir.  SQL veritabanı kullanarak bu Bekletme dönemi içinde zaman içinde herhangi bir noktaya kadar geri yükleme yeteneği sağlar [noktası zaman Kurtarma (PITR içinde)](sql-database-recovery-using-backups.md#point-in-time-restore).

|Performans katmanı|Gün Bekletme dönemi|
|---|:---:|
|Temel|7|
|Standart|35|
|Premium|35|
|||

Ayrıca, [uzun vadeli bekletme (LTR)](sql-database-long-term-retention.md) özelliği, yedekleme dosyalarını daha uzun bir süre için özellikle, en fazla 10 yıl basılı tutun ve verileri belirtilen süre içinde herhangi bir noktada bu yedeklerden geri olanak tanır. Ayrıca, veritabanı yedeklemeleri bölgesel catastrophe gelen esnekliği emin olmak için bir coğrafi olarak çoğaltılmış depolama tutulur. Ayrıca bu yedeklemelerin saklama dönemi içinde süreyi herhangi bir noktada herhangi bir Azure bölgesine geri yükleyebilirsiniz. Bkz: [iş sürekliliğine genel bakış](sql-database-business-continuity.md).

### <a name="how-do-i-ensure-business-continuity-in-the-event-of-a-datacenter-level-disaster-or-regional-catastrophe"></a>Bir veri merkezi düzeyinde bir olağanüstü durum veya bölgesel catastrophe durumunda iş sürekliliği nasıl emin olursunuz?
Veritabanı yedeklerinizi bölgesel bir olağanüstü durumda emin olmak için bir coğrafi olarak çoğaltılmış depolama alt sistemindeki depolandığından, başka bir Azure bölgesine yedeği geri yükleyebilirsiniz. Bu, coğrafi geri yükleme çağrılır. Bu RPO (kurtarma noktası hedefi) genellikle < 1 saat ve Ekle (tahmini kurtarma zamanı – birkaç dakika ile saat) içindir.

İş açısından önemli veritabanları için Azure SQL DB teklif, aktif coğrafi çoğaltma. Bu temelde yaptığı, coğrafi olarak çoğaltılmış ikincil veritabanının bir kopyasını, özgün başka bir bölgede oluşturmasıdır. Örneğin, veritabanınızı başlangıçta Azure Batı ABD bölgesinde barındırılan ve bölgesel olağanüstü durum esnekliği istiyorsanız. Batı ABD Doğu ABD söylemek için etkin coğrafi çoğaltma veritabanının oluşturursunuz. Batı ABD üzerinde calamity sağlar, Doğu ABD bölgesinde yük devretme gerçekleştirebilirsiniz. Bu veritabanını otomatik olarak üzerinden Doğu ABD ikincil kaybedildiği bir olağanüstü durumda başarısız olduğunu sağlar otomatik yük devretme grubunda yapılandırma daha iyi nedeni. Bu RPO'su < 5 saniye ile Ekle < 30 saniye olur.

Bir otomatik yük devretme grubu yapılandırılmamışsa, uygulamanızın ikincil bir yük devretme başlatın ve etkin olarak için olağanüstü durum izlemek gerekir. En fazla 4 böyle active coğrafi-çoğaltmalar farklı Azure bölgelerinde oluşturabilirsiniz. Daha iyi alır. Bu ikincil etkin coğrafi çoğaltmalı-çoğaltmaların salt okunur erişim için de erişebilirsiniz. Bu uygulama coğrafi olarak dağıtılan senaryo için gecikme süresini azaltmak için çok faydalı olur. 

### <a name="how-does-my-disaster-recovery-plan-change-from-on-premises-to-sql-database"></a>Nasıl my olağanüstü durum kurtarma planı şirket içi SQL veritabanına değişiyor mu?
Özet olarak, geleneksel şirket içi SQL Server Kurulumu, etkin olarak yük devretme kümelemesi, veritabanı yansıtma, işlem çoğaltma, günlük aktarma vb. gibi özellikleri kullanarak, kullanılabilirlik yönetmek ve korumak emin olmak için yedeklemeleri ve yönetmek gerekli İş sürekliliği. Geliştirme ve veritabanı uygulamanızı en iyi duruma getirme ve olabildiğince fazla olağanüstü durum yönetimi konusunda endişe değil SQL veritabanı ile bu platform yönetir. Yedekleme ve olağanüstü durum kurtarma planları yapılandırılmış ve Azure Portalı'nı (veya PowerShell API'lerini kullanarak birkaç komutları) yalnızca birkaç tıklama ile çalışma olabilir. 

Olağanüstü durum kurtarma hakkında daha fazla bilgi için bkz: [Azure SQL Db olağanüstü durum kurtarma 101](https://azure.microsoft.com/blog/azure-sql-databases-disaster-recovery-101/)

## <a name="security-and-compliance"></a>Güvenlik ve uyumluluk
SQL veritabanı güvenlik ve gizlilik çok ciddiye alır. Güvenlik SQL veritabanı içinde veritabanı düzeyinde ve platform düzeyinde kullanılabilir ve birkaç katmanlara kategorilere olduğunda en iyi anlaşılmalıdır. Her katmanda denetlemek ve uygulamanız için en iyi güvenlik sağlamak alın. Katmanları şunlardır:
- Identity & kimlik doğrulaması ([Windows/SQL kimlik doğrulaması ve Azure Active Directory [AAD] kimlik](sql-database-control-access.md)).
- Etkinlik izleme ([denetim](sql-database-auditing.md) ve [tehdit algılama](sql-database-threat-detection.md)).
- Gerçek veri koruma ([saydam veri şifreleme [TDE]](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) ve [[AE]'her zaman şifreli](/sql/relational-databases/security/encryption/always-encrypted-database-engine)). 
- Hassas ve ayrıcalıklı veri erişimini denetleme ([satır düzeyi güvenlik](/sql/relational-databases/security/row-level-security) ve [dinamik veri maskeleme](/sql/relational-databases/security/dynamic-data-masking)).

[Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) Azure, şirket içi ve diğer Bulutları çalışan iş yükleri arasında Merkezi güvenlik yönetimi sunar. Gerekli olup olmadığını görebilirsiniz gibi SQL veritabanı korumasını [denetim](sql-database-auditing.md) ve [saydam veri şifreleme [TDE]](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) tüm kaynaklara yapılandırılır ve kendi gereksinimlerinize göre ilkeleri oluşturun.

### <a name="what-user-authentication-methods-are-offered-in-sql-database"></a>Kullanıcı kimlik doğrulama yöntemleri, SQL veritabanında sunulur?
Vardır [iki kimlik doğrulama yöntemleri](sql-database-control-access.md#authentication) SQL veritabanında sunulan: 
- [Azure Active Directory kimlik doğrulaması](sql-database-aad-authentication.md)
- SQL kimlik doğrulaması. 

Geleneksel windows kimlik doğrulaması desteklenmiyor. Azure Active Directory (AD) merkezi bir kimlik ve erişim yönetimi hizmettir. Bu konuda, çok rahat bir çoklu oturum açma erişim (SSO) için tüm personel, kuruluşunuzda sağlayabilirsiniz. Ne bu kimlik bilgilerini daha basit kimlik doğrulaması için tüm Azure hizmetlerinde paylaşılır anlamına gelir. AAD destekleyen [MFA (çok faktörlü kimlik doğrulamasını)](sql-database-ssms-mfa-authentication.md) ve ile bir [birkaç tıklatmayla](../active-directory/connect/active-directory-aadconnect-get-started-express.md) AAD Windows Server Active Directory ile tümleştirilebilir. SQL kimlik doğrulaması, tam olarak bunu geçmişte kullanmakta olduğunuz gibi çalışır. Bir kullanıcı adı/parola sağlayın ve verilen bir mantıksal sunucu üzerindeki herhangi bir veritabanına kullanıcıların kimliklerini doğrulayabilirsiniz. Bu aynı zamanda SQL Database ve SQL Data Warehouse çok faktörlü kimlik doğrulaması ve Azure AD etki alanı içinde Konuk kullanıcı hesaplarını sunmanızı sağlar. Bir Active Directory şirket içi zaten varsa, dizininiz için Azure genişletmek için Azure Active Directory dizininizle.

|**Varsa...**|**SQL veritabanı / SQL veri ambarı**|
|---|---|
|Azure Active Directory (AD) ile Azure kullanmayı tercih eder|Kullanım [SQL kimlik doğrulaması](sql-database-security-overview.md)|
|SQL Server üzerinde kullanılan AD şirket içi|[Azure AD ile birleştirmek](../active-directory/connect/active-directory-aadconnect.md)ve Azure AD kimlik doğrulaması kullanın. Bu, çoklu oturum açma kullanabilirsiniz.|
|Çok faktörlü kimlik doğrulaması (MFA) zorunlu kılmanız gerekiyorsa|Bir ilke olarak MFA gerektirecek [Microsoft koşullu erişim](sql-database-conditional-access.md)ve [MFA desteği ile Azure AD Evrensel kimlik doğrulaması](sql-database-ssms-mfa-authentication.md).|
|Microsoft hesapları (live.com, outlook.com) veya diğer etki alanları (gmail.com) Konuk hesapları sahip|Kullanım [Azure AD Evrensel kimlik doğrulama](sql-database-ssms-mfa-authentication.md) SQL veritabanı/veri ambarında hangi yararlanır [Azure AD B2B işbirliği](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md).|
|Windows Azure AD kimlik bilgilerinizi kullanarak bir Federasyon etki alanından oturum açmış|Kullanım [Azure AD ile tümleşik kimlik doğrulaması](sql-database-aad-authentication-configure.md).|
|Windows Azure ile Federasyon olmayan bir etki alanı kimlik bilgilerini kullanarak oturum açmış|Kullanım [Azure AD ile tümleşik kimlik doğrulaması](sql-database-aad-authentication-configure.md).|
|Orta katman Hizmetleri, bir SQL veritabanı veya SQL Data Warehouse bağlanmak için gereken|Kullanım [Azure AD ile tümleşik kimlik doğrulaması](sql-database-aad-authentication-configure.md).|
|||

### <a name="how-do-i-limit-or-control-connectivity-access-to-my-database"></a>Nasıl ı sınırlamak denetlemek veya veritabanı bağlantısı erişimi?
Uygulamanız için en iyi bağlantı kuruluş elde etmek için kullanabilirsiniz, elden adresindeki birden çok tekniği vardır. 
- Güvenlik Duvarı Kuralları
- VNET Hizmeti uç noktaları
- Ayrılmış IP’ler

#### <a name="firewall"></a>Güvenlik duvarı
Bir güvenlik duvarı erişim sunucunuza bir dış varlık mantıksal sunucunuza yalnızca belirli varlıkları erişime izin vererek önler. Varsayılan olarak, tüm bağlantıları ve mantıksal Sunucusu'ndaki veritabanları, Azure hizmetlerinden gelen bağlantıları dışında izin verilmiyor. Bir güvenlik duvarı kuralı ile bu bilgisayarın IP adresi güvenlik duvarı aracılığıyla izin vererek onayladığınız yalnızca varlıklara (örneğin, bir geliştirici makine) sunucunuza erişim açabilirsiniz. Ayrıca, bir mantıksal sunucu erişmesine izin vermek istediğiniz IP aralığı belirtmenize olanak sağlar. Örneğin, kuruluşunuzdaki Geliştirici Makine IP adreslerini aynı anda Güvenlik Duvarı Ayarları sayfasında bir aralığı belirterek eklenebilir. 

Sunucu düzeyinde veya veritabanı düzeyinde güvenlik duvarı kuralları oluşturabilirsiniz. Sunucu düzeyinde güvenlik duvarı kuralları SSMS veya portal aracılığıyla oluşturulan ya da kullanabilirsiniz. Bir sunucu ve veritabanı düzeyi güvenlik duvarı kuralı ayarlama hakkında daha fazla bilgi için bkz: [SQL veritabanında güvenlik duvarı kuralları oluşturma](sql-database-security-tutorial.md#create-a-server-level-firewall-rule-in-the-azure-portal).

#### <a name="service-endpoints"></a>Hizmet uç noktaları
Varsayılan olarak, SQL veritabanınız ise "tüm Azure hizmetlerini izin verecek şekilde" yapılandırılmış – herhangi bir sanal makine azure'da anlamına gelir, veritabanına bağlanma girişiminde bulunabilir. Hala bu girişimleri, kimlik doğrulaması zorunda. Tüm Azure IP'leri tarafından erişilebilir olması için veritabanının istediğiniz değil, ancak "tüm Azure hizmetlerini izin ver" devre dışı bırakabilirsiniz. Ayrıca, yapılandırabileceğiniz [VNET hizmet uç noktaları](sql-database-vnet-service-endpoint-rule-overview.md).

Hizmet uç noktaları (SE), kritik Azure kaynaklarınıza yalnızca kendi Azure sanal ağında özel kullanıma izin verir. Bunu yaparak, aslında genel erişim kaynaklarınıza ortadan kaldırır. Azure sanal ağınıza arasındaki trafiği Azure omurga ağı üzerinde kalır. SE zorlamalı tünel paket yönlendirme alın. Sanal ağınız, kuruluşunuzun Internet trafiği ve aynı yol yerine gitmek için Azure hizmeti trafiğinin zorlar. Hizmet uç noktaları ile bu paketlerin akışı doğrudan sanal ağınızdan Azure omurga ağı hizmetine en iyi duruma getirebilirsiniz.

![VNET Hizmeti uç noktaları](./media/sql-database-manage-after-migration/vnet-service-endpoints.png) 

#### <a name="reserved-ips"></a>Ayrılmış IP’ler
Sağlamak için başka bir seçenektir [ayrılmış IP'ler](../virtual-network/virtual-networks-reserved-public-ip.md) , VM'ler ve beyaz liste için VM IP adresleri Server'da belirli bir bu güvenlik duvarı ayarları. Ayrılmış IP'ler atayarak IP adreslerinin değiştirilmesi ile güvenlik duvarı kurallarını güncelleştir gerek kalmadan, sorun kaydedin.

### <a name="what-port-do-i-connect-to-sql-database-on"></a>Hangi bağlantı noktasını SQL veritabanına üzerinde bağlanırım?

Bağlantı noktası 1433. SQL veritabanı, bu bağlantı noktası üzerinden iletişim kurar. Şirket ağı içinde bağlanması için kuruluşunuzun güvenlik duvarı ayarlarında bir giden kuralı eklemeniz gerekir. Bir kılavuz olarak, bağlantı noktası 1433 Azure sınır dışında gösterme kaçının. Azure kullanarak SSMS çalıştırabilirsiniz [Azure RemoteApp](https://www.microsoft.com/cloud-platform/azure-remoteapp-client-apps). Bu, 1433 bağlantı noktasına giden bağlantıları'nı açmak gerektirmez, IP DB RemoteApp yalnızca açık olabilir ve çok faktörlü kimlik doğrulama (MFA) destekler statik.

### <a name="how-can-i-monitor-and-regulate-activity-on-my-server-and-database-in-sql-database"></a>Nasıl izlemek ve etkinlik my server ve SQL veritabanında veritabanı üzerinde Düzenleyen?
#### <a name="sql-database-auditing"></a>SQL veritabanı denetimi
SQL veritabanı ile veritabanı olaylarını izlemek için açık denetim kapatabilirsiniz. [SQL veritabanı denetimi](sql-database-auditing.md) veritabanı olayları kaydeder ve onları bir Azure depolama hesabınızdaki denetim günlük dosyasına yazar. Denetim olası güvenlik ve ilke ihlallerini kavramanıza düşünüyorsanız vb. yönetmeliklere uygunluğu korumanıza özellikle yararlı olur. Bu tanımlama ve denetleme gerekir ve önceden yapılandırılmış raporları ve veritabanınızda gerçekleşen olayların bir özetini almak için bir Pano elde edebilirsiniz temel düşündüğünüz olayları belirli kategorilerini yapılandırmanıza olanak sağlar. Veritabanı düzeyinde veya sunucu düzeyinde bu denetim ilkeleri uygulayabilirsiniz. Sunucu/veritabanı için denetim devre dışı bırakma hakkında bir Kılavuzu'na bakın: [SQL veritabanı denetimi etkinleştirme](sql-database-security-tutorial.md#enable-sql-database-auditing-if-necessary).

#### <a name="threat-detection"></a>Tehdit Algılama
İle [tehdit algılama](sql-database-threat-detection.md), kolayca denetleyerek bulunan güvenlik veya ilke ihlallerini görev olanağı alın. Olası tehditler veya ihlalleri sisteminizde adres Uzman güvenlik olması gerekmez. Tehdit algılama, SQL ekleme algılama gibi bazı yerleşik özellikleri de vardır. SQL ekleme, değiştirme veya verileri ve veritabanı uygulaması genel saldırmak oldukça yaygın yolu tehlikeye bir denemedir. SQL veritabanı tehdit algılama anormal veritabanı erişimi desenleri (örneğin, olağan dışı bir konumdan veya tanınmayan bir asıl tarafından erişim) yanı sıra olası güvenlik açıkları ve SQL ekleme saldırıları algılamak algoritmaları birden çok kez çalışır. Bir tehdit veritabanı üzerinde algılanırsa, güvenlik görevlileri veya diğer atanan Yöneticiler bir e-posta bildirimi alırsınız. Her bir bildirim hakkında daha fazla araştırmak ve tehdidi azaltmak öneriler ve şüpheli Etkinlik ayrıntıları sağlar. Tehdit algılamayı etkinleştirme konusunda bilgi almak için bkz: [etkinleştirmek SQL veritabanı tehdit algılama](sql-database-security-tutorial.md#enable-sql-database-threat-detection). 
### <a name="how-do-i-protect-my-data-in-general-on-sql-database"></a>I verilerimi Genel SQL veritabanındaki korunmasına nasıl?
Şifreleme korumak ve saldırganların önemli verilerinizi güvenli hale getirmek için güçlü bir mekanizma sağlar. Şifrelenmiş verilerinizi hiçbir kullanım için şifre çözme anahtarı olmadan davetsiz ' dir. Bu nedenle, ek bir SQL veritabanında yerleşik güvenlik varolan katmanları üstünde koruma katmanı ekler. SQL veritabanında verilerinizi korumaya gereken iki yön vardır: 
- Çalışmıyorken veri ve günlük dosyalarında, verileri 
- Yürütülen verilerinizi. 

SQL veritabanı'nda, varsayılan olarak, depolama alt sistemi veri ve günlük dosyalarını bekleyen verilerinizi tamamen her zaman üzerinden şifrelenir ve [saydam veri şifreleme [TDE]](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql). Yedeklemelerinizin de şifrelenir. TDE ile bu verilere erişme, uygulama tarafında gerekli değişiklik yoktur. Şifreleme ve şifre çözme şeffaf bir şekilde gerçekleşir; Bu nedenle adı. Yürütülen hassas verilerinizi korumaya ve rest, SQL veritabanı adında bir özellik sağlar [her zaman şifreli (AE)](/sql/relational-databases/security/encryption/always-encrypted-database-engine). AE veritabanınızdaki hassas sütunlar (Veritabanı yöneticileri ve yetkisiz kullanıcılar şifreli olduğu için) şifreleyen istemci tarafı şifreleme biçimidir. Sunucu başından itibaren şifrelenmiş verileri alır. Her zaman şifreli için anahtar de istemci tarafında depolanır, böylece yalnızca yetkili istemcilerin hassas sütunları şifresini çözebilir. Şifreleme anahtarları istemcide depolandığından sunucu ve veri yöneticilerinin hassas verileri göremiyorum. AE uçtan uca, fiziksel diske yetkisiz istemcilerden tablodaki hassas sütun şifreler. DBAs kendi SQL komutlarını bir parçası olarak şifrelenmiş sütunlar sorgulamaya devam edebilmek için AE eşitlik karşılaştırmaları bugün destekler. Her zaman şifreli anahtar deposu seçenekleri, çeşitli gibi kullanılabilir [Azure anahtar kasası](sql-database-always-encrypted-azure-key-vault.md), Windows sertifika deposunda ve yerel donanım güvenlik modülleri.

|**Özellikleri**|**Her zaman şifreli**|**Saydam veri şifreleme**|
|---|---|---|
|**Şifreleme aralık**|Uçtan uca|Çalışmıyorken veri|
|**Veritabanı sunucusu hassas verilere erişebilir**|Hayır|Evet, veri merkezinde kalan veriler için şifreleme olduğundan|
|**İzin verilen T-SQL işlemleri**|Eşitlik karşılaştırması|Tüm T-SQL yüzey alanını kullanılabilir|
|**Bu özelliği kullanmak için gerekli uygulama değişiklikleri**|En az|Çok az|
|**Şifreleme ayrıntı düzeyi**|Sütun düzeyi|Veritabanı düzeyi|
||||

### <a name="how-can-i-limit-access-to-sensitive-data-in-my-database"></a>Nasıl ı my veritabanında hassas bilgilere erişimi sınırlayabilirsiniz?
Her uygulama gizli verilerin belirli bir bit herkese görünür olmasını korunması gereken veritabanı vardır. Kuruluş içinde belirli personel bu verileri görüntülemek için ancak diğerlerinin bu verileri görüntüleyebilir neden olmaması gerekir. Çalışan ücretlerini bir örnektir. Bir yönetici kullanıcının bağlı çalışanları ücret bilgilere erişimi ancak gerekir, ekip üyelerinin tek tek kendi eşleri ücret bilgilere erişimi sahip olmamalıdır. Örneğin, müşterilerin Ssn'ler test veya geliştirme aşamaları sırasında hassas verilerle etkileşim olabilir veri geliştiriciler başka bir senaryodur. Bu bilgileri tekrar geliştiriciye açığa çıkarılması gerekmez. Böyle durumlarda, hassas verilerinizi ya da maskelenecek veya hiç gösterilmeyen gerekir. SQL veritabanı yetkisiz kullanıcıların hassas verileri görüntülenmesini engellemek için iki tür yaklaşım sunmaktadır:

[Dinamik veri maskeleme](sql-database-dynamic-data-masking-get-started.md) ayrıcalıklı olmayan kullanıcılara uygulama katmanı maskeleyerek gizli verilerin açığa sınırlamanıza olanak sağlayan bir veri maskeleme özelliğidir. Maskeleme deseni oluşturan bir maskeleme kuralı tanımlayın (örneğin, yalnızca Göster Ulusal kimliği SSN dört rakamı son: XXX-XX-0000 ve çoğu Xs olarak işaretleyin) ve hangi kullanıcıların maskeleme kuraldan dışlanan belirleme. Maskeleme üzerinde-çalışma sırasında olur ve çeşitli maskeleme işlevlerine çeşitli veri kategorilerini için kullanılabilir. Dinamik veri maskeleme, otomatik olarak veritabanınızda hassas verileri algılamak ve maskeleme uygulayan olanak sağlar.

[Satır düzeyi güvenlik](/sql/relational-databases/security/row-level-security) satır düzeyinde erişimi denetlemenize olanak verir. Yani, belirli (grubu üyeliği veya yürütme bağlamı) sorgu yürütülürken kullanıcıyı temel alarak bir veritabanı tablosundaki satırları gizlenir. Erişim kısıtlaması veritabanı katmanında yerine uygulama mantığınızın basitleştirmek için bir uygulama katmanı üzerinde gerçekleştirilir. Bu satırlar erişebilen sunulmaz olan satırları ve güvenlik ilkesi sonraki tanımlama filtreleme bir filtre koşulu, oluşturarak başlayın. Son olarak, son kullanıcı kendi sorgu çalıştırır ve kullanıcının ayrıcalık bağlı olarak, kısıtlı satırları görüntülemek ya da hiç görmek sorunu yaşıyor.

### <a name="how-do-i-manage-encryption-keys-in-the-cloud"></a>Şifreleme anahtarları bulutta nasıl yönetebilirim?

Anahtar yönetim seçeneklerini (istemci tarafı şifreleme)'her zaman şifrelenir ve saydam veri şifreleme (Bekleyen şifreleme) vardır. Şifreleme anahtarlarını düzenli aralıklarla döndürmeniz önerilir. Döndürme sıklığı, iç kuruluş düzenlemeleri ve uyumluluk gereksinimleri ile uyumlu olmalıdır.

**Saydam veri şifreleme (TDE)**: iki anahtar hiyerarşi içinde TDE – her kullanıcı veritabanındaki verileri sunucu benzersiz bir asimetrik RSA tarafından sırayla şifrelenir bir simetrik AES 256 veritabanı benzersiz bir veritabanı şifreleme anahtarı (DEK) tarafından şifrelenir 2048 ana anahtar. Ana anahtar olabilir ya da yönetilen:
- Otomatik olarak platformu tarafından - SQL veritabanı.
- Veya sizin kullanarak [Azure anahtar kasası](sql-database-always-encrypted-azure-key-vault.md) anahtar deposu olarak.

Varsayılan olarak, ana anahtar saydam veri şifreleme için kolaylık sağlamak için SQL veritabanı hizmeti tarafından yönetilir. Kuruluşunuz ana anahtar üzerinde denetim kullanmak isterseniz, Azure anahtar Vault](sql-database-always-encrypted-azure-key-vault.md) anahtar deposu olarak kullanılacak bir seçenek yoktur. Azure anahtar kasası kullanarak, kuruluşunuz anahtar sağlama, döndürme, izin denetimleri üzerinde denetim varsayar. [Döndürme veya TDE ana anahtar türü değiştirme](/sql/relational-databases/security/encryption/transparent-data-encryption-byok-azure-sql-key-rotation) , yalnızca DEK yeniden şifreler gibi hızlıdır. Güvenlik ve veri yönetimi arasında rollerinin ayırmalı kuruluşlar için bir güvenlik yöneticisi Azure anahtar Kasası'nda TDE ana anahtar için anahtar malzemesi sağlamak ve için kullanılacak veritabanı yöneticisi için bir Azure anahtar kasası anahtar tanımlayıcısını belirtin bir sunucu üzerinde bekleyen şifreleme. Anahtar kasası, Microsoft bakın veya herhangi bir şifreleme anahtarı ayıklamak şekilde tasarlanmıştır. Ayrıca, kuruluşunuz için anahtarları için merkezi bir yönetim alın. 

**Her zaman şifreli**: Ayrıca bir [iki anahtar hiyerarşi](/sql/relational-databases/security/encryption/overview-of-key-management-for-always-encrypted) her zaman şifreli - gizli verilerin bir sütun sırayla sütun ana anahtar (CMK) ile şifrelenmiş bir AES 256 sütun şifreleme anahtarı (CEK) tarafından şifrelenir. Her zaman şifreli için sağlanan istemci sürücüleri CMKs uzunluğu hiçbir sınırlamaları vardır. CEK şifrelenmiş değerini veritabanında depolanır ve CMK Windows sertifika deposu, Azure anahtar kasası ya da bir donanım güvenlik modülü gibi güvenilir bir anahtar deposunda saklanır. 
- Her iki [CEK ve CMK](/sql/relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell) döndürülebilir. 
- CEK döndürme veri işlemi boyutudur ve zaman yoğunluklu tabloları boyutuna bağlı olarak şifrelenmiş sütunlar içeren olabilir. Bu nedenle CEK döndürmeleri uygun şekilde planlamak akıllıca olur. 
- CMK döndürme, ancak ile veritabanı performansını etkilemez ve ayrılmış rolleriyle yapılabilir.
Aşağıdaki diyagramda her zaman şifreli sütun ana anahtarları için anahtar deposu seçeneklerini gösterir.

![Her zaman şifreli CMK depolama sağlayıcıları](./media/sql-database-manage-after-migration/always-encrypted.png)

### <a name="how-can-i-optimize-and-secure-the-traffic-between-my-organization-and-sql-database"></a>Nasıl en iyi duruma getirme ve my kuruluş ve SQL veritabanı arasındaki trafiğin güvenli mi?
Kuruluş ve SQL veritabanı arasındaki ağ trafiğini genellikle ortak bir ağ üzerinden yönlendirilmiş. Ancak, bu yol en iyi duruma getirme ve daha güvenli hale getirmek isterseniz, Express rotaya bakabilirsiniz. Hızlı rota temelde şirket ağınıza Azure platformuna özel bir bağlantı üzerinden genişletmenizi sağlar. Bunun yapılması, genel Internet üzerinden geçmemektedir. Ayrıca daha yüksek güvenlik, güvenilirlik ve yönlendirme en iyi duruma getirme, düşük ağ gecikme çeviren elde edersiniz ve daha çok yüksek hız normalde genel internet üzerinden giderek deneyimi. Kuruluş ve Azure arasında önemli bir öbek veri aktarma üzerinde planlıyorsanız, hızlı rota kullanarak maliyet avantajları sağlar. Bağlantı için üç farklı bağlantı modeli kuruluşunuzdan Azure'a seçebilirsiniz: 
- [Bulut Exchange birlikte bulundurma](../expressroute/expressroute-connectivity-models.md#CloudExchange)
- [Any herhangi](../expressroute/expressroute-connectivity-models.md#IPVPN)
- [Noktadan noktaya](../expressroute/expressroute-connectivity-models.md#Ethernet)

Hızlı rota 2 için ek ücret ödemeden satın bant genişliği sınırı x veri bloğu sağlar. Hızlı rota kullanarak bölge bağlanabilirlik yapılandırmak da mümkündür. ER bağlantı sağlayıcıları listesini görmek için bkz: [Express rota iş ortakları ve eşleme konumları](../expressroute/expressroute-locations.md). Aşağıdaki makaleleri hızlı rota daha ayrıntılı olarak açıklanmıştır:
- [Hızlı rota üzerinde giriş](../expressroute/expressroute-introduction.md)
- [Önkoşullar](../expressroute/expressroute-prerequisites.md)
- [İş akışları](../expressroute/expressroute-workflows.md)

### <a name="is-sql-database-compliant-with-any-regulatory-requirements-and-how-does-that-help-with-my-own-organizations-compliance"></a>SQL veritabanı Yasal gereksinimlere ile uyumlu olan ve, kendi kuruluşunuzun uyumluluğun nasıl yardımcı olur?
SQL veritabanı yasal compliances aralığı ile uyumludur. Karşılandığından compliances son kümesini görüntülemek için ziyaret [Microsoft Trust Center](https://www.microsoft.com/trustcenter/compliance/complianceofferings) ve detaya gitme SQL veritabanı uyumlu Azure Hizmetleri altında dahil edilmiş olup olmadığını görmek için kuruluşunuz için önemli olan compliances üzerinde. SQL veritabanı uyumlu bir hizmet sertifikalı ancak kuruluşunuzun hizmet uyumlu yardımları ancak otomatik olarak bunu garanti etmez dikkate almak önemlidir.

## <a name="intelligent-database-monitoring-and-maintenance-after-migration"></a>Akıllı veritabanı izleme ve Bakım geçişten sonra

Veritabanınız SQL veritabanına geçirdikten sonra (örneğin, kaynak kullanımının nasıl olduğu gibi onay veya DBCC denetimleri için), veritabanınızı izlemek istediğiniz giderek iş ve düzenli bakım gerçekleştirmek (örneğin, yeniden veya Reorganize komutunu dizinler, istatistikleri vb.). Neyse ki, SQL veritabanı, geçmiş eğilimleri ve kaydedilen ölçümleri ve istatistikler, uygulamanızın her zaman en iyi şekilde çalışır proaktif olarak izlemek ve veritabanınızı tutmanıza yardımcı olmak için kullandığını anlamda akıllı yok. Bazı durumlarda, Azure SQL DB bakım görevlerini yapılandırma ayarlarınızı bağlı olarak otomatik olarak gerçekleştirebilirsiniz. SQL veritabanı, veritabanınızı izleme için üç yönleri şunlardır:
- Performans izleme ve en iyi duruma getirme.
- Güvenlik en iyi duruma getirme
- En iyi duruma getirme maliyeti.

**Performans izleme ve en iyi duruma getirme**: ile sorgu performansı öngörüleri alabilirsiniz özel öneriler için veritabanının yükünüzü uygulamalarınızı çalışmaya devam bir en iyi düzeyde - her zaman böylece. Ayrıca, böylece bu önerileri otomatik olarak uygulanan ve bakım görevlerini gerçekleştirme rahatsız gerekmez ayarlayabilirsiniz. Dizin Danışmanı ile dizin önerisi, iş yüküne göre otomatik olarak uygulayabileceğiniz - otomatik ayarı adı verilir. Uygulama iş yükü değişikliklerinizi en ilgili öneriler sağlamak için öneriler değiştikçe. Ayrıca el ile bu önerileri gözden geçirin ve bunları da istediğiniz kadar uygulama seçeneği elde edersiniz.  

**Güvenlik en iyi duruma getirme**: SQL veritabanı sağlar tanımlama ve potansiyel iş parçacığı için oluşturabileceği şüpheli veritabanı etkinliklerini İnceleme için veri ve tehdit algılama korumanıza yardımcı olması için kullanılabilir güvenlik önerileri Veritabanı. [SQL güvenlik açığı değerlendirmesi](sql-vulnerability-assessment.md) tarama ve veritabanlarınızı ölçekte güvenlik durumunu izlemenizi ve güvenlik risklerini belirlemeniz ve tarafından tanımlanan bir güvenlik taban çizgisiyle kayma sağlayan hizmet raporlama bir veritabanıdır. Her taramadan sonra tıklatılabilir adımları ve düzeltme betikleri özelleştirilmiş bir listesi sunulur yardımcı olduğunuz için Uyumluluk gereklerini karşılamak için kullanılabilir bir değerlendirme raporu yanı sıra.

Azure Güvenlik Merkezi ile güvenlik önerileri Tahta boyunca tanımlamak ve bunları tek bir tıklatmayla uygulayabilirsiniz. 

**En iyi duruma getirme maliyet**: Azure SQL platform kullanım geçmişini değerlendirir ve maliyet en iyi duruma getirme seçenekleri sizin adınıza önerilir sunucuya veritabanları arasında çözümler. Genellikle bu çözümleme, çözümleme ve tıklatılabilir öneriler yapı için bir fortnight alır. Esnek havuz bu tür bir seçenek değil. Öneri portalında bir başlık görüntülenir:

![Esnek havuz önerileri](./media/sql-database-manage-after-migration/elastic-pool-recommendations.png) 

Bu çözümleme "Advisor" bölümünde de görüntüleyebilirsiniz:

![Esnek havuz önerileri-Danışmanı](./media/sql-database-manage-after-migration/advisor-section.png)

### <a name="how-do-i-monitor-the-performance-and-resource-utilization-in-sql-database"></a>Performans ve kaynak kullanımını SQL veritabanında nasıl izlerim?

SQL veritabanı performansını izlemek ve buna uygun olarak ayarlamak için platformun akıllı Öngörüler yararlanabilirsiniz. Performans ve kaynak kullanımını aşağıdaki yöntemleri kullanarak SQL veritabanında izleyebilirsiniz:
- **Azure portal**: Azure portal veritabanı seçerek ve grafik genel bakış bölmesinde tıklatarak tek veritabanlarının kullanımını gösterir. CPU yüzdesi, DTU yüzdesi, veri g/ç yüzdesi, oturumlar yüzdesi ve veritabanı boyutunun yüzdesi dahil olmak üzere birden çok ölçümleri göstermek için grafik değiştirebilirsiniz.

   ![Grafik izleme](./media/sql-database-manage-after-migration/monitoring-chart.png)

   ![Grafik2 izleme](./media/sql-database-manage-after-migration/chart.png)

Bu grafik, kaynak tarafından uyarıları da yapılandırabilirsiniz. Bu uyarılar, bir e-posta ile kaynak durumları yanıt, bir HTTPS/HTTP uç noktasına yazmak veya bir eylemi gerçekleştirmek izin verir. Bkz: [SQL veritabanında veritabanı performansını izleme](sql-database-single-database-monitor.md) ayrıntılı yönergeler için.

- **Dinamik Yönetim görünümlerini**: Sorgulayabileceğiniz [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) kaynak tüketimi istatistikleri geçmişini son bir saat geri dönmek için dinamik yönetim görünümünü ve [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) sistem katalog görünümünü son 14 gün boyunca geçmişi dönün.
- **Sorgu performansı öngörüleri**: [sorgu performansı öngörüleri](sql-database-query-performance.md) üst kaynak tüketen sorguları ve uzun süre çalışan sorgular için belirli bir veritabanı geçmişin görmenizi sağlar. Kaynak kullanımı, süre ve yürütme sıklığını tarafından en sık kullanılan sorguların hızla tanımlayabilirsiniz. Sorguları izlemek ve regresyon algılayabilir. Bu özellik gerektirir [Query Store](/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) etkin ve veritabanı için etkin olmalıdır.

   ![Sorgu Performansı Öngörüleri](./media/sql-database-manage-after-migration/query-performance-insight.png)

- **Günlük analizi Azure SQL analizleri (Önizleme)**: [Azure günlük analizi](../log-analytics/log-analytics-azure-sql.md) 150.000 SQL veritabanları ve 5.000 SQL esnek havuzu başına en fazla toplamak ve anahtar Azure SQL Azure performans ölçümleri, görselleştirme sayesinde destekleme Çalışma alanı. İzleme ve bildirimleri almak için kullanabilirsiniz. SQL veritabanı ve esnek havuz ölçümleri birden çok Azure abonelikleri ve esnek havuzlar arasında izleyebilir ve bir uygulama yığınının her katmanda sorunları belirlemek için kullanılabilir.

### <a name="i-am-noticing-performance-issues-how-does-my-sql-database-troubleshooting-methodology-differ-from-sql-server"></a>Performans sorunlarını tercihinize: nasıl my SQL veritabanı sorun giderme yöntemi farklı SQL Server'dan?
Sorun giderme tekniklerini önemli bir kısmını sorgu tanılamak için kullanırsınız ve veritabanı performans sorunlarıyla aynı kalır. Sonra tüm aynı SQL Server bulut altyapısı çalıştırır. Ancak, platform - Azure SQL DB 'gösterimi' oluşturdu. Bu, hatta daha kolay performans sorunlarını tanılamak ve gidermek yardımcı olabilir. Bu aynı zamanda düzeltme bunlardan bazıları sizin adınıza ve bazı durumlarda işlemlerini, önceden - otomatik olarak düzeltin. 

Performans sorunlarını giderme doğrultusunda, bir yaklaşım, önemli ölçüde akıllı özellikler gibi kullanarak yararlanabilir [sorgu performansı Insight(QPI)](sql-database-query-performance.md) ve [veritabanı Danışmanı](sql-database-advisor.md) birlikte ve Metodoloji fark farklıdır saygı – artık gerçekleştirmeniz gereken şekilde yardımcı olabilecek temel ayrıntılarını grinding el ile iş elinizdeki sorun giderin. Platform sabit iş sizin için yapar. QPI örneğidir. QPI ile tüm sorgu düzeye kadar detaya gitmek ve geçmiş eğilimleri bakın ve tam olarak sorgu gerileyen zaman anlayabilir. Veritabanı Danışmanı'nı, öneriler hakkında genel performansı genellikle eksik dizinler gibi - geliştirmenize yardımcı şeyler dizinleri, sorgularınızı vb. kümesini parametreleştirme bırakmayı sağlar. 

Performans sorunlarını giderme ile yalnızca uygulamayı değil veya uygulama performansını etkileyen, yedekleme veritabanı olup olmadığını belirlemek önemlidir. Genellikle performans sorunu uygulama katmanında arasındadır. Mimari veya veri erişimi deseni olabilir. Örneğin, ağ gecikmesi duyarlıdır chatty bir uygulamaya sahip göz önünde bulundurun. Olurdu çünkü çok kısa istekleri gidip bu durumda, uygulamanızın yükselmesine ("chatty") uygulama ve sunucu arasındaki ve yoğun bir ağda, bu gidiş-dönüş hızlı ekleyin. Bu durumda performansı artırmak için kullanabileceğiniz [Toplu sorguları](sql-database-performance-guidance.md#batch-queries). İsteklerinizi bir toplu işlemde şimdi işleme için toplu işlemleri kullanarak büyük ölçüde yardımcı olur; Bu nedenle, gidiş dönüş gecikmesine kesme ve uygulama performansının artırılmasına yardımcı olur. 

Ayrıca, veritabanınızın genel performans düşüşünü fark ederseniz izleyebilirsiniz [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) ve [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) dinamik yönetim görünümleri için CPU, g/ç ve bellek tüketimini anlayın. Veritabanınıza kaynaklarını gerek duyuldu çünkü performansınızı belki de etkilenmiş. Performans düzeyini değiştirmek ve/veya hizmet katmanını büyüyen ve iş yükü taleplerini küçültme göre gerekebilir olabilir. 

Kapsamlı bir performans sorunlarını ayarlama önerileri için bkz: [veritabanınızı ince ayar](sql-database-performance-guidance.md#tune-your-database).

### <a name="how-do-i-ensure-i-am-using-the-appropriate-service-tier-and-performance-level"></a>İlgili hizmeti katmanını ve performans düzeyini kullanıyorum nasıl emin olursunuz?
SQL veritabanı çeşitli temel, standart ve Premium hizmet katmanları sunar. Her hizmet katmanı, bu hizmet düzeyi bağlı garantili öngörülebilir bir performans alırsınız. İş yükünüz bağlı olarak, burada, kaynak kullanımı olan geçerli performans düzeyini üst sınıra ulaştı. WINS'e etkinlik olabilir. Herhangi bir ayarı (örneğin ekleme veya bir dizin vb. değiştirilmesi.) yardımcı olabilecek olup olmadığını Böyle durumlarda, değerlendirme tarafından ilk başlatmak için yararlıdır. Hala sınırı sorunlarla karşılaşırsanız, daha yüksek performans düzeyi ya da hizmet düzeyi taşımayı düşünün. 

|**Hizmet düzeyi**|**Genel kullanım örneği senaryosu**|
|---|---|
|**Temel**|Bazı kullanıcılar ve yüksek eşzamanlılık, ölçek ve performans gereksinimlerini sahip olmayan bir veritabanı ile uygulamaları. |
|**Standart**|Önemli bir eşzamanlılık, ölçek ve performans gereksinimleri olan uygulamalar, Orta GÇ talepleri düşük ile bağlı. |
|**Premium**|Çok sayıda eşzamanlı kullanıcı sayısı, yüksek CPU/bellek ve yüksek uygulamalarla GÇ ihtiyaç duyar. Premium düzeyi yüksek eşzamanlılık, yüksek verimlilik ve gecikme süresine duyarlı uygulamalar yararlanabilirsiniz. |
|||

Doğru performans düzeyini olduğunuzdan emin olmak, sorgu ve veritabanı kaynak tüketimini "Nasıl SQL veritabanında performans ve kaynak kullanımı izlerim" yukarıda belirtilen yollardan biriyle üzerinden izleyebilirsiniz. Sorguları/veritabanlarınızı tutarlı bir şekilde etkin CPU/bellek daha yüksek bir performans düzeyine ölçeklendirmeyi düşünebilirsiniz vb. üzerinde çalıştığını bulmanız gerekir. Benzer şekilde, hatta yoğun saatlerde unutmayın, kaynakları kadar kullanmak için görünmüyor; geçerli performans düzeyini ölçeklendirme göz önünde bulundurun. 

SaaS uygulama deseni veya bir veritabanı birleştirme senaryo varsa, bir esnek havuz için maliyet iyileştirmesi kullanmayı düşünün. Esnek havuz veritabanı birleştirme ve maliyet iyileştirmesi elde etmek için harika bir yoludur. Daha fazla bilgi için bkz. esnek havuz kullanarak birden çok veritabanını yönetme hakkında: [havuzları ve veritabanlarını yönetme](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-portal). 

### <a name="how-often-do-i-need-to-run-database-integrity-checks-for-my-database"></a>Veritabanı için veritabanı tutarlılığı denetimleri çalıştırmak ne sıklıkta gerekiyor mu?
SQL veritabanı otomatik olarak ve herhangi bir veri kaybı olmadan veri bozulması belirli sınıfları işlemeye izin bazı akıllı teknikleri kullanır. Bu teknikler hizmete yerleşiktir ve hizmet tarafından işlevden gerektiğinde ortaya çıkar. Düzenli aralıklarla, bunları geri yükleme ve DBCC CHECKDB üzerinde çalıştırarak veritabanı yedeklerinizi hizmet genelinde sınanır. Sorun varsa, SQL veritabanı proaktif olarak bunları giderir. [Otomatik sayfa onarım](/sql/sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring) sayfalarını bozuk veya veri bütünlüğü sorunları düzeltmek için kullanılabilir. Veritabanı sayfaları, sayfa bütünlüğünü doğrular varsayılan sağlama toplamı ayarı ile her zaman doğrulanır. SQL veritabanı proaktif olarak izler ve veritabanınızı veri bütünlüğünü inceler ve sorunlar çıkması durumunda bunları en yüksek öncelikli giderir. Bunlara ek olarak, isteğe bağlı olarak, işlem sırasında kendi bütünlük denetimlerinin çalıştırmak isteyebilirsiniz.  Daha fazla bilgi için bkz: [SQL veritabanında veri bütünlüğü](https://azure.microsoft.com/blog/data-integrity-in-azure-sql-database/)

## <a name="data-movement-after-migration"></a>Geçişten sonra veri taşıma

### <a name="how-do-i-export-and-import-data-as-bacpac-files-from-sql-database"></a>Nasıl dışarı aktarma ve BACPAC dosyalarıyla SQL veritabanından veri içeri aktarın?

- **Dışarı aktarma**: Azure SQL veritabanınızı Azure portalından bir BACPAC dosyası olarak dışa aktarabilirsiniz

   ![Veritabanı dışarı aktarma](./media/sql-database-export/database-export.png)

- **Alma**: da veri bir BACPAC dosyası olarak Azure Portalı'nı kullanarak veritabanına aktarabilirsiniz.

   ![Veritabanını içeri aktarma](./media/sql-database-import/import.png)

### <a name="how-do-i-synchronize-data-between-sql-database-and-sql-server"></a>SQL Database ve SQL Server arasında veri eşitlemek nasıl?
Bunu başarmak için birkaç yolu vardır: 
- **[Veri eşitleme](sql-database-sync-data.md)**  – bu özellik, çift yönlü birden çok şirket içi SQL Server veritabanları ve SQL veritabanı arasında veri eşitlemek yardımcı olur. Şirket içi SQL Server veritabanları ile eşitlemek için yükleme ve eşitleme aracısı yerel bir bilgisayarda yapılandırmak ve giden TCP bağlantı noktası 1433'ü açmak gerekir.
- **[İşlem çoğaltma](https://azure.microsoft.com/blog/transactional-replication-to-azure-sql-database-is-now-generally-available/)**  – işlem çoğaltma ile şirket içi verilerinizi Azure SQL DB'ye yayımcı ve Azure SQL DB abone olan şirket içi ile eşitleyebilirsiniz. Şu an için yalnızca bu kurulum desteklenir. Verilerinizi şirket içi Azure SQL en az kapalı kalma süresi ile geçirme hakkında daha fazla bilgi için bkz: [kullanım işlem çoğaltma](sql-database-cloud-migrate.md#method-2-use-transactional-replication)

## <a name="next-steps"></a>Sonraki adımlar
Hakkında bilgi edinin [SQL veritabanı](sql-database-technical-overview.md).

