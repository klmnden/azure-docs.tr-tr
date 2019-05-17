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
ms.topic: tutorial
ms.date: 05/14/2019
ms.author: spelluru
ms.openlocfilehash: b29798bb87b7c5c677e7d80e552e45e8d1290541
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65787007"
---
# <a name="respond-to-azure-service-bus-events-received-via-azure-event-grid-by-using-azure-functions-and-azure-logic-apps"></a>Azure işlevleri ve Azure Logic Apps'ı kullanarak Azure Event Grid alınan Azure Service Bus olaylarını yanıtlama
Bu öğreticide, Azure işlevleri ve Azure Logic Apps'ı kullanarak Azure Event Grid alınan Azure Service Bus olaylara yanıt verme konusunda bilgi edinin. Aşağıdaki adımları gerçekleştirin:
 
- Hata ayıklamaya ve Event grid'den olayların ilk akışını görüntülemeye ilişkin bir test Azure işlevi oluşturun.
- Event Grid olaylarını temel alarak Azure Service Bus iletileri almak ve işlemek için bir Azure işlevi oluşturun.
- Event Grid olaylarına yanıt vermek için bir mantıksal uygulama oluşturma

Service Bus, Event Grid, Azure işlevleri ve Logic Apps yapıtları oluşturduktan sonra aşağıdaki eylemleri gerçekleştirin: 

1. Bir Service Bus konusuna iletiler gönderir. 
2. Konuya abonelik bu iletileri aldınız emin olun
3. Olay için abone işlev veya mantıksal uygulama olay aldığını doğrulayın. 

