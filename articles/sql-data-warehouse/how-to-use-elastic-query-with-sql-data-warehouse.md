---
title: "Azure SQL veri ambarı ile esnek sorgu kavramları | Microsoft Docs"
description: "Azure SQL Data Warehouse esnek sorgu kavramları"
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: 
ms.assetid: e2dc8f3f-10e3-4589-a4e2-50c67dfcf67f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 09/18/2017
ms.author: elbutter
ms.openlocfilehash: 295cc59fdb23105534b4e7431902eaa720643330
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-elastic-query-with-sql-data-warehouse"></a>SQL veri ambarı ile esnek sorgu kullanma



Azure SQL veri ambarı ile esnek sorgu dış tablolara kullanımı ile Azure SQL Data Warehouse örneğine uzaktan gönderilen bir SQL veritabanında Transact-SQL yazmanızı sağlar. Bu özelliği kullanarak, maliyet tasarrufu ve senaryoya bağlı olarak daha fazla kullanıcı mimarileri sağlar.

Bu özellik iki birincil senaryolarına olanak sağlar:

1. Etki alanı yalıtımı
2. Uzaktan sorgu yürütme

### <a name="domain-isolation"></a>Etki alanı yalıtımı

Etki alanı yalıtımı Klasik veri reyonu senaryosuna başvuruyor. Bazı senaryolarda, bir veri mantıksal bir etki alanı için çeşitli nedenlerle de dahil olmak üzere veri ambarını geri kalanındaki yalıtılmış ancak bunlarla sınırlı olmamak kaydıyla aşağı akış kullanıcılara sağlamak isteyebilirsiniz:

1. Kaynak ayırma - SQL veritabanı eşzamanlı kullanıcı hizmet veren biraz farklı iş yükleri için veri ambarı ayrılmış büyük analitik sorguları daha büyük bir taban hizmet vermek için optimize edilmiştir. Sağ iş yükleri sağ araçları tarafından sunulan yalıtım sağlar.
2. Güvenlik yalıtımı - yetkili veri alt belirli şemaları aracılığıyla seçmeli olarak ayırmak için.
3. Korumalı alan - örnek bir veri kümesi "üretim sorguları vb. keşfetmek için bir playground" sağlayın.

Esnek sorgu kolayca SQL veri ambarı veri alt kümesi seçin ve bir SQL veritabanı örneğine taşıma olanağı sağlayabilir. Ayrıca bu yalıtım ayrıca daha ilginç "önbellek" senaryoları için izin verme uzaktan sorgu yürütme etkinleştirme özelliği engellemek değil.

### <a name="remote-query-execution"></a>Uzaktan sorgu yürütme

SQL veri ambarı örneği üzerinde uzaktan sorgu yürütme için esnek sorgu sağlar. Bir SQL database ve SQL veri ambarı en iyi sıcak ve soğuk verilerinizi iki veritabanı arasında ayırarak kullanabilirler. Kullanıcıların, raporları ve ortalama iş kullanıcıları çok sayıda kullanılabileceği bir SQL veritabanı içinde daha yeni veri kullanmaya devam edebilir. Ancak, daha fazla veri veya hesaplama gerekli olduğunda, bir kullanıcı, çok daha hızlı ve daha verimli bir şekilde büyük ölçekli toplamalar burada işlenebilen bir SQL veri ambarı örneği için sorgunun parçası boşaltabilir.



## <a name="elastic-query-overview"></a>Esnek sorgu genel bakış

Esnek bir sorgu, SQL veri içinde bulunan veri ambarı SQL veritabanı örnekleri kullanılabilir hale getirmek için kullanılabilir. Esnek sorgu tablolar uzak bir SQL veri ambarı örneği ile bir SQL veritabanı sorgularından bakın sağlar. 

İlk adım, SQL veri ambarı içinde varolan kullanıcı kimlik bilgilerini kullanan SQL veri ambarı örneği başvurduğu bir dış veri kaynağı tanımını oluşturmaktır. Hiçbir değişiklik üzerinde uzak SQL veri ambarı örneği gereklidir. 

