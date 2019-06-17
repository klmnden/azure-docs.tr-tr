---
title: MariaDB için Azure veritabanı'nda sorgu performansı sorunlarını giderme
description: Bu makalede, MariaDB için Azure veritabanı'nda sorgu performans sorunlarını gidermek için açıklama kullanmayı açıklar.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 11/09/2018
ms.openlocfilehash: 672635c8d8c84fa16c106ae79e97332fd740928d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60745171"
---
# <a name="how-to-use-explain-to-profile-query-performance-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı'nda profili sorgu performansı için açıklama kullanma
**AÇIKLAYAN** sorguları iyileştirmek için kullanışlı bir araçtır. Deyim SQL deyimlerinin nasıl yürütüldüğü hakkında bilgi almak için kullanılabilir AÇIKLANMAKTADIR. Aşağıdaki çıktı, örnek olarak bir açıklama deyiminin yürütülmesini gösterir.

```sql
mysql> EXPLAIN SELECT * FROM tb1 WHERE id=100\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 995789
     filtered: 10.00
        Extra: Using where
```

Bu örnekte, değerini görülebileceği gibi *anahtar* null. Bu çıkış, MariaDB sorgu için en iyi duruma getirilmiş tüm dizinler bulunamıyor ve tam tabloyu tarar anlamına gelir. Şimdi bir dizin ekleyerek bu sorgu iyileştirme **kimliği** sütun.

```sql
mysql> ALTER TABLE tb1 ADD KEY (id);
mysql> EXPLAIN SELECT * FROM tb1 WHERE id=100\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: ref
possible_keys: id
          key: id
      key_len: 4
          ref: const
         rows: 1
     filtered: 100.00
        Extra: NULL
```

Yeni açıklama MariaDB artık dizin arama süresini önemli ölçüde sırayla kısalttık 1 satır sayısını sınırlamak için kullandığı gösterir.
 
## <a name="covering-index"></a>Dizin kapsayan
Kapak dizin tabloları değer alma azaltmak için dizindeki bir sorgunun tüm sütunları oluşur. Aşağıdaki çizim işte **GROUP BY** deyimi.
 
```sql
mysql> EXPLAIN SELECT MAX(c1), c2 FROM tb1 WHERE c2 LIKE '%100' GROUP BY c1\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 995789
     filtered: 11.11
        Extra: Using where; Using temporary; Using filesort
```

Uygun dizin kullanılabilir olmadığından MariaDB çıktısı görüldüğü gibi tüm dizinlerin kullanmaz. Ayrıca gösterir *geçici; kullanma Dosya sıralama kullanarak*, yani MariaDB karşılamak için geçici bir tablo oluşturur. **GROUP BY** yan tümcesi.
 
Sütunda dizin oluşturma **c2** hiçbir fark ve MariaDB halen gereken geçici bir tablo oluşturmak için tek başına yapar:

```sql 
mysql> ALTER TABLE tb1 ADD KEY (c2);
mysql> EXPLAIN SELECT MAX(c1), c2 FROM tb1 WHERE c2 LIKE '%100' GROUP BY c1\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 995789
     filtered: 11.11
        Extra: Using where; Using temporary; Using filesort
```

Bu durumda, bir **kapsanan dizin** hem de **c1** ve **c2** değerini ekleme gerçekleştirilmesine oluşturulabilir **c2**"doğrudan içinde dizini Daha fazla veri arama ortadan kaldırır.

```sql 
mysql> ALTER TABLE tb1 ADD KEY covered(c1,c2);
mysql> EXPLAIN SELECT MAX(c1), c2 FROM tb1 WHERE c2 LIKE '%100' GROUP BY c1\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: index
possible_keys: covered
          key: covered
      key_len: 108
          ref: NULL
         rows: 995789
     filtered: 11.11
        Extra: Using where; Using index
```

Yukarıdaki açıklama gösterildiği gibi MariaDB, artık kapsanan dizini kullanır ve geçici bir tablo oluşturmaktan kaçının. 

## <a name="combined-index"></a>Birleşik dizini
Birleştirilmiş bir dizin birden fazla sütundaki değerleri oluşur ve bir dizi dizini oluşturulmuş sütunların değerlerinin birleştirerek sıralanan satırlar kabul edilebilir. Bu yöntem yararlı olabilir. bir **GROUP BY** deyimi.

```sql
mysql> EXPLAIN SELECT c1, c2 from tb1 WHERE c2 LIKE '%100' ORDER BY c1 DESC LIMIT 10\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 995789
     filtered: 11.11
        Extra: Using where; Using filesort
```

MariaDB gerçekleştiren bir *dosya sıralama* oldukça olan işlem yavaş, özellikle çok sayıda sıralanacak varsa. Bu sorgu iyileştirmek için birleştirilmiş bir dizin sıralanır her iki sütun üzerinde oluşturulabilir.

```sql 
mysql> ALTER TABLE tb1 ADD KEY my_sort2 (c1, c2);
mysql> EXPLAIN SELECT c1, c2 from tb1 WHERE c2 LIKE '%100' ORDER BY c1 DESC LIMIT 10\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: index
possible_keys: NULL
          key: my_sort2
      key_len: 108
          ref: NULL
         rows: 10
     filtered: 11.11
        Extra: Using where; Using index
```

Açıklama, artık MariaDB birleşik dizini dizini zaten sıralanmış olduğundan ek sıralama önlemek mümkün olduğunu gösterir.
 
## <a name="conclusion"></a>Sonuç
 
Açıklama ve farklı türde bir dizin kullanarak performansı önemli ölçüde artırabilir. Yalnızca bir dizine sahip için tablo mutlaka MariaDB sorgularınızı kullanabilmek için gelmez. Her zaman açıklama kullanarak, varsayımları doğrulamak ve dizinleri kullanarak sorgularınızı iyileştirmeniz.

## <a name="next-steps"></a>Sonraki adımlar
- Eş en ilgili sorularınıza yanıt bulun veya yeni bir soru/yanıt post için şurayı ziyaret edin [MSDN Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureDatabaseforMariadb) veya [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-database-mariadb).
