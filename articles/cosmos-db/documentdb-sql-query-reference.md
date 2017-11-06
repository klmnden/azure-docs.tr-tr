---
title: "Azure DB Cosmos DocumentDB API: SQL söz dizimi | Microsoft Docs"
description: "Azure Cosmos DB DocumentDB API SQL sorgu dili için başvuru belgeleri."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 10/18/2017
ms.author: mimig
ms.openlocfilehash: 4907df15fddfb7d8d6128dc994b0920ca601f2c7
ms.sourcegitcommit: d6ad3203ecc54ab267f40649d3903584ac4db60b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2017
---
# <a name="azure-cosmos-db-documentdb-api-sql-syntax-reference"></a>Azure DB Cosmos DocumentDB API: SQL söz dizimi başvurusu

Azure Cosmos DB DocumentDB API tanıdık SQL (yapılandırılmış sorgu dili) kullanarak belgelerin sorgulanmasını dilbilgisi gibi hiyerarşik JSON belgelerini gerektirmeden açık şema veya ikincil dizinlerin oluşturulmasını destekler. Bu konu, başvuru belgeleri DocumentDB API SQL sorgu dili için sağlar.

DocumentDB API SQL sorgu dili için bkz [SQL sorgularını Azure Cosmos DB DocumentDB API için](documentdb-sql-query.md).  
  