> [!IMPORTANT] 
> 
> ALTER ANY dış veri KAYNAĞINA iznine sahip olması gerekir. Bu izin ALTER DATABASE izniyle dahil edilir. Uzak Veri kaynağına başvurmak için ALTER ANY dış veri kaynağı izinleri gereklidir.

Sonraki uzak dış tablo tanımındaki bir uzak tablo SQL veri ambarı işaret eden bir SQL veritabanı örneğinde oluşturuyoruz. Bir dış tablo kullanan bir sorgu kullandığınızda, dış tabloya başvuran sorgu bölümünü işlenmek üzere SQL veri ambarı örneği için gönderilir. Sorgu tamamlandıktan sonra sonuç kümesi arama SQL veritabanı örneğine gönderilir. SQL veritabanı ile SQL veri ambarı arasında esnek bir sorgu ayarlama kısa öğretici için bkz [yapılandırma esnek sorgu SQL veri ambarı ile][Configure Elastic Query with SQL Data Warehouse].

SQL Database esnek sorgu hakkında daha fazla bilgi için bkz: [Azure SQL Database esnek sorgu genel bakış][Azure SQL Database elastic query overview].



## <a name="best-practices"></a>En iyi uygulamalar

### <a name="general"></a>Genel

- Uzaktan sorgu yürütme kullanırken, yalnızca gerekli sütunları seçme ve sağ filtre uygulayarak emin olun. Yalnızca olmadığından bu artış işlem gerekli, ancak daha da artırır sonuç kümesi boyutu ve bu nedenle veri miktarı, iki örnekleri arasında taşınmaları gerekir.
- Kümelenmiş columnstore analytiIcal performans için SQL veri ambarı ve SQL veritabanı analitik amaçlarla verileri korur.
- Kaynak tabloları sorgu ve veri taşıma için bölümlenir emin olun.
- Daha ayrıntılı güncelleştirmeleri ve daha kolay Yönetimi etkinleştirmek için bir önbellek olarak kullanılan SQL veritabanı örneği bölümlenmiş emin olun. 
- İdeal olarak kümelenmiş columnstore Premium veritabanlarındaki indirimle g/ç yoğun iş yükleri üzerinde odaklanılan dizin analitik avantajları sağladığından PremiumRS veritabanları kullanın.
- Yükleri sonra Yük veya geçerlilik tarihi Kimlik sütunları için önbellek kaynağı bütünlüğünü sağlamak için SQL veritabanı durumlarda upserts kullanın. 
- Ayrı oturum açma ve kullanıcı, SQL veri ambarı örneği için dış veri kaynağında tanımlanan SQL veritabanı uzak oturum açma kimlik bilgilerinizi oluşturun. 

### <a name="elastic-querying"></a>Esnek sorgulama

- SQL veritabanı örneği ile farklı nesneler olarak interally önbelleğe alınmış tablo ve dış tablo yok. Bir görünümü tablo ve dış tablo önbelleğe alınmış bölümünü üst hangi birleşimler oluşturma hem tabloları ve filtreleri her tablo sınır noktasında geçerlidir göz önünde bulundurun.

  SQL veritabanı örneğinde veri en son yıl tutmak isteriz düşünün. İki tablo sahibiz **dahili Siparişleri**, veri ambarı başvuran tablolar, sıralar ve **dbo. Siparişleri** en son SQL veritabanı örneğinde veri yıl eşitleyeceğini temsil eder. Bir tablo ya da başka bir sorgu karar vermek için kullanıcıların isteyen yerine bir görünüm bölüm noktasında en son yılın her iki tablonun üst üzerinden oluşturuyoruz.

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

  Bir görünüm üretilen bir şekilde sorgu derleyici şimdi belirlemek kullanıcılar sorgunuzu yanıtlamak için veri ambarı örneği kullanmak gerekip gerekmediğini. 

  Yoktur gönderme, derleme, çalıştıran ve her esnek sorgu veri ambarı örneğiyle ilişkili veri taşıma işlemlerinin ek yükü. Esnek her sorgu, eşzamanlılık yuvaları karşı sayar ve kaynak kullanan emin olun.  


