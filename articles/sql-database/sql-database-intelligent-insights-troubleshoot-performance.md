---
title: Akıllı Insights ile Azure SQL veritabanı performans sorunlarını giderme | Microsoft Docs
description: Akıllı Öngörüler Azure SQL veritabanı performans sorunlarını gidermenize yardımcı olur.
services: sql-database
author: danimir
manager: craigg
ms.reviewer: carlrab
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: article
ms.date: 04/04/2018
ms.author: v-daljep
ms.openlocfilehash: 7830a8a4bfc43e158069cc7cdc186e289e166751
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="troubleshoot-azure-sql-database-performance-issues-with-intelligent-insights"></a>Akıllı Insights ile Azure SQL veritabanı performans sorunlarını giderme

Bu sayfa aracılığıyla algılanan Azure SQL veritabanı performans sorunları hakkında bilgi sağlar. [akıllı Öngörüler](sql-database-intelligent-insights.md) veritabanı performans tanılama günlük. Bu tanılama günlük gönderilebilir [Azure günlük analizi](../log-analytics/log-analytics-azure-sql.md), [Azure Event Hubs](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md), [Azure Storage](sql-database-metrics-diag-logging.md#stream-into-storage), veya bir üçüncü taraf çözümü özel DevOps uyarı ve raporlama için yetenekleri.

> [!NOTE]
> Hızlı SQL veritabanı performans sorun giderme kılavuzu için akıllı Öngörüler aracılığıyla, bkz: [akış sorun giderme önerilen](sql-database-intelligent-insights-troubleshoot-performance.md#recommended-troubleshooting-flow) bu belgedeki akış çizelgesi.
>

## <a name="detectable-database-performance-patterns"></a>Algılanabilir veritabanı performans desenleri

Akıllı Öngörüler otomatik olarak algılar performans sorunlarını SQL sorgusu yürütme bekleme süresini, hatalar veya zaman aşımları göre veritabanı ile. Ardından, tanılama günlük algılanan performans desenlerini çıkarır. Algılanabilir performans desenleri aşağıdaki tabloda özetlenmiştir:

| Algılanabilir performans desenleri | Yüzdelik ayrıntıları |
| :------------------- | ------------------- |
| [Ulaşması kaynak sınırları](sql-database-intelligent-insights-troubleshoot-performance.md#reaching-resource-limits) | Kullanılabilir kaynakları (Dtu'lar), veritabanı çalışan iş parçacığı veya veritabanı oturum açma oturumları izlenen abonelikte kullanılabilir tüketimini sınırları, SQL veritabanı performans sorunlarıyla neden olan sınırına ulaştı. |
| [İş yükü artış](sql-database-intelligent-insights-troubleshoot-performance.md#workload-increase) | SQL veritabanı performans sorunlarıyla neden olan iş yükü artış ya da iş yükü veritabanında sürekli toplamı algılandı. |
| [Bellek baskısı](sql-database-intelligent-insights-troubleshoot-performance.md#memory-pressure) | Bellek verir istenen çalışanları için bellek ayırma istatistiksel olarak önemli miktarda zaman için beklemeniz gerekir. Veya SQL veritabanı performansını etkiler bellek verir istenen çalışanları artan toplamı yok. |
| [Kilitleme](sql-database-intelligent-insights-troubleshoot-performance.md#locking) | Aşırı veritabanı kilitleme, SQL veritabanı performansını etkiler algılandı. |
| [Artan MAXDOP](sql-database-intelligent-insights-troubleshoot-performance.md#increased-maxdop) | Paralellik seçeneği (MAXDOP) en büyük ölçüde değişti ve sorgu yürütme verimliliği etkiler. |
| [Pagelatch çakışması](sql-database-intelligent-insights-troubleshoot-performance.md#pagelatch-contention) | SQL veritabanı performansını etkiler Pagelatch çakışma algılandı. Birden çok iş parçacığı aynı anda aynı bellek içi veri arabelleği sayfaları erişme girişimi. Bu, artan bekleme zamanlarda, SQL veritabanı performansını etkiler sonuçlanır. |
| [Dizin eksik](sql-database-intelligent-insights-troubleshoot-performance.md#missing-index) | SQL veritabanı performansını etkiler eksik bir dizin sorunu algılandı. |
| [Yeni sorgu](sql-database-intelligent-insights-troubleshoot-performance.md#new-query) | Yeni bir sorgu genel SQL veritabanı performansı etkiler algılandı. |
| [Olağan dışı bekleme İstatistiği](sql-database-intelligent-insights-troubleshoot-performance.md#unusual-wait-statistic) | SQL veritabanı performansını etkiler olağan dışı veritabanı bekleme süresini algılandı. |
| [TempDB çakışması](sql-database-intelligent-insights-troubleshoot-performance.md#tempdb-contention) | SQL veritabanı performansını etkileyen bir performans sorunu neden aynı tempDB kaynaklara erişmek birden çok iş parçacığı deneyin. |
| [Esnek havuz DTU azalması](sql-database-intelligent-insights-troubleshoot-performance.md#elastic-pool-dtu-shortage) | Esnek Havuzda kullanılabilir Edtu yetersizliğinden SQL veritabanı performansını etkiler. |
| [Regresyon planlama](sql-database-intelligent-insights-troubleshoot-performance.md#plan-regression) | SQL veritabanı performansını etkiler yeni bir plan veya var olan bir planı iş yükünü bir değişiklik algılandı. |
| [Veritabanı kapsamlı yapılandırma değeri Değiştir](sql-database-intelligent-insights-troubleshoot-performance.md#database-scoped-configuration-value-change) | Bir yapılandırma değişikliği veritabanında SQL veritabanı performansını etkiler. |
| [İstemci yavaş](sql-database-intelligent-insights-troubleshoot-performance.md#slow-client) | SQL veritabanı performansını etkiler yeterince hızlı SQL veritabanından çıkış tüketen alamıyor yavaş uygulama istemci algılandı. |
| [Fiyatlandırma katmanı indirgeme](sql-database-intelligent-insights-troubleshoot-performance.md#pricing-tier-downgrade) | Bir fiyatlandırma katmanı indirgeme eylemi, SQL veritabanı performansını etkiler kullanılabilir kaynaklar azaltılabilir. |

> [!TIP]
> SQL veritabanı sürekli performansı iyileştirmek için etkinleştirme [Azure SQL veritabanı otomatik ayarlama](https://docs.microsoft.com/azure/sql-database/sql-database-automatic-tuning). SQL veritabanı yerleşik zekaya'Bu benzersiz özellik sürekli SQL veritabanınız izler, otomatik olarak dizinler ayarladığını ve sorgu yürütme planı düzeltmeleri uygular.
>

Aşağıdaki bölümde daha ayrıntılı daha önce listelenen algılanabilir performans desenleri açıklar.

## <a name="reaching-resource-limits"></a>Ulaşması kaynak sınırları

### <a name="what-is-happening"></a>Ne oluyor

Bu algılanabilir performans düzeni, kullanılabilir kaynak sınırları, çalışan sınırları ve oturum sınırları ulaşmak için ilgili performans sorunlarını birleştirir. Bu performans sorununu saptadıktan sonra tanılama günlüğünün açıklama alanı performans sorunu kaynak, çalışan veya oturum sınırları ilgili olup olmadığını gösterir.

SQL veritabanı kaynaklardaki genellikle denir [DTU kaynaklarını](https://docs.microsoft.com/azure/sql-database/sql-database-what-is-a-dtu). Bunlar, CPU ve g/ç (veri ve işlem günlüğü g/ç) kaynakları oluşan karışık bir ölçüyü oluşur. Kaynak sınırları ulaşmasını desenini algılandığında tanınır sorgu performansında ölçülen kaynak sınırları hiçbirini ulaşmasını nedeni.

Oturum sınırları kaynak SQL veritabanı için kullanılabilir eşzamanlı oturum açma sayısını ifade eder. SQL veritabanları için bağlı olan uygulamalar veritabanına kullanılabilir eşzamanlı oturum açma sayısı üst sınırına ulaştınız olduğunda bu performans deseni tanınır. Uygulamalar bir veritabanında mevcut olandan daha fazla oturumları kullanmayı denerseniz, sorgu performansı etkilenir.

Çalışan sınırları ulaşmasını kullanılabilir çalışanları DTU kullanımı sayılan değil çünkü kaynak sınırları ulaşmasını, belirli bir durumdur. Bir veritabanı üzerinde çalışan sınırları ulaşmasını ve sorgu performansında sonuçlanır neden kaynak özgü bekleme zamanlarının neden olabilir.

### <a name="troubleshooting"></a>Sorun giderme

Tanılama Günlüğü, performans ve kaynak tüketimi yüzdeleri etkilenen sorgularının sorgu karmaları çıkarır. Veritabanının yükünüzü en iyi duruma getirmek için bir başlangıç noktası olarak bu bilgileri kullanabilirsiniz. Özellikle, bir performans düşüşü dizinleri ekleyerek etkileyen sorguları iyileştirebilir. Veya daha fazla bile iş yükü dağıtım uygulamalarla en iyi duruma getirebilirsiniz. İş yükü azaltın veya en iyi duruma getirme olun yapamıyorsanız, kullanılabilir kaynakları miktarını artırmak için SQL veritabanı aboneliğinizin fiyatlandırma katmanı artırmayı düşünün.

Kullanılabilir oturum sınırları ulaştıysanız, veritabanında yapılan oturum açma sayısı azaltılarak, uygulamalarınızı en iyi duruma getirebilirsiniz. Veritabanı, uygulamalardan oturumu sayısını azaltın yapamıyorsanız, fiyatlandırma katmanı, veritabanınızın artırmayı düşünün. Veya bölme ve veritabanınızı daha dengelenmiş bir iş yükü dağıtım için birden çok veritabanı içine taşıyın.

Oturum sınırları çözme ile ilgili daha fazla öneriler için bkz: [SQL veritabanı en fazla oturum açma sınırları ile nasıl](https://blogs.technet.microsoft.com/latam/2015/06/01/how-to-deal-with-the-limits-of-azure-sql-database-maximum-logins/). Abonelik katmanı için kullanılabilir kaynak sınırları öğrenmek için bkz: [SQL veritabanı kaynak sınırları](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits).

## <a name="workload-increase"></a>İş yükü artış

### <a name="what-is-happening"></a>Ne oluyor

Bu performans deseni bir iş yükü artış nedeni veya daha ciddi formunda, iş yükü pile-up sorunlarını tanımlar.

Bu algılama, çeşitli ölçümler bir birleşimi yapılır. Ölçülen temel ölçüm, son iş yükü taban çizgisi ile karşılaştırıldığında iş yükü artışı algılıyor. Algılama diğer tür sorgu performansını etkileyen kadar büyük bir büyük etkin çalışan iş parçacığı artış ölçmeye dayanır.

Kendi daha ciddi formunda, iş yükü sürekli olarak iş yükünü işlemek için nedeniyle kullanamama SQL veritabanı üst üste yığmak. İş yükü pile-up koşul sürekli büyüyen bir iş yükü boyutu sonucudur. Bu koşul nedeniyle, iş yükü yürütme için bekleyeceği süreyi artar. Bu koşul en ciddi veritabanı performans sorunlarıyla birini temsil eder. Bu sorunu durdurulan çalışan iş parçacığı sayısını artırma izleme yoluyla algılandı. 

### <a name="troubleshooting"></a>Sorun giderme

Tanılama günlük, yürütme artırmıştır sorgu sayısı ve iş yükü artış en büyük katkısı sorguyla sorgu karmasını çıkarır. İş yükü en iyi duruma getirmek için bir başlangıç noktası olarak bu bilgileri kullanabilirsiniz. İş yükünü artırmak için en büyük Katılımcısı olarak tanımlanan sorgu başlangıç noktası olarak özellikle yararlıdır.

İş yükleri daha eşit veritabanına dağıtma düşünebilirsiniz. Dizinleri ekleyerek performansı etkileyen sorgu en iyi duruma getirme göz önünde bulundurun. Ayrıca, iş yükünün birden çok veritabanı arasında dağıtabilirsiniz. Bu çözümlerin mümkün değilse, kullanılabilir kaynakları miktarını artırmak için SQL veritabanı aboneliğinizin fiyatlandırma katmanı artırmayı düşünün.

## <a name="memory-pressure"></a>Bellek baskısı

### <a name="what-is-happening"></a>Ne oluyor

Bu performans deseni bellek baskısı veya daha ciddi biçimde son yedi gün performans taban çizgisine göre bir bellek pile-up durumu nedeniyle geçerli veritabanı performans düşüşü gösterir.

Bellek baskısı çok sayıda çalışan iş parçacığı istekte bulunan bellek SQL veritabanında verir olduğu bir performans koşulu gösterir. Yüksek hacimli SQL veritabanı verimli bir şekilde istekte tüm çalışanlar için bellek ayıramıyor olan bir yüksek bellek kullanımına neden olur. Bu sorunu en yaygın nedenlerinden biri, bir yandan SQL veritabanına kullanılabilir bellek miktarı için ilişkilidir. Diğer taraftan, iş yükü artış çalışan iş parçacıkları ve bellek baskısı artırma neden olur.

Bellek baskısı daha ciddi biçiminde bellek pile-up durumdur. Bu durum, belleği serbest bırakma sorguları daha yüksek bir çalışan iş parçacığı sayısını bellek verir isteyen gösterir. SQL veritabanı altyapısı talebi karşılamak için verimli bir şekilde yeterli bellek ayıramıyor olduğundan bu istekte bulunan bellek ayrıca verir çalışan iş parçacığı sayısını sürekli (gösterilmelerini) artırmak olması. Bellek pile-up koşul en ciddi veritabanı performans sorunlarıyla birini temsil eder.

### <a name="troubleshooting"></a>Sorun giderme

Tanılama günlük yüksek bellek kullanımı ile ilgili zaman damgaları en yüksek nedenini olarak işaretlenmiş yazıcısı (diğer bir deyişle, çalışan iş parçacığı) ile bellek nesne deposu ayrıntıları çıkarır. Sorun giderme için temel olarak bu bilgileri kullanabilirsiniz. 

En iyi duruma getirme veya elemanı en yüksek bellek kullanımı ile ilgili sorgular kaldırın. Kullanmayı planladığınız olmayan veri sorgulama olmayan emin olabilirsiniz. Her zaman bir WHERE yan tümcesi sorgularınızda kullanmak iyi uygulamadır. Ayrıca, yerine veri arama için tarama kümelenmemiş dizinler oluşturmanızı öneririz.

En iyi duruma getirme veya birden fazla veritabanı dağıtma ayrıca iş yükünü azaltabilir. Veya, iş yükünün birden çok veritabanı arasında dağıtabilirsiniz. Bu çözümlerin mümkün değilse, SQL veritabanı aboneliğinizi veritabanına kullanılabilir bellek kaynakları miktarını artırmak için fiyatlandırma katmanı artırmayı düşünün.

Ek sorun giderme önerileri için bkz: [bellek meditasyon verir: gizemli SQL Server bellek tüketici birçok adlarla](https://blogs.msdn.microsoft.com/sqlmeditation/2013/01/01/memory-meditation-the-mysterious-sql-server-memory-consumer-with-many-names/).

## <a name="locking"></a>Kilitleme

### <a name="what-is-happening"></a>Ne oluyor

Bu performans deseni, aşırı veritabanı kilitleme son yedi gün performans taban çizgisine göre algılandığında geçerli veritabanı performans düşüşü gösterir. 

Modern RDBMS içinde kilitleme birden çok eşzamanlı çalışan ve paralel veritabanı işlemleri, mümkün olduğunda çalıştırarak performans ekranı birden çok iş parçacıklı sistemleri uygulamak için gereklidir. Bu bağlamda kilitleme yalnızca tek bir işlem özel olarak satır, sayfalar, tablolar ve gereklidir ve kaynaklar için başka bir işlem ile rekabet olmayan dosyalar erişebilmeniz için yerleşik erişim mekanizması ifade eder. Kaynakları kullanmak için kilitli işlem bunlarla yapıldığında, gerekli kaynaklara erişmek diğer işlemleri sağlayan bu kaynaklar üzerindeki kilit yayımlanır. Kilitleme hakkında daha fazla bilgi için bkz: [Veritabanı Altyapısı'nda kilitlemek](https://msdn.microsoft.com/library/ms190615.aspx).

SQL altyapısı tarafından yürütülen işlemler kullanım için kilitli kaynaklara erişmek için uzun süreler için bekleyen varsa, bu bekleme süresi iş yükü yürütme performans yavaşlama neden olur. 

### <a name="troubleshooting"></a>Sorun giderme

Tanılama günlük sorun giderme için temel olarak kullanabileceğiniz kilitleme ayrıntıları çıkarır. Bildirilen engelleme sorguları, diğer bir deyişle, kilitleme performansında tanıtmak sorguları çözümlemek ve bunları kaldırın. Bazı durumlarda, engelleme sorguları en iyi duruma getirme başarılı olabilir.

Sorunu azaltmak için kolay ve güvenli işlemler kısa tutun ve en pahalı sorguların kilit ayak izini azaltmak için yoludur. Daha küçük işlemlere işlemlerinin büyük bir toplu bozulabilir. Sorgu olabildiğince verimli hale getirerek sorgu kilit ayak izini azaltmak için iyi bir uygulama olur. Kilitlenmeler olasılığını artırmak ve genel veritabanı performansını olumsuz olduğundan büyük taramaları azaltın. Kilitleme neden tanımlanan sorgular için yeni dizinler oluşturun veya sütunlar tablo tarama önlemek için mevcut dizin ekleyin. 

Daha fazla bilgi için bkz: [SQL Server'da kilit etkinleşmesini nedeni engelleme sorunlarını gidermek nasıl](https://support.microsoft.com/help/323630/how-to-resolve-blocking-problems-that-are-caused-by-lock-escalation-in).

## <a name="increased-maxdop"></a>Artan MAXDOP

### <a name="what-is-happening"></a>Ne oluyor

Seçilen sorgu yürütme planı olduğu bir koşulu olması gereken fazlasını paralel birkaç ölçeklendirin bu algılanabilir performans düzeni gösterir. SQL veritabanı sorgu iyileştiricisi, mümkün olduğunda işlemleri hızlandırmak için paralel sorgular yürüterek iş yükü performansını geliştirebilirsiniz. Bazı durumlarda, bir sorgu işleme paralel çalışan diğer eşitleyebilir ve daha az sayıda paralel arkadaşlarınızla veya hatta bir tek çalışan iş parçacığı karşılaştırıldığında bazı durumlarda aynı sorgusu yürütme için karşılaştırıldığında sonuçları birleştirme bekleyen daha fazla zaman ayırın.

Expert sistem taban çizgisi dönemin karşılaştırıldığında geçerli veritabanı performansını analiz eder. Bu, daha önce çalışan bir sorgu sorgu yürütme planı daha fazla paralel birkaç ölçeklendirin için önce olmalıdır daha yavaş çalışıp çalışmadığını belirler.

SQL veritabanı MAXDOP sunucu yapılandırma seçeneğini kaç CPU çekirdekleri paralel olarak aynı sorguyu yürütmek için kullanılan denetlemek için kullanılır. 

### <a name="troubleshooting"></a>Sorun giderme

Tanılama günlük bunlar olması gereken birden fazla paralel birkaç ölçeklendirin için kendisi için yürütme süresi artar sorguları ilişkili sorgu karmaları çıkarır. Günlük CXP bekleme süresini de çıkarır. Bu süre sonuçları birleştirme ve şimdi taşıma önce tamamlamak tüm diğer iş parçacıkları için bekleyen bir tek bir düzenleyici/Düzenleyici iş parçacığı (iş parçacığı 0) süresini temsil eder. Ayrıca, düşük performanslı sorguları yürütme genel bekliyorduk bekleme süresini tanılama günlük çıkarır. Sorun giderme için temel olarak bu bilgileri kullanabilirsiniz.

İlk olarak, en iyi duruma getirme veya karmaşık sorgular basitleştirin. Uzun toplu işler içine küçük olanları bölmeniz iyi uygulamadır. Ayrıca, sorgularınızı desteklemek için dizinler oluşturduğunuzdan emin olun. Zayıf gerçekleştirme olarak işaretlenmiş bir sorgu için maksimum paralellik derecesi (MAXDOP) el ile de uygulayabilir. Bu işlem, T-SQL kullanarak yapılandırmak için bkz: [MAXDOP sunucu yapılandırma seçeneğini yapılandırma](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option).

MAXDOP sunucu yapılandırma seçeneği SQL veritabanı tek bir sorgu yürütme iş parçacığı paralel hale tüm kullanılabilir mantıksal CPU çekirdekleri kullanabileceğiniz varsayılan bir değer gösterir olarak sıfır (0) ayarlama. Ayarı birine (1) MAXDOP gösterir, yalnızca bir çekirdek tek bir sorgu yürütme için kullanılabilir. Pratikteki, bu paralellik devre dışı anlamına gelir. Servis talebi başına servis talebi temel bağlı olarak veritabanı ve tanılama için kullanılabilir çekirdekler bilgileri günlüğe kaydeder, durumunuz sorunu çözebilecek paralel sorgu yürütme için kullanılan çekirdek sayısı MAXDOP seçeneğine ayarlayabilirsiniz.

## <a name="pagelatch-contention"></a>Pagelatch çakışması

### <a name="what-is-happening"></a>Ne oluyor

Bu performans düzeni geçerli veritabanı iş yükü performans düşüşünü pagelatch Çekişme son yedi gün iş yükü taban çizgisine göre gösterir.

Mandal etkinleştirmek için SQL veritabanı tarafından kullanılan basit eşitleme mekanizmaları olan çoklu iş parçacığı kullanımı. Bunlar, dizinler, veri sayfaları ve diğer iç yapıları içeren bellek içi yapıların tutarlılığı garanti.

SQL database türlerde tutma yok. Kolaylık olması amacıyla, arabellek tutma arabellek havuzu bellek içi sayfalarında korumak için kullanılır. G/ç tutma henüz arabellek havuzu yüklenen sayfaları korumak için kullanılır. Her veri yazılan veya arabellek havuzu içinde bir sayfa okuma bir çalışan iş parçacığı bir arabellek Mandal sayfası için öncelikle edinmeniz gerekir. Bir çalışan iş parçacığı zaten bellek içi arabellek havuzunda kullanılabilir olmayan bir sayfaya erişmeye çalıştığında, bir g/ç isteği depolama biriminden gerekli bilgileri yüklemek için yapılır. Bu olaylar dizisi performansında daha ciddi biçimi gösterir.

Birden çok iş parçacığı aynı anda artan bekleme süresi sorgu yürütme tanıtır aynı bellek içi yapısına kilitler elde etmeye sayfasında tutma üzerinde Çekişme oluşur. Veri depolama biriminden erişilmesi gerektiğinde pagelatch GÇ çakışma olması durumunda bu bekleme süresi daha büyüktür. İş yükü performansını önemli ölçüde etkileyebilir. Pagelatch Çekişme birbirine bekleyen ve birden çok CPU sistem kaynakları için rekabete iş parçacıklarının en yaygın senaryodur.

### <a name="troubleshooting"></a>Sorun giderme

Tanılama günlük pagelatch Çekişme ayrıntıları çıkarır. Sorun giderme için temel olarak bu bilgileri kullanabilirsiniz.

Bir iç denetim mekanizmasını SQL veritabanının bir pagelatch olduğu için bunu otomatik olarak bunların ne zaman kullanılacağı belirler. Şema tasarımına dahil olmak üzere uygulama kararları tutma belirleyici davranışını nedeniyle pagelatch davranışı etkileyebilir.

Mandal Çekişme işlemek için bir yöntem ekler bir dizin aralığı eşit olarak dağıtmanızı sıralı olmayan bir anahtara sahip bir sıralı dizin anahtarı değiştirmektir. Genellikle, dizin önde gelen bir sütunda iş yükü orantılı olarak dağıtır. Tablo bölümleme dikkate alınması gereken başka bir yöntem. Bölümlenmiş bir tablodaki bir hesaplanan sütun düzeniyle bölümleme karma oluşturma aşırı Mandal Çekişme Azaltıcı için ortak bir yaklaşımdır. Pagelatch GÇ çakışma olması durumunda, dizinleri sunarak bu performans sorunu azaltılmasına yardımcı olur. 

Daha fazla bilgi için bkz: [Tanıla ve çözümleme Mandal Çekişme SQL Server'da](http://download.microsoft.com/download/B/9/E/B9EDF2CD-1DBF-4954-B81E-82522880A2DC/SQLServerLatchContention.pdf) (PDF indirme).

## <a name="missing-index"></a>Dizin eksik

### <a name="what-is-happening"></a>Ne oluyor

Bu performans deseni eksik dizin nedeniyle son yedi gün temel karşılaştırılan geçerli veritabanı iş yükü performans düşüşü gösterir.

Bir dizin sorgularının performansını hızlandırmak için kullanılır. Ziyaret veya taranan gerek dataset sayfa sayısını azaltarak tablo verileri hızlı erişim sağlar.

Performans düşüşüne neden özel sorgular oluşturma dizinleri performans için yararlı olacak bu algılama ile tanımlanır.

### <a name="troubleshooting"></a>Sorun giderme

Tanılama günlük iş yükü performansını etkileyen için tanımlanmış sorgular için sorgu karmaları çıkarır. Bu sorgular için dizin oluşturabilirsiniz. Ayrıca en iyi duruma getirme veya gerekli değildir, bu sorguları kaldırın. İyi bir performans uygulama kullanmadığınız veri sorgulama kaçınmaktır.

> [!TIP]
> SQL veritabanı yerleşik zekaya veritabanlarınız için en iyi performans gösteren dizinleri otomatik olarak yönetebilirsiniz biliyor muydunuz?
>
> SQL veritabanı sürekli performansı iyileştirmek için etkinleştirmenizi öneririz [SQL veritabanı otomatik ayarlama](sql-database-automatic-tuning.md). SQL veritabanı yerleşik zekaya'Bu benzersiz özellik sürekli SQL veritabanınız izler ve otomatik olarak ayarladığını ve veritabanınız için dizin oluşturur.
>

## <a name="new-query"></a>Yeni Sorgu

### <a name="what-is-happening"></a>Ne oluyor

Bu performans deseni, yeni bir sorgu kötü gerçekleştirme ve yedi günlük performans taban çizgisine göre iş yükü performansınızı etkilemeden algılandığını gösterir.

Sorgu iyi çalıştırma bazen yazmak zor bir görev olabilir. Sorgu yazmakla ilgili daha fazla bilgi için bkz: [yazma SQL sorguları](https://msdn.microsoft.com/library/bb264565.aspx). Varolan sorgu performansını iyileştirmek için bkz: [sorgu ayarlama](https://msdn.microsoft.com/library/ms176005.aspx).

### <a name="troubleshooting"></a>Sorun giderme

Tanılama çıktıları bilgileri kendi sorgu karmaları dahil olmak üzere iki yeni CPU tüketimi sorguların çoğu, en fazla oturum açın. Algılanan sorgu iş yükü performansı etkilediğinden, sorgunuzu en iyi duruma getirebilirsiniz. Kullanmanız gereken verileri almak için iyi bir uygulama olur. Ayrıca sorguları WHERE yan tümcesi ile kullanmanızı öneririz. Ayrıca karmaşık sorgular basitleştirmek ve küçük sorgulara bölün öneririz. Daha küçük Toplu sorguları büyük Toplu sorgulara bölmek için başka bir iyi bir uygulama olur. Yeni sorgular için dizin Tanıtımı genellikle bu performans sorunu azaltmak için iyi bir uygulamadır.

Kullanmayı [Azure SQL veritabanı sorgu performansı öngörüleri](sql-database-query-performance.md).

## <a name="unusual-wait-statistic"></a>Olağan dışı bekleme İstatistiği

### <a name="what-is-happening"></a>Ne oluyor

Bu algılanabilir performans deseni düşük performanslı sorguların son yedi gün iş yükü taban çizgisine göre tanımlanan bir iş yükü performans düşüşü gösterir.

Bu durumda, sistem herhangi bir standart algılanabilir performans kategoriler altında düşük performanslı sorguların sınıflandırmak olamaz, ancak bekleme istatistiği regresyon için sorumlu algılandı. Bu nedenle, onu bunları sorgularıyla olarak düşünür *olağan dışı bekleme istatistikleri*, burada regresyon için sorumlu olağan dışı bekleme istatistiği da sunulur. 

### <a name="troubleshooting"></a>Sorun giderme

Tanılama günlük olağan dışı bekleme süresi ayrıntıları hakkında bilgi, etkilenen sorgular ve bekleme süresinin sorgu karmaları çıkarır.

Sistem kök nedeni düşük performanslı sorgular için başarılı bir şekilde tanımlanamadı çünkü tanılama el ile sorun giderme için iyi bir başlangıç noktası bilgilerdir. Bu sorguların performansını en iyi duruma getirebilirsiniz. Yalnızca kullanın ve basitleştirmek ve küçük olanları içine karmaşık sorgular bölmek için gereksinim duyduğunuz veri getirme iyi bir uygulamadır. 

Sorgu performansı en iyi duruma getirme hakkında daha fazla bilgi için bkz: [sorgu ayarlama](https://msdn.microsoft.com/library/ms176005.aspx).

## <a name="tempdb-contention"></a>TempDB çakışması

### <a name="what-is-happening"></a>Ne oluyor

Bu algılanabilir performans deseni tempDB kaynaklara erişmeye çalışan iş parçacığı bir performans sorunu bulunduğu bir veritabanı performans koşulu belirtir. (Bu durum GÇ ilgili değildir.) Bu performans sorunu için tipik senaryo tüm oluşturma, kullanma ve küçük tempDB tablolarını bırak eş zamanlı sorguları yüzlerce ' dir. Sistem, aynı tempDB tablolar kullanarak eş zamanlı sorguları sayısı son yedi gün performans taban çizgisine göre veritabanı performansını etkileyebilir yeterli istatistiksel anlamlı ile artan algıladı.

### <a name="troubleshooting"></a>Sorun giderme

Tanılama günlük tempDB Çekişme ayrıntıları çıkarır. Sorun giderme için başlangıç noktası olarak bilgileri kullanabilirsiniz. Bu tür bir çakışma hafifletmek ve genel iş yükünün verimliliği artırmak için pursue iki şey vardır: geçici tabloları kullanarak durdurabilirsiniz. Bellek için iyileştirilmiş tablolar de kullanabilirsiniz. 

Daha fazla bilgi için bkz: [bellek için iyileştirilmiş tablolara giriş](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables). 

## <a name="elastic-pool-dtu-shortage"></a>Esnek havuz DTU azalması

### <a name="what-is-happening"></a>Ne oluyor

Bu algılanabilir performans deseni bir son yedi günlük taban çizgisine göre geçerli veritabanı iş yükü performans düşüşü gösterir. Aboneliğinizin esnek Havuzda kullanılabilir Dtu'lar yetersizliği nedeniyle değil. 

SQL veritabanı kaynaklardaki genellikle denir [DTU kaynaklarını](sql-database-what-is-a-dtu.md), CPU ve g/ç (veri ve işlem günlüğü g/ç) kaynakları oluşan karışık bir ölçüyü oluşur. [Azure esnek havuz kaynakları](sql-database-elastic-pool.md) amacıyla ölçekleme için birden çok veritabanı arasında paylaşılan kullanılabilir eDTU kaynağı havuzu olarak kullanılır. Kullanılabilir eDTU kaynaklarını esnek havuzunuzdaki havuzdaki tüm veritabanlarını desteklemek için yeterli büyüklükte değil, sistem tarafından bir esnek havuz DTU azalması performans sorunu algılandı.

### <a name="troubleshooting"></a>Sorun giderme

Tanılama günlük esnek havuz hakkında bilgi çıkarır, üst DTU tüketen veritabanlarını listeler ve üst tüketen veritabanı tarafından kullanılan havuzun DTU yüzdesi sağlar.

Bu performans koşulu aynı havuz edtu / esnek havuzda kullanarak birden çok veritabanı ilişkili olduğundan, sorun giderme adımlarını üst DTU tüketen veritabanlarına odaklanın. Bu veritabanlarında üst tüketen sorguları en iyi duruma getirilmesi içeren iş yükü üst tüketen veritabanlarında azaltabilir. Kullanmadığınız veri sorgulama değil de sağlayabilirsiniz. Başka bir üst DTU tüketen veritabanları kullanarak uygulamaları iyileştirmek ve iş yükünün birden çok veritabanları arasında yeniden dağıtmak için bir yaklaşımdır.

Azaltma ve üst DTU tüketen veritabanlarında geçerli iş yükü en iyi duruma getirilmesi mümkün değilse, fiyatlandırma katmanı, esnek havuz artırmayı düşünün. Örneğin esnek Havuzda kullanılabilir Dtu'lar artış sonuçlarında artırın.

## <a name="plan-regression"></a>Regresyon planlama

### <a name="what-is-happening"></a>Ne oluyor

Bu algılanabilir performans deseni SQL veritabanı yetersiz sorgu yürütme planı kullanan bir koşul gösterir. Yetersiz plan genellikle uzun kez geçerli ve diğer sorgular için beklenecek müşteri adayları artan sorgu yürütme neden olur.

SQL veritabanı en az sorgu yürütme planla belirler bir sorgu yürütme maliyeti. Sorgular ve iş yüklerini değişiklik türü, bazen mevcut planları artık verimlidir veya SQL veritabanı iyi değerlendirme belki de siz yapmadıysanız. Düzeltme meselesi sorgu yürütme planları el ile zorlanabilir.

Bu algılanabilir performans deseni planı regresyon üç farklı örneklerini birleştirir: Yeni plan regresyon, eski plan regresyon ve var olan planları değiştirilen iş yükü. Oluştu planı regresyon belirli türünü sağlanan *ayrıntıları* tanılama günlük bir özellik.

Yeni plan regresyon koşul içinde SQL veritabanı eski plan olarak verimli olmayan yeni bir sorgu yürütme planı Yürütülüyor başladığı bir durumu gösterir. Yeni plan olarak verimli olmayan eski plan yeni, daha verimli bir plana kullanarak SQL veritabanı geçiş yaptığında eski plan regresyon koşul durumuna başvuruyor. Var olan planları değiştirilen iş yükü regresyon, eski ve yeni planlar sürekli olarak, daha düşük performanslı planı doğru giderek Bakiye alternatif durumunu ifade eder.

Plan gerileme hakkında daha fazla bilgi için bkz: [planı regresyon SQL Server'daki nedir?](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2017/06/09/what-is-plan-regression-in-sql-server/). 

### <a name="troubleshooting"></a>Sorun giderme

Tanılama günlük sorgu karmaları, iyi plan kimliği, hatalı planı kimliği ve sorgu kimlikleri çıkarır. Sorun giderme için temel olarak bu bilgileri kullanabilirsiniz.

Hangi planı sağlanan sorgu karmaları ile tanımlayabilirsiniz, özel sorgular gerçekleştirmek daha iyi analiz edebilirsiniz. Hangi planı sorgularınızı daha iyi çalışır belirledikten sonra el ile zorlayabilirsiniz. 

Daha fazla bilgi için bkz: [SQL Server'ın planı gerileme nasıl engeller öğrenin](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2017/04/25/you-shall-not-regress-how-sql-server-2017-prevents-plan-regressions/).

> [!TIP]
> SQL veritabanı yerleşik zekaya en gerçekleştirme sorgu yürütme planları, veritabanları için otomatik olarak yönetebilirsiniz biliyor muydunuz?
>
> SQL veritabanı sürekli performansı iyileştirmek için etkinleştirmenizi öneririz [SQL veritabanı otomatik ayarlama](sql-database-automatic-tuning.md). SQL veritabanı yerleşik zekaya'Bu benzersiz özellik sürekli SQL veritabanınız izler ve otomatik olarak ayarladığını ve sorgu en gerçekleştirme veritabanlarınız için yürütme planları oluşturur.
>

## <a name="database-scoped-configuration-value-change"></a>Veritabanı kapsamlı yapılandırma değeri Değiştir

### <a name="what-is-happening"></a>Ne oluyor

Bu algılanabilir performans deseni veritabanı kapsamlı yapılandırma değişikliği için son yedi gün veritabanı iş yükü davranışını karşılaştırıldığında algılanan performans regresyon neden olan bir koşulu belirtir. Bu model için veritabanı kapsamlı yapılandırma yapılan son zamanlarda bir değişiklik, veritabanınızın performansını yararlı görünmemektedir gösterir.

Veritabanı kapsamlı yapılandırma değişiklikleri tek tek her veritabanı için ayarlanabilir. Bu yapılandırma, bir olay temelinde tek tek veritabanı performansını optimize etmek üzere kullanılır. Tek tek her veritabanı için aşağıdaki seçenekler yapılandırılabilir: MAXDOP, LEGACY_CARDINALITY_ESTIMATION, PARAMETER_SNIFFING, QUERY_OPTIMIZER_HOTFIXES ve Temizle PROCEDURE_CACHE.

### <a name="troubleshooting"></a>Sorun giderme

Tanılama önceki yedi günlük iş yükü davranışını kıyasla performans düşüşüne neden yakın zamanda yapılan çıkışları veritabanı kapsamlı yapılandırma değişiklikleri oturum açın. Önceki değerleri yapılandırma değişikliklerini geri dönebilirsiniz. İstenen Performans düzeyinin ulaşılana kadar değerini değere de ayarlayabilirsiniz. Tatmin edici performansa ile benzer bir veritabanından veritabanı kapsamlı yapılandırma değerlerini kopyalayın. Performans sorunlarını giderme yapamıyorsanız, varsayılan SQL veritabanı varsayılan değerlere geri dönmek ve bu temelinden başlayarak ince ayar girişimi.

Veritabanı kapsamlı yapılandırma ve yapılandırmasını değiştirme T-SQL söz dizimi en iyi duruma getirme hakkında daha fazla bilgi için bkz: [Alter veritabanı kapsamlı yapılandırma (Transact-SQL)](https://msdn.microsoft.com/library/mt629158.aspx).

## <a name="slow-client"></a>İstemci yavaş

### <a name="what-is-happening"></a>Ne oluyor

Bu algılanabilir performans deseni veritabanı sonuçları gönderir kadar hızlı SQL veritabanını kullanarak istemci veritabanından çıkış kullanamayacaklarını bir koşulu belirtir. SQL veritabanı yürütülen sorguların sonuçlarını arabellekte depolama değil çünkü yavaşlar ve devam etmeden önce iletilen sorgu çıkışları kullanacak istemci bekler. Bu durum SQL veritabanından çıkışları Süren istemciye iletilecek yeterince yeterince hızlı olmayan bir ağ için de ilgili olabilir.

Bu koşul için son yedi gün veritabanı iş yükü davranışını kıyasla performans regresyon yalnızca algılanırsa, oluşturulur. Bu performans sorunu istatistiksel olarak önemli ölçüde performans düşüşünü oluşursa, yalnızca önceki performans davranışını karşılaştırıldığında algılandı.

### <a name="troubleshooting"></a>Sorun giderme

Bu algılanabilir performans deseni istemci-tarafı koşulu belirtir. Sorun giderme, istemci-tarafı uygulaması veya istemci tarafı ağ gereklidir. Tanılama günlük sorgu karmaları ve en son iki saat içinde kullanmak istemcinin bekliyor göründüğü bekleme süresini çıkarır. Sorun giderme için temel olarak bu bilgileri kullanabilirsiniz.

Bu sorguların tüketimi için uygulamanızın performansını en iyi duruma getirebilirsiniz. Olası ağ gecikmesi sorunları da göz önünde bulundurabilirsiniz. Son yedi gün performans taban çizgisi değişikliği performans düşüşünü sorunun temel nedeni son uygulama ya da ağ koşulu değişiklikleri bu performans regresyon olay neden olup olmadığını araştırabilirsiniz. 

## <a name="pricing-tier-downgrade"></a>Fiyatlandırma katmanı indirgeme

### <a name="what-is-happening"></a>Ne oluyor

Bu algılanabilir performans deseni SQL veritabanı aboneliğinizi fiyatlandırma katmanı alt sürüme bir koşulu belirtir. Nedeniyle azaltma kaynakların (Dtu'lar) veritabanı için kullanılabilen, sistem, bir açılan son yedi günlük taban çizgisine göre geçerli veritabanı performansını algıladı.

Ayrıca, SQL veritabanı aboneliğinizi fiyatlandırma katmanı alt sürüme ve sonra kısa bir süre içinde daha yüksek bir katman için yükseltilmiş bir koşul olabilir. Bu geçici performans düşüşünü algılanması tanılama günlük bir fiyatlandırma katmanı düşürme ve yükseltme olarak Ayrıntılar bölümünde yüzdelik.

### <a name="troubleshooting"></a>Sorun giderme

Fiyatlandırma katmanınızı ve bu nedenle SQL veritabanına kullanılabilir Dtu'lar azaltılmış ve performans ile memnun kaldığınızda, hiçbir şey yapmanıza gerek yoktur. Fiyatlandırma katmanınızı azaltılmış ve SQL veritabanı performansınızı ile memnun, veritabanı iş yüklerinizi azaltın veya daha yüksek bir düzeye fiyatlandırma katmanı artırmayı düşünün.

## <a name="recommended-troubleshooting-flow"></a>Önerilen sorun giderme akış

 Akıllı Öngörüler kullanarak performans sorunlarını gidermek önerilen bir yaklaşım için akış çizelgesi izleyin.

Azure SQL analizi giderek Azure portalı üzerinden akıllı Öngörüler erişim. Gelen Performans Uyarısı bulup seçin girişiminde bulunuldu. Algılama sayfasında neler olduğunu tanımlayın. Sorun, sorgu metni, sorgu zaman eğilimleri ve olay evrimi sağlanan kök neden analizi gözlemleyin. Performans sorunu Azaltıcı için akıllı Öngörüler öneri kullanarak sorunu çözmeyi deneyin. 

[![Sorun giderme akış çizelgesi](./media/sql-database-intelligent-insights/intelligent-insights-troubleshooting-flowchart.png)](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/intelligent-insight/Troubleshoot%20Azure%20SQL%20Database%20performance%20issues%20using%20Intelligent%20Insight.pdf)

> [!TIP]
> Bir PDF sürümünü indirmek için akış çizelgesi seçin.

Akıllı Öngörüler genellikle bir saat performans sorunu kök neden çözümlemesi yapmak için zaman gerekir. Akıllı öngörü sorununuzu bulunamıyor ve sizin için önemli ise, Query Store el ile performans sorunu kök nedenini belirlemek için kullanın. (Genellikle, bu sorunları değerinden bir saat öncesine aittir.) Daha fazla bilgi için bkz: [Query Store kullanarak performansı izleyerek](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store).

## <a name="next-steps"></a>Sonraki adımlar
- Bilgi [akıllı Öngörüler](sql-database-intelligent-insights.md) kavramları.
- Kullanım [akıllı Öngörüler Azure SQL veritabanı performans tanılama günlük](sql-database-intelligent-insights-use-diagnostics-log.md).
- İzleyici [Azure SQL analizi kullanarak Azure SQL veritabanı](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-sql).
- Öğrenme [toplamak ve Azure kaynaklarınızdan günlük verilerini tüketen](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).
