---
title: İzleme ve performans ayarlama - Azure SQL veritabanı | Microsoft Docs
description: Performans değerlendirmesi ve geliştirme ile Azure SQL veritabanı'nda ayarlama için ipuçları.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: v-daljep
ms.reviewer: carlrab
manager: craigg
ms.date: 10/15/2018
ms.openlocfilehash: fc97aa18328fafc299ad941e6bf12dd21e9029d0
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49345297"
---
# <a name="monitoring-and-performance-tuning"></a>İzleme ve performans ayarlama

Azure SQL veritabanı otomatik olarak yönetilir ve burada kolayca izleyebilir, kullanım, ekleyebilir veya kaynakları (CPU, bellek, g/ç) kaldırmak esnek veri hizmeti bulun, veritabanınızın performansını veya veritabanı iş yükünüze uyum sağlar. öneriler ve otomatik olarak performansı iyileştirin.

## <a name="the-state-of-an-active-query"></a>Etkin bir sorgu durumu

Azure SQL veritabanı performansını artırmak için her etkin sorgu isteği uygulamanızdan bir çalışan veya bekleme durumunda olduğunu anlayın. Aşağıdaki grafikte, Azure SQL veritabanında bir performans sorunu gidermede dikkat edin:

![İş yükü durumları](./media/sql-database-monitor-tune-overview/workload-states.png)

Performans sorunları olan bir iş yükü için performans sorunu my CPU Çekişme nedeniyle olabilir (bir **çalıştırma ile ilgili** koşul) veya tek tek sorgular üzerinde bir bekleyen (bir **bekleme ilgili** koşul) .

- **Aşırı CPU kullanımı, Azure SQL veritabanı'nda**:

  Aşırı CPU kullanımına neden olan performans sorunları aşağıdaki koşullarda görebilirsiniz:

  - Çok fazla sayıda çalışan sorguları
  - Çok fazla derleme sorguları
  - Bir veya daha çok yürütülen sorguları optimum sorgu planı kullanma

  İş yükünüz için Durum buysa, amacınız tanımlamak ve ilişkili sorgularınızı ayarlamak veya işlem boyutu yükseltin veya hizmet katmanını CPU gereksinimleri etkisini azaltmak amacıyla Azure SQL veritabanınızın kapasitesini artırmak için sağlamaktır. Tek veritabanları için kaynakların ölçeklendirilmesi, daha fazla bilgi için [Azure SQL veritabanı'nda tek bir veritabanı kaynakları ölçeklendirme](sql-database-single-database-scale.md) ve elastik havuzlar için kaynakların ölçeklendirilmesi için bkz [Azure SQL elastik havuzu kaynakları ölçeklendirme Veritabanı](sql-database-elastic-pool-scale.md).

- **Tek bir sorgu üzerinde bekliyor**

  Her sorgu için bekleyen sorgu nedeniyle performans sorunları olabilir. Bu senaryoda, kaldırmak veya bekleme süresini azaltmak için amacınız anlamaktır.

### <a name="determine-if-you-have-a-running-related-performance-issue"></a>Bir çalıştırma ile ilgili performans sorunu olup olmadığını belirleme

Çeşitli yöntemler kullanarak çalıştırma ile ilgili performans sorunlarını tespit edebilirsiniz. En sık kullanılan yöntemler şunlardır:

- Kullanım [Azure portalında](#monitor-databases-using-the-azure-portal) CPU yüzdesi kullanımı izlemek için.
- Aşağıdaki [dinamik yönetim görünümlerini](sql-database-monitoring-with-dmvs.md):

  - [sys.dm_db_resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) bir Azure SQL veritabanı için CPU, g/ç ve bellek tüketimi döndürür. Veritabanında hiç etkinlik olsa her 15 saniyede bir satır yok. Geçmiş verileri, bir saat boyunca korunur.
  - [sys.resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) bir Azure SQL veritabanı için CPU kullanımı ve depolama verilerini döndürür. Veriler toplanır ve beş dakikalık aralıklarla içinde toplanır.

> [!TIP]
> Genel bir kural olarak, CPU kullanımı veya % 80'de, üzerindeki tutarlı bir şekilde ise bir çalıştırma ile ilgili performans sorunu gerekir.

### <a name="determine-if-you-have-a-waiting-related-performance-issue"></a>Bekleyen ilgili performans sorunu olup olmadığını belirleme

İlk olarak, bu yüksek CPU, çalıştırma ile ilgili performans sorunu olmadığını emin olun. Yüklü değilse, sonraki adımda, uygulama iş yükünüz ile ilişkili üst bekler belirlemektir.  Üst göstermek için ortak yöntemleri türü kategorilerini bekleyin:

- [Query Store](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) bekleme istatistikleri sorgu başına zamanla sağlar. Query Store bekleme türleri bekleme kategoriler halinde birleştirilir. Eşleme türleri beklemek bekleme kategorilerin kullanılabilir [sys.query_store_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md#wait-categories-mapping-table).
- [sys.dm_db_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database) işlemi sırasında yürütülen iş parçacığı tarafından karşılaşılan bekler hakkında bilgi verir. Azure SQL veritabanı ve ayrıca özel sorgular ve toplu işler ile performans sorunlarını tanılamak için bu birleşik bir görünüm kullanabilirsiniz.
- [sys.dm_os_waiting_tasks](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql) bazı kaynak üzerinde bekleyen görevlerin bekleme kuyruk hakkındaki bilgileri döndürür.

Önceki grafikte gösterildiği gibi en yaygın bekler şunlardır:

- Kilitleri (engelleme)
- G/Ç
- tempdb ilgili çakışması
- Bellek verme bekler

Gördüğünüz bağlı olarak, farklı bir sorun giderme yol her bekleme kategorisi vardır.

## <a name="overview-of-monitoring-database-performance-in-azure-sql-database"></a>Azure SQL database'de veritabanı performansını izlemeye genel bakış

Azure SQL veritabanı performansını izlemeye, seçtiğiniz veritabanı performans düzeyiyle ilgili kaynak kullanımını izleyerek başlarsınız. İzleme yardımcı olur, veritabanınızın gerekenden fazla kapasiteye sahip veya bu kaynakları dışarı ayarlanma çünkü sorun yaşıyor belirlemek ve ardından hizmet katmanları, veritabanı ve işlem boyutunu ayarlamak için süre olup olmadığına karar vermek [DTU tabanlı satın alma Model](sql-database-service-tiers-dtu.md) veya [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md). Veritabanınızda izleyebilirsiniz [Azure portalında](https://portal.azure.com) aşağıdaki grafik araçları veya SQL kullanarak [dinamik yönetim görünümlerini (Dmv'ler)](sql-database-monitoring-with-dmvs.md).

Azure SQL veritabanı sayesinde artırmak ve kaynakları değiştirmeden geçirerek sorgu performansını iyileştirmek için fırsatlarını belirlemek [performans ayarlama önerilerinde](sql-database-advisor.md). Veritabanı performansının düşük olmasına yol açan yaygın nedenler, dizinlerin eksik olması ve sorguların hatalı bir şekilde iyileştirilmesidir. İş yükünüzün performansını artırmak için bu ayar önerileri uygulayabilirsiniz.
Let Azure SQL veritabanı'na ayrıca [otomatik olarak, sorguların performansını en iyi duruma getirme](sql-database-automatic-tuning.md) uygulayarak tüm öneriler ve veritabanı performansını artırmak doğrulama belirledik. İzleme ve sorun giderme veritabanı performans için aşağıdaki seçenekleriniz:

- İçinde [Azure portalında](https://portal.azure.com), tıklayın **SQL veritabanları**, veritabanını seçin ve ardından aramak için en fazla yaklaşan kaynakları için izleme grafiği kullanın. DTU tüketimi, varsayılan olarak gösterilir. Tıklayın **Düzenle** gösterilen değerler ve zaman aralığını değiştirmek için.
- Kullanım [sorgu performansı İçgörüleri](sql-database-query-performance.md) kaynaklarının en çok harcama sorguları tanımlamak için.
- Kullanım [SQL veritabanı Danışmanı](sql-database-advisor-portal.md) oluşturmak ve dizinleri bırakmayı, sorguları kümesini parametreleştirme ve şema sorunlarını giderme önerileri görüntüleyebilirsiniz.
- Kullanım [Azure SQL Intelligent Insights](sql-database-intelligent-insights.md) otomatik, veritabanı performansını izleme. Bir performans sorunu algılandığında bir tanılama günlüğü, Ayrıntılar ve sorunun kök neden analizi (RCA) ile oluşturulur. Mümkün olduğunda performans iyileştirme öneri sağlanmaktadır.
- [Otomatik ayarlamayı etkinleştirme](sql-database-automatic-tuning-enable.md) ve Azure SQL tanımlanan düzeltme performans sorunlarını otomatik olarak veritabanı sağlar.
- Ayrıca [dinamik yönetim görünümlerini (Dmv'ler)](sql-database-monitoring-with-dmvs.md), [Genişletilmiş olaylar (`XEvents`) (sql-database/sql-database-xevent-db-diff-from-svr.md) ve [Query Store](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) performansı elde etmek için gerçek zamanlı parametreleri. Bkz: [performans rehberi](sql-database-performance-guidance.md) bu raporları ve görünümleri kullanarak bazı sorunu olduğunu belirlerseniz, Azure SQL veritabanı performansını artırmak için kullanabileceğiniz teknikleri bulunacak.

## <a name="monitor-databases-using-the-azure-portal"></a>Azure portalını kullanarak veritabanlarını izleme

[Azure portalında](https://portal.azure.com/), tek veritabanlarının kullanımını veritabanınızı seçip **İzleme** grafiğine tıklayarak izleyebilirsiniz. Bu işlem sonrasında bir **Ölçüm** penceresi görüntülenir. **Grafiği düzenle** düğmesine tıklayarak değişiklik yapabilirsiniz. Şu ölçümleri ekleyin:

- CPU yüzdesi
- DTU yüzdesi
- Veri G/Ç yüzdesi
- Veri boyutu yüzdesi

Bu ölçümleri ekledikten sonra görüntülemeye devam edebilirsiniz **izleme** grafik hakkında daha fazla bilgi **ölçüm** penceresi. Dört ölçümün tümü de veritabanınızın ortalama **DTU** kullanım yüzdesini gösterir. Bkz: [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md) makaleler hizmet katmanları hakkında daha fazla bilgi için.  

![Hizmet katmanına göre veritabanı performansını izleme.](./media/sql-database-single-database-monitoring/sqldb_service_tier_monitoring.png)

Performans ölçümlerine ilişkin uyarıları da yapılandırabilirsiniz. **Ölçüm** penceresindeki **Uyarı ekle** düğmesine tıklayın. Uyarınızı yapılandırmak için sihirbazı takip edin. Ölçümlerin belirli bir eşiği aşması veya belirli bir eşiğin altına düşmesi halinde uyarı alabilirsiniz.

Örneğin, veritabanınızdaki bir iş yükünün artmasını bekliyorsanız bir e-posta uyarısı yapılandırarak veritabanınızın herhangi bir performans ölçümünde %80 sınırına ulaşması halinde uyarı alabilirsiniz. Zaman sonraki daha yüksek işlem boyutu geçmeniz gerektiğini anlamak üzere erken bir uyarı olarak kullanabilirsiniz.

Performans ölçümleri, daha düşük bir işlem boyutu için geçemeyeceğinizi belirlemenize de yardımcı olabilir. Standart S2 veritabanını kullandığınızı ve tüm performans ölçümlerinin, veritabanının belirli bir zaman için ortalama %10'dan daha fazla kullanımda bulunmadığını gösterdiğini varsayın. Bu, veritabanının Standart S1'de de düzgün şekilde çalışabileceğini gösterir. Ancak, ani değişiklik veya dalgalanma gösteren bir alt işlem boyutu geçmeye karar vermeden önce iş yüklerini farkında olun.

## <a name="improving-database-performance-with-more-resources"></a>Daha fazla kaynak ile veritabanı performansı iyileştirme

Son olarak, veritabanınızın performansını iyileştirebilir hiçbir eyleme dönüştürülebilir öğe varsa, Azure SQL veritabanı'nda kullanılabilir kaynakların miktarını değiştirebilirsiniz. Daha fazla kaynak değiştirerek atayabilirsiniz [DTU hizmet katmanı](sql-database-service-tiers-dtu.md) tek veritabanı veya elastik havuz edtu'ları dilediğiniz zaman artırın. Alternatif olarak, kullanıyorsanız [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md), hizmet katmanını değiştirebilir veya veritabanınız için ayrılan kaynakları artırın.

1. Tek veritabanları için yapabilecekleriniz [hizmet katmanlarını değiştirme](sql-database-service-tiers-dtu.md) veya [işlem kaynaklarını](sql-database-service-tiers-vcore.md) veritabanı performansını artırmak için isteğe bağlı.
2. Birden fazla veritabanı için kullanmayı [elastik havuzlar](sql-database-elastic-pool-guidance.md) kaynakları otomatik olarak ölçeklendirmek için.

## <a name="tune-and-refactor-application-or-database-code"></a>Ayarlayın ve yeniden düzenleme uygulama veya veritabanı kod

Uygulama kodunun daha verimli veritabanı kullanmak, dizinleri değiştirin, plan zorlama veya iş yükünüz için veritabanını el ile uyum ipuçlarını kullanma değiştirebilirsiniz. El ile ayarlama ve kodu yeniden yazma için bazı yönergeler ve ipuçları bulabilirsiniz [performans Kılavuzu konu](sql-database-performance-guidance.md) makalesi.

## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL veritabanı'nda Otomatik ayarlamayı etkinleştirme ve otomatik ayarlama özelliğini tam olarak iş yükünüzü yönetmenize izin vermek için bkz: [otomatik ayarlamayı etkinleştirme](sql-database-automatic-tuning-enable.md).
- El ile ayarlama kullanmak için gözden geçirebilirsiniz [ayarlama önerileri Azure portalında](sql-database-advisor-portal.md) ve el ile sorgularınızı performansını olanları uygulayın.
- Değiştirerek, veritabanında mevcut olan kaynakları değiştirmek [Azure SQL veritabanı hizmet katmanları](sql-database-performance-guidance.md)
