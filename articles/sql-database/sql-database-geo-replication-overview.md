---
title: Yük devretme grupları ve etkin coğrafi çoğaltma - Azure SQL veritabanı | Microsoft Docs
description: Etkin coğrafi çoğaltma otomatik yük devretme grupları kullanmak ve sistemin çalışmaması durumunda autoomatic yük devretme etkinleştirin.
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.date: 08/06/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 3007227808fd7fc4ec185b87a8a44c4497c66597
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39579512"
---
# <a name="overview-active-geo-replication-and-auto-failover-groups"></a>Genel Bakış: Etkin coğrafi çoğaltma ve otomatik yük devretme grupları
Etkin coğrafi çoğaltma, bir veri merkezi ölçek kesinti olması durumunda hızlı bir olağanüstü durum kurtarma gerçekleştirmek uygulama izin veren bir iş sürekliliği çözüm olarak tasarlanmıştır. Coğrafi çoğaltma etkinleştirildiğinde, uygulama farklı bir Azure bölgesinde bir ikincil veritabanına yük devretme başlatabilirsiniz. En fazla dört ikincil veritabanı aynı veya farklı bölgelerde desteklenir ve ikincil veritabanı salt okunur erişim sorgular için de kullanılabilir. Yük devretmeyi el ile uygulama veya kullanıcı tarafından başlatılması gerekir. Yük devretme işleminden sonra yeni birincil farklı bir bağlantı uç noktası vardır. 

> [!NOTE]
> Etkin coğrafi çoğaltma, tüm veritabanları, tüm bölgelerde tüm hizmet katmanlarında kullanılabilir.
>  

Otomatik Yük devretme grupları etkin coğrafi çoğaltma, bir uzantısıdır. Yük devretme birden çok coğrafi çoğaltmalı veritabanı sumultaneously kullanarak bir uygulama tarafından başlatılan yük devretme işlemlerini yönetmek için tasarlanan veya bir kullanıcıya bağlı SQL veritabanı hizmeti tarafından gerçekleştirilmesi için yük devretme için temsilci seçme tarafından tanımlanan ölçütleri. İkincisi, otomatik olarak geri dönülemez bir arıza ya da birincil bölgedeki SQL veritabanı hizmetinizin kullanılabilirliğini tam veya kısmi kaybı ile sonuçlanır diğer planlanmamış bir olay sonra ikincil bir bölgede birden çok ilişkili veritabanlarını kurtarmanıza olanak tanır. Ayrıca, okunabilir ikincil veritabanı salt okunur sorgu iş yükleri yük boşaltması için kullanabilirsiniz. Otomatik Yük devretme grupları, birden çok veritabanı içerdiğinden, bu veritabanları birincil sunucuda yapılandırılması gerekir. Yük devretme grubundaki veritabanları için birincil ve ikincil sunucular, aynı abonelikte olmalıdır. Otomatik Yük devretme grupları için farklı bir bölgedeki tek bir ikincil sunucu grubundaki tüm veritabanlarının çoğaltma destekler.

> [!NOTE]
> Etkin coğrafi çoğaltma, birden fazla ikincil veritabanı gerekiyorsa kullanın.
>  

Etkin coğrafi çoğaltma kullanıyorsanız ve için birincil veritabanı başarısız neden veya çevrimdışı olarak kullanılabileceğini gerekiyor, yük devretme ikincil veritabanlarınızı birini başlatabilirsiniz. Yük devretme ikincil veritabanlarından biri etkinleştirildiğinde, diğer tüm ikincil veritabanı otomatik olarak yeni birincil siteye bağlanır. Veritabanı kurtarma ve herhangi bir kesinti yönetmek için otomatik yük devretme gruplarını kullanıyorsanız, bir veya birkaç veritabanları, otomatik yük devretme grubu sonuçları etkiler. Uygulamanızın gereksinimlerine göre en iyi karşılayan otomatik yük devretme İlkesi yapılandırıp yapılandıramayacağını veya geri çevirmek ve el ile etkinleştirme kullanın. Ayrıca, otomatik yük devretme grupları okuma-yazma sağlar ve kalan salt okuma dinleyici uç noktalarının yük devretme sırasında açısından bir farklılık göstermez. El ile veya otomatik yük devretme etkinleştirme kullanmanıza bakılmaksızın, yük devretme grubunda tüm ikincil veritabanlarını birincil geçer. Veritabanı yük devretme tamamlandıktan sonra yeni bölge için uç noktalarının yönlendirmek için DNS kaydını otomatik olarak güncelleştirilir.

Çoğaltma ve yük devretme işlemlerini tek bir veritabanı veya bir sunucuda veya etkin coğrafi çoğaltmayı kullanarak elastik bir havuzdaki veritabanları kümesi yönetebilirsiniz. Bu kullanarak yapabilirsiniz. 

