---
title: "Azure IoT Hub'a genel bakış | Microsoft Belgeleri"
description: "Azure IoT Hub hizmetine genel bakış: IoT Hub, cihaz bağlantısı, nesnelerin interneti iletişim düzenleri, ağ geçitleri ve hizmet destekli iletişim düzeni nedir?"
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 24376318-5344-4a81-a1e6-0003ed587d53
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/29/2018
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b00c89183ff0d4e7df49d29834508643e68246bb
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="overview-of-the-azure-iot-hub-service"></a>Azure IoT Hub hizmetine genel bakış

Azure IoT Hub'a hoş geldiniz. Bu makale Azure IoT Hub'a genel bir bakış sağlar ve bir Nesnelerin İnterneti (IoT) çözümü uygularken bu hizmeti neden kullanmanız gerektiğini açıklar. Azure IoT Hub, milyonlarca IoT cihazları ile bir çözüm arka ucu arasında güvenilir ve güvenli çift yönlü iletişimler sağlayan tam olarak yönetilen bir hizmettir. Azure IoT Hub:

* Birden fazla cihaz-bulut ve bulut-cihaz iletişim seçeneği sunar. Bu seçenekler tek yönlü mesajlaşmayı, dosya aktarımını ve istek-yanıt yöntemlerini içerir.
* Diğer Azure hizmetlerine yerleşik bildirim temelli ileti yönlendirme sunar.
* Cihaz meta verileri ve eşitlenmiş durum bilgileri için sorgulanabilir depolama sağlar.
* Cihaz başına güvenlik anahtarları veya X.509 sertifikaları kullanarak güvenli iletişim ve erişim denetimi olanağı sağlar.
* Cihaz bağlantısı ve cihaz kimlik yönetimi etkinlikleri için kapsamlı izleme sağlar.
* En popüler diller ve platformlar için cihaz kitaplıklarını içerir.

[IoT Hub ile Event Hubs Karşılaştırması][lnk-compare] makalesi, bu iki hizmet arasındaki önemli farklılıkları açıklar ve IoT çözümlerinizde IoT Hub'ı kullanmanın avantajlarını vurgular.

Azure ve IoT Hub'ın, IoT çözümünüzün güvenliğini sağlamanıza nasıl yardımcı olduğu hakkında daha fazla bilgi için bkz. [Her yönüyle Nesnelerin İnterneti güvenliği][lnk-security-ground-up].

![Nesnelerin interneti çözümünde bulut ağ geçidi olarak Azure IoT Hub][img-architecture]

> [!NOTE]
> IoT mimarisinin ayrıntılı incelemesi için bkz. [Microsoft Azure IoT Başvuru Mimarisi][lnk-refarch].

## <a name="iot-device-connectivity-challenges"></a>IoT cihaz bağlantısı zorlukları

IoT Hub ve cihaz kitaplıkları, cihazların düzgün ve güvenli bir şekilde çözüm arka ucuna bağlanmasındaki zorlukları aşabilmenize yardımcı olur. IoT cihazları:

* İnsan olan bir operatörü bulunmayan ve genellikle katıştırılmış sistemlerdir.
* Fiziksel erişimin pahalı olduğu uzak konumlarda olabilir.
* Yalnızca çözüm arka ucu aracılığıyla erişilebilir.
* Sınırlı güç ve işleme kaynaklarına sahip olabilir.
* Aralıklı, yavaş veya pahalı bir ağ bağlantısına sahip olabilir.
* Mülkiyete ait, özel veya sektöre özgü uygulama protokolleri kullanması gerekebilir.
* Büyük bir popüler donanım ve yazılım platformu kümesi kullanılarak oluşturulabilir.

Yukarıdaki gereksinimlere ek olarak, tüm IoT çözümlerinin ölçek, güvenlik ve güvenilirlik de sunması gerekir. Elde edilen bağlantı gereksinimleri kümesi, web kapsayıcıları ve mesajlaşma aracıları gibi geleneksel teknolojiler kullandığınızda uygulaması zor ve zaman alıcı olabilir.

## <a name="why-use-azure-iot-hub"></a>Azure IoT Hub neden kullanılır?

Azure IoT Hub birden fazla [cihaz-bulut][lnk-d2c-guidance] ve [bulut-cihaz][lnk-c2d-guidance] iletişim seçeneği sunar. Azure IoT Hub ayrıca cihazlara aşağıdaki şekilde bağlantı kurulduğunda ortaya çıkan güvenilirlik ve güvenlik sorunlarına çözüm getirir:

* **Cihaz çiftleri**. [Cihaz çiftlerini][lnk-twins] kullanarak depolama, eşitleme, cihaz meta verilerini ve durum bilgisini sorgulama işlemlerini yapabiliriniz. Cihaz çiftleri; meta veriler, yapılandırmalar ve koşullar gibi cihaz durumu bilgilerini depolayan JSON belgelerdir. IoT Hub'ı, IoT Hub'ına bağladığınız her cihaz için bir cihaz çifti tutar.

