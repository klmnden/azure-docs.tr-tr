---
title: Sistem işlevleri
description: Azure Cosmos DB'de SQL sistem işlevleri hakkında bilgi edinin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: mjbrown
ms.openlocfilehash: 11a6fdad187670bcb5af4c56198fd7343680690d
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342946"
---
# <a name="system-functions"></a>Sistem işlevleri

 Cosmos DB, çok sayıda yerleşik SQL işlevleri sağlar. Yerleşik işlevler kategorileri aşağıda listelenmiştir.  
  
|İşlev|Açıklama|  
|--------------|-----------------|  
|[Matematik işlevleri](#mathematical-functions)|Matematik işlevleri her, genellikle bağımsız değişken olarak sağlanan ve sayısal bir değer döndürmesi giriş değerlerine göre bir hesaplama gerçekleştirir.|  
|[Tür denetimini işlevleri](#type-checking-functions)|Tür denetimi işlevleri SQL sorgusu içindeki bir ifadenin türü denetlemenizi sağlar.|  
|[Dize işlevleri](#string-functions)|Dize işlevleri, bir dize giriş değeri bir işlem gerçekleştirmek ve bir dize, sayısal veya Boolean değeri döndürür.|  
|[Dizi işlevleri](#array-functions)|Dizi işlevler bir dizi giriş değeri ve dönüş sayısal, Boole değeri veya dizi değeri üzerinde bir işlem gerçekleştirir.|
|[Tarih ve saat işlevleri](#date-time-functions)|Tarih ve saat işlevleri geçerli UTC tarih ve saat iki biçimde almanızı sağlar. değeri milisaniye cinsinden veya ISO 8601 biçimine uyan bir dize olarak UNIX dönem olan sayısal bir zaman damgası.|
|[Uzamsal İşlevler](#spatial-functions)|Uzamsal İşlevler, uzamsal nesne giriş değeri bir işlem gerçekleştirmek ve bir sayısal veya Boolean değeri döndürür.|  

Her bir kategorideki işlevlerin bir listesi aşağıda verilmiştir:

| İşlev grubu | İşlemler |
|---------|----------|
| Matematik işlevleri | ABS, TAVAN, EXP, FLOOR, GÜNLÜK, LOG10, GÜÇ, HEPSİNİ, OTURUM, SQRT, KARE, TRUNC, ACOS, ASIN, ATAN, ATN2, COT, DERECE, PI, RADIANS, SIN COS, TAN |
| Tür denetleme işlevleri | IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED, IS_PRIMITIVE |
| Dize işlevleri | CONCAT, İÇERİR, ENDSWITH, INDEX_OF, SOL, UZUNLUĞU, DÜŞÜK LTRIM, DEĞİŞTİR, ÇOĞALTMAK, SAĞ TERS RTRIM, STARTSWITH, ALT, ÜST |
| Dizi işlevleri | ARRAY_CONCAT, ARRAY_CONTAINS ARRAY_LENGTH ve ARRAY_SLICE |
| Tarih ve saat işlevleri | GETCURRENTDATETIME, GETCURRENTTIMESTAMP,  |
| Uzamsal İşlevler | ST_DISTANCE ST_WITHIN, ST_INTERSECTS ST_ISVALID, ST_ISVALIDDETAILED |

Şu anda yerleşik işlevi artık kullanılabilir olduğu bir kullanıcı tanımlı işlev (UDF) kullanıyorsanız, karşılık gelen yerleşik işlev çalıştırmak daha hızlı ve daha verimli olacaktır.

Cosmos DB işlevleri ve ANSI SQL işlevleri arasındaki temel fark, Cosmos DB işlevleri de şemasız hem de karma şema verilerle çalışacak şekilde tasarlanmıştır ' dir. Örneğin, bir özellik eksik veya varsa bir sayısal olmayan değer gibi `unknown`, bir hata döndürmek yerine öğenin atlandı.

##  <a name="mathematical-functions"></a> Matematik işlevleri  

Matematiksel işlevler her bağımsız değişken olarak sağlanan ve sayısal bir değer döndürmesi giriş değerlerini temel alan, bir hesaplama gerçekleştirir.

Aşağıdaki örnekte olduğu gibi sorguları çalıştırabilirsiniz:

```sql
    SELECT VALUE ABS(-4)
```

Sonuç olur:

```json
    [4]
```

Desteklenen yerleşik matematiksel işlevler tablosu aşağıdadır.
  
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
  
####  <a name="bk_abs"></a> ABS  
 Belirtilen sayısal ifade (pozitif) mutlak değerini döndürür.  
  
 **Söz dizimi**  
  
```  
ABS (<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `numeric_expression`  
  
   Sayısal bir ifadedir.  
  
  **Dönüş türleri**  
  
  Sayısal bir ifade döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, üç farklı numaralarında ABS işlevi kullanarak sonuçları gösterir.  
  
```  
SELECT ABS(-1) AS abs1, ABS(0) AS abs2, ABS(1) AS abs3 
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{abs1: 1, abs2: 0, abs3: 1}]  
```  
  
####  <a name="bk_acos"></a> ACOS  
 Kosinüsü belirtilen sayısal ifadesidir radyan cinsinden açı döndürür; arkkosinüsünü olarak da adlandırılır.  
  
 **Söz dizimi**  
  
```  
ACOS(<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `numeric_expression`  
  
   Sayısal bir ifadedir.  
  
  **Dönüş türleri**  
  
  Sayısal bir ifade döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek ACOS-1 döndürür.  
  
```  
SELECT ACOS(-1) AS acos 
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"acos": 3.1415926535897931}]  
```  
  
####  <a name="bk_asin"></a> ASIN  
 Açının sinüsü belirtilen sayısal ifadesidir radyan cinsinden döndürür. Bu arksinüsünü olarak da adlandırılır.  
  
 **Söz dizimi**  
  
```  
ASIN(<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `numeric_expression`  
  
   Sayısal bir ifadedir.  
  
  **Dönüş türleri**  
  
  Sayısal bir ifade döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, ASIN-1 döndürür.  
  
```  
SELECT ASIN(-1) AS asin  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"asin": -1.5707963267948966}]  
```  
  
####  <a name="bk_atan"></a> ATAN  
 Tanjantı belirtilen sayısal ifadesidir radyan cinsinden açı döndürür. Bu arktanjantını olarak da adlandırılır.  
  
 **Söz dizimi**  
  
```  
ATAN(<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `numeric_expression`  
  
   Sayısal bir ifadedir.  
  
  **Dönüş türleri**  
  
  Sayısal bir ifade döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, ATAN belirtilen değeri döndürür.  
  
```  
SELECT ATAN(-45.01) AS atan  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"atan": -1.5485826962062663}]  
```  
  
####  <a name="bk_atn2"></a> ATN2  
 Radyan cinsinden ifade edilen x, y / Ark tanjant değerini döndürür.  
  
 **Söz dizimi**  
  
```  
ATN2(<numeric_expression>, <numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `numeric_expression`  
  
   Sayısal bir ifadedir.  
  
  **Dönüş türleri**  
  
  Sayısal bir ifade döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek için belirtilen ATN2 hesaplar x ve y bileşenleri.  
  
```  
SELECT ATN2(35.175643, 129.44) AS atn2  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"atn2": 1.3054517947300646}]  
```  
  
####  <a name="bk_ceiling"></a> TAVAN  
 Büyüktür veya eşittir, belirtilen sayısal ifadenin en küçük tamsayı değerini döndürür.  
  
 **Söz dizimi**  
  
```  
CEILING (<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `numeric_expression`  
  
   Sayısal bir ifadedir.  
  
  **Dönüş türleri**  
  
  Sayısal bir ifade döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, pozitif bir sayısal, negatif ve sıfır değerlerini CEILING işlevi ile gösterilir.  
  
```  
SELECT CEILING(123.45) AS c1, CEILING(-123.45) AS c2, CEILING(0.0) AS c3  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{c1: 124, c2: -123, c3: 0}]  
```  
  
####  <a name="bk_cos"></a> COS  
 Radyan cinsinden belirtilen ifade trigonometrik belirtilen bir açının kosinüsünü döndürür.  
  
 **Söz dizimi**  
  
```  
COS(<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `numeric_expression`  
  
   Sayısal bir ifadedir.  
  
  **Dönüş türleri**  
  
  Sayısal bir ifade döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek belirtilen bir açının COS hesaplar.  
  
```  
SELECT COS(14.78) AS cos  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"cos": -0.59946542619465426}]  
```  
  
####  <a name="bk_cot"></a> COT  
 Trigonometrik belirtilen bir açının kotanjantını radyan cinsinden belirtilen bir sayısal ifade döndürür.  
  
 **Söz dizimi**  
  
```  
COT(<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `numeric_expression`  
  
   Sayısal bir ifadedir.  
  
  **Dönüş türleri**  
  
  Sayısal bir ifade döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek belirtilen bir açının COT hesaplar.  
  
```  
SELECT COT(124.1332) AS cot  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"cot": -0.040311998371148884}]  
```  
  
####  <a name="bk_degrees"></a> DERECE  
 Karşılık gelen açıyı derece için radyan cinsinden belirtilen bir açı cinsinden döndürür.  
  
 **Söz dizimi**  
  
```  
DEGREES (<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `numeric_expression`  
  
   Sayısal bir ifadedir.  
  
  **Dönüş türleri**  
  
  Sayısal bir ifade döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, PI/2 radyan cinsinden açı dereceye döndürür.  
  
```  
SELECT DEGREES(PI()/2) AS degrees  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"degrees": 90}]  
```  
  
####  <a name="bk_floor"></a> KAT  
 Belirtilen sayısal ifade küçük veya eşit en büyük tamsayı döndürür.  
  
 **Söz dizimi**  
  
```  
FLOOR (<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `numeric_expression`  
  
   Sayısal bir ifadedir.  
  
  **Dönüş türleri**  
  
  Sayısal bir ifade döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, pozitif bir sayısal, negatif ve sıfır değerlerini FLOOR işlevi ile gösterilir.  
  
```  
SELECT FLOOR(123.45) AS fl1, FLOOR(-123.45) AS fl2, FLOOR(0.0) AS fl3  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{fl1: 123, fl2: -124, fl3: 0}]  
```  
  
####  <a name="bk_exp"></a> EXP  
 Üstel belirtilen sayısal ifadenin değerini döndürür.  
  
 **Söz dizimi**  
  
```  
EXP (<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `numeric_expression`  
  
   Sayısal bir ifadedir.  
  
  **Dönüş türleri**  
  
  Sayısal bir ifade döndürür.  
  
  **Açıklamalar**  
  
  Sabit **e** (2.718281...) Doğal logaritmanın tabanıdır.  
  
  Bir sayının üssünü sabittir **e** sayının üssü. Örneğin, EXP(1.0) = e ^ 1.0 = 2.71828182845905 ve EXP(10) = e ^ 10 = 22026.4657948067.  
  
  Bir sayının doğal logaritmasını üssü sayıdır kendisini: EXP (günlüğü (n)) = n. Ve üstel bir sayının doğal logaritmasını sayı kendisini: Günlük (EXP (n)) = n.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, bir değişken bildirir ve üstel belirtilen değişkeni (10) değerini döndürür.  
  
```  
SELECT EXP(10) AS exp  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{exp: 22026.465794806718}]  
```  
  
 Aşağıdaki örnek, üstel değeri 20 natural logarithm ve üstel 20 doğal logaritmasını döndürür. Bu işlevler, başka bir ters işlevler olduğundan, dönüş değeri her iki durumda kayan nokta Matematiği için yuvarlama ile 20'dir.  
  
```  
SELECT EXP(LOG(20)) AS exp1, LOG(EXP(20)) AS exp2  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{exp1: 19.999999999999996, exp2: 20}]  
```  
  
####  <a name="bk_log"></a> GÜNLÜK  
 Belirtilen sayısal ifadenin doğal logaritmasını döndürür.  
  
 **Söz dizimi**  
  
```  
LOG (<numeric_expression> [, <base>])  
```  
  
 **Bağımsız Değişkenler**  
  
- `numeric_expression`  
  
   Sayısal bir ifadedir.  
  
- `base`  
  
   Logaritmanın tabanı ayarlayan isteğe bağlı sayısal bağımsız değişken.  
  
  **Dönüş türleri**  
  
  Sayısal bir ifade döndürür.  
  
  **Açıklamalar**  
  
  Varsayılan olarak, LOG() doğal logaritmasını döndürür. Logaritmanın tabanı, isteğe bağlı temel parametresini kullanarak başka bir değere değiştirebilirsiniz.  
  
  Logaritmanın tabanı için doğal logaritmasını olan **e**burada **e** bir Irrational 2.718281828 için yaklaşık olarak eşit sabittir.  
  
  Üstel bir sayının doğal logaritmasını sayıdır kendisini: Günlük (EXP (n)) = n. Ve üstel bir sayının doğal logaritma sayı kendisini: EXP (günlüğü (n)) = n.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, bir değişken bildirir ve belirtilen değişkeni (10) logaritmasını döndürür.  
  
```  
SELECT LOG(10) AS log  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{log: 2.3025850929940459}]  
```  
  
 Aşağıdaki örnek günlük için sayının üssünü hesaplar.  
  
```  
SELECT EXP(LOG(10)) AS expLog  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{expLog: 10.000000000000002}]  
```  
  
####  <a name="bk_log10"></a> LOG10  
 Belirtilen sayısal ifade 10 tabanında logaritmasını döndürür.  
  
 **Söz dizimi**  
  
```  
LOG10 (<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `numeric_expression`  
  
   Sayısal bir ifadedir.  
  
  **Dönüş türleri**  
  
  Sayısal bir ifade döndürür.  
  
  **Açıklamalar**  
  
  LOG10 ve güç işlevleri inversely birbirleriyle ilişkilidir. Örneğin, 10 ^ LOG10(n) = n.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, bir değişken bildirir ve belirtilen değişkeni (100) LOG10 değerini döndürür.  
  
```  
SELECT LOG10(100) AS log10 
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{log10: 2}]  
```  
  
####  <a name="bk_pi"></a> PI  
 PI sayısının sabit değerini döndürür.  
  
 **Söz dizimi**  
  
```  
PI ()  
```  
  
 **Bağımsız Değişkenler**  
  
- `numeric_expression`  
  
   Sayısal bir ifadedir.  
  
  **Dönüş türleri**  
  
  Sayısal bir ifade döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, PI değerini döndürür.  
  
```  
SELECT PI() AS pi 
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"pi": 3.1415926535897931}]  
```  
  
####  <a name="bk_power"></a> GÜÇ  
 Belirtilen güç için belirtilen ifadenin değerini döndürür.  
  
 **Söz dizimi**  
  
```  
POWER (<numeric_expression>, <y>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `numeric_expression`  
  
   Sayısal bir ifadedir.  
  
- `y`  
  
   Hangi yükseltmek güç `numeric_expression`.  
  
  **Dönüş türleri**  
  
  Sayısal bir ifade döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, bir sayı değerini 3 (numarası küp) için yükseltme gösterir.  
  
```  
SELECT POWER(2, 3) AS pow1, POWER(2.5, 3) AS pow2  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{pow1: 8, pow2: 15.625}]  
```  
  
####  <a name="bk_radians"></a> RADYAN CİNSİNDEN  
 Derece sayısal bir ifadenin girildiğinde radyan cinsinden döndürür.  
  
 **Söz dizimi**  
  
```  
RADIANS (<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `numeric_expression`  
  
   Sayısal bir ifadedir.  
  
  **Dönüş türleri**  
  
  Sayısal bir ifade döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, birkaç açıları girdi olarak alır ve bunlara karşılık gelen radian değerler döndürür.  
  
```  
SELECT RADIANS(-45.01) AS r1, RADIANS(-181.01) AS r2, RADIANS(0) AS r3, RADIANS(0.1472738) AS r4, RADIANS(197.1099392) AS r5  
```  
  
  Sonuç kümesini burada verilmiştir.  
  
```  
[{  
       "r1": -0.7855726963226477,  
       "r2": -3.1592204790349356,  
       "r3": 0,  
       "r4": 0.0025704127119236249,  
       "r5": 3.4402174274458375  
   }]  
```  
  
####  <a name="bk_round"></a> YUVARLAK  
 En yakın tamsayı değerine yuvarlanır sayısal bir değer döndürür.  
  
 **Söz dizimi**  
  
```  
ROUND(<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `numeric_expression`  
  
   Sayısal bir ifadedir.  
  
  **Dönüş türleri**  
  
  Sayısal bir ifade döndürür.  
  
  **Açıklamalar**
  
  Gerçekleştirilen yuvarlama işlemi sıfırdan öteye yuvarlama uygulanır Orta izler. Ardından giriş tam olarak iki tam sayılar arasında kalan sayısal bir ifadenin ise sonucu en yakın tamsayı değerini sıfırdan ıraksayarak olacaktır.  
  
  |< numeric_expression >|Yuvarlak|
  |-|-|
  |-6.5000|-7|
  |-0.5|-1|
  |0,5|1|
  |6.5000|7||
  
  **Örnekler**  
  
  Aşağıdaki örnek, aşağıdaki pozitif ve negatif sayıları en yakın tamsayıya yuvarlar.  
  
```  
SELECT ROUND(2.4) AS r1, ROUND(2.6) AS r2, ROUND(2.5) AS r3, ROUND(-2.4) AS r4, ROUND(-2.6) AS r5  
```  
  
  Sonuç kümesini burada verilmiştir.  
  
```  
[{r1: 2, r2: 3, r3: 3, r4: -2, r5: -3}]  
```  
  
####  <a name="bk_sign"></a> OTURUM  
 (+ 1) pozitif, sıfır (0) veya eksi (-1) belirtilen sayısal ifade döndürür.  
  
 **Söz dizimi**  
  
```  
SIGN(<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `numeric_expression`  
  
   Sayısal bir ifadedir.  
  
  **Dönüş türleri**  
  
  Sayısal bir ifade döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnekte, -2 ' 2'ye sayıların işaretini değerleri döndürür.  
  
```  
SELECT SIGN(-2) AS s1, SIGN(-1) AS s2, SIGN(0) AS s3, SIGN(1) AS s4, SIGN(2) AS s5  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{s1: -1, s2: -1, s3: 0, s4: 1, s5: 1}]  
```  
  
####  <a name="bk_sin"></a> SIN  
 Radyan cinsinden belirtilen ifade trigonometrik belirtilen bir açının sinüsünü döndürür.  
  
 **Söz dizimi**  
  
```  
SIN(<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `numeric_expression`  
  
   Sayısal bir ifadedir.  
  
  **Dönüş türleri**  
  
  Sayısal bir ifade döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek belirtilen bir açının SIN hesaplar.  
  
```  
SELECT SIN(45.175643) AS sin  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"sin": 0.929607286611012}]  
```  
  
####  <a name="bk_sqrt"></a> SQRT  
 Belirtilen sayısal değerinin kare kökünü döndürür.  
  
 **Söz dizimi**  
  
```  
SQRT(<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `numeric_expression`  
  
   Sayısal bir ifadedir.  
  
  **Dönüş türleri**  
  
  Sayısal bir ifade döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek sayılar 1-3 kareköklerini döndürür.  
  
```  
SELECT SQRT(1) AS s1, SQRT(2.0) AS s2, SQRT(3) AS s3  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{s1: 1, s2: 1.4142135623730952, s3: 1.7320508075688772}]  
```  
  
####  <a name="bk_square"></a> KARE  
 Belirtilen bir sayısal değer karesini döndürür.  
  
 **Söz dizimi**  
  
```  
SQUARE(<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `numeric_expression`  
  
   Sayısal bir ifadedir.  
  
  **Dönüş türleri**  
  
  Sayısal bir ifade döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, 1-3 sayıların kareler döndürür.  
  
```  
SELECT SQUARE(1) AS s1, SQUARE(2.0) AS s2, SQUARE(3) AS s3  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{s1: 1, s2: 4, s3: 9}]  
```  
  
####  <a name="bk_tan"></a> TAN  
 Radyan cinsinden belirtilen ifadesi belirtilen bir açının tanjantını döndürür.  
  
 **Söz dizimi**  
  
```  
TAN (<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `numeric_expression`  
  
   Sayısal bir ifadedir.  
  
  **Dönüş türleri**  
  
  Sayısal bir ifade döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, PI () tanjantını hesaplar / 2.  
  
```  
SELECT TAN(PI()/2) AS tan 
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"tan": 16331239353195370 }]  
```  
  
####  <a name="bk_trunc"></a> TRUNC  
 En yakın tamsayı değerine kesilmiş sayısal bir değer döndürür.  
  
 **Söz dizimi**  
  
```  
TRUNC(<numeric_expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `numeric_expression`  
  
   Sayısal bir ifadedir.  
  
  **Dönüş türleri**  
  
  Sayısal bir ifade döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, aşağıdaki pozitif ve negatif sayıları en yakın tamsayı değerine keser.  
  
```  
SELECT TRUNC(2.4) AS t1, TRUNC(2.6) AS t2, TRUNC(2.5) AS t3, TRUNC(-2.4) AS t4, TRUNC(-2.6) AS t5  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{t1: 2, t2: 2, t3: 2, t4: -2, t5: -2}]  
```

## <a id="type-checking-functions"></a>Tür denetimini işlevleri

Bir SQL sorgusu içindeki bir ifadenin türünü kontrol tür denetimi işlevlerini sağlar. Değişken veya bilinmeyen olduklarında özellikleri içinde hareket halindeyken öğeleri türlerini belirlemek için tür denetimi işlevlerini kullanabilirsiniz. Desteklenen yerleşik tür denetimi işlevler tablosu şu şekildedir:

Tür denetimini karşı giriş değerleri aşağıdaki işlevleri destekler ve her bir Boole değeri döndürür.  
  
||||  
|-|-|-|  
|[IS_ARRAY](#bk_is_array)|[IS_BOOL](#bk_is_bool)|[IS_DEFINED](#bk_is_defined)|  
|[IS_NULL](#bk_is_null)|[IS_NUMBER](#bk_is_number)|[IS_OBJECT](#bk_is_object)|  
|[IS_PRIMITIVE](#bk_is_primitive)|[IS_STRING](#bk_is_string)||  
  
####  <a name="bk_is_array"></a> IS_ARRAY  
 Belirtilen ifade türü bir dizi olup olmadığını gösteren bir Boole değeri döndürür.  
  
 **Söz dizimi**  
  
```  
IS_ARRAY(<expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `expression`  
  
   Herhangi bir geçerli ifade var.  
  
  **Dönüş türleri**  
  
  Bir Boolean ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek JSON Boolean, sayı, dize, null Nesne, nesne, dizi ve IS_ARRAY işlevi kullanarak tanımlanmamış türler denetler.  
  
```  
SELECT   
 IS_ARRAY(true) AS isArray1,   
 IS_ARRAY(1) AS isArray2,  
 IS_ARRAY("value") AS isArray3,  
 IS_ARRAY(null) AS isArray4,  
 IS_ARRAY({prop: "value"}) AS isArray5,   
 IS_ARRAY([1, 2, 3]) AS isArray6,  
 IS_ARRAY({prop: "value"}.prop2) AS isArray7  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"isArray1":false,"isArray2":false,"isArray3":false,"isArray4":false,"isArray5":false,"isArray6":true,"isArray7":false}]
```  
  
####  <a name="bk_is_bool"></a> IS_BOOL  
 Belirtilen ifade türünü bir Boole değeri olup olmadığını gösteren bir Boole değeri döndürür.  
  
 **Söz dizimi**  
  
```  
IS_BOOL(<expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `expression`  
  
   Herhangi bir geçerli ifade var.  
  
  **Dönüş türleri**  
  
  Bir Boolean ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek JSON Boolean, sayı, dize, null Nesne, nesne, dizi ve IS_BOOL işlevi kullanarak tanımlanmamış türler denetler.  
  
```  
SELECT   
    IS_BOOL(true) AS isBool1,   
    IS_BOOL(1) AS isBool2,  
    IS_BOOL("value") AS isBool3,   
    IS_BOOL(null) AS isBool4,  
    IS_BOOL({prop: "value"}) AS isBool5,   
    IS_BOOL([1, 2, 3]) AS isBool6,  
    IS_BOOL({prop: "value"}.prop2) AS isBool7  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"isBool1":true,"isBool2":false,"isBool3":false,"isBool4":false,"isBool5":false,"isBool6":false,"isBool7":false}]
