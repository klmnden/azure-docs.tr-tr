---
title: Coğrafi olağanüstü durum kurtarma - Azure Event Hubs | Microsoft Docs
description: Yük devretme için coğrafi bölgeler kullanın ve Azure Event hubs'ı olağanüstü durum kurtarma gerçekleştirmek nasıl
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: a1dafb8e4c16a59bfed51016ce9ccb0ec3eb7d6c
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66754758"
---
# <a name="azure-event-hubs---geo-disaster-recovery"></a>Azure Event Hubs - coğrafi olağanüstü durum kurtarma 

Tüm durumlarda Azure bölgeleri veya veri merkezleri (hiçbir [kullanılabilirlik alanları](../availability-zones/az-overview.md) kullanılır), kesinti yaşamak veri işleme, farklı bir bölge veya veri merkezi içinde çalışmaya devam etmek önemlidir. Bu nedenle, *coğrafi olağanüstü durum kurtarma* ve *coğrafi çoğaltma* tüm kuruluş için önemli özelliklerdir. Azure Event Hubs, hem coğrafi olağanüstü durum kurtarma ve coğrafi çoğaltma, ad alanı düzeyinde destekler. 

Coğrafi olağanüstü durum kurtarma özelliği, standart Event Hubs hem de adanmış SKU için genel olarak kullanılabilir. Aynı SKU katmanı üzerinden yalnızca coğrafi çifti ad alanları için lütfen unutmayın. Yalnızca bizim ayrılmış SKU'da sunulur bir kümedeki bir ad alanı varsa, örneğin, yalnızca bir ad alanındaki başka bir küme ile eşlenebilmesi için. 

## <a name="outages-and-disasters"></a>Kesintileri ve olağanüstü durumları yönetme

"Kesintiler" ve "olağanüstü durumlar" arasındaki fark dikkate almak önemlidir Bir *kesinti* olan Azure Event Hubs'ın geçici olarak kullanım dışı kalması ve bazı bileşenleri gibi bir Mesajlaşma deposu veya hatta veri merkezinin tamamı hizmete etkileyebilir. Sorun düzeltildikten sonra Bununla birlikte, Event Hubs yeniden kullanılabilir hale gelir. Genellikle, bir kesinti iletileri veya diğer veri kaybına neden olmaz. Böyle bir kesinti örneği, veri merkezinde güç kesintisi olabilir. Bazı kesintiler yalnızca kısa bağlantı kayıpları geçici ya da ağ sorunları nedeniyle olduğundan. 

A *olağanüstü durum* bir olay hub'ları kümesi, Azure bölgesi veya veri merkezi kalıcı ya da daha uzun vadeli zarar tanımlanır. Bölge veya veri merkezi olabilir veya yeniden kullanılabilir olmaktan çıkabilir veya saatler veya günler için aşağı sonuna olmayabilir. Ateş, taşmasını veya deprem gibi olağanüstü örnekleridir. Kalıcı hale olağanüstü bir durum, bazı iletileri, olayları ya da diğer veri kaybına neden olabilir. Ancak, çoğu durumda olması gerekir veri kaybı olmadan ve yedekleme veri merkezi olduktan sonra iletileri kurtarılabilir.

Azure Event Hubs'ın coğrafi olağanüstü durum kurtarma özelliği bir olağanüstü durum kurtarma çözümüdür. Bu makalede açıklanan iş akışı ve kavramları olağanüstü durum senaryoları ve geçici ve geçici kesintiler için geçerlidir. Microsoft azure'da olağanüstü durum kurtarma hakkında ayrıntılı bilgi için bkz: [bu makalede](/azure/architecture/resiliency/disaster-recovery-azure-applications).

## <a name="basic-concepts-and-terms"></a>Temel kavramlar ve terimler

Olağanüstü Durum Kurtarma özelliği meta verileri olağanüstü durum kurtarma uygular ve birincil ve ikincil olağanüstü durum kurtarma ad alanlarında kullanır. Coğrafi olağanüstü durum kurtarma özelliği için kullanılabilir olduğunu unutmayın [standart SKU](https://azure.microsoft.com/pricing/details/event-hubs/) yalnızca. Bir diğer ad üzerinden bağlantı kurulmadığı herhangi bir bağlantı dizesi değişiklik yapılması gerekmez.

Bu makalede aşağıdaki terimler kullanılır:

-  *Diğer ad*: Ayarladığınız bir olağanüstü durum kurtarma Yapılandırması adı. Diğer ad, tek bir kararlı tam etki alanı adı (FQDN) bağlantı dizesi sağlar. Uygulamaları, bir ad alanına bağlanmak için bu diğer ad bağlantı dizesi kullanır. 

-  *Birincil/ikincil ad alanı*: Diğer adı için karşılık gelen ad alanları. Birincil ad "etkin" ve iletileri (Bu, mevcut veya yeni bir ad olabilir) alır. İkincil ad alanı, "pasif" ve iletileri almaz. Her ikisi arasındaki bir meta veri eşitlenmiş olarak olduğundan her ikisi de sorunsuz bir şekilde uygulama kodu veya bağlantı dizesi değişiklik yapmadan iletileri kabul edebilir. Etkin ad alanı iletileri aldığından emin olmak için diğer adı kullanmanız gerekir. 

-  *meta veri*: Event hubs ve tüketici grupları gibi varlıkları; ve hizmetin ad alanıyla ilişkili özellikleri. Varlıklar ve ayarları otomatik olarak çoğaltılır unutmayın. İletileri ve olayları çoğaltılmaz. 

-  *Yük devretme*: İkincil ad alanı etkinleştiriliyor işlemi.

## <a name="setup-and-failover-flow"></a>Kurulum ve yük devretme akışı

Aşağıdaki bölümde, yük devretme işlemini bir genel bakıştır ve ilk devretmeyi açıklanmaktadır. 

![1][]

### <a name="setup"></a>Kurulum

İlk oluşturun veya var olan bir birincil ad alanı ve yeni bir ikincil ad alanı kullanın ve sonra iki eşleştirin. Bu eşleştirme bağlanmak için kullanabileceğiniz bir diğer ad verir. Bir diğer ad kullandığından, bağlantı dizelerini değiştirmek gerekmez. Yeni ad alanları, yük devretme çifti için eklenebilir. Son olarak, bazı bir yük devretme gerekli olup olmadığını algılamak için izleme eklemeniz gerekir. Çoğu durumda, hizmet geniş ekosistem bir parçası olduğundan, çok sık yük devretmeleri kalan alt sistemi veya altyapı ile eşitleme gerçekleştirilmelidir. böylece otomatik yük devretme işlemleri nadiren mümkün olduğundan.

### <a name="example"></a>Örnek

Bu senaryonun bir örneği, iletileri ya da olayların yayan bir satış noktası (POS) çözümünü göz önünde bulundurun. Event hubs'ı, sonra daha fazla işleme için başka bir sistem için eşlenmiş veri ileten bazı eşleştirme veya yeniden biçimlendirme çözüme olaylar geçirir. Bu noktada, tüm bu sistemlerin aynı Azure bölgesinde barındırılabilir. Karar ne zaman ve hangi bölümünün yük devretme altyapınızdaki veri akışını bağlıdır. 

Yük devretme ile izleme sistemlerinden veya özel olarak geliştirilmiş izleme çözümleri ile otomatik hale getirebilirsiniz. Ancak, bu tür Otomasyon ek planlama ve bu makalenin kapsamı dışında olan iş alır.

### <a name="failover-flow"></a>Yük devretme akışı

Yük devretmeyi başlatın, iki adım gereklidir:

1. Başka bir kesinti oluşursa, yeniden yük devretme yönetebilmek istiyorsunuz. Bu nedenle, başka bir pasif ad alanı ayarlama ve eşleştirmesini güncelleştirin. 

2. Yeniden kullanılabilir olduğunda eski birincil ad alanındaki iletileri çekin. Bundan sonra bu ad alanı normal coğrafi kurtarma kurulumunuzu dışında Mesajlaşma için kullanın veya eski birincil ad alanı silin.

> [!NOTE]
> Yalnızca hata iletme semantiği desteklenir. Bu senaryoda, yük devretme ve ardından yeni bir ad alanı ile yeniden eşleştirin. İlk duruma döndürmeden desteklenmiyor; Örneğin SQL kümesi. 

![2][]

## <a name="management"></a>Yönetim

Bir hata yaptıysanız; Örneğin, ilk kurulum sırasında yanlış bölgede eşleştirilmiş, iki ad alanlarını herhangi bir zamanda eşleştirme bozabilir. Eşleştirilen ad alanları normal bir ad kullanmak istiyorsanız, diğer silin.

## <a name="samples"></a>Örnekler

[Örneğine Github'dan](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/GeoDRClient) ayarlama ve bir yük devretme başlatma işlemi gösterilmektedir. Bu örnek aşağıdaki kavramları göstermektedir:

- Event Hubs ile Azure Resource Manager'ı kullanmak için Azure Active Directory'de gerekli ayarları. 
- Örnek kod yürütmek için gerekli adımlar. 
- Geçerli birincil ad alanından gönderip yeniden açın. 

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu sürümle birlikte göz önünde tutmak için aşağıdaki konuları göz önünde bulundurun:

1. Planınızı yük devretme saat faktörü düşünmelisiniz. Örneğin, 15-20 dakikadan fazla bağlantısını kaybederseniz, yük devretmeyi başlatmak karar verebilirsiniz. 
 
2. Hiçbir veri çoğaltılır şu anda etkin oturumları değil çoğaltıldığından emin anlamına gelir. Ayrıca, yinelenen algılama ve zamanlanmış iletileri çalışmayabilir. Yeni oturumlar, zamanlanan mesajlar ve yeni yinelenen çalışır. 

3. Karmaşık dağıtılmış bir altyapı yük devrediliyor olmalıdır [prova](/azure/architecture/reliability/disaster-recovery#disaster-recovery-plan) en az bir kez. 

4. Varlık eşitleme dakika başına yaklaşık 50-100 varlık biraz zaman alabilir.

## <a name="availability-zones"></a>Kullanılabilirlik Alanları 

Event Hubs standart SKU'nun destekler [kullanılabilirlik](../availability-zones/az-overview.md), sağlayan bir Azure bölgesi içinde hatadan yalıtılmış konumlardır. 

> [!NOTE]
> Kullanılabilirlik alanları desteği için standart Azure Event Hubs yalnızca kullanılabilir [Azure bölgeleri](../availability-zones/az-overview.md#services-support-by-region) kullanılabilirlik nerede bulunduğunu.

Kullanılabilirlik alanları, yeni ad alanları üzerinde yalnızca, Azure portalını kullanarak etkinleştirebilirsiniz. Olay hub'ları, var olan ad alanlarının geçişini desteklemez. Bölge artıklığı ad alanınızı etkinleştirildikten sonra devre dışı bırakılamıyor.

![3][]

## <a name="next-steps"></a>Sonraki adımlar

* [Örneğine Github'dan](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/GeoDRClient) coğrafi eşleştirme oluşturan ve başlatan bir olağanüstü durum kurtarma senaryosu için bir yük devretme basit bir iş akışı gösterilmektedir.
* [REST API Başvurusu](/rest/api/eventhub/disasterrecoveryconfigs) coğrafi olağanüstü durum kurtarma yapılandırması gerçekleştirmek için API'leri açıklar.

Event Hubs hakkında daha fazla bilgi için şu bağlantıları ziyaret edin:

* [Event Hubs öğreticisi](event-hubs-dotnet-standard-getstarted-send.md) ile çalışmaya başlayın
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
* [Event Hubs kullanan örnek uygulamalar](https://github.com/Azure/azure-event-hubs/tree/master/samples)

[1]: ./media/event-hubs-geo-dr/geo1.png
[2]: ./media/event-hubs-geo-dr/geo2.png
[3]: ./media/event-hubs-geo-dr/eh-az.png
