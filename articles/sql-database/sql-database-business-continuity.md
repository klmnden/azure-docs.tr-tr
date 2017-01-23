---
title: "Bulutta iş sürekliliği - veritabanı kurtarma - SQL Veritabanı | Microsoft Belgeleri"
description: "Azure SQL Veritabanı&quot;nın bulutta iş sürekliliği ve veritabanı kurtarmanın yanı sıra görev açısından kritik bulut uygulamalarının kesintisiz çalışması için sunduğu destek özelliklerini öğrenin."
keywords: "iş sürekliliği, bulutta iş sürekliliği, veritabanı olağanüstü durum kurtarma, veritabanı kurtarma"
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: 
ms.assetid: 18e5d3f1-bfe5-4089-b6fd-76988ab29822
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/13/2016
ms.author: carlrab;sashan
translationtype: Human Translation
ms.sourcegitcommit: 747f6ca642a33c4ce9bcaacad4976e8eaed8fa44
ms.openlocfilehash: f642cfade2369f5c758ab45994c7cf3f37b6d4c5


---
# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Azure SQL Veritabanı'nda iş sürekliliğine genel bakış
Bu genel bakış, Azure SQL Veritabanı'nın iş sürekliliği ve olağanüstü durum kurtarma özelliklerini açıklamaktadır. Veri kaybına veya veritabanınızın ve uygulamanızın kullanım dışı kalmasına neden olabilecek olaylardan kurtarmak için seçenekler, öneriler ve öğreticiler sunulmaktadır. Veri bütünlüğünü etkileyen bir kullanıcı veya uygulama hatası ortaya çıktığında, bir Azure bölgesinde kesinti yaşandığında veya uygulamanız için bakım yapılması gerektiğinde uygulanması gereken eylemler anlatılmaktadır. 

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>İş sürekliliği sağlamak için kullanabileceğiniz SQL Veritabanı özellikleri
SQL Veritabanı; otomatik yedekleme ve isteğe bağlı veritabanı çoğaltma gibi birçok iş sürekliliği özelliği sunar. Her özellik, tahmini kurtarma süresi (ERT) ve son işlemler için olası veri kaybı açısından farklı özelliklere sahiptir. Bu seçenekleri kavradıktan sonra aralarından birini seçebilir ve çoğu senaryoda farklı durumlar için birden fazlasını birlikte kullanabilirsiniz. İş sürekliliği planınızı geliştirirken, uygulamanın kesintiden sonra tamamen kurtarılmasına kadar kabul edilebilen maksimum süre olan kurtarma süresi hedefini (RTO) belirlemeniz gerekir. Ayrıca uygulama kesintiden sonra kurtarılırken kabul edilebilecek son veri güncelleştirmelerinin (zaman aralığı) maksimum miktarı olan kurtarma noktası hedefini (RPO) de kavramanız gerekir. 

Aşağıdaki tabloda en çok kullanılan üç senaryo için ERT ve RPO değerleri karşılaştırılmaktadır.

| Özellik | Temel katman | Standart katman | Premium katman |
| --- | --- | --- | --- |
| Yedekten belirli bir noktaya geri yükleme |7 gün içinde herhangi bir geri yükleme noktasına |35 gün içinde herhangi bir geri yükleme noktasına |35 gün içinde herhangi bir geri yükleme noktasına |
| Coğrafi çoğaltmalı yedeklerden coğrafi geri yükleme |ERT < 12 sa., RPO < 1 sa |ERT < 12 sa., RPO < 1 sa |ERT < 12 sa., RPO < 1 sa |
| Azure Backup Vault'tan geri yükleme |ERT < 12 sa., RPO < 1 hft. |ERT < 12 sa., RPO < 1 hft. |ERT < 12 sa., RPO < 1 hft. |
| Etkin Coğrafi Çoğaltma |ERT < 30 sn., RPO < 5 sn. |ERT < 30 sn., RPO < 5 sn. |ERT < 30 sn., RPO < 5 sn. |

