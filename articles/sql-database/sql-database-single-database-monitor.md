---
title: Azure SQL Veritabanı’nda veritabanı performansını izleme | Microsoft Belgeleri
description: Azure araçlarını ve dinamik yönetim görünümlerini kullanarak veritabanınızı izleme seçenekleri hakkında bilgi edinin.
keywords: veritabanı izleme, bulut veritabanı performansı
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: carlrab
ms.openlocfilehash: dc04a9334b63656719a7633a8dd7154ed6cd6993
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39092588"
---
# <a name="monitoring-database-performance-in-azure-sql-database"></a>Azure SQL Database'de veritabanı performansını izleme
Azure SQL veritabanı performansını izlemeye, seçtiğiniz veritabanı performans düzeyiyle ilgili kaynak kullanımını izleyerek başlarsınız. İzleme yardımcı olur, veritabanınızın gerekenden fazla kapasiteye sahip veya bu kaynakları dışarı ayarlanma çünkü sorun yaşıyor belirlemek ve ardından performans düzeyinin ve hizmet katmanları, veritabanı zaman olup olmadığına karar vermek [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) veya [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md). Veritabanınızı [Azure portalında](https://portal.azure.com) bulunan grafik araçlarını veya SQL [dinamik yönetim görünümlerini](https://msdn.microsoft.com/library/ms188754.aspx) kullanarak izleyebilirsiniz.

> [!TIP]
> Kullanım [Azure SQL Intelligent Insights](sql-database-intelligent-insights.md) otomatik, veritabanı performansını izleme. Bir performans sorunu algılandığında bir tanılama günlüğü, Ayrıntılar ve sorunun kök neden analizi (RCA) ile oluşturulur. Mümkün olduğunda performans iyileştirme öneri sağlanmaktadır.
>

## <a name="monitor-databases-using-the-azure-portal"></a>Azure portalını kullanarak veritabanlarını izleme
[Azure portalında](https://portal.azure.com/), tek veritabanlarının kullanımını veritabanınızı seçip **İzleme** grafiğine tıklayarak izleyebilirsiniz. Bu işlem sonrasında bir **Ölçüm** penceresi görüntülenir. **Grafiği düzenle** düğmesine tıklayarak değişiklik yapabilirsiniz. Şu ölçümleri ekleyin:

* CPU yüzdesi
* DTU yüzdesi
* Veri G/Ç yüzdesi
* Veri boyutu yüzdesi

Bu ölçümleri ekledikten sonra görüntülemeye devam edebilirsiniz **izleme** grafik hakkında daha fazla bilgi **ölçüm** penceresi. Dört ölçümün tümü de veritabanınızın ortalama **DTU** kullanım yüzdesini gösterir. Bkz: [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md) makaleler hizmet katmanları hakkında daha fazla bilgi için.  

![Hizmet katmanına göre veritabanı performansını izleme.](./media/sql-database-single-database-monitoring/sqldb_service_tier_monitoring.png)

Performans ölçümlerine ilişkin uyarıları da yapılandırabilirsiniz. **Ölçüm** penceresindeki **Uyarı ekle** düğmesine tıklayın. Uyarınızı yapılandırmak için sihirbazı takip edin. Ölçümlerin belirli bir eşiği aşması veya belirli bir eşiğin altına düşmesi halinde uyarı alabilirsiniz.

Örneğin, veritabanınızdaki bir iş yükünün artmasını bekliyorsanız bir e-posta uyarısı yapılandırarak veritabanınızın herhangi bir performans ölçümünde %80 sınırına ulaşması halinde uyarı alabilirsiniz. Bu uyarıyı, daha yüksek bir performans düzeyine ne zaman geçmeniz gerektiğini anlamak üzere erken bir uyarı olarak kullanabilirsiniz.

Performans ölçümleri, daha düşük bir performans düzeyine geçip geçemeyeceğinizi belirlemenize de yardımcı olabilir. Standart S2 veritabanını kullandığınızı ve tüm performans ölçümlerinin, veritabanının belirli bir zaman için ortalama %10'dan daha fazla kullanımda bulunmadığını gösterdiğini varsayın. Bu, veritabanının Standart S1'de de düzgün şekilde çalışabileceğini gösterir. Ancak daha düşük bir performans düzeyine geçmeye karar vermeden önce, ani değişiklik veya dalgalanma gösteren iş yüklerine dikkat edin.

## <a name="monitor-databases-using-dmvs"></a>DMV'leri kullanarak veritabanlarını izleme
Portalda kullanıma sunulan ölçümler aynı zamanda şu sistem görünümleri aracılığıyla da kullanılabilir: sunucunuzun mantıksal **asıl** veritabanında bulunan [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) ve kullanıcı veritabanında bulunan [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx). Daha uzun bir sürede daha az ayrıntılı veri izlemeniz gerekiyorsa **sys.resource_stats** görünümünü kullanın. Daha küçük bir zaman diliminde daha fazla ayrıntılı veri izlemeniz gerekiyorsa **sys.dm_db_resource_stats** görünümünü kullanın. Daha fazla bilgi için bkz. [Azure SQL Database Performans Rehberi](sql-database-single-database-monitor.md#monitor-resource-use).

> [!NOTE]
> **sys.dm_db_resource_stats**, Web ve İşletme sürümü veritabanlarında (bu sürümler kullanımdan kaldırılmıştır) kullanıldığında boş bir sonuç kümesi getirir.
>
>

### <a name="monitor-resource-use"></a>Kaynak kullanımını izleme

Kaynak kullanımı kullanarak izleyebileceğiniz [SQL veritabanı sorgu performansı İçgörüleri](sql-database-query-performance.md) ve [Query Store](https://msdn.microsoft.com/library/dn817826.aspx).

Bu iki görünüm kullanarak kullanımını da izleyebilirsiniz:

* [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)
* [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx)

#### <a name="sysdmdbresourcestats"></a>sys.dm_db_resource_stats
Kullanabileceğiniz [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) her SQL veritabanı'nda görüntüle. **Sys.dm_db_resource_stats** son kaynak kullanım verilerini hizmet katmanına göre görüntüler. Ortalama CPU, veri GÇ, günlük yazma ve bellek yüzdelerini her 15 saniyede kaydedilir ve 1 saat boyunca korunur.

Bu görünüm, kaynak kullanımını daha ayrıntılı göz sağladığından, kullanın **sys.dm_db_resource_stats** herhangi bir geçerli durumu çözümlemesi için ilk ya da sorun giderme. Örneğin, bu sorgu, geçerli veritabanı için ortalama ve en yüksek kaynak kullanımı son bir saat içinde gösterir:

    SELECT  
        AVG(avg_cpu_percent) AS 'Average CPU use in percent',
        MAX(avg_cpu_percent) AS 'Maximum CPU use in percent',
        AVG(avg_data_io_percent) AS 'Average data IO in percent',
        MAX(avg_data_io_percent) AS 'Maximum data IO in percent',
        AVG(avg_log_write_percent) AS 'Average log write use in percent',
        MAX(avg_log_write_percent) AS 'Maximum log write use in percent',
        AVG(avg_memory_usage_percent) AS 'Average memory use in percent',
        MAX(avg_memory_usage_percent) AS 'Maximum memory use in percent'
    FROM sys.dm_db_resource_stats;  

Diğer sorgular için örneklere bakın [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx).

#### <a name="sysresourcestats"></a>sys.resource_stats
[Sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) görünümünde **ana** veritabanı, belirli hizmet katmanını ve performans düzeyinde SQL veritabanınızın performansını izlemenize yardımcı olabilecek ek bilgiler vardır. Veriler her 5 dakikada bir toplanan ve yaklaşık 14 gün boyunca tutulur. Bu görünüm, SQL veritabanınızı kaynakları nasıl kullandığını daha uzun vadeli bir geçmiş analize yararlı olur.

Aşağıdaki grafikte CPU kaynak kullanımı için bir Premium veritabanının P2 performans düzeyine sahip olduğu her saat bir hafta içinde gösterilmektedir. Bu grafik Pazartesi günü başlar, 5 iş günü gösterir ve ardından hafta gösterir, uygulama üzerinde çok az olur.

![SQL veritabanı kaynak kullanımı](./media/sql-database-performance-guidance/sql_db_resource_utilization.png)

Verileri, bu veritabanının yüzde 50'den yalnızca en yüksek CPU yükü şu anda sahip P2 performans düzeyi (Salı günü öğle saati) göre CPU kullanımı. CPU uygulamanın kaynak profilinde baskın faktörü ise, iş yükü her zaman en uygun olduğunu garanti etmek için doğru performans düzeyi P2 olduğunu karar verebilirsiniz. Bir uygulama, zaman içinde artmasına bekliyorsanız, böylece uygulama hiç olmadığı kadar performans düzeyi sınırına ulaşmadığınız olmayan bir ek kaynak arabelleği sağlamak için iyi bir fikirdir. Performans düzeyini artırmak istiyorsanız, bir veritabanı istekleri özellikle gecikmeye duyarlı ortamlarda etkili bir şekilde işlemek için yeterli güce sahip olmadığında ortaya çıkabilir müşteri görünür hatalarının önüne geçmek yardımcı olabilir. Örnek Web sayfalarını veritabanı çağrıları sonuçlarına göre boyar uygulamanın desteklediği bir veritabanıdır.

Diğer uygulama türleri aynı grafikte farklı yorumlayabilir. Örneğin, bir uygulama her gün Bordro veri işlemeye çalışır ve aynı grafiğe varsa, bu tür bir "toplu" modeli P1 performans düzeyinde ince ayar yapabilirsiniz. P1 performans düzeyi P2 performans düzeyinde 200 dtu'ya varan kıyasla 100 Dtu'yu sahiptir. P1 performans düzeyi P2 performans düzeyini yarım performans sağlar. Bu nedenle, P2 CPU kullanımı yüzde 50, P1, CPU kullanımı yüzde 100'e eşittir. Uygulama zaman aşımları yoksa bugün Bitti, bir işin 2 saat veya 2.5 tamamlamak için saat sürüyor olup olmaması önemli değil. Bu kategorideki bir uygulama, büyük olasılıkla P1 performans düzeyi kullanabilirsiniz. Böylece tüm "büyük yoğun" troughs birine gün içinde sığdırmaya kaynak kullanımının düşük olduğunda günde süreler olması, bir avantajlarından yararlanabilirsiniz. İşleri her gün zamanında bitirebilir sürece P1 performans düzeyi için bu türden uygulamayı (ve tasarruf), iyi olabilir.

Tüketilen kaynak bilgilerini active her veritabanı için Azure SQL veritabanı kullanıma sunan **sys.resource_stats** görünümünü **ana** her bir sunucudaki veritabanı. Tablodaki verileri için 5 dakikalık aralıklarla toplanır. Temel, standart ve Premium hizmet katmanlarıyla veri tabloda, bu verileri neredeyse gerçek zamanlı analiz yerine geçmiş çözümleme için daha yararlı olacak şekilde görünmesini fazla 5 dakika sürebilir. Sorgu **sys.resource_stats** bir veritabanının en son geçmişini görmek için görüntüleyin ve doğrulamak için mi ayırma, seçtiğiniz gerektiğinde istediğiniz performans teslim.

> [!NOTE]
> Bağlanmanız gereken **ana** mantıksal SQL veritabanı sunucunuzun sorgu için veritabanı **sys.resource_stats** ilişkin aşağıdaki örneklerde.
> 
> 

Bu örnekte, bu görünümdeki veriler nasıl sunulur gösterir:

    SELECT TOP 10 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![Sys.resource_stats Katalog görünümü](./media/sql-database-performance-guidance/sys_resource_stats.png)

Sonraki örnek, kullanabileceğiniz farklı yolları gösterir **sys.resource_stats** Katalog görünümü, SQL veritabanı kaynak kullanımı hakkında bilgi almak için:

1. Geçen hafta kaynakta aramak için veritabanı userdb1 için kullanın, bu sorguyu çalıştırabilirsiniz:
   
        SELECT *
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND
              start_time > DATEADD(day, -7, GETDATE())
        ORDER BY start_time DESC;
2. İş yükünüz performans düzeyini ne kadar iyi uyduğunu değerlendirmek için kaynak ölçümleri her yönüyle detaya gerekir: CPU, okuma, yazma, çalışan sayısı ve oturum sayısı. İşte bir düzeltilmiş kullanarak sorgu **sys.resource_stats** bu kaynak ölçümleri ortalama ve maksimum değerler bildirmek için:
   
        SELECT
            avg(avg_cpu_percent) AS 'Average CPU use in percent',
            max(avg_cpu_percent) AS 'Maximum CPU use in percent',
            avg(avg_data_io_percent) AS 'Average physical data IO use in percent',
            max(avg_data_io_percent) AS 'Maximum physical data IO use in percent',
            avg(avg_log_write_percent) AS 'Average log write use in percent',
            max(avg_log_write_percent) AS 'Maximum log write use in percent',
            avg(max_session_percent) AS 'Average % of sessions',
            max(max_session_percent) AS 'Maximum % of sessions',
            avg(max_worker_percent) AS 'Average % of workers',
            max(max_worker_percent) AS 'Maximum % of workers'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
3. Bu her kaynak ölçümü ile ortalama ve maksimum değerleri hakkında bilgi iş yükünüz seçtiğiniz performans düzeyi ile ne kadar iyi uyduğunu değerlendirebilirsiniz. Genellikle, değerleri ortalama **sys.resource_stats** karşı hedef boyutu kullanmak iyi bir temel sağlar. Bu, birincil ölçüm Sopası olmalıdır. Örneğin, standart hizmet katmanı ile performans düzeyi S2 kullanıyor olabilir. Ortalama CPU ve g/ç okuma için yüzde kullanın ve yüzde 40'a yazma olan, çalışan ortalama sayısı 50 altına olduğu ve ortalama oturum sayısı 200. İş yükünüz S1 performans düzeyine uygun olmayabilir. Veritabanınızı çalışan ve oturumu sınırları uygun olup olmadığını görmek kolay bir işlemdir. Bir veritabanı içine CPU bakımından daha düşük bir performans düzeyine uygun olup olmadığını görmek için okuma ve yazma işlemleri, geçerli performans düzeyiniz DTU sayısı daha düşük performans düzeyi DTU sayısını bölün ve ardından sonucu 100 ile çarpın:
   
    **S1 DTU / S2 DTU * 100 = 20 / 50 * 100 = 40**
   
    Yüzde cinsinden iki performans düzeyleri arasındaki göreli performans farkı sonucudur. Kaynak kullanımınızı bu miktarı aşmıyorsa, daha düşük bir performans düzeyine iş yükünüze en uygun. Ancak, kaynak kullanımı değerlerinin tüm aralıklarına arayın ve, yüzde olarak, ne sıklıkta, veritabanı iş yükünüzü daha düşük bir performans düzeyine yerleştirin belirlemek gerekir. Aşağıdaki sorgu uygun yüzdesi eşiği Biz bu örnekte hesaplanan yüzde 40'ın temel kaynak boyut başına verir:
   
        SELECT
            (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log Write Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical Data IO Fit Percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
   
    Veritabanı Hizmet düzeyi hedefi üzerinde (SLO) bağlı olarak, iş yükünüzü daha düşük bir performans düzeyine uygun olup olmadığını karar verebilirsiniz. Yüzde 99, 9, veritabanı iş yükünüzü SLO ise ve önceki sorgunun tüm üç kaynak boyutlar yüzde 99, 9'dan büyük değerleri döndürür, yükünüz büyük olasılıkla daha düşük bir performans düzeyine sığar.
   
    Ayrıca uygun yüzdesiyle arayan, SLO karşılamak için sonraki daha yüksek performans düzeyini mi taşımalısınız Öngörüler sağlar. Örneğin, geçen hafta için aşağıdaki CPU kullanımı userdb1 gösterir:
   
   | Ortalama CPU yüzdesi | En fazla CPU yüzdesi |
   | --- | --- |
   | 24.5 |100.00 |
   
    Ortalama CPU sınırı veritabanının performans düzeyini de sığması performans düzeyini çeyreği hakkındadır. Ancak, en yüksek değer, veritabanının performans düzeyini ulaştığını gösterir. Sonraki daha yüksek performans düzeyine geçmeye gerekiyor mu? Birden çok kez, iş yükü ulaştığında yüzde 100 konuları ele ve veritabanı yükünüzü SLO ardından karşılaştırır.
   
        SELECT
        (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log write fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical data IO fit percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
   
    Bu sorgu, bir değer döndürürse yüzde 99, 9'den küçük üç kaynak boyutların hiçbiri için sonraki daha yüksek performans düzeyini taşımayı deneyin veya SQL veritabanı üzerindeki yükü azaltmak için uygulama ayarlamayı tekniklerini kullanın.
4. Bu alıştırmada, tahmin edilen iş yükü artışı da gelecekte değerlendirir.

Elastik havuzlar için bu bölümde açıklanan olan tekniklerle havuzda bulunan tek veritabanlarını izleyebilirsiniz. Ancak havuzu bir bütün olarak da izleyebilirsiniz. Bilgi için bkz. [Elastik havuz izleme ve yönetme](sql-database-elastic-pool-manage-portal.md).


### <a name="maximum-concurrent-requests"></a>Maksimum eşzamanlı istekler
Eş zamanlı istek sayısını görmek için bu Transact-SQL sorgusu SQL veritabanınızda çalıştırın:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R

Şirket içi SQL Server veritabanı iş yükü analiz etmek için analiz etmek istediğiniz belirli veritabanında filtrelemek için bu sorguyu değiştirin. Örneğin, bu Transact-SQL sorgusu Veritabanım adlı bir şirket içi veritabanı varsa, bu veritabanında eş zamanlı istek sayısını döndürür:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R
    INNER JOIN sys.databases D ON D.database_id = R.database_id
    AND D.name = 'MyDatabase'

Zaman içinde bir anlık görüntü yalnızca tek bir noktada budur. İş yükü ve eş zamanlı istek gereksinimlerini daha iyi anlamak için zaman içinde çok sayıda toplamak gerekir.

### <a name="maximum-concurrent-logins"></a>En fazla eşzamanlı oturum açma
Oturumlarının sıklığı hakkında bir fikir edinmek için kullanıcı ve uygulama desenleri analiz edebilirsiniz. Ayrıca, bu ya da bu makalede ele diğer sınırlamaları aldığınızı değil emin emin olmak için bir test ortamında gerçek dünyadaki yüklerin da çalıştırabilirsiniz. Tek bir sorgu veya eşzamanlı oturum açma sayıları göstermek, dinamik yönetim görünümünü (DMV) veya geçmiş yok.

Hizmet, birden çok istemci aynı bağlantı dizesi kullanıyorsanız, her oturum açma kimliğini doğrular. Aynı anda 10 kullanıcı aynı kullanıcı adı ve parola kullanarak bir veritabanına bağlanmak, 10 eşzamanlı oturum açma olacaktır. Bu sınır, oturum açma ve kimlik doğrulama süresi geçerlidir. Eşzamanlı oturum açma sayısı hiçbir zaman 10 kullanıcıları sırayla veritabanına bağlanmak, 1'den büyük olacaktır.

> [!NOTE]
> Şu anda, bu sınır elastik havuzlardaki veritabanları için geçerli değildir.
> 
> 

### <a name="maximum-sessions"></a>En fazla oturum sayısı
Geçerli etkin oturumları sayısını görmek için bu Transact-SQL sorgusu SQL veritabanınızda çalıştırın:

    SELECT COUNT(*) AS [Sessions]
    FROM sys.dm_exec_connections

Bir şirket içi SQL Server iş yükü çözümlediğiniz, belirli bir veritabanı üzerinde odaklanmak için sorguyu değiştirin. Bu sorguyu Azure SQL veritabanı'na taşıma düşünüyorsanız veritabanı için olası oturum gereksinimlerini belirlemenize yardımcı olur.

    SELECT COUNT(*)  AS [Sessions]
    FROM sys.dm_exec_connections C
    INNER JOIN sys.dm_exec_sessions S ON (S.session_id = C.session_id)
    INNER JOIN sys.databases D ON (D.database_id = S.database_id)
    WHERE D.name = 'MyDatabase'

Yeniden, bu sorguları zaman içinde nokta sayısını döndürür. Zaman içinde birden fazla örnek toplarsanız kullanmak en iyi anlama oturumunuz sahip olacaksınız.

SQL veritabanı analiz için geçmişe dönük İstatistikler oturumları sorgulayarak alabileceğiniz [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) görüntüleme ve İnceleme **active_session_count** sütun. 

## <a name="next-steps"></a>Sonraki adımlar

- Veritabanı dizinleri ayarlama ve sorgu yürütme planlarını kullanarak otomatik olarak [Azure SQL veritabanı otomatik ayarlama](sql-database-automatic-tuning.md).
- Veritabanı performansını otomatik olarak kullanarak izleme [Azure SQL Intelligent Insights](sql-database-intelligent-insights.md). Bu özellik, tanılama bilgilerini sağlar ve kök neden analizi performans sorunlarını.
