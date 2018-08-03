---
title: Azure Event Grid güvenliğini ve kimlik doğrulaması
description: Azure Event Grid ve kavramlarını açıklar.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/27/2018
ms.author: babanisa
ms.openlocfilehash: d2bc0d8f78e6fe0806afb3208c88df28b8cce1f9
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39460243"
---
# <a name="event-grid-security-and-authentication"></a>Event Grid güvenliğini ve kimlik doğrulaması 

Azure Event Grid, kimlik doğrulaması üç tür vardır:

* Olay abonelikleri
* Etkinlik yayımlama
* Web kancası olay teslimi

## <a name="webhook-event-delivery"></a>Web kancası olay teslimi

Web kancaları olayları Azure Event Grid'den almak için birçok yöntemlerinden biridir. Yeni bir olay hazır olduğunda EventGrid hizmet istek gövdesinde olay ile yapılandırılmış uç noktasına bir HTTP isteği gönderir.

Web kancalarını destekleyen birçok diğer hizmetleri gibi bu uç noktasına olayları göndermeye başlamadan önce Web kancası uç noktanızı "sahipliğini" kanıtlamak EventGrid gerektirir. Bu gereksinim duymayan bir uç nokta EventGrid olay teslimi için hedef uç noktası olmaktan engellemektir. Ancak, aşağıda listelenen üç Azure hizmetlerinden herhangi birinin kullandığınızda, Azure altyapısının bu doğrulama otomatik olarak işler:

* Azure Logic Apps
* Azure Otomasyonu
* Azure işlevleri EventGrid tetikleyicisi için.

HTTP tetikleyicisi tabanlı Azure işlevi gibi başka türde bir uç noktasını kullanıyorsanız, uç nokta kodunuzun EventGrid doğrulama sıkışmaya katılmak gerekir. İki farklı doğrulama el sıkışması modeli EventGrid destekler:

1. Temel ValidationCode el sıkışması: olay aboneliği oluşturma sırasında bir "abonelik doğrulama olayı" uç noktanıza EventGrid gönderir. Bu olayın şeması için başka bir EventGridEvent benzer ve bu olay veri bölümünü "validationCode" özelliği içerir. Uygulamanızı doğrulama isteği için beklenen olay aboneliği olduğunu doğruladıktan sonra uygulama kodunuz geri Yankı EventGrid doğrulama kodu ile yanıt vermesi gerekir. Bu anlaşma mekanizması tüm EventGrid sürümlerinde desteklenir.

2. Temel ValidationURL el sıkışması (el ile anlaşması): Bazı durumlarda, uç nokta tabanlı ValidationCode el sıkışması uygulamak için kaynak kodu denetimi olmayabilir. Örneğin, bir üçüncü taraf hizmet kullanın (gibi [Zapier](https://zapier.com) veya [IFTTT](https://ifttt.com/)), program aracılığıyla doğrulama koduyla yanıt vermesi mümkün olmayabilir. Bu nedenle, sürümü 2018-05-01-preview ile başlayarak, EventGrid artık el ile doğrulama el sıkışması destekler. Bu yeni API sürümünü kullanın SDK/araçlarını kullanarak bir olay aboneliği oluşturuyorsanız (2018-05-01-Önizleme) EventGrid abonelik doğrulama verileri kısmı bir parçası olarak bir "validationUrl" özelliği (ek olarak "validationCode" özelliği) gönderir olay. Anlaşma tamamlamak için yalnızca bir GET REST istemcisi ya da web tarayıcınızı kullanarak aracılığıyla bu URL'de ister. Sağlanan validationUrl bu süre içinde el ile doğrulama tamamlamazsanız, olay aboneliğinin provisioningState "Başarısız" olarak geçer ve olay oluşturmayı deneyin gerekecek şekilde yalnızca yaklaşık 10 dakika için geçerlidir yeniden el ile doğrulama yapmak denemeden önce abonelik.

Bu mekanizma el ile doğrulama Önizleme aşamasındadır. Bunu kullanmak istiyorsanız [AZ CLI 2.0](/cli/azure/install-azure-cli) için [Event Grid uzantısını](/cli/azure/azure-cli-extensions-list) yüklemeniz gerekir. `az extension add --name eventgrid` ile yükleyebilirsiniz. REST API’yi kullanıyorsanız `api-version=2018-05-01-preview` kullandığınızdan emin olun.

### <a name="validation-details"></a>Doğrulama ayrıntıları

* Olay aboneliği oluşturma/güncelleştirme zaman Event Grid aboneliği doğrulama olayı hedef uç noktasına gönderir. 
* Olay üst bilgi değeri "Aeg olay türü: SubscriptionValidation" içerir.
* Olay gövdesinde diğer Event Grid olaylarına aynı şemaya sahip.
* Olay türü olay "Microsoft.EventGrid.SubscriptionValidationEvent" özelliğidir.
* Olayın veri özelliği bir "validationCode" özelliği ile rastgele oluşturulmuş bir dize içerir. Örneğin, "validationCode: acb13...".
* API sürümü 2018-05-01-preview kullanıyorsanız, olay verileri el ile abonelik doğrulamak için bir URL ile bir "validationUrl" özelliğini de içerir.
* Dizi doğrulama olay içeriyor. Doğrulama kodu geri echo sonra gelen diğer olayları ayrı bir istek gönderilir.
* EventGrid veri düzlemi SDK'ları abonelik doğrulama olay verileri ve abonelik doğrulama yanıt karşılık gelen sınıfları içerir.

Bir örnek SubscriptionValidationEvent aşağıdaki örnekte gösterilmiştir:

```json
[{
  "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "subject": "",
  "data": {
    "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6",
    "validationUrl": "https://rp-eastus2.eventgrid.azure.net:553/eventsubscriptions/estest/validate?id=B2E34264-7D71-453A-B5FB-B62D0FDC85EE&t=2018-04-26T20:30:54.4538837Z&apiVersion=2018-05-01-preview&token=1BNqCxBBSSE9OnNSfZM4%2b5H9zDegKMY6uJ%2fO2DFRkwQ%3d"
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

Alternatif olarak, doğrulama URL'si için bir GET isteği göndererek abonelik el ile doğrulayabilirsiniz. Olay aboneliği doğrulandı kadar bir bekleme durumunda kalır.

C# abonelik doğrulama el sıkışması sırasında nasıl ele alınacağını gösteren örnek bulabilirsiniz https://github.com/Azure-Samples/event-grid-dotnet-publish-consume-events/blob/master/EventGridConsumer/EventGridConsumer/Function1.cs.

### <a name="checklist"></a>Denetim listesi

Gibi bir hata iletisini görüyorsanız, olay aboneliği oluşturma sırasında "sağlanan uç nokta doğrulama girişimi https://your-endpoint-here başarısız oldu. Daha fazla bilgi için ziyaret https://aka.ms/esvalidation", içinde doğrulama el sıkışması başarısız olduğunu gösterir. Bu hatayı gidermek için aşağıdaki durumlara doğrulayın:

* Hedef uç noktasında denetim uygulama kodu var mı? Örneğin, Azure işlevi bir HTTP tetikleyici tabanlı yazıyorsanız, değişiklik yapmak için uygulama koduna erişmeniz gerekiyor?
* Lütfen uygulama koduna erişiminiz varsa temel ValidationCode el sıkışması mekanizması yukarıdaki örnekte gösterildiği gibi uygulayın.

* Uygulama koduna erişiminiz yoksa (örneğin Web kancalarını destekleyen bir üçüncü taraf hizmetini kullanıyorsanız), el ile anlaşma mekanizması kullanabilirsiniz. Bunu yapmak için (örneğin yukarıda açıklanan EventGrid CLI uzantısını kullanarak) 2018-05-01-Önizleme API sürümü kullandığınızdan emin olun doğrulama olayında validationUrl almak için. El ile doğrulama anlaşma tamamlamak için "validationUrl" özelliğinin değeri alın ve web tarayıcınızda bu URL'yi ziyaret edin. Doğrulama başarılı olursa, doğrulamanın başarılı olması ve olay aboneliğinin provisioningState "başarılı olduğunu" ifadesini görürsünüz, web tarayıcınızda bir ileti görürsünüz. 

### <a name="event-delivery-security"></a>Olay teslimi güvenliği

Bir olay aboneliği oluştururken, Web kancası URL'si sorgu parametreleri ekleyerek, Web kancası uç noktası güvenli hale getirebilirsiniz. Ayarlanmış bir gizli dizi gibi olması için bu sorgu parametreleri bir [erişim belirteci](https://en.wikipedia.org/wiki/Access_token) Web kancası olay tanımak için kullanabileceğiniz Event Grid'den gelen geçerli izinleriyle geliyor. Olay Kılavuzu her Web kancası olay teslimi bu sorgu parametreleri içerir.

Olay aboneliği düzenlerken, sorgu parametreleri olmayan görüntülenen veya kaldırılacak sürece döndürülen [--dahil tam-endpoint-url](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-show) parametresi Azure'da kullanılan [CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest).

Son olarak, Azure Event Grid yalnızca HTTPS Web kancası uç noktaları desteklediğini unutmayın.

## <a name="event-subscription"></a>Olay aboneliği

Bir olaya abone için olmalıdır **Microsoft.EventGrid/EventSubscriptions/Write** gerekli kaynak izni. Kaynağın yeni bir abonelik kapsamda yazmakta olduğunuz çünkü bu iznine sahip olmanız gerekir. Gerekli kaynak sistem konusu ya da özel konuya abone göre farklılık gösterir. Bu bölümde iki türü de açıklanmaktadır.

### <a name="system-topics-azure-service-publishers"></a>Sistem konuları (Azure hizmet yayımcılar)

Sistem konuları için yeni bir olay aboneliği kapsamında olayın yayımlanması kaynak yazma izni gerekir. Kaynak biçimi şu şekildedir: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`

Örneğin, bir depolama hesabı üzerinde bir olaya abone olmak için adlı **myacct**, üzerinde Microsoft.EventGrid/EventSubscriptions/Write izninizin olması gerekiyor: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`

### <a name="custom-topics"></a>Özel konular

Özel konu için yeni olay aboneliğinizi kapsamında olay Kılavuzu konusu yazma izni gerekir. Kaynak biçimi şu şekildedir: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`

Örneğin, bir özel konuya abone olma adlı **mytopic**, üzerinde Microsoft.EventGrid/EventSubscriptions/Write izninizin olması gerekiyor: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`

## <a name="topic-publishing"></a>Konu yayımlama

Paylaşılan erişim imzası (SAS) veya anahtar kimlik doğrulaması konuları kullanın. SAS öneririz, ancak anahtar kimlik doğrulaması basit bir programlama sağlar ve birçok mevcut Web kancası yayımcıları ile uyumludur. 

HTTP üst bilgisi kimlik doğrulaması değeri içerir. SAS için kullanmak **aeg sas belirteci** üstbilgi değeri. Anahtar kimlik doğrulaması için **aeg sas anahtarı** üstbilgi değeri.

### <a name="key-authentication"></a>Anahtar kimlik doğrulaması

Anahtar kimlik doğrulaması, kimlik doğrulamasının en basit biçimidir. Biçimini kullanın: `aeg-sas-key: <your key>`

Örneğin, bir anahtar ile geçirin:

```
aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a>SAS belirteçleri

Event Grid için SAS belirteci, kaynak, sona erme süresini ve imza içerir. SAS belirteci biçimi: `r={resource}&e={expiration}&s={signature}`.

Kaynak için olayları gönderiyorsunuz olay Kılavuzu konusu yoludur. Örneğin, geçerli bir kaynak yolu şöyledir: `https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`

Bir anahtar, imza üret

Örneğin, geçerli **aeg sas belirteci** değerdir:

```http
aeg-sas-token: r=https%3a%2f%2fmytopic.eventgrid.azure.net%2feventGrid%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=a4oNHpRZygINC%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
```

Aşağıdaki örnek, Event Grid ile kullanmak için bir SAS belirteci oluşturur:

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

Azure Event Grid olay abonelikleri listesi gibi çeşitli yönetim işlemlerini yapmak için yenilerini oluşturun ve anahtarlar oluşturmak için farklı kullanıcılara verilen erişim düzeyini denetlemenize olanak tanır. Event Grid, Azure'nın rol tabanlı erişim denetleyin (RBAC) kullanır.

### <a name="operation-types"></a>İşlem türleri

Azure event grid, aşağıdaki eylemleri destekler:

* Microsoft.EventGrid/*/read
* Microsoft.EventGrid/*/write
* Microsoft.EventGrid/*/delete
* Microsoft.EventGrid/eventSubscriptions/getFullUrl/action
* Microsoft.EventGrid/topics/listKeys/action
* Microsoft.EventGrid/topics/regenerateKey/action

Son üç işlemi dışında normal okuma işlemleri filtrelenmiş büyük olasılıkla gizli bilgi döndürür. Bu, bu işlemler için erişimi kısıtlamak en iyi uygulamadır. Özel rolleri kullanarak oluşturulabilir [Azure PowerShell](../role-based-access-control/role-assignments-powershell.md), [Azure komut satırı arabirimi (CLI)](../role-based-access-control/role-assignments-cli.md)ve [REST API](../role-based-access-control/role-assignments-rest.md).

### <a name="enforcing-role-based-access-check-rbac"></a>Zorlama rol tabanlı erişim denetimi (RBAC)

Farklı kullanıcılar için RBAC zorlamak için aşağıdaki adımları kullanın:

#### <a name="create-a-custom-role-definition-file-json"></a>Bir özel rol tanımı dosyası (.json) oluşturun

Farklı eylemleri kümesini gerçekleştirmek kullanıcıların örnek Event Grid rol tanımları aşağıda verilmiştir.

**EventGridReadOnlyRole.json**: yalnızca salt okunur işlemlere izin verin.

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

**EventGridNoDeleteListKeysRole.json**: izin kısıtlı sonrası eylemler ancak Sil eylemlerinin izin vermeyin.

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

**EventGridContributorRole.json**: tüm event grid Eylemler sağlar.

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

Bir kullanıcıya rol atamak için bu seçeneği kullanın:

```azurecli
az role assignment create --assignee <user name> --role "<name of role>"
```

## <a name="next-steps"></a>Sonraki adımlar

* Event grid'e giriş için bkz [Event Grid hakkında](overview.md)
