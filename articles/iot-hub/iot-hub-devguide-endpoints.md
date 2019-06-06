---
title: Azure IOT Hub uç noktaları anlama | Microsoft Docs
description: Geliştirici Kılavuzu - IOT Hub hakkında başvuru bilgileri cihaz hem de hizmet'e yönelik uç noktalar.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/18/2018
ms.openlocfilehash: 5854a795ba7ceeeb4512f1e2fd16d98826d55dd5
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66477979"
---
# <a name="reference---iot-hub-endpoints"></a>Başvuru - IOT Hub uç noktaları

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="iot-hub-names"></a>IOT hub'ı adları

Ana bilgisayar adını, uç noktaları, hub'ın şirket Portalı'nda barındıran IOT hub'ı bulabilirsiniz **genel bakış** sayfası. Varsayılan olarak, IOT hub'ının DNS adı şu şekilde görünür: `{your iot hub name}.azure-devices.net`.

## <a name="list-of-built-in-iot-hub-endpoints"></a>Yerleşik IOT Hub uç noktaları listesi

Azure IOT hub'ı çeşitli aktörler için işlevselliği sunan çok kiracılı bir hizmettir. Aşağıdaki diyagramda, IOT hub'ı göstermesini çeşitli uç noktalarını gösterir.

![IoT Hub uç noktaları](./media/iot-hub-devguide-endpoints/endpoints.png)

Aşağıdaki listede, uç noktalar açıklanmaktadır:

