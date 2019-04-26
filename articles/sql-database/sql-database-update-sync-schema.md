---
title: Azure SQL Data Sync şema değişikliklerinin çoğaltmayı otomatik hale getirme | Microsoft Docs
description: Azure SQL Data Sync şema değişikliklerinin çoğaltmayı otomatik hale getirme hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: data sync
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: carlrab
manager: craigg
ms.date: 11/14/2018
ms.openlocfilehash: 712ccfa71c85629111428a4e0c7acaea050942b8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60331353"
---
# <a name="automate-the-replication-of-schema-changes-in-azure-sql-data-sync"></a>Azure SQL Data Sync şema değişikliklerinin çoğaltmayı otomatik hale getirme

SQL Data Sync kullanıcıların verileri Azure SQL veritabanı arasında eşitleme sağlar ve SQL Server tek yönlü veya çift yönlü şirket. SQL Data Sync, geçerli sınırlamalar şema değişiklik çoğaltması için destek eksikliği biridir. Tablo şemasını değiştirdiğiniz her durumda hub ve tüm üyeleri dahil olmak üzere el ile tüm uç noktalarda, değişiklikleri uygulamak ve ardından eşitleme şemasını güncelleştirmek gerekir.

Bu makalede, tüm SQL Data Sync uç noktalar için şema değişiklikleri otomatik olarak çoğaltılmasını bir çözüm sunar.
1. Bu çözüm, bir DDL tetikleyicisi şema değişiklikleri izlemek için kullanır.
1. Tetikleyici şema değişikliği komutları bir izleme tablosuna ekler.
1. Bu izleme tablosu veri eşitleme hizmeti kullanan tüm uç noktalar için eşitlenir.
1. DML Tetikleyicileri ekleme sonra diğer uç noktalardan üzerinde şema değişiklikleri uygulamak için kullanılır.

Bu makalede, ALTER TABLE bir şema değişikliği örnek olarak kullanılmıştır, ancak bu çözüm, şema değişikliklerinin diğer türleri için de çalışır.

