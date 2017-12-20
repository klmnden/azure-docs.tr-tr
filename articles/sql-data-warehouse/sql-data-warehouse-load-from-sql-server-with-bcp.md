---
title: Verileri SQL Server'dan Azure SQL Data warehouse'a (bcp) | Microsoft Docs
description: "Küçük boyutlu veriler söz konusu olduğunda SQL Server'dan düz dosyalara veri aktarmak ve verileri doğrudan Azure SQL Data Warehouse'a aktarmak için bcp yardımcı programını kullanın."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: f87d8d7c-8276-43c5-90e7-d4ccc0e3a224
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: dae7b5f7456f4ec0daf60d55f9c38b780896ff83
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-flat-files"></a>SQL Server'dan Azure SQL Data Warehouse'a veri yükleme (düz dosyalar)
> [!div class="op_single_selector"]
> * [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
> * [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
> * [bcp](sql-data-warehouse-load-from-sql-server-with-bcp.md)
> 
> 

Küçük veri kümeleri söz konusu olduğunda SQL Server'dan veri aktarmak ve doğrudan Azure SQL Data Warehouse'a yüklemek için bcp komut satırı yardımcı programını kullanabilirsiniz.

Bu öğreticide bcp'yi kullanarak şunları yapacaksınız:

* bcp out komutunu kullanarak SQL Server'daki bir tabloyu dışarı aktarma (veya basit bir örnek dosya oluşturma)
* Tabloyu bir düz dosyadan SQL Data Warehouse'a aktarın.
* Yüklenen verilere ilişkin istatistikler oluşturun.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-into-Azure-SQL-Data-Warehouse-with-BCP/player]
> 
> 

## <a name="before-you-begin"></a>Başlamadan önce
### <a name="prerequisites"></a>Önkoşullar
Bu öğreticide ilerleyebilmeniz için şunlar gereklidir:

* SQL Data Warehouse veritabanı
* bcp komut satırı yardımcı programının yüklü olması
* sqlcmd komut satırı yardımcı programının yüklü olması

bcp ve sqlcmd yardımcı programlarını [Microsoft İndirme Merkezi][Microsoft Download Center]'nden indirebilirsiniz.

### <a name="data-in-ascii-or-utf-16-format"></a>ASCII veya UTF-16 biçimindeki veriler
UTF-8 biçimi bcp tarafından desteklenmediğinden, bu öğreticiyi kendi verilerinizle deniyorsanız verilerinizin ASCII veya UTF-16 kodlamasını kullanıyor olması gerekir. 

PolyBase UTF-8'i destekler ancak henüz UTF-16'yi desteklemiyor. bcp ve PolyBase'i birleştirmek istiyorsanız verileri SQL Server'dan dışarı aktardıktan sonra UTF-8 biçimine dönüştürmeniz gerektiğini unutmayın. 

## <a name="1-create-a-destination-table"></a>1. Hedef tablo oluşturma
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
    WITH
    (
        CLUSTERED COLUMNSTORE INDEX,
        DISTRIBUTION = ROUND_ROBIN
    );
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

## <a name="4-create-statistics"></a>4. İstatistik oluşturma
SQL Data Warehouse henüz istatistiklerin otomatik olarak oluşturulup güncelleştirilmesini desteklemiyor. Sorgularınızdan en iyi performansı elde edebilmeniz için ilk yüklemeden veya verilerdeki önemli değişikliklerden sonra istatistiklerin tüm sütunlarda oluşturulması önemlidir. İstatistiklerin ayrıntılı bir açıklaması için bkz [istatistikleri][Statistics]. 

Yeni yüklenen tablonuzda istatistik oluşturmak için aşağıdaki komutu çalıştırın.

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="5-export-data-from-sql-data-warehouse"></a>5. SQL Data Warehouse'dan veri aktarma
Alıştırma yapmak için biraz önce SQL Data Warehouse'dan yüklediğiniz verileri tekrar dışarı aktarabilirsiniz.  Dışarı aktarma komutu, SQL Server'dan veri aktarma komutuyla tamamen aynıdır.

Ancak sonuçlarda bir fark görürsünüz. Veriler SQL Data Warehouse'da dağıtılmış konumlarda depolandığı için, dışarı veri aktardığınızda her bir İşlem düğümü kendi verisini çıkış dosyasına yazar. Çıkış dosyasındaki veri sırası, giriş dosyasındaki veri sırasından farklı olabilir.

### <a name="export-a-table-and-compare-exported-results"></a>Bir tabloyu dışarı aktarma ve dışarı aktarılan sonuçları karşılaştırma
Dışarı aktarılan verileri görmek için bir komut istemi açın ve kendi parametrelerinizi kullanarak bu komutu çalıştırın. ServerName, Azure mantıksal SQL Server'ınızın adıdır.

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Yeni dosyayı açarak verilerin düzgün bir şekilde dışarı aktarıldığını doğrulayabilirsiniz. Dosyadaki veriler aşağıdaki metinle eşleşmelidir ancak sıraları farklı olabilir:

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

### <a name="export-the-results-of-a-query"></a>Bir sorgunun sonuçlarını dışarı aktarma
Tüm tabloyu dışarı aktarmak yerine, bcp yardımcı programının **queryout** işlevini kullanarak bir sorgunun sonuçlarını dışarı aktarabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
Yüklemeye genel bakış için bkz. [SQL Veri Ambarı'na veri yükleme][Load data into SQL Data Warehouse].
Geliştirme ile ilgili daha fazla ipucu için bkz. [SQL Veri Ambarı’nda geliştirmeye genel bakış][SQL Data Warehouse development overview].
Bkz: [tablo genel bakışı] [ Table Overview] veya [CREATE TABLE söz dizimi] [ CREATE TABLE syntax] SQL Data Warehouse'da tablo oluşturma hakkında daha fazla bilgi.

<!--Image references-->

<!--Article references-->

[Load data into SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[Table Overview]: ./sql-data-warehouse-tables-overview.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
