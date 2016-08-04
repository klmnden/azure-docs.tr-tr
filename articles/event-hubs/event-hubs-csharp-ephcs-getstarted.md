<properties
    pageTitle="C# Dilinde Event Hubs Kullanmaya Başlayın | Microsoft Azure"
    description="Azure Event Hubs’ı C# ve EventProcessorHost ile kullanmaya başlamak için bu öğreticiyi izleyin."
    services="event-hubs"
    documentationCenter=""
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="05/13/2016"
    ms.author="sethm"/>

# Event Hubs kullanmaya başlayın

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## Giriş

Event Hubs bağlı cihaz ve uygulamalardan büyük miktarlarda olay verileri (telemetri) işleyen bir hizmettir. Verileri Event Hubs’a topladıktan sonra bir depolama kümesi kullanarak depolayabilir veya gerçek zamanlı bir analiz sağlayıcısı kullanarak dönüştürebilirsiniz. Bu büyük ölçekli olay toplama ve işleme özelliği, Nesnelerin İnterneti (IoT) dahil modern uygulama mimarilerinin temel bir bileşenidir.

Bu öğretici, klasik Azure portalının bir Event Hub'ı oluşturmak için nasıl kullanılacağını gösterir. Ayrıca C# dilinde yazılmış bir konsol uygulaması kullanarak iletilerin bir Event Hub'ına toplanması ve C# [Olay İşleyicisi Konağı][] kitaplığı kullanılarak bunların paralel olarak alınması gösterilmektedir.

Bu öğreticiyi tamamlamak için şunlar gerekir:

+ Microsoft Visual Studio 2013 veya üzeri ya da Windows için Microsoft Visual Studio Express. Bu makaledeki örnekler Visual Studio 2015’i kullanır.

+ Etkin bir Azure hesabı. <br/>Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir hesap oluşturabilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F target="_blank").

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]


[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Visual Studio’da daha önce oluşturduğunuz **Alıcı** projesini açın.

2. **Alıcı** çözümüne sağ tıklayın, ardından **Ekle** ve **Mevcut Proje**’ye tıklayın.
 
3. Var olan Sender.csproj dosyasını bulun ve sonra çözüme eklemek için çift tıklayın.
 
4. Yeniden, **Alıcı** çözümüne sağ tıklayın ve ardından **Özellikler**’e tıklayın. **Alıcı** özellik sayfası gösterilir.

5. **Başlangıç Projesi**'ne ve **Birden fazla başlangıç projesi** düğmesine tıklayın. Hem **Alıcı** hem de **Gönderen** projelerinin **Eylem** kutusunu **Başlat** olarak ayarlayın.

    ![][19]

6. **Proje Bağımlılıkları**'na tıklayın. **Projeler** kutusunda **Gönderen**’e tıklayın. **Bağımlıdır** kutusunda **Alıcı** öğesinin seçildiğinden emin olun.

    ![][20]

7. **Özellikler** iletişim kutusunu kapatmak için **Tamam**'a tıklayın.

1.  F5 tuşuna basarak **Alıcı** projesini Visual Studio içinden çalıştırın, ardından tüm bölümler için alıcıları başlatmasını bekleyin.

    ![][21]

2.  **Gönderen** projesi otomatik olarak çalıştırılır. Konsol penceresinde **Enter** tuşuna basın ve alıcı penceresinde hangi olayların göründüğüne bakın.

    ![][22]

**Gönderen** penceresinde **Ctrl+C** tuşlarına basarak Gönderen uygulamasını sonlandırın, ardından Alıcı penceresinde **Enter** tuşuna basarak bu uygulamayı kapatın.

## Sonraki adımlar

Event Hub'ı oluşturan ve veri gönderip alan çalışan bir uygulama oluşturduğunuza göre aşağıdaki senaryolara geçebilirsiniz:

- [Event Hubs kullanan bir örnek uygulamanın][] tamamı.
- [Event Hubs ile Olay İşleme Ölçeğini Artırma][] örneği.
- Service Bus kuyruklarını kullanan [kuyruğa alınan mesajlaşma çözümü][].
- [Event Hubs’a genel bakış][]

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[Klasik Azure portalı]: https://manage.windowsazure.com/
[Olay İşleyicisi Konağı]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Event Hubs’a genel bakış]: event-hubs-overview.md
[Event Hubs kullanan bir örnek uygulamanın]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Event Hubs ile Olay İşleme Ölçeğini Artırma]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[kuyruğa alınan mesajlaşma çözümü]: ../service-bus/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 



<!----HONumber=Jun16_HO2-->


