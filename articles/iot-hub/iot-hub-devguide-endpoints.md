---
title: Azure IOT Hub uç noktaları anlama | Microsoft Docs
description: Geliştirici Kılavuzu - IOT Hub hakkında başvuru bilgileri cihaz hem de hizmet'e yönelik uç noktalar.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: dobett
ms.openlocfilehash: e6b4ee3425f6a490f33f998cab4f33734b23df22
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46982112"
---
# <a name="reference---iot-hub-endpoints"></a>Başvuru - IOT Hub uç noktaları

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="iot-hub-names"></a>IOT hub'ı adları

Ana bilgisayar adını, uç noktaları, hub'ın şirket Portalı'nda barındıran IOT hub'ı bulabilirsiniz **genel bakış** sayfası. Varsayılan olarak, IOT hub'ının DNS adı şu şekilde görünür: `{your iot hub name}.azure-devices.net`.

Azure DNS, IOT hub'ınız için özel bir DNS adı oluşturmak için kullanabilirsiniz. Daha fazla bilgi için bkz. [Bir Azure hizmeti için özel etki alanı ayarları sağlamak üzere Azure DNS'yi kullanma](../dns/dns-custom-domain.md).

## <a name="list-of-built-in-iot-hub-endpoints"></a>Yerleşik IOT Hub uç noktaları listesi

Azure IOT hub'ı çeşitli aktörler için işlevselliği sunan çok kiracılı bir hizmettir. Aşağıdaki diyagramda, IOT hub'ı göstermesini çeşitli uç noktalarını gösterir.

![IoT Hub uç noktaları][img-endpoints]

Aşağıdaki listede, uç noktalar açıklanmaktadır:

* **Kaynak sağlayıcısı**. IOT hub'ı kaynak sağlayıcısı sunan bir [Azure Resource Manager] [ lnk-arm] arabirimi. Azure abonelik sahipleri oluşturun ve IOT hub'ları silme ve IOT hub'ı özelliklerini güncelleştirmek için bu arabirimi sağlar. IOT hub'ı özelliklerini belirleyen [hub düzeyindeki güvenlik ilkeleri][lnk-accesscontrol], cihaz düzeyinde erişim denetimi ve bulut-cihaz ve CİHAZDAN buluta iletiler için işlevsel seçenekleri yerine. IOT hub'ı kaynak sağlayıcısı ayrıca sayesinde [cihaz kimliklerini dışarı][lnk-importexport].
* **Cihaz Kimlik Yönetimi**. Her IOT hub cihaz kimliklerini yönetme için HTTPS REST uç kümesi sunar (oluşturma, alma, güncelleştirme ve silme). [Cihaz kimliklerini] [ lnk-device-identities] cihaz kimlik doğrulaması ve erişim denetimi için kullanılır.
* **Cihaz ikizi Yönetimi**. Her IOT hub'ı sunan bir dizi sorgu ve güncelleştirme hizmeti kullanıma yönelik HTTPS REST uç noktasına [cihaz ikizlerini] [ lnk-twins] (etiketler ve özelliklerini güncelleştirin).
* **Management işler**. Her IOT hub'ı bir dizi sorgulamanızı ve yönetmenizi hizmeti kullanıma yönelik HTTPS REST uç noktasını kullanıma sunar. [işleri][lnk-jobs].
* **Cihaz uç noktalarına**. Kimlik kayıt defterinde her cihaz için bir uç nokta kümesine IOT hub'ı kullanıma sunar:

  * *CİHAZDAN buluta ileti gönderme*. Bir cihaz için bu endpoint kullanır [CİHAZDAN buluta ileti gönderme][lnk-d2c].
  * *Bulut-cihaz iletilerini*. Bir cihaz, hedeflenen almak için bu endpoint kullanır. [bulut-cihaz iletilerini][lnk-c2d].
  * *Dosya yüklemeleri başlatmak*. Bir cihaz IOT Hub'ına bir Azure depolama SAS URI'si almak için bu uç nokta kullanan [bir dosyayı karşıya yüklemeyi][lnk-upload].
  * *Alma ve cihaz ikizi özelliklerini güncelleştirme*. Bir cihaza erişmek için bu uç nokta kullanır, [cihaz ikizi][lnk-twins]ait özellikleri.
  * *Doğrudan yöntem isteklerini alacak*. Bir cihaz dinlemek için bu endpoint kullanır. [doğrudan yöntemini][lnk-methods]ait istekleri.

    Bu uç noktaları kullanarak sunulan [MQTT v3.1.1][lnk-mqtt], HTTPS 1.1 ve [AMQP 1.0] [ lnk-amqp] protokoller. AMQP üzerinden kullanılabilir ayrıca [WebSockets] [ lnk-websockets] bağlantı noktası 443 üzerinden.

