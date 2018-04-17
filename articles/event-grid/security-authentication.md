---
title: Azure olay kılavuz güvenlik ve kimlik doğrulama
description: Azure olay kılavuz ve onun kavramlarını açıklar.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 03/15/2018
ms.author: babanisa
ms.openlocfilehash: f97de4e93c9330206ed22c071d8ade0821bf6691
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="event-grid-security-and-authentication"></a>Olay kılavuz güvenlik ve kimlik doğrulama 

Azure olay kılavuz üç tür kimlik doğrulama vardır:

* Olay abonelikleri
* Olay yayımlama
* Web kancası olay teslimi

## <a name="webhook-event-delivery"></a>Web kancası olay teslimi

Web kancası Azure olay kılavuzdan olaylarını almak için birçok yolu vardır. Yeni bir olay hazır olduğunda, olay kılavuz Web kancası olay gövdesinde yapılandırılan HTTP uç noktası bir HTTP isteği gönderir.

Kendi Web kancası bitiş noktası içeren olay kılavuz kaydettiğinizde, uç nokta sahipliği kanıtlamak için basit bir doğrulama kodu içeren bir POST isteği gönderir. Uygulamanızın geri doğrulama kodu Yankı tarafından yanıt vermesi gerekir. Olay kılavuz doğrulamayı geçen henüz Web kancası Uç noktalara olayları teslim değil.

### <a name="validation-details"></a>Doğrulama ayrıntıları

* Olay aboneliği oluşturma/güncelleştirme zaman olay kılavuz "SubscriptionValidationEvent" olay hedef uç noktasına gönderir.
* Olay bir üstbilgi değeri "Aeg olay türü: SubscriptionValidation" içerir.
* Olay gövdesi diğer olay kılavuz olaylarla aynı şeması vardır.
* Olay verileri rastgele oluşturulmuş bir dize olan bir "validationCode" özelliği içerir. Örneğin, "validationCode: acb13...".
* Dizi yalnızca doğrulama olayı içerir. Doğrulama kodu geri echo sonra diğer olayları ayrı bir istekte gönderilir.

Örnek SubscriptionValidationEvent aşağıdaki örnekte gösterilmiştir:

```json
[{
  "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "subject": "",
  "data": {
    "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
  },
  "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
  "eventTime": "2018-01-25T22:12:19.4556811Z",
  "metadataVersion": "1",
  "dataVersion": "1"
}]
```

Uç nokta sahipliği kanıtlamak için geri validationResponse özelliğinde doğrulama kodu aşağıdaki örnekte gösterildiği gibi echo:

```json
{
  "validationResponse": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
}
```
### <a name="event-delivery-security"></a>Olay teslimi güvenliği

Bir olay abonelik oluştururken, Web kancası URL'si sorgu parametreleri ekleyerek Web kancası uç noktanızı güvenliğini sağlayabilirsiniz. Gizli gibi olması için bu sorgu parametrelerini ayarlayın bir [erişim belirteci](https://en.wikipedia.org/wiki/Access_token) Web kancası olay tanımak için kullanabileceğiniz olay kılavuzdan geçerli izinleriyle geliyor. Olay kılavuz her olay teslimi Web kancası için bu sorgu parametrelerini içerir.

Olay aboneliği düzenlerken, sorgu parametrelerini değil görüntülenen veya kaldırılacak sürece döndürülen [--dahil-tam-endpoint-url](https://docs.microsoft.com/en-us/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_show) parametresi Azure'da kullanılan [CLI](https://docs.microsoft.com/en-us/cli/azure?view=azure-cli-latest).

Son olarak, Azure olay kılavuz yalnızca HTTPS Web kancası uç noktaları desteklediğini dikkate almak önemlidir.

## <a name="event-subscription"></a>Olay aboneliği

Bir olaya abone olmak için bilmeniz gereken **Microsoft.EventGrid/EventSubscriptions/Write** gerekli kaynak izni. Yeni bir kapsam abonelik kaynağının yazıyorsanız çünkü bu izni gerekir. Gerekli kaynak sistem konusunu ya da özel konu abone göre farklılık gösterir. Her iki tür, bu bölümde açıklanmıştır.

### <a name="system-topics-azure-service-publishers"></a>Sistem konuları (Azure hizmeti yayımcılar)

Sistem konuları için yeni bir olay aboneliği kapsamında olay yayımlama kaynağının yazma izni gerekir. Kaynak biçimi şöyledir: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`

Örneğin, bir depolama hesabı üzerinde bir olaya abone olmak için adlı **myacct**, üzerinde Microsoft.EventGrid/EventSubscriptions/Write izniniz olmalıdır: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`

### <a name="custom-topics"></a>Özel konular

Özel konular için yeni bir olay aboneliği kapsamında olay kılavuz konunun yazma izni gerekir. Kaynak biçimi şöyledir: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`

Örneğin, özel bir konuya abone olmak için adlı **mytopic**, üzerinde Microsoft.EventGrid/EventSubscriptions/Write izniniz olmalıdır: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`

## <a name="topic-publishing"></a>Konu yayımlama

Konular, paylaşılan erişim imzası (SAS) veya anahtar kimlik doğrulaması kullanın. SAS öneririz, ancak anahtar kimlik doğrulaması basit programlama sağlar ve birçok varolan Web kancası yayımcıları ile uyumludur. 

HTTP üstbilgisinde kimlik doğrulama değeri içerir. SA'ları için kullanmak **aeg sas belirteci** üstbilgi değeri. Anahtar kimlik doğrulaması için kullanmak **aeg sas anahtarı** üstbilgi değeri.

### <a name="key-authentication"></a>Anahtar kimlik doğrulaması

Anahtar kimlik doğrulaması kimlik doğrulaması en basit biçimidir. Biçimi kullanın: `aeg-sas-key: <your key>`

Örneğin, bir anahtar ile geçirin:

```
aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a>SAS belirteçleri

Olay kılavuz için SAS belirteci kaynağı, sona erme süresi ve imza içerir. SAS belirteci biçimi: `r={resource}&e={expiration}&s={signature}`.

Kaynak olayları gönderiyorsanız olay kılavuz konusu yoludur. Örneğin, geçerli bir kaynak yolu şöyledir: `https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`

İmza bir anahtarı oluşturur.

Örneğin, geçerli **aeg sas belirteci** değer:

```http
aeg-sas-token: r=https%3a%2f%2fmytopic.eventgrid.azure.net%2feventGrid%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=a4oNHpRZygINC%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
```

Aşağıdaki örnek, olay kılavuz ile kullanmak için bir SAS belirteci oluşturur:

```cs
static string BuildSharedAccessSignature(string resource, DateTime expirationUtc, string key)
{
    const char Resource = 'r';
    const char Expiration = 'e';
    const char Signature = 's';

    string encodedResource = HttpUtility.UrlEncode(resource);
    var culture = CultureInfo.CreateSpecificCulture("en-US");
    var encodedExpirationUtc = HttpUtility.UrlEncode(expirationUtc.ToString(culture));

    string unsignedSas = $"{Resource}={encodedResource}&{Expiration}={encodedExpirationUtc}";
    using (var hmac = new HMACSHA256(Convert.FromBase64String(key)))
    {
        string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(unsignedSas)));
        string encodedSignature = HttpUtility.UrlEncode(signature);
        string signedSas = $"{unsignedSas}&{Signature}={encodedSignature}";

        return signedSas;
    }
}
```

## <a name="management-access-control"></a>Yönetim erişim denetimi

Azure olay kılavuz listesi olay abonelikleri gibi çeşitli yönetim işlemlerini yapmak için yeni kampanya oluşturmak ve anahtarlar oluşturmak için farklı kullanıcılara verilen erişim düzeyini denetlemenize olanak verir. Olay kılavuz Azure'nın rol tabanlı erişim denetleyin (RBAC) kullanır.

### <a name="operation-types"></a>İşlem türleri

Azure olay kılavuz, aşağıdaki eylemleri destekler:

* Microsoft.EventGrid/*/read
* Microsoft.EventGrid/*/write
* Microsoft.EventGrid/*/delete
* Microsoft.EventGrid/eventSubscriptions/getFullUrl/action
* Microsoft.EventGrid/topics/listKeys/action
* Microsoft.EventGrid/topics/regenerateKey/action

Son üç işlemi normal okuma işlemleri dışında filtre potansiyel olarak gizli bilgileri döndürün. Bu, bu işlemler için erişimi kısıtlamak en iyi uygulamadır. Özel roller kullanılarak oluşturulabilir [Azure PowerShell](../role-based-access-control/role-assignments-powershell.md), [Azure komut satırı arabirimi (CLI)](../role-based-access-control/role-assignments-cli.md)ve [REST API](../role-based-access-control/role-assignments-rest.md).

### <a name="enforcing-role-based-access-check-rbac"></a>Rol uygulamaya dayalı erişim denetimi (RBAC)

Farklı kullanıcılar için RBAC zorlamak için aşağıdaki adımları kullanın:

#### <a name="create-a-custom-role-definition-file-json"></a>Bir özel rol tanımı dosyası (.json) oluşturun

Kullanıcıların farklı eylemler gerçekleştirmesine olanak sağlayan örnek olay kılavuz rol tanımları verilmiştir.

**EventGridReadOnlyRole.json**: yalnızca salt okunur işlemlere izin verir.

```json
{
  "Name": "Event grid read only role",
  "Id": "7C0B6B59-A278-4B62-BA19-411B70753856",
  "IsCustom": true,
  "Description": "Event grid read only role",
  "Actions": [
    "Microsoft.EventGrid/*/read"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription Id>"
  ]
}
```

**EventGridNoDeleteListKeysRole.json**: kısıtlı sonrası eylemler ancak silme işlemlerinin izin vermeyecek izin.

```json
{
  "Name": "Event grid No Delete Listkeys role",
  "Id": "B9170838-5F9D-4103-A1DE-60496F7C9174",
  "IsCustom": true,
  "Description": "Event grid No Delete Listkeys role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action"
  ],
  "NotActions": [
    "Microsoft.EventGrid/*/delete"
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

**EventGridContributorRole.json**: tüm olay kılavuz eylemleri sağlar.

```json
{
  "Name": "Event grid contributor role",
  "Id": "4BA6FB33-2955-491B-A74F-53C9126C9514",
  "IsCustom": true,
  "Description": "Event grid contributor role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/*/delete",
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
  ],
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

#### <a name="create-and-assign-custom-role-with-azure-cli"></a>Oluşturma ve Azure CLI ile özel rol atama

Özel bir rol oluşturmak için kullanın:

```azurecli
az role definition create --role-definition @<file path>
```

Bir kullanıcıya rol atamak için kullanın:

```azurecli
az role assignment create --assignee <user name> --role "<name of role>"
```

## <a name="next-steps"></a>Sonraki adımlar

* Olay kılavuz giriş için bkz: [olay kılavuz hakkında](overview.md)
