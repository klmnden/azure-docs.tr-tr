---
title: "SQL Azure Cosmos DB sorgularında | Microsoft Docs"
description: "Azure Cosmos DB SQL söz dizimi, veritabanı kavramlarını ve SQL sorguları hakkında bilgi edinin. SQL Azure Cosmos veritabanı bir JSON sorgu dili olarak kullanabilir."
keywords: "SQL söz dizimi, sql sorgusu, sql sorguları, json sorgu dili, veritabanı kavramlarını ve sql sorguları, toplama işlevleri"
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: a73b4ab3-0786-42fd-b59b-555fce09db6e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: arramac
ms.openlocfilehash: f620e7eac0bd0c9d3e5047b52bcc149aa11c5644
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="sql-queries-for-azure-cosmos-db"></a>Azure Cosmos DB SQL sorguları

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

Microsoft Azure Cosmos DB SQL (yapılandırılmış sorgu dili) kullanarak belgelerin sorgulanmasını SQL API hesapları bir JSON sorgu dili olarak destekler. Azure Cosmos DB gerçekten şemasız. Doğrudan veritabanı altyapısının içinde JSON veri modeli için kendi taahhüt, otomatik JSON belgelerinin dizinini gerektirmeden açık şema veya ikincil dizinlerin oluşturulmasını sağlar.

Cosmos DB için sorgu dili tasarlarken size iki hedefleri düşünerek vardı:

* Yeni bir JSON sorgu dili inventing yerine biz SQL desteği istedik. SQL en tanıdık ve popüler sorgu dilleri biridir. Cosmos veritabanı SQL, JSON belgelerini zengin sorgular için resmi bir programlama modeli sağlar.
* Bir JSON belgesi veritabanı doğrudan veritabanı altyapısının içinde JavaScript yürütme özellikli, biz JavaScript'in programlama modeli bizim sorgu dili için temel olarak kullanmak istedik. SQL API JavaScript'in tür sistemi, ifade değerlendirmesi ve işlev çağrısını kökü belirtilmiş. Bu içinde dönüş JSON belgeleri, kendi kendine birleşimler, uzamsal sorguları ve kullanıcı tanımlı işlevler (UDF'ler) diğer özellikler arasında JavaScript tamamen yazılmış çalıştırılışı arasında ilişkisel projeksiyonları, hiyerarşik gezinti için doğal bir programlama modeli sağlar. 

Bu özellikler uygulama ve veritabanı arasında uyuşmazlık azaltmak için anahtar ve geliştirici üretkenliği için kritik önem taşıyan inanıyoruz.

Burada Aravind Ramachandran gösterir Cosmos DB özellikleri sorgulama, aşağıdaki videoyu izlemeyi ve ziyaret tarafından Başlarken öneririz bizim [Query Playground](http://www.documentdb.com/sql/demo), burada Cosmos DB deneyin ve SQL sorguları çalıştırma bizim veri kümesi.

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/DataExposedQueryingDocumentDB/player]
> 
> 

Ardından, bu makalede, burada size bazı basit JSON belgeleri ve SQL komutları anlatan bir SQL sorgusu öğretici başlayın döndür.

## <a id="GettingStarted"></a>Cosmos DB SQL komutları ile çalışmaya başlama
Cosmos veritabanı SQL iş görmek için şimdi birkaç basit JSON belgeleri ile başlar ve bazı basit sorgular size yol. Bu iki JSON belgeleri iki ailesi hakkında göz önünde bulundurun. Cosmos DB ile biz herhangi bir şemaları ya da ikincil dizinlerin açıkça oluşturmak gerekmez. Biz JSON belgeleri Cosmos DB koleksiyonuna eklemek ve daha sonra sorgu yeterlidir. Basit bir JSON burada sahibiz belge Andersen ailesi, üst, alt öğelerini (ve bunların Evcil Hayvanlar), adresinizi ve kayıt bilgileri. Belge dizeler, sayılar, Boole değerlerini, dizileri ve iç içe özellikler vardır. 

**Belge**  

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

Bir fark – sahip ikinci bir belge işte `givenName` ve `familyName` yerine kullanılan `firstName` ve `lastName`.

**Belge**  

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

Artık Azure Cosmos veritabanı SQL sorgu dili anahtar yönlerini bazıları anlamak için bu verileri birkaç sorguları deneyelim. Örneğin, aşağıdaki sorguyu ID alanı eşleştiği belgeleri döndürür `AndersenFamily`. Olduğundan bir `SELECT *`, tam JSON belgesi sorgunun çıktısı şöyledir:

**Sorgu**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Sonuçları**

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


Şimdi Burada farklı bir şekli JSON çıkışında yeniden biçimlendirmek için ihtiyacımız durumu göz önünde bulundurun. Adres Şehir durumu olarak aynı ada sahip olduğunda bu sorgu adı ve şehir olmak üzere iki seçilen alan ile yeni bir JSON nesnesi projeleri. Bu durumda, "NY, NY" eşleşir.

**Sorgu**    

    SELECT {"Name":f.id, "City":f.address.city} AS Family 
    FROM Families f 
    WHERE f.address.city = f.address.state

**Sonuçları**

    [{
        "Family": {
            "Name": "WakefieldFamily", 
            "City": "NY"
        }
    }]


Sonraki sorgu kimliğine eşleşen ailesinde alt tüm verilen adlarını döndürür `WakefieldFamily` ikamet ettiğiniz şehre göre sıralanmış.

**Sorgu**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC

**Sonuçları**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


Dikkat çekmek için birkaç önemli yönleri kadarki gördük örnekler üzerinden Cosmos DB sorgu dili isteriz:  

* SQL API'si JSON değerler üzerinde çalışır olduğundan, satır ve sütunların yerine varlıklar şeklinde ağaç ile ilgilidir. Bu nedenle, dil, herhangi bir rastgele derinliğe ağacını düğümlerinin gibi bakın sağlar `Node1.Node2.Node3…..Nodem`benzer şekilde iki bölümü referansı başvuran ilişkisel SQL `<table>.<column>`.   
* Yapılandırılmış sorgu dili şema daha az verilerle çalışır. Bu nedenle, tür sistemi dinamik olarak bağlı gerekir. Aynı ifadeye farklı belgelere farklı türlerde üretebilir. Bir sorgunun sonucu, geçerli bir JSON değer, ancak bir sabit şemasına olması garanti edilmez.  
* Cosmos DB yalnızca Katı JSON belgelerini destekler. Bu tür sistemi ve ifadeler yalnızca JSON türleri ile mücadele etmek için sınırlı olduğu anlamına gelir. Başvurmak [JSON belirtimi](http://www.json.org/) daha fazla ayrıntı için.  
* Cosmos DB koleksiyon JSON belgeleri, şemasız bir kapsayıcısıdır. İlişkileri veri varlıklarında içinde ve bir koleksiyondaki belgeler arasında örtük olarak kapsama ve birincil anahtar ve yabancı anahtar ilişkileri tarafından yakalanır. Bu makalenin sonraki bölümlerinde ele alınan içi belge birleştirmeler etkinliğinin düzenleyicileri gösteren değer önemli bir yönü budur.

## <a id="Indexing"></a>Cosmos DB dizin oluşturma
Biz SQL söz dizimi alın önce Azure Cosmos DB dizin tasarımında incelenmesi yararlı vardır. 

Veritabanı dizinlerini amacı, çeşitli formlar ve şekiller sorgularda (örneğin, CPU ve giriş/çıkış) en düşük kaynak kullanımına sahip görev iyi performans ve düşük gecikme süresi sunarken yapmaktır. Genellikle, bir veritabanını sorgulamak için doğru dizin seçimi kadar planlama ve deneme gerektirir. Bu yaklaşım, burada veri katı bir şemaya uygun değil ve hızlı bir şekilde dönüşmesi şema daha az veritabanları için bir zorluk oluşturur. 

Bu nedenle, biz Cosmos DB dizin alt sistemi tasarlarken aşağıdaki hedefleri ayarlar:

* Dizin belgeleri şema gerek kalmadan: dizin oluşturma alt sistemi herhangi bir şema bilgi gerektirmez veya şeması hakkında tüm varsayımlarda belgelerin. 
* Verimli, zengin hiyerarşik ve ilişkisel sorguları için destek: hiyerarşik ve ilişkisel tahminleri desteği dahil olmak üzere verimli bir şekilde dizin Cosmos DB sorgu dili destekler.
* Yazma sürekli bir birim in face of tutarlı sorgular için destek: tutarlı sorgularla yüksek yazma üretilen iş yükleri için dizin artımlı olarak, verimli ve çevrimiçi sürekli bir yazma hacmi karşısında güncelleştirilir. Kullanıcı belge hizmeti yapılandırılan tutarlılık düzeyinde sorguları sunmak tutarlı dizin güncelleştirmesi önemlidir.
* Çoklu kiracı için destek: ayırmaya dayalı modeli için kaynak İdaresi kiracılar arasında verildiğinde, dizin güncelleştirmeleri bütçe çoğaltma ayrılan sistem kaynaklarını (İşlemci, bellek ve saniye başına girdi/çıktı işlemleri) içinde gerçekleştirilir. 
* Depolama verimliliği: maliyet verimliliği için disk üzerinde depolama ek yükü dizin sınırlanmış ve tahmin edilebilir. Cosmos DB maliyet tabanlı bileşim dizin yükü sorgu performansı ile ilgili olarak arasında yapmak Geliştirici sağladığından, bu önemlidir.  

Başvurmak [Azure Cosmos DB örnekleri](https://github.com/Azure/azure-documentdb-net) bir koleksiyon için dizin oluşturma ilkesini yapılandırmak nasıl gösteren örnekler için MSDN'de. Şimdi şimdi Azure Cosmos DB SQL söz dizimi ayrıntıları alın.

## <a id="Basics"></a>Bir Azure Cosmos DB SQL sorgusu temelleri
Her sorgu, bir SELECT yan tümcesi ve isteğe bağlı FROM oluşur ve WHERE yan tümcelerini ANSI SQL standartları başına. Genellikle, her sorgu için kaynak FROM yan tümcesinde numaralandırılır. Ardından WHERE yan tümcesinde filtre bir alt kümesini JSON belgelerini almak için kaynak üzerinde uygulanır. Son olarak, SELECT yan tümcesi, select listesindeki istenen JSON değerlerin proje için kullanılır.

    SELECT <select_list> 
    [FROM <from_specification>] 
    [WHERE <filter_condition>]
    [ORDER BY <sort_specification]    


## <a id="FromClause"></a>FROM yan tümcesi
`FROM <from_specification>` Kaynak filtre veya sorguyu daha sonra öngörülen sürece yan tümcesi isteğe bağlıdır. Bu yan tümce amacı bağlı sorgu çalışmalıdır veri kaynağı belirtmektir. Yaygın olarak tüm koleksiyon kaynağıdır ancak bir koleksiyon kümesini yerine belirtebilirsiniz. 

Bir sorgu ister `SELECT * FROM Families` tüm aileleri koleksiyon üzerinden numaralandırmak kaynak olduğunu gösterir. Özel bir tanımlayıcısı kök, koleksiyon adını kullanmak yerine koleksiyonu temsil etmek için kullanılabilir. Aşağıdaki listede sorgu zorlanan kurallarını içerir:

* Koleksiyon gibi diğer adı, olabilir `SELECT f.id FROM Families AS f` ya da yalnızca `SELECT f.id FROM Families f`. Burada `f` eşdeğerdir `Families`. `AS`diğer isteğe bağlı bir anahtar sözcüğü tanımlayıcısıdır.
* Bir kez diğer adı, özgün kaynak bağlanamaz. Örneğin, `SELECT Families.id FROM Families f` "Aileleri" tanımlayıcısı artık çözümlenemiyor beri sözdizimsel olarak geçersiz.
* Başvurulması gerekiyorsa tüm özellikleri tam olarak nitelenmiş olmalıdır. Kesin Şema bağlılığı olmaması durumunda, bu öğeler belirsiz herhangi bağlamalar önlemek için uygulanır. Bu nedenle, `SELECT id FROM Families f` özelliği bu yana sözdizimsel olarak geçersiz `id` bağlı değil.

### <a name="subdocuments"></a>Alt belgeleri
Kaynak, aynı zamanda daha küçük bir alt azaltılabilir. Örneğin, yalnızca bir alt ağacı her belgedeki numaralandırma için subroot sonra kaynak aşağıdaki örnekte gösterildiği gibi hale gelebilir:

**Sorgu**

    SELECT * 
    FROM Families.children

**Sonuçları**  

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

Yukarıdaki örnek kaynağı olarak bir dizi kullanılan olsa da, bir nesne de aşağıdaki örnekte gösterilen olan kaynağı olarak kullanılabilir: kaynak bulunabilir (tanımsız değil) herhangi geçerli JSON değer sorgunun sonucu eklenmek üzere olarak kabul edilir. Bazı aileleri yoksa bir `address.state` değeri sorgu sonucu hariç tutulur.

**Sorgu**

    SELECT * 
    FROM Families.address.state

**Sonuçları**

    [
      "WA", 
      "NY"
    ]


## <a id="WhereClause"></a>WHERE yan tümcesi
WHERE yan tümcesi (**`WHERE <filter_condition>`**) isteğe bağlıdır. Sonucunu bir parçası olarak dahil edilmesi için JSON belgelerini kaynak tarafından sağlanan koşulları karşılamalıdır belirtir. Herhangi bir JSON belge için sonucu olarak kabul edilmesi için "true" belirtilen koşulları değerlendirmeniz gerekir. WHERE yan tümcesi tarafından dizini katman mutlak en küçük alt sonuç parçası olabilir kaynak belgeleri kümesini belirlemek için kullanılır. 

Aşağıdaki sorgu, değeri olan bir ad özelliği içeren belgeleri istekleri `AndersenFamily`. Name özelliği olmayan herhangi bir belge veya burada değeri eşleşmiyor `AndersenFamily` dışlandı. 

**Sorgu**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Sonuçları**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


Önceki örnekte basit eşitlik sorgu gösterdi. SQL API skaler ifadelerin çeşitli de destekler. En yaygın kullanılan ikili ve birli ifadelerini. Kaynak JSON nesnesi özelliği başvurularından da geçerli ifadelerini. 

Aşağıdaki ikili işleçler şu anda desteklenen ve aşağıdaki örneklerde gösterildiği gibi sorgularında kullanılabilir:  

<table>
<tr>
<td>Aritmetik</td>    
<td>+,-,*,/,%</td>
</tr>
<tr>
<td>Bit düzeyinde</td>    
<td>|, &, ^, <<>>,, >>> (sıfır dolgu sağa kaydırma)</td>
</tr>
<tr>
<td>Mantıksal</td>
<td>VE VEYA DEĞİL</td>
</tr>
<tr>
<td>Karşılaştırma</td>    
<td>=, !=, &lt;, &gt;, &lt;=, &gt;=, <></td>
</tr>
<tr>
<td>Dize</td>    
<td>|| (birleştirme)</td>
</tr>
</table>  


İkili işleçler kullanarak bazı sorguları bir göz atalım.

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5     -- matching grades == 5


Birli işleçleri +,-, ~ değil de desteklenir ve aşağıdaki örnekte gösterildiği gibi sorgular içinde kullanılabilir:

    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1

    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5



İkili ve birli işleçler ek olarak, özellik başvuruları de izin verilir. Örneğin, `SELECT * FROM Families f WHERE f.isRegistered` içeren özellik JSON belgesini döndürür `isRegistered` özelliğin değeri olduğu için JSON eşit `true` değeri. Diğer tüm değerler (false, null, tanımlanmamış, `<number>`, `<string>`, `<object>`, `<array>`, vs.) sonucundan dışlanan kaynak belge için yol açar. 

### <a name="equality-and-comparison-operators"></a>Eşitlik ve Karşılaştırma işleçleri
Aşağıdaki tabloda, her iki JSON türleri arasındaki SQL API'sindeki eşitlik karşılaştırmaları sonucunu gösterir.

<table style = "width:300px">
   <tbody>
      <tr>
         <td valign="top">
            <strong>OP</strong>
         </td>
         <td valign="top">
            <strong>Tanımlanmamış</strong>
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
            <strong>Dize</strong>
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
            <strong>Tanımlanmamış<strong>
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
            <strong>Dize<strong>
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

Gibi diğer Karşılaştırma işleçleri için >, > =,! =, < ve < =, aşağıdaki kurallar geçerlidir:   

* Karşılaştırma türü üzerinden içinde tanımlanmamış sonuçlanır.
* İki nesne veya iki arasında karşılaştırma tanımlanmamış sonuçlarında dizi.   

Skaler ifade filtresi sonucu olup olmadığını tanımlanmamış "true" mantıksal olarak eşitlemek değil bu yana tanımlanmamışsa, ilgili belge sonucunda dahil edilir değil.

### <a name="between-keyword"></a>ARASINDA anahtar sözcüğü
BETWEEN anahtar sözcüğü, ANSI SQL gibi değerler aralıklarına sorguları express için de kullanabilirsiniz. ARASINDA dizeyi veya sayı karşı kullanılabilir.

Örneğin, bu sorgu, ilk alt düzey 1-5 arasında (her ikisi de dahil) olan tüm ailesi belgeleri döndürür. 

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5

Farklı ANSI-SQL'de de BETWEEN yan tümcesi aşağıdaki örnekteki gibi FROM yan tümcesinde kullanabilirsiniz.

    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c

Tüm sayısal özellikleri/BETWEEN yan tümcesinde filtrelenen yolları karşı bir aralık dizin türünü kullanan bir dizin oluşturma ilkesi oluşturmak daha hızlı sorgu yürütme süreleri için unutmayın. 

BETWEEN ANSI SQL ve SQL API'yi kullanarak arasındaki temel fark karma türlerinin özellikleri aralığı sorguları express – Örneğin, "bir sayı (5) düzeyde" olabilir bazı belgelerde ve diğerleri ("grade4") dizelerde ' dir. Bu gibi durumlarda gibi JavaScript'te, iki farklı sonuçlarında "tanımsız" ve belge arasında bir karşılaştırma atlanacak.

### <a name="logical-and-or-and-not-operators"></a>Mantıksal (AND, OR ve NOT) işleçleri
Mantıksal işleçler Boole değerleri üzerinde çalışır. Bu işleçlere için mantıksal gerçekte tabloları aşağıdaki tablolarda gösterilmiştir.

| OR | True | False | Tanımlanmadı |
| --- | --- | --- | --- |
| True |True |True |True |
| False |True |False |Tanımlanmadı |
| Tanımlanmadı |True |Tanımlanmadı |Tanımlanmadı |

| VE | True | False | Tanımlanmadı |
| --- | --- | --- | --- |
| True |True |False |Tanımlanmadı |
| False |False |False |False |
| Tanımlanmadı |Tanımlanmadı |False |Tanımlanmadı |

| DEĞİL |  |
| --- | --- |
| True |False |
| False |True |
| Tanımlanmadı |Tanımlanmadı |

### <a name="in-keyword"></a>Anahtar SÖZCÜĞÜ
IN anahtar sözcüğü, belirtilen bir değeri bir listedeki herhangi bir değer eşleşip eşleşmediğini kontrol etmek için kullanılabilir. Örneğin, bu sorgu kimliği "WakefieldFamily" veya "AndersenFamily" biri olduğu tüm ailesi belgeleri döndürür. 

    SELECT *
    FROM Families 
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')

Bu örnek, durum belirtilen değerlerden herhangi birini olduğu tüm belgeleri döndürür.

    SELECT *
    FROM Families 
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")

### <a name="ternary--and-coalesce--operators"></a>Üçlü (?) ve birleşim (?) işleçleri
Üçlü ve birleşim işleçleri, koşullu ifadeleri, C# ve JavaScript gibi popüler programlama dilleri benzer oluşturmak için kullanılabilir. 

Üçlü (?) işleci kolay bir şekilde yeni JSON özellikleri oluşturulurken çok kullanışlı olabilir. Örneğin, artık, başlangıç/Orta/aşağıda gösterildiği gibi gelişmiş gibi İnsan okunabilir bir form içine sınıfı düzeyleri sınıflandırmak için sorgu yazabilirsiniz.

     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel 
     FROM Families.children[0] c

Like işleci sorgu çağrıları da yerleştirebilirsiniz.

    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high")  AS gradeLevel 
    FROM Families.children[0] c

Olarak diğer sorgu işleçleri ile koşullu ifade başvurulan özelliklerinde herhangi bir belgesinde eksikse veya karşılaştırılan türleri farklıysa, sonra bu belgeleri sorgu sonuçlarında hariç tutulur.

Birleşim (?) işleci verimli bir şekilde bir özellik (paketini olup olmadığını denetlemek için kullanılabilir tanımlanır) belgede. Yarı yapılandırılmış karşı sorgularken bu yararlıdır veya karma türlerinin veri. Örneğin, mevcut değilse bu sorgu "Soyadı" varsa ya da "Soyadı" döndürür.

    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f

### <a id="EscapingReservedKeywords"></a>Tırnak işaretli özelliği erişimcisi
Özellikler tırnak işaretli özelliği işlecini kullanarak da erişebilirsiniz `[]`. Örneğin, `SELECT c.grade` ve `SELECT c["grade"]` eşdeğerdir. Bu sözdizimi, boşluk, özel karakterler içeriyor veya bir SQL anahtar sözcüğü ya da ayrılmış sözcük adıyla aynı paylaşmak için olur bir özellik atlamanız gerekir yararlıdır.

    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"


## <a id="SelectClause"></a>SELECT yan tümcesi
SELECT yan tümcesi (**`SELECT <select_list>`**) zorunludur ve değerleri sorgudan tıpkı ANSI SQL'de alınır belirtir. Kaynak belgeleri üstünde filtre uygulanmış alt burada belirtilen JSON değerleri alınır ve yeni bir JSON nesnesi oluşturulur, projeksiyon aşamasında, sürüklediğinizde geçirilen her giriş için üzerine geçirilir. 

Aşağıdaki örnek, tipik bir seçme sorgusu gösterir. 

**Sorgu**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Sonuçları**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


### <a name="nested-properties"></a>İç içe Özellikler
Aşağıdaki örnekte, biz iki iç içe özellikler yansıtma `f.address.state` ve `f.address.city`.

**Sorgu**

    SELECT f.address.state, f.address.city
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Sonuçları**

    [{
      "state": "WA", 
      "city": "seattle"
    }]


Projeksiyon ayrıca aşağıdaki örnekte gösterildiği gibi JSON ifadeleri destekler:

**Sorgu**

    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Sonuçları**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle", 
        "name": "AndersenFamily"
      }
    }]


