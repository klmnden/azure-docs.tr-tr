---
title: Lucene sorgu örnekleri için Azure Search | Microsoft Docs
description: Lucene sorgu söz dizimi belirsiz arama, yakınlık araması, terimle, normal ifade araması ve joker karakter araması için.
author: LiamCa
manager: pablocas
tags: Lucene query analyzer syntax
services: search
ms.service: search
ms.topic: conceptual
ms.date: 06/28/2018
ms.author: liamca
ms.openlocfilehash: 24fa427ad67a953020370a16b4d156c82a0a1cf6
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39036675"
---
# <a name="lucene-query-syntax-examples-for-building-queries-in-azure-search"></a>Lucene sorgu söz dizimi örnekleri, Azure Search'te sorgular oluşturmak için
Azure arama için sorgular oluşturmak, ya da varsayılan kullanabilirsiniz [Basit Sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) veya alternatif [Azure Search'te Lucene sorgu ayrıştırıcısına](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search). Lucene sorgu ayrıştırıcısına alan kapsamlı sorgular, belirsiz arama, yakınlık araması, terimle ve normal ifade araması gibi daha karmaşık sorgu yapılarını destekler.

Bu makalede, tam sözdizimini kullanırken kullanılabilir sorgu işlemleri gösteren örnekler geçebilirsiniz.

## <a name="viewing-the-examples-in-jsfiddle"></a>Örnekler JSFiddle içinde görüntüleme

Bu makaledeki örneklerde önceden yüklenmiş bir arama dizinini karşı çalışan yürütülebilir sorguları tümü [JSFiddle](https://jsfiddle.net), betik ve HTML test etmek için bir çevrimiçi Kod Düzenleyicisi. 

Bunları çalıştırmak için sorgu örnek URL'lerin JSFiddle ayrı bir pencerede açmak için sağ tıklayın.

> [!NOTE]
> Aşağıdaki örnekler tarafından sağlanan bir veri kümesini temel alan işleri kullanılabilir oluşan bir arama dizini yararlanarak [New York City OpenData](https://nycopendata.socrata.com/) girişim. Bu veriler, geçerli ya da tam düşünülmemelidir. Microsoft tarafından sağlanan bir korumalı alan hizmeti dizinidir. Bir Azure aboneliği veya Azure Search'ün bu sorguları deneyin gerekmez.
>


## <a name="how-to-invoke-full-lucene-parsing"></a>Tam Lucene ayrıştırma çağırmak nasıl

Tüm bu makaledeki örneklerde belirtin **queryType = full** arama parametresi, gösteren tam sözdizimini Lucene sorgu ayrıştırıcı tarafından işlenir. 

**Örnek 1** --JSFiddle yükler ve sorguyu çalıştıran yeni bir tarayıcı sayfasını açmak için aşağıdaki sorgu kod parçacığı sağ tıklayın:

* [& queryType = full & arama = *](https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*)

Yeni tarayıcı penceresinde çıkış HTML ve JavaScript kaynak yan yana sunulur. Betik, tam bir sorgu (bağlantıya gösterildiği gibi yalnızca kod parçacığında) başvuruyor. Tam sorgu her örnek için URL'leri gösterilir. 

Bu sorgu belgeleri New York City işleri dizinimizi (bir korumalı alan hizmete yüklenen nycjobs) döndürür. Konuyu uzatmamak amacıyla, yalnızca iş başlıkları döndürülen sorguyu belirtir. Tam temel alınan sorgu aşağıdaki gibidir:

    https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*

**SearchFields** parametresi yalnızca iş başlık alanı için arama kısıtlar. **QueryType** ayarlanır **tam**, Azure Search, Lucene sorgu ayrıştırıcısına bu sorgu için kullanılacak talimatı verir.

> [!NOTE]
> Sorgu işleme hakkında arka plan bilgileri için bkz. [nasıl tam metin araması Azure Search'te çalışır](search-lucene-query-architecture.md). Arama parametreleri hakkında daha fazla bilgi için bkz. [Search belgeleri (Azure Search Hizmeti REST API'si)](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).
>

### <a name="fielded-query-operation"></a>Fielded sorgu işlemi
Bu makaledeki örneklerde belirterek değiştirebileceğiniz bir **fieldname:searchterm** burada tek bir sözcük alanıdır ve arama terimini de tek bir sözcük veya tümcecik, isteğe bağlı olarak ile fielded sorgu işlemi tanımlamak için oluşturma Boole işleçleri. Bazı örnekler şunlardır:

* business_title:(senior NOT junior)
* Durum: ("New York" ve "Yeni Jersey")

Konum alanında iki farklı şehirleri arama bu örnekte olduğu gibi tek bir varlık olarak değerlendirilebilmesi için her iki dize istiyorsanız birden çok dizeyi tırnak işaretleri içinde emin olun. Ayrıca, işleç NOT ile gördüğünüz gibi büyük emin olun ve and

Belirtilen alan **fieldname:searchterm** aranabilir bir alanı olmalıdır. Bkz: [dizin oluşturma (Azure Search Hizmeti REST API'si)](https://docs.microsoft.com/rest/api/searchservice/create-index) dizin özniteliklerini alan tanımlarını nasıl kullanıldığı hakkında ayrıntılar için.

**Örnek 2** --bu sorgu, iş başlıkları terim Kıdemli bunları ancak değil çırak arar aşağıdaki sorgu kod parçacığı sağ tıklayın:

* [& queryType = full & araması business_title:senior değil = çırak](https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26$select=business_title%26queryType=full%26search=business_title:senior+NOT+junior)

## <a name="fuzzy-search-example"></a>Belirsiz arama örneği
Belirsiz arama eşleşme bulur benzer bir yapı olması koşuluyla. Başına [Lucene belgeleri](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html), belirsiz arama temel [Damerau Levenshtein uzaklık](https://en.wikipedia.org/wiki/Damerau%e2%80%93Levenshtein_distance).

Belirsiz arama yapmak için tilde ekleme `~` isteğe bağlı bir parametre, düzenleme uzaklığı belirten bir değeri 0. ve 2 arasındaki tek bir sözcük sonuna simgesi. Örneğin, `blue~` veya `blue~1` mavi, mavi ve bağlantılı döndürür.

**Örnek 3** --aşağıdaki sorgu kod parçacığı sağ tıklayın. Bu sorgu, (burada, yanlış yazılmış) terim ilişkilendirme işlerle arar:

* [& queryType = full & araması business_title:asosiate = ~](https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26$select=business_title%26queryType=full%26search=business_title:asosiate~)

> [!Note]
> Belirsiz sorguları olmayan [analiz](https://docs.microsoft.com/azure/search/search-lucene-query-architecture#stage-2-lexical-analysis), olabilen dallanma veya başsözcüğe bekliyorsanız şaşırtıcı. Sözcük analizi yalnızca tam koşullarınızda gerçekleştirilir (terimi sorgu veya sorgu deyimi). Sorgu türleri (ön ek sorgu, joker karakter sorgu, normal ifade sorgu, belirsiz sorgu) tamamlanmamış koşullarıyla analysis aşaması atlayarak doğrudan sorgu ağacına eklenir. Sorgu eksik koşullarınızda gerçekleştirilen yalnızca dönüşümü harfe.
>

## <a name="proximity-search-example"></a>Yakınlık araması örneği
Yakınlık arama koşulları bulmak için kullanılan olan birbirine yakın bir belgede. Bir tilde işareti ekle "~" simgesiyle sonunda bir ifade, yakınlık sınır oluşturan bir kelimelerin sayısı. Örneğin, "otel havaalanı" ~ 5 belgeye birbiriyle 5 sözcük içinde havaalanı ve koşulları otel bulacaksınız.

**Örnek 4** --sorguyu sağ tıklayın. Birden fazla sözcük ayrıldığı işlerle "üst düzey analist" terimini arayın:

* [& queryType = full & araması business_title =: "üst düzey analist" yaklaşık 1](https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~1)

**Örnek 5** --sözcükler "üst düzey analist" terimi arasında kaldırmayı yeniden deneyin.

* [& queryType = full & araması business_title =: "üst düzey analist" ~ 0](https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~0)

## <a name="term-boosting-examples"></a>Terim artırma örnekleri
Terimle terim içermeyen belgeleri göre artırmalı terimi içeriyorsa, daha yüksek bir belge sıralaması için ifade eder. Bu, Puanlama profillerini belirli alanları yerine belirli koşulları artırın, Puanlama profilleri öğesinden farklıdır. Aşağıdaki örnek, farklar göstermeye yardımcı olur.

Belirli bir alanda gibi eşleşen artırıyor bir Puanlama profili göz önünde bulundurun **Tarz** musicstoreindex örnekte. Terimle belirli arama terimleri diğerlerine göre daha yüksek daha fazla artırmak için kullanılabilir. Örneğin, "rock ^ 2 elektronik" arama terimlerini içeren belgeleri artıracak **Tarz** alan diğer aranabilir alanları dizindeki daha yüksek. Ayrıca, "harika" arama terimini içeren belgeleri "terimi boost değeri (2) sonucunda elektronik" başka bir arama terimi daha yüksek derece verilecek.

Giriş işaretini bir terim artırma kullanılacağı "^", sembol arama terimi sonunda bir boost faktörle (sayı). Yüksek boost faktör, daha fazla ilgili terimi diğer arama terimlerini göreli olacaktır. Varsayılan olarak, boost faktörü 1'dir. Boost çarpanı sıfırdan büyük olmalı ancak (örneğin, 0.2) 1'den küçük olabilir.

**Örnek 6** --sorguyu sağ tıklayın. "Sözcükler bilgisayar hem analist sonuç yok, ancak sonuçları en üstündeki analist işleri, burada görürüz bilgisayar analist" terimi ile işleri arayın.

* [& queryType = full & araması business_title:computer analist =](https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

**Örnek 7** --hem sözcük yoksa bu zaman artırma terimi bilgisayarla terimi analisti sonuçlarını, yeniden deneyin.

* [& queryType = full & araması business_title:computer = ^ 2 analisti](https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

## <a name="regular-expression-example"></a>Normal ifade örneği
Bir normal ifade araması eğik arasında "/", belgelenmiş içinde olarak içeriğine göre bir eşleşme bulur [RegExp sınıfı](http://lucene.apache.org/core/4_10_2/core/org/apache/lucene/util/automaton/RegExp.html).

**Örnek 8** --sorguyu sağ tıklayın. Ya da olan işler için üst düzey veya alt düzey terimini arayın.

* `&queryType=full&$select=business_title&search=business_title:/(Sen|Jun)ior/`

Bu örnek için URL, sayfanın düzgün şekilde oluşturulmaz. Geçici bir çözüm olarak aşağıdaki URL'yi kopyalayın ve tarayıcı URL'sini adrese yapıştırın: `https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26queryType=full%26$select=business_title%26search=business_title:/(Sen|Jun)ior/)`

## <a name="wildcard-search-example"></a>Joker karakter araması örneği
Birden çok genel olarak kabul edilen sözdizimini kullanabilirsiniz (\*) veya tek bir karakter joker karakter aramalarını (?). Lucene sorgu ayrıştırıcısına Not tek bir terim ve bir ifade ile bu sembolleri kullanımını destekler.

**Örnek 9** --sorguyu sağ tıklayın. 'İş başlıkları programlama terimleri ve programcı da dahil prog' ön eki içeren projeler için arama yapın.

* [& queryType = tam & $select business_title & arama = business_title:prog* =](https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26queryType=full%26$select=business_title%26search=business_title:prog*)

Kullanamazsınız bir * veya? Sembol arama ilk karakteri olarak.

## <a name="next-steps"></a>Sonraki adımlar
Lucene sorgu ayrıştırıcısına kodunuzda belirtmeyi deneyin. Aşağıdaki bağlantıları arama sorguları hem .NET hem de REST API için nasıl yapılacağını açıklar. Belirtmek için bu makalede öğrendiklerinizi uygulamak ihtiyacınız olacak şekilde bağlantıları varsayılan basit söz dizimi kullanın. **queryType**.

* [Azure Search .NET SDK kullanarak dizininizi sorgulama](search-query-dotnet.md)
* [Azure Search REST API kullanarak dizininizi sorgulama](search-query-rest-api.md)

## <a name="see-also"></a>Ayrıca bkz.

 [Metin arama Azure Search'te tam nasıl çalışır](search-lucene-query-architecture.md)
