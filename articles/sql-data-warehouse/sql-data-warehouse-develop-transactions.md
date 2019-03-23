---
title: Azure SQL veri ambarı'nda işlemleri kullanarak | Microsoft Docs
description: İşlem çözümleri geliştirme için Azure SQL veri ambarı'nda uygulama hakkında ipuçları.
services: sql-data-warehouse
author: ckarst
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 03/22/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 0b4ce6f4479552f42d32124149f64614b7e3cb70
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369505"
---
# <a name="using-transactions-in-sql-data-warehouse"></a>SQL veri ambarı'nda işlemleri kullanma
İşlem çözümleri geliştirme için Azure SQL veri ambarı'nda uygulama hakkında ipuçları.

## <a name="what-to-expect"></a>Sizi neler bekliyor
Beklediğiniz gibi SQL veri ambarı, veri ambarı iş yükünün parçası olarak işlemleri destekler. Ancak, SQL veri ambarı performans ölçekli olarak korunduğundan emin olmak için bazı özellikler SQL Server'a kıyasla sınırlıdır. Bu makalede farklar vurgulanmaktadır ve diğerleri listeler. 

## <a name="transaction-isolation-levels"></a>İşlem yalıtım düzeyleri
SQL veri ambarı, ACID işlemlerini uygular. Bununla birlikte, işlem desteği yalıtım düzeyini READ UNCOMMITTED sınırlıdır; Bu düzeyi değiştirilemez. READ UNCOMMITTED önemliyse, verilerin kirli okuma engellemek için yöntem kodlama sayısı uygulayabilirsiniz. En popüler yöntemleri, kullanıcıların hala hazırlanıyor verileri sorgulamasını engellemek için CTAS ve tablo (genellikle kayan pencere düzeni olarak bilinir) bölüm değiştirme kullanın. Verileri önceden filtre uygulayan görünümleri, ayrıca yaygın bir yaklaşım vardır.  

## <a name="transaction-size"></a>İşlem boyutu
Bir tek veri değişikliği işlem boyutu sınırlıdır. Dağıtım sınır uygulanır. Bu nedenle, toplam ayırma dağıtım sayısı sınırı çarpılarak hesaplanır. İçin yaklaşık işlemde satır sayısı dağıtım cap her satır toplam boyutu tarafından bölün. Değişken uzunluğu sütununa için en büyük boyut kullanılarak yerine ortalama sütununun uzunluğu alma göz önünde bulundurun.

Aşağıdaki tabloda aşağıdaki varsayımların yapılmıştır:

* Bir veri dağılımı oluştu 
* Ortalama satır uzunluğu 250 bayttır

## <a name="gen2"></a>Gen2

| [DWU](sql-data-warehouse-overview-what-is.md) | Cap dağıtım (GB) | Dağıtımların sayısı | En fazla işlem boyutu (GB) | Sayısı dağıtım başına satır | İşlem başına en fazla satır |
| --- | --- | --- | --- | --- | --- |
| DW100c |1 |60 |60 |4,000,000 |240,000,000 |
| DW200c |1,5 |60 |90 |6,000,000 |360,000,000 |
| DW300c |2.25 |60 |135 |9,000,000 |540,000,000 |
| DW400c |3 |60 |180 |12,000,000 |720,000,000 |
| DW500c |3.75 |60 |225 |15,000,000 |900,000,000 |
| DW1000c |7.5 |60 |450 |30,000,000 |1,800,000,000 |
| DW1500c |11.25 |60 |675 |45,000,000 |2,700,000,000 |
| DW2000c |15 |60 |900 |60,000,000 |3,600,000,000 |
| DW2500c |18.75 |60 |1125 |75,000,000 |4,500,000,000 |
| DW3000c |22.5 |60 |1,350 |90,000,000 |5,400,000,000 |
| DW5000c |37.5 |60 |2,250 |150,000,000 |9,000,000,000 |
| DW6000c |45 |60 |2,700 |180,000,000 |10,800,000,000 |
| DW7500c |56.25 |60 |3,375 |225,000,000 |13,500,000,000 |
| DW10000c |75 |60 |4,500 |300,000,000 |18,000,000,000 |
| DW15000c |112.5 |60 |6,750 |450,000,000 |27,000,000,000 |
| DW30000c |225 |60 |13,500 |900,000,000 |54,000,000,000 |

## <a name="gen1"></a>Gen1

| [DWU](sql-data-warehouse-overview-what-is.md) | Cap dağıtım (GB) | Dağıtımların sayısı | En fazla işlem boyutu (GB) | Sayısı dağıtım başına satır | İşlem başına en fazla satır |
| --- | --- | --- | --- | --- | --- |
| DW100 |1 |60 |60 |4,000,000 |240,000,000 |
| DW200 |1,5 |60 |90 |6,000,000 |360,000,000 |
| DW300 |2.25 |60 |135 |9,000,000 |540,000,000 |
| DW400 |3 |60 |180 |12,000,000 |720,000,000 |
| DW500 |3.75 |60 |225 |15,000,000 |900,000,000 |
| DW600 |4,5 |60 |270 |18,000,000 |1,080,000,000 |
| DW1000 |7.5 |60 |450 |30,000,000 |1,800,000,000 |
| DW1200 |9 |60 |540 |36,000,000 |2,160,000,000 |
| DW1500 |11.25 |60 |675 |45,000,000 |2,700,000,000 |
| DW2000 |15 |60 |900 |60,000,000 |3,600,000,000 |
| DW3000 |22.5 |60 |1,350 |90,000,000 |5,400,000,000 |
| DW6000 |45 |60 |2,700 |180,000,000 |10,800,000,000 |

İşlem boyutu sınırı, işlem veya işlem uygulanır. Tüm eşzamanlı işlemler arasında uygulanmaz. Bu nedenle her işlem, bu veri miktarı günlüğüne yazmak için izin verilir. 

En iyi duruma getirmek ve günlüğe yazılan veri miktarını en aza indirmek için lütfen bkz [işlemleri en iyi uygulamalar](sql-data-warehouse-develop-best-practices-transactions.md) makalesi.

> [!WARNING]
> En fazla işlem boyutu yalnızca KARMA gerçekleştirilebilir ve hatta ROUND_ROBIN dağıtılmış tablolar, verilerin bulunduğu. İşlem için dağıtımları dengesiz bir biçimde veri yazarken sınırı önce en fazla işlem boyutu erişilmesi olasılığı.
> <!--REPLICATED_TABLE-->
> 
> 

## <a name="transaction-state"></a>İşlem durumu
SQL veri ambarı XACT_STATE() işlevi -2 değerini kullanarak bir başarısız işlem bildirmek için kullanır. Bu değer, işlem başarısız oldu ve yalnızca geri alma için işaretlenmiş anlamına gelir.

> [!NOTE]
> -2 başarısız işlem belirtmek için XACT_STATE işlevi tarafından kullanımı, SQL Server için farklı bir davranış temsil eder. SQL Server yürütülemeyen bir işlem temsil etmek için -1 değerini kullanır. SQL Server bu yürütülemeyen'olarak işaretlenmiş gerek olmadan bir işlem içinde bazı hatalar dayanabilir. Örneğin `SELECT 1/0` hataya neden, ancak yürütülemeyen bir durum harekete zorunlu değildir. SQL Server yürütülemeyen bir işlem de okuma izin verir. Ancak, SQL veri ambarı bunu yapmanıza izin vermez. SQL veri ambarı işlem içinde bir hata oluşursa,-2 durumu otomatik olarak girer ve deyim geri kadar daha fazla select deyimleri yapmak mümkün olmayacaktır. Bu nedenle, uygulama kodunuz, XACT_STATE() olarak kullanıp kullanmadığını görmek için kod değişiklikleri yapmanız gerekebilir denetlemek önemlidir.
> 
> 

Örneğin, SQL Server'da aşağıdakine benzer bir işlem görebilirsiniz:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;

        IF @@TRANCOUNT > 0
        BEGIN
            ROLLBACK TRAN;
            PRINT 'ROLLBACK';
        END

    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Yukarıdaki kod, aşağıdaki hata iletisini sağlar:

Msg 111233, durum 1, 1 111233 satır düzeyi 16; Geçerli işlem iptal edildi ve tüm bekleyen değişiklikleri geri alındı. Neden: Salt geri alma durumunda bir işlem açıkça geri DDL, DML veya SELECT deyimi önce alınmadı.

ERROR_ * işlevlerin çıkış elde etmezsiniz.

SQL veri ambarı'nda kod biraz değiştirilmesi gerekir:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Beklenen davranış artık dikkate alınır. İşlem hata yönetilir ve beklendiği gibi değerler ERROR_ * işlevleri sağlar.

Değişmiş olan işlem geri alma ve CATCH bloğundaki hata bilgilerinin okuma önce gerçekleşmesi vardı.

## <a name="errorline-function"></a>Error_Line() işlevi
Ayrıca, SQL veri ambarı etmez uygulamak veya ERROR_LINE() işlevini destekler,'nı hatalarının ayıklanabileceğini belirtmekte yarar. Kodunuzda bu varsa, SQL veri ambarı ile uyumlu olması için kaldırmanız gerekir. Kodunuzda sorgu etiketleri, bunun yerine eşdeğer bir işlevselliği uygulamak için kullanın. Daha fazla ayrıntı için [etiket](sql-data-warehouse-develop-label.md) makalesi.

## <a name="using-throw-and-raiserror"></a>THROW ve RAISERROR kullanma
SQL veri ambarı'nda özel durumlarını oluşturma daha modern uygulama THROW olsa da RAISERROR de desteklenir. Dikkat edin ancak ödeme değer olan bazı farklar vardır.

* Kullanıcı tanımlı hata iletileri sayı THROW 100.000 150.000 aralığında olamaz
* RAISERROR hata iletileri 50.000 düzeltilen
* Çıktı kullanımı desteklenmiyor

## <a name="limitations"></a>Sınırlamalar
SQL veri ambarı işlemleri ile ilgili diğer birkaç kısıtlama yok.

Bunlar aşağıda belirtilmiştir:

* Hiçbir dağıtılmış işlemler
* İç içe işlem izin yoktur
* İzin verilen noktaları kaydetme
* Adlandırılmış bir işlem yoktur
* Hiçbir işaretli işlemleri
* Kullanıcı tanımlı bir işlem içinde gibi CREATE TABLE DDL desteği

## <a name="next-steps"></a>Sonraki adımlar
İşlemleri iyileştirme hakkında daha fazla bilgi için bkz: [işlemleri en iyi uygulamalar](sql-data-warehouse-develop-best-practices-transactions.md). Diğer SQL veri ambarı en iyi yöntemler hakkında bilgi edinmek için [SQL veri ambarı en iyi](sql-data-warehouse-best-practices.md).

