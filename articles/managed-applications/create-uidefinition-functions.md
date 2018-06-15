---
title: Azure yönetilen uygulama kullanıcı Arabirimi tanımı işlevlerin oluşturma | Microsoft Docs
description: Azure yönetilen uygulamaları için kullanıcı Arabirimi tanımları oluşturulurken kullanılacak işlevleri açıklanmaktadır
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/12/2017
ms.author: tomfitz
ms.openlocfilehash: a01a59a7e8c9757cb41d328cd26a34fa219f9152
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34304513"
---
# <a name="createuidefinition-functions"></a>CreateUiDefinition işlevleri
Bu bölüm bir CreateUiDefinition tüm desteklenen işlevlerini imzaları içerir.

Bir işlevi kullanmak için köşeli bildirimiyle koyun. Örneğin:

```json
"[function()]"
```

Dizeler ve diğer işlevleri işlevi için parametre olarak başvurulabilir, ancak dizeler tek tırnak içine alınmalıdır. Örneğin:

```json
"[fn1(fn2(), 'foobar')]"
```

Uygunsa, dot işleci kullanılarak bir işlev çıktısını özelliklerini başvuruda bulunabilir. Örneğin:

```json
"[func().prop1]"
```

## <a name="referencing-functions"></a>Başvuru işlevleri
Bu işlevler, özellikleri veya bir CreateUiDefinition bağlamında çıkışları başvurmak için kullanılabilir.

### <a name="basics"></a>temel kavramları
Temel kavramları adımında tanımlı bir öğenin çıktı değerleri döndürür.

Aşağıdaki örnek adlı öğe çıktısı verir `foo` temelleri adımda:

```json
"[basics('foo')]"
```

### <a name="steps"></a>adımlar
Belirtilen adımında tanımlı bir öğenin çıktı değerleri döndürür. Temel kavramları adımda öğelerin çıkış değerleri almak için kullanın `basics()` yerine.

Aşağıdaki örnek adlı öğe çıktısı verir `bar` adlı adımda `foo`:

```json
"[steps('foo').bar]"
```

### <a name="location"></a>location
Temel adımı veya geçerli bağlamı içinde seçilen konumu döndürür.

Aşağıdaki örnek döndürebilir `"westus"`:

```json
"[location()]"
```

## <a name="string-functions"></a>Dize işlevleri
Bu işlevler yalnızca JSON dizelerle kullanılabilir.

### <a name="concat"></a>concat
Bir veya daha fazla art arda ekler.

Örneğin, varsa çıkış değeri `element1` varsa `"bar"`, bu örnek dizesini döndürür sonra `"foobar!"`:

```json
"[concat('foo', steps('step1').element1), '!']"
```

### <a name="substring"></a>substring
Belirtilen dizenin alt dizeyi döndürür. Alt dizeyi belirtilen dizinden başlatır ve belirtilen uzunluğa sahip.

Aşağıdaki örnek verir `"ftw"`:

```json
"[substring('azure-ftw!!!1one', 6, 3)]"
```

### <a name="replace"></a>Değiştir
Geçerli dizesinde belirtilen dizenin tüm oluşumlarını başka dize ile değiştirilir bir dize döndürür.

Aşağıdaki örnek verir `"Everything is awesome!"`:

```json
"[replace('Everything is terrible!', 'terrible', 'awesome')]"
```

### <a name="guid"></a>GUID
Genel olarak benzersiz bir dize (GUID) oluşturur.

Aşağıdaki örnek döndürebilir `"c7bc8bdc-7252-4a82-ba53-7c468679a511"`:

```json
"[guid()]"
```

### <a name="tolower"></a>toLower
Küçük harfe dönüştürülmüş bir dize döndürür.

Aşağıdaki örnek verir `"foobar"`:

```json
"[toLower('FOOBAR')]"
```

### <a name="toupper"></a>toUpper
Büyük harfe dönüştürülmüş bir dize döndürür.

