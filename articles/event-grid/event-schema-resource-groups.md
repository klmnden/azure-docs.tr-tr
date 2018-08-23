---
title: Azure Event Grid kaynak grubu olay şeması
description: Kaynak grubu olayları Azure Event Grid ile sağlanan özellikleri tanımlar
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: reference
ms.date: 08/17/2018
ms.author: tomfitz
ms.openlocfilehash: 22629ba553cc58435f99ed0fed97be252b24b409
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2018
ms.locfileid: "42058302"
---
# <a name="azure-event-grid-event-schema-for-resource-groups"></a>Kaynak grupları için Azure Event Grid olay şeması

Bu makale, kaynak grubu olayları için şema ve özellikleri sağlar. Olay şemaları için bir giriş için bkz [Azure Event Grid olay şeması](event-schema.md).

Azure Abonelikleriniz ve kaynak grupları, aynı olay türleri gösterin. Olay türleri, kaynakları değişikliklere ilgilidir. Birincil olayları kaynak grubu içindeki kaynaklar için kaynak grubu yayma ve Azure abonelikleri abonelik kaynaklarla ilgili olayları yayma farktır.

Kaynak olayları PUT, PATCH, oluşturulur ve silme işlemleri gönderilen `management.azure.com`. POST ve GET işlemleri olayları oluşturmayın. İşlemler için veri düzlemi gönderilen (gibi `myaccount.blob.core.windows.net`) olayları oluşturmayın.

Bir kaynak grubu için olaylara abone olduğunuzda, uç noktanız için kaynak grubunun tüm olaylarını alır. Olaylar, olay, bir sanal makine güncelleştiriliyor gibi görmek istediğiniz zamanda belki de yeni bir giriş dağıtım geçmişini yazma gibi sizin için önemli olmayan olaylar içerebilir. Tüm olaylar, uç noktada alabilir ve kullanmak istediğiniz olayları işleyen kod yazma ya da olay aboneliği oluştururken bir filtre ayarlayabilirsiniz.

Program aracılığıyla olayları işlemek için olayları bakarak sıralayabilirsiniz `operationName` değeri. Örneğin, olay uç noktanızı eşit olan işlemleri için olayları yalnızca işleyebilir `Microsoft.Compute/virtualMachines/write` veya `Microsoft.Storage/storageAccounts/write`.

Etkinlik konusu işlemin hedef kaynağın kaynak kimliğidir. Bir kaynak için olayları filtrelemek için bu kaynak sağlayan olay aboneliği oluştururken kimliği.  Bir kaynak türüne göre filtre uygulamak için aşağıdaki biçimde bir değer kullanın: `/subscriptions/<subscription-id>/resourcegroups/<resource-group>/providers/Microsoft.Compute/virtualMachines`

Örnek betikler ve öğreticiler listesi için bkz: [kaynak grubu olay kaynağı](event-sources.md#resource-groups).

## <a name="available-event-types"></a>Kullanılabilir olay türleri

Bir VM oluşturulduğunda veya bir depolama hesabı silinmiş gibi kaynak grupları Yönetimi olayları Azure Kaynak Yöneticisi'nden gösterin.

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
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}"
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
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}"
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
| veri | object | Kaynak grubu olay verileri. |
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

* Azure Event grid'e giriş için bkz [Event Grid nedir?](overview.md)
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid aboneliği şema](subscription-creation-schema.md).
