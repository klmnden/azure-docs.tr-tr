---
title: Olağanüstü durum kurtarma çözümleri - Azure SQL veritabanı tasarlama | Microsoft Docs
description: Doğru yük devretme düzeni seçerek olağanüstü durum kurtarma için bulut çözümünüzü tasarlamayı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 6a332ce265a4bb41a9ad3c0c3a29683187a0f0d4
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62098414"
---
# <a name="disaster-recovery-strategies-for-applications-using-sql-database-elastic-pools"></a>SQL veritabanı esnek havuzları kullanan uygulamalar için olağanüstü durum kurtarma stratejileri

Bulut Hizmetleri kusursuz değildir ve yıkıcı olaylara meydana yıllar içinde edindiğimiz. SQL veritabanı, bu olaylar meydana geldiğinde uygulamanız için iş sürekliliği sağlamak için çeşitli özellikleri sağlar. [Elastik havuzlar](sql-database-elastic-pool.md) ve tek veritabanları aynı türde bir olağanüstü durum kurtarma (DR) özellikleri destekler. Elastik havuzlar için bu makalede, çeşitli DR stratejileri açıklanır. Bu SQL veritabanı iş sürekliliği özellikleri yararlanın.

Bu makalede aşağıdaki kurallı ISV SaaS uygulama düzeni kullanır:

Modern bulut tabanlı bir web uygulaması için son kullanıcı bir SQL veritabanı sağlar. ISV birçok müşterisi var ve bu nedenle Kiracı veritabanları bilinen, çok sayıda veritabanı kullanır. Kiracı veritabanları genellikle beklenmeyen etkinlik desenlerini olduğundan, ISV elastik havuz maliyet veritabanı uzun süreler boyunca öngörülebilir hale getirmek için kullanır. Kullanıcı etkinliği ani zaman elastik havuz performans yönetimi de basitleştirir. Kiracı veritabanlarını yanı sıra uygulama ayrıca birden fazla veritabanı kullanır kullanıcı profillerini, güvenliği yönetmek, kullanım desenleri vb. toplamak için. Tek tek kiracılar kullanılabilirliğini bütün olarak uygulamanın kullanılabilirliğini etkilemez. Ancak, yönetim veritabanları, performans ve kullanılabilirlik için uygulamanın işlevi kritik ve yönetim veritabanlarını çevrimdışı olduğunda uygulamanın tamamı çevrimdışı olur.

Bu makalede, olanları katı kullanılabilirlik gereksinimleri olan maliyet hassas başlatma uygulamaları senaryolarından aralığını kapsayan DR stratejileri açıklanır.

> [!NOTE]
> Premium veya iş açısından kritik veritabanları ve elastik havuzlar kullanıyorsanız, bunları dayanıklı bölgesel kesintiler için bunları bölge yedekli dağıtım yapılandırması için dönüştürme yapabilirsiniz. Bkz: [bölgesel olarak yedekli veritabanları](sql-database-high-availability.md).

## <a name="scenario-1-cost-sensitive-startup"></a>Senaryo 1. Hassas başlangıç maliyeti

Yeni kurulan bir işletmeniz am ve son derece hassas maliyet bildirimi.  Dağıtım ve uygulama yönetimini basitleştirmek istiyorum ve her müşteri için sınırlı bir SLA'sı olabilir. Ancak, uygulama bir bütün hiçbir zaman çevrimdışı olduğundan emin olmak istiyorum.

Kolaylık olması şartını sağlamak için tercih ettiğiniz bir Azure bölgesinde bir elastik havuz içine tüm Kiracı veritabanlarında dağıtın ve yönetim veritabanları olarak coğrafi olarak çoğaltılmış tek veritabanları dağıtma. Coğrafi gelen ek bir maliyet olmadan geri yükleme, kiracılar olağanüstü durum kurtarma için kullanın. Yönetim veritabanlarını kullanılabilirliğini sağlamak için coğrafi çoğaltma otomatik yük devretme bir kullanarak başka bir bölgeye gruplamak (1. adım). Devam eden bu senaryo olağanüstü durum kurtarma yapılandırmasında ikincil veritabanlarıyla toplam maliyetine eşit maliyetidir. Bu yapılandırma, bir sonraki diyagramda gösterilmiştir.

