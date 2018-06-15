---
title: Azure hdınsight'ta Phoenix performans | Microsoft Docs
description: Phoenix performansını iyileştirmek için en iyi yöntemler.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: ashishth
ms.openlocfilehash: b4c1e3fb919ab9ad88a15b51a5e204290a7a12cf
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34164644"
---
# <a name="phoenix-performance-best-practices"></a>Phoenix performansı için en iyi yöntemler

En önemli Phoenix performans temel HBase en iyi duruma getirme yönüdür. Phoenix taramaları gibi HBase işlemleri SQL sorguları dönüştürür HBase üzerinde ilişkisel veri modeli oluşturur. Tablo şemasını, seçim ve birincil anahtarınızı ve tüm dizinleri kullanımınız alanlara sıralamasını tasarımını Phoenix performansı etkiler.

## <a name="table-schema-design"></a>Tablo şemasını tasarımı

Phoenix bir tablo oluşturduğunuzda, bu tablo bir HBase tablosunda depolanır. HBase tablosuyla birlikte erişilen (sütun ailesi) sütunların gruplarını içerir. Phoenix tablosunda bir satırı, burada her satır bir veya daha fazla sütun ile ilişkili sürümü tutulan hücreleri oluşur HBase tablosundaki satırıdır. Mantıksal olarak, tek bir HBase satır her aynı rowkey değerine sahip bir anahtar-değer çiftleri koleksiyonudur. Diğer bir deyişle, her anahtar-değer çifti bir rowkey özniteliği var ve bu rowkey özniteliğin değerini belirli bir satır için aynıdır.

Phoenix tablo şema tasarımına birincil içeren anahtar tasarım, sütun ailesi tasarımı, tek tek sütun tasarımı ve nasıl verileri bölümlenen.

### <a name="primary-key-design"></a>Birincil anahtar tasarım

Phoenix tablosunda tanımlı birincil anahtarı temel HBase tablo rowkey içinde verilerin depolanma şeklini belirler. HBase, belirli bir satır erişmek için tek ile rowkey yoludur. Ayrıca, bir HBase tablosunda depolanan verileri rowkey göre sıralanır. Phoenix, her satırda birincil anahtarda tanımlanma sıraları sütunların değerlerini birleştirerek rowkey değer oluşturur.

Örneğin, kişiler için bir tablo adı, son adı, telefon numarasını ve adresi, hepsinin aynı sütun ailesi sahiptir. Artan bir sıra numarasına dayanan bir birincil anahtar tanımlayabilirsiniz:

|rowkey|       Adres|   telefon| FirstName| Soyadı|
|------|--------------------|--------------|-------------|--------------|
|  1000|1111 San GABRIEL Dr.|1-425-000-0002|    John|Dole|
|  8396|5415 San GABRIEL Dr.|1-230-555-0191|  Calvin|Raji|

Soyadı tarafından sık Query her sorgu her lastName değerini okumaya tam tablo taraması gerektirdiğinden ancak, bu birincil anahtar da çalışmayabilir. Bunun yerine, birincil anahtar lastName, firstName ve sosyal güvenlik numarası sütunları tanımlayabilirsiniz. Bu son aynı adresinde bir öğe ve son gibi aynı ada sahip iki Satışlar belirsizliğini ortadan kaldırmak için bir sütundur.

|rowkey|       Adres|   telefon| FirstName| Soyadı| socialSecurityNum |
|------|--------------------|--------------|-------------|--------------| ---|
|  1000|1111 San GABRIEL Dr.|1-425-000-0002|    John|Dole| 111 |
|  8396|5415 San GABRIEL Dr.|1-230-555-0191|  Calvin|Raji| 222 |

Bu yeni birincil anahtar satır ile Phoenix tarafından üretilen anahtarlar olacaktır:

|rowkey|       Adres|   telefon| FirstName| Soyadı| socialSecurityNum |
|------|--------------------|--------------|-------------|--------------| ---|
|  Dole John 111|1111 San GABRIEL Dr.|1-425-000-0002|    John|Dole| 111 |
|  Raji Calvin 222|5415 San GABRIEL Dr.|1-230-555-0191|  Calvin|Raji| 222 |

İlk satırının yukarıdaki rowkey verileri gösterildiği gibi gösterilir:

|rowkey|       anahtar|   değer| 
|------|--------------------|---|
|  Dole John 111|Adres |1111 San GABRIEL Dr.|  
|  Dole John 111|telefon |1-425-000-0002|  
|  Dole John 111|FirstName |John|  
|  Dole John 111|Soyadı |Dole|  
|  Dole John 111|socialSecurityNum |111| 

