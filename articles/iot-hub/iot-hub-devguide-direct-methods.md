---
title: "Azure IOT hub'ı doğrudan yöntemlerini anlama | Microsoft Docs"
description: "Geliştirici Kılavuzu - kod cihazlarınızda service uygulamasından çağırmak için doğrudan yöntemlerini kullanın."
services: iot-hub
documentationcenter: .net
author: nberdy
manager: timlt
editor: 
ms.assetid: 9f0535f1-02e6-467a-9fc4-c0950702102d
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/29/2018
ms.author: nberdy
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 003b3f6ef8a6fbc1c6fcdfc58f7d35bf6c42c9ee
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="understand-and-invoke-direct-methods-from-iot-hub"></a>Anlama ve IOT hub'ı doğrudan yöntemleri çağırma
IOT Hub, bulutta cihazlarda doğrudan yöntemlerini çağırmasına olanak sağlar. Başarılı veya başarısız hemen (bir kullanıcı tarafından belirtilen zaman aşımından sonra), bir HTTP çağrısıyla benzer bir cihaz istek-yanıt etkileşim doğrudan yöntemleri temsil eder. Bu yaklaşım, Acil eylem seyri aygıtı yanıt verebilmesini olmasına bağlı olarak farklı olduğu senaryolar için kullanışlıdır. Örneğin, bir SMS Uyandırma (çevrimdışı SMS yöntem çağrısı daha pahalı olması) ise bir cihaza gönderme.
Her cihaz yöntemi tek bir cihazı hedefler. [İşlerini] [ lnk-devguide-jobs] doğrudan yöntemlerini birden çok aygıta çağırmak için bir yol sağlar ve zamanlama yöntem çağırma bağlantısı kesilmiş aygıtları için.

Herkesle **service bağlanma** IOT Hub üzerindeki izinleri bir aygıtta bir yöntemi çağırma.

Doğrudan yöntemleri bir istek-yanıt desen izleyin ve kendi sonucunun hemen onay gerektiren iletişimleri için yöneliktir. Örneğin, etkileşimli denetim üzerinde fan kapatma gibi cihaz.

Başvurmak [bulut-cihaz iletişimi Kılavuzu] [ lnk-c2d-guidance] istenen özelliklerini kullanarak arasında emin değilseniz, yöntemler veya Bulut-cihaz iletilerini doğrudan.

## <a name="method-lifecycle"></a>Yöntem yaşam döngüsü
Doğrudan yöntemleri cihazda uygulanır ve doğru örneği oluşturmak için yöntem yükte sıfır veya daha fazla girdi gerektirebilir. Bir hizmet dönük URI üzerinden doğrudan bir yöntemi çağırma (`{iot hub}/twins/{device id}/methods/`). Bir aygıt bir aygıta özgü MQTT konu aracılığıyla doğrudan yöntemleri alır (`$iothub/methods/POST/{method name}/`) veya AMQP bağlantılar yoluyla (`IoThub-methodname` ve `IoThub-status` uygulama özellikleri). 

> [!NOTE]
> Bir cihazda doğrudan bir yöntemini çağırdığınızda, özellik adları ve değerleri yalnızca US-ASCII yazdırılabilir içerebilir herhangi aşağıdaki kümesindeki dışında alfasayısal: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.
> 
> 

Zaman uyumlu yöntemleri ve ya da başarılı doğrudan veya zaman aşımı süresinden sonra başarısız (varsayılan: 30 saniye, 3600 saniye olarak ayarlanabilir kurma). Doğrudan yöntemler cihaz çevrimiçi ve alıcı komutları varsa ve yalnızca ise görev yapması için bir aygıt istediğiniz etkileşimli senaryolarda kullanışlıdır. Örneğin, bir Phone ışığı kapatılıyor. Bu senaryolarda, bulut hizmeti sonucuna mümkün olan en kısa sürede hareket etmesini sağlamak bir hemen başarı veya başarısızlık görmek istediğinizi açıklayın. Cihaz bazı ileti gövdesi yöntemi sonucunda döndürebilir ancak yöntemi bunu yapmak gerekli değildir. Garanti sıralama üzerinde veya tüm eşzamanlılık semantiği yöntem çağrılarını üzerinde yok.

Yöntemleri doğrudan HTTPS salt bulut yan ve MQTT veya AMQP aygıt taraftaki.

Yöntem istekleri ve yanıtları için yükü, bir JSON belgesi en fazla 128 KB ' dir.

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>Bir arka uç uygulamasından doğrudan bir yöntem çağırma
### <a name="method-invocation"></a>Yöntem çağırma
Bir cihazda doğrudan yöntem çağrılarını oluşturan HTTPS çağrıları şunlardır:

* *URI* aygıta belirli (`{iot hub}/twins/{device id}/methods/`)
* POST *yöntemi*
* *Üstbilgileri* yetkilendirme içeren, istek kimliği, içerik türü ve içerik kodlaması
* Saydam JSON *gövde* şu biçimde:

    ```json
    {
        "methodName": "reboot",
        "responseTimeoutInSeconds": 200,
        "payload": {
            "input1": "someInput",
            "input2": "anotherInput"
        }
    }
    ```

Zaman aşımı saniye cinsindendir. Zaman aşımı ayarlanmamışsa 30 saniye olarak varsayılan olarak ayarlanır.

### <a name="response"></a>Yanıt
Arka uç uygulama içeren bir yanıt alır:

* *HTTP durum kodu*, IOT Hub'ından gelen hatalara kullanıldığı, 404 hatası cihazlar için şu anda da dahil olmak üzere bağlı
* *Üstbilgileri* ETag içeren, istek kimliği, içerik türü ve içerik kodlaması
* JSON *gövde* şu biçimde:

    ```json
    {
        "status" : 201,
        "payload" : {...}
    }
    ```

    Her ikisi de `status` ve `body` cihaz tarafından sağlanan ve cihazın kendi durum kodu ve/veya açıklama ile yanıtlamak için kullanılır.

## <a name="handle-a-direct-method-on-a-device"></a>Bir cihazda doğrudan bir yöntem işleme
### <a name="mqtt"></a>MQTT
#### <a name="method-invocation"></a>Yöntem çağırma
Cihazlar, MQTT konusunda doğrudan yöntem isteği alır:`$iothub/methods/POST/{method name}/?$rid={request id}`

Cihaz aldıktan gövdesi aşağıdaki biçimdedir:

```json
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

Yöntemi, QoS 0 isteklerdir.

#### <a name="response"></a>Yanıt
Cihaz yanıtlarını gönderir `$iothub/methods/res/{status}/?$rid={request id}`, burada:

* `status` Yöntemi yürütme aygıt tarafından sağlanan durumunu bir özelliktir.
* `$rid` IOT Hub'ından alınan yöntemi çağırma isteği Kimliğinden bir özelliktir.

Gövde aygıt tarafından ayarlanır ve herhangi bir durum olabilir.

### <a name="amqp"></a>AMQP
#### <a name="method-invocation"></a>Yöntem çağırma
Cihaz adresinde alma bağlantı oluşturarak doğrudan yöntem isteği alır`amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound`

AMQP ileti yöntemi isteğinin temsil eden alma bağlantıyı ulaşır. Aşağıdakileri içerir:
* Karşılık gelen yöntemi yanıtı ile iletilmesi gereken bir istek kimliği içeren bağıntı kimliği özelliği
* Adlı bir uygulama özelliği `IoThub-methodname`, çağrılmakta olan yöntemin adını içerir
* Yöntem yükü JSON olarak içeren AMQP ileti gövdesi

#### <a name="response"></a>Yanıt
Aygıt adresi yöntemi yanıtı döndürmek için gönderen bir bağlantı oluşturur`amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound`

Yöntemin yanıt gönderen bağlantıyı döndürülür ve aşağıdaki gibi yapılandırılmış:
* İstek Kimliği içeren bağıntı kimliği özelliği yöntemin istek iletisinde geçirilen
* Adlı bir uygulama özelliği `IoThub-status`, kullanıcı içeren sağlanan yöntemi durumu
* JSON olarak yöntemi yanıtı içeren AMQP ileti gövdesi

## <a name="additional-reference-material"></a>Ek başvuru bilgileri
IOT Hub Geliştirici Kılavuzu'ndaki diğer başvuru konuları içerir:

* [IOT Hub uç noktaları] [ lnk-endpoints] her IOT hub'ı çalışma zamanı ve yönetim işlemleri için kullanıma sunan çeşitli uç noktaları açıklar.
* [Azaltma ve kotaları] [ lnk-quotas] geçerli kotalar ve azaltma davranışı IOT hub'ı kullandığınızda beklediğiniz açıklar.
* [Azure IOT cihaz ve hizmet SDK'ları] [ lnk-sdks] çeşitli dil IOT Hub ile etkileşim hem cihaz hem de hizmet uygulamaları geliştirirken kullanabilir SDK'ları listeler.
* [IOT Hub cihaz çiftlerini, işler ve ileti yönlendirme için sorgu dili] [ lnk-query] IOT Hub'ından, cihaz çiftlerini ve işleri hakkında bilgi almak için kullanabileceğiniz IOT hub'ı sorgu dili açıklar.
* [IOT Hub MQTT Destek] [ lnk-devguide-mqtt] IOT hub'ı desteği hakkında daha fazla bilgi için MQTT Protokolü sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Doğrudan yöntemlerini kullanmayı öğrendiniz artık aşağıdaki IOT Hub Geliştirici Kılavuzu makalesinde ilgilenen olabilir:

* [Birden çok aygıta işleri zamanla][lnk-devguide-jobs]

Bu makalede açıklanan kavramları bazıları denemek istiyorsanız, aşağıdaki IOT hub'ı öğreticide ilgilenen olabilir:

* [Doğrudan yöntemleri kullanın][lnk-methods-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-devguide-messages]: iot-hub-devguide-messaging.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