```  
  
####  <a name="bk_is_defined"></a> IS_DEFINED  
 Özellik değeri atanıp atanmadığını gösteren bir Boole değeri döndürür.  
  
 **Söz dizimi**  
  
```  
IS_DEFINED(<expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `expression`  
  
   Herhangi bir geçerli ifade var.  
  
  **Dönüş türleri**  
  
  Bir Boolean ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, bir özelliği belirtilen JSON belgesi içinde varlığını denetler. İlk "a" var, ancak "b" eksik olduğundan ikinci false döndürürse bu yana true döndürür.  
  
```  
SELECT IS_DEFINED({ "a" : 5 }.a) AS isDefined1, IS_DEFINED({ "a" : 5 }.b) AS isDefined2 
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"isDefined1":true,"isDefined2":false}]  
```  
  
####  <a name="bk_is_null"></a> IS_NULL  
 Belirtilen ifadenin türü null olup olmadığını gösteren bir Boole değeri döndürür.  
  
 **Söz dizimi**  
  
```  
IS_NULL(<expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `expression`  
  
   Herhangi bir geçerli ifade var.  
  
  **Dönüş türleri**  
  
  Bir Boolean ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek JSON Boolean, sayı, dize, null Nesne, nesne, dizi ve IS_NULL işlevi kullanarak tanımlanmamış türler denetler.  
  
```  
SELECT   
    IS_NULL(true) AS isNull1,   
    IS_NULL(1) AS isNull2,  
    IS_NULL("value") AS isNull3,   
    IS_NULL(null) AS isNull4,  
    IS_NULL({prop: "value"}) AS isNull5,   
    IS_NULL([1, 2, 3]) AS isNull6,  
    IS_NULL({prop: "value"}.prop2) AS isNull7  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"isNull1":false,"isNull2":false,"isNull3":false,"isNull4":true,"isNull5":false,"isNull6":false,"isNull7":false}]
