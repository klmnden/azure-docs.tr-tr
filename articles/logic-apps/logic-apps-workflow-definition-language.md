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
ms.date: 05/13/2019
ms.openlocfilehash: 3b0ad33ea6348f24079b3c88f972437244c0bc93
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65596752"
---
# <a name="schema-reference-for-workflow-definition-language-in-azure-logic-apps"></a>Azure Logic Apps iş akışı tanımı dil Şeması Başvurusu

Bir mantıksal uygulama çalıştırmasında oluşturduğunuzda [Azure Logic Apps](../logic-apps/logic-apps-overview.md), mantıksal uygulamanızın mantıksal uygulamanızda çalışan gerçek mantığı tanımlayan bir temel alınan bir iş akışı tanımı içeriyor. Bu iş akışı tanımı kullanan [JSON](https://www.json.org/) ve iş akışı tanımı dil şeması tarafından doğrulanmış bir yapıyı izler. Bu başvuru, bu yapı ve şema öznitelikler, iş akışı tanımında nasıl tanımlar hakkında genel bir bakış sağlar.

## <a name="workflow-definition-structure"></a>İş akışı tanım yapısı

Mantıksal uygulamanızı yanı sıra, tetikleyici başlatıldıktan sonra çalışacak bir veya daha fazla eylemleri örnekleme için bir tetikleyici her zaman bir iş akışı tanımı içerir.

Bir iş akışı tanımı için üst düzey yapısı şu şekildedir:

```json
"definition": {
  "$schema": "<workflow-definition-language-schema-version>",
  "actions": { "<workflow-action-definitions>" },
  "contentVersion": "<workflow-definition-version-number>",
  "outputs": { "<workflow-output-definitions>" },
  "parameters": { "<workflow-parameter-definitions>" },
  "staticResults": { "<static-results-definitions>" },
  "triggers": { "<workflow-trigger-definitions>" }
}
```

| Öznitelik | Gerekli | Açıklama |
|-----------|----------|-------------|
| `definition` | Evet | Başlangıç öğesi, iş akışı tanımı |
| `$schema` | Yalnızca bir iş akışı tanımı dışarıdan başvurma sırasında | Burada bulabilirsiniz iş akışı tanımı dil sürümü tanımlayan JSON şema dosyası için konumu: <p>`https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json`</p> |
| `actions` | Hayır | İş akışı çalışma zamanında yürütmek bir veya daha fazla eylem tanımları. Daha fazla bilgi için [tetikleyiciler ve Eylemler](#triggers-actions). <p><p>En fazla eylemler: 250 |
| `contentVersion` | Hayır | Varsayılan "1.0.0.0" olan, iş akışı tanımı sürüm numarası. Tanımlamak ve doğru tanımı bir iş akışı dağıtırken doğrulamak için kullanılacak bir değer belirtin. |
| `outputs` | Hayır | Döndüren bir iş akışından çıkış tanımları. Daha fazla bilgi için [çıkışları](#outputs). <p><p>En fazla çıktı: 10 |
| `parameters` | Hayır | Veri, iş akışınıza aktarmak bir veya daha fazla parametre tanımları. Daha fazla bilgi için [parametreleri](#parameters). <p><p>En fazla Parametreler: 50 |
| `staticResults` | Hayır | Bu eylemler statik sonuçları etkinleştirildiğinde sahte çıkışları eylemleri tarafından döndürülen bir veya daha fazla statik sonuçları tanımları. Her eylem tanımındaki `runtimeConfiguration.staticResult.name` özniteliği başvuruda içinde karşılık gelen tanımını `staticResults`. Daha fazla bilgi için [statik sonuçları](#static-results). |
| `triggers` | Hayır | Tanımları, iş akışı örneği bir veya daha fazla tetikleyici. Ancak yalnızca iş akışı tanımlama dili ile Logic Apps Tasarımcısı ile görsel olarak değil birden fazla tetikleyici tanımlayabilirsiniz. Daha fazla bilgi için [tetikleyiciler ve Eylemler](#triggers-actions). <p><p>En fazla Tetikleyicileri: 10 |
||||

<a name="triggers-actions"></a>

## <a name="triggers-and-actions"></a>Tetikleyiciler ve Eylemler

Bir iş akışı tanımı `triggers` ve `actions` iş akışının yürütme sırasında gerçekleşen çağrıları bölümleri tanımlar. Söz dizimi ve bu bölümleri hakkında daha fazla bilgi için bkz: [iş akışı tetikleyici ve Eylemler](../logic-apps/logic-apps-workflow-actions-triggers.md).

<a name="outputs"></a>

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

| Öznitelik | Gerekli | Tür | Açıklama |
|-----------|----------|------|-------------|
| <*key-name*> | Evet | String | Anahtar adı çıkışı için dönüş değeri |
| <*key-type*> | Evet | int, float, string, securestring, bool, array, JSON nesnesi | Çıkış döndürülen değerin türü |
| <*anahtar-değer*> | Evet | Aynı <*anahtar türü*> | Çıkış dönüş değeri |
|||||

Bir iş akışından işlemin çıktısını almak için mantıksal uygulamanızın çalıştırma geçmişi ve Azure portalında ayrıntılarını gözden geçirebilir veya [iş akışı REST API](https://docs.microsoft.com/rest/api/logic/workflows). Böylece panolar oluşturabilir, çıkış harici sistemlere, örneğin, Power BI geçirebilirsiniz.

<a name="parameters"></a>

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

| Öznitelik | Gerekli | Tür | Açıklama |
|-----------|----------|------|-------------|
| <*parameter-type*> | Evet | int, float, string, securestring, bool, array, JSON nesnesi, secureobject <p><p>**Not**: Tüm parolalar, anahtarlar ve gizli dizileri için kullanmak `securestring` ve `secureobject` çünkü `GET` işlemi, bu tür döndürmez. Parametreleri güvenliğini sağlama hakkında daha fazla bilgi için bkz. [mantıksal uygulamanızı güvenli hale getirme](../logic-apps/logic-apps-securing-a-logic-app.md#secure-action-parameters) | Parametresinin türü |
| <*default-parameter-value*> | Evet | Aynı `type` | İş akışı örneğini oluşturduğunda hiçbir değer belirtilmemişse varsayılan parametre değeri |
| <*array-with-permitted-parameter-values*> | Hayır | Array | Bir dizi parametre kabul edebilen değerlerle |
| `metadata` | Hayır | JSON nesnesi | Diğer parametre ayrıntıları, örneğin, ad veya mantıksal uygulama, akış veya Visual Studio veya diğer araçları tarafından kullanılan tasarım zamanı veri için okunabilir bir açıklaması |
||||

<a name="static-results"></a>

## <a name="static-results"></a>Statik sonuçları

İçinde `staticResults` öznitelik, bir eylemin sahte tanımlama `outputs` ve `status` eylemin statik sonucu ayarı etkin olduğunda, bir eylem döndürür. Eylemin tanımındaki `runtimeConfiguration.staticResult.name` özniteliği başvuruda statik sonucu tanımının içinde adı `staticResults`. Öğrenin [logic apps ile sahte veri statik sonuçlarını ayarlayarak test](../logic-apps/test-logic-apps-mock-data-static-results.md).

```json
"definition": {
   "$schema": "<...>",
   "actions": { "<...>" },
   "contentVersion": "<...>",
   "outputs": { "<...>" },
   "parameters": { "<...>" },
   "staticResults": {
      "<static-result-definition-name>": {
         "outputs": {
            <output-attributes-and-values-returned>,
            "headers": { <header-values> },
            "statusCode": "<status-code-returned>"
         },
         "status": "<action-status>"
      }
   },
   "triggers": { "<...>" }
}
```

| Öznitelik | Gerekli | Tür | Açıklama |
|-----------|----------|------|-------------|
| <*static-result-definition-name*> | Evet | String | Bir eylem tanımı aracılığıyla başvurabilirsiniz bir statik sonucu tanımı adı bir `runtimeConfiguration.staticResult` nesne. Daha fazla bilgi için [çalışma zamanı yapılandırma ayarlarını](../logic-apps/logic-apps-workflow-actions-triggers.md#runtime-config-options). <p>İstediğiniz herhangi bir benzersiz ad kullanabilirsiniz. Varsayılan olarak, bu benzersiz bir ad gerekirse artırılır bir sayı ile eklenir. |
| <*output-attributes-and-values-returned*> | Evet | Varies | Bu öznitelikler için gereksinimleri farklı koşullara göre farklılık gösterir. Örneğin, `status` olduğu `Succeeded`, `outputs` öznitelik içeren öznitelikler ve değerler olarak sahte çıkışları eylem tarafından döndürülen. Varsa `status` olduğu `Failed`, `outputs` özniteliği içeren `errors` bir veya daha fazla hata ile dizisi özniteliği `message` hata bilgilerini sahip nesneleri. |
| <*header-values*> | Hayır | JSON | Eylem tarafından döndürülen herhangi bir üstbilgi değeri |
| <*status-code-returned*> | Evet | String | Eylem tarafından döndürülen durum kodu |
| <*action-status*> | Evet | String | Örneğin, eylemin durumu `Succeeded` veya `Failed` |
|||||

Örneğin, bu HTTP eylemi tanımı'ndaki `runtimeConfiguration.staticResult.name` başvuruları öznitelik `HTTP0` içinde `staticResults` eylemi için sahte çıkışları tanımlandığı özniteliği. `runtimeConfiguration.staticResult.staticResultOptions` Özniteliği belirtir statik sonucu ayarının olduğunu `Enabled` HTTP eylemi.

```json
"actions": {
   "HTTP": {
      "inputs": {
         "method": "GET",
         "uri": "https://www.microsoft.com"
      },
      "runAfter": {},
      "runtimeConfiguration": {
         "staticResult": {
            "name": "HTTP0",
            "staticResultOptions": "Enabled"
         }
      },
      "type": "Http"
   }
},
```

HTTP eylem çıkışları döndürür `HTTP0` tanımının içinde `staticResults`. Bu örnekte, durum kodu için sahte çıktıdır `OK`. Üstbilgi değerleri için sahte bir çıktıdır `"Content-Type": "application/JSON"`. Eylemin durumu için sahte bir çıktıdır `Succeeded`.

```json
"definition": {
   "$schema": "<...>",
   "actions": { "<...>" },
   "contentVersion": "<...>",
   "outputs": { "<...>" },
   "parameters": { "<...>" },
   "staticResults": {
      "HTTP0": {
         "outputs": {
            "headers": {
               "Content-Type": "application/JSON"
            },
            "statusCode": "OK"
         },
         "status": "Succeeded"
      }
   },
   "triggers": { "<...>" }
},
```

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
