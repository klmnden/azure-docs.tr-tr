---
title: Azure Event Grid aboneliği olay şeması
description: Abonelik olayları Azure Event Grid ile sağlanan özellikleri tanımlar
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 08/02/2018
ms.author: tomfitz
ms.openlocfilehash: 59c41d4303cad00107793f3c09382347fbb1fb5b
ms.sourcegitcommit: fc5555a0250e3ef4914b077e017d30185b4a27e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39480394"
---
# <a name="azure-event-grid-event-schema-for-subscriptions"></a>Abonelikler için Azure Event Grid olay şeması

Bu makalede, Azure aboneliği olayları için şema ve özellikleri sağlar. Olay şemaları için bir giriş için bkz [Azure Event Grid olay şeması](event-schema.md).

Azure Abonelikleriniz ve kaynak grupları, aynı olay türleri gösterin. Olay türleri, kaynakları değişikliklere ilgilidir. Birincil olayları kaynak grubu içindeki kaynaklar için kaynak grubu yayma ve Azure abonelikleri abonelik kaynaklarla ilgili olayları yayma farktır.

## <a name="available-event-types"></a>Kullanılabilir olay türleri

Bir VM oluşturulduğunda veya bir depolama hesabı silinmiş gibi azure abonelikleri Yönetimi olayları Azure Kaynak Yöneticisi'nden gösterin.

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.Resources.ResourceWriteSuccess | Oluşturulan, ne zaman bir kaynak oluşturma veya güncelleştirme işlemi başarılı olur. |
| Microsoft.Resources.ResourceWriteFailure | Kaynak oluşturma veya güncelleştirme işlemi başarısız olduğunda oluşturulur. |
| Microsoft.Resources.ResourceWriteCancel | Oluşturulan, ne zaman bir kaynak oluşturma veya güncelleştirme işlemi iptal edildi. |
| Microsoft.Resources.ResourceDeleteSuccess | Bir kaynak silme işlemi başarılı olduğunda oluşturulur. |
| Microsoft.Resources.ResourceDeleteFailure | Bir kaynak silme işlemi başarısız olduğunda oluşturulur. |
| Microsoft.Resources.ResourceDeleteCancel | Bir kaynak silme işlemi iptal edildiğinde oluşturulur. Bu olay, bir şablon dağıtımı iptal edildiğinde gerçekleşir. |

## <a name="example-event"></a>Örnek olay

İçin şemayı aşağıdaki örnekte bir **ResourceWriteSuccess** olay. Aynı şemaya kullanılan **ResourceWriteFailure** ve **ResourceWriteCancel** olayları için farklı değerlerle `eventType`.

```json
[{
  "subject": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
  "eventType": "Microsoft.Resources.ResourceWriteSuccess",
  "eventTime": "2018-07-19T18:38:04.6117357Z",
  "id": "4db48cba-50a2-455a-93b4-de41a3b5b7f6",
  "data": {
    "authorization": {
      "scope": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
      "action": "Microsoft.Storage/storageAccounts/write",
      "evidence": {
        "role": "Subscription Admin"
      }
    },
    "claims": {
      "aud": "{audience-claim}",
      "iss": "{issuer-claim}",
      "iat": "{issued-at-claim}",
      "nbf": "{not-before-claim}",
      "exp": "{expiration-claim}",
      "_claim_names": "{\"groups\":\"src1\"}",
      "_claim_sources": "{\"src1\":{\"endpoint\":\"{URI}\"}}",
      "http://schemas.microsoft.com/claims/authnclassreference": "1",
      "aio": "{token}",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "rsa,mfa",
      "appid": "{ID}",
      "appidacr": "2",
      "http://schemas.microsoft.com/2012/01/devicecontext/claims/identifier": "{ID}",
      "e_exp": "{expiration}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "{last-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "{first-name}",
      "ipaddr": "{IP-address}",
      "name": "{full-name}",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "{ID}",
      "onprem_sid": "{ID}",
      "puid": "{ID}",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "{ID}",
      "http://schemas.microsoft.com/identity/claims/tenantid": "{ID}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "{user-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "{user-name}",
      "uti": "{ID}",
      "ver": "1.0"
    },
    "correlationId": "{ID}",
    "resourceProvider": "Microsoft.Storage",
    "resourceUri": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
    "operationName": "Microsoft.Storage/storageAccounts/write",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}"
  },
  "dataVersion": "2",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}"
}]
```

