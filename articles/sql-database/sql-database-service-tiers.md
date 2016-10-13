<properties
    pageTitle="SQL Database performansı ve seçenekleri: Hizmet katmanları | Microsoft Azure"
    description="Maliyeti ve işlevleri, kendi ölçeklendirmenize göre dengelemek için SQL Database performansını ve hizmet katmanlarının iş sürekliliği özelliklerini karşılaştırın."
    keywords="veritabanı seçenekleri,veritabanı performansı"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="CarlRabeler"/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="08/10/2016"
    ms.author="carlrab"/>


# SQL Database seçenekleri ve performansı: Her hizmet katmanında nelerin kullanılabildiğini anlama

[Azure SQL Veritabanı](sql-database-technical-overview.md), farklı iş yüklerini işlemek üzere birden çok performans düzeyine sahip üç hizmet katmanı sunar. Her performans düzeyi, gittikçe artan bir işleme hacmi sağlamak üzere tasarlanmış bir şekilde bir önceki düzeye göre daha fazla sayıda kaynak sağlar. Her veritabanını, kendi [hizmet katmanı](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels) içinde, kendine ait performans düzeyinde yönetebilirsiniz. Birden çok veritabanını, paylaşılan bir grup kaynakla bir [esnek havuz](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) içinde de yönetebilirsiniz. Tek başına veritabanlarının kullanılabileceği kaynaklar, Veritabanı İşlem Birimleri (DTU'lar) ve esnek havuzlar için esnek DTU'lar, yani eDTU'lar cinsinden ifade edilir. DTU'lar ve eDTU'lar hakkında daha fazla bilgi edinmek için bkz. [DTU nedir?](sql-database-what-is-a-dtu.md). 

Her iki durumda da **Temel**, **Standart** ve **Premium** hizmet katmanları mevcuttur. Bu katmanlardaki veritabanı seçenekleri tek başına veritabanları ve esnek havuzlar için benzerlik gösterir ancak esnek havuzlar için dikkat edilmesi gereken bazı noktalar söz konusudur. Bu makalede, tek başına veritabanları ve esnek havuzlar için hizmet katmanlarına yönelik ayrıntılı bilgi verilmiştir.

## Hizmet katmanları ve veritabanı seçenekleri
Temel, Standart ve Premium hizmet katmanlarının tümü için %99,99 oranında çalışma süresi SLA'sı vardır. Hizmet katmanları; tahmin edilebilir performans, esnek iş sürekliliği seçenekleri, güvenlik özellikleri ve saatlik faturalandırma olanağı sunar. Aşağıdaki tabloda farklı uygulama iş yükleri için en uygun katman örnekleri verilmiştir.

| Hizmet katmanı | Hedef iş yükleri |
|---|---|
| **Temel** | Genellikle belirli bir zamanda tek bir işlemin etkin olmasını destekler ve küçük veritabanları için uygundur. Geliştirme veya test amaçlı kullanılan veritabanları ya da küçük ölçekli ve az sıklıkta kullanılan uygulamalar örnek olarak verilebilir. |
| **Standart** | Birden çok eşzamanlı sorguyu destekler ve çoğu bulut uygulaması için doğru tercihtir. Çalışma grupları ve web uygulamaları örnek olarak verilebilir. |
| **Premium** | Yüksek iş hacimleri için, çok sayıda eşzamanlı kullanıcıyı destekleyecek ve en üst düzey iş sürekliliği işlevlerini gerekli kılacak şekilde tasarlanmıştır. İş açısından önemli uygulamaları destekleyen veritabanları örnek olarak verilebilir. |

>[AZURE.NOTE] Web ve İşletme sürümleri kullanımdan kaldırıldı. Web ve İşletme sürümlerini kullanmaya devam etmeyi planlıyorsanız [Sunset ile ilgili SSS](https://azure.microsoft.com/pricing/details/sql-database/web-business/) sayfasına göz atın.

## Tek başına veritabanı hizmet katmanları ve performans düzeyleri
Tek başına veritabanları için her bir hizmet katmanında birden çok performans düzeyi bulunur. İş yükünüzün taleplerini en iyi karşılayan düzeyi seçme esnekliğine sahipsiniz. Ölçeği artırmanız veya azaltmanız gerekiyorsa veritabanınızın katmanlarını kolaylıkla değiştirebilirsiniz. Ayrıntılı bilgi için bkz. [Veritabanı Hizmet Katmanlarını ve Performans Düzeylerini Değiştirme](sql-database-scale-up.md).

Burada listelenen performans özellikleri [SQL Database V12](sql-database-v12-whats-new.md) kullanılarak oluşturulan veritabanları için geçerlidir. Barındırılan veritabanı sayısı göz önünde bulundurulmaksızın, veritabanınız kesin olarak belirlenmiş bir kaynak kümesini alır ve veritabanınızdan beklenen performans özellikleri etkilenmez.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

>[AZURE.NOTE] Bu hizmet katmanları tablosundaki diğer tüm satırların ayrıntılı bir açıklaması için bkz. [Hizmet katmanı özellikleri ve sınırları](sql-database-performance-guidance.md#service-tier-capabilities-and-limits).

## eDTU'lardaki esnek havuz hizmet katmanları ve performansı
Tek başına veritabanı oluşturmanın ve ölçeklendirmenin yanı sıra bir [esnek havuz](sql-database-elastic-pool.md) içinde birden çok veritabanını yönetme seçeneğine de sahipsiniz. Bir esnek havuzdaki tüm veritabanları, ortak bir kaynak kümesini paylaşır. Performans özellikleri *esnek Veritabanı İşlem Birimleri* (eDTU'lar) tarafından ölçülür. Tek başına veritabanlarında olduğu gibi, havuzlar da üç hizmet katmanıyla sunulur: **Temel**, **Standart** ve **Premium**. Bu üç hizmet katmanı, havuzlar için genel performans sınırlarını ve çeşitli özellikleri tanımlar 

Havuzlar, veritabanlarının havuzdaki her bir veritabanına belirli bir performans düzeyi atanmasına gerek kalmadan DTU kaynaklarını paylaşıp kullanmasına olanak sağlar. Örneğin, Standart havuzdaki tek başına veritabanı için eDTU kullanımı, 0 ile havuzu yapılandırdığınız sırada ayarladığınız maksimum veritabanı eDTU sayısı arasında değişiklik gösterebilir. Havuzlar, çeşitli iş yüklerine sahip birden çok veritabanının tüm havuz için kullanılabilir eDTU kaynaklarını verimli bir şekilde kullanmasına olanak sağlar. Ayrıntılı bilgi için bkz. [Esnek havuzlar için fiyat ve performans ile ilgili dikkat edilmesi gerekenler](sql-database-elastic-pool-guidance.md).

Aşağıdaki tabloda havuz hizmet katmanlarının özellikleri açıklanmıştır.

[AZURE.INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Bir havuz içindeki her bir veritabanı, aynı zamanda ilgili katmanın tek başına veritabanı özelliklerine de bağlı kalır. Örneğin, Temel havuzun havuz başına maksimum oturum sayısı 4800 ile 28.800 arasında değişir ancak Temel havuz içindeki tek veritabanı için veritabanı sınırı 300 oturumdur.

## Hizmet katmanı seçme

Hizmet katmanı seçimini yapmak için, veritabanının tek başına veritabanı mı yoksa bir esnek havuzun parçası mı olması gerektiğini belirleyerek işe başlayın. 

### Tek başına veritabanı için hizmet katmanı seçme

Tek başına veritabanına yönelik hizmet katmanı seçimini yapmak üzere, SQL Veritabanı sürümünü seçmek için gereken veritabanı özelliklerini belirleyerek işe başlayın:

- Veritabanı boyutu (performans düzeyine bağlı olarak Temel için maksimum 2 GB, Standart için maksimum 250 GB ve Premium için maksimum 500 GB ila 1 TB)
- Veritabanı yedeği saklama dönemi (Temel için 7 gün, Standart için 35 gün ve Premium için 35 gün)

SQL Database sürümünü belirledikten sonra veritabanının performans düzeyini belirlemeye hazır olursunuz (DTU sayısı). Tahminde bulunabilir ve sonra gerçek deneyime göre [dinamik olarak ölçek artırabilir veya azaltabilirsiniz](sql-database-scale-up.md). Gerekli DTU sayısını yaklaşık olarak belirlemek için [DTU Hesaplayıcı](http://dtucalculator.azurewebsites.net/)’yı da kullanabilirsiniz. 

### Esnek veritabanı havuzu için hizmet katmanı seçme.

Esnek veritabanı havuzu için hizmet katmanına karar vermek üzere, havuzunuzun hizmet katmanını seçmek için gereken veritabanı özelliklerini belirleyerek işe başlayın.

- Veritabanı boyutu (Temel için 2 GB, Standart için 250 GB ve Premium için 500 GB)
- Veritabanı yedeği saklama dönemi (Temel için 7 gün, Standart için 35 gün ve Premium için 35 gün)
- Havuz başına veritabanı sayısı (Temel için 400, Standart için 400 ve Premium için 50)
- Havuz başına en fazla depolama alanı (Temel için 117 GB, Standart için 1200 ve Premium için 750)

Havuzunuzun hizmet katmanını belirledikten sonra havuzun performans düzeyini belirlemeye (eDTU) hazır olursunuz. Tahminde bulunabilir ve sonra gerçek deneyime göre [dinamik olarak ölçek artırabilir veya ölçek azaltabilirsiniz](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool). Havuzdaki her bir veritabanı için gereken yaklaşık DTU sayısını belirlemek üzere, havuzun üst sınırını belirlemenize yardımcı olması için [DTU Hesaplayıcı](http://dtucalculator.azurewebsites.net/)'yı kullanabilirsiniz.

## Sonraki adımlar
- Bu katmanlara ilişkin fiyatlandırmalar hakkında daha fazla bilgi için bkz. [SQL Database Fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).
- [Esnek havuzlar](sql-database-elastic-pool-guidance.md) ve [esnek havuzlara ilişkin fiyat ve performans ile ilgili dikkat edilmesi gerekenler](sql-database-elastic-pool-guidance.md) hakkında ayrıntılı bilgi edinin.
- [Esnek havuzları izleme, yönetme ve yeniden boyutlandırma](sql-database-elastic-pool-manage-portal.md) ve [Tek başına veritabanlarının performansını izleme](sql-database-single-database-monitor.md) işlemleri hakkında bilgi edinin.
- Artık SQL Database katmanları hakkında bilgi edindiğinize göre öğrendiklerinizi bir [ücretsiz hesap](https://azure.microsoft.com/pricing/free-trial/) kullanarak deneyin ve [ilk SQL veritabanınızı nasıl oluşturacağınızı](sql-database-get-started.md) öğrenin.

## Ek kaynaklar

Çok kiracılı hizmet olarak yazılım (SaaS) veritabanı uygulamalarının ortak veri mimarisi düzenlerine ilişkin bilgi için bkz. [Azure SQL Database ile Çok Kiracılı SaaS Uygulamaları için Tasarım Düzenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md).



<!--HONumber=Oct16_HO1-->


