---
title: Tarayıcıdan - Azure Bilişsel hizmetler API çağrısı | Microsoft Docs
description: Bir Web sayfası API yaparak iyileştirmek için Azure özel karar hizmeti ile nasıl doğrudan bir tarayıcıdan çağırır.
services: cognitive-services
author: slivkins
manager: slivkins
ms.service: cognitive-services
ms.topic: article
ms.date: 05/09/2018
ms.author: slivkins,marcozo,alekh
ms.openlocfilehash: 10236c9d8f70d9b90a896464b4f86a847ee904c2
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354748"
---
# <a name="call-api-from-a-browser"></a>Tarayıcıdan API çağrısı yapma

Bu makalede Azure özel karar hizmeti API çağrıları doğrudan bir tarayıcıdan yapmanıza yardımcı olur.

Şunları yaptığınızdan emin olun [uygulamanızı kaydetme](custom-decision-service-get-started-register.md), ilk.

Haydi başlayalım. Uygulamanızı birkaç makale sayfalarına bağlantılar bir ön sayfa sahip olarak modellenir. Ön sayfa kendi makale sayfaları sıralamasını belirtmek için özel karar hizmetini kullanır. Ön sayfasında HTML head aşağıdaki kodu ekleyin:

```html
// Define the "callback function" to render UI
<script> function callback(data) { … } </script>

// call Ranking API, after callback() is defined
<script src="https://ds.microsoft.com/api/v2/<appId>/rank/<actionSetId>" async></script>
```

`data` Bağımsız değişkeni işlenmek üzere URL'leri sıralamasını içerir. Daha fazla bilgi için bkz: başvurusu [API](custom-decision-service-api-reference.md).

Üst makale kullanıcı tıklama işlemek için aşağıdaki kod ön sayfasında arayın:

```javascript
// call Reward API to report a click
$.ajax({
    type: "POST",
    url: '//ds.microsoft.com/api/v2/<appId>/reward/' + data.eventId,
    contentType: "application/json" })
```

Burada, `data` bağımsız değişkenidir `callback()` işlevi. Bu uygulama örneği bulunabilir [Öğreticisi](custom-decision-service-tutorial-news.md#use-the-apis).

Son olarak, eylem özel karar hizmeti tarafından kabul edilmesi için (Eylemler) makalelerinin listesi döndüren API kümesini, sağlamanız gerekir. Bu API bir RSS akışı olarak aşağıda gösterildiği gibi uygulayın:

```xml
<rss version="2.0">
<channel>
   <item>
      <title><![CDATA[title (possibly with url) ]]></title>
      <link>url</link>
      <pubDate>Thu, 27 Apr 2017 16:30:52 GMT</pubDate>
    </item>
   <item>
       ....
   </item>
</channel>
</rss>
```

Burada, her üst düzey `<item>` öğesi bir makale açıklar. `<link>` Zorunludur ve bir eylem kimliği özel karar hizmeti tarafından kullanılır. Belirtin `<date>` (standart biçimdeki RSS) 15'ten fazla makaleleri varsa. 15 en son makaleleri kullanılır. `<title>` İsteğe bağlıdır ve makale için metin güvenlikle ilgili özellikler oluşturmak için kullanılır.

## <a name="next-steps"></a>Sonraki adımlar

* Aracılığıyla iş bir [öğretici](custom-decision-service-tutorial-news.md) daha ayrıntılı bir örnek için.
* Başvuru başvurun [API](custom-decision-service-api-reference.md) sağlanan işlevselliği hakkında daha fazla bilgi için.