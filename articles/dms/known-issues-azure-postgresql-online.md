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
ms.date: 04/23/2019
ms.openlocfilehash: 83c5401298d2682328da4e45d150d2d0416601fc
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64691943"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-to-azure-db-for-postgresql"></a>PostgreSQL için Azure DB online geçişleri ile bilinen sorunları/geçiş sınırlamaları

Bilinen sorunlar ve sınırlamalar online geçişleri PostgreSQL için Azure veritabanı için PostgreSQL ile ilişkili aşağıdaki bölümlerde açıklanmıştır. 

## <a name="online-migration-configuration"></a>Çevrimiçi geçişi yapılandırma
- PostgreSQL sunucusu kaynağı 9.5.11, 9.6.7 veya 10.3 sürümü çalıştırması gerekir veya üzeri. Daha fazla bilgi için bkz [desteklenen PostgreSQL veritabanı sürümlere](../postgresql/concepts-supported-versions.md).
- Yalnızca aynı sürüm geçişler desteklenir. Örneğin, 9.6.7 PostgreSQL için Azure veritabanı geçişi PostgreSQL 9.5.11 desteklenmiyor.

    > [!NOTE]
    > PostgreSQL için sürüm 10, şu anda DMS yalnızca PostgreSQL için Azure veritabanı 10.3 sürümüne geçişini destekler. Daha yeni sürümlerini PostgreSQL desteği, çok yakında planlıyorsanız.

- İçinde mantıksal çoğaltmayı etkinleştirmek için **PostgreSQL postgresql.conf kaynak** dosya için şu parametreleri ayarlayın:
    - **wal_level** mantıksal =
    - **max_replication_slots** = [veritabanı geçiş için en fazla sayısı]; 4 veritabanının geçirmek istiyorsanız, değer 4'e ayarlayın.
    - **max_wal_senders** [eşzamanlı olarak çalışan veritabanı sayısı] =; önerilen değer: 10
- DMS aracı IP'si için kaynak PostgresSQL pg_hba.conf Ekle
    1. DMS bir örneğini sağlama işlemini tamamladıktan sonra DMS IP adresini not edin.
    2. IP adresi gösterildiği pg_hba.conf dosyaya ekleyin:

        Tüm 172.16.136.18/10 md5 konak çoğaltma postgres 172.16.136.18/10 md5 barındırın

- Kullanıcının kaynak veritabanını barındıran sunucuda süper kullanıcı izninizin olması gerekir
- Kaynak veritabanı şemasında ENUM olması dışında kaynak ve hedef veritabanı şemalarını eşleşmesi gerekir.
- PostgreSQL için Azure veritabanı hedef şemasında yabancı anahtarlar olmaması gerekir. Yabancı anahtarlar bırakmak için aşağıdaki sorguyu kullanın:

    ```
                                SELECT Queries.tablename
           ,concat('alter table ', Queries.tablename, ' ', STRING_AGG(concat('DROP CONSTRAINT ', Queries.foreignkey), ',')) as DropQuery
                ,concat('alter table ', Queries.tablename, ' ', 
                                                STRING_AGG(concat('ADD CONSTRAINT ', Queries.foreignkey, ' FOREIGN KEY (', column_name, ')', 'REFERENCES ', foreign_table_name, '(', foreign_column_name, ')' ), ',')) as AddQuery
        FROM
        (SELECT
        tc.table_schema, 
        tc.constraint_name as foreignkey, 
        tc.table_name as tableName, 
        kcu.column_name, 
        ccu.table_schema AS foreign_table_schema,
        ccu.table_name AS foreign_table_name,
        ccu.column_name AS foreign_column_name 
    FROM 
        information_schema.table_constraints AS tc 
        JOIN information_schema.key_column_usage AS kcu
          ON tc.constraint_name = kcu.constraint_name
          AND tc.table_schema = kcu.table_schema
        JOIN information_schema.constraint_column_usage AS ccu
          ON ccu.constraint_name = tc.constraint_name
          AND ccu.table_schema = tc.table_schema
    WHERE constraint_type = 'FOREIGN KEY') Queries
      GROUP BY Queries.tablename;
    
    ```

    Sorgu sonucunda bırakma yabancı anahtarını (ikinci sütun) çalıştırın.

