---
title: Azure Event Hubs kimlik doğrulaması ve güvenlik modeline genel bakış | Microsoft Docs
description: Olay hub'ları kimlik doğrulaması ve güvenlik modeline genel bakış.
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: 93841e30-0c5c-4719-9dc1-57a4814342e7
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/30/2018
ms.author: sethm
ms.openlocfilehash: 5264930dcb802c2a58abc179bdd0041acc9f58d0
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2018
ms.locfileid: "32311378"
---
# <a name="event-hubs-authentication-and-security-model-overview"></a>Olay hub'ları kimlik doğrulaması ve güvenlik modeline genel bakış

Azure Event Hubs güvenlik modeli, aşağıdaki gereksinimleri karşıladığından:

* Geçerli kimlik bilgileri sunmak yalnızca istemcileri verileri olay hub'ına gönderebilirsiniz.
* Bir istemci, başka bir istemci taklit edilemez.
* Standart dışı bir istemci, bir olay hub'ına veri gönderme engellenebilir.

## <a name="client-authentication"></a>İstemci kimlik doğrulaması

Olay hub'ları güvenlik modeli bir bileşimine dayalı [paylaşılan erişim imzası (SAS)](../service-bus-messaging/service-bus-sas.md) belirteçleri ve *olay yayımcıları*. Sanal bir uç noktası bir event hub için bir olay yayımcısı tanımlar. Yayımcı, yalnızca bir olay hub'ına iletileri göndermek için kullanılabilir. Bir yayımcıdan iletileri almak mümkün değildir.

Genellikle, istemci başına bir yayımcı bir olay hub'ı kullanır. Herhangi bir olay hub'ı yayımcılar gönderilen tüm iletileri sıraya alınan bu olay hub'ın içinde edilir. Yayımcılar ayrıntılı erişim denetimi ve azaltma etkinleştirin.

Her olay hub'ları istemci istemciye karşıya bir benzersiz belirteci atanır. Her benzersiz belirteç erişimi için farklı bir benzersiz yayımcı verir şekilde belirteçleri oluşturulur. Bir belirteç sahip bir istemci, yalnızca bir yayımcı, ancak herhangi bir yayımcı gönderebilirsiniz. Birden çok istemci aynı belirteci paylaşıyorsanız, bunların her biri bir yayımcı paylaşır.

Önerilmemesine rağmen bir olay hub'ına doğrudan erişim belirteçleri ile cihazları Donatı mümkündür. Bu belirteç tutan herhangi bir aygıt, doğrudan bu olay hub'ına iletileri gönderebilir. Böyle bir cihazı azaltma tabi olmayacaktır. Ayrıca, cihaz, olay hub'ına göndermesinin kara listede olamaz.

Tüm belirteçlerin bir SAS anahtarla imzalanmıştır. Genellikle, tüm belirteçleri aynı anahtarla imzalanmıştır. İstemciler anahtarın farkında değildir; Bu, diğer istemcilerin belirteçleri üretim önler.

### <a name="create-the-sas-key"></a>SAS anahtarı oluşturma

Bir olay hub'ları ad alanı oluştururken, hizmet adında bir 256 bit SAS anahtarı otomatik olarak oluşturur. **RootManageSharedAccessKey**. Bu kural bir ilişkili çift gönderme izni birincil ve ikincil anahtar içeriyor, dinleme ve ad alanı hakkı yönetin. Ek anahtarlar da oluşturabilirsiniz. Belirli bir olay hub'ına verir izinleri göndermek bir anahtar oluşturmak önerilir. Bu konunun geri kalanı için bu anahtar adlı görünür duruma varsayılır **EventHubSendKey**.

Aşağıdaki örnek, olay hub'ı oluştururken yalnızca gönderme bir anahtar oluşturur:

```csharp
// Create namespace manager.
string serviceNamespace = "YOUR_NAMESPACE";
string namespaceManageKeyName = "RootManageSharedAccessKey";
string namespaceManageKey = "YOUR_ROOT_MANAGE_SHARED_ACCESS_KEY";
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, string.Empty);
TokenProvider td = TokenProvider.CreateSharedAccessSignatureTokenProvider(namespaceManageKeyName, namespaceManageKey);
NamespaceManager nm = new NamespaceManager(namespaceUri, namespaceManageTokenProvider);

// Create event hub with a SAS rule that enables sending to that event hub
EventHubDescription ed = new EventHubDescription("MY_EVENT_HUB") { PartitionCount = 32 };
string eventHubSendKeyName = "EventHubSendKey";
string eventHubSendKey = SharedAccessAuthorizationRule.GenerateRandomKey();
SharedAccessAuthorizationRule eventHubSendRule = new SharedAccessAuthorizationRule(eventHubSendKeyName, eventHubSendKey, new[] { AccessRights.Send });
ed.Authorization.Add(eventHubSendRule); 
nm.CreateEventHub(ed);
```

### <a name="generate-tokens"></a>Belirteçleri oluşturmak

SAS anahtarını kullanarak belirteçleri üretebilir. İstemci başına yalnızca bir belirteç üretmek gerekir. Belirteçleri, ardından aşağıdaki yöntemi kullanarak üretilebilir. Tüm belirteçleri kullanarak oluşturulan **EventHubSendKey** anahtarı. Her belirteç benzersiz bir URI atanır.

```csharp
public static string SharedAccessSignatureTokenProvider.GetSharedAccessSignature(string keyName, string sharedAccessKey, string resource, TimeSpan tokenTimeToLive)
```

Bu yöntem çağrılırken URI olarak belirtilmelidir: `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`. Tüm belirteçleri için URI dışında aynı `PUBLISHER_NAME`, olacağı için her belirteci farklı. İdeal olarak, `PUBLISHER_NAME` belirtecini alır istemci Kimliğini temsil eder.

Bu yöntem, şu yapıda bir belirteç oluşturur:

```csharp
SharedAccessSignature sr={URI}&sig={HMAC_SHA256_SIGNATURE}&se={EXPIRATION_TIME}&skn={KEY_NAME}
```

Belirteç süre sonu zamanı, 1 Ocak 1970'ten gelen saniye cinsinden belirtilir. Belirtecin bir örnek verilmiştir:

```csharp
SharedAccessSignature sr=contoso&sig=nPzdNN%2Gli0ifrfJwaK4mkK0RqAB%2byJUlt%2bGFmBHG77A%3d&se=1403130337&skn=RootManageSharedAccessKey
```

Genellikle, belirteçleri benzer veya istemci Sysprep'in aşıyor bir kullanım ömrü vardır. İstemci yeni bir belirteç elde yeteneği varsa, daha kısa bir ömre sahip belirteçler kullanılabilir.

### <a name="sending-data"></a>Verileri gönderme

Belirteçleri oluşturduktan sonra her bir istemci kendi benzersiz belirteciyle sağlanır.

İstemci bir event hub'ına veri gönderdiğinde, kendi gönderme isteği belirteci ile etiketler. Bir saldırgan, gizli dinleme ve belirteç çalınmasını engellemek için olay hub'ı ile istemci arasındaki iletişimi şifrelenmiş bir kanal üzerinden gerçekleşmelidir.

### <a name="blacklisting-clients"></a>İstemcileri kara liste

Bir saldırgan tarafından bir belirteç çalınırsa, saldırgan, belirteç çalınırsa istemcinin kimliğine bürünebilir. Farklı bir yayımcı kullanan yeni bir belirteç alıncaya kadar bir istemci kara liste istemci kullanılamaz işler.

## <a name="authentication-of-back-end-applications"></a>Arka uç uygulamalarının kimlik doğrulaması

Olay hub'ları istemciler tarafından oluşturulan verileri kullanan arka uç uygulama kimliğini doğrulamak için Event Hubs için Service Bus konuları kullanılan model benzer bir güvenlik modeli kullanır. Bir olay hub'ları tüketici grubu, Service Bus konu başlığına bir abonelik eşdeğerdir. Tüketici grubu oluşturma isteği tarafından bir belirteç verir ayrıcalıkları olay hub'ı ya da olay hub'ı ait olduğu ad alanını yönetmek bulunuyorsa, istemci bir tüketici grubu oluşturabilirsiniz. Bir istemci bu tüketici grubu, olay hub'ı veya olay hub'ı ait olduğu ad alanını alma hakları veren bir belirteç tarafından alma isteği bulunuyorsa, verileri bir tüketici grubundan kullanmak izin verilmez.

Hizmet veri yolu geçerli sürümü tek tek abonelikler için SAS kuralları desteklemez. Aynı olay hub'ları tüketici grupları için geçerlidir. SAS desteği için her iki özellik gelecekte eklenir.

Tek tek tüketici grupları için SAS kimlik doğrulaması olmaması durumunda tüm tüketici grupları bir ortak anahtar ile güvenli hale getirmek için SAS tuşlarını kullanabilirsiniz. Bu yaklaşım, herhangi bir event hub tüketici grupları verileri kullanmak üzere bir uygulama sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Event Hubs hakkında daha fazla bilgi için aşağıdaki konuları ziyaret edin:

* [Event Hubs’a genel bakış]
* [Paylaşılan erişim imzaları genel bakış]
* [Event Hubs kullanan örnek uygulamalar]

[Event Hubs’a genel bakış]: event-hubs-what-is-event-hubs.md
[Event Hubs kullanan örnek uygulamalar]: https://github.com/Azure/azure-event-hubs/tree/master/samples
[Paylaşılan erişim imzaları genel bakış]: ../service-bus-messaging/service-bus-sas.md

