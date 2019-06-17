---
title: Birden çok SQL veritabanı elastik havuzları-Azure ile yönetme | Microsoft Docs
description: Yönetin ve ölçeklendirin birden çok SQL veritabanı - yüzlerce ve binlerce kullanarak elastik havuzlar -. Kaynakları gerektiğinde dağıtmak için tek fiyat.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: ninarn, carlrab
manager: craigg
ms.date: 02/28/2019
ms.openlocfilehash: c1db16475224cc3c91a5353ead0aabd091098e14
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66240374"
---
# <a name="elastic-pools-help-you-manage-and-scale-multiple-azure-sql-databases"></a>Elastik havuzlar, yönetmenize ve birden çok Azure SQL veritabanını ölçeklendirme Yardım

SQL Veritabanı elastik havuzları, kullanım talepleri değişken olan ve öngörülmeyen birden çok veritabanını yönetmeye ve ölçeklendirmeye yönelik kolay ve ekonomik bir çözümdür. Bir elastik havuzdaki veritabanları, tek bir Azure SQL veritabanı sunucusunda bulunan ve bir fiyat karşılığında kaynakları sayıda paylaşın. Azure SQL Veritabanındaki elastik havuzlar, SaaS geliştiricilerinin bir veritabanı grubuna ait fiyat performansını belirtilen bütçe dahilinde iyileştirmesini ve aynı zamanda her veritabanı için performans Elastikliği sunmasını sağlar.

## <a name="what-are-sql-elastic-pools"></a>SQL elastik havuzları nelerdir

SaaS geliştiricileri, birden fazla veritabanından oluşan büyük ölçekli veri katmanlarının üzerinde uygulamalar oluşturur. Her müşteri için tek veritabanı sağlanması yaygın bir uygulama modelidir. Ancak, farklı müşteriler genellikle değişen ve tahmin edilemeyen kullanım modellerine sahiptir ve her veritabanı kullanıcısının kaynak gereksinimlerini tahmin etmek zordur. Geleneksel olarak iki seçeneğiniz vardır:

- En yüksek kullanımı üzerinden ödeme, temel kaynakları fazladan sağlama veya
- Maliyet, performans ve müşteri memnuniyetini çoğaltamaz kaydetmek için eksik sağlama.

Elastik havuzlar, veritabanları ihtiyaç duydukları zaman ihtiyaç duydukları performans kaynaklarını almasını sağlayarak bu sorunu çözer. Bunlar, tahmin edilebilir bir bütçe içinde basit bir kaynak ayırma mekanizması sağlar. Elastik havuzları kullanan SaaS uygulamalarının tasarım desenleri hakkında daha fazla bilgi edinmek için bkz. [Azure SQL Veritabanı kullanan Çok Kiracılı SaaS Uygulamaları için Tasarım Desenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md).

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Elastic-databases-helps-SaaS-developers-tame-explosive-growth/player]
>

> [!IMPORTANT]
> Elastik havuzlar için veritabanı başına ücret yoktur. Bir havuz en yüksek eDTU veya sanal çekirdek, kullanım veya havuza bir saatten az için etkin olup bağımsız olarak mevcut olduğu her saat için faturalandırılırsınız.

Elastik havuzlar geliştiricinin tek veritabanları tarafından öngörülemez süreler boyunca kullanımı sağlamak için birden çok veritabanı tarafından paylaşılan bir havuz için kaynak satın etkinleştirin. Havuz ya da tabanlı kaynaklarını yapılandırabilirsiniz [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) veya [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md). Bir havuz için kaynak gereksinimi, veritabanlarının toplam kullanımına tarafından belirlenir. Havuz için kullanılabilen kaynakları miktarı Geliştirici bütçesine göre denetlenir. Geliştirici yalnızca veritabanlarını havuza ekler, veritabanları için minimum ve maksimum kaynakları ayarlar (minimum ve maksimum Dtu veya en düşük veya en yüksek Vcore modeli kaynaklama dillerinden bağlı olarak) ve ardından kaynakları temel ayarlar, Bütçe. Geliştirici, hizmetini zayıf bir başlangıçtan sürekli artan ölçekte olgun bir işletmeye sorunsuzca büyütmek için havuzları kullanabilir.

