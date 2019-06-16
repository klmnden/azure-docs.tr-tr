---
title: Akıllı İçgörüler - Azure SQL veritabanı ile veritabanı performansını izleme | Microsoft Docs
description: Azure SQL veritabanı Intelligent Insights yerleşik zeka, yapay zeka veritabanı kullanımını sürekli olarak izleyip kötü performansa neden aksatıcı olayları algılayacak kullanır.
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
ms.date: 12/19/2018
ms.openlocfilehash: 15154844c954e53ca1add5d3fbaa3e9d02152ad2
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60703226"
---
# <a name="intelligent-insights-using-ai-to-monitor-and-troubleshoot-database-performance"></a>Veritabanı performansı izleme ve sorun giderme için yapay ZEKA kullanarak akıllı Öngörüler

Azure SQL veritabanı akıllı Öngörüler, SQL veritabanı ve yönetilen örneği veritabanınızın performansı ile neler olduğunu bilmenizi sağlar.

Akıllı Öngörüler, yapay zeka veritabanı kullanımını sürekli olarak izleyip kötü performansa neden aksatıcı olayları algılayacak yerleşik zeka kullanır. Ayrıntılı bir analiz algılandığında bir sorunun akıllı değerlendirme ile tanılama günlüğü oluşturan gerçekleştirilir. Bu değerlendirme kök neden Analizi veritabanı performans sorununun oluşur ve mümkün olduğunda performans geliştirme önerileri.

## <a name="what-can-intelligent-insights-do-for-you"></a>Akıllı İçgörüler sizin için neler

Akıllı İçgörüler sağlayan aşağıdaki değeri Azure yerleşik zekası, benzersiz bir özellik şöyledir:

- Öngörülebilir izleme
- Özel olarak uyarlanmış performans öngörüleri
- Veritabanı performans düşüşü erkenden algılanması
- Kök neden analizini algılanan sorunlar
- Performans İyileştirme önerileri
- Yüz binlerce veritabanını yeteneğini ölçeği genişletme
- Olumlu etkiyi DevOps kaynaklara ve toplam sahip olma maliyeti

## <a name="how-does-intelligent-insights-work"></a>Akıllı Öngörüler nasıl çalışır

Akıllı Öngörüler veritabanı iş yükünden son bir saat son yedi gün temel iş yükü ile karşılaştırarak veritabanı performansı çözümler. Veritabanı iş yükü sorguları en yinelenen ve en büyük sorgular gibi veritabanı performans için en önemli belirlenen oluşur. Her veritabanı alt yapısı, veri, kullanımı ve uygulama göre benzersiz olduğu için oluşturulan her bir iş yükü taban çizgisi tek bir örneği için benzersiz ve özel. Akıllı Öngörüler, iş yükü taban, bağımsız mutlak işletimsel eşikleri izler ve aşırı bekleme süresini, kritik özel durumları ve performansı etkileyebilir sorgu parameterizations sorunları ile ilgili sorunları algılar.

Bir performans düşüşü sorunu yapay zeka kullanarak birden çok gözlemlenen ölçümlerinden algılanmadığında, analiz gerçekleştirilir. Veritabanınız ile neler üzerinde bir akıllı Öngörüler tanılama günlüğü oluşturulur. Akıllı İçgörüler veritabanı performans sorunu çözülene kadar ilk görünümünü izleme kolaylaştırır. Her algılandı. sorunu yaşam döngüsü ile ilk sorunu algılama ve performans iyileştirme doğrulama tamamlanmasını için izlenir.

![Veritabanı Performans Analizi iş akışı](./media/sql-database-intelligent-insights/intelligent-insights-concept.png)