İçin şemayı aşağıdaki örnekte bir **ResourceDeleteSuccess** olay. Aynı şemaya kullanılan **ResourceDeleteFailure** ve **ResourceDeleteCancel** olayları için farklı değerlerle `eventType`.

```json
[{
  "subject": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
  "eventType": "Microsoft.Resources.ResourceDeleteSuccess",
  "eventTime": "2018-07-19T19:24:12.763881Z",
  "id": "19a69642-1aad-4a96-a5ab-8d05494513ce",
  "data": {
    "authorization": {
      "scope": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
      "action": "Microsoft.Storage/storageAccounts/delete",
      "evidence": {
        "role": "Subscription Admin"
      }
    },
    "claims": {
      "aud": "{audience-claim}",
      "iss": "{issuer-claim}",
      "iat": "{issued-at-claim}",
      "nbf": "{not-before-claim}",
      "exp": "{expiration-claim}",
      "_claim_names": "{\"groups\":\"src1\"}",
      "_claim_sources": "{\"src1\":{\"endpoint\":\"{URI}\"}}",
      "http://schemas.microsoft.com/claims/authnclassreference": "1",
      "aio": "{token}",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "rsa,mfa",
      "appid": "{ID}",
      "appidacr": "2",
      "http://schemas.microsoft.com/2012/01/devicecontext/claims/identifier": "{ID}",
      "e_exp": "262800",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "{last-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "{first-name}",
      "ipaddr": "{IP-address}",
      "name": "{full-name}",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "{ID}",
      "onprem_sid": "{ID}",
      "puid": "{ID}",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "{ID}",
      "http://schemas.microsoft.com/identity/claims/tenantid": "{ID}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "{user-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "{user-name}",
      "uti": "{ID}",
      "ver": "1.0"
    },
    "correlationId": "{ID}",
    "httpRequest": {
      "clientRequestId": "{ID}",
      "clientIpAddress": "{IP-address}",
      "method": "DELETE",
      "url": "https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}?api-version=2018-02-01"
    },
    "resourceProvider": "Microsoft.Storage",
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
    "operationName": "Microsoft.Storage/storageAccounts/delete",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}"
  },
  "dataVersion": "2",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}"
}]
```

## <a name="event-properties"></a>Olay Özellikleri

Bir olay aşağıdaki üst düzey veri vardır:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| konu başlığı | dize | Olay kaynağı tam kaynak yolu. Bu alan, yazılabilir değil. Event Grid, bu değeri sağlar. |
| Konu | dize | Yayımcı tarafından tanımlanan olay konu yolu. |
| olay türü | dize | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | dize | Olayın oluşturulduğu zamandan, sağlayıcının UTC saatini temel alan. |
| id | dize | Olayın benzersiz tanımlayıcısı. |
| veri | object | Abonelik olay verileri. |
| dataVersion | dize | Veri nesnesinin şema sürümü. Yayımcı, şema sürümü tanımlar. |
| metadataVersion | dize | Olay meta verilerinin şema sürümü. Event Grid, şemanın en üst düzey özellikleri tanımlar. Event Grid, bu değeri sağlar. |

Veri nesnesi, aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Yetkilendirme | object | İşlem için istenen yetkilendirme. |
| Talep | object | Talep özellikleri. Daha fazla bilgi için [JWT belirtimi](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html). |
| correlationId | dize | Sorun giderme için bir işlem kimliği. |
| HTTP isteği | object | İşlem ayrıntıları. Bu nesne yalnızca olan mevcut bir kaynağı güncelleştirirken dahil ya da bir kaynak siliniyor. |
| ResourceProvider | dize | İşlemi kaynak sağlayıcısı. |
| resourceUri | dize | İşlemi kaynak URI'si. |
| operationName | dize | Gerçekleştirilen işlem. |
| durum | dize | İşlemin durumu. |
| subscriptionId | dize | Kaynak abonelik kimliği. |
| Kiracı kimliği | dize | Kaynak Kiracı kimliği. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Event grid'e giriş için bkz [Event Grid nedir?](overview.md).
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid aboneliği şema](subscription-creation-schema.md).
