---
title: MariaDB için Azure veritabanı sunucusu günlükleri
description: MariaDB ve farklı günlüğe kaydetme düzeylerini etkinleştirmek için kullanılabilir parametreleri için Azure veritabanı'nda kullanılabilir günlükleri açıklar.
author: rachel-msft
ms.author: raagyema
ms.service: mariadb
ms.topic: conceptual
ms.date: 04/29/2019
ms.openlocfilehash: 8a78a9b8f0772a83e45ac2b926878e61e6ee2e61
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64926329"
---
# <a name="server-logs-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı'nda sunucu günlüklerini
MariaDB için Azure veritabanı'nda yavaş sorgu günlüğü kullanıcılar tarafından kullanılabilir. İşlem günlüğü erişimi desteklenmiyor. Yavaş sorgu günlüğü, sorun giderme için performans sorunlarını tanımlamak için kullanılabilir.

Yavaş sorgu günlüğü hakkında daha fazla bilgi için bkz: MariaDB belgelerine [yavaş sorgu günlüğü](https://mariadb.com/kb/en/library/slow-query-log-overview/).

## <a name="access-server-logs"></a>Sunucu günlüklerine erişme
Liste ve Azure veritabanı, Azure portalı ve Azure CLI kullanarak MariaDB için sunucu günlüklerine indirin.

Azure portalında MariaDB için Azure veritabanı sunucunuza seçin. Altında **izleme** başlığı seçin **sunucu günlükleri** sayfası.

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI kullanarak sunucu günlükleri ve erişimi Yapılandır](howto-configure-server-logs-cli.md).

## <a name="log-retention"></a>Günlük tutma
Günlükleri, bunların oluşturma yedi güne kadar kullanılabilir. Kullanılabilir günlükleri toplam boyutu 7 GB aşarsa alanı olana kadar eski dosyalar silinir.

Günlükleri Döndürülmüş her 24 saat veya 7 GB, hangisi gelir önce.

## <a name="configure-logging"></a>Günlük tutmayı yapılandırma
Yavaş sorgu günlüğü varsayılan olarak devre dışıdır. Bunu etkinleştirmek için slow_query_log açık olarak ayarlayın.

Ayarlayabileceğiniz diğer parametreler şunlardır:

- **long_query_time**: sorgu (saniye cinsinden) long_query_time uzun sürerse, sorgu günlüğe kaydedilir. Varsayılan değer 10 saniyedir.
- **log_slow_admin_statements**: ON slow_query_log için yazılan deyimlerinde ALTER_TABLE ve ANALYZE_TABLE gibi yönetim deyimleri içeriyorsa.
- **log_queries_not_using_indexes**: dizinleri kullanmayan sorgular için slow_query_log kaydedilip kaydedilmeyeceğini belirler
- **log_throttle_queries_not_using_indexes**: Bu parametre için yavaş sorgu günlüğü yazılabilir olmayan dizin sorguların sayısını sınırlar. Log_queries_not_using_indexes ON olarak ayarlandığında bu parametre etkinleşir.

MariaDB bkz [yavaş sorgu günlüğü belgeleri](https://mariadb.com/kb/en/library/slow-query-log-overview/) yavaş sorgu günlüğü parametrelerini tam açıklamaları için.

## <a name="diagnostic-logs"></a>Tanılama günlükleri
MariaDB için Azure veritabanı Azure İzleyici tanılama günlükleri ile tümleştirilir. Yavaş sorgu günlüklerini MariaDB sunucunuzda etkinleştirdikten sonra bunları Azure İzleyici günlüklerine, Event Hubs veya Azure depolama yayılan sahip olmayı seçebilirsiniz. Tanılama günlüklerini etkinleştirme hakkında daha fazla bilgi edinmek için nasıl bölümüne bakın. [tanılama günlükleri belgeleri](../azure-monitor/platform/diagnostic-logs-overview.md).

> [!IMPORTANT]
> Sunucu günlükleri için tanılama bu özellik yalnızca genel amaçlı ve bellek için iyileştirilmiş kullanılabilir [fiyatlandırma katmanları](concepts-pricing-tiers.md).

Aşağıdaki tabloda, her oturum açma yenilikler açıklanır. Yer alan alanlar ve göründükleri sırayla çıkış yöntemine bağlı olarak değişebilir.

| **Özellik** | **Açıklama** |
|---|---|
| `TenantId` | Kiracı Kimliğiniz |
| `SourceSystem` | `Azure` |
| `TimeGenerated` [UTC] | Günlük UTC olarak kaydedildiği zaman damgası |
| `Type` | Günlük türü. Her zaman `AzureDiagnostics` |
| `SubscriptionId` | Sunucunun ait olduğu aboneliğin GUID |
| `ResourceGroup` | Sunucunun ait olduğu kaynak grubu adı |
| `ResourceProvider` | Kaynak sağlayıcı adı. Her zaman `MICROSOFT.DBFORMARIADB` |
| `ResourceType` | `Servers` |
| `ResourceId` | Kaynak URI'si |
| `Resource` | Sunucusunun adı |
| `Category` | `MySqlSlowLogs` |
| `OperationName` | `LogEvent` |
| `Logical_server_name_s` | Sunucusunun adı |
| `start_time_t` [UTC] | Sorgu başladığı saat |
| `query_time_s` | Sorguyu yürütmek için geçen toplam süre |
| `lock_time_s` | Sorgu kilitli olan toplam süre |
| `user_host_s` | Kullanıcı adı |
| `rows_sent_s` | Gönderilen satır sayısı |
| `rows_examined_s` | Denetlenen satır sayısı |
| `last_insert_id_s` | [last_insert_id](https://mariadb.com/kb/en/library/last_insert_id/) |
| `insert_id_s` | Kimliği Ekle |
| `sql_text_s` | Tam sorgu |
| `server_id_s` | Sunucu Kimliği |
| `thread_id_s` | İş Parçacığı Kimliği |
| `\_ResourceId` | Kaynak URI'si |

## <a name="next-steps"></a>Sonraki adımlar
- [Sunucu günlükleri, Azure portalından erişmek ve yapılandırma](howto-configure-server-logs-portal.md).
