---
title: MariaDB için Azure veritabanı'nda izleme
description: Bu makalede izleme ve CPU, depolama ve bağlantı istatistikleri de dahil olmak üzere MariaDB için Azure veritabanı için uyarı ölçümleri açıklanır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 06/12/2019
ms.openlocfilehash: fb998edffed290bb7bc59945163f0fd48c55cbf5
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67612520"
---
# <a name="monitoring-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı'nda izleme
İzleme verilerini sunucularınız hakkında sorun giderme ve iş yükünüz için iyileştirmenize yardımcı olur. MariaDB için Azure veritabanı sunucunuzu davranışını öngörü sunan çeşitli ölçümleri sağlar.

## <a name="metrics"></a>Ölçümler
Tüm Azure ölçümleri bir dakikalık sıklığı, ve 30 günlük geçmişi her ölçüm sağlar. Ölçümler üzerinde uyarılar yapılandırabilirsiniz. Diğer görevler otomatik eylemleri ayarlama, Gelişmiş analiz gerçekleştirme ve geçmiş arşivleme içerir. Daha fazla bilgi için [Azure ölçümlerine genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

Adım adım yönergeler için bkz. [uyarıları ayarlamak nasıl](howto-alert-metric.md).

### <a name="list-of-metrics"></a>Ölçümlerin listesi
Bu ölçümler, MariaDB için Azure veritabanı için kullanılabilir:

|Ölçüm|Ölçüm görünen adı|Birim|Açıklama|
|---|---|---|---|
|cpu_percent|CPU yüzdesi|Percent|CPU yüzdesi kullanılıyor.|
|memory_percent|Bellek yüzdesi|Percent|Kullanılan bellek yüzdesi.|
|io_consumption_percent|G/ç yüzdesi|Percent|G/ç yüzdesi kullanılıyor.|
|storage_percent|Depolama yüzdesi|Percent|En fazla depolama sunucusu dışında kullanılan yüzdesi.|
|storage_used|Kullanılan depolama|Bayt|Kullanılan depolama miktarı. Hizmet tarafından kullanılan depolama, veritabanı dosyaları, işlem günlükleri ve sunucu günlüklerini içerebilir.|
|serverlog_storage_percent|Sunucu günlüğü depolama yüzdesi|Percent|Sunucunun en fazla sunucu günlük depolama dışında kullanılan sunucu günlük depolama yüzdesi.|
|serverlog_storage_usage|Kullanılan sunucu günlük depolama alanı|Bayt|Sunucu günlüğü depolama miktarı.|
|serverlog_storage_limit|Sunucu günlük depolama sınırı|Bayt|Bu sunucu için en fazla sunucu günlük depolama.|
|storage_limit|Depolama sınırı|Bayt|Bu sunucu için en fazla depolama alanı.|
|active_connections|Etkin bağlantılar|Count|Sunucu için etkin bağlantı sayısı.|
|connections_failed|Başarısız Bağlantılar|Count|Başarısız bağlantılar sunucu sayısı.|
|network_bytes_egress|Ağ Çıkışı|Bayt|Ağ çıkışı arasında etkin bağlantılar.|
|network_bytes_ingress|Ağ Girişi|Bayt|Ağ içinde arasında etkin bağlantılar.|

## <a name="server-logs"></a>Sunucu günlükleri

Yavaş sorgu günlüğü sunucunuzda etkinleştirebilirsiniz. Bu günlükler de Azure İzleyici günlüklerini, olay hub'ları ve depolama hesabı Azure tanılama günlükleri kullanılabilir. Günlüğe kaydetme hakkında daha fazla bilgi edinmek için [sunucu günlükleri](concepts-server-logs.md) sayfası.

## <a name="query-store"></a>Sorgu Deposu

[Query Store](concepts-query-store.md) sorgu süresi dahil olmak üzere üzerinde performans sorgu çalışma zamanı istatistikleri ve olayları bekleyin ve genel Önizleme özelliğidir. Özellik sorgu çalışma zamanı performans bilgileri kalıcı **mysql** şema. Çeşitli yapılandırma düğmelerini aracılığıyla veri depolama ve koleksiyon denetleyebilirsiniz.

## <a name="query-performance-insight"></a>Sorgu Performansı İçgörüleri

[Sorgu performansı İçgörüleri](concepts-query-performance-insight.md) Query Store, Azure portalından erişilebilir görselleştirmeleri sunmak için birlikte çalışır. Bu grafik, anahtar sorguları tanımlamak için performansı etkileyebilir olanak sağlar. Sorgu performansı İçgörüleri genel Önizleme aşamasındadır ve erişebileceğiniz **akıllı performans** MariaDB sunucunuzun portal sayfası için Azure veritabanı bölümü.

## <a name="performance-recommendations"></a>Performans Önerileri

[Performans önerileri](concepts-performance-recommendations.md) iş yükü performansı arttırmaya yönelik fırsatlar özelliği tanımlar. Genel Önizleme sürümü, performans, iş yüklerinizin performansını olanağına sahip yeni bir dizin oluşturmak için öneriler sağlar. Dizin önerileri üretmek için özelliği, şema ve sorgu Store tarafından bildirilen gibi iş yükü dahil olmak üzere, çeşitli veritabanı özelliklerini dikkate alır. Herhangi bir performans önerisi uyguladıktan sonra müşteriler bu değişikliklerin etkisini değerlendirmek için performans test etmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- Erişim ve Azure portalı, REST API veya CLI kullanarak ölçümleri dışarı aktarma hakkında daha fazla bilgi için bkz. [Azure ölçümlerine genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
  - Bkz: [uyarıları ayarlamak nasıl](howto-alert-metric.md) bir ölçüme göre bir uyarı oluşturma hakkında yönergeler için.
