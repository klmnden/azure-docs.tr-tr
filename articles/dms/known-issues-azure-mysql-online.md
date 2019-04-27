---
title: MySQL için Azure veritabanı çevrimiçi geçişleri ile bilinen sorunları/geçiş sınırlamalarıyla ilgili bir makale | Microsoft Docs
description: MySQL için Azure veritabanı çevrimiçi geçişleri ile bilinen sorunları/geçiş sınırlamaları hakkında bilgi edinin.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 03/12/2019
ms.openlocfilehash: 0641545c10d7f59cb1874659eae9c7e7bf65932e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60532258"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-to-azure-db-for-mysql"></a>MySQL için Azure DB online geçişleri ile bilinen sorunları/geçiş sınırlamaları

Bilinen sorunlar ve sınırlamalar online geçişleri MySQL için Azure veritabanı için MySQL ile ilişkili aşağıdaki bölümlerde açıklanmıştır. 

## <a name="online-migration-configuration"></a>Çevrimiçi geçişi yapılandırma
- Kaynak MySQL Server sürümü sürümü 5.6.35, 5.7.18 olmalıdır veya üzeri
- MySQL için Azure veritabanı'nı destekler:
    - MySQL community edition
    - Innodb altyapısı
- Aynı sürüm geçişi. MySQL 5.7 için Azure veritabanı geçişi MySQL 5.6 desteklenmez.
- My.ini (Windows) veya my.cnf (Unix) ikili günlüğü etkinleştirme
    - 1, örneğin, Server_id Server_id istediğiniz kadar büyük veya eşittir ayarlayın (yalnızca için MySQL 5.6) = 1
    - Ayarlanmış log-bin = \<yolu > (yalnızca için MySQL 5.6)
    - Ayarlama binlog_format = satır
    - Expire_logs_days = 5 (önerilen - yalnızca MySQL 5.6 için)
- Kullanıcı ReplicationAdmin rolüne sahip olmalıdır.
- Kaynak MySQL veritabanı için tanımlanan harmanlamaları hedef Azure veritabanını MySQL için tanımlanan ayarlara ile aynıdır.
- MySQL için Azure veritabanı'nda hedef veritabanını kaynak MySQL veritabanı arasındaki şema eşleşmelidir.
- Hedef MySQL için Azure veritabanı şemasında yabancı anahtarlar olmaması gerekir. Yabancı anahtarlar bırakmak için aşağıdaki sorguyu kullanın:
    ```
    SET group_concat_max_len = 8192;
    SELECT SchemaName, GROUP_CONCAT(DropQuery SEPARATOR ';\n') as DropQuery, GROUP_CONCAT(AddQuery SEPARATOR ';\n') as AddQuery
    FROM
    (SELECT 
    KCU.REFERENCED_TABLE_SCHEMA as SchemaName, KCU.TABLE_NAME, KCU.COLUMN_NAME,
        CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' DROP FOREIGN KEY ', KCU.CONSTRAINT_NAME) AS DropQuery,
        CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' ADD CONSTRAINT ', KCU.CONSTRAINT_NAME, ' FOREIGN KEY (`', KCU.COLUMN_NAME, '`) REFERENCES `', KCU.REFERENCED_TABLE_NAME, '` (`', KCU.REFERENCED_COLUMN_NAME, '`) ON UPDATE ',RC.UPDATE_RULE, ' ON DELETE ',RC.DELETE_RULE) AS AddQuery
        FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE KCU, information_schema.REFERENTIAL_CONSTRAINTS RC
        WHERE
          KCU.CONSTRAINT_NAME = RC.CONSTRAINT_NAME
          AND KCU.REFERENCED_TABLE_SCHEMA = RC.UNIQUE_CONSTRAINT_SCHEMA
      AND KCU.REFERENCED_TABLE_SCHEMA = ['schema_name']) Queries
      GROUP BY SchemaName;
    ```

    Sorgu sonucunda bırakma yabancı anahtarını (ikinci sütun) çalıştırın.
- Şema hedef MySQL için Azure veritabanı içinde hiçbir tetikleyici olmaması gerekir. Hedef veritabanında Tetikleyicileri bırakmak için:
    ```
    SELECT Concat('DROP TRIGGER ', Trigger_Name, ';') FROM  information_schema.TRIGGERS WHERE TRIGGER_SCHEMA = 'your_schema';
    ```

## <a name="datatype-limitations"></a>DataType sınırlamaları
- **Sınırlama**: Kaynak MySQL veritabanında bir JSON veri türü varsa, geçiş sırasında sürekli eşitleme başarısız olur.

    **Geçici çözüm**: Orta metin ya da kaynak MySQL veritabanında longtext JSON veri türünü değiştirin.

- **Sınırlama**: Tablolarda birincil anahtar varsa, sürekli eşitleme başarısız olur.
 
    **Geçici çözüm**: Bir birincil anahtar tablosu geçişe devam etmek için geçici olarak ayarlar. Veri geçişi tamamlandıktan sonra birincil anahtarı kaldırabilirsiniz.

## <a name="lob-limitations"></a>LOB sınırlamaları
Büyük nesne (LOB) sütunları boyutu büyük büyüyebilir sütunlarıdır. MySQL, Orta metin için Longtext, Blob, Mediumblob, Longblob, vb. LOB veri türleri bazıları verilmiştir.

- **Sınırlama**: LOB veri türleri birincil anahtarlar kullanılıyorsa, geçiş başarısız olur.

    **Geçici çözüm**: Birincil anahtar, diğer veri türleri veya LOB olmayan sütunlar ile değiştirin.

- **Sınırlama**: Büyük nesne (LOB) sütun uzunluğu 32 KB'den daha büyük ise, hedefte veri kesilebilir. Bu sorguyu kullanarak LOB sütunu uzunluğunu kontrol edebilirsiniz:
    ```
    SELECT max(length(description)) as LEN from catalog;
    ```

    **Geçici çözüm**: 32 KB'den büyük LOB nesne varsa, mühendislik ekibiyle iletişime geçin. [isteyin Azure veritabanı geçişlerini](mailto:AskAzureDatabaseMigrations@service.microsoft.com). 

## <a name="other-limitations"></a>Diğer sınırlamalar
- Açma ve kapatma küme parantezleri {} başlangıcına ve sonuna kadar parola dizesi olan bir parola dizesi desteklenmiyor. Bu sınırlama, her iki bağlanmaya MySQL kaynak ve hedef Azure veritabanını MySQL için geçerlidir.
- Aşağıdaki DDLs desteklenmez:
    - Tüm bölüm DDLs
    - Tablo bırakma
    - Tabloyu yeniden adlandırma
- Kullanarak *alter tablo < table_name > ekleyin < column_name > sütun* başına veya bir tablonun ikinci sütun eklemek için deyimi desteklenmiyor. *Alter tablo < table_name > ekleyin < column_name > sütun* sütunu tablonun sonuna ekler.
- Sütun verileri tek bir parçası üzerinde oluşturulan dizinleri desteklenmiyor. Aşağıdaki deyim, sütun verileri yalnızca bir kısmını kullanarak bir dizin oluşturan bir örnek verilmiştir:
    ``` 
    CREATE INDEX partial_name ON customer (name(10));
    ```

- DMS tek tek geçiş etkinliği geçirilecek veritabanları sınırını dörttür.
