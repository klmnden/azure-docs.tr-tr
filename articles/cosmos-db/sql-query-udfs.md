---
title: Kullanıcı tanımlı işlevler (UDF'ler) olarak Azure Cosmos DB'de
description: Azure Cosmos DB'de kullanıcı tanımlı işlevleri hakkında bilgi edinin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: mjbrown
ms.openlocfilehash: e168e450230720f4ad78516e6edcdc3aa08ba3e1
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342442"
---
# <a name="user-defined-functions-udfs-in-azure-cosmos-db"></a>Kullanıcı tanımlı işlevler (UDF'ler) olarak Azure Cosmos DB'de

SQL API'si, kullanıcı tanımlı işlevler (UDF'ler) için destek sağlar. Skaler UDF ile sıfır veya daha fazla bağımsız değişkenleri geçirmek ve tek bir bağımsız değişken sonuç döndürür. API'nin her bağımsız değişken geçerli JSON değerleri teşekkür denetler.  

API'si UDF'leri kullanarak özel uygulama mantığını destekleyecek şekilde SQL söz dizimi genişletir. SQL API'si ile UDF'ler kaydetmek ve bunları SQL sorgularında başvurun. Aslında, UDF'ler exquisitely sorgularından çağırmak için tasarlanmıştır. Bir corollary UDF'ler saklı yordamları ve Tetikleyicileri gibi diğer JavaScript türleri gibi bağlam nesnesi için erişiminiz yok. Sorgular salt okunurdur ve birincil veya ikincil çoğaltma üzerinde çalıştırabilirsiniz. Diğer JavaScript türlerinin aksine, UDF'ler, ikincil çoğaltmalarda çalışacak şekilde tasarlanmıştır.

Aşağıdaki örnek bir öğe kapsayıcısı altında bir UDF Cosmos DB veritabanına kaydeder. Örnek adı olan bir UDF oluşturur `REGEX_MATCH`. İki JSON dizesi değerini kabul `input` ve `pattern`, ve ilk eşleşme ikinci desen belirtilmişse denetimleri kullanarak JavaScript'in `string.match()` işlevi.

## <a name="examples"></a>Örnekler

```javascript
       UserDefinedFunction regexMatchUdf = new UserDefinedFunction
       {
           Id = "REGEX_MATCH",
           Body = @"function (input, pattern) {
                      return input.match(pattern) !== null;
                   };",
       };

       UserDefinedFunction createdUdf = client.CreateUserDefinedFunctionAsync(
           UriFactory.CreateDocumentCollectionUri("myDatabase", "families"),
           regexMatchUdf).Result;  
```

Şimdi bu UDF bir sorgu projeksiyon kullanın. Büyük/küçük harfe ön ekine sahip UDF nitelemelidir `udf.` bunları sorgulara çağırırken.

```sql
    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families
```

Sonuçlar şu şekildedir:

```json
    [
      {
        "$1": true
      },
      {
        "$1": false
      }
    ]
```

İle tam UDF kullanabileceğiniz `udf.` aşağıdaki örnekteki gibi bir filtre içinde öneki:

```sql
    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")
```

Sonuçlar şu şekildedir:

```json
    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]
```

Esas olarak, UDF'ler izdüşümler ve filtreler kullandığınız geçerli bir skaler ifadelerin ' dir.

UDF'ler gücüyle genişletmek için koşullu mantık ile başka bir örneğe bakın:

```javascript
       UserDefinedFunction seaLevelUdf = new UserDefinedFunction()
       {
           Id = "SEALEVEL",
           Body = @"function(city) {
                   switch (city) {
                       case 'Seattle':
                           return 520;
                       case 'NY':
                           return 410;
                       case 'Chicago':
                           return 673;
                       default:
                           return -1;
                    }"
            };

            UserDefinedFunction createdUdf = await client.CreateUserDefinedFunctionAsync(
                UriFactory.CreateDocumentCollectionUri("myDatabase", "families"),
                seaLevelUdf);
```

Aşağıdaki örnek UDF uygular:

```sql
    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f
```

Sonuçlar şu şekildedir:

```json
     [
      {
        "city": "Seattle",
        "seaLevel": 520
      },
      {
        "city": "NY",
        "seaLevel": 410
      }
    ]
```

Başvurulan özellikleri tarafından UDF parametreler JSON değeri kullanılamaz, parametre olarak kabul edilir tanımsız ve UDF çağrısı atlandı. Benzer şekilde, UDF sonucu tanımlanmamış ise, sonuçta bulunmaz.

Önceki örneklerde gösterildiği gibi UDF'ler JavaScript dilinin gücünü SQL API'si ile tümleştirin. UDF yerleşik JavaScript çalışma zamanı Özellikleri'nın yardımıyla karmaşık yordam, koşullu mantık yapmak için zengin bir programlanabilir arabirimi sağlar. SQL API'si bağımsız değişkenler UDF için her kaynak öğesi için geçerli nerede veya SELECT yan tümcesi sağlar aşaması. Sonuç, sorunsuz bir şekilde genel yürütme işlem hattında eklenmiştir. Özet olarak, karmaşık iş mantığı sorguları bir parçası olarak yapmak için harika Araçlar UDF'ler var.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB'ye giriş](introduction.md)
- [Sistem işlevleri](sql-query-system-functions.md)
- [toplamaları](sql-query-aggregates.md)