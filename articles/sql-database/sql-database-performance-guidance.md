---
title: Azure SQL veritabanı performans Kılavuzu ayarlama | Microsoft Docs
description: Azure SQL veritabanı sorgu performansını artırmak için öneriler kullanma hakkında bilgi edinin.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: article
ms.date: 02/12/2018
ms.author: carlrab
ms.openlocfilehash: c9a04f6ebbca60e969d608e0ad92839b5e04d772
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="tuning-performance-in-azure-sql-database"></a>Azure SQL veritabanında performans ayarlama

Azure SQL veritabanı sağlar [önerileri](sql-database-advisor.md) veritabanınızın performansını geliştirmek için kullanabilir veya Azure SQL veritabanı sağlayabilirsiniz [uygulamanızı otomatik olarak uyum](sql-database-automatic-tuning.md) ve değişiklikleri uygulayın, İş yükünüzün performansını artırır.

Düzenlemek istediğiniz herhangi bir geçerli önerimiz yok ve performans sorunlarını çözümlenmedi, performanslarını iyileştirmek için aşağıdaki yöntemleri kullanabilirsiniz:
1. Artırmak [hizmet katmanları](sql-database-service-tiers.md) ve veritabanınızı daha fazla kaynak sağlayın.
2. Uygulamanızı ayarlama ve performansı artırabilir bazı en iyi uygulamalar uygulayın. 
3. Veritabanı, dizinler ve daha verimli bir şekilde verilerle çalışmak için sorgular değiştirerek ayarlayın.

El ile uygulanan yöntemleri ne karar vermeniz gerekir çünkü bunlar [hizmet katmanları](sql-database-service-tiers.md) seçmeniz veya uygulama ya da veritabanı kodu yeniden yazma ve değişiklikleri dağıtmak gerekir.

## <a name="increasing-performance-tier-of-your-database"></a>Veritabanınızın artan performans katmanı

Azure SQL veritabanı iki satın alma modeli, vCore tabanlı satın alma modeli ve v-çekirdek tabanlı satın alma modeli sunar. Her model birden çok sahip [hizmet katmanları](sql-database-service-tiers.md) , arasından seçim yapabilirsiniz. Her hizmet katmanında kesinlikle SQL veritabanınız kullanabilirsiniz ve bu hizmet düzeyi için tahmin edilebilir performans garanti kaynakları yalıtır. Bu makalede, uygulamanız için hizmet katmanı seçmenize yardımcı olacak yönergeler sağlıyoruz. Biz de en iyi Azure SQL veritabanı için uygulamanızı ayarlayabilirsiniz yolları ele alınmıştır.

> [!NOTE]
> Bu makalede tek veritabanları Azure SQL Database performans rehberi odaklanır. Esnek havuzlar için ilgili performans yönergeler için bkz [esnek havuzlar için fiyat ve performans konuları](sql-database-elastic-pool-guidance.md). Ancak, bu makaledeki ayarlama önerilerin esnek havuzdaki veritabanları için geçerlidir ve benzer performans avantajlarından yararlanabilmek unutmayın.
> 

* **Temel**: temel hizmet katmanı teklifleri iyi bir performans öngörülebilirlik her veritabanı için saat üzerinden saat. Temel bir veritabanında yeterli kaynakları birden çok eşzamanlı istek yok küçük bir veritabanında iyi bir performans desteklemez. Temel Hizmet katmanını kullandığınızda tipik kullanım örnekleri şunlardır:
  * **Yalnızca Azure SQL veritabanı ile çalışmaya Başlarken**. Genellikle geliştirme olan uygulamalar, yüksek performans düzeyleri gerek yoktur. Veritabanı geliştirme veya sınama, düşük fiyat noktada için ideal bir ortam temel veritabanlarıdır.
  * **Tek bir kullanıcıyla bir veritabanına sahip**. Tek bir kullanıcı genellikle bir veritabanıyla ilişkilendirin uygulamaları yüksek tutarlılık ve performans gereksinimlerini yok. Bu uygulamalar temel hizmet katmanının bir aday değildir.
