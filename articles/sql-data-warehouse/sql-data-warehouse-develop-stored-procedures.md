---
title: Azure SQL veri ambarı'nda saklı yordamları kullanma | Microsoft Docs
description: Saklı yordamlar çözümleri geliştirmek için Azure SQL veri ambarı'nda uygulama hakkında ipuçları.
services: sql-data-warehouse
author: ckarst
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/02/2019
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: 8a53a63b7425935e117d7af951717999bc9340b7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61439732"
---
# <a name="using-stored-procedures-in-sql-data-warehouse"></a>SQL veri ambarı'nda saklı yordamları kullanma
Saklı yordamlar çözümleri geliştirmek için Azure SQL veri ambarı'nda uygulama hakkında ipuçları.

## <a name="what-to-expect"></a>Sizi neler bekliyor

SQL veri ambarı SQL Server kullanılan T-SQL özelliklerinin çoğunu destekler. Daha da önemlisi, çözümünüzün performansını en üst düzeye çıkarmak için kullanabileceğiniz genişleme belirli özellikleri vardır.

Ancak korumak için gereken ölçeğe ve SQL veri ambarı performans var. Ayrıca bazı özellikler ve davranış farklılıkları ve diğer desteklenmeyen işlevler değildir.


## <a name="introducing-stored-procedures"></a>Saklı yordamları ile tanışın
Saklı yordamlar SQL kodunuzu kapsüllemek için harika bir yoludur; veri ambarı'nda, verilere yaklaştırılmasıyla depolamak. Saklı yordamlar, geliştiricilerin çözümlerine kod yönetilebilir birimler halinde kapsülleyerek modülerleştirmek yardımcı; büyük kod kullanılırlığı etkinleştirme. Her bir saklı yordam, ayrıca daha esnek hale getirmek için parametreleri kabul edebilir.

SQL veri ambarı, Basitleştirilmiş ve kolaylaştırılmış saklı yordamı bir uygulamasını sağlar. SQL Server'a kıyasla en büyük fark, saklı yordamın önceden derlenmiş kodu değil ' dir. Veri ambarında, derleme zamanı büyük veri birimlerine karşı sorguları çalıştırmak için gereken süreyi kolaylığına küçüktür. Saklı yordam kodu büyük sorgular için doğru bir şekilde iyileştirilmesini sağlamak daha önemlidir. Saat, dakika ve saniye, milisaniye değil kaydetmek için kullanılan hedeftir. Bu nedenle saklı yordamlar SQL mantıksal için kapsayıcı olarak düşünün daha yararlı olur.     

SQL veri ambarı, depolanmış yordamınızdaki yürütüldüğünde, SQL deyimlerini ayrıştırıldığında, çevrilmiş ve çalışma zamanında en iyi duruma getirilmiş. Bu işlem sırasında her deyim dağıtılmış sorgulara dönüştürülür. Veri karşı yürütülen SQL kod gönderilen sorgu farklıdır.

## <a name="nesting-stored-procedures"></a>İç içe geçme depolanan yordamları
Saklı yordamlar diğer saklı yordamları çağıran ya da dinamik SQL yürütme, ardından iç saklı yordamı veya kodu çağırma iç içe kabul edilir.

SQL veri ambarı en fazla sekiz iç içe geçme düzeyini destekler. Bu SQL Server için biraz farklıdır. SQL Server iç içe geçirme düzeyinde 32'dir.

Düzey 1 içine yerleştirmek için üst düzey bir saklı yordam çağrısı karşılık gelmektedir.

```sql
EXEC prc_nesting
```
Saklı yordamı da başka bir ÇALIŞTIRILABİLİR çağrısı yaparsa, iç içe düzeyi için iki artırır.

```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
İkinci yordam sonra bazı dinamik SQL yürütülürse, iç içe düzeyi için üç artırır.

```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

Unutmayın, SQL veri ambarı şu anda desteklemiyor [@@NESTLEVEL](/sql/t-sql/functions/nestlevel-transact-sql). İç içe düzeyi izlemek gerekir. Sekiz iç içe düzeyi sınırı daha düşüktür, ancak bunu yaparsanız, bu sınırı içinde iç içe geçme düzeylerini uyacak şekilde kodunuzu yeniden gerekir.

## <a name="insertexecute"></a>EKLE... YÜRÜTME
SQL veri ambarı, sonuç kümesi saklı yordam INSERT deyimi ile kullanmak için izin vermez. Ancak, kullanabileceğiniz alternatif bir yaklaşım yoktur. Örneğin, makaleye bakın [geçici tablolar](sql-data-warehouse-tables-temporary.md). 

## <a name="limitations"></a>Sınırlamalar
SQL veri ambarı'nda uygulanmaz ve Transact-SQL saklı yordamları bazı yönlerini vardır.

Bunlar:

* geçici saklı yordamlar
* numaralanmış saklı yordamlar
* Genişletilmiş saklı yordamlar
* CLR saklı yordamları
* Şifreleme seçeneği
* çoğaltma seçeneği
* Tablo değerli Parametreler
* salt okunur parametreleri
* varsayılan parametreleri
* yürütme bağlamları
* return deyimi

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipuçları için bkz: [geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).

