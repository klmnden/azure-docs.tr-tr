---
title: Azure Service Bus - Event Grid tümleştirmesi örnekleri | Microsoft Docs
description: Service Bus mesajlaşma ve Event Grid tümleştirmesi örnekleri
services: service-bus-messaging
documentationcenter: .net
author: ChristianWolf42
manager: timlt
editor: ''
ms.assetid: f99766cb-8f4b-4baf-b061-4b1e2ae570e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 02/15/2018
ms.author: chwolf
ms.openlocfilehash: 3819a274696762861fbe76a9684b8495f1724f6a
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="azure-service-bus-to-azure-event-grid-examples"></a>Azure Service Bus - Azure Event Grid örnekleri

Bu belgede, Event Grid’den gelen bir olayın alınmasına dayalı iletiler alan bir mantıksal uygulamanın ve Azure işlevlerinin nasıl ayarlanacağını öğreneceksiniz. Örnekte, iki aboneliği olan bir Service Bus konusu olduğu ve yalnızca bir Service Bus aboneliğine ilişkin olaylar göndermek için bir şekilde Event Grid aboneliği oluşturulduğu varsayılır. Daha sonra Service Bus konusuna iletiler gönderir, bu Service Bus aboneliği için olayın oluşturulduğunu ve işlev veya mantıksal uygulamanın o Service Bus aboneliğinden iletiler aldığını doğrular ve işlemi tamamlarsınız.

