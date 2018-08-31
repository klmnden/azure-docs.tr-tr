---
title: Azure SQL veritabanı SSS | Microsoft Docs
description: Yaygın soruların müşterilere yanıtlar, bulutta bir hizmet olarak bulut veritabanları ve Azure SQL veritabanı, Microsoft'un ilişkisel veritabanı yönetim sistemi (RDBMS) ve veritabanı hakkında isteyin.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: reference
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: carlrab
ms.openlocfilehash: a7837ac6af82b5c67ea5779340aedc16cb78d156
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43286341"
---
# <a name="sql-database-faq"></a>SQL Veritabanı SSS

## <a name="what-is-the-current-version-of-sql-database"></a>SQL veritabanı'nın geçerli sürümü nedir?
Geçerli sürümü, SQL veritabanı V12 ' dir. Sürüm V11 kullanımdan kaldırılmıştır.

## <a name="what-is-the-sla-for-sql-database"></a>SQL veritabanı SLA'sı nedir?
Süre en az % 99,99 oranında kullanılabilir olacağına garanti veriyoruz, Microsoft Azure SQL veritabanı ile Internet ağ geçidimizle arasında hizmet katmanınızın bağımsız olarak bağlantınız. %0,01 düzeltme ekleri, yükseltmeleri ve yük devretme işlemleri için ayrılmıştır. Daha fazla bilgi için [SLA](http://azure.microsoft.com/support/legal/sla/). Azure SQL veritabanı kullanılabilirlik mimarisi hakkında daha fazla bilgi için bkz. [yüksek kullanılabilirlik ve Azure SQL veritabanı](sql-database-high-availability.md). 

## <a name="can-i-control-when-patching-downtime-occurs"></a>Kapalı kalma süresi düzeltme eki uygulama oluştuğunda denetleyebilirim
Hayır. Düzeltme eki uygulama etkisini genellikle gözümüzün değil ise, [yeniden deneme mantığı uyguluyor](sql-database-develop-overview.md#resiliency) uygulamanızda.
## <a name="what-is-the-new-vcore-based-purchasing-model-for-azure-sql-database"></a>Azure SQL veritabanı için yeni sanal çekirdek tabanlı satın alma modeli nedir?

Yeni satın alma modeli, mevcut DTU temelli model üzerinde yapılan bir eklemedir. Sanal çekirdek tabanlı model, müşterilere esneklik, denetimi, saydamlık sağlamak üzere tasarlanmıştır ve basit bir yol çevirmek için şirket iş yükü gereksinimlerini buluta. Ayrıca, iş yükü ihtiyaçlarını alarak kendi işlem ve depolama kaynakları yönetebileceğiniz ölçeklendirme müşterilerin olanak tanır. Tek veritabanı ve elastik havuz seçenekleri vCore modeli kullanarak da ile yüzde 30 tasarruf için uygun yedekleme [SQL Server için Azure hibrit kullanım teklifi](../virtual-machines/windows/hybrid-use-benefit-licensing.md). Bkz: [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md) daha fazla bilgi için. 

## <a name="what-is-a-vcore"></a>Sanal çekirdek nedir? 
Sanal çekirdek, donanım Nesilleri arasında seçim yapma olanağı ile sunulan mantıksal CPU'yu temsil eder. Gen 4 mantıksal CPU'lar Intel E5-2673 v3 (Haswell) 2,4 GHz işlemcileri ve genel 5 mantıksal CPU'lar Intel E5-2673 v4 dayalı (Broadwell) 2.3 GHz işlemcileri.

## <a name="is-moving-to-the-vcore-based-model-required"></a>Sanal çekirdek tabanlı modele geçiş yapmak zorunlu mudur?
Hayır, müşteri tercihlerine ve esnekliğe olan taahhüdümüzün bir elastik havuz ile tek veritabanı dağıtım seçeneklerinde sanal çekirdek tabanlı modelin giriş yansıtır. Müşteriler, DTU tabanlı modeli kullanmaya devam etmek istiyorsanız, bu duyuru ve deneyimlerini herhangi bir işlem yapmanız gerekmez ve faturalandırma değişmeden kalır. 

Çoğu durumda, önceden yapılandırılmış bir paket kaynak basitliğinin uygulamalar yararlı olabilir. Bu nedenle, sunmak ve müşterilerimize bu DTU tabanlı seçenekleri destekler devam ediyoruz. Bunları kullanmakta olduğunuz ve iş gereksinimlerinizi karşılayan, bunu yapmak devam.

DTU ve sanal çekirdek tabanlı modeller birlikte sunulmaya devam edecektir. Size daha fazla şeffaflığın veritabanı kaynakları ve işlem ve depolama kaynaklarını ayrı olarak ölçeklendirebilme olanağı için müşteri isteklerine yanıt olarak sanal çekirdek tabanlı modeli kullanıma sunuyoruz. Sanal çekirdek tabanlı model ayrıca ek tasarruf için Azure hibrit avantajı etkin Yazılım Güvencesi olan müşteriler SQL Server için etkinleştirir.

## <a name="how-should-i-choose-between-the-dtu-based-purchasing-model-vs-the-vcore-based-purchasing-model"></a>DTU tabanlı satın alma modeli vs arasında sanal çekirdek tabanlı satın alma modeli nasıl seçmeliyim? 
Veritabanı İşlem Birimi (DTU); CPU, bellek, okuma ve yazma eylemlerinin karma bir ölçümüne dayalıdır. DTU tabanlı performans düzeyleri, farklı uygulama performansı düzenleri elde etmek için önceden yapılandırılmış kaynak paketlerini temsil eder. Müşterilerin, temel alınan kaynaklarla ilgili endişe ve her ay sabit bir tutar ödeme sırasında önceden yapılandırılmış bir paketin basitliğini tercih etmek istiyor musunuz DTU tabanlı model ihtiyaçları için daha uygun fark edebilirsiniz. Ancak, temel alınan kaynaklara ilişkin daha fazla içgörüye ihtiyacınız veya bunları birbirinden bağımsız olarak en iyi performans elde etmek için ölçeklendirmek gereken müşteriler için sanal çekirdek tabanlı model en iyi seçenek olacaktır.  Ayrıca, bir müşterinin SQL Server için bir etkin Yazılım Güvencesi (SA) varsa, bunlar var olan yatırımları yararlanın ve % 30 ile tasarruf [SQL Server için Azure hibrit kullanım teklifi](../virtual-machines/windows/hybrid-use-benefit-licensing.md).  Seçenekler her satın alma modeli, otomatik yedeklemeler, yazılım güncelleştirmeleri ve düzeltme ekleri gibi tam olarak yönetilen bir hizmet avantajlarını sağlar. 

## <a name="what-is-the-azure-hybrid-benefit-for-sql-server"></a>SQL Server için Azure Hibrit Avantajı nedir? 
[SQL Server için Azure hibrit kullanım teklifi](../virtual-machines/windows/hybrid-use-benefit-licensing.md) , mevcut lisans yatırımlarınızdan değerini en üst düzeye çıkarmak ve bunların geçiş bulut yolculuğunuzu hızlandırın yardımcı olur. SQL Server için Azure hibrit avantajı, Yazılım Güvencesi içeren SQL Server lisanslarınızı kullanarak SQL veritabanı'nda indirimli ücret ("taban fiyat") ödemenize olanak sağlayan bir Azure tabanlı avantajdır. SQL Server için Azure hibrit avantajı, SQL veritabanı tek veritabanları ve elastik havuzlar için sanal çekirdek tabanlı satın alma modeli, genel Önizleme sırasında kullanılabilir. SKU etkin olduğunda bile bu avantajı uygulayabilirsiniz ancak temel ücreti Azure Portalı'nda seçtiğiniz andan itibaren uygulanan Not. Geriye dönük olarak herhangi bir kredi düzenlenmez.

## <a name="are-there-dual-use-rights-with-azure-hybrid-benefit-for-sql-server"></a>SQL Server için Azure hibrit avantajı ile çift kullanımlı hakları var mı?
Geçişleri sorunsuz bir şekilde çalıştığından emin olmak için ikili kullanım hakları lisans 180 günü var. Bu 180 günlük süre sonra SQL Server Lisansı yalnızca SQL veritabanı bulutta kullanılabilir ve ikili kullanım hakları şirket içi yok ve bulut.

## <a name="how-does-azure-hybrid-benefit-for-sql-server-differ-from-license-mobility"></a>Azure hibrit avantajı, SQL Server için lisans taşınabilirliği ' farkı nedir?
Bugün, lisans taşınabilirliği avantajlarından paylaşılan üçüncü taraf sunucular için lisanslarını yeniden atanmasına imkan tanır Yazılım Güvencesi içeren SQL Server müşterileri için sunuyoruz. Bu avantajı, Azure Iaas ve AWS EC2 üzerinde kullanılabilir.
SQL Server için Azure hibrit avantajı, lisans taşınabilirliği iki temel alanlarda farklıdır:
- Bu, yüksek oranda sanallaştırılmış iş yüklerini Azure'a taşımak için ekonomik avantajlarını sağlar. SQL EE müşteriler, 4 çekirdek alabilir yüksek oranda sanallaştırılmış uygulamalar için şirket içi oldukları her çekirdek için genel amaçlı SKU'SUNDA Azure'da içinde. Lisans taşınabilirliği sanallaştırılmış iş yüklerini buluta taşımak için herhangi bir özel maliyet avantajları izin vermez.
- SQL Server ile şirket içinde yüksek oranda uyumludur (SQL veritabanı yönetilen örneği) Azure PaaS hedefte sağlar

## <a name="what-are-the-specific-rights-of-the-azure-hybrid-benefit-for-sql-server"></a>SQL Server için Azure hibrit Avantajı'ndan belirli haklar nelerdir?
SQL veritabanı müşterilerinin, SQL Server için Azure hibrit avantajı ile ilişkili aşağıdaki hakkına sahip olursunuz:

|Lisans Ayak izi|Azure hibrit avantajı SQL Server almak sizin için yapar?|
|---|---|
|SQL Server Enterprise Edition çekirdek müşterilerle SA|<li>Genel amaçlı ya da iş açısından kritik SKU'SUNDA taban ücreti ödeyebilirsiniz</li><br><li>1 çekirdek şirket içi genel amaçlı SKU'SUNDA 4 çekirdek =</li><br><li>1 çekirdek şirket içi iş açısından kritik SKU'SUNDA 1 çekirdek =</li>|
|SQL Server Standard Edition çekirdek müşterilerle SA|<li>Taban fiyatı yalnızca genel amaçlı SKU'SUNDA ödeyebilirsiniz</li><br><li>1 çekirdek şirket içi genel amaçlı SKU'SUNDA 1 çekirdek =</li>|
|||

## <a name="is-the-vcore-based-model-available-to-sql-database-managed-instance"></a>Sanal çekirdek tabanlı model, SQL veritabanı yönetilen örneği için kullanılabilir mi?
[Yönetilen örnek](sql-database-managed-instance.md) yalnızca sanal çekirdek tabanlı model ile kullanılabilir. Daha fazla bilgi için Ayrıca bkz: [SQL veritabanı fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/sql-database/managed/). 

## <a name="does-the-cost-of-compute-and-storage-depend-on-the-service-tier-that-i-choose"></a>İşlem ve depolama maliyetini seçmem hizmet katmanına bağlı bağımlı mı?
İşlem maliyet, uygulama için sağlanan toplam işlem kapasitesini yansıtır. İş açısından kritik hizmet katmanında, biz otomatik olarak en az 3 her zaman açık çoğaltma ayırın. Bu ek işlem kaynakları ayrılması yansıtacak şekilde yaklaşık 2.7 x daha yüksek iş açısından kritik sanal çekirdek fiyatı değeridir. Aynı nedenden dolayı GB başına daha yüksek depolama fiyatı iş açısından kritik katmanında SSD depolama, düşük gecikme süresi ve yüksek g/ç yansıtır. Her iki durumda da bir standart depolama sınıfı kullandığımızdan aynı zamanda, yedekleme depolama maliyeti, farklı değildir.

## <a name="how-am-i-charged-for-storage---based-on-what-i-configure-upfront-or-on-what-the-database-uses"></a>Nasıl ne önceden yapılandırabilirim veya hangi veritabanı kullanan temel depolama için-ücretlendirilir miyim?
Farklı depolama türlerini farklı faturalandırılır. Veri depolama için seçtiğiniz en fazla veritabanı veya havuz boyutuna bağlı olarak sağlanan depolama alanı için ücretlendirilirsiniz. Maliyeti azaltmak veya artırmak, en fazla sürece değiştirmez. Yedekleme alanı, örneğinizin otomatik yedekleme işlemleriyle ilişkilidir. Yedekleme saklama döneminizin artırılması, örneğiniz tarafından kullanılan yedekleme alanının artmasına neden olur. Toplam sağlanan sunucu depolama alanınızın yüzde 100’üne ulaşana kadar yedekleme alanı için ek ücret ödemezsiniz. Ek yedekleme alanı kullanımı, GB cinsinden aylık olarak ücretlendirilir. Örneğin, veritabanı depolama alanınızın boyutu 100 GB’sa ek maliyet olmaksızın 100 GB yedekleme alanına sahip olursunuz. Ancak yedekleme boyutu 110 GB olursa fazlalık 10 GB için ödeme yapın.

Tek bir veritabanı yedekleme alanı için veritabanının boyutuna eksi veritabanı yedeklemeleri için ayrılan depolama alanı için günlere eşit olarak ücretlendirilir. Elastik havuz Yedekleme depolaması için veritabanı yedeklerini eksi elastik havuzun en yüksek boyut havuzdaki tüm veritabanları için ayrılan depolama alanı için günlere eşit olarak ücretlendirilir. Tüm elastik havuz ve veritabanı boyutunu artırma veya işlem hızı artış daha fazla depolama alanı gerektirir ve bu nedenle yedekleme depolama faturanız artırır.  En yüksek boyut artırdığınızda, bu yeni miktar faturalandırılan yedekleme depolama boyutundan çıkarılır.

## <a name="how-do-i-select-the-right-sku-when-converting-an-existing-database-to-the-new-service-tiers"></a>SKU'ları doğru nasıl mevcut bir veritabanı yeni hizmet katmanlarına nasıl dönüştürülürken seçin? 
DTU tabanlı modeli kullanarak mevcut SQL veritabanı uygulamaları için genel amaçlı hizmet katmanı standart katmanı ile karşılaştırılabilir. İş açısından kritik Hizmet katmanını Premium katmanı ile karşılaştırılabilir. Her iki durumda da DTU tabanlı modelde uygulamanızın kullandığı her 100 DTU için en az 1 sanal çekirdek ayırmanız gerekir.

## <a name="do-the-new-vcore-based-service-tiers-offer-the-performance-levels-compatible-with-all-existing-service-level-objectives-slos"></a>Yeni sanal çekirdek tabanlı hizmet katmanları, mevcut tüm hizmet düzeyi hedefleri ile (Slo) uyumlu performans düzeyleri sunuyor musunuz?
Yeni sanal çekirdek tabanlı hizmet katmanları için tüm elastik havuzları ve veritabanlarını veya daha fazla 100 dtu'ları kullanarak benzer performans seçenekleri sunar.  Sub 100 DTU iş yüklerine uyum sağlamak için zaman içinde daha fazla Slo'lar eklemeye devam eder.

## <a name="are-there-any-database-feature-differences-between-the-existing-dtu-based-and-new-vcore-based-service-tiers"></a>Herhangi bir veritabanı mevcut DTU tabanlı ve yeni sanal çekirdek tabanlı hizmet katmanları arasındaki özellik farkları vardır? 
Yeni hizmet katmanları, geçerli DTU tabanlı teklifleri ile sunulan özellikler kümesi desteği. Ek özellikler, bir dizi ek dinamik yönetim görünümlerini (Dmv'ler) ve ek kaynak yapılandırma seçenekleri içerir. 

## <a name="if-my-database-is-cpu-bound-and-does-not-use-much-storage-can-i-increase-the-compute-without-paying-for-extra-storage"></a>Veritabanım CPU bağlantılı ve kadar depolama alanı kullanmaz, işlem ek depolama için ödeme yapmadan artırabilirim?
Evet, bağımsız olarak uygulamanız gereken ve depolama değişmeden Koru işlem düzeyini seçebilirsiniz. Depolama 32 GB olarak düşük olarak ayarlayabilirsiniz. 

## <a name="will-the-new-vcore-based-tiers-support-point-in-time-restore-pitr-for-35-days-as-today"></a>Yeni sanal çekirdek tabanlı katmanlarını bugün olarak 35 gün boyunca (PITR) noktaya geri yükleme noktası desteklenecek? 
Yedekleme bekletme 7 ila 35 gün arasında PITR için yapılandırabilirsiniz. Yedekleme depolama, en fazla veri boyutuna eşit bir depolama alanı miktarı aşarsa, gerçek depolama tüketimi ayrı olarak göre ücretlendirilir. Önizleme aşamasında olan, PITR saklama süresi 7 gün için varsayılan olarak ayarlanır. Çoğu durumda, en yüksek Boyut 7 güne kadar yedek depolamak için yeterlidir.

## <a name="why-do-you-allow-selection-of-the-hardware-generation-for-compute"></a>İşlem donanımı oluşturma seçilmesine neden sağlar?
Hedefimiz uygulama ihtiyaçlarını yakından eşleşen bir performans yapılandırması seçebilmeniz maksimum esneklik etkinleştirmektir. Donanım 4. nesil sanal çekirdek başına önemli ölçüde daha fazla bellek sunar. Ancak, 5. nesil donanım çok daha yüksek bilgi işlem kaynaklarını ölçeklendirme olanak tanır. Daha fazla bilgi için [vCore satın alma modeli](sql-database-service-tiers-vcore.md)

## <a name="do-i-need-to-take-my-application-offline-to-convert-from-a-dtu-based-database-to-a-vcore-based-service-tier"></a>DTU tabanlı bir veritabanından sanal çekirdek tabanlı hizmet katmanı için dönüştürmek için uygulamamı çevrimdışı duruma getirmem gerekiyor mu? 
Yeni hizmet katmanları, Standard hizmet katmanından Premium hizmet katmanına ve tam tersi yönde veritabanı yükseltmesi için kullanılan mevcut işleme benzeyen basit bir çevrimiçi dönüştürme yöntemi sunar. Bu dönüştürme, Azure portalı, PowerShell, Azure CLI, T-SQL veya REST API kullanılarak başlatılabilir. Bkz: [tek veritabanlarını yönetmek](sql-database-single-database-scale.md) ve [elastik havuzları yönetme](sql-database-elastic-pool.md).

## <a name="can-i-convert-a-database-from-a-vcore-based-service-tier-to-a-dtu-based-one"></a>Ben bir veritabanı sanal çekirdek tabanlı hizmet katmanından bir DTU tabanlı bir dala dönüştürebilir miyim? 
Evet, Azure portalı, PowerShell, Azure CLI, T-SQL veya REST API'yi kullanarak tüm desteklenen performans hedefi veritabanınızı kolayca dönüştürebilirsiniz. Bkz: [tek veritabanlarını yönetmek](sql-database-single-database-scale.md) ve [elastik havuzları yönetme](sql-database-elastic-pool.md).

## <a name="can-i-upgrade-or-downgrade-between-the-general-purpose-and-business-critical-service-tiers"></a>Ben yükseltebilir veya genel amaçlı ve iş açısından kritik hizmet katmanları arasında düşürebilirsiniz? 
Evet, bazı kısıtlamalar ile. Hedef SKU'su, en fazla veritabanı veya elastik havuz boyutu mevcut dağıtımınız için yapılandırılmış karşılaması gerekir. Kullanıyorsanız [SQL Server için Azure hibrit kullanım teklifi](../virtual-machines/windows/hybrid-use-benefit-licensing.md), iş açısından kritik SKU'SUNDA yalnızca Enterprise Edition lisansları olan müşteriler için kullanılabilir. İçin Azure hibrit avantajı için SQL Server Enterprise Edition lisansları ile kullanarak genel amaçlı şirket içinden geçirilmiş olan müşteriler, iş kritik yükseltebilirsiniz. Ayrıntılar için bkz. [SQL Server için Azure hibrit kullanım teklifi, belirli haklar nelerdir](../virtual-machines/windows/hybrid-use-benefit-licensing.md)?

Bu dönüştürme, kapalı kalma süresi sonuçlanmaz ve Azure portalı, PowerShell, Azure CLI, T-SQL veya REST API kullanılarak başlatılabilir. Bkz: [tek veritabanlarını yönetmek](sql-database-single-database-scale.md) ve [elastik havuzları yönetme](sql-database-elastic-pool.md).

## <a name="i-am-using-a-premium-rs-database-that-will-not-be-generally-available---can-i-upgrade-it-to-a-new-tier-and-achieve-a-similar-priceperformance-benefit"></a>Genel olarak kullanılabilir olmayacak bir Premium RS veritabanı kullanıyorum - yeni bir katmana yükseltmeniz ve miyim benzer bir fiyat/performans avantajı elde?
VCore modeli sağlanan işlem ve depolama miktarını bağımsız denetime izin verdiğinden, Premium RS veritabanları için çekici bir hedef yapmadan elde edilen maliyetleri, daha etkili bir şekilde yönetebilirsiniz. Ayrıca, [SQL Server için Azure hibrit kullanım teklifi](../virtual-machines/windows/hybrid-use-benefit-licensing.md) sanal çekirdek tabanlı model kullanılırken önemli bir indirim sağlar. 

## <a name="how-often-can-i-adjust-the-resources-per-pool"></a>Havuz başına kaynakları ne sıklıkta değiştirebilir miyim?
Sıklıkta istediğiniz. Bkz: [elastik havuzları yönetme](sql-database-elastic-pool.md).

## <a name="how-long-does-it-take-to-change-the-service-tier-or-performance-level-of-a-single-database-or-move-a-database-in-and-out-of-an-elastic-pool"></a>Ne kadar tek bir veritabanının Hizmet katmanını veya performans düzeyini değiştirmek veya bir veritabanını bir elastik havuzun içine ve dışına taşımak için sürer?
Arka plan işlemi olarak platformunda kopyalanacak veritabanı havuzunun içine veya dışına taşımak ve bir veritabanının Hizmet katmanını değiştirmeniz gerekir. Hizmet katmanını değiştirmeniz birkaç dakika veya veritabanlarını boyutuna bağlı olarak birkaç saat sürebilir. Her iki durumda da, veritabanlarını taşıma işlemi sırasında çevrimiçi ve kullanılabilir kalır. Tek veritabanları değiştirme hakkında daha fazla bilgi için bkz [bir veritabanının Hizmet katmanını değiştirebilirsiniz](sql-database-service-tiers-dtu.md). 

## <a name="when-should-i-use-a-single-database-vs-elastic-databases"></a>Tek bir veritabanı ve elastik veritabanları ne zaman kullanmalıyım?
Genel olarak, elastik havuzlar için tipik bir tasarlanmıştır [hizmet olarak yazılım (SaaS) uygulama düzeni](sql-database-design-patterns-multi-tenancy-saas-applications.md), müşteri veya Kiracı başına bir veritabanı olduğunda. Veritabanlarına göre değişen maksimum gereksinimleri karşılamak için ayrı ayrı veritabanları satın alıp bu veritabanlarını tekrar tekrar sağlamak, ekonomik açıdan çok verimli bir yöntem değildir. Havuzlar, havuzun toplu performansını yönetin ve veritabanlarını otomatik olarak yukarı ve aşağı ölçeklendirin. Azure'nın akıllı altyapısı kullanım deseniyle gerektirdiğini veritabanları için bir havuz kullanılmasını önerir. Ayrıntılar için bkz [elastik havuzu kılavuzluğu](sql-database-elastic-pool.md).

## <a name="how-does-the-usage-of-sql-database-using-the-dtu-based-purchasing-model-show-up-on-my-bill"></a>DTU tabanlı satın alma modeli kullanarak SQL veritabanı kullanımı faturamda nasıl göstermez?
SQL veritabanı faturalandırılır öngörülebilir bir saatlik fiyat temel alarak [satın alma modeli](sql-database-service-tiers-dtu.md). Gerçek kullanım hesaplanır ve saatlik olarak bir saat kesirlerini faturanızı gösterebilir şekilde eşit olarak. Örneğin, bir veritabanı ayda 12 saat çalışıyorsa faturanızda 0,5 gün kullanımını gösterir. 

## <a name="what-if-a-single-database-is-active-for-less-than-an-hour-or-uses-a-higher-service-tier-for-less-than-an-hour"></a>Ne tek bir veritabanının bir saatten az için etkin olan veya daha yüksek bir hizmet katmanına bir saatten az için kullanır?
Faturalandırılırsınız her bir veritabanının en yüksek hizmet katmanı kullanarak mevcut saat + kullanıma veya veritabanının bir saatten az için etkin olup bağımsız olarak bu saat sırasında uygulanan performans düzeyi. Örneğin, tek bir veritabanı oluşturup beş dakika sonra silerseniz faturanıza bir veritabanı saati ücreti yansıtır. 

Örnekler:

* Temel bir veritabanı oluşturur ve hemen ardından standart S1'e yükseltirseniz, ilk saat için standart S1 oranı üzerinden ücretlendirilir.
* Bir veritabanı Basic'ten Premium 10: 00'da yükseltirseniz ve yükseltme 1: 35'da tamamlanırsa sonraki gün, 1: 00'da itibaren Premium ücreti ödersiniz 
* 11: 00'da bir veritabanını temel sürümüne düşürürseniz, ve 2: 15'da tamamlandıktan sonra veritabanı Premium 3:00 p.M'de kadar sonra da temel fiyatları üzerinden ücretlendirilir üzerinden ücretlendirilir.

## <a name="how-does-elastic-pool-usage-using-the-dtu-based-purchasing-model-show-up-on-my-bill"></a>DTU tabanlı satın alma modeli kullanarak esnek havuz kullanımı faturamda nasıl göstermez?
Elastik havuz ücretleri show yukarı faturanızda esnek Dtu'lar (Edtu) veya çekirdek artı gösterilen halinde depolama olarak [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/sql-database/). Elastik havuzlar için veritabanı başına ücret yoktur. Bir havuz en yüksek eDTU veya sanal çekirdek, kullanım veya havuza bir saatten az için etkin olup bağımsız olarak mevcut olduğu her saat için faturalandırılırsınız. 

DTU tabanlı satın alma modeli örnekleri:

* Standart elastik havuz 200 Edtu'dan ile 11: 18'da oluşturursanız, beş veritabanı havuzuna ekleme için 200 edtu'ları için tüm saat, saat 11'de başlayarak ödersiniz Günün geri kalanında.
* Veritabanı 1 gün 2, 5: 05'da, 50 edtu'ları kullanmaya başladığında ve gün düzenli tutar. Veritabanları 2-5, 0 ile 80 edtu'ları arasında dalgalanma. Gün, gün boyunca çeşitli Edtu kullanmaz beş veritabanları ekleyin. 2. gün 200 eDTU faturalandırılır tam bir gündür. 
* Sabah üzerinde 3, 5 başka bir 15 veritabanları ekleyin. Burada 8: 05'te 400 200'den Havuz edtu'ları artırmaya karar noktası için gün boyunca veritabanı kullanımını artırır. 200 eDTU düzeyinde ücretleri 8 pm kadar etkili olan ve kalan dört saat için 400 Edtu'ya kadar artırır. 

## <a name="how-are-elastic-pool-billed-for-the-dtu-based-purchasing-model"></a>Elastik havuz DTU tabanlı için fatura modeli nasıl satın?
Elastik havuzlar, aşağıdaki özelliklere faturalandırılır:

* Havuzda veritabanı yoksa bir esnek havuz oluşturulduktan sonra faturalandırılır.
* Elastik havuz, saatlik olarak faturalandırılır. Performans düzeyleri tek veritabanları için aynı Ölçüm sıklığı budur.
* Ardından bir elastik havuz boyutlandırılırsa, yeniden boyutlandırma işlemi tamamlanana kadar havuza yeni kaynakların miktarını göre faturalandırılmaz. Bu, tek veritabanı performans düzeyini değiştirme olarak aynı deseni izler.
* Elastik havuz fiyatını, havuzu kaynaklardaki temel alır. Elastik havuz fiyatını, sayısını ve içerdiği esnek veritabanlarının kullanım bağımsızdır.

Ayrıntılar için bkz [SQL veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/), [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md), ve [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md).

## <a name="how-does-the-vcore-based-usage-show-up-in-my-bill"></a>Nasıl sanal çekirdek tabanlı kullanımı faturama içinde gösterilir? 
Sanal çekirdek tabanlı modelde hizmet Hizmet katmanını temel alan öngörülebilir ve saatlik bir ücret faturalandırılır sağlanan işlem, sanal çekirdek depolama GB cinsinden aylık sağlanan ve yedekleme depolama miktarı. Yedeklemeler için depolama toplam veritabanı boyutunu (diğer bir deyişle, veritabanı boyutunun %100) aşıyorsa, ek ücreti yoktur. Sanal çekirdek saatleri, yapılandırılan veritabanı depolaması, kullanılan GÇ ve yedekleme depolama açıkça, kullandığınız kaynakların ayrıntılarını görmeyi kolaylaştıracak kolayca. Kadar yedekleme alanı en fazla veritabanı boyutunun % 100'dür dahil, GB/ay aylık olarak kullanılan hangi içinde faturalandırılırsınız ötesinde.

Örneğin:
- SQL veritabanı ayda 12 saat çalışıyorsa fatura sanal çekirdeğin 12 saat boyunca kullanımını gösterir. SQL veritabanı depolama ek bir 100 GB sağladıysa faturada depolama kullanımını GB/ay birimi içinde saatlik olarak eşit dağıtılan ve bir ay içerisinde tüketilen GÇ sayısı gösterir.
- SQL veritabanı bir saatten kısa bir süre için etkin olursa, veritabanının mevcut olduğu her saat seçilmedi, en yüksek hizmet katmanı kullanılarak depolama ve bu kullanım veya veritabanı için etkin daha az daha olmasına bakılmaksızın saat sırasında uygulanan GÇ sağlanan için faturalandırılırsınız bir saat.
- Bir Yönetilen Örnek oluşturup beş dakika sonra silerseniz, bir veritabanı saati ücreti ödersiniz.
- Genel Amaçlı katmanında 8 sanal çekirdekli bir Yönetilen Örnek oluşturup bunu hemen 16 sanal çekirdekli yapılandırmaya yükseltirseniz, ilk saat için 16 sanal çekirdek fiyatı üzerinden ücretlendirilirsiniz.

> [!NOTE]
> Bir süreliğine yedekleme ve GÇ ücreti ücretsizdir.

## <a name="how-does-the-use-of-active-geo-replication-in-an-elastic-pool-show-up-on-my-bill"></a>Nasıl faturamı üzerinde bir elastik havuz gösteren etkin coğrafi çoğaltma kullanımı?
Kullanarak, tek veritabanlarının aksine [etkin coğrafi çoğaltma](sql-database-geo-replication-overview.md) esnek veritabanları sayesinde doğrudan bir faturalama etkisi yoktur.  Yalnızca her biri (birincil havuzu ve ikincil havuzu) havuzları için sağlanmış olan kaynakları için ücretlendirilirsiniz

## <a name="how-does-the-use-of-the-auditing-feature-impact-my-bill"></a>Denetim özelliğinin kullanılması faturamı nasıl etkiler?
Denetim içinde oluşturulan hiçbir ek SQL veritabanı hizmetinin maliyeti ve tüm hizmet katmanlarda kullanılabilir. Ancak, denetim günlüklerini depolamak için denetim özelliği kullandığı bir Azure depolama hesabı ve tablolar ve Kuyruklar Azure depolama ücretleri uygulanır denetim günlüğünüzün boyutuna göre.

## <a name="how-do-i-reset-the-password-for-the-server-admin"></a>Sunucu Yöneticisi parolasını sıfırlama?
İçinde [Azure portalında](https://portal.azure.com), tıklayın **SQL sunucuları**listeden sunucuyu seçin ve ardından **parola sıfırlama**.

## <a name="how-do-i-manage-databases-and-logins"></a>Veritabanlarını ve oturum açma bilgileri nasıl yönetebilirim?
Bkz: [veritabanlarını ve oturum açma bilgilerini yönetme](sql-database-manage-logins.md). 

> [!NOTE]
> Oluşturulduktan sonra sunucu yönetici hesabı adını değiştiremezsiniz.

## <a name="how-do-i-make-sure-only-authorized-ip-addresses-are-allowed-to-access-a-server"></a>Yalnızca yetkili IP adreslerini bir sunucuya erişim izni nasıl emin olabilirim?
Bkz: [nasıl yapılır: SQL veritabanı'nda Güvenlik Duvarı ayarlarını yapılandırma](sql-database-configure-firewall-settings.md).

## <a name="what-is-an-expected-replication-lag-when-geo-replicating-a-database-between-two-regions-within-the-same-azure-geography"></a>Coğrafi çoğaltma işleminde bir beklenen çoğaltma gecikmesi nedir Azure aynı coğrafyadaki iki bölge arasında bir veritabanı?
Şu anda beş saniyede bir RPO destekleyen ve çoğaltma gecikmesi'nin süresi olan değerinden coğrafi-ikincil Azure'da barındırılan zaman eşleştirilmiş bölge önerilen ve aynı hizmet katmanında.

## <a name="what-is-an-expected-replication-lag-when-geo-secondary-is-created-in-the-same-region-as-the-primary-database"></a>Birincil veritabanı ile aynı bölgede coğrafi-ikincil oluşturulduğunda bir beklenen çoğaltma gecikmesi'nin süresi nedir?
Görgül verilerini temel alarak yapıldığında değil içi bölge ve bölgeler arası çoğaltma gecikmesi çok fazla birbirinden Azure eşleştirilmiş bölge kullanılan önerilir. 

## <a name="if-there-is-a-network-failure-between-two-regions-how-does-the-retry-logic-work-when-geo-replication-is-set-up"></a>İki bölgeleri arasında bir ağ hatası varsa, coğrafi çoğaltma kurulduğunda yeniden deneme mantığı nasıl çalışır?
Bir bağlantıyı kes varsa, 10 saniyede yeniden bağlantı kurmak için tekrar deneyeceğiz.

## <a name="what-can-i-do-to-guarantee-that-a-critical-change-on-the-primary-database-is-replicated"></a>Birincil veritabanında önemli bir değişiklik çoğaltılır garanti etmek için ne yapabilirim?
Coğrafi-ikincil bir zaman uyumsuz kopya olduğunu ve biz birincil ile tam eşitleme tutmak çalışmayın. Ancak, önemli değişiklikler (örneğin, parola güncelleştirmeleri) çoğaltılmasını sağlamak için eşitlemeyi zorlama yöntemi sunuyoruz. Tüm kaydedilmiş işlemleri çoğaltılır kadar çağıran iş parçacığını engeller. çünkü zorlamalı eşitleme performansı etkiler. Ayrıntılar için bkz [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx). 

## <a name="what-tools-are-available-to-monitor-the-replication-lag-between-the-primary-database-and-geo-secondary"></a>Hangi Araçlar, birincil veritabanı arasındaki coğrafi-ikincil çoğaltma gecikmesi izlemek kullanılabilir mi?
Gerçek zamanlı çoğaltma gecikmesi'nin süresi birincil veritabanı arasındaki coğrafi-ikincil bir DMV üzerinden kullanıma sunuyoruz. Ayrıntılar için bkz [sys.dm_geo_replication_link_status](https://msdn.microsoft.com/library/mt575504.aspx).

## <a name="to-move-a-database-to-a-different-server-in-the-same-subscription"></a>Bir veritabanını aynı Abonelikteki farklı bir sunucuya taşımak için
İçinde [Azure portalında](https://portal.azure.com), tıklayın **SQL veritabanları**, listeden bir veritabanı seçin ve ardından **kopyalama**. Bkz: [bir Azure SQL veritabanını kopyalamayı](sql-database-copy.md) daha fazla ayrıntı için.

## <a name="to-move-a-database-between-subscriptions"></a>Bir veritabanını abonelikler arasında taşıma
İçinde [Azure portalında](https://portal.azure.com), tıklayın **SQL sunucuları** ve ardından listeden veritabanınızı barındıran sunucuyu belirleyin. Tıklayın **taşıma**ve ardından taşınacak kaynaklar ve taşımak için bir abonelik seçin.

