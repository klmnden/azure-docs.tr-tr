---
title: Intelligent Insights Performans Tanılama Günlüğü - Azure SQL veritabanı | Microsoft Docs
description: Akıllı İçgörüler sağlayan bir Azure SQL veritabanı performans sorunlarını tanılama günlüğü
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
ms.date: 04/04/2018
ms.openlocfilehash: 70096c8f3a5c07fa757b68494c04519b63435dcd
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166903"
---
# <a name="use-the-intelligent-insights-azure-sql-database-performance-diagnostics-log"></a>Intelligent ınsights'ı Azure SQL veritabanı performans tanılama günlüğünü kullanma

Bu sayfa, Azure SQL veritabanı performans tanılama günlüğü oluşturan kullanma hakkında bilgi sağlar. [Intelligent Insights](sql-database-intelligent-insights.md), biçimi ve özel geliştirme gereksinimleriniz için veriler. Bu tanılama günlüğüne gönderdiğiniz [Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md), [Azure Event Hubs](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md), [Azure depolama](sql-database-metrics-diag-logging.md#stream-into-storage), veya bir üçüncü taraf çözümü için uyarı ve raporlama özel DevOps özellikleri.

## <a name="log-header"></a>Günlük üst bilgisi

Tanılama Günlüğü Intelligent Insights bulguları çıktısını almak için JSON standart bir biçim kullanır. Bir akıllı Öngörüler günlüğü erişmek için tam kategori özelliği, "SQLInsights" sabit değerdir.

Günlük üst bilgisi için ortaktır ve gösteren bir giriş oluşturulduğu zaman damgasını (TimeGenerated) oluşur. Ayrıca, belirli bir SQL veritabanını giriş ilişkili başvurduğu bir kaynak kimliği (ResourceId) içerir. Kategori (kategori), düzeyi (düzeyi) ve işlem adı (OperationName) değerlerini değiştirmeyin özellikler sabittir. Bunlar, günlük girdisini bilgi amaçlıdır ve akıllı İçgörüler (SQLInsights) geldiğini gösterir.

```json
"TimeGenerated" : "2017-9-25 11:00:00", // time stamp of the log entry
"ResourceId" : "database identifier", // value points to a database resource
"Category": "SQLInsights", // fixed property
"Level" : "Informational", // fixed property
"OperationName" : "Insight", // fixed property
```

## <a name="issue-id-and-database-affected"></a>Sorun kimliği ve etkilenen veritabanı

Sorun kimliği özelliği (issueId_d) onlar çözülene kadar benzersiz performans sorunlarını izleme, bir yol sağlar. Akıllı İçgörüler her sorun yaşam döngüsü "Etkin", "Doğrulama" veya "Tamamlandı" olarak gözlemler. Oturum açma birden çok olay kayıtlarını her bu durum aşamaları akıllı İçgörüler kaydedebilirsiniz. Her biri bu girdiler için sorun numarası benzersiz kalır. Akıllı İçgörüler sorunu yaşam döngüsü aracılığıyla izler ve bir öngörü 15 dakikada bir tanılama günlüğüne oluşturur.

Bir performans sorunu algılanmadığında ve onu sürer sürece, sorunu durumu (status_s) özelliği altında "Etkin" olarak bildirilen için. Algılanan bir sorun giderildikten sonra bu doğrulandı ve durum (status_s) özelliği altında "Doğrulama" olarak bildirilen. Sorunun artık mevcut değilse durumu (status_s) özelliği bu sorunu "Tamamlandı" bildirir.

Sorun kimliği yanı sıra tanılama günlüğü (intervalStartTime_t) başlangıç ve bitiş (intervalEndTme_t) zaman damgaları tanılama günlüğüne bir sorunla ilgili belirli bir olay bildirir.

Elastik havuz (elasticPoolName_s) özelliği, bir sorunu veritabanının ait hangi elastik havuz gösterir. Veritabanını bir elastik havuzun parçası değilse, bu özelliğin değeri yoktur. Bir sorunun algılandığı veritabanı veritabanı adı (databaseName_s) özelliğinde ifşa edilmiştir.

```json
"intervalStartTime_t": "2017-9-25 11:00", // start of the issue reported time stamp
"intervalEndTme_t":"2017-9-25 12:00", // end of the issue reported time stamp
"elasticPoolName_s" : "", // resource elastic pool (if applicable) 
"databaseName_s" : "db_name",  // database name
"issueId_d" : 1525, // unique ID of the issue detected
"status_s" : "Active" // status of the issue – possible values: "Active", "Verifying", and "Complete"
```

## <a name="detected-issues"></a>Algılanan sorunlar

Akıllı Öngörüler performans günlüğü, bir sonraki bölümüne, yerleşik yapay zeka ile algılanan performans sorunlarını içerir. Algılama özellikleri JSON tanılama günlüğü içinde bildirilen. Bu algılamaların amacı, bir sorun kategorisi, sorun, etkilenen sorgular ve ölçümleri etkisini oluşur. Algılanan birden çok performans sorunlarını algılama özellikleri içerebilir.

Algılanan performans sorunlarını algılama özelliği yapısıyla bildirilir:

```json
"detections_s" : [{
"impact" : 1 to 3, // impact of the issue detected, possible values 1-3 (1 low, 2 moderate, 3 high impact)
"category" : "Detectable performance pattern", // performance issue detected, see the table
"details": <Details outputted> // details of an issue (see the table)
}] 
```

Aşağıdaki tabloda, algılanabilir performans desenleri ve tanılama günlüğüne yüzdelik Ayrıntılar sağlanır.

### <a name="detection-category"></a>Algılama kategorisi

Kategori (kategori) özelliği kategorisini algılanabilir performans desenleri açıklar. Tüm olası kategorileri algılanabilir performans desenleri için aşağıdaki tabloya bakın. Daha fazla bilgi için [veritabanı Intelligent Insights ile performans sorunlarını giderme](sql-database-intelligent-insights-troubleshoot-performance.md).

Algılanan performans sorunu tanılamada yüzdelik ayrıntıları günlük dosyası farklılık uygun şekilde.

| Algılanabilir performans desenleri | Yüzdelik ayrıntıları |
| :------------------- | ------------------- |
| Kaynak sınırlarını ulaşma | <li>Etkilenen kaynaklar</li><li>Sorgu karmaları</li><li>Kaynak tüketimi yüzdesi</li> |
| İş yükü artışı | <li>Yürütme artan sorgu sayısı</li><li>Karma sorgu iş yükü artırmak için en büyük katkı ile sorgulama</li> |
| Bellek baskısı | <li>Bellek yazıcısı</li> |
| Kilitleniyor | <li>Etkilenen sorgu karmaları</li><li>Sorgu karma engelleme</li> |
| Artan MAXDOP | <li>Sorgu karmaları</li><li>CXP bekleme süreleri</li><li>Kez bekleyin</li> |
| Pagelatch çakışması | <li>Çekişme neden sorguların karmaları sorgulama</li> |
| Dizini yok | <li>Sorgu karmaları</li> |
| Yeni Sorgu | <li>Sorgu karması yeni sorgu</li> |
| Olağan dışı bekleme istatistikleri | <li>Olağan dışı bekleme türleri</li><li>Sorgu karmaları</li><li>Sorgu bekleme süresini</li> |
| TempDB çakışması | <li>Çekişme neden sorguların karmaları sorgulama</li><li>Sorgu attribution genel veritabanı pagelatch Çekişme bekleme süresi [%] için</li> |
| Elastik havuz DTU eksik | <li>Elastik havuz</li><li>Üst veritabanı DTU kullanma</li><li>Havuz DTU üst tüketici tarafından kullanılan yüzdesi</li> |
| Regresyon planlama | <li>Sorgu karmaları</li><li>İyi planı kimlikleri</li><li>Hatalı planı kimlikleri</li> |
| Veritabanı kapsamlı yapılandırma değeri Değiştir | <li>Varsayılan değerlere kıyasla veritabanı kapsamlı yapılandırma değişiklikleri</li> |
| Yavaş istemci | <li>Sorgu karmaları</li><li>Kez bekleyin</li> |
| Fiyatlandırma katmanı düşürme | <li>Metin bildirimi</li> |

### <a name="impact"></a>Etki

Etki (etkisi özelliği için bir veritabanı olan sorunu algılanan davranış ne kadar katkıda tanımlar). Aralık 1-3, en büyük katkıyı yapan olarak 3, 2, Orta olarak ve 1, en düşük katkı etkiler. Etkisi değeri gereksinimlerinize bağlı olarak özel uyarı Otomasyon için bir giriş olarak kullanılabilir. Özellik sorgularına etkilenen (QueryHashes) belirli bir saptama tarafından etkilenen karmaları sorgu listesini sağlar.

### <a name="impacted-queries"></a>Etkilenen sorgular

Akıllı Öngörüler günlüğü sonraki bölümü tarafından algılanan performans sorunlarını etkilenmiştir belirli sorguları hakkında bilgi sağlar. Bu bilgiler impact_s özelliğinde katıştırılmış nesneleri dizisi olarak açıktır. Varlıklar ve ölçümleri etki özelliğinin oluşur. Varlıkların belirli bir sorgu bakın (tür: Sorgu). Benzersiz sorgu karma değeri (değer) özelliği altında açıklanır. Ayrıca, her duyurulmuş sorguların bir ölçüm ve bir değer tarafından algılanan performans sorunu olduğunu izler.

Aşağıdaki günlük örnekte artan bir yürütme süresi olan karma 0x9102EXZ4 sorguyla algılandı (ölçüm: DurationIncreaseSeconds). Bu belirli bir sorgu yürütmek için artık 110 saniye sürdü 110 saniye değerini gösterir. Birden çok sorgu algılanabilir olduğundan, bu belirli günlük bölümünde birden çok sorgu girişi içerebilir.

```json
"impact" : [{
"entity" : { 
"Type" : "Query", // type of entity - query
"Value" : "query hash value", // for example "0x9102EXZ4" query hash value },
"Metric" : "DurationIncreaseSeconds", // measured metric and the measurement unit (in this case seconds)
"Value" : 110 // value of the measured metric (in this case seconds)
}]
```

### <a name="metrics"></a>Ölçümler

Bildirilen her bir ölçüm için ölçü birimini altındaki ölçüm (ölçüm) özelliği ile saniye sayısı ve yüzdesi, olası değerler sağlanır. Ölçülen bir ölçüm değeri, değer (değer) özelliğinde bildirilir.

Saniye cinsinden ölçü DurationIncreaseSeconds özelliği sağlar. Ölçü CriticalErrorCount hata sayısını temsil eden bir sayıdır.

```json
"metric" : "DurationIncreaseSeconds", // issue metric type – possible values: DurationIncreaseSeconds, CriticalErrorCount, WaitingSeconds
"value" : 102 // value of the measured metric (in this case seconds)
```

## <a name="root-cause-analysis-and-improvement-recommendations"></a>Kök neden analizi ve iyileştirme önerileri

Akıllı Öngörüler performans günlüğü son kısmını tanımlanan performans düşüşü sorunu otomatik kök neden analizi için geçerlidir. Kök neden analizi (rootCauseAnalysis_s) özelliğinde İnsan dostu duyuruları bilgileri görüntülenir. İyileştirme önerileri, mümkün olduğunda günlüğüne dahil edilir.

```json
// example of reported root cause analysis of the detected performance issue, in a human-readable format

"rootCauseAnalysis_s" : "High data IO caused performance to degrade. It seems that this database is missing some indexes that could help."
```

Akıllı Öngörüler performans günlüğü ile kullanabileceğiniz [Azure Log Analytics]( https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-sql) veya bir üçüncü taraf çözümü uyarılar ve raporlama özellikleri özel DevOps için.

## <a name="next-steps"></a>Sonraki adımlar
- Hakkında bilgi edinin [Intelligent Insights](sql-database-intelligent-insights.md) kavramları.
- Bilgi edinmek için nasıl [Intelligent Insights ile Azure SQL veritabanı performans sorunlarını giderme](sql-database-intelligent-insights-troubleshoot-performance.md).
- Bilgi edinmek için nasıl [Azure SQL Analytics kullanarak Azure SQL veritabanını İzle](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-sql).
- Bilgi edinmek için nasıl [toplamak ve Azure kaynaklarınızdan günlük verilerini kullanma](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).



