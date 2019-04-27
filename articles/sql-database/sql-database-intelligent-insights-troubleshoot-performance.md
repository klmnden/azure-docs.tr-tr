---
title: Akıllı Öngörüler sayesinde Azure SQL veritabanı performans sorunlarını giderme | Microsoft Docs
description: Akıllı İçgörüler Azure SQL veritabanı performans sorunlarını gidermenize yardımcı olur.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: fff4aa947f878974d2d0f18f373b8c0917ed7d70
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60703508"
---
# <a name="troubleshoot-azure-sql-database-performance-issues-with-intelligent-insights"></a>Akıllı Öngörüler sayesinde Azure SQL veritabanı performans sorunlarını giderme

Bu sayfa, Azure SQL veritabanı'nda bilgileri sağlar ve yönetilen örnek performans sorunlarını tespit aracılığıyla [Intelligent Insights](sql-database-intelligent-insights.md) veritabanı performans tanılama günlük. Tanılama Günlüğü telemetri için yapılabilen [Azure İzleyici günlükleri](../azure-monitor/insights/azure-sql.md), [Azure Event Hubs](../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md), [Azure depolama](sql-database-metrics-diag-logging.md#stream-into-storage), veya bir üçüncü taraf çözümü özel DevOps uyarı verme ve raporlama özellikleri.

> [!NOTE]
> Sorun giderme kılavuzu Intelligent ınsights'ı kullanarak bir hızlı SQL veritabanı performans için bkz: [akışla ilgili sorunları giderme önerilen](sql-database-intelligent-insights-troubleshoot-performance.md#recommended-troubleshooting-flow) bu belgedeki akış çizelgesi.
>

## <a name="detectable-database-performance-patterns"></a>Algılanabilir veritabanı performansı düzenleri

Akıllı İçgörüler otomatik olarak algılar performans sorunlarını sorgu yürütme bekleme süreleri, hatalar veya zaman aşımları göre SQL veritabanı ve yönetilen örnek veritabanları ile. Bu, algılanan performans desenleri tanılama günlüğüne çıkarır. Algılanabilir performans desenleri aşağıdaki tabloda özetlenmiştir.

