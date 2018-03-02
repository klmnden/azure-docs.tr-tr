---
title: "Azure veritabanında PostgreSQL için izleme"
description: "Bu makalede izleme ve Azure veritabanı için CPU, depolama ve bağlantı istatistikleri de dahil olmak üzere PostgreSQL için uyarı için ölçümleri açıklanmaktadır."
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: d0a57fe6d7b1040c32f6d67e2bf0259176c72099
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="monitoring-in-azure-database-for-postgresql"></a>Azure veritabanında PostgreSQL için izleme
İzleme verilerini sunucularınız hakkında sorun giderme ve işleminizi iş yükü için en iyi duruma yardımcı olur. Azure veritabanı PostgreSQL için PostgreSQL sunucunun destekleyen kaynaklarda davranışını bir anlayış veren çeşitli ölçümleri sağlar. 

## <a name="metrics"></a>Ölçümler
Tüm Azure ölçümlerini bir dakikalık sıklık, ve her ölçümü geçmişi 30 gün sağlar. Ölçümleri uyarılar yapılandırabilirsiniz. Adım adım yönergeler için bkz [uyarıları ayarlamak nasıl](howto-alert-on-metric.md). Diğer görevleri otomatik eylemler oluşturma, gelişmiş analizler gerçekleştirme ve geçmiş arşivleme içerir. Daha fazla bilgi için bkz: [Azure ölçümleri genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Ölçümleri listesi
Bu ölçümler Azure veritabanı PostgreSQL için kullanılabilir:

|Ölçüm|Ölçüm görünen adı|Birim|Açıklama|
|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|CPU yüzdesi kullanımda.|
|memory_percent|Bellek yüzdesi|Yüzde|Kullanılan bellek yüzdesi.|
|io_consumption_percent|G/ç yüzdesi|Yüzde|G/ç yüzdesi kullanımda.|
|storage_percent|Depolama yüzdesi|Yüzde|En fazla sunucunun dışında kullanılan depolama alanı yüzdesi.|
|storage_used|Kullanılan depolama|Bayt|Kullanımdaki depolama miktarı. Veritabanı dosyaları, işlem günlüklerinin ve sunucu günlüklerini hizmet tarafından kullanılan depolama alanını içerir.|
|storage_limit|Depolama sınırı|Bayt|Bu sunucu için en fazla depolama alanı.|
|active_connections|Toplam etkin bağlantılar|Sayı|Sunucuya etkin bağlantı sayısı.|
|connections_failed|Toplam başarısız bağlantıları|Sayı|Sunucuya başarısız bağlantılarının sayısı.|


## <a name="next-steps"></a>Sonraki adımlar
- Bkz: [uyarıları ayarlamak nasıl](howto-alert-on-metric.md) ölçüm üzerinde bir uyarı oluşturma konusunda yönergeler için.
- Erişim ve Azure portalı, REST API veya CLI kullanarak ölçümleri dışarı aktarma hakkında daha fazla bilgi için bkz: [Azure ölçümleri genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