```  
  
####  <a name="bk_is_number"></a> IS_NUMBER  
 Belirtilen ifade türünü bir sayı olup olmadığını gösteren bir Boole değeri döndürür.  
  
 **Söz dizimi**  
  
```  
IS_NUMBER(<expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `expression`  
  
   Herhangi bir geçerli ifade var.  
  
  **Dönüş türleri**  
  
  Bir Boolean ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek JSON Boolean, sayı, dize, null Nesne, nesne, dizi ve IS_NULL işlevi kullanarak tanımlanmamış türler denetler.  
  
```  
SELECT   
    IS_NUMBER(true) AS isNum1,   
    IS_NUMBER(1) AS isNum2,  
    IS_NUMBER("value") AS isNum3,   
    IS_NUMBER(null) AS isNum4,  
    IS_NUMBER({prop: "value"}) AS isNum5,   
    IS_NUMBER([1, 2, 3]) AS isNum6,  
    IS_NUMBER({prop: "value"}.prop2) AS isNum7  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"isNum1":false,"isNum2":true,"isNum3":false,"isNum4":false,"isNum5":false,"isNum6":false,"isNum7":false}]  
```  
  
####  <a name="bk_is_object"></a> IS_OBJECT  
 Belirtilen ifade türünü bir JSON nesnesi olup olmadığını gösteren bir Boole değeri döndürür.  
  
 **Söz dizimi**  
  
```  
IS_OBJECT(<expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `expression`  
  
   Herhangi bir geçerli ifade var.  
  
  **Dönüş türleri**  
  
  Bir Boolean ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek JSON Boolean, sayı, dize, null Nesne, nesne, dizi ve IS_OBJECT işlevi kullanarak tanımlanmamış türler denetler.  
  
```  
SELECT   
    IS_OBJECT(true) AS isObj1,   
    IS_OBJECT(1) AS isObj2,  
    IS_OBJECT("value") AS isObj3,   
    IS_OBJECT(null) AS isObj4,  
    IS_OBJECT({prop: "value"}) AS isObj5,   
    IS_OBJECT([1, 2, 3]) AS isObj6,  
    IS_OBJECT({prop: "value"}.prop2) AS isObj7  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"isObj1":false,"isObj2":false,"isObj3":false,"isObj4":false,"isObj5":true,"isObj6":false,"isObj7":false}]
