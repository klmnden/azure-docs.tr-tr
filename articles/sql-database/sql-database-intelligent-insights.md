---
title: "Akıllı Öngörüler - Azure SQL veritabanı ile veritabanı kullanımını izleme | Microsoft Docs"
description: "Azure SQL veritabanı akıllı Öngörüler yerleşik zekaya sürekli yapay zeka aracılığıyla veritabanı kullanımını izlemek ve yetersiz performansa neden kesintiye uğratan olayları algılamak için kullanır."
services: sql-database
documentationcenter: 
author: danimir
manager: drasumic
editor: carlrab
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Inactive
ms.date: 09/25/2017
ms.author: v-daljep
ms.openlocfilehash: 823855d88396a14ff7e5428a12d71384cdfe95a1
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2017
---
# <a name="intelligent-insights"></a>Intelligent Insights

Azure SQL veritabanı akıllı Öngörüler, veritabanı performansınızı ile neler olduğunu bilmenizi sağlar.

Akıllı Öngörüler sürekli yapay zeka aracılığıyla veritabanı kullanımını izlemek ve yetersiz performansa neden kesintiye uğratan olayları algılamak için yerleşik zekaya kullanır. Algılanan sonra ayrıntılı bir analiz sorunu akıllı değerlendirmesini içeren bir tanılama günlüğü oluşturan gerçekleştirilir. Bu değerlendirme veritabanı performans sorunu bir kök neden analizi oluşur ve mümkün olduğunda önerileri için performans iyileştirmeleri. 

## <a name="what-can-intelligent-insights-do-for-you"></a>Akıllı Öngörüler sizin için ne?

Akıllı Öngörüler aşağıdaki değeri sağlayan Azure yerleşik zekaya benzersiz yeteneğini şöyledir:

- Öngörülebilir izleme
- Uyarlanmış performansı öngörüleri
- Veritabanı performans düşüşünü erken algılanması
- Kök neden analizi algılanan sorunları
- Performansı geliştirme önerileri
- Yüz binlerce veritabanları yeteneğini ölçeğini genişletme
- DevOps kaynaklara ve toplam sahip olma maliyetini pozitif etkisi

## <a name="how-does-intelligent-insights-work"></a>Akıllı Öngörüler nasıl çalışır?

Akıllı Öngörüler son bir saat veritabanı yükünden son yedi gün temel iş yükü ile karşılaştırarak SQL veritabanı performansını çözümler. Veritabanı performansı gibi en yinelenen ve en büyük sorguları için en önemli olduğu belirlenen sorguları, veritabanının iş yükü oluşur. Her veritabanı alt yapısı, verileri, kullanım ve uygulama göre de benzersiz olduğundan, oluşturulan her bir iş yükü taban çizgisi belirli ve tek tek bir örneği için benzersiz değildir. Akıllı Insights, iş yükü taban çizgisini bağımsız ayrıca mutlak operasyon eşikleri izler ve aşırı bekleme süresini, kritik özel durumları ve performans etkileyebilecek sorgu parameterizations ile ilgili sorunları ile ilgili sorunları algılar.

Bir performans düşüşü sorunu yapay bilgileri kullanılarak birden çok gözlemlenen ölçümleri algılandıktan sonra analiz gerçekleştirilir. Tanılama Günlüğü veritabanınızı ile neler olduğunu üzerinde akıllı bir öngörü ile oluşturulur. Akıllı Öngörüler veritabanı performans sorunu çözümleme kadar ilk görünümünü izleme kolaylaştırır. Her sorun kendi ömrü ilk sorun algılama ve performans geliştirmesi doğrulanması kendi tamamlanıncaya kadar izlenen algıladı. Güncelleştirmeleri 15 dakikada bir tanılama günlüğüne sağlanır. 

![Veritabanı Performans Analizi iş akışı](./media/sql-database-intelligent-insights/intelligent-insights-concept.png)