### <a name="use-database-backups-to-recover-a-database"></a>Bir veritabanını kurtarmak için veritabanı yedeklerini kullanma
SQL Veritabanı, işletmenizi veri kaybına karşı korumak için haftalık tam veritabanı yedekleri, saatlik veritabanı değişiklik yedekleri ve beş dakikalık işlem günlüğü yedeklerini otomatik olarak gerçekleştirerek birlikte kullanır. Bu yedekler, yerel olarak yedekli depolamada Standart ve Premium hizmet katmanlarındaki veritabanları için 35 gün boyunca, Temel hizmet katmanındaki veritabanları için ise yedi gün boyunca saklanır. Hizmet katmanları hakkında daha fazla bilgi almak için bkz. [hizmet katmanları](sql-database-service-tiers.md). Hizmet katmanızın saklama süresi işletmenizin ihtiyaçlarını karşılamıyorsa, [hizmet katmanını değiştirerek](sql-database-scale-up.md) saklama süresini uzatabilirsiniz. Tam yedekler ve değişiklik yedekleri, veri merkezi kesintilerine karşı [eşleştirilmiş veri merkezine](../best-practices-availability-paired-regions.md) de çoğaltılır. Daha fazla ayrıntı için bkz. [otomatik veri yedekleme](sql-database-automated-backups.md).

Mevcut saklama süresi uygulamanız için yeterli değilse, veritabanlarınız için uzun süreli saklama ilkesini yapılandırarak uzatabilirsiniz. Daha fazla bilgi için bkz. [Uzun süreli saklama](sql-database-long-term-retention.md). 

Bu otomatik veritabanı yedekleme özelliklerini kullanarak hem kendi veri merkezinizdeki hem de başka veri merkezlerindeki veritabanlarını çeşitli kesintilerden kurtarabilirsiniz. Otomatik veritabanı yedekleriyle tahmini kurtarma süresi, aynı anda aynı bölgede kurtarılan veri tabanı sayısı, veritabanı boyutu, işlem günlüğü boyutu ve ağ bant genişliği gibi birden fazla etmene göre değişiklik gösterir. Çoğu durumda, kurtarma süresi 12 saati geçmez. Başka bir veri bölgesine kurtarma gerçekleştirirken, potansiyel veri kaybı saatlik veritabanı değişiklik yedeklerinin coğrafi olarak yedekli olması sayesinde 1 saatle sınırlıdır. 

> [!IMPORTANT]
> Otomatik yedekleri kullanarak kurtarma gerçekleştirmek için SQL Server Katılımcısı rolü üyesi veya abonelik sahibi olmanız gerekir. [RBAC: Yerleşik roller](../active-directory/role-based-access-built-in-roles.md). Verileri Azure portalı, PowerShell veya REST API kullanarak kurtarabilirsiniz. Transact-SQL kullanamazsınız.
> 
> 

Uygulamanız aşağıdaki koşullara uygunsa iş sürekliliği ve kurtarma hizmeti olarak otomatik yedeklemeyi kullanabilirsiniz:

* Görev açısından kritik değilse.
* Bağlayıcı SLA yoksa ve bu nedenle 24 saat veya üzeri kesinti süreleri mali yükümlülük yaratmayacaksa.
* Veri değişiklik oranı (bir saat içinde gerçekleştirilen işlem sayısı) düşükse ve bir saatlik değişiklik kaybı kabul edilebilirse. 
* Maliyetler önemliyse. 

Daha hızlı veri kurtarmaya ihtiyacınız varsa [Etkin Coğrafi Çoğaltma](sql-database-geo-replication-overview.md) (aşağıda açıklanmıştır) özelliğini kullanın. 35 günden eski verileri kurtarmanız gerekiyorsa, veritabanınızı düzenli olarak bir BACPAC dosyasına (veritabanı şemanızı ve ilgili verileri içeren sıkıştırılmış dosya) arşivleyin ve Azure blob depolama alanında ya da istediğiniz bir konumda saklayın. İşlemsel açıdan tutarlı veritabanı arşivi oluşturma hakkında daha fazla bilgi almak için bkz. [veritabanı kopyası oluşturma](sql-database-copy.md) ve [veritabanı kopyasını dışarı aktarma](sql-database-export.md). 

