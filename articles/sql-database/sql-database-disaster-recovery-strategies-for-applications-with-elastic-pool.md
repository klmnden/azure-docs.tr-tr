---
title: Olağanüstü durum kurtarma çözümleri - Azure SQL veritabanı tasarım | Microsoft Docs
description: Bulut çözümünüz olağanüstü durum kurtarma için doğru yük düzeni seçerek tasarım öğrenin.
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: article
ms.date: 04/04/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 1f2f0819f987bf389ff4b2816ad422fdd8a81f82
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="disaster-recovery-strategies-for-applications-using-sql-database-elastic-pools"></a>SQL Database esnek havuzları kullanan uygulamalar için olağanüstü durum kurtarma stratejileri
Yıllar içinde biz bulut Hizmetleri kusursuz değildir ve geri dönülemez olaylar gerçekleşir öğrendiniz. SQL veritabanı bu olaylar meydana geldiğinde, uygulamanızı iş sürekliliği sağlamak için çeşitli özellikleri sağlar. [Esnek havuzlar](sql-database-elastic-pool.md) ve olağanüstü durum kurtarma özellikleri aynı türde tek veritabanlarını destekler. Esnek havuz için bu makalede birkaç DR stratejilerini açıklar. Bu SQL veritabanını iş sürekliliği özelliklerden yararlanın.

Bu makalede aşağıdaki kurallı SaaS ISV uygulama deseni kullanır:

<i>Modern bulut tabanlı web uygulamasının her son kullanıcı için bir SQL veritabanı sağlar. ISV birçok müşteri vardır ve bu nedenle Kiracı veritabanları olarak bilinen birçok veritabanı kullanır. Kiracı veritabanları genellikle öngörülemeyen etkinlik desenlerini sahip olduğundan, ISV bir esnek havuz maliyet veritabanını uzun süreler boyunca çok tahmin edilebilir yapmak için kullanır. Kullanıcı etkinliği ani olduğunda esnek havuz da performans yönetimini basitleştirir. Kiracı veritabanları yanı sıra uygulama da birden fazla veritabanı kullanıcı profillerini, güvenliği yönetmek, kullanım desenlerini vb. toplamak için kullanır. Tek tek kiracılar kullanılabilirliğini bütün olarak uygulamanın kullanılabilirliğini etkilemez. Ancak, kullanılabilirliğini ve performansını yönetim veritabanları için uygulamanın işlevi kritik ve yönetim veritabanlarını çevrimdışı olduğunda tüm uygulama çevrimdışı olur.</i>  

Bu makalede olanları sıkı kullanılabilirlik gereksinimleri olan maliyet hassas başlangıç uygulamaları senaryolarından aralığını kapsayan DR stratejiler açıklanmaktadır.

> [!NOTE]
> Premium kullanıyorsanız veya iş kritik (Önizleme) veritabanları ve esnek havuzlar, yapabilirsiniz bunları dayanıklı bölgesel kesintileri (şu anda önizlemede) bölge olarak yedekli dağıtım yapılandırması için dönüştürerek. Bkz: [bölge olarak yedekli veritabanları](sql-database-high-availability.md).

## <a name="scenario-1-cost-sensitive-startup"></a>Senaryo 1. Hassas başlangıç maliyeti
<i>Bir başlangıç iş 'M ve son derece hassas maliyet bildirimi.  Dağıtım ve uygulama yönetimini basitleştirmek istiyorum ve tek tek müşteriler için sınırlı bir SLA olabilir. Ancak bir bütün hiçbir zaman çevrimdışı olduğu gibi uygulama emin olmak istersiniz.</i>

Basitlik gereksinimi karşılamak için tercih ettiğiniz Azure bölgesindeki bir esnek havuz içine tüm Kiracı veritabanları dağıtmak ve yönetim veritabanları coğrafi olarak çoğaltılmış tek veritabanları olarak dağıtın. Kiracılar olağanüstü durum kurtarma için coğrafi hiçbir ek ücret gelen geri yükleme, kullanın. Yönetim veritabanlarını kullanılabilirliğini sağlamak için coğrafi çoğaltılacağı bir otomatik yük devretme kullanarak başka bir bölgeye gruplandırın (içinde-önizleme) (1. adım). Bu senaryoda olağanüstü durum kurtarma yapılandırmasının devam eden maliyeti toplam sahip olma maliyeti ikincil veritabanları için eşittir. Bu yapılandırma sonraki diyagramda gösterilmiştir.