Veritabanı performans sorunlarını algılamak ve ölçmek için kullanılan ölçümleri sorgu süresi, zaman aşımı istekleri, aşırı bekleme süresini ve hatalı istekleri temel alır. Ölçümler hakkında daha fazla bilgi için bkz: [algılama ölçümleri](sql-database-intelligent-insights.md#detection-metrics) bu belgenin bölüm.

SQL veritabanı performans degradations aşağıdaki özellikleri oluşur akıllı girişlerle tanılama günlüğüne kaydedilir tanımlanan:

| Özellik             | Ayrıntılar              |
| :------------------- | ------------------- |
| Veritabanı bilgileri | Üzerinde bir öngörü, kaynak URI'si gibi algılandı veritabanıyla ilgili meta veriler. |
| Gözlemlenen zaman aralığı | Algılanan Insight döneminin başlangıç ve bitiş zamanı. |
| Etkilenen ölçümleri | Oluşturulacak bir öngörü neden ölçümleri: <ul><li>Sorgu süresi [saniye] artırın.</li><li>Aşırı bekleme [saniye].</li><li>Zaman aşımına uğradı istekleri [yüzdesi].</li><li>Kapanan istekleri [yüzdesi].</li></ul>|
| Etkisi değeri | Ölçüm değeri ölçülür. |
| Etkilenen sorgular ve hata kodları | Karma ya da hata kodu sorgu. Bunlar kolayca etkilenen sorguları ilişkilendirmek için kullanılabilir. Ya da sorgu süresi artış, bekleme süresi, zaman aşımı sayıları veya hata kodları bileşiminden ölçümleri sağlanır. |
| Algılama | Bir olay süre boyunca veritabanını tanımlanan algılama. 15 algılama desenleri vardır. Daha fazla bilgi için bkz: [akıllı Insights ile veritabanı performans sorunlarını giderme](sql-database-intelligent-insights-troubleshoot-performance.md). |
| Kök neden analizi | Kök okunabilir bir biçimde tanımlanan sorunun çözümlenmesi neden. Bazı incelemeler, mümkün olduğunda performans geliştirme öneri içerebilir. |
|||

Tanılama günlüğüne performans sorunlarını sorunu yaşam döngüsünün üç durumdan birine ile işaretlenen: "Active", "Doğrulanıyor" ve "Tamamlandı". Sonra bir performans sorunu algılanır ve uzun onu mevcut SQL veritabanı yerleşik zekaya tarafından kabul olarak sorunu "Etkin" olarak işaretlenir. Sorunu azaltıldığından olarak kabul edildiğinde doğrulanır ve sorunu durumu "Doğrulanıyor" değiştirilir. SQL veritabanı yerleşik zekaya çözümlenen sorunu göz önünde bulundurur sonra sorun durumu "Tamamlandı" işaretlenir.

## <a name="use-intelligent-insights"></a>Akıllı Öngörüler kullanın

Akıllı Öngörüler akıllı Performans Tanılama Günlüğü olur. Tüketimi için diğer ürünleri ile tümleştirilebilir ve bu tür belirli uygulamalar Azure günlük analizi, Azure Event Hubs'a ve Azure depolama veya bir üçüncü taraf ürünleri. 

Azure günlük analizi birlikte akıllı Öngörüler genellikle bir web tarayıcısı ve belki de bir ürün kullanarak zemin kapalı almak için en kolay yollarından biri aracılığıyla Öngörüler görüntülemek için kullanılır. Azure Event Hubs ile birlikte akıllı Öngörüler genellikle özel izleme ve senaryoları uyarı yapılandırmak için kullanılır. Özel uygulama geliştirme için Azure storage ile birlikte akıllı Öngörüler kullanılan genellikle, gibi örneğin özel raporlama, veya belki de veri arşivleme ve alma.

Diğer ürünleri Azure günlük analizi ile akıllı Öngörüler, Azure olay hub'ı, Azure depolama ya da üçüncü taraf ürünleri tüketimi için tümleştirme ilk etkinleştirme akıllı (SQLInsights günlüğü) ve ardından yapılandırma Öngörüler gerçekleştirilir Akıllı Öngörüler bu ürünlerden birinin içine akışını veri oturum açın. Akıllı Öngörüler günlüğünü etkinleştirin ve süren bir ürüne akışını günlüğü verilerini yapılandırmak için hakkında daha fazla bilgi için bkz: [Azure SQL Database ölçümleri ve tanılama günlükleri](sql-database-metrics-diag-logging.md). 

Azure günlük analizi ile akıllı Öngörüler kullanarak uygulamalı bir genel bakış ve tipik kullanım senaryoları için katıştırılmış video bakın:


> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Get-Intelligent-Insights-for-Improving-Azure-SQL-Database-Performance/player]
>

