---
title: MariaDB için Azure veritabanı için Denetim günlükleri
description: MariaDB ve günlüğe kaydetme düzeylerini etkinleştirmek için kullanılabilir parametreleri için Azure veritabanı'nda kullanılabilir olan denetim günlüklerini açıklar.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: 765db8461465b74ac068782c1b91d3c68b73f7d4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079528"
---
# <a name="audit-logs-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı'nda denetim günlükleri

MariaDB için Azure veritabanı'nda denetim günlüğü kullanıcılar tarafından kullanılabilir. Denetim günlüğü, veritabanı düzeyinde etkinliğini izlemek için kullanılabilir ve uyumluluk için yaygın olarak kullanılır.

> [!IMPORTANT]
> Denetim günlüğü işlevselliği şu anda Önizleme aşamasındadır.

## <a name="configure-audit-logging"></a>Denetim günlüğünü yapılandırma

Denetim günlüğü varsayılan olarak devre dışıdır. Bunu etkinleştirmek için ayarlanmış `audit_log_enabled` açık.

Ayarlayabileceğiniz diğer parametreler şunlardır:

- `audit_log_events`: olayları günlüğe kaydedilmesini denetler. Tablo belirli denetim olaylarını için aşağıya bakın.
- `audit_log_exclude_users`: MariaDB kullanıcılar, günlük hariç tutulacak. En fazla dört kullanıcıya izin verir. Parametresinin en fazla uzunluk 256 karakterdir.

| **Olay** | **Açıklama** |
|---|---|
| `CONNECTION` | -Bağlantı başlatma (başarılı veya başarısız) <br> -Oturumu sırasında farklı bir kullanıcı/parola ile yeniden kimlik kullanıcı <br> -Bağlantı sonlandırma |
| `DML_SELECT`| SELECT sorgusu |
| `DML_NONSELECT` | EKLEME/silme/güncelleme sorguları |
| `DML` | DML DML_SELECT + DML_NONSELECT = |
| `DDL` | "DROP DATABASE" gibi sorguları |
| `DCL` | "İzin ver" gibi sorguları |
| `ADMIN` | "Durumu göster" gibi sorguları |
| `GENERAL` | Tüm DML_SELECT, DML_NONSELECT, DML, DDL, DCL ve yönetim |

## <a name="access-audit-logs"></a>Denetim günlüklerine erişme

Denetim günlükleri, Azure İzleyici tanılama günlükleri ile tümleştirilir. Denetim günlükleri MariaDB sunucunuzda etkinleştirdikten sonra bunları Azure İzleyici günlüklerine, Event Hubs veya Azure depolama gönderebilir. Azure portalında tanılama günlüklerini etkinleştirme hakkında daha fazla bilgi için bkz: [denetim günlüğü portal makale](howto-configure-audit-logs-portal.md#set-up-diagnostic-logs).

## <a name="schemas"></a>Şemalar

Aşağıdaki bölümlerde, çıktı tarafından MariaDB denetim günlüklerini Olay türüne göre nedir açıklanmaktadır. Yer alan alanlar ve göründükleri sırayla çıkış yöntemine bağlı olarak değişebilir.

### <a name="connection"></a>Bağlantı

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
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `event_class` | `connection_log` |
| `event_subclass` | `CONNECT`, `DISCONNECT` |
| `connection_id` | MariaDB tarafından oluşturulan benzersiz bağlantı kimliği |
| `host` | Boş |
| `ip` | MariaDB için bağlanan istemci IP adresi |
| `user` | Sorguyu yürüten kullanıcının adı |
| `db` | Bağlı veritabanının adı |
| `\_ResourceId` | Kaynak URI'si |

### <a name="general"></a>Genel

Aşağıdaki şema genel, DML_SELECT, DML_NONSELECT, DML, DDL, DCL ve yönetici olay türleri için geçerlidir.

| **Özellik** | **Açıklama** |
|---|---|
| `TenantId` | Kiracı Kimliğiniz |
| `SourceSystem` | `Azure` |
| `TimeGenerated` [UTC] | Tshe günlük UTC olarak kaydedildiği zaman damgası |
| `Type` | Günlük türü. Her zaman `AzureDiagnostics` |
| `SubscriptionId` | Sunucunun ait olduğu aboneliğin GUID |
| `ResourceGroup` | Sunucunun ait olduğu kaynak grubu adı |
| `ResourceProvider` | Kaynak sağlayıcı adı. Her zaman `MICROSOFT.DBFORMARIADB` |
| `ResourceType` | `Servers` |
| `ResourceId` | Kaynak URI'si |
| `Resource` | Sunucusunun adı |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `event_class` | `general_log` |
| `event_subclass` | `LOG`, `ERROR`, `RESULT` |
| `event_time` | Sorgu başlangıç saniye içinde UNIX zaman damgası |
| `error_code` | Sorgu başarısız olursa hata kodu. `0` hiçbir hata anlamına gelir |
| `thread_id` | Sorgu yürütülen iş parçacığının kimliği |
| `host` | Boş |
| `ip` | MariaDB için bağlanan istemci IP adresi |
| `user` | Sorguyu yürüten kullanıcının adı |
| `sql_text` | Tam sorgu metni |
| `\_ResourceId` | Kaynak URI'si |

### <a name="table-access"></a>Tablo erişim

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
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `event_class` | `table_access_log` |
| `event_subclass` | `READ`, `INSERT`, `UPDATE`, veya `DELETE` |
| `connection_id` | MariaDB tarafından oluşturulan benzersiz bağlantı kimliği |
| `db` | Erişilen veritabanının adı |
| `table` | Erişilen tablosunun adı |
| `sql_text` | Tam sorgu metni |
| `\_ResourceId` | Kaynak URI'si |

## <a name="next-steps"></a>Sonraki adımlar

- [Denetim günlüklerinin Azure portalında yapılandırma](howto-configure-audit-logs-portal.md)