![Şekil 1](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-1.png)

Birincil bölgede bir kesinti oluşursa, Kurtarma adımları uygulamanız çevrimiçi duruma getirmek için sonraki diyagram tarafından gösterilmiştir.

* Yük devretme grubu otomatik yük devretme yönetim veritabanının DR bölgeye başlatır. Uygulama otomatik olarak yeni birincil ve tüm yeni hesapları kurulana ve Kiracı veritabanları DR bölgede oluşturulur. Var olan müşteriler geçici olarak devre dışı verilerini bakın.
* Özgün havuz (2) aynı yapılandırmaya sahip esnek havuz oluşturun.
* Coğrafi geri yükleme, Kiracı kopyalarını veritabanları (3) oluşturmak için kullanın. Son kullanıcı bağlantılar tarafından tek tek geri yüklemeler tetikleme göz önünde bulundurun veya başka bir uygulamaya özgü Öncelik düzenini kullanın.


Bu noktada, uygulamanızın DR bölgede tekrar çevrimiçi değil, ancak bazı müşteriler kendi verilere erişirken gecikme olur.

![Şekil 2](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-2.png)

Kesinti geçici varsa, tüm veritabanı geri yüklemeler DR bölgede tam önce birincil bölge Azure tarafından kurtarılır mümkündür. Bu durumda, birincil bölge uygulamaya geri taşıma yönetirler. İşlemin sonraki diyagramda gösterildiği tedbirleri alır.

* Tüm bekleyen coğrafi geri yükleme istekleri iptal edin.   
* Yönetim veritabanları birincil bölge (5) için yük devri. Bölgenin kurtarma işleminden sonra eski ana otomatik olarak ikincil hale getirildi. Şimdi bunların rolleri yeniden geçin. 
* Birincil bölgesine işaret etmek için uygulamanın bağlantı dizesini değiştirin. Şimdi tüm yeni hesaplarını ve Kiracı veritabanları birincil bölgede oluşturulur. Bazı var olan müşteriler geçici olarak devre dışı verilerini bakın.   
* Salt okunur DR bölgede (6) değiştirilemez emin olmak için DR havuzdaki tüm veritabanları ayarlayın. 
* Her bir veritabanına kurtarma bu yana değişti DR havuzunda için yeniden adlandırın veya birincil havuzunda (7) karşılık gelen veritabanlarını silin. 
* Güncelleştirilmiş veritabanları DR havuzundan birincil havuzu (8) kopyalayın. 
* DR havuzu (9) Sil

Bu noktada, uygulamanız tüm Kiracı veritabanları ile kullanılabilir birincil havuzunda birincil bölgede çevrimiçidir.

![Şekil 3](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-3.png)

Anahtar **yararlı** bu strateji düşük devam eden veri katmanı artıklığı maliyetidir. Yedeklemeler, hiçbir ek ücret ödemeden hiçbir uygulama yeniden yazma ile SQL veritabanı hizmeti tarafından otomatik olarak alınır.  Maliyet esnek veritabanları yalnızca geri yüklendiğinde oluşur. **Dengelemeyi** tüm Kiracı veritabanlarının tam kurtarma önemli zaman gerçekleştirir. Toplam süre bağlıdır DR bölgede başlatmak geri yüklemeler sayısı ve Kiracı veritabanları genel boyutu. Bazı kiracılar geri yüklemeler diğerlerine göre daha öncelikli olsa bile, hizmet istemlerde ve var olan müşteriler veritabanlarında genel etkisini en aza indirmek için kısıtlar olarak aynı bölgede başlatılan tüm diğer geri yükleme ile rekabet ediyor. Ayrıca, yeni bir esnek havuz DR bölgede oluşturulana kadar Kiracı veritabanları kurtarma başlatamıyor.

