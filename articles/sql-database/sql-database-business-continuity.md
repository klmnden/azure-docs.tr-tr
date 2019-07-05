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
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 06/25/2019
ms.openlocfilehash: 2f3723e0a1b14edd6f516f3cc080501bea80d486
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442043"
---
# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Azure SQL Veritabanı'nda iş sürekliliğine genel bakış

**İş sürekliliği** mekanizmaları, ilkeleri ve işletmenizi, bilgi işlem altyapısı için özellikle kesintisi karşılaşıldığında çalışmaya devam etkinleştirme yordamlar Azure SQL veritabanı'nda ifade eder. Örneklerinin çoğunda, Azure SQL veritabanı bulut ortamında gerçekleşir ve uygulamalarınızı ve iş süreçlerini çalıştıran tutmak aksatıcı olayları işler. Ancak, SQL veritabanı tarafından otomatik olarak gibi işlenemez bazı aksatıcı olayları vardır:

- Kullanıcı, yanlışlıkla silinmiş veya bir tabloda bir satırın güncelleştirilmiş.
- Kötü amaçlı bir saldırganın verileri silmek ya da bir veritabanı başarıyla oluşturuldu.
- Deprem güç kesintisi ve geçici devre dışı veri merkezi neden oldu.

Bu genel bakış, Azure SQL Veritabanı'nın iş sürekliliği ve olağanüstü durum kurtarma özelliklerini açıklamaktadır. Seçenekler, öneriler ve öğreticiler, veri kaybına neden veya veritabanı ve uygulama kullanılamaz hale gelmesine neden olaylardan kurtarmak için hakkında bilgi edinin. Bir kullanıcı veya uygulama hatası veri bütünlüğünü etkileyen, bir Azure bölgesinde kesinti yaşandığında veya uygulamanız zaman bakıma gerek duyacağını ne yapılacağını öğrenin.

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>İş sürekliliği sağlamak için kullanabileceğiniz SQL Veritabanı özellikleri

Veritabanı açısından bakıldığında, dört ana olası kesintileri senaryo vardır:

- Bir disk sürücüsü arızası gibi veritabanı düğümü etkileyen yerel donanım veya yazılım hatası.
- Verilerin bozulması veya nedeni genellikle bir uygulama hatası ya da insan hatası tarafından silinmesi. Bu tür hataları, uygulamaya özgü ve genellikle veritabanı hizmeti tarafından algılanamıyor.
- Veri Merkezi kesintisi, büyük olasılıkla bir doğal felaket tarafından neden. Bu senaryo, bazı alternatif bir veri merkezine uygulama yük devretme ile coğrafi yedeklilik düzeyini gerektirir.
- Yükseltme veya bakım hataları, altyapı bakımı sırasında beklenmeyen sorunlar planlı veya yükseltme veritabanı önceki bir duruma hızla geri alma gerektirebilir.

Yerel donanım ve yazılım hataları gidermek için SQL veritabanı içeren bir [yüksek kullanılabilirlik mimarisi](sql-database-high-availability.md), %99.995 ile Bu hatalardan otomatik kurtarma garantileyen kullanılabilirlik SLA'sı.  

İşletmenizi veri kaybına karşı korumak için SQL veritabanı otomatik olarak tam veritabanı yedeklemeleri haftalık, değişiklik veritabanı yedeklemeleri her 12 saatte oluşturur ve işlem günlük yedeklemeleri her 5 - 10 dakika. Yedeklemeler, tüm hizmet katmanları için en az 7 gün için RA-GRS depolama alanında depolanır. Tüm hizmet katmanları temel dışında yapılandırılabilir yedekleme bekletme süresi için zaman içinde nokta geri yükleme, en fazla 35 gün öncesine destekler. 

SQL veritabanı ayrıca çeşitli planlanmamış senaryolarda etkisini azaltmak için kullanabileceğiniz çeşitli iş sürekliliği özellikleri sunar. 