### <a name="use-active-geo-replication-to-reduce-recovery-time-and-limit-data-loss-associated-with-a-recovery"></a>Kurtarma süresini kısaltmak ve bir kurtarma işlemiyle ilişkilendirilmiş veri kaybını sınırlamak için Etkin Coğrafi Çoğaltma özelliğini kullanma
İş kesintisi durumunda veritabanı kurtarma için veritabanı yedeklerini kullanmaya ek olarak [Etkin Coğrafi Çoğaltma](sql-database-geo-replication-overview.md) özelliğini kullanarak bir veritabanı için seçtiğiniz bölgelerde en fazla dört okunabilir ikincil veritabanı yapılandırabilirsiniz. Bu ikincil veritabanları, birincil veritabanıyla zaman uyumsuz çoğaltma sistemi kullanılarak eşitlenir. Bu özellik, veri merkezi kesintisi veya uygulama yükseltme sırasında iş kesintilerini önlemek için kullanılır. Etkin Coğrafi Çoğaltma ayrıca farklı coğrafi bölgelerde bulunan kullanıcılara daha iyi salt okunur sorgu performansı sunmak için de kullanılabilir.

Birincil veritabanının beklenmedik şekilde çevrimdışı olması veya bakım nedeniyle çevrimdışı duruma getirmeniz gerekmesi halinde hızlı bir şekilde ikincil veritabanlarından birini birincil veritabanı olarak belirleyebilir (yük devretme olarak da bilinir) ve uygulamaları yeni birincil veritabanına bağlanacak şekilde yapılandırabilirsiniz. Planlı yük devretme sayesinde veri kaybı yaşanmaz. Plansız yük devretme durumunda ise zaman uyumsuz çoğaltmanın özelliğinden dolayı en son işlemler için az miktarda veri kaybı yaşanabilir. Yük devretme sonrasında belirli bir plana göre veya veri merkezi çevrimiçi duruma geldiğinde veritabanını yeniden çalıştırabilirsiniz. Tüm durumlarda kullanıcılar kısa süreli bir kesinti yaşar ve yeniden bağlantı kurmaları gerekir. 

> [!IMPORTANT]
> Etkin Coğrafi Çoğaltma özelliğini kullanmak için abonelik sahibi olmanız veya SQL Server'da yönetici izinlerine sahip olmanız gerekir. Yük devretmeyi abonelik izinlerini kullanarak Azure portal, PowerShell veya REST API aracılığıyla, SQL Server izinlerini kullanarak da Transact-SQL aracılığıyla yapılandırabilir ve gerçekleştirebilirsiniz.
> 
> 

Uygulamanız aşağıdaki ölçütleri karşılıyorsa Etkin Coğrafi Çoğaltma özelliğini kullanabilirsiniz:

* Görev açısından kritikse.
* 24 saat veya üzeri kesinti süresine izin vermeyen hizmet düzeyi sözleşmesine (SLA) sahipse.
* Kesinti süresi mali yükümlülük yaratıyorsa.
* Veri değişiklik oranı yüksekse ve bir saatlik değişiklik kaybı kabul edilebilir değilse.
* Etkin coğrafi çoğaltma ek maliyeti, olası mali yükümlülükten veya ilgili iş kaybından daha düşükse.

## <a name="recover-a-database-after-a-user-or-application-error"></a>Kullanıcı veya uygulama hatasından sonra bir veritabanını kurtarma
*Hiç kimse mükemmel değildir! Bir kullanıcı yanlışlıkla veri silebilir, istemeden önemli bir tabloyu ve hatta bir veritabanının tamamını bırakabilir. Ya da bir uygulama, hata nedeniyle yanlışlıkla gereksiz verileri gerekli verilerin üzerine yazabilir. 

