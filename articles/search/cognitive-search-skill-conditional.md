---
title: Koşullu bilişsel arama beceri (Azure Search) | Microsoft Docs
description: Filtreleme, varsayılan oluşturma ve değerleri birleştirme koşullu yeteneği sağlar.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2019
ms.author: luisca
ms.openlocfilehash: 149b701d4a1700787656448e2bdd0d92d2a93844
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65791510"
---
#   <a name="conditional-skill"></a>Koşullu beceri

*Koşullu beceri* atamak için bir çıktı verileri belirlemek için bir Boole işlem gerektiren Azure Search senaryoların gerçekleşmesine olanak tanır. Bu senaryolar, filtreleme, varsayılan bir değer atamak ve bir koşulu temel alarak veri birleştirme içerir.

Aşağıdaki sözde kod koşullu Yeteneğin ne gerçekleştirir gösterir:

```
if (condition) 
    { output = whenTrue } 
else 
    { output = whenFalse } 
```

> [!NOTE]
> Bu yetenek, bir Azure Bilişsel hizmetler API'si bağlı olmayan ve kullanmak için ücretlendirilmezsiniz. Ancak hala [Bilişsel hizmetler kaynağı ekleme](cognitive-search-attach-cognitive-services.md) zenginleştirmelerinin günde az sayıda için sınırlar "Ücretsiz" resource seçeneği geçersiz kılınacak.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Util.ConditionalSkill


## <a name="evaluated-fields"></a>Hesaplanan alanlar

Girişleri hesaplanan alanlar olduğundan bu özel bir yetenektir.

Aşağıdaki öğeler, bir ifadenin geçerli değerler şunlardır:

-   Ek açıklama yolları (ifadeler yollarında ayrılmış, tarafından "$(" and ")")
 <br/>
    Örnekler:
    ```
        "= $(/document)"
        "= $(/document/content)"
    ```

-  Değişmez değerler (dizeler, sayılar, true, false, null) <br/>
    Örnekler:
    ```
       "= 'this is a string'"   // string (note the single quotation marks)
       "= 34"                   // number
       "= true"                 // Boolean
       "= null"                 // null value
    ```

-  Karşılaştırma işleçleri kullanan ifade (==,! =, > =, >, < =, <) <br/>
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

-   Sayısal işleçleri kullanan ifade (+, -, \*, /, %) <br/>
    Örnekler: 
    ```
        "= $(/document/sentiment) + 0.5"         // addition
        "= $(/document/totalValue) * 1.10"       // multiplication
        "= $(/document/lengthInMeters) / 0.3049" // division
    ```

Koşullu beceri değerlendirme desteklediğinden, küçük transformation senaryolarda kullanabilirsiniz. Örneğin, [beceri tanımı 4](#transformation-example).

## <a name="skill-inputs"></a>Beceri girişleri
Girdi büyük/küçük harfe duyarlıdır.

| Giriş   | Açıklama |
|-------------|-------------|
| condition   | Bu giriş bir [hesaplanan alan](#evaluated-fields) değerlendirmek için koşulu temsil eder. Bu durum, bir Boolean değerine değerlendirmelidir (*true* veya *false*).   <br/>  Örnekler: <br/> "= true" <br/> "$(/document/language) ="fr"==" <br/> "= $(/belge/sayfaları/\*/language) $ == (/ belge/expectedLanguage)" <br/> |
| whenTrue    | Bu giriş bir [hesaplanan alan](#evaluated-fields) koşulu için değerlendirilir ise döndürülecek değeri temsil eden *true*. Dize sabitleri tek tırnak işaretleri döndürülen ('ve'). <br/>Örnek değerler: <br/> "'sözleşme' ="<br/>"= $(/ belge/contractType)" <br/> "= $(/belge/varlıklar/\*)" <br/> |
| whenFalse   | Bu giriş bir [hesaplanan alan](#evaluated-fields) koşulu için değerlendirilir ise döndürülecek değeri temsil eden *false*. <br/>Örnek değerler: <br/> "'sözleşme' ="<br/>"= $(/ belge/contractType)" <br/> "= $(/belge/varlıklar/\*)" <br/>

## <a name="skill-outputs"></a>Beceri çıkışları
Yalnızca "çıkış" olarak adlandırılan tek bir çıkış Değeri döndürür *whenFalse* koşul false ise veya *whenTrue* koşul true ise.

## <a name="examples"></a>Örnekler

### <a name="sample-skill-definition-1-filter-documents-to-return-only-french-documents"></a>Örnek beceri tanımı 1: Yalnızca Fransızca belgeler döndürülecek belgeleri filtreleme

Belgenin dilinin Fransızca ise aşağıdaki çıktıyı cümlelerden ("/ Belge/frenchSentences") bir dizi döndürür. Fransızca Dil değilse, değeri şuna ayarlı *null*.

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
"/ Belge/frenchSentences" olarak kullanılıp kullanılmadığını *bağlam* "/ Belge/frenchSentences" olarak ayarlarsanız değil, başka bir beceri, yetenek yalnızca çalışır *null*.


### <a name="sample-skill-definition-2-set-a-default-value-for-a-value-that-doesnt-exist"></a>Örnek beceri tanımı 2: Mevcut olmayan bir değer için varsayılan değer ayarlama

Aşağıdaki çıktı dil ayarlanmadıysa, belge dili veya "es" olarak bir açıklama ("/ Belge/languageWithDefault") oluşturur.

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

### <a name="sample-skill-definition-3-merge-values-from-two-fields-into-one"></a>Örnek beceri tanımı 3: İki alan değerlerini birleştirmek

Bu örnekte, bazı cümleler sahip bir *frenchSentiment* özelliği. Her *frenchSentiment* özelliği null, kullanmak istediğiniz *englishSentiment* değeri. Biz çıkış çağrılan bir üyesine atama *yaklaşım* ("/ Belge/yaklaşım / * / yaklaşım").

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

## <a name="transformation-example"></a>Örnek dönüştürme
### <a name="sample-skill-definition-4-data-transformation-on-a-single-field"></a>Örnek beceri tanımı 4: Tek bir alana veri dönüştürme

Bu örnekte, aldığımız bir *yaklaşım* 0 ile 1 arasında olmasıdır. Bunu -1 ile 1 arasında olacak şekilde dönüştürmek istiyoruz. Bu küçük transformation yapmak için koşullu beceri kullanabiliriz.

Bu örnekte biz koşul her zaman olduğundan yetenek koşullu yönüyle kullanmayın *true*.

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
Bazı parametreler değerlendirilir, bu belgelenmiş desende özellikle dikkat etmek gerekir. İfadeler, bir eşittir işareti ile başlamalıdır. Bir yol ile sınırlanması gerekir "$(" and ")". Dizeleri içinde tek tırnak işaretleri koyma emin olun. Bu, dizeler ve gerçek yolları ve işleçleri arasında ayrım değerlendirici yardımcı olur. Ayrıca işleçler etrafındaki boşlukları yerleştirdiğinizden emin olun (örneğin, bir "*" bir yolda multiply ' farklı bir şey anlamına gelir).


## <a name="next-steps"></a>Sonraki adımlar

+ [Önceden tanımlanmış beceriler](cognitive-search-predefined-skills.md)
+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)
