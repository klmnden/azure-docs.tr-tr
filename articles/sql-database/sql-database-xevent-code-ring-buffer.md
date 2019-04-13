---
title: SQL veritabanı için XEvent halka arabelleği kod | Microsoft Docs
description: Azure SQL veritabanı'nda halka arabelleği hedef kullanımını tarafından kolay ve hızlı bir şekilde yapılan bir Transact-SQL kod örneği sağlanmıştır.
services: sql-database
ms.service: sql-database
ms.subservice: monitor
ms.custom: ''
ms.devlang: PowerShell
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: jrasnik
manager: craigg
ms.date: 12/19/2018
ms.openlocfilehash: bb493fc0a9d3a9173ef4faf17b3cdd4e3781a557
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59526172"
---
# <a name="ring-buffer-target-code-for-extended-events-in-sql-database"></a>Halka arabelleği hedef kodu için SQL veritabanı'nda genişletilmiş olaylar

[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Bir kod örneği için bir genişletilmiş olay yakalama ve rapor bilgilerini bir test sırasında kolay hızlı şekilde istersiniz. Genişletilmiş olay verileri için kolay bir hedeftir [halka arabelleği hedef](https://msdn.microsoft.com/library/ff878182.aspx).

Bu konuda, bir Transact-SQL kod örneği sunar:

1. İle göstermek için verileri içeren bir tablo oluşturur.
2. Ayrıca var olan bir genişletilmiş olay için bir oturum oluşturur **sqlserver.sql_statement_starting**.
   
   * Belirli bir güncelleştirme dizesini içeren SQL deyimlerini olay sınırlıdır: **deyimi '% güncelleştirme tabEmployee %' gibi**.
   * Yani etkinliğin çıkışı bir tür halka arabelleği hedefine gönderilecek seçer **package0.ring_buffer**.
3. Olay oturumu başlatır.
4. Birkaç basit SQL UPDATE ifadeleriyle verir.
5. Halka arabelleği olay çıkış almak için bir SQL SELECT deyimi verir.
   
   * **sys.dm_xe_database_session_targets** ve diğer dinamik yönetim görünümlerini (Dmv'ler) birleştirilir.
6. Olay oturumu durdurur.
7. Halka arabelleği hedef kaynaklarını serbest bırakır.
8. Olay oturumu ve tanıtım tabloyu bırakır.

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure hesabı ve aboneliği [Ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Herhangi bir veritabanı içinde bir tablo oluşturabilirsiniz.
  
  * İsteğe bağlı olarak yapabilecekleriniz [oluşturmak bir **AdventureWorksLT** demo veritabanı](sql-database-get-started.md) dakikalar içinde.
* SQL Server Management Studio (ssms.exe), ideal olarak en son aylık güncelleştirme sürümü. 
  Gelen son ssms.exe indirebilirsiniz:
  
  * Başlıklı konusuna [SQL Server Management Studio'yu indirme](https://msdn.microsoft.com/library/mt238290.aspx).
  * [İndirme için doğrudan bir bağlantı.](https://go.microsoft.com/fwlink/?linkid=616025)

## <a name="code-sample"></a>Kod örneği

Çok az değişiklikle aşağıdaki halka arabelleği kod örneği, Azure SQL veritabanı veya Microsoft SQL Server üzerinde çalıştırılabilir. Bazı dinamik yönetim görünümlerini (Dmv'ler) adını ' v_eritabanı adım 5'te FROM yan tümcesinde kullanılan' düğümü varlığını farktır. Örneğin:

* sys.dm_xe<strong>v_eritabanı</strong>_session_targets
* sys.dm_xe_session_targets

&nbsp;

```sql
GO
----  Transact-SQL.
---- Step set 1.

SET NOCOUNT ON;
GO


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'tabEmployee')
BEGIN
    DROP TABLE tabEmployee;
END
GO


CREATE TABLE tabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO tabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO

---- Step set 2.


IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'eventsession_gm_azuresqldb51')
BEGIN
    DROP EVENT SESSION eventsession_gm_azuresqldb51
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE '%UPDATE tabEmployee%'
            )
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
GO

---- Step set 3.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = START;
GO

---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
GO

---- Step set 5.


SELECT
    se.name                      AS [session-name],
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source,
    st.target_data,
    CAST(st.target_data AS XML)  AS [target_data_XML]
FROM
               sys.dm_xe_database_session_event_actions  AS ac

    INNER JOIN sys.dm_xe_database_session_events         AS ev  ON ev.event_name = ac.event_name
        AND CAST(ev.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_object_columns AS oc
         ON CAST(oc.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_targets        AS st
         ON CAST(st.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_sessions               AS se
         ON CAST(ac.event_session_address AS BINARY(8)) = CAST(se.address AS BINARY(8))
WHERE
        oc.column_name = 'occurrence_number'
    AND
        se.name        = 'eventsession_gm_azuresqldb51'
    AND
        ac.action_name = 'sql_text'
ORDER BY
    se.name,
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source
;
GO

---- Step set 6.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = STOP;
GO

---- Step set 7.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO

---- Step set 8.


DROP EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE;
GO

DROP TABLE tabEmployee;
GO
```


&nbsp;

## <a name="ring-buffer-contents"></a>Halka arabelleği içeriği

Kod örneği çalıştırmak için ssms.exe kullandık.

Sonuçları görüntülemek için biz hücre bölümündeki sütun başlıklarından tıklanan **target_data_XML**.

Sonuçlar bölmesinde biz hücre bölümündeki sütun başlıklarından tıkladı ardından **target_data_XML**. Bunu başka bir dosya sekmesi, sonuç hücrenin içeriğinin, XML olarak görüntülenen ssms.exe oluşturdunuz.

Çıktı aşağıdaki bloğunda gösterilmiştir. Uzun görünüyor, ancak bu yalnızca iki  **\<olay >** öğeleri.

&nbsp;

```
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="2" eventCount="2" droppedCount="0" memoryUsed="1728">
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.317Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>7</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>184</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>328</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.327Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>10</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>340</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>486</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
</RingBufferTarget>
```


#### <a name="release-resources-held-by-your-ring-buffer"></a>Halka arabelleği tarafından tutulan kaynakları serbest bırakmak

Halka arabelleği ile işiniz bittiğinde kaldırın ve verme kaynaklarını serbest bir **ALTER** aşağıdaki gibidir:

```sql
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO
```


Olay oturumunuz tanımı güncelleştirildi, ancak bırakılmadı. Daha sonra olay oturumuna halka arabelleği başka bir örneğinin ekleyebilirsiniz:

```sql
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
```


## <a name="more-information"></a>Daha fazla bilgi

Azure SQL veritabanı'nda genişletilmiş olaylar için birincil anahtar konudur:

* [SQL veritabanı'nda olay konuları Genişletilmiş](sql-database-xevent-db-diff-from-svr.md), Microsoft SQL Server yerine Azure SQL veritabanı arasında farklılık gösteren genişletilmiş olaylar bazı yönlerini karşılaştırır.

Diğer kod örnek konuları genişletilmiş olaylar için aşağıdaki bağlantılarda kullanılabilir. Ancak, Microsoft Azure SQL veritabanı ve SQL Server örneği mı hedeflediği görmek için herhangi bir örnek düzenli olarak işaretlemeniz gerekir. Ardından, küçük değişiklikler örneği çalıştırmak için gerekli olup olmadığını karar verebilirsiniz.

* Azure SQL veritabanı için örnek kod: [SQL veritabanı'nda genişletilmiş olaylar için olay dosyası hedef kodu](sql-database-xevent-code-event-file.md)

<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](https://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](https://msdn.microsoft.com/library/bb630355.aspx)
-->
