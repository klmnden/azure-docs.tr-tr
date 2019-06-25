---
title: İş akışı tanımlama dili - Azure Logic Apps ve Microsoft Flow işlevleri için başvuru
description: Azure Logic Apps ve Microsoft Flow için iş akışı tanımlama dili ile oluşturulan ifadelerde işlevlerine yönelik başvuru kılavuzu
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: reference
ms.date: 08/15/2018
ms.openlocfilehash: f5e2af7a7118eaa95e43049b3594ffd584aad4cc
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203072"
---
# <a name="functions-reference-for-workflow-definition-language-in-azure-logic-apps-and-microsoft-flow"></a>Azure Logic Apps ve Microsoft Flow, iş akışı tanımlama dili için işlev başvurusu

İş akışı tanımları için [Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve [Microsoft Flow](https://docs.microsoft.com/flow/getting-started), bazı [ifadeleri](../logic-apps/logic-apps-workflow-definition-language.md#expressions) henüz bulunmayabilir çalışma zamanı eylemlerden değerlerini alın, iş akışınızı çalışmaya başlar. Bu değerleri başvuru ya da bu ifadelerdeki değerler işlemek için kullanabileceğiniz *işlevleri* tarafından sağlanan [iş akışı tanımlama dili](../logic-apps/logic-apps-workflow-definition-language.md). 

> [!NOTE]
> Bu başvuru sayfası, Azure Logic Apps ve Microsoft Flow için geçerlidir, ancak Azure Logic Apps belgelerinde görünür. Özel mantıksal uygulamalar için bu sayfayı başvuruyor ancak bu işlevler, akışları ve logic apps için çalışır. İşlevleri ve ifadeleri Microsoft Flow hakkında daha fazla bilgi için bkz. [koşullarda ifadeleri kullanma](https://docs.microsoft.com/flow/use-expressions-in-conditions).

Örneğin, matematik işlevleri gibi kullanarak değerleri hesaplama [add() işlevi](../logic-apps/workflow-definition-language-functions-reference.md#add)tamsayılar veya float toplamı istediğinizde. İşlevler ile gerçekleştirebileceğiniz diğer birkaç örnek görevler aşağıda verilmiştir:

| Görev | İşlev sözdizimi | Sonuç |
| ---- | --------------- | ------ |
| Küçük harf biçiminde bir dize döndürür. | toLower ('<*metin*>') <p>Örneğin: toLower('Hello') | "hello" |
| Bir genel benzersiz tanıtıcısı (GUID) döndürür. | Guid() |"c2ecc88d-88c8-4096-912c-d6f2e2b138ce" |
||||

İşlevlerin bulunacağı [kendi genel amacına bağlı](#ordered-by-purpose), aşağıdaki tablolarda gözden geçirin. Veya her işlevi hakkında ayrıntılı bilgi için bkz. [alfabetik liste](#alphabetical-list).

> [!NOTE]
> Parametre tanımlarıyla sözdiziminde, bir soru bir parametre, parametre anlamına gelir sonra görüntülenen işareti (?) isteğe bağlıdır.
> Örneğin, [getFutureTime()](#getFutureTime).

## <a name="functions-in-expressions"></a>İşlevler ifadelerde

Bir ifade bir işlev kullanmayı göstermek için bu örnek değeri nasıl elde edileceği gösterilmektedir `customerName` parametresi ve değeri Ata `accountName` kullanarak özellik [parameters()](#parameters) işlevi bir ifadede:

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

Örneğin, `concat()` işlevi parametre olarak dize değerleri iki veya daha fazla sürebilir. Bu işlev, bu dizelerin tek dizesinde birleştirir.
Birleştirilmiş bir dize, "SophiaOwen" alabilmeniz ya da dize değişmez değerleri, örneğin, "Sophia" ve "Owen" geçirebilirsiniz:

```json
"customerName": "@concat('Sophia', 'Owen')"
```

Veya, parametrelerinden dize değerleri alabilirsiniz. Bu örnekte `parameters()` her işlevde `concat()` parametresi ve `firstName` ve `lastName` parametreleri. Daha sonra çıkan dizeleri için geçirdiğiniz `concat()` birleşik bir dize, örneğin, "SophiaOwen" alabilmeniz işlev:

```json
"customerName": "@concat(parameters('firstName'), parameters('lastName'))"
```

Her iki durumda da, sonucu örneklerin her ikisi de atama `customerName` özelliği.

Sıralı, genel amaçlı kullanabileceğiniz işlevler şunlardır veya temel işlevleri göz atabilirsiniz [alfabetik](#alphabetical-list).

<a name="ordered-by-purpose"></a>
<a name="string-functions"></a>

## <a name="string-functions"></a>Dize işlevleri

Dizeleri ile çalışmak için bu dize işlevleri ve ayrıca bazı kullanabilirsiniz [toplama işlevleri](#collection-functions).
Dize işlevleri yalnızca dizeler üzerinde çalışır.

| Dize işlevi | Görev |
| --------------- | ---- |
| [concat](../logic-apps/workflow-definition-language-functions-reference.md#concat) | İki veya daha fazla dizeleri birleştirmek ve birleştirilmiş dizeyi döndürür. |
| [endsWith](../logic-apps/workflow-definition-language-functions-reference.md#endswith) | Bir dizeyi, belirtilen alt dizeyle bitip bitmediğini kontrol edin. |
| [guid](../logic-apps/workflow-definition-language-functions-reference.md#guid) | Bir dize olarak bir genel benzersiz tanıtıcısı (GUID) oluşturur. |
| [indexOf](../logic-apps/workflow-definition-language-functions-reference.md#indexof) | Bir alt dizenin başlangıç konumunu döndürür. |
| [lastIndexOf](../logic-apps/workflow-definition-language-functions-reference.md#lastindexof) | Bir dizenin son a geçişi için başlangıç konumunu döndürür. |
| [replace](../logic-apps/workflow-definition-language-functions-reference.md#replace) | Belirtilen dizenin bir alt dizenin yerini ve güncelleştirilmiş dizeyi döndür. |
| [split](../logic-apps/workflow-definition-language-functions-reference.md#split) | Belirtilen sınırlayıcı karakter özgün dizedeki göre daha büyük bir dizeden virgülle ayrılmış bir alt dizeler, içeren bir dizi döndürür. |
| [startsWith](../logic-apps/workflow-definition-language-functions-reference.md#startswith) | Bir dizeyi belirli bir alt dizesi ile başlayıp başlamadığını kontrol edin. |
| [substring](../logic-apps/workflow-definition-language-functions-reference.md#substring) | Belirtilen konumundan başlayan bir dizeden karakterleri döndürür. |
| [toLower](../logic-apps/workflow-definition-language-functions-reference.md#toLower) | Küçük harf biçiminde bir dize döndürür. |
| [toUpper](../logic-apps/workflow-definition-language-functions-reference.md#toUpper) | Büyük harf biçiminde bir dize döndürür. |
| [trim](../logic-apps/workflow-definition-language-functions-reference.md#trim) | Bir dizeden öndeki ve sondaki boşlukları kaldırın ve güncelleştirilmiş bir dize döndürür. |
|||

<a name="collection-functions"></a>

## <a name="collection-functions"></a>Toplama işlevleri

Koleksiyonlar, genellikle dizi, dizeleri ve sözlükleri ile bazen çalışmak için bu toplama işlevleri kullanabilirsiniz.

| Toplama işlevi | Görev |
| ------------------- | ---- |
| [contains](../logic-apps/workflow-definition-language-functions-reference.md#contains) | Bir koleksiyon için belirli bir öğe olup olmadığını denetleyin. |
| [empty](../logic-apps/workflow-definition-language-functions-reference.md#empty) | Bir koleksiyonun boş olup olmadığını denetleyin. |
| [first](../logic-apps/workflow-definition-language-functions-reference.md#first) | Bir koleksiyondaki ilk öğeyi döndürür. |
| [intersection](../logic-apps/workflow-definition-language-functions-reference.md#intersection) | Sahip bir koleksiyonun dönüş *yalnızca* belirtilen koleksiyonlarla arasında ortak öğeleri. |
| [item](../logic-apps/workflow-definition-language-functions-reference.md#item) | Bir dizi üzerindeki bir yinelenen eylemi olduğu zaman içinde eylemin geçerli yineleme sırasında dizideki geçerli öğeyi döndürür. |
| [join](../logic-apps/workflow-definition-language-functions-reference.md#join) | İçeren bir dize döndürecek *tüm* bir dizi, öğeleri belirtilen karakteriyle ayrılmış. |
| [last](../logic-apps/workflow-definition-language-functions-reference.md#last) | Bir koleksiyondaki son öğeyi döndürür. |
| [length](../logic-apps/workflow-definition-language-functions-reference.md#length) | Bir dize ya da dizideki öğe sayısını döndürür. |
| [skip](../logic-apps/workflow-definition-language-functions-reference.md#skip) | Bir koleksiyonun önünden öğeleri kaldırıp dönüş *diğer tüm* öğeleri. |
| [take](../logic-apps/workflow-definition-language-functions-reference.md#take) | Bir koleksiyonun önünden öğelerini döndürür. |
| [union](../logic-apps/workflow-definition-language-functions-reference.md#union) | Sahip bir koleksiyonun dönüş *tüm* belirtilen koleksiyonlarla öğelerinden. |
|||

<a name="comparison-functions"></a>

## <a name="logical-comparison-functions"></a>Mantıksal karşılaştırma işlevleri

Koşullar ile çalışma, değerleri ve ifade sonuçları karşılaştırmak ya da mantıksal çeşitli değerlendirmek için bu mantıksal karşılaştırma işlevleri kullanabilirsiniz.
Her işlev hakkındaki tam başvuru için bkz: [alfabetik liste](../logic-apps/workflow-definition-language-functions-reference.md#alphabetical-list).

| Mantıksal bir karşılaştırma işlevi | Görev |
| --------------------------- | ---- |
| [and](../logic-apps/workflow-definition-language-functions-reference.md#and) | Tüm ifadelerin doğru olup olmadığını denetleyin. |
| [equals](../logic-apps/workflow-definition-language-functions-reference.md#equals) | Her iki değer eşit olup olmadığını denetleyin. |
| [greater](../logic-apps/workflow-definition-language-functions-reference.md#greater) | İlk değer ikinci değerden büyük olup olmadığını denetleyin. |
| [greaterOrEquals](../logic-apps/workflow-definition-language-functions-reference.md#greaterOrEquals) | İlk değer ikinci değerine eşit veya daha büyük olup olmadığını denetleyin. |
| [if](../logic-apps/workflow-definition-language-functions-reference.md#if) | İfadenin true veya false olup olmadığını denetleyin. Sonuca bağlı, belirli bir değeri döndürme. |
| [less](../logic-apps/workflow-definition-language-functions-reference.md#less) | İkinci değer ilk değer olup değerinden denetleyin. |
| [lessOrEquals](../logic-apps/workflow-definition-language-functions-reference.md#lessOrEquals) | İlk değer ya da ikinci değer eşit olup olmadığını denetleyin. |
| [not](../logic-apps/workflow-definition-language-functions-reference.md#not) | Bir ifadenin false olup olmadığını denetleyin. |
| [or](../logic-apps/workflow-definition-language-functions-reference.md#or) | En az bir ifadenin doğru olup olmadığını denetleyin. |
|||

<a name="conversion-functions"></a>

## <a name="conversion-functions"></a>Dönüştürme işlevleri

Bir değer türü veya biçimi değiştirmek için bu dönüştürme işlevleri kullanabilirsiniz.
Örneğin, bir Boole değeri tamsayıya değiştirebilirsiniz.
Logic Apps içerik türlerine dönüştürme işlemi sırasında nasıl işlediği hakkında daha fazla bilgi için bkz. [içerik türlerini işleme](../logic-apps/logic-apps-content-type.md).
Her işlev hakkındaki tam başvuru için bkz: [alfabetik liste](../logic-apps/workflow-definition-language-functions-reference.md#alphabetical-list).

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
| [json](../logic-apps/workflow-definition-language-functions-reference.md#json) | JavaScript nesne gösterimi (JSON) türü değeri veya dize veya XML nesnesi döndürür. |
| [dize](../logic-apps/workflow-definition-language-functions-reference.md#string) | Dize sürümü için giriş değeri döndürür. |
| [uriComponent](../logic-apps/workflow-definition-language-functions-reference.md#uriComponent) | URL güvenli olmayan karakterleri kaçış karakterleri ile değiştirerek bir giriş değeri için URI ile kodlanmış sürümünü döndürür. |
| [uriComponentToBinary](../logic-apps/workflow-definition-language-functions-reference.md#uriComponentToBinary) | İkili dosya sürümü için URI ile kodlanmış bir dize döndürür. |
| [uriComponentToString](../logic-apps/workflow-definition-language-functions-reference.md#uriComponentToString) | Dize sürümü için URI ile kodlanmış bir dize döndürür. |
| [xml](../logic-apps/workflow-definition-language-functions-reference.md#xml) | XML sürümü bir dize döndürür. |
|||

<a name="math-functions"></a>

## <a name="math-functions"></a>Matematik işlevleri

Tamsayı float'lar ile çalışmak için bu matematik işlevleri kullanabilirsiniz.
Her işlev hakkındaki tam başvuru için bkz: [alfabetik liste](../logic-apps/workflow-definition-language-functions-reference.md#alphabetical-list).

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

## <a name="date-and-time-functions"></a>Tarih ve saat işlevleri

Tarihler ve saatler ile çalışmak için bu tarih ve saat işlevleri kullanabilirsiniz.
Her işlev hakkındaki tam başvuru için bkz: [alfabetik liste](../logic-apps/workflow-definition-language-functions-reference.md#alphabetical-list).

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

## <a name="workflow-functions"></a>İş akışı işlevleri

Bu iş akışı işlevleri size yardımcı olabilir:

* Çalışma zamanında bir iş akışı örneği hakkındaki ayrıntıları alın.
* Mantıksal uygulamalar veya akışlar oluşturmak için kullanılan girişleri çalışın.
* Tetikleyiciler ve Eylemler çıkışlarına başvuruyor.

Örneğin, bir eylemden çıkışlarına başvuruyor ve bir sonraki eylem bu verileri kullanabilirsiniz.
Her işlev hakkındaki tam başvuru için bkz: [alfabetik liste](../logic-apps/workflow-definition-language-functions-reference.md#alphabetical-list).

| İş akışı işlevi | Görev |
| ----------------- | ---- |
| [Eylem](../logic-apps/workflow-definition-language-functions-reference.md#action) | Çalışma zamanı veya değerleri geçerli eylemin çıkışını diğer JSON ad ve değer çiftlerinden geri döndürür. Ayrıca bkz: [eylemleri](../logic-apps/workflow-definition-language-functions-reference.md#actions). |
| [actionBody](../logic-apps/workflow-definition-language-functions-reference.md#actionBody) | Bir eylemin dönüş `body` çalışma zamanında çıktı. Ayrıca bkz: [gövdesi](../logic-apps/workflow-definition-language-functions-reference.md#body). |
| [actionOutputs](../logic-apps/workflow-definition-language-functions-reference.md#actionOutputs) | Çalışma zamanında bir eylemin çıkışını geri döndürür. Bkz: [eylemleri](../logic-apps/workflow-definition-language-functions-reference.md#actions). |
| [Eylemler](../logic-apps/workflow-definition-language-functions-reference.md#actions) | Bir eylemin çıkışını çalışma zamanı veya değerleri diğer JSON ad ve değer çiftlerinden geri döndürür. Ayrıca bkz: [eylem](../logic-apps/workflow-definition-language-functions-reference.md#action).  |
| [body](#body) | Bir eylemin dönüş `body` çalışma zamanında çıktı. Ayrıca bkz: [actionBody](../logic-apps/workflow-definition-language-functions-reference.md#actionBody). |
| [formDataMultiValues](../logic-apps/workflow-definition-language-functions-reference.md#formDataMultiValues) | Bir anahtar adı ile eşleşen değerleriyle bir dizi oluşturmak *form verilerini* veya *form kodlu* eylem çıktı. |
| [formDataValue](../logic-apps/workflow-definition-language-functions-reference.md#formDataValue) | Bir eylem içinde bir anahtar adı ile eşleşen tek bir değer döndürmesi *form verisinin* veya *form kodlu çıkış*. |
| [Öğesi](../logic-apps/workflow-definition-language-functions-reference.md#item) | Bir dizi üzerindeki bir yinelenen eylemi olduğu zaman içinde eylemin geçerli yineleme sırasında dizideki geçerli öğeyi döndürür. |
| [Öğeleri](../logic-apps/workflow-definition-language-functions-reference.md#items) | Bir Foreach, zaman içindeki ya da Until döngüsü belirtilen döngünün geçerli öğeyi döndürür.|
| [iterationIndexes](../logic-apps/workflow-definition-language-functions-reference.md#iterationIndexes) | Bir Until döngü olduğu zaman içinde geçerli yineleme için dizin değerini döndürür. Döngüler kadar iç içe geçmiş içinde bu işlevi kullanabilirsiniz. |
| [listCallbackUrl](../logic-apps/workflow-definition-language-functions-reference.md#listCallbackUrl) | "Bir tetikleyici veya eylemi çağırır geri çağırma URL'si" döndürür. |
| [multipartBody](../logic-apps/workflow-definition-language-functions-reference.md#multipartBody) | Birden çok bölümü olan bir eylemin çıkış belirli bir parçanın gövdesini döndürür. |
| [parametreler](../logic-apps/workflow-definition-language-functions-reference.md#parameters) | Tanımlanan bir parametre için değer iş akışı tanımınızı döndürün. |
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

## <a name="uri-parsing-functions"></a>URI ayrıştırma işlevleri

Tekdüzen Kaynak Tanımlayıcıları (URI'lar) çalışır ve bu bir URI'leri için çeşitli özellik değerlerini almak için bu URI ayrıştırma işlevleri kullanabilirsiniz.
Her işlev hakkındaki tam başvuru için bkz: [alfabetik liste](../logic-apps/workflow-definition-language-functions-reference.md#alphabetical-list).

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

## <a name="manipulation-functions-json--xml"></a>İşleme işlevleri: JSON & XML

JSON nesneleri ve XML düğümü ile çalışmak için bu işleme işlevleri kullanabilirsiniz.
Her işlev hakkındaki tam başvuru için bkz: [alfabetik liste](../logic-apps/workflow-definition-language-functions-reference.md#alphabetical-list).

| İşleme işlevi | Görev |
| --------------------- | ---- |
| [addProperty](../logic-apps/workflow-definition-language-functions-reference.md#addProperty) | Bir özellik, değer veya ad-değer çifti için bir JSON nesnesi ekleyin ve güncelleştirilmiş nesneyi döndürür. |
| [birleşim](../logic-apps/workflow-definition-language-functions-reference.md#coalesce) | Bir veya daha fazla parametrelerinden ilk null olmayan değer döndürür. |
| [removeProperty](../logic-apps/workflow-definition-language-functions-reference.md#removeProperty) | Bir JSON nesnesinden bir özelliğini kaldırın ve güncelleştirilmiş nesneyi döndürür. |
| [setProperty](../logic-apps/workflow-definition-language-functions-reference.md#setProperty) | Bir JSON nesnesi özelliği değerini ayarlayın ve güncelleştirilmiş nesneyi döndürür. |
| [XPath](../logic-apps/workflow-definition-language-functions-reference.md#xpath) | Düğümleri veya bir (XML Path Language) XPath ifadesi eşleşen değerler için XML denetleyin ve eşleşen düğümleri veya değerleri döndürür. |
|||

<a name="alphabetical-list"></a>
<a name="action"></a>

### <a name="action"></a>action

Dönüş *geçerli* çalışma zamanı ya da bir ifadeye atayabilirsiniz diğer JSON ad ve değer çiftleri değerlerinden çıkış eylem.
Varsayılan olarak, bu işlevi tüm eylem nesnesine başvuruyor ancak değerini istediğiniz bir özellik isteğe bağlı olarak belirtebilirsiniz.
Ayrıca bkz: [actions()](../logic-apps/workflow-definition-language-functions-reference.md#actions).

Kullanabileceğiniz `action()` işlevi yalnızca bu konumları:

* `unsubscribe` Özelliği bir Web kancası eylem sonucu orijinal olandan erişebilmesi için `subscribe` isteği
* `trackedProperties` Eylemdir özelliği
* `do-until` Döngü bir eylem koşulunu

```
action()
action().outputs.body.<property>
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Özelliği*> | Hayır | String | Değerini istediğiniz eylem nesnenin özellik adı: **adı**, **startTime**, **endTime**, **girişleri**,  **çıkaran**, **durumu**, **kod**, **Trackingıd**, ve **clientTrackingId**. Azure portalında bir belirli çalıştırma geçmişi ait ayrıntıları gözden geçirerek bu özelliklere bulabilirsiniz. Daha fazla bilgi için [REST API - iş akışı çalıştırma eylemi](https://docs.microsoft.com/rest/api/logic/workflowrunactions/get). |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | -----| ----------- |
| <*action-output*> | String | Geçerli eylem veya özellik çıktısı |
||||

<a name="actionBody"></a>

### <a name="actionbody"></a>actionBody

Bir eylemin dönüş `body` çalışma zamanında çıktı.
İçin Toplu özellik `actions('<actionName>').outputs.body`.
Bkz: [body()](#body) ve [actions()](#actions).

```
actionBody('<actionName>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*actionName*> | Evet | String | Eylemin adını `body` istediğiniz çıkış |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | -----| ----------- |
| <*Eylem gövdesi çıkışı*> | String | `body` Belirtilen eylem çıktısı |
||||

*Örnek*

Bu örnekte `body` Twitter eylem çıkış `Get user`:

```
actionBody('Get_user')
```

Ve bu sonucu verir:

```json
"body": {
  "FullName": "Contoso Corporation",
  "Location": "Generic Town, USA",
  "Id": 283541717,
  "UserName": "ContosoInc",
  "FollowersCount": 172,
  "Description": "Leading the way in transforming the digital workplace.",
  "StatusesCount": 93,
  "FriendsCount": 126,
  "FavouritesCount": 46,
  "ProfileImageUrl": "https://pbs.twimg.com/profile_images/908820389907722240/gG9zaHcd_400x400.jpg"
}
```

<a name="actionOutputs"></a>

### <a name="actionoutputs"></a>actionOutputs

Çalışma zamanında bir eylemin çıkışını geri döndürür.
İçin Toplu özellik `actions('<actionName>').outputs`.
Bkz: [actions()](#actions).

```
actionOutputs('<actionName>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*actionName*> | Evet | String | Eylem adı, istediğiniz çıktı |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | -----| ----------- |
| <*Çıkış*> | String | Belirtilen eylem çıktısı |
||||

*Örnek*

Bu örnek çıkış Twitter eyleminden alır `Get user`:

```
actionOutputs('Get_user')
```

Ve bu sonucu verir:

```json
{
  "statusCode": 200,
  "headers": {
    "Pragma": "no-cache",
    "Vary": "Accept-Encoding",
    "x-ms-request-id": "a916ec8f52211265d98159adde2efe0b",
    "X-Content-Type-Options": "nosniff",
    "Timing-Allow-Origin": "*",
    "Cache-Control": "no-cache",
    "Date": "Mon, 09 Apr 2018 18:47:12 GMT",
    "Set-Cookie": "ARRAffinity=b9400932367ab5e3b6802e3d6158afffb12fcde8666715f5a5fbd4142d0f0b7d;Path=/;HttpOnly;Domain=twitter-wus.azconn-wus.p.azurewebsites.net",
    "X-AspNet-Version": "4.0.30319",
    "X-Powered-By": "ASP.NET",
    "Content-Type": "application/json; charset=utf-8",
    "Expires": "-1",
    "Content-Length": "339"
  },
  "body": {
    "FullName": "Contoso Corporation",
    "Location": "Generic Town, USA",
    "Id": 283541717,
    "UserName": "ContosoInc",
    "FollowersCount": 172,
    "Description": "Leading the way in transforming the digital workplace.",
    "StatusesCount": 93,
    "FriendsCount": 126,
    "FavouritesCount": 46,
    "ProfileImageUrl": "https://pbs.twimg.com/profile_images/908820389907722240/gG9zaHcd_400x400.jpg"
  }
}
```

## <a name="all-functions---alphabaetical-list"></a>Tüm İşlevler - alphabaetical listesi

<a name="actions"></a>

### <a name="actions"></a>Eylemler

Bir ifadeyi atayabilirsiniz diğer JSON ad ve değer çiftleri gelen bir eylemin çıkış çalışma zamanı veya değerleri döndürür. Varsayılan olarak, tüm eylem nesnesi işlev başvuruyor, ancak isteğe bağlı olarak bir özellik belirtin, istediğiniz değeri.
Toplu sürümleri için bkz: [actionBody()](#actionBody), [actionOutputs()](#actionOutputs), ve [body()](#body).
Geçerli eylem için bkz: [action()](#action).

> [!NOTE]
> Daha önce kullanabileceğinizi `actions()` işlevi veya `conditions` eylem çıktıyı başka bir eylem tabanlı çalıştırıldığını belirtirken öğesi. Ancak, açıkça Eylemler arasındaki bağımlılıkları bildirmek için artık bağımlı eylemin kullanmalısınız `runAfter` özelliği.
> Hakkında daha fazla bilgi edinmek için `runAfter` özelliğine bakın [Catch ve runAfter özelliğiyle hataları işlemek](../logic-apps/logic-apps-workflow-definition-language.md).

```
actions('<actionName>')
actions('<actionName>').outputs.body.<property>
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*actionName*> | Evet | String | Çıkış istediğiniz eylem nesnesi adı  |
| <*Özelliği*> | Hayır | String | Değerini istediğiniz eylem nesnenin özellik adı: **adı**, **startTime**, **endTime**, **girişleri**,  **çıkaran**, **durumu**, **kod**, **Trackingıd**, ve **clientTrackingId**. Azure portalında bir belirli çalıştırma geçmişi ait ayrıntıları gözden geçirerek bu özelliklere bulabilirsiniz. Daha fazla bilgi için [REST API - iş akışı çalıştırma eylemi](https://docs.microsoft.com/rest/api/logic/workflowrunactions/get). |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | -----| ----------- |
| <*action-output*> | String | Belirtilen eylem veya özellik çıktısı |
||||

*Örnek*

Bu örnekte `status` özellik değeri Twitter eyleminin `Get user` zamanında:

```
actions('Get_user').outputs.body.status
```

Ve bu sonucu verir: `"Succeeded"`

<a name="add"></a>

### <a name="add"></a>add

İki sayıyı eklemesini sonucu döndürür.

```
add(<summand_1>, <summand_2>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*summand_1*>, <*summand_2*> | Evet | İnteger, Float, veya karma | Eklenecek sayı |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | -----| ----------- |
| <*sonucu toplama*> | Tamsayı veya kayan | Belirtilen sayı eklemesini sonucu |
||||

*Örnek*

Bu örnek belirtilen sayı ekler:

```
add(1, 1.5)
```

Ve bu sonucu verir: `2.5`

<a name="addDays"></a>

### <a name="adddays"></a>addDays

Bir gün sayısı için bir zaman damgası ekleyin.

```
addDays('<timestamp>', <days>, '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | String | Zaman damgası içeren dize |
| <*gün*> | Evet | Integer | Eklenecek gün sayısı pozitif veya negatif |
| <*Biçim*> | Hayır | String | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddTHH:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Güncelleştirilen zaman damgası*> | String | Zaman damgası ve belirtilen gün sayısı  |
||||

*Örnek 1*

Bu örnek, belirtilen zaman damgası için 10 gün ekler:

```
addDays('2018-03-15T13:00:00Z', 10)
```

Ve bu sonucu verir: `"2018-03-25T00:00:0000000Z"`

*Örnek 2*

Bu örnek, belirtilen zaman damgası beş günden çıkarır:

```
addDays('2018-03-15T00:00:00Z', -5)
```

Ve bu sonucu verir: `"2018-03-10T00:00:0000000Z"`

<a name="addHours"></a>

### <a name="addhours"></a>addHours

Saat sayısı için bir zaman damgası ekleyin.

```
addHours('<timestamp>', <hours>, '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | String | Zaman damgası içeren dize |
| <*saat*> | Evet | Integer | Eklenecek saat sayısı pozitif veya negatif |
| <*Biçim*> | Hayır | String | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddTHH:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Güncelleştirilen zaman damgası*> | String | Zaman damgası artı belirtilen sayıda saat  |
||||

*Örnek 1*

Bu örnek, 10 saat için belirtilen zaman damgası ekler:

```
addHours('2018-03-15T00:00:00Z', 10)
```

Ve bu sonucu verir: `"2018-03-15T10:00:0000000Z"`

*Örnek 2*

Bu örnek, belirtilen zaman damgası beş saatten çıkarır:

```
addHours('2018-03-15T15:00:00Z', -5)
```

Ve bu sonucu verir: `"2018-03-15T10:00:0000000Z"`

<a name="addMinutes"></a>

### <a name="addminutes"></a>addMinutes

Dakika sayısı için bir zaman damgası ekleyin.

```
addMinutes('<timestamp>', <minutes>, '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | String | Zaman damgası içeren dize |
| <*dakika*> | Evet | Integer | Pozitif veya negatif eklenecek dakika sayısı |
| <*Biçim*> | Hayır | String | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddTHH:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Güncelleştirilen zaman damgası*> | String | Zaman damgası artı belirtilen sayıda dakika |
||||

*Örnek 1*

Bu örnek, 10 dakika için belirtilen zaman damgası ekler:

```
addMinutes('2018-03-15T00:10:00Z', 10)
```

Ve bu sonucu verir: `"2018-03-15T00:20:00.0000000Z"`

*Örnek 2*

Bu örnek, belirtilen zaman damgası beş dakika çıkarır:

```
addMinutes('2018-03-15T00:20:00Z', -5)
```

Ve bu sonucu verir: `"2018-03-15T00:15:00.0000000Z"`

<a name="addProperty"></a>

### <a name="addproperty"></a>addProperty

Bir özellik, değer veya ad-değer çifti için bir JSON nesnesi ekleyin ve güncelleştirilmiş nesneyi döndürür. Çalışma zamanında nesne zaten varsa, işlev bir hata oluşturur.

```
addProperty(<object>, '<property>', <value>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*object*> | Evet | Object | Bir özellik eklemek istediğiniz JSON nesnesi |
| <*Özelliği*> | Evet | String | Eklenecek özelliğin adı |
| <*Değer*> | Evet | Tüm | Özelliğinin değeri |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*güncelleştirilmiş nesne*> | Object | Belirtilen özellik ile güncelleştirilen JSON nesnesi |
||||

*Örnek*

Bu örnek ekler `accountNumber` özelliğini `customerProfile` JSON ile dönüştürülür nesnesi [JSON()](#json) işlevi.
İşlev tarafından oluşturulan bir değer atar [guid()](#guid) işlev ve güncelleştirilmiş nesne döndürür:

```
addProperty(json('customerProfile'), 'accountNumber', guid())
```

<a name="addSeconds"></a>

### <a name="addseconds"></a>saniyeEkle

Saniye sayısı için bir zaman damgası ekleyin.

```
addSeconds('<timestamp>', <seconds>, '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | String | Zaman damgası içeren dize |
| <*saniye*> | Evet | Integer | Pozitif veya negatif eklenecek saniye sayısı |
| <*Biçim*> | Hayır | String | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddTHH:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Güncelleştirilen zaman damgası*> | String | Zaman damgası artı belirtilen saniye sayısı  |
||||

*Örnek 1*

Bu örnek, belirtilen zaman damgası için 10 saniye ekler:

```
addSeconds('2018-03-15T00:00:00Z', 10)
```

Ve bu sonucu verir: `"2018-03-15T00:00:10.0000000Z"`

*Örnek 2*

Bu örnek, belirtilen zaman damgası için beş saniye çıkarır:

```
addSeconds('2018-03-15T00:00:30Z', -5)
```

Ve bu sonucu verir: `"2018-03-15T00:00:25.0000000Z"`

<a name="addToTime"></a>

### <a name="addtotime"></a>addToTime

Zaman birimlerinin sayısı için bir zaman damgası ekleyin.
Ayrıca bkz: [getFutureTime()](#getFutureTime).

```
addToTime('<timestamp>', <interval>, '<timeUnit>', '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | String | Zaman damgası içeren dize |
| <*aralığı*> | Evet | Integer | Eklenecek belirtilen zaman birimi |
| <*timeUnit*> | Evet | String | İle kullanılacak zaman birimi *aralığı*: "Saniye", "Minute", "Hour", "Day", "Week", "Month", "Year" |
| <*Biçim*> | Hayır | String | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddTHH:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Güncelleştirilen zaman damgası*> | String | Belirtilen zaman birimi sayısı artı zaman damgası  |
||||

*Örnek 1*

Bu örnek, bir gün için belirtilen zaman damgası ekler:

```
addToTime('2018-01-01T00:00:00Z', 1, 'Day')
```

Ve bu sonucu verir: `"2018-01-02T00:00:00.0000000Z"`

*Örnek 2*

Bu örnek, bir gün için belirtilen zaman damgası ekler:

```
addToTime('2018-01-01T00:00:00Z', 1, 'Day', 'D')
```

Ve isteğe bağlı bir "D" biçim kullanarak sonucunu döndürür: `"Tuesday, January 2, 2018"`

<a name="and"></a>

### <a name="and"></a>ve

Tüm ifadelerin doğru olup olmadığını denetleyin.
Tüm ifadeler doğru olduğunda true döndürür veya en az bir ifade false olduğunda false döndürür.

```
and(<expression1>, <expression2>, ...)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*İfade1*>, <*expression2*>,... | Evet | Boolean | Denetlenecek ifadeleri |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | -----| ----------- |
| TRUE veya false | Boolean | Tüm ifadeler doğru olduğunda true döndürür. En az bir ifade false olduğunda false döndürür. |
||||

*Örnek 1*

Bu örnekler, belirtilen Boole değerlerin tümü doğru olup olmadığını denetleyin:

```
and(true, true)
and(false, true)
and(false, false)
```

Ve bu sonuçları döndürür:

* İlk örnek: Her iki ifade de doğruysa, bu nedenle döndürür `true`.
* İkinci örnek: Bir ifade yanlış olduğunda, bu nedenle döndürür `false`.
* Üçüncü örnek: Her iki ifade yanlış ise, bu nedenle döndürür `false`.

*Örnek 2*

Bu örnekler, belirtilen ifadeleri tümü doğru olup olmadığını denetleyin:

```
and(equals(1, 1), equals(2, 2))
and(equals(1, 1), equals(1, 2))
and(equals(1, 2), equals(1, 3))
```

Ve bu sonuçları döndürür:

* İlk örnek: Her iki ifade de doğruysa, bu nedenle döndürür `true`.
* İkinci örnek: Bir ifade yanlış olduğunda, bu nedenle döndürür `false`.
* Üçüncü örnek: Her iki ifade yanlış ise, bu nedenle döndürür `false`.

<a name="array"></a>

### <a name="array"></a>array

Belirtilen tek bir giriş için bir dizi döndürür.
Birden çok giriş için bkz. [createArray()](#createArray).

```
array('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | String | Dize dizisi oluşturmak için |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| [<*değer*>] | Dizi | Tek belirtilen giriş içeren bir dizi |
||||

*Örnek*

Bu örnekte, "hello" dizesi bir dizi oluşturur:

```
array('hello')
```

Ve bu sonucu verir: `["hello"]`

<a name="base64"></a>

### <a name="base64"></a>Base64

Base64 kodlamalı sürümü bir dize döndürür.

```
base64('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | String | Giriş dizesi |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Base64 dizesi*> | String | Girdi dizesi için base64 kodlamalı sürümü |
||||

*Örnek*

Bu örnekte, "hello" dizesi bir base64 ile kodlanmış dizeye dönüştürür:

```
base64('hello')
```

Ve bu sonucu verir: `"aGVsbG8="`

<a name="base64ToBinary"></a>

### <a name="base64tobinary"></a>base64ToBinary

İkili dosya sürümü için base64 ile kodlanmış bir dize döndürür.

```
base64ToBinary('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | String | Base64 ile kodlanmış dize dönüştürmek için |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*ikili için base64 dizesi*> | String | Base64 ile kodlanmış dize için ikili dosya sürümü |
||||

*Örnek*

Bu örnek "aGVsbG8 =" base64 ile kodlanmış dizeyi ikili dizeye:

```
base64ToBinary('aGVsbG8=')
```

Ve bu sonucu verir:

`"0110000101000111010101100111001101100010010001110011100000111101"`

<a name="base64ToString"></a>

### <a name="base64tostring"></a>base64ToString

Base64 dizesi etkili bir şekilde kod çözme base64 ile kodlanmış bir dize, dize sürümü döndürür.
Bu işlevi kullanmak yerine [decodeBase64()](#decodeBase64).
Her iki işlev aynı şekilde çalışır ancak `base64ToString()` tercih edilir.

```
base64ToString('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | String | Kodu çözülecek base64 ile kodlanmış dize |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*kodu çözülen-base64-string*> | String | Base64 ile kodlanmış bir dize dizesi sürümü |
||||

*Örnek*

Bu örnek "aGVsbG8 =" base64 ile kodlanmış dize yalnızca bir dize olarak:

```
base64ToString('aGVsbG8=')
```

Ve bu sonucu verir: `"hello"`

<a name="binary"></a>

### <a name="binary"></a>binary

İkili dosya sürümü bir dize döndürür.

```
binary('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | String | Dönüştürülecek dize. |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*ikili için giriş değeri*> | String | Belirtilen dize için ikili dosya sürümü |
||||

*Örnek*

Bu örnek "Merhaba" dizeyi ikili dizeye dönüştürür:

```
binary('hello')
```

Ve bu sonucu verir:

`"0110100001100101011011000110110001101111"`

<a name="body"></a>

### <a name="body"></a>Gövde

Bir eylemin dönüş `body` çalışma zamanında çıktı.
İçin Toplu özellik `actions('<actionName>').outputs.body`.
Bkz: [actionBody()](#actionBody) ve [actions()](#actions).

```
body('<actionName>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*actionName*> | Evet | String | Eylemin adını `body` istediğiniz çıkış |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | -----| ----------- |
| <*Eylem gövdesi çıkışı*> | String | `body` Belirtilen eylem çıktısı |
||||

*Örnek*

Bu örnekte `body` çıktı `Get user` Twitter eylem:

```
body('Get_user')
```

Ve bu sonucu verir:

```json
"body": {
    "FullName": "Contoso Corporation",
    "Location": "Generic Town, USA",
    "Id": 283541717,
    "UserName": "ContosoInc",
    "FollowersCount": 172,
    "Description": "Leading the way in transforming the digital workplace.",
    "StatusesCount": 93,
    "FriendsCount": 126,
    "FavouritesCount": 46,
    "ProfileImageUrl": "https://pbs.twimg.com/profile_images/908820389907722240/gG9zaHcd_400x400.jpg"
}
```

<a name="bool"></a>

### <a name="bool"></a>bool

Boole sürümü için bir değer döndürür.

```
bool(<value>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Tüm | Dönüştürülecek değer |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| TRUE veya false | Boolean | Belirtilen değer Boolean sürümü |
||||

*Örnek*

Bu örnekler, belirtilen değerlerini Boolean değerlerine dönüştürmek:

```
bool(1)
bool(0)
```

Ve bu sonuçları döndürür:

* İlk örnek: `true`
* İkinci örnek: `false`

<a name="coalesce"></a>

### <a name="coalesce"></a>birleşim

Bir veya daha fazla parametrelerinden ilk null olmayan değer döndürür.
Boş dizeler, boş diziler ve boş nesneler boş değildir.

```
coalesce(<object_1>, <object_2>, ...)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*object_1*>, <*object_2*>,... | Evet | Herhangi biri, türlerini karıştırmak | Null denetimi yapılacak bir veya daha fazla öğe |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*ilk olmayan-boş-öğesi*> | Tüm | İlk öğe veya null olmayan değer. Tüm parametreleri null ise, bu işlev null döndürür. |
||||

*Örnek*

Tüm değerleri null olduğunda bu örneklerin ilk null olmayan değer belirtilen değerleri veya null Döndür:

```
coalesce(null, true, false)
coalesce(null, 'hello', 'world')
coalesce(null, null, null)
```

Ve bu sonuçları döndürür:

* İlk örnek: `true`
* İkinci örnek: `"hello"`
* Üçüncü örnek: `null`

<a name="concat"></a>

### <a name="concat"></a>concat

İki veya daha fazla dizeleri birleştirmek ve birleştirilmiş dizeyi döndürür.

```
concat('<text1>', '<text2>', ...)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*text1*>, <*text2*>, ... | Evet | String | Birleştirmek için en az iki dizeleri |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*text1text2...* > | String | Birleşik giriş dizelerinden oluşturulan bir dize |
||||

*Örnek*

Bu örnekte, "Hello" ve "World" dizeleri birleştirir:

```
concat('Hello', 'World')
```

Ve bu sonucu verir: `"HelloWorld"`

<a name="contains"></a>

### <a name="contains"></a>içerir

Bir koleksiyon için belirli bir öğe olup olmadığını denetleyin.
Öğesi bulunduğunda true döndürür veya return false olduğunda nebyl nalezen.
Bu işlev büyük/küçük harf duyarlıdır.

```
contains('<collection>', '<value>')
contains([<collection>], '<value>')
```

Özellikle, bu işlev, bu koleksiyon türleri üzerinde çalışır:

* A *dize* bulmak için bir *alt dize*
* Bir *dizi* bulmak için bir *değeri*
* A *sözlük* bulmak için bir *anahtarı*

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Koleksiyon*> | Evet | Dize, dizi veya sözlük | Denetlenecek koleksiyon |
| <*Değer*> | Evet | Dize, dizi veya sözlük, sırasıyla | Bulunacak öğe |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| TRUE veya false | Boolean | Öğesi bulunduğunda true döndürür. Ne zaman nebyl nalezen false döndürün. |
||||

*Örnek 1*

Bu örnekte, "hello world" alt "world" için dize denetler ve true değerini döndürür:

```
contains('hello world', 'world')
```

*Örnek 2*

Bu örnek, "hello world" alt "evreni" için dize denetler ve false döndürür:

```
contains('hello world', 'universe')
```

<a name="convertFromUtc"></a>

### <a name="convertfromutc"></a>convertFromUtc

Bir zaman damgası (UTC) saat Eşgüdümlü Evrensel hedef saat dilimine dönüştürür.

```
convertFromUtc('<timestamp>', '<destinationTimeZone>', '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | String | Zaman damgası içeren dize |
| <*destinationTimeZone*> | Evet | String | Hedef saat dilimi adı. Daha fazla bilgi için [saat dilimi kimliklerini](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). |
| <*Biçim*> | Hayır | String | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddTHH:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Dönüştürülen zaman damgası*> | String | Hedef saat dilimine dönüştürülen zaman damgası |
||||

*Örnek 1*

Bu örnek, bir zaman damgasını belirtilen saat dilimine dönüştürür:

```
convertFromUtc('2018-01-01T08:00:00.0000000Z', 'Pacific Standard Time')
```

Ve bu sonucu verir: `"2018-01-01T00:00:00.0000000"`

*Örnek 2*

Bu örnek, bir zaman damgası belirtilen saat dilimini ve biçimini dönüştürür:

```
convertFromUtc('2018-01-01T08:00:00.0000000Z', 'Pacific Standard Time', 'D')
```

Ve bu sonucu verir: `"Monday, January 1, 2018"`

<a name="convertTimeZone"></a>

### <a name="converttimezone"></a>convertTimeZone

Bir zaman damgasını kaynak saat diliminden hedef saat dilimine dönüştürür.

```
convertTimeZone('<timestamp>', '<sourceTimeZone>', '<destinationTimeZone>', '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | String | Zaman damgası içeren dize |
| <*sourceTimeZone*> | Evet | String | Kaynak saat dilimi adı. Daha fazla bilgi için [saat dilimi kimliklerini](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). |
| <*destinationTimeZone*> | Evet | String | Hedef saat dilimi adı. Daha fazla bilgi için [saat dilimi kimliklerini](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). |
| <*Biçim*> | Hayır | String | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddTHH:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Dönüştürülen zaman damgası*> | String | Hedef saat dilimine dönüştürülen zaman damgası |
||||

*Örnek 1*

Bu örnekte, kaynak saat dilimi hedef saat dilimine dönüştürür:

```
convertTimeZone('2018-01-01T08:00:00.0000000Z', 'UTC', 'Pacific Standard Time')
```

Ve bu sonucu verir: `"2018-01-01T00:00:00.0000000"`

*Örnek 2*

Bu örnek belirtilen saat dilimini ve biçimini bir saat dilimine dönüştürür:

```
convertTimeZone('2018-01-01T80:00:00.0000000Z', 'UTC', 'Pacific Standard Time', 'D')
```

Ve bu sonucu verir: `"Monday, January 1, 2018"`

<a name="convertToUtc"></a>

### <a name="converttoutc"></a>convertToUtc

Bir zaman damgasını kaynak saat diliminden saati Eşgüdümlü Evrensel (UTC) dönüştürün.

```
convertToUtc('<timestamp>', '<sourceTimeZone>', '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | String | Zaman damgası içeren dize |
| <*sourceTimeZone*> | Evet | String | Kaynak saat dilimi adı. Daha fazla bilgi için [saat dilimi kimliklerini](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). |
| <*Biçim*> | Hayır | String | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddTHH:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Dönüştürülen zaman damgası*> | String | UTC'ye dönüştürüldükten zaman damgası |
||||

*Örnek 1*

Bu örnek, bir zaman damgasını UTC'ye dönüştürür:

```
convertToUtc('01/01/2018 00:00:00', 'Pacific Standard Time')
```

Ve bu sonucu verir: `"2018-01-01T08:00:00.0000000Z"`

*Örnek 2*

Bu örnek, bir zaman damgasını UTC'ye dönüştürür:

```
convertToUtc('01/01/2018 00:00:00', 'Pacific Standard Time', 'D')
```

Ve bu sonucu verir: `"Monday, January 1, 2018"`

<a name="createArray"></a>

### <a name="createarray"></a>createArray

Birden çok girişler bir dizi döndürür.
Tek giriş diziler için bkz: [array()](#array).

```
createArray('<object1>', '<object2>', ...)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*object1*>, <*object2*>,... | Evet | Herhangi biri karışık değil | Dizi oluşturmak için en az iki öğe |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| [<*object1*>, <*object2*>,...] | Dizi | Tüm giriş öğelerinden oluşan dizi |
||||

*Örnek*

Bu örnekte, bu girişler bir dizi oluşturur:

```
createArray('h', 'e', 'l', 'l', 'o')
```

Ve bu sonucu verir: `["h", "e", "l", "l", "o"]`

<a name="dataUri"></a>

### <a name="datauri"></a>dataUri

Bir veri Tekdüzen kaynak tanımlayıcısıdır (URI) bir dize döndürür.

```
dataUri('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | String | Dönüştürülecek dize. |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Veri URI'si*> | String | Girdi dizesi için veri URI'si |
||||

*Örnek*

Bu örnekte, "hello" dizesi için bir veri URI'si oluşturulur:

```
dataUri('hello')
```

Ve bu sonucu verir: `"data:text/plain;charset=utf-8;base64,aGVsbG8="`

<a name="dataUriToBinary"></a>

### <a name="datauritobinary"></a>dataUriToBinary

İkili dosya sürümü için bir veri Tekdüzen Kaynak Tanımlayıcısı (URI) döndürür.
Bu işlevi kullanmak yerine [decodeDataUri()](#decodeDataUri).
Her iki işlev aynı şekilde çalışır ancak `dataUriBinary()` tercih edilir.

```
dataUriToBinary('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | String | Dönüştürülecek veri URI'si |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*ikili için veri URI'si*> | String | Veri URI'si için ikili dosya sürümü |
||||

*Örnek*

Bu örnek, bu veri URI'si için bir ikili dosya sürümü oluşturur:

```
dataUriToBinary('data:text/plain;charset=utf-8;base64,aGVsbG8=')
```

Ve bu sonucu verir:

`"01100100011000010111010001100001001110100111010001100101011110000111010000101111011100000
1101100011000010110100101101110001110110110001101101000011000010111001001110011011001010111
0100001111010111010101110100011001100010110100111000001110110110001001100001011100110110010
10011011000110100001011000110000101000111010101100111001101100010010001110011100000111101"`

<a name="dataUriToString"></a>

### <a name="datauritostring"></a>dataUriToString

Bir veri Tekdüzen Kaynak Tanımlayıcısı (URI) dize sürümü döndürür.

```
dataUriToString('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | String | Dönüştürülecek veri URI'si |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*string-for-data-uri*> | String | Veri URI'si için dize sürümü |
||||

*Örnek*

Bu örnek, bu veri URI'si için bir dize oluşturur:

```
dataUriToString('data:text/plain;charset=utf-8;base64,aGVsbG8=')
```

Ve bu sonucu verir: `"hello"`

<a name="dayOfMonth"></a>

### <a name="dayofmonth"></a>dayOfMonth

Ayın gününü itibaren zaman damgası döndürür.

```
dayOfMonth('<timestamp>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | String | Zaman damgası içeren dize |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Ayın günü*> | Integer | Belirtilen zaman damgası ayın günü |
||||

*Örnek*

Bu örnek sayısı itibaren bu zaman damgası için ayın gününü döndürür:

```
dayOfMonth('2018-03-15T13:27:36Z')
```

Ve bu sonucu verir: `15`

<a name="dayOfWeek"></a>

### <a name="dayofweek"></a>dayOfWeek

Haftanın günü itibaren zaman damgası döndürür.

```
dayOfWeek('<timestamp>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | String | Zaman damgası içeren dize |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Haftanın günü*> | Integer | 1 ve benzeri Pazar 0, Pazartesi olduğu belirtilen zaman damgası öğesinden haftanın günü olduğu |
||||

*Örnek*

Bu örnek sayısı itibaren bu zaman damgası için haftanın gününü döndürür:

```
dayOfWeek('2018-03-15T13:27:36Z')
```

Ve bu sonucu verir: `3`

<a name="dayOfYear"></a>

### <a name="dayofyear"></a>dayOfYear

İtibaren zaman damgası yılın gününü döndürür.

```
dayOfYear('<timestamp>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | String | Zaman damgası içeren dize |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*yılın günü*> | Integer | Belirtilen zaman damgası yılın günü |
||||

*Örnek*

Bu örnekte itibaren bu zaman damgası yılın gününü verir:

```
dayOfYear('2018-03-15T13:27:36Z')
```

Ve bu sonucu verir: `74`

<a name="decodeBase64"></a>

### <a name="decodebase64"></a>decodeBase64

Base64 dizesi etkili bir şekilde kod çözme base64 ile kodlanmış bir dize, dize sürümü döndürür.
Kullanmayı [base64ToString()](#base64ToString) yerine `decodeBase64()`.
Her iki işlev aynı şekilde çalışır ancak `base64ToString()` tercih edilir.

```
decodeBase64('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | String | Kodu çözülecek base64 ile kodlanmış dize |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*kodu çözülen-base64-string*> | String | Base64 ile kodlanmış bir dize dizesi sürümü |
||||

*Örnek*

Bu örnek, base64 olarak kodlanmış bir dize için bir dize oluşturur:

```
decodeBase64('aGVsbG8=')
```

Ve bu sonucu verir: `"hello"`

<a name="decodeDataUri"></a>

### <a name="decodedatauri"></a>decodeDataUri

İkili dosya sürümü için bir veri Tekdüzen Kaynak Tanımlayıcısı (URI) döndürür.
Consider using [dataUriToBinary()](#dataUriToBinary), rather than `decodeDataUri()`.
Her iki işlev aynı şekilde çalışır ancak `dataUriToBinary()` tercih edilir.

```
decodeDataUri('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | String | ' % S'verisi URI dizesinin kodunu çözmek için |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*ikili için veri URI'si*> | String | Verisi URI dizesinin ikili sürümü |
||||

*Örnek*

Bu örnekte, bu veri URI'si için ikili dosya sürümü döndürür:

```
decodeDataUri('data:text/plain;charset=utf-8;base64,aGVsbG8=')
```

Ve bu sonucu verir:

`"01100100011000010111010001100001001110100111010001100101011110000111010000101111011100000
1101100011000010110100101101110001110110110001101101000011000010111001001110011011001010111
0100001111010111010101110100011001100010110100111000001110110110001001100001011100110110010
10011011000110100001011000110000101000111010101100111001101100010010001110011100000111101"`

<a name="decodeUriComponent"></a>

### <a name="decodeuricomponent"></a>Decodeurıcomponent

Değiştirir kodu çözülmüş sürümleriyle karakter kaçış bir dize döndürür.

```
decodeUriComponent('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | String | Kodu çözülecek kaçış karakterli dize |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*kodu çözülen URI'si*> | String | Kodu çözülmüş kaçış karakterleri ile güncelleştirilen dize |
||||

*Örnek*

Bu örnekte bu dize kaçış karakterleri ile kodu çözülen sürümlerini değiştirir:

```
decodeUriComponent('http%3A%2F%2Fcontoso.com')
```

Ve bu sonucu verir: `"https://contoso.com"`

<a name="div"></a>

### <a name="div"></a>div

Tamsayı sonucu, iki sayı arasındaki bölme işleminin sonucunu döndürür.
Kalanı sonucu elde etmek için bkz: [mod()](#mod).

```
div(<dividend>, <divisor>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Bölünen*> | Evet | Tamsayı veya kayan | Bölecek sayı *bölen* |
| <*bölen*> | Evet | Tamsayı veya kayan | Bölen sayı *bölünen*, 0 olamaz |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*sayının sonucu*> | Integer | İlk sayısı ile ikinci sayı bölen gelen tamsayı sonucu |
||||

*Örnek*

Örneklerin her ikisi de ikinci sayı ilk sayısı ayırın:

```
div(10, 5)
div(11, 5)
```

Ve bu sonucu döndürür: `2`

<a name="encodeUriComponent"></a>

### <a name="encodeuricomponent"></a>Encodeurıcomponent

URL güvenli olmayan karakterleri kaçış karakterleri ile değiştirerek bir Tekdüzen Kaynak Tanımlayıcısı (URI) ile kodlanmış sürümü bir dize döndürür.
Kullanmayı [uriComponent()](#uriComponent), yerine `encodeUriComponent()`.
Her iki işlev aynı şekilde çalışır ancak `uriComponent()` tercih edilir.

```
encodeUriComponent('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | String | URI ile kodlanmış biçimine dönüştürülecek dize. |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Kodlanmış uri*> | String | Kaçış karakterli URI ile kodlanmış dize |
||||

*Örnek*

Bu örnek, bu dize için bir URI ile kodlanmış sürümü oluşturur:

```
encodeUriComponent('https://contoso.com')
```

Ve bu sonucu verir: `"http%3A%2F%2Fcontoso.com"`

<a name="empty"></a>

### <a name="empty"></a>boş

Bir koleksiyonun boş olup olmadığını denetleyin.
Koleksiyon boş olduğunda true döndürür veya boş değil olduğunda false döndürür.

```
empty('<collection>')
empty([<collection>])
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Koleksiyon*> | Evet | Dize, dizi veya nesne | Denetlenecek koleksiyon |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| TRUE veya false | Boolean | Koleksiyon boş olduğunda true döndürür. Boş değilse, false döndürün. |
||||

*Örnek*

Bu örnekler, belirtilen koleksiyonların boş olup olmadığını denetleyin:

```
empty('')
empty('abc')
```

Ve bu sonuçları döndürür:

* İlk örnek: Boş bir dize işlevi döndürecek şekilde geçirir `true`.
* İkinci örnek: İşlev döndürecek şekilde "abc" dizesine geçirir `false`.

<a name="endswith"></a>

### <a name="endswith"></a>endsWith

Bir dizeyi belirli bir alt dizesi ile bitip bitmediğini kontrol edin.
Alt dize bulunduğunda true döndürür veya return false olduğunda nebyl nalezen.
Bu işlev büyük küçük harfe duyarlı değil.

```
endsWith('<text>', '<searchText>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Metin*> | Evet | String | Denetlenecek dize |
| <*Aramametni*> | Evet | String | Bitiş alt dizeyi Bul |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| TRUE veya false  | Boolean | Bitiş alt dizeyi bulunduğunda true döndürür. Ne zaman nebyl nalezen false döndürün. |
||||

*Örnek 1*

Bu örnekte, "hello world" dize "world" dizesi ile bitip bitmediğini denetler:

```
endsWith('hello world', 'world')
```

Ve bu sonucu verir: `true`

*Örnek 2*

Bu örnekte, "hello world" dize "evreni" dizesi ile bitip bitmediğini denetler:

```
endsWith('hello world', 'universe')
```

Ve bu sonucu verir: `false`

<a name="equals"></a>

### <a name="equals"></a>eşittir

Hem değer, ifadeler veya nesneleri eşdeğer olup olmadığını denetleyin.
Her ikisi de eşit veya eşdeğer bulunmasa false döndürün olduğunda true döndürür.

```
equals('<object1>', '<object2>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*object1*>, <*object2*> | Evet | Çeşitli | Değerleri, ifadeler veya karşılaştırmak için nesneleri |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| TRUE veya false | Boolean | Her ikisi de eşdeğer olduğunda true döndürür. Eşdeğer değil, false döndürün. |
||||

*Örnek*

Bu örnekler, belirtilen girişler eşdeğer olup olmadığını denetleyin.

```
equals(true, 1)
equals('abc', 'abcd')
```

Ve bu sonuçları döndürür:

* İlk örnek: İşlev döndürecek şekilde her iki değer eşdeğerdir `true`.
* İkinci örnek: Her iki değer işlev döndürecek şekilde eşdeğeri olmayan `false`.

<a name="first"></a>

### <a name="first"></a>ilk

Bir dize veya diziden ilk öğeyi döndürür.

```
first('<collection>')
first([<collection>])
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Koleksiyon*> | Evet | Dize veya dizi | Koleksiyon ilk öğeyi nerede bulacağını |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*ilk koleksiyon öğesi*> | Tüm | Koleksiyondaki ilk öğe |
||||

*Örnek*

Bu örnekler, bu koleksiyonlarında ilk öğeyi bulun:

```
first('hello')
first(createArray(0, 1, 2))
```

Ve bu sonuçlar döndürebilir:

* İlk örnek: `"h"`
* İkinci örnek: `0`

<a name="float"></a>

### <a name="float"></a>float

Bir kayan noktalı sayı için bir dize sürümü için gerçek kayan nokta sayısı dönüştürün.
Özel Parametreler uygulamaya, örneğin, bir mantıksal uygulamada veya akışta geçirirken, bu işlevi kullanabilirsiniz.

```
float('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | String | Dönüştürmek için geçerli bir kayan noktalı sayı olan dizesini |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*kayan nokta değeri*> | Float | Belirtilen dize için kayan noktalı sayı |
||||

*Örnek*

Bu örnek, bir dize sürüm için bu kayan noktalı sayı oluşturur:

```
float('10.333')
```

Ve bu sonucu verir: `10.333`

<a name="formatDateTime"></a>

### <a name="formatdatetime"></a>formatDateTime

Bir zaman damgası belirtilen biçimde döndürür.

```
formatDateTime('<timestamp>', '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | String | Zaman damgası içeren dize |
| <*Biçim*> | Hayır | String | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddTHH:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Getirilecek zaman damgası*> | String | Belirtilen biçimde güncelleştirilen zaman damgası |
||||

*Örnek*

Bu örnek, bir zaman damgası belirtilen biçimine dönüştürür:

```
formatDateTime('03/15/2018 12:00:00', 'yyyy-MM-ddTHH:mm:ss')
```

Ve bu sonucu verir: `"2018-03-15T12:00:00"`

<a name="formDataMultiValues"></a>

### <a name="formdatamultivalues"></a>formDataMultiValues

Bir eylem içinde bir anahtar adıyla eşleşen değerleri içeren bir dizi döndürür *form verisinin* veya *form kodlu* çıktı.

```
formDataMultiValues('<actionName>', '<key>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*actionName*> | Evet | String | Eylem çıktısı istediğiniz anahtar değerine sahip |
| <*Anahtarı*> | Evet | String | Değer, istediğiniz anahtarı adı |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| [<*dizi ile anahtar değerlerini*>] | Dizi | Belirtilen bir anahtarla eşleşen tüm değerleri içeren bir dizi |
||||

*Örnek*

Bu örnekte "Konu" anahtarının değerden belirtilen eylemin form-data veya form olarak kodlanan bir dizi oluşturur:

```
formDataMultiValues('Send_an_email', 'Subject')
```

Ve konu metnini, örneğin bir dizide döndürür: `["Hello world"]`

<a name="formDataValue"></a>

### <a name="formdatavalue"></a>formDataValue

Bir eylem içinde bir anahtar adı ile eşleşen tek bir değer döndürmesi *form verisinin* veya *form kodlu* çıktı.
İşlev, işlev birden fazla eşleşme bulunursa, bir hata oluşturur.

```
formDataValue('<actionName>', '<key>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*actionName*> | Evet | String | Eylem çıktısı istediğiniz anahtar değerine sahip |
| <*Anahtarı*> | Evet | String | Değer, istediğiniz anahtarı adı |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*anahtar-değer*> | String | Belirtilen anahtar değeri  |
||||

*Örnek*

Bu örnek bir dize "Konu" anahtarın değeri belirtilen eylemin form-data veya form kodlu çıktı oluşturur:

```
formDataValue('Send_an_email', 'Subject')
```

Ve konu metnini, örneğin bir dize olarak döndürür: `"Hello world"`

<a name="getFutureTime"></a>

### <a name="getfuturetime"></a>getFutureTime

Yanı sıra belirtilen zaman birimi geçerli zaman damgasını döndürür.

```
getFutureTime(<interval>, <timeUnit>, <format>?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*aralığı*> | Evet | Integer | Otomatik olarak çıkarmak için belirtilen zaman birimi |
| <*timeUnit*> | Evet | String | İle kullanılacak zaman birimi *aralığı*: "Saniye", "Minute", "Hour", "Day", "Week", "Month", "Year" |
| <*Biçim*> | Hayır | String | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddTHH:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Güncelleştirilen zaman damgası*> | String | Geçerli zaman damgasını artı belirtilen zaman birimi sayısı |
||||

*Örnek 1*

Geçerli zaman damgasını olduğunu varsayın "2018-03-01T00:00:00.0000000Z".
Bu örnek, zaman damgası için beş gün ekler:

```
getFutureTime(5, 'Day')
```

Ve bu sonucu verir: `"2018-03-06T00:00:00.0000000Z"`

*Örnek 2*

Geçerli zaman damgasını olduğunu varsayın "2018-03-01T00:00:00.0000000Z".
Bu örnekte, beş gün ekler ve sonucu "D" biçimine dönüştürür:

```
getFutureTime(5, 'Day', 'D')
```

Ve bu sonucu verir: `"Tuesday, March 6, 2018"`

<a name="getPastTime"></a>

### <a name="getpasttime"></a>getPastTime

Belirtilen zaman birimi eksi geçerli zaman damgasını döndürür.

```
getPastTime(<interval>, <timeUnit>, <format>?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*aralığı*> | Evet | Integer | Otomatik olarak çıkarmak için belirtilen zaman birimi |
| <*timeUnit*> | Evet | String | İle kullanılacak zaman birimi *aralığı*: "Saniye", "Minute", "Hour", "Day", "Week", "Month", "Year" |
| <*Biçim*> | Hayır | String | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddTHH:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Güncelleştirilen zaman damgası*> | String | Belirtilen zaman birimi sayısı eksi geçerli zaman damgası |
||||

*Örnek 1*

Geçerli zaman damgasını olduğunu varsayın "2018-02-01T00:00:00.0000000Z".
Bu örnek, zaman damgası beş günden çıkarır:

```
getPastTime(5, 'Day')
```

Ve bu sonucu verir: `"2018-01-27T00:00:00.0000000Z"`

*Örnek 2*

Geçerli zaman damgasını olduğunu varsayın "2018-02-01T00:00:00.0000000Z".
Bu örnekte, beş gün çıkarır ve sonucu "D" biçimine dönüştürür:

```
getPastTime(5, 'Day', 'D')
```

Ve bu sonucu verir: `"Saturday, January 27, 2018"`

<a name="greater"></a>

### <a name="greater"></a>daha büyük

İlk değer ikinci değerden büyük olup olmadığını denetleyin.
İlk değeri daha fazla olduğunda true döndürür veya ne zaman false döndürün daha az.

```
greater(<value>, <compareTo>)
greater('<value>', '<compareTo>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Tamsayı, kayan noktalı sayı veya dize | İkinci değerden büyük olup olmadığı denetlenecek ilk değer |
| <*compareTo*> | Evet | Tamsayı, kayan noktalı sayı veya dize, sırasıyla | Karşılaştırma değeri |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| TRUE veya false | Boolean | İlk değeri ikinci değerden daha büyük olduğunda true döndürür. İlk değer eşit veya değerinden ikinci düşük olduğunda false döndürür. |
||||

*Örnek*

Bu örneklerin ilk değer ikinci değerden büyük olup olmadığını denetleyin:

```
greater(10, 5)
greater('apple', 'banana')
```

Ve bu sonuçlar döndürebilir:

* İlk örnek: `true`
* İkinci örnek: `false`

<a name="greaterOrEquals"></a>

### <a name="greaterorequals"></a>greaterOrEquals

İlk değer ikinci değerine eşit veya daha büyük olup olmadığını denetleyin.
İlk değeri sıfırdan büyük veya eşit olduğunda true döndürür veya ilk değeri daha az olduğunda false döndürür.

```
greaterOrEquals(<value>, <compareTo>)
greaterOrEquals('<value>', '<compareTo>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Tamsayı, kayan noktalı sayı veya dize | İkinci değerine eşit veya daha büyük olup olmadığını denetlemek için ilk değer |
| <*compareTo*> | Evet | Tamsayı, kayan noktalı sayı veya dize, sırasıyla | Karşılaştırma değeri |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| TRUE veya false | Boolean | İlk değer ikinci değerine eşit veya daha büyük olduğunda true döndürür. İkinci değer'den küçük olan ilk değer olduğunda false döndürür. |
||||

*Örnek*

Bu örnekler, ilk değer ikinci değerinden büyük veya buna eşit olup olmadığını denetleyin:

```
greaterOrEquals(5, 5)
greaterOrEquals('apple', 'banana')
```

Ve bu sonuçlar döndürebilir:

* İlk örnek: `true`
* İkinci örnek: `false`

<a name="guid"></a>

### <a name="guid"></a>GUID

Örneğin, "c2ecc88d-88c8-4096-912c-d6f2e2b138ce" bir dize olarak bir genel benzersiz tanıtıcısı (GUID) oluşturur:

```
guid()
```

Ayrıca varsayılan çizgi ile ayrılmış 32 basamak "D" biçim dışında GUID için farklı bir biçim belirtin.

```
guid('<format>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Biçim*> | Hayır | String | Tek bir [biçim belirticisi](https://msdn.microsoft.com/library/97af8hh4) döndürülen GUID. Varsayılan olarak, "D" biçim olduğu halde "N", "D", "B", "P" veya "X" kullanabilirsiniz. |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*GUID değeri*> | String | Rastgele oluşturulmuş bir GUID |
||||

*Örnek*

Bu örnekte, aynı GUID oluşturur ancak çizgi ile 32 basamak ayrılmış ve parantez içine alınmış:

```
guid('P')
```

Ve bu sonucu verir: `"(c2ecc88d-88c8-4096-912c-d6f2e2b138ce)"`

<a name="if"></a>

### <a name="if"></a>if

İfadenin true veya false olup olmadığını denetleyin.
Sonuca bağlı, belirli bir değeri döndürme.

```
if(<expression>, <valueIfTrue>, <valueIfFalse>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*İfade*> | Evet | Boolean | Denetlenecek ifadesi |
| <*Koşul*> | Evet | Tüm | İfadenin true olduğunda döndürülecek değer |
| <*valueIfFalse*> | Evet | Tüm | İfade false olduğunda döndürülecek değer |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Belirtilen-dönüş değeri*> | Tüm | True veya false olup olmadığına göre bağlı döndüren belirtilen değeri ifadesi |
||||

*Örnek*

Bu örnekte döndürür `"yes"` çünkü belirtilen ifade true döndürür.
Aksi takdirde, örnek verir `"no"`:

```
if(equals(1, 1), 'yes', 'no')
```

<a name="indexof"></a>

### <a name="indexof"></a>indexOf

Başlangıç konumu veya bir alt dizin değerini döndürür.
Bu işlev büyük küçük harfe duyarlı değildir ve dizinleri sayısı 0 ile başlatın.

```
indexOf('<text>', '<searchText>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Metin*> | Evet | String | Bulunacak alt dizeyi içeren dize |
| <*Aramametni*> | Evet | String | Bulunacak alt dizeyi |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Dizin değeri*>| Integer | Belirtilen alt dizenin başlangıç konumunu veya dizin değeri. <p>Dize bulunamazsa numarası -1 döndürür. |
||||

*Örnek*

Bu örnekte başlangıç dizini değerini "hello world" dizesindeki "world" alt dizeyi bulur:

```
indexOf('hello world', 'world')
```

Ve bu sonucu verir: `6`

<a name="int"></a>

### <a name="int"></a>int

Tamsayı sürümü bir dize döndürür.

```
int('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | String | Dönüştürülecek dize. |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*tamsayı sonucu*> | Integer | Belirtilen dize tamsayı sürümü |
||||

*Örnek*

Bu örnek, "10" dizesi için bir tamsayı sürümünü oluşturur:

```
int('10')
```

Ve bu sonucu verir: `10`

<a name="item"></a>

### <a name="item"></a>Öğesi

Bir dizi üzerinde yinelenen bir eylem içinde kullanıldığında, geçerli öğeyi dizide eylemin geçerli yineleme sırasında döndürür.
Ayrıca, bu öğeye ait özellikleri değerleri alabilirsiniz.

```
item()
```

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Geçerli dizi öğesi*> | Tüm | Dizi eylemin geçerli yineleme için geçerli öğe |
||||

*Örnek*

Bu örnekte `body` öğesi için-her döngünün geçerli yineleme içinde "Send_an_email" eylemi için geçerli iletisi:

```
item().body
```

<a name="items"></a>

### <a name="items"></a>items

Her döngü için-her bir döngü içinde öğesinden geçerli öğeyi döndürür.
Bu işlev için-her bir döngü içinde kullanın.

```
items('<loopName>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*loopName*> | Evet | String | Ad için-her bir döngü için |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Öğesi*> | Tüm | Belirtilen için-her bir döngü geçerli döngüsünde öğesinden |
||||

*Örnek*

Bu örnek, geçerli öğenin belirtilen için-her bir döngü alır:

```
items('myForEachLoopName')
```

<a name="iterationIndexes"></a>

### <a name="iterationindexes"></a>iterationIndexes

Until döngü içinde geçerli yineleme için dizin değerini döndürür. Döngüler kadar iç içe geçmiş içinde bu işlevi kullanabilirsiniz. 

```
iterationIndexes('<loopName>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*loopName*> | Evet | String | Until döngü adı | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Dizin*> | Integer | Geçerli yineleme içinde belirtilen dizin değeri Until döngüsü | 
|||| 

*Örnek* 

Sayaç değeri beş ulaşana kadar bu örnek bir sayaç değişkeni ve artışlarla bu değişkenin tek bir Until döngü her yinelemede sırasında oluşturur. Örnek ayrıca, her yineleme için geçerli dizin izleyen bir değişken oluşturur. Until döngü, her yineleme sırasında örnek sayacını artırır ve sonra sayaç değeri için geçerli dizin değeri atar ve ardından sayaç artırılır. Herhangi bir zamanda geçerli dizin değerini alarak geçerli yineleme sayısını belirleyebilirsiniz.

```
{
   "actions": {
      "Create_counter_variable": {
         "type": "InitializeVariable",
         "inputs": {
            "variables": [ 
               {
                  "name": "myCounter",
                  "type": "Integer",
                  "value": 0
               }
            ]
         },
         "runAfter": {}
      },
      "Create_current_index_variable": {
         "type": "InitializeVariable",
         "inputs": {
            "variables": [
               {
                  "name": "myCurrentLoopIndex",
                  "type": "Integer",
                  "value": 0
               }
            ]
         },
         "runAfter": {
            "Create_counter_variable": [ "Succeeded" ]
         }
      },
      "Until": {
         "type": "Until",
         "actions": {
            "Assign_current_index_to_counter": {
               "type": "SetVariable",
               "inputs": {
                  "name": "myCurrentLoopIndex",
                  "value": "@variables('myCounter')"
               },
               "runAfter": {
                  "Increment_variable": [ "Succeeded" ]
               }
            },
            "Increment_variable": {
               "type": "IncrementVariable",
               "inputs": {
                  "name": "myCounter",
                  "value": 1
               },
               "runAfter": {}
            }
         },
         "expression": "@equals(variables('myCounter'), 5),
         "limit": {
            "count": 60,
            "timeout": "PT1H"
         },
         "runAfter": {
            "Create_current_index_variable": [ "Succeeded" ]
         }
      }
   }
}
```

<a name="json"></a>

### <a name="json"></a>json

JavaScript nesne gösterimi (JSON) türü değeri veya dize veya XML nesnesi döndürür.

```
json('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Dize veya XML | Dönüştürülecek dize veya XML |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*JSON sonuç*> | JSON yerel bir tür veya nesne | Nesne için belirtilen dize veya XML veya JSON yerel tür değeri. Dize null ise, işlev boş bir nesne döndürür. |
||||

*Örnek 1*

Bu örnekte bu dize JSON değerine dönüştürür:

```
json('[1, 2, 3]')
```

Ve bu sonucu verir: `[1, 2, 3]`

*Örnek 2*

Bu örnekte bu dize JSON değerine dönüştüren:

```
json('{"fullName": "Sophia Owen"}')
```

Ve bu sonucu verir:

```
{
  "fullName": "Sophia Owen"
}
```

*Örnek 3*

Bu örnekte bu XML, JSON değerine dönüştüren:

```
json(xml('<?xml version="1.0"?> <root> <person id='1'> <name>Sophia Owen</name> <occupation>Engineer</occupation> </person> </root>'))
```

Ve bu sonucu verir:

```json
{
   "?xml": { "@version": "1.0" },
   "root": {
      "person": [ {
         "@id": "1",
         "name": "Sophia Owen",
         "occupation": "Engineer"
      } ]
   }
}
```

<a name="intersection"></a>

### <a name="intersection"></a>kesişimi

Sahip bir koleksiyonun dönüş *yalnızca* belirtilen koleksiyonlarla arasında ortak öğeleri.
Sonuçta görüntülenmek üzere bir öğe bu işleve geçirilen tüm koleksiyonlarda yer almalıdır.
Bir veya daha fazla öğe aynı ada sahipse, bu ada sahip son öğeyi sonuçta da görüntülenir.

```
intersection([<collection1>], [<collection2>], ...)
intersection('<collection1>', '<collection2>', ...)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Collection1*>, <*collection2*>,... | Evet | Dizi veya nesne ancak ikisini birden değil | Öğesinden, istediğiniz koleksiyonları *yalnızca* ortak öğeler |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Ortak öğeler*> | Dizi veya nesne, sırasıyla | Belirtilen Koleksiyonlar boyunca yalnızca ortak öğeler içeren bir koleksiyon |
||||

*Örnek*

Bu örnekte, bu dizileri arasında ortak öğeleri bulur:

```
intersection(createArray(1, 2, 3), createArray(101, 2, 1, 10), createArray(6, 8, 1, 2))
```

Ve bir dizi döndürür *yalnızca* bu öğeler: `[1, 2]`

<a name="join"></a>

### <a name="join"></a>join

Bir dizideki tüm öğeler ve her karakter sahip bir dize ayırarak döndürülecek bir *sınırlayıcı*.

```
join([<collection>], '<delimiter>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Koleksiyon*> | Evet | Dizi | Katılmak için öğeleri içeren bir dizi |
| <*Sınırlayıcı*> | Evet | String | Sonuç dizesindeki her karakter arasında görünür ayırıcı |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*char1*><*sınırlayıcı*><*char2*><*sınırlayıcı*>... | String | Belirtilen dizideki tüm öğeler oluşturulan Sonuç dizesini |
||||

*Örnek*

Bu örnek bir dize sınırlayıcı belirtilen karakter ile bu dizideki öğelerden oluşturur:

```
join(createArray('a', 'b', 'c'), '.')
```

Ve bu sonucu verir: `"a.b.c"`

<a name="last"></a>

### <a name="last"></a>Son

Bir koleksiyondaki son öğeyi döndürür.

```
last('<collection>')
last([<collection>])
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Koleksiyon*> | Evet | Dize veya dizi | Koleksiyonun son öğeyi nerede bulacağını |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Son koleksiyon öğesi*> | Dize veya dizi, sırasıyla | Koleksiyondaki son öğe |
||||

*Örnek*

Bu örnekler, bu koleksiyonlarında son öğeyi bulun:

```
last('abcd')
last(createArray(0, 1, 2, 3))
```

Ve bu sonuçları döndürür:

* İlk örnek: `"d"`
* İkinci örnek: `3`

<a name="lastindexof"></a>

### <a name="lastindexof"></a>lastIndexOf

Başlangıç konumu veya bir alt dizenin son oluşum dizin değerini döndürür.
Bu işlev büyük küçük harfe duyarlı değildir ve dizinleri sayısı 0 ile başlatın.

```
lastIndexOf('<text>', '<searchText>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Metin*> | Evet | String | Bulunacak alt dizeyi içeren dize |
| <*Aramametni*> | Evet | String | Bulunacak alt dizeyi |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Bitiş dizin değeri*> | Integer | Son oluşum belirtilen alt dizenin başlangıç konumunu veya dizin değeri. <p>Dize bulunamazsa numarası -1 döndürür. |
||||

*Örnek*

Bu örnekte, "hello world" dizesindeki "world" alt dizenin son a geçişi için başlangıç dizini değerini bulur:

```
lastIndexOf('hello world', 'world')
```

Ve bu sonucu verir: `6`

<a name="length"></a>

### <a name="length"></a>length

Bir koleksiyondaki öğe sayısını döndürür.

```
length('<collection>')
length([<collection>])
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Koleksiyon*> | Evet | Dize veya dizi | Koleksiyona sahip öğelerin sayısı |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Uzunluk-veya-count*> | Integer | Koleksiyondaki öğe sayısı |
||||

*Örnek*

Bu örneklerde bu koleksiyonlardaki öğe sayısı:

```
length('abcd')
length(createArray(0, 1, 2, 3))
```

Ve bu sonucu döndürür: `4`

<a name="less"></a>

### <a name="less"></a>daha az

İkinci değer ilk değer olup değerinden denetleyin.
İlk değer daha az olduğunda true döndürür veya ilk değeri daha fazla olduğunda false döndürür.

```
less(<value>, <compareTo>)
less('<value>', '<compareTo>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Tamsayı, kayan noktalı sayı veya dize | İlk değer kısa olup olmadığını denetlemek için ikinci değer |
| <*compareTo*> | Evet | Tamsayı, kayan noktalı sayı veya dize, sırasıyla | Karşılaştırma öğesi |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| TRUE veya false | Boolean | İkinci değer'den küçük olan ilk değer olduğunda true döndürür. İlk değeri ikinci değerden büyük veya eşit olduğunda false döndürür. |
||||

*Örnek*

Bu örnekler, ikinci değer ilk değer olup değerinden denetleyin.

```
less(5, 10)
less('banana', 'apple')
```

Ve bu sonuçlar döndürebilir:

* İlk örnek: `true`
* İkinci örnek: `false`

<a name="lessOrEquals"></a>

### <a name="lessorequals"></a>lessOrEquals

İlk değer ya da ikinci değer eşit olup olmadığını denetleyin.
İlk değer küçük veya eşit olduğunda true döndürür veya ilk değeri daha fazla olduğunda false döndürür.

```
lessOrEquals(<value>, <compareTo>)
lessOrEquals('<value>', '<compareTo>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Tamsayı, kayan noktalı sayı veya dize | Küçüktür olup olmadığını denetlemek için ilk değer veya ikinci değer eşitliği |
| <*compareTo*> | Evet | Tamsayı, kayan noktalı sayı veya dize, sırasıyla | Karşılaştırma öğesi |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| TRUE veya false  | Boolean | İlk değer ikinci değerine eşit veya daha az olduğunda true döndürür. İlk değeri ikinci değerden daha büyük olduğunda false döndürür. |
||||

*Örnek*

Bu örnekler, ilk değer ikinci değerinden küçük veya buna eşit olup olmadığını denetleyin.

```
lessOrEquals(10, 10)
lessOrEquals('apply', 'apple')
```

Ve bu sonuçlar döndürebilir:

* İlk örnek: `true`
* İkinci örnek: `false`

<a name="listCallbackUrl"></a>

### <a name="listcallbackurl"></a>listCallbackUrl

"Bir tetikleyici veya eylemi çağırır geri çağırma URL'si" döndürür.
Bu işlev, tetikleyiciler ve Eylemler için birlikte çalışır **HttpWebhook** ve **ApiConnectionWebhook** bağlayıcı türleri, ancak **el ile**,  **Yinelenme**, **HTTP**, ve **APIConnection** türleri.

```
listCallbackUrl()
```

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*geri çağırma URL'si*> | String | Bir tetikleyici veya eylem için geri çağırma URL'si |
||||

*Örnek*

Bu örnek, bu işlev döndürebilir örnek geri çağırma URL'sini gösterir:

`"https://prod-01.westus.logic.azure.com:443/workflows/<*workflow-ID*>/triggers/manual/run?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=<*signature-ID*>"`

<a name="max"></a>

### <a name="max"></a>en fazla

Bir listeden veya dizi sayılarla her iki uçta da dahil en yüksek değeri döndürür.

```
max(<number1>, <number2>, ...)
max([<number1>, <number2>, ...])
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Sayı1*>, <*sayı2*>,... | Evet | Tamsayı, kayan nokta veya her ikisini de | En yüksek değerini istediğiniz sayı kümesini |
| [<*Sayı1*>, <*sayı2*>,...] | Evet | Dizi - tamsayı, kayan nokta veya her ikisini de | En yüksek değerini istediğiniz sayı dizisi |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*max-value*> | Tamsayı veya kayan | Belirtilen dizi ya da dizi sayının en yüksek değeri |
||||

*Örnek*

Bu örnekler, sayı ve bir dizi kümesinden en yüksek değeri alın:

```
max(1, 2, 3)
max(createArray(1, 2, 3))
```

Ve bu sonucu döndürür: `3`

<a name="min"></a>

### <a name="min"></a>dk

En düşük değer bir dizi numaraları veya bir dizi döndürür.

```
min(<number1>, <number2>, ...)
min([<number1>, <number2>, ...])
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Sayı1*>, <*sayı2*>,... | Evet | Tamsayı, kayan nokta veya her ikisini de | En düşük değerini istediğiniz sayı kümesini |
| [<*Sayı1*>, <*sayı2*>,...] | Evet | Dizi - tamsayı, kayan nokta veya her ikisini de | En düşük değerini istediğiniz sayı dizisi |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*en düşük değer*> | Tamsayı veya kayan | En düşük değer belirtilen sayı dizisi veya belirtilen dizi |
||||

*Örnek*

Bu örnekler, en düşük değer sayı ve bir dizi kümesinde alın:

```
min(1, 2, 3)
min(createArray(1, 2, 3))
```

Ve bu sonucu döndürür: `1`

<a name="mod"></a>

### <a name="mod"></a>mod

Kalan iki sayı arasındaki bölme işleminin sonucunu döndürür.
Tamsayı sonucu elde etmek için bkz: [div()](#div).

```
mod(<dividend>, <divisor>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Bölünen*> | Evet | Tamsayı veya kayan | Bölecek sayı *bölen* |
| <*bölen*> | Evet | Tamsayı veya kayan | Bölen sayı *bölünen*, 0 olamaz. |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Modül sonucu*> | Tamsayı veya kayan | Kalan'ın ilk sayısı ikinci sayı ile bölme |
||||

*Örnek*

Bu örnekte, ikinci sayı ilk sayıyı böler:

```
mod(3, 2)
```

Ve bu sonucu döndürür: `1`

<a name="mul"></a>

### <a name="mul"></a>mul

Ürün iki sayı arasındaki çarpma işleminin sonucunu döndürür.

```
mul(<multiplicand1>, <multiplicand2>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*multiplicand1*> | Evet | Tamsayı veya kayan | Tarafından çarpılacağı sayı *multiplicand2* |
| <*multiplicand2*> | Evet | Tamsayı veya kayan | Sayı, katları *multiplicand1* |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Ürün sonucu*> | Tamsayı veya kayan | İkinci sayı ilk sayısı çarparak gelen ürün |
||||

*Örnek*

Bu örnekler ikinci sayı birden fazla ilk sayısı:

```
mul(1, 2)
mul(1.5, 2)
```

Ve bu sonuçlar döndürebilir:

* İlk örnek: `2`
* İkinci örnek `3`

<a name="multipartBody"></a>

### <a name="multipartbody"></a>multipartBody

Birden çok bölümü olan bir eylemin çıkış belirli bir parçanın gövdesini döndürür.

```
multipartBody('<actionName>', <index>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*actionName*> | Evet | String | Birden çok bölümleriyle çıktısı bir eylemin adı |
| <*Dizin*> | Evet | Integer | Dizin değeri istediğiniz bölümü |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Gövde*> | String | Belirtilen bir parçanın gövdesini |
||||

<a name="not"></a>

### <a name="not"></a>değil

Bir ifadenin false olup olmadığını denetleyin.
İfade false olduğunda true döndürür veya true olduğunda false döndürür.

```json
not(<expression>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*İfade*> | Evet | Boolean | Denetlenecek ifadesi |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| TRUE veya false | Boolean | İfade false olduğunda true döndürür. İfadenin true olduğunda false döndürür. |
||||

*Örnek 1*

Bu örnekler, belirtilen ifadenin false olup olmadığını denetleyin:

```json
not(false)
not(true)
```

Ve bu sonuçlar döndürebilir:

* İlk örnek: İşlev döndürecek şekilde ifadeyi false ise `true`.
* İkinci örnek: İşlev döndürecek şekilde ifadeyi true ise `false`.

*Örnek 2*

Bu örnekler, belirtilen ifadenin false olup olmadığını denetleyin:

```json
not(equals(1, 2))
not(equals(1, 1))
```

Ve bu sonuçlar döndürebilir:

* İlk örnek: İşlev döndürecek şekilde ifadeyi false ise `true`.
* İkinci örnek: İşlev döndürecek şekilde ifadeyi true ise `false`.

<a name="or"></a>

### <a name="or"></a>or

En az bir ifadenin doğru olup olmadığını denetleyin.
En az bir ifade true olduğunda true döndürür veya tümü false olduğunda false döndürür.

```
or(<expression1>, <expression2>, ...)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*İfade1*>, <*expression2*>,... | Evet | Boolean | Denetlenecek ifadeleri |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| TRUE veya false | Boolean | En az bir ifade true olduğunda true döndürür. Tüm ifadeler false olduğunda false döndürür. |
||||

*Örnek 1*

Bu örnekler, en az bir ifadenin doğru olup olmadığını denetleyin:

```json
or(true, false)
or(false, false)
```

Ve bu sonuçlar döndürebilir:

* İlk örnek: İşlev döndürecek şekilde en az bir ifade true ise `true`.
* İkinci örnek: İşlev döndürecek şekilde her iki ifade false `false`.

*Örnek 2*

Bu örnekler, en az bir ifadenin doğru olup olmadığını denetleyin:

```json
or(equals(1, 1), equals(1, 2))
or(equals(1, 2), equals(1, 3))
```

Ve bu sonuçlar döndürebilir:

* İlk örnek: İşlev döndürecek şekilde en az bir ifade true ise `true`.
* İkinci örnek: İşlev döndürecek şekilde her iki ifade false `false`.

<a name="parameters"></a>

### <a name="parameters"></a>parametreler

Tanımlanan bir parametre için değer iş akışı tanımınızı döndürün.

```
parameters('<parameterName>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*parameterName*> | Evet | String | Değeri, istediğiniz parametrenin adı |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*parametre değeri*> | Tüm | Belirtilen parametre değeri |
||||

*Örnek*

Bu JSON değer olduğunu varsayın:

```json
{
  "fullName": "Sophia Owen"
}
```

Bu örnek belirtilen parametre değeri alır:

```
parameters('fullName')
```

Ve bu sonucu verir: `"Sophia Owen"`

<a name="rand"></a>

### <a name="rand"></a>rand

Yalnızca başlangıç sonunda dahil belirtilen bir aralıktan rastgele bir tamsayı döndürür.

```
rand(<minValue>, <maxValue>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*minValue*> | Evet | Integer | Aralıktaki en küçük tamsayı |
| <*maxValue*> | Evet | Integer | İşlev döndürebilen aralığında en yüksek tamsayı izleyen tamsayı |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*rastgele bir sonuç*> | Integer | Belirtilen aralıktan döndürülen rastgele tamsayı |
||||

*Örnek*

Bu örnek, rastgele bir tamsayı en büyük değeri hariç belirtilen aralıktan alır:

```
rand(1, 5)
```

Ve sonuç olarak bu numaraları birini döndürür: `1`, `2`, `3`, veya `4`

<a name="range"></a>

### <a name="range"></a>Aralığı

Belirtilen bir tamsayı başlayan bir tamsayı dizisi döndürür.

```
range(<startIndex>, <count>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*startIndex*> | Evet | Integer | Dizinin ilk öğesi olarak başlayan bir tamsayı değeri |
| <*Sayısı*> | Evet | Integer | Dizideki tamsayıların sayısı |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| [<*aralığı sonucu*>] | Dizi | Belirtilen dizinden başlayarak tamsayılar dizi |
||||

*Örnek*

Bu örnek belirtilen dizinden başlatır ve belirtilen sayıda tamsayı olan bir tamsayı dizisi oluşturur:

```
range(1, 4)
```

Ve bu sonucu verir: `[1, 2, 3, 4]`

<a name="replace"></a>

### <a name="replace"></a>Değiştir

Belirtilen dizenin bir alt dizenin yerini ve sonuç dizeyi döndürür. Bu işlev büyük/küçük harf duyarlıdır.

```
replace('<text>', '<oldText>', '<newText>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Metin*> | Evet | String | Alt dizeyi değiştirilecek olan dize |
| <*YeniMetin*> | Evet | String | Alt dizeyi değiştirin |
| <*newText*> | Evet | String | Değiştirme dizesi |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Güncelleştirilen metin*> | String | Alt dizeyi değiştirdikten sonra güncelleştirilmiş dize <p>Alt dize bulunamazsa, orijinal dizeyi döndürür. |
||||

*Örnek*

Bu örnek, "eski dizesinde" "eski" alt dizeyi bulur ve "eski"Yeni"ile" değiştirir:

```
replace('the old string', 'old', 'new')
```

Ve bu sonucu verir: `"the new string"`

<a name="removeProperty"></a>

### <a name="removeproperty"></a>removeProperty

Bir nesneden bir özelliğini kaldırın ve güncelleştirilmiş nesneyi döndürür.

```
removeProperty(<object>, '<property>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*object*> | Evet | Object | Bir özelliği kaldırmak istediğiniz JSON nesnesi |
| <*Özelliği*> | Evet | String | Kaldırılacak özelliğin adı |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*güncelleştirilmiş nesne*> | Object | Belirtilen özellik olmadan güncelleştirilmiş JSON nesnesi |
||||

*Örnek*

Bu örnek kaldırır `"accountLocation"` özelliğinden bir `"customerProfile"` JSON ile dönüştürülür nesnesi [JSON()](#json) işlev ve güncelleştirilmiş nesne döndürür:

```
removeProperty(json('customerProfile'), 'accountLocation')
```

<a name="setProperty"></a>

### <a name="setproperty"></a>setProperty

Bir nesnenin özellik değerini ayarlayın ve güncelleştirilmiş nesneyi döndürür.
Yeni bir özellik eklemek için bu işlevi kullanabilirsiniz veya [addProperty()](#addProperty) işlevi.

```
setProperty(<object>, '<property>', <value>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*object*> | Evet | Object | Özelliği ayarlamak istiyorsanız JSON nesnesi |
| <*Özelliği*> | Evet | String | Ayarlamak için mevcut veya yeni özellik adı |
| <*Değer*> | Evet | Tüm | Belirtilen özellik için ayarlanacak değer |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*güncelleştirilmiş nesne*> | Object | Özelliği, ayarladığınız güncelleştirilmiş JSON nesnesi |
||||

*Örnek*

Bu örnekte ayarlar `"accountNumber"` özelliği bir `"customerProfile"` JSON ile dönüştürülür nesnesi [JSON()](#json) işlevi.
İşlev tarafından oluşturulan bir değer atar [guid()](#guid) işlev ve güncelleştirilmiş bir JSON nesnesi döndürür:

```
setProperty(json('customerProfile'), 'accountNumber', guid())
```

<a name="skip"></a>

### <a name="skip"></a>Atla

Bir koleksiyonun önünden öğeleri kaldırıp dönüş *diğer tüm* öğeleri.

```
skip([<collection>], <count>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Koleksiyon*> | Evet | Dizi | Öğeleri kaldırmak istediğiniz koleksiyonu |
| <*Sayısı*> | Evet | Integer | Önündeki kaldırılacak öğe sayısı için pozitif bir tamsayı |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| [<*koleksiyon güncelleştirildi*>] | Dizi | Belirtilen öğeleri kaldırdıktan sonra güncelleştirilmiş koleksiyonu |
||||

*Örnek*

Bu örnek, bir öğe, ' % s'sayısı 0, belirtilen dizinin Önden kaldırır:

```
skip(createArray(0, 1, 2, 3), 1)
```

Bu dizinin kalan öğeleri döndürür: `[1,2,3]`

<a name="split"></a>

### <a name="split"></a>split

Belirtilen sınırlayıcı karakter özgün dizedeki göre virgülle ayrılmış bir alt dizeler, içeren bir dizi döndürür.

```
split('<text>', '<delimiter>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Metin*> | Evet | String | Özgün dizedeki belirtilen sınırlayıcıya göre alt dizelere ayrılacak dize |
| <*Sınırlayıcı*> | Evet | String | Ayırıcı olarak kullanılacak özgün dizedeki karakter |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| [<*substring1*>, <*substring2*>,...] | Dizi | Virgülle ayırarak orijinal dizeden alt dizeleri içeren bir dizi |
||||

*Örnek*

Bu örnekte alt dizeler ayırıcı olarak belirtilen karakter göre belirtilen dizenin bir dizi oluşturur:

```
split('a_b_c', '_')
```

Ve sonuç olarak bu dizinin döndürür: `["a","b","c"]`

<a name="startOfDay"></a>

### <a name="startofday"></a>startOfDay

Bir zaman damgası için günün başlangıcını döndürür.

```
startOfDay('<timestamp>', '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | String | Zaman damgası içeren dize |
| <*Biçim*> | Hayır | String | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddTHH:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Güncelleştirilen zaman damgası*> | String | Belirtilen zaman damgası ancak gün için sıfır saat işaretinden başlayarak |
||||

*Örnek*

Bu örnekte, bu zaman damgası için günün başlangıcını bulur:

```
startOfDay('2018-03-15T13:30:30Z')
```

Ve bu sonucu verir: `"2018-03-15T00:00:00.0000000Z"`

<a name="startOfHour"></a>

### <a name="startofhour"></a>startOfHour

Bir zaman damgası için saatin başlangıcını döndürür.

```
startOfHour('<timestamp>', '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | String | Zaman damgası içeren dize |
| <*Biçim*> | Hayır | String | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddTHH:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Güncelleştirilen zaman damgası*> | String | Belirtilen zaman damgası ancak saat için sıfır dakikalık işaretinde başlatılıyor |
||||

*Örnek*

Bu örnekte, bu zaman damgası için saatin başlangıcını bulur:

```
startOfHour('2018-03-15T13:30:30Z')
```

Ve bu sonucu verir: `"2018-03-15T13:00:00.0000000Z"`

<a name="startOfMonth"></a>

### <a name="startofmonth"></a>startOfMonth

Bir zaman damgası için ayın başlangıcını döndürür.

```
startOfMonth('<timestamp>', '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | String | Zaman damgası içeren dize |
| <*Biçim*> | Hayır | String | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddTHH:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Güncelleştirilen zaman damgası*> | String | Belirtilen zaman damgası ancak sıfır saat işaretinden, ayın ilk günü başlayan |
||||

*Örnek*

Bu örnekte, bu zaman damgası için ayın başlangıcını döndürür:

```
startOfMonth('2018-03-15T13:30:30Z')
```

Ve bu sonucu verir: `"2018-03-01T00:00:00.0000000Z"`

<a name="startswith"></a>

### <a name="startswith"></a>startsWith

Bir dizeyi belirli bir alt dizesi ile başlayıp başlamadığını kontrol edin.
Alt dize bulunduğunda true döndürür veya return false olduğunda nebyl nalezen.
Bu işlev büyük küçük harfe duyarlı değil.

```
startsWith('<text>', '<searchText>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Metin*> | Evet | String | Denetlenecek dize |
| <*Aramametni*> | Evet | String | Bulmak için başlangıç dizesi |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| TRUE veya false  | Boolean | Başlangıç substring bulunduğunda true döndürür. Ne zaman nebyl nalezen false döndürün. |
||||

*Örnek 1*

Bu örnekte, "hello world" dize "hello" alt dizesi ile başlayıp başlamadığını denetler:

```
startsWith('hello world', 'hello')
```

Ve bu sonucu verir: `true`

*Örnek 2*

Bu örnekte, "hello world" dize "greetings" alt dizesi ile başlayıp başlamadığını denetler:

```
startsWith('hello world', 'greetings')
```

Ve bu sonucu verir: `false`

<a name="string"></a>

### <a name="string"></a>string

Dize sürümü için bir değer döndürür.

```
string(<value>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Tüm | Dönüştürülecek değer |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*dize değeri*> | String | Belirtilen değer dizesi sürümü |
||||

*Örnek 1*

Bu örnek, bu sayı dize sürümü oluşturur:

```
string(10)
```

Ve bu sonucu verir: `"10"`

*Örnek 2*

Bu örnek belirtilen JSON nesnesinin bir dize oluşturur ve eğik çizgi kullanır (\\) çift tırnak işareti (") için bir kaçış karakteri olarak.

```
string( { "name": "Sophie Owen" } )
```

Ve bu sonucu verir: `"{ \\"name\\": \\"Sophie Owen\\" }"`

<a name="sub"></a>

### <a name="sub"></a>sub

İlk numarasından ikinci sayı arasındaki çıkarma işleminin sonucu döndürür.

```
sub(<minuend>, <subtrahend>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Eksilen*> | Evet | Tamsayı veya kayan | İçinden çıkarılacak sayı *çıkarılan* |
| <*çıkarılan*> | Evet | Tamsayı veya kayan | Den çıkarılacak sayı *Eksilen* |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Sonuç*> | Tamsayı veya kayan | İlk sayı ikinci numarasından çıkararak gelen sonucu |
||||

*Örnek*

Bu örnekte, ikinci sayı ilk sayısından çıkarır:

```
sub(10.3, .3)
```

Ve bu sonucu verir: `10`

<a name="substring"></a>

### <a name="substring"></a>alt dize

Dönüş karakteri bir dizeden belirtilen konumu veya dizin başlatılıyor.
Sayı 0 ile değerleri başlangıç dizini.

```
substring('<text>', <startIndex>, <length>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Metin*> | Evet | String | İstediğiniz karakter dizesi |
| <*startIndex*> | Evet | Integer | Başlangıç konumu veya dizin değeri olarak kullanmak istediğiniz 0'dan büyük veya eşit pozitif bir sayı |
| <*Uzunluğu*> | Evet | Integer | İçinde alt dizenin istediğiniz karakter pozitif bir sayı |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*alt dize sonucu*> | String | Belirtilen sayıda karakteri, kaynak dizedeki belirtilen dizinden başlayarak bir dizeyle |
||||

*Örnek*

Bu örnekte 6 dizin değerinden başlayarak belirtilen dizeyi beş karakterli alt dizeyi oluşturur:

```
substring('hello world', 6, 5)
```

Ve bu sonucu verir: `"world"`

<a name="subtractFromTime"></a>

### <a name="subtractfromtime"></a>subtractFromTime

Bir zaman damgası saat birimleri sayısı çıkarın.
Ayrıca bkz: [getPastTime](#getPastTime).

```
subtractFromTime('<timestamp>', <interval>, '<timeUnit>', '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | String | Zaman damgası içeren dize |
| <*aralığı*> | Evet | Integer | Otomatik olarak çıkarmak için belirtilen zaman birimi |
| <*timeUnit*> | Evet | String | İle kullanılacak zaman birimi *aralığı*: "Saniye", "Minute", "Hour", "Day", "Week", "Month", "Year" |
| <*Biçim*> | Hayır | String | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddTHH:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Güncelleştirilen zaman damgası*> | String | Belirtilen zaman birimi sayısı eksi zaman damgası |
||||

*Örnek 1*

Bu örnekte, bu zaman damgasından günden çıkarır:

```
subtractFromTime('2018-01-02T00:00:00Z', 1, 'Day')
```

Ve bu sonucu verir: `"2018-01-01T00:00:00:0000000Z"`

*Örnek 2*

Bu örnekte, bu zaman damgasından günden çıkarır:

```
subtractFromTime('2018-01-02T00:00:00Z', 1, 'Day', 'D')
```

Ve isteğe bağlı "D" biçimini kullanarak bu sonucunu döndürür: `"Monday, January, 1, 2018"`

<a name="take"></a>

### <a name="take"></a>sınav zamanı

Bir koleksiyonun önünden öğelerini döndürür.

```
take('<collection>', <count>)
take([<collection>], <count>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Koleksiyon*> | Evet | Dize veya dizi | Kullanmak istediğiniz öğeleri koleksiyonu |
| <*Sayısı*> | Evet | Integer | Önünden istediğiniz öğeleri sayısı pozitif bir tamsayı |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*alt*> ya da [<*alt*>] | Dize veya dizi, sırasıyla | Bir dize veya belirtilen sayıda öğeyi özgün koleksiyonun önünden gerçekleştirilecek olan bir dizi |
||||

*Örnek*

Bu örnekler, bu koleksiyonun önünden belirtilen sayıda öğeyi alın:

```
take('abcde', 3)
take(createArray(0, 1, 2, 3, 4), 3)
```

Ve bu sonuçlar döndürebilir:

* İlk örnek: `"abc"`
* İkinci örnek: `[0, 1, 2]`

<a name="ticks"></a>

### <a name="ticks"></a>saat döngüsü

Dönüş `ticks` belirtilen bir zaman damgası için özellik değeri.
A *değer çizgisi* 100 nanosaniyelik aralık.

```
ticks('<timestamp>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | String | Dize için bir zaman damgası |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*saat döngüsü numarası*> | Integer | Belirtilen zaman damgası beri tıklarının sayısını |
||||

<a name="toLower"></a>

### <a name="tolower"></a>toLower

Küçük harf biçiminde bir dize döndürür. Dizesindeki bir karakter, bir küçük harf sürüm yoksa, bu karakter döndürülen dizeye değiştirilmeden kalır.

```
toLower('<text>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Metin*> | Evet | String | Dizeyi küçük harfli biçiminde döndürmek için |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*küçük metin*> | String | Küçük harf biçimde özgün dizeye |
||||

*Örnek*

Bu örnekte bu dizeyi küçük harfe dönüştürür:

```
toLower('Hello World')
```

Ve bu sonucu verir: `"hello world"`

<a name="toUpper"></a>

### <a name="toupper"></a>toUpper

Büyük harf biçiminde bir dize döndürür. Dizesindeki bir karakter, bir büyük harfli sürümünü yoksa, bu karakter döndürülen dizeye değiştirilmeden kalır.

```
toUpper('<text>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Metin*> | Evet | String | Dizeyi büyük harfli biçiminde döndürmek için |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*büyük harfli metin*> | String | Orijinal dizeyi büyük harfli biçimde |
||||

*Örnek*

Bu örnekte bu dizeyi büyük harfe dönüştürür:

```
toUpper('Hello World')
```

Ve bu sonucu verir: `"HELLO WORLD"`

<a name="trigger"></a>

### <a name="trigger"></a>Tetikleyici

Bir ifadeyi atayabilirsiniz diğer JSON ad ve değer çiftleri gelen bir tetikleyicinin çıkış çalışma zamanı veya değerleri döndürür.

* Bir tetikleyici girişleri içinde bu işlev, önceki yürütmeyi çıkışı döndürür.

* Bir tetikleyici koşul içinde bu işlev, geçerli yürütülmesini çıkışı döndürür.

Varsayılan olarak, tüm tetikleyici nesnesi işlev başvuruyor, ancak isteğe bağlı olarak bir özellik belirtin, istediğiniz değeri.
Ayrıca, bu işlev sahip toplu sürümleri kullanılabilir, bkz: [triggerOutputs()](#triggerOutputs) ve [triggerBody()](#triggerBody).

```
trigger()
```

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Tetikleyici çıkışı*> | String | Çalışma zamanında bir tetikleyici çıkışı |
||||

<a name="triggerBody"></a>

### <a name="triggerbody"></a>triggerBody

Bir tetikleyicinin dönüş `body` çalışma zamanında çıktı.
İçin Toplu özellik `trigger().outputs.body`.
Bkz: [trigger()](#trigger).

```
triggerBody()
```

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Tetikleyici gövdesi çıkış*> | String | `body` Tetikleyiciden çıkış |
||||

<a name="triggerFormDataMultiValues"></a>

### <a name="triggerformdatamultivalues"></a>triggerFormDataMultiValues

Bir tetikleyici içinde bir anahtar adıyla eşleşen değerleri içeren bir dizi döndürür *form verisinin* veya *form kodlu* çıktı.

```
triggerFormDataMultiValues('<key>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Anahtarı*> | Evet | String | Değer, istediğiniz anahtarı adı |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| [<*dizi ile anahtar değerlerini*>] | Dizi | Belirtilen bir anahtarla eşleşen tüm değerleri içeren bir dizi |
||||

*Örnek*

Bu örnek, bir RSS tetikleyicinin form-data veya form kodlu çıkış "feedUrl" anahtar değerinden bir dizi oluşturur:

```
triggerFormDataMultiValues('feedUrl')
```

Ve bu dizi bir örnek sonucu verir: `["http://feeds.reuters.com/reuters/topNews"]`

<a name="triggerFormDataValue"></a>

### <a name="triggerformdatavalue"></a>triggerFormDataValue

Bir tetikleyici içinde bir anahtar adı ile eşleşen tek bir değer içeren bir dize döndürecek *form verisinin* veya *form kodlu* çıktı.
İşlev, işlev birden fazla eşleşme bulunursa, bir hata oluşturur.

```
triggerFormDataValue('<key>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Anahtarı*> | Evet | String | Değer, istediğiniz anahtarı adı |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*anahtar-değer*> | String | Belirtilen anahtar değeri |
||||

*Örnek*

Bu örnekte RSS tetikleyicinin form-data veya form kodlu çıkış "feedUrl" anahtar değeri bir dize oluşturur:

```
triggerFormDataValue('feedUrl')
```

Bir örnek sonucu olarak bu dize döndürür: `"http://feeds.reuters.com/reuters/topNews"`

<a name="triggerMultipartBody"></a>

### <a name="triggermultipartbody"></a>triggerMultipartBody

Birden çok bölümden oluşur, bir tetikleyicinin çıktısında belirli bir parçanın gövdesini döndürür.

```
triggerMultipartBody(<index>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Dizin*> | Evet | Integer | Dizin değeri istediğiniz bölümü |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Gövde*> | String | Bir tetikleyicinin çok parçalı çıkışında belirtilen parçanın gövdesini |
||||

<a name="triggerOutputs"></a>

### <a name="triggeroutputs"></a>triggerOutputs

Çalışma zamanı veya değerleri bir tetikleyicinin çıkışını diğer JSON ad ve değer çiftlerinden geri döndürür.
İçin Toplu özellik `trigger().outputs`.
Bkz: [trigger()](#trigger).

```
triggerOutputs()
```

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Tetikleyici çıkışı*> | String | Çalışma zamanında bir tetikleyici çıkışı  |
||||

<a name="trim"></a>

### <a name="trim"></a>Kırpma

Bir dizeden öndeki ve sondaki boşlukları kaldırın ve güncelleştirilmiş bir dize döndürür.

```
trim('<text>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Metin*> | Evet | String | Öndeki ve sondaki boşlukları kaldırmak için olan dizesini |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*updatedText*> | String | Güncelleştirilmiş sürümü için özgün dizeye başta veya sonda boşluk olmadan |
||||

*Örnek*

Bu örnekte, "Hello World" dizesi öndeki ve sondaki boşlukları kaldırır:

```
trim(' Hello World  ')
```

Ve bu sonucu verir: `"Hello World"`

<a name="union"></a>

### <a name="union"></a>birleşim

Sahip bir koleksiyonun dönüş *tüm* belirtilen koleksiyonlarla öğelerinden.
Sonuçta görüntülenmek üzere bu işleve geçirilen koleksiyondaki bir öğe görünebilir. Bir veya daha fazla öğe aynı ada sahipse, bu ada sahip son öğeyi sonuçta da görüntülenir.

```
union('<collection1>', '<collection2>', ...)
union([<collection1>], [<collection2>], ...)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Collection1*>, <*collection2*>,...  | Evet | Dizi veya nesne ancak ikisini birden değil | Öğesinden, istediğiniz koleksiyonları *tüm* öğeleri |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*updatedCollection*> | Dizi veya nesne, sırasıyla | Belirtilen koleksiyonların - yinelenen değer yok tüm öğeleri içeren bir koleksiyon |
||||

*Örnek*

Bu örnekte *tüm* bu koleksiyonlara öğeleri:

```
union(createArray(1, 2, 3), createArray(1, 2, 10, 101))
```

Ve bu sonucu verir: `[1, 2, 3, 10, 101]`

<a name="uriComponent"></a>

### <a name="uricomponent"></a>uriComponent

URL güvenli olmayan karakterleri kaçış karakterleri ile değiştirerek bir Tekdüzen Kaynak Tanımlayıcısı (URI) ile kodlanmış sürümü bir dize döndürür.
Bu işlevi kullanmak yerine [encodeUriComponent()](#encodeUriComponent).
Her iki işlev aynı şekilde çalışır ancak `uriComponent()` tercih edilir.

```
uriComponent('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | String | URI ile kodlanmış biçimine dönüştürülecek dize. |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Kodlanmış uri*> | String | Kaçış karakterli URI ile kodlanmış dize |
||||

*Örnek*

Bu örnek, bu dize için bir URI ile kodlanmış sürümü oluşturur:

```
uriComponent('https://contoso.com')
```

Ve bu sonucu verir: `"http%3A%2F%2Fcontoso.com"`

<a name="uriComponentToBinary"></a>

### <a name="uricomponenttobinary"></a>uriComponentToBinary

Bir Tekdüzen Kaynak Tanımlayıcısı (URI) bileşeni için ikili dosya sürümü döndürür.

```
uriComponentToBinary('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | String | Dönüştürülecek URI ile kodlanmış dize. |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*ikili-için--URI ile kodlanacak*> | String | URI ile kodlanmış bir dize için ikili dosya sürümü. Tarafından temsil edilen ve base64 ile kodlanmış ikili içerik `$content`. |
||||

*Örnek*

Bu örnek, bu URI ile kodlanmış bir dize için ikili dosya sürümü oluşturur:

```
uriComponentToBinary('http%3A%2F%2Fcontoso.com')
```

Ve bu sonucu verir:

`"001000100110100001110100011101000111000000100101001100
11010000010010010100110010010001100010010100110010010001
10011000110110111101101110011101000110111101110011011011
110010111001100011011011110110110100100010"`

<a name="uriComponentToString"></a>

### <a name="uricomponenttostring"></a>uriComponentToString

Bir Tekdüzen Kaynak Tanımlayıcısı (URI) dize sürümü kodlamalı dize, etkili bir şekilde URI ile kodlanmış bir dize kod çözme döndürür.

```
uriComponentToString('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | String | Kodu çözülecek URI ile kodlanmış bir dize |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*kodu çözülen URI'si*> | String | URI ile kodlanmış bir dize için kodu çözülmüş sürümü |
||||

*Örnek*

Bu örnek kodu çözülen dize sürüm için bu URI ile kodlanmış bir dize oluşturur:

```
uriComponentToString('http%3A%2F%2Fcontoso.com')
```

Ve bu sonucu verir: `"https://contoso.com"`

<a name="uriHost"></a>

### <a name="urihost"></a>uriHost

Dönüş `host` Tekdüzen Kaynak Tanımlayıcısı (URI) değeri.

```
uriHost('<uri>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*uri*> | Evet | String | URI, `host` istediğiniz değer |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Ana değer*> | String | `host` Belirtilen URI değeri |
||||

*Örnek*

Bu örnekte bulur `host` bu URI değeri:

```
uriHost('https://www.localhost.com:8080')
```

Ve bu sonucu verir: `"www.localhost.com"`

<a name="uriPath"></a>

### <a name="uripath"></a>uriPath

Dönüş `path` Tekdüzen Kaynak Tanımlayıcısı (URI) değeri.

```
uriPath('<uri>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*uri*> | Evet | String | URI, `path` istediğiniz değer |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*yol değeri*> | String | `path` Belirtilen URI değeri. Varsa `path` bir değere sahip değil, "/" karakteri döndürür. |
||||

*Örnek*

Bu örnekte bulur `path` bu URI değeri:

```
uriPath('http://www.contoso.com/catalog/shownew.htm?date=today')
```

Ve bu sonucu verir: `"/catalog/shownew.htm"`

<a name="uriPathAndQuery"></a>

### <a name="uripathandquery"></a>uriPathAndQuery

Dönüş `path` ve `query` Tekdüzen Kaynak Tanımlayıcısı (URI) değerleri.

```
uriPathAndQuery('<uri>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*uri*> | Evet | String | URI, `path` ve `query` değerleri |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*yol sorgusu değeri*> | String | `path` Ve `query` Belirtilen URI değerleri. Varsa `path` olmayan bir değer belirtin, "/" karakteri döndürür. |
||||

*Örnek*

Bu örnekte bulur `path` ve `query` bu URI için değerler:

```
uriPathAndQuery('http://www.contoso.com/catalog/shownew.htm?date=today')
```

Ve bu sonucu verir: `"/catalog/shownew.htm?date=today"`

<a name="uriPort"></a>

### <a name="uriport"></a>uriPort

Dönüş `port` Tekdüzen Kaynak Tanımlayıcısı (URI) değeri.

```
uriPort('<uri>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*uri*> | Evet | String | URI, `port` istediğiniz değer |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*bağlantı noktası değeri*> | Integer | `port` Belirtilen URI değeri. Varsa `port` olmayan bir değer belirtin, protokolü için varsayılan bağlantı noktasını döndürür. |
||||

*Örnek*

Bu örnekte döndürür `port` bu URI değeri:

```
uriPort('http://www.localhost:8080')
```

Ve bu sonucu verir: `8080`

<a name="uriQuery"></a>

### <a name="uriquery"></a>uriQuery

Dönüş `query` Tekdüzen Kaynak Tanımlayıcısı (URI) değeri.

```
uriQuery('<uri>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*uri*> | Evet | String | URI, `query` istediğiniz değer |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Sorgu değeri*> | String | `query` Belirtilen URI değeri |
||||

*Örnek*

Bu örnekte döndürür `query` bu URI değeri:

```
uriQuery('http://www.contoso.com/catalog/shownew.htm?date=today')
```

Ve bu sonucu verir: `"?date=today"`

<a name="uriScheme"></a>

### <a name="urischeme"></a>uriScheme

Dönüş `scheme` Tekdüzen Kaynak Tanımlayıcısı (URI) değeri.

```
uriScheme('<uri>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*uri*> | Evet | String | URI, `scheme` istediğiniz değer |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*düzeni değeri*> | String | `scheme` Belirtilen URI değeri |
||||

*Örnek*

Bu örnekte döndürür `scheme` bu URI değeri:

```
uriScheme('http://www.contoso.com/catalog/shownew.htm?date=today')
```

Ve bu sonucu verir: `"http"`

<a name="utcNow"></a>

### <a name="utcnow"></a>utcNow

Geçerli zaman damgasını döndürür.

```
utcNow('<format>')
```

İsteğe bağlı olarak, farklı bir biçim ile belirtebilirsiniz <*biçimi*> parametresi.


| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Biçim*> | Hayır | String | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddTHH:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Geçerli zaman damgası*> | String | Geçerli tarih ve saat |
||||

*Örnek 1*

Bugün 1:00: 00'da 15 Nisan 2018'olduğunu varsayın.
Bu örnek, geçerli zaman damgasını alır:

```
utcNow()
```

Ve bu sonucu verir: `"2018-04-15T13:00:00.0000000Z"`

*Örnek 2*

Bugün 1:00: 00'da 15 Nisan 2018'olduğunu varsayın.
Bu örnek, isteğe bağlı "D" biçimini kullanarak geçerli zaman damgasını alır:

```
utcNow('D')
```

Ve bu sonucu verir: `"Sunday, April 15, 2018"`

<a name="variables"></a>

### <a name="variables"></a>Değişkenleri

Belirtilen bir değişkenin değerini döndürür.

```
variables('<variableName>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değişkenadı*> | Evet | String | Değerini istediğiniz değişkenin adı |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*değişken değeri*> | Tüm | Belirtilen değişken için değeri |
||||

*Örnek*

20 "numItems" değişkeni için geçerli değer olduğunu varsayın.
Bu örnek, bu değişken için tamsayı değeri alır:

```
variables('numItems')
```

Ve bu sonucu verir: `20`

<a name="workflow"></a>

### <a name="workflow"></a>iş akışı

İş akışı hakkında tüm ayrıntıları, çalışma zamanı sırasında döndürür.

```
workflow().<property>
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Özelliği*> | Hayır | String | İş akışı özelliğinin değerini istediğiniz adı <p>Bir iş akışı nesnesini şu özelliklere sahiptir: **adı**, **türü**, **kimliği**, **konumu**, ve **çalıştırma**. **Çalıştırma** özellik değeri, aynı zamanda bu özelliklere sahip bir nesne: **adı**, **türü**, ve **kimliği**. |
|||||

*Örnek*

Bu örnek, bir iş akışının geçerli çalıştırma için adı döndürür:

```
workflow().run.name
```

<a name="xml"></a>

### <a name="xml"></a>xml

XML sürümü için bir JSON nesnesi içeren bir dize döndürür.

```
xml('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | String | Dönüştürülecek JSON nesnesi içeren dize <p>JSON nesnesi, bir dizi olamaz, yalnızca bir kök özelliği olmalıdır. <br>Eğik çizgi kullanın (\\) çift tırnak işareti (") için bir kaçış karakteri olarak. |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*XML sürümü*> | Object | Kodlamalı XML belirtilen dize veya JSON nesnesi |
||||

*Örnek 1*

Bu örnek, bir JSON nesnesi içeren bu dize, XML sürümü oluşturur:

`xml(json('{ \"name\": \"Sophia Owen\" }'))`

Ve bu XML sonucunu döndürür:

```xml
<name>Sophia Owen</name>
```

*Örnek 2*

Bu JSON nesnesinin olduğunu varsayalım:

```json
{
  "person": {
    "name": "Sophia Owen",
    "city": "Seattle"
  }
}
```

Bu örnek XML bu JSON nesnesi içeren bir dize oluşturur:

`xml(json('{\"person\": {\"name\": \"Sophia Owen\", \"city\": \"Seattle\"}}'))`

Ve bu XML sonucunu döndürür:

```xml
<person>
  <name>Sophia Owen</name>
  <city>Seattle</city>
<person>
```

<a name="xpath"></a>

### <a name="xpath"></a>XPath

Düğümleri veya bir (XML Path Language) XPath ifadesi eşleşen değerler için XML denetleyin ve eşleşen düğümleri veya değerleri döndürür. Bir XPath ifadesi veya yalnızca "XPath", bir XML belge yapısı içinde XML içeriği düğümleri veya işlem değerleri seçebilirsiniz gezinmenize yardımcı olur.

```
xpath('<xml>', '<xpath>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*XML*> | Evet | Tüm | Düğümleri veya bir XPath ifadesi değeriyle eşleşen değerler aramak için XML dizesi |
| <*XPath*> | Evet | Tüm | Eşleşen bir XML düğümüyle veya değerleri bulmak için kullanılan XPath ifadesi |
|||||

| Dönüş değeri | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*XML düğümü*> | XML | Yalnızca tek bir düğüm belirtilen XPath ifade eşleştiğinde bir XML düğümü |
| <*Değer*> | Tüm | Yalnızca tek bir değer belirtilen XPath ifade eşleştiğinde bir XML düğümü değeri |
| [<*xml Düğüm1*>, <*xml Düğüm2*>,...] </br>veya </br>[<*value1*>, <*value2*>,...] | Dizi | XML düğüm veya belirtilen XPath ifadesi eşleşen değerleri olan bir dizi |
||||

*Örnek 1*

Bu örnek ile eşleşen düğümleri bulur `<name></name>` belirtilen bağımsız değişkenler düğümü ve bu düğüm değerleri içeren bir dizi döndürür:

`xpath(xml(parameters('items')), '/produce/item/name')`

Bağımsız değişkenleri şunlardır:

* Bu XML içeriyor "Items" dizesi:

  `"<?xml version="1.0"?> <produce> <item> <name>Gala</name> <type>apple</type> <count>20</count> </item> <item> <name>Honeycrisp</name> <type>apple</type> <count>10</count> </item> </produce>"`

  Örnekte [parameters()](#parameters) XML dizesi "Items" bağımsız değişkeninden almak için çalışır, ancak aynı zamanda dizenin XML biçimine dönüştürmeniz gerekir [xml()](#xml) işlevi.

* Bir dize olarak geçirilen bu XPath ifadesi:

  `"/produce/item/name"`

Sonuç dizisi ile eşleşen düğümleri işte `<name></name`:

`[ <name>Gala</name>, <name>Honeycrisp</name> ]`

*Örnek 2*

Bu örnek ile eşleşen düğümleri bulur örnek 1'de aşağıdakileri `<count></count>` düğüm ve o düğüm değerleri ile ekler `sum()` işlevi:

`xpath(xml(parameters('items')), 'sum(/produce/item/count)')`

Ve bu sonucu verir: `30`

*Örnek 3*

Bu örnekte, her iki ifade ile eşleşen düğümleri bulma `<location></location>` düğümünde XML ad alanı ile dahil belirtilen bağımsız. Ters eğik çizgi karakteri ifadeleri kullanma (\\) çift tırnak işareti (") için bir kaçış karakteri olarak.

* *1 ifadesi*

  `xpath(xml(body('Http')), '/*[name()=\"file\"]/*[name()=\"location\"]')`

* *İfade 2*

  `xpath(xml(body('Http')), '/*[local-name()=\"file\" and namespace-uri()=\"http://contoso.com\"]/*[local-name()=\"location\"]')`

Bağımsız değişkenleri şunlardır:

* XML belge isim uzayı içerir, bu XML `xmlns="http://contoso.com"`:

  ```xml
  <?xml version="1.0"?> <file xmlns="http://contoso.com"> <location>Paris</location> </file>
  ```

* Her iki XPath ifadesi burada:

  * `/*[name()=\"file\"]/*[name()=\"location\"]`

  * `/*[local-name()=\"file\" and namespace-uri()=\"http://contoso.com\"]/*[local-name()=\"location\"]`

Eşleşen bir sonuç düğümü işte `<location></location>` düğüm:

```xml
<location xmlns="https://contoso.com">Paris</location>
```

*Örnek 4*

Bu örnek değeri bulur örnek 3'de aşağıdakileri `<location></location>` düğüm:

`xpath(xml(body('Http')), 'string(/*[name()=\"file\"]/*[name()=\"location\"])')`

Ve bu sonucu verir: `"Paris"`

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [iş akışı tanımlama dili](../logic-apps/logic-apps-workflow-definition-language.md)
