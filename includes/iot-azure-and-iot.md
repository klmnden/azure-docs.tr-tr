---
title: include dosyası
description: include dosyası
services: iot-suite
author: dominicbetts
ms.service: iot-suite
ms.topic: include
ms.date: 04/25/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 1dced2eecafa45b0d172b807dc492942d461de71
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64919030"
---
# <a name="azure-and-the-internet-of-things"></a>Azure ve Nesnelerin İnterneti

Microsoft Azure’a ve Nesnelerin İnterneti’ne (IOT) Hoş Geldiniz. Bu makale, bulutta IoT çözümünün genel özelliklerini açıklar. IoT çözümleri, sayıları milyonları bulabilecek cihazlar arasında güvenli, çift yönlü iletişim ve çözüm arka ucu gerektirir. Örneğin, bir çözüm cihazdan buluta olay akışınızdan bilgileri açığa çıkarmak için otomatik, tahmine dayalı analizleri kullanabilir.

## <a name="iot-solution-architecture"></a>IOT çözüm mimarisi

Aşağıdaki diyagram tipik bir IoT çözüm mimarisinin temel öğelerini göstermektedir. Diyagram, kullanılan Azure hizmetleri ve cihaz işletim sistemleri gibi belirli uygulama ayrıntılarından bağımsızdır. Bu mimaride, IoT cihazları bulut ağ geçidine gönderdikleri verileri toplar. Bulut ağ geçidi, verileri diğer arka uç hizmetleri tarafından işlenebilir hale getirir. Bu arka uç hizmetleri şuralara veri iletebilir:

* Diğer iş kolu uygulamaları.
* Pano veya başka bir sunu cihazı üzerinden insan kullanıcılar.

![IOT çözüm mimarisi][img-solution-architecture]

> [!NOTE]
> IoT mimarisinin ayrıntılı incelemesi için bkz. [Microsoft Azure IoT Başvuru Mimarisi][lnk-refarch].

### <a name="device-connectivity"></a>Cihaz bağlantısı

Bir IoT çözüm mimarisinde cihazlar genellikle depolama ve işleme amacıyla buluta telemetri gönderir. Örneğin, Tahmine dayalı bakım senaryosunda çözüm arka ucu, belirli bir pompanın ne zaman bakıma gerek duyacağını saptamak için sensör verilerinin akışını kullanabilir. Cihazlar, bulut uç noktasına ait iletileri okuyarak buluttan cihaza iletileri de alıp yanıtlayabilir. Aynı örnekte çözüm arka ucu, bakımın başlamasından hemen önce akışların yeniden yönlendirilmesini başlatmak amacıyla pompa istasyonundaki diğer pompalara da ileti gönderebilir. Bu yordam, bakım mühendisinin ulaşır ulaşmaz işe başlamasını sağlar.

IoT çözümlerinde görülen en büyük zorluk genellikle cihazları güvenli ve güvenilir bir şekilde bağlamaktır. Bunun nedeni, tarayıcılar ve mobil uygulamalar gibi diğer istemcilerle karşılaştırıldığında IoT cihazlarında farklı özelliklerin bulunmasıdır. IoT cihazları şu özelliklere sahiptir:

* Genellikle insan kullanıcısı bulunmayan katıştırılmış sistemlerdir (bir telefonun aksine).
* Fiziksel erişimin pahalı olduğu uzak konumlarda dağıtılabilir.
* Yalnızca çözüm arka ucu aracılığıyla erişilebilir. Cihazla etkileşime geçmek için başka bir yol yoktur.
* Sınırlı güç ve işleme kaynaklarına sahip olabilir.
* Aralıklı, yavaş veya pahalı bir ağ bağlantısına sahip olabilir.
* Mülkiyete ait, özel veya sektöre özgü uygulama protokolleri kullanması gerekebilir.
* Büyük bir popüler donanım ve yazılım platformu kümesi kullanılarak oluşturulabilir.

Önceki kısıtlamalara ek olarak, tüm IoT çözümlerinin aynı zamanda ölçeklenebilir, güvenli ve güvenilir olması gerekir.

İletişim protokolüne ve ağ kullanılabilirliğine bağlı olarak, bir cihaz doğrudan veya bir ara ağ geçidi üzerinden bulutla iletişim kurabilir. IoT mimarileri genellikle bu iki iletişim deseninin bir karışımını kullanır.

