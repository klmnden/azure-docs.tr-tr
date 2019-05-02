---
title: Azure Data Factory veri akışı eşleme özelliği, ifade işlevleri
description: Veri akışı eşleme ifade işlevleri hakkında bilgi edinin.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/15/2019
ms.openlocfilehash: 6b4df70114dd481566ae1a666c91c1c9b5bad86f
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64717004"
---
# <a name="data-transformation-expressions-in-mapping-data-flow"></a>Veri dönüştürme ifadelerinde eşleme veri akışı 

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

## <a name="expression-functions"></a>İfade işlevleri

Data Factory'de veri dönüştürme seçenekleri yapılandırmak için veri akışı eşleme özelliği, ifade dili kullanın.

*********************************
<code>abs</code>
==============================
<code><b>abs(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Bu ifade, bir çift sayı pozitif modulus işaretler.
* ``abs(-20) -> 20``
* ``abs(10) -> 10``
*********************************
<code>acos</code>
==============================
<code><b>acos(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade Kosinüs ters değerini hesaplar.
* ``acos(1) -> 0.0``
*********************************
<code>add</code>
==============================
<code><b>add(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Bu ifade bir dize veya sayı çift ekler. Bir tarih bir gün sayısı ekler. Başka bir dizi benzer bir tür ekler. 
* ``add(10, 20) -> 30``
* ``10 + 20 -> 30``
* ``add('ice', 'cream') -> 'icecream'``
* ``'ice' + 'cream' + ' cone' -> 'icecream cone'``
* ``add(toDate('2012-12-12'), 3) -> 2012-12-15 (date value)``
* ``toDate('2012-12-12') + 3 -> 2012-12-15 (date value)``
* ``[10, 20] + [30, 40] => [10, 20, 30, 40]``

**Ekleme** işleci, aynı **+** işleci.
*********************************
<code>addDays</code>
==============================
<code><b>addDays(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;days to add&gt;</i> : integral) => datetime</b></code><br/><br/>
Bu ifade, bir tarih veya zaman damgası gün ekler. 
* ``addDays(toDate('2016-08-08'), 1) -> 2016-08-09``

**AddDays** işleci, aynı **+** tarihler için işleci.
*********************************
<code>addMonths</code>
==============================
<code><b>addMonths(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;months to add&gt;</i> : integral) => datetime</b></code><br/><br/>
Bu ifade, bir tarih veya zaman damgası ay ekler.
* ``addMonths(toDate('2016-08-31'), 1) -> 2016-09-30``
* ``addMonths(toTimestamp('2016-09-30 10:10:10'), -1) -> 2016-08-31 10:10:10``
*********************************
<code>and</code>
==============================
<code><b>and(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></code><br/><br/>
Bir mantıksal budur **ve** işleci. Bu, aynı **&&** işleci.
* ``and(true, false) -> false``
* ``true && false -> false``
*********************************
<code>asin</code>
==============================
<code><b>asin(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade bir ters sinüs değerini hesaplar.
* ``asin(0) -> 0.0``
*********************************
<code>atan</code>
==============================
<code><b>atan(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade, bir Ters Tanjant değerini hesaplar.
* ``atan(0) -> 0.0``
*********************************
<code>atan2</code>
==============================
<code><b>atan2(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade bir pozitif x ekseni ile koordinatları veren noktası arasında radyan cinsinden açıyı döndürür.
* ``atan2(0, 0) -> 0.0``
*********************************
<code>avg</code>
==============================
<code><b>avg(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Bu ifade, bir sütunun değerlerin ortalamasını alır.
* ``avg(sales) -> 7523420.234``
*********************************
<code>avgIf</code>
==============================
<code><b>avgIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Bu ifade, bir ölçütü temel alan, bir sütunun değerlerin ortalamasını alır.
* ``avgIf(region == 'West', sales) -> 7523420.234``
*********************************
<code>byName</code>
==============================
<code><b>byName(<i>&lt;column name&gt;</i> : string) => any</b></code><br/><br/>
Bu ifade bir sütun değeri akışa ada göre seçer. İlk eşleşme, birden çok eşleşme varsa, döndürülür. İfade, eşleşme yoksa NULL değer döndürür. Döndürülen değerin türü-dönüştürülen bir tür dönüştürme işlevleri (TO_DATE, TO_STRING...) olmalıdır. Tasarım zamanında bilinen sütun adları adlarına göre hemen ilgilenilmesi gerekir. Hesaplanan girişleri desteklenmez, ancak parametresi değişimleri kullanabilirsiniz.

* ``toString(byName('parent')) -> appa``
* ``toLong(byName('income')) -> 9000000000009``
* ``toBoolean(byName('foster')) -> false``
* ``toLong(byName($debtCol)) -> 123456890``
* ``birthDate -> 12/31/2050``
* ``toString(byName('Bogus Column')) -> NULL``
*********************************
<code>byPosition</code>
==============================
<code><b>byPosition(<i>&lt;position&gt;</i> : integer) => any</b></code><br/><br/>
Bu ifade akışta göreli konumunu (1 tabanlı) bir sütun değeri seçer. Sınırların dışında konumu NULL değer döndürür. Tür-dönüştürülen bir tür dönüştürme işlevleri (TO_DATE, TO_STRING ve benzeri) tarafından döndürülen değer olmalıdır. Hesaplanan girişleri desteklenmez, ancak parametresi değişimleri kullanabilirsiniz.

* ``toString(byPosition(1)) -> amma``
* ``toDecimal(byPosition(2), 10, 2) -> 199990.99``
* ``toBoolean(byName(4)) -> false``
* ``toString(byName($colName)) -> family``
* ``toString(byPosition(1234)) -> NULL``
*********************************
<code>case</code>
==============================
<code><b>case(<i>&lt;condition&gt;</i> : boolean, <i>&lt;true_expression&gt;</i> : any, <i>&lt;false_expression&gt;</i> : any, ...) => any</b></code><br/><br/>
Koşullar değişen üzerinde bağlı olarak, bu ifade bir değer veya diğer geçerlidir. 
* ``case(custType == 'Premium', 10, 4.5)``
* ``case(custType == 'Premium', price*0.95, custType == 'Elite',   price*0.9, price*2)``
* ``case(dayOfWeek(saleDate) == 1, 'Sunday', dayOfWeek(saleDate) == 6, 'Saturday')``

Girdi sayısı çift sayı ise, başka bir değer için son koşul NULL olur.
*********************************
<code>cbrt</code>
==============================
<code><b>cbrt(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade, bir sayının küpünü kök hesaplar.
* ``cbrt(8) -> 2.0``
*********************************
<code>ceil</code>
==============================
<code><b>ceil(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Bu ifade, sayıdan küçük değil en küçük tamsayı döndürür.
* ``ceil(-0.1) -> 0``
*********************************
<code>concat</code>
==============================
<code><b>concat(<i>&lt;this&gt;</i> : string, <i>&lt;that&gt;</i> : string, ...) => string</b></code><br/><br/>
Bu ifade bir değişken sayıda dizeyi birleştirir. **Concat** işleci, aynı **+** dizeleriyle işleci.
* ``concat('Awesome', 'Cool', 'Product') -> 'AwesomeCoolProduct'``
* ``'Awesome' + 'Cool' + 'Product' -> 'AwesomeCoolProduct'``
* ``concat(addrLine1, ' ', addrLine2, ' ', city, ' ', state, ' ', zip)``
* ``addrLine1 + ' ' + addrLine2 + ' ' + city + ' ' + state + ' ' + zip``
*********************************
<code>concatWS</code>
==============================
<code><b>concatWS(<i>&lt;separator&gt;</i> : string, <i>&lt;this&gt;</i> : string, <i>&lt;that&gt;</i> : string, ...) => string</b></code><br/><br/>
Bu ifade bir değişken sayıda dizeyi bir ayırıcı ile birlikte art arda ekler. Ayırıcı ilk parametredir.
* ``concatWS(' ', 'Awesome', 'Cool', 'Product') -> 'Awesome Cool Product'``
* ``concatWS(' ' , addrLine1, addrLine2, city, state, zip) ->``
* ``concatWS(',' , toString(order_total), toString(order_discount))``
*********************************
<code>cos</code>
==============================
<code><b>cos(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade Kosinüs değerini hesaplar.
* ``cos(10) -> -0.83907152907``
*********************************
<code>cosh</code>
==============================
<code><b>cosh(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade bir değer bir hiperbolik kosinüsünü hesaplar.
* ``cosh(0) -> 1.0``
*********************************
<code>count</code>
==============================
<code><b>count([<i>&lt;value1&gt;</i> : any]) => long</b></code><br/><br/>
Bu ifade değerleri toplam sayısını alır. İsteğe bağlı bir sütun veya sütunlar ise belirtilen, NULL değerler sayısı göz ardı edilir.
* ``count(custId) -> 100``
* ``count(custId, custName) -> 50``
* ``count() -> 125``
* ``count(iif(isNull(custId), 1, NULL)) -> 5``
*********************************
<code>countDistinct</code>
==============================
<code><b>countDistinct(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : any], ...) => long</b></code><br/><br/>
Bu ifade değerlerinin sütun kümesini sayısını toplam sayısını alır.
* ``countDistinct(custId, custName) -> 60``
*********************************
<code>countIf</code>
==============================
<code><b>countIf(<i>&lt;value1&gt;</i> : boolean, [<i>&lt;value2&gt;</i> : any]) => long</b></code><br/><br/>
Bu ifade ölçütü temel alarak, değerleri toplam sayısını alır. İsteğe bağlı bir sütun ise, belirtilen, NULL değerler sayısı göz ardı edilir.
* ``countIf(state == 'CA' && commission < 10000, name) -> 100``
*********************************
<code>covariancePopulation</code>
==============================
<code><b>covariancePopulation(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade, iki sütun arasında doldurma kovaryansı alır.
* ``covariancePopulation(sales, profit) -> 122.12``
*********************************
<code>covariancePopulationIf</code>
==============================
<code><b>covariancePopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number, <i>&lt;value3&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade, bir ölçütü temel alan, iki sütun doldurma kovaryansı alır.
* ``covariancePopulationIf(region == 'West', sales) -> 122.12``
*********************************
<code>covarianceSample</code>
==============================
<code><b>covarianceSample(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade, iki sütun örnek kovaryansı alır.
* ``covarianceSample(sales, profit) -> 122.12``
*********************************
<code>covarianceSampleIf</code>
==============================
<code><b>covarianceSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number, <i>&lt;value3&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade, bir ölçütü temel alan, iki sütun örnek kovaryansı alır.
* ``covarianceSampleIf(region == 'West', sales, profit) -> 122.12``
*********************************
<code>crc32</code>
==============================
<code><b>crc32(<i>&lt;value1&gt;</i> : any, ...) => long</b></code><br/><br/>
Bu ifade değerleri yalnızca 0(256) olabilir bir bit uzunluğu verilen basit veri türleri, değişen sütun kümesini CRC32 karmasını hesaplar 224, 256, 384, 512. Kullanabileceğiniz **crc32** parmak izi bir satır için hesaplanacak işleci.
* ``crc32(256, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> 3630253689``
*********************************
<code>cumeDist</code>
==============================
<code><b>cumeDist() => integer</b></code><br/><br/>
Bu işlev, bölüm bir değer tüm değerleri göreli konumunu hesaplar. Geçerli satırı penceresi bölüm satır toplam sayısı ile bölünmesiyle bölümü, sıralama, eşit veya önceki satır sayısını sonucudur. Aynı konumda sıralama herhangi KRAVAT değerlerini döndürür.
* ``cumeDist() -> 1``
*********************************
<code>currentDate</code>
==============================
<code><b>currentDate([<i>&lt;value1&gt;</i> : string]) => date</b></code><br/><br/>
Bu ifade, iş çalışmaya başladığında geçerli tarihi alır. 
* ``currentDate() -> 12-12-2030``
* ``currentDate('PST') -> 12-31-2050``

İsteğe bağlı bir saat dilimi biçiminde geçirebilirsiniz `'GMT'`, `'PST'`, `'UTC'`, `'America/Cayman'`. Yerel saat dilimi varsayılandır.
*********************************
<code>currentTimestamp</code>
==============================
<code><b>currentTimestamp() => timestamp</b></code><br/><br/>
İşi yerel saat dilimi ile çalışmaya başladığında bu ifade geçerli zaman damgasını alır.
* ``currentTimestamp() -> 12-12-2030T12:12:12``
*********************************
<code>currentUTC</code>
==============================
<code><b>currentUTC([<i>&lt;value1&gt;</i> : string]) => timestamp</b></code><br/><br/>
Bu ifade geçerli UTC zaman damgası alır. 
* ``currentUTC() -> 12-12-2030T19:18:12``
* ``currentUTC('Asia/Seoul') -> 12-13-2030T11:18:12``

İsteğe bağlı bir saat dilimi biçiminde geçirebilirsiniz `'GMT'`, `'PST'`, `'UTC'`, `'America/Cayman'`. Yerel saat dilimi varsayılandır.
*********************************
<code>dayOfMonth</code>
==============================
<code><b>dayOfMonth(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Bu ifade, bir tarihi göz önünde bulundurulduğunda, ayın gününü alır.
* ``dayOfMonth(toDate('2018-06-08')) -> 08``
*********************************
<code>dayOfWeek</code>
==============================
<code><b>dayOfWeek(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Bu ifade, bir tarihi göz önünde bulundurulduğunda, haftanın gününü alır. 
* ``dayOfWeek(toDate('2018-06-08')) -> 7``

Gün değerleri aşağıdaki gibidir:
- 1 - Pazar
- 2 - Pazartesi 
- ...
- 7 - Cumartesi
*********************************
<code>dayOfYear</code>
==============================
<code><b>dayOfYear(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Bu ifade, bir tarihi göz önünde bulundurulduğunda, yılın gününü alır.
* ``dayOfYear(toDate('2016-04-09')) -> 100``
*********************************
<code>degrees</code>
==============================
<code><b>degrees(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade Radyanı dereceye çevirir.
* ``degrees(3.141592653589793) -> 180``
*********************************
<code>denseRank</code>
==============================
<code><b>denseRank(<i>&lt;value1&gt;</i> : any, ...) => integer</b></code><br/><br/>
Bu ifade, boyut değerlerinin bir gruptaki bir değer sayısını hesaplar. Geçerli satırın bölümünü sıralama, eşit veya önceki satır sayısı artı bir sonucudur. Değerler, sıradaki boşlukları üretir olmaz. 
* ``denseRank(salesQtr, salesAmt) -> 1``

**DenseRank** işleci bile verilerin ne zaman sıralanmamış çalışır. Değerleri bir değişiklik arar.
*********************************
<code>divide</code>
==============================
<code><b>divide(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Bu ifade çift sayıların böler. **Bölmek** işleci, aynı **/** işleci.
* ``divide(20, 10) -> 2``
* ``20 / 10 -> 2``
*********************************
<code>endsWith</code>
==============================
<code><b>endsWith(<i>&lt;string&gt;</i> : string, <i>&lt;substring to check&gt;</i> : string) => boolean</b></code><br/><br/>
Bu ifade, dize verilen dize ile bitip bitmediğini denetler.
* ``endsWith('great', 'eat') -> true``
*********************************
<code>equals</code>
==============================
<code><b>equals(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Karşılaştırma eşittir işleci budur. Aynı olan **==** işleci.
* ``equals(12, 24) -> false``
* ``12==24 -> false``
* ``'bad'=='bad' -> true``
* ``'good'== NULL -> false``
* ``NULL===NULL -> false``
*********************************
<code>equalsIgnoreCase</code>
==============================
<code><b>equalsIgnoreCase(<i>&lt;value1&gt;</i> : string, <i>&lt;value2&gt;</i> : string) => boolean</b></code><br/><br/>
**EqualsIgnoreCase** işleci, küçük büyük harf duyarlı bir karşılaştırma eşittir işleci. Aynı olan **<=>** işleci.
* ``'abc'=='abc' -> true``
* ``equalsIgnoreCase('abc', 'Abc') -> true``
*********************************
<code>factorial</code>
==============================
<code><b>factorial(<i>&lt;value1&gt;</i> : number) => long</b></code><br/><br/>
Bu ifade, bir sayının faktöriyelini hesaplar.
* ``factorial(5) -> 120``
*********************************
<code>false</code>
==============================
<code><b>false() => boolean</b></code><br/><br/>
Bu ifade her zaman false değeri döndürür. 
* ``isDiscounted == false()``
* ``isDiscounted() == false``

Kullanım `syntax(false())` bir sütun ise işlev *false*.
*********************************
<code>first</code>
==============================
<code><b>first(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : boolean]) => any</b></code><br/><br/>
Bu ifade, sütun grubu ilk değerini alır. İkinci parametresini atlarsanız, `ignoreNulls`, false olarak kabul edilir.
* ``first(sales) -> 12233.23``
* ``first(sales, false) -> NULL``
*********************************
<code>floor</code>
==============================
<code><b>floor(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Bu ifade, sayıdan büyük olmayan en büyük tamsayı döndürür.
* ``floor(-0.1) -> -1``
*********************************
<code>fromUTC</code>
==============================
<code><b>fromUTC(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => timestamp</b></code><br/><br/>
Bu ifade için zaman damgasını UTC'den dönüştürür. İsteğe bağlı olarak saat dilimi biçiminde geçirebilirsiniz `'GMT'`, `'PST'`, `'UTC'`, `'America/Cayman'`. Geçerli saat dilimi varsayılandır.

* ``fromUTC(currentTimeStamp()) -> 12-12-2030T19:18:12``
* ``fromUTC(currentTimeStamp(), 'Asia/Seoul') -> 12-13-2030T11:18:12``
*********************************
<code>greater</code>
==============================
<code><b>greater(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Bu karşılaştırma, **büyük** işleci. Aynı olan **>** işleci.
* ``greater(12, 24) -> false``
* ``'abcd' > 'abc' -> true``
*********************************
<code>greaterOrEqual</code>
==============================
<code><b>greaterOrEqual(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Karşılaştırma büyük daha-veya-eşittir işleci budur. Bu işleç aynıdır **>=** işleci.
* ``greaterOrEqual(12, 12) -> false``
* ``'abcd' >= 'abc' -> true``
*********************************
<code>greatest</code>
==============================
<code><b>greatest(<i>&lt;value1&gt;</i> : any, ...) => any</b></code><br/><br/>
Bu ifade, en büyük değer değerleri listesinde giriş olarak döndürür. Tüm girişleri NULL ise NULL döndürür.
* ``greatest(10, 30, 15, 20) -> 30``
* ``greatest(toDate('12/12/2010'), toDate('12/12/2011'), toDate('12/12/2000')) -> '12/12/2011'``
*********************************
<code>hour</code>
==============================
<code><b>hour(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Bu ifade, bir zaman damgası saat değerini alır. 
* ``hour(toTimestamp('2009-07-30T12:58:59')) -> 12``
* ``hour(toTimestamp('2009-07-30T12:58:59'), 'PST') -> 12``

İsteğe bağlı bir saat dilimi biçiminde geçirebilirsiniz `'GMT'`, `'PST'`, `'UTC'`, `'America/Cayman'`. Yerel saat dilimi varsayılandır.
*********************************
<code>iif</code>
==============================
<code><b>iif(<i>&lt;condition&gt;</i> : boolean, <i>&lt;true_expression&gt;</i> : any, [<i>&lt;false_expression&gt;</i> : any]) => any</b></code><br/><br/>
Bir koşula bağlı olarak, bu ifade bir değer veya diğer geçerlidir. Başka bir değer belirtilmemişse, NULL olarak kabul edilir. Her iki değer uyumlu olması gerekir (sayısal veya örneğin bir dize).
* ``iif(custType == 'Premium', 10, 4.5)``
* ``iif(amount > 100, 'High')``
* ``iif(dayOfWeek(saleDate) == 6, 'Weekend', 'Weekday')``
*********************************
<code>in</code>
==============================
<code><b>in(<i>&lt;array of items&gt;</i> : array, <i>&lt;item to find&gt;</i> : any) => boolean</b></code><br/><br/>
Bu ifade dizide bir öğe olup olmadığını denetler.
* ``in([10, 20, 30], 10) -> true``
* ``in(['good', 'kid'], 'bad') -> false``
*********************************
<code>initCap</code>
==============================
<code><b>initCap(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Bu ifade her sözcüğün ilk harfini büyük harfe dönüştürür. Sözcükler boşluk ayırmaları tarafından tanımlanır.
* ``initCap('cool iceCREAM') -> 'Cool IceCREAM'``
*********************************
<code>instr</code>
==============================
<code><b>instr(<i>&lt;string&gt;</i> : string, <i>&lt;substring to find&gt;</i> : string) => integer</b></code><br/><br/>
Bu ifade (1 tabanlı) bir dize içinde alt dizenin konumu bulur. Konumu bulunmazsa, 0 döndürülür. 
* ``instr('great', 'eat') -> 3``
* ``instr('microsoft', 'o') -> 7``
* ``instr('good', 'bad') -> 0``
*********************************
<code>isDelete</code>
==============================
<code><b>isDelete([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Bu ifade silme için işaretlenen satırları denetler. 
* ``isDelete() -> true``
* ``isDelete(1) -> false``

Birden fazla giriş akışı dönüşümünüzü aldığı durumlarda, akışın (1 tabanlı) dizini geçirebilirsiniz. Akış dizini için varsayılan değer 1'dir.
*********************************
<code>isError</code>
==============================
<code><b>isError([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Bu ifade, hata olarak işaretli satırlar için denetler.
* ``isError() -> true``
* ``isError(1) -> false``

Birden fazla giriş akışı dönüşümünüzü aldığı durumlarda, akışın (1 tabanlı) dizini geçirebilirsiniz. Akış dizini için varsayılan değer 1'dir.
*********************************
<code>isIgnore</code>
==============================
<code><b>isIgnore([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Bu ifade için yok sayılacak işaretlenen satırları denetler.
* ``isIgnore() -> true``
* ``isIgnore(1) -> false``

Birden fazla giriş akışı dönüşümünüzü aldığı durumlarda, akışın (1 tabanlı) dizini geçirebilirsiniz. Akış dizini için varsayılan değer 1'dir.
*********************************
<code>isInsert</code>
==============================
<code><b>isInsert([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Bu deyim ekleme için işaretlenen satırları denetler. 
* ``isInsert() -> true``
* ``isInsert(1) -> false``

Birden fazla giriş akışı dönüşümünüzü aldığı durumlarda, akışın (1 tabanlı) dizini geçirebilirsiniz. Akış dizini için varsayılan değer 1'dir.
*********************************
<code>isMatch</code>
==============================
<code><b>isMatch([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Bu ifade aramada eşleşen satırları denetler. 
* ``isMatch() -> true``
* ``isMatch(1) -> false``

Birden fazla giriş akışı dönüşümünüzü aldığı durumlarda, akışın (1 tabanlı) dizini geçirebilirsiniz. Akış dizini için varsayılan değer 1'dir.
*********************************
<code>isNull</code>
==============================
<code><b>isNull(<i>&lt;value1&gt;</i> : any) => boolean</b></code><br/><br/>
Bu ifade NULL değerini denetler.
* ``isNull(NULL()) -> true``
* ``isNull('') -> false'``
*********************************
<code>isUpdate</code>
==============================
<code><b>isUpdate([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Bu ifade, güncelleştirme için işaretlenen satırları denetler. 
* ``isUpdate() -> true``
* ``isUpdate(1) -> false``

Birden fazla giriş akışı dönüşümünüzü aldığı durumlarda, akışın (1 tabanlı) dizini geçirebilirsiniz. Akış dizini için varsayılan değer 1'dir.
*********************************
<code>kurtosis</code>
==============================
<code><b>kurtosis(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade, bir sütunun basıklığını alır.
* ``kurtosis(sales) -> 122.12``
*********************************
<code>kurtosisIf</code>
==============================
<code><b>kurtosisIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade bir ölçütü temel alan, bir sütunun basıklığını alır.
* ``kurtosisIf(region == 'West', sales) -> 122.12``
*********************************
<code>lag</code>
==============================
<code><b>lag(<i>&lt;value&gt;</i> : any, [<i>&lt;number of rows to look before&gt;</i> : number], [<i>&lt;default value&gt;</i> : any]) => any</b></code><br/><br/>
Bu ifadenin değerlendirilmesi ilk parametre değerini alır *n* önce geçerli satır satır. İkinci parametre geri satır sayısıdır. Varsayılan değer 1’dir. Sayıda satır değilseniz varsayılan değeri belirtilmediği sürece bir NULL değeri döndürülür.
* ``lag(amount, 2) -> 60``
* ``lag(amount, 2000, 100) -> 100``
*********************************
<code>last</code>
==============================
<code><b>last(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : boolean]) => any</b></code><br/><br/>
Bu ifade, sütun grubu son değerini alır. İkinci parametresini atlarsanız, `ignoreNulls`, ifade döndürür `false`.
* ``last(sales) -> 523.12``
* ``last(sales, false) -> NULL``
*********************************
<code>lastDayOfMonth</code>
==============================
<code><b>lastDayOfMonth(<i>&lt;value1&gt;</i> : datetime) => date</b></code><br/><br/>
Bu ifade, bir tarihi göz önünde bulundurulduğunda, ayın son gününü alır.
* ``lastDayOfMonth(toDate('2009-01-12')) -> 2009-01-31``
*********************************
<code>lead</code>
==============================
<code><b>lead(<i>&lt;value&gt;</i> : any, [<i>&lt;number of rows to look after&gt;</i> : number], [<i>&lt;default value&gt;</i> : any]) => any</b></code><br/><br/>
Bu ifadenin değerlendirilmesi ilk parametre değerini alır *n* sonra geçerli satır satır. İkinci parametre sabırsızlıkla bekliyoruz satır sayısıdır. Varsayılan değer 1’dir. Sayıda satır değilseniz varsayılan değeri belirtilmediği sürece bir NULL değeri döndürülür.
* ``lead(amount, 2) -> 60``
* ``lead(amount, 2000, 100) -> 100``
*********************************
<code>least</code>
==============================
<code><b>least(<i>&lt;value1&gt;</i> : any, ...) => any</b></code><br/><br/>
Karşılaştırma küçük değerinden-veya-eşittir işleci budur. Aynı olan **<=** işleci.
* ``least(10, 30, 15, 20) -> 10``
* ``least(toDate('12/12/2010'), toDate('12/12/2011'), toDate('12/12/2000')) -> '12/12/2000'``
*********************************
<code>left</code>
==============================
<code><b>left(<i>&lt;string to subset&gt;</i> : string, <i>&lt;number of characters&gt;</i> : integral) => string</b></code><br/><br/>
Bu ifade, dizin 1 alt dizenin başlangıcında ayıklar ve karakter sayısını kullanır. **Sol** işleci, aynı **alt dize (str, 1, n)**.
* ``left('bojjus', 2) -> 'bo'``
* ``left('bojjus', 20) -> 'bojjus'``
*********************************
<code>length</code>
==============================
<code><b>length(<i>&lt;value1&gt;</i> : string) => integer</b></code><br/><br/>
Bu ifade, dize uzunluğunu döndürür.
* ``length('kiddo') -> 5``
*********************************
<code>lesser</code>
==============================
<code><b>lesser(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Bu, daha düşük bir karşılaştırma-işleci. Aynı olan **<** işleci.
* ``lesser(12 < 24) -> true``
* ``'abcd' < 'abc' -> false``
*********************************
<code>lesserOrEqual</code>
==============================
<code><b>lesserOrEqual(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Karşılaştırma küçük değerinden-veya-eşittir işleci budur. Aynı olan **<=** işleci.
* ``lesserOrEqual(12, 12) -> true``
* ``'abcd' <= 'abc' -> false``
*********************************
<code>levenshtein</code>
==============================
<code><b>levenshtein(<i>&lt;from string&gt;</i> : string, <i>&lt;to string&gt;</i> : string) => integer</b></code><br/><br/>
Bu ifade, iki dizeyi arasındaki Levenshtein uzaklık alır.
* ``levenshtein('boys', 'girls') -> 4``
*********************************
<code>like</code>
==============================
<code><b>like(<i>&lt;string&gt;</i> : string, <i>&lt;pattern match&gt;</i> : string) => boolean</b></code><br/><br/>
Bu ifade, deseni tam anlamıyla eşleşen bir dizedir. 
* ``like('icecream', 'ice%') -> true``

Özel durumlar, şu özel karakterleri şunlardır:  
* `_` Bu, giriş herhangi bir karakterle eşleşir. Benzer `.` POSIX normal ifadelerde.
* `%` Bu girişte sıfır veya daha fazla karakterle eşleşir. Bu sembol benzer `.*` POSIX normal ifadelerde.
* `''` Kaçış karakteri budur. Bir kaçış karakteri özel bir simge veya başka bir kaçış karakteri önceyse, şu karakteri tam anlamıyla eşleştirilir. Başka bir karakter kaçış olamaz.
*********************************
<code>locate</code>
==============================
<code><b>locate(<i>&lt;substring to find&gt;</i> : string, <i>&lt;string&gt;</i> : string, [<i>&lt;from index - 1-based&gt;</i> : integral]) => integer</b></code><br/><br/>
Bu ifade (1 göre) belirli bir konumunda başlayan bir dizedeki alt dizenin konumu bulur. Konumu atlarsanız, değerlendirme dize başlangıcında başlar. Konumu bulunmazsa, 0 döndürülür. 
* ``locate('eat', 'great') -> 3``
* ``locate('o', 'microsoft', 6) -> 7``
* ``locate('bad', 'good') -> 0``
*********************************
<code>log</code>
==============================
<code><b>log(<i>&lt;value1&gt;</i> : number, [<i>&lt;value2&gt;</i> : number]) => double</b></code><br/><br/>
Bu ifade, günlük değerini hesaplar. Kullanıldığında, isteğe bağlı bir taban veya Euler birkaç sağlayabilirsiniz.
* ``log(100, 10) -> 2``
*********************************
<code>log10</code>
==============================
<code><b>log10(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade bir taban 10 günlük değerini hesaplamak için kullanır.
* ``log10(100) -> 2``
*********************************
<code>lower</code>
==============================
<code><b>lower(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Bu ifade, küçük bir dizesini ayarlar.
* ``lower('GunChus') -> 'gunchus'``
*********************************
<code>lpad</code>
==============================
<code><b>lpad(<i>&lt;string to pad&gt;</i> : string, <i>&lt;final padded length&gt;</i> : integral, <i>&lt;padding&gt;</i> : string) => string</b></code><br/><br/>
Sağlanan doldurma, bu ifade sol doldurmalar kullanarak belirli bir süre dize dizesi kadar ulaşır. Dizenin uzunluğundan büyük veya eşit olup olmadığını bir işlem yok (İşlemsiz) dikkate almıştır.
* ``lpad('great', 10, '-') -> '-----great'``
* ``lpad('great', 4, '-') -> 'great'``
* '' lpad ('harika' 8 ' <>') -> ' <><great'``
*********************************
<code>ltrim</code>
==============================
<code><b>ltrim(<i>&lt;string to trim&gt;</i> : string, <i>&lt;trim characters&gt;</i> : string) => string</b></code><br/><br/>
Bu ifadenin sol-bir dizenin baştaki kırpar. İkinci parametre belirtilmemişse, ifade boşlukları kırpar. Aksi takdirde, ikinci parametresinin belirttiği karakter kırpar.
* ``ltrim('!--!wor!ld!', '-!') -> 'wor!ld!'``
*********************************
<code>max</code>
==============================
<code><b>max(<i>&lt;value1&gt;</i> : any) => any</b></code><br/><br/>
Bu ifadenin bir sütun en büyük değerini alır.
* ``MAX(sales) -> 12312131.12``
*********************************
<code>maxIf</code>
==============================
<code><b>maxIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Bu ifade, bir ölçütü temel alan, bir sütun en büyük değerini alır.
* ``maxIf(region == 'West', sales) -> 99999.56``
*********************************
<code>md5</code>
==============================
<code><b>md5(<i>&lt;value1&gt;</i> : any, ...) => string</b></code><br/><br/>
Bu ifade, değişen temel veri türlerinin sütun kümesini MD5 özetini hesaplar. 32 karakterlik bir onaltılık dize döndürür. 
* ``md5(5, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> 'c1527622a922c83665e49835e46350fe'``

Kullanabileceğiniz **md5** parmak izi bir satır için hesaplanacak işleci.
*********************************
<code>mean</code>
==============================
<code><b>mean(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Bu ifade bir sütunun değerlerin ortalamasını alır. **Ortalama** ortalama aynı Is işleci
* ``mean(sales) -> 7523420.234``
*********************************
<code>meanIf</code>
==============================
<code><b>meanIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Bu ifade, bir ölçütü temel alan, bir sütunun değerlerin ortalamasını alır. **MeanIf** işleci, aynı **avgIf**.
* ``meanIf(region == 'West', sales) -> 7523420.234``
*********************************
<code>min</code>
==============================
<code><b>min(<i>&lt;value1&gt;</i> : any) => any</b></code><br/><br/>
Bu ifadenin bir sütun en küçük değerini alır.
* ``min(sales) -> 00.01``
* ``min(orderDate) -> 12/12/2000``
*********************************
<code>minIf</code>
==============================
<code><b>minIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Bu ifade, bir ölçütü temel alan, en az bir sütunun değerini alır.
* ``minIf(region == 'West', sales) -> 00.01``
*********************************
<code>minus</code>
==============================
<code><b>minus(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Bu ifade sayı çıkarır. Örneğin, birkaç gün içinde bir tarih çıkarabilirsiniz. **Eksi** işleci, aynı **-** işleci.
* ``minus(20, 10) -> 10``
* ``20 - 10 -> 10``
* ``minus(toDate('2012-12-15'), 3) -> 2012-12-12 (date value)``
* ``toDate('2012-12-15') - 3 -> 2012-12-13 (date value)``
*********************************
<code>minute</code>
==============================
<code><b>minute(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Bu ifade, bir zaman damgası dakika değerini alır. 
* ``minute(toTimestamp('2009-07-30T12:58:59')) -> 58``
* ``minute(toTimestamp('2009-07-30T12:58:59', 'PST')) -> 58``

İsteğe bağlı bir saat dilimi biçiminde geçirebilirsiniz `'GMT'`, `'PST'`, `'UTC'`, `'America/Cayman'`. Yerel saat dilimi varsayılandır.
*********************************
<code>mod</code>
==============================
<code><b>mod(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
**Mod** işleci bir mod çift sayıların işaretler. Bu, aynı **%** işleci.
* ``mod(20, 8) -> 4``
* ``20 % 8 -> 4``
*********************************
<code>month</code>
==============================
<code><b>month(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Bu ifade, bir tarih veya zaman damgası ay değerini alır.
* ``month(toDate('2012-8-8')) -> 8``
*********************************
<code>monthsBetween</code>
==============================
<code><b>monthsBetween(<i>&lt;from date/timestamp&gt;</i> : datetime, <i>&lt;to date/timestamp&gt;</i> : datetime, [<i>&lt;time zone&gt;</i> : boolean], [<i>&lt;value4&gt;</i> : string]) => double</b></code><br/><br/>
Bu ifade, iki tarih arasındaki ay sayısını alır. 
* ``monthsBetween(toDate('1997-02-28 10:30:00'), toDate('1996-10-30')) -> 3.94959677``

İsteğe bağlı bir saat dilimi biçiminde geçirebilirsiniz `'GMT'`, `'PST'`, `'UTC'`, `'America/Cayman'`. Yerel saat dilimi varsayılandır.
*********************************
<code>multiply</code>
==============================
<code><b>multiply(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Bu ifade bir çift sayı ile çarpar. **Çarp** işleci, aynı * işleci.
* ``multiply(20, 10) -> 200``
* ``20 * 10 -> 200``
*********************************
<code>nTile</code>
==============================
<code><b>nTile([<i>&lt;value1&gt;</i> : integer]) => integer</b></code><br/><br/>
**NTile** işlevi, her pencere bölüme satırları böler *n* 1'den en fazla aralık demet *n*. Demet değerler en fazla 1 göre farklılık gösterir. Bölüm satır sayısı sayıda demete eşit olarak bölmeye değil, her kalan bir değeri ilk demet ile başlayan bir demet içinde dağıtılır. 

* ``nTile() -> 1``
* ``nTile(numOfBuckets) -> 1``

**NTile** işlevi, tertiles, Dörtte birlikler deciles ve diğer ortak bir Özet istatistiklerini hesaplamak gerektiğinde yararlıdır. İşlevi, başlatma sırasında iki değişkeni hesaplar. Normal bir demet için fazladan bir satır eklenir. Her iki değişken, geçerli bölüm boyutuna dayanır. 

Hesaplama sırasında işlev geçerli satır numarasını ve geçerli demet numarasını demet (bucketThreshold) değişecek mi satır sayısını izler. Satır numarasını demetine eşiğe ulaştığında, bir demet değeri artırır. Geçerli demet azsa, eşiği kova boyutu artı bir tarafından ek artırır.
*********************************
<code>negate</code>
==============================
<code><b>negate(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Bu ifade bir sayı geçersiz hale getirir. Pozitif sayıları kapatır negatif sayılar ve bunun tersi de geçerlidir.
* ``negate(13) -> -13``
*********************************
<code>nextSequence</code>
==============================
<code><b>nextSequence() => long</b></code><br/><br/>
Bu ifade sonraki benzersiz dizisi döndürür. Yalnızca bir bölüm içinde art arda sayısıdır. Tarafından öneki `partitionId`.
* ``nextSequence() -> 12313112``
*********************************
<code>normalize</code>
==============================
<code><b>normalize(<i>&lt;String to normalize&gt;</i> : string) => string</b></code><br/><br/>
Vurgulu unicode karakterlerini ayırmak için dize değeri Normalleştir * ``normalize('boys') -> 'boys'``
*********************************
<code>not</code>
==============================
<code><b>not(<i>&lt;value1&gt;</i> : boolean) => boolean</b></code><br/><br/>
**Değil** işleci, bir mantıksal değilleme işleci.
* ``not(true) -> false``
* ``not(premium)``
*********************************
<code>notEquals</code>
==============================
<code><b>notEquals(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
**NotEquals** işleci, bir karşılaştırma eşit değil işleci. Aynı olan **! =** işleci.
* ``12!=24 -> true``
* ``'abc'!='abc' -> false``
*********************************
<code>null</code>
==============================
<code><b>null() => null</b></code><br/><br/>
Bu ifade, bir NULL değer döndürür. 
* ``custId = NULL (for derived field)``
* ``custId == NULL -> NULL``
* ``'nothing' + NULL -> NULL``
* ``10 * NULL -> NULL'``
* ``NULL == '' -> NULL'``

İşlevini **syntax(null())** bir sütun ise *null*. NULL işlecini kullanan herhangi bir işlem NULL olur.
*********************************
<code>or</code>
==============================
<code><b>or(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></code><br/><br/>
**Veya** işleci, bir mantıksal **veya** işleci. Aynı olan **||** işleci.
* ``or(true, false) -> true``
* ``true || false -> true``
*********************************
<code>pMod</code>
==============================
<code><b>pMod(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
**PMod** işaretler, bir çift sayı pozitif modulus işleci.
* ``pmod(-20, 8) -> 4``
*********************************
<code>power</code>
==============================
<code><b>power(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade, başka bir sayıyı başlatır.
* ``power(10, 2) -> 100``
*********************************
<code>rank</code>
==============================
<code><b>rank(<i>&lt;value1&gt;</i> : any, ...) => integer</b></code><br/><br/>
Bu ifade, boyut değerlerinin bir gruptaki bir değer sayısını hesaplar. Geçerli satırın bölümünü sıralama, eşit veya önceki satır sayısı artı bir sonucudur. Değerler sıradaki boşlukları üretir. 
* ``rank(salesQtr, salesAmt) -> 1``

**Derece** işleci bile verilerin ne zaman sıralanmamış çalışır. Değerleri değişiklikleri arar.
*********************************
<code>regexExtract</code>
==============================
<code><b>regexExtract(<i>&lt;string&gt;</i> : string, <i>&lt;regex to find&gt;</i> : string, [<i>&lt;match group 1-based index&gt;</i> : integral]) => string</b></code><br/><br/>
Bu ifade, belirli bir normal ifade deseni için eşleşen bir alt dizeyi ayıklar. Son parametre eşleşen grubunu tanımlar. Son parametresini atlarsanız, varsayılan değer 1'dir. 
* ``regexExtract('Cost is between 600 and 800 dollars', '(\\d+) and (\\d+)', 2) -> '800'``
* ``regexExtract('Cost is between 600 and 800 dollars', `(\d+) and (\d+)`, 2) -> '800'``

Kullanım `<regex>`(kaçış olmadan bir dizeyle eşleştirme yapmayı geriye tırnak).
*********************************
<code>regexMatch</code>
==============================
<code><b>regexMatch(<i>&lt;string&gt;</i> : string, <i>&lt;regex to match&gt;</i> : string) => boolean</b></code><br/><br/>
Bu ifade, dize belirli bir normal ifade deseni ile eşleşip eşleşmediğini denetler. 
* ``regexMatch('200.50', '(\\d+).(\\d+)') -> true``
* ``regexMatch('200.50', `(\d+).(\d+)`) -> true``

Kullanım `<regex>`(kaçış olmadan bir dizeyle eşleştirme yapmayı geriye tırnak).
*********************************
<code>regexReplace</code>
==============================
<code><b>regexReplace(<i>&lt;string&gt;</i> : string, <i>&lt;regex to find&gt;</i> : string, <i>&lt;substring to replace&gt;</i> : string) => string</b></code><br/><br/>
Bu ifade verilen dize içinde başka bir alt dizenin bir normal ifade deseni tüm oluşumlarını değiştirir.
* ``regexReplace('100 and 200', '(\\d+)', 'bojjus') -> 'bojjus and bojjus'``
* ``regexReplace('100 and 200', `(\d+)`, 'gunchus') -> 'gunchus and gunchus'``

Kullanım `<regex>`(kaçış olmadan bir dizeyle eşleştirme yapmayı geriye tırnak).
*********************************
<code>regexSplit</code>
==============================
<code><b>regexSplit(<i>&lt;string to split&gt;</i> : string, <i>&lt;regex expression&gt;</i> : string) => array</b></code><br/><br/>
Bu ifade, normal ifade üzerinde temel bir sınırlayıcı dayalı bir dize böler. Dize dizisi döndürür.
* ``regexSplit('oneAtwoBthreeC', '[CAB]') -> ['one', 'two', 'three']``
* ``regexSplit('oneAtwoBthreeC', '[CAB]')[1] -> 'one'``
* ``regexSplit('oneAtwoBthreeC', '[CAB]')[0] -> NULL``
* ``regexSplit('oneAtwoBthreeC', '[CAB]')[20] -> NULL``
*********************************
<code>replace</code>
==============================
<code><b>replace(<i>&lt;string&gt;</i> : string, <i>&lt;substring to find&gt;</i> : string, <i>&lt;substring to replace&gt;</i> : string) => string</b></code><br/><br/>
Bu ifade başka bir alt dizenin belirtilen dizedeki bir alt dizenin tüm oluşumlarını değiştirir.
* ``replace('doggie dog', 'dog', 'cat') -> 'catgie cat'``
* ``replace('doggie dog', 'dog', '') -> 'gie'``
*********************************
<code>reverse</code>
==============================
<code><b>reverse(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Bu ifade bir dize tersine çevirir.
* ``reverse('gunchus') -> 'suhcnug'``
*********************************
<code>right</code>
==============================
<code><b>right(<i>&lt;string to subset&gt;</i> : string, <i>&lt;number of characters&gt;</i> : integral) => string</b></code><br/><br/>
Bu ifade sağ taraftan karakter sayısı ile bir alt dizeyi ayıklar. 
* ``right('bojjus', 2) -> 'us'``
* ``right('bojjus', 20) -> 'bojjus'``

**Doğru** işlev, aynı alt dize (str, LENGTH(str) - n, n).
*********************************
<code>rlike</code>
==============================
<code><b>rlike(<i>&lt;string&gt;</i> : string, <i>&lt;pattern match&gt;</i> : string) => boolean</b></code><br/><br/>
Bu ifade, dize belirli bir normal ifade deseni ile eşleşip eşleşmediğini denetler.
* ``rlike('200.50', '(\d+).(\d+)') -> true``
*********************************
<code>round</code>
==============================
<code><b>round(<i>&lt;number&gt;</i> : number, [<i>&lt;scale to round&gt;</i> : number], [<i>&lt;rounding option&gt;</i> : integral]) => double</b></code><br/><br/>
Verilen isteğe bağlı bir ölçek ve isteğe bağlı bir yuvarlama modu, bu ifade bir sayıyı yuvarlar. Ölçek atlarsanız, varsayılan değer 0'dır. Modu atlarsanız, ROUND_HALF_UP(5) varsayılandır. 

* ``round(100.123) -> 100.0``
* ``round(2.5, 0) -> 3.0``
* ``round(5.3999999999999995, 2, 7) -> 5.40``

Yuvarlama için değerler şunlardır:
* 1 - ROUND_UP
* 2 - ROUND_DOWN
* 3 - ROUND_CEILING
* 4 - ROUND_FLOOR
* 5 - ROUND_HALF_UP
* 6 - ROUND_HALF_DOWN
* 7 - ROUND_HALF_EVEN
* 8 - ROUND_UNNECESSARY

*********************************
<code>rowNumber</code>
==============================
<code><b>rowNumber() => integer</b></code><br/><br/>
Bu ifade bir penceresinde, 1 ile başlayan satırlar için numaralandırma sıralı bir satır atar.
* ``rowNumber() -> 1``
*********************************
<code>rpad</code>
==============================
<code><b>rpad(<i>&lt;string to pad&gt;</i> : string, <i>&lt;final padded length&gt;</i> : integral, <i>&lt;padding&gt;</i> : string) => string</b></code><br/><br/>
Bu ifade sağ bölmeleri dize kadar sağlanan doldurma dizesi belirli bir süre ulaşır. Dizenin uzunluğundan büyük veya eşit olup olmadığını bir işlem yok (İşlemsiz) dikkate almıştır.
* ``rpad('great', 10, '-') -> 'great-----'``
* ``rpad('great', 4, '-') -> 'great'``
* ``rpad('great', 8, '<>') -> 'great<><'``
*********************************
<code>rtrim</code>RTrim</code>
==============================
<code><b>rtrim(<i>&lt;string to trim&gt;</i> : string, <i>&lt;trim characters&gt;</i> : string) => string</b></code><br/><br/>
Bu ifade sağ-bir dizenin baştaki kırpar. İkinci parametre belirtmezseniz, ifade boşlukları kırpar. Aksi takdirde, ikinci parametresinin belirttiği herhangi bir karakterle kırpar.
* ``rtrim('!--!wor!ld!', '-!') -> '!--!wor!ld'``
*********************************
<code>second</code>
==============================
<code><b>second(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Bu ifade, ikinci bir tarih değerini alır. 
* ``second(toTimestamp('2009-07-30T12:58:59')) -> 59``

İsteğe bağlı bir saat dilimi biçiminde geçirebilirsiniz `'GMT'`, `'PST'`, `'UTC'`, `'America/Cayman'`. Yerel saat dilimi varsayılandır.
*********************************
<code>sha1</code>
==============================
<code><b>sha1(<i>&lt;value1&gt;</i> : any, ...) => string</b></code><br/><br/>
Bu ifade, değişen temel veri türlerinin sütun kümesini SHA-1 özetini hesaplar. 40 karakterlik bir onaltılık dize döndürür. 
* ``sha1(5, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> '63849fd2abb65fbc626c60b1f827bd05573f0cea'``

Kullanabileceğiniz `sha1` parmak izi bir satır için hesaplanacak.
*********************************
<code>sha2</code>
==============================
<code><b>sha2(<i>&lt;value1&gt;</i> : integer, <i>&lt;value2&gt;</i> : any, ...) => string</b></code><br/><br/>
Bu ifade değerleri yalnızca 0(256) olabilir bir bit uzunluğu verilen basit veri türleri, değişen sütun kümesini SHA-2 özetini hesaplar 224, 256, 384, 512. 
* ``sha2(256, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> 'd3b2bff62c3a00e9370b1ac85e428e661a7df73959fa1a96ae136599e9ee20fd'``

Kullanabileceğiniz `sha2` parmak izi bir satır için hesaplanacak.
*********************************
<code>sin</code>
==============================
<code><b>sin(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade sinüs değerini hesaplar.
* ``sin(2) -> 0.90929742682``
*********************************
<code>sinh</code>
==============================
<code><b>sinh(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade, hiperbolik sinüs değerini hesaplar.
* ``sinh(0) -> 0.0``
*********************************
<code>skewness</code>
==============================
<code><b>skewness(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade, bir sütunun komutunu alır.
* ``skewness(sales) -> 122.12``
*********************************
<code>skewnessIf</code>
==============================
<code><b>skewnessIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade, bir ölçütü temel alan, bir sütunun komutunu alır.
* ``skewnessIf(region == 'West', sales) -> 122.12``
*********************************
<code>slice</code>
==============================
<code><b>slice(<i>&lt;array to slice&gt;</i> : array, <i>&lt;from 1-based index&gt;</i> : integral, [<i>&lt;number of items&gt;</i> : integral]) => array</b></code><br/><br/>
Bu ifade, bir dizi kümesini bir konumdan ayıklar. Konum 1 temel alır. Uzunluğu atlarsanız, dizenin sonuna kadar varsayılandır.
* ``slice([10, 20, 30, 40], 1, 2) -> [10, 20]``
* ``slice([10, 20, 30, 40], 2) -> [20, 30, 40]``
* ``slice([10, 20, 30, 40], 2)[1] -> 20``
* ``slice([10, 20, 30, 40], 2)[0] -> NULL``
* ``slice([10, 20, 30, 40], 2)[20] -> NULL``
* ``slice([10, 20, 30, 40], 8) -> []``
*********************************
<code>soundex</code>
==============================
<code><b>soundex(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Bu ifade, dize soundex kodunu alır.
* ``soundex('genius') -> 'G520'``
*********************************
<code>split</code>
==============================
<code><b>split(<i>&lt;string to split&gt;</i> : string, <i>&lt;split characters&gt;</i> : string) => array</b></code><br/><br/>
Bu ifade, bir sınırlayıcıyı bir dizeyi böler. Dize dizisi döndürür.
* ``split('100,200,300', ',') -> ['100', '200', '300']``
* ``split('100,200,300', '|') -> ['100,200,300']``
* ``split('100, 200, 300', ', ') -> ['100', '200', '300']``
* ``split('100, 200, 300', ', ')[1] -> '100'``
* ``split('100, 200, 300', ', ')[0] -> NULL``
* ``split('100, 200, 300', ', ')[20] -> NULL``
* ``split('100200300', ',') -> ['100200300']``
*********************************
<code>sqrt</code>
==============================
<code><b>sqrt(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade, bir sayının karekökünü hesaplar.
* ``sqrt(9) -> 3``
*********************************
<code>startsWith</code>
==============================
<code><b>startsWith(<i>&lt;string&gt;</i> : string, <i>&lt;substring to check&gt;</i> : string) => boolean</b></code><br/><br/>
Bu ifade, dize sağlanan dizeyle başlayıp başlamadığını denetler.
* ``startsWith('great', 'gr') -> true``
*********************************
<code>stddev</code>
==============================
<code><b>stddev(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade, bir sütunun standart sapma alır.
* ``stdDev(sales) -> 122.12``
*********************************
<code>stddevIf</code>
==============================
<code><b>stddevIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade, bir ölçütü temel alan, bir sütunun standart sapma alır.
* ``stddevIf(region == 'West', sales) -> 122.12``
*********************************
<code>stddevPopulation</code>
==============================
<code><b>stddevPopulation(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bu ifadenin bir sütun popülasyon standart sapmasını alır.
* ``stddevPopulation(sales) -> 122.12``
*********************************
<code>stddevPopulationIf</code>
==============================
<code><b>stddevPopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade, bir ölçütü temel alan, bir sütun popülasyon standart sapmasını alır.
* ``stddevPopulationIf(region == 'West', sales) -> 122.12``
*********************************
<code>stddevSample</code>
==============================
<code><b>stddevSample(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bu ifadenin bir sütun örneği standart sapmasını alır.
* ``stddevSample(sales) -> 122.12``
*********************************
<code>stddevSampleIf</code>
==============================
<code><b>stddevSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade, bir ölçütü temel alan, bir sütun örneği standart sapmasını alır.
* ``stddevSampleIf(region == 'West', sales) -> 122.12``
*********************************
<code>subDays</code>
==============================
<code><b>subDays(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;days to subtract&gt;</i> : integral) => datetime</b></code><br/><br/>
Bu ifade bir tarihten itibaren ay çıkarır. 
* ``subDays(toDate('2016-08-08'), 1) -> 2016-08-09``
**SubDays** işleci, aynı **-** işleci tarih.
*********************************
<code>subMonths</code>
==============================
<code><b>subMonths(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;months to subtract&gt;</i> : integral) => datetime</b></code><br/><br/>
Bu ifade, bir tarih veya zaman damgası aylardan çıkarır.
* ``subMonths(toDate('2016-09-30'), 1) -> 2016-08-31``
*********************************
<code>substring</code>
==============================
<code><b>substring(<i>&lt;string to subset&gt;</i> : string, <i>&lt;from 1-based index&gt;</i> : integral, [<i>&lt;number of characters&gt;</i> : integral]) => string</b></code><br/><br/>
Bu ifade bir konumdan belirli bir süre alt dizeyi ayıklar. Konum 1 temel alır. Uzunluğu atlarsanız, dizenin sonuna kadar varsayılandır.
* ``substring('Cat in the hat', 5, 2) -> 'in'``
* ``substring('Cat in the hat', 5, 100) -> 'in the hat'``
* ``substring('Cat in the hat', 5) -> 'in the hat'``
* ``substring('Cat in the hat', 100, 100) -> ''``
*********************************
<code>sum</code>
==============================
<code><b>sum(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Bu ifade, sayısal bir sütun toplama toplamını alır.
* ``sum(col) -> value``
*********************************
<code>sumDistinct</code>
==============================
<code><b>sumDistinct(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Bu ifade, toplama toplamı değerlerinin bir sayısal sütun sayısını alır.
* ``sumDistinct(col) -> value``
*********************************
<code>sumDistinctIf</code>
==============================
<code><b>sumDistinctIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Bu ifade bir ölçütü temel alan, sayısal bir sütun toplama toplamını alır. Herhangi bir sütunu koşulu temel alabilir.
* ``sumDistinctIf(state == 'CA' && commission < 10000, sales) -> value``
* ``sumDistinctIf(true, sales) -> SUM(sales)``
*********************************
<code>sumIf</code>
==============================
<code><b>sumIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Bu ifade ölçütü temel alan, sayısal bir sütun toplama toplamını alır. Herhangi bir sütunu koşulu temel alabilir.
* ``sumIf(state == 'CA' && commission < 10000, sales) -> value``
* ``sumIf(true, sales) -> SUM(sales)``
*********************************
<code>tan</code>
==============================
<code><b>tan(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade tanjant değerini hesaplar.
* ``tan(0) -> 0.0``
*********************************
<code>tanh</code>
==============================
<code><b>tanh(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade bir hiperbolik tanjant değerini hesaplar.
* ``tanh(0) -> 0.0``
*********************************
<code>toBoolean</code>
==============================
<code><b>toBoolean(<i>&lt;value1&gt;</i> : string) => boolean</b></code><br/><br/>
Bu ifadenin değeri dönüştürür `('t', 'true', 'y', 'yes', '1')` true. Bunu dönüştüren `('f', 'false', 'n', 'no', '0')` false. Diğer herhangi bir değer için NULL değer döndürür.
* ``toBoolean('true') -> true``
* ``toBoolean('n') -> false``
* ``toBoolean('truthy') -> NULL``
*********************************
<code>toDate</code>
==============================
<code><b>toDate(<i>&lt;string&gt;</i> : any, [<i>&lt;date format&gt;</i> : string]) => date</b></code><br/><br/>
Bu ifade bir isteğe bağlı bir tarih biçimi göz önünde bulundurulduğunda, bir tarihi bir dizeye dönüştürür. Java SimpleDateFormat için tüm olası tarih biçimleri için bakın. 
* ``toDate('2012-8-8') -> 2012-8-8``
* ``toDate('12/12/2012', 'MM/dd/yyyy') -> 2012-12-12``

Tarih biçimi atlarsanız, aşağıdaki birleşimlerini kabul edilir: [yyyy, yyyy-[M] M, yyyy [d]-[M] M - d, yyyy-[M] M-[d] d, yyyy-[M] M-[d] d, yyyy-[M] M-[d] dT *].
*********************************
<code>toDecimal</code>
==============================
<code><b>toDecimal(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : integral], [<i>&lt;value3&gt;</i> : integral], [<i>&lt;value4&gt;</i> : string]) => decimal(10,0)</b></code><br/><br/>
Bu ifade, bir ondalık değer için herhangi bir sayısal veya dize dönüştürür. 
* ``toDecimal(123.45) -> 123.45``
* ``toDecimal('123.45', 8, 4) -> 123.4500``
* ``toDecimal('$123.45', 8, 4,'$###.00') -> 123.4500``

Kesinlik ve ölçek belirtmezseniz (10,2) varsayılandır. Java için isteğe bağlı bir ondalık biçim dönüştürme için kullanabilirsiniz.

*********************************
<code>toDouble</code>
==============================
<code><b>toDouble(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string]) => double</b></code><br/><br/>
Bu ifade, herhangi bir sayısal veya dize bir çift değere dönüştürür. Java için isteğe bağlı bir ondalık biçim dönüştürme için kullanabilirsiniz.
* ``toDouble(123.45) -> 123.45``
* ``toDouble('123.45') -> 123.45``
* ``toDouble('$123.45', '$###.00') -> 123.45``
*********************************
<code>toFloat</code>
==============================
<code><b>toFloat(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string]) => float</b></code><br/><br/>
Bu ifade, herhangi bir sayısal veya dize bir kayan nokta değerine dönüştürür. Java için isteğe bağlı bir ondalık biçim dönüştürme için kullanabilirsiniz. 
* ``toFloat(123.45) -> 123.45``
* ``toFloat('123.45') -> 123.45``
* ``toFloat('$123.45', '$###.00') -> 123.45``

**ToFloat** işlevi herhangi bir çift keser.
*********************************
<code>toInteger</code>
==============================
<code><b>toInteger(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string]) => integer</b></code><br/><br/>
Bu ifade herhangi bir sayısal veya dize, tamsayı değerine dönüştürür. Java için isteğe bağlı bir ondalık biçim dönüştürme için kullanabilirsiniz. 
* ``toInteger(123) -> 123``
* ``toInteger('123') -> 123``
* ``toInteger('$123', '$###') -> 123``

**ToInteger** işlevi keser herhangi uzun, float veya çift.
*********************************
<code>toLong</code>
==============================
<code><b>toLong(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string]) => long</b></code><br/><br/>
Bu ifade, herhangi bir sayısal veya dize uzun değerine dönüştürür. Java için isteğe bağlı bir ondalık biçim dönüştürme için kullanabilirsiniz. 
* ``toLong(123) -> 123``
* ``toLong('123') -> 123``
* ``toLong('$123', '$###') -> 123``

**ToLong** işlevi herhangi bir float veya double keser.
*********************************
<code>toShort</code>
==============================
<code><b>toShort(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string]) => short</b></code><br/><br/>
Bu ifade, herhangi bir sayısal veya dize kısa bir değerine dönüştürür. Java için isteğe bağlı bir ondalık biçim dönüştürme için kullanabilirsiniz. 
* ``toShort(123) -> 123``
* ``toShort('123') -> 123``
* ``toShort('$123', '$###') -> 123``

**ToShort** işlevi keser herhangi bir tamsayı, kayan veya çift.
*********************************
<code>toString</code>
==============================
<code><b>toString(<i>&lt;value&gt;</i> : any, [<i>&lt;number format/date format&gt;</i> : string]) => string</b></code><br/><br/>
Bu ifade, bir basit veri türü bir dizeye dönüştürür. 
* ``toString(10) -> '10'``
* ``toString('engineer') -> 'engineer'``
* ``toString(123456.789, '##,###.##') -> '123,456.79'``
* ``toString(123.78, '000000.000') -> '000123.780'``
* ``toString(12345, '##0.#####E0') -> '12.345E3'``
* ``toString(toDate('2018-12-31')) -> '2018-12-31'``
* ``toString(toDate('2018-12-31'), 'MM/dd/yy') -> '12/31/18'``
* ``toString(4 == 20) -> 'false'``

Sayılar ve tarihler için biçim belirtebilirsiniz. Bir biçim belirtmezseniz, sistem varsayılan kullanılır. Varsayılan biçimi `yyyy-MM-dd`. Java ondalık sayılar izleyin. Tüm olası tarih biçimleri için Java SimpleDateFormat bakın. 
*********************************
<code>toTimestamp</code>
==============================
<code><b>toTimestamp(<i>&lt;string&gt;</i> : any, [<i>&lt;timestamp format&gt;</i> : string], [<i>&lt;time zone&gt;</i> : string]) => timestamp</b></code><br/><br/>
Bu ifade isteğe bağlı bir zaman damgası biçimi göz önünde bulundurulduğunda, bir tarihi bir dizeye dönüştürür. 
* ``toTimestamp('2016-12-31 00:12:00') -> 2012-8-8T00:12:00``
* ``toTimestamp('2016/12/31T00:12:00', 'MM/dd/yyyyThh:mm:ss') -> 2012-12-12T00:12:00``

Tüm olası biçimleri için Java SimpleDateFormat bakın. Zaman damgası, varsayılan düzen atlarsanız `yyyy-[M]M-[d]d hh:mm:ss[.f...]` kullanılır.
*********************************
<code>toUTC</code>
==============================
<code><b>toUTC(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => timestamp</b></code><br/><br/>
Bu ifade için UTC zaman damgası dönüştürür. 
* ``toUTC(currentTimeStamp()) -> 12-12-2030T19:18:12``
* ``toUTC(currentTimeStamp(), 'Asia/Seoul') -> 12-13-2030T11:18:12``

İsteğe bağlı bir saat dilimi biçiminde geçirebilirsiniz `'GMT'`, `'PST'`, `'UTC'`, `'America/Cayman'`. Geçerli saat dilimi varsayılandır.
*********************************
<code>translate</code>
==============================
<code><b>translate(<i>&lt;string to translate&gt;</i> : string, <i>&lt;lookup characters&gt;</i> : string, <i>&lt;replace characters&gt;</i> : string) => string</b></code><br/><br/>
Bu ifade bir karakter kümesini başka bir dizedeki karakter kümesini değiştirir. Karakter bire bir değiştirme var.
* ``translate('(Hello)', '()', '[]') -> '[Hello]'``
* ``translate('(Hello)', '()', '[') -> '[Hello'``
*********************************
<code>trim</code>
==============================
<code><b>trim(<i>&lt;string to trim&gt;</i> : string, [<i>&lt;trim characters&gt;</i> : string]) => string</b></code><br/><br/>
Bu ifade, başında ve sonunda bir karakter dizesi kırpar. 
* ``trim('!--!wor!ld!', '-!') -> 'wor!ld'``

İkinci parametre belirtmezseniz, işlev boşlukları kırpar. Aksi takdirde, ikinci parametresinin belirttiği herhangi bir karakterle kırpar.

*********************************
<code>true</code>
==============================
<code><b>true() => boolean</b></code><br/><br/>
Bu ifade her zaman döndürür bir `true` değeri.  
* ``isDiscounted == true()``
* ``isDiscounted() == true``

Sütun ise *true*, işlevini **syntax(true())**.
*********************************
<code>typeMatch</code>
==============================
<code><b>typeMatch(<i>&lt;type&gt;</i> : string, <i>&lt;base type&gt;</i> : string) => boolean</b></code><br/><br/>
Bu ifade, sütun türü eşleşir. 
* ``typeMatch(type, 'number') -> true``
* ``typeMatch('date', 'number') -> false``

Desen ifadelerinde yalnızca bu işlevi kullanın: numarasıyla eşleşen kısa, tamsayı, long, double, kayan veya tamsayı ondalık eşleşen kısa, tamsayı, çift uzun, kesirli eşleşme, float, ondalık ve datetime eşleşme tarih veya zaman damgası türü.
*********************************
<code>upper</code>
==============================
<code><b>upper(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Bu ifade bir dizeyi büyük harf olarak ayarlar.
* ``upper('bojjus') -> 'BOJJUS'``
*********************************
<code>variance</code>
==============================
<code><b>variance(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade, bir sütunun varyans alır.
* ``variance(sales) -> 122.12``
*********************************
<code>varianceIf</code>
==============================
<code><b>varianceIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade, bir ölçütü temel alan, bir sütunun varyans alır.
* ``varianceIf(region == 'West', sales) -> 122.12``
*********************************
<code>variancePopulation</code>
==============================
<code><b>variancePopulation(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bu ifadenin bir sütun popülasyon varyansını alır.
* ``variancePopulation(sales) -> 122.12``
*********************************
<code>variancePopulationIf</code>
==============================
<code><b>variancePopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade, bir ölçütü temel alan, bir sütun popülasyon varyansını alır.
* ``variancePopulationIf(region == 'West', sales) -> 122.12``
*********************************
<code>varianceSample</code>
==============================
<code><b>varianceSample(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bu ifadenin bir sütun popülasyon varyansını alır.
* ``varianceSample(sales) -> 122.12``
*********************************
<code>varianceSampleIf</code>
==============================
<code><b>varianceSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Bu ifade, bir ölçütü temel alan, bir sütun popülasyon varyansını alır.
* ``varianceSampleIf(region == 'West', sales) -> 122.12``
*********************************
<code>weekOfYear</code>
==============================
<code><b>weekOfYear(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Bu ifade, bir tarihi göz önünde bulundurulduğunda, yılın haftası alır.
* ``weekOfYear(toDate('2008-02-20')) -> 8``
*********************************
<code>xor</code>
==============================
<code><b>xor(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></code><br/><br/>
Bu ifade mantıksal kullanır **xor** işleci. Bu işleç aynıdır **^** işleci.
* ``xor(true, false) -> true``
* ``xor(true, true) -> false``
* ``true ^ false -> true``
*********************************
<code>year</code>
==============================
<code><b>year(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Bu ifade, bir tarihi göz önünde bulundurulduğunda, yıl değerini alır.
* ``year(toDate('2012-8-8')) -> 2012``

## <a name="next-steps"></a>Sonraki adımlar

[İfade Oluşturucu kullanmayı öğrenin](concepts-data-flow-expression-builder.md).