- [Zamana bağlı tablolar](sql-database-temporal-tables.md) satır sürümlerini herhangi bir noktadan geri yüklemenize olanak sağlar.
- [Yerleşik otomatik yedeklerinde](sql-database-automated-backups.md) ve [geri yükleme noktası](sql-database-recovery-using-backups.md#point-in-time-restore) tam veritabanını belirli bir noktada için yapılandırılmış elde tutma dönemi içinde zaman içinde yukarı 35 gün öncesine geri yüklemenize olanak sağlar.
- Yapabilecekleriniz [silinen bir veritabanını geri yükleme](sql-database-recovery-using-backups.md#deleted-database-restore) başlangıçtan silinmiş varsa noktasına **SQL veritabanı sunucusu silinmediğini**.
- [Uzun süreli yedek saklama](sql-database-long-term-retention.md) yedeklemeleri 10 yıla kadar açık kalmasını sağlar.
- [Etkin coğrafi çoğaltma](sql-database-active-geo-replication.md) okunabilir çoğaltma ve el ile bir veri merkezi kesintisi veya uygulama yükseltmesi durumunda herhangi bir çoğaltmaya yük devretme oluşturmanıza olanak sağlar.
- [Otomatik Yük devretme grubu](sql-database-auto-failover-group.md#auto-failover-group-terminology-and-capabilities) uygulamayı otomatik olarak bir veri merkezi kesintisi durumunda kurtarma sağlar.

## <a name="recover-a-database-within-the-same-azure-region"></a>Aynı Azure bölgesindeki bir veritabanını kurtarma

Bir veritabanını geçmişteki zaman içinde bir noktaya geri yüklemek için otomatik veritabanı yedeklemelerini kullanabilirsiniz. Bu şekilde insan hataları nedeniyle veri bozulmaları gelen kurtarabilirsiniz. Zaman noktası geri yükleme bozulan olay öncesinde verilerin durumunu temsil eden ile aynı sunucuda yeni bir veritabanı oluşturmanıza olanak sağlar. Çoğu veritabanı geri yükleme işlemlerini 12 saatten kısa bir süre sürer. Çok büyük veya çok etkin bir veritabanına kurtarılması daha uzun sürebilir. Kurtarma süresi hakkında daha fazla bilgi için bkz. [veritabanı kurtarma zamanı](sql-database-recovery-using-backups.md#recovery-time). 

Belirli bir noktaya geri yükleme (PITR), uygulamanız için yeterli değil. desteklenen en yüksek yedek saklama dönemi, veritabanları için bir uzun süreli saklama (LTR) ilkesi yapılandırarak genişletebilirsiniz. Daha fazla bilgi için [uzun süreli yedek saklama](sql-database-long-term-retention.md).

## <a name="recover-a-database-to-another-azure-region"></a>Başka bir Azure bölgesine bir veritabanını kurtarma

Çok sık olmasa da Azure veri merkezlerinde kesintiler yaşanabilir. Kesinti yaşandığında yalnızca birkaç dakika sürebilecek veya saatler alacak bir hizmet kesintisi söz konusu olabilir.

- Seçeneklerden biri, veri merkezi kesintisi sona erdiğinde veritabanınızın çevrimdışı olmasını beklemektir. Bu, veritabanının çevrimdışı olmasının kabul edilebildiği uygulamalar için geçerlidir. Örnek olarak üzerinde sürekli çalışma yapmadığınız bir geliştirme projesi veya ücretsiz deneme sürümü verilebilir. Bir veri merkezinde bir kesinti varsa, böylece veritabanınızı bir süredir ihtiyacınız yoksa bu seçenek yalnızca çalışır, kesinti ne kadar sürebilecek, bilmezsiniz.
- Bir veritabanını kullanarak herhangi bir Azure bölgesi içinde herhangi bir sunucuda geri yüklemek için başka bir seçenektir [coğrafi olarak yedekli veritabanı yedeklemeleri](sql-database-recovery-using-backups.md#geo-restore) (coğrafi geri yükleme). Coğrafi geri yükleme, coğrafi olarak yedekli bir yedeklemesini, kaynağı olarak kullanır ve veritabanı veya veri merkezinde bir kesinti nedeniyle erişilemez durumda olsa bile bir veritabanını kurtarmak için kullanılabilir.
- Son olarak, coğrafi-ikincil kullanarak yapılandırdıysanız, kesinti hızla kurtarabilirsiniz [etkin coğrafi çoğaltma](sql-database-active-geo-replication.md) veya [otomatik yük devretme grubu](sql-database-auto-failover-group.md) veritabanını veya veritabanlarını için. Bu teknolojilerin dilediğiniz bağlı olarak, el ile veya otomatik yük devretme kullanabilirsiniz. Yük devretme kendisi yalnızca birkaç saniye sürer ancak hizmet etkinleştirmek için en az 1 saat sürer. Bu, yük devretme kesinti ölçek tarafından karardır emin olmak gereklidir. Ayrıca, yük devretme zaman uyumsuz çoğaltma niteliği nedeniyle küçük veri kaybına neden olabilir. 

İş sürekliliği planınızı geliştirirken, uygulamanın kesintiden sonra tamamen kurtarılmasına kadar kabul edilebilen maksimum süreyi anlamanız gerekir. Uygulamanın tamamen kurtarmak için gereken süre, Kurtarma süresi hedefi (RTO) bilinir. Ayrıca uygulama edilebilecek son veri güncelleştirmelerinin (zaman aralığı) maksimum süreyi anlamanız gereken planlanmamış bir kesintiden kurtarılırken. Olası veri kaybından kurtarma noktası hedefi (RPO) bilinir.

Farklı düzeylerde RPO ve RTO farklı kurtarma yöntemi sunar. Bir belirli kurtarma yöntemi seçebilir veya tam uygulama kurtarma achicethe yöntemlere birleşimini kullanın. Aşağıdaki tabloda RPO ve RTO her kurtarma seçeneği karşılaştırır.

| Kurtarma yöntemi | RTO | RPO |
| --- | --- | --- | 
| Coğrafi çoğaltmalı yedeklerden coğrafi geri yükleme | 12 sa. | 1 saat |
| Otomatik yük devretme grupları | 1 saat | 5 s |
| Elle yapılan veritabanı yük devretme | 30 s | 5 s |

> [!NOTE]
> *Elle yapılan veritabanı yük devretme* , coğrafi olarak çoğaltılmış ikincil kullanarak tek bir veritabanının yük devretme başvurduğu [planlanmamış modu](sql-database-active-geo-replication.md#active-geo-replication-terminology-and-capabilities).
Ayrıntılar için bu makalenin otomatik yük devretme RTO ve RPO tabloya bakın.


Uygulamanız aşağıdaki ölçütleri karşılıyorsa, otomatik yük devretme grupları kullanın:

- Görev açısından kritikse.
- 12 saat veya daha fazla kapalı kalma süresi için izin vermeyen bir hizmet düzeyi sözleşmesi (SLA) sahiptir.
- Kesinti süresi mali yükümlülük neden olabilir.
- Veri değişiklik oranı yüksek olan ve 1 saatlik veri kaybı kabul edilebilir değil.
- Etkin coğrafi çoğaltma ek maliyeti, olası mali yükümlülükten veya ilgili iş kaybından daha düşükse.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-protecting-important-DBs-from-regional-disasters-is-easy/player]
>

Veritabanı Yedeklemeleri ve uygulama gereksinimlerinize bağlı olarak active geografickou replikaci kullanmayı tercih edebilirsiniz. Tasarım konuları bağımsız veritabanları ve elastik havuzlar için bu iş sürekliliği özelliklerini kullanan bir tartışma için bkz [bulutta olağanüstü durum kurtarma için uygulama tasarlama](sql-database-designing-cloud-solutions-for-disaster-recovery.md) ve [elastik havuz olağanüstü durum Kurtarma stratejileri](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

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
