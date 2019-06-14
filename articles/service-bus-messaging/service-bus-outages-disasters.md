---
title: Azure Service Bus uygulamaları kesintiler ve olağanüstü durumlar karşı insulating | Microsoft Docs
description: Uygulamaları olası bir hizmet veri yolu kesintiye karşı korumak için teknikleri.
services: service-bus-messaging
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.topic: article
ms.date: 09/14/2018
ms.author: aschhab
ms.openlocfilehash: 24611e265788cf046aa0733bc423917aaf305427
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60589738"
---
# <a name="best-practices-for-insulating-applications-against-service-bus-outages-and-disasters"></a>Hizmet veri yolu kesintilerini ve olağanüstü durumları yönetme karşı uygulamalar insulating için en iyi yöntemler

Görev açısından kritik uygulamalar, planlanmamış kesinti veya olağanüstü saklanacaktır bile sürekli olarak çalışması gerekir. Bu makalede hizmet veri yolu uygulamaları potansiyel hizmet kesintisi veya olağanüstü durum karşı korumak için kullanabileceğiniz teknikleri açıklar.

Kesinti geçici olarak kullanım dışı kalması Azure Service Bus tanımlanır. Service Bus, bazı bileşenleri gibi bir Mesajlaşma deposu veya bile tüm veri merkezi kesintisi etkileyebilir. Sorun çözüldükten sonra Service Bus yeniden kullanılabilir hale gelir. Genellikle, bir kesinti iletileri veya diğer veri kaybına neden olmaz. Belirli bir Mesajlaşma deposu kullanılamama bileşeni hatası örneğidir. Veri Merkezi hatalı veri merkezi ağ anahtarı veya güç kesintisi veri merkezi çapında kesinti örneğidir. Kesinti birkaç dakika veya birkaç gün sürebilir.

Olağanüstü bir durumda, kalıcı bir Service Bus ölçek birimi veya veri merkezi kaybına tanımlanır. Veri merkezi olabilir veya yeniden kullanılabilir olmaktan çıkabilir. Genellikle bir olağanüstü durum bazı veya tüm iletileri veya diğer veri kaybına neden olur. Ateş, taşmasını veya deprem felaketler örnekleridir.

## <a name="protecting-against-outages-and-disasters---service-bus-premium"></a>Kesintileri ve olağanüstü durumları yönetme - hizmet veri yolu Premium karşı koruma
Yüksek kullanılabilirlik ve olağanüstü durum kurtarma kavramları yerleşik Azure Service Bus Premium katmanı, her ikisi de aynı bölgede (aracılığıyla, kullanılabilirlik alanları) olarak sağ ve farklı bölgelere (aracılığıyla coğrafi olağanüstü durum kurtarma).

### <a name="geo-disaster-recovery"></a>Coğrafi olağanüstü durum kurtarma

Service Bus Premium ad alanı düzeyinde coğrafi olağanüstü durum kurtarma destekler. Daha fazla bilgi için [Azure Service Bus Geo-olağanüstü durum kurtarma](service-bus-geo-dr.md). İçin olağanüstü durum kurtarma özelliği [Premium SKU](service-bus-premium-messaging.md) yalnızca meta veri olağanüstü durum kurtarma uygular ve birincil ve ikincil olağanüstü durum kurtarma ad alanlarında kullanır.

### <a name="availability-zones"></a>Kullanılabilirlik Alanları

Service Bus Premium SKU destekler [kullanılabilirlik](../availability-zones/az-overview.md), aynı Azure bölgesindeki hatadan yalıtılmış konumlardır sağlama.

> [!NOTE]
> Azure Service Bus Premium kullanılabilirlik desteği yalnızca kullanılabilir [Azure bölgeleri](../availability-zones/az-overview.md#services-support-by-region) kullanılabilirlik nerede bulunduğunu.

Kullanılabilirlik alanları, yeni ad alanları üzerinde yalnızca, Azure portalını kullanarak etkinleştirebilirsiniz. Service Bus, var olan ad alanlarının geçişini desteklemez. Bölge artıklığı ad alanınızı etkinleştirildikten sonra devre dışı bırakılamıyor.

![1][]


## <a name="protecting-against-outages-and-disasters---service-bus-standard"></a>Kesintileri ve olağanüstü durumları yönetme - Service Bus standart karşı koruma
Service Bus Mesajlaşma fiyatlandırma katmanını standart kullanırken veri merkezi kesintilerine karşı dayanıklılık elde etmek için iki yaklaşım destekler: *etkin* ve *pasif* çoğaltma. Belirli bir kuyruk veya konuda bir veri merkezi kesintisi saklanacaktır erişilebilir kalması gereken her bir yaklaşıma, bu iki ad alanları oluşturabilirsiniz. Her iki varlık aynı ada sahip olabilir. Örneğin, birincil bir kuyruk altında ulaşılabilir **contosoPrimary.servicebus.windows.net/myQueue**altında ikincil çözümlemesiyle ulaşılabilir korurken **contosoSecondary.servicebus.windows.net/myQueue**.

>[!NOTE]
> **Etkin çoğaltma** ve **pasif çoğaltma** Kurulum: genel amaçlı çözümleri ve Service Bus özel olmayan özellikler. (2 farklı ad alanlarına gönderme) çoğaltma mantıksal gönderen uygulamaları yaşar ve alıcı, yinelenen algılama için özel mantığı iznine sahip olması gerekir.

Uygulamayı kalıcı gönderenin alıcı iletişim gerektirmiyorsa, uygulamayı dayanıklı bir istemci-tarafı kuyruk iletisi kaybını önlemek için ve Service Bus geçici hatalar gönderenden yüklerden uygulayabilirsiniz.

### <a name="active-replication"></a>Etkin çoğaltma
Etkin çoğaltma varlıklar her işlem için her iki ad alanlarında kullanır. Bir ileti gönderen herhangi bir istemci aynı ileti iki kopyasını gönderir. İlk kopyalama için birincil varlık gönderilir (örneğin, **contosoPrimary.servicebus.windows.net/sales**), ve iletiyi ikinci bir kopyası ikincil varlığa gönderilir (örneğin,  **contosoSecondary.servicebus.windows.net/sales**).

Bir istemci, her iki sıralarından iletileri alır. Alıcı ilk iletinin kopyasını işler ve ikinci kopya bastırılır. Gönderen, yinelenen iletileri bastırmak için her iletinin benzersiz bir tanımlayıcı etiketlemeniz gerekir. Her iki kopyasında iletinin tanımlayıcısı aynı olan etiketlenmelidir. Kullanabileceğiniz [BrokeredMessage.MessageId] [ BrokeredMessage.MessageId] veya [BrokeredMessage.Label] [ BrokeredMessage.Label] özellikleri ya da ileti etiketlemek için özel bir özellik. Alıcı listesi zaten aldığı iletileri sürdürmeniz gerekir.

[Coğrafi çoğaltma ile Service Bus standart katman] [ Geo-replication with Service Bus Standard Tier] örnek etkin çoğaltma, Mesajlaşma varlıkları gösterir.

> [!NOTE]
> Etkin çoğaltma yaklaşım işlemlerin sayısı iki katına çıkarır, dolayısıyla bu yaklaşım daha yüksek maliyete yol açabilir.
> 
> 

### <a name="passive-replication"></a>Etkin olmayan çoğaltma
Hata ücretsiz durumda pasif çoğaltma iki Mesajlaşma varlıkları yalnızca birini kullanır. Bir istemcinin etkin varlığa iletiyi gönderir. Etkin varlık üzerinde işlem etkin varlık barındıran veri merkezinde kullanılamayabilir belirten bir hata kodu ile başarısız olursa istemci yedekleme varlığa iletinin bir kopyasını gönderir. Bu noktada etkin ve yedekleme varlıkları rollerini değiştirmek: Yeni yedekleme varlığı olarak eski active varlık gönderen istemci olarak kabul eder ve eski yedekleme varlık yeni etkin varlıktır. Hem de işlemler başarısız gönderirseniz, iki varlık rollerini değişmeden kalır ve bir hata döndürdü.

Bir istemci, her iki sıralarından iletileri alır. Alıcı, alıcının aynı ileti iki kopyasını aldığı bir fırsat olmadığından, yinelenen iletileri gösterme gerekir. Etkin çoğaltma için açıklandığı gibi aynı şekilde yinelenen gösterilmemesini sağlayabilirsiniz.

Çoğu durumda, yalnızca bir işlem gerçekleştirilir genel olarak, etkin olmayan çoğaltma etkin çoğaltma daha fazla ekonomik olmasıdır. Gecikme süresi, aktarım hızı ve parasal maliyet Çoğaltılmayan senaryoya aynıdır.

Etkin olmayan çoğaltma kullanırken, aşağıdaki senaryolarda iletileri kaybolabilir veya iki kez alındı:

* **İleti gecikmesi ya da zarar**: Gönderen birincil kuyruğa bir ileti m1 başarıyla gönderildi. ve ardından alıcı m1 almadan önce sıranın kullanılamaz hale varsayılır. Gönderici bir sonraki ileti m2 ikincil kuyruğa gönderir. Birincil kuyruk geçici olarak kullanılamıyorsa, sıranın tekrar kullanılabilir hale geldikten sonra alıcı m1 alır. Bir olağanüstü bir durumda, alıcı m1 hiçbir zaman alabilir.
* **Alma yinelenen**: Birincil kuyruğa gönderen bir ileti m gönderdiğini varsayar. Service Bus başarıyla m işler ancak yanıt göndermek başarısız olur. Gönderme işlemi zaman aşımına sonra gönderen m özdeş birer kopyası ikincil kuyruğa gönderir. Alıcı birincil kuyruk kullanılamaz duruma gelirse, önce ilk kopyayı m alabildiği, alıcı yaklaşık aynı zamanda her iki kopyasında m alır. Alıcı birincil kuyruk kullanılamaz duruma gelirse, önce ilk kopyayı m almak mümkün değilse, alıcı başlangıçta yalnızca m ikinci kopyasını alır, ancak birincil kuyruğa kullanıma sunulduğunda ardından m ikinci bir kopyasını alır.

[Coğrafi çoğaltma ile Service Bus standart katman] [ Geo-replication with Service Bus Standard Tier] örnek, Mesajlaşma varlıkları pasif çoğaltma gösterir.

## <a name="protecting-relay-endpoints-against-datacenter-outages-or-disasters"></a>Geçiş uç noktalarına felaketler ya da veri merkezi kesintilerine karşı koruma
Coğrafi çoğaltma, geçiş uç noktaları, hizmet veri yolu kesintilerini saklanacaktır erişilebilir olması için bir geçiş uç noktası hizmetidir sağlar. Coğrafi çoğaltma elde etmek için hizmet, farklı ad alanlarına iki geçiş uç noktası oluşturmanız gerekir. Ad alanları farklı veri merkezlerinde bulunmalıdır ve iki uç nokta adları farklı olmalıdır. Örneğin, bir birincil uç nokta altında ulaşılabilir **contosoPrimary.servicebus.windows.net/myPrimaryService**altında ikincil çözümlemesiyle ulaşılabilir korurken **contosoSecondary.servicebus.windows.net /mySecondaryService**.

Hizmet daha sonra her iki bitiş noktasında dinler ve bir istemci, hizmeti ya da uç noktası aracılığıyla çağırabilirsiniz. Bir istemci uygulaması rastgele geçişleri biri birincil uç noktası olarak seçer ve etkin uç noktaya, isteği gönderir. İşlem bir hata kodu ile başarısız olursa, bu hata, geçiş uç noktası kullanılamıyor gösteriyor. Uygulama, yedekleme uç nokta için bir kanal açar ve isteği yeniden yayımlar. Bu noktada etkin ve yedekleme uç rollerini değiştirmek: istemci uygulama yeni bir yedekleme uç nokta ve yeni etkin uç noktası olacak şekilde eski yedekleme uç nokta olacak şekilde eski etkin uç nokta olarak değerlendirir. Hem de işlemler başarısız gönderirseniz, iki varlık rollerini değişmeden kalır ve bir hata döndürdü.

## <a name="next-steps"></a>Sonraki adımlar
Olağanüstü durum kurtarma hakkında daha fazla bilgi edinmek için şu makalelere bakın:

* [Azure Service Bus Geo-olağanüstü durum kurtarma](service-bus-geo-dr.md)
* [Azure SQL veritabanı iş sürekliliği][Azure SQL Database Business Continuity]
* [Azure için dayanıklı uygulamalar tasarlama][Azure resiliency technical guidance]

[Service Bus Authentication]: service-bus-authentication-and-authorization.md
[Partitioned messaging entities]: service-bus-partitioning.md
[Asynchronous messaging patterns and high availability]: service-bus-async-messaging.md#failure-of-service-bus-within-an-azure-datacenter
[BrokeredMessage.MessageId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[BrokeredMessage.Label]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Geo-replication with Service Bus Standard Tier]: https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoReplication
[Azure SQL Database Business Continuity]: ../sql-database/sql-database-business-continuity.md
[Azure resiliency technical guidance]: /azure/architecture/resiliency

[1]: ./media/service-bus-outages-disasters/az.png
