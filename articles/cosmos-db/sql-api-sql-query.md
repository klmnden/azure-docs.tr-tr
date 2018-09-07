---
title: Azure Cosmos DB SQL sorgularında | Microsoft Docs
description: Azure Cosmos DB SQL söz dizimi, veritabanı kavramlarını ve SQL sorguları hakkında bilgi edinin. SQL Azure Cosmos DB'de JSON sorgu dili olarak kullanılabilir.
keywords: SQL söz dizimi, sql sorgu, sql sorguları, json sorgu dili, veritabanı kavramlarını ve sql sorguları, toplama işlevleri
services: cosmos-db
author: LalithaMV
manager: kfile
editor: monicar
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: na
ms.topic: conceptual
ms.date: 08/10/2018
ms.author: laviswa
ms.openlocfilehash: 766a2a9a2b71d9cd013f26b843d413d7603ab1fd
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44051366"
---
# <a name="query-azure-cosmos-db-data-with-sql-queries"></a>Azure Cosmos DB verileri içeren SQL sorguları sorgulama

Microsoft Azure Cosmos DB SQL API hesabı bir JSON sorgu dili olarak SQL (yapılandırılmış sorgu dili) kullanarak belgelerin sorgulanmasını destekler. Azure Cosmos DB için sorgu dili tasarlarken aşağıdaki iki hedefleri olarak kabul edilir:

* Yeni bir sorgu dili inventing yerine en bilinen ve en popüler sorgu dillerden biri SQL desteklemek için Azure Cosmos DB yaptık. Azure Cosmos DB SQL, JSON belgeleri için zengin sorguların biçimsel bir programlama modeli sağlar.  

* Azure Cosmos DB, temel olarak JavaScript'in programlama modeli için sorgu dili kullanır. SQL API'si, JavaScript'in tür sistemi, ifade değerlendirmesi ve işlev çağrısını kökü belirtilmemiş. Bu, dönüş JSON belgelerini, kendinden birleştirmeler, uzamsal sorgular ve tamamen yanı sıra başka özellikler, JavaScript dilinde yazılmış kullanıcı tanımlı işlevler (UDF'ler) çağrılmasını projeksiyonlar ilişkisel, hiyerarşik gezinme için doğal bir programlama modeli sağlar. 

Bu makalede basit JSON belgelerini kullanarak SQL sorguları bazı örnekler gösterilmektedir. Azure Cosmos DB SQL dili sözdizimi hakkında bilgi edinmek için [SQL söz dizimi başvurusu](sql-api-sql-query-reference.md) makalesi. 

## <a id="GettingStarted"></a>SQL komutları ile çalışmaya başlama
İki basit JSON belgelerinin ve bu verilere karşı sorgu oluşturalım. İki JSON belgelerini aileleri hakkında göz önünde bulundurun, bu JSON belgelerini bir koleksiyona ekleyin ve ardından verileri sorgulamak. Burada basit JSON sahibiz belge Andersen ve Wakefield ailesi, üst, alt öğelerini (ve bunların Evcil Hayvanlar), adresi ve kayıt bilgileri. Belge dizeleri, sayı, Boole, diziler ve iç içe özellikler vardır. 

**Document1**  

```JSON
{
  "id": "AndersenFamily",
  "lastName": "Andersen",
  "parents": [
     { "firstName": "Thomas" },
     { "firstName": "Mary Kay"}
  ],
  "children": [
     {
         "firstName": "Henriette Thaulow", 
         "gender": "female", 
         "grade": 5,
         "pets": [{ "givenName": "Fluffy" }]
     }
  ],
  "address": { "state": "WA", "county": "King", "city": "seattle" },
  "creationDate": 1431620472,
  "isRegistered": true
}
```

Bir fark – ikinci bir belgesiyle işte `givenName` ve `familyName` yerine kullanılan `firstName` ve `lastName`.

**Document2**  

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```

Artık Azure Cosmos DB SQL sorgu dili önemli yönlerini bazıları anlamak için bu verilere karşı birkaç sorgu deneyelim. 

**Sorgu1**: Örneğin, aşağıdaki sorgu Kimliği alanı eşleştiği belgeleri döndürür `AndersenFamily`. Olduğundan bir `SELECT *`sorgunun çıkışı eksiksiz JSON belgesidir, söz dizimi hakkında bilgi edinmek için [SELECT deyimi](sql-api-sql-query-reference.md#select-query):

```sql
    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"
```

**Sonuçlar**

```json
    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]
```

**Sorgu2** : artık burada ihtiyacımız farklı JSON çıkışını yeniden biçimlendirmek için bir durum düşünün. Adresi Şehir durumu olarak aynı ada sahip olduğunda bu sorgu adı ve şehir olmak üzere iki seçili alanları içeren yeni bir JSON nesnesi projelere. Bu durumda, "NY, NY" ile eşleşir.   

```sql
    SELECT {"Name":f.id, "City":f.address.city} AS Family 
    FROM Families f 
    WHERE f.address.city = f.address.state
```

**Sonuçlar**

```json
    [{
        "Family": {
            "Name": "WakefieldFamily", 
            "City": "NY"
        }
    }]
```

**Query3**: kimliği eşleşir ailedeki çocukların tüm adlarını Children bu sorgunun döndürdüğü `WakefieldFamily` ikametgahınızda şehirlere göre sıralanmış.

```sql
    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC
```

**Sonuçlar**

```json
    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]
```

Şu ana kadar gördüğünüz örnekleri Cosmos sorgu dili bazı yönleri şunlardır:  

* SQL API'si, JSON değerleri üzerinde çalışır olduğundan, satır ve sütun yerine varlıklar şeklinde ağaç ile ilgilidir. Bu nedenle, dil, rastgele herhangi derinliği ağaç düğümleri gibi başvurmak sağlar `Node1.Node2.Node3…..Nodem`benzer şekilde iki bölümlü başvuru başvuran ilişkisel SQL `<table>.<column>`.   

* Yapılandırılmış sorgu dili, şemasız verileri ile çalışır. Bu nedenle, tür sisteminde dinamik olarak bağlanması gerekir. Aynı ifadeye farklı belgelere farklı türlerde üretebilir. Bir sorgunun sonucu, geçerli bir JSON değer, ancak bir sabit şemasına olması garanti edilmez.  

* Azure Cosmos DB, yalnızca Katı JSON belgelerini destekler. Bu tür sistemi ve ifadeleri yalnızca JSON türleri ile dağıtılacak sınırlı olduğu anlamına gelir. Başvurmak [JSON belirtimi](http://www.json.org/) daha fazla ayrıntı için.  

* Bir Cosmos DB koleksiyonu JSON belgelerinin şemasız bir kapsayıcıdır. Veri varlıkları içinde ve bir koleksiyondaki belgeler arasında ilişkiler, kapsama ve birincil anahtar ve yabancı anahtar ilişkileri tarafından örtük olarak yakalanır. Belirtmemiz bu makalenin sonraki bölümlerinde ele alınan içi belge birleştirmeler sonra önemli bir yönüdür.

## <a id="SelectClause"></a>select tümcesi

Her sorgu bir SELECT yan tümcesi ve isteğe bağlı FROM oluşur ve WHERE yan tümcelerini başına ANSI SQL standartları. Genellikle, her sorgu için kaynak FROM yan tümcesindeki numaralandırılmış alan şeklinde. Ardından filtre WHERE yan tümcesinde bir alt kümesi JSON belgelerini almak için kaynak uygulanır. Son olarak, SELECT yan tümcesi, select listesindeki istenen JSON değerleri proje için kullanılır. Söz dizimi hakkında bilgi edinmek için [SELECT söz dizimi](sql-api-sql-query-reference.md#bk_select_query).

Aşağıdaki örnek, tipik bir SELECT sorgusu gösterir. 

**Sorgu**

```sql
    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"
```

**Sonuçlar**

```json
    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]
```

### <a name="nested-properties"></a>İç içe Özellikler
Aşağıdaki örnekte, biz iki iç içe özellikler yansıtma `f.address.state` ve `f.address.city`.

**Sorgu**

```sql
    SELECT f.address.state, f.address.city
    FROM Families f 
    WHERE f.id = "AndersenFamily"
```

**Sonuçlar**

```json
    [{
      "state": "WA", 
      "city": "seattle"
    }]
```

Projeksiyon, aşağıdaki örnekte gösterildiği gibi JSON ifadeleri de destekler:

**Sorgu**

```sql
    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"
```

**Sonuçlar**

```json
    [{
      "$1": {
        "state": "WA", 
        "city": "seattle", 
        "name": "AndersenFamily"
      }
    }]
