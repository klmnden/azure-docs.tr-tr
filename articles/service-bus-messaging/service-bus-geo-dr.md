---
title: Azure Service Bus coğrafi olağanüstü durum kurtarma | Microsoft Docs
description: Coğrafi bölgeler için yük devretme kullanın ve Azure hizmet veri yolundaki olağanüstü durum kurtarma gerçekleştirmek nasıl
services: service-bus-messaging
documentationcenter: ''
author: christianwolf42
manager: timlt
editor: ''
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: sethm
ms.openlocfilehash: 652adcf78add8ae699a7f827a915e90ce1694c61
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
ms.locfileid: "30237354"
---
# <a name="azure-service-bus-geo-disaster-recovery"></a>Azure Service Bus coğrafi olağanüstü durum kurtarma

Tüm zaman Azure bölgeleri veya veri merkezleri (yoksa [kullanılabilirlik bölgeleri](../availability-zones/az-overview.md) kullanılır), sürecek farklı bölge veya veri merkezi içinde çalışmaya devam etmek veri işleme için önemlidir. Bu nedenle, *coğrafi olağanüstü durum kurtarma* ve *coğrafi çoğaltma* tüm kuruluş için önemli özelliklerdir. Azure Service Bus coğrafi olağanüstü durum kurtarma ve coğrafi çoğaltma, ad alanı düzeyinde destekler. 

Coğrafi olağanüstü durum kurtarma özelliği için Service Bus Premium SKU genel olarak kullanılabilir. 

## <a name="outages-and-disasters"></a>Kesintiler ve olağanüstü durumları

"Kesintileri" ve "afetler." arasında ayrım dikkate almak önemlidir Bir *kesinti* geçici olarak kullanım dışı kalması Azure Service Bus olduğu ve bazı bileşenleri bir Mesajlaşma deposu veya hatta tüm veri merkezi gibi hizmet etkileyebilir. Sorun düzeltildikten sonra Bununla birlikte, hizmet veri yolu yeniden kullanılabilir hale gelir. Genellikle, bir kesinti iletileri veya diğer veri kaybına neden olmaz. Bu tür bir kesinti örneği veri merkezinde bir güç kesintisi olabilir. Bazı kesintiler yalnızca kısa bağlantı zararları geçici ya da ağ sorunları nedeniyle ' dir. 

A *olağanüstü durum* kalıcı ya da daha uzun vadeli kaybı bir Service Bus küme, Azure bölgesi veya veri merkezi tanımlanmış. Bölge veya veri merkezi olabilir veya yeniden kullanılabilir olmaktan veya aşağı saatlerce veya günlerce olması olabilir. Bu tür afetler yangın, taşmasını veya deprem gösterilebilir. Kalıcı hale bir olağanüstü durum bazı iletiler, olayları ya da diğer veri kaybına neden olabilir. Ancak, çoğu durumda olması gerekir veri kaybı ve yedekleme veri merkezi başladıktan sonra iletileri kurtarılabilir.

Azure Service Bus coğrafi olağanüstü durum kurtarma özelliği bir olağanüstü durum kurtarma çözümüdür. Bu makalede açıklanan iş akışı ve kavramları olağanüstü durum senaryoları ve değil geçici ya da geçici kesintileri uygulanır. Microsoft Azure olağanüstü durum kurtarma hakkında ayrıntılı bilgi için bkz: [bu makalede](/azure/architecture/resiliency/disaster-recovery-azure-applications).   

## <a name="basic-concepts-and-terms"></a>Temel kavramlar ve terimler

Olağanüstü Durum Kurtarma özelliği meta verileri olağanüstü durum kurtarma uygular ve birincil ve ikincil olağanüstü durum kurtarma ad alanında bulunan dayanır. Coğrafi olağanüstü durum kurtarma özelliği için kullanılabilir olduğunu unutmayın [Premium SKU](service-bus-premium-messaging.md) yalnızca. Bağlantı bir diğer ad üzerinden yapılan gibi herhangi bir bağlantı dizesi değişiklik yapılması gerekmez.

Bu makalede aşağıdaki terimler kullanılır:

-  *Diğer ad*: sizin ayarladığınız bir olağanüstü durum kurtarma yapılandırması için ad. Diğer adı tek bir tutarlı tam etki alanı adı (FQDN) bağlantı dizesini sağlar. Uygulamalar, bir ad alanına bağlanmak için bu diğer ad bağlantı dizesini kullanın. 

-  *Birincil/ikincil ad alanı*: diğer adı için karşılık gelen ad alanları. Birincil ad "etkin" (Bu, mevcut veya yeni bir ad olabilir) iletileri ve alır. İkincil ad "pasif" ve iletileri almaz. Her ikisi arasındaki bir meta veri eşitleme, olduğundan her ikisi de sorunsuz olarak hiçbir uygulama kodu veya bağlantı dizesi değişiklik yapmadan iletileri kabul edebilir. Yalnızca etkin ad iletileri aldığından emin olmak için diğer adı kullanmanız gerekir. 

-  *Meta veri*: Kuyruklar, konuları ve abonelikleri; ve ad alanı ile ilişkili özellikleri hizmetinin gibi varlıklar. Yalnızca varlıkları ve ayarlarını otomatik olarak çoğaltılır unutmayın. İletileri çoğaltılmaz. 

-  *Yük devretme*: ikincil ad alanı etkinleştirme işlemi.

## <a name="setup-and-failover-flow"></a>Kurulum ve yük devretme akışı

Aşağıdaki bölümde yük devretme işlemini bir genel bakıştır ve ilk yük devretmeyi ayarlanacağı açıklanmaktadır. 

![1][]

### <a name="setup"></a>Kurulum

İlk oluşturmak veya var olan bir birincil ad alanını ve yeni bir ikincil ad alanı kullanın ve sonra iki eşleştirin. Bu eşleştirme bağlanmak için kullanabileceğiniz bir diğer ad sağlar. Bir diğer ad kullandığından, bağlantı dizelerinizi değiştirmeniz gerekmez. Yalnızca yeni ad alanları, yük devretme çifti için eklenebilir. Son olarak, bazı bir yük devretme gerekli olup olmadığını algılamak için izleme eklemeniz gerekir. Çoğu durumda, hizmet büyük bir ekosistem bir parçası, bu nedenle çok sık yerine kalan alt sistemi veya altyapı ile eşitleme gerçekleştirilmelidir gibi otomatik yük devretme işlemlerini nadiren mümkün.

### <a name="example"></a>Örnek

Bu senaryo bir örnekte iletileri veya olayları yayar bir satış noktası (POS) çözümünü göz önünde bulundurun. Hizmet veri yolu konusu olaylar bazı eşleme veya başka bir işleme için başka bir sistem için eşlenen veri ileten çözümü yeniden biçimlendirme geçirir. Bu noktada, tüm bu sistemleri aynı Azure bölgesinde barındırılabilir. Karar ne zaman ve hangi bölümü üzerinden vermesine altyapınızdaki veri akışını bağlıdır. 

Yük devretme sistemleri izleme ile ya da özel olarak geliştirilmiş izleme çözümleriyle otomatik hale getirebilirsiniz. Ancak, bu tür Otomasyon ek planlama ve bu makalenin kapsamı dışında olan iş alır.

### <a name="failover-flow"></a>Yük devretme akışı

Yük devretme'ı başlattığınızda, iki adım gerekli değildir:

1. Başka bir kesinti oluşursa yeniden yük devretme kullanabilmek ister. Bu nedenle, başka bir pasif ad alanı ayarlama ve eşleştirme güncelleştirin. 

2. Yeniden kullanılabilir olduğunda eski birincil ad alanından iletileri çeker. Bundan sonra normal coğrafi kurtarma kurulumunuzu dışında ileti için bu ad alanını kullanmak veya eski birincil ad alanını silin.

> [!NOTE]
> Yalnızca hata iletme semantiği desteklenir. Bu senaryoda, yük devri ve yeni bir ad alanı ile yeniden eşleştirin. Geri başarısız desteklenmiyor; Örneğin, bir SQL kümesinde. 

![2][]

## <a name="management"></a>Yönetim

Hata yaptıysanız; Örneğin, yanlış bölgeler ilk kurulum sırasında eşlenmiş, herhangi bir anda iki ad alanlarını eşleştirme bölün. Eşleştirilmiş ad alanları normal ad kullanmak istiyorsanız, diğer adı silin.

## <a name="use-existing-namespace-as-alias"></a>Var olan ad alanı diğer ad olarak kullanın

Üreticileri ve tüketicileri bağlantıların değiştiremezsiniz bir senaryo varsa, diğer ad ad alanı adınızı yeniden kullanabilirsiniz. Bkz: [örnek buraya kodu github'da](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR_existing_namespace_name).

## <a name="samples"></a>Örnekler

[Örnekler github'da](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/) ayarlamak ve yük devretme işlemi gösterilmektedir. Bu örnekleri aşağıdaki kavramları göstermektedir:

- .Net örnek ve Azure Azure Resource Manager ile Service Bus kurulumunda ve coğrafi olağanüstü durum kurtarmayı etkinleştirmek için Active Directory'de gerekli ayarları.
- Örnek kod yürütmek için gerekli adımlar.
- Var olan bir ad alanı diğer ad olarak kullanma
- Bunun yerine PowerShell veya CLI aracılığıyla coğrafi olağanüstü durum kurtarmayı etkinleştirmek için adımlar.
- [Gönderme ve alma](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1) diğer adı kullanarak geçerli birincil veya ikincil ad.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu sürüm ile göz önünde bulundurmanız gereken aşağıdaki konuları göz önünde bulundurun:

1. Planınızı yük devretme zaman faktörü de dikkate almalısınız. Örneğin, 15-20 dakikadan fazla bağlantısı kesilirse, yük devretme başlatın karar verebilirsiniz. 
 
2. Hiçbir veri çoğaltılır olgu şu anda etkin oturumları değil çoğaltıldığından emin anlamına gelir. Ayrıca, yinelenen algılama ve zamanlanmış iletileri çalışmayabilir. Yeni oturumlar, yeni zamanlanmış iletileri ve yeni yinelenen çalışır. 

3. Karmaşık bir dağıtılmış altyapı yapabilmesini olmalıdır [prova](/azure/architecture/resiliency/disaster-recovery-azure-applications#disaster-simulation) en az bir kez. 

4. Varlıkları eşitleme dakika başına yaklaşık 50-100 varlık biraz zaman alabilir. Abonelikler ve kuralları da varlıklar sayısı. 

## <a name="next-steps"></a>Sonraki adımlar

- Coğrafi olağanüstü durum kurtarma Bkz [burada REST API Başvurusu](/rest/api/servicebus/disasterrecoveryconfigs).
- Coğrafi olağanüstü durum kurtarma işlemi Çalıştır [github'da örnek](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR2).
- Coğrafi olağanüstü durum kurtarma Bkz [bir diğer ad iletileri gönderir örnek](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1).

Service Bus Mesajlaşma hizmeti hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
* [REST API'si](/rest/api/servicebus/) 

[1]: ./media/service-bus-geo-dr/geo1.png
[2]: ./media/service-bus-geo-dr/geo2.png
