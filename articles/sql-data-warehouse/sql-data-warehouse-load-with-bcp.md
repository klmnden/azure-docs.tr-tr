<properties
   pageTitle="bsp yardımcı programını kullanarak SQL Data Warehouse'a veri yükleme | Microsoft Azure"
   description="bcp'nin ne olduğunu ve veri depolama senaryolarında nasıl kullanılacağını öğrenin."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="04/21/2016"
   ms.author="mausher;barbkess;sonyama"/>


# bcp ile veri yükleme

> [AZURE.SELECTOR]
- [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)
- [BCP](sql-data-warehouse-load-with-bcp.md)


**[bcp][]**; SQL Server, veri dosyaları ve SQL Data Warehouse arasında veri kopyalamanıza olanak sağlayan bir komut satırı toplu yükleme yardımcı programıdır. Çok sayıda satırı SQL Data Warehouse tablolarına aktarmak veya SQL Server tablolarından veri dosyalarına veri aktarmak için bcp yardımcı programını kullanın. queryout seçeneğiyle kullanılmadığı sürece bcp programını kullanmak için Transact-SQL ile ilgili herhangi bir bilginizin olması gerekmez.

bcp, nispeten küçük veri kümelerini SQL Data Warehouse veritabanına aktarmanın veya SQL Data Warehouse'dan dışarı aktarmanın hızlı ve kolay bir yoludur. bcp aracılığıyla yükleme/ayıklama işlemi gerçekleştirmek için önerilen tam veri miktarı, Azure veri merkezine yönelik ağ bağlantınıza göre değişir.  Genel olarak, boyut tabloları bcp aracılığıyla kolayca yüklenip ayıklanabilir ancak büyük miktarda veriyi yüklemek veya ayıklamak için bcp'nin kullanılması önerilmez.  SQL Data Warehouse'un yüksek düzeyde paralel işleme mimarisinden faydalanma konusunda daha başarılı olduğundan, büyük miktarlarda veri yükleme ve ayıklama işlemleri için Polybase aracının kullanılması önerilir.

bcp ile yapabilecekleriniz:

- Basit bir komut satırı yardımcı programını kullanarak SQL Data Warehouse'a veri yükleme.
- Basit bir komut satırı yardımcı programını kullanarak SQL Data Warehouse'dan veri ayıklama.

Bu öğreticide şunları nasıl yapacağınızı gösterilecek:

- bcp in komutunu kullanarak bir tabloya veri aktarma
- bcp out komutunu kullanarak bir tablodaki verileri dışarı aktarma

>[AZURE.VIDEO loading-data-into-azure-sql-data-warehouse-with-bcp]

## Önkoşullar

Bu öğreticide ilerleyebilmeniz için şunlar gereklidir:

- SQL Data Warehouse veritabanı
- bcp komut satırı yardımcı programının yüklü olması
- SQLCMD komut satırı yardımcı programının yüklü olması

>[AZURE.NOTE] bcp ve sqlcmd yardımcı programlarını [Microsoft İndirme Merkezi][]'nden indirebilirsiniz.

## SQL Data Warehouse'a veri aktarma

Bu öğreticide, Azure SQL Data Warehouse'da bir tablo oluşturup tabloya veri aktaracaksınız.

### 1. Adım: Azure SQL Data Warehouse'da tablo oluşturma

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

>[AZURE.NOTE] SQL Data Warehouse'da tablo oluşturma ve WITH yan tümcesinde bulunan seçenekler hakkında daha fazla bilgi için bkz. [Tablo Tasarımı][] veya [CREATE TABLE söz dizimi][].

### 2. Adım: Kaynak veri dosyası oluşturma

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

> [AZURE.NOTE] bcp.exe dosyasının UTF-8 dosya kodlamasını desteklemediğini unutmayın. bcp.exe dosyasını kullanırken lütfen ASCII dosyalarını veya UTF-16 kodlu dosyaları kullanın.

### 3. Adım: Bağlanma ve verileri içeri aktarma
bcp ile aşağıdaki komutu kullanıp değerleri uygun şekilde değiştirerek bağlanabilir ve verileri içeri aktarabilirsiniz.

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

sqlcmd ile aşağıdaki sorguyu çalıştırarak verilerin yüklendiğini doğrulayabilirsiniz:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Bu işlem sonrasında şu sonuçların döndürülmesi gerekir:

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

### 4. Adım: Yeni yüklenmiş verilerinize ilişkin İstatistikler oluşturma

Azure SQL Data Warehouse henüz istatistiklerin otomatik olarak oluşturulup güncelleştirilmesini desteklemiyor. Sorgularınızdan en iyi performansı elde edebilmeniz için ilk yüklemeden veya verilerdeki önemli değişikliklerden sonra her tablonun her sütununa ilişkin istatistiklerin oluşturulması önemlidir. İstatistikler hakkında ayrıntılı bir açıklama için Geliştirme ile ilgili konu başlığı grubunda yer alan [İstatistikler][] bölümüne göz atın. Aşağıda bu örnekte yüklenen tablolar için nasıl istatistik oluşturacağınıza yönelik kısa bir örnek verilmiştir.

Bir sqlcmd isteminden şu CREATE STATISTICS deyimlerini yürütün:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## SQL Data Warehouse'dan veri aktarma
Bu öğreticide SQL Data Warehouse'da bulunan bir tablodan veri dosyası oluşturacaksınız. Yukarıda oluşturduğumuz verileri DimDate2_export.txt adlı dosyaya aktaracağız.

### 1. Adım: Verileri dışarı aktarma

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

>[AZURE.NOTE] Dağıtılmış sistemlerin yapısı gereği, veri sırası SQL Data Warehouse veritabanlarında aynı olmayabilir. Tüm tabloyu dışarı aktarmak yerine, bcp yardımcı programının **queryout** işlevini kullanarak bir sorgu ayıklaması da yazabilirsiniz.

## Sonraki adımlar
Yüklemeye genel bakış için bkz. [SQL Data Warehouse'a veri yükleme][].
Geliştirme ile ilgili daha fazla ipucu için bkz. [SQL Data Warehouse geliştirmeye genel bakış][].

<!--Image references-->

<!--Article references-->

[SQL Data Warehouse'a veri yükleme]: sql-data-warehouse-overview-load.md
[SQL Data Warehouse geliştirmeye genel bakış]: sql-data-warehouse-overview-develop.md
[Tablo Tasarımı]: sql-data-warehouse-develop-table-design.md
[İstatistikler]: sql-data-warehouse-develop-statistics.md

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE söz dizimi]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft İndirme Merkezi]: https://www.microsoft.com/download/details.aspx?id=36433



<!----HONumber=Jun16_HO2-->


