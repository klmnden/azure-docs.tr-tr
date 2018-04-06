---
title: Azure SQL veritabanı ile ilgili SSS | Microsoft Docs
description: Ortak sorular müşteriler yanıtlarını bulutta bir hizmet olarak bulut veritabanları ve Azure SQL Database, Microsoft'un ilişkisel veritabanı yönetim sistemine (RDBMS) ve veritabanı hakkında isteyin.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: reference
ms.topic: article
ms.date: 04/04/2018
ms.author: carlrab
ms.openlocfilehash: f98337044bdad788d2a4c9eac0c67a2031810430
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="sql-database-faq"></a>SQL Veritabanı SSS

## <a name="what-is-the-current-version-of-sql-database"></a>SQL veritabanı'nın geçerli sürümü nedir?
Geçerli SQL veritabanı V12 sürümüdür. Sürüm V11 devre dışı bırakılmış.

## <a name="what-is-the-sla-for-sql-database"></a>SQL veritabanı için SLA nedir?
Süre en az % 99,99 garanti ediyoruz, bağlantı, Microsoft Azure SQL veritabanı ve bizim Internet ağ geçidi arasında bağımsız olarak, hizmet katmanı gerekir. Daha fazla bilgi için bkz: [SLA](http://azure.microsoft.com/support/legal/sla/).

## <a name="whatis-the-new-vcore-based-purchasing-model-for-azure-sql-database"></a>Azure SQL veritabanı için yeni vCore tabanlı satın alma modeli Whatis?

Yeni satın alma modeli varolan DTU tabanlı yanı sıra modelidir. VCore tabanlı modeli müşteriler esneklik, Denetim, saydamlık ve şirket içi iş yükü gereksinimlerini buluta çevirmek için basit bir yol sağlamak için tasarlanmıştır. Bilgi işlem ve depolama birimine iş yükü gereksinimlerine göre müşterileri de sağlar. Tek veritabanı ve esnek havuz seçenekleri vCore modelini kullanarak da ile yüzde 30 tasarrufları uygun için yukarı [SQL Server için Azure karma kullanımı avantajı](../virtual-machines/windows/hybrid-use-benefit-licensing.md). Bkz: [hizmet katmanları](sql-database-service-tiers.md) DTU tabanlı satın alma modeli ve vCore tabanlı satın alma modeli hakkında ayrıntılar için.

## <a name="what-is-a-vcore"></a>Sanal çekirdek nedir? 
Sanal bir çekirdek donanım nesli arasında seçmek için bir seçenek ile birlikte sunulan mantıksal CPU temsil eder. 4 mantıksal CPU üzerinde Intel E5-2673 v3 dayalı gen (Haswell) 2.4 GHz işlemci ve Gen 5 mantıksal CPU üzerinde Intel E5-2673 v4 dayalı (Broadwell) 2.3 GHz işlemci.

## <a name="is-moving-to-the-vcore-based-model-required"></a>Gerekli vCore tabanlı modeli taşıma?
Hayır, giriş vCore tabanlı modelinin tek veritabanı dağıtım seçenekleri ve esnek havuz müşteri seçeneğini ve esnekliğini müşterilerimize taahhütler yansıtır. Müşteriler DTU tabanlı modeli kullanmaya devam etmek istiyorsanız, bu duyuru ve deneyimlerini ile herhangi bir eylemde bulunmanız gerekmez ve faturalama değişmeden kalır. 

Çoğu durumda, önceden yapılandırılmış bir paket kaynakların Basitlik uygulamalar yararlı olabilir. Bu nedenle, sunar ve bu DTU tabanlı seçenekler müşterilerimiz için destek devam ediyoruz. Bunları kullanmakta olduğunuz ve iş gereksinimlerinizi karşılayan, bunu yapmak devam etmelidir.

DTU ve vCore tabanlı modelleri mevcut yan yana devam eder. Biz, daha fazla saydamlık veritabanı kaynaklarını ve işlem ve depolama ayrı olarak ölçeklendirebilmeniz çözmek için müşteri isteklerine yanıt vCore tabanlı modelinde başlatma. VCore tabanlı modeli ayrıca ek tasarruf Azure karma avantajı ile etkin Yazılım Güvencesi sahip müşteriler için SQL Server sağlar.

## <a name="how-should-i-choose-between-the-dtu-based-purchasing-model-vs-the-vcore-based-purchasing-model"></a>VCore tabanlı satın alma modeli nasıl satın alma modeli DTU tabanlı vs arasında seçmem gerekir? 
Veritabanı işlem birimi (DTU), CPU karışık bir ölçüyü temel alır, bellek, okur ve yazar. DTU tabanlı performans düzeyleri sürücü farklı düzeylerde uygulama performansı için önceden yapılandırılmış paketleri kaynakların temsil eder. Temel alınan kaynaklar hakkında endişelenmeniz ve her ay sabit bir tutar ödeme sırasında önceden yapılandırılmış bir paketin Basitlik tercih etmek istiyor musunuz müşteriler DTU tabanlı modeli gereksinimlerine için daha uygun bulabilirsiniz. Ancak, temel alınan kaynaklara konusunda daha fazla bilgi gerekiyor ya da bunları bağımsız olarak en iyi performans elde etmek için ölçeklendirmek gereken müşteriler için vCore tabanlı modeli en iyi seçenek olacaktır.  Ayrıca, bir müşteri SQL Server için bir etkin Yazılım Güvencesi (SA) varsa, bunlar kendi varolan yatırımdan yararlanmanızı ve % 30 ile Kaydet [SQL Server için Azure karma kullanımı avantajı](../virtual-machines/windows/hybrid-use-benefit-licensing.md).  Seçenekler her satın alma modeli içinde tam olarak yönetilen bir hizmet otomatik yedeklemeler, yazılım güncelleştirmelerini ve düzeltme ekleri gibi yararları sağlar. 

## <a name="what-is-the-azure-hybrid-benefit-for-sql-server"></a>SQL Server için Azure karma avantajı nedir? 
[SQL Server için Azure karma kullanımı avantajı](../virtual-machines/windows/hybrid-use-benefit-licensing.md) geçerli lisans yatırımlarınızı değerinden en üst düzeye çıkarmak ve kendi geçiş buluta artırmanıza yardımcı olur. Azure karma yararlanmak için SQL Server, SQL Server lisanslarınızı ödeme SQL veritabanındaki bir azaltılmış oranı ("temel hızı") için Yazılım Güvencesi ile kullanmanıza olanak sağlayan bir Azure tabanlı avantajdır. SQL Server için Azure karma avantajı, SQL veritabanı tek veritabanları ve esnek havuzlar için vCore tabanlı satın alma modeli genel önizlemesini mevcuttur. SKU etkin olsa bile bu avantajı uygulamak edebilirsiniz, ancak Azure portalında seçin zamandan taban fiyat uygulanır unutmayın. Kredi firmalarda geriye dönük verilir.

## <a name="are-there-dual-use-rights-with-azure-hybrid-benefit-for-sql-server"></a>SQL Server için Azure karma avantajı ile çift kullanım hakları vardır?
180 gün geçişler sorunsuz bir şekilde çalıştığından emin olmak için çift kullanım haklarının lisans var. Bu 180 günlük süre sonra SQL Server Lisans yalnızca SQL veritabanında bulutta kullanılabilir ve çift kullanım hakları şirket içi yok ve bulutta.


## <a name="how-does-azure-hybrid-benefit-for-sql-server-differ-from-license-mobility"></a>SQL Server için Azure karma avantajı lisans taşınabilirliği nasıl farkı nedir?
Bugün, Yazılım Güvencesi Lisansı olan SQL Server müşterileri paylaşılan üçüncü taraf sunucularına lisansları kullanımlarını yeniden atama sağlayan mobility avantajları sunuyoruz. Bu avantajı Azure Iaas ve AWS EC2 kullanılabilir.
SQL Server için Azure karma avantajı lisans taşınabilirliği iki anahtar alanlarda farklıdır:
- Bu, yüksek oranda sanallaştırılmış iş yüklerini Azure'a taşımak için ekonomik avantaj sağlar. SQL EE müşteriler, 4 çekirdek alabilirsiniz şirket içi yüksek oranda sanallaştırılmış uygulamaları oldukları her çekirdek için genel amaçlı SKU azure'da. Lisans taşınabilirliği sanallaştırılmış iş yükleri buluta taşımak için hiçbir özel maliyet avantajlarını izin vermiyor.
- SQL Server ile şirket içi – SQL veritabanı yönetilen örneği çok uyumlu Azure üzerinde bir PaaS hedef sağlar.

## <a name="what-are-the-specific-rights-of-the-azure-hybrid-benefit-for-sql-server"></a>SQL Server için Azure karma avantajı belirli haklar nelerdir?
SQL veritabanı müşteriler, SQL Server için Azure karma avantajı ile ilişkili aşağıdaki haklarına sahip olur:

|Lisans ayak izini|Azure karma avantajı SQL Server alma sizin için ne yaptığını?|
|---|---|
|SQL Server Enterprise Edition çekirdek SA müşterilerle|<li>Genel amaçlı ya da iş kritik SKU temel oranı ödeme</li><br><li>1 çekirdek şirket içi genel amaçlı SKU 4 çekirdeğini =</li><br><li>1 çekirdek şirket içi iş kritik SKU 1 çekirdek =</li>|
|SQL Server Standard Edition çekirdek SA müşterilerle|<li>Temel genel amaçlı SKU üzerinde yalnızca ödeme oranı</li><br><li>1 çekirdek şirket içi genel amaçlı SKU 1 çekirdek =</li>|
|||

## <a name="is-the-vcore-based-model-available-to-sql-database-managed-instance"></a>VCore tabanlı model SQL veritabanı örneğine yönetilen var mı?
[Örnek yönetilen](sql-database-managed-instance.md) yalnızca vCore tabanlı modeliyle kullanılabilir. Daha fazla bilgi için Ayrıca bkz. [SQL veritabanı fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/sql-database/managed/). 

## <a name="does-the-cost-of-compute-and-storage-depend-on-the-service-tier-that-i-choose"></a>İşlem ve depolama maliyetini seçmem hizmet katmanında bağlı?
İşlem maliyet uygulama için sağlanan toplam işlem kapasitesini yansıtır. İş kritik hizmet katmanında biz otomatik olarak en az 3 her zaman açık çoğaltmaları ayırır. Bu ek ayırma işlem kaynakları yansıtacak şekilde vCore fiyat yaklaşık 2.7 x kritik iş daha yüksek olur. Aynı nedenden dolayı yüksek g/ç ve düşük gecikme süresi SSD depolama alanının GB daha yüksek depolama fiyatı iş kritik katmanındaki yansıtır. Her iki durumda da biz standart depolama sınıfının kullandığından aynı anda yedekleme depolama maliyeti, farklı değildir.

## <a name="how-am-i-charged-for-storage---based-on-what-i-configure-upfront-or-on-what-the-database-uses"></a>Depolama - ne ı önceden yapılandırmak veya ne veritabanını kullanan temel için nasıl tahsil?
Farklı depolama türlerini farklı faturalandırılır. Veri depolama için olması için ücretlendirilirsiniz sağlanan depolama en fazla veritabanı veya seçtiğiniz havuzu boyutuna. Azaltın veya bu maksimum artırın sürece maliyeti değiştirmez. Yedekleme alanı, örneğinizin otomatik yedekleme işlemleriyle ilişkilidir. Yedekleme saklama döneminizin artırılması, örneğiniz tarafından kullanılan yedekleme alanının artmasına neden olur. Toplam sağlanan sunucu depolama alanınızın yüzde 100’üne ulaşana kadar yedekleme alanı için ek ücret ödemezsiniz. Ek yedekleme depolama tüketiminin GB aylık ücret kesilir. Örneğin, veritabanı depolama alanınızın boyutu 100 GB’sa ek maliyet olmaksızın 100 GB yedekleme alanına sahip olursunuz. Ancak yedekleme 110 GB ise, ek 10 GB için ödeme yaparsınız.

Tek bir veritabanı yedekleme depolaması için veritabanının boyutunu eksi veritabanı yedeklemeleri için ayrılan depolama alanı için eşit olarak bölünmüş temelinde ücretlendirilirsiniz. Bir esnek havuz Yedekleme depolaması için eşit olarak bölünmüş olarak veritabanı yedeklemeleri esnek havuz maksimum veri boyutu eksi havuzdaki tüm veritabanları için ayrılan depolama alanı için ücretlendirilirsiniz. Tüm veritabanı boyutu veya esnek havuz artış ya da işlem hızı artış daha fazla depolama alanı gerektirir ve bu nedenle Yedekleme depolaması faturanızda artırır.  Maksimum veri boyutu artırdığınızda, bu yeni tutarı faturalanan yedekleme depolama boyutu çıkarılır.

## <a name="how-do-i-select-the-right-sku-when-converting-an-existing-database-to-the-new-service-tiers"></a>Sağ SKU nasıl varolan bir veritabanını yeni hizmet katmanları dönüştürülürken seçilsin mi? 
DTU tabanlı modelini kullanarak var olan SQL veritabanı uygulamaları için genel amaçlı hizmet katmanı standart katmanı ile karşılaştırılabilir. İş kritik hizmet katmanı Premium katmanı ile karşılaştırılabilir. Her iki durumda da, uygulamanızın DTU tabanlı modelinde kullandığı her 100 DTU için en az 1 vCore ayırmanız gerekir.

## <a name="do-the-new-vcore-based-service-tiers-offer-the-performance-levels-compatible-with-all-existing-service-level-objectives-slos"></a>Yeni vCore tabanlı hizmet katmanları, varolan tüm hizmet düzeyi hedefleri ile (Slo'lar) uyumlu performans düzeyleri sunar?
Yeni vCore tabanlı hizmet katmanları, tüm esnek havuzlar ve 100 Dtu'lar ya da daha fazla kullanan veritabanları için karşılaştırılabilir performans seçenekleri sunar.  Sub 100 DTU iş yüklerini barındırmak için zaman içinde daha fazla Slo'lar eklemeye devam eder.

## <a name="are-there-any-database-feature-differences-between-the-existing-dtu-based-and-new-vcore-based-service-tiers"></a>Özellik farklılıkları varolan DTU tabanlı ve yeni vCore tabanlı hizmet katmanları arasında herhangi bir veritabanı var mı? 
Yeni hizmet katmanları geçerli DTU tabanlı teklifleri ile kullanılabilen özelliklerin bir alt kümesi destekler. Ek Özellikleri ek dinamik yönetim görünümlerini (Dmv'leri) ve ek kaynak yapılandırma seçenekleri kümesi içerir. 

## <a name="if-my-database-is-cpu-bound-and-does-not-use-much-storage-can-i-increase-the-compute-without-paying-for-extra-storage"></a>Veritabanım CPU bağımlı ise ve kadar depolama alanı kullanmaz, ek depolama alanı için ödeme olmadan ı işlem artırabilirsiniz?
Evet, bağımsız olarak uygulamanız gerekir ve değişmeden depolama tutmak işlem düzeyini seçebilirsiniz. Depolama 32 GB düşük olarak ayarlanabilir. 

## <a name="will-the-new-vcore-based-tiers-support-point-in-time-restore-pitr-for-35-days-as-today"></a>Yeni Katmanlar vCore tabanlı geri yükleme (PITR) Bugün olarak 35 gün noktası destekleyecek mi? 
Yedekleme bekletme 7 ile 35 gün arasında PITR için yapılandırabilirsiniz. Yedekleme depolama maksimum veri boyutuna eşit depolama miktarını aşarsa, ayrı ayrı depolamanın tüketime dayanarak ücretlendirilir. Önizleme'de, varsayılan olarak PITR Bekletme dönemi 7 gün olarak ayarlanır. Çoğu durumda maksimum veri boyutu 7 gün yedeklemelerini depolamak için yeterli olur.

## <a name="why-do-you-allow-selection-of-the-hardware-generation-for-compute"></a>İşlem için donanım nesil seçimi neden verilsin mi?
Amacımız uygulama ihtiyaçlarını yakından eşleşen bir performans yapılandırması seçebilmeleri maksimum esneklik sağlamaktır. Yukarıdaki tabloda Gen4 ve Gen5 arasındaki farklar gösterilmektedir. Özellikle, Gen4 donanım vCore başına önemli ölçüde daha fazla bellek sunar. Ancak, Gen5 donanım işlem çok daha yüksek ölçeklendirmenizi sağlar. Böylece, uygulamanız için en iyi fiyat/performans oranı elde edebilirsiniz Bu farklılıklar saydam olmak istiyoruz.

## <a name="do-i-need-to-take-my-application-offline-to-convert-from-a-dtu-based-database-to-a-vcore-based-service-tier"></a>DTU tabanlı bir veritabanından bir vCore tabanlı hizmet katmanına dönüştürmek için Uygulamam çevrimdışı olması gerekiyor mu? 
Yeni hizmet katmanları Premium Hizmet katmanını ve tersi yönde veritabanları standart yükseltme varolan işlemi için benzer bir basit çevrimiçi dönüştürme yöntem sunar. Bu dönüştürme, Portal, ARM, PowerShell, Azure CLI veya T-SQL kullanarak başlatılabilir. Bkz: [tek veritabanlarını yönetme](sql-database-single-database-resources.md) ve [yönetmek esnek havuzlar](sql-database-elastic-pool.md).

## <a name="can-i-convert-a-database-from-a-vcore-based-service-tier-to-a-dtu-based-one"></a>I bir veritabanı vCore tabanlı hizmet katmanından bir DTU tabanlı bir dönüştürebilir miyim? 
Evet, tüm desteklenen performans hedefi portalı veya program aracılığıyla Portal, ARM, PowerShell, Azure CLI veya T-SQL kullanarak veritabanınızı kolayca dönüştürebilirsiniz. Bkz: [tek veritabanlarını yönetme](sql-database-single-database-resources.md) ve [yönetmek esnek havuzlar](sql-database-elastic-pool.md).

## <a name="can-i-upgrade-or-downgrade-between-the-general-purpose-and-business-critical-service-tiers"></a>I yükseltebilir veya genel amaçlı ve iş kritik hizmet katmanları arasında düşürmek? 
Evet, bazı kısıtlamalarla. Hedefinizi SKU en fazla veritabanı ya da varolan dağıtımınız için yapılandırılmış esnek havuz boyutu karşılaması gerekir. Kullanıyorsanız [SQL Server için Azure karma kullanımı avantajı](../virtual-machines/windows/hybrid-use-benefit-licensing.md), iş kritik SKU yalnızca Enterprise Edition lisansına sahip müşteriler için kullanılabilir. Yalnızca şirket içi Azure karma avantajı için SQL Server Enterprise Edition ile lisanslarla genel amaçlı geçirilen müşteriler için kritik iş yükseltebilirsiniz. Ayrıntılar için bkz: [SQL Server için Azure karma kullanımı avantajı belirli haklar nelerdir](../virtual-machines/windows/hybrid-use-benefit-licensing.md)?

Bu dönüştürme kapalı kalma sürelerine neden değil ve Portal, ARM, PowerShell, Azure CLI veya T-SQL kullanarak başlatılabilir. Bkz: [tek veritabanlarını yönetme](sql-database-single-database-resources.md) ve [yönetmek esnek havuzlar](sql-database-elastic-pool.md).

## <a name="i-am-using-a-premium-rs-database-that-will-not-be-generally-available---can-i-upgrade-it-to-a-new-tier-and-achieve-a-similar-priceperformance-benefit"></a>Genel olarak kullanılabilir olmaz Premium RS veritabanı kullanıyorum - miyim yeni katmana yükseltin ve benzer bir fiyat/performans avantajı elde?
VCore modeli sağlanan işlem ve depolama miktarını bağımsız denetime izin verdiğinden, Premium RS veritabanları için çekici bir hedef kolaylaştırarak sonuçta elde edilen maliyetleri daha etkili bir şekilde yönetebilirsiniz. Ayrıca, [SQL Server için Azure karma kullanımı avantajı](../virtual-machines/windows/hybrid-use-benefit-licensing.md) vCore tabanlı modeli kullanıldığında önemli indirim sağlar. 

## <a name="how-often-can-i-adjust-the-resources-per-pool"></a>Havuzu başına kaynakları ne sıklıkta göre ayarlayabilirsiniz?
Sıklıkta istiyor. Bkz: [yönetmek esnek havuzlar](sql-database-elastic-pool.md).

## <a name="how-long-does-it-take-to-change-the-service-tier-or-performance-level-of-a-single-database-or-move-a-database-in-and-out-of-an-elastic-pool"></a>Ne kadar tek bir veritabanının Hizmet katmanını veya performans düzeyini değiştirmek veya bir veritabanını bir esnek havuz ve bu moddan taşımak için sürer?
Bir veritabanının Hizmet katmanını değiştirmeyi ve bir havuz ve bu moddan taşıma arka plan işlemi olarak platformunda kopyalanacak veritabanı gerektirir. Hizmet katmanını değiştirmeyi birkaç dakika arasında veritabanlarını boyutuna bağlı olarak birkaç saat sürebilir. Her iki durumda da veritabanları taşıma işlemi sırasında çevrimiçi ve kullanılabilir olmaya devam eder. Tek veritabanları değiştirme hakkında daha fazla bilgi için bkz [bir veritabanının Hizmet katmanını değiştirme](sql-database-service-tiers.md). 

## <a name="when-should-i-use-a-single-database-vs-elastic-databases"></a>Tek bir veritabanının esnek veritabanları ve ne zaman kullanmalıyım?
Genel olarak, esnek havuzlar için tipik bir tasarlanmış [hizmet olarak yazılım (SaaS) uygulama düzeni](sql-database-design-patterns-multi-tenancy-saas-applications.md), müşteri veya Kiracı başına tek bir veritabanı olduğu. Veritabanlarına göre değişen maksimum gereksinimleri karşılamak için ayrı ayrı veritabanları satın alıp bu veritabanlarını tekrar tekrar sağlamak, ekonomik açıdan çok verimli bir yöntem değildir. Havuzlarıyla, havuzun toplu performansı yönetme ve veritabanlarını yukarı ve aşağı otomatik olarak ölçeklendirin. Kullanım deseniyle garanti eder, azure'nın akıllı altyapısı veritabanları için bir havuz önerir. Ayrıntılar için bkz [esnek havuz Kılavuzu](sql-database-elastic-pool.md).

## <a name="how-does-the-usage-of-sql-database-using-the-dtu-based-purchasing-model-show-up-on-my-bill"></a>Nasıl DTU tabanlı satın alma modeli kullanarak SQL veritabanı'nın kullanımını faturamı üzerinde görünmesini?
SQL veritabanı faturaları tahmin edilebilir bir saatlik hızı üzerinde temel alarak [satın alma modeli](sql-database-service-tiers.md). Gerçek kullanım hesaplanan ve saatlik, faturanızı bir saat kesirlerini gösterebilir. böylece Faturalaması. Örneğin, 12 saat içindeki bir ay için bir veritabanı zaten varsa, faturanıza 0,5 gün kullanımını gösterir. 

## <a name="what-if-a-single-database-is-active-for-less-than-an-hour-or-uses-a-higher-service-tier-for-less-than-an-hour"></a>Ne tek bir veritabanı değerinden bir saat için etkin veya daha az bir saatten daha yüksek bir hizmet katmanı kullanır?
Faturalandırılır her bir veritabanı yüksek hizmet katmanı kullanarak mevcut saat + kullanım veya veritabanı değerinden bir saat için etkin olup olmadığını bağımsız olarak bu saatte uygulanan performans düzeyi için. Örneğin, tek bir veritabanı oluşturun ve daha sonra beş dakika silerseniz faturanızı bir veritabanı saat için bir ücret yansıtır. 

Örnekler:

* Temel bir veritabanı oluşturun ve ardından hemen standart S1 yükseltin, standart S1 oranı ilk saat için sizden ücret kesilir.
* Bir veritabanı Basic'ten Premium 10: 00'da yükseltirseniz, ve yükseltme 1: 35'da tamamlar 1: 00'da başlayan Premium oranı ücretlendirilen aşağıdaki günü 
* 11: 00'da Premium bir veritabanından temel düşürmek varsa ve 2: 15'da tamamlandıktan sonra veritabanı Premium oranı 3: 00'a kadar geçmesi temel oranlarda doludur doludur.

## <a name="how-does-elastic-pool-usage-using-the-dtu-based-purchasing-model-show-up-on-my-bill"></a>Nasıl DTU tabanlı satın alma modeli kullanarak esnek Havuz Kullanım faturamı üzerinde görünmesini?
Esnek havuz ücretlendirilen Göster yukarı faturanızda esnek Dtu'lar (Edtu'lar) veya vCores artı gösterilen halinde depolama olarak [fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/sql-database/). Esnek havuzlar için veritabanı başına ücret ödemeden yoktur. Bir havuz en yüksek eDTU veya kullanım veya havuza değerinden bir saat için etkin olup olmadığını bağımsız olarak vCores bulunduğu her saat için faturalandırılır. 

DTU tabanlı satın alma modeli örnekler:

* 11: 18'da ile 200 Edtu'lar standart bir esnek havuz oluşturursanız, beş veritabanları havuzuna ekleme, 200 Edtu'lar için tüm saat için 11'da başlayan ücretlendirilirsiniz Günlük geri kalanı ile.
* Gün 2, 5: 05'da, veritabanı 1 50 edtu'larını kullanmaya başladığında ve gün sürekli tutar. Veritabanları 2-5 dalgalanma gösteren 0 ile 80 Edtu'lar arasında. Gün içinde gün boyunca değişen Edtu'lar tüketen beş veritabanları ekleyin. Gün 2 200 eDTU faturalandırılır tam bir günüdür. 
* Sabah üzerinde 3, 5 başka bir 15 veritabanları ekleyin. Burada Edtu'lar 200 havuzundan 400 için 20: 05'da artırmaya karar noktasına gün boyunca veritabanı kullanımı artırır 200 eDTU düzeyinde ücretleri 8 pm kadar yürürlükte olan ve kalan dört saat için 400 Edtu'lar artırır. 

## <a name="how-are-elastic-pool-billed-for-the-dtu-based-purchasing-model"></a>Nasıl esnek havuz DTU tabanlı için fatura satın alma modeli?
Esnek havuzları aşağıdaki özelliklere faturalandırılır:

* Olduğunda bile hiçbir veritabanı havuzundaki bir esnek havuz oluşturulduktan sonra faturalandırılır.
* Bir esnek havuz saatlik faturalandırılır. Bu mod tek veritabanlarının performans düzeyleri için olduğu gibi aynı Ölçüm sıklığıdır.
* Bir esnek havuz boyutlandırılır, yeniden boyutlandırma işlemi tamamlanana kadar ardından havuzu yeni miktarda kaynak göre faturalandırılır değil. Bu, tek veritabanları performans düzeyini değiştirme olarak aynı düzeni izler.
* Bir esnek havuz fiyat havuzu kaynaklardaki temel alır. Bir esnek havuz fiyat sayısına ve esnek veritabanları içindeki kullanımını bağımsızdır.

Ayrıntılar için bkz [SQL Database fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/) ve [hizmet katmanları](sql-database-service-tiers.md).

## <a name="how-does-the-vcore-based-usage-show-up-in-my-bill"></a>Nasıl vCore tabanlı kullanım faturamı içinde görünmesini? 
VCore tabanlı modelinde, hizmet Hizmet katmanını temel alan bir öngörülebilir, saatlik oranı faturalandırılır vCores içinde sağlanan işlem sağlanan GB/ay içinde depolama ve yedekleme depolama tüketilen. Yedeklemeler için depolama toplam veritabanı boyutu (diğer bir deyişle, veritabanı boyutu %100) aşarsa, ek ücretlere vardır. açıkça vCore saatleri, yapılandırılmış veritabanı depolama, tüketilen GÇ ve yedekleme depolama fatura içinde kullanılan kaynakları ayrıntılarını görmek kolaylaştırma listelenmektedir. Depolama en fazla yedekleme en fazla veritabanı boyutunun % 100'dür dahil, hangi, içinde faturalandırılır ötesinde GB/ay ay içinde tüketilen.

Örneğin:
- 12 saat içindeki bir ay için SQL veritabanı zaten varsa, fatura 12 saatlik vCore kullanımını gösterir. Ek 100 GB depolama SQL veritabanı sağlanan, fatura depolama kullanımı GB/ay birimlerinde eşit olarak bölünmüş saatlik ve bir ay içinde tüketilen IOs sayısı gösterir.
- SQL veritabanı bir saatten kısa bir süre için etkin ise, veritabanı var. her bir saat seçili, en yüksek hizmet katmanı kullanarak depolama ve kullanım veya veritabanı için etkin daha az daha olmasına bakılmaksızın bu saatte uygulanan GÇ sağlanan için faturalandırılır bir saat.
- Yönetilen bir örneği oluşturun ve daha sonra beş dakika silerseniz bir veritabanı saat için sizden ücret.
- Genel Amaçlı katmanında 8 sanal çekirdekli bir Yönetilen Örnek oluşturup bunu hemen 16 sanal çekirdekli yapılandırmaya yükseltirseniz, ilk saat için 16 sanal çekirdek fiyatı üzerinden ücretlendirilirsiniz.

> [!NOTE]
> Yedekleme ücretleri ve g/ç ücretleri için sınırlı bir süre 30 Haziran 2018 aracılığıyla ücretsiz.

## <a name="how-does-the-use-of-active-geo-replication-in-an-elastic-pool-show-up-on-my-bill"></a>Aktif coğrafi çoğaltma faturamı üzerinde bir esnek havuz gösterisini içinde kullanımını nasıl yapar?
Kullanarak tek veritabanlarını aksine [aktif coğrafi çoğaltma](sql-database-geo-replication-overview.md) esnek veritabanları ile doğrudan bir faturalama etkisi yoktur.  Yalnızca her havuzları (birincil havuzu ve ikincil havuzu) için sağlanan kaynakları için ücretlendirilirsiniz

## <a name="how-does-the-use-of-the-auditing-feature-impact-my-bill"></a>Denetim özelliğinin kullanılması faturamı nasıl etkiler?
Denetim içinde oluşturulan SQL Database hizmeti hiçbir ek maliyet ve tüm hizmet katmanları üzerinde kullanılabilir. Ancak, denetim günlüklerini depolamak için bir Azure Storage hesabı ve tablolar ve Kuyruklar Azure Storage'da yer Fiyatlardan geçerli denetim özelliğini kullanır, Denetim günlüğü boyutuna göre.

## <a name="how-do-i-reset-the-password-for-the-server-admin"></a>Sunucu Yöneticisi parolasını sıfırlama nasıl?
İçinde [Azure portal](https://portal.azure.com), tıklatın **SQL sunucuları**, listeden sunucuyu seçin ve ardından **parola sıfırlama**.

## <a name="how-do-i-manage-databases-and-logins"></a>Veritabanlarını ve oturum açma bilgileri nasıl yönetebilirim?
Bkz: [veritabanları ve oturum açma bilgileri yönetme](sql-database-manage-logins.md).

## <a name="how-do-i-make-sure-only-authorized-ip-addresses-are-allowed-to-access-a-server"></a>Yalnızca yetkili IP adreslerini bir sunucuya erişim izni nasıl emin?
Bkz: [nasıl yapılır: SQL veritabanında güvenlik duvarı ayarlarını yapılandırma](sql-database-configure-firewall-settings.md).

## <a name="what-is-an-expected-replication-lag-when-geo-replicating-a-database-between-two-regions-within-the-same-azure-geography"></a>Coğrafi çoğaltma yapılırken bir beklenen çoğaltma gecikmesi nedir aynı Azure Coğrafya içinde iki bölgeler arasında bir veritabanı?
Çoğaltma gecikmesi olmuştur ve biz şu anda beş saniyede bir RPO desteklenmesi değerinden coğrafi ikincil Azure zaman barındırılan eşleştirilmiş bölge önerilen ve aynı hizmet katmanı.

## <a name="what-is-an-expected-replication-lag-when-geo-secondary-is-created-in-the-same-region-as-the-primary-database"></a>Birincil veritabanı ile aynı bölgede coğrafi ikincil oluşturulduğunda beklenen çoğaltma gecikmesi nedir?
Ampirik verilerini temel alarak yok içi bölge ve arası bölge çoğaltma gecikmesi çok fazla birbirinden eşleştirilmiş bölge kullanılan Azure önerilen olduğunda. 

## <a name="if-there-is-a-network-failure-between-two-regions-how-does-the-retry-logic-work-when-geo-replication-is-set-up"></a>İki bölgeler arasında bir ağ hatası varsa, coğrafi çoğaltma ayarlarken yeniden deneme mantığı nasıl çalışır?
Bir bağlantıyı kes ise, biz 10 saniyede bağlantıları yeniden oluşturmak için yeniden deneyin.

## <a name="what-can-i-do-to-guarantee-that-a-critical-change-on-the-primary-database-is-replicated"></a>Birincil veritabanı üzerinde önemli bir değişiklik çoğaltılır güvence altına almak için ne yapabilirim?
Coğrafi ikincil bir zaman uyumsuz çoğaltma ve biz birincil ile tam eşitleme tutmak deneyin değil. Ancak biz önemli değişiklikler (örneğin, parola güncelleştirmeleri) çoğaltma emin olmak için eşitlenmesini zorlamak için bir yöntem sağlar. Tüm kaydedilmiş işlemleri çoğaltılana çağıran iş parçacığı engellediğinden zorlanmış eşitleme performansını etkiler. Ayrıntılar için bkz [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx). 

## <a name="what-tools-are-available-to-monitor-the-replication-lag-between-the-primary-database-and-geo-secondary"></a>Hangi Araçlar birincil veritabanı arasındaki coğrafi ikincil çoğaltma gecikmesi izlemek kullanılabilir var mı?
Biz, birincil veritabanı ile coğrafi-ikincil bir DMV aracılığıyla arasında gerçek zamanlı çoğaltma gecikmesi kullanıma sunar. Ayrıntılar için bkz [sys.dm_geo_replication_link_status](https://msdn.microsoft.com/library/mt575504.aspx).

## <a name="to-move-a-database-to-a-different-server-in-the-same-subscription"></a>Bir veritabanını aynı abonelik içinde farklı bir sunucuya taşımak için
İçinde [Azure portal](https://portal.azure.com), tıklatın **SQL veritabanları**, listeden bir veritabanı seçin ve ardından **kopya**. Bkz: [bir Azure SQL veritabanını kopyalama](sql-database-copy.md) daha fazla ayrıntı için.

## <a name="to-move-a-database-between-subscriptions"></a>Bir veritabanını abonelikler arasında taşıma
İçinde [Azure portal](https://portal.azure.com), tıklatın **SQL sunucuları** ve ardından listeden, veritabanını barındıran sunucuyu belirleyin. Tıklatın **taşıma**ve taşımak istediğiniz kaynakları ve taşımak için abonelik seçin.
