---
title: "İçerik türleri - Azure mantıksal uygulamaları işlemek | Microsoft Docs"
description: "Azure mantıksal uygulamaları tasarım ve çalışma zamanı içerik türleri ile nasıl ilgileneceğini"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: cd1f08fd-8cde-4afc-86ff-2e5738cc8288
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: ac67838344bbd10384299c086ff096fbe5dec6a9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="handle-content-types-in-logic-apps"></a>Logic apps içinde içerik türlerini yönetmek

Farklı türlerde içerik JSON, XML, düz dosyalar ve ikili veriler dahil olmak üzere bir mantıksal uygulama akabilir. Logic Apps altyapısı tüm içerik türlerini destekler, ancak bazı yerel Logic Apps altyapısı tarafından anlaşılır. Başkalarının atama veya dönüştürmeler gerektiğinde gerektirebilir. Bu makalede altyapısı farklı içerik türlerini nasıl işlediğini ve doğru bir şekilde gerektiğinde bu türleri nasıl ele alınacağını açıklar.

## <a name="content-type-header"></a>Content-Type üstbilgisi

Temel olarak başlatmak için iki bakalım `Content-Types` yok gerektiren dönüştürme veya bir mantıksal uygulama kullanabileceğiniz atama: `application/json` ve `text/plain`.

## <a name="applicationjson"></a>Application/JSON

İş akışı altyapısının dayanan `Content-Type` uygun işleme belirlemek için HTTP başlığından çağırır. İçerik türü herhangi bir istekle `application/json` depolanır ve bir JSON nesnesi olarak işlenir. Ayrıca, herhangi bir atama gerek kalmadan JSON içeriği, varsayılan olarak'e ayrıştırılabilir. 

Örneğin, içerik türü üstbilgisi bir istek ayrıştırılamıyor `application/json ` gibi bir ifade kullanarak bir iş akışında `@body('myAction')['foo'][0]` değeri almaya `bar` bu durumda:

```
{
    "data": "a",
    "foo": [
        "bar"
    ]
}
```

Hiçbir ek atama gereklidir. JSON ancak belirtilen üstbilgi olmadığına verileri ile çalışıyorsanız, el ile JSON kullanmaya çevirebilirsiniz `@json()` işlev, örneğin: `@json(triggerBody())['foo']`.

### <a name="schema-and-schema-generator"></a>Şema ve şema Oluşturucusu

İstek tetikleyici için almayı beklediğiniz yükü JSON şeması girmenizi sağlar. İstek içeriği tüketebileceği şekilde bu şema Tasarımcısı generate belirteçleri sağlar. Bir şema hazır yoksa seçin **şema üretmek için kullanım örnek yük**, bir örnek yükü JSON şeması oluşturabilirsiniz.

![Şema](./media/logic-apps-http-endpoint/manualtrigger.png)

### <a name="parse-json-action"></a>'Parse JSON' eylemi

`Parse JSON` Eylem mantığı uygulama tüketimi için kolay belirteçler içine JSON içeriği ayrıştırılamıyor olanak sağlar. Benzer şekilde istek tetikleyici, bu eylem girin veya ayrıştırma istediğiniz içerik için JSON şeması oluşturma olanak sağlar. Bu araç Süren veri Service Bus, Azure Cosmos DB ve vb. kolaylaştırır.

![JSON ayrıştırılamıyor](./media/logic-apps-content-type/ParseJSON.png)

## <a name="textplain"></a>Metin/düz

Benzer şekilde `application/json`, HTTP iletileri ile alınan `Content-Type` üstbilgisinin `text/plain` ham biçiminde depolanır. Bu ileti sonraki eylemleri atama olmadan içinde yer alan, ayrıca, bu istekleri ile gider `Content-Type`: `text/plain` üstbilgi. Örneğin, düz bir dosya ile çalışırken, bu HTTP içerik olarak alabilirsiniz `text/plain`:

```
Date,Name,Address
Oct-1,Frank,123 Ave.
```

Bir sonraki eylem, başka bir istek gövdesi olarak istek göndermesi durumunda (`@body('flatfile')`), istek olması gereken bir `text/plain` Content-Type üstbilgisi. Düz metin olan ancak belirtilen üstbilgi olmadığına verilerle çalışıyorsanız, metin kullanarak verileri el ile çevirebilirsiniz `@string()` işlev, örneğin: `@string(triggerBody())`.

## <a name="applicationxml-and-applicationoctet-stream-and-converter-functions"></a>Application/xml ve uygulama/octet-stream ve dönüştürücü işlevleri

Logic Apps altyapısı her zaman korur `Content-Type` HTTP isteği veya yanıtı alınmadı. Altyapı olan içeriği alırsa, bunu `Content-Type` , `application/octet-stream`, ve bir sonraki eylem atama olmadan içeriği, giden istek olduğunu dahil `Content-Type`: `application/octet-stream`. Bu şekilde, iş akışı taşırken veri kaybı olmadığından altyapısı garanti edebilir. Ancak, iş akışı durumu hareket ederken eylem durumu (girişleri ve çıkışları) bir JSON nesnesinde depolanır. Bazı veri türleri korumak için her ikisi de korur uygun meta verilerle ikili base64 ile kodlanmış dizeye içerik altyapısı dönüştürür şekilde `$content` ve `$content-type`, otomatik olarak olduğu dönüştürülmesi. 

* `@json()`-Veri çevirir`application/json`
* `@xml()`-Veri çevirir`application/xml`
* `@binary()`-Veri çevirir`application/octet-stream`
* `@string()`-Veri çevirir`text/plain`
* `@base64()`-İçerik base64 dizeye dönüştürür
* `@base64toString()`-base64 ile kodlanmış dizeye dönüştürür`text/plain`
* `@base64toBinary()`-base64 ile kodlanmış dizeye dönüştürür`application/octet-stream`
* `@encodeDataUri()`-dize dataUri bayt dizisi olarak kodlar
* `@decodeDataUri()`-bir dataUri bir bayt dizisine kodunu çözer.

Örneğin, bir HTTP isteğiyle aldıysanız `Content-Type`: `application/xml`:

```
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

Cast ve daha sonra aşağıdakine benzer ile kullanmak `@xml(triggerBody())`, veya bir işlev `@xpath(xml(triggerBody()), '/CustomerName')`.

## <a name="other-content-types"></a>Diğer içerik türleri

Diğer içerik türleri desteklenir ve logic apps ile çalışır, ancak ileti gövdesi çözerek el ile alma gerektirebilir `$content`. Örneğin, size tetiklemek varsayalım bir `application/x-www-url-formencoded` isteği nereye `$content` olan tüm verileri korumak için bir base64 dizesi kodlanmış yükü:

```
CustomerName=Frank&Address=123+Avenue
```

Düz metin veya JSON isteği olmadığı için istek eyleminde şekilde depolanır:

```
...
"body": {
    "$content-type": "application/x-www-url-formencoded",
    "$content": "AAB1241BACDFA=="
}
```

Şu anda, form verileri için yerel bir işlevi yoktur, bir işlev verilerle el ile erişerek bu verileri bir iş akışında hala kullanabilirsiniz şekilde ister `@string(body('formdataAction'))`. Ayrıca yönelik giden istek istediyseniz `application/x-www-url-formencoded` içerik türü üstbilgisi ekleyebilirsiniz istek eylem gövdeye gibi herhangi bir atama olmadan `@body('formdataAction')`. Gövde yalnızca parametresinde ise ancak, bu yöntem yalnızca çalışır `body` giriş. Kullanmayı denerseniz `@body('formdataAction')` içinde bir `application/json` isteği kodlu gövde gönderdiğinden çalışma zamanı hatası alırsınız.

