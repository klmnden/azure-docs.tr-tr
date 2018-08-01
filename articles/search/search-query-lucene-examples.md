---
title: Lucene sorgu örnekleri için Azure Search | Microsoft Docs
description: Lucene sorgu söz dizimi belirsiz arama, yakınlık araması, terimle, normal ifade araması ve joker karakter için bir Azure Search hizmetinizde arama yapar.
author: LiamCa
manager: pablocas
tags: Lucene query analyzer syntax
services: search
ms.service: search
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: liamca
ms.openlocfilehash: d90a7b2d12a147b8020abbd51ef055f0e70471fb
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39365437"
---
# <a name="lucene-syntax-query-examples-for-building-advanced-queries-in-azure-search"></a>Lucene sözdizimi sorgu örnekleri, Azure Search'te Gelişmiş sorgular oluşturmak için
Azure arama için sorgular oluşturma sırasında varsayılan değiştirebilirsiniz [Basit Sorgu ayrıştırıcı](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) bir alternatif ile [Azure Search'te Lucene sorgu ayrıştırıcısına](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) özelleştirilmiş ve Gelişmiş sorguyu formüle etmek için tanımları. 

Lucene sorgu ayrıştırıcısına alan kapsamlı sorgular, belirsiz ve joker arama, yakınlık araması, terimle ve normal ifade araması gibi daha karmaşık sorgu yapılarını destekler. Ek güç ek işleme gereksinimleri ile birlikte gelir. Bu makalede, tam sözdizimini kullanırken kullanılabilir sorgu işlemleri gösteren örnekler geçebilirsiniz.

> [!Note]
> Birçok özel sorgu yapılarını tam Lucene sorgu söz dizimi etkin olmayan [çözümlenmiş metin](https://docs.microsoft.com/azure/search/search-lucene-query-architecture#stage-2-lexical-analysis), olabilen dallanma veya başsözcüğe bekliyorsanız şaşırtıcı. Sözcük analizi yalnızca tam koşullarınızda gerçekleştirilir (terimi sorgu veya sorgu deyimi). Sorgu türleri (ön ek sorgu, joker karakter sorgu, normal ifade sorgu, belirsiz sorgu) tamamlanmamış koşullarıyla analysis aşaması atlayarak doğrudan sorgu ağacına eklenir. Sorgu eksik koşullarınızda gerçekleştirilen yalnızca dönüşümü harfe. 
>

## <a name="formulate-requests-in-postman"></a>Postman'deki istekleri düzenleme

Aşağıdaki örnekler tarafından sağlanan bir veri kümesini temel alan işleri kullanılabilir oluşan NYC işleri arama dizini yararlanarak [New York City OpenData](https://nycopendata.socrata.com/) girişim. Bu veriler, geçerli ya da tam düşünülmemelidir. Diğer bir deyişle, bir Azure aboneliği veya Azure Search'ün bu sorguları deneyin gerekmez Microsoft tarafından sağlanan bir korumalı alan hizmeti dizinidir.

Ne gerekiyor, Postman veya HTTP GET isteği verme eşdeğer bir aracı değil. Daha fazla bilgi için [REST istemcileri ile Test](search-fiddler.md).

### <a name="set-the-request-header"></a>İstek üst bilgisini ayarlayın

1. İstek üstbilgisinde ayarlanan **Content-Type** için `application/json`.

2. Ekleme bir **api anahtarını**ve bu dize olarak ayarlayın: `252044BE3886FE4A8E3BAA4F595114BB`. NYC işleri dizini barındırma korumalı alan arama hizmeti için bir sorgu anahtarı budur.

İstek üstbilgisi belirttikten sonra onu tüm sorgular bu makalede, yalnızca takas tekrar kullanabilirsiniz **arama =** dize. 

  ![Postman isteği üst bilgisi](media/search-query-lucene-examples/postman-header.png)

### <a name="set-the-request-url"></a>İstek URL'sini ayarlayın

Azure arama uç noktası ve arama dizesini içeren URL'yi ile eşleştirilmiş bir GET komutu isteğidir.

  ![Postman isteği üst bilgisi](media/search-query-lucene-examples/postman-basic-url-request-elements.png)

URL'si birleşimi aşağıdaki öğeleri içerir:

+ **`https://azs-playground.search.windows.net/`** bir korumalı alan arama hizmeti, Azure Search geliştirme ekibi tarafından korunur. 
+ **`indexes/nycjobs/`** Bu hizmetin dizinlerini koleksiyonunda NYC işleri dizinidir. İstekte hizmet adını ve dizin gereklidir.
+ **`docs`** documents koleksiyonunu içeren tüm aranabilir içeriği. İstek üst bilgisinde sağlanan sorgunuzun api anahtarını, yalnızca documents koleksiyonunu hedefleyen okuma işlemleri üzerinde çalışır.
+ **`api-version=2017-11-11`** her istekte gerekli bir parametre olan api-version, ayarlar.
+ **`search=*`** ilk sorgu null sorgu dizesi, ilk 50 sonuçları döndüren (varsayılan).

## <a name="send-your-first-query"></a>İlk sorgunuzu Gönder

Bir doğrulama adımı aşağıdaki isteği GET yapıştırın ve tıklayın **Gönder**. Sonuçları ayrıntılı JSON belgeleri olarak döndürülür. Kopyalama bu URL'yi aşağıdaki ilk örnekte yapıştırma.

  ```http
  https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&search=*
  ```

Sorgu dizesi **`search=*`**, belirtilmeyen bir arama null veya boş aramaya eşdeğerdir. Özellikle kullanışlı değildir, ancak yapabileceğiniz basit arama olur.

İsteğe bağlı olarak ekleyebileceğiniz **`$count=true`** arama ölçütleriyle eşleşen belgelerin sayısını döndürmek için URL. Üzerinde bir boş bir arama dizesi (2802 NYC işleri söz konusu olduğunda) dizindeki tüm belgelerin budur.

## <a name="how-to-invoke-full-lucene-parsing"></a>Tam Lucene ayrıştırma çağırmak nasıl

Ekleme **queryType = full** varsayılan Basit Sorgu söz dizimi geçersiz kılma tam sorgu söz dizimi çağırmak için. 

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&queryType=full&search=*
```

Tüm bu makaledeki örneklerde belirtin **queryType = full** arama parametresi, gösteren tam sözdizimini Lucene sorgu ayrıştırıcı tarafından işlenir. 

## <a name="example-1-field-scoped-query"></a>Örnek 1: Alan kapsamlı sorgu

İlk sorgu (hem basit hem de tam sözdizimi çalışır) tam Lucene sözdizimi gösterimini değildir ancak biz makul okunabilir bir JSON yanıtı üreten bir temel sorgu kavramı tanıtmak için bu örneği sağlama. Konuyu uzatmamak amacıyla, sorgu hedefleyen yalnızca *business_title* alan ve yalnızca iş başlıkları döndürülür belirtir. 

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&queryType=full&search=*
```

**SearchFields** parametresi yalnızca iş başlık alanı için arama kısıtlar. **Seçin** parametre, sonuç kümesinde hangi alanların ekleneceğini belirler.

Bu sorgu için yanıt, aşağıdaki ekran görüntüsüne benzer görünmelidir.

  ![Postman örnek yanıt](media/search-query-lucene-examples/postman-sample-results.png)

Arama puanı belirtilmemiş olsa bile arama puanı her belge için de döndürülen fark etmiş olabilirsiniz. Arama puanı sonuçları sıralama düzenini belirten bir değer ile meta verileri olmasıdır. 1 Tekdüzen puanları olduğunda hiçbir sıralama veya arama değil, tam metin araması olduğundan ya da uygulamak için ölçüt olduğundan oluşur. Null arama için ölçüt yoktur ve geri dönmeyi satırlar rastgele sırayla şöyledir. Arama ölçütleri hakkında daha fazla tanım yararlanırken, anlamlı değerlere puanları gelişmek arama görürsünüz.

## <a name="example-2-in-field-filtering"></a>2. örnek:-Filtre alanda

Tam Lucene sözdizimi bir alandaki ifadeleri destekler. Bu sorgu iş başlıkları terim Kıdemli bunları ancak değil çırak arar:

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&queryType=full&search=business_title:senior+NOT+junior
```

Belirterek bir **fieldname:searchterm** oluşturma, burada tek bir sözcük alanıdır ve arama terimini de tek bir sözcük veya tümcecik, Boole işleçleri ile isteğe bağlı olarak bir fielded sorgu işlemi tanımlayabilirsiniz. Bazı örnekler şunlardır:

* business_title:(senior NOT junior)
* Durum: ("New York" ve "Yeni Jersey")

Konum alanında iki farklı şehirleri arama bu örnekte olduğu gibi tek bir varlık olarak değerlendirilebilmesi için her iki dize istiyorsanız birden çok dizeyi tırnak işaretleri içinde emin olun. Ayrıca, işleç NOT ile gördüğünüz gibi büyük emin olun ve and

Belirtilen alan **fieldname:searchterm** aranabilir bir alanı olmalıdır. Bkz: [dizin oluşturma (Azure Search Hizmeti REST API'si)](https://docs.microsoft.com/rest/api/searchservice/create-index) dizin özniteliklerini alan tanımlarını nasıl kullanıldığı hakkında ayrıntılar için.

## <a name="example-3-fuzzy-search"></a>Örnek 3: Belirsiz arama

Tam Lucene sözdizimi, belirsiz arama, benzer bir yapı koşullarınızda eşleşen da destekler. Belirsiz arama yapmak için tilde ekleme `~` isteğe bağlı bir parametre, düzenleme uzaklığı belirten bir değeri 0. ve 2 arasındaki tek bir sözcük sonuna simgesi. Örneğin, `blue~` veya `blue~1` mavi, mavi ve bağlantılı döndürür.

Bu sorgu için işleri "terimi ilişkilendirme (bilerek yanlış yazılmış)" arar:

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&queryType=full&search=business_title:asosiate~
```

Başına [Lucene belgeleri](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html), belirsiz arama temel [Damerau Levenshtein uzaklık](https://en.wikipedia.org/wiki/Damerau%e2%80%93Levenshtein_distance).

> [!Note]
> Belirsiz sorguları olmayan [analiz](https://docs.microsoft.com/azure/search/search-lucene-query-architecture#stage-2-lexical-analysis). Sorgu türleri (ön ek sorgu, joker karakter sorgu, normal ifade sorgu, belirsiz sorgu) tamamlanmamış koşullarıyla analysis aşaması atlayarak doğrudan sorgu ağacına eklenir. Sorgu eksik koşullarınızda gerçekleştirilen yalnızca dönüşümü harfe.
>

## <a name="example-4-proximity-search"></a>Örnek 4: Yakınlık araması
Yakınlık arama koşulları bulmak için kullanılan olan birbirine yakın bir belgede. Bir tilde işareti ekle "~" simgesiyle sonunda bir ifade, yakınlık sınır oluşturan bir kelimelerin sayısı. Örneğin, "otel havaalanı" ~ 5 belgeye birbiriyle 5 sözcük içinde havaalanı ve koşulları otel bulacaksınız.

Bu sorguda, "üst düzey analist tarafından birden fazla sözcük ayrıldığı" terimi ile işleri için:

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&queryType=full&search=business_title:%22senior%20analyst%22~1
```

Sözcükler "üst düzey analist" terimi arasında kaldırmayı yeniden deneyin. Önceki sorgunun 10 aksine bu sorgu için 8 belgeler döndürülür dikkat edin.

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&queryType=full&search=business_title:%22senior%20analyst%22~0
```

## <a name="example-5-term-boosting"></a>Örnek 5: Terimle
Terimle terim içermeyen belgeleri göre artırmalı terimi içeriyorsa, daha yüksek bir belge sıralaması için ifade eder. Giriş işaretini bir terim artırma kullanılacağı "^", sembol arama terimi sonunda bir boost faktörle (sayı). 

Bu konuda "önce" sorgu ifadesi olan işler için arama *bilgisayar analist* ve her iki sözcükleri içeren sonuç olduğuna dikkat edin *bilgisayar* ve *analist*, henüz  *bilgisayar* işleri, sonuçları üstünde.

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&queryType=full&search=business_title:computer%20analyst
```

Arama terimi sonuçlarıyla artırma şu "sonra" sorguda yineleme *analist* terimi üzerinden *bilgisayar* hem sözcük yoksa. 

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&queryType=full&search=business_title:computer%20analyst%5e2
```
Yukarıdaki sorguda daha fazla insan tarafından okunabilir sürümü `search=business_title:computer analyst^2`. Çalışılabilir bir sorgu için `^2` olarak kodlanmış `%5E2`, olduğu zor görmek için.

Terimle, Puanlama profillerini belirli alanları yerine belirli koşulları artırın, Puanlama profilleri öğesinden farklıdır. Aşağıdaki örnek, farklar göstermeye yardımcı olur.

Belirli bir alanda gibi eşleşen artırıyor bir Puanlama profili göz önünde bulundurun **Tarz** musicstoreindex örnekte. Terimle belirli arama terimleri diğerlerine göre daha yüksek daha fazla artırmak için kullanılabilir. Örneğin, "rock ^ 2 elektronik" arama terimlerini içeren belgeleri artıracak **Tarz** alan diğer aranabilir alanları dizindeki daha yüksek. Ayrıca, "harika" arama terimini içeren belgeleri "terimi boost değeri (2) sonucunda elektronik" başka bir arama terimi daha yüksek derece verilecek.

Faktörü düzeyi ayarlarken terimi yüksek boost faktör, daha fazla ilgili diğer arama terimlerini göreli olacaktır. Varsayılan olarak, boost faktörü 1'dir. Boost çarpanı sıfırdan büyük olmalı ancak (örneğin, 0.2) 1'den küçük olabilir.


## <a name="example-6-regex"></a>Örnek 6: normal ifade

Bir normal ifade araması eğik arasında "/", belgelenmiş içinde olarak içeriğine göre bir eşleşme bulur [RegExp sınıfı](http://lucene.apache.org/core/4_10_2/core/org/apache/lucene/util/automaton/RegExp.html).

Terim üst düzey veya alt düzey olan işler için bu sorgu, arama: ' arama business_title:/(Sen| = Haz) ior /''.

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&queryType=full&search=business_title:/(Sen|Jun)ior/
```
> [!Note]
> Regex sorguları olmayan [analiz](https://docs.microsoft.com/azure/search/search-lucene-query-architecture#stage-2-lexical-analysis). Sorgu eksik koşullarınızda gerçekleştirilen yalnızca dönüşümü harfe.
>

## <a name="example-7-wildcard-search"></a>Örnek 7: Joker karakter araması
Birden çok genel olarak kabul edilen sözdizimini kullanabilirsiniz (\*) veya tek bir karakter joker karakter aramalarını (?). Lucene sorgu ayrıştırıcısına Not tek bir terim ve bir ifade ile bu sembolleri kullanımını destekler.

Bu sorguda önek 'iş başlıkları programlama terimleri ve programcı da dahil prog' içeren işleri arayın.

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&queryType=full&search=business_title:prog*
```
Kullanamazsınız bir * veya? Sembol arama ilk karakteri olarak.

> [!Note]
> Joker karakter sorguları olmayan [analiz](https://docs.microsoft.com/azure/search/search-lucene-query-architecture#stage-2-lexical-analysis). Sorgu eksik koşullarınızda gerçekleştirilen yalnızca dönüşümü harfe.
>

## <a name="next-steps"></a>Sonraki adımlar
Lucene sorgu ayrıştırıcısına kodunuzda belirtmeyi deneyin. Aşağıdaki bağlantıları arama sorguları hem .NET hem de REST API için nasıl yapılacağını açıklar. Belirtmek için bu makalede öğrendiklerinizi uygulamak ihtiyacınız olacak şekilde bağlantıları varsayılan basit söz dizimi kullanın. **queryType**.

* [Azure Search .NET SDK kullanarak dizininizi sorgulama](search-query-dotnet.md)
* [Azure Search REST API kullanarak dizininizi sorgulama](search-query-rest-api.md)

Ek söz dizimi başvurusu, sorgu mimarisi ve örnekleri aşağıdaki bağlantılarda bulunabilir:

+ [Basit sözdizimi sorgu örnekleri](search-query-simple-examples.md)
+ [Metin arama Azure Search'te tam nasıl çalışır](search-lucene-query-architecture.md)
+ [Basit sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)
+ [Tam Lucene sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)