Ölçümleri ölçme ve veritabanı performans sorunlarını algılamak için kullanılan sorgu süresi, istek zaman aşımı, aşırı bekleme süresini ve dönemdeki hatalı istek temel alır. Ölçümler hakkında daha fazla bilgi için bkz. [algılama ölçümleri](sql-database-intelligent-insights.md#detection-metrics) bu belgenin bölüm.

SQL veritabanı performans performansındaki düşüşleri aşağıdaki özellikleri içerir, akıllı girişlerle tanılama günlüğüne kaydedilir tanımlanan:

| Özellik             | Ayrıntılar              |
| :------------------- | ------------------- |
| veritabanı bilgileri | Bir veritabanı üzerinde bir öngörü, Kaynak URI gibi algılandığı hakkındaki meta verileri. |
| Gözlemlenen zaman aralığı | Algılanan Insight aralığının başlangıç ve bitiş zamanı. |
| Etkilenen ölçümleri | Bir öngörü oluşturulmasına neden ölçümleri: <ul><li>Sorgu süresini uzatarak [saniye].</li><li>Aşırı bekleniyor [saniye].</li><li>[Yüzdesi] istekleri zaman aşımına uğradı.</li><li>Kapanan istekleri [yüzdesi].</li></ul>|
| Etkisi değeri | Ölçüm değeri ölçülür. |
| Etkilenen sorgular ve hata kodları | Karma ya da hata kodunu sorgulayın. Bu, etkilenen sorgular için kolayca ilişkilendirmek için kullanılabilir. Sorgu süresini artırmak, bekleme süresi, zaman aşımı sayıları veya hata kodları oluşur ölçümleri sağlanır. |
| Algılamalar | Bir olayın olduğu süre boyunca veritabanında tanımlanan algılama. 15 algılama desenleri vardır. Daha fazla bilgi için [veritabanı Intelligent Insights ile performans sorunlarını giderme](sql-database-intelligent-insights-troubleshoot-performance.md). |
| Kök neden analizi | Kök neden analizini okunabilir bir biçimde tanımlanan sorun. Bazı Öngörüler, mümkün olduğunda performans iyileştirme önerisi içeriyor olabilir. |
|||

Azure SQL Analytics ile akıllı İçgörüler kullanarak uygulamalı bir genel bakış ve tipik kullanım senaryoları için katıştırılmış video bakın:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Get-Intelligent-Insights-for-Improving-Azure-SQL-Database-Performance/player]
>

Akıllı Öngörüler keşfetmek ve SQL veritabanı performans sorunlarını ortaya çıkar. SQL veritabanı ve yönetilen örnek veritabanı performans sorunlarını gidermek için Intelligent Insights'ı kullanmak için bkz: [akıllı Öngörüler ile ilgili sorunları giderme Azure SQL veritabanı performans sorunlarını](sql-database-intelligent-insights-troubleshoot-performance.md).

## <a name="configure-intelligent-insights"></a>Intelligent ınsights'ı yapılandırma

Akıllı İçgörüler çıktısını bir akıllı Performans Tanılama Günlüğü ' dir. Bu günlük, Azure SQL Analytics, Azure Event Hubs'a ve Azure depolama için akış aracılığıyla çeşitli şekillerde - tüketilebilir veya bir üçüncü taraf ürün.

- Ürün ile birlikte kullanmak [Azure SQL Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-sql) Azure portalı kullanıcı arabirimi aracılığıyla öngörülerini görüntülemek için. Tümleşik Azure çözüm ve öngörülerini görüntülemek için en tipik yöntemi budur.
- Ürün geliştirme için özel izleme ve uyarı senaryoları biri Azure Event Hubs ile kullanma
- Ürün özel uygulama geliştirme için Azure depolama ile kullanın, örneğin özel raporlama, uzun süreli veri arşivleme ve benzeri gibi sunulur.

Akıllı İçgörüler diğer ürünlerle Azure SQL Analytics, Azure olay hub'ı, Azure depolama veya üçüncü taraf ürünleri için tüketim tümleştirilmesi ilk etkinleştirme akıllı günlük Insights'ta ("SQLInsights" günlüğü) tanı aracılığıyla gerçekleştirilir ayarlar dikey penceresinden bir veritabanı ve Intelligent Insights'ı yapılandırma verilerini bu ürünlerden birine akışla oturum açın.

