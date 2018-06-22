---
title: Azure Event Hubs coğrafi olağanüstü durum kurtarma | Microsoft Docs
description: Coğrafi bölgeler için yük devretme kullanın ve Azure Event Hubs olağanüstü durum kurtarma gerçekleştirmek nasıl
services: event-hubs
documentationcenter: ''
author: sethmanheim
manager: timlt
editor: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2018
ms.author: sethm
ms.openlocfilehash: 0192f65f394a3bb6d5cffc90639966b5f913b291
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36302122"
---
# <a name="azure-event-hubs-geo-disaster-recovery"></a>Azure Event Hubs coğrafi olağanüstü durum kurtarma

Tüm zaman Azure bölgeleri veya veri merkezleri (yoksa [kullanılabilirlik bölgeleri](../availability-zones/az-overview.md) kullanılır), sürecek farklı bölge veya veri merkezi içinde çalışmaya devam etmek veri işleme için önemlidir. Bu nedenle, *coğrafi olağanüstü durum kurtarma* ve *coğrafi çoğaltma* tüm kuruluş için önemli özelliklerdir. Azure Event Hubs coğrafi olağanüstü durum kurtarma ve coğrafi çoğaltma, ad alanı düzeyinde destekler. 

Coğrafi olağanüstü durum kurtarma özelliği, olay hub'ları standart SKU için genel olarak kullanılabilir.

## <a name="outages-and-disasters"></a>Kesintiler ve olağanüstü durumları

"Kesintileri" ve "afetler." arasında ayrım dikkate almak önemlidir Bir *kesinti* geçici olarak kullanım dışı kalması Azure Event Hubs olduğu ve bazı bileşenleri bir Mesajlaşma deposu veya hatta tüm veri merkezi gibi hizmet etkileyebilir. Sorun düzeltildikten sonra Bununla birlikte, olay hub'ları yeniden kullanılabilir hale gelir. Genellikle, bir kesinti iletileri veya diğer veri kaybına neden olmaz. Bu tür bir kesinti örneği veri merkezinde bir güç kesintisi olabilir. Bazı kesintiler yalnızca kısa bağlantı zararları geçici ya da ağ sorunları nedeniyle ' dir. 

A *olağanüstü durum* kalıcı ya da daha uzun vadeli kaybı bir olay hub'ları küme, Azure bölgesi veya veri merkezi tanımlanmış. Bölge veya veri merkezi olabilir veya yeniden kullanılabilir olmaktan veya aşağı saatlerce veya günlerce olması olabilir. Bu tür afetler yangın, taşmasını veya deprem gösterilebilir. Kalıcı hale bir olağanüstü durum bazı iletiler, olayları ya da diğer veri kaybına neden olabilir. Ancak, çoğu durumda olması gerekir veri kaybı ve yedekleme veri merkezi başladıktan sonra iletileri kurtarılabilir.

Azure Event Hubs coğrafi olağanüstü durum kurtarma özelliği bir olağanüstü durum kurtarma çözümüdür. Bu makalede açıklanan iş akışı ve kavramları olağanüstü durum senaryoları ve değil geçici ya da geçici kesintileri uygulanır. Microsoft Azure olağanüstü durum kurtarma hakkında ayrıntılı bilgi için bkz: [bu makalede](/azure/architecture/resiliency/disaster-recovery-azure-applications).

## <a name="basic-concepts-and-terms"></a>Temel kavramlar ve terimler

