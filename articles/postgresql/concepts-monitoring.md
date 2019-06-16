---
title: İzleme ve - tek bir sunucu PostgreSQL için Azure veritabanı'nda ayarlayın
description: Bu makalede, PostgreSQL - tek bir sunucu için Azure veritabanı izleme ve ayarlama özellikleri açıklanmaktadır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: b7d69e0fe16f96b0e3886c3736f8b91d4c06b446
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67063746"
---
# <a name="monitor-and-tune-azure-database-for-postgresql---single-server"></a>İzleme ve - tek bir sunucu PostgreSQL için Azure veritabanı ayarlama
İzleme verilerini sunucularınız hakkında sorun giderme ve iş yükünüz için iyileştirmenize yardımcı olur. PostgreSQL için Azure veritabanı sunucunuzu davranışını bir anlayış sağlamak için çeşitli izleme seçenekleri sağlar.

## <a name="metrics"></a>Ölçümler
PostgreSQL için Azure veritabanı, PostgreSQL sunucusu destekleyen kaynaklarda davranışını öngörü sunan çeşitli ölçümleri sağlar. Her ölçü bir dakikalık sıklığında yayılır ve 30 güne kadar geçmişi vardır. Ölçümler üzerinde uyarılar yapılandırabilirsiniz. Adım adım yönergeler için bkz. [uyarıları ayarlamak nasıl](howto-alert-on-metric.md). Diğer görevler otomatik eylemleri ayarlama, Gelişmiş analiz gerçekleştirme ve geçmiş arşivleme içerir. Daha fazla bilgi için [Azure ölçümlerine genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Ölçümlerin listesi
Bu ölçümler, PostgreSQL için Azure veritabanı için kullanılabilir:

|Ölçüm|Ölçüm görünen adı|Birim|Açıklama|
|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|CPU yüzdesi kullanılıyor.|
|memory_percent|Bellek yüzdesi|Yüzde|Kullanılan bellek yüzdesi.|
|io_consumption_percent|G/ç yüzdesi|Yüzde|G/ç yüzdesi kullanılıyor.|
|storage_percent|Depolama yüzdesi|Yüzde|En fazla depolama sunucusu dışında kullanılan yüzdesi.|
|storage_used|Kullanılan depolama|Bayt|Kullanılan depolama miktarı. Hizmet tarafından kullanılan depolama, veritabanı dosyaları, işlem günlükleri ve sunucu günlüklerini içerebilir.|
|storage_limit|Depolama sınırı|Bayt|Bu sunucu için en fazla depolama alanı.|
|serverlog_storage_percent|Sunucu günlüğü depolama yüzdesi|Yüzde|Sunucunun en fazla sunucu günlük depolama dışında kullanılan sunucu günlük depolama yüzdesi.|
|serverlog_storage_usage|Kullanılan sunucu günlük depolama alanı|Bayt|Sunucu günlüğü depolama miktarı.|
|serverlog_storage_limit|Sunucu günlük depolama sınırı|Bayt|Bu sunucu için en fazla sunucu günlük depolama.|
|active_connections|Etkin bağlantılar|Count|Sunucu için etkin bağlantı sayısı.|
|connections_failed|Başarısız Bağlantılar|Count|Başarısız bağlantılar sunucu sayısı.|
|network_bytes_egress|Ağ Çıkışı|Bayt|Ağ çıkışı arasında etkin bağlantılar.|
|network_bytes_ingress|Ağ Girişi|Bayt|Ağ içinde arasında etkin bağlantılar.|
|backup_storage_used|Kullanılan yedekleme depolama alanı|Bayt|Kullanılan yedekleme depolama alanı miktarı.|

## <a name="server-logs"></a>Sunucu günlükleri
Sunucunuzda, günlüğe kaydetmeyi etkinleştirebilirsiniz. Bu günlükler Azure tanılama günlükleri aracılığıyla da kullanılabilir [Azure İzleyici günlükleri](../azure-monitor/log-query/log-query-overview.md), olay hub'ları ve depolama hesabı. Günlüğe kaydetme hakkında daha fazla bilgi edinmek için [sunucu günlükleri](concepts-server-logs.md) sayfası.

## <a name="query-store"></a>Sorgu Deposu
[Query Store](concepts-query-store.md) süresi dahil olmak üzere üzerinde performans sorgu çalışma zamanı istatistikleri ve olayları bekleyin sorgu izler. Özellik adlı bir sistem veritabanında sorgu çalışma zamanı performans bilgilerini devam ederse **azure_sys** query_store şema altında. Çeşitli yapılandırma düğmelerini aracılığıyla veri depolama ve koleksiyon denetleyebilirsiniz.

## <a name="query-performance-insight"></a>Sorgu Performansı İçgörüleri
[Sorgu performansı İçgörüleri](concepts-query-performance-insight.md) Query Store, Azure portalından erişilebilir görselleştirmeleri sunmak için birlikte çalışır. Bu grafik, anahtar sorguları tanımlamak için performansı etkileyebilir olanak sağlar. Sorgu performansı Insightis erişilebilir **destek + sorun giderme** PostgreSQL sunucunuzun portal sayfası için Azure veritabanı bölümü.

## <a name="performance-recommendations"></a>Performans Önerileri
[Performans önerileri](concepts-performance-recommendations.md) iş yükü performansı arttırmaya yönelik fırsatlar özelliği tanımlar. Performans önerileri sağlar, iş yüklerinizin performansını olanağına sahip yeni bir dizin oluşturmak için öneriler. Dizin önerileri üretmek için özelliği, şema ve sorgu Store tarafından bildirilen gibi iş yükü dahil olmak üzere, çeşitli veritabanı özelliklerini dikkate alır. Herhangi bir performans önerisi uyguladıktan sonra müşteriler bu değişikliklerin etkisini değerlendirmek için performans test etmeniz gerekir. 

## <a name="next-steps"></a>Sonraki adımlar
- Bkz: [uyarıları ayarlamak nasıl](howto-alert-on-metric.md) bir ölçüme göre bir uyarı oluşturma hakkında yönergeler için.
- Erişim ve Azure portalı, REST API veya CLI kullanarak ölçümleri dışarı aktarma hakkında daha fazla bilgi için bkz. [Azure ölçümlerine genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
- Blogumuzu okuyun [sunucunuzu izlemek için en iyi yöntemler](https://azure.microsoft.com/blog/best-practices-for-alerting-on-metrics-with-azure-database-for-postgresql-monitoring/).
