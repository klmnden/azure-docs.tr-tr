---
title: Azure IOT hub'ı yüksek kullanılabilirlik ve olağanüstü durum kurtarma | Microsoft Docs
description: Yüksek oranda kullanılabilir Azure IOT çözümleri ile olağanüstü durum kurtarma özellikleri oluşturmanıza yardımcı olan Azure ve IOT hub'ı özelliklerini açıklar.
author: fsautomata
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/13/2017
ms.author: elioda
ms.openlocfilehash: 992c42511f7bc9e9af71ff552ee91bc2472ebcf8
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39185712"
---
# <a name="iot-hub-high-availability-and-disaster-recovery"></a>IOT hub'ı yüksek kullanılabilirlik ve olağanüstü durum kurtarma
Bir Azure hizmeti IOT hub'ı, çözüm için gerekli olan herhangi bir ek çalışma yapmadan Azure bölgesi düzeyinde fazlalıkları kullanarak yüksek kullanılabilirlik (HA) sağlar. Microsoft Azure platformu, olağanüstü durum kurtarma (DR) özellikleri veya bölgeler arası kullanılabilirlik ile çözümler oluşturmanıza yardımcı olacak özellikler de içerir. Genel sağlamak istiyorsanız, bölgeler arası yüksek kullanılabilirlik için cihazlara veya kullanıcılara, bu Azure DR özelliklerden yararlanın. Makaleyi [Azure iş sürekliliği teknik rehberlik](../resiliency/resiliency-technical-guidance.md) iş sürekliliği ve olağanüstü durum kurtarma için azure'da yerleşik özellikler açıklanmaktadır. [Olağanüstü durum kurtarma ve Azure uygulamaları için yüksek kullanılabilirlik] [ Disaster recovery and high availability for Azure applications] kağıt, HA ve DR elde etmek, Azure uygulamaları için stratejileri hakkında mimari rehberlik sağlar.

## <a name="azure-iot-hub-dr"></a>Azure IOT hub'ı DR
Bölge içi HA ek olarak, IOT hub'ı olağanüstü durum kurtarma için hiçbir kullanıcı müdahalesi gerekli yük devretme mekanizmaları uygular. IOT hub'ı DR Self başlatılır ve bir kurtarma süresi hedefi (RTO) 2-26 saat ve aşağıdaki kurtarma noktası hedeflerine (RPO'lar) vardır:

| İşlev | RPO |
| --- | --- |
| Kayıt defteri ve iletişim işlemlerini yönelik hizmet kullanılabilirliği |CName kaybı |
| Kimlik kayıt defterinde kimlik verilerini |0-5 dakika veri kaybı |
| Cihazdan buluta iletiler |Okunmamış iletileri kaybolur |
| İşlem izleme iletileri |Okunmamış iletileri kaybolur |
| Bulut-cihaz iletilerini |0-5 dakika veri kaybı |
| Bulut-cihaz geri bildirim kuyruğu |Okunmamış iletileri kaybolur |
| Cihaz ikizi verileri |0-5 dakika veri kaybı |
| Üst ve cihaz işleri |0-5 dakika veri kaybı |

## <a name="regional-failover-with-iot-hub"></a>IOT Hub ile bölgesel yük devretme
IOT çözümlerinde dağıtım topolojilerinin eksiksiz bir işlemden bu makalenin kapsamı dışında ' dir. Açıklanır makale *bölgesel yük devretme* yüksek kullanılabilirlik ve olağanüstü durum kurtarma amacıyla dağıtım modeli.

Bölgesel yük devretme modelinde, çözüm geri öncelikle tek bir veri merkezi konumda son çalışır. Bir IOT hub'ı ikincil ve arka uç farklı bir veri merkezi konumuna dağıtılır. IOT hub birincil veri merkezinde bir kesinti alternatife veya CİHAZDAN birincil veri merkezindeki ağ bağlantısı kesildi, ikincil bir hizmet uç noktası cihazları kullanın. Tek bir bölgede kaldığını yerine bölgeler arası yük devretme model uygulayarak çözüm kullanılabilirlik iyileştirebilir. 

Yüksek düzeyde, IOT Hub ile bölgesel yük devretme modeli uygulamak için aşağıdakiler gerekir:

* **İkincil bir IOT hub ve cihaz mantıksal yönlendirme**: Hizmet, birincil bölgedeki kesintiye uğrarsa, aygıtlar, ikincil bir bölgeye bağlanma başlatmanız gerekir. Durumu algılayan dahil çoğu Hizmetleri göz önünde bulundurulduğunda, bölgeler arası yük devretme işlemi tetiklemek çözüm Yöneticiler için yaygındır. İşlemin denetimini elde tutarak cihazlara, yeni uç nokta iletişim kurmak için en iyi yolu bunları düzenli olarak kontrol sahip olmaktır bir *concierge* geçerli etkin uç noktası için hizmet. Concierge hizmetini kopyalandığı ve erişilebilir kalmasını bir web uygulaması olabilir DNS-yeniden yönlendirme teknikleri kullanarak (örneğin, kullanarak [Azure Traffic Manager][Azure Traffic Manager]).
* **Kimlik kayıt defteri çoğaltma**: kullanılabilir olması için ikincil IOT hub'ı çözümüne bağlayabilirsiniz tüm cihaz kimlikleri içermelidir. Çözümün coğrafi olarak çoğaltılmış yedekleme cihaz kimliklerini tutmak ve ikincil IOT hub'ına cihazlar için etkin bir uç nokta geçmeden önce bunları yüklemek gerekir. IOT hub cihaz kimliği dışarı aktarma işlevi bu bağlamda yararlı olur. Daha fazla bilgi için [IOT Hub Geliştirici Kılavuzu - kimlik kayıt defteri][IoT Hub developer guide - identity registry].
* **Mantıksal birleştirme**: birincil bölge tekrar kullanılabilir duruma geldiğinde, tüm ikincil sitede oluşturulan verileri ve durum geçirilmelidir birincil bölgeye geri. Bu durum ve verileri, cihaz kimliklerini ve birincil IOT hub'ı ve birincil bölgedeki başka bir uygulamaya özgü depoları ile birleştirilmelidir uygulama meta verileri için çoğunlukla ilgilidir. Bu adım basitleştirmek için kere etkili olan işlemler kullanmalısınız. Idempotent işlemlerinin yan etkileri nihai tutarlı dağıtım olayların ve yinelemeleri ya da olayların sırası teslimini en aza indirin. Ayrıca, uygulama mantığı, olası tutarsızlıklar ya da "biraz" güncel durum tolerans tasarlanmalıdır. Bu durum, sistemin "kurtarma noktası hedeflerini (RPO) tabanlı onarımı" gereken ek süre nedeniyle ortaya çıkabilir.

## <a name="next-steps"></a>Sonraki adımlar
Azure IOT Hub hakkında daha fazla bilgi için aşağıdaki bağlantıları izleyin:

* [IOT Hubs (eğitim) ile çalışmaya başlama][lnk-get-started]
* [Azure IOT Hub nedir?][What is Azure IoT Hub?]

[Disaster recovery and high availability for Azure applications]: ../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md
[Azure Business Continuity Technical Guidance]: https://azure.microsoft.com/documentation/articles/resiliency-technical-guidance/
[Azure Traffic Manager]: https://azure.microsoft.com/documentation/services/traffic-manager/
[IoT Hub developer guide - identity registry]: iot-hub-devguide-identity-registry.md

[lnk-get-started]: quickstart-send-telemetry-dotnet.md
[What is Azure IoT Hub?]: about-iot-hub.md
