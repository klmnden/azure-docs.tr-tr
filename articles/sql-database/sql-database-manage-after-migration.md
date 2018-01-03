---
title: "Geçişten sonra - Azure SQL veritabanını yönetme | Microsoft Docs"
description: "Azure SQL veritabanı geçiş sonrasında veritabanınızı yönetmeyi öğrenin."
services: sql-database
documentationcenter: 
author: joesackmsft
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Inactive
ms.date: 10/14/2016
ms.author: Joe.Sack
ms.suite: sql
ms.prod_service: sql-database
ms.component: migration
ms.openlocfilehash: e562c33cabc7d39d1f6a911c21343f85da205c0b
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="how-should-i-manage-my-azure-sql-database-after-migration"></a>Geçişten sonra nasıl Azure SQL veritabanını yönetebilirim?

*Sık sorulan sorular geçici Azure SQL veritabanı yatırımlarınızı yönetme*

Bu nedenle, en son SQL Server veritabanlarını Azure SQL veritabanına geçtiğinizi veya belki de kısa süre içinde taşımayı planlıyorsanız. Taşınamaz sonraki nedir? SQL veritabanı olması koşuluyla, bir *hizmet olarak Platform*, Microsoft sizin adınıza çeşitli alanları işler. Ancak bu, şirketinizin yöntemler güvenlik, iş sürekliliği, veritabanı bakımı, performans ayarlama, izleme ve daha fazlasını gibi önemli alanlar geçici tam olarak nasıl değişiyor mu? 

Kaynaklar ve SQL veritabanı yatırımlarınızı yönetmeye kaydırma yapmak için gereken yönergeler temellerini düzenlemek için bu makalede amacı budur. Bu makalede temel alanları, iş sürekliliği, güvenlik, veritabanı bakım ve izleme, performansı ve veri taşıma kapsar. Biz, SQL Server ve SQL veritabanı arasında farklıysa, avantajları en üst düzeye çıkarmak ve riski en aza indirmenize yardımcı olur işletimsel en iyi yöntemler çağıran önemli alanlar ele alacağız. 

## <a name="manage-business-continuity-after-migration"></a>Geçişten sonra iş sürekliliği yönetme

### <a name="how-do-i-create-and-manage-backups-on-sql-database"></a>Nasıl oluşturun ve SQL veritabanı yedeklemeleri yönetme? 
SQL veritabanı otomatik olarak sizin için veritabanlarını yedekler ve Bekletme dönemi zamanda herhangi bir noktaya kadar geri yükleme yeteneği sağlar. Bekletme süresi 35 gün standart ve Premium veritabanlarına yönelik ve temel veritabanları için 7 gün olur. Ayrıca, uzun vadeli bekletme özelliği, daha uzun bir süre (en fazla 10 yıl) için yedekleme dosyalarının tutun ve herhangi bir noktada bu yedeklerden geri yüklemek sağlar. Ayrıca, herhangi bir bölgede coğrafi geri yükleme yeteneği olağanüstü durum veya bölgesel catastrophe durumunda emin olmak için coğrafi çoğaltılmış veritabanı yedeklemeleri. Bkz: [iş sürekliliğine genel bakış](sql-database-business-continuity.md).

### <a name="how-do-i-ensure-business-continuity-in-the-event-of-a-datacenter-level-disaster-or-a-regional-catastrophe"></a>İş sürekliliği datacenter düzeyinde bir olağanüstü durum veya bölgesel catastrophe durumunda nasıl emin olursunuz? 

Bir olağanüstü durum veya bölgesel catastrophe durumunda herhangi bir bölgede coğrafi geri yükleme yeteneği sağlamak için coğrafi çoğaltılmış veritabanı yedeklemeleri. Bkz: [iş sürekliliğine genel bakış](sql-database-business-continuity.md). Ayrıca, SQL veritabanı aktif coğrafi olarak çoğaltılmış ikincil veritabanları başka bir bölgede korumak için yeteneği sağlar. Bir otomatik yük devretme grubu yapılandırma veritabanlarını otomatik olarak ikincil bir olağanüstü durum senaryosunda devreden olduğunu güvence altına alır. Bir otomatik yük devretme grubu yapılandırılmamışsa, uygulamanızın ikincil bir yük devretme başlatın ve etkin olarak için olağanüstü durum izlemek gerekir. 
### <a name="sql-server-provided-me-readable-secondary-replicas-can-i-access-the-secondaries-on-sql-database"></a>SQL Server bana okunabilir ikincil çoğaltmalarda sağlanan, ikincil SQL veritabanı üzerinde erişim? 

