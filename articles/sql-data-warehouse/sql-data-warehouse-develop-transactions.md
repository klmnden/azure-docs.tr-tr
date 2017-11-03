---
title: "SQL veri ambarı işlemlerinde | Microsoft Docs"
description: "Çözümleri geliştirme için Azure SQL Data Warehouse'da işlemleri uygulamak için ipuçları."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: ae621788-e575-41f5-8bfe-fa04dc4b0b53
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 29d53e18539f2c24dd64090b2ac6f9dd4c783961
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="transactions-in-sql-data-warehouse"></a>SQL veri ambarı işlemleri
Beklediğiniz gibi SQL veri ambarı veri ambarı iş yükü bir parçası olarak işlemleri destekler. Ancak, SQL veri ambarı performansını ölçekte korunduğundan emin olmak için bazı özellikler SQL Server'a karşılaştırıldığında sınırlıdır. Bu makalede farklar vurgular ve diğerleri listeler. 

## <a name="transaction-isolation-levels"></a>İşlem yalıtım düzeyi
SQL veri ambarı ACID işlemlerini uygular. Ancak, işlem desteği yalıtım sınırlıdır `READ UNCOMMITTED` ve bu değiştirilemez. Bu sizin için önemliyse verilerin kirli okuma engellemek için yöntem kodlama sayısı uygulayabilirsiniz. En yaygın yöntem, kullanıcıların hala hazırlanan veri sorgulama önlemek için hem CTAS hem de (genellikle kayan pencere düzeni bilinir) tabloda bölüm değiştirme yararlanın. Filtre öncesi veri görünümleri popüler bir yaklaşım da olur.  

## <a name="transaction-size"></a>İşlem boyutu
Bir tek veri değişikliği işlem boyutu sınırlıdır. Sınır Bugün "dağıtım" uygulanır. Bu nedenle, toplam ayırma dağıtım sayısı sınırı çarpılmasıyla hesaplanır. İçin yaklaşık işlemde satır sayısının üst sınırını dağıtım ucun her satır toplam boyutu tarafından bölün. Değişken uzunlukta sütunlar için en büyük boyutu kullanmak yerine bir ortalama sütun uzunluğu alma göz önünde bulundurun.

Aşağıdaki varsayımlar aşağıdaki tabloda yapılmıştır:

* Bir dağılmış verilerinizin oluştu 
* Ortalama satır uzunluğu 250 bayttır

| [DWU][DWU] | Dağıtım (GiB) cap | Dağıtımların sayısı | En fazla işlem boyutu (GiB) | # Dağıtım başına satır | İşlem başına en fazla satır |
| --- | --- | --- | --- | --- | --- |
| DW100 |1 |60 |60 |4,000,000 |240,000,000 |
| DW200 |1.5 |60 |90 |6,000,000 |360,000,000 |
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

En iyi duruma getirme ve günlüğe yazılan veri miktarını en aza indirmek için lütfen [işlemleri en iyi uygulamalar] [ Transactions best practices] makalesi.

> [!WARNING]
> Maksimum hareket boyutu yalnızca karma değeri sağlanabilir ve hatta dağıtılmış ROUND_ROBIN tabloları, verilerin bulunduğu. İşlem dağıtımlarına çarpık bir şekilde veri yazma, ardından sınırı önce maksimum hareket boyutu erişilmesi olasılığı yüksektir.
> <!--REPLICATED_TABLE-->
> 
> 

## <a name="transaction-state"></a>İşlem durumu
SQL veri ambarı XACT_STATE() işlevi -2 değerini kullanarak bir başarısız işlem raporlamak için kullanır. Bu işlem başarısız oldu ve yalnızca geri almak için işaretlenmiş anlamına gelir

> [!NOTE]
> -2 başarısız işlem belirtmek için XACT_STATE işlevi tarafından kullanımını farklı bir davranışı SQL Server'a temsil eder. SQL Server -1 değeri kaydedilemez bir işlem göstermek için kullanır. SQL Server bu olarak kaydedilemez işaretlenmesi gerek olmadan bir işlemin içindeki bazı hatalar dayanabilir. Örneğin `SELECT 1/0` hataya neden, ancak bir işlem kaydedilemez bir duruma zorla sağlamaz. SQL Server kaydedilemez işlemde de okuma izin verir. Ancak, SQL Data Warehouse, bunun izin vermez. Bir SQL Data Warehouse işlem içinde bir hata oluşursa,-2 durumu otomatik olarak girer ve deyim geri kadar daha fazla select deyimi yapmak mümkün olmaz. Bu nedenle, onu XACT_STATE() gibi kullanıp kullanmadığını görmek için uygulama kodunuzda bir kod değişikliği yapmanız gerekebilir denetlemek önemlidir.
> 
> 

Örneğin, SQL Server'da şuna benzer bir işlem görebilirsiniz:

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

Ardından yukarıdaki olduğu gibi kodunuzu bırakırsanız aşağıdaki hata iletisini alırsınız:

Msg 111233, Level 16, State 1, satır 1 111233; Geçerli işlem iptal edildi ve bekleyen değişiklikleri geri alındı. Neden: Bir işlemi yalnızca geri alma durumunda açıkça DDL, DML veya SELECT deyimi önce geri alınmadı.

Ayrıca ERROR_ * işlevleri çıktısını almazsınız.

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

Değişen tüm olan `ROLLBACK` işlemi vardı hata bilgileri okuma önce gerçekleşmesi `CATCH` bloğu.

## <a name="errorline-function"></a>Error_Line() işlevi
Ayrıca, SQL Data Warehouse uygulama ya da ERROR_LINE() işlevini destekler, dikkate değerdir. Bu, kodunuzda varsa, SQL Data Warehouse ile uyumlu olması için kaldırmanız gerekir. Sorgu etiketleri, kodunuzda eşdeğer işlevsellik uygulamak için bunun yerine kullanın. Lütfen [etiket] [ LABEL] bu özellik hakkında daha fazla ayrıntı için makale.

## <a name="using-throw-and-raiserror"></a>THROW ve RAISERROR kullanma
SQL Data warehouse'da özel durumlarını oluşturma için daha modern uygulama THROW olmakla birlikte RAISERROR da desteklenir. Dikkat edilmesi ancak ödeme değer olan bazı farklar vardır.

* Kullanıcı tanımlı hata iletilerinin THROW 100.000 150.000 aralığında sayı olamaz
* RAISERROR hata iletileri 50.000 düzeltilen
* Sistem iletilerinde kullanımı desteklenmiyor

## <a name="limitiations"></a>Limitiations
SQL veri ambarı için işlemleri ile ilgili diğer birkaç kısıtlamalar sahip.

Bunlar aşağıdaki gibidir:

* Dağıtılmış işlem
* İzin verilen hiçbir iç içe geçmiş işlemler
* İzin verilen noktaları kaydetme
* Adlandırılmış işlem
* İşaretli işlem
* DDL gibi desteği `CREATE TABLE` işlem içinde bir kullanıcı tanımlı

## <a name="next-steps"></a>Sonraki adımlar
İşlemler en iyi duruma getirme hakkında daha fazla bilgi için bkz: [işlemleri en iyi uygulamalar][Transactions best practices].  Diğer SQL Data Warehouse en iyi uygulamalar hakkında bilgi edinmek için [SQL veri ambarı en iyi yöntemler][SQL Data Warehouse best practices].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md
[development overview]: ./sql-data-warehouse-overview-develop.md
[Transactions best practices]: ./sql-data-warehouse-develop-best-practices-transactions.md
[SQL Data Warehouse best practices]: ./sql-data-warehouse-best-practices.md
[LABEL]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->

<!--Other Web references-->