```  
  
####  <a name="bk_is_primitive"></a> IS_PRIMITIVE  
 Belirtilen ifadenin türü basit bir tür olup olmadığını gösteren bir Boole değeri döndürür (, Boole, sayısal ya da boş dize).  
  
 **Söz dizimi**  
  
```  
IS_PRIMITIVE(<expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `expression`  
  
   Herhangi bir geçerli ifade var.  
  
  **Dönüş türleri**  
  
  Bir Boolean ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek JSON Boolean, sayı, dize, null Nesne, nesne, dizi ve IS_PRIMITIVE işlevi kullanarak tanımlanmamış türler denetler.  
  
```  
SELECT   
           IS_PRIMITIVE(true) AS isPrim1,   
           IS_PRIMITIVE(1) AS isPrim2,  
           IS_PRIMITIVE("value") AS isPrim3,   
           IS_PRIMITIVE(null) AS isPrim4,  
           IS_PRIMITIVE({prop: "value"}) AS isPrim5,   
           IS_PRIMITIVE([1, 2, 3]) AS isPrim6,  
           IS_PRIMITIVE({prop: "value"}.prop2) AS isPrim7  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"isPrim1": true, "isPrim2": true, "isPrim3": true, "isPrim4": true, "isPrim5": false, "isPrim6": false, "isPrim7": false}]  
```  
  
####  <a name="bk_is_string"></a> IS_STRING  
 Belirtilen ifadenin türü dize olup olmadığını gösteren bir Boole değeri döndürür.  
  
 **Söz dizimi**  
  
```  
IS_STRING(<expression>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `expression`  
  
   Herhangi bir geçerli ifade var.  
  
  **Dönüş türleri**  
  
  Bir Boolean ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek JSON Boolean, sayı, dize, null Nesne, nesne, dizi ve IS_STRING işlevi kullanarak tanımlanmamış türler denetler.  
  
```  
SELECT   
       IS_STRING(true) AS isStr1,   
       IS_STRING(1) AS isStr2,  
       IS_STRING("value") AS isStr3,   
       IS_STRING(null) AS isStr4,  
       IS_STRING({prop: "value"}) AS isStr5,   
       IS_STRING([1, 2, 3]) AS isStr6,  
       IS_STRING({prop: "value"}.prop2) AS isStr7  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"isStr1":false,"isStr2":false,"isStr3":true,"isStr4":false,"isStr5":false,"isStr6":false,"isStr7":false}] 
```  

## <a id="string-functions"></a>Dize işlevleri

Aşağıdaki skaler İşlevler, bir dize giriş değeri bir işlem gerçekleştirmek ve bir dize, sayısal veya Boolean değeri döndürür:
  
||||  
|-|-|-|  
|[CONCAT](#bk_concat)|[İÇERİR](#bk_contains)|[ENDSWITH](#bk_endswith)|  
|[INDEX_OF](#bk_index_of)|[SOL](#bk_left)|[UZUNLUĞU](#bk_length)|  
|[DAHA DÜŞÜK](#bk_lower)|[LTRIM](#bk_ltrim)|[DEĞİŞTİR](#bk_replace)|  
|[ÇOĞALTILAN](#bk_replicate)|[GERİYE DOĞRU](#bk_reverse)|[SAĞ](#bk_right)|  
|[RTRIM](#bk_rtrim)|[STARTSWITH](#bk_startswith)|[StringToArray](#bk_stringtoarray)|
|[StringToBoolean](#bk_stringtoboolean)|[StringToNull](#bk_stringtonull)|[StringToNumber](#bk_stringtonumber)|
|[StringToObject](#bk_stringtoobject)|[ALT DİZE](#bk_substring)|[ToString](#bk_tostring)|
|[KIRPMA](#bk_trim)|[ÜST](#bk_upper)||
  
####  <a name="bk_concat"></a> CONCAT  
 İki veya daha fazla dize değerlerini birleştirirken sonucu olan bir dize döndürür.  
  
 **Söz dizimi**  
  
```  
CONCAT(<str_expr>, <str_expr> [, <str_expr>])  
```  
  
 **Bağımsız Değişkenler**  
  
- `str_expr`  
  
   Herhangi bir geçerli dize ifade var.  
  
  **Dönüş türleri**  
  
  Bir dize ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, belirtilen değerler birleştirilmiş dizeyi döndürür.  
  
```  
SELECT CONCAT("abc", "def") AS concat  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"concat": "abcdef"}  
```  
  
####  <a name="bk_contains"></a> İÇERİR  
 Döndürür bir Boolean gösteren ikinci ilk dize ifade olup olmadığını içerir.  
  
 **Söz dizimi**  
  
```  
CONTAINS(<str_expr>, <str_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `str_expr`  
  
   Herhangi bir geçerli dize ifade var.  
  
  **Dönüş türleri**  
  
  Bir Boolean ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, "abc" "ab" ve "d" içerir, denetler.  
  
```  
SELECT CONTAINS("abc", "ab") AS c1, CONTAINS("abc", "d") AS c2 
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"c1": true, "c2": false}]  
```  
  
####  <a name="bk_endswith"></a> ENDSWITH  
 Boole döndürüp döndüremeyeceğini belirten döndürür ilk dize ifade olup olmadığını ve ikinci sona erer.  
  
 **Söz dizimi**  
  
```  
ENDSWITH(<str_expr>, <str_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `str_expr`  
  
   Herhangi bir geçerli dize ifade var.  
  
  **Dönüş türleri**  
  
  Bir Boolean ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, "abc", "b" ve "bc" ile biten döndürür.  
  
```  
SELECT ENDSWITH("abc", "b") AS e1, ENDSWITH("abc", "bc") AS e2 
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"e1": false, "e2": true}]  
```  
  
####  <a name="bk_index_of"></a> INDEX_OF  
 İkinci dizenin başlangıç konumunu döndürür dize bulunamazsa, ilk belirtilen dize ifadesi veya -1 içindeki ifadenin dize.  
  
 **Söz dizimi**  
  
```  
INDEX_OF(<str_expr>, <str_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `str_expr`  
  
   Herhangi bir geçerli dize ifade var.  
  
  **Dönüş türleri**  
  
  Sayısal bir ifade döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, "abc" içindeki çeşitli alt dizeler dizinini döndürür.  
  
```  
SELECT INDEX_OF("abc", "ab") AS i1, INDEX_OF("abc", "b") AS i2, INDEX_OF("abc", "c") AS i3 
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"i1": 0, "i2": 1, "i3": -1}]  
```  
  
####  <a name="bk_left"></a> SOL  
 Belirtilen sayıda karakteri içeren bir dize sol bölümünü döndürür.  
  
 **Söz dizimi**  
  
```  
LEFT(<str_expr>, <num_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `str_expr`  
  
   Herhangi bir geçerli dize ifade var.  
  
- `num_expr`  
  
   Geçerli bir sayısal ifade var.  
  
  **Dönüş türleri**  
  
  Bir dize ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, çeşitli uzunluğu değerler için sol "abc" bölümünü döndürür.  
  
```  
SELECT LEFT("abc", 1) AS l1, LEFT("abc", 2) AS l2 
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"l1": "a", "l2": "ab"}]  
```  
  
####  <a name="bk_length"></a> UZUNLUĞU  
 Belirtilen dize ifadesinin karakter sayısını döndürür.  
  
 **Söz dizimi**  
  
```  
LENGTH(<str_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `str_expr`  
  
   Herhangi bir geçerli dize ifade var.  
  
  **Dönüş türleri**  
  
  Bir dize ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek bir dizenin uzunluğunu döndürür.  
  
```  
SELECT LENGTH("abc") AS len 
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"len": 3}]  
```  
  
####  <a name="bk_lower"></a> DAHA DÜŞÜK  
 Büyük harf karakter verileri küçük harfe dönüştürmenin sonra bir dize ifadesi döndürür.  
  
 **Söz dizimi**  
  
```  
LOWER(<str_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `str_expr`  
  
   Herhangi bir geçerli dize ifade var.  
  
  **Dönüş türleri**  
  
  Bir dize ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek sorguda DÜŞÜREBİLİRSİNİZ kullanmayı gösterir.  
  
```  
SELECT LOWER("Abc") AS lower
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"lower": "abc"}]  
  
