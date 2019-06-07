---
title: Azure Event Grid kaynak grubu olay şeması
description: Kaynak grubu olayları Azure Event Grid ile sağlanan özellikleri tanımlar
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: reference
ms.date: 01/12/2019
ms.author: spelluru
ms.openlocfilehash: 6cbfc06f380d7c4818ca82e858c23bb18849fb7c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60561702"
---
# <a name="azure-event-grid-event-schema-for-resource-groups"></a>Kaynak grupları için Azure Event Grid olay şeması

Bu makale, kaynak grubu olayları için şema ve özellikleri sağlar. Olay şemaları için bir giriş için bkz [Azure Event Grid olay şeması](event-schema.md).

Azure Abonelikleriniz ve kaynak grupları, aynı olay türleri gösterin. Olay türlerini, kaynak değişiklikleri veya Eylemler ilgilidir. Birincil olayları kaynak grubu içindeki kaynaklar için kaynak grubu yayma ve Azure abonelikleri abonelik kaynaklarla ilgili olayları yayma farktır.

Kaynak olayları için PUT, PATCH, GÖNDERİ oluşturulur ve silme işlemleri gönderilen `management.azure.com`. ALMA işlemleri, olayları oluşturmayın. İşlemler için veri düzlemi gönderilen (gibi `myaccount.blob.core.windows.net`) olayları oluşturmayın. Eylem olaylar, bir kaynak için anahtarları listeleme gibi işlemler için olay verilerini sağlar.

Bir kaynak grubu için olaylara abone olduğunuzda, uç noktanız için kaynak grubunun tüm olaylarını alır. Olaylar, olay, bir sanal makine güncelleştiriliyor gibi görmek istediğiniz zamanda belki de yeni bir giriş dağıtım geçmişini yazma gibi sizin için önemli olmayan olaylar içerebilir. Tüm olayları, uç noktada almak ve kullanmak istediğiniz olayları işleyen kod yazın. Veya olay aboneliği oluştururken bir filtre ayarlayabilirsiniz.

Program aracılığıyla olayları işlemek için olayları bakarak sıralayabilirsiniz `operationName` değeri. Örneğin, olay uç noktanızı eşit olan işlemleri için olayları yalnızca işleyebilir `Microsoft.Compute/virtualMachines/write` veya `Microsoft.Storage/storageAccounts/write`.

Etkinlik konusu işlemin hedef kaynağın kaynak kimliğidir. Bir kaynak için olayları filtrelemek için bu kaynak sağlayan olay aboneliği oluştururken kimliği.  Bir kaynak türüne göre filtre uygulamak için aşağıdaki biçimde bir değer kullanın: `/subscriptions/<subscription-id>/resourcegroups/<resource-group>/providers/Microsoft.Compute/virtualMachines`

Örnek betikler ve öğreticiler listesi için bkz: [kaynak grubu olay kaynağı](event-sources.md#resource-groups).

## <a name="available-event-types"></a>Kullanılabilir olay türleri

Bir VM oluşturulduğunda veya bir depolama hesabı silinmiş gibi kaynak grupları Yönetimi olayları Azure Kaynak Yöneticisi'nden gösterin.

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.Resources.ResourceActionCancel | Kaynak üzerinde işlem iptal edildiğinde oluşturulur. |
| Microsoft.Resources.ResourceActionFailure | Kaynak eylem başarısız olduğunda oluşturulur. |
| Microsoft.Resources.ResourceActionSuccess | Kaynak üzerinde işlem başarılı olduğunda oluşturulur. |
| Microsoft.Resources.ResourceDeleteCancel | Arandığında silme işlemi iptal edildi. Bu olay, bir şablon dağıtımı iptal edildiğinde gerçekleşir. |
| Microsoft.Resources.ResourceDeleteFailure | Arandığında delete işlemi başarısız oluyor. |
| Microsoft.Resources.ResourceDeleteSuccess | Arandığında silme işlemi başarılı olur. |
| Microsoft.Resources.ResourceWriteCancel | Arandığında oluşturma veya güncelleştirme işlemi iptal edildi. |
| Microsoft.Resources.ResourceWriteFailure | Arandığında oluşturma veya güncelleştirme işlemi başarısız olur. |
| Microsoft.Resources.ResourceWriteSuccess | Arandığında oluşturma veya güncelleştirme işlemi başarılı olur. |

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

İçin şemayı aşağıdaki örnekte bir **ResourceActionSuccess** olay. Aynı şemaya kullanılan **ResourceActionFailure** ve **ResourceActionCancel** olayları için farklı değerlerle `eventType`.

```json
[{   
  "subject": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventHub/namespaces/{namespace}/AuthorizationRules/RootManageSharedAccessKey",
  "eventType": "Microsoft.Resources.ResourceActionSuccess",
  "eventTime": "2018-10-08T22:46:22.6022559Z",
  "id": "{ID}",
  "data": {
    "authorization": {
      "scope": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventHub/namespaces/{namespace}/AuthorizationRules/RootManageSharedAccessKey",
      "action": "Microsoft.EventHub/namespaces/AuthorizationRules/listKeys/action",
      "evidence": {
        "role": "Contributor",
        "roleAssignmentScope": "/subscriptions/{subscription-id}",
        "roleAssignmentId": "{ID}",
        "roleDefinitionId": "{ID}",
        "principalId": "{ID}",
        "principalType": "ServicePrincipal"
      }     
    },
    "claims": {
      "aud": "{audience-claim}",
      "iss": "{issuer-claim}",
      "iat": "{issued-at-claim}",
      "nbf": "{not-before-claim}",
      "exp": "{expiration-claim}",
      "aio": "{token}",
      "appid": "{ID}",
      "appidacr": "2",
      "http://schemas.microsoft.com/identity/claims/identityprovider": "{URL}",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "{ID}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "{ID}",       "http://schemas.microsoft.com/identity/claims/tenantid": "{ID}",
      "uti": "{ID}",
      "ver": "1.0"
    },
    "correlationId": "{ID}",
    "httpRequest": {
      "clientRequestId": "{ID}",
      "clientIpAddress": "{IP-address}",
      "method": "POST",
      "url": "https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventHub/namespaces/{namespace}/AuthorizationRules/RootManageSharedAccessKey/listKeys?api-version=2017-04-01"
    },
    "resourceProvider": "Microsoft.EventHub",
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventHub/namespaces/{namespace}/AuthorizationRules/RootManageSharedAccessKey",
    "operationName": "Microsoft.EventHub/namespaces/AuthorizationRules/listKeys/action",
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
| topic | string | Olay kaynağı tam kaynak yolu. Bu alan, yazılabilir değil. Event Grid, bu değeri sağlar. |
| subject | string | Yayımcı tarafından tanımlanan olay konu yolu. |
| eventType | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Olayın oluşturulduğu zamandan, sağlayıcının UTC saatini temel alan. |
| id | string | Olayın benzersiz tanımlayıcısı. |
| data | object | Kaynak grubu olay verileri. |
| dataVersion | string | Veri nesnesinin şema sürümü. Yayımcı, şema sürümü tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid, şemanın en üst düzey özellikleri tanımlar. Event Grid, bu değeri sağlar. |

Veri nesnesi, aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| authorization | object | İşlem için istenen yetkilendirme. |
| claims | object | Talep özellikleri. Daha fazla bilgi için [JWT belirtimi](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html). |
| correlationId | string | Sorun giderme için bir işlem kimliği. |
| httpRequest | object | İşlem ayrıntıları. Bu nesne yalnızca olan mevcut bir kaynağı güncelleştirirken dahil ya da bir kaynak siliniyor. |
| resourceProvider | string | İşlemi kaynak sağlayıcı. |
| resourceUri | string | İşlemi kaynak URI'si. |
| operationName | string | Alınan işlemi. |
| status | string | İşlemin durumu. |
| subscriptionId | string | Kaynak abonelik kimliği. |
| tenantId | string | Kaynak Kiracı kimliği. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Event grid'e giriş için bkz [Event Grid nedir?](overview.md)
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid aboneliği şema](subscription-creation-schema.md).
