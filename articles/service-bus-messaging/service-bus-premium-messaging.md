---
title: Azure Service Bus Premium ve Standart Mesajlaşma hizmeti fiyatlandırma katmanlarına genel bakış | Microsoft Docs
description: Service Bus Premium ve Standart Mesajlaşma katmanları
services: service-bus-messaging
documentationcenter: .net
author: djrosanova
manager: timlt
editor: ''
ms.assetid: e211774d-821c-4d79-8563-57472d746c58
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/10/2017
ms.author: sethm
ms.openlocfilehash: cf750f451351f729296991499f233b235b27a5e7
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="service-bus-premium-and-standard-messaging-tiers"></a>Service Bus Premium ve Standart Mesajlaşma katmanları

Kuyruklar ve konu başlıkları gibi varlıkları içeren Service Bus Mesajlaşma, kuruluşun mesajlaşma işlevlerini bulut ölçeğinde zengin yayımla-abone ol semantiği ile birleştirir. Service Bus Mesajlaşması birçok gelişmiş bulut çözümü için iletişimin temel öğesi olarak kullanılır.

Service Bus Mesajlaşma hizmetinin *Premium* katmanı, görev açısından kritik uygulamalar için ölçek, performans ve kullanılabilirlik bağlamında yaygın müşteri isteklerini karşılar. Özellikler kümeleri neredeyse aynı olsa da, Service Bus Mesajlaşma hizmetinin bu iki katmanı farklı kullanım durumlarına göre tasarlanmıştır.

Aşağıdaki tabloda bazı üst düzey farklılıklar vurgulanmıştır.

| Premium | Standart |
| --- | --- |
| Yüksek verimlilik |Değişken işleme |
| Tahmin edilebilir performans |Değişken gecikme süresi |
| Sabit fiyatlandırma |Kullandıkça Öde değişken fiyatlandırması |
| İş yükünün ölçeğini artırma veya azaltma |Yok |
| İleti boyutu 1 MB’a kadar |İleti boyutu 256 KB’a kadar |

**Service Bus Premium Mesajlaşma Hizmeti**, CPU'da ve bellek düzeyinde kaynak yalıtımına olanak sağladığından her müşterinin iş yükü yalıtımlı şekilde çalışır. Bu kaynak kapsayıcısı *mesajlaşma birimi* olarak adlandırılır. Her premium ad alanı, en az bir mesajlaşma birimi için ayrılmıştır. Her Service Bus Premium ad alanı için 1, 2 veya 4 mesajlaşma birimi satın alabilirsiniz. Tek bir iş yükü veya varlık, birden çok mesajlaşma birimine yayılabilir ve faturalandırma 24 saatlik veya günlük oran fiyatlarında gerçekleştirilse de mesajlaşma birimlerinin sayısı isteğe bağlı olarak değiştirilebilir. Sonuç olarak, Service Bus tabanlı çözümünüz için tahmin edilebilir ve tekrarlanabilir bir performans elde edersiniz.

Daha tahmin edilebilir ve kullanılabilir olmasının yanı sıra bu performans, daha hızlıdır. Service Bus Premium Mesajlaşma hizmeti, [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) kısmında tanıtılan depolama motorunda derlenir. Premium Mesajlaşma sayesinde, en yüksek performans Standart katmanda olduğundan daha hızlıdır.

## <a name="premium-messaging-technical-differences"></a>Premium Mesajlaşmanın teknik farklılıkları

Aşağıdaki bölümlerde Premium ve Standart mesajlaşma katmanları arasındaki bazı farklar ele alınmaktadır.

### <a name="partitioned-queues-and-topics"></a>Bölümlenmiş kuyruklar ve konular

Bölümlenmiş kuyruklar ve konular, Premium Mesajlaşma hizmetinde desteklenir; aslında bu varlıklar her zaman bölümlenmiş durumdadır (ve devre dışı bırakılamaz). Ancak, Premium bölümlenmiş kuyruklar ve konular Service Bus mesajlaşma hizmetinin Standart katmanında aynı şekilde işlev görmez. Premium mesajlaşma, SQL'i bir veri deposu olarak kullanmaz ve artık paylaşılan platforma ilişkin olası kaynak rekabetini barındırmaz. Sonuç olarak, performansı iyileştirmek için bölümleme gerekli değildir. Ayrıca, Standart Mesajlaşmada 16 olan bölüm sayısı Premium’da 2 bölüm olarak değiştirilmiştir. İki bölümlemeye sahip olmak kullanılabilirliği garanti altına alır ve Premium çalışma zamanı ortamı için daha uygun bir sayıdır. 

Premium mesajlaşmada, [MaxSizeInMegabytes](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxsizeinmegabytes#Microsoft_ServiceBus_Messaging_QueueDescription_MaxSizeInMegabytes) ile bir varlığın boyutunu belirttiğinizde, toplam boyutun belirtilen boyuttan 16 kat fazla olduğu [Standart bölümlü varlıkların](service-bus-partitioning.md#standard) aksine bu boyut, 2 bölüm arasında eşit olarak bölünür. 

Bölümleme hakkında daha fazla bilgi için bkz. [Bölümlenmiş kuyruklar ve konular](service-bus-partitioning.md).

### <a name="express-entities"></a>İfade varlıkları

Premium mesajlaşma tamamen yalıtılmış bir çalışma zamanı ortamında çalıştığından Premium ad alanlarında ifade varlıkları desteklenmez. İfade özellikleri hakkında daha fazla bilgi için [QueueDescription.EnableExpress](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enableexpress#Microsoft_ServiceBus_Messaging_QueueDescription_EnableExpress) özelliğine bakın.

Standart mesajlaşma altında çalışan bir kodunuz varsa ve Premium katmanı ile bağlantı noktası oluşturmak istiyorsanız, [EnableExpress](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enableexpress#Microsoft_ServiceBus_Messaging_QueueDescription_EnableExpress) özelliğinin **false** (varsayılan değer) olarak ayarlandığından emin olun.

## <a name="get-started-with-premium-messaging"></a>Premium Mesajlaşmayı kullanmaya başlama

Premium Mesajlaşma ile çalışmaya başlamak kolaydır ve süreç Standart Mesajlaşma ile benzerlik gösterir. [Azure Portal](https://portal.azure.com)'da [ad alanı oluşturarak](service-bus-create-namespace-portal.md) başlayın. **Fiyatlandırma katmanınızı seçin** alanında **Premium**'u seçtiğinizden emin olun.

![create-premium-namespace][create-premium-namespace]

Ayrıca [Azure Resource Manager şablonlarını kullanarak premium ad alanları](https://azure.microsoft.com/resources/templates/101-servicebus-pn-ar/) oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus Mesajlaşma hizmeti hakkında daha fazla bilgi edinmek için aşağıdaki konu başlıklarına bakın.

* [Azure Service Bus Mesajlaşma hizmetine giriş (blog gönderisi)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [Azure Service Bus Premium Mesajlaşma hizmetine giriş (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [Service Bus Mesajlaşma hizmetine genel bakış](service-bus-messaging-overview.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)

<!--Image references-->

[create-premium-namespace]: ./media/service-bus-premium-messaging/select-premium-tier.png