Akıllı İçgörüler günlük kaydını etkinleştirmek ve alıcı ürüne akışını günlüğü verilerini yapılandırmak için nasıl hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı ölçümleri ve tanılama günlüğünü](sql-database-metrics-diag-logging.md).

### <a name="set-up-with-azure-sql-analytics"></a>Azure SQL Analytics ile ayarlama

Azure SQL Analytics çözümünü grafik kullanıcı arabirimi, raporlama ve uyarı verme özellikleri veritabanı performansını sağlar, yanı sıra akıllı Öngörüler tanılama verilerini günlüğe kaydedebilirsiniz.

> [!TIP]
> Hızlı Başlangıç: Veritabanı performans sorunlarını grafik kullanıcı arabirimi sağlayan Azure SQL Analytics ile birlikte kullanmak için Intelligent Insights'ı kullanarak hızlı bir başlangıç kapalı almak için en kolay yolu olan. Azure SQL Analytics çözümünü marketten ekleyin, bu çözüm içindeki bir çalışma alanı oluşturun ve üzerinde Intelligent Insights'ı etkinleştirmek istediğiniz her veritabanı için bir veritabanı için tanılama ayarları dikey penceresindeki "SQLInsights" günlük akışını yapılandırın Azure SQL Analytics çalışma alanı.
>

Ön gereksinim olan Azure SQL Analytics marketten Azure portalı panonuza eklenir ve bir çalışma alanı oluşturmak için bkz. [Azure SQL Analytics yapılandırın](../azure-monitor/insights/azure-sql.md#configuration)

Akıllı İçgörüler Azure SQL Analytics ile kullanmak için önceki adımda oluşturduğunuz Azure SQL Analytics çalışma alanına akışını Intelligent Insights günlük verilerini yapılandırma Bkz [Azure SQL veritabanı ölçümleri ve tanılama günlüğü](sql-database-metrics-diag-logging.md).

Aşağıdaki örnek, Azure SQL Analytics bir akıllı Öngörüler görüntülenen gösterir:

![Akıllı İçgörüler raporu](./media/sql-database-intelligent-insights/intelligent-insights-azure-sql-analytics.png)

### <a name="set-up-with-event-hubs"></a>Event Hubs ile ayarlama

Event Hubs ile akıllı İçgörüler kullanmak için Event Hubs'a akışını görmek için günlük verilerini akıllı İçgörüler yapılandırma [Stream Azure tanılama günlüklerinin Event Hubs'a](../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md).