- Şemanın hedef PostgreSQL için Azure veritabanı içinde hiçbir tetikleyici olmaması gerekir. Hedef veritabanı Tetikleyicileri devre dışı bırakmak için aşağıdakileri kullanın:

     ```
    SELECT Concat('DROP TRIGGER ', Trigger_Name, ';') FROM  information_schema.TRIGGERS WHERE TRIGGER_SCHEMA = 'your_schema';
     ```

## <a name="datatype-limitations"></a>DataType sınırlamaları

- **Sınırlama**: Kaynak PostgreSQL veritabanına bir sabit listesi veri türü varsa, geçiş sırasında sürekli eşitleme başarısız olur.

    **Geçici çözüm**: PostgreSQL için Azure veritabanı'nda değişen karakter sabit listesi veri türüne değiştirin.

- **Sınırlama**: Tablolarda birincil anahtar varsa, sürekli eşitleme başarısız olur.

    **Geçici çözüm**: Bir birincil anahtar tablosu geçişe devam etmek için geçici olarak ayarlar. Veri geçişi tamamlandıktan sonra birincil anahtarı kaldırabilirsiniz.

## <a name="lob-limitations"></a>LOB sınırlamaları
Büyük nesne (LOB) sütunları fazla büyüyebilir sütunlarıdır. PostgreSQL için örnekleri LOB veri türleri, XML, JSON, resim, metin, vb. içerir.

- **Sınırlama**: LOB veri türleri birincil anahtarlar kullanılıyorsa, geçiş başarısız olur.

    **Geçici çözüm**: Birincil anahtar, diğer veri türleri veya LOB olmayan sütunlar ile değiştirin.

- **Sınırlama**: Büyük nesne (LOB) sütun uzunluğu 32 KB'den daha büyük ise, hedefte veri kesilebilir. Bu sorguyu kullanarak LOB sütunu uzunluğunu kontrol edebilirsiniz:

    ```
    SELECT max(length(cast(body as text))) as body FROM customer_mail
    ```

    **Geçici çözüm**: 32 KB'den büyük LOB nesne varsa, mühendislik ekibiyle iletişime geçin. [isteyin Azure veritabanı geçişlerini](mailto:AskAzureDatabaseMigrations@service.microsoft.com).

- **Sınırlama**: Tablodaki LOB sütunları vardır ve birincil anahtar ödenmez tablo için verileri bu tablo için geçirilmiş olabilir değil.

    **Geçici çözüm**: Bir birincil anahtar tablosu geçişe devam etmek için geçici olarak ayarlar. Veri geçişi tamamlandıktan sonra birincil anahtarı kaldırabilirsiniz.

## <a name="postgresql10-workaround"></a>PostgreSQL10 geçici çözüm
PostgreSQL 10.x pg_xlog klasör adları ve bu nedenle geçiş beklendiği gibi çalışmıyor neden çeşitli değişiklikler yapar. PostgreSQL geçiş yapıyorsanız 10.x PostgreSQL 10.3 için Azure veritabanı kaynak PostgreSQL veritabanında pg_xlog işlevleri çevresinde sarmalayıcı işlevi oluşturmak için aşağıdaki betiği yürütün.

