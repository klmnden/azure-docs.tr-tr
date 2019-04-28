---
title: SQL veritabanı denetim günlük biçimi | Microsoft Docs
description: SQL veritabanı denetim günlüklerini nasıl yapılandırılmıştır anlayın.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: vainolo
ms.author: arib
ms.reviewer: vanto
manager: craigg
ms.date: 01/03/2019
ms.openlocfilehash: 0fefe01e413e30e4aa3c1fa90de77cbdece39c38
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61417397"
---
# <a name="sql-database-audit-log-format"></a>SQL veritabanı denetim günlük biçimi

[Azure SQL veritabanı denetimi](sql-database-auditing.md) veritabanı olaylarını ve bir denetim günlüğüne Azure depolama hesabınızdaki Yazar izler ve bunları olay hub'ı veya Log Analytics aşağı akış işleme ve analiz için gönderir.

## <a name="naming-conventions"></a>Adlandırma kuralları

### <a name="blob-audit"></a>BLOB denetimi

BLOB depolamada depolanan denetim günlüklerini adlı bir kapsayıcıda depolanan `sqldbauditlogs` Azure depolama hesabındaki. Kapsayıcı içindeki dizin hiyerarşisinin biçimindedir `<ServerName>/<DatabaseName>/<AuditName>/<Date>/`. Blob dosya adı biçimi `<CreationTime>_<FileNumberInSession>.xel`burada `CreationTime` UTC biçiminde olan `hh_mm_ss_ms` biçimi ve `FileNumberInSession` çalışan bir dizine oturumu arasında birden çok Blob dosyalarını yayılma günlükleri durumunda olduğundan.

Örneğin, veritabanı için `Database1` üzerinde `Server1` olası geçerli bir yol aşağıda verilmiştir:

    Server1/Database1/SqlDbAuditing_ServerAudit_NoRetention/2019-02-03/12_23_30_794_0.xel

### <a name="event-hub"></a>Olay Hub'ı

Denetim olayları denetim yapılandırması sırasında tanımlandı ve gövdesinde yakalanan ad alanı ve olay hub'ına yazılır [Apache Avro](https://avro.apache.org/) olayları ve UTF-8 kodlaması ile JSON biçimlendirme kullanılarak depolanabilir. Denetim günlüklerini okumak için kullanabileceğiniz [Avro Araçları](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview#use-avro-tools) veya bu biçim işleme benzer araçları.

### <a name="log-analytics"></a>Log Analytics

