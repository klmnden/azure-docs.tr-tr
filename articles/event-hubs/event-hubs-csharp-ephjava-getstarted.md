---
title: "C# Dilinde Event Hubs Kullanmaya Başlayın | Microsoft Belgeleri"
description: "Azure Event Hubs’ı kullanmaya başlamak, olayları C# dilinde gönderip EventProcessorHost kullanarak Java dilinde almak için bu öğreticiyi izleyin."
services: event-hubs
documentationcenter: 
author: jtaubensee
manager: timlt
editor: 
ms.assetid: 059fb733-a397-400e-8e43-0c7ea5930b8b
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 09/27/2016
ms.author: jotaub;sethm
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 51d880650ab8059f3346b5c1272c232b49be33e9


---
# <a name="get-started-with-event-hubs"></a>Event Hubs kullanmaya başlayın
[!INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Giriş
Event Hubs bağlı cihaz ve uygulamalardan büyük miktarlarda olay verileri (telemetri) işleyen bir hizmettir. Verileri Event Hubs’a topladıktan sonra bir depolama kümesi kullanarak depolayabilir veya gerçek zamanlı bir analiz sağlayıcısı kullanarak dönüştürebilirsiniz. Bu büyük ölçekli olay toplama ve işleme özelliği, Nesnelerin İnterneti (IoT) gibi modern uygulama mimarilerinin temel bir bileşenidir.

Bu öğretici, klasik Azure portalının bir Event Hub'ı oluşturmak için nasıl kullanılacağını gösterir. Ayrıca C# dilinde yazılmış bir konsol uygulaması kullanarak iletilerin bir Event Hub'ına toplanması ve Java Olay İşleyicisi Konağı kitaplığı kullanılarak bunların paralel olarak alınması gösterilmektedir.

Bu öğreticiyi tamamlamak için şunlar gerekir:

* [Microsoft Visual Studio](http://visualstudio.com)
* Etkin bir Azure hesabı. <br/>Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir hesap oluşturabilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F target="_blank").

[!INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[!INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[!INCLUDE [service-bus-event-hubs-get-started-receive-ephjava](../../includes/service-bus-event-hubs-get-started-receive-ephjava.md)]

## <a name="run-the-applications"></a>Uygulamaları çalıştırma
Şimdi uygulamaları çalıştırmaya hazırsınız.

1. **Alıcı** projesini çalıştırın.
   
   ![][21]
2. **Gönderen** projesini çalıştırın.
   
   ![][22]

## <a name="next-steps"></a>Sonraki adımlar
Event Hub'ı oluşturan ve veri gönderip alan çalışan bir uygulama oluşturduğunuza göre aşağıdaki senaryolara geçebilirsiniz:

* Eksiksiz bir [Event Hubs kullanan örnek uygulama][Event Hubs kullanan örnek uygulama].
* [Event Hubs ile Olay İşleme Ölçeğini Artırma][Event Hubs ile Olay İşleme Ölçeğini Artırma] örneği.
* [Event Hubs’a genel bakış][Event Hubs’a genel bakış]

<!-- Images. -->
[21]: ./media/event-hubs-csharp-ephjava-getstarted/ephjava.png
[22]: ./media/event-hubs-csharp-ephjava-getstarted/cs-send.png

<!-- Links -->
[Klasik Azure portalı]: https://manage.windowsazure.com/
[Event Hubs’a genel bakış]: event-hubs-overview.md
[Event Hubs kullanan örnek uygulama]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Event Hubs ile Olay İşleme Ölçeğini Artırma]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3



<!--HONumber=Nov16_HO2-->


