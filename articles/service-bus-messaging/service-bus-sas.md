---
title: "Paylaşılan erişim imzaları ile Azure Service Bus kimlik doğrulaması | Microsoft Docs"
description: "Hizmet veri yolu paylaşılan erişim imzaları genel bakış, SAS kimlik doğrulaması Azure Service Bus ile ilgili ayrıntıları kullanarak kimlik doğrulamasına genel bakış."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/21/2017
ms.author: sethm
ms.openlocfilehash: cdbac0fd18ad440ece35881cbe165c3c7eff8914
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2017
---
# <a name="service-bus-authentication-with-shared-access-signatures"></a>Paylaşılan erişim imzaları ile Service Bus kimlik doğrulaması

*Paylaşılan erişim imzası* (SAS) olan Service Bus Mesajlaşma hizmeti için birincil güvenlik mekanizması. Bu makalede, SAS, nasıl çalıştığını ve bunları bir platform belirsiz şekilde nasıl kullanılacağını açıklanmaktadır.

Hizmet veri yolu ad alanı veya belirli haklar ilişkili Mesajlaşma varlığıyla (kuyruk veya konu) yapılandırılmış bir erişim anahtarı kullanarak kimlik doğrulaması uygulamaları SAS kimlik doğrulaması sağlar. Bu anahtar ardından istemcilerin sırayla Service Bus kimlik doğrulaması yapmak için kullanabileceği bir SAS belirteci oluşturmak için de kullanabilirsiniz.

SAS kimlik doğrulama desteği Azure SDK sürüm 2.0 ve üzeri dahil edilmiştir.

## <a name="overview-of-sas"></a>SAS genel bakış

Paylaşılan erişim imzaları, SHA-256 güvenli karmaları veya URI dayalı bir kimlik doğrulama mekanizmasıdır. SAS tüm Service Bus hizmetler tarafından kullanılan son derece güçlü bir mekanizmadır. Gerçek kullanımda SAS iki bileşenden oluşur: bir *paylaşılan erişim ilkesi* ve *paylaşılan erişim imzası* (genellikle olarak adlandırılan bir *belirteci*).

Hizmet veri yolu SAS kimlik doğrulaması bir şifreleme anahtarıyla ilişkili haklar bir Service Bus kaynakta yapılandırılmasını içerir. İstemciler bir SAS belirteci sunarak Service Bus kaynaklarına erişim talep. Bu belirteç Kaynak URI erişilen oluşur ve bir süre sonu yapılandırılmış anahtarı ile imzalanmış.

Service Bus paylaşılan erişim imzası yetkilendirme kuralları yapılandırabilirsiniz [geçişleri](service-bus-fundamentals-hybrid-solutions.md#relays), [sıraları](service-bus-fundamentals-hybrid-solutions.md#queues), ve [konuları](service-bus-fundamentals-hybrid-solutions.md#topics).

SAS kimlik doğrulaması aşağıdaki öğeleri kullanır:

* [Paylaşılan erişim yetkilendirme kuralı](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule): A 256 bit Base64 gösteriminde birincil şifreleme anahtarı, isteğe bağlı bir ikincil anahtarı ve bir anahtar adı ve ilişkili haklar (koleksiyonu *dinleme*, *Gönder*, veya *Yönet* hakları).
* [Paylaşılan erişim imzası](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) belirteci: HMAC-erişilen Kaynak URI ve bir süre sonu şifreleme anahtarıyla oluşan SHA256 kaynak dizesi kullanılarak oluşturulan. İmza ve diğer öğeleri aşağıdaki bölümlerde açıklanan SAS belirteci oluşturmak için bir dize olarak biçimlendirilir.

## <a name="shared-access-policy"></a>Paylaşılan Erişim İlkesi

SAS hakkında anlamak için bir önemli bir ilkeyle başlatır şeydir. Her ilke için üç parça bilgi karar verin: **adı**, **kapsam**, ve **izinleri**. **Adı** yalnızca bu; bu kapsam içinde benzersiz bir ad. Kapsamı kadar kolaydır: söz konusu kaynak URI'dir. Bir hizmet veri yolu ad alanı için kapsam tam etki alanı adı (FQDN) olduğu gibi `https://<yournamespace>.servicebus.windows.net/`.

Bir ilke kullanılabilir izinlerini büyük ölçüde kendinden açıklamalıdır şunlardır:

* Gönder
* Dinle
* Yönetme

İlke oluşturduktan sonra atanan bir *birincil anahtar* ve *ikincil anahtar*. Şifreleme açısından güçlü anahtarları şunlardır. Bunlar kaybolur veya bunları sızıntısı yok - bunlar her zaman kullanılabilir olması [Azure portal][Azure portal]. Oluşturulan anahtarları birini kullanabilirsiniz ve her zaman yeniden oluşturabilirsiniz. Ancak, yeniden oluşturmak veya ilke birincil anahtarda değiştirirseniz tüm paylaşılan erişim imzaları oluşturulan geçersiz kılınır.

Bir hizmet veri yolu ad alanı oluşturduğunuzda, bir ilke adı verilen tüm ad alanı için otomatik olarak oluşturulur **RootManageSharedAccessKey**, ve bu ilkeyi tüm izinlere sahiptir. Olarak oturum yok **kök**, gerçekten iyi bir neden olmadıkça bu ilkeyi kullanmayın. Ek ilkelerinde oluşturabileceğiniz **yapılandırma** sekmesini portaldaki ad. Service Bus (ad alanı, kuyruk, vb.) içinde tek ağacı düzeyi bağlı en fazla 12 ilkeleri yalnızca olabilir dikkate almak önemlidir.

## <a name="configuration-for-shared-access-signature-authentication"></a>Paylaşılan erişim imzası kimlik doğrulaması için yapılandırma
Yapılandırabileceğiniz [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) hizmet veri yolu ad alanları, kuyruk veya konuları kuralı. Yapılandırma bir [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) bir Service Bus abonelik şu anda desteklenmiyor ancak abonelikleri erişimi güvenli hale getirmek için bir ad alanı veya konu yapılandırılmış kurallarını kullanabilirsiniz. Bu yordam gösterilmektedir çalışan bir örnek için bkz [kullanarak paylaşılan erişim imzası (SAS) kimlik doğrulama hizmet veri yolu abonelikleri ile](http://code.msdn.microsoft.com/Using-Shared-Access-e605b37c) örnek.

En fazla 12 Bu kurallar, bir hizmet veri yolu ad alanı, kuyruk veya konu yapılandırılabilir. Bu ad alanındaki tüm varlıklara bir hizmet veri yolu ad alanı üzerinde yapılandırılmış olan kurallar uygulanır.

![SAS](./media/service-bus-sas/service-bus-namespace.png)

Bu şekilde *manageRuleNS*, *sendRuleNS*, ve *listenRuleNS* S1 sırasını ve konu T1, için yetkilendirme kuralları uygula sırada *listenRuleQ*  ve *sendRuleQ* yalnızca sıra S1 uygulamak ve *sendRuleT* yalnızca konu T1 geçerlidir.

Anahtar parametreleri bir [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) aşağıdaki gibidir:

| Parametre | Açıklama |
| --- | --- |
| *Anahtar adı* |Yetkilendirme kuralı tanımlayan bir dize. |
| *PrimaryKey* |İmzalama ve SAS belirteci doğrulanırken bir base64 ile kodlanmış 256 bit birincil anahtar. |
| *SecondaryKey* |İmzalama ve SAS belirteci doğrulanırken bir base64 ile kodlanmış 256 bit ikincil anahtarı. |
| *ErişimHakları* |Yetkilendirme kuralı tarafından verilen erişim hakları listesi. Bu haklar dinle, Gönder ve Yönet hakların herhangi bir koleksiyonu olabilir. |

Hizmet veri yolu ad alanı sağlandığında bir [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule), ile [KeyName](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_KeyName) kümesine **RootManageSharedAccessKey**, varsayılan olarak oluşturulur.

## <a name="generate-a-shared-access-signature-token"></a>Paylaşılan erişim imzası (belirteç) oluştur

İlke hizmet veri yolu için erişim belirteci değil. İçinden erişim belirteci - birincil veya ikincil anahtar kullanılarak oluşturulan nesnedir. Paylaşılan erişim yetkilendirme kuralında belirtilen İmzalama anahtarları erişimi olan herhangi bir istemci, SAS belirteci oluşturabilir. Belirteç dikkatle aşağıdaki biçiminde bir dize olarak hazırlayın tarafından oluşturulur:

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

Burada `signature-string` belirtecin kapsamını SHA-256 Karması (**kapsam** önceki bölümde açıklandığı gibi) eklenmiş CRLF ve sona erme saati (dönem bu yana saniye cinsinden: `00:00:00 UTC` 1 Ocak 1970'ten üzerinde). 

> [!NOTE]
> Kısa belirteç süre sonu zamanı önlemek için en az bir 32 bit işaretsiz tamsayıyı veya uzun süre (64 bit) tamsayı tercihen süre sonu zamanı değeri kodlamak önerilir.  
> 
> 

Karma aşağıdaki sözde kodu benzer ve 32 bayt döndürür.

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

Karma olmayan değerler bulunan **SharedAccessSignature** böylece alıcı aynı sonucu verir emin olmak için aynı parametrelerle karma hesaplayabilirsiniz dize. URI kapsamı belirler ve ilke karma hesaplamak için kullanılacak anahtar adı tanımlar. Bu, güvenlik açısından önemlidir. İmza olduğu (Service Bus) alıcı hesaplar eşleşmiyorsa, erişim reddedildi. Bu noktada gönderen anahtarına erişime sahip ve ilkede belirtilen hakları verilmelidir emin olabilirsiniz.

Bu işlem için kodlanmış kaynak URI'si kullanması gerektiğini unutmayın. Kaynak URI erişim talep Service Bus kaynağının tam URI değil. Örneğin, `http://<namespace>.servicebus.windows.net/<entityPath>` veya `sb://<namespace>.servicebus.windows.net/<entityPath>`; diğer bir deyişle, `http://contoso.servicebus.windows.net/contosoTopics/T1/Subscriptions/S3`.

İmzalama için kullanılan paylaşılan erişim yetkilendirme kuralı bu URI tarafından ya da hiyerarşik üst öğelerinden biri tarafından belirtilen varlık yapılandırılması gerekir. Örneğin, `http://contoso.servicebus.windows.net/contosoTopics/T1` veya `http://contoso.servicebus.windows.net` önceki örnekte.

Bir SAS belirteci tüm kaynaklar için geçerli `<resourceURI>` kullanılan `signature-string`.

[KeyName](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_KeyName) SAS belirteci başvurduğu **keyName** belirteci üretmek için kullanılan paylaşılan erişim Yetkilendirme kuralının.

*URL kodlanmış resourceURI* dize-için-oturum aç imza hesaplaması sırasında kullanılan URI ile aynı olması gerekir. Olmalıdır [yüzde olarak kodlanmış](https://msdn.microsoft.com/library/4fkewx0t.aspx).

Kullanılan anahtarları düzenli aralıklarla yeniden önerilir [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) nesnesi. Uygulamalar genellikle kullanması gereken [PrimaryKey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_PrimaryKey) bir SAS belirteci oluşturmak için. Anahtarlarını yeniden oluştururken değiştirmelisiniz [SecondaryKey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_SecondaryKey) ile eski birincil anahtar ve yeni birincil anahtarı olarak yeni bir anahtar oluşturur. Bu eski birincil anahtarıyla verilmiş olan ve henüz süresinin dolmadığını yetkilendirme için belirteç kullanmaya devam etmenizi sağlar.

Bir anahtarınıza ve anahtarlarını iptal varsa, her ikisini de yeniden oluşturabilmeniz [PrimaryKey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_PrimaryKey) ve [SecondaryKey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_SecondaryKey) , bir [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule)değiştirerek bunları yeni anahtarlarla. Bu yordam, eski anahtarlarla imzalanması tüm belirteçleri geçersiz kılar.

## <a name="how-to-use-shared-access-signature-authentication-with-service-bus"></a>Service Bus ile paylaşılan erişim imzası kimlik doğrulaması kullanma

Aşağıdaki senaryolar, yetkilendirme kuralları yapılandırma, nesil SAS belirteci ve istemci kimlik doğrulaması içerir.

Bir tam için yapılandırma ve kullandığı SAS yetkilendirme gösterilmektedir bir hizmet veri yolu uygulaması çalışma örneği bkz [Service Bus ile paylaşılan erişim imzası kimlik doğrulaması](http://code.msdn.microsoft.com/Shared-Access-Signature-0a88adf8). Ad alanları veya hizmet veri yolu abonelikleri güvenli hale getirmek için konuları yapılandırılan SAS yetkilendirme kuralları kullanımını göstermektedir ilgili bir örnek aşağıda verilmiştir: [hizmet veri yolu abonelikleriilekullanarakpaylaşılanerişimimzası(SAS)kimlikdoğrulaması](http://code.msdn.microsoft.com/Using-Shared-Access-e605b37c).

## <a name="access-shared-access-authorization-rules-on-a-namespace"></a>Bir ad alanında erişim paylaşılan erişim yetkilendirme kuralları

Hizmet veri yolu ad alanı kökünde işlemler sertifika kimlik doğrulaması gerektirir. Azure aboneliğiniz için bir yönetim sertifikası yüklemeniz gerekir. Bir yönetim sertifikasını karşıya yüklemek için adımları izleyin [burada](../cloud-services/cloud-services-configure-ssl-certificate-portal.md#step-3-upload-a-certificate)kullanarak [Azure portal][Azure portal]. Azure yönetim sertifikaları hakkında daha fazla bilgi için bkz: [Azure sertifikalara genel bakış](../cloud-services/cloud-services-certs-create.md#what-are-management-certificates).

Bir hizmet veri yolu ad alanı paylaşılan erişim yetkilendirme kurallarının erişmek için uç nokta aşağıdaki gibidir:

```http
https://management.core.windows.net/{subscriptionId}/services/ServiceBus/namespaces/{namespace}/AuthorizationRules/
```

Oluşturmak için bir [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) nesne üzerinde bir hizmet veri yolu ad alanı, JSON veya XML olarak serileştirilmiş kural bilgilerle POST işlemine bu uç noktada yürütün. Örneğin:

```csharp
// Base address for accessing authorization rules on a namespace
string baseAddress = @"https://management.core.windows.net/<subscriptionId>/services/ServiceBus/namespaces/<namespace>/AuthorizationRules/";

// Configure authorization rule with base64-encoded 256-bit key and Send rights
var sendRule = new SharedAccessAuthorizationRule("contosoSendAll",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Send });

// Operations on the Service Bus namespace root require certificate authentication.
WebRequestHandler handler = new WebRequestHandler
{
    ClientCertificateOptions = ClientCertificateOption.Manual
};
// Access the management certificate by subject name
handler.ClientCertificates.Add(GetCertificate(<certificateSN>));

HttpClient httpClient = new HttpClient(handler)
{
    BaseAddress = new Uri(baseAddress)
};
httpClient.DefaultRequestHeaders.Accept.Add(
    new MediaTypeWithQualityHeaderValue("application/json"));
httpClient.DefaultRequestHeaders.Add("x-ms-version", "2015-01-01");

// Execute a POST operation on the baseAddress above to create an auth rule
var postResult = httpClient.PostAsJsonAsync("", sendRule).Result;
```

Benzer şekilde, ad yapılandırılmış yetkilendirme kuralları okumak için uç alma işlemi kullanın.

Belirli yetkilendirme kuralını silmek ve güncelleştirmek için aşağıdaki bitiş noktasını kullanın:

```http
https://management.core.windows.net/{subscriptionId}/services/ServiceBus/namespaces/{namespace}/AuthorizationRules/{KeyName}
```

## <a name="access-shared-access-authorization-rules-on-an-entity"></a>Bir varlık üzerinde erişim paylaşılan erişim yetkilendirme kuralları

Erişebileceğiniz bir [Microsoft.ServiceBus.Messaging.SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) bir hizmet veri yolu kuyruğu ya da konu üzerinden yapılandırılmış nesne [AuthorizationRules](/dotnet/api/microsoft.servicebus.messaging.authorizationrules) koleksiyonunda karşılık gelen [QueueDescription](/dotnet/api/microsoft.servicebus.messaging.queuedescription) veya [TopicDescription](/dotnet/api/microsoft.servicebus.messaging.topicdescription).

Aşağıdaki kod, sıra için yetkilendirme kuralları eklemek gösterilmiştir.

```csharp
// Create an instance of NamespaceManager for the operation
NamespaceManager nsm = NamespaceManager.CreateFromConnectionString(
    <connectionString> );
QueueDescription qd = new QueueDescription( <qPath> );

// Create a rule with send rights with keyName as "contosoQSendKey"
// and add it to the queue description.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoSendKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Send }));

// Create a rule with listen rights with keyName as "contosoQListenKey"
// and add it to the queue description.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoQListenKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Listen }));

// Create a rule with manage rights with keyName as "contosoQManageKey"
// and add it to the queue description.
// A rule with manage rights must also have send and receive rights.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoQManageKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] {AccessRights.Manage, AccessRights.Listen, AccessRights.Send }));

// Create the queue.
nsm.CreateQueue(qd);
```

## <a name="use-shared-access-signature-authorization"></a>Paylaşılan erişim imzası yetkilendirme kullanın

Hizmet veri yolu .NET kitaplıklarıyla Azure .NET SDK'yı kullanarak uygulamaları aracılığıyla SAS yetkilendirme kullanabileceğiniz [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) sınıfı. Aşağıdaki kod bir Service Bus kuyruğuna ileti göndermek için belirteç sağlayıcı kullanımını gösterir.

```csharp
Uri runtimeUri = ServiceBusEnvironment.CreateServiceUri("sb",
    <yourServiceNamespace>, string.Empty);
MessagingFactory mf = MessagingFactory.Create(runtimeUri,
    TokenProvider.CreateSharedAccessSignatureTokenProvider(keyName, key));
QueueClient sendClient = mf.CreateQueueClient(qPath);

//Sending hello message to queue.
BrokeredMessage helloMessage = new BrokeredMessage("Hello, Service Bus!");
helloMessage.MessageId = "SAS-Sample-Message";
sendClient.Send(helloMessage);
```

Uygulamalar da SAS kimlik doğrulaması için bağlantı dizelerini kabul yöntemlerinde bir SAS bağlantı dizesi kullanarak kullanabilirsiniz.

SAS yetkilendirme ile Service Bus geçişlerini kullanmak için üzerinde hizmet veri yolu ad alanı yapılandırılmış SAS anahtarları kullanabileceğinizi unutmayın. Bir geçiş ad açıkça oluşturursanız ([NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) ile bir [RelayDescription](/dotnet/api/microsoft.servicebus.messaging.relaydescription)) nesnesi, bu geçiş için yalnızca SAS kuralları ayarlayabilirsiniz. Hizmet veri yolu aboneliklerle SAS yetkilendirme kullanmak için bir hizmet veri yolu ad alanı veya konu yapılandırılmış SAS tuşlarını kullanabilirsiniz.

## <a name="use-the-shared-access-signature-at-http-level"></a>Paylaşılan erişim imzası (HTTP düzeyinde) kullanın

Service Bus içinde herhangi bir varlık için paylaşılan erişim imzaları oluşturma bildiğinize göre bir HTTP POST gerçekleştirmek hazırsınız:

```http
POST https://<yournamespace>.servicebus.windows.net/<yourentity>/messages
Content-Type: application/json
Authorization: SharedAccessSignature sr=https%3A%2F%2F<yournamespace>.servicebus.windows.net%2F<yourentity>&sig=<yoursignature from code above>&se=1438205742&skn=KeyName
ContentType: application/atom+xml;type=entry;charset=utf-8
``` 

Unutmayın, bu her şey için çalışır. Bir kuyruk, konu veya abonelik için SAS oluşturabilirsiniz. 

Bir gönderici veya istemci bir SAS belirteci verirseniz, anahtarı doğrudan yoktur ve bunu elde etmek için karma tersine çeviremezsiniz. Bu nedenle, Denetim nasıl ve ne erişebilecekleri üzerinden uzun sahip. Anımsaması bir önemli İlkesi birincil anahtarda değiştirirseniz, tüm paylaşılan erişim imzaları oluşturulan doğrulanacak şeydir.

## <a name="use-the-shared-access-signature-at-amqp-level"></a>Paylaşılan erişim imzası (AMQP düzeyinde) kullanın

Önceki bölümde, SAS belirteci ile HTTP POST isteği için hizmet veri yolu veri göndermek için kullanmayı gördünüz. Bildiğiniz gibi Service Bus Gelişmiş Message Queuing Protokolü (Performans nedeniyle, birçok senaryoda kullanmak için tercih edilen protokolü olan AMQP) kullanarak erişebilirsiniz. AMQP ile SAS belirteci kullanımını belgede açıklanan [AMQP Claim-Based güvenlik sürüm 1.0](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc) diğer bir deyişle içinde çalışma taslak bugün 2013 Azure tarafından desteklenen iyi ancak bu yana.

Service Bus hizmetine veri göndermeye başlamadan önce yayımcı adlı iyi tanımlanmış bir AMQP düğüm için bir AMQP iletisi içinde SAS belirteci göndermelidir **$cbs** (edinmeli ve tüm SAS doğrulamak için hizmet tarafından kullanılan bir "özel" sırası olarak görebileceğiniz belirteçleri). Yayımcı belirtmelisiniz **ReplyTo** alan AMQP ileti içine; bu, hizmet yanıtlar (Basit istek/yanıt desen yayımcı ve hizmeti arasında belirteci doğrulama sonucu yayımcıyla düğümü ). Bu yanıt düğüm oluşturulmuş "açık"dinamik oluşturma uzak düğümün hakkında"tarafından AMQP 1.0 belirtimi açıklandığı gibi konuşma uç,". Yayımcı, SAS belirteci geçerli olup olmadığını kontrol ettikten sonra İleri gidin ve hizmete veri göndermek başlatın.

Aşağıdaki adımlar AMQP protokolünü kullanarak ile SAS belirteci göndermek nasıl gösterir [AMQP.Net Lite](https://github.com/Azure/amqpnetlite) kitaplığı. Bu resmi hizmet veri yolu SDK'sı kullanamıyorsanız faydalı olur (örneğin üzerinde WinRT, .net Compact Framework, .net mikro Framework ve Mono) C'de geliştirme\#. Elbette, bu kitaplık nasıl talep tabanlı güvenlik anlamanıza yardımcı olması yararlıdır (ile bir HTTP POST isteği ve "Yetkilendirme" üstbilgisi içinde gönderilen SAS belirteci) HTTP düzeyinde nasıl çalışır anlatıldığı gibi AMQP düzeyinde çalışır. AMQP hakkında ayrıntılı oluşturabildiğinden gerekmiyorsa, resmi hizmet veri yolu SDK'sı .net ile kullanabileceğiniz sizin için ne yapacağını Framework uygulamaları.

### <a name="c35"></a>C&#35;

```csharp
/// <summary>
/// Send claim-based security (CBS) token
/// </summary>
/// <param name="shareAccessSignature">Shared access signature (token) to send</param>
private bool PutCbsToken(Connection connection, string sasToken)
{
    bool result = true;
    Session session = new Session(connection);

    string cbsClientAddress = "cbs-client-reply-to";
    var cbsSender = new SenderLink(session, "cbs-sender", "$cbs");
    var cbsReceiver = new ReceiverLink(session, cbsClientAddress, "$cbs");

    // construct the put-token message
    var request = new Message(sasToken);
    request.Properties = new Properties();
    request.Properties.MessageId = Guid.NewGuid().ToString();
    request.Properties.ReplyTo = cbsClientAddress;
    request.ApplicationProperties = new ApplicationProperties();
    request.ApplicationProperties["operation"] = "put-token";
    request.ApplicationProperties["type"] = "servicebus.windows.net:sastoken";
    request.ApplicationProperties["name"] = Fx.Format("amqp://{0}/{1}", sbNamespace, entity);
    cbsSender.Send(request);

    // receive the response
    var response = cbsReceiver.Receive();
    if (response == null || response.Properties == null || response.ApplicationProperties == null)
    {
        result = false;
    }
    else
    {
        int statusCode = (int)response.ApplicationProperties["status-code"];
        if (statusCode != (int)HttpStatusCode.Accepted && statusCode != (int)HttpStatusCode.OK)
        {
            result = false;
        }
    }

    // the sender/receiver may be kept open for refreshing tokens
    cbsSender.Close();
    cbsReceiver.Close();
    session.Close();

    return result;
}
```

`PutCbsToken()` Yöntemi alır *bağlantı* (AMQP bağlantı sınıf örneği tarafından sağlanan gibi [AMQP .NET Lite Kitaplığı](https://github.com/Azure/amqpnetlite)) TCP bağlantısı temsil eden hizmet ve *sasToken* SAS parametre belirteci göndermek için. 

> [!NOTE]
> Bağlantı ile oluşturduğunuz önemlidir **SASL kimlik doğrulama mekanizması ayarlamak için dış** (ve değil varsayılan DÜZ kullanıcı adı ve parola SAS belirteci göndermek gerekmediğinde kullanılır).
> 
> 

Ardından, yayımcı SAS belirteci göndermek ve hizmetinden (belirteci doğrulama sonucu) yanıt almak için iki AMQP bağlantılarını oluşturur.

AMQP ileti bir dizi özellikler ve basit bir ileti daha fazla bilgi içerir. SAS belirteci (kendi oluşturucusunu kullanarak) ileti gövdesi ' dir. **"ReplyTo"** özelliği (değiştirebileceğiniz adını ve dinamik olarak hizmeti tarafından oluşturulan istiyorsanız) alıcı bağlantıyı doğrulama sonucu almak için düğüm adı olarak ayarlanmış. Son üç uygulama/özel özellikler hizmet tarafından yürütülecek olan ne tür bir işlemi belirtmek için kullanılır. CBS taslak belirtimine göre açıklandığı gibi olmalıdırlar **işlem adı** ("put-token"), **Belirtecin türü** (Bu durumda, bir "servicebus.windows.net:sastoken"), gelen ve **"adı" hedef kitleyi** belirtecin geçerli olduğu (tüm varlık).

SAS belirteci gönderen bağlantıyı gönderdikten sonra yayımcı alıcı bağlantıya yanıt okumalısınız. Adlı bir uygulama özelliği basit bir AMQP iletisiyle yanıt olan **"durum kodu"** bir HTTP durum kodu aynı değerlere içerebilir.

## <a name="rights-required-for-service-bus-operations"></a>Hizmet veri yolu işlemleri için gereken haklar

Aşağıdaki tabloda Service Bus kaynaklarını üzerinde çeşitli işlemler için gerekli erişim haklarını gösterir.

| İşlem | Gerekli talep | Talep kapsamı |
| --- | --- | --- |
| **Namespace** | | |
| Yetkilendirme kuralı üzerinde bir ad alanı yapılandırma |Yönetme |Herhangi bir ad alanı adresi |
| **Hizmet kayıt defteri** | | |
| Özel ilkeler listeleme |Yönetme |Herhangi bir ad alanı adresi |
| Bir ad alanı üzerinde dinleme yapmaya başlamasını |Dinle |Herhangi bir ad alanı adresi |
| Bir ad alanı konumundaki bir dinleyici iletileri gönder |Gönder |Herhangi bir ad alanı adresi |
| **Sırası** | | |
| Bir kuyruk oluşturma |Yönetme |Herhangi bir ad alanı adresi |
| Bir kuyruk silme |Yönetme |Herhangi bir geçerli sıra adresi |
| Kuyruklar listeleme |Yönetme |$ Kaynakları/sıraları |
| Al sıra açıklaması |Yönetme |Herhangi bir geçerli sıra adresi |
| Sıra için yetkilendirme kuralı yapılandırma |Yönetme |Herhangi bir geçerli sıra adresi |
| İçine kuyruğa gönderme |Gönder |Herhangi bir geçerli sıra adresi |
| Kuyruktan ileti alma |Dinle |Herhangi bir geçerli sıra adresi |
| Abandon veya gözlem kilidinin modunda iletiyi aldıktan sonra tamamlandı iletileri |Dinle |Herhangi bir geçerli sıra adresi |
| Sonraki alınması için bir ileti erteleme |Dinle |Herhangi bir geçerli sıra adresi |
| Sahipsiz bir ileti |Dinle |Herhangi bir geçerli sıra adresi |
| Bir ileti sırası oturumla ilişkili durumunu Al |Dinle |Herhangi bir geçerli sıra adresi |
| Bir ileti sırası oturumla ilişkili durumunu ayarlama |Dinle |Herhangi bir geçerli sıra adresi |
| **Konu** | | |
| Konu başlığı oluşturma |Yönetme |Herhangi bir ad alanı adresi |
| Bir konu Sil |Yönetme |Herhangi bir geçerli konu adresi |
| Konular listeleme |Yönetme |$ Kaynakları/konuları |
| Al konu açıklaması |Yönetme |Herhangi bir geçerli konu adresi |
| Bir konu için yetkilendirme kuralı yapılandırma |Yönetme |Herhangi bir geçerli konu adresi |
| Konuya Gönder |Gönder |Herhangi bir geçerli konu adresi |
| **Abonelik** | | |
| Abonelik oluşturma |Yönetme |Herhangi bir ad alanı adresi |
| Aboneliği silin |Yönetme |../myTopic/Subscriptions/mySubscription |
| Aboneliklerini listeleme |Yönetme |../ myTopic/abonelikleri |
| Al abonelik açıklaması |Yönetme |../myTopic/Subscriptions/mySubscription |
| Abandon veya gözlem kilidinin modunda iletiyi aldıktan sonra tamamlandı iletileri |Dinle |../myTopic/Subscriptions/mySubscription |
| Sonraki alınması için bir ileti erteleme |Dinle |../myTopic/Subscriptions/mySubscription |
| Sahipsiz bir ileti |Dinle |../myTopic/Subscriptions/mySubscription |
| Bir konu oturumla ilişkili durumunu Al |Dinle |../myTopic/Subscriptions/mySubscription |
| Bir konu oturumla ilişkili durumunu ayarlama |Dinle |../myTopic/Subscriptions/mySubscription |
| **Kuralları** | | |
| Bir kural oluşturun |Yönetme |../myTopic/Subscriptions/mySubscription |
| Kural silme |Yönetme |../myTopic/Subscriptions/mySubscription |
| Kuralları listeleme |Dinlemek veya yönetme |../myTopic/Subscriptions/mySubscription/Rules 

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşma hizmeti hakkında daha fazla bilgi edinmek için aşağıdaki konu başlıklarına bakın.

* [Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyruklarını kullanma](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[Azure portal]: https://portal.azure.com