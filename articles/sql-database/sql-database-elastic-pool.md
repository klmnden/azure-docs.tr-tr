---
title: Esnek havuzları Azure ile birden çok SQL veritabanlarını yönetme | Microsoft Docs
description: Yönetmek ve birden çok SQL veritabanı - ölçeklendirme yüzlerce ve binlik - esnek havuzlarını kullanarak. Bir fiyat kaynakların gerektiğinde dağıtabilirsiniz.
keywords: birden çok veritabanı, veritabanı kaynakları, veritabanı performansı
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.date: 06/20/2018
ms.author: ninarn
ms.topic: conceptual
ms.openlocfilehash: 5ef32b231a77906a6840ad3550e81b631ddc0c13
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309663"
---
# <a name="elastic-pools-help-you-manage-and-scale-multiple-azure-sql-databases"></a>Esnek havuz yönetmek ve birden çok Azure SQL veritabanı ölçekleme Yardım

SQL Database esnek havuzlar, yönetme ve değişen ve tahmin edilemeyen kullanım taleplerini sahip birden çok veritabanı ölçekleme için basit ve düşük maliyetli bir çözümdür. Esnek havuzdaki veritabanları tek bir Azure SQL veritabanı sunucusuna olan ve kaynak kümesi fiyata kümesi sayısı paylaşın. Azure SQL Veritabanındaki elastik havuzlar, SaaS geliştiricilerinin bir veritabanı grubuna ait fiyat performansını belirtilen bütçe dahilinde iyileştirmesini ve aynı zamanda her veritabanı için performans Elastikliği sunmasını sağlar.

## <a name="what-are-sql-elastic-pools"></a>SQL esnek havuzu nelerdir?

SaaS geliştiricileri, birden fazla veritabanından oluşan büyük ölçekli veri katmanlarının üzerinde uygulamalar oluşturur. Her müşteri için tek veritabanı sağlanması yaygın bir uygulama modelidir. Ancak, farklı müşteriler genellikle değişen ve tahmin edilemeyen kullanım modellerine sahiptir ve her veritabanı kullanıcısının kaynak gereksinimlerini tahmin etmek zordur. Geleneksel olarak, iki seçenek vardı:

- En yüksek kullanımı ve ödeme, üzerinden dayalı kaynakları aşırı sağlamak veya
- Maliyeti, performansı ve müşteri memnuniyetini yükselmeleri sırasında ödün verme pahasına kaydetmek için eksik sağlama.

Esnek havuzlar veritabanları ihtiyaç duydukları gereksinim performans kaynakları alma sağlayarak bu sorunu çözün. Bunlar, tahmin edilebilir bir bütçe içinde basit bir kaynak ayırma mekanizması sağlar. Esnek havuzları kullanan SaaS uygulamalarının tasarım desenleri hakkında daha fazla bilgi edinmek için bkz. [Azure SQL Database kullanan Çok Kiracılı SaaS Uygulamaları için Tasarım Desenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md).

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Elastic-databases-helps-SaaS-developers-tame-explosive-growth/player]
>

Esnek havuzlar kaynakları tahmin edilemeyen kullanım dönemlerini uyum sağlamak için birden çok veritabanı tarafından ayrı veritabanlarını tarafından paylaşılan bir havuz için satın almak Geliştirici etkinleştirin. Havuz ya da temel için kaynaklarını yapılandırabilirsiniz [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) veya [vCore tabanlı satın alma modeli (Önizleme)](sql-database-service-tiers-vcore.md). Bir havuz için kaynak gereksinimi veritabanlarını toplama kullanımı tarafından belirlenir. Kaynak havuzunun kullanımına miktarını Geliştirici bütçe tarafından denetlenir. Geliştirici yalnızca veritabanları havuzuna ekler, veritabanları için minimum ve maksimum kaynakları ayarlar (minimum ve maksimum Dtu ya da minimum veya maksimum vCores modeli resourcing, tercih ettiğiniz bağlı olarak) ve kaynaklarına göre havuzunun ayarlar kendi Bütçe. Geliştirici, hizmetini zayıf bir başlangıçtan sürekli artan ölçekte olgun bir işletmeye sorunsuzca büyütmek için havuzları kullanabilir.

