---
title: Kişiselleştirme - Azure Bilişsel hizmetler makale | Microsoft Docs
description: Bir öğretici için Azure özel karar Service, bulut tabanlı bir API bağlamsal dağıtılmasında makale kişiselleştirme.
services: cognitive-services
author: slivkins
manager: slivkins
ms.service: cognitive-services
ms.topic: article
ms.date: 05/08/2018
ms.author: slivkins;marcozo;alekh;marossi
ms.openlocfilehash: 35d0567f81a23d4726461059eb6fd31e04228697
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354587"
---
# <a name="article-personalization"></a>Makale kişiselleştirme

Bu öğretici, bir Web sitesi ön sayfasında makalelerinin seçimini kişiselleştirme odaklanır. Özel karar Hizmeti'ni etkiler *birden çok* ön makaleleri listesi sayfası, örneğin. Belki de yalnızca siyaset ve Spor kapsayan bir haber Web sayfasıdır. Makalelerin üç dereceli listeleri göstermeniz: siyaset, spor ve son.

## <a name="applications-and-action-sets"></a>Uygulamalar ve eylem kümeleri

Framework'e senaryonuza uyacak şekilde etkinleştirilir. Şimdi en iyileştirilmemiş her listesi için üç uygulama düşünün: uygulama siyaset, app-Spor ve uygulama son. Her uygulama için aday makaleleri belirtmek için iki eylem kümesi bulunur: siyaset, diğeri spor için için. İçin uygulama son ayarlanan eylemi otomatik olarak diğer iki kümelerinin UNION gelir.

> [!TIP]
> Eylem kümelerini özel karar hizmetinde uygulamalar arasında paylaşılabilir.

## <a name="prepare-action-set-feeds"></a>Eylem kümesini akışları hazırlama

Özel karar hizmet müşteri tarafından sağlanan RSS veya Atom akışları aracılığıyla eylem kümelerini kullanır. İki akışları sağlar: biri siyaset, diğeri spor için. Gelen sunulan varsayalım `http://www.domain.com/feeds/<feed-name>`.

Her akış makalelerin listesini sağlar. RSS, her biri tarafından belirtilen bir `<item>` şekilde öğesi:

```xml
<rss version="2.0"><channel>
   <item>
      <title><![CDATA[article title]]></title>
      <link>"article url"</link>
      <pubDate>publication date</pubDate>
    </item>
</channel></rss>
```

