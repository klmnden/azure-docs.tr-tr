---
title: İş akışı tanımlama dili - Azure Logic Apps için şema başvurusu
description: Azure Logic apps'te iş akışı tanımı dil şeması için başvuru kılavuzu
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: reference
ms.date: 04/30/2018
ms.openlocfilehash: d80ffa862546f56e93a338a7a1db031e2cb55990
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60845751"
---
# <a name="schema-reference-for-workflow-definition-language-in-azure-logic-apps"></a>Azure Logic Apps iş akışı tanımı dil Şeması Başvurusu

Bir mantıksal uygulama çalıştırmasında oluşturduğunuzda [Azure Logic Apps](../logic-apps/logic-apps-overview.md), mantıksal uygulamanızın mantıksal uygulamanızda çalışan gerçek mantığı tanımlayan bir temel alınan bir iş akışı tanımı içeriyor. Bu iş akışı tanımı kullanan [JSON](https://www.json.org/) ve iş akışı tanımı dil şeması tarafından doğrulanmış bir yapıyı izler. Bu başvuru, bu yapı nasıl şema öğeleri ve iş akışı tanımınızı tanımlar hakkında genel bir bakış sağlar.

## <a name="workflow-definition-structure"></a>İş akışı tanım yapısı

Mantıksal uygulamanızı yanı sıra, tetikleyici başlatıldıktan sonra çalışacak bir veya daha fazla eylemleri örnekleme için bir tetikleyici her zaman bir iş akışı tanımı içerir.

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
| $schema | Yalnızca bir iş akışı tanımı dışarıdan başvurma sırasında | Burada bulabilirsiniz iş akışı tanımı dil sürümü tanımlayan JSON şema dosyası için konumu: <p>`https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json`</p> |
| contentVersion | Hayır | Varsayılan "1.0.0.0" olan, iş akışı tanımı sürüm numarası. Tanımlamak ve doğru tanımı bir iş akışı dağıtırken doğrulamak için kullanılacak bir değer belirtin. |
| parametreler | Hayır | Veri, iş akışınıza aktarmak bir veya daha fazla parametre tanımları <p><p>En fazla Parametreler: 50 |
| tetikleyiciler | Hayır | Tanımları, iş akışı örneği bir veya daha fazla tetikleyici. Ancak yalnızca iş akışı tanımlama dili ile Logic Apps Tasarımcısı ile görsel olarak değil birden fazla tetikleyici tanımlayabilirsiniz. <p><p>En fazla Tetikleyicileri: 10 |
| Eylemler | Hayır | İş akışı çalışma zamanında yürütmek bir veya daha fazla eylem için tanımları <p><p>En fazla eylemler: 250 |
| çıkışlar | Hayır | Döndüren bir iş akışından çıkış tanımları <p><p>En fazla çıktı: 10 |
||||

## <a name="parameters"></a>Parametreler

İçinde `parameters` bölümünde, giriş kabul etmek için dağıtım iş akışı tanımınızı kullanan tüm iş akışı parametreleri tanımlayın. Dağıtım sırasında parametre bildirimleri hem parametre değerlerini gereklidir. Diğer iş akışı bölümlerde bu parametreler kullanabilmeniz için önce bu bölümlerdeki tüm parametreleri bildirdiğinizden emin olun. 

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
| type | Evet | int, kayan noktalı sayı, dize, securestring, bool, dizi, JSON nesnesi, secureobject <p><p>**Not**: Tüm parolalar, anahtarlar ve gizli dizileri için kullanmak `securestring` ve `secureobject` çünkü `GET` işlemi, bu tür döndürmez. Parametreleri güvenliğini sağlama hakkında daha fazla bilgi için bkz. [mantıksal uygulamanızı güvenli hale getirme](../logic-apps/logic-apps-securing-a-logic-app.md#secure-action-parameters) | Parametresinin türü |
| defaultValue | Evet | Aynı `type` | İş akışı örneğini oluşturduğunda hiçbir değer belirtilmemişse varsayılan parametre değeri |
| izin verilen değerler | Hayır | Aynı `type` | Bir dizi parametre kabul edebilen değerlerle |
| meta veriler | Hayır | JSON nesnesi | Diğer parametre ayrıntıları, örneğin, ad veya mantıksal uygulama, akış veya Visual Studio veya diğer araçları tarafından kullanılan tasarım zamanı veri için okunabilir bir açıklaması |
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
| <*anahtar adı*> | Evet | String | Anahtar adı çıkışı için dönüş değeri |
| type | Evet | int, kayan noktalı sayı, dize, securestring, bool, dizi, JSON nesnesi | Çıkış döndürülen değerin türü |
| value | Evet | Aynı `type` | Çıkış dönüş değeri |
|||||

Bir iş akışından işlemin çıktısını almak için mantıksal uygulamanızın çalıştırma geçmişi ve Azure portalında ayrıntılarını gözden geçirebilir veya [iş akışı REST API](https://docs.microsoft.com/rest/api/logic/workflows). Böylece panolar oluşturabilir, çıkış harici sistemlere, örneğin, Power BI geçirebilirsiniz.

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
| "\@parameters('myAge')" | Bu sayıyı döndürür: 42 |
| "\@{parameters('myAge')}" | Bu sayı, dize olarak döndürür: "42" |
| "Benim geçerlilik süresi \@{parameters('myAge')}" | Bu dize döndürecek: "42 yaşımı is" |
| "\@concat (' yaşımı ', string(parameters('myAge')))" | Bu dize döndürecek: "42 yaşımı is" |
| "Benim geçerlilik süresi \@ \@{parameters('myAge')}" | İfade içerir. Bu dizeyi döndürün: ' Benim geçerlilik süresi \@{parameters('myAge')}' |
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

Bazı ifadelerin değerleri, iş akışı tanımınızı çalışmaya başladığında henüz bulunmayabilir çalışma zamanı eylemlerden alın. Başvuru veya bu değerleri ifadelerde çalışmak için kullanabileceğiniz [ *işlevleri* ](../logic-apps/workflow-definition-language-functions-reference.md) , iş akışı tanımlama dili sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [iş akışı tanımlama dili eylemleri ve Tetikleyicileri](../logic-apps/logic-apps-workflow-actions-triggers.md)
* Program aracılığıyla oluşturma ve logic apps ile yönetme hakkında bilgi edinin [iş akışı REST API](https://docs.microsoft.com/rest/api/logic/workflows)
