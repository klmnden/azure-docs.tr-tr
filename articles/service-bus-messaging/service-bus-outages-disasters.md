---
title: Azure Service Bus uygulama kesintiler ve olağanüstü karşı insulating | Microsoft Docs
description: Uygulamaların olası bir hizmet veri yolu kesinti karşı koruma için teknikler.
services: service-bus-messaging
author: sethmanheim
manager: timlt
ms.service: service-bus-messaging
ms.topic: article
ms.date: 06/14/2018
ms.author: sethm
ms.openlocfilehash: 1d960349b50e2618365fd085cba7b3e55fa53874
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36301725"
---
# <a name="best-practices-for-insulating-applications-against-service-bus-outages-and-disasters"></a>Hizmet veri yolu kesintileri ve olağanüstü karşı uygulamalar insulating için en iyi uygulamalar

Görev açısından kritik uygulamalar, Planlanmayan kesintiler veya olağanüstü varlığında olsa bile sürekli olarak çalışması gerekir. Bu makale, Service Bus uygulamaların potansiyel hizmet kesintisi veya olağanüstü durum karşı koruma için kullanabileceğiniz teknikleri açıklar.

Bir kesinti geçici olarak kullanım dışı kalması Azure hizmet veri yolu tanımlanır. Kesinti Service Bus Mesajlaşma deposu veya hatta tüm veri merkezi gibi bazı bileşenleri etkileyebilir. Sorun çözüldükten sonra hizmet veri yolu yeniden kullanılabilir hale gelir. Genellikle, bir kesinti iletileri veya diğer veri kaybına neden olmaz. Belirli bir Mesajlaşma deposu kullanılamama bileşeni hatası örnektir. Bir güç kesintisi datacenter ya da hatalı veri merkezi ağ anahtarı, bir veri merkezi çapında kesinti örnektir. Bir kesinti birkaç dakika ile birkaç gün sürebilir.

Bir olağanüstü durum kalıcı kaybı Service Bus ölçek birimi veya veri merkezi tanımlanır. Veri merkezi olabilir veya yeniden kullanılabilir duruma gelebilir. Genellikle bir olağanüstü durum bazı veya tüm iletileri veya diğer veri kaybına neden olur. Yangın, taşmasını veya deprem afetler örnekleridir.

## <a name="current-architecture"></a>Geçerli mimari
Service Bus kuyrukları veya konuları gönderilen iletileri depolamak için birden çok Mesajlaşma deposu kullanır. Bölümlenmemiş kuyruk veya konu bir Mesajlaşma deposuna atanır. Bu ileti deposunu kullanılamıyorsa, kuyruk veya konu tüm işlemler başarısız olur.

Bir veri merkezi ile bağlantılı bir hizmet ad alanındaki tüm Service Bus Mesajlaşma varlıkları (kuyruklar, konular, geçişler) bulunur. Hizmet veri yolu destekler [ *coğrafi olağanüstü durum kurtarma* ve *coğrafi çoğaltma* ](service-bus-geo-dr.md) ad alanı düzeyinde.

## <a name="protecting-queues-and-topics-against-messaging-store-failures"></a>Kuyruklar ve konu başlıkları deposu başarısızlıkları Mesajlaşma karşı koruma
Bölümlenmemiş kuyruk veya konu bir Mesajlaşma deposuna atanır. Bu ileti deposunu kullanılamıyorsa, kuyruk veya konu tüm işlemler başarısız olur. Bölümlenmiş bir sıra diğer taraftan, birden çok parçalarını oluşur. Her parça farklı bir Mesajlaşma deposunda depolanır. Bölümlenmiş kuyruk veya konu için bir ileti gönderildiğinde, hizmet veri yolu ileti parçasının birine atar. Hizmet veri yolu ileti için farklı bir parçası, karşılık gelen ileti deposu kullanılamıyorsa, mümkünse yazar. Bölümlenen varlıklar de artık desteklenmektedir [Premium SKU](service-bus-premium-messaging.md). 

Bölümlenen varlıklar hakkında daha fazla bilgi için bkz: [bölümlenmiş Mesajlaşma varlıkları][Partitioned messaging entities].

## <a name="protecting-against-datacenter-outages-or-disasters"></a>Veri Merkezi kesintilerini veya olağanüstü karşı koruma
İki veri merkezi arasında bir yük devretme için izin vermek için her veri merkezinde bir Service Bus hizmeti ad alanı oluşturabilirsiniz. Örneğin, Service Bus hizmeti ad alanı **contosoPrimary.servicebus.windows.net** Amerika Birleşik Devletleri Kuzey/Orta bölgesinde bulunan ve **contosoSecondary.servicebus.windows.net** BİZE Güney/Orta bölgesinde bulunan. Service Bus varlık Mesajlaşma bir veri merkezi kesintisinden varlığında erişilebilir kalması gereken, varlığın her iki ad alanları oluşturabilirsiniz.

Daha fazla bilgi için "Service Bus Azure veri merkezi içinde hatası" bölümüne bakın [zaman uyumsuz desenleri ve yüksek kullanılabilirlik Mesajlaşma][Asynchronous messaging patterns and high availability].