![Şekil 1](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-1.png)

Birincil bölgede bir kesinti oluşursa, Kurtarma adımları uygulamanızı çevrimiçi duruma göre bir sonraki diyagramda gösterilmiştir.

* Yük devretme grubu otomatik yük devretme yönetim veritabanının DR bölgesindeki başlatır. Uygulama, yeni birincil ve tüm yeni hesaplar için otomatik olarak bağlanır ve DR bölgesinde Kiracı veritabanları oluşturulur. Mevcut müşteriler verilerine geçici olarak devre dışı bakın.
* Özgün havuz (2) olarak aynı yapılandırmaya sahip esnek havuz oluşturun.
* Coğrafi geri yükleme veritabanları (3) Kiracı kopyalarını oluşturmak için kullanın. Son kullanıcı bağlantılar tarafından tek tek geri yükleme tetikleniyor düşünün veya başka bir uygulamaya özgü Öncelik düzenini kullanın.

Uygulamanızı DR bölgesinde yeniden çevrimiçi olduğundan bu noktada, ancak bazı müşteriler verilerine erişirken gecikme olur.

![Şekil 2](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-2.png)

Kesinti geçici varsa, tüm veritabanını geri yükler DR bölgesinde getirildiğinden önce birincil bölgeye Azure tarafından kurtarılması mümkündür. Bu durumda, uygulamayı birincil bölgeye geri taşıyarak düzenleyin. İşlem bir sonraki Diyagramda gösterilen adımlarında rehberlik eder.

* Tüm bekleyen coğrafi geri yükleme isteği iptal edin.
* Yönetim veritabanlarını (5) birincil bölgeye yük devretme. Eski ana, bölgenin kurtarma işleminden sonra otomatik olarak bu ikincil veritabanı haline geldi. Artık bunlar rolleri yeniden geçiş yapın.
* Uygulamanın bağlantı dizesini birincil bölgeye işaret edecek şekilde değiştirin. Artık tüm yeni hesaplar ve Kiracı veritabanları birincil bölgede oluşturulur. Bazı mevcut müşteriler verilerine geçici olarak devre dışı bakın.
* Salt okunur DR bölgesindeki (6) değiştirilemez sağlamak için DR havuzdaki tüm veritabanları ayarlayın.
* Kurtarma beri değişmiş DR havuzdaki her bir veritabanı için yeniden adlandırın veya birincil havuzunda (7) karşılık gelen veritabanlarını silin.
* Güncelleştirilmiş veritabanlarını DR havuzdan birincil havuza (8) kopyalayın.
* (9) DR havuzunu Sil

Bu noktada, uygulamanız tüm Kiracı veritabanlarında kullanılabilir birincil havuzu ile birincil bölgedeki çevrimiçidir.

![Şekil 3](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-3.png)

Anahtar **fayda** bu strateji düşük devam eden veri katmanı yedekleme maliyetidir. Yedeklemeler, ek ücret ödemeden hiçbir uygulama yeniden yazma ile SQL veritabanı hizmeti tarafından otomatik olarak alınır.  Esnek veritabanları yalnızca geri yüklendiğinde ücret uygulanır. **Dengeyi** olan tüm Kiracı veritabanlarının tam kurtarma'nın önemli bir zaman alır. Toplam sürenin uzunluğunu bağlıdır sayı DR bölgesinde başlatan geri yüklemeler ve Kiracı veritabanlarını genel boyutu. Diğer bazı kiracılar geri yüklemeler önceliğini bile hizmet istemlerde ve mevcut müşteriler veritabanları genel etkisini en aza indirmek için kısıtlar gibi aynı bölgede başlatılan tüm diğer geri yükleme ile rekabet halinde olduğunuz. Yeni elastik havuz DR bölgesinde oluşturulana kadar ek olarak, Kiracı veritabanlarını kurtarma başlatamıyor.

## <a name="scenario-2-mature-application-with-tiered-service"></a>Senaryo 2. Olgun bir uygulama katmanlı Service

