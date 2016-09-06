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
   ms.date="08/27/2016"
   ms.author="sonyama;barbkess"/>

# Azure SQL Data Warehouse’a bağlanma

> [AZURE.SELECTOR]
- [Genel Bakış](sql-data-warehouse-connect-overview.md)
- [Kimlik Doğrulaması](sql-data-warehouse-authentication.md)
- [Sürücüler](sql-data-warehouse-connection-strings.md)

Azure SQL Data Warehouse’a bağlanmaya genel bakış. 

## Portaldan bağlantı dizesini bulma

SQL Data Warehouse bir Azure SQL sunucusu ile ilişkilidir. Bağlanmak için sunucunun tam adı gereklidir.  Örneğin, **myserver**.database.windows.net.

Tam sunucu adını bulmak için:

1. [Azure Portal][] gidin.
2. **SQL veritabanları** seçeneğine tıklayın ve ardından bağlanmak istediğiniz veritabanına tıklayın. Bu örnekte AdventureWorksDW örnek veritabanı kullanılmıştır.
3. Tam sunucu adını bulun.

    ![Tam sunucu adı][1]

## Bağlantı ayarları

SQL Data Warehouse, bağlantı ve nesne oluşturma sırasında birkaç ayarı standart hale getirir. Bunlar geçersiz kılınamaz.

| Veritabanı Ayarı       | Değer                        |
| :--------------------- | :--------------------------- |
| [ANSI_NULLS][]         | AÇIK                           |
| [QUOTED_IDENTIFIERS][] | AÇIK                           |
| [DATEFORMAT][]         | mdy                          |
| [DATEFIRST][]          | 7                            |

## Bağlantıları ve sorguları izleme

Bir bağlantı oluşturulup oturum kurulduktan sonra sorgu yazıp SQL Veri Ambarı’na göndermeye hazır olursunuz.  Oturumları ve sorguları izleme hakkında bilgi almak için bkz. [DMV’leri kullanarak iş yükünüzü izleme][].

## Sonraki adımlar

Visual Studio ve diğer uygulamalarla veri ambarınızı sorgulamaya başlamak için bkz. [Visual Studio ile sorgulama][]. 

<!--Articles-->
[Visual Studio ile sorgulama]: ./sql-data-warehouse-query-visual-studio.md
[DMV’leri kullanarak iş yükünüzü izleme]: ./sql-data-warehouse-manage-monitor.md

<!--MSDN references-->
[ANSI_NULLS]: https://msdn.microsoft.com/library/ms188048.aspx
[QUOTED_IDENTIFIERS]: https://msdn.microsoft.com/library/ms174393.aspx
[DATEFORMAT]: https://msdn.microsoft.com/library/ms189491.aspx
[DATEFIRST]: https://msdn.microsoft.com/library/ms181598.aspx

<!--Other-->
[Azure Portal]: https://portal.azure.com

<!--Image references-->
[1]: media/sql-data-warehouse-connect-overview/get-server-name.png





<!--HONumber=ago16_HO5-->


