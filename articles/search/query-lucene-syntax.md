---
title: Lucene sorgu söz dizimi - Azure Search
description: Azure Search ile kullanılan tam Lucene sözdizimi için başvuru.
services: search
ms.service: search
ms.topic: conceptual
ms.date: 03/25/2019
author: brjohnstmsft
ms.author: brjohnst
ms.manager: cgronlun
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: 64a688df3b6ed8602bb440d72e7f061c5f5893d1
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58885613"
---
# <a name="lucene-query-syntax-in-azure-search"></a>Azure Search'te Lucene sorgu sözdizimi
Azure arama sorguları dayalı zengin üzerinde yazma [Lucene sorgu ayrıştırıcısına](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html) özel sorgu formları için söz dizimi: joker karakter, belirsiz arama, yakınlık araması, normal ifadeler birkaç örnek verilmiştir. Lucene sorgu ayrıştırıcısına sözdizimi çok [bozulmadan Azure Search'te uygulanan](search-lucene-query-architecture.md), dışında *aralığı aramaları* Azure Search ile oluşturulmuş `$filter` ifadeler. 

## <a name="how-to-invoke-full-parsing"></a>Tam ayrıştırma çağırmak nasıl

Ayarlama `queryType` arama parametresini kullanmak için hangi ayrıştırıcı belirtmek için. Geçerli değerler `simple|full`, ile `simple` varsayılan olarak ve `full` Lucene için. 

<a name="bkmk_example"></a> 

### <a name="example-showing-full-syntax"></a>Örnek gösteren tam söz dizimi

Aşağıdaki örnek, yetkisiz değiştirmeye karşı korumalı Lucene sorgu sözdizimini kullanarak dizindeki belgeleri bulur `queryType=full` parametresi. Bu sorgu, burada "budget" terimi alan içerir ve "son renovated" ifadesini içeren tüm aranabilir alanları hotels döndürür. "Son renovated" ifadesini içeren belgeleri yüksek terim artırma değeri (3) sonucu olarak sıralanır.  

`searchMode=all` Parametresi, bu örnekte ilgili. Sorgu işleçleri söz konusu olduğunda, genellikle ayarlamalısınız `searchMode=all` emin olmak için *tüm* ölçütleriyle eşleşen.

```
GET /indexes/hotels/docs?search=category:budget AND \"recently renovated\"^3&searchMode=all&api-version=2015-02-28&querytype=full
```

 Alternatif olarak, POST kullanın:  

```
POST /indexes/hotels/docs/search?api-version=2015-02-28
{
  "search": "category:budget AND \"recently renovated\"^3",
  "queryType": "full",
  "searchMode": "all"
}
```

Diğer örnekler için [Lucene sorgu söz dizimi örnekleri, Azure Search'te sorgular oluşturmaya yönelik](search-query-lucene-examples.md). Sorgu parametreleri, tam contingent belirtme hakkında daha fazla ayrıntı için bkz. [arama belgeleri &#40;Azure arama hizmeti REST API'si&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).

> [!NOTE]  
>  Azure Search'ü de desteklediği [Basit Sorgu söz dizimi](query-simple-syntax.md), basit ve güçlü sorgu basit anahtar sözcük araması için kullanılan dili.  

##  <a name="bkmk_syntax"></a> Temel söz dizimi bilgileri  
 Lucene sözdizimi kullanan tüm sorguları için aşağıdaki söz dizimini temelleri uygulayın.  

### <a name="operator-evaluation-in-context"></a>Bağlamda değerlendirme işleci

Yerleştirme, sembol işleç veya başka bir karakter, bir dize olarak yorumlanır olup olmadığını belirler.

Örneğin, Lucene tam sözdiziminde, belirsiz arama ve yakınlık araması için tilde (~) kullanılır. Tırnak içine alınmış bir ifade sonra yerleştirildiğinde ~ yakınlık araması çağırır. Bir terimi sonunda yerleştirildiğinde ~ belirsiz arama çağırır.

İçinde bir terim gibi "iş ~ analist", karakterin bir operatör olarak değerlendirilmez. Bu durumda, sorgu varsayılarak izin ver olan terimini veya tümceciğini sorgu [tam metin araması](search-lucene-query-architecture.md) ile [sözcük analizi](search-lucene-query-architecture.md#stage-2-lexical-analysis) kullanıma şeritler ~ ve dönem sonu "iş ~ analist" iki: iş veya analist.

Yukarıdaki örnekte, tilde (~) olmakla birlikte her işleci için de aynı ilke geçerlidir.

### <a name="escaping-special-characters"></a>Kaçış özel karakterleri

 Özel karakterler arama metni bir parçası olarak kullanılacak kaçış karakterleri eklenmelidir. Bunları ters eğik çizgi koyarak çıkış (\\). Atlanması gereken özel karakterler şunlardır:  
`+ - && || ! ( ) { } [ ] ^ " ~ * ? : \ /`  

 Örneğin, bir joker karakter kaçış için kullanın \\*.

### <a name="encoding-unsafe-and-reserved-characters-in-urls"></a>URL güvenli olmayan ve ayrılmış karakter kodlama

 Tüm güvenli hem de ayrılmış karakterleri bir URL kodlanmış emin olun. Örneğin, '#', bir URL fragement/bağlantı tanımlayıcıda olduğundan güvenli olmayan bir karakterdir. İçin karakter kodlanmalı `%23` URL'de kullanılan. ' &' and '=' parametreleri sınırlandırın ve Azure arama'değerlerini belirtin ayrılmış karakterleri örnekleridir. Lütfen [RFC1738: Tekdüzen Kaynak Konum Belirleyicisi (URL)](https://www.ietf.org/rfc/rfc1738.txt) daha fazla ayrıntı için.

 Güvenli olmayan karakterleri ``" ` < > # % { } | \ ^ ~ [ ]``. Ayrılmış karakterler `; / ? : @ = + &`.

### <a name="precedence-operators-grouping-and-field-grouping"></a>Öncelik işleçleri: gruplandırma ve alan gruplandırma  
 Parantez, alt sorgularda, parantez ifadesi içinde işleçleri dahil olmak üzere oluşturmak için kullanabilirsiniz. Örneğin, `motel+(wifi||luxury)` "motel" terimini ve "wifi" veya "lüks" (veya her ikisi de) içeren belgeleri arar.

Alan gruplamasıdır benzer ancak tek bir alan için gruplandırma kapsamları. Örneğin, `hotelAmenities:(gym+(wifi||pool))` "hotelAmenities" alanı "uygulamaları" ve "wifi" veya "uygulamaları" ve "havuzu" için arama yapar.  

### <a name="searchmode-parameter-considerations"></a>SearchMode parametre konuları  
 Etkisini `searchMode` açıklandığı sorguları [Basit Sorgu söz dizimi Azure Search'te](query-simple-syntax.md), Lucene sorgu söz dizimi için eşit oranda geçerlidir. Yani, `searchMode` NOT ile birlikte işleçleri parametresi nasıl kümesinin etkilerini Temizle değilseniz, bu olağan dışı görünebilir sorgu sonuçlarını neden olabilir. Varsayılan olarak tutuyorsanız `searchMode=any`ve "New York" değil "Seattle" Seattle olmayan tüm şehirleri döndürür, işlemi bir NOT işleci veya eylem olarak hesaplanır.  

##  <a name="bkmk_boolean"></a> Boole işleçleri (AND, OR, NOT) 
 Her zaman tüm harflerini büyük metin Boole işleçleri (AND, OR, NOT) belirtin.  

### <a name="or-operator-or-or-"></a>OR işleci `OR` veya `||`

OR işleci, dikey çubuk veya dikey çizgi karakterinden ' dir. Örneğin: `wifi || luxury` "wifi" veya "lüks" veya her ikisini de içeren belgeleri arar. Olmadığından veya varsayılan birleştirme işleci büyütüp genişletebilir, aynı zamanda bırakabilir şekilde `wifi luxury` eşdeğerdir `wifi || luxuery`.

### <a name="and-operator-and--or-"></a>VE işleci `AND`, `&&` veya `+`

AND işleci, ve işareti veya artı işareti ' dir. Örneğin: `wifi && luxury` hem "wifi" ve "lüks" içeren belgeleri arar. (+) Artı karakteri için gerekli koşulları kullanılır. Örneğin, `+wifi +luxury` her iki terim tek bir belge alanında yerde görünmelidir değerlendirmeyeceği koşulunu koyar.


### <a name="not-operator-not--or--"></a>NOT işleci `NOT`, `!` veya `-`

NOT işleci bir ünlem işareti veya eksi işareti var. Örneğin: `wifi !luxury` "wifi" olan belgeleri terimi ve/veya "lüks" olmaması için arama yapar. `searchMode` Bir terim işleç and işleciyle ORed olmaması sorgusunda diğer koşullarıyla olup denetimleri seçeneği bir + veya || işleci. Bu geri çağırma `searchMode` olarak ayarlanabilir `any`(varsayılan) veya `all`.

Kullanarak `searchMode=any` "Veya NOT" yorumlanacaktır - daha fazla sonuç ekleyerek ve varsayılan değer sorguları geri çağırma bildirimi yayımlayabiliriz artırır. Örneğin, `wifi -luxury` ya da terimi içeren belgeleri eşleşecektir *wifi* ya da terim içermeyen *lüks.*

Kullanarak `searchMode=all` daha az sonuç ekleyerek ve varsayılan değer sorguları duyarlığını artırır - "Ve NOT" yorumlanacaktır. Örneğin, `wifi -luxury` terimlerini içeren belgelerle eşleşmesini `wifi` ve terim içermeyen `luxury`. Bu tartışmaya bir daha sezgisel, davranıştır işleci. Bu nedenle, seçme düşünmelisiniz `searchMode=all` üzerinden `searchMode=any` , duyarlık geri çekme yerine arar en iyi duruma getirmek istiyorsanız *ve* kullanıcılarınızın sık kullandığınız `-` aramalarındaki işleci.

##  <a name="bkmk_querysizelimits"></a> Sorgu boyutu sınırlamaları  
 Bir Azure Search'e göndermek için sorgu boyutu sınırı yoktur. Özellikle, en fazla 1024 yan tümceleri (AND, OR ve benzeri ayrılmış ifadeler) olabilir. Yaklaşık 32 KB boyutunda bir sorguda herhangi bir tek terimli bir sınır yoktur. Uygulamanızı program aracılığıyla arama sorguları oluşturursa, sınırsız boyutu sorguları oluşturmaz şekilde tasarlama öneririz.  

##  <a name="bkmk_searchscoreforwildcardandregexqueries"></a> Joker karakter ve normal ifade sorguları Puanlama
 Azure Search kullanan sıklığa dayalı Puanlama ([TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)) metin sorguları için. Bununla birlikte, burada koşulları kapsamında potansiyel olarak geniş olabilir joker karakter ve normal ifade sorgular için eşleşmeler arasından nadir koşulları doğrultusunda biasing gelen sıralaması önlemek için sıklığı çarpanı yoksayılır. Tüm eşleşmeleri için joker karakter ve normal ifade eşit olarak kabul edilir arar.

##  <a name="bkmk_fields"></a> Alan kapsamlı sorgular  
 Belirtebileceğiniz bir `fieldname:searchterm` burada tek bir sözcük alanıdır ve arama terimini de tek bir sözcük veya tümcecik, Boole işleçleri ile isteğe bağlı olarak bir fielded sorgu işlemi tanımlamak için yapı. Bazı örnekler şunlardır:  

- genre:jazz NOT history  

- sanatçıların: ("mil Davis" "John Coltrane")

  Bu durumda iki ayrı Sanatçılar arama tek bir varlık olarak değerlendirilebilmesi için her iki dize istiyorsanız birden çok dizeyi tırnak işaretleri içinde yerleştirdiğinizden emin olun `artists` alan.  

  Belirtilen alan `fieldname:searchterm` olmalıdır bir `searchable` alan.  Bkz: [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index) dizin özniteliklerini alan tanımlarını nasıl kullanıldığı hakkında ayrıntılar için.  

##  <a name="bkmk_fuzzy"></a> Belirsiz arama  
 Belirsiz arama eşleşme bulur benzer bir yapı olması koşuluyla. Başına [Lucene belgeleri](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html), belirsiz arama temel [Damerau Levenshtein uzaklık](https://en.wikipedia.org/wiki/Damerau%E2%80%93Levenshtein_distance). Belirsiz arama, en yüksek uzaklığı ölçütleri karşılayan 50 koşulları kadar bir terim genişletebilirsiniz. 

 Belirsiz arama yapmak için tilde kullanın "~" isteğe bağlı bir parametre, düzenleme uzaklığı belirten 0-2 (varsayılan) arasında bir sayı ile tek bir sözcük sonuna simgesi. Örneğin, "mavi ~" veya "mavi ~ 1" şunu döndürür "mavi", "mavi" ve "Yapıştırıcı".

 Belirsiz arama terimleri, olmayan tümcecikler, yalnızca uygulanabilir, ancak tek tek bir çok parçalı adın veya ifade de her terim için tilde ekleyebilirsiniz. Örneğin, "Unviersty ~, ~" Wshington ~ ""University of Washington üzerinde"eşleşir.
 

##  <a name="bkmk_proximity"></a> Yakınlık araması  
 Yakınlık arama koşulları bulmak için kullanılan olan birbirine yakın bir belgede. Bir tilde işareti ekle "~" simgesiyle sonunda bir ifade, yakınlık sınır oluşturan bir kelimelerin sayısı. Örneğin, `"hotel airport"~5` birbiriyle 5 sözcük içinde "havaalanı" ve "otel" koşulları bir belgede bulur.  


##  <a name="bkmk_termboost"></a> Terim artırma  
 Terimle terim içermeyen belgeleri göre artırmalı terimi içeriyorsa, daha yüksek bir belge sıralaması için ifade eder. Bu, Puanlama profillerini belirli alanları yerine belirli koşulları artırın, Puanlama profilleri öğesinden farklıdır.  

Aşağıdaki örnek, farklar göstermeye yardımcı olur. Bir Puanlama, var olduğunu varsayın artırıyor profili belirli bir alanda deyin eşleşen *Tarz* içinde [musicstoreindex örnek](index-add-scoring-profiles.md#bkmk_ex). Terimle belirli arama terimleri diğerlerine göre daha yüksek daha fazla artırmak için kullanılabilir. Örneğin, `rock^2 electronic` diğer aranabilir alanları dizindeki daha yüksek Tarz alanında arama terimlerini içeren belgeleri artırır. Daha fazla, arama terimini içeren belgeleri *rock* bir arama terimi daha yüksek derece verilecek *elektronik* sonucunda terim artırma değeri (2).  

 Bir terimi artırmak için giriş işaretini kullanın "^", sembol arama terimi sonunda bir boost faktörle (sayı). İfadeleri de artırabilir. Yüksek boost faktör, daha fazla ilgili terimi diğer arama terimlerini göreli olacaktır. Varsayılan olarak, boost faktörü 1'dir. Boost çarpanı sıfırdan büyük olmalı ancak (örneğin, 0,20) 1'den küçük olabilir.  

##  <a name="bkmk_regex"></a> Normal ifade araması  
 Bir normal ifade araması eğik arasında "/", belgelenmiş içinde olarak içeriğine göre bir eşleşme bulur [RegExp sınıfı](https://lucene.apache.org/core/4_10_2/core/org/apache/lucene/util/automaton/RegExp.html).  

 Örneğin, "motel" veya "otel" içeren belgeleri bulmak için belirtmeniz `/[mh]otel/`.  Normal ifade arama sözcükleri karşı eşleştirilir.   

##  <a name="bkmk_wildcard"></a> joker karakter araması  
 Genel olarak kabul edilen sözdizimi için birden çok (*) veya tek (?) kullanabilirsiniz. joker karakter aramalarını karakter. Lucene sorgu ayrıştırıcısına Not tek bir terim ve bir ifade ile bu sembolleri kullanımını destekler.  

 Örneğin, "Not", "Not" veya "Not" gibi önek sözcükleri içeren belgeleri bulmak için "Not *" belirtin.  

> [!NOTE]  
>  Kullanamazsınız bir * veya? Sembol arama ilk karakteri olarak.  
>  Hiçbir metin analizi joker arama sorguları üzerinde gerçekleştirilir. Sorgu zamanında joker sorgu terimleriyle arama dizini çözümlenen koşullarını karşı karşılaştırılır ve genişletilmiş.

## <a name="see-also"></a>Ayrıca bkz.  

+ [Search belgeleri](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
+ [OData ifadesi söz dizimi filtreleri ve sıralama](query-odata-filter-orderby-syntax.md)   
+ [Azure Search'te Basit Sorgu söz dizimi](query-simple-syntax.md)   