Bu rowkey şimdi bir kopyasını verileri depolar. Bu değer temel HBase tablodaki her hücre birlikte olduğundan birincil anahtarınızı içeren sütun sayısı ve boyutu göz önünde bulundurun.

Ayrıca, birincil anahtar düz olarak artan değerlere sahipse, tabloyla oluşturmalısınız *demet salt* yazma etkin oluşturmamak Yardım - görmek için [bölüm veri](#partition-data).

### <a name="column-family-design"></a>Sütun ailesi tasarımı

Bazı sütunları diğerlerinden daha sık erişilen, sık erişilen sütunları seyrek erişilen sütunlarından ayırmak için birden çok sütun ailesi oluşturmanız gerekir.

Ayrıca, bazı sütunlar birlikte erişilecek eğilimi varsa, bu sütunları aynı sütun ailesi yerleştirin.

### <a name="column-design"></a>Sütun tasarımı

* VARCHAR sütunları yaklaşık 1 MB g/ç maliyetleri nedeniyle büyük sütunları altında tutun. Sorguları işlerken HBase tam hücrelerde üzerinden istemciye göndermeden önce gerçeğe ve istemcinin bunları tam olarak bunları devre dışı uygulama kodu teslim etmeden önce alır.
* Sıkıştırılmış biçimde protobuf, Avro, msgpack veya BSON gibi kullanarak sütun değerlerini depolar. Daha büyük olduğu gibi JSON önerilmez.
* Gecikme süresi ve g/ç maliyetleri kesme depolama önce verileri sıkıştırmayı göz önünde bulundurun.

### <a name="partition-data"></a>Verileri bölümleme

Phoenix okuma/yazma performansı önemli ölçüde artırabilir, verilerinizi dağıtılacağı yeri, bölge sayısı denetlemenize olanak sağlar. Phoenix tablo oluştururken salt veya verilerinizi önceden bölün.

Bir tablo oluşturma sırasında salt için salt sayısını belirtin:

    CREATE TABLE CONTACTS (...) SALT_BUCKETS = 16

Bu katarak tablonun birincil anahtarlar, değerleri otomatik olarak seçme değerlerini boyunca böler. 

Tablo bölmelerini gerçekleştiği denetlemek için tablonun bölme işleminin gerçekleştiği aralık değerleri sağlayarak önceden bölebilirsiniz. Örneğin, bir tablo oluşturmak için üç bölgeler böl:

    CREATE TABLE CONTACTS (...) SPLIT ON ('CS','EU','NA')

## <a name="index-design"></a>Dizin tasarımı

Phoenix bazılarını veya tümünü dizin oluşturulmuş tabloya verilerden bir kopyasını depolayan bir HBase tablosu dizinidir. Bir dizin belirli sorgu türleri için performansı geliştirir.

Tanımlanan birden fazla dizine sahip ve bir tablo sorgu Phoenix sorgu için en iyi dizini otomatik olarak seçer. Birincil dizin seçtiğiniz birincil tuşlar göre otomatik olarak oluşturulur.

Beklenen sorgular için bunların sütunlarını belirterek ayrıca ikincil dizinler oluşturabilirsiniz.

Dizinlerinizi tasarlarken:

* Yalnızca gereksinim duyduğunuz dizinler oluşturun.
* Sık güncelleştirilen tablolardaki dizinler sayısını sınırlayın. Ana Tablo ve dizin tablolar yazma işlemlerini içine bir tabloya güncelleştirmeleri çevir.

## <a name="create-secondary-indexes"></a>İkincil dizinler oluşturma

İkincil dizinler ne depolama alanı, bir noktası arama içine tam tablo taraması olması ve yazma hızı kapatarak okuma performansı artırabilir. İkincil dizinler eklenebilir veya tablo oluşturulduktan sonra kaldırılır ve varolan sorguları değişiklikler gerektirmeyen – sorgular yalnızca daha hızlı çalışır. Gereksinimlerinize bağlı olarak, kapsanan dizinleri, işlevsel dizinler veya her ikisini oluşturmayı düşünün.

### <a name="use-covered-indexes"></a>Kapsanan dizinleri kullanın

Kapsanan dizinleri satırın dizini değerleri ek verileri dahil dizinler ' dir. İstenen dizin girişi bulduktan sonra birincil tabloya erişim için gerek yoktur.

Örneğin, örnekte yalnızca socialSecurityNum sütunu ikincil bir dizin oluşturabilirsiniz tablo başvurun. Bu ikincil dizini socialSecurityNum değerlerine göre filtre sorguları hızlandırmak, ancak diğer alan değerlerini alma başka bir ana tablo karşı okuma gerektirir.

|rowkey|       Adres|   telefon| FirstName| Soyadı| socialSecurityNum |
|------|--------------------|--------------|-------------|--------------| ---|
|  Dole John 111|1111 San GABRIEL Dr.|1-425-000-0002|    John|Dole| 111 |
|  Raji Calvin 222|5415 San GABRIEL Dr.|1-230-555-0191|  Calvin|Raji| 222 |

Ancak, genellikle firstName ve lastName socialSecurityNum verilen aramak istiyorsanız, dizin tablosunda gerçek veri olarak firstName ve lastName içerir kapsanan bir dizin oluşturabilirsiniz:

    CREATE INDEX ssn_idx ON CONTACTS (socialSecurityNum) INCLUDE(firstName, lastName);

Bu kapsanan dizini ikincil dizin içeren tablo okunurken tarafından yalnızca tüm verileri almak için aşağıdaki sorguyu sağlar:

    SELECT socialSecurityNum, firstName, lastName FROM CONTACTS WHERE socialSecurityNum > 100;

### <a name="use-functional-indexes"></a>İşlev dizinleri kullanın

İşlev dizinler, dizin sorguları kullanılacak beklediğiniz rasgele bir ifade oluşturmak izin verir. Bir işlev dizini kullanıyor ve bir sorgu ifade kullanır sonra dizini veri tablosu yerine sonuçları almak için kullanılabilir.

Örneğin, birleştirilmiş ilk ad ve Soyadı, bir kişinin büyük küçük harf duyarsız arama yapmanıza izin verdiği için bir dizin oluşturabilirsiniz:

     CREATE INDEX FULLNAME_UPPER_IDX ON "Contacts" (UPPER("firstName"||' '||"lastName"));

## <a name="query-design"></a>Sorgu tasarımı

Sorgu Tasarım ana dikkat edilmesi gerekenler şunlardır:

* Sorgu planı anlamak ve beklenen davranışını doğrulayın.
* Verimli bir şekilde katılın.

### <a name="understand-the-query-plan"></a>Sorgu planı anlama

İçinde [SQLLine](http://sqlline.sourceforge.net/), SQL sorgusu tarafından izlenen AÇIKLA Phoenix gerçekleştirecek işlemleri planını görüntülemek için kullanın. Denetleyin planı:

* Uygun olduğunda, birincil anahtarı kullanır.
* Veri tablosu yerine ikincil dizinler kullanır uygun.
* Aralık tarama Atla tarama mümkün olduğunda yerine veya tablo taraması kullanır.

#### <a name="plan-examples"></a>Planı örnekleri

Örneğin, uçuş gecikme bilgilerini depolayan UÇUŞLAR adlı bir tablo olduğunu varsayalım.

Bir airlineid ile tüm uçuşları seçmek için `19805`airlineid birincil anahtar veya bir dizin olan bir alan eder:

    select * from "FLIGHTS" where airlineid = '19805';

Açıklama komut aşağıdaki gibi çalıştırın:

    explain select * from "FLIGHTS" where airlineid = '19805';

Sorgu planı şöyle görünür:

    CLIENT 1-CHUNK PARALLEL 1-WAY ROUND ROBIN FULL SCAN OVER FLIGHTS
        SERVER FILTER BY AIRLINEID = '19805'

Bu planda UÇUŞLAR üzerinden tam tarama tümcecik unutmayın. Bu tümcecik daha verimli ARALIĞI tarama veya Atla Tarama seçeneğini kullanmak yerine tablonun tüm satırlarda üzerinden tablo tarama yürütme mu gösterir.

Şimdi, uçuşlar için 2 Ocak 2014'te taşıyıcı için sorgulamak istediğiniz söyleyin `AA` kendi flightnum bulunduğu 1'den büyük. Sütunları yıl, ay, dayofmonth, taşıyıcı ve flightnum örnek tablosunda ve tüm bileşik birincil anahtarın parçası olan varsayalım. Sorgu aşağıdaki gibi görünür:

    select * from "FLIGHTS" where year = 2014 and month = 1 and dayofmonth = 2 and carrier = 'AA' and flightnum > 1;

Bu sorguyla planlama inceleyelim:

    explain select * from "FLIGHTS" where year = 2014 and month = 1 and dayofmonth = 2 and carrier = 'AA' and flightnum > 1;

Sonuçta elde edilen planı aşağıdaki gibidir:

    CLIENT 1-CHUNK PARALLEL 1-WAY ROUND ROBIN RANGE SCAN OVER FLIGHTS [2014,1,2,'AA',2] - [2014,1,2,'AA',*]

Köşeli ayraçlar içinde birincil anahtarlar için değer aralığını değerlerdir. Bu durumda, aralık değerleri 2014 yıl, ay 1 ve dayofmonth 2 ile giderilen, ancak değerler 2 ve üzerinde flightnum başlatılıyor için izin (`*`). Bu sorgu planı, birincil anahtar beklendiği gibi kullanılmakta olduğunu doğrular.

Ardından, dizin adlı UÇUŞLAR tablo oluşturma `carrier2_idx` yalnızca taşıyıcı alanı olmasıdır. Bu dizin ayrıca flightdate, tailnum, kaynak ve flightnum verisini ayrıca dizinde depolanır kapsanan sütunlar içerir.

    CREATE INDEX carrier2_idx ON FLIGHTS (carrier) INCLUDE(FLIGHTDATE,TAILNUM,ORIGIN,FLIGHTNUM);

Aşağıdaki sorgu olduğu gibi tailnum ve flightdate birlikte taşıyıcı almak istediğinizi varsayalım:

    explain select carrier,flightdate,tailnum from "FLIGHTS" where carrier = 'AA';

Kullanılan bu dizini görmeniz gerekir:

    CLIENT 1-CHUNK PARALLEL 1-WAY ROUND ROBIN RANGE SCAN OVER CARRIER2_IDX ['AA']

Görüntülenebilir öğelerinin tam listesi için plan sonuçları açıklayan, açıklayan planları bölümüne bakın [Apache Phoenix ayarlama Kılavuzu](https://phoenix.apache.org/tuning_guide.html).

### <a name="join-efficiently"></a>Verimli bir şekilde katılma

Genellikle, sütunlardan özellikle sık sorgulamaları küçük olmadığı sürece birleştirmeler önlemek istiyor.

Gerekirse, büyük birleştirmeler ile yapabileceğiniz varsa `/*+ USE_SORT_MERGE_JOIN */` ipucu ancak büyük birleştirme büyük sayıda satırı pahalı bir işlem değildir. Kullanılabilir bellek tüm sağ taraftaki hand tabloları toplam boyutunu aşabilir kullanırsanız `/*+ NO_STAR_JOIN */` ipucu.

## <a name="scenarios"></a>Senaryolar

Aşağıdaki yönergeler, bazı ortak desenler açıklanmaktadır.

### <a name="read-heavy-workloads"></a>Okuma yoğun iş yükleri

Read-ağır için kullanım örnekleri, dizinleri kullandığınızdan emin olun. Ayrıca, ek yükü okuma zaman kazanmak için kapsanan dizin oluşturmayı düşünün.

### <a name="write-heavy-workloads"></a>Yazma yoğun iş yükleri

Burada birincil anahtar düz olarak artan yazma yoğun iş yükleri için gereken ek taramaları nedeniyle genel okuma verimini ödün verme pahasına yazma etkin önlemeye yardımcı olmak için salt demet oluşturun. Ayrıca, çok sayıda kayıt yazmak için UPSERT kullanırken otomatik kayıt işlemleri ve kayıtları toplu işi devre dışı.

### <a name="bulk-deletes"></a>Toplu silme

Böylece istemci, tüm silinen satır satır tuşları unutmayın gerekmez. büyük bir veri kümesi silerken, otomatik kayıt işlemleri üzerinde silme sorgusu vermeden önce kapatın. Otomatik kayıt işlemleri, o Phoenix bunları doğrudan istemciye döndürme gerekmeksizin bölge sunucularda silebilirsiniz şekilde DELETE tarafından etkilenen satırlar arabelleğe alma istemci engeller.

### <a name="immutable-and-append-only"></a>Sabit ve yalnızca ekleme

Senaryonuz yazma hızı veri bütünlüğünü korur, yazma tamamlanan günlük tablolarınızı oluştururken devre dışı bırakma göz önünde bulundurun:

    CREATE TABLE CONTACTS (...) DISABLE_WAL=true;

Bu ve diğer seçenekleri hakkında daha fazla bilgi için bkz: [Phoenix Dilbilgisi](http://phoenix.apache.org/language/index.html#options).

## <a name="next-steps"></a>Sonraki adımlar

* [Phoenix ayarlama Kılavuzu](https://phoenix.apache.org/tuning_guide.html)
* [İkincil dizinler](http://phoenix.apache.org/secondary_indexing.html)