* Lütfen önce tüm [Önkoşulların](#prerequisites) mevcut olduğundan emin olun.
* Hata ayıklamaya ve Event Grid’den olayların ilk akışını görmeye ilişkin [basit bir test Azure İşlevi](#test-function-setup) oluşturun.  **3. veya 4. adımın yürütülüp yürütülmediğine bakılmaksızın bu adım uygulanmalıdır.**
* Event Grid olaylarını temel alarak [Service Bus iletilerini almak ve işlemek için bir Azure İşlevi](#receive-messages-using-azure-function) oluşturun.
* [Logic Apps](#receive-messages-using-azure-logic-app)’i kullanın.

## <a name="prerequisites"></a>Ön koşullar

### <a name="service-bus-namespace"></a>Service Bus Ad Alanı

Bir Service Bus Premium Ad Alanı oluşturun. İki aboneliği olan bir Service Bus Konusu oluşturun.

### <a name="code-to-send-message-to-the-service-bus-topic"></a>Service Bus konusuna İleti Göndermek için kod

Service Bus konusuna ileti göndermek için herhangi bir yöntemi kullanabilirsiniz. Alternatif olarak aşağıdaki örneği kullanabilirsiniz. Örnek kod, Visual Studio 2017 kullandığınızı varsayar.

[Bu GitHub deposunu](https://github.com/Azure/azure-service-bus/) kopyalayın.

Şu klasöre gidin:

\samples\DotNet\Microsoft.ServiceBus.Messaging\ServiceBusEventGridIntegration ve şu dosyayı açın: SBEventGridIntegration.sln.

Ardından MessageSender projesine gidin ve Program.cs’yi açın.

![8][]

Konu adınızı ve bağlantı dizenizi doldurup konsol uygulamasını yürütün:

```CSharp
const string ServiceBusConnectionString = "YOUR CONNECTION STRING";
const string TopicName = "YOUR TOPIC NAME";
```

## <a name="test-function-setup"></a>Test işlevi kurulumu

Uçtan uca senaryonun tamamı üzerinde çalışmadan önce, hata ayıklamak ve hangi olayların akışa alındığını görmek için kullanabileceğiniz en az bir küçük test işlevine sahip olmak istersiniz.

Portalda yeni bir Azure İşlevi Uygulaması oluşturun. Azure İşlevleri’nin temel bilgilerini öğrenmek için şu [bağlantıyı](https://docs.microsoft.com/en-us/azure/azure-functions/) izleyin.

Yeni oluşturduğunuz işlevde bir http tetikleyici işlevi eklemek için küçük artı işaretine tıklayın:

![2][]

![3][]

Ardından işleve şu kodu kopyalayın:

```CSharp
#r "Newtonsoft.Json"
using System.Net;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");
    // parse query parameter
    var content = req.Content;

    string jsonContent = await content.ReadAsStringAsync(); 
    log.Info($"Received Event with payload: {jsonContent}");

IEnumerable<string> headerValues;
if (req.Headers.TryGetValues("Aeg-Event-Type", out headerValues))
{
var validationHeaderValue = headerValues.FirstOrDefault();
if(validationHeaderValue == "SubscriptionValidation")
{
var events = JsonConvert.DeserializeObject<GridEvent[]>(jsonContent);
     var code = events[0].Data["validationCode"];
     return req.CreateResponse(HttpStatusCode.OK,
     new { validationResponse = code });
}
}

    return jsonContent == null
    ? req.CreateResponse(HttpStatusCode.BadRequest, "Pass a name on the query string or in the request body")
    : req.CreateResponse(HttpStatusCode.OK, "Hello " + jsonContent);
}

public class GridEvent
{
    public string Id { get; set; }
    public string EventType { get; set; }
    public string Subject { get; set; }
    public DateTime EventTime { get; set; }
    public Dictionary<string, string> Data { get; set; }
    public string Topic { get; set; }
}
```

Kaydet ve çalıştır’a tıklayın.

## <a name="connect-function-and-namespace-via-event-grid"></a>Event Grid Aracılığıyla İşlev ve Ad Alanını Bağlama

Bir sonraki adım, işlevi ve Service Bus ad alanını birbirine bağlamaktır. Bu örnek için Azure portalını kullanın. Aynı şeyi elde etmek amacıyla PowerShell veya Azure CLI’nın nasıl kullanılacağını anlamak için [kavramlar](service-bus-to-event-grid-integration-concept.md) sayfasına bakın.

Yeni bir Azure Event Grid aboneliği oluşturmak için, Azure portalındaki ad alanınıza gidin ve Event Grid kamasını seçin. “+ Olay Aboneliği”ne tıklayın.

Ekran görüntüsünün ardından, önceden birkaç Event Grid aboneliği olan bir ad alanı gösterilir.

![20][]

Ekran görüntüsünün ardından, belirli bir filtreleme olmadan bir Azure İşlevine veya Web Kancasına nasıl abone olunacağı gösterilir. Service Bus aboneliğiniz için "Sonek Filtresi" olarak ilgili filtreyi eklemeyi unutmayın:

![21][]

Önkoşullarda belirtildiği gibi Service Bus konunuza bir ileti gönderin ve Azure İşlevinin İzleme özelliği aracılığıyla olayların akışa alındığını doğrulayın.

![9][]

### <a name="receive-messages-using-azure-function"></a>Azure İşlevini kullanarak ileti alma

Önceki bölümde, basit bir test ve hata ayıklama senaryosuna baktınız ve olayların akışa alındığından emin oldunuz. Bu bölümdeyse, bir olay aldıktan sonra iletileri nasıl alacağınıza ve işleyeceğinize bakacaksınız.

Aşağıdaki şekilde bir Azure İşlevi eklenmesinin nedeni, Azure İşlevleri içindeki Service Bus İşlevlerinin henüz yerel olarak yeni Event Grid tümleştirmesini desteklemiyor oluşudur.

Önkoşullarda açtığınız Visual Studio Çözümünde ReceiveMessagesOnEvent.cs öğesini seçin. Koda bağlantı dizenizi girin:

![10][]

```Csharp
const string ServiceBusConnectionString = "YOUR CONNECTION STRING";
```

Ardından Azure portalına gidin ve daha önce [2. Test işlevi kurulumu](#2-test-function-setup) bölümünde oluşturduğunuz Azure İşlevi için yayımlama profilini indirin.

![11][]

Daha sonra Visual Studio'da SBEventGridIntegration öğesine sağ tıklayın ve Yayımla’yı seçin. Daha önceden indirdiğiniz yayımlama profilini kullanın, içeri aktarma profilini seçin ve Yayımla'ya tıklayın.

![12][]

Yeni Azure işlevini yayımladıktan sonra yeni Azure İşlevini işaret eden yeni bir Azure Event Grid Aboneliği oluşturun. Doğru “İle biter” filtresini uyguladığınızdan emin olun. Bu filtre, Service Bus Aboneliği Adınız olmalıdır.

Ardından, daha önce oluşturduğunuz Azure Service Bus konusuna bir ileti gönderin ve portaldaki Azure İşlevi günlüğünde, olayların akışa alınıp alınmadığını ve iletilerin alınıp alınmadığını inceleyin.

![12-1][]

### <a name="receive-messages-using-azure-logic-app"></a>Azure Logic App kullanarak ileti alma

Aşağıdaki yönergeler, Azure Service Bus ve Azure Event Grid ile birlikte bir Azure Logic App’e nasıl bağlanacağınızı göstermektedir:

İlk olarak, Azure portalında yeni bir Mantıksal Uygulama oluşturun ve başlangıç eylemi olarak Event Grid’i seçin.

![13][]

Logic Apps tasarımcısında başlangıç görünümü aşağıdaki Ekran Görüntüsüne benzer şekilde görünmelidir. Ancak “Kaynak Adı”nı kendi ad alanı adınızla değiştirmeniz gerekir. Ayrıca gelişmiş seçenekleri genişlettiğinizden ve aboneliğiniz için sonek filtresini eklediğinizden de emin olun:

![14][]

Ardından bir konu aboneliğinden alınacak bir hizmet veri yolu Alma eylemi ekleyin. Son eylem şu Ekran Görüntüsüne benzer şekilde görünmelidir.

![15][]

Ardından bir tamamlanma olayı ekleyin. Olay aşağıdaki gibi görünmelidir.

![16][]

Mantıksal uygulamayı kaydedin ve önkoşullarda belirtildiği gibi Service Bus konunuza bir ileti gönderin. Ardından Mantıksal uygulamaların yürütülmesini gözlemleyin. "Genel Bakış" seçeneğine ve sonra da "Çalıştırma geçmişi"ne tıklarsanız yürütme ile ilgili daha fazla veri de alabilirsiniz.

![17][]

![18][]

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/) hakkında daha fazla bilgi edinin.
* [Azure İşlevleri](https://docs.microsoft.com/azure/azure-functions/) hakkında daha fazla bilgi edinin.
* [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/) hakkında daha fazla bilgi edinin.
* [Azure Service Bus](https://docs.microsoft.com/azure/service-bus/) hakkında daha fazla bilgi edinin.

[2]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid2.png
[3]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid3.png
[7]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid7.png
[8]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid8.png
[9]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid9.png
[10]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid10.png
[11]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid11.png
[12]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid12.png
[12-1]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid12-1.png
[12-2]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid12-2.png
[13]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid13.png
[14]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid14.png
[15]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid15.png
[16]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid16.png
[17]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid17.png
[18]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid18.png
[20]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal.png
[21]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal2.png
