---
title: MariaDB için Azure veritabanı'nda izleme
description: Bu makalede izleme ve CPU, depolama ve bağlantı istatistikleri de dahil olmak üzere MariaDB için Azure veritabanı için uyarı ölçümleri açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: mariadb
ms.topic: conceptual
ms.date: 11/10/2018
ms.openlocfilehash: 476e74a4d167fb3e9158f07cc5c073f129a74daa
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53545901"
---
# <a name="monitoring-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı'nda izleme
İzleme verilerini sunucularınız hakkında sorun giderme ve iş yükünüz için iyileştirmenize yardımcı olur. MariaDB için Azure veritabanı sunucunuzu davranışını öngörü sunan çeşitli ölçümleri sağlar.

## <a name="metrics"></a>Ölçümler
Tüm Azure ölçümleri bir dakikalık sıklığı, ve 30 günlük geçmişi her ölçüm sağlar. Ölçümler üzerinde uyarılar yapılandırabilirsiniz. Diğer görevler otomatik eylemleri ayarlama, Gelişmiş analiz gerçekleştirme ve geçmiş arşivleme içerir. Daha fazla bilgi için bkz. [Azure ölçümlerine genel bakış] (.. /Monitoring-and-Diagnostics/Monitoring-Overview-Metrics.MD).

<!--For step by step guidance, see [How to set up alerts](howto-alert-on-metric.md). -->

### <a name="list-of-metrics"></a>Ölçümlerin listesi
Bu ölçümler, MariaDB için Azure veritabanı için kullanılabilir:

|Ölçüm|Ölçüm görünen adı|Birim|Açıklama|
|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|CPU yüzdesi kullanılıyor.|
|memory_percent|Bellek yüzdesi|Yüzde|Kullanılan bellek yüzdesi.|
|io_consumption_percent|G/ç yüzdesi|Yüzde|G/ç yüzdesi kullanılıyor.|
|storage_percent|Depolama yüzdesi|Yüzde|En fazla depolama sunucusu dışında kullanılan yüzdesi.|
|storage_used|Kullanılan depolama|Bayt|Kullanılan depolama miktarı. Hizmet tarafından kullanılan depolama, veritabanı dosyaları, işlem günlükleri ve sunucu günlüklerini içerebilir.|
|serverlog_storage_percent|Sunucu günlüğü depolama yüzdesi|Yüzde|Sunucunun en fazla sunucu günlük depolama dışında kullanılan sunucu günlük depolama yüzdesi.|
|serverlog_storage_usage|Kullanılan sunucu günlük depolama alanı|Bayt|Sunucu günlüğü depolama miktarı.|
|serverlog_storage_limit|Sunucu günlük depolama sınırı|Bayt|Bu sunucu için en fazla sunucu günlük depolama.|
|storage_limit|Depolama sınırı|Bayt|Bu sunucu için en fazla depolama alanı.|
|active_connections|Etkin bağlantılar|Sayı|Sunucu için etkin bağlantı sayısı.|
|connections_failed|Başarısız Bağlantılar|Sayı|Başarısız bağlantılar sunucu sayısı.|
|network_bytes_egress|Ağ Çıkışı|Bayt|Ağ çıkışı arasında etkin bağlantılar.|
|network_bytes_ingress|Ağ Girişi|Bayt|Ağ içinde arasında etkin bağlantılar.|

## <a name="server-logs"></a>Sunucu günlükleri
Yavaş sorgu günlüğü sunucunuzda etkinleştirebilirsiniz. Günlüğe kaydetme hakkında daha fazla bilgi edinmek için [sunucu günlükleri](concepts-server-logs.md) sayfası.

## <a name="next-steps"></a>Sonraki adımlar
- Erişim ve Azure portalı, REST API veya CLI kullanarak ölçümleri dışarı aktarma hakkında daha fazla bilgi için bkz. [Azure ölçümlerine genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

<!-- - See [How to set up alerts](howto-alert-on-metric.md) for guidance on creating an alert on a metric.-->
