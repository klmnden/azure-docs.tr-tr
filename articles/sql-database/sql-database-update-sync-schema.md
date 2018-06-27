---
title: Azure SQL veri eşitleme şema değişikliklerini çoğaltma işlemini otomatikleştirmek | Microsoft Docs
description: Azure SQL veri eşitleme şema değişikliklerini çoğaltma işlemini otomatikleştirmek öğrenin.
services: sql-database
ms.date: 06/19/2018
ms.topic: conceptual
ms.service: sql-database
author: allenwux
ms.author: xiwu
ms.reviewer: douglasl
manager: craigg
ms.custom: data-sync
ms.openlocfilehash: a39e060708514fdca11a5d89858486b442a18309
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37019596"
---
# <a name="automate-the-replication-of-schema-changes-in-azure-sql-data-sync"></a>Azure SQL veri eşitleme şema değişikliklerini çoğaltma otomatikleştirme

SQL veri eşitleme kullanıcıları Azure SQL veritabanları arasında verileri Eşitle sağlar ve SQL Server bir yönde veya her iki yönde şirket. SQL veri eşitleme geçerli sınırlamalar şema değişiklikleri çoğaltma için destek eksikliği biridir. Tablo şemasını her değiştirdiğinizde, hub ve tüm üyeleri de dahil olmak üzere el ile tüm uç noktaları üzerinde değişiklikleri uygulayın ve eşitleme şema güncelleştirmesi gerekir.

Bu makalede, tüm SQL veri eşitleme uç noktaları için şema değişiklikleri otomatik olarak çoğaltmak için bir çözüm sunar.
1. Bu çözüm DDL tetikleyicisi şema değişiklikleri izlemek için kullanır.
2. Tetikleyici bir izleme tablodaki şema değişikliği komutları ekler.
3. Bu izleme tablosu veri eşitleme hizmetini kullanarak tüm uç noktalara eşitlendi.
4. DML Tetikleyicileri ekleme sonra diğer uç noktalarda şema değişiklikleri uygulamak için kullanılır.

Bu makalede ALTER TABLE bir şema değişikliği bir örnek olarak kullanılmıştır, ancak bu çözüm de şema değişiklikleri diğer türleri için çalışır.