* **Kaynak sağlayıcısı**. IOT hub'ı kaynak sağlayıcısı sunan bir [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) arabirimi. Azure abonelik sahipleri oluşturun ve IOT hub'ları silme ve IOT hub'ı özelliklerini güncelleştirmek için bu arabirimi sağlar. IOT hub'ı özelliklerini belirleyen [hub düzeyindeki güvenlik ilkeleri](iot-hub-devguide-security.md#access-control-and-permissions), cihaz düzeyinde erişim denetimi ve bulut-cihaz ve CİHAZDAN buluta iletiler için işlevsel seçenekleri yerine. IOT hub'ı kaynak sağlayıcısı ayrıca sayesinde [cihaz kimliklerini dışarı](iot-hub-devguide-identity-registry.md#import-and-export-device-identities).

* **Cihaz Kimlik Yönetimi**. Her IOT hub cihaz kimliklerini yönetme için HTTPS REST uç kümesi sunar (oluşturma, alma, güncelleştirme ve silme). [Cihaz kimliklerini](iot-hub-devguide-identity-registry.md) cihaz kimlik doğrulaması ve erişim denetimi için kullanılır.

* **Cihaz ikizi Yönetimi**. Her IOT hub'ı sunan bir dizi sorgu ve güncelleştirme hizmeti kullanıma yönelik HTTPS REST uç noktasına [cihaz ikizlerini](iot-hub-devguide-device-twins.md) (etiketler ve özelliklerini güncelleştirin).

* **Management işler**. Her IOT hub'ı bir dizi sorgulamanızı ve yönetmenizi hizmeti kullanıma yönelik HTTPS REST uç noktasını kullanıma sunar. [işleri](iot-hub-devguide-jobs.md).

* **Cihaz uç noktalarına**. Kimlik kayıt defterinde her cihaz için bir uç nokta kümesine IOT hub'ı kullanıma sunar:

  * *CİHAZDAN buluta ileti gönderme*. Bir cihaz için bu endpoint kullanır [CİHAZDAN buluta ileti gönderme](iot-hub-devguide-messages-d2c.md).

  * *Bulut-cihaz iletilerini*. Bir cihaz, hedeflenen almak için bu endpoint kullanır. [bulut-cihaz iletilerini](iot-hub-devguide-messages-c2d.md).

  * *Dosya yüklemeleri başlatmak*. Bir cihaz IOT Hub'ına bir Azure depolama SAS URI'si almak için bu uç nokta kullanan [bir dosyayı karşıya yüklemeyi](iot-hub-devguide-file-upload.md).

  * *Alma ve cihaz ikizi özelliklerini güncelleştirme*. Bir cihaza erişmek için bu uç nokta kullanır, [cihaz ikizi](iot-hub-devguide-device-twins.md)ait özellikleri.

  * *Doğrudan yöntem isteklerini alacak*. Bir cihaz dinlemek için bu endpoint kullanır. [doğrudan yöntemini](iot-hub-devguide-direct-methods.md)ait istekleri.

    Bu uç noktaları kullanarak sunulan [MQTT v3.1.1](https://mqtt.org/), HTTPS 1.1 ve [AMQP 1.0](https://www.amqp.org/) protokoller. AMQP üzerinden kullanılabilir ayrıca [WebSockets](https://tools.ietf.org/html/rfc6455) bağlantı noktası 443 üzerinden.

* **Hizmet uç noktalarını**. Her IOT hub uç noktaları cihazlarınızla iletişim kurmak çözüm arka ucunuz için bir dizi kullanıma sunar. Bir özel durum dışında Bu uç noktalar yalnızca kullanılarak açılır [AMQP](https://www.amqp.org/) protokolü. Yöntem çağırma uç noktası HTTPS protokolü üzerinden sunulur.
  
  * *CİHAZDAN buluta iletilerini*. Bu uç nokta ile uyumlu [Azure Event Hubs](https://azure.microsoft.com/documentation/services/event-hubs/). Bir arka uç hizmeti okunacak kullanabilirsiniz [CİHAZDAN buluta iletileri](iot-hub-devguide-messages-d2c.md) , cihazlarınız tarafından gönderildi. Bu yerleşik uç nokta yanı sıra IOT hub'ınızdaki özel uç noktaları oluşturabilirsiniz.
  
  * *Bulut buluttan cihaza iletileri gönderme ve teslim alındı bildirimleri almak*. Bu uç noktaları güvenilir göndermek çözüm arka ucunuz etkinleştirme [bulut-cihaz iletilerini](iot-hub-devguide-messages-c2d.md), karşılık gelen teslim veya süre sonu bildirimleri almak için.
  
  * *Dosya bildirimlerin*. Bu ileti uç cihazlarınızı bir dosya karşıya başarıyla bildirimleri almak sağlar. 
  
  * *Doğrudan yöntem çağırma*. Bu uç nokta çağırmak bir arka uç hizmeti sağlayan bir [doğrudan yöntemini](iot-hub-devguide-direct-methods.md) bir cihazda.
  
  * *Alma işlemleri izleme olaylarını*. Bu uç nokta işlemleri izleme olaylarını IOT hub'ınıza bunları yaymak için yapılandırılmış olması halinde almanızı sağlar. Daha fazla bilgi için [IOT Hub işlemlerini izleme](iot-hub-operations-monitoring.md).

[Azure IOT SDK'ları](iot-hub-devguide-sdks.md) makalede bu uç noktalarına erişmek için çeşitli yollar.

Tüm IOT Hub uç noktaları kullanma [TLS](https://tools.ietf.org/html/rfc5246) protokol ve uç nokta şifrelenmemiş ve güvenli kanallar üzerinde gösterilen hiç olmadığı kadar.

## <a name="custom-endpoints"></a>Özel uç noktalar

Aboneliğinizde var olan Azure Hizmetleri, IOT hub ileti yönlendirme için uç nokta olarak yapması bağlayabilirsiniz. Bu uç noktaları, hizmet uç noktaları davranmasına ve havuz ileti yollarını kullanılır. Cihazları doğrudan ek uç noktalar için yazılamıyor. Daha fazla bilgi edinin [ileti yönlendirme](../iot-hub/iot-hub-devguide-messages-d2c.md).

IOT hub'ı, şu anda ek uç noktalar olarak aşağıdaki Azure Hizmetleri destekler:

* Azure depolama kapsayıcıları
* Event Hubs
* Service Bus Kuyrukları
* Service Bus Konuları

Uç noktalar ekleyebilirsiniz sayısı limitleri için bkz [kotalar ve azaltma](iot-hub-devguide-quotas-throttling.md).

REST API kullanabilirsiniz [uç nokta sistem durumu alma](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) uç sistem durumunu almak için. Kullanmanızı öneririz [IOT hub'ı ölçümleri](iot-hub-metrics.md) belirlemek ve uç nokta sistem durumu ölü ya da sağlıksız, olduğunda gecikme süresi, uç nokta bu durumlardan birinde olduğunda daha yüksek olmasını bekliyoruz gibi hataları hata ayıklamak için yönlendirme ileti gecikme süresi ile ilgili.

|Sistem durumu|Açıklama|
|---|---|
|İyi durumda|Uç nokta, beklendiği gibi iletileri kabul ediyor.|
|İyi durumda olmayan|Uç nokta ileti beklendiği gibi kabul etmiyor ve IOT hub'ı Bu uç noktaya veri göndermek için yeniden deneniyor. IOT Hub durumu sonunda tutarlı bir duruma olduğunda sağlıksız bir uç nokta durumunu iyi durumda olacak şekilde güncelleştirilir.|
|Bilinmiyor|IOT Hub uç noktası ile bağlantı değil. İleti teslim veya bu uç noktadan reddetti.|
|eski|IOT hub'ı iletileri gönderme retrial dönemin denenen sonra uç noktayı iletileri kabul etmiyor.|

## <a name="field-gateways"></a>Alan ağ geçitleri

Bir IOT çözümündeki bir *alan ağ geçidi* cihazlarınız ve IOT Hub uç noktalarınızı arasında yer alır. Cihazlarınızı yakın genellikle bulunur. Cihazlarınızı, cihazlar tarafından desteklenen bir protokolü kullanarak doğrudan alan ağ geçidi ile iletişim kurar. Alan ağ geçidi, IOT Hub tarafından desteklenen bir protokol kullanarak bir IOT Hub uç noktasına bağlanır. Bir alan ağ geçidi, adanmış bir donanımsal cihaz veya özel bir ağ geçidi yazılımının bilgisayarı düşük güç olabilir.

Kullanabileceğiniz [Azure IOT Edge](/azure/iot-edge/) bir alan ağ geçidi uygulamak için. IOT Edge çoğullama iletişimler birden çok cihaz üzerinde aynı IOT Hub bağlantısı gibi işlevler sunar.

## <a name="next-steps"></a>Sonraki adımlar

Bu IOT Hub Geliştirici Kılavuzu'nda olan diğer başvuru konularını içerir:

* [Cihaz ikizleri, işler ve ileti yönlendirme için IOT Hub sorgu dili](iot-hub-devguide-query-language.md)
* [Kotalar ve azaltma](iot-hub-devguide-quotas-throttling.md)
* [IOT hub'ı MQTT desteği](iot-hub-mqtt-support.md)
