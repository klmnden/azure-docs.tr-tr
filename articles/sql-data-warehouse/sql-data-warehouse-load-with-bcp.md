---
title: "bcp yardımcı programını kullanarak SQL Veri Ambarı'na veri yükleme | Microsoft Belgeleri"
description: "bcp'nin ne olduğunu ve veri depolama senaryolarında nasıl kullanılacağını öğrenin."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: f9467d11-fcd6-4131-a65a-2022d2c32d24
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 01/22/2018
ms.author: cakarst;barbkess
ms.openlocfilehash: 55211e29149cd334421bd8723d47278a19afbfbb
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="load-data-with-bcp"></a>bcp ile veri yükleme

**[bcp](/sql/tools/bcp-utility.md)**; SQL Server, veri dosyaları ve SQL Data Warehouse arasında veri kopyalamanıza olanak sağlayan bir komut satırı toplu yükleme yardımcı programıdır. Çok sayıda satırı SQL Data Warehouse tablolarına aktarmak veya SQL Server tablolarından veri dosyalarına veri aktarmak için bcp yardımcı programını kullanın. queryout seçeneğiyle kullanılmadığı sürece bcp programını kullanmak için Transact-SQL ile ilgili herhangi bir bilginizin olması gerekmez.

bcp, nispeten küçük veri kümelerini SQL Data Warehouse veritabanına aktarmanın veya SQL Data Warehouse'dan dışarı aktarmanın hızlı ve kolay bir yoludur. bcp aracılığıyla yükleme/ayıklama işlemi gerçekleştirmek için önerilen tam veri miktarı, Azure’a yönelik ağ bağlantınıza göre değişir.  Küçük boyutlu tablolar, bcp ile kolayca yüklenebilir ve ayıklanabilir. Ancak, büyük hacimli verileri yükleme ve ayıklama işlemleri için önerilen araç PolyBase’tir (bcp değil). PolyBase, SQL Veri Ambarı’nın büyük ölçüde paralel işleme mimarisi için tasarlanmıştır.

bcp ile yapabilecekleriniz:

* Bir komut satırı yardımcı programını kullanarak SQL Veri Ambarı'na veri yükleyin.
* Bir komut satırı yardımcı programını kullanarak SQL Veri Ambarı’ndan veri ayıklayın.

Bu öğreticide:

* Komut içi bcp kullanılarak bir tabloya veri aktarılır
* Komut içi bcp kullanarak bir tablodan veri aktarılır

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-into-Azure-SQL-Data-Warehouse-with-BCP/player]
> 
> 

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticide ilerleyebilmeniz için şunlar gereklidir:

* SQL Data Warehouse veritabanı
* bcp ve sqlcmd komut satırı yardımcı programıyla çalışılır. Bu programları [Microsoft Yükleme Merkezi](https://www.microsoft.com/download/details.aspx?id=36433)'nden indirebilirsiniz. 

### <a name="data-in-ascii-or-utf-16-format"></a>ASCII veya UTF-16 biçimindeki veriler
UTF-8 biçimi bcp tarafından desteklenmediğinden, bu öğreticiyi kendi verilerinizle deniyorsanız verilerinizin ASCII veya UTF-16 kodlamasını kullanıyor olması gerekir. 

PolyBase UTF-8'i destekler ancak henüz UTF-16'yi desteklemiyor. Veri aktarma için bcp’yi ve sonra veri yükleme için PolyBase’i kullandıktan sonra, SQL Server’dan aktarılan verileri UTF-8 biçimine dönüştürmeniz gerekir. 

## <a name="import-data-into-sql-data-warehouse"></a>SQL Data Warehouse'a veri aktarma
Bu öğreticide, Azure SQL Data Warehouse'da bir tablo oluşturup tabloya veri aktaracaksınız.

### <a name="step-1-create-a-table-in-azure-sql-data-warehouse"></a>1. Adım: Azure SQL Data Warehouse'da tablo oluşturma
Örneğinizde bir tablo oluşturmak için aşağıdaki sorguyu çalıştırmak üzere bir komut isteminden sqlcmd yardımcı programını kullanın:

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

Tablo oluşturma hakkında daha fazla bilgi için bkz. [Tabloya Genel Bakış](sql-data-warehouse-tables-overview.md) veya [CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse.md) söz dizimi.
 

### <a name="step-2-create-a-source-data-file"></a>2. Adım: Kaynak veri dosyası oluşturma
Not Defteri'ni açın ve yeni bir metin dosyasına aşağıdaki veri satırlarını kopyalayıp dosyayı yerel geçici dizininize (C:\Temp\DimDate2.txt) kaydedin.

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

> [!NOTE]
> bcp.exe dosyasının UTF-8 dosya kodlamasını desteklemediğini unutmayın. bcp.exe dosyasını kullanırken ASCII dosyalarını veya UTF-16 kodlu dosyaları kullanın.
> 
> 

### <a name="step-3-connect-and-import-the-data"></a>3. Adım: Bağlanma ve verileri içeri aktarma
bcp ile aşağıdaki komutu kullanıp değerleri uygun şekilde değiştirerek bağlanabilir ve verileri içeri aktarabilirsiniz.

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

sqlcmd ile aşağıdaki sorguyu çalıştırarak verilerin yüklendiğini doğrulayabilirsiniz:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Bu sorgu sonrasında şu sonuçların döndürülmesi gerekir:

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

### <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>4. Adım: Yeni yüklenmiş verilerinize ilişkin İstatistikler oluşturma
Veriler yüklendikten sonra, son adım istatistikleri oluşturmak veya güncelleştirmektir. Bu, sorgu performansını artırmaya yardımcı olur. Daha fazla bilgi için bkz. [İstatistikler](sql-data-warehouse-tables-statistics.md). Aşağıdaki sqlcmd örneği, istatistikleri yeni yüklenen verileri içeren tabloda oluşturur.


```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a>SQL Data Warehouse'dan veri aktarma
Bu öğreticide SQL Veri Ambarı'nda bulunan bir tablodan veri dosyası oluşturulmaktadır. Bu dosya, önceki bölümde içeri aktardığınız verileri dışarı aktarır. Sonuçlar, DimDate2_export.txt adlı bir dosyaya kaydedilir.

### <a name="step-1-export-the-data"></a>1. Adım: Verileri dışarı aktarma
bcp yardımcı programı ile aşağıdaki komutu kullanıp değerleri uygun şekilde değiştirerek bağlanabilir ve verileri içeri aktarabilirsiniz.

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Yeni dosyayı açarak verilerin düzgün bir şekilde dışarı aktarıldığını doğrulayabilirsiniz. Dosyadaki verilerin aşağıdaki metinle eşleşmesi gerekir:

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

> [!NOTE]
> Dağıtılmış sistemlerin yapısı gereği, veri sırası SQL Data Warehouse veritabanlarında aynı olmayabilir. Tüm tabloyu dışarı aktarmak yerine, bcp yardımcı programının **queryout** işlevini kullanarak bir sorgu ayıklaması da yazabilirsiniz.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Yükleme işleminizi tasarlamak için bkz. [Genel bakışı yükleme](sql-data-warehouse-design-elt-data-loading].  



<!--MSDN references-->



<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
