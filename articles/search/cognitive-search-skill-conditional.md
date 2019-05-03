---
title: Koşullu bilişsel arama beceri (Azure Search) | Microsoft Docs
description: Filtreleme, varsayılan oluşturma ve değerleri birleştirme veren koşullu yeteneği.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2019
ms.author: luisca
ms.openlocfilehash: 6a203a38437ccb6a9c325e6594289744e0148c84
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028433"
---
#   <a name="conditional-skill"></a>Koşullu beceri

**Koşullu beceri** bir çıktı atanmalıdır veri karar vermek için bir Boole işlem gerektiren senaryolar çeşitli sağlar. Bu senaryolar şunlardır: bir koşula dayalı filtreleme, varsayılan bir değer atamak ve veri birleştirme.

Aşağıdaki sözde kod koşullu Yeteneğin ne gerçekleştirir açıklanmaktadır:

```
if (condition) 
    { output = whenTrue } 
else 
    { output = whenFalse } 
```

> [!NOTE]
> Bu yetenek bir Bilişsel hizmetler API'sine bağlı değil ve kullanmak için ücretlendirilmez. Hala [Bilişsel hizmetler kaynağı ekleme](cognitive-search-attach-cognitive-services.md), ancak geçersiz kılmak için **ücretsiz** , az sayıda gün başına günlük zenginleştirmelerinin sınırlar resource seçeneği.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Util.ConditionalSkill


## <a name="evaluated-fields"></a>Hesaplanan alanlar

Girişleri hesaplanan alanlar olduğundan bu özel bir yetenektir.

Bir ifadenin geçerli değerler şunlardır:

-   Ek açıklama yolları (ifadeler yollarında ayrılmış, tarafından "$(" and ")") <br/>
    Örnekler:
    ```
        "= $(/document)"
        "= $(/document/content)"
    ```

-  Değişmez değerler (dizeler, sayılar, true, false, null) <br/>
    Örnekler:
    ```
       "= 'this is a string'"   // string, note the single quotes
       "= 34"                   // number
       "= true"                 // boolean
       "= null"                 // null value
    ```

-  İfadeleri karşılaştırma işlecini kullanın (==,! =, > =, >, < =, <) <br/>
    Örnekler:
    ```
        "= $(/document/language) == 'en'"
        "= $(/document/sentiment) >= 0.5"
    ```

-   Boole işleçleri kullanan ifade (& &, ||,!, ^) <br/>
    Örnekler:
    ```
        "= $(/document/language) == 'en' && $(/document/sentiment) > 0.5"
        "= !true"
    ```

-   Sayısal bir operatör kullanan ifade (+, -, \*, /, %) <br/>
    Örnekler: 
    ```
        "= $(/document/sentiment) + 0.5"         // addition
        "= $(/document/totalValue) * 1.10"       // multiplication
        "= $(/document/lengthInMeters) / 0.3049" // division
    ```

