---
title: "Azure olay kılavuz güvenlik ve kimlik doğrulama"
description: "Azure olay kılavuz ve onun kavramlarını açıklar."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 09/18/2017
ms.author: babanisa
ms.openlocfilehash: 510e578f3ebeb6ad7a4d81249cdfd7ce1d3684ad
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="event-grid-security-and-authentication"></a>Olay kılavuz güvenlik ve kimlik doğrulama 

Azure olay kılavuz üç tür kimlik doğrulama vardır:

* Olay abonelikleri
* Olay yayımlama
* Web kancası olay teslimi

## <a name="webhook-event-delivery"></a>Web kancası olay teslimi

Web kancası Azure olay kılavuz gelen gerçek zamanlı olaylarını almak için birçok yolu vardır. Olduğundan her zaman yeni bir olay teslim edilecek hazır, olay kılavuz Web kancası olay gövdesinde yapılandırılan HTTP uç noktası bir HTTP isteği gönderir.

Kendi Web kancası bitiş noktası içeren olay kılavuz kaydettiğinizde, uç nokta sahipliği kanıtlamak için basit bir doğrulama kodu içeren bir POST isteği gönderir. Uygulamanızın geri doğrulama kodu Yankı tarafından yanıt vermesi gerekir. Olay kılavuz olayları doğrulama geçmedi Web kancası Uç noktalara dağıtmaz.

### <a name="validation-details"></a>Doğrulama ayrıntıları

* Olay aboneliği oluşturma/güncelleştirme zaman olay kılavuz "SubscriptionValidationEvent" olay hedef uç noktasına gönderir.
* Olay bir üstbilgi değeri "Aeg olay türü: SubscriptionValidation" içerir.
* Olay gövdesi diğer olay kılavuz olaylarla aynı şeması vardır.
* Olay verileri rastgele oluşturulmuş bir dize olan bir "validationCode" özelliği içerir. Örneğin, "validationCode: acb13...".

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
  "eventTime": "2017-08-06T22:09:30.740323Z"
}]
```

Uç nokta sahipliği kanıtlamak için geri validationResponse özelliğinde doğrulama kodu aşağıdaki örnekte gösterildiği gibi echo:

```json
{
  "validationResponse": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
}
```

Son olarak, Azure olay kılavuz yalnızca HTTPS Web kancası uç noktaları desteklediğini dikkate almak önemlidir.

## <a name="event-subscription"></a>Olay aboneliği

Bir olaya abone olmak için bilmeniz gereken **Microsoft.EventGrid/EventSubscriptions/Write** gerekli kaynak izni. Kapsamındaki yeni bir abonelik kaynağının yazmak için bu izni gerekiyor. Gerekli kaynak sistem konusunu ya da özel konu abone göre farklılık gösterir. Her iki tür, bu bölümde açıklanmıştır.

### <a name="system-topics-azure-service-publishers"></a>Sistem konuları (Azure hizmeti yayımcılar)

Sistem konuları için yeni bir olay aboneliği kapsamında olay yayımlama kaynağının yazma izni gerekir. Kaynak biçimi şöyledir:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`

Örneğin, bir depolama hesabı üzerinde bir olaya abone olmak için adlı **myacct**, üzerinde Microsoft.EventGrid/EventSubscriptions/Write izniniz olmalıdır:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`

### <a name="custom-topics"></a>Özel konular

Özel konular için yeni bir olay aboneliği kapsamında olay kılavuz konunun yazma izni gerekir. Kaynak biçimi şöyledir:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`

Örneğin, özel bir konuya abone olmak için adlı **mytopic**, üzerinde Microsoft.EventGrid/EventSubscriptions/Write izniniz olmalıdır:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`

## <a name="topic-publishing"></a>Konu yayımlama

Konular, paylaşılan erişim imzası (SAS) veya anahtar kimlik doğrulaması kullanın. SAS öneririz, ancak anahtar kimlik doğrulaması basit programlama sağlar ve birçok varolan Web kancası yayımcıları ile uyumludur. 

HTTP üstbilgisinde kimlik doğrulama değeri içerir. SA'ları için kullanmak **aeg sas belirteci** üstbilgi değeri. Anahtar kimlik doğrulaması için kullanmak **aeg sas anahtarı** üstbilgi değeri.

### <a name="key-authentication"></a>Anahtar kimlik doğrulaması

Anahtar kimlik doğrulaması kimlik doğrulaması en basit biçimidir. Biçimi kullanın:`aeg-sas-key: <your key>`

Örneğin, bir anahtar ile geçirin:

```
aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a>SAS belirteçleri

Olay kılavuz için SAS belirteci kaynağı, sona erme süresi ve imza içerir. SAS belirteci biçimi: `r={resource}&e={expiration}&s={signature}`.

Kaynak olayların gönderilmesi konu yolu değil. Örneğin, geçerli bir kaynak yolu şöyledir:`https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`

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

Son üç işlemi normal okuma işlemleri dışında filtre potansiyel olarak gizli bilgileri döndürün. Bu, bu işlemler için erişimi kısıtlamak en iyi uygulamadır. Özel roller kullanılarak oluşturulabilir [Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md), [Azure komut satırı arabirimi (CLI)](../active-directory/role-based-access-control-manage-access-azure-cli.md)ve [REST API](../active-directory/role-based-access-control-manage-access-rest.md).

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

**EventGridNoDeleteListKeysRole.json**: Allow restricted post actions but disallow delete actions.

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

**EventGridContributorRole.json**: Allows all event grid actions.

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
