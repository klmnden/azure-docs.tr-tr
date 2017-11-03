---
title: "SQL Data Warehouse'a örnek veri yükleme | Microsoft Docs"
description: "SQL Data Warehouse'a örnek veri yükleme"
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: e338ecf8-cfee-419b-b7b6-98108d381c62
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 1e0df958a2f18fe1e988168918e5cfd293f84e64
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="load-sample-data-into-sql-data-warehouse"></a>SQL Data Warehouse'a örnek veri yükleme
Yük ve Adventure Works örnek veritabanını sorgulamak için basit adımları izleyin. Bu komut dosyalarını sqlcmd tabloları ve görünümleri oluşturacak olan SQL çalıştırmak için ilk olarak kullanın. Tabloları oluşturduktan sonra komut dosyaları veri yüklemek için bcp kullanır.  Sqlcmd ve yüklü bcp zaten sahip değilseniz, bu bağlantıları izleyin [bcp yüklemek] [ install bcp] ve [sqlcmd yüklemek][install sqlcmd].

## <a name="load-sample-data"></a>Örnek verileri yükleme
1. Karşıdan [SQL Data Warehouse için Adventure Works örnek komut dosyaları] [ Adventure Works Sample Scripts for SQL Data Warehouse] zip dosyası.
2. Dosyaları indirilen ZIP yerel makinenizde bir dizine ayıklayın.
3. Ayıklanan dosya aw_create.bat düzenleyin ve dosyanın üst kısmında bulunan aşağıdaki değişkenleri ayarlayın.  Hiçbir boşluk arasında ayırdığınızdan emin olun "=" ve parametresi.  Düzenlemeleriniz nasıl görünebileceği örnekleri aşağıda verilmiştir.
   
    ```
    server=mylogicalserver.database.windows.net
    user=mydwuser
    password=Mydwpassw0rd
    database=mydwdatabase
    ```
4. Bir Windows komut isteminden düzenlenen aw_create.bat çalıştırın.  Düzenlenen aw_create.bat sürümünüz kaydettiğiniz dizininde olduğundan emin olun.
   Bu komut dosyası olacak...
   
   * Adventure Works tablolar veya veritabanınızda zaten görünümleri bırakın
   * Adventure Works tablolar ve görünümler oluşturma
   * BCP kullanarak her Adventure Works tablosu yükleme
   * Her Adventure Works tablo satır sayılarını doğrula
   * Her sütun için her Adventure Works tablo istatistikleri Topla

## <a name="query-sample-data"></a>Örnek verileri Sorgulama
SQL Data Warehouse'a bazı örnek veriler yüklenen sonra birkaç sorgu hızlı bir şekilde çalıştırabilirsiniz.  Bir sorguyu çalıştırmak için yeni oluşturulan Adventure Works veritabanınızda Visual Studio ve SSDT, kullanarak Azure SQL DW açıklandığı gibi bağlanmak [sorgu Visual Studio ile] [ query with Visual Studio] belge.

Çalışanların tüm bilgileri almak için basit select deyimi örneği:

```sql
SELECT * FROM DimEmployee;
```

Her gün tüm satış toplam miktarı bakmak için GROUP BY gibi yapıları kullanarak daha karmaşık bir sorgu örneği:

```sql
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

Belirli bir tarihten önce siparişleri filtrelemek için bir WHERE yan tümcesi ile bir SELECT örneği:

```
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
WHERE OrderDateKey > '20020801'
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

SQL veri ambarı SQL Server destekleyen neredeyse tüm T-SQL yapılarını destekler.  Farkları belgelenmiştir bizim [kodunu taşıma] [ migrate code] belgeleri.

## <a name="next-steps"></a>Sonraki adımlar
Nasıl yapılır, bazı sorgular örnek verilerle denemek için ettikten, kullanıma [geliştirmek][develop], [yük][load], veya [ geçiş] [ migrate] SQL veri ambarı.

<!--Image references-->

<!--Article references-->
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[query with Visual Studio]: sql-data-warehouse-query-visual-studio.md
[migrate code]: sql-data-warehouse-migrate-code.md
[install bcp]: sql-data-warehouse-load-with-bcp.md
[install sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other Web references-->
[Adventure Works Sample Scripts for SQL Data Warehouse]: https://migrhoststorage.blob.core.windows.net/sqldwsample/AdventureWorksSQLDW2012.zip