- Bir detaya planlıyorsa veri ambarı örneği sonuç kümesinden içine daha fazla, SQL veritabanı performans ve gereksiz kaynak kullanımı önlemek için geçici tablosunda gerçekleştirilmesini göz önünde bulundurun.

### <a name="moving-data"></a>Veri taşıma 

- Mümkünse, güncelleştirmelerin veri ambarı ve veritabanı örnekleri arasında kolayca sürdürülebilir şekilde veri yönetimi salt sonuna kaynak tabloları ile daha kolay tutun.
- Veri ambarı düzeyi ve veritabanı örneği güncel tutmak için taşınabilir veri miktarını sorgu maliyetini en aza indirmek için temizleme ve dolgu semantiği ile bölüm düzeyinde verileri taşıyın. 

### <a name="when-to-choose-azure-analysis-services-vs-sql-database"></a>Ne zaman Azure Analysis Services vs SQL veritabanı seçin

#### <a name="azure-analysis-services"></a>Azure Analysis Services

- Çok sayıda küçük sorguları gönderen bir BI aracıyla önbelleğiniz kullanmayı planladığınız
- Sorgu gecikmesi subsecond
- Yönetme/modelleri için analiz hizmetlerini geliştirmeye deneyiminiz 

#### <a name="sql-database"></a>SQL Veritabanı

- Önbellek verilerinizle SQL sorgulamak istediğiniz
- Bazı sorgular için uzaktan yürütme gerekir
- Daha büyük önbellek gereksinimleri vardır



## <a name="faq"></a>SSS

S: veritabanları esnek sorgu sahip bir esnek veritabanı havuzu içinde kullanabilirim?

Y: Evet. Esnek havuz içindeki SQL veritabanları, esnek sorgu kullanabilirsiniz. 

S: esnek sorgu için kullanabileceğim kaç veritabanları için bir sınır var mı?

Y: mantıksal sunucu DTU sınırları yanlışlıkla overspending müşterilerin önlemek yerinde sahip. Bir SQL Data Warehouse örneğine yanında esnek sorgu için birden fazla veritabanı etkinleştiriyorsanız ucun beklenmedik bir şekilde isabet. Bu gerçekleşirse, mantıksal sunucunuz DTU sınırını artırmak için bir istek gönderin. Tarafından kotayı artırabilir [bir destek bileti oluşturma](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-create-support-ticket) ve seçerek *kota* istek türü

S: satır düzeyi güvenlik/dinamik veri kullanabilirim esnek sorguyla maskeleme?

A: SQL veritabanı ile daha gelişmiş güvenlik özellikleri kullanmak istediğiniz müşteriler ilk taşıyarak ve verileri SQL veritabanında depolamak bunu yapabilirsiniz. Şu anda satır düzeyi güvenlik veya DDM dış tablolara sorgulanan verilere uygulayamazsınız. 

S: veri ambarı örneği için my SQL veritabanı örneğinden yazma?

A: şu anda bu özellik desteklenmiyor. Ziyaret bizim [görüş sayfası] [ Feedback page] oluşturma/bu işlevselliği için bu gelecekte görmek istediğiniz bir özellik ise oy için. 

S: geometri/geography gibi uzamsal türlerini kullanmak?

A: varbinary(max) değerleri olarak SQL veri ambarı'nda, uzamsal türler saklayabilirsiniz. Esnek sorgu kullanarak bu sütunları sorguladığınızda, çalışma zamanında uygun türlerine dönüştürebilirsiniz.

![uzamsal türler](./media/sql-data-warehouse-elastic-query-with-sql-database/geometry-types.png)





<!--Image references-->

<!--Article references-->

[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop/
[Configure Elastic Query with SQL Data Warehouse]: ./tutorial-elastic-query-with-sql-datababase-and-sql-data-warehouse.md
[Feedback Page]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Azure SQL Database elastic query overview]: ../sql-database/sql-database-elastic-query-overview.md

<!--MSDN references-->

<!--Other Web references-->