* **Cihaz başına kimlik doğrulaması ve güvenli bağlantı**. IoT Hub'a bağlanmalarını sağlamak için her bir cihaza kendi [güvenlik anahtarını][lnk-devguide-security] sağlayabilirsiniz. [IoT Hub kimlik kayıt defteri][lnk-devguide-identityregistry], cihaz kimliklerini ve anahtarlarını bir çözümün içinde depolar. Bir çözüm arka ucu, cihazları tek tek izin verme veya reddetme listesine ekleyerek cihaz erişiminde tam denetim olanağı sağlar.

* **Bildirim temelli kurallara göre cihazdan buluta iletileri Azure hizmetlerine yönlendirme**. IoT Hub, hub'ınızın cihazdan buluta iletileri göndereceği yeri denetlemek için yönlendirme kurallarına göre ileti yolları tanımlamanızı sağlar. Yönlendirme kuralları için kod yazmanız gerekmez ve bu kurallar, özel alma sonrası ileti dağıtıcılarının yerini alabilir.

* **IoT Hub olaylarını iş uygulamalarınızla tümleştirin**. IoT Hub, Azure Event Grid ile tümleşir. Bu tümleştirmeyi, IoT Hub olaylarını dinlemek amacıyla diğer Azure hizmetlerini veya üçüncü taraf uygulamalarını yapılandırmak için kullanın. Azure Event Grid sayesinde önemli olaylara güvenilir, ölçeklenebilir ve güvenli bir şekilde tepki verebilirsiniz.

* **Cihaz bağlantısı işlemlerini izleme**. Cihaz kimlik yönetimi işlemleri ve cihaz bağlantısı etkinlikleri hakkında ayrıntılı işlem günlükleri alabilirsiniz. Bu izleme özelliği IoT çözümünüzün bağlantı sorunlarını tanımlamasını sağlar. Bu günlükleri kullanarak yanlış kimlik bilgileri ileten veya çok sık ileti gönderen cihazları tanımlayabilir veya tüm bulut-cihaz iletilerini reddedebilirsiniz.

* **Kapsamlı bir cihaz kitaplıkları kümesi**. [Azure IoT cihaz SDK'ları][lnk-device-sdks], Linux dağıtımları, Windows ve gerçek zamanlı işletim sistemlerinin çoğu için C gibi çeşitli diller ve platformlarda kullanılabilir ve desteklenir. Azure IoT cihaz SDK'ları C#, Java ve JavaScript gibi yönetilen dilleri de destekler.

* **IoT protokolleri ve genişletilebilirlik**. Çözümünüz cihaz kitaplıklarını kullanamazsa IoT Hub, cihazların yerel olarak MQTT v3.1.1, HTTPS 1.1 veya AMQP 1.0 protokollerini kullanmasını sağlayan bir genel protokolü kullanıma sunar. Ayrıca özel protokollere destek sağlamak için şu yollarla IoT Hub'ı genişletebilirsiniz:

  * Özel protokolünüzü IoT Hub tarafından algılanan bir protokole dönüştüren [Azure IoT Edge][lnk-iot-edge] ile bir alan ağ geçidi oluşturarak.
  * Bulutta çalışan açık kaynaklı bir bileşen olan [Azure IoT protokolü ağ geçidini][protocol-gateway] özelleştirerek.

* **Ölçek**. Azure IoT Hub, saniye başına milyonlarca eş zamanlı cihazı ve milyonlarca etkinliği ölçeklendirir.

* **Cihaz sağlama**. [IoT Hub Cihazı Sağlama Hizmeti](https://docs.microsoft.com/azure/iot-dps/) doğru IoT hub’a, kullanıcı müdahalesi gerektirmeden otomatik olarak tam zamanında sağlama olanağı sunan bir yardımcı hizmettir. Bu sayede milyonlarca cihazı güvenli ve ölçeklenebilir bir şekilde sağlayabilirsiniz.

## <a name="gateways"></a>Ağ geçitleri

Bir IoT çözümündeki ağ geçidi genellikle buluta dağıtılan bir [protokol ağ geçidi][lnk-iotedge] veya cihazlarınızla yerel olarak dağıtılan bir [alan ağ geçidi][lnk-field-gateway] olur.

_Protokol ağ geçidi_, MQTT veya AMQP gibi protokol çevirisi gerçekleştirir.

_Alan ağ geçidi_ şunları yapabilir:

* Uçta analiz çalıştırma.
* Gecikme süresini azaltmak için zamana duyarlı kararlar alma.
* Cihaz yönetim hizmetleri sağlama.
* Güvenlik ve gizlilik kısıtlamaları zorunlu kılma.
* Protokol çevirileri gerçekleştirme.

Her iki ağ geçidi de cihazlarınız ve IoT Hub'ınız arasında aracı görevi yapar.

Bir alan ağ geçidi, çözümünüzde erişim ve bilgi akışını yönetmede genellikle etkin bir rol oynadığından, basit bir trafik yönlendirme cihazından (bir ağ adresi çeviri cihazı veya güvenlik duvarı gibi) farklıdır.

Bir çözümde hem protokol hem de alan geçitleri olabilir.

## <a name="how-does-iot-hub-work"></a>IoT Hub nasıl çalışır?

Azure IoT Hub, cihazlarınız ve çözüm arka ucunuz arasındaki etkileşimlere aracılık etmek için [hizmet destekli iletişim][lnk-service-assisted-pattern] deseni uygular. Desenin amacı, IoT Hub gibi bir denetim sistemi ile güvenilmeyen fiziksel alanlardaki özel amaçlı cihazlar arasında güvenilir ve çift yönlü iletişim yolları oluşturmaktır. Desen aşağıdaki ilkeleri oluşturur:

* Güvenlik diğer tüm işlevlerden önceliklidir.

* Cihazlar istenmemiş ağ bilgilerini kabul etmez. Bir cihaz tüm bağlantıları kurar ve yalnızca giden bağlantı şeklinde yönlendirir. Bir cihazın çözüm arka ucundan bir komut alması için, işlenmeyi bekleyen herhangi bir komut olup olmadığını denetlemek üzere cihazın düzenli olarak bağlantı başlatması gerekir.

* Cihazlar, eşlendikleri hizmetlerden yalnızca IoT Hub gibi iyi bilinenlere bağlanmalı veya yönlendirme oluşturmalıdır.

* Cihaz ile hizmet veya ağ geçidi arasındaki iletişim yolunun güvenliği, uygulama protokol katmanında sağlanır.

* Sistem düzeyinde yetkilendirme ve kimlik doğrulama cihaz başına kimliği temel alır. Bunlar erişim kimlik bilgilerini ve izinleri neredeyse anında iptal edilebilir hale getirir.

* Güç veya bağlantı sorunları nedeniyle belirli aralıklarla bağlanan cihazlar için çift yönlü iletişim, komutların ve bildirimlerin bir cihazın bunları almak için bağlanmasına kadar tutularak gerçekleştirilir. IoT Hub, cihazın gönderdiği komutlar için cihaza özgü kuyruklar oluşturur.

* Uygulama yük verilerinin güvenliği, ağ geçitleri aracılığıyla belirli bir hizmete korumalı geçiş için ayrı ayrı sağlanır.

Mobil sektörü, [Windows Anında Bildirim Servisi][lnk-wns], [Google Cloud Messaging][lnk-google-messaging] ve [Apple Anında İletilen Bildirim Servisi][lnk-apple-push] gibi anında iletme bildirimi hizmetlerini uygulamak amacıyla hizmet destekli iletişim düzenini kullanmaktadır.

IoT Hub, ExpressRoute'un ortak eşleme yolundan desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

Bazı kodları yazmaya ve bazı örnekleri çalıştırmaya başlamak için [IoT Hub ile çalışmaya başlama][lnk-get-started] öğreticisine bakın.

Bir cihazdan ileti göndermenin ve bu iletileri IoT Hub'dan almanın yanı sıra ileti yollarını nasıl yapılandıracağınızı öğrenmek için bkz. [IoT Hub ile ileti gönderme ve alma][lnk-send-messages].

Cihazlarınızı uzaktan yönetmeniz, yapılandırmanız ve güncelleştirmeniz için IoT Hub'ın standartlara dayalı cihaz yönetimini nasıl etkinleştirdiği hakkında bilgi almak üzere bkz. [IoT Hub ile cihaz yönetimine genel bakış][lnk-device-management].

İstemci uygulamalarını çok sayıda cihaz donanımı platformunda ve işletim sisteminde uygulamak için Azure IoT cihaz SDK'larını kullanabilirsiniz. Cihaz SDK'ları, bir IoT hub'ına telemetri gönderme ve buluttan cihaza iletilerini alma işlemlerini gerçekleştiren kitaplıkları içerir. Cihaz SDK'larını kullandığınızda, IoT Hub ile iletişim kurmak için çeşitli ağ protokolleri arasından seçim yapabilirsiniz. Daha fazla bilgi için bkz. [Cihaz SDK'ları hakkında bilgi][lnk-device-sdks].

[img-architecture]: media/iot-hub-what-is-iot-hub/hubarchitecture.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[protocol-gateway]: https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md
[lnk-service-assisted-pattern]: http://blogs.msdn.com/b/clemensv/archive/2014/02/10/service-assisted-communication-for-connected-devices.aspx "Hizmet Destekli İletişim, Clemens Vasters'ın blog yazısı"
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-iotedge]: iot-hub-protocol-gateway.md
[lnk-field-gateway]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-devguide-identityregistry]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-wns]: https://msdn.microsoft.com/library/windows/apps/mt187203.aspx
[lnk-google-messaging]: https://developers.google.com/cloud-messaging/
[lnk-apple-push]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-iot-edge]: https://docs.microsoft.com/azure/iot-edge/
[lnk-send-messages]: iot-hub-devguide-messaging.md
[lnk-device-management]: iot-hub-device-management-overview.md

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[lnk-security-ground-up]: iot-hub-security-ground-up.md
