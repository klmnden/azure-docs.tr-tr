---
title: Yük devretme grupları ve etkin coğrafi çoğaltma - Azure SQL veritabanı | Microsoft Docs
description: Etkin coğrafi çoğaltma otomatik yük devretme grupları kullanın ve kesinti olması durumunda otomatik yük devretme etkinleştirin.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 12/03/2018
ms.openlocfilehash: de439683082909e65d285a7946a71eb781287937
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52843487"
---
# <a name="overview-active-geo-replication-and-auto-failover-groups"></a>Genel Bakış: Etkin coğrafi çoğaltma ve otomatik yük devretme grupları

Etkin coğrafi çoğaltma, okunabilir çoğaltma veritabanlarını tek tek aynı veya farklı bir veri merkezi (bölge) oluşturmanıza olanak sağlar, Azure SQL veritabanı özelliğidir.

![Coğrafi çoğaltma](./media/sql-database-geo-replication-failover-portal/geo-replication.png )

Etkin coğrafi çoğaltma, tek veritabanlarını bölgesel disastor veya büyük ölçekli kesinti durumunda hızlı bir olağanüstü durum kurtarma gerçekleştirmek uygulama izin veren bir iş sürekliliği çözümü olarak tasarlanmıştır. Coğrafi çoğaltma etkinleştirildiğinde, uygulama farklı bir Azure bölgesinde bir ikincil veritabanına yük devretme başlatabilirsiniz. En fazla dört ikincil veritabanı aynı veya farklı bölgelerde desteklenir ve ikincil veritabanı salt okunur erişim sorgular için de kullanılabilir. Yük devretmeyi el ile uygulama veya kullanıcı tarafından başlatılması gerekir. Yük devretme işleminden sonra yeni birincil farklı bir bağlantı uç noktası vardır.