```

Rolü, bakalım `$1` burada. `SELECT` Yan tümcesi bir JSON nesnesi oluşturmak için gereksinim duyduğu ve anahtar sağlanan örtük bağımsız değişken adları ile başlayan kullanıyoruz `$1`. Örneğin, iki örtük bağımsız değişkenlerini etiketlenmiş, bu sorgunun döndürdüğü `$1` ve `$2`.

**Sorgu**

```sql
    SELECT { "state": f.address.state, "city": f.address.city }, 
           { "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"
```

**Sonuçlar**

```json
    [{
      "$1": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]
```

## <a id="FromClause"></a>FROM yan tümcesi

Kaynak filtre veya sorguyu daha sonra öngörülen sürece < from_specification > yan tümcesinin isteğe bağlıdır. Söz dizimi hakkında bilgi edinmek için [SÖZDİZİMİNDEN](sql-api-sql-query-reference.md#bk_from_clause). Bir sorgu ister `SELECT * FROM Families` tüm aileleri koleksiyona kaynak üzerinden numaralandırmak olduğunu gösterir. Özel bir tanımlayıcısı kök, koleksiyon adını kullanmak yerine bir koleksiyonu temsil etmek için kullanılabilir. Aşağıdaki listede, sorgu uygulanan kurallar içerir:

* Koleksiyon gibi diğer adı, olabilir `SELECT f.id FROM Families AS f` ya da yalnızca `SELECT f.id FROM Families f`. Burada `f` eşdeğerdir `Families`. `AS` diğer isteğe bağlı bir anahtar sözcük tanımlayıcısıdır.  

* Diğer adlı bir kez özgün kaynağına bağımlı olamaz. Örneğin, `SELECT Families.id FROM Families f` artık "Aileleri" tanımlayıcısı çözümlenemiyor beri sözdizimsel olarak geçersiz.  

* Başvurulabilmesi için gereken tüm özellikleri tam olarak nitelenmiş olmalıdır. Katı şema bağlılığı olmaması durumunda, belirsiz bağlamaları önlemek için bu zorunlu kılınır. Bu nedenle, `SELECT id FROM Families f` beri özellik sözdizimsel olarak geçersiz `id` bağlı değil.

### <a name="get-subdocuments-using-from-clause"></a>FROM yan tümcesi kullanarak alt Al

Kaynak, ayrıca daha küçük bir alt kümesine azaltılabilir. Örneğin, yalnızca bir alt ağacı her belgede numaralandırma için subroot sonra kaynak aşağıdaki örnekte gösterildiği gibi hale gelebilir:

**Sorgu**

```sql
    SELECT * 
    FROM Families.children
```

**Sonuçlar**  

```json
    [
      [
        {
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [
              {
                  "givenName": "Fluffy"
              }
            ]
        }
      ],
      [
        {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        },
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]
```

Yukarıdaki örnekte, bir dizi kaynak olarak kullanılabilir. ancak, bir nesne de, aşağıdaki örnekte gösterilene kaynağı olarak kullanılabilir: sorgu sonucunu eklenmesi için kaynak bulunabilir (tanımsız değil) tüm geçerli JSON değeri olarak kabul edilir. Bazı aileleri yoksa bir `address.state` değeri sorgu sonucu hariç tutulur.

**Sorgu**

```sql
    SELECT * 
    FROM Families.address.state
```

**Sonuçlar**

```json
    [
      "WA", 
      "NY"
    ]
```

## <a id="WhereClause"></a>WHERE yan tümcesi
WHERE yan tümcesi (**`WHERE <filter_condition>`**) isteğe bağlıdır. Bu, JSON belgelerini kaynak tarafından sağlanan koşulları sonucunu bir parçası olarak dahil edilmesi için karşılaması gereken belirtir. Herhangi bir JSON belgesi, "için sonuç olarak kabul edilmesi için true olarak" belirli koşullar değerlendirmelidir. WHERE yan tümcesi, sonuç bir parçası olabilir kaynak belgeleri mutlak en küçük kümesini belirlemek için dizin katmanı tarafından kullanılır. Söz dizimi hakkında bilgi edinmek için [nerede söz dizimi](sql-api-sql-query-reference.md#bk_where_clause).

Aşağıdaki sorgu, değeri olan bir ad özelliği içeren belgeleri istekleri `AndersenFamily`. Bir name özelliğine sahip olmayan başka bir belgeye veya burada değeri eşleşmiyor `AndersenFamily` çıkarılır. 

**Sorgu**

```sql
    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"
```

**Sonuçlar**

```json
    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]
```

Önceki örnekte, bir basit eşitlik sorgu gösterdi. SQL API'si, skaler ifadelerin çeşitli de destekler. En sık kullanılan ikili ve birli ifadelerdir. Kaynak JSON nesne özelliği başvurularından da geçerli ifadelerdir. 

Aşağıdaki ikili işleçleri, şu anda desteklenen ve sorgularda aşağıdaki örneklerde gösterildiği gibi kullanılabilir:  

|**İşleç türü**  |**Değerler**  |
|---------|---------|
|Aritmetik    |   +,-,*,/,%   |
|bit düzeyinde  |   |, &, ^, <<>>,, >>> (sıfır dolgu sağa kaydırma)      |
|Mantıksal   |   VE, VEYA DEĞİL      |
|Karşılaştırma   |    =, !=, &lt;, &gt;, &lt;=, &gt;=, <>     |
|Dize  |  || (birleştirme)       |

İkili işleçler kullanarak bazı sorguları bir göz atalım.

```sql
    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5     -- matching grades == 5
```

Birli işleçler +,-, ~, değil de desteklenir ve aşağıdaki örneklerde gösterildiği gibi sorguları içinde kullanılabilir:

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1

    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5
```

İkili ve birli işleçler ek olarak başvuran bir özelliğe de izin verilir. Örneğin, `SELECT * FROM Families f WHERE f.isRegistered` özelliği içeren JSON belgesini döndürür `isRegistered` özelliğinin değeri olduğu JSON eşit `true` değeri. Herhangi bir değer (false, null, Undefined `<number>`, `<string>`, `<object>`, `<array>`, vs.) sonuçtan dışlanan kaynak belge için yol açar. 

### <a name="equality-and-comparison-operators"></a>Eşitlik ve Karşılaştırma işleçleri
Aşağıdaki tabloda, her iki JSON türünden SQL API eşitlik karşılaştırmaları sonucunu gösterir.

<table style = "width:300px">
   <tbody>
      <tr>
         <td valign="top">
            <strong>OP</strong>
         </td>
         <td valign="top">
            <strong>Tanımsız</strong>
         </td>
         <td valign="top">
            <strong>Null</strong>
         </td>
         <td valign="top">
            <strong>Boole değeri</strong>
         </td>
         <td valign="top">
            <strong>Sayı</strong>
         </td>
         <td valign="top">
            <strong>dize</strong>
         </td>
         <td valign="top">
            <strong>Nesne</strong>
         </td>
         <td valign="top">
            <strong>Dizi</strong>
         </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Tanımsız<strong>
         </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Null<strong>
         </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
            <strong>TAMAM</strong>
         </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Boole değeri<strong>
         </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
            <strong>TAMAM</strong>
         </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Sayı<strong>
         </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
            <strong>TAMAM</strong>
         </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>dize<strong>
         </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
            <strong>TAMAM</strong>
         </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Nesne<strong>
         </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
            <strong>TAMAM</strong>
         </td>
         <td valign="top">
Tanımlanmadı </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Dizi<strong>
         </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
Tanımlanmadı </td>
         <td valign="top">
            <strong>TAMAM</strong>
         </td>
      </tr>
   </tbody>
</table>

Gibi diğer Karşılaştırma işleçleri için >, > =,! =, <, ve < =, aşağıdaki kurallar geçerlidir:   

* Karşılaştırma türlerinde içinde tanımsız olur.  
* İki nesne ya da iki arasında karşılaştırma sonuçlarında tanımlanmamış dizi.   

Filtredeki bir skaler ifade sonucu olup olmadığını tanımlanmamış mantıksal olarak "true" günleriyle değil olduğundan tanımlanmamışsa, ilgili belge sonucunda dahil.

## <a name="between-keyword"></a>ARASINDA anahtar sözcüğü
BETWEEN anahtar sözcüğü, ANSI SQL gibi değer sorguları express için de kullanabilirsiniz. ARASINDA karşı dize veya sayı kullanılabilir.

Örneğin, bu sorgu ilk alt öğenin sınıf 1-5 arasında (her ikisi de dahil) olduğu tüm ailesi belgeleri döndürür. 

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5
```

Farklı ANSI-SQL'de BETWEEN yan tümcesi aşağıdaki örnekteki gibi FROM yan tümcesinde kullanabilirsiniz.

```sql
    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c
```

SQL API ve ANSI SQL BETWEEN arasındaki temel fark, bazı belgelerde ve diğerleri ("grade4") dizelerde karma türlerin özelliklerine karşı aralık sorguları ifade edebilirsiniz: Örneğin, "sınıf bir sayı (5)" olabilir olduğu. Bu gibi durumlarda gibi JavaScript'te "Tanımlanmamış" içindeki iki farklı sonuçlar ve belge arasında bir karşılaştırma atlanacak.

> [!NOTE]
> Daha hızlı sorgu yürütme süreleri için tüm sayısal özellikleri/BETWEEN yan tümcesinde göre filtrelenmiş olan yollar karşı bir aralık dizin türü kullanan bir dizin oluşturma ilkesi oluşturmak unutmayın. 

### <a name="logical-and-or-and-not-operators"></a>Mantıksal (AND, OR ve NOT) işleçleri
Mantıksal işleçler Boole değerleri üzerinde çalışır. Bu işleçler için mantıksal gerçekte tabloları aşağıdaki tabloda gösterilmektedir.

**OR işleci**

| OR | True | False | Tanımlanmadı |
| --- | --- | --- | --- |
| True |True |True |True |
| False |True |False |Tanımlanmadı |
| Tanımlanmadı |True |Tanımlanmadı |Tanımlanmadı |

**AND işleci**

| VE | True | False | Tanımlanmadı |
| --- | --- | --- | --- |
| True |True |False |Tanımlanmadı |
| False |False |False |False |
| Tanımlanmadı |Tanımlanmadı |False |Tanımlanmadı |

**NOT işleci**

| DEĞİL |  |
| --- | --- |
| True |False |
| False |True |
| Tanımlanmadı |Tanımlanmadı |

## <a name="in-keyword"></a>Anahtar SÖZCÜĞÜ

IN anahtar sözcüğü, bir listedeki herhangi bir değer belirtilen bir değerle eşleşip eşleşmediğini kontrol etmek için kullanılabilir. Örneğin, bu sorgu kimliği "WakefieldFamily" veya "AndersenFamily" biri olduğu tüm ailesi belgeleri döndürür. 

```sql
    SELECT *
    FROM Families 
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')
```

Bu örnek, durum belirtilen değerlerden herhangi birini olduğu tüm belgeleri döndürür.

```sql
    SELECT *
    FROM Families 
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")
```

## <a name="ternary--and-coalesce--operators"></a>Ternary (?) ve (?) birleşim işleçleri

Üçlü ve birleşim işleçleri, C# ve JavaScript gibi popüler programlama dillerini benzer olarak, koşullu ifadeleri oluşturmak için kullanılabilir. Ternary (?) işleci hareket halindeyken yeni JSON özellikleri oluştururken çok kullanışlı olabilir. Örneğin, artık başlangıç/Orta/aşağıda gösterildiği gibi gelişmiş gibi bir insan tarafından okunabilir formda uygulamasına sınıfı düzeyleri sınıflandırmak için sorgular yazarsınız.

```sql
     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel 
     FROM Families.children[0] c
```

Like işleci aşağıdaki sorgu çağrıları iç içe yerleştirebilirsiniz.

```sql
    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high")  AS gradeLevel 
    FROM Families.children[0] c
```

Olarak diğer sorgu işleçleri ile herhangi bir belgede başvurulan özelliklerini koşullu ifadede eksikse veya karşılaştırılan türleri farklıysa, ardından bu belgeleri sorgu sonuçlarını hariç tutulur.

Coalesce (?) işleci, verimli bir şekilde bir özellik için (diğer adıyla) olup olmadığını denetlemek için kullanılabilir tanımlanır) bir belgedeki. Yarı yapılandırılmış karşı sorgularken bu kullanışlıdır veya karma türlerde verileri. Örneğin, mevcut değilse, bu sorgu "lastName" belirlenirse ya da "Soyadı" döndürür.

```sql
    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f
```

## <a id="EscapingReservedKeywords"></a>Tırnak işaretli bir özellik erişimcisi
Tırnak işaretli bir özellik işleci kullanarak özelliklerini de erişebilirsiniz `[]`. Örneğin, `SELECT c.grade` ve `SELECT c["grade"]` eşdeğerdir. Bu sözdizimi, boşluk, özel karakterler içeriyor veya bir SQL anahtar sözcüğü ya da ayrılmış sözcük olarak aynı adı paylaşmasını olur bir özelliği kaçış gerektiğinde faydalıdır.

```sql
    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"
```

## <a name="aliasing"></a>Diğer ad kullanımı

Şimdi şimdi yukarıdaki değerleri örnek açık bir diğer ad kullanımı ile genişletin. Olduğu gibi bir diğer ad kullanımı için kullanılan anahtar sözcüğü. İkinci değer olarak yansıtma sırasında gösterildiği gibi isteğe bağlı `NameInfo`. 

Sorguda iki özellik aynı ada sahip olması durumunda, diğer ad kullanımı, böylece bunlar öngörülen sonucunda disambiguated birini veya her ikisini özelliklerini yeniden adlandırmak için kullanılmalıdır.

**Sorgu**
```sql
    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo, 
           { "name": f.id } NameInfo
    FROM Families f 
    WHERE f.id = "AndersenFamily"
```

**Sonuçlar**

```json
    [{
      "AddressInfo": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]
```

## <a name="scalar-expressions"></a>Skaler ifade
Özellik başvurularını yanı sıra, SELECT yan tümcesi skaler ifadeler sabitler, aritmetik ifadeler, mantıksal ifadeleri, vb. gibi de destekler. Örneğin, basit bir "Merhaba Dünya" sorgu aşağıdadır.

**Sorgu**

```sql
    SELECT "Hello World"
```

**Sonuçlar**

```json
    [{
      "$1": "Hello World"
    }]
```

Skaler bir ifade kullanan daha karmaşık bir örnek aşağıda verilmiştir.

**Sorgu**

```sql
    SELECT ((2 + 11 % 7)-2)/3    
```

**Sonuçlar**

```json
    [{
      "$1": 1.33333
    }]
```

Aşağıdaki örnekte, bir Boolean skaler ifade sonucudur.

**Sorgu**

```sql
    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f    
```

**Sonuçlar**

```json
    [
      {
        "AreFromSameCityState": false
      }, 
      {
        "AreFromSameCityState": true
      }
    ]
```

## <a name="object-and-array-creation"></a>Nesne ve dizi oluşturma
Başka bir anahtar SQL API'si dizi/nesne oluşturma özelliğidir. Önceki örnekte, oluşturduğumuz yeni bir JSON nesnesi unutmayın. Benzer şekilde, bir de diziler aşağıdaki örneklerde gösterildiği gibi oluşturabilirsiniz:

**Sorgu**

```sql
    SELECT [f.address.city, f.address.state] AS CityState 
    FROM Families f    
```

**Sonuçlar**  

```json
    [
      {
        "CityState": [
          "seattle", 
          "WA"
        ]
      }, 
      {
        "CityState": [
          "NY", 
          "NY"
        ]
      }
    ]
```

## <a id="ValueKeyword"></a>VALUE anahtar sözcüğü
**Değer** anahtar sözcüğü, JSON değeri döndürmek için bir yol sağlar. Örneğin, aşağıda gösterilen sorguyu skaler döndürür `"Hello World"` yerine `{$1: "Hello World"}`.

**Sorgu**

```sql
    SELECT VALUE "Hello World"
```

**Sonuçlar**

```json
    [
      "Hello World"
    ]
```

Aşağıdaki sorgu olmayan JSON değerini döndürür `"address"` sonuçlarında etiketi.

**Sorgu**

```sql
    SELECT VALUE f.address
    FROM Families f    
```

**Sonuçlar**  

```json
    [
      {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }, 
      {
        "state": "NY", 
        "county": "Manhattan", 
        "city": "NY"
      }
    ]
```

Aşağıdaki örnek JSON basit değerlerin (yaprak düzey JSON ağacı) iade işlemini göstermek için bu genişletir. 

**Sorgu**

```sql
    SELECT VALUE f.address.state
    FROM Families f    
```

**Sonuçlar**

```json
    [
      "WA",
      "NY"
    ]
```

## <a name="-operator"></a>* İşleci
Özel işleci (*) belgesi olarak projeye desteklenen-olduğu. Kullanıldığında yansıtılan tek alan olması gerekir. While gibi bir sorguda `SELECT * FROM Families f` geçerli `SELECT VALUE * FROM Families f ` ve `SELECT *, f.id FROM Families f ` geçerli değildir.

**Sorgu**

```sql
    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"
```

**Sonuçlar**

```json
    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]
```

## <a id="TopKeyword"></a>TOP işleci
Üst anahtar sözcüğü, değerleri sorgudan sayısını sınırlamak için kullanılabilir. ÜST ORDER BY yan tümcesi ile birlikte kullanıldığında, sonuç kümesinin sıralı değerleri ilk N sayıya sınırlıdır; Aksi takdirde, tanımlanmamış bir sırada ilk N sonuç sayısını döndürür. En iyi uygulama, bir SELECT deyimi bir ORDER BY yan tümcesi her zaman ile TOP yan tümcesini kullanın. Hangi satır üst tarafından etkilenen tahmin edilebilir bir biçimde belirtmek için tek yolu budur. 

**Sorgu**

```sql
    SELECT TOP 1 * 
    FROM Families f 
```

**Sonuçlar**

```json
    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]
```

ÜST veya bir değişken değeri parametreli sorgular kullanma (yukarıda gösterildiği gibi) bir sabit değer ile kullanılabilir. Daha fazla ayrıntı için lütfen aşağıdaki parametreli sorgular bakın.

## <a id="Aggregates"></a>Toplama işlevleri
Toplamaları de gerçekleştirebilirsiniz `SELECT` yan tümcesi. Toplama işlevleri, bir değerler kümesi üzerinde bir hesaplama gerçekleştirmek ve tek bir değer döndürür. Örneğin, aşağıdaki sorguyu koleksiyonundaki ailesi belgelerin sayısını döndürür.

**Sorgu**

```sql
    SELECT COUNT(1) 
    FROM Families f 
```

**Sonuçlar**

```json
    [{
        "$1": 2
    }]
```

Kullanarak ayrıca toplamanın skaler değer döndürebilir `VALUE` anahtar sözcüğü. Örneğin, aşağıdaki sorgu, tek bir sayı değerlerin sayısını döndürür:

**Sorgu**

```sql
    SELECT VALUE COUNT(1) 
    FROM Families f 
```

**Sonuçlar**

```json
    [ 2 ]
```

Filtrelerle birlikte toplamalar de gerçekleştirebilirsiniz. Örneğin, aşağıdaki sorgu Washington eyaleti adresiyle belgelerin sayısını döndürür.

**Sorgu**

```sql
    SELECT VALUE COUNT(1) 
    FROM Families f
    WHERE f.address.state = "WA" 
```

**Sonuçlar**

```json
    [ 1 ]
```

Aşağıdaki tabloda, SQL API'SİNDE desteklenen toplama işlevleri listesini gösterir. `SUM` ve `AVG` ise sayısal değer üzerinde gerçekleştirilen `COUNT`, `MIN`, ve `MAX` sayılar, dizeler, Boole değerleri ve null değerlere gerçekleştirilebilir. 

| Kullanım | Açıklama |
|-------|-------------|
| SAYI | İfade öğe sayısını döndürür. |
| TOPLA   | İfadedeki tüm değerlerin toplamını döndürür. |
| EN DÜŞÜK   | İfadedeki en küçük değeri döndürür. |
| EN YÜKSEK   | İfadedeki en büyük değeri döndürür. |
| ORT   | İfadedeki değerlerin ortalamasını döndürür. |

Toplamlar, ayrıca bir dizi yineleme sonuçları üzerinde gerçekleştirilebilir. Daha fazla bilgi için [dizi yineleme sorgularda](#Iteration).

> [!NOTE]
> Azure portalında Veri Gezgini kullanırken, toplama sorguları bir sorgu sayfası kısmen toplanan sonuçlarda döndürebilir unutmayın. SDK'ları, tüm sayfalara tek bir toplu değer oluşturur. 
> 
> Kod kullanarak toplama sorguları gerçekleştirmek için .NET SDK'sı 1.12.0, .NET Core SDK'sı 1.1.0 veya Java SDK'sı 1.9.5 gerekir veya üzeri.    
>

## <a id="OrderByClause"></a>ORDER BY yan tümcesi
ANSI-SQL'de sorgulanırken isteğe bağlı bir Order By yan tümcesi ekleyebilirsiniz gibi. Yan tümcesi, sonuçlar alınması gereken sırayı belirtmek için isteğe bağlı ASC/DESC bağımsız değişken içerebilir.

Örneğin, yerleşik şehir adı sırasını ailelerinde alan bir sorgu aşağıdadır.

**Sorgu**

```sql
    SELECT f.id, f.address.city
    FROM Families f 
    ORDER BY f.address.city
```

**Sonuçlar**

```json
    [
      {
        "id": "WakefieldFamily",
        "city": "NY"
      },
      {
        "id": "AndersenFamily",
        "city": "Seattle"    
      }
    ]
```

Ve ailelerinde dönem temsil eden bir sayı olarak depolanan oluşturulma tarihi sırasını alır. bir sorgu süresi, yani, 1 Ocak 1970 bu yana geçen süreyi saniye aşağıda verilmiştir.

**Sorgu**

```sql
    SELECT f.id, f.creationDate
    FROM Families f 
    ORDER BY f.creationDate DESC
```

**Sonuçlar**

```json
    [
      {
        "id": "WakefieldFamily",
        "creationDate": 1431620462
      },
      {
        "id": "AndersenFamily",
        "creationDate": 1431620472    
      }
    ]
```

## <a id="Advanced"></a>Gelişmiş veritabanı kavramlarını ve SQL sorguları

### <a id="Iteration"></a>Yineleme
Aracılığıyla eklenen yeni bir yapısı **IN** SQL API'si, JSON diziler yineleme için destek sağlamak üzere anahtar sözcük. FROM kaynak yineleme için destek sağlar. Aşağıdaki örnek ile başlayalım:

**Sorgu**

```sql
    SELECT * 
    FROM Families.children
```

**Sonuçlar**  

```json
    [
      [
        {
          "firstName": "Henriette Thaulow", 
          "gender": "female", 
          "grade": 5, 
          "pets": [{ "givenName": "Fluffy"}]
        }
      ], 
      [
        {
            "familyName": "Merriam", 
            "givenName": "Jesse", 
            "gender": "female", 
            "grade": 1
        }, 
        {
            "familyName": "Miller", 
            "givenName": "Lisa", 
            "gender": "female", 
            "grade": 8
        }
      ]
    ]
```

Şimdi, koleksiyondaki alt öğeleri üzerinde yineleme gerçekleştiren başka bir sorgu göz atalım. Çıkış dizisinde farka dikkat edin. Bu örnekte böler `children` ve tek bir dizide sonuçları düzleştirir.  

**Sorgu**

```sql
    SELECT * 
    FROM c IN Families.children
```

**Sonuçlar**  

```json
    [
      {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy" }]
      },
      {
          "familyName": "Merriam",
          "givenName": "Jesse",
          "gender": "female",
          "grade": 1
      },
      {
          "familyName": "Miller",
          "givenName": "Lisa",
          "gender": "female",
          "grade": 8
      }
    ]
```

Bu ek aşağıdaki örnekte gösterildiği gibi dizi her bir giriş filtrelemek için kullanılabilir:

**Sorgu**

```sql
    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8
```

**Sonuçlar**  

```json
    [{
      "givenName": "Lisa"
    }]
```

Ayrıca, bir dizi yineleme sonucun üzerine toplama gerçekleştirebilirsiniz. Örneğin, aşağıdaki sorgu tüm aileleri arasında alt öğeyi sayar.

**Sorgu**

```sql
    SELECT COUNT(child) 
    FROM child IN Families.children
```

**Sonuçlar**  

```json
    [
      { 
        "$1": 3
      }
    ]
```

### <a id="Joins"></a>Birleşimler
İlişkisel bir veritabanında tabloları arasında birleştirme için gereken büyük/küçük harf önemlidir. Bu, normalleştirilmiş şemaları tasarlamaya mantıksal corollary olur. Bunun aksine, SQL API'si şemasız belgelerin normalleştirilmişlikten çıkarılmış veri modeli ile ilgilidir. Bu mantıksal eşdeğeri olan bir "kendi kendine birleşme".

< From_source1 > JOIN < from_source2 > birleştirme dilin desteklediği sözdizimi aşağıdaki gibidir... < From_sourceN > katılın. Genel olarak, bu bir dizi döndürür **N**- tanımlama grubu (olan tanımlama grubu **N** değerler). Her bir tanımlama grubunu tüm koleksiyon diğer adları kendi ilgili ayarlar yineleme tarafından üretilen değerler içeriyor. Diğer bir deyişle, tam bir çapraz ürün birleştirme işleminde katılan kümelerinin budur.

Aşağıdaki örnekler, JOIN yan tümcesi nasıl çalıştığını gösterir. Aşağıdaki örnekte, her kaynak belgedeki vektörel çarpımını beri sonucu boştur ve boş boştur.

**Sorgu**

```sql
    SELECT f.id
    FROM Families f
    JOIN f.NonExistent
```

**Sonuçlar**  

```json
    [{
    }]
```

Aşağıdaki örnekte, birleştirme arasında belge kökü olan ve `children` subroot. Bu, iki JSON nesnesi arasında çapraz bir üründür. Biz alt dizi için tek bir kök çalışıyorsanız bu yana alt dizi hale birleştirme işleminde etkili değildir. Bu nedenle tam olarak tek bir belge her dizi belgeyle çapraz çarpımını üretir beri sonucu yalnızca iki sonuçlarını içerir.

**Sorgu**

```sql
    SELECT f.id
    FROM Families f
    JOIN f.children
```

**Sonuçlar**

```json
    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]
