---
title: Elastik sorgu - erişim verileri Azure SQL veritabanından Azure SQL veri ambarı | Microsoft Docs
description: Azure SQL veritabanından Azure SQL veri ambarı'nda verilere erişme elastik sorgu kullanarak en iyi yöntemleri öğrenin.
services: sql-data-warehouse
author: hirokib
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/11/2018
ms.author: elbutter
ms.reviewer: igorstan
ms.openlocfilehash: 344cb1bed56b0b6af7bd3704f8674ae30695f885
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42061090"
---
# <a name="best-practices-for-using-elastic-query-in-azure-sql-database-to-access-data-in-azure-sql-data-warehouse"></a>Azure SQL veri ambarı'nda verilere erişmek, Azure SQL veritabanı'nda esnek sorgu kullanmak için en iyi uygulamalar
Azure SQL veritabanından Azure SQL veri ambarı'nda verilere erişme elastik sorgu kullanarak en iyi yöntemleri öğrenin. 

## <a name="what-is-an-elastic-query"></a>Elastik sorgu nedir?
Elastik sorgu uzaktan bir Azure SQL veri ambarı'na gönderilen bir Azure SQL veritabanında bir sorgu yazmak için T-SQL ve dış tablolar kullanmanıza olanak tanır. Bu özelliği kullanarak, maliyet tasarrufu ve senaryoya bağlı olarak daha fazla yüksek performanslı mimarisine sağlar.

Bu özellik, iki ana senaryo sağlar:

1. Etki alanı yalıtımı
2. Uzaktan sorgu yürütme

### <a name="domain-isolation"></a>Etki alanı yalıtımı

Etki alanı yalıtımı Klasik veri reyonu senaryoya ifade eder. Bazı senaryolarda, bir veri ambarı, çeşitli nedenlerle de dahil olmak üzere geri kalanındaki yalıtılmış ancak bunlarla sınırlı olmamak üzere aşağı akış kullanıcılara veri mantıksal bir etki alanı sağlamak isteyebilirsiniz:

1. Kaynak yalıtımı - SQL veritabanı, eşzamanlı kullanıcıya hizmet veren biraz farklı iş yükleri için veri ambarı ayrılmış büyük analitik sorguları daha büyük bir taban hizmet vermek için optimize edilmiştir. Doğru iş yüklerini doğru araçları tarafından sunulan yalıtım sağlar.
2. Güvenlik yalıtımı - bir yetkili veri alt kümesi seçerek, belirli şemaları aracılığıyla ayırın.
3. Korumalı alana alma - "üretim sorguları vb. keşfetmek için bir oyun alanı" bir örnek veri kümesini sağlar.

Esnek sorgu kolayca SQL veri ambarı veri kümelerine seçin ve bir SQL veritabanı örneğine taşıma olanağı sağlar. Ayrıca bu yalıtım ayrıca daha ilgi çekici "önbellek" senaryoları için izin verme uzaktan sorgu yürütme sağlama yeteneği kullanımını değil.

### <a name="remote-query-execution"></a>Uzaktan sorgu yürütme

SQL veri ambarı örneği üzerinde uzaktan sorgu yürütme için esnek sorgu sağlar. Sıcak ve soğuk verilerinizi iki veritabanı arasında ayırarak bir SQL veritabanı hem de SQL veri ambarı en iyi şekilde kullanabilir. Kullanıcılar, raporları ve ortalama iş kullanıcıları çok sayıda hizmet verebilen bir SQL veritabanı, daha yeni verileri tutabilirsiniz. Ancak, daha fazla veri veya hesaplama gerektiğinde, bir kullanıcı, büyük ölçekli toplamalar çok daha hızlı ve daha verimli bir şekilde burada işlenebilecek bir SQL veri ambarı örneği için bir sorgunun parçası boşaltabilirsiniz.

## <a name="elastic-query-process"></a>Elastik sorgu işlemi
Elastik sorgu, bir SQL veri içinde bulunan veri ambarı SQL veritabanı örnekleri kullanılabilir hale getirmek için kullanılabilir. Esnek sorgu, uzak bir SQL veri ambarı örneği tablolarında bir SQL veritabanı sorgularından başvurmak sağlar. 

İlk adım, var olan kullanıcı kimlik bilgilerini kullanan SQL data warehouse'da SQL veri ambarı örneği başvuran bir dış veri kaynağı tanımını oluşturmaktır. Uzak SQL veri ambarı örneği üzerinde değişiklik gereklidir. 

> [!IMPORTANT] 
> 
> ALTER ANY dış veri kaynağı iznine sahip olması gerekir. Bu izne ALTER DATABASE izni dahil edilir. Uzak Veri kaynağına başvurmak için ALTER ANY dış veri kaynağı izinleri gereklidir.

Ardından, uzak bir SQL veri ambarı tablosunu işaret bir SQL veritabanı örneğinde bir uzak dış tablo tanımı oluşturun. Bir sorgu bir dış tablo kullandığında, dış tabloya başvuran sorgu bölümünü işlenecek SQL veri ambarı örneğine gönderilir. Sorgu tamamlandıktan sonra sonuç kümesi çağıran SQL veritabanı örneğine gönderilir. SQL veritabanı ve SQL veri ambarı arasında elastik sorgu ayarlama kısa öğretici için bkz [SQL veri ambarı ile esnek sorgu yapılandırma][Configure Elastic Query with SQL Data Warehouse].

SQL veritabanı esnek sorgu üzerinde daha fazla bilgi için bkz. [Azure SQL veritabanı esnek sorgu genel bakış][Azure SQL Database elastic query overview].

## <a name="best-practices"></a>En iyi uygulamalar
Esnek sorgu etkili bir şekilde kullanmak için bu en iyi uygulamaları kullanın.

### <a name="general"></a>Genel

- Uzaktan sorgu yürütme kullanırken, yalnızca gerekli sütunların seçilmesi ve doğru filtreler uygulayarak emin olun. Yalnızca mu gerekli işlem bu artış, ancak daha da artırır sonuç kümesinin boyutunu ve bu nedenle, veri miktarı, iki örnekleri arasında taşınması gereken.
- Kümelenmiş columnstore analytiIcal performans için hem SQL veritabanı, hem de SQL veri ambarı analitik amaçlar için verileri korur.
- Kaynak tablolarına sorgu ve veri taşıma işlemi için bölümlenir emin olun.
- Daha ayrıntılı güncelleştirmeleri ve daha kolay yönetimini etkinleştirmek için bir önbellek olarak kullanılan SQL veritabanı örnekleri bölümlenir emin olun. 
- İdeal olarak bunlar g/ç yoğunluklu iş yüklerini indirimli fiyatla Premium veritabanlarında odaklanan dizin kümelenmiş columnstore analitik avantajlarından sağladıklarından PremiumRS veritabanlarını kullanır.
- Yükleri sonra Yük veya geçerlilik tarihi Kimlik sütunları upsert eder önbellek kaynağı bütünlüğünü korumak için SQL veritabanı örneğinde için kullanır. 
- SQL veri ambarı örneğinizi dış veri kaynağında tanımlanan SQL veritabanı uzaktan oturum açma kimlik bilgilerinizi için ayrı bir oturum açma ve kullanıcı oluşturun. 

### <a name="elastic-querying"></a>Elastik sorgulama

- Çoğu durumda, bir bir tür tablonuzu bir kısmını SQL veritabanı içinde önbelleğe alınan verileri SQL veri ambarı'nda depolanan verileri geri kalanı ile performansa yönelik olarak olduğu esnetilen tablonun yönetmek isteyebilirsiniz. SQL veritabanı'nda iki nesne gerekir: SQL veritabanı, SQL veri ambarı ve SQL veritabanı içinde tablo "önbelleğe alınan" bölümü içindeki temel tablo başvuruları içinde bir dış tablo. Bir görünümü üst tablo ve dış tablo önbelleğe alınmış kısmının hangi birleşimler oluşturma hem tablolar ve dış tablolar sunulan SQL veritabanı ve SQL veri ambarı veri içinde gerçekleştirilmiş veri ayrı filtreler uygulayan göz önünde bulundurun.

  Bir SQL veritabanı örneğinde veri en son yıl saklamak istediğinizi düşünün. **Dahili Siparişler** tablo başvuruları, veri ambarı tabloları sıralar. **Dbo. Siparişler** en son yıl değerinde SQL veritabanı örneğinde veri temsil eder. Bir tablo veya diğer sorgu karar vermek için kullanıcıların isteyen yerine, her iki tablonun en son yılın bölüm noktasında üst üzerinden bir görünüm oluşturun.

  ```sql
  CREATE VIEW dbo.Orders_Elastic AS
  SELECT 
    [col_a]     
  , [col_b]         
  , ...
  , [col_n]
  FROM
    [dbo].[Orders]
  WHERE 
    YEAR([o_orderdate]) >= '<Most Recent Year>'
  UNION
  SELECT 
    [col_a]     
  , [col_b]         
  , ...
  , [col_n]         
  FROM
    [ext].[Orders]
  WHERE
    YEAR([o_orderdate]) < '<Most Recent Year>'
  ```

  Bir görünüm şekilde üretilen sorgu derleyici şimdi belirlemek kullanıcılar sorgunuzu yanıtlamak için veri ambarı örneği kullanmak gerekip gerekmediğini. 

  Var olan gönderme, derleme, çalıştırma ve veri ambarı örneği karşı esnek her sorgu ilişkili veri taşıma işlemlerinin ek yükü. Her esnek sorgu, eşzamanlılık yuvaları karşı sayar ve kaynak kullanan emin olun.  