Havuz içerisinde tek tek veritabanlarına belirli parametreler içinde otomatik olarak ölçeklendirme esnekliği tanınır. Ağır yük bir veritabanı talebi karşılamak üzere daha fazla kaynak kullanımına neden olabilir. Yükü az kullanma ve veritabanlarını hiçbir yük altında hiçbir kaynak kullanan. Tek tek veritabanları yerine tüm havuz için kaynak sağlamak, yönetim görevlerinizi basitleştirir. Ayrıca, havuz için tahmin edilebilir bir bütçe sahip. Veritabanlarını yeni eDTU ayırma için ek işlem kaynakları sağlamak üzere taşınması gerekebilir dışında ek kaynaklar, veritabanı kapalı kalma süresi olmadan, mevcut bir havuza eklenebilir. Ek Kaynaklar artık gerekli değilse benzer şekilde, bunlar herhangi bir noktada mevcut bir havuzdan sürede kaldırılabilir. Ayrıca havuza veritabanları ekleyebilir veya havuzdan veritabanları kaldırabilirsiniz. Bir veritabanı kaynakları tahmin edilebilir bir şekilde normalden az kullanıyorsa bu veritabanını havuzdan çıkarın.

> [!NOTE]
> İçine veya dışına elastik havuz veritabanlarını taşıma olduğunda kesinti süresi olmaksızın bir kısa bir süre (saniye) bazında işlemi sonunda dışında veritabanı bağlantıları bırakıldığında.

## <a name="when-should-you-consider-a-sql-database-elastic-pool"></a>Ne zaman bir SQL veritabanı esnek havuzunu göz önünde bulundurmalıyım

Havuzlar, belirli kullanım düzenlerine sahip çok sayıda veritabanı bulunan durumlar için çok uygundur. Söz konusu kullanım düzeni, belirli bir veritabanı için ortalama düşük düzeyde kullanım ile nispeten nadir zamanlarda kullanımın ani olarak artması şeklindedir.

Bir havuza ekleyebileceğiniz veritabanı sayısı arttıkça, tasarruflarınız artar. Uygulama kullanım modelinize bağlı olarak, yalnızca iki S3 veritabanı ile tasarruf edildiğini görmek mümkündür.

Aşağıdaki bölümler veritabanı koleksiyonunuzun bir havuzda olmasının yararlarını nasıl değerlendireceğini anlamanıza yardımcı olabilir. Örneklerde Standart havuzlar kullanılmaktadır, ancak aynı ilkeler Temel ve Premium havuzlar için de geçerlidir.

### <a name="assessing-database-utilization-patterns"></a>Veritabanı kullanım modellerini değerlendirme

Aşağıdaki şekilde zamanın büyük bölümünü boşta geçiren, ancak düzenli olarak ani etkinlikler sergileyen bir veritabanı örneğini göstermektedir. Bu model bir havuz için uygun olan kullanım modelidir:

   ![havuz için uygun bir tek veritabanı](./media/sql-database-elastic-pool/one-database.png)

Gösterilen beş dakikalık süre boyunca Veritabanı1, 90 DTU’ya kadar yükselir, ancak genel ortalama kullanım beş DTU’dan azdır. Bir S3 işlem boyutu, bu iş yükünü tek veritabanında çalıştırmak için gereklidir, ancak bu kaynakların çoğunu kullanılmamış düşük etkinlik dönemlerinde bırakır.

Havuz bu kullanılmayan DTU’ların birden fazla veritabanında paylaşılmasına olanak tanır ve böylece gereken DTU ile genel maliyeti azaltır.

Önceki örnekten devam ederek, Veritabanı1 ile benzer kullanım modellerine sahip ek veritabanları olduğunu varsayalım. Sonraki iki aşağıdaki resimde, dört veritabanı ve 20 veritabanının kullanımı katmanlı kullanımın zaman DTU tabanlı satın alma modeli kullanarak çakışmayan yapısını göstermek için aynı grafiğe sürükleyin:

   ![bir havuz için uygun kullanım modeli ile dört veritabanı](./media/sql-database-elastic-pool/four-databases.png)

   ![bir havuz için uygun kullanım modeli ile yirmi veritabanı](./media/sql-database-elastic-pool/twenty-databases.png)