## <a name="scenario-2-mature-application-with-tiered-service"></a>Senaryo 2. Katmanlı hizmetiyle olgun uygulama
<i>Katmanlı hizmet teklifleri ve deneme müşteriler ve müşterilerin ödeme için farklı SLA ile olgun bir SaaS uygulaması ben. Deneme müşteriler için mümkün olduğunca maliyetini azaltmak sahibim. Deneme müşteriler kapalı kalma süresi alabilir ancak kendi olasılığını azaltmak istiyorum. Ödeyen müşteriler için kapalı kalma süresi uçuş sorununa neden olur. Bu ödeme emin olmak istiyorum böylece müşteriler her zaman verilerine erişebilir.</i> 

Bu senaryoyu desteklemek için deneme kiracıları, ücretli kiracılardan ayrı esnek havuzları halinde koyarak ayırın. Deneme müşteriler alt eDTU veya Kiracı ve alt SLA daha uzun bir kurtarma süresi ile başına vCores sahip. Daha yüksek eDTU veya Kiracı ve daha yüksek bir SLA başına vCores ile bir havuzdaki ödeyen müşterilerdir bakın. En düşük kurtarma zamanı sağlamak için coğrafi olarak çoğaltılmış ödeyen müşterilerin Kiracı veritabanları. Bu yapılandırma sonraki diyagramda gösterilmiştir. 

![Şekil 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-4.png)

Tek bir coğrafi olarak çoğaltılmış veritabanı için (1) kullandığınız şekilde ilk senaryoda olduğu gibi yönetim veritabanlarını oldukça etkindir. Bu yeni kullanıcı aboneliklerini, profil güncelleştirmeleri ve diğer yönetim işlemleri için tahmin edilebilir performans sağlar. Yönetim veritabanlarını ana bulunduğu bölgeyi birincil bölge ve yönetim veritabanlarını ikincil bulunduğu bölgeyi DR bölgesi.

Ödeyen müşteri Kiracı veritabanları birincil bölge içinde sağlanan "Ücretli" havuzunda etkin veritabanlarına sahip. DR bölgede aynı ada sahip bir ikincil havuzu sağlayın. Her bir kiracı, ikincil havuzu (2) coğrafi olarak çoğaltılmış ' dir. Bu, yük devretme kullanarak tüm Kiracı veritabanlarını Hızlı Kurtarma sağlar. 

Birincil bölgede bir kesinti oluşursa, Kurtarma adımları uygulamanız çevrimiçi duruma getirmek için sonraki diyagramda gösterilmiştir:

![Şekil 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-5.png)

* Yönetim veritabanlarını DR bölgeye (3) üzerinden hemen başarısız.
* DR bölgesine işaret etmek için uygulamanın bağlantı dizesini değiştirin. Şimdi tüm yeni hesaplarını ve Kiracı veritabanları DR bölgede oluşturulur. Varolan deneme müşteriler geçici olarak devre dışı verilerini bakın.
* Ücretli kiracının veritabanları hemen kullanılabilirliklerini (4) geri yüklemek için DR bölgede havuzuna yük devri. Yük devretme hızlı meta veri düzeyi değişikliği olduğundan, burada bireysel yük devretmeleri isteğe bağlı son kullanıcı bağlantılar tarafından tetiklenen bir iyileştirme göz önünde bulundurun. 
* İkincil veritabanları yalnızca ikincil kopyalanırken değişikliği günlükleri işlemek için kapasite gerektirdiğinden ikincil havuz eDTU boyutu veya vCore değeri birincil düşük, tam iş yükü karşılamak için havuz kapasitesi şimdi hemen artırın (5) tüm kiracılar. 
* Aynı ada ve deneme müşterilerin veritabanları (6) için DR bölgede aynı yapılandırmaya sahip yeni bir esnek havuz oluşturun. 
* Deneme müşterilerin havuzu oluşturulduktan sonra coğrafi geri yükleme yeni havuza (7) tek tek deneme Kiracı veritabanlarını geri yüklemek için kullanın. Son kullanıcı bağlantılar tarafından tek tek geri yüklemeler tetikleme göz önünde bulundurun veya başka bir uygulamaya özgü Öncelik düzenini kullanın.