Rolü, bakalım `$1` burada. `SELECT` Yan tümcesi bir JSON nesnesi oluşturmak için gereksinim duyduğu ve hiçbir anahtar sağlanan beri örtük bağımsız değişken adları başlayarak kullanırız `$1`. Örneğin, etiketli iki örtük bağımsız değişkenlerini bu sorgunun döndürdüğü `$1` ve `$2`.

**Sorgu**

    SELECT { "state": f.address.state, "city": f.address.city }, 
           { "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Sonuçları**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]


### <a name="aliasing"></a>Diğer ad
Şimdi şimdi yukarıda değerlerin örneği açık yumuşatma ile genişletmek. Diğer ad için kullanılan anahtar sözcük olduğu gibi. İkinci değer olarak yansıtma sırasında gösterildiği gibi isteğe bağlı `NameInfo`. 

Aynı ada sahip iki özellik bir sorgu sahip olmaması durumunda, böylece bunlar tahmini sonucunda disambiguated birini veya her ikisini özelliklerini yeniden adlandırmak için diğer ad kullanılması gerekir.

**Sorgu**

    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo, 
           { "name": f.id } NameInfo
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Sonuçları**

    [{
      "AddressInfo": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]


### <a name="scalar-expressions"></a>Skaler ifade
Özellik başvuruları yanı sıra, SELECT yan tümcesi skaler ifadeler sabitler, aritmetik ifadeler, mantıksal ifadeler, vb. gibi de destekler. Örneğin, basit bir "Hello World" sorgu aşağıdadır.

**Sorgu**

    SELECT "Hello World"

**Sonuçları**

    [{
      "$1": "Hello World"
    }]


Burada, skaler bir ifade kullanır daha karmaşık bir örnek verilmiştir.

**Sorgu**

    SELECT ((2 + 11 % 7)-2)/3    

**Sonuçları**

    [{
      "$1": 1.33333
    }]


Aşağıdaki örnekte, bir Boole değeri bir skaler ifade sonucudur.

**Sorgu**

    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f    

**Sonuçları**

    [
      {
        "AreFromSameCityState": false
      }, 
      {
        "AreFromSameCityState": true
      }
    ]


### <a name="object-and-array-creation"></a>Nesne ve dizi oluşturma
Başka bir anahtar SQL API dizi/nesne oluşturma özelliğidir. Önceki örnekte yeni bir JSON nesnesi oluşturduğumuz unutmayın. Benzer şekilde, biri de diziler aşağıdaki örneklerde gösterildiği gibi oluşturabilirsiniz:

**Sorgu**

    SELECT [f.address.city, f.address.state] AS CityState 
    FROM Families f    

**Sonuçları**  

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

### <a id="ValueKeyword"></a>VALUE anahtar sözcüğü
**Değeri** anahtar sözcüğü JSON değerini döndürmek için bir yol sağlar. Örneğin, aşağıda gösterilen sorgu skaler döndürür `"Hello World"` yerine `{$1: "Hello World"}`.

**Sorgu**

    SELECT VALUE "Hello World"

**Sonuçları**

    [
      "Hello World"
    ]


Aşağıdaki sorgu olmadan JSON değerini döndürür `"address"` sonuçları etiketi.

**Sorgu**

    SELECT VALUE f.address
    FROM Families f    

**Sonuçları**  

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

Aşağıdaki örnekte bu dönüş JSON ilkel değerlerini (yaprak düzeyi JSON ağacının) göstermek için genişletir. 

**Sorgu**

    SELECT VALUE f.address.state
    FROM Families f    

**Sonuçları**

    [
      "WA",
      "NY"
    ]


### <a name="-operator"></a>* İşleci
Özel işleci (*) belgesi olarak projeye desteklenir-değil. Kullanıldığında, yalnızca tahmini alan olmalıdır. While gibi bir sorgu `SELECT * FROM Families f` geçerli olduğu `SELECT VALUE * FROM Families f ` ve `SELECT *, f.id FROM Families f ` geçerli değildir.

**Sorgu**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Sonuçları**

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

### <a id="TopKeyword"></a>TOP işleci
Üst anahtar sözcüğü değerleri sorgudan sayısını sınırlamak için kullanılabilir. ÜST ORDER BY yan tümcesi ile birlikte kullanıldığında, sonuç kümesi ilk N sıralı değer sayısına sınırlıdır; Aksi takdirde, tanımlanmamış bir sırada sonuçları ilk N sayısını döndürür. En iyi uygulama, bir SELECT deyimi içinde ORDER BY yan tümcesi her zaman üst yan tümcesiyle birlikte kullanın. Hangi satır üst tarafından etkilenen beklendiği belirtmek için tek yolu budur. 

**Sorgu**

    SELECT TOP 1 * 
    FROM Families f 

**Sonuçları**

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

ÜST bir değişken değeri parametreli sorgular kullanmayı veya sabit bir değer (yukarıda gösterildiği gibi) ile kullanılabilir. Daha fazla ayrıntı için lütfen aşağıdaki parametreli sorgular bakın.

### <a id="Aggregates"></a>Toplama işlevleri
Toplamalar de gerçekleştirebilirsiniz `SELECT` yan tümcesi. Toplama işlevleri, bir değerleri kümesi üzerinde bir hesaplama gerçekleştirmek ve tek bir değer döndürür. Örneğin, aşağıdaki sorguyu koleksiyonundaki ailesi belge sayısını döndürür.

**Sorgu**

    SELECT COUNT(1) 
    FROM Families f 

**Sonuçları**

    [{
        "$1": 2
    }]

Kullanarak toplama skaler değer döndürebilir `VALUE` anahtar sözcüğü. Örneğin, aşağıdaki sorgu, tek bir sayı değerlerin sayısını döndürür:

**Sorgu**

    SELECT VALUE COUNT(1) 
    FROM Families f 

**Sonuçları**

    [ 2 ]

Filtrelerle birlikte toplamalar de gerçekleştirebilirsiniz. Örneğin, aşağıdaki sorgu Washington eyaleti adresiyle belge sayısını döndürür.

**Sorgu**

    SELECT VALUE COUNT(1) 
    FROM Families f
    WHERE f.address.state = "WA" 

**Sonuçları**

    [ 1 ]

Aşağıdaki tabloda SQL API desteklenen toplama işlevleri listesini gösterir. `SUM`ve `AVG` ise sayısal değer üzerinde gerçekleştirilen `COUNT`, `MIN`, ve `MAX` numaraları, dizeleri, Boole değerlerini ve null değerlere gerçekleştirilebilir. 

| Kullanım | Açıklama |
|-------|-------------|
| SAYISI | İfade öğe sayısını döndürür. |
| TOPLA   | İfadedeki tüm değerlerin toplamını döndürür. |
| EN DÜŞÜK   | İfade en küçük değeri döndürür. |
| EN YÜKSEK   | İfade en büyük değeri döndürür. |
| ORTALAMA   | İfade değerlerin ortalamasını döndürür. |

Toplamalar, bir dizi yineleme sonuçları de gerçekleştirilebilir. Daha fazla bilgi için bkz: [dizi yineleme sorgularda](#Iteration).

> [!NOTE]
> Azure portal'ın sorgu Gezgini kullanırken, toplama sorguları sorgu sayfası kısmen toplanmış sonuçlar döndürebilir unutmayın. SDK'ları tüm sayfalardaki tek bir toplu değer oluşturur. 
> 
> Kod kullanarak toplama sorguları gerçekleştirmek için .NET SDK'sı 1.12.0, .NET Core SDK 1.1.0 veya Java SDK'sı 1.9.5 gerekir veya üstü.    
>

## <a id="OrderByClause"></a>ORDER BY yan tümcesi
Gibi ANSI-SQL'de, isteğe bağlı bir Order By yan tümcesi sorgularken ekleyebilirsiniz. Yan tümcesi sonuçlar alınması gereken sırayı belirtmek için isteğe bağlı ASC/DESC bağımsız değişken ekleyebilirsiniz.

Örneğin, yerleşik Şehir kişinin adını sırasına aileleri alır bir sorgu aşağıdadır.

**Sorgu**

    SELECT f.id, f.address.city
    FROM Families f 
    ORDER BY f.address.city

**Sonuçları**

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

Ve aileleri dönem temsil eden bir sayı olarak depolanan oluşturma tarih sırasına göre alır bir sorgu süre, yani, 1 Ocak 1970'ten içinde bu yana geçen süre saniye burada'nın.

**Sorgu**

    SELECT f.id, f.creationDate
    FROM Families f 
    ORDER BY f.creationDate DESC

**Sonuçları**

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

## <a id="Advanced"></a>Gelişmiş veritabanı kavramlarını ve SQL sorguları

### <a id="Iteration"></a>Yineleme
Yeni bir yapı aracılığıyla eklendi **IN** JSON diziler yineleme için destek sağlamak için SQL API anahtar sözcük. FROM kaynak yineleme için destek sağlar. Aşağıdaki örnek ile başlayalım:

**Sorgu**

    SELECT * 
    FROM Families.children

**Sonuçları**  

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

Şimdi koleksiyondaki çocukların üzerinden yineleme gerçekleştiren başka bir sorgu bakalım. Çıkış dizisi fark unutmayın. Bu örnek böler `children` ve tek bir dizi sonuçları düzleştirir.  

**Sorgu**

    SELECT * 
    FROM c IN Families.children

**Sonuçları**  

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

Bu daha fazla dizi her bir giriş aşağıdaki örnekte gösterildiği gibi filtrelemek için kullanılabilir:

**Sorgu**

    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8

**Sonuçları**  

    [{
      "givenName": "Lisa"
    }]

Toplama dizi yineleme sonuç de gerçekleştirebilirsiniz. Örneğin, aşağıdaki sorgu tüm aileleri arasında alt sayısını sayar.

**Sorgu**

    SELECT COUNT(child) 
    FROM child IN Families.children

**Sonuçları**  

    [
      { 
        "$1": 3
      }
    ]

### <a id="Joins"></a>Birleşimler
İlişkisel bir veritabanında tablolar katılmak için gereken önemlidir. Normalleştirilmiş şemaları tasarlama için mantıksal corollary olur. Bu aykırı SQL API belgeleri şemasız Normalleştirilmemiş veri modeli ile ilgilidir. Bu mantıksal eşdeğerdir "Self Katıl" a.

Dilin desteklediği sözdizimi < from_source1 > JOIN < from_source2 > birleştirme ediyor... < From_sourceN > katılın. Genel olarak, bu bir dizi döndürür **N**- diziler (ile tanımlama grubu **N** değerleri). Her tanımlama grubu tüm koleksiyon diğer adları kendi ilgili ayarlar yineleme tarafından üretilen değerler içeriyor. Diğer bir deyişle, bu bir tam çapraz birleştirme katılan kümeleri ürünüdür.

Aşağıdaki örnekler, JOIN yan tümcesi nasıl çalıştığını gösterir. Aşağıdaki örnekte, her bir belgenin kaynağından vektörel çarpımını itibaren sonucu boştur ve boş boştur.

**Sorgu**

    SELECT f.id
    FROM Families f
    JOIN f.NonExistent

**Sonuçları**  

    [{
    }]


Aşağıdaki örnekte, birleştirme belge arasında köküdür ve `children` subroot. Bu, iki JSON nesnesi arasındaki arası bir üründür. Biz alt öğeleri dizisi için tek bir kök çalışıyorsanız bu yana alt öğeleri olan bir dizi olgu birleştirme etkili değildir. Bu nedenle tam olarak yalnızca bir belge diziye sahip her bir belgenin vektörel çarpımını üretir beri sonucu yalnızca iki sonucu içerir.

**Sorgu**

    SELECT f.id
    FROM Families f
    JOIN f.children

**Sonuçları**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]


Aşağıdaki örnek, daha geleneksel bir birleştirme gösterir:

**Sorgu**

    SELECT f.id
    FROM Families f
    JOIN c IN f.children 

**Sonuçları**

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



Dikkat edilecek ilk şey olan `from_source` , **katılma** yan tümcesi olan yineleyici. Bu nedenle, akış bu durumda aşağıdaki gibidir:  

* Her alt öğesi genişletin **c** dizideki.
* Çapraz ürün belgenin kökü ile geçerli **f** her alt öğesi olan **c** ilk adımda düzleştirilmiş.
* Son olarak, kök nesnesi proje **f** name özelliği bırakır. 

İlk belgeyi (`AndersenFamily`) yalnızca bu belgeye karşılık gelen tek bir nesne sonuç kümesini içerecek şekilde yalnızca bir alt öğe içeriyor. İkinci belge (`WakefieldFamily`) iki alt öğeleri içerir. Bu nedenle, çapraz ürün, böylece bu belgeye karşılık gelen her bir alt için iki nesne sonuçta her bir alt için ayrı bir nesne oluşturur. Bir çapraz ürün beklendiği gibi hem bu belgeleri kök alanları aynıdır.

Gerçek katılma form başlıkları, aksi takdirde projeye zor olan bir şekil içinde çapraz ürünün programıdır. Ayrıca, aşağıdaki örnekte, biz gördüğünüz gibi sağlar tarafından başlıklar genel memnun bir koşul kullanıcının seçtiği bir tanımlama grubu birleşimi filtre uygulayabilirsiniz.

**Sorgu**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets

**Sonuçları**

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



Bu örnek önceki örnekte doğal bir uzantıdır ve çift birleştirme gerçekleştirir. Bu nedenle, çapraz ürün aşağıdaki sözde kodu olarak görüntülenebilir:

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

`AndersenFamily`bir evcil hayvan sahip bir alt sahiptir. Bu nedenle, bir satır çapraz ürün verir (1\*1\*1) bu aile gelen. WakefieldFamily ancak iki alt öğe, ancak yalnızca bir alt "Jesse" Evcil Hayvanlar içeriyor. Jesse iki Evcil Hayvanlar yine de vardır. Bu nedenle çapraz ürün 1 verir\*1\*2 = 2 Bu ailesinden satırlar.

Sonraki örnekte olduğundan bir ek filtre `pet`. Burada Evcil adı "Gölge" değil tüm başlıklar dışlar. Biz diziler dizileri, herhangi bir tanımlama grubu öğelerinin filtre gelen derleme ve öğeleri herhangi bir bileşimini proje olduğuna dikkat edin. 

**Sorgu**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"

**Sonuçları**

    [
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]


## <a id="JavaScriptIntegration"></a>JavaScript tümleştirme
Azure Cosmos DB JavaScript tabanlı uygulama mantığını saklı yordamları ve Tetikleyicileri bakımından koleksiyonlar üzerinde doğrudan yürütmek için bir programlama modeli sağlar. Bu, her ikisi için sağlar:

* Yüksek performanslı işlem CRUD işlemleri ve belgeleri doğrudan veritabanı altyapısının içinde JavaScript çalışma zamanı derin tümleştirmesi sayesinde bir koleksiyondaki sorguları yeteneği. 
* Denetim akışı, değişken kapsamı, atama ve özel durum işleme temelleri veritabanı işlemleri ile tümleştirilmesi doğal bir model. JavaScript tümleştirme için Azure Cosmos DB desteği hakkında daha fazla ayrıntı için lütfen JavaScript sunucu tarafı programlama belgelerine başvurun.

### <a id="UserDefinedFunctions"></a>Kullanıcı tanımlı işlevler (UDF'ler)
Bu makalede önceden tanımlanmış türleri yanı sıra, SQL API'yi kullanıcı tanımlı işlevler (UDF) için destek sağlar. Özellikle, skaler UDF'ler burada geliştiriciler sıfır veya daha çok değişkenlerinde geçirmek ve geri tek bağımsız değişken sonuç desteklenir. Bu değişkenin her biri geçerli JSON değerleri olmak için denetlenir.  

SQL söz dizimi, bu kullanıcı tanımlı işlevler kullanılarak özel uygulama mantığını destekleyecek şekilde genişletilir. UDF'ler SQL API'si ile kayıtlı olması ve sonra bir SQL sorgusu bir parçası olarak başvuru. Aslında, UDF'ler exquisitely sorgular tarafından çağrılan için tasarlanmıştır. Bu seçenek için bir corollary UDF'ler diğer JavaScript türleri (saklı yordamları ve Tetikleyicileri) olan bağlam nesnesi erişiminiz yok. Salt okunur olarak sorguları yürütmek olduğundan, birincil veya ikincil çoğaltmaları çalıştırabilirsiniz. Bu nedenle, UDF'ler, diğer JavaScript türlerinin aksine ikincil çoğaltmalar üzerinde çalışmak üzere tasarlanmıştır.

Aşağıda, özellikle bir belge koleksiyonu altında Cosmos DB veritabanı sırasında bir UDF nasıl kaydedilebilir bir örnektir.

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

Önceki örnekte adı olan bir UDF oluşturur `REGEX_MATCH`. İki JSON dizesi değerlerini kabul `input` ve `pattern` ve ilk eşleşme deseni ikinci belirtilmişse denetimleri kullanarak JavaScript'in string.match() işlevi.

Bir yansıtma sorgu Biz bu UDF artık kullanabilirsiniz. UDF'ler büyük küçük harfe duyarlı önekiyle "udf." nitelenmiş olmalıdır gelen sorgulara çağrıldığında. 

> [!NOTE]
> 3/17/2015 öncesinde Cosmos DB "udf." olmadan UDF çağrı desteklenir. önek seçin REGEX_MATCH() ister. Bu arama deseni kullanım dışı bırakıldı.  
> 
> 

**Sorgu**

    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families

**Sonuçları**

    [
      {
        "$1": true
      }, 
      {
        "$1": false
      }
    ]

UDF de bir filtre içinde de "udf ile." tam aşağıdaki örnekte gösterildiği gibi kullanılabilir öneki:

**Sorgu**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")

**Sonuçları**

    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]


Esas olarak, UDF'ler geçerli skaler ifadelerini ve tahminleri ve filtreleri kullanılabilir. 

UDF'ler gücüyle genişletmek için başka bir örneğe koşullu mantığı ile bakalım:

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


UDF uygular örneği aşağıdadır.

**Sorgu**

    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f    

**Sonuçları**

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


Önceki örneklerde sergiler gibi UDF'ler JavaScript dil gücünü yerleşik JavaScript çalışma zamanı yeteneklerini yardımıyla karmaşık bir yordam, koşullu mantık yapmak için zengin bir programlanabilir arabirimi sağlamak için SQL API ile tümleştirin.

SQL API bağımsız değişkenler için UDF'ler kaynağındaki her belge için UDF işleme geçerli aşamada (WHERE yan tümcesi veya SELECT yan tümcesi) sağlar. Sonuç genel yürütme ardışık düzeninde sorunsuz olarak eklenmiştir. Özellikler başvurulan tarafından UDF parametreleri JSON değeri kullanılabilir değil, parametre olarak kabul tanımlanmamış ve bu nedenle UDF çağırma tamamen atlandı. Benzer şekilde UDF sonucu tanımsız ise, sonuçta bulunmaz. 

Özet olarak, UDF'ler karmaşık iş mantığı sorgu bir parçası olarak yapmak için harika araçlardır.

### <a name="operator-evaluation"></a>İşleç değerlendirme
Cosmos DB, bir JSON veritabanı olan virtue tarafından JavaScript işleçleri ve değerlendirme semantiği ile parallels çizer. JSON desteği bakımından JavaScript sematiğini korumak Cosmos DB çalışır, ancak bazı durumlarda işlemi değerlendirme farklıdır.

Değerleri veritabanından alınana kadar SQL API'yi aksine geleneksel SQL değerlerin türleri bilinmez genellikle. Verimli bir şekilde sorguları yürütmek için işleçleri çoğunu sıkı tür gereksinimleri vardır. 

SQL API JavaScript aksine örtük dönüşümler gerçekleştirmez. Örneğin, bir sorgu ister `SELECT * FROM Person p WHERE p.Age = 21` eşleşen değeri olan 21 yaş özelliği içeren belgeleri. "021", "21.0", "0021", "00021" dize "21", veya diğer büyük olasılıkla sonsuz Çeşitlemeler, yaş özelliği eşleşen herhangi bir belge ister, vb. eşlenemiyor. Bunun aksine dize değerleri nerede sayılara örtük olarak Integer JavaScript sağlamaktır (örneğin, operatöre dayanan: ==). Bu seçenek, SQL API'yi eşleşen verimli dizin için önemlidir. 

## <a name="parameterized-sql-queries"></a>Parametreli SQL sorguları
Cosmos DB bilinen gösterimi @ ile ifade parametrelerle sorguları destekler. Parametreli SQL sağlam işleme ve kullanıcı girişi, SQL ekleme üzerinden veri yanlışlıkla açığa çıkmaya önleme, kaçış sağlar. 

Örneğin, Soyadı ve adres durumu kullandığı parametreler bir sorgu yazın ve son adı ve kullanıcı girişini temel alarak adresi durumunun çeşitli değerlerin yürütün.

    SELECT * 
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState

Bu istek sonra Cosmos DB gibi parametreli bir JSON sorgu olarak aşağıda gösterilen gönderilebilir.

    {      
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",     
        "parameters": [          
            {"name": "@lastName", "value": "Wakefield"},         
            {"name": "@addressState", "value": "NY"},           
        ] 
    }

İLK bağımsız değişkeni gibi parametreli sorgular kullanmayı aşağıda gösterilen ayarlanabilir.

    {      
        "query": "SELECT TOP @n * FROM Families",     
        "parameters": [          
            {"name": "@n", "value": 10},         
        ] 
    }

Parametre değerleri geçerli bir JSON olabilir (dizeler, sayılar, Boole değerlerini, null, hatta dizileri veya JSON iç içe geçmiş). Ayrıca parametreleri Cosmos DB Şeması daha az olduğundan, karşı herhangi bir tür doğrulanmaz.

## <a id="BuiltinFunctions"></a>Yerleşik işlevler
Cosmos DB yerleşik işlevleri gibi kullanıcı tanımlı işlevler (UDF'ler) sorguları içinde kullanılan ortak işlemleri için de destekler.

| İşlev grubu          | İşlemler                                                                                                                                          |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| Matematik işlevleri  | ABS TAVAN, EXP, FLOOR, günlük, LOG10, güç, HEPSİNİ, oturum, SQRT, KARE, TRUNC, ACOS, ASIN, ATAN, ATN2, COS, COT, derece, PI, radyan, SIN ve TAN |
| Denetimi işlevleri yazın | IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED ve IS_PRIMITIVE                                                           |
| Dize işlevleri        | CONCAT, içerir, ENDSWITH, INDEX_OF, sol, uzunluk, alt, LTRIM, Değiştir, çoğaltılması, geriye doğru sağ, RTRIM, STARTSWITH, SUBSTRING ve üst       |
| Dizi işlevleri         | ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH ve ARRAY_SLICE                                                                                         |
| Uzamsal işlevleri       | St_dıstance, ST_WITHIN, ST_INTERSECTS, ST_ISVALID ve ST_ISVALIDDETAILED                                                                           | 

Şu anda yerleşik işlevi olduğu şimdi kullanılabilir bir kullanıcı tanımlı işlev (UDF) kullanıyorsanız, bunu çalıştırmak daha hızlı olacak şekilde karşılık gelen yerleşik işlevi kullanmalıdır ve daha verimli bir şekilde. 

### <a name="mathematical-functions"></a>Matematik işlevleri
Matematik işlevleri her bağımsız değişken olarak sağlanır ve sayısal bir değeri döndürme giriş değerlerine göre bir hesaplama gerçekleştirir. Burada, desteklenen yerleşik matematik işlevleri tablosu verilmiştir.


| Kullanım | Açıklama |
|----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [[ABS (num_expr)](#bk_abs) | Belirtilen sayısal ifade (pozitif) mutlak değerini döndürür. |
| [Üst SINIRA (num_expr)](#bk_ceiling) | Büyüktür veya eşittir, belirtilen sayısal ifadenin en küçük tamsayı değeri döndürür. |
| [FLOOR (num_expr)](#bk_floor) | Belirtilen sayısal ifade küçük veya eşit en büyük tamsayıyı döndürür. |
| [EXP (num_expr)](#bk_exp) | Belirtilen sayısal ifade üs döndürür. |
| [Günlük (num_expr [, temel])](#bk_log) | Belirtilen sayısal ifade ya da belirtilen taban kullanarak logaritmasını doğal logaritmasını döndürür |
| [Log10 (num_expr)](#bk_log10) | Belirtilen sayısal ifade 10 tabanında Logaritmik değerini döndürür. |
| [ROUND (num_expr)](#bk_round) | En yakın tamsayı değerine yuvarlanan sayısal bir değer döndürür. |
| [TRUNC (num_expr)](#bk_trunc) | En yakın tamsayı değerine kesilmiş sayısal bir değer döndürür. |
| [SQRT (num_expr)](#bk_sqrt) | Belirtilen sayısal ifade kare kökünü döndürür. |
| [KARE (num_expr)](#bk_square) | Belirtilen sayısal ifade kare döndürür. |
| [GÜÇ (num_expr, num_expr)](#bk_power) | Belirtilen sayısal ifade gücünü belirtilen değeri döndürür. |
| [OTURUM (num_expr)](#bk_sign) | İşareti (-1, 0, 1) belirtilen sayısal ifadenin değerini döndürür. |
| [ACOS (num_expr)](#bk_acos) | Açının kosinüsü belirtilen sayısal ifadesidir radyan cinsinden döndürür; arccosine olarak da bilinir. |
| [ASIN (num_expr)](#bk_asin) | Açının sinüsü belirtilen sayısal ifadesidir radyan cinsinden döndürür. Bu arksinüsünü olarak da adlandırılır. |
| [ATAN (num_expr)](#bk_atan) | Açının tanjantı belirtilen sayısal ifadesidir radyan cinsinden döndürür. Bu arktanjantını olarak da adlandırılır. |
| [ATN2 (num_expr)](#bk_atn2) | Burada açıyı pozitif x ekseni ve noktasına (y, x), kaynaktan ray arasında radyan cinsinden döndürür x ve y iki belirtilen float ifadeleri değerlerdir. |
| [COS (num_expr)](#bk_cos) | Radyan cinsinden belirtilen ifade trigonometrik belirtilen açının kosinüsünü döndürür. |
| [COT (num_expr)](#bk_cot) | Belirtilen açının trigonometrik kotanjantını radyan cinsinden belirtilen sayısal ifade döndürür. |
| [DERECE (num_expr)](#bk_degrees) | Radyan cinsinden Açı derece cinsinden karşılık gelen açıyı döndürür. |
| [PI)](#bk_pi) | PI sayısının sabit değeri döndürür. |
| [Radyan CİNSİNDEN (num_expr)](#bk_radians) | Derece sayısal bir ifadenin girildiğinde radyan cinsinden döndürür. |
| [SIN (num_expr)](#bk_sin) | Radyan cinsinden belirtilen ifade trigonometrik belirtilen açının sinüsünü döndürür. |
| [TAN (num_expr)](#bk_tan) | Belirtilen ifade giriş ifadesi tanjantını döndürür. |

Örneğin, şimdi aşağıdaki gibi sorguları çalıştırabilirsiniz:

**Sorgu**

    SELECT VALUE ABS(-4)

**Sonuçları**

    [4]

ANSI SQL karşılaştırıldığında Cosmos veritabanı işlevleri arasındaki temel fark, bunlar da şema küçüktür ve karma şema verilerle çalışmak üzere tasarlanmıştır ' dir. Örneğin, burada Size özelliği eksik veya sahip bir belgeniz varsa "Bilinmeyen" gibi bir sayısal olmayan değer sonra belge üzerinde bir hata döndürüyor yerine atlanır.

### <a name="type-checking-functions"></a>Denetimi işlevleri yazın
Tür denetleme işlevleri SQL sorguları içinde bir ifade türünü kontrol olanak sağlar. Tür denetleme işlevleri, değişken veya bilinmeyen olduğunda kolay bir şekilde belgelerde özellikleri türünü belirlemek için kullanılabilir. Burada, desteklenen yerleşik tür işlevleri denetlemesini tablosu verilmiştir.

<table>
<tr>
  <td><strong>Kullanım</strong></td>
  <td><strong>Açıklama</strong></td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (ifade)</a></td>
  <td>Değerin türü bir dizi olup olmadığını gösteren bir Boole değeri döndürür.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (ifade)</a></td>
  <td>Türde bir değer bir Boole değeri olup olmadığını gösteren bir Boole değeri döndürür.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (ifade)</a></td>
  <td>Değerin türü null olup olmadığını gösteren bir Boole değeri döndürür.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (ifade)</a></td>
  <td>Türde bir değer bir sayı olup olmadığını gösteren bir Boole değeri döndürür.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (ifade)</a></td>
  <td>Değerin türü bir JSON nesnesi olup olmadığını gösteren bir Boole değeri döndürür.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (ifade)</a></td>
  <td>Değerin türü bir dize olup olmadığını gösteren bir Boole değeri döndürür.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (ifade)</a></td>
  <td>Özellik değeri atanmış olan gösteren bir Boole değeri döndürür.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (ifade)</a></td>
  <td>Değerin türü bir dize, sayı, Boole veya null olup olmadığını gösteren bir Boole değeri döndürür.</td>
</tr>

</table>

Bu işlevler kullanılarak, şimdi aşağıdaki gibi sorguları çalıştırabilirsiniz:

**Sorgu**

    SELECT VALUE IS_NUMBER(-4)

**Sonuçları**

    [true]

### <a name="string-functions"></a>Dize işlevleri
Aşağıdaki skaler işlevler dize giriş değeri üzerinde bir işlemi gerçekleştirmek ve bir dize, sayısal ya da Boole değeri döndürür. Yerleşik dize işlevleri tablosu aşağıdadır:

| Kullanım | Açıklama |
| --- | --- |
| [UZUNLUĞU (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_length) |Belirtilen dize ifadesinin karakterlerin sayısını döndürür |
| [CONCAT (str_expr, str_expr [, str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat) |İki veya daha fazla dize değerlerini birleştirme sonucu olan bir dize döndürür. |
| [SUBSTRING (str_expr, num_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_substring) |Bir dize ifadesi bölümünü döndürür. |
| [STARTSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_startswith) |Döndürür Boolean belirten bir ilk ifade dize olup olmadığını ve ikinci sona erer |
| [ENDSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_endswith) |Döndürür Boolean belirten bir ilk ifade dize olup olmadığını ve ikinci sona erer |
| [İÇERİR (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_contains) |Döndüren bir Boolean belirten ikinci ilk ifade dize olup olmadığını içerir. |
| [INDEX_OF (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_index_of) |İkinci ilk örneğinin başlangıç konumunu döndürür dizesi ifade ilk belirtilen dize ifadesi veya -1 içinde dizesi bulunamadı. |
| [Sol (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_left) |Sol bölümü belirtilen sayıda karakteri içeren bir dize döndürür. |
| [SAĞ (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_right) |Belirtilen sayıda karakteri içeren bir dize sağ bölümünü döndürür. |
| [LTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ltrim) |Öndeki boşlukları kaldırır sonra bir dize ifadesi döndürür. |
| [RTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_rtrim) |Tüm sondaki boşlukları kesilmesi sonrasında bir dize ifadesi döndürür. |
| [Alt (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_lower) |Büyük harf karakter verileri küçük harfe dönüştürmek sonra bir dize ifadesi döndürür. |
| [ÜST (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_upper) |Küçük harf karakter verileri büyük harfe dönüştürme sonra bir dize ifadesi döndürür. |
| [REPLACE (str_expr, str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replace) |Belirtilen dize değeri tüm oluşumlarını başka bir dize değeri ile değiştirir. |
| [REPLICATE (str_expr, num_expr)](https://docs.microsoft.com/azure/cosmos-db/documentdb-sql-query-reference#bk_replicate) |Bir dize değeri, belirtilen sayıda yineler. |
| [Ters (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_reverse) |Ters sırada bir dize değerini döndürür. |

Bu işlevleri kullanarak, şimdi aşağıdaki gibi sorguları çalıştırabilirsiniz. Örneğin, aile adı büyük şu şekilde döndürebilirsiniz:

**Sorgu**

    SELECT VALUE UPPER(Families.id)
    FROM Families

**Sonuçları**

    [
        "WAKEFIELDFAMILY", 
        "ANDERSENFAMILY"
    ]

Veya bu örnekteki gibi dizeyi birleştirmek:

**Sorgu**

    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families

**Sonuçları**

    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "seattle,WA"
    }]


Dize işlevleri, WHERE yan tümcesinde aşağıdaki örnekte gibi sonuçları filtrelemek için de kullanılabilir:

**Sorgu**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")

**Sonuçları**

    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]

### <a name="array-functions"></a>Dizi işlevleri
Aşağıdaki skaler işlevler bir dizi giriş değeri ve return sayısal, Boole veya dizi değer üzerinde bir işlemi gerçekleştirir. Yerleşik dizi işlevleri tablosu aşağıdadır:

| Kullanım | Açıklama |
| --- | --- |
| [ARRAY_LENGTH (arr_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_length) |Belirtilen dizi ifadesi öğe sayısını döndürür. |
| [ARRAY_CONCAT (arr_expr, arr_expr [, arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat) |İki veya daha fazla dizi değerlerini birleştirme sonucu olan bir dizi döndürür. |
| [ARRAY_CONTAINS (arr_expr, ifade [, bool_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains) |Dizi belirtilen değeri içerip içermediğini gösteren bir Boole değeri döndürür. Tam veya kısmi eşleşme olup olmadığını belirtebilirsiniz. |
| [ARRAY_SLICE (arr_expr, num_expr [, num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice) |Bir dizi ifadesi bölümünü döndürür. |

Dizi işlevleri, JSON içinde diziler işlemek için kullanılabilir. Örneğin, bir üst "Deneme Wakefield" olduğu tüm belgeleri döndüren bir sorgu aşağıdadır. 

**Sorgu**

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })

**Sonuçları**

    [{
      "id": "WakefieldFamily"
    }]

Dizi öğeleri eşleşen bir kısmi parça belirtebilirsiniz. Aşağıdaki sorgu ile tüm üst bulur `givenName` , `Robin`.

**Sorgu**

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin" }, true)

**Sonuçları**

    [{
      "id": "WakefieldFamily"
    }]


Burada, aile başına alt sayısını almak için ARRAY_LENGTH kullanan başka bir örnek verilmiştir.

**Sorgu**

    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 

**Sonuçları**

    [{
      "id": "WakefieldFamily",
      "numberOfChildren": 2
    },
    {
      "id": "AndersenFamily",
      "numberOfChildren": 1
    }]

### <a name="spatial-functions"></a>Uzamsal işlevleri
Cosmos DB Jeo-uzamsal sorgulamak için aşağıdaki açık Jeo-uzamsal Konsorsiyumu (OGC) yerleşik işlevleri destekler. 

<table>
<tr>
  <td><strong>Kullanım</strong></td>
  <td><strong>Açıklama</strong></td>
</tr>
<tr>
  <td>St_dıstance (point_expr, point_expr)</td>
  <td>İki GeoJSON noktası, çokgen veya LineString ifadeleri uzaklığı döndürür.</td>
</tr>
<tr>
  <td>ST_WITHIN (point_expr, polygon_expr)</td>
  <td>İlk GeoJSON nesne (noktası, çokgen veya LineString) ikinci GeoJSON nesne içinde (noktası, çokgen veya LineString) olup olmadığını gösteren bir Boole ifadesi döndürür.</td>
</tr>
<tr>
  <td>ST_INTERSECTS (spatial_expr, spatial_expr)</td>
  <td>İki belirtilen GeoJSON nesne (noktası, çokgen veya LineString) kesiştiği olup olmadığını gösteren bir Boole ifadesi döndürür.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Belirtilen GeoJSON noktası, çokgen veya LineString ifade geçerli olup olmadığını gösteren bir Boole değeri döndürür.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Belirtilen GeoJSON noktası, çokgen veya LineString ifade geçerli olup olmadığını ve geçersiz bir Boole değeri içeren bir JSON değeri değeri döndürür, ayrıca bir dize değeri olarak nedeni.</td>
</tr>
</table>

Uzamsal işlevleri uzamsal veriler yakınlaştırmalı sorguları gerçekleştirmek için kullanılabilir. Örneğin, içinde 30 km st_dıstance yerleşik işlevini kullanarak belirtilen konumun olan tüm ailesi belgeleri döndüren bir sorgu aşağıdadır. 

**Sorgu**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Sonuçları**

    [{
      "id": "WakefieldFamily"
    }]

Cosmos DB Jeo-uzamsal desteği hakkında daha fazla ayrıntı için lütfen bkz. [Azure Cosmos veritabanı Jeo-uzamsal verilerle çalışma](geospatial.md). Cosmos DB için uzamsal işlevleri ve SQL söz dizimi, sarmalar. Şimdi nasıl çalışır ve sözdizimi ile nasıl etkileşim kurduğu sorgulama LINQ kadarki gördük bir bakalım.

## <a id="Linq"></a>LINQ-SQL API
LINQ, hesaplama nesnelerin akışları sorgular olarak ifade eder ve .NET çalışan bir programlama modelidir. Cosmos DB LINQ ile arabirim oluşturmak için bir istemci-tarafı kitaplığı, JSON ve .NET nesneleri ve bir alt kümesinden LINQ sorgularını eşleme Cosmos DB sorgulara arasında dönüştürme kolaylaştırarak sağlar. 

Aşağıdaki resimde Cosmos DB kullanarak LINQ sorgularını destekleme mimarisi gösterilmektedir.  Cosmos DB İstemcisi'ni kullanarak geliştiriciler oluşturabilir bir **Iqueryable** doğrudan sonra Cosmos DB sorguda LINQ sorgusu çevirir Cosmos DB sorgu sağlayıcısı sorgular nesnesi. Sorgu daha sonra bir JSON biçiminde sonuç kümesi almak için Cosmos DB sunucuya geçirilir. Döndürülen sonuçların, istemci tarafında .NET nesnelerin bir akışa serisi.

![SQL API'yi - kullanarak LINQ sorgularını SQL söz dizimi, JSON sorgu dili, veritabanı kavramlarını ve SQL sorguları destekleyen mimarisi][1]

### <a name="net-and-json-mapping"></a>.NET ve JSON eşleme
.NET nesneleri ve JSON belgeleri arasında eşleme doğal - her bir veri üyesi alan burada alan adı nesne "anahtarı" parçası eşlendi ve "value" bölümü nesne değer bölümünü eşlenen yinelemeli bir JSON nesnesi ile eşleştirilir. Aşağıdaki örneği göz önünde bulundurun: oluşturulan aile nesnesi, JSON belgesi eşlendiği, aşağıda gösterildiği gibi. Ve tersi, JSON belgesini bir .NET nesnesine eşlendi.

**C# sınıfı**

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


**JSON**  

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



### <a name="linq-to-sql-translation"></a>LINQ-SQL çevirisi
Cosmos DB sorgu Sağlayıcısı'nı bir en iyi çaba eşleme Cosmos DB SQL sorgusu bir LINQ Sorgu gerçekleştirir. Aşağıdaki açıklamasında okuyucu temel benzerlik LINQ sahip varsayalım.

İlk olarak, tür sistemi için tüm JSON ilkel türler – sayısal türler, boolean, dize ve null destekliyoruz. Yalnızca bu JSON türleri desteklenir. Aşağıdaki skaler ifadelerin desteklenir.

* Sabit değerler – bunlar sorgu değerlendirilir zaman temel veri türlerinin sabit değerleri içerir.
* Özellik/dizi dizini ifadeleri – bu ifadeler bir nesneyi veya bir dizi öğesine özelliğine bakın.
  
     ailesi. Kimliği;    Family.Children[0].familyName;    Family.Children[0].grade;    Family.Children[n].grade; n bir int değişkenidir
* Aritmetik ifadeler - bunlar ortak aritmetik ifadeler sayısal ve Boole değerleri içerir. Tam listesi için SQL belirtimine bakın.
  
     2 * family.children[0].grade;    x + y;
* Dize karşılaştırma ifadesi - bunlar bazı sabit dize değeri bir dize değeri karşılaştırma içerir.  
  
     mother.familyName == "Smith";    child.givenName s; == bir dize değişkeni s'dir
* Nesne/oluşturma ifadesi - dönüş Bu deyimler bileşik değer türü veya anonim tür bir nesneyi veya bir dizi tür nesneler dizisi. Bu değerleri iç içe.
  
     Yeni üst {familyName givenName "Smith" = "Can" =}; Yeni {ilk = 1, ikincisi 2 =}; iki alan sahip anonim bir tür              
     Yeni int [] {3, child.grade, 5};

### <a id="SupportedLinqOperators"></a>Desteklenen LINQ işleçlerin listesi
SQL .NET SDK'sı ile dahil LINQ Sağlayıcısı'nda desteklenen LINQ işleçlerin bir listesi aşağıda verilmiştir.

* **Seçin**: tahminleri Çevir SQL nesne oluşturması dahil olmak üzere SEÇMEK için
* **Burada**: filtreleri SQL WHERE için çevirin ve Destek arasında çeviri & &, || ve! SQL işleçleri
* **SelectMany**: SQL JOIN yan tümcesine dizi geriye doğru izleme sağlar. Dizi öğeleri filtrelemek için ifadeleri zinciri/iç içe geçirme için kullanılabilir
* **OrderBy ve OrderByDescending**: ORDER BY artan/azalan şekilde çevirir
* **Count**, **toplam**, **Min**, **Max**, ve **ortalama** toplama ve zaman uyumsuz eşdeğerlerine işleçleri **CountAsync**, **SumAsync**, **MinAsync**, **MaxAsync**, ve **AverageAsync**.
* **CompareTo**: aralık karşılaştırmaları çevirir. .NET ile karşılaştırılabilir değilseniz bu yana dizeleri için yaygın olarak kullanılan
* **Ele**: bir sorgunun sonuçlarına sınırlama SQL üstüne çevirir
* **Matematik işlevleri**: çevrilmesi destekler. NET'in Abs, Acos, Asin Cos tavan Atan, Exp, Floor, günlük, Log10, Pow, hepsini, oturum, Sin, Sqrt, Bronz, eşdeğer SQL yerleşik işlevler Truncate.
* **Dize işlevleri**: çevrilmesi destekler. NET'in Concat, içerir, EndsWith, IndexOf, sayısı, ToLower, TrimStart, Değiştir, geriye doğru TrimEnd, StartsWith, SubString, eşdeğer SQL yerleşik işlevlere ToUpper.
* **Dizi işlevleri**: çevrilmesi destekler. NET'in Concat içerir ve eşdeğer SQL yerleşik işlevlere sayısı.
* **Jeo-uzamsal uzantı işlevleri**: saplama yöntemleri IsValid ve IsValidDetailed içinde uzaklığı çevrilecek eşdeğer SQL yerleşik işlevleri destekler.
* **Kullanıcı tanımlı işlev uzantı işlevi**: UserDefinedFunctionProvider.Invoke saplama yönteminden çeviri karşılık gelen kullanıcı tanımlı işlev için destekler.
* **Çeşitli**: koşullu işleçler ve birleşim çevrilmesi destekler. Dize İÇERİYOR, ARRAY_CONTAINS veya bağlam bağlı olarak SQL IN içerir çevirebilir.

### <a name="sql-query-operators"></a>SQL sorgu işleçleri
Aşağıda, bazı standart LINQ Sorgu işleçleri Cosmos DB sorguları aşağıya doğru nasıl dönüştürüleceğini gösteren bazı örnekler verilmiştir.

#### <a name="select-operator"></a>İşleç Seç
Sözdizimi `input.Select(x => f(x))`, burada `f` skaler bir ifade değil.

**LINQ lambda ifadesi**

    input.Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f



**LINQ lambda ifadesi**

    input.Select(family => family.children[0].grade + c); // c is an int variable


**SQL** 

    SELECT VALUE f.children[0].grade + c
    FROM Families f 



**LINQ lambda ifadesi**

    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });


**SQL** 

    SELECT VALUE {"name":f.children[0].familyName, 
                  "grade": f.children[0].grade + 3 }
    FROM Families f



#### <a name="selectmany-operator"></a>SelectMany işleci
Sözdizimi `input.SelectMany(x => f(x))`, burada `f` koleksiyon türü döndüren bir skaler ifade.

**LINQ lambda ifadesi**

    input.SelectMany(family => family.children);

**SQL** 

    SELECT VALUE child
    FROM child IN Families.children



#### <a name="where-operator"></a>Burada işleci
Sözdizimi `input.Where(x => f(x))`, burada `f` bir Boole değeri döndürür skaler bir ifade değil.

**LINQ lambda ifadesi**

    input.Where(family=> family.parents[0].familyName == "Smith");

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith" 



**LINQ lambda ifadesi**

    input.Where(
        family => family.parents[0].familyName == "Smith" && 
        family.children[0].grade < 3);

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
    AND f.children[0].grade < 3


### <a name="composite-sql-queries"></a>Bileşik SQL sorguları
Daha güçlü sorgular oluşturmak için yukarıdaki işleçleri oluşturulabilir. Cosmos DB iç içe geçmiş koleksiyonları desteklediğinden, birleşim birleştirilmiş iç içe geçmiş ya da.

#### <a name="concatenation"></a>Birleştirme
Sözdizimi `input(.|.SelectMany())(.Select()|.Where())*`. Birleştirilmiş bir sorgu ile isteğe bağlı başlatabilirsiniz `SelectMany` sorgu birden fazla ardından `Select` veya `Where` işleçler.

**LINQ lambda ifadesi**

    input.Select(family=>family.parents[0])
        .Where(familyName == "Smith");

**SQL**

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"



**LINQ lambda ifadesi**

    input.Where(family => family.children[0].grade > 3)
        .Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f
    WHERE f.children[0].grade > 3



**LINQ lambda ifadesi**

    input.Select(family => new { grade=family.children[0].grade}).
        Where(anon=> anon.grade < 3);

**SQL** 

    SELECT *
    FROM Families f
    WHERE ({grade: f.children[0].grade}.grade > 3)



**LINQ lambda ifadesi**

    input.SelectMany(family => family.parents)
        .Where(parent => parents.familyName == "Smith");

**SQL** 

    SELECT *
    FROM p IN Families.parents
    WHERE p.familyName = "Smith"



#### <a name="nesting"></a>İç içe geçme
Sözdizimi `input.SelectMany(x=>x.Q())` Q olduğu bir `Select`, `SelectMany`, veya `Where` işleci.

İç içe bir sorgu iç sorgu dış toplama her öğesine uygulanır. İç sorgu gibi dış koleksiyondaki öğelerin alanlara başvurabilir önemli özelliklerinden biri otomatik olarak birleştirir.

**LINQ lambda ifadesi**

    input.SelectMany(family=> 
        family.parents.Select(p => p.familyName));

**SQL** 

    SELECT VALUE p.familyName
    FROM Families f
    JOIN p IN f.parents


**LINQ lambda ifadesi**

    input.SelectMany(family => 
        family.children.Where(child => child.familyName == "Jeff"));

**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = "Jeff"



**LINQ lambda ifadesi**

    input.SelectMany(family => family.children.Where(
        child => child.familyName == family.parents[0].familyName));

**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = f.parents[0].familyName


## <a id="ExecutingSqlQueries"></a>SQL sorguları yürütme
Cosmos DB herhangi bir dil tarafından HTTP/HTTPS istekleri çağrılabilir bir REST API'si aracılığıyla kaynaklarını kullanıma sunar. Ayrıca, Cosmos DB .NET, Node.js, JavaScript ve Python gibi çeşitli popüler dilde programlama kitaplıkları sunar. Tüm REST API ve çeşitli kitaplıkları SQL sorgulama destekler. .NET SDK'sı SQL yanı sıra sorgulama LINQ destekler.

Aşağıdaki örnekler, bir sorgu oluşturun ve Cosmos DB veritabanı hesabı karşı göndermek üzere gösterilmektedir.

### <a id="RestAPI"></a>REST API'Sİ
Cosmos DB HTTP üzerinden açık bir RESTful programlama modeli sunar. Bir Azure aboneliği kullanarak veritabanı hesaplarını sağlanabilir. Cosmos DB kaynak modeli kaynaklar her biri mantıksal ve kararlı bir URI kullanılarak adreslenebilir bir veritabanı hesabı altında bir dizi oluşur. Bir kaynak kümesi için bu belgede bir akış olarak adlandırılır. Veritabanı hesabı her birden çok koleksiyonu kapsayan bir veritabanları kümesi oluşur belgeleri, UDF'ler ve diğer kaynak türlerini her hangi içinde dönüş içerebilir.

Temel etkileşim bu kaynaklarla HTTP fiilleri GET, PUT, POST ve silme ile standart kullanıcıların yorumu modelidir. POST fiil yeni bir kaynak oluşturulmasını, bir saklı yordamı yürütmek veya Cosmos DB sorgu verme için kullanılır. Sorguları her zaman salt okunur hiçbir yan etkileri olan işlemleridir.

Aşağıdaki örnekler, biz kadarki geçirdikten iki örnek belgeleri içeren bir koleksiyon karşı yapılan bir SQL API sorgu için bir POST gösterir. Sorgu basit bir filtre JSON name özelliğine göre sahiptir. Kullanımına dikkat edin `x-ms-documentdb-isquery` ve Content-Type: `application/query+json` işlemi bir sorgu olduğunu belirtmek için üstbilgiler.

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


**Sonuçları**

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


İkinci örnek birleştirme sonucu birden çok sonuç döndüren daha karmaşık bir sorgu gösterir.

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


**Sonuçları**

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


Bir sorgunun sonuçları sonuçlar tek sayfalık içinde sığamıyorsa sonra REST API aracılığıyla devamlılık belirteci döndürür `x-ms-continuation-token` yanıtı üstbilgisi. İstemcileri sonraki sonuçlarında üstbilgi dahil olmak üzere sonuçları sayfalara bölme. Sayfa başına sonuç sayısı da aracılığıyla denetlenebilir `x-ms-max-item-count` numara üstbilgi. Belirtilen sorgu gibi bir toplama işlevi varsa `COUNT`, sorgu sayfası sonuçlar sayfası kısmen toplu değer döndürebilir sonra. İstemciler, son, örneğin sonuçlar, toplam sayı bu sayıyı dönmek için tek tek sayfaları döndürülen sayıları üzerinden toplamak için bu sonuçlar ikinci düzey toplama gerçekleştirmeniz gerekir.

Sorguları için veri tutarlılığı ilkelerini yönetmek için `x-ms-consistency-level` tüm REST API istekleri gibi üstbilgi. Oturum tutarlılığı için de en son echo gerekli `x-ms-session-token` sorgu istekte tanımlama bilgisi üstbilgisi. Sorgulanan koleksiyonunun dizin oluşturma ilkesini de sorgu sonuçları tutarlılığını etkileyebilir. İle dizin oluşturma ilkesi ayarları varsayılan olarak koleksiyonlar için dizini her zaman ile belge içeriklerini geçerli ve sorgu sonuçları için veri seçilen tutarlılık eşleşmesi. Lazy için dizin oluşturma ilkesini rahat, sorgular eski sonuçlar döndürebilir. Daha fazla bilgi için bkz: [Azure Cosmos DB tutarlılık düzeylerini][consistency-levels].

Koleksiyon için yapılandırılan dizin oluşturma ilkesini belirtilen sorgu destekleyemiyorsa, Azure Cosmos DB sunucusu 400 "hatalı istek" döndürür. Bu karma (eşitlik) aramaları için ve dizine almasını açıkça yolları için yapılandırılmış yolları aralığı sorguları için döndürülür. `x-ms-documentdb-query-enable-scan` Başlığı, bir dizini olmadığında bir tarama gerçekleştirmek sorgu izin vermek için belirtilebilir.

Ayrıntılı ölçümler ayarlayarak sorgu yürütme elde edebilirsiniz `x-ms-documentdb-populatequerymetrics` başlığına `True`. Daha fazla bilgi için bkz: [Azure Cosmos DB SQL sorgu ölçümlerini](documentdb-sql-query-metrics.md).

### <a id="DotNetSdk"></a>C# (.NET) SDK
LINQ ve SQL .NET SDK'yı destekleyen sorgulama. Aşağıdaki örnek, bu belgenin önceki bölümlerinde sunulan basit filtre sorgusu gerçekleştirmeye gösterilmiştir.

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


Bu örnek her belge içinde eşitlik için iki özellik karşılaştırır ve anonim tahminleri kullanır. 

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


Sonraki örnek LINQ SelectMany ifade birleştirmeler gösterir.

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



.NET istemci otomatik olarak yukarıda gösterildiği gibi foreach blokları sorgu sonuçlarında tüm sayfaları aracılığıyla yineler. REST API bölümünde sunulan sorgu seçeneklerini de .NET SDK kullanarak kullanılabilir `FeedOptions` ve `FeedResponse` CreateDocumentQuery yöntemi sınıflarda. Sayfa sayısı kullanılarak denetlenebilir `MaxItemCount` ayarı. 

Disk belleği oluşturarak açıkça kontrol edebilirsiniz `IDocumentQueryable` kullanarak `IQueryable` okuyarak ardından nesne` ResponseContinuationToken` değerleri ve bunları geçirme geri olarak `RequestContinuationToken` içinde `FeedOptions`. `EnableScanInQuery`Sorgu yapılandırılmış bir dizin oluşturma ilkesi tarafından desteklendiğinde taramaları etkinleştirmek için ayarlanabilir. Bölümlenmiş koleksiyonlar için kullandığınız `PartitionKey` karşı tek bir sorguyu çalıştırmak için bölüm (Cosmos DB otomatik olarak bu sorgu metni ayıklayabilirsiniz rağmen), ve `EnableCrossPartitionQuery` karşı birden çok bölüm çalıştırılması gereken sorguları çalıştırmak için. 

Başvurmak [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-documentdb-net) sorguları içeren daha fazla örnekleri için. 

### <a id="JavaScriptServerSideApi"></a>JavaScript sunucu tarafı API
Cosmos DB JavaScript tabanlı uygulama mantığını saklı yordamları ve Tetikleyicileri kullanarak doğrudan koleksiyonlar üzerinde yürütmek için bir programlama modeli sağlar. Bir koleksiyon düzeyinde kayıtlı JavaScript mantığı sonra verilen koleksiyon belgelerde işlemlerini veritabanı işlemlerini verebilir. Bu işlemler çevresel ACID işlemlerini sarılır.

Aşağıdaki örnek queryDocuments JavaScript Sunucusu API sorgularından yapmak için nasıl kullanılacağını gösterir iç saklı yordamları ve Tetikleyicileri.

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
1. [Azure Cosmos DB giriş][introduction]
2. [Azure Cosmos veritabanı SQL belirtimi](http://go.microsoft.com/fwlink/p/?LinkID=510612)
3. [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-documentdb-net)
4. [Azure Cosmos DB tutarlılık düzeyleri][consistency-levels]
5. ANSI SQL 2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
6. JSON [http://json.org/](http://json.org/)
7. JavaScript belirtimi [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm) 
8. LINQ [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx) 
9. Sorgu büyük veritabanları için değerlendirme teknikleri [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611)
10. Sorgu 1994 paralel ilişkisel veritabanı sistemleri, IEEE bilgisayar topluluğu basın, işleme
11. Lu, Ooi, Bronz, sorgu 1994 paralel ilişkisel veritabanı sistemleri, IEEE bilgisayar topluluğu basın, işleme.
12. Christopher Olston, Benjamin Reed, Utkarsh Srivastava, Ravi Kumar, Barış Tomkins: Pig Latin: veri işleme, SIGMOD 2008 için Not şekilde yabancı dil.
13. G. Graefe. Sorgu iyileştirme için basamaklar çerçevesi. IEEE veri Müh Bull., 18(3): 1995.

[1]: ./media/documentdb-sql-query/sql-query1.png
[introduction]: introduction.md
[consistency-levels]: consistency-levels.md