```  
  
####  <a name="bk_ltrim"></a> LTRIM  
 Baştaki boşluklar kaldırdıktan sonra bir dize ifadesi döndürür.  
  
 **Söz dizimi**  
  
```  
LTRIM(<str_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `str_expr`  
  
   Herhangi bir geçerli dize ifade var.  
  
  **Dönüş türleri**  
  
  Bir dize ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, bir sorgu içinde LTRIM kullanmayı gösterir.  
  
```  
SELECT LTRIM("  abc") AS l1, LTRIM("abc") AS l2, LTRIM("abc   ") AS l3 
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"l1": "abc", "l2": "abc", "l3": "abc   "}]  
```  
  
####  <a name="bk_replace"></a> DEĞİŞTİR  
 Belirtilen dize değeri tüm oluşumlarını başka bir dize değeri ile değiştirir.  
  
 **Söz dizimi**  
  
```  
REPLACE(<str_expr>, <str_expr>, <str_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `str_expr`  
  
   Herhangi bir geçerli dize ifade var.  
  
  **Dönüş türleri**  
  
  Bir dize ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, bir sorgu Değiştir kullanmayı gösterir.  
  
```  
SELECT REPLACE("This is a Test", "Test", "desk") AS replace 
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"replace": "This is a desk"}]  
```  
  
####  <a name="bk_replicate"></a> ÇOĞALTMA  
 Bir dize değeri, belirtilen sayıda yineler.  
  
 **Söz dizimi**  
  
```  
REPLICATE(<str_expr>, <num_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `str_expr`  
  
   Herhangi bir geçerli dize ifade var.  
  
- `num_expr`  
  
   Geçerli bir sayısal ifade var. Num_expr negatif veya sonlu olmayan ise, sonuç tanımsızdır.

  > [!NOTE]
  > Sonucun en fazla uzunluğu 10.000 karakterden yani (length(str_expr) * num_expr) < = 10.000.
  
  **Dönüş türleri**  
  
  Bir dize ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, bir sorgu çoğaltma kullanmayı gösterir.  
  
```  
SELECT REPLICATE("a", 3) AS replicate  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"replicate": "aaa"}]  
```  
  
####  <a name="bk_reverse"></a> GERİYE DOĞRU  
 Bir dize değerinin ters sırada döndürür.  
  
 **Söz dizimi**  
  
```  
REVERSE(<str_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `str_expr`  
  
   Herhangi bir geçerli dize ifade var.  
  
  **Dönüş türleri**  
  
  Bir dize ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, bir sorgu ters kullanmayı gösterir.  
  
```  
SELECT REVERSE("Abc") AS reverse  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"reverse": "cbA"}]  
```  
  
####  <a name="bk_right"></a> SAĞ  
 Belirtilen sayıda karakteri içeren bir dize sağ bölümünü döndürür.  
  
 **Söz dizimi**  
  
```  
RIGHT(<str_expr>, <num_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `str_expr`  
  
   Herhangi bir geçerli dize ifade var.  
  
- `num_expr`  
  
   Geçerli bir sayısal ifade var.  
  
  **Dönüş türleri**  
  
  Bir dize ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, çeşitli uzunluğu değerleri için "abc" sağ bölümünü döndürür.  
  
```  
SELECT RIGHT("abc", 1) AS r1, RIGHT("abc", 2) AS r2 
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"r1": "c", "r2": "bc"}]  
```  
  
####  <a name="bk_rtrim"></a> RTRIM  
 Sonundaki boşlukları kaldırdıktan sonra bir dize ifadesi döndürür.  
  
 **Söz dizimi**  
  
```  
RTRIM(<str_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `str_expr`  
  
   Herhangi bir geçerli dize ifade var.  
  
  **Dönüş türleri**  
  
  Bir dize ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, bir sorgu içinde RTRIM kullanmayı gösterir.  
  
```  
SELECT RTRIM("  abc") AS r1, RTRIM("abc") AS r2, RTRIM("abc   ") AS r3  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"r1": "   abc", "r2": "abc", "r3": "abc"}]  
```  
  
####  <a name="bk_startswith"></a> STARTSWITH  
 Boole döndürüp döndüremeyeceğini belirten döndürür ilk dize ifade olup olmadığını ve ikinci başlatır.  
  
 **Söz dizimi**  
  
```  
STARTSWITH(<str_expr>, <str_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `str_expr`  
  
   Herhangi bir geçerli dize ifade var.  
  
  **Dönüş türleri**  
  
  Bir Boolean ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, "abc" dize "b" ile başlayıp başlamadığını denetler ve "a".  
  
```  
SELECT STARTSWITH("abc", "b") AS s1, STARTSWITH("abc", "a") AS s2  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"s1": false, "s2": true}]  
```  

  ####  <a name="bk_stringtoarray"></a> StringToArray  
 Bir diziye çevrilmiş bir ifade döndürür. İfade tercüme edilemez, tanımsız döndürür.  
  
 **Söz dizimi**  
  
```  
StringToArray(<expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `expr`  
  
   Bir JSON dizisi ifade olarak değerlendirilebilmesi için geçerli bir skaler ifade var. İç içe geçmiş dize değerleri geçerli olması için çift tırnak işareti yazılması gerektiğini unutmayın. JSON biçimi hakkında daha fazla bilgi için bkz: [json.org](https://json.org/)
  
  **Dönüş türleri**  
  
  Bir dizi ifadesi döndürür ya da tanımlanmamış.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, StringToArray farklı türleri arasında nasıl davranacağını gösterir. 
  
 Geçerli giriş ile örnekleri aşağıda verilmiştir.

```
SELECT 
    StringToArray('[]') AS a1, 
    StringToArray("[1,2,3]") AS a2,
    StringToArray("[\"str\",2,3]") AS a3,
    StringToArray('[["5","6","7"],["8"],["9"]]') AS a4,
    StringToArray('[1,2,3, "[4,5,6]",[7,8]]') AS a5
```

Sonuç kümesini burada verilmiştir.

```
[{"a1": [], "a2": [1,2,3], "a3": ["str",2,3], "a4": [["5","6","7"],["8"],["9"]], "a5": [1,2,3,"[4,5,6]",[7,8]]}]
```

Geçersiz giriş örneği verilmiştir. 
   
 Dizi içinde tek tırnak işareti, geçerli bir JSON değil.
Bir sorgu içinde geçerli olsa da, geçerli dizilere ayrıştırılamıyor. Dizi dize içindeki dizeler ya da konulmalıdır "[\\"\\"]" ya da çevreleyen tırnak tek ' [""]'.

```
SELECT
    StringToArray("['5','6','7']")
```

Sonuç kümesini burada verilmiştir.

```
[{}]
```

Geçersiz girdi örnekleri aşağıda verilmiştir.
   
 Geçen ifadeye bir JSON dizisi olarak ayrıştırılacak; aşağıdaki dizi türü ve dolayısıyla tanımlanmamış döndürmek için değerlendirmez.
   
```
SELECT
    StringToArray("["),
    StringToArray("1"),
    StringToArray(NaN),
    StringToArray(false),
    StringToArray(undefined)
```

Sonuç kümesini burada verilmiştir.

```
[{}]
```

####  <a name="bk_stringtoboolean"></a> StringToBoolean  
 İfade çevrilmiş bir Boole değeri döndürür. İfade tercüme edilemez, tanımsız döndürür.  
  
 **Söz dizimi**  
  
```  
StringToBoolean(<expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `expr`  
  
   Bir Boole ifadesi değerlendirilebilmesi için geçerli bir skaler ifade var.  
  
  **Dönüş türleri**  
  
  Bir Boolean ifadesi döndürür ya da tanımlanmamış.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, StringToBoolean farklı türleri arasında nasıl davranacağını gösterir. 
 
 Geçerli giriş ile örnekleri aşağıda verilmiştir.

Boşluk yalnızca önce veya sonra "true"/ "false" izin verilmiyor.

```  
SELECT 
    StringToBoolean("true") AS b1, 
    StringToBoolean("    false") AS b2,
    StringToBoolean("false    ") AS b3
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"b1": true, "b2": false, "b3": false}]
```  

Geçersiz giriş ile örnekleri aşağıda verilmiştir.

 Boole değerleri büyük/küçük harfe duyarlıdır ve tüm küçük harfle ör "true" ve "false" için yazılmış olmalıdır.

```  
SELECT 
    StringToBoolean("TRUE"),
    StringToBoolean("False")
```  

Sonuç kümesini burada verilmiştir.  
  
```  
[{}]
``` 

Geçen ifadeye bir Boole ifadesi ayrıştırılacak; Boolean türü ve dolayısıyla tanımlanmamış döndürmek için bu girişlerin değerlendirmez.

```  
SELECT 
    StringToBoolean("null"),
    StringToBoolean(undefined),
    StringToBoolean(NaN), 
    StringToBoolean(false), 
    StringToBoolean(true)
```  

Sonuç kümesini burada verilmiştir.  
  
```  
[{}]
```  

####  <a name="bk_stringtonull"></a> StringToNull  
 Null çevrilmiş bir ifade döndürür. İfade tercüme edilemez, tanımsız döndürür.  
  
 **Söz dizimi**  
  
```  
StringToNull(<expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `expr`  
  
   Boş bir ifade olarak değerlendirilebilmesi için geçerli bir skaler ifade var.
  
  **Dönüş türleri**  
  
  Boş bir ifade döndürür ya da tanımlanmamış.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, StringToNull farklı türleri arasında nasıl davranacağını gösterir. 

Geçerli giriş ile örnekleri aşağıda verilmiştir.

 Boşluk yalnızca önce veya sonra "null" izin verilmiyor.

```  
SELECT 
    StringToNull("null") AS n1, 
    StringToNull("  null ") AS n2,
    IS_NULL(StringToNull("null   ")) AS n3
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"n1": null, "n2": null, "n3": true}]
```  

Geçersiz giriş ile örnekleri aşağıda verilmiştir.

Null büyük/küçük harfe duyarlıdır ve tüm küçük harfle "null" yani yazılmış olmalıdır.

```  
SELECT    
    StringToNull("NULL"),
    StringToNull("Null")
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{}]
```  

Geçen ifadeye null bir ifade olarak ayrıştırılacak; null yazın ve bu nedenle tanımlanmamış döndürmek için bu girişlerin değerlendirmez.

```  
SELECT    
    StringToNull("true"), 
    StringToNull(false), 
    StringToNull(undefined),
    StringToNull(NaN) 
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{}]
```  

####  <a name="bk_stringtonumber"></a> StringToNumber  
 Çevrilmiş bir sayıyı ifade döndürür. İfade tercüme edilemez, tanımsız döndürür.  
  
 **Söz dizimi**  
  
```  
StringToNumber(<expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `expr`  
  
   JSON sayı ifade olarak değerlendirilebilmesi için geçerli bir skaler ifade var. JSON sayı bir tamsayı veya kayan nokta olmalıdır. JSON biçimi hakkında daha fazla bilgi için bkz: [json.org](https://json.org/)  
  
  **Dönüş türleri**  
  
  Sayı bir ifade döndürür ya da tanımlanmamış.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, StringToNumber farklı türleri arasında nasıl davranacağını gösterir. 

Boşluk yalnızca önce veya sonra sayısı izin verilir.

```  
SELECT 
    StringToNumber("1.000000") AS num1, 
    StringToNumber("3.14") AS num2,
    StringToNumber("   60   ") AS num3, 
    StringToNumber("-1.79769e+308") AS num4
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
{{"num1": 1, "num2": 3.14, "num3": 60, "num4": -1.79769e+308}}
```  

JSON biçiminde geçerli bir sayı olmalıdır ya da bir tamsayı veya kayan olması nokta sayısı.

```  
SELECT   
    StringToNumber("0xF")
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
{{}}
```  

Geçen ifadeye sayı bir ifade olarak ayrıştırılacak; Numarasını yazın ve bu nedenle tanımlanmamış döndürmek için bu girişlerin değerlendirmez. 

```  
SELECT 
    StringToNumber("99     54"),   
    StringToNumber(undefined),
    StringToNumber("false"),
    StringToNumber(false),
    StringToNumber(" "),
    StringToNumber(NaN)
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
{{}}
```  

####  <a name="bk_stringtoobject"></a> StringToObject  
 Bir nesneye çevrilmiş bir ifade döndürür. İfade tercüme edilemez, tanımsız döndürür.  
  
 **Söz dizimi**  
  
```  
StringToObject(<expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `expr`  
  
   Bir JSON nesne ifadesi değerlendirilebilmesi için geçerli bir skaler ifade var. İç içe geçmiş dize değerleri geçerli olması için çift tırnak işareti yazılması gerektiğini unutmayın. JSON biçimi hakkında daha fazla bilgi için bkz: [json.org](https://json.org/)  
  
  **Dönüş türleri**  
  
  Bir nesne ifadesi döndürür ya da tanımlanmamış.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, StringToObject farklı türleri arasında nasıl davranacağını gösterir. 
  
 Geçerli giriş ile örnekleri aşağıda verilmiştir.

``` 
SELECT 
    StringToObject("{}") AS obj1, 
    StringToObject('{"A":[1,2,3]}') AS obj2,
    StringToObject('{"B":[{"b1":[5,6,7]},{"b2":8},{"b3":9}]}') AS obj3, 
    StringToObject("{\"C\":[{\"c1\":[5,6,7]},{\"c2\":8},{\"c3\":9}]}") AS obj4
``` 

Sonuç kümesini burada verilmiştir.

```
[{"obj1": {}, 
  "obj2": {"A": [1,2,3]}, 
  "obj3": {"B":[{"b1":[5,6,7]},{"b2":8},{"b3":9}]},
  "obj4": {"C":[{"c1":[5,6,7]},{"c2":8},{"c3":9}]}}]
```

 Geçersiz giriş ile örnekleri aşağıda verilmiştir.
Bir sorgu içinde geçerli olsa da, geçerli nesnelere ayrıştırılamıyor. Nesnenin dize içindeki dizeler ya da konulmalıdır "{\\" bir\\":\\" str\\"}" veya çevreleyen tırnak tek ' {"a": "dizesi"}'.

Özellik adlarının çevreleyen tırnak geçerli JSON değil.

``` 
SELECT 
    StringToObject("{'a':[1,2,3]}")
```

Sonuç kümesini burada verilmiştir.

```  
[{}]
```  

Özellik adlarının çevreleyen tırnak işaretleri olmadan geçerli bir JSON değil.

``` 
SELECT 
    StringToObject("{a:[1,2,3]}")
```

Sonuç kümesini burada verilmiştir.

```  
[{}]
``` 

Geçersiz giriş ile örnekleri aşağıda verilmiştir.

 Geçen ifadeye bir JSON nesnesi olarak ayrıştırılacak; nesne türünü ve bu nedenle tanımlanmamış döndürmek için bu girişlerin değerlendirmez.

``` 
SELECT 
    StringToObject("}"),
    StringToObject("{"),
    StringToObject("1"),
    StringToObject(NaN), 
    StringToObject(false), 
    StringToObject(undefined)
``` 
 
 Sonuç kümesini burada verilmiştir.

```
[{}]
```

####  <a name="bk_substring"></a> ALT DİZE  
 Belirtilen karakterin sıfır tabanlı konumunda başlayan bir dize ifadesi bölümünü döndürür ve belirtilen uzunlukta veya dizenin sonuna kadar devam eder.  
  
 **Söz dizimi**  
  
```  
SUBSTRING(<str_expr>, <num_expr>, <num_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `str_expr`  
  
   Herhangi bir geçerli dize ifade var.  
  
- `num_expr`  
  
   Başlangıç ve bitiş karakteri belirtmek için geçerli bir sayısal ifade var.    
  
  **Dönüş türleri**  
  
  Bir dize ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, "abc" alt 1 ve 1 karakter uzunluğunu başlangıç döndürür.  
  
```  
SELECT SUBSTRING("abc", 1, 1) AS substring  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"substring": "b"}]  
```  
####  <a name="bk_tostring"></a> ToString  
 Skaler ifade bir dize gösterimini döndürür. 
  
 **Söz dizimi**  
  
```  
ToString(<expr>)
```  
  
 **Bağımsız Değişkenler**  
  
- `expr`  
  
   Geçerli bir skaler ifade var.  
  
  **Dönüş türleri**  
  
  Bir dize ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, ToString farklı türleri arasında nasıl davranacağını gösterir.   
  
```  
SELECT 
    ToString(1.0000) AS str1, 
    ToString("Hello World") AS str2, 
    ToString(NaN) AS str3, 
    ToString(Infinity) AS str4,
    ToString(IS_STRING(ToString(undefined))) AS str5, 
    ToString(0.1234) AS str6, 
    ToString(false) AS str7, 
    ToString(undefined) AS str8
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"str1": "1", "str2": "Hello World", "str3": "NaN", "str4": "Infinity", "str5": "false", "str6": "0.1234", "str7": "false"}]  
```  
 Şu giriş verilen:
```  
{"Products":[{"ProductID":1,"Weight":4,"WeightUnits":"lb"},{"ProductID":2,"Weight":32,"WeightUnits":"kg"},{"ProductID":3,"Weight":400,"WeightUnits":"g"},{"ProductID":4,"Weight":8999,"WeightUnits":"mg"}]}
```    
 Aşağıdaki örnek, diğer CONCAT gibi dize işlevlerle ToString'ın nasıl kullanılabileceğini gösterir.   
 
```  
SELECT 
CONCAT(ToString(p.Weight), p.WeightUnits) 
FROM p in c.Products 
```  

Sonuç kümesini burada verilmiştir.  
  
```  
[{"$1":"4lb" },
{"$1":"32kg"},
{"$1":"400g" },
{"$1":"8999mg" }]

```  
Şu giriş verilir.
```
{"id":"08259","description":"Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX","nutrients":[{"id":"305","description":"Caffeine","units":"mg"},{"id":"306","description":"Cholesterol, HDL","nutritionValue":30,"units":"mg"},{"id":"307","description":"Sodium, NA","nutritionValue":612,"units":"mg"},{"id":"308","description":"Protein, ABP","nutritionValue":60,"units":"mg"},{"id":"309","description":"Zinc, ZN","nutritionValue":null,"units":"mg"}]}
```
Aşağıdaki örnek, diğer değiştirme gibi dize işlevlerle ToString'ın nasıl kullanılabileceğini gösterir.   
```
SELECT 
    n.id AS nutrientID,
    REPLACE(ToString(n.nutritionValue), "6", "9") AS nutritionVal
FROM food 
JOIN n IN food.nutrients
```
Sonuç kümesini burada verilmiştir.  
 ```
[{"nutrientID":"305"},
{"nutrientID":"306","nutritionVal":"30"},
{"nutrientID":"307","nutritionVal":"912"},
{"nutrientID":"308","nutritionVal":"90"},
{"nutrientID":"309","nutritionVal":"null"}]
``` 
 
####  <a name="bk_trim"></a> KIRPMA  
 Baştaki ve sondaki boşlukları kaldırır sonra bir dize ifadesi döndürür.  
  
 **Söz dizimi**  
  
```  
TRIM(<str_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `str_expr`  
  
   Herhangi bir geçerli dize ifade var.  
  
  **Dönüş türleri**  
  
  Bir dize ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, bir sorgu içinde TRIM kullanmayı gösterir.  
  
```  
SELECT TRIM("   abc") AS t1, TRIM("   abc   ") AS t2, TRIM("abc   ") AS t3, TRIM("abc") AS t4
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"t1": "abc", "t2": "abc", "t3": "abc", "t4": "abc"}]  
``` 
####  <a name="bk_upper"></a> ÜST  
 Küçük harf karakter verileri büyük harfe dönüştürmenin sonra bir dize ifadesi döndürür.  
  
 **Söz dizimi**  
  
```  
UPPER(<str_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `str_expr`  
  
   Herhangi bir geçerli dize ifade var.  
  
  **Dönüş türleri**  
  
  Bir dize ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek sorguda üst kullanma işlemi gösterilmektedir  
  
```  
SELECT UPPER("Abc") AS upper  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"upper": "ABC"}]  
```

## <a id="array-functions"></a>Dizi işlevleri

Aşağıdaki skaler işlevler bir dizi giriş değeri ve dönüş sayısal, Boole veya dizi değeri üzerinde bir işlem uygulayın:
  
||||  
|-|-|-|  
|[ARRAY_CONCAT](#bk_array_concat)|[ARRAY_CONTAINS](#bk_array_contains)|[ARRAY_LENGTH](#bk_array_length)|  
|[ARRAY_SLICE](#bk_array_slice)|||  
  
####  <a name="bk_array_concat"></a> ARRAY_CONCAT  
 İki veya daha fazla dizi değerlerini birleştirirken sonucu olan bir dizi döndürür.  
  
 **Söz dizimi**  
  
```  
ARRAY_CONCAT (<arr_expr>, <arr_expr> [, <arr_expr>])  
```  
  
 **Bağımsız Değişkenler**  
  
- `arr_expr`  
  
   Tüm geçerli dizi ifadesidir.  
  
  **Dönüş türleri**  
  
  Bir dizi ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek iki diziyi birleştirme.  
  
```  
SELECT ARRAY_CONCAT(["apples", "strawberries"], ["bananas"]) AS arrayConcat 
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"arrayConcat": ["apples", "strawberries", "bananas"]}]  
```  
  
####  <a name="bk_array_contains"></a> ARRAY_CONTAINS  
Dizi belirtilen değeri içerip içermediğini gösteren bir Boole değeri döndürür. Komut içinde bir Boole ifadesi kullanarak bir nesne için bir kısmi veya tam eşleşme kontrol edebilirsiniz. 

**Söz dizimi**  
  
```  
ARRAY_CONTAINS (<arr_expr>, <expr> [, bool_expr])  
```  
  
 **Bağımsız Değişkenler**  
  
- `arr_expr`  
  
   Tüm geçerli dizi ifadesidir.  
  
- `expr`  
  
   Herhangi bir geçerli ifade var.  

- `bool_expr`  
  
   Herhangi bir boolean ifadesi var. Bu ayarlanırsa ' true'and belirtilen arama değeri bir nesne ise, komutu bir kısmi eşleşme (arama nesnesi nesnelerinden birine özelliklerinin bir alt kümesidir) için denetler. 'False' olarak ayarlanırsa komutu dizi içinde tüm nesnelerin bir tam eşleşme olup olmadığını denetler. Belirtilmezse, varsayılan değer false'tur. 
  
  **Dönüş türleri**  
  
  Bir Boolean değer döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek nasıl ARRAY_CONTAINS kullanarak bir diziye üyeliğini denetleyin.  
  
```  
SELECT   
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "apples") AS b1,  
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "mangoes") AS b2  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"b1": true, "b2": false}]  
```  

Aşağıdaki örnek nasıl bir JSON dizi ARRAY_CONTAINS kısmi bir eşleşme olup olmadığını denetleyin.  
  
```  
SELECT  
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "apples"}, true) AS b1, 
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "apples"}) AS b2,
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "mangoes"}, true) AS b3 
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{
  "b1": true,
  "b2": false,
  "b3": false
}] 
```  
  
####  <a name="bk_array_length"></a> ARRAY_LENGTH  
 Belirtilen bir dizi ifadesinin öğelerin sayısını döndürür.  
  
 **Söz dizimi**  
  
```  
ARRAY_LENGTH(<arr_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `arr_expr`  
  
   Tüm geçerli dizi ifadesidir.  
  
  **Dönüş türleri**  
  
  Sayısal bir ifade döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek nasıl ARRAY_LENGTH kullanarak bir dizinin uzunluğu.  
  
```  
SELECT ARRAY_LENGTH(["apples", "strawberries", "bananas"]) AS len  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{"len": 3}]  
```  
  
####  <a name="bk_array_slice"></a> ARRAY_SLICE  
 Bir dizi ifadesi bölümünü döndürür.
  
 **Söz dizimi**  
  
```  
ARRAY_SLICE (<arr_expr>, <num_expr> [, <num_expr>])  
```  
  
 **Bağımsız Değişkenler**  
  
- `arr_expr`  
  
   Tüm geçerli dizi ifadesidir.  
  
- `num_expr`  
  
   Dizi başlanan sıfır tabanlı sayısal dizini. Negatif değerler dizisi yani -1 başvuruları son öğesi göreli başlangıç dizini dizideki son öğeyi belirtmek için kullanılabilir.  

- `num_expr`  

   Sonuçta elde edilen dizideki öğelerin sayısı.    

  **Dönüş türleri**  
  
  Bir dizi ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, ARRAY_SLICE kullanarak bir dizi farklı dilim alma gösterilmektedir.  
  
```  
SELECT
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1) AS s1,  
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1) AS s2,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], -2, 1) AS s3,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], -2, 2) AS s4,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 0) AS s5,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1000) AS s6,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, -100) AS s7      
  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{  
           "s1": ["strawberries", "bananas"],   
           "s2": ["strawberries"],
           "s3": ["strawberries"],  
           "s4": ["strawberries", "bananas"], 
           "s5": [],
           "s6": ["strawberries", "bananas"],
           "s7": [] 
}]  
```  
## <a id="date-time-functions"></a>Tarih ve saat işlevi

Aşağıdaki skaler işlevler geçerli UTC tarih ve saat iki biçimde almanızı sağlar. değeri milisaniye cinsinden veya ISO 8601 biçimine uyan bir dize olarak UNIX dönem olan sayısal bir zaman damgası. 

|||
|-|-|
|[GETCURRENTDATETIME](#bk_get_current_date_time)|[GETCURRENTTIMESTAMP](#bk_get_current_timestamp)||

####  <a name="bk_get_current_date_time"></a> GETCURRENTDATETIME
 Geçerli UTC tarihi ve saati ISO 8601 dize olarak döndürür.
  
 **Söz dizimi**
  
```
GETCURRENTDATETIME ()
```
  
  **Dönüş türleri**
  
  Geçerli UTC tarihi ve saati ISO 8601 dize değeri döndürür. 

  Bu biçimi YYYY-AA-DDThh:mm:ss.sssZ ifade burada:
  
  |||
  |-|-|
  |YYYY|dört rakamlı yıl|
  |MM|iki haneli ay (01 Ocak, = vs.)|
  |DD|iki basamaklı ayın (01 ile 31)|
  |T|Başlangıç saati öğelerin signifier|
  |hh|iki basamaklı saat (00-23)|
  |aa|iki basamaklı dakika (00 ile 59)|
  |ss|iki basamaklı saniye (00 ile 59)|
  |.SSS|kesirli saniyenin üç hanesi|
  |Z|UTC (Eşgüdümlü Evrensel Saat) göstergesi||
  
  ISO 8601 biçimi hakkında daha fazla bilgi için bkz. [ISO_8601](https://en.wikipedia.org/wiki/ISO_8601)

  **Açıklamalar**

  GETCURRENTDATETIME belirleyici olmayan bir işlevdir. 
  
  UTC (Eşgüdümlü Evrensel Saat) döndürülen sonuç çizgidir.

  **Örnekler**  
  
  Aşağıdaki örnek, geçerli UTC tarih saat GetCurrentDateTime yerleşik işlevini kullanarak nasıl gösterir.
  
```  
SELECT GETCURRENTDATETIME() AS currentUtcDateTime
```  
  
 İşte bir örnek sonuç kümesi.
  
```  
[{
  "currentUtcDateTime": "2019-05-03T20:36:17.784Z"
}]  
```  

####  <a name="bk_get_current_timestamp"></a> GETCURRENTTIMESTAMP
 00:00:00 beri Perşembe, 1 Ocak 1970 geçen milisaniye sayısını döndürür. 
  
 **Söz dizimi**  
  
```  
GETCURRENTTIMESTAMP ()  
```  
  
  **Dönüş türleri**  
  
  Sayısal bir değer, geçerli UNIX dönem itibaren Örneğin 00:00:00 beri Perşembe, 1 Ocak 1970 geçen milisaniye sayısını geçen milisaniye sayısını döndürür.

  **Açıklamalar**

  GETCURRENTTIMESTAMP belirleyici olmayan bir işlevdir.
  
  UTC (Eşgüdümlü Evrensel Saat) döndürülen sonuç çizgidir.

  **Örnekler**  
  
  Aşağıdaki örnek alma GetCurrentTimestamp yerleşik işlevi kullanarak geçerli zaman damgasını gösterir.
  
```  
SELECT GETCURRENTTIMESTAMP() AS currentUtcTimestamp
```  
  
 İşte bir örnek sonuç kümesi.
  
```  
[{
  "currentUtcTimestamp": 1556916469065
}]  
```

## <a id="spatial-functions"></a>Uzamsal İşlevler

Cosmos DB, Jeo-uzamsal sorgulamak için aşağıdaki açık Jeo-uzamsal Consortium (OGC) yerleşik işlevleri destekler. Aşağıdaki skaler İşlevler, uzamsal nesne giriş değeri bir işlem gerçekleştirmek ve bir sayısal veya Boolean değeri döndürür.  
  
|||||
|-|-|-|-|
|[ST_DISTANCE](#bk_st_distance)|[ST_WITHIN](#bk_st_within)|[ST_INTERSECTS](#bk_st_intersects)|[ST_ISVALID](#bk_st_isvalid)|
|[ST_ISVALIDDETAILED](#bk_st_isvaliddetailed)||||
  
####  <a name="bk_st_distance"></a> ST_DISTANCE  
 İki GeoJSON noktası, çokgen veya LineString ifadeler uzaklığı döndürür.  
  
 **Söz dizimi**  
  
```  
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `spatial_expr`  
  
   Tüm geçerli GeoJSON noktası, çokgen veya LineString nesne ifadesidir.  
  
  **Dönüş türleri**  
  
  Uzaklık içeren sayısal bir ifade döndürür. Bu varsayılan başvuru sistemi için ölçümleri ifade edilir.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, ST_DISTANCE yerleşik işlevi kullanarak belirtilen konumun içinde 30 KM tüm ailesi belgeler döndürülecek gösterilmektedir. .  
  
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
  
