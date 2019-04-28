---
title: Veri odaklı-style ifadeler içinde Azure haritalar Web SDK'sı | Microsoft Docs
description: Azure haritalar Web SDK'sı verilerle stili ifadeleri kullanma
author: rbrundritt
ms.author: richbrun
ms.date: 4/4/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendleton
ms.custom: codepen
ms.openlocfilehash: 3b234ca37783fe557baf307f198de9636b06a382
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60905009"
---
# <a name="data-driven-style-expressions-web-sdk"></a>Veri odaklı stili ifadeleri (Web SDK'sı)

İfadeler her bir veri kaynağı şeklinde içinde tanımlanan özellikler gözlemleyin stil seçeneklerini iş mantığı uygulamak etkinleştirin. İfadeler, bir veri kaynağı veya katman verileri filtrelemek için de kullanılabilir. İfadeler gibi IF deyimi, koşullu mantık oluşabilir ve ile verileri işlemek için de kullanılabilir; dize, mantıksal ve matematik işleçleri. 

Veri odaklı stilleri stil etrafında iş mantığı uygulamak için gereken kod miktarını azaltabilirsiniz. Katmanlar ile kullanıldığında, kullanıcı Arabirimi iş parçacığında iş mantığı değerlendirmek için karşılaştırıldığında daha yüksek performans sağlayan ayrı bir iş parçacığı üzerinde oluşturma zamanında ifadeler değerlendirilir.

Aşağıdaki video, veri odaklı stil Azure haritalar Web SDK'sındaki genel bir bakış sağlar.

<br/>

<iframe src="https://channel9.msdn.com/Shows/Internet-of-Things-Show/Data-Driven-Styling-with-Azure-Maps/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

İfadeleri JSON dizileri olarak temsil edilir. Dizi içindeki bir ifadenin ilk öğesi ifade işleci adını belirten bir dizedir. Örneğin, "+" veya "Durum". Sonraki öğeleri (varsa) ifadesi için bağımsız değişkenler. Her bağımsız değişken bir değişmez değer olduğunu (bir dize, sayı, Boole, veya `null`), veya başka bir ifade dizi. Aşağıdaki sözde kod, bir ifade temel yapısını tanımlar. 

```javascript
[ 
    expression_operator, 
    argument0, 
    argument1, 
    …
] 
```

Azure haritalar Web SDK'sı kendi veya diğer ifadeler birlikte kullanılan ifadeler çoğu türlerini destekler.