Olağanüstü Durum Kurtarma özelliği meta verileri olağanüstü durum kurtarma uygular ve birincil ve ikincil olağanüstü durum kurtarma ad alanında bulunan dayanır. Coğrafi olağanüstü durum kurtarma özelliği için kullanılabilir olduğunu unutmayın [standart SKU](https://azure.microsoft.com/pricing/details/event-hubs/) yalnızca. Bağlantı bir diğer ad üzerinden yapılan gibi herhangi bir bağlantı dizesi değişiklik yapılması gerekmez.

Bu makalede aşağıdaki terimler kullanılır:

-  *Diğer ad*: sizin ayarladığınız bir olağanüstü durum kurtarma yapılandırması için ad. Diğer adı tek bir tutarlı tam etki alanı adı (FQDN) bağlantı dizesini sağlar. Uygulamalar, bir ad alanına bağlanmak için bu diğer ad bağlantı dizesini kullanın. 

-  *Birincil/ikincil ad alanı*: diğer adı için karşılık gelen ad alanları. Birincil ad "etkin" (Bu, mevcut veya yeni bir ad olabilir) iletileri ve alır. İkincil ad "pasif" ve iletileri almaz. Her ikisi arasındaki bir meta veri eşitleme, olduğundan her ikisi de sorunsuz olarak hiçbir uygulama kodu veya bağlantı dizesi değişiklik yapmadan iletileri kabul edebilir. Yalnızca etkin ad iletileri aldığından emin olmak için diğer adı kullanmanız gerekir. 

-  *Meta veri*: olay hub'ları ve tüketici grupları; ve ad alanınızla ilişkilendirilen özelliklerinin hizmetinin gibi varlıklar. Yalnızca varlıkları ve ayarlarını otomatik olarak çoğaltılır unutmayın. İletileri ve olayları çoğaltılmaz. 

-  *Yük devretme*: ikincil ad alanı etkinleştirme işlemi.

## <a name="setup-and-failover-flow"></a>Kurulum ve yük devretme akışı

Aşağıdaki bölümde yük devretme işlemini bir genel bakıştır ve ilk yük devretmeyi ayarlanacağı açıklanmaktadır. 

![1][]

### <a name="setup"></a>Kurulum

İlk oluşturmak veya var olan bir birincil ad alanını ve yeni bir ikincil ad alanı kullanın ve sonra iki eşleştirin. Bu eşleştirme bağlanmak için kullanabileceğiniz bir diğer ad sağlar. Bir diğer ad kullandığından, bağlantı dizelerinizi değiştirmeniz gerekmez. Yalnızca yeni ad alanları, yük devretme çifti için eklenebilir. Son olarak, bazı bir yük devretme gerekli olup olmadığını algılamak için izleme eklemeniz gerekir. Çoğu durumda, hizmet büyük bir ekosistem bir parçası, bu nedenle çok sık yerine kalan alt sistemi veya altyapı ile eşitleme gerçekleştirilmelidir gibi otomatik yük devretme işlemlerini nadiren mümkün.

### <a name="example"></a>Örnek

Bu senaryo bir örnekte iletileri veya olayları yayar bir satış noktası (POS) çözümünü göz önünde bulundurun. Olay hub'ları, başka bir işleme için başka bir sistem için eşlenen veri ileten bazı eşleştirme veya yeniden biçimlendirme çözüme olayları geçirir. Bu noktada, tüm bu sistemleri aynı Azure bölgesinde barındırılabilir. Karar ne zaman ve hangi bölümü üzerinden vermesine altyapınızdaki veri akışını bağlıdır. 

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

## <a name="samples"></a>Örnekler

[Github'da örnek](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/GeoDRClient) ayarlamak ve yük devretme işlemi gösterilmektedir. Bu örnek aşağıdaki kavramları göstermektedir:

- Azure Active Directory'de Azure Resource Manager Event Hubs ile kullanmak için gerekli ayarları. 
- Örnek kod yürütmek için gerekli adımlar. 
- Geçerli birincil ad alanından gönderip yeniden açın. 

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu sürüm ile göz önünde bulundurmanız gereken aşağıdaki konuları göz önünde bulundurun:

1. Planınızı yük devretme zaman faktörü de dikkate almalısınız. Örneğin, 15-20 dakikadan fazla bağlantısı kesilirse, yük devretme başlatın karar verebilirsiniz. 
 
2. Hiçbir veri çoğaltılır olgu şu anda etkin oturumları değil çoğaltıldığından emin anlamına gelir. Ayrıca, yinelenen algılama ve zamanlanmış iletileri çalışmayabilir. Yeni oturumlar, zamanlanmış iletileri ve yeni yinelenen çalışır. 

3. Karmaşık bir dağıtılmış altyapı yapabilmesini olmalıdır [prova](/azure/architecture/resiliency/disaster-recovery-azure-applications#disaster-simulation) en az bir kez. 

4. Varlıkları eşitleme dakika başına yaklaşık 50-100 varlık biraz zaman alabilir.

## <a name="availability-zones-preview"></a>Kullanılabilirlik bölgeler (Önizleme)

Olay hub'ları standart SKU da destekler [kullanılabilirlik bölgeleri](../availability-zones/az-overview.md), bir Azure bölgesi hataya yalıtılmış konumlara sağlama. 

> [!NOTE]
> Kullanılabilirlik bölgeleri Önizleme yalnızca desteklenen **Orta ABD**, **Doğu ABD 2**, ve **Fransa Merkezi** bölgeleri.

Azure Portalı'nı kullanarak yeni ad alanında bulunan yalnızca kullanılabilirlik bölgeleri etkinleştirebilirsiniz. Olay hub'ları, var olan ad alanlarının geçişini desteklemez. Bölge artıklık ad alanınıza etkinleştirdikten sonra devre dışı bırakılamıyor.

![3][]

## <a name="next-steps"></a>Sonraki adımlar

* [Github'da örnek](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/GeoDRClient) coğrafi çifti oluşturur ve bir olağanüstü durum kurtarma senaryosunda bir yük devretme başlatır basit bir iş akışı anlatılmaktadır.
* [REST API Başvurusu](/rest/api/eventhub/disasterrecoveryconfigs) coğrafi olağanüstü durum kurtarma yapılandırması gerçekleştirmek için API'ları açıklar.

Event Hubs hakkında daha fazla bilgi için şu bağlantıları ziyaret edin:

* [Event Hubs öğreticisi](event-hubs-dotnet-standard-getstarted-send.md) ile çalışmaya başlayın
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
* [Event Hubs kullanan örnek uygulamalar](https://github.com/Azure/azure-event-hubs/tree/master/samples)

[1]: ./media/event-hubs-geo-dr/geo1.png
[2]: ./media/event-hubs-geo-dr/geo2.png
[3]: ./media/event-hubs-geo-dr/eh-az.png