Desteklenen değerlendirme nedeniyle koşullu beceri küçük transformation senaryoları için kullanılabilir. Örnek görmek [beceri tanımı 4](#transformation-examples) örneği.

## <a name="skill-inputs"></a>Beceri girişleri
Girdi büyük/küçük harfe duyarlıdır.

| Girişler      | Açıklama |
|-------------|-------------|
| koşul   | Bu giriş bir [hesaplanan alan](#evaluated-fields) değerlendirmek için koşulu temsil eder. Bu koşul için bir Boole değeri (true veya false) değerlendirmelidir.   <br/>  Örnekler: <br/> "= true" <br/> "$(/document/language) ="fr"==" <br/> "= $(/belge/sayfaları/\*/language) $ == (/ belge/expectedLanguage)" <br/> |
| whenTrue    | Bu giriş bir [hesaplanan alan](#evaluated-fields). Koşul değerlendirilir ise döndürülecek değer true. Dize sabitleri olarak döndürülmesi ' ' tırnak işaretleri. <br/>Örnek değerler: <br/> "'sözleşme' ="<br/>"= $(/ belge/contractType)" <br/> "= $(/belge/varlıklar/\*)" <br/> |
| whenFalse   | Bu giriş bir [hesaplanan alan](#evaluated-fields). Koşul yanlış olarak değerlendirilir, döndürülecek değer.  <br/>Örnek değerler: <br/> "'sözleşme' ="<br/>"= $(/ belge/contractType)" <br/> "= $(/belge/varlıklar/\*)" <br/>

## <a name="skill-outputs"></a>Beceri çıkışları
'Output' olarak adlandırılan tek bir çıkış yok. Koşul true ise koşul false ise whenFalse veya whenTrue değerini döndürür.

## <a name="examples"></a>Örnekler

### <a name="sample-skill-definition-1-filtering-documents-to-return-only-french-documents"></a>Örnek beceri tanımı 1: Yalnızca "Fransızca" belgeler döndürülecek belgeleri filtreleme

Aşağıdaki çıktı bir dizi cümlelerden ("/ Belge/frenchSentences") belgenin dili Fransızca döndürür. Fransızca Dil değilse, bu değeri ayarlamak için null.

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document",
    "inputs": [
        { "name": "condition", "source": "= $(/document/language) == 'fr'" },
        { "name": "whenTrue", "source": "/document/sentences" },
        { "name": "whenFalse", "source": "= null" }
    ],
    "outputs": [ { "name": "output", "targetName": "frenchSentences" } ]
}
```
"/ Belge/frenchSentences" olarak kullanılıp kullanılmadığını *bağlam* "/ Belge/frenchSentences" ayarlanmamışsa, başka bir beceri yetenek yalnızca çalışır null


### <a name="sample-skill-definition-2-setting-a-default-value-when-it-does-not-exist"></a>Örnek beceri tanımı 2: Mevcut olduğunda, varsayılan değer ayarlama.

Aşağıdaki çıktı dil ayarlanmamışsa, ya da belge dili veya "es" olarak ayarlanırsa bir ek açıklama ("/ Belge/languageWithDefault") oluşturur.

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document",
    "inputs": [
        { "name": "condition", "source": "= $(/document/language) == null" },
        { "name": "whenTrue", "source": "= 'es'" },
        { "name": "whenFalse", "source": "= $(/document/language)" }
    ],
    "outputs": [ { "name": "output", "targetName": "languageWithDefault" } ]
}
```

### <a name="sample-skill-definition-3-merging-values-from-two-different-fields-into-a-single-field"></a>Örnek beceri tanımı 3: Tek bir alanı iki farklı alanların değerlerini birleştirme

Bu örnekte, bazı cümleler sahip bir *frenchSentiment* özelliği. Her *frenchSentiment* kullanmak istiyoruz, özellik NULL *englishSentiment* değeri. Biz çıkış yalnızca adında bir üyesine atama *yaklaşım* ("/ Belge/yaklaşım / * / yaklaşım").

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document/sentences/*",
    "inputs": [
        { "name": "condition", "source": "= $(/document/sentences/*/frenchSentiment) == null" },
        { "name": "whenTrue", "source": "/document/sentences/*/englishSentiment" },
        { "name": "whenFalse", "source": "/document/sentences/*/frenchSentiment" }
    ],
    "outputs": [ { "name": "output", "targetName": "sentiment" } ]
}
```

## <a name="transformation-examples"></a>Dönüşüm örnekleri
### <a name="sample-skill-definition-4-performing-data-transformations-on-a-single-field"></a>Örnek beceri tanımı 4: Tek bir alanda veri dönüşümleri gerçekleştirme

Bu örnekte, 0 ile 1 arasında bir yaklaşım aldığımız ve -1 ile 1 arasında olması, dönüştürme istiyoruz. Koşullu yeteneği kullanarak yapabileceğimiz, küçük matematik dönüşümdür.

Koşul her zaman true olduğunda belirli Bu örnekte, hiçbir zaman yetenek koşullu yönüyle kullanırız. 

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document/sentences/*",
    "inputs": [
        { "name": "condition", "source": "= true" },
        { "name": "whenTrue", "source": "= $(/document/sentences/*/sentiment) * 2 - 1" },
        { "name": "whenFalse", "source": "= 0" }
    ],
    "outputs": [ { "name": "output", "targetName": "normalizedSentiment" } ]
}
```


## <a name="special-considerations"></a>Özel durumlar
Belgelenen düzenini izleyerek özellikle dikkatli olmanız gerekir. Bu nedenle bazı parametreler değerlendirildiğini, unutmayın. İfadeler, bir eşittir işareti ="ile" başlamalı ve yolları ayrılmış, tarafından "$(" and ")". Dizeler ve gerçek yolları ve işleçleri arasında ayrım değerlendirici yardımcı olacağından 'tek tırnak içine' dizelerinizi yerleştirdiğinizden emin olun. Ayrıca, işleçler etrafındaki bir boşluk koyun emin olun (örneğin bir * bir yolda çarpma işleci değerinden farklı bir anlama sahiptir).


## <a name="next-steps"></a>Sonraki adımlar

+ [Önceden tanımlanmış beceriler](cognitive-search-predefined-skills.md)
+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)
