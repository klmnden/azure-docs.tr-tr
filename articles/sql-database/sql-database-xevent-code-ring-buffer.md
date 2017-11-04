---
title: "SQL veritabanı için XEvent halka arabelleği kodu | Microsoft Docs"
description: "Kolay ve hızlı bir şekilde halka arabelleği hedefinin Azure SQL Database kullanılarak yapılan bir Transact-SQL kod örneğini sağlar."
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: 
tags: 
ms.assetid: 2510fb3f-c8f2-437a-8f49-9d5f6c96e75b
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: genemi
ms.openlocfilehash: 61251eb9b125209ffd15adafdb0bace495e7cadd
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="ring-buffer-target-code-for-extended-events-in-sql-database"></a>Halka arabelleği hedef kod SQL veritabanında genişletilmiş olaylar

[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Genişletilmiş olay yakalama ve rapor bilgilerini bir test sırasında kolay hızlı şekilde için tam bir kod örneği istiyor. Genişletilmiş olay verilerini kolay hedefidir [halka arabelleği hedef](http://msdn.microsoft.com/library/ff878182.aspx).

Bu konuda, bir Transact-SQL kodunu örnek sunulmaktadır:

1. İle göstermek için veri içeren bir tablo oluşturur.
2. Ayrıca varolan bir genişletilmiş olay için bir oturum oluşturur **sqlserver.sql_statement_starting**.
   
   * Olay belirli bir güncelleştirme dizesini içeren SQL deyimlerini sınırlıdır: **deyimi '% güncelleştirme tabEmployee %' gibi**.
   * Olayın çıkış türü halka arabelleği hedefe öğesine göndermek seçtiği **package0.ring_buffer**.
3. Olay oturumu başlatır.
4. Birkaç basit SQL güncelleştirme deyimleri verir.
5. Olay çıkış halkası arabelleğinden almak için SQL SELECT deyimine verir.
   
   * **sys.dm_xe_database_session_targets** ve diğer dinamik yönetim görünümlerini (Dmv'leri) birleştirilir.
6. Olay oturumu durdurur.
7. Kaynaklarını serbest bırakmak için halka arabelleği hedef bırakır.
8. Olay oturumu ve tanıtım tabloyu bırakır.

## <a name="prerequisites"></a>Ön koşullar

* Bir Azure hesabı ve aboneliği [Ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Herhangi bir veritabanı, bir tablodaki oluşturabilirsiniz.
  
  * İsteğe bağlı olarak yapabileceğiniz [oluşturma bir **AdventureWorksLT** demo veritabanı](sql-database-get-started.md) dakika.
* SQL Server Management Studio (ssms.exe), ideal olarak en son aylık güncelleştirme sürümü. 
  En son ssms.exe öğesinden yükleyebilirsiniz:
  
  * Başlıklı konuda [SQL Server Management Studio'yu indirme](http://msdn.microsoft.com/library/mt238290.aspx).
  * [İndirme doğrudan bağlantı.](http://go.microsoft.com/fwlink/?linkid=616025)

## <a name="code-sample"></a>Kod örneği

Çok küçük değişiklik yapmadan aşağıdaki halka arabelleği kod örneği, Azure SQL veritabanı ya da Microsoft SQL Server çalıştırılabilir. Bazı dinamik yönetim görünümlerini (Dmv'leri) adına ' _veritabanı adım 5'te FROM yan tümcesinde kullanılan' düğümü varlığını farktır. Örneğin:

* sys.dm_xe**_veritabanı**_session_targets
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

Sonuçları görüntülemek için biz hücre sütun başlığı altında tıklattınız **target_data_XML**.

Sonuçlar bölmesinde biz hücre sütun başlığı altında tıklattınız sonra **target_data_XML**. Bunu başka bir dosya sekmesi, sonuç hücrenin içeriğinin, XML olarak görüntülenen ssms.exe oluşturdunuz.

Çıktı aşağıdaki bloğunda gösterilir. Uzun görünüyor, ancak bunu yalnızca iki  **<event>**  öğeleri.

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


#### <a name="release-resources-held-by-your-ring-buffer"></a>Halka arabelleği ile tutulan yayın kaynakları

Halka arabelleği ile işiniz bittiğinde, kaldırmak ve veren kaynaklarını serbest bir **ALTER** aşağıdaki gibi:

```sql
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO
```


Olay oturumunuz tanımını güncelleştirildi, ancak atılmadı. Daha sonra olay oturumuna halka arabelleği başka bir örneği ekleyebilirsiniz:

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

Azure SQL veritabanı genişletilmiş olaylar için birincil anahtar konusudur:

* [SQL veritabanı olay konuları Genişletilmiş](sql-database-xevent-db-diff-from-svr.md), Microsoft SQL Server ile Azure SQL veritabanı arasında farklılık genişletilmiş olaylar bazı yönlerini karşılaştırır.

Aşağıdaki bağlantılarda diğer kod örnek konuları genişletilmiş olaylar için kullanılabilir. Ancak, örnek Microsoft SQL Server Azure SQL veritabanına karşı hedefler olup olmadığını görmek için herhangi bir örnek düzenli olarak denetlemeniz gerekir. Ardından, küçük değişiklikler örneği çalıştırmak için gerekli olup olmadığını karar verebilirsiniz.

* Azure SQL veritabanı için kod örneği: [SQL veritabanında genişletilmiş olaylar için olay dosyası hedef kodu](sql-database-xevent-code-event-file.md)

<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
