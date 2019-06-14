---
title: UI tanımı işlevleri Azure yönetilen uygulaması oluşturma | Microsoft Docs
description: Azure yönetilen uygulamalar için kullanıcı Arabirimi tanımları oluştururken kullanılacak işlevleri açıklar
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
ms.openlocfilehash: 80fd593eecf189d516a8c9d7ef2a94ec9f23fc39
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60587929"
---
# <a name="createuidefinition-functions"></a>CreateUiDefinition işlevleri
Bu bölüm bir CreateUiDefinition desteklenen tüm işlevleri imzaları içerir.

Bir işlevi kullanmak için köşeli ayraç ile bildirim alın. Örneğin:

```json
"[function()]"
```

Dizeler ve diğer işlevler bir işlev için parametre olarak başvurulabilir, ancak dizeleri tek tırnak içine alınmalıdır. Örneğin:

```json
"[fn1(fn2(), 'foobar')]"
```

Uygunsa dot işleci kullanılarak, bir işlevin çıktısı özelliklerini başvurabilirsiniz. Örneğin:

```json
"[func().prop1]"
```

## <a name="referencing-functions"></a>Başvuru işlevleri
Bu işlevler, çıkışlar özellikleri veya bir CreateUiDefinition bağlamında başvurmak için kullanılabilir.

### <a name="basics"></a>Temel bilgileri
Temel adımında tanımlı bir öğenin çıkış değerleri döndürür.

Aşağıdaki örnek adlı bir öğe çıktısını verir `foo` temel bilgileri adım:

```json
"[basics('foo')]"
```

### <a name="steps"></a>adımlar
Belirtilen adımında tanımlı bir öğenin çıkış değerleri döndürür. Temel bilgileri adım öğe çıkış değerlerini almak için kullanın `basics()` yerine.

Aşağıdaki örnek adlı bir öğe çıktısını verir `bar` adlı adımda `foo`:

```json
"[steps('foo').bar]"
```

### <a name="location"></a>location
Temel bilgileri adım veya geçerli bağlam içinde seçilen konumu döndürür.

Aşağıdaki örnek döndürebilir `"westus"`:

```json
"[location()]"
```

## <a name="string-functions"></a>Dize işlevleri
Bu işlevler yalnızca JSON dizeleri ile kullanılabilir.

### <a name="concat"></a>concat
Bir veya daha fazla dizeleri birleştirir.

Örneğin, çıkış değeri `element1` varsa `"bar"`, bu örnekte dize verir `"foobar!"`:

```json
"[concat('foo', steps('step1').element1, '!')]"
```

### <a name="substring"></a>alt dize
Belirtilen dizenin bir alt dizeyi döndürür. Alt dizenin belirtilen dizindeki başlatır ve belirtilen uzunluğa sahip.

Aşağıdaki örnek döndürür `"ftw"`:

```json
"[substring('azure-ftw!!!1one', 6, 3)]"
```

### <a name="replace"></a>Değiştir
Belirtilen dize geçerli dizedeki tüm tekrarlarını başka bir dize ile değiştirilir bir dize döndürür.

Aşağıdaki örnek döndürür `"Everything is awesome!"`:

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

Aşağıdaki örnek döndürür `"foobar"`:

```json
"[toLower('FOOBAR')]"
```

### <a name="toupper"></a>toUpper
Büyük harfe dönüştürülmüş bir dize döndürür.

Aşağıdaki örnek döndürür `"FOOBAR"`:

```json
"[toUpper('foobar')]"
```

## <a name="collection-functions"></a>Toplama işlevleri
Bu işlevler, koleksiyon, JSON dizeler, diziler ve nesneleri gibi kullanılabilir.

### <a name="contains"></a>içerir
Döndürür `true` belirtilen alt dizeyi içerir belirtilen değer bir dizi içeriyor veya belirtilen anahtar bir nesne içerir.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek döndürür `true`:

