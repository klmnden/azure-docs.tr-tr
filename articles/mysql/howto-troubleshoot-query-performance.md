---
title: MySQL için Azure veritabanında sorgu performansı ile ilgili sorunları giderme
description: Bu makalede, Azure veritabanında sorgu performansı MySQL için sorun giderme için açıklama kullanmayı açıklar.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 72b047c37ac88e4b33c8723f8df14c6794e84399
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35266185"
---
# <a name="how-to-use-explain-to-profile-query-performance-in-azure-database-for-mysql"></a>MySQL için Azure veritabanında profili sorgu performansı için açıklama kullanma
**AÇIKLAYAN** sorguları optimize etmek için kullanışlı bir araçtır. Deyim SQL deyimlerini nasıl yürütülen hakkında bilgi almak için kullanılabilir AÇIKLANMAKTADIR. Aşağıdaki çıkış bir açıklama deyimi yürütme örneği gösterir.

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

Bu örnekte, değerini görüldüğü gibi *anahtar* null. Bu çıktı, MySQL sorgu için en iyi duruma getirilmiş dizinleri bulunamıyor ve tam tablo taraması gerçekleştirir anlamına gelir. Şimdi bu sorguyu bir dizin ekleyerek en iyi duruma **kimliği** sütun.

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

Yeni açıklama MySQL artık dizin arama süresini önemli ölçüde sırayla kısaltılmış 1 satır sayısını sınırlamak için kullandığı gösterir.
 
## <a name="covering-index"></a>Dizin kapsayan
Veri tabloları değeri alımı azaltmak için dizin sorguda tüm sütunları, bir kapsayıcı dizini oluşur. Aşağıdaki çizim işte **GROUP BY** deyimi.
 
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

Uygun dizin kullanılabilir olmadığından MySQL çıktısını görüldüğü gibi tüm dizinler kullanmaz. Ayrıca gösterir *geçici; kullanma Dosya sıralama kullanarak*, anlamına MySQL karşılamak için geçici bir tablo oluşturur. **GROUP BY** yan tümcesi.
 
Sütunda dizin oluşturma **c2** hiçbir fark ve MySQL halen gereken geçici bir tablo oluşturmak için tek başına yapar:

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

Bu durumda, bir **kapsanan dizini** hem **c1** ve **c2** değerini ekleme aslına oluşturulabilir **c2**"doğrudan içinde dizini Daha fazla veri arama ortadan kaldırır.

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

Yukarıdaki açıklama gösterildiği gibi MySQL artık kapsanan dizini kullanır ve geçici bir tablo oluşturmamaya özen gösterin. 

## <a name="combined-index"></a>Birleşik dizini
Birleşik bir dizin birden çok sütun değerlerinden oluşur ve bir dizi dizini oluşturulmuş sütunların değerlerini birleştirerek sıralanmış satır kabul edilebilir. Bu yöntem yararlı olabilir bir **GROUP BY** deyimi.

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

MySQL gerçekleştiren bir *dosya sıralama* oldukça olan işlem yavaş, özellikle bu sayıda satır sıralamak sahip olduğunda. Bu sorgu iyileştirmek için birleştirilmiş bir dizin sıralanır her iki sütunlarda oluşturulabilir.

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

Açıklama şimdi MySQL dizini zaten sıralanmış beri ek sıralama önlemek için birleşik dizini kullanmanız mümkün olduğunu gösterir.
 
## <a name="conclusion"></a>Sonuç
 
Açıklama ve farklı türde bir dizinleri kullanarak performansı önemli ölçüde artırabilir. Yalnızca bir dizine sahip olduğundan tablo mutlaka MySQL sorgularınızı kullanabilmek için gelmez. Her zaman AÇIKLA kullanarak, varsayımları doğrulamak ve dizinler kullanılarak sorgularınızı en iyi duruma getirme.


## <a name="next-steps"></a>Sonraki adımlar
- Eş sorularınıza en ilgili bulma veya yeni bir soru/yanıt göndermek için ziyaret [MSDN Forumu](https://social.msdn.microsoft.com/forums/security/en-US/home?forum=AzureDatabaseforMySQL) veya [yığın taşması](https://stackoverflow.com/questions/tagged/azure-database-mysql).
