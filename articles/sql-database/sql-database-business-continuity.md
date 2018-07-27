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
ms.date: 07/25/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 46ab4a177cc7d86e5d967ff8e219dae96f82a0dc
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39263155"
---
# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Azure SQL Veritabanı'nda iş sürekliliğine genel bakış

Bu genel bakış, Azure SQL Veritabanı'nın iş sürekliliği ve olağanüstü durum kurtarma özelliklerini açıklamaktadır. Seçenekler, öneriler ve öğreticiler, veri kaybına neden veya veritabanı ve uygulama kullanılamaz hale gelmesine neden olaylardan kurtarmak için hakkında bilgi edinin. Bir kullanıcı veya uygulama hatası veri bütünlüğünü etkileyen, bir Azure bölgesinde kesinti yaşandığında veya uygulamanız zaman bakıma gerek duyacağını ne yapılacağını öğrenin.

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>İş sürekliliği sağlamak için kullanabileceğiniz SQL Veritabanı özellikleri

SQL Veritabanı; otomatik yedekleme ve isteğe bağlı veritabanı çoğaltma gibi birçok iş sürekliliği özelliği sunar. Her özellik, tahmini kurtarma süresi (ERT) ve son işlemler için olası veri kaybı açısından farklı özelliklere sahiptir. Bu seçenekleri kavradıktan sonra aralarından birini seçebilir ve çoğu senaryoda farklı durumlar için birden fazlasını birlikte kullanabilirsiniz. İş sürekliliği planınızı geliştirirken, uygulamanın kesintiden sonra tamamen kurtarılmasına kadar kabul edilebilen maksimum süre olan kurtarma süresi hedefini (RTO) belirlemeniz gerekir. Ayrıca en son veri miktarını anlamanıza gerek güncelleştirmelerinin (zaman aralığı) uygulama edilebilecek kesintiden sonra kurtarılırken - Bu, kurtarma noktası hedefi (RPO).

Aşağıdaki tabloda, her hizmet katmanı için üç yaygın senaryo için ERT ve RPO değerleri karşılaştırılmaktadır.

| Özellik | Temel | Standart | Premium  | Genel Amaçlı | İş Açısından Kritik
| --- | --- | --- | --- |--- |--- |
| Yedekten belirli bir noktaya geri yükleme |7 gün içinde herhangi bir geri yükleme noktasına |35 gün içinde herhangi bir geri yükleme noktasına |35 gün içinde herhangi bir geri yükleme noktasına |Yapılandırılan süre (en fazla 35 gün) içinde herhangi bir geri yükleme noktası|Yapılandırılan süre (en fazla 35 gün) içinde herhangi bir geri yükleme noktası|
| Coğrafi çoğaltmalı yedeklerden coğrafi geri yükleme |ERT < 12 sa., RPO < 1 sa |ERT < 12 sa., RPO < 1 sa |ERT < 12 sa., RPO < 1 sa |ERT < 12 sa., RPO < 1 sa|ERT < 12 sa., RPO < 1 sa|
| Uzun süreli saklama SQL geri yükleme |ERT < 12 sa., RPO < 1 hft. |ERT < 12 sa., RPO < 1 hft. |ERT < 12 sa., RPO < 1 hft. |ERT < 12 sa., RPO < 1 hft.|ERT < 12 sa., RPO < 1 hft.|
| Etkin coğrafi çoğaltma |ERT < 30 sn., RPO < 5 sn. |ERT < 30 sn., RPO < 5 sn. |ERT < 30 sn., RPO < 5 sn. |ERT < 30 sn., RPO < 5 sn.|ERT < 30 sn., RPO < 5 sn.|

### <a name="use-point-in-time-restore-to-recover-a-database"></a>Bir veritabanını kurtarmak için zaman içinde nokta geri yükleme kullanın

SQL veritabanı otomatik olarak tam veritabanı yedeklemeleri birlikte haftalık gerçekleştirir, saatlik veritabanı değişiklik yedeklerinin ve işlem yedeklemeleri her beş - on dakika işletmenizi veri kaybına karşı korumak için günlüğü. Kullanıyorsanız [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md), veritabanları, standart ve Premium hizmet katmanlarında ve 7 gün boyunca temel hizmet katmanındaki veritabanları için 35 gün boyunca bu yedeklemeleri RA-GRS depolama alanında depolanır. Hizmet katmanızın saklama süresi işletmenizin ihtiyaçlarını karşılamıyorsa, [hizmet katmanını değiştirerek](sql-database-single-database-scale.md) saklama süresini uzatabilirsiniz. Kullanıyorsanız [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md), genel amaçlı ve iş kritik katmanlarda yedeklemelerin saklanacağı 35 gün öncesine yapılandırılabilir. Tam yedekler ve değişiklik yedekleri, veri merkezi kesintilerine karşı [eşleştirilmiş veri merkezine](../best-practices-availability-paired-regions.md) de çoğaltılır. Daha fazla bilgi için [otomatik veritabanı yedeklemelerini](sql-database-automated-backups.md).

Maksimum desteklenen-belirli bir noktaya geri yükleme (PITR) saklama süresi uygulamanız için yeterli değil, veritabanları için bir uzun süreli saklama (LTR) ilkesi yapılandırarak genişletebilirsiniz. Daha fazla bilgi için [otomatik yedeklemeler](sql-database-automated-backups.md) ve [uzun süreli yedek saklama](sql-database-long-term-retention.md).

Bu otomatik veritabanı yedekleme özelliklerini kullanarak hem kendi veri merkezinizdeki hem de başka veri merkezlerindeki veritabanlarını çeşitli kesintilerden kurtarabilirsiniz. Otomatik veritabanı yedekleriyle tahmini kurtarma süresi, aynı anda aynı bölgede kurtarılan veri tabanı sayısı, veritabanı boyutu, işlem günlüğü boyutu ve ağ bant genişliği gibi birden fazla etmene göre değişiklik gösterir. Kurtarma zamanı, genellikle daha az 12 saati geçmez. Çok büyük veya etkin bir veritabanına kurtarılması daha uzun sürebilir. Kurtarma süresi hakkında daha fazla ayrıntı için bkz. [veritabanı kurtarma zamanı](sql-database-recovery-using-backups.md#recovery-time). Başka bir veri bölgesine kurtarma gerçekleştirirken, potansiyel veri kaybı saatlik veritabanı değişiklik yedeklerinin coğrafi olarak yedekli olması sayesinde 1 saatle sınırlıdır.

> [!IMPORTANT]
> Otomatik yedekleri kullanarak kurtarma gerçekleştirmek için SQL Server Katılımcısı rolü üyesi veya abonelik sahibi olmanız gerekir. [RBAC: Yerleşik roller](../role-based-access-control/built-in-roles.md). Verileri Azure portalı, PowerShell veya REST API kullanarak kurtarabilirsiniz. Transact-SQL kullanamazsınız.
>

Uygulamanız aşağıdaki koşullara uygunsa iş sürekliliği ve kurtarma hizmeti olarak otomatik yedeklemeyi kullanabilirsiniz:

* Görev açısından kritik değilse.
* Bağlayıcı SLA - 24 saatlik bir kapalı kalma süresi yok veya artık mali yükümlülük sonuçlanmaz.
* Veri değişiklik oranı (bir saat içinde gerçekleştirilen işlem sayısı) düşükse ve bir saatlik değişiklik kaybı kabul edilebilirse.
* Maliyetler önemliyse.

Daha hızlı veri kurtarmaya ihtiyacınız varsa, [etkin coğrafi çoğaltma](sql-database-geo-replication-overview.md) (aşağıda açıklanmıştır). 35 günden daha uzun bir süre veri kurtarma, kullanmak gerekiyorsa [uzun süreli saklama](sql-database-long-term-retention.md). 

### <a name="use-active-geo-replication-and-auto-failover-groups-to-reduce-recovery-time-and-limit-data-loss-associated-with-a-recovery"></a>Kurtarma süresini kısaltmak ve bir kurtarma işlemiyle ilişkilendirilmiş veri kaybını sınırlamak için etkin coğrafi çoğaltma ve otomatik yük devretme grupları kullanma

Kullanabileceğiniz iş kesintisi oluşursa veritabanı yedeklemeleri için veritabanı kurtarma kullanmanın yanı sıra, [etkin coğrafi çoğaltma](sql-database-geo-replication-overview.md) seçtiğiniz bölgelerde en fazla dört okunabilir ikincil veritabanı bir veritabanı yapılandırmak için. Bu ikincil veritabanları, birincil veritabanıyla zaman uyumsuz çoğaltma sistemi kullanılarak eşitlenir. Bu özellik, bir veri merkezi kesintisi yaşanırsa veya uygulama yükseltme sırasında iş kesintiye karşı korumak için kullanılır. Etkin coğrafi çoğaltma, coğrafi olarak dağınık kullanıcılara salt okunur sorgular için daha iyi sorgu performansını sağlamak için de kullanılabilir.

Etkinleştirmek için otomatik ve şeffaf yük devretme, düzenleme coğrafi olarak çoğaltılmış veritabanlarınızı kullanarak gruplar halinde [otomatik yük devretme grubu](sql-database-geo-replication-overview.md) SQL veritabanı özelliğidir.

Birincil veritabanının beklenmedik şekilde çevrimdışı olması veya çevrimdışı bakım etkinlikleri için yapmanız gereken, (bir yük devretme olarak da bilinir) duruma ve uygulamaları yükseltilen birincil veritabanına bağlanacak şekilde yapılandırmak için ikincil bir kolayca yükseltebilirsiniz. Uygulamanızın yük devretme grubu dinleyicisi kullanarak veritabanlarını bağlanırsa, yük devretme işleminden sonra SQL bağlantı dizesi yapılandırmasını değiştirmek gerekmez. Planlı yük devretme sayesinde veri kaybı yaşanmaz. Plansız yük devretme durumunda ise zaman uyumsuz çoğaltmanın özelliğinden dolayı en son işlemler için az miktarda veri kaybı yaşanabilir. Otomatik Yük devretme grupları kullanarak, olası veri kaybını en aza indirmek için yük devretme İlkesi özelleştirebilirsiniz. Yük devretme sonrasında belirli bir plana göre veya veri merkezi çevrimiçi duruma geldiğinde veritabanını yeniden çalıştırabilirsiniz. Tüm durumlarda kullanıcılar kısa süreli bir kesinti yaşar ve yeniden bağlantı kurmaları gerekir.

> [!IMPORTANT]
> Etkin coğrafi çoğaltma ve otomatik yük devretme grupları kullanmak için abonelik sahibi olmanız veya SQL Server'da yönetici izinlerine sahip olmalıdır. Yapılandırma ve Azure'ı kullanmaya üzerinden başarısız portal, PowerShell veya Azure aboneliği izinleri veya SQL Server izinleriyle Transact-SQL kullanarak bir REST API.
> 

Uygulamanız aşağıdaki ölçütleri karşılıyorsa etkin coğrafi çoğaltma ve otomatik yük devretme grupları kullanın:

* Görev açısından kritikse.
* 24 saat veya üzeri kesinti süresine izin vermeyen hizmet düzeyi sözleşmesine (SLA) sahipse.
* Kesinti süresi mali yükümlülük neden olabilir.
* Veri değişiklik oranı yüksekse ve bir saatlik değişiklik kaybı kabul edilebilir değilse.
* Etkin coğrafi çoğaltma ek maliyeti, olası mali yükümlülükten veya ilgili iş kaybından daha düşükse.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-protecting-important-DBs-from-regional-disasters-is-easy/player]
>

## <a name="recover-a-database-after-a-user-or-application-error"></a>Kullanıcı veya uygulama hatasından sonra bir veritabanını kurtarma

Hiç kimse mükemmel değildir! Bir kullanıcı yanlışlıkla veri silebilir, istemeden önemli bir tabloyu ve hatta bir veritabanının tamamını bırakabilir. Ya da bir uygulama, hata nedeniyle yanlışlıkla gereksiz verileri gerekli verilerin üzerine yazabilir.

Bu senaryoda kullanabileceğiniz kurtarma seçenekleri aşağıda verilmiştir.

### <a name="perform-a-point-in-time-restore"></a>Belirli bir noktaya geri yükleme gerçekleştirme
Noktanın veritabanı saklama süresi içinde olması şartıyla otomatik yedekleri kullanarak veritabanınızın bir kopyasını belirli bir noktaya geri yükleyebilirsiniz. Veritabanı geri yüklendikten sonra özgün veritabanını geri yüklenen veritabanıyla değiştirebilir veya geri yüklenen veritabanından gerekli verileri özgün veritabanına kopyalayabilirsiniz. Etkin coğrafi çoğaltma veritabanı kullanıyorsa, gerekli verileri özgün veritabanına geri yüklenen kopyadan kopyalamak öneririz. Özgün veritabanını geri yüklenen veritabanıyla değiştirmeniz halinde yeniden yapılandırın ve etkin coğrafi (Bu oldukça büyük bir veritabanı için biraz zaman alabilir) çoğaltmayı yeniden eşitlemek gerekir. Bu son bir veritabanını geri yüklerken, zaman içinde kullanılabilir noktası coğrafi-ikincil, zaman herhangi bir noktasına geri yüklenmesi şu anda desteklenmiyor.

Azure portal veya PowerShell kullanarak bir veritabanını belirli bir noktaya geri yüklemeyle ilgili ayrıntılı adımlar ve daha fazla bilgi için bkz. [belirli bir noktaya geri yükleme](sql-database-recovery-using-backups.md#point-in-time-restore). Transact-SQL kullanarak kurtarma gerçekleştiremezsiniz.

### <a name="restore-a-deleted-database"></a>Silinen veritabanını geri yükleme
Veritabanı silinmiş ancak mantıksal sunucu silinmemişse, silinen veritabanını silindiği noktaya geri yükleyebilirsiniz. Bu işlem bir veritabanı yedeğini silindiği mantıksal SQL sunucusuna geri yükler. Özgün adı kullanarak geri yükleyebilir veya geri yüklenen veritabanı için yeni bir ad belirleyebilirsiniz.

Azure portal veya PowerShell kullanarak silinen bir veritabanını geri yüklemeyle ilgili ayrıntılı adımlar ve daha fazla bilgi için bkz. [silinen veritabanını geri yükleme](sql-database-recovery-using-backups.md#deleted-database-restore). Transact-SQL kullanarak geri yükleme gerçekleştiremezsiniz.

> [!IMPORTANT]
> Mantıksal sunucu silinmişse, silinen bir veritabanını kurtaramazsınız.


### <a name="restore-backups-from-long-term-retention"></a>Uzun süreli saklama yedeklerini geri

Otomatik yedeklemeler için geçerli saklama süresinin dışında veri kaybı oluştu ve veritabanınızı Azure blob depolama kullanarak uzun süreli saklama için yapılandırıldıysa, Azure blob depolama alanındaki tam bir yedekten yeni bir veritabanına geri yükleyebilirsiniz. Bu noktada özgün veritabanını geri yüklenen veritabanıyla değiştirebilir veya geri yüklenen veritabanından gerekli verileri özgün veritabanına kopyalayabilirsiniz. Veritabanınızın büyük bir uygulama yükseltmesinden önce eski bir sürümünü almak gerekiyorsa, Azure blob depolama alanında kayıtlı bir tam yedeği kullanarak bir veritabanı oluşturabileceğiniz denetçiler veya yasal kurumlardan gelen bir istek karşılamak.  Daha fazla bilgi için bkz. [Uzun süreli saklama](sql-database-long-term-retention.md).

## <a name="recover-a-database-to-another-region-from-an-azure-regional-data-center-outage"></a>Azure bölgesel veri merkezi kesintisinde bir veritabanını başka bir bölgeye kurtarma
<!-- Explain this scenario -->

Çok sık olmasa da Azure veri merkezlerinde kesintiler yaşanabilir. Kesinti yaşandığında yalnızca birkaç dakika sürebilecek veya saatler alacak bir hizmet kesintisi söz konusu olabilir.

* Seçeneklerden biri, veri merkezi kesintisi sona erdiğinde veritabanınızın çevrimdışı olmasını beklemektir. Bu, veritabanının çevrimdışı olmasının kabul edilebildiği uygulamalar için geçerlidir. Örnek olarak üzerinde sürekli çalışma yapmadığınız bir geliştirme projesi veya ücretsiz deneme sürümü verilebilir. Bir veri merkezinde bir kesinti varsa, böylece veritabanınızı bir süredir ihtiyacınız yoksa bu seçenek yalnızca çalışır, kesinti ne kadar sürebilecek, bilmezsiniz.
* Etkin coğrafi çoğaltma veya kurtarma kullanıyorsanız, başka bir seçenek ya da başarısız olmasına üzerinden başka bir veri bölgesine coğrafi olarak yedekli veritabanı yedeklerini (coğrafi geri yükleme) kullanarak bir veritabanı hizmetidir. Veritabanı kurtarma yedeklemelerinden saat sürer ancak yük devretme yalnızca birkaç saniye sürer.

Eylem gerçekleştirmeniz ne zaman bunu, kurtarılır ne kadar sürdüğü ve ücretler ne kadar veri kaybını nasıl, uygulamanızda bu iş sürekliliği özelliklerini kullanma şeklinize bağlıdır. Aslında, veritabanı yedeklemeleri ve uygulama gereksinimlerinize bağlı olarak active geografickou replikaci kullanmayı tercih edebilirsiniz. Bu iş sürekliliği özelliklerini kullanan bağımsız veritabanları ve elastik havuzlarla ilgili dikkate alınacak uygulama tasarımı noktaları hakkında ayrıntılı bilgi için bkz. [Bulutta olağanüstü durum kurtarma için uygulama tasarlama](sql-database-designing-cloud-solutions-for-disaster-recovery.md) ve [Elastik havuz olağanüstü durum kurtarma stratejileri](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

Aşağıdaki bölümlerde veritabanı yedeklerini veya etkin coğrafi çoğaltmayı kullanarak kurtarma adımlarını genel bir bakış sağlar. Planlama gereksinimleri, Kurtarma sonrası adımlar ve kesinti simülasyonu yapma hakkında bilgi olağanüstü durum kurtarma tatbikatı gerçekleştirme dahil olmak üzere ayrıntılı adımlar için bkz: [bir SQL veritabanını kesintiden kurtarma](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Kesinti için hazırlanma
Kullandığınız iş sürekliliği özelliğinden bağımsız olarak aşağıdaki adımları uygulamanız gerekir:

* Sunucu düzeyi güvenlik duvarı kuralları, giriş bilgileri ve ana veritabanı düzeyi izinler dahil olmak üzere hedef sunucuyu belirlemek ve hazırlamak
* İstemcilerin ve istemci uygulamalarının yeni sunucuya nasıl yönlendirileceğini belirlemek
* Denetim ayarları ve uyarılar gibi diğer bağımlılık belgelerini oluşturmak

Ayrıca, düzgün bir şekilde uygulamalarınıza ek süre bir yük devretme veya veritabanı kurtarma işlemi gerçekleştirildikten sonra çevrimiçi ve büyük olasılıkla hazırlık aşamalarını değil, stres - hatalı bir birleşimini teker teker sorun giderme gerektirir.

### <a name="fail-over-to-a-geo-replicated-secondary-database"></a>Coğrafi çoğaltmalı ikincil veritabanına yük devretme
Kurtarma sisteminizde etkin coğrafi çoğaltma ve otomatik yük devretme grupları kullanıyorsanız, otomatik yük devretme ilkesini yapılandırma veya kullanma [el ile yük devretme](sql-database-disaster-recovery.md#fail-over-to-geo-replicated-secondary-server-in-the-failover-group). Yük devretme başlatıldıktan sonra yeni birincil veritabanı haline ve yeni işlemleri kaydetmeye ve sorguları - henüz çoğaltılan veriler için en düşük düzeyde veri kaybıyla yanıtlamak için hazır ikincil neden olur. Yük devretme işlemini tasarlama hakkında daha fazla bilgi için bkz: [bulutta olağanüstü durum kurtarma için uygulama tasarlama](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [!NOTE]
> Veri Merkezi tekrar çevrimiçi olduğunda eski seçimlerine otomatik olarak yeni birincil siteye yeniden bağlayın ve ikincil veritabanları olur. Özgün bölgesiyle birincil arka dışında yeniden konumlandırmakta gerekiyorsa, planlanmış bir yük devretmeyi el ile başlatabilir (yeniden çalışma). 
> 

### <a name="perform-a-geo-restore"></a>Bir coğrafi geri yükleme gerçekleştirme
Kurtarma sisteminizde coğrafi olarak yedekli depolama çoğaltması ile otomatik yedekleri kullanıyorsanız [coğrafi geri yükleme kullanarak bir veritabanı kurtarma işlemini başlatmada](sql-database-disaster-recovery.md#recover-using-geo-restore). Kurtarma işlemi genelde 12 saat içinde gerçekleşir. Son saatlik değişiklik yedeğinin alındığı ve çoğaltıldığı zaman göre en fazla bir saatlik veri kaybı yaşanabilir. Kurtarma işlemi tamamlanana kadar veritabanı işlem kaydedemez ve sorgulara yanıt veremez. Bu son bir veritabanını geri yüklerken, zaman içinde kullanılabilir noktası coğrafi-ikincil, zaman herhangi bir noktasına geri yüklenmesi şu anda desteklenmiyor.

> [!NOTE]
> Kurtarılan veritabanı için uygulamanızı geçebilir önce veri merkezi tekrar çevrimiçi olursa kurtarma işlemini iptal edebilirsiniz.  
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
Bazen bir uygulama bir uygulamanın yükseltme gibi planlı bakım nedeniyle çevrimdışına alınmalıdır. [Uygulama yükseltmelerini yönetme](sql-database-manage-application-rolling-upgrade.md) etkin coğrafi çoğaltma, yükseltme sırasında kapalı kalma süresini en aza indirmek ve bir sorun yaşanırsa kurtarma yolu sağlama amacıyla bulut uygulamanızın sıralı yükseltmelerini etkinleştirmek için nasıl kullanılacağını açıklar. 

## <a name="next-steps"></a>Sonraki adımlar
Bağımsız veritabanları ve elastik havuzlarla ilgili dikkate alınacak uygulama tasarımı noktaları hakkında ayrıntılı bilgi için bkz. [Bulutta olağanüstü durum kurtarma için uygulama tasarlama](sql-database-designing-cloud-solutions-for-disaster-recovery.md) ve [Elastik havuz olağanüstü durum kurtarma stratejileri](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
