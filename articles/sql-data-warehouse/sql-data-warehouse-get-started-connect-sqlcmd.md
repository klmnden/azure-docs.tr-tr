<properties
   pageTitle="Azure SQL Data Warehouse sorgulama (sqlcmd)| Microsoft Azure"
   description="Azure SQL Data Warehouse’u sqlcmd Komut Satırı Yardımcı Programı ile sorgulama."
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
   ms.date="08/30/2016"
   ms.author="mausher;barbkess;sonyama"/>

# Azure SQL Data Warehouse sorgulama (sqlcmd)

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Bu izlenecek yolda Azure SQL Veri Ambarı’nı sorgulamak için [sqlcmd][] komut satırı yardımcı programı kullanılır.  

## 1. Bağlan

**Sqlcmd** kullanmaya başlamadan önce komut istemini açın ve [sqlcmd][] öğesinden sonra SQL Veri Ambarı veritabanınızın bağlantı dizesini girin. Bağlantı dizesi için aşağıdaki parametreler gereklidir:

+ **Server (-S):** `<`Sunucu Adı`>`.database.windows.net biçiminde belirtilmiş sunucu
+ **Database (-d):** Veritabanı adı.
+ **User (-U):** `<`Kullanıcı biçimindeki sunucu kullanıcısı`>`
+ **Password (-P):** Kullanıcıyla ilişkili parola.
+ **Enable Quoted Identifiers (-I):** Bir SQL Data Warehouse örneğine bağlanmak için tırnak işaretli tanımlayıcıların etkinleştirilmesi gerekir.

Örneğin, bağlantı dizeniz aşağıdaki gibi görünebilir:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

> [AZURE.NOTE] Tırnak içindeki tanımlayıcıları etkinleştiren -I seçeneği şu anda SQL Data Warehouse’a bağlanmak için gereklidir.

## 2. Sorgu

Bağlantının ardından desteklenen herhangi bir Transact-SQL deyimini örnekte yayımlayabilirsiniz.  Bu örnekte sorgular etkileşimli modda gönderilir.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Bu sonraki örnekler -Q seçeneği kullanarak veya SQL’i sqlcmd öğesine ekleyerek sorgularınızı toplu iş modunda nasıl çalıştırabileceğinizi gösterir.

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## Sonraki adımlar

Sqlcmd’de kullanılabilen seçenekler hakkında daha fazla bilgi için bkz. [sqlcmd belgeleri][sqlcmd].

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[sqlcmd]: https://msdn.microsoft.com/library/ms162773.aspx
[Azure Portal]: https://portal.azure.com

<!--Other Web references-->



<!--HONumber=ago16_HO5-->


