---
title: İş akışı tanımlama dili şema - Azure Logic Apps | Microsoft Docs
description: İş akışı tanımlama dili ile Azure mantıksal uygulamaları için özel iş akışı tanımları yazma
services: logic-apps
author: ecfan
manager: SyntaxC4
editor: ''
documentationcenter: ''
ms.assetid: 26c94308-aa0d-4730-97b6-de848bffff91
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: reference
ms.date: 04/25/2018
ms.author: estfan
ms.openlocfilehash: 7c253fd83bcc1f1dde93ac6ef0c26da1fa1a9a4b
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="logic-apps-workflow-definitions-with-the-workflow-definition-language-schema"></a>İş akışı tanımlama dili şema Logic Apps iş akışı tanımları

Bir mantıksal uygulama iş akışı oluşturduğunuzda [Azure Logic Apps](../logic-apps/logic-apps-overview.md), mantıksal uygulamanızı çalıştıran gerçek mantığı, iş akışınızın temel tanımı açıklar. Bu açıklama kullanır iş akışı tanımlama dili şema tarafından tanımlanan doğrulandı ve olan bir yapı izleyen [JavaScript nesne gösterimi (JSON)](https://www.json.org/) biçimi. 
  
## <a name="workflow-definition-structure"></a>İş akışı tanımı yapısı

Bir iş akışı tanımı mantıksal uygulamanızı başlatır en az bir tetikleyici yanı sıra, mantıksal uygulamanızı çalıştıran bir veya daha fazla eylemler vardır. 

Bir iş akışı tanımı için üst düzey yapısı şöyledir:  
  
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
| tanım | Evet | İş akışı tanımı için başlangıç öğesi | 
| $schema | Yalnızca harici bir iş akışı tanımı başvururken | Burada bulabilirsiniz iş akışı tanımı dil sürümünü tanımlayan JSON şema dosyası konumu: <p>`https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json`</p> |   
| contentVersion | Hayır | Varsayılan olarak "1.0.0.0", iş akışı tanımı sürüm numarası. Tanımlamak ve bir iş akışı dağıtırken doğru tanımı onaylamak için kullanılacak bir değer belirtin. | 
| parametreler | Hayır | İş akışınıza veri iletmek bir veya daha fazla parametre tanımları <p><p>En fazla parametreleri: 50 | 
| Tetikleyicileri | Hayır | İş akışınızı örneği bir veya daha fazla Tetikleyicileri tanımlarında. Birden çok tetikleyici, ancak yalnızca iş akışı tanımlama dili ile Logic Apps Tasarımcısı aracılığıyla değil görsel olarak tanımlayabilirsiniz. <p><p>En fazla Tetikleyicileri: 10 | 
| Eylemler | Hayır | İş akışı çalışma zamanında yürütmek bir veya daha fazla eylemler için tanımları <p><p>Maksimum eylem: 250 | 
| çıkışlar | Hayır | Çalıştıran bir iş akışını dönüş çıkışları tanımları <p><p>En fazla çıkışları: 10 |  
|||| 

## <a name="parameters"></a>Parametreler

İçinde `parameters` bölümünde, iş akışı çalışma zamanında girdileri kabul tüm parametrelerini tanımlayın. Bu parametreler diğer iş akışı bölümlerinde kullanmadan önce bu bölümlerdeki tüm parametreleri bildirme emin olun.

Bir parametre tanımı için genel yapısı şöyledir:  

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
| type | Evet | int, float, dize, securestring, bool, dizi, JSON nesnesi, secureobject <p><p>**Not**: tüm parolalar, anahtarlar ve gizli anahtarları için kullanmak `securestring` ve `secureobject` çünkü `GET` işlemi, bu tür iade değil. | Parametresinin türü |
| defaultValue | Hayır | Aynı `type` | İş akışı başlattığında değer belirtilmezse, varsayılan parametre değeri | 
| allowedValues | Hayır | Aynı `type` | Bir dizi parametre kabul edebilir değerlerle |  
| meta veriler | Hayır | JSON nesnesi | Tüm diğer parametre ayrıntıları, örneğin, adı veya mantıksal uygulama veya Visual Studio'ya veya diğer araçları tarafından kullanılan tasarım zamanı verileri için okunabilir bir açıklama |  
||||
  
## <a name="triggers-and-actions"></a>Tetikleyiciler ve eylemler  

Bir iş akışı tanımında `triggers` ve `actions` bölümler, iş akışınızın yürütme sırasında gerçekleşecek çağrıları tanımlayın. Sözdizimi ve bu bölümleri hakkında daha fazla bilgi için bkz: [iş akışı tetikleyiciler ve Eylemler](../logic-apps/logic-apps-workflow-actions-triggers.md).
  
## <a name="outputs"></a>Çıkışlar 

İçinde `outputs` bölümünde, iş akışınızı bittiğinde dönebilirsiniz veri tanımlama çalışıyor. Örneğin, bir özel durum veya değer her çalıştırma izlemek için iş akışı çıktı verileri döndürür belirtin. 

Bir çıkış tanımı için genel yapısı şöyledir: 

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
| <*anahtar adı*> | Evet | Dize | Anahtar adı çıktısı için dönüş değeri |  
| type | Evet | int, float, dize, securestring, bool, dizi, JSON nesnesi | Output dönüş değeri türü | 
| değer | Evet | Aynı `type` | Output dönüş değeri |  
||||| 

Çalıştıran bir iş akışından çıkış almak için mantığı uygulamanın çalıştırma geçmişi ve Azure portalında ayrıntılarını gözden geçirmek veya kullanmak [iş akışı REST API](https://docs.microsoft.com/rest/api/logic/workflows). Panolar oluşturabilmesi için dış sistemlere, örneğin, Powerbı çıkış de geçirebilirsiniz. 

> [!NOTE]
> Bir hizmetin REST API öğesinden gelen isteklere yanıt verirken kullanmayın `outputs`. Bunun yerine, kullanın `Response` eylem türü. Daha fazla bilgi için bkz: [iş akışı tetikleyiciler ve Eylemler](../logic-apps/logic-apps-workflow-actions-triggers.md).

<a name="expressions"></a>

## <a name="expressions"></a>İfadeler

JSON ile tasarım zamanında, örneğin mevcut değişmez değer değerlere sahip olabilir:

```json
"customerName": "Sophia Owen", 
"rainbowColors": ["red", "orange", "yellow", "green", "blue", "indigo", "violet"], 
"rainbowColorsCount": 7 
```

Ayrıca, çalışma zamanına kadar yoksa değerlere sahip olabilir. Bu değerleri temsil etmek için kullanabileceğiniz *ifadeleri*, çalışma zamanında değerlendirilir. Bir ifadenin bir veya daha fazla içerebilen bir dizidir [işlevleri](#functions), [işleçleri](#operators), değişkenleri, açık değerler veya sabitler. İş akışı tanımınızı bir ifade herhangi bir yerden bir JSON dizesi değerindeki oturum sırasında ifadesiyle ekleyerek kullanabilirsiniz (@). Bir JSON değerini temsil eden bir ifade değerlendirirken, deyim gövdesinin kaldırarak ayıklanan @ karakteri ve her zaman başka bir JSON değeri sonuçlanır. 

Örneğin, önceden tanımlanmış `customerName` özelliğini kullanarak özellik değeri alabilirsiniz [parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) işlev bir ifadede ve bu değer atadığınız `accountName` özelliği:

```json
"customerName": "Sophia Owen", 
"accountName": "@parameters('customerName')"
```

*Dize ilişkilendirme* ayrıca birden çok ifadelerinin tarafından Sarmalanan dizeleri içinde kullandığınız sağlar @ karakteri ve küme ayraçları ({}). Sözdizimi şöyledir:

```json
@{ "<expression1>", "<expression2>" }
```

Bu özellik benzer yapmadan bir dize sonucudur her zaman `concat()` işlev, örneğin: 

```json
"customerName": "First name: @{parameters('firstName')} Last name: @{parameters('lastName')}"
```

İle başlayan bir sabit dize varsa karakter öneki @ karakteri kaçış karakteri olarak başka bir karakterle @: @@

Bu örnekler ifadeler nasıl değerlendirilir gösterir:

| JSON değeri | Sonuç |
|------------|--------| 
| "Sophia Owen" | Bu karakterleri döndürür: 'Sophia Owen' |
| "dizisi [1]" | Bu karakterleri döndürür: 'array [1]' |
| "@@" | Bu karakterleri tek karakterli bir dize döndürür: ' @' |   
| " @" | Bu karakterler iki karakterli bir dize döndürür: ' @' |
|||

Bu örnekler için "myBirthMonth" eşit "Ocak" ve "myAge" 42 sayıya eşit tanımlayın varsayın:  
  
```json
"myBirthMonth": "January",
"myAge": 42
```

Bu örnekler, aşağıdaki ifadeler nasıl değerlendirildiği gösterir:

| JSON ifade | Sonuç |
|-----------------|--------| 
| "@parameters('myBirthMonth')" | Bu dize döndürecek: "Ocak" |  
| "@{parameters('myBirthMonth')}" | Bu dize döndürecek: "Ocak" |  
| "@parameters('myAge')" | Bu sayı döndürür: 42 |  
| "@{parameters('myAge')}" | Bu bir dize olarak numara döndürür: "42" |  
| "Benim geçerlilik süresi @{parameters('myAge')}" | Bu dize döndürecek: "yaşımı 42 olduğu" |  
| "@concat(' Yaşımı ', string(parameters('myAge')))" | Bu dize döndürecek: "yaşımı 42 olduğu" |  
| "Benim geçerlilik süresi @@ {parameters('myAge')}" | İfade içeriyor bu dizeyi döndürür: "Benim geçerlilik süresi @{parameters('myAge')}' | 
||| 

Logic Apps Tasarımcısı'nda görsel olarak çalışırken, Deyim Oluşturucusu'nu aracılığıyla ifadeleri örneğin oluşturabilirsiniz: 

![Logic Apps Tasarımcısı > ifade oluşturucusu](./media/logic-apps-workflow-definition-language/expression-builder.png)

İşiniz bittiğinde, ifade iş akışı tanımınızı karşılık gelen özellik için örneğin görünür, `searchQuery` özelliği burada:

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

İçinde [ifadeleri](#expressions) ve [işlevleri](#functions), işleçler başvurusu bir özellik veya bir değer dizideki gibi belirli görevleri gerçekleştirebilir. 

| İşleç | Görev | 
|----------|------|
| ' | Kaydırma Örneğin, tek tırnak işareti dizesiyle yalnızca bir dize sabit değeri giriş olarak veya ifadeleri ve işlevleri kullanmak için `'<myString>'`. Çift tırnak işaretleri kullanmayın (""), JSON tüm ifadenin biçimlendirme ile çakışıyor. Örneğin: <p>**Evet**: length('Hello') </br>**Hayır**: length("Hello") <p>Diziler veya sayı geçirdiğinizde, noktalama kaydırma gerekmez. Örneğin: <p>**Evet**: uzunluğu ([1, 2, 3]) </br>**Hayır**: uzunluğu ("[1, 2, 3]") | 
| [] | Belirli bir konuma (dizin) bir dizi değerinde başvurmak için köşeli ayraç kullanın. Örneğin, bir dizi ikinci öğesini almak için şunu yazın: <p>`myArray[1]` | 
| . | Bir nesne özelliğinde başvurmak için nokta işleci kullanın. Örneğin, almak için `name` özelliği için bir `customer` JSON nesnesi: <p>`"@parameters('customer').name"` | 
| ? | Çalışma zamanı hatası olmadan bir nesne null özelliklerinde başvurmak için soru işareti işlecini kullanın. Örneğin, bir tetikleyiciden null çıkışları işlemek için bu ifade kullanabilirsiniz: <p>`@coalesce(trigger().outputs?.body?.<someProperty>, '<property-default-value>')` | 
||| 

<a name="functions"></a>

## <a name="functions"></a>İşlevler

Bazı ifadeler bir mantıksal uygulama çalışmaya başladığında henüz var olmayabilir çalışma zamanı Eylemler değerlerini alırlar. Bir başvuru veya bu değerleri ifadelerde çalışmak için kullanabileceğiniz *işlevler*. Örneğin, hesaplamalar için matematik işlevleri gibi kullanabilirsiniz [add()](../logic-apps/workflow-definition-language-functions-reference.md#add) tamsayılar veya float toplamını döndürür işlevi. 

İşlevleri ile gerçekleştirebileceğiniz sadece birkaç örnek görevleri şunlardır: 

| Görev | İşlev sözdizimi | Sonuç | 
| ---- | --------------- | -------------- | 
| Küçük harf biçiminde bir dize döndürür. | toLower ('<*metin*>') <p>Örneğin: toLower('Hello') | "hello" | 
| Bir genel benzersiz tanımlayıcı (GUID) döndürür. | Guid() |"c2ecc88d-88c8-4096-912c-d6f2e2b138ce" | 
|||| 

Bu örnek, değerden nasıl erişebileceğini gösterir `customerName` parametresi ve değeri Ata `accountName` kullanarak özellik [parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) işlevi bir ifadede:

```json
"accountName": "@parameters('customerName')"
```

İfadelerde işlevleri kullanabileceğiniz diğer bazı genel yollar şunlardır:

| Görev | Bir ifade sözdiziminde işlevi | 
| ---- | -------------------------------- | 
| Bu öğe bir işleve geçirerek bir öğesiyle iş gerçekleştirin. | "@<*functionName*> (<*öğesi*>)" | 
| 1. Alma *parameterName*'s iç içe kullanarak değer `parameters()` işlevi. </br>2. Bu değeri geçirerek sonuç ile çalışmayı gerçekleştirmek *functionName*. | "@<*functionName*> (parametreleri ('<*parameterName*>'))" | 
| 1. İç içe geçmiş iç işlevinden sonucu elde *functionName*. </br>2. Dış işlev için sonuç geçirmek *functionName2*. | "@<*functionName2*> (<*functionName*> (<*öğesi*>))" | 
| 1. Sonuç elde *functionName*. </br>2. Sonuç özelliğine sahip bir nesne olması koşuluyla, *propertyName*, bu özelliğin değerini alın. | "@<*functionName*>(<*item*>). <*propertyName*>" | 
||| 

Örneğin, `concat()` işlevi parametre olarak iki veya daha fazla dize değerlerini alabilir. Bu işlevin Bu dizelerin bir dizesinde birleştirir. Böylece "SophiaOwen" birleştirilmiş bir dize almak ya da dize değişmez değerleri Örneğin, "Sophia" ve "Owen" geçirebilirsiniz:

```json
"customerName": "@concat('Sophia', 'Owen')"
```

Veya parametrelerinden dize değerleri alabilirsiniz. Bu örnekte `parameters()` işlevi her `concat()` parametre ve `firstName` ve `lastName` parametreleri. Ardından elde edilen dizelere geçirdiğiniz `concat()` birleşik bir dize, örneğin, "SophiaOwen" almak için işlev:

```json
"customerName": "@concat(parameters('firstName'), parameters('lastName'))"
```

Her iki durumda da, her iki örnekler sonucu atamak `customerName` özelliği. 

Her işlevi hakkında ayrıntılı bilgi için bkz: [alfabetik başvurusu makalesinde](../logic-apps/workflow-definition-language-functions-reference.md).
Veya, kendi genel amacına bağlı işlevleri hakkında bilgi almaya devam etmek.

<a name="string-functions"></a>

### <a name="string-functions"></a>Dize işlevleri

Dizelerle çalışmak için bu dize işlevleri ve ayrıca bazı kullanabilirsiniz [toplama işlevleri](#collection-functions). Dize işlevleri yalnızca dizeleri çalışır. 

| String işlevi | Görev | 
| --------------- | ---- | 
| [concat](../logic-apps/workflow-definition-language-functions-reference.md#concat) | İki veya daha fazla dizeleri birleştirmek ve birleştirilmiş dizeyi döndürür. | 
| [endsWith](../logic-apps/workflow-definition-language-functions-reference.md#endswith) | Bir dizeyi belirtilen dizeyle bitip olup olmadığını denetler. | 
| [GUID](../logic-apps/workflow-definition-language-functions-reference.md#guid) | Bir genel benzersiz tanımlayıcı (GUID) olarak bir dize oluşturur. | 
| [IndexOf](../logic-apps/workflow-definition-language-functions-reference.md#indexof) | Bir alt dizesi başlangıç konumunu döndürür. | 
| [lastIndexOf](../logic-apps/workflow-definition-language-functions-reference.md#lastindexof) | Bitiş konumu için bir alt dizeyi döndürür. | 
| [Değiştir](../logic-apps/workflow-definition-language-functions-reference.md#replace) | Bir alt dizenin belirtilen dizesi ile değiştirin ve güncelleştirilmiş dize döndürür. | 
| [split](../logic-apps/workflow-definition-language-functions-reference.md#split) | Bir dizeden tüm karakterler içeriyor ve her bir karakteri ile belirli sınırlayıcı karakter ayıran bir dizi döndürür. | 
| [startsWith](../logic-apps/workflow-definition-language-functions-reference.md#startswith) | Bir dizeyi belirli bir alt dizesi ile başlayıp başlamadığını denetleyin. | 
| [substring](../logic-apps/workflow-definition-language-functions-reference.md#substring) | Belirtilen konumdan başlayarak, bir dizeden karakterleri döndür. | 
| [toLower](../logic-apps/workflow-definition-language-functions-reference.md#toLower) | Küçük harf biçiminde bir dize döndürür. | 
| [toUpper](../logic-apps/workflow-definition-language-functions-reference.md#toUpper) | Büyük harf biçiminde bir dize döndürür. | 
| [Kırpma](../logic-apps/workflow-definition-language-functions-reference.md#trim) | Bir dizeden öndeki ve sondaki boşlukları kaldırın ve güncelleştirilmiş dize döndürür. | 
||| 

<a name="collection-functions"></a>

### <a name="collection-functions"></a>Toplama işlevleri

Koleksiyonlar, genellikle dizileri, dizeleri ve sözlük ile bazı durumlarda, çalışması için bu toplama işlevleri kullanabilirsiniz. 

| Toplama işlevi | Görev | 
| ------------------- | ---- | 
| [içerir](../logic-apps/workflow-definition-language-functions-reference.md#contains) | Bir koleksiyon belirli bir öğeye sahip olup olmadığını denetleyin. |
| [boş](../logic-apps/workflow-definition-language-functions-reference.md#empty) | Bir koleksiyonu boş olup olmadığını denetleyin. | 
| [ilk](../logic-apps/workflow-definition-language-functions-reference.md#first) | İlk öğeyi koleksiyondan döndürür. | 
| [kesişim](../logic-apps/workflow-definition-language-functions-reference.md#intersection) | Sahip bir koleksiyonun dönmek *yalnızca* belirtilen koleksiyonlarla arasında ortak öğeler. | 
| [join](../logic-apps/workflow-definition-language-functions-reference.md#join) | Sahip bir dize döndürecek *tüm* bir dizi öğelerinden belirtilen karakteriyle ayrılmış. | 
| [Son](../logic-apps/workflow-definition-language-functions-reference.md#last) | Son öğeyi koleksiyondan döndürür. | 
| [uzunluğu](../logic-apps/workflow-definition-language-functions-reference.md#length) | Bir dize veya dizi öğe sayısını döndürür. | 
| [Atla](../logic-apps/workflow-definition-language-functions-reference.md#skip) | Bir koleksiyon Önden öğeleri kaldırmak ve geri dönüp *diğer tüm* öğeleri. | 
| [Al](../logic-apps/workflow-definition-language-functions-reference.md#take) | Öğeleri bir koleksiyon Önden döndür. | 
| [birleşim](../logic-apps/workflow-definition-language-functions-reference.md#union) | Sahip bir koleksiyonun dönmek *tüm* belirtilen koleksiyonlarla öğelerinden. | 
||| 

<a name="comparison-functions"></a>

### <a name="comparison-functions"></a>Karşılaştırma işlevleri

Koşullarla çalışma, değerler ve ifade sonuçları karşılaştırmak ya da çeşitli mantığı değerlendirmek için bu karşılaştırma işlevlerini kullanabilirsiniz. Her işlevi hakkında tam başvuru için bkz: [alfabetik başvurusu makalesinde](../logic-apps/workflow-definition-language-functions-reference.md).

| Karşılaştırma işlevi | Görev | 
| ------------------- | ---- | 
| [Ve](../logic-apps/workflow-definition-language-functions-reference.md#and) | Tüm ifadelerin doğru olup olmadığını denetleyin. | 
| [eşittir](../logic-apps/workflow-definition-language-functions-reference.md#equals) | Her iki değerin eşit olup olmadığını denetleyin. | 
| [büyük](../logic-apps/workflow-definition-language-functions-reference.md#greater) | İlk değer ikinci değerden daha büyük olup olmadığını denetleyin. | 
| [greaterOrEquals](../logic-apps/workflow-definition-language-functions-reference.md#greaterOrEquals) | İlk değer ikinci değerine eşit veya daha büyük olup olmadığını denetleyin. | 
| [Eğer](../logic-apps/workflow-definition-language-functions-reference.md#if) | Bir ifadenin true veya false olup olmadığını denetleyin. Sonucuna bağlı olarak, belirtilen bir değeri döndürür. | 
| [daha az](../logic-apps/workflow-definition-language-functions-reference.md#less) | İlk değer olup değerinden ikinci denetleyin. | 
| [lessOrEquals](../logic-apps/workflow-definition-language-functions-reference.md#lessOrEquals) | İlk değer küçük veya bu ikinci değer eşit olup olmadığını denetleyin. | 
| [değil](../logic-apps/workflow-definition-language-functions-reference.md#not) | Bir ifade yanlış olup olmadığını denetleyin. | 
| [Veya](../logic-apps/workflow-definition-language-functions-reference.md#or) | En az bir ifadenin doğru olup olmadığını denetleyin. |
||| 

<a name="conversion-functions"></a>

### <a name="conversion-functions"></a>Dönüşüm işlevleri

Bir değerin türü veya biçimi değiştirmek için bu dönüşüm işlevleri kullanabilirsiniz. Örneğin, bir değer bir tamsayı olarak bir Boole değeri değiştirebilirsiniz. Logic Apps içerik türleri dönüştürme sırasında nasıl işlediğini öğrenmek için bkz: [işleyecek içerik türleri](../logic-apps/logic-apps-content-type.md). Her işlevi hakkında tam başvuru için bkz: [alfabetik başvurusu makalesinde](../logic-apps/workflow-definition-language-functions-reference.md).

| Dönüştürme işlevi | Görev | 
| ------------------- | ---- | 
| [Dizi](../logic-apps/workflow-definition-language-functions-reference.md#array) | Belirtilen tek bir giriş için bir dizi döndürür. Birden çok giriş için bkz: [createArray](../logic-apps/workflow-definition-language-functions-reference.md#createArray). | 
| [Base64](../logic-apps/workflow-definition-language-functions-reference.md#base64) | Base64 ile kodlanmış sürüm bir dize döndürür. | 
| [base64ToBinary](../logic-apps/workflow-definition-language-functions-reference.md#base64ToBinary) | İkili dosya sürümü base64 ile kodlanmış bir dize döndürür. | 
| [base64ToString](../logic-apps/workflow-definition-language-functions-reference.md#base64ToString) | Dize sürüm base64 ile kodlanmış bir dize döndürür. | 
| [İkili](../logic-apps/workflow-definition-language-functions-reference.md#binary) | İkili dosya sürümü için bir giriş değeri döndürür. | 
| [bool](../logic-apps/workflow-definition-language-functions-reference.md#bool) | Giriş değeri Boolean sürümü döndürür. | 
| [createArray](../logic-apps/workflow-definition-language-functions-reference.md#createArray) | Birden çok girişlerinde bir dizi döndürür. | 
| [dataUri](../logic-apps/workflow-definition-language-functions-reference.md#dataUri) | Bir giriş değerinin verilerini URI döndürür. | 
| [dataUriToBinary](../logic-apps/workflow-definition-language-functions-reference.md#dataUriToBinary) | Veri URI'si için ikili dosya sürümü döndürür. | 
| [dataUriToString](../logic-apps/workflow-definition-language-functions-reference.md#dataUriToString) | Veri URI'si için string sürümüne dönün. | 
| [decodeBase64](../logic-apps/workflow-definition-language-functions-reference.md#decodeBase64) | Dize sürüm base64 ile kodlanmış bir dize döndürür. | 
| [decodeDataUri](../logic-apps/workflow-definition-language-functions-reference.md#decodeDataUri) | Veri URI'si için ikili dosya sürümü döndürür. | 
| [Decodeurıcomponent](../logic-apps/workflow-definition-language-functions-reference.md#decodeUriComponent) | Return değiştirir kodu çözülmüş sürümleriyle karakterlerinden Çık bir dize. | 
| [Encodeurıcomponent](../logic-apps/workflow-definition-language-functions-reference.md#encodeUriComponent) | Kaçış karakterli URL güvenli olmayan karakterleri değiştirir bir dize döndürür. | 
| [Kayan nokta](../logic-apps/workflow-definition-language-functions-reference.md#float) | Dönüş bir kayan nokta sayısı için bir giriş değeri. | 
| [Int](../logic-apps/workflow-definition-language-functions-reference.md#int) | Tamsayı sürüm bir dize döndürür. | 
| [JSON](../logic-apps/workflow-definition-language-functions-reference.md#json) | JavaScript nesne gösterimi (JSON) türü değeri veya bir dize veya XML nesnesi döndürür. | 
| [Dize](../logic-apps/workflow-definition-language-functions-reference.md#string) | Giriş değeri dize sürümünü döndürür. | 
| [uriComponent](../logic-apps/workflow-definition-language-functions-reference.md#uriComponent) | Kaçış karakterli URL güvenli olmayan karakterleri değiştirerek bir giriş değerinin URI kodlu sürümü döndürür. | 
| [uriComponentToBinary](../logic-apps/workflow-definition-language-functions-reference.md#uriComponentToBinary) | İkili dosya sürümü için URI ile kodlanmış bir dize döndürür. | 
| [uriComponentToString](../logic-apps/workflow-definition-language-functions-reference.md#uriComponentToString) | Dize sürümü için URI ile kodlanmış bir dize döndürür. | 
| [xml](../logic-apps/workflow-definition-language-functions-reference.md#xml) | XML sürümü bir dize döndürür. | 
||| 

<a name="math-functions"></a>

### <a name="math-functions"></a>Matematik işlevleri

Tamsayılar ve float çalışmak için bu matematik işlevleri kullanabilirsiniz. Her işlevi hakkında tam başvuru için bkz: [alfabetik başvurusu makalesinde](../logic-apps/workflow-definition-language-functions-reference.md).

| Matematik işlevi | Görev | 
| ------------- | ---- | 
| [Ekleme](../logic-apps/workflow-definition-language-functions-reference.md#add) | İki sayı ekleme sonucu döndürür. | 
| [div](../logic-apps/workflow-definition-language-functions-reference.md#div) | İki sayı bölen sonucu döndürür. | 
| [max](../logic-apps/workflow-definition-language-functions-reference.md#max) | En yüksek değer bir dizi numaraları veya bir dizi döndürür. | 
| [Min](../logic-apps/workflow-definition-language-functions-reference.md#min) | En düşük değer bir dizi numaraları veya bir dizi döndürür. | 
| [mod](../logic-apps/workflow-definition-language-functions-reference.md#mod) | İki sayı bölen kalanı döndürür. | 
| [mul](../logic-apps/workflow-definition-language-functions-reference.md#mul) | İki sayı çarparak ürün döndür. | 
| [rand](../logic-apps/workflow-definition-language-functions-reference.md#rand) | Belirtilen aralığından rasgele bir tamsayı döndürür. | 
| [Aralık](../logic-apps/workflow-definition-language-functions-reference.md#range) | Belirtilen bir tamsayı başlayan tamsayı dizisi döndürür. | 
| [Sub](../logic-apps/workflow-definition-language-functions-reference.md#sub) | İkinci sayı ilk sayıdan çıkarılmasıyla sonucu döndürür. | 
||| 

<a name="date-time-functions"></a>

### <a name="date-and-time-functions"></a>Tarih ve saat işlevleri

Tarih ve saatlerle çalışmak için bu tarih ve saat işlevlerini kullanabilirsiniz.
Her işlevi hakkında tam başvuru için bkz: [alfabetik başvurusu makalesinde](../logic-apps/workflow-definition-language-functions-reference.md).

| Tarih veya saat işlevi | Görev | 
| --------------------- | ---- | 
| [addDays](../logic-apps/workflow-definition-language-functions-reference.md#addDays) | Bir gün sayısı için bir zaman damgası ekleyin. | 
| [addHours](../logic-apps/workflow-definition-language-functions-reference.md#addHours) | Saat sayısı için bir zaman damgası ekleyin. | 
| [addMinutes](../logic-apps/workflow-definition-language-functions-reference.md#addMinutes) | Dakika sayısı için bir zaman damgası ekleyin. | 
| [saniyeEkle](../logic-apps/workflow-definition-language-functions-reference.md#addSeconds) | Saniye sayısı için bir zaman damgası ekleyin. |  
| [addToTime](../logic-apps/workflow-definition-language-functions-reference.md#addToTime) | Zaman birimi sayısı için bir zaman damgası ekleyin. Ayrıca bkz. [getFutureTime](../logic-apps/workflow-definition-language-functions-reference.md#getFutureTime). | 
| [convertFromUtc](../logic-apps/workflow-definition-language-functions-reference.md#convertFromUtc) | Bir zaman damgası (UTC) Evrensel zaman Eşgüdümlü gelen hedef zaman dilimine dönüştürün. | 
| [convertTimeZone](../logic-apps/workflow-definition-language-functions-reference.md#convertTimeZone) | Bir zaman damgası hedef zaman dilimine kaynak saat dilimine dönüştürür. | 
| [convertToUtc](../logic-apps/workflow-definition-language-functions-reference.md#convertToUtc) | Bir zaman damgası (UTC) saati Eşgüdümlü Evrensel Kaynak saat dilimine dönüştürün. | 
| [DayOfMonth](../logic-apps/workflow-definition-language-functions-reference.md#dayOfMonth) | Bir zaman damgası ay bileşenini gününü döndür. | 
| [DayOfWeek](../logic-apps/workflow-definition-language-functions-reference.md#dayOfWeek) | Bir zaman damgası haftanın günü bileşenini döndür. | 
| [DayOfYear](../logic-apps/workflow-definition-language-functions-reference.md#dayOfYear) | Bir zaman damgası yıl bileşenini gününü döndür. | 
| [FormatDateTime](../logic-apps/workflow-definition-language-functions-reference.md#formatDateTime) | Bir zaman damgası tarihi döndürür. | 
| [getFutureTime](../logic-apps/workflow-definition-language-functions-reference.md#getFutureTime) | Geçerli zaman damgası artı belirtilen zaman birimlerini döndür. Ayrıca bkz. [addToTime](../logic-apps/workflow-definition-language-functions-reference.md#addToTime). | 
| [getPastTime](../logic-apps/workflow-definition-language-functions-reference.md#getPastTime) | Belirtilen zaman birimlerini eksi geçerli zaman damgası döndür. Ayrıca bkz. [subtractFromTime](../logic-apps/workflow-definition-language-functions-reference.md#subtractFromTime). | 
| [startOfDay](../logic-apps/workflow-definition-language-functions-reference.md#startOfDay) | Bir zaman damgası için günün başlangıcını döndür. | 
| [startOfHour](../logic-apps/workflow-definition-language-functions-reference.md#startOfHour) | Bir zaman damgası için saatin başlangıcını döndür. | 
| [startOfMonth](../logic-apps/workflow-definition-language-functions-reference.md#startOfMonth) | Bir zaman damgası için ayın başlangıcını döndürür. | 
| [subtractFromTime](../logic-apps/workflow-definition-language-functions-reference.md#subtractFromTime) | Bir zaman damgası zaman birimlerinden sayısı çıkarın. Ayrıca bkz. [getPastTime](../logic-apps/workflow-definition-language-functions-reference.md#getPastTime). | 
| [işaretleri](../logic-apps/workflow-definition-language-functions-reference.md#ticks) | Dönüş `ticks` belirtilen bir zaman damgası için özellik değeri. | 
| [utcNow](../logic-apps/workflow-definition-language-functions-reference.md#utcNow) | Geçerli zaman damgası dize olarak döndürür. | 
||| 

<a name="workflow-functions"></a>

### <a name="workflow-functions"></a>İş akışı işlevleri

Bu iş akışı işlevler yardımcı olabilir:

* Bir iş akışı örneği ayrıntılarını çalışma zamanında alın. 
* Logic apps oluşturmak için kullanılan girişleri ile çalışır.
* Tetikleyiciler ve Eylemler çıkışları başvuru.

Örneğin, bir eylemden çıktıları başvuru ve bu verilerin bir sonraki eylem kullanın. Her işlevi hakkında tam başvuru için bkz: [alfabetik başvurusu makalesinde](../logic-apps/workflow-definition-language-functions-reference.md).

| İş akışı işlevi | Görev | 
| ----------------- | ---- | 
| [Eylem](../logic-apps/workflow-definition-language-functions-reference.md#action) | Çalışma zamanı veya değerleri geçerli eylemin çıkış diğer JSON ad/değer çiftlerinden döndür. Ayrıca bkz. [Eylemler](../logic-apps/workflow-definition-language-functions-reference.md#actions). | 
| [actionBody](../logic-apps/workflow-definition-language-functions-reference.md#actionBody) | Bir eylemin dönüş `body` çalışma zamanında çıktı. Ayrıca bkz. [gövde](../logic-apps/workflow-definition-language-functions-reference.md#body). | 
| [actionOutputs](../logic-apps/workflow-definition-language-functions-reference.md#actionOutputs) | Çalışma zamanında bir eylemin çıkış döndür. Bkz: [Eylemler](../logic-apps/workflow-definition-language-functions-reference.md#actions). | 
| [Eylemler](../logic-apps/workflow-definition-language-functions-reference.md#actions) | Çalışma zamanı veya değerleri bir eylemin çıkış diğer JSON ad/değer çiftlerinden döndür. Ayrıca bkz. [eylem](../logic-apps/workflow-definition-language-functions-reference.md#action).  | 
| [Gövde](#body) | Bir eylemin dönüş `body` çalışma zamanında çıktı. Ayrıca bkz. [actionBody](../logic-apps/workflow-definition-language-functions-reference.md#actionBody). | 
| [formDataMultiValues](../logic-apps/workflow-definition-language-functions-reference.md#formDataMultiValues) | Anahtar adı ile eşleşmesi değerleri içeren bir dizi oluşturmak *form verilerini* veya *form kodlu* eylem çıkarır. | 
| [formDataValue](../logic-apps/workflow-definition-language-functions-reference.md#formDataValue) | Bir eylemin bir anahtar adı ile eşleşen tek bir değer döndürmesi *form verilerini* veya *form kodlu çıkış*. | 
| [Öğesi](../logic-apps/workflow-definition-language-functions-reference.md#item) | Bir dizi üzerinden bir yinelenen eylemi olduğu zaman içinde eylemin geçerli yineleme sırasında dizideki geçerli öğeyi döndürür. | 
| [Öğeleri](../logic-apps/workflow-definition-language-functions-reference.md#items) | İçinde bir için-her veya-kadar-döngü, dönüş olduğunda geçerli öğe belirtilen döngü.| 
| [listCallbackUrl](../logic-apps/workflow-definition-language-functions-reference.md#listCallbackUrl) | "Bir tetikleyici ya da eylem çağıran geri çağırma URL'si" döndürür. | 
| [multipartBody](../logic-apps/workflow-definition-language-functions-reference.md#multipartBody) | Gövdesi belirli bir bölümü için birden çok bölümü olan bir eylemin çıkış döndür. | 
| [parametreler](../logic-apps/workflow-definition-language-functions-reference.md#parameters) | Logic app tanımı'nda açıklanan bir parametre için değer döndürür. | 
| [Tetikleyici](../logic-apps/workflow-definition-language-functions-reference.md#trigger) | Tetikleyici ait çıktı çalışma zamanında veya diğer JSON ad ve değer çiftleri döndür. Ayrıca bkz. [triggerOutputs](#triggerOutputs) ve [triggerBody](../logic-apps/workflow-definition-language-functions-reference.md#triggerBody). | 
| [triggerBody](../logic-apps/workflow-definition-language-functions-reference.md#triggerBody) | Bir tetikleyici dönmek `body` çalışma zamanında çıktı. Bkz: [tetikleyici](../logic-apps/workflow-definition-language-functions-reference.md#trigger). | 
| [triggerFormDataValue](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataValue) | Bir anahtar adı eşleşen tek bir değer döndürmesi *form verilerini* veya *form kodlu* tetiklemek çıkarır. | 
| [triggerMultipartBody](../logic-apps/workflow-definition-language-functions-reference.md#triggerMultipartBody) | Belirli bir bölümü gövdesi bir tetikleyicinin çok bölümlü çıktısında döndür. | 
| [triggerFormDataMultiValues](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataMultiValues) | Değerleri bir anahtar adı eşleşen bir dizi oluşturmak *form verilerini* veya *form kodlu* tetiklemek çıkarır. | 
| [triggerOutputs](../logic-apps/workflow-definition-language-functions-reference.md#triggerOutputs) | Çalışma zamanı veya değerleri bir tetikleyicinin çıkış diğer JSON ad/değer çiftlerinden döndür. Bkz: [tetikleyici](../logic-apps/workflow-definition-language-functions-reference.md#trigger). | 
| [değişkenleri](../logic-apps/workflow-definition-language-functions-reference.md#variables) | Belirtilen bir değişken değeri döndürür. | 
| [İş akışı](../logic-apps/workflow-definition-language-functions-reference.md#workflow) | İş akışı ile ilgili tüm ayrıntıları çalışma zamanı sırasında döndür. | 
||| 

<a name="uri-parsing-functions"></a>

### <a name="uri-parsing-functions"></a>URI işlevleri ayrıştırma

Tekdüzen Kaynak Tanımlayıcıları (URI'ler) ile çalışır ve bu URI çeşitli özellik değerlerini almak için işlevleri ayrıştırma bu URI kullanabilirsiniz. Her işlevi hakkında tam başvuru için bkz: [alfabetik başvurusu makalesinde](../logic-apps/workflow-definition-language-functions-reference.md).

| URI işlevi ayrıştırma | Görev | 
| -------------------- | ---- | 
| [uriHost](../logic-apps/workflow-definition-language-functions-reference.md#uriHost) | Dönüş `host` Tekdüzen Kaynak Tanımlayıcısı (URI) değeri. | 
| [uriPath](../logic-apps/workflow-definition-language-functions-reference.md#uriPath) | Dönüş `path` Tekdüzen Kaynak Tanımlayıcısı (URI) değeri. | 
| [uriPathAndQuery](../logic-apps/workflow-definition-language-functions-reference.md#uriPathAndQuery) | Dönüş `path` ve `query` Tekdüzen Kaynak Tanımlayıcısı (URI) değerleri. | 
| [uriPort](../logic-apps/workflow-definition-language-functions-reference.md#uriPort) | Dönüş `port` Tekdüzen Kaynak Tanımlayıcısı (URI) değeri. | 
| [uriQuery](../logic-apps/workflow-definition-language-functions-reference.md#uriQuery) | Dönüş `query` Tekdüzen Kaynak Tanımlayıcısı (URI) değeri. | 
| [UriScheme](../logic-apps/workflow-definition-language-functions-reference.md#uriScheme) | Dönüş `scheme` Tekdüzen Kaynak Tanımlayıcısı (URI) değeri. | 
||| 

<a name="manipulation-functions"></a>

### <a name="json-and-xml-functions"></a>JSON ve XML işlevleri

JSON nesnelerinin ve XML düğümlerini çalışmak için bu işleme işlevleri kullanabilirsiniz. Her işlevi hakkında tam başvuru için bkz: [alfabetik başvurusu makalesinde](../logic-apps/workflow-definition-language-functions-reference.md).

| İşleme işlevi | Görev | 
| --------------------- | ---- | 
| [addProperty](../logic-apps/workflow-definition-language-functions-reference.md#addProperty) | Bir özellik ve onun değeri veya ad-değer çiftinin bir JSON nesnesi ekleyin ve güncelleştirilmiş nesneyi döndürür. | 
| [birleşim](../logic-apps/workflow-definition-language-functions-reference.md#coalesce) | İlk değeri null olmayan bir veya daha fazla parametrelerinden döndür. | 
| [removeProperty](../logic-apps/workflow-definition-language-functions-reference.md#removeProperty) | Bir JSON nesnesinde bir özelliğini kaldırın ve güncelleştirilmiş nesneyi döndürür. | 
| [setProperty](../logic-apps/workflow-definition-language-functions-reference.md#setProperty) | Bir JSON nesnesinin özellik değerini ayarlayın ve güncelleştirilmiş nesneyi döndürür. | 
| [XPath](../logic-apps/workflow-definition-language-functions-reference.md#xpath) | XML düğüm veya XPath (XML Path dili) ifadenin eşleşen değerleri olup olmadığını denetleyin ve eşleşen düğümleri veya değerleri döndürür. | 
||| 

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [iş akışı tanımlama dili eylemleri ve Tetikleyicileri](../logic-apps/logic-apps-workflow-actions-triggers.md)
* Program aracılığıyla oluşturma ve logic apps ile yönetme hakkında bilgi edinin [iş akışı REST API'si](https://docs.microsoft.com/rest/api/logic/workflows)
