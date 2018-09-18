---
title: Dinamik Yönetim görünümlerini kullanarak Azure SQL veritabanı izleme | Microsoft Docs
description: Algılama ve dinamik yönetim görünümlerini kullanarak Microsoft Azure SQL veritabanı izlemek için genel performans sorunlarını tanılayın öğrenin.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 08/08/2018
ms.author: carlrab
ms.openlocfilehash: 8750670f2acc41cd712254ba11b4d2ec20aa58aa
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "45981856"
---
# <a name="monitoring-azure-sql-database-using-dynamic-management-views"></a>Dinamik yönetim görünümlerini kullanarak Azure SQL Database’i izleme
Microsoft Azure SQL veritabanı, engellenen veya uzun süre çalışan sorguları, kaynak darboğazları, zayıf sorgu planlarına ve benzeri tarafından kaynaklanabilir performans sorunları tanılamak için dinamik yönetim görünümlerini kümesini sağlar. Bu konuda, dinamik yönetim görünümlerini kullanarak sık karşılaşılan performans sorunlarını algılamak nasıl hakkında bilgiler sağlar.

SQL veritabanı, kısmen dinamik yönetim görünümlerini üç kategoriye destekler:

* Veritabanı ile ilgili dinamik yönetimi görünümleri.
* Yürütme ile ilgili dinamik yönetimi görünümleri.
* İşlemle ilgili dinamik yönetimi görünümleri.

Dinamik Yönetim görünümlerini hakkında ayrıntılı bilgi için bkz. [dinamik yönetim görünümleri ve işlevleri (Transact-SQL)](https://msdn.microsoft.com/library/ms188754.aspx) SQL Server Books Online.

## <a name="permissions"></a>İzinler
SQL veritabanı'nda dinamik yönetim görünümünü sorgulama gerektiren **VIEW DATABASE STATE** izinleri. **VIEW DATABASE STATE** izin geçerli veritabanı içindeki tüm nesneler hakkında bilgi döndürür.
Vermek **VIEW DATABASE STATE** izin, belirli bir veritabanı kullanıcısı da aşağıdaki sorguyu çalıştırın:

```GRANT VIEW DATABASE STATE TO database_user; ```

Şirket içi SQL Server örneğinde, dinamik yönetim görünümlerini sunucu durumu bilgilerini döndürür. SQL veritabanı'nda, bunlar, yalnızca geçerli mantıksal veritabanı ile ilgili bilgi döndürür.

## <a name="calculating-database-size"></a>Veritabanı boyutu hesaplanıyor
Aşağıdaki sorgu veritabanınızın (megabayt cinsinden) boyutunu döndürür:

```
-- Calculates the size of the database.
SELECT SUM(CAST(FILEPROPERTY(name, 'SpaceUsed') AS bigint) * 8192.) / 1024 / 1024 AS DatabaseSizeInMB
FROM sys.database_files
WHERE type_desc = 'ROWS';
GO
```

Aşağıdaki sorgu, veritabanınızda (megabayt cinsinden) tek tek nesnelerin boyutunu döndürür:

```
-- Calculates the size of individual database objects.
SELECT sys.objects.name, SUM(reserved_page_count) * 8.0 / 1024
FROM sys.dm_db_partition_stats, sys.objects
WHERE sys.dm_db_partition_stats.object_id = sys.objects.object_id
GROUP BY sys.objects.name;
GO
```

## <a name="monitoring-connections"></a>Bağlantı izleme
Kullanabileceğiniz [sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) belirli bir Azure SQL veritabanı sunucusu ve her bağlantı ayrıntıları için kurulan bağlantılar hakkında bilgi almak için görünümü. Ayrıca, [sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) görünümü, tüm etkin kullanıcı bağlantıları ve iç görevler hakkında bilgi alırken yararlıdır.
Aşağıdaki sorgu, geçerli bağlantı hakkında bilgi alır:

```
SELECT
    c.session_id, c.net_transport, c.encrypt_option,
    c.auth_scheme, s.host_name, s.program_name,
    s.client_interface_name, s.login_name, s.nt_domain,
    s.nt_user_name, s.original_login_name, c.connect_time,
    s.login_time
FROM sys.dm_exec_connections AS c
JOIN sys.dm_exec_sessions AS s
    ON c.session_id = s.session_id
WHERE c.session_id = @@SPID;
```

> [!NOTE]
> Yürütülürken **sys.dm_exec_requests** ve **sys.dm_exec_sessions görünümleri**, varsa **VIEW DATABASE STATE** izni veritabanında, tüm çalıştırma görürsünüz. oturumlarının veritabanında; Aksi takdirde, yalnızca mevcut oturum bakın.
> 
> 

## <a name="monitor-resource-use"></a>Kaynak kullanımını izleme

Kaynak kullanımı kullanarak izleyebileceğiniz [SQL veritabanı sorgu performansı İçgörüleri](sql-database-query-performance.md) ve [Query Store](https://msdn.microsoft.com/library/dn817826.aspx).

Bu iki görünüm kullanarak kullanımını da izleyebilirsiniz:

* [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)
* [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx)

### <a name="sysdmdbresourcestats"></a>sys.dm_db_resource_stats
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

### <a name="sysresourcestats"></a>sys.resource_stats
[Sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) görünümünde **ana** veritabanı, belirli bir hizmet katmanında SQL veritabanınızın performansını izlemek ve boyutu işlem yardımcı olabilecek ek bilgiler vardır. Veriler her 5 dakikada bir toplanan ve yaklaşık 14 gün boyunca tutulur. Bu görünüm, SQL veritabanınızı kaynakları nasıl kullandığını daha uzun vadeli bir geçmiş analize yararlı olur.

Aşağıdaki grafikte CPU kaynak kullanımı için bir Premium veritabanının P2 işlem boyutu ile her saat için bir hafta içinde gösterilmektedir. Bu grafik Pazartesi günü başlar, 5 iş günü gösterir ve ardından hafta gösterir, uygulama üzerinde çok az olur.

![SQL veritabanı kaynak kullanımı](./media/sql-database-performance-guidance/sql_db_resource_utilization.png)

Verileri, bu veritabanı şu anda en yüksek CPU yükü yok. yalnızca yüzde 50'işlem CPU kullanım P2 göreli boyutu (Salı günü öğle saati). CPU uygulamanın kaynak profilinde baskın faktörü ise, P2 iş yükü her zaman en uygun olduğunu garanti etmek için doğru işlem boyutu olduğunu karar verebilirsiniz. Bir uygulama, zaman içinde artmasına bekliyorsanız, böylece uygulama hiç olmadığı kadar performans düzeyi sınırına ulaşmadığınız olmayan bir ek kaynak arabelleği sağlamak için iyi bir fikirdir. İşlem boyutu artırmak istiyorsanız, bir veritabanı istekleri özellikle gecikmeye duyarlı ortamlarda etkili bir şekilde işlemek için yeterli güce sahip olmadığında ortaya çıkabilir müşteri görünür hatalarının önüne geçmek yardımcı olabilir. Örnek Web sayfalarını veritabanı çağrıları sonuçlarına göre boyar uygulamanın desteklediği bir veritabanıdır.

Diğer uygulama türleri aynı grafikte farklı yorumlayabilir. Örneğin, uygulamanın her gün Bordro veri işlemeye çalışır ve aynı grafiğe varsa, bu tür bir "toplu" modeli P1 bilgi işlem boyutta ince ayar yapabilirsiniz. İşlem boyutu 100 Dtu'yu kıyasla 200 dtu'ya varan P2, P1 işlem boyutu vardır. P1 işlem boyutu, boyut P2 yarım performansını işlem sağlar. Bu nedenle, P2 CPU kullanımı yüzde 50, P1, CPU kullanımı yüzde 100'e eşittir. Uygulama zaman aşımları yoksa bugün Bitti, bir işin 2 saat veya 2.5 tamamlamak için saat sürüyor olup olmaması önemli değil. Bu kategorideki bir uygulama, büyük olasılıkla P1 işlem boyutu kullanabilirsiniz. Böylece tüm "büyük yoğun" troughs birine gün içinde sığdırmaya kaynak kullanımının düşük olduğunda günde süreler olması, bir avantajlarından yararlanabilirsiniz. İşleri her gün zamanında bitirebilir sürece P1 işlem boyutu için bu türden uygulamayı (ve tasarruf), iyi olabilir.

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
2. İş yükünüz işlem boyutu ne kadar iyi uyduğunu değerlendirmek için kaynak ölçümleri her yönüyle detaya gerekir: CPU, okuma, yazma, çalışan sayısı ve oturum sayısı. İşte bir düzeltilmiş kullanarak sorgu **sys.resource_stats** bu kaynak ölçümleri ortalama ve maksimum değerler bildirmek için:
   
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
3. Bu her kaynak ölçümü ile ortalama ve maksimum değerleri hakkında bilgi iş yükünüz, seçtiğiniz işlem boyuta ne kadar iyi uyduğunu değerlendirebilirsiniz. Genellikle, değerleri ortalama **sys.resource_stats** karşı hedef boyutu kullanmak iyi bir temel sağlar. Bu, birincil ölçüm Sopası olmalıdır. Örneğin, standart hizmet katmanı S2 işlem boyutu ile kullanıyor olabilir. Ortalama CPU ve g/ç okuma için yüzde kullanın ve yüzde 40'a yazma olan, çalışan ortalama sayısı 50 altına olduğu ve ortalama oturum sayısı 200. İş yükünüz S1 işlem boyutuna sığdırmaya. Veritabanınızı çalışan ve oturumu sınırları uygun olup olmadığını görmek kolay bir işlemdir. Bir veritabanı içine bir alt işlem boyutu CPU, okuma ve yazma işlemleri bölme bakımından en uygun olup olmadığını görmek için daha düşük DTU sayısını işlem boyutu, geçerli işlem boyutu DTU sayısı ve ardından sonucu 100 ile çarpın:
   
    **S1 DTU / S2 DTU * 100 = 20 / 50 * 100 = 40**
   
    İkisi arasındaki göreli performans farkı boyutları yüzde cinsinden işlem sonucudur. İş yükünüz, kaynak kullanımınızı bu miktarı aşmıyorsa, daha düşük işlem boyutuna sığdırmaya. Ancak, kaynak kullanımı değerlerinin tüm aralıklarına bakın ve belirlemek, yüzde olarak, ne sıklıkta, veritabanı iş yükünüzü daha düşük işlem boyutuna sığdırmaya gerekir. Aşağıdaki sorgu uygun yüzdesi eşiği Biz bu örnekte hesaplanan yüzde 40'ın temel kaynak boyut başına verir:
   
        SELECT
            (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log Write Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical Data IO Fit Percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
   
    Veritabanı Hizmet katmanına bağlı olarak, iş yükünüzü daha düşük işlem boyuta uygun olup olmadığını karar verebilirsiniz. Veritabanı iş yükü hedefiniz yüzde 99,9 ise ve önceki sorgunun tüm üç kaynak boyutlar yüzde 99, 9'dan büyük değerleri döndürür, yükünüz büyük olasılıkla daha düşük işlem boyutuna sığar.
   
    Ayrıca uygun yüzdesiyle bakarak hedefiniz karşılamak için sonraki daha yüksek işlem boyutu için mi taşımalısınız Öngörüler sağlar. Örneğin, geçen hafta için aşağıdaki CPU kullanımı userdb1 gösterir:
   
   | Ortalama CPU yüzdesi | En fazla CPU yüzdesi |
   | --- | --- |
   | 24.5 |100.00 |
   
    Ortalama CPU iyi veritabanının işlem boyutuna sığdırmaya işlem boyutu sınırını çeyreği hakkındadır. Ancak, veritabanı işlem boyutu sınırına ulaştığında en büyük değeri gösterir. Sonraki daha yüksek işlem boyutu için taşımanız gerekiyor mu? Birden çok kez, iş yükü ulaştığında yüzde 100 konuları ele ve ardından veritabanı iş yükü hedefiniz karşılaştırır.
   
        SELECT
        (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log write fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical data IO fit percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
   
    Bu sorgu, bir değer döndürürse yüzde 99, 9'den küçük üç kaynak boyutların hiçbiri için sonraki daha yüksek işlem boyutu taşımayı deneyin veya SQL veritabanı üzerindeki yükü azaltmak için uygulama ayarlamayı tekniklerini kullanın.
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

## <a name="monitoring-query-performance"></a>Sorgu performansını izleme
Yavaş veya uzun süre çalışan sorguların önemli sistem kaynakları kullanabilir. Bu bölümde, birkaç ortak sorgu performansı sorunlarını algılamak için dinamik yönetim görünümlerini nasıl yapılacağı açıklanır. Sorun giderme, eski ancak yine de yararlı bir başvuru [SQL Server 2008'de performans sorunlarını giderme](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) Microsoft TechNet makalesi.

### <a name="finding-top-n-queries"></a>İlk N sorgularını bulma
Aşağıdaki örnek, ortalama CPU süresine göre sıralanmış ilk beş sorgu hakkındaki bilgileri döndürür. Mantıksal eşdeğer sorgular tarafından toplam kaynak tüketimi gruplanması Bu örnekte sorgu, sorgu karmasına göre toplar.

```
SELECT TOP 5 query_stats.query_hash AS "Query Hash",
    SUM(query_stats.total_worker_time) / SUM(query_stats.execution_count) AS "Avg CPU Time",
    MIN(query_stats.statement_text) AS "Statement Text"
FROM
    (SELECT QS.*,
    SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
    ((CASE statement_end_offset
        WHEN -1 THEN DATALENGTH(ST.text)
        ELSE QS.statement_end_offset END
            - QS.statement_start_offset)/2) + 1) AS statement_text
     FROM sys.dm_exec_query_stats AS QS
     CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
GROUP BY query_stats.query_hash
ORDER BY 2 DESC;
```

### <a name="monitoring-blocked-queries"></a>Engellenen sorguları izleme
Yavaş veya uzun süre çalışan sorgular, aşırı kaynak tüketimi için katkıda bulunan ve engellenen sorguların sonucu olabilir. Engelleme nedeni, zayıf uygulama tasarımı, hatalı sorgu planlarına, kullanışlı dizinleri ve benzeri olmaması olabilir. Azure SQL veritabanı'nda geçerli kilitleme etkinliği hakkında bilgi almak için sys.dm_tran_locks görünümü kullanabilirsiniz. Örnek kod için bkz: [sys.dm_tran_locks (Transact-SQL)](https://msdn.microsoft.com/library/ms190345.aspx) SQL Server Books Online.

### <a name="monitoring-query-plans"></a>Sorgu planlarına izleme
Bir verimsiz bir sorgu planı, CPU tüketimi da artırabilir. Aşağıdaki örnekte [sys.dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) hangi sorgu toplu en çok CPU kullanan belirlemek için görüntüleme.

```
SELECT
    highest_cpu_queries.plan_handle,
    highest_cpu_queries.total_worker_time,
    q.dbid,
    q.objectid,
    q.number,
    q.encrypted,
    q.[text]
FROM
    (SELECT TOP 50
        qs.plan_handle,
        qs.total_worker_time
    FROM
        sys.dm_exec_query_stats qs
    ORDER BY qs.total_worker_time desc) AS highest_cpu_queries
    CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS q
ORDER BY highest_cpu_queries.total_worker_time DESC;
```

## <a name="see-also"></a>Ayrıca bkz.
[SQL Database'e giriş](sql-database-technical-overview.md)

