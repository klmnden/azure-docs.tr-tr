<properties
 pageTitle="Azure IoT Hub'a genel bakış | Microsoft Azure"
 description="Azure IoT Hub hizmetine genel bakış: iot hub'ı, cihaz bağlantısı, nesnelerin interneti iletişim düzenleri ve hizmet destekli iletişim düzeni nedir?"
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/25/2016"
 ms.author="dobett"/>


# Azure IoT Hub nedir?

Azure IoT Hub'a hoş geldiniz. Bu makale Azure IoT Hub'a genel bir bakış sağlar ve bir Nesnelerin İnterneti (IoT) çözümü uygularken bu hizmeti neden kullanmanız gerektiğini açıklar. Azure IoT Hub, milyonlarca IoT cihazları ile bir çözüm arka ucu arasında güvenilir ve güvenli çift yönlü iletişimler sağlayan tam olarak yönetilen bir hizmettir. Azure IoT Hub:

- Cihazdan buluta ve buluttan cihaza ölçekli şekilde güvenilir mesajlaşma sağlar.
- Cihaz başına güvenlik kimlik bilgisi ve erişim denetimi kullanarak güvenli iletişimlere olanak sağlar.
- Cihaz bağlantısı ve cihaz kimlik yönetimi etkinlikleri için kapsamlı izleme sağlar.
- En popüler diller ve platformlar için cihaz kitaplıklarını içerir.

[IoT Hub ile Event Hubs Karşılaştırması][lnk-compare] makalesi, bu iki hizmet arasındaki önemli farklılıkları açıklar ve IoT çözümlerinizde IoT Hub'ı kullanmanın avantajlarını vurgular.

![Nesnelerin interneti çözümünde bulut ağ geçidi olarak Azure IoT Hub][img-architecture]

> [AZURE.NOTE] IoT mimarisinin ayrıntılı incelemesi için bkz. [Microsoft Azure IoT Başvuru Mimarisi][lnk-refarch].

## IoT cihaz bağlantısı zorlukları

IoT Hub ve cihaz kitaplıkları, cihazların düzgün ve güvenli bir şekilde çözüm arka ucuna bağlanmasındaki zorlukları aşabilmenize yardımcı olur. IoT cihazları:

- İnsan olan bir operatörü bulunmayan ve genellikle katıştırılmış sistemlerdir.
- Fiziksel erişimin pahalı olduğu uzak konumlarda olabilir.
- Yalnızca çözüm arka ucu aracılığıyla erişilebilir.
- Sınırlı güç ve işleme kaynaklarına sahip olabilir.
- Aralıklı, yavaş veya pahalı bir ağ bağlantısına sahip olabilir.
- Mülkiyete ait, özel veya sektöre özgü uygulama protokolleri kullanması gerekebilir.
- Büyük bir popüler donanım ve yazılım platformu kümesi kullanılarak oluşturulabilir.

Yukarıdaki gereksinimlere ek olarak, tüm IoT çözümlerinin ölçek, güvenlik ve güvenilirlik de sunması gerekir. Elde edilen bağlantı gereksinimleri kümesi, web kapsayıcıları ve mesajlaşma aracıları gibi geleneksel teknolojiler kullandığınızda uygulaması zor ve zaman alıcı olabilir.

## Azure IoT Hub neden kullanılır?

Azure IoT Hub aşağıdaki yollarla cihaz bağlantısı sorunlarını ele alır:

-   **Cihaz başına kimlik doğrulaması ve güvenli bağlantı**. IoT Hub'a bağlanmalarını sağlamak için her bir cihaza kendi [güvenlik anahtarını][lnk-devguide-security] sağlayabilirsiniz. [IoT Hub kimlik kayıt defteri][lnk-devguide-identityregistry], cihaz kimliklerini ve anahtarlarını bir çözümün içinde depolar. Bir çözüm arka ucu, cihazları tek tek izin verme veya reddetme listesine ekleyerek cihaz erişiminde tam denetim olanağı sağlar.

-   **Cihaz bağlantısı işlemlerini izleme**. Cihaz kimlik yönetimi işlemleri ve cihaz bağlantısı etkinlikleri hakkında ayrıntılı işlem günlükleri alabilirsiniz. Bu izleme olanağı, IoT çözümünüzün yanlış kimlik bilgileriyle bağlanmaya, çok sık ileti göndermeye veya tüm bulut-cihaz iletilerini reddetmeye çalışan cihazlar gibi bağlantı sorunlarını tanımlaması sağlanır.

-   **Kapsamlı bir cihaz kitaplıkları kümesi**. [Azure IoT cihaz SDK'ları][lnk-device-sdks], birçok Linux dağıtımı, Windows ve gerçek zamanlı işletim sistemleri için C gibi çeşitli diller ve platformlarda kullanılabilir ve desteklenir. Azure IoT cihaz SDK'ları C#, Java ve JavaScript gibi yönetilen dilleri de destekler.

-   **IoT protokolleri ve genişletilebilirlik**. Çözümünüz cihaz kitaplıklarını kullanamazsa IoT Hub, cihazların yerel olarak MQTT v3.1.1, HTTP 1.1 veya AMQP 1.0 protokollerini kullanmasını sağlayan bir genel protokolü kullanıma sunar. Ayrıca, özel protokollere destek sağlamak için şunları yaparak IoT Hub'ı genişletebilirsiniz:

    - Özel protokolünüzü IoT Hub tarafından anlaşılan üç protokolden birine dönüştüren [Azure IoT Ağ Geçidi SDK'sı][lnk-gateway-sdk] ile bir alan ağ geçidi oluşturarak. 
    - Bulutta çalışan açık kaynaklı bir bileşen olan [Azure IoT protokolü ağ geçidini][protocol-gateway] özelleştirerek.

-   **Ölçek**. Azure IoT Hub, saniye başına milyonlarca eş zamanlı cihazı ve milyonlarca etkinliği ölçeklendirir.

Bu avantajlar birçok iletişim deseni için geneldir. Şu anda IoT Hub aşağıdaki belirli iletişim düzenlerini uygulamanıza olanak sağlar:

-   **Etkinlik tabanlı cihazdan buluta alım.** IoT Hub, cihazlarınızdan saniye başına milyonlarca etkinliği düzgün bir şekilde alabilir. Ardından, bir etkinlik işlemcisi altyapısını kullanarak bunları etkin yolunuzda işleyebilir. Ayrıca, analiz için bunları etkin olmayan yolunuzda depolayabilir. IoT Hub, güvenilir işlemeyi garantilemek ve yükteki en yüksek noktaları almak amacıyla etkinlik verilerini yedi güne kadar saklar.

-   **Güvenilir buluttan cihaza mesajlaşma (veya *komutlar*).** Çözüm arka ucu, tek cihazlara en az bir kere teslim garantisiyle ileti göndermek için IoT Hub kullanabilir. Her bir iletinin ayrı bir yaşam süresi ayarı bulunur ve arka uç hem teslim hem de sonra erme girişi isteğinde bulunabilir. Bu girişler, bir buluttan cihaza iletinin yaşam döngüsünde tam görünürlük sağlar. Ardından, cihazlarda çalışan işlemleri içeren iş mantığını uygulayabilirsiniz.

-   **Dosyaları ve önbelleğe alınan sensör verilerini buluta yükleyin.** Cihazlarınız, IoT Hub tarafından sizin için yönetilen SAS URI'lerini kullanarak Azure Storage'a dosya yükleyebilir. IoT Hub, dosyalar buluta geldiğinde bunların arka uç tarafından işlenmelerini sağlamak için bildirimler oluşturabilir.

## Ağ geçitleri

Bir IoT çözümündeki bir ağ geçidi genellikle buluta dağıtılan bir [protokol ağ geçidi][lnk-gateway] veya cihazlarınızla yerel olarak dağıtılan bir [alan ağ geçidi][lnk-field-gateway] olur. Bir protokol ağ geçidi, MQTT veya AMQP gibi protokol çevirisi gerçekleştirir. Bir alan ağ geçidi kenarda analiz çalıştırabilir, gecikme süresini düşüren, cihaz yönetimi hizmetleri sağlayan, güvenlik ve gizlilik kısıtlamaları getiren zamana duyarlı kararlar verebilir ve protokol çevirileri gerçekleştirebilir. Her iki ağ geçidi de cihazlarınız ve IoT hub'ınız arasında aracı görevi yapar.

Bir alan ağ geçidi, çözümünüzde erişim ve bilgi akışını yönetmede genellikle etkin bir rol oynadığından, basit bir trafik yönlendirme cihazından (bir ağ adresi çeviri cihazı veya güvenlik duvarı gibi) farklıdır.

Bir çözümde hem protokol hem de alan geçitleri olabilir.

## IoT Hub nasıl çalışır?

Azure IoT Hub, cihazlarınız ve çözüm arka ucunuz arasındaki etkileşimlere aracılık etmek için [hizmet destekli iletişim][lnk-service-assisted-pattern] deseni uygular. Hizmet destekli iletişimin amacı, IoT Hub gibi bir denetim sistemi ile güvenilmeyen fiziksel alanlarda dağıtılan özel amaçlı cihazlar arasında güvenilir ve çift yönlü iletişim yolları oluşturmaktır. Desen aşağıdaki ilkeleri oluşturur:

- Güvenlik diğer tüm işlevlerden önceliklidir.
- Cihazlar istenmemiş ağ bilgilerini kabul etmez. Bir cihaz tüm bağlantıları kurar ve yalnızca giden bağlantı şeklinde yönlendirir. Bir cihazın arka uçtan bir komut alması için, işlenmeyi bekleyen herhangi bir komut olup olmadığını denetlemek üzere cihazın düzenli olarak bağlantı başlatması gerekir.
- Cihazlar, eşlendikleri hizmetlerden yalnızca IoT Hub gibi iyi bilinenlere bağlanmalı veya yönlendirme oluşturmalıdır.
- Cihaz ile hizmet veya cihaz ile ağ geçidi arasındaki iletişim yolunun güvenliği, uygulama protokol katmanında sağlanır.
- Sistem düzeyinde yetkilendirme ve kimlik doğrulama cihaz başına kimliği temel alır. Bunlar erişim kimlik bilgilerini ve izinleri neredeyse anında iptal edilebilir hale getirir.
- Güç veya bağlantı sorunları nedeniyle belirli aralıklarla bağlanan cihazlar için çift yönlü iletişim, komutların ve cihaz bildirimlerinin bir cihazın bunları almak için bağlanmasına kadar tutularak gerçekleştirilir. IoT Hub, cihazın gönderdiği komutlar için cihaza özgü kuyruklar oluşturur.
- Uygulama yük verilerinin güvenliği, ağ geçitleri aracılığıyla belirli bir hizmete korumalı geçiş için ayrı ayrı sağlanır.

Mobil sektörü, [Windows Anında İletilen Bildirim Servisi][lnk-wns], [Google Cloud Messaging][lnk-google-messaging] ve [Apple Anında İletilen Bildirim Servisi][lnk-apple-push] gibi anında iletilen bildirim hizmetlerini uygulamak için hizmet destekli iletişim düzenini devasa ölçüde kullanmaktadır.

## Sonraki adımlar

Cihazlarınızı uzaktan yönetmeniz, yapılandırmanız ve güncelleştirmeniz için Azure IoT Hub’ın standartlara dayalı IoT cihaz yönetimini nasıl etkinleştirdiği hakkında bilgi almak için bkz. [Azure IoT Hub cihaz yönetimine genel bakış][lnk-device-management].

Çok çeşitli cihaz donanım platformları ve işletim sistemlerinde istemci uygulamalarını uygulamak için IoT cihaz SDK'larını kullanabilirsiniz. IoT cihaz SDK'ları, bir IoT hub'ına telemetri göndermeyi ve bulut-cihaz komutlarını almayı gerçekleştiren kitaplıkları içerir. SDK'ları kullandığınızda IoT Hub ile iletişim kurmak için çeşitli ağ protokolleri arasından seçim yapabilirsiniz. Daha fazla bilgi için bkz. [Cihaz SDK'ları hakkında bilgi][lnk-device-sdks].

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
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-device-management]: iot-hub-device-management-overview.md



<!--HONumber=Oct16_HO1-->


