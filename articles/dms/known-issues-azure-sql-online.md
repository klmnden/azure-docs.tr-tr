---
title: Bilinen sorunları/geçiş sınırlamaları online geçişleri için Azure SQL veritabanı ile ilgili bir makale | Microsoft Docs
description: Bilinen sorunları/geçiş sınırlamalarıyla birlikte Azure SQL veritabanı çevrimiçi geçiş hakkında bilgi edinin.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 04/09/2019
ms.openlocfilehash: a822e540db87c36358f1a0e34d75e05ed866868d
ms.sourcegitcommit: 6e32f493eb32f93f71d425497752e84763070fad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2019
ms.locfileid: "59471248"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-to-azure-sql-db"></a>Bilinen sorunları/geçiş sınırlamalarıyla birlikte Azure SQL DB'ye online geçişleri

Bilinen sorunlar ve çevrimiçi SQL Server'dan Azure SQL veritabanı ile ilgili sınırlamalar aşağıda açıklanmıştır.

> [!IMPORTANT]
> Azure SQL veritabanı için SQL Server'ın çevrimiçi migrations ile geçiş SQL_variant veri türleri desteklenmiyor.

### <a name="migration-of-temporal-tables-not-supported"></a>Zamana bağlı tablolarda desteklenmeyen bir geçişini

**Belirti**

Kaynak veritabanınızın bir veya daha fazla zamana bağlı tablolarda oluşuyorsa, veritabanı geçişinizi "tam veri yüklemesi" işlemi sırasında başarısız oluyor ve aşağıdaki iletiyi görebilirsiniz:

{"ResourceId": "/subscriptions/<subscription id>/resourceGroups/migrateready/providers/Microsoft.DataMigration/services/<DMS Service name>", "errorType": "Veritabanı geçiş hatası", "errorEvents": "[" yakalama işlevleri ayarlanamadı. RetCode: SQL_ERROR hatası SqlState: 42000 NativeError: 13570 ileti: sistem sürümü tutulan zamana bağlı tabloyla [Microsoft] [SQL Server Native Client 11.0] [SQL Server] çoğaltma kullanımı desteklenmiyor ' [uygulaması. Şehir]' satır: 1 sütun: -1 "]"}
 
 ![Zamana bağlı tablo hataları örnek](media/known-issues-azure-sql-online/dms-temporal-tables-errors.png)

**Geçici çözüm**

1. Zamana bağlı tablolarda aşağıdaki sorguyu kullanarak, kaynak şemasında bulun.
     ``` 
     select name,temporal_type,temporal_type_desc,* from sys.tables where temporal_type <>0
     ```
2. Bu tablodan hariç **geçiş ayarlarını yapılandırma** tablolar geçiş için belirttiğiniz dikey penceresinde.

3. Geçiş etkinlik yeniden çalıştırın.

**Kaynaklar**

Daha fazla bilgi için bkz [zamana bağlı tablolarda](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables?view=sql-server-2017).
 
### <a name="migration-of-tables-includes-one-or-more-columns-with-the-hierarchyid-data-type"></a>Tablo geçiş HierarchyId veri türü olan bir veya daha fazla sütun içerir.

**Belirti**

"N metin"tam veri yüklemesi"işlemi sırasında hierarchyid ile uyumlu değil" önerme SQL özel durumu görebilirsiniz:
     
![HierarchyId hataları örnek](media/known-issues-azure-sql-online/dms-hierarchyid-errors.png)

**Geçici çözüm**

1. HierarchyId veri türü aşağıdaki sorguyu kullanarak sütunları içeren kullanıcı tabloları bulun.

      ``` 
      select object_name(object_id) 'Table name' from sys.columns where system_type_id =240 and object_id in (select object_id from sys.objects where type='U')
      ``` 

2. Bu tablodan hariç **geçiş ayarlarını yapılandırma** tablolar geçiş için belirttiğiniz dikey penceresinde.

3. Geçiş etkinlik yeniden çalıştırın.

### <a name="migration-failures-with-various-integrity-violations-with-active-triggers-in-the-schema-during-full-data-load-or-incremental-data-sync"></a>"Tam veri yüklemesi" veya "artımlı veri eşitleme" sırasında şema etkin tetikleyicilerle çeşitli bütünlüğü ihlali Geçiş hataları

**Geçici çözüm**

1. Aşağıdaki sorguyu kullanarak kaynak veritabanında şu anda etkin olan Tetikleyiciler bulun:

     ```
     select * from sys.triggers where is_disabled =0
     ```

2. Tetikleyiciler makalede sağlanan adımları kullanarak, kaynak veritabanı üzerindeki devre dışı [TETİKLEYİCİYİ devre dışı bırak (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/disable-trigger-transact-sql?view=sql-server-2017).

3. Geçiş etkinlik yeniden çalıştırın.

### <a name="support-for-lob-data-types"></a>LOB veri türleri için destek

**Belirti**

Büyük nesne (LOB) sütun uzunluğu 32 KB'den daha büyük ise, veri hedefte kesilmiş. Aşağıdaki sorguyu kullanarak LOB sütunu uzunluğunu kontrol edebilirsiniz: 

``` 
SELECT max(DATALENGTH(ColumnName)) as LEN from TableName
```

**Geçici çözüm**

32 KB'den daha büyük bir LOB sütunu varsa, mühendislik ekibiyle iletişime geçin. [isteyin Azure veritabanı geçişlerini](mailto:AskAzureDatabaseMigrations@service.microsoft.com).

### <a name="issues-with-timestamp-columns"></a>Zaman damgası sütunları ile ilgili sorunlar

**Belirti**

DMS, kaynak zaman damgası değeri geçirmediğini; Bunun yerine, DMS, hedef tabloda yeni bir zaman damgası değeri oluşturur.

**Geçici çözüm**

DMS kaynak tablosunda depolanan tam zaman damgası değeri geçirmek için gerekiyorsa, mühendislik ekibi ile iletişime geçin [isteyin Azure veritabanı geçişlerini](mailto:AskAzureDatabaseMigrations@service.microsoft.com).

### <a name="data-migration-errors-dont-provide-additional-details-on-the-database-detailed-status-blade"></a>Veri Geçiş hataları ek ayrıntılar veritabanı ayrıntılı durum dikey sağlaması gerekmez.

**Belirti**

Geçiş hataları veritabanları ayrıntıları durumu görünümünde arasında geldiğinizde, seçerek **veri geçiş hataları** bağlantı üstteki Şeritte ek ayrıntılar için geçiş hataları belirli değil sağlayabilir.

![Veri Geçiş hataları ayrıntıları örnek](media/known-issues-azure-sql-online/dms-data-migration-errors-no-details.png)

**Geçici çözüm**

Belirli hata ayrıntıları almak için aşağıdaki adımları izleyin.

1. Geçiş etkinlik ekranı görüntülemek için veritabanı ayrıntılı durum dikey penceresini kapatın.

     ![geçiş etkinlik ekranı](media/known-issues-azure-sql-online/dms-migration-activity-screen.png)

2. Seçin **hata ayrıntılarına** Geçiş hataları gidermek için yardımcı belirli hata iletilerini görüntülemek için.
