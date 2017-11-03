---
title: "Saklı yordamlar SQL veri ambarı'nda | Microsoft Docs"
description: "Saklı yordamlar çözümleri geliştirmek için Azure SQL Data Warehouse'da uygulamak için ipuçları."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 9b238789-6efe-4820-bf77-5a5da2afa0e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: e42d80f0ca35f3fbb67389c66d072bc40d8a8d2c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="stored-procedures-in-sql-data-warehouse"></a>Saklı yordamlar SQL veri ambarı
SQL veri ambarı SQL Server'da bulunan Transact-SQL özelliklerinin çoğunu destekler. Daha da önemlisi ölçek genişletme biz çözümünüzün performansını en üst düzeye çıkarmak için yararlanmak isteyen belirli özellikler vardır.

Ancak, korumak için ölçek ve var. SQL veri ambarı performansını ayrıca bazı özellikler ve davranış farklılıkları ve diğerleri desteklenmeyen işlevsellik değildir.

Bu makalede, SQL Data Warehouse içinde saklı yordamlar uygulamak açıklanmaktadır.

## <a name="introducing-stored-procedures"></a>Saklı yordamlar Tanıtımı
Saklı yordamlar SQL kodunuzu Kapsüllenen harika bir yöntemdir; verilerinizi veri ambarındaki yakın depolama. Kod yönetilebilir birimler halinde kapsülleyerek saklı yordamlar çözümleri modülarize etmek geliştiricilerin yardımcı; kod büyük re-usability kolaylaştırmanın. Her bir saklı yordam, ayrıca bunları daha esnek hale getirmek için parametre kabul edebilir.

SQL veri ambarı Basitleştirilmiş ve kolaylaştırılmış saklı yordam uygulamasını sağlar. SQL Server'a karşılaştırma büyük fark saklı yordamı önceden derlenmiş kod olmamasıdır. Veri ambarları genellikle derleme süresi ile daha az endişe duyuyoruz. Saklı yordam kodu doğru büyük veri birimlerine karşı çalışırken en iyi duruma getirilmiş emin daha önemlidir. Saat, dakika ve saniyeleri değil milisaniye kaydetmek için belirtilir. Bu nedenle saklı yordamlar SQL mantığı için kapsayıcı olarak düşünmek daha yardımcı olur.     

SQL Data Warehouse, saklı yordam yürüttüğünde SQL deyimlerini, çevrilen ve çalışma zamanında en iyi duruma getirilmiş ayrıştırılır. Bu işlem sırasında her deyim dağıtılmış sorgular dönüştürülür. Veri karşı gerçekleştirilmeden SQL kodunu gönderilen sorgu farklıdır.

## <a name="nesting-stored-procedures"></a>İç içe geçme saklı yordamlar
Ne zaman saklı yordamlar diğer saklı yordamlar çağırabilir veya iç saklı yordam veya kod çağırma iç içe söylenir sonra dinamik sql Yürüt.

SQL veri ambarı en fazla 8 iç içe geçme düzeyi destekler. Bu SQL Server için biraz farklıdır. SQL Server iç içe geçirme düzeyi 32'dir.

Üst düzey saklı yordam çağrısı düzey 1 iç içe karşılık gelir.

```sql
EXEC prc_nesting
```
Ardından saklı yordamı da başka bir yürütme çağrı yaparsa bu 2 iç içe geçirme düzeyi artırır

```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
Ardından ikinci yordam sonra bazı dinamik sql çalıştırırsa bu iç içe geçirme düzeyi 3 artırır

```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

Not SQL Data Warehouse desteklememektedir@NESTLEVEL. Bir iç içe geçirme düzeyi kendiniz izlenmesi gerekir. 8 iç içe geçirme düzeyi sınırı karşılaşır ancak bunu yaparsanız bu sınırı içinde uyduğunu böylece kodunuzu yeniden çalışmaya ve "düzleştirmek" gerekir olası değil.

## <a name="insertexecute"></a>EKLE... YÜRÜTME
SQL veri ambarı INSERT deyimi olan bir saklı yordam sonuç kümesini kullanmasına izin vermez. Ancak, kullanabileceğiniz alternatif bir yaklaşım yoktur.

Lütfen üzerinde aşağıdaki makaleye bakın [geçici tablolara] bunun nasıl yapılacağı hakkında bir örnek.

## <a name="limitations"></a>Sınırlamalar
SQL veri ambarı'nda uygulanmadı Transact-SQL saklı yordamları bazı yönleri vardır.

Bunlar:

* geçici saklı yordamlar
* numaralandırılmış saklı yordamlar
* genişletilmiş saklı yordamlar
* CLR saklı yordamlar
* şifreleme seçeneği
* çoğaltma seçeneği
* Tablo değerli parametreleri
* salt okunur parametreleri
* varsayılan parametreleri
* yürütme bağlamı
* return deyimi

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipuçları için bkz: [geliştirmeye genel bakış][development overview].

<!--Image references-->

<!--Article references-->
[geçici tablolara]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[development overview]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[nest level]: https://msdn.microsoft.com/library/ms187371.aspx

<!--Other Web references-->
