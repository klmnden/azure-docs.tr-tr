---
title: İş akışı tanımlama dili şema - Azure Logic Apps | Microsoft Docs
description: Azure mantıksal uygulamaları için iş akışı tanımı şemasını temel iş akışları tanımlar
services: logic-apps
author: jeffhollan
manager: anneta
editor: ''
documentationcenter: ''
ms.assetid: 26c94308-aa0d-4730-97b6-de848bffff91
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 03/21/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 42932e6d1727a1444c62f565ae3c48dc178aeb2b
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
# <a name="workflow-definition-language-schema-for-azure-logic-apps"></a>Azure mantıksal uygulamaları için iş akışı tanımlama dili şeması

Bir iş akışı tanımı mantıksal uygulamanızı bir parçası olarak yürütür gerçek mantığını içerir. Bu tanımı, bir veya daha fazla mantıksal uygulama Başlat tetikleyiciler ve yapılacak mantıksal uygulama için bir veya daha fazla eylemleri içerir.  
  
## <a name="basic-workflow-definition-structure"></a>Temel iş akışı tanımı yapısı

Bir iş akışı tanımı temel yapısını şöyledir:  
  
```json
{
    "$schema": "<schema-of the-definition>",
    "contentVersion": "<version-number-of-definition>",
    "parameters": { <parameter-definitions-of-definition> },
    "triggers": [ { <definition-of-flow-triggers> } ],
    "actions": [ { <definition-of-flow-actions> } ],
    "outputs": { <output-of-definition> }
}
```
  
