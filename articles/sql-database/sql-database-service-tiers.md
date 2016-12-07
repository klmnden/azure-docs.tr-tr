---
title: "SQL Veritabanı performansı: Hizmet katmanları | Microsoft Belgeleri"
description: "SQL Veritabanı hizmet katmanlarını karşılaştırın."
keywords: "veritabanı seçenekleri,veritabanı performansı"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: CarlRabeler
ms.assetid: f5c5c596-cd1e-451f-92a7-b70d4916e974
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 11/15/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: e8bb9e5a02a7caf95dae0101c720abac1c2deff3
ms.openlocfilehash: 7bbdbe345bd468c01e2a790610bcf6c063c11f9b


---
# <a name="sql-database-service-tiers-for-single-databases-and-elastic-database-pools"></a>Tekli veritabanları ve esnek veritabanı havuzları için SQL Veritabanı hizmet katmanları
[Azure SQL Veritabanı](sql-database-technical-overview.md), farklı iş yüklerini işlemek üzere birden çok performans düzeyine sahip üç hizmet katmanı sunar. Yüksek performans düzeyi, gittikçe artan bir işleme hacmi sağlamak üzere tasarlanmış bir şekilde bir önceki düzeye göre daha fazla sayıda kaynak sağlar. Hizmet katmanlarını ve performans düzeylerini dinamik olarak değiştirebilirsiniz. Ayrıntılı bilgi için bkz. [Veritabanı Hizmet Katmanlarını ve Performans Düzeylerini Değiştirme](sql-database-scale-up.md).

