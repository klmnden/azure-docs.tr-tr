---
title: -Tek bir sunucu PostgreSQL için Azure veritabanı'nda sunucu günlüklerini
description: Bu makalede PostgreSQL için Azure veritabanı - sorgu ve Hata günlüklerini ve günlük tutma nasıl yapılandırıldığını tek bir sunucu oluşturur.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 4d1cf2c59e324cedd9b747b1ac65d6edcb9deb45
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65067402"
---
# <a name="server-logs-in-azure-database-for-postgresql---single-server"></a>-Tek bir sunucu PostgreSQL için Azure veritabanı'nda sunucu günlüklerini
PostgreSQL için Azure veritabanı oluşturur, sorgu ve hata günlükleri. Sorgu ve Hata günlüklerini belirlemek, sorun giderme ve yapılandırma hatalarını ve performansın onarmak için kullanılabilir. (İşlem günlükleri için erişim dahil değildir). 

## <a name="configure-logging"></a>Günlük tutmayı yapılandırma 
Günlüğe kaydetme sunucu parametreleri kullanarak sunucunuza günlüğü yapılandırabilirsiniz. Her yeni sunucuda **log_checkpoints** ve **log_connections** varsayılan olarak etkindir. Günlük kaydını ihtiyaçlarınıza uyacak şekilde ayarlayabileceğiniz ek parametreler şunlardır: 

![PostgreSQL - günlük parametreleri için Azure veritabanı](./media/concepts-server-logs/log-parameters.png)

Bu parametreler hakkında daha fazla bilgi için bkz: PostgreSQL'ın [hata bildirimi ve günlüğe kaydetme](https://www.postgresql.org/docs/current/static/runtime-config-logging.html) belgeleri. PostgreSQL parametreleri için Azure veritabanı yapılandırma konusunda bilgi için bkz: [portal belgeleri](howto-configure-server-parameters-using-portal.md) veya [CLI belgeleri](howto-configure-server-parameters-using-cli.md).

## <a name="access-server-logs-through-portal-or-cli"></a>Portal veya CLI aracılığıyla sunucu günlüklerine erişme
Günlükleri etkinleştirdiyseniz, bunları günlük depolama kullanılarak PostgreSQL için Azure veritabanı'ndan erişebilirsiniz [Azure portalında](howto-configure-server-logs-in-portal.md), [Azure CLI](howto-configure-server-logs-using-cli.md)ve Azure REST API'leri. Her 1 saat ya da 100 MB boyutunda günlük dosyalarını döndürmek, hangisinin önce geldiğine. Bu günlük depolama kullanmak için saklama süresini ayarlayabilirsiniz **günlük\_bekletme\_süresi** sunucunuzla ilişkili parametre. Varsayılan değer 3 gündür; en yüksek değer 7 gündür. Sunucunuz, yeterli olmalıdır depolama günlük dosyalarını tutmak için ayrılmış. (Bu bekletme parametre Azure tanılama günlükleri yönetmez.)


## <a name="diagnostic-logs"></a>Tanılama günlükleri
PostgreSQL için Azure veritabanı Azure İzleyici tanılama günlükleri ile tümleştirilir. PostgreSQL sunucunuzda günlükleri etkinleştirdikten sonra bunları için yayılan sahip olmayı seçebilirsiniz [Azure İzleyici günlükleri](../azure-monitor/log-query/log-query-overview.md), Event Hubs veya Azure depolama. Tanılama günlüklerini etkinleştirme hakkında daha fazla bilgi için nasıl yapılır bölümüne bakın [tanılama günlükleri belgeleri](../azure-monitor/platform/diagnostic-logs-overview.md). 

> [!IMPORTANT]
> Sunucu günlükleri için tanılama bu özellik yalnızca genel amaçlı ve bellek için iyileştirilmiş kullanılabilir [fiyatlandırma katmanları](concepts-pricing-tiers.md).

Aşağıdaki tabloda, her oturum açma yenilikler açıklanır. Seçtiğiniz çıkış uç noktası, yer alan alanlar ve değişebilir göründükleri sırayla bağlı olarak. 

|**Alan** | **Açıklama** |
|---|---|
| TenantId | Kiracı Kimliğiniz |
| SourceSystem | `Azure` |
| TimeGenerated [UTC] | Günlük UTC olarak kaydedildiği zaman damgası |
| Tür | Günlük türü. Her zaman `AzureDiagnostics` |
| SubscriptionId | Sunucunun ait olduğu aboneliğin GUID |
| ResourceGroup | Sunucunun ait olduğu kaynak grubu adı |
| ResourceProvider | Kaynak sağlayıcı adı. Her zaman `MICROSOFT.DBFORPOSTGRESQL` |
| ResourceType | `Servers` |
| ResourceId | Kaynak URI'si |
| Resource | Sunucusunun adı |
| Category | `PostgreSQLLogs` |
| OperationName | `LogEvent` |
| errorLevel | Günlüğe kaydetme düzeyi, örneğin: GÜNLÜK, HATA BİLDİRİMİ |
| `Message` | Birincil günlük iletisi | 
| Domain | Sunucu sürümü, örnek: postgres 10 |
| Ayrıntı | İkincil günlük iletisi (varsa) |
| ColumnName | (Eğer varsa) sütunun adı |
| SchemaName | (Eğer varsa) şema adı |
| DatatypeName | (Eğer varsa) veri türü adı |
| LogicalServerName | Sunucusunun adı | 
| _ResourceId | Kaynak URI'si |

## <a name="next-steps"></a>Sonraki adımlar
- Günlüklerinden erişme hakkında daha fazla bilgi [Azure portalında](howto-configure-server-logs-in-portal.md) veya [Azure CLI](howto-configure-server-logs-using-cli.md).
- Daha fazla bilgi edinin [Azure İzleyici fiyatlandırma](https://azure.microsoft.com/pricing/details/monitor/).
