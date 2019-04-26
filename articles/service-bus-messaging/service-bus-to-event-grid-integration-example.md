---
title: Azure Service Bus - Event Grid tümleştirmesi örnekleri | Microsoft Docs
description: Bu makalede, Service Bus mesajlaşma ve Event Grid tümleştirmesi örnekleri sağlanmaktadır.
services: service-bus-messaging
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: f99766cb-8f4b-4baf-b061-4b1e2ae570e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/15/2018
ms.author: spelluru
ms.openlocfilehash: 4e1ea3d822c8b032617b7f202f1c176aeb966210
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60333416"
---
# <a name="azure-service-bus-to-azure-event-grid-integration-examples"></a>Azure Service Bus - Azure Event Grid tümleştirmesi örnekleri

Bu makalede, Azure Event Grid’den gelen bir olayın alınmasına dayalı iletiler alan bir Azure işlevi ve mantıksal uygulamanın nasıl ayarlanacağını öğreneceksiniz. Yapacaklarınız:
 
* Hata ayıklamaya ve Event grid'den olayların ilk akışını görüntülemeye ilişkin basit bir test Azure işlevi oluşturun. Diğer adımları gerçekleştirip gerçekleştirmediğinize bakmaksızın bu adımı gerçekleştirin.
* Event Grid olaylarını temel alarak Azure Service Bus iletileri almak ve işlemek için bir Azure işlevi oluşturun.
* Azure App Service'in Logic Apps özelliğini kullanın.

Oluşturduğunuz örnekte, Service Bus konusunun iki aboneliğinin olduğu varsayılır. Ayrıca bu örnekte yalnızca bir Service Bus aboneliğine yönelik olaylar göndermek için Event Grid aboneliğinin oluşturulduğu da varsayılır. 

Örnekte, Service Bus konusuna iletiler gönderir ve sonra bu Service Bus aboneliği için olayın oluşturulduğunu doğrularsınız. İşlev veya mantıksal uygulama, Service Bus aboneliğinden iletileri alır ve tamamlar.

## <a name="prerequisites"></a>Önkoşullar
Başlamadan önce, sonraki iki bölümde yer alan adımları tamamladığınızdan emin olun.

### <a name="create-a-service-bus-namespace"></a>Service Bus ad alanı oluşturma

Bir Service Bus Premium ad alanı oluşturun ve iki abonelik içeren bir Service Bus konusu oluşturun.

### <a name="send-a-message-to-the-service-bus-topic"></a>Service Bus konusuna ileti gönderme

Service Bus konunuza ileti göndermek için herhangi bir yöntemi kullanabilirsiniz. Bu yordamın sonundaki örnek kod, Visual Studio 2017 kullandığınızı varsayar.

1. [GitHub azure-service-bus deposunu](https://github.com/Azure/azure-service-bus/) kopyalayın.

1. Visual Studio’da *\samples\DotNet\Microsoft.ServiceBus.Messaging\ServiceBusEventGridIntegration* klasörüne gidin ve *SBEventGridIntegration.sln* dosyasını açın.

1. **MessageSender** projesine gidin ve **Program.cs** öğesini seçin.

   ![8][]

1. Konu adınızı ve bağlantı dizenizi doldurup aşağıdaki konsol uygulama kodunu yürütün:

    ```CSharp
    const string ServiceBusConnectionString = "YOUR CONNECTION STRING";
    const string TopicName = "YOUR TOPIC NAME";
    ```

## <a name="set-up-a-test-function"></a>Bir test işlevi ayarlama

Senaryonun tamamı üzerinde çalışmadan önce, hata ayıklamak ve hangi olayların akışa alındığını görmek için kullanabileceğiniz en az bir küçük test işlevi ayarlayın.

1. Azure portalında yeni bir Azure İşlevleri uygulaması oluşturun. Azure İşlevlerinin temel bilgilerini öğrenmek için [Azure İşlevleri belgelerine](https://docs.microsoft.com/azure/azure-functions/) bakın.

1. Yeni oluşturduğunuz işlevde artı işaretini (+) seçerek bir HTTP tetikleyici işlevi ekleyin:

    ![2][]
    
    **Önceden hazırlanmış bir işlev ile hızlıca çalışmaya başlayın** penceresi açılır.

    ![3][]

1. Sırayla **Web Kancası + API** düğmesini, **CSharp** öğesini seçin ve **Bu işlevi oluştur** seçeneğini belirleyin.
 
1. Aşağıdaki kodu işleve yapıştırın:

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

1. **Kaydet ve çalıştır**’ı seçin.

## <a name="connect-the-function-and-namespace-via-event-grid"></a>Event Grid aracılığıyla işlev ve ad alanını bağlama

Bu bölümde, işlevi ve Service Bus ad alanını birbirine bağlarsınız. Bu örnek için Azure portalını kullanın. Bu yordamı gerçekleştirmek için PowerShell veya Azure CLI’nın nasıl kullanılacağını anlamak için bkz. [Azure Service Bus - Azure Event Grid tümleştirmesine genel bakış](service-bus-to-event-grid-integration-concept.md).

Azure Event Grid aboneliği oluşturmak için aşağıdakileri yapın:
1. Azure portalında ad alanınıza gidin ve ardından sol bölmede **Event Grid**’i seçin.  
    Ad alanı pencereniz açılır ve sağ bölmede iki Event Grid aboneliği görüntülenir.

    ![20][]

1. **Olay Aboneliği**’ni seçin.  
    **Olay Aboneliği** penceresi açılır. Aşağıdaki resimde, filtreler uygulamadan bir Azure işlevine veya web kancasına abone olmanın bir formu görüntülenir.

    ![21][]

1. Gösterildiği gibi formu doldurun ve **Sonek Filtresi** kutusuna ilgili filtreyi girmeyi unutmayın.

1. **Oluştur**’u seçin.

1. "Önkoşullar" bölümünde belirtildiği gibi Service Bus konunuza bir ileti gönderin ve Azure İşlevleri İzleme özelliği aracılığıyla olayların akışa alındığını doğrulayın.

Bir sonraki adım, işlevi ve Service Bus ad alanını birbirine bağlamaktır. Bu örnek için Azure portalını kullanın. Bu adımı gerçekleştirmek için PowerShell veya Azure CLI’nın nasıl kullanılacağını anlamak için bkz. [Azure Service Bus - Azure Event Grid tümleştirmesine genel bakış](service-bus-to-event-grid-integration-concept.md).

![9][]

### <a name="receive-messages-by-using-azure-functions"></a>Azure İşlevleri’ni kullanarak ileti alma

Önceki bölümde, basit bir test ve hata ayıklama senaryosuna baktınız ve olayların akışa alındığından emin oldunuz. 

Bu bölümde, bir olay aldıktan sonra nasıl ileti alınacağını ve işleneceğini öğreneceksiniz.

Azure İşlevleri içindeki Service Bus işlevleri henüz yeni Event Grid tümleştirmesini yerel ortamda desteklemediğinden aşağıdaki örnekte gösterildiği gibi bir Azure işlevi eklersiniz.

1. Önkoşullarda açtığınız Visual Studio Çözümünde **ReceiveMessagesOnEvent.cs** öğesini seçin. 

    ![10][]

1. Aşağıdaki koda bağlantı dizenizi girin:

    ```Csharp
    const string ServiceBusConnectionString = "YOUR CONNECTION STRING";
    ```

1. Azure portalında, "Test işlevi ayarlama" bölümünde oluşturduğunuz Azure işlevi için yayımlama profilini indirin.

    ![11][]

1. Visual Studio’da **SBEventGridIntegration** öğesine sağ tıklayın ve **Yayımla**’yı seçin. 

1. Önceden indirdiğiniz yayımlama profilinin **Yayımla** bölmesinde **Profili içeri aktar**’ı ve sonra **Yayımla**’yı seçin.

    ![12][]

1. Yeni Azure işlevini yayımladıktan sonra, yeni Azure işlevini işaret eden yeni bir Azure Event Grid aboneliği oluşturun.  
    **İle biter** kutusunda, doğru filtreyi uyguladığınızdan emin olun. Bu, Service Bus abonelik adınız olmalıdır.

1. Daha önce oluşturduğunuz Azure Service Bus konusuna bir ileti gönderin ve olayların akışa alındığından ve iletilerin alındığından emin olmak için Azure portalında Azure İşlevleri günlüğünü izleyin.

    ![12-1][]

### <a name="receive-messages-by-using-logic-apps"></a>Logic Apps’i kullanarak ileti alma

Aşağıdakileri yaparak Azure Service Bus ve Azure Event Grid ile bir mantıksal uygulamayı bağlayın:

1. Azure portalında yeni bir mantıksal uygulama oluşturun ve başlangıç eylemi olarak **Event Grid**’i seçin.

    ![13][]

    Logic Apps tasarımcı penceresi açılır.

    ![14][]

1. Aşağıdakileri yaparak bilgilerinizi ekleyin:

    a. **Kaynak Adı** kutusuna kendi ad alanı adınızı girin. 

    b. **Gelişmiş seçenekler** bölümünde **Sonek Filtresi** kutusuna aboneliğiniz için filtre girin.

1. Bir konu aboneliğinden iletiler almak için Service Bus alma eylemi ekleyin.  
    Aşağıdaki resimde son eylem gösterilmektedir:

    ![15][]

1. Aşağıdaki resimde gösterildiği gibi tam bir olay ekleyin:

    ![16][]

1. Mantıksal uygulamayı kaydedin ve "Önkoşullar" bölümünde belirtildiği gibi Service Bus konunuza bir ileti gönderin.  
    Mantıksal uygulamanın yürütülmesini gözlemleyin. Yürütmeye ilişkin daha fazla veri görüntülemek için **Genel Bakış**’ı seçin ve **Çalıştırma geçmişi** bölümünde verileri görüntüleyin.

    ![17][]

    ![18][]

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/) hakkında daha fazla bilgi edinin.
* [Azure İşlevleri](https://docs.microsoft.com/azure/azure-functions/) hakkında daha fazla bilgi edinin.
* [Azure App Service’in Logic Apps özelliği](https://docs.microsoft.com/azure/logic-apps/) hakkında daha fazla bilgi edinin.
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