- Detaya gitmek bir plan veri ambarı örneği sonuç kümesi içine daha fazla, SQL veritabanı performans için ve gereksiz kaynak kullanımını önlemek için geçici bir tablodaki düzeniyle göz önünde bulundurun.

### <a name="moving-data"></a>Veri taşıma 

- Mümkünse, güncelleştirmelerin veri ambarı ve veritabanı örnekleri arasında kolayca rahat, veri yönetimi salt kaynak tabloları ile daha kolay tutun.
- Bölüm düzeyinde ile veri taşıma boşaltmaya ve veriler üzerinde sorgu maliyeti en aza indirmek için dolgu semantiği ambarı düzeyi ve veritabanı örneği güncel tutmak için taşınan veri miktarı. 

### <a name="when-to-choose-azure-analysis-services-vs-sql-database"></a>Azure Analysis Services ve SQL veritabanı ne zaman

Azure kullanan Analiz Hizmetleri:

- Çok sayıda küçük sorgular gönderen bir BI aracına önbelleğinizi kullanmayı planlıyorsanız
- Sorgu gecikmesi subsecond
- Yönetme/Analysis Services'e yönelik geliştirme modelleri deneyiminiz 

Azure SQL kullanmak ne zaman veritabanı:

- SQL önbellek verilerinizle sorgulamak istediğiniz
- Bazı sorgular için uzaktan yürütme gerekir
- Daha büyük bir önbellek gereksinimleri vardır

## <a name="faq"></a>SSS

S: veritabanlarını elastik sorgu içeren bir elastik havuzun içine kullanabilirim?

C: Evet. SQL veritabanları, elastik bir havuzdaki elastik sorgu kullanabilirsiniz. 

S: kaç veritabanları elastik sorgu için kullanabileceğim bir sınır var mı?

Y: yoktur hiçbir sabit sınır kaç veritabanları için esnek sorgu kullanılabilir üzerinde. Ancak, her bir elastik sorgu (SQL veri ambarı isabet sorgular) normal Eş zamanlılık limitlerine doğru olarak sayılır.

S: DTU sınırları esnek sorgu ile ilgili var mı?

Y: DTU sınırları olmayan farklı ile esnek sorgu uygulanmaz. Müşteriler, yanlışlıkla uyarabilirsiniz önlemek için bir yerde sahip olacak mantıksal sunucuları DTU sınırları standart ilkesidir. Bir SQL Data Warehouse örneğine yanı sıra esnek sorgu için birden fazla veritabanı etkinleştiriyorsanız cap beklenmedik bir şekilde karşılaşabilirsiniz. Bu meydana gelirse, mantıksal sunucunuzdaki DTU sınırını artırmak için bir istek gönderin. Tarafından kotanızı artırabilirsiniz [bir destek bileti oluşturma](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-create-support-ticket) seçerek *kota* olarak istek türü

Satır düzeyi güvenlik/dinamik veri kullanın s: ile esnek sorgu maskeleme?

Y: SQL veritabanı ile daha gelişmiş güvenlik özellikleri kullanmak isteyen müşteriler, ilk taşıyarak ve verileri SQL veritabanı'nda depolayarak bunu yapabilirsiniz. Dış tablolar sorgulanan veriler üzerinde şu anda satır düzeyi güvenlik veya DDM uygulanamıyor. 

Veri ambarı örneği için SQL veritabanı Örneğim yazma miyim?

Y: şu anda bu özellik desteklenmiyor. Ziyaret bizim [geri bildirim sayfası] [ Feedback page] oluşturma/bu işlevselliği için bir özellik gelecekte görmek istediğiniz buysa oy için. 

Geometri/Coğrafya gibi uzamsal türlerini kullanabilirim miyim?

VARBINARY(max) değerleri olarak SQL veri ambarı'nda c: uzamsal türlerini depolayabilirsiniz. Elastik sorgu kullanarak bu sütunları sorguladığınızda, çalışma zamanında uygun türlerine dönüştürme yapabilirsiniz.

![uzamsal türler](./media/sql-data-warehouse-elastic-query-with-sql-database/geometry-types.png)

<!--Article references-->

[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[Configure Elastic Query with SQL Data Warehouse]: tutorial-elastic-query-with-sql-datababase-and-sql-data-warehouse.md
[Feedback Page]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Azure SQL Database elastic query overview]: ../sql-database/sql-database-elastic-query-overview.md