> [!IMPORTANT]
> Bu okumanızı öneririz dikkatlice hakkında özellikle bölümleri makalesi [sorun giderme](#troubleshoot) ve [dikkat edilecek diğer noktalar](#other)otomatik şema değişikliği çoğaltmaya uygulamak başlamadan önce Eşitleme ortamınızı. Ayrıca okumanızı öneririz [verileri Eşitle birden fazla Bulut ve şirket içi veritabanında SQL Data Sync ile](sql-database-sync-data.md). Bu makalede açıklanan çözüm bazı veritabanı işlemleri kesilebilir. Bu sorunları gidermek için SQL Server Transact-SQL ve ek etki alanı bilgisini gerekebilir.

![Şema değişiklikleri çoğaltmasını otomatikleştirmesi](media/sql-database-update-sync-schema/automate-schema-changes.png)

## <a name="set-up-automated-schema-change-replication"></a>Otomatik şema değişikliği çoğaltmayı ayarlama

### <a name="create-a-table-to-track-schema-changes"></a>Şema değişiklikleri izlemek için bir tablo oluşturma

Eşitleme grubundaki tüm veritabanlarında şema değişiklikleri izlemek için bir tablo oluşturun:

```sql
CREATE TABLE SchemaChanges (
ID bigint IDENTITY(1,1) PRIMARY KEY,
SqlStmt nvarchar(max),
[Description] nvarchar(max)
)
```

Bu tablonun sırasını şema değişiklikleri izlemek için bir kimlik sütunu var. Daha fazla bilgi gerekirse oturum için daha fazla alan ekleyebilirsiniz.

### <a name="create-a-table-to-track-the-history-of-schema-changes"></a>Şema değişiklikleri geçmişini izlemek için bir tablo oluşturma

Tüm uç noktalarda, son uygulanan şema değişikliği komut kimliği izlemek için bir tablo oluşturun.

```sql
CREATE TABLE SchemaChangeHistory (
LastAppliedId bigint PRIMARY KEY
)
GO

INSERT INTO SchemaChangeHistory VALUES (0)
```

### <a name="create-an-alter-table-ddl-trigger-in-the-database-where-schema-changes-are-made"></a>Burada şema değişiklikleri yapılan veritabanında bir ALTER TABLE DDL tetikleyicisi oluşturma

Bir DDL tetikleyicisi için ALTER TABLE işlemlerine oluşturun. Bu tetikleyici şema değişiklikleri burada yapılan veritabanında oluşturmanız yeterlidir. Çakışmaları önlemek için yalnızca şema değişiklikleri bir veritabanında bir eşitleme grubuna izin verir.

```sql
CREATE TRIGGER AlterTableDDLTrigger
ON DATABASE
FOR ALTER_TABLE
AS

-- You can add your own logic to filter ALTER TABLE commands instead of replicating all of them.

IF NOT (EVENTDATA().value('(/EVENT_INSTANCE/SchemaName)[1]', 'nvarchar(512)') like 'DataSync')

INSERT INTO SchemaChanges (SqlStmt, Description)
    VALUES (EVENTDATA().value('(/EVENT_INSTANCE/TSQLCommand/CommandText)[1]', 'nvarchar(max)'), 'From DDL trigger')
```

Tetikleyici şema değişiklik izleme tabloda her ALTER TABLE komutu için bir kayıt ekler. Bu örnek şema şema değişikliklerinin çoğaltılması önlemek için bir filtre ekler **DataSync**, çünkü bunlar veri eşitleme hizmeti tarafından yapılan olasılığı daha yüksektir. Yalnızca belirli türdeki şema değişiklikleri çoğaltmak istiyorsanız daha fazla filtre ekleyin.

Şema değişiklikleri diğer türlerini çoğaltmak için daha fazla tetikleyici de ekleyebilirsiniz. Örneğin, saklı yordamlar için değişiklikleri çoğaltmak için CREATE_PROCEDURE ve ALTER_PROCEDURE DROP_PROCEDURE Tetikleyicileri oluşturun.

### <a name="create-a-trigger-on-other-endpoints-to-apply-schema-changes-during-insertion"></a>Bir tetikleyici ekleme sırasında şema değişikliklerini uygulamak için diğer uç noktalar oluşturma

Diğer uç noktalara eşitlendiğinde, bu tetikleyici şema değişikliği komutu yürütür. Tüm uç noktalarda, şema değişiklikleri yapılan burada dışındaki Bu tetikleyici oluşturmanız gerekir (diğer bir deyişle, burada DDL tetikleyicisi veritabanında `AlterTableDDLTrigger` önceki adımda oluşturulan).

```sql
CREATE TRIGGER SchemaChangesTrigger
ON SchemaChanges
AFTER INSERT
AS
DECLARE \@lastAppliedId bigint
DECLARE \@id bigint
DECLARE \@sqlStmt nvarchar(max)
SELECT TOP 1 \@lastAppliedId=LastAppliedId FROM SchemaChangeHistory
SELECT TOP 1 \@id = id, \@SqlStmt = SqlStmt FROM SchemaChanges WHERE id \> \@lastAppliedId ORDER BY id
IF (\@id = \@lastAppliedId + 1)
BEGIN
    EXEC sp_executesql \@SqlStmt
        UPDATE SchemaChangeHistory SET LastAppliedId = \@id
    WHILE (1 = 1)
    BEGIN
        SET \@id = \@id + 1
        IF exists (SELECT id FROM SchemaChanges WHERE ID = \@id)
            BEGIN
                SELECT \@sqlStmt = SqlStmt FROM SchemaChanges WHERE ID = \@id
                EXEC sp_executesql \@SqlStmt
                UPDATE SchemaChangeHistory SET LastAppliedId = \@id
            END
        ELSE
            BREAK;
    END
END
```

Bu tetikleyici eklemeden sonra çalışır ve geçerli komut sonraki çalıştırılması gerekip gerekmediğini denetler. Kod mantığınızı herhangi bir şema değişikliği deyimi atlanır ve ekleme sıralamaya olsa bile, tüm değişiklikler uygulanır sağlar.

### <a name="sync-the-schema-change-tracking-table-to-all-endpoints"></a>Tabloda tüm uç noktaları için izleme şema değişikliği eşitleme

Şema değişiklik tablosu mevcut eşitleme grubuna veya yeni bir eşitleme grubu kullanarak tüm uç noktaları için izleme eşitleyebilirsiniz. Tek yönlü eşitleme özellikle kullanıyorsanız, tüm uç noktalar için değişiklik izleme tabloda eşitlenebilir emin olun.

Bu tabloda farklı uç noktalarda farklı durumunu koruyan beri şema değişiklik geçmiş tablosu, eşitleme.

### <a name="apply-the-schema-changes-in-a-sync-group"></a>Bir eşitleme grubunda şema değişikliklerini uygula

DDL tetikleyicisi oluşturulduğu veritabanına yapılan şema değişiklikleri çoğaltılır. Diğer veritabanlarında şema değişikliklerinin çoğaltılmaz.

Şema değişiklikleri tüm uç noktalarına çoğaltılır sonra da başlatmak veya durdurmak yeni sütunlar eşitleme için eşitleme şemasını güncelleştirmek için ek adımlar uygulaması gerekir.

#### <a name="add-new-columns"></a>Yeni sütun ekleme

1.  Şema değişiklik yapın.

1.  Tetikleyici oluşturur. adımı tamamlayana kadar yeni sütunlar burada katılan herhangi bir veri değişikliği kaçının.

1.  Tüm uç noktalar için şema değişiklikleri uygulanana kadar bekleyin.

1.  Veritabanı şemasını yenilemesi ve eşitleme şemasına yeni bir sütun ekleyin.

1.  Yeni sütundaki veriler, sonraki eşitleme işlemi sırasında eşitlenir.

#### <a name="remove-columns"></a>Sütunları kaldırma

1.  Eşitleme şemasından sütunları kaldırın. Veri eşitleme, bu sütunlardaki verileri eşitleniyor durdurur.

1.  Şema değişiklik yapın.

1.  Veritabanı şemasını yeniler.

#### <a name="update-data-types"></a>Güncelleştirme veri türleri

1.  Şema değişiklik yapın.

1.  Tüm uç noktalar için şema değişiklikleri uygulanana kadar bekleyin.

1.  Veritabanı şemasını yeniler.

1.  Gelen değiştirirseniz yeni ve eski veri türleri gibi tam olarak uyumlu - değilse `int` için `bigint` -eşitleme Tetikleyicileri oluşturma adımları tamamlanmadan önce başarısız olabilir. Eşitleme başarılı olduktan sonra yeniden deneyin.

#### <a name="rename-columns-or-tables"></a>Sütunları veya tabloları yeniden adlandırma

Sütunları veya tabloları yeniden adlandırma, veri çalışmamaya eşitleme yapar. Yeni Tablo veya sütun doldurma veri oluşturun ve eski bir tablo veya sütun yeniden adlandırma yerine silin.

#### <a name="other-types-of-schema-changes"></a>Şema değişiklikleri diğer türleri

Şema değişiklikleri - diğer türleri için örneğin, saklı yordamlar oluşturma veya bir dizin - bırakarak eşitleme şemasını güncelleştirme gerekli değildir.

## <a name="troubleshoot"></a> Otomatik şema değişikliği çoğaltma sorunlarını giderme

Bu konuda açıklanan çoğaltma mantığı makale durdurur çalışan bazı durumlarda - Örneğin, bir şema, Azure SQL veritabanında desteklenmeyen bir şirket içi veritabanı değişikliği yaptıysanız. Bu durumda, şema değişiklik izleme tabloda eşitleme başarısız olur. Bu sorunu el ile düzeltmeniz:

1.  DDL tetikleyicisi devre dışı bırakmak ve herhangi başka bir şema değişikliği sorun düzeltilene kadar kaçının.

1.  Sorunun gerçekleştiği uç noktası veritabanında burada şema değişiklik yapılamıyor uç noktasında sonra eklemek tetikleyici devre dışı bırakın. Bu eylem şema değişikliği komut eşitlenmesine imkan sağlar.

1.  Şema değişiklik izleme tabloda eşitlemek için eşitleme tetikleyin.

1.  Sorunun gerçekleştiği uç noktası veritabanında sorgu şeması son uygulanan şema değişikliği komut Kimliğini almak için geçmiş tablosu ile değiştirin.

1.  Şema değişiklik izleme tabloda kimliği değerinden daha büyük bir Kimliğe sahip tüm komutları önceki adımda aldığınız listelemek için sorgu.

    a.  Uç nokta veritabanında yürütülen komutları yoksayın. Şema tutarsızlığıdır ile uğraşmak gerekir. Tutarsızlık, uygulamanızın etkiliyorsa özgün şema değişiklikleri geri döndür.

    b.  El ile uygulanması gereken komutları uygulayın.

1.  Şema değişiklik geçmişi tablosunu güncelleştirmek ve son uygulanan kimliği doğru değerine ayarlayın.

1.  Şema güncel olup olmadığını denetleyin.

1.  İkinci adımda devre dışı sonra Ekle tetikleyici yeniden etkinleştirin.

1.  İlk adımda devre dışı DDL tetikleyicisi yeniden etkinleştirin.

Şema değişiklik izleme tabloda kayıtları temizlemek isterseniz, silme, TRUNCATE yerine kullanın. Hiçbir zaman yeniden çekirdek oluşturma DBCC CHECKIDENT kullanarak tablo izleme şema değişikliği kimlik sütunu. Yeni şema değişiklik izleme tablosu oluşturun ve reseeding gerekliyse DDL tetikleyicisi tablo adını güncelleştirin.

## <a name="other"></a> Dikkat edilecek diğer noktalar

-   Hub ve üye veritabanlarını yapılandırma veritabanı kullanıcılar şema değişikliği komutları yürütmek için yeterli izne sahip olması gerekir.

-   DDL tetikleyicisi yalnızca seçilen tablolar veya işlem şema değişikliği çoğaltmak için daha fazla filtre ekleyebilirsiniz.

-   Bu gibi durumlarda, şema değişiklikleri yalnızca DDL tetikleyicisi oluşturulduğu veritabanında yapabilirsiniz.

-   Bir şirket içi SQL Server veritabanında değişiklik yapıyorsanız, Azure SQL veritabanı'nda desteklenen şema değişikliği emin olun.

-   DDL tetikleyicisi oluşturulduğu veritabanı dışında veritabanlarında şema değişiklikleri yaptıysanız değişiklikler çoğaltılmaz. Bu sorunu önlemek için diğer uç noktalardan değişiklikleri engellemek için DDL tetikleyicilerini oluşturabilirsiniz.

-   Gerekirse şema değişiklik izleme tablosunun şeması değiştirmek, değişikliği yapın ve değişikliği tüm uç noktalar için el ile uygulamanız önce DDL tetikleyicisi devre dışı bırakın. Aynı tabloda bir sonra Ekle tetikleyicisinin şeması güncelleştiriliyor çalışmaz.

-   Yeniden çekirdek oluşturma DBCC CHECKIDENT kullanılarak kimlik sütunu yok.

-   TRUNCATE şema değişiklik izleme tabloda verileri temizlemek için kullanmayın.

## <a name="next-steps"></a>Sonraki adımlar

SQL Data Sync hakkında daha fazla bilgi için bkz.:

-   Genel Bakış - [verileri Eşitle birden fazla Bulut ve şirket içi veritabanı arasında Azure SQL Data Sync ile](sql-database-sync-data.md)
-   Data Sync'i Ayarla
    - Portalda - [Öğreticisi: Azure SQL veritabanı ve SQL Server arasında verileri eşitlemek amacıyla şirket içi SQL Data Sync'i Ayarla](sql-database-get-started-sql-data-sync.md)
    - PowerShell ile
        -  [PowerShell kullanarak birden çok Azure SQL veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-sql-databases.md)
        -  [PowerShell kullanarak bir Azure SQL Veritabanı ile SQL Server şirket içi veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-azure-onprem.md)
-   Veri Eşitleme Aracısı - [veri Aracısı Azure SQL Data Sync için eşitleme](sql-database-data-sync-agent.md)
-   En iyi uygulamalar - [en iyi uygulamalar için Azure SQL Data Sync](sql-database-best-practices-data-sync.md)
-   İzleyici - [SQL Data Sync'i Azure İzleyici ile izleme günlükleri](sql-database-sync-monitor-oms.md)
-   Sorun giderme - [Azure SQL Data Sync ile ilgili sorunları giderme](sql-database-troubleshoot-data-sync.md)
-   Eşitleme şemasını güncelleştirmek
    -   PowerShell ile- [var olan bir eşitleme grubunda eşitleme şemasını güncelleştirmek için PowerShell kullanma](scripts/sql-database-sync-update-schema.md)
