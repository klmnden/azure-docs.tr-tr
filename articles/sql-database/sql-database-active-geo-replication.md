---
title: Etkin coğrafi çoğaltma - Azure SQL veritabanı | Microsoft Docs
description: Etkin coğrafi çoğaltma, aynı veya farklı bir veri merkezi (bölge) veritabanlarını tek tek okunabilir ikincil veritabanı oluşturmak için kullanın.
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
ms.date: 03/26/2019
ms.openlocfilehash: ca53f4bfa80d6fdead24dc7d562c2240bb3fa86d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60387460"
---
# <a name="creating-and-using-active-geo-replication"></a>Oluşturma ve etkin coğrafi çoğaltma kullanma

Etkin coğrafi çoğaltma, okunabilir ikincil veritabanı veritabanlarını tek tek bir SQL veritabanı sunucusunda aynı veya farklı bir veri merkezi (bölge) oluşturmanıza olanak sağlar, Azure SQL veritabanı özelliğidir.

> [!NOTE]
> Etkin coğrafi çoğaltma, yönetilen örneği tarafından desteklenmiyor. Kullanmak için coğrafi yük devretme yönetilen örnekleri [otomatik yük devretme grupları](sql-database-auto-failover-group.md).

Etkin coğrafi çoğaltma, tek veritabanlarını bölgesel bir olağanüstü durum ya da büyük ölçekli kesinti olması durumunda hızlı bir olağanüstü durum kurtarma gerçekleştirmek uygulama izin veren bir iş sürekliliği çözümü olarak tasarlanmıştır. Coğrafi çoğaltma etkinleştirildiğinde, uygulama farklı bir Azure bölgesinde bir ikincil veritabanına yük devretme başlatabilirsiniz. En fazla dört ikincil veritabanı aynı veya farklı bölgelerde desteklenir ve ikincil veritabanı salt okunur erişim sorgular için de kullanılabilir. Yük devretmeyi el ile uygulama veya kullanıcı tarafından başlatılması gerekir. Yük devretme işleminden sonra yeni birincil farklı bir bağlantı uç noktası vardır. Aşağıdaki diyagramda, etkin coğrafi çoğaltmayı kullanarak coğrafi olarak yedekli bir bulut uygulamasının tipik yapılandırma gösterilmektedir.

![Etkin coğrafi çoğaltma](./media/sql-database-active-geo-replication/geo-replication.png )

> [!IMPORTANT]
> SQL veritabanı, otomatik yük devretme grupları da destekler. Daha fazla bilgi için bkz [otomatik yük devretme grupları](sql-database-auto-failover-group.md). Ayrıca, bir yönetilen örnek içinde oluşturulan veritabanları için etkin coğrafi çoğaltma desteklenmiyor. Kullanmayı [yük devretme grupları](sql-database-auto-failover-group.md) yönetilen örneğine sahip.

Örneğin herhangi nedeni, birincil veritabanı başarısız olursa veya çevrimdışı olarak kullanılabileceğini gerekiyor, yük devretme ikincil veritabanlarınızı birini başlatabilirsiniz. Yük devretme ikincil veritabanlarından biri etkinleştirildiğinde, diğer tüm ikincil veritabanı otomatik olarak yeni birincil siteye bağlanır.

Çoğaltma ve yük devretme işlemlerini tek bir veritabanı veya bir sunucuda veya etkin coğrafi çoğaltmayı kullanarak elastik bir havuzdaki veritabanları kümesi yönetebilirsiniz. Bu kullanarak bunu yapabilirsiniz:

- [Azure portalı](sql-database-geo-replication-portal.md)
- [PowerShell: Tek veritabanı](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
- [PowerShell: Elastik havuz](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
- [Transact-SQL: Tek veritabanı'nı veya elastik Havuzu'nu](/sql/t-sql/statements/alter-database-azure-sql-database)
- [REST API: Tek veritabanı](https://docs.microsoft.com/rest/api/sql/replicationlinks)

Yük devretme işleminden sonra yeni birincil sunucu ve veritabanı için kimlik doğrulama gereksinimleri yapılandırıldığından emin olun. Ayrıntılar için bkz [olağanüstü durum kurtarma sonrasında SQL veritabanı güvenlik](sql-database-geo-replication-security-config.md).

Etkin coğrafi çoğaltma yararlanır [Always On](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) zaman uyumsuz olarak kaydedilen işlem sayısı birincil veritabanında anlık görüntü yalıtımı kullanarak ikincil bir veritabanı çoğaltma için SQL Server teknolojisi. Otomatik Yük devretme grupları üzerinde etkin coğrafi çoğaltma grubu semantiği sağlar ancak aynı zaman uyumsuz çoğaltma mekanizması kullanılır. Herhangi bir anda ederken, ikincil veritabanı birincil veritabanının gerisinde biraz olabilir, ikincil veri kısmi hareket hiçbir zaman olması garanti edilir. Bölgeler arası yedeklilik uygulamaları hızlı bir şekilde veri merkezinin tamamı veya doğal afetler, geri dönülemez bir insan hatalarına veya kötü amaçlı davranışlara neden bir veri merkezinin bölümleri kalıcı kaybı kurtarmanıza olanak sağlar. Belirli RPO verileri şu yolda bulunabilir: [iş Sürekliliğine genel bakış](sql-database-business-continuity.md).

> [!NOTE]
> İki bölgeleri arasında bir ağ hatası varsa, 10 saniyede yeniden bağlantı kurmak için tekrar deneyeceğiz.
> [!IMPORTANT]
> Birincil veritabanında önemli bir değişiklik, yük devretmeden önce ikincil çoğaltılır sağlamak için önemli değişiklikler (örneğin, parola güncelleştirmeleri) çoğaltılmasını sağlamak için eşitleme zorlayabilirsiniz. Tüm kaydedilmiş işlemleri çoğaltılır kadar çağıran iş parçacığını engeller. çünkü zorlamalı eşitleme performansı etkiler. Ayrıntılar için bkz [sp_wait_for_database_copy_sync](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync). Birincil veritabanı arasındaki coğrafi-ikincil çoğaltma gecikmesi izlemek için bkz: [sys.dm_geo_replication_link_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database).

Aşağıdaki şekilde Orta Güney ABD bölgesinde yapılandırılmış Kuzey Orta ABD bölgesinde birincil ve ikincil etkin coğrafi çoğaltma örneği gösterilmektedir.

![coğrafi çoğaltma ilişkisi](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

Okunabilir ikincil veritabanları olduğundan, raporlama işleri gibi salt okunur iş yüklerini boşaltmak için kullanılabilir. Etkin coğrafi çoğaltma kullanıyorsanız, aynı bölgede birincil ile ikincil veritabanı oluşturmak mümkündür, ancak uygulamanın esnekliği yıkıcı hatalarına artırmaz. Otomatik Yük devretme gruplarını kullanıyorsanız ikincil veritabanınıza her zaman farklı bir bölgede oluşturulur.

Ek olarak olağanüstü durum kurtarma etkin coğrafi çoğaltma, aşağıdaki senaryolarda kullanılabilir:

- **Veritabanı geçişi**: Etkin coğrafi çoğaltma, bir veritabanı için en düşük kapalı kalma süresi ile çevrimiçi başka bir sunucudan geçirmek için kullanabilirsiniz.
- **Uygulama yükseltmeleri**: Uygulama yükseltmeleri sırasında başarısız geri kopya olarak, ek bir ikincil oluşturabilirsiniz.

Gerçek iş sürekliliği elde etmek için veri merkezleri arasında veritabanı yedeklilik ekleyerek yalnızca çözümün parçasıdır. Bir arıza kurtarma hizmeti oluşturan tüm bileşenlerini ve tüm bağımlı hizmetlerin gerektirir sonra bir uygulama (hizmet) için uçtan uca kurtarma. İstemci yazılımını (örneğin, tarayıcı özel bir JavaScript ile), web ön uçları, depolama ve DNS bu bileşenlerin örnekleridir. Tüm bileşenlerin aynı hatalara karşı dayanıklı ve kurtarma süresi hedefi, uygulamanızın içinde (RTO) kullanılabilir hale önemlidir. Bu nedenle, tüm bağımlı hizmetleri tanımlayın ve yetenekleri sağlarlar ve garanti anlamak gerekir. Ardından, bağımlı olduğu hizmetlerin yük devretme sırasında hizmetinizin emin olmak için yeterli adımları atmanız gerekir. Olağanüstü durum kurtarma çözümleri tasarlama hakkında daha fazla bilgi için bkz. [olağanüstü durum kurtarma'yı kullanarak etkin coğrafi çoğaltma için bulut çözümleri tasarlama](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="active-geo-replication-terminology-and-capabilities"></a>Etkin coğrafi çoğaltma terminoloji ve özellikleri

- **Otomatik zaman uyumsuz çoğaltma**

  Mevcut bir veritabanına ekleyerek yalnızca ikincil bir veritabanı oluşturabilirsiniz. Herhangi bir Azure SQL veritabanı sunucusu, ikincil oluşturulabilir. Oluşturulduktan sonra ikincil veritabanı birincil veritabanından kopyalanan verileri doldurulur. Bu işlem, dengeli dağıtım olarak bilinir. İkincil veritabanı oluşturduktan ve çekirdek değeri oluşturulmuş sonra birincil veritabanına güncelleştirmeleri zaman uyumsuz olarak ikincil veritabanına otomatik olarak çoğaltılır. Zaman uyumsuz çoğaltma, birincil veritabanında işlemleri yapıldığından ikincil veritabanına çoğaltılırken anlamına gelir.

- **Okunabilir ikincil veritabanı**

  Bir uygulama, ikincil bir veritabanını birincil veritabanına erişmek için kullanılan aynı veya farklı güvenlik prensiplerinin salt okunur işlemler için erişebilirsiniz. Güncelleştirmeler (günlük yeniden yürütme) birincil çoğaltması ikincil yürütülen sorgular tarafından gecikiyor değil emin olmak için anlık görüntü yalıtım modunda ikincil veritabanlarıyla çalışır.

> [!NOTE]
> Birincil şema güncelleştirmeleri varsa, günlük yeniden yürütme ikincil veritabanı üzerinde gecikir. İkincisi, ikincil veritabanında bir şema kilidi gerektirir.
> [!IMPORTANT]
> Coğrafi çoğaltma, birincil olarak aynı bölgede ikincil bir veritabanı oluşturmak için kullanabilirsiniz. Bu ikincil veritabanına Yük Dengeleme salt okunur iş yükleri aynı bölgede kullanabilirsiniz. Ancak, ikincil bir veritabanı ile aynı bölgede hataya dayanıklılık sağlamaz ve bu nedenle bir olağanüstü durum kurtarma için uygun yük devretme hedefi değil. Ayrıca avaialability bölge yalıtım garantilemez. İş açısından kritik ya da Premium hizmet katmanı ile kullanmak [bölge yedekli Yapılandırması](sql-database-high-availability.md#zone-redundant-configuration) avaialability bölge ayırma sağlamak için.   
>

- **Planlı yük devretme**

  Planlı yük devretme, birincil ve ikincil veritabanlarını birincil role ikincil anahtarlar önce arasında tam eşitleme gerçekleştirir. Bu, veri kaybı olmadan garanti eder. Planlı yük devretme kullanılan aşağıdaki senaryolar: (a) DR gerçekleştirmek ayrıntılı açıklanmıştır üretimde veri kaybı kabul edilebilir; olmadığında veritabanını farklı bir bölgeye; dışında yeniden konumlandırmakta (b) ve (yeniden çalışma) (c) kesinti sonra veritabanı birincil bölgeye geri dönmek için azaltılabilir.

- **Planlanmamış yük devretme**

  Planlanmamış veya zorlamalı yük devretme hemen ikincil birincil ile herhangi bir eşitleme olmadan birincil role geçirir. Bu işlem veri kaybına yol açar. Birincil erişilebilir olmadığı durumlarda, planlanmamış yük devretme kesintiler sırasında kurtarma yöntemi olarak kullanılır. Özgün birincil yeniden çevrimiçi olduğunda, bunu otomatik olarak eşitleme yeniden bağlanma ve yeni ikincil olur.

- **Birden çok okunabilir ikincil veritabanı**

  En fazla 4 ikincil veritabanları için her birincil oluşturulabilir. Yeni bir ikincil veritabanı oluşturulana kadar uygulamayı yalnızca bir ikincil veritabanı yoktur ve yine başarısız olursanız, daha yüksek risklere açıktır. Birden fazla ikincil veritabanı yoksa, ikincil veritabanlarından biri başarısız olsa bile uygulama korumalı olarak kalır. Ek ikinciller de kullanılabilir genişleme salt okunur iş yükleri için

  > [!NOTE]
  > Global olarak dağıtılmış bir uygulama oluşturun ve dörtten fazla bölgede veri salt okunur erişim sağlamak etkin coğrafi çoğaltma kullanıyorsanız ikincil bir ikincil (zincirleme olarak bilinen bir işlem) oluşturabilirsiniz. Bu şekilde veritabanı çoğaltma neredeyse sınırsız ölçek elde edebilirsiniz. Ayrıca, zincirleme birincil veritabanı çoğaltma ek yükü azaltır. Yaprak en ikincil veritabanları üzerinde artan çoğaltma gecikmesi dengedir.

- **Bir elastik havuzdaki veritabanlarının coğrafi çoğaltma**

  Her ikincil veritabanı, bir elastik havuzda ayrı olarak katılan ya da herhangi bir elastik havuzda hiç olmaması. Her bir ikincil veritabanı için havuz seçeneği ayrıdır ve diğer bir ikincil yapılandırmanıza bağlı değildir (birincil veya ikincil olup olmadığını) veritabanı. Her esnek havuzun tek bir bölgede yer alan, bu nedenle aynı topoloji birden fazla ikincil veritabanları elastik havuz hiçbir zaman paylaşabilirsiniz.

- **İkincil veritabanının yapılandırılabilir işlem boyutu**

  Birincil ve ikincil veritabanları aynı hizmet katmanı için gereklidir. Aynı işlem boyutu (Dtu veya sanal çekirdekler) olarak birincil ile ikincil veritabanı oluşturulan de önemle tavsiye edilir. İkincil bir alt işlem boyutu ile olası olmadığından, ikincil bir artan çoğaltma gecikmesi'nin süresi, risk altındadır ve sonuç olarak bir yük devretme sonrasında önemli veri kaybı riski. Sonuç olarak, yayımlanan RPO = 5 sn olamaz garanti edilir. Diğer sorununa neden daha yüksek bir işlem boyutu için yükseltilene kadar yük devretmeden sonra uygulama performansını işlem kapasitesi yeni birincil eksikliği nedeniyle etkilenecek emin olur. Yükseltme süresini veritabanı boyutuna bağlıdır. Ayrıca, şu anda bu tür yükseltme birincil ve ikincil veritabanları çevrimiçi olduğundan ve bu nedenle, kesinti giderildikten kadar tamamlanamıyor, gerektirir. Alt işlem boyutu ile ikincil oluşturmaya karar verirseniz, Azure portalında günlük g/ç yüzdesi grafik ikincil çoğaltma yükü sürdürebilmek için gereken en düşük işlem boyutunu tahmin etmek için iyi bir yol sağlar. Örneğin, birincil veritabanınız P6 ise (1000 DTU) ve kendi günlük g/ç yüzdesi 50 ikincil en az olması gerekir % P4 (500 DTU). Kullanarak günlük GÇ veri de alabilirsiniz [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) veya [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) veritabanı görünümleri.  SQL veritabanı işlem boyutları hakkında daha fazla bilgi için bkz. [SQL veritabanı hizmet katmanları nelerdir](sql-database-purchase-models.md).

- **Kullanıcı tarafından denetlenen bir yük devretme ve yeniden çalışma**

  İkincil bir veritabanı açıkça birincil role herhangi bir zamanda uygulama veya kullanıcı tarafından değiştirilebilir. Gerçek bir kesinti sırasında "planlanmamış" seçeneği, hangi hemen ikincil yükseltir birincil olarak kullanılmalıdır. Başarısız birincil kurtarır ve yeniden kullanılabilir olduğunda, sistem otomatik olarak kurtarılan birincil ikincil olarak işaretler ve yeni birincil ile güncel getirin. Birincil ikincil en son değişiklikleri çoğaltır önce başarısız olursa zaman uyumsuz çoğaltma yapısı nedeniyle, az miktarda veriniz planlanmamış yük devretme sırasında kaybolur. Birden çok ikincilleriyle birincil yük devrettiğinde, sistem otomatik olarak çoğaltma ilişkilerini yeniden yapılandırır ve kalan ikincil veritabanı herhangi bir kullanıcı müdahalesi gerektirmeden yeni yükseltilen birincil siteye bağlar. Yük devretme neden oldu kesinti giderildikten sonra uygulamayı birincil bölgeye geri istenebilir. Bunu yapmak için yük devretme komut "planlanmış" seçeneği ile çağrılmalıdır.

- **Kimlik bilgileri ve güvenlik duvarı kuralları eşitlenmiş durumda tutma**

Kullanmanızı öneririz [veritabanı düzeyinde güvenlik duvarı kuralları IP](sql-database-firewall-configure.md) bu kurallar, tüm ikincil veritabanlarını birincil olarak aynı IP güvenlik duvarı kuralları olduğundan emin olmak için veritabanı kullanılarak çoğaltılabilir. Bu nedenle coğrafi çoğaltmalı veritabanı. Bu yaklaşım el ile yapılandırmak ve hem birincil ve ikincil veritabanlarını barındıran sunucu güvenlik duvarı kurallarını sağlamak müşterilerin ihtiyacını ortadan kaldırır. Benzer şekilde, kullanarak [kapsanan veritabanı kullanıcıları](sql-database-manage-logins.md) veriler için birincil ve ikincil veritabanları her zaman sahip aynı erişim sağlar. kullanıcı kimlik bilgilerini bir yük devretme sırasında bu yüzden hiçbir kesinti nedeniyle oturum açma ve parolaları ile uyuşmuyor. Ek olarak [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md), müşteriler, hem birincil hem de ikincil veritabanları için kullanıcı erişimini yönetebilir ve yönetme gereksinimini ortadan kimlik bilgileri veritabanlarında tamamen.

## <a name="upgrading-or-downgrading-a-primary-database"></a>Yükseltme veya bir birincil veritabanı önceki sürüme indirme

Yükseltme veya birincil farklı işlem boyuta (aynı hizmet katmanında, genel amaçlı ve iş açısından kritik arasında değil) tüm ikincil veritabanlarını kesmeden düşürebilirsiniz. Yükseltme sırasında ikincil veritabanı yükseltmeniz ve ardından birincil yükseltmeniz önerilir. Önceki sürüme indirirken ters sırada: birincil ilk düşürmek ve ardından ikincil düşürme. Bu öneri, yükseltme ya da farklı bir hizmet katmanına düşürebilirsiniz zorlanır.

> [!NOTE]
> Yük devretme grubu yapılandırmasının bir parçası olarak ikincil bir veritabanı oluşturduysanız, bu ikincil veritabanını indirgemek için önerilmez. Bu veri katmanınızın yük devretme etkinleştirildikten sonra normal iş yükünü işlemek için yeterli kapasiteye sahip sağlamaktır.

> [!IMPORTANT]
> Bir yük devretme grubunda birincil veritabanı, ikincil veritabanı ilk olarak üst katmana ölçeklendirilir sürece daha yüksek bir katmana ölçeklendirilemez. İkincil veritabanı ölçeklendirilir önce birincil veritabanının ölçeğini çalışırsanız aşağıdaki hatayı alabilirsiniz:
>
> `Error message: The source database 'Primaryserver.DBName' cannot have higher edition than the target database 'Secondaryserver.DBName'. Upgrade the edition on the target before upgrading the source.`
>

## <a name="preventing-the-loss-of-critical-data"></a>Önemli verilerin kaybını önleme

Geniş alan ağları yüksek gecikme nedeniyle, sürekli kopyalama bir zaman uyumsuz çoğaltma mekanizması kullanır. Bir hata oluşması durumunda zaman uyumsuz çoğaltma bazı veri kaybı kaçınılmaz yapar. Ancak, bazı uygulamalar, veri kaybı olmadan gerektirebilir. Bu kritik güncelleştirmeler korumak için bir uygulama geliştiricisi çağırabilirsiniz [sp_wait_for_database_copy_sync](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) işlem Sistemi'ne hemen sonra sistem yordamı. Çağırma **sp_wait_for_database_copy_sync** son kabul edilen işlem ikincil veritabanına gönderilene kadar çağıran iş parçacığını engeller. Ancak, yeniden yürütülmesi ve ikincil kaydedilen iletilen işlemler için beklemez. **sp_wait_for_database_copy_sync** belirli sürekli kopyalama bağlantısı kapsamlıdır. Birincil veritabanına bağlantı haklarına sahip herhangi bir kullanıcı, bu yordam çağırabilir.

> [!NOTE]
> **sp_wait_for_database_copy_sync** yük devretmeden sonra veri kaybı yaşanmasını önler, ancak tam eşitleme yönelik okuma erişimi garanti etmez. Gecikme nedeniyle bir **sp_wait_for_database_copy_sync** yordam çağrısı önemli ölçüde fazla olabilir ve aramanın zaman işlem günlüğünün boyutuna bağlıdır.

## <a name="monitoring-geo-replication-lag"></a>Coğrafi çoğaltma gecikmesi izleme

Lag RPO'ya göre izlemek için kullanabilirsiniz *replication_lag_sec* sütununun [sys.dm_geo_replication_link_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database) birincil veritabanı. Kabul edilen birincil ve ikincil kalıcı işlemleri arasındaki saniye cinsinden gecikme gösterir. Örneğin gecikme değeri 1 saniyedir, birincil şu anda bir kesintisinden etkilenirse anlamına gelir ve yük devretme intiated, 1 saniye, en son transtions kaydedilmeyecek. 

İkincil bölgeden okumak başka bir deyişle kullanılabilir ikincil uygulanmış olan değişikliklere birincil veritabanında lag ölçmek için karşılaştırma *last_commit* zaman ikincil veritabanı birincil aynı değere sahip Veritabanı.

> [!NOTE]
> Bazen *replication_lag_sec* birincil veritabanının birincil şu anda ne kadar ikincil olduğunu bilmez, yani bir NULL değerine sahip.   Bu genellikle sonra işlemi yeniden başlatır ve gereken olur geçici bir durum olabilir. Uygulama, uyarı göz önünde bulundurun *replication_lag_sec* uzun bir süre için NULL döndürür. Bunu kalıcı bir bağlantı hatası nedeniyle birincil ile ikincil veritabanı iletişim kuramıyor gösterir. Ayrıca arasındaki farkı neden olabilecek koşullar vardır *last_commit* ikincil ve büyük boyutlu hale gelmesi için birincil veritabanında zaman. Örneğin bir işleme hiçbir değişiklik uzun bir süre sonra birincil yaptıysanız fark hızla 0 olarak dönmeden önce kadar büyük bir değere atlayacaktır. Bu iki değer arasındaki farkı uzun bir süredir büyük kaldığında bir hata durumu göz önünde bulundurun.


## <a name="programmatically-managing-active-geo-replication"></a>Etkin coğrafi çoğaltma programlama yoluyla yönetme

Daha önce açıklandığı gibi etkin coğrafi çoğaltma ayrıca Azure PowerShell ve REST API'sini kullanarak program aracılığıyla yönetilebilir. Aşağıdaki tabloda kullanılabilir komut kümesi açıklanmaktadır. Etkin coğrafi çoğaltma içeren bir Azure Resource Manager API'leri kümesi yönetimi için de dahil olmak üzere [Azure SQL veritabanı REST API](https://docs.microsoft.com/rest/api/sql/) ve [Azure PowerShell cmdlet'lerini](https://docs.microsoft.com/powershell/azure/overview). Bu API'ler, kaynak gruplarının kullanımı gerektirir ve rol tabanlı güvenlik (RBAC) desteği. Uygulama erişim rolleri hakkında daha fazla bilgi için bkz. [Azure rol tabanlı erişim denetimi](../role-based-access-control/overview.md).

### <a name="t-sql-manage-failover-of-single-and-pooled-databases"></a>T-SQL: Yük devretme işlemlerini tek ve havuza alınmış veritabanlarını yönetme

> [!IMPORTANT]
> Yalnızca şu Transact-SQL komutlarını uygulamak için etkin coğrafi çoğaltma ve yük devretme grupları için geçerli değildir. Yalnızca Yük devretme grupları destekledikleri olarak bu nedenle, bunlar da yönetilen örnekleri için geçerli değildir.

| Komut | Açıklama |
| --- | --- |
| [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current) |İkincil üzerinde Sunucu Ekle bağımsız değişkeni bir var olan veritabanı ve başladığında veri çoğaltma için ikincil bir veritabanı oluşturmak için kullanın |
| [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current) |Yük devretmeyi başlatmak için birincil olarak ikincil bir veritabanı geçiş için yük DEVRETME veya FORCE_FAILOVER_ALLOW_DATA_LOSS kullanın |
| [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current) |Bir SQL veritabanı ile belirtilen ikincil veritabanı arasında bir veri çoğaltma sonlandırmak için ikincil üzerinde SUNUCUYU Kaldır'ı kullanın. |
| [sys.geo_replication_links](/sql/relational-databases/system-dynamic-management-views/sys-geo-replication-links-azure-sql-database) |Azure SQL veritabanı sunucusunda her veritabanı için mevcut tüm yineleme bağlantıları hakkında bilgi döndürür. |
| [sys.dm_geo_replication_link_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database) |Belirli bir SQL veritabanı için son çoğaltma saati, son çoğaltma gecikmesine ve çoğaltma bağlantısı ile ilgili diğer bilgileri alır. |
| [sys.dm_operation_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) |Çoğaltma bağlantılarının durumunu ve dahil olmak üzere tüm veritabanı işlemlerinin durumunu gösterir. |
| [sp_wait_for_database_copy_sync](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) |tüm kaydedilmiş işlemleri çoğaltılır ve etkin ikincil veritabanı tarafından onaylanır kadar beklenecek uygulamanın neden olur. |
|  | |

### <a name="powershell-manage-failover-of-single-and-pooled-databases"></a>PowerShell: Yük devretme işlemlerini tek ve havuza alınmış veritabanlarını yönetme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure Resource Manager PowerShell modülü, Azure SQL veritabanı tarafından hala desteklenmektedir, ancak tüm gelecekteki geliştirme için Az.Sql modüldür. Bu cmdlet'ler için bkz. [Azurerm.SQL'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az modül ve AzureRm modülleri komutları için bağımsız değişkenler büyük ölçüde aynıdır.

| Cmdlet | Açıklama |
| --- | --- |
| [Get-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabase) |Bir veya daha fazla veritabanını alır. |
| [Yeni AzSqlDatabaseSecondary](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabasesecondary) |Mevcut bir veritabanı için ikincil bir veritabanı oluşturur ve veri çoğaltmaya başlar. |
| [Set-AzSqlDatabaseSecondary](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabasesecondary) |Yük devretmeyi başlatmak için ikincil bir veritabanını birincil olarak değiştirir. |
| [Remove-AzSqlDatabaseSecondary](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqldatabasesecondary) |Bir SQL Veritabanı ile belirtilen ikincil veritabanı arasında veri çoğaltmayı sonlandırır. |
| [Get-AzSqlDatabaseReplicationLink](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasereplicationlink) |Bir Azure SQL Veritabanı ve bir kaynak grubu veya SQL Server arasındaki coğrafi çoğaltma bağlantılarını alır. |
|  | |

> [!IMPORTANT]
> Örnek betikler için bkz: [yapılandırın ve yük devretme etkin coğrafi çoğaltmayı kullanarak tek veritabanı](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) ve [yapılandırın ve yük devretme etkin coğrafi çoğaltmayı kullanarak havuza alınmış bir veritabanı](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md).

### <a name="rest-api-manage-failover-of-single-and-pooled-databases"></a>REST API: Yük devretme işlemlerini tek ve havuza alınmış veritabanlarını yönetme

| API | Açıklama |
| --- | --- |
| [Oluşturma veya güncelleştirme veritabanı (createMode geri yükleme =)](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) |Oluşturur, güncelleştirir veya bir birincil veya ikincil bir veritabanını geri yükler. |
| [Alma oluşturma veya güncelleştirme veritabanı durumu](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) |Durum oluşturma işlemi sırasında döndürür. |
| [İkincil veritabanı birincil (Planlı yük devretme) olarak ayarlayın.](https://docs.microsoft.com/rest/api/sql/replicationlinks/failover) |Hangi ikincil veritabanına yük devretme geçerli birincil veritabanından tarafından birincil ayarlar. **Bu seçeneği, yönetilen örneği için desteklenmiyor.**|
| [İkincil veritabanı birincil (planlanmamış yük devretme) olarak ayarlayın.](https://docs.microsoft.com/rest/api/sql/replicationlinks/failoverallowdataloss) |Hangi ikincil veritabanına yük devretme geçerli birincil veritabanından tarafından birincil ayarlar. Bu işlem veri kaybına neden. **Bu seçeneği, yönetilen örneği için desteklenmiyor.**|
| [Çoğaltma bağlantısı alın](https://docs.microsoft.com/rest/api/sql/replicationlinks/get) |Belirli bir çoğaltma bağlantısı belirli bir SQL veritabanı'nda bir coğrafi çoğaltma ortaklığı alır. Bu sys.geo_replication_links katalog görünümünde görünür bilgilerini alır. **Bu seçeneği, yönetilen örneği için desteklenmiyor.**|
| [Çoğaltma bağlantılarını - veritabanı listesi](https://docs.microsoft.com/rest/api/sql/replicationlinks/listbydatabase) | Belirli bir SQL veritabanı'nda bir coğrafi çoğaltma ortaklığı tüm çoğaltma bağlantılarını alır. Bu sys.geo_replication_links katalog görünümünde görünür bilgilerini alır. |
| [Çoğaltma bağlantısı Sil](https://docs.microsoft.com/rest/api/sql/replicationlinks/delete) | Bir veritabanı çoğaltma bağlantısını siler. Yük devretme sırasında yapılamaz. |
|  | |

## <a name="next-steps"></a>Sonraki adımlar

- Örnek betikler için bkz:
  - [Etkin coğrafi çoğaltmayı kullanarak tek bir veritabanını yapılandırma ve tek bir veritabanının yükünü devretme](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
  - [Etkin coğrafi çoğaltmayı kullanarak havuza alınan veritabanını yapılandırma ve havuza alınmış veritabanının yükünü devretme](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
- SQL veritabanı, otomatik yük devretme grupları da destekler. Daha fazla bilgi için bkz [otomatik yük devretme grupları](sql-database-auto-failover-group.md).
- İş sürekliliğine genel bakış ve senaryolar için bkz: [iş sürekliliğine genel bakış](sql-database-business-continuity.md)
- Veritabanı otomatik yedeklemeler Azure SQL hakkında bilgi edinmek için bkz: [SQL veritabanı otomatik yedeklerinde](sql-database-automated-backups.md).
- Otomatik yedekleme, kurtarma için kullanma hakkında bilgi edinmek için [hizmet tarafından başlatılan yedeklemelerden veritabanını geri yükleme](sql-database-recovery-using-backups.md).
- Yeni birincil sunucu ve veritabanı için kimlik doğrulama gereksinimleri hakkında bilgi edinmek için bkz. [olağanüstü durum kurtarma sonrasında SQL veritabanı güvenlik](sql-database-geo-replication-security-config.md).
