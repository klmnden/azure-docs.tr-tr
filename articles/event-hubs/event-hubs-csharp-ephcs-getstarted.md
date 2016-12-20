---
title: "C# Dilinde Event Hubs Kullanmaya Başlayın | Microsoft Belgeleri"
description: "Azure Event Hubs&quot;ı C# ve Event Processor Host ile kullanmaya başlamak için bu öğreticiyi izleyin."
services: event-hubs
documentationcenter: 
author: jtaubensee
manager: timlt
editor: 
ms.assetid: 2ec2378a-34f7-44c3-b976-cc444c98c338
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/07/2016
ms.author: jotaub;sethm
translationtype: Human Translation
ms.sourcegitcommit: 015dbab81f807cd05d1fedb97f5fc62a012f2084
ms.openlocfilehash: 54aaf68864fc83501b860ecc3daef1c0216c0140


---
# <a name="get-started-with-event-hubs"></a>Event Hubs kullanmaya başlayın
[!INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Giriş
Event Hubs bağlı cihaz ve uygulamalardan büyük miktarlarda olay verileri (telemetri) işleyen bir hizmettir. Verileri Event Hubs’a topladıktan sonra bir depolama kümesi kullanarak depolayabilir veya gerçek zamanlı bir analiz sağlayıcısı kullanarak dönüştürebilirsiniz. Bu büyük ölçekli olay toplama ve işleme özelliği, Nesnelerin İnterneti (IoT) gibi modern uygulama mimarilerinin temel bir bileşenidir.

Bu öğretici, Azure portalının bir Olay Hub'ı oluşturmak için nasıl kullanılacağını gösterir. Ayrıca C# dilinde yazılmış bir konsol uygulaması kullanarak iletilerin bir Olay Hub'ına toplanması ve C# [Olay İşleyicisi Konağı][Event Processor Host] kitaplığı kullanılarak bunların paralel olarak alınması gösterilmektedir.

Bu öğreticiyi tamamlamak için şunlar gerekir:

* [Microsoft Visual Studio](http://visualstudio.com)
* Etkin bir Azure hesabı. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir hesap oluşturabilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme](https://azure.microsoft.com/free/).

[!INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[!INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[!INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## <a name="run-the-applications"></a>Uygulamaları çalıştırma
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
8. F5 tuşuna basarak **Alıcı** projesini Visual Studio içinden çalıştırın, ardından tüm bölümler için alıcıları başlatmasını bekleyin.
   
   ![][21]
9. **Gönderen** projesi otomatik olarak çalıştırılır. Konsol penceresinde **Enter** tuşuna basın ve alıcı penceresinde hangi olayların göründüğüne bakın.
   
   ![][22]

**Gönderen** penceresinde **Ctrl+C** tuşlarına basarak Gönderen uygulamasını sonlandırın, ardından Alıcı penceresinde **Enter** tuşuna basarak bu uygulamayı kapatın.

## <a name="next-steps"></a>Sonraki adımlar
Event Hub'ı oluşturan ve veri gönderip alan çalışan bir uygulama oluşturduğunuza göre aşağıdaki senaryolara geçebilirsiniz:

* [Event Hubs kullanan bir örnek uygulamanın][sample application that uses Event Hubs] tamamı.
* [Olay İşleyicisi Konağı](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost)
* [Event Hubs ile Olay İşleme Ölçeğini Artırma][Scale out Event Processing with Event Hubs] örneği.
* [Event Hubs'a genel bakış][Event Hubs overview]

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[Event Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Event Hubs overview]: event-hubs-overview.md
[sample application that uses Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[queued messaging solution]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md




<!--HONumber=Dec16_HO1-->