### <a name="data-processing-and-analytics"></a>Veri işleme ve analizi

Modern IoT çözümlerinde veri işleme, bulutta veya cihaz tarafında gerçekleşebilir. Cihaz tarafında işleme, *Uç bilgi işlem* olarak adlandırılır. Verilerin işleneceği yerle ilgili seçim aşağıdaki gibi etkenlere bağlıdır:

* Ağ kısıtlamaları. Cihazlar ile bulut arasındaki bant genişliği sınırlı ise, daha fazla uç işleme yapmak için teşvik vardır.
* Yanıt süresi. Neredeyse gerçek zamanlı olarak cihaz üzerinde işlem yapma gereksinimi varsa, yanıtı cihazın içinde işlemek daha iyi olabilir. Örneğin, acil durumda durdurulması gereken bir robot kolu.
* Yasal ortam. Bazı veriler buluta gönderilemez.

Genel olarak, hem uç hem de bulutta veri işleme aşağıdaki özelliklerin bir birleşimidir:

* Telemetriyi cihazlarınızdan ölçekli alma ve bu verilerin nasıl işleneceğini ve depolanacağını saptama.
* Gerçek zamanlı veya olaydan sonra olmasına bakılmaksızın öngörüler sağlamak için telemetriyi analiz etme.
* Buluttan veya bir ağ geçidi cihazından belirli bir cihaza komut gönderme.

Ayrıca, bir IoT bulut arka ucu şunları sağlamalıdır:

* Aşağıdakileri yapmanıza olanak tanıyan cihaz kayıt özellikleri:
    * Cihaz sağlama.
    * Altyapınıza bağlanmasına izin verilen cihazları denetleme.
* Cihazlarınızın durumunu denetlemeye ve etkinliklerini izlemenize yönelik cihaz yönetimi.

Örneğin, tahmine dayalı bakım senaryosunda bulut arka ucu geçmiş telemetri verilerini depolar. Çözüm, bu verileri kullanarak belirli pompalardaki anormal davranışları gerçek bir soruna neden olmadan önce tanımlar. Veri analizi kullanarak, önleyici çözümün düzeltici bir eylem gerçekleştirmek için cihaza yeniden komut göndermek olduğunu belirleyebilir. Bu işlem, cihaz ile bulut arasında çözüm verimliliğini önemli ölçüde artıran otomatik bir geri bildirim döngüsü oluşturur.

### <a name="presentation-and-business-connectivity"></a>Sunu ve iş bağlantısı

Sunu ve iş bağlantı katmanı son kullanıcıların IoT çözümü ve cihazlarla etkileşime geçmesini sağlar. Kullanıcıların kendi cihazlarından toplanan verileri görüntülemelerini ve çözümlemelerini sağlar. Bu görünümler panolar veya hem geçmiş verileri, hem de yakın gerçek zamanlı verileri görüntüleyebilen BI raporu biçiminde olabilir. Örneğin, bir kullanıcı belirli bir pompa istasyonunun durumunu denetleyebilir ve sistem tarafından gerçekleştirilen tüm uyarıları görebilir. Bu katman, kurumsal iş süreçlerine veya iş akışlarına bağlanmak üzere var olan iş kolu uygulamalarına sahip IoT çözüm arka ucunun tümleştirilmesini de sağlar. Örneğin, bir Tahmine Dayalı Bakım çözüm hızlandırıcısı bir pompaya bakım gerektiğini tanımladığında mühendisin pompayı ziyaretini ayarlayan zamanlama sistemiyle tümleşebilir.

[img-solution-architecture]: ./media/iot-azure-and-iot/iot-reference-architecture.png
[img-dashboard]: ./media/iot-azure-and-iot/iot-suite.png

[lnk-iot-hub]: ../articles/iot-hub/about-iot-hub.md
[lnk-iot-suite]: ../articles/iot-accelerators/about-iot-accelerators.md
[lnk-machinelearning]: https://azure.microsoft.com/documentation/services/machine-learning/
[Azure IoT solution accelerators]: https://azure.microsoft.com/solutions/iot
[lnk-protocol-gateway]:  ../articles/iot-hub/iot-hub-protocol-gateway.md
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