Akıllı Öngörüler inanılmaz keşfetme ve SQL veritabanı performans sorunlarını giderme. SQL veritabanı performans sorunlarını gidermek için akıllı Öngörüler kullanabilmeniz için bkz: [akıllı Insights ile ilgili sorunları giderme Azure SQL veritabanı performans sorunlarını](sql-database-intelligent-insights-troubleshoot-performance.md).

## <a name="set-up-intelligent-insights-with-log-analytics"></a>Günlük analizi ile akıllı Öngörüler ayarlama 

Raporlama çözümü sağlar analizi ve tanılama veri oturum akıllı Öngörüler işlevleri uyarı.

Akıllı Öngörüler günlük analizi ile kullanmak için günlük analizi akışı için bkz: Akıllı Öngörüler günlüğü verilerini yapılandırmak [Azure SQL Database ölçümleri ve tanılama günlükleri](sql-database-metrics-diag-logging.md). 

Aşağıdaki örnek, bir akıllı Öngörüler raporu Azure SQL Analytics'te gösterir:

![Akıllı Öngörüler raporu](./media/sql-database-intelligent-insights/intelligent-insights-azure-sql-analytics.png)

Akıllı Öngörüler tanılama günlük SQL analizi için veri akışı için yapılandırıldıktan sonra şunları yapabilirsiniz [SQL analizi kullanarak SQL veritabanına izlemek](../log-analytics/log-analytics-azure-sql.md).

## <a name="set-up-intelligent-insights-with-event-hubs"></a>Event Hubs ile akıllı Öngörüler ayarlama

Akıllı Öngörüler Event Hubs ile kullanmak için Event Hubs'a akışı için bkz: Akıllı Öngörüler günlüğü verilerini yapılandırmak [akış Azure tanılama günlükleri Event hubs'a](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md).

Özel İzleme ve uyarı kurulumu için olay hub'ları kullanmak için bkz: [ne ölçümleri ve tanılama yapmak olay hub ' günlükleri](sql-database-metrics-diag-logging.md#what-to-do-with-metrics-and-diagnostics-logs-in-event-hubs). 

## <a name="set-up-intelligent-insights-with-storage"></a>Akıllı Insights depolama ile ayarlama

Akıllı Öngörüler depolama ile kullanmak için depolama alanına akışı için bkz: Akıllı Öngörüler günlüğü verilerini yapılandırmak [Azure Storage akışa](sql-database-metrics-diag-logging.md#stream-into-storage).

## <a name="custom-integrations-of-intelligent-insights-log"></a>Akıllı Öngörüler günlüğünün özel tümleştirmeler

Üçüncü taraf araçlarla ya da özel uyarı ve geliştirme izleme için akıllı Öngörüler kullanmak için bkz: [akıllı Öngörüler veritabanı performans tanılama günlük kullanım](sql-database-intelligent-insights-use-diagnostics-log.md).

## <a name="detection-metrics"></a>Algılama ölçümleri

Akıllı Öngörüler oluşturmak algılama modelleri için kullanılan ölçümleri izleme üzerinde temel alır:

- Sorgu süresi
- Zaman aşımı istekleri
- Aşırı bekleme süresi
- Hatalı istekleri çıkışı

Sorgu süresi ve zaman aşımı istekleri veritabanı iş yükü performansı ile ilgili sorunları seviyelerinden birincil modelleri olarak kullanılır. Bunlar doğrudan yüküyle neler olduğunu ölçmek için kullanılırlar. Düşüşü, aşırı iş yükü performansını tüm olası durumların algılamak için saat bekleyin ve kapanan istekleri ek modelleri olarak iş yükü performansını etkileyen sorunlar belirtmek için kullanılır.

Sistem iş yükü değişiklikleri otomatik olarak kabul eder ve normal ve çıkış sıradan veritabanı performans eşiklerini değişiklikleri veritabanına dinamik olarak yapılan bir sorgu isteği sayısı belirleyin.

Tüm ölçümleri birlikte algılanan her performans sorunu kategorilere ayıran bir scientifically türetilen veri modeli aracılığıyla çeşitli ilişkilerde olarak kabul edilir. Akıllı bir öngörü sağlanan bilgileri içerir:

* Algılanan performans sorunu ayrıntıları. 
* Algılanan sorun bir kök neden analizi. 
* Mümkün olduğunda izlenen SQL veritabanı performansını iyileştirmek nasıl öneriler sunar.

## <a name="query-duration"></a>Sorgu süresi

Sorgu süresi düşüşü model tek tek sorguları analiz eder ve derleyin ve performans taban çizgisine göre bir sorguyu çalıştırmak için geçen süre içinde artırma algılar.

SQL veritabanı yerleşik zekaya sorgu derleme veya iş yükü performansını etkileyen bir sorgu yürütme süresini önemli bir artış algılarsa, bu sorguları sorgu süresi işaretlenmiş performans düşüşünü sorunları. 

Akıllı Öngörüler tanılama günlük performans düşürülmüş sorgu sorgu karmasını çıkarır. Sorgu karma performansında hangi sorgu süresini artan sorgu derleme veya yürütme süresi artırmak için ilgili olup olmadığını gösterir.

## <a name="timeout-requests"></a>Zaman aşımı istekleri

Zaman aşımı istekleri düşüşü model tek tek sorguları analiz eder ve sorgu yürütme düzeyinde zaman aşımları ve genel istek zaman aşımı performans taban çizgisi dönemin karşılaştırıldığında veritabanı düzeyinde herhangi bir artış algılar.

Hatta yürütme aşaması ulaşmadan bazı sorgular zaman aşımı olabilir. İptal edilen çalışanları yapılan istekleri ve araçlarla, SQL veritabanı yerleşik zekaya ölçer ve yürütme aşamaya veya var olup olmadığını veritabanı ulaşıldı tüm sorguları analiz eder. 

Yürütülen sorgular için zaman aşımı sayısı veya iptal edilen isteği çalışan sayısı sistem yönetilen eşik kestiği sonra tanılama günlük akıllı ınsights ile doldurulur.

Oluşturulan Öngörüler zaman aşımına uğramış istek sayısı ve zaman aşımına uğradı olan sorgu sayısını içerir. Genel veritabanı düzeyi sağlanan veya zaman aşımı artış yürütme aşamada performansında bir göstergesi ilgilidir. Zaman aşımları artış veritabanı performansını önemli kabul edilir, bu sorguları zaman aşımı performans düşüşünü sorunlarını işaretlenir. 

## <a name="excessive-wait-times"></a>Aşırı bekleme süreleri

Aşırı bekleme süresi modeli tek tek veritabanı sorguları izler. Sistem tarafından yönetilen mutlak eşiklerin aşıldığı olağandışı ölçüde yüksek sorgu bekleme istatistikleri algılar. Aşağıdaki sorgu aşırı bekleme süresi ölçümleri yeni SQL Server özelliği, sorgu deposu bekleyin istatistikleri (sys.query_store_wait_stats) kullanarak gözlemlenen:

- Ulaşması kaynak sınırları
- Ulaşması esnek havuz kaynak sınırları
- Çalışan veya oturum iş parçacığı sayısının çok fazla
- Aşırı veritabanı kilitleme
- Bellek baskısı
- Diğer bekleme istatistikleri

Bir abonelik veya esnek havuz kullanılabilir kaynakların tüketimini mutlak eşiklerin aşıldığı ulaşması kaynak sınırları veya esnek havuzu kaynak sınırlarını gösterir. Bu istatistikleri iş yükü performans düşüşü gösterir. Çalışan veya oturum iş parçacığı sayısının çok fazla sayıda çalışan iş parçacığı veya başlatılan oturumlara mutlak eşiklerin aşıldığı bir koşul gösterir. Bu istatistikleri iş yükü performans düşüşü gösterir.

Aşırı veritabanı kilitleme, bir veritabanı üzerinde kilit sayısı mutlak eşiklerin aşıldığı bir koşul gösterir. Bu durum, iş yükü bir performans düşüşü gösterir. Bellek baskısı bellek isteme iş parçacığı sayısını verdiği bir koşul mutlak bir eşiği aşıldı. Bu durum, iş yükü bir performans düşüşü gösterir.

Diğer bekleme istatistikleri algılama sorgu deposu bekleyin istatistikleri ölçülen çeşitli ölçümleri mutlak bir eşik aşıldığında bir koşulu belirtir. Bu istatistikleri iş yükü performans düşüşü gösterir.

Aşırı bekleme süresini, kullanılabilir veri bağlı olarak algılanan sonra performans, beklenecek sorguları yürütme, neden ölçümleri ayrıntılarını düşürülmüş etkileyen ve etkilenen sorgularının çıktıları karmaları akıllı Öngörüler tanılama günlük ve ölçülen bekleme süresi.

## <a name="errored-requests"></a>Hatalı istekleri

İzleyiciler tek sorgular ve hata verdi, çıkışı temel dönemin karşılaştırıldığında, sorguları sayısında artış algılar hatalı istekleri düşüşü modeli. Bu model, SQL veritabanı yerleşik zekaya tarafından yönetilen mutlak eşiklerin aşıldığı kritik özel durumları de izler. Sistem veritabanı ve iş yükü değişiklikleri hesaplar izlenen dönemde yapılan sorgu isteği sayısı otomatik olarak göz önünde bulundurur.

Hatalı istekleri yapılan isteklerin toplam sayısı göre ölçülen artış iş yükü performansını önemli kabul edilir, etkilenen sorguları hatalı istekleri performans düşüşünü sorunlarını işaretlenir.

Akıllı Öngörüler günlük hatalı isteklerinin sayısı çıkarır. Bir performans düşüşü hatalı istekleri artış veya izlenen kritik özel durum eşiği ve performans düşüşünü ölçülen süresi geçmeden ilişkili belirtir. 

İzlenen kritik özel durumlar sistem tarafından yönetilen mutlak eşikleri arası ise akıllı bir öngörü kritik özel durum ayrıntıları ile oluşturulur.

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [akıllı Insights ile SQL veritabanı performans sorunlarını giderme](sql-database-intelligent-insights-troubleshoot-performance.md).
* Kullanım [akıllı Öngörüler SQL veritabanı performans tanılama günlük](sql-database-intelligent-insights-use-diagnostics-log.md).
* Bilgi edinmek için nasıl [SQL analizi kullanarak SQL veritabanı izleme](../log-analytics/log-analytics-azure-sql.md).
* Bilgi edinmek için nasıl [toplamak ve Azure kaynaklarınızdan günlük verilerini tüketen](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).