| İfade türü | Açıklama |
|---------------------|-------------|
| [Boole ifadeleri](#boolean-expressions) | Boolean ifadeleri Boole karşılaştırmalar değerlendirmek için bir Boole işleçleri ifade kümesi sağlar. |
| [Renk ifadeleri](#color-expressions) | Renk ifadeleri oluşturma ve renk değerleri kolaylaştırır. |
| [Koşullu ifadeler](#conditional-expressions) | Koşullu ifadeler gibi IF deyimi mantıksal işlemleri sağlar. |
| [Veri ifadeleri](#data-expressions) | Bir özellik özelliği verilere erişim sağlar. |
| [Enterpolasyon ve adım ifadeleri](#interpolate-and-step-expressions) | Enterpolasyon ve adım ifadeleri ilişkilendirilmiş bir eğri ya da adım işlevi değerleri hesaplamak için kullanılabilir. |
| [Katman belirli ifadeler](#layer-specific-expressions) | Yalnızca tek bir katmana uygulanabilen özel ifadeler. |
| [Matematik ifadeleri](#math-expressions) | İfade framework içindeki veri temelli hesaplamaları gerçekleştirmek için matematik işleçleri sağlar. |
| [Dize işleci ifadeleri](#string-operator-expressions) | Dize işleci ifadeleri birleştirerek ve durumun dönüştürme gibi dizeleri dönüştürme işlemleri. |
| [Tür ifadeleri](#type-expressions) | Tür ifadeleri test etmek ve dizeler, sayılar ve Boole değerleri gibi farklı veri türlerini dönüştürme için araçlar sağlar. |
| [Değişken bağlama ifadeleri](#variable-binding-expressions) | Değişken bağlama ifadeleri hesaplamanın sonuçlarını bir değişkende depolanması sağlar ve başka bir yerde bir deyim içinde depolanan değeri yeniden hesaplar gerek kalmadan birden çok kez başvurulan. |
| [Yakınlaştırma ifadesi](#zoom-expression) | Geçerli harita yakınlaştırma düzeyini işleme zaman alır. |

Bu belgedeki tüm örneklerde, farklı türde ifadeler kullanılabilir, farklı yolları göstermek için aşağıdaki özellik kullanır. 

```javascript
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-122.13284, 47.63699]
    },
    "properties": {     
        "entityType": "restaurant",
        "revenue": 12345,
        "subTitle": "Building 40", 
        "temperature": 72,
        "title": "Cafeteria", 
        "zoneColor": "red"
    }
}
```

## <a name="data-expressions"></a>Veri ifadeleri

Veri ifadeleri bir özellik özelliği verilere erişim sağlar. 

| İfade | Dönüş türü | Açıklama |
|------------|-------------|-------------|
| `['at', number, array]` | object | Bir dizideki öğeyi alır. |
| `['geometry-type']` | string | Özelliğin geometri türünü alır: Point, MultiPoint, LineString, MultiLineString, Polygon, MultiPolygon. |
| `['get', string]` | value | Özellik değeri geçerli özelliğin özelliklerini alır. İstenen özellik eksik değilse null değerini döndürür. |
| `['get', string, object]` | value | Sağlanan nesne özelliklerinden özellik değerini alır. İstenen özellik eksik değilse null değerini döndürür. |
| `['has', string]` | boole | Bir özellik özelliklerini belirtilen özellik olup olmadığını belirler. |
| `['has', string, object]` | boole | Nesnenin özelliklerini belirtilen özellik olup olmadığını belirler. |
| `['id']` | value | Varsa özelliğin kimliği alır. |
| `['length', string | array]` | number | Bir dize ya da dizinin uzunluğunu alır. |

**Örnekler**

Bir özellik özelliklerini erişilebilir bir deyimde doğrudan kullanarak bir `get` ifade. Aşağıdaki örnek, Kabarcık katmanın rengini özelliği belirtmek için bu özellik "zoneColor" değerini kullanır. 

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: ['get', 'zoneColor'] //Get the zoneColor value.
});
```

Yukarıdaki örnekte tüm noktası özellikleri varsa düzgün çalışır `zoneColor` bunlar yapmadığınız takdirde, ancak özelliği, renk büyük olasılıkla geri döner "siyah olarak". Geri dönüş rengini değiştirmek için bir `case` ifade ile birlikte kullanılabilir `has` özelliği varsa ve bir geri dönüş rengine yerine döndürmüyor denetlemek için ifade.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'case', //Use a conditional case expression.

        ['has', 'zoneColor'],   //Check to see if feature has a "zoneColor" property
        ['get', 'zoneColor'],   //If it does, use it.

        'blue'  //If it doesn't, default to blue.
    ]
});
```

Kabarcık ve simge katmanlarını bir veri kaynağındaki tüm şekiller koordinatlarını varsayılan olarak işlenir. Bu Çokgen veya çizgi köşelerini vurgulamak için yapılabilir. `filter` Katmanın seçeneği, işler kullanarak özellikleri geometri türlerini sınırlamak için kullanılabilir bir `['geometry-type']` Boole ifadesinde bulunan bir ifade. Aşağıdaki örnek, yalnızca bir kabarcık katmanı sınırlar `Point` özellikleri işlenir.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    filter: ['==', ['geometry-type'], 'Point']
});
```

Aşağıdaki örnek, her ikisi de sağlayacak `Point` ve `MultiPoint` işlenecek özellikleri. 

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    filter: ['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]
});
```

Benzer şekilde, çokgenler özetini satırı katmanlarında işlenir. Çizgi katmanı bu davranışı devre dışı bırakmak için yalnızca veren filtre ekleme `LineString` ve `MultiLineString` özellikleri.  

## <a name="math-expressions"></a>Matematik ifadeleri

Matematik ifadeleri ifade çerçevesine veri temelli hesaplamaları gerçekleştirmek için matematik işleçleri sağlar.

| İfade | Dönüş türü | Açıklama |
|------------|-------------|-------------|
| `['+', number, number, …]` | number | Belirtilen sayıların toplamını hesaplar. |
| `['-', number]` | number | 0 ile belirtilen sayıyı çıkartır. |
| `['-', number, number]` | number | İkinci sayı ilk sayılarla çıkarır. |
| `['*', number, number, …]` | number | Birlikte belirtilen sayı ile çarpar. |
| `['/', number, number]` | number | İlk sayı ikinci sayı olarak böler. |
| `['%', number, number]` | number | İkinci sayı ilk sayısı bölerken kalanı hesaplar. |
| `['^', number, number]` | number | İkinci sayı üssü ilk değeri hesaplar. |
| `['abs', number]` | number | Belirtilen sayının mutlak değerini hesaplar. |
| `['acos', number]` | number | Belirtilen sayının arkkosinüsünü hesaplar. |
| `['asin', number]` | number | Belirtilen sayının ark sinüsünü hesaplar. |
| `['atan', number]` | number | Belirtilen sayının ark tanjantını hesaplar. |
| `['ceil', number]` | number | Sonraki tamsayı en fazla sayıyı yuvarlar. |
| `['cos', number]` | number | Belirtilen sayının cos hesaplar. |
| `['e']` | number | Matematik sabiti döndürür `e`. |
| `['floor', number]` | number | Önceki tamsayı sayıyı yuvarlar. |
| `['ln', number]` | number | Belirtilen sayının doğal logaritmasını hesaplar. |
| `['ln2']` | number | Matematik sabiti döndürür `ln(2)`. |
| `['log10', number]` | number | Belirtilen sayının tabanı on logaritmasını hesaplar. |
| `['log2', number]` | number | Belirtilen sayının tabanı iki logaritmasını hesaplar. |
| `['max', number, number, …]` | number | Sayının belirtilen kümesindeki en fazla sayı hesaplar. |
| `['min', number, number, …]` | number | Sayının belirtilen kümedeki alt sınır hesaplar. |
| `['pi']` | number | Matematik sabiti döndürür `PI`. |
| `['round', number]` | number | En yakın tamsayıya yuvarlar. Yarım değerler, sıfırdan uzağa yuvarlanır. Örneğin, `['round', -1.5]` -2 olarak değerlendirilir. |
| `['sin', number]` | number | Belirtilen sayının sinüsünü hesaplar. |
| `['sqrt', number]` | number | Belirtilen Sayının karekökünü hesaplar. |
| `['tan', number]` | number | Belirtilen sayının tanjantını hesaplar. |
## <a name="boolean-expressions"></a>Mantıksal ifadeler

Boolean ifadeleri Boole karşılaştırmalar değerlendirmek için bir Boole işleçleri ifade kümesi sağlar.

Değerleri karşılaştırma, karşılaştırma kesin olarak belirlenmiş. Farklı türlerde değerler her zaman eşit olarak kabul edilir. Burada türleri ayrıştırma zamanında farklı olduğu bilinen durumları geçersiz olarak kabul edilir ve bir ayrıştırma hatası oluşturur. 

| İfade | Dönüş türü | Açıklama |
|------------|-------------|-------------|
| `['! ', boolean]` | boole | Mantıksal değilleme. Döndürür `true` giriş ise `false`, ve `false` giriş ise `true`. |
| `['!= ', value, value]` | boole | Döndürür `true` giriş değerler eşit değilse `false` Aksi takdirde. |
| `['<', value, value]` | boole | Döndürür `true` ilk giriş ikinci kesinlikle küçük ise `false` Aksi takdirde. Bağımsız değişken ya da hem dize veya iki sayı olması gerekir. |
| `['<=', value, value]` | boole | Döndürür `true` ilk giriş saniye veya daha az ise `false` Aksi takdirde. Bağımsız değişken ya da hem dize veya iki sayı olması gerekir. |
| `['==', value, value]` | boole | Döndürür `true` giriş değerler eşitse, `false` Aksi takdirde. Bağımsız değişken ya da hem dize veya iki sayı olması gerekir. |
| `['>', value, value]` | boole | Döndürür `true` ilk giriş ikinci kesinlikle büyük ise `false` Aksi takdirde. Bağımsız değişken ya da hem dize veya iki sayı olması gerekir. |
| `['>=' value, value]` | boole | Döndürür `true` ilk giriş büyüktür veya eşittir, ikinci ise `false` Aksi takdirde. Bağımsız değişken ya da hem dize veya iki sayı olması gerekir. |
| `['all', boolean, boolean, …]` | boole | Döndürür `true` tüm girişleri varsa `true`, `false` Aksi takdirde. |
| `['any', boolean, boolean, …]` | boole | Döndürür `true` herhangi biri girişleri `true`, `false` Aksi takdirde. |

## <a name="conditional-expressions"></a>Koşullu ifadeler

Koşullu ifadeler gibi IF deyimi mantıksal işlemleri sağlar.

Aşağıdaki ifadeler, giriş veri çubuğunda koşullu mantık işlemleri. Örneğin, `case` ifade ederken "if/then/else" mantığı sağlar `match` "anahtar-gibi bir ifade" ifadesidir. 

### <a name="case-expression"></a>CASE ifadesi

A `case` ifade if deyimi (if/then/else) mantıksal gibi sağlayan koşullu ifade bir türüdür. Bu tür bir ifade, bir boolean Koşul listesi üzerinden adımları ve doğru olan ilk boolean koşulu çıkış değeri döndürür.

Aşağıdaki sözde kod yapısını tanımlayan `case` ifade. 

```javascript
[
    'case',
    condition1: boolean, 
    output1: value,
    condition2: boolean, 
    output2: value,
    ...,
    fallback: value
]
```

**Örnek**

Aşağıdaki örnek olarak değerlendirilen bir tane bulana kadar farklı Boole koşulları adımları `true`ve ardından ilişkilendirilen değer döndürür. Herhangi bir boolean koşul değerlendirilirse `true`, bir geri dönüş değeri döndürülür. 

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'case',

        //Check to see if the first boolean expression is true, and if it is, return its assigned result.
        ['has', 'zoneColor'],
        ['get', 'zoneColor'],

        //Check to see if the second boolean expression is true, and if it is, return its assigned result.
        ['all', ['has', ' temperature '], ['>', ['get', 'temperature'], 100]],
        'red',

        //Specify a default value to return.
        'green'
    ]
});
```

### <a name="match-expression"></a>Eşleştirme ifadesi

A `match` ifadesi switch ifadesi gibi mantıksal sağlayan koşullu ifade bir türüdür. Giriş gibi herhangi bir ifade olabilir `['get', 'entityType']` bir dize veya sayı döndürür. Her etiket, tek bir değişmez değer veya bir dizi değişmez değerleri, tüm dize veya tüm sayı değerleri olmalıdır olması gerekir. Giriş dizisi eşleşen değerler varsa eşleşir. Her etiket, benzersiz olması gerekir. Etiketlerin türü giriş türü eşleşmiyorsa, geri dönüş değeri sonuç olacaktır.

Aşağıdaki sözde kod yapısını tanımlayan `match` ifade. 

```javascript
[
    'match',
    input: number | string,
    label1: number | string | (number | string)[], 
    output1: value,
    label2: number | string | (number | string)[], 
    output2: value,
    ...,
    fallback: value
]
```

**Örnekler**

Aşağıdaki örnek bakan `entityType` Kabarcık katmanda noktası özelliğinin özelliği için bir eşleşme arar. Bu bir eşleşme bulunursa, döndürülen değer veya geri dönüş değeri döndürür, belirtilen.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'match',

        //Get the property to match.
        ['get', 'entityType'],

        //List the values to match and the result to return for each match.
        'restaurant', 'red',
        'park', 'green',

        //Specify a default value to return if no match is found.
        'black'
    ]
});
```

Aşağıdaki örnek, bir dizi tümü aynı değeri döndürmelidir etiketleri listelemek için bir dizi kullanır. Bu tek tek her etiket listesinde daha çok daha verimli olur. Bu durumda, `entityType` özelliktir "Restoran" veya "grocery_store", "red" renk döndürülür.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'match',

        //Get the property to match.
        ['get', 'entityType'],

        //List the values to match and the result to return for each match.
        ['restaurant', 'grocery_store'], 'red',

        'park', 'green',

        //Specify a default value to return if no match is found.
        'black'
    ]
});
```

### <a name="coalesce-expression"></a>Birleştirme ifadesini

A `coalesce` ifade adımları bir ifade kümesi kadar ilk null olmayan değer elde edilir ve bu değeri döndürür. 

Aşağıdaki sözde kod yapısını tanımlayan `coalesce` ifade. 

```javascript
[
    'coalesce', 
    value1, 
    value2, 
    …
]
```

**Örnek**

Aşağıdaki örnekte bir `coalesce` ayarlamak için ifade `textField` sembol katmanının seçeneği. `title` Özellik veya kümesine özelliği eksik `null`, aranırken ifade deneyecek `subtitle` özelliği varsa, eksik veya `null`, bunu daha sonra boş bir dize olarak döner. 

```javascript
var layer = new atlas.layer.SymbolLayer(datasource, null, {
    textOptions: {
        textField: [
            'coalesce',

            //Try getting the title property.
            ['get', 'title'],

            //If there is no title, try getting the subtitle. 
            ['get', 'subtitle'],

            //Default to an empty string.
            ''
        ]
    }
});
```

## <a name="type-expressions"></a>Tür ifadeleri

Tür ifadeleri test etmek ve dizeler, sayılar ve Boole değerleri gibi farklı veri türlerini dönüştürme için araçlar sağlar.

| İfade | Dönüş türü | Açıklama |
|------------|-------------|-------------|
| `['literal', array]`<br/><br/>`['literal', object]` | dizi \| nesnesi | Değişmez değer bir dizi veya nesne değeri döndürür. Bu ifade, bir ifade olarak değerlendirilen bir dizi veya nesne engellemek için kullanın. Bir ifade tarafından döndürülen bir dizi veya nesne ihtiyacı olduğunda, bu gereklidir. |
| `['to-boolean', value]` | boole | Giriş değeri bir boolean değerine dönüştürür. Sonuç `false` giriş boş bir dize olduğunda `0`, `false`, `null`, veya `NaN`; Aksi takdirde, `true`. |
| `['to-color', value]`<br/><br/>`['to-color', value1, value2…]` | color | Giriş değeri için bir renk dönüştürür. Birden çok değer sağlanıyorsa, ilk başarılı dönüştürme alınana kadar her birini sırayla değerlendirilir. Girişlerin hiçbiri dönüştürülebilir ise, ifade bir hatadır. |
| `['to-number', value]`<br/><br/>`['to-number', value1, value2, …]` | number | Mümkünse, giriş değeri bir sayıya dönüştürür. Giriş ise `null` veya `false`, sonuç 0'dır. Giriş ise `true`, sonuç 1'dir. Giriş bir dize ise, bir sayı kullanarak dönüştürülür [ToNumber](https://tc39.github.io/ecma262/#sec-tonumber-applied-to-the-string-type) dize işlevi ECMAScript dil belirtimi. Birden çok değer sağlanıyorsa, ilk başarılı dönüştürme alınana kadar her birini sırayla değerlendirilir. Girişlerin hiçbiri dönüştürülebilir ise, ifade bir hatadır. |
| `['to-string', value]` | string | Giriş değeri bir dizeye dönüştürür. Giriş ise `null`, sonuç `""`. Giriş bir boolean sonucu ise, `"true"` veya `"false"`. Giriş bir sayı ise, kullanarak bir dize dönüştürülür [ToString](https://tc39.github.io/ecma262/#sec-tostring-applied-to-the-number-type) ECMAScript dil belirtimi işlevi sayı. Giriş rengi ise, RGBA CSS renk dizesine dönüştürülür `"rgba(r,g,b,a)"`. Aksi takdirde, giriş kullanarak bir dize dönüştürülür [JSON.stringify](https://tc39.github.io/ecma262/#sec-json.stringify) işlevi ECMAScript dil belirtimi. |
| `['typeof', value]` | string | Verilen değerin türünü tanımlayan bir dize döndürür. |

> [!TIP]
> Bir hata iletisi, benzer `Expression name must be a string, but found number instead. If you wanted a literal array, use ["literal", [...]].` tarayıcı konsolda göründüğü yere ilk değeri için bir dize olmayan bir dizisi olduğundan kodunuzda bir ifade olduğu anlamına gelir. Bir dizi döndürecek şekilde ifadeyi istiyorsanız, dizi ile sarmalamak `literal` ifade. Aşağıdaki örnekte ayarlar simgesine `offset` seçeneği kullanarak iki sayı içeren bir dizi olması gereken bir sembol katmanının bir `match` arasında iki uzaklık değerleri seçmek için ifade değerini temel alarak `entityType` noktasının özelliği özelliği.
>
> ```javascript
> var layer = new atlas.layer.SymbolLayer(datasource, null, {
>     iconOptions: {
>         offset: [
>             'match',
>
>             //Get the entityType value.
>             ['get', 'entityType'],
>
>             //If there is no title, try getting the subtitle. 
>             'restaurant', ['literal', [0, -10]],
>
>             //Default to value.
>             ['literal', [0, 0]]
>         ]
>     }
> });
> ```

## <a name="color-expressions"></a>Renk ifadeleri

Renk ifadeleri oluşturma ve renk değerleri kolaylaştırır.

| İfade | Dönüş türü | Açıklama |
|------------|-------------|-------------|
| `['rgb', number, number, number]` | color | Bir renk değerinden oluşturur *kırmızı*, *yeşil*, ve *mavi* arasında aralığında olması gerekir bileşenleri `0` ve `255`ve biralfabileşeni`1`. Herhangi bir bileşeni aralık dışında ise, ifade bir hatadır. |
| `['rgba', number, number, number, number]` | color | Bir renk değerinden oluşturur *kırmızı*, *yeşil*, *mavi* arasında aralığında olması gerekir bileşenleri `0` ve `255`ve bir dizi içinde bir alfa bileşeni `0` ve `1`. Herhangi bir bileşeni aralık dışında ise, ifade bir hatadır. |
| `['to-rgba']` | \[sayı, sayı, sayı, numarası\] | Giriş rengin içeren dört öğeli bir dizi döndürür *kırmızı*, *yeşil*, *mavi*, ve *alfa* o sırada bileşenleri. |

**Örnek**

Aşağıdaki örnek, oluşturur ve RGB renk değeri olan bir *kırmızı* değerini `255`, ve *yeşil* ve *mavi* çarpılarakhesaplanandeğerleri`2.5` değeriyle `temperature` özelliği. Farklı gri rengi değişir sıcaklık değiştikçe *kırmızı*.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'rgb', //Create a RGB color value.

        255,    //Set red value to 255.

        ['*', 2.5, ['get', 'temperature']], //Multiple the temperature by 2.5 and set the green value.

        ['*', 2.5, ['get', 'temperature']]  //Multiple the temperature by 2.5 and set the blue value.
    ]
});
```

## <a name="string-operator-expressions"></a>Dize işleci ifadeleri

Dize işleci ifadeleri birleştirerek ve durumun dönüştürme gibi dizeleri dönüştürme işlemleri. 

| İfade | Dönüş türü | Açıklama |
|------------|-------------|-------------|
| `['concat', string, string, …]` | string | Birden çok dizeyi birlikte art arda ekler. Her değer bir dize olmalıdır. Kullanım `to-string` gerekirse diğer değer türleri dizeye dönüştürmek için ifade yazın. |
| `['downcase', string]` | string | Belirtilen dizeyi küçük harfe dönüştürür. |
| `['upcase', string]` | string | Belirtilen dizeyi büyük harfe dönüştürür. |

**Örnek**

Aşağıdaki örnek `temperature` noktasının özelliğinin bir dizeye özellik ve sonuna "° F" art arda ekler.

```javascript
var layer = new atlas.layer.SymbolLayer(datasource, null, {
    textOptions: {
        textField: ['concat', ['to-string', ['get', 'temperature']], '°F'],

        //Some additional style options.
        offset: [0, -1.5],
        size: 12,
        color: 'white'
    }
});
```

Yukarıdaki ifadeyi "64 °, aşağıdaki resimde gösterildiği gibi üzerine yayılan F" metniyle harita üzerinde bir PIN işler.

<center>

![Dize işleci ifade örneği](media/how-to-expressions/string-operator-expression.png) </center>

## <a name="interpolate-and-step-expressions"></a>Enterpolasyon ve adım ifadeleri

Enterpolasyon ve adım ifadeleri ilişkilendirilmiş bir eğri ya da adım işlevi değerleri hesaplamak için kullanılabilir. Bu ifadeler, giriş olarak bir sayısal değeri döndürür. Örneğin bir ifadede ele `['get',  'temperature']`. Giriş değeri "olarak ilişkilendirilmiş eğri ya da adım işlevi en uygun değeri belirlemek için durdurulur" olarak adlandırılan, giriş ve çıkış değerleri çiftlerini karşı değerlendirilir. Her durağı giriş değeri bir sayı ve artan sırada olması gerekir. Çıkış değerleri, bir sayı ve bir sayı dizisi veya bir renk olmalıdır.

### <a name="interpolate-expression"></a>İfade enterpolasyon

Bir `interpolate` ifade, sürekli bir kesintisiz değerler kümesini durdurma değerleri arasında ilişkilendirme ile hesaplamak için kullanılabilir. Bir `interpolate` hangi sonucunda değerleri seçili gelen renk gradyan renk değerleri döndüren ifadeyi üretir.

Üç tür kullanılabilir ilişkilendirme yöntemi bir `interpolate` ifade:
 
* `['linear']` -Durakları çifti arasında doğrusal olarak ilişkilendirir.
* `['exponential', base]` -Katlanarak durakları arasında ilişkilendirir. `base` Değer, çıkış artırır oranı denetler. Yüksek değerler daha yüksek aralığın sonuna doğru artırmak çıkış yapın. A `base` değeri 1'e yakın daha doğrusal olarak artar bir çıktı üretir.
* `['cubic-bezier', x1, y1, x2, y2]` -İlişkilendirileceğini kullanarak bir [üçüncü dereceden Bezier eğrisi](https://developer.mozilla.org/docs/Web/CSS/timing-function) verilen denetim noktaları tarafından tanımlanır.

Bu ilişkilendirme farklı türde görünmesi bir örnek aşağıda verilmiştir. 

| Doğrusal  | Üstel | Üçüncü derece Bezier |
|---------|-------------|--------------|
| ![Doğrusal enterpolasyon grafiği](media/how-to-expressions/linear-interpolation.png) | ![Üstel ilişkilendirme grafiği](media/how-to-expressions/exponential-interpolation.png) | ![Üçüncü dereceden Bezier ilişkilendirme grafiği](media/how-to-expressions/bezier-curve-interpolation.png) |

Aşağıdaki sözde kod yapısını tanımlayan `interpolate` ifade. 

```javascript
[
    'interpolate',
    interpolation: ['linear'] | ['exponential', base] | ['cubic-bezier', x1, y1, x2, y2],
    input: number,
    stopInput1: number, 
    stopOutput1: value1,
    stopInput2: number, 
    stopOutput2: value2, 
    ...
]
```

**Örnek**

Aşağıdaki örnekte bir `linear interpolate` ayarlamak için ifade `color` Kabarcık katman özelliğini temel alarak `temperature` noktası özelliği özelliği. Varsa `temperature` değer olan 60, "mavi", "red" 80 veya daha büyük döndürdüyse 70 ve 80 arasında "turuncu", döndürülecek, 60 ve 70'den az sarı, döndürülecek döndürülen daha az.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'interpolate',
        ['linear'],
        ['get', 'temperature'],
        50,        
        'blue',
        60,
        'yellow',
        70,
        'orange',
        80,
        'red'
    ]
});
```

Aşağıdaki görüntüde, renk için yukarıdaki ifadeyi nasıl seçilir gösterilmektedir.
 
<center>

![İfade örneği enterpolasyon](media/how-to-expressions/interpolate-expression-example.png) </center>

### <a name="step-expression"></a>Adım ifadesi

A `step` değerlendirilerek ayrık, basamaklı sonuç değerleri hesaplamak için ifade kullanılabilir bir [piecewise sabiti işlevi](http://mathworld.wolfram.com/PiecewiseConstantFunction.html) durakları tarafından tanımlanır. 

Aşağıdaki sözde kod yapısını tanımlayan `step` ifade. 

```javascript
[
    'step',
    input: number,
    output0: value0,
    stop1: number, 
    output1: value1,
    stop2: number, 
    output2: value2, 
    ...
]
```

Adım ifadeleri ilk Durdur'den az giriş değeri ya da giriş ise ilk giriş değeri hemen önce Durdur çıkış değeri döndürür. 

**Örnek**

Aşağıdaki örnekte bir `step` ayarlamak için ifade `color` Kabarcık katman özelliğini temel alarak `temperature` noktası özelliği özelliği. Varsa `temperature` değer olan 60, "mavi", "red" 80 veya daha büyük döndürdüyse 70 ve 80 arasında "turuncu", döndürülecek, 60 ve 70'den az arasında "Sarı", döndürülecek döndürülen daha az.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'step',
        ['get', 'temperature'],
        'blue',
        60,
        'yellow',
        70,
        'orange',
        80,
        'red'
    ]
});
```

