---
title: Azure Event Hubs nedir? | Microsoft Belgeleri
description: "Azure Event Hubs’a genel bakış ve açıklama"
services: event-hubs
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 4391d750-5bbe-456d-9091-b416127bc754
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/30/2016
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 05ca343cfdfc602759eb3ea30a7186a0bb47bd74
ms.openlocfilehash: eddc3e7c4914936a8a83a042dc0f7d528b91f059


---
# <a name="what-is-azure-event-hubs"></a>Azure Event Hubs nedir?
Azure Event Hubs, bağlı cihazlarınız ve uygulamalarınız tarafından üretilen oldukça büyük miktardaki verileri işleyip analiz edebilmeniz için saniye başına milyonlarca olayı işleyebilen ileri düzeyde ölçeklenebilir bir veri alım sistemidir. Event Hubs bir olay komut zincirinin "ön kapısı" olarak görev yapar ve veriler bir Event Hub'ına toplandıktan sonra herhangi bir gerçek zamanlı analiz sağlayıcısı veya toplu işlem/depolama bağdaştırıcısı kullanılarak dönüştürülebilir ve depolanabilir. Event Hubs olay akışı üretimlerini bu olayların tüketilmesinden ayırır, böylece olay tüketicileri olaylara kendi zamanlamalarında erişebilir. Daha fazla bilgi ve teknik ayrıntılar için bkz. [Event Hubs’a genel bakış](event-hubs-overview.md).

## <a name="event-hubs-capabilities"></a>Event Hubs özellikleri
Event Hubs, düşük gecikme süresi ve yüksek güvenilirlik sunan, büyük ölçekte olay ve telemetri işlenmesini sağlayan bir olay işleme hizmetidir. Bu hizmet özellikle aşağıdakiler için yararlıdır:

* Uygulama izleme
* Kullanıcı deneyimi veya iş akışı işleme
* Nesnelerin İnterneti (IoT) senaryoları

Diğer temel Event Hubs özelliklerinden bazıları ise mobil uygulamalarda davranış işleme, web gruplarından trafik bilgileri, konsol oyunlarında oyun içi olay yakalama veya sanayi makinelerinden ya da bağlı taşıtlardan toplanan telemetri verileridir.

## <a name="next-steps"></a>Sonraki adımlar
Event Hubs hakkında ayrıntılı bilgi için aşağıdaki konulara bakın.

* [Event Hubs’a genel bakış](event-hubs-overview.md)
* [Event Hubs programlama kılavuzu](event-hubs-programming-guide.md)
* [Event Hubs kullanılabilirliği ve destek SSS](event-hubs-availability-and-support-faq.md)
* [Event Hubs öğreticisi][Event Hubs tutorial] ile çalışmaya başlama
* [Event Hubs kullanan bir örnek uygulamanın][sample application that uses Event Hubs] tamamı

[Event Hubs tutorial]: event-hubs-csharp-ephcs-getstarted.md
[sample application that uses Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097



<!--HONumber=Dec16_HO1-->


