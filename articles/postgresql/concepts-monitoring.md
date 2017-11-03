---
title: "Azure veritabanında PostgreSQL için izleme | Microsoft Docs"
description: "Bu makalede izleme ve Azure veritabanı için CPU, sınırları, depolama ve bağlantı istatistikleri de dahil olmak üzere PostgreSQL için uyarı için ölçümleri açıklanmaktadır."
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 10/24/2017
ms.openlocfilehash: d48159fbc5e52bf1916a744e3912ca7ceb0ab60f
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="monitoring-in-azure-database-for-postgresql"></a>Azure veritabanında PostgreSQL için izleme
İzleme verilerini sunucularınız hakkında sorun giderme ve işleminizi iş yükü için en iyi duruma yardımcı olur. Azure veritabanı PostgreSQL için PostgreSQL sunucunun destekleyen kaynaklarda davranışını bir anlayış veren çeşitli ölçümleri sağlar. 

## <a name="metrics"></a>Ölçümler
Tüm Azure ölçümlerini bir dakikalık sıklık, ve her ölçümü geçmişi 30 gün sağlar. 

Ölçümleri uyarılar yapılandırabilirsiniz. Adım adım yönergeler için bkz [uyarıları ayarlamak nasıl](howto-alert-on-metric.md). 

Diğer görevleri otomatik eylemler oluşturma, gelişmiş analizler gerçekleştirme ve geçmiş arşivleme içerir. Daha fazla bilgi için bkz: [Azure ölçümleri genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Ölçümleri listesi
Bu ölçümler Azure veritabanı PostgreSQL için kullanılabilir:

|Ölçüm|Ölçüm görünen adı|Birim|Açıklama|
|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|CPU yüzdesi kullanımda.|
|compute_limit|Birim sınırı işlem|Sayı|Bu sunucunun en fazla işlem birimi sayısı|
|compute_consumption_percent|Birim yüzdesi işlem|Yüzde|En fazla sunucunun dışında kullanılan işlem birimleri yüzdesi.|
|memory_percent|Bellek yüzdesi|Yüzde|Kullanılan bellek yüzdesi.|
|io_consumption_percent|G/ç yüzdesi|Yüzde|G/ç yüzdesi kullanımda.|
|storage_percent|Depolama yüzdesi|Yüzde|En fazla sunucunun dışında kullanılan depolama alanı yüzdesi.|
|storage_used|Kullanılan depolama|Bayt|Kullanımdaki depolama miktarı. Veritabanı dosyaları, işlem günlüklerinin ve sunucu günlüklerini hizmet tarafından kullanılan depolama alanını içerir.|
|storage_limit|Depolama sınırı|Bayt|Bu sunucu için en fazla depolama alanı.|
|active_connections|Toplam etkin bağlantılar|Sayı|Sunucuya etkin bağlantı sayısı.|
|connections_failed|Toplam başarısız bağlantıları|Sayı|Sunucuya başarısız bağlantılarının sayısı.|


> [!NOTE]
> İşlem bellek ve CPU birim oluşur. İşlem birim yüzdesidir max (% bellek, cpu %). Hangi işlem birim yüzdesi değişiklikler katkıda bulunan saptamak için bellek ve cpu grafiklerini inceleyin. Daha fazla bilgi için bkz: [birimleri işlem](concepts-compute-unit-and-storage.md).

## <a name="next-steps"></a>Sonraki adımlar
- Adım adım yönergeler için bkz [uyarıları ayarlamak nasıl](howto-alert-on-metric.md). 
- Erişim ve Azure portalı, REST API veya CLI kullanarak ölçümleri dışarı aktarma hakkında daha fazla bilgi için bkz: [Azure ölçümleri genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
