---
title: Azure uygulama yapılandırması anahtar-değer olayları tanımlandığında | Microsoft Docs
description: Uygulama yapılandırma olaylarına abone olmak için Azure Event grid'i kullanın.
services: azure-app-configuration,event-grid
author: jimmyca
ms.author: jimmyca
ms.date: 05/30/2019
ms.topic: article
ms.service: azure-app-configuration
ms.openlocfilehash: 601124aef37d2b285db71130f5c63b3620c7768f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66735654"
---
# <a name="reacting-to-azure-app-configuration-events"></a>Azure uygulama yapılandırması olaylara tepki verme

Azure uygulama yapılandırma olayları, uygulamalar, anahtar-değer değişikliklere tepki vermek etkinleştirin. Bu, karmaşık kod veya pahalı ve verimsiz yoklama Hizmetleri gerek kalmadan gerçekleştirilir. Bunun yerine, olayların gönderilmesini [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) gibi abonelere [Azure işlevleri](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), ve hatta kendi özel http dinleyicisi ve yalnızca kullandığınız kadarı için ödeme yaparsınız.

Azure uygulama yapılandırması olayları Azure Event zengin bir deneyimle uygulamalarınızda güvenilir teslim hizmetleri yeniden deneme ilkeleri ve teslim edilemeyen sağlayan kılavuza gönderilir. Daha fazla bilgi için bkz. [Event Grid iletiyi teslim ve yeniden deneme](https://docs.microsoft.com/azure/event-grid/delivery-and-retry).

Uygulama yapılandırması, dağıtımları veya bir yapılandırma odaklı iş akışı tetikleyen yenileme yaygın uygulama yapılandırma olay senaryolar şunlardır. Seyrek görülen değişiklikler, ancak senaryonuza anında yanıt verme hızını gerektirir, olay tabanlı mimari özellikle etkili olabilir.

Bir göz atın [yol Azure uygulama yapılandırması olaylarını bir özel web uç noktası - CLI](./howto-app-configuration-event.md) hızlı bir örnek için. 

![Event Grid modeli](./media/event-grid-functional-model.png)

## <a name="available-azure-app-configuration-events"></a>Mevcut Azure uygulama yapılandırma olayları
Event grid'i kullanır [olay abonelikleri](../event-grid/concepts.md#event-subscriptions) abonelere olay iletileri yönlendirmek için. Azure uygulama yapılandırması olay abonelikleri, iki tür olay şunları içerebilir:  

> |Olay adı|Açıklama|
> |----------|-----------|
> |`Microsoft.AppConfiguration.KeyValueModified`|Bir anahtar-değer oluşturulan veya değiştirilen başlatıldı|
> |`Microsoft.AppConfiguration.KeyValueDeleted`|Bir anahtar-değer silindiğinde tetiklendi|

## <a name="event-schema"></a>Olay şeması
Azure uygulama yapılandırması olayları verilerinizdeki değişiklikleri yanıtlamak için gereken tüm bilgileri içerir. EventType özelliği "Microsoft.AppConfiguration" ile başladığından bir uygulama yapılandırma olayı belirleyebilirsiniz. Event Grid olay özelliklerinin kullanımı hakkında ek bilgi belirtilmiştir [Event Grid olay şeması](../event-grid/event-schema.md).  

> |Özellik|Tür|Açıklama|
> |-------------------|------------------------|-----------------------------------------------------------------------|
> |topic|string|Tam Azure Resource Manager kimliğini yayan olay uygulama yapılandırması.|
> |subject|string|Anahtar-değer konusu olayın URI'si.|
> |eventTime|string|Bu olayın oluşturulduğu, ISO 8601 biçiminde tarih.|
> |eventType|string|"Microsoft.AppConfiguration.KeyValueModified" veya "Microsoft.AppConfiguration.KeyValueDeleted".|
> |Kimlik|string|Bu etkinliğin benzersiz tanımlayıcısı.|
> |dataVersion|string|Veri nesnesinin şema sürümü.|
> |metadataVersion|string|Üst düzey özellikler şema sürümü.|
> |data|object|Azure uygulama yapılandırması belirli olay verileri koleksiyonu|
> |Data.Key|string|Anahtar-değer değiştirilen veya silinen anahtarı.|
> |Data.Label|string|Etiketi, anahtar-değer, değiştirilmiş veya silinmiş.|
> |Data.ETag|string|İçin `KeyValueModified` yeni anahtar-değer etag'i. İçin `KeyValueDeleted` silindikten sonra anahtar-değer etag'i.|

KeyValueModified olayın bir örnek aşağıda verilmiştir:
```json
[{
  "id": "84e17ea4-66db-4b54-8050-df8f7763f87b",
  "topic": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/microsoft.appconfiguration/configurationstores/contoso",
  "subject": "https://contoso.azconfig.io/kv/Foo?label=FizzBuzz",
  "data": {
    "key": "Foo",
    "label": "FizzBuzz",
    "etag": "FnUExLaj2moIi4tJX9AXn9sakm0"
  },
  "eventType": "Microsoft.AppConfiguration.KeyValueModified",
  "eventTime": "2019-05-31T20:05:03Z",
  "dataVersion": "1",
  "metadataVersion": "1"
}]

```

Daha fazla bilgi için [Azure uygulama yapılandırma olayları şeması](../event-grid/event-schema-app-configuration.md).

## <a name="practices-for-consuming-events"></a>Olayları kullanan uygulamalar
Uygulama yapılandırma olaylarını işleme uygulamaları birkaç önerilen uygulamaları izlemelisiniz:
> [!div class="checklist"]
> * Birden çok abonelik aynı olay işleyicisi için rota olayları yapılandırılması gibi belirli bir kaynaktan gelen olayları olduğu varsayılır değil ancak beklediğiniz uygulama yapılandırmasından geldiğinden emin olmak için iletisinin konu denetlemek için önemlidir.
> * Benzer şekilde, eventType tek bir işlem için hazır ve aldığınız tüm olayları beklediğiniz türleri olacağını varsaymayın olup olmadığını denetleyin.
> * Bazı gecikmeden sonra ve iletileri sıralamaya gelen nesnelerle ilgili bilgilerinizi hala güncel olup olmadığını anlamak için etag alanları kullanın.  Ayrıca, herhangi bir nesne üzerinde olayların sırası anlamak için sıralayıcı'yı alanlarını kullanın.
> * Anahtar-değer değiştirildiği erişmek için konu alanını kullanın.


## <a name="next-steps"></a>Sonraki adımlar

Event Grid hakkında daha fazla bilgi edinin ve Azure uygulama yapılandırma olayları deneyin:

- [Event Grid Hakkında](../event-grid/overview.md)
- [Azure uygulama yapılandırma olaylarını bir özel web uç noktasına yönlendirme](./howto-app-configuration-event.md)