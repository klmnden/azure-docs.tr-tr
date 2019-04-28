---
title: Paylaşılan erişim imzaları ile Azure Service Bus erişim denetimi | Microsoft Docs
description: Paylaşılan erişim imzaları genel bakış, Azure Service Bus ile SAS yetkilendirme ayrıntılarını kullanarak Service Bus erişim denetimine genel bakış.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/14/2018
ms.author: aschhab
ms.openlocfilehash: 8f5c1755462d2bbd28dd7f8db427cda141817588
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61472236"
---
# <a name="service-bus-access-control-with-shared-access-signatures"></a>Paylaşılan erişim imzaları ile Service Bus erişim denetimi

*Paylaşılan erişim imzaları* (SA) olan Service Bus mesajlaşması için birincil güvenlik mekanizması. Bu makalede, SAS, nasıl çalıştıklarını ve bunları bir platformdan şekilde nasıl kullanacağınızı açıklar.

SAS, Service Bus yetkilendirme kurallarına göre erişimi korur. Bu, bir ad alanı veya bir Mesajlaşma varlığı (geçiş, kuyruk veya konu) yapılandırılır. Bir yetkilendirme kuralı bir ada sahip, belirli haklar ile ilişkilendirilir ve bir çift şifreleme anahtar taşır. Bir SAS belirteci oluşturmak için kuralın adı ve anahtarı aracılığıyla hizmet veri yolu SDK'sı veya kendi kodunuzu'nı kullanın. Bir istemci, ardından istenen işlem için yetkilendirme kanıtlamak için Service Bus belirteci geçirebilirsiniz.

## <a name="overview-of-sas"></a>SAS genel bakış

Paylaşılan erişim imzaları basit belirteçleri kullanarak bir beyana dayalı yetkilendirme mekanizması mevcuttur. SAS kullanarak, anahtarları asla kablo geçirilir. Anahtarlar şifreli olarak daha sonra hizmet tarafından doğrulanabilir bilgi imzalamak için kullanılır. SAS benzer bir kullanıcı adı ve parola düzeni için istemci anında bir yetkilendirme kuralı adı ve eşleşen bir anahtar elinde olduğu kullanılabilir. SAS de benzer bir Federasyon güvenlik modeli, burada istemci bir süre sınırlı ve imzalanmış bir erişim belirteci bir güvenlik belirteci Hizmeti'nden imzalama anahtarı elinde gelmeden alır kullanılabilir.

Service Bus SAS kimlik doğrulaması yapılandırılmış adlı [paylaşılan erişimi yetkilendirme kuralları](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) ilişkili erişim hakları ve bir çift birincil ve ikincil şifreleme anahtarına. Anahtarları Base64 gösterimine 256 bitlik değerler. Ad alanı düzeyinde, hizmet veri yolu kuralları yapılandırabilirsiniz [geçişleri](../service-bus-relay/relay-what-is-it.md), [kuyrukları](service-bus-messaging-overview.md#queues), ve [konuları](service-bus-messaging-overview.md#topics).

[Paylaşılan erişim imzası](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) belirteç erişilebilir, bir süre sonu anında, kaynağın URI'sini seçilen Yetkilendirme kuralının adını içerir ve HMAC SHA256 şifreleme imzası kullanarak bu alanları hesaplanır birincil veya ikincil şifreleme anahtarı seçilen yetkilendirme kuralı.

## <a name="shared-access-authorization-policies"></a>Paylaşılan erişim Yetkilendirme İlkeleri

Her Service Bus ad alanı ve her bir Service Bus varlık kuralları oluşan bir paylaşılan erişim yetkilendirme ilkesi var. Ad alanı düzeyinde bir ilke tek tek ilke yapılandırmalarını bakılmaksızın ad alanı içindeki tüm varlıklar için geçerlidir.

Her yetkilendirme ilkesi kuralı için üç parça bilgi karar verin: **adı**, **kapsam**, ve **hakları**. **Adı** yalnızca bu; bu kapsam içinde benzersiz bir ad. Kapsam oldukça kolaydır: söz konusu kaynak URI'dir. Bir Service Bus ad alanı için tam etki alanı adı (FQDN) gibi kapsamdır `https://<yournamespace>.servicebus.windows.net/`.

İlke kuralı tarafından ağır basıp basmadığını hakları, bir birleşimi olabilir:

* 'Gönder' - Confers varlığa ileti göndermek için sağ
* 'Dinleme' - Confers (geçiş) dinlemek veya (kuyruk, abonelik) alma hakkı ve ilgili tüm ileti işleme
* 'Yönetme' - oluşturma ve varlıkları silmek gibi ad alanının topolojisini yönetme hakkı Confers

'Yönet' sağ 'Gönder' ve 'Receive' hakları içerir.

Bir ad alanı veya varlık ilke kuralları, üç adet yer her temel hakları ve gönderme ve dinleme birleşimi kapsayan sağlayarak, 12 adede kadar paylaşılan erişimi yetkilendirme kuralları barındırabilir. SAS İlkesi depolamak bu sınır çizgileri, bir kullanıcı veya hizmet hesap deposu olması için tasarlanmamıştır. Service Bus'ın kullanıcı veya hizmet kimlikleri göre erişim vermek uygulamanız gerekiyorsa, bir kimlik doğrulama ve erişim denetimi sonra SAS belirteçlerini çıkartan güvenlik belirteci hizmeti uygulamanız gerekir.

Bir yetkilendirme kuralı atanmış bir *birincil anahtar* ve *ikincil anahtar*. Bu şifreleme açısından güçlü bir anahtarlarıdır. Bunlar kaybolur veya bunları sızıntı yoksa - bunlar her zaman içinde kullanıma sunulacak [Azure portalında][Azure portal]. Oluşturulan anahtarların kullanabilirsiniz ve dilediğiniz zaman yeniden oluşturabilirsiniz. Bir ilke anahtarı yeniden oluşturulmuş veya, tüm daha önce bu anahtarı hemen geçersiz duruma temel verilen belirteçler. Ancak, bu tür belirteçleri üzerinde göre oluşturulan devam eden bağlantıları belirteç süresi dolana kadar çalışmaya devam eder.

Service Bus ad alanı oluşturduğunuzda, bir ilke kuralı adlı **RootManageSharedAccessKey** ad alanı için otomatik olarak oluşturulur. Bu ilke, tüm ad alanı için izinleri yönetir sahiptir. Bu kural bir yönetim gibi ele almanız tavsiye edilir **kök** hesap ve uygulamanızda kullanmayın. Ek ilke kuralları oluşturabilirsiniz **yapılandırma** portalında, Powershell veya Azure CLI aracılığıyla ad alanı için sekmesinde.

## <a name="configuration-for-shared-access-signature-authentication"></a>Paylaşılan erişim imzası kimlik doğrulaması için yapılandırma

Yapılandırabileceğiniz [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) kural Service Bus ad alanı, kuyruklar veya Konular. Yapılandırma bir [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) üzerinde bir Service Bus abonelik şu anda desteklenmiyor ancak aboneliklerine erişimin güvenliğini sağlamak için bir ad alanı veya konu üzerinde yapılandırılan kuralları kullanabilirsiniz. Bu yordam gösterir çalışan bir örnek için bkz [kullanarak paylaşılan erişim imzası (SAS) kimlik doğrulaması ile Service Bus abonelikleri](https://code.msdn.microsoft.com/Using-Shared-Access-e605b37c) örnek.

![SAS](./media/service-bus-sas/service-bus-namespace.png)

Bu şekilde *manageRuleNS*, *sendRuleNS*, ve *listenRuleNS* S1 kuyruk ve konu T1, için yetkilendirme kuralları geçerlidir ancak *listenRuleQ*  ve *sendRuleQ* yalnızca kuyruğa S1 uygulamak ve *sendRuleT* yalnızca konu T1 geçerlidir.

## <a name="generate-a-shared-access-signature-token"></a>Paylaşılan erişim imzası belirteci oluştur

Bir yetkilendirme kuralı adı erişimi olan herhangi bir istemci ve kendi İmzalama anahtarları bir SAS belirteci oluşturur. Belirteç, aşağıdaki biçimde bir dizeye kaynaklı tarafından oluşturulur:

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

* **`se`** -Belirteç süre sonu anlık. Dönem beri geçen saniye sayısı yansıtan tamsayı `00:00:00 UTC` 1 Ocak belirtecin süresinin sona erdiği 1970 (UNIX dönem) üzerinde.
* **`skn`** -Yetkilendirme kuralının adı.
* **`sr`** -Erişilen kaynak URI'si.
* **`sig`** -İmzası.

`signature-string` SHA-256 karma kaynak URI'si hesaplanır (**kapsam** önceki bölümde açıklandığı gibi) ve anında, belirteç süre sonu dize gösterimini CRLF tarafından ayrılmış.

Karma hesaplama, aşağıdaki sözde koda benzer ve 256 bit/32 bayt karma değer döndürür.

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

Alıcı karma aynı parametrelere sahip geçerli bir imzalama anahtarı elinde veren doğrulanıyor yeniden oynatmanız, böylece belirteç karma olmayan değerler içeriyor.

URI tam erişim talep edildikten Service Bus kaynağın URI'sini bir kaynaktır. Örneğin, `http://<namespace>.servicebus.windows.net/<entityPath>` veya `sb://<namespace>.servicebus.windows.net/<entityPath>`; diğer bir deyişle, `http://contoso.servicebus.windows.net/contosoTopics/T1/Subscriptions/S3`. Bir URI olmalıdır [yüzde olarak kodlanmış](https://msdn.microsoft.com/library/4fkewx0t.aspx).

Bu URI veya hiyerarşik öğelerinden belirtilen varlık üzerinde imzalamak için kullanılan paylaşılan erişim yetkilendirme kuralı yapılandırılması gerekir. Örneğin, `http://contoso.servicebus.windows.net/contosoTopics/T1` veya `http://contoso.servicebus.windows.net` önceki örnekte.

Ön ekine sahip tüm kaynaklar için bir SAS belirteci geçerliyse `<resourceURI>` kullanılan `signature-string`.

## <a name="regenerating-keys"></a>Anahtarları yeniden oluşturuluyor

İçinde kullanılan anahtarlar düzenli aralıklarla yeniden önerilir [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) nesne. Kademeli olarak anahtarlarını döndürmek için birincil ve ikincil anahtar yuva yok. Uygulamanızı genellikle birincil anahtarı kullanır, ikincil anahtar yuvaya birincil anahtarı kopyalayın ve ardından yalnızca birincil anahtarı yeniden. Yeni birincil anahtar değerini daha sonra ikincil yuvada eski birincil anahtar kullanarak erişim devam istemci uygulamalara yapılandırılabilir. Tüm istemcilerin güncelleştirildikten sonra son olarak, eski birincil anahtarı devre dışı bırakmak için ikincil anahtarı yeniden oluşturabilirsiniz.

Bildiğiniz veya bir anahtar güvenliği aşıldığında ve anahtarlar iptal etmek sahip olduğunuz şüpheleniyorsanız, her ikisi de yeniden oluşturabilirsiniz [PrimaryKey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) ve [ikincil anahtarı oluşturma](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) , bir [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule), bunları yeni anahtarlarla değiştirme. Bu yordam, tüm belirteçlerin eski anahtarlarla imzalanması geçersiz kılar.

## <a name="shared-access-signature-authentication-with-service-bus"></a>Service Bus ile paylaşılan erişim imzası kimlik doğrulaması

Aşağıda açıklanan senaryolar yetkilendirme kuralları yapılandırma, SAS belirteçleri ve istemci yetkilendirme oluşturulmasını içerir.

Bir tam çalışma yapılandırması ve kullandığı SAS yetkilendirme gösteren bir Service Bus uygulaması örneğini görmek [Service Bus ile paylaşılan erişim imzası kimlik doğrulaması](https://code.msdn.microsoft.com/Shared-Access-Signature-0a88adf8). Ad alanları veya konuları Service Bus abonelikleri güvenliğini sağlamak için yapılandırılmış olan SAS yetkilendirme kuralları kullanışını ilgili bir örnek burada kullanılabilir: [Service Bus abonelikleri ile paylaşılan erişim imzası (SAS) kimlik doğrulaması kullanarak](https://code.msdn.microsoft.com/Using-Shared-Access-e605b37c).

## <a name="access-shared-access-authorization-rules-on-an-entity"></a>Bir varlıkta erişim paylaşılan erişimi yetkilendirme kuralları

Hizmet veri yolu .NET Framework kitaplıkları ile erişebileceğiniz bir [Microsoft.ServiceBus.Messaging.SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) bir Service Bus kuyruğu veya konuyu yapılandırılmış nesne [Authorizationrules'a](/dotnet/api/microsoft.servicebus.messaging.authorizationrules) karşılık gelen koleksiyon [QueueDescription](/dotnet/api/microsoft.servicebus.messaging.queuedescription) veya [TopicDescription](/dotnet/api/microsoft.servicebus.messaging.topicdescription).

Aşağıdaki kod, bir kuyruk için yetkilendirme kuralları eklemek gösterilmektedir.

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

Hizmet veri yolu .NET kitaplıklarıyla Azure .NET SDK kullanarak uygulamaları aracılığıyla SAS yetkilendirme kullanabileceğiniz [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) sınıfı. Aşağıdaki kod, bir Service Bus kuyruğuna ileti göndermek için belirteç sağlayıcısı kullanımını gösterir. Daha önce verilmiş bir belirteç için belirteç sağlayıcı Üreteç yöntemi de geçirebilirsiniz, burada gösterilen kullanım alternatifi.

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

Belirteç sağlayıcı doğrudan belirteçlerini vermek için diğer istemcilere geçirmek için kullanabilirsiniz.

Bağlantı dizeleri, kural adı içerebilir (*SharedAccessKeyName*) ve kuralın anahtarı (*SharedAccessKey*) veya daha önce verilmiş bir belirteç (*SharedAccessSignature*). Bu herhangi bir oluşturucu veya bir bağlantı dizesi kabul Üreteç yöntemi geçirilen bağlantı dizesinde mevcut olduğunda, SAS belirteci sağlayıcısı otomatik olarak oluşturulan doldurulur ve.

Service Bus geçişlerine SAS yetkilendirme kullanmak için Service Bus ad alanınızdaki yapılandırılmış SAS anahtarları kullanabileceğinizi unutmayın. Bir geçiş ad alanı üzerinde açıkça oluşturursanız ([NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) ile bir [RelayDescription](/dotnet/api/microsoft.servicebus.messaging.relaydescription)) nesne yalnızca bu geçiş için SAS kuralları ayarlayabilirsiniz. Service Bus abonelikleri SAS yetkilendirme kullanmak için yapılandırılmış bir Service Bus ad alanında veya konu SAS anahtarları kullanabilirsiniz.

## <a name="use-the-shared-access-signature-at-http-level"></a>Paylaşılan erişim imzası (HTTP düzeyinde) kullanın

Service Bus içinde herhangi bir varlık için paylaşılan erişim imzaları oluşturma artık bildiğinize göre bir HTTP POST gerçekleştirin geçmeye hazırsınız:

```http
POST https://<yournamespace>.servicebus.windows.net/<yourentity>/messages
Content-Type: application/json
Authorization: SharedAccessSignature sr=https%3A%2F%2F<yournamespace>.servicebus.windows.net%2F<yourentity>&sig=<yoursignature from code above>&se=1438205742&skn=KeyName
ContentType: application/atom+xml;type=entry;charset=utf-8
```

Bunun için her şeyin çalıştığını unutmayın. Bir kuyruk, konu veya abonelik için SAS oluşturabilirsiniz.

Bir gönderici veya istemci bir SAS belirteci izni verirseniz anahtarı doğrudan yoksa ve bunu elde etmek için karma geri alınamaz. Bu nedenle, nasıl yanı sıra, üzerinden erişim denetimi uzun vardır. Anımsanması bir önemli olan, birincil anahtar ilkesinde değiştirirseniz, tüm paylaşılan erişim imzaları oluşturulan geçersiz kılınır olmasıdır.

## <a name="use-the-shared-access-signature-at-amqp-level"></a>Paylaşılan erişim imzası (AMQP düzeyinde) kullanın

Önceki bölümde, Service Bus'na veri göndermek için bir HTTP POST isteği ile bir SAS belirteci kullanabilir öğrendiniz. Bildiğiniz gibi Advanced Message Queuing Protocol (tercih edilen Protokolü birçok senaryoda performansı artırmak için kullanılacak olan AMQP) kullanarak Service Bus erişebilirsiniz. AMQP ile SAS belirteci kullanımı belgede açıklanan [AMQP Claim-Based güvenlik sürüm 1.0](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc) diğer bir deyişle söz çalışma taslak bugün 2013 ancak Azure tarafından desteklenen iyi itibaren.

Service Bus veri göndermeye başlamadan önce yayımcı adlı iyi tanımlanmış bir AMQP düğüm için SAS belirteci bir AMQP iletisi içinde göndermelisiniz **$cbs** (almak ve tüm SAS doğrulamak için hizmet tarafından kullanılan bir "özel" sırası olarak görebileceğiniz belirteci). Yayımcı belirtmelisiniz **ReplyTo** AMQP ileti içinde alan; bu, hizmet yanıt (Basit istek/yanıt desen yayımcı ve hizmet arasında belirteç doğrulama sonucu olan yayımcıyla düğümü ). Bu yanıt düğümü oluşturulur "hareket halindeyken"dinamik oluşturma uzak düğümün hakkında"AMQP 1.0 belirtimi tarafından açıklandığı gibi". Yayımcı, SAS belirtecinin geçerli olup olmadığını denetleyerek sonra İleri gidin ve veri hizmetine göndermek başlatın.

Aşağıdaki adımlar ile AMQP protokolünü kullanarak SAS belirteci gönderme işlemini gösterir [AMQP.NET Lite](https://github.com/Azure/amqpnetlite) kitaplığı. C'de resmi (örneğin üzerinde WinRT, .NET Compact Framework, .NET mikro Framework ve Mono) hizmet veri yolu SDK'sı geliştirme kullanamıyorsanız kullanışlıdır\#. Elbette, bu kitaplığı nasıl talep tabanlı güvenlik anlamanıza yardımcı olması kullanışlıdır (ile bir HTTP POST isteği ve "Yetkilendirme" üst bilgisi içinde gönderilen SAS belirteci) HTTP düzeyinde şekli gördüğünüz gibi AMQP düzeyinde çalışır. AMQP gibi ayrıntılı bilgileri gerekmiyorsa, sizin için yapacak .NET Framework uygulamaları ile resmi hizmet veri yolu SDK'sını kullanabilirsiniz.

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

`PutCbsToken()` Yöntemi alır *bağlantı* (AMQP bağlantısı sınıf örneği tarafından sağlanan [AMQP .NET Lite Kitaplığı](https://github.com/Azure/amqpnetlite)) TCP bağlantısını temsil eden hizmete ve *sasToken* parametresine SAS belirteci göndermek için.

> [!NOTE]
> Bağlantı ile oluşturduğunuz önemlidir **SASL kimlik doğrulama mekanizmasını ayarlamak için anonim** (ve varsayılan DÜZ kullanıcı adını ve SAS belirteci göndermek ihtiyacınız kalmadığında kullanılan parola ile değil).
>
>

Ardından, yayımcı, SAS belirteci gönderme ve hizmetten yanıt (belirteç doğrulama sonucu) almak için iki AMQP bağlantıları oluşturur.

AMQP ileti bir dizi özellik ve basit bir ileti daha fazla bilgi içerir. (Kendi oluşturucuyu kullanarak) iletisinin gövdesi, SAS belirtecidir. **"ReplyTo"** özelliği (değiştirebilirsiniz adını ve hizmet tarafından dinamik olarak oluşturulur) alıcı bağlantıya doğrulama sonucu almak için düğüm adına ayarlanır. Son üç uygulama/özel özellikler hizmet tarafından yürütülecek olan ne tür bir işlem belirtmek için kullanılır. CBS taslak belirtimi tarafından açıklandığı gibi bunlar olmalıdır **işlem adı** ("put-token"), **Belirtecin türü** (Bu durumda, bir `servicebus.windows.net:sastoken`) ve **"name" hedef kitlenizin** belirtecin geçerli olduğu (tüm varlık).

SAS belirteci gönderen bağlantıyı gönderdikten sonra yayımcı alıcı bağlantıya yanıtı okumalısınız. Adlı bir uygulama özelliği basit bir AMQP iletisiyle yanıt olduğunu **"durum kodu"** bir HTTP durum kodu aynı değerleri içerebilir.

## <a name="rights-required-for-service-bus-operations"></a>Service Bus işlemleri için gereken hakları

Aşağıdaki tablo, Service Bus kaynakları üzerinde çeşitli işlemler için gerekli erişim haklarını gösterir.

| İşlem | Gerekli talep | Kapsam talebi |
| --- | --- | --- |
| **Namespace** | | |
| Üzerinde bir ad alanı yetkilendirme kuralı yapılandırma |Yönetme |Herhangi bir ad alanı adresi |
| **Hizmet kayıt defteri** | | |
| Özel ilkeler listeleme |Yönetme |Herhangi bir ad alanı adresi |
| Bir ad alanı üzerinde dinleme başlayın |Dinle |Herhangi bir ad alanı adresi |
| Bir ad alanı, bir dinleyici iletiler gönderme |Gönder |Herhangi bir ad alanı adresi |
| **Kuyruk** | | |
| Bir kuyruk oluşturma |Yönetme |Herhangi bir ad alanı adresi |
| Bir kuyruk silme |Yönetme |Herhangi bir geçerli kuyruk adresi |
| Kuyrukları listeleme |Yönetme |$ Kaynaklar/kuyruklar |
| Kuyruk açıklamasını alın |Yönetme |Herhangi bir geçerli kuyruk adresi |
| Bir kuyruk için yetkilendirme kuralı yapılandırma |Yönetme |Herhangi bir geçerli kuyruk adresi |
| İçine kuyruğa gönderme |Gönder |Herhangi bir geçerli kuyruk adresi |
| Bir kuyruktan ileti alma |Dinle |Herhangi bir geçerli kuyruk adresi |
| İptal veya iletiyi gözlem kilidi modunda aldıktan sonra iletileri tamamlayın |Dinle |Herhangi bir geçerli kuyruk adresi |
| Daha sonra almak için bir iletiyi ertele |Dinle |Herhangi bir geçerli kuyruk adresi |
| Bir ileti teslim edilemeyen iletiler |Dinle |Herhangi bir geçerli kuyruk adresi |
| Bir ileti kuyruğu oturumla ilişkili durumunu Al |Dinle |Herhangi bir geçerli kuyruk adresi |
| Bir ileti kuyruğu oturumla ilişkili durumunu ayarla |Dinle |Herhangi bir geçerli kuyruk adresi |
| Bir ileti sonraki teslimat zamanlaması; Örneğin, [ScheduleMessageAsync()](/dotnet/api/microsoft.azure.servicebus.queueclient.schedulemessageasync#Microsoft_Azure_ServiceBus_QueueClient_ScheduleMessageAsync_Microsoft_Azure_ServiceBus_Message_System_DateTimeOffset_) |Dinle | Herhangi bir geçerli kuyruk adresi
| **Konu** | | |
| Konu başlığı oluşturma |Yönetme |Herhangi bir ad alanı adresi |
| Bir konu Sil |Yönetme |Herhangi bir geçerli konunun adresi |
| Konuları listeleme |Yönetme |$ Kaynaklar/konuları |
| Konunun açıklamayı Al |Yönetme |Herhangi bir geçerli konunun adresi |
| Bir konu için yetkilendirme kuralı yapılandırma |Yönetme |Herhangi bir geçerli konunun adresi |
| Konuya gönderme |Gönder |Herhangi bir geçerli konunun adresi |
| **Abonelik** | | |
| Abonelik oluşturma |Yönetme |Herhangi bir ad alanı adresi |
| Aboneliği silin |Yönetme |../myTopic/Subscriptions/mySubscription |
| Aboneliklerini listeleme |Yönetme |../ myTopic/abonelikleri |
| Abonelik açıklaması Al |Yönetme |../myTopic/Subscriptions/mySubscription |
| İptal veya iletiyi gözlem kilidi modunda aldıktan sonra iletileri tamamlayın |Dinle |../myTopic/Subscriptions/mySubscription |
| Daha sonra almak için bir iletiyi ertele |Dinle |../myTopic/Subscriptions/mySubscription |
| Bir ileti teslim edilemeyen iletiler |Dinle |../myTopic/Subscriptions/mySubscription |
| Bir konu oturumla ilişkili durumunu Al |Dinle |../myTopic/Subscriptions/mySubscription |
| Bir konu oturumla ilişkili durumunu ayarla |Dinle |../myTopic/Subscriptions/mySubscription |
| **kuralları** | | |
| Kural oluşturma |Yönetme |../myTopic/Subscriptions/mySubscription |
| Kuralı silme |Yönetme |../myTopic/Subscriptions/mySubscription |
| Kuralları listeleme |Dinlemek veya yönetme |../myTopic/Subscriptions/mySubscription/Rules

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşma hizmeti hakkında daha fazla bilgi edinmek için aşağıdaki konu başlıklarına bakın.

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyruklarını kullanma](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[Azure portal]: https://portal.azure.com
