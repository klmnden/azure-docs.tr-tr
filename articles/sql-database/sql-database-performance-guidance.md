---
title: Azure SQL veritabanı performans ayarlama Kılavuzu | Microsoft Docs
description: Azure SQL veritabanı sorgu performansı artırmak için önerileri kullanma hakkında bilgi edinin.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: carlrab
ms.openlocfilehash: 791fb4ba2f16a48441d489a888360912afa8fa11
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "45983235"
---
# <a name="tuning-performance-in-azure-sql-database"></a>Azure SQL veritabanı'nda performans ayarlama

Azure SQL veritabanı tarafından sağlanan [önerileri](sql-database-advisor.md) veritabanınızın performansını geliştirmek için kullanabilir veya Azure SQL veritabanı sağlayabilirsiniz [uygulamanızı otomatik olarak uyum](sql-database-automatic-tuning.md) ve değişiklikleri uygulayın, İş yükünüzün performansını geliştirir.

Size uygun önerisi yok ve performans sorunlarını çözümlenmedi, performans iyileştirmek için aşağıdaki yöntemleri kullanabilirsiniz:
- Hizmet katmanlarında artırmak, [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) veya [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md) veritabanınıza daha fazla kaynak sağlamak için.
- Uygulamanızı ayarlamak ve performansı artırmak bazı en iyi yöntemler uygulayın. 
- Dizinleri değiştirerek veritabanını ayarlama ve sorguları daha verimli bir şekilde verileri ile çalışma.

İhtiyaçlarınızı karşılamak kaynakları miktarını karar vermeniz gerekir çünkü bunlar el ile yöntemleridir. Aksi takdirde, uygulamanın veya veritabanı kodu yeniden yazın ve değişiklikleri dağıtmak gerekir.

## <a name="increasing-servicce-tier-of-your-database"></a>Veritabanınızın artan servicce katmanı

Azure SQL veritabanı iki satın alma modeli sunar bir [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md) arasından seçim. Her hizmet katmanı, SQL veritabanınızı kullanabilirsiniz ve bu hizmet katmanı için tahmin edilebilir performans garantileri kaynakları kesinlikle yalıtır. Bu makalede, uygulamanız için Hizmet katmanını seçmek yardımcı olacak rehberlik sunuyoruz. Ayrıca Azure SQL veritabanı en iyi şekilde yararlanmak için uygulamanızı ayarlama yolları ele alır.

> [!NOTE]
> Bu makalede, Azure SQL veritabanı'nda tek veritabanları için performans yönergeleri odaklanır. Elastik havuzlar için ilgili performans rehberi için bkz [elastik havuzlar için fiyat ve performans konuları](sql-database-elastic-pool-guidance.md). Ancak, bu makaledeki ayarlama önerilerin çoğu bir elastik havuzdaki veritabanları için geçerlidir ve benzer performans avantajı unutmayın.
> 

* **Temel**: temel hizmet katmanı sunar iyi bir performans öngörülebilirliğini her veritabanı için saat saat içinde. Temel bir veritabanında yeterli kaynakları birden çok eş zamanlı istek sahip olmayan küçük bir veritabanı içinde iyi bir performans desteklemez. Temel hizmet katmanındaki kullanırsınız, tipik kullanım durumları verilmiştir:
  * **Yalnızca Azure SQL veritabanı ile başlangıç**. Genellikle geliştirme aşamasında olan uygulamaların yüksek-boyutları işlem yoksa. Veritabanı geliştirme veya sınama, düşük fiyat noktasında için ideal bir ortam temel veritabanlarıdır.
  * **Tek bir kullanıcıyla bir veritabanına sahip**. Tek bir kullanıcı, genellikle bir veritabanı ile ilişkilendiren yüksek tutarlılık ve performans gereksinimlerini uygulamanız yok. Bu uygulamalar, temel hizmet katmanı için adaylardır.
* **Standart**: Standart hizmet katmanı sunar Gelişmiş performans öngörülebilirliğini ve çalışma grubu ve web uygulamaları gibi birden çok eş zamanlı istek sahip veritabanları için iyi bir performans sağlar. Standart hizmet katmanındaki veritabanına seçtiğinizde, veritabanı uygulamanızın öngörülebilir performans, boyut dakika dakika üzerinden.
  * **Birden çok eş zamanlı istek veritabanınızı sahip**. Genellikle aynı anda birden fazla kullanıcı hizmet uygulamaları, daha yüksek bilgi işlem boyutlarına gerekir. Örneğin, birden çok eşzamanlı sorguyu destekleyen Orta g/ç trafiği gereksinimleri için en düşük olan çalışma grubu veya web standart hizmet katmanı için iyi adaylar uygulamalardır.
