<properties
   pageTitle="CSV dosyasından Azure SQL Veritabanına veri yükleme (bcp) | Microsoft Azure"
   description="Küçük veri boyutları için Azure SQL Veritabanına veri aktarırken bcp kullanır."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="carlrab"/>


# CSV dosyasından Azure SQL Data Warehouse'a veri yükleme (düz dosyalar)

Bir CSV dosyasından Azure SQL Database’e veri aktarmak için bcp komut satırı yardımcı programını kullanabilirsiniz.

## Başlamadan önce

### Önkoşullar

Bu öğreticide ilerleyebilmeniz için şunlar gereklidir:

- Azure SQL Database mantıksal sunucusu ve veritabanı
- bcp komut satırı yardımcı programının yüklü olması
- sqlcmd komut satırı yardımcı programının yüklü olması

bcp ve sqlcmd yardımcı programlarını [Microsoft İndirme Merkezi][]'nden indirebilirsiniz.

### ASCII veya UTF-16 biçimindeki veriler

UTF-8 biçimi bcp tarafından desteklenmediğinden, bu öğreticiyi kendi verilerinizle deniyorsanız verilerinizin ASCII veya UTF-16 kodlamasını kullanıyor olması gerekir. 

PolyBase UTF-8'i destekler ancak henüz UTF-16'yi desteklemiyor. bcp ve PolyBase'i birleştirmek istiyorsanız verileri SQL Server'dan dışarı aktardıktan sonra UTF-8 biçimine dönüştürmeniz gerektiğini unutmayın. 


## 1. Hedef tablo oluşturma

SQL Data Warehouse'da daha sonra yükleme için hedef tablo olacak bir tablo tanımlayın. Tablodaki sütunlar, veri dosyanızın tüm satırlarındaki verilere karşılık gelmelidir.

Tablo oluşturmak için bir komut istemi açın ve sqlcmd.exe dosyasını kullanarak şu komutu çalıştırın:


```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    ;
"
```


## 2. Kaynak veri dosyası oluşturma

Not Defteri'ni açın ve yeni bir metin dosyasına aşağıdaki veri satırlarını kopyalayıp dosyayı yerel geçici dizininize (C:\Temp\DimDate2.txt) kaydedin. Bu veri ASCII biçimindedir.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

(İsteğe bağlı) SQL Server veritabanından kendi verilerinizi dışarı aktarmak için bir komut istemi açın ve aşağıdaki komutu çalıştırın. TableName (Tablo Adı), ServerName (Sunucu Adı), DatabaseName (Veritabanı Adı), Username (Kullanıcı Adı) ve Password (Parola) alanlarına kendi bilgilerinizi yazın.

```sql
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t ','
```

## 3. Verileri yükleme
Verileri yüklemek için bir komut satırı açın; Sunucu Adı, Veritabanı Adı, Kullanıcı Adı ve Parola alanlarına kendi bilgilerinizi yazarak aşağıdaki komutu çalıştırın.

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ','
```

Bu komutu kullanarak verilerin düzgün şekilde yüklendiğini doğrulayın

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Sonuçlar şu şekilde görünmelidir:

DateId |CalendarQuarter |FiscalQuarter
----------- |--------------- |-------------
20150101 |1 |3
20150201 |1 |3
20150301 |1 |3
20150401 |2 |4
20150501 |2 |4
20150601 |2 |4
20150701 |3 |1
20150801 |3 |1
20150801 |3 |1
20151001 |4 |2
20151101 |4 |2
20151201 |4 |2


## Sonraki adımlar

Bir SQL Server veritabanına geçiş yapmak için bkz. [SQL Server veritabanı geçişi](sql-database-cloud-migrate.md).

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE söz dizimi]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft İndirme Merkezi]: https://www.microsoft.com/download/details.aspx?id=36433



<!--HONumber=Aug16_HO1-->