```

Aşağıdaki örnek, daha geleneksel bir birleştirme gösterir:

**Sorgu**

```sql
    SELECT f.id
    FROM Families f
    JOIN c IN f.children 
```

**Sonuçlar**

```json
    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]
```

Dikkat edilecek ilk şey olan `from_source` , **katılın** yan tümcesi ise bir yineleyici. Bu nedenle, akışı bu durumda şu şekildedir:  

* Her alt öğeyi Genişlet **c** dizi.
* Bir çapraz ürün belgenin kökü ile geçerli **f** her alt öğe **c** ilk adımda düzleştirilmiş.
* Son olarak, kök nesnenin proje **f** tek başına bir özellik olarak adlandırın. 

İlk belge (`AndersenFamily`) yalnızca bir alt öğesi içerir, dolayısıyla yalnızca bu belgeye karşılık gelen tek bir nesne sonuç kümesini içerir. İkinci belgeyi (`WakefieldFamily`) iki alt öğeleri içerir. Bu nedenle, çapraz böylece bu belgeye karşılık gelen her alt için iki nesne sonuçta her bir alt için ayrı bir nesne oluşturur. Bir çapraz ürün beklediğiniz gibi hem bu belgeleri kök alanları aynıdır.

Birleştirme gerçek faydası, aksi takdirde projeye zor olan şekle çapraz ürün form diziler için arasındadır. Ayrıca, aşağıdaki örnekte görüyoruz gibi sağlar tanımlama grubu tarafından genel koşullar karşılanırsa bir koşul kullanıcının seçtiği bir tanımlama grubu birleşimi üzerinde filtre uygulayabilirsiniz.

**Sorgu**

```sql
    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