Bu senaryoda kullanabileceğiniz kurtarma seçenekleri aşağıda verilmiştir.

### <a name="perform-a-point-in-time-restore"></a>Belirli bir noktaya geri yükleme gerçekleştirme
Noktanın veritabanı saklama süresi içinde olması şartıyla otomatik yedekleri kullanarak veritabanınızın bir kopyasını belirli bir noktaya geri yükleyebilirsiniz. Veritabanı geri yüklendikten sonra özgün veritabanını geri yüklenen veritabanıyla değiştirebilir veya geri yüklenen veritabanından gerekli verileri özgün veritabanına kopyalayabilirsiniz. Veritabanınız Etkin Coğrafi Çoğaltma özelliğini kullanıyorsa, geri yüklenen veritabanından gerekli verileri özgün veritabanına geri yüklemenizi öneririz. Özgün veritabanını geri yüklenen veritabanıyla değiştirmeniz halinde Etkin Coğrafi Çoğaltma özelliğini yeniden yapılandırmanız ve yeniden eşitlemeniz gerekir (bu işlemler büyük bir veritabanı için uzun sürebilir). 

Azure portal veya PowerShell kullanarak bir veritabanını belirli bir noktaya geri yüklemeyle ilgili ayrıntılı adımlar ve daha fazla bilgi için bkz. [belirli bir noktaya geri yükleme](sql-database-recovery-using-backups.md#point-in-time-restore). Transact-SQL kullanarak kurtarma gerçekleştiremezsiniz.

### <a name="restore-a-deleted-database"></a>Silinen veritabanını geri yükleme
Veritabanı silinmiş ancak mantıksal sunucu silinmemişse, silinen veritabanını silindiği noktaya geri yükleyebilirsiniz. Bu işlem bir veritabanı yedeğini silindiği mantıksal SQL sunucusuna geri yükler. Özgün adı kullanarak geri yükleyebilir veya geri yüklenen veritabanı için yeni bir ad belirleyebilirsiniz.

Azure portal veya PowerShell kullanarak silinen bir veritabanını geri yüklemeyle ilgili ayrıntılı adımlar ve daha fazla bilgi için bkz. [silinen veritabanını geri yükleme](sql-database-recovery-using-backups.md#deleted-database-restore). Transact-SQL kullanarak geri yükleme gerçekleştiremezsiniz.

> [!IMPORTANT]
> Mantıksal sunucu silinmişse, silinen bir veritabanını kurtaramazsınız. 
> 
> 

### <a name="restore-from-azure-backup-vault"></a>Azure Backup Vault'tan geri yükleme
Veri kaybı, otomatik yedekler için geçerli saklama süresinin dışında gerçekleştiyse ve veritabanınız uzun süreli saklama için yapılandırıldıysa, Azure Backup Vault'taki haftalık yedeklerden yeni bir veritabanına geri yükleyebilirsiniz. Bu noktada özgün veritabanını geri yüklenen veritabanıyla değiştirebilir veya geri yüklenen veritabanından gerekli verileri özgün veritabanına kopyalayabilirsiniz. Veritabanınızın büyük bir uygulama yükseltmesinden önceki sürümünü almanız ya da denetçilerden veya yasal kurumlardan gelen bir isteği karşılamanız gerekirse, Azure Backup Vault'ta kayıtlı bir tam yedeği kullanarak yeni bir veritabanı oluşturabilirsiniz.  Daha fazla bilgi için bkz. [Uzun süreli saklama](sql-database-long-term-retention.md).

## <a name="recover-a-database-to-another-region-from-an-azure-regional-data-center-outage"></a>Azure bölgesel veri merkezi kesintisinde bir veritabanını başka bir bölgeye kurtarma
<!-- Explain this scenario -->

Çok sık olmasa da Azure veri merkezlerinde kesintiler yaşanabilir. Kesinti yaşandığında yalnızca birkaç dakika sürebilecek veya saatler alacak bir hizmet kesintisi söz konusu olabilir. 

* Seçeneklerden biri, veri merkezi kesintisi sona erdiğinde veritabanınızın çevrimdışı olmasını beklemektir. Bu, veritabanının çevrimdışı olmasının kabul edilebildiği uygulamalar için geçerlidir. Örnek olarak üzerinde sürekli çalışma yapmadığınız bir geliştirme projesi veya ücretsiz deneme sürümü verilebilir. Bir veri merkezinde yaşanan kesintinin süresi tam olarak kestirilemeyeceği için bu seçenek yalnızca veritabanınıza sürekli erişmeniz gerekmiyorsa kullanışlı olacaktır.
* Başka bir seçenek de Etkin Coğrafi Çoğaltma kullanıyorsanız başka bir veri bölgesine yük devretme gerçekleştirmeniz veya verileri coğrafi çoğaltmalı veritabanı yedeklerini kullanarak kurtarmaktır (Coğrafi Geri Yükleme). Yük devretme yalnızca birkaç saniye sürer ancak yedekten kurtarma saatler sürebilir.

Bir veri merkezi kesintisinde harekete geçtiğinizde kurtarma için gereken süre ve kaybedeceğiniz veri miktarı, uygulamanızda yukarıda anlatılan iş sürekliliği özelliklerini kullanma şeklinize bağlıdır. Uygulamanızın gereksinimlerine göre veritabanı yedekleriyle Etkin Coğrafi Çoğaltma özelliklerini bir arada kullanmayı tercih edebilirsiniz. Bu iş sürekliliği özelliklerini kullanan bağımsız veritabanları ve elastik havuzlarla ilgili dikkate alınacak uygulama tasarımı noktaları hakkında ayrıntılı bilgi için bkz. [Bulutta olağanüstü durum kurtarma için uygulama tasarlama](sql-database-designing-cloud-solutions-for-disaster-recovery.md) ve [Elastik havuz olağanüstü durum kurtarma stratejileri](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

Aşağıdaki bölümlerde veritabanı yedeklerini veya Etkin Coğrafi Çoğaltma özelliğini kullanarak veri kurtarma adımlarının özeti verilmiştir. Planlama gereksinimleri ve kurtarma sonrası adımlar dahil olmak üzere ayrıntılı adımların yanı sıra olağanüstü durum kurtarma tatbikatı yapmak için kesinti simülasyonu yapma hakkında bilgiler için bkz. [Bir SQL veritabanını kesintiden kurtarma](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Kesinti için hazırlanma
Kullandığınız iş sürekliliği özelliğinden bağımsız olarak aşağıdaki adımları uygulamanız gerekir:

* Sunucu düzeyi güvenlik duvarı kuralları, giriş bilgileri ve ana veritabanı düzeyi izinler dahil olmak üzere hedef sunucuyu belirlemek ve hazırlamak
* İstemcilerin ve istemci uygulamalarının yeni sunucuya nasıl yönlendirileceğini belirlemek
* Denetim ayarları ve uyarılar gibi diğer bağımlılık belgelerini oluşturmak 

Planlama ve hazırlık aşamalarını doğru şekilde yapmazsanız, yük devretme veya kurtarma sonrasında uygulamalarınızın çevrimiçi olması daha uzun sürebilir ve ortaya çıkan sorunları sınırlı bir sürede gidermek zorunda kalarak sıkıntıya girebilirsiniz.

### <a name="failover-to-a-geo-replicated-secondary-database"></a>Coğrafi çoğaltmalı ikincil veritabanına yük devretme
Kurtarma sistemi olarak Etkin Coğrafi Çoğaltma hizmetini kullanıyorsanız, [coğrafi çoğaltmalı ikincil veritabanına yük devretmeyi zorlayabilirsiniz](sql-database-disaster-recovery.md#failover-to-geo-replicated-secondary-database). İkincil veritabanı saniyeler içinde yeni birincil veritabanı haline getirilerek yeni işlemleri kaydetmeye ve sorguları yanıtlamaya hazır hale gelir. Yalnızca henüz çoğaltılmamış veriler için birkaç saniyelik veri kaybı yaşanır. Yük devretme işlemini otomatik hale getirme hakkında bilgi için bkz. [Bir uygulamayı bulutta olağanüstü durum kurtarma için tasarlama](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [!NOTE]
> Veri merkezi tekrar çevrimiçi olduğunda özgün birincil veritabanını yeniden çalıştırabilirsiniz (isterseniz).
> 
> 

### <a name="perform-a-geo-restore"></a>Coğrafi Geri Yükleme gerçekleştirme
Kurtarma sisteminizde coğrafi olarak yedekli depolamayı kullanıyorsanız [Coğrafi Geri Yükleme kullanarak bir veritabanı kurtarma işlemi başlatabilirsiniz](sql-database-disaster-recovery.md#recover-using-geo-restore). Kurtarma işlemi genelde 12 saat içinde gerçekleşir. Son saatlik değişiklik yedeğinin alındığı ve çoğaltıldığı zaman göre en fazla bir saatlik veri kaybı yaşanabilir. Kurtarma işlemi tamamlanana kadar veritabanı işlem kaydedemez ve sorgulara yanıt veremez. 

> [!NOTE]
> Veri merkezi uygulamanızı kurtarılan veritabanına geçirmeden çevrimiçi olursa kurtarma işlemini iptal edebilirsiniz.  
> 
> 

### <a name="perform-post-failover--recovery-tasks"></a>Yük devretme/kurtarma sonrası görevleri gerçekleştirme
Bu iki kurtarma sisteminden herhangi biriyle gerçekleştirilen kurtarma işleminden sonra kullanıcılarınızın ve uygulamalarınızın çalışmaya devam etmesi için aşağıdaki ek görevleri gerçekleştirmeniz gerekir:

* İstemcilerin ve istemci uygulamalarının yeni sunucuya ve geri yüklenen veritabanına nasıl yönlendirileceğini belirleme
* Kullanıcılarınızın bağlantı kurabilmesi için gerekli sunucu düzeyi güvenlik duvarı kurallarının mevcut olduğunu doğrulama ([veritabanı düzeyi güvenlik duvarı](sql-database-firewall-configure.md#creating-database-level-firewall-rules) da kullanabilirsiniz)
* Uygun giriş bilgilerinin ve ana veritabanı düzeyi izinlerin mevcut olduğunu doğrulama ([kapsanan kullanıcıları](https://msdn.microsoft.com/library/ff929188.aspx) da kullanabilirsiniz)
* Denetimi uygun şekilde yapılandırma
* Uyarıları uygun şekilde yapılandırma

## <a name="upgrade-an-application-with-minimal-downtime"></a>Bir uygulamayı en az kesinti süresiyle yükseltme
Bazen bir uygulamanın yükseltme gibi nedenlerle planlı bakım kapsamında çevrimdışı olması gerekebilir. [Uygulama yükseltmelerini yönetme](sql-database-manage-application-rolling-upgrade.md) sayfasında, yükseltme işlemleri sırasında kesinti süresini en aza indirme ve hata ihtimaline karşı kurtarma yolu sağlama amacıyla bulut uygulamanızın sıralı yükseltmelerini etkinleştirmek üzere Etkin Coğrafi Çoğaltma özelliğini nasıl kullanacağınız açıklanmaktadır. Bu makalede yükseltme işlemini yönetmek için iki farklı yöntem ele alınmakta ve her birinin artıları ve eksileri incelenmektedir.

## <a name="next-steps"></a>Sonraki adımlar
Bağımsız veritabanları ve elastik havuzlarla ilgili dikkate alınacak uygulama tasarımı noktaları hakkında ayrıntılı bilgi için bkz. [Bulutta olağanüstü durum kurtarma için uygulama tasarlama](sql-database-designing-cloud-solutions-for-disaster-recovery.md) ve [Elastik havuz olağanüstü durum kurtarma stratejileri](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).




<!--HONumber=Dec16_HO2-->


