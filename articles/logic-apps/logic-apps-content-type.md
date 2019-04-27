---
title: İçerik türlerini - Azure Logic Apps işleme | Microsoft Docs
description: Logic Apps tanıtıcıları tasarım zamanında içerik türlerini ve çalışma zamanı nasıl öğrenin
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.topic: article
ms.date: 07/20/2018
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 2a9318317d5a01136a42b4fb6d580bafaf53ec4e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60685768"
---
# <a name="handle-content-types-in-azure-logic-apps"></a>Azure Logic apps'te içerik türlerini işleme

Çeşitli içerik türleri, bir mantıksal uygulama, örneğin, JSON, XML, düz dosyalar ve ikili verileri akabilir. Logic Apps tüm içerik türleri desteklese de, bazı yerel desteği olan ve atama veya logic apps dönüştürme gerekmez. Diğer türler atama veya gerektiği gibi dönüştürme gerektirebilir. Bu makalede Logic Apps içerik türleri nasıl işlediğini ve nasıl doğru şekilde tür dönüştürme veya gerekli olduğunda bu tür dönüştürme açıklanmaktadır.

İçerik türlerini işleme için en uygun yolu belirlemek için Logic Apps dayanan `Content-Type` üstbilgi değerini HTTP çağrıları, örneğin:

* [Uygulama/json](#application-json) (yerel tür)
* [metin/düz](#text-plain) (yerel tür)
* [Application/xml ve uygulama/octet-akış](#application-xml-octet-stream)
* [Diğer içerik türleri](#other-content-types)

<a name="application-json"></a>

## <a name="applicationjson"></a>uygulama/json

Logic Apps depolar ve işler herhangi bir istekle *application/json* içerik türü bir JavaScript gösterimi (JSON) nesnesi. Varsayılan olarak, herhangi bir atama olmadan JSON içeriği ayrıştırabilirsiniz. "Application/json" içerik türü sahip bir üstbilgi sahip bir istek ayrıştırmak için bir ifade kullanabilirsiniz. Bu örnek değeri döndürür `dog` gelen `animal-type` olmadan dizi: 
 
`@body('myAction')['animal-type'][0]` 
  
  ```json
  {
    "client": {
       "name": "Fido",
       "animal-type": [ "dog", "cat", "rabbit", "snake" ]
    }
  }
  ```

Üst bilgi belirtmeyen JSON verileri ile çalışıyorsanız, el ile bu verileri JSON kullanarak çevirebilirsiniz [json() işlevi](../logic-apps/workflow-definition-language-functions-reference.md#json), örneğin: 
  
`@json(triggerBody())['animal-type']`

### <a name="create-tokens-for-json-properties"></a>JSON özellikleri için belirteçleri oluşturun

Logic Apps özelliği, başvurulacağını ve bu özellikleri daha kolay mantıksal uygulamanızın iş akışında kullanabilmeniz için JSON içeriği özellikleri temsil eden kullanıcı dostu belirteçleri oluşturmak sağlar.

* **İstek tetikleyicisi**

  Mantıksal Uygulama Tasarımcısı'nda bu tetikleyiciyi kullandığınızda, almayı beklediğiniz yükü açıklayan bir JSON şeması sağlayabilirsiniz. 
  Tasarımcı, bu şema kullanarak JSON içeriği ayrıştırır ve JSON içeriğinizi özellikleri temsil eden kullanıcı dostu belirteçleri oluşturur. 
  Daha sonra kolayca başvurmak ve mantıksal uygulamanızın iş akışı boyunca bu özellikleri kullanın. 
  
  Bir şema yoksa şemayı oluşturabilirsiniz. 
  
  1. İstek tetikleyicisinde seçin **şema oluşturmak için örnek yük kullanma**.  
  
  2. Altında **girin veya yapıştırın örnek JSON yükü**, bir örnek yük sağlayın ve ardından **Bitti**. Örneğin: 

     ![Örnek JSON yükü girin](./media/logic-apps-content-type/request-trigger.png)

     Oluşturulan şema artık Tetikleyiciniz içinde görünür.

     ![Örnek JSON yükü girin](./media/logic-apps-content-type/generated-schema.png)

     Kod Görünümü düzenleyicisinde istek Tetikleyiciniz için temel alınan tanımı aşağıda verilmiştir:

     ```json
     "triggers": { 
        "manual": {
           "type": "Request",
           "kind": "Http",
           "inputs": { 
              "schema": {
                 "type": "object",
                 "properties": {
                    "client": {
                       "type": "object",
                       "properties": {
                          "animal-type": {
                             "type": "array",
                             "items": {
                                "type": "string"
                             },
                          },
                          "name": {
                             "type": "string"
                          }
                       }
                    }
                 }
              }
           }
        }
     }
     ```

  3. İsteğinizde, eklediğiniz emin bir `Content-Type` üstbilgi ve üst bilginin değeri `application/json`.

* **Eylem JSON Ayrıştır**

  Mantıksal Uygulama Tasarımcısı'nda bu eylemini kullandığınızda, JSON çıkışı çözümlenemedi ve JSON içeriğinizi özellikleri temsil eden kullanıcı dostu belirteçleri oluşturur. 
  Daha sonra kolayca başvurmak ve mantıksal uygulamanızın iş akışı boyunca bu özellikleri kullanın. Benzer şekilde istek tetikleyicisi, sağlayın veya yapabilirsiniz ayrıştırmak için JSON içeriği açıklayan bir JSON şema oluşturmak. 
  Bu şekilde, Azure Service Bus, Azure Cosmos DB ve benzeri verilerini daha kolay kullanabilir.

  ![JSON Ayrıştır](./media/logic-apps-content-type/parse-json.png)

<a name="text-plain"></a>

## <a name="textplain"></a>metin/düz

Mantıksal uygulamanızı olan HTTP iletiler aldığında `Content-Type` üstbilgi kümesine `text/plain`, mantıksal uygulamanız bu iletileri ham biçiminde depolar. Sonraki eylemler olmadan bu iletileri dahil ederseniz, istekleri ile gider `Content-Type` başlığı ayarlayın `text/plain`. 

Örneğin, bir düz dosya ile çalışırken, bir HTTP isteğiyle alabilirsiniz `Content-Type` üstbilgi kümesine `text/plain` içerik türü:

`Date,Name,Address`</br>
`Oct-1,Frank,123 Ave`

Ardından bu isteği bir sonraki eylem için başka bir istek gövdesi olarak Örneğin, gönderirseniz `@body('flatfile')`, ikinci bir istek de sahip bir `Content-Type` ayarlamak için üst bilgi `text/plain`. Düz metin, ancak bir üst bilgi eklemediğiniz verileri ile çalışıyorsanız, el ile metin için bu verileri kullanarak çevirebilirsiniz [string() işlevi](../logic-apps/workflow-definition-language-functions-reference.md#string) Bu ifade gibi: 

`@string(triggerBody())`

<a name="application-xml-octet-stream"></a>

## <a name="applicationxml-and-applicationoctet-stream"></a>Application/xml ve uygulama/octet-akış

Mantıksal uygulamalar her zaman korur `Content-Type` alınan HTTP istek veya yanıtı. Mantıksal uygulamanız ile içeriği alırsa, bu nedenle `Content-Type` kümesine `application/octet-stream`, ve içerik sonraki bir eylem olmadan Giden istek ayrıca olduğunu dahil `Content-Type` kümesine `application/octet-stream`. Bu şekilde, Logic Apps, veri akışı taşırken kayıp katılmaz olduğunu garanti edebilir. Bununla birlikte, eylem durumu veya girişler ve çıkışlar, depolanan bir JSON nesnesinde akışı durumu hareket ederken. 

## <a name="converter-functions"></a>Dönüştürücü işlevleri

Bazı veri türleri korumak için Logic Apps içerik korur hem de uygun meta verilerle bir ikili base64 ile kodlanmış dize dönüştürür `$content` yükü ve `$content-type`, hangi otomatik olarak dönüştürülür. 

Bu liste, bunları kullandığınızda Logic Apps içerik nasıl dönüştürür açıklar [işlevleri](../logic-apps/workflow-definition-language-functions-reference.md):

* `json()`: Yayınları verileri `application/json`
* `xml()`: Yayınları verileri `application/xml`
* `binary()`: Yayınları verileri `application/octet-stream`
* `string()`: Yayınları verileri `text/plain`
* `base64()`: İçeriği bir base64 dizesine dönüştürür
* `base64toString()`: Bir base64 ile kodlanmış dizeye dönüştürür `text/plain`
* `base64toBinary()`: Bir base64 ile kodlanmış dizeye dönüştürür `application/octet-stream`
* `encodeDataUri()`: Bir dize dataUri bayt dizisi olarak kodunu çözer.
* `decodeDataUri()`: Kodunu çözer bir `dataUri` bir bayt dizisi halinde

Örneğin, bir HTTP isteğini almaya devam ederseniz burada `Content-Type` kümesine `application/xml`, bu içeriği gibi:

```html
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

Kullanarak bu içeriği çevirebilirsiniz `@xml(triggerBody())` ifadesiyle `xml()` ve `triggerBody()` çalışır ve ardından bu içerik daha sonra kullanın. Veya, kullanabileceğiniz `@xpath(xml(triggerBody()), '/CustomerName')` ifadesiyle `xpath()` ve `xml()` işlevleri. 

## <a name="other-content-types"></a>Diğer içerik türleri

Logic Apps ile çalışır ve diğer içerik türlerini destekler, ancak çözerek ileti gövdesi el ile alma gerektirebilir `$content` değişkeni.

Örneğin, mantıksal uygulamanızı bir istekle tarafından tetiklenen varsayalım `application/x-www-url-formencoded` içerik türü. Verilerinizi korumak için `$content` değişkenin istek gövdesinde bir base64 dizesi olarak kodlanmış bir yükü vardır:

`CustomerName=Frank&Address=123+Avenue`

Düz metin veya JSON isteği olmadığı için istek eylem aşağıdaki şekilde depolanır:

```json
"body": {
   "$content-type": "application/x-www-url-formencoded",
   "$content": "AAB1241BACDFA=="
}
```

Logic Apps, örneğin form verilerini işlemeye yönelik yerel işlevleri sağlar: 

* [triggerFormDataValue()](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataValue)
* [triggerFormDataMultiValues()](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataMultiValues)
* [formDataValue()](../logic-apps/workflow-definition-language-functions-reference.md#formDataValue) 
* [formDataMultiValues()](../logic-apps/workflow-definition-language-functions-reference.md#formDataMultiValues)

Veya bu örnek gibi bir ifade kullanılarak el ile verilere erişebilir:

`@string(body('formdataAction'))` 

İsterseniz aynı yönelik giden istek `application/x-www-url-formencoded` içerik türü üst bilgisi, ekleyebileceğiniz istek eylemin body olmadan herhangi bir ifade gibi kullanarak `@body('formdataAction')`. Gövde yalnızca parametresinde olduğunda ancak, bu yöntem yalnızca çalışır `body` giriş. Kullanmayı denerseniz `@body('formdataAction')` ifadesinde bir `application/json` istek gövdesi kodlanmış gönderildiği için çalışma zamanı hatası elde.