## <a name="create-a-service-bus-namespace"></a>Service Bus ad alanı oluşturma
Bu öğreticide yönergeleri izleyin: [Hızlı Başlangıç: Bir Service Bus konusu ve konu için Abonelik oluşturmak için Azure portal'ı kullanmanızı](service-bus-quickstart-topics-subscriptions-portal.md) aşağıdaki görevleri gerçekleştirmek için:

- Oluşturma bir **premium** Service Bus ad alanı. 
- Bağlantı dizesini alın. 
- Bir Service Bus konusu oluşturun.
- İki konuya abonelik oluşturun. 

## <a name="prepare-a-sample-application-to-send-messages"></a>İleti göndermek için örnek uygulamayı hazırlama
Service Bus konunuza ileti göndermek için herhangi bir yöntemi kullanabilirsiniz. Bu yordamın sonundaki örnek kod, Visual Studio 2017'yi kullandığınız varsayılır.

1. [GitHub azure-service-bus deposunu](https://github.com/Azure/azure-service-bus/) kopyalayın.
2. Visual Studio’da *\samples\DotNet\Microsoft.ServiceBus.Messaging\ServiceBusEventGridIntegration* klasörüne gidin ve *SBEventGridIntegration.sln* dosyasını açın.
3. **MessageSender** projesine gidin ve **Program.cs** öğesini seçin.
4. Service Bus konu başlığı adınız ve önceki adımda aldığınız bağlantı dizesi girin:

    ```CSharp
    const string ServiceBusConnectionString = "YOUR CONNECTION STRING";
    const string TopicName = "YOUR TOPIC NAME";
    ```
5. Derleme ve Service Bus konu başlığına sınama iletileri göndermek için programı çalıştırın. 

## <a name="set-up-a-test-function-on-azure"></a>Azure üzerinde bir test işlevi ayarlama 
Senaryonun tamamı çalışmadan önce hata ayıklama ve akışa olayları gözlemektir için kullanabileceğiniz en az bir küçük test işlevi ayarlayın. Yönergeleri [Azure portalında ilk işlevinizi oluşturma](../azure-functions/functions-create-first-azure-function.md) makale aşağıdaki görevleri gerçekleştirmek için: 

1. Bir işlev uygulaması oluşturun.
2. HTTP ile tetiklenen bir işlev oluşturun. 

Ardından, aşağıdaki adımları uygulayın: 


# <a name="azure-functions-v2tabv2"></a>[Azure işlevler V2](#tab/v2)

1. Genişletin **işlevleri** ağaç görünümünü ve işlevinizi seçin. İşlev kodunu aşağıdaki kodla değiştirin: 

    ```CSharp
    #r "Newtonsoft.Json"
    
    using System.Net;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Primitives;
    using Newtonsoft.Json;
    
    public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
    {
        log.LogInformation("C# HTTP trigger function processed a request.");
        var content = req.Body;
        string jsonContent = await new StreamReader(content).ReadToEndAsync();
        log.LogInformation($"Received Event with payload: {jsonContent}");
    
        IEnumerable<string> headerValues;
        headerValues = req.Headers.GetCommaSeparatedValues("Aeg-Event-Type");
    
        if (headerValues.Count() != 0)
        {
            var validationHeaderValue = headerValues.FirstOrDefault();
            if(validationHeaderValue == "SubscriptionValidation")
            {
                var events = JsonConvert.DeserializeObject<GridEvent[]>(jsonContent);
                var code = events[0].Data["validationCode"];
                log.LogInformation("Validation code: {code}");
                return (ActionResult) new OkObjectResult(new { validationResponse = code });
            }
        }
    
        return jsonContent == null
            ? new BadRequestObjectResult("Please pass a name on the query string or in the request body")
            : (ActionResult)new OkObjectResult($"Hello, {jsonContent}");
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
2. **Kaydet ve çalıştır**’ı seçin.

    ![İşlevi uygulama çıktısı](./media/service-bus-to-event-grid-integration-example/function-run-output.png)
3. Seçin **işlev URL'sini Al** Not URL'sini alın. 

    ![İşlev URL'sini al](./media/service-bus-to-event-grid-integration-example/get-function-url.png)

# <a name="azure-functions-v1tabv1"></a>[Azure işlevleri V1](#tab/v1)

1. Kullanılacak işlev yapılandırma **V1** sürümü: 
    1. İşlev uygulamanızı ağaç görünümünde seçip **işlev uygulaması ayarları**. 

        ![İşlev uygulaması ayarları]()./media/service-bus-to-event-grid-integration-example/function-app-settings.png)
    2. Seçin **yaklaşık 1** için **çalışma zamanı sürümü**. 
2. Genişletin **işlevleri** ağaç görünümünü ve işlevinizi seçin. İşlev kodunu aşağıdaki kodla değiştirin: 

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
4. **Kaydet ve çalıştır**’ı seçin.

    ![İşlevi uygulama çıktısı](./media/service-bus-to-event-grid-integration-example/function-run-output.png)
4. Seçin **işlev URL'sini Al** Not URL'sini alın. 

    ![İşlev URL'sini al](./media/service-bus-to-event-grid-integration-example/get-function-url.png)

---

## <a name="connect-the-function-and-namespace-via-event-grid"></a>Event Grid aracılığıyla işlev ve ad alanını bağlama
Bu bölümde, işlevi ve Service Bus ad alanı Azure portalını kullanarak birbirine. 

Azure Event Grid aboneliği oluşturmak için aşağıdaki adımları izleyin:

1. Azure portalında ad alanınıza gidin ve ardından, sol bölmede **olayları**. Ad alanı pencereniz açılır ve sağ bölmede iki Event Grid aboneliği görüntülenir. 
    
    ![Service Bus - olayları sayfası](./media/service-bus-to-event-grid-integration-example/service-bus-events-page.png)
2. Seçin **+ olay aboneliği** araç. 
3. Üzerinde **olay aboneliği oluşturma** sayfasında, aşağıdaki adımları uygulayın:
    1. Girin bir **adı** abonelik için. 
    2. Seçin **Web kancası** için **uç noktası türü**. 

        ![Service Bus - Event Grid aboneliği](./media/service-bus-to-event-grid-integration-example/event-grid-subscription-page.png)
    3. Seçtiğiniz **bir uç nokta seçin**işlev URL'sini yapıştırın ve ardından **seçimi onaylamanız**. 

        ![Function - uç noktayı seçin](./media/service-bus-to-event-grid-integration-example/function-select-endpoint.png)
    4. Geçiş **filtreleri** sekmesinde, adı girin **ilk abonelik** Service Bus konu başlığına önceki ve ardından oluşturduğunuz **Oluştur** düğmesi. 

        ![Olay abonelik filtresi](./media/service-bus-to-event-grid-integration-example/event-subscription-filter.png)
4. Olay aboneliği listesinde gördüğünüzü onaylayın.

    ![Olay aboneliği listesinde](./media/service-bus-to-event-grid-integration-example/event-subscription-in-list.png)

## <a name="send-messages-to-the-service-bus-topic"></a>Service Bus konusuna iletiler gönderir
1. .NET çalışan C# uygulama, Service Bus konusuna iletiler gönderir. 

    ![Konsol uygulama çıktısı](./media/service-bus-to-event-grid-integration-example/console-app-output.png)
1. Azure işlev uygulamanız için bir sayfada genişletin **işlevleri**, genişletin, **işlevi**seçip **İzleyici**. 

    ![İzleyici işlevi](./media/service-bus-to-event-grid-integration-example/function-monitor.png)

## <a name="receive-messages-by-using-azure-functions"></a>Azure İşlevleri’ni kullanarak ileti alma
Önceki bölümde, basit bir test ve hata ayıklama senaryosuna baktınız ve olayların akışa alındığından emin oldunuz. 

Bu bölümde, bir olay aldıktan sonra nasıl ileti alınacağını ve işleneceğini öğreneceksiniz.

### <a name="publish-a-function-from-visual-studio"></a>Bir işlev Visual Studio'dan yayımlama
1. Aynı Visual Studio çözümünde (**Sbeventgridıntegration**) açtığınız, seçin **ReceiveMessagesOnEvent.cs** içinde **Sbeventgridıntegration** proje. 
2. Service Bus bağlantı dizenizi şu kodu girin:

    ```Csharp
    const string ServiceBusConnectionString = "YOUR CONNECTION STRING";
    ```
3. İndirme **yayımlama profilini** işlevi için:
    1. İşlev uygulamanızı seçin. 
    2. Seçin **genel bakış** zaten seçili değilse, sekme. 
    3. Seçin **yayımlama profili Al** araç. 

        ![Yayımlama profili işlevi için Al](./media/service-bus-to-event-grid-integration-example/function-download-publish-profile.png)
    4. Dosyayı proje klasörüne kaydedin. 
4. Visual Studio’da **SBEventGridIntegration** öğesine sağ tıklayın ve **Yayımla**’yı seçin. 
5. Seçin *Başlat** üzerinde **Yayımla** sayfası. 
6. Üzerinde **yayımlama hedefi seçin** sayfasında aşağıdaki adımları uygulayın, **profilini içeri aktarma**. 

    ![Visual Studio - profili İçeri Aktar düğmesi](./media/service-bus-to-event-grid-integration-example/visual-studio-import-profile-button.png)
7. Seçin **yayımlama profili dosyası** daha önce indirdiğiniz. 
8. Seçin **Yayımla** üzerinde **Yayımla** sayfası. 

    ![Visual Studio - yayımlama](./media/service-bus-to-event-grid-integration-example/select-publish.png)
9. Yeni Azure işlevini gördüğünüzü onaylayın **ReceiveMessagesOnEvent**. Gerekirse, sayfayı yenileyin. 

    ![Yeni işlev oluşturulduğunu onaylayın](./media/service-bus-to-event-grid-integration-example/function-receive-messages.png)
10. Yeni işlev URL'sini alın ve bunu not alın. 

### <a name="event-grid-subscription"></a>Event Grid aboneliği

1. Var olan Event Grid aboneliği silin:
    1. Üzerinde **Service Bus Namespace** sayfasında **olayları** sol menüsünde. 
    2. Mevcut bir olay aboneliği seçin. 
    3. Üzerinde **olay aboneliği** sayfasında **Sil**.
2. Yönergeleri [işlev ve ad alanı Event Grid aracılığıyla bağlama](#connect-the-function-and-namespace-via-event-grid) yeni işlev URL'sini kullanarak bir Event Grid aboneliği oluşturmak için bölümü.
3. Verilen yönergeleri izleyin [Service Bus konu başlığına ileti gönderme](#send-messages-to-the-service-bus-topic) konusuna iletiler gönderir ve işlev izleme bölümü. 

## <a name="receive-messages-by-using-logic-apps"></a>Logic Apps’i kullanarak ileti alma
Mantıksal uygulama, aşağıdaki adımları izleyerek Azure Service Bus ve Azure Event Grid ile bağlanın:

1. Azure portalında bir mantıksal uygulama oluşturun.
    1. Seçin **+ kaynak Oluştur**seçin **tümleştirme**ve ardından **mantıksal uygulama**. 
    2. Üzerinde **- mantıksal uygulama oluşturma** want bir **adı** mantıksal uygulama için.
    3. Azure **aboneliğinizi** seçin. 
    4. Seçin **var olanı kullan** için **kaynak grubu**, daha önce oluşturduğunuz için diğer kaynaklar (örneğin, Azure işlevi, Service Bus ad alanı) kullandığınız kaynak grubunu seçin. 
    5. Seçin **konumu** mantıksal uygulama için. 
    6. Seçin **Oluştur** mantıksal uygulamanızı oluşturmak için. 
2. Üzerinde **Logic Apps Tasarımcısı'nda** sayfasında **boş mantıksal uygulama** altında **şablonları**. 
3. Tasarımcıda, aşağıdaki adımları uygulayın:
    1. Arama **Event grid'i**. 
    2. Seçin **kaynak olayı Azure Event Grid (Önizleme) - gerçekleştiğinde**. 

        ![Logic Apps Tasarımcısı - Event Grid tetikleyiciyi seçin](./media/service-bus-to-event-grid-integration-example/logic-apps-event-grid-trigger.png)
4. Seçin **oturum**, Azure kimlik bilgilerinizi girin ve seçin **erişime izin ver**. 
5. Üzerinde **kaynak olayı gerçekleştiğinde** sayfasında, aşağıdaki adımları uygulayın:
    1. Azure aboneliğinizi seçin. 
    2. İçin **kaynak türü**seçin **Microsoft.ServiceBus.Namespaces**. 
    3. İçin **kaynak adı**, Service Bus ad alanınızı seçin. 
    4. Seçin **yeni parametre Ekle**seçip **sonek filtresi**. 
    5. İçin **sonek filtresi**, ikinci, Service Bus konu aboneliği adı girin. 

        ![Logic Apps Tasarımcısı - olay yapılandırın](./media/service-bus-to-event-grid-integration-example/logic-app-configure-event.png)
6. Seçin **+ yeni adım** Tasarımcısı'nda ve aşağıdaki adımları uygulayın:
    1. Arama **Service Bus**.
    2. Seçin **Service Bus** listesinde. 
    3. Seçim için **iletileri alma** içinde **eylemleri** listesi. 
    4. Seçin **bir konu aboneliğinden (gözlem kilidi) iletileri alma**. 

        ![Logic Apps Tasarımcısı - get iletileri eylemi](./media/service-bus-to-event-grid-integration-example/service-bus-get-messages-step.png)
    5. Girin bir **bağlantı adı**. Örneğin: **Bir konu aboneliğinden iletileri alma**ve Service Bus ad alanını seçin. 

        ![Logic Apps Tasarımcısı - Service Bus ad alanını seçin](./media/service-bus-to-event-grid-integration-example/logic-apps-select-namespace.png) 
    6. Seçin **RootManageSharedAccessKey**.

        ![Logic Apps Tasarımcısı - paylaşılan erişim anahtarı seçin](./media/service-bus-to-event-grid-integration-example/logic-app-shared-access-key.png) 
    7. **Oluştur**’u seçin. 
    8. Konu ve abonelik seçin. 
    
        ![Logic Apps Tasarımcısı - Service Bus konu ve abonelik seçme](./media/service-bus-to-event-grid-integration-example/logic-app-select-topic-subscription.png)
7. Seçin **+ yeni adım**, ve aşağıdaki adımları uygulayın: 
    1. **Service Bus**'ı seçin.
    2. Seçin **konu aboneliğindeki iletiyi Tamamla** eylemler listesinden. 
    3. Service Bus seçin **konu**.
    4. İkinci seçin **abonelik** konuya.
    5. İçin **iletinin kilit belirteci**seçin **kilit belirteci** gelen **dinamik içerik**. 

        ![Logic Apps Tasarımcısı - Service Bus konu ve abonelik seçme](./media/service-bus-to-event-grid-integration-example/logic-app-complete-message.png)
8. Seçin **Kaydet** mantıksal uygulamayı kaydetmek için Logic Apps Tasarımcısı araç çubuğunda. 
9. Verilen yönergeleri izleyin [Service Bus konu başlığına ileti gönderme](#send-messages-to-the-service-bus-topic) konuya ileti göndermek için bölüm. 
10. Geçiş **genel bakış** mantıksal uygulamanızın sayfası. Mantıksal uygulama çalıştırmaları gördüğünüz **çalıştırma geçmişi** gönderilen iletiler için.

    ![Logic Apps Tasarımcısı - mantıksal uygulama çalıştırmaları](./media/service-bus-to-event-grid-integration-example/logic-app-runs.png)

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