- [Azure portalı](sql-database-geo-replication-portal.md)
- [PowerShell: Tek veritabanı](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
- [PowerShell: Elastik havuz](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
- [PowerShell: Yük devretme grubu](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
- [Transact-SQL: Tek bir veritabanı veya elastik havuz](/sql/t-sql/statements/alter-database-azure-sql-database)
- [REST API: Tek veritabanı](/rest/api/sql/replicationlinks/failover)
- [REST API: Yük devretme grubu](/rest/api/sql/failovergroups/failover). 
 
Yük devretme işleminden sonra yeni birincil sunucu ve veritabanı için kimlik doğrulama gereksinimleri yapılandırıldığından emin olun. Ayrıntılar için bkz [olağanüstü durum kurtarma sonrasında SQL veritabanı güvenlik](sql-database-geo-replication-security-config.md). Bu, hem de etkin coğrafi çoğaltma ve otomatik yük devretme grupları geçerlidir.

Etkin coğrafi çoğaltma yararlanır [Always On](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) zaman uyumsuz olarak kaydedilen işlem sayısı birincil veritabanında anlık görüntü yalıtımı kullanarak ikincil bir veritabanı çoğaltma için SQL Server teknolojisi. Otomatik Yük devretme grupları üzerinde etkin coğrafi çoğaltma grubu semantiği sağlar ancak aynı zaman uyumsuz çoğaltma mekanizması kullanılır. Herhangi bir anda ederken, ikincil veritabanı birincil veritabanının gerisinde biraz olabilir, ikincil veri kısmi hareket hiçbir zaman olması garanti edilir. Bölgeler arası yedeklilik uygulamaları hızlı bir şekilde veri merkezinin tamamı veya doğal afetler, geri dönülemez bir insan hatalarına veya kötü amaçlı davranışlara neden bir veri merkezinin bölümleri kalıcı kaybı kurtarmanıza olanak sağlar. Belirli RPO verileri şu yolda bulunabilir: [iş Sürekliliğine genel bakış](sql-database-business-continuity.md).

Aşağıdaki şekilde Orta Güney ABD bölgesinde yapılandırılmış Kuzey Orta ABD bölgesinde birincil ve ikincil etkin coğrafi çoğaltma örneği gösterilmektedir.

![coğrafi çoğaltma ilişkisi](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

Okunabilir ikincil veritabanları olduğundan, raporlama işleri gibi salt okunur iş yüklerini boşaltmak için kullanılabilir. Etkin coğrafi çoğaltma kullanıyorsanız, aynı bölgede birincil ile ikincil veritabanı oluşturmak mümkündür, ancak uygulamanın esnekliği yıkıcı hatalarına artırmaz. Otomatik Yük devretme gruplarını kullanıyorsanız ikincil veritabanınıza her zaman farklı bir bölgede oluşturulur.

Ek olarak olağanüstü durum kurtarma etkin coğrafi çoğaltma, aşağıdaki senaryolarda kullanılabilir:

* **Veritabanı geçişi**: etkin coğrafi çoğaltma, bir veritabanı için en düşük kapalı kalma süresi ile çevrimiçi başka bir sunucudan geçirmek için kullanabilirsiniz.
* **Uygulama yükseltmeleri**: uygulama yükseltmeleri sırasında bir hata geri kopya olarak ek bir ikincil oluşturabilirsiniz.

Gerçek iş sürekliliği elde etmek için veri merkezleri arasında veritabanı yedeklilik ekleyerek yalnızca çözümün parçasıdır. Bir arıza kurtarma hizmeti oluşturan tüm bileşenlerini ve tüm bağımlı hizmetlerin gerektirir sonra bir uygulama (hizmet) için uçtan uca kurtarma. İstemci yazılımını (örneğin, tarayıcı özel bir JavaScript ile), web ön uçları, depolama ve DNS bu bileşenlerin örnekleridir. Tüm bileşenlerin aynı hatalara karşı dayanıklı ve kurtarma süresi hedefi, uygulamanızın içinde (RTO) kullanılabilir hale önemlidir. Bu nedenle, tüm bağımlı hizmetleri tanımlayın ve yetenekleri sağlarlar ve garanti anlamak gerekir. Ardından, bağımlı olduğu hizmetlerin yük devretme sırasında hizmetinizin emin olmak için yeterli adımları atmanız gerekir. Olağanüstü durum kurtarma çözümleri tasarlama hakkında daha fazla bilgi için bkz. [olağanüstü durum kurtarma'yı kullanarak etkin coğrafi çoğaltma için bulut çözümleri tasarlama](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="active-geo-replication-capabilities"></a>Etkin coğrafi çoğaltma özellikleri
Etkin coğrafi çoğaltma özelliğini aşağıdaki temel özellikleri sağlar:
* **Otomatik zaman uyumsuz çoğaltma**: mevcut bir veritabanına ekleyerek yalnızca ikincil bir veritabanı oluşturabilirsiniz. Herhangi bir Azure SQL veritabanı sunucusu, ikincil oluşturulabilir. Oluşturulduktan sonra ikincil veritabanı birincil veritabanından kopyalanan verileri doldurulur. Bu işlem, dengeli dağıtım olarak bilinir. İkincil veritabanı oluşturduktan ve çekirdek değeri oluşturulmuş sonra birincil veritabanına güncelleştirmeleri zaman uyumsuz olarak ikincil veritabanına otomatik olarak çoğaltılır. Zaman uyumsuz çoğaltma, birincil veritabanında işlemleri yapıldığından ikincil veritabanına çoğaltılırken anlamına gelir. 
* **Okunabilir ikincil veritabanı**: ikincil bir veritabanını birincil veritabanına erişmek için kullanılan aynı veya farklı güvenlik prensiplerinin salt okunur işlemler için bir uygulamaya erişebilir. Güncelleştirmeler (günlük yeniden yürütme) birincil çoğaltması ikincil yürütülen sorgular tarafından gecikiyor değil emin olmak için anlık görüntü yalıtım modunda ikincil veritabanlarıyla çalışır.

> [!NOTE]
> Birincil şema güncelleştirmeleri varsa, günlük yeniden yürütme ikincil veritabanı üzerinde gecikir. İkincisi, ikincil veritabanında bir şema kilidi gerektirir. 
> 

* **Birden çok okunabilir ikincil veritabanı**: iki veya daha fazla ikincil veritabanı yedeklilik ve birincil veritabanı ve uygulama koruma düzeyini artırın. Birden fazla ikincil veritabanı yoksa, ikincil veritabanlarından biri başarısız olsa bile uygulama korumalı olarak kalır. Yeni bir ikincil veritabanı oluşturulana kadar uygulamayı yalnızca bir ikincil veritabanı yoktur ve yine başarısız olursanız, daha yüksek risklere açıktır.

> [!NOTE]
> Global olarak dağıtılmış bir uygulama oluşturun ve dörtten fazla bölgede veri salt okunur erişim sağlamak etkin coğrafi çoğaltma kullanıyorsanız ikincil bir ikincil (zincirleme olarak bilinen bir işlem) oluşturabilirsiniz. Bu şekilde veritabanı çoğaltma neredeyse sınırsız ölçek elde edebilirsiniz. Ayrıca, zincirleme birincil veritabanı çoğaltma ek yükü azaltır. Yaprak en ikincil veritabanları üzerinde artan çoğaltma gecikmesi dengedir. 
>

* **Elastik havuz veritabanları desteği**: her çoğaltma ayrı bir elastik havuzda katılın veya herhangi bir elastik havuzda hiç olmaması. Her çoğaltma için havuz seçeneği ayrıdır ve herhangi bir çoğaltma yapılandırması sırasında bağımlı değildir (birincil veya ikincil olup olmadığını). Her esnek havuzun tek bir bölgede yer alan, bu nedenle lrs'de aynı topolojiyi içinde bir elastik havuz hiçbir zaman paylaşabilirsiniz.
* **İkincil veritabanının yapılandırılabilir bir performans düzeyini**: birincil ve ikincil veritabanları aynı hizmet katmanı için gereklidir. Bir ikincil veritabanı birincil daha düşük performans düzeyine (Dtu) ile oluşturulabilir. Artan çoğaltma gecikmesi'nin süresi bir yük devretme sonrasında önemli veri kaybı riskini artırır çünkü bu seçenek, yüksek veritabanı yazma etkinliği ile uygulamalar için önerilmez. Ayrıca, yeni birincil daha yüksek bir performans düzeyine yükseltilene kadar yük devretmeden sonra uygulama performansını etkilenmez. Azure portalında günlük g/ç yüzdesi grafik, ikincil çoğaltma yükü sürdürebilmek için gereken en düşük performans düzeyini tahmin etmek için iyi bir yol sağlar. Örneğin, birincil veritabanınız P6 ise (1000 DTU) ve kendi günlük g/ç yüzdesi 50 ikincil en az olması gerekir % P4 (500 DTU). Kullanarak günlük GÇ veri de alabilirsiniz [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) veya [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) veritabanı görünümleri.  SQL veritabanı performans düzeyleri hakkında daha fazla bilgi için bkz. [SQL veritabanı hizmet katmanları nelerdir](sql-database-service-tiers.md). 
* **Kullanıcı tarafından denetlenen bir yük devretme ve yeniden çalışma**: ikincil bir veritabanı açıkça birincil role herhangi bir zamanda uygulama veya kullanıcı tarafından değiştirilebilir. Gerçek bir kesinti sırasında "planlanmamış" seçeneği, hangi hemen ikincil yükseltir birincil olarak kullanılmalıdır. Başarısız birincil kurtarır ve yeniden kullanılabilir olduğunda, sistem otomatik olarak kurtarılan birincil ikincil olarak işaretler ve yeni birincil ile güncel getirin. Birincil ikincil en son değişiklikleri çoğaltır önce başarısız olursa zaman uyumsuz çoğaltma yapısı nedeniyle, az miktarda veriniz planlanmamış yük devretme sırasında kaybolur. Birden çok ikincilleriyle birincil yük devrettiğinde, sistem otomatik olarak çoğaltma ilişkilerini yeniden yapılandırır ve kalan ikincil veritabanı herhangi bir kullanıcı müdahalesi gerektirmeden yeni yükseltilen birincil siteye bağlar. Yük devretme neden oldu kesinti giderildikten sonra uygulamayı birincil bölgeye geri istenebilir. Bunu yapmak için yük devretme komut "planlanmış" seçeneği ile çağrılmalıdır. 
* **Kimlik bilgileri ve güvenlik duvarı kuralları eşitlenmiş durumda kalmasını**: kullanmanızı öneririz [veritabanı güvenlik duvarı kuralları](sql-database-firewall-configure.md) bu kurallar, tüm ikincil veritabanlarına sahip olmak için veritabanıyla çoğaltılabilir şekilde coğrafi çoğaltmalı veritabanı birincil olarak aynı güvenlik duvarı kuralları. Bu yaklaşım el ile yapılandırmak ve hem birincil ve ikincil veritabanlarını barındıran sunucu güvenlik duvarı kurallarını sağlamak müşterilerin ihtiyacını ortadan kaldırır. Benzer şekilde, kullanarak [kapsanan veritabanı kullanıcıları](sql-database-manage-logins.md) veriler için birincil ve ikincil veritabanları her zaman sahip aynı erişim sağlar. kullanıcı kimlik bilgilerini bir yük devretme sırasında bu yüzden hiçbir kesinti nedeniyle oturum açma ve parolaları ile uyuşmuyor. Ek olarak [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md), müşteriler, hem birincil hem de ikincil veritabanları için kullanıcı erişimini yönetebilir ve yönetme gereksinimini ortadan kimlik bilgileri veritabanlarında tamamen.

## <a name="auto-failover-group-capabilities"></a>Otomatik Yük devretme grubu özellikleri

Otomatik Yük devretme grupları özelliği, grubu düzeyi çoğaltma ve otomatik yük devretme destekleyerek etkin coğrafi çoğaltma, güçlü bir Özet sağlar. Ayrıca, ek bir dinleyici uç noktalarının sağlayarak SQL bağlantı dizesi yük devretmeden sonra değiştirmek için yeterlilikte kaldırır. 

* **Yük devretme grubu**: bir veya daha çok yük devretme grupları, farklı bölgelerde (birincil ve ikincil sunucular) iki sunucu arasında oluşturulabilir. Her grup, tüm veya bazı birincil veritabanı birincil bölgede kesinti nedeniyle kullanılamıyor, bir birim olarak kurtarılan bir veya birden çok veritabanı içerebilir.  
* **Birincil sunucu**: yük devretme grubunda birincil veritabanlarını barındıran bir sunucu.
* **İkincil sunucu**: yük devretme grubundaki ikincil veritabanlarını barındıran bir sunucu. İkincil sunucu, birincil sunucu ile aynı bölgede olamaz.
* **Veritabanları yük devretme grubuna ekleme**: bir sunucu veya elastik havuz içinde birden fazla veritabanı aynı yük devretme grubuna yerleştirin. Tek başına veritabanı grubuna eklerseniz, otomatik olarak aynı sürüm ve performans düzeyi kullanarak ikincil bir veritabanı oluşturur. Birincil veritabanını bir elastik havuzda, aynı ada sahip bir elastik havuzdaki ikincil otomatik olarak oluşturulur. İkincil bir veritabanı ikincil sunucuya zaten olan bir veritabanının eklerseniz, söz konusu coğrafi çoğaltma grubu tarafından devralınır.

> [!NOTE]
> İkincil veritabanına yük devretme grubunun parçası olmayan bir Server'da zaten bir veritabanı eklerken, yeni bir ikincil ikincil sunucuya oluşturulur. 
>

* **Yük devretme grubu okuma / yazma dinleyici**: bir DNS CNAME kaydı biçimlendirilmiş olarak  **&lt;yük devretme grubu adı&gt;. database.windows.net** işaret eden geçerli birincil sunucu URL'si. Yük devretme sonrasında birincil değiştiğinde şeffaf bir şekilde birincil veritabanına bağlanmak okuma-yazma SQL uygulamalar sağlar. 
* **Yük devretme grubu salt okunur dinleyicisi**: bir DNS CNAME kaydı biçimlendirilmiş olarak  **&lt;yük devretme grubu adı&gt;. secondary.database.windows.net** işaret eden ikincil sunucu URL'si. Belirtilen yük dengeleyici kurallarını kullanarak ikincil bir veritabanı için saydam bir şekilde bağlanmak salt okunur SQL uygulamalar sağlar. 
* **Otomatik Yük devretme İlkesi**: varsayılan olarak, yük devretme grubuna bir otomatik yük devretme İlkesi ile yapılandırılır. Sistem hatası algıladı ve yetkisiz kullanım süresi dolmuştur sonra yük devretmeyi tetikler. Sistem, kesinti etkisi ölçeğini son SQL veritabanı hizmeti yerleşik yüksek kullanılabilirlik altyapısı tarafından azaltılamaz doğrulamanız gerekir. Uygulama yük devretme iş akışını denetlemek istiyorsanız, otomatik yük devretme kapatabilirsiniz. 
* **Salt okunur bir yük devretme İlkesi**: varsayılan olarak, yük devretme işlemlerini salt okunur dinleyicisi devre dışı bırakıldı. İkincil çevrimdışı olduğunda birincil performansını etkilenmez sağlar. Ancak, bu da salt okunur oturumları ikincil kurtarılmadan kadar bağlanmak mümkün olmayacaktır anlamına gelir. Kapalı kalma süresi için salt okunur oturumları tolere olamaz ve olası performans düşüşü birincil çoğaltamaz hem salt okunur ve okuma-yazma trafiği geçici olarak birincil kullanılmak üzere Tamam, salt okunur dinleyici için yük devretme etkinleştirebilirsiniz. Bu durumda ikincil sunucu kullanılabilir değilse, salt okunur trafiği otomatik olarak birincil sunucuya yönlendirilecek.  
* **El ile yük devretme**: yük devretme manuel otomatik yük devretme yapılandırması bağımsız olarak herhangi bir zamanda olarak başlatabilirsiniz. Otomatik Yük devretme İlkesi yapılandırılmamışsa, el ile yük devretme yük devretme grubundaki veritabanlarını kurtarmak için gereklidir. Zorlanmış veya kolay yük devretme (tam veri eşitlemesi ile) başlatabilir. İkincisi etkin sunucu birincil bölgeye yeniden konumlandırmak için kullanılabilir. Yük devretme tamamlandığında, doğru sunucuyla bağlantı sağlamak için DNS kayıtlarını otomatik olarak güncelleştirilir.
* **Veri kaybı yetkisiz kullanım süresi**: zaman uyumsuz çoğaltması kullanılarak birincil ve ikincil veritabanları eşitlenmiş olduğundan, yük devretme veri kaybına neden olabilir. Otomatik Yük devretme İlkesi, veri kaybı toleransını uygulamanızın yansıtacak şekilde özelleştirebilirsiniz. Yapılandırarak **GracePeriodWithDataLossHours**, sistem için sonuç veri kaybı olasılığı yüksek olan yük devretmeyi başlatmadan önce bekleyeceği süreyi denetleyebilirsiniz. 

> [!NOTE]
> Sistem algıladığında grubundaki veritabanları hala çevrimiçi olduğu (örneğin, kesinti yalnızca hizmet denetim düzlemi etkilenen), tarafındanayarlanandeğerebakılmaksızınhementamverieşitlemesi(kolayyükdevretme)ileyükdevretmeyietkinleştirir **GracePeriodWithDataLossHours**. Bu davranış, Kurtarma sırasında veri kaybı olmadan olmasını sağlar. Yalnızca kolay bir yük devretme mümkün olmadığında yetkisiz kullanım süresi geçerli olur. Yetkisiz kullanım süresi dolmadan önce kesinti giderildikten, yük devretme etkinleştirilmedi.
>

* **Birden çok yük devretme grupları**: yük devretme işlemleri ölçeğini denetlemek için birden çok yük devretme grupları sunucuları aynı çifti için yapılandırabilirsiniz. Her grup bağımsız olarak yük devreder. Elastik havuzlar, çok kiracılı bir uygulama kullanıyorsa, birincil ve ikincil veritabanlarının havuzdaki her karıştırmak için bu özelliği kullanabilirsiniz. Bu şekilde kesinti etkisini kiracılar yalnızca yarısını için azaltabilir.

## <a name="best-practices-of-using-failover-groups-for-business-continuity"></a>Yük devretme grupları kullanarak iş sürekliliği için en iyi uygulamalar
İş sürekliliği aklınızda ile bir hizmet tasarlarken, bu yönergeleri izlemelidir:

- **Yük devretme işlemlerini birden fazla veritabanını yönetmek için bir veya birden çok yük devretme grupları kullanma**: bir veya daha çok yük devretme grupları, farklı bölgelerde (birincil ve ikincil sunucular) iki sunucu arasında oluşturulabilir. Her grup, tüm veya bazı birincil veritabanı birincil bölgede kesinti nedeniyle kullanılamıyor, bir birim olarak kurtarılan bir veya birden çok veritabanı içerebilir. Yük devretme grubu, birincil olarak aynı hizmet hedefi ile coğrafi-ikincil veritabanı oluşturur. Mevcut bir coğrafi çoğaltma ilişkisi için yük devretme grubuna eklerseniz, coğrafi-ikincil ile aynı hizmet düzeyi hedefi birincil olarak yapılandırıldığından emin olun.
- **OLTP iş yükü için okuma / yazma dinleyici kullanın**: OLTP işlemleri gerçekleştirirken kullanmak  **&lt;yük devretme grubu adı&gt;. database.windows.net** URL ve bağlantı sunucuyu belirtir otomatik olarak birincil siteye yönlendirilir. Bu URL, yük devretme sonrasında değiştirmez. Not yük devretme, yalnızca istemci DNS önbelleği yenilendiğini sonra istemci bağlantıları yeni birincil siteye yönlendirilir DNS kaydı güncelleştirmeniz gerekir.
- **Salt okunur iş yükü için salt okunur bir dinleyici kullanın**: belirli veri eskime dayanıklı olan, mantıksal olarak yalıtılmış bir salt okunur yükünü varsa, ikincil veritabanı uygulamada kullanabilirsiniz. Salt okunur oturumları kullanmanızın  **&lt;yük devretme grubu adı&gt;. secondary.database.windows.net** sunucusu olarak URL ve bağlantı otomatik olarak yönlendirilir ikincil. Ayrıca bağlantı dizesi kullanarak hedefi okuma belirtmek önerilir **ApplicationIntent salt okunur =**. 
- **Performans düşüşü için hazır**: SQL yük devretme karar uygulama veya kullanılan diğer hizmetlerin geri kalanından bağımsız. Uygulama "tek bir bölge ve bazı durumlarda başka bazı bileşenleri ile karışık". Performans düşüşü önlemek için DR bölgesinde yedekli uygulama dağıtımını sağlamak ve bu makaledeki yönergeleri izleyin. Farklı bağlantı dizesini kullanmak Not DR bölgesinde uygulama yok.  
- **Veri kaybı hazırlama**: kesinti algılanırsa, sıfır veri kaybı bizim bilgi en iyi ise SQL otomatik olarak okuma / yazma yük devretmeyi tetikler. Aksi takdirde, belirtilen süre boyunca bekler **GracePeriodWithDataLossHours**. Belirttiyseniz **GracePeriodWithDataLossHours**, veri kaybı için hazır. Genel olarak, kesintiler sırasında Azure kullanılabilirlik ayrıcalık tanır. Veri kaybını göze alamayacağı, ayarladığınızdan emin olun **GracePeriodWithDataLossHours** 24 saat gibi yeterince büyük bir sayı. 

> [!IMPORTANT]
> 800 ya da daha az dtu'ları ve coğrafi çoğaltmayı kullanarak 250'den fazla veritabanları ile elastik havuzları artık planlanan yük devretmeler sorunlarla karşılaşabilirsiniz ve performansı düştü.  Bu sorunları yazma yoğunluklu iş yükleri için coğrafi çoğaltma uç noktaları coğrafyaya göre değişim yaygın olarak ayrılıyorsa ya da her veritabanı için birden çok ikincil uç noktaları kullanıldığında ortaya daha yüksektir.  Coğrafi çoğaltma gecikmesi zamanla artar, bu sorunları belirtileri gösterilir.  Bu gecikme kullanılarak izlenebilir [sys.dm_geo_replication_link_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database).  Bu sorun oluşursa, risk azaltma işlemleri havuz Dtu sayısını artırmak veya coğrafi olarak çoğaltılmış veritabanlarının aynı havuzdaki sayısını azaltmayı içerir.

## <a name="failover-groups-and-network-security"></a>Yük devretme grupları ve ağ güvenliği 

Veri katmanı için ağ erişimi belirli bir bileşeni ya da VM gibi bileşenleri kısıtlanmıştır güvenlik kuralları gerektiren bazı uygulamalar için web hizmeti vb. Bu gereksinim, iş sürekliliği tasarım için bazı zorluklar ve yük devretme grupları kullanımını gösterir. Kısıtlı erişim uygularken aşağıdaki seçenekleri düşünmelisiniz.

### <a name="using-failover-groups-and-virtual-network-rules"></a>Yük devretme grupları ve sanal ağ kuralları kullanma

Kullanıyorsanız [sanal ağ hizmet uç noktaları ve kuralları](sql-database-vnet-service-endpoint-rule-overview.md) SQL veritabanınıza erişimi kısıtlamak için her sanal ağ hizmet uç noktası geçerli tek bir Azure bölgesine olduğunu unutmayın. Uç nokta, alt ağından gelen iletişimi kabul etmek üzere diğer bölgeler etkinleştirmez. Bu nedenle, yalnızca aynı bölgede dağıtılan istemci uygulamaları birincil veritabanına bağlanabilirsiniz. Yük devretme (ikincil) farklı bölgedeki bir sunucuya yönlendirdi SQL istemci oturumlarını sonuçlanır olduğundan, bu oturumlar o bölgenin dışında bir istemciden geldiyse başarısız olur. Sanal ağ kuralları katılan sunucular eklenirse, bu nedenle, otomatik yük devretme İlkesi etkinleştirilemez. El ile yük devretme desteği için aşağıdaki adımları izleyin:

1.  Ön uç bileşenleri, uygulamanızın (web hizmeti, sanal makineler vb.) ikincil bölgedeki yedekli kopyalarını sağlama
2.  Yapılandırma [sanal ağ kuralları](sql-database-vnet-service-endpoint-rule-overview.md) birincil ve ikincil bir sunucu için ayrı ayrı
3.  Etkinleştirme [ön uç yük devretme trafik Yöneticisi yapılandırması kullanma](sql-database-designing-cloud-solutions-for-disaster-recovery.md#scenario-1-using-two-azure-regions-for-business-continuity-with-minimal-downtime)
4.  Kesinti algılandığında el ile yük devretme başlatma

Bu seçenek, ön uç ve veri katmanı arasında tutarlı bir gecikme gerektiren uygulamalar için iyileştirilmiştir ve ön uç, veri katmanı veya her ikisi de kesintiden etkilendiğinde kurtarmayı destekler. 

> [!NOTE]
> Kullanıyorsanız **salt okuma dinleyici** yük dengelemek için bir salt okunur iş yükü, ikincil veritabanına bağlanabilmesi için bu iş yükünü bir VM veya ikincil bölgedeki diğer noktaları kaynak yürütüldüğünden emin olun.
>

 ### <a name="using-failover-groups-and-sql-database-firewall-rules"></a>Yük devretme grupları ve SQL veritabanı güvenlik duvarı kuralları kullanma

İş sürekliliği planınızın otomatik yük devretme grupları kullanarak yük devretme gerektiriyorsa, geleneksel güvenlik duvarı kuralları kullanarak SQL veritabanınıza erişimi kısıtlayabilirsiniz.  Otomatik Yük devretme desteği için aşağıdaki adımları izleyin:

1.  [Bir genel IP oluşturma](../virtual-network/virtual-network-public-ip-address.md#create-a-public-ip-address) 
2.  [Bir genel yük dengeleyici oluşturma](../load-balancer/quickstart-create-basic-load-balancer-portal.md#create-a-basic-load-balancer) ve genel IP atayın. 
3.  [Bir sanal ağ ve sanal makineler oluşturma](../load-balancer/quickstart-create-basic-load-balancer-portal.md#create-back-end-servers) , ön uç bileşenleri 
4.  [Ağ güvenlik grubu oluşturma](../virtual-network/security-overview.md) ve gelen bağlantıları yapılandırın. 
5. Giden bağlantılar 'Sql' kullanarak Azure SQL veritabanı'na açık olduğundan emin olun [hizmet etiketi](../virtual-network/security-overview.md#service-tags). 
5.  Oluşturma bir [SQL veritabanı güvenlik duvarı kuralı](sql-database-firewall-configure.md) 1. adımda oluşturduğunuz genel IP adresinden gelen trafiğe izin verme. 

Hakkında daha fazla bilgi için giden erişimi nasıl yapılandırılacağı ve hangi IP güvenlik duvarı kurallarında kullanmak için bkz: [yük dengeleyici giden bağlantılar](../load-balancer/load-balancer-outbound-connections.md).

Yukarıdaki yapılandırma, otomatik yük devretme ön uç bileşenlerini bağlantılarından engellemez ve uygulama ön ucu ve veri katmanı arasında daha uzun gecikme etkilenmemesini varsayar sağlayacaktır.

> [!IMPORTANT]
> Bölgesel kesintiler için iş sürekliliği sağlamak için hem ön uç bileşenleri hem de veritabanı için coğrafi artıklık emin olmanız gerekir. 
>

## <a name="upgrading-or-downgrading-a-primary-database"></a>Yükseltme veya bir birincil veritabanı önceki sürüme indirme
Yükseltme veya bir birincil tüm ikincil veritabanlarını kesmeden (aynı hizmet katmanında) bir farklı bir performans düzeyine düşürebilirsiniz. Yükseltme sırasında ikincil veritabanı yükseltmeniz ve ardından birincil yükseltmeniz önerilir. Önceki sürüme indirirken ters sırada: birincil ilk düşürmek ve ardından ikincil düşürme. Bu öneri, yükseltme ya da farklı bir hizmet katmanına düşürebilirsiniz zorlanır. 

> [!NOTE]
> Yük devretme grubu yapılandırmasının bir parçası olarak ikincil bir veritabanı oluşturduysanız, bu ikincil veritabanını indirgemek için önerilmez. Bu veri katmanınızın yük devretme etkinleştirildikten sonra normal iş yükünü işlemek için yeterli kapasiteye sahip sağlamaktır. 
>

## <a name="preventing-the-loss-of-critical-data"></a>Önemli verilerin kaybını önleme
Geniş alan ağları yüksek gecikme nedeniyle, sürekli kopyalama bir zaman uyumsuz çoğaltma mekanizması kullanır. Bir hata oluşması durumunda zaman uyumsuz çoğaltma bazı veri kaybı kaçınılmaz yapar. Ancak, bazı uygulamalar, veri kaybı olmadan gerektirebilir. Bu kritik güncelleştirmeler korumak için bir uygulama geliştiricisi çağırabilirsiniz [sp_wait_for_database_copy_sync](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) işlem Sistemi'ne hemen sonra sistem yordamı. Çağırma **sp_wait_for_database_copy_sync** son kabul edilen işlem ikincil veritabanına gönderilene kadar çağıran iş parçacığını engeller. Ancak, yeniden yürütülmesi ve ikincil kaydedilen iletilen işlemler için beklemez. **sp_wait_for_database_copy_sync** belirli sürekli kopyalama bağlantısı kapsamlıdır. Birincil veritabanına bağlantı haklarına sahip herhangi bir kullanıcı, bu yordam çağırabilir.

> [!NOTE]
> **sp_wait_for_database_copy_sync** yük devretmeden sonra veri kaybı yaşanmasını önler, ancak tam eşitleme yönelik okuma erişimi garanti etmez. Gecikme nedeniyle bir **sp_wait_for_database_copy_sync** yordam çağrısı önemli ölçüde fazla olabilir ve aramanın zaman işlem günlüğünün boyutuna bağlıdır. 
> 

## <a name="programmatically-managing-failover-groups-and-active-geo-replication"></a>Yük devretme grupları ve etkin coğrafi çoğaltma programlama yoluyla yönetme
Otomatik Yük devretme grupları ve etkin daha önce açıklandığı gibi coğrafi çoğaltma ayrıca Azure PowerShell ve REST API'sini kullanarak program aracılığıyla yönetilebilir. Aşağıdaki tabloda kullanılabilir komut kümesi açıklanmaktadır.

**Azure Resource Manager API'si ve rol tabanlı güvenlik**: içeren Azure Resource Manager API'leri bir dizi yönetim için etkin coğrafi çoğaltma dahil olmak üzere [Azure SQL veritabanı REST API](https://docs.microsoft.com/rest/api/sql/) ve [Azure PowerShell cmdlet'leri](https://docs.microsoft.com/powershell/azure/overview). Bu API'ler, kaynak gruplarının kullanımı gerektirir ve rol tabanlı güvenlik (RBAC) desteği. Uygulama erişim rolleri hakkında daha fazla bilgi için bkz. [Azure rol tabanlı erişim denetimi](../role-based-access-control/overview.md).

## <a name="manage-sql-database-failover-using-transact-sql"></a>SQL veritabanı yük devretme Transact-SQL kullanarak yönetme

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

## <a name="manage-sql-database-failover-using-powershell"></a>PowerShell kullanarak SQL veritabanı yük devretme yönetme

| Cmdlet | Açıklama |
| --- | --- |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase) |Bir veya daha fazla veritabanını alır. |
| [New-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary) |Mevcut bir veritabanı için ikincil bir veritabanı oluşturur ve veri çoğaltmaya başlar. |
| [Set-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary) |Yük devretmeyi başlatmak için ikincil bir veritabanını birincil olarak değiştirir. |
| [Remove-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/remove-azurermsqldatabasesecondary) |Bir SQL Veritabanı ile belirtilen ikincil veritabanı arasında veri çoğaltmayı sonlandırır. |
| [Get-AzureRmSqlDatabaseReplicationLink](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) |Bir Azure SQL Veritabanı ve bir kaynak grubu veya SQL Server arasındaki coğrafi çoğaltma bağlantılarını alır. |
| [New-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |   Bu komut, bir yük devretme grubu oluşturur ve birincil ve ikincil sunucularda kaydeder|
| [Remove-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/remove-azurermsqldatabasefailovergroup) | Yük devretme grubuna sunucudan kaldırır ve tüm siler ikincil veritabanları grubu dahil |
| [Get-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/get-azurermsqldatabasefailovergroup) | Yük devretme grubu yapılandırmasını alır. |
| [Set-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |   Yük devretme grubu yapılandırmasını değiştirir |
| [Switch-AzureRMSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/switch-azurermsqldatabasefailovergroup) | İkincil sunucuya Yük devretme grubu yük devretme Tetikleyicileri |
|  | |

> [!IMPORTANT]
> Örnek betikler için bkz: [yapılandırın ve yük devretme etkin coğrafi çoğaltmayı kullanarak tek veritabanı](scripts/sql-database-setup-geodr-and-failover-database-powershell.md), [yapılandırın ve yük devretme etkin coğrafi çoğaltmayı kullanarak havuza alınmış bir veritabanı](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md), ve [ Yapılandırma ve yük devretme grubu yük devretme için tek bir veritabanı](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md).
>

## <a name="manage-sql-database-failover-using-the-rest-api"></a>REST API kullanarak SQL veritabanı yük devretme yönetme
| API | Açıklama |
| --- | --- |
| [Oluşturma veya güncelleştirme veritabanı (createMode geri yükleme =)](/rest/api/sql/Databases/CreateOrUpdate) |Oluşturur, güncelleştirir veya bir birincil veya ikincil bir veritabanını geri yükler. |
| [Alma oluşturma veya güncelleştirme veritabanı durumu](/rest/api/sql/Databases/CreateOrUpdate) |Durum oluşturma işlemi sırasında döndürür. |
| [İkincil veritabanı birincil (Planlı yük devretme) olarak ayarlayın.](/rest/api/sql/replicationlinks/failover) |Yük devretme geçerli birincil çoğaltmayı veritabanından tarafından birincil veritabanı çoğaltmasını ayarlar. |
| [İkincil veritabanı birincil (planlanmamış yük devretme) olarak ayarlayın.](/rest/api/sql/replicationlinks/failoverallowdataloss) |Yük devretme geçerli birincil çoğaltmayı veritabanından tarafından birincil veritabanı çoğaltmasını ayarlar. Bu işlem veri kaybına neden. |
| [Çoğaltma bağlantısı alın](/rest/api/sql/replicationlinks/get) |Belirli bir çoğaltma bağlantısı belirli bir SQL veritabanı'nda bir coğrafi çoğaltma ortaklığı alır. Bu sys.geo_replication_links katalog görünümünde görünür bilgilerini alır. |
| [Çoğaltma bağlantılarını - veritabanı listesi](/rest/api/sql/replicationlinks/listbydatabase) | Belirli bir SQL veritabanı'nda bir coğrafi çoğaltma ortaklığı tüm çoğaltma bağlantılarını alır. Bu sys.geo_replication_links katalog görünümünde görünür bilgilerini alır. |
| [Çoğaltma bağlantısı Sil](/rest/api/sql/databases/delete) | Bir veritabanı çoğaltma bağlantısını siler. Yük devretme sırasında yapılamaz. |
| [Oluşturma veya yük devretme grubu güncelleştirme](/rest/api/sql/failovergroups/createorupdate) | Oluşturur veya bir yük devretme grubu güncelleştirir |
| [Yük devretme grubunu sil](/rest/api/sql/failovergroups/delete) | Yük devretme grubuna sunucudan kaldırır |
| [Yük devretme (Planlı)](/rest/api/sql/failovergroups/failover) | Geçerli birincil sunucudan bu sunucuya devreder. |
| [Zorla yük devretme, veri kaybı izin ver](/rest/api/sql/failovergroups/forcefailoverallowdataloss) |üzerinden bu sunucu için geçerli birincil sunucudan ails. Bu işlem veri kaybına neden. |
| [Yük devretme grubunu Al](/rest/api/sql/failovergroups/get) | Bir yük devretme grubunu alır. |
| [Sunucu tarafından yük devretme grupları Listele](/rest/api/sql/failovergroups/listbyserver) | Sunucu yük devretme gruplarını listeler. |
| [Yük devretme grubu güncelleştirme](/rest/api/sql/failovergroups/update) | Bir yük devretme grubu güncelleştirir. |
|  | |

## <a name="next-steps"></a>Sonraki adımlar
* Örnek betikler için bkz:
   - [Etkin coğrafi çoğaltmayı kullanarak tek bir veritabanını yapılandırma ve tek bir veritabanının yükünü devretme](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
   - [Etkin coğrafi çoğaltmayı kullanarak havuza alınmış veritabanını yapılandırma ve havuza alınmış veritabanının yükünü devretme](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
   - [Tek bir veritabanı için bir yük devretme grubunu yapılandırma ve yük devretme grubunun yükünü devretme](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
* İş sürekliliğine genel bakış ve senaryolar için bkz: [iş sürekliliğine genel bakış](sql-database-business-continuity.md)
* Veritabanı otomatik yedeklemeler Azure SQL hakkında bilgi edinmek için bkz: [SQL veritabanı otomatik yedeklerinde](sql-database-automated-backups.md).
* Otomatik yedekleme, kurtarma için kullanma hakkında bilgi edinmek için [hizmet tarafından başlatılan yedeklemelerden veritabanını geri yükleme](sql-database-recovery-using-backups.md).
* Yeni birincil sunucu ve veritabanı için kimlik doğrulama gereksinimleri hakkında bilgi edinmek için bkz. [olağanüstü durum kurtarma sonrasında SQL veritabanı güvenlik](sql-database-geo-replication-security-config.md).