Olgun bir SaaS uygulamanız katmanlı hizmet teklifleri ve farklı SLA'lar deneme yapan müşteriler ve müşteriler ödeme yapmak istiyorum. Deneme yapan müşteriler için maliyet mümkün olduğunca azaltmak sahibim. Deneme yapan müşteriler kapalı kalma süresi alabilir ancak kendi olasılığını azaltmak istiyorum. Ücretli müşterileri için kapalı kalma süresi bir uçuş riski oluşturur. Müşterilerin her zaman bu ödeme emin olmak istiyorum verilerine erişebilir.

Bu senaryoyu desteklemek için deneme kiracıları, ücretli kiracılardan ayrı elastik havuzlara koyarak ayırın. Deneme yapan müşteriler, daha düşük eDTU veya sanal çekirdek başına Kiracı ve daha düşük bir SLA ile daha uzun bir kurtarma süresi vardır. Bir havuzu daha yüksek eDTU veya sanal çekirdek başına Kiracı ve daha yüksek bir SLA ile Ücretli müşterileri bulunan. En düşük kurtarma zamanı sağlamak için coğrafi olarak çoğaltılmış Ücretli müşterileri Kiracı veritabanları değildir. Bu yapılandırma, bir sonraki diyagramda gösterilmiştir.

![Şekil 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-4.png)

İlk senaryoda olduğu gibi tek bir coğrafi çoğaltmalı veritabanı (1) kullanmak için yönetim veritabanlarını oldukça etkin. Bu, yeni müşteri Abonelikleri, profili güncelleştirmeleri ve diğer yönetim işlemleri için tahmin edilebilir performans sağlar. Ana yönetim veritabanlarının bulunduğu bölge birincil bölgeye ve ikincil yönetim veritabanlarının bulunduğu bölgeyi DR bölgesindeki.

Ücretli müşterileri Kiracı veritabanlarını, birincil bölgede sağlanan "Ücretli" havuzundaki etkin veritabanlarına sahip. DR bölgesinde aynı ada sahip ikinci bir havuz sağlayın. Her Kiracı, coğrafi çoğaltmalı ikincil havuza (2). Bu hızlı yük devretme kullanarak tüm Kiracı veritabanlarına kurtarılmasını sağlar.

Birincil bölgede bir kesinti oluşursa, Kurtarma adımları uygulamanızı çevrimiçi duruma getirmek için sonraki diyagramda gösterilmiştir:

![Şekil 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-5.png)

* Yönetim veritabanlarını DR bölgesindeki (3) için hemen devredin.
* Uygulamanın bağlantı dizesi için DR bölgesindeki işaret edecek şekilde değiştirin. Artık tüm yeni hesaplar ve Kiracı veritabanlarını DR bölgesinde oluşturulur. Varolan deneme yapan müşteriler verilerine geçici olarak devre dışı bakın.
* Ücretli kiracının hemen kullanılabilirliklerini (4) geri yüklemek için DR bölgesinde havuzdan veritabanları yük devretme. Yük devretme hızlı meta veri düzeyi değişikliği olduğundan, burada bireysel yük devretmeleri isteğe bağlı olarak son kullanıcı bağlantılar tarafından tetiklenen bir iyileştirme göz önünde bulundurun.
* Tam iş yüküne uyum sağlamak için havuz kapasitesi artık ikincil veritabanları yalnızca ikincil veritabanı'na sürerken değişiklik günlüklerini işlemek için kapasite gerektirdiğinden ikincil havuz eDTU boyutu veya sanal Çekirdek değer birincil düşük, hemen artırın Tüm kiracılar (5).
* Aynı ada ve aynı yapılandırmayla deneme yapan müşteriler veritabanları (6) için DR bölgesinde yeni bir esnek havuz oluşturun.
* Deneme yapan müşteriler havuzu oluşturulduktan sonra coğrafi geri yükleme (7) yeni havuza bireysel deneme Kiracı veritabanlarını geri yükleyin. Son kullanıcı bağlantılar tarafından tek tek geri yükleme tetikleniyor düşünün veya başka bir uygulamaya özgü Öncelik düzenini kullanın.

Bu noktada, uygulamanızın DR bölgesinde yeniden çevrimiçi değil. Deneme yapan müşteriler verilerine erişirken gecikme sırada tüm ödeyen müşteri verilerine erişebilir.

Ne zaman birincil bölgeden kurtarılan Azure tarafından *sonra* devam ederek bu bölgede uygulamayı çalıştıran DR bölgesindeki uygulamada geri yükledikten veya birincil bölgeye geri başarısız olmasına karar verebilirsiniz. Birincil bölge kurtarıldığında *önce* yük devretme işlemi tamamlandıktan, yeniden çalıştırmadan hemen göz önünde bulundurun. Yeniden çalışma, sonraki diyagramda gösterildiği adımları gerçekleştirir:

![Şekil 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-6.png)

* Tüm bekleyen coğrafi geri yükleme isteği iptal edin.
* Yönetim veritabanları (8) başarısız. Bölgenin kurtarma işleminden sonra eski birincil otomatik olarak ikincil olur. Artık birincil yeniden olur.  
* Ücretli Kiracı veritabanları (9) başarısız. Benzer şekilde, bölgenin kurtarma işleminden sonra eski seçimlerine otomatik olarak ikincil veritabanı haline gelir. Ana yeniden duruma göre.
* DR bölgesinde salt okunur (10) değişmiş olan deneme geri yüklenen veritabanları ayarlayın.
* Kurtarma beri değiştirilen deneme yapan müşteriler DR havuzdaki her bir veritabanı için yeniden adlandırın veya deneme yapan müşteriler birincil havuzunda (11) karşılık gelen veritabanını silin.
* Güncelleştirilmiş veritabanlarını DR havuzdan birincil havuza (12) kopyalayın.
* DR havuzu (13) silin.

> [!NOTE]
> Yük devretme işlemi zaman uyumsuzdur. Kurtarma zamanı en aza indirmek için en az 20 veritabanınızın toplu işler üzerinde Kiracı veritabanları yük devretme komutu yürütün önemlidir.

Anahtar **fayda** bu strateji, en yüksek SLA'sı için ücretli müşterileri sağlamasıdır. Ayrıca, deneme DR havuzu oluşturulduktan hemen sonra yeni denemeler engellenmemiş olmasını sağlar. **Dengeyi** müşterilerin Ücretli Bu kurulum Kiracı veritabanlarını ikincil DR havuzunun maliyetine göre toplam maliyetini artırır. Ayrıca, ikincil havuz başka bir boyutu varsa, DR bölgesindeki içinde havuzu yükseltmesi tamamlanana kadar düşük performans yük devretmeden sonra ödeyen müşteri deneyimi.

## <a name="scenario-3-geographically-distributed-application-with-tiered-service"></a>Senaryo 3. Katmanlı Hizmet ile coğrafi olarak dağıtılmış uygulama

Katmanlı hizmet teklifleri olgun bir SaaS uygulamanız var. Ücretli müşterilerimin çok agresif bir SLA sunar ve daha kısa kesintinin müşteri memnuniyetsizliğine neden olabileceği için kesintiler meydana geldiğinde etkisi riskini en aza indirmek istersiniz. Kritik ödeyen müşteri verilerine her zaman erişebilir. Ücretsiz deneme sürümleri ve deneme süresi boyunca bir SLA sunulmaz.

Bu senaryoyu desteklemek için üç ayrı elastik havuzu kullanın. Yüksek Edtu veya veritabanı Ücretli müşterileri Kiracı veritabanlarını içerecek şekilde iki farklı bölge başına sanal çekirdek içeren iki aynı boyutta havuz sağlayın. Deneme kiracıları içeren üçüncü havuzu daha düşük Edtu veya veritabanı başına sanal çekirdek alabilir ve iki bölgeleri biri sağlanmalıdır.

Kesintiler sırasında en düşük kurtarma zamanı sağlamak için her iki bölgeleri birincil veritabanlarını yüzdesi 50 ile coğrafi olarak çoğaltılmış Ücretli müşterileri Kiracı veritabanları değildir. Benzer şekilde, her bölge, ikincil veritabanlarıyla yüzdesi 50 sahiptir. Bu şekilde bir bölgeyi çevrimdışı ise yalnızca %50 Ücretli müşterileri veritabanlarının etkilendiğini ve yük devretme gerekir. Diğer veritabanlarının değişmeden kalır. Bu yapılandırma, aşağıdaki diyagramda gösterilmiştir:

![Şekil 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-7.png)

Bu nedenle önceki senaryolarda yönetim veritabanlarını oldukça etkin olduğundan bunları olarak tek coğrafi çoğaltmalı veritabanı (1) yapılandırın. Bu abonelikler, profili güncelleştirmeleri ve diğer yönetim işlemlerini yeni müşterinin tahmin edilebilir performans sağlar. Bir bölge yönetim veritabanları için birincil bölgedir ve B bölge yönetim veritabanlarını kurtarılması için kullanılır.

Ücretli müşterileri Kiracı veritabanlarını da coğrafi olarak çoğaltılmış ancak seçimlerine ikinciller ile bölge A ve B (2) bölgesi arasında bölün. Bu şekilde kesintisinden etkileniyor birincil Kiracı veritabanlarını başka bir bölgeye yük devretmek ve kullanılabilir hale gelir. Diğer yarısı Kiracı veritabanlarını değil olması etkilenir hiç.

Bir sonraki diyagramda a bölgede kesinti oluşursa, Kurtarma adımlar gösterilmektedir.

![Şekil 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-8.png)

* Yönetim veritabanlarını bölgeye B (3) hemen başarısız.
* B değişiklik bölgede yönetim veritabanlarını yeni hesaplar emin olmak için yönetim veritabanlarını işaret edecek şekilde uygulamanın bağlantı dizesini değiştirebilir ve B bölgede Kiracı veritabanları oluşturulur ve Kiracı veritabanlarında var. de bulunur. Varolan deneme yapan müşteriler verilerine geçici olarak devre dışı bakın.
* Ücretli kiracının veritabanı havuzuna 2 bölgesindeki B hemen kullanılabilirliklerini (4) geri yüklemek için yük devredebilirsiniz. Yük devretme hızlı meta veri düzeyi değişikliği olduğundan, burada bireysel yük devretmeleri isteğe bağlı olarak son kullanıcı bağlantılar tarafından tetiklenen bir iyileştirme düşünebilirsiniz.
* Şimdi bu yana 2 havuzu yalnızca birincil veritabanları, havuz artar toplam iş yüküne içerir ve hemen eDTU boyutunu (5) ya da sanal çekirdek sayısını artırabilir.
* Aynı ada ve aynı yapılandırmayla B deneme yapan müşteriler veritabanları (6) için bölgede yeni bir esnek havuz oluşturun.
* Havuzu oluşturulduktan sonra coğrafi geri yükleme, tek bir deneme Kiracı veritabanı (7) havuza geri yüklemek için kullanın. Son kullanıcı bağlantılar tarafından tek tek geri yükleme tetikleniyor düşünün veya başka bir uygulamaya özgü Öncelik düzenini kullanın.

> [!NOTE]
> Yük devretme işlemi zaman uyumsuzdur. Kurtarma zamanı en aza indirmek için en az 20 veritabanınızın toplu işler üzerinde Kiracı veritabanları yük devretme komutu yürütün önemlidir.

Uygulamanızın b bölgede yeniden çevrimiçi olduğundan bu noktada Deneme yapan müşteriler verilerine erişirken gecikme sırada tüm ödeyen müşteri verilerine erişebilir.

Bir bölge kurtarıldığında bölge B deneme yapan müşteriler veya deneme yapan müşteriler havuzu A. bölgede kullanarak yeniden çalışma için kullanmak istiyorsanız karar vermeniz gerekir Bir ölçüt % deneme Kiracı veritabanlarının kurtarma beri değiştirilmiş olabilir. Bağımsız olarak kararı, ücretli kiracılar iki havuzlar arasında yeniden dengelemeniz gerekir. Deneme Kiracı veritabanlarını geri bölge A. başarısız olduğunda bir sonraki diyagramda işlemini gösterir.  

![Şekil 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-9.png)