```
BEGIN;
CREATE SCHEMA IF NOT EXISTS fnRenames;
CREATE OR REPLACE FUNCTION fnRenames.pg_switch_xlog() RETURNS pg_lsn AS $$ 
   SELECT pg_switch_wal(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_xlog_replay_pause() RETURNS VOID AS $$ 
   SELECT pg_wal_replay_pause(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_xlog_replay_resume() RETURNS VOID AS $$ 
   SELECT pg_wal_replay_resume(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_current_xlog_location() RETURNS pg_lsn AS $$ 
   SELECT pg_current_wal_lsn(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_is_xlog_replay_paused() RETURNS boolean AS $$ 
   SELECT pg_is_wal_replay_paused(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_xlogfile_name(lsn pg_lsn) RETURNS TEXT AS $$ 
   SELECT pg_walfile_name(lsn); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_last_xlog_replay_location() RETURNS pg_lsn AS $$ 
   SELECT pg_last_wal_replay_lsn(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_last_xlog_receive_location() RETURNS pg_lsn AS $$ 
   SELECT pg_last_wal_receive_lsn(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_current_xlog_flush_location() RETURNS pg_lsn AS $$ 
   SELECT pg_current_wal_flush_lsn(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_current_xlog_insert_location() RETURNS pg_lsn AS $$ 
   SELECT pg_current_wal_insert_lsn(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_xlog_location_diff(lsn1 pg_lsn, lsn2 pg_lsn) RETURNS NUMERIC AS $$ 
   SELECT pg_wal_lsn_diff(lsn1, lsn2); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_xlogfile_name_offset(lsn pg_lsn, OUT TEXT, OUT INTEGER) AS $$ 
   SELECT pg_walfile_name_offset(lsn); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_create_logical_replication_slot(slot_name name, plugin name, 
   temporary BOOLEAN DEFAULT FALSE, OUT slot_name name, OUT xlog_position pg_lsn) RETURNS RECORD AS $$ 
   SELECT slot_name::NAME, lsn::pg_lsn FROM pg_catalog.pg_create_logical_replication_slot(slot_name, plugin, 
   temporary); $$ LANGUAGE SQL;
ALTER USER PG_User SET search_path = fnRenames, pg_catalog, "$user", public;

-- DROP SCHEMA fnRenames CASCADE;
-- ALTER USER PG_User SET search_path TO DEFAULT;
COMMIT;
```

## <a name="other-limitations"></a>Diğer sınırlamalar
- Veritabanı adı, noktalı virgül (;) içeremez.
- Açma ve kapatma küme parantezleri {} olan bir parola dizesi desteklenmiyor. Her iki bağlanmaya PostgreSQL kaynak ve hedef Azure veritabanını Postgresql'ye bu sınırlama geçerlidir.
- Yakalanan bir tabloda bir birincil anahtarı olmalıdır. Bir tabloda bir birincil anahtar yoksa, silme ve güncelleştirme kayıt işlemleri sonucu tahmin edilemez olacaktır.
- Bir birincil anahtar kesimi güncelleştirme göz ardı edilir. Bu gibi durumlarda, bu tür bir güncelleştirmeyi uygulamadan hedef tarafından herhangi bir satır güncelleştirilmedi ve özel durumlar tablosuna yazılan bir kayıt sonuçlanır bir güncelleştirme olarak tanımlanır.
- Aynı ada ancak farklı bir durumda (örneğin table1, tablo1 ve Table1) sahip birden fazla tablo geçişini öngörülemeyen davranışlara neden olabilir ve bu nedenle desteklenmiyor.
- İşlenmesi değiştirme [oluştur | ALTER | Bir iç işlev/yordam gövde bloğundaki veya diğer iç içe geçmiş yapılar tutulduğu sürece bırakma] tablo DDLs desteklenir. Örneğin, aşağıdaki değişikliği yakalanmaz:

    ```
    CREATE OR REPLACE FUNCTION pg.create_distributors1() RETURNS void
    LANGUAGE plpgsql
    AS $$
    BEGIN
    create table pg.distributors1(did serial PRIMARY KEY,name varchar(40)
    NOT NULL);
    END;
    $$;
    ```

- Kesme işlemleri (sürekli eşitleme) işlenmesini değişikliği desteklenmiyor. Geçişini bölümlenmiş tablolar desteklenmiyor. Bölümlenmiş bir tablodaki algılandığında, aşağıdakiler gerçekleşir:
    - Veritabanı, üst ve alt tablo listesini bildirir.
    - Hedef tablo olarak seçilen tabloları aynı özelliklere sahip olağan bir tablo olarak oluşturulur.
    - Kaynak veritabanında üst tablo kendi alt tablolar aynı birincil anahtar değeri varsa, "yinelenen anahtar" hatası oluşturulur.
- DMS tek tek geçiş etkinliği geçirilecek veritabanları sınırını dörttür.