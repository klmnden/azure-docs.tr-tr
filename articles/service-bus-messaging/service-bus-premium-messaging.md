<properties
    pageTitle="Service Bus Premium ve Standart Mesajlaşma hizmeti fiyatlandırma katmanlarına genel bakış | Microsoft Azure"
    description="Service Bus Premium ve Standart Mesajlaşma Hizmeti"
    services="service-bus"
    documentationCenter=".net"
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/02/2016"
    ms.author="darosa;sethm"/>


# <a name="service-bus-premium-and-standard-messaging-tiers"></a>Service Bus Premium ve Standart Mesajlaşma katmanları 

Kuyruklar ve konu başlıkları gibi mesajlaşma varlıklarını içeren Service Bus mesajlaşma, kuruluşun mesajlaşma işlevlerini bulut ölçeğinde zengin yayımla-abone ol semantiği ile birleştirir. Service Bus mesajlaşması birçok gelişmiş bulut çözümü için iletişimin temel öğesi olarak kullanılır.

Service Bus mesajlaşma hizmetinin *Premium* katmanı, görev açısından kritik uygulamalar için ölçek, performans ve kullanılabilirlik bağlamında yaygın müşteri isteklerini karşılar. Özellikler kümeleri neredeyse aynı olsa da, Service Bus mesajlaşma hizmetinin bu iki katmanı farklı kullanım durumlarına göre tasarlanmıştır.

Aşağıdaki tabloda bazı üst düzey farklılıklar vurgulanmıştır.

| Premium                               | Standart                       |
|---------------------------------------|--------------------------------|
| Yüksek verimlilik                       | Değişken işleme            |
| Tahmin edilebilir performans               | Değişken gecikme süresi               |
| Tahmin edilebilir fiyatlandırma                   | Kullandıkça Öde değişken fiyatlandırması |
| İş yükünü yukarı ve aşağı ölçeklendirebilme | Yok                            |
| İleti boyutu > 256 KB                  | İleti boyutu 256 KB'dir          |

**Service Bus Premium Mesajlaşma Hizmeti**, CPU'da ve bellek katmanında kaynak yalıtımına olanak sağladığından her müşterinin iş yükü yalıtımlı şekilde çalışır. Bu kaynak kapsayıcısı *mesajlaşma birimi* olarak adlandırılır. Her premium ad alanı, en az bir mesajlaşma birimi için ayrılmıştır. Her Service Bus Premium ad alanı için 1, 2 veya 4 mesajlaşma birimi satın alabilirsiniz. Tek bir iş yükü veya varlık, birden çok mesajlaşma birimine yayılabilir ve faturalandırma 24 saatlik veya günlük oran fiyatlarında gerçekleştirilse de mesajlaşma birimlerinin sayısı isteğe bağlı olarak değiştirilebilir. Sonuç olarak, Service Bus tabanlı çözümünüz için tahmin edilebilir ve tekrarlanabilir bir performans elde edersiniz.

Daha tahmin edilebilir ve kullanılabilir olmasının yanı sıra bu performans, daha hızlıdır. Service Bus Premium mesajlaşma hizmeti, [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) kısmında tanıtılan depolama motorunda derlenir. Premium mesajlaşma sayesinde, en yüksek performans Standart katmanda olduğundan daha hızlıdır.

## <a name="premium-messaging-technical-differences"></a>Premium Mesajlaşmanın teknik farklılıkları

Premium ve Standart mesajlaşma katmanları arasındaki bazı farklar aşağıda verilmiştir.

### <a name="partitioned-queues-and-topics"></a>Bölümlenmiş kuyruklar ve konular

Bölümlenmiş kuyruklar ve konular Premium mesajlaşmada desteklenir ancak Service Bus mesajlaşma hizmetinin Standart ve Temel katmanlarında aynı şekilde işlev görmez. Premium mesajlaşma, SQL'i bir veri deposu olarak kullanmaz ve artık paylaşılan platforma ilişkin olası kaynak rekabetini barındırmaz. Sonuç olarak, bölümleme gerekli değildir. Ayrıca, Standart mesajlaşmada 16 olan bölüm sayısı Premium'da 2 bölüm olarak değiştirilmiştir. İki bölümlemeye sahip olmak kullanılabilirliği garanti altına alır ve Premium çalışma zamanı ortamı için daha uygun bir sayıdır. Bölümleme hakkında daha fazla bilgi için bkz. [Bölümlenmiş kuyruklar ve konular](service-bus-partitioning.md).

### <a name="express-entities"></a>İfade varlıkları

Premium Mesajlaşma tamamen yalıtılmış bir çalışma zamanı ortamında çalıştığından Premium ad alanlarında ifade varlıkları desteklenmemektedir. İfade özellikleri hakkında daha fazla bilgi için [Microsoft.ServiceBus.Messaging.QueueDescription.EnableExpress](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enableexpress.aspx) özelliğine bakın.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşma hizmeti hakkında daha fazla bilgi edinmek için aşağıdaki konu başlıklarına bakın.

- [Azure Service Bus mesajlaşma hizmetine giriş (blog gönderisi)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
- [Azure Service Bus Premium mesajlaşma hizmetine giriş (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
- [Service Bus mesajlaşma hizmetine genel bakış](service-bus-messaging-overview.md)
- [Service Bus kuyruklarını kullanma](service-bus-dotnet-get-started-with-queues.md)



<!--HONumber=Oct16_HO3-->