* **Premium**: Premium Hizmet katmanını tahmin edilebilir performans sağlar ikinci olarak, her bir Premium veya iş açısından kritik veritabanı için ikinci üzerinden. Premium Hizmet katmanını seçtiğinizde, bu veritabanı için en yüksek yük veritabanı uygulamanızı yeniden boyutlandırabilirsiniz. Plan hangi performans farkı küçük sorgular, gecikmeye duyarlı işlemlerinde beklenenden daha uzun sürmesine neden olabilir durumlarda kaldırır. Bu model, en yüksek kaynak gereksinimlerine, performans farkı veya sorgunun gecikme süresi hakkında güçlü ifadelerini yapmaya gerek duyan uygulamalar için geliştirme ve ürün doğrulama döngüsü büyük ölçüde basitleştirebilir. Çoğu Premium Hizmet katmanını kullanım örnekleri, bir veya daha fazla şu özelliklere sahiptir:
  * **Yüksek yük düzeylerine ulaşan yük**. Önemli miktarda CPU, bellek veya giriş/çıkış (GÇ) işlemlerini tamamlamak için gerektiren bir uygulama bir özel, yüksek işlem boyutu gerektirir. Örneğin, birden fazla CPU çekirdeği uzun bir süre için kullanılacağı bilinen bir veritabanı işlemi Premium hizmet katmanı için bir adaydır.
  * **Birçok eşzamanlı isteği**. Yüksek trafik hacmine hizmet veren bir Web sitesi sahip olduğunda bazı veritabanı uygulamaları birçok eşzamanlı isteği, örneğin, hizmet. Temel ve standart hizmet katmanları, veritabanı başına eşzamanlı istek sayısını sınırlar. Daha fazla bağlantı gerektiren uygulamalar sayısı gerekli istekleri işlemek için bir uygun rezervasyon boyutu seçmek gerekir.
  * **Düşük gecikme süresi**. Bazı uygulamalar mümkün olduğunca az zamanda veritabanından bir yanıt garanti gerekir. Belirli bir saklı yordam daha geniş bir müşteri işleminin bir parçası çağrılırsa, bu çağrı geri döndürme en fazla 20 milisaniye cinsinden süre yüzde 99'olan bir gereksinimi olabilir. Bu tür bir uygulama, gerekli bilgi işlem gücüne kullanılabilir olduğundan emin olmak için Premium hizmet katmanından, fayda sağlar.

SQL veritabanınız için gereken hizmet katmanı, her kaynak boyutu için en yüksek yük gereksinimlerine bağlıdır. Bazı uygulamalar, basit bir tek bir kaynak miktarını kullanır, ancak diğer kaynaklar için önemli gereksinimleri vardır.

### <a name="service-tier-capabilities-and-limits"></a>Hizmet katmanı özellikleri ve sınırları

Yalnızca gereksinim duyduğunuz kapasitesi için ödeme esnekliğine sahip her hizmet katmanında işlem boyutunu ayarlayın. Yapabilecekleriniz [kapasite ayarlama](sql-database-service-tiers-dtu.md), yukarı veya aşağı, iş yükü değiştikçe. Örneğin, arka Okul alışveriş sezonu sırasında veritabanı iş yükünüzü yüksekse, belirlenen süre Temmuz-Eylül veritabanı için işlem boyutu artırabilir. En yüksek dönemi sona erdiğinde azaltabilir. Bulut ortamınıza işletmenizin mevsimsellik iyileştirerek ödeme en aza indirebilirsiniz. Bu model, iyi yazılım ürün yayın döngüleri için de çalışır. Test takımı, test çalıştırmaları ve test tamamladığınızda bu kapasiteye serbest bırakmak sırada kapasite ayırma. Gereksinim ve nadiren kullanıyor olabileceğiniz ayrılmış kaynaklarda harcama kaçının bir kapasite istek modeli kapasitesi için ödeme.

### <a name="why-service-tiers"></a>Neden katmanları hizmet?
Her veritabanı iş yükünüzü değişebilir olsa da, amacı hizmet katmanları, çeşitli işlem boyutlarda performans öngörülebilirliğini sağlamaktır. Büyük ölçekli veritabanı kaynak gereksinimleri olan müşterilerin daha ayrılmış bir bilgi işlem ortamda çalışabilir.

