---
title: Azure API Management, Event Hubs ve Runscope ile API'leri izleme | Microsoft Docs
description: Bağlanan Azure API Management, Azure Event Hubs ve Runscope HTTP günlüğe kaydetme ve izleme için günlük eventhub ilkeyle gösteren örnek uygulaması
services: api-management
documentationcenter: ''
author: darrelmiller
manager: erikre
editor: ''
ms.assetid: c528cf6f-5f16-4a06-beea-fa1207541a47
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2018
ms.author: apimpm
ms.openlocfilehash: 3a868eb98121ff2e2a30657e301afba7b8618361
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2019
ms.locfileid: "55728489"
---
# <a name="monitor-your-apis-with-azure-api-management-event-hubs-and-runscope"></a>Azure API Management, Event Hubs ve Runscope ile Apı'lerinizi izleme
[API Management hizmeti](api-management-key-concepts.md) HTTP API'nize gönderilen HTTP isteklerinin işlenmesini geliştirmek için çok sayıda özellik sağlar. Ancak istek ve yanıtların varlığını geçici olabilir. İstek yapıldığında ve arka uç API'niz için API Management hizmeti aracılığıyla akar. API isteği işler ve API tüketiciye yanıt geriye doğru akar. API Management hizmeti görüntülemek için API'ler ile ilgili bazı önemli istatistikleri Azure portal panosunda ancak ötesine ayrıntılarını kaldırılmıştır, tutar.

