---
title: "Azure IOT hub'ı yüksek kullanılabilirlik ve olağanüstü durum kurtarma | Microsoft Docs"
description: "Yüksek oranda kullanılabilir Azure çözümlere olağanüstü durum kurtarma özellikleri oluşturmak için yardımcı Azure ve IOT hub'ı özellikleri açıklar."
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: ae320e58-aa20-45b9-abdc-fa4faae8e6dd
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: elioda
ms.openlocfilehash: 3ea10ee8652dc2a03791feb66041431e7b3c6ae1
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2017
---
# <a name="iot-hub-high-availability-and-disaster-recovery"></a>IOT hub'ı yüksek kullanılabilirlik ve olağanüstü durum kurtarma
Bir Azure hizmeti IOT hub'ı çözümü tarafından gerekli herhangi bir ek iş olmadan Azure bölgesi düzeyinde açarken kullanarak yüksek kullanılabilirlik (HA) sağlar. Microsoft Azure platformu da olağanüstü durum kurtarma (DR) özelliklerini veya çapraz bölge kullanılabilirlik çözümleri oluşturmanıza yardımcı olmak için özellikler içerir. Genel sağlamak istiyorsanız, çapraz bölge yüksek kullanılabilirlik aygıtların veya kullanıcıların, bu Azure DR özelliklerden alın. Makaleyi [Azure iş sürekliliği teknik kılavuz](../resiliency/resiliency-technical-guidance.md) iş sürekliliği ve DR için Azure içinde yerleşik özellikler açıklanmaktadır. [Azure uygulamaları için yüksek kullanılabilirlik ve olağanüstü durum kurtarma] [ Disaster recovery and high availability for Azure applications] kağıt HA ve DR elde etmek Azure uygulamalarını stratejileri hakkında mimari yönergeleri sağlar.

## <a name="azure-iot-hub-dr"></a>Azure IOT hub'ı DR
İçi bölge HA ek olarak, IOT hub'ı olağanüstü durum kurtarma için hiçbir kullanıcı müdahalesi gerekli yük devretme mekanizmaları uygular. IOT Hub DR otomatik olarak başlatılır ve bir kurtarma süresi hedefi (RTO) 2-26 saat ve aşağıdaki kurtarma noktası hedefi (RPO'lar) vardır:

| İşlev | RPO |
| --- | --- |
| Kayıt defteri ve iletişim işlemleri için hizmet kullanılabilirliği |CName kaybolma |
| Kimlik verilerini kimlik kayıt defterinde |0-5 dakika veri kaybı |
| Cihazdan buluta iletiler |Okunmamış iletileri kaybolur |
| İleti izleme işlemleri |Okunmamış iletileri kaybolur |
| Bulut-cihaz iletilerini |0-5 dakika veri kaybı |
| Bulut cihaz geri bildirim kuyruğu |Okunmamış iletileri kaybolur |

## <a name="regional-failover-with-iot-hub"></a>IOT Hub ile bölgesel yük devretme
Dağıtım topolojileri IOT çözümlerinde tam işlenmesi bu makalenin kapsamı dışındadır ' dir. Makalede ele *bölgesel yük devretme* yüksek kullanılabilirlik ve olağanüstü durum kurtarma amacıyla dağıtım modeli.

Bölgesel yük devretme modelinde, çözüm son çalıştığında öncelikle bir konumdaki datacenter yedekleyin. Başka bir veri merkezi konumda ikincil IOT hub ve arka uç dağıtılır. IOT hub'ı birincil veri merkezinde bir kesinti yükselmesine veya birincil veri merkezindeki ağ bağlantısını aygıttan kesintiye, ikincil hizmet uç noktası cihazları kullanın. Tek bir bölge içinde kaldığını yerine bir çapraz bölge yük devretme modeli uygulayarak çözüm kullanılabilirlik artırabilir. 

Yüksek düzeyde, IOT Hub ile bölgesel yük devretme modeli uygulamak için aşağıdakiler gerekir:

* **İkincil bir IOT hub ve cihaz mantığı yönlendirme**: birincil bölgenizdeki hizmet kesintiye uğrarsa, aygıtlar, ikincil bölge'ye bağlanma başlatmanız gerekir. Çoğu Hizmetleri söz konusu durumu algılayan yapısını verildiğinde, çözüm yöneticileri arası bölge yük devretme işlemi tetiklemek için yaygın bir sorundur. Bunları düzenli olarak denetlemek için işlemin denetimi korurken cihazlara, yeni uç nokta iletişim kurmak için en iyi yolu olan bir *Danışman* geçerli etkin uç noktası için hizmet. Danışman hizmetinin çoğaltılır ve ulaşılabilir tutulan bir web uygulaması olabilir DNS-yeniden yönlendirme teknikleri kullanarak (örneğin, kullanarak [Azure Traffic Manager][Azure Traffic Manager]).
* **Kimlik kayıt defteri çoğaltma**: kullanılabilmesi için ikincil IOT hub'ı çözümüne bağlayabilirsiniz tüm cihaz kimliklerini içermesi gerekir. Çözüm cihaz kimliklerini coğrafi olarak çoğaltılmış yedeklerini tutmak ve aygıtlar için etkin uç nokta değiştirmeden önce ikincil IOT hub'ına karşıya yüklemeniz gerekir. IOT hub cihaz kimlik dışa aktarma işlevi bu bağlamda yararlıdır. Daha fazla bilgi için bkz: [IOT Hub Geliştirici Kılavuzu - kimlik kayıt defteri][IoT Hub developer guide - identity registry].
* **Mantıksal birleştirme**: birincil bölge yeniden kullanılabilir duruma geldiğinde, tüm durum ikincil sitede oluşturulan veri gerekir geçirilen ve birincil bölge yedekleyin. Bu durum ve verileri cihaz kimliklerini ve birincil IOT hub'ı ve birincil bölge içinde başka bir uygulamaya özgü depoları ile birleştirilmelidir uygulama meta verileri genellikle ilişkilidir. Bu adım basitleştirmek için ıdempotent işlemleri kullanmanız gerekir. Idempotent işlemleri yan etkileri olayların nihai tutarlı dağıtım ve yinelemeleri ya da olayların sırası teslimini en aza indirin. Ayrıca, olası tutarsızlıklar veya "biraz" güncel durumda tolerans için uygulama mantığını tasarlanmalıdır. Bu durum, "kurtarma noktası hedefi (RPO) üzerinde dayalı olarak onarma" sisteme geçen ek süre nedeniyle oluşabilir.

## <a name="next-steps"></a>Sonraki adımlar
Azure IOT Hub hakkında daha fazla bilgi için aşağıdaki bağlantıları izleyin:

* [(Öğretici) IOT hub ile çalışmaya başlama][lnk-get-started]
* [Azure IOT Hub nedir?][What is Azure IoT Hub?]

[Disaster recovery and high availability for Azure applications]: ../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md
[Azure Business Continuity Technical Guidance]: https://azure.microsoft.com/documentation/articles/resiliency-technical-guidance/
[Azure Traffic Manager]: https://azure.microsoft.com/documentation/services/traffic-manager/
[IoT Hub developer guide - identity registry]: iot-hub-devguide-identity-registry.md

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[What is Azure IoT Hub?]: iot-hub-what-is-iot-hub.md