| Algılanabilir performans desenleri | Azure SQL veritabanı ve elastik havuzlar için açıklama | Yönetilen örnek veritabanları için açıklama |
| :------------------- | ------------------- | ------------------- |
| [Kaynak sınırlarını ulaşma](sql-database-intelligent-insights-troubleshoot-performance.md#reaching-resource-limits) | Kullanılabilir kaynakları (Dtu), veritabanı iş parçacıklarını veya veritabanı oturum açma oturumları izlenen abonelikte kullanılabilir tüketimini sınırlarına ulaştı. Bu SQL veritabanı performansı etkilediğini. | Yönetilen örnek limitleri CPU kaynaklarının kullanımını ulaştı. Bu veritabanı performansı etkilediğini. |
| [İş yükü artışı](sql-database-intelligent-insights-troubleshoot-performance.md#workload-increase) | İş yükünü artırmak veya iş yükü veritabanında sürekli birikmesi algılandı. Bu SQL veritabanı performansı etkilediğini. | İş yükü artışı algılandı. Bu veritabanı performansı etkilediğini. |
| [Bellek baskısı](sql-database-intelligent-insights-troubleshoot-performance.md#memory-pressure) | İstenen bellek verir çalışanları, bellek ayırmaları için istatistiksel olarak önemli miktarda zaman için beklemek zorunda. Veya istenen bellek verir çalışanları artan birikmesi yok. Bu SQL veritabanı performansı etkilediğini. | Bellek verir istediniz çalışanları bellek ayırmaları için istatistiksel bir zaman miktarı için bekliyor. Bu veritabanı performansı etkilediğini. |
| [Kilitleme](sql-database-intelligent-insights-troubleshoot-performance.md#locking) | Kilitleme aşırı veritabanı, SQL veritabanı performansını etkileyen algılandı. | Kilitleme aşırı veritabanı, veritabanı performansını etkileyen algılandı. |
| [Artan MAXDOP](sql-database-intelligent-insights-troubleshoot-performance.md#increased-maxdop) | Maksimum paralellik derecesi (MAXDOP) paralellik seçeneği, sorgu yürütme verimliliği etkileyen değişti. Bu SQL veritabanı performansı etkilediğini. | Maksimum paralellik derecesi (MAXDOP) paralellik seçeneği, sorgu yürütme verimliliği etkileyen değişti. Bu veritabanı performansı etkilediğini. |
| [Pagelatch çakışması](sql-database-intelligent-insights-troubleshoot-performance.md#pagelatch-contention) | Birden çok iş parçacığı, eşzamanlı olarak artan bekleme sürelerini kaynaklanan ve pagelatch Çekişme neden aynı bellek içi verileri arabellek sayfaları erişmeye çalıştığınız. Bu SQL veritabanı performansını etkiliyor. | Birden çok iş parçacığı, eşzamanlı olarak artan bekleme sürelerini kaynaklanan ve pagelatch Çekişme neden aynı bellek içi verileri arabellek sayfaları erişmeye çalıştığınız. Bu veritabanı performansı etkilediğini. |
| [Dizini yok](sql-database-intelligent-insights-troubleshoot-performance.md#missing-index) | SQL veritabanı performansını etkileyen eksik bir dizin algılandı. | Eksik bir dizin, veritabanı performansını etkileyen algılandı. |
| [Yeni sorgu](sql-database-intelligent-insights-troubleshoot-performance.md#new-query) | Yeni sorgu genel SQL veritabanı performansını etkileyen algılandı. | Yeni sorgu genel veritabanı performansını etkileyen algılandı. |
| [Artan bekleme istatistikleri](sql-database-intelligent-insights-troubleshoot-performance.md#increased-wait-statistic) | Artan veritabanı bekleme süresini, SQL veritabanı performansını etkileyen algılandı. | Artan veritabanı bekleme süresini, veritabanı performansını etkileyen algılandı. |
| [TempDB çakışması](sql-database-intelligent-insights-troubleshoot-performance.md#tempdb-contention) | Birden çok iş parçacığı, bir performans sorununa neden aynı TempDB kaynağa erişmeye çalışıyorsunuz. Bu SQL veritabanı performansı etkilediğini. | Birden çok iş parçacığı, bir performans sorununa neden aynı TempDB kaynağa erişmeye çalışıyorsunuz. Bu veritabanı performansı etkilediğini. |
| [Elastik havuz DTU eksik](sql-database-intelligent-insights-troubleshoot-performance.md#elastic-pool-dtu-shortage) | SQL veritabanı performansı esnek Havuzda kullanılabilir Edtu yetersiz etkiliyor. | Yönetilen örnek için olarak kullanılamaz, vCore modeli kullanır. |
| [Regresyon planlama](sql-database-intelligent-insights-troubleshoot-performance.md#plan-regression) | Yeni plan veya var olan bir planı yükündeki bir değişikliği algılandı. Bu SQL veritabanı performansı etkilediğini. | Yeni plan veya var olan bir planı yükündeki bir değişikliği algılandı. Bu veritabanı performansı etkilediğini. |
| [Veritabanı kapsamlı yapılandırma değeri Değiştir](sql-database-intelligent-insights-troubleshoot-performance.md#database-scoped-configuration-value-change) | Veritabanı performansını etkileyen SQL veritabanı'nda yapılandırma değişikliği algılandı. | Veritabanı performansını etkileyen veritabanı yapılandırma değişikliği algılandı. |
| [Yavaş istemci](sql-database-intelligent-insights-troubleshoot-performance.md#slow-client) | Yavaş uygulama istemci veritabanından çıkış yeterince hızlı tüketen silemiyor. Bu SQL veritabanı performansı etkilediğini. | Yavaş uygulama istemci veritabanından çıkış yeterince hızlı tüketen silemiyor. Bu veritabanı performansı etkilediğini. |
| [Fiyatlandırma katmanı düşürme](sql-database-intelligent-insights-troubleshoot-performance.md#pricing-tier-downgrade) | Fiyatlandırma katmanı indirgeme eylemi kullanılabilir kaynaklar azaltılabilir. Bu SQL veritabanı performansı etkilediğini. | Fiyatlandırma katmanı indirgeme eylemi kullanılabilir kaynaklar azaltılabilir. Bu veritabanı performansı etkilediğini. |

> [!TIP]
> SQL veritabanı'nın sürekli performans iyileştirme için etkinleştirme [Azure SQL veritabanı otomatik ayarlama](sql-database-automatic-tuning.md). SQL veritabanı'nın yerleşik zekası, bu benzersiz özellik sürekli olarak izler, SQL veritabanı, otomatik olarak dizinleri tabanlarını ve sorgu yürütme planı düzeltmeleri uygular.
>

Aşağıdaki bölümde algılanabilir performans modellerini daha ayrıntılı açıklanmaktadır.

## <a name="reaching-resource-limits"></a>Kaynak sınırlarını ulaşma

### <a name="what-is-happening"></a>Ne oluyor

Bu algılanabilir performans desen, kullanılabilir kaynak sınırları, çalışan sınırları ve oturum sınırlarını ulaşma için ilgili performans sorunlarını birleştirir. Bu performans sorununu algılandıktan sonra bir açıklama alanı tanılama günlüğü, performans sorunu kaynak, çalışan veya oturum sınırları ilgili olup olmadığını gösterir.

SQL veritabanı'nda kaynaklar için genellikle adlandırılır [DTU](sql-database-what-is-a-dtu.md) veya [sanal çekirdek](sql-database-service-tiers-vcore.md) kaynakları. Kaynak sınırlarını ulaşma desenini algılanan değerlendirilmiştir sorgu performansında nedeni ölçülen kaynak sınırlarını ulaşma.

Oturumu sınırları kaynak SQL veritabanı kullanılabilir eşzamanlı oturum açma sayısını gösterir. Bu performans desen, SQL veritabanlarına bağlanan uygulamalar veritabanına kullanılabilir eşzamanlı oturum açma sayısı üst sınırına ulaştınız, tanınır. Uygulamaları bir veritabanı üzerinde bulunandan daha fazla oturumları kullanmayı denerseniz, sorgu performansı etkilenir.

Çalışan sınırlarını ulaşma kullanılabilir çalışanlar, DTU veya sanal çekirdek kullanımı sayılmaz çünkü kaynak sınırlarını ulaşma, belirli bir durumdur. Bir veritabanı üzerinde çalışan sınırlarını ulaşma sorgu performansında sonuçları kaynağa özgü bekleme süresini Yükselişi neden olabilir.

### <a name="troubleshooting"></a>Sorun giderme

Tanılama Günlüğü performans ve kaynak tüketimi yüzdeleri etkilenen sorgu sorgu karmaları çıkarır. Bu bilgiler, veritabanı iş yükünüzü iyileştirmek için başlangıç noktası olarak kullanabilirsiniz. Özellikle, gelen performans azalmasını dizinleri ekleyerek etkileyen sorguları iyileştirebilir. Veya daha fazla bile iş yükü dağıtım uygulamalarıyla en iyi duruma getirebilirsiniz. İş yüklerini azaltmak veya en iyi duruma getirme yapmak zamanınız yoksa fiyatlandırma katmanını kullanılabilir kaynakları miktarını artırmak için SQL veritabanı aboneliğinizi çıkartabilirsiniz.

Kullanılabilir oturum sınırları ulaştıysanız, veritabanında yapılan oturum açma sayısını azaltarak uygulamalarınızı en iyi duruma getirebilirsiniz. Oturumlarının veritabanına uygulamalarınızdan azaltmak yapamıyorsanız, artan veritabanı fiyatlandırma katmanı göz önünde bulundurun. Bölme ve veritabanınızı daha dengeli bir iş yükü dağıtımı için birden çok veritabanı içine taşıyın.

Oturum sınırları çözümleme hakkında daha fazla öneri için bkz. [ile SQL veritabanı en fazla oturum açma bilgileri sınırlarını başa çıkma](https://blogs.technet.microsoft.com/latam/20../../how-to-deal-with-the-limits-of-azure-sql-database-maximum-logins/). Bkz: [kaynak bakış sınırlayan bir SQL veritabanı sunucusunda](sql-database-resource-limits-database-server.md) sunucu ve abonelik düzeyinde sınırları hakkında daha fazla bilgi için.

## <a name="workload-increase"></a>İş yükü artışı

### <a name="what-is-happening"></a>Ne oluyor

Bu performans desen tarafından bir iş yükü artış veya daha ciddi formunda, iş yükü pile-up kaynaklanan sorunları tanımlar.

Bu algılama yöntemi, çeşitli ölçümleri bir birleşimi yapılır. Ölçülen temel ölçüm son iş yükü taban çizgisi ile karşılaştırıldığında iş yükünde bir artış algılıyor. Bir form algılama, sorgu performansı etkileyen kadar büyük olduğundan büyük etkin çalışan iş parçacığı artış ölçmeye dayanır.

Daha ciddi hâli içinde iş yükü sürekli olarak iş yükünü işlemek için nedeniyle yükleyememesine SQL veritabanı'nın üst üste yığmak. İş yükü pile-up koşul sürekli olarak büyüyen bir iş yükü boyutu sonucudur. Bu koşul nedeniyle, iş yükü yürütme için bekleyeceği süreyi büyür. Bu durum çok ciddidir veritabanı performans sorunlarını birini temsil eder. İptal edilen çalışan iş parçacığı sayısını artırma izleme yoluyla bu sorunu algılandı. 

### <a name="troubleshooting"></a>Sorun giderme

Tanılama Günlüğü, yürütme arttı sorguların sayısını ve en büyük katkı iş yükünü artırmak için sorguyu sorgu karması çıkarır. İş yükü iyileştirmek için bir başlangıç noktası olarak bu bilgileri kullanabilirsiniz. En büyük iş yükü artışı katkıda bulunan olarak tanımlanan sorgu, başlangıç noktası olarak özellikle yararlıdır.

İş yükü daha eşit veritabanı dağıtma göz önünde bulundurabilirsiniz. Dizinleri ekleyerek performansını etkileyen sorgu en iyi duruma getirme göz önünde bulundurun. İş yükünüz birden fazla veritabanı arasında da dağıtabilirsiniz. Bu çözümler mümkün değilse, fiyatlandırma katmanını kullanılabilir kaynakları miktarını artırmak için SQL veritabanı aboneliğinizi çıkartabilirsiniz.

## <a name="memory-pressure"></a>Bellek baskısı

### <a name="what-is-happening"></a>Ne oluyor

Bu performans Düzen bellek baskısı veya daha ciddi biçimde son yedi günlük performans taban çizgisine göre bir bellek pile-up koşul nedeniyle geçerli veritabanı performans düşüşü gösterir.

Bellek baskısı çok sayıda çalışan iş parçacıkları SQL veritabanı'nda isteme bellek veren olduğu bir performans koşulu belirtir. Yüksek hacimli verimli bir şekilde isteyen tüm çalışanları bellek ayıramıyor SQL veritabanı yüksek bellek kullanımı durum neden olur. Bu sorun için en yaygın nedenlerinden biri, bir yandan SQL veritabanı için kullanılabilir bellek miktarı ilişkilidir. Öte yandan, iş yükü artış artışı çalışan iş parçacıkları ve bellek baskısı neden olur.

Bellek baskısı daha ciddi biçiminde bellek pile-up durumdur. Bellek serbest bırakma sorguları sayısından daha yüksek bir çalışan iş parçacığı sayısını bellek verir isteyen bu koşulu belirtir. SQL veritabanı altyapısı, talebi karşılamak için verimli bir şekilde yeterli bellek ayıramıyor olduğundan bu istekte bulunan bellek de verir çalışan iş parçacığı sayısını sürekli olarak (gösterilmelerini) artırma olması. Bellek pile-up koşul en ağır veritabanı performans sorunlarını birini temsil eder.

### <a name="troubleshooting"></a>Sorun giderme

Tanılama Günlüğü bellek nesne deposu ayrıntıları yüksek bellek kullanımı ile ilgili zaman damgaları en yüksek nedenini olarak işaretlenmiş (diğer bir deyişle, iş parçacığı) memuru ile çıkarır. Bu bilgileri, sorun giderme için bir temel olarak kullanabilirsiniz. 

En iyi duruma getirme veya elemanı en yüksek bellek kullanımı ile ilgili sorgularını kaldırın. Ayrıca kullanmayı planlamıyor veri sorgulama olmayan emin olmak isteyebilirsiniz. Her zaman bir WHERE yan tümcesi sorgularınızdaki kullanmak iyi uygulamadır. Ayrıca, verileri arama yerine tarayarak kümelenmemiş dizin oluşturmanızı öneririz.

En iyi duruma getirme veya birden çok veritabanı dağıtma iş yükünü de azaltabilir. Veya iş yükünüz birden fazla veritabanı arasında dağıtabilirsiniz. Bu çözümler mümkün değilse, fiyatlandırma katmanını veritabanına kullanılabilir bellek kaynaklarının miktarını artırmak için SQL veritabanı aboneliğinizi çıkartabilirsiniz.

Ek sorun giderme önerileri için bkz. [bellek meditasyon verir: Gizemli SQL Server bellek tüketici birçok adlarla](https://blogs.msdn.microsoft.com/sqlmeditation/20../../memory-meditation-the-mysterious-sql-server-memory-consumer-with-many-names/).

## <a name="locking"></a>Kilitleniyor

### <a name="what-is-happening"></a>Ne oluyor

Bu performans desen, aşırı veritabanı kilitleme son yedi günlük performans taban çizgisine göre algılandığında geçerli veritabanı performans düşüşü gösterir. 

Modern RDBMS kilitleme mümkün olduğu durumlarda birden çok eş zamanlı çalışanlar ve paralel veritabanı işlemleri çalıştırarak performansı ekranı birden çok iş parçacıklı sistemlerini uygulamak için temel bir özelliktir. Bu bağlamda kilitleme yalnızca tek bir işlem özel olarak satırları, sayfalar, tablolar ve gereklidir ve kaynaklar için başka bir işlem ile rekabet olmayan dosyalar erişmek yerleşik erişim mekanizması ifade eder. Kendileriyle kaynakları kullanmak için kilitli olan işlem yapıldığında, gerekli kaynaklara erişmek diğer işlemleri sağlayan bu kaynaklar üzerindeki kilit yayımlanır. Kilitleme hakkında daha fazla bilgi için bkz: [veritabanı altyapısı, kilit](https://msdn.microsoft.com/library/ms190615.aspx).

Kullanım için kilitli kaynaklara erişmek için uzun süreler için SQL altyapısı tarafından yürütülen işlem bekliyorsanız, bu bekleme süresi iş yükü yürütme performansı yavaşlama neden olur. 

### <a name="troubleshooting"></a>Sorun giderme

Tanılama Günlüğü temel olarak sorun giderme için kullanabileceğiniz kilitleme ayrıntılarını çıkarır. Bildirilen engelleme sorguları, diğer bir deyişle, kilitleme performansında tanıtan sorgular analiz edin ve bunları kaldırın. Bazı durumlarda, engelleme sorguları en iyi duruma getirme başarılı olabilir.

Sorunu gidermek için basit ve güvenli işlemler kısa tutmak ve en pahalı sorgu kilit ayak izini azaltmak için yoludur. Büyük bir toplu işlemleri daha küçük işlemler bozabilir. Sorgu mümkün olduğunda verimli hale getirerek sorgu kilit ayak izini azaltmak iyi uygulamadır. Kilitlenmeler olasılığını artırmak ve genel veritabanı performansı olumsuz etkileyebilir çünkü büyük taramalar azaltın. Kilitleme neden tanımlanan sorgular için yeni bir dizin oluşturun ya da tablo taramasından önlemek için mevcut dizini için sütunları ekleyin. 

Daha fazla öneri için bkz. [kilit azaltımı SQL Server'da kaynaklanan engelleme sorunlarını gidermek nasıl](https://support.microsoft.com/help/323630/how-to-resolve-blocking-problems-that-are-caused-by-lock-escalation-in).

## <a name="increased-maxdop"></a>Artan MAXDOP

### <a name="what-is-happening"></a>Ne oluyor

Seçilen sorgu yürütme planı olduğu bir koşul verilmiş olması kadar çok paralel bu algılanabilir performans desenini gösterir. SQL veritabanı sorgu iyileştiricisi, mümkün olduğunda işlemleri hızlandırmak için paralel olarak sorgu yürüterek iş yükü performansını geliştirebilirsiniz. Bazı durumlarda, bir sorgu işleme paralel çalışanların diğer eşitlemek ve daha az sayıda paralel çalışanların veya hatta bir tek iş parçacığı kıyasla bazı durumlarda aynı sorgu yürütme için Karşılaştırma sonuçları birleştirmek için bekleyen daha fazla zaman ayırıyor.

Uzman sistem taban çizgisi döneme göre geçerli veritabanı performansı analiz eder. Bu, daha önce çalışan bir sorgu daha sorgu yürütme planını daha paralel için önce olmamalıdır daha yavaş çalışıp çalışmadığını belirtir.

SQL veritabanı'nda MAXDOP sunucu yapılandırma seçeneği, aynı sorgu paralel olarak yürütmek için ne kadar CPU çekirdeği kullanılabilir denetlemek için kullanılır. 

### <a name="troubleshooting"></a>Sorun giderme

Tanılama Günlüğü verilmiş olması birden fazla paralel olduğundan, yürütme süresini artırılmış sorguları ilgili sorgu karmaları çıkarır. Günlük CXP bekleme süresini de çıkarır. Bu süre, tek bir düzenleyici/Düzenleyici iş parçacığı (iş parçacığı 0) için sonuçları birleştirme ve ileri taşıma önce tamamlamak tüm diğer iş parçacıklarını bekleme süresini temsil eder. Ayrıca, düşük performanslı sorgular yürütme genel bekliyorduk bekleme süresini Tanılama Günlüğü çıkarır. Bu bilgileri, sorun giderme için bir temel olarak kullanabilirsiniz.

İlk olarak, en iyi duruma getirme veya karmaşık sorgular basitleştirin. Yedekleme uzun toplu işler halinde daha küçük depolara bölmek için yararlı olur. Ayrıca, sorgularınızı desteklemek için dizinleri oluşturduğunuzdan emin olun. Zayıf gerçekleştirme olarak işaretlenmiş bir sorgu için maksimum paralellik derecesi (MAXDOP) el ile de uygulayabilirsiniz. Bu işlem, T-SQL kullanarak yapılandırmak için bkz [MAXDOP sunucu yapılandırma seçeneğini yapılandırma](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option).

MAXDOP sunucu yapılandırma seçeneği, SQL veritabanı kullanılabilir mantıksal CPU çekirdekleri tek bir sorgu yürütme iş parçacığı paralel hale getirmek için kullanabileceğiniz varsayılan bir değer gösterir şekilde sıfır (0) ayarlama. Ayarlama-tek (1) MAXDOP gösterir, yalnızca bir çekirdek tek bir sorgu yürütme için kullanılabilir. Pratikte, paralellik kapalıdır anlamına gelir. Örneği başına çalışması temel bağlı olarak kullanılabilir çekirdek sayısı veritabanı ve tanılama bilgileri günlüğe kaydetmek, MAXDOP seçeneğini, bu durumda sorunu giderebilecek paralel sorgu yürütme için kullanılan çekirdek sayısını ayarlayabilirsiniz.

## <a name="pagelatch-contention"></a>Pagelatch çakışması

### <a name="what-is-happening"></a>Ne oluyor

Bu performans deseni son yedi günlük iş yükü temel ile karşılaştırmasına pagelatch Çekişme nedeniyle geçerli veritabanı iş yükü performans düşüşü gösterir.

Mandal etkinleştirmek için SQL veritabanı tarafından kullanılan hafif eşitleme mekanizmalarını olan çoklu iş parçacığı kullanımı. Bunlar, dizinler, veri sayfaları ve diğer dahili yapıları içeren bir bellek içi yapıları tutarlılığı garanti.

SQL veritabanı'nda kullanılabilen birçok türde tutma vardır. Kolaylık olması amacıyla, arabellek tutma, bellek içi sayfalarında arabellek havuzu korumak için kullanılır. GÇ tutma, henüz arabellek havuzu yüklenen sayfalarda korumak için kullanılır. Her veri yazılır veya arabellek havuzunda bir sayfa okuma iş parçacığı bir arabellek Mandal sayfa için öncelikle edinmeniz gerekir. İş parçacığı zaten bellek içi arabellek havuzunda kullanılabilir olmayan bir sayfaya erişmeye çalıştığında, depolama alanından gerekli bilgileri yüklemek için bir g/ç isteği yapılır. Bu olaylar dizisi performans düşüşü daha ciddi bir form gösterir.

Birden çok iş parçacığı eşzamanlı olarak sorgu yürütme için bir artan bekleme süresi tanıtır aynı bellek içi yapısına Mandal alma girişimi sayfasında tutma üzerinde Çekişme gerçekleşir. Veri depolama alanından erişilmesi gerektiğinde pagelatch GÇ çakışma olması durumunda bu bekleme süresi daha büyüktür. İş yükü performansını önemli ölçüde etkileyebilir. Pagelatch Çekişme birbirleri üzerinde bekleyen ve birden çok CPU sistem kaynakları için rekabete iş parçacıklarının en yaygın senaryodur.

### <a name="troubleshooting"></a>Sorun giderme

Tanılama Günlüğü pagelatch Çekişme ayrıntılarını çıkarır. Bu bilgileri, sorun giderme için bir temel olarak kullanabilirsiniz.

SQL veritabanı'nın bir iç denetim mekanizması bir pagelatch olduğu için bunu otomatik olarak ne zaman kullanacağınızın belirler. Şema tasarımına dahil olmak üzere uygulama kararlar belirleyici davranışını Mandal nedeniyle pagelatch davranışı etkileyebilir.

Mandal Çekişme işlemek için bir yöntem ekler bir dizin aralığının arasında eşit bir şekilde dağıtmak için sıralı olmayan bir anahtara sahip bir sıralı dizin anahtarının değiştirmektir. Genellikle, önde gelen bir dizin sütunu orantılı olarak iş yükü dağıtır. Tablo bölümleme dikkate alınması gereken başka bir yöntem. Bölümleme düzeni ile bölümlenmiş bir tablodaki bir hesaplanan sütunda bir karma değer oluşturmak için aşırı Mandal çekişmeyi azaltmaya yaygın bir yaklaşımdır. Pagelatch GÇ çakışma olması durumunda, bu performans sorunu gidermek için dizinleri giriş'yardımcı olur. 

Daha fazla bilgi için [Tanıla ve Çöz Mandal SQL Server üzerinde Çekişme](https://download.microsoft.com/download/B/9/E/B9EDF2CD-1DBF-4954-B81E-82522880A2DC/SQLServerLatchContention.pdf) (indirilebilir PDF).

## <a name="missing-index"></a>Dizini yok

### <a name="what-is-happening"></a>Ne oluyor

Bu performans desen eksik bir dizin nedeniyle son yedi günlük taban çizgisine göre geçerli veritabanı iş yükü performans düşüşü gösterir.

Dizin, sorgu performansını hızlandırmak için kullanılır. Ziyaret ettiğinde veya taraması gereken veri kümesi sayfa sayısını azaltarak tablo verilerini hızlı erişim sağlar.

Performans düşüşüne neden olan özel sorgular oluşturma dizinleri performans için yararlı olacaktır bu algılama aracılığıyla tanımlanır.

### <a name="troubleshooting"></a>Sorun giderme

Tanılama Günlüğü, iş yükü performansı etkileyecek şekilde tanımlanan sorgular için sorgu karmaları çıkarır. Bu sorguları için dizinleri oluşturabilirsiniz. Ayrıca en iyi duruma getirme veya gerekli değilse, bu sorguları kaldırın. İyi bir performans uygulama kullanmadığınız veri sorgulama kaçınmaktır.

> [!TIP]
> SQL veritabanı'nın yerleşik zekası otomatik olarak en yüksek performansa veritabanlarınızın dizinlerini yönet biliyor muydunuz?
>
> SQL veritabanı'nın sürekli performansı iyileştirmek için bunu, etkinleştirmenizi öneririz. [SQL veritabanı otomatik ayarlama](sql-database-automatic-tuning.md). SQL veritabanı'nın yerleşik zekası, bu benzersiz özellik sürekli olarak izler, SQL veritabanı ve otomatik olarak ayarlar ve veritabanlarınızın dizinlerini oluşturur.
>

## <a name="new-query"></a>Yeni Sorgu

### <a name="what-is-happening"></a>Ne oluyor

Bu performans düzeni, yeni bir sorgu kötü gerçekleştirme ve yedi günlük performans taban çizgisine göre iş yükü performansını etkileyen algılandığını gösterir.

Bazen, iyi performanslı sorgu yazmak zor bir görev olabilir. Sorgu yazmakla ilgili daha fazla bilgi için bkz. [yazma SQL sorguları](https://msdn.microsoft.com/library/bb264565.aspx). Var olan sorgu performansını iyileştirmek için bkz [sorguyu ayarlamayı](https://msdn.microsoft.com/library/ms176005.aspx).

### <a name="troubleshooting"></a>Sorun giderme

Tanılama çıktıları bilgi kendi sorgu karmaları dahil olmak üzere iki yeni çoğu CPU kullanan sorguları, en fazla oturum açın. Algılanan sorgu iş yükü performansı etkilediğinden, sorgunuzun en iyi duruma getirebilirsiniz. Kullanmanız gereken verileri almak için yararlı olur. Ayrıca, sorguları WHERE yan tümcesi ile kullanmanızı öneririz. Ayrıca, karmaşık sorgular basitleştirin ve daha küçük sorgulara bölün öneririz. Başka bir büyük toplu iş sorguları daha küçük toplu işlem sorguları içine ayırmanız uygulamadır. Yeni sorgular için dizinleri giriş genellikle bu performans sorunu gidermek için iyi bir uygulamadır.

Kullanmayı [Azure SQL veritabanı sorgu performansı İçgörüleri](sql-database-query-performance.md).

## <a name="increased-wait-statistic"></a>Artan bekleme istatistikleri

### <a name="what-is-happening"></a>Ne oluyor

Bu algılanabilir performans Düzen düşük performanslı sorgular son yedi günlük iş yükü taban çizgisine göre tanımlanan bir iş yükü performans düşüşü gösterir.

Bu durumda, sistemin herhangi bir algılanabilir standart performans kategoriler altında düşük performanslı sorgular sınıflandırılamıyor ancak bekleme istatistikleri için gerileme sorumlu algılandı. Bu nedenle, bunları sorgularla olarak göz önünde bulundurur *artırılmış bekleme istatistikleri*, gerileme için sorumlu bekleme istatistikleri de burada gösterilir. 

### <a name="troubleshooting"></a>Sorun giderme

Tanılama artan bekleme süresi ayrıntıları ve sorgu karmaları etkilenen sorguların çıktıları bilgi oturum açın.

Sistem başarıyla düşük performanslı sorgular için kök nedeni tanımlanamadı çünkü tanılama bilgilerini el ile sorun giderme için iyi bir başlangıç noktası var. Bu sorguların performansını en iyi duruma getirebilirsiniz. Yalnızca kullanın ve basitleştirmek ve karmaşık sorgulara daha küçük depolara bölmek için ihtiyacınız olan verileri getirme iyi bir uygulamadır. 

Sorgu performansını en iyi duruma getirme hakkında daha fazla bilgi için bkz: [sorguyu ayarlamayı](https://msdn.microsoft.com/library/ms176005.aspx).

## <a name="tempdb-contention"></a>TempDB çakışması

### <a name="what-is-happening"></a>Ne oluyor

Bu algılanabilir performans Düzen tempDB kaynaklara erişmeye çalışan iş parçacıklarının engeli bulunduğu bir veritabanı performans koşulu belirtir. (Bu durum GÇ ilgili değildir.) Tipik bir senaryo için bu performans sorununu tüm oluşturma, kullanma ve ardından küçük tempDB tabloları kaldırın eş zamanlı sorguları yüzlerce ' dir. Sistem, aynı tempDB tabloları kullanarak eş zamanlı sorgu sayısı son yedi günlük performans taban çizgisine göre veritabanı performansını etkilemek için yeterli istatistiksel önemi daha fazla algıladı.

### <a name="troubleshooting"></a>Sorun giderme

Tanılama Günlüğü tempDB Çekişme ayrıntılarını çıkarır. Sorun giderme için başlangıç noktası olarak bilgileri kullanabilirsiniz. Bu tür bir çakışma çıkmıştır ve genel iş yükü, aktarım hızını artırmak için sonra amacınızın iki şey vardır: Geçici tabloları kullanarak durdurabilirsiniz. Bellek için iyileştirilmiş tablolar da kullanabilirsiniz. 

Daha fazla bilgi için [giriş bellek için iyileştirilmiş tablolara](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables). 

## <a name="elastic-pool-dtu-shortage"></a>Elastik havuz DTU eksik

### <a name="what-is-happening"></a>Ne oluyor

Bu algılanabilir performans deseni bir düşüş son yedi günlük taban çizgisine göre geçerli veritabanı iş yükü performansını gösterir. Aboneliğinizin esnek Havuzda kullanılabilir Dtu'lar yetersizliği kaynaklanır. 

SQL veritabanı'nda kaynaklar genellikle denir [DTU kaynaklarını](sql-database-purchase-models.md#dtu-based-purchasing-model), harmanlanmış bir CPU ve g/ç (veri ve işlem günlüğü g/ç) kaynakları oluşur. [Azure elastik havuzu kaynakları](sql-database-elastic-pool.md) amacıyla ölçeklendirme için birden çok veritabanı arasında paylaşılan kullanılabilir eDTU kaynaklarını oluşan bir havuz olarak kullanılır. Kullanılabilir eDTU kaynaklarını, elastik havuzdaki tüm veritabanlarının havuzdaki desteklemek için yeterli büyüklükte değil, sistem tarafından bir elastik havuz DTU eksik performans sorunu algılandı.

### <a name="troubleshooting"></a>Sorun giderme

Tanılama Günlüğü elastik havuz hakkında bilgi verir, üst DTU kullanan veritabanlarını listeler ve havuz DTU üst tüketen veritabanı tarafından kullanılan yüzdesini sağlar.

Bu performans koşulu birden çok veritabanını elastik havuzun Edtu aynı havuzu kullanarak ilişkili olduğundan, sorun giderme adımlarını üst DTU kullanan veritabanları üzerinde odaklanın. Bu veritabanlarında üst tüketen sorguları en iyi duruma getirilmesi içeren iş yükü üst tüketen veritabanlarında azaltabilir. Ayrıca, kullanmadığınız veri sorgulama olmayan emin olabilirsiniz. Başka bir yaklaşım, üst DTU kullanan veritabanlarını kullanarak uygulamaları en iyi duruma getirmek ve birden çok veritabanı arasındaki iş yükünü yeniden oluşturmaktır.

Azaltma ve üst DTU kullanan veritabanlarınızı geçerli iş yüküne en iyi duruma getirilmesi mümkün değilse, fiyatlandırma katmanı, bir elastik havuz artırmayı düşünün. Örneğin esnek Havuzda kullanılabilir dtu artışı sonuçlarında artırın.

## <a name="plan-regression"></a>Regresyon planlama

### <a name="what-is-happening"></a>Ne oluyor

Bu algılanabilir performans Düzen yetersiz sorgu yürütme planı, SQL veritabanı kullanan bir durumu gösterir. Yetersiz planı artık geçerli ve diğer sorgular kez beklenecek müşteri adayları artan sorgu yürütme, genellikle neden olur.

SQL veritabanı en az bir sorgu yürütme planı belirleyen bir sorgu yürütme için maliyet. Sorguları ve iş yüklerini değişiklik türünü, bazen mevcut planları artık verimlidir veya belki de, SQL veritabanı iyi değerlendirme yapmadım. Düzeltme ile ilgili konular, sorgu yürütme planlarını el ile zorlanabilir.

Plan gerileme üç farklı durumlarda bu algılanabilir performans desen birleştirir: Yeni plan regresyon, eski planı regresyon ve mevcut planları değiştirilen iş yükü. Belirli tür oluştu planı gerileme sağlanan *ayrıntıları* tanılama günlüğü özelliği.

Yeni plan regresyon koşul içinde SQL veritabanı eski planı kadar verimli olmayan yeni bir sorgu yürütme planı Yürütülüyor başladığı bir durumu gösterir. SQL veritabanı kullanarak yeni ve daha verimli bir planından yeni plan kadar verimli olmayan eski plana geçiş yaptığında eski planı regresyon koşul durumuna ifade eder. Var olan planları değiştirilen iş yükü regresyon, eski ve yeni planlar sürekli olarak, daha düşük performanslı planı doğru gidip Bakiye alternatif durumu ifade eder.

Plan gerilemeleri hakkında daha fazla bilgi için bkz. [planı gerileme SQL Server nedir?](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../what-is-plan-regression-in-sql-server/). 

### <a name="troubleshooting"></a>Sorun giderme

Tanılama Günlüğü sorgu karmaları, iyi bir plan kimliği, hatalı planı kimliği ve sorgu kimlikleri çıkarır. Bu bilgileri, sorun giderme için bir temel olarak kullanabilirsiniz.

Hangi planı için sağlanan sorgu karmalarıyla tanımlayabilirsiniz, belirli sorgularınızı gerçekleştirmek daha iyidir çözümleyebilirsiniz. Hangi planı sorgularınızı daha iyi çalışır belirledikten sonra el ile zorlayabilirsiniz. 

Daha fazla bilgi için [nasıl SQL Server planı gerilemeyi önler öğrenin](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../you-shall-not-regress-how-sql-server-2017-prevents-plan-regressions/).

> [!TIP]
> SQL veritabanı'nın yerleşik zekası, en yüksek performansa sorgu yürütme planlarını veritabanlarınız için otomatik olarak yönetebilirsiniz biliyor muydunuz?
>
> SQL veritabanı'nın sürekli performansı iyileştirmek için bunu, etkinleştirmenizi öneririz. [SQL veritabanı otomatik ayarlama](sql-database-automatic-tuning.md). SQL veritabanı'nın yerleşik zekası, bu benzersiz özellik sürekli olarak izler, SQL veritabanı ve otomatik olarak ayarlar ve en yüksek performansa sorgu veritabanlarınız için yürütme planlarını oluşturur.
>

## <a name="database-scoped-configuration-value-change"></a>Veritabanı kapsamlı yapılandırma değeri Değiştir

### <a name="what-is-happening"></a>Ne oluyor

Bu algılanabilir performans düzeni, veritabanı kapsamlı yapılandırma değişikliği son yedi günlük veritabanı iş yükü davranışını karşılaştırıldığında algılanan performans regresyon neden olan bir koşulu belirtir. Bu düzen veritabanı kapsamlı yapılandırmayı yapılan son değişiklik veritabanı performansınızı yararlı yaramadı gösterir.

Veritabanı kapsamlı yapılandırma değişiklikleri tek tek her veritabanı için ayarlanabilir. Bu yapılandırma, tek veritabanı performansını iyileştirmek için olay olarak kullanılır. Tek tek her veritabanı için aşağıdaki seçenekler yapılandırılabilir: MAXDOP, LEGACY_CARDINALITY_ESTIMATION, PARAMETER_SNIFFING, QUERY_OPTIMIZER_HOTFIXES ve NET PROCEDURE_CACHE.

### <a name="troubleshooting"></a>Sorun giderme

Tanılama için önceki yedi günlük iş yükü davranışını kıyasla performans düşüşüne neden olan kısa bir süre önce yapılan çıkışları veritabanı kapsamlı yapılandırma değişiklikleri günlüğe yazılır. Önceki değerleri yapılandırma değişiklikleri geri dönebilirsiniz. İstenen bir performans düzeyi ulaşılana kadar değere göre değeri de ayarlayabilirsiniz. Tatmin edici performansa ile benzer bir veritabanından veritabanı kapsamlı yapılandırma değerlerini kopyalayabilirsiniz. Performans sorunlarını giderme yapamıyorsanız, varsayılan SQL veritabanı varsayılan değerlere geri dönmesi ve bu temelinden başlayarak ince ayar yapma girişimi.

Veritabanı kapsamlı yapılandırma ve yapılandırmayı değiştirme T-SQL söz dizimi iyileştirme ile ilgili daha fazla bilgi için bkz: [Alter veritabanı kapsamlı yapılandırma (Transact-SQL)](https://msdn.microsoft.com/library/mt629158.aspx).

## <a name="slow-client"></a>Yavaş istemci

### <a name="what-is-happening"></a>Ne oluyor

Bu algılanabilir performans Düzen SQL veritabanını kullanarak istemci veritabanından çıkış kullanamıyor koşul sonuçları veritabanına gönderir gibi hızlı gösterir. SQL veritabanı bir arabellek çalıştırılan sorguların sonuçlarını depolamak üzere değildir çünkü yavaşlar ve devam etmeden önce iletilen sorgu çıkışları kullanacak istemci bekler. Bu durum ayrıca kullanan istemci SQL veritabanından çıktıları iletmek için yeterince yeterince hızlı olmayan bir ağ ilgili olabilir.

Bu durum, yalnızca performans regresyon, geçen yedi günlük veritabanı iş yükü davranışını karşılaştırıldığında algılanırsa oluşturulur. Bu performans sorununu istatistiksel performans düşüşü oluşması durumunda yalnızca önceki performans davranışını için kıyasla algılandı.

### <a name="troubleshooting"></a>Sorun giderme

Bu algılanabilir performans desen, bir istemci-tarafı koşulu belirtir. İstemci tarafı uygulama veya istemci tarafı ağ sorunlarını giderme gereklidir. Tanılama Günlüğü, son iki saat içinde tüketmeye istemcisi için en iyi bekleyen görünmektedir bekleme süresini ve sorgu karmaları çıkarır. Bu bilgileri, sorun giderme için bir temel olarak kullanabilirsiniz.

Bu sorguları tüketimi için uygulamanızın performansını en iyi duruma getirebilirsiniz. Olası ağ gecikmesi sorunları da göz önüne alabilirsiniz. Son yedi günlük performans taban çizgisi değişiklik performans düşüşü sorunu temel nedeni, yeni uygulama veya ağda koşul değişiklikler bu performans regresyon olayı kaynaklanmadığını araştırabilirsiniz. 

## <a name="pricing-tier-downgrade"></a>Fiyatlandırma katmanı düşürme

### <a name="what-is-happening"></a>Ne oluyor

Bu algılanabilir performans desen, SQL veritabanı aboneliğinizin fiyatlandırma katmanını ileri tarihli bir koşulu belirtir. Sistem, son yedi günlük taban çizgisine göre geçerli veritabanı performans nedeniyle azaltma kaynak (Dtu) veritabanı için kullanılabilir bir bırakma algıladı.

Ayrıca, SQL veritabanı aboneliğinizin fiyatlandırma katmanını indirgenen ve sonra kısa bir süre içinde daha yüksek bir katmana yükseltme bir koşul da olabilir. Bu geçici bir performans düşüşü algılama, tanılama günlük bir fiyatlandırma katmanı düşürme ve yükseltme olarak Ayrıntılar bölümünde yüzdelik.

### <a name="troubleshooting"></a>Sorun giderme

Fiyatlandırma katmanınızı ve bu nedenle SQL veritabanı için kullanılabilir Dtu'lar azaltılmış ve performans ile memnun kaldığınızda, hiçbir şey yapmanıza gerek yoktur. Fiyatlandırma katmanınızı azaltılmış ve SQL veritabanı performansınızı hizmetlerinizden, veritabanı iş yüklerinizi azaltın veya daha yüksek bir düzeye fiyatlandırma katmanını artırmayı düşünün.

## <a name="recommended-troubleshooting-flow"></a>Önerilen akışla ilgili sorunları giderme

 Intelligent ınsights'ı kullanarak performans sorunlarını gidermek önerilen bir yaklaşım için akış çizelgesi izleyin.

Erişim için Azure SQL Analytics giderek Azure Portalı aracılığıyla akıllı Öngörüler. Gelen Performans Uyarısı bulup seçin girişimi. Algılamalar sayfasında neler olduğunu tanımlayın. Belirtilen kök neden analizi sorunu, sorgu metni, sorgu zaman eğilimleri ve olay evrimi gözlemleyin. Performans sorunu giderme için akıllı Öngörüler öneri kullanarak sorunu çözmeyi deneyin. 

[![Sorun giderme akış çizelgesi](./media/sql-database-intelligent-insights/intelligent-insights-troubleshooting-flowchart.png)](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/intelligent-insight/Troubleshoot%20Azure%20SQL%20Database%20performance%20issues%20using%20Intelligent%20Insight.pdf)

> [!TIP]
> PDF sürümünü indirmek için akış'ı seçin.

Akıllı Öngörüler, genellikle bir saatlik zaman performans sorununun kök neden analizi gerçekleştirmek için gerekir. Akıllı İçgörüler sorununuzu bulunamıyor, kritik ise, Query Store el ile performans sorunun kök nedenini belirlemek için kullanın. (Genellikle, bu sorunları kısa bir saat öncesine aittir.) Daha fazla bilgi için [Query Store kullanarak performans izleme](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store).

## <a name="next-steps"></a>Sonraki adımlar
- Bilgi [Intelligent Insights](sql-database-intelligent-insights.md) kavramları.
- Kullanım [Intelligent ınsights'ı Azure SQL veritabanı performans tanılama günlüğü](sql-database-intelligent-insights-use-diagnostics-log.md).
- İzleyici [Azure SQL veritabanı'nı kullanarak Azure SQL Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-sql).
- Öğrenme [toplamak ve Azure kaynaklarınızdan günlük verilerini kullanma](../azure-monitor/platform/diagnostic-logs-overview.md).
