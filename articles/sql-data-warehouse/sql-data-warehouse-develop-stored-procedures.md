---
title: Azure SQL Data Warehouse'da saklı yordamları kullanma | Microsoft Docs
description: Saklı yordamlar çözümleri geliştirmek için Azure SQL Data Warehouse'da uygulamak için ipuçları.
services: sql-data-warehouse
author: ckarst
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: 5659e8f29d87c48c447a5cb81c836b0be9dabd45
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="using-stored-procedures-in-sql-data-warehouse"></a>SQL veri ambarı'nda saklı yordamları kullanma
Saklı yordamlar çözümleri geliştirmek için Azure SQL Data Warehouse'da uygulamak için ipuçları.

## <a name="what-to-expect"></a>Sizi neler bekliyor

SQL veri ambarı SQL Server kullanılan T-SQL özelliklerini çoğunu destekler. Daha da önemlisi, çözümünüzün performansını en üst düzeye çıkarmak için kullanabileceğiniz genişleme belirli özellikler vardır.

Ancak, korumak için ölçek ve var. SQL veri ambarı performansını ayrıca bazı özellikler ve davranış farklılıkları ve diğerleri desteklenmeyen işlevsellik değildir.


## <a name="introducing-stored-procedures"></a>Saklı yordamlar Tanıtımı
Saklı yordamlar SQL kodunuzu Kapsüllenen harika bir yöntemdir; verilerinizi veri ambarındaki yakın depolama. Saklı yordamlar kodu yönetilebilir birimler halinde kapsülleyerek çözümleri modülarize etmek geliştiricilerin yardımcı; kod büyük kullanılırlığı kolaylaştırmanın. Her bir saklı yordam, ayrıca bunları daha esnek hale getirmek için parametre kabul edebilir.

SQL veri ambarı Basitleştirilmiş ve kolaylaştırılmış saklı yordam uygulamasını sağlar. SQL Server'a karşılaştırma büyük fark saklı yordamı önceden derlenmiş kod olmamasıdır. Veri ambarlarında derleme süresi büyük veri birimlerine karşı sorguları çalıştırmak için geçen süreyi kıyasla küçüktür. Saklı yordam doğru büyük sorgular için en iyileştirilmiş kodda emin olmak daha önemlidir. Saat, dakika ve saniye, değil milisaniye kaydetmek için belirtilir. Bu nedenle saklı yordamlar SQL mantığı için kapsayıcı olarak düşünmek daha yardımcı olur.     

SQL Data Warehouse, saklı yordam yürüttüğünde SQL deyimlerini ayrıştırılır, çevrilen ve çalışma zamanında en iyi duruma getirilmiş. Bu işlem sırasında her deyim dağıtılmış sorgular dönüştürülür. Y veri karşı yürütülen SQL kodunu gönderilen sorgu farklıdır.

## <a name="nesting-stored-procedures"></a>İç içe geçme saklı yordamlar
Saklı yordamlar diğer saklı yordamları çağırmak ya da dinamik SQL Yürüt ardından iç saklı yordam veya kod çağırma iç içe söylenir.

SQL veri ambarı en fazla sekiz iç içe geçme düzeyi destekler. Bu SQL Server için biraz farklıdır. SQL Server iç içe geçirme düzeyi 32'dir.

Üst düzey saklı yordam çağrısı düzey 1 iç içe karşılık gelir.

```sql
EXEC prc_nesting
```
Saklı yordam ayrıca başka bir yürütme çağrı yaparsa, iç içe geçirme düzeyi iki artırır.

```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
İkinci yordam sonra bazı dinamik SQL yürütülürse, iç içe geçirme düzeyi üç artırır.

```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

Not: SQL Data Warehouse desteklememektedir [@@NESTLEVEL](/sql/t-sql/functions/nestlevel-transact-sql). İç içe geçirme düzeyi izlemeniz gerekir. Sekiz iç içe geçirme düzeyi sınırı aşan daha düşüktür, ancak bunu yaparsanız, bu sınırı içinde iç içe geçme düzeyi uyacak şekilde kodunuzu rework gerekir.

## <a name="insertexecute"></a>EKLE... YÜRÜTME
SQL veri ambarı INSERT deyimi olan bir saklı yordam sonuç kümesini kullanmasına izin vermez. Ancak, kullanabileceğiniz alternatif bir yaklaşım yoktur. Bir örnek için üzerinde makalesine bakın. [geçici tablolar](sql-data-warehouse-tables-temporary.md). 

## <a name="limitations"></a>Sınırlamalar
SQL veri ambarı'nda uygulanmadı Transact-SQL saklı yordamları bazı yönleri vardır.

Bunlar:

* geçici saklı yordamlar
* numaralandırılmış saklı yordamlar
* genişletilmiş saklı yordamlar
* CLR saklı yordamlar
* 
* şifreleme seçeneği
* çoğaltma seçeneği
* Tablo değerli parametreleri
* salt okunur parametreleri
* varsayılan parametreleri
* yürütme bağlamı
* return deyimi

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipuçları için bkz: [geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).

