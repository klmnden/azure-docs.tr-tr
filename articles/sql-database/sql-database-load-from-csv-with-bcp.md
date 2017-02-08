---
title: "CSV dosyasından Azure SQL Veritabanı’na veri yükleme (bcp) | Microsoft Belgeleri"
description: "Küçük veri boyutları için Azure SQL Veritabanına veri aktarırken bcp kullanır."
services: sql-database
documentationcenter: NA
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 875f9b8d-f1a1-4895-b717-f45570fb7f80
ms.service: sql-database
ms.custom: migrate and move
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 01/10/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 36748312506a08ed7932a6bb355a5dc7393bb002
ms.openlocfilehash: 5e15f8a0ebb8cab5dce5b3c1cf6b62dee362a8d0


---
# <a name="load-data-from-csv-into-azure-sql-database-flat-files"></a>CSV dosyasından Azure SQL Veritabanı'na veri yükleme (düz dosyalar)
Bir CSV dosyasından Azure SQL Database’e veri aktarmak için bcp komut satırı yardımcı programını kullanabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
### <a name="prerequisites"></a>Önkoşullar
Bu öğreticide ilerleyebilmeniz için şunlar gereklidir:

* Azure SQL Database mantıksal sunucusu ve veritabanı
* bcp komut satırı yardımcı programının yüklü olması
* sqlcmd komut satırı yardımcı programının yüklü olması

bcp ve sqlcmd yardımcı programlarını [Microsoft İndirme Merkezi][Microsoft Download Center]'nden indirebilirsiniz.

### <a name="data-in-ascii-or-utf-16-format"></a>ASCII veya UTF-16 biçimindeki veriler
UTF-8 biçimi bcp tarafından desteklenmediğinden, bu öğreticiyi kendi verilerinizle deniyorsanız verilerinizin ASCII veya UTF-16 kodlamasını kullanıyor olması gerekir. 

## <a name="1-create-a-destination-table"></a>1. Hedef tablo oluşturma
SQL Veritabanı'nda bir tabloyu hedef tablo olarak tanımlayın. Tablodaki sütunlar, veri dosyanızın tüm satırlarındaki verilere karşılık gelmelidir.

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


## <a name="2-create-a-source-data-file"></a>2. Kaynak veri dosyası oluşturma
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

## <a name="3-load-the-data"></a>3. Verileri yükleme
Verileri yüklemek için bir komut satırı açın; Sunucu Adı, Veritabanı Adı, Kullanıcı Adı ve Parola alanlarına kendi bilgilerinizi yazarak aşağıdaki komutu çalıştırın.

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ','
```

Bu komutu kullanarak verilerin düzgün şekilde yüklendiğini doğrulayın

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Sonuçlar şu şekilde görünmelidir:

| DateId | CalendarQuarter | FiscalQuarter |
| --- | --- | --- |
| 20150101 |1 |3 |
| 20150201 |1 |3 |
| 20150301 |1 |3 |
| 20150401 |2 |4 |
| 20150501 |2 |4 |
| 20150601 |2 |4 |
| 20150701 |3 |1 |
| 20150801 |3 |1 |
| 20150801 |3 |1 |
| 20151001 |4 |2 |
| 20151101 |4 |2 |
| 20151201 |4 |2 |

## <a name="next-steps"></a>Sonraki adımlar
Bir SQL Server veritabanına geçiş yapmak için bkz. [SQL Server veritabanı geçişi](sql-database-cloud-migrate.md).

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433



<!--HONumber=Jan17_HO4-->