> [!IMPORTANT]
> Bu okumanızı öneririz dikkatlice hakkında özellikle bölümleri makale [sorun giderme](#troubleshooting) ve [diğer noktalar](#other), otomatik şema değişikliği çoğaltmaya uygulamak başlamadan önce Eşitleme ortamınızı. Ayrıca okumanızı öneririz [verileri Eşitle birden çok Bulut ve şirket içi veritabanları arasında SQL veri eşitleme ile](sql-database-sync-data.md). Bazı veritabanı işlemleri, bu makalede açıklanan çözüm kesilebilir. Bu sorunları gidermek için SQL Server ve Transact-SQL ek etki alanı bilgisi gerekli.

![Şema değişiklikleri çoğaltma otomatikleştirme](media/sql-database-update-sync-schema/automate-schema-changes.png)

## <a name="set-up-automated-schema-change-replication"></a>Otomatik şema değişikliği çoğaltmayı ayarlama

### <a name="create-a-table-to-track-schema-changes"></a>Şema değişiklikleri izlemek için bir tablo oluştur

Eşitleme grubundaki tüm veritabanlarının şema değişikliklerini izlemek için bir tablo oluşturun:

```sql
CREATE TABLE SchemaChanges (
ID bigint IDENTITY(1,1) PRIMARY KEY,
SqlStmt nvarchar(max),
[Description] nvarchar(max)
)
```

Bu tablo şema değişiklikleri sırasını izlemek için bir kimlik sütunu var. Daha fazla bilgi gerekirse oturum için daha fazla alanlar ekleyebilirsiniz.

### <a name="create-a-table-to-track-the-history-of-schema-changes"></a>Şema değişiklik geçmişini izlemek için bir tablo oluştur

Üzerindeki tüm uç noktaları, en son uygulanan şema değişikliği komut kimliği izlemek için bir tablo oluşturun.

```sql
CREATE TABLE SchemaChangeHistory (
LastAppliedId bigint PRIMARY KEY
)
GO

INSERT INTO SchemaChangeHistory VALUES (0)
```

### <a name="create-an-alter-table-ddl-trigger-in-the-database-where-schema-changes-are-made"></a>Şema değişiklikleri burada yapılan veritabanında bir ALTER TABLE DDL tetikleyicisi oluşturmak

ALTER TABLE işlemleri için DDL tetikleyicisi oluşturun. Yalnızca Bu tetikleyici burada şema değişiklikleri yapılan veritabanında oluşturmanız gerekir. Çakışmaları önlemek için yalnızca şema değişiklikleri eşitleme grubundaki bir veritabanında izin verir.

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

Tetikleyici tablosunda her ALTER TABLE komutu için şema değişiklik izleme bir kayıt ekler. Bu örnek, şema altında yapılan şema değişiklikleri çoğaltmaya önlemek için bir filtre ekler **DataSync**, bu veri eşitleme hizmeti tarafından yapılan olasılığı daha yüksektir. Yalnızca belirli türde bir şema değişiklikleri çoğaltmak istiyorsanız daha fazla filtreleri ekleyin.

Şema değişiklikleri diğer türlerini çoğaltmak için daha fazla Tetikleyicileri de ekleyebilirsiniz. Örneğin, saklı yordamları değişiklikleri çoğaltmak için CREATE_PROCEDURE, ALTER_PROCEDURE ve DROP_PROCEDURE tetikler oluşturun.

### <a name="create-a-trigger-on-other-endpoints-to-apply-schema-changes-during-insertion"></a>Diğer uç noktaları ekleme sırasında şema değişiklikleri uygulamak için bir Tetikleyici oluşturma

Diğer uç noktalara eşitlendiğinde, bu tetikleyici şema değişikliği komutu yürütür. Bu tetikleyici tüm uç noktaları, şema değişiklikleri yapılan burada dışındaki oluşturmanız gerekir (diğer bir deyişle, burada DDL tetiklemek veritabanında `AlterTableDDLTrigger` önceki adımda oluşturulan).

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

Bu tetikleyici sonra ekleme çalışır ve geçerli komutu sonraki çalıştırılması gerekip gerekmediğini denetler. Hiçbir şema değişikliği bildirimi atlanır ve ekleme bozuk olsa bile, tüm değişiklikler uygulanır kod mantığı sağlar.

### <a name="sync-the-schema-change-tracking-table-to-all-endpoints"></a>Şema değişiklik tablosu tüm uç noktalara izleme eşitleme

Tablosunda var olan eşitleme grubu veya yeni bir eşitleme grubu kullanarak tüm uç noktaları için şema değişiklik izleme eşitleyebilirsiniz. Tek yönlü eşitleme özellikle kullanıyorsanız, tüm uç noktaları için değişiklik izleme tabloda eşitlenebilen emin olun.

Bu tablo farklı uç noktalar farklı durumda tutar bu yana şema değişikliği geçmiş tablosu eşitlenmez.

### <a name="apply-the-schema-changes-in-a-sync-group"></a>Bir eşitleme grubundaki şema değişikliklerini uygula

DDL tetikleyicisi oluşturulduğu veritabanında yalnızca şema değişikliklerinin çoğaltılır. Şema değişikliklerinin diğer veritabanlarında çoğaltılmaz.

Şema değişiklikleri tüm uç noktalara çoğaltılır sonra da başlatmak veya yeni sütunlar eşitleniyor durdurmak için eşitleme şemasını güncelleştirmek için ek adımlar gerekir.

#### <a name="add-new-columns"></a>Yeni sütunlar ekleme

1.  Değiştirme şema olun.

2.  Tetikleyici oluşturur adımı tamamlayana kadar yeni sütunlar burada dahil herhangi bir veri değişikliği kaçının.

3.  Şema değişiklikleri tüm uç noktalara uygulanan kadar bekleyin.

4.  Veritabanı şeması yenileyin ve eşitleme şemasına yeni bir sütun ekleyin.

5.  Yeni bir sütun verilerde sonraki eşitleme işlemi sırasında eşitlenmedi.

#### <a name="remove-columns"></a>Sütunları kaldırma

1.  Sütunları eşitleme şemadan kaldırın. Veri Eşitleme bu sütunlardaki verileri eşitleniyor durdurur.

2.  Değiştirme şema olun.

3.  Veritabanı şeması yenileyin.

#### <a name="update-data-types"></a>Güncelleştirme veri türleri

1.  Değiştirme şema olun.

2.  Şema değişiklikleri tüm uç noktalara uygulanan kadar bekleyin.

3.  Veritabanı şeması yenileyin.

4.  Gelen değiştirirseniz, yeni ve eski veri türleri gibi tam uyumlu - değilse `int` için `bigint` -eşitleme Tetikleyicileri oluşturma adımları tamamlanmadan önce başarısız olabilir. Eşitleme başarılı olduktan sonra yeniden deneyin.

#### <a name="rename-columns-or-tables"></a>Sütun veya tablo yeniden adlandırma

Sütun veya tablo yeniden adlandırma veri çalışmamaya eşitleme yapar. Yeni Tablo veya sütuna, doldurma veri oluşturun ve ardından eski tablo ya da yeniden adlandırmak yerine sütun silin.

#### <a name="other-types-of-schema-changes"></a>Şema değişiklikleri diğer türleri

Şema değişiklikleri - diğer türleri için örneğin, saklı yordamlar oluşturmak veya bir dizin - bırakarak eşitleme şemasını güncelleştirme gerekli değildir.

## <a name="troubleshoot"></a> Otomatik şema değişikliği çoğaltma sorunlarını giderme

Bu konuda açıklanan çoğaltma mantığı makale durdurur çalışma bazı durumlarda - Örneğin, bir şema, Azure SQL veritabanı'nda desteklenmeyen bir şirket içi veritabanında değişiklik yaptıysanız. Bu durumda, şema değişiklik izleme tabloda eşitleme başarısız olur. Bu sorunu el ile düzeltmeniz:

1.  DDL tetikleyicisi devre dışı bırakın ve sorun düzeltilene kadar herhangi başka bir şema değişikliği kaçının.

2.  Sorun nerede gerçekleştiği uç nokta veritabanında nerede şema değişiklik yapılamıyor uç AFTER INSERT tetikleyicisi devre dışı bırakın. Bu eylem eşitlenmesine şema değişikliği komutu sağlar.

3.  Şema değişiklik izleme tabloda eşitlemek için eşitleme tetikler.

4.  Sorun nerede gerçekleştiği uç nokta veritabanında sorgu şema son uygulanan şema değişikliği komut Kimliğini almak için geçmiş tablosu değiştirin.

5.  Önceki adımda alınan tüm komutları kimliği değerinden daha büyük bir Kimliğe sahip listelemek için tablo izleme şema değişikliği sorgu.

    a.  Uç nokta veritabanında yürütülemez komutlarda yoksay. Şema tutarsızlığıdır ile ilgilenmeniz gereken. Tutarsızlığı uygulamanızı etkiliyorsa özgün şema değişikliklerini geri alın.

    b.  El ile uygulanması gereken komutları uygulayın.

6.  Şema değişiklik geçmişi tablosunun güncelleştirin ve doğru değerine ayarlanmış son uygulanan kimliği.

7.  Şema güncel olup olmadığını denetleyin.

8.  İkinci adımda devre dışı AFTER INSERT tetikleyicisi yeniden etkinleştirin.

9.  İlk adımda devre dışı DDL tetikleyicisi yeniden etkinleştirin.

Şema değişiklik izleme tablosu kayıtları temizlemek isterseniz, DELETE yerine TRUNCATE kullanın. Hiçbir zaman yeniden çekirdek oluşturma tablosunda DBCC CHECKIDENT kullanarak şema değişiklik izleme Kimlik sütununda. Yeni şema değişiklik izleme tablosu oluşturun ve reseeding gerekliyse DDL tetikleyicisi tablo adını güncelleştirin.

## <a name="other"></a> Diğer konular

-   Hub ve üye veritabanlarını yapılandırma veritabanı kullanıcılar şema değişikliği komutları yürütmek için yeterli izne sahip olması gerekir.

-   Daha fazla filtre yalnızca şema değişikliği Seçili tablolar veya işlem çoğaltmak için DDL tetikleyicisi ekleyebilirsiniz.

-   Yalnızca şema DDL tetikleyicisi oluşturulduğu veritabanında değişiklik yapabilirsiniz.

-   Bir şirket içi SQL Server veritabanında bir değişiklik yapıyorsanız, şema değişikliği Azure SQL veritabanı'nda desteklenen emin olun.

-   Şema değişiklikleri DDL tetikleyicisi oluşturulduğu veritabanı dışında veritabanlarında yaptıysanız değişiklikler çoğaltılmaz. Bu sorunu önlemek için diğer uç noktalarda değişiklikleri engellemek için DDL Tetikleyicileri oluşturabilirsiniz.

-   Değiştirmeniz gerekirse, şema şeması izleme tablosu değiştirmek, değişikliği yapın ve değişikliği tüm uç noktaları için el ile uygulamak için önce DDL tetikleyicisi devre dışı. Bir AFTER INSERT tetikleyicisi aynı tabloda yer alan şemada güncelleştirme çalışmaz.

-   Yeniden çekirdek oluşturma DBCC CHECKIDENT kullanarak kimlik sütunu yok.

-   TRUNCATE tablosunda şema değişiklik izleme verileri temizlemek için kullanmayın.
