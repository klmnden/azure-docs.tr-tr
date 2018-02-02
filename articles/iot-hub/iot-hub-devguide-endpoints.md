---
title: "Azure IOT Hub uç noktaları anlama | Microsoft Docs"
description: "Geliştirici Kılavuzu - IOT Hub hakkında başvuru bilgileri aygıt bakan ve hizmeti kullanıma yönelik uç noktalar."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 57ba52ae-19c6-43e4-bc6c-d8a5c2476e95
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/29/2018
ms.author: dobett
ms.openlocfilehash: 54491d0ca1f515786af07146d83ef65fc7d46f11
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="reference---iot-hub-endpoints"></a>Başvuru - IOT Hub uç noktaları

## <a name="iot-hub-names"></a>IOT Hub adları

Şirket portalı'nda uç noktalarınızı barındıran IOT hub'ı adı bulabilirsiniz **genel bakış** dikey. IOT hub'ı DNS adını varsayılan olarak, aşağıdaki gibi görünür: `{your iot hub name}.azure-devices.net`.

Azure DNS, IOT hub'ınız için özel bir DNS adı oluşturmak için kullanabilirsiniz. Daha fazla bilgi için bkz: [bir Azure hizmetini özel etki alanı ayarları sağlamak için Azure DNS'yi](../dns/dns-custom-domain.md).

## <a name="list-of-built-in-iot-hub-endpoints"></a>Yerleşik IOT Hub uç noktaları listesi

Azure IOT hub'ı çeşitli aktörler için işlevselliği kullanıma sunan çok kiracılı bir hizmettir. Aşağıdaki diyagramda, IOT hub'ı gösterir çeşitli uç noktalarını gösterir.

![IoT Hub uç noktaları][img-endpoints]

Aşağıdaki listede uç noktalar açıklanmaktadır:

* **Kaynak sağlayıcısı**. IOT hub'ı kaynak sağlayıcısı kullanıma sunan bir [Azure Resource Manager] [ lnk-arm] arabirimi. Bu arabirim, Azure abonelik sahipleri oluşturun ve IOT hub'ları silin ve IOT hub'ı özelliklerini güncelleştirmek için etkinleştirir. IOT hub'ı özelliklerini yöneten [hub düzeyindeki güvenlik ilkeleri][lnk-accesscontrol], cihaz düzeyinde erişim denetimi ve bulut-cihaz ve cihaz bulut Mesajlaşma işlevsel seçeneklerini aksine. IOT hub'ı kaynak sağlayıcısı ayrıca sağlar [cihaz kimliklerini verme][lnk-importexport].
* **Cihaz Kimlik Yönetimi**. Her IOT hub cihaz kimlikleri yönetmek için HTTPS REST uç noktalar kümesi sunar (oluşturma, alma, güncelleştirme ve silme). [Cihaz kimliklerini] [ lnk-device-identities] cihaz kimlik doğrulaması ve erişim denetimi için kullanılır.
* **Cihaz çifti Yönetimi**. Her IOT hub'ı sorgu ve güncelleştirme hizmeti dönük HTTPS REST uç noktasına kümesi sunan [cihaz çiftlerini] [ lnk-twins] (güncelleştirme etiketleri ve özellikleri).
* **İşlerini Yönetim**. Her IOT hub'ı sorgulamak ve yönetmek için hizmet dönük HTTPS REST uç nokta kümesi sunan [işleri][lnk-jobs].
* **Cihaz uç noktaları**. IOT Hub kimlik kayıt defterinde her cihaz için uç noktalar kümesi sunar:

  * *Cihaz bulut iletilerini göndermek*. Bu uç noktaya bir aygıtın kullandığı [cihaz bulut iletilerini göndermek][lnk-d2c].
  * *Bulut-cihaz iletilerini*. Hedeflenen almak için bu uç noktası bir aygıtı kullandığı [bulut-cihaz iletilerini][lnk-c2d].
  * *Dosya yüklemeleri başlatmak*. Bir cihaz IOT Hub'ına bir Azure depolama SAS URI'sini almak için bu uç nokta kullanır [bir dosyayı karşıya yüklemeyi][lnk-upload].
  * *Alma ve güncelleştirme cihaz çifti özellikleri*. Bir aygıt bu uç nokta erişmek için kullanır. kendi [cihaz çifti][lnk-twins]ait özellikler.
  * *Doğrudan yöntemi isteklerini alacak*. Bir aygıt bu uç nokta dinlemek için kullanır. [doğrudan yöntemi][lnk-methods]'s istekleri.

    Bu uç noktalar kullanılarak kullanıma sunulan [MQTT v3.1.1][lnk-mqtt], HTTPS 1.1 ve [AMQP 1.0] [ lnk-amqp] protokoller. AMQP kullanılabilir ayrıca üzerinden [WebSockets] [ lnk-websockets] bağlantı noktası 443'tür.

* **Hizmet uç noktaları**. Her IOT hub uç noktaları, aygıtlar ile iletişim kurmak çözüm arka ucunuz için bir dizi kullanıma sunar. Bunun tek istisnası Bu uç noktalar yalnızca kullanarak gösterilen [AMQP] [ lnk-amqp] protokolü. Yöntem çağırma uç noktası HTTPS protokolü üzerinden açıktır.
  
  * *Cihaz bulut iletilerini*. Bu uç nokta ile uyumlu [Azure Event Hubs][lnk-event-hubs]. Bir arka uç hizmeti okumak için kullanabileceğiniz [cihaz bulut iletilerini] [ lnk-d2c] cihazlar tarafından gönderilen. Bu yerleşik bir uç nokta ek olarak IOT hub'ınızı özel uç noktaları oluşturabilirsiniz.
  * *Bulut-cihaz iletilerini göndermek ve teslim alındı bildirimleri almak*. Bu uç noktalar güvenilir göndermek çözüm arka ucunuz etkinleştirmek [bulut-cihaz iletilerini][lnk-c2d]ve karşılık gelen teslim veya süre sonu bildirimleri almak için.
  * *Dosya bildirimlerin*. Bu ileti uç aygıtlarınızı başarıyla bir dosyayı karşıya yüklediğinizde bildirimlerini almak üzere sağlar. 
  * *Yöntem çağırma doğrudan*. Bu uç noktaya çağırmak bir arka uç hizmeti sağlayan bir [doğrudan yöntemi] [ lnk-methods] bir aygıtta.
  * *Alma olaylarını izleme işlemleri*. Bu uç noktayı alma işlemlerinin, IOT hub'ınızı bunları yaymak üzere yapılandırılmışsa, olayları izleme sağlar. Daha fazla bilgi için bkz: [izleme IOT hub'ı operations][lnk-operations-mon].

[Azure IOT SDK'ları] [ lnk-sdks] makalede, bu uç noktalar erişmek için çeşitli yolları açıklanmaktadır.

Tüm IOT Hub uç noktaları kullanma [TLS] [ lnk-tls] protokolü ve uç nokta yok şifrelenmemiş ve güvenli kanallar üzerinde gösterilen herhangi bir zamanda.

## <a name="custom-endpoints"></a>Özel uç noktalar

İleti yönlendirme için uç noktalar olarak hareket edecek, IOT hub'ına aboneliğinizde var olan Azure Hizmetleri bağlayabilirsiniz. Bu uç noktalar hizmet uç noktalar olarak hareket ve iç havuzlar ileti yollar için kullanılır. Cihazları doğrudan ek uç noktaları için yazamıyor. İleti yollar hakkında daha fazla bilgi için üzerinde Geliştirici Kılavuzu girişine bakın [IOT hub ile ileti gönderme ve alma][lnk-devguide-messaging].

IOT Hub aşağıdaki Azure hizmetlerini ek uç noktalar olarak şu anda destekler:

* Azure Storage kapsayıcıları
* Event Hubs
* Service Bus Kuyrukları
* Service Bus Konuları

IOT Hub, ileti yönlendirmesinin çalışması için bu hizmet uç noktaları yazma erişimi olmalıdır. Azure Portalı aracılığıyla uç noktalarınızı yapılandırırsanız, sizin için gerekli izinleri eklenir. Hizmetlerinizin beklenen verimlilik desteklemek üzere yapılandırdığınızdan emin olun. IOT çözümünüzü ilk yapılandırırken, ek noktalarınızı izlemek ve gerçek yükleme için gerekli ayarlamaları yapmak gerekebilir.

IOT hub'ı ileti Bu uç noktasına tüm aynı uç noktası birden çok yönlendiren bir ileti eşleşirse, yalnızca bir kez sunar. Bu nedenle, hizmet veri yolu kuyruğu ya da konu yinelenenleri kaldırmayı yapılandırma gerekmez. Bölümlenmiş sıralarındaki bölüm benzeşim ileti sıralama güvence altına alır.

Ekleyebileceğiniz uç noktaların sayısını sınırları için bkz: [kotalar ve azaltma][lnk-devguide-quotas].

### <a name="when-using-azure-storage-containers"></a>Azure Storage kapsayıcıları kullanırken

IOT hub'ı yalnızca destekler bloblar olarak Azure Storage kapsayıcıları için veri yazma [Apache Avro](http://avro.apache.org/) biçimi. IOT Hub iletileri toplu işlemleri ve bir blob verileri yazar zaman:

* Toplu işlem belirli bir boyuta ulaştığında.
* Veya belirli bir süre geçti.

IOT hub'ı mu yazma yazmak için veri yoksa boş bir blob.

IOT hub'ı varsayılan olarak aşağıdaki dosya adlandırma kuralları:

```
{iothub}/{partition}/{YYYY}/{MM}/{DD}/{HH}/{mm}
```

İstediğiniz adlandırma hangi dosya, ancak listelenen tüm belirteçleri kullanmanız gerekir kullanabilir.

### <a name="when-using-service-bus-queues-and-topics"></a>Service Bus kuyrukları ve konularından kullanırken

Service Bus kuyrukları ve konularından IOT Hub uç noktaları değil olarak kullanılan **oturumları** veya **yinelenen saptama** etkin. Bu seçeneklerden birini etkinleştirilmişse, uç nokta olarak görünür **ulaşılamıyor** Azure portalında.

## <a name="field-gateways"></a>Alan ağ geçitleri

Bir IOT çözümündeki bir *alan ağ geçidi* , cihazlarınız ve IOT Hub uç noktaları arasında bulunur. Aygıtlarınızı yakın genellikle yer alır. Aygıtlarınızı, cihazlar tarafından desteklenen bir protokolü kullanarak doğrudan alan ağ geçidi ile iletişim kurar. IOT Hub tarafından desteklenen bir protokolünü kullanarak bir IOT hub'ı uç alan ağ geçidi bağlanır. Bir alan ağ geçidi, adanmış bir donanımsal cihaz veya özel ağ geçidi yazılımını çalıştıran bir düşük güç bilgisayar olabilir.

Kullanabileceğiniz [Azure IOT kenar] [ lnk-iot-edge] bir alan ağ geçidi uygulamak için. IOT kenar aynı IOT Hub bağlantı üzerine birden çok aygıt çoğullama iletişimlerinden gibi işlevsellikler sunar.

## <a name="next-steps"></a>Sonraki adımlar

Bu IOT Hub Geliştirici Kılavuzu'ndaki diğer başvuru konuları içerir:

* [Cihaz çiftlerini, işler ve ileti yönlendirme için IOT hub'ı sorgulama dili][lnk-devguide-query]
* [Kotalar ve azaltma][lnk-devguide-quotas]
* [IOT Hub MQTT desteği][lnk-devguide-mqtt]

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
