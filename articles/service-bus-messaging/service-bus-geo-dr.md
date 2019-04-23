---
title: Azure Service Bus Geo-olağanüstü durum kurtarma | Microsoft Docs
description: Yük devretme için coğrafi bölgeler kullanın ve Azure Service Bus olağanüstü durum kurtarma gerçekleştirmek nasıl
services: service-bus-messaging
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.topic: article
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: a0581ef43e8a3c02126612a21122db559a941370
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60009183"
---
# <a name="azure-service-bus-geo-disaster-recovery"></a>Azure Service Bus Geo-olağanüstü durum kurtarma

Tüm durumlarda Azure bölgeleri veya veri merkezleri (hiçbir [kullanılabilirlik alanları](../availability-zones/az-overview.md) kullanılır), kesinti yaşamak veri işleme, farklı bir bölge veya veri merkezi içinde çalışmaya devam etmek önemlidir. Bu nedenle, *coğrafi olağanüstü durum kurtarma* tüm kuruluş için önemli bir özelliğidir. Azure Service Bus ad alanı düzeyinde coğrafi olağanüstü durum kurtarma destekler.

Coğrafi olağanüstü durum kurtarma özelliği, Service Bus Premium SKU için genel olarak kullanılabilir. 

>[!NOTE]
> Coğrafi olağanüstü durum kurtarma şu anda yalnızca meta veriler (kuyruklar, konular, abonelikler, filtreleri) kopyalanır üzerinden, birincil ad alanından birlikte kullanıldığında ikincil ad alanına sağlar.

## <a name="outages-and-disasters"></a>Kesintileri ve olağanüstü durumları yönetme

"Kesintiler" ve "olağanüstü durumlar" arasındaki fark dikkate almak önemlidir 

Bir *kesinti* Azure Service Bus geçici olarak kullanım dışı kalması olduğu ve bazı bileşenleri gibi bir Mesajlaşma deposu veya hatta veri merkezinin tamamı hizmete etkileyebilir. Sorun düzeltildikten sonra ancak Service Bus yeniden kullanılabilir hale gelir. Genellikle, bir kesinti iletileri veya diğer veri kaybına neden olmaz. Böyle bir kesinti örneği, veri merkezinde güç kesintisi olabilir. Bazı kesintiler yalnızca kısa bağlantı kayıpları geçici ya da ağ sorunları nedeniyle olduğundan. 

A *olağanüstü durum* bir Service Bus kümesi, Azure bölgesi veya veri merkezi kalıcı ya da daha uzun vadeli zarar tanımlanır. Bölge veya veri merkezi olabilir veya yeniden kullanılabilir olmaktan çıkabilir veya saatler veya günler için aşağı sonuna olmayabilir. Ateş, taşmasını veya deprem gibi olağanüstü örnekleridir. Kalıcı hale olağanüstü bir durum, bazı iletileri, olayları ya da diğer veri kaybına neden olabilir. Ancak, çoğu durumda olması gerekir veri kaybı olmadan ve yedekleme veri merkezi olduktan sonra iletileri kurtarılabilir.

Azure Service Bus coğrafi olağanüstü durum kurtarma özelliği bir olağanüstü durum kurtarma çözümüdür. Bu makalede açıklanan iş akışı ve kavramları olağanüstü durum senaryoları ve geçici ve geçici kesintiler için geçerlidir. Microsoft azure'da olağanüstü durum kurtarma hakkında ayrıntılı bilgi için bkz: [bu makalede](/azure/architecture/resiliency/disaster-recovery-azure-applications).   

## <a name="basic-concepts-and-terms"></a>Temel kavramlar ve terimler

Olağanüstü Durum Kurtarma özelliği meta verileri olağanüstü durum kurtarma uygular ve birincil ve ikincil olağanüstü durum kurtarma ad alanlarında kullanır. Coğrafi olağanüstü durum kurtarma özelliği için kullanılabilir olduğunu unutmayın [Premium SKU](service-bus-premium-messaging.md) yalnızca. Bir diğer ad üzerinden bağlantı kurulmadığı herhangi bir bağlantı dizesi değişiklik yapılması gerekmez.

Bu makalede aşağıdaki terimler kullanılır:

-  *Diğer ad*: Ayarladığınız bir olağanüstü durum kurtarma Yapılandırması adı. Diğer ad, tek bir kararlı tam etki alanı adı (FQDN) bağlantı dizesi sağlar. Uygulamaları, bir ad alanına bağlanmak için bu diğer ad bağlantı dizesi kullanır. Bir diğer ad kullanımı, yük devretme tetiklendiğinde, bağlantı dizesini değişmeden olmasını sağlar.

-  *Birincil/ikincil ad alanı*: Diğer adı için karşılık gelen ad alanları. Birincil ad "etkin" ve iletileri (Bu, mevcut veya yeni bir ad olabilir) alır. İkincil ad alanı, "pasif" ve iletileri almaz. Her ikisi arasındaki bir meta veri eşitlenmiş olarak olduğundan her ikisi de sorunsuz bir şekilde uygulama kodu veya bağlantı dizesi değişiklik yapmadan iletileri kabul edebilir. Etkin ad alanı iletileri aldığından emin olmak için diğer adı kullanmanız gerekir. 

-  *meta veri*: Kuyruklar, konular ve abonelikler gibi varlıkları; ve hizmetin ad alanıyla ilişkili özellikleri. Varlıklar ve ayarları otomatik olarak çoğaltılır unutmayın. İletileri çoğaltılmaz.

-  *Yük devretme*: İkincil ad alanı etkinleştiriliyor işlemi.

## <a name="setup"></a>Kurulum

Aşağıdaki bölümde, ad alanları arasında eşleştirme kurulum için bir genel bakıştır.

![1][]

Kurulum işlemi aşağıdaki gibidir-

1. Sağlama bir ***birincil*** hizmet veri yolu Premium Namespace.

2. Sağlama bir ***ikincil*** Service Bus Premium Namespace bir bölgede *nerede birincil ad alanı sağlandığında farklı*. Bu, farklı bir veri merkezinde bölgeler arasında hata Yalıtımı izin yardımcı olur.

3. Elde etmek için ikincil ad alanı ve birincil ad alanı arasında eşleşmeyi ***diğer***.

4. Kullanım ***diğer*** 3. adımda elde ettiğiniz Geo-DR istemci uygulamalarınıza bağlanmak için birincil ad alanı etkin. Başlangıçta, diğer birincil ad alanına işaret eder.

5. [İsteğe bağlı] Bir yük devretme gerekli olup olmadığını algılamak için bazı izleme ekleyin.

## <a name="failover-flow"></a>Yük devretme akışı

Bir yük devretme, el ile müşterinin (ya açık bir komut veya komut tetikleyen iş mantığı ait istemci) ve Azure tarafından hiçbir zaman tetiklenir. Bu müşteri tam sahiplik ve görünürlük kesintisi çözümlenmek Azure'un omurga sağlar.

![4][]

Yük devretmeden sonra tetiklenen-

1. ***Diğer*** bağlantı dizesi, ikincil bir Premium ad alanına işaret edecek şekilde güncelleştirilir.

2. İstemciler (göndericiler ve alıcılar) otomatik olarak ikincil ad alanına bağlanın.

3. Var olan birincil ve ikincil premium ad alanı arasında eşleştirme bozulur.

Yük devretme başlatıldıktan sonra-

1. Başka bir kesinti oluşursa, baştan başarısız yönetebilmek istiyorsunuz. Bu nedenle, başka bir pasif ad alanı ayarlama ve eşleştirmesini güncelleştirin. 

2. Yeniden kullanılabilir olduğunda eski birincil ad alanındaki iletileri çekin. Bundan sonra bu ad alanı normal coğrafi kurtarma kurulumunuzu dışında Mesajlaşma için kullanın veya eski birincil ad alanı silin.

> [!NOTE]
> Yalnızca hata iletme semantiği desteklenir. Bu senaryoda, yük devretme ve ardından yeni bir ad alanı ile yeniden eşleştirin. İlk duruma döndürmeden desteklenmiyor; Örneğin SQL kümesi. 

Yük devretme ile izleme sistemlerinden veya özel olarak geliştirilmiş izleme çözümleri ile otomatik hale getirebilirsiniz. Ancak, bu tür Otomasyon ek planlama ve bu makalenin kapsamı dışında olan iş alır.

![2][]

## <a name="management"></a>Yönetim

Bir hata yaptıysanız; Örneğin, ilk kurulum sırasında yanlış bölgede eşleştirilmiş, iki ad alanlarını herhangi bir zamanda eşleştirme bozabilir. Eşleştirilen ad alanları normal bir ad kullanmak istiyorsanız, diğer silin.

## <a name="use-existing-namespace-as-alias"></a>Var olan ad alanı diğer ad olarak kullanın

Üreticilerin ve tüketicilerin bağlantılar değiştiremezsiniz senaryo varsa, diğer ad, ad alanınızın adıyla yeniden kullanabilirsiniz. Bkz: [örnek kodu github'da Buraya](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR_existing_namespace_name).

## <a name="samples"></a>Örnekler

[Samples github'da](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/) ayarlama ve bir yük devretme başlatma işlemi gösterilmektedir. Bu örnekler, aşağıdaki kavramları göstermektedir:

- Bir .NET örneği ve Azure Active Directory'de Azure Resource Manager, ayarlamak ve coğrafi olağanüstü durum kurtarmayı etkinleştirmek için Service Bus ile kullanmak için gerekli ayarları.
- Örnek kod yürütmek için gerekli adımlar.
- Mevcut bir ad alanı diğer ad olarak kullanma
- Bunun yerine PowerShell veya CLI aracılığıyla coğrafi olağanüstü durum kurtarma etkinleştirme adımları.
- [Gönderme ve alma](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1) diğer adı kullanarak geçerli birincil veya ikincil ad.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu sürümle birlikte göz önünde tutmak için aşağıdaki konuları göz önünde bulundurun:

1. Planınızı yük devretme saat faktörü düşünmelisiniz. Örneğin, 15-20 dakikadan fazla bağlantısını kaybederseniz, yük devretmeyi başlatmak karar verebilirsiniz.

2. Hiçbir veri çoğaltılır şu anda etkin oturumları değil çoğaltıldığından emin anlamına gelir. Ayrıca, yinelenen algılama ve zamanlanmış iletileri çalışmayabilir. Yeni oturumlar, yeni zamanlanmış iletileri ve yeni yinelenen çalışır. 

3. Karmaşık dağıtılmış bir altyapı yük devrediliyor olmalıdır [prova](/azure/architecture/resiliency/disaster-recovery-azure-applications#disaster-simulation) en az bir kez.

4. Varlık eşitleme dakika başına yaklaşık 50-100 varlık biraz zaman alabilir. Abonelikler ve kuralları da varlıklar olarak sayılır.

## <a name="availability-zones"></a>Kullanılabilirlik Alanları

Ayrıca, Service Bus Premium SKU destekler [kullanılabilirlik](../availability-zones/az-overview.md), sağlayan bir Azure bölgesi içinde hatadan yalıtılmış konumlardır.

> [!NOTE]
> Azure Service Bus Premium kullanılabilirlik desteği yalnızca kullanılabilir [Azure bölgeleri](../availability-zones/az-overview.md#services-support-by-region) kullanılabilirlik nerede bulunduğunu.

Kullanılabilirlik alanları, yeni ad alanları üzerinde yalnızca, Azure portalını kullanarak etkinleştirebilirsiniz. Service Bus, var olan ad alanlarının geçişini desteklemez. Bölge artıklığı ad alanınızı etkinleştirildikten sonra devre dışı bırakılamıyor.

![3][]

## <a name="next-steps"></a>Sonraki adımlar

- Coğrafi olağanüstü durum kurtarma Bkz [REST API Başvurusu](/rest/api/servicebus/disasterrecoveryconfigs).
- Coğrafi olağanüstü durum kurtarmayı çalıştırabilirsiniz [örneğine Github'dan](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR2).
- Coğrafi olağanüstü durum kurtarma Bkz [iletiler gönderen bir diğer örnek](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1).

Service Bus mesajlaşması hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
* [REST API](/rest/api/servicebus/) 

[1]: ./media/service-bus-geo-dr/geodr_setup_pairing.png
[2]: ./media/service-bus-geo-dr/geo2.png
[3]: ./media/service-bus-geo-dr/az.png
[4]: ./media/service-bus-geo-dr/geodr_failover_alias_update.png