Denetim olayları denetim yapılandırması sırasında çok tanımlı Log Analytics çalışma alanına yazılır `AzureDiagnostics` kategorisiyle tablo `SQLSecurityAuditEvents`. Log Analytics arama dili ve komutlar hakkında başka yararlı bilgiler için bkz. [Log Analytics Arama başvurusu](https://docs.microsoft.com/azure/log-analytics/log-analytics-log-search).

## <a id="subheading-1"></a>Denetim günlüğü alanları

| Ad (Blob) | Ad (olay hub'ları / Log Analytics'e) | Açıklama | BLOB türü | Olay hub'ları / Log Analytics türü |
|-------------|---------------------------------|-------------|-----------|-------------------------------|
| action_id | action_id_s | Eylem Kimliği | VARCHAR(4) | string |
| action_name | action_name_s | Eylemin adı | Yok | string |
| additional_information | additional_information_s | XML olarak depolanan bu olay hakkında ek bilgiler | nvarchar(4000) | string |
| affected_rows | affected_rows_d | Sorgu tarafından etkilenen satır sayısı | bigint | int |
| application_name | application_name_s| İstemci uygulamasının adı | nvarchar(128) | string |
| audit_schema_version | audit_schema_version_d | Her zaman 1 | int | int |
| class_type | class_type_s | Denetim oluşma zamanı denetlenebilir varlık türü | VARCHAR(2) | string |
| class_type_desc | class_type_description_s | Denetim oluşma zamanı denetlenebilir varlık açıklaması | Yok | string |
| client_ip | client_ip_s | Kaynak IP istemci uygulaması | nvarchar(128) | string |
| connection_id | Yok | Sunucu bağlantı kimliği | GUID | Yok |
| data_sensitivity_information | data_sensitivity_information_s | Bilgi türleri ve veritabanındaki sınıflandırılmış sütunlar göre denetlenmiş sorgu tarafından döndürülen duyarlılık etiketler. Daha fazla bilgi edinin [Azure SQL veritabanı'nın verileri bulma ve sınıflandırma](sql-database-data-discovery-and-classification.md) | nvarchar(4000) | string |
| database_name | database_name_s | Eylemin gerçekleştiği veritabanı bağlamı | Sistem adı | string |
| database_prıncıpal_ıd | database_principal_id_d | Gerçekleştirilecek bir veritabanı kullanıcı bağlamı kimliği | int | int |
| database_principal_name | database_principal_name_s | Gerçekleştirilen eylem içinde veritabanı kullanıcı bağlamı adı | Sistem adı | string |
| duration_milliseconds | duration_milliseconds_d | Milisaniye cinsinden sorgu yürütme süresi | bigint | int |
| event_time | event_time_t | Tarih ve saat denetlenebilir eylemi olduğunda tetiklenir | datetime2 | datetime |
| host_name | Yok | İstemci ana bilgisayar adı | string | Yok |
| is_column_permission | is_column_permission_s | Bu sütun düzeyi izni olup olmadığını belirten bayrak. 1 = true, 0 = false | bit | string |
| Yok | is_server_level_audit_s | Bu denetim, sunucu düzeyinde olup olmadığını belirten bayrak | Yok | string |
| object_ kimliği | object_id_d | Denetim üzerinde oluşan varlık kimliği. Bu içerir: sunucu nesneleri, veritabanlarını, veritabanı nesneleri ve şema nesneleri. Nesne düzeyinde varlık sunucusu ise ya da Denetim değilse 0 gerçekleştirilen | int | int |
| object_name | object_name_s | Denetim üzerinde oluşan varlığın adı. Bu içerir: sunucu nesneleri, veritabanlarını, veritabanı nesneleri ve şema nesneleri. Nesne düzeyinde varlık sunucusu ise ya da Denetim değilse 0 gerçekleştirilen | Sistem adı | string |
| permission_bitmask | permission_bitmask_s | Uygun olduğunda, verilen, engellenen, veya iptal edilen izinleri gösterilir. | varbinary(16) | string |
| response_rows | response_rows_d | Sonuç kümesinde döndürülen satır sayısını | bigint | int |
| schema_name | schema_name_s | Şema eylemin gerçekleştiği bağlamı. Bir şema dışında gerçekleşen denetimleri için NULL | Sistem adı | string |
| Yok | securable_class_type_s | Denetlenmekte class_type eşleyen güvenli kılınabilir nesne | Yok | string |
| sequence_group_id | sequence_group_id_g | Benzersiz tanımlayıcı | Varbinary | GUID |
| sequence_number | sequence_number_d | Denetimleri için yazma arabelleği sığmayacak kadar büyük bir tek bir denetim kaydı içinde kayıt dizisini izler | int | int |
| server_instance_name | server_instance_name_s | Denetim gerçekleştiği sunucu örneğinin adı | Sistem adı | string |
| server_principal_id | server_principal_id_d | Gerçekleştirilen eylem içinde oturum açma bağlamını kimliği | int | int |
| server_principal_name | server_principal_name_s | Geçerli oturum açma | Sistem adı | string |
| server_principal_sid | server_principal_sid_s | Geçerli oturum açma SID | Varbinary | string |
| session_ıd | session_id_d | Olayın gerçekleştiği oturum kimliği | smallint | int |
| session_server_principal_name | session_server_principal_name_s | Asıl sunucu oturum için | Sistem adı | string |
| Deyimi | statement_s | (Varsa) yürütülen bir T-SQL deyimi | nvarchar(4000) | string |
| başarılı | succeeded_s | Olayı tetikleyen eylemi başarılı olup olmadığını gösterir. Oturum açma ve batch dışındaki olaylar için bu yalnızca izni denetimi başarılı veya başarısız, işlemi olduğunu bildirir. 1 = başarılı, 0 = başarısız | bit | string |
| target_database_principal_id | target_database_principal_id_d | Asıl veritabanı üzerinde GRANT/DENY/REVOKE işlemi gerçekleştirilir. uygulanabilir değilse 0 | int | int |
| target_database_principal_name | target_database_principal_name_s | Hedef kullanıcı eyleminin. Uygulanabilir değilse NULL | string | string |
| target_server_principal_id | target_server_principal_id_d | GRANT/DENY/REVOKE işlemi gerçekleştirilir asıl sunucu. Uygulanabilir değilse 0 değerini döndürür | int | int |
| target_server_principal_name | target_server_principal_name_s | Eylem hedef oturum açın. Uygulanabilir değilse NULL | Sistem adı | string |
| target_server_principal_sid | target_server_principal_sid_s | Hedef oturum açma SID'si. Uygulanabilir değilse NULL | Varbinary | string |
| transaction_id | transaction_id_d | (2016'dan itibaren) - yalnızca SQL Server için Azure SQL DB 0 | bigint | int |
| user_defined_event_id | user_defined_event_id_d | Olay Kimliği sp_audit_write için bağımsız değişken olarak geçirilen kullanıcı tanımlı. Sistem olayları (varsayılan) boş ve sıfır için kullanıcı tanımlı olay. Daha fazla bilgi için [sp_audit_write (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-audit-write-transact-sql) | smallint | int |
| user_defined_information | user_defined_information_s | Sp_audit_write için bağımsız değişken olarak geçirilen bilgileri kullanıcı tanımlı. Sistem olayları (varsayılan) boş ve sıfır için kullanıcı tanımlı olay. Daha fazla bilgi için [sp_audit_write (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-audit-write-transact-sql) | nvarchar(4000) | string |

## <a name="next-steps"></a>Sonraki Adımlar

Daha fazla bilgi edinin [Azure SQL veritabanı denetim](sql-database-auditing.md).