Aşağıdaki görüntüde, renk için yukarıdaki ifadeyi nasıl seçilir gösterilmektedir.
 
<center>

![Adım ifadesi örneği](media/how-to-expressions/step-expression-example.png)
</center>

## <a name="layer-specific-expressions"></a>Katman belirli ifadeler

Yalnızca belirli katmanlara uygulamak özel ifadeler.

### <a name="heat-map-density-expression"></a>Isı Haritası yoğunluğu ifadesi

Isı Haritası yoğunluğu ifadesi her pikselin ısı Haritası katman ısı Haritası yoğunluklu değerini alır ve olarak tanımlanan `['heatmap-density']`. Bu değer arasında bir sayıdır `0` ve `1` ve birlikte kullanılan bir `interpolation` veya `step` ısı Haritası renklendirmek için kullanılan renk gradyanı tanımlamak için ifade. Bu ifade, yalnızca kullanılabilir [renk seçeneği](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.heatmaplayeroptions?view=azure-iot-typescript-latest#color) ısı Haritası katman.

> [!TIP]
> Dizin 0 ilişkilendirme deyimde rengi veya bir adım renk varsayılan rengi tanımlar renk alanının veri olduğu ve arka plan rengi tanımlamak için kullanılabilir. Bu değer saydam veya yarı saydam siyah ayarlamak birçok tercih eder. 

**Örnek**

Bu örnek düz renk gradyan ısı Haritası işlemeye oluşturmak için bir liner ilişkilendirme ifade kullanır. 

```javascript 
var layer = new atlas.layer.HeatMapLayer(datasource, null, {
    color: [
        'interpolate',
        ['linear'],
        ['heatmap-density'],
        0, 'transparent',
        0.01, 'purple',
        0.5, '#fb00fb',
        1, '#00c3ff'
    ]
});
```

Kesintisiz gradyan ısı Haritası renklendirmek için kullanmanın yanı sıra renkleri aralıkları bir dizi içinde kullanılarak belirtilebilir bir `step` ifade. Kullanarak bir `step` ısı Haritası renklendirme ifade aralıkları içine görsel olarak ve bu nedenle daha dağılımı ya da radar stili harita benzer yoğunluklu keser.  

```javascript 
var layer = new atlas.layer.HeatMapLayer(datasource, null, {
    color: [
        'step',
        ['heatmap-density'],
        'transparent',
        0.01, 'navy',
        0.25, 'navy',
        0.5, 'green',
        0.75, 'yellow',
        1, 'red'
    ]
});
```

Daha fazla bilgi için [ısı Haritası katman eklemek](map-add-heat-map-layer.md) belgeleri.

### <a name="line-progress-expression"></a>Çizgi ilerleme ifadesi

Bir çizgi ilerleme ifadesi bir gradyan çizgi katmanı satır boyunca ilerleme durumunu alır ve olarak tanımlanan `['line-progress']`. Bu değer 0 ile 1 arasında bir sayı ve birlikte kullanılan bir `interpolation` veya `step` ifade. Bu ifade ile yalnızca kullanılabilir [strokeGradient seçeneği]( https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest#strokegradient) çizgi katmanı. 

> [!NOTE]
> `strokeGradient` Çizgi katmanı seçeneğini gerektirir `lineMetrics` ayarlamak için seçenek veri kaynağının `true`.

**Örnek**

Aşağıdaki örnekte `['line-progress']` vuruşunun bir satır için bir renk gradyanı uygulamak için ifade.

```javascript
var layer = new atlas.layer.LineLayer(datasource, null, {
    strokeGradient: [
        'interpolate',
        ['linear'],
        ['line-progress'],
        0, "blue",
        0.1, "royalblue",
        0.3, "cyan",
        0.5, "lime",
        0.7, "yellow",
        1, "red"
    ]
});
```

[Canlı örneğe bakın](map-add-shape.md#line-stroke-gradient)

### <a name="text-field-format-expression"></a>Metin alanı biçim ifadesi

Metin alanı biçimi ifade ile kullanılan `textField` sembol katmanların seçeneği `textOptions` karışık metin biçimlendirmesini sağlamak için özellik. Bu ifade, bir dizi Giriş dizeleri ve biçimlendirme seçenekleri belirtilmesini sağlar. Aşağıdaki seçenekler, her bir Giriş dizesinin Bu ifadedeki için belirtilebilir.

 * `'font-scale'` -Ölçeklendirme çarpanı için yazı tipi boyutunu belirtir. Belirtilmişse bu değer kılar `size` özelliği `textOptions` için tek tek dize.
 * `'text-font'` -Bu dize için kullanılması gereken bir veya birden çok yazı tipi aileleri belirtir. Belirtilmişse bu değer kılar `font` özelliği `textOptions` için tek tek dize.

Aşağıdaki sözde kod metin alanı biçimi ifadesi yapısını tanımlar. 

```javascript
[
    'format', 
    input1: string, 
    options1: { 
        'font-scale': number, 
        'text-font': string[] 
    },
    input2: string, 
    options2: { 
        'font-scale': number, 
        'text-font': string[] 
    },
    …
]
```

**Örnek**

Aşağıdaki örnek, bir kalın yazı tipi ekleyerek ve yazı tipi boyutunu ölçeklendirme metin alanına biçimlendirir `title` özelliğinin özelliği. Bu örnek ayrıca ekler `subtitle` bir yeni satır ile genişletilmiş bir özelliği özelliği aşağı yazı tipi boyutu.

```javascript
var layer = new atlas.layer.SymbolLayer(datasource, null, {
    textOptions: {
        textField: [
            'format',

            //Bold the title property and scale its font size up.
            ['get', 'title'],
            {
                'text-font': ['literal', ['StandardFont-Bold']],
                'font-scale': 1.25
            },

            '\n', {},   //Add a new line without any formatting.

            //Scale the font size down of the subtitle property. 
            ['get', 'subtitle'],
            { 'font-scale': 0.75 }
        ]
    }
});
```

Bu katman, aşağıdaki resimde gösterildiği gibi noktası özelliği işlenir:
 
<center>

![Biçimlendirilmiş metin alanı noktası özelliğiyle görüntüsü](media/how-to-expressions/text-field-format-expression.png) </center>

### <a name="number-format-expression"></a>Sayı biçim ifadesi

`number-format` İfade yalnızca kullanılabilir ile `textField` sembol katmanının seçeneği. Bu ifade, sağlanan sayıyı biçimlendirilmiş bir dizeye dönüştürür. JavaScript'ın bu ifadeyi sarar [Number.toLocalString](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number/toLocaleString) işlev ve aşağıdaki seçenekleri kümesini destekler.

 * `locale` -İçin belirtilen dil ile uygun şekilde dizeleri sayılara dönüştürme için bu seçeneği belirtin. Başarılı bir [BCP 47 dil etiketi](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Intl#Locale_identification_and_negotiation) halinde bu seçeneği.
 * `currency` -Sayı bir para birimini temsil eden bir dizeye dönüştürmek için. Olası değerler [ISO 4217 para birimi kodları](https://en.wikipedia.org/wiki/ISO_4217)ABD Doları "ABD", "Euro" için EUR veya Çince RMB için "CNY" gibi.
 * `'min-fraction-digits'` -Ondalık sayının dize sürümde eklenecek en az sayısını belirtir.
 * `'max-fraction-digits'` -Ondalık sayının dize sürümünde içerecek şekilde en fazla sayısını belirtir.

Aşağıdaki sözde kod metin alanı biçimi ifadesi yapısını tanımlar. 

```javascript
[
    'number-format', 
    input: number, 
    options: {
        locale: string, 
        currency: string, 
        'min-fraction-digits': number, 
        'max-fraction-digits': number
    }
]
```

**Örnek**

Aşağıdaki örnekte bir `number-format` değiştirmek için ifade nasıl `revenue` noktası özelliği özelliği işlenir `textField` seçeneği bir simgenin bir ABD Doları değer göründüğü şekilde katman.

```javascript
var layer = new atlas.layer.SymbolLayer(datasource, null, {
    textOptions: {
        textField: [
            'number-format', 
            ['get', 'revenue'], 
            { ‘currency’: 'USD' }
        ],

        offset: [0, 0.75]
    }
});
```

Bu katman, aşağıdaki resimde gösterildiği gibi noktası özelliği işlenir:

<center>

![Sayı biçimi ifade örneği](media/how-to-expressions/number-format-expression.png) </center>

## <a name="zoom-expression"></a>Yakınlaştırma ifadesi

A `zoom` ifade geçerli harita yakınlaştırma düzeyini işleme zaman almak için kullanılır ve olarak tanımlanan `['zoom']`. Bu ifade, haritanın en düşük ve en yüksek yakınlaştırma düzeyinde aralığında bir sayı döndürür. Bu ifade kullanarak stilleri harita yakınlaştırma düzeyini değiştikçe dinamik olarak değiştirilmesine izin verir. `zoom` İfade yalnızca kullanılabilir ile `interpolate` ve `step` ifadeler.

**Örnek**

Varsayılan olarak, tüm yakınlaştırma düzeyleri için bir sabit piksel RADIUS yarıçaplarını ısı Haritası katmanda işlenen veri noktası var. Harita uzaklaştırılacağını gibi birlikte veri toplama ve ısı Haritası katman farklı görünüyor. A `zoom` ifade, her veri noktası aynı fiziksel alan harita kapsar, her yakınlaştırma düzeyi için RADIUS ölçeklendirmek için kullanılabilir. Bu konum ısı Haritası katman daha statik ve tutarlı hale getirir. Her harita yakınlaştırma düzeyini iki katı daha fazla piksel, dikey ve yatay önceki yakınlaştırma düzeyi vardır. RADIUS her yakınlaştırma düzeyi ile iki şekilde ölçeklendirme tüm yakınlaştırma düzeylerinin tutarlı görünen bir ısı Haritası oluşturun. Bu kullanarak gerçekleştirilebilir `zoom` ifadeyle bir `base 2 exponential interpolation` aşağıda gösterildiği gibi bir ifade. 

```javascript 
var layer = new atlas.layer.HeatMapLayer(datasource, null, {
    radius: [
        'interpolate',
        ['exponential', 2],
        ['zoom'],
        
        //For zoom level 1 set the radius to 2 pixels.
        10, 2,

        //Between zoom level 1 and 19, exponentially scale the radius from 2 pixels to 10,000 pixels.
        19, 10000
    ]
};
```

[Canlı örneğe bakın](map-add-heat-map-layer.md#consistent-zoomable-heat-map)

## <a name="variable-binding-expressions"></a>Değişken bağlama ifadeleri

Değişken bağlama ifadeleri, hesaplamanın sonuçlarını bir değişkende depolayın, başka bir yerde bir deyimde birden çok kez onu yeniden hesaplama yapmak zorunda kalmadan başvurulabilir. Çok sayıda hesaplama içeren ifadeler için kullanışlı bir iyileştirme budur.

| İfade | Dönüş türü | Açıklama |
|--------------|---------------|--------------|
| \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;'let'<br/>&nbsp;&nbsp;&nbsp;&nbsp;name1: dize<br/>&nbsp;&nbsp;&nbsp;&nbsp;Değer1: herhangi biri<br/>&nbsp;&nbsp;&nbsp;&nbsp;name2: dize<br/>&nbsp;&nbsp;&nbsp;&nbsp;Value2: herhangi biri<br/>&nbsp;&nbsp;&nbsp;&nbsp;…<br/>&nbsp;&nbsp;&nbsp;&nbsp;childExpression<br/>\] | | Bir veya daha fazla değer tarafından kullanılacak değişkenleri olarak depolayan `var` ifade alt ifadede sonucunu döndürür. |
| `['var', name: string]` | herhangi biri | Kullanılarak oluşturulmuş bir değişkene başvuruyor `let` ifade. |

**Örnek**

Bu örnekte geliri sıcaklık oranı ve kullandığı hesaplar bir ifade bir `case` bu değeri Boole farklı işlemlerin değerlendirilecek ifade. `let` İfade yalnızca bir kez hesaplanacak gerekir böylece sıcaklık oranına göre gelir depolamak için kullanılan ve `var` ifadesi bu değişken, yeniden hesaplama yapmak zorunda kalmadan gerektiği sıklıkta başvuruyor.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        //Divide the point features `revenue` property by the `temperature` property and store it in a variable called `ratio`.
        'let', 'ratio', ['/', ['get', 'revenue'], ['get', 'temperature']],
        //Evaluate the child expression in which the stored variable will be used.
        [
            'case',

            //Check to see if the ratio is less than 100, return 'red'.
            ['<', ['var', 'ratio'], 100],
            'red',

            //Check to see if the ratio is less than 200, return 'green'.
            ['<', ['var', 'ratio'], 200],
            'green',

            //Return `blue` for values greater or equal to 200.
            'blue'
        ]
    ]
});
```

## <a name="next-steps"></a>Sonraki adımlar

İfadeleri uygulamak daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"] 
> [Sembol katmanı Ekle](map-add-pin.md)

> [!div class="nextstepaction"] 
> [Kabarcık katmanı Ekle](map-add-bubble-layer.md)

> [!div class="nextstepaction"] 
> [Şekil ekleme](map-add-shape.md)

> [!div class="nextstepaction"] 
> [Isı Haritası katman ekleyin](map-add-heat-map-layer.md)

İfadeleri destekleyen katman seçenekleri hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"] 
> [BubbleLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.bubblelayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"] 
> [HeatMapLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.heatmaplayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"] 
> [LineLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"] 
> [PolygonLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"] 
> [SymbolLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions?view=azure-iot-typescript-latest) 