> [!NOTE]
> Etkin coğrafi çoğaltma, tüm veritabanları, tüm bölgelerde tüm hizmet katmanlarında kullanılabilir.
> Etkin coğrafi çoğaltma, bir yönetilen örnek içinde oluşturulan veritabanları için desteklenmiyor. Kullanmayı [yük devretme grupları](#auto-failover-group-capabilities) yönetilen örneğine sahip.

Otomatik Yük devretme grupları etkin coğrafi çoğaltma, bir uzantısıdır. Yük devretme aynı anda uygulama tarafından başlatılan bir yük devretme kullanan birden çok coğrafi çoğaltmalı veritabanı veya kullanıcı tanımlı bir ölçütleri temel alarak SQL veritabanı hizmeti tarafından gerçekleştirilmesi için yük devretme için temsilci seçme ile yönetmek için tasarlanmıştır. İkincisi, otomatik olarak geri dönülemez bir arıza ya da birincil bölgedeki SQL veritabanı hizmetinizin kullanılabilirliğini tam veya kısmi kaybı ile sonuçlanır diğer planlanmamış bir olay sonra ikincil bir bölgede birden çok ilişkili veritabanlarını kurtarmanıza olanak tanır. Ayrıca, okunabilir ikincil veritabanı salt okunur sorgu iş yükleri yük boşaltması için kullanabilirsiniz. Otomatik Yük devretme grupları, birden çok veritabanı içerdiğinden, bu veritabanları birincil sunucuda yapılandırılması gerekir. Yük devretme grubundaki veritabanları için birincil ve ikincil sunucular, aynı abonelikte olmalıdır. Otomatik Yük devretme grupları için farklı bir bölgedeki tek bir ikincil sunucu grubundaki tüm veritabanlarının çoğaltma destekler.

> [!NOTE]
> Etkin coğrafi çoğaltma, birden fazla ikincil veritabanı gerekiyorsa kullanın.
> [!IMPORTANT]
> Otomatik Yük devretme desteği yönetilen örnekler için genel Önizleme aşamasındadır.

Etkin coğrafi çoğaltma kullanıyorsanız ve için birincil veritabanı başarısız neden veya çevrimdışı olarak kullanılabileceğini gerekiyor, yük devretme ikincil veritabanlarınızı birini başlatabilirsiniz. Yük devretme ikincil veritabanlarından biri etkinleştirildiğinde, diğer tüm ikincil veritabanı otomatik olarak yeni birincil siteye bağlanır. Veritabanı kurtarma ve herhangi bir kesinti yönetmek için otomatik yük devretme gruplarını kullanıyorsanız, bir veya birkaç veritabanları, otomatik yük devretme grubu sonuçları etkiler. Uygulamanızın gereksinimlerine göre en iyi karşılayan otomatik yük devretme İlkesi yapılandırıp yapılandıramayacağını veya geri çevirmek ve el ile etkinleştirme kullanın. Ayrıca, otomatik yük devretme grupları okuma-yazma sağlar ve kalan salt okuma dinleyici uç noktalarının yük devretme sırasında açısından bir farklılık göstermez. El ile veya otomatik yük devretme etkinleştirme kullanmanıza bakılmaksızın, yük devretme grubunda tüm ikincil veritabanlarını birincil geçer. Veritabanı yük devretme tamamlandıktan sonra yeni bölge için uç noktalarının yönlendirmek için DNS kaydını otomatik olarak güncelleştirilir.

Çoğaltma ve yük devretme işlemlerini tek bir veritabanı veya bir sunucuda veya etkin coğrafi çoğaltmayı kullanarak elastik bir havuzdaki veritabanları kümesi yönetebilirsiniz. Bu kullanarak bunu yapabilirsiniz:

- [Azure portalı](sql-database-geo-replication-portal.md)
- [PowerShell: Tek veritabanı](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
- [PowerShell: Elastik havuz](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
- [PowerShell: Yük devretme grubu](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
- [Transact-SQL: Tek bir veritabanı veya elastik havuz](/sql/t-sql/statements/alter-database-azure-sql-database)
- [REST API: Tek veritabanı](https://docs.microsoft.com/rest/api/sql/replicationlinks)
- [REST API: Yük devretme grubu](https://docs.microsoft.com/rest/api/sql/failovergroups).

Yük devretme işleminden sonra yeni birincil sunucu ve veritabanı için kimlik doğrulama gereksinimleri yapılandırıldığından emin olun. Ayrıntılar için bkz [olağanüstü durum kurtarma sonrasında SQL veritabanı güvenlik](sql-database-geo-replication-security-config.md). Bu, hem de etkin coğrafi çoğaltma ve otomatik yük devretme grupları geçerlidir.

Etkin coğrafi çoğaltma yararlanır [Always On](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) zaman uyumsuz olarak kaydedilen işlem sayısı birincil veritabanında anlık görüntü yalıtımı kullanarak ikincil bir veritabanı çoğaltma için SQL Server teknolojisi. Otomatik Yük devretme grupları üzerinde etkin coğrafi çoğaltma grubu semantiği sağlar ancak aynı zaman uyumsuz çoğaltma mekanizması kullanılır. Herhangi bir anda ederken, ikincil veritabanı birincil veritabanının gerisinde biraz olabilir, ikincil veri kısmi hareket hiçbir zaman olması garanti edilir. Bölgeler arası yedeklilik uygulamaları hızlı bir şekilde veri merkezinin tamamı veya doğal afetler, geri dönülemez bir insan hatalarına veya kötü amaçlı davranışlara neden bir veri merkezinin bölümleri kalıcı kaybı kurtarmanıza olanak sağlar. Belirli RPO verileri şu yolda bulunabilir: [iş Sürekliliğine genel bakış](sql-database-business-continuity.md).

Aşağıdaki şekilde Orta Güney ABD bölgesinde yapılandırılmış Kuzey Orta ABD bölgesinde birincil ve ikincil etkin coğrafi çoğaltma örneği gösterilmektedir.

![coğrafi çoğaltma ilişkisi](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

Okunabilir ikincil veritabanları olduğundan, raporlama işleri gibi salt okunur iş yüklerini boşaltmak için kullanılabilir. Etkin coğrafi çoğaltma kullanıyorsanız, aynı bölgede birincil ile ikincil veritabanı oluşturmak mümkündür, ancak uygulamanın esnekliği yıkıcı hatalarına artırmaz. Otomatik Yük devretme gruplarını kullanıyorsanız ikincil veritabanınıza her zaman farklı bir bölgede oluşturulur.

Ek olarak olağanüstü durum kurtarma etkin coğrafi çoğaltma, aşağıdaki senaryolarda kullanılabilir:

- **Veritabanı geçişi**: etkin coğrafi çoğaltma, bir veritabanı için en düşük kapalı kalma süresi ile çevrimiçi başka bir sunucudan geçirmek için kullanabilirsiniz.
- **Uygulama yükseltmeleri**: uygulama yükseltmeleri sırasında bir hata geri kopya olarak ek bir ikincil oluşturabilirsiniz.

Gerçek iş sürekliliği elde etmek için veri merkezleri arasında veritabanı yedeklilik ekleyerek yalnızca çözümün parçasıdır. Bir arıza kurtarma hizmeti oluşturan tüm bileşenlerini ve tüm bağımlı hizmetlerin gerektirir sonra bir uygulama (hizmet) için uçtan uca kurtarma. İstemci yazılımını (örneğin, tarayıcı özel bir JavaScript ile), web ön uçları, depolama ve DNS bu bileşenlerin örnekleridir. Tüm bileşenlerin aynı hatalara karşı dayanıklı ve kurtarma süresi hedefi, uygulamanızın içinde (RTO) kullanılabilir hale önemlidir. Bu nedenle, tüm bağımlı hizmetleri tanımlayın ve yetenekleri sağlarlar ve garanti anlamak gerekir. Ardından, bağımlı olduğu hizmetlerin yük devretme sırasında hizmetinizin emin olmak için yeterli adımları atmanız gerekir. Olağanüstü durum kurtarma çözümleri tasarlama hakkında daha fazla bilgi için bkz. [olağanüstü durum kurtarma'yı kullanarak etkin coğrafi çoğaltma için bulut çözümleri tasarlama](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="active-geo-replication-capabilities"></a>Etkin coğrafi çoğaltma özellikleri

Etkin coğrafi çoğaltma özelliğini aşağıdaki temel özellikleri sağlar:

- **Otomatik zaman uyumsuz çoğaltma**

 Mevcut bir veritabanına ekleyerek yalnızca ikincil bir veritabanı oluşturabilirsiniz. Herhangi bir Azure SQL veritabanı sunucusu, ikincil oluşturulabilir. Oluşturulduktan sonra ikincil veritabanı birincil veritabanından kopyalanan verileri doldurulur. Bu işlem, dengeli dağıtım olarak bilinir. İkincil veritabanı oluşturduktan ve çekirdek değeri oluşturulmuş sonra birincil veritabanına güncelleştirmeleri zaman uyumsuz olarak ikincil veritabanına otomatik olarak çoğaltılır. Zaman uyumsuz çoğaltma, birincil veritabanında işlemleri yapıldığından ikincil veritabanına çoğaltılırken anlamına gelir.

- **Okunabilir ikincil veritabanı**

  Bir uygulama, ikincil bir veritabanını birincil veritabanına erişmek için kullanılan aynı veya farklı güvenlik prensiplerinin salt okunur işlemler için erişebilirsiniz. Güncelleştirmeler (günlük yeniden yürütme) birincil çoğaltması ikincil yürütülen sorgular tarafından gecikiyor değil emin olmak için anlık görüntü yalıtım modunda ikincil veritabanlarıyla çalışır.

  > [!NOTE]
  > Birincil şema güncelleştirmeleri varsa, günlük yeniden yürütme ikincil veritabanı üzerinde gecikir. İkincisi, ikincil veritabanında bir şema kilidi gerektirir.

- **Birden çok okunabilir ikincil veritabanı**

  İki veya daha fazla ikincil veritabanı yedeklilik ve birincil veritabanı ve uygulama koruma düzeyini artırın. Birden fazla ikincil veritabanı yoksa, ikincil veritabanlarından biri başarısız olsa bile uygulama korumalı olarak kalır. Yeni bir ikincil veritabanı oluşturulana kadar uygulamayı yalnızca bir ikincil veritabanı yoktur ve yine başarısız olursanız, daha yüksek risklere açıktır.

  > [!NOTE]
  > Global olarak dağıtılmış bir uygulama oluşturun ve dörtten fazla bölgede veri salt okunur erişim sağlamak etkin coğrafi çoğaltma kullanıyorsanız ikincil bir ikincil (zincirleme olarak bilinen bir işlem) oluşturabilirsiniz. Bu şekilde veritabanı çoğaltma neredeyse sınırsız ölçek elde edebilirsiniz. Ayrıca, zincirleme birincil veritabanı çoğaltma ek yükü azaltır. Yaprak en ikincil veritabanları üzerinde artan çoğaltma gecikmesi dengedir.

- **Elastik havuz veritabanları desteği**

  Her yineleme, bir elastik havuzda ayrı olarak katılan ya da herhangi bir elastik havuzda hiç olmaması. Her çoğaltma için havuz seçeneği ayrıdır ve herhangi bir çoğaltma yapılandırması sırasında bağımlı değildir (birincil veya ikincil olup olmadığını). Her esnek havuzun tek bir bölgede yer alan, bu nedenle lrs'de aynı topolojiyi içinde bir elastik havuz hiçbir zaman paylaşabilirsiniz.

- **İkincil veritabanının yapılandırılabilir işlem boyutu**

  Birincil ve ikincil veritabanları aynı hizmet katmanı için gereklidir. Aynı işlem boyutu (Dtu veya sanal çekirdekler) olarak birincil ile ikincil veritabanı oluşturulan de önemle tavsiye edilir. İkincil bir alt işlem boyutu ile olası olmadığından, ikincil bir artan çoğaltma gecikmesi'nin süresi, risk altındadır ve sonuç olarak bir yük devretme sonrasında önemli veri kaybı riski. Sonuç olarak, yayımlanan RPO = 5 sn olamaz garanti edilir. Diğer sorununa neden daha yüksek bir işlem boyutu için yükseltilene kadar yük devretmeden sonra uygulama performansını işlem kapasitesi yeni birincil eksikliği nedeniyle etkilenecek emin olur. Yükseltme süresini veritabanı boyutuna bağlıdır. Ayrıca, şu anda bu tür yükseltme birincil ve ikincil veritabanları çevrimiçi olduğundan ve bu nedenle, kesinti giderildikten kadar tamamlanamıyor, gerektirir. Alt işlem boyutu ile ikincil oluşturmaya karar verirseniz, Azure portalında günlük g/ç yüzdesi grafik ikincil çoğaltma yükü sürdürebilmek için gereken en düşük işlem boyutunu tahmin etmek için iyi bir yol sağlar. Örneğin, birincil veritabanınız P6 ise (1000 DTU) ve kendi günlük g/ç yüzdesi 50 ikincil en az olması gerekir % P4 (500 DTU). Kullanarak günlük GÇ veri de alabilirsiniz [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) veya [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) veritabanı görünümleri.  SQL veritabanı işlem boyutları hakkında daha fazla bilgi için bkz. [SQL veritabanı hizmet katmanları nelerdir](sql-database-service-tiers.md).

- **Kullanıcı tarafından denetlenen bir yük devretme ve yeniden çalışma**

  İkincil bir veritabanı açıkça birincil role herhangi bir zamanda uygulama veya kullanıcı tarafından değiştirilebilir. Gerçek bir kesinti sırasında "planlanmamış" seçeneği, hangi hemen ikincil yükseltir birincil olarak kullanılmalıdır. Başarısız birincil kurtarır ve yeniden kullanılabilir olduğunda, sistem otomatik olarak kurtarılan birincil ikincil olarak işaretler ve yeni birincil ile güncel getirin. Birincil ikincil en son değişiklikleri çoğaltır önce başarısız olursa zaman uyumsuz çoğaltma yapısı nedeniyle, az miktarda veriniz planlanmamış yük devretme sırasında kaybolur. Birden çok ikincilleriyle birincil yük devrettiğinde, sistem otomatik olarak çoğaltma ilişkilerini yeniden yapılandırır ve kalan ikincil veritabanı herhangi bir kullanıcı müdahalesi gerektirmeden yeni yükseltilen birincil siteye bağlar. Yük devretme neden oldu kesinti giderildikten sonra uygulamayı birincil bölgeye geri istenebilir. Bunu yapmak için yük devretme komut "planlanmış" seçeneği ile çağrılmalıdır.

- **Kimlik bilgileri ve güvenlik duvarı kuralları eşitlenmiş durumda tutma**

  Kullanmanızı öneririz [veritabanı güvenlik duvarı kuralları](sql-database-firewall-configure.md) bu kurallar, tüm ikincil veritabanlarını birincil olarak aynı güvenlik duvarı kuralları olduğundan emin olmak için veritabanı kullanılarak çoğaltılabilir. Bu nedenle coğrafi çoğaltmalı veritabanı. Bu yaklaşım el ile yapılandırmak ve hem birincil ve ikincil veritabanlarını barındıran sunucu güvenlik duvarı kurallarını sağlamak müşterilerin ihtiyacını ortadan kaldırır. Benzer şekilde, kullanarak [kapsanan veritabanı kullanıcıları](sql-database-manage-logins.md) veriler için birincil ve ikincil veritabanları her zaman sahip aynı erişim sağlar. kullanıcı kimlik bilgilerini bir yük devretme sırasında bu yüzden hiçbir kesinti nedeniyle oturum açma ve parolaları ile uyuşmuyor. Ek olarak [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md), müşteriler, hem birincil hem de ikincil veritabanları için kullanıcı erişimini yönetebilir ve yönetme gereksinimini ortadan kimlik bilgileri veritabanlarında tamamen.

## <a name="auto-failover-group-capabilities"></a>Otomatik Yük devretme grubu özellikleri

Otomatik Yük devretme grupları özelliği, grubu düzeyi çoğaltma ve otomatik yük devretme destekleyerek etkin coğrafi çoğaltma, güçlü bir Özet sağlar. Ayrıca, ek bir dinleyici uç noktalarının sağlayarak SQL bağlantı dizesi yük devretmeden sonra değiştirmek için yeterlilikte kaldırır.

> [!IMPORTANT]
> Örnekleri destek otomatik yük devretme grupları bir genel Önizleme özellikleri yönetiliyor. Bkz: [en iyi uygulamalar, yönetilen örnekler ile yük devretme grupları kullanma](#best-practices-of-using-failover-groups-with-managed-instances)

- **Yük devretme grubu**

  Bir veya daha çok yük devretme grupları, farklı bölgelerde (birincil ve ikincil sunucular) iki sunucu arasında oluşturulabilir. Her grup, tüm veya bazı birincil veritabanı birincil bölgede kesinti nedeniyle kullanılamıyor, bir birim olarak kurtarılan bir veya birden çok veritabanı içerebilir.  

- **Birincil sunucu**

  Yük devretme grubunda birincil veritabanlarını barındıran sunucu.

- **İkincil sunucu**

  Yük devretme grubundaki ikincil veritabanlarını barındıran sunucu. İkincil sunucu, birincil sunucu ile aynı bölgede olamaz.

- **Veritabanları yük devretme grubuna ekleme**

  Elastik havuz veya bir sunucu içinde birden fazla veritabanı aynı yük devretme grubuna koyabilirsiniz. Tek bir veritabanı grubuna eklerseniz, otomatik olarak aynı sürümü ve işlem boyutu kullanarak ikincil bir veritabanı oluşturur. Birincil veritabanını bir elastik havuzda, aynı ada sahip bir elastik havuzdaki ikincil otomatik olarak oluşturulur. İkincil bir veritabanı ikincil sunucuya zaten olan bir veritabanının eklerseniz, söz konusu coğrafi çoğaltma grubu tarafından devralınır.

  > [!NOTE]
  > İkincil veritabanına yük devretme grubunun parçası olmayan bir Server'da zaten bir veritabanı eklerken, yeni bir ikincil ikincil sunucuya oluşturulur.
  > [!IMPORTANT]
  > Bir yönetilen örnek, tüm kullanıcı veritabanlarına çoğaltılır. Kullanıcı veritabanı çoğaltması için bir alt kümesi yük devretme grubunda seçemezsiniz.

- **Yük devretme grubu okuma / yazma dinleyici**

  Bir DNS CNAME kaydı olarak biçimlendirilmiş  **&lt;yük devretme grubu adı&gt;. database.windows.net** işaret eden geçerli birincil sunucu URL'si. Yük devretme sonrasında birincil değiştiğinde şeffaf bir şekilde birincil veritabanına bağlanmak okuma-yazma SQL uygulamalar sağlar.

- **Yük devretme grubu salt okunur dinleyicisi**

  Bir DNS CNAME kaydı olarak biçimlendirilmiş  **&lt;yük devretme grubu adı&gt;. secondary.database.windows.net** işaret eden ikincil sunucu URL'si. Belirtilen yük dengeleyici kurallarını kullanarak ikincil bir veritabanı için saydam bir şekilde bağlanmak salt okunur SQL uygulamalar sağlar.

  > [!IMPORTANT]
  > Yönetilen örneği'nde yük devretme grubu oluşturulduğunda, DNS CNAME kayıtları dinleyiciler için **&lt;yük devretme grubu adı&gt;.&lt; zone_id&gt;. database.windows.net ve &lt;yük devretme grubu adı&gt;.secondary.&lt; zone_id&gt;. database.windows.net. Bkz: [yönetilen örnekleri ile yük devretme grupları için en iyi yöntemler](#best-practices-of-using-failover-groups-for-business-continuity-with-managed-instances) zone_id yönetilen örneği ile kullanma ayrıntılı bilgi için.

- **Otomatik Yük devretme İlkesi**

  Varsayılan olarak, yük devretme grubuna bir otomatik yük devretme İlkesi ile yapılandırılır. Sistem hatası algıladı ve yetkisiz kullanım süresi dolmuştur sonra yük devretmeyi tetikler. Sistem, kesinti yerleşik yüksek kullanılabilirlik altyapısı ölçek etkisi nedeniyle SQL veritabanı hizmeti tarafından azaltılamaz doğrulamanız gerekir. Uygulama yük devretme iş akışını denetlemek istiyorsanız, otomatik yük devretme kapatabilirsiniz.

- **Salt okunur bir yük devretme İlkesi**

  Varsayılan olarak, yük devretme işlemlerini salt okuma dinleyici devre dışıdır. İkincil çevrimdışı olduğunda birincil performansını etkilenmez sağlar. Ancak, bu da salt okunur oturumları ikincil kurtarılmadan kadar bağlanmak mümkün olmayacaktır anlamına gelir. Kapalı kalma süresi için salt okunur oturumları tolere olamaz ve olası performans düşüşü birincil çoğaltamaz hem salt okunur ve okuma-yazma trafiği geçici olarak birincil kullanılmak üzere Tamam, salt okunur dinleyici için yük devretme etkinleştirebilirsiniz. Bu durumda ikincil sunucu kullanılabilir değilse, salt okunur trafiği otomatik olarak birincil sunucuya yönlendirilecek.  

- **El ile yük devretme**

  Otomatik Yük devretme yapılandırması bağımsız olarak herhangi bir zamanda yük devretme el ile başlatabilir. Otomatik Yük devretme İlkesi yapılandırılmamışsa, el ile yük devretme yük devretme grubundaki veritabanlarını kurtarmak için gereklidir. Zorlanmış veya kolay yük devretme (tam veri eşitlemesi ile) başlatabilir. İkincisi etkin sunucu birincil bölgeye yeniden konumlandırmak için kullanılabilir. Yük devretme tamamlandığında, doğru sunucuyla bağlantı sağlamak için DNS kayıtlarını otomatik olarak güncelleştirilir.

- **Veri kaybı yetkisiz kullanım süresi**

  Zaman uyumsuz çoğaltması kullanılarak birincil ve ikincil veritabanları eşitlenmiş olduğundan, yük devretme veri kaybına neden olabilir. Otomatik Yük devretme İlkesi, veri kaybı toleransını uygulamanızın yansıtacak şekilde özelleştirebilirsiniz. Yapılandırarak **GracePeriodWithDataLossHours**, sistem için sonuç veri kaybı olasılığı yüksek olan yük devretmeyi başlatmadan önce bekleyeceği süreyi denetleyebilirsiniz.

  > [!NOTE]
  > Sistem algıladığında grubundaki veritabanları hala çevrimiçi olduğu (örneğin, kesinti yalnızca hizmet denetim düzlemi etkilenen), tarafındanayarlanandeğerebakılmaksızınhementamverieşitlemesi(kolayyükdevretme)ileyükdevretmeyietkinleştirir **GracePeriodWithDataLossHours**. Bu davranış, Kurtarma sırasında veri kaybı olmadan olmasını sağlar. Yalnızca kolay bir yük devretme mümkün olmadığında yetkisiz kullanım süresi geçerli olur. Yetkisiz kullanım süresi dolmadan önce kesinti giderildikten, yük devretme etkinleştirilmedi.

- **Birden çok yük devretme grupları**

  Yük devretmeleri ölçeğini denetlemek için birden çok yük devretme grupları sunucuları aynı çifti için yapılandırabilirsiniz. Her grup bağımsız olarak yük devreder. Elastik havuzlar, çok kiracılı bir uygulama kullanıyorsa, birincil ve ikincil veritabanlarının havuzdaki her karıştırmak için bu özelliği kullanabilirsiniz. Bu şekilde kesinti etkisini kiracılar yalnızca yarısını için azaltabilir.

  > [!IMPORTANT]
  > Yönetilen örnek, birden çok yük devretme grupları desteklemez.

## <a name="best-practices-of-using-failover-groups-with-single-and-pooled-databases"></a>Yük devretme grupları, tek ve havuza alınmış veritabanlarıyla kullanmanın en iyi uygulamalar

İş sürekliliği aklınızda ile bir hizmet tasarlarken aşağıdaki genel yönergeleri izleyin:

- **Yük devretme işlemlerini birden fazla veritabanını yönetmek için bir veya birden çok yük devretme grupları kullanma**

  Bir veya daha çok yük devretme grupları, farklı bölgelerde (birincil ve ikincil sunucular) iki sunucu arasında oluşturulabilir. Her grup, tüm veya bazı birincil veritabanı birincil bölgede kesinti nedeniyle kullanılamıyor, bir birim olarak kurtarılan bir veya birden çok veritabanı içerebilir. Yük devretme grubu, birincil olarak aynı hizmet hedefi ile coğrafi-ikincil veritabanı oluşturur. Mevcut bir coğrafi çoğaltma ilişkisi için yük devretme grubuna eklerseniz, coğrafi-ikincil aynı hizmet katmanına ve işlem boyutu birincil olarak yapılandırıldığından emin olun.

- **OLTP iş yükü için okuma / yazma dinleyici kullanın**

  OLTP işlemleri gerçekleştirirken kullanmak  **&lt;yük devretme grubu adı&gt;. database.windows.net** sunucunun URL'sini ve bağlantı otomatik olarak birincil siteye yönlendirilir. Bu URL, yük devretme sonrasında değiştirmez. Not yük devretme, yalnızca istemci DNS önbelleği yenilendiğini sonra istemci bağlantıları yeni birincil siteye yönlendirilir DNS kaydı güncelleştirmeniz gerekir.

- **Salt okunur iş yükü için salt okunur bir dinleyici kullanın**

  Verilerin belirli eskime dayanıklı olan, mantıksal olarak yalıtılmış bir salt okunur yükünü varsa, ikincil veritabanı uygulamada kullanabilirsiniz. Salt okunur oturumları kullanmanızın  **&lt;yük devretme grubu adı&gt;. secondary.database.windows.net** sunucusu olarak URL ve bağlantı otomatik olarak yönlendirilir ikincil. Ayrıca bağlantı dizesi kullanarak hedefi okuma belirtmek önerilir **ApplicationIntent salt okunur =**.

- **Performans düşüşü için hazırlıklı olmalıdır**

  SQL yük devretme karar, uygulamanın veya kullanılan diğer hizmetlerin geri kalanından bağımsızdır. Uygulama "tek bir bölge ve bazı durumlarda başka bazı bileşenleri ile karışık". Performans düşüşü önlemek için DR bölgesinde yedekli uygulama dağıtımı emin olun ve aşağıdaki adımları [güvenlik yönergeleri ağ](#failover-groups-and-network-security).

  > [!NOTE]
  > Farklı bağlantı dizesini kullanmak DR bölgesinde uygulama yok.  

- **Veri kaybı için hazırlama**

  Kesinti algılanırsa, SQL tarafından belirtilen süre boyunca bekler **GracePeriodWithDataLossHours**. Varsayılan değer 1 saattir. Veri kaybını göze alamayacağı, ayarladığınızdan emin olun **GracePeriodWithDataLossHours** 24 saat gibi yeterince büyük bir sayı. Birincil siteden ikincil bölgeden başarısız için el ile grubu yük devretme kullanın.

> [!IMPORTANT]
> 800 ya da daha az dtu'ları ve coğrafi çoğaltmayı kullanarak 250'den fazla veritabanları ile elastik havuzları artık planlanan yük devretmeler sorunlarla karşılaşabilirsiniz ve performansı düştü.  Bu sorunları yazma yoğunluklu iş yükleri için coğrafi çoğaltma uç noktaları coğrafyaya göre değişim yaygın olarak ayrılıyorsa ya da her veritabanı için birden çok ikincil uç noktaları kullanıldığında ortaya daha yüksektir.  Coğrafi çoğaltma gecikmesi zamanla artar, bu sorunları belirtileri gösterilir.  Bu gecikme kullanılarak izlenebilir [sys.dm_geo_replication_link_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database).  Bu sorun oluşursa, risk azaltma işlemleri havuz Dtu sayısını artırmak veya coğrafi olarak çoğaltılmış veritabanlarının aynı havuzdaki sayısını azaltmayı içerir.

## <a name="best-practices-of-using-failover-groups-with-managed-instances"></a>Yönetilen örnekler ile yük devretme grupları kullanmanın en iyi uygulamalar

Uygulamanız, yönetilen örnek, veri katmanı olarak kullanıyorsa, iş sürekliliği için tasarlarken şu genel yönergeleri izleyin:

- **İkincil örneği birincil örnekle aynı DNS bölgesi oluşturma**

  Yeni bir örneği oluşturulduğunda, benzersiz bir kimliği otomatik olarak oluşturulan DNS bölgesi ve örnek DNS adını dahil. Bu örnek biçiminde SAN alanı sağlandığında için çoklu etki alanı (SAN) sertifika &lt;zone_id&gt;. database.windows.net. Bu sertifika, aynı DNS bölgesinde örneğine istemci bağlantılarının kimliğini doğrulamak için kullanılabilir. Birincil örneğe yönelik bağlantının kesintiye yük devretmenin ardından birincil ve ikincil emin olmak için örnekleri aynı DNS bölgesinde olmalıdır. Uygulamanızı Üretim dağıtımı için hazır olduğunda, ikincil bir örneği farklı bir bölgede oluşturun ve DNS bölgesini birincil örneğiyle paylaştığı emin olun. Bu belirtilerek yapılır bir `DNS Zone Partner` isteğe bağlı parametresi `create instance` PowerShell komutu.

- **İki örnek arasındaki çoğaltma trafiğinin etkinleştir**

  Her örnek kendi VNET'ine izole edilmiş olduğundan, bu sanal ağlar arasında iki yönlü trafik izin verilmesi gerekir. Bkz: [çoğaltma ile SQL veritabanı yönetilen örneği](replication-with-sql-database-managed-instance.md).

- **Bir yük devretme grubu yük devretme tüm örneğinin yönetmek için yapılandırma**

  Yük devretme grubu yük devretmesi örnekteki tüm veritabanlarını yönetebilir. Bir grup oluşturulduğunda, her bir örneği veritabanında otomatik olarak coğrafi-ikincil örneğine çoğaltılan olacaktır. Yük devretme grupları, bir alt kümesinin veritabanlarını kısmi bir yük devretmeyi başlatmak için kullanamazsınız.

  > [!IMPORTANT]
  > Bir veritabanını birincil örneğinden kaldırılırsa, bu da otomatik olarak coğrafi ikincil örnekteki bırakılır.

- **OLTP iş yükü için okuma / yazma dinleyici kullanın**

  OLTP işlemleri gerçekleştirirken kullanmak &lt;yük devretme grubu adı&gt;.&lt; zone_id&gt;. database.windows.net sunucu URL'sini ve bağlantı olarak otomatik olarak birincil siteye yönlendirilir. Bu URL, yük devretme sonrasında değiştirmez. Yük devretme, yalnızca istemci DNS önbelleği yenilendiğini sonra istemci bağlantıları yeni birincil siteye yönlendirilir için DNS kaydı güncelleştirmeniz gerekir. İkincil örneği ile birincil DNS bölgesi paylaştığından, istemci uygulaması aynı SAN sertifikayı kullanarak yeniden bağlanmanız mümkün olacaktır.

- **Coğrafi çoğaltmalı ikincil salt okunur sorgular için doğrudan bağlanma**

  Verilerin belirli eskime dayanıklı olan, mantıksal olarak yalıtılmış bir salt okunur yükünü varsa, ikincil veritabanı uygulamada kullanabilirsiniz. Doğrudan coğrafi çoğaltmalı ikincil veritabanına bağlanmak için &lt;sunucu&gt;.secondary.&lt; zone_id&gt;. database.windows.net sunucu URL'sini ve bağlantı olarak doğrudan coğrafi çoğaltmalı ikincil veritabanına yapılır.

  > [!NOTE]
  > Belirli hizmet katmanlarında, Azure SQL veritabanı kullanımını destekler. [salt okunur çoğaltmalar](sql-database-read-scale-out.md) yüklemek ve bir salt okunur çoğaltma kapasitesini kullanmaya Bakiye salt okunur sorgu iş yükleri için `ApplicationIntent=ReadOnly` bağlantı parametresi dize. Coğrafi çoğaltmalı ikincil yapılandırıldığında, ya da salt okunur çoğaltma birincil konumda veya coğrafi olarak çoğaltılmış bir konuma bağlanmak için bu özelliği kullanabilirsiniz.
  > - Salt okunur bir çoğaltması birincil konumda bağlanmak için kullanma &lt;yük devretme grubu adı&gt;.&lt; zone_id&gt;. database.windows.net.
  > - Salt okunur bir çoğaltması birincil konumda bağlanmak için kullanma &lt;yük devretme grubu adı&gt;.secondary.&lt; zone_id&gt;. database.windows.net.
- **Performans düşüşü için hazırlıklı olmalıdır**

  SQL yük devretme karar, uygulamanın veya kullanılan diğer hizmetlerin geri kalanından bağımsızdır. Uygulama "tek bir bölge ve bazı durumlarda başka bazı bileşenleri ile karışık". Performans düşüşü önlemek için DR bölgesinde yedekli uygulama dağıtımını sağlamak ve bu makalede bir ağ güvenlik yönergelerini izlenmesini <link>.

- **Veri kaybı için hazırlama**

  Kesinti algılanırsa, sıfır veri kaybı bizim bilgi en iyi ise SQL otomatik olarak okuma / yazma yük devretmeyi tetikler. Aksi takdirde GracePeriodWithDataLossHours tarafından belirtilen süre boyunca bekler. GracePeriodWithDataLossHours belirtilmişse, veri kaybı için hazırlıklı olmalıdır. Genel olarak, kesintiler sırasında Azure kullanılabilirlik ayrıcalık tanır. Veri kaybını göze alamayacağı, 24 saat gibi yeterince büyük bir sayı GracePeriodWithDataLossHours ayarladığınızdan emin olun.

> [!IMPORTANT]
> El ile grubu yük devretme seçimlerine özgün konuma geri taşımak için kullanın: yük devretme neden oldu kesinti giderildikten sonra birincil veritabanlarınızı özgün konuma taşıyabilirsiniz. Bunu yapmak için el ile yük devretme grubunun başlatmalıdır.

> [!IMPORTANT]
> DNS güncelleştirme okuma-yazma dinleyici, yük devretme hemen başlatıldıktan sonra gerçekleşir. Bu işlem veri kaybına neden olmaz. Ancak, veritabanı rolleri geçiş işleminin normal koşullar altında 5 dakikaya kadar sürebilir. Tamamlanana kadar yeni birincil örnekteki bazı veritabanları yine de salt okunur olacaktır. PowerShell kullanarak yük devretme başlatılması halinde tüm operasyon uyumludur. Azure portalını kullanarak başlatılmışsa UI tamamlanma durumu gösterir. REST API kullanarak başlatılır, standart ARM'ın yoklama mekanizması tamamlanma durumunu izlemek için kullanın.

## <a name="failover-groups-and-network-security"></a>Yük devretme grupları ve ağ güvenliği

Veri katmanı için ağ erişimi belirli bir bileşeni ya da VM gibi bileşenleri kısıtlanmıştır güvenlik kuralları gerektiren bazı uygulamalar için web hizmeti vb. Bu gereksinim, iş sürekliliği tasarım için bazı zorluklar ve yük devretme grupları kullanımını gösterir. Kısıtlı erişim uygularken aşağıdaki seçenekleri düşünmelisiniz.

### <a name="using-failover-groups-and-virtual-network-rules"></a>Yük devretme grupları ve sanal ağ kuralları kullanma

Kullanıyorsanız [sanal ağ hizmet uç noktaları ve kuralları](sql-database-vnet-service-endpoint-rule-overview.md) SQL veritabanınıza erişimi kısıtlamak için her sanal ağ hizmet uç noktası geçerli tek bir Azure bölgesine olduğunu unutmayın. Uç nokta, alt ağından gelen iletişimi kabul etmek üzere diğer bölgeler etkinleştirmez. Bu nedenle, yalnızca aynı bölgede dağıtılan istemci uygulamaları birincil veritabanına bağlanabilirsiniz. Yük devretme, bir sunucu (ikincil) farklı bir bölgede yönlendirdi SQL istemci oturumlarını sonuçlanır olduğundan, bu oturumların, bölgesinin dışındaki bir istemcinin kaynaklandığı başarısız olur. Sanal ağ kuralları katılan sunucular eklenirse, bu nedenle, otomatik yük devretme İlkesi etkinleştirilemez. El ile yük devretme desteği için aşağıdaki adımları izleyin:

1. Ön uç bileşenleri, uygulamanızın (web hizmeti, sanal makineler vb.) ikincil bölgedeki yedekli kopyalarını sağlama
2. Yapılandırma [sanal ağ kuralları](sql-database-vnet-service-endpoint-rule-overview.md) birincil ve ikincil bir sunucu için ayrı ayrı
3. Etkinleştirme [ön uç yük devretme trafik Yöneticisi yapılandırması kullanma](sql-database-designing-cloud-solutions-for-disaster-recovery.md#scenario-1-using-two-azure-regions-for-business-continuity-with-minimal-downtime)
4. Kesinti algılandığında el ile yük devretme başlatın. Bu seçenek, ön uç ve veri katmanı arasında tutarlı bir gecikme gerektiren uygulamalar için iyileştirilmiştir ve ön uç, veri katmanı veya her ikisi de kesintiden etkilendiğinde kurtarmayı destekler.

> [!NOTE]
> Kullanıyorsanız **salt okuma dinleyici** yük dengelemek için bir salt okunur iş yükü, ikincil veritabanına bağlanabilmesi için bu iş yükünü bir VM veya ikincil bölgedeki diğer kaynak yürütüldüğünden emin olun.

### <a name="using-failover-groups-and-sql-database-firewall-rules"></a>Yük devretme grupları ve SQL veritabanı güvenlik duvarı kuralları kullanma

İş sürekliliği planınızın otomatik yük devretme grupları kullanarak yük devretme gerektiriyorsa, geleneksel güvenlik duvarı kuralları kullanarak SQL veritabanınıza erişimi kısıtlayabilirsiniz.  Otomatik Yük devretme desteği için aşağıdaki adımları izleyin:

1. [Bir genel IP oluşturma](../virtual-network/virtual-network-public-ip-address.md#create-a-public-ip-address)
2. [Bir genel yük dengeleyici oluşturma](../load-balancer/quickstart-create-basic-load-balancer-portal.md#create-a-basic-load-balancer) ve genel IP atayın.
3. [Bir sanal ağ ve sanal makineler oluşturma](../load-balancer/quickstart-create-basic-load-balancer-portal.md#create-back-end-servers) , ön uç bileşenleri
4. [Ağ güvenlik grubu oluşturma](../virtual-network/security-overview.md) ve gelen bağlantıları yapılandırın.
5. Giden bağlantılar 'Sql' kullanarak Azure SQL veritabanı'na açık olduğundan emin olun [hizmet etiketi](../virtual-network/security-overview.md#service-tags).
6. Oluşturma bir [SQL veritabanı güvenlik duvarı kuralı](sql-database-firewall-configure.md) 1. adımda oluşturduğunuz genel IP adresinden gelen trafiğe izin verme.

Hakkında daha fazla bilgi için giden erişimi nasıl yapılandırılacağı ve hangi IP güvenlik duvarı kurallarında kullanmak için bkz: [yük dengeleyici giden bağlantılar](../load-balancer/load-balancer-outbound-connections.md).

Yukarıdaki yapılandırma, otomatik yük devretme ön uç bileşenlerini bağlantılarından engellemez ve uygulama ön ucu ve veri katmanı arasında daha uzun gecikme etkilenmemesini varsayar sağlayacaktır.

> [!IMPORTANT]
> Bölgesel kesintiler için iş sürekliliği sağlamak için hem ön uç bileşenleri hem de veritabanı için coğrafi artıklık emin olmanız gerekir.

## <a name="enabling-replication-between-managed-instances-and-their-vnets"></a>Yönetilen örnekleri ve bunların sanal ağlar arasında çoğaltmayı etkinleştirme

Birincil ve ikincil iki farklı bölgede yönetilen örnekleri arasında bir yük devretme grupları ayarladığınızda, her örnek bağımsız bir sanal ağ kullanarak yalıtılmıştır. Bu sanal ağlar arasındaki trafiği çoğaltmaya izin vermek için aşağıdaki adımları izleyin:

1. Bunları ya da Express Route bağlantı hattı bağlayın (https://docs.microsoft.com/azure/expressroute/expressroute-circuit-peerings) veya bir VPN ağ geçidi (https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
2. Her NSG, bağlantı noktası 5022 ve aralığın 11000 12000 diğer vnet'ten gelen bağlantılar için açık şekilde ayarlayın.

## <a name="upgrading-or-downgrading-a-primary-database"></a>Yükseltme veya bir birincil veritabanı önceki sürüme indirme

Yükseltme veya birincil farklı işlem boyuta (aynı hizmet katmanında, genel amaçlı ve iş açısından kritik arasında değil) tüm ikincil veritabanlarını kesmeden düşürebilirsiniz. Yükseltme sırasında ikincil veritabanı yükseltmeniz ve ardından birincil yükseltmeniz önerilir. Önceki sürüme indirirken ters sırada: birincil ilk düşürmek ve ardından ikincil düşürme. Bu öneri, yükseltme ya da farklı bir hizmet katmanına düşürebilirsiniz zorlanır.

> [!NOTE]
> Yük devretme grubu yapılandırmasının bir parçası olarak ikincil bir veritabanı oluşturduysanız, bu ikincil veritabanını indirgemek için önerilmez. Bu veri katmanınızın yük devretme etkinleştirildikten sonra normal iş yükünü işlemek için yeterli kapasiteye sahip sağlamaktır.

## <a name="preventing-the-loss-of-critical-data"></a>Önemli verilerin kaybını önleme

Geniş alan ağları yüksek gecikme nedeniyle, sürekli kopyalama bir zaman uyumsuz çoğaltma mekanizması kullanır. Bir hata oluşması durumunda zaman uyumsuz çoğaltma bazı veri kaybı kaçınılmaz yapar. Ancak, bazı uygulamalar, veri kaybı olmadan gerektirebilir. Bu kritik güncelleştirmeler korumak için bir uygulama geliştiricisi çağırabilirsiniz [sp_wait_for_database_copy_sync](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) işlem Sistemi'ne hemen sonra sistem yordamı. Çağırma **sp_wait_for_database_copy_sync** son kabul edilen işlem ikincil veritabanına gönderilene kadar çağıran iş parçacığını engeller. Ancak, yeniden yürütülmesi ve ikincil kaydedilen iletilen işlemler için beklemez. **sp_wait_for_database_copy_sync** belirli sürekli kopyalama bağlantısı kapsamlıdır. Birincil veritabanına bağlantı haklarına sahip herhangi bir kullanıcı, bu yordam çağırabilir.

> [!NOTE]
> **sp_wait_for_database_copy_sync** yük devretmeden sonra veri kaybı yaşanmasını önler, ancak tam eşitleme yönelik okuma erişimi garanti etmez. Gecikme nedeniyle bir **sp_wait_for_database_copy_sync** yordam çağrısı önemli ölçüde fazla olabilir ve aramanın zaman işlem günlüğünün boyutuna bağlıdır.

## <a name="programmatically-managing-failover-groups-and-active-geo-replication"></a>Yük devretme grupları ve etkin coğrafi çoğaltma programlama yoluyla yönetme

Otomatik Yük devretme grupları ve etkin daha önce açıklandığı gibi coğrafi çoğaltma ayrıca Azure PowerShell ve REST API'sini kullanarak program aracılığıyla yönetilebilir. Aşağıdaki tabloda kullanılabilir komut kümesi açıklanmaktadır. Etkin coğrafi çoğaltma içeren bir Azure Resource Manager API'leri kümesi yönetimi için de dahil olmak üzere [Azure SQL veritabanı REST API](https://docs.microsoft.com/rest/api/sql/) ve [Azure PowerShell cmdlet'lerini](https://docs.microsoft.com/powershell/azure/overview). Bu API'ler, kaynak gruplarının kullanımı gerektirir ve rol tabanlı güvenlik (RBAC) desteği. Uygulama erişim rolleri hakkında daha fazla bilgi için bkz. [Azure rol tabanlı erişim denetimi](../role-based-access-control/overview.md).

### <a name="manage-sql-database-failover-using-transact-sql"></a>SQL veritabanı yük devretme Transact-SQL kullanarak yönetme

> [!IMPORTANT]
> Yalnızca şu Transact-SQL komutlarını uygulamak için etkin coğrafi çoğaltma ve yük devretme grupları için geçerli değildir. Yalnızca Yük devretme grupları destekledikleri olarak bu nedenle, bunlar da yönetilen örnekleri için geçerli değildir.

| Komut | Açıklama |
| --- | --- |
| [ALTER DATABASE (Azure SQL veritabanı)](/sql/t-sql/statements/alter-database-azure-sql-database) |İkincil üzerinde Sunucu Ekle bağımsız değişkeni bir var olan veritabanı ve başladığında veri çoğaltma için ikincil bir veritabanı oluşturmak için kullanın |
| [ALTER DATABASE (Azure SQL veritabanı)](/sql/t-sql/statements/alter-database-azure-sql-database) |Yük devretmeyi başlatmak için birincil olarak ikincil bir veritabanı geçiş için yük DEVRETME veya FORCE_FAILOVER_ALLOW_DATA_LOSS kullanın |
| [ALTER DATABASE (Azure SQL veritabanı)](/sql/t-sql/statements/alter-database-azure-sql-database) |Bir SQL veritabanı ile belirtilen ikincil veritabanı arasında bir veri çoğaltma sonlandırmak için ikincil üzerinde SUNUCUYU Kaldır'ı kullanın. |
| [sys.geo_replication_links (Azure SQL veritabanı)](/sql/relational-databases/system-dynamic-management-views/sys-geo-replication-links-azure-sql-database) |Azure SQL veritabanı mantıksal sunucusuna her veritabanı için mevcut tüm yineleme bağlantıları hakkında bilgi döndürür. |
| [sys.dm_geo_replication_link_status (Azure SQL veritabanı)](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database) |Belirli bir SQL veritabanı için son çoğaltma saati, son çoğaltma gecikmesine ve çoğaltma bağlantısı ile ilgili diğer bilgileri alır. |
| [sys.dm_operation_status (Azure SQL veritabanı)](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) |Çoğaltma bağlantılarının durumunu ve dahil olmak üzere tüm veritabanı işlemlerinin durumunu gösterir. |
| [sp_wait_for_database_copy_sync (Azure SQL veritabanı)](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) |tüm kaydedilmiş işlemleri çoğaltılır ve etkin ikincil veritabanı tarafından onaylanır kadar beklenecek uygulamanın neden olur. |
|  | |

### <a name="manage-sql-database-failover-using-powershell"></a>PowerShell kullanarak SQL veritabanı yük devretme yönetme

#### <a name="managing-failover-with-single-and-pooled-databases"></a>Yük devretme tek ve havuza alınmış veritabanları ile yönetme

| Cmdlet | Açıklama |
| --- | --- |
| [AzureRmSqlDatabaseToFailoverGroup ekleyin](https://docs.microsoft.com/powershell/module/azurerm.sql/add-azurermsqldatabasetofailovergroup)|Bir veya daha fazla veritabanı için bir Azure SQL veritabanı yük devretme grubu ekler|
| [Get-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldatabase) |Bir veya daha fazla veritabanını alır. |
| [New-AzureRmSqlDatabaseSecondary](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary) |Mevcut bir veritabanı için ikincil bir veritabanı oluşturur ve veri çoğaltmaya başlar. |
| [Set-AzureRmSqlDatabaseSecondary](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary) |Yük devretmeyi başlatmak için ikincil bir veritabanını birincil olarak değiştirir. |
| [Remove-AzureRmSqlDatabaseSecondary](https://docs.microsoft.com/powershell/module/azurerm.sql/remove-azurermsqldatabasesecondary) |Bir SQL Veritabanı ile belirtilen ikincil veritabanı arasında veri çoğaltmayı sonlandırır. |
| [Get-AzureRmSqlDatabaseReplicationLink](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) |Bir Azure SQL Veritabanı ve bir kaynak grubu veya SQL Server arasındaki coğrafi çoğaltma bağlantılarını alır. |
| [New-AzureRmSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |Bu komut, bir yük devretme grubu oluşturur ve birincil ve ikincil sunucularda kaydeder|
| [Remove-AzureRmSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/remove-azurermsqldatabasefailovergroup) | Yük devretme grubuna sunucudan kaldırır ve tüm siler ikincil veritabanları grubu dahil |
| [Get-AzureRmSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldatabasefailovergroup) | Yük devretme grubu yapılandırmasını alır. |
| [Set-AzureRmSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |Yük devretme grubu yapılandırmasını değiştirir |
| [Switch-AzureRMSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/switch-azurermsqldatabasefailovergroup) | İkincil sunucuya Yük devretme grubu yük devretme Tetikleyicileri |
|  | |

> [!IMPORTANT]
> Örnek betikler için bkz: [yapılandırın ve yük devretme etkin coğrafi çoğaltmayı kullanarak tek veritabanı](scripts/sql-database-setup-geodr-and-failover-database-powershell.md), [yapılandırın ve yük devretme etkin coğrafi çoğaltmayı kullanarak havuza alınmış bir veritabanı](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md), ve [ Yapılandırma ve yük devretme grubu yük devretme için tek bir veritabanı](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md).
>

#### <a name="managing-failover-groups-with-managed-instances-preview"></a>Yönetilen örnek (Önizleme) ile yük devretme grupları yönetme

##### <a name="install-the-newest-pre-release-version-of-powershell"></a>En yeni Powershell yayın öncesi sürümünü yükleyin

1. Powershellget modülü 1.6.5 (veya en yeni önizleme sürümü) güncelleştirin. Bkz: [PowerShell Önizleme site](https://www.powershellgallery.com/packages/AzureRM.Sql/4.11.6-preview).

   ```Powershell
      install-module powershellget -MinimumVersion 1.6.5 -force
   ```

2. Yeni bir powershell penceresinde aşağıdaki komutları yürütün:

   ```Powershell
      import-module powershellget
      get-module powershellget #verify version is 1.6.5 (or newer)
      install-module azurerm.sql -RequiredVersion 4.5.0-preview -AllowPrerelease –Force
      import-module azurerm.sql
   ```

##### <a name="prerequisites-before-setting-up"></a>Ayarlamadan önce önkoşulları

- İki yönetilen örnek farklı coğrafi bölgelerde olması gerekir.
- İkincil olması gerekir (kullanıcı veritabanları) boş.
- Birincil ve ikincil yönetilen örnekler aynı kaynak grubunda olması gerekir.
- Yönetilen örnek olması gereken bir parçası olan Vnet'ler [bir VPN ağ geçidi yoluyla bağlanıldığında](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). VNet eşleme desteklenmiyor.
- İki yönetilen örnek alt ağ, çakışan IP adreslerine sahip olamaz.
- Kur, ağ güvenlik grupları (Bu bağlantı noktası 5022 NSG) bu tür ve ' % s'aralığı 11000 gerek ~ 12000 açık bağlantılar diğer sanal ağdan. Örnekler arasında çoğaltma trafiğine izin verecek şekilde budur.
- Bir DNS bölgesi & Dns bölgesi iş ortağı yapılandırmanız gerekir. Bir DNS bölgesi, bir yönetilen örnek için kullanılan bir özelliktir. Yönetilen örnek adı izler ve önündeki ana bilgisayar adı bölümü temsil eder. database.windows.net soneki. Her sanal ağ içindeki ilk yönetilen örnek oluşturma sırasında rastgele bir dize olarak oluşturulur. DNS bölgesi yönetilen örneği oluşturulduktan sonra değiştirilemez ve aynı sanal ağ içindeki tüm yönetilen örnekleri aynı DNS bölge değeri paylaşın. Yönetilen örnek birincil ve ikincil yönetilen örneğe Manged örneği yük devretme grubu kurulumu için aynı DBS bölge değeri paylaşmanız gerekir. Bunu belirterek gerçekleştirmek `DnsZonePartner` ikincil yönetilen örneği oluşturulurken parametre. DNS bölgesi iş ortağı özelliği, bir örnek yük devretme grubu ile paylaşmak için yönetilen örneğe tanımlar. DnsZonePartner giriş olarak başka bir yönetilen örnek kaynak kimliğini uygulamasına geçirme tarafından yönetilen örneği şu anda oluşturuluyor yönetilen örnek iş ortağı aynı DNS bölgesi değerini devralır.

##### <a name="powershell-commandlets-to-create-an-instance-failover-group"></a>Bir örnek yük devretme grubu oluşturmak için Powershell cmdlet'leri

| API | Açıklama |
| --- | --- |
| Yeni AzureRmSqlDatabaseInstanceFailoverGroup |Bu komut, bir yük devretme grubu oluşturur ve birincil ve ikincil sunucularda kaydeder|
| Set-AzureRmSqlDatabaseInstanceFailoverGroup |Yük devretme grubu yapılandırmasını değiştirir|
| Get-AzureRmSqlDatabaseInstanceFailoverGroup |Yük devretme grubu yapılandırmasını alır.|
| Anahtar AzureRmSqlDatabaseInstanceFailoverGroup |İkincil sunucuya Yük devretme grubu yük devretme Tetikleyicileri|

### <a name="manage-sql-database-failover-using-the-rest-api"></a>REST API kullanarak SQL veritabanı yük devretme yönetme

#### <a name="managing-failover-with-single-and-pooled-databases-using-rest-api"></a>Yük devretme REST API kullanarak tek ve havuza alınmış veritabanları ile yönetme

| API | Açıklama |
| --- | --- |
| [Oluşturma veya güncelleştirme veritabanı (createMode geri yükleme =)](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) |Oluşturur, güncelleştirir veya bir birincil veya ikincil bir veritabanını geri yükler. |
| [Alma oluşturma veya güncelleştirme veritabanı durumu](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) |Durum oluşturma işlemi sırasında döndürür. |
| [İkincil veritabanı birincil (Planlı yük devretme) olarak ayarlayın.](https://docs.microsoft.com/rest/api/sql/replicationlinks/failover) |Yük devretme geçerli birincil çoğaltmayı veritabanından tarafından birincil veritabanı çoğaltmasını ayarlar. **Bu seçeneği, yönetilen örneği için desteklenmiyor.**|
| [İkincil veritabanı birincil (planlanmamış yük devretme) olarak ayarlayın.](https://docs.microsoft.com/rest/api/sql/replicationlinks/failoverallowdataloss) |Yük devretme geçerli birincil çoğaltmayı veritabanından tarafından birincil veritabanı çoğaltmasını ayarlar. Bu işlem veri kaybına neden. **Bu seçeneği, yönetilen örneği için desteklenmiyor.**|
| [Çoğaltma bağlantısı alın](https://docs.microsoft.com/rest/api/sql/replicationlinks/get) |Belirli bir çoğaltma bağlantısı belirli bir SQL veritabanı'nda bir coğrafi çoğaltma ortaklığı alır. Bu sys.geo_replication_links katalog görünümünde görünür bilgilerini alır. **Bu seçeneği, yönetilen örneği için desteklenmiyor.**|
| [Çoğaltma bağlantılarını - veritabanı listesi](https://docs.microsoft.com/rest/api/sql/replicationlinks/listbydatabase) | Belirli bir SQL veritabanı'nda bir coğrafi çoğaltma ortaklığı tüm çoğaltma bağlantılarını alır. Bu sys.geo_replication_links katalog görünümünde görünür bilgilerini alır. |
| [Çoğaltma bağlantısı Sil](https://docs.microsoft.com/rest/api/sql/replicationlinks/delete) | Bir veritabanı çoğaltma bağlantısını siler. Yük devretme sırasında yapılamaz. |
| [Oluşturma veya yük devretme grubu güncelleştirme](https://docs.microsoft.com/rest/api/sql/failovergroups/createorupdate) | Oluşturur veya bir yük devretme grubu güncelleştirir |
| [Yük devretme grubunu sil](https://docs.microsoft.com/rest/api/sql/failovergroups/delete) | Yük devretme grubuna sunucudan kaldırır |
| [Yük devretme (Planlı)](https://docs.microsoft.com/rest/api/sql/failovergroups/failover) | Geçerli birincil sunucudan bu sunucuya devreder. |
| [Zorla yük devretme, veri kaybı izin ver](https://docs.microsoft.com/rest/api/sql/failovergroups/forcefailoverallowdataloss) |üzerinden bu sunucu için geçerli birincil sunucudan ails. Bu işlem veri kaybına neden. |
| [Yük devretme grubunu Al](https://docs.microsoft.com/rest/api/sql/failovergroups/get) | Bir yük devretme grubunu alır. |
| [Sunucu tarafından yük devretme grupları Listele](https://docs.microsoft.com/rest/api/sql/failovergroups/listbyserver) | Sunucu yük devretme gruplarını listeler. |
| [Yük devretme grubu güncelleştirme](https://docs.microsoft.com/rest/api/sql/failovergroups/update) | Bir yük devretme grubu güncelleştirir. |
|  | |

#### <a name="managing-failover-groups-with-managed-instances-using-rest-api-preview"></a>REST API'si (Önizleme) kullanarak yönetilen örnekleri'ile yük devretme grupları yönetme

| API | Açıklama |
| --- | --- |
| [Oluşturma veya yük devretme grubu güncelleştirme](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/createorupdate) | Oluşturur veya bir yük devretme grubu güncelleştirir |
| [Yük devretme grubunu sil](https://docs.microsoft.com/rest/api/instancefailovergroups/delete) | Yük devretme grubuna sunucudan kaldırır |
| [Yük devretme (Planlı)](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/failover) | Geçerli birincil sunucudan bu sunucuya devreder. |
| [Zorla yük devretme, veri kaybı izin ver](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/forcefailoverallowdataloss) |üzerinden bu sunucu için geçerli birincil sunucudan ails. Bu işlem veri kaybına neden. |
| [Yük devretme grubunu Al](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/get) | Bir yük devretme grubunu alır. |
| [Yük devretme grupları - konuma göre listesi](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/listbylocation) | Bir konumdaki yük devretme gruplarını listeler. |

## <a name="next-steps"></a>Sonraki adımlar

- Örnek betikler için bkz:
  - [Etkin coğrafi çoğaltmayı kullanarak tek bir veritabanını yapılandırma ve tek bir veritabanının yükünü devretme](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
  - [Etkin coğrafi çoğaltmayı kullanarak havuza alınmış veritabanını yapılandırma ve havuza alınmış veritabanının yükünü devretme](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
  - [Tek bir veritabanı için bir yük devretme grubunu yapılandırma ve yük devretme grubunun yükünü devretme](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
- İş sürekliliğine genel bakış ve senaryolar için bkz: [iş sürekliliğine genel bakış](sql-database-business-continuity.md)
- Veritabanı otomatik yedeklemeler Azure SQL hakkında bilgi edinmek için bkz: [SQL veritabanı otomatik yedeklerinde](sql-database-automated-backups.md).
- Otomatik yedekleme, kurtarma için kullanma hakkında bilgi edinmek için [hizmet tarafından başlatılan yedeklemelerden veritabanını geri yükleme](sql-database-recovery-using-backups.md).
- Yeni birincil sunucu ve veritabanı için kimlik doğrulama gereksinimleri hakkında bilgi edinmek için bkz. [olağanüstü durum kurtarma sonrasında SQL veritabanı güvenlik](sql-database-geo-replication-security-config.md).
