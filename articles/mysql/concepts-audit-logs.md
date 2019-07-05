---
title: MySQL için Azure veritabanı için Denetim günlükleri
description: Günlüğe kaydetme düzeylerini etkinleştirmek için kullanılabilir parametrelerde ve MySQL için Azure veritabanı'nda kullanılabilir olan denetim günlüklerini açıklar.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 06/26/2019
ms.openlocfilehash: 86750cea5e7f0d4726f3e0e9a03795ef2a602d8b
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443854"
---
# <a name="audit-logs-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı'nda denetim günlükleri

MySQL için Azure veritabanı'nda denetim günlüğü kullanıcılar tarafından kullanılabilir. Denetim günlüğü, veritabanı düzeyinde etkinliğini izlemek için kullanılabilir ve uyumluluk için yaygın olarak kullanılır.

> [!IMPORTANT]
> Denetim günlüğü işlevselliği şu anda Önizleme aşamasındadır.

## <a name="configure-audit-logging"></a>Denetim günlüğünü yapılandırma

Denetim günlüğü varsayılan olarak devre dışıdır. Bunu etkinleştirmek için ayarlanmış `audit_log_enabled` açık.

Ayarlayabileceğiniz diğer parametreler şunlardır:

- `audit_log_events`: olayları günlüğe kaydedilmesini denetler. Tablo belirli denetim olaylarını için aşağıya bakın.
- `audit_log_exclude_users`: MySQL kullanıcıları, günlük hariç tutulacak. En fazla dört kullanıcıya izin verir. Parametresinin en fazla uzunluk 256 karakterdir.

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
| `TABLE_ACCESS` | -Yalnızca MySQL 5.7 için kullanılabilir <br> -Tablo seçin veya INSERT INTO gibi ifadeleri okuyun... SELECT <br> -SİLME veya TRUNCATE TABLE gibi table delete deyimleri <br> -EKLEME veya değiştirme gibi table INSERT deyimleri <br> -GÜNCELLEŞTİRME gibi table update ifadeleriyle |

## <a name="access-audit-logs"></a>Denetim günlüklerine erişme

Denetim günlükleri, Azure İzleyici tanılama günlükleri ile tümleştirilir. Denetim günlükleri, MySQL sunucunuzda etkinleştirdikten sonra bunları Azure İzleyici günlüklerine, Event Hubs veya Azure depolama gönderebilir. Azure portalında tanılama günlüklerini etkinleştirme hakkında daha fazla bilgi için bkz: [denetim günlüğü portal makale](howto-configure-audit-logs-portal.md#set-up-diagnostic-logs).

## <a name="diagnostic-logs-schemas"></a>Tanılama günlükleri şemaları

Aşağıdaki bölümlerde, çıktı tarafından MySQL denetim günlüklerini Olay türüne göre nedir açıklanmaktadır. Yer alan alanlar ve göründükleri sırayla çıkış yöntemine bağlı olarak değişebilir.

### <a name="connection"></a>Bağlantı

| **Özellik** | **Açıklama** |
|---|---|
| `TenantId` | Kiracı Kimliğiniz |
| `SourceSystem` | `Azure` |
| `TimeGenerated [UTC]` | Günlük UTC olarak kaydedildiği zaman damgası |
| `Type` | Günlük türü. Her zaman `AzureDiagnostics` |
| `SubscriptionId` | Sunucunun ait olduğu aboneliğin GUID |
| `ResourceGroup` | Sunucunun ait olduğu kaynak grubu adı |
| `ResourceProvider` | Kaynak sağlayıcı adı. Her zaman `MICROSOFT.DBFORMYSQL` |
| `ResourceType` | `Servers` |
| `ResourceId` | Kaynak URI'si |
| `Resource` | Sunucusunun adı |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `LogicalServerName_s` | Sunucusunun adı |
| `event_class_s` | `connection_log` |
| `event_subclass_s` | `CONNECT`, `DISCONNECT`, `CHANGE USER` (yalnızca MySQL 5.7 için kullanılabilir) |
| `connection_id_d` | MySQL tarafından oluşturulan benzersiz bağlantı kimliği |
| `host_s` | Boş |
| `ip_s` | Mysql'e bağlanma istemcinin IP adresi |
| `user_s` | Sorguyu yürüten kullanıcının adı |
| `db_s` | Bağlı veritabanının adı |
| `\_ResourceId` | Kaynak URI'si |

### <a name="general"></a>Genel

Aşağıdaki şema genel, DML_SELECT, DML_NONSELECT, DML, DDL, DCL ve yönetici olay türleri için geçerlidir.

| **Özellik** | **Açıklama** |
|---|---|
| `TenantId` | Kiracı Kimliğiniz |
| `SourceSystem` | `Azure` |
| `TimeGenerated [UTC]` | Günlük UTC olarak kaydedildiği zaman damgası |
| `Type` | Günlük türü. Her zaman `AzureDiagnostics` |
| `SubscriptionId` | Sunucunun ait olduğu aboneliğin GUID |
| `ResourceGroup` | Sunucunun ait olduğu kaynak grubu adı |
| `ResourceProvider` | Kaynak sağlayıcı adı. Her zaman `MICROSOFT.DBFORMYSQL` |
| `ResourceType` | `Servers` |
| `ResourceId` | Kaynak URI'si |
| `Resource` | Sunucusunun adı |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `LogicalServerName_s` | Sunucusunun adı |
| `event_class_s` | `general_log` |
| `event_subclass_s` | `LOG`, `ERROR`, `RESULT` (yalnızca MySQL 5.6 için kullanılabilir) |
| `event_time` | Sorgu başlangıç saniye içinde UNIX zaman damgası |
| `error_code_d` | Sorgu başarısız olursa hata kodu. `0` hiçbir hata anlamına gelir |
| `thread_id_d` | Sorgu yürütülen iş parçacığının kimliği |
| `host_s` | Boş |
| `ip_s` | Mysql'e bağlanma istemcinin IP adresi |
| `user_s` | Sorguyu yürüten kullanıcının adı |
| `sql_text_s` | Tam sorgu metni |
| `\_ResourceId` | Kaynak URI'si |

### <a name="table-access"></a>Tablo erişim

| **Özellik** | **Açıklama** |
|---|---|
| `TenantId` | Kiracı Kimliğiniz |
| `SourceSystem` | `Azure` |
| `TimeGenerated [UTC]` | Günlük UTC olarak kaydedildiği zaman damgası |
| `Type` | Günlük türü. Her zaman `AzureDiagnostics` |
| `SubscriptionId` | Sunucunun ait olduğu aboneliğin GUID |
| `ResourceGroup` | Sunucunun ait olduğu kaynak grubu adı |
| `ResourceProvider` | Kaynak sağlayıcı adı. Her zaman `MICROSOFT.DBFORMYSQL` |
| `ResourceType` | `Servers` |
| `ResourceId` | Kaynak URI'si |
| `Resource` | Sunucusunun adı |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `LogicalServerName_s` | Sunucusunun adı |
| `event_class_s` | `table_access_log` |
| `event_subclass_s` | `READ`, `INSERT`, `UPDATE`, veya `DELETE` |
| `connection_id_d` | MySQL tarafından oluşturulan benzersiz bağlantı kimliği |
| `db_s` | Erişilen veritabanının adı |
| `table_s` | Erişilen tablosunun adı |
| `sql_text_s` | Tam sorgu metni |
| `\_ResourceId` | Kaynak URI'si |

## <a name="next-steps"></a>Sonraki adımlar

- [Denetim günlüklerinin Azure portalında yapılandırma](howto-configure-audit-logs-portal.md)