Havuz içerisinde tek tek veritabanlarına belirli parametreler içinde otomatik olarak ölçeklendirme esnekliği tanınır. Ağır yük altında bir veritabanı talebi karşılamak üzere daha fazla kaynak kullanabilir. Hafif yükleri altındaki veritabanları daha az tüketmesine ve veritabanları hiçbir yük altında hiçbir kaynaklarını tüketebilir. Tek tek veritabanları yerine tüm havuz için kaynak sağlamak, yönetim görevlerinizi basitleştirir. Ayrıca, havuz için tahmin edilebilir bir bütçe sahip. Veritabanları için yeni eDTU ayırma ek işlem kaynaklarını sağlamaya taşınması gerekebilir dışında hiçbir veritabanı kapalı kalma süresi ile var olan bir havuzu için ek kaynaklar eklenebilir. Ek Kaynaklar artık gerekirse benzer şekilde, bunlar herhangi bir noktada mevcut bir havuzdan zamanında kaldırılabilir. Ayrıca havuza veritabanları ekleyebilir veya havuzdan veritabanları kaldırabilirsiniz. Bir veritabanı kaynakları tahmin edilebilir bir şekilde normalden az kullanıyorsa bu veritabanını havuzdan çıkarın.

## <a name="when-should-you-consider-a-sql-database-elastic-pool"></a>Ne zaman bir SQL Database esnek havuzunu dikkat etmeliyim?

Havuzlar, belirli kullanım düzenlerine sahip çok sayıda veritabanı bulunan durumlar için çok uygundur. Söz konusu kullanım düzeni, belirli bir veritabanı için ortalama düşük düzeyde kullanım ile nispeten nadir zamanlarda kullanımın ani olarak artması şeklindedir.

Bir havuza ekleyebileceğiniz veritabanı sayısı arttıkça, tasarruflarınız artar. Uygulama kullanım modelinize bağlı olarak, yalnızca iki S3 veritabanı ile tasarruf edildiğini görmek mümkündür.

Aşağıdaki bölümler veritabanı koleksiyonunuzun bir havuzda olmasının yararlarını nasıl değerlendireceğini anlamanıza yardımcı olabilir. Örneklerde Standart havuzlar kullanılmaktadır, ancak aynı ilkeler Temel ve Premium havuzlar için de geçerlidir.

### <a name="assessing-database-utilization-patterns"></a>Veritabanı kullanım modellerini değerlendirme

Aşağıdaki şekilde zamanın büyük bölümünü boşta geçiren, ancak düzenli olarak ani etkinlikler sergileyen bir veritabanı örneğini göstermektedir. Bu model bir havuz için uygun olan kullanım modelidir:

   ![havuz için uygun bir tek veritabanı](./media/sql-database-elastic-pool/one-database.png)

Gösterilen beş dakikalık süre boyunca Veritabanı1, 90 DTU’ya kadar yükselir, ancak genel ortalama kullanım beş DTU’dan azdır. Bu iş yükünü tek veritabanında çalıştırmak için S3 performans düzeyi gereklidir, ancak bu düzey düşük etkinlik dönemlerinde kaynakların çoğunu kullanılmamış halde bırakır.

Havuz bu kullanılmayan DTU’ların birden fazla veritabanında paylaşılmasına olanak tanır ve böylece gereken DTU ile genel maliyeti azaltır.

Önceki örnekten devam ederek, Veritabanı1 ile benzer kullanım modellerine sahip ek veritabanları olduğunu varsayalım. Sonraki iki resimde içinde dört veritabanları ve 20 veritabanları kullanımını katmanlı kullanımlarını DTU tabanlı satın alma modeli kullanarak zamanla çakışmayan doğasına göstermek için aynı grafik üzerine:

   ![bir havuz için uygun kullanım modeli ile dört veritabanı](./media/sql-database-elastic-pool/four-databases.png)

   ![bir havuz için uygun kullanım modeli ile yirmi veritabanı](./media/sql-database-elastic-pool/twenty-databases.png)

20 veritabanının tamamındaki toplam DTU kullanımı, önceki şekilde siyah çizgi ile gösterilmiştir. Bu şekil, toplam DTU kullanımının 100 DTU’yu hiçbir zaman aşmadığını ve 20 veritabanının bu süre boyunca 100 eDTU’yu paylaşabileceğini gösterir. Bu durum, tek veritabanları için S3 performans düzeylerindeki veritabanlarının her birini yerleştirmeye kıyasla DTU sayısında 20 kat azalmaya ve 13 kat fiyat azalmasına yol açar.

Bu örnek aşağıdaki nedenlerle idealdir:

* Bir veritabanındaki en yüksek kullanım ile ortalama kullanım arasında büyük farklar mevcuttur.
* Bir veritabanının en yüksek kullanımı zamanın farklı noktalarında gerçekleşir.
* eDTU'lar birden fazla veritabanı arasında paylaşılır.

Bir havuzun fiyatı, havuz eDTU'larının bir işlevidir. Bir havuzun eDTU birim fiyatı, tek veritabanının DTU birim fiyatından 1,5 kat fazladır. Bununla birlikte **havuz eDTU'ları çok sayıda veritabanı tarafından paylaşılabilir ve toplam eDTU sayısı gereklidir**. Fiyatlandırma ve eDTU paylaşımındaki bu farklılıklar, havuzların sağlayabileceği tasarruf potansiyelinin temelini oluşturur.

Veritabanı sayısı ve veritabanı kullanımıyla ilgili aşağıdaki temel kurallar, bir havuzun tek veritabanları için kullanılan performans düzeylerine kıyasla daha az maliyet doğurmasını sağlar.

### <a name="minimum-number-of-databases"></a>En az veritabanı sayısı

Birleşik miktarda kaynak tek veritabanları için birden fazla 1.5 havuzu için gereken kaynakları x ise, bir esnek havuz daha uygun maliyetli olması.

***DTU tabanlı satın alma model örneği***<br>
100 eDTU havuzun tek veritabanı performans düzeylerini kullanmaya kıyasla daha uygun maliyetli olması için en az iki S3 veritabanı veya en az 15 adet S0 veritabanı gereklidir.

### <a name="maximum-number-of-concurrently-peaking-databases"></a>Eşzamanlı olarak en üst seviyeye çıkan en fazla veritabanı sayısı

Kaynakları paylaşarak, bir havuzdaki tüm veritabanları aynı anda kaynakları kullanılabilir sınırına kadar tek veritabanları için kullanabilir. Daha az veritabanları, aynı anda yoğun saatler, düşük havuz kaynakları ayarlanabilir ve daha düşük maliyetli havuzu olur. Genel, veritabanlarının havuzdaki geçmeyen 2/3 (veya % 67) aynı anda kaynakları sınırlarını tepe.

***DTU tabanlı satın alma model örneği***<br>
200 eDTU içeren bir havuzdaki üç S3 veritabanının maliyetlerini azaltmak için, bu veritabanlarının en fazla iki tanesi kullanım sırasında en üst seviyeye çıkabilir. Aksi takdirde, bu dört S3 veritabanının ikiden fazlası eşzamanlı olarak en üst seviyeye çıkarsa, havuzun boyutu 200 eDTU’dan fazla olmak zorundadır. Havuz 200 eDTU’dan fazlasına yeniden boyutlandırılırsa, maliyetin tek veritabanı performans düzeylerinden düşük tutulması için havuza daha fazla S3 veritabanı eklenmesi gerekir.

Bu örnek, havuzdaki diğer veritabanlarının kullanımını dikkate almaz. Herhangi bir zamanda tüm veritabanlarının kullanımı aynı olursa, veritabanlarının 2/3’ünden (veya %67) daha azı eşzamanlı olarak en üst seviyeye çıkabilir.

### <a name="resource-utilization-per-database"></a>Veritabanı başına kaynak kullanımı
Bir veritabanının en yüksek ile ortalama kullanımı arasında büyük bir fark olması, uzun süreli düşük kullanımı ve kısa süreli yüksek kullanımı ifade eder. Bu kullanım modeli, veritabanları arasında kaynakların paylaşılması için idealdir. Bir veritabanının en yüksek kullanımı ortalama kullanımından 1,5 kat fazla olduğunda, veritabanı havuz için düşünülmelidir.

***DTU tabanlı satın alma model örneği***<br>
En yüksek kullanımı 100 DTU’ya varan ve ortalama olarak en fazla 67 DTU kullanan bir S3 veritabanı, bir havuzda eDTU paylaşmak için iyi bir adaydır. Alternatif olarak, en yüksek kullanımı 20 DTU’ya varan ve ortalama olarak en fazla 13 DTU kullanan bir S1 veritabanı da havuz için iyi bir adaydır.

## <a name="how-do-i-choose-the-correct-pool-size"></a>Doğru havuz boyutunun nasıl seçer?

Bir havuz için en iyi boyutu havuzdaki tüm veritabanları için gereken birleşik kaynakları bağlıdır. Bu, aşağıdaki belirleme içerir:

* (En fazla Dtu'lar veya model resourcing, tercih ettiğiniz bağlı olarak en fazla vCores) havuzdaki tüm veritabanları tarafından kullanılan en fazla kaynak.
* Havuzdaki tüm veritabanları tarafından kullanılan en fazla depolama baytı sayısı.

Her kaynak modeli için kullanılabilir hizmet katmanları için bkz: [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) veya [vCore tabanlı satın alma modeli (Önizleme)](sql-database-service-tiers-vcore.md).

SQL Veritabanı, mevcut bir SQL Veritabanı sunucusundaki veritabanlarının geçmiş kaynak kullanımını otomatik olarak değerlendirir ve Azure portalda uygun havuz yapılandırmasını önerir. Önerilere ek olarak, yerleşik deneyim sunucu üzerindeki özel bir veritabanı grubu için eDTU kullanımını tahmin eder. Bu deneyim, havuza veritabanlarını etkileşimli bir şekilde ekleyerek ve değişiklikleri uygulamadan önce kaynak kullanım analizi ile boyutlandırma önerisini almak üzere veritabanlarını kaldırarak "durum" çözümlemesi yapmanıza olanak tanır. Nasıl yapılır konuları için bkz. [Elastik havuzlarını izleme, yönetme ve boyutlandırma](#monitor-an-elastic-pool-and-its-databases).

Araçları kullanamadığınız durumlarda aşağıdaki adım adım yönergeler bir havuzun tek veritabanlarından daha uygun maliyetli olup olmadığını tahmin etmenize yardımcı olabilir:

1. Edtu veya havuza gibi gerekli vCores tahmin edersiniz:

   DTU tabanlı satın alma modeli için: en fazla (<*DBs toplam sayısı* X *ortalama DTU kullanımı DB başına*>,<br>
   <*Eşzamanlı olarak en üst seviyeye çıkan veritabanı sayısı* X *Veritabanı başına en yüksek DTU kullanımı*)

   Satın alma modeli vCore tabanlı (Önizleme) için: en fazla (<*DBs toplam sayısı* X *ortalama DB başına vCore kullanımını*>,<br>
   <*Aynı anda peaking sayısı DBs* X *DB başına en yüksek vCore kullanımını*)

2. Havuzdaki tüm veritabanları için gereken bayt sayısını ekleyerek havuz için gereken depolama alanını tahmin edin. Ardından, bu depolama miktarını sağlayan eDTU havuz boyutunu belirleyin.
3. DTU tabanlı satın alma modeli için eDTU tahminleri daha büyük adım 1 ve 2. adım alın. VCore tabanlı satın alma modeli için (Önizleme), 1. adımdan vCore tahmin alın.
4. Bkz: [SQL veritabanı fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/sql-database/) ve en küçük havuz boyutu Bul adım 3 tahmin büyüktür.
5. 5 Adımdaki havuz fiyatını, tek veritabanları için uygun performans düzeylerini kullanma fiyatıyla karşılaştırın.

## <a name="using-other-sql-database-features-with-elastic-pools"></a>Esnek havuzları ile diğer SQL veritabanı özelliklerini kullanma

### <a name="elastic-jobs-and-elastic-pools"></a>Esnek işler ve esnek havuzlar

Bir havuz kullanılarak **[esnek işlerde](sql-database-elastic-jobs-overview.md)** betik çalıştırma yoluyla yönetim görevleri kolaylaştırılır. Elastik iş, çok sayıda veritabanından kaynaklanan sorunların çoğunu ortadan kaldırır. Başlamak için bkz. [Elastik işlerle çalışmaya başlama](sql-database-elastic-jobs-getting-started.md).

Birden fazla veritabanıyla çalışmak için kullanılabilen diğer veritabanı araçları hakkında daha fazla bilgi için bkz. [Azure SQL Veritabanı ile ölçek genişletme](sql-database-elastic-scale-introduction.md).

### <a name="business-continuity-options-for-databases-in-an-elastic-pool"></a>Esnek havuzdaki veritabanları için iş sürekliliği seçenekleri
Havuza alınan veritabanları genellikle tek veritabanları için kullanılabilen [iş sürekliliği özelliklerinin](sql-database-business-continuity.md) aynılarını destekler.

- **Zaman içinde nokta geri yükleme**: zaman içinde nokta geri yükleme zamanında belirli bir noktaya bir havuzdaki bir veritabanını kurtarmak için otomatik veritabanı yedeklemeyi kullanır. Bkz. [Belirli Bir Noktaya Geri Yükleme](sql-database-recovery-using-backups.md#point-in-time-restore)

- **Coğrafi geri yükleme**: coğrafi geri yükleme, bir veritabanı, veritabanı barındırıldığı bölgede bir olay nedeniyle kullanılamaz duruma geldiğinde varsayılan kurtarma seçeneği sağlar. Bkz. [Bir Azure SQL Veritabanını geri yükleme veya ikincil veritabanına yük devretme](sql-database-disaster-recovery.md)

- **Aktif coğrafi çoğaltma**: coğrafi geri yükleme sunabileceğiniz çok daha agresif kurtarma gereksinimlerine sahip uygulamalar için yapılandırma [aktif coğrafi çoğaltma](sql-database-geo-replication-overview.md).

## <a name="creating-a-new-sql-database-elastic-pool-using-the-azure-portal"></a>Azure Portalı'nı kullanarak yeni bir SQL Database esnek havuzunu oluşturma

Azure portalında bir esnek havuz oluşturmanın iki yolu vardır.
1. Arayarak bir esnek havuz oluşturabilirsiniz **SQL esnek havuzu** içinde **Market** veya tıklatarak **+ Ekle** SQL esnek havuzlarını dikey göz atın. İş akışı sağlama bu havuzu aracılığıyla yeni veya var olan bir sunucuyu belirtmek kullanabilirsiniz.
2. Veya mevcut bir SQL sunucusu gezinme ve tıklayarak bir esnek havuz oluşturabilirsiniz **havuzu oluşturma** doğrudan o sunucuya bir havuzu oluşturmak için. Burada tek fark, sunucu havuzu iş akışı sağlama sırasında belirttiğiniz adımı atlayın olmasıdır.

> [!NOTE]
> Bir sunucuda birden çok havuz oluşturabilirsiniz, ancak aynı havuza farklı sunuculara ait veritabanlarını ekleyemezsiniz.

Havuzun hizmet katmanı havuzu ve en büyük miktarda kaynak her veritabanı için kullanılabilen elastics için kullanılabilen özelliklerin belirler. Ayrıntılar için bkz: kaynak sınırlarını esnek havuzlar için [DTU modeli](sql-database-dtu-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-performance-levels). Esnek havuzlar için vCore tabanlı kaynak sınırları için bkz: [vCore tabanlı kaynak sınırları - esnek havuzlar](sql-database-vcore-resource-limits-elastic-pools.md).

Kaynaklarını yapılandırmak ve havuzu, fiyatlandırma tıklatın **havuzu yapılandırma**. Bir hizmet katmanı seçin, havuza veritabanı ekleme ve havuzu ve onun veritabanları için kaynak sınırlarını yapılandırın.

Havuz yapılandırma tamamladıktan sonra 'Uygula', adı havuzu tıklatın ve 'havuzu oluşturmak için Tamam' ı tıklatın.

## <a name="monitor-an-elastic-pool-and-its-databases"></a>Bir esnek havuz ve veritabanlarını izleme

Azure portalında bir esnek havuz ve bu havuz içindeki veritabanlarının kullanımını izleyebilirsiniz. Ayrıca, esnek havuz için bir değişiklik kümesini yapın ve aynı anda tüm değişiklikleri gönderir. Bu değişiklikler ekleyerek veya veritabanları kaldırma, esnek havuz ayarlarınızı değiştirme veya veritabanı ayarlarını değiştirerek içerir.

Esnek havuz izlemeye başlamak için bulun ve Portalı'nda bir esnek havuz açın. Önce esnek havuz durumunu genel bir fikir veren bir ekran görürsünüz. Buna aşağıdakiler dahildir:

* Esnek havuz kaynakların kullanımını gösteren grafikleri izleme
* Son uyarıları ve öneriler, esnek havuz için kullanılabilir olması durumunda

Aşağıdaki grafikte bir örnek esnek havuz gösterir:

![Havuz görünümü](./media/sql-database-elastic-pool-manage-portal/basic.png)

Havuz hakkında daha fazla bilgi istiyorsanız, bu genel bakışta kullanılabilir bilgi hiçbirinde tıklatabilirsiniz. Tıklayarak **kaynak kullanımı** grafik yönlendirilirsiniz Azure Monitoring görünümüne grafikte gösterilen ölçümleri ve zaman penceresini burada özelleştirebilirsiniz. Kullanılabilir bildirimlere tıklatarak o uyarı ya da öneri tam ayrıntılarını gösteren bir dikey penceresine yönlendirir.

Havuzunuzu içinde veritabanları izlemek istiyorsanız tıklatabilirsiniz **veritabanı kaynak kullanımını** içinde **izleme** soldaki kaynak menünün bölümünde.

![Veritabanı kaynak kullanımı sayfası](./media/sql-database-elastic-pool-manage-portal/db-utilization.png)

### <a name="to-customize-the-chart-display"></a>Grafik görüntüsünü özelleştirmek için

Grafik ve diğer ölçümleri CPU yüzdesi, veri g/ç yüzdesi ve kullanılan günlük GÇ yüzdesi gibi görüntülenecek ölçüm sayfa düzenleyebilirsiniz.

Üzerinde **grafiği Düzenle** form, sabit bir zaman seçebilirsiniz aralığı veya tıklatın **özel** son iki hafta içinde herhangi bir 24 saatlik penceresi seçin ve ardından izlemek için kaynakları seçin.

### <a name="to-select-databases-to-monitor"></a>İzlemek üzere seçmek için veritabanları

Varsayılan olarak, grafikte **veritabanı kaynak kullanımını** dikey DTU veya CPU ilk 5 veritabanları (bağlı olarak, hizmet katmanı) gösterir. Bu grafikteki veritabanlarını seçerek ve onay kutularını soldaki aracılığıyla grafiği aşağıdaki listeden veritabanları unselecting geçebilirsiniz.

Daha fazla ölçümleri yan yana veritabanları performansınızı daha eksiksiz bir görünümünü almak için bu veritabanı tablosunda görünümüne de seçebilirsiniz.

Daha fazla bilgi için bkz: [Azure Portalı'nda SQL veritabanı uyarıları oluşturma](sql-database-insights-alerts-portal.md).

## <a name="next-steps"></a>Sonraki adımlar

- Esnek havuzlar ölçeklendirmek için bkz: [esnek havuzlar ölçeklendirme](sql-database-elastic-pool.md) ve [bir esnek havuz - örnek kod ölçeklendirme](scripts/sql-database-monitor-and-scale-pool-powershell.md)
* Video için bkz: [Microsoft Virtual Academy video indirmelere Azure SQL Database esnek özellikleri hakkında](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)
* Esnek havuzları kullanan SaaS uygulamalarının tasarım desenleri hakkında daha fazla bilgi edinmek için bkz. [Azure SQL Database kullanan Çok Kiracılı SaaS Uygulamaları için Tasarım Desenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md).
* Esnek havuzları kullanan SaaS öğretici için bkz: [Wingtip SaaS uygulamasına giriş](sql-database-wtp-overview.md).
