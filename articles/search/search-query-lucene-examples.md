---
title: Lucene sorgu örnekler Azure arama için | Microsoft Docs
description: Lucene sorgu söz dizimi belirsiz arama, yakınlık araması, terim artırma, normal ifade araması ve joker karakterle arama için.
author: LiamCa
manager: jlembicz
tags: Lucene query analyzer syntax
services: search
ms.service: search
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: liamca
ms.openlocfilehash: 46e03834cb307ea103a8794616f6f38227881272
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="lucene-query-syntax-examples-for-building-queries-in-azure-search"></a>Lucene sorgu söz dizimi örnekler Azure arama sorguları oluşturmak için
Azure arama sorguları oluşturmak, her iki varsayılan kullanabilirsiniz [Basit Sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) veya diğer [Lucene sorgu ayrıştırıcı Azure Search'te](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search). Lucene sorgu ayrıştırıcı alan kapsamlı sorgular, benzer arama, yakınlık araması, terim artırma ve normal ifade araması gibi daha karmaşık sorgu yapıları destekler.

Bu makalede, sorgu işlemleri kullanılabilir tam sözdizimi kullanılırken gösteren örneklerle geçebilirsiniz.

## <a name="viewing-the-examples-in-jsfiddle"></a>Örnekler JSFiddle içinde görüntüleme

Bu makaledeki örneklerde önceden yüklenmiş bir arama dizini karşı çalışan yürütülebilir sorguları tümü [JSFiddle](https://jsfiddle.net), komut dosyası ve HTML test etmek için bir çevrimiçi Kod Düzenleyicisi. 

Bunları çalıştırmak için sorgu örnek URL'lerin JSFiddle ayrı bir tarayıcı penceresinde açmak için sağ tıklayın.

> [!NOTE]
> Aşağıdaki örnekler tarafından sağlanan bir veri kümesini temel işleri kullanılabilir oluşan bir arama dizinini yararlanın [New York OpenData Şehir](https://nycopendata.socrata.com/) girişimi. Bu veriler, geçerli veya tam düşünülmemelidir. Microsoft tarafından sağlanan bir korumalı alan hizmetine dizinidir. Bir Azure aboneliği veya bu sorguları denemek için Azure Search gerekmez.
>


## <a name="how-to-invoke-full-lucene-parsing"></a>Tam Lucene ayrıştırma çağırmak nasıl

Bu makaledeki örneklerde tümünün belirtin **queryType tam =** tamamını Lucene sorgu ayrıştırıcı tarafından işlendiğini gösteren parametre arayın. 

**Örnek 1** --JSFiddle yükleyen ve sorgu çalıştıran yeni bir tarayıcı sayfasını açmak için aşağıdaki sorguyu parçacığını sağ tıklatın:

* [& queryType tam = & arama = *](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*)

Yeni tarayıcı penceresinde JavaScript kaynak ve HTML çıkışını yan yana sunulur. Komut dosyası tam bir sorgu (bağlantıyı gösterildiği gibi yalnızca kod parçacığında,) başvuruda bulunuyor. Tam sorgu, her örneğin URL'lerde gösterilir. 

Bu sorgu belgeleri de New York şehrinde işleri dizinimize (bir korumalı alan hizmeti yüklenmiş nycjobs) döndürür. Konuyu uzatmamak amacıyla, sorgu yalnızca iş başlıkları döndürülen belirtir. Tam temel sorgu aşağıdaki gibidir:

    http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*

**SearchFields** parametresi yalnızca iş başlığı alanı arama kısıtlar. **QueryType** ayarlanır **tam**, bu sorgu için Lucene sorgu ayrıştırıcı kullanmak için Azure Search talimatı verir.

> [!NOTE]
> Sorgu işlemi arka planda için bkz: [nasıl tam metin araması Azure Search'te çalışır](search-lucene-query-architecture.md). Arama parametreleri hakkında daha fazla bilgi için bkz: [Search belgeleri (Azure Search Hizmeti REST API'si)](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).
>

### <a name="fielded-query-operation"></a>Fielded sorgu işlemi
Bu makaledeki örneklerde belirterek değiştirebileceğiniz bir **fieldname:searchterm** burada alan tek bir sözcük ve arama terimini de tek bir sözcük veya tümcecik, isteğe bağlı olduğu bir fielded sorgu işlemi tanımlamak için oluşturma Boole işleçleri. Bazı örnekler aşağıdakileri içerir:

* business_title:(senior NOT junior)
* Durum: ("New York" ve "Yeni bölgesi")

Konum alanı iki farklı şehirlerde arama bu durumda olduğu gibi tek bir varlık olarak değerlendirilmesi için her iki dize istiyorsanız tırnak işaretleri içinde birden çok dizeleri yerleştirdiğinizden emin olun. Ayrıca, işleci'nın büyük harfli olmayan gördüğünüz gibi emin olun ve and

Belirtilen alan **fieldname:searchterm** aranabilir alan olması gerekir. Bkz: [Create Index (Azure Search Hizmeti REST API'si)](https://docs.microsoft.com/rest/api/searchservice/create-index) dizin öznitelikleri alan tanımlarında nasıl kullanıldığı hakkında bilgi.

**Örnek 2** --bu sorguyu arar bunları ancak değil çırak terim Kıdemli ile iş başlıkları için aşağıdaki sorguyu parçacığını sağ tıklatın:

* [& queryType tam = & arama business_title:senior değil = çırak](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26$select=business_title%26queryType=full%26search=business_title:senior+NOT+junior)

## <a name="fuzzy-search-example"></a>Benzer arama örneği
Benzer arama eşleştiğini bulur benzer yapım olması koşuluyla. Başına [Lucene belgelerine](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html), benzer aramaları temel [Damerau Levenshtein uzaklığı](https://en.wikipedia.org/wiki/Damerau%e2%80%93Levenshtein_distance).

Benzer arama yapmak için tilde Ekle "~" isteğe bağlı parametresi, 0 ve 2 ' nin arasında düzenleme uzaklığını belirtir bir değer tek bir sözcük sonunda simgesi. Örneğin, "mavi ~" veya "mavi ~ 1" döndürebildiği mavi, mavi ve Yapıştırıcı işlevi görür.

**Örnek 3** --aşağıdaki sorgu parçacığını sağ tıklayın. Bu sorgu, (burada, yanlış yazılmış) terim ilişkilendirme işleriyle arar:

* [& queryType tam = & arama business_title:asosiate = ~](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26$select=business_title%26queryType=full%26search=business_title:asosiate~)

> [!Note]
> Belirsiz sorguları olmayan [analiz](https://docs.microsoft.com/azure/search/search-lucene-query-architecture#stage-2-lexical-analysis), olabilen dallanma veya lemmatization bekliyorsanız, şaşırtıcı. Sözcük analiz yalnızca tam koşullarınızda gerçekleştirilen (terim sorgu veya tümcecik sorgusu). Sorgu türleri (önek sorgu, joker karakter sorgu, regex sorgu, benzer sorgu) tamamlanmamış koşullarla analysis aşaması atlayarak doğrudan sorgu ağacına eklenir. Tamamlanmamış sorgu terimlerinin üzerinde gerçekleştirilen yalnızca dönüştürmeyi lowercasing.
>

## <a name="proximity-search-example"></a>Yakınlık arama örneği
Yakınlık aramaları şartlarını bulmak için kullanılan olan diğer bir belge. Bir tilde Ekle "~" tümcecik sonunda simge izlenen yakınlık sınır oluşturmak sözcükler sayısına göre. Örneğin, "otel havaalanı" ~ 5 koşulları otel ve havaalanı birbiriyle 5 sözcük içinde bir belgede bulacaksınız.

**Örnek 4** --sorguyu sağ tıklayın. Birden fazla sözcük ayrıldığı "Kıdemli analist" terimi işleriyle arayın:

* [& queryType tam = & arama business_title =: "Kıdemli analist" ~ 1](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~1)

**Örnek 5** --sözcükler "Kıdemli analist" terimi arasında kaldırmayı yeniden deneyin.

* [& queryType tam = & arama business_title =: "Kıdemli analist" ~ 0](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~0)

## <a name="term-boosting-examples"></a>Örnekler artırmanın terimi
Terim artırma terimi içermeyen belgeleri göre boosted terimi içeriyorsa, daha yüksek bir belge sıralaması için ifade eder. Bu, belirli alanları yerine belirli terimleri Puanlama profilleri artırabilir, profilleri Puanlama farklıdır. Aşağıdaki örnek farklar göstermeye yardımcı olur.

Artırır bir Puanlama profili eşleşen belirli bir alana gibi göz önünde bulundurun **Tarz** musicstoreindex örnekte. Terim artırma daha fazla belirli arama terimleri diğerlerinden daha yüksek artırmak için kullanılabilir. Örneğin, "rock ^ 2 elektronik" arama terimlerini içeren belgeleri artıracak **Tarz** alan dizini diğer aranabilir alanlara daha yüksek. Ayrıca, arama terimi "rock" içeren belgeleri "(2) terim artırma değeri sonucunda elektronik" başka bir arama terimi daha yüksek derece verilecek.

Bir terim artırma, düzeltme işareti kullanmak için "^", simge arama terimi sonunda bir artırma faktörle (sayı). Yüksek yükseltme faktörü, daha ilgili terim diğer arama terimleri göreli olacaktır. Varsayılan olarak, yükseltme faktörü 1'dir. Yükseltme faktörü pozitif olması gerekse de (örneğin, 0.2) 1'den küçük olabilir.

**Örnek 6** --sorguyu sağ tıklayın. "Nerede biz sözcükler bilgisayar hem analist sonuç yok henüz olan sonuçlar en üstte analist işleri görmek bilgisayar analist" terimi işleriyle arayın.

* [& queryType tam = & arama business_title:computer analist =](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

**Örnek 7** --yeniden deneyin, her iki sözcük yoksa bu zaman artırmanın terim bilgisayarla terim analist sonuçlanır.

* [& queryType tam = & arama business_title:computer = ^ 2 analisti](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

## <a name="regular-expression-example"></a>Normal ifade örneği
Normal ifade araması eğik arasında "/", içinde belgelenen olarak içeriğine göre bir eşleşme bulur [RegExp sınıfı](http://lucene.apache.org/core/4_10_2/core/org/apache/lucene/util/automaton/RegExp.html).

**Örnek 8** --sorguyu sağ tıklayın. Ya da işleriyle Kıdemli veya alt düzey terimini arayın.

* `&queryType=full&$select=business_title&search=business_title:/(Sen|Jun)ior/`

Bu örnek için URL sayfanın düzgün çalışmaz. Geçici bir çözüm olarak aşağıdaki URL'yi kopyalayın ve tarayıcı URL adresine yapıştırın: `http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26queryType=full%26$select=business_title%26search=business_title:/(Sen|Jun)ior/)`

## <a name="wildcard-search-example"></a>Joker karakter arama örneği
Birden çok için genellikle tanınan söz dizimini kullanabilirsiniz (\*) ya da tek (?) karakteri joker aramalar. Lucene sorgu ayrıştırıcı tek bir terim ve bir deyimi bu simgeleri kullanımını desteklediğini unutmayın.

**Örnek 9** --sorguyu sağ tıklayın. 'Da iş başlıkları programlama hüküm ve programcı da içerir prog' öneki içeren işleri arayın.

* [& queryType tam & $select = business_title & arama = business_title:prog* =](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26queryType=full%26$select=business_title%26search=business_title:prog*)

Kullanarak bir * veya? bir arama ilk karakteri olarak simge.

## <a name="next-steps"></a>Sonraki adımlar
Lucene sorgu ayrıştırıcı kodunuzda belirtmeyi deneyin. Aşağıdaki bağlantılar arama sorguları .NET ve REST API için nasıl yapılacağını açıklar. Bağlantıları belirtmek için bu makalede öğrendiklerinizi uygulamak gerekir varsayılan basit söz dizimini kullanın **queryType**.

* [Azure Search .NET SDK kullanarak dizininizi sorgulama](search-query-dotnet.md)
* [Azure Search REST API kullanarak dizininizi sorgulama](search-query-rest-api.md)

## <a name="see-also"></a>Ayrıca bkz.

 [Azure Search'te nasıl tam metin araması çalışır](search-lucene-query-architecture.md)