```

**Sonuçlar**

```json
    [
      {
        "familyName": "AndersenFamily", 
        "childFirstName": "Henriette Thaulow", 
        "petName": "Fluffy"
      }, 
      {
        "familyName": "WakefieldFamily", 
        "childGivenName": "Jesse", 
        "petName": "Goofy"
      }, 
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]
```

Bu örnek, önceki örnekte doğal bir uzantısıdır ve double JOIN gerçekleştirir. Bu nedenle, aşağıdaki sözde kod olarak çapraz görüntülenebilir:

```
    for-each(Family f in Families)
    {    
        for-each(Child c in f.children)
        {
            for-each(Pet p in c.pets)
            {
                return (Tuple(f.id AS familyName, 
                  c.givenName AS childGivenName, 
                  c.firstName AS childFirstName,
                  p.givenName AS petName));
            }
        }
    }
```

`AndersenFamily` bir evcil hayvan olan bir alt öğesi var. Bu nedenle, çapraz bir satır üretir (1\*1\*1) bu aile öğesinden. WakefieldFamily ancak iki alt öğe, ancak yalnızca bir alt "Jesse" Evcil Hayvanlar içeriyor. Jesse iki Evcil Hayvanlar yine de vardır. Bu nedenle çapraz 1 verir\*1\*2 = 2, bu ailesinden satırlar.

Sonraki örnekte olduğundan bir ek filtre `pet`. Bu, evcil hayvan adı "Gölge" olduğu tüm diziler dışlar. Biz diziler dizilerden tanımlama grubu öğelerinin herhangi bir filtre oluşturun ve öğeleri herhangi bir birleşimini proje olduğuna dikkat edin. 

**Sorgu**

```sql
    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"
```

**Sonuçlar**

```json
    [
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]
```

## <a id="JavaScriptIntegration"></a>JavaScript tümleştirme
Azure Cosmos DB, JavaScript tabanlı uygulama mantığını saklı yordamlar ve tetikleyicilerle açısından koleksiyonlar üzerinde doğrudan yürütmek için bir programlama modeli sağlar. Bu, hem sağlar:

* Yüksek performanslı işlem CRUD işlemleri ve belgeleri bir koleksiyonda doğrudan veritabanı altyapısının içinde JavaScript çalışma zamanı derin tümleştirmesi sayesinde sorguları olanağı. 
* Denetim akışı, değişken kapsamı, atama ve özel durum işleme temelleri veritabanı işlemleriyle tümleştirme doğal bir model. JavaScript tümleştirme için Azure Cosmos DB desteği hakkında daha fazla ayrıntı için lütfen JavaScript'i sunucu tarafı programlama belgelerine başvurun.

### <a id="UserDefinedFunctions"></a>Kullanıcı tanımlı işlevler (UDF'ler)
Bu makalede önceden tanımlı türleri ile birlikte, SQL API'si, kullanıcı tanımlı işlevler (UDF) için destek sağlar. Özellikle, burada geliştiriciler sıfır veya daha fazla bağımsız değişken geçirme ve geri tek bağımsız değişken sonuç skaler UDF'ler desteklenir. Bu değişkenin her biri, geçerli JSON değerleri olan için denetlenir.  

SQL söz dizimini kullanarak bu kullanıcı tanımlı işlevler özel uygulama mantığını destekleyecek şekilde genişletilir. UDF SQL API'sine kaydedilebilir ve sonra bir SQL sorgusunun bir parçası başvurulamaz. Aslında, UDF'ler sorgular tarafından çağrılacak exquisitely tasarlanmıştır. Bu seçenek için bir corollary UDF'ler JavaScript türlerinde (saklı yordamları ve Tetikleyicileri) olan bağlam nesnesi için erişiminiz yok. Salt okunur olarak sorgular yürütün olduğundan, birincil veya ikincil çoğaltmaları çalıştırabilirsiniz. Bu nedenle, UDF'ler, diğer JavaScript türlerinin aksine, ikincil çoğaltmalar üzerinde çalışacak şekilde tasarlanmıştır.

Cosmos DB veritabanını, özellikle bir belge koleksiyonu, bir UDF nasıl kaydedilebilir bir örnek aşağıdadır.

```javascript
       UserDefinedFunction regexMatchUdf = new UserDefinedFunction
       {
           Id = "REGEX_MATCH",
           Body = @"function (input, pattern) { 
                       return input.match(pattern) !== null;
                   };",
       };

       UserDefinedFunction createdUdf = client.CreateUserDefinedFunctionAsync(
           UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
           regexMatchUdf).Result;  
```

Yukarıdaki örnekte, adı olan bir UDF oluşturur `REGEX_MATCH`. İki JSON dizesi değerini kabul `input` ve `pattern` ve ilk eşleşme ikinci desen belirtilmişse denetimleri kullanarak JavaScript'in string.match() işlevi.

Artık bu UDF sorguda projeksiyon kullanabiliriz. UDF büyük/küçük harfe "udf." öneki ile tam olarak nitelenmiş olmalıdır sorgulara çağrılır olduğunda. 

> [!NOTE]
> 3/17/2015 tarihinden önce Cosmos DB UDF çağrıları "udf." olmadan desteklenir. ön eki seçin REGEX_MATCH() ister. Bu arama deseni kullanım dışıdır.  
> 
> 

**Sorgu**

```sql
    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families
```

**Sonuçlar**

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

UDF de bir filtre içinde de "udf ile." nitelenmiş aşağıdaki örnekte gösterildiği gibi kullanılabilir ön eki:

**Sorgu**

```sql
    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")
```

**Sonuçlar**

```json
    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]