Evet, 'Etkin coğrafi çoğaltma' özelliği, okunabilir ikincil kopya oluşturmak için kullanılır. 

### <a name="how-does-my-disaster-recovery-plan-change-from-on-premise-to-sql-database"></a>Nasıl my olağanüstü durum kurtarma planı şirket içi SQL veritabanına değişiyor mu? 
SQL Server uygulamalarını etkin olarak yük devretme kümelemesi, veritabanı yansıtma, çoğaltma, günlük aktarma veya yalnızca düz temel alınan BACPAC yedeklemeler gibi özellikleri kullanarak yedeklemeler yönetmek gerekli. Ancak, SQL veritabanı yedeklemeleri tam olarak Microsoft tarafından yönetilir ve yapılandırılmış ve Azure Portalı'nı (veya birkaç PowerShell komutlarını) yalnızca birkaç tıklama ile çalışan yedekleme ve olağanüstü durum kurtarma planları yalnızca olabilir. ‌
### <a name="in-the-event-of-disaster-how-do-i-recover-my-databases"></a>Olağanüstü durumda nasıl ı my veritabanlarını kurtarmak? 
SQL veritabanı, otomatik olarak veritabanlarınızı zaman son 35 gün içinde herhangi bir noktaya kadar geri olanak sağlar. Veri kaybı ya da bir uygulama ile ilgili olağanüstü durum yüz bir seçenek budur. 

Coğrafi olarak çoğaltılmış ikincil veritabanlarıyla yapılandırılmışsa karşılaştığınız bölgesel bir olağanüstü durum halinde, başka bir bölgede coğrafi ikincil veritabanları kurtarabilir. Uygulamalarınıza gerçek zamanlı erişim için diğer bölgede coğrafi ikincil el ile yük. Alternatif olarak, yapılandırılmış otomatik yük devretme grubunuz varsa, bu yük devretme coğrafi ikincil bir olağanüstü durum senaryosunda otomatik olarak gerçekleşir. Yapılandırılmış coğrafi olarak çoğaltılmış ikincil veritabanına sahip değilseniz, veritabanlarınızı otomatik çoğaltılmış yedekleme dosyalarından kurtarmaya devam edebilirsiniz (yerleşik işlevsellik yapılandırma gerekli değildir), görece daha uzun kurtarma süresi (12 saat RTO) ve bir kadar saat veri kaybı. 

### <a name="are-the-failovers-to-secondary-transparent-how-does-my-application-handle-database-failovers"></a>Yük devretmeleri ikincil saydam misiniz? Uygulamam veritabanı yük devretme işlemlerini nasıl işler? 
Yapılandırılmış otomatik yük devretme gruplarınız varsa, ikincil için yük devretme saydamdır. Yüklemediyseniz, ancak sonra uygulamanızın birincil kullanılabilirliğini izleyin ve daha sonra el ile ikincil yük mantığını eklemenizi gerekir. 
 
## <a name="manage-security-after-migration"></a>Geçişten sonra güvenlik yönetme

### <a name="how-can-i-restrict-access-to-my-sql-database"></a>Nasıl ı my SQL veritabanına erişimi kısıtlayabilirsiniz? 
 
SQL veritabanları için bağlantı erişim kilitlemenize birkaç yolu vardır. 
1. Internet Express rota üzerinden sınırı trafik, ayrılmış fiber Azure ağını verir, böylece verilerinizi Internet üzerinden yolculuk değil. Hızlı rota kullanarak bölge bağlanabilirlik yapılandırmak da mümkündür. Aşağıdaki bağlantılar hızlı rota daha ayrıntılı olarak açıklanmıştır: 
 - [Hızlı rota üzerinde giriş](../expressroute/expressroute-introduction.md)
 - [Önkoşullar](../expressroute/expressroute-prerequisites.md) 
 - [İş akışları](../expressroute/expressroute-workflows.md) 
 