Aşağıdaki örnek verir `"FOOBAR"`:

```json
"[toUpper('foobar')]"
```

## <a name="collection-functions"></a>Toplama işlevleri
Bu işlevler, JSON dizeler, dizileri ve nesneleri gibi koleksiyonlar ile kullanılabilir.

### <a name="contains"></a>içerir
Döndürür `true` bir dizeyi belirtilen alt dizeyi içeren belirtilen değer bir dizi içeriyor veya belirtilen anahtar bir nesne içerir.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek verir `true`:

```json
"[contains('foobar', 'foo')]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
Varsayın `element1` döndürür `[1, 2, 3]`. Aşağıdaki örnek verir `false`:

```json
"[contains(steps('foo').element1, 4)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesnesi
Varsayın `element1` döndürür:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Aşağıdaki örnek verir `true`:

```json
"[contains(steps('foo').element1, 'key1')]"
```

### <a name="length"></a>uzunluğu
Bir dize, değer dizideki veya nesnedeki anahtar sayısı karakterlerin sayısını döndürür.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek verir `6`:

```json
"[length('foobar')]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
Varsayın `element1` döndürür `[1, 2, 3]`. Aşağıdaki örnek verir `3`:

```json
"[length(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesnesi
Varsayın `element1` döndürür:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Aşağıdaki örnek verir `2`:

```json
"[length(steps('foo').element1)]"
```

### <a name="empty"></a>boş
Döndürür `true` dize, dizi veya nesne null veya boş ise.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek verir `true`:

```json
"[empty('')]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
Varsayın `element1` döndürür `[1, 2, 3]`. Aşağıdaki örnek verir `false`:

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesnesi
Varsayın `element1` döndürür:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Aşağıdaki örnek verir `false`:

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-4-null-and-undefined"></a>Örnek 4: boş ve tanımlanmamış
Varsayın `element1` olan `null` veya tanımlanmamış. Aşağıdaki örnek verir `true`:

```json
"[empty(steps('foo').element1)]"
```

### <a name="first"></a>ilk
Belirtilen dizenin ilk karakteri döndürür; Belirtilen dizinin ilk değerini; veya ilk anahtarı ve belirtilen nesnenin değeri.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek verir `"f"`:

```json
"[first('foobar')]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
Varsayın `element1` döndürür `[1, 2, 3]`. Aşağıdaki örnek verir `1`:

```json
"[first(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesnesi
Varsayın `element1` döndürür:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
Aşağıdaki örnek verir `{"key1": "foobar"}`:

```json
"[first(steps('foo').element1)]"
```

### <a name="last"></a>Son
Belirtilen dize, son değerini belirtilen dizi veya son anahtarı ve belirtilen nesne değerini son karakteri döndürür.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek verir `"r"`:

```json
"[last('foobar')]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
Varsayın `element1` döndürür `[1, 2, 3]`. Aşağıdaki örnek verir `2`:

```json
"[last(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesnesi
Varsayın `element1` döndürür:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Aşağıdaki örnek verir `{"key2": "raboof"}`:

```json
"[last(steps('foo').element1)]"
```

### <a name="take"></a>Al
Belirtilen sayıda ardışık karakteri dize başından, bitişik değerleri dizisi başından itibaren belirli sayıda veya belirtilen sayıda bitişik anahtarlar ve değerler nesne başından döndürür.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek verir `"foo"`:

```json
"[take('foobar', 3)]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
Varsayın `element1` döndürür `[1, 2, 3]`. Aşağıdaki örnek verir `[1, 2]`:

```json
"[take(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesnesi
Varsayın `element1` döndürür:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Aşağıdaki örnek verir `{"key1": "foobar"}`:

```json
"[take(steps('foo').element1, 1)]"
```

### <a name="skip"></a>Atla
Belirtilen bir koleksiyondaki öğelerin sayısı atlar ve kalan öğeleri döndürür.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek verir `"bar"`:

```json
"[skip('foobar', 3)]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
Varsayın `element1` döndürür `[1, 2, 3]`. Aşağıdaki örnek verir `[3]`:

```json
"[skip(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesnesi
Varsayın `element1` döndürür:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
Aşağıdaki örnek verir `{"key2": "raboof"}`:

```json
"[skip(steps('foo').element1, 1)]"
```

## <a name="logical-functions"></a>Mantıksal işlevler
Bu işlevler koşulları içinde kullanılabilir. Bazı işlevler tüm JSON veri türlerini desteklemiyor olabilir.

### <a name="equals"></a>şuna eşittir:
Döndürür `true` parametrelerinin her ikisini de aynı türde ve değer varsa. Bu işlev tüm JSON veri türlerini destekler.

Aşağıdaki örnek verir `true`:

```json
"[equals(0, 0)]"
```

Aşağıdaki örnek verir `true`:

```json
"[equals('foo', 'foo')]"
```

Aşağıdaki örnek verir `false`:

```json
"[equals('abc', ['a', 'b', 'c'])]"
```

### <a name="less"></a>daha az
Döndürür `true` ilk parametre ikinci parametre değerinden kesinlikle küçük ise. Bu işlev parametreleri yalnızca türü numarası ve dize destekler.

Aşağıdaki örnek verir `true`:

```json
"[less(1, 2)]"
```

Aşağıdaki örnek verir `false`:

```json
"[less('9', '10')]"
```

### <a name="lessorequals"></a>lessOrEquals
Döndürür `true` ilk parametre ikinci parametre küçük veya buna eşit olması durumunda. Bu işlev parametreleri yalnızca türü numarası ve dize destekler.

Aşağıdaki örnek verir `true`:

```json
"[lessOrEquals(2, 2)]"
```

### <a name="greater"></a>büyük
Döndürür `true` ilk parametre ikinci parametre kesinlikle büyük ise. Bu işlev parametreleri yalnızca türü numarası ve dize destekler.

Aşağıdaki örnek verir `false`:

```json
"[greater(1, 2)]"
```

Aşağıdaki örnek verir `true`:

```json
"[greater('9', '10')]"
```

### <a name="greaterorequals"></a>greaterOrEquals
Döndürür `true` ilk parametre ikinci parametre eşit veya daha büyük olduğunda. Bu işlev parametreleri yalnızca türü numarası ve dize destekler.

Aşağıdaki örnek verir `true`:

```json
"[greaterOrEquals(2, 2)]"
```

### <a name="and"></a>ve
Döndürür `true` için tüm parametreleri değerlendirin varsa `true`. Bu işlev yalnızca Boole türünde iki veya daha fazla parametre destekler.

Aşağıdaki örnek verir `true`:

```json
"[and(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

Aşağıdaki örnek verir `false`:

```json
"[and(equals(0, 0), greater(1, 2))]"
```

### <a name="or"></a>or
Döndürür `true` parametrelerin en az biri değerlendirilirse `true`. Bu işlev yalnızca Boole türünde iki veya daha fazla parametre destekler.

Aşağıdaki örnek verir `true`:

```json
"[or(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

Aşağıdaki örnek verir `true`:

```json
"[or(equals(0, 0), greater(1, 2))]"
```

### <a name="not"></a>değil
Döndürür `true` parametresi değerlendirilirse `false`. Bu işlev yalnızca Boole türünde parametreleri destekler.

Aşağıdaki örnek verir `true`:

```json
"[not(false)]"
```

Aşağıdaki örnek verir `false`:

```json
"[not(equals(0, 0))]"
```

### <a name="coalesce"></a>birleşim
İlk null olmayan parametresinin değeri döndürür. Bu işlev tüm JSON veri türlerini destekler.

Varsayın `element1` ve `element2` tanımlanmaz. Aşağıdaki örnek verir `"foobar"`:

```json
"[coalesce(steps('foo').element1, steps('foo').element2, 'foobar')]"
```

## <a name="conversion-functions"></a>Dönüşüm işlevleri
Bu işlevler, JSON veri türleri ve Kodlamalar arasında değerleri dönüştürmek için kullanılabilir.

### <a name="int"></a>Int
Parametresi, bir tamsayıya dönüştürür. Bu işlev türü numarası ve dize parametreleri destekler.

Aşağıdaki örnek verir `1`:

```json
"[int('1')]"
```

Aşağıdaki örnek verir `2`:

```json
"[int(2.9)]"
```

### <a name="float"></a>float
Parametresi için bir kayan nokta dönüştürür. Bu işlev türü numarası ve dize parametreleri destekler.

Aşağıdaki örnek verir `1.0`:

```json
"[float('1.0')]"
```

Aşağıdaki örnek verir `2.9`:

```json
"[float(2.9)]"
```

### <a name="string"></a>dize
Parametresi bir dizeye dönüştürür. Bu işlev parametreleri tüm JSON veri türlerini destekler.

Aşağıdaki örnek verir `"1"`:

```json
"[string(1)]"
```

Aşağıdaki örnek verir `"2.9"`:

```json
"[string(2.9)]"
```

Aşağıdaki örnek verir `"[1,2,3]"`:

```json
"[string([1,2,3])]"
```

Aşağıdaki örnek verir `"{"foo":"bar"}"`:

```json
"[string({\"foo\":\"bar\"})]"
```

### <a name="bool"></a>bool
Parametresi bir Boolean değerine dönüştürür. Bu işlev türü numarası, dize ve Boolean parametreleri destekler. Boole değerlerini JavaScript'te benzeyen, herhangi bir değer dışında `0` veya `'false'` döndürür `true`.

Aşağıdaki örnek verir `true`:

```json
"[bool(1)]"
```

Aşağıdaki örnek verir `false`:

```json
"[bool(0)]"
```

Aşağıdaki örnek verir `true`:

```json
"[bool(true)]"
```

Aşağıdaki örnek verir `true`:

```json
"[bool('true')]"
```

### <a name="parse"></a>Ayrıştırma
Parametresi, yerel bir türe dönüştürür. Diğer bir deyişle, bu işlev, tersidir `string()`. Bu işlev yalnızca dize türünde parametreleri destekler.

Aşağıdaki örnek verir `1`:

```json
"[parse('1')]"
```

Aşağıdaki örnek verir `true`:

```json
"[parse('true')]"
```

Aşağıdaki örnek verir `[1,2,3]`:

```json
"[parse('[1,2,3]')]"
```

Aşağıdaki örnek verir `{"foo":"bar"}`:

```json
"[parse('{\"foo\":\"bar\"}')]"
```

### <a name="encodebase64"></a>encodeBase64
Base-64 kodlu bir dize parametresi kodlar. Bu işlev yalnızca dize türünde parametreleri destekler.

Aşağıdaki örnek verir `"Zm9vYmFy"`:

```json
"[encodeBase64('foobar')]"
```

### <a name="decodebase64"></a>decodeBase64
Base-64 kodlu bir dize parametresinden kodunu çözer. Bu işlev yalnızca dize türünde parametreleri destekler.

Aşağıdaki örnek verir `"foobar"`:

```json
"[decodeBase64('Zm9vYmFy')]"
```

### <a name="encodeuricomponent"></a>Encodeurıcomponent
Kodlanan URL dizesi parametresi olarak kodlar. Bu işlev yalnızca dize türünde parametreleri destekler.

Aşağıdaki örnek verir `"https%3A%2F%2Fportal.azure.com%2F"`:

```json
"[encodeUriComponent('https://portal.azure.com/')]"
```

### <a name="decodeuricomponent"></a>Decodeurıcomponent
Bir URL kodlanmış dize parametresinden kodunu çözer. Bu işlev yalnızca dize türünde parametreleri destekler.

Aşağıdaki örnek verir `"https://portal.azure.com/"`:

```json
"[decodeUriComponent('https%3A%2F%2Fportal.azure.com%2F')]"
```

## <a name="math-functions"></a>Matematiksel işlevler
### <a name="add"></a>ekle
İki sayı ekleyen ve sonuç döndürür.

Aşağıdaki örnek verir `3`:

```json
"[add(1, 2)]"
```

### <a name="sub"></a>Sub
İkinci sayı ilk sayıdan çıkarır ve sonucu döndürür.

Aşağıdaki örnek verir `1`:

```json
"[sub(3, 2)]"
```

### <a name="mul"></a>mul
İki sayıyı çarpar ve sonucu döndürür.

Aşağıdaki örnek verir `6`:

```json
"[mul(2, 3)]"
```

### <a name="div"></a>div
İkinci sayı ilk sayıyı böler ve sonucu döndürür. Sonuç her zaman bir tamsayıdır.

Aşağıdaki örnek verir `2`:

```json
"[div(6, 3)]"
```

### <a name="mod"></a>mod
İkinci sayı ilk sayıyı böler ve kalanı döndürür.

Aşağıdaki örnek verir `0`:

```json
"[mod(6, 3)]"
```

Aşağıdaki örnek verir `2`:

```json
"[mod(6, 4)]"
```

### <a name="min"></a>dk
İki sayının küçük döndürür.

Aşağıdaki örnek verir `1`:

```json
"[min(1, 2)]"
```

### <a name="max"></a>en çok
Büyük iki sayının döndürür.

Aşağıdaki örnek verir `2`:

```json
"[max(1, 2)]"
```

### <a name="range"></a>Aralık
Belirtilen aralıkta tam sayı sayılardan oluşan bir dizi oluşturur.

Aşağıdaki örnek verir `[1,2,3]`:

```json
"[range(1, 3)]"
```

### <a name="rand"></a>rand
Belirtilen aralık içinde rastgele bir tamsayı döndürür. Bu işlev, şifreleme açısından güvenli rastgele sayılar oluşturmaz.

Aşağıdaki örnek döndürebilir `42`:

```json
"[rand(-100, 100)]"
```

### <a name="floor"></a>Kat
Belirtilen sayıdan küçük veya eşit en büyük tamsayıyı döndürür.

Aşağıdaki örnek verir `3`:

```json
"[floor(3.14)]"
```

### <a name="ceil"></a>ceil
Büyük veya eşit belirtilen en büyük tamsayıyı döndürür.

Aşağıdaki örnek verir `4`:

```json
"[ceil(3.14)]"
```

## <a name="date-functions"></a>Date işlevleri
### <a name="utcnow"></a>utcNow
Bir dize, yerel bilgisayardaki geçerli tarih ve saati ISO 8601 biçiminde döndürür.

Aşağıdaki örnek döndürebilir `"1990-12-31T23:59:59.000Z"`:

```json
"[utcNow()]"
```

### <a name="addseconds"></a>saniyeEkle
Bir tam sayı saniye sayısı için belirtilen zaman damgası ekler.

Aşağıdaki örnek verir `"1991-01-01T00:00:00.000Z"`:

```json
"[addSeconds('1990-12-31T23:59:60Z', 1)]"
```

### <a name="addminutes"></a>addMinutes
Bir tam sayı dakika sayısı için belirtilen zaman damgası ekler.

Aşağıdaki örnek verir `"1991-01-01T00:00:59.000Z"`:

```json
"[addMinutes('1990-12-31T23:59:59Z', 1)]"
```

### <a name="addhours"></a>addHours
Bir tamsayı saat için belirtilen zaman damgası ekler.

Aşağıdaki örnek verir `"1991-01-01T00:59:59.000Z"`:

```json
"[addHours('1990-12-31T23:59:59Z', 1)]"
```

## <a name="next-steps"></a>Sonraki adımlar
* Bir bir giriş için Azure Resource Manager'ı için bkz: [Azure Resource Manager'a genel bakış](../azure-resource-manager/resource-group-overview.md).