```

Esas olarak, UDF'ler geçerli skaler ifadeler ve izdüşümler ve filtreler kullanılabilir. 

UDF gücüyle genişletmek için başka bir örneğe koşullu mantığı ile bakalım:

```javascript
       UserDefinedFunction seaLevelUdf = new UserDefinedFunction()
       {
           Id = "SEALEVEL",
           Body = @"function(city) {
                   switch (city) {
                       case 'seattle':
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
                UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
                seaLevelUdf);
```

UDF sınayan bir örnek aşağıda verilmiştir.

**Sorgu**

```sql
    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f    
```

**Sonuçlar**

```json
     [
      {
        "city": "seattle", 
        "seaLevel": 520
      }, 
      {
        "city": "NY", 
        "seaLevel": 410
      }
    ]
```

Önceki örneklerde göstermek gibi UDF'ler JavaScript dilinin gücünü yerleşik JavaScript çalışma zamanı özelliklerinden yardımıyla karmaşık yordam, koşullu mantık yapmak için zengin bir programlanabilir arabirim sağlamak üzere SQL API'si ile tümleştirin.

SQL API'si bağımsız değişkenler için UDF'ler kaynağındaki her belge için UDF işleme geçerli aşamada (WHERE yan tümcesi veya SELECT yan tümcesi) sağlar. Sonuç genel yürütme işlem hattında sorunsuz bir şekilde eklenmiştir. Başvurulan özellikleri tarafından UDF parametreleri JSON değeri kullanılabilir değil, parametre olarak kabul edilir tanımlı değil ve bu nedenle UDF çağırma tamamen atlandı. Benzer şekilde UDF sonucu tanımlanmamış ise, sonuçta bulunmaz. 

Özet olarak, karmaşık iş mantığı sorgunun bir parçası yapmak için harika Araçlar UDF'ler var.

### <a name="operator-evaluation"></a>Değerlendirme işleci
Cosmos DB, parallels JavaScript işleçleri ve kendi değerlendirme semantiğine sahip bir JSON veritabanı oluş virtue tarafından çizer. Cosmos DB, JSON desteği açısından JavaScript sematiğini korumak dener, ancak bazı durumlarda işlemi değerlendirme farklıdır.

Değerleri veritabanından kadar SQL API'SİNDE aksine geleneksel SQL değer türleri bilinen değildir. Verimli bir şekilde sorguları yürütmek için işleçlerin en katı tür gereksinimleri vardır. 

SQL API'si, JavaScript aksine, örtük dönüştürmelerin gerçekleştirmez. Örneğin, bir sorgu ister `SELECT * FROM Person p WHERE p.Age = 21` eşleşen değeri olan 21 bir geçerlilik süresi özelliği içeren belgeleri. İster "021", "21.0", "0021", "00021" dize "21", veya diğer büyük olasılıkla sınırsız değişimleri, geçerlilik süresi özelliği eşleşen herhangi bir belge, vb. eşlenemiyor. Bu dize değerleri nerede örtük olarak sayıya Integer JavaScript için buna yapılır (örn, operatörüne göre: ==). Bu seçenek, SQL API'SİNDE eşleşen verimli dizini için önemlidir. 

## <a name="parameterized-sql-queries"></a>Parametreli SQL sorguları
Cosmos DB ile tanıdık ifade parametrelerle sorguları destekler \@ gösterimi. Parametreli SQL sağlam işleme ve kullanıcı girişi, SQL ekleme üzerinden verilerin yanlışlıkla açığa çıkmaya önleme, kaçış sağlar. 

Örneğin, son adı ve adresi durum parametreleri alan bir sorgu yazma ve çeşitli değerleri son adı ve kullanıcı girişini temel alarak adresi durumunun yürütün.

```sql
    SELECT * 
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState
```

Bu istek ardından Cosmos DB gibi bir JSON sorgusu olarak aşağıda gösterilen gönderilebilir.

```sql
    {      
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",     
        "parameters": [          
            {"name": "@lastName", "value": "Wakefield"},         
            {"name": "@addressState", "value": "NY"},           
        ] 
    }
```

İLK bağımsız değişkeni parametreli sorgular gibi kullanarak aşağıda gösterilen ayarlanabilir.

```sql
    {      
        "query": "SELECT TOP @n * FROM Families",     
        "parameters": [          
            {"name": "@n", "value": 10},         
        ] 
    }
```

Parametre değerleri geçerli bir JSON olabilir (dizeler, sayılar ve Boole değerlerini, null, hatta diziler veya JSON iç içe geçmiş). Ayrıca parametreleri Cosmos DB, şemasız olduğundan, karşı herhangi bir tür doğrulanmaz.

## <a id="BuiltinFunctions"></a>Yerleşik işlevler
Cosmos DB, kullanıcı tanımlı işlevler (UDF'ler) gibi sorguları içinde kullanılan yaygın işlemleri için çok sayıda yerleşik işlev de destekler.

| İşlev grubu          | İşlemler                                                                                                                                          |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| Matematik işlevleri  | ABS, TAVAN, EXP, FLOOR, günlük, LOG10, güç, HEPSİNİ, oturum, SQRT, KARE, TRUNC, ACOS, ASIN, ATAN, ATN2, COS, COT, derece, PI, RADIANS, SIN ve TAN |
| Tür denetimini işlevleri | IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED ve IS_PRIMITIVE                                                           |
| Dize işlevleri        | CONCAT, içerir, ENDSWITH, INDEX_OF, sol, UZUNLUĞU, alt, LTRIM, Değiştir, çoğaltma, ters, sağ, RTRIM, STARTSWITH, alt ve üst       |
| Dizi işlevleri         | ARRAY_CONCAT, ARRAY_CONTAINS ARRAY_LENGTH ve ARRAY_SLICE                                                                                         |
| Uzamsal İşlevler       | ST_DISTANCE, ST_WITHIN ST_INTERSECTS ST_ISVALID ve ST_ISVALIDDETAILED                                                                           | 

Şu an için yerleşik işlev kullanılabildiği artık bir kullanıcı tanımlı işlev (UDF) kullanıyorsanız, çalıştırmak hızlı gitme gibi karşılık gelen yerleşik işlev kullanmalıdır ve daha verimli bir şekilde. 

### <a name="mathematical-functions"></a>Matematik işlevleri
Matematiksel işlevler her bağımsız değişken olarak sağlanan ve sayısal bir değer döndürmesi giriş değerlerini temel alan, bir hesaplama gerçekleştirir. Desteklenen yerleşik matematiksel işlevler tablosu aşağıdadır.


| Kullanım | Açıklama |
|----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [[ABS (num_expr)](#bk_abs) | Belirtilen sayısal ifade (pozitif) mutlak değerini döndürür. |
| [TAVAN (num_expr)](#bk_ceiling) | Büyüktür veya eşittir, belirtilen sayısal ifadenin en küçük tamsayı değerini döndürür. |
| [KATI (num_expr)](#bk_floor) | Belirtilen sayısal ifade küçük veya eşit en büyük tamsayı döndürür. |
| [EXP (num_expr)](#bk_exp) | Belirtilen sayısal ifadenin üssünü döndürür. |
| [Günlük (num_expr [, temel])](#bk_log) | Belirtilen sayısal ifade veya kullanarak belirtilen tabanda logaritmasını doğal logaritmasını döndürür |
| [Log10 (num_expr)](#bk_log10) | 10 tabanında Logaritmik belirtilen sayısal ifadenin değerini döndürür. |
| [ROUND (num_expr)](#bk_round) | En yakın tamsayı değerine yuvarlanır sayısal bir değer döndürür. |
| [TRUNC (num_expr)](#bk_trunc) | En yakın tamsayı değerine kesilmiş sayısal bir değer döndürür. |
| [SQRT (num_expr)](#bk_sqrt) | Belirtilen sayısal ifadenin kare kökünü döndürür. |
| [KARE (num_expr)](#bk_square) | Belirtilen sayısal ifade karesini döndürür. |
| [GÜÇ (num_expr, num_expr)](#bk_power) | Belirtilen sayısal ifade gücünü belirtilen değeri döndürür. |
| [OTURUM (num_expr)](#bk_sign) | Oturum (-1, 0, 1) belirtilen sayısal ifadenin değerini döndürür. |
| [ACOS (num_expr)](#bk_acos) | Kosinüsü belirtilen sayısal ifadesidir radyan cinsinden açı döndürür; arkkosinüsünü olarak da adlandırılır. |
| [ASIN (num_expr)](#bk_asin) | Açının sinüsü belirtilen sayısal ifadesidir radyan cinsinden döndürür. Bu arksinüsünü olarak da adlandırılır. |
| [ATAN (num_expr)](#bk_atan) | Tanjantı belirtilen sayısal ifadesidir radyan cinsinden açı döndürür. Bu arktanjantını olarak da adlandırılır. |
| [ATN2 (num_expr)](#bk_atn2) | Burada açıyı pozitif x ekseni ve kaynaktan ray noktasına (y, x) arasında radyan cinsinden döndürür x ve y iki belirtilen float ifadelerin değerlerdir. |
| [COS (num_expr)](#bk_cos) | Radyan cinsinden belirtilen ifade trigonometrik belirtilen bir açının kosinüsünü döndürür. |
| [COT (num_expr)](#bk_cot) | Trigonometrik belirtilen bir açının kotanjantını radyan cinsinden belirtilen bir sayısal ifade döndürür. |
| [DERECE (num_expr)](#bk_degrees) | Karşılık gelen açıyı derece için radyan cinsinden belirtilen bir açı cinsinden döndürür. |
| [PI ()](#bk_pi) | PI sayısının sabit değerini döndürür. |
| [RADYAN (num_expr)](#bk_radians) | Derece sayısal bir ifadenin girildiğinde radyan cinsinden döndürür. |
| [SIN (num_expr)](#bk_sin) | Radyan cinsinden belirtilen ifade trigonometrik belirtilen bir açının sinüsünü döndürür. |
| [TAN (num_expr)](#bk_tan) | Belirtilen ifadedeki giriş ifadesi tanjantını döndürür. |

Örneğin, artık aşağıdakiler gibi sorguları çalıştırabilirsiniz:

**Sorgu**

```sql
    SELECT VALUE ABS(-4)
```

**Sonuçlar**

```json
    [4]
```
Cosmos DB'nin işlevler ANSI SQL'e kıyasla arasındaki temel fark, bunlar şemasız ve karışık şema verilerle iyi çalışacak şekilde tasarlanmıştır ' dir. Örneğin, burada Size özelliği eksik veya sahip bir belge varsa, "Bilinmeyen" gibi bir sayısal olmayan değer sonra belge üzerinde bir hata döndürmek yerine atlanır.

### <a name="type-checking-functions"></a>Tür denetimini işlevleri
Tür denetimi işlevleri SQL sorgusu içindeki bir ifadenin türü denetlemenizi sağlar. Tür denetleme işlevleri, değişken veya bilinmeyen olduğunda hareket halindeyken belgelerde özellikleri türünü belirlemek için kullanılabilir. Desteklenen yerleşik tür denetimini işlevler tablosu aşağıdadır.

<table>
<tr>
  <td><strong>Kullanım</strong></td>
  <td><strong>Açıklama</strong></td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (ifade)</a></td>
  <td>Değer türü bir dizi olup olmadığını gösteren bir Boole değeri döndürür.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (ifade)</a></td>
  <td>Bir Boolean değer türü olup olmadığını gösteren bir Boole değeri döndürür.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (ifade)</a></td>
  <td>Değerin türü null olup olmadığını gösteren bir Boole değeri döndürür.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (ifade)</a></td>
  <td>Değerin türü, bir sayı olup olmadığını gösteren bir Boole değeri döndürür.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (ifade)</a></td>
  <td>Değerin türü, bir JSON nesnesi olup olmadığını gösteren bir Boole değeri döndürür.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (ifade)</a></td>
  <td>Bir dize değerinin türü olup olmadığını gösteren bir Boole değeri döndürür.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (ifade)</a></td>
  <td>Özellik değeri atanıp atanmadığını gösteren bir Boole değeri döndürür.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (ifade)</a></td>
  <td>Değer türü bir dize, sayı, Boole veya null olup olmadığını gösteren bir Boole değeri döndürür.</td>
</tr>

</table>

Bu işlevleri kullanarak, artık aşağıdakiler gibi sorguları çalıştırabilirsiniz:

**Sorgu**

```sql
    SELECT VALUE IS_NUMBER(-4)
```

**Sonuçlar**

```json
    [true]
```

### <a name="string-functions"></a>Dize işlevleri
Aşağıdaki skaler İşlevler, bir dize giriş değeri bir işlem gerçekleştirmek ve bir dize, sayısal veya Boolean değeri döndürür. Yerleşik dize işlevleri tablosu şu şekildedir:

| Kullanım | Açıklama |
| --- | --- |
| [UZUNLUK (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_length) |Belirtilen dize ifadesinin karakter sayısını döndürür |
| [CONCAT (str_expr str_expr [, str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat) |İki veya daha fazla dize değerlerini birleştirirken sonucu olan bir dize döndürür. |
| [Alt dize (str_expr, num_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_substring) |Parçası olan bir dize ifadesi döndürür. |
| [STARTSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_startswith) |Boole döndürüp döndüremeyeceğini belirten döndürür ilk dize ifade olup olmadığını ve ikinci başlatır |
| [ENDSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_endswith) |Boole döndürüp döndüremeyeceğini belirten döndürür ilk dize ifade olup olmadığını ve ikinci sona erer |
| [İÇERİR (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_contains) |Döndürür bir Boolean gösteren ikinci ilk dize ifade olup olmadığını içerir. |
| [INDEX_OF (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_index_of) |İkinci dizenin başlangıç konumunu döndürür dize bulunamazsa, ilk belirtilen dize ifadesi veya -1 içindeki ifadenin dize. |
| [Sol (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_left) |Belirtilen sayıda karakteri içeren bir dize sol bölümünü döndürür. |
| [SAĞ (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_right) |Belirtilen sayıda karakteri içeren bir dize sağ bölümünü döndürür. |
| [LTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ltrim) |Baştaki boşluklar kaldırdıktan sonra bir dize ifadesi döndürür. |
| [RTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_rtrim) |Sonundaki tüm boşlukları kesilmesi sonrasında bir dize ifadesi döndürür. |
| [DÜŞÜK (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_lower) |Büyük harf karakter verileri küçük harfe dönüştürmenin sonra bir dize ifadesi döndürür. |
| [ÜST (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_upper) |Küçük harf karakter verileri büyük harfe dönüştürmenin sonra bir dize ifadesi döndürür. |
| [Değiştir (str_expr, str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replace) |Belirtilen dize değeri tüm oluşumlarını başka bir dize değeri ile değiştirir. |
| [Çoğaltma (str_expr, num_expr)](https://docs.microsoft.com/azure/cosmos-db/sql-api-sql-query-reference#bk_replicate) |Bir dize değeri, belirtilen sayıda yineler. |
| [REVERSE (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_reverse) |Bir dize değerinin ters sırada döndürür. |

Bu işlevlerin kullanılması, artık aşağıdakiler gibi sorguları çalıştırabilirsiniz. Örneğin, aile adı büyük harfle yazılmış şu şekilde döndürebilirsiniz:

**Sorgu**

```sql
    SELECT VALUE UPPER(Families.id)
    FROM Families
```

**Sonuçlar**

```json
    [
        "WAKEFIELDFAMILY", 
        "ANDERSENFAMILY"
    ]
```

Veya bu örnekteki gibi dizeyi art arda ekler:

**Sorgu**

```sql
    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families
```

**Sonuçlar**

```json
    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "seattle,WA"
    }]