* **Hizmet uç noktalarını**. Her IOT hub uç noktaları cihazlarınızla iletişim kurmak çözüm arka ucunuz için bir dizi kullanıma sunar. Bir özel durum dışında Bu uç noktalar yalnızca kullanılarak açılır [AMQP] [ lnk-amqp] protokolü. Yöntem çağırma uç noktası HTTPS protokolü üzerinden sunulur.
  
  * *CİHAZDAN buluta iletilerini*. Bu uç nokta ile uyumlu [Azure Event Hubs][lnk-event-hubs]. Bir arka uç hizmeti okunacak kullanabilirsiniz [CİHAZDAN buluta iletileri] [ lnk-d2c] , cihazlarınız tarafından gönderildi. Bu yerleşik uç nokta yanı sıra IOT hub'ınızdaki özel uç noktaları oluşturabilirsiniz.
  * *Bulut buluttan cihaza iletileri gönderme ve teslim alındı bildirimleri almak*. Bu uç noktaları güvenilir göndermek çözüm arka ucunuz etkinleştirme [bulut-cihaz iletilerini][lnk-c2d], karşılık gelen teslim veya süre sonu bildirimleri almak için.
  * *Dosya bildirimlerin*. Bu ileti uç cihazlarınızı bir dosya karşıya başarıyla bildirimleri almak sağlar. 
  * *Doğrudan yöntem çağırma*. Bu uç nokta çağırmak bir arka uç hizmeti sağlayan bir [doğrudan yöntemini] [ lnk-methods] bir cihazda.
  * *Alma işlemleri izleme olaylarını*. Bu uç nokta işlemleri izleme olaylarını IOT hub'ınıza bunları yaymak için yapılandırılmış olması halinde almanızı sağlar. Daha fazla bilgi için [IOT Hub işlemlerini izleme][lnk-operations-mon].

[Azure IOT SDK'ları] [ lnk-sdks] makalede bu uç noktalarına erişmek için çeşitli yollar.

Tüm IOT Hub uç noktaları kullanma [TLS] [ lnk-tls] protokol ve uç nokta şifrelenmemiş ve güvenli kanallar üzerinde gösterilen hiç olmadığı kadar.

## <a name="custom-endpoints"></a>Özel uç noktalar

Aboneliğinizde var olan Azure Hizmetleri, IOT hub ileti yönlendirme için uç nokta olarak yapması bağlayabilirsiniz. Bu uç noktaları, hizmet uç noktaları davranmasına ve havuz ileti yollarını kullanılır. Cihazları doğrudan ek uç noktalar için yazılamıyor. Daha fazla bilgi edinin [ileti yönlendirme](../iot-hub/iot-hub-devguide-messages-d2c.md).

IOT hub'ı, şu anda ek uç noktalar olarak aşağıdaki Azure Hizmetleri destekler:

* Azure depolama kapsayıcıları
* Event Hubs
* Service Bus Kuyrukları
* Service Bus Konuları

Uç noktalar ekleyebilirsiniz sayısı limitleri için bkz [kotalar ve azaltma][lnk-devguide-quotas].

## <a name="field-gateways"></a>Alan ağ geçitleri

Bir IOT çözümündeki bir *alan ağ geçidi* cihazlarınız ve IOT Hub uç noktalarınızı arasında yer alır. Cihazlarınızı yakın genellikle bulunur. Cihazlarınızı, cihazlar tarafından desteklenen bir protokolü kullanarak doğrudan alan ağ geçidi ile iletişim kurar. Alan ağ geçidi, IOT Hub tarafından desteklenen bir protokol kullanarak bir IOT Hub uç noktasına bağlanır. Bir alan ağ geçidi, adanmış bir donanımsal cihaz veya özel bir ağ geçidi yazılımının bilgisayarı düşük güç olabilir.

Kullanabileceğiniz [Azure IOT Edge] [ lnk-iot-edge] bir alan ağ geçidi uygulamak için. IOT Edge çoğullama iletişimler birden çok cihaz üzerinde aynı IOT Hub bağlantısı gibi işlevler sunar.

## <a name="next-steps"></a>Sonraki adımlar

Bu IOT Hub Geliştirici Kılavuzu'nda olan diğer başvuru konularını içerir:

* [Cihaz ikizleri, işler ve ileti yönlendirme için IOT Hub sorgu dili][lnk-devguide-query]
* [Kotalar ve azaltma][lnk-devguide-quotas]
* [IOT hub'ı MQTT desteği][lnk-devguide-mqtt]

[lnk-iot-edge]: https://github.com/Azure/iot-edge

[img-endpoints]: ./media/iot-hub-devguide-endpoints/endpoints.png
[lnk-amqp]: https://www.amqp.org/
[lnk-mqtt]: http://mqtt.org/
[lnk-websockets]: https://tools.ietf.org/html/rfc6455
[lnk-arm]: ../azure-resource-manager/resource-group-overview.md
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/

[lnk-tls]: https://tools.ietf.org/html/rfc5246


[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-accesscontrol]: iot-hub-devguide-security.md#access-control-and-permissions
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-device-identities]: iot-hub-devguide-identity-registry.md
[lnk-upload]: iot-hub-devguide-file-upload.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md

[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[lnk-operations-mon]: iot-hub-operations-monitoring.md
