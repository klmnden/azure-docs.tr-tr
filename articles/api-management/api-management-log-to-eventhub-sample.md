---
title: "Azure API Management, olay hub'ları ve Runscope API izleme | Microsoft Docs"
description: "Bağlanan Azure API Management, Azure olay hub'ları ve günlüğe kaydetme ve izleme HTTP Runscope günlük eventhub ilkeyle gösteren örnek uygulama"
services: api-management
documentationcenter: 
author: darrelmiller
manager: erikre
editor: 
ms.assetid: c528cf6f-5f16-4a06-beea-fa1207541a47
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 70ee752c5639c90f77dde104ce85eec0a1062300
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="monitor-your-apis-with-azure-api-management-event-hubs-and-runscope"></a>Azure API Management, olay hub'ları ve Runscope Apı'lerinizi izleme
[API Management hizmeti](api-management-key-concepts.md) HTTP API'nizi gönderilen HTTP isteklerinin işlenmesini geliştirmek için çok sayıda özellik sağlar. Ancak, isteklerin ve yanıtların varlığını geçici. İstek yapıldığında ve arka uç API'niz için API Management hizmeti aracılığıyla akar. API'nizi isteği işler ve bir yanıt API tüketiciye geriye doğru akar. API Management hizmeti bazı önemli istatistik görüntü API'leri hakkında Publisher portal panosunda, ancak ötesine ayrıntıları kayboldu, tutar.

Kullanarak [günlük eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) [İlkesi](api-management-howto-policies.md) API Management hizmeti, herhangi bir ayrıntıyı istek ve yanıt olarak gönderebileceğiniz bir [Azure olay hub'ı](../event-hubs/event-hubs-what-is-event-hubs.md). Çeşitli nedenlerle neden HTTP iletileri için Apı'lerinizi gönderilen olaylar oluşturmak isteyebilirsiniz vardır. Denetim izi güncelleştirmeler, kullanım analizi, özel durum uyarı ve 3 taraf tümleştirmeler bazı örnekler.   

Bu makalede, tüm HTTP istek ve yanıt iletisi yakalamak, olay Hub'ına göndermek ve HTTP günlüğe kaydetme ve izleme hizmetleri sağlayan hizmet üçüncü bir tarafa ileti geçiş gösterilmiştir.

## <a name="why-send-from-api-management-service"></a>API Management hizmetinden neden gönderilsin mi?
HTTP istekleri ve yanıtları yakalamak ve günlüğe kaydetme ve izleme sistemleri içine akış için HTTP API çerçeveleri takın HTTP Ara yazmanız mümkündür. Bu yaklaşımın dezavantajı, HTTP ara yazılımı API'si arka uç tümleştirilmesi gerekir ve platform API eşleşmelidir ' dir. Birden çok API'leri varsa her bir ara yazılım dağıtmanız gerekir. Genellikle arka uç API'leri neden güncelleştirilemiyor nedenleri vardır.

Günlüğe kaydetme altyapısı ile tümleştirmek için Azure API Management hizmeti kullanarak merkezi ve platformdan bağımsız bir çözüm sağlar. Ayrıca, kısmen son biçimde ölçeklendirilebilir [coğrafi çoğaltma](api-management-howto-deploy-multi-region.md) Azure API yönetimi özellikleri.

## <a name="why-send-to-an-azure-event-hub"></a>Neden Azure olay Hub'ına gönderilsin mi?
Bu sorun, neden Azure Event Hubs için özel bir ilke oluşturmak makul mi? Burada isteklerim oturum isteyebilirsiniz pek çok farklı yerde vardır. Neden yalnızca isteklerini doğrudan son hedefe gönderilsin mi?  Bir seçenektir. Ancak, bir API management hizmeti günlük kaydı isteklerinden yaparken günlük iletilerini API performansını nasıl etkiler dikkate alınması gereken gereklidir. Aşamalı yükleme artışlar sistem bileşenleri kullanılabilir örneklerini artırarak veya coğrafi çoğaltma yararlanarak işlenebilir. Ancak, kısa ani trafiğinin yük altında yavaş istekleri günlüğe kaydetme altyapısına başlatırsanız, önemli ölçüde Gecikmeli isteklerine neden olabilir.

Azure Event Hubs, HTTP çoğu API işlem isteklerinin sayısı daha için olayların ne kadar daha yüksek bir sayı ile ilgilenen kapasiteli giriş çok büyük miktarda veri için tasarlanmıştır. Olay hub'ı Gelişmiş arabellek API management hizmetiniz depolamak ve iletileri işleme altyapısı arasındaki bir tür olarak görev yapar. Bu API performansınızı nedeniyle günlük altyapısı görmeyecektir sağlar.  

Verileri bir olay Hub kalıcıdır ve işlemek olay hub'ı Tüketiciler için bekleyeceği geçtikten sonra. Olay hub'ı nasıl işlenir ilgilenmez, yalnızca ileti başarıyla teslim emin olmanızı hedefler cares.     

Olay hub'ları birden çok tüketici grupları akış olayları yeteneğine sahip. Bu, tamamen farklı sistemleri tarafından işlenecek olayların sağlar. Bu, yalnızca bir olay oluşturulması gereken şekilde ek gecikmeler API Management hizmeti içinde API isteğinin işlenmesi üzerinde koymadan birçok tümleştirme senaryolarına destek sağlar.

## <a name="a-policy-to-send-applicationhttp-messages"></a>Uygulama/http iletileri göndermek için bir ilke
Bir olay hub'ı olay verisi basit bir dize olarak kabul eder. Bu dizeyi içeriğini tamamen size bağlıdır. Bir HTTP isteğini oluşturan paketini ve Event Hubs'a gönderme için kimliğinizi biçim dizesi istek veya yanıt bilgilerle gerekiyor. Bu gibi durumlarda varolan bir biçim ise yeniden kullanabilirsiniz, sonra biz kendi kod ayrıştırma yazmak olmayabilir. Başlangıçta t kullanarak kabul [HAR](http://www.softwareishard.com/blog/har-12-spec/) HTTP istekleri ve yanıtları göndermek için. Ancak, bu biçim HTTP istekleri dizisi tabanlı JSON biçiminde depolamak için optimize edilmiştir. Kablo üzerinden HTTP ileti iletme senaryosu için gereksiz karmaşıklık eklenen zorunlu öğelerin sayısını içeriyor.  

Alternatif bir seçenek kullanılmasıydır `application/http` medya türü HTTP belirtiminde açıklandığı gibi [RFC 7230](http://tools.ietf.org/html/rfc7230). Bu ortam türünü gerçekte HTTP ileti kablo üzerinden göndermek için kullanılan tam aynı biçimi kullanır, ancak tüm ileti başka bir HTTP istek gövdesinde put olabilir. Örneğimizde biz yalnızca gövde bizim iletisi olarak Event Hubs'a göndermek için kullanacağınız. Rahat, var. bir çözümleyici yok [Microsoft ASP.NET Web API 2.2 istemci](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) Bu biçim ayrıştırabilir ve yerel Dönüştür kitaplıkları `HttpRequestMessage` ve `HttpResponseMessage` nesneleri.

C# dayalı yararlanmak ihtiyacımız bu iletiyi oluşturmak için [ilke ifadelerini](https://msdn.microsoft.com/library/azure/dn910913.aspx) Azure API Management'te. Azure Event Hubs'a bir HTTP istek iletisini gönderen İlkesi aşağıdadır.

```xml
<log-to-eventhub logger-id="conferencelogger" partition-id="0">
@{
   var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                               context.Request.Method,
                                               context.Request.Url.Path + context.Request.Url.QueryString);

   var body = context.Request.Body?.As<string>(true);
   if (body != null && body.Length > 1024)
   {
       body = body.Substring(0, 1024);
   }

   var headers = context.Request.Headers
                          .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                          .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                          .ToArray<string>();

   var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

   return "request:"   + context.Variables["message-id"] + "\n"
                       + requestLine + headerString + "\r\n" + body;
}
</log-to-eventhub>
```

### <a name="policy-declaration"></a>İlke bildirimi
Var. Bu ilke ifadesi hakkında söz değerinde belirli birkaç. Günlük eventhub İlkesi API Management hizmet içinde oluşturulan Günlükçü adına başvuran Günlükçü kimliği adlı bir öznitelik içeriyor. Bir olay hub'ı Günlükçü API Management hizmet Kurulum nasıl ayrıntılarını belgesinde bulunabilir [Azure Event hubs'a Azure API Management olaylarını günlüğe kaydedecek şekilde nasıl](api-management-howto-log-event-hubs.md). İkinci öznitelik, iletide depolamak için bölüm Event Hubs yönlendiren isteğe bağlı bir parametredir. Olay hub'ları, en az iki gerekli ve scalabilty için bölümleri kullanın. İletilerin sıralı teslimini yalnızca bir bölüm içinde sağlanır. Biz olay hub'ı ileti yerleştirmek için hangi bölümünde istemeniz değil, yükü dağıtmak için hepsini algoritması kullanır. Ancak, bazı bozuk işlenecek bizim iletilerinin neden olabilir.  

### <a name="partitions"></a>Bölümler
Bizim iletileri tüketiciye sırayla teslim edilir ve bölümlerinin yük dağıtım özellikten yararlanabilir emin olmak için bir bölüm ve HTTP yanıt iletilerini ikinci bir bölüm için HTTP isteği iletilerini göndermek seçtiğim. Bu durum, hatta yük dağıtımı güvence altına alır ve tüm isteklerin sırada kullanılır ve tüm yanıtları sırayla tüketilebilir garanti ediyoruz. Karşılık gelen isteği önce kullanılması için bir yanıt mümkündür, ancak sahibiz gibi bir sorun olmadığı yanıtları ilişkilendirme için farklı bir mekanizma ister ve istekleri her zaman yanıtları önce gelen biliyoruz.

### <a name="http-payloads"></a>HTTP yükü
Yapı sonra `requestLine` biz istek gövdesi kesilmiş denetleyin. İstek gövdesi için yalnızca 1024 kesilir. Tek bir olay hub'ı ileti 256 KB ile sınırlı ancak bu, artırılacak bazı HTTP iletisi tek iletiye sığmayacak değil gövdeleri, büyük olasılıkla gelir. Günlüğe kaydetme ve analiz yaparken önemli miktarda bilgi yalnızca HTTP isteği çizgi ve üst bilgileri elde edilebilir. Ayrıca, birçok API istekleri yalnızca küçük gövdeleri dönün ve böylece büyük gövdeleri kesilmesiyle tarafından bilgi değeri kaybı oldukça tüm gövdesi içeriği korumak için Aktarım, işleme ve depolama maliyetlerini azaltma kıyasla en alt düzeydedir. Gövde işleme hakkında son OneNote olduğu geçirmek ihtiyacımız `true` AS<string>() yönteminin gövdesi içeriği okumakta olduğunuz nedeni ancak, aynı zamanda arka uç gövdesi okuyabilir API istiyor. Bu yöntem true geçirerek biz böylece ikinci kez okunabilecek arabellekli için gövdesini neden. Bu çok büyük dosyaları karşıya yükleme veya uzun yoklama kullanan bir API varsa bilincinde olmak önemlidir. Bu durumlarda gövde hiç okuma önlemek en iyi olacaktır.   

### <a name="http-headers"></a>HTTP üstbilgileri
HTTP üstbilgileri yalnızca üzerinden basit anahtar/değer çifti biçiminde ileti biçimi içine aktarılabilir. Kimlik bilgileri gereksiz yere sızmasını önlemek için belirli güvenlik duyarlı alanları, Şerit seçtiniz. API anahtarları ve diğer kimlik bilgilerini analiz amaçlı kullanılacak düşüktür. Biz kullanıcı ve belirli ürün çözümleme yapmak isterseniz, kullandıkları sonra biz değerinden alabilir `context` nesne ve iletiye ekleyin.     

### <a name="message-metadata"></a>İleti meta verileri
Olay hub'ına göndermek için tam iletiyi oluştururken, ilk satırı değil gerçekte parçası `application/http` ileti. İlk satırı ileti bir istek veya yanıt iletisi ve yanıtları isteklerine ilişkilendirmek için kullanılan bir ileti kimliği olduğundan oluşan ek meta verilerdir. İleti kimliği, şuna benzer başka bir ilke kullanılarak oluşturulur:

```xml
<set-variable name="message-id" value="@(Guid.NewGuid())" />
```

Biz istek iletisi oluşturulan, bir değişkende depolanan kadar yanıtta döndürülen ve sonra yalnızca istek ve yanıt tek bir ileti gönderilir. Ancak, istek ve yanıt bağımsız olarak gönderme ve iki ilişkilendirmek için bir ileti kimliği kullanılarak, biz biraz daha fazla esneklik ileti boyutu, ileti sırası ve istek koruyarak görünür yaparken birden çok bölüm yararlanmak olanağı Al Bizim günlük panosunda daha erken. Ayrıca burada geçerli bir yanıt hiçbir zaman muhtemelen bir API Management hizmeti önemli isteği hatası nedeniyle olay hub'ına gönderilen ancak biz yine isteğin bir kayda sahip olacaksınız bazı senaryolar olabilir.

Yanıt HTTP iletisi göndermek için ilke isteği çok benzer ve bu nedenle tam ilke yapılandırması şöyle görünür:

```xml
<policies>
  <inbound>
      <set-variable name="message-id" value="@(Guid.NewGuid())" />
      <log-to-eventhub logger-id="conferencelogger" partition-id="0">
      @{
          var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                      context.Request.Method,
                                                      context.Request.Url.Path + context.Request.Url.QueryString);

          var body = context.Request.Body?.As<string>(true);
          if (body != null && body.Length > 1024)
          {
              body = body.Substring(0, 1024);
          }

          var headers = context.Request.Headers
                               .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                               .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                               .ToArray<string>();

          var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

          return "request:"   + context.Variables["message-id"] + "\n"
                              + requestLine + headerString + "\r\n" + body;
      }
  </log-to-eventhub>
  </inbound>
  <backend>
      <forward-request follow-redirects="true" />
  </backend>
  <outbound>
      <log-to-eventhub logger-id="conferencelogger" partition-id="1">
      @{
          var statusLine = string.Format("HTTP/1.1 {0} {1}\r\n",
                                              context.Response.StatusCode,
                                              context.Response.StatusReason);

          var body = context.Response.Body?.As<string>(true);
          if (body != null && body.Length > 1024)
          {
              body = body.Substring(0, 1024);
          }

          var headers = context.Response.Headers
                                          .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                          .ToArray<string>();

          var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

          return "response:"  + context.Variables["message-id"] + "\n"
                              + statusLine + headerString + "\r\n" + body;
     }
  </log-to-eventhub>
  </outbound>
</policies>
```

`set-variable` İlke, her ikisi için de erişilebilir olan bir değer oluşturur `log-to-eventhub` ilkesinde `<inbound>` bölüm ve `<outbound>` bölümü.  

## <a name="receiving-events-from-event-hubs"></a>Event Hubs'a ait alma olaylarını
Azure Event Hub olaylarından kullanarak alınan [AMQP protokolünü](http://www.amqp.org/). Microsoft Service Bus ekibine istemci kitaplıkları Süren olayları kolaylaştırmak kullanılabilir yaptınız. Desteklenen iki farklı yaklaşım vardır, biri yükleniyor bir *doğrudan tüketici* ve diğer kullanarak `EventProcessorHost` sınıfı. Bu iki yaklaşım örnekleri bulunabilir [Event Hubs Programlama Kılavuzu](../event-hubs/event-hubs-programming-guide.md). Kısa farklar sürümüdür, `Direct Consumer` tam denetim verir ve `EventProcessorHost` ancak bu olayları nasıl işleyecek hakkında bazı varsayımlarda bulunur tesisat iş bazıları desteklemez.  

### <a name="eventprocessorhost"></a>EventProcessorHost
Bu örnekte, kullanacağız `EventProcessorHost` kolaylık sağlamak için ancak bunu olabilir en iyi seçenek belirli bu senaryo için değil. `EventProcessorHost`iş parçacığı oluşturma sorunları belirli olay işlemcisi sınıfı içinde hakkında endişelenmeniz gerekmez emin olmak zor bir iş yapar. Ancak, Senaryomuzda biz yalnızca ileti başka bir biçime dönüştürme ve onu boyunca bir zaman uyumsuz yöntem kullanarak başka bir hizmete geçirmeden. Paylaşılan durum ve iş parçacığı oluşturma sorunları, bu nedenle hiçbir risk güncelleştirmek için gerek yoktur. Çoğu senaryo için `EventProcessorHost` büyük olasılıkla en iyi seçenek, kesinlikle daha kolay seçeneği ise.     

### <a name="ieventprocessor"></a>Ieventprocessor
Kullanırken merkezi kavram `EventProcessorHost` oluşturmaktır bir uygulaması `IEventProcessor` yöntemi içeren arabirimi `ProcessEventAsync`. Bu yöntem özünü burada gösterilir:

```c#
async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
{

   foreach (EventData eventData in messages)
   {
       _Logger.LogInfo(string.Format("Event received from partition: {0} - {1}", context.Lease.PartitionId,eventData.PartitionKey));

       try
       {
           var httpMessage = HttpMessage.Parse(eventData.GetBodyStream());
           await _MessageContentProcessor.ProcessHttpMessage(httpMessage);
       }
       catch (Exception ex)
       {
           _Logger.LogError(ex.Message);
       }
   }
    ... checkpointing code snipped ...
}
```

EventData nesnelerin bir listesini, yönteme geçirilir ve biz bu liste yineleme. Her yöntem bayt HttpMessage nesnesine Ayrıştırılan ve söz konusu nesne IHttpMessageProcessor örneğine geçirilir.

### <a name="httpmessage"></a>HttpMessage
`HttpMessage` Örnek veri üç parçalarını içerir:

```c#
public class HttpMessage
{
   public Guid MessageId { get; set; }
   public bool IsRequest { get; set; }
   public HttpRequestMessage HttpRequestMessage { get; set; }
   public HttpResponseMessage HttpResponseMessage { get; set; }

... parsing code snipped ...

}
```

`HttpMessage` Örneğini içeren bir `MessageId` bize karşılık gelen HTTP yanıtı ve nesne örneğini bir HttpRequestMessage ve httpresponsemessage öğesini içeriyorsa tanımlayan bir Boole değeri HTTP isteği bağlanmasını sağlayan GUID. HTTP sınıflardan yerleşik kullanılarak `System.Net.Http`, ı yararlanmak kurtararak `application/http` dahil kod ayrıştırma `System.Net.Http.Formatting`.  

### <a name="ihttpmessageprocessor"></a>IHttpMessageProcessor
`HttpMessage` Örnek uygulaması için sonra iletilen `IHttpMessageProcessor` oluşturduğum alma ve Azure olay hub'ı olayından yorumu ve gerçek işlenmesini aynı şekilde bir arabirim olduğu.

## <a name="forwarding-the-http-message"></a>HTTP ileti iletme
Bu örnek için onu olacaktır için üzerinden HTTP isteği göndermek ilginç ı karar [Runscope](http://www.runscope.com). Runscope hata ayıklama, günlüğe kaydetme ve izleme HTTP uzmanlaşmış bulut tabanlı bir hizmettir. Deneyin kolaydır ve gerçek zamanlı bizim API Management hizmeti aracılığıyla akan HTTP istek bkz kurmamızı sağlayan ücretsiz bir katman sahiptirler.

`IHttpMessageProcessor` Uygulaması şu şekilde görünür

```c#
public class RunscopeHttpMessageProcessor : IHttpMessageProcessor
{
   private HttpClient _HttpClient;
   private ILogger _Logger;
   private string _BucketKey;
   public RunscopeHttpMessageProcessor(HttpClient httpClient, ILogger logger)
   {
       _HttpClient = httpClient;
       var key = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-KEY", EnvironmentVariableTarget.User);
       _HttpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("bearer", key);
       _HttpClient.BaseAddress = new Uri("https://api.runscope.com");
       _BucketKey = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-BUCKET", EnvironmentVariableTarget.User);
       _Logger = logger;
   }

   public async Task ProcessHttpMessage(HttpMessage message)
   {
       var runscopeMessage = new RunscopeMessage()
       {
           UniqueIdentifier = message.MessageId
       };

       if (message.IsRequest)
       {
           _Logger.LogInfo("Sending HTTP request " + message.MessageId.ToString());
           runscopeMessage.Request = await RunscopeRequest.CreateFromAsync(message.HttpRequestMessage);
       }
       else
       {
           _Logger.LogInfo("Sending HTTP response " + message.MessageId.ToString());
           runscopeMessage.Response = await RunscopeResponse.CreateFromAsync(message.HttpResponseMessage);
       }

       var messagesLink = new MessagesLink() { Method = HttpMethod.Post };
       messagesLink.BucketKey = _BucketKey;
       messagesLink.RunscopeMessage = runscopeMessage;
       var runscopeResponse = await _HttpClient.SendAsync(messagesLink.CreateRequest());
       _Logger.LogDebug("Request sent to Runscope");
   }
}
```

I yararlanmak için bir [Runscope için var olan istemci Kitaplığı](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) , anında iletme kolaylaştırır `HttpRequestMessage` ve `HttpResponseMessage` kendi hizmetinde örnekleri ayarlama. Runscope API erişmek için bir hesap ve bir API anahtarı gerekir. Bir API anahtarı alma yönergeleri bulunabilir [erişim Runscope API uygulamaları oluşturma](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) ekran kaydı.

## <a name="complete-sample"></a>Tam örnek
[Kaynak kodu](https://github.com/darrelmiller/ApimEventProcessor) ve testleri örnek için GitHub üzerinde. İhtiyacınız olacak bir [API Management hizmeti](api-management-get-started.md), [bağlı bir olay hub '](api-management-howto-log-event-hubs.md)ve bir [depolama hesabı](../storage/common/storage-create-storage-account.md) örneği kendiniz çalıştırmak için.   

Örnek olay Hub'ından gelen olayların dönüştürür için bunlara dinler yalnızca basit bir konsol uygulaması olan bir `HttpRequestMessage` ve `HttpResponseMessage` nesneleri ve bunları açın Runscope API iletir.

Animasyonlu aşağıdaki görüntüde, işlenen iletilen ve alınan ileti gösteren konsol uygulaması ve ardından istek ve yanıt Runscope trafiği Denetçisi'nde gösteren Geliştirici Portalı'nda bir API için yapılan bir istek görebilirsiniz.

![İstek için Runscope iletilen Tanıtımı](./media/api-management-log-to-eventhub-sample/apim-eventhub-runscope.gif)

## <a name="summary"></a>Özet
Azure API Management hizmeti, Apı'lerinizi gelen ve giden seyahatte HTTP trafiğini yakalamak için ideal bir yer sağlar. Azure Event Hubs bu trafiği yakalamak için bir yüksek oranda ölçeklenebilir ve düşük maliyetli çözümdür ve günlüğe kaydetme için ikincil işleme sistemleri içine besleme, izleme ve diğer gelişmiş analiz. Runscope basit bir düzine birkaç kod olduğu gibi sistemleri izleme 3 taraf trafiği bağlanılıyor.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Event Hubs hakkında daha fazla bilgi edinin
  * [Azure Event Hubs ile çalışmaya başlama](../event-hubs/event-hubs-c-getstarted-send.md)
  * [EventProcessorHost bulunan iletiler alma](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [Event Hubs programlama kılavuzu](../event-hubs/event-hubs-programming-guide.md)
* API Management ve Event Hubs ile tümleştirme hakkında daha fazla bilgi edinin
  * [Azure Event hubs'a Azure API Management'te olayları günlüğe kaydetme hakkında](api-management-howto-log-event-hubs.md)
  * [Günlükçü varlık başvurusu](https://msdn.microsoft.com/library/azure/mt592020.aspx)
  * [Günlük eventhub ilke başvurusu](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