####  <a name="bk_st_within"></a> ST_WITHIN  
 İlk bağımsız değişkende belirtilen GeoJSON nesne (noktası, çokgen veya LineString) ikinci bağımsız değişkende GeoJSON (noktası, çokgen veya LineString) içinde olup olmadığını gösteren bir Boole ifadesi döndürür.  
  
 **Söz dizimi**  
  
```  
ST_WITHIN (<spatial_expr>, <spatial_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `spatial_expr`  
  
   Tüm geçerli GeoJSON noktası, çokgen veya LineString nesne ifadesidir.  
 
- `spatial_expr`  
  
   Tüm geçerli GeoJSON noktası, çokgen veya LineString nesne ifadesidir.  
  
  **Dönüş türleri**  
  
  Bir Boolean değer döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, tüm ailesi bulmanın ST_WITHIN kullanarak bir Çokgen içinde gösterilmektedir.  
  
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

####  <a name="bk_st_intersects"></a> ST_INTERSECTS  
 İlk bağımsız değişkende belirtilen GeoJSON nesne (noktası, çokgen veya LineString) ikinci bağımsız değişkende GeoJSON (noktası, çokgen veya LineString) kesişip kesişmediğini belirten bir Boole ifadesi döndürür.  
  
 **Söz dizimi**  
  
```  
ST_INTERSECTS (<spatial_expr>, <spatial_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `spatial_expr`  
  
   Tüm geçerli GeoJSON noktası, çokgen veya LineString nesne ifadesidir.  
 
- `spatial_expr`  
  
   Tüm geçerli GeoJSON noktası, çokgen veya LineString nesne ifadesidir.  
  
  **Dönüş türleri**  
  
  Bir Boolean değer döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, ile belirtilen Çokgen kesişen tüm alanları gösterilmektedir.  
  
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
  
####  <a name="bk_st_isvalid"></a> ST_ISVALID  
 Belirtilen GeoJSON noktası, çokgen veya LineString ifade geçerli olup olmadığını gösteren bir Boole değeri döndürür.  
  
 **Söz dizimi**  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 **Bağımsız Değişkenler**
  
- `spatial_expr`  
  
   Herhangi bir geçerli GeoJSON noktası, çokgen veya LineString ifade var.  
  
  **Dönüş türleri**  
  
  Bir Boolean ifadesi döndürür.  
  
  **Örnekler**  
  
  Aşağıdaki örnek, bir nokta ST_VALID kullanarak geçerli olup olmadığını denetlemek gösterilmektedir.  
  
  Örneğin, bu nokta geçerli değerler [-90, 90] aralığında böylece sorgu döndürür false değil bir enlem değeri vardır.  
  
  Çokgen için GeoJSON belirtimi sağlanan son koordinat çifti kapalı şekli oluşturmak için birinci ile aynı olması gerekir. İçinde bir Çokgen noktalarının saat yönünün tersi düzende belirtilmesi gerekir. Bir çokgenin belirtilen saat yönünde sırayla bölgesinde tersini temsil eder.  
  
```  
SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] }) AS b 
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{ "b": false }]  
```  
  
####  <a name="bk_st_isvaliddetailed"></a> ST_ISVALIDDETAILED  
 Bir Boole değeri içeren bir JSON değeri, belirtilen GeoJSON noktası, çokgen veya LineString ifade geçerliyse ve geçersiz değeri döndürür, ayrıca bir dize değeri olarak nedeni.  
  
 **Söz dizimi**  
  
```  
ST_ISVALIDDETAILED(<spatial_expr>)  
```  
  
 **Bağımsız Değişkenler**  
  
- `spatial_expr`  
  
   Tüm geçerli GeoJSON noktası veya Çokgen ifadesidir.  
  
  **Dönüş türleri**  
  
  Bir Boole değeri içeren bir JSON değeri, belirtilen GeoJSON noktası veya Çokgen ifade geçerliyse ve geçersiz değeri döndürür, ayrıca bir dize değeri olarak nedeni.  
  
  **Örnekler**  
  
  Aşağıdaki örnek nasıl (Ayrıntılar ile) ST_ISVALIDDETAILED kullanarak geçerliliğini denetlemek.  
  
```  
SELECT ST_ISVALIDDETAILED({   
  "type": "Polygon",   
  "coordinates": [[ [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] ]]  
}) AS b 
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```  
[{  
  "b": {
    "valid": false,
    "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points."   
  }  
}]  
```  

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB'ye giriş](introduction.md)
- [UDF](sql-query-udfs.md)
- [toplamaları](sql-query-aggregates.md)