```

Dize işlevleri, aşağıdaki örnekte gibi sonuçları filtrelemek için WHERE yan tümcesinde de kullanılabilir:

**Sorgu**

```sql
    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")
```

**Sonuçlar**

```json
    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]
```

### <a name="array-functions"></a>Dizi işlevleri
Aşağıdaki skaler işlevler bir dizi giriş değeri ve dönüş sayısal, Boole veya dizi değeri üzerinde bir işlem gerçekleştirin. Yerleşik bir dizi işlev bir tablo şu şekildedir:

| Kullanım | Açıklama |
| --- | --- |
| [ARRAY_LENGTH (arr_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_length) |Belirtilen bir dizi ifadesinin öğelerin sayısını döndürür. |
| [ARRAY_CONCAT (arr_expr arr_expr [, arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat) |İki veya daha fazla dizi değerlerini birleştirirken sonucu olan bir dizi döndürür. |
| [ARRAY_CONTAINS (arr_expr, ifade [, bool_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains) |Dizi belirtilen değeri içerip içermediğini gösteren bir Boole değeri döndürür. Tam veya kısmi eşleşme olup olmadığını belirtebilirsiniz. |
| [ARRAY_SLICE (arr_expr num_expr [, num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice) |Bir dizi ifadesi bölümünü döndürür. |

Dizi işlevleri dizileri JSON içinde işlemek için kullanılabilir. Örneğin, bir üst öğeyi "Robin Wakefield" olduğu tüm belgeleri döndüren bir sorgu aşağıdadır. 

**Sorgu**

```sql
    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })
```

**Sonuçlar**

```json
    [{
      "id": "WakefieldFamily"
    }]
```

Diziden öğeleri eşleştirmek için kısmi bir parçası olarak belirtebilirsiniz. Aşağıdaki sorgu tüm üst bulur `givenName` , `Robin`.

**Sorgu**

```sql
    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin" }, true)
```

**Sonuçlar**

```json
    [{
      "id": "WakefieldFamily"
    }]
```

Alt öğe sayısı ailesi başına almak için ARRAY_LENGTH kullanan başka bir örnek aşağıda verilmiştir.

**Sorgu**

```sql
    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 
```

**Sonuçlar**

```json
    [{
      "id": "WakefieldFamily",
      "numberOfChildren": 2
    },
    {
      "id": "AndersenFamily",
      "numberOfChildren": 1
    }]
```

### <a name="spatial-functions"></a>Uzamsal İşlevler
Cosmos DB, Jeo-uzamsal sorgulamak için aşağıdaki açık Jeo-uzamsal Consortium (OGC) yerleşik işlevleri destekler. 

<table>
<tr>
  <td><strong>Kullanım</strong></td>
  <td><strong>Açıklama</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (point_expr, point_expr)</td>
  <td>İki GeoJSON noktası, çokgen veya LineString ifadeler uzaklığı döndürür.</td>
</tr>
<tr>
  <td>ST_WITHIN (point_expr, polygon_expr)</td>
  <td>İlk GeoJSON nesne (noktası, çokgen veya LineString) ikinci GeoJSON nesne içinde (noktası, çokgen veya LineString) olup olmadığını gösteren bir Boole ifadesi döndürür.</td>
</tr>
<tr>
  <td>ST_INTERSECTS (spatial_expr, spatial_expr)</td>
  <td>İki belirtilen GeoJSON nesnesi (noktası, çokgen veya LineString) kesişen olup olmadığını belirten bir Boole ifadesi döndürür.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Belirtilen GeoJSON noktası, çokgen veya LineString ifade geçerli olup olmadığını gösteren bir Boole değeri döndürür.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Bir Boole değeri içeren bir JSON değeri, belirtilen GeoJSON noktası, çokgen veya LineString ifade geçerliyse ve geçersiz değeri döndürür, ayrıca bir dize değeri olarak nedeni.</td>
</tr>
</table>

Uzamsal İşlevler, uzamsal veri yakınlık sorguları gerçekleştirmek için kullanılabilir. Örneğin, ST_DISTANCE yerleşik işlevi kullanarak belirtilen konumun içinde 30 KM ailesi tüm belgeleri döndüren bir sorgu aşağıdadır. 

**Sorgu**

```sql
    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000
```

**Sonuçlar**

```json
    [{
      "id": "WakefieldFamily"
    }]
```

Cosmos DB'de Jeo-uzamsal destek hakkında daha fazla ayrıntı için lütfen bkz [Azure Cosmos DB Jeo-uzamsal verilerle çalışmaya](geospatial.md). Cosmos DB için uzamsal işlevler ve SQL söz dizimi sonuna geldik. Şimdi nasıl çalıştığını ve söz dizimi ile nasıl etkileşime gireceğini sorgulama LINQ şimdiye gördük göz atalım.

## <a id="Linq"></a>LINQ to SQL API'si
LINQ sorguları akışlarında nesnelerin olarak hesaplama ifade bir .NET programlama modelidir. Cosmos DB, JSON ve .NET nesneleri ve LINQ sorguları kümesini bir eşleme Cosmos DB sorgular arasındaki bir dönüştürmenin kolaylaştırarak LINQ ile arabirim oluşturmak için bir istemci tarafı kitaplık sağlar. 

Aşağıdaki resimde, Cosmos DB kullanarak LINQ sorgularını destekleyen mimarisi gösterilmektedir.  Cosmos DB istemci kullanarak geliştiriciler oluşturabilirsiniz bir **Iqueryable** doğrudan ardından Cosmos DB bir sorguda LINQ sorgusu çevirir Cosmos DB sorgu sağlayıcısı sorgular nesne. Sorgu, daha sonra bir JSON biçiminde sonuç kümesi almak için Cosmos DB sunucuya geçirilir. Ve döndürülen sonuçlarda akışı istemci tarafında .NET nesnelerini seri durumdan.

![SQL söz dizimi, JSON sorgu dili, veritabanı kavramlarını ve SQL sorguları - SQL API'sini kullanarak LINQ sorgularını destekleyen mimarisi][1]

### <a name="net-and-json-mapping"></a>.NET ve JSON eşleme
.NET nesneleri ile JSON belgelerini arasındaki eşlemeyi doğal - burada nesne "anahtarını" bölümüne alan adı eşlenir ve "value" bölümü eşlenmiş nesne değeri parçası yinelemeli olarak bir JSON nesnesi, her veri üyesi alanı eşlenir. Aşağıdaki örneği göz önünde bulundurun: oluşturulan aile nesnesi aşağıda gösterildiği gibi JSON belgesi için eşlenmiş. Ve bir .NET nesnesini JSON belgesini tam tersi eşlendi.

**C# sınıfı**

```csharp
    public class Family
    {
        [JsonProperty(PropertyName="id")]
        public string Id;
        public Parent[] parents;
        public Child[] children;
        public bool isRegistered;
    };

    public struct Parent
    {
        public string familyName;
        public string givenName;
    };

    public class Child
    {
        public string familyName;
        public string givenName;
        public string gender;
        public int grade;
        public List<Pet> pets;
    };

    public class Pet
    {
        public string givenName;
    };

    public class Address
    {
        public string state;
        public string county;
        public string city;
    };

    // Create a Family object.
    Parent mother = new Parent { familyName= "Wakefield", givenName="Robin" };
    Parent father = new Parent { familyName = "Miller", givenName = "Ben" };
    Child child = new Child { familyName="Merriam", givenName="Jesse", gender="female", grade=1 };
    Pet pet = new Pet { givenName = "Fluffy" };
    Address address = new Address { state = "NY", county = "Manhattan", city = "NY" };
    Family family = new Family { Id = "WakefieldFamily", parents = new Parent [] { mother, father}, children = new Child[] { child }, isRegistered = false };