20 veritabanının tamamındaki toplam DTU kullanımı, önceki şekilde siyah çizgi ile gösterilmiştir. Bu şekil, toplam DTU kullanımının 100 DTU’yu hiçbir zaman aşmadığını ve 20 veritabanının bu süre boyunca 100 eDTU’yu paylaşabileceğini gösterir. Dtu 20 kat azalma sonuçlanır ve 13 kat fiyat indirimi S3 veritabanlarının her birini yerleştirmeye kıyasla boyutları tek veritabanları için işlem.

Bu örnek aşağıdaki nedenlerle idealdir:

- Bir veritabanındaki en yüksek kullanım ile ortalama kullanım arasında büyük farklar mevcuttur.
- Bir veritabanının en yüksek kullanımı zamanın farklı noktalarında gerçekleşir.
- eDTU'lar birden fazla veritabanı arasında paylaşılır.

Bir havuzun fiyatı, havuz eDTU'larının bir işlevidir. Bir havuzun eDTU birim fiyatı, tek veritabanının DTU birim fiyatından 1,5 kat fazladır. Bununla birlikte **havuz eDTU'ları çok sayıda veritabanı tarafından paylaşılabilir ve toplam eDTU sayısı gereklidir**. Fiyatlandırma ve eDTU paylaşımındaki bu farklılıklar, havuzların sağlayabileceği tasarruf potansiyelinin temelini oluşturur.

Aşağıdaki kuralları veritabanı sayısı ve veritabanı kullanımıyla ilgili karşısında bir havuzu tek veritabanları için işlem boyutlarını kullanmaya kıyasla daha az maliyet doğurmasını yardımcı olur.

### <a name="minimum-number-of-databases"></a>En az veritabanı sayısı

Tek veritabanları için kaynakları toplam miktarı 1,5 havuz için gerekli kaynakları kat fazla ise elastik havuz daha uygun maliyetli.

***DTU tabanlı satın alma modeli örnek***<br>
100 eDTU havuzun tek veritabanları için işlem boyutlarını kullanmaktan daha uygun maliyetli olması en az iki S3 veritabanı veya en az 15 adet S0 veritabanı gereklidir.

### <a name="maximum-number-of-concurrently-peaking-databases"></a>Eşzamanlı olarak en üst seviyeye çıkan en fazla veritabanı sayısı

Kaynakları paylaşarak, bir havuzdaki tüm veritabanlarının aynı anda mevcut olan sınıra kadar kaynakları tek veritabanları için kullanabilirsiniz. Daha az sayıda eşzamanlı olarak en üst seviyeye, alt kaynakları ayarlanabileceği ve havuz daha uygun maliyetli hale gelir. Genel, fazla 2/3 (veya % 67) havuzdaki veritabanları, aynı anda kaynakları sınırlarını ulaşmalıdır.

***DTU tabanlı satın alma modeli örnek***

200 eDTU içeren bir havuzdaki üç S3 veritabanının maliyetlerini azaltmak için, bu veritabanlarının en fazla iki tanesi kullanım sırasında en üst seviyeye çıkabilir. Aksi takdirde, bu dört S3 veritabanının ikiden fazlası eşzamanlı olarak en üst seviyeye çıkarsa, havuzun boyutu 200 eDTU’dan fazla olmak zorundadır. Havuz 200 Edtu'dan boyutlandırılırsa, daha fazla S3 veritabanı boyutları tek veritabanları için işlem daha düşük tutulması için havuza eklenmesi gerekir.

Bu örnek, havuzdaki diğer veritabanlarının kullanımını dikkate almaz. Herhangi bir zamanda tüm veritabanlarının kullanımı aynı olursa, veritabanlarının 2/3’ünden (veya %67) daha azı eşzamanlı olarak en üst seviyeye çıkabilir.

### <a name="resource-utilization-per-database"></a>Veritabanı başına kaynak kullanımı

Bir veritabanının en yüksek ile ortalama kullanımı arasında büyük bir fark olması, uzun süreli düşük kullanımı ve kısa süreli yüksek kullanımı ifade eder. Bu kullanım modeli, veritabanları arasında kaynakların paylaşılması için idealdir. Bir veritabanının en yüksek kullanımı ortalama kullanımından 1,5 kat fazla olduğunda, veritabanı havuz için düşünülmelidir.

