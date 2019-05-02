---
title: Azure HDInsight Phoenix performansı
description: Phoenix performansı iyileştirmek için en iyi yöntemler.
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: ashishth
ms.openlocfilehash: 4fc4d1843ddb8d007ca062d928ebbddf90909583
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64690042"
---
# <a name="apache-phoenix-performance-best-practices"></a>Apache Phoenix performansı için en iyi yöntemler

En önemli yönüyle [Apache Phoenix](https://phoenix.apache.org/) performansı, arka plandaki en iyi duruma getirme [Apache HBase](https://hbase.apache.org/). Phoenix, ilişkisel bir veri modeli üzerine taramalar gibi HBase işlemlerini SQL sorguları dönüştürür HBase oluşturur. Tablo şemanızı, seçim ve birincil anahtarınızı ve tüm dizinleri kullanımınız alanların sıralamasını tasarım, Phoenix performansı etkiler.

## <a name="table-schema-design"></a>Tablo şema tasarımı

Phoenix bir tablo oluşturduğunuzda, bu tablo bir HBase tablosunda depolanır. HBase tablosuyla birlikte erişilen sütunları (sütun ailesi) gruplarını içerir. Phoenix tablosunda bir satıra burada her satır bir veya daha fazla sütunu ile ilişkili tutulan hücreleri oluşan HBase tablosundaki bir satır var. Mantıksal olarak, tek bir HBase satır her rowkey aynı değere sahip bir anahtar-değer çiftleri koleksiyonudur. Diğer bir deyişle, her anahtar-değer çifti rowkey özniteliği vardır ve bu rowkey özniteliğinin değeri belirli bir satır için aynıdır.

Şema tasarım Phoenix tablonun birincil içerir tasarım, sütun ailesi tasarımı, tek tek sütun tasarım ve verilerin nasıl bölümlendiğini anahtar.

### <a name="primary-key-design"></a>Birincil anahtar tasarım

Phoenix tablosunda tanımlı bir birincil anahtar, temel alınan HBase tablo rowkey içinde verilerin depolanma şeklini belirler. Hbase'de, belirli bir satıra erişme girişiminde tek ile rowkey yoludur. Ayrıca, bir HBase tablosu veri rowkey göre sıralanır. Phoenix, her bir satırın birincil anahtarı tanımlandıkları sırayla sütunlardaki değerleri ile birleştirerek rowkey değer oluşturur.

Örneğin, bir tablo kişiler için ad, son adı, telefon numarası ve tümünü aynı sütun ailesinde adresi vardır. Artan bir dizisi sayısına göre bir birincil anahtar tanımlayabilirsiniz:

|rowkey|       adres|   telefon| FirstName| Soyadı|
|------|--------------------|--------------|-------------|--------------|
|  1000|1111 San Gabriel Dr.|1-425-000-0002|    John|Dole|
|  8396|5415 San Gabriel Dr.|1-230-555-0191|  Calvin|Raji|

Sık tarafından lastName sorgularsanız her sorgu her lastName değerini okumak için bir tam tablo taraması gerektirdiğinden ancak, bu birincil anahtar de çalışmayabilir. Bunun yerine, birincil anahtar lastName, firstName ve sosyal güvenlik numarası sütunları olarak tanımlayabilirsiniz. Bu son aynı adresten bir Baba ve son gibi aynı ada sahip iki vatandaşlar ayırt etmek için bir sütundur.

|rowkey|       adres|   telefon| FirstName| Soyadı| socialSecurityNum |
|------|--------------------|--------------|-------------|--------------| ---|
|  1000|1111 San Gabriel Dr.|1-425-000-0002|    John|Dole| 111 |
|  8396|5415 San Gabriel Dr.|1-230-555-0191|  Calvin|Raji| 222 |

Bu yeni birincil anahtarla satır Phoenix tarafından oluşturulan anahtarları olacaktır:

|rowkey|       adres|   telefon| FirstName| Soyadı| socialSecurityNum |
|------|--------------------|--------------|-------------|--------------| ---|
|  Dole John 111|1111 San Gabriel Dr.|1-425-000-0002|    John|Dole| 111 |
|  Raji Calvin 222|5415 San Gabriel Dr.|1-230-555-0191|  Calvin|Raji| 222 |

İlk satırda yukarıdaki rowkey için veriler gösterildiği gibi gösterilir:

|rowkey|       anahtar|   value| 
|------|--------------------|---|
|  Dole John 111|adres |1111 San Gabriel Dr.|  
|  Dole John 111|telefon |1-425-000-0002|  
|  Dole John 111|FirstName |John|  
|  Dole John 111|Soyadı |Dole|  
|  Dole John 111|socialSecurityNum |111| 

Bu rowkey artık verilerin bir kopyasını depolar. Bu değeri temel HBase tablosundaki her bir hücreyle dahil olduğundan boyutu ve birincil anahtarınızı içeren sütun sayısını göz önünde bulundurun.

Ayrıca, birincil anahtar tekdüze artırıyorsunuz değerlere sahipse, tablosu oluşturmanız gerekir *demet salt* yazma sıcak oluşturmaktan kaçının Yardım - görmek için [verileri bölümleme](#partition-data).

### <a name="column-family-design"></a>Sütun ailesi tasarımı

Bazı sütunlarda diğerlerinden daha sık erişilen, sık erişilen sütunları nadiren erişilen sütunları ayırmak için birden çok sütun ailesi oluşturmanız gerekir.

Ayrıca, bazı sütunlar birlikte erişilme eğilimi varsa, bu sütunların aynı sütun ailesinde yerleştirin.

### <a name="column-design"></a>Sütun tasarımı

* Altında yaklaşık 1 MB g/ç maliyetleri nedeniyle büyük sütunları VARCHAR sütunları tutun. Sorguları işlerken HBase tam hücrelerde üzerinden istemciye göndermeden önce gerçekleştiren ve istemcinin bunları tam olarak bunları devre dışı uygulama koduna teslim etmeden önce alır.
* Sütun değerleri kompakt bir biçime protobuf, Avro, msgpack veya BSON gibi kullanarak Store. Daha büyük olduğu gibi JSON önerilmez.
* Gecikme süresi ve g/ç maliyetleri kesmek için depolama önce veri sıkıştırma göz önünde bulundurun.

### <a name="partition-data"></a>Verileri bölümleme

Phoenix, okuma/yazma performansını önemli ölçüde artırabilir verilerinizi dağıtıldığı bölge sayısı denetlemenize olanak tanıyor. Phoenix tablo oluştururken salt veya verilerinizi önceden bölün.

Bir tablo oluşturma sırasında salt için salt demet sayısını belirtin:

    CREATE TABLE CONTACTS (...) SALT_BUCKETS = 16

Bu katarak tablonun birincil anahtar değerlerini otomatik olarak seçmek, değerleri boyunca böler. 

Tablo bölmelerini nerede meydana denetlemek için tablonun bölme işleminin gerçekleştiği aralık değerleri sağlayarak önceden bölebilirsiniz. Örneğin, bir tablo oluşturmak için üç bölgeleri bölme:

    CREATE TABLE CONTACTS (...) SPLIT ON ('CS','EU','NA')

## <a name="index-design"></a>Dizin tasarımı

Phoenix bazılarını veya tümünü dizinlenmiş tablosundaki verilerin bir kopyasını depolayan bir HBase tablosu dizinidir. Bir dizin belirli sorgu türleri için performansı artırır.

Tanımlanan birden fazla dizine sahip ve ardından bir tabloyu sorgulamak, Phoenix sorgu için en iyi dizini otomatik olarak seçer. Birincil dizin seçtiğiniz birincil anahtarlar üzerinde göre otomatik olarak oluşturulur.

Beklenen sorguları için kendilerine ait sütunların belirterek de ikincil dizin oluşturabilirsiniz.

Dizinlerinizi tasarlarken:

* Yalnızca gereksinim duyduğunuz dizin oluşturun.
* Sık güncelleştirilen tablolarda dizinler sayısını sınırlayın. Güncelleştirmeleri bir tablo için yazar ana tablo hem dizin tabloları küçültmesini.

## <a name="create-secondary-indexes"></a>İkincil dizinler oluşturma

İkincil dizinler, hangi depolama alanı, bir noktası arama içine tam tablo taraması olması ve yazma hızı kapatarak okuma performansı artırabilir. İkincil dizinler eklenebilir veya tablo oluşturulduktan sonra kaldırılır ve mevcut sorguları değişiklikleri gerektirmeyen – yalnızca sorguları daha hızlı çalışır. Gereksinimlerinize bağlı olarak, kapsanan dizinleri, işlevsel dizinlere veya her ikisini de oluşturma göz önünde bulundurun.

### <a name="use-covered-indexes"></a>Kapsanan dizinleri kullanın

Kapsanan dizinleri dizinlenir değerlere ek olarak satırdaki verileri içeren dizinler var. İstenen dizin girişi bulduktan sonra birincil tablo erişmeye gerek yoktur.

Örneğin, örnekte socialSecurityNum sütunu yalnızca ikincil dizin oluşturabilirsiniz tablo başvurun. Bu ikincil dizin socialSecurityNum değerlere göre filtre sorguları hızlandırmak, ancak diğer alan değerlerini alma başka bir ana tablo karşı okuma gerektirir.

|rowkey|       adres|   telefon| FirstName| Soyadı| socialSecurityNum |
|------|--------------------|--------------|-------------|--------------| ---|
|  Dole John 111|1111 San Gabriel Dr.|1-425-000-0002|    John|Dole| 111 |
|  Raji Calvin 222|5415 San Gabriel Dr.|1-230-555-0191|  Calvin|Raji| 222 |

Ancak, genellikle firstName ve lastName socialSecurityNum verilen ara istiyorsanız, firstName ve lastName olarak dizin tablosundaki gerçek verileri içeren kapsanan bir dizin oluşturabilirsiniz:

    CREATE INDEX ssn_idx ON CONTACTS (socialSecurityNum) INCLUDE(firstName, lastName);

Kapsanan bu dizini yalnızca ikincil dizin içeren tablodan okuyarak tüm verileri almak için aşağıdaki sorguyu sağlar:

    SELECT socialSecurityNum, firstName, lastName FROM CONTACTS WHERE socialSecurityNum > 100;

### <a name="use-functional-indexes"></a>İşlevsel dizinleri kullanın

İşlevsel dizinleri sorgularında kullanılmasını beklediğiniz rastgele bir ifade üzerinde dizin oluşturmanıza imkan tanır. İşlevsel bir dizin altyapınız var ve bir sorgu ifade kullanır. sonra dizin veri tablosu yerine sonuçları almak için kullanılabilir.

Örneğin, bir kişinin soyadı ve birleşik ilk adı büyük küçük harf duyarsız arama yapmak, izin vermek için bir dizin oluşturabilirsiniz:

     CREATE INDEX FULLNAME_UPPER_IDX ON "Contacts" (UPPER("firstName"||' '||"lastName"));

## <a name="query-design"></a>Sorgu Tasarım

Sorgu Tasarım ana hususlardır:

* Sorgu planını anlayın ve beklenen davranışını doğrulayın.
* Verimli bir şekilde katılın.

### <a name="understand-the-query-plan"></a>Sorgu planını anlayın

İçinde [SQLLine](http://sqlline.sourceforge.net/), SQL sorgunuzun ardından açıklama planı Phoenix gerçekleştireceği işlemleri görüntülemek için kullanın. Bu maddeyi planı:

* Uygun olduğunda birincil anahtarı kullanır.
* Veri tablosu yerine ikincil dizinler kullandığı uygun.
* Tarama ARALIĞI Atla tarama mümkün olduğunda yerine veya tablo taraması kullanır.

#### <a name="plan-examples"></a>Plan örnekleri

Örneğin, uçuş gecikme bilgilerini depolayan UÇUŞLAR adlı bir tablonuz varsayalım.

Bir airlineid ile tüm uçuşlar seçilecek `19805`burada airlineid birincil anahtar veya herhangi bir dizinde olan bir alan adıdır:

    select * from "FLIGHTS" where airlineid = '19805';

Açıklama komutunu aşağıdaki gibi çalıştırın:

    explain select * from "FLIGHTS" where airlineid = '19805';

Sorgu planı şöyle görünür:

    CLIENT 1-CHUNK PARALLEL 1-WAY ROUND ROBIN FULL SCAN OVER FLIGHTS
        SERVER FILTER BY AIRLINEID = '19805'

Bu planda UÇUŞLAR üzerinde tam tarama tümcecik unutmayın. Bu ifade, daha verimli ARALIĞI tarama veya tarama Atla seçeneğini kullanmak yerine tablonun tüm satırlarda bir tablo tarama yürütme yaptığını gösterir.

Şimdi, uçuşlar için 2 Ocak 2014'te taşıyıcısı sorgulamak istediğiniz varsayalım `AA` kendi flightnum olduğu 1'den büyük. Sütunları yıl, ay, dayofmonth, taşıyıcı ve flightnum örnek tablosunda ve tüm bileşik birincil anahtarın parçası olan varsayalım. Sorguyu şu şekilde görünür:

    select * from "FLIGHTS" where year = 2014 and month = 1 and dayofmonth = 2 and carrier = 'AA' and flightnum > 1;

Bu sorguyla planlama inceleyelim:

    explain select * from "FLIGHTS" where year = 2014 and month = 1 and dayofmonth = 2 and carrier = 'AA' and flightnum > 1;

Sonuçta elde edilen planıdır:

    CLIENT 1-CHUNK PARALLEL 1-WAY ROUND ROBIN RANGE SCAN OVER FLIGHTS [2014,1,2,'AA',2] - [2014,1,2,'AA',*]

Birincil anahtarlar için değer aralığını köşeli ayraç değerlerdir. Bu durumda, aralık değerleri 2014 yılının, aylık 1 ve 2 dayofmonth ile sabittir, ancak flightnum 2 ve başlatma için değere izin ver (`*`). Bu sorgu planı, birincil anahtar beklendiği gibi kullanılmakta olduğunu doğrular.

Ardından, adlı UÇUŞLAR tablosunda dizin oluşturma `carrier2_idx` yalnızca taşıyıcı alanı olmasıdır. Bu dizin de flightdate, tailnum, kaynak ve flightnum verisini ayrıca dizinde depolanan kapsanan sütunlar olarak içerir.

    CREATE INDEX carrier2_idx ON FLIGHTS (carrier) INCLUDE(FLIGHTDATE,TAILNUM,ORIGIN,FLIGHTNUM);

Taşıyıcı yanı sıra flightdate ve aşağıdaki sorguyu olduğu gibi tailnum almak istediğinizi varsayalım:

    explain select carrier,flightdate,tailnum from "FLIGHTS" where carrier = 'AA';

Bu dizin kullanılan görmeniz gerekir:

    CLIENT 1-CHUNK PARALLEL 1-WAY ROUND ROBIN RANGE SCAN OVER CARRIER2_IDX ['AA']

Görünebilen öğelerinin tam listesi için planı sonuçlarını açıklar, planları açıklayan bölümüne bakın [Apache Phoenix ayarlama Kılavuzu](https://phoenix.apache.org/tuning_guide.html).

### <a name="join-efficiently"></a>Verimli bir şekilde katılın

Genellikle, özellikle sık kullanılan sorgular, bir tarafındaki küçüktür sürece birleştirmeler önlemek istersiniz.

Gerekirse, büyük birleştirmeler ile bunu yapabilirsiniz, `/*+ USE_SORT_MERGE_JOIN */` ipucu, ancak büyük bir birleştirme çok sayıda satırı pahalı bir işlem değildir. Kullanılabilir bellek sağ taraftaki sorgularınızdan toplam boyutunu aşacak kullanırsanız `/*+ NO_STAR_JOIN */` ipucu.

## <a name="scenarios"></a>Senaryolar

Aşağıdaki yönergeler, bazı ortak desenleri açıklar.

### <a name="read-heavy-workloads"></a>Okuma yoğunluklu iş yükleri

Okuma yoğunluklu için kullanım örnekleri, dizinleri kullandığınızdan emin olun. Ayrıca, ek yükü okuma zaman kazanmak için kapsanan dizin oluşturmayı düşünün.

### <a name="write-heavy-workloads"></a>Yazma yoğunluklu iş yükleri

Birincil anahtarı tekdüze burada genişliyorsa yazma yoğunluklu iş yükleri için gereken ek taramalar nedeniyle genel okuma verimini çoğaltamaz yazma sıcak önlemeye yardımcı olmak için salt demet oluşturun. Ayrıca, UPSERT, çok sayıda kayıtları yazmak için kullanırken otomatik yürütme ve batch kayıtlarını kapatın.

### <a name="bulk-deletes"></a>Toplu silme

Böylece istemci, tüm silinen satırlar için satır anahtarı unutmayın gerekmez. büyük bir veri kümesi silinirken, otomatik yürütme üzerinde silme sorgusu vermeden önce kapatın. Otomatik yürütme, Phoenix bunları doğrudan istemciye döndürme gerekmeksizin bölge sunucuları üzerinde silebilirsiniz tarafından silme, etkilenen satırları ara belleğe alma istemci engeller.

### <a name="immutable-and-append-only"></a>Sabittir ve salt ekleme

Senaryonuz yazma hızı üzerinde veri bütünlüğünü korur, yazma önceden yazılan günlük tablolarınızı oluştururken devre dışı bırakma göz önünde bulundurun:

    CREATE TABLE CONTACTS (...) DISABLE_WAL=true;

Bu ve diğer seçenekleri hakkında daha fazla bilgi için bkz: [Apache Phoenix Dilbilgisi](https://phoenix.apache.org/language/index.html#options).

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Phoenix ayarlama Kılavuzu](https://phoenix.apache.org/tuning_guide.html)
* [İkincil dizinler](https://phoenix.apache.org/secondary_indexing.html)
