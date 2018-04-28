---
title: Azure SQL Data Warehouse'da işlemleri kullanma | Microsoft Docs
description: Çözümleri geliştirme için Azure SQL Data Warehouse'da işlemleri uygulamak için ipuçları.
services: sql-data-warehouse
author: ckarst
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: 7fa3d19cc0fca81616969773a40c3d3dbccc4a26
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="using-transactions-in-sql-data-warehouse"></a>SQL veri ambarı'nda işlemleri kullanma
Çözümleri geliştirme için Azure SQL Data Warehouse'da işlemleri uygulamak için ipuçları.

## <a name="what-to-expect"></a>Sizi neler bekliyor
Beklediğiniz gibi SQL veri ambarı veri ambarı iş yükü bir parçası olarak işlemleri destekler. Ancak, SQL veri ambarı performansını ölçekte korunduğundan emin olmak için bazı özellikler SQL Server'a karşılaştırıldığında sınırlıdır. Bu makalede farklar vurgular ve diğerleri listeler. 

## <a name="transaction-isolation-levels"></a>İşlem yalıtım düzeyi
SQL veri ambarı ACID işlemlerini uygular. Ancak, işlem desteği yalıtım düzeyini READ UNCOMMITTED sınırlıdır; Bu düzey değiştirilemez. READ UNCOMMITTED önemliyse, verilerin kirli okuma engellemek için yöntem kodlama sayısı uygulayabilirsiniz. En popüler yöntemleri, kullanıcıların hala hazırlanan veri sorgulama önlemek için hem CTAS hem de (genellikle kayan pencere düzeni bilinir) tabloda bölüm değiştirme kullanın. Verileri önceden filtre görünümleri de popüler bir yaklaşım vardır.  

## <a name="transaction-size"></a>İşlem boyutu
Bir tek veri değişikliği işlem boyutu sınırlıdır. Sınır dağıtım uygulanır. Bu nedenle, toplam ayırma dağıtım sayısı sınırı çarpılmasıyla hesaplanır. İçin yaklaşık işlemde satır sayısının üst sınırını dağıtım ucun her satır toplam boyutu tarafından bölün. Değişken uzunlukta sütunlar için en büyük boyutu kullanmak yerine bir ortalama sütun uzunluğu alma göz önünde bulundurun.

Aşağıdaki varsayımlar aşağıdaki tabloda yapılmıştır:

* Bir dağılmış verilerinizin oluştu 
* Ortalama satır uzunluğu 250 bayttır

| [DWU](sql-data-warehouse-overview-what-is.md) | Dağıtım (GiB) cap | Dağıtımların sayısı | En fazla işlem boyutu (GiB) | # Dağıtım başına satır | İşlem başına en fazla satır |
| --- | --- | --- | --- | --- | --- |
| DW100 |1 |60 |60 |4,000,000 |240,000,000 |
| DW200 |1,5 |60 |90 |6,000,000 |360,000,000 |
| DW300 |2.25 |60 |135 |9,000,000 |540,000,000 |
| DW400 |3 |60 |180 |12,000,000 |720,000,000 |
| DW500 |3.75 |60 |225 |15,000,000 |900,000,000 |
| DW600 |4.5 |60 |270 |18,000,000 |1,080,000,000 |
| DW1000 |7.5 |60 |450 |30,000,000 |1,800,000,000 |
| DW1200 |9 |60 |540 |36,000,000 |2,160,000,000 |
| DW1500 |11.25 |60 |675 |45,000,000 |2,700,000,000 |
| DW2000 |15 |60 |900 |60,000,000 |3,600,000,000 |
| DW3000 |22.5 |60 |1,350 |90,000,000 |5,400,000,000 |
| DW6000 |45 |60 |2,700 |180,000,000 |10,800,000,000 |

İşlem boyut sınırı, işlem veya işlem uygulanır. Tüm eşzamanlı işlemler arasında uygulanmaz. Bu nedenle her bir işlem günlüğüne bu miktarda veri yazmak için izin verilir. 

En iyi duruma getirme ve günlüğe yazılan veri miktarını en aza indirmek için lütfen [işlemleri en iyi uygulamalar](sql-data-warehouse-develop-best-practices-transactions.md) makalesi.

> [!WARNING]
> Maksimum hareket boyutu yalnızca karma değeri sağlanabilir ve hatta dağıtılmış ROUND_ROBIN tabloları, verilerin bulunduğu. İşlem dağıtımlarına çarpık bir şekilde veri yazma, ardından sınırı önce maksimum hareket boyutu erişilmesi olasılığı yüksektir.
> <!--REPLICATED_TABLE-->
> 
> 

## <a name="transaction-state"></a>İşlem durumu
SQL veri ambarı XACT_STATE() işlevi -2 değerini kullanarak bir başarısız işlem raporlamak için kullanır. Bu değer, işlem başarısız oldu ve yalnızca geri almak için işaretlenmiş anlamına gelir.

> [!NOTE]
> -2 başarısız işlem belirtmek için XACT_STATE işlevi tarafından kullanımını farklı bir davranışı SQL Server'a temsil eder. SQL Server -1 değeri yürütülemeyen bir işlem göstermek için kullanır. SQL Server bu olarak yürütülemeyen işaretlenmesi gerek olmadan bir işlemin içindeki bazı hatalar dayanabilir. Örneğin `SELECT 1/0` hataya neden, ancak yürütülemeyen bir durum harekete zorla sağlamaz. SQL Server yürütülemeyen işlemde de okuma izin verir. Ancak, SQL Data Warehouse, bunun izin vermez. Bir SQL Data Warehouse işlem içinde bir hata oluşursa,-2 durumu otomatik olarak girer ve deyim geri kadar daha fazla select deyimi yapmak mümkün olmaz. Bu nedenle, onu XACT_STATE() gibi kullanıp kullanmadığını görmek için uygulama kodunuzda bir kod değişikliği yapmanız gerekebilir denetlemek önemlidir.
> 
> 

Örneğin, SQL Server'da, aşağıdakine benzer bir işlem görebilirsiniz:

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
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Önceki kod, aşağıdaki hata iletisini sağlar:

Msg 111233, Level 16, State 1, satır 1 111233; Geçerli işlem iptal edildi ve bekleyen değişiklikleri geri alındı. Neden: Bir işlemi yalnızca geri alma durumunda açıkça DDL, DML veya SELECT deyimi önce geri alınmadı.

ERROR_ * işlevleri çıktısını alamazsınız.

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

Şimdi beklenen bir davranış gözlenir. İşlem hata yönetilir ve beklendiği gibi ERROR_ * işlevleri değerler sağlayın.

Değişen tüm olduğundan işlem geri CATCH bloğu içinde hata bilgilerinin okuma önce gerçekleşir gerekiyordu.

## <a name="errorline-function"></a>Error_Line() işlevi
Ayrıca, SQL Data Warehouse uygulama ya da ERROR_LINE() işlevini destekler, dikkate değerdir. Bu, kodunuzda varsa, SQL Data Warehouse ile uyumlu olması için kaldırmanız gerekir. Sorgu etiketleri, kodunuzda eşdeğer işlevsellik uygulamak için bunun yerine kullanın. Daha fazla ayrıntı için bkz: [etiket](sql-data-warehouse-develop-label.md) makalesi.

## <a name="using-throw-and-raiserror"></a>THROW ve RAISERROR kullanma
SQL Data warehouse'da özel durumlarını oluşturma için daha modern uygulama THROW olmakla birlikte RAISERROR da desteklenir. Dikkat edilmesi ancak ödeme değer olan bazı farklar vardır.

* Kullanıcı tanımlı hata iletilerinin numaraları THROW 100.000 150.000 aralığının olamaz.
* RAISERROR hata iletileri 50.000 düzeltilen
* Sistem iletilerinde kullanımı desteklenmiyor

## <a name="limitations"></a>Sınırlamalar
SQL veri ambarı için işlemleri ile ilgili diğer birkaç kısıtlamalar sahip.

Bunlar aşağıdaki gibidir:

* Dağıtılmış işlem
* İzin verilen hiçbir iç içe geçmiş işlemler
* İzin verilen noktaları kaydetme
* Adlandırılmış işlem
* İşaretli işlem
* Kullanıcı tanımlı bir işlemin içindeki CREATE TABLE gibi DDL desteği

## <a name="next-steps"></a>Sonraki adımlar
İşlemler en iyi duruma getirme hakkında daha fazla bilgi için bkz: [işlemleri en iyi uygulamalar](sql-data-warehouse-develop-best-practices-transactions.md). Diğer SQL Data Warehouse en iyi uygulamalar hakkında bilgi edinmek için [SQL veri ambarı en iyi yöntemler](sql-data-warehouse-best-practices.md).

