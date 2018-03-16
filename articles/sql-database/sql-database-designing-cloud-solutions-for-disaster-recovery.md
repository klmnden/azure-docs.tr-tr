---
title: "Tasarım Azure SQL veritabanı kullanarak yüksek oranda kullanılabilir hizmet | Microsoft Docs"
description: "Uygulama tasarımı için Azure SQL veritabanı kullanarak yüksek oranda kullanılabilir hizmetler hakkında bilgi edinin."
keywords: "Bulut olağanüstü durum kurtarma, olağanüstü durum kurtarma çözümleri, uygulama veri yedekleme, coğrafi çoğaltma, iş sürekliliği planlama"
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: article
ms.date: 03/07/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: c596006e33c2c4f0228c14a65f58e82bcf300727
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="designing-highly-available-services-using-azure-sql-database"></a>Azure SQL veritabanı kullanarak yüksek oranda kullanılabilir hizmetler tasarlama

Derleme ve Azure SQL veritabanı yüksek oranda kullanılabilir hizmetleri dağıtma, kullandığınız [yük devretme grupları ve etkin coğrafi çoğaltma](sql-database-geo-replication-overview.md) bölgesel kesintiler ve geri dönülemez hataları için esneklik sağlamak için. Ayrıca, ikincil veritabanlarıyla Hızlı Kurtarma sağlar. Bu makale üzerinde ortak uygulama düzenleri odaklanır ve avantajları ve dengelemeler her seçenekle ele almaktadır. Esnek havuzlar etkin coğrafi çoğaltma hakkında daha fazla bilgi için bkz: [esnek havuz olağanüstü durum kurtarma stratejilerini](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

> [!NOTE]
> Premium veritabanları ve havuzları kullanıyorsanız, bunları dayanıklı bölgesel kesintileri (şu anda önizlemede) bölge olarak yedekli dağıtım yapılandırması için dönüştürerek yapabileceğiniz. Bkz: [bölge olarak yedekli veritabanları](sql-database-high-availability.md).  

## <a name="scenario-1-using-two-azure-regions-for-business-continuity-with-minimal-downtime"></a>Senaryo 1: en az kapalı kalma süresi ile iş sürekliliği için iki Azure bölgeleri kullanma
Bu senaryoda, uygulamaları aşağıdaki özelliklere sahiptir: 
*   Uygulama bir Azure bölgesinde etkindir
*   Tüm veritabanı oturumları okuma ve yazma erişimi (RW) veri gerektirir
*   Web Katmanı ve veri katmanı gecikme süresi ve trafik maliyetini azaltmak için birlikte bulunan gerekir 
*   Temelde, kapalı kalma süresi bir daha yüksek iş riski bu uygulamalar için veri kaybı daha oluşturur

Bu durumda, uygulama dağıtım topolojisi tüm uygulama bileşenleri için yük devretme birlikte gerektiğinde bölgesel afetler işlemek için optimize edilmiştir. Aşağıdaki diyagram bu topolojiyi gösterir. Coğrafi artıklık için uygulamanın kaynaklarına A ve b bölgeye dağıtılır Ancak, bölge A fark edene kadar bölge B kaynaklarında kullanılmaz. Bir yük devretme grubu veritabanı bağlantısı, çoğaltma ve yük devretme yönetmek için iki bölgeler arasında yapılandırılır. Web hizmeti hem bölgelerde okuma-yazma dinleyicisi aracılığıyla veritabanına erişmek için yapılandırılmış  **&lt;yük devretme grup adı&gt;. database.windows.net** (1). Trafik Yöneticisi kullanmak üzere ayarlandı [öncelik yönlendirme yöntemi](../traffic-manager/traffic-manager-configure-priority-routing-method.md) (2).  

> [!NOTE]
> [Azure trafik Yöneticisi](../traffic-manager/traffic-manager-overview.md) bu makalede yalnızca gösterim amacıyla kullanılmıştır. Öncelik yönlendirme yöntemini destekleyen yük dengeleme çözümü kullanabilirsiniz.    
>

Aşağıdaki diyagramda bu yapılandırma bir kesinti önce gösterilmektedir:

![Senaryo 1. Kesinti önce yapılandırma.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario1-a.png)

Birincil bölgede kesinti sonrasında, SQL veritabanı hizmetinin algılar birincil veritabanı erişilebilir değil ve (1) otomatik yük devretme İlkesi parametrelere bağlı olarak ikincil bölgeye yük devretmeyi tetikler. Uygulama SLA bağlı olarak, zamanı kesinti algılanmasını ve yük devretme kendisi arasında denetimleri bir yetkisiz kullanım süresi yapılandırabilirsiniz. Yük devretme grubu veritabanının yük devretmeyi tetikler önce bu trafik Yöneticisi uç nokta yük devretme başlatır mümkündür. Bu durumda web uygulaması veritabanına hemen yeniden bağlanamaz. Ancak veritabanı yük devretme tamamlandıktan hemen sonra tutarsızlıklara otomatik olarak başarılı olur. Başarısız bölge geri yüklenen ve tekrar çevrimiçi olduğunda, eski birincil otomatik olarak yeni ikincil olarak yeniden bağlanır. Aşağıdaki diyagramda, yük devretme sonrasında yapılandırması gösterilmiştir.
 
> [!NOTE]
> Yük devretme sonrasında kaydedilen tüm işlem yeniden bağlanma sırasında kaybolur. Yük devretme işlemi tamamlandıktan sonra B bölgede kullanıcı istekleri işlemeyi yeniden başlatın ve yeniden uygulamasıdır. Web uygulaması ve birincil veritabanı B bölgede sunulmuştur ve aynı konumda kalır. n>

![Senaryo 1. Yük devretme sonrasında yapılandırma](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario1-b.png)

B bölgede bir kesinti oluşursa, birincil ve ikincil veritabanı arasındaki çoğaltma işlemi askıya ancak ikisi arasındaki bağlantı olduğu gibi (1) kalır. Yönetilen trafiği bölge B bağlantı bozuluyor ve bitiş noktası web uygulaması 2 Degraded (2) işaretler algılar. Bu durumda, uygulamanızın performansı etkilenmez, ancak veritabanı açık hale gelir ve bu nedenle veri kaybı durumunda yüksek risk bölge A art arda başarısız olur.

> [!NOTE]
> Olağanüstü durum kurtarma için iki bölgede uygulama dağıtımı sınırlı yapılandırmayla öneririz. Azure farklı coğrafyalara çoğunu yalnızca iki bölgede sahip olmasıdır. Bu yapılandırma, uygulamanızın hem bölgeler bir eşzamanlı geri dönülemez hatasından korumaz. Bu tür bir hatanın olası olayda, veritabanlarınızı üçüncü bölge kullanarak kurtarabilirsiniz [coğrafi geri yükleme işlemi](sql-database-disaster-recovery.md#recover-using-geo-restore).
>

 Kesinti azaltıldığından sonra ikincil veritabanı otomatik olarak birincil ile yeniden eşitler. Eşitleme sırasında birincil performansı etkilenebilir. Belirli etkisi, yük devretme alınan yeni birincil veri miktarına bağlıdır. Aşağıdaki diyagramda bir kesinti ikincil bölge içinde gösterilmektedir:

![Senaryo 1. Kesinti ikincil bölge içinde sonra yapılandırma.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario1-c.png)

Anahtar **avantajları** bu tasarım modeli şunlardır:

* Aynı web uygulamasını hem bölgelere herhangi bir bölgeye özgü yapılandırma olmadan dağıtılır ve yük devretme yönetmek için ilave bir mantık gerektirmez. 
* Yük devretme web uygulaması olarak uygulama performansı etkilenmez ve veritabanı birlikte bulunan her zaman.

Ana **kolaylığını** bölge B uygulama kaynaklarında çoğu zaman gereğinden az olduğu.

## <a name="scenario-2-azure-regions-for-business-continuity-with-maximum-data-preservation"></a>2. Senaryo: Maksimum veri koruma ile iş sürekliliği Azure bölgeleri
Bu seçenek aşağıdaki özelliklere sahip uygulamalar için uygundur:

* Tüm veri kayıplarını yüksek iş sorununa neden olur. Kesinti tarafından geri dönülemez bir hataya neden oluyorsa veritabanı yük devretme yalnızca son çare olarak kullanılabilir.
* Uygulama işlemlerinin salt okunur ve okuma-yazma modlarını destekler ve "salt okunur modda" bir süre çalışabilir.

Okuma-yazma bağlantı zaman aşımı hataları alma başlattığınızda bu modelinde uygulama salt okunur moda geçer. Web uygulaması hem bölgelere dağıtılan ve okuma-yazma dinleyicisi uç noktası bağlantısı ve salt okunur dinleyicisi uç noktası (1) farklı bağlantısı içerir. Trafik Yöneticisi profili kullanması gereken [öncelik yönlendirme](../traffic-manager/traffic-manager-configure-priority-routing-method.md). [Uç noktası izleme](../traffic-manager/traffic-manager-monitoring.md) her bölgede (2) uygulama uç noktası için etkinleştirilmiş olmalıdır.

Aşağıdaki diyagramda bu yapılandırma bir kesinti önce gösterilmektedir:

![Senaryo 2. Kesinti önce yapılandırma.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario2-a.png)

Trafik Yöneticisi bir bölge için bir bağlantı hatası algıladığında, bu otomatik olarak kullanıcı trafiğinin B. bölgede uygulama örneğine geçer Bu desen ile veri kaybı yetkisiz kullanım süresi yeterince yüksek bir değere örneğin 24 saat ayarladığınız önemlidir. Bu süre içinde kesinti azaltıldığından, veri kaybı önlenmiş olur sağlar. Web uygulaması B bölgede etkinleştirildiğinde okuma-yazma işlemleri başarısız olmaya başlar. Bu noktada, salt okunur modda (1) için anahtar. Bu modda istekleri otomatik olarak ikincil veritabanına yönlendirilir. Büyük olasılıkla kesinti tarafından geri dönülemez bir hataya neden oluyorsa, yetkisiz kullanım süresi içinde azaltılamaz. Yük devretme grubu Tetikleyicileri yük devretme süresi bittiğinde. Sonra okuma-yazma dinleyicisi kullanılabilir hale gelir ve (2) başarısız olan bağlantıları durdurun. Aşağıdaki diyagram, kurtarma işlemi iki aşamalarını gösterir.

> [!NOTE]
> Kesinti birincil bölge içinde yetkisiz kullanım süresi içinde azaltılan, trafik Yöneticisi bağlantı geri birincil bölgede algılar ve kullanıcı trafiğinin A. bölgede uygulama örneğine geri geçer Bu uygulama örneği sürdürür ve birincil veritabanı tarafından önceki diyagramda gösterildiği gibi bir bölgede kullanarak okuma-yazma modunda çalışır.
>

![Senaryo 2. Olağanüstü durum kurtarma aşamaları.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario2-b.png)

B bölgede bir kesinti oluşursa, trafik Yöneticisi uç noktası web-app-2 başarısızlığını bölge B ve işaretlerini (1) düşürülmüş algılar. Bu arada, yük devretme grubu salt okunur dinleyicisi (2) A bölgeye geçer. Bu kesinti son kullanıcı deneyimi olumsuz etkilemez ancak birincil veritabanı kesinti sırasında sunulur. Aşağıdaki diyagram, ikincil bölge içinde bir hata gösterir:

![Senaryo 2. İkincil bölge kesinti.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario2-c.png)

Kesinti azaltıldığından sonra ikincil veritabanını hemen birincil ile eşitlenir ve salt okunur dinleyicisi B. bölgede ikincil veritabanına geri çevrilir Eşitleme sırasında birincil performansını eşitlenmesi için gereken veri miktarı bağlı olarak biraz etkilenebilir.

Bu tasarım deseni vardır **avantajları**:

* Geçici kesintileri sırasında veri kaybı ortadan kaldırır.
* Kapalı kalma süresi yalnızca nasıl hızlı bir şekilde trafik Yöneticisi yapılandırılabilir bağlantı hatası algılar üzerinde bağlıdır.

**Kolaylığını** uygulama salt okunur modda çalışabilir olması gerekir.

## <a name="scenario-3-application-relocation-to-a-different-geography-without-data-loss-and-near-zero-downtime"></a>Senaryo 3: Veri kaybı olmadan ve sıfır kapalı kalma süresi yakın farklı bir coğrafi konum uygulama yeniden konumlandırma 
Bu senaryoda, uygulama aşağıdaki özelliklere sahiptir: 
* Son kullanıcılar farklı coğrafyalara uygulamaya erişim
* Son güncelleştirmeleri içeren tam eşitleme dayanmayan salt okunur iş yükleri uygulama içerir
* Veri yazma erişimi için kullanıcıların çoğunluğunun aynı Coğrafya desteklenmesi gereken 
* Okuma gecikmesi için son kullanıcı deneyimi kritik 


Kullanıcı cihazı güvence altına almak gereken bu gereksinimleri karşılamak için **her zaman** gözatma verileri gibi salt okunur işlemler için aynı Coğrafya dağıtılan uygulamayı bağlandığı analytics vb. OLTP işlemleri aynı Coğrafya işlenir ancak **çoğu zaman**. Örneğin, günlük süre içinde aynı coğrafi bölge OLTP işlemleri işlenir ancak kapalı saatlerde bunlar farklı bir Coğrafya işlenemedi. Son kullanıcı etkinliği çoğunlukla çalışma saatlerinde olursa, en iyi performans için kullanıcıların çoğunun garanti edebilir çoğu zaman. Aşağıdaki diyagramda bu topoloji gösterilmektedir. 
 
Uygulamanın kaynaklarını önemli kullanım isteğe bağlı olduğu her Coğrafya dağıtılmalıdır. Uygulamanızı etkin olarak Amerika Birleşik Devletleri'nde kullandıysanız, örneğin, Avrupa Birliği ve Güneydoğu Asya uygulamanın tüm bu coğrafyalara dağıtılmalıdır. Birincil veritabanı dinamik olarak bir Coğrafya'dan sonraki çalışma saatleri sona erdiğinde geçirilmesi. Bu yöntem, "sun takip" adı verilir. OLTP iş yükü veritabanını okuma-yazma dinleyicisi aracılığıyla her zaman bağlandığı  **&lt;yük devretme grup adı&gt;. database.windows.net** (1). Salt okunur iş yükü veritabanları sunucusu uç noktası kullanarak doğrudan yerel veritabanına bağlanan  **&lt;sunucu adı&gt;. database.windows.net** (2). Trafik Yöneticisi ile yapılandırıldığında [performans yönlendirme yöntemini](../traffic-manager/traffic-manager-configure-performance-routing-method.md). Bu, son kullanıcının aygıtı en yakın bölgeyi web hizmetinde bağlandığı sağlar. Trafik Yöneticisi uç noktası izleme her web hizmeti uç noktası (3) etkin ayarlanmalıdır.

> [!NOTE]
> Yük devretme grubu yapılandırmasını yük devretme için hangi bölgede kullanıldığını tanımlar. Etkilenen bölge tekrar çevrimiçi olana kadar yeni birincil farklı bir Coğrafya OLTP ve salt okunur iş yükleri için daha uzun gecikme yük devretme sonuçlarında olduğundan.
>

![Senaryo 3. Doğu ABD birincil yapılandırmayla.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario3-a.png)

(Örneğin, yerel saat 23: 00) günün sonunda etkin veritabanları sonraki bölgeyi (Kuzey Avrupa) geçirilmesi. Bu görev kullanarak tam olarak otomatikleştirilebilir [Azure Hizmet zamanlama](../scheduler/scheduler-intro.md).  Görev aşağıdaki adımları içerir:
* Anahtar birincil sunucu grubundaki kolay yük devretme (1) kullanarak yük devretme Kuzey Avrupa
* Doğu ABD Kuzey Avrupa arasındaki yük devretme grubunu Kaldır
* Aynı ada sahip ancak Kuzey Avrupa ve Doğu Asya'da (2) arasında yeni bir yük devretme grubu oluşturun. 
* Birincil Kuzey Avrupa ve Doğu Asya'da ikincil Bu yük devretme grubuna (3) ekleyin.


Aşağıdaki diyagram, planlı yük devretme sonrasında yeni yapılandırmayı gösterir:

![Senaryo 3. Kuzey Avrupa için birincil geçirme.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario3-b.png)

Bir kesinti Kuzey Avrupa'da örneğin olursa, otomatik veritabanı yük devretme etkili bir şekilde (1) zamanlamanın sonraki bölgeyi uygulamaya taşıma sonuçları yük devretme grubu tarafından başlatılır.  Bu durumda Kuzey Avrupa tekrar çevrimiçi olana kadar ABD Doğu yalnızca kalan ikincil bölge ' dir. Kalan iki bölgede, tüm üç coğrafyalara müşteriler rolleri geçerek hizmet. Azure Zamanlayıcı buna uygun olarak ayarlanması gerekir. Kalan bölgeler Avrupa ek kullanıcı trafiği aldığından, uygulamanın performansını yalnızca ek gecikme ancak aynı zamanda tarafından son kullanıcı bağlantıları sayısının artması etkilenmez. Kesinti Kuzey Avrupa'da azaltılan sonra ikincil veritabanı hemen geçerli birincil ile eşitlenir. Aşağıdaki diyagram, Kuzey Avrupa'da bir kesinti gösterir:

![Senaryo 3. Kuzey Avrupa kesinti.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario3-c.png)

> [!NOTE]
> Son kullanıcı deneyimi Avrupa'da uzun gecikme tarafından ne zaman düşürülmüş süresini azaltabilir. Aldığınız yapmak için proaktif olarak bir uygulama kopya dağıtmak ve ikincil veritabanlarını başka bir yerel bölgede (Batı Avrupa) yenisini Kuzey Avrupa çevrimdışı Uygulama örneğinin olarak oluşturun. İkinci tekrar çevrimiçi olduğunda, Batı Avrupa kullanmaya devam etmek için veya uygulama var. kopyasını kaldırıp yeniden Kuzey Avrupa kullanmaya geçiş karar verebilir,
>

Anahtar **avantajları** bu tasarımını şunlardır:
* Salt okunur uygulama iş yükü her zaman odaları bölgede bulunan verilere erişiyor. 
* Her Coğrafya en yüksek etkinlik dönemi sırasında okuma-yazma uygulama iş yükü en yakın bölgeyi verilere eriştiğinde
* Uygulama için birden çok bölgeye dağıtıldığı için önemli kapalı kalma süresi olmadan bölgelerinden kaybı varlığını sürdürmesini. 

Ancak bazı **dengelerin**:
* Bölgesel bir kesintinin Coğrafya uzun gecikmesine etkilenmiş sonuçlanır. Hem okuma-yazma ve salt okunur iş yükleri farklı coğrafi konum uygulama tarafından sunulan. 
* Salt okunur iş yükleri için farklı bir uç noktası her bölgede bağlanmanız gerekir. 


## <a name="business-continuity-planning-choose-an-application-design-for-cloud-disaster-recovery"></a>İş sürekliliği planlama: Bulut olağanüstü durum kurtarma için bir uygulama tasarımı seçin
Özel bulut olağanüstü durum kurtarma stratejiniz birleştirebilir veya en iyi uygulamanızın ihtiyaçlarını karşılamak için bu tasarım desenleri genişletir.  Daha önce belirtildiği gibi seçtiğiniz stratejisi müşterileriniz ve uygulama dağıtım topolojisi sunmak isteyebilirsiniz SLA temel alır. Kararınızı yol göstermesi için kurtarma noktası hedefi (RPO) ve tahmini kurtarma saatini (Ekle) temel alan seçenekleri aşağıdaki tabloda karşılaştırır.

| Desen | RPO | EKLE |
|:--- |:--- |:--- |
| Aktif-Pasif dağıtım birlikte bulunan veritabanı erişimi olan olağanüstü durum kurtarma |Okuma-yazma erişimi < 5 saniye |Hata algılama süresi + DNS TTL |
| Uygulama Yük Dengeleme için etkin-etkin dağıtım |Okuma-yazma erişimi < 5 saniye |Hata algılama süresi + DNS TTL |
| Veri koruma için Aktif-Pasif dağıtımı |Salt okunur erişim < 5 saniye | Salt okunur erişim = 0 |
||Okuma-yazma erişimi sıfır = | Okuma-yazma erişimi = hatası algılama zamanı + veri kaybı yetkisiz kullanım süresi |
|||

## <a name="next-steps"></a>Sonraki adımlar
* İş sürekliliğine genel bakış ve senaryolar için bkz: [iş sürekliliğine genel bakış](sql-database-business-continuity.md)
* Coğrafi çoğaltma ve yük devretme grupları hakkında bilgi edinmek için [aktif coğrafi çoğaltma](sql-database-geo-replication-overview.md)  
* Esnek havuzlar etkin coğrafi çoğaltma hakkında daha fazla bilgi için bkz: [esnek havuz olağanüstü durum kurtarma stratejilerini](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