* **Standart**: Standart hizmet katmanı performansı öngörülebilirlik sunar ve çalışma grubu ve web uygulamaları gibi birden çok eş zamanlı istekleri veritabanları için iyi bir performans sağlar. Bir standart hizmet katmanı veritabanı seçtiğinizde, veritabanı uygulamanızı tahmin edilebilir performans üzerinde boyutlandırabilirsiniz dakika üzerinden dakika.
  * **Birden çok eşzamanlı istek veritabanınızı sahip**. Genellikle aynı anda birden fazla kullanıcı hizmet uygulamaları, daha yüksek performans düzeyleri gerekir. Örneğin, düşük birden çok eşzamanlı sorguyu destekler Orta g/ç trafiği için gereksinimlerin çalışma grubu veya web uygulamaları, standart hizmet katmanı için iyi bir aday değildir.
* **Premium**: Premium Hizmet katmanını tahmin edilebilir performans sunar her Premium veya iş kritik (Önizleme) için ikinci üzerinden ikinci veritabanı. Premium Hizmet katmanını seçtiğinizde, bu veritabanı için yoğun yük temel veritabanı uygulamanızı boyutlandırabilirsiniz. Plan durumları hangi performans farkı küçük sorguları gecikme süresine duyarlı işlemleri beklenenden daha uzun sürmesine neden olabilir kaldırır. Bu model, geliştirme ve ürün doğrulama döngüleri güçlü deyimleri en yüksek kaynak gereksinimleri, performans farkı veya sorgu gecikmesi hakkında vermeniz gereken uygulamalar için büyük ölçüde basitleştirebilir. Çoğu Premium Hizmet katmanını kullanım örnekleri, bir veya daha fazla şu özelliklere sahiptir:
  * **Yüksek yoğun yük**. Önemli ölçüde CPU, bellek veya giriş/çıkış (IO) işlemlerini tamamlamak için gerektiren bir uygulama, ayrılmış, yüksek performans düzeyi gerektirir. Örneğin, uzun bir süre çeşitli CPU çekirdekleri kullanmak için bilinen bir veritabanı işlemi Premium hizmet katmanı için bir adaydır.
  * **Çok sayıda eşzamanlı istek**. Bir Web sitesi, hizmet veren bir yüksek trafik hacmi olduğunda bazı veritabanı çok sayıda eşzamanlı istek, örneğin, hizmet uygulamaları. Temel ve standart hizmet katmanları veritabanı başına eşzamanlı istek sayısını sınırlayın. Daha fazla bağlantı gerektiren uygulamalar sayısı gerekli istekleri işlemek için uygun ayırma boyutu seçmek gerekir.
  * **Düşük gecikme süresi**. En az sürede yanıt veritabanından güvence altına almak bazı uygulamaları gerekir. Belirli bir saklı yordam daha geniş bir müşteri işleminin bir parçası çağrılırsa, bu çağrı bir dönüş en fazla 20 milisaniye cinsinden süre yüzde 99'olan bir gereksinimi olabilir. Bu tür bir uygulama için gerekli hesaplama gücü kullanılabilir olduğundan emin olmak için Premium hizmet katmanından, yararlı olur.

SQL veritabanınız için gereken hizmet düzeyi her kaynak boyutu için en yüksek yük gereksinimlerine bağlıdır. Bazı uygulamaları tek bir kaynak Önemsiz miktarını kullanır, ancak diğer kaynaklar için önemli gereksinimleri vardır.

### <a name="service-tier-capabilities-and-limits"></a>Hizmet katmanı özellikleri ve sınırları

Her hizmet katmanı yalnızca gereksinim duyduğunuz kapasitesi için ödeme esnekliğine sahip şekilde performans düzeyi ayarlayın. Yapabilecekleriniz [kapasite ayarlamak](sql-database-service-tiers.md), yukarı veya aşağı, iş yükü değişiklikleri olarak. Örneğin, veritabanının yükünüzü geri Okul alışveriş sezonu sırasında yüksekse, Temmuz Eylül aracılığıyla belirlenen bir süre için veritabanı performans düzeyi artırabilir. Yoğun sezonu sona erdiğinde azaltabilir. Bulut ortamınızın işinizin mevsimselliğin en iyi duruma getirme tarafından ödeme en aza indirebilirsiniz. Bu model de iyi yazılım ürün yayın döngüsü boyunca çalışır. Test çalışmasını ve test bitirdiğinizde kapasiteyi yayın sırasında test takımı kapasite ayırabilir. Gereksinim ve nadiren kullanabilecek ayrılmış kaynakları harcama kaçının bir kapasite isteği modelde kapasiteyi ücret ödersiniz.

### <a name="why-service-tiers"></a>Neden katmanları hizmet?
Her veritabanı iş yükü farklı olabilir ancak hizmet katmanları amacı performans öngörülebilirlik çeşitli performans düzeyleri sağlamaktır. Büyük ölçekli veritabanı kaynak gereksinimleri müşterilerle daha ayrılmış bir bilgisayar ortamda çalışabilir.

## <a name="tune-your-application"></a>Uygulamanızı ayarlama
Geleneksel şirket içi SQL Server ' ilk kapasite planlama işlemi genellikle uygulama üretimde çalışan işlemi ayrılır. Donanım ve Ürün lisans ilk satın ve performans ayarlama daha sonra yapılır. Azure SQL veritabanı kullandığınızda, bir uygulama çalıştıran ve onu ayarlama işleminde interweave iyi bir fikirdir. İsteğe bağlı kapasite için ödeme modeliyle, uygulamanızı bir uygulama için hangi sıklıkta yanlış Gelecekteki büyümeyi planları tahmin üzerinde temel donanımda işleminin yerine şimdi, gereken en düşük kaynakları kullanacak şekilde ayarlayabilirsiniz. Bazı müşteriler bir uygulama değil yapma seçin ve bunun yerine donanım kaynakları overprovision seçin. Meşgul bir süre içinde önemli uygulama değiştirmek istemiyorsanız, bu yaklaşım iyi bir fikir olabilir. Ancak, Azure SQL veritabanı'nda hizmet katmanları kullandığınızda, bir uygulama ayarı kaynak gereksinimlerini ve daha düşük aylık faturaları en aza indirebilirsiniz.

### <a name="application-characteristics"></a>Uygulama özellikleri
Azure SQL Database hizmet katmanları performans kararlılık ve öngörülebilirlik uygulamanın iyileştirmek için tasarlanmış olsa da, bazı en iyi uygulamalar, daha iyi bir performans düzeyinde kaynakları avantajlarından yararlanmak için uygulamanızı ayarlayın yardımcı olabilir. Birçok uygulama, daha yüksek bir performans düzeyine geçerek önemli performans artışları sahip olsa da veya hizmet katmanı, bazı uygulamalar ihtiyaç daha yüksek bir düzeyde hizmet yararlanmak için ek ayarlama. Performansı artırmak için bu özelliklere sahip uygulamalar için ek uygulama ayarlamayı göz önünde bulundurun:

* **Yavaş performansı nedeniyle "chatty" davranışı uygulamaları**. Chatty uygulamaları ağ gecikmesi hassas olduğunu aşırı veri erişim işlemleri yapın. Bu tür veri erişim işlemleri SQL veritabanına sayısını azaltmak için uygulamaların değiştirmeniz gerekebilir. Örneğin, geçici sorguları toplu işleme veya sorgular için saklı yordamlar taşıma gibi teknikler kullanılarak uygulama performansını iyileştirebilir. Daha fazla bilgi için bkz: [Toplu sorguları](#batch-queries).
* **Tüm tek makine tarafından desteklenen yoğun bir iş yükü veritabanlarıyla**. En yüksek Premium performans düzeyi kaynakları aşan veritabanları iş yükünü ölçeklendirme yararlı. Daha fazla bilgi için bkz: [veritabanları arası parçalama](#cross-database-sharding) ve [işlevsel bölümleme](#functional-partitioning).
* **Yetersiz sorguları uygulamaları**. Uygulamalar, özellikle olanlar sorguları kötü düzenlediniz veri erişim katmanı, daha yüksek bir performans düzeyinden yarar değil. Bu, WHERE yan tümcesi eksik, dizinleri eksik olan veya istatistikleri eski sorguları içerir. Bu uygulamalar standart sorgu teknikleri performans ayarlama yarar. Daha fazla bilgi için bkz: [eksik dizinler](#identifying-and-adding-missing-indexes) ve [ayarlama ve ipucu sorgu](#query-tuning-and-hinting).
* **Yetersiz veri uygulamaları erişim tasarım**. Devralınan veri erişim eşzamanlılık sorunları, örneğin kilitlenmenin, uygulamaları daha yüksek bir performans düzeyinden yararlı değildir. Azure önbelleği hizmeti veya başka bir önbelleğe alma teknolojisini istemci tarafında önbelleğe alma verileri tarafından gidiş dönüş Azure SQL veritabanına karşı azaltmayı deneyin. Bkz: [uygulama katmanı önbelleğe alma](#application-tier-caching).

## <a name="tune-your-database"></a>Veritabanınızı ayarlama
Bu bölümde, uygulamanız için en iyi performansı elde etmek ve en düşük olası performans düzeyinde çalıştırmak için Azure SQL veritabanını ayarlamak için kullanabileceğiniz bazı teknikleri ele. Bu teknikler bazıları geleneksel SQL Server en iyi yöntemler ayarlama eşleşen ancak diğerleri Azure SQL veritabanına özgüdür. Bazı durumlarda, daha fazla ayarlamak ve Azure SQL veritabanındaki iş için geleneksel SQL Server teknikler genişletmek için alanlar bulmak bir veritabanı için tüketilen kaynakları inceleyebilirsiniz.

### <a name="identify-performance-issues-using-azure-portal"></a>Azure portalını kullanarak performans sorunlarını belirle
Azure portalında aşağıdaki araçlar, SQL veritabanıyla performans sorunlarını çözün ve çözümlemenize yardımcı olabilir:

* 
  [Sorgu Performansı İçgörüleri](sql-database-query-performance.md)
* [SQL Veritabanı Danışmanı](sql-database-advisor.md)

Azure portal her ikisi de bu araçlar ve bunların nasıl kullanılacağını hakkında daha fazla bilgi sahiptir. Verimli bir şekilde sorunlarını tanılamak ve düzeltmek için öncelikle Azure portalında araçları deneyin öneririz. Eksik dizinler ve özel durumlarda sorgu ayarlama için ardından, aşağıdakiler ele yaklaşımlar ayarlama el ile kullanmanızı öneririz.

Üzerinde Azure SQL veritabanı sorunlarını tanımlama hakkında daha fazla bilgi bulmak [performans izleme](sql-database-single-database-monitor.md) makalesi.

### <a name="identifying-and-adding-missing-indexes"></a>Tanımlama ve eksik dizinler ekleme
Ortak bir OLTP veritabanı performans sorunu fiziksel veritabanı tasarım ilişkilendirir. Genellikle, veritabanı şemalarını tasarlanır ve ölçekli (ya da yükleme veya veri birimi) sınamadan geliyordu. Ne yazık ki, bir sorgu planı performansını bir küçük ölçekte kabul edilebilir ancak üretim düzeyinde veri birimlerini önemli ölçüde düşürebilir. Bu sorunu en yaygın kaynağı filtre ya da başka kısıtlamalar sorguda karşılamak için uygun dizin yetersizliğidir. Dizin arama yeterli genellikle, bir tablo olarak eksik dizinler bildirimleri taramasını.

Bir arama yeterli, bu örnekte, bir tarama seçili sorgu planı kullanır:

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

![Eksik dizinler içeren bir sorgu planı](./media/sql-database-performance-guidance/query_plan_missing_indexes.png)

Azure SQL veritabanı Bul ve koşullar dizin düzeltme ortak eksik yardımcı olabilir. Azure SQL veritabanına yerleşik Dmv'leri içinde bir dizin önemli ölçüde bir sorguyu çalıştırmak için tahmini maliyetini azaltmak sorgu derlemelerinin arayın. Sorgu yürütme sırasında SQL veritabanı ne sıklıkta her sorgu planı yürütülür ve burada bu dizinde varolan tahmini boşluk yürütülen sorgu planı ve imagined bir arasında izler izler. Bu Dmv'leri, fiziksel veritabanı tasarımınız için hangi değişikliklerin bir veritabanı ve onun gerçek iş yükü için Genel İş Yükü Maliyeti iyileştirebilir hızla tahmin etmek için kullanabilirsiniz.

Olası eksik dizinler değerlendirmek için bu sorguyu kullanın:

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

Oluşturulduktan sonra aynı SELECT deyimi bir tarama yerine bir arama kullanan ve plan daha verimli bir şekilde yürütür farklı bir plan seçer:

![Düzeltilmiş dizinleri içeren bir sorgu planı](./media/sql-database-performance-guidance/query_plan_corrected_indexes.png)

Anahtar Insight paylaşılan, ticari sistem GÇ kapasitesini daha, adanmış sunucu makinesi sınırlıdır. Azure SQL Database hizmet katmanları her performans düzeyi DTU içinde sistem en büyük avantajlarından yararlanmak için gereksiz GÇ en aza üzerinde bir premium yoktur. Uygun fiziksel veritabanı tasarım seçenekleri tek tek sorgular için gecikme süresini önemli ölçüde iyileştirebilen ölçek birimi işlenen eş zamanlı isteklerin verimini artırmak ve sorgu karşılamak için gerekli maliyetleri en aza indirmek. Eksik dizini Dmv'leri hakkında daha fazla bilgi için bkz: [sys.dm_db_missing_index_details](https://msdn.microsoft.com/library/ms345434.aspx).

### <a name="query-tuning-and-hinting"></a>Sorgu ayarlama ve ipuçları
Azure SQL veritabanındaki sorgu iyileştiricisi için geleneksel SQL Server sorgu iyileştiricisi benzer. Sorguları ayarlama ve mantığı anlamak için en iyi uygulamaları çoğu sorgu iyileştiricisi modeli sınırlamalar de Azure SQL veritabanı için geçerlidir. Azure SQL Database sorguları ayarlamak, birleşik kaynak taleplerini azaltma ek bir avantaja alabilirsiniz. Uygulamanızı karıncalı gösteren eşdeğer bir daha düşük bir maliyetle daha düşük bir performans düzeyinde çalışabildiğinden çalıştırmanız mümkün olabilir.

SQL Server'da ortak ve Azure SQL veritabanına da geçerli örnek nasıl sorgu iyileştiricisi "sızar" parametreleri. Derleme sırasında sorgu iyileştiricisi daha uygun bir sorgu planı oluştur olup olmadığını belirlemek için bir parametrenin geçerli değeri değerlendirir. Bu strateji çoğunlukla bilinen parametre değerlerini derlenmiş bir planı önemli ölçüde daha hızlı bir sorgu planı açabilir ancak şu anda imperfectly her ikisi de çalıştığını SQL Server ve Azure SQL veritabanı. Bazen parametresi sniffed değil ve bazen parametresi sniffed ancak oluşturulan plan bir iş yükü parametre değerleri tam kümesi için yetersiz. Microsoft hedefi daha bilinçli olarak belirtin ve parametre algılaması varsayılan davranışı geçersiz kılma sorgulama ipuçlarını (yönergeleri) içerir. Genellikle, ipuçlarını kullanırsanız, SQL Server veya Azure SQL veritabanı'nın varsayılan davranışını belirli bir müşteri iş yükü için kusurlu olduğu durumlar düzeltebilirsiniz.

Sonraki örnek Sorgu işlemcisi hem performans ve kaynak gereksinimleri için uygun olmayan bir planı nasıl oluşturacağınız gösterilmiştir. Bu örnek bir sorgu ipucu kullanırsanız, SQL veritabanınız için sorgu çalıştırma zaman ve kaynak gereksinimlerini azaltmak de gösterir:

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

Kurulum kodu, veri dağıtımı eğri bir tablo oluşturur. En iyi sorgu planı hangi parametresini seçili göre farklılık gösterir. Ne yazık ki, önbelleğe alma davranışı planı her zaman en yaygın parametre değerine göre sorgusunu yeniden derleyin değil. Bu nedenle, önbelleğe alınan ve hatta farklı bir plan daha iyi bir plan seçim ortalama olabilir birçok değerler için kullanılacaktır için yetersiz bir plan için mümkündür. Daha sonra sorgu planı özel sorgu ipucu varsa ancak bu, aynı olan iki saklı yordamlar oluşturur.

**Örneğin, bölüm 1**

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

**Örneğin, bölüm 2**

(Sonuçları elde edilen telemetri verileri ayrı; böylece örnek 2 parçası başlamadan önce en az 10 dakika bekleyin öneririz.)

    EXEC psp2 @param2=1;
    TRUNCATE TABLE t1;

    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp2 @param2=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

Bu örnekte her parçası (bir sınama veri kümesi olarak kullanmak için yeterli bir yük oluşturmak için) parametreli INSERT deyiminin 1.000 kez çalıştırmayı dener. Saklı yordamlar yürütüldüğünde, Sorgu işlemcisi (parametre "algılaması"), ilk derleme sırasında yordamına geçirilen parametre değeri inceler. İşlemci elde edilen planı önbelleğe alır ve parametre değeri farklı olsa bile sonraki etkinleştirmeleri için kullanır. En iyi planı tüm durumlarda kullanılmayabilir. Bazen zaman sorgu önce derlenen gelen özel durum yerine ortalama çalışması için daha iyi bir plan seçmek için en iyi hale getirme Kılavuzu gerekir. Bu örnekte, ilk planı parametreyle eşleşen her bir değeri bulmak için tüm satırların okuyan bir "tarama" plan oluşturur:

![Bir tarama planı kullanarak ayarlama sorgulama](./media/sql-database-performance-guidance/query_tuning_1.png)

1 değeri kullanılarak biz yordamı yürütülen olduğundan, sonuçta elde edilen planı 1 değeri için en iyi ancak tablosundaki diğer tüm değerler için yetersiz. Büyük olasılıkla sonuç planda daha yavaş çalışır ve daha fazla kaynak kullandığı için her plan rastgele, çekme olsaydınız ne istersiniz değil.

Test ile çalıştırırsanız `SET STATISTICS IO` kümesine `ON`, bu örnekte mantıksal tarama iş arka planda gerçekleştirilir. (Ortalama yalnızca tek bir satır döndürülecek durumda verimsiz) plan tarafından yapılan 1,148 okuma olduğunu görebilirsiniz:

![Mantıksal bir tarama kullanarak ayarlama sorgulama](./media/sql-database-performance-guidance/query_tuning_2.png)

Örneğin ikinci bölümü derleme işlemi sırasında belirli bir değer kullanmak için en iyi hale getirme bildirmek için bir sorgu ipucu kullanır. Bu durumda, parametre olarak geçirilen değer yoksaymak için Sorgu işlemcisi zorlar ve bunun yerine varsaymak `UNKNOWN`. Bu ortalama sıklığını (Eğme yoksayar) tablosunda sahip bir değer gösterir. Sonuçta elde edilen planı daha hızlı ve daha az kaynak ortalama, plan daha bölüm bu örnek 1 kullanan bir arama tabanlı planı şöyledir:

![Bir sorgu ipucunu kullanarak sorguyu ayarlamayı](./media/sql-database-performance-guidance/query_tuning_3.png)

Aslında görebilirsiniz **sys.resource_stats** tablosu (test ve ne zaman veri tablosu doldurur yürütme zamandan bir gecikme olur). Bu örnek, 22:25:00 zaman penceresi sırasında yürütülen bölüm 1 ve 2. Bölüm 22:35: 00'dan yürütülür. Önceki zaman penceresi pencere (nedeniyle plan Verimlilik geliştirmeleri) sonraki olandan daha fazla kaynak kullanılır.

    SELECT TOP 1000 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![Sorgu ayarlama örneği sonuçları](./media/sql-database-performance-guidance/query_tuning_4.png)

> [!NOTE]
> Bu örnekte birim özellikle küçük olsa da, yetersiz parametreleri etkisini özellikle büyük veritabanları üzerinde önemli olabilir. Olağanüstü durumlarda, fark hızlı durumlarda saniye ile yavaş durumlarda saat arasında olabilir.
> 
> 

İnceleyebilirsiniz **sys.resource_stats** kaynak bir test için başka bir test daha az veya daha fazla kaynak kullandığını belirlemek için. Veri karşılaştırdığınızda, böylece aynı 5 dakikalık penceresinde olmadıkları testleri zamanlama ayrı **sys.resource_stats** görünümü. Toplam kullanılan kaynakların en aza indirmek ve yoğun kaynak en aza değil alıştırma hedefidir. Genellikle, bir gecikme için kod parçasını iyileştirme Ayrıca kaynak tüketimini azaltır. Bir uygulamaya yaptığınız değişiklikler gereklidir ve değişiklikleri sorgulama ipuçlarını uygulamada kullanabilecek biri için müşteri deneyimini olumsuz yönde etkilemediğinden emin olun.

Bir iş yükü sorgularını yineleme kümesi varsa, genellikle yakalamak ve veritabanını barındırmak için gereken en düşük kaynak boyutu birimi sürücüleri için en iyi hale getirme planı seçenekleriniz, doğrulamak için mantıklıdır. Bazen, doğrulama sonra bunlar değil düşürülmüş olduğundan emin olun yardımcı olmak için planları yeniden gözden geçirin. Hakkında daha fazla bilgiyi [sorgulama ipuçlarını (Transact-SQL)](https://msdn.microsoft.com/library/ms181714.aspx).

### <a name="cross-database-sharding"></a>Veritabanları arası parçalama
Azure SQL veritabanı ticari donanım üzerinde çalıştığından, tek bir veritabanı için kapasite limitlerini geleneksel şirket içi SQL Server yüklemesi için daha düşük. Bazı müşteriler, işlemleri Azure SQL Database tek bir veritabanında sınırları içinde uymayan zaman veritabanı işlemlerini birden fazla veritabanı yayılan için parçalama tekniklerini kullanın. Azure SQL veritabanı'nda parçalama teknikleri kullanan çoğu müşteri verilerini tek bir boyuttaki birden çok veritabanı arasında bölün. Bu yaklaşım, OLTP uygulamalar genellikle yalnızca bir satır veya şema satır küçük bir gruba uygulanan işlemler gerçekleştirmek anlamanız gerekir.

> [!NOTE]
> SQL veritabanı artık ile parçalama yardımcı olmak için bir kitaplık sağlar. Daha fazla bilgi için bkz: [esnek veritabanı istemci kitaplığına genel bakış](sql-database-elastic-database-client-library.md).
> 
> 

Bir veritabanında müşteri adı, sipariş ve sipariş ayrıntıları (örneğin, SQL Server ile birlikte gelen geleneksel örnek Northwind veritabanı) varsa, örneğin, size bu verileri birden çok veritabanlarına Sipariş Ayrıntısı ve ilgili sırası müşteriyle gruplandırarak bölebilir bilgi. Müşteri'nin verileri tek bir veritabanında kalır garanti edebilir. Uygulama, birden çok veritabanı arasında etkili bir şekilde yük yayılmak veritabanları arasında farklı müşterilere ayırırsınız. Parçalama, müşteriler yalnızca en fazla veritabanı boyut sınırına önleyebilirsiniz, ancak Azure SQL Database tek tek her veritabanı kendi DTU uygun sürece farklı performans düzeylerini sınırlardan önemli ölçüde daha büyük iş yüklerini de işleyebilir.

Veritabanı parçalama bir çözüm için birleşik kaynak kapasitesini azaltmaz ancak birden fazla veritabanı yayılır çok büyük çözümlerde destekleyen en son derece etkili olur. Her veritabanı çok büyük desteklemek için farklı performans düzeyinde yüksek kaynak gereksinimleri "etkin" veritabanları çalıştırabilirsiniz.

### <a name="functional-partitioning"></a>İşlevsel bölümleme
SQL Server kullanıcıları genellikle tek bir veritabanında birçok işlevini birleştirin. Örneğin, bir uygulama bir mağaza için stok yönetmek için mantığı varsa, bu veritabanı izleme satınalma siparişi, saklı yordamları ve ayın son bildirdikleri yönetmek dizinlenmiş veya gerçekleştirilmiş görünümler stok ile ilişkili mantığı sahip olabilir. Bu teknik yedekleme gibi işlemleri için veritabanını yönetmek üzere kolaylaştırır, ancak aynı zamanda bir uygulamanın tüm işlevlerinin yoğun yükü işlemek üzere donanım boyutunu gerekir.

Azure SQL veritabanı genişleme mimarisinde kullanırsanız, uygulamanın farklı veritabanlarına farklı işlevler bölmek için iyi bir fikirdir. Bu yöntemi kullanarak, her uygulama bağımsız olarak ölçekler. Bir uygulama yoğun olur (ve veritabanı üzerindeki yükü artırır gibi), yönetici uygulamada her işlevin bağımsız performans düzeyleri seçebilirsiniz. Bu mimari ile sınırı, bir uygulama birden fazla makine arasında yük yayıldığı için tek Emtia makine işleyebileceğinden daha büyük olamaz.

### <a name="batch-queries"></a>Toplu sorguları
Yüksek hacimli kullanarak veri erişim uygulamalar için sık, geçici sorgulama, yanıt süresini önemli miktarda uygulama katmanı ve Azure SQL veritabanı katmanı arasındaki ağ iletişimi harcanmaktadır. Hem uygulama hem de Azure SQL veritabanını aynı veri merkezinde olsa bile, ikisi arasındaki ağ gecikmesi çok sayıda veri tarafından erişim işlemleri büyütülmüş. Ağ azaltmak için veri erişim işlemleri için döngü, geçici sorguları ya da toplu veya olarak saklı yordamlar derlemeye seçeneğini kullanmayı düşünün. Geçici sorguları toplu varsa, birden çok sorgu tek bir seyahat bir büyük toplu olarak Azure SQL veritabanına gönderebilirsiniz. Saklı yordamdaki geçici sorgular derleme yaparsanız, bunları toplu gibi aynı sonucu elde. Ayrıca bir saklı yordamı kullanarak saklı yordamı yeniden kullanabilmeniz için Azure SQL veritabanındaki sorgu planları önbelleğe alma olasılığını artırmak avantajı sağlar.

Bazı uygulamalar yazma yoğunluklu. Bazen yazma birlikte toplu nasıl dikkate alarak bir veritabanında toplam g/ç yükünü azaltabilir. Genellikle, bu saklı yordamları ve geçici toplu otomatik tamamlama işlemlerini yerine açık işlemleri kullanmanız yeterlidir. Kullanabileceğiniz farklı teknikleri değerlendirme için bkz: [teknikleri Azure SQL veritabanı uygulamaları için toplu işleme](https://msdn.microsoft.com/library/windowsazure/dn132615.aspx). Toplu işlem için doğru modeli bulmak için kendi iş yükü deneme. Bir model biraz farklı işlem tutarlılığı garanti olabilir anladığınızdan emin olun. Kaynak kullanımını en aza indirir sağ iş yükü bulma tutarlılık ve performans dengelemeler doğru bileşimini bulma gerektirir.

### <a name="application-tier-caching"></a>Uygulama katmanı önbelleğe alma
Bazı veritabanı uygulamaları okuma yoğun iş yükleri vardır. Katmanlar önbelleğe alma veritabanı azaltabilir ve Azure SQL veritabanını kullanarak bir veritabanı desteklemek için gereken performans düzeyi azaltmak. İle [Azure Redis önbelleği](https://azure.microsoft.com/services/cache/), okuma ağır iş yükü varsa, bir kez veri okuma (ya da belki nasıl yapılandırıldığına bağlı olarak bir kez uygulama katmanı makine başına,), SQL veritabanınızın dışındaki verileri depolamak ve. Bu veritabanı Yükü (CPU ve okuma g/ç) azaltmak için bir yoludur ancak önbellekten okunan verileri veritabanındaki verileri ile eşitlenmemiş olabilir çünkü işlemsel tutarlılık üzerinde bir etkisi olan. Birçok uygulamada belirli bir düzeyde tutarsızlık kabul edilebilir olmasına rağmen tüm iş yükleri için geçerli değildir. Bir uygulama katmanı önbelleğe alma stratejisi uygulamadan önce herhangi bir uygulama gereksinimi tam olarak anlamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar
* Hizmet katmanları hakkında daha fazla bilgi için bkz: [SQL Database seçenekleri ve performansı](sql-database-service-tiers.md)
* Esnek havuzları hakkında daha fazla bilgi için bkz: [Azure bir esnek havuz nedir?](sql-database-elastic-pool.md)
* Performans ve esnek havuzları hakkında daha fazla bilgi için bkz: [ne zaman bir esnek havuz göz önünde bulundurun](sql-database-elastic-pool-guidance.md)