## <a name="protecting-relay-endpoints-against-datacenter-outages-or-disasters"></a>Veri Merkezi kesintilerini veya olağanüstü karşı koruma geçiş uç noktaları
Coğrafi çoğaltma geçiş uç noktaları varlığında Service Bus kesintileri erişilebilir olması için bir geçiş uç noktası kullanıma sunan bir hizmet sağlar. Coğrafi çoğaltma elde etmek için hizmet, farklı ad alanlarında iki geçiş uç noktalar oluşturmanız gerekir. Ad alanları farklı veri merkezlerinde bulunmalıdır ve iki uç nokta adları farklı olmalıdır. Örneğin, birincil bir uç nokta altında ulaşılabilen **contosoPrimary.servicebus.windows.net/myPrimaryService**altında ikincil kendisine karşılık gelen ulaşılabilen yaparken **contosoSecondary.servicebus.windows.net/mySecondaryService**.

Hizmet ardından her iki bitiş noktasında dinler ve bir istemci hizmeti ya da uç nokta ile çağırabilirsiniz. Bir istemci uygulaması rastgele geçişler birini birincil uç noktası olarak seçer ve isteğini etkin uç noktasına gönderir. İşlem hata kodu ile başarısız olursa geçiş uç noktası kullanılamıyor bu hatayı gösterir. Uygulama yedekleme uç noktası için bir kanal açar ve isteği yeniden yayımlar. Bu noktada rolleri etkin ve yedekleme uç noktaları geçiş: istemci uygulamasının yeni yedekleme uç noktası ve yeni etkin uç noktası için eski yedekleme uç noktası için eski etkin uç noktası olarak değerlendirir. Her ikisi de işlemleri başarısız gönderirseniz, iki varlık rolleri değişmeden kalır ve bir hata döndürdü.

## <a name="protecting-queues-and-topics-against-datacenter-outages-or-disasters"></a>Kuyruklar ve konu başlıkları datacenter kesintileri ya da olağanüstü karşı koruma
Hizmet veri yolu kullanarak aracılı Mesajlaşma, veri merkezi kesintilere karşı esnekliği elde etmek için iki yaklaşım destekler: *etkin* ve *pasif* çoğaltma. Belirtilen kuyruk veya konu veri merkezi kesintisinden varlığında erişilebilir kalması gereken varsa her bir yaklaşım, her iki ad alanları oluşturabilirsiniz. Her iki varlık aynı ada sahip olabilir. Örneğin, birincil bir kuyruk altında ulaşılabilen **contosoPrimary.servicebus.windows.net/myQueue**altında ikincil kendisine karşılık gelen ulaşılabilen yaparken **contosoSecondary.servicebus.windows.net/myQueue**.

Uygulama kalıcı gönderenin alıcı iletişim gerektirmiyorsa, uygulama ileti kaybını önlemek için ve geçici hizmet veri yolu hataları gönderenden korunamadı dayanıklı bir istemci-tarafı sıra uygulayabilirsiniz.

## <a name="active-replication"></a>Etkin çoğaltma
Etkin çoğaltma varlıklar her işlem için her iki ad kullanır. Bir ileti gönderen herhangi bir istemci aynı ileti iki kopyasını gönderir. İlk kopyayı birincil varlığa gönderilen (örneğin, **contosoPrimary.servicebus.windows.net/sales**), ve iletiyi ikinci bir kopyası ikincil varlığa gönderilen (örneğin, **contosoSecondary.servicebus.windows.net/sales**).

Bir istemci, her iki sıralarından iletilerini alır. İletinin ilk kopyasını alıcı işler ve ikinci kopya engellenir. Yinelenen iletileri bastırmak için gönderen her iletinin benzersiz bir tanımlayıcı etiketlemeniz gerekir. İletinin her iki kopyası aynı tanımlayıcıyla etiketlenmelidir. Kullanabileceğiniz [BrokeredMessage.MessageId] [ BrokeredMessage.MessageId] veya [BrokeredMessage.Label] [ BrokeredMessage.Label] özellikleri ya da ileti etiketlemek için özel bir özellik. Alıcı zaten aldığı iletileri listesini bulundurmanız gerekir.

[Service Bus aracılı iletiler coğrafi çoğaltma] [ Geo-replication with Service Bus Brokered Messages] örnek Mesajlaşma, etkin çoğaltma gösterir.

> [!NOTE]
> Etkin çoğaltma yaklaşım işlemlerinin sayısı iki katına çıkarır, bu nedenle, bu yaklaşım için daha yüksek maliyet yol açabilir.
> 
> 

## <a name="passive-replication"></a>Pasif çoğaltma
Hataya serbest durumda pasif çoğaltma iki Mesajlaşma varlıkları yalnızca birini kullanır. Bir istemcinin etkin varlığa iletisi gönderir. Etkin varlık üzerinde işlem etkin varlık barındıran veri merkezinde kullanılamayabilir belirten bir hata kodu ile başarısız olursa, istemci yedekleme varlığa iletinin bir kopyasını gönderir. Bu noktada etkin ve yedekleme varlıkları rollerini değiştirmek: Yeni yedekleme varlığı olarak eski active varlık gönderen istemci göz önünde bulundurur ve eski yedekleme varlık yeni etkin varlıktır. Her ikisi de işlemleri başarısız gönderirseniz, iki varlık rolleri değişmeden kalır ve bir hata döndürdü.

