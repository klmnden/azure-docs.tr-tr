---
title: "SQL Veritabanı performansı: Hizmet katmanları | Microsoft Belgeleri"
description: "SQL Veritabanı hizmet katmanlarını karşılaştırın."
keywords: "veritabanı seçenekleri,veritabanı performansı"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: f5c5c596-cd1e-451f-92a7-b70d4916e974
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 01/11/2017
ms.author: carlrab; janeng
translationtype: Human Translation
ms.sourcegitcommit: 0a00aff343bfd31c956f6cbc831e89cc1cc84b23
ms.openlocfilehash: 95ae4bd67b7d08755035e7b5559ca9648d45bdaa


---
# <a name="sql-database-options-and-performance-understand-whats-available-in-each-service-tier"></a>SQL Database seçenekleri ve performansı: Her hizmet katmanında nelerin kullanılabildiğini anlama
[Azure SQL Veritabanı](sql-database-technical-overview.md), farklı iş yüklerini işlemek üzere birden çok performans düzeyine sahip üç hizmet katmanı (**Temel**, **Standart** ve **Premium**) sunar. Yüksek performans düzeyi, gittikçe artan bir işleme hacmi sağlamak üzere tasarlanmış bir şekilde bir önceki düzeye göre daha fazla sayıda kaynak sağlar. [Hizmet katmanlarını ve performans düzeylerini kapalı kalma süresi olmadan dinamik olarak](sql-database-scale-up.md) değiştirebilirsiniz. Temel, Standart ve Premium hizmet katmanlarının tümü için %99,99 oranında çalışma süresi SLA'sı vardır. Hizmet katmanları; esnek iş sürekliliği seçenekleri, güvenlik özellikleri ve saatlik faturalandırma olanağı sunar. 