* Deneme DR havuzu tüm bekleyen coğrafi geri yükleme isteklerine iptal edin.
* Yönetim veritabanı üzerinde (8) başarısız. Bölgenin kurtarma işleminden sonra eski birincil otomatik olarak ikincil hale geldi. Artık birincil yeniden olur.  
* Hangi Ücretli Kiracı veritabanlarını geri havuzu 1 ve başlatma yük devretme (9), ikincil veritabanı için başarısız'ı seçin. Bölgenin kurtarma işleminden sonra 1 havuzdaki tüm veritabanları otomatik olarak ikincil veritabanı hale geldi. Artık % 50'bunları yeniden ana olur.
* 2 sanal çekirdek sayısı ve özgün eDTU (10) için havuzunun boyutunu azaltın.
* Tüm kümesi B bölgede deneme veritabanları salt okunur (11'e) geri.
* Kurtarma beri değişmiş deneme DR havuzdaki her bir veritabanı için yeniden adlandırın veya deneme birincil havuzundaki (12) karşılık gelen veritabanını silin.
* Güncelleştirilmiş veritabanlarını DR havuzdan birincil havuza (13) kopyalayın.
* DR havuzu (14) silin.

Anahtar **avantajları** bu strateji şunlardır:

* Kesinti Kiracı veritabanlarını % 50'den fazla olamaz etkileyen ulaşılamamasını garantilediğinden Ücretli müşterileri için en agresif SLA'yı destekler.
* İzi DR havuzu Kurtarma sırasında oluşturulduktan hemen sonra yeni denemeler engellenmemektedir güvence altına alır.
* Havuz kapasitesi havuzu 1 ikincil veritabanları yüzdesi 50 olarak daha verimli kullanılmasına izin verir ve Havuz 2 birincil veritabanlarından daha az etkin olmasını garanti edilir.

Ana **dengelemeler** şunlardır:

* Yönetim veritabanlarını karşı bir CRUD işlemleri, birincil yönetim veritabanlarını karşı gerçekleştirilirken B bölgesine bağlı son kullanıcılar için bir bölge için bağlı son kullanıcılar için daha düşük gecikme süresine sahiptir.
* Yönetim veritabanı, daha karmaşık bir tasarım gerektiriyor. Örneğin, her Kiracı kaydı yük devretme ve yeniden çalışma sırasında değiştirilmesi gereken bir konum etiket var.  
* Ücretli müşterileri, B bölgede havuzu yükseltmesi tamamlanana kadar normalden daha düşük performans karşılaşabilirsiniz.

## <a name="summary"></a>Özet

Bu makalede, SaaS ISV çok kiracılı bir uygulama tarafından kullanılan veritabanı katmanı için olağanüstü durum kurtarma stratejileri üzerinde odaklanır. Seçtiğiniz strateji iş modeli, müşterilerinize sunmak, kısıtlama vb. Bütçe istediğiniz SLA'sı uygulama gereksinimlerine göre faturalandırılır. Bir düzeltme eki bilinçli kararlar böylece açıklandığı gibi her stratejinin avantajları ve dengeyi özetlenmektedir. Ayrıca, büyük olasılıkla belirli uygulamanız diğer Azure bileşenlerini içerir. Bu nedenle, iş sürekliliği yönergeleri gözden geçirmeniz ve veritabanı katmanı ile bunları kurtarılmasını düzenleyin. Veritabanı uygulamalarının azure'da kurtarmayı yönetme hakkında daha fazla bilgi için bkz [olağanüstü durum kurtarma için bulut çözümleri tasarlama](sql-database-designing-cloud-solutions-for-disaster-recovery.md).  

## <a name="next-steps"></a>Sonraki adımlar

* Veritabanı otomatik yedeklemeler Azure SQL hakkında bilgi edinmek için bkz: [SQL veritabanı otomatik yedeklerinde](sql-database-automated-backups.md).
* İş sürekliliğine genel bakış ve senaryolar için bkz: [iş sürekliliğine genel bakış](sql-database-business-continuity.md).
* Otomatik yedekleme, kurtarma için kullanma hakkında bilgi edinmek için [hizmet tarafından başlatılan yedeklemelerden veritabanını geri yükleme](sql-database-recovery-using-backups.md).
* Daha hızlı kurtarma seçenekleri hakkında bilgi edinmek için [etkin coğrafi çoğaltma](sql-database-active-geo-replication.md) ve [otomatik yük devretme grupları](sql-database-auto-failover-group.md).
* Otomatik yedekleri arşivlemek için kullanma hakkında bilgi edinmek için [veritabanı kopyalama](sql-database-copy.md).
