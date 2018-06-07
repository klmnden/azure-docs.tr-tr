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
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: 44d68d69a7034e80846fb44f3ae26c0d73c61f28
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34648318"
---
# <a name="monitoring-database-performance-in-azure-sql-database"></a>Azure SQL Database'de veritabanı performansını izleme
Azure SQL veritabanı performansını izlemeye, seçtiğiniz veritabanı performans düzeyiyle ilgili kaynak kullanımını izleyerek başlarsınız. İzleme yardımcı olur, veritabanınızın gerekenden fazla kapasiteye sahip veya çıkışı kaynak kullanımının çünkü zorlanıyor belirlemek ve ardından veritabanınızda katmanlarını hizmet ve performans düzeyini ayarlamak için zaman olup olmadığına karar vermek [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) veya [vCore tabanlı satın alma modeli (Önizleme)](sql-database-service-tiers-vcore.md). Veritabanınızı [Azure portalında](https://portal.azure.com) bulunan grafik araçlarını veya SQL [dinamik yönetim görünümlerini](https://msdn.microsoft.com/library/ms188754.aspx) kullanarak izleyebilirsiniz.

> [!TIP]
> Kullanım [Azure SQL akıllı Öngörüler](sql-database-intelligent-insights.md) otomatik veritabanı performansını izleme. Bir performans sorunu algılandığında tanılama günlük ayrıntıları ve sorunun kök neden analizi (RCA) ile oluşturulur. Performansı geliştirme öneri, mümkün olduğunda sağlanır.
>

## <a name="monitor-databases-using-the-azure-portal"></a>Azure portalını kullanarak veritabanlarını izleme
[Azure portalında](https://portal.azure.com/), tek veritabanlarının kullanımını veritabanınızı seçip **İzleme** grafiğine tıklayarak izleyebilirsiniz. Bu işlem sonrasında bir **Ölçüm** penceresi görüntülenir. **Grafiği düzenle** düğmesine tıklayarak değişiklik yapabilirsiniz. Şu ölçümleri ekleyin:

* CPU yüzdesi
* DTU yüzdesi
* Veri G/Ç yüzdesi
* Veri boyutu yüzdesi

Bu ölçümleri ekledikten sonra görüntülemeye devam edebilirsiniz **izleme** grafik hakkında daha fazla bilgi **ölçüm** penceresi. Dört ölçümün tümü de veritabanınızın ortalama **DTU** kullanım yüzdesini gösterir. Bkz: [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [vCore tabanlı satın alma modeli (Önizleme)](sql-database-service-tiers-vcore.md) makaleler hizmet katmanları hakkında daha fazla bilgi için.  

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

Kaynak kullanım kullanarak izleyebilirsiniz [SQL veritabanı sorgu performansı öngörüleri](sql-database-query-performance.md) ve [Query Store](https://msdn.microsoft.com/library/dn817826.aspx).

Bu iki görünüm kullanarak kullanımı da izleyebilirsiniz:

* [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)
* [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx)

#### <a name="sysdmdbresourcestats"></a>sys.dm_db_resource_stats
Kullanabileceğiniz [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) her SQL veritabanını görünümünde. **Sys.dm_db_resource_stats** son kaynak kullanım verilerini hizmet katmanına göre görüntüler. Ortalama CPU, veri g/ç, günlük yazma ve bellek yüzdelerini 15 dakikada kaydedilir ve 1 saat boyunca saklanır.

Bu görünüm kaynak kullanımını daha ayrıntılı bir bakış sağladığından, kullanın **sys.dm_db_resource_stats** tüm geçerli durumu çözümleme için ilk ya da sorun giderme. Örneğin, bu sorgu geçerli veritabanı için ortalama ve en fazla kaynak kullanımı son bir saat içinde gösterilmektedir:

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
[Sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) görünümünde **ana** veritabanının bulunduğu SQL veritabanınızın belirli hizmet katmanını ve performans düzeyinde performansını izlemenize yardımcı olacak ek bilgiler. Veriler her 5 dakikada bir toplanan ve yaklaşık 14 gün boyunca tutulur. Bu görünüm, SQL veritabanınız kaynakları nasıl kullandığını, daha uzun vadeli bir geçmiş analize kullanışlıdır.

Aşağıdaki grafikte CPU kaynak kullanımı bir Premium veritabanı için P2 performans düzeyine sahip her saat için bir hafta içinde gösterilmiştir. Bu grafik Pazartesi günü başlar, 5 iş günlerini gösterir ve hafta sonu gösterir, uygulamaya göre çok daha az olur.

![SQL veritabanı kaynak kullanımı](./media/sql-database-performance-guidance/sql_db_resource_utilization.png)

Verileri, bu veritabanı şu anda yüzde 50'ten sadece en yüksek CPU yükü yok (Öğle saati Salı günü) P2 performans düzeyine göre CPU kullanımı. CPU uygulamanın kaynak profilinde baskın faktörü ise, P2 iş yükü her zaman uygun güvence altına almak için doğru performans düzeyinin olduğunu karar verebilirsiniz. Bir uygulama zamanla artmasına bekliyorsanız, böylece uygulama herhangi bir zamanda performans düzeyi sınırına ulaştığında olmayan bir ek kaynak arabelleği sağlamak için bir fikirdir. Performans düzeyini artırmak için bir veritabanı isteklerini özellikle gecikme süresine duyarlı ortamlarında etkili bir şekilde işlemek için yeterli güç sahip olmadığında ortaya çıkabilir müşteri görünür hataları önlemenize yardımcı olur. Örnek Web sayfalarını veritabanı çağrılarını sonuçlarına dayalı boyar uygulamanın destekleyen bir veritabanıdır.

Diğer uygulama türleriyle aynı grafikte farklı yorumlayabilir. Örneğin, bir uygulama her gün bordro verileri işlemek çalışır ve aynı grafik varsa, bu tür bir "toplu" modeli P1 performans düzeyi ince uygulayabilirsiniz. 200 Dtu'lar P2 performans düzeyinde karşılaştırılan 100 Dtu'lar P1 performans düzeyi vardır. P1 performans düzeyi P2 performans düzeyinin yarım performans sağlar. Bu nedenle, yüzde 50 ' P2 CPU kullanımı yüzde 100 CPU P1 kullanımda eşittir. Uygulama zaman aşımları yoksa, bir iş 2 saat veya tamamlamak için 2.5 saat sürerse bugün Bitti, önemli değil. Bu kategorideki bir uygulama, büyük olasılıkla P1 performans düzeyi kullanabilirsiniz. Böylece tüm "büyük yoğun" troughs birine gün içinde sığdırmaya kaynak kullanımının düşük olduğunda gün sırasında süreler olduğunu olayının avantajından yararlanabilirsiniz. İşleri her gün saat bitirebilirsiniz sürece P1 performans düzeyi için bu türden uygulamasının (ve tasarruf) iyi olabilir.

Azure SQL veritabanı çıkarır tüketilen etkin her veritabanı için kaynak bilgileri **sys.resource_stats** görünümünü **ana** her sunucu veritabanında. Tablodaki verileri için 5 dakikalık aralıklarla toplanır. Temel, standart ve Premium hizmet katmanları ile verileri bu verileri neredeyse gerçek zamanlı analiz yerine geçmiş çözümleme için daha yararlı olacak şekilde tabloda görünmesi birden fazla 5 dakika sürebilir. Sorgu **sys.resource_stats** bir veritabanı yakın zamandaki geçmişini görmek için görüntülemek ve doğrulamak için mi ayırma seçtiğiniz gerektiğinde istediğiniz performans teslim.

> [!NOTE]
> Bağlanmanız gerekir **ana** mantıksal SQL veritabanı sunucunuzun sorgu için veritabanı **sys.resource_stats** aşağıdaki örneklerde.
> 
> 

Bu örnek nasıl verileri bu görünümde gösterilen gösterir:

    SELECT TOP 10 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![Sys.resource_stats Katalog görünümü](./media/sql-database-performance-guidance/sys_resource_stats.png)

Sonraki örnek kullanabileceğiniz farklı yolları gösterir **sys.resource_stats** katalog SQL veritabanınız kaynakları nasıl kullandığı hakkında bilgi almak için görünümü:

1. Geçen hafta kaynakta aramak için bu sorguyu çalıştırmak için veritabanı userdb1 kullanın:
   
        SELECT *
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND
              start_time > DATEADD(day, -7, GETDATE())
        ORDER BY start_time DESC;
2. İş yükünüzün performans düzeyine ne kadar iyi uyduğunu değerlendirmek için her kaynak ölçümleri yönü detaya gerekir: CPU, okuma, yazma, çalışan sayısı ve oturum sayısı. İşte bir düzeltilmiş kullanarak sorgu **sys.resource_stats** bu kaynak ölçümleri ortalama ve en yüksek değerleri bildirmek için:
   
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
3. Bu her kaynak ölçümü ile ortalama ve en yüksek değerleri hakkında bilgi yükünüzü seçtiğiniz performans düzeyi ne kadar iyi uyduğunu değerlendirebilirsiniz. Genellikle, değerleri ortalama **sys.resource_stats** karşı hedef boyutu kullanmak için iyi bir temel sağlar. Birincil ölçüm çubuğu olmalıdır. Bir örnek için standart hizmet katmanında S2 performans düzeyiyle kullanıyor olabilir. Ortalama CPU ve g/ç okuma yüzdeleri kullanın ve yazma 40 oranın altında olan, çalışan ortalama sayısı 50 olduğu ve ortalama oturum sayısını 200. İş yükünüzün S1 performans düzeyine uygun. Veritabanı içinde çalışan ve oturumu sınırları uygun olup olmadığını görmek kolaydır. Bir veritabanı içine CPU göre daha düşük bir performans düzeyine uygun olup olmadığını görmek için okuma ve yazma, geçerli performans düzeyi DTU sayısına göre daha düşük performans düzeyine DTU sayısı ayırın ve ardından sonucu 100 ile çarpın:
   
    **S1 DTU / S2 DTU * 100 = 20 / 50 * 100 = 40**
   
    Yüzde cinsinden iki performans düzeyleri arasında göreceli performans farkı sonucudur. Kaynak kullanımınız bu miktar aşmadığından, iş yükünüzü daha düşük performans düzeyine uygun. Ancak, tüm kaynak kullanım değerleri aralıklarını arayın ve belirlemek, yüzde olarak, veritabanının yükünüzü daha düşük performans düzeyine ne sıklıkta uyar gerekir. Aşağıdaki sorguyu kaynak boyutu, 40 oranında Biz bu örnekte, hesaplanan eşik göre başına uygun yüzde çıkarır:
   
        SELECT
            (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log Write Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical Data IO Fit Percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
   
    Veritabanı Hizmet düzeyi hedefi (SLO) bağlı olarak, iş yükünüzü daha düşük performans düzeyine uygun olup olmadığını karar verebilirsiniz. Veritabanının yükünüzü SLO yüzde 99,9 ise ve önceki sorgunun değerlerinin tüm üç kaynak boyutları için yüzde 99,9 büyük döndürür, büyük olasılıkla, iş yükünü daha düşük performans düzeyine sığar.
   
    Ayrıca uygun yüzdesiyle arayan, SLO karşılamak için sonraki daha yüksek performans düzeyine olup taşımalısınız içine Insight sağlar. Örneğin, userdb1 geçen hafta için aşağıdaki CPU kullanımını gösterir:
   
   | Ortalama CPU yüzdesi | En fazla CPU yüzdesi |
   | --- | --- |
   | 24.5 |100.00 |
   
    Ortalama CPU sınırı veritabanı performans düzeyini de uyar performans düzeyini çeyreği hakkındadır. Ancak, veritabanı performans düzeyi sınırına ulaştığında en büyük değeri gösterir. Sonraki daha yüksek performans düzeyine geçmeye gerekiyor mu? Ne birçok kez iş yükü ulaştığında yüzde 100 arayın ve veritabanı yükünüzü SLO karşılaştırın.
   
        SELECT
        (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log write fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical data IO fit percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
   
    Bu sorgu bir değer döndürürse yüzde 99,9'den az üç kaynak boyutlardan herhangi birinde için sonraki daha yüksek performans düzeyine taşımayı düşünün veya SQL veritabanı üzerindeki yükü azaltmak için uygulama ayarlama tekniklerini kullanın.
4. Bu alıştırmada ayrıca tahmini iş yükü artış gelecekte göz önünde bulundurur.

Elastik havuzlar için bu bölümde açıklanan olan tekniklerle havuzda bulunan tek veritabanlarını izleyebilirsiniz. Ancak havuzu bir bütün olarak da izleyebilirsiniz. Bilgi için bkz. [Elastik havuz izleme ve yönetme](sql-database-elastic-pool-manage-portal.md).


### <a name="maximum-concurrent-requests"></a>Maksimum eşzamanlı istek
Eşzamanlı istek sayısını görmek için bu Transact-SQL sorgusu SQL veritabanınızda çalıştırın:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R

Bir şirket içi SQL Server veritabanının iş yükü çözümlemek için çözümlemek istediğiniz belirli veritabanı üzerinde filtre uygulamak için bu sorguyu değiştirin. Örneğin, Veritabanım adlı bir şirket içi veritabanınız varsa, bu Transact-SQL sorgusu bu veritabanında eşzamanlı istek sayısını döndürür:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R
    INNER JOIN sys.databases D ON D.database_id = R.database_id
    AND D.name = 'MyDatabase'

Bu, zaman içinde yalnızca tek bir noktada anlık görüntüsüdür. İş yükü ve eş zamanlı istek gereksinimlerini daha iyi anlamak için zaman içinde çok sayıda toplamak gerekir.

### <a name="maximum-concurrent-logins"></a>En fazla eşzamanlı oturum açma
Oturum açma bilgileri sıklığını hakkında bir fikir edinmek için kullanıcı ve uygulama desenleri analiz edebilirsiniz. Bu veya bu makalede aşağıdakiler ele diğer sınırları basarsa değil olduğundan emin olmak için bir sınama ortamında gerçek yükleri de çalıştırabilirsiniz. Tek bir sorgu veya dinamik yönetim görünümünü (DMV), eşzamanlı oturum açma sayar gösterebilir veya geçmişi yok.

Hizmet birden çok istemci aynı bağlantı dizesini kullanırsanız, her oturum açma kimliğini doğrular. 10 kullanıcılar aynı anda bir veritabanına aynı kullanıcı adı ve parola kullanarak bağlanıyorsa, 10 eşzamanlı oturum açma olacaktır. Bu sınır, oturum açma ve kimlik doğrulama süresi uygulanır. Aynı 10 kullanıcılar veritabanına sırayla bağlanıyorsa, eşzamanlı oturum açma sayısı hiçbir zaman 1'den büyük olacaktır.

> [!NOTE]
> Şu anda, bu sınır esnek havuzlar veritabanları için geçerli değildir.
> 
> 

### <a name="maximum-sessions"></a>En fazla oturum
Geçerli etkin oturum sayısı görmek için bu Transact-SQL sorgusu SQL veritabanınızda çalıştırın:

    SELECT COUNT(*) AS [Sessions]
    FROM sys.dm_exec_connections

Bir şirket içi SQL Server iş yükü analiz etme, belirli bir veritabanında odaklanmak için sorguyu değiştirin. Bu sorgu, Azure SQL veritabanına taşıma düşünüyorsanız veritabanı için olası oturum gereksinimlerini belirlemenize yardımcı olur.

    SELECT COUNT(*)  AS [Sessions]
    FROM sys.dm_exec_connections C
    INNER JOIN sys.dm_exec_sessions S ON (S.session_id = C.session_id)
    INNER JOIN sys.databases D ON (D.database_id = S.database_id)
    WHERE D.name = 'MyDatabase'

Yeniden, bu sorguları zaman içinde nokta sayısını döndürür. Zaman içinde birden fazla örnek toplarsanız kullanmak en iyi anlaşılmasını oturumunuz sahip olacaksınız.

SQL veritabanı analize geçmiş istatistikleri oturumlarını sorgulayarak alabileceğiniz [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) Görünüm ve gözden geçirme **active_session_count** sütun. 

## <a name="next-steps"></a>Sonraki adımlar

- Otomatik olarak veritabanı dizinlerini ayarlamak ve yürütme planları kullanarak sorgu [Azure SQL veritabanı otomatik ayarlama](sql-database-automatic-tuning.md).
- Otomatik olarak kullanarak veritabanı performansını izleme [Azure SQL akıllı Öngörüler](sql-database-intelligent-insights.md). Bu özellik, tanılama bilgilerini sağlar ve kök neden performans sorunlarını çözümleme.
