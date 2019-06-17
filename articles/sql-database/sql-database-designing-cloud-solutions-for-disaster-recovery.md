---
title: Azure SQL veritabanı ile küresel olarak kullanılabilir hizmetler tasarlama | Microsoft Docs
description: Azure SQL veritabanı ile yüksek oranda kullanılabilir hizmetler için uygulama tasarımı hakkında bilgi edinin.
keywords: Bulut olağanüstü durum kurtarma, olağanüstü durum kurtarma çözümleri, uygulama veri yedekleme, coğrafi çoğaltma, iş sürekliliği planlama
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: 46232afcaf9504d4cfbd80160e2d7e7ea958d600
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61488213"
---
# <a name="designing-globally-available-services-using-azure-sql-database"></a>Azure SQL veritabanı ile küresel olarak kullanılabilir hizmetler tasarlama

Azure SQL veritabanı ile bulut hizmetlerini derleyip dağıtmayı, kullandığınız [etkin coğrafi çoğaltma](sql-database-active-geo-replication.md) veya [otomatik yük devretme grupları](sql-database-auto-failover-group.md) bölgesel kesintiler ve olağanüstü durumlara hataları esnekliği sağlamak için. Aynı özellik, yerel veri erişimi için en iyi duruma getirilmiş Global olarak dağıtılmış uygulamalar oluşturmanıza olanak sağlar. Bu makalede, avantaj ve dezavantajlarına her bir seçeneğin dahil olmak üzere genel uygulama desenleri açıklar.

> [!NOTE]
> Premium veya iş açısından kritik veritabanları ve elastik havuzlar kullanıyorsanız, bunları dayanıklı bölgesel kesintiler için bunları bölge yedekli dağıtım yapılandırması için dönüştürme yapabilirsiniz. Bkz: [bölgesel olarak yedekli veritabanları](sql-database-high-availability.md).  

## <a name="scenario-1-using-two-azure-regions-for-business-continuity-with-minimal-downtime"></a>Senaryo 1: En düşük kapalı kalma süresi ile iş sürekliliği için iki Azure bölgeleri kullanma

Bu senaryoda, uygulamalar, aşağıdaki özelliklere sahiptir:

* Uygulamayı bir Azure bölgesinde etkin
* Tüm veritabanı oturumları okuma ve yazma erişimi (RW) verileri gerektirir.
* Web Katmanı ve veri katmanı gecikme süresi ve trafiği maliyetini azaltmak için birlikte bulunan gerekir
* Temelde, kapalı kalma süresi daha yüksek iş uygulamalar için daha veri kaybı risktir

Bu durumda, uygulama dağıtım topolojisi, tüm uygulama bileşenleri için yük devretme birlikte gerektiğinde bölgesel felaketler işlemek için optimize edilmiştir. Aşağıdaki diyagramda, bu topoloji gösterilmektedir. A ve b bölgesine dağıtılan coğrafi yedeklilik için uygulama kaynakları Ancak, bölge bir çalışana kadar bölge B kaynakları kullanılmaz. Bir yük devretme grubu, veritabanı bağlantısı, çoğaltma ve yük devretme yönetmek için iki bölgeleri arasında yapılandırılır. Her iki bölgede de web hizmeti veritabanı okuma / yazma dinleyici aracılığıyla erişmek için yapılandırılmış  **&lt;yük devretme grubu adı&gt;. database.windows.net** (1). Traffic manager kullanmak üzere ayarlandı [öncelikli yönlendirme yöntemini](../traffic-manager/traffic-manager-configure-priority-routing-method.md) (2).  

> [!NOTE]
> [Azure traffic manager](../traffic-manager/traffic-manager-overview.md) Bu makale boyunca yalnızca gösterim amacıyla kullanılır. Öncelikli yönlendirme yöntemini destekleyen hiçbir yük dengeleme çözümü kullanabilirsiniz.

Bu yapılandırma kesinti önce aşağıdaki diyagramda gösterilmiştir:

![Senaryo 1. Kesinti önce yapılandırma.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario1-a.png)

Birincil bölgede kesinti sonrasında, SQL veritabanı hizmeti, birincil veritabanı erişilebilir değil ve otomatik yük devretme İlkesi (1) parametreleri temel alarak ikincil bölgeye yük devretmeyi tetikler algılar. Uygulama SLA bağlı olarak, kesinti algılama ve yük devretme kendisini arasındaki süreyi denetleyen bir yetkisiz kullanım süresi yapılandırabilirsiniz. Yük devretme grubuna veritabanının yük devretmeyi tetiklemeden önce traffic manager uç nokta yük devretme başlatır mümkündür. Bu durumda web uygulaması için veritabanını hemen yeniden bağlanamaz. Ancak veritabanı yük devretme tamamlandıktan hemen sonra tutarsızlıklara otomatik olarak başarılı olur. Başarısız bölge, geri yüklenen ve tekrar çevrimiçi olduğunda, eski birincil yeni bir ikincil otomatik olarak yeniden bağlanır. Aşağıdaki diyagramda, yük devretme işleminden sonra yapılandırmayı gösterir.

> [!NOTE]
> Yük devretme işleminden sonra kaydedilen tüm işlem yeniden bağlanma sırasında kaybolur. Yük devretme tamamlandıktan sonra B bölgede yeniden ve kullanıcı isteklerini işleme yeniden uygulamasıdır. Hem web uygulaması hem de birincil veritabanı artık B bölgenizdedir ve birlikte bulunan kalır.

![Senaryo 1. Yük devretmeden sonra yapılandırma](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario1-b.png)

B bölgede kesinti olursa, birincil ve ikincil veritabanı arasındaki çoğaltma işlemi askıya ancak ikisi arasındaki bağlantıyı (1) olduğu gibi kalır. Yönetilen trafiği bölge B bağlantısı kesilir ve uç nokta web uygulaması 2 (2) Degraded işaretler algılar. Bu durumda, uygulama performansını etkilenmez ancak veritabanı açık hale gelir ve bu nedenle daha yüksek risk veri kaybı durumunda bir bölge art arda başarısız olur.

> [!NOTE]
> Olağanüstü durum kurtarma için iki bölgeleri için sınırlı uygulama dağıtımı ile yapılandırmayı öneririz. Azure coğrafyaları çoğunu yalnızca iki bölgeleri sahip olmasıdır. Bu yapılandırma, her iki bölgeleri bir eş zamanlı yıkıcı hatasından uygulamanızı korumaz. Böyle bir hatanın yaşandığı nadir, üçüncü bir bölge kullanmaya veritabanlarınızı kurtarabilirsiniz [coğrafi geri yükleme işlemi](sql-database-disaster-recovery.md#recover-using-geo-restore).
>

 Kesinti giderildikten sonra ikincil veritabanı birincil ile otomatik olarak yeniden eşitler. Eşitleme sırasında birincil performansı etkilenebilir. Belirli etkisi, yük devretme alınan yeni birincil veri miktarına bağlıdır. Aşağıdaki diyagram, ikincil bölgede kesinti gösterir:

![Senaryo 1. Yapılandırmadan sonra ikincil bölgedeki bir kesinti.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario1-c.png)

Anahtar **avantajları** bu tasarım modeli şunlardır:

* Aynı web uygulamasını herhangi bir bölgeye özgü yapılandırma olmadan her iki bölgede dağıtılır ve yük devretme yönetmek için ilave bir mantık gerektirmez.
* Yük devretme için web uygulaması olarak uygulama performansı etkilenmez ve veritabanı her zaman birlikte bulunur.

Ana **tradeoff** bölge B uygulama kaynaklarında çoğu zaman gereğinden az olduğu.

## <a name="scenario-2-azure-regions-for-business-continuity-with-maximum-data-preservation"></a>Senaryo 2: Maksimum veri koruma ile iş sürekliliği için Azure bölgeleri

Bu seçenek aşağıdaki özelliklere sahip uygulamalar için idealdir:

* Veri kaybını yüksek iş riski oluşturur. Kesinti tarafından geri dönülemez bir hataya neden oluyorsa veritabanı yük devretme yalnızca son çare olarak kullanılabilir.
* Uygulama, işlemleri salt okunur ve okuma-yazma modunu destekler ve "salt okunur modda" bir süre çalışabilir.

Okuma-yazma bağlantı zaman aşımı hataları alıyorsanız başlattığınızda bu düzende, uygulama için salt okunur moda geçer. Web uygulaması için her iki bölgede dağıtılır ve okuma / yazma dinleyici uç noktası bağlantı ve salt okuma dinleyici uç noktası (1) için farklı bir bağlantı içerir. Traffic manager profili kullanmalısınız [öncelikli yönlendirme](../traffic-manager/traffic-manager-configure-priority-routing-method.md). [Uç nokta izleme](../traffic-manager/traffic-manager-monitoring.md) uygulama uç noktası (2) her bölgedeki etkinleştirilmesi gerekir.

Aşağıdaki diyagram, kesinti önce bu yapılandırmayı gösterir:

![Senaryo 2. Kesinti önce yapılandırma.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario2-a.png)

Traffic manager bir bölge için bir bağlantı hatası algıladığında otomatik olarak kullanıcı trafiğinin bölgede b Uygulama örneğinin geçer Bu düzende, veri kaybı yetkisiz kullanım süresi yeterince yüksek bir değere örneğin 24 saat ayarlamanız önemlidir. Bu süre içinde kesinti giderildikten, veri kaybı önlenmiş olur sağlar. Web uygulaması B bölgede etkinleştirildiğinde okuma-yazma işlemleri başarısız olmaya başlar. Bu noktada, salt okunur moda (1) geçiş. Bu modda istekleri ikincil veritabanına otomatik olarak yönlendirilir. Büyük olasılıkla kesinti tarafından geri dönülemez bir hataya neden oluyorsa, yetkisiz kullanım süresi içinde azaltılamaz. Ne zaman yük devretme yük devretme grubu Tetikleyiciler süresi. Sonra okuma-yazma dinleyici kullanılabilir hale gelir ve (2) başarısız olan bağlantıları durdurun. Aşağıdaki diyagram, kurtarma işlemi iki aşamalarını gösterir.

> [!NOTE]
> Kesinti birincil bölgedeki yetkisiz kullanım süresi içinde azalır, traffic manager birincil bölgeye geri bağlantısının algılar ve kullanıcı trafiğinin bölgede A. uygulama örneğini geri döner Bu uygulama örneğini sürdürür ve birincil veritabanı tarafından önceki diyagramda gösterildiği gibi bir bölgede kullanarak okuma-yazma modunda çalışır.

![Senaryo 2. Olağanüstü durum kurtarma aşamaları.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario2-b.png)

B bölgede kesinti olursa, traffic manager uç noktası web-app-2 hatasını bölge B ve işaretlerini (1) düşürülmüş algılar. Bu arada, salt okunur dinleyicisi (2) bir bölgeye yük devretme grubuna geçer. Bu kesinti son kullanıcı deneyimi etkilemez, ancak birincil veritabanı kesinti sırasında sunulur. Aşağıdaki diyagram, ikincil bölgede bir hata gösterir:

![Senaryo 2. İkincil bölgeye kesinti.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario2-c.png)

Kesinti giderildikten sonra ikincil veritabanını hemen birincil ile eşitlenir ve salt okunur dinleyici b bölgede ikincil veritabanına geçiş Eşitleme sırasında performans birincil eşitlenmesi gereken verilerin miktarına bağlı olarak biraz etkilenebilir.

Bu tasarım deseni vardır **avantajları**:

* Bu, geçici kesintiler sırasında veri kaybı önler.
* Kapalı kalma süresi, yalnızca ne kadar hızlı traffic manager yapılandırılabilir bağlantı hatası algılar üzerinde bağlıdır.

**Tradeoff** uygulama salt okunur modda çalışabilir olmalıdır.

## <a name="scenario-3-application-relocation-to-a-different-geography-without-data-loss-and-near-zero-downtime"></a>Senaryo 3: Farklı bir Coğrafya veri kaybı olmadan ve neredeyse sıfır kapalı kalma süresi için uygulamayı yeniden konumlandırma

Bu senaryoda uygulama aşağıdaki özelliklere sahiptir:

* Son kullanıcılar, farklı coğrafyalara uygulamaya erişim
* Uygulama en son güncelleştirmeleri içeren tam eşitleme bağlı olmayan bir salt okunur iş yüklerini içerir.
* Kullanıcıların çoğu için aynı coğrafi veri yazma erişimi desteklenmelidir.
* Okuma gecikme süresi için son kullanıcı deneyimi önemlidir

Kullanıcı cihazı güvence altına almak gereken bu gereksinimleri karşılamak için **her zaman** gözatma verileri gibi salt okunur işlemler için aynı Coğrafya dağıtılan uygulamayı bağlandığı analytics vb. OLTP işlemleri aynı Coğrafya işlenir ancak **çoğu zaman**. Örneğin, günlük süre boyunca aynı Coğrafya OLTP operations işlenir ancak kapalı saatleri sırasında bunlar farklı bir coğrafi bölge işlenemedi. Son kullanıcı etkinliği çoğunlukla çalışma saatlerinde olursa, en iyi performans için kullanıcıların çoğunun garanti çoğu zaman. Aşağıdaki diyagramda, bu topoloji gösterilmektedir.

Önemli bir kullanım isteğe sahip olduğu her bir coğrafi konum uygulama kaynaklarının dağıtılması gerekir. Uygulamanız etkin olarak Amerika Birleşik Devletleri'nde kullandıysanız, örneğin, Avrupa Birliği ve Güneydoğu Asya uygulamanın tüm bu coğrafyalar için dağıtılmalıdır. Birincil veritabanının dinamik olarak bir Coğrafya'dan sonraki çalışma saatlerini sonunda geçiş. Bu yöntem, "Güneş İzle" adı verilir. OLTP iş yükü okuma / yazma dinleyici aracılığıyla veritabanı her zaman bağlandığı  **&lt;yük devretme grubu adı&gt;. database.windows.net** (1). Salt okunur iş yükü veritabanları sunucu uç noktasını kullanarak doğrudan yerel veritabanına bağlar  **&lt;sunucu-adı&gt;. database.windows.net** (2). Traffic manager ile yapılandırılmış [performans yönlendirme yöntemini](../traffic-manager/traffic-manager-configure-performance-routing-method.md). Bu, son kullanıcının cihazında web hizmetine en yakın bölgede bağlandığını sağlar. Traffic manager uç noktası ile izleme her web hizmeti uç noktası (3) etkin ayarlanmalıdır.

> [!NOTE]
> Yük devretme grubu yapılandırması, yük devretme için hangi bölgeyi kullanıldığını tanımlar. Etkilenen bölge yeniden çevrimiçi olana kadar yeni birincil farklı bir coğrafi bölge OLTP ve salt okunur iş yükleri için daha uzun gecikme yük devretme sonuçları olduğundan.

![Senaryo 3. Yapılandırması ile birincil Doğu ABD.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario3-a.png)

(Örneğin, yerel saat 23: 00) günün sonunda sonraki bölge (Kuzey Avrupa) etkin veritabanları geçiş. Bu görevi kullanılarak tamamen otomatikleştirilebilir [Azure Hizmet zamanlama](../scheduler/scheduler-intro.md).  Görev, aşağıdaki adımları içerir:

* Kolay yük devretme (1) kullanarak, Kuzey Avrupa için yük devretme grubunda birincil sunucu anahtarı
* Doğu ABD ve Kuzey Avrupa arasında yük devretme grubu Kaldır
* Aynı ada sahip ancak Kuzey Avrupa ve Doğu Asya (2) arasında yeni bir yük devretme grubu oluşturun.
* Birincil, Kuzey Avrupa ve Doğu Asya'da ikincil Bu yük devretme grubuna (3) ekleyin.

Aşağıdaki diyagramda planlanan yük devretme sonrasında yeni yapılandırmayı gösterir:

![Senaryo 3. Kuzey Avrupa için birincil geçirme.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario3-b.png)

Kuzey Avrupa'da bir kesinti olur, örneğin, otomatik veritabanı yük devretme etkili bir şekilde uygulama (1) zamanlamanın sonraki bölgeye taşıma sonuçları yük devretme grubu tarafından başlatılır.  Bu durumda Kuzey Avrupa tekrar çevrimiçi olana kadar ABD Doğu yalnızca kalan ikincil bölge ' dir. Kalan iki bölgeleri üç tüm coğrafyalardaki müşteriler, rolleri geçerek işlevi görür. Azure Zamanlayıcı uygun şekilde ayarlanması gerekir. Kalan bölgeleri Avrupa'dan ek kullanıcı trafiğinin aldığından, yalnızca ek gecikme ancak aynı zamanda tarafından son kullanıcı bağlantıları çok sayıda uygulama performansını etkilenmez. Kuzey Avrupa, kesinti giderildikten sonra ikincil veritabanı hemen geçerli birincil ile eşitlenir. Aşağıdaki diyagramda, Kuzey Avrupa bölgesinde kesinti gösterilmektedir:

![Senaryo 3. Kuzey Avrupa bölgesinde kesinti.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario3-c.png)

> [!NOTE]
> Avrupa son kullanıcı deneyimi uzun gecikme süresinden ne zaman düşürülmüş süreyi kısaltabilirsiniz. Bunu yapmak için proaktif olarak uygulama kopyalama dağıtma ve Kuzey Avrupa'da çevrimdışı Uygulama örneğinin yerine başka bir yerel bölgede (Batı Avrupa) ikincil veritabanları oluşturun. İkinci tekrar çevrimiçi olduğunda, Batı Avrupa kullanmaya devam etmek için veya uygulamayı kopyasını kaldırın ve Kuzey Avrupa kullanmaya geçmek karar verebilirsiniz.

Anahtar **avantajları** bu tasarımı şunlardır:

* Salt uygulama iş yükünü, her zaman odaları bölgede erişir.
* Okuma-yazma uygulama iş yükünü en yakın bölgede her coğrafi yüksek etkinlik dönemi boyunca erişir.
* Uygulama birden fazla bölgeye dağıtılır çünkü önemli kapalı kalma süresi olmadan bölgelerden birine kaybı hayatta kalamaz.

Vardır, ancak bazı **ödünler**:

* Daha uzun gecikme süresinden etkilenmesine coğrafi bölgesel bir kesinti olur. Hem okuma-yazma ve salt okunur iş yükleri tarafından uygulamanın farklı bir coğrafi bölgesinden.
* Salt okunur iş yükleri, her bölgede farklı bir uç noktasına bağlanmanız gerekir.

## <a name="business-continuity-planning-choose-an-application-design-for-cloud-disaster-recovery"></a>İş sürekliliği planlama: Bulutta olağanüstü durum kurtarma için bir uygulama tasarımı seçin

Özel bulut olağanüstü durum kurtarma stratejiniz birleştirebilir veya en iyi uygulamanızın ihtiyaçlarını karşılamak için bu tasarım desenleri.  Daha önce bahsedildiği gibi seçtiğiniz strateji müşterileriniz ve uygulama dağıtım topolojisi sunmak istediğiniz SLA'sına bağlıdır. Kararınız rehberlik için kurtarma noktası hedefi (RPO) ve tahmini kurtarma süresi (ERT) göre seçenekleri aşağıdaki tabloda karşılaştırılmıştır.

| Desen | RPO | ERT |
|:--- |:--- |:--- |
| Birlikte bulunan veritabanı erişimi ile olağanüstü durum kurtarma için Aktif-Pasif dağıtım |Okuma-yazma erişimi < 5 sn |Hata algılama süresi + DNS TTL'si |
| Uygulama Yük Dengelemesi için etkin-etkin dağıtım |Okuma-yazma erişimi < 5 sn |Hata algılama süresi + DNS TTL'si |
| Veri koruma için Aktif-Pasif dağıtım |Salt okunur erişim < 5 sn | Salt okunur erişim = 0 |
||Okuma-yazma erişimi sıfır = | Okuma-yazma erişimi = hata algılama zamanı + veri kaybı yetkisiz kullanım süresi |
|||

## <a name="next-steps"></a>Sonraki adımlar

* İş sürekliliğine genel bakış ve senaryolar için bkz: [iş sürekliliğine genel bakış](sql-database-business-continuity.md)
* Etkin coğrafi çoğaltma hakkında bilgi edinmek için [etkin coğrafi çoğaltma](sql-database-active-geo-replication.md).
* Otomatik Yük devretme grupları hakkında daha fazla bilgi için bkz: [otomatik yük devretme grupları](sql-database-auto-failover-group.md).
* Etkin coğrafi çoğaltma ile elastik havuzları hakkında daha fazla bilgi için bkz. [elastik havuz olağanüstü durum kurtarma stratejileri](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
