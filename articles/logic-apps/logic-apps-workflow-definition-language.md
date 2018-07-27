---
title: İş akışı tanımlama dili - Azure Logic Apps için şema başvurusu | Microsoft Docs
description: Azure Logic Apps iş akışı tanımlama dili ile özel iş akışı tanımları yazma
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.topic: reference
ms.date: 04/30/2018
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 9268ca3db6c99c4e660690e25a2331a1fa1cdf96
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39263683"
---
# <a name="schema-reference-for-workflow-definition-language-in-azure-logic-apps"></a>Azure Logic Apps iş akışı tanımı dil Şeması Başvurusu

Mantıksal uygulama iş akışı ile oluşturduğunuzda [Azure Logic Apps](../logic-apps/logic-apps-overview.md), mantıksal uygulamanız için çalıştırılan gerçek mantığı temel aldığı iş akışının tanımını açıklar. Bu açıklama kullanan iş akışı tanımı dil şeması tarafından tanımlanan ve doğrulanmış bir yapı aşağıdaki [JavaScript nesne gösterimi (JSON)](https://www.json.org/). 
  
## <a name="workflow-definition-structure"></a>İş akışı tanım yapısı

Bir iş akışı tanımı, mantıksal uygulamanızı başlatan en az bir tetikleyici yanı sıra, mantıksal uygulamanızı çalıştıran bir veya daha fazla eylem vardır. 

Bir iş akışı tanımı için üst düzey yapısı şu şekildedir:  
  
```json
"definition": {
  "$schema": "<workflow-definition-language-schema-version>",
  "contentVersion": "<workflow-definition-version-number>",
  "parameters": { "<workflow-parameter-definitions>" },
  "triggers": { "<workflow-trigger-definitions>" },
  "actions": { "<workflow-action-definitions>" },
  "outputs": { "<workflow-output-definitions>" }
}
```
  
| Öğe | Gerekli | Açıklama | 
|---------|----------|-------------| 
| tanım | Evet | Başlangıç öğesi, iş akışı tanımı | 
| $schema | Yalnızca bir iş akışı tanımı dışarıdan başvurma sırasında | Burada bulabilirsiniz iş akışı tanımı dil sürümü tanımlayan JSON şema dosyası için konumu: <p>`https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json`</p> |   
| contentVersion | Hayır | Varsayılan "1.0.0.0" olan, iş akışı tanımı sürüm numarası. Tanımlamak ve doğru tanımı bir iş akışı dağıtırken doğrulamak için kullanılacak bir değer belirtin. | 
| parametreler | Hayır | Veri, iş akışınıza aktarmak bir veya daha fazla parametre tanımları <p><p>En fazla parametreleri: 50 | 
| Tetikleyiciler | Hayır | Tanımları, iş akışı örneği bir veya daha fazla tetikleyici. Ancak yalnızca iş akışı tanımlama dili ile Logic Apps Tasarımcısı ile görsel olarak değil birden fazla tetikleyici tanımlayabilirsiniz. <p><p>En fazla tetikleyiciler: 10 | 
| Eylemler | Hayır | İş akışı çalışma zamanında yürütmek bir veya daha fazla eylem için tanımları <p><p>Maksimum eylem: 250 | 
| çıkışlar | Hayır | Döndüren bir iş akışından çıkış tanımları <p><p>En fazla çıktı: 10 |  
|||| 

## <a name="parameters"></a>Parametreler

İçinde `parameters` bölümünde, mantıksal uygulamanızı girişleri kabul etmek için dağıtım kullanan tüm iş akışı parametreleri tanımlayın. Dağıtım sırasında parametre bildirimleri hem parametre değerlerini gereklidir. Diğer iş akışı bölümlerde bu parametreler kullanabilmeniz için önce bu bölümlerdeki tüm parametreleri bildirdiğinizden emin olun. 

Bir parametre tanımında genel yapısı şu şekildedir:  

```json
"parameters": {
  "<parameter-name>": {
    "type": "<parameter-type>",
    "defaultValue": "<default-parameter-value>",
    "allowedValues": [ <array-with-permitted-parameter-values> ],
    "metadata": { 
      "key": { 
        "name": "<key-value>"
      } 
    }
  }
},
```

| Öğe | Gerekli | Tür | Açıklama |  
|---------|----------|------|-------------|  
| type | Evet | int, kayan noktalı sayı, dize, securestring, bool, dizi, JSON nesnesi, secureobject <p><p>**Not**: tüm parolalar, anahtarlar ve gizli dizileri için kullanmak `securestring` ve `secureobject` çünkü `GET` işlemi, bu tür döndürmez. | Parametresinin türü |
| defaultValue | Hayır | Aynı `type` | İş akışı örneğini oluşturduğunda hiçbir değer belirtilmemişse varsayılan parametre değeri | 
| izin verilen değerler | Hayır | Aynı `type` | Bir dizi parametre kabul edebilen değerlerle |  
| meta veriler | Hayır | JSON nesnesi | Diğer parametre ayrıntıları, örneğin, ad veya mantıksal uygulama veya Visual Studio veya diğer araçları tarafından kullanılan tasarım zamanı verileri için okunabilir bir açıklaması |  
||||

## <a name="triggers-and-actions"></a>Tetikleyiciler ve eylemler  

Bir iş akışı tanımı `triggers` ve `actions` iş akışının yürütme sırasında gerçekleşen çağrıları bölümleri tanımlar. Söz dizimi ve bu bölümleri hakkında daha fazla bilgi için bkz: [iş akışı tetikleyici ve Eylemler](../logic-apps/logic-apps-workflow-actions-triggers.md).
  
## <a name="outputs"></a>Çıkışlar 

İçinde `outputs` bölümünde, iş akışınızı bittiğinde döndürebilir verileri tanımlar çalışıyor. Örneğin, bir özel durum veya değer, her bir çalıştırmanın izlemek için iş akışı çıkışı verileri döndürür belirtin. 

> [!NOTE]
> Bir hizmetin REST API'SİNDEN gelen isteklere yanıt zaman kullanmayın `outputs`. Bunun yerine, `Response` eylem türü. Daha fazla bilgi için [iş akışı tetikleyici ve Eylemler](../logic-apps/logic-apps-workflow-actions-triggers.md).

Bir çıkış tanımı için genel yapısı şu şekildedir: 

```json
"outputs": {
  "<key-name>": {  
    "type": "<key-type>",  
    "value": "<key-value>"  
  }
} 
```

| Öğe | Gerekli | Tür | Açıklama | 
|---------|----------|------|-------------| 
| <*anahtar adı*> | Evet | Dize | Anahtar adı çıkışı için dönüş değeri |  
| type | Evet | int, kayan noktalı sayı, dize, securestring, bool, dizi, JSON nesnesi | Çıkış döndürülen değerin türü | 
| değer | Evet | Aynı `type` | Çıkış dönüş değeri |  
||||| 

Bir iş akışından çıktısını almak için mantıksal uygulama çalıştırma geçmişi ve Azure portalında ayrıntılarını gözden geçirebilir veya [iş akışı REST API](https://docs.microsoft.com/rest/api/logic/workflows). Böylece panolar oluşturabilir, çıkış harici sistemlere, örneğin, Power BI geçirebilirsiniz. 

<a name="expressions"></a>

## <a name="expressions"></a>İfadeler

JSON ile tasarım zamanında, örneğin mevcut değişmez değerlere sahip olabilir:

```json
"customerName": "Sophia Owen", 
"rainbowColors": ["red", "orange", "yellow", "green", "blue", "indigo", "violet"], 
"rainbowColorsCount": 7 
```

Ayrıca, çalışma zamanına kadar mevcut olmayan değerleri de sağlayabilirsiniz. Bu değerleri temsil etmek için kullanabileceğiniz *ifadeleri*, çalışma zamanında değerlendirilir. Bir ifade bir veya daha fazla bilgi içeren bir dizidir [işlevleri](#functions), [işleçleri](#operators), değişkenleri, açık değerler ya da sabitler. İş akışı tanımınızı bir ifade herhangi bir yerde bir JSON dizesi değerinin oturum sırasında ifade koyarak kullanabilirsiniz (\@). Temsil eden bir JSON değeri bir ifade değerlendirilirken, deyim gövdesi kaldırarak ayıklanan \@ karakteri ve her zaman başka bir JSON değeri sonuçlanır. 

Örneğin, önceden tanımlanmış `customerName` özelliğini kullanarak özellik değeri alabilirsiniz [parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) bu değer atayın ve işlev bir ifadede `accountName` özelliği:

```json
"customerName": "Sophia Owen", 
"accountName": "@parameters('customerName')"
```

*Dize ilişkilendirme* ayrıca tarafından Sarmalanan dizeleri içinde birden fazla ifade kullanmanızı sağlar \@ karakter ve küme ayraçları ({}). Söz dizimi şu şekildedir:

```json
@{ "<expression1>", "<expression2>" }
```

Sonucu her zaman bu özellik benzer hale getirme, bir dize ise `concat()` işlevi, örneğin: 

```json
"customerName": "First name: @{parameters('firstName')} Last name: @{parameters('lastName')}"
```

İle başlayan bir sabit dizesi varsa \@ karakter, önek \@ başka bir karakter \@ karakter kaçış karakteri olarak: \@\@

Bu örnekler, ifadelerin nasıl değerlendirilir gösterir:

| JSON değeri | Sonuç |
|------------|--------| 
| "Sophia Owen" | Bu karakterler döndürür: 'Sophia Owen' |
| "[1] dizi" | Bu karakterler döndürür: 'array [1]' |
| "\@\@" | Bu karakterler tek karakterli dize olarak döndürür: '\@' |   
| " \@" | Bu karakter iki karakterli dize olarak döndürür: ' \@' |
|||

Bu örnekler için "myBirthMonth" eşit "Ocak için" ve "myAge" 42 sayıya eşit tanımladığınız varsayalım:  
  
```json
"myBirthMonth": "January",
"myAge": 42
```

Bu örnekler, aşağıdaki ifadeler nasıl değerlendirilir gösterir:

| JSON ifadesi | Sonuç |
|-----------------|--------| 
| "\@parameters('myBirthMonth')" | Bu dize döndürecek: "Ocak" |  
| "\@{parameters('myBirthMonth')}" | Bu dize döndürecek: "Ocak" |  
| "\@parameters('myAge')" | Bu sayı döndürür: 42 |  
| "\@{parameters('myAge')}" | Bu bir dize olarak sayı döndürür: "42" |  
| "Benim geçerlilik süresi \@{parameters('myAge')}" | Bu dize döndürecek: "42 yaşımı is" |  
| "\@concat (' yaşımı ', string(parameters('myAge')))" | Bu dize döndürecek: "42 yaşımı is" |  
| "Benim geçerlilik süresi \@ \@{parameters('myAge')}" | İfade içerir. Bu dize döndürecek: "My geçerlilik süresi \@{parameters('myAge')}' | 
||| 

Logic Apps Tasarımcısı'nda görsel olarak çalışırken, ifadeleri ifade oluşturucusu aracılığıyla örneğin oluşturabilirsiniz: 

![Logic Apps Tasarımcısı'nda > ifade oluşturucusu](./media/logic-apps-workflow-definition-language/expression-builder.png)

İşiniz bittiğinde, ifade, iş akışı tanımı, karşılık gelen özellik için örneğin görünür, `searchQuery` burada özelliği:

```json
"Search_tweets": {
  "inputs": {
    "host": {
      "connection": {
       "name": "@parameters('$connections')['twitter']['connectionId']"
      }
    }
  },
  "method": "get",
  "path": "/searchtweets",
  "queries": {
    "maxResults": 20,
    "searchQuery": "Azure @{concat('firstName','', 'LastName')}"
  }
},
```

<a name="operators"></a>

## <a name="operators"></a>İşleçler

İçinde [ifadeleri](#expressions) ve [işlevleri](#functions), işleçler başvurusu bir özellik veya bir dizi değer gibi belirli görevleri gerçekleştirin. 

| İşleç | Görev | 
|----------|------|
| ' | Kaydırma gibi yalnızca tek tırnak işareti, dizenin bir dize sabit değeri giriş olarak veya ifade ve işlevleri kullanmak için `'<myString>'`. Çift tırnak işareti kullanmayın (""), JSON biçimlendirme geçici olarak tüm bir ifade ile çakışıyor. Örneğin: <p>**Evet**: length('Hello') </br>**Hayır**: length("Hello") <p>Diziler veya rakam geçirdiğinizde, noktalama sarmalama gerekmez. Örneğin: <p>**Evet**: uzunluğu ([1, 2, 3]) </br>**Hayır**: uzunluğu ("[1, 2, 3]") | 
| [] | Bir dizideki belirli bir konuma (dizin) değerinde başvurmak için köşeli ayraç kullanın. Örneğin, bir dizi içinde ikinci öğeyi almak için şunu yazın: <p>`myArray[1]` | 
| . | Bir nesneyi bir özelliği başvuru için nokta işlecini kullanın. Örneğin, almak için `name` özelliği için bir `customer` JSON nesnesi: <p>`"@parameters('customer').name"` | 
| ? | Bir çalışma zamanı hatası olmadan bir nesne null özelliklerinde başvurmak için soru işareti işleci kullanın. Örneğin, bir tetikleyici null çıkışları işlemek için bu ifade kullanabilirsiniz: <p>`@coalesce(trigger().outputs?.body?.<someProperty>, '<property-default-value>')` | 
||| 

<a name="functions"></a>

## <a name="functions"></a>İşlevler

Bazı ifadelerin, bir mantıksal uygulama çalışmaya başladığında henüz bulunmayabilir çalışma zamanı eylemlerden değerleri alın. Başvuru veya bu değerleri ifadelerde çalışmak için kullanabileceğiniz [ *işlevleri*](../logic-apps/workflow-definition-language-functions-reference.md). Örneğin, hesaplamalar için matematiksel işlevler gibi kullanabilirsiniz [add()](../logic-apps/workflow-definition-language-functions-reference.md#add) tamsayılar ya da float toplamını döndüren işlev. Her işlevi hakkında ayrıntılı bilgi için bkz: [alfabetik başvurusu makalesinde](../logic-apps/workflow-definition-language-functions-reference.md).
Veya, İşlevler ve genel amaçları öğrenmeye devam edin.

İşlevler ile gerçekleştirebileceğiniz sadece birkaç örnek görevler aşağıda verilmiştir: 

| Görev | İşlev sözdizimi | Sonuç | 
| ---- | --------------- | ------ | 
| Küçük harf biçiminde bir dize döndürür. | toLower ('<*metin*>') <p>Örneğin: toLower('Hello') | "hello" | 
| Bir genel benzersiz tanıtıcısı (GUID) döndürür. | Guid() |"c2ecc88d-88c8-4096-912c-d6f2e2b138ce" | 
|||| 

Bu örnek, değeri nasıl elde edebilirsiniz gösterir `customerName` parametresi ve değeri Ata `accountName` kullanarak özellik [parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) işlevi bir ifadede:

```json
"accountName": "@parameters('customerName')"
```

İşlevler ifadelerde kullanabilirsiniz diğer bazı genel yollar şunlardır:

| Görev | İfadede işlev sözdizimi | 
| ---- | -------------------------------- | 
| Bu öğe bir işleve geçirerek bir öğe ile çalışma gerçekleştirin. | "\@<*functionName*> (<*öğesi*>)" | 
| 1. Alma *parameterName*ait iç içe kullanarak değer `parameters()` işlevi. </br>2. Bu değeri geçirerek sonuç içeren çalışma gerçekleştirme *functionName*. | "\@<*functionName*> (parametreleri ('<*parameterName*>'))" | 
| 1. İç içe geçmiş içteki işlevden sonucu elde *functionName*. </br>2. Sonuç dış işlevine geçirebileceğimiz *functionName2*. | "\@<*functionName2*> (<*functionName*> (<*öğesi*>))" | 
| 1. Sonuç Alma *functionName*. </br>2. Sonuç özelliğine sahip bir nesne olması koşuluyla, *propertyName*, bu özelliğin değerini alın. | "\@<*functionName*>(<*item*>). <*propertyName*>" | 
||| 

Örneğin, `concat()` işlevi parametre olarak dize değerleri iki veya daha fazla sürebilir. Bu işlev, bu dizelerin tek dizesinde birleştirir. Birleştirilmiş bir dize, "SophiaOwen" alabilmeniz ya da dize değişmez değerleri, örneğin, "Sophia" ve "Owen" geçirebilirsiniz:

```json
"customerName": "@concat('Sophia', 'Owen')"
```

Veya, parametrelerinden dize değerleri alabilirsiniz. Bu örnekte `parameters()` her işlevde `concat()` parametresi ve `firstName` ve `lastName` parametreleri. Daha sonra çıkan dizeleri için geçirdiğiniz `concat()` birleşik bir dize, örneğin, "SophiaOwen" alabilmeniz işlev:

```json
"customerName": "@concat(parameters('firstName'), parameters('lastName'))"
```

Her iki durumda da, sonucu örneklerin her ikisi de atama `customerName` özelliği. 

Her işlevi hakkında ayrıntılı bilgi için bkz: [alfabetik başvurusu makalesinde](../logic-apps/workflow-definition-language-functions-reference.md).
Veya kendi genel amacına bağlı işlevler hakkında daha fazla bilgi.

<a name="string-functions"></a>

### <a name="string-functions"></a>Dize işlevleri

Dizeleri ile çalışmak için bu dize işlevleri ve ayrıca bazı kullanabilirsiniz [toplama işlevleri](#collection-functions). Dize işlevleri yalnızca dizeler üzerinde çalışır. 

| Dize işlevi | Görev | 
| --------------- | ---- | 
| [concat](../logic-apps/workflow-definition-language-functions-reference.md#concat) | İki veya daha fazla dizeleri birleştirmek ve birleştirilmiş dizeyi döndürür. | 
| [endsWith](../logic-apps/workflow-definition-language-functions-reference.md#endswith) | Bir dizeyi, belirtilen alt dizeyle bitip bitmediğini kontrol edin. | 
| [GUID](../logic-apps/workflow-definition-language-functions-reference.md#guid) | Bir dize olarak bir genel benzersiz tanıtıcısı (GUID) oluşturur. | 
| [indexOf](../logic-apps/workflow-definition-language-functions-reference.md#indexof) | Bir alt dizenin başlangıç konumunu döndürür. | 
| [lastIndexOf](../logic-apps/workflow-definition-language-functions-reference.md#lastindexof) | Bitiş konumu için bir alt dizeyi döndürür. | 
| [Değiştir](../logic-apps/workflow-definition-language-functions-reference.md#replace) | Belirtilen dizenin bir alt dizenin yerini ve güncelleştirilmiş dizeyi döndür. | 
| [split](../logic-apps/workflow-definition-language-functions-reference.md#split) | Tüm karakterleri ve her karakter ile belirli sınırlayıcı karakter ayıran bir dizi döndürür. | 
| [startsWith](../logic-apps/workflow-definition-language-functions-reference.md#startswith) | Bir dizeyi belirli bir alt dizesi ile başlayıp başlamadığını kontrol edin. | 
| [alt dize](../logic-apps/workflow-definition-language-functions-reference.md#substring) | Belirtilen konumundan başlayan bir dizeden karakterleri döndürür. | 
| [toLower](../logic-apps/workflow-definition-language-functions-reference.md#toLower) | Küçük harf biçiminde bir dize döndürür. | 
| [toUpper](../logic-apps/workflow-definition-language-functions-reference.md#toUpper) | Büyük harf biçiminde bir dize döndürür. | 
| [Kırpma](../logic-apps/workflow-definition-language-functions-reference.md#trim) | Bir dizeden öndeki ve sondaki boşlukları kaldırın ve güncelleştirilmiş bir dize döndürür. | 
||| 

<a name="collection-functions"></a>

### <a name="collection-functions"></a>Toplama işlevleri

Koleksiyonlar, genellikle dizi, dizeleri ve sözlükleri ile bazen çalışmak için bu toplama işlevleri kullanabilirsiniz. 

| Toplama işlevi | Görev | 
| ------------------- | ---- | 
| [içerir](../logic-apps/workflow-definition-language-functions-reference.md#contains) | Bir koleksiyon için belirli bir öğe olup olmadığını denetleyin. |
| [boş](../logic-apps/workflow-definition-language-functions-reference.md#empty) | Bir koleksiyonun boş olup olmadığını denetleyin. | 
| [ilk](../logic-apps/workflow-definition-language-functions-reference.md#first) | Bir koleksiyondaki ilk öğeyi döndürür. | 
| [kesişimi](../logic-apps/workflow-definition-language-functions-reference.md#intersection) | Sahip bir koleksiyonun dönüş *yalnızca* belirtilen koleksiyonlarla arasında ortak öğeleri. | 
| [join](../logic-apps/workflow-definition-language-functions-reference.md#join) | İçeren bir dize döndürecek *tüm* bir dizi, öğeleri belirtilen karakteriyle ayrılmış. | 
| [Son](../logic-apps/workflow-definition-language-functions-reference.md#last) | Bir koleksiyondaki son öğeyi döndürür. | 
| [Uzunluğu](../logic-apps/workflow-definition-language-functions-reference.md#length) | Bir dize ya da dizideki öğe sayısını döndürür. | 
| [Atla](../logic-apps/workflow-definition-language-functions-reference.md#skip) | Bir koleksiyonun önünden öğeleri kaldırıp dönüş *diğer tüm* öğeleri. | 
| [sınav zamanı](../logic-apps/workflow-definition-language-functions-reference.md#take) | Bir koleksiyonun önünden öğelerini döndürür. | 
| [birleşim](../logic-apps/workflow-definition-language-functions-reference.md#union) | Sahip bir koleksiyonun dönüş *tüm* belirtilen koleksiyonlarla öğelerinden. | 
||| 

<a name="comparison-functions"></a>

### <a name="comparison-functions"></a>Karşılaştırma işlevleri

Koşullar ile çalışma, değerleri ve ifade sonuçları karşılaştırmak ya da mantıksal çeşitli değerlendirmek için bu karşılaştırma işlevleri kullanabilirsiniz. Her işlev hakkındaki tam başvuru için bkz: [alfabetik başvurusu makalesinde](../logic-apps/workflow-definition-language-functions-reference.md).

| Karşılaştırma işlevi | Görev | 
| ------------------- | ---- | 
| [ve](../logic-apps/workflow-definition-language-functions-reference.md#and) | Tüm ifadelerin doğru olup olmadığını denetleyin. | 
| [eşittir](../logic-apps/workflow-definition-language-functions-reference.md#equals) | Her iki değer eşit olup olmadığını denetleyin. | 
| [daha büyük](../logic-apps/workflow-definition-language-functions-reference.md#greater) | İlk değer ikinci değerden büyük olup olmadığını denetleyin. | 
| [greaterOrEquals](../logic-apps/workflow-definition-language-functions-reference.md#greaterOrEquals) | İlk değer ikinci değerine eşit veya daha büyük olup olmadığını denetleyin. | 
| [Eğer](../logic-apps/workflow-definition-language-functions-reference.md#if) | İfadenin true veya false olup olmadığını denetleyin. Sonuca bağlı, belirli bir değeri döndürme. | 
| [daha az](../logic-apps/workflow-definition-language-functions-reference.md#less) | İkinci değer ilk değer olup değerinden denetleyin. | 
| [lessOrEquals](../logic-apps/workflow-definition-language-functions-reference.md#lessOrEquals) | İlk değer ya da ikinci değer eşit olup olmadığını denetleyin. | 
| [değil](../logic-apps/workflow-definition-language-functions-reference.md#not) | Bir ifadenin false olup olmadığını denetleyin. | 
| [veya](../logic-apps/workflow-definition-language-functions-reference.md#or) | En az bir ifadenin doğru olup olmadığını denetleyin. |
||| 

<a name="conversion-functions"></a>

### <a name="conversion-functions"></a>Dönüşüm işlevleri

Bir değer türü veya biçimi değiştirmek için bu dönüştürme işlevleri kullanabilirsiniz. Örneğin, bir Boole değeri tamsayıya değiştirebilirsiniz. Logic Apps içerik türlerine dönüştürme işlemi sırasında nasıl işlediğini öğrenmek için bkz: [içerik türlerini işleme](../logic-apps/logic-apps-content-type.md). Her işlev hakkındaki tam başvuru için bkz: [alfabetik başvurusu makalesinde](../logic-apps/workflow-definition-language-functions-reference.md).

| Dönüştürme işlevi | Görev | 
| ------------------- | ---- | 
| [Dizi](../logic-apps/workflow-definition-language-functions-reference.md#array) | Belirtilen tek bir giriş için bir dizi döndürür. Birden çok giriş için bkz. [createArray](../logic-apps/workflow-definition-language-functions-reference.md#createArray). | 
| [Base64](../logic-apps/workflow-definition-language-functions-reference.md#base64) | Base64 kodlamalı sürümü bir dize döndürür. | 
| [base64ToBinary](../logic-apps/workflow-definition-language-functions-reference.md#base64ToBinary) | İkili dosya sürümü için base64 ile kodlanmış bir dize döndürür. | 
| [base64ToString](../logic-apps/workflow-definition-language-functions-reference.md#base64ToString) | Dize sürümü base64 ile kodlanmış bir dize döndürür. | 
| [İkili](../logic-apps/workflow-definition-language-functions-reference.md#binary) | İkili dosya sürümü için giriş değeri döndürür. | 
| [bool](../logic-apps/workflow-definition-language-functions-reference.md#bool) | Boole sürümü için giriş değeri döndürür. | 
| [createArray](../logic-apps/workflow-definition-language-functions-reference.md#createArray) | Birden çok girişler bir dizi döndürür. | 
| [dataUri](../logic-apps/workflow-definition-language-functions-reference.md#dataUri) | ' % S'veri URI'si için bir giriş değeri döndürür. | 
| [dataUriToBinary](../logic-apps/workflow-definition-language-functions-reference.md#dataUriToBinary) | Bir veri URI'SİNİN ikili sürümü döndürür. | 
| [dataUriToString](../logic-apps/workflow-definition-language-functions-reference.md#dataUriToString) | Bir veri URI'SİNİN dize sürümü döndürür. | 
| [decodeBase64](../logic-apps/workflow-definition-language-functions-reference.md#decodeBase64) | Dize sürümü base64 ile kodlanmış bir dize döndürür. | 
| [decodeDataUri](../logic-apps/workflow-definition-language-functions-reference.md#decodeDataUri) | Bir veri URI'SİNİN ikili sürümü döndürür. | 
| [Decodeurıcomponent](../logic-apps/workflow-definition-language-functions-reference.md#decodeUriComponent) | Değiştirir kodu çözülmüş sürümleriyle karakter kaçış bir dize döndürür. | 
| [Encodeurıcomponent](../logic-apps/workflow-definition-language-functions-reference.md#encodeUriComponent) | URL güvenli olmayan karakterleri kaçış karakterleri yerini alan bir dize döndürür. | 
| [kayan nokta](../logic-apps/workflow-definition-language-functions-reference.md#float) | Döndürür bir kayan nokta sayısı için bir giriş değeri. | 
| [int](../logic-apps/workflow-definition-language-functions-reference.md#int) | Tamsayı sürümü bir dize döndürür. | 
| [JSON](../logic-apps/workflow-definition-language-functions-reference.md#json) | JavaScript nesne gösterimi (JSON) türü değeri veya dize veya XML nesnesi döndürür. | 
| [dize](../logic-apps/workflow-definition-language-functions-reference.md#string) | Dize sürümü için giriş değeri döndürür. | 
| [uriComponent](../logic-apps/workflow-definition-language-functions-reference.md#uriComponent) | URL güvenli olmayan karakterleri kaçış karakterleri ile değiştirerek bir giriş değeri için URI ile kodlanmış sürümünü döndürür. | 
| [uriComponentToBinary](../logic-apps/workflow-definition-language-functions-reference.md#uriComponentToBinary) | İkili dosya sürümü için URI ile kodlanmış bir dize döndürür. | 
| [uriComponentToString](../logic-apps/workflow-definition-language-functions-reference.md#uriComponentToString) | Dize sürümü için URI ile kodlanmış bir dize döndürür. | 
| [xml](../logic-apps/workflow-definition-language-functions-reference.md#xml) | XML sürümü bir dize döndürür. | 
||| 

<a name="math-functions"></a>

### <a name="math-functions"></a>Matematiksel işlevler

Tamsayı float'lar ile çalışmak için bu matematik işlevleri kullanabilirsiniz. Her işlev hakkındaki tam başvuru için bkz: [alfabetik başvurusu makalesinde](../logic-apps/workflow-definition-language-functions-reference.md).

| Matematik işlevi | Görev | 
| ------------- | ---- | 
| [Ekleme](../logic-apps/workflow-definition-language-functions-reference.md#add) | İki sayıyı eklemesini sonucu döndürür. | 
| [div](../logic-apps/workflow-definition-language-functions-reference.md#div) | Sonucu, iki sayı arasındaki bölme işleminin sonucunu döndürür. | 
| [en fazla](../logic-apps/workflow-definition-language-functions-reference.md#max) | En yüksek değer bir dizi numaraları veya bir dizi döndürür. | 
| [Min](../logic-apps/workflow-definition-language-functions-reference.md#min) | En düşük değer bir dizi numaraları veya bir dizi döndürür. | 
| [mod](../logic-apps/workflow-definition-language-functions-reference.md#mod) | Kalan iki sayı arasındaki bölme işleminin sonucunu döndürür. | 
| [mul](../logic-apps/workflow-definition-language-functions-reference.md#mul) | Ürün iki sayı arasındaki çarpma işleminin sonucunu döndürür. | 
| [rand](../logic-apps/workflow-definition-language-functions-reference.md#rand) | Belirtilen bir aralıktaki rastgele bir tamsayı döndürür. | 
| [Aralığı](../logic-apps/workflow-definition-language-functions-reference.md#range) | Belirtilen bir tamsayı başlayan bir tamsayı dizisi döndürür. | 
| [alt](../logic-apps/workflow-definition-language-functions-reference.md#sub) | İlk numarasından ikinci sayı arasındaki çıkarma işleminin sonucu döndürür. | 
||| 

<a name="date-time-functions"></a>

### <a name="date-and-time-functions"></a>Tarih ve saat işlevleri

Tarihler ve saatler ile çalışmak için bu tarih ve saat işlevleri kullanabilirsiniz.
Her işlev hakkındaki tam başvuru için bkz: [alfabetik başvurusu makalesinde](../logic-apps/workflow-definition-language-functions-reference.md).

| Tarih veya saat işlevi | Görev | 
| --------------------- | ---- | 
| [addDays](../logic-apps/workflow-definition-language-functions-reference.md#addDays) | Bir gün sayısı için bir zaman damgası ekleyin. | 
| [addHours](../logic-apps/workflow-definition-language-functions-reference.md#addHours) | Saat sayısı için bir zaman damgası ekleyin. | 
| [addMinutes](../logic-apps/workflow-definition-language-functions-reference.md#addMinutes) | Dakika sayısı için bir zaman damgası ekleyin. | 
| [saniyeEkle](../logic-apps/workflow-definition-language-functions-reference.md#addSeconds) | Saniye sayısı için bir zaman damgası ekleyin. |  
| [addToTime](../logic-apps/workflow-definition-language-functions-reference.md#addToTime) | Zaman birimlerinin sayısı için bir zaman damgası ekleyin. Ayrıca bkz: [getFutureTime](../logic-apps/workflow-definition-language-functions-reference.md#getFutureTime). | 
| [convertFromUtc](../logic-apps/workflow-definition-language-functions-reference.md#convertFromUtc) | Bir zaman damgası (UTC) saat Eşgüdümlü Evrensel hedef saat dilimine dönüştürür. | 
| [convertTimeZone](../logic-apps/workflow-definition-language-functions-reference.md#convertTimeZone) | Bir zaman damgasını kaynak saat diliminden hedef saat dilimine dönüştürür. | 
| [convertToUtc](../logic-apps/workflow-definition-language-functions-reference.md#convertToUtc) | Bir zaman damgasını kaynak saat diliminden saati Eşgüdümlü Evrensel (UTC) dönüştürün. | 
| [dayOfMonth](../logic-apps/workflow-definition-language-functions-reference.md#dayOfMonth) | İtibaren zaman damgası ayın günü bileşenini döndürür. | 
| [dayOfWeek](../logic-apps/workflow-definition-language-functions-reference.md#dayOfWeek) | İtibaren zaman damgası haftanın günü bileşenini döndürür. | 
| [dayOfYear](../logic-apps/workflow-definition-language-functions-reference.md#dayOfYear) | İtibaren zaman damgası yılın günü bileşenini döndürür. | 
| [formatDateTime](../logic-apps/workflow-definition-language-functions-reference.md#formatDateTime) | Tarihten itibaren zaman damgası döndürür. | 
| [getFutureTime](../logic-apps/workflow-definition-language-functions-reference.md#getFutureTime) | Yanı sıra belirtilen zaman birimi geçerli zaman damgasını döndürür. Ayrıca bkz: [addToTime](../logic-apps/workflow-definition-language-functions-reference.md#addToTime). | 
| [getPastTime](../logic-apps/workflow-definition-language-functions-reference.md#getPastTime) | Belirtilen zaman birimi eksi geçerli zaman damgasını döndürür. Ayrıca bkz: [subtractFromTime](../logic-apps/workflow-definition-language-functions-reference.md#subtractFromTime). | 
| [startOfDay](../logic-apps/workflow-definition-language-functions-reference.md#startOfDay) | Bir zaman damgası için günün başlangıcını döndürür. | 
| [startOfHour](../logic-apps/workflow-definition-language-functions-reference.md#startOfHour) | Bir zaman damgası için saatin başlangıcını döndürür. | 
| [startOfMonth](../logic-apps/workflow-definition-language-functions-reference.md#startOfMonth) | Bir zaman damgası için ayın başlangıcını döndürür. | 
| [subtractFromTime](../logic-apps/workflow-definition-language-functions-reference.md#subtractFromTime) | Bir zaman damgası saat birimleri sayısı çıkarın. Ayrıca bkz: [getPastTime](../logic-apps/workflow-definition-language-functions-reference.md#getPastTime). | 
| [saat döngüsü](../logic-apps/workflow-definition-language-functions-reference.md#ticks) | Dönüş `ticks` belirtilen bir zaman damgası için özellik değeri. | 
| [utcNow](../logic-apps/workflow-definition-language-functions-reference.md#utcNow) | Geçerli zaman damgasını dize olarak döndürür. | 
||| 

<a name="workflow-functions"></a>

### <a name="workflow-functions"></a>İş akışı işlevleri

Bu iş akışı işlevleri size yardımcı olabilir:

* Çalışma zamanında bir iş akışı örneği hakkındaki ayrıntıları alın. 
* Mantıksal uygulamalar oluşturmak için kullanılan girişleri çalışın.
* Tetikleyiciler ve Eylemler çıkışlarına başvuruyor.

Örneğin, bir eylemden çıkışlarına başvuruyor ve bir sonraki eylem bu verileri kullanabilirsiniz. Her işlev hakkındaki tam başvuru için bkz: [alfabetik başvurusu makalesinde](../logic-apps/workflow-definition-language-functions-reference.md).

| İş akışı işlevi | Görev | 
| ----------------- | ---- | 
| [Eylem](../logic-apps/workflow-definition-language-functions-reference.md#action) | Çalışma zamanı veya değerleri geçerli eylemin çıkışını diğer JSON ad ve değer çiftlerinden geri döndürür. Ayrıca bkz: [eylemleri](../logic-apps/workflow-definition-language-functions-reference.md#actions). | 
| [actionBody](../logic-apps/workflow-definition-language-functions-reference.md#actionBody) | Bir eylemin dönüş `body` çalışma zamanında çıktı. Ayrıca bkz: [gövdesi](../logic-apps/workflow-definition-language-functions-reference.md#body). | 
| [actionOutputs](../logic-apps/workflow-definition-language-functions-reference.md#actionOutputs) | Çalışma zamanında bir eylemin çıkışını geri döndürür. Bkz: [eylemleri](../logic-apps/workflow-definition-language-functions-reference.md#actions). | 
| [Eylemler](../logic-apps/workflow-definition-language-functions-reference.md#actions) | Bir eylemin çıkışını çalışma zamanı veya değerleri diğer JSON ad ve değer çiftlerinden geri döndürür. Ayrıca bkz: [eylem](../logic-apps/workflow-definition-language-functions-reference.md#action).  | 
| [Gövde](#body) | Bir eylemin dönüş `body` çalışma zamanında çıktı. Ayrıca bkz: [actionBody](../logic-apps/workflow-definition-language-functions-reference.md#actionBody). | 
| [formDataMultiValues](../logic-apps/workflow-definition-language-functions-reference.md#formDataMultiValues) | Bir anahtar adı ile eşleşen değerleriyle bir dizi oluşturmak *form verilerini* veya *form kodlu* eylem çıktı. | 
| [formDataValue](../logic-apps/workflow-definition-language-functions-reference.md#formDataValue) | Bir eylem içinde bir anahtar adı ile eşleşen tek bir değer döndürmesi *form verisinin* veya *form kodlu çıkış*. | 
| [Öğesi](../logic-apps/workflow-definition-language-functions-reference.md#item) | Bir dizi üzerindeki bir yinelenen eylemi olduğu zaman içinde eylemin geçerli yineleme sırasında dizideki geçerli öğeyi döndürür. | 
| [Öğeleri](../logic-apps/workflow-definition-language-functions-reference.md#items) | İçinde için-her veya-kadar-döngü, geri döndürme işlevi, geçerli öğenin belirtilen döngünün.| 
| [listCallbackUrl](../logic-apps/workflow-definition-language-functions-reference.md#listCallbackUrl) | "Bir tetikleyici veya eylemi çağırır geri çağırma URL'si" döndürür. | 
| [multipartBody](../logic-apps/workflow-definition-language-functions-reference.md#multipartBody) | Birden çok bölümü olan bir eylemin çıkış belirli bir parçanın gövdesini döndürür. | 
| [parametreler](../logic-apps/workflow-definition-language-functions-reference.md#parameters) | Mantıksal uygulama tanımınızı açıklanan bir parametre için bir değer döndürür. | 
| [Tetikleyici](../logic-apps/workflow-definition-language-functions-reference.md#trigger) | Çalışma zamanı veya diğer JSON ad ve değer çiftleri bir tetikleyicinin çıkışını geri döndürür. Ayrıca bkz: [triggerOutputs](#triggerOutputs) ve [triggerBody](../logic-apps/workflow-definition-language-functions-reference.md#triggerBody). | 
| [triggerBody](../logic-apps/workflow-definition-language-functions-reference.md#triggerBody) | Bir tetikleyicinin dönüş `body` çalışma zamanında çıktı. Bkz: [tetikleyici](../logic-apps/workflow-definition-language-functions-reference.md#trigger). | 
| [triggerFormDataValue](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataValue) | İçinde bir anahtar adı ile eşleşen tek bir değer döndürmesi *form verisinin* veya *form kodlu* çıkışları tetikleyin. | 
| [triggerMultipartBody](../logic-apps/workflow-definition-language-functions-reference.md#triggerMultipartBody) | Bir tetikleyicinin çok parçalı çıkışında belirli bir parçanın gövdesini döndürür. | 
| [triggerFormDataMultiValues](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataMultiValues) | Değerleri bir anahtar adı eşleşen bir dizi oluşturmak *form-data* veya *form kodlu* çıkışları tetikleyin. | 
| [triggerOutputs](../logic-apps/workflow-definition-language-functions-reference.md#triggerOutputs) | Çalışma zamanı veya değerleri bir tetikleyicinin çıkışını diğer JSON ad ve değer çiftlerinden geri döndürür. Bkz: [tetikleyici](../logic-apps/workflow-definition-language-functions-reference.md#trigger). | 
| [Değişkenleri](../logic-apps/workflow-definition-language-functions-reference.md#variables) | Belirtilen bir değişkenin değerini döndürür. | 
| [İş akışı](../logic-apps/workflow-definition-language-functions-reference.md#workflow) | İş akışı hakkında tüm ayrıntıları, çalışma zamanı sırasında döndürür. | 
||| 

<a name="uri-parsing-functions"></a>

### <a name="uri-parsing-functions"></a>URI ayrıştırma işlevleri

Tekdüzen Kaynak Tanımlayıcıları (URI'lar) çalışır ve bu bir URI'leri için çeşitli özellik değerlerini almak için bu URI ayrıştırma işlevleri kullanabilirsiniz. Her işlev hakkındaki tam başvuru için bkz: [alfabetik başvurusu makalesinde](../logic-apps/workflow-definition-language-functions-reference.md).

| URI ayrıştırma işlevi | Görev | 
| -------------------- | ---- | 
| [uriHost](../logic-apps/workflow-definition-language-functions-reference.md#uriHost) | Dönüş `host` Tekdüzen Kaynak Tanımlayıcısı (URI) değeri. | 
| [uriPath](../logic-apps/workflow-definition-language-functions-reference.md#uriPath) | Dönüş `path` Tekdüzen Kaynak Tanımlayıcısı (URI) değeri. | 
| [uriPathAndQuery](../logic-apps/workflow-definition-language-functions-reference.md#uriPathAndQuery) | Dönüş `path` ve `query` Tekdüzen Kaynak Tanımlayıcısı (URI) değerleri. | 
| [uriPort](../logic-apps/workflow-definition-language-functions-reference.md#uriPort) | Dönüş `port` Tekdüzen Kaynak Tanımlayıcısı (URI) değeri. | 
| [uriQuery](../logic-apps/workflow-definition-language-functions-reference.md#uriQuery) | Dönüş `query` Tekdüzen Kaynak Tanımlayıcısı (URI) değeri. | 
| [uriScheme](../logic-apps/workflow-definition-language-functions-reference.md#uriScheme) | Dönüş `scheme` Tekdüzen Kaynak Tanımlayıcısı (URI) değeri. | 
||| 

<a name="manipulation-functions"></a>

### <a name="json-and-xml-functions"></a>JSON ve XML işlevleri

JSON nesneleri ve XML düğümü ile çalışmak için bu işleme işlevleri kullanabilirsiniz. Her işlev hakkındaki tam başvuru için bkz: [alfabetik başvurusu makalesinde](../logic-apps/workflow-definition-language-functions-reference.md).

| İşleme işlevi | Görev | 
| --------------------- | ---- | 
| [addProperty](../logic-apps/workflow-definition-language-functions-reference.md#addProperty) | Bir özellik, değer veya ad-değer çifti için bir JSON nesnesi ekleyin ve güncelleştirilmiş nesneyi döndürür. | 
| [birleşim](../logic-apps/workflow-definition-language-functions-reference.md#coalesce) | Bir veya daha fazla parametrelerinden ilk null olmayan değer döndürür. | 
| [removeProperty](../logic-apps/workflow-definition-language-functions-reference.md#removeProperty) | Bir JSON nesnesinden bir özelliğini kaldırın ve güncelleştirilmiş nesneyi döndürür. | 
| [setProperty](../logic-apps/workflow-definition-language-functions-reference.md#setProperty) | Bir JSON nesnesi özelliği değerini ayarlayın ve güncelleştirilmiş nesneyi döndürür. | 
| [XPath](../logic-apps/workflow-definition-language-functions-reference.md#xpath) | Düğümleri veya bir (XML Path Language) XPath ifadesi eşleşen değerler için XML denetleyin ve eşleşen düğümleri veya değerleri döndürür. | 
||| 

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [iş akışı tanımlama dili eylemleri ve Tetikleyicileri](../logic-apps/logic-apps-workflow-actions-triggers.md)
* Program aracılığıyla oluşturma ve logic apps ile yönetme hakkında bilgi edinin [iş akışı REST API](https://docs.microsoft.com/rest/api/logic/workflows)