Her veritabanını, kendi [hizmet katmanı](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) içinde, kendine ait performans düzeyinde yönetebilirsiniz. Birden çok veritabanını, paylaşılan bir grup kaynakla bir [esnek veritabanı havuzu](sql-database-service-tiers.md#elastic-database-pool-service-tiers-and-performance-in-edtus) içinde de yönetebilirsiniz. Tek veritabanlarının kullanılabileceği kaynaklar, Veritabanı İşlem Birimleri (DTU'lar) ve esnek veritabanı havuzları için esnek DTU'lar, yani eDTU'lar cinsinden ifade edilir. DTU'lar ve eDTU'lar hakkında daha fazla bilgi edinmek için bkz. [DTU nedir?](sql-database-what-is-a-dtu.md). 

Her iki durumda da **Temel**, **Standart** ve **Premium** hizmet katmanları mevcuttur. 

## <a name="service-tiers"></a>Hizmet katmanları
Temel, Standart ve Premium hizmet katmanlarının tümü için %99,99 oranında çalışma süresi SLA'sı vardır. Hizmet katmanları; tahmin edilebilir performans, esnek iş sürekliliği seçenekleri, güvenlik özellikleri ve saatlik faturalandırma olanağı sunar. Aşağıdaki tabloda farklı uygulama iş yükleri için en uygun katman örnekleri verilmiştir.

| Hizmet katmanı | Hedef iş yükleri |
| :--- | --- |
| **Temel** |Genellikle belirli bir zamanda tek bir işlemin etkin olmasını destekler ve küçük veritabanları için uygundur. Geliştirme veya test amaçlı kullanılan veritabanları ya da küçük ölçekli ve az sıklıkta kullanılan uygulamalar örnek olarak verilebilir. |
| **Standart** |Birden çok eşzamanlı sorguyu destekler ve çoğu bulut uygulaması için doğru tercihtir. Çalışma grupları ve web uygulamaları örnek olarak verilebilir. |
| **Premium** |Yüksek iş hacimleri için, çok sayıda eşzamanlı kullanıcıyı destekleyecek ve en üst düzey iş sürekliliği işlevlerini gerekli kılacak şekilde tasarlanmıştır. İş açısından önemli uygulamaları destekleyen veritabanları örnek olarak verilebilir. |

> [!NOTE]
> Web ve İşletme sürümleri kullanımdan kaldırıldı. Web ve İşletme sürümlerini kullanmaya devam etmeyi planlıyorsanız [Sunset ile ilgili SSS](https://azure.microsoft.com/pricing/details/sql-database/web-business/) sayfasına göz atın.
> 
> 

## <a name="single-database-service-tiers-and-performance-levels"></a>Tek veritabanı hizmet katmanları ve performans düzeyleri
Tek veritabanları için her bir hizmet katmanında birden çok performans düzeyi vardır. İş yükünüzün taleplerini en iyi karşılayan düzeyi seçme esnekliğine sahipsiniz. Ölçeği artırmanız veya azaltmanız gerekiyorsa veritabanınızın performans düzeyini hızlı bir şekilde değiştirebilirsiniz. Ayrıntılı bilgi için bkz. [Veritabanı Hizmet Katmanlarını ve Performans Düzeylerini Değiştirme](sql-database-scale-up.md).

Burada listelenen performans özellikleri [SQL Database V12](sql-database-technical-overview.md) kullanılarak oluşturulan veritabanları için geçerlidir. Barındırılan veritabanı sayısı göz önünde bulundurulmaksızın, veritabanınız kesin olarak belirlenmiş bir kaynak kümesini alır ve veritabanınızdan beklenen performans özellikleri etkilenmez.

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

> [!NOTE]
> Bu hizmet katmanları tablosundaki diğer tüm satırların ayrıntılı bir açıklaması için bkz. [Hizmet katmanı özellikleri ve sınırları](sql-database-performance-guidance.md#service-tier-capabilities-and-limits).
> 
> 

## <a name="elastic-database-pool-service-tiers-and-performance-in-edtus"></a>eDTU'lardaki esnek veritabanı havuzu hizmet katmanları ve performansı
Ayrıca [esnek veritabanı havuzu](sql-database-elastic-pool.md) içinde birden çok veritabanını yönetebilirsiniz. Bir esnek veritabanı havuzundaki tüm veritabanları, ortak bir kaynak kümesini paylaşır. Performans özellikleri *esnek Veritabanı İşlem Birimleri* (eDTU'lar) tarafından ölçülür. Tek veritabanlarında olduğu gibi, havuzlar da üç hizmet katmanıyla sunulur: **Temel**, **Standart** ve **Premium**. Bu üç hizmet katmanı, havuzlar için genel performans sınırlarını ve çeşitli özellikleri tanımlar 

Havuzlar, veritabanlarının havuzdaki her bir veritabanına belirli bir performans düzeyi atanmasına gerek kalmadan DTU kaynaklarını paylaşıp kullanmasına olanak sağlar. Örneğin, Standart havuzdaki bir veritabanı için eDTU kullanımı, 0 ila havuzu yapılandırdığınız sırada ayarladığınız maksimum veritabanı eDTU sayısı arasında değişiklik gösterebilir. Havuzlar, çeşitli iş yüklerine sahip birden çok veritabanının tüm havuz için kullanılabilir eDTU kaynaklarını verimli bir şekilde kullanmasına olanak sağlar. Ayrıntılar için bkz. [Esnek veritabanı havuzuna ilişkin fiyat ve performans ile ilgili dikkat edilmesi gerekenler](sql-database-elastic-pool-guidance.md).

Aşağıdaki tabloda Temel, Standart ve Premium esnek veritabanı havuzlarının özellikleri açıklanmaktadır.

[!INCLUDE [SQL DB service tiers table for elastic database pools](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Bir havuz içindeki her bir veritabanı, aynı zamanda ilgili katmanın tek veritabanı özelliklerine de bağlı kalır. Örneğin, Temel havuzun havuz başına maksimum oturum sayısı 4800 ile 28.800 arasında değişir ancak Temel havuz içindeki tek veritabanı için veritabanı sınırı 300 oturumdur.

## <a name="choosing-a-service-tier"></a>Hizmet katmanı seçme
Hizmet katmanı seçimini yapmak için, veritabanının tek veritabanı mı yoksa bir esnek veritabanı havuzunun parçası mı olması gerektiğini belirleyerek işe başlayın. 

### <a name="choosing-a-service-tier-for-a-single-database"></a>Tek veritabanı için hizmet katmanı seçme
Tek veritabanına yönelik hizmet katmanı seçimini yapmak üzere, SQL Veritabanı sürümünü seçmek için gereken veritabanı özelliklerini belirleyerek işe başlayın:

* Veritabanı boyutu (performans düzeyine bağlı olarak Temel için maksimum 2 GB, Standart için maksimum 250 GB ve Premium için maksimum 500 GB ila 1 TB)
* Veritabanı yedeği saklama dönemi (Temel için 7 gün, Standart için 35 gün ve Premium için 35 gün)

SQL Database sürümünü belirledikten sonra veritabanının performans düzeyini belirlemeye hazır olursunuz (DTU sayısı). Tahminde bulunabilir ve sonra gerçek deneyime göre [dinamik olarak ölçek artırabilir veya azaltabilirsiniz](sql-database-scale-up.md). Gerekli DTU sayısını yaklaşık olarak belirlemek için [DTU Hesaplayıcı](http://dtucalculator.azurewebsites.net/)’yı da kullanabilirsiniz. 

### <a name="choosing-a-service-tier-for-an-elastic-database-pool"></a>Esnek veritabanı havuzu için hizmet katmanı seçme.
Esnek veritabanı havuzu için hizmet katmanına karar vermek üzere, havuzunuzun hizmet katmanını seçmek için gereken veritabanı özelliklerini belirleyerek işe başlayın.

* Veritabanı boyutu (Temel için 2 GB, Standart için 250 GB ve Premium için 500 GB)
* Veritabanı yedeği saklama dönemi (Temel için 7 gün, Standart için 35 gün ve Premium için 35 gün)
* Havuz başına veritabanı sayısı (Temel için 400, Standart için 400 ve Premium için 50)
* Havuz başına en fazla depolama alanı (Temel için 117 GB, Standart için 1200 ve Premium için 750)

Havuzunuzun hizmet katmanını belirledikten sonra havuzun performans düzeyini belirlemeye (eDTU) hazır olursunuz. Tahminde bulunabilir ve sonra gerçek deneyime göre [dinamik olarak ölçek artırabilir veya ölçek azaltabilirsiniz](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool). Havuzdaki her bir veritabanı için gereken yaklaşık DTU sayısını belirlemek üzere, havuzun üst sınırını belirlemenize yardımcı olması için [DTU Hesaplayıcı](http://dtucalculator.azurewebsites.net/)'yı kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* Bu katmanlara ilişkin fiyatlandırmalar hakkında daha fazla bilgi için bkz. [SQL Database Fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).
* [Esnek veritabanı havuzları](sql-database-elastic-pool-guidance.md) ve [esnek veritabanı havuzlarına ilişkin fiyat ve performans ile ilgili dikkat edilmesi gerekenler](sql-database-elastic-pool-guidance.md) hakkında ayrıntılı bilgi edinin.
* [Esnek veritabanı havuzlarını izleme, yönetme ve yeniden boyutlandırma](sql-database-elastic-pool-manage-portal.md) ve [Tek veritabanlarının performansını izleme](sql-database-single-database-monitor.md) işlemleri hakkında bilgi edinin.
* Artık SQL Database katmanları hakkında bilgi edindiğinize göre öğrendiklerinizi bir [ücretsiz hesap](https://azure.microsoft.com/pricing/free-trial/) kullanarak deneyin ve [ilk SQL veritabanınızı nasıl oluşturacağınızı](sql-database-get-started.md) öğrenin.

## <a name="additional-resources"></a>Ek kaynaklar
* [Azure SQL veritabanını kullanarak çok kiracılı SaaS uygulamaları için düzenler tasarlama](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Azure SQL Veritabanı’ndaki esnek veritabanı özellikleriyle ilgili Microsoft Virtual Academy video dersi](https://mva.microsoft.com/en-US/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)




<!--HONumber=Nov16_HO4-->