Makaleler önemlidir sırası. Makaleleri nasıl sıralanacağını, en iyi tahmin olan varsayılan derecelendirme belirtir. Varsayılan derecelendirme sonra performans karşılaştırma için kullanılan [Pano](#performance-dashboard).

Akış biçimi hakkında daha fazla bilgi için bkz: [API Başvurusu](custom-decision-service-api-reference.md#action-set-api-customer-provided).

## <a name="register-a-new-app"></a>Yeni uygulamayı kaydedin

1. Oturum açmak, [Microsoft hesabı](https://account.microsoft.com/account). Şerit'te tıklatın **My Portal**.

2. Yeni bir uygulama kaydolmak için **yeni uygulama** düğmesi.

    ![Özel Karar Alma Hizmeti portalı](./media/custom-decision-service-tutorial/portal.png)

3. Uygulamanızda için benzersiz bir ad girin **uygulama kimliği** metin kutusu. Bu ad zaten başka bir müşteri tarafından kullanılıyorsa, sistem, farklı uygulama kimliğini almak için ister Seçin **Gelişmiş** onay kutusunu işaretleyin ve girin [bağlantı dizesi](../../storage/common/storage-configure-connection-string.md) Azure depolama hesabınız için. Normalde, tüm uygulamalar için aynı depolama hesabı kullanın.

    ![Yeni uygulama iletişim kutusu](./media/custom-decision-service-tutorial/new-app-dialog.png)

    Yukarıdaki senaryoda üç uygulama kaydettikten sonra listelenmiştir:

    ![Uygulama listesi](./media/custom-decision-service-tutorial/apps.png)

    Bu listeye tıklayarak geri gelebilir **uygulamaları** düğmesi.

4. İçinde **yeni uygulama** iletişim kutusunda, bir eylem akış belirtin. Eylem akışları de belirtilebilir tıklayarak **akışları** düğmesine ve ardından tıklayarak **yeni akış** düğmesi. Girin bir **adı** yeni akış için girin **URL** aldığı sunulur ve girin **yenileme zamanı**. Özel karar hizmet akışın ne sıklıkta yenilemelisiniz yenileme süresini belirtir.

    ![Yeni akış iletişim kutusu](./media/custom-decision-service-tutorial/new-feed-dialog.png)

    Eylem akışları burada belirtilen bağımsız olarak tüm uygulama tarafından kullanılabilir. Bir senaryoda her iki eylem akışları belirttikten sonra bunlar listelenir:

    ![Akışları listesi](./media/custom-decision-service-tutorial/feeds.png)

    Bu listeye tıklayarak geri gelebilir **akışları** düğmesi.

## <a name="use-the-apis"></a>API'leri kullanın

Özel karar hizmet makaleleri sıralaması API aracılığıyla derecelendirir. Bu API kullanmak için ön sayfasında HTML head aşağıdaki kodu ekleyin:

```html
<!-- Define the "callback function" to render UI -->
<script> function callback(data) { … } </script>

<!--  call Ranking API after callback() is defined, separately for each app -->
<script src="https://ds.microsoft.com/api/v2/app-politics/rank/feed-politics" async></script>
<script src="https://ds.microsoft.com/api/v2/app-sports/rank/feed-sports" async></script>
<script src="https://ds.microsoft.com/api/v2/app-recent/rank/feed-politics/feed-sports" async></script>
<!-- NB: action feeds for 'app-recent' are listed one after another. -->
```

HTTP yanıtı sıralaması API'sinden JSONP biçimli bir dizedir. App-siyaset, örneğin, dize şuna benzer:

```json
callback({
   "ranking":[{"id":"url1"}, {"id":"url2"}, {"id":"url3"}],
   "eventId":"<opaque event string>",
   "appId":"app-politics",
   "actionSets":[{"id":"feed-politics","lastRefresh":"date"}] });
```

Tarayıcı için bir çağrı olarak bu dize ardından yürütür `callback()` işlevi. `data` Değişkeninde `callback()` uygulama kimliği ve URL'leri sıralamasını işlenmek üzere işlevi içerir. Özellikle, `callback()` kullanması gereken `data.appId` üç uygulamalar arasında ayrım yapmak için. `eventId` Özel karar hizmeti tarafından sağlanan karşılık gelen bir tıklatmayla sıralaması varsa eşleşmesi için dahili olarak kullanılır.

> [!TIP]
> `callback()` Her eylem için yenilik kullanarak akış kontrol `lastRefresh` alan. Belirli bir akış yeterince yeni değilse `callback()` , sağlanan derecelendirme yoksay, bu akışın doğrudan çağırabilir ve akış tarafından sunulan varsayılan derecelendirme kullanın.

Belirtimleri ve sıralaması API'si tarafından sağlanan ek seçenekler hakkında daha fazla bilgi için bkz: [API Başvurusu](custom-decision-service-api-reference.md).

Ödül API'sini çağırarak kullanıcı üst makale seçeneklerden döndürülür. Bir üst makale seçim alındığında, aşağıdaki kodu ön sayfasında çağrılır:

```javascript
$.ajax({
    type: "POST",
    url: '//ds.microsoft.com/api/v2/<appId>/reward/<eventId>',
    contentType: "application/json" })
```

Kullanarak `appId` ve `eventId` tıklatın işleme kodda bazı dikkat gerektirir. Örneğin, uygulama `callback()` gibi işlev:

```javascript
function callback(data) {
    $.map(data.ranking, function (element) {
        //custom rendering function given content info
        render({appId:data.appId,
                article:element,
                onClick: element.id != data.rewardAction ? null :
                   function() {
                       $.ajax({
                       type: "POST",
                       url: '//ds.microsoft.com/api/v2/' + data.appId + '/reward/' + data.eventId,
                       contentType: "application/json" })}
                });
}}
```

Bu örnekte, uygulama `render()` belirli bir uygulamada belirli bir makaleye işlenecek işlevi. Bu işlev, uygulama kimliği ve makaleyi (biçiminde sıralaması API'sinden) girilir. `onClick` Parametredir öğesinden çağrılması işlevi `render()` tıklama işlemek için. Onu tıklayın üst yuvada olup olmadığını denetler. Olay Kimliği ve uygun uygulama kimliği ile ödül API çağrıları

## <a name="next-steps"></a>Sonraki adımlar

* Başvurun [API Başvurusu](custom-decision-service-api-reference.md) sağlanan işlevselliği hakkında daha fazla bilgi için.