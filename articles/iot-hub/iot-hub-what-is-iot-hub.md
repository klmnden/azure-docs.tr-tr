---
title: "Azure IoT Hub&quot;a genel bakış | Microsoft Belgeleri"
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
ms.date: 05/02/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 5b5792d2edee2069c7d021415632511643d68136
ms.contentlocale: tr-tr
ms.lasthandoff: 05/10/2017


---
# <a name="overview-of-the-azure-iot-hub-service"></a>Azure IoT Hub hizmetine genel bakış
Azure IoT Hub'a hoş geldiniz. Bu makale Azure IoT Hub'a genel bir bakış sağlar ve bir Nesnelerin İnterneti (IoT) çözümü uygularken bu hizmeti neden kullanmanız gerektiğini açıklar. Azure IoT Hub, milyonlarca IoT cihazları ile bir çözüm arka ucu arasında güvenilir ve güvenli çift yönlü iletişimler sağlayan tam olarak yönetilen bir hizmettir. Azure IoT Hub:

* Tek yönlü mesajlaşma, dosya aktarımı ve request-reply metotları gibi cihazdan buluta ve buluttan cihaza iletişimi seçeneği sunar.
* Diğer Azure hizmetlerine yerleşik bildirim temelli ileti yönlendirme sunar.
* Cihaz meta verileri ve eşitlenmiş durum bilgileri için sorgulanabilir depolama sağlar.
* Cihaz başına güvenlik anahtarları veya X.509 sertifikaları kullanarak güvenli iletişim ve erişim denetimi olanağı sağlar.
* Cihaz bağlantısı ve cihaz kimlik yönetimi etkinlikleri için kapsamlı izleme sağlar.
* En popüler diller ve platformlar için cihaz kitaplıklarını içerir.

[IoT Hub ile Event Hubs Karşılaştırması][lnk-compare] makalesi, bu iki hizmet arasındaki önemli farklılıkları açıklar ve IoT çözümlerinizde IoT Hub'ı kullanmanın avantajlarını vurgular.

Azure ve IoT Hub'ının, IoT çözümünüzün güvenliğini sağlamanıza nasıl yardımcı olduğu hakkında daha fazla bilgi için bkz. [Internet of Things security from the ground up (Her yönüyle Nesnelerin İnterneti güvenliği)][lnk-security-ground-up].

![Nesnelerin interneti çözümünde bulut ağ geçidi olarak Azure IoT Hub][img-architecture]

> [!NOTE]
> IoT mimarisinin ayrıntılı incelemesi için bkz. [Microsoft Azure IoT Başvuru Mimarisi][lnk-refarch].
> 
> 

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
Azure IoT Hub, cihaz bağlantısı sorunlarını mesajlaşma, dosya aktarımı ve request-reply metotları gibi çok sayıda [cihazdan buluta][lnk-d2c-guidance] ve [buluttan cihaza][lnk-c2d-guidance] iletişim seçeneğine ek olarak, aşağıdaki yollarla ele alır:

* **Cihaz çiftleri**. [Cihaz çiftlerini][lnk-twins] kullanarak depolama, eşitleme, cihaz meta verilerini ve durum bilgisini sorgulama işlemlerini yapabiliriniz. Cihaz çiftleri, cihaz durumu bilgilerini (meta veriler, yapılandırmalar ve koşullar) depolayan JSON belgelerdir. IoT Hub’ı, IoT Hub'ına bağladığınız her cihaz için bir cihaz çifti sürdürür. 
* **Cihaz başına kimlik doğrulaması ve güvenli bağlantı**. IoT Hub'a bağlanmalarını sağlamak için her bir cihaza kendi [güvenlik anahtarını][lnk-devguide-security] sağlayabilirsiniz. [IoT Hub kimlik kayıt defteri][lnk-devguide-identityregistry], cihaz kimliklerini ve anahtarlarını bir çözümün içinde depolar. Bir çözüm arka ucu, cihazları tek tek izin verme veya reddetme listesine ekleyerek cihaz erişiminde tam denetim olanağı sağlar.
* **Bildirim temelli kurallara göre cihazdan buluta iletileri Azure hizmetlerine yönlendirme**. IoT Hub, hub'ınızın cihazdan buluta iletileri göndereceği yeri denetlemek için yönlendirme kurallarına göre ileti yolları tanımlamanızı sağlar. Yönlendirme kuralları için kod yazmanız gerekmez ve bu kurallar, özel alma sonrası ileti dağıtıcılarının yerini alabilir.
* **Cihaz bağlantısı işlemlerini izleme**. Cihaz kimlik yönetimi işlemleri ve cihaz bağlantısı etkinlikleri hakkında ayrıntılı işlem günlükleri alabilirsiniz. Bu izleme olanağı, IoT çözümünüzün yanlış kimlik bilgileriyle bağlanmaya, çok sık ileti göndermeye veya tüm bulut-cihaz iletilerini reddetmeye çalışan cihazlar gibi bağlantı sorunlarını tanımlaması sağlanır.
* **Kapsamlı bir cihaz kitaplıkları kümesi**. [Azure IoT cihaz SDK'ları][lnk-device-sdks], Linux dağıtımları, Windows ve gerçek zamanlı işletim sistemlerinin çoğu için C gibi çeşitli diller ve platformlarda kullanılabilir ve desteklenir. Azure IoT cihaz SDK'ları C#, Java ve JavaScript gibi yönetilen dilleri de destekler.
* **IoT protokolleri ve genişletilebilirlik**. Çözümünüz cihaz kitaplıklarını kullanamazsa IoT Hub, cihazların yerel olarak MQTT v3.1.1, HTTP 1.1 veya AMQP 1.0 protokollerini kullanmasını sağlayan bir genel protokolü kullanıma sunar. Ayrıca, özel protokollere destek sağlamak için şunları yaparak IoT Hub'ı genişletebilirsiniz:
  
  * Özel protokolünüzü IoT Hub tarafından anlaşılan üç protokolden birine dönüştüren [Azure IoT Edge][lnk-gateway-sdk] ile bir alan ağ geçidi oluşturarak. 
  * Bulutta çalışan açık kaynaklı bir bileşen olan [Azure IoT protokolü ağ geçidini][protocol-gateway] özelleştirerek.
* **Ölçek**. Azure IoT Hub, saniye başına milyonlarca eş zamanlı cihazı ve milyonlarca etkinliği ölçeklendirir.

## <a name="gateways"></a>Ağ geçitleri
Bir IoT çözümündeki ağ geçidi genellikle buluta dağıtılan bir [protokol ağ geçidi][lnk-gateway] veya cihazlarınızla yerel olarak dağıtılan bir [alan ağ geçidi][lnk-field-gateway] olur. Bir protokol ağ geçidi, MQTT veya AMQP gibi protokol çevirisi gerçekleştirir. Bir alan ağ geçidi kenarda analiz çalıştırabilir, gecikme süresini düşüren, cihaz yönetimi hizmetleri sağlayan, güvenlik ve gizlilik kısıtlamaları getiren zamana duyarlı kararlar verebilir ve protokol çevirileri gerçekleştirebilir. Her iki ağ geçidi de cihazlarınız ve IoT hub'ınız arasında aracı görevi yapar.

Bir alan ağ geçidi, çözümünüzde erişim ve bilgi akışını yönetmede genellikle etkin bir rol oynadığından, basit bir trafik yönlendirme cihazından (bir ağ adresi çeviri cihazı veya güvenlik duvarı gibi) farklıdır.

Bir çözümde hem protokol hem de alan geçitleri olabilir.

## <a name="how-does-iot-hub-work"></a>IoT Hub nasıl çalışır?
Azure IoT Hub, cihazlarınız ve çözüm arka ucunuz arasındaki etkileşimlere aracılık etmek için [hizmet destekli iletişim][lnk-service-assisted-pattern] deseni uygular. Hizmet destekli iletişimin amacı, IoT Hub gibi bir denetim sistemi ile güvenilmeyen fiziksel alanlarda dağıtılan özel amaçlı cihazlar arasında güvenilir ve çift yönlü iletişim yolları oluşturmaktır. Desen aşağıdaki ilkeleri oluşturur:

* Güvenlik diğer tüm işlevlerden önceliklidir.
* Cihazlar istenmemiş ağ bilgilerini kabul etmez. Bir cihaz tüm bağlantıları kurar ve yalnızca giden bağlantı şeklinde yönlendirir. Bir cihazın çözüm arka ucundan bir komut alması için, işlenmeyi bekleyen herhangi bir komut olup olmadığını denetlemek üzere cihazın düzenli olarak bağlantı başlatması gerekir.
* Cihazlar, eşlendikleri hizmetlerden yalnızca IoT Hub gibi iyi bilinenlere bağlanmalı veya yönlendirme oluşturmalıdır.
* Cihaz ile hizmet veya cihaz ile ağ geçidi arasındaki iletişim yolunun güvenliği, uygulama protokol katmanında sağlanır.
* Sistem düzeyinde yetkilendirme ve kimlik doğrulama cihaz başına kimliği temel alır. Bunlar erişim kimlik bilgilerini ve izinleri neredeyse anında iptal edilebilir hale getirir.
* Güç veya bağlantı sorunları nedeniyle belirli aralıklarla bağlanan cihazlar için çift yönlü iletişim, komutların ve cihaz bildirimlerinin bir cihazın bunları almak için bağlanmasına kadar tutularak gerçekleştirilir. IoT Hub, cihazın gönderdiği komutlar için cihaza özgü kuyruklar oluşturur.
* Uygulama yük verilerinin güvenliği, ağ geçitleri aracılığıyla belirli bir hizmete korumalı geçiş için ayrı ayrı sağlanır.

Mobil sektörü, [Windows Anında Bildirim Servisi][lnk-wns], [Google Cloud Messaging][lnk-google-messaging] ve [Apple Anında İletilen Bildirim Servisi][lnk-apple-push] gibi anında iletme bildirimi hizmetlerini uygulamak amacıyla hizmet destekli iletişim düzenini çok büyük ölçeklerle kullanmaktadır.

IoT Hub, ExpressRoute'un ortak eşleme yolundan desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar
Bir cihazdan ileti göndermenin ve bu iletileri IoT Hub'dan almanın yanı sıra ileti yollarını nasıl yapılandıracağınızı öğrenmek için bkz. [IoT Hub ile ileti gönderme ve alma][lnk-send-messages].

Cihazlarınızı uzaktan yönetmeniz, yapılandırmanız ve güncelleştirmeniz için IoT Hub'ın standartlara dayalı cihaz yönetimini nasıl etkinleştirdiği hakkında bilgi almak üzere bkz. [IoT Hub ile cihaz yönetimine genel bakış][lnk-device-management].

İstemci uygulamalarını çok sayıda cihaz donanımı platformunda ve işletim sisteminde uygulamak için Azure IoT cihaz SDK'larını kullanabilirsiniz. Cihaz SDK'ları, bir IoT hub'ına telemetri gönderme ve buluttan cihaza iletilerini alma işlemlerini gerçekleştiren kitaplıkları içerir. Cihaz SDK'larını kullandığınızda, IoT Hub ile iletişim kurmak için çeşitli ağ protokolleri arasından seçim yapabilirsiniz. Daha fazla bilgi için bkz. [Cihaz SDK'ları hakkında bilgi][lnk-device-sdks].

Bazı kodları yazmaya ve bazı örnekleri çalıştırmaya başlamak için [IoT Hub ile çalışmaya başlama][lnk-get-started] öğreticisine bakın.

[img-architecture]: media/iot-hub-what-is-iot-hub/hubarchitecture.png


[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[protocol-gateway]: https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md
[lnk-service-assisted-pattern]: http://blogs.msdn.com/b/clemensv/archive/2014/02/10/service-assisted-communication-for-connected-devices.aspx "Hizmet Destekli İletişim, Clemens Vasters'ın blog yazısı"
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-gateway]: iot-hub-protocol-gateway.md
[lnk-field-gateway]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-devguide-identityregistry]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-wns]: https://msdn.microsoft.com/library/windows/apps/mt187203.aspx
[lnk-google-messaging]: https://developers.google.com/cloud-messaging/
[lnk-apple-push]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-gateway-sdk]: https://github.com/Azure/iot-edge
[lnk-send-messages]: iot-hub-devguide-messaging.md
[lnk-device-management]: iot-hub-device-management-overview.md

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[lnk-security-ground-up]: iot-hub-security-ground-up.md

