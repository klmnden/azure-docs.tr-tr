---
title: "Azure olay kılavuz abonelik şeması"
description: "Azure olay kılavuz olan abonelik olaylar için sağlanan özellikler açıklar"
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 11/08/2017
ms.author: tomfitz
ms.openlocfilehash: 7dc10d0cc73960fac4759a0cebec8d294cf1b463
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2017
---
# <a name="azure-event-grid-event-schema-for-subscriptions"></a>Abonelikler için Azure olay kılavuz şeması

Bu makale, Azure aboneliği olaylar için şema ve özellikleri sağlar. Olay şemaları giriş için bkz: [Azure olay kılavuz olay şema](event-schema.md).

Azure Abonelikleriniz ve kaynak grupları aynı olay türlerini yayma. Olay türleri kaynakları değişiklikleri ilgilidir. Birincil olayları kaynak grubunda kaynaklar için kaynak gruplarını yayma ve Azure abonelikleri olayları kaynaklar için abonelik üzerinden yayma farktır.

## <a name="available-event-types"></a>Kullanılabilir olay türleri

Bir VM oluşturulduğunda veya bir depolama hesabı silinir gibi azure abonelikleri Yönetimi olayları Azure Kaynak Yöneticisi'nden yayma.

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.Resources.ResourceWriteSuccess | Yükseltilmiş ne zaman bir kaynak oluşturma veya güncelleştirme işlemi başarılı olur. |
| Microsoft.Resources.ResourceWriteFailure | Kaynak oluşturma veya güncelleştirme işlemi başarısız olduğunda oluşturulur. |
| Microsoft.Resources.ResourceWriteCancel | Yükseltilmiş ne zaman bir kaynak oluşturma veya güncelleştirme işlemi iptal edildi. |
| Microsoft.Resources.ResourceDeleteSuccess | Bir kaynak silme işlemi başarılı olduğunda oluşturulur. |
| Microsoft.Resources.ResourceDeleteFailure | Bir kaynak silme işlemi başarısız olduğunda oluşturulur. |
| Microsoft.Resources.ResourceDeleteCancel | Bir kaynak silme işlemi iptal edildiğinde oluşturulur. Bu olay, bir şablon dağıtımı iptal ettiğinizde gerçekleşir. |

## <a name="example-event"></a>Örnek olayı

Aşağıdaki örnek, olay oluşturulan bir kaynak şeması gösterir: 

```json
[
    {
    "topic":"/subscriptions/{subscription-id}",
    "subject":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
    "eventType":"Microsoft.Resources.ResourceWriteSuccess",
    "eventTime":"2017-08-16T03:54:38.2696833Z",
    "id":"25b3b0d0-d79b-44d5-9963-440d4e6a9bba",
    "data": {
        "authorization":"{azure_resource_manager_authorizations}",
        "claims":"{azure_resource_manager_claims}",
        "correlationId":"54ef1e39-6a82-44b3-abc1-bdeb6ce4d3c6",
        "httpRequest":"{request-operation}",
        "resourceProvider":"Microsoft.EventGrid",
        "resourceUri":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
        "operationName":"Microsoft.EventGrid/eventSubscriptions/write",
        "status":"Succeeded",
        "subscriptionId":"{subscription-id}",
        "tenantId":"72f988bf-86f1-41af-91ab-2d7cd011db47"
        },
    }
]
```

Silinen kaynak olay için şemayı benzer:

```json
[{
  "topic":"/subscriptions/{subscription-id}",
  "subject": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicApp0ecd6c02-2296-4d7c-9865-01532dc99c93",
  "eventType": "Microsoft.Resources.ResourceDeleteSuccess",
  "eventTime": "2017-11-07T21:24:19.6959483Z",
  "id": "7995ecce-39d4-4851-b9d7-a7ef87a06bf5",
  "data": {
    "authorization": "{azure_resource_manager_authorizations}",
    "claims": "{azure_resource_manager_claims}",
    "correlationId": "7995ecce-39d4-4851-b9d7-a7ef87a06bf5",
    "httpRequest": "{request-operation}",
    "resourceProvider": "Microsoft.EventGrid",
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
    "operationName": "Microsoft.EventGrid/eventSubscriptions/delete",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "72f988bf-86f1-41af-91ab-2d7cd011db47"
  }
}]
```

## <a name="event-properties"></a>Olay Özellikleri

Bir olay aşağıdaki üst düzey veri sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Konu | Dize | Olay kaynağı tam kaynak yolu. Bu alan yazılabilir değil. |
| Konu | Dize | Olay konu yayımcı tarafından tanımlanan yolu. |
| Olay türü | Dize | Bu olay kaynağı için kayıtlı olay türünden biri. |
| EventTime | Dize | Olayı oluşturan zaman sağlayıcının UTC zamanı temel alınarak. |
| id | Dize | Olay için benzersiz tanımlayıcı. |
| Veri | Nesne | Abonelik olay verileri. |

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Yetkilendirme | Dize | İstenen Yetkilendirme işlemi için. |
| Talepleri | Dize | Talep özellikleri. |
| correlationId | Dize | Sorun giderme için bir işlem kimliği. |
| httpRequest | Dize | İşlem ayrıntıları. |
| resourceProvider | Dize | İşlemi gerçekleştiren kaynak sağlayıcısı. |
| resourceUri | Dize | İşlemi kaynak URI'si. |
| operationName | Dize | Gerçekleştirilen işlem. |
| durum | Dize | İşlem durumu. |
| subscriptionId | Dize | Kaynak abonelik kimliği. |
| Tenantıd | Dize | Kaynak Kiracı kimliği. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure olay kılavuz giriş için bkz: [olay kılavuz nedir?](overview.md).
* Bir Azure olay kılavuz abonelik oluşturma hakkında daha fazla bilgi için bkz: [olay kılavuz abonelik şema](subscription-creation-schema.md).
