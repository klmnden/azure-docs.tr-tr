---
title: Geçişten sonra - Azure SQL veritabanı tek ve havuza alınmış veritabanlarını yönetme | Microsoft Docs
description: Azure SQL veritabanına geçişten sonra veritabanınızı yönetmeyi öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: joesackmsft
ms.author: josack
ms.reviewer: sstein
manager: craigg
ms.date: 02/13/2019
ms.openlocfilehash: 73bc2d9889727a1633986e12642bd06cf2714632
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66357321"
---
# <a name="new-dba-in-the-cloud--managing-your-single-and-pooled-databases-in-azure-sql-database"></a>Yeni DBA bulutta – Azure SQL veritabanı'nda, tek ve havuza alınmış veritabanlarını yönetme

Geleneksel yönetimden kendi kendine yönetilen bir PaaS ortamı için kendiliğinden denetimli bir ortamda görünen biraz zor taşıma ilk. Bir uygulama geliştiricisi veya DBA olarak, uygulamanızın kullanılabilir durumda tutmanıza yardımcı olacak Platform, yüksek performanslı, güvenli ve dayanıklı - çekirdek özellikler her zaman bilmek istersiniz. Bu makalede, tam olarak bunu amaçlar. Makale temellerini kaynakları düzenler ve nasıl en iyi şekilde yönetmek ve uygulamanızın verimli bir şekilde çalışmaya devam etmesi ve bulutta en iyi sonuçları elde etmek için tek ve havuza alınmış veritabanları ile SQL veritabanı'nın anahtar özelliklerini kullanmaya yönelik bazı yönergeler sağlar. Bu makale için tipik İzleyici bu olacak olan:

- Azure SQL veritabanı, uygulamaları Modernleştirme – kendi uygulamaları geçişini Değerlendiriyorlar.
- Kullanıcıların uygulamaları – devam eden geçiş senaryosu geçiş sürecinde olduğundan.
- Azure SQL DB – yeni DBA bulut geçişi yakın zamanda tamamladınız.

Bu makalede Azure SQL veritabanı'nın temel özelliklerinden bazıları kolayca çalışırken yararlanabileceğiniz bir platform ele alınmaktadır ile tek veritabanları ve elastik havuza alınmış veritabanları. Bunlar aşağıda verilmiştir:

- Veritabanını Azure portalını kullanarak izleme
- İş sürekliliği ve olağanüstü durum kurtarma (BCDR)
- Güvenlik ve uyumluluk
- Akıllı veritabanı izleme ve Bakım
- Veri taşıma

> [!NOTE]
> Bu makale Azure SQL veritabanı'nda aşağıdaki dağıtım seçenekleri için geçerlidir: tek veritabanları ve elastik havuzlar. SQL veritabanı yönetilen örnek dağıtım seçeneği geçerli değil.

## <a name="monitor-databases-using-the-azure-portal"></a>Azure portalını kullanarak veritabanlarını izleme