**DTU tabanlı satın alma modeli örnek**: En yüksek kullanımı 100 DTU’ya varan ve ortalama olarak en fazla 67 DTU kullanan bir S3 veritabanı, bir havuzda eDTU paylaşmak için iyi bir adaydır. Alternatif olarak, en yüksek kullanımı 20 DTU’ya varan ve ortalama olarak en fazla 13 DTU kullanan bir S1 veritabanı da havuz için iyi bir adaydır.

## <a name="how-do-i-choose-the-correct-pool-size"></a>Doğru havuz boyutunu nasıl seçerim

Bir havuz için en iyi boyut, havuzdaki tüm veritabanları için gereken toplama kaynakları bağlıdır. Bu belirleme aşağıdakileri içerir:

- (En fazla dtu sayısı veya en yüksek Vcore modeli kaynaklama dillerinden bağlı olarak) havuzdaki tüm veritabanları tarafından kullanılan en fazla kaynak.
- Havuzdaki tüm veritabanları tarafından kullanılan en fazla depolama baytı sayısı.

Her kaynak modeli için kullanılabilir hizmet katmanları için bkz: [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) veya [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md).

Araçları kullanamadığınız durumlarda aşağıdaki adım adım yönergeler bir havuzun tek veritabanlarından daha uygun maliyetli olup olmadığını tahmin etmenize yardımcı olabilir:

1. Edtu veya sanal çekirdek havuz için şu şekilde gerekli tahmin:

   DTU tabanlı satın alma modeli için: MAKS(<*Toplam veritabanı sayısı* X *Veritabanı başına ortalama DTU kullanımı*>,<br>  
   <*Eşzamanlı olarak en üst seviyeye çıkan veritabanı sayısı* X *Veritabanı başına en yüksek DTU kullanımı*)

   Sanal çekirdek tabanlı satın alma modeli için: Maks (<*toplam veritabanı sayısı* X *sanal çekirdek kullanımı veritabanı başına ortalama*>,<br>  
   <*Eşzamanlı olarak en üst seviyeye çıkan sayısı DBs* X *veritabanı başına en yüksek sanal çekirdek kullanımı*)

2. Havuzdaki tüm veritabanları için gereken bayt sayısını ekleyerek havuz için gereken depolama alanını tahmin edin. Ardından, bu depolama miktarını sağlayan eDTU havuz boyutunu belirleyin.
3. DTU tabanlı satın alma modeli için eDTU tahminleri büyük adım 1 ve 2 adımlardaki yararlanın. Sanal çekirdek tabanlı satın alma modeli için sanal çekirdek tahmini 1. adımdaki yararlanın.
4. Bkz: [SQL veritabanı fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/sql-database/) ve 3. adım tahminden büyük en küçük havuz boyutunu bulun.
5. 5\. adımdaki havuz fiyatını, tek veritabanları için uygun bilgi işlem boyutlarına kullanma fiyatıyla karşılaştırın.

## <a name="using-other-sql-database-features-with-elastic-pools"></a>Elastik havuzlar sayesinde diğer SQL veritabanı özelliklerini kullanma

### <a name="elastic-jobs-and-elastic-pools"></a>Esnek işler ve elastik havuzlar

Bir havuz kullanılarak **[esnek işlerde](elastic-jobs-overview.md)** betik çalıştırma yoluyla yönetim görevleri kolaylaştırılır. Elastik iş, çok sayıda veritabanından kaynaklanan sorunların çoğunu ortadan kaldırır.

Birden fazla veritabanıyla çalışmak için kullanılabilen diğer veritabanı araçları hakkında daha fazla bilgi için bkz. [Azure SQL Veritabanı ile ölçek genişletme](sql-database-elastic-scale-introduction.md).

### <a name="business-continuity-options-for-databases-in-an-elastic-pool"></a>Bir elastik havuzdaki veritabanları için iş sürekliliği seçenekleri

Havuza alınan veritabanları genellikle tek veritabanları için kullanılabilen [iş sürekliliği özelliklerinin](sql-database-business-continuity.md) aynılarını destekler.

- **Belirli bir noktaya geri yükleme**

  Belirli bir noktaya geri yükleme, bir veritabanı havuzunda belirli bir noktaya geri için otomatik veritabanı yedeklemelerini kullanır. Bkz. [Belirli Bir Noktaya Geri Yükleme](sql-database-recovery-using-backups.md#point-in-time-restore)

- **Coğrafi geri yükleme**

  Coğrafi geri yükleme, bir veritabanı bir veritabanı barındırıldığı bölgedeki bir olay nedeniyle kullanılamaz olduğunda varsayılan kurtarma seçeneğini sağlar. Bkz. [Bir Azure SQL Veritabanını geri yükleme veya ikincil veritabanına yük devretme](sql-database-disaster-recovery.md)

- **Etkin coğrafi çoğaltma**

  Coğrafi geri yüklemenin sunabileceğinden daha agresif kurtarma gereksinimleri olan uygulamalar için yapılandırma [etkin coğrafi çoğaltma](sql-database-active-geo-replication.md) veya [otomatik yük devretme grubu](sql-database-auto-failover-group.md).

## <a name="creating-a-new-sql-database-elastic-pool-using-the-azure-portal"></a>Azure portalını kullanarak yeni bir SQL veritabanı elastik havuz oluşturma

Azure portalında bir elastik havuz oluşturmanın iki yolu vardır.

1. Arayarak bir elastik havuz oluşturabilirsiniz **SQL esnek havuzu** içinde **Market** veya **+ Ekle** üzerinde SQL esnek havuzları dikey penceresine göz atın. Havuz sağlama iş akışı aracılığıyla yeni veya var olan sunucu güvendiklerini belirtebilirler.
2. Veya mevcut bir SQL server için gezinme ve tıklayarak elastik havuz oluşturabilirsiniz **havuz oluşturma** doğrudan bu sunucu bir havuz oluşturmak için. Burada tek fark, sunucu havuzu sağlama iş akışı sırasında belirttiğiniz adımı atla ' dir.

> [!NOTE]
> Bir sunucu üzerinde birden fazla havuzu oluşturabilirsiniz, ancak aynı havuza farklı sunuculara ait veritabanlarını ekleyemezsiniz.

Elastik havuz ve her bir veritabanı için kullanılabilir kaynakları en uzun süreyi için kullanılabilen özelliklerin havuzun hizmet katmanına belirler. Ayrıntılar için bkz elastik havuzlar için kaynak sınırları [DTU model](sql-database-dtu-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-compute-sizes). Elastik havuzlar için sanal çekirdek tabanlı kaynak sınırları için bkz: [sanal çekirdek tabanlı kaynak sınırları - elastik havuzlar](sql-database-vcore-resource-limits-elastic-pools.md).

Kaynaklarını yapılandırmak ve havuzu, fiyatlandırma **havuzu Yapılandır**. Bir hizmet katmanını seçip, havuza veritabanları ekleyebilir ve havuz ve veritabanlarında için kaynak sınırları yapılandırın.

Havuz yapılandırma tamamladıktan sonra havuzu adı ' Uygula' öğesine tıklayın ve 'havuzu oluşturmak için Tamam' ı tıklatın.

## <a name="monitor-an-elastic-pool-and-its-databases"></a>Elastik havuz ve veritabanlarını izleme

Azure portalında bir elastik havuz ve bu havuzdaki veritabanlarının kullanımını izleyebilirsiniz. Ayrıca, elastik havuzunuz için bir grup değişiklik yapmak ve aynı anda tüm değişiklikleri gönderir. Bu değişiklikler, ekleme veya kaldırma veritabanları, elastik havuz ayarlarınızı değiştirme veya veritabanı ayarlarınızı değiştirme içerir.

Elastik havuzunuzu izlemeye başlamak için bulun ve elastik havuzu portalda açın. Önce elastik havuzun durumunu genel bir fikir veren bir ekran görürsünüz. Buna aşağıdakiler dahildir:

- Elastik havuz kaynak kullanımını gösteren grafikler izleme
- Son uyarıları ve öneriler, esnek havuz için kullanılabilir olması durumunda

Aşağıdaki grafikte örnek bir elastik havuz gösterilmektedir:

![Havuz görüntüle](./media/sql-database-elastic-pool-manage-portal/basic.png)

Havuz hakkında daha fazla bilgi istiyorsanız bu genel bakışta kullanılabilir bilgileri tıklayabilirsiniz. Tıklayarak **kaynak kullanımını** grafik yönlendirilirsiniz Azure izleme görünümü nerede grafikte gösterilen ölçümleri ve zaman penceresini özelleştirebilirsiniz. Kullanılabilir bildirimlere tıklayarak, uyarı veya öneri tam ayrıntılarını gösteren bir dikey penceresine yönlendirir.

Veritabanları, havuz içindeki izlemek istiyorsanız, tıklayabilirsiniz **veritabanı kaynak kullanımı** içinde **izleme** ve kaynak menüsünde sol bölümü.

![Veritabanı kaynak kullanımı sayfası](./media/sql-database-elastic-pool-manage-portal/db-utilization.png)

### <a name="to-customize-the-chart-display"></a>Grafik görüntüsünü özelleştirmek için

Grafiği ve ölçüm sayfasını CPU yüzdesi, veri g/ç yüzdesi ve kullanılan günlük g/ç yüzdesi gibi diğer ölçümler görüntülenecek şekilde düzenleyebilirsiniz.

Üzerinde **grafiği Düzenle** form, sabit bir zaman seçebilirsiniz aralığı veya tıklayın **özel** son iki hafta içinde herhangi bir 24 saatlik penceresi seçin ve sonra izlemek istediğiniz kaynakları seçin.

### <a name="to-select-databases-to-monitor"></a>İzlemek için veritabanlarını seçin

Varsayılan olarak, grafikte **veritabanı kaynak kullanımı** dikey penceresinde, DTU veya CPU tarafından en çok kullanılan 5 veritabanları (hizmet katmanınızın) bağlı olarak gösterilir. Bu grafikte veritabanlarını seçerek ve soldaki onay kutularını aracılığıyla grafiği aşağıdaki listeden veritabanlarını seçimini geçebilirsiniz.

Daha fazla ölçümleri görüntülemek yan yana veritabanları performansınızı daha eksiksiz bir görünümünü elde etmek için bu veritabanı tablosunda de seçebilirsiniz.

Daha fazla bilgi için [Azure portalında SQL veritabanı uyarıları oluşturma](sql-database-insights-alerts-portal.md).

## <a name="customer-case-studies"></a>Müşteri örnek olay incelemeleri

- [SnelStart](https://azure.microsoft.com/resources/videos/azure-sql-database-case-study-snelstart/)

  SnelStart, ayda 1.000 yeni Azure SQL veritabanı bir hızda Kurumsal hizmetlerini hızla genişletmek için Azure SQL veritabanı ile elastik havuzları kullanılır.

- [Umbraco](https://azure.microsoft.com/resources/videos/azure-sql-database-case-study-umbraco/)

  Umbraco, hızlı sağlama ve ölçeklendirme hizmetlerine kiracılar bulutta binlerce ile Azure SQL veritabanı elastik havuzları kullanır.

- [Daxko/CSI](https://customers.microsoft.com/story/csi-used-azure-to-accelerate-its-development-cycle-and-to-enhance-its-customer-services)

  Daxko/CSI kendi geliştirme döngüsünü hızlandırmalarına ve müşteri hizmet ve performansı artırmak için Azure SQL veritabanı ile elastik havuzları kullanır.

## <a name="next-steps"></a>Sonraki adımlar

- Elastik havuzlar ölçeklendirmek için bkz: [elastik ölçeklendirme](sql-database-elastic-pool.md) ve [- örnek kodu bir elastik havuzu ölçekleme](scripts/sql-database-monitor-and-scale-pool-powershell.md)
- Video için bkz. [Azure SQL veritabanı elastiklik özellikleriyle ilgili Microsoft Virtual Academy video dersi](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)
- Elastik havuzları kullanan SaaS uygulamalarının tasarım desenleri hakkında daha fazla bilgi edinmek için bkz. [Azure SQL Veritabanı kullanan Çok Kiracılı SaaS Uygulamaları için Tasarım Desenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md).
- Esnek havuzları kullanan bir SaaS öğretici için bkz. [Wingtip SaaS uygulamasına giriş](sql-database-wtp-overview.md).