2. Hangi kaynakların SQL veritabanına bağlan'ı seçin: 

   Varsayılan olarak, SQL veritabanınız ise "tüm Azure hizmetlerini izin verecek şekilde" yapılandırılmış – tüm VM'ler için Azure'da başka bir deyişle, veritabanına bağlanma girişiminde bulunabilir.  Tüm oturumlarının kimlik doğrulaması yine olmalıdır. Tüm Azure IP'leri erişilebilir olması için veritabanının değil isterseniz, "tüm Azure hizmetlerini izin ver" devre dışı bırakıp kullanmak [VNET hizmet uç noktaları](sql-database-vnet-service-endpoint-rule-overview.md) içinde olan Azure kaynaklardan gelen veritabanına gelen erişimi kısıtlamak için bir verilen Azure sanal ağ alt ağı. 

   ![VNET Hizmeti uç noktaları](./media/sql-database-manage-after-migration/vnet-service-endpoints.png) 

   Sağlamak için alternatif bir seçenektir [ayrılmış IP'ler](../virtual-network/virtual-networks-reserved-public-ip.md) , VM'ler ve beyaz liste için VM IP adresleri Server'da belirli bir bu güvenlik duvarı ayarları. (Azure portalında bir örnek olarak aşağıdaki ekran görüntüsüne bakın.) Ayrılmış IP'ler atayarak IP adreslerinin değiştirilmesi ile güvenlik duvarı kurallarını güncelleştir gerek kalmadan, sorun kaydedin. 

3. Bağlantı noktası 1433 Azure dışında gösterme kaçının

   SSMS Azure kullanarak çalıştırmak [Azure RemoteApp](https://www.microsoft.com/cloud-platform/azure-remoteapp-client-apps). Bu 1433 bağlantı noktasına giden bağlantıları'nı açmak gerektirmez DB RemoteApp yalnızca açık şekilde IP statik, çok kullanıcılı ve çok faktörlü kimlik doğrulama (MFA) destekler. 

### <a name="what-authentication-methods-are-offered-in-sql-database"></a>Kimlik doğrulama yöntemleri, SQL veritabanında sunulur?

Temel kimlik doğrulama yöntemlerini SQL Database ve SQL Data Warehouse Azure Active Directory kimlik doğrulaması ve SQL kimlik doğrulaması ile sunulur. Azure Active Directory (AD) merkezi bir kimlik ve erişim yönetimi hizmettir ve SQL Azure AD ile tümleşik çoğu Azure hizmeti biridir. Merkezi olarak yönetilen bir hizmet avantajı, bir kullanıcının kimlik bilgilerini paylaşılan, daha basit kimlik doğrulaması için kullandığınız tüm Azure hizmetlerinde ' dir. Bu aynı zamanda SQL Database ve SQL Data Warehouse çok faktörlü kimlik doğrulaması ve Azure AD etki alanı içinde Konuk kullanıcı hesaplarını sunmanızı sağlar. 

Bir Active Directory şirket içi zaten varsa, dizininiz için Azure genişletmek için Azure Active Directory dizininizle. 


|||
|---|---|
| Eğer…|Azure SQL veritabanı / Azure SQL veri ambarı|
| Azure Active Directory (AD) ile Azure kullanmayı tercih eder|Kullanım [SQL kimlik doğrulaması](sql-database-security-overview.md)|
| SQL Server üzerinde kullanılan AD şirket içi|[Azure AD ile birleştirmek](../active-directory/connect/active-directory-aadconnect.md)ve Azure AD kimlik doğrulaması kullanın. Bu, çoklu oturum açma kullanabilirsiniz.|
| Çok faktörlü kimlik doğrulaması (MFA) zorunlu kılmanız gerekiyorsa|Bir ilke olarak MFA gerektirecek [Microsoft koşullu erişim](sql-database-conditional-access.md)ve [MFA desteği ile Azure AD Evrensel kimlik doğrulaması](sql-database-ssms-mfa-authentication.md).|
| Microsoft hesapları (live.com, outlook.com) veya diğer etki alanları (gmail.com) Konuk hesapları sahip|Kullanım [Azure AD Evrensel kimlik doğrulama](sql-database-ssms-mfa-authentication.md) SQL veritabanı/veri ambarında hangi yararlanır [Azure AD B2B işbirliği](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md).|
| Windows Azure AD kimlik bilgilerinizi kullanarak bir Federasyon etki alanından oturum açmış|Kullanım [Azure AD ile tümleşik kimlik doğrulaması](sql-database-aad-authentication-configure.md).|
| Windows Azure ile Federasyon olmayan bir etki alanı kimlik bilgilerini kullanarak oturum açmış|Kullanım [Azure AD parola kimlik doğrulaması](sql-database-aad-authentication-configure.md).|
| Orta katman Hizmetleri, bir Azure SQL veritabanı veya veri ambarına bağlanmak için gereken|Kullanım [Azure AD belirteci kimlik doğrulaması](sql-database-aad-authentication-configure.md).
|||

### <a name="how-can-i-limit-access-to-sensitive-data-in-my-databases-from-the-application-side"></a>Nasıl ı erişim için hassas verileri my veritabanlarında uygulama taraftan sınırlayabilirsiniz? 

Yetkisiz kullanıcıların hassas verileri görüntülenmesini engellemek için birkaç seçenek vardır SQL veritabanı'nda kullanılabilir: 

- [Her zaman şifreli](/sql/relational-databases/security/encryption/always-encrypted-database-engine) veritabanınızdaki hassas sütunlar (Veritabanı yöneticileri ve yetkisiz kullanıcılar şifreli olduğu için) şifreleyen istemci tarafı şifreleme biçimidir. Yalnızca yetkili istemcilerin hassas sütunları şifresini çözebilir için her zaman şifreli için anahtar istemci tarafında depolanır. Her zaman DBAs kendi SQL komutlarını bir parçası olarak şifrelenmiş sütunlar sorgulamaya devam edebilmek için eşitlik karşılaştırmaları Bugün, şifreli destekler. Her zaman şifreli anahtar deposu seçenekleri, çeşitli gibi kullanılabilir [Azure anahtar kasası](sql-database-always-encrypted-azure-key-vault.md), Windows sertifika deposunda ve yerel donanım güvenlik modülleri.
- [Dinamik veri maskeleme](sql-database-dynamic-data-masking-get-started.md) ayrıcalıklı olmayan kullanıcılara uygulama katmanı maskeleyerek gizli verilerin açığa sınırlayan bir veri maskeleme özelliğidir. Maskeleme deseni oluşturan bir maskeleme kuralı tanımlayın (örneğin, yalnızca Göster son 4 basamağı Ulusal kimliğinin ve rest olarak işaretlemek X'lerin) ve hangi kullanıcıların maskeleme kuraldan bırakılabileceğini belirleyin.
- [Satır düzeyi güvenlik](/sql/relational-databases/security/row-level-security) (grubu üyeliği veya yürütme bağlamı) sorgu yürütülürken kullanıcıya göre denetleme erişmek için bir veritabanı tablosundaki satırları sağlar. Erişim kısıtlaması veritabanı katmanında yerine uygulama mantığınızın basitleştirmek için bir uygulama katmanı üzerinde gerçekleştirilir. 

### <a name="what-encryption-options-do-i-have-in-sql-database-and-what-actors-does-the-encryption-protect-from"></a>Hangi şifreleme seçeneklerini SQL veritabanında sahibim ve hangi aktörler şifreleme karşı koruma sağlar?
SQL veritabanı'nda kullanılabilir üç ana şifreleme teknolojilerini vardır: 
- [Her zaman şifreli](/sql/relational-databases/security/encryption/always-encrypted-database-engine) (hangi belirtilen yukarıdaki sorunun): uçtan uca, fiziksel diske yetkisiz istemcilerden tablodaki hassas sütun şifreler. Şifreleme anahtarları istemcide depolandığından sunucu ve veri yöneticilerinin hassas verileri göremiyorum. 
- [Saydam veri şifreleme](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) (TDE): veritabanı düzeyinde şifreler ve veri dosyalarını, günlük dosyaları ve ilişkili yedeklemelerinizi fiziksel ortam hırsızlığından korur, bekleyen şifreleme. TDE, tüm yeni oluşturulan veritabanları için varsayılan olarak etkindir.
 
  Aşağıdaki diyagramda teknoloji seçimleri şifreleme özetini gösterir.

   ![Şifreleme genel bakış](./media/sql-database-manage-after-migration/overview-encryption.png)

### <a name="how-should-i-manage-encryption-keys-in-the-cloud"></a>Şifreleme anahtarları bulutta nasıl yönetmeniz gerekir? 
Anahtar yönetim seçeneklerini (istemci tarafı şifreleme)'her zaman şifrelenir ve saydam veri şifreleme (Bekleyen şifreleme) vardır. Şifreleme anahtarları düzenli olarak döndürmek için önerilen ve bir sıklıkta, iç düzenlemeleri ve uyumluluk ile gereksinimleri uyuşur.

- **Her zaman şifreli**: var olan bir [iki anahtar hiyerarşi](/sql/relational-databases/security/encryption/overview-of-key-management-for-always-encrypted) her zaman şifreli - gizli verilerin bir sütun sırayla sütun ana anahtar (CMK) ile şifrelenmiş bir AES 256 sütun şifreleme anahtarı (CEK) tarafından şifrelenir. Her zaman şifreli için sağlanan istemci sürücüleri CMKs uzunluğu hiçbir sınırlamaları vardır.

  CEK şifrelenmiş değerini veritabanında depolanır ve CMK Windows sertifika deposu, Azure anahtar kasası ya da bir donanım güvenlik modülü gibi güvenilir bir anahtar deposunda saklanır. 
  
  Her iki [CEK ve CMK](/sql/relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell) döndürülebilir. CEK döndürme şifrelenmiş sütunlar içeren tablolar boyutuna bağlı olarak zaman yoğunluklu olabilir. Bu nedenle, CEK döndürmeleri dikkatle planlayın. CMK döndürme, diğer yandan ile veritabanı performansını etkilemez ve ayrılmış rolleriyle yapılabilir.

  Aşağıdaki diyagramda her zaman şifreli sütun ana anahtarları için anahtar deposu seçeneklerini gösterir. 

   ![Her zaman şifreli CMK depolama sağlayıcıları](./media/sql-database-manage-after-migration/always-encrypted.png)

- **Saydam veri şifreleme (TDE)**: iki anahtar hiyerarşi içinde TDE – her kullanıcı veritabanındaki verileri sunucu benzersiz bir asimetrik RSA tarafından sırayla şifrelenir bir simetrik AES 256 veritabanı benzersiz bir veritabanı şifreleme anahtarı (DEK) tarafından şifrelenir 2048 ana anahtar. 

  Varsayılan olarak, ana anahtar saydam veri şifreleme için kolaylık sağlamak için SQL veritabanı hizmeti tarafından yönetilir. Kuruluşunuz ana anahtar üzerinde denetim kullanmak isterseniz, kullanılacak bir seçenek yoktur [Azure anahtar kasası](/sql/relational-databases/security/encryption/transparent-data-encryption-byok-azure-sql) anahtar deposu olarak. 

  Azure anahtar kasası kullanarak, kuruluşunuz anahtar sağlama, döndürme, izin denetimleri üzerinde denetim varsayar. [Döndürme veya TDE ana anahtar türü değiştirme](/sql/relational-databases/security/encryption/transparent-data-encryption-byok-azure-sql-key-rotation) , yalnızca DEK yeniden şifreler gibi hızlıdır. 

  Güvenlik ve veri yönetimi arasında rollerinin ayırmalı kuruluşlar için bir güvenlik yöneticisi sağlanamadı Azure anahtar Kasası'nda TDE ana anahtar için anahtar malzemesi ve için kullanılacak veritabanı yöneticisi için bir Azure anahtar kasası anahtar tanımlayıcısını belirtin bir sunucu üzerinde bekleyen şifreleme. 

## <a name="monitoring-and-compliance-after-migration"></a>İzleme ve uyum geçişten sonra

### <a name="how-do-i-monitor-database-activities-in-sql-database"></a>SQL veritabanında veritabanı etkinliklerini nasıl izlerim?
Birkaç vardır SQL veritabanına yerleşik özelliklerini izleme, hangi izlemek güvenlik ve diğer olayları veritabanı üzerinde:
- [SQL denetimi](sql-database-auditing.md) denetim günlüklerini Azure depolama hesabınızda veritabanı olayların toplamanızı sağlar.
- [SQL tehdit algılama](sql-database-threat-detection.md) ihlal etme veya veritabanındaki verileri yararlanma erişmek için bir olası kötü amaçlı belirten şüpheli etkinlikleri algılamanıza olanak tanır. SQL veritabanı tehdit algılama anormal veritabanı erişimi desenleri (örneğin, olağan dışı bir konumdan veya tanınmayan bir asıl tarafından erişim) yanı sıra olası güvenlik açıkları ve SQL ekleme saldırıları algılamak algoritmaları birden çok kez çalışır. Bir tehdit veritabanı üzerinde algılanırsa, güvenlik görevlileri veya diğer atanan Yöneticiler bir e-posta bildirimi alırsınız. Her bir bildirim hakkında daha fazla araştırmak ve tehdidi azaltmak öneriler ve şüpheli Etkinlik ayrıntıları sağlar. 
- [SQL güvenlik açığı değerlendirmesi](sql-vulnerability-assessment.md) tarama ve veritabanlarınızı ölçekli olarak güvenlik durumunu izlemenizi ve güvenlik risklerini belirlemeniz ve tarafından tanımlanan bir güvenlik taban çizgisiyle kayma sağlayan hizmet raporlama bir veritabanıdır. Her taramadan sonra tıklatılabilir adımları ve düzeltme betikleri özelleştirilmiş bir listesi sunulur yardımcı olduğunuz için Uyumluluk gereklerini karşılamak için kullanılabilir bir değerlendirme raporu yanı sıra. 
- [SQL OMS güvenlik eşitleme uygulama](https://github.com/Microsoft/Azure-SQL-DB-auditing-OMS-integration) için günlük analizi ve özelliği de dahil olmak üzere özel algılama uyarıları tanımlamak için OMS SQL denetim günlükleri göndermek için ortak API'ler Operations Management Suite (OMS) kullanır: 
 - SQL veritabanı denetimi döşeme & Pano, veritabanı etkinliklerini NET ve tutarlı raporun sağlayan. 
 - Veritabanı etkinliklerinizi çözümlemek ve ifade eden tutarsızlıklar ve şüphe duyulan güvenlik ihlalleri gösterebilir anormallikleri araştırmak için SQL günlük analizi.
 - Uyarıları belirli kurallar e-posta tetikleyen gözlemlenen olaylarına Gelişmiş, Web kancası ve Azure Otomasyonu runbook'u (yani parola değişiklikleri, after-hours, belirli SQL komutlarını) uyarır.
- [Azure Güvenlik Merkezi](../security-center/security-center-intro.md) Azure, şirket içi ve diğer Bulutları çalışan iş yükleri arasında Merkezi güvenlik yönetimi sunar. Görüntüleyebileceğiniz denetim ve saydam veri şifreleme gibi temel SQL veritabanı korumasını tüm kaynaklara yapılandırılır ve kendi gereksinimlerinize göre ilkeleri oluşturmak isteyip. 

### <a name="is-sql-database-compliant-with-any-regulatory-requirements-and-how-does-that-help-with-my-own-organizations-compliance"></a>SQL veritabanı Yasal gereksinimlere ile uyumlu olan ve, kendi kuruluşunuzun uyumluluğun nasıl yardımcı olur? 
Azure SQL veritabanı yasal compliances aralığı ile uyumludur. Karşılandığından compliances son kümesini görüntülemek için ziyaret [Microsoft Trust Center](https://www.microsoft.com/trustcenter/compliance/complianceofferings) ve Azure SQL veritabanı altında uyumlu Azure dahil edilmiş olup olmadığını görmek için kuruluşunuz için önemli olan compliances üzerinde detaya Hizmetler. Azure SQL veritabanı uyumlu bir hizmet sertifikalı ancak kuruluşunuzun hizmet uyumlu yardımları ancak otomatik olarak bunu garanti etmez dikkate almak önemlidir. 

## <a name="database-maintenance-and-monitoring-after-migration"></a>Veritabanı bakım ve geçiş sonrasında izleme

### <a name="how-do-i-monitor-growth-in-data-size-and-resource-utilization"></a>Veri boyutu ve kaynak kullanımı büyüme nasıl izlerim?

- Veritabanı boyutu ve kaynak kullanımı Azure portalında 'İzleme' grafikte ilgili İzleyicisi ölçümleri görüntüleyebilirsiniz. 

  ![Grafik izleme](./media/sql-database-manage-after-migration/monitoring-chart.png)

- Daha derin hakkında bilgi edinme ve sorguları ayrıntıları detaya için Azure Portal'da 'Sorgu performansı öngörüleri' kullanılabilir kullanabilirsiniz. Bu 'Query Store' veritabanınızda etkin olduğunu gerektirir.

  ![Sorgu Performansı Öngörüleri](./media/sql-database-manage-after-migration/query-performance-insight.png)

- Alternatif olarak, dinamik yönetim görünümlerini (Dmv'leri) çok kullanarak - kullanarak ölçümleri görüntüleyebilirsiniz [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) ve [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database). 

### <a name="how-often-do-i-need-to-run-consistency-checks-like-dbcccheckdb"></a>Tutarlılık denetimleri gibi DBCC_CHECKDB çalıştırmak ne sıklıkta gerekiyor mu?
DBCC_CHECKDB veritabanındaki tüm nesneleri mantıksal ve fiziksel bütünlüğünü denetler. Artık bu Microsoft Azure tarafından yönetilen çünkü bu denetimler yapmanız gerekir. Daha fazla bilgi için bkz: [Azure SQL veritabanında veri bütünlüğü](https://azure.microsoft.com/blog/data-integrity-in-azure-sql-database/)

## <a name="monitor-performance-and-resource-utilization-after-migration"></a>Geçişten sonra performans ve kaynak kullanımını izleme

### <a name="how-do-i-monitor-performance-and-resource-utilization-in-azure-sql-database"></a>Performans ve kaynak kullanımını Azure SQL veritabanında nasıl izlerim?
Azure SQL veritabanında aşağıdaki yöntemleri kullanarak performans ve kaynak kullanımını izleyebilirsiniz:

- **Azure portal**: Azure portal veritabanı seçerek ve grafik genel bakış bölmesinde tıklatarak tek veritabanlarının kullanımını gösterir. CPU yüzdesi, DTU yüzdesi, veri g/ç yüzdesi, oturumlar yüzdesi ve veritabanı boyutunun yüzdesi dahil olmak üzere birden çok ölçümleri göstermek için grafik değiştirebilirsiniz. 
  ![Kaynak kullanımı](./media/sql-database-manage-after-migration/resource-utilization.png)

  Bu grafik kaynak tarafından uyarıları da yapılandırabilirsiniz. Bu uyarılar kaynak durumları bir e-posta ile yanıt, bir HTTPS/HTTP uç noktasına yazmak veya bir eylem gerçekleştirmek izin verin. Bkz: [Azure SQL database'de veritabanı performansını izleme](sql-database-single-database-monitor.md) ayrıntılı yönergeler için.

- **Görünümler**: Sorgulayabileceğiniz [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) kaynak tüketimi istatistikleri geçmişini son bir saat geri dönmek için dinamik yönetim görünümünü ve [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) Son 14 gün boyunca geçmişi döndürmek için sistem Katalog görünümü. 

- **Sorgu performansı öngörüleri**: [sorgu performansı öngörüleri](sql-database-query-performance.md) üst kaynak tüketen sorguları ve uzun süre çalışan sorgular için belirli bir veritabanı geçmişin görmenizi sağlar. Bu özellik gerektirir [Query Store](/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) etkin ve veritabanı için etkin olmalıdır.

- **Günlük analizi Azure SQL analizleri (Önizleme)**: [Azure günlük analizi](../log-analytics/log-analytics-azure-sql.md) destekleyen en fazla 150.000 Azure SQL veritabanları ve 5.000 SQL esnek toplamak ve anahtar Azure SQL Azure performans ölçümleri, görselleştirme olanak tanır Çalışma alanı başına havuzları. İzleme ve bildirimleri almak için kullanabilirsiniz. Azure SQL veritabanı ve esnek havuz ölçümleri birden çok Azure abonelikleri ve esnek havuzlar arasında izleyebilir ve bir uygulama yığınının her katmanda sorunları belirlemek için kullanılabilir. 

### <a name="how-do-i-ensure-i-am-using-the-appropriate-service-tier-and-performance-level"></a>İlgili hizmeti katmanını ve performans düzeyini kullanıyorum nasıl emin olursunuz?
İzleyici [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) ve [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) CPU, g/ç ve bellek tüketimini anlamak için dinamik yönetim görünümlerini. SQL veritabanı de kullanabilirsiniz [sorgu performansı öngörüleri](sql-database-query-performance.md) kaynak tüketimini görmek için. Yüksek kullanılabilir kaynakları yüzdesiyle sürekli olarak çalıştırıyorsanız, varolan hizmet katmanı içinde daha yüksek bir performans düzeyine taşımayı deneyin ya da daha yüksek bir hizmet katmanına taşıyın. Buna karşılık, sürekli olarak kullanılabilir kaynakları düşük yüzdesi kullanıyorsanız, daha düşük bir performans düzeyine taşımayı deneyin veya hizmet katmanını.

### <a name="i-am-seeing-performance-issues-how-does-my-azure-sql-database-troubleshooting-methodology-differ-from-sql-server"></a>Performans sorunlarını görüyorum. My Azure SQL veritabanı sorun giderme Metodoloji SQL Server'dan nasıl farkı nedir?
Performans sorunlarını giderme yönteminize pek çok görünüşünün Azure SQL veritabanında aynı kalır ancak bazı farklılıklar olacaktır. Örneğin, bir genel performans düşüşü görürseniz, izleme [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) ve [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) dinamik yönetim görünümlerini CPU, g/ç ve bellek olduğunu anlamak için Tüketim. Performans düzeyini değiştirmek ve/veya hizmet katmanını iş yükü taleplerine göre gerekebilir.
Kapsamlı bir performans sorunlarını ayarlama önerileri için bkz: [Azure SQL veritabanında performans ayarlaması](sql-database-performance-guidance.md). 

### <a name="do-i-need-to-maintain-indexes-and-statistics"></a>Dizinler ve İstatistikler korumak gerekiyor mu?
Azure SQL veritabanı, dizinler ve İstatistikler hizmetin bir parçası olarak otomatik olarak korumaz. Dizinler ve istatistikler, bakım zamanlaması için sorumlu. Aşağıdaki makalede, Azure Automation yöntemleri, Azure SQL veritabanında bakım işleri zamanlamak için birkaç seçenek ayrıntılarını verir.

## <a name="data-movement-after-migration"></a>Geçişten sonra veri taşıma

### <a name="how-do-i-export-and-import-data-as-bacpac-files-from-azure-sql-database"></a>Nasıl dışarı aktarma ve BACPAC dosyalarıyla Azure SQL veritabanından veri içeri aktarın? 

- **Dışarı aktarma**: Azure SQL veritabanınıza Azure portalından bir BACPAC dosyası olarak dışa aktarabilirsiniz.

  ![Bir BACPAC dışarı aktarma](./media/sql-database-export/database-export.png)

- **Alma**: Azure portalını kullanarak bir veritabanı BACPAC dosyası olarak içeri aktarabilirsiniz.

  ![Bir BACPAC alma](./media/sql-database-import/import.png)

### <a name="how-do-i-synchronize-data-between-azure-sql-database-sql-server-2016--2012"></a>Ne ı eşitleme Azure SQL veritabanı SQL Server 2016 arasında veri / 2012?
[Veri eşitleme](sql-database-sync-data.md) özelliği birden çok şirket içi SQL Server veritabanları ve Azure SQL veritabanı arasında çift yönlü veri eşitleme yardımcı olur. Ancak, bu tetikleyici bağlı olduğundan, nihai tutarlılık (hiçbir veri kaybı) garanti ancak işlem tutarlılığı garanti edilmez. 

## <a name="next-steps"></a>Sonraki adımlar
Hakkında bilgi edinin [Azure SQL veritabanı](sql-database-technical-overview.md).