İçinde [Azure portalında](https://portal.azure.com/), tek veritabanı s kullanımını veritabanınızı seçip tıklayarak izleyebilirsiniz **izleme** grafiği. Bu işlem sonrasında bir **Ölçüm** penceresi görüntülenir. **Grafiği düzenle** düğmesine tıklayarak değişiklik yapabilirsiniz. Şu ölçümleri ekleyin:

- CPU yüzdesi
- DTU yüzdesi
- Veri G/Ç yüzdesi
- Veri boyutu yüzdesi

Bu ölçümleri ekledikten sonra görüntülemeye devam edebilirsiniz **izleme** grafik hakkında daha fazla bilgi **ölçüm** penceresi. Dört ölçümün tümü de veritabanınızın ortalama **DTU** kullanım yüzdesini gösterir. Bkz: [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md) makaleler hizmet katmanları hakkında daha fazla bilgi için.  

![Hizmet katmanına göre veritabanı performansını izleme.](./media/sql-database-single-database-monitoring/sqldb_service_tier_monitoring.png)

Performans ölçümlerine ilişkin uyarıları da yapılandırabilirsiniz. **Ölçüm** penceresindeki **Uyarı ekle** düğmesine tıklayın. Uyarınızı yapılandırmak için sihirbazı takip edin. Ölçümlerin belirli bir eşiği aşması veya belirli bir eşiğin altına düşmesi halinde uyarı alabilirsiniz.

Örneğin, veritabanınızdaki bir iş yükünün artmasını bekliyorsanız bir e-posta uyarısı yapılandırarak veritabanınızın herhangi bir performans ölçümünde %80 sınırına ulaşması halinde uyarı alabilirsiniz. Zaman sonraki en yüksek işlem boyutu geçmeniz gerektiğini anlamak üzere erken bir uyarı olarak kullanabilirsiniz.

Performans ölçümleri, daha düşük bir işlem boyutu için geçemeyeceğinizi belirlemenize de yardımcı olabilir. Standart S2 veritabanını kullandığınızı ve tüm performans ölçümlerinin, veritabanının belirli bir zaman için ortalama %10'dan daha fazla kullanımda bulunmadığını gösterdiğini varsayın. Bu, veritabanının Standart S1'de de düzgün şekilde çalışabileceğini gösterir. Ancak, ani değişiklik veya dalgalanma gösteren bir alt işlem boyutu geçmeye karar vermeden önce iş yüklerini farkında olun.

## <a name="business-continuity-and-disaster-recovery-bcdr"></a>İş sürekliliği ve olağanüstü durum kurtarma (BCDR)

İş sürekliliği ve olağanüstü durum kurtarma yeteneklerini işletmenizi, olağanüstü bir durumunda her zamanki şekilde devam etmek etkinleştirin. Olağanüstü durum veritabanı düzeyi olay olabilir (örneğin, birisi yanlışlıkla önemli bir tabloyu bırakacağını) veya bir veri merkezi düzeyi olay (Bölgesel felaketler, örneğin bir tsunami).

### <a name="how-do-i-create-and-manage-backups-on-sql-database"></a>Nasıl oluştururum ve SQL veritabanı yedeklemelerini yönetme

Azure SQL DB'de yedeklemeleri oluşturmayın ve gerek yoktur çünkü. Artık zamanlama, alma ve yedekleri yönetme hakkında endişelenmeniz gereken şekilde SQL veritabanı, sizin için otomatik olarak veritabanlarını yedekler. Platform, her hafta, fark yedekleme birkaç saatte bir günlük 5 verimli olağanüstü durum kurtarma sağlamak için dakikada, yedekleme ve ve veri kaybını en az bir tam yedekleme alır. Bir veritabanını oluşturur oluşturmaz, ilk tam yedekleme olur. Bu yedeklemeler "Bekletme dönemi" adlı belirli bir süre için kullanabileceğiniz ve seçtiğiniz hizmet katmanına göre değişir. SQL veritabanı kullanarak bu elde tutma dönemi içinde zaman içinde herhangi bir noktasına geri yükleme olanağı sağlar [işaret zaman Kurtarma (PITR) içinde](sql-database-recovery-using-backups.md#point-in-time-restore).

|Hizmet katmanı|Bekletme süresi (gün)|
|---|:---:|
|Temel|7|
|Standart|35|
|Premium|35|
|||

Ayrıca, [uzun süreli saklama (LTR)](sql-database-long-term-retention.md) özelliği, yedekleme dosyalarının çok daha uzun bir süre için 10 yıla kadar özellikle tutun ve belirtilen süre içinde herhangi bir noktada bu yedeklemelerinden veri geri olanak tanır. Ayrıca, veritabanı yedeklemeleri bölgesel felaketler gelen esnekliği sağlamak için coğrafi olarak yedekli depolama alanında tutulur. Ayrıca, herhangi bir Azure bölgesinde süre Bekletme dönemi içinde herhangi bir noktada bu yedekleri geri yükleyebilirsiniz. Bkz: [iş sürekliliğine genel bakış](sql-database-business-continuity.md).

### <a name="how-do-i-ensure-business-continuity-in-the-event-of-a-datacenter-level-disaster-or-regional-catastrophe"></a>Bölgesel felaketler ya da veri merkezi düzeyinde olağanüstü durum yaşanması durumunda iş sürekliliğini nasıl emin olabilirim

Veritabanı Yedekleriniz bölgesel bir olağanüstü durum oluşması halinde emin olmak için coğrafi olarak yedekli depolama alanında depolandığından, yedekleme için başka bir Azure bölgesine geri yükleyebilirsiniz. Bu, coğrafi geri yükleme olarak adlandırılır. ' % S'rpo (kurtarma noktası hedefi) Bu genellikle < 1 saat (tahmini kurtarma süresi – saatleri için birkaç dakika) ERT olduğundan.

Görev açısından kritik veritabanları için Azure SQL DB sunar, etkin coğrafi çoğaltma. Bu temelde yapar, coğrafi olarak çoğaltılmış ikincil veritabanının bir kopyası, özgün başka bir bölgede oluşturmasıdır. Örneğin, veritabanınızı başlangıçta Azure Batı ABD bölgesinde barındırılıyor ve bölgesel bir olağanüstü durum kurtarma sağlayacak istiyorsanız. Batı ABD Doğu ABD aktarmada bir etkin coğrafi çoğaltma veritabanı oluşturursunuz. Batı ABD üzerinde calamity durumla karşılaştığınızda, Doğu ABD bölgesinde devredebilir. Bu veritabanı otomatik olarak Doğu ABD'de ikincil bir olağanüstü durum yaşanması yöneltilir olduğunu ulaşılamamasını garantilediğinden bunları bir otomatik yük devretme grubunda yapılandırma daha iyi bir seçimdir. Bunun için RPO < 5 saniye ile ERT < 30 saniye ' dir.

Daha sonra otomatik yük devretme grubu yapılandırılmamışsa, etkin ikincil bir yük devretme başlatın ve bir olağanüstü durum için izlemek uygulamanız gerekir. Farklı Azure bölgelerinde 4 gibi etkin coğrafi-çoğaltmaları oluşturabilirsiniz. Daha da iyi alır. Bu ikincil etkin coğrafi-çoğaltmaların salt okunur erişim için de erişebilirsiniz. Bu bir coğrafi olarak dağıtılmış uygulama senaryosu için gecikme süresini azaltmak için çok faydalı olur.

### <a name="how-does-my-disaster-recovery-plan-change-from-on-premises-to-sql-database"></a>Olağanüstü durum kurtarma planı kullandığımı nasıl SQL veritabanı'na şirket içinden değişir

Özet olarak, geleneksel SQL Server Kurulumu, etkin bir şekilde yük devretme kümelemesi, veritabanı yansıtma, işlem çoğaltma veya günlük aktarma gibi özellikleri kullanarak kullanılabilirlik kümenizi yönetmek ve korumak ve emin olmak için yedeklemeleri yönetmek gerekli şirket İş sürekliliği. Geliştirme ve veritabanı uygulamanızı en iyi duruma getirme ve çok olağanüstü durum yönetimi konusunda endişe duymamanızı SQL veritabanı ile bunları, platform yönetir. Yedekleme ve olağanüstü durum kurtarma planları yapılandırılır ve yalnızca birkaç tıklamayla Azure portalında (veya PowerShell API'leri kullanarak bazı komutlar) ile çalışma olabilir.

Olağanüstü durum kurtarma hakkında daha fazla bilgi için bkz: [Azure SQL Db olağanüstü durum kurtarma 101](https://azure.microsoft.com/blog/azure-sql-databases-disaster-recovery-101/)

## <a name="security-and-compliance"></a>Güvenlik ve uyumluluk

SQL veritabanı güvenlik ve gizlilik çok ciddi bir şekilde alır. SQL veritabanı içinde güvenlik veritabanı düzeyinde ve platform düzeyinde kullanılabilir ve birkaç katmanlara kategorilere olduğunda en iyi anlaşılmalıdır. Her katmanda denetlemek ve uygulamanız için en iyi güvenlik sağlamak alın. Katmanlar şunlardır:

- Kimlik ve kimlik doğrulaması ([Windows/SQL kimlik doğrulaması ve Azure Active Directory [AAD] kimlik](sql-database-control-access.md)).
- Etkinlik izleme ([denetim](sql-database-auditing.md) ve [tehdit algılama](sql-database-threat-detection.md)).
- Gerçek veri koruma ([saydam veri şifrelemesi [TDE]](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) ve [[AE]'Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine)).
- Hassas ve ayrıcalıklı verilere erişimi denetleme ([satır düzeyi güvenlik](/sql/relational-databases/security/row-level-security) ve [dinamik veri maskeleme](/sql/relational-databases/security/dynamic-data-masking)).

[Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) azure'da, şirket içinde ve diğer bulutlarda çalışan iş yükleri arasında Merkezi güvenlik yönetimi sunar. Gerekli olup olmadığını görüntüleyebilirsiniz SQL veritabanı koruma gibi [denetim](sql-database-auditing.md) ve [saydam veri şifrelemesi [TDE]](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) tüm kaynaklar üzerinde yapılandırılır ve kendi gereksinimlerinize dayalı olarak ilkeler oluşturabilir.

### <a name="what-user-authentication-methods-are-offered-in-sql-database"></a>SQL veritabanı'nda kullanıcı kimlik doğrulama yöntemleri sunulur

Vardır [iki kimlik doğrulama yöntemleri](sql-database-control-access.md#authentication) SQL veritabanı'nda sunulan:

- [Azure Active Directory kimlik doğrulaması](sql-database-aad-authentication.md)
- SQL kimlik doğrulaması

Geleneksel windows kimlik doğrulaması desteklenmiyor. Azure Active Directory (AD) bir merkezi kimlik ve erişim yönetimi hizmetidir. Bu, çok bir kolayca bir çoklu oturum açma erişimi (SSO) için tüm personel, kuruluşunuzda sağlayabilirsiniz. Ne bu kimlik bilgileri daha basit kimlik doğrulaması için tüm Azure hizmetleri arasında paylaşıldığı anlamına gelir. Destekleyen AAD [MFA (çok faktörlü kimlik doğrulamasını)](sql-database-ssms-mfa-authentication.md) ile bir [yalnızca birkaç tıklamayla](../active-directory/hybrid/how-to-connect-install-express.md) AAD Windows Server Active Directory ile tümleştirilebilir. SQL kimlik doğrulaması, tam olarak, geçmişte kullandığınız gibi çalışır. Bir kullanıcı adı/parola sağlayın ve kullanıcıların belirli bir SQL veritabanı sunucu üzerindeki herhangi bir veritabanı için kimlik doğrulaması yapabilir. Bu da çok faktörlü kimlik doğrulaması ve Azure AD etki alanı içinde Konuk kullanıcı hesaplarını sunmak SQL veritabanı ve SQL veri ambarı sağlar. Bir Active Directory şirket içi zaten varsa, dizininize Azure'a genişletmek için Azure Active Directory ile dizin ad'sini birleştirebilir.

|**Varsa...**|**SQL veritabanı / SQL veri ambarı**|
|---|---|
|Azure Active Directory (AD) ile Azure kullanmayı tercih edin|Kullanım [SQL kimlik doğrulaması](sql-database-security-overview.md)|
|SQL Server'da kullanılan AD şirket içi|[Azure AD ile federasyona](../active-directory/hybrid/whatis-hybrid-identity.md)ve Azure AD kimlik doğrulaması kullanın. Bu, çoklu oturum açma kullanabilirsiniz.|
|Çok faktörlü kimlik doğrulaması (MFA) zorunlu kılmanız gerekiyorsa|Bir ilke olarak MFA gerektirecek [Microsoft koşullu erişim](sql-database-conditional-access.md)ve [MFA desteği ile Azure AD Evrensel kimlik doğrulaması](sql-database-ssms-mfa-authentication.md).|
|Microsoft hesapları (live.com, outlook.com) veya diğer etki alanları (gmail.com) Konuk hesapları sahip|Kullanım [Azure AD Evrensel kimlik doğrulaması](sql-database-ssms-mfa-authentication.md) SQL veritabanı/veri ambarı'nda hangi yararlanır [Azure AD B2B işbirliği](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md).|
|Bir Federasyon etki alanına ait Azure AD kimlik bilgilerinizi kullanarak Windows için oturum|Kullanım [Azure AD ile tümleşik kimlik doğrulaması](sql-database-aad-authentication-configure.md).|
|Windows Azure ile birleştirilmiş değil bir etki alanı kimlik bilgilerini kullanarak oturum açtığınız|Kullanım [Azure AD ile tümleşik kimlik doğrulaması](sql-database-aad-authentication-configure.md).|
|Orta katman Hizmetleri, bir SQL veritabanı veya SQL veri ambarı için bağlanması gerekir|Kullanım [Azure AD ile tümleşik kimlik doğrulaması](sql-database-aad-authentication-configure.md).|
|||

### <a name="how-do-i-limit-or-control-connectivity-access-to-my-database"></a>Nasıl miyim sınırlamak denetlemek veya bağlantı access veritabanı

Elinizin altında uygulamanız için en iyi bağlantı kuruluş elde etmek için kullanabileceğiniz birden fazla teknik vardır.

- Güvenlik Duvarı Kuralları
- Sanal ağ hizmet uç noktaları
- Ayrılmış IP’ler

#### <a name="firewall"></a>Güvenlik duvarı

Bir güvenlik duvarı erişimi sunucunuza bir dış varlık SQL veritabanı sunucunuza yalnızca belirli varlıklara erişimi vererek engeller. Varsayılan olarak, tüm bağlantılar ve SQL veritabanı sunucu içindeki veritabanlarına, diğer Azure hizmetlerinden gelen bağlantılar dışında izin verilmez. Bir güvenlik duvarı kuralı, bu bilgisayarın IP adresini güvenlik duvarı üzerinden izin vererek onayladığınız yalnızca varlıklara (örneğin, bir geliştirici makine), sunucunuza erişim açabilirsiniz. Ayrıca, SQL veritabanı sunucusuna erişmesine izin vermek istediğiniz IP aralığı belirtmenize olanak sağlar. Örneğin, geliştirici Makine IP adresleri kuruluşunuzdaki tek seferde Güvenlik Duvarı ayarları sayfasındaki bir aralığı belirterek eklenebilir.

Sunucu düzeyinde veya veritabanı düzeyinde güvenlik duvarı kuralları oluşturabilirsiniz. Sunucu düzeyi IP güvenlik duvarı kuralları ya da Azure portal veya ile SSMS kullanarak oluşturulabilir. Bir sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralı ayarlama hakkında daha fazla bilgi için bkz: [SQL veritabanı'nda IP güvenlik duvarı kuralları oluşturma](sql-database-security-tutorial.md#create-firewall-rules).

#### <a name="service-endpoints"></a>Hizmet uç noktaları

Varsayılan olarak, SQL veritabanınız için yapılandırılmış "Azure hizmetlerinin sunucuya erişmesine izin ver" – azure'da herhangi bir sanal makineyi başka bir deyişle, veritabanınıza bağlanma girişiminde bulunabilir. Yine de bu girişimler, kimlik doğrulaması zorunda. Tüm Azure IP'ler tarafından erişilebilir olması için veritabanının istemiyorum, ancak "Azure hizmetlerinin sunucuya erişmesine izin ver" devre dışı bırakabilirsiniz. Buna ek olarak, yapılandırabileceğiniz [sanal ağ hizmet uç noktaları](sql-database-vnet-service-endpoint-rule-overview.md).

Hizmet uç noktaları (SE) azure'da kendi özel sanal ağ için yalnızca kritik Azure kaynaklarınızı kullanıma sunmanıza olanak sağlar. Bunun yapılması, temelde genel erişim kaynaklarınıza ortadan kaldırır. Azure sanal ağınıza arasındaki trafiğin Azure omurga ağı üzerinde kalır. Zorlamalı tünel paketi yönlendirme SE olursunuz. Sanal ağınız, kuruluşunuz internet trafiğini ve aynı yol üzerinden gitmek için Azure hizmet trafiğini zorlar. Hizmet uç noktaları ile bu paket akışı doğrudan sanal ağınızdan Azure omurga ağındaki hizmete en iyi duruma getirebilirsiniz.

![Sanal ağ hizmet uç noktaları](./media/sql-database-manage-after-migration/vnet-service-endpoints.png)

#### <a name="reserved-ips"></a>Ayrılmış IP’ler

Başka bir seçenek sağlamak, [ayrılmış IP'ler](../virtual-network/virtual-networks-reserved-public-ip.md) , VM'ler ve beyaz liste için VM IP adresleri sunucu belirli bu güvenlik duvarı ayarları. Ayrılmış IP'ler atayarak, IP adreslerinin değiştirilmesi ile güvenlik duvarı kuralları güncelleştirmek zorunda derdinden kaydedin.

### <a name="what-port-do-i-connect-to-sql-database-on"></a>Hangi bağlantı noktası için SQL veritabanı üzerinde bağlanabilirim

Bağlantı noktası 1433'tür. SQL veritabanı, bu bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanmak için kuruluşunuzun güvenlik duvarı ayarlarında bir giden kuralı eklemeniz gerekir. Bir kural olarak, Azure sınırının dışında 1433 numaralı bağlantı noktasını kullanıma sunma kaçının.

### <a name="how-can-i-monitor-and-regulate-activity-on-my-server-and-database-in-sql-database"></a>Nasıl izleyebilir ve etkinlik my server ve SQL veritabanı üzerinde düzenleyen mi

#### <a name="sql-database-auditing"></a>SQL veritabanı denetimi

SQL veritabanı ile veritabanı olaylarını izlemek için ON denetimini kapatabilirsiniz. [SQL veritabanı denetimi](sql-database-auditing.md) veritabanı olaylarını kaydeder ve Azure depolama hesabınızdaki bir denetim günlük dosyasına yazar. Denetim, olası güvenlik ve ilke ihlallerini bir anlayış kazanmak istiyorsanız mevzuatla uyumluluk vb. özellikle yararlı olur. Tanımlamak ve belirli denetim gerekir ve önceden yapılandırılmış raporları ve veritabanınızda gerçekleşen olaylar hakkında genel bakışını almak için bir Pano alabilirsiniz temel düşündüğünüz olayların kategorilerini yapılandırmanıza olanak sağlar. Bu denetim veritabanı düzeyinde veya sunucu düzeyinde ilkeler uygulayabilirsiniz. Sunucu/veritabanı için denetim üzerinde açma konusunda kılavuz bakın: [SQL veritabanı'nı etkinleştirme denetimi](sql-database-security-tutorial.md#enable-security-features).

#### <a name="threat-detection"></a>Tehdit algılama

İle [tehdit algılama](sql-database-threat-detection.md), kolayca denetleyerek bulunan güvenlik veya ilke ihlallerini alacak sunma olanağı elde edin. Bir güvenlik sisteminizde olası tehditleri veya ihlallerine yönelik uzman olmanız gerekmez. Tehdit algılama, SQL ekleme algılama gibi bazı yerleşik özellikleri de vardır. SQL ekleme, değiştirme veya veri ve genel bir veritabanı uygulaması saldırmak oldukça yaygın bir yolu tehlikeye girişimi. Tehdit algılama, birden fazla anormal veritabanı erişim modellerinin (örneğin, olağan dışı bir konumdan veya alışkın olmadığınız bir asıl erişimi) yanı sıra olası güvenlik açıklarına ve SQL ekleme saldırıları algılamak algoritmalar çalıştırılır. Veritabanında bir tehdit algılanırsa, güvenlik sorumlularını ya da diğer atanan Yöneticiler bir e-posta bildirimi alırsınız. Her uyarı hakkında daha fazla araştırmak ve tehdidi azaltmak öneriler ve şüpheli etkinlik ayrıntılarını sağlar. Tehdit algılamayı etkinleştirme konusunda bilgi için bkz: [Tehdit algılamayı etkinleştirme](sql-database-security-tutorial.md#enable-security-features).

### <a name="how-do-i-protect-my-data-in-general-on-sql-database"></a>Nasıl verilerimi Genel SQL veritabanı'nda koruyabilirim

Şifreleme, saldırganların önemli verilerinizin korunmasına yönelik güçlü bir mekanizma sağlar. Şifrelenmiş verilerinizi yetkisiz şifre çözme anahtarı olmadan herhangi bir kullanıma önemlidir. Bu nedenle, ek bir SQL veritabanında yerleşik güvenlik mevcut katmanları üzerinde koruma katmanı ekler. SQL veritabanı'ndaki verilerinizi koruma gereken iki unsur vardır:

- Bekleyen verilerin ve günlük dosyalarında, verileri
- Verilerinizi uçuşan

SQL veritabanı'nda, varsayılan olarak, depolama alt sisteminin veri ve günlük dosyalarında bekleyen verilerinizi tamamen her zaman aracılığıyla şifrelenir ve [saydam veri şifrelemesi [TDE]](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql). Yedeklemeleriniz de şifrelenir. İle TDE bu verilere erişmek, uygulama tarafından gereken bir değişiklik bulunmamaktadır. Şifreleme ve şifre çözme şeffaf bir şekilde gerçekleşir; Bu nedenle adı.
Uçuşan hassas verilerinizi korumaya ve bekleme sırasında SQL veritabanı adlı bir özellik sağlar. [her zaman şifreli (AE)](/sql/relational-databases/security/encryption/always-encrypted-database-engine). AE hassas sütunları veritabanında (Veritabanı yöneticileri ve yetkisiz kullanıcıların şifreli olduğu için) şifreleyen istemci tarafı şifreleme bir biçimidir. Sunucu, şifrelenmiş verilerin başlangıç olarak alır. Yalnızca yetkili istemcilerin hassas sütunları şifresini çözmek için her zaman şifreli için anahtar de istemci tarafında depolanır. Şifreleme anahtarları istemcide depolandığından sunucuyu ve veri yöneticilerinin hassas verileri göremez. Uçtan uca, fiziksel diske yetkisiz istemcilerden tablodaki hassas sütun AE şifreler. Dba'lar kendi SQL komutlarını bir parçası olarak şifrelenmiş sütunlar sorgu devam edebilmesi AE eşitlik karşılaştırmaları bugün desteklemektedir. Her zaman şifreli çeşitli anahtar deposu seçenekler ile gibi kullanılabilir [Azure anahtar kasası](sql-database-always-encrypted-azure-key-vault.md), Windows sertifika deposu ve yerel donanım güvenlik modülleri.

|**Özellikleri**|**Her zaman şifreli**|**Saydam veri şifrelemesi**|
|---|---|---|
|**Şifreleme yayılma**|Uçtan uca|Bekleyen veriler|
|**Veritabanı sunucusu gizli verilere erişebilir.**|Hayır|Evet, bekleyen veriler için şifreleme olduğundan|
|**İzin verilen T-SQL işlemleri**|Eşitlik karşılaştırması|Tüm T-SQL yüzey alanını kullanılabilir|
|**Bu özelliği kullanmak için gerekli uygulama değişiklikleri**|En az|Çok az|
|**Şifreleme ayrıntı düzeyi**|Sütun düzeyi|Veritabanı düzeyi|
||||

### <a name="how-can-i-limit-access-to-sensitive-data-in-my-database"></a>Nasıl miyim Veritabanımın hassas verilere erişimi sınırlayabilir

Her uygulamanın belirli bir bit hassas verilerin herkese görünür olmasını korunması gereken veritabanı vardır. Kuruluş içinde belirli bir personel, bu verileri görüntülemek için ancak diğerleri bu verileri işleyememelidir gerekir. Çalışan ücretler bir örnektir. Bir yönetici ücret bilgilere erişim için doğrudan raporlarının ancak gerekir, bireysel takım üyelerinin kendi eş ücret bilgilere erişimi olmaması. Örneğin, müşterilerin Ssn'ler test veya geliştirme aşamalarında hassas verilerle etkileşim veri geliştiriciler başka bir senaryodur. Bu bilgileri tekrar geliştirici için açık gerekmez. Bu gibi durumlarda, hassas verilerinizi ya da maskelenmiş olamaz ya da hiç gösterilmeyen gerekir. SQL veritabanı, yetkisiz kullanıcıların hassas verileri görüntüleyebilirsiniz engellemek için bu iki yaklaşımı sunar:

[Dinamik veri maskeleme](sql-database-dynamic-data-masking-get-started.md) , hassas verilerin görünürlüğünü ayrıcalık sahibi uygulama katmanı üzerinde ayrıcalıklı olmayan kullanıcıları maskeleyerek sınırlamanıza olanak tanır bir veri maskeleme özelliğidir. Maskeleme deseni oluşturan bir maskeleme kuralı tanımlayın (örneğin, yalnızca gösteriye abone son dört rakamı ulusal bir kimliği SSN: XXX-xx-0000 ve çoğu Xs olarak işaretleyin) ve hangi kullanıcıların maskeleme kuralından hariç tutulacak olduğunu belirleyin. Maskeleme üzerinde anında gerçekleşir ve çeşitli veri kategorileri için kullanılabilen çeşitli maskeleme işlevi vardır. Dinamik veri maskeleme, otomatik olarak veritabanınızda hassas verileri algılamak ve maskeleme uygulamak olanak tanır.

[Satır düzeyi güvenlik](/sql/relational-databases/security/row-level-security) satır düzeyinde erişimi denetlemenize olanak verir. Diğer bir deyişle, (Grup üyeliği veya yürütme bağlamı) Sorguyu yürüten kullanıcı tabanlı bir veritabanı tablosundaki belirli satırlar gizlidir. Erişimi kısıtlama veritabanı katmanında yerine uygulama mantığınızın basitleştirmek için bir uygulama katmanı üzerinde gerçekleştirilir. Bu satırlara erişimi olan değil kullanıma sunulan satırları ve güvenlik ilkesi sonraki tanımlama out filtreleme bir filtre koşulu, oluşturarak başlayın. Son olarak, son kullanıcı sorgularını çalıştırır ve kullanıcının ayrıcalık bağlı olarak bunlar kısıtlı satırları görüntülemek ya da hiç göremiyorsanız.

### <a name="how-do-i-manage-encryption-keys-in-the-cloud"></a>Şifreleme anahtarlarını bulutta nasıl yönetebilirim

(İstemci tarafı şifreleme)'Always Encrypted ve saydam veri şifrelemesi (Bekleyen şifreleme) için anahtar yönetimi seçeneği vardır. Şifreleme anahtarlarını düzenli aralıklarla döndürmeniz önerilir. Döndürme sıklığı, iç kuruluş düzenlemeleri ve uyumluluk gereksinimlerini ile hizalamanız gerekir.

#### <a name="transparent-data-encryption-tde"></a>Saydam Veri Şifrelemesi (TDE)

TDE iki anahtar hiyerarşisi yoktur: her bir kullanıcı veritabanındaki verileri hangi sırayla bir sunucu benzersiz asimetrik RSA 2048 ana anahtar ile şifrelenmiş bir simetrik AES-256'yı veritabanı benzersiz bir veritabanı şifreleme anahtarı (DEK) tarafından şifrelenir. Ana anahtarı olabilir ya da yönetilen:

- Otomatik olarak platform tarafından - SQL veritabanı.
- Veya siz kullanarak [Azure anahtar kasası](sql-database-always-encrypted-azure-key-vault.md) olarak anahtar deposu.

Varsayılan olarak, saydam veri şifrelemesi için ana anahtar, kolaylık sağlamak için SQL veritabanı hizmeti tarafından yönetilir. Kuruluşunuzun ana anahtarı üzerinde denetim isterseniz, Azure anahtar Vault](sql-database-always-encrypted-azure-key-vault.md) anahtar deposu olarak kullanılacak bir seçenek yoktur. Azure anahtar Kasası'nı kullanarak, kuruluşunuzun anahtar sağlama, döndürme ve izin denetimleri denetime varsayar. [Döndürme veya TDE ana anahtar türünü değiştirme](/sql/relational-databases/security/encryption/transparent-data-encryption-byok-azure-sql-key-rotation) yalnızca DEK'yi yeniden şifreler gibi hızlıdır. Rolleri, güvenlik ve veri yönetimi arasındaki ayrımı olan kuruluşlar için Güvenlik Yöneticisi Azure Key vault'taki TDE ana anahtarı için anahtar malzemesi sağlamak ve kullanmak için veritabanı yöneticisine için bir Azure Key Vault anahtarı tanımlayıcısı sağlayın bir sunucuda bekleyen şifreleme. Key Vault, Microsoft bakın veya herhangi bir şifreleme anahtarı ayıklamak olacak şekilde tasarlanmıştır. Ayrıca, kuruluşunuz için anahtarları için merkezi bir yönetim alın.

#### <a name="always-encrypted"></a>Always Encrypted

Ayrıca bir [iki anahtar hiyerarşi](/sql/relational-databases/security/encryption/overview-of-key-management-for-always-encrypted) Always Encrypted - hassas veriler içeren bir sütun sütun ana anahtarı (CMK) tarafından sırayla şifrelenir bir AES 256-sütun şifreleme anahtarı (CEK) tarafından şifrelenir. İstemci sürücüleri her zaman şifreli için sağlanan sınırsız CMKs uzunluğuna sahip. Şifrelenmiş CEK değerini veritabanında depolanır ve CMK Windows sertifika Store, Azure anahtar kasası veya donanım güvenlik modülü gibi güvenilen bir anahtar deposunda saklanır.

- Her iki [CEK ve CMK](/sql/relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell) döndürülebilir.
- CEK döndürme bir veri işlemi boyutudur ve zaman yoğunluklu tabloların boyutuna bağlı olarak şifrelenmiş sütunlar içeren olabilir. Bu nedenle CEK dönüşümüne planlarken akıllıca olur.
- CMK döndürme, ancak ile veritabanı performansını etkilemez ve ayrılmış rolleriyle yapılabilir.

Aşağıdaki diyagramda Always Encrypted sütun ana anahtarları için anahtar deposu seçenekleri gösterilmektedir.

![Her zaman şifreli CMK depolama sağlayıcıları](./media/sql-database-manage-after-migration/always-encrypted.png)

### <a name="how-can-i-optimize-and-secure-the-traffic-between-my-organization-and-sql-database"></a>Nasıl en iyi duruma getirmek ve benim kuruluş ve SQL veritabanı arasında trafiği güvenli mi

Kuruluşunuz ve SQL veritabanı arasındaki ağ trafiğini genellikle ortak bir ağ üzerinden yönlendirilir. Ancak, bu yol en iyi duruma getirmek ve daha güvenli hale getirmek isterseniz, Express Route bakabilirsiniz. Express route temelde, şirket ağınıza bir özel bağlantı üzerinden Azure platformundaki genişletmenizi sağlar. Bunun yapılması, genel Internet üzerinden geçmemektedir. Daha yüksek güvenlik, güvenilirlik ve yönlendirme en iyi duruma getirme, daha düşük ağ gecikme süresi için çeviren ayrıca Al ve çok daha hızlı işleyebileceğiniz, normalde genel internet üzerinden gidip deneyimleyeceği. Express Route kullanarak kuruluşunuz ve Azure arasında önemli bir Öbek ile veri aktarma üzerinde planlıyorsanız, maliyet avantajları sağlayabilir. Bağlantı için üç farklı bağlantı modelleri kuruluşunuzdan Azure'a seçebilirsiniz:

- [Bulut Exchange birlikte bulundurma](../expressroute/expressroute-connectivity-models.md#CloudExchange)
- [Herhangi bir ağdan herhangi](../expressroute/expressroute-connectivity-models.md#IPVPN)
- [Noktadan noktaya](../expressroute/expressroute-connectivity-models.md#Ethernet)

Express Route 2 katına için ek ücret satın aldığınız bant genişliği sınırını veri bloğu sağlar. Expressroute kullanarak, bölge bağlantı yapılandırmak da mümkündür. ER bağlantı sağlayıcılarının bir listesini görmek için bkz: [Express rota iş ortakları ve eşleme konumları](../expressroute/expressroute-locations.md). Aşağıdaki makaleler Express Route daha ayrıntılı olarak açıklanmaktadır:

- [Expressroute üzerinde giriş](../expressroute/expressroute-introduction.md)
- [Önkoşullar](../expressroute/expressroute-prerequisites.md)
- [İş akışları](../expressroute/expressroute-workflows.md)

### <a name="is-sql-database-compliant-with-any-regulatory-requirements-and-how-does-that-help-with-my-own-organizations-compliance"></a>SQL veritabanı herhangi bir yasal gereksinimleriyle uyumlu olan ve, kendi kuruluşun uyumu nasıl yardımcı olur

SQL veritabanı, bir dizi özellikleri ile uyumludur. En son SQL veritabanı tarafından karşılandığından özellikleri görmek için ziyaret edin [Microsoft Trust Center](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) ve detaya gitme; SQL veritabanı uyumlu altında dahil edilmiş olup olmadığını görmek için kuruluşunuz için önemli olan özellikleri hakkında Azure Hizmetleri. Uyumlu bir hizmet SQL veritabanı sertifikalı ancak kuruluşunuzun hizmet uyumluluk kolaylık sağlar ancak bunu otomatik olarak garantilemez unutulmaması önemlidir.

## <a name="intelligent-database-monitoring-and-maintenance-after-migration"></a>Akıllı veritabanı izleme ve Bakım geçişten sonra

Veritabanınızın SQL veritabanı'na geçiş yaptıktan sonra (örneğin, kaynak kullanımını nasıl benzer denetimi veya DBCC denetimleri için), veritabanı izlemek istediğiniz edecek olan ve düzenli bakım gerçekleştirmek (örneğin, yeniden oluşturun veya reorganıze dizinleyen, istatistikler gibi.). Neyse ki, SQL veritabanı, geçmiş eğilimleri ve kaydedilen ölçümleri ve istatistikleri böylece uygulamanızın her zaman en iyi şekilde çalıştığı proaktif olarak izlemeye ve veritabanınızı korumaya yardımcı olması için kullandığını anlamda akıllı hizmetidir. Bazı durumlarda, Azure SQL DB, otomatik olarak yapılandırma kurulumunuza bağlı olarak bakım görevleri gerçekleştirebilirsiniz. Veritabanınızın SQL veritabanı'nda izleme için üç özellikleri vardır:

- Performans izleme ve iyileştirme.
- Güvenlik en iyi duruma getirme
- Maliyet iyileştirme.

### <a name="performance-monitoring-and-optimization"></a>Performans izleme ve iyileştirme

Sorgu performansı öngörüleri sayesinde, uygulamalarınızı çalışmaya devam en iyi düzeyde - her zaman böylece veritabanı iş yükünüz için özel olarak uyarlanmış önerileri alabilirsiniz. Ayrıca, böylece bu önerileri otomatik olarak uygulandığından ve bakım görevlerini gerçekleştirme rahatsız gerekmez ayarlayabilirsiniz. Dizin Danışmanı ile dizin önerileri, iş yüküne göre otomatik olarak uygulayabilir: Bu otomatik ayarlama çağrılır. Uygulama iş yükü değişikliklerinizi en ilgili öneriler sağlamak için öneriler değiştikçe. Ayrıca el ile bu önerileri gözden geçirin ve bunları istediğiniz kadar uygulama seçeneğine sahip olursunuz.  

### <a name="security-optimization"></a>Güvenlik en iyi duruma getirme

SQL veritabanı, tanımlama ve olası bir iş parçacığı veritabanına oluşturabileceği şüpheli veritabanı etkinlikleri araştırma için verileri ve tehdit algılama güvenli hale getirmenize yardımcı olmak için eyleme dönüştürülebilir güvenlik önerileri sağlar. [Güvenlik Açığı değerlendirmesi](sql-vulnerability-assessment.md) tarama ve veritabanlarınızı uygun ölçekte güvenlik durumunu izleme ve güvenlik risklerini tanımlayın ve tanımladığınız güvenlik çizgisinden kayma olanak tanıyan bir hizmet raporlama bir veritabanıdır. Her bir taramadan sonra eyleme dönüştürülebilir adımlar ve düzeltme betikleri özelleştirilmiş bir listesini, uyumluluk gereksinimlerini karşılamanıza yardımcı olmak için kullanılan bir değerlendirme raporu yanı sıra sağlanır.

Azure Güvenlik Merkezi ile güvenlik önerileri Pano genelinde tanımlamak ve bunları tek bir tıklamayla uygulayabilirsiniz.

### <a name="cost-optimization"></a>Maliyet iyileştirmesi

Azure SQL platformu kullanım geçmişini değerlendirir ve maliyet iyileştirmesi seçenekler önerilir sunucuya veritabanları arasında analiz eder. Bu analiz, genellikle çözümleyin ve eyleme dönüştürülebilir öneriler oluşturmak için bir fortnight alır. Elastik havuz, böyle bir seçenek ' dir. Öneri Portalı'nda bir başlık görünür:

![Elastik havuz önerileri](./media/sql-database-manage-after-migration/elastic-pool-recommendations.png)

Bu analiz "Advisor" bölümü altında da görüntüleyebilirsiniz:

![Elastik havuz önerileri-Danışman](./media/sql-database-manage-after-migration/advisor-section.png)

### <a name="how-do-i-monitor-the-performance-and-resource-utilization-in-sql-database"></a>SQL veritabanı kaynak kullanımı ve performansı nasıl izleyebilirim

SQL veritabanı'nda platformunun performansını izlemek ve buna uygun olarak ayarlamak için akıllı Öngörüler yararlanabilirsiniz. Performans ve kaynak kullanımı aşağıdaki yöntemleri kullanarak SQL veritabanı'nda izleyebilirsiniz:

#### <a name="azure-portal"></a>Azure portal

Azure portalında veritabanı seçme ve grafik genel bakış bölmesinde tıklatarak bir veritabanlarının kullanımını gösterir. CPU yüzdesi, DTU yüzdesi, veri g/ç yüzdesi, oturumları yüzdesi ve veritabanı boyutunun yüzdesi de dahil olmak üzere birden çok ölçüm göstermek için grafiğin değiştirebilirsiniz.

![Grafik izleme](./media/sql-database-manage-after-migration/monitoring-chart.png)

![Grafik2 izleme](./media/sql-database-manage-after-migration/chart.png)

Bu grafikte, kaynak tarafından uyarıları da yapılandırabilirsiniz. Bu uyarılar bir e-posta ile kaynak koşulları yanıt, bir HTTPS/HTTP uç noktasına yazma veya bir eylem gerçekleştirmeye olanak sağlar. Daha fazla bilgi için [uyarıları oluşturma](sql-database-insights-alerts-portal.md).

#### <a name="dynamic-management-views"></a>Dinamik Yönetim görünümleri

Sorgulayabilirsiniz [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) kaynak tüketimi istatistikleri geçmişi, son bir saat geri dönmek için dinamik yönetim görünümünü ve [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) döndürmek için sistem Katalog görünümü Geçmiş son 14 gündür.

#### <a name="query-performance-insight"></a>Sorgu Performansı İçgörüleri

[Sorgu performansı İçgörüleri](sql-database-query-performance.md) en çok kaynak tüketen sorguları ve belirli bir veritabanı için uzun süre çalışan sorguların geçmişini görmenizi sağlar. Kaynak kullanımı, süresi ve yürütme sıklığı en sık kullanılan sorgular hızla belirleyebilirsiniz. Sorguları izleyebilir ve regresyon algılayın. Bu özellik gerektirir [Query Store](/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) etkinleştirildiğinde ve veritabanı için etkin olması.

![Sorgu Performansı İçgörüleri](./media/sql-database-manage-after-migration/query-performance-insight.png)

#### <a name="azure-sql-analytics-preview-in-azure-monitor-logs"></a>Azure İzleyici günlüklerine Azure SQL Analytics (Önizleme)

[Azure İzleyici günlüklerine](../azure-monitor/insights/azure-sql.md) destekleyen en fazla 150.000 SQL veritabanları ve çalışma alanı başına 5.000 SQL elastik havuzları toplamanıza ve anahtar Azure SQL Azure performans ölçümleri görselleştirmenize olanak tanır. İzleme ve bildirimler almak için kullanabilirsiniz. Bir uygulama yığınının her katmanında sorunları belirlemek için kullanılabilir ve SQL veritabanı ve elastik havuz ölçümleri birden çok Azure abonelikleri ve elastik havuzlar arasında izleyebilirsiniz.

### <a name="i-am-noticing-performance-issues-how-does-my-sql-database-troubleshooting-methodology-differ-from-sql-server"></a>Performans sorunlarını bildirimde bulunmadan: SQL veritabanı benim sorun giderme Metodoloji SQL Server'dan farkı nedir

Sorun giderme teknikleri önemli bir kısmı, sorgu tanılamak için kullanacağınız ve veritabanı performans sorunlarını aynı kalır. Sonra tüm aynı SQL Server, bulut altyapısı çalıştırır. Ancak, platform - Azure SQL DB 'zekası' oluşturdu. Bu, hatta daha kolay performans sorunlarını tanılama ve giderme yardımcı olabilir. Bu da bu düzeltici eylemler bazılarını sizin adınıza ve bazı durumlarda gerçekleştirmek, proaktif olarak bunları - otomatik olarak düzeltin.

Performans sorunlarını giderme yönelik yaklaşımınızı gibi akıllı özellikleri kullanarak önemli ölçüde avantaj [sorgu performansı Insight(QPI)](sql-database-query-performance.md) ve [veritabanı Danışmanı](sql-database-advisor.md) birlikte ve Metodoloji fark farklıdır saygı – artık gerçekleştirmeniz gereken şekilde yardımcı olabilecek temel ayrıntıları grinding el ile çalışma eldeki sorunu giderme. Platform zor işleri sizin için halleder. Bir örneği QPI'yi ' dir. QPI'yi ile tüm sorgu düzeye ayrıntıya ve geçmiş eğilimleri bakın ve tam sorgu gerileyen, anlayabilir. Veritabanı Danışmanı, öneriler genel performansınızı dizinlerin eksik olması gibi - genel geliştirmenize yardımcı olabilecek bir şey, sorguları vb. kümesini parametreleştirme dizinleri bırakmayı sağlar.

Performans sorunlarını giderme ile yalnızca uygulamayı ya da uygulama performansınızı etkileyen, yedekleme, veritabanı tanımlamak önemlidir. Genellikle performans sorunu uygulama katmanında arasındadır. Mimari veya veri erişim modelini olabilir. Örneğin, sahip olduğunuz ağ gecikmesine duyarlıdır sık iletişim kuran bir uygulama düşünün. Olacaktır çünkü çok kısa istekleri gidip bu durumda, uygulamanızı alternatife Bu gidiş-dönüş ("geveze") uygulama sunucusu arasında ve yoğun ağ üzerinde hızlı ekleyin. Bu durumda performansını artırmak için kullanabileceğiniz [toplu işlem sorguları](sql-database-performance-guidance.md#batch-queries). İsteklerinizi bir toplu işte şimdi işlenemedi çünkü toplu kullanarak büyük ölçüde yardımcı olur; Bu nedenle, gidiş dönüş gecikmesine Kes ve uygulama performansınızı geliştirmenize yardımcı olur.

Veritabanınızı genel performansını düşüş fark ederseniz, ayrıca, izleyebilirsiniz [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) ve [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) dinamik yönetim görünümleri için CPU, GÇ ve bellek tüketimi anlayın. Veritabanınızı, kaynakları starved için performansınızı belki etkilendi. İşlem boyutu değiştirmek ve/veya hizmet katmanını büyüyen ve iş yükü taleplerini küçültme göre gerekebilir olabilir.

Kapsamlı bir performans sorunlarını ayarlama önerileri için bkz: [Veritabanınızı ayarlayın](sql-database-performance-guidance.md#tune-your-database).

### <a name="how-do-i-ensure-i-am-using-the-appropriate-service-tier-and-compute-size"></a>Uygun hizmet katmanını kullanıyorum ve işlem boyutu nasıl emin olabilirim

SQL veritabanı çeşitli hizmet katmanları temel, standart ve Premium sunar. Her bir hizmet katmanına bağlı söz konusu hizmet katmanı için garantili bir tahmin edilebilir performans alın. İş yükünüze bağlı olarak, burada kaynak kullanımınızı içinde geçerli işlem boyutu tavanını neden olabilir, etkinlik artışları olabilir. Herhangi bir ayar (örneğin ekleme veya bir dizin vb. değiştirme.) yardımcı olup olmadığını gibi durumlarda, değerlendirme ile başlamanız için kullanışlıdır. Hala sınır sorunlarla karşılaşırsanız, boyutu işlem ya da daha yüksek bir hizmet katmanına taşıyarak göz önünde bulundurun.

|**Hizmet katmanı**|**Genel kullanım örneği senaryoları**|
|---|---|
|**Temel**|Bazı kullanıcılar ve yüksek eşzamanlılık, ölçek ve performans gereksinimlerine sahip olmayan bir veritabanına sahip olan uygulamalar. |
|**Standart**|Önemli bir eşzamanlılık, ölçek ve performans gereksinimleri olan uygulamalar, Orta GÇ talepleri düşük ile bağlı. |
|**Premium**|Uygulama çok sayıda eşzamanlı kullanıcıyı, yüksek CPU/bellek ve yüksek g/ç ihtiyaç duyar. Premium düzeyi yüksek eşzamanlılık, yüksek aktarım hızı ve gecikme süresi önemli uygulamaları yararlanabilirsiniz. |
|||

Doğru işlem boyutu olduğunuzdan emin olmak, sorgu ve veritabanı kaynak tüketiminizi "Nasıl performans ve kaynak kullanımı SQL veritabanı'nda izleyebilirim", yukarıda belirtilen şekilde aracılığıyla izleyebilirsiniz. Sorgular/veritabanlarını tutarlı bir şekilde sık erişimli bir daha yüksek işlem boyutu için ölçeği artırmayı denemeniz CPU/bellek vb. üzerinde çalıştığını bulmanız gerekir. Benzer şekilde, hatta yoğun saatlerde unutmayın, kadar kaynakları kullanma görünmüyor; Geçerli işlem boyutundan ölçeği göz önünde bulundurun.

SaaS uygulama düzeni veya bir veritabanı birleştirme senaryo varsa, maliyet iyileştirmesi için elastik havuz kullanımının göz önünde bulundurun. Elastik havuz, veritabanı birleştirme ve maliyet iyileştirmesi elde etmek için harika bir yoludur. Daha fazla bilgi için elastik havuz kullanımının birden çok veritabanını yönetme hakkında bkz: [Havuzları ve veritabanlarını yönetmek](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases).

### <a name="how-often-do-i-need-to-run-database-integrity-checks-for-my-database"></a>Veritabanım için veritabanı tutarlılığı denetimleri çalıştırmak ne sıklıkla ihtiyacım var

SQL veritabanı otomatik olarak ve herhangi bir veri kaybı olmadan belirli sınıflarına ait veri bozulması işlemelerine izin akıllı bazı teknikleri kullanır. Bu teknikler, hizmette yerleşik olarak bulunan ve hizmet tarafından kullanılan gerektiğinde ortaya çıkar. Düzenli olarak, bunları geri yükleme ve DBCC CHECKDB üzerinde çalışan tarafından veritabanı Yedekleriniz hizmet arasında test edilmez. Sorun varsa, SQL veritabanı proaktif olarak bunları yöneliktir. [Otomatik onarım](/sql/sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring) sayfalarını bozuk veya veri bütünlüğü sorunları düzeltmek için kullanılır. Veritabanı sayfalar, sayfanın bütünlüğünü doğrular varsayılan sağlama TOPLAMINI Ayarla her zaman doğrulanır. SQL veritabanı proaktif bir şekilde izler ve veritabanınıza veri bütünlüğünü inceler ve en yüksek öncelikli sorunlar çıkması durumunda bunları adresleri. Bu ek olarak, isteğe bağlı olarak kendi bütünlük denetimi, gerçekleştirilse çalıştırmayı da seçebilirsiniz.  Daha fazla bilgi için [SQL veritabanında veri bütünlüğü](https://azure.microsoft.com/blog/data-integrity-in-azure-sql-database/)

## <a name="data-movement-after-migration"></a>Geçişten sonra veri taşıma

### <a name="how-do-i-export-and-import-data-as-bacpac-files-from-sql-database"></a>Nasıl dışarı aktarma ve verileri SQL veritabanından BACPAC dosyası olarak içeri

- **Dışarı aktarma**: Azure SQL veritabanınızı Azure portalından bir BACPAC dosyasını dışarı aktarabilirsiniz

   ![Veritabanı dışarı aktarma](./media/sql-database-export/database-export1.png)

- **İçeri aktarma**: Ayrıca Azure portalını kullanarak veritabanına BACPAC dosyası olarak veri da içeri aktarabilirsiniz.

   ![Veritabanı içeri aktarma](./media/sql-database-import/import1.png)

### <a name="how-do-i-synchronize-data-between-sql-database-and-sql-server"></a>SQL veritabanı ve SQL Server arasında verileri eşitleyebilmeniz nasıl

Bunu başarmak için birkaç yolunuz vardır:

- **[Veri eşitleme](sql-database-sync-data.md)**  – bu özellik çift birden çok şirket içi SQL Server veritabanları ve SQL veritabanı arasında verileri eşitleyebilmeniz yardımcı olur. Şirket içi SQL Server veritabanları ile eşitlemek için yüklemeniz, giden TCP bağlantı noktası 1433'ü açın ve ve yerel bilgisayarda eşitleme Aracısı yapılandırmanız gerekir.
- **[İşlem çoğaltma](https://azure.microsoft.com/blog/transactional-replication-to-azure-sql-database-is-now-generally-available/)**  – işlem çoğaltma ile şirket içi verilerinizi Azure SQL DB'ye yayımcısı ve abonesi olan Azure SQL DB ile şirket içi eşitleyebilirsiniz. Şu an için yalnızca bu kurulum desteklenir. Verilerinizin şirket içinden en az kapalı kalma süresi ile Azure SQL'e geçirme hakkında daha fazla bilgi için bkz: [İşlem çoğaltma kullanma](sql-database-single-database-migrate.md#method-2-use-transactional-replication)

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [SQL veritabanı](sql-database-technical-overview.md).
