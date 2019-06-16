---
title: MySQL için Azure veritabanı sunucusu günlükleri
description: Farklı günlüğe kaydetme düzeylerini etkinleştirmek için kullanılabilir parametrelerde ve MySQL için Azure veritabanı'nda kullanılabilir yavaş sorgu günlüklerini açıklar.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 05/29/2019
ms.openlocfilehash: 1a8956d40ef30e8d52fbdded3448019e14ab16a5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67062409"
---
# <a name="slow-query-logs-in-azure-database-for-mysql"></a>Yavaş sorgu, MySQL için Azure veritabanı'nda günlüğe kaydeder
MySQL için Azure veritabanı'nda yavaş sorgu günlüğü kullanıcılar tarafından kullanılabilir. İşlem günlüğü erişimi desteklenmiyor. Yavaş sorgu günlüğü, sorun giderme için performans sorunlarını tanımlamak için kullanılabilir.

MySQL yavaş sorgu günlüğü hakkında daha fazla bilgi için bkz: MySQL Başvuru Kılavuzu'nın [yavaş sorgu günlüğü bölümü](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html).

## <a name="access-slow-query-logs"></a>Yavaş sorgu günlüklerine erişme
Liste ve Azure portalı ve Azure CLI kullanarak MySQL yavaş sorgu günlüklerini için Azure veritabanı indirin.

Azure portalında MySQL için Azure veritabanı sunucunuza seçin. Altında **izleme** başlığı seçin **sunucu günlükleri** sayfası.

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI kullanarak sunucu günlükleri ve erişimi Yapılandır](howto-configure-server-logs-in-cli.md).

## <a name="log-retention"></a>Günlük tutma
Günlükleri, bunların oluşturma yedi güne kadar kullanılabilir. Kullanılabilir günlükleri toplam boyutu 7 GB aşarsa alanı olana kadar eski dosyalar silinir. 

Günlükleri Döndürülmüş her 24 saat veya 7 GB, hangisi gelir önce.

## <a name="configure-slow-query-logging"></a>Yavaş sorgu günlük tutmayı yapılandırma 
Yavaş sorgu günlüğü varsayılan olarak devre dışıdır. Bunu etkinleştirmek için slow_query_log açık olarak ayarlayın.

Ayarlayabileceğiniz diğer parametreler şunlardır:

- **long_query_time**: sorgu (saniye cinsinden) long_query_time uzun sürerse, sorgu günlüğe kaydedilir. Varsayılan değer 10 saniyedir.
- **log_slow_admin_statements**: ON slow_query_log için yazılan deyimlerinde ALTER_TABLE ve ANALYZE_TABLE gibi yönetim deyimleri içeriyorsa.
- **log_queries_not_using_indexes**: dizinleri kullanmayan sorgular için slow_query_log kaydedilip kaydedilmeyeceğini belirler
- **log_throttle_queries_not_using_indexes**: Bu parametre için yavaş sorgu günlüğü yazılabilir olmayan dizin sorguların sayısını sınırlar. Log_queries_not_using_indexes ON olarak ayarlandığında bu parametre etkinleşir.

MySQL bkz [yavaş sorgu günlüğü belgeleri](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html) yavaş sorgu günlüğü parametrelerini tam açıklamaları için.

## <a name="diagnostic-logs"></a>Tanılama günlükleri
MySQL için Azure veritabanı Azure İzleyici tanılama günlükleri ile tümleştirilir. Yavaş sorgu günlüklerini MySQL sunucunuzda etkinleştirdikten sonra bunları Azure İzleyici günlüklerine, Event Hubs veya Azure depolama yayılan sahip olmayı seçebilirsiniz. Tanılama günlüklerini etkinleştirme hakkında daha fazla bilgi edinmek için nasıl bölümüne bakın. [tanılama günlükleri belgeleri](../azure-monitor/platform/diagnostic-logs-overview.md).

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
| `ResourceProvider` | Kaynak sağlayıcı adı. Her zaman `MICROSOFT.DBFORMYSQL` |
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
| `last_insert_id_s` | [last_insert_id](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_last-insert-id) |
| `insert_id_s` | Kimliği Ekle |
| `sql_text_s` | Tam sorgu |
| `server_id_s` | Sunucunun kimliği |
| `thread_id_s` | İş parçacığı kimliği |
| `\_ResourceId` | Kaynak URI'si |

## <a name="next-steps"></a>Sonraki Adımlar
- [Yapılandırma ve Azure CLI'dan sunucu günlüklerine erişme](howto-configure-server-logs-in-cli.md).