> [!NOTE]
> [İş akışı yönetimi REST API](https://docs.microsoft.com/rest/api/logic/workflows) belgeleri oluşturmak ve mantıksal uygulama iş akışlarını yönetme konusunda bilgi vardır.
  
|Öğe adı|Gerekli|Açıklama|  
|------------------|--------------|-----------------|  
|$schema|Hayır|Tanım dili sürümü açıklanmaktadır JSON şema dosyası için konumu belirtir. Bu konum, bir tanım dışarıdan başvurduğunuzda gereklidir. Bu belge için konum şöyledir: <p>`https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json`|  
|contentVersion|Hayır|Tanım sürümü belirtir. Bir iş akışı tanımı kullanarak dağıttığınızda, doğru tanımı kullanıldığından emin olmak için bu değeri kullanabilirsiniz.|  
|parametreler|Hayır|Tanımı veri girişi için kullanılan parametreleri belirtir. En fazla 50 parametreleri tanımlanabilir.|  
|Tetikleyicileri|Hayır|İş akışını başlatmak Tetikleyicileri bilgilerini belirtir. En fazla 10 Tetikleyicileri tanımlanabilir.|  
|Eylemler|Hayır|Akış yürütür olarak gerçekleştirilen eylemler belirtir. En fazla 250 Eylemler tanımlanabilir.|  
|çıkışlar|Hayır|Dağıtılan kaynak hakkındaki bilgileri belirtir. En fazla 10 çıkışları tanımlanabilir.|  
  
## <a name="parameters"></a>Parametreler

Bu bölümde iş akışı tanımı dağıtım zamanında kullanılan tüm parametreleri belirtir. Tanımı diğer bölümlerinde kullanılabilmesi için önce bu bölümdeki tüm parametreleri bildirilmesi gerekir.  
  
Aşağıdaki örnek, bir parametrenin tanımını yapısını gösterir:  

```json
"parameters": {
    "<parameter-name>" : {
        "type" : "<type-of-parameter-value>",
        "defaultValue": <default-value-of-parameter>,
        "allowedValues": [ <array-of-allowed-values> ],
        "metadata" : { "key": { "name": "value"} }
    }
}
```

|Öğe adı|Gerekli|Açıklama|  
|------------------|--------------|-----------------|  
|type|Evet|**Tür**: dize <p> **Bildirim**: `"parameters": {"parameter1": {"type": "string"}}` <p> **Belirtimi**: `"parameters": {"parameter1": {"value": "myparamvalue1"}}` <p> **Tür**: securestring <p> **Bildirim**: `"parameters": {"parameter1": {"type": "securestring"}}` <p> **Belirtimi**: `"parameters": {"parameter1": {"value": "myparamvalue1"}}` <p> **Tür**: int <p> **Bildirim**: `"parameters": {"parameter1": {"type": "int"}}` <p> **Belirtimi**: `"parameters": {"parameter1": {"value" : 5}}` <p> **Tür**: bool <p> **Bildirim**: `"parameters": {"parameter1": {"type": "bool"}}` <p> **Belirtimi**: `"parameters": {"parameter1": { "value": true }}` <p> **Tür**: dizi <p> **Bildirim**: `"parameters": {"parameter1": {"type": "array"}}` <p> **Belirtimi**: `"parameters": {"parameter1": { "value": [ array-of-values ]}}` <p> **Tür**: nesnesi <p> **Bildirim**: `"parameters": {"parameter1": {"type": "object"}}` <p> **Belirtimi**: `"parameters": {"parameter1": { "value": { JSON-object } }}` <p> **Tür**: secureobject <p> **Bildirim**: `"parameters": {"parameter1": {"type": "object"}}` <p> **Belirtimi**: `"parameters": {"parameter1": { "value": { JSON-object } }}` <p> **Not:** `securestring` ve `secureobject` türleri alınmadı içinde `GET` işlemleri. Tüm parolalar, anahtarlar ve gizli anahtarları bu türünü kullanmanız gerekir.|  
|defaultValue|Hayır|Hiçbir değer Kaynak oluşturulduğunda belirtildiğinde parametresinin varsayılan değeri belirtir.|  
|allowedValues|Hayır|Parametre için izin verilen değerler dizisini belirtir.|  
|meta veriler|Hayır|Okunabilir açıklama veya Visual Studio veya diğer araçları tarafından kullanılan tasarım zamanı verileri gibi parametre hakkında ayrıntılı bilgileri belirtir.|  
  
Bu örnek, bir eylem gövde bölümünü bir parametre nasıl kullanabileceğinizi gösterir:  
  
```json
"body" :
{
  "property1": "@parameters('parameter1')"
}
```

 Parametreleri çıktılarında de kullanılabilir.  
  
## <a name="triggers-and-actions"></a>Tetikleyiciler ve eylemler  

Tetikleyiciler ve Eylemler katılabilir çağrıları iş akışı yürütme belirtin. Bu bölümde hakkında daha fazla ayrıntı için bkz: [iş akışı eylemleri ve Tetikleyicileri](logic-apps-workflow-actions-triggers.md).
  
## <a name="outputs"></a>Çıkışlar  

Çıkış çalıştıran bir iş akışından döndürülebilecek bilgilerini belirtin. Örneğin, bir özel durum ya da her çalıştırmak için izlemek istediğiniz değer varsa, bu verilerin çalışma çıktılarında içerebilir. Veri Yönetimi REST API için çalıştıran ve Azure portalında çalıştıran için kullanıcı Arabirimi yönetim görünür. Panoları oluşturmak için de bu çıktıları Powerbı gibi diğer dış sistemler akabilir. Çıkış olan *değil* hizmeti REST API'si gelen isteklerini yanıtlamak için kullanılır. Bir gelen talep kullanmaya yanıt `response` eylem türü örnek aşağıda verilmiştir:
  
```json
"outputs": {  
  "key1": {  
    "value": "value1",  
    "type" : "<type-of-value>"  
  }  
} 
```

|Öğe adı|Gerekli|Açıklama|  
|------------------|--------------|-----------------|  
|key1|Evet|Çıktı anahtar tanımlayıcısını belirtir. Değiştir **key1** çıkış tanımlamak için kullanmak istediğiniz bir adla.|  
|değer|Evet|Çıkış değerini belirtir.|  
|type|Evet|Belirtilen değer türünü belirtir. Olası değerler türleri şunlardır: <ul><li>`string`</li><li>`securestring`</li><li>`int`</li><li>`bool`</li><li>`array`</li><li>`object`</li></ul>|
  
## <a name="expressions"></a>İfadeler  

JSON değerler tanımında değişmez değer olabilir veya tanımı kullanıldığında değerlendirilen bir ifade olabilir. Örneğin:  
  
```json
"name": "value"
```

 or  
  
```json
"name": "@parameters('password') "
```

> [!NOTE]
> Bazı ifadeleri değerlerini yürütme başlangıcında varolmayabilir çalışma zamanı eylemlerine alırlar. Kullanabileceğiniz **işlevleri** bu değerlerden bazıları almak yardımcı olacak.  
  
İfadeler bir JSON dizesi değerindeki herhangi bir yerde görünür ve her zaman başka bir JSON değeri neden. Bir JSON değeri bir ifade olarak belirlendiğinde ifadesinin gövdesi oturum sırasında kaldırarak ayıklanan (@). Sabit değerli bir dize ile başlayan gerekliyse, @, dize kullanarak kaçış uygulanmalıdır@. Aşağıdaki örnekler, ifadeler nasıl değerlendirilir gösterir.  
  
|JSON değeri|Sonuç|  
|----------------|------------|  
|"parametreleri"|Karakter 'parameters' döndürülür.|  
|"parametreleri [1]"|Karakter 'parametreleri [1]' döndürülür.|  
|"@@"|İçeren bir 1 karakter dizesi ' @' döndürülür.|  
|" @"|İçeren bir 2 karakter dizesi ' @' döndürülür.|  
  
İle *dize ilişkilendirme*, ifadeler burada ifadeleri sarılır içinde dizeleri içinde de görüntülenebilir `@{ ... }`. Örneğin: <p>`"name" : "First Name: @{parameters('firstName')} Last Name: @{parameters('lastName')}"`

Bu özellik benzer kılar bir dize sonucudur her zaman `concat` işlevi. Tanımladığınız varsayalım `myNumber` olarak `42` ve `myString` olarak `sampleString`:  
  
|JSON değeri|Sonuç|  
|----------------|------------|  
|"@parameters('myString')"|Döndürür `sampleString` dize olarak.|  
|"@{parameters('myString')}"|Döndürür `sampleString` dize olarak.|  
|"@parameters('myNumber')"|Döndürür `42` olarak bir *numarası*.|  
|"@{parameters('myNumber')}"|Döndürür `42` olarak bir *dize*.|  
|"Yanıt: @{parameters('myNumber')}"|Bir dize döndürür `Answer is: 42`.|  
|"@concat(' Yanıt: ', string(parameters('myNumber')))"|Bir dize döndürür `Answer is: 42`|  
|"Yanıt: @@ {parameters('myNumber')}"|Bir dize döndürür `Answer is: @{parameters('myNumber')}`.|  
  
## <a name="operators"></a>İşleçler  

İşleçler ifadeler veya işlevler içinde kullanabileceğiniz karakterler olur. 
  
|İşleç|Açıklama|  
|--------------|-----------------|  
|.|Nokta işleci, nesnenin özelliklerini başvuru olanak sağlar|  
|?|Soru işareti işleci, null bir çalışma zamanı hatası olmadan bir nesnenin özelliklerini başvuru sağlar. Örneğin, bu ifade null tetikleyici çıkışları işlemek için kullanabilirsiniz: <p>`@coalesce(trigger().outputs?.body?.property1, 'my default value')`|  
|'|Tek tırnak işareti dize değişmez değerleri sarmalamak için tek yoludur. Bu noktalama tüm ifade sarmalar JSON teklifle çakıştığından, çift tırnak içine ifadeleri kullanamazsınız.|  
|[]|Köşeli ayraçlar, belirli bir dizine sahip bir dizi gelen bir değer almak için kullanılabilir. Örneğin, başarılı bir eylem varsa `range(0,10)`içinde için `forEach` işlevi, dizilerin öğeden almak için bu işlevi kullanabilirsiniz:  <p>`myArray[item()]`|  
  
## <a name="functions"></a>İşlevler  

Ayrıca, ifadeler işlevlerinde çağırabilirsiniz. Aşağıdaki tabloda kullanılabilir işlevler bir ifadede gösterir.  
  
|İfade|Değerlendirme|  
|----------------|----------------|  
|"@function('Hello')"|Değişmez değer dize Hello tanımıyla ilk parametre olarak işlevin üyesi çağırır.|  
|"@function(', '' S Cool!')"|Değişmez değer dize tanımıyla işlevi üyesi 'Cool kadar!' çağırır İlk parametre olarak|  
|"@function() .prop1"|Prop1 özelliğinin değerini döndürür `myfunction` tanımı üyesi.|  
|"@function('Hello').prop1"|Değişmez değer dize tanımıyla işlevi üyesi 'Hello' ilk parametre olarak çağırır ve nesnesinin prop1 özelliği döndürür.|  
|"@function(parameters('Hello'))"|Merhaba parametre değerlendirir ve değeri işleve geçirir|  
  
### <a name="referencing-functions"></a>İşlevler başvurma  

Mantıksal uygulama veya mantıksal uygulama oluşturulduğunda geçirilen değerleri diğer eylemlerine çıkışları başvurmak için bu işlevler kullanabilirsiniz. Örneğin, başka bir programda kullanmak için bir adım veri başvuruda bulunabilir.  
  
|İşlev adı|Açıklama|  
|-------------------|-----------------|  
|parametreler|Tanımında tanımlanan bir parametre değeri döndürür. <p>`parameters('password')` <p> **Numaralı parametre**: 1 <p> **Ad**: parametre <p> **Açıklama**: gerekli. Değerleri istediğiniz parametresinin adı.|  
|action|İfadenin değerini diğer JSON ad ve değer çiftleri ya da geçerli çalışma zamanı eylem çıktısını türetmemize olanak sağlar. Aşağıdaki örnekte propertyPath tarafından temsil edilen özelliği isteğe bağlıdır. PropertyPath belirtilmezse, tüm eylem nesnesine başvurudur. Bu işlev yalnızca içinde kullanılabilir-bir eylem koşullarını kadar. <p>`action().outputs.body.propertyPath`|  
|Eylemler|Bir ifadenin değerini diğer JSON ad ve değer çiftleri ya da çalışma zamanı eylem çıktısını türetilen sağlar. Bu ifadeler, açıkça bir eylem başka bir eyleme dayanan bildirin. Aşağıdaki örnekte propertyPath tarafından temsil edilen özelliği isteğe bağlıdır. PropertyPath belirtilmezse, tüm eylem nesnesine başvurudur. Bağımlılıkları belirtmek için bu öğeyi veya koşullar öğesini kullanabilirsiniz, ancak aynı bağımlı kaynak için her ikisini de kullanmanız gerekmez. <p>`actions('myAction').outputs.body.propertyPath` <p> **Numaralı parametre**: 1 <p> **Ad**: eylem adı <p> **Açıklama**: gerekli. Değerleri istediğiniz eylemin adı. <p> Eylem nesnesindeki kullanılabilir özellikleri şunlardır: <ul><li>`name`</li><li>`startTime`</li><li>`endTime`</li><li>`inputs`</li><li>`outputs`</li><li>`status`</li><li>`code`</li><li>`trackingId`</li><li>`clientTrackingId`</li></ul> <p>Bkz: [Rest API](http://go.microsoft.com/fwlink/p/?LinkID=850646) bu özellikleri hakkında ayrıntılı bilgi için.|
|Tetikleyici|İfadenin değerini diğer JSON ad ve değer çiftleri ya da çalışma zamanı tetikleyici çıktısını türetmemize olanak sağlar. Aşağıdaki örnekte propertyPath tarafından temsil edilen özelliği isteğe bağlıdır. PropertyPath belirtilmezse, tüm tetikleyici nesnesine başvurudur. <p>`trigger().outputs.body.propertyPath` <p>İçinde bir tetikleyicinin girişleri kullanıldığında, önceki yürütme çıkışları işlevi döndürür. Ancak, bir tetikleyici koşul içinde kullanıldığında `trigger` işlevi, geçerli yürütme çıkışları döndürür. <p> Tetikleyici nesne üzerinde kullanılabilir özellikleri şunlardır: <ul><li>`name`</li><li>`scheduledTime`</li><li>`startTime`</li><li>`endTime`</li><li>`inputs`</li><li>`outputs`</li><li>`status`</li><li>`code`</li><li>`trackingId`</li><li>`clientTrackingId`</li></ul> <p>Bkz: [Rest API](http://go.microsoft.com/fwlink/p/?LinkID=850644) bu özellikleri hakkında ayrıntılı bilgi için.|
|actionOutputs|Bu işlev için toplu özelliktir `actions('actionName').outputs` <p> **Numaralı parametre**: 1 <p> **Ad**: eylem adı <p> **Açıklama**: gerekli. Değerleri istediğiniz eylemin adı.|  
|actionBody ve gövde|Bu işlevler için kestirme olan `actions('actionName').outputs.body` <p> **Numaralı parametre**: 1 <p> **Ad**: eylem adı <p> **Açıklama**: gerekli. Değerleri istediğiniz eylemin adı.|  
|triggerOutputs|Bu işlev için toplu özelliktir `trigger().outputs`|  
|triggerBody|Bu işlev için toplu özelliktir `trigger().outputs.body`|  
|Öğesi|İçinde bir yinelenen eylemi kullanıldığında, bu işlev dizi eylemin bu yineleme için bulunduğu öğeyi döndürür. Örneğin, her öğe için bir dizi iletileri çalıştıran bir eylem varsa, bu söz dizimini kullanabilirsiniz: <p>`"input1" : "@item().subject"`| 
  
### <a name="collection-functions"></a>Toplama işlevleri  

Bu işlevler koleksiyonlar çalışır ve genellikle dizileri, dizeleri ve sözlükleri bazen uygulayın.  
  
|İşlev adı|Açıklama|  
|-------------------|-----------------|  
|içerir|Dize alt dizeyi içeren veya sözlük bir anahtar, listesi içeriyorsa true döndürür değeri içeriyor. Örneğin, bu işlevi döndürür `true`: <p>`contains('abacaba','aca')` <p> **Numaralı parametre**: 1 <p> **Ad**: koleksiyonundaki <p> **Açıklama**: gerekli. Aranacak koleksiyonu. <p> **Numaralı parametre**: 2 <p> **Ad**: Bul nesnesi <p> **Açıklama**: gerekli. İçinde bulunacak nesne **koleksiyonundaki**.|  
|uzunluğu|Bir dizi veya dize öğe sayısını döndürür. Örneğin, bu işlevi döndürür `3`:  <p>`length('abc')` <p> **Numaralı parametre**: 1 <p> **Ad**: koleksiyonu <p> **Açıklama**: gerekli. Uzunluğu almak üzere koleksiyonu.|  
|boş|Nesne, dizi veya dize boşsa, true döndürür. Örneğin, bu işlevi döndürür `true`: <p>`empty('')` <p> **Numaralı parametre**: 1 <p> **Ad**: koleksiyonu <p> **Açıklama**: gerekli. Boş olup olmadığını denetlemek için koleksiyonu.|  
|kesişim|Tek bir dizi veya dizi ya da geçirilen nesneleri arasında ortak öğeler olan nesneyi döndürür. Örneğin, bu işlevi döndürür `[1, 2]`: <p>`intersection([1, 2, 3], [101, 2, 1, 10],[6, 8, 1, 2])` <p>İşlev parametrelerini ya da nesne veya dizi (değil her ikisinin bir karışımıyla) kümesi olabilir. Aynı ada sahip iki nesne, bu adı taşıyan son nesne son nesnesinde görüntülenir. <p> **Numaralı parametre**: 1... *n* <p> **Ad**: koleksiyon *n* <p> **Açıklama**: gerekli. Değerlendirilecek koleksiyonları. Bir nesne sonuçta görünmesini geçirilen tüm koleksiyonlar olması gerekir.|  
|birleşim|Tek bir dizi veya nesne dizi veya nesne içinde bu işleve tüm öğeleri döndürür. Örneğin, bu işlevi döndürür `[1, 2, 3, 10, 101]`: <p>`union([1, 2, 3], [101, 2, 1, 10])` <p>İşlev parametrelerini ya da nesne veya dizi (olmayan bir karışımını bunların) kümesi olabilir. Son çıktıda aynı ada sahip iki nesne, bu adı taşıyan son nesne son nesnesinde görüntülenir. <p> **Numaralı parametre**: 1... *n* <p> **Ad**: koleksiyon *n* <p> **Açıklama**: gerekli. Değerlendirilecek koleksiyonları. Koleksiyonları hiçbirinde görünen bir nesnenin sonucunda da görüntülenir.|  
|ilk|Dizi ya da geçirilen dize ilk öğeyi döndürür. Örneğin, bu işlevi döndürür `0`: <p>`first([0,2,3])` <p> **Numaralı parametre**: 1 <p> **Ad**: koleksiyonu <p> **Açıklama**: gerekli. İlk nesneyi yapılacak koleksiyonu.|  
|Son|Dizi ya da geçirilen dize son öğeyi döndürür. Örneğin, bu işlevi döndürür `3`: <p>`last('0123')` <p> **Numaralı parametre**: 1 <p> **Ad**: koleksiyonu <p> **Açıklama**: gerekli. Son nesnesinden yapılacak koleksiyonu.|  
|Al|İlk döndürür **sayısı** dizisi veya dize öğelerinden geçirildi. Örneğin, bu işlevi döndürür `[1, 2]`:  <p>`take([1, 2, 3, 4], 2)` <p> **Numaralı parametre**: 1 <p> **Ad**: koleksiyonu <p> **Açıklama**: gerekli. İlk almak nereye koleksiyondan **sayısı** nesneleri. <p> **Numaralı parametre**: 2 <p> **Ad**: sayısı <p> **Açıklama**: gerekli. Gelen yapılacak nesne sayısı **koleksiyonu**. Pozitif bir tamsayı olmalıdır.|  
|Atla|Öğeleri dizinden başlayarak bir diziye döndürür **sayısı**. Örneğin, bu işlevi döndürür `[3, 4]`: <p>`skip([1, 2 ,3 ,4], 2)` <p> **Numaralı parametre**: 1 <p> **Ad**: koleksiyonu <p> **Açıklama**: gerekli. İlk atlamak için koleksiyon **sayısı** nesnelerin. <p> **Numaralı parametre**: 2 <p> **Ad**: sayısı <p> **Açıklama**: gerekli. Önüne kaldırmak için nesne sayısı **koleksiyonu**. Pozitif bir tamsayı olmalıdır.|  
|join|Örneğin bu döndürür bir sınırlayıcısıyla birleştirilmiş bir dizinin her bir öğesiyle bir dize döndürür `"1,2,3,4"`:<br /><br /> `join([1, 2, 3, 4], ',')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: koleksiyonu<br /><br /> **Açıklama**: gerekli. Öğelerden katılmaya koleksiyonu.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: sınırlayıcı<br /><br /> **Açıklama**: gerekli. Öğeleri ile sınırlandırmak için dize.|  
  
### <a name="string-functions"></a>Dize işlevleri

Aşağıdaki işlevleri yalnızca dizeleri için geçerlidir. Dizeleri bazı toplama işlevleri de kullanabilirsiniz.  
  
|İşlev adı|Açıklama|  
|-------------------|-----------------|  
|concat|Dizeleri herhangi bir sayıda birlikte birleştirir. Örneğin, 1 parametresi ise `p1`, bu işlevi döndürür `somevalue-p1-somevalue`: <p>`concat('somevalue-',parameters('parameter1'),'-somevalue')` <p> **Numaralı parametre**: 1... *n* <p> **Ad**: dize *n* <p> **Açıklama**: gerekli. Tek bir dize halinde birleştirmek için dizeleri.|  
|substring|Bir dizeden karakterleri kümesini döndürür. Örneğin, bu işlevi döndürür `abc`: <p>`substring('somevalue-abc-somevalue',10,3)` <p> **Numaralı parametre**: 1 <p> **Ad**: dize <p> **Açıklama**: gerekli. Alt dizeyi alındığı dizesi. <p> **Numaralı parametre**: 2 <p> **Ad**: Başlangıç dizini <p> **Açıklama**: gerekli. Alt dizeyi parametresi 1 başladığı dizin. <p> **Numaralı parametre**: 3 <p> **Ad**: uzunluğu <p> **Açıklama**: gerekli. Dizenin uzunluğu.|  
|Değiştir|Bir dizeyi belirli bir dizeyle değiştirir. Örneğin, bu işlevi döndürür `the new string`: <p>`replace('the old string', 'old', 'new')` <p> **Numaralı parametre**: 1 <p> **Ad**: dize <p> **Açıklama**: gerekli. Parametresi 2 için arama ve parametre 2 parametresi 1 bulunduğunda 3 parametreyle güncelleştirilen dize. <p> **Numaralı parametre**: 2 <p> **Ad**: eski dizesi <p> **Açıklama**: gerekli. Dize parametresi 1 bir eşleşme bulunduğunda 3 parametreyle değiştirmek için <p> **Numaralı parametre**: 3 <p> **Ad**: yeni bir dize <p> **Açıklama**: gerekli. 1 parametresinde bir eşleşme bulunduğunda parametresi 2 dizesini değiştirmek için kullanılan dize.|  
|GUID|Bu işlev, genel olarak benzersiz bir dize (GUID) oluşturur. Örneğin, bu işlev bu GUID oluşturabilirsiniz: `c2ecc88d-88c8-4096-912c-d6f2e2b138ce` <p>`guid()` <p> **Numaralı parametre**: 1 <p> **Ad**: biçimi <p> **Açıklama**: isteğe bağlı. Gösteren bir tek biçim belirticisi [bu GUID değeri biçimine](https://msdn.microsoft.com/library/97af8hh4%28v=vs.110%29.aspx). Format parametresi "N", "D", "B", "P" veya "X" olabilir. Biçim sağlanmazsa, "D" kullanılır.|  
|toLower|Bir dizeyi küçük harflere dönüştürür. Örneğin, bu işlevi döndürür `two by two is four`: <p>`toLower('Two by Two is Four')` <p> **Numaralı parametre**: 1 <p> **Ad**: dize <p> **Açıklama**: gerekli. Alt kasasını dönüştürmek için dize. Karakter döndürülen dize değiştirmeden, bir karakter dizesi, bir küçük harf eşdeğeri yoksa dahil edilir.|  
|toUpper|Bir dizeyi büyük harfe dönüştürür. Örneğin, bu işlevi döndürür `TWO BY TWO IS FOUR`: <p>`toUpper('Two by Two is Four')` <p> **Numaralı parametre**: 1 <p> **Ad**: dize <p> **Açıklama**: gerekli. Üst kasasını dönüştürmek için dize. Karakter döndürülen dize değiştirmeden, bir karakter dizesi, bir büyük harf eşdeğeri yoksa dahil edilir.|  
|IndexOf|Bir dize çalışması içinde bir değerle dizinini insensitively bulun. Örneğin, bu işlevi döndürür `7`: <p>`indexof('hello, world.', 'world')` <p> **Numaralı parametre**: 1 <p> **Ad**: dize <p> **Açıklama**: gerekli. Değer içerebilir dize. <p> **Numaralı parametre**: 2 <p> **Ad**: dize <p> **Açıklama**: gerekli. Dizini arama için değer.|  
|lastIndexOf|Bir dize çalışması arasında bir değer son dizinini insensitively bulun. Örneğin, bu işlevi döndürür `3`: <p>`lastindexof('foofoo', 'foo')` <p> **Numaralı parametre**: 1 <p> **Ad**: dize <p> **Açıklama**: gerekli. Değer içerebilir dize. <p> **Numaralı parametre**: 2 <p> **Ad**: dize <p> **Açıklama**: gerekli. Dizini arama için değer.|  
|startswith|Dize değeri durumuyla insensitively başlayıp başlamadığını denetler. Örneğin, bu işlevi döndürür `true`: <p>`startswith('hello, world', 'hello')` <p> **Numaralı parametre**: 1 <p> **Ad**: dize <p> **Açıklama**: gerekli. Değer içerebilir dize. <p> **Numaralı parametre**: 2 <p> **Ad**: dize <p> **Açıklama**: gerekli. Dize değeri ile başlayabilir.|  
|endswith|Dize değeri durumuyla insensitively bitip bitmediğini denetler. Örneğin, bu işlevi döndürür `true`: <p>`endswith('hello, world', 'world')` <p> **Numaralı parametre**: 1 <p> **Ad**: dize <p> **Açıklama**: gerekli. Değer içerebilir dize. <p> **Numaralı parametre**: 2 <p> **Ad**: dize <p> **Açıklama**: gerekli. Dize değeri ile bitebilir.|  
|split|Ayırıcı kullanarak dize böler. Örneğin, bu işlevi döndürür `["a", "b", "c"]`: <p>`split('a;b;c',';')` <p> **Numaralı parametre**: 1 <p> **Ad**: dize <p> **Açıklama**: gerekli. Bölünen dize. <p> **Numaralı parametre**: 2 <p> **Ad**: dize <p> **Açıklama**: gerekli. Ayırıcı.|  
  
### <a name="logical-functions"></a>Mantıksal işlevleri  

Bu işlevler içinde koşullar yararlıdır ve herhangi bir türde mantığı değerlendirmek için kullanılan.  
  
|İşlev adı|Açıklama|  
|-------------------|-----------------|  
|şuna eşittir:|İki değer eşitse true değerini döndürür. Örneğin, parametre1 someValue ise, bu işlev, döndürür `true`: <p>`equals(parameters('parameter1'), 'someValue')` <p> **Numaralı parametre**: 1 <p> **Ad**: 1 nesne <p> **Açıklama**: gerekli. Karşılaştırma yapılacak nesne **nesne 2**. <p> **Numaralı parametre**: 2 <p> **Ad**: 2 nesnesi <p> **Açıklama**: gerekli. Karşılaştırma yapılacak nesne **nesne 1**.|  
|daha az|İlk bağımsız değişken daha az ise true değeri döndürür ikinciden. Not, değerler yalnızca türü tamsayı, kayan noktalı sayı veya dize olabilir. Örneğin, bu işlevi döndürür `true`: <p>`less(10,100)` <p> **Numaralı parametre**: 1 <p> **Ad**: 1 nesne <p> **Açıklama**: gerekli. Olup olmadığını denetlemek için nesne değerinden **nesne 2**. <p> **Numaralı parametre**: 2 <p> **Ad**: 2 nesnesi <p> **Açıklama**: gerekli. Büyük olup olmadığını denetlemek için nesne **nesne 1**.|  
|lessOrEquals|İlk bağımsız değişken ikinci eşit veya daha az ise true, aksi durumda değeri döndürür. Not, değerler yalnızca türü tamsayı, kayan noktalı sayı veya dize olabilir. Örneğin, bu işlevi döndürür `true`: <p>`lessOrEquals(10,10)` <p> **Numaralı parametre**: 1 <p> **Ad**: 1 nesne <p> **Açıklama**: gerekli. Bu daha az olup olmadığını denetleyin veya eşit nesnesine **nesne 2**. <p> **Numaralı parametre**: 2 <p> **Ad**: 2 nesnesi <p> **Açıklama**: gerekli. Büyük veya eşit olup olmadığını denetlemek için nesne **nesne 1**.|  
|büyük|İlk bağımsız değişken saniyeden büyükse, true döndürür. Not, değerler yalnızca türü tamsayı, kayan noktalı sayı veya dize olabilir. Örneğin, bu işlevi döndürür `false`:  <p>`greater(10,10)` <p> **Numaralı parametre**: 1 <p> **Ad**: 1 nesne <p> **Açıklama**: gerekli. Büyük olup olmadığını denetlemek için nesne **nesne 2**. <p> **Numaralı parametre**: 2 <p> **Ad**: 2 nesnesi <p> **Açıklama**: gerekli. Olup olmadığını denetlemek için nesne değerinden **nesne 1**.|  
|greaterOrEquals|İlk bağımsız değişkeni sıfırdan büyük veya ona eşit olduğunda true değerini döndürür. Not, değerler yalnızca türü tamsayı, kayan noktalı sayı veya dize olabilir. Örneğin, bu işlevi döndürür `false`: <p>`greaterOrEquals(10,100)` <p> **Numaralı parametre**: 1 <p> **Ad**: 1 nesne <p> **Açıklama**: gerekli. Büyük veya eşit olup olmadığını denetlemek için nesne **nesne 2**. <p> **Numaralı parametre**: 2 <p> **Ad**: 2 nesnesi <p> **Açıklama**: gerekli. Bu küçük veya eşit olup olmadığını denetlemek için nesne **nesne 1**.|  
|ve|Parametrelerinin her ikisini de doğruysa, true döndürür. Her iki değişken Boole değerlerini olması gerekir. Örneğin, bu işlevi döndürür `false`: <p>`and(greater(1,10),equals(0,0))` <p> **Numaralı parametre**: 1 <p> **Ad**: Boolean 1 <p> **Açıklama**: gerekli. Gereken ilk bağımsız değişken `true`. <p> **Numaralı parametre**: 2 <p> **Ad**: Boolean 2 <p> **Açıklama**: gerekli. İkinci bağımsız değişkeni olmalıdır `true`.|  
|or|Her iki parametre true olduğunda true değerini döndürür. Her iki değişken Boole değerlerini olması gerekir. Örneğin, bu işlevi döndürür `true`: <p>`or(greater(1,10),equals(0,0))` <p> **Numaralı parametre**: 1 <p> **Ad**: Boolean 1 <p> **Açıklama**: gerekli. Olabilir ilk bağımsız değişken `true`. <p> **Numaralı parametre**: 2 <p> **Ad**: Boolean 2 <p> **Açıklama**: gerekli. İkinci bağımsız değişkeni olabilir `true`.|  
|değil|Parametreler, true döndürür `false`. Her iki değişken Boole değerlerini olması gerekir. Örneğin, bu işlevi döndürür `true`: <p>`not(contains('200 Success','Fail'))` <p> **Numaralı parametre**: 1 <p> **Ad**: Boolean <p> **Açıklama**: parametreler varsa true değerini döndürür `false`. Her iki değişken Boole değerlerini olması gerekir. Bu işlev, döndürür `true`:  `not(contains('200 Success','Fail'))`|  
|if|Olup olmadığını ifade ile sonuçlandı üzerinde temel belirtilen değeri döndüren `true` veya `false`.  Örneğin, bu işlevi döndürür `"yes"`: <p>`if(equals(1, 1), 'yes', 'no')` <p> **Numaralı parametre**: 1 <p> **Ad**: ifade <p> **Açıklama**: gerekli. Hangi değerini belirleyen bir boolean değeri ifade döndürmelidir. <p> **Numaralı parametre**: 2 <p> **Ad**: True <p> **Açıklama**: gerekli. İfade ise döndürülecek değer `true`. <p> **Numaralı parametre**: 3 <p> **Ad**: yanlış <p> **Açıklama**: gerekli. İfade ise döndürülecek değer `false`.|  
  
### <a name="conversion-functions"></a>Dönüşüm işlevleri  

Bu işlevlerin her dil içindeki yerel türler arasında dönüştürme için kullanılır:  
  
- string  
  
- integer  
  
- Kayan nokta  
  
- boole  
  
- Diziler  
  
- Sözlük  

-   Formlar  
  
|İşlev adı|Açıklama|  
|-------------------|-----------------|  
|Int|Parametresi, bir tamsayıya dönüştürür. Örneğin, bu işlev bir dize yerine bir sayı olarak 100 döndürür: <p>`int('100')` <p> **Numaralı parametre**: 1 <p> **Ad**: değer <p> **Açıklama**: gerekli. Bir tamsayıya dönüştürülüp değeri.|  
|string|Parametresi bir dizeye dönüştürün. Örneğin, bu işlevi döndürür `'10'`: <p>`string(10)` <p>Ayrıca, bir nesne bir dizeye dönüştürebilirsiniz. Örneğin, varsa `myPar` parametredir bir özelliği olan bir nesne `abc : xyz`, bu işlevi döndürür sonra `{"abc" : "xyz"}`: <p>`string(parameters('myPar'))` <p> **Numaralı parametre**: 1 <p> **Ad**: değer <p> **Açıklama**: gerekli. Bir dizeye dönüştürülen değer.|  
|JSON|Parametresi bir JSON türü değerine dönüştürür ve tersidir `string()`. Örneğin, bu işlevi döndürür `[1,2,3]` bir dize yerine bir dizi olarak: <p>`json('[1,2,3]')` <p>Benzer şekilde, bir nesneye bir dize dönüştürebilirsiniz. Örneğin, bu işlevi döndürür `{ "abc" : "xyz" }`: <p>`json('{"abc" : "xyz"}')` <p> **Numaralı parametre**: 1 <p> **Ad**: dize <p> **Açıklama**: gerekli. Yerel tür değerine dönüştürülüp dize. <p>`json()` İşlevi çok giriş XML destekler. Örneğin, parametre değeri: <p>`<?xml version="1.0"?> <root>   <person id='1'>     <name>Alan</name>     <occupation>Engineer</occupation>   </person> </root>` <p>Bu JSON biçiminde seri hale dönüştürülür: <p>`{ "?xml": { "@version": "1.0" },   "root": {     "person": [     {       "@id": "1",       "name": "Alan",       "occupation": "Engineer"     }   ]   } }`|  
|Kayan nokta|Parametre bağımsız değişkeni bir kayan nokta sayıya dönüştürün. Örneğin, bu işlevi döndürür `10.333`: <p>`float('10.333')` <p> **Numaralı parametre**: 1 <p> **Ad**: değer <p> **Açıklama**: gerekli. Kayan nokta sayıya dönüştürülmüş değeri.|  
|bool|Parametre bir Boole değeri dönüştürün. Örneğin, bu işlevi döndürür `false`: <p>`bool(0)` <p> **Numaralı parametre**: 1 <p> **Ad**: değer <p> **Açıklama**: gerekli. Bir Boole değeri dönüştürülen değer.|  
|Base64|Giriş dizesi base64 gösterimini döndürür. Örneğin, bu işlevi döndürür `c29tZSBzdHJpbmc=`: <p>`base64('some string')` <p> **Numaralı parametre**: 1 <p> **Ad**: Dize 1 <p> **Açıklama**: gerekli. Base64 gösterimine kodlanacak dize.|  
|base64ToBinary|Bir base64 kodlu dize ikili bir gösterimini döndürür. Örneğin, bu işlev ikili gösterimini döndürür `some string`: <p>`base64ToBinary('c29tZSBzdHJpbmc=')` <p> **Numaralı parametre**: 1 <p> **Ad**: dize <p> **Açıklama**: gerekli. Base64 ile kodlanmış dize.|  
|base64ToString|Kodlanmış based64 dize dize gösterimini döndürür. Örneğin, bu işlevi döndürür `some string`: <p>`base64ToString('c29tZSBzdHJpbmc=')` <p> **Numaralı parametre**: 1 <p> **Ad**: dize <p> **Açıklama**: gerekli. Base64 ile kodlanmış dize.|  
|İkili|Değer ikili bir gösterimini döndürür.  Örneğin, bu işlev bir ikili biçimi döndürür `some string`: <p>`binary('some string')` <p> **Numaralı parametre**: 1 <p> **Ad**: değer <p> **Açıklama**: gerekli. İkili dosya dönüştürülen değer.|  
|dataUriToBinary|Bir ikili veri URI'si gösterimini döndürür. Örneğin, bu işlev ikili gösterimini döndürür `some string`: <p>`dataUriToBinary('data:;base64,c29tZSBzdHJpbmc=')` <p> **Numaralı parametre**: 1 <p> **Ad**: dize <p> **Açıklama**: gerekli. Veriler ikili gösterimine dönüştürmek için URI.|  
|dataUriToString|Bir veri URI dize gösterimini döndürür. Örneğin, bu işlevi döndürür `some string`: <p>`dataUriToString('data:;base64,c29tZSBzdHJpbmc=')` <p> **Numaralı parametre**: 1 <p> **Ad**: dize<p> **Açıklama**: gerekli. Verileri dize gösterimi dönüştürmek için URI.|  
|dataUri|Bir veri URI değeri döndürür. Örneğin, bu işlev, bu verileri URI döndürür `text/plain;charset=utf8;base64,c29tZSBzdHJpbmc=`: <p>`dataUri('some string')` <p> **Numaralı parametre**: 1<p> **Ad**: değer<p> **Açıklama**: gerekli. Veri URI'si dönüştürülecek değer.|  
|decodeBase64|Bir giriş based64 dize dize gösterimini döndürür. Örneğin, bu işlevi döndürür `some string`: <p>`decodeBase64('c29tZSBzdHJpbmc=')` <p> **Numaralı parametre**: 1 <p> **Ad**: dize <p> **Açıklama**: bir giriş based64 dize dize gösterimini döndürür.|  
|Encodeurıcomponent|URL çıkışları geçirilen dize. Örneğin, bu işlevi döndürür `You+Are%3ACool%2FAwesome`: <p>`encodeUriComponent('You Are:Cool/Awesome')` <p> **Numaralı parametre**: 1 <p> **Ad**: dize <p> **Açıklama**: gerekli. URL güvenli olmayan karakterler kaçınmak için dize.|  
|Decodeurıcomponent|Kaydını URL çıkışları geçirilen dize. Örneğin, bu işlevi döndürür `You Are:Cool/Awesome`: <p>`decodeUriComponent('You+Are%3ACool%2FAwesome')` <p> **Numaralı parametre**: 1 <p> **Ad**: dize <p> **Açıklama**: gerekli. URL güvenli olmayan karakterler çözecek dizesi.|  
|decodeDataUri|İkili bir giriş verileri URI dize gösterimini döndürür. Örneğin, bu işlev ikili gösterimini döndürür `some string`: <p>`decodeDataUri('data:;base64,c29tZSBzdHJpbmc=')` <p> **Numaralı parametre**: 1 <p> **Ad**: dize <p> **Açıklama**: gerekli. İkili gösterimine çözecek dataURI.|  
|uriComponent|Bir URI döndürür değerinin gösterimiyle kodlanmış. Örneğin, bu işlevi döndürür `You+Are%3ACool%2FAwesome`: <p>`uriComponent('You Are:Cool/Awesome')` <p> **Numaralı parametre**: 1<p> **Ad**: dize <p> **Açıklama**: gerekli. URI ile kodlanan olmasını dizesi.|  
|uriComponentToBinary|Bir URI ikili gösterimidir kodlanmış dizesi döndürür. Örneğin, bu işlev bir ikili biçimi döndürür `You Are:Cool/Awesome`: <p>`uriComponentToBinary('You+Are%3ACool%2FAwesome')` <p> **Numaralı parametre**: 1 <p> **Ad**: dize<p> **Açıklama**: gerekli. URI kodlanmış dize.|  
|uriComponentToString|Bir URI dize gösterimini kodlanmış dizesi döndürür. Örneğin, bu işlevi döndürür `You Are:Cool/Awesome`: <p>`uriComponentToString('You+Are%3ACool%2FAwesome')` <p> **Numaralı parametre**: 1<p> **Ad**: dize<p> **Açıklama**: gerekli. URI kodlanmış dize.|  
|xml|Değerinin XML gösterimini döndürür. Örneğin, bu işlev XML tarafından temsil edilen içerik döndürür `'\<name>Alan\</name>'`: <p>`xml('\<name>Alan\</name>')` <p>`xml()` İşlevi çok giriş JSON nesnesi destekler. Örneğin, parametre `{ "abc": "xyz" }` XML içeriği dönüştürülür: `\<abc>xyz\</abc>` <p> **Numaralı parametre**: 1<p> **Ad**: değer<p> **Açıklama**: gerekli. XML biçimine dönüştürülecek değer.|  
|array|Parametresi bir diziye dönüştürür. Örneğin, bu işlevi döndürür `["abc"]`: <p>`array('abc')` <p> **Numaralı parametre**: 1 <p> **Ad**: değer <p> **Açıklama**: gerekli. Bir dizi dönüştürülen değer.|
|createArray|Bir dizi parametrelerinden oluşturur. Örneğin, bu işlevi döndürür `["a", "c"]`: <p>`createArray('a', 'c')` <p> **Numaralı parametre**: 1... *n* <p> **Ad**: tüm *n* <p> **Açıklama**: gerekli. Bir diziye birleştirmek için kullanılan değerler.|
|triggerFormDataValue|Form verileri veya form kodlu tetikleyici çıkış anahtar adıyla eşleşen tek bir değer döndürür.  Varsa birden çok işlem hata eşleşir.  Örneğin, aşağıdaki döndürülecek `bar`: `triggerFormDataValue('foo')`<br /><br />**Numaralı parametre**: 1<br /><br />**Ad**: anahtar adı<br /><br />**Açıklama**: gerekli. Döndürülecek form veri değeri anahtar adı.|
|triggerFormDataMultiValues|Form verileri veya form kodlu tetikleyici çıkış anahtar adıyla eşleşen değerleri dizisi döndürür.  Örneğin, aşağıdaki döndürülecek `["bar"]`: `triggerFormDataValue('foo')`<br /><br />**Numaralı parametre**: 1<br /><br />**Ad**: anahtar adı<br /><br />**Açıklama**: gerekli. Döndürülecek form veri değerleri anahtar adı.|
|triggerMultipartBody|Gövde bölümünün çok bölümlü tetikleyici çıktısında döndürür.<br /><br />**Numaralı parametre**: 1<br /><br />**Ad**: dizin<br /><br />**Açıklama**: gerekli. Alınacak bölümü dizini.|
|formDataValue|Form verileri veya form kodlu eylem çıkışı anahtar adıyla eşleşen tek bir değer döndürür.  Varsa birden çok işlem hata eşleşir.  Örneğin, aşağıdaki döndürülecek `bar`: `formDataValue('someAction', 'foo')`<br /><br />**Numaralı parametre**: 1<br /><br />**Ad**: eylem adı<br /><br />**Açıklama**: gerekli. Form verileri veya form kodlu yanıt olan eylem adı.<br /><br />**Numaralı parametre**: 2<br /><br />**Ad**: anahtar adı<br /><br />**Açıklama**: gerekli. Döndürülecek form veri değeri anahtar adı.|
|formDataMultiValues|Form verileri veya form kodlu eylem çıkışı anahtar adıyla eşleşen değerleri dizisi döndürür.  Örneğin, aşağıdaki döndürülecek `["bar"]`: `formDataMultiValues('someAction', 'foo')`<br /><br />**Numaralı parametre**: 1<br /><br />**Ad**: eylem adı<br /><br />**Açıklama**: gerekli. Form verileri veya form kodlu yanıt olan eylem adı.<br /><br />**Numaralı parametre**: 2<br /><br />**Ad**: anahtar adı<br /><br />**Açıklama**: gerekli. Döndürülecek form veri değerleri anahtar adı.|
|multipartBody|Gövde bölümünün çok bölümlü bir eylem çıktısında döndürür.<br /><br />**Numaralı parametre**: 1<br /><br />**Ad**: eylem adı<br /><br />**Açıklama**: gerekli. Çok bölümlü bir yanıt olan eylem adı.<br /><br />**Numaralı parametre**: 2<br /><br />**Ad**: dizin<br /><br />**Açıklama**: gerekli. Alınacak bölümü dizini.|

### <a name="manipulation-functions"></a>İşleme işlevleri
 
Bu işlevler, XML ve nesneler için geçerlidir.
 
|İşlev adı|Açıklama|  
|-------------------|-----------------| 
|birleşim|Geçirilen bağımsız değişkenleri ilk null olmayan nesne döndürür. **Not**: boş bir dize null değil. Örneğin, 1 ve 2 parametreleri tanımlanmazsa, bu işlev, döndürür `fallback`:  <p>`coalesce(parameters('parameter1'), parameters('parameter2') ,'fallback')` <p> **Numaralı parametre**: 1... *n* <p> **Ad**: nesne*n* <p> **Açıklama**: gerekli. Null denetlemek için nesneleri.|
|addProperty|Bir ek özelliğine sahip bir nesne döndürür. Çalışma zamanında özelliği zaten varsa bir hata oluşturulur. Örneğin, bu işlev nesnesi döndürür `{ "abc" : "xyz", "def": "uvw" }`: <p>`addProperty(json('{"abc" : "xyz"}'), 'def', 'uvw')` <p> **Numaralı parametre**: 1 <p> **Ad**: nesnesi <p> **Açıklama**: gerekli. Yeni bir özellik eklenecek nesne. <p> **Numaralı parametre**: 2 <p> **Ad**: özellik adı <p> **Açıklama**: gerekli. Yeni özellik adı. <p> **Numaralı parametre**: 3 <p> **Ad**: değer <p> **Açıklama**: gerekli. Yeni özelliğine atanacak değer.|
|setProperty|Bir ek özellik veya belirtilen değere ayarlayın mevcut bir özellik olan bir nesne döndürür. Örneğin, bu işlev nesnesi döndürür `{ "abc" : "uvw" }`: <p>`setProperty(json('{"abc" : "xyz"}'), 'abc', 'uvw')` <p> **Numaralı parametre**: 1 <p> **Ad**: nesnesi <p> **Açıklama**: gerekli. Özelliği ayarlamak üzere nesne.<p> **Numaralı parametre**: 2 <p> **Ad**: özellik adı<p> **Açıklama**: gerekli. Yeni veya var olan özellik adı. <p> **Numaralı parametre**: 3 <p> **Ad**: değer <p> **Açıklama**: gerekli. Özelliğine atanacak değer.|
|removeProperty|Kaldırılan bir özelliğe sahip bir nesne döndürür. Kaldırmak için özelliği yoksa, özgün nesne döndürülür. Örneğin, bu işlev nesnesi döndürür `{ "abc" : "xyz" }`: <p>`removeProperty(json('{"abc" : "xyz", "def": "uvw"}'), 'def')` <p> **Numaralı parametre**: 1 <p> **Ad**: nesnesi <p> **Açıklama**: gerekli. Özelliğinden kaldırılacak nesne.<p> **Numaralı parametre**: 2 <p> **Ad**: özellik adı <p> **Açıklama**: gerekli. Kaldırılacak özelliğin adı. <p>|
|XPath|Xpath ifadesi xpath ifadesi değerlendiren bir değeri ile eşleşen XML düğümleri bir dizi döndürür. <p> **Örnek 1** <p>Parametresinin değeri varsayın `p1` bu XML dize gösterimi ise: <p>`<?xml version="1.0"?> <lab>   <robot>     <parts>5</parts>     <name>R1</name>   </robot>   <robot>     <parts>8</parts>     <name>R2</name>   </robot> </lab>` <p>Bu kod: `xpath(xml(parameters('p1')), '/lab/robot/name')` <p>döndürür <p>`[ <name>R1</name>, <name>R2</name> ]` <p>Bu kodu: <p>`xpath(xml(parameters('p1')), ' sum(/lab/robot/parts)')` <p>döndürür <p>`13` <p> <p> **Örnek 2** <p>Aşağıdaki XML içeriğini verilen: <p>`<?xml version="1.0"?> <File xmlns="http://foo.com">   <Location>bar</Location> </File>` <p>Bu kod: `@xpath(xml(body('Http')), '/*[name()=\"File\"]/*[name()=\"Location\"]')` <p>veya bu kod: <p>`@xpath(xml(body('Http')), '/*[local-name()=\"File\" and namespace-uri()=\"http://foo.com\"]/*[local-name()=\"Location\" and namespace-uri()=\"\"]')` <p>döndürür <p>`<Location xmlns="http://abc.com">xyz</Location>` <p>Ve bu kodu: `@xpath(xml(body('Http')), 'string(/*[name()=\"File\"]/*[name()=\"Location\"])')` <p>döndürür <p>``xyz`` <p> **Numaralı parametre**: 1 <p> **Ad**: Xml <p> **Açıklama**: gerekli. XPath ifadesi değerlendirileceği XML. <p> **Numaralı parametre**: 2 <p> **Ad**: XPath <p> **Açıklama**: gerekli. Değerlendirilecek XPath ifadesi.|

### <a name="math-functions"></a>Matematik işlevleri  

Bu işlevlerin her iki tür numarası için kullanılabilir: **tamsayılar** ve **gezinen**.  
  
|İşlev adı|Açıklama|  
|-------------------|-----------------|  
|ekle|İki sayı ekleme sonucunu döndürür. Örneğin, bu işlevi döndürür `20.333`: <p>`add(10,10.333)` <p> **Numaralı parametre**: 1 <p> **Ad**: Summand 1 <p> **Açıklama**: gerekli. Eklemek üzere numarasını **Summand 2**. <p> **Numaralı parametre**: 2 <p> **Ad**: Summand 2 <p> **Açıklama**: gerekli. Eklemek üzere numarasını **Summand 1**.|  
|Sub|İki sayı çıkarılmasıyla sonucunu döndürür. Örneğin, bu işlevi döndürür `-0.333`: <p>`sub(10,10.333)` <p> **Numaralı parametre**: 1 <p> **Ad**: Minuend <p> **Açıklama**: gerekli. Sayı, **Subtrahend** kaldırılır. <p> **Numaralı parametre**: 2 <p> **Ad**: Subtrahend <p> **Açıklama**: gerekli. Kaldırmak üzere numarasını **Minuend**.|  
|mul|İki sayının çarparak sonucunu döndürür. Örneğin, bu işlevi döndürür `103.33`: <p>`mul(10,10.333)` <p> **Numaralı parametre**: 1 <p> **Ad**: Multiplicand 1 <p> **Açıklama**: gerekli. Çarp üzere numarasını **Multiplicand 2** ile. <p> **Numaralı parametre**: 2 <p> **Ad**: Multiplicand 2 <p> **Açıklama**: gerekli. Çarp üzere numarasını **Multiplicand 1** ile.|  
|div|İki sayının sonucunu döndürür. Örneğin, bu işlevi döndürür `1.0333`: <p>`div(10.333,10)` <p> **Numaralı parametre**: 1 <p> **Ad**: bölünen <p> **Açıklama**: gerekli. Sayı bölün **bölen**. <p> **Numaralı parametre**: 2 <p> **Ad**: bölen <p> **Açıklama**: gerekli. Bölmek için numarası **bölünen** tarafından.|  
|mod|(Modül) iki sayı bölen sonra kalanı döndürür. Örneğin, bu işlevi döndürür `2`: <p>`mod(10,4)` <p> **Numaralı parametre**: 1 <p> **Ad**: bölünen <p> **Açıklama**: gerekli. Sayı bölün **bölen**. <p> **Numaralı parametre**: 2 <p> **Ad**: bölen <p> **Açıklama**: gerekli. Bölmek için numarası **bölünen** tarafından. Ayırmadan sonra kalan alınır.|  
|dk|Bu işlevi çağırmak için iki farklı modeli vardır. <p>Burada `min` bir dizi alır ve işlevi döndürür `0`: <p>`min([0,1,2])` <p>Alternatif olarak, bu işlev virgülle ayrılmış bir liste değerleri ve ayrıca döndürür sürebilir `0`: <p>`min(0,1,2)` <p> **Not**: tüm değerleri sayı olması gerekir böylece parametresi bir dizi ise dizinin yalnızca rakam olmalıdır. <p> **Numaralı parametre**: 1 <p> **Ad**: koleksiyon veya değeri <p> **Açıklama**: gerekli. Ya da bir dizi en küçük değerini bulmak için değerleri ya da bir kümenin ilk değer. <p> **Numaralı parametre**: 2... *n* <p> **Ad**: değer *n* <p> **Açıklama**: isteğe bağlı. İlk parametre bir değer ise, ek değerleri geçirmek ve geçirilen tüm değerlerin minimum döndürülür.|  
|en çok|Bu işlevi çağırmak için iki farklı modeli vardır. <p>Burada `max` bir dizi alır ve işlevi döndürür `2`: <p>`max([0,1,2])` <p>Alternatif olarak, bu işlev virgülle ayrılmış bir liste değerleri ve ayrıca döndürür sürebilir `2`: <p>`max(0,1,2)` <p> **Not**: tüm değerleri sayı olması gerekir böylece parametresi bir dizi ise dizinin yalnızca rakam olmalıdır. <p> **Numaralı parametre**: 1 <p> **Ad**: koleksiyon veya değeri <p> **Açıklama**: gerekli. Ya da bir dizi en büyük değeri bulmak için değerleri ya da bir kümenin ilk değer. <p> **Numaralı parametre**: 2... *n* <p> **Ad**: değer *n* <p> **Açıklama**: isteğe bağlı. İlk parametre bir değer ise, ek değerleri geçirmek ve maksimum tüm geçirilen değeri döndürülür.|  
|Aralık|Belirli bir numarasından başlayarak dizisi oluşturur. Döndürülen dizi uzunluğu, tanımlayın. <p>Örneğin, bu işlevi döndürür `[3,4,5,6]`: <p>`range(3,4)` <p> **Numaralı parametre**: 1 <p> **Ad**: Başlangıç dizini <p> **Açıklama**: gerekli. Dizideki ilk tamsayı. <p> **Numaralı parametre**: 2 <p> **Ad**: sayısı <p> **Açıklama**: gerekli. Bu değer dizideki olan tamsayılar sayısıdır.|  
|rand|(Yalnızca ilk ucunda dahil) belirtilen aralıkta rasgele bir tamsayı oluşturur. Örneğin, bu işlev ya da döndürebilirsiniz `0` veya '1': <p>`rand(0,2)` <p> **Numaralı parametre**: 1 <p> **Ad**: en az <p> **Açıklama**: gerekli. Döndürülebilecek en küçük tamsayı. <p> **Numaralı parametre**: 2 <p> **Ad**: en fazla <p> **Açıklama**: gerekli. Bu değeri sonraki döndürülebilen en büyük tamsayıyı sonra tamsayıdır.|  
 
### <a name="date-functions"></a>Date işlevleri  

|İşlev adı|Açıklama|  
|-------------------|-----------------|  
|UtcNow|Geçerli zaman damgası örneğin bir dize döndürür: `2017-03-15T13:27:36Z`: <p>`utcnow()` <p> **Numaralı parametre**: 1 <p> **Ad**: biçimi <p> **Açıklama**: isteğe bağlı. Ya da bir [tek bir biçim belirticisi karakter](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) veya [özel biçim deseni](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) bu zaman damgası değeri biçimine gösterir. Biçim sağlanmazsa, ISO 8601 biçiminde ("o") kullanılır.|  
|saniyeEkle|Bir tamsayı saniyeyi geçirilen dize zaman damgası ekler. Saniye sayısı pozitif veya negatif olabilir. Bir biçim belirticisi sağlanmadığı sürece varsayılan olarak, bir dize ("o"), ISO 8601 biçiminde sonucudur. Örneğin: `2015-03-15T13:27:00Z`: <p>`addseconds('2015-03-15T13:27:36Z', -36)` <p> **Numaralı parametre**: 1 <p> **Ad**: zaman damgası <p> **Açıklama**: gerekli. Saati içeren bir dize. <p> **Numaralı parametre**: 2 <p> **Ad**: saniye <p> **Açıklama**: gerekli. Eklemek için saniye sayısı. Saniye çıkartılacak negatif olabilir. <p> **Numaralı parametre**: 3 <p> **Ad**: biçimi <p> **Açıklama**: isteğe bağlı. Ya da bir [tek bir biçim belirticisi karakter](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) veya [özel biçim deseni](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) bu zaman damgası değeri biçimine gösterir. Biçim sağlanmazsa, ISO 8601 biçiminde ("o") kullanılır.|  
|AddMinutes|Bir dakika sayısıyla geçirilen dize zaman damgası ekler. Dakika sayısını olumlu veya olumsuz olabilir. Bir biçim belirticisi sağlanmadığı sürece varsayılan olarak, bir dize ("o"), ISO 8601 biçiminde sonucudur. Örneğin: `2015-03-15T14:00:36Z`: <p>`addminutes('2015-03-15T13:27:36Z', 33)` <p> **Numaralı parametre**: 1 <p> **Ad**: zaman damgası <p> **Açıklama**: gerekli. Saati içeren bir dize. <p> **Numaralı parametre**: 2 <p> **Ad**: dakika <p> **Açıklama**: gerekli. Eklemek için dakika sayısı. Dakika çıkartılacak negatif olabilir. <p> **Numaralı parametre**: 3 <p> **Ad**: biçimi <p> **Açıklama**: isteğe bağlı. Ya da bir [tek bir biçim belirticisi karakter](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) veya [özel biçim deseni](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) bu zaman damgası değeri biçimine gösterir. Biçim sağlanmazsa, ISO 8601 biçiminde ("o") kullanılır.|  
|addhours|Bir tamsayı saat geçirilen dize zaman damgası ekler. Saat sayısı pozitif veya negatif olabilir. Bir biçim belirticisi sağlanmadığı sürece varsayılan olarak, bir dize ("o"), ISO 8601 biçiminde sonucudur. Örneğin: `2015-03-16T01:27:36Z`: <p>`addhours('2015-03-15T13:27:36Z', 12)` <p> **Numaralı parametre**: 1 <p> **Ad**: zaman damgası <p> **Açıklama**: gerekli. Saati içeren bir dize. <p> **Numaralı parametre**: 2 <p> **Ad**: saat <p> **Açıklama**: gerekli. Eklemek için saat sayısı. Saat çıkartılacak negatif olabilir. <p> **Numaralı parametre**: 3 <p> **Ad**: biçimi <p> **Açıklama**: isteğe bağlı. Ya da bir [tek bir biçim belirticisi karakter](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) veya [özel biçim deseni](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) bu zaman damgası değeri biçimine gösterir. Biçim sağlanmazsa, ISO 8601 biçiminde ("o") kullanılır.|  
|adddays|Geçirilen bir dize zaman damgası gün tamsayı sayısı ekler. Gün sayısı pozitif veya negatif olabilir. Bir biçim belirticisi sağlanmadığı sürece varsayılan olarak, bir dize ("o"), ISO 8601 biçiminde sonucudur. Örneğin: `2015-03-13T13:27:36Z`: <p>`adddays('2015-03-15T13:27:36Z', -2)` <p> **Numaralı parametre**: 1 <p> **Ad**: zaman damgası <p> **Açıklama**: gerekli. Saati içeren bir dize. <p> **Numaralı parametre**: 2 <p> **Ad**: gün <p> **Açıklama**: gerekli. Eklenecek gün sayısı. Gün çıkartılacak negatif olabilir. <p> **Numaralı parametre**: 3 <p> **Ad**: biçimi <p> **Açıklama**: isteğe bağlı. Ya da bir [tek bir biçim belirticisi karakter](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) veya [özel biçim deseni](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) bu zaman damgası değeri biçimine gösterir. Biçim sağlanmazsa, ISO 8601 biçiminde ("o") kullanılır.|  
|FormatDateTime|Tarih biçiminde bir dize döndürür. Bir biçim belirticisi sağlanmadığı sürece varsayılan olarak, bir dize ("o"), ISO 8601 biçiminde sonucudur. Örneğin: `2015-03-15T13:27:36Z`: <p>`formatDateTime('2015-03-15T13:27:36Z', 'o')` <p> **Numaralı parametre**: 1 <p> **Ad**: tarih <p> **Açıklama**: gerekli. Tarih içeren bir dize. <p> **Numaralı parametre**: 2 <p> **Ad**: biçimi <p> **Açıklama**: isteğe bağlı. Ya da bir [tek bir biçim belirticisi karakter](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) veya [özel biçim deseni](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) bu zaman damgası değeri biçimine gösterir. Biçim sağlanmazsa, ISO 8601 biçiminde ("o") kullanılır.|  
|startOfHour|Geçirilen bir dize zaman damgası için saatin başlangıcını döndürür. Örneğin `2017-03-15T13:00:00Z`:<br /><br /> `startOfHour('2017-03-15T13:27:36Z')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: zaman damgası<br /><br /> **Açıklama**: gerekli. Bu saati içeren bir dizedir.<br /><br />**Numaralı parametre**: 2<br /><br /> **Ad**: biçimi<br /><br /> **Açıklama**: isteğe bağlı. Ya da bir [tek bir biçim belirticisi karakter](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) veya [özel biçim deseni](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) bu zaman damgası değeri biçimine gösterir. Biçim sağlanmazsa, ISO 8601 biçiminde ("o") kullanılır.|  
|startOfDay|Geçirilen bir dize zaman damgası için günün başlangıcını döndürür. Örneğin `2017-03-15T00:00:00Z`:<br /><br /> `startOfDay('2017-03-15T13:27:36Z')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: zaman damgası<br /><br /> **Açıklama**: gerekli. Bu saati içeren bir dizedir.<br /><br />**Numaralı parametre**: 2<br /><br /> **Ad**: biçimi<br /><br /> **Açıklama**: isteğe bağlı. Ya da bir [tek bir biçim belirticisi karakter](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) veya [özel biçim deseni](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) bu zaman damgası değeri biçimine gösterir. Biçim sağlanmazsa, ISO 8601 biçiminde ("o") kullanılır.| 
|startOfMonth|Geçirilen bir dize zaman damgası için ayın başlangıcını döndürür. Örneğin `2017-03-01T00:00:00Z`:<br /><br /> `startOfMonth('2017-03-15T13:27:36Z')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: zaman damgası<br /><br /> **Açıklama**: gerekli. Bu saati içeren bir dizedir.<br /><br />**Numaralı parametre**: 2<br /><br /> **Ad**: biçimi<br /><br /> **Açıklama**: isteğe bağlı. Ya da bir [tek bir biçim belirticisi karakter](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) veya [özel biçim deseni](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) bu zaman damgası değeri biçimine gösterir. Biçim sağlanmazsa, ISO 8601 biçiminde ("o") kullanılır.| 
|DayOfWeek|Bir dize zaman damgası bileşeninin hafta gününü verir.  Pazar 0, Pazartesi 1 ve benzeri. Örneğin `3`:<br /><br /> `dayOfWeek('2017-03-15T13:27:36Z')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: zaman damgası<br /><br /> **Açıklama**: gerekli. Bu saati içeren bir dizedir.| 
|DayOfMonth|Bir dize zaman damgası ay bileşenini gününü verir. Örneğin `15`:<br /><br /> `dayOfMonth('2017-03-15T13:27:36Z')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: zaman damgası<br /><br /> **Açıklama**: gerekli. Bu saati içeren bir dizedir.| 
|DayOfYear|Bir dize zaman damgası yıl bileşenini gününü verir. Örneğin `74`:<br /><br /> `dayOfYear('2017-03-15T13:27:36Z')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: zaman damgası<br /><br /> **Açıklama**: gerekli. Bu saati içeren bir dizedir.| 
|işaretleri|Çizgilerine bir dize zaman damgası özelliği döndürür. Örneğin `1489603019`:<br /><br /> `ticks('2017-03-15T18:36:59Z')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: zaman damgası<br /><br /> **Açıklama**: gerekli. Bu saati içeren bir dizedir.| 
  
### <a name="workflow-functions"></a>İş akışı işlevleri  

Bu işlevler çalışma zamanında iş akışı hakkında bilgi almak yardımcı olur.  
  
|İşlev adı|Açıklama|  
|-------------------|-----------------|  
|listCallbackUrl|Tetikleyici veya eylem çağırmak için aranacak bir dize döndürür. <p> **Not**: Bu işlev yalnızca kullanılabilir bir **httpWebhook** ve **apiConnectionWebhook**, değil de bir **el ile**, **yineleme**, **http**, veya **apiConnection**. <p>Örneğin, `listCallbackUrl()` işlev döndürür: <p>`https://prod-01.westus.logic.azure.com:443/workflows/1235...ABCD/triggers/manual/run?api-version=2015-08-01-preview&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=xxx...xxx` |  
|iş akışı|Bu işlev iş akışının kendisini çalışma zamanında ait ayrıntıları sağlar. <p> İş akışı nesnesindeki kullanılabilir özellikleri şunlardır: <ul><li>`name`</li><li>`type`</li><li>`id`</li><li>`location`</li><li>`run`</li></ul> <p> Değeri `run` aşağıdaki özelliklere sahip bir nesne özelliğidir: <ul><li>`name`</li><li>`type`</li><li>`id`</li></ul> <p>Bkz: [Rest API](http://go.microsoft.com/fwlink/p/?LinkID=525617) bu özellikleri hakkında ayrıntılı bilgi için.<p> Örneğin geçerli çalışma adını almak için kullanın `"@workflow().run.name"` ifade. |

## <a name="next-steps"></a>Sonraki adımlar

[İş akışı eylemleri ve tetikleyicileri](logic-apps-workflow-actions-triggers.md)
