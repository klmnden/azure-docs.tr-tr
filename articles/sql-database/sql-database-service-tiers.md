<properties
    pageTitle="SQL Database performansı ve seçenekleri: Hizmet katmanları | Microsoft Azure"
    description="Maliyeti ve işlevleri, kendi ölçeklendirmenize göre dengelemek için SQL Database performansını ve hizmet katmanlarının iş sürekliliği özelliklerini karşılaştırın."
    keywords="database options,database performance"
    services="sql-database"
    documentationCenter=""
    authors="carlrabeler"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="05/13/2016"
    ms.author="carlrab"/>

# SQL Database seçenekleri ve performansı: Her hizmet katmanında nelerin kullanılabildiğini anlama

[Azure SQL Database](sql-database-technical-overview.md) farklı iş yükleriyle kullanılabilecek birden çok hizmet katmanı içerir. Hizmet katmanlarını, uygulamanız için kesintiye neden olmaksızın dilediğiniz zaman değiştirebilirsiniz. Ayrıca, tanımlanan özellikler ve fiyatlandırma ile [tek bir veritabanı oluşturabilirsiniz](sql-database-get-started.md). Veya [esnek veritabanı havuzu oluşturarak](sql-database-elastic-pool-create-portal.md) birden çok veritabanını yönetebilirsiniz. Her iki durumda **Temel**, **Standart** ve **Premium** katmanları mevcuttur. Bu katmanlardaki veritabanı seçenekleri, tek veritabanları ve esnek havuzlar için benzer olup esnek havuzlar için dikkat edilmesi gereken bazı noktalar vardır. Bu makalede tek veritabanları ve esnek veritabanları için hizmet katmanlarına yönelik ayrıntılı bilgi verilmiştir.

## Hizmet katmanları ve veritabanı seçenekleri
Temel, Standart ve Premium hizmet katmanlarının tümü için %99,99 oranında çalışma süresi SLA'sı vardır. Hizmet katmanları; tahmin edilebilir performans, esnek iş sürekliliği seçenekleri, güvenlik özellikleri ve saatlik faturalandırma olanağı sunar. Aşağıdaki tabloda farklı uygulama iş yükleri için en uygun katman örnekleri verilmiştir.

| Hizmet katmanı | Hedef iş yükleri |
|---|---|
| **Temel** | Genellikle belirli bir zamanda tek bir işlemin etkin olmasını destekler ve küçük veritabanları için uygundur. Geliştirme veya test amaçlı kullanılan veritabanları ya da küçük ölçekli ve az sıklıkta kullanılan uygulamalar örnek olarak verilebilir. |
| **Standart** | Birden çok eşzamanlı sorguyu destekler ve çoğu bulut uygulaması için doğru tercihtir. Çalışma grupları ve web uygulamaları örnek olarak verilebilir. |
| **Premium** | Çok sayıda eşzamanlı kullanıcıyı desteklemesi ve en üst düzey iş sürekliliği işlevlerini gerekli kılmasıyla yüksek işlem hacimleri için tasarlanmıştır. İş açısından önemli uygulamaları destekleyen veritabanları örnek olarak verilebilir. |

>[AZURE.NOTE] Web ve İşletme sürümleri kullanımdan kaldırıldı. Web ve İşletme sürümlerini kullanmaya devam etmeyi planlıyorsanız lütfen[Sunset ile ilgili SSS](https://azure.microsoft.com/pricing/details/sql-database/web-business/) sayfasına göz atın.

## Tek veritabanı hizmet katmanları ve performans düzeyleri
Tek veritabanları için her bir hizmet katmanında birden çok performans düzeyi vardır. İş yükünüzün taleplerini en iyi karşılayan düzeyi seçme esnekliğine sahipsiniz. Ölçeği artırmanız veya azaltmanız gerekiyorsa **uygulamanız için kesintiye neden olmaksızın** veritabanınızın katmanlarını kolaylıkla değiştirebilirsiniz. Ayrıntılı bilgi için bkz. [Veritabanı Hizmet Katmanlarını ve Performans Düzeylerini Değiştirme](sql-database-scale-up.md).

Burada listelenen performans özellikleri [SQL Database V12](sql-database-v12-whats-new.md) kullanılarak oluşturulan veritabanları için geçerlidir. Azure'da temel alınan donanımın birden çok veritabanını barındırdığı durumlarda, veritabanınız yine de kesin olarak belirli bir kaynak kümesini alır ve veritabanınızdan beklenen performans özellikleri etkilenmez.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

DTU'lar hakkında daha fazla bilgi edinmek için bu konu başlığındaki [DTU bölümüne](#understanding-dtus) göz atın.

>[AZURE.NOTE] Bu hizmet katmanları tablosundaki diğer tüm satırların ayrıntılı bir açıklaması için bkz. [Hizmet katmanı özellikleri ve sınırları](sql-database-performance-guidance.md#service-tier-capabilities-and-limits).

## eDTU'lardaki esnek havuz hizmet katmanları ve performansı
Tek veritabanı oluşturmanın ve ölçeklendirmenin yanı sıra bir [esnek havuz](sql-database-elastic-pool.md) içinde birden çok veritabanını yönetme seçeneğine de sahipsiniz. Bir esnek havuzdaki tüm veritabanları ortak bir kaynak kümesini paylaşır. Performans özellikleri *esnek Veritabanı İşlem Birimleri* (eDTU'lar) tarafından ölçülür. Tek veritabanlarında olduğu gibi, havuzlar da üç hizmet katmanıyla sunulur: **Temel**, **Standart** ve **Premium**. Bu üç hizmet katmanı, havuzlar için genel performans sınırlarını ve çeşitli özellikleri tanımlar 

Havuzlar, esnek veritabanlarının havuzdaki veritabanlarına belirli bir performans düzeyi atanmasına gerek kalmadan DTU kaynaklarını paylaşmasına ve kullanmasına olanak sağlar. Örneğin, Standart havuzdaki tek veritabanı için eDTU kullanımı, 0 ila havuzu yapılandırdığınız sırada ayarladığınız maksimum veritabanı eDTU sayısı arasında değişiklik gösterebilir. Bu, çeşitli iş yüklerine sahip birden çok veritabanının tüm havuz için kullanılabilir eDTU kaynaklarını verimli bir şekilde kullanmasına olanak sağlar. Ayrıntılı bilgi için bkz. [Esnek havuzlar için fiyat ve performans ile ilgili dikkat edilmesi gerekenler](sql-database-elastic-pool-guidance.md).

Aşağıdaki tabloda havuz hizmet katmanlarının özellikleri açıklanmıştır.

[AZURE.INCLUDE [SQL DB service tiers table for elastic databases](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Bir havuz içindeki her bir veritabanı, aynı zamanda ilgili katmanın tek veritabanı özelliklerine de bağlı kalır. Örneğin, Temel havuzun havuz başına maksimum oturum sınırı 4800-28800 arasında değişir ancak aynı havuz içindeki tek veritabanı için oturum sınırı 300'dür (bir önceki bölümde Temel katmandaki tek veritabanı için belirtilen sınır).

## DTU'ları anlama

[AZURE.INCLUDE [SQL DB DTU description](../../includes/sql-database-understanding-dtus.md)]

## Sonraki adımlar
- Bu katmanlara ilişkin fiyatlandırmalar hakkında daha fazla bilgi için bkz. [SQL Database Fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).
- [Esnek veritabanı havuzları](sql-database-elastic-pool-guidance.md) ve [esnek veritabanı havuzlarına ilişkin fiyat ve performans ile ilgili dikkat edilmesi gerekenler](sql-database-elastic-pool-guidance.md) hakkında ayrıntılı bilgi edinin.
- [Esnek havuzlarını izleme, yönetme ve yeniden boyutlandırma](sql-database-elastic-pool-manage-portal.md) ve [Tek veritabanlarının performansını izleme](sql-database-single-database-monitor.md) işlemleri hakkında bilgi edinin.
- Artık SQL Database katmanları hakkında bilgi edindiğinize göre öğrendiklerinizi bir [ücretsiz hesap](https://azure.microsoft.com/pricing/free-trial/) kullanarak deneyin ve [ilk SQL veritabanınızı nasıl oluşturacağınızı](sql-database-get-started.md) öğrenin.



<!---HONumber=Jun16_HO2-->


