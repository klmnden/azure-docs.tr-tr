---
title: İzleme ve performans ayarlama - Azure SQL veritabanı | Microsoft Docs
description: Performans değerlendirmesi ve geliştirme ile Azure SQL veritabanı'nda ayarlama için ipuçları.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jrasnik, carlrab
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 2fa43fcd48736a3d044deb07ed690af580c3b987
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66416284"
---
# <a name="monitoring-and-performance-tuning"></a>İzleme ve performans ayarlama

Kolayca kullanımını izlemek için ekleyip kaynakları (CPU, bellek, g/ç), azure SQL veritabanı sağlar, olası sorunları gidermek ve veritabanınızın performansı artırmak önerileri bulabilirsiniz. Azure SQL veritabanı, iş yükünüze uyum ve otomatik olarak performansı en iyi duruma veritabanı izin vermek istiyorsanız, veritabanlarınızda sorunları otomatik olarak düzeltebilir çok sayıda özelliğe sahiptir. Ancak, sorun giderme için ihtiyacınız olabilecek bazı özel sorunları vardır. Bu makalede, bazı en iyi yöntemler ve performans sorunlarını gidermek için kullanabileceğiniz araçlar açıklanmaktadır.

Bu, veritabanı sorunsuz çalıştığından emin olmak için gerçekleştirmeniz gereken iki ana etkinlik vardır:
- [Veritabanı performansını izleme](#monitoring-database-performance) veritabanına atanan kaynakları, iş yükü işleyebileceğinden emin olmak için. Kaynak sınırlarını ulaşma görürseniz, en çok kaynak tüketen sorguları tanımlamak ve bunları en iyi duruma getirmek için ya da hizmet katmanına yükselterek daha fazla kaynak eklemeniz gerekir.
- [Performans sorunlarını giderme](#troubleshoot-performance-issues) bazı olası bir sorun oluştu. nedeni belirlemek için kök nedenini sorunu ve sorunu düzeltir eylem tanımlayın.

## <a name="monitoring-database-performance"></a>Veritabanı performansını izleme

Azure SQL veritabanı performansını izlemeye, seçtiğiniz veritabanı performans düzeyiyle ilgili kaynak kullanımını izleyerek başlarsınız. Aşağıdaki kaynaklar izlemeniz gereken:
 - **CPU kullanımı** -olan erişmeye CPU kullanımının %100 daha uzun bir süre içinde denetlemek gerekir. Bu, veritabanı veya örnek yükseltmeniz veya tanımlayın ve çoğu işlem gücü kullanan sorgularınızı ayarlamak gerekebilir olduğunu gösteriyor olabilir.
 - **İstatistikleri bekleyin** -ne denetlemek gereken bazı kaynaklar için bekleyen sorgularınızı neden. Queriesmig veri getirilen ya da kaydedilmiş bazı kaynak sınırına ulaştığınız için bekleyen veritabanı dosyaları, vb. için bekle.
 - **GÇ kullanımını** -olan erişmeye temel depolama GÇ sınırlarından denetlemek gerekir.
 - **Bellek kullanımı** -veritabanı veya örnek orantılı sanal çekirdek sayısı ve kontrol etmeniz için kullanılabilir bellek yeterli iş yükünüz için olduğu. Sayfa yaşam beklentisinin sayfalarınızı hızlı bir şekilde bellekten kaldırıldığını gösteren parametreleri biridir.

Azure SQL veritabanı **yardımcı olabilecek bildirimleri sorun giderme ve olası performans sorunlarını çözün sağlar**. Artırmak ve kaynakları değiştirmeden geçirerek sorgu performansının iyileştirilmesi için fırsatlar kolayca belirleyebilirsiniz [performans ayarlama önerilerinde](sql-database-advisor.md). Veritabanı performansının düşük olmasına yol açan yaygın nedenler, dizinlerin eksik olması ve sorguların hatalı bir şekilde iyileştirilmesidir. İş yükünüzün performansını artırmak için bu ayar önerileri uygulayabilirsiniz. Let Azure SQL veritabanı'na ayrıca [otomatik olarak, sorguların performansını en iyi duruma getirme](sql-database-automatic-tuning.md) uygulayarak tüm öneriler ve veritabanı performansını artırmak doğrulama belirledik.

İzleme ve sorun giderme veritabanı performans için aşağıdaki seçenekleriniz:

- İçinde [Azure portalında](https://portal.azure.com), tıklayın **SQL veritabanları**, veritabanını seçin ve ardından aramak için en fazla yaklaşan kaynakları için izleme grafiği kullanın. DTU tüketimi, varsayılan olarak gösterilir. Tıklayın **Düzenle** gösterilen değerler ve zaman aralığını değiştirmek için.
- SQL Server Management Studio gibi araçlar sağlayan birçok yararlı raporları bir [performans Pano](https://docs.microsoft.com/sql/relational-databases/performance/performance-dashboard?view=sql-server-2017) kaynak kullanımını izlemek ve üst kaynak kullanan sorgular, tanımlamak veya [Query Store](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store#Regressed)yeri belirleyebilirsiniz sorguları azaltılmış performansa.
- Kullanım [sorgu performansı İçgörüleri](sql-database-query-performance.md) [Azure portalında](https://portal.azure.com) kaynaklarının en çok harcama sorguları tanımlamak için. Bu özellik, yalnızca tek veritabanı ve elastik havuzların içinde kullanılabilir.
- Kullanım [SQL veritabanı Danışmanı](sql-database-advisor-portal.md) oluşturmak ve dizinleri bırakmayı, sorguları kümesini parametreleştirme ve şema sorunlarını giderme önerileri görüntüleyebilirsiniz. Bu özellik, yalnızca tek veritabanı ve elastik havuzların içinde kullanılabilir.
- Kullanım [Azure SQL Intelligent Insights](sql-database-intelligent-insights.md) otomatik, veritabanı performansını izleme. Bir performans sorunu algılandığında bir tanılama günlüğü, Ayrıntılar ve sorunun kök neden analizi (RCA) ile oluşturulur. Mümkün olduğunda performans iyileştirme öneri sağlanmaktadır.
- [Otomatik ayarlamayı etkinleştirme](sql-database-automatic-tuning-enable.md) ve Azure SQL tanımlanan düzeltme performans sorunlarını otomatik olarak veritabanı sağlar.
- Kullanım [dinamik yönetim görünümlerini (Dmv'ler)](sql-database-monitoring-with-dmvs.md), [genişletilmiş olaylar](sql-database-xevent-db-diff-from-svr.md)ve [Query Store](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) daha ayrıntılı performans sorunlarını gidermek için.

> [!TIP]
> Bkz: [performans rehberi](sql-database-performance-guidance.md) tanımlayan bir veya daha yukarıdaki yöntemlerden birini kullanarak performans sorunu sonra Azure SQL veritabanı performansını artırmak için kullanabileceğiniz teknikleri bulunacak.

## <a name="troubleshoot-performance-issues"></a>Performans sorunlarını giderme

Performans sorunları tanılamak ve gidermek için her etkin sorgu ve her iş yükünün durumuna ilgili performans sorunlarına neden olan koşulları durumu anlayarak başlayın. Azure SQL veritabanı performansını artırmak için her etkin sorgu isteği uygulamanızdan bir çalışan veya bekleme durumunda olduğunu anlayın. Azure SQL veritabanında bir performans sorunu giderirken aşağıdaki grafikte, performans sorunları tanılamak ve gidermek için bu makaleyi okuyun olarak göz önünde bulundurun.

![İş yükü durumları](./media/sql-database-monitor-tune-overview/workload-states.png)

Performans sorunları olan bir iş yükü için performans sorunu nedeniyle CPU Çekişme olabilir (bir **çalıştırma ile ilgili** koşul) veya tek tek sorgular üzerinde bir bekleyen (bir **bekleme ilgili** koşulu ).

Neden veya **çalıştırma ile ilgili** sorunları olabilir:
- **Derleme sorunlarını** -SQL sorgu iyileştiricisi, eski istatistikleri, işlenecek satır sayısı yanlış tahmin veya tahmin gerekli bellek nedeniyle iyinin planı üretebilir. Bu sorgu, daha hızlı geçmiş veya başka örneğinde (yönetilen örneğine veya SQL Server örneği) yürütülmesi biliyorsanız, gerçek yürütmesi planları ve bunları görmek için oldukları karşılaştırma farklı yararlanın. Sorgu ipuçları veya yeniden istatistikleri veya dizinleri kadar iyi planlama almak için uygulamayı deneyin. Otomatik plan düzeltme otomatik olarak bu sorunları hafifletmek için Azure SQL veritabanı'nda etkinleştirin.
- **Yürütme sorunları** - sorgu planı en iyi olduğunu sonra büyük olasılıkla veritabanında günlük yazma üretimi gibi bazı kaynak sınırlarını ulaşma veya oluşturulmalıdır birleştirilmiş dizinleri kullanıyor olabilir. Çok sayıda kaynakları harcama eş zamanlı sorgu yürütme sorunların nedenini de olabilir. **Bekleyen ilgili** sorunlardır yürütme sorunlarıyla ilgili çalışmalarının çoğu için verimli bir şekilde yürütülmüyor sorguları büyük olasılıkla bazı kaynaklar bekleniyor.

Neden veya **bekleme ilgili** sorunları olabilir:
- **Engelleme** -bir sorgu tutun kilit veritabanındaki bazı nesneler başkalarının aynı nesneleri erişmeye çalışırken. Engelleme sorguları DMV'sini kullanarak veya izleme araçları kolayca belirleyebilirsiniz.
- **G/ç sorunlarını** -sorgular bekleyen veri veya günlük dosyalarına yazılacak sayfaları için. Bu durumda görürsünüz `INSTANCE_LOG_RATE_GOVERNOR`, `WRITE_LOG`, veya `PAGEIOLATCH_*` DMV istatistikleri bekleyin.
- **TempDB sorunları** - çok sayıda geçici tablolar kullanıyorsanız veya çok fazla tempdb sıvı sıçraması planlarınızda TempDB aktarım hızı ile ilgili bir sorun olabilir, sorgularınızı bakın. 
- **Bellekle ilgili sorunları** -, yeterli bellek, iş yükü için sayfa yaşam beklentisinin bırak ya da sorgularınızı gerekenden daha az bellek ataması alıyorsanız sahip olmayabilir. Bazı durumlarda, yerleşik zeka sorgu iyileştiricisi, bu sorunları düzeltir.
 
Aşağıdaki bölümlerde, belirleme ve bu sorunlardan bazılarını gidermenize açıklanacaktır.

## <a name="running-related-performance-issues"></a>Çalıştırma ile ilgili performans sorunları

Genel bir kural olarak, CPU kullanımı veya % 80'de, üzerindeki tutarlı bir şekilde ise bir çalıştırma ile ilgili performans sorunu gerekir. Çalışan ilgili bir sorun varsa, CPU kaynakları yetersiz olabilir veya bunu ilgili aşağıdaki koşullardan biri olabilir:

- Çok fazla sayıda çalışan sorguları
- Çok fazla derleme sorguları
- Bir veya daha çok yürütülen sorguları optimum sorgu planı kullanıyorsanız

Bir çalıştırma ile ilgili performans sorunu olduğunu belirlerseniz, bir veya daha fazla yöntemi kullanarak kesin sorunu tanımlamak için amacınız anlamaktır. Çalıştırma ile ilgili sorunları tanımlamaya yönelik en yaygın yöntemler şunlardır:

- Kullanım [Azure portalında](sql-database-manage-after-migration.md#monitor-databases-using-the-azure-portal) CPU yüzdesi kullanımı izlemek için.
- Aşağıdaki [dinamik yönetim görünümlerini](sql-database-monitoring-with-dmvs.md):

  - [sys.dm_db_resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) bir Azure SQL veritabanı için CPU, g/ç ve bellek tüketimi döndürür. Veritabanında hiç etkinlik olsa her 15 saniyede bir satır yok. Geçmiş verileri, bir saat boyunca korunur.
  - [sys.resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) bir Azure SQL veritabanı için CPU kullanımı ve depolama verilerini döndürür. Veriler toplanır ve beş dakikalık aralıklarla içinde toplanır.

> [!IMPORTANT]
> Bir kümesi için bkz: CPU kullanımı sorunlarını gidermek için bu Dmv'leri kullanarak bir T-SQL sorguları [tanımlamak CPU performans sorunlarını](sql-database-monitoring-with-dmvs.md#identify-cpu-performance-issues).

### <a name="ParamSniffing"></a> Sorgu parametresi duyarlı sorgu yürütme planı sorunları giderme

Parametre hassas planı (PSP) sorunu burada yalnızca bir özel parametre değeri (veya değerlerin kümesini) en iyi bir sorgu yürütme planı sorgu iyileştiricisi oluşturur ve önbelleğe alınan plan kullanılan parametre değerleri için uygun olmayan durumda bir senaryo gösterir birbirini izleyen yürütmeleri. Uygun olmayan planları sorgu performansı sorunlarını ve genel iş yükü performans düşüşüne neden olabilir. Parametre algılaması ve sorgu işleme hakkında daha fazla bilgi için bkz. [sorgu işleme Mimarisi Kılavuzu](/sql/relational-databases/query-processing-architecture-guide#ParamSniffing).

Her ilişkili ödünler ve dezavantajları sorunları gidermek için kullanılan birkaç geçici çözümler vardır:

- Kullanım [DERLEMENİZ](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) her sorgu yürütme, sorgu ipucu. Bu geçici çözüm derleme zamanı ve daha iyi planı kalitesi için daha yüksek CPU arasında denge kurar. Kullanarak `RECOMPILE` seçeneği genellikle yüksek performans gerektiren iş yükleri için mümkün değildir.
- Kullanım [seçeneği (için İYİLEŞTİR...) ](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) parametre değeri olanaklar çoğu için yeterince iyi bir plan üreten tipik bir parametre değeri ile gerçek parametre değeri geçersiz kılmak üzere sorgu ipucu.   Bu seçenek en iyi parametre değerleri ve ilişkili planı özelliklerini iyi bir anlayış gerektirir.
- Kullanım [(en iyi duruma getirme için bilinmeyen seçenek)](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) yoğunluklu vektör ortalama kullanarak lisanslarınıza gerçek parametre değeri geçersiz kılmak üzere sorgu ipucu. Bunu yapmak için başka bir yerel değişkene gelen parametre değerlerini yakalamak ve ardından parametreleri kullanmak yerine koşullarına yerel değişkenler kullanarak yoludur. Ortalama sıklık olmalıdır *yeterince iyi* bu belirli düzeltme.
- Parametresi yalnızca kullanarak algılaması devre dışı [DISABLE_PARAMETER_SNIFFING](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) sorgu ipucu.
- Kullanım [KEEPFIXEDPLAN](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) önlemek üzere sorgu ipucu yeniden derler önbelleğinde sırada. Bu geçici çözüm varsayar *yeterince iyi* ortak planı, bir önbellek zaten. Ayrıca otomatik güncelleştirmeler istatistiklerini çıkarılıyor iyi planı ve derlenen yeni hatalı bir plan olasılığını azaltmak için devre dışı.
- Açıkça kullanarak, planı zorlamanıza [USE PLAN](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) sorgu İpucu (açıkça belirterek, Query Store kullanarak belirli bir plana ayarlayarak veya etkinleştirerek [otomatik ayarlama](sql-database-automatic-tuning.md).
- Tek bir yordam her kullanılabilir koşullu mantık ve ilişkili parametre değerleri temel alan yordamları iç içe bir dizi değiştirin.
- Dinamik dize yürütme alternatifleri statik yordamı tanımı oluşturun.

Bu tür sorunları giderme hakkında ek bilgi için bkz:

- Bu [ben bir parametre smell](https://blogs.msdn.microsoft.com/queryoptteam/2006/03/31/i-smell-a-parameter/) blog gönderisi
- Bu [parametreli sorgular için plan kalite karşı dinamik sql](https://blogs.msdn.microsoft.com/conor_cunningham_msft/2009/06/03/conor-vs-dynamic-sql-vs-procedures-vs-plan-quality-for-parameterized-queries/) blog gönderisi
- Bu [SQL Server'da SQL sorgu iyileştirme teknikleri: Parametre Sniffing](https://www.sqlshack.com/query-optimization-techniques-in-sql-server-parameter-sniffing/) blog gönderisi

### <a name="troubleshooting-compile-activity-due-to-improper-parameterization"></a>Hatalı Parametreleştirme nedeniyle etkinlik derleme sorunlarını giderme

Bir sorgu hazır olduğunda, otomatik olarak da ifade parametre haline getirmek veritabanı altyapısı seçer veya bir kullanıcı açıkça bu derler sayısını azaltmak için parametreleştirebilirsiniz. Çok sayıda aynı desene ancak farklı değişmez değerleri kullanarak bir sorgunun derler içinde yüksek CPU kullanımına neden olabilir. Değişmez değerleri için devam eden bir sorgu yalnızca kısmen Parametreleştirme, benzer şekilde, veritabanı altyapısı, daha fazla parametrelemez.  Kısmen parametreli bir sorgu örneği aşağıdadır:

```sql
SELECT * FROM t1 JOIN t2 ON t1.c1 = t2.c1
WHERE t1.c1 = @p1 AND t2.c2 = '961C3970-0E54-4E8E-82B6-5545BE897F8F'
```

Önceki örnekte, `t1.c1` alır `@p1` ancak `t2.c2` devam GUID değişmez değer olarak alın. Değeri değiştirirseniz, bu durumda `c2`, sorgu, farklı bir sorgu kabul edilir ve yeni bir derleme meydana gelir. Önceki örnek derlemelerde azaltmak için ayrıca GUID Parametreleştirme çözümdür.

Aşağıdaki sorguyu sorgu düzgün parametreli olup olmadığını belirlemek için sorgu karma olarak sorgularının sayısı gösterilmektedir:

```sql
SELECT  TOP 10  
  q.query_hash
  , count (distinct p.query_id ) AS number_of_distinct_query_ids
  , min(qt.query_sql_text) AS sampled_query_text
FROM sys.query_store_query_text AS qt
  JOIN sys.query_store_query AS q
     ON qt.query_text_id = q.query_text_id
  JOIN sys.query_store_plan AS p 
     ON q.query_id = p.query_id
  JOIN sys.query_store_runtime_stats AS rs 
     ON rs.plan_id = p.plan_id
  JOIN sys.query_store_runtime_stats_interval AS rsi
     ON rsi.runtime_stats_interval_id = rs.runtime_stats_interval_id
WHERE
  rsi.start_time >= DATEADD(hour, -2, GETUTCDATE())
  AND query_parameterization_type_desc IN ('User', 'None')
GROUP BY q.query_hash
ORDER BY count (distinct p.query_id) DESC
```
### <a name="factors-influencing-query-plan-changes"></a>Sorgu planı değişikliklerini etkileyen faktörler

Sorgu yürütme planı öyle ne başlangıçta önbelleğe ilişkili öğesinden farklı bir oluşturulan sorgu planında neden olabilir. Neden bir varolan orijinal plan otomatik olarak yeniden derlenmesi çeşitli nedenleri vardır:
- Sorgu tarafından başvurulan şema değişiklikleri
- Sorgu tarafından başvurulan tablolar için veri değişikliklerini 
- Sorgu bağlamı seçeneklerinde yapılan değişiklikler 

Derlenmiş bir plan için çeşitli nedenlerle örneği yeniden başlatma da dahil olmak üzere, önbellekten çıkarılmasını, yapılandırma değişikliklerinin, bellek baskısı ve önbelleği temizlemek için açık istekleri veritabanı kapsamlı. Ayrıca, RECOMPILE ipucu kullanarak bir plan önbelleğe alınabilir olmaz anlamına gelir.

Yeniden derleme (veya yeni Derleme Önbelleği çıkarma sonra) hala aynı sorgu yürütme planı bir ilk gözlemlenen nesil sonuçlanabilir.  Ancak, varsa, önceki ya da orijinal plana göre plana değişiklikleri, bir sorgu yürütme planı neden değiştirilen için en yaygın açıklamaları aşağıda verilmiştir:

- **Fiziksel tasarım değiştirilen**. Örneğin, daha etkili bir şekilde yeni bir derleme üzerinde bir sorgu gereksinimlerini sorgu iyileştiricisi verirse kullanılabilir kapak başlangıçta ilk sürümü için seçilen veri yapısı kullanmak daha yeni bir dizin yararlanmak için daha uygun olduğunu yeni dizin oluşturuldu. sorgu yürütme.  Başvurulan nesneleri fiziksel değişiklikleri, yeni bir plan seçtiğiniz derleme zamanında neden olabilir.

- **Sunucu kaynak fark**. Burada sistemde"A" ve "Sistem B" – kaynaklarının, kullanılabilir işlemci sayısı gibi bir plan farklı bir senaryoda hangi planı oluşturulan etkileyebilir.  Örneğin, bir sistem daha yüksek bir sayı işlemci varsa, paralel bir plandaki seçmiş olabilirsiniz. 

- **Farklı istatistikleri**. Başvurulan nesneleri ile ilişkili istatistikleri değiştirilmiş veya özgün sistemin istatistiklerini olması durumunda farklıdır.  İstatistikleri değiştirirseniz ve çalışabilmeleri gerçekleşir, sorgu iyileştiricisi istatistikleri belirli o noktadan itibaren zaman kullanın. Düzeltilmiş istatistikleri, dağıtımları önemli ölçüde farklı veri ve orijinal derleme durumda değildi frekansları sahip olabilir.  Bu değişiklikler, kardinalite tahmin eder (mantıksal sorgu ağacı ile akış için beklenen satır sayısı) tahmin etmek için kullanılır.  Kardinalite tahmin değişiklikleri farklı fiziksel işleçler ve ilişkili sıralı-ın-işlemler seçmek için bize yol açabilir.  İstatistiklerini bile küçük değişiklikler, değiştirilen sorguyu yürütme planında neden olabilir.

- **Değiştirilen veritabanı uyumluluk düzeyi veya kardinalite tahmin aracı sürümü**.  Veritabanı uyumluluk düzeyi değişiklikleri, yeni stratejiler ve farklı bir sorgu yürütme planında neden özellikleri etkinleştirebilirsiniz.  Veritabanı uyumluluk düzeyinin devre dışı bırakma veya 4199 izleme bayrağını etkinleştirme ya da QUERY_OPTIMIZER_HOTFIXES da etkileyebilir veritabanı kapsamlı yapılandırma durumunu değiştirmeyi, derleme zamanında yürütme planı seçenekleri sorgulayın.  İzleme Bayrakları 9481 (zorla eski CE) ve (varsayılan CE zorla) 2312 de planlamanız etkileyen. 

### <a name="resolve-problem-queries-or-provide-more-resources"></a>Sorun sorguları çözümlemek veya daha fazla kaynak sağlayın

Sorun tanımladıktan sonra sorun sorgularınızı ayarlamak veya işlem boyutunu yükseltebilir veya hizmet katmanını CPU gereksinimleri etkisini azaltmak amacıyla Azure SQL veritabanınızın kapasitesini artırmak için. Tek veritabanları için kaynakların ölçeklendirilmesi hakkında daha fazla bilgi için bkz: [Azure SQL veritabanı'nda tek bir veritabanı kaynakları ölçeklendirme](sql-database-single-database-scale.md) ve elastik havuzlar için kaynakların ölçeklendirilmesi için bkz [Azure SQL elastik havuzu kaynakları ölçeklendirme Veritabanı](sql-database-elastic-pool-scale.md). Yönetilen örnek ölçeklendiriliyor hakkında daha fazla bilgi için bkz: [örnek düzeyi kaynak sınırları](sql-database-managed-instance-resource-limits.md#instance-level-resource-limits).

### <a name="determine-if-running-issues-due-to-increase-workload-volume"></a>Sorunları nedeniyle artış iş yükü birimi çalıştırılıyorsa belirleme

Uygulama trafiğini ve iş yükü artış daha yüksek CPU kullanımı için hesap, ancak düzgün bir şekilde bu sorunu tanılamak dikkatli olmanız gerekir. Yüksek CPU senaryosunda, gerçekten CPU artışı nedeniyle iş yükü birimi değişiklikler olup olmadığını belirlemek için aşağıdaki soruları yanıtlayın:

1. Uygulama sorgularından yüksek CPU sorunun nedenini misiniz?
2. (Tanımlanabilir) en çok CPU kullanan sorguları için:

   - Birden çok yürütme planı aynı sorgusu ile ilişkili olup olmadığını belirler. Bu durumda, neden belirleyin.
   - Aynı yürütme planı ile sorgularında yürütme sürelerini tutarlı ve yürütme sayısı artan belirleyin. Yanıt Evet ise, iş yükü artışı nedeniyle olası performans sorunları vardır.

Özetle, sorgu yürütme planı farklı yürütme olmadı, ancak CPU kullanımı ile birlikte yürütme sayısı artan var. büyük olasılıkla bir iş yükü performansı artırma ile ilgili sorun

Her zaman bir CPU sorunu yürüten bir iş yükü birimi değişiklik sonuçlandırmak kolay değildir.   Dikkat edilecek noktalar: 

- **Değiştirilen kaynak kullanımı**

  Örneğin, burada CPU uzun bir süre için %80 artan bir senaryo düşünün.  Tek başına CPU kullanımı iş yükü birimi değiştirilen anlamına gelmez.  Uygulamanın tam aynı iş yükünü yürütüyor olsa bile gerilemeleri ve veri dağıtım değişiklikleri de daha fazla kaynak kullanımına katkıda bulunabilir yürütme planını sorgulayın.

- **Yeni sorgu göründü**

   Uygulamanın yeni bir sorgu kümesi farklı zamanlarda sürücü.

- **İstek sayısını artırabilir veya azaltılabilir**

   Bu senaryo, iş yükünün en belirgin ölçümüdür. Sorgu sayısı daha fazla kaynak kullanımı için her zaman karşılık gelmiyor. Ancak bu ölçüm, yine de diğer etkenler değiştirilmemiş olduğunu varsayarak önemli bir sinyal olabilir.

## <a name="waiting-related-performance-issues"></a>Bekleyen ilgili performans sorunları

Karşılaştığınız değil, yüksek CPU, çalıştırma ile ilgili performans sorunu sonra bekleyen ilgili performans sorunu karşılaştığınız. CPU üzerinde başka bir kaynak beklediği ayrıca CPU kaynaklarınızı verimli bir şekilde kullanılmıyor. Bu durumda, sonraki adımınız ne CPU kaynaklarınızı bekleyen belirlemektir. Üst göstermek için en yaygın yöntemleri türü kategorilerini bekleyin:

- [Query Store](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) bekleme istatistikleri sorgu başına zamanla sağlar. Query Store bekleme türleri bekleme kategoriler halinde birleştirilir. Eşleme türleri beklemek bekleme kategorilerin kullanılabilir [sys.query_store_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql#wait-categories-mapping-table).
- [sys.dm_db_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database) işlemi sırasında yürütülen iş parçacığı tarafından karşılaşılan bekler hakkında bilgi verir. Azure SQL veritabanı ve ayrıca özel sorgular ve toplu işler ile performans sorunlarını tanılamak için bu birleşik bir görünüm kullanabilirsiniz.
- [sys.dm_os_waiting_tasks](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql) bazı kaynak üzerinde bekleyen görevlerin bekleme kuyruk hakkındaki bilgileri döndürür.

Yüksek CPU senaryolarda, Query Store ve bekleme istatistikleri her zaman CPU kullanımı bu iki nedenden dolayı yansıtmaz:

- Yüksek CPU kullanan sorguları hala yürütülüyor ve sorguları tamamlamadınız
- Yüksek CPU kullanan sorguları bir yük devretme gerçekleştiğinde çalışmakta olan

Query Store ve bekleme istatistikleri izleme dinamik yönetim görünümlerini yalnızca başarıyla tamamlanmış ve zaman aşımına uğradı sorguların sonuçlarını gösterir ve (bunlar tamamlanana kadar), şu anda deyimleri yürütme için veri gösterme. Dinamik Yönetim görünümünü [sys.dm_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) şu anda yürütülen sorgular ve ilişkili çalışan zaman izlemenize olanak tanır.

Önceki grafikte gösterildiği gibi en yaygın bekler şunlardır:

- Kilitleri (engelleme)
- G/Ç
- `tempdb`-ilgili çakışması
- Bellek verme bekler

> [!IMPORTANT]
> Bu bekleme ile ilgili sorunları gidermek için bu Dmv'leri kullanarak bir T-SQL sorguları için bkz:
>
> - [G/ç performans sorunlarını belirleme](sql-database-monitoring-with-dmvs.md#identify-io-performance-issues)
> - [Tanımlamak `tempdb` performans sorunları](sql-database-monitoring-with-dmvs.md#identify-io-performance-issues)
> - [Bellek verme bekler tanımlayın](sql-database-monitoring-with-dmvs.md#identify-memory-grant-wait-performance-issues)
> - [TigerToolbox - bekler ve kilitler](https://github.com/Microsoft/tigertoolbox/tree/master/Waits-and-Latches)
> - [TigerToolbox - usp_whatsup](https://github.com/Microsoft/tigertoolbox/tree/master/usp_WhatsUp)

## <a name="improving-database-performance-with-more-resources"></a>Daha fazla kaynak ile veritabanı performansı iyileştirme

Son olarak, veritabanınızın performansını iyileştirebilir hiçbir eyleme dönüştürülebilir öğe varsa, Azure SQL veritabanı'nda kullanılabilir kaynakların miktarını değiştirebilirsiniz. Daha fazla kaynak değiştirerek atayabilirsiniz [DTU hizmet katmanı](sql-database-service-tiers-dtu.md) tek veritabanı veya elastik havuz edtu'ları dilediğiniz zaman artırın. Alternatif olarak, kullanıyorsanız [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md), hizmet katmanını değiştirebilir veya veritabanınız için ayrılan kaynakları artırın.

1. Tek veritabanları için yapabilecekleriniz [hizmet katmanlarını değiştirme](sql-database-single-database-scale.md) veya [işlem kaynaklarını](sql-database-single-database-scale.md) veritabanı performansını artırmak için isteğe bağlı.
2. Birden fazla veritabanı için kullanmayı [elastik havuzlar](sql-database-elastic-pool-guidance.md) kaynakları otomatik olarak ölçeklendirmek için.

## <a name="tune-and-refactor-application-or-database-code"></a>Ayarlayın ve yeniden düzenleme uygulama veya veritabanı kod

Uygulama kodunun daha verimli veritabanı kullanmak, dizinleri değiştirin, plan zorlama veya iş yükünüz için veritabanını el ile uyum ipuçlarını kullanma değiştirebilirsiniz. El ile ayarlama ve kodu yeniden yazma için bazı yönergeler ve ipuçları bulabilirsiniz [performans Kılavuzu konu](sql-database-performance-guidance.md) makalesi.

## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL veritabanı'nda Otomatik ayarlamayı etkinleştirme ve otomatik ayarlama özelliğini tam olarak iş yükünüzü yönetmenize izin vermek için bkz: [otomatik ayarlamayı etkinleştirme](sql-database-automatic-tuning-enable.md).
- El ile ayarlama kullanmak için gözden geçirebilirsiniz [ayarlama önerileri Azure portalında](sql-database-advisor-portal.md) ve el ile sorgularınızı performansını olanları uygulayın.
- Değiştirerek, veritabanında mevcut olan kaynakları değiştirmek [Azure SQL veritabanı hizmet katmanları](sql-database-performance-guidance.md)