```

**JSON**  

```json
    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", 
                "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
              "familyName": "Miller", 
              "givenName": "Lisa", 
              "gender": "female", 
              "grade": 8 
            }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
    };
```


### <a name="linq-to-sql-translation"></a>LINQ to SQL çeviri
Cosmos DB sorgu sağlayıcısı bir en iyi çaba eşleme bir Cosmos DB SQL sorgusu bir LINQ Sorgu gerçekleştirir. Aşağıdaki açıklama, temel bir LINQ aşina olduğunuz okuyucu yok varsayıyoruz.

İlk olarak, bir tür sistemi için tüm JSON ilkel türler – sayısal türler, boolean, dize ve null destekliyoruz. Yalnızca bu JSON türleri desteklenmektedir. Aşağıdaki skaler ifadelerin desteklenir.

* Sabit değerler – bunlar sorgu değerlendirilir zaman temel veri türlerinin sabit değerleri içerir.
* Özellik/dizi dizini ifadeleri – bu ifadeler, bir nesne veya dizi öğesi özelliğine bakın.
  
     ailesi. Kimliği;    Family.Children[0].familyName;    Family.Children[0].grade;    Family.Children[n].grade; n bir tamsayı değişkenidir
* Aritmetik ifadeler - bunlar genel aritmetik ifadeler sayısal ve boolean değerleri içerir. Tam liste için SQL belirtimine bakın.
  
     2 * family.children[0].grade;    x + y;
* Dize karşılaştırma ifadesi - bunlar bazı sabit dize değeri bir dize değeri karşılaştırma içerir.  
  
     mother.familyName == "Smith";    child.givenName s; == s string değişkeni.
* Nesne/oluşturma ifadesi - dönüş Bu deyimler bir nesne bileşik değer türü veya anonim bir türün veya bu tür nesneler bir dizi dizisi. Bu değerleri yuvalanabilir.
  
     Yeni üst {familyName "Smith", givenName = "ALi" =}; Yeni {ilk = 1, ikincisi 2 =}; iki alan içeren bir anonim tür              
     Yeni int [] {3, child.grade, 5};

### <a id="SupportedLinqOperators"></a>Desteklenen LINQ işleçleri listesi
SQL .NET SDK'sı ile dahil LINQ Sağlayıcısı'nda desteklenen LINQ işleçlerin bir listesi aşağıda verilmiştir.

* **Seçin**: projeksiyonlar Çevir SQL nesne oluşturmayı da dahil olmak üzere Seç
* **Burada**: filtreler SQL WHERE için çevir ve Destek arasında çeviri & &, || ve! SQL işleçleri
* **SelectMany**: SQL JOIN yan tümcesine dizi geriye doğru izleme sağlar. Dizi öğeleri üzerinde filtre ifadeleri zinciri/iç içe yerleştirme için kullanılabilir
* **OrderBy ve OrderByDescending**: ORDER BY artan/azalan düzene çevirir
* **Sayısı**, **toplam**, **Min**, **Max**, ve **ortalama** işleçleri toplama ve zaman uyumsuz eşdeğerlerine **CountAsync**, **SumAsync**, **MinAsync**, **MaxAsync**, ve **AverageAsync**.
* **CompareTo**: aralık karşılaştırmalar çevirir. . NET'te karşılaştırılabilir değilseniz bu yana dizeleri için yaygın olarak kullanılan
* **Ele**: SQL sorgu sonuçlarından sınırlamak için üst çevirir
* **Matematik işlevleri**: çevrilmesi destekler. NET Abs, Acos, Asin, Atan, Cos, Ceiling, Exp, kat, günlük, Log10, Pow, hepsini, oturum, Sin, Sqrt, Bronz, Truncate eşdeğer SQL yerleşik işlevler.
* **Dize işlevleri**: çevrilmesi destekler. NET Concat, Contains, EndsWith, IndexOf, sayısı, ToLower, TrimStart, Değiştir, ters, TrimEnd, StartsWith, SubString, ToUpper eşdeğer SQL yerleşik işlevleri.
* **Dizi işlevleri**: çevrilmesi destekler. NET Concat, içerir ve eşdeğer SQL yerleşik işlevler sayısı.
* **Jeo-uzamsal uzantı işlevleri**: saptama yöntemleri IsValid ve IsValidDetailed içinde bir uzaklık çevrilecek eşdeğer SQL yerleşik işlevleri destekler.
* **Kullanıcı tanımlı işlev uzantı işlevi**: çeviri saplama yönteminden UserDefinedFunctionProvider.Invoke karşılık gelen kullanıcı tanımlı işlevi destekler.
* **Çeşitli**: çeviri Koşullu işleçler ve birleşim destekler. Dize içerir, ARRAY_CONTAINS veya bağlama bağlı olarak SQL IN içerir çevirebilir.

### <a name="sql-query-operators"></a>SQL sorgu işleçleri
Bazı standart LINQ Sorgu işleçleri Cosmos DB sorgular aşağı nasıl dönüştürüleceğini gösteren bazı örnekleri aşağıda verilmiştir.

#### <a name="select-operator"></a>İşleç seçin
Söz dizimi `input.Select(x => f(x))`burada `f` skaler bir ifade.

**LINQ lambda ifadesi**

    input.Select(family => family.parents[0].familyName);

**SQL** 

```sql
    SELECT VALUE f.parents[0].familyName
    FROM Families f
```

**LINQ lambda ifadesi**

    input.Select(family => family.children[0].grade + c); // c is an int variable


**SQL** 

```sql
    SELECT VALUE f.children[0].grade + c
    FROM Families f 
```


**LINQ lambda ifadesi**

    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });


**SQL** 

```sql
    SELECT VALUE {"name":f.children[0].familyName, 
                  "grade": f.children[0].grade + 3 }
    FROM Families f
```


#### <a name="selectmany-operator"></a>SelectMany işleci
Söz dizimi `input.SelectMany(x => f(x))`burada `f` koleksiyon türü döndüren bir skaler ifade.

**LINQ lambda ifadesi**

    input.SelectMany(family => family.children);

**SQL** 

```sql
    SELECT VALUE child
    FROM child IN Families.children
```

#### <a name="where-operator"></a>Burada işleci
Söz dizimi `input.Where(x => f(x))`burada `f` bir Boole değeri döndüren bir skaler ifade.

**LINQ lambda ifadesi**

    input.Where(family=> family.parents[0].familyName == "Smith");

**SQL** 

```sql
    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith" 
```

**LINQ lambda ifadesi**

    input.Where(
        family => family.parents[0].familyName == "Smith" && 
        family.children[0].grade < 3);

**SQL** 

```sql
    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
    AND f.children[0].grade < 3
```

### <a name="composite-sql-queries"></a>Bileşik SQL sorguları
Daha güçlü sorgular oluşturmak için yukarıdaki işleçleri oluşabilir. Cosmos DB, iç içe geçmiş koleksiyonlar desteklediğinden, birleştirme ya da art arda eklenmiş iç içe geçmiş veya.

#### <a name="concatenation"></a>Birleştirme
Söz dizimi `input(.|.SelectMany())(.Select()|.Where())*`. Birleştirilmiş bir sorgu ile isteğe bağlı olarak başlatabilirsiniz `SelectMany` sorgu birden fazla izlenen `Select` veya `Where` işleçleri.

**LINQ lambda ifadesi**

    input.Select(family=>family.parents[0])
        .Where(familyName == "Smith");

**SQL**

```sql
    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
```

**LINQ lambda ifadesi**

    input.Where(family => family.children[0].grade > 3)
        .Select(family => family.parents[0].familyName);

**SQL** 

```sql
    SELECT VALUE f.parents[0].familyName
    FROM Families f
    WHERE f.children[0].grade > 3
```


**LINQ lambda ifadesi**

    input.Select(family => new { grade=family.children[0].grade}).
        Where(anon=> anon.grade < 3);

**SQL** 

```sql
    SELECT *
    FROM Families f
    WHERE ({grade: f.children[0].grade}.grade > 3)
```

**LINQ lambda ifadesi**

    input.SelectMany(family => family.parents)
        .Where(parent => parents.familyName == "Smith");

**SQL** 

```sql
    SELECT *
    FROM p IN Families.parents
    WHERE p.familyName = "Smith"
```


#### <a name="nesting"></a>İç içe geçirme
Söz dizimi `input.SelectMany(x=>x.Q())` Q olduğu bir `Select`, `SelectMany`, veya `Where` işleci.

İç içe geçmiş bir sorguda iç sorgu dış koleksiyonun her öğesine uygulanır. Bir önemli özelliği iç sorgu gibi dış koleksiyondaki öğelerin alanlara başvurabilir kendinden birleştirmeler.

**LINQ lambda ifadesi**

    input.SelectMany(family=> 
        family.parents.Select(p => p.familyName));

**SQL** 

```sql
    SELECT VALUE p.familyName
    FROM Families f
    JOIN p IN f.parents
```

**LINQ lambda ifadesi**

    input.SelectMany(family => 
        family.children.Where(child => child.familyName == "Jeff"));

**SQL** 

```sql
    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = "Jeff"
```


**LINQ lambda ifadesi**

    input.SelectMany(family => family.children.Where(
        child => child.familyName == family.parents[0].familyName));

**SQL** 

```sql
    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = f.parents[0].familyName
```

## <a id="ExecutingSqlQueries"></a>SQL sorguları yürütme
Cosmos DB, HTTP/HTTPS istekleri yapabilen bir dille çağrılabilen REST API'si aracılığıyla kaynaklarını kullanıma sunar. Ayrıca, Cosmos DB .NET, Node.js, JavaScript ve Python gibi birçok popüler dilde programlama kitaplıkları sunar. REST API ve çeşitli kitaplıklara tüm SQL ile sorgulama desteği. .NET SDK'sı ek olarak SQL sorgulama LINQ destekler.

Aşağıdaki örnekler bir sorgu oluşturun ve Cosmos DB veritabanı hesabını karşı gönderme işlemini göstermektedir.

### <a id="RestAPI"></a>REST API
Cosmos DB, HTTP üzerinden açık bir RESTful programlama modeli sunar. Veritabanı hesaplarında, bir Azure aboneliğini kullanarak sağlanabilir. Cosmos DB kaynak modeli, her biri mantıksal ve kararlı bir URI kullanılarak adreslenebilir bir veritabanı hesabı altında kaynak kümesinden oluşur. Bir kaynak kümesi bu belgedeki bir akış olarak adlandırılır. Bir veritabanı hesabı birden çok koleksiyon içeren her bir veritabanları kümesi oluşur belgeleri, UDF'ler ve diğer kaynak türlerini her hangi içinde dönüş içerir.

Bu kaynaklar temel etkileşim modeliyle HTTP fiilleri GET, PUT, POST ve DELETE kendi standart ile yorumlamasıdır. POST edimi yeni bir kaynak oluşturulmasını, bir saklı yordamı çalıştırmak için veya bir Cosmos DB sorgu verme için kullanılır. Sorgu her zaman salt okunur yan etkileri olan işlemlerdir.

Aşağıdaki örnekler, biz şu ana kadar gözden geçirdiğimize göre iki örnek belgeleri içeren bir koleksiyon karşı yapılan SQL API sorgu için bir gönderme gösterir. Sorgu, basit bir filtre JSON adı özelliği vardır. Kullanımına dikkat edin `x-ms-documentdb-isquery` ve Content-Type: `application/query+json` işlemi bir sorgu olduğunu belirtmek için üstbilgiler.

**İstek**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {      
        "query": "SELECT * FROM Families f WHERE f.id = @familyId",     
        "parameters": [          
            {"name": "@familyId", "value": "AndersenFamily"}         
        ] 
    }


**Sonuçlar**

```
    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32

    <indented for readability, results highlighted>

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "id":"AndersenFamily",
             "lastName":"Andersen",
             "parents":[  
                {  
                   "firstName":"Thomas"
                },
                {  
                   "firstName":"Mary Kay"
                }
             ],
             "children":[  
                {  
                   "firstName":"Henriette Thaulow",
                   "gender":"female",
                   "grade":5,
                   "pets":[  
                      {  
                         "givenName":"Fluffy"
                      }
                   ]
                }
             ],
             "address":{  
                "state":"WA",
                "county":"King",
                "city":"seattle"
             },
             "_rid":"u1NXANcKogEcAAAAAAAAAA==",
             "_ts":1407691744,
             "_self":"dbs\/u1NXAA==\/colls\/u1NXANcKogE=\/docs\/u1NXANcKogEcAAAAAAAAAA==\/",
             "_etag":"00002b00-0000-0000-0000-53e7abe00000",
             "_attachments":"_attachments\/"
          }
       ],
       "count":1
    }
```

İkinci örnek, birleştirme sonucu birden çok sonuç döndüren daha karmaşık bir sorguyu gösterir.

**İstek**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {      
        "query": "SELECT 
                     f.id AS familyName, 
                     c.givenName AS childGivenName, 
                     c.firstName AS childFirstName, 
                     p.givenName AS petName 
                  FROM Families f 
                  JOIN c IN f.children 
                  JOIN p in c.pets",     
        "parameters": [] 
    }


**Sonuçlar**

```
    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84

    <indented for readability, results highlighted>

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "familyName":"AndersenFamily",
             "childFirstName":"Henriette Thaulow",
             "petName":"Fluffy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Goofy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Shadow"
          }
       ],
       "count":3
    }
```

Bir sorgunun sonuçlarını tek bir sonuç sayfasını içinde sığamıyorsa sonra REST API aracılığıyla bir devamlılık belirteci döndürür `x-ms-continuation-token` yanıtı üstbilgisi. İstemcileri, sonraki sonuçları üst bilgisi dahil olmak üzere sonuçlarını sayfalandırma. Sayfa başına sonuç sayısı üzerinden de denetlenebilir `x-ms-max-item-count` sayı başlığı. Belirtilen sorgu gibi bir toplama işlevi varsa `COUNT`, ardından sorgu sayfası sonuçları sayfanın kısmen toplanan bir değer döndürebilir. İstemciler, örneğin son sonuçlar, her bir sayfayı toplam sayısını döndürmek için döndürülen sayıları üzerinden toplamak için bu sonuçlar ikinci düzey toplama gerçekleştirmeniz gerekir.

Sorgular için veri tutarlılık ilkesi yönetmek için kullandığınız `x-ms-consistency-level` gibi tüm REST API isteği üstbilgisi. Oturum tutarlılığı için de en son echo gerekli `x-ms-session-token` sorgu istekteki tanımlama bilgisi üstbilgisi. Sorgulanan koleksiyonunun dizin oluşturma ilkesini tutarlılığını sorgu sonuçlarını da etkileyebilir. İle dizin oluşturma ilkesi ayarları varsayılan olarak, koleksiyonlar için dizin her zaman belge içeriğiyle geçerli ve sorgu sonuçları için veri seçilen tutarlılık eşleşmesi. Lazy için dizin oluşturma ilkesini yumuşatılmıştır, sorguları eski sonuçları geri dönebilirsiniz. Daha fazla bilgi için [Azure Cosmos DB tutarlılık düzeyleri][consistency-levels].

Belirtilen sorgu koleksiyonu üzerinde yapılandırılmış dizin oluşturma ilkesini destekleyemiyorsa, Azure Cosmos DB sunucusu 400 "Bad Request" döndürür. Bu aralık sorguları açıkça dizine elmadan hariç yolları yanı sıra, karma (eşitlik) aramaları için yapılandırılan yollar için döndürülür. `x-ms-documentdb-query-enable-scan` Bir dizini olmadığında bir tarama gerçekleştirmek sorgu izin vermek için üst bilgi belirtilebilir.

Ayarlayarak, sorgu yürütme ayrıntılı ölçümleri alabilirsiniz `x-ms-documentdb-populatequerymetrics` başlığına `True`. Daha fazla bilgi için [Azure Cosmos DB için SQL sorgu ölçümleri](sql-api-sql-query-metrics.md).

### <a id="DotNetSdk"></a>C# (.NET) SDK'SI
LINQ hem SQL .NET SDK'yı destekleyen sorgulama. Aşağıdaki örnek, bu belgenin önceki bölümlerinde sunulan basit filtre sorgusu gerçekleştirmeyi gösterir.

    foreach (var family in client.CreateDocumentQuery(collectionLink, 
        "SELECT * FROM Families f WHERE f.id = \"AndersenFamily\""))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    SqlQuerySpec query = new SqlQuerySpec("SELECT * FROM Families f WHERE f.id = @familyId");
    query.Parameters = new SqlParameterCollection();
    query.Parameters.Add(new SqlParameter("@familyId", "AndersenFamily"));

    foreach (var family in client.CreateDocumentQuery(collectionLink, query))
    {
        Console.WriteLine("\tRead {0} from parameterized SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery(collectionLink)
        where f.Id == "AndersenFamily"
        select f))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in client.CreateDocumentQuery(collectionLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


Bu örnek, her bir belge içindeki eşitlik için iki özellik karşılaştırır ve anonim projeksiyonlar kullanır. 

    foreach (var family in client.CreateDocumentQuery(collectionLink,
        @"SELECT {""Name"": f.id, ""City"":f.address.city} AS Family 
        FROM Families f 
        WHERE f.address.city = f.address.state"))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery<Family>(collectionLink)
        where f.address.city == f.address.state
        select new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in
        client.CreateDocumentQuery<Family>(collectionLink)
        .Where(f => f.address.city == f.address.state)
        .Select(f => new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


Sonraki örnek, birleşimler, LINQ SelectMany ifade gösterir.

    foreach (var pet in client.CreateDocumentQuery(collectionLink,
          @"SELECT p
            FROM Families f 
                 JOIN c IN f.children 
                 JOIN p in c.pets 
            WHERE p.givenName = ""Shadow"""))
    {
        Console.WriteLine("\tRead {0} from SQL", pet);
    }

    // Equivalent in Lambda expressions
    foreach (var pet in
        client.CreateDocumentQuery<Family>(collectionLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }



.NET istemci otomatik olarak sorgu sonuçları foreach bloklar yukarıda da gösterildiği gibi tüm sayfaları aracılığıyla yinelenir. REST API bölümünde sunulan sorgu seçeneklerini de .NET SDK kullanarak kullanılabilir `FeedOptions` ve `FeedResponse` CreateDocumentQuery yöntemi sınıflar. Sayfa sayısı kullanılarak denetlenebilir `MaxItemCount` ayarı. 

Disk belleği oluşturarak de açıkça denetleyebilirsiniz `IDocumentQueryable` kullanarak `IQueryable` okuyarak sonra nesne,` ResponseContinuationToken` değerleri ve bunları geçirmeden geri olarak `RequestContinuationToken` içinde `FeedOptions`. `EnableScanInQuery` Sorgu yapılandırılan dizin oluşturma ilkesi tarafından desteklendiğinde taramaları etkinleştirmek için ayarlanabilir. Bölümlenmiş koleksiyonlar için kullanabileceğiniz `PartitionKey` karşı tek bir sorguyu çalıştırmak için bölüm (Cosmos DB otomatik olarak bu sorgu metni ayıklayabilir rağmen), ve `EnableCrossPartitionQuery` karşı birden çok bölüm çalıştırılması gereken sorguları çalıştırmak için. 

Başvurmak [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-documentdb-net) sorgular içeren daha fazla örnek için. 

### <a id="JavaScriptServerSideApi"></a>JavaScript sunucu tarafı API'si
Cosmos DB, JavaScript tabanlı uygulama mantığını saklı yordamları ve Tetikleyicileri kullanarak doğrudan koleksiyonlarda yürütmek için bir programlama modeli sağlar. Koleksiyon düzeyinde kayıtlı JavaScript mantığının ardından belgeler üzerinde işlemler verilen koleksiyon veritabanı işlemlerini verebilir. Bu işlemler çevresel ACID işlemlerini sarmalanır.

Aşağıdaki örnek queryDocuments JavaScript Sunucusu API sorgularından yapmak için nasıl kullanılacağını gösterir iç saklı yordamlar ve tetikleyiciler.

    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();
        var collectionLink = collectionManager.getSelfLink()

        // create a new document.
        collectionManager.createDocument(collectionLink,
            { name: name, author: author },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);

                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);

                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don't need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }

## <a id="References"></a>Başvuruları
1. [Azure Cosmos DB'ye giriş][introduction]
2. [Azure Cosmos DB SQL belirtimi](http://go.microsoft.com/fwlink/p/?LinkID=510612)
3. [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-documentdb-net)
4. [Azure Cosmos DB tutarlılık düzeyleri][consistency-levels]
5. ANSI SQL 2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
6. JSON [http://json.org/](http://json.org/)
7. JavaScript belirtimi [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm) 
8. LINQ [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx) 
9. Değerlendirme tekniklerini büyük veritabanları için sorgu [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611)
10. Sorgu 1994 paralel ilişkisel veritabanı sistemleri, IEEE bilgisayarda topluluğu Press, işleme
11. Lu, Ooi, Bronz, sorgu 1994 paralel ilişkisel veritabanı sistemleri, IEEE bilgisayarda topluluğu Press, işleme.
12. Christopher Olston, Benjamin Reed, Utkarsh Srivastava, Ravi Kumar, Andrew Tomkins: Pig Latin: olmayan şekilde yabancı dil için veri işleme, SIGMOD 2008.
13. G. Graefe. Basamaklar framework sorgu iyileştirme. IEEE veri Müh Bull., 18(3): 1995.

[1]: ./media/sql-api-sql-query/sql-query1.png
[introduction]: introduction.md
[consistency-levels]: consistency-levels.md