## <a name="tune-your-application"></a>Uygulamanızı ayarlamak
Geleneksel şirket içi SQL Server ilk kapasite planlama işlemi, üretim ortamında bir uygulama çalışan işlemi genellikle ayrılır. Donanım ve ürün lisansları ilk satın alınır ve performans ayarı daha sonra gerçekleştirilir. Azure SQL veritabanı kullandığınızda, uygulama çalıştırma ve bu ayarlama işleminin interweave iyi bir fikirdir. İsteğe bağlı kapasite için ödeme modeliyle, uygulamanızı genellikle hatalı bir uygulama için gelecekteki büyüme planlarını, tahmin üzerinde temel donanım üzerinde açıdan yerine artık, gerekli en düşük kaynak kullanacak şekilde ayarlayabilirsiniz. Bazı müşteriler ayarlama olmayan bir uygulama seçin ve bunun yerine donanım kaynakları fazladan tercih. Meşgul bir dönem için bir anahtar uygulamasını değiştirmek istemiyorsanız, bu yaklaşım bir fikir olabilir. Ancak, Azure SQL veritabanı'nda hizmet katmanlarını kullandığınızda bir uygulama ayarlamayı kaynak gereksinimlerini ve daha düşük bir aylık fatura en aza indirebilirsiniz.

### <a name="application-characteristics"></a>Uygulama özellikleri
Azure SQL veritabanı hizmet katmanları, performans, kararlılık ve öngörülebilirlik için uygulama geliştirmek için tasarlanmış olmasına rağmen bazı en iyi uygulamaları daha iyi kaynakları bir işlem boyutta yararlanmak için uygulamanızı ayarlamak yardımcı olabilir. Birçok uygulama önemli olsa yüksek bir geçiş powershell'inizi yazarak performans kazancı elde edildi işlem boyutu veya hizmet katmanını, bazı uygulamalar daha yüksek bir düzeyde hizmet yararlanmak için ek ayar gerekir. Performansı artırmak için bu özelliklere sahip uygulamalar için ek bir uygulama ayarlamayı göz önünde bulundurun:

* **"Geveze" davranışı nedeniyle yavaş performans sorunu olan uygulamalar**. Sık iletişim kuran uygulamaları için ağ gecikmesini duyarlıdır aşırı veri erişim işlemleri yapın. Bu tür bir SQL veritabanına veri erişim işlemlerinin sayısını azaltmak için uygulamaları değiştirmeniz gerekebilir. Örneğin, toplu işleme geçici sorgular veya saklı yordamlar için sorgular taşıma gibi teknikler kullanılarak uygulama performansını iyileştirebilir. Daha fazla bilgi için [toplu iş sorguları](#batch-queries).
* **Tüm tek bir makine tarafından desteklenen bir kullanımı yoğun iş yükü veritabanlarıyla**. Aşan en yüksek Premium kaynaklarını veritabanları bilgi işlem boyutu iş yükünü ölçeklendirme alanından yararlanabilir. Daha fazla bilgi için [veritabanları arası parçalama](#cross-database-sharding) ve [işlevsel bölümleme](#functional-partitioning).
* **Yetersiz sorguları uygulamaları**. Uygulamalar, özellikle de kötü performans gösteren sorguları düzenlediniz veri erişim katmanında, daha yüksek bir işlem boyutundan avantajlı olmayabilir. Bu, WHERE yan tümcesi eksik, dizinlerin eksik olması veya istatistikleri eski sorguları içerir. Bu uygulamalar, standart sorgu performansını ayarlama teknikleri yarar. Daha fazla bilgi için [eksik dizinleri](#identifying-and-adding-missing-indexes) ve [sorgu ayarlama ve ipucu](#query-tuning-and-hinting).
* **Yetersiz veri uygulamalara erişim tasarım**. Örneğin kilitlenmenin, iç veri erişim eşzamanlılık sorunları olan uygulamalar daha yüksek bir işlem boyutundan avantajlı olmayabilir. Azure önbellek hizmeti veya başka bir önbelleğe alma teknolojisini istemci tarafında önbelleğe alma verilerinizi Azure SQL veritabanında gidiş dönüş azaltmayı göz önünde bulundurun. Bkz: [uygulama katmanı önbelleğe alma](#application-tier-caching).

## <a name="tune-your-database"></a>Veritabanınızı ayarlayın
Bu bölümde, uygulamanız için en iyi performans elde edin ve en düşük olası işlem boyutu çalıştırmak için Azure SQL veritabanı ayarlamak için kullanabileceğiniz bazı teknikleri atacağız. Aşağıdaki tekniklerden bazılarını geleneksel SQL Server en iyi ayarlama eşleşen, ancak diğerleri Azure SQL veritabanı'na özgüdür. Bazı durumlarda, tüketilen kaynaklar için ayarlama ve Azure SQL veritabanı'nda çalışmak için geleneksel SQL Server teknikleri genişletmek için alanlar bulmak bir veritabanı inceleyebilirsiniz.

### <a name="identify-performance-issues-using-azure-portal"></a>Azure portalını kullanarak performans sorunlarını belirleme
Azure portalında aşağıdaki araçlar, SQL veritabanı ile performans sorunlarını çözün ve çözümlemenize yardımcı olabilir:

* [Sorgu Performansı İçgörüleri](sql-database-query-performance.md)
* [SQL Veritabanı Danışmanı](sql-database-advisor.md)

Azure portalda hem de bu araçları ve bunların nasıl kullanılacağı hakkında daha fazla bilgi bulunur. Verimli bir şekilde tanılayın ve sorunlarını düzeltmek için ilk Azure portalında araçları deneyebilirsiniz öneririz. Ardından, için eksik dizinleri ve sorgu, özel durumlarda ayarlama ele yaklaşımları ayarlama el ile kullanmanızı öneririz.

Üzerinde Azure SQL veritabanı'ndaki sorunları tanımlama hakkında daha fazla bilgi [performansı Azure portalında izleme](sql-database-monitor-tune-overview.md) ve [Dmv'leri kullanarak veritabanlarını izleme](sql-database-monitoring-with-dmvs.md) makaleler.

### <a name="identifying-and-adding-missing-indexes"></a>Tanımlama ve eksik dizinleri ekleme
Ortak bir sorunu OLTP veritabanı performans için fiziksel veritabanı tasarımı ilişkilendirir. Genellikle veritabanı şemalarını tasarlanmış ve uygun ölçekte (veya yük veri hacmindeki) sınamadan birlikte gönderilir. Ne yazık ki, bir sorgu planı performansını bir küçük ölçekte kabul edilebilir ancak üretim düzeyinde veri birimlerini önemli ölçüde düşürebilir. En yaygın bu sorunu filtreleri ya da diğer kısıtlamalar sorguda karşılamak için uygun dizinleri eksikliği kaynağıdır. Genellikle, bir dizin arama yeterli, tablo olarak eksik dizinleri bildirimleri tarayın.

Bir arama yeterli, bu örnekte, seçilen sorgu planı tarama kullanır:

    DROP TABLE dbo.missingindex;
    CREATE TABLE dbo.missingindex (col1 INT IDENTITY PRIMARY KEY, col2 INT);
    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO dbo.missingindex(col2) VALUES (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION;
    GO
    SELECT m1.col1
    FROM dbo.missingindex m1 INNER JOIN dbo.missingindex m2 ON(m1.col1=m2.col1)
    WHERE m1.col2 = 4;

![Eksik dizinleri içeren bir sorgu planı](./media/sql-database-performance-guidance/query_plan_missing_indexes.png)

Azure SQL veritabanı, bulma ve düzeltme genel koşullar dizin eksik yardımcı olabilir. Azure SQL veritabanı'na yerleşik Dmv'leri içinde dizin bir sorguyu çalıştırmak için tahmini maliyetini önemli ölçüde azaltır sorgu derlemesi sırasında arayın. Sorgu yürütme işlemi sırasında SQL veritabanı, ne sıklıkta her bir sorgu planı yürütülür ve burada bu dizinin var tahmini boşluk imagined bir yürütme sorgu planı arasında izler izler. Bu Dmv'leri fiziksel veritabanı tasarımınız için hangi değişikliklerin bir veritabanı ve onun gerçek iş yükü için Genel İş Yükü Maliyeti iyileştirebilir hızlı bir şekilde tahmin etmek için kullanabilirsiniz.

Olası eksik dizinleri değerlendirmek için bu sorguyu kullanabilirsiniz:

    SELECT CONVERT (varchar, getdate(), 126) AS runtime,
        mig.index_group_handle, mid.index_handle,
        CONVERT (decimal (28,1), migs.avg_total_user_cost * migs.avg_user_impact *
                (migs.user_seeks + migs.user_scans)) AS improvement_measure,
        'CREATE INDEX missing_index_' + CONVERT (varchar, mig.index_group_handle) + '_' +
                  CONVERT (varchar, mid.index_handle) + ' ON ' + mid.statement + '
                  (' + ISNULL (mid.equality_columns,'')
                  + CASE WHEN mid.equality_columns IS NOT NULL
                              AND mid.inequality_columns IS NOT NULL
                         THEN ',' ELSE '' END + ISNULL (mid.inequality_columns, '')
                  + ')'
                  + ISNULL (' INCLUDE (' + mid.included_columns + ')', '') AS create_index_statement,
        migs.*,
        mid.database_id,
        mid.[object_id]
    FROM sys.dm_db_missing_index_groups AS mig
    INNER JOIN sys.dm_db_missing_index_group_stats AS migs
        ON migs.group_handle = mig.index_group_handle
    INNER JOIN sys.dm_db_missing_index_details AS mid
        ON mig.index_handle = mid.index_handle
    ORDER BY migs.avg_total_user_cost * migs.avg_user_impact * (migs.user_seeks + migs.user_scans) DESC

Bu örnekte sorgu, bu önerisine sonuçlandı:

    CREATE INDEX missing_index_5006_5005 ON [dbo].[missingindex] ([col2])  

Oluşturulduktan sonra aynı SELECT deyimi bir arama yerine bir tarama kullanır ve ardından plan daha verimli bir şekilde yürütür başka bir plana seçer:

![Düzeltilmiş dizinleri içeren bir sorgu planı](./media/sql-database-performance-guidance/query_plan_corrected_indexes.png)

Önemli bir içgörü paylaşılan, bir ticari sistem GÇ kapasitesini daha, adanmış bir sunucunuz sınırlıdır. Azure SQL veritabanı hizmet katmanları her işlem boyutunun DTU, sistemin en fazla yararlanmak için gereksiz g/ç en aza indirme premium yoktur. Uygun fiziksel veritabanı tasarımı seçimleri her sorgu, gecikme sürelerini önemli ölçüde artırabilir eş zamanlı isteklerin ölçek birimi işlenen verimini artırmak ve sorgu karşılamak adına gerekli maliyetleri en aza indirmek. Eksik dizini Dmv'leri hakkında daha fazla bilgi için bkz. [sys.dm_db_missing_index_details](https://msdn.microsoft.com/library/ms345434.aspx).

### <a name="query-tuning-and-hinting"></a>Sorguyu ayarlamayı ve ipuçları
Azure SQL veritabanında sorgu iyileştiricisi, geleneksel SQL Server sorgu iyileştiricisi için benzerdir. Sorguları ayarlama ve mantık anlamak için en iyi yöntemlerin çoğu sorgu iyileştiricisi modeli sınırlamaları Azure SQL veritabanı için de geçerlidir. Azure SQL veritabanı sorgularında ayarlamak, toplam kaynak taleplerini azaltmak ek bir avantaja alabilirsiniz. Uygulamanızı karıncalı gösteren eşdeğer daha düşük bir maliyetle daha düşük bir işlem boyutta çalıştırılabildiği çalıştırmak mümkün olabilir.

SQL Server'da ortak ve Azure SQL veritabanı'na da geçerli bir örnek nasıl sorgu iyileştiricisi "olduğunu gösteriyorsa" parametreleri. Derleme sırasında daha iyi bir sorgu planı oluşturmak olup olmadığını belirlemek için bir parametrenin geçerli değeri sorgu iyileştiricisi değerlendirir. Bu strateji genellikle bilinen parametre değerleri derlenmiş bir plan kıyasla önemli ölçüde daha hızlı bir sorgu planı açabilir ancak şu anda imperfectly hem çalıştığını SQL Server ve Azure SQL veritabanı'nda. Bazen parametresi sızılmasını değil ve bazen parametresi sızılmasını ancak oluşturulan plan bir iş yükü parametre değerlerini tam kümesi için yetersiz. Microsoft hedefi daha bilinçli olarak belirtin ve parametre algılaması varsayılan davranışını geçersiz sorgu ipuçları (yönergeleri) içerir. Genellikle, ipuçları kullanırsanız, varsayılan SQL Server veya Azure SQL veritabanı davranışı için belirli bir müşteri iş yükü kusurlu olduğu durumlarda düzeltebilirsiniz.

Sonraki örnek, hem performans ve kaynak gereksinimleri için yetersiz bir plan Sorgu işlemcisi nasıl oluşturabileceğiniz gösterilmektedir. Bu örnek ayrıca sorgu ipucu kullanıyorsanız, SQL veritabanınızın sorgu çalıştırma zaman ve kaynak gereksinimlerini azaltmak gösterir:

    DROP TABLE psptest1;
    CREATE TABLE psptest1(col1 int primary key identity, col2 int, col3 binary(200));

    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO psptest1(col2) values (1);
        INSERT INTO psptest1(col2) values (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION
    CREATE INDEX i1 on psptest1(col2);
    GO

    CREATE PROCEDURE psp1 (@param1 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1
        WHERE col2 = @param1
        ORDER BY col2;
    END
    GO

    CREATE PROCEDURE psp2 (@param2 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1 WHERE col2 = @param2
        ORDER BY col2
        OPTION (OPTIMIZE FOR (@param2 UNKNOWN))
    END
    GO

    CREATE TABLE t1 (col1 int primary key, col2 int, col3 binary(200));
    GO

Kurulum kodu, veri dağıtımı dengesiz bir tablo oluşturur. En iyi sorgu planı hangi parametre seçili göre farklılık gösterir. Ne yazık ki, önbelleğe alma davranışını planı her zaman en yaygın parametre değerine göre sorgu derlemeniz değil. Bu nedenle, önbelleğe alınmış ve hatta farklı bir plan daha iyi bir plan seçenek ortalama olabilir, birçok değer için kullanılan yetersiz bir plan için mümkündür. Ardından sorgu planı, bir özel sorgu ipucu varsa hariç, aynıdır, iki saklı yordam oluşturur.

**Örneğin, 1. Bölüm**

    -- Prime Procedure Cache with scan plan
    EXEC psp1 @param1=1;
    TRUNCATE TABLE t1;

    -- Iterate multiple times to show the performance difference
    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp1 @param1=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

**Örneğin, 2. Bölüm**

(Sonuçları elde edilen telemetri verileri farklı olacak şekilde, örneğin, bölüm 2 başlamadan önce en az 10 dakika beklemenizi öneririz.)

    EXEC psp2 @param2=1;
    TRUNCATE TABLE t1;

    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp2 @param2=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

Bu örnekte her parçası (sınama veri kümesi olarak kullanmak için yeterli yük oluşturmak için) bir parametreli INSERT deyimini 1.000 kez çalıştırmak çalışır. Saklı yordamlar yürütüldüğünde, Sorgu işlemcisi (parametre "algılaması"), derleme sırasında yordamına geçirilen parametre değeri inceler. İşlemci, sonuçta elde edilen planı önbelleğe alır ve parametre değeri farklı olsa bile sonraki çağrılar için kullanır. Her durumda en iyi planı kullanılabilir değil. Bazen iyileştirici olduğunda sorguyu önce derlenen gelen özel durumu yerine ortalama çalışması için daha iyi bir plan seçmek için kılavuz gerekir. Bu örnekte, ilk planı parametreyle eşleşen her bir değeri bulmak için tüm satırları okuyan bir "tarama" planı oluşturur:

![Ayarı taraması planı kullanarak sorgulama](./media/sql-database-performance-guidance/query_tuning_1.png)

Biz yordamı ' % s'değeri 1'i kullanarak yürütülen olduğundan, sonuçta elde edilen plan 1 değeri için en iyi ancak diğer tablodaki tüm değerleri için yetersiz. Plan daha yavaş çalışır ve daha fazla kaynak kullandığı için her plan rastgele olarak çekmek için olsaydı istiyor büyük olasılıkla sonucu değildir.

Test çalıştırmasıyla çalıştırırsanız `SET STATISTICS IO` kümesine `ON`, mantıksal tarama işi bu örnekte, arka planda gerçekleştirilir. (Bu yalnızca bir satır döndürülecek ortalama durum söz konusuysa verimsiz bir durumdur) planına göre yapılır 1,148 okuma olduğunu görebilirsiniz:

![Sorgu mantıksal bir tarama kullanarak ayarlama](./media/sql-database-performance-guidance/query_tuning_2.png)

Örnek ikinci bölümü, belirli bir değer derleme işlemi sırasında kullanılacak iyileştirici bildirmek için bir sorgu ipucu kullanır. Bu durumda, parametre olarak geçirilen değer yok saymak için Sorgu işlemcisi zorlar ve bunun yerine varsaymak `UNKNOWN`. Bu, ortalama sıklığını (eğriltme yoksayar) tablosunda olan bir değere başvurur. Sonuçta elde edilen planı, daha hızlı ve daha az kaynak bölümü bu örnek 1 ortalama olarak daha planı kullanılıyorsa bir arama tabanlı bir plandır:

![Bir sorgu ipucunu kullanarak sorguyu ayarlama](./media/sql-database-performance-guidance/query_tuning_3.png)

Aslında gördüğünüz **sys.resource_stats** tablo (test ve ne zaman veri tablosunu dolduran yürütme zamanından bir gecikme olur). Bu örnek, 22:25: 00'zaman penceresi sırasında yürütülen bölüm 1 ve 2. Bölüm 22:35: 00'da yürütülür. Önceki bir zaman penceresi (nedeniyle, verimlilik geliştirmelerini planı) daha yeni bir pencere daha fazla kaynak kullanılır.

    SELECT TOP 1000 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![Sorgu örnek sonuçlar ayarlama](./media/sql-database-performance-guidance/query_tuning_4.png)

> [!NOTE]
> Bu örnekte birim kasıtlı olarak küçük olsa da, yetersiz parametreler etkisini daha büyük veritabanları üzerinde özellikle önemli olabilir. Aşırı durumlarda fark hızlı çalışmaları saniye ile yavaş çalışmalarının saat arasında olabilir.
> 
> 

İnceleyebilirsiniz **sys.resource_stats** kaynak bir test için başka bir test daha az veya daha fazla kaynak kullanıp kullanmadığını belirlemek için. Veri karşılaştırdığınızda, bunlar aynı 5 dakikalık penceresinde böylece test zamanlama ayrı **sys.resource_stats** görünümü. Kullanılan kaynakları toplam miktarı en aza indirmek ve yoğun kaynak değil en aza indirmek alıştırmalar hedefidir. Genellikle, gecikme süresi için kod parçasını en iyi duruma getirme Ayrıca kaynak tüketimini azaltır. Bir uygulamaya yaptığınız değişiklikler gereklidir ve değişiklikleri sorgu ipuçları uygulamada kullanıyor olabilecek birisi için müşteri deneyimini olumsuz yönde etkilemediğinden emin olun.

Bir iş yükü sorguları yinelenen bir dizi varsa, genellikle yakalamak ve veritabanını barındırmak için gereken en düşük kaynak boyutu birim sürücüleri için en iyi hale getirme planı Seçimlerinizden doğrulamak için mantıklıdır. Bazen, doğruladıktan sonra bunlar değil düşürülmüş olduğunu emin olmanıza yardımcı olmak için planlar yeniden gözden geçirin. Daha fazla bilgi edinebilirsiniz [sorgu ipuçları (Transact-SQL)](https://msdn.microsoft.com/library/ms181714.aspx).

### <a name="cross-database-sharding"></a>Çapraz veritabanı parçalama
Azure SQL veritabanı ticari donanımlarda çalıştığından, tek bir veritabanı için kapasite sınırları geleneksel şirket içi SQL Server yüklemesi için daha düşük. Bazı müşteriler işlemleri tek bir veritabanının Azure SQL veritabanı'nda sınırları içinde uygun olmayan zaman birden çok veritabanı üzerinde veritabanı işlemleri yaymak için parçalama tekniklerini kullanın. Azure SQL veritabanı'nda parçalama teknikleri kullanan çoğu müşteri verilerine tek bir boyutta birden fazla veritabanında bölün. Bu yaklaşım için OLTP uygulamalar genellikle yalnızca bir satır veya satır şema küçük bir gruba uygulanan işlemler gerçekleştirmek bilmeniz gerekir.

> [!NOTE]
> SQL veritabanı artık ile parçalama yardımcı olmak için bir kitaplık sağlar. Daha fazla bilgi için [elastik veritabanı istemci kitaplığına genel bakış](sql-database-elastic-database-client-library.md).
> 
> 

Bir veritabanı, müşteri adına, sırası ve sipariş ayrıntıları (örneğin, SQL Server ile birlikte gelen geleneksel örnek Northwind veritabanı) varsa, örneğin, bu verileri birden çok veritabanlarına bir müşteri sipariş ayrıntısı ve ilgili sipariş ile gruplayarak bölebilir bilgiler. Müşteri verileri tek bir veritabanında kalır garanti edebilir. Uygulama, farklı müşteriler, etkili bir şekilde yük birden fazla veritabanında yayılmasını veritabanlarında ayırırsınız. Parçalama, müşterilerin yalnızca en fazla veritabanı boyutu sınırı kaçınabilirsiniz, ancak Azure SQL veritabanı, DTU tek tek her veritabanında uygun olduğu sürece farklı bilgi işlem boyutlarına sınırlardan önemli ölçüde daha büyük olan iş yükleri de işleyebilirsiniz.

Veritabanı parçalama bir çözüm için toplam kaynak kapasitesini azaltmaz ancak birden çok veritabanı yayılan çok büyük çözümlerde destekleyen en son derece etkilidir. Her veritabanı, çok büyük desteklemek için farklı işlem boyutta yüksek kaynak gereksinimlerine "etkin" veritabanlarıyla çalıştırabilirsiniz.

### <a name="functional-partitioning"></a>İşlevsel bölümleme
SQL Server kullanıcıları genellikle tek bir veritabanında çok sayıda işlevleri birleştirin. Örneğin, bir uygulamanın mantıksal deposunun envanterini yönetmek için varsa, veritabanı satınalma siparişleri, saklı yordamlar ve ayın son raporlamayı yönetme dizinli ya da gerçekleştirilmiş görünümler izleme envanteri ile ilişkili mantıksal olabilir. Bu teknik veritabanı için yedekleme gibi işlemleri yönetmenizi kolaylaştırır, ancak bir uygulamanın tüm işlevlerinin yükü işlemek için donanım boyutlandırmak de gerektirir.

Azure SQL veritabanı'nda bir ölçek genişletmeli mimarisinden kullanırsanız, uygulamanın farklı veritabanlarındaki farklı işlevlere bölmek için iyi bir fikirdir. Bu tekniği kullanarak, her uygulamayı bağımsız şekilde ölçeklendirir. Uygulama meşgul duruma (ve veritabanı üzerindeki yükü artırır gibi), yönetici uygulamada her işlev için bağımsız işlem boyutları seçebilirsiniz. Bu mimari ile sınır, uygulamanın yük, birden çok makineye yayılmış çünkü tek ticari makine işleyebileceğinden daha büyük olabilir.

### <a name="batch-queries"></a>Toplu işlem sorguları
Yüksek hacimli kullanarak verilere erişen uygulamalar için sık, geçici sorgulama, yanıt süresi önemli miktarda uygulama katmanı ve Azure SQL veritabanı katmanı arasında ağ iletişimi harcanır. Hem uygulama hem de Azure SQL veritabanı aynı veri merkezinde olsa bile, ikisi arasındaki ağ gecikme süresi çok sayıda veri tarafından erişim işlemleri büyütülmüş. Ağ azaltmak için veri erişim işlemleri için hepsini Geçiren, geçici sorgular ya da toplu veya olarak saklı yordamlar derlemeye seçeneğini kullanmayı düşünün. Geçici sorgular toplu varsa, birden çok sorgu tek bir seyahat büyük bir toplu olarak Azure SQL veritabanı'na gönderebilirsiniz. Geçici sorgular bir saklı yordamda derlerseniz, bunları toplu olarak, aynı sonucu elde. Ayrıca bir saklı yordamı kullanarak saklı yordamı yeniden kullanabilmeniz için Azure SQL veritabanında sorgu planlarına önbelleğe alma şansını artırmak avantajı sağlar.

Bazı uygulamalar yazma yoğunluklu olan. Bazen yazma birlikte batch nasıl dikkate alarak, bir veritabanındaki toplam GÇ yükü azaltabilir. Genellikle, bu saklı yordamlar ve geçici toplu otomatik tamamlama işlemleri yerine açık işlemleri kullanmanız yeterlidir. Kullanabileceğiniz farklı teknik değerlendirme için bkz: [teknikleri Azure SQL veritabanı uygulamaları için toplu işleme](https://msdn.microsoft.com/library/windowsazure/dn132615.aspx). Toplu işlem için doğru model bulmak için kendi iş yükü ile denemeler yapın. Bir model biraz farklı işlemsel tutarlılık garantisi olabileceğini anlamak emin olun. Kaynak kullanımını en aza indiren doğru iş yükü bulma, tutarlılık ve performans dengelemeler doğru birleşimini bulma gerektirir.

### <a name="application-tier-caching"></a>Uygulama katmanı önbelleğe alma
Bazı veritabanı uygulamalarının okuma yoğunluklu iş yükleri vardır. Katmanlar önbelleğe alma, veritabanı üzerindeki yükü azaltabilir ve potansiyel olarak Azure SQL veritabanı'nı kullanarak bir veritabanını desteklemek için gereken işlem boyutunu azaltabilir. İle [Azure Redis Cache](https://azure.microsoft.com/services/cache/), okuma yoğun iş yükü varsa, bir kez veri okuma (ya da belki nasıl yapılandırıldığına bağlı olarak bir kez uygulama katmanlı makine başına) ve SQL veritabanınızı dışında veri depolayın. (CPU ve okuma GÇ) veritabanı yükünü azaltmak için bir yol budur ancak önbellekten okunan verilerin eşitlenmemiş veritabanındaki verilerle olabileceğinden işlem tutarlılığı üzerinde bir etkisi olan. Birçok uygulamada tutarsızlık belirli bir düzeyde kabul edilebilir olsa da, tüm iş yükleri için geçerli değildir. Bir uygulama katmanı önbelleğe alma stratejisi uygulamadan önce herhangi bir uygulama gereksinimi tam olarak anlamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar
* DTU tabanlı hizmet katmanları hakkında daha fazla bilgi için bkz. [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md).
* Sanal çekirdek tabanlı hizmet katmanları hakkında daha fazla bilgi için bkz. [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md).
* Elastik havuzlar hakkında daha fazla bilgi için bkz: [Azure elastik havuzu nedir?](sql-database-elastic-pool.md)
* Performans ve elastik havuzlar hakkında daha fazla bilgi için bkz: [ne zaman elastik havuz göz önünde bulundurun.](sql-database-elastic-pool-guidance.md)

