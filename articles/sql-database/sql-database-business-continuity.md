---
title: Bulutta iş sürekliliği - veritabanı kurtarma - SQL Veritabanı | Microsoft Belgeleri
description: Azure SQL Veritabanı'nın bulutta iş sürekliliği ve veritabanı kurtarmanın yanı sıra görev açısından kritik bulut uygulamalarının kesintisiz çalışması için sunduğu destek özelliklerini öğrenin.
keywords: iş sürekliliği, bulutta iş sürekliliği, veritabanı olağanüstü durum kurtarma, veritabanı kurtarma
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.workload: On Demand
ms.date: 04/04/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 0399b9037e162aa712b87b498b968750226af23a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34646397"
---
# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Azure SQL Veritabanı'nda iş sürekliliğine genel bakış

Bu genel bakış, Azure SQL Veritabanı'nın iş sürekliliği ve olağanüstü durum kurtarma özelliklerini açıklamaktadır. Seçenekler, öneriler ve veri kaybına neden ya da veritabanı ve uygulamanızın kullanılamaz hale gelmesine neden kesintiye uğratan olaylarından kurtarmak için öğreticiler hakkında bilgi edinin. Bir kullanıcı veya uygulama hatası veri bütünlüğü etkiler, bir Azure bölgesinin bir kesinti veya bakım uygulamanızın gerektirdiği olmadığında yapmanız gerekenler hakkında bilgi edinin.

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>İş sürekliliği sağlamak için kullanabileceğiniz SQL Veritabanı özellikleri

SQL Veritabanı; otomatik yedekleme ve isteğe bağlı veritabanı çoğaltma gibi birçok iş sürekliliği özelliği sunar. Her özellik, tahmini kurtarma süresi (ERT) ve son işlemler için olası veri kaybı açısından farklı özelliklere sahiptir. Bu seçenekleri kavradıktan sonra aralarından birini seçebilir ve çoğu senaryoda farklı durumlar için birden fazlasını birlikte kullanabilirsiniz. İş sürekliliği planınızı geliştirirken, uygulamanın kesintiden sonra tamamen kurtarılmasına kadar kabul edilebilen maksimum süre olan kurtarma süresi hedefini (RTO) belirlemeniz gerekir. Ayrıca en fazla son veri miktarı anlamak ihtiyacınız uygulama güncelleştirmeleri (zaman aralığı) tolerans kesintiye uğratan olayından sonra kurtarırken kaybetme - Bu, kurtarma noktası hedefi (RPO).

Aşağıdaki tabloda, her hizmet katmanı üç yaygın senaryo için Ekle ve RPO karşılaştırır.

| Özellik | Temel | Standart | Premium  | Genel Amaçlı | İş Açısından Kritik
| --- | --- | --- | --- |--- |--- |
| Yedekten belirli bir noktaya geri yükleme |7 gün içinde herhangi bir geri yükleme noktasına |35 gün içinde herhangi bir geri yükleme noktasına |35 gün içinde herhangi bir geri yükleme noktasına |Yapılandırılan süre (35 gün) içinde herhangi bir geri yükleme noktası|Yapılandırılan süre (35 gün) içinde herhangi bir geri yükleme noktası|
| Coğrafi olarak çoğaltılmış yedeklerden coğrafi geri yükleme |ERT < 12 sa., RPO < 1 sa |ERT < 12 sa., RPO < 1 sa |ERT < 12 sa., RPO < 1 sa |ERT < 12 sa., RPO < 1 sa|ERT < 12 sa., RPO < 1 sa|
| Azure Backup Vault'tan geri yükleme |ERT < 12 sa., RPO < 1 hft. |ERT < 12 sa., RPO < 1 hft. |ERT < 12 sa., RPO < 1 hft. |ERT < 12 sa., RPO < 1 hft.|ERT < 12 sa., RPO < 1 hft.|
| Aktif coğrafi çoğaltma |ERT < 30 sn., RPO < 5 sn. |ERT < 30 sn., RPO < 5 sn. |ERT < 30 sn., RPO < 5 sn. |ERT < 30 sn., RPO < 5 sn.|ERT < 30 sn., RPO < 5 sn.|

### <a name="use-point-in-time-restore-to-recover-a-database"></a>Bir veritabanını kurtarmak için zaman içinde nokta geri yükleme kullanın

SQL veritabanı otomatik olarak tam veritabanı yedeklemeleri haftalık bir birleşimini gerçekleştirir, artımlı veritabanı yedeklemeleri saatlik ve işlem yedeklemeleri her beş - on dakika işletmenizi veri kaybına karşı koruyun oturum açın. Kullanıyorsanız, [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md), bu yedeklemeler veritabanları standart ve Premium hizmet katmanları ve temel hizmet katmanındaki veritabanları için 7 gün için 35 gün için RA-GRS depolama alanında depolanır. Hizmet katmanızın saklama süresi işletmenizin ihtiyaçlarını karşılamıyorsa, [hizmet katmanını değiştirerek](sql-database-service-tiers-dtu.md#choosing-a-service-tier-in-the-dtu-based-purchasing-model) saklama süresini uzatabilirsiniz. Kullanıyorsanız, [vCore tabanlı satın alma modeli (Önizleme)](sql-database-service-tiers-vcore.md), genel amaçlı ve iş kritik katmanları yedeklemeleri bekletme 35 gün yapılandırılabilir ayarlama. Tam yedekler ve değişiklik yedekleri, veri merkezi kesintilerine karşı [eşleştirilmiş veri merkezine](../best-practices-availability-paired-regions.md) de çoğaltılır. Daha fazla bilgi için bkz: [otomatik veritabanı yedeklemeyi](sql-database-automated-backups.md).

Maksimum desteklenen PITR saklama dönemi, uygulamanız için yeterli değilse, veritabanları için uzun vadeli bir bekletme (LTR) ilkesini yapılandırarak genişletebilirsiniz. Daha fazla bilgi için bkz. [Uzun süreli saklama](sql-database-long-term-retention.md).

Bu otomatik veritabanı yedekleme özelliklerini kullanarak hem kendi veri merkezinizdeki hem de başka veri merkezlerindeki veritabanlarını çeşitli kesintilerden kurtarabilirsiniz. Otomatik veritabanı yedekleriyle tahmini kurtarma süresi, aynı anda aynı bölgede kurtarılan veri tabanı sayısı, veritabanı boyutu, işlem günlüğü boyutu ve ağ bant genişliği gibi birden fazla etmene göre değişiklik gösterir. Kurtarma süresi genellikle değerinden 12 saattir. Başka bir veri bölgesine kurtarma gerçekleştirirken, potansiyel veri kaybı saatlik veritabanı değişiklik yedeklerinin coğrafi olarak yedekli olması sayesinde 1 saatle sınırlıdır.

> [!IMPORTANT]
> Otomatik yedekleri kullanarak kurtarma gerçekleştirmek için SQL Server Katılımcısı rolü üyesi veya abonelik sahibi olmanız gerekir. [RBAC: Yerleşik roller](../role-based-access-control/built-in-roles.md). Verileri Azure portalı, PowerShell veya REST API kullanarak kurtarabilirsiniz. Transact-SQL kullanamazsınız.
>

Uygulamanız aşağıdaki koşullara uygunsa iş sürekliliği ve kurtarma hizmeti olarak otomatik yedeklemeyi kullanabilirsiniz:

* Görev açısından kritik değilse.
* Bir bağlama SLA - 24 saatlik bir kapalı kalma süresi yok veya daha uzun süre içinde finansal yükümlülük sonuçlanmaz.
* Veri değişiklik oranı (bir saat içinde gerçekleştirilen işlem sayısı) düşükse ve bir saatlik değişiklik kaybı kabul edilebilirse.
* Maliyetler önemliyse.

Daha hızlı kurtarma gerekiyorsa kullanın [aktif coğrafi çoğaltma](sql-database-geo-replication-overview.md) (sonraki bölümde açıklanmaktadır). Veri 35 gün sayısından önceki bir dönemden kullanmak kurtarmanız mümkün olması gerektiğinde [uzun vadeli bekletme](sql-database-long-term-retention.md). 

### <a name="use-active-geo-replication-and-auto-failover-groups-in-preview-to-reduce-recovery-time-and-limit-data-loss-associated-with-a-recovery"></a>Kurtarma zamanı azaltır ve bir kurtarma ile ilişkilendirilen veri kaybı sınırlamak için etkin coğrafi çoğaltma ve otomatik yük devretme gruplar (Önizleme-) kullanın

Kullanabileceğiniz bir iş kesintisi oluşursa Veritabanı Yedeklemeleri veritabanı kurtarma için kullanmanın yanı sıra [aktif coğrafi çoğaltma](sql-database-geo-replication-overview.md) tercih ettiğiniz bölgelerde en fazla dört okunabilir ikincil veritabanları için veritabanını yapılandırmak için. Bu ikincil veritabanları, birincil veritabanıyla zaman uyumsuz çoğaltma sistemi kullanılarak eşitlenir. Bu özellik, veri merkezi kesintisinden oluşursa veya uygulama yükseltme sırasında iş kesintisi karşı korumak için kullanılır. Aktif coğrafi çoğaltma, coğrafi olarak dağınık kullanıcılara salt okunur sorgular için daha iyi sorgu performansını sağlamak için de kullanılabilir.

Etkinleştirmek için otomatik ve şeffaf yük devretme, düzenlemek coğrafi olarak çoğaltılmış veritabanlarınızı kullanarak gruplar halinde [otomatik yük devretme grubu](sql-database-geo-replication-overview.md) SQL veritabanı (Önizleme-) özelliğidir.

Birincil veritabanı beklenmedik şekilde çevrimdışı kaldığında veya çevrimdışı bakım etkinlikler için atmanız gereken varsa (bir yük devretme olarak da bilinir) birincil hale ve yükseltilen birincil bağlanmak için uygulamaları yapılandırmak için ikincil bir hızlı bir şekilde yükseltebilirsiniz. Uygulamanızı veritabanlarını yük devretme grubu dinleyicisi kullanarak bağlanıyorsa, yük devretme sonrasında SQL bağlantı dizesi yapılandırmasını değiştirmeniz gerekmez. Planlı yük devretme sayesinde veri kaybı yaşanmaz. Plansız yük devretme durumunda ise zaman uyumsuz çoğaltmanın özelliğinden dolayı en son işlemler için az miktarda veri kaybı yaşanabilir. Otomatik Yük devretme grupları (Önizleme-) kullanarak, olası veri kaybını en aza indirmek için yük devretme İlkesi özelleştirebilirsiniz. Yük devretme sonrasında belirli bir plana göre veya veri merkezi çevrimiçi duruma geldiğinde veritabanını yeniden çalıştırabilirsiniz. Tüm durumlarda kullanıcılar kısa süreli bir kesinti yaşar ve yeniden bağlantı kurmaları gerekir.

> [!IMPORTANT]
> Aktif coğrafi çoğaltma ve otomatik yük devretme grupları (Önizleme-) kullanmak için abonelik sahibi olmanız veya SQL Server yönetici izinlerine sahip gerekir. Yapılandırma ve Azure kullanarak üzerinden başarısız portal, PowerShell veya Azure aboneliği izinleri veya SQL Server izinleriyle Transact-SQL kullanarak REST API.
> 

Uygulamanız bu ölçütler hiçbirini karşılıyorsa etkin coğrafi çoğaltma ve otomatik yük devretme gruplar (önizlemede) kullanın:

* Görev açısından kritikse.
* 24 saat veya üzeri kesinti süresine izin vermeyen hizmet düzeyi sözleşmesine (SLA) sahipse.
* Kapalı kalma süresi içinde finansal yükümlülük neden olabilir.
* Veri değişiklik oranı yüksekse ve bir saatlik değişiklik kaybı kabul edilebilir değilse.
* Etkin coğrafi çoğaltma ek maliyeti, olası mali yükümlülükten veya ilgili iş kaybından daha düşükse.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-protecting-important-DBs-from-regional-disasters-is-easy/player]
>

## <a name="recover-a-database-after-a-user-or-application-error"></a>Kullanıcı veya uygulama hatasından sonra bir veritabanını kurtarma

Hiç kimse mükemmeldir! Bir kullanıcı yanlışlıkla veri silebilir, istemeden önemli bir tabloyu ve hatta bir veritabanının tamamını bırakabilir. Ya da bir uygulama, hata nedeniyle yanlışlıkla gereksiz verileri gerekli verilerin üzerine yazabilir.

Bu senaryoda kullanabileceğiniz kurtarma seçenekleri aşağıda verilmiştir.

### <a name="perform-a-point-in-time-restore"></a>Belirli bir noktaya geri yükleme gerçekleştirme
Noktanın veritabanı saklama süresi içinde olması şartıyla otomatik yedekleri kullanarak veritabanınızın bir kopyasını belirli bir noktaya geri yükleyebilirsiniz. Veritabanı geri yüklendikten sonra özgün veritabanını geri yüklenen veritabanıyla değiştirebilir veya geri yüklenen veritabanından gerekli verileri özgün veritabanına kopyalayabilirsiniz. Veritabanı aktif coğrafi çoğaltma kullanırsa, özgün veritabanına geri yüklenen kopyadan gerekli verileri kopyalayarak öneririz. Özgün veritabanının geri yüklenen veritabanı ile değiştirirseniz, yeniden yapılandırın ve etkin coğrafi (hangi oldukça büyük bir veritabanı için biraz zaman alabilir) çoğaltma yeniden eşitlemek gerekir. Bu bir veritabanı son geri yüklerken coğrafi ikincil zamanında herhangi bir noktaya kadar geri kullanılabilir bir noktada, şu anda desteklenmiyor.

Azure portal veya PowerShell kullanarak bir veritabanını belirli bir noktaya geri yüklemeyle ilgili ayrıntılı adımlar ve daha fazla bilgi için bkz. [belirli bir noktaya geri yükleme](sql-database-recovery-using-backups.md#point-in-time-restore). Transact-SQL kullanarak kurtarma gerçekleştiremezsiniz.

### <a name="restore-a-deleted-database"></a>Silinen veritabanını geri yükleme
Veritabanı silinmiş ancak mantıksal sunucu silinmemişse, silinen veritabanını silindiği noktaya geri yükleyebilirsiniz. Bu işlem bir veritabanı yedeğini silindiği mantıksal SQL sunucusuna geri yükler. Özgün adı kullanarak geri yükleyebilir veya geri yüklenen veritabanı için yeni bir ad belirleyebilirsiniz.

Azure portal veya PowerShell kullanarak silinen bir veritabanını geri yüklemeyle ilgili ayrıntılı adımlar ve daha fazla bilgi için bkz. [silinen veritabanını geri yükleme](sql-database-recovery-using-backups.md#deleted-database-restore). Transact-SQL kullanarak geri yükleme gerçekleştiremezsiniz.

> [!IMPORTANT]
> Mantıksal sunucu silinmişse, silinen bir veritabanını kurtaramazsınız.


### <a name="restore-backups-from-long-term-retention"></a>Yedeklemeleri uzun vadeli bekletme geri yükleme

Otomatik yedeklemeler için geçerli Bekletme dönemi dışında veri kaybı oluştu ve veritabanınızı uzun vadeli bekletme için yapılandırılmışsa, LTR depolama tam bir yedekten yeni bir veritabanına geri yükleyebilirsiniz. Bu noktada özgün veritabanını geri yüklenen veritabanıyla değiştirebilir veya geri yüklenen veritabanından gerekli verileri özgün veritabanına kopyalayabilirsiniz. Veritabanınızı ana uygulama yükseltmeden önce eski bir sürümünü almak gerekiyorsa, denetçi veya yasal bir sipariş isteğinden Azure yedekleme kasasında kayıtlı tam yedekleme kullanarak bir veritabanı oluşturabilirsiniz karşılamak.  Daha fazla bilgi için bkz. [Uzun süreli saklama](sql-database-long-term-retention.md).

## <a name="recover-a-database-to-another-region-from-an-azure-regional-data-center-outage"></a>Azure bölgesel veri merkezi kesintisinde bir veritabanını başka bir bölgeye kurtarma
<!-- Explain this scenario -->

Çok sık olmasa da Azure veri merkezlerinde kesintiler yaşanabilir. Kesinti yaşandığında yalnızca birkaç dakika sürebilecek veya saatler alacak bir hizmet kesintisi söz konusu olabilir.

* Seçeneklerden biri, veri merkezi kesintisi sona erdiğinde veritabanınızın çevrimdışı olmasını beklemektir. Bu, veritabanının çevrimdışı olmasının kabul edilebildiği uygulamalar için geçerlidir. Örnek olarak üzerinde sürekli çalışma yapmadığınız bir geliştirme projesi veya ücretsiz deneme sürümü verilebilir. Bir veri merkezinde bir kesinti olduğunda, böylece veritabanınızı biraz gerekmiyorsa, bu seçenek yalnızca çalışır, ne kadar süreyle kesinti son, bilmezsiniz.
* Aktif coğrafi çoğaltma veya kurtarma kullanıyorsanız, başka bir seçenek ya da başarısız üzerinden başka bir veri bölgesine coğrafi olarak yedekli veritabanı yedeklemeleri (coğrafi geri yükleme) kullanarak bir veritabanıdır. Veritabanı kurtarma yedeklerden saat sürer ancak yük devretme yalnızca birkaç saniye sürer.

Ne zaman önlem, kurtarmanızı süreyi ve tabi ne kadar veri kaybını nasıl uygulamanızda bu iş sürekliliği özelliklerini kullanmaya karar üzerine bağlıdır. Aslında, veritabanı yedeklemeleri ve etkin coğrafi uygulama gereksinimlerinize bağlı olarak çoğaltma bileşimini kullanmayı seçebilirsiniz. Bu iş sürekliliği özelliklerini kullanan bağımsız veritabanları ve elastik havuzlarla ilgili dikkate alınacak uygulama tasarımı noktaları hakkında ayrıntılı bilgi için bkz. [Bulutta olağanüstü durum kurtarma için uygulama tasarlama](sql-database-designing-cloud-solutions-for-disaster-recovery.md) ve [Elastik havuz olağanüstü durum kurtarma stratejileri](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

Aşağıdaki bölümlerde veritabanı yedeklemelerini veya aktif coğrafi çoğaltma kullanarak kurtarma adımlarını genel bir bakış sağlar. Planlama gereksinimleri, post kurtarma adımları ve kesinti benzetimini yapma hakkında bilgi bir olağanüstü durum kurtarma ayrıntıya gerçekleştirmek için de dahil olmak üzere ayrıntılı adımlar için bkz: [bir SQL veritabanı bir kesintisinden kurtarma](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Kesinti için hazırlanma
Kullandığınız iş sürekliliği özelliğinden bağımsız olarak aşağıdaki adımları uygulamanız gerekir:

* Sunucu düzeyi güvenlik duvarı kuralları, giriş bilgileri ve ana veritabanı düzeyi izinler dahil olmak üzere hedef sunucuyu belirlemek ve hazırlamak
* İstemcilerin ve istemci uygulamalarının yeni sunucuya nasıl yönlendirileceğini belirlemek
* Denetim ayarları ve uyarılar gibi diğer bağımlılık belgelerini oluşturmak

Ayrıca, düzgün şekilde, bir yük devretme veya bir veritabanı kurtarma ek süre gerçekleştirildikten sonra çevrimiçi ve büyük olasılıkla uygulamalarınızı getiren değil hazırlıyorsanız stres - bozuk birlikte aynı anda sorun giderme gerektirir.

### <a name="fail-over-to-a-geo-replicated-secondary-database"></a>Coğrafi olarak çoğaltılmış bir ikincil veritabanı yük devri
Aktif coğrafi çoğaltma ve otomatik yük devretme grupları (Önizleme-), kurtarma mekanizması olarak kullanıyorsanız, bir otomatik yük devretme İlkesi yapılandırmak veya kullanmak [el ile yük devretme](sql-database-disaster-recovery.md#fail-over-to-geo-replicated-secondary-server-in-the-failover-group). Yük devretme başlatılan sonra yeni birincil haline gelir ve yeni işlemleri kaydetmek ve henüz çoğaltılan veriler için en düşük düzeyde veri kaybı ile - sorgularını yanıtlamak için hazır ikincil neden olur. Yük devretme işlemini tasarlama hakkında daha fazla bilgi için bkz: [bulut olağanüstü durum kurtarma için bir uygulama tasarlamanızı](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [!NOTE]
> Veri Merkezi tekrar çevrimiçi olduğunda eski ana otomatik olarak yeniden yeni birincil ve ikincil veritabanlarıyla haline gelir. Özgün bölgesine birincil geri taşıyabilir gerekiyorsa, planlanmış bir yük devretme el ile başlatabilir (yeniden çalışma). 
> 

### <a name="perform-a-geo-restore"></a>Bir coğrafi geri yükleme gerçekleştirin
Coğrafi olarak yedekli depolama çoğaltma ile otomatik yedeklemeler kullanıyorsanız, kurtarma mekanizması olarak [coğrafi geri yükleme kullanarak bir veritabanı kurtarma](sql-database-disaster-recovery.md#recover-using-geo-restore). Kurtarma işlemi genelde 12 saat içinde gerçekleşir. Son saatlik değişiklik yedeğinin alındığı ve çoğaltıldığı zaman göre en fazla bir saatlik veri kaybı yaşanabilir. Kurtarma işlemi tamamlanana kadar veritabanı işlem kaydedemez ve sorgulara yanıt veremez. Bu bir veritabanı son geri yüklerken coğrafi ikincil zamanında herhangi bir noktaya kadar geri kullanılabilir bir noktada, şu anda desteklenmiyor.

> [!NOTE]
> Kurtarılan veritabanı için uygulamanızı geçebilir önce veri merkezi tekrar çevrimiçi olursa kurtarma iptal edebilirsiniz.  
>
>

### <a name="perform-post-failover--recovery-tasks"></a>Yük devretme/kurtarma sonrası görevleri gerçekleştirme
Bu iki kurtarma sisteminden herhangi biriyle gerçekleştirilen kurtarma işleminden sonra kullanıcılarınızın ve uygulamalarınızın çalışmaya devam etmesi için aşağıdaki ek görevleri gerçekleştirmeniz gerekir:

* İstemcilerin ve istemci uygulamalarının yeni sunucuya ve geri yüklenen veritabanına nasıl yönlendirileceğini belirleme
* Kullanıcılarınızın bağlantı kurabilmesi için gerekli sunucu düzeyi güvenlik duvarı kurallarının mevcut olduğunu doğrulama ([veritabanı düzeyi güvenlik duvarı](sql-database-firewall-configure.md#creating-and-managing-firewall-rules) da kullanabilirsiniz)
* Uygun giriş bilgilerinin ve ana veritabanı düzeyi izinlerin mevcut olduğunu doğrulama ([kapsanan kullanıcıları](https://msdn.microsoft.com/library/ff929188.aspx) da kullanabilirsiniz)
* Denetimi uygun şekilde yapılandırma
* Uyarıları uygun şekilde yapılandırma

## <a name="upgrade-an-application-with-minimal-downtime"></a>Bir uygulamayı en az kesinti süresiyle yükseltme
Bazen bir uygulama gibi bir uygulama yükseltme planlı bakım nedeniyle çevrimdışına alınmalıdır. [Uygulama yükseltmelerini yönetme](sql-database-manage-application-rolling-upgrade.md) yükseltmelerini yükseltmeler sırasında kapalı kalma süresini en aza indirmek ve bir sorun yaşanırsa kurtarma yolu sağlamak için bulut uygulamanızın etkinleştirmek için etkin coğrafi çoğaltma kullanmayı açıklar. 

## <a name="next-steps"></a>Sonraki adımlar
Bağımsız veritabanları ve elastik havuzlarla ilgili dikkate alınacak uygulama tasarımı noktaları hakkında ayrıntılı bilgi için bkz. [Bulutta olağanüstü durum kurtarma için uygulama tasarlama](sql-database-designing-cloud-solutions-for-disaster-recovery.md) ve [Elastik havuz olağanüstü durum kurtarma stratejileri](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