Bir istemci, her iki sıralarından iletilerini alır. Alıcı aynı ileti iki kopyasını alır şansı olduğundan, alıcı yinelenen iletilerini gerekir. Etkin çoğaltma için açıklandığı gibi aynı şekilde çoğaltmaları gizleyebilirsiniz.

Genel olarak, çoğu durumda yalnızca bir işlem yapıldığından pasif çoğaltma etkin çoğaltma daha ekonomik. Gecikme süresi, üretilen iş ve parasal maliyeti çoğaltılmamış senaryo ile aynıdır.

Pasif çoğaltma kullanırken, aşağıdaki senaryolarda iletileri kaybolabilir veya iki kez alındı:

* **İleti gecikme veya kayıp**: Gönderenin iletiyi m1 birincil kuyruğuna başarıyla gönderildi. ve ardından alıcı m1 almadan önce sıranın kullanılamaz hale varsayılır. Gönderici bir sonraki iletiyi m2 ikincil kuyruğuna gönderir. Birincil kuyruk geçici olarak devre dışı ise, sıranın yeniden kullanılabilir hale geldikten sonra alıcı m1 alır. Bir olağanüstü durumda alıcı hiçbir zaman m1 alabilirsiniz.
* **Alma yinelenen**: gönderenin birincil kuyruğuna ileti m gönderir varsayalım. Hizmet veri yolu başarıyla m işler ancak yanıt gönderme başarısız olur. Gönderme işlemi zaman aşımına sonra gönderen ikincil kuyruğuna m özdeş bir kopyasını gönderir. Alıcı birincil kuyruk kullanılamaz hale önce m ilk kopyasına almak mümkün ise, alıcı yaklaşık aynı zamanda m iki kopyasını alır. Alıcı birincil kuyruk kullanılamaz hale önce m ilk kopyasına almak mümkün değilse, alıcı başlangıçta yalnızca ikinci bir kopyası m alır, ancak birincil kuyruk kullanılabilir hale geldiğinde m ikinci bir kopyasını alır.

[Coğrafi çoğaltma ile Service Bus aracılı ileti] [ Geo-replication with Service Bus Brokered Messages] örnek Mesajlaşma, pasif çoğaltma gösterir.

## <a name="geo-replication"></a>Coğrafi çoğaltma

Hizmet veri yolu coğrafi olağanüstü durum kurtarma ve coğrafi çoğaltma, ad alanı düzeyinde destekler. Daha fazla bilgi için bkz: [Azure Service Bus coğrafi olağanüstü durum kurtarma](service-bus-geo-dr.md). Kullanılabilir olağanüstü durum kurtarma özelliği [Premium SKU](service-bus-premium-messaging.md) yalnızca meta veri olağanüstü durum kurtarma uygular ve birincil ve ikincil olağanüstü durum kurtarma ad alanında bulunan kullanır.

## <a name="availability-zones-preview"></a>Kullanılabilirlik bölgeler (Önizleme)

Service Bus Premium SKU destekleyen [kullanılabilirlik bölgeleri](../availability-zones/az-overview.md), bir Azure bölgesi hataya yalıtılmış konumlara sağlama. 

> [!NOTE]
> Kullanılabilirlik bölgeleri Önizleme yalnızca desteklenen **Orta ABD**, **Doğu ABD 2**, ve **Fransa Merkezi** bölgeleri.

Azure Portalı'nı kullanarak yeni ad alanında bulunan yalnızca kullanılabilirlik bölgeleri etkinleştirebilirsiniz. Hizmet veri yolu var olan ad alanlarının geçişini desteklemez. Bölge artıklık ad alanınıza etkinleştirdikten sonra devre dışı bırakılamıyor.

![1][]

## <a name="next-steps"></a>Sonraki adımlar
Olağanüstü durum kurtarma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure Service Bus coğrafi olağanüstü durum kurtarma](service-bus-geo-dr.md)
* [Azure SQL veritabanı iş sürekliliği][Azure SQL Database Business Continuity]
* [Azure için dayanıklı uygulamalar tasarlama][Azure resiliency technical guidance]

[Service Bus Authentication]: service-bus-authentication-and-authorization.md
[Partitioned messaging entities]: service-bus-partitioning.md
[Asynchronous messaging patterns and high availability]: service-bus-async-messaging.md#failure-of-service-bus-within-an-azure-datacenter
[BrokeredMessage.MessageId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId
[BrokeredMessage.Label]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label
[Geo-replication with Service Bus Brokered Messages]: https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoReplication
[Azure SQL Database Business Continuity]: ../sql-database/sql-database-business-continuity.md
[Azure resiliency technical guidance]: /azure/architecture/resiliency

[1]: ./media/service-bus-outages-disasters/az.png