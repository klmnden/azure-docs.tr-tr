<properties
   pageTitle="Azure SQL Data Warehouse'a Bağlanma | Microsoft Azure"
   description="Azure SQL Data Warehouse’a bağlanmaya genel bakış"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/20/2016"
   ms.author="sonyama;barbkess"/>

# Azure SQL Data Warehouse’a bağlanma

> [AZURE.SELECTOR]
- [Genel Bakış](sql-data-warehouse-connect-overview.md)
- [Kimlik Doğrulaması](sql-data-warehouse-authentication.md)
- [Sürücüler](sql-data-warehouse-connection-strings.md)

Azure SQL Data Warehouse’a bağlanmaya genel bakış. 

## Portaldan bağlantı dizesini bulma

SQL Data Warehouse bir Azure SQL sunucusu ile ilişkilidir. Bağlanmak için sunucunun tam adı (**servername**.database.windows.net*) gereklidir.

Tam sunucu adını bulmak için:

1. [Azure portalına][] gidin.
2. **SQL veritabanları** seçeneğine tıklayın ve ardından bağlanmak istediğiniz veritabanına tıklayın. Bu örnekte AdventureWorksDW örnek veritabanı kullanılmıştır.
3. Tam sunucu adını bulun.

    ![Tam sunucu adı][1]

## Bağlantı ayarları
SQL Data Warehouse, bağlantı ve nesne oluşturma sırasında birkaç ayarı standart hale getirir. Bunlar geçersiz kılınamaz.

| Veritabanı Ayarı   | Değer                        |
| :----------------- | :--------------------------- |
| ANSI_NULLS         | AÇIK                           |
| QUOTED_IDENTIFIERS | AÇIK                           |
| NO_COUNT           | KAPALI                          |
| DATEFORMAT         | mdy                          |
| DATEFIRST          | 7                            |
| Veritabanı Harmanlama | SQL_Latin1_General_CP1_CI_AS |

## Oturumlar ve istekler
Bir bağlantı oluşturulup oturum kurulduktan sonra sorgu yazıp SQL Data Warehouse’a göndermeye hazır olursunuz.

Her sorgu bir veya daha fazla istek tanımlayıcısıyla temsil edilir. Bu bağlantı üzerinde gönderilen tüm sorgular tek bir oturumun parçasıdır ve bu nedenle tek bir oturum kimliği ile temsil edilir.

Ancak, SQL Data Warehouse dağıtılmış bir MPP (Yüksek Düzeyde Paralel İşleme) sistemi olduğundan hem oturum hem de istek tanımlayıcıları SQL Server’a kıyasla biraz daha farklı gösterilir.

Oturumlar ve istekler mantıksal olarak kendi tanımlayıcılarıyla temsil edilir.

| Tanımlayıcı | Örnek değer |
| :--------- | :------------ |
| Oturum Kimliği | SID123456     |
| İstek Kimliği | QID123456     |

Oturum Kimliği ön ekinin SID (Oturum Kimliği kısaltması) ve istek ön ekinin QID (Sorgu Kimliği kısaltması) olduğuna dikkat edin.

Sorgu performansınızı izlerken sorgunuzu tanımlamaya yardımcı olmak üzere bu bilgilere ihtiyacınız olacaktır. [Azure Portal] ve dinamik yönetim görünümlerini kullanarak sorgu performansınızı izleyebilirsiniz.

Bu sorgu geçerli oturumunuzu tanımlar.

```sql
SELECT SESSION_ID()
;
```

Veri ambarınıza karşı çalışan veya kısa süre önce çalışmış tüm sorguları görüntülemek için aşağıdaki örneği kullanabilirsiniz. Bu bir görünüm oluşturur ve ardından görünümü çalıştırır.

```sql
CREATE VIEW dbo.vSessionRequests
AS
SELECT   s.[session_id]                                 AS Session_ID
        ,s.[status]                                     AS Session_Status
        ,s.[login_name]                                 AS Session_LoginName
        ,s.[login_time]                                 AS Session_LoginTime
        ,r.[request_id]                                 AS Request_ID
        ,r.[status]                                     AS Request_Status
        ,r.[submit_time]                                AS Request_SubmitTime
        ,r.[start_time]                                 AS Request_StartTime
        ,r.[end_compile_time]                           AS Request_EndCompileTime
        ,r.[end_time]                                   AS Request_EndTime
        ,r.[total_elapsed_time]                         AS Request_TotalElapsedDuration_ms
        ,DATEDIFF(ms,[submit_time],[start_time])        AS Request_InitiateDuration_ms
        ,DATEDIFF(ms,[start_time],[end_compile_time])   AS Request_CompileDuration_ms
        ,DATEDIFF(ms,[end_compile_time],[end_time])     AS Request_ExecDuration_ms
        ,[label]                                        AS Request_QueryLabel
        ,[command]                                      AS Request_Command
        ,[database_id]                                  AS Request_Database_ID
FROM    sys.dm_pdw_exec_requests r
JOIN    sys.dm_pdw_exec_sessions s  ON  r.[session_id] = s.[session_id]
WHERE   s.[session_id] <> SESSION_ID()
;

SELECT * FROM dbo.vSessionRequests;
```

## Sonraki adımlar

Visual Studio ve diğer uygulamalarla veri ambarınızı sorgulamaya başlamak için bkz. [Visual Studio ile sorgulama][].


<!--Arcticles-->

[Visual Studio ile sorgulama]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Azure portalına]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-connect-overview/get-server-name.png





<!--HONumber=Aug16_HO1-->