```json
"[contains('foobar', 'foo')]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
Varsayar `element1` döndürür `[1, 2, 3]`. Aşağıdaki örnek döndürür `false`:

```json
"[contains(steps('foo').element1, 4)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
Varsayar `element1` döndürür:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Aşağıdaki örnek döndürür `true`:

```json
"[contains(steps('foo').element1, 'key1')]"
```

### <a name="length"></a>length
Bir dize, bir dizideki değerlerin sayısını veya bir nesnedeki anahtar sayısı karakter sayısını döndürür.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek döndürür `6`:

```json
"[length('foobar')]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
Varsayar `element1` döndürür `[1, 2, 3]`. Aşağıdaki örnek döndürür `3`:

```json
"[length(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
Varsayar `element1` döndürür:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Aşağıdaki örnek döndürür `2`:

```json
"[length(steps('foo').element1)]"
```

### <a name="empty"></a>boş
Döndürür `true` dize, dizi veya nesne null veya boş ise.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek döndürür `true`:

```json
"[empty('')]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
Varsayar `element1` döndürür `[1, 2, 3]`. Aşağıdaki örnek döndürür `false`:

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
Varsayar `element1` döndürür:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Aşağıdaki örnek döndürür `false`:

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-4-null-and-undefined"></a>Örnek 4: null ve tanımlanmamış
Varsayar `element1` olan `null` veya tanımlanmamış. Aşağıdaki örnek döndürür `true`:

```json
"[empty(steps('foo').element1)]"
```

### <a name="first"></a>ilk
Belirtilen dizenin ilk karakteri döndürür; Belirtilen dizinin ilk değerini; veya ilk anahtarı ve belirtilen nesnenin değeri.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek döndürür `"f"`:

```json
"[first('foobar')]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
Varsayar `element1` döndürür `[1, 2, 3]`. Aşağıdaki örnek döndürür `1`:

```json
"[first(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
Varsayar `element1` döndürür:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
Aşağıdaki örnek döndürür `{"key1": "foobar"}`:

```json
"[first(steps('foo').element1)]"
```

### <a name="last"></a>Son
Belirtilen dize, son değerini belirtilen diziye veya son anahtarı ve belirtilen nesnenin değerini son karakteri döndürür.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek döndürür `"r"`:

```json
"[last('foobar')]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
Varsayar `element1` döndürür `[1, 2, 3]`. Aşağıdaki örnek döndürür `2`:

```json
"[last(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
Varsayar `element1` döndürür:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Aşağıdaki örnek döndürür `{"key2": "raboof"}`:

```json
"[last(steps('foo').element1)]"
```

### <a name="take"></a>sınav zamanı
Belirtilen bir dizenin başından itibaren bitişik karakter sayısı, bitişik değerleri dizinin başından itibaren belirtilen sayıda veya bitişik anahtarlar ve değerler nesne başından itibaren belirtilen sayıda döndürür.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek döndürür `"foo"`:

```json
"[take('foobar', 3)]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
Varsayar `element1` döndürür `[1, 2, 3]`. Aşağıdaki örnek döndürür `[1, 2]`:

```json
"[take(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
Varsayar `element1` döndürür:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Aşağıdaki örnek döndürür `{"key1": "foobar"}`:

```json
"[take(steps('foo').element1, 1)]"
```

### <a name="skip"></a>Atla
Belirtilen sayıda bir koleksiyondaki öğeleri atlar ve kalan öğeleri döndürür.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek döndürür `"bar"`:

```json
"[skip('foobar', 3)]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
Varsayar `element1` döndürür `[1, 2, 3]`. Aşağıdaki örnek döndürür `[3]`:

```json
"[skip(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
Varsayar `element1` döndürür:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
Aşağıdaki örnek döndürür `{"key2": "raboof"}`:

```json
"[skip(steps('foo').element1, 1)]"
```

## <a name="logical-functions"></a>Mantıksal işlevler
Bu işlevler koşullular içinde kullanılabilir. Bazı işlevler tüm JSON veri türlerini desteklemiyor olabilir.

### <a name="equals"></a>eşittir
Döndürür `true` her iki parametre aynı türde ve değer varsa. Bu işlev, tüm JSON veri türlerini destekler.

Aşağıdaki örnek döndürür `true`:

```json
"[equals(0, 0)]"
```

Aşağıdaki örnek döndürür `true`:

```json
"[equals('foo', 'foo')]"
```

Aşağıdaki örnek döndürür `false`:

```json
"[equals('abc', ['a', 'b', 'c'])]"
```

### <a name="less"></a>daha az
Döndürür `true` ilk parametre ikinci parametresinin kesinlikle küçük ise. Bu işlev, yalnızca, tür numarası ve dize parametreleri destekler.

Aşağıdaki örnek döndürür `true`:

```json
"[less(1, 2)]"
```

Aşağıdaki örnek döndürür `false`:

```json
"[less('9', '10')]"
```

### <a name="lessorequals"></a>lessOrEquals
Döndürür `true` ilk parametre, ikinci parametresinin değerinden küçük veya eşit ise. Bu işlev, yalnızca, tür numarası ve dize parametreleri destekler.

Aşağıdaki örnek döndürür `true`:

```json
"[lessOrEquals(2, 2)]"
```

### <a name="greater"></a>daha büyük
Döndürür `true` ilk parametre, ikinci parametresinin kesinlikle büyük olması durumunda. Bu işlev, yalnızca, tür numarası ve dize parametreleri destekler.

Aşağıdaki örnek döndürür `false`:

```json
"[greater(1, 2)]"
```

Aşağıdaki örnek döndürür `true`:

```json
"[greater('9', '10')]"
```

### <a name="greaterorequals"></a>greaterOrEquals
Döndürür `true` ilk parametresinin değerinden büyük veya eşittir ikinci parametre ise. Bu işlev, yalnızca, tür numarası ve dize parametreleri destekler.

Aşağıdaki örnek döndürür `true`:

```json
"[greaterOrEquals(2, 2)]"
```

### <a name="and"></a>ve
Döndürür `true` tüm parametreleri sonucunu verirse `true`. Bu işlev, yalnızca Boole türünde iki veya daha fazla parametrelerini destekler.

Aşağıdaki örnek döndürür `true`:

```json
"[and(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

Aşağıdaki örnek döndürür `false`:

```json
"[and(equals(0, 0), greater(1, 2))]"
```

### <a name="or"></a>or
Döndürür `true` parametrelerden en az biri değerlendirilirse `true`. Bu işlev, yalnızca Boole türünde iki veya daha fazla parametrelerini destekler.

Aşağıdaki örnek döndürür `true`:

```json
"[or(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

Aşağıdaki örnek döndürür `true`:

```json
"[or(equals(0, 0), greater(1, 2))]"
```

### <a name="not"></a>değil
Döndürür `true` parametresi değerlendirilirse `false`. Bu işlev, yalnızca Boole türünde parametreleri destekler.

Aşağıdaki örnek döndürür `true`:

```json
"[not(false)]"
```

Aşağıdaki örnek döndürür `false`:

```json
"[not(equals(0, 0))]"
```

### <a name="coalesce"></a>birleşim
İlk null olmayan parametre değerini döndürür. Bu işlev, tüm JSON veri türlerini destekler.

Varsayar `element1` ve `element2` tanımsızdır. Aşağıdaki örnek döndürür `"foobar"`:

```json
"[coalesce(steps('foo').element1, steps('foo').element2, 'foobar')]"
```

## <a name="conversion-functions"></a>Dönüştürme işlevleri
Bu işlevlerin değerlerini JSON veri türleri ve Kodlamalar arasında dönüştürmek için kullanılabilir.

### <a name="int"></a>int
Parametreyi bir tamsayıya dönüştürür. Bu işlev türü sayısı ve dize parametreleri destekler.

Aşağıdaki örnek döndürür `1`:

```json
"[int('1')]"
```

Aşağıdaki örnek döndürür `2`:

```json
"[int(2.9)]"
```

### <a name="float"></a>float
Parametre için bir kayan nokta dönüştürür. Bu işlev türü sayısı ve dize parametreleri destekler.

Aşağıdaki örnek döndürür `1.0`:

```json
"[float('1.0')]"
```

Aşağıdaki örnek döndürür `2.9`:

```json
"[float(2.9)]"
```

### <a name="string"></a>string
Parametreyi bir dizeye dönüştürür. Bu işlev, tüm JSON veri türlerinin parametrelerini destekler.

Aşağıdaki örnek döndürür `"1"`:

```json
"[string(1)]"
```

Aşağıdaki örnek döndürür `"2.9"`:

```json
"[string(2.9)]"
```

Aşağıdaki örnek döndürür `"[1,2,3]"`:

```json
"[string([1,2,3])]"
```

Aşağıdaki örnek döndürür `"{"foo":"bar"}"`:

```json
"[string({\"foo\":\"bar\"})]"
```

### <a name="bool"></a>bool
Parametresi bir Boolean değerine dönüştürür. Bu işlev türü sayısı, dize ve Boole parametreleri destekler. JavaScript içinde Boole değerlerini için benzer, herhangi bir değer dışında `0` veya `'false'` döndürür `true`.

Aşağıdaki örnek döndürür `true`:

```json
"[bool(1)]"
```

Aşağıdaki örnek döndürür `false`:

```json
"[bool(0)]"
```

Aşağıdaki örnek döndürür `true`:

```json
"[bool(true)]"
```

Aşağıdaki örnek döndürür `true`:

```json
"[bool('true')]"
```

### <a name="parse"></a>Ayrıştırma
Yerel bir tür parametresi dönüştürür. Diğer bir deyişle, bu işlev tersini, `string()`. Bu işlev, yalnızca dize türündeki parametrelerini destekler.

Aşağıdaki örnek döndürür `1`:

```json
"[parse('1')]"
```

Aşağıdaki örnek döndürür `true`:

```json
"[parse('true')]"
```

Aşağıdaki örnek döndürür `[1,2,3]`:

```json
"[parse('[1,2,3]')]"
```

Aşağıdaki örnek döndürür `{"foo":"bar"}`:

```json
"[parse('{\"foo\":\"bar\"}')]"
```

### <a name="encodebase64"></a>encodeBase64
Base 64 kodlu bir dize parametresi kodlar. Bu işlev, yalnızca dize türündeki parametrelerini destekler.

Aşağıdaki örnek döndürür `"Zm9vYmFy"`:

```json
"[encodeBase64('foobar')]"
```

### <a name="decodebase64"></a>decodeBase64
Base 64 kodlu bir dize parametresinden kodunu çözer. Bu işlev, yalnızca dize türündeki parametrelerini destekler.

Aşağıdaki örnek döndürür `"foobar"`:

```json
"[decodeBase64('Zm9vYmFy')]"
```

### <a name="encodeuricomponent"></a>Encodeurıcomponent
URL olarak kodlanmış bir dize parametresi kodlar. Bu işlev, yalnızca dize türündeki parametrelerini destekler.

Aşağıdaki örnek döndürür `"https%3A%2F%2Fportal.azure.com%2F"`:

```json
"[encodeUriComponent('https://portal.azure.com/')]"
```

### <a name="decodeuricomponent"></a>Decodeurıcomponent
Bir URL olarak kodlanmış dize parametresinden kodunu çözer. Bu işlev, yalnızca dize türündeki parametrelerini destekler.

Aşağıdaki örnek döndürür `"https://portal.azure.com/"`:

```json
"[decodeUriComponent('https%3A%2F%2Fportal.azure.com%2F')]"
```

## <a name="math-functions"></a>Matematik işlevleri
### <a name="add"></a>add
İki sayı ekleyen ve sonucu döndürür.

Aşağıdaki örnek döndürür `3`:

```json
"[add(1, 2)]"
```

### <a name="sub"></a>sub
İkinci sayı ilk sayısından çıkarır ve sonucu döndürür.

Aşağıdaki örnek döndürür `1`:

```json
"[sub(3, 2)]"
```

### <a name="mul"></a>mul
İki sayıyı çarpar ve sonucu döndürür.

Aşağıdaki örnek döndürür `6`:

```json
"[mul(2, 3)]"
```

### <a name="div"></a>div
İkinci sayı ilk sayıyı böler ve sonucu döndürür. Sonucu her zaman bir tamsayıdır.

Aşağıdaki örnek döndürür `2`:

```json
"[div(6, 3)]"
```

### <a name="mod"></a>mod
İkinci sayı ilk sayıyı böler ve kalanı döndürür.

Aşağıdaki örnek döndürür `0`:

```json
"[mod(6, 3)]"
```

Aşağıdaki örnek döndürür `2`:

```json
"[mod(6, 4)]"
```

### <a name="min"></a>dk
İki sayının küçük döndürür.

Aşağıdaki örnek döndürür `1`:

```json
"[min(1, 2)]"
```

### <a name="max"></a>en fazla
İki sayı daha büyük döndürür.

Aşağıdaki örnek döndürür `2`:

```json
"[max(1, 2)]"
```

### <a name="range"></a>Aralığı
Tam sayı belirtilen aralıkta bir sıra üretir.

Aşağıdaki örnek döndürür `[1,2,3]`:

```json
"[range(1, 3)]"
```

### <a name="rand"></a>rand
Belirtilen aralıkta rastgele bir tamsayı döndürür. Bu işlev, şifreleme yoluyla güvenli rasgele sayılar oluşturmaz.

Aşağıdaki örnek döndürebilir `42`:

```json
"[rand(-100, 100)]"
```

### <a name="floor"></a>Kat
Belirtilen sayıdan küçük veya eşit en büyük tamsayı döndürür.

Aşağıdaki örnek döndürür `3`:

```json
"[floor(3.14)]"
```

### <a name="ceil"></a>Ceil
Büyüktür veya eşittir belirtilen sayıdan büyük tamsayı döndürür.

Aşağıdaki örnek döndürür `4`:

```json
"[ceil(3.14)]"
```

## <a name="date-functions"></a>Tarih işlevleri
### <a name="utcnow"></a>utcNow
Bir dize, yerel bilgisayardaki geçerli tarih ve saati ISO 8601 biçiminde döndürür.

Aşağıdaki örnek döndürebilir `"1990-12-31T23:59:59.000Z"`:

```json
"[utcNow()]"
```

### <a name="addseconds"></a>saniyeEkle
Bir integral saniye sayısı için belirtilen zaman damgası ekler.

Aşağıdaki örnek döndürür `"1991-01-01T00:00:00.000Z"`:

```json
"[addSeconds('1990-12-31T23:59:60Z', 1)]"
```

### <a name="addminutes"></a>addMinutes
Bir integral dakika sayısı için belirtilen zaman damgası ekler.

Aşağıdaki örnek döndürür `"1991-01-01T00:00:59.000Z"`:

```json
"[addMinutes('1990-12-31T23:59:59Z', 1)]"
```

### <a name="addhours"></a>addHours
Bir tamsayı saat için belirtilen zaman damgası ekler.

Aşağıdaki örnek döndürür `"1991-01-01T00:59:59.000Z"`:

```json
"[addHours('1990-12-31T23:59:59Z', 1)]"
```

## <a name="next-steps"></a>Sonraki adımlar
* Azure Resource Manager'a giriş için bkz [Azure Resource Manager'a genel bakış](../azure-resource-manager/resource-group-overview.md).

