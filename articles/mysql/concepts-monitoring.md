---
title: MySQL için Azure veritabanı'nda izleme
description: Bu makalede izleme ve CPU, depolama ve bağlantı istatistikleri de dahil olmak üzere MySQL için Azure veritabanı için uyarı ölçümleri açıklanır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 06/05/2019
ms.openlocfilehash: 0122f952e586d0535fc2e482c7b78266f8809272
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67062446"
---
# <a name="monitoring-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı'nda izleme
İzleme verilerini sunucularınız hakkında sorun giderme ve iş yükünüz için iyileştirmenize yardımcı olur. MySQL için Azure veritabanı sunucunuzu davranışını öngörü sunan çeşitli ölçümleri sağlar.

## <a name="metrics"></a>Ölçümler
Tüm Azure ölçümleri bir dakikalık sıklığı, ve 30 günlük geçmişi her ölçüm sağlar. Ölçümler üzerinde uyarılar yapılandırabilirsiniz. Adım adım yönergeler için bkz. [uyarıları ayarlamak nasıl](howto-alert-on-metric.md). Diğer görevler otomatik eylemleri ayarlama, Gelişmiş analiz gerçekleştirme ve geçmiş arşivleme içerir. Daha fazla bilgi için [Azure ölçümlerine genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Ölçümlerin listesi
Bu ölçümler, MySQL için Azure veritabanı için kullanılabilir:

|Ölçüm|Ölçüm görünen adı|Birim|Açıklama|
|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|CPU yüzdesi kullanılıyor.|
|memory_percent|Bellek yüzdesi|Yüzde|Kullanılan bellek yüzdesi.|
|io_consumption_percent|G/ç yüzdesi|Yüzde|G/ç yüzdesi kullanılıyor.|
|storage_percent|Depolama yüzdesi|Yüzde|En fazla depolama sunucusu dışında kullanılan yüzdesi.|
|storage_used|Kullanılan depolama|Bayt|Kullanılan depolama miktarı. Hizmet tarafından kullanılan depolama, veritabanı dosyaları, işlem günlükleri ve sunucu günlüklerini içerebilir.|
|serverlog_storage_percent|Sunucu günlüğü depolama yüzdesi|Yüzde|Sunucunun en fazla sunucu günlük depolama dışında kullanılan sunucu günlük depolama yüzdesi.|
|serverlog_storage_usage|Kullanılan sunucu günlük depolama alanı|Bayt|Sunucu günlüğü depolama miktarı.|
|serverlog_storage_limit|Sunucu günlük depolama sınırı|Bayt|Bu sunucu için en fazla sunucu günlük depolama.|
|storage_limit|Depolama sınırı|Bayt|Bu sunucu için en fazla depolama alanı.|
|active_connections|Etkin bağlantılar|Count|Sunucu için etkin bağlantı sayısı.|
|connections_failed|Başarısız Bağlantılar|Sayı|Başarısız bağlantılar sunucu sayısı.|
|seconds_behind_master|Saniyeler içinde çoğaltma gecikmesi|Count|Çoğaltma sunucusu, ana sunucu karşı geciken saniye sayısı.|
|network_bytes_egress|Ağ Çıkışı|Bayt|Ağ çıkışı arasında etkin bağlantılar.|
|network_bytes_ingress|Ağ Girişi|Bayt|Ağ içinde arasında etkin bağlantılar.|
|backup_storage_used|Kullanılan yedekleme depolama alanı|Bayt|Kullanılan yedekleme depolama alanı miktarı.|

## <a name="server-logs"></a>Sunucu günlükleri
Yavaş sorgu etkinleştirebilir ve sunucunuzda günlüğü denetleme. Bu günlükler de Azure İzleyici günlüklerini, olay hub'ları ve depolama hesabı Azure tanılama günlükleri kullanılabilir. Günlüğe kaydetme hakkında daha fazla bilgi edinmek için [denetim günlükleri](concepts-audit-logs.md) ve [yavaş sorgu günlüklerini](concepts-server-logs.md) makaleler.

## <a name="query-store"></a>Sorgu Deposu
[Query Store](concepts-query-store.md) sorgu süresi dahil olmak üzere üzerinde performans sorgu çalışma zamanı istatistikleri ve olayları bekleyin ve genel Önizleme özelliğidir. Özellik sorgu çalışma zamanı performans bilgileri kalıcı **mysql** şema. Çeşitli yapılandırma düğmelerini aracılığıyla veri depolama ve koleksiyon denetleyebilirsiniz.

## <a name="query-performance-insight"></a>Sorgu Performansı İçgörüleri
[Sorgu performansı İçgörüleri](concepts-query-performance-insight.md) Query Store, Azure portalından erişilebilir görselleştirmeleri sunmak için birlikte çalışır. Bu grafik, anahtar sorguları tanımlamak için performansı etkileyebilir olanak sağlar. Sorgu performansı İçgörüleri genel Önizleme aşamasındadır ve erişebileceğiniz **akıllı performans** Azure veritabanınızı MySQL server'ın portal sayfasının bölümünde.

## <a name="performance-recommendations"></a>Performans Önerileri
[Performans önerileri](concepts-performance-recommendations.md) iş yükü performansı arttırmaya yönelik fırsatlar özelliği tanımlar. Genel Önizleme sürümü, performans, iş yüklerinizin performansını olanağına sahip yeni bir dizin oluşturmak için öneriler sağlar. Dizin önerileri üretmek için özelliği, şema ve sorgu Store tarafından bildirilen gibi iş yükü dahil olmak üzere, çeşitli veritabanı özelliklerini dikkate alır. Herhangi bir performans önerisi uyguladıktan sonra müşteriler bu değişikliklerin etkisini değerlendirmek için performans test etmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar
- Bkz: [uyarıları ayarlamak nasıl](howto-alert-on-metric.md) bir ölçüme göre bir uyarı oluşturma hakkında yönergeler için.
- Erişim ve Azure portalı, REST API veya CLI kullanarak ölçümleri dışarı aktarma hakkında daha fazla bilgi için bkz. [Azure ölçümlerine genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
- Blogumuzu okuyun [sunucunuzu izlemek için en iyi yöntemler](https://azure.microsoft.com/blog/best-practices-for-alerting-on-metrics-with-azure-database-for-mysql-monitoring/).
