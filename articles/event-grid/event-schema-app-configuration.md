---
title: Azure Event Grid Azure uygulama yapılandırması olay şeması
description: Azure uygulama yapılandırması olayları Azure Event Grid ile sağlanan özellikleri tanımlar
services: event-grid
author: jimmyca
ms.service: event-grid
ms.topic: reference
ms.date: 05/30/2019
ms.author: jimmyca
ms.openlocfilehash: fe0274f723692eea3cfd25cc0e9e146b35dce2ae
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66735789"
---
# <a name="azure-event-grid-event-schema-for-azure-app-configuration"></a>Azure uygulama yapılandırması için Azure Event Grid olay şeması

Bu makalede, Azure uygulama yapılandırma olayları için şema ve özellikleri sağlar. Olay şemaları için bir giriş için bkz [Azure Event Grid olay şeması](event-schema.md).

Örnek betikler ve öğreticiler listesi için bkz: [Azure uygulama yapılandırması olay kaynağı](event-sources.md#app-configuration).

## <a name="available-event-types"></a>Kullanılabilir olay türleri

Azure uygulama yapılandırması aşağıdaki olay türlerini gösterir:

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.AppConfiguration.KeyValueModified | Bir anahtar-değer oluşturulduğunda veya değiştirilen oluşturulur. |
| Microsoft.AppConfiguration.KeyValueDeleted | Bir anahtar-değer silindiğinde oluşturulur. |

## <a name="example-event"></a>Örnek olay

Aşağıdaki örnek, bir anahtar-değer değiştirilmiş olay şeması gösterir: 

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

Bir anahtar-değer silinen olayın şeması benzer: 

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
  "eventType": "Microsoft.AppConfiguration.KeyValueDeleted",
  "eventTime": "2019-05-31T20:05:03Z",
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```
 
## <a name="event-properties"></a>Olay Özellikleri

Bir olay aşağıdaki üst düzey veri vardır:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| topic | string | Olay kaynağı tam kaynak yolu. Bu alan, yazılabilir değil. Event Grid, bu değeri sağlar. |
| subject | string | Yayımcı tarafından tanımlanan olay konu yolu. |
| eventType | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Olayın oluşturulduğu zamandan, sağlayıcının UTC saatini temel alan. |
| id | string | Olayın benzersiz tanımlayıcısı. |
| data | object | Uygulama yapılandırması olay verileri. |
| dataVersion | string | Veri nesnesinin şema sürümü. Yayımcı, şema sürümü tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid, şemanın en üst düzey özellikleri tanımlar. Event Grid, bu değeri sağlar. |

Veri nesnesi, aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| anahtar | string | Anahtar-değer değiştirilen veya silinen anahtarı. |
| etiket | string | Etiketi, anahtar-değer, değiştirilmiş veya silinmiş. |
| ETag | string | İçin `KeyValueModified` yeni anahtar-değer etag'i. İçin `KeyValueDeleted` silindikten sonra anahtar-değer etag'i. |
 
## <a name="next-steps"></a>Sonraki adımlar

* Azure Event grid'e giriş için bkz [Event Grid nedir?](overview.md)
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid aboneliği şema](subscription-creation-schema.md).
* Azure uygulama yapılandırması olaylar ile çalışmayla giriş için bkz: [yol Azure uygulama yapılandırması olayları - Azure CLI](../azure-app-configuration/howto-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json). 