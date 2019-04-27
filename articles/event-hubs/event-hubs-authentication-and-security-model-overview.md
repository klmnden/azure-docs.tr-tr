---
title: Kimlik doğrulama ve güvenlik modeli - Azure Event Hubs | Microsoft Docs
description: Bu makalede, Azure Event Hubs'ın kimlik doğrulama ve güvenlik modeli açıklanır.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: 93841e30-0c5c-4719-9dc1-57a4814342e7
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 19b347423c28b4c615f90f325ead462b9d3e8e9e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60822594"
---
# <a name="azure-event-hubs---authentication-and-security-model"></a>Azure Event Hubs - kimlik doğrulaması ve güvenlik modeli

Azure Event Hubs güvenlik modeli, aşağıdaki gereksinimleri karşılar:

* Geçerli kimlik bilgilerini sunmak yalnızca istemciler, bir olay hub'ına veri gönderebilir.
* İstemci, başka bir istemci kimliğine bürünme olamaz.
* Standart dışı bir istemci, verileri olay hub'ına göndermesini engellenebilir.

## <a name="client-authentication"></a>İstemci kimlik doğrulaması

Event Hubs güvenlik modeli, bir bileşimine dayalı [paylaşılan erişim imzası (SAS)](../service-bus-messaging/service-bus-sas.md) belirteçleri ve *olay yayımcıları*. Bir olay yayımcısı, bir olay hub'ı için sanal bir uç nokta tanımlar. Yayımcı, yalnızca bir olay hub'ına ileti göndermek için kullanılabilir. Bir yayımcıdan iletileri almak mümkün değildir.

Genellikle, bir yayımcı istemci başına bir olay hub'ı kullanır. Herhangi bir olay hub'ının yayımcıları gönderilen tüm iletilerin sıraya alınan olay hub'ındaki ' dir. Ayrıntılı erişim denetimi ve kısıtlamanın yayımcılar etkinleştirin.

Her bir olay hub'ları istemci, istemci için karşıya bir benzersiz belirteci atanır. Her benzersiz belirteci farklı bir benzersiz yayımcı erişim verir, belirteçleri üretilir. Bir belirteç sahip bir istemci, yalnızca bir yayımcı, ancak başka bir yayımcı için gönderebilirsiniz. Birden çok istemci aynı belirteci paylaşıyorsanız, bunların her biri bir yayımcı paylaşır.

Önerilmemesine rağmen olay hub'ına doğrudan erişim belirteçleri ile cihazları Donatı mümkündür. Bu belirteç içeren herhangi bir CİHAZDAN ileti doğrudan bu olay hub'ına gönderebilirsiniz. Böyle bir cihaz, ağ kapasitesi azaltmaya tabidir olmayacaktır. Ayrıca, cihaz, olay hub'ına göndermesini kara listede olamaz.

Tüm belirteçlerin SAS anahtarı ile imzalanmıştır. Genellikle, tüm belirteçler aynı anahtarla imzalanmıştır. İstemciler anahtarın farkında değildir; Bu, istemcilerin diğer istemcilerle belirteçleri üretim öğesinden engeller.

### <a name="create-the-sas-key"></a>SAS anahtarı oluşturma

Bir Event Hubs ad alanı oluştururken, hizmet adında, 256 bit bir SAS anahtarı otomatik olarak oluşturur. **RootManageSharedAccessKey**. Bu kural gönderme izni birincil ve ikincil anahtarları ilişkili bir çift olan, dinleme ve ad alanına hakları. Ayrıca, ek anahtarlar da oluşturabilirsiniz. Bir anahtar verir izinlerini belirli bir olay hub'ına gönderme oluşturmak önerilir. Bu konunun geri kalanı için bu anahtarı adlı görünür duruma varsayılır **EventHubSendKey**.

Aşağıdaki örnek, olay hub'ı oluşturulurken yalnızca gönderme anahtarı oluşturur:

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

### <a name="generate-tokens"></a>Belirteç oluştur

SAS anahtarını kullanarak belirteç oluşturabilir. İstemci başına yalnızca bir belirteci üretmesi gerekir. Belirteçleri aşağıdaki yöntemi kullanarak, daha sonra yeniden üretilebilir. Tüm belirteçlerin kullanılarak oluşturulan **EventHubSendKey** anahtarı. Her belirteç benzersiz bir URI atanır. 'Resource' parametresi URI uç noktasına (Bu durumda event hub) hizmetinin karşılık gelir.

```csharp
public static string SharedAccessSignatureTokenProvider.GetSharedAccessSignature(string keyName, string sharedAccessKey, string resource, TimeSpan tokenTimeToLive)
```

Bu yöntem çağrılırken, URI olarak belirtilmelidir `https://<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`. Tüm belirteçlerin için URI dışında aynı olan `PUBLISHER_NAME`, olacağı için her bir belirteç farklı. İdeal olarak, `PUBLISHER_NAME` Bu belirteci aldığında istemci Kimliğini temsil eder.

Bu yöntem, aşağıdaki yapıya sahip bir belirteç oluşturur:

```csharp
SharedAccessSignature sr={URI}&sig={HMAC_SHA256_SIGNATURE}&se={EXPIRATION_TIME}&skn={KEY_NAME}
```

Belirteç sona erme zamanı, 1 Ocak 1970 saniyeler içinde belirtilir. Bir belirteç örneği verilmiştir:

```csharp
SharedAccessSignature sr=contoso&sig=nPzdNN%2Gli0ifrfJwaK4mkK0RqAB%2byJUlt%2bGFmBHG77A%3d&se=1403130337&skn=RootManageSharedAccessKey
```

Genellikle, belirteçleri istemci ömrü aşıyor veya benzer bir ömre sahip. İstemci yeni bir belirteç almak için bir özellik varsa, belirteçleri daha kısa bir kullanım ömrü ile kullanılabilir.

### <a name="sending-data"></a>Veri gönderme

Belirteçleri oluşturduktan sonra her bir istemci kendi benzersiz bir belirteç ile sağlanır.

İstemci, bir olay hub'ına veri gönderdiğinde, gönderme isteği belirteci ile etiketler. Bir saldırgan, gizlice ve belirteç çalma önlemek için olay hub'ı ile istemci arasındaki iletişimin şifreli bir kanal gerçekleşmelidir.

### <a name="blacklisting-clients"></a>İstemciler bloke liste oluşturma

Bir saldırgan tarafından bir belirteç çalınırsa, saldırgan belirteci çalınırsa istemcinin kimliğine bürünebilir. Farklı bir yayımcı kullanan yeni bir belirteç alıncaya kadar bir istemci kara liste istemci kullanılamaz yapar.

## <a name="authentication-of-back-end-applications"></a>Arka uç uygulamalarını kimlik doğrulaması

Event Hubs istemciler tarafından oluşturulan verileri arka uca uygulamaların kimliğini doğrulamak için Event Hubs için Service Bus konu başlıklarını kullandığınız modeline benzer bir güvenlik modeli kullanır. Bir Event hubs'ı tüketici grubu, bir Service Bus konu başlığına bir abonelik eşdeğerdir. Tüketici grubu oluşturma isteği tarafından bir belirteç verir olay hub'ı ya da olay hub'ı ait olduğu ad alanı için ayrıcalıkları yönetme bulunuyorsa, bir istemci bir tüketici grubu oluşturabilirsiniz. Bir istemci alma isteği, tüketici grubu, olay hub'ı veya olay hub'ı ait olduğu ad alanını alma hakları veren bir belirteç olarak bulunuyorsa bir tüketici grubundan veri kullanmasına izin verilmez.

Service Bus'ın geçerli sürümü için bireysel abonelikler SAS kuralları desteklemez. Aynı olay hub'ları tüketici grupları için de geçerlidir. SAS desteği için her iki özellik gelecekte eklenecektir.

Tek bir tüketici grubu için SAS kimlik doğrulaması olmaması durumunda, tüm tüketici grupları bir ortak anahtar ile güvenli hale getirmek için SAS anahtarları da kullanabilirsiniz. Bu yaklaşım, herhangi bir olay hub'ı tüketici gruplarını kullanmak bir uygulama sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Event Hubs hakkında daha fazla bilgi için aşağıdaki konulara ziyaret edin:

* [Event Hubs’a genel bakış]
* [Paylaşılan erişim imzaları'ne genel bakış]
* [Event Hubs kullanan örnek uygulamalar]

[Event Hubs’a genel bakış]: event-hubs-what-is-event-hubs.md
[Event Hubs kullanan örnek uygulamalar]: https://github.com/Azure/azure-event-hubs/tree/master/samples
[Paylaşılan erişim imzaları'ne genel bakış]: ../service-bus-messaging/service-bus-sas.md

