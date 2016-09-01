<properties
    pageTitle="Azure Event Hubs nedir? | Microsoft Azure"
    description="Azure Event Hubs’a genel bakış ve açıklama"
    services="event-hubs"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/17/2016"
    ms.author="sethm"/>

# Azure Event Hubs nedir?

Azure Event Hubs, bağlı cihazlarınız ve uygulamalarınız tarafından üretilen oldukça büyük miktardaki verileri işleyip analiz edebilmeniz için saniye başına milyonlarca olayı işleyebilen ileri düzeyde ölçeklenebilir bir veri alım sistemidir. Event Hubs bir olay komut zincirinin "ön kapısı" olarak görev yapar ve veriler bir Event Hub'ına toplandıktan sonra herhangi bir gerçek zamanlı analiz sağlayıcısı veya toplu işlem/depolama bağdaştırıcısı kullanılarak dönüştürülebilir ve depolanabilir. Event Hubs olay akışı üretimlerini bu olayların tüketilmesinden ayırır, böylece olay tüketicileri olaylara kendi zamanlamalarında erişebilir. Daha fazla bilgi ve teknik ayrıntılar için bkz. [Event Hubs’a genel bakış](event-hubs-overview.md).

## Event Hubs özellikleri

Event Hubs, düşük gecikme süresi ve yüksek güvenilirlik sunan, büyük ölçekte olay ve telemetri işlenmesini sağlayan bir olay işleme hizmetidir. Bu hizmet özellikle aşağıdakiler için yararlıdır:

- Uygulama izleme
- Kullanıcı deneyimi veya iş akışı işleme
- Nesnelerin İnterneti (IoT) senaryoları

Diğer temel Event Hubs özelliklerinden bazıları ise mobil uygulamalarda davranış işleme, web gruplarından trafik bilgileri, konsol oyunlarında oyun içi olay yakalama veya sanayi makinelerinden ya da bağlı taşıtlardan toplanan telemetri verileridir.

Event Hubs [Service Bus kuyrukları ve konularından](../service-bus/service-bus-messaging-overview.md) farklı olarak ölçekli mesajlaşma akışı işlemesi sunmaya odaklanır. Event Hubs’ın özellikleri, güçlü şekilde yüksek verimlilik ve olay işleme senaryolarına eğilimli olmasıyla Service Bus konularından ayrılır. Sonuç olarak, Event Hubs [konular](../service-bus/service-bus-fundamentals-hybrid-solutions.md#topics) için mevcut olan bazı mesajlaşma özelliklerini kullanılmaz. Bu özellikler gerekli olursa hala en uygun seçenek konulardır.

## Sonraki adımlar

Event Hubs hakkında ayrıntılı bilgi için aşağıdaki konulara bakın.

- [Event Hubs’a genel bakış](event-hubs-overview.md)
- [Event Hubs programlama kılavuzu](event-hubs-programming-guide.md)
- [Event Hubs kullanılabilirliği ve destek SSS](event-hubs-availability-and-support-faq.md)
- [Event Hubs öğreticisi][] ile çalışmaya başlayın
- [Event Hubs kullanan bir örnek uygulamanın][] tamamı.

[Event Hubs öğreticisi]: event-hubs-csharp-ephcs-getstarted.md
[Event Hubs kullanan bir örnek uygulamanın]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097



<!--HONumber=Aug16_HO4-->


