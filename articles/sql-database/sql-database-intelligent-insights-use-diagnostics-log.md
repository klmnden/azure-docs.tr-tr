---
title: "Akıllı Öngörüler Performans Tanılama Günlüğü - Azure SQL veritabanı | Microsoft Docs"
description: "Akıllı Öngörüler Azure SQL veritabanı performans sorunlarını tanılama günlüğünü sağlar"
services: sql-database
author: danimir
manager: craigg
ms.reviewer: carlrab
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: article
ms.date: 09/25/2017
ms.author: v-daljep
ms.openlocfilehash: b380d3a8a35750602a4a0d20d595f71b125fc118
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="use-the-intelligent-insights-azure-sql-database-performance-diagnostics-log"></a>Akıllı Öngörüler Azure SQL veritabanı performans tanılama günlüğü kullanın

Bu sayfa tarafından oluşturulan Azure SQL veritabanı performans tanılama günlük kullanımı hakkında bilgi sağlar. [akıllı Öngörüler](sql-database-intelligent-insights.md), biçimi ve özel geliştirme gereksinimleriniz için içerdiği veriler. Bu tanılama günlük gönderebilirsiniz [Azure günlük analizi](../log-analytics/log-analytics-azure-sql.md), [Azure Event Hubs](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md), [Azure Storage](sql-database-metrics-diag-logging.md#stream-into-storage), veya bir üçüncü taraf çözümü özel DevOps uyarı ve raporlama için yetenekleri.

## <a name="log-header"></a>Günlük üstbilgisi

Tanılama günlük akıllı Öngörüler bulgularını çıktısını almak için JSON standart biçimi kullanır. Bir akıllı Öngörüler günlüğü erişmek için tam kategori sabit değer "SQLInsights" özelliğidir.

Günlük üstbilgisinin yaygındır ve gösteren bir giriş oluşturulduğu zaman damgası (TimeGenerated) oluşur. Ayrıca, giriş teklifiyle belirli bir SQL veritabanını başvurduğu bir kaynak kimliği (ResourceId) içerir. Kategori (kategori), düzey (düzey) ve işlem adı (OperationName) değerleri değiştirmeyin özellikler sabittir. Bunlar, günlük girişinin bilgi amaçlıdır ve akıllı Öngörüler (SQLInsights) geldiğini belirtir.

```json
"TimeGenerated" : "2017-9-25 11:00:00", // time stamp of the log entry
"ResourceId" : "database identifier", // value points to a database resource
"Category": "SQLInsights", // fixed property
"Level" : "Informational", // fixed property
"OperationName" : "Insight", // fixed property
```

## <a name="issue-id-and-database-affected"></a>Sorun kimliği ve etkilenen veritabanı

Sorun kimliği özelliği (issueId_d) bunlar çözümlenene kadar benzersiz olarak performans sorunlarını izleme, bir yol sağlar. Akıllı Öngörüler her sorun yaşam döngüsü "Active", "Doğrulanıyor" veya "Tamamlandı" olarak görür. Her bu durum aşamalar, akıllı Öngörüler birden çok olay kayıtlarını günlüğe kaydedebilir. Her bu girişleri için sorun kimliği numarasını benzersiz kalır. Akıllı Öngörüler sorunu kendi döngüsü boyunca izler ve bir öngörü 15 dakikada bir tanılama günlüğüne oluşturur.

Bir performans sorunu algılandıktan sonra ve onu sürer sürece, sorunu altındaki durum (status_s) özelliğini "Etkin" olarak bildirilen için. Algılanan Sorun azaltıldığından sonra onu doğrulandı ve altındaki durum (status_s) özelliğini "Doğrulanıyor" olarak bildirilen. Sorunun artık mevcut değilse durumu (status_s) özelliği bu sorunu "Tamamlandı" bildirir.

Sorun kimliği ile birlikte (intervalStartTime_t) başlangıç ve bitiş (intervalEndTme_t) zaman damgaları tanılama günlüğüne bildirilen bir sorunla ilgili belirli olay tanılama günlük raporlar.

Esnek havuz (elasticPoolName_s) özelliği veritabanı ile ilgili bir sorun ait hangi esnek havuz gösterir. Veritabanını bir esnek havuz parçası değilse, bu özellik değeri yok. Bir sorun algılandı veritabanı veritabanı adı (databaseName_s) özelliğinde ifşa edilmiştir.

```json
"intervalStartTime_t": "2017-9-25 11:00", // start of the issue reported time stamp
"intervalEndTme_t":"2017-9-25 12:00", // end of the issue reported time stamp
"elasticPoolName_s" : "", // resource elastic pool (if applicable) 
"databaseName_s" : "db_name",  // database name
"issueId_d" : 1525, // unique ID of the issue detected
"status_s" : "Active" // status of the issue – possible values: "Active", "Verifying", and "Complete"
```

## <a name="detected-issues"></a>Algılanan sorunları

Akıllı Öngörüler performans günlüğü sonraki bölümü yerleşik yapay zeka algılandı performans sorunları içerir. Algılama özellikleri JSON tanılama günlük içinde bildirilen. Bu algılamaların bir sorun kategorisi, sorun, etkilenen sorgular ve ölçümleri etkisini oluşur. Algılama özellikleri algılandı birden çok performans sorunlarını içerebilir.

Algılanan performans sorunlarını aşağıdaki algılamaların özelliği yapısıyla rapor edilir:

```json
"detections_s" : [{
"impact" : 1 to 3, // impact of the issue detected, possible values 1-3 (1 low, 2 moderate, 3 high impact)
"category" : "Detectable performance pattern", // performance issue detected, see the table
"details": <Details outputted> // details of an issue (see the table)
}] 
```

Aşağıdaki tabloda algılanabilir performans modellerini ve tanılama günlük yüzdelik ayrıntıları sağlanır.

### <a name="detection-category"></a>Algılama kategorisi

Kategori (kategori) özelliği algılanabilir performans modellerini kategorisini açıklar. Tüm olası kategorileri algılanabilir performans desenler için aşağıdaki tabloya bakın. Daha fazla bilgi için bkz: [akıllı Insights ile veritabanı performans sorunlarını giderme](sql-database-intelligent-insights-troubleshoot-performance.md).

Algılandı, performans sorunu tanılamada yüzdelik ayrıntıları günlük dosyası farklılık buna göre.

| Algılanabilir performans desenleri | Yüzdelik ayrıntıları |
| :------------------- | ------------------- |
| Kaynak sınırları ulaşmasını | <li>Etkilenen kaynakları</li><li>Sorgu karmaları</li><li>Kaynak tüketimi yüzdesi</li> |
| İş yükü artış | <li>Yürütme artan sorgularının sayısı</li><li>İş yükü artış en büyük katkısı sorgularıyla sorgu karmalarını</li> |
| Bellek baskısı | <li>Bellek yazıcısı</li> |
| Kilitleme | <li>Sorgu karmaları etkilenen</li><li>Sorgu karmaları engelleme</li> |
| Artan MAXDOP | <li>Sorgu karmaları</li><li>CXP bekleme süreleri</li><li>Kez bekleyin</li> |
| Pagelatch çakışması | <li>Çekişme neden sorguların karmaları sorgulama</li> |
| Dizin eksik | <li>Sorgu karmaları</li> |
| Yeni Sorgu | <li>Yeni sorgular sorgu karması</li> |
| Olağan dışı bekleme İstatistiği | <li>Olağan dışı bekleme türleri</li><li>Sorgu karmaları</li><li>Sorgu bekleme süreleri</li> |
| TempDB çakışması | <li>Çekişme neden sorguların karmaları sorgulama</li><li>Genel veritabanı pagelatch Çekişme bekleme süresi [%] için sorgu attribution</li> |
| Esnek havuz DTU azalması | <li>Esnek havuz</li><li>Üst DTU tüketen veritabanı</li><li>Havuz üst tüketici tarafından kullanılan DTU yüzdesi</li> |
| Regresyon planlama | <li>Sorgu karmaları</li><li>İyi plan kimlikleri</li><li>Hatalı planı kimlikleri</li> |
| Veritabanı kapsamlı yapılandırma değeri Değiştir | <li>Veritabanı kapsamlı yapılandırma değişiklikleri için varsayılan değerleri karşılaştırma</li> |
| İstemci yavaş | <li>Sorgu karmaları</li><li>Kez bekleyin</li> |
| Fiyatlandırma katmanı indirgeme | <li>Metin bildirimi</li> |

### <a name="impact"></a>Etki

Etkisi (etki algılanan davranış ne kadar katkıda bulunan bir veritabanı sahip sorun özelliği tanımlar). 3 yüksek katkı olarak 3, 2 olarak orta ve düşük katkı olarak 1 ile 1 arasında etkiler. Etkisi değeri, gereksinimlerinize bağlı olarak özel uyarı Otomasyon için bir giriş olarak kullanılıyor olabilir. Özellik sorguları, belirli bir algılama tarafından etkilenen karmaları sorgu listesini etkilenen (QueryHashes) sağlar.

### <a name="impacted-queries"></a>Etkilenen sorguları

Akıllı Öngörüler günlük sonraki bölümü tarafından algılanan performans sorunlarını etkilendi belirli sorguları hakkında bilgi sağlar. Bu bilgiler impact_s özelliğinde katıştırılmış nesnelerinin bir dizisi olarak ifşa edilmiştir. Varlıklar ve ölçümleri etki özelliğinin oluşur. Varlıkların belirli bir sorgu bakın (tür: Sorgu). Benzersiz sorgu karma değer (değer) özelliği altında ifşa edilmiştir. Ayrıca, her duyurulmuş sorguları bir ölçüm ve bir değer tarafından algılanan performans sorunu gösteren izlenir.

Aşağıdaki günlük örnekte sorgu karma 0x9102EXZ4 ile artan bir yürütme süresi olan algılandı (ölçüm: DurationIncreaseSeconds). Bu belirli bir sorgu yürütmek için uzun 110 saniye sürdü 110 saniye değerini gösterir. Birden çok sorgu algıladığı için bu belirli günlük bölüm birden çok sorgu girişleri içerebilir.

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

Bildirilen her ölçümü ölçü saniye, sayı ve yüzde olası değerler ile altındaki ölçüm (ölçüm) özelliğini sağlanır. Değer (değer) özelliğinde bildirilen ölçülen ölçüm değer.

Saniye cinsinden ölçü birimi DurationIncreaseSeconds özelliği sağlar. CriticalErrorCount ölçü bir hata sayısı temsil eden bir sayıdır.

```json
"metric" : "DurationIncreaseSeconds", // issue metric type – possible values: DurationIncreaseSeconds, CriticalErrorCount, WaitingSeconds
"value" : 102 // value of the measured metric (in this case seconds)
```

## <a name="root-cause-analysis-and-improvement-recommendations"></a>Kök neden analizi ve geliştirme önerileri

Akıllı Öngörüler performans günlüğü son parçası tanımlanan performans düşüşünü sorunu otomatik kök neden analizi için geçerlidir. Kök neden analizi (rootCauseAnalysis_s) özelliğinde İnsan kolay duyuruları bilgileri görüntülenir. Geliştirme önerileri, mümkün olduğunda günlüğüne dahil edilir.

```json
// example of reported root cause analysis of the detected performance issue, in a human-readable format

"rootCauseAnalysis_s" : "High data IO caused performance to degrade. It seems that this database is missing some indexes that could help."
```

Akıllı Öngörüler performans günlüğü ile kullanabileceğiniz [Azure günlük analizi]( https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-sql) veya bir üçüncü taraf çözümü uyarı ve raporlama özellikleri özel DevOps için.

## <a name="next-steps"></a>Sonraki adımlar
- Hakkında bilgi edinin [akıllı Öngörüler](sql-database-intelligent-insights.md) kavramları.
- Bilgi edinmek için nasıl [akıllı Insights ile Azure SQL veritabanı performans sorunlarını giderme](sql-database-intelligent-insights-troubleshoot-performance.md).
- Bilgi edinmek için nasıl [Azure SQL analizi kullanarak Azure SQL veritabanı izleme](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-sql).
- Bilgi edinmek için nasıl [toplamak ve Azure kaynaklarınızdan günlük verilerini tüketen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).



