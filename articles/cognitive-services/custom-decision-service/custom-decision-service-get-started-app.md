---
title: Bir app - Azure Bilişsel hizmetler API çağrısı | Microsoft Docs
description: Smartphone uygulamadan API'leri çağırırsanız Azure özel karar hizmetiyle nereden başlayacaksınız.
services: cognitive-services
author: slivkins
manager: slivkins
ms.service: cognitive-services
ms.topic: article
ms.date: 05/10/2018
ms.author: slivkins
ms.reviewer: marcozo, alekh
ms.openlocfilehash: 2d02b0deaaa701dd0b4818638827c04e2c946558
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355691"
---
# <a name="call-api-from-an-app"></a>Uygulamadan API çağrısı yapma

Smartphone uygulamasından Azure özel karar hizmet API'lerine çağrıları yapma. Bu makalede, bazı temel seçenekler ile çalışmaya başlama açıklanmaktadır.

Şunları yaptığınızdan emin olun [uygulamanızı kaydetme](custom-decision-service-get-started-register.md), ilk.

Özel karar hizmetine smartphone uygulamanızdan olun iki API çağrıları vardır: içeriğinizi ve bir ödül rapor yapılan bir çağrı ödül API sıralı bir listesini almak için bir çağrı sıralaması API. Örnek çağrılarında işte [cURL](https://en.wikipedia.org/wiki/CURL).

Daha fazla bilgi için bkz: başvurusu [API](custom-decision-service-api-reference.md).

Derecelendirme API çağrısı ile başlayın. Dosyayı oluşturma `<request.json>`, hangi eylemini kimliği ayarlanmış Bu kimliği karşılık gelen RSS adıdır veya portalda girdiğiniz Atom akışı:

```json
{"decisions":
     [{ "actionSets":[{"id":{"id":"<actionSetId>"}}] }]}
```

Birçok eylem kümelerini gibi belirtilebilir:

```json
{"decisions":
    [{ "actionSets":[{"id":{"id":"<actionSetId1>"}},
                     {"id":{"id":"<actionSetId2>"}}] }]}
```

Bu JSON dosyası derecelendirme isteğin bir parçası gönderilir:

```shell
curl -d @<request.json> -X POST https://ds.microsoft.com/api/v2/<appId>/rank --header "Content-Type: application/json"
```

Burada, `<appId>` uygulamanızın adı portalda kaydedilir. Uygulamanızda işleyebilen içerik öğeleri sıralı bir dizi almanız gerekir. Dönüş bir örnek şuna benzer:

```json
[{ "ranking":[{"id":"actionId3"}, {"id":"actionId1"}, {"id":"actionId2"}],
   "eventId":"<opaque event string>",
   "appId":"<your app id>",
   "rewardAction":"actionId3",
   "actionSets":[{"id":"<actionSetId1>","lastRefresh":"2017-04-29T22:34:25.3401438Z"},
                 {"id":"<actionSetId2>","lastRefresh":"2017-04-30T22:34:25.3401438Z"}]}]
```

Dönüş ilk parçası eylem kimlikleri tarafından belirtilen sıralı eylemlerin bir listesini içerir. Bir makale için bir URL eylem kimliğidir. Genel istek de benzersiz olan `<eventId>`, sistem tarafından oluşturulmuş.

Daha sonra ilk içerik öğesi olduğundan bu olaydan tıklama gözlenen varsa belirtebilirsiniz `<actionId3>`. Ardından bir ödül bu raporlayabilirsiniz `<eventId>` ödül API aracılığıyla özel karar hizmet için başka bir işlemle isteği gibi:

```shell
curl -v https://ds.microsoft.com/api/v2/<appId>/reward/<eventId> -X POST
```

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

Burada, her üst düzey `<item>` öğesi bir makale açıklar. `<link>` Zorunludur ve bir eylem kimliği özel karar hizmeti tarafından kullanılır. Belirtin `<date>` (standart biçimdeki RSS) 15'ten fazla makaleleri olduğunuz durumunda. 15 en son makaleleri kullanılır. `<title>` İsteğe bağlıdır ve makale için metin güvenlikle ilgili özellikler oluşturmak için kullanılır.

## <a name="next-steps"></a>Sonraki adımlar

* Aracılığıyla iş bir [öğretici](custom-decision-service-tutorial-news.md) daha ayrıntılı bir örnek için.
* Başvuru başvurun [API](custom-decision-service-api-reference.md) sağlanan işlevselliği hakkında daha fazla bilgi için.