Ayrıca ziyaret etmek için davet ediyoruz [Query Playground](http://www.documentdb.com/sql/demo) burada Azure Cosmos DB deneyin ve SQL sorgularını kümemize karşı çalıştırabilirsiniz.  
  
## <a name="select-query"></a>SEÇME sorgusu  
JSON belgeleri veritabanından alır. İfade değerlendirme, filtreleme tahminleri destekler ve birleştirir.  Seçim deyimleri tanımlamak için kullanılan kuralları sözdizimi kuralları bölümünde tabloda verilmiştir.  
  
**Sözdizimi**  
  
```
<select_query> ::=  
SELECT <select_specification>   
    [ FROM <from_specification>]   
    [ WHERE <filter_condition> ]  
    [ ORDER BY <sort_specification> ]  
```  
  
 **Açıklamalar**  
  
 Ayrıntılar için bölümlere her yan tümcesi aşağıdaki bakın:  
  
-   [SELECT yan tümcesi](#bk_select_query)  
  
-   [FROM yan tümcesi](#bk_from_clause)  
  
-   [WHERE yan tümcesi](#bk_where_clause)  
  
-   [ORDER BY yan tümcesi](#bk_orderby_clause)  
  
SELECT deyiminde yan tümceleri, yukarıda gösterildiği gibi sıralanmalıdır. İsteğe bağlı bir yan tümceleri herhangi biri atlanabilir. Ancak isteğe bağlı bir yan tümceleri kullanıldığında, doğru sırayla görünmesi gerekir.  
  
**SELECT deyimi mantıksal işleme sırası**  
  
Yan tümceleri işlenme sırası şöyledir:  

1.  [FROM yan tümcesi](#bk_from_clause)  
2.  [WHERE yan tümcesi](#bk_where_clause)  
3.  [ORDER BY yan tümcesi](#bk_orderby_clause)  
4.  [SELECT yan tümcesi](#bk_select_query)  

Bu sözdiziminde görünme sırasını farklı olduğunu unutmayın. İşlenen yan tümcesi ile sunulan tüm yeni sembolleri görünür ve daha sonra işlenen yan tümcelerinde kullanılabilir gibi sıralama olmamasıdır. Örneğin, bir FROM yan tümcesinde bildirilen diğer adlar WHERE erişilebilir ve SELECT yan tümceleri.  

**Boşluk karakterleri ve açıklamaları**  

Tırnak içine alınan bir dizeyi bir parçası olmayan veya tanımlayıcı tırnak içine alınmış tüm boşluk karakterleri dil dilbilgisi parçası olmayan ve ayrıştırma sırasında yok sayılır.  

T-SQL stili yorumlar gibi sorgu dili destekler  

-   SQL deyimi`-- comment text [newline]`  

Boşluk karakterleri ve açıklamalar herhangi anlamlı dilbilgisi değil olsa da, belirteçlerin ayırmak için kullanılmalıdır. Örneğin: `-1e5` bir tek sayı belirteç süre olan`: – 1 e5` eksi bir belirteç numarası 1 ve tanımlayıcı e5 tarafından izlenir.  

##  <a name="bk_select_query"></a>SELECT yan tümcesi  
SELECT deyiminde yan tümceleri, yukarıda gösterildiği gibi sıralanmalıdır. İsteğe bağlı bir yan tümceleri herhangi biri atlanabilir. Ancak isteğe bağlı bir yan tümceleri kullanıldığında, doğru sırayla görünmesi gerekir.  

**Sözdizimi**  
```  
SELECT <select_specification>  

<select_specification> ::=   
      '*'   
      | <object_property_list>   
      | VALUE <scalar_expression> [[ AS ] value_alias]  
  
<object_property_list> ::=   
{ <scalar_expression> [ [ AS ] property_alias ] } [ ,...n ]  
  
```  
  
 **Bağımsız değişkenler**  
  
 `<select_specification>`  
  
 Özellikler veya sonuç kümesi için seçilecek değeri.  
  
 `'*'`  
  
Değer herhangi bir değişiklik yapmadan alınması gerektiğini belirtir. İşlenen değer bir nesne ise, özellikle, tüm özellikler alınır.  
  
 `<object_property_list>`  
  
Alınacak özellikler listesini belirtir. Her döndürülen değeri, belirtilen özellikleri olan bir nesne olacaktır.  
  
`VALUE`  
  
JSON değeri yerine tam JSON nesnesi alınması gerektiğini belirtir. Bu, aksine `<property_list>` Öngörülen Değer bir nesne içinde kaymasını değil.  
  
`<scalar_expression>`  
  
Hesaplanacak değerin temsil eden ifadesi. Bkz: [skaler ifadelerin](#bk_scalar_expressions) ayrıntıları bölümü.  
  
**Açıklamalar**  
  
`SELECT *` Sözdizimi geçerli yalnızca tam olarak bir diğer ad belirtmiş FROM yan tümcesi varsa. `SELECT *`projeksiyon gerektiğinde yararlı olabilecek bir kimlik projeksiyon sağlar. SEÇİN * yalnızca FROM yan tümcesi belirtilirse geçerlidir ve yalnızca tek bir giriş kaynağı sunmuştur.  
  
Unutmayın `SELECT <select_list>` ve `SELECT *` "söz dizimi sugar" olduğunu ve bunun yerine aşağıda gösterildiği gibi basit SELECT deyimi kullanılarak belirtilebilir.  
  
1.  `SELECT * FROM ... AS from_alias ...`  
  
     eşdeğerdir:  
  
     `SELECT from_alias FROM ... AS from_alias ...`  
  
2.  `SELECT <expr1> AS p1, <expr2> AS p2,..., <exprN> AS pN [other clauses...]`  
  
     eşdeğerdir:  
  
     `SELECT VALUE { p1: <expr1>, p2: <expr2>, ..., pN: <exprN> }[other clauses...]`  
  
**Ayrıca bkz.**  
  
[Skaler ifade](#bk_scalar_expressions)  
[SELECT yan tümcesi](#bk_select_query)  
  
##  <a name="bk_from_clause"></a>FROM yan tümcesi  
Kaynak veya birleştirilmiş kaynakları belirtir. FROM yan tümcesi isteğe bağlıdır. Aksi durumda belirtilen, diğer yan tümceleri hala FROM yan tümcesi tek bir belgenin sağladıysanız olarak yürütülür.  
  
**Sözdizimi**  
  
```  
FROM <from_specification>  
  
<from_specification> ::=   
        <from_source> {[ JOIN <from_source>][,...n]}  
  
<from_source> ::=   
          <collection_expression> [[AS] input_alias]  
        | input_alias IN <collection_expression>  
  
<collection_expression> ::=   
        ROOT   
     | collection_name  
     | input_alias  
     | <collection_expression> '.' property_name  
     | <collection_expression> '[' "property_name" | array_index ']'  
```  
  
**Bağımsız değişkenler**  
  
`<from_source>`  
  
Bir veri kaynağı olan veya olmayan bir diğer ad belirtir. Diğer ad belirtilmezse, gelen çıkarılabilir olacaktır `<collection_expression>` kuralları kullanarak:  
  
-   İfade bir toplama_adı ise, toplama_adı bir diğer ad olarak kullanılır.  
  
-   İfade ise `<collection_expression>`, property_name sonra property_name bir diğer ad olarak kullanılır. İfade bir toplama_adı ise, toplama_adı bir diğer ad olarak kullanılır.  
  
OLARAK`input_alias`  
  
Belirleyen `input_alias` temel Toplama ifadesi tarafından döndürülen değerler kümesidir.  
 
`input_alias`IN  
  
Belirleyen `input_alias` temel Toplama ifadesi tarafından döndürülen her dizinin tüm dizi öğeleri üzerinde yineleme tarafından elde edilen değerleri kümesi temsil etmelidir. Dizi olmayan temel Toplama ifadesi tarafından döndürülen herhangi bir değer yok sayılır.  
  
`<collection_expression>`  
  
Belge almak için kullanılacak Toplama ifadesi belirtir.  
  
`ROOT`  
  
Bu belge varsayılan olarak şu anda bağlı koleksiyonu alınması gerektiğini belirtir.  
  
`collection_name`  
  
Bu belgede sağlanan koleksiyondan alınması gerektiğini belirtir. Koleksiyon adı şu anda bağlı koleksiyonunun adı eşleşmelidir.  
  
`input_alias`  
  
Bu belgede sağlanan diğer adı tarafından tanımlanan diğer kaynaktan alınması gerektiğini belirtir.  
  
`<collection_expression> '.' property_`  
  
Bu belge erişerek alınabilir belirtir `property_name` özelliği veya dizi_dizini dizi öğesi tarafından alınan tüm belgeler için belirtilen toplama ifadesi.  
  
`<collection_expression> '[' "property_name" | array_index ']'`  
  
Bu belge erişerek alınabilir belirtir `property_name` özelliği veya dizi_dizini dizi öğesi tarafından alınan tüm belgeler için belirtilen toplama ifadesi.  
  
**Açıklamalar**  
  
Tüm diğer adlar sağlanan veya içinde çıkarımı yapılan `<from_source>(`s) benzersiz olması gerekir. Sözdizimi `<collection_expression>.`property_name aynı olup `<collection_expression>' ['"property_name"']'`. Ancak, bir özellik adı tanımlayıcı olmayan karakterleri içeriyorsa, ikinci sözdizimi kullanılabilir.  
  
**Dizi öğeleri eksik özellikleri eksik değerleri işleme tanımlanmamış**  
  
Bir toplama ifadesi özellikleri veya dizi öğeleri ve değer yok erişirse, bu değeri göz ardı ve daha fazla işlenmedi.  
  
**Toplama ifadesi içerik kapsamı**  
  
Bir toplama ifadesi koleksiyonu kapsamlı veya belge kapsamlı olabilir:  
  
-   Temel alınan kaynak koleksiyonu ifadesinin ya da kök ise koleksiyonu kapsamlı, ifade olan veya `collection_name`. Bu tür bir ifade koleksiyondan doğrudan alınan belgeleri kümesini temsil eder ve diğer toplama ifadeleri işlenmesini bağımlı değildir.  
  
-   Belge temel alınan kaynak koleksiyonu ifadesinin ise kapsamlı, ifade olan `input_alias` sorgu sunmuştur. Bu tür bir ifade diğer koleksiyonla ilişkili kümesine ait her bir belgenin kapsamında toplama ifadesinin hesaplanmasıyla elde belgeleri kümesini temsil eder.  Sonuç kümesi her temel kümesindeki belgelerin toplama ifadesinin hesaplanmasıyla elde edilen kümeleri birleşim olacaktır.  
  
**Birleşimler**  
  
Geçerli sürümde Azure Cosmos DB iç birleştirmeler destekler. Ek birleştirme özellikleri yeni çıkacak.

İç birleşimler birleştirme katılan kümelerinin eksiksiz bir çapraz ürün sonuçlanır. Burada yer alan tanımlama grubu içindeki her değerin katılmasını birleştirme ayarlamak diğer adı ile ilişkili ve söz konusu diğer ad diğer yan tümcelerinde başvurarak erişilen N-öğe başlık kümesi bir N yönlü birleştirme sonucudur.  
  
Değerlendirme katılma katılımcı ayarlar bağlamı kapsamına bağlıdır:  
  
-  Bir birleştirme arasında koleksiyon kümesi A ve koleksiyon kapsamlı B, A ve b kümelerindeki tüm öğelerin çapraz ürün sonuçları ayarlayın
  
-   Tüm ayarlar her belge için B A. belge kapsamlı ayarlama değerlendirerek elde birleşimi sonuçlanıyor kümesi belge kapsamlı kümesi B arasındaki bir birleştirme  
  
 Geçerli sürümde, bir koleksiyon kapsamlı ifade maksimum Sorgu işlemcisi tarafından desteklenir.  
  
**Birleştirme örnekleri:**  
  
Aşağıdaki FROM yan tümcesi bakalım:`<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`  
  
 Tanımlamak her kaynak izin `input_alias1, input_alias2, …, input_aliasN`. Bu FROM yan tümcesi N-diziler (tuple N değerlerle) kümesini döndürür. Her tanımlama grubu tüm koleksiyon diğer adları kendi ilgili ayarlar yineleme tarafından üretilen değerler içeriyor.  
  
*Örnek 1, 2 kaynaklarıyla KATIL:*  
  
- Let `<from_source1>` koleksiyonu kapsamlı ve kümesi {A, B, C} temsil eder.  
  
- Let `<from_source2>` input_alias1 başvuran belge kapsamlı ve ayarlar temsil eder:  
  
    {1, 2} için`input_alias1 = A,`  
  
    için {3}`input_alias1 = B,`  
  
    {4, 5} için`input_alias1 = C,`  
  
- FROM yan tümcesi `<from_source1> JOIN <from_source2>` aşağıdaki başlıklar neden olur:  
  
    (`input_alias1, input_alias2`):  
  
    `(A, 1), (A, 2), (B, 3), (C, 4), (C, 5)`  
  
*Örnek 2, 3 kaynaklarıyla KATIL:*  
  
- Let `<from_source1>` koleksiyonu kapsamlı ve kümesi {A, B, C} temsil eder.  
  
- Let `<from_source2>` belge kapsamlı başvuran olması `input_alias1` ve ayarlar temsil eder:  
  
    {1, 2} için`input_alias1 = A,`  
  
    için {3}`input_alias1 = B,`  
  
    {4, 5} için`input_alias1 = C,`  
  
- Let `<from_source3>` belge kapsamlı başvuran olması `input_alias2` ve ayarlar temsil eder:  
  
    {100, 200} için`input_alias2 = 1,`  
  
    {300} için`input_alias2 = 3,`  
  
- FROM yan tümcesi `<from_source1> JOIN <from_source2> JOIN <from_source3>` aşağıdaki başlıklar neden olur:  
  
    (input_alias1, input_alias2, input_alias3):  
  
    (A, 1, 100), (A, 1, 200), (B, 3, 300)  
  
> [!NOTE]
> Diğer değerler için diziler eksikliği `input_alias1`, `input_alias2`, kendisi için `<from_source3>` hiçbir değer döndürmedi.  
  
*Örnek 3, 3 kaynaklarıyla KATIL:*  
  
- < Koleksiyon kapsamlı ve {A, B, C} kümesini temsil eden from_source1 > olanak tanır.  
  
- Let `<from_source1>` koleksiyonu kapsamlı ve kümesi {A, B, C} temsil eder.  
  
- < Belge kapsamlı başvuru input_alias1 olması ve ayarlar temsil from_source2 > sağlar:  
  
    {1, 2} için`input_alias1 = A,`  
  
    için {3}`input_alias1 = B,`  
  
    {4, 5} için`input_alias1 = C,`  
  
- Let `<from_source3>` kapsamlı `input_alias1` ve ayarlar temsil eder:  
  
    {100, 200} için`input_alias2 = A,`  
  
    {300} için`input_alias2 = C,`  
  
- FROM yan tümcesi `<from_source1> JOIN <from_source2> JOIN <from_source3>` aşağıdaki başlıklar neden olur:  
  
    (`input_alias1, input_alias2, input_alias3`):  
  
    (A, 1, 100), (A, 1, 200), (A, 2, 100), (A, 2, 200), (C, 4, 300) (C, 5, 300)  
  
> [!NOTE]
> Bu arasında çapraz ürün ile sonuçlandı `<from_source2>` ve `<from_source3>` her ikisi de aynı kapsamındaki çünkü `<from_source1>`.  Bu 4 (2 x 2) sonuçlandı diziler değerini 0 diziler B (1 x 0) değeri sahip olması ve 2 (2 x 1) değeri C. diziler  
  
**Ayrıca bkz.**  
  
 [SELECT yan tümcesi](#bk_select_query)  
  
##  <a name="bk_where_clause"></a>WHERE yan tümcesi  
 Sorgu tarafından döndürülen belgeler için arama koşulu belirtir.  
  
 **Sözdizimi**  
  
```  
WHERE <filter_condition>  
<filter_condition> ::= <scalar_expression>  
  
```  
  
 **Bağımsız değişkenler**  
  
-   `<filter_condition>`  
  
     Döndürülecek belgeler için karşılanması gereken koşulu belirtir.  
  
-   `<scalar_expression>`  
  
     Hesaplanacak değerin temsil eden ifadesi. Bkz: [skaler ifadelerin](#bk_scalar_expressions) ayrıntıları bölümü.  
  
 **Açıklamalar**  
  
 Filtre olarak belirtilen bir ifade döndürülecek belge için sırayla koşulunu true olarak değerlendirmeniz gerekir. Boole değeri true koşulu, başka bir değer yerine getirecek yalnızca: tanımlanmamış, null, false, sayı, dizi veya nesne uygun olmadığı koşul.  
  
##  <a name="bk_orderby_clause"></a>ORDER BY yan tümcesi  
 Sorgu tarafından döndürülen sonuçları sıralama düzenini belirtir.  
  
 **Sözdizimi**  
  
```  
ORDER BY <sort_specification>  
<sort_specification> ::= <sort_expression> [, <sort_expression>]  
<sort_expression> ::= <scalar_expression> [ASC | DESC]  
  
```  
  
 **Bağımsız değişkenler**  
  
-   `<sort_specification>`  
  
     Bir özellik veya sorgu sonucu kümesi sıralama yapılacak ifade belirtir. Sıralama sütunu adı veya sütun diğer adı belirtilebilir.  
  
     Birden çok sütunları sıralama belirtilebilir. Sütun adları benzersiz olmalıdır. ORDER BY yan tümcesinde sütunları sıralama sırasını sıralanmış sonuç kümesi organizasyonu tanımlar. Diğer bir deyişle, sonuç kümesi ilk özelliği tarafından sıralanır ve ardından bu sıralı liste ikinci özelliği vb. göre sıralanır.  
  
     ORDER BY yan tümcesinde başvurulan sütun adları seçim listesindeki bir sütun veya herhangi belirsizlikleri olmadan FROM yan tümcesinde belirtilen bir tablodaki tanımlanmış bir sütuna karşılık gelmelidir.  
  
-   `<sort_expression>`  
  
     Tek bir özellik veya sorgu sonucu kümesi sıralama yapılacak ifade belirtir.  
  
-   `<scalar_expression>`  
  
     Bkz: [skaler ifadelerin](#bk_scalar_expressions) ayrıntıları bölümü.  
  
-   `ASC | DESC`  
  
     Belirtilen sütunundaki değerler artan veya azalan sırada sıralanması gerektiğini belirtir. ASC en düşük değerden yüksek değere sıralar. DESC en yüksek değerden düşük değere sıralar. ASC varsayılan sıralama düzeni ' dir. Null değerler en düşük olası değerler kabul edilir.  
  
 **Açıklamalar**  
  
 Sorgu dilbilgisi özellikleri tarafından birden çok sipariş desteklerken, yalnızca tek bir özellik, yalnızca adlarla ve özellik adları, yani, hesaplanan özellikleri karşı sıralama Azure Cosmos DB sorgu çalışma zamanı destekler. Sıralama, ayrıca dizin oluşturma ilkesi özelliği ve en yüksek duyarlık ile belirtilen tür için bir aralığı dizini içerir gerektirir. Daha fazla ayrıntı için dizin oluşturma ilkesi belgelerine bakın.  
  
##  <a name="bk_scalar_expressions"></a>Skaler ifade  
 Skaler bir ifade, simgeler ve tek bir değer almak için değerlendirilen işleçleri birleşimidir. Basit ifadeler sabitler, özellik başvuruları, dizi öğesi başvuruları, diğer başvurular veya işlev çağrılarını olabilir. Basit ifadeler işleçleri kullanarak karmaşık ifadelere birleştirilebilir.  
  
 Hangi skaler ifade olabilir değerleri hakkında daha fazla bilgi için bkz: [sabitleri](#bk_constants) bölümü.  
  
 **Sözdizimi**  
  
```  
<scalar_expression> ::=  
       <constant>   
     | input_alias   
     | parameter_name  
     | <scalar_expression>.property_name  
     | <scalar_expression>'['"property_name"|array_index']'  
     | unary_operator <scalar_expression>  
     | <scalar_expression> binary_operator <scalar_expression>    
     | <scalar_expression> ? <scalar_expression> : <scalar_expression>  
     | <scalar_function_expression>  
     | <create_object_expression>   
     | <create_array_expression>  
     | (<scalar_expression>)   
  
<scalar_function_expression> ::=  
        'udf.' Udf_scalar_function([<scalar_expression>][,…n])  
        | builtin_scalar_function([<scalar_expression>][,…n])  
  
<create_object_expression> ::=  
   '{' [{property_name | "property_name"} : <scalar_expression>][,…n] '}'  
  
<create_array_expression> ::=  
   '[' [<scalar_expression>][,…n] ']'  
  
```  
  
 **Bağımsız değişkenler**  
  
-   `<constant>`  
  
     Sabit bir değeri temsil eder. Bkz: [sabitleri](#bk_constants) ayrıntıları bölümü.  
  
-   `input_alias`  
  
     Tarafından tanımlanan bir değeri temsil `input_alias` sunulan `FROM` yan tümcesi.  
    Bu değer olmayan olması garanti **tanımsız** –**tanımsız** giriş değerleri atlanır.  
  
-   `<scalar_expression>.property_name`  
  
     Bir nesnenin özellik değerini temsil eder. Özelliği mevcut değil veya özellik bir nesne olmayan bir değer başvurulan durumunda için ifadeyi hesaplar **tanımsız** değeri.  
  
-   `<scalar_expression>'['"property_name"|array_index']'`  
  
     Ada sahip özelliğin değerini temsil eden `property_name` veya dizi öğesi dizine sahip `array_index` nesne/dizisi. Özellik/dizi dizini yok veya nesne/dizi olmayan bir değer özelliği/dizi dizini başvurulan, ifade için Tanımsız değer değerlendirir.  
  
-   `unary_operator <scalar_expression>`  
  
     Tek bir değer uygulanan bir işleç temsil eder. Bkz: [işleçleri](#bk_operators) ayrıntıları bölümü.  
  
-   `<scalar_expression> binary_operator <scalar_expression>`  
  
     İki değer için uygulanan bir işleç temsil eder. Bkz: [işleçleri](#bk_operators) ayrıntıları bölümü.  
  
-   `<scalar_function_expression>`  
  
     İşlev çağrısının sonucunu tarafından tanımlanan bir değeri temsil eder.  
  
-   `udf_scalar_function`  
  
     Kullanıcının adını skaler işlev tanımlı.  
  
-   `builtin_scalar_function`  
  
     Yerleşik skaler işlev adı.  
  
-   `<create_object_expression>`  
  
     Belirtilen özelliklere sahip yeni bir nesne oluşturarak alınan bir değer ve bunların değerleri temsil eder.  
  
-   `<create_array_expression>`  
  
     Öğeleri olarak belirtilen değerlerle yeni bir dizi oluşturarak elde bir değeri temsil eder  
  
-   `parameter_name`  
  
     Belirtilen parametre adı değerini temsil eder. Parametre adları @ tek bir ilk karakter olarak olması gerekir.  
  
 **Açıklamalar**  
  
 Bir yerleşik veya kullanıcı çağırma skaler işlev tanımlandığında tüm bağımsız değişkenler tanımlanması gerekir. Bağımsız değişkenlerden biri ise tanımlanmamış, işlev çağrılmaz ve sonucu tanımsız olacaktır.  
  
 Bir nesne oluştururken Tanımsız değer atanmış herhangi bir özelliği atlandı ve oluşturulan nesnesinde yer almayan.  
  
 Ne zaman herhangi bir öğeyi değer bir dizi oluşturma atanmış **tanımsız** değeri atlandı ve oluşturulan nesnesinde dahil değildir. Bu, oluşturulan dizi dizinleri atlandı değil, şekilde yerini alacak sonraki tanımlanan öğe neden olur.  
  
##  <a name="bk_operators"></a>İşleçler  
 Bu bölümde desteklenen işleçleri açıklanmaktadır. Her işleci tam olarak bir kategoriye atayabilirsiniz.  
  
 Bkz: **işleci kategorileri** Ayrıntılar için aşağıdaki tabloya işlenmesi ile ilgili **tanımsız** değerleri, giriş değerleri ve işleme türlerini eşleşmeyen ile değerlerin türü gereksinimleri.  
  
 **İşleç kategoriler:**  
  
|**Kategori**|**Ayrıntılar**|  
|-|-|  
|**aritmetik**|İşleç input(s) istendiğiniz olmasını bekler. Çıktı ayrıca bir sayıdır. Herhangi biri ise **tanımsız** veya numarası sonra sonuç dışında türüdür **tanımsız**.|  
|**bit tabanlı**|İşleç input(s) 32-bit işaretli tamsayıyı istendiğiniz olmasını bekler. Çıktı da 32 bit imzalı numarası tamsayıdır.<br /><br /> Herhangi bir tamsayı olmayan değer yuvarlanır. Pozitif bir değer yuvarlanacağı, negatif değerleri yuvarlanan.<br /><br /> 32 bit tamsayı aralığın dışında herhangi bir değer, son 32 bitlik, iki kişinin tamamlama gösterimi gerçekleştirerek dönüştürülür.<br /><br /> Herhangi biri ise **tanımsız** sonuç sonra diğer, sayıdan yazın veya **tanımsız**.<br /><br /> **Not:** yukarıdaki davranışı JavaScript bit düzeyinde işleci davranışı ile uyumludur.|  
|**mantıksal**|İşleç input(s) Boolean(s) olmasını bekler. Çıktı de bir Boole değeri olur.<br />Herhangi biri ise **tanımsız** sonucu olacaktır sonra Boolean, diğerinden yazın veya **tanımsız**.|  
|**Karşılaştırma**|İşleci, aynı türe sahip ve tanımsız olmaması için input(s) bekliyor. Çıktı bir Boole değeri değil.<br /><br /> Herhangi biri ise **tanımsız** veya girişleri farklı türlerine sahip sonra sonuç **tanımsız**.<br /><br /> Bkz: **karşılaştırma için değerlerin sıralama** ayrıntıları sıralama değeri için tablo.|  
|**dize**|İşleç input(s) dizelerini olmasını bekler. Çıktı ayrıca bir dizedir.<br />Herhangi biri ise **tanımsız** sonuç sonra dize dışında yazın veya **tanımsız**.|  
  
 **Birli işleçleri:**  
  
|**Ad**|**İşleci**|**Ayrıntılar**|  
|-|-|-|  
|**aritmetik**|+<br /><br /> -|Sayı değeri döndürür.<br /><br /> Bit tabanlı değil işlecini. Sayı değeri tasarruflarını döndürür.|  
|**bit tabanlı**|~|Olanları tamamlama. Tamamlama sayı değeri döndürür.|  
|**Mantıksal**|**DEĞİL**|Değilleme. Boole değeri döndürür tasarruflarını.|  
  
 **İkili işleçler:**  
  
|**Ad**|**İşleci**|**Ayrıntılar**|  
|-|-|-|  
|**aritmetik**|+<br /><br /> -<br /><br /> *<br /><br /> /<br /><br /> %|Ayrıca.<br /><br /> Çıkarma.<br /><br /> Çarpma.<br /><br /> Bölme.<br /><br /> Modülasyon.|  
|**bit tabanlı**|&#124;<br /><br /> &<br /><br /> ^<br /><br /> <<<br /><br /> >><br /><br /> >>>|Bit düzeyinde OR.<br /><br /> Bit düzeyinde and<br /><br /> Bit düzeyinde XOR.<br /><br /> Sola kaydırma.<br /><br /> Sağa kaydırma.<br /><br /> Sıfır dolgu sağa kaydırma.|  
|**mantıksal**|**VE**<br /><br /> **VEYA**|Mantıksal ve işlecini. Döndürür **true** iki bağımsız değişkenler ise **true**, döndürür **false** Aksi takdirde.<br /><br /> Mantıksal ve işlecini. Döndürür **true** iki bağımsız değişkenler ise **true**, döndürür **false** Aksi takdirde.|  
|**Karşılaştırma**|**=**<br /><br /> **!=, <>**<br /><br /> **>**<br /><br /> **>=**<br /><br /> **<**<br /><br /> **<=**<br /><br /> **??**|Eşittir. Döndürür **true** bağımsız değişkenleri eşit olup olmadığını döndürür **false** Aksi takdirde.<br /><br /> Eşit değil. Döndürür **true** bağımsız değişkenleri eşit değilse döndürür **false** Aksi takdirde.<br /><br /> Büyüktür. Döndürür **true** ilk bağımsız değişken ikinci sürümden daha büyükse, dönüş **false** Aksi takdirde.<br /><br /> Büyüktür veya eşittir. Döndürür **true** ilk bağımsız değişkeni sıfırdan büyük veya eşit ikincisi ise, döndürür **false** Aksi takdirde.<br /><br /> Küçüktür. Döndürür **true** ilk bağımsız değişken düşükse ikinci bir dönüş daha **false** Aksi takdirde.<br /><br /> Küçük veya eşittir. Döndürür **true** ilk bağımsız değişken ikinci eşit veya daha az ise, döndürür **false** Aksi takdirde.<br /><br /> Birleşim. İlk bağımsız değişkeni ise, ikinci bağımsız değişkeni döndürür bir **tanımsız** değeri.|  
|**Dize**|**&#124;&#124;**|Birleştirme. Her iki değişken birleşimini döndürür.|  
  
 **Üçlü işleçler:**  
  
|Üçlü işleci|?|İlk bağımsız değişken değerlendirilirse ikinci bağımsız değişkeni döndürür **true**; üçüncü bağımsız değişken yoksa döndürür.|  
|-|-|-|  
  
 **Karşılaştırma için değerlerin sıralama**  
  
|**Tür**|**Değerleri sırası**|  
|-|-|  
|**Tanımlanmamış**|Karşılaştırılabilir değil.|  
|**Null**|Tek değer: **null**|  
|**Sayı**|Doğal bir gerçek sayı.<br /><br /> Negatif sonsuz değerle diğer sayı değeri küçüktür.<br /><br /> Pozitif sonsuz değerle diğer numara değerden daha büyük. **NaN** değeri karşılaştırılabilir değil. İle karşılaştırma **NaN** sonuçlanır **tanımsız** değeri.|  
|**Dize**|Lexicographical sırası.|  
|**Dizi**|Yoktur, ancak Tarafsız sıralaması.|  
|**Nesne**|Yoktur, ancak Tarafsız sıralaması.|  
  
 **Açıklamalar**  
  
 Veritabanından alınan gerçekte kadar Azure Cosmos DB'de değerlerin türleri bilinen genellikle değil. Sorguları verimli yürütülmesini desteklemek için işleçler çoğunu sıkı tür gereksinimleri vardır. Ayrıca kendilerini işleçleriyle örtük dönüşümler gerçekleştirmeyin.  
  
 Bu sorguda ister anlamına gelir: seçin * gelen kök r WHERE r.Age = 21 yalnızca döndürecektir özelliği geçerlilik süresi ile belgeleri sayısına eşit 21. Belgeleri özelliği geçerlilik süresi dizesi "21" veya "0021" dizesi eşit ile eşleşmez, "21" ifadesi olarak = 21 çok tanımsız değerlendirir. Dizinlerin, daha iyi kullanımı için çünkü böylece belirli bir değer arama (yani 21 sayı) (yani numarası 21 veya dizeler "21", "021", "21.0"...) olası eşleşmeler sonsuz sayıda ara daha hızlıdır. Bu, JavaScript farklı türlerde değerler işleçlerini nasıl değerlendirir alanından farklıdır.  
  
 **Diziler ve nesneleri eşitlik ve karşılaştırma**  
  
 Aralık işleçleri kullanarak dizi veya nesne değerleri karşılaştırma (>, > =, <, < =) üzerinde nesne ya da dizi değerleri tanımlanan sırasını değil olarak tanımlanmamış sonuçlanır. Ancak eşitlik/eşitsizlik işleçlerini kullanma (=,! =, <>) desteklenir ve değerleri karşılaştırılabilir yapısal olarak.  
  
 Diziler, her iki diziler aynı sayıda öğe varsa ve konumlarını eşleşen adresindeki öğeleri de eşit eşit. Tüm öğeleri sonuçlarında çiftinin karşılaştırma tanımlanmamış, dizi karşılaştırma sonucu tanımlanmamıştır.  
  
 Her iki nesne tanımlanan aynı özelliklere sahipse ve Özellikler eşleşen değerleri de aynıysa eşit nesneleridir. Nesne karşılaştırma sonucunu, herhangi bir özellik değerleri sonuçlarında çifti karşılaştırma tanımlanmamış, tanımlanmamıştır.  
  
##  <a name="bk_constants"></a>Sabitleri  
 Bir sabit olarak da bilinen bir hazır değer veya bir skaler değere belirli veri değeri temsil eden bir simge ' dir. Bir sabit biçimi temsil ettiği değer veri türüne bağlıdır.  
  
 **Skaler veri türleri desteklenir:**  
  
|**Tür**|**Değerleri sırası**|  
|-|-|  
|**Tanımlanmamış**|Tek değer: **tanımlanmamış**|  
|**Null**|Tek değer: **null**|  
|**Boole değeri**|Değerler: **false**, **doğru**.|  
|**Sayı**|Çift duyarlıklı kayan noktalı sayı, standart IEEE 754.|  
|**Dize**|Sıfır veya daha fazla Unicode karakter dizisi. Dizeleri tek veya çift tırnak içine alınmalıdır.|  
|**Dizi**|Sıfır veya daha fazla öğeleri dizisi. Her öğe tanımlanmamış dışında herhangi bir skaler veri türü değeri olabilir.|  
|**Nesne**|Sırasız bir sıfır veya daha fazla ad/değer çiftleri kümesi. Adı bir UNICODE dizesi, değeri dışında herhangi bir skaler veri türde olabilir **tanımlanmamış**.|  
  
 **Sözdizimi**  
  
```  
<constant> ::=  
   <undefined_constant>  
     | <null_constant>   
     | <boolean_constant>   
     | <number_constant>   
     | <string_constant>   
     | <array_constant>   
     | <object_constant>   
  
<undefined_constant> ::= undefined  
  
<null_constant> ::= null  
  
<boolean_constant> ::= false | true  
  
<number_constant> ::= decimal_literal | hexadecimal_literal  
  
<string_constant> ::= string_literal  
  
<array_constant> ::=  
    '[' [<constant>][,...n] ']'  
  
<object_constant> ::=   
   '{' [{property_name | "property_name"} : <constant>][,...n] '}'  
  
```  
  
 **Bağımsız değişkenler**  
  
1.  `<undefined_constant>; undefined`  
  
     Türü tanımlanmamış değeri tanımsız temsil eder.  
  
2.  `<null_constant>; null`  
  
     Temsil eden **null** türü değeri **Null**.  
  
3.  `<boolean_constant>`  
  
     Boolean türünde sabiti temsil eder.  
  
4.  `false`  
  
     Temsil eden **yanlış** türü Boolean değeri.  
  
5.  `true`  
  
     Temsil eden **true** türü Boolean değeri.  
  
6.  `<number_constant>`  
  
     Bir sabiti temsil eder.  
  
7.  `decimal_literal`  
  
     Ondalık değişmez değerleri ondalık sayı veya bilimsel gösterim kullanılarak temsil numaralarıdır.  
  
8.  `hexadecimal_literal`  
  
     Onaltılık değişmez değerler, önek '0 x bir veya daha fazla onaltılık basamak ile izlenen' kullanılarak temsil numaralarıdır.  
  
9. `<string_constant>`  
  
     Dize türünde bir sabiti temsil eder.  
  
10. `string _literal`  
  
     Dize değişmez değerleri, sıfır veya daha fazla Unicode karakter dizisi veya kaçış sıraları tarafından temsil edilen Unicode dizelerdir. Dize değişmez değerleri tek tırnak içine alınmış (kesme işareti: ') veya çift tırnak (tırnak işareti: ").  
  
 Aşağıdaki kaçış sıralarına izin vermesi:  
  
|**Kaçış sırası**|**Açıklama**|**Unicode karakter**|  
|-|-|-|  
|\\'|kesme işareti (')|U + 0027|  
|\\"|tırnak işareti (")|U + 0022|  
|\\\|Ters solidus (\\)|U + 005C|  
|\\/|solidus (/)|U + 002F|  
|\b|Geri Al|U + 0008|  
|\f|sonraki sayfaya geçme|U + 000C|  
|\n|satır besleme|U + 000A|  
|\r|satır başı|U + 000D|  
|\t|Sekmesi|U + 0009|  
|\uXXXX|4 onaltılık basamak tarafından tanımlanan bir Unicode karakter.|U + XXXX|  
  
##  <a name="bk_query_perf_guidelines"></a>Sorgu performans kuralları  
 Büyük bir koleksiyon için verimli bir şekilde yürütülmek üzere bir sorgu için sırasıyla bir veya daha fazla dizinleri sunulan filtreleri kullanmanız gerekir.  
  
 Aşağıdaki filtreler için dizin arama olarak kabul edilir:  
  
-   Eşitlik işleci (=), bir belge yol ifadesi ve bir sabit ile kullanın.  
  
-   Aralık işleçleri kullanın (<, \<=, >, > =) bir belge yol ifadesi ve sayı sabitleri.  
  
-   Belge yol ifadesi sabit bir yol başvurulan veritabanı koleksiyonundan belgelerdeki tanımlayan herhangi bir ifade gösterir.  
  
 **Belge yol ifadesi**  
  
 Belge yol ifadelerini ifadeleri, özelliği veya dizi dizin oluşturucu assessors veritabanı koleksiyon belgelerden gelen bir belge üzerinden yolu. Bu yol, doğrudan veritabanı koleksiyonundaki belgelerde bir filtrede başvurulan değerleri konumunu tanımlamak için kullanılabilir.  
  
 Bir ifadenin bir belge yol ifadesi olarak kabul edilmesi bunu yapmalısınız:  
  
1.  Koleksiyon kök doğrudan başvurun.  
  
2.  Bazı belge yol ifadesi başvuru özelliği ya da sabiti dizi Oluşturucusu  
  
3.  Bazı belge yol ifadesi temsil eden bir diğer ad başvuru.  
  
     **Sözdizimi kuralları**  
  
     Aşağıdaki tabloda, DocumentDB API sorgu dili başvurusu sözdizimi tanımlamak için kullanılan kuralları açıklar.  
  
    |**Kuralı**|**İçin kullanılır**|  
    |-|-|    
    |BÜYÜK HARF|Büyük küçük harf duyarlı anahtar sözcükler.|  
    |küçük harf|Büyük küçük harfe duyarlı anahtar sözcükler.|  
    |\<nonterminal >|Terminal dışı, ayrı olarak tanımlı.|  
    |\<nonterminal >:: =|Nonterminal sözdizimi tanımı.|  
    |other_terminal|Sözcük içindeki ayrıntısı açıklanan Terminal (belirteç).|  
    |Tanımlayıcı|Tanımlayıcı. Aşağıdaki karakterleri yalnızca sağlar: a-z A-Z 0-9 _First karakter, bir sayı olamaz.|  
    |"dize"|Tırnak işaretli dizesi. Geçerli bir dize verir. Bir string_literal açıklamasına bakın.|  
    |'simgesi'|Sözdizimi parçası olan değişmez değer simge.|  
    |&#124; (dikey çubuk)|Alternatifleri sözdizimi öğeleri için. Belirtilen öğeleri yalnızca birini kullanabilirsiniz.|  
    |[] /(brackets)|Köşeli bir veya daha fazla isteğe bağlı öğeleri kapatın.|  
    |[,...n]|Önündeki öğeyi yinelenen n kaç kez uygulanıp uygulanamayacağını gösterir. Yineleme virgülle ayrılır.|  
    |[...n]|Önündeki öğeyi yinelenen n kaç kez uygulanıp uygulanamayacağını gösterir. Yineleme boşlukla ayrılır.|  
  
##  <a name="bk_built_in_functions"></a>Yerleşik işlevler  
 Azure Cosmos DB birçok yerleşik SQL işlevleri sağlar. Yerleşik işlevler kategorilerini aşağıda listelenmiştir.  
  
|İşlevi|Açıklama|  
|--------------|-----------------|  
|[Matematik işlevleri](#bk_mathematical_functions)|Matematik işlevleri her genellikle, bağımsız değişken olarak sağlanır ve sayısal bir değeri döndürme giriş değerlerine göre bir hesaplama gerçekleştirir.|  
|[Denetimi işlevleri yazın](#bk_type_checking_functions)|Tür denetleme işlevleri SQL sorguları içinde bir ifade türünü kontrol olanak sağlar.|  
|[Dize işlevleri](#bk_string_functions)|Dize işlevleri dizesi giriş değeri üzerinde bir işlemi gerçekleştirmek ve bir dize, sayısal ya da Boole değeri döndürür.|  
|[Dizi işlevleri](#bk_array_functions)|Dizi işlevleri bir Boole değeri veya dizi değeri, bir dizi giriş değeri ve return sayısal işlem gerçekleştirir.|  
|[Uzamsal işlevleri](#bk_spatial_functions)|Uzamsal işlevleri uzamsal nesne giriş değeri üzerinde bir işlemi gerçekleştirmek ve bir sayısal ya da Boole değeri döndürür.|  
  
###  <a name="bk_mathematical_functions"></a>Matematik işlevleri  
 Aşağıdaki işlevleri her genellikle, bağımsız değişken olarak sağlanır ve sayısal bir değeri döndürme giriş değerlerine göre bir hesaplama gerçekleştirir.  
  
||||  
|-|-|-|  
|[ABS](#bk_abs)|[ACOS](#bk_acos)|[ASIN](#bk_asin)|  
|[ATAN](#bk_atan)|[ATN2](#bk_atn2)|[TAVAN](#bk_ceiling)|  
|[COS](#bk_cos)|[COT](#bk_cot)|[DERECE](#bk_degrees)|  
|[EXP](#bk_exp)|[KAT](#bk_floor)|[GÜNLÜK](#bk_log)|  
|[LOG10](#bk_log10)|[PI](#bk_pi)|[GÜÇ](#bk_power)|  
|[RADYAN CİNSİNDEN](#bk_radians)|[YUVARLAK](#bk_round)|[SIN](#bk_sin)|  
|[SQRT](#bk_sqrt)|[KARE](#bk_square)|[OTURUM](#bk_sign)|  
|[TAN](#bk_tan)|[TRUNC](#bk_trunc)||  
  
####  <a name="bk_abs"></a>ABS  
 Belirtilen sayısal ifade (pozitif) mutlak değerini döndürür.  
  
 **Sözdizimi**  
  
```  
ABS (<numeric_expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnekte, üç farklı numaralarında ABS işlevi kullanarak sonuçlarını gösterir.  
  
```  
SELECT ABS(-1), ABS(0), ABS(1)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: 1, $2: 0, $3: 1}]  
```  
  
####  <a name="bk_acos"></a>ACOS  
 Açının kosinüsü belirtilen sayısal ifadesidir radyan cinsinden döndürür; arccosine olarak da bilinir.  
  
 **Sözdizimi**  
  
```  
ACOS(<numeric_expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek ACOS-1 döndürür.  
  
```  
SELECT ACOS(-1)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": 3.1415926535897931}]  
```  
  
####  <a name="bk_asin"></a>ASIN  
 Açının sinüsü belirtilen sayısal ifadesidir radyan cinsinden döndürür. Bu arksinüsünü olarak da adlandırılır.  
  
 **Sözdizimi**  
  
```  
ASIN(<numeric_expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek ASIN-1 döndürür.  
  
```  
SELECT ASIN(-1)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": -1.5707963267948966}]  
```  
  
####  <a name="bk_atan"></a>ATAN  
 Açının tanjantı belirtilen sayısal ifadesidir radyan cinsinden döndürür. Bu arktanjantını olarak da adlandırılır.  
  
 **Sözdizimi**  
  
```  
ATAN(<numeric_expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, belirtilen değer ATAN döndürür.  
  
```  
SELECT ATAN(-45.01)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": -1.5485826962062663}]  
```  
  
####  <a name="bk_atn2"></a>ATN2  
 Radyan cinsinden ifade edilen y / x, ark tanjantını döndürür.  
  
 **Sözdizimi**  
  
```  
ATN2(<numeric_expression>, <numeric_expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek için belirtilen ATN2 hesaplar x ve y bileşenleri.  
  
```  
SELECT ATN2(35.175643, 129.44)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": 1.3054517947300646}]  
```  
  
####  <a name="bk_ceiling"></a>TAVAN  
 Büyüktür veya eşittir, belirtilen sayısal ifadenin en küçük tamsayı değeri döndürür.  
  
 **Sözdizimi**  
  
```  
CEILING (<numeric_expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, pozitif sayısal, negatif ve TAVAN işlevi ile sıfır değerleri gösterir.  
  
```  
SELECT CEILING(123.45), CEILING(-123.45), CEILING(0.0)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: 124, $2: -123, $3: 0}]  
```  
  
####  <a name="bk_cos"></a>COS  
 Radyan cinsinden belirtilen ifade trigonometrik belirtilen açının kosinüsünü döndürür.  
  
 **Sözdizimi**  
  
```  
COS(<numeric_expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, belirtilen açının COS hesaplar.  
  
```  
SELECT COS(14.78)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": -0.59946542619465426}]  
```  
  
####  <a name="bk_cot"></a>COT  
 Belirtilen açının trigonometrik kotanjantını radyan cinsinden belirtilen sayısal ifade döndürür.  
  
 **Sözdizimi**  
  
```  
COT(<numeric_expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, belirtilen açının COT hesaplar.  
  
```  
SELECT COT(124.1332)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": -0.040311998371148884}]  
```  
  
####  <a name="bk_degrees"></a>DERECE  
 Radyan cinsinden Açı derece cinsinden karşılık gelen açıyı döndürür.  
  
 **Sözdizimi**  
  
```  
DEGREES (<numeric_expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, PI/2 radyan cinsinden Açı derece sayısını döndürür.  
  
```  
SELECT DEGREES(PI()/2)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": 90}]  
```  
  
####  <a name="bk_floor"></a>KAT  
 Belirtilen sayısal ifade küçük veya eşit en büyük tamsayıyı döndürür.  
  
 **Sözdizimi**  
  
```  
FLOOR (<numeric_expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, pozitif sayısal, negatif ve FLOOR işlevi sıfır değerleri gösterir.  
  
```  
SELECT FLOOR(123.45), FLOOR(-123.45), FLOOR(0.0)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: 123, $2: -124, $3: 0}]  
```  
  
####  <a name="bk_exp"></a>EXP  
 Üstel belirtilen sayısal ifadenin değerini döndürür.  
  
 **Sözdizimi**  
  
```  
EXP (<numeric_expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal ifade döndürür.  
  
 **Açıklamalar**  
  
 Sabit **e** (2.718281...) Doğal logaritmanın tabanıdır.  
  
 Bir sayının üs. sabit olduğunda **e** sayının üssünün. Örneğin EXP(1.0) e = ^ 1.0 = 2.71828182845905 ve EXP(10) e = ^ 10 = 22026.4657948067.  
  
 Bir sayının doğal logaritmasını üstel sayıdır kendisini: EXP (günlüğü (n)) = n. Ve üstel bir sayının doğal logaritmasını sayı kendisini: günlük (EXP (n)) = n.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, bir değişken bildirir ve belirtilen değişkeni (10) üstel değerini döndürür.  
  
```  
SELECT EXP(10)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: 22026.465794806718}]  
```  
  
 Aşağıdaki örnek üstel değeri 20 natural logarithm ve üstel 20 doğal logaritmasını döndürür. Bu işlevler inverse işlevleri birbirinden olduğundan, dönüş değeri için her iki durumda da kayan nokta matematik yuvarlama ile 20'dir.  
  
```  
SELECT EXP(LOG(20)), LOG(EXP(20))  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: 19.999999999999996, $2: 20}]  
```  
  
####  <a name="bk_log"></a>GÜNLÜK  
 Belirtilen sayısal ifade doğal logaritmasını döndürür.  
  
 **Sözdizimi**  
  
```  
LOG (<numeric_expression> [, <base>])  
```  
  
 **Bağımsız değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
-   `base`  
  
     Logaritmanın tabanı ayarlar isteğe bağlı sayısal değişkeni.  
  
 **Dönüş türleri**  
  
 Sayısal ifade döndürür.  
  
 **Açıklamalar**  
  
 Varsayılan olarak, LOG() doğal logaritmasını döndürür. İsteğe bağlı temel parametresini kullanarak başka bir değere logaritmanın tabanı değiştirebilirsiniz.  
  
 Logaritmanın tabanı için doğal logaritmasını olan **e**, burada **e** Irrational sabiti 2.718281828 için yaklaşık eşittir.  
  
 Üstel bir sayının doğal logaritmasını sayıdır kendisini: günlük (EXP (n)) = n. Ve sayının doğal logaritmasını Üstel sayı kendisini: EXP (günlüğü (n)) = n.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, bir değişken bildirir ve belirtilen değişkeni (10) logaritmasını değerini döndürür.  
  
```  
SELECT LOG(10)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: 2.3025850929940459}]  
```  
  
 Aşağıdaki örnek, bir sayı üs için günlük hesaplar.  
  
```  
SELECT EXP(LOG(10))  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: 10.000000000000002}]  
```  
  
####  <a name="bk_log10"></a>LOG10  
 Belirtilen sayısal ifade 10 tabanında logaritmasını döndürür.  
  
 **Sözdizimi**  
  
```  
LOG10 (<numeric_expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal ifade döndürür.  
  
 **Açıklamalar**  
  
 LOG10 ve güç işlevleri inversely birbirleriyle ilişkilidir. Örneğin, 10 ^ LOG10(n) = n.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, bir değişken bildirir ve belirtilen değişkeni (100) LOG10 değerini döndürür.  
  
```  
SELECT LOG10(100)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: 2}]  
```  
  
####  <a name="bk_pi"></a>PI  
 PI sayısının sabit değeri döndürür.  
  
 **Sözdizimi**  
  
```  
PI ()  
```  
  
 **Bağımsız değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek PI değerini döndürür.  
  
```  
SELECT PI()  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": 3.1415926535897931}]  
```  
  
####  <a name="bk_power"></a>GÜÇ  
 Belirtilen güç belirtilen ifadenin değerini döndürür.  
  
 **Sözdizimi**  
  
```  
POWER (<numeric_expression>, <y>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
-   `y`  
  
     Hangi yükseltmek güç `numeric_expression`.  
  
 **Dönüş türleri**  
  
 Sayısal ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, bir sayı (sayının küp) 3 üssüne yükseltme gösterir.  
  
```  
SELECT POWER(2, 3), POWER(2.5, 3)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: 8, $2: 15.625}]  
```  
  
####  <a name="bk_radians"></a>RADYAN CİNSİNDEN  
 Derece sayısal bir ifadenin girildiğinde radyan cinsinden döndürür.  
  
 **Sözdizimi**  
  
```  
RADIANS (<numeric_expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, birkaç açıları girdi olarak alır ve bunlara karşılık gelen radian değerler döndürür.  
  
```  
SELECT RADIANS(-45.01), RADIANS(-181.01), RADIANS(0), RADIANS(0.1472738), RADIANS(197.1099392)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{  
       "$1": -0.7855726963226477,  
       "$2": -3.1592204790349356,  
       "$3": 0,  
       "$4": 0.0025704127119236249,  
       "$5": 3.4402174274458375  
   }]  
```  
  
####  <a name="bk_round"></a>YUVARLAK  
 En yakın tamsayı değerine yuvarlanan sayısal bir değer döndürür.  
  
 **Sözdizimi**  
  
```  
ROUND(<numeric_expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek şu pozitif ve negatif sayıları en yakın tamsayıya yuvarlar.  
  
```  
SELECT ROUND(2.4), ROUND(2.6), ROUND(2.5), ROUND(-2.4), ROUND(-2.6)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: 2, $2: 3, $3: 3, $4: -2, $5: -3}]  
```  
  
####  <a name="bk_sign"></a>OTURUM  
 Artı (+ 1), sıfır (0) veya belirtilen sayısal ifadenin eksi (-1) işareti döndürür.  
  
 **Sözdizimi**  
  
```  
SIGN(<numeric_expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, 2 -2 ' sayıların oturum değerleri döndürür.  
  
```  
SELECT SIGN(-2), SIGN(-1), SIGN(0), SIGN(1), SIGN(2)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: -1, $2: -1, $3: 0, $4: 1, $5: 1}]  
```  
  
####  <a name="bk_sin"></a>SIN  
 Radyan cinsinden belirtilen ifade trigonometrik belirtilen açının sinüsünü döndürür.  
  
 **Sözdizimi**  
  
```  
SIN(<numeric_expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, belirtilen açının SIN hesaplar.  
  
```  
SELECT SIN(45.175643)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": 0.929607286611012}]  
```  
  
####  <a name="bk_sqrt"></a>SQRT  
 Belirtilen sayısal değer kare kökünü döndürür.  
  
 **Sözdizimi**  
  
```  
SQRT(<numeric_expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, 1-3 sayının kare kökleri döndürür.  
  
```  
SELECT SQRT(1), SQRT(2.0), SQRT(3)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: 1, $2: 1.4142135623730952, $3: 1.7320508075688772}]  
```  
  
####  <a name="bk_square"></a>KARE  
 Kare belirtilen sayısal değeri döndürür.  
  
 **Sözdizimi**  
  
```  
SQUARE(<numeric_expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek numaraları 1-3 kareleri döndürür.  
  
```  
SELECT SQUARE(1), SQUARE(2.0), SQUARE(3)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: 1, $2: 4, $3: 9}]  
```  
  
####  <a name="bk_tan"></a>TAN  
 Belirtilen ifadedeki radyan cinsinden belirtilen açının tanjantını döndürür.  
  
 **Sözdizimi**  
  
```  
TAN (<numeric_expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek PI () tanjantını hesaplar / 2.  
  
```  
SELECT TAN(PI()/2);  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": 16331239353195370 }]  
```  
  
####  <a name="bk_trunc"></a>TRUNC  
 En yakın tamsayı değerine kesilmiş sayısal bir değer döndürür.  
  
 **Sözdizimi**  
  
```  
TRUNC(<numeric_expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `numeric_expression`  
  
     Sayısal bir ifadedir.  
  
 **Dönüş türleri**  
  
 Sayısal ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek şu pozitif ve negatif sayıları yakın tamsayı değeri tamsayıya dönüştürür.  
  
```  
SELECT TRUNC(2.4), TRUNC(2.6), TRUNC(2.5), TRUNC(-2.4), TRUNC(-2.6)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: 2, $2: 2, $3: 2, $4: -2, $5: -2}]  
```  
  
###  <a name="bk_type_checking_functions"></a>Denetimi işlevleri yazın  
 Aşağıdaki işlevleri karşı giriş değerleri denetleme türünü destekleyen ve her bir Boole değeri döndürür.  
  
||||  
|-|-|-|  
|[IS_ARRAY](#bk_is_array)|[IS_BOOL](#bk_is_bool)|[IS_DEFINED](#bk_is_defined)|  
|[IS_NULL](#bk_is_null)|[IS_NUMBER](#bk_is_number)|[IS_OBJECT](#bk_is_object)|  
|[IS_PRIMITIVE](#bk_is_primitive)|[IS_STRING](#bk_is_string)||  
  
####  <a name="bk_is_array"></a>IS_ARRAY  
 Belirtilen ifade türü bir dizi olup olmadığını gösteren bir Boole değeri döndürür.  
  
 **Sözdizimi**  
  
```  
IS_ARRAY(<expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `expression`  
  
     Geçerli bir ifade değil.  
  
 **Dönüş türleri**  
  
 Boole ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, JSON Boolean, sayı, dize, null nesneleri, nesne, dizi ve IS_ARRAY işlevini kullanarak tanımsız türleri denetler.  
  
```  
SELECT   
 IS_ARRAY(true),   
 IS_ARRAY(1),  
 IS_ARRAY("value"),  
 IS_ARRAY(null),  
 IS_ARRAY({prop: "value"}),   
 IS_ARRAY([1, 2, 3]),  
 IS_ARRAY({prop: "value"}.prop2)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: false, $2: false, $3: false, $4: false, $5: false, $6: true}]  
```  
  
####  <a name="bk_is_bool"></a>IS_BOOL  
 Belirtilen ifade türü bir Boole değeri olup olmadığını gösteren bir Boole değeri döndürür.  
  
 **Sözdizimi**  
  
```  
IS_BOOL(<expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `expression`  
  
     Geçerli bir ifade değil.  
  
 **Dönüş türleri**  
  
 Boole ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, JSON Boolean, sayı, dize, null nesneleri, nesne, dizi ve IS_BOOL işlevini kullanarak tanımsız türleri denetler.  
  
```  
SELECT   
    IS_BOOL(true),   
    IS_BOOL(1),  
    IS_BOOL("value"),   
    IS_BOOL(null),  
    IS_BOOL({prop: "value"}),   
    IS_BOOL([1, 2, 3]),  
    IS_BOOL({prop: "value"}.prop2)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: true, $2: false, $3: false, $4: false, $5: false, $6: false}]  
```  
  
####  <a name="bk_is_defined"></a>IS_DEFINED  
 Özellik değeri atanmış olan gösteren bir Boole değeri döndürür.  
  
 **Sözdizimi**  
  
```  
IS_DEFINED(<expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `expression`  
  
     Geçerli bir ifade değil.  
  
 **Dönüş türleri**  
  
 Boole ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek belirtilen JSON belgesi içinde bir özellik varlığını denetler. İlk, "a" var, ancak "b" mevcut olduğundan ikinci false döndürür beri true değerini döndürür.  
  
```  
SELECT IS_DEFINED({ "a" : 5 }.a), IS_DEFINED({ "a" : 5 }.b)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{  
       "$1": true,    
       "$2": false   
   }]  
```  
  
####  <a name="bk_is_null"></a>IS_NULL  
 Belirtilen ifade türü null olup olmadığını gösteren bir Boole değeri döndürür.  
  
 **Sözdizimi**  
  
```  
IS_NULL(<expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `expression`  
  
     Geçerli bir ifade değil.  
  
 **Dönüş türleri**  
  
 Boole ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, JSON Boolean, sayı, dize, null nesneleri, nesne, dizi ve IS_NULL işlevini kullanarak tanımsız türleri denetler.  
  
```  
SELECT   
    IS_NULL(true),   
    IS_NULL(1),  
    IS_NULL("value"),   
    IS_NULL(null),  
    IS_NULL({prop: "value"}),   
    IS_NULL([1, 2, 3]),  
    IS_NULL({prop: "value"}.prop2)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: false, $2: false, $3: false, $4: true, $5: false, $6: false}]  
```  
  
####  <a name="bk_is_number"></a>IS_NUMBER  
 Belirtilen ifade türü bir sayı olup olmadığını gösteren bir Boole değeri döndürür.  
  
 **Sözdizimi**  
  
```  
IS_NUMBER(<expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `expression`  
  
     Geçerli bir ifade değil.  
  
 **Dönüş türleri**  
  
 Boole ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, JSON Boolean, sayı, dize, null nesneleri, nesne, dizi ve IS_NULL işlevini kullanarak tanımsız türleri denetler.  
  
```  
SELECT   
    IS_NUMBER(true),   
    IS_NUMBER(1),  
    IS_NUMBER("value"),   
    IS_NUMBER(null),  
    IS_NUMBER({prop: "value"}),   
    IS_NUMBER([1, 2, 3]),  
    IS_NUMBER({prop: "value"}.prop2)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: false, $2: true, $3: false, $4: false, $5: false, $6: false}]  
```  
  
####  <a name="bk_is_object"></a>IS_OBJECT  
 Belirtilen ifade türü bir JSON nesnesi olup olmadığını gösteren bir Boole değeri döndürür.  
  
 **Sözdizimi**  
  
```  
IS_OBJECT(<expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `expression`  
  
     Geçerli bir ifade değil.  
  
 **Dönüş türleri**  
  
 Boole ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, JSON Boolean, sayı, dize, null nesneleri, nesne, dizi ve IS_OBJECT işlevini kullanarak tanımsız türleri denetler.  
  
```  
SELECT   
    IS_OBJECT(true),   
    IS_OBJECT(1),  
    IS_OBJECT("value"),   
    IS_OBJECT(null),  
    IS_OBJECT({prop: "value"}),   
    IS_OBJECT([1, 2, 3]),  
    IS_OBJECT({prop: "value"}.prop2)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: false, $2: false, $3: false, $4: false, $5: true, $6: false}]  
```  
  
####  <a name="bk_is_primitive"></a>IS_PRIMITIVE  
 Belirtilen ifade türü bir basit tür olup olmadığını gösteren bir Boole değeri döndürür (dize, Boolean, sayısal veya null).  
  
 **Sözdizimi**  
  
```  
IS_PRIMITIVE(<expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `expression`  
  
     Geçerli bir ifade değil.  
  
 **Dönüş türleri**  
  
 Boole ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, JSON Boolean, sayı, dize, null nesneleri, nesne, dizi ve IS_PRIMITIVE işlevini kullanarak tanımsız türleri denetler.  
  
```  
SELECT   
           IS_PRIMITIVE(true),   
           IS_PRIMITIVE(1),  
           IS_PRIMITIVE("value"),   
           IS_PRIMITIVE(null),  
           IS_PRIMITIVE({prop: "value"}),   
           IS_PRIMITIVE([1, 2, 3]),  
           IS_PRIMITIVE({prop: "value"}.prop2)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": true, "$2": true, "$3": true, "$4": true, "$5": false, "$6": false, "$7": false}]  
```  
  
####  <a name="bk_is_string"></a>IS_STRING  
 Belirtilen ifade türü bir dize olup olmadığını gösteren bir Boole değeri döndürür.  
  
 **Sözdizimi**  
  
```  
IS_STRING(<expression>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `expression`  
  
     Geçerli bir ifade değil.  
  
 **Dönüş türleri**  
  
 Boole ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, JSON Boolean, sayı, dize, null nesneleri, nesne, dizi ve IS_STRING işlevini kullanarak tanımsız türleri denetler.  
  
```  
SELECT   
       IS_STRING(true),   
       IS_STRING(1),  
       IS_STRING("value"),   
       IS_STRING(null),  
       IS_STRING({prop: "value"}),   
       IS_STRING([1, 2, 3]),  
       IS_STRING({prop: "value"}.prop2)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{$1: false, $2: false, $3: true, $4: false, $5: false, $6: false}]  
```  
  
###  <a name="bk_string_functions"></a>Dize işlevleri  
 Aşağıdaki skaler işlevler dize giriş değeri üzerinde bir işlemi gerçekleştirmek ve bir dize, sayısal ya da Boole değeri döndürür.  
  
||||  
|-|-|-|  
|[CONCAT](#bk_concat)|[İÇERİR](#bk_contains)|[ENDSWITH](#bk_endswith)|  
|[INDEX_OF](#bk_index_of)|[SOL](#bk_left)|[UZUNLUĞU](#bk_length)|  
|[DAHA DÜŞÜK](#bk_lower)|[LTRIM](#bk_ltrim)|[DEĞİŞTİR](#bk_replace)|  
|[ÇOĞALTILAN](#bk_replicate)|[TERS ÇEVİR](#bk_reverse)|[SAĞ](#bk_right)|  
|[RTRIM](#bk_rtrim)|[STARTSWITH](#bk_startswith)|[SUBSTRING](#bk_substring)|  
|[ÜST](#bk_upper)|||  
  
####  <a name="bk_concat"></a>CONCAT  
 İki veya daha fazla dize değerlerini birleştirme sonucu olan bir dize döndürür.  
  
 **Sözdizimi**  
  
```  
CONCAT(<str_expr>, <str_expr> [, <str_expr>])  
```  
  
 **Bağımsız değişkenler**  
  
-   `str_expr`  
  
     Tüm geçerli bir dize ifadesidir.  
  
 **Dönüş türleri**  
  
 Bir dize ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, belirtilen değerler birleştirilmiş dizeyi döndürür.  
  
```  
SELECT CONCAT("abc", "def")  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": "abcdef"}  
```  
  
####  <a name="bk_contains"></a>İÇERİR  
 Döndüren bir Boolean belirten ikinci ilk ifade dize olup olmadığını içerir.  
  
 **Sözdizimi**  
  
```  
CONTAINS(<str_expr>, <str_expr>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `str_expr`  
  
     Tüm geçerli bir dize ifadesidir.  
  
 **Dönüş türleri**  
  
 Boole ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnekte, "abc" "ab" ve "d" içerir olmadığını denetler.  
  
```  
SELECT CONTAINS("abc", "ab"), CONTAINS("abc", "d")  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": true, "$2": false}]  
```  
  
####  <a name="bk_endswith"></a>ENDSWITH  
 Döndürür Boolean belirten bir ilk ifade dize olup olmadığını ve ikinci sona erer.  
  
 **Sözdizimi**  
  
```  
ENDSWITH(<str_expr>, <str_expr>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `str_expr`  
  
     Tüm geçerli bir dize ifadesidir.  
  
 **Dönüş türleri**  
  
 Boole ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnekte, "abc", "b" ve "bc" ile biten döndürür.  
  
```  
SELECT ENDSWITH("abc", "b"), ENDSWITH("abc", "bc")  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": false, "$2": true}]  
```  
  
####  <a name="bk_index_of"></a>INDEX_OF  
 İkinci ilk örneğinin başlangıç konumunu döndürür dizesi ifade ilk belirtilen dize ifadesi veya -1 içinde dizesi bulunamadı.  
  
 **Sözdizimi**  
  
```  
INDEX_OF(<str_expr>, <str_expr>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `str_expr`  
  
     Tüm geçerli bir dize ifadesidir.  
  
 **Dönüş türleri**  
  
 Sayısal ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnekte, "abc" içinde çeşitli alt dizeler dizinini döndürür.  
  
```  
SELECT INDEX_OF("abc", "ab"), INDEX_OF("abc", "b"), INDEX_OF("abc", "c")  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": 0, "$2": 1, "$3": -1}]  
```  
  
####  <a name="bk_left"></a>SOL  
 Sol bölümü belirtilen sayıda karakteri içeren bir dize döndürür.  
  
 **Sözdizimi**  
  
```  
LEFT(<str_expr>, <num_expr>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `str_expr`  
  
     Tüm geçerli bir dize ifadesidir.  
  
-   `num_expr`  
  
     Geçerli herhangi bir sayısal ifadeye ' dir.  
  
 **Dönüş türleri**  
  
 Bir dize ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnekte "abc" sol bölümü için çeşitli uzunluk değeri döndürür.  
  
```  
SELECT LEFT("abc", 1), LEFT("abc", 2)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": "a", "$2": "ab"}]  
```  
  
####  <a name="bk_length"></a>UZUNLUĞU  
 Belirtilen dize ifadesinin karakterlerin sayısını döndürür.  
  
 **Sözdizimi**  
  
```  
LENGTH(<str_expr>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `str_expr`  
  
     Tüm geçerli bir dize ifadesidir.  
  
 **Dönüş türleri**  
  
 Bir dize ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, bir dize uzunluğunu döndürür.  
  
```  
SELECT LENGTH("abc")  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": 3}]  
```  
  
####  <a name="bk_lower"></a>DAHA DÜŞÜK  
 Büyük harf karakter verileri küçük harfe dönüştürmek sonra bir dize ifadesi döndürür.  
  
 **Sözdizimi**  
  
```  
LOWER(<str_expr>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `str_expr`  
  
     Tüm geçerli bir dize ifadesidir.  
  
 **Dönüş türleri**  
  
 Bir dize ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek sorguda düşük kullanmayı gösterir.  
  
```  
SELECT LOWER("Abc")  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": "abc"}]  
  
```  
  
####  <a name="bk_ltrim"></a>LTRIM  
 Öndeki boşlukları kaldırır sonra bir dize ifadesi döndürür.  
  
 **Sözdizimi**  
  
```  
LTRIM(<str_expr>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `str_expr`  
  
     Tüm geçerli bir dize ifadesidir.  
  
 **Dönüş türleri**  
  
 Bir dize ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek bir sorgu içinde LTRIM kullanmayı gösterir.  
  
```  
SELECT LTRIM("  abc"), LTRIM("abc"), LTRIM("abc   ")  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": "abc", "$2": "abc", "$3": "abc   "}]  
```  
  
####  <a name="bk_replace"></a>DEĞİŞTİR  
 Belirtilen dize değeri tüm oluşumlarını başka bir dize değeri ile değiştirir.  
  
 **Sözdizimi**  
  
```  
REPLACE(<str_expr>, <str_expr>, <str_expr>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `str_expr`  
  
     Tüm geçerli bir dize ifadesidir.  
  
 **Dönüş türleri**  
  
 Bir dize ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek sorguda Değiştir kullanmayı gösterir.  
  
```  
SELECT REPLACE("This is a Test", "Test", "desk")  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": "This is a desk"}]  
```  
  
####  <a name="bk_replicate"></a>ÇOĞALTMA  
 Bir dize değeri, belirtilen sayıda yineler.  
  
 **Sözdizimi**  
  
```  
REPLICATE(<str_expr>, <num_expr>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `str_expr`  
  
     Tüm geçerli bir dize ifadesidir.  
  
-   `num_expr`  
  
     Geçerli herhangi bir sayısal ifadeye ' dir.  
  
 **Dönüş türleri**  
  
 Bir dize ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek sorguda REPLICATE kullanmayı gösterir.  
  
```  
SELECT REPLICATE("a", 3)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": "aaa"}]  
```  
  
####  <a name="bk_reverse"></a>TERS ÇEVİR  
 Ters sırada bir dize değerini döndürür.  
  
 **Sözdizimi**  
  
```  
REVERSE(<str_expr>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `str_expr`  
  
     Tüm geçerli bir dize ifadesidir.  
  
 **Dönüş türleri**  
  
 Bir dize ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek sorguda ters kullanmayı gösterir.  
  
```  
SELECT REVERSE("Abc")  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": "cbA"}]  
```  
  
####  <a name="bk_right"></a>SAĞ  
 Belirtilen sayıda karakteri içeren bir dize sağ bölümünü döndürür.  
  
 **Sözdizimi**  
  
```  
RIGHT(<str_expr>, <num_expr>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `str_expr`  
  
     Tüm geçerli bir dize ifadesidir.  
  
-   `num_expr`  
  
     Geçerli herhangi bir sayısal ifadeye ' dir.  
  
 **Dönüş türleri**  
  
 Bir dize ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnekte, "abc" sağ parçası için çeşitli uzunluk değeri döndürür.  
  
```  
SELECT RIGHT("abc", 1), RIGHT("abc", 2)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": "c", "$2": "bc"}]  
```  
  
####  <a name="bk_rtrim"></a>RTRIM  
 Sondaki boşlukları kaldırır sonra bir dize ifadesi döndürür.  
  
 **Sözdizimi**  
  
```  
RTRIM(<str_expr>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `str_expr`  
  
     Tüm geçerli bir dize ifadesidir.  
  
 **Dönüş türleri**  
  
 Bir dize ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek bir sorgu içinde RTRIM kullanmayı gösterir.  
  
```  
SELECT RTRIM("  abc"), RTRIM("abc"), RTRIM("abc   ")  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": "   abc", "$2": "abc", "$3": "abc"}]  
```  
  
####  <a name="bk_startswith"></a>STARTSWITH  
 Döndüren bir Boolean belirten ilk ifade dize olup olmadığını ve ikinci başlatır.  
  
 **Sözdizimi**  
  
```  
STARTSWITH(<str_expr>, <str_expr>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `str_expr`  
  
     Tüm geçerli bir dize ifadesidir.  
  
 **Dönüş türleri**  
  
 Boole ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnekte "abc" dize "b" ile başlayıp başlamadığını denetler ve "a".  
  
```  
SELECT STARTSWITH("abc", "b"), STARTSWITH("abc", "a")  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": false, "$2": true}]  
```  
  
####  <a name="bk_substring"></a>SUBSTRING  
 Belirtilen karakter sıfır tabanlı konumdan başlayarak bir dize ifadesi bölümünü döndürür ve belirtilen uzunlukta veya dize sonu devam eder.  
  
 **Sözdizimi**  
  
```  
SUBSTRING(<str_expr>, <num_expr> [, <num_expr>])  
```  
  
 **Bağımsız değişkenler**  
  
-   `str_expr`  
  
     Tüm geçerli bir dize ifadesidir.  
  
-   `num_expr`  
  
     Geçerli herhangi bir sayısal ifadeye ' dir.  
  
 **Dönüş türleri**  
  
 Bir dize ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, 1 ve 1 karakter uzunluğu için başlangıç "abc" alt dizeyi döndürür.  
  
```  
SELECT SUBSTRING("abc", 1, 1)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": "b"}]  
```  
  
####  <a name="bk_upper"></a>ÜST  
 Küçük harf karakter verileri büyük harfe dönüştürme sonra bir dize ifadesi döndürür.  
  
 **Sözdizimi**  
  
```  
UPPER(<str_expr>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `str_expr`  
  
     Tüm geçerli bir dize ifadesidir.  
  
 **Dönüş türleri**  
  
 Bir dize ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek sorguda üst kullanmayı gösterir  
  
```  
SELECT UPPER("Abc")  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": "ABC"}]  
```  
  
###  <a name="bk_array_functions"></a>Dizi işlevleri  
 Aşağıdaki skaler işlevler bir dizi giriş değeri ve return sayısal, Boole veya dizi değer üzerinde bir işlemi gerçekleştirme  
  
||||  
|-|-|-|  
|[ARRAY_CONCAT](#bk_array_concat)|[ARRAY_CONTAINS](#bk_array_contains)|[ARRAY_LENGTH](#bk_array_length)|  
|[ARRAY_SLICE](#bk_array_slice)|||  
  
####  <a name="bk_array_concat"></a>ARRAY_CONCAT  
 İki veya daha fazla dizi değerlerini birleştirme sonucu olan bir dizi döndürür.  
  
 **Sözdizimi**  
  
```  
ARRAY_CONCAT (<arr_expr>, <arr_expr> [, <arr_expr>])  
```  
  
 **Bağımsız değişkenler**  
  
-   `arr_expr`  
  
     Tüm geçerli dizi ifadesidir.  
  
 **Dönüş türleri**  
  
 Bir dizi ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek dizilerinin birleştirmek nasıl.  
  
```  
SELECT ARRAY_CONCAT(["apples", "strawberries"], ["bananas"])  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": ["apples", "strawberries", "bananas"]}]  
```  
  
####  <a name="bk_array_contains"></a>ARRAY_CONTAINS  
Dizi belirtilen değeri içerip içermediğini gösteren bir Boole değeri döndürür. Tam veya kısmi eşleşme olup olmadığını belirtebilirsiniz. 

 **Sözdizimi**  
  
```  
ARRAY_CONTAINS (<arr_expr>, <expr> [, bool_expr])  
```  
  
 **Bağımsız değişkenler**  
  
-   `arr_expr`  
  
     Tüm geçerli dizi ifadesidir.  
  
-   `expr`  
  
     Geçerli bir ifade değil.  

-   `bool_expr`  
  
     Boole herhangi ifadesidir.       
  
 **Dönüş türleri**  
  
 Bir Boole değeri döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnekte nasıl ARRAY_CONTAINS kullanarak bir dizi üyeliği denetlenir.  
  
```  
SELECT   
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "apples"),  
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "mangoes")  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": true, "$2": false}]  
```  

 Aşağıdaki örnekte nasıl ARRAY_CONTAINS kullanarak bir dizi JSON'de kısmi bir eşleşme için denetlenir.  
  
```  
SELECT  
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "apples"}, true), 
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "apples"}),
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "mangoes"}, true) 
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{
  "$1": true,
  "$2": false,
  "$3": false
}] 
```  
  
####  <a name="bk_array_length"></a>ARRAY_LENGTH  
 Belirtilen dizi ifadesi öğe sayısını döndürür.  
  
 **Sözdizimi**  
  
```  
ARRAY_LENGTH(<arr_expr>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `arr_expr`  
  
     Tüm geçerli dizi ifadesidir.  
  
 **Dönüş türleri**  
  
 Sayısal ifade döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek ARRAY_LENGTH kullanarak bir dizi uzunluğu alma.  
  
```  
SELECT ARRAY_LENGTH(["apples", "strawberries", "bananas"])  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1": 3}]  
```  
  
####  <a name="bk_array_slice"></a>ARRAY_SLICE  
 Bir dizi ifadesi bölümünü döndürür.
  
 **Sözdizimi**  
  
```  
ARRAY_SLICE (<arr_expr>, <num_expr> [, <num_expr>])  
```  
  
 **Bağımsız değişkenler**  
  
-   `arr_expr`  
  
     Tüm geçerli dizi ifadesidir.  
  
-   `num_expr`  
  
     Geçerli herhangi bir sayısal ifadeye ' dir.  
  
 **Dönüş türleri**  
  
 Bir Boole değeri döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek ARRAY_SLICE kullanarak bir dizi parçası alma.  
  
```  
SELECT   
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1),  
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1)  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{  
           "$1": ["strawberries", "bananas"],   
           "$2": ["strawberries"]  
       }]  
```  
  
###  <a name="bk_spatial_functions"></a>Uzamsal işlevleri  
 Aşağıdaki skaler işlevler uzamsal nesne giriş değeri üzerinde bir işlemi gerçekleştirmek ve bir sayısal ya da Boole değeri döndürür.  
  
||||  
|-|-|-|  
|[ST_DISTANCE](#bk_st_distance)|[ST_WITHIN](#bk_st_within)|[ST_INTERSECTS](#bk_st_intersects)|[ST_ISVALID](#bk_st_isvalid)|  
|[ST_ISVALIDDETAILED](#bk_st_isvaliddetailed)|||  
  
####  <a name="bk_st_distance"></a>ST_DISTANCE  
 İki GeoJSON noktası, çokgen veya LineString ifadeleri uzaklığı döndürür.  
  
 **Sözdizimi**  
  
```  
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `spatial_expr`  
  
     Tüm geçerli GeoJSON noktası, çokgen veya LineString nesne ifadesidir.  
  
 **Dönüş türleri**  
  
 Uzaklığı içeren sayısal bir ifade döndürür. Bu varsayılan başvuru sistemi metre cinsinden ifade edilir.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, içinde 30 km st_dıstance yerleşik işlevini kullanarak belirtilen konumun olan tüm ailesi belgeleri döndürülecek gösterilmiştir. .  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{  
  "id": "WakefieldFamily"  
}]  
```  
  
####  <a name="bk_st_within"></a>ST_WITHIN  
 İlk bağımsız değişkeninde belirtilen GeoJSON nesne (noktası, çokgen veya LineString) ikinci bağımsız GeoJSON (noktası, çokgen veya LineString) içinde olup olmadığını gösteren bir Boole ifadesi döndürür.  
  
 **Sözdizimi**  
  
```  
ST_WITHIN (<spatial_expr>, <spatial_expr>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `spatial_expr`  
  
     Tüm geçerli GeoJSON noktası, çokgen veya LineString nesne ifadesidir.  
 
-   `spatial_expr`  
  
     Tüm geçerli GeoJSON noktası, çokgen veya LineString nesne ifadesidir.  
  
 **Dönüş türleri**  
  
 Bir Boole değeri döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, ST_WITHIN kullanarak bir Çokgen içindeki tüm ailesi belgeleri bulmak gösterilmiştir.  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_WITHIN(f.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{ "id": "WakefieldFamily" }]  
```  

####  <a name="bk_st_intersects"></a>ST_INTERSECTS  
 İlk bağımsız değişkeninde belirtilen GeoJSON nesne (noktası, çokgen veya LineString) ikinci bağımsız GeoJSON (noktası, çokgen veya LineString) kesiştiğinden olup olmadığını gösteren bir Boole ifadesi döndürür.  
  
 **Sözdizimi**  
  
```  
ST_INTERSECTS (<spatial_expr>, <spatial_expr>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `spatial_expr`  
  
     Tüm geçerli GeoJSON noktası, çokgen veya LineString nesne ifadesidir.  
 
-   `spatial_expr`  
  
     Tüm geçerli GeoJSON noktası, çokgen veya LineString nesne ifadesidir.  
  
 **Dönüş türleri**  
  
 Bir Boole değeri döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek ile belirtilen Çokgen kesiştiğinden tüm alanlar bulmak nasıl gösterir.  
  
```  
SELECT a.id   
FROM Areas a   
WHERE ST_INTERSECTS(a.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{ "id": "IntersectingPolygon" }]  
```  
  
####  <a name="bk_st_isvalid"></a>ST_ISVALID  
 Belirtilen GeoJSON noktası, çokgen veya LineString ifade geçerli olup olmadığını gösteren bir Boole değeri döndürür.  
  
 **Sözdizimi**  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `spatial_expr`  
  
     Tüm geçerli GeoJSON noktası, çokgen veya LineString ifadesidir.  
  
 **Dönüş türleri**  
  
 Boole ifadesi döndürür.  
  
 **Örnekler**  
  
 Aşağıdaki örnek, bir nokta ST_VALID kullanarak geçerli olup olmadığını denetlemek gösterilmiştir.  
  
 Örneğin, bu nokta değerleri [-90, 90], geçerli aralıkta şekilde sorgu döndürür false değil bir enlem değer içeriyor.  
  
 Çokgenler için sağlanan son koordinat çifti kapalı bir şekil oluşturmak için ilk olarak, aynı olmalıdır GeoJSON belirtilmesini gerektiriyor. Çokgen içindeki noktaları yönünün sırayla belirtilmelidir. Belirtilen saat yönünde sırayla Çokgen bölge içindeki tersini temsil eder.  
  
```  
SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{ "$1": false }]  
```  
  
####  <a name="bk_st_isvaliddetailed"></a>ST_ISVALIDDETAILED  
 Belirtilen GeoJSON noktası, çokgen veya LineString ifade geçerli olup olmadığını ve geçersiz bir Boole değeri içeren bir JSON değeri değeri döndürür, ayrıca bir dize değeri olarak nedeni.  
  
 **Sözdizimi**  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 **Bağımsız değişkenler**  
  
-   `spatial_expr`  
  
     Tüm geçerli GeoJSON noktası veya Çokgen ifadesidir.  
  
 **Dönüş türleri**  
  
 Belirtilen GeoJSON noktası veya Çokgen ifade geçerli olup olmadığını ve geçersiz bir Boole değeri içeren bir JSON değeri değeri döndürür, ayrıca bir dize değeri olarak nedeni.  
  
 **Örnekler**  
  
 Aşağıdaki örnekte nasıl ST_ISVALIDDETAILED kullanarak geçerlilik (ayrıntılarla gibi) denetlenir.  
  
```  
SELECT ST_ISVALIDDETAILED({   
  "type": "Polygon",   
  "coordinates": [[ [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] ]]  
})  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{  
  "$1": {   
    "valid": false,   
    "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points."   
  }  
}]  
```  
  
## <a name="next-steps"></a>Sonraki adımlar  
 [SQL söz dizimi ve Azure Cosmos DB SQL sorgusu](documentdb-sql-query.md)   
 [Azure Cosmos DB belgeleri](https://docs.microsoft.com/en-us/azure/cosmos-db/)  
  
  