Seçili [performans düzeyi](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) üzerinde ayrılmış kaynak ile tek veritabanları oluşturabilirsiniz. Ayrıca, kaynakların veritabanları arasında paylaşıldığı bir [elastik havuzda](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) birden çok veritabanını yönetebilirsiniz. Tek veritabanlarının kullanılabileceği kaynaklar, Veritabanı İşlem Birimleri (DTU'lar) ve elastik havuzlar için esnek DTU'lar (eDTU) cinsinden ifade edilir. DTU'lar ve eDTU'lar hakkında daha fazla bilgi edinmek için bkz. [DTU nedir?](sql-database-what-is-a-dtu.md). 

Her iki durumda da **Temel**, **Standart** ve **Premium** hizmet katmanları mevcuttur. 

## <a name="choosing-a-service-tier"></a>Hizmet katmanı seçme
Aşağıdaki tabloda farklı uygulama iş yükleri için en uygun katman örnekleri verilmiştir.

| Hizmet katmanı | Hedef iş yükleri |
| :--- | --- |
| **Temel** | Genellikle belirli bir zamanda tek bir işlemin etkin olmasını destekler ve küçük veritabanları için uygundur. Geliştirme veya test amaçlı kullanılan veritabanları ya da küçük ölçekli ve az sıklıkta kullanılan uygulamalar örnek olarak verilebilir. |
| **Standart** |Birden çok eşzamanlı sorguyu destekleyen ve G/Ç performansı gereksinimleri düşük veya orta olan bulut uygulamaları için en uygun seçenektir. Çalışma grupları ve web uygulamaları örnek olarak verilebilir. |
| **Premium** | Çok sayıda eşzamanlı kullanıcıyı destekleyen ve G/Ç performansı gereksinimleri yüksek olan, yüksek işlem hacimleri için tasarlanmıştır. İş açısından önemli uygulamaları destekleyen veritabanları örnek olarak verilebilir. |

Öncelikle, tek veritabanı çalıştırma ya da kaynakları paylaşan veritabanlarını gruplandırma seçeneklerinden hangisini kullanacağınıza karar verin. [Elastik havuz hakkında dikkat edilmesi gereken konuları](sql-database-elastic-pool-guidance.md) gözden geçirin. Bir hizmet katmanına karar vermek için, ihtiyacınızı olan en düşük veritabanı özelliklerini belirleyerek başlayın:

* Tek başına veritabanları için en büyük veritabanı boyutu (yüksek performans düzeylerinde Temel için maksimum 2 GB, Standart için maksimum 250 GB ve Premium için maksimum 500 GB ila 1 TB)
* Elastik havuz kullanıldığında toplam depolama alanı için üst sınır (Temel için 117 GB, Standart için 1200 GB ve Premium için 750 GB)
* Havuz başına en fazla veritabanı sayısı (Temel için 400, Standart için 400 ve Premium için 50)
* Veritabanı yedeği saklama dönemi (Temel için 7 gün, Standart ve Premium için 35 gün)

Minimum hizmet katmanını belirledikten sonra, veritabanının performans düzeyini belirlemeye (DTU sayısı) hazır olursunuz. Çoğu durumda standart S2 ve S3 performans düzeyleri iyi bir başlangıç noktasıdır. Yüksek CPU veya G/Ç gereksinimlerine sahip veritabanları için, Premium performans düzeyleri doğru başlangıç noktasıdır. Premium, daha fazla CPU sunar ve en yüksek Standart performans düzeyine kıyasla 10 kat daha fazla G/Ç ile başlar.

Başlangıçta bir performans düzeyi seçtikten sonra gerçek deneyime bağlı olarak, [elastik havuzunuzun](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) [tek başına veritabanının](sql-database-scale-up.md) ölçeğini artırabilir veya azaltabilirsiniz. Geçiş senaryoları için, gerekli DTU sayısını yaklaşık olarak belirlemenizi sağlayan [DTU Hesaplayıcıyı](http://dtucalculator.azurewebsites.net/) da kullanabilirsiniz. 

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

## <a name="single-database-service-tiers-and-performance-levels"></a>Tek veritabanı hizmet katmanları ve performans düzeyleri
Tek veritabanları için her bir hizmet katmanında birden çok performans düzeyi vardır. İş yükünüzün taleplerini en iyi karşılayan düzeyi seçme esnekliğine sahipsiniz. Ölçeği artırmanız veya azaltmanız gerekiyorsa veritabanınızın katmanlarını kolaylıkla değiştirebilirsiniz. Ayrıntılı bilgi için bkz. [Veritabanı Hizmet Katmanlarını ve Performans Düzeylerini Değiştirme](sql-database-scale-up.md).

Barındırılan veritabanı sayısı göz önünde bulundurulmaksızın, veritabanınız kesin olarak belirlenmiş bir kaynak kümesini alır ve veritabanınızdan beklenen performans özellikleri etkilenmez.

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

> [!NOTE]
> Bu hizmet katmanları tablosundaki diğer tüm satırların ayrıntılı bir açıklaması için bkz. [Hizmet katmanı özellikleri ve sınırları](sql-database-performance-guidance.md#service-tier-capabilities-and-limits).
> 

## <a name="elastic-pool-service-tiers-and-performance-in-edtus"></a>eDTU'lardaki esnek havuz hizmet katmanları ve performansı

Havuzlar, veritabanlarının havuzdaki her bir veritabanına belirli bir performans düzeyi atanmasına gerek kalmadan eDTU kaynaklarını paylaşıp kullanmasına olanak sağlar. Örneğin, Standart havuzdaki tek veritabanı için eDTU kullanımı, 0 ila havuzu yapılandırdığınız sırada ayarladığınız maksimum veritabanı eDTU sayısı arasında değişiklik gösterebilir. Havuzlar, çeşitli iş yüklerine sahip birden çok veritabanının tüm havuz için kullanılabilir eDTU kaynaklarını verimli bir şekilde kullanmasına olanak sağlar. Ayrıntılı bilgi için bkz. [Esnek havuzlar için fiyat ve performans ile ilgili dikkat edilmesi gerekenler](sql-database-elastic-pool-guidance.md).

Aşağıdaki tabloda havuz hizmet katmanlarının özellikleri açıklanmıştır.

[!INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-pools.md)]

Bir havuz içindeki her bir veritabanı, aynı zamanda ilgili katmanın tek veritabanı özelliklerine de bağlı kalır. Örneğin, Temel havuzun havuz başına maksimum oturum sayısı 4800 ile 28.800 arasında değişir ancak Temel havuz içindeki tek veritabanı için veritabanı sınırı 300 oturumdur.


## <a name="next-steps"></a>Sonraki adımlar

* [Esnek havuzlar](sql-database-elastic-pool-guidance.md) ve [esnek havuzlara ilişkin fiyat ve performans ile ilgili dikkat edilmesi gerekenler](sql-database-elastic-pool-guidance.md) hakkında ayrıntılı bilgi edinin.
* [Esnek havuzlarını izleme, yönetme ve yeniden boyutlandırma](sql-database-elastic-pool-manage-portal.md) ve [Tek veritabanlarının performansını izleme](sql-database-single-database-monitor.md) işlemleri hakkında bilgi edinin.
* Artık SQL Database katmanları hakkında bilgi edindiğinize göre öğrendiklerinizi bir [ücretsiz hesap](https://azure.microsoft.com/pricing/free-trial/) kullanarak deneyin ve [ilk SQL veritabanınızı nasıl oluşturacağınızı](sql-database-get-started.md) öğrenin.




<!--HONumber=Jan17_HO2-->


