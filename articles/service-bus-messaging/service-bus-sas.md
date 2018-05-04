---
title: Paylaşılan erişim imzaları ile Azure Service Bus erişim denetimi | Microsoft Docs
description: Paylaşılan erişim imzaları genel bakış, SAS yetkilendirme Azure Service Bus ile ilgili ayrıntıları kullanarak Service Bus erişim denetimine genel bakış.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/14/2018
ms.author: sethm
ms.openlocfilehash: 420f4573fbe8b5139a4e1e5fa4dea3404c4e099d
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2018
---
# <a name="service-bus-access-control-with-shared-access-signatures"></a>Paylaşılan erişim imzaları ile Service Bus erişim denetimi

*Paylaşılan erişim imzası* (SAS) olan Service Bus Mesajlaşma hizmeti için birincil güvenlik mekanizması. Bu makalede, SAS, nasıl çalıştığını ve bunları bir platform belirsiz şekilde nasıl kullanılacağını açıklanmaktadır.

SAS yetkilendirme kurallarına göre Service Bus erişimi korur. Bu, bir ad alanı veya bir Mesajlaşma varlığı (geçiş, kuyruk veya konu) yapılandırılır. Bir yetkilendirme kuralı bir ada sahip, belirli haklar ile ilişkili ve şifreleme anahtarlarını çifti taşır. Bir SAS belirteci üretmek için kuralın adını ve anahtar hizmet veri yolu SDK'sı üzerinden veya kendi kodunuzu'nı kullanın. Bir istemci, istenen işlem için yetkilendirme kanıtlamak için Service Bus belirteç sonra geçiş yapabilir.

## <a name="overview-of-sas"></a>SAS genel bakış

Paylaşılan erişim imzaları basit belirteçleri kullanarak talep tabanlı yetkilendirme bir sistemdir. SAS kullanarak, anahtarları asla kablo geçirilir. Anahtarlar, şifreleme açısından daha sonra hizmeti tarafından doğrulanabilen bilgileri imzalamak için kullanılır. SAS benzer şekilde bir kullanıcı adı ve parola düzeni istemci hemen bir yetkilendirme kuralı adı ve eşleşen bir anahtarı elinde olduğu kullanılabilir. SAS da benzer bir federe güvenlik modeline burada istemci zaman sınırlı ve imzalanmış erişim belirteci bir güvenlik belirteci Hizmeti'nden imzalama anahtarı elinde gelmeden alır kullanılabilir.