Bu noktada, uygulamanızın DR bölgede tekrar çevrimiçi değil. Deneme müşteriler kendi verilere erişirken gecikme sırada tüm ödeyen müşteri verilerine erişimi.

Ne zaman birincil bölge kurtarılan Azure tarafından *sonra* uygulama bu bölgede uygulamayı çalıştırmayı devam edebilirsiniz DR bölgedeki geri veya birincil bölgesine kapatmaya karar verebilirsiniz. Birincil bölge kurtarıldığında *önce* yük devretme işlemi tamamlandı, başarısız olan geri hemen göz önünde bulundurun. Yeniden çalışma sonraki diyagramda gösterildiği adımları gerçekleştirir: 

![Şekil 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-6.png)

* Tüm bekleyen coğrafi geri yükleme istekleri iptal edin.   
* Yönetim veritabanlarını (8) başarısız. Bölgenin kurtarma işleminden sonra eski birincil otomatik olarak ikincil olur. Şimdi bu birincil yeniden olur.  
* Ücretli Kiracı veritabanı (9) başarısız. Benzer şekilde, bölgenin kurtarma işleminden sonra eski ana otomatik olarak ikincil haline gelir. Şimdi ana yeniden olduklarında. 
* DR bölgede salt okunur (10) değiştirilmiştir geri yüklenen Deneme veritabanlarının ayarlayın.
* Her bir veritabanına kurtarma beri değiştirilen deneme müşteriler DR havuzunda için yeniden adlandırın veya deneme müşteriler birincil havuzunda (11) karşılık gelen veritabanını silin. 
* Güncelleştirilmiş veritabanları DR havuzundan birincil havuzu (12) kopyalayın. 
* (13) DR havuzunu silme 

> [!NOTE]
> Yük devretme işlemi zaman uyumsuzdur. Kurtarma süresini en aza indirmek için en az 20 veritabanları toplu olarak Kiracı veritabanı yük devretme komutu yürütün önemlidir. 
> 
> 

Anahtar **yararlı** bu strateji, en yüksek SLA ödeyen müşterilerin sağlamasıdır. Ayrıca, deneme DR havuzu oluşturulduktan hemen sonra yeni denemeler engeli garanti eder. **Dengelemeyi** müşteriler Ücretli bu kurulumu toplam sahip olma maliyeti Kiracı veritabanı tarafından ikincil DR havuzu için maliyetini artırır. Ayrıca, ikincil havuzu farklı bir boyut varsa, DR bölgede havuzu yükseltme tamamlanana kadar düşük performans yük devretme sonrasında ödeyen müşteri deneyimi. 

## <a name="scenario-3-geographically-distributed-application-with-tiered-service"></a>Senaryo 3. Katmanlı hizmetiyle coğrafi olarak dağıtılmış uygulama
<i>Katmanlı hizmet teklifleri ile olgun bir SaaS uygulaması var. My Ücretli müşterilere çok agresif bir SLA sunmak ve hatta kısa kesinti müşteri memnuniyetsizliği neden olabileceğinden kesintiler durumunda etkisi riskini en aza istiyorum. Ödeyen müşteri verilerini her zaman erişebilmesini önemlidir. Denemeler ücretsiz ve deneme süresi boyunca bir SLA önerilmez. </i> 

Bu senaryoyu desteklemek için üç ayrı esnek havuzu kullanın. Yüksek Edtu veya vCores Ücretli müşterilerin Kiracı veritabanları içerecek şekilde iki farklı bölgelerde veritabanı başına iki eşit boyutu havuzlarıyla sağlayın. Deneme kiracıları içeren üçüncü havuzu alt Edtu veya veritabanı başına vCores sahip olabilir ve iki bölgede birinde sağlanması.

En düşük kurtarma süresini kesintiler sırasında güvence altına almak için ücretli müşterilerin Kiracı %50 birincil veritabanlarının her iki bölgede coğrafi olarak çoğaltılmış veritabanlarıdır. Benzer şekilde, her bölge ikincil veritabanlarıyla % 50'si vardır. Bu şekilde, bir bölge çevrimdışıysa, ücretli müşterilerin veritabanları % 50'yalnızca etkilenen ve yük devri gerekir. Diğer veritabanlarındaki değişmeden kalır. Bu yapılandırma aşağıdaki çizimde gösterilmiştir:

![Şekil 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-7.png)

Bu nedenle önceki senaryolarda yönetim veritabanlarını oldukça etkin olarak bunları olarak tek coğrafi olarak çoğaltılmış veritabanları (1) yapılandırın. Bu abonelik, profil güncelleştirmeleri ve diğer yönetim işlemleri yeni müşterinin tahmin edilebilir performans sağlar. Bölgedir A yönetim veritabanları için birincil bölge ve bölge B yönetim veritabanlarını kurtarmak için kullanılır.

Coğrafi olarak çoğaltılmış ödeyen müşterilerin Kiracı veritabanları aynı zamanda, ancak ana ve ikincil bir bölge ve bölge B (2) arasında bölme. Bu şekilde kesintisi etkilenen Kiracı birincil veritabanı diğer bölgeye yük devri ve kullanılabilir hale gelir. Diğer yarısı Kiracı veritabanları olan değil etkilenebilir hiç. 

Sonraki diyagram A. bölgede bir kesinti oluşursa yapılacak kurtarma adımlarını gösterir

![Şekil 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-8.png)

* Hemen yönetim veritabanlarını bölgeye B (3) başarısız.
* Yönetim veritabanlarını bölgede B. değiştirmek için yeni hesapları emin olmak için yönetim veritabanları işaret edecek şekilde uygulamanın bağlantı dizesini değiştirin ve Kiracı veritabanları B bölgede oluşturulur ve varolan Kiracı veritabanları var. de bulunur. Varolan deneme müşteriler geçici olarak devre dışı verilerini bakın.
* Ücretli kiracının veritabanları havuzuna 2 bölgesindeki B hemen kullanılabilirliklerini (4) geri yüklemek için yük devri. Yük devretme hızlı meta veri düzeyi değişikliği olduğundan, burada bireysel yük devretmeleri isteğe bağlı son kullanıcı bağlantılar tarafından tetiklenen bir iyileştirme düşünebilirsiniz. 
* Şimdi bu yana 2 havuzuna yalnızca birincil veritabanları, toplam iş yükü havuzu artar içerir ve hemen kendi eDTU boyutunu (5) veya vCores sayısını artırabilirsiniz. 
* Aynı ada ve aynı yapılandırmasıyla bölgede B deneme müşterilerin veritabanları (6) için yeni esnek havuzu oluşturun. 
* Havuzu oluşturulduktan sonra coğrafi geri yükleme (7) havuza tek tek deneme Kiracı veritabanını geri yüklemek için kullanın. Son kullanıcı bağlantılar tarafından tek tek geri yüklemeler tetikleme göz önünde bulundurun veya başka bir uygulamaya özgü Öncelik düzenini kullanın.

> [!NOTE]
> Yük devretme işlemi zaman uyumsuzdur. Kurtarma süresini en aza indirmek için en az 20 veritabanları toplu olarak Kiracı veritabanı yük devretme komutu yürütün önemlidir. 
> 

Bu noktada uygulamanız B. bölgede tekrar çevrimiçi değil Deneme müşteriler kendi verilere erişirken gecikme sırada tüm ödeyen müşteri verilerine erişimi.

Bölge A kurtarıldığında deneme müşteriler veya deneme müşteriler havuzu A. bölgede kullanarak yeniden çalışma için bölge B kullanmak isteyip istemediğinizi karar vermeniz gerekir Tek bir ölçüt deneme Kiracı veritabanları kurtarma beri değiştirilen % olabilir. Bağımsız olarak kararı, ücretli kiracılar iki havuzları arasında yeniden dengelemeniz gerekir. Deneme Kiracı veritabanları yeniden bölge A. başarısız olduğunda sonraki diyagramda işlemi gösterilmektedir.  

![Şekil 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-9.png)

* Deneme DR havuzu tüm bekleyen coğrafi geri yükleme isteklerine iptal edin.   
* Yönetim veritabanını (8) başarısız. Bölgenin kurtarma işleminden sonra eski birincil otomatik olarak ikincil hale geldi. Şimdi bu birincil yeniden olur.  
* Veritabanlarını geri havuzu 1 ve başlatma yük devretme kendi ikincil (9) için başarısız, ücretli bir kiracı seçin. Bölgenin kurtarma işleminden sonra 1 havuzdaki tüm veritabanları otomatik olarak ikincil hale geldi. Şimdi bunların % 50 ana yeniden haline gelir. 
* Havuzuna 2 özgün eDTU (10) ya da vCores sayısını azaltın.
* Tüm kümesi B bölgede deneme veritabanları salt okunur (11'e) geri.
* Her bir veritabanına kurtarma bu yana değişti deneme DR havuzundaki için yeniden adlandırın veya deneme birincil havuzunda (12) karşılık gelen veritabanını silin. 
* Güncelleştirilmiş veritabanları DR havuzundan birincil havuzu (13) kopyalayın. 
* DR havuzu (14) Sil 

Anahtar **avantajları** bu stratejisi şunlardır:

* Bir kesinti Kiracı veritabanları % 50'den fazla etkisi olamaz sağladığından ödeyen müşteriler için en agresif SLA destekler. 
* DR havuzu izi Kurtarma sırasında oluşturulduktan hemen sonra yeni denemeler engeli güvence altına alır. 
* Havuz kapasitesi 1 havuzundaki ikincil veritabanlarıyla % 50 olarak daha verimli kullanılmasına izin verir ve havuzu 2 birincil veritabanları daha az etkin olmasını garanti edilir.

Ana **dengelemeler** şunlardır:

* Yönetim veritabanlarını karşı CRUD işlemleri bölge bir yönetim veritabanlarını birincil karşı yürütülen gibi B bölgesine bağlı son kullanıcılar için bağlı son kullanıcılar için daha düşük gecikme süresine sahiptir.
* Daha karmaşık bir tasarım yönetim veritabanının gerektirir. Örneğin, her bir kiracı kaydı yük devretme ve yeniden çalışma sırasında değiştirilmesi gereken bir konum etiketi yok.  
* B bölgede havuzu yükseltme tamamlanana kadar ödeyen müşteri normalden daha düşük performans düşebilir. 

## <a name="summary"></a>Özet
Bu makalede bir SaaS ISV çok kiracılı uygulama tarafından kullanılan veritabanı katmanı için olağanüstü durum kurtarma stratejileri odaklanır. Seçtiğiniz stratejisi iş modeli, müşterilerinize sunar, kısıtlama vb. Bütçe istediğiniz SLA gibi uygulamanızın gereksinimlerine bağlıdır. Bilinçli karar şekilde açıklanan her strateji avantajları ve dengelemeyi özetlenmektedir. Ayrıca, büyük olasılıkla belirli uygulamanızı Azure diğer bileşenleri içerir. Böylece, iş sürekliliği Kılavuzu gözden geçirebilir ve bunlarla veritabanı katmanı kurtarma düzenlemek. Veritabanı uygulamalarında Azure kurtarılması yönetme hakkında daha fazla bilgi için bkz [tasarlama bulut çözümleri olağanüstü durum kurtarma için](sql-database-designing-cloud-solutions-for-disaster-recovery.md).  

## <a name="next-steps"></a>Sonraki adımlar
* Veritabanı Yedeklemeleri otomatik Azure SQL hakkında bilgi edinmek için bkz: [SQL veritabanı yedeklemeleri otomatik](sql-database-automated-backups.md).
* İş sürekliliğine genel bakış ve senaryolar için bkz: [iş sürekliliğine genel bakış](sql-database-business-continuity.md).
* Kurtarma için otomatik yedeklemeler kullanma hakkında bilgi edinmek için bkz: [bir veritabanını hizmeti tarafından başlatılan yedeklerden geri](sql-database-recovery-using-backups.md).
* Daha hızlı kurtarma seçenekleri hakkında bilgi edinmek için [aktif coğrafi çoğaltma](sql-database-geo-replication-overview.md).
* Arşivleme için otomatik yedeklemeler kullanma hakkında bilgi edinmek için bkz: [veritabanı kopyalama](sql-database-copy.md).