API Management hizmetinde günlük eventhub İlkesi kullanarak istek ve yanıt olarak herhangi bir ayrıntıyı gönderebilirsiniz bir [Azure olay hub'ı](../event-hubs/event-hubs-what-is-event-hubs.md). Çeşitli nedenlerle Apı'leriniz için gönderilen HTTP iletileri olayları oluşturmak isteyebilirsiniz neden vardır. Denetim izi'ni güncelleştirmeleri, kullanım analizi, özel durum uyarı ve üçüncü taraf entegrasyonlara buna örnek verilebilir.

Bu makalede, tüm HTTP istek ve yanıt iletisi yakalama, olay Hub'ına gönderir ve sonra bu iletiyi HTTP günlüğe kaydetme ve izleme hizmetleri sağlayan bir üçüncü taraf hizmetine geçiş gösterilmektedir.

## <a name="why-send-from-api-management-service"></a>API Management hizmetinden neden gönderilsin mi?
HTTP isteklerini ve yanıtlarını yakalayın ve günlüğe kaydetme ve izleme sistemlerinden içine akışı HTTP API'si çerçeveleri takın HTTP ara yazılım yazmak mümkündür. Bu yaklaşımın bir dezavantajı, HTTP ara yazılımı arka uç API tümleşik gerekir ve API'nin platform eşleşmelidir ' dir. Birden çok API varsa, her bir ara yazılım dağıtmanız gerekir. Genellikle arka uç API'leri neden güncelleştirilemiyor neden vardır.

Günlük kaydı altyapısı ile tümleştirmek için Azure API Management hizmeti kullanarak merkezi ve platformdan bağımsız bir çözüm sağlar. Aynı zamanda ölçeklenebilir, kısmen verilecek [coğrafi çoğaltma](api-management-howto-deploy-multi-region.md) Azure API yönetimi özellikleri.

## <a name="why-send-to-an-azure-event-hub"></a>Neden Azure olay Hub'ına gönderilsin mi?
Bu sorun, neden Azure Event Hubs için özel bir ilke oluşturmak makul mi? Burada isteklerim oturum isteyebilirsiniz birçok farklı yer vardır. Neden yalnızca istekleri doğrudan son hedefe gönderilsin mi?  Bir seçenektir. Ancak, bir API management hizmeti günlük kaydı istekleri yaparken, günlük iletilerini API performansı nasıl etkiler dikkate alınması gereken gereklidir. Yük aşamalı bir artış sistem bileşenleri kullanılabilir örneklerini artırarak veya coğrafi çoğaltma yararlanarak işlenebilir. Ancak, kısa ani trafik günlüğü altyapı isteklerine yük altındayken yavaş başlatırsanız Gecikmeli isteklerine neden olabilir.

Azure Event Hubs, çoğu API işlemi sayısı, HTTP istekleri daha olayların çok daha yüksek bir sayı ile ilgilenen kapasite ile giriş muazzam miktarlarda veri için tasarlanmıştır. Olay hub'ı, bir API Yönetimi hizmetiniz ve depolar ve iletileri işleyen altyapınız arasında karmaşık arabellek olarak görev yapar. Bu API performansınızı nedeniyle günlük altyapısı görmeyecektir sağlar.

Olay Hub'ına veri üzere geçtikten sonra kalıcı hale getirilir ve işlem sırasında olay hub'ı Tüketiciler için bekler. Olay hub'ı nasıl işleneceğini ilgilenmez, yalnızca ileti başarıyla teslim emin olma hakkında önemser.

Event Hubs olay akışı için birden fazla tüketici grupları için özelliğine sahiptir. Bu, farklı sistemleri tarafından işlenmesi olayları sağlar. Bu, yalnızca bir olay oluşturulması gereken şekilde ek gecikmelere API Management hizmetinde API isteğinin işlenmesi koyarak olmadan birçok tümleştirme senaryolarına destek sağlar.

## <a name="a-policy-to-send-applicationhttp-messages"></a>Uygulama/http ileti göndermek için bir ilke
Bir olay hub'ı olay verilerinde basit bir dize olarak kabul eder. Bu dizenin içeriği en fazla olursunuz. Bir HTTP isteği paketini ve Event Hubs'a gönderme için biçim dizesine istek veya yanıtı bilgilerle ihtiyacımız var. Bu gibi durumlarda bir varolan biçimi ise yeniden kullanabilir ve ardından biz kendi kod ayrıştırmayı yazılacak olmayabilir. Başlangıçta miyim kullanarak kabul [HAR](http://www.softwareishard.com/blog/har-12-spec/) HTTP istekleri ve yanıtları göndermek için. Ancak, bu biçim, bir dizi HTTP isteklerini JSON tabanlı bir biçimde depolamak için optimize edilmiştir. Bu gereksiz karmaşıklık, kablo üzerinden HTTP ileti geçirme senaryosu için eklenen zorunlu öğeleri bir dizi içeriyor.

Alternatif bir seçenek kullanılmasıydır `application/http` medya türü HTTP Belirtimi'nde açıklanan [RFC 7230](https://tools.ietf.org/html/rfc7230). Bu ortam türünü gerçekten kablo üzerinden HTTP iletileri göndermek için kullanılan tam aynı biçimi kullanır, ancak tüm ileti başka bir HTTP istek gövdesinde put olabilir. Bu örnekte, yalnızca gövdesi bizim iletisi olarak Event Hubs'a göndermek için kullanılacak kullanacağız. Rahat, var olan bir ayrıştırıcı yoktur [Microsoft ASP.NET Web API 2.2 istemci](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) Bu biçim ayrıştırabilir ve yerel dönüştürmek kitaplıkları `HttpRequestMessage` ve `HttpResponseMessage` nesneleri.

Bu ileti oluşturabilmek için C# temel avantajlarından yararlanmak ihtiyacımız [ilke ifadeleri](https://msdn.microsoft.com/library/azure/dn910913.aspx) Azure API Management. Azure Event Hubs için bir HTTP isteği iletisi gönderir ilkeyi, aşağıda verilmiştir.

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
Var. Bu ilke ifadesi hakkında bahseden değer birkaç özel nokta. Günlük eventhub ilkesinin, API Management hizmetinde oluşturulan bir Günlükçü adına başvuran Günlükçü-id adlı bir öznitelik vardır. API Management hizmetinde bir olay hub'ı Günlükçü kurma ayrıntılarını belgede bulunabilir [olayları Azure Event Hubs, Azure API Yönetimi'nde oturum nasıl](api-management-howto-log-event-hubs.md). İkinci öznitelik iletide depolamak için bölüm Event Hubs yönlendiren isteğe bağlı bir parametredir. Event Hubs bölümleri ölçeklenebilirlik etkinleştirmek ve en az iki gerektiren için kullanır. İletilerin sıralı teslim yalnızca bir bölüm içinde garanti edilir. Biz olay Hub'ında ileti yerleştirmek için hangi bölümünün isteyin değil, yükü dağıtmak için hepsini bir kez deneme algoritması kullanır. Ancak, bazı sıralamaya işlenecek bizim iletileri neden olabilir.

### <a name="partitions"></a>Bölümler
Bizim ileti tüketiciler sırayla teslim edilir ve bölüm yükü dağıtım özellikten faydalanmak sağlamak için HTTP isteği iletilerini bir bölüm ve HTTP yanıt iletilerini ikinci bir bölüme göndermek seçtim. Tüm istekleri sırayla tüketilir ve tüm yanıtları sırayla tüketilir ediyoruz ve bu bir bile yük dağıtımı sağlar. Yanıt karşılık gelen bir istekten önce kullanılması mümkündür, ancak sahip olduğumuz gibi bir sorun olmadığından ilişkilendirmek için farklı bir mekanizma isteklerini yanıtlar ve istekleri her zaman önce yanıtları geldiğini biliyoruz.

### <a name="http-payloads"></a>HTTP yükler
Yapı sonra `requestLine`, biz istek gövdesi kesilmiş olmadığını denetleyin. İstek gövdesi için yalnızca 1024 kesilir. Tek bir olay hub'ına ileti 256 KB ile sınırlı ancak bu, artırılacak biçimde HTTP ileti tek bir iletiye sığmayacak yok edecek gövdeleri emin olma olasılığı yüksektir. Günlüğe kaydetme ve analiz yaparken yalnızca HTTP isteği çizgi ve üst bilgi önemli miktarda türetilebilir. Ayrıca, çok sayıda API istek yalnızca dönüş küçük gövdeleri ve bu nedenle büyük gövdeleri kesiliyor tarafından bilgi değer kaybı aktarımı, işleme ve depolama maliyetlerini tüm gövde içeriği tutmak azalma kıyasla oldukça az. Gövdesinin işlenmesi hakkında son bir not olduğunu geçirilecek ihtiyacımız `true` için `As<string>()` yöntemi çünkü biz gövde içeriğini okuma ancak olsa da arka uç API gövdesini okumak için istiyordu. Geçirerek bu yöntem için true, biz ikinci kez okuyabilmesini arabelleğe alınan gövdesi neden. Bu, daha büyük dosyaları karşıya yükleme veya uzun yoklama kullanan bir API'niz varsa bilmeniz önemlidir. Bu durumda, gövdesi hiç okuma önlemek en iyi olacaktır.

### <a name="http-headers"></a>HTTP üstbilgileri
HTTP üstbilgileri üzerinden basit bir anahtar/değer çifti biçimi iletisi biçiminde içine aktarılabilir. Gereksiz yere kimlik bilgilerini sızdırılmasını önlemek için belirli güvenlik duyarlı alanları, Şerit seçtik. API anahtarları ve diğer kimlik bilgileri analiz amacıyla kullanılacak düşüktür. Kullanıcı ve kullanıyordur belirli ürün analizlerini istediğimiz sonra aldığımız `context` nesne ve bu iletiye ekleyin.

### <a name="message-metadata"></a>İleti meta verileri
Olay hub'ına gönderilecek iletinin tamamını oluştururken, ilk satırı değil gerçekten parçası `application/http` ileti. İlk satır, iletinin bir istek veya yanıt iletisi ve yanıtlarını ilişkilendirmek için kullanılan kimliği, istekleri bir ileti olup oluşan ek meta verilerdir. İleti kimliği, şuna benzer başka bir ilke kullanılarak oluşturulur:

```xml
<set-variable name="message-id" value="@(Guid.NewGuid())" />
```

Biz istek iletisi oluşturulan, yanıt döndürdü ve istek ve yanıt tek bir ileti gönderilmesi kadar bir değişkende depolanır. Ancak, istek ve yanıt bağımsız olarak gönderme ve iki ilişkilendirmek için bir ileti kimliği'ni kullanarak, biraz daha fazla esneklik ileti boyutu, ileti sırası ve istek görünür yaparken birden çok bölüm yararlanmak olanağı aldığımız Bizim günlük panosunda daha çabuk. Ayrıca olabilir bazı senaryolar burada geçerli bir yanıt asla API Management hizmetinde önemli istek hatası nedeniyle büyük olasılıkla olay hub'ına gönderilir ancak hala bir kayıt isteğinin sahibiz.

HTTP yanıt iletisi göndermek için ilke isteği benzer ve bu nedenle tam bir ilke yapılandırmasını aşağıdaki gibi görünür:

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

`set-variable` İlkesi tarafından erişilebilir olan bir değer oluşturur `log-to-eventhub` ilkesinde `<inbound>` bölümü ve `<outbound>` bölümü.

## <a name="receiving-events-from-event-hubs"></a>Event Hubs'a ait alma olaylarını
Olayları Azure Event Hub'ından alınan kullanarak [AMQP protokolünü](https://www.amqp.org/). Microsoft Service Bus ekibine istemci kitaplıklarını kullanan olaylar kolaylaştırmak kullanılabilir yapıldı. Desteklenen iki farklı yaklaşım, bir okunuyor bir *doğrudan tüketici* ve diğer kullanarak `EventProcessorHost` sınıfı. Bu iki yaklaşımı örnekler bulunabilir [Event Hubs Programlama Kılavuzu](../event-hubs/event-hubs-programming-guide.md). Kısa farklar sürümüdür, `Direct Consumer` tam denetim verir ve `EventProcessorHost` , ancak bu olayları işleminin nasıl hakkında bazı varsayımlarda bulunur tesisat işinin bir kısmını desteklemez.

### <a name="eventprocessorhost"></a>EventProcessorHost
Bu örnekte, kullandığımız `EventProcessorHost` kolaylık sağlamak için ancak bunu olabilir bu belirli senaryo yok en iyi seçim. `EventProcessorHost` iş parçacığı oluşturma sorunları belirli olay işlemcisi sınıfı içinde hakkında endişelenmek zorunda olmadığınız emin olmak zor işi yapar. Ancak, senaryomuzdaki ise biz yalnızca ileti başka bir biçime dönüştürme ve zaman uyumsuz bir yöntem kullanarak başka bir hizmete boyunca iletmeden. Paylaşılan durum ve bu nedenle iş parçacığı oluşturma sorunları, risk güncelleştirme gerek yoktur. Çoğu senaryo için `EventProcessorHost` kesinlikle kolay seçenek olduğunu ve büyük olasılıkla en iyi seçenektir.

### <a name="ieventprocessor"></a>Ieventprocessor
Kullanırken yönetim kavramı `EventProcessorHost` uygulaması oluşturmaktır `IEventProcessor` yöntemi içeren arabirimi `ProcessEventAsync`. Bu yöntem özünü aşağıda gösterilmiştir:

```csharp
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

EventData nesnelerin bir listesini, yönteme geçirilir ve biz bu liste üzerinde yineleme. Her yöntemin bayt HttpMessage nesneyi Ayrıştırılan ve söz konusu nesne IHttpMessageProcessor örneğine geçirilir.

### <a name="httpmessage"></a>HttpMessage
`HttpMessage` Örneği verileri üç parça içerir:

```csharp
public class HttpMessage
{
    public Guid MessageId { get; set; }
    public bool IsRequest { get; set; }
    public HttpRequestMessage HttpRequestMessage { get; set; }
    public HttpResponseMessage HttpResponseMessage { get; set; }

... parsing code snipped ...

}
```

`HttpMessage` Örneği içeren bir `MessageId` karşılık gelen HTTP yanıtı ve nesne örneğini bir HttpRequestMessage ve HttpResponseMessage içeriyorsa tanımlayan bir Boole değeri HTTP isteği bağlanmasına olanak sağlayan bir GUID. Yerleşik HTTP kullanarak gelen sınıflar `System.Net.Http`, ı yararlanmak için `application/http` yer aldığı kod ayrıştırmayı `System.Net.Http.Formatting`.  

### <a name="ihttpmessageprocessor"></a>IHttpMessageProcessor
`HttpMessage` Örneği uygulanmasına ardından iletilen `IHttpMessageProcessor`, oluşturduğum alma ve Azure olay Hub'ından olay yorumu ve işlenmesini onu ayrıştırmak için bir arabirim olduğundan.

## <a name="forwarding-the-http-message"></a>HTTP ileti iletme
Bu örnek için ı olacağını için üzerinden HTTP isteği göndermek ilginç karar [Runscope](https://www.runscope.com). Runscope HTTP hata ayıklama, günlüğe kaydetme ve izleme konusunda uzmanlaşmış bir bulut tabanlı bir hizmettir. Denemek kolay bir işlemdir ve gerçek zamanlı API Yönetimi hizmetimiz giden HTTP isteklerini görmemizi sağlar, böylece bir ücretsiz katmanı ile sahiptirler.

`IHttpMessageProcessor` Uygulamasını şu şekilde görünür

```csharp
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

Ben yararlanmak için bir [Runscope mevcut istemci kitaplığının](https://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) , anında iletme kolaylaştırır `HttpRequestMessage` ve `HttpResponseMessage` kendi hizmetinde örnekler ayarlama. Runscope API'sine erişmek için bir hesap ve bir API anahtarı gerekir. Bir API anahtarı alma yönelik yönergeler bulunabilir [erişim Runscope API uygulamaları oluşturma](https://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) yayını.

## <a name="complete-sample"></a>Tam örnek
[Kaynak kodu](https://github.com/darrelmiller/ApimEventProcessor) ve testleri örnek için GitHub üzerinde. Gereksinim duyduğunuz bir [API Management hizmeti](get-started-create-service-instance.md), [bağlı bir olay hub'ı](api-management-howto-log-event-hubs.md)ve [depolama hesabı](../storage/common/storage-create-storage-account.md) kendiniz örneği çalıştırmak için.   

Örnek olay Hub'ından gelen olayların dönüştürür için bunları dinler yalnızca basit bir konsol uygulaması olan bir `HttpRequestMessage` ve `HttpResponseMessage` nesnelerini ve sonra bunları açın Runscope API iletir.

Animasyonlu aşağıdaki görüntüde, Geliştirici Portalı, iletisi alındı, işlenen iletilen ve gösteren konsol uygulaması ve ardından istek ve yanıt Runscope Traffic denetçi'deki gösteren bir API'ye yapılan bir istek görebilirsiniz.

![İstek için Runscope iletilen Tanıtımı](./media/api-management-log-to-eventhub-sample/apim-eventhub-runscope.gif)

## <a name="summary"></a>Özet
Azure API Management hizmeti, Apı'lerinizi gelen ve giden seyahat HTTP trafiği yakalamak için ideal bir yer sağlar. Azure Event Hubs, o trafiğini yakalamaktan ve günlüğe kaydetme, izleme ve diğer gelişmiş analiz için ikincil sistemlerin içine besleme için yüksek oranda ölçeklenebilen, düşük maliyetli bir çözümdür. İzleme sistemlerinden Runscope birkaç düzine satırlık kod basit gibi üçüncü taraf trafiği bağlanıyor.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Event Hubs hakkında daha fazla bilgi edinin
  * [Azure Event Hubs ile çalışmaya başlama](../event-hubs/event-hubs-c-getstarted-send.md)
  * [EventProcessorHost bulunan iletiler alma](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [Event Hubs programlama kılavuzu](../event-hubs/event-hubs-programming-guide.md)
* API Management ve olay hub'ları ile tümleştirme hakkında daha fazla bilgi edinin
  * [Azure Event hubs'a, Azure API Management'ta olayları günlüğe kaydetme hakkında](api-management-howto-log-event-hubs.md)
  * [Günlükçü varlık başvurusu](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity)
  * [Günlük eventhub ilke başvurusu](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