Hizmet veri yolu SAS kimlik doğrulaması yapılandırılmış adında [paylaşılan erişim yetkilendirme kuralları](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) erişim hakları ve bir birincil ve ikincil şifreleme anahtar çiftini ilişkili. Anahtarları 256 bit Base64 gösteriminde değerlerdir. Ad alanı düzeyinde Service Bus kuralları yapılandırabilirsiniz [geçişleri](service-bus-fundamentals-hybrid-solutions.md#relays), [sıraları](service-bus-fundamentals-hybrid-solutions.md#queues), ve [konuları](service-bus-fundamentals-hybrid-solutions.md#topics).

[Paylaşılan erişim imzası](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) belirteç erişilen, kaynak anlık, geçerlilik süresi URI'sini seçilen Yetkilendirme kuralının adını içerir ve HMAC SHA256 şifreleme imzası kullanarak bu alanları hesaplanır birincil veya ikincil şifreleme anahtarı seçilen yetkilendirme kuralı.

## <a name="shared-access-authorization-policies"></a>Paylaşılan erişim Yetkilendirme İlkeleri

Her bir hizmet veri yolu ad alanı ve her Service Bus varlık kuralları oluşan bir paylaşılan erişim yetkilendirme ilkesi vardır. Bir ad alanı içinde tek tek ilke yapılandırmalarını bağımsız olarak tüm varlıklar için ad alanı düzeyinde ilke uygulanır.

Her yetkilendirme ilkesi kuralı için üç parça bilgi karar verin: **adı**, **kapsam**, ve **hakları**. **Adı** yalnızca bu; bu kapsam içinde benzersiz bir ad. Kapsamı kadar kolaydır: söz konusu kaynak URI'dir. Bir hizmet veri yolu ad alanı için kapsam tam etki alanı adı (FQDN) olduğu gibi `https://<yournamespace>.servicebus.windows.net/`.

İlke kuralı tarafından'basıp basmadığını değerlendirerek hakları bir bileşimi olabilir:

* 'Gönderme' - Confers sağındaki varlığa iletileri göndermek için
* 'Dinleme' - Confers (geçiş) dinlemek veya (sıra, abonelikleri) almak için sağ ve ilişkili tüm ileti işleme
* 'Yönet' - oluşturma ve varlıkları silme de dahil olmak üzere ad alanının topolojisini yönetme hakkı Confers

'Yönet' right 'Gönder' ve 'Al' hakları içerir.

Bir ad alanı veya varlık ilke kuralları, üç adet yer her temel hakları ve gönderme ve dinleme birleşimi kapsayan sağlayan en fazla 12 paylaşılan erişim yetkilendirme kuralları basılı tutabilirsiniz. SAS ilke deposu bu sınırı alt çizgiler, bir kullanıcı veya hizmet hesap deposu olması için tasarlanmamıştır. Uygulamanızın hizmet veri yolu kullanıcı veya hizmet kimlikleri göre erişim vermek gerekirse, bir kimlik doğrulama ve erişim denetimi sonra SAS belirteçleri bir güvenlik belirteci hizmeti uygulamanız gerekir.

Bir yetkilendirme kuralı atanmış bir *birincil anahtar* ve *ikincil anahtar*. Şifreleme açısından güçlü anahtarları şunlardır. Bunlar kaybolur veya bunları sızıntısı yok - bunlar her zaman kullanılabilir olması [Azure portal][Azure portal]. Oluşturulan anahtarları birini kullanabilirsiniz ve her zaman yeniden oluşturabilirsiniz. Yeniden oluşturmak veya bir anahtarı ilkesinde değiştirirseniz tüm daha önce hemen geçersiz duruma bu anahtarı temel verilen belirteçler. Ancak, bu tür simgelerinde göre oluşturulan devam eden bağlantıları belirtecin süresi doluncaya kadar çalışmaya devam eder.

Bir hizmet veri yolu ad alanı oluşturduğunuzda, ilke kuralı adlı **RootManageSharedAccessKey** ad alanı için otomatik olarak oluşturulur. Bu ilke tüm ad alanı için yönetme izinleri var. Bu kural bir yönetim gibi davran önerilir **kök** hesap ve uygulamanızda kullanmayın. Ek ilke kuralları oluşturabilirsiniz **yapılandırma** ad alanı portalda, Powershell veya Azure CLI aracılığıyla sekmesi.

## <a name="configuration-for-shared-access-signature-authentication"></a>Paylaşılan erişim imzası kimlik doğrulaması için yapılandırma

Yapılandırabileceğiniz [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) hizmet veri yolu ad alanları, kuyruk veya konuları kuralı. Yapılandırma bir [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) bir Service Bus abonelik şu anda desteklenmiyor ancak abonelikleri erişimi güvenli hale getirmek için bir ad alanı veya konu yapılandırılmış kurallarını kullanabilirsiniz. Bu yordam gösterilmektedir çalışan bir örnek için bkz [kullanarak paylaşılan erişim imzası (SAS) kimlik doğrulama hizmet veri yolu abonelikleri ile](http://code.msdn.microsoft.com/Using-Shared-Access-e605b37c) örnek.

![SAS](./media/service-bus-sas/service-bus-namespace.png)

Bu şekilde *manageRuleNS*, *sendRuleNS*, ve *listenRuleNS* S1 sırasını ve konu T1, için yetkilendirme kuralları uygula sırada *listenRuleQ*  ve *sendRuleQ* yalnızca sıra S1 uygulamak ve *sendRuleT* yalnızca konu T1 geçerlidir.

## <a name="generate-a-shared-access-signature-token"></a>Bir paylaşılan erişim imzası belirteci oluştur

Bir yetkilendirme kuralı adı erişimi olan herhangi bir istemci ve imzalama anahtarları biri bir SAS belirteci oluşturabilirsiniz. Belirteç aşağıdaki biçiminde bir dize olarak hazırlayın tarafından oluşturulur:

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

* **`se`** -Belirteç süre sonu anlık. Dönem saniyesi yansıtma tamsayı `00:00:00 UTC` 1 Ocak belirtecin süresi dolduğunda 1970'ten (UNIX dönem) üzerinde.
* **`skn`** -Yetkilendirme kuralının adı.
* **`sr`** -Erişilen kaynak URI'si.
* **`sig`** -İmzası.

`signature-string` SHA-256 karma kaynak URI'si hesaplanır (**kapsam** önceki bölümde açıklandığı gibi) ve anlık, belirteç süre sonu dize gösterimini CRLF ile ayrılır.

Karma hesaplama aşağıdaki sözde kodu benzer ve 256 bit/32 bayt karma değer döndürür.

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

Böylece alıcı karma aynı parametrelere sahip geçerli bir imzalama anahtarı elinde veren doğrulama yeniden hesaplamak belirteç karma olmayan değerler içeriyor. 

Kaynak URI erişim talep Service Bus kaynağının tam URI değil. Örneğin, `http://<namespace>.servicebus.windows.net/<entityPath>` veya `sb://<namespace>.servicebus.windows.net/<entityPath>`; diğer bir deyişle, `http://contoso.servicebus.windows.net/contosoTopics/T1/Subscriptions/S3`. URI olmalıdır [yüzde olarak kodlanmış](https://msdn.microsoft.com/library/4fkewx0t.aspx).

İmzalama için kullanılan paylaşılan erişim yetkilendirme kuralı bu URI tarafından ya da hiyerarşik üst öğelerinden biri tarafından belirtilen varlık yapılandırılması gerekir. Örneğin, `http://contoso.servicebus.windows.net/contosoTopics/T1` veya `http://contoso.servicebus.windows.net` önceki örnekte.

Bir SAS belirteci önekine sahip tüm kaynaklar için geçerli `<resourceURI>` kullanılan `signature-string`.

## <a name="regenerating-keys"></a>Anahtarlarını yeniden oluşturma

Kullanılan anahtarları düzenli aralıklarla yeniden önerilir [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) nesnesi. Yavaş anahtarlarını döndürmek için birincil ve ikincil anahtar yuva yok. Uygulamanızı genellikle birincil anahtarı kullanıyorsa, ikincil anahtar yuvaya birincil anahtarı kopyalayın ve sonra yalnızca birincil anahtar yeniden. Yeni birincil anahtar değerine sonra ikincil yuvasında eski birincil anahtar kullanarak erişim devam istemci uygulamalara yapılandırılabilir. Tüm istemciler güncelleştirilmiş sonra son olarak eski birincil anahtar devre dışı bırakmak için ikincil anahtarı yeniden oluşturabilirsiniz.

Bilmeniz veya bir anahtar güvenliği aşıldığında ve anahtarlarını iptal gerekir şüphe, her ikisini de yeniden oluşturabilmeniz [PrimaryKey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_PrimaryKey) ve [SecondaryKey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_SecondaryKey) , bir [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule), yeni anahtarlarla değiştirme. Bu yordam, eski anahtarlarla imzalanması tüm belirteçleri geçersiz kılar.

## <a name="shared-access-signature-authentication-with-service-bus"></a>Service Bus ile paylaşılan erişim imzası kimlik doğrulaması

Aşağıda açıklandığı senaryolar yetkilendirme kuralları yapılandırma, SAS belirteci ve istemci kimlik doğrulaması olarak oluşturulmasını içerir.

Bir tam için yapılandırma ve kullandığı SAS yetkilendirme gösterilmektedir bir hizmet veri yolu uygulaması çalışma örneği bkz [Service Bus ile paylaşılan erişim imzası kimlik doğrulaması](http://code.msdn.microsoft.com/Shared-Access-Signature-0a88adf8). Ad alanları veya hizmet veri yolu abonelikleri güvenli hale getirmek için konuları yapılandırılan SAS yetkilendirme kuralları kullanımını göstermektedir ilgili bir örnek aşağıda verilmiştir: [hizmet veri yolu abonelikleriilekullanarakpaylaşılanerişimimzası(SAS)kimlikdoğrulaması](http://code.msdn.microsoft.com/Using-Shared-Access-e605b37c).

## <a name="access-shared-access-authorization-rules-on-an-entity"></a>Bir varlık üzerinde erişim paylaşılan erişim yetkilendirme kuralları

Hizmet veri yolu .NET Framework kitaplıklarıyla erişebileceğiniz bir [Microsoft.ServiceBus.Messaging.SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) bir hizmet veri yolu kuyruğu ya da konu üzerinden yapılandırılmış nesne [AuthorizationRules](/dotnet/api/microsoft.servicebus.messaging.authorizationrules) karşılık gelen koleksiyon [QueueDescription](/dotnet/api/microsoft.servicebus.messaging.queuedescription) veya [TopicDescription](/dotnet/api/microsoft.servicebus.messaging.topicdescription).

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

Hizmet veri yolu .NET kitaplıklarıyla Azure .NET SDK'yı kullanarak uygulamaları aracılığıyla SAS yetkilendirme kullanabileceğiniz [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) sınıfı. Aşağıdaki kod bir Service Bus kuyruğuna ileti göndermek için belirteç sağlayıcı kullanımını gösterir. Burada, daha önce verilen belirteç için belirteç sağlayıcı Üreteç yöntemi geçebilen gösterilen kullanım alternatif.

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

Ayrıca belirteç sağlayıcı doğrudan belirteçlerini vermek için diğer istemcilere geçirmek için kullanabilirsiniz. 

Bağlantı dizeleri, bir kural adı içerebilir (*SharedAccessKeyName*) ve kuralın anahtarı (*SharedAccessKey*) ya da daha önce verilen bir belirteç (*SharedAccessSignature*). Bu herhangi bir oluşturucu veya bir bağlantı dizesi kabul Üreteç yöntemi geçirilen bağlantı dizesinde mevcut olduğunda, SAS belirteci sağlayıcısı otomatik olarak oluşturulan doldurulur ve.

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

Bir gönderici veya istemci bir SAS belirteci verirseniz, anahtarı doğrudan yoktur ve bunu elde etmek için karma tersine çeviremezsiniz. Bu nedenle, Denetim nasıl ve ne erişebilecekleri üzerinden uzun sahip. Anımsaması bir önemli İlkesi birincil anahtarda değiştirirseniz, tüm paylaşılan erişim imzaları oluşturulan geçersiz kılınır şeydir.

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
> Bağlantı ile oluşturduğunuz önemlidir **SASL kimlik doğrulama mekanizması ayarlamak için anonim** (ve değil varsayılan DÜZ kullanıcı adı ve parola SAS belirteci göndermek gerekmediğinde kullanılır).
> 
> 

Ardından, yayımcı SAS belirteci göndermek ve hizmetinden (belirteci doğrulama sonucu) yanıt almak için iki AMQP bağlantılarını oluşturur.

AMQP ileti bir dizi özellikler ve basit bir ileti daha fazla bilgi içerir. SAS belirteci (kendi oluşturucusunu kullanarak) ileti gövdesi ' dir. **"ReplyTo"** özelliği (değiştirebileceğiniz adını ve dinamik olarak hizmeti tarafından oluşturulan istiyorsanız) alıcı bağlantıyı doğrulama sonucu almak için düğüm adı olarak ayarlanmış. Son üç uygulama/özel özellikler hizmet tarafından yürütülecek olan ne tür bir işlemi belirtmek için kullanılır. CBS taslak belirtimine göre açıklandığı gibi olmalıdırlar **işlem adı** ("put-token"), **Belirtecin türü** (Bu durumda, bir `servicebus.windows.net:sastoken`) ve **İzleyici"adı"** belirtecin geçerli olduğu (tüm varlık).

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
| Bir ileti daha sonra gönderim için zamanlama; Örneğin, [ScheduleMessageAsync()](/dotnet/api/microsoft.azure.servicebus.queueclient.schedulemessageasync#Microsoft_Azure_ServiceBus_QueueClient_ScheduleMessageAsync_Microsoft_Azure_ServiceBus_Message_System_DateTimeOffset_) |Dinle | Herhangi bir geçerli sıra adresi
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