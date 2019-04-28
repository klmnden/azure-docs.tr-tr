---
title: Bulutta iş sürekliliği - veritabanı kurtarma - SQL Veritabanı | Microsoft Belgeleri
description: Azure SQL Veritabanı'nın bulutta iş sürekliliği ve veritabanı kurtarmanın yanı sıra görev açısından kritik bulut uygulamalarının kesintisiz çalışması için sunduğu destek özelliklerini öğrenin.
keywords: iş sürekliliği, bulutta iş sürekliliği, veritabanı olağanüstü durum kurtarma, veritabanı kurtarma
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: mathoma, carlrab
manager: digimobile
origin.date: 04/04/2019
ms.date: 04/15/2019
ms.openlocfilehash: dfa5d4cb2d782f1466329300157a64fd17765460
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61412352"
---
# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Azure SQL Veritabanı'nda iş sürekliliğine genel bakış

**İş sürekliliği** mekanizmaları, ilkeleri ve işletmenizi, bilgi işlem altyapısı için özellikle kesintisi karşılaşıldığında çalışmaya devam etkinleştirme yordamlar Azure SQL veritabanı'nda ifade eder. Örneklerinin çoğunda, Azure SQL veritabanı bulut ortamında gerçekleşir ve uygulamalarınızı ve iş süreçlerini çalıştıran tutmak aksatıcı olayları işler. Ancak, SQL veritabanı tarafından gibi işlenemez bazı aksatıcı olayları vardır:

- Kullanıcı, yanlışlıkla silinmiş veya bir tabloda bir satırın güncelleştirilmiş.
- Kötü amaçlı bir saldırganın verileri silmek ya da bir veritabanı başarıyla oluşturuldu.
- Deprem güç kesintisi ve geçici devre dışı veri merkezi neden oldu.

Uygulamaları çalıştırmayı sürdürün ve verilerinizi kurtarmanıza olanak tanıyan SQL veritabanı iş sürekliliği özelliklerini kullanmak gerekir, böylece bu gibi durumlarda Azure SQL veritabanı tarafından kontrol edilemez.

Bu genel bakış, Azure SQL Veritabanı'nın iş sürekliliği ve olağanüstü durum kurtarma özelliklerini açıklamaktadır. Seçenekler, öneriler ve öğreticiler, veri kaybına neden veya veritabanı ve uygulama kullanılamaz hale gelmesine neden olaylardan kurtarmak için hakkında bilgi edinin. Bir kullanıcı veya uygulama hatası veri bütünlüğünü etkileyen, bir Azure bölgesinde kesinti yaşandığında veya uygulamanız zaman bakıma gerek duyacağını ne yapılacağını öğrenin.

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>İş sürekliliği sağlamak için kullanabileceğiniz SQL Veritabanı özellikleri

Veritabanı açısından bakıldığında, dört ana olası kesintileri senaryo vardır:

- Bir disk sürücüsü arızası gibi veritabanı düğümü etkileyen yerel donanım veya yazılım hatası.
- Verilerin bozulması veya nedeni genellikle bir uygulama hatası ya da insan hatası tarafından silinmesi. Bu tür hataları doğası gereği uygulamaya özgü olur ve bir kural veya algılanan altyapısı tarafından otomatik olarak azaltılabilir olamaz.
- Veri Merkezi kesintisi, büyük olasılıkla bir doğal felaket tarafından neden. Bu senaryo, bazı alternatif bir veri merkezine uygulama yük devretme ile coğrafi yedeklilik düzeyini gerektirir.
- Bir uygulama veya veritabanı bakım veritabanı önceki bir duruma hızla geri alma gerektirebilir veya hatalar, yükseltme veya bakım yükseltmeleri sırasında beklenmeyen sorunlar planlanmış.

SQL veritabanı, otomatik yedeklemeler ve bu senaryolar azaltabilirsiniz isteğe bağlı veritabanı çoğaltma dahil olmak üzere çeşitli iş sürekliliği özellikleri sunar. İlk olarak anlamak ihtiyacınız nasıl SQL veritabanı [yüksek kullanılabilirlik mimarisi](sql-database-high-availability.md) % 99,99 kullanılabilirlik ve iş sürecinizin etkileyebilecek bazı aksatıcı olayları karşı dayanıklılık sağlar.
Ardından, SQL veritabanı yüksek kullanılabilirlik mimarisi ile gibi işlenemez olaylardan kurtarmak için kullanabileceğiniz ek mekanizmaları hakkında bilgi edinebilirsiniz:

- [Zamana bağlı tablolarda](sql-database-temporal-tables.md) satır sürümleri herhangi bir noktadan geri yükleme sağlar.
- [Yerleşik otomatik yedeklerinde](sql-database-automated-backups.md) ve [geri yükleme noktası](sql-database-recovery-using-backups.md#point-in-time-restore) son 35 gün içindeki belirli bir noktada tam veritabanını geri yüklemenize olanak sağlar.
- Yapabilecekleriniz [silinen bir veritabanını geri yükleme](sql-database-recovery-using-backups.md#deleted-database-restore) başlangıçtan silinmiş varsa noktasına **SQL veritabanı sunucusu silinmediğini**.
- [Uzun süreli yedek saklama](sql-database-long-term-retention.md) yedeklemeleri 10 yıla kadar açık kalmasını sağlar.
- [Etkin coğrafi çoğaltma](sql-database-active-geo-replication.md) okunabilir çoğaltma ve el ile bir veri merkezi kesintisi veya uygulama yükseltmesi durumunda herhangi bir çoğaltmaya yük devretme oluşturmanıza olanak sağlar.
- [Otomatik Yük devretme grubu](sql-database-auto-failover-group.md#auto-failover-group-terminology-and-capabilities) uygulamayı otomatik olarak bir veri merkezi kesintisi durumunda kurtarma sağlar.

Her özellik, tahmini kurtarma süresi (ERT) ve son işlemler için olası veri kaybı açısından farklı özelliklere sahiptir. Bu seçenekleri kavradıktan sonra aralarından birini seçebilir ve çoğu senaryoda farklı durumlar için birden fazlasını birlikte kullanabilirsiniz. İş sürekliliği planınızı geliştirirken, uygulamanın kesintiden sonra tamamen kurtarır önce kabul edilebilen maksimum süre anlamanız gerekir. Uygulamanın tamamen kurtarmak için gereken süre, Kurtarma süresi hedefi (RTO) bilinir. Ayrıca uygulama edilebilecek son veri güncelleştirmelerinin (zaman aralığı) maksimum süreyi anlamanız gereken kesintiden sonra kurtarılırken. Zaman dilimi kaybetmeyi göze güncelleştirmeleri, kurtarma noktası hedefi (RPO) bilinir.

Aşağıdaki tabloda, her hizmet katmanı için en yaygın senaryolar için ERT ve RPO değerleri karşılaştırılmaktadır.

| Özellik | Temel | Standart | Premium | Genel Amaçlı | İş Açısından Kritik
| --- | --- | --- | --- |--- |--- |
| Yedekten belirli bir noktaya geri yükleme |Herhangi bir yedi gün içinde nokta geri yükleme |35 gün içinde herhangi bir geri yükleme noktasına |35 gün içinde herhangi bir geri yükleme noktasına |Yapılandırılan süre (en fazla 35 gün) içinde herhangi bir geri yükleme noktası|Yapılandırılan süre (en fazla 35 gün) içinde herhangi bir geri yükleme noktası|
| Coğrafi çoğaltmalı yedeklerden coğrafi geri yükleme |ERT < 12 sa.<br> RPO < 1 saat |ERT < 12 sa.<br>RPO < 1 saat |ERT < 12 sa.<br>RPO < 1 saat |ERT < 12 sa.<br>RPO < 1 saat|ERT < 12 sa.<br>RPO < 1 saat|
| Otomatik yük devretme grupları |RTO 1 saat =<br>RPO < 5 sn |RTO 1 saat =<br>RPO < 5 s |RTO 1 saat =<br>RPO < 5 s |RTO 1 saat =<br>RPO < 5 s|RTO 1 saat =<br>RPO < 5 s|
| Elle yapılan veritabanı yük devretme |ERT = 30 s<br>RPO < 5 sn |ERT = 30 s<br>RPO < 5 s |ERT = 30 s<br>RPO < 5 s |ERT = 30 s<br>RPO < 5 s|ERT = 30 s<br>RPO < 5 s|

> [!NOTE]
> *Elle yapılan veritabanı yük devretme* , coğrafi olarak çoğaltılmış ikincil kullanarak tek bir veritabanının yük devretme başvurduğu [planlanmamış modu](sql-database-active-geo-replication.md#active-geo-replication-terminology-and-capabilities).

## <a name="recover-a-database-to-the-existing-server"></a>Sunucunun var olan bir veritabanını kurtarma

SQL veritabanı otomatik olarak her 12 saatte bir, genellikle yapılan Türevsel veritabanı yedekleri tam veritabanı yedeklemeleri birlikte haftalık, gerçekleştirir ve işlem işletmenizi veri kaybına karşı korumak için 5-10 dakikada bir yedekleme günlüğü. Yedeklemeler, yedeklemeler için 7 gün depolandığı temel DTU hizmet katmanları dışındaki tüm hizmet katmanları için 35 gün boyunca RA-GRS depolama alanında depolanır. Daha fazla bilgi için [otomatik veritabanı yedeklemelerini](sql-database-automated-backups.md). Var olan bir veritabanı formunu otomatik yedekleri için daha önceki bir noktaya aynı SQL veritabanı sunucusunda Azure portalı, PowerShell veya REST API'yi kullanarak yeni bir veritabanı olarak zaman içinde geri yükleyebilirsiniz. Daha fazla bilgi için [-belirli bir noktaya geri yükleme](sql-database-recovery-using-backups.md#point-in-time-restore).

Maksimum desteklenen-belirli bir noktaya geri yükleme (PITR) saklama süresi uygulamanız için yeterli değil, veritabanları için bir uzun süreli saklama (LTR) ilkesi yapılandırarak genişletebilirsiniz. Daha fazla bilgi için [uzun süreli yedek saklama](sql-database-long-term-retention.md).

Bu otomatik veritabanı yedekleme özelliklerini kullanarak hem kendi veri merkezinizdeki hem de başka veri merkezlerindeki veritabanlarını çeşitli kesintilerden kurtarabilirsiniz. Kurtarma zamanı, genellikle daha az 12 saati geçmez. Çok büyük veya etkin bir veritabanına kurtarılması daha uzun sürebilir. Otomatik veritabanı yedekleriyle tahmini kurtarma süresi, aynı anda aynı bölgede kurtarılan veri tabanı sayısı, veritabanı boyutu, işlem günlüğü boyutu ve ağ bant genişliği gibi birden fazla etmene göre değişiklik gösterir. Kurtarma süresi hakkında daha fazla bilgi için bkz. [veritabanı kurtarma zamanı](sql-database-recovery-using-backups.md#recovery-time). Başka bir veri bölgesine kurtarma gerçekleştirirken, potansiyel veri kaybı ile coğrafi olarak yedekli yedeklemeleri kullanımını 1 saat sınırlıdır.

Otomatik yedekleri kullanın ve [-belirli bir noktaya geri yükleme](sql-database-recovery-using-backups.md#point-in-time-restore) iş sürekliliği ve kurtarma mekanizmanızı olarak, uygulamanızı:

- Görev açısından kritik değilse.
- Bağlayıcı SLA - 24 saatlik bir kapalı kalma süresi yok veya artık mali yükümlülük sonuçlanmaz.
- Veri değişiklik oranı (bir saat içinde gerçekleştirilen işlem sayısı) düşükse ve bir saatlik değişiklik kaybı kabul edilebilirse.
- Maliyetler önemliyse.

Daha hızlı veri kurtarmaya ihtiyacınız varsa, [etkin coğrafi çoğaltma](sql-database-active-geo-replication.md) veya [otomatik yük devretme grupları](sql-database-auto-failover-group.md). 35 günden daha uzun bir süre veri kurtarma, kullanmak gerekiyorsa [uzun süreli saklama](sql-database-long-term-retention.md).

## <a name="recover-a-database-to-another-region"></a>Bir veritabanını başka bir bölgeye kurtarma

Çok sık olmasa da Azure veri merkezlerinde kesintiler yaşanabilir. Kesinti yaşandığında yalnızca birkaç dakika sürebilecek veya saatler alacak bir hizmet kesintisi söz konusu olabilir.

- Seçeneklerden biri, veri merkezi kesintisi sona erdiğinde veritabanınızın çevrimdışı olmasını beklemektir. Bu, veritabanının çevrimdışı olmasının kabul edilebildiği uygulamalar için geçerlidir. Örnek olarak üzerinde sürekli çalışma yapmadığınız bir geliştirme projesi veya ücretsiz deneme sürümü verilebilir. Bir veri merkezinde bir kesinti varsa, böylece veritabanınızı bir süredir ihtiyacınız yoksa bu seçenek yalnızca çalışır, kesinti ne kadar sürebilecek, bilmezsiniz.
- Bir veritabanını kullanarak herhangi bir Azure bölgesi içinde herhangi bir sunucuda geri yüklemek için başka bir seçenektir [coğrafi olarak yedekli veritabanı yedeklemeleri](sql-database-recovery-using-backups.md#geo-restore) (coğrafi geri yükleme). Coğrafi geri yükleme, coğrafi olarak yedekli bir yedeklemesini, kaynağı olarak kullanır ve veritabanı veya veri merkezinde bir kesinti nedeniyle erişilemez durumda olsa bile bir veritabanını kurtarmak için kullanılabilir.
- Son olarak, coğrafi-ikincil kullanarak yapılandırdıysanız, kesinti hızla kurtarabilirsiniz [etkin coğrafi çoğaltma](sql-database-active-geo-replication.md) veya [otomatik yük devretme grubu](sql-database-auto-failover-group.md) veritabanını veya veritabanlarını için. Bu teknolojilerin dilediğiniz bağlı olarak, el ile veya otomatik yük devretme kullanabilirsiniz. Yük devretme kendisi yalnızca birkaç saniye sürer ancak hizmet etkinleştirmek için en az 1 saat sürer. Bu, yük devretme kesinti ölçek tarafından karardır emin olmak gereklidir. Ayrıca, yük devretme zaman uyumsuz çoğaltma niteliği nedeniyle küçük veri kaybına neden olabilir. Ayrıntılar için bu makalenin otomatik yük devretme RTO ve RPO tabloya bakın.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-protecting-important-DBs-from-regional-disasters-is-easy/player]
>
> [!IMPORTANT]
> Etkin coğrafi çoğaltma ve otomatik yük devretme grupları kullanmak için abonelik sahibi olmanız veya SQL Server'da yönetici izinlerine sahip olmalıdır. Yapılandırma ve Azure'ı kullanmaya üzerinden başarısız portal, PowerShell veya Azure aboneliği izinleri veya SQL Server izinleriyle Transact-SQL kullanarak bir REST API.

Uygulamanız aşağıdaki ölçütleri karşılıyorsa, otomatik yük devretme grupları kullanın:

- Görev açısından kritikse.
- 12 saat veya daha fazla kapalı kalma süresi için izin vermeyen bir hizmet düzeyi sözleşmesi (SLA) sahiptir.
- Kesinti süresi mali yükümlülük neden olabilir.
- Veri değişiklik oranı yüksek olan ve 1 saatlik veri kaybı kabul edilebilir değil.
- Etkin coğrafi çoğaltma ek maliyeti, olası mali yükümlülükten veya ilgili iş kaybından daha düşükse.

Eylem gerçekleştirmeniz ne zaman bunu, kurtarılır ne kadar sürdüğü ve ücretler ne kadar veri kaybını nasıl, uygulamanızda bu iş sürekliliği özelliklerini kullanma şeklinize bağlıdır. Aslında, veritabanı yedeklemeleri ve uygulama gereksinimlerinize bağlı olarak active geografickou replikaci kullanmayı tercih edebilirsiniz. Uygulama tasarımı noktaları hakkında bağımsız veritabanları ve elastik havuzlar için bu iş sürekliliği özelliklerini kullanan bir tartışma için bkz [bulutta olağanüstü durum kurtarma için uygulama tasarlama](sql-database-designing-cloud-solutions-for-disaster-recovery.md) ve [esnek Havuz olağanüstü durum kurtarma stratejileri](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).


Aşağıdaki bölümlerde veritabanı yedeklerini veya etkin coğrafi çoğaltmayı kullanarak kurtarma adımlarını genel bir bakış sağlar. Planlama gereksinimleri, Kurtarma sonrası adımlar ve kesinti simülasyonu yapma hakkında bilgi olağanüstü durum kurtarma tatbikatı gerçekleştirme dahil olmak üzere ayrıntılı adımlar için bkz: [bir SQL veritabanını kesintiden kurtarma](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Kesinti için hazırlanma

Kullandığınız iş sürekliliği özelliğinden bağımsız olarak aşağıdaki adımları uygulamanız gerekir:

- Tanımlamak ve sunucu düzeyinde IP güvenlik duvarı kuralları, oturum açma bilgileri ve ana veritabanı düzeyi izinler dahil olmak üzere hedef sunucuyu hazırlayın.
- İstemcilerin ve istemci uygulamalarının yeni sunucuya nasıl yönlendirileceğini belirlemek
- Denetim ayarları ve uyarılar gibi diğer bağımlılık belgelerini oluşturmak

Ayrıca, düzgün bir şekilde uygulamalarınıza ek süre bir yük devretme veya veritabanı kurtarma işlemi gerçekleştirildikten sonra çevrimiçi ve büyük olasılıkla hazırlık aşamalarını değil, stres - hatalı bir birleşimini teker teker sorun giderme gerektirir.

### <a name="fail-over-to-a-geo-replicated-secondary-database"></a>Coğrafi çoğaltmalı ikincil veritabanına yük devretme

Kurtarma sisteminizde etkin coğrafi çoğaltma veya otomatik yük devretme grupları kullanıyorsanız, otomatik yük devretme ilkesini yapılandırma veya kullanma [el ile planlanmamış yük devretme](sql-database-active-geo-replication-portal.md#initiate-a-failover). Yük devretme başlatıldıktan sonra yeni birincil veritabanı haline ve yeni işlemleri kaydetmeye ve sorguları - henüz çoğaltılan veriler için en düşük düzeyde veri kaybıyla yanıtlamak için hazır ikincil neden olur. Yük devretme işlemini tasarlama hakkında daha fazla bilgi için bkz: [bulutta olağanüstü durum kurtarma için uygulama tasarlama](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [!NOTE]
> Veri Merkezi tekrar çevrimiçi olduğunda eski seçimlerine otomatik olarak yeni birincil siteye yeniden bağlayın ve ikincil veritabanları olur. Özgün bölgesiyle birincil arka dışında yeniden konumlandırmakta gerekiyorsa, planlanmış bir yük devretmeyi el ile başlatabilir (yeniden çalışma).

### <a name="perform-a-geo-restore"></a>Bir coğrafi geri yükleme gerçekleştirme

Otomatik yedeklemelerini coğrafi olarak yedekli depolama (varsayılan olarak etkindir) ile kullanıyorsanız, veritabanı kullanarak kurtarabileceğiniz [coğrafi geri yükleme](sql-database-disaster-recovery.md#recover-using-geo-restore). Kurtarma genellikle son günlük yedeği geçen ve çoğaltılan tarafından belirlenen en fazla bir saatlik veri kaybı - 12 saat içinde gerçekleşir. Kurtarma işlemi tamamlanana kadar veritabanı işlem kaydedemez ve sorgulara yanıt veremez. Unutmayın, coğrafi geri yükleme son kullanılabilir noktaya veritabanını yalnızca zaman içinde geri yükler.

> [!NOTE]
> Kurtarılan veritabanı için uygulamanızı geçebilir önce veri merkezi tekrar çevrimiçi olursa kurtarma işlemini iptal edebilirsiniz.

### <a name="perform-post-failover--recovery-tasks"></a>Yük devretme/kurtarma sonrası görevleri gerçekleştirme

Bu iki kurtarma sisteminden herhangi biriyle gerçekleştirilen kurtarma işleminden sonra kullanıcılarınızın ve uygulamalarınızın çalışmaya devam etmesi için aşağıdaki ek görevleri gerçekleştirmeniz gerekir:

- İstemcilerin ve istemci uygulamalarının yeni sunucuya ve geri yüklenen veritabanına nasıl yönlendirileceğini belirleme
- Uygun sunucu düzeyi IP güvenlik duvarı kurallarını kullanın veya bağlanmak kullanıcılar için yerinde olmasını [veritabanı düzeyinde güvenlik duvarları](sql-database-firewall-configure.md#manage-server-level-ip-firewall-rules-using-the-azure-portal) uygun kuralları etkinleştirmek için.
- Uygun giriş bilgilerinin ve ana veritabanı düzeyi izinlerin mevcut olduğunu doğrulama ([kapsanan kullanıcıları](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) da kullanabilirsiniz)
- Denetimi uygun şekilde yapılandırma
- Uyarıları uygun şekilde yapılandırma

> [!NOTE]
> Bir yük devretme grubu kullanıyorsanız ve okuma-yazma lstener kullanan veritabanına bağlanmak, yeniden yönlendirme yük devretmeden sonra uygulamaya otomatik olarak ve şeffaf bir şekilde gerçekleşir.

## <a name="upgrade-an-application-with-minimal-downtime"></a>Bir uygulamayı en az kesinti süresiyle yükseltme

Bazen bir uygulama bir uygulamanın yükseltme gibi planlı bakım nedeniyle çevrimdışına alınmalıdır. [Uygulama yükseltmelerini yönetme](sql-database-manage-application-rolling-upgrade.md) etkin coğrafi çoğaltma, yükseltme sırasında kapalı kalma süresini en aza indirmek ve bir sorun yaşanırsa kurtarma yolu sağlama amacıyla bulut uygulamanızın sıralı yükseltmelerini etkinleştirmek için nasıl kullanılacağını açıklar.

## <a name="next-steps"></a>Sonraki adımlar

Uygulama tasarımı noktaları hakkında tek başına veritabanları ve elastik havuzlar için bkz [bulutta olağanüstü durum kurtarma için uygulama tasarlama](sql-database-designing-cloud-solutions-for-disaster-recovery.md) ve [elastik havuz olağanüstü durum kurtarma stratejileri](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