Özel İzleme ve uyarı kurmak için Event Hubs'ı kullanmak için bkz: [ölçümleri ve tanılama özellikli yapmanız gerekenler günlüklerini, olay hub'ları](sql-database-metrics-diag-logging.md#what-to-do-with-metrics-and-diagnostics-logs-in-event-hubs).

### <a name="set-up-with-azure-storage"></a>Azure Depolama'yı ayarlama

Akıllı İçgörüler depolama ile kullanmak için Intelligent Insights günlük verileri depolama alanına akışla öğrenmek için yapılandırma [Stream Azure Depolama'ya](sql-database-metrics-diag-logging.md#stream-into-storage).

### <a name="custom-integrations-of-intelligent-insights-log"></a>Akıllı Öngörüler günlüğü, özel tümleştirmeler

Üçüncü taraf araçlarla veya birleştirilerek özel uyarılar ve geliştirme izleme Intelligent ınsights'ı kullanmak için bkz [Intelligent Insights veritabanı performans tanılama günlük kullanım](sql-database-intelligent-insights-use-diagnostics-log.md).

## <a name="detection-metrics"></a>Algılama ölçümleri

Akıllı Öngörüler oluşturmak olan algılama modelleri için kullanılan ölçümleri izleme temel alan:

- Sorgu süresi
- Zaman aşımı istekleri
- Aşırı bekleme süresi
- İstek hata verme

Sorgu süre ve zaman aşımı istekleri, birincil veritabanı iş yükü performansı ile ilgili sorunlar algılanıyor modelleri olarak kullanılır. Bunlar iş yükü neler doğrudan ölçmek için kullanılırlar. Performans düşüşü, aşırı iş yükü performansını tüm olası durumları algılamaya süre bekleyin ve kapanan istekleri ek modelleri iş yükü performansını etkileyen sorunları belirtmek için kullanılır.

Sistem otomatik olarak iş yükü değişiklikleri göz önünde bulundurur ve veritabanı için dinamik olarak yapılan sorgu isteği sayısı değişiklikleri normal ve çıkış sıradan veritabanı performans eşikleri belirleyin.

Tüm ölçümler birlikte algılanan her bir performans sorunu kategorilere ayıran bir scientifically türetilmiş veri modeli aracılığıyla çeşitli ilişkileri olarak kabul edilir. Bir akıllı Öngörüler sağlanan bilgileri içerir:

- Algılanan performans sorunu ayrıntıları.
- Kök neden analizi bir sorun algılandı.
- Mümkün olduğunda izlenen SQL veritabanının performansını geliştirmeye yönelik öneriler.

## <a name="query-duration"></a>Sorgu süresi

Sorgu süresi düşüşü modelini bireysel sorguya analiz eder ve artırma derlenip performans taban çizgisine göre bir sorgu yürütme süresini de algılar.

Derleme sorgu veya sorgu yürütme süresi, iş yükü performansını etkileyen SQL veritabanı'nın yerleşik zekası önemli bir artış algılarsa, bu sorguları sorgu süresi işaretlenmiş performans düşüşü sorunlarını.

Akıllı Öngörüler Tanılama Günlüğü performansı düşürülmüş sorguyu sorgu karması çıkarır. Sorgu karması, gelen performans azalmasını hangi sorgu süresini artan sorgu derleme ya da yürütme süresi artırmak için ilgili olup olmadığını gösterir.

## <a name="timeout-requests"></a>Zaman aşımı istekleri

Zaman aşımı istekleri performans düşüşü modeli, her sorgu analiz eder ve sorgu yürütme düzeyinde zaman aşımları ve genel istek zaman aşımı ve veritabanı düzeyinde performans taban çizgisi döneme göre herhangi bir artış algılar.

Yürütme aşaması bile ulaşmadan önce bazı sorgular zaman aşımına uğrayabilir. Durdurulan çalışan istekleri ve araçlarla, SQL veritabanı'nın yerleşik zekası, ölçer ve yürütme aşamasına veya var olup olmadığını veritabanı sınırına tüm sorguları analiz eder.

Yürütülen sorgulara ilişkin zaman aşımlarını sayısı veya iptal edilen istek çalışanların sayısını sistem yönetilen eşiği aştığında sonra ile akıllı Öngörüler Tanılama Günlüğü doldurulur.

Oluşturulan ınsights zaman aşımına uğrayan istek sayısı ve zaman aşımına uğradı sorguların sayısını içerir. Genel veritabanı düzeyi sağlanan veya performans düşüşü göstergesi yürütme aşamasında zaman aşımı artış ile ilgilidir. Zaman aşımları artış veritabanı performans için önemli kabul edilir, bu sorguları zaman aşımı performans düşüşü sorunlarını işaretlenir.

## <a name="excessive-wait-times"></a>Aşırı bekleme süreleri

Tek veritabanı sorguları aşırı bekleme zaman modeli izler. Bu, sistem tarafından yönetilen mutlak eşiklerin aşıldığı olağandışı ölçüde yüksek sorgu bekleme istatistikleri algılar. Aşağıdaki sorgu, aşırı bekleme süresi ölçümleri, Query Store bekleme istatistikleri (sys.query_store_wait_stats) yeni SQL Server özelliğini kullanarak uyulması gereken:

- Kaynak sınırlarını ulaşma
- Elastik havuz kaynak sınırlarını ulaşma
- Aşırı sayıda çalışan veya oturumu iş parçacıkları
- Aşırı veritabanı kilitleme
- Bellek baskısı
- Diğer bekleme istatistikleri

Kaynak sınırlarını ulaşma veya elastik havuz kaynak sınırları üzerinde bir abonelik veya esnek havuzdaki kullanılabilir kaynakların tüketimini mutlak eşikler aşıldığında gösterir. İstatistiklerine iş yükü performans düşüşü gösterir. Çalışan veya oturumu iş parçacıklarının aşırı sayıda içinde çalışan iş parçacıkları veya başlatılan oturumları sayısını mutlak eşiklerin aşıldığı bir durumu gösterir. İstatistiklerine iş yükü performans düşüşü gösterir.

Aşırı veritabanı kilitleme, bir veritabanında kilit sayısı mutlak eşikler aşıldığında bir koşul gösterir. Bu durum, iş yükü performans düşüşü gösterir. Bellek baskısı bellek isteyen iş parçacığı sayısını veren bir koşul mutlak bir eşiği aşıldı. Bu durum, iş yükü performans düşüşü gösterir.

Diğer bekleme istatistikleri algılama, Query Store bekleyin istatistikleri ölçülür çeşitli ölçümleri mutlak bir eşiği aşıldığında bir koşulu belirtir. İstatistiklerine iş yükü performans düşüşü gösterir.

Aşırı bekleme süresini algılandığında, kullanılabilir verileri bağlı olarak sonra Akıllı Öngörüler tanılama performans, beklenecek sorguları, yürütme neden ölçümleri ayrıntılarını düşürülmüş etkileyen ve etkilenen sorgularının çıktıları karmaları oturum ve ölçülen bekleme süresi.

## <a name="errored-requests"></a>Dönemdeki hatalı istek

İzleyiciler tek sorgular ve hata verme temel dönemindeki karşılaştırıldığında, sorgu sayısında artış algılar dönemdeki hatalı istek performans düşüşü modeli. Bu model, SQL veritabanı yerleşik zekası ile yönetilen mutlak eşiklerin aşıldığı kritik özel durumlar da izler. Sistem veritabanı ve iş yükü değişiklikleri hesaplar izlenen dönem içinde yapılan sorgu isteği sayısı otomatik olarak değerlendirir.

Ölçülen artış dönemdeki hatalı istek yapılan istekleri, toplam sayısına göre iş yükü performansının önemli kabul edilir, etkilenen sorgular dönemdeki hatalı istek performans düşüşü sorunlarını işaretlenir.

Akıllı İçgörüler günlük dönemdeki hatalı istek sayısı çıkarır. Bu, gelen performans azalmasını dönemdeki hatalı istek artış veya izlenen kritik özel durum eşiği ve performans düşüşü ölçülen süresi geçmeden ilgili gösterir.

İzlenen kritik özel durumlar sistem tarafından yönetilen mutlak eşikleri arası bir akıllı Öngörüler kritik özel durum ayrıntıları ile oluşturulur.

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [akıllı Öngörüler ile SQL veritabanı performans sorunlarını giderme](sql-database-intelligent-insights-troubleshoot-performance.md).
- Kullanım [Intelligent Insights SQL veritabanı performans tanılama günlüğü](sql-database-intelligent-insights-use-diagnostics-log.md).
- Bilgi edinmek için nasıl [SQL Analytics kullanarak SQL veritabanını İzle](../azure-monitor/insights/azure-sql.md).
- Bilgi edinmek için nasıl [toplamak ve Azure kaynaklarınızdan günlük verilerini kullanma](../azure-monitor/platform/diagnostic-logs-overview.md).
