---
title: Azure IOT hub'ı doğrudan yöntemleri anlama | Microsoft Docs
description: Geliştirici Kılavuzu - kod cihazlarınızda bir hizmet uygulamasından çağırmak için doğrudan yöntemleri kullanın.
author: nberdy
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/17/2018
ms.author: nberdy
ms.openlocfilehash: 881262816fc8bd634b7f577fd05aa0c8c062e4ca
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39126533"
---
# <a name="understand-and-invoke-direct-methods-from-iot-hub"></a>Anlama ve IOT Hub'ından doğrudan metotları çağırma
IOT Hub buluttan cihazlar üzerinde doğrudan metotları çağırma yeteneği sağlar. Başarılı veya başarısız hemen (kullanıcı tarafından belirtilen zaman aşımından sonra) doğrudan yöntemler için HTTP çağrısı benzer bir cihazla bir istek-yanıt etkileşimi temsil eder. Bu yaklaşım, hemen işlem boyunca cihaz yanıt verebilmesi olmasına bağlı olarak farklı olduğu senaryolar için kullanışlıdır.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Her cihaz yöntemi tek bir cihaz hedefler. [İşleri] [ lnk-devguide-jobs] birden fazla cihazda doğrudan metotları çağırma için bir yol sağlar ve yöntem çağırma bağlantısı kesilmiş cihazlar için zamanlama.

Hiç kimseyle **hizmetini bağlama** IOT hub'ı izinlerini cihazda yöntem çağırma.

Doğrudan yöntemler istek-yanıt deseni izler ve bunların sonucunun anında onay gerektiren iletişimleri için tasarlanmıştır. Örneğin, etkileşimli denetim üzerinde fan kapatma gibi cihaz.

Başvurmak [bulut buluttan cihaza iletişim Kılavuzu] [ lnk-c2d-guidance] arasında istenen özellikleri kullanarak bu konuda şüpheleriniz varsa, yöntemler veya Bulut-cihaz iletilerini doğrudan.

## <a name="method-lifecycle"></a>Yöntem yaşam döngüsü
Doğrudan yöntemler cihazda uygulandı ve sıfır veya daha fazla giriş doğru örnekleyin için yöntemi yükünde gerektirebilir. Hizmet kullanıma yönelik bir URI aracılığıyla bir doğrudan yöntem çağırma (`{iot hub}/twins/{device id}/methods/`). Bir cihaz doğrudan bir cihaza özgü MQTT konu yöntemleri alır (`$iothub/methods/POST/{method name}/`) veya AMQP bağlantıları aracılığıyla (`IoThub-methodname` ve `IoThub-status` uygulama özellikleri). 

> [!NOTE]
> Bir cihazda doğrudan yöntem çağırdığınızda, özellik adları ve değerleri yalnızca US-ASCII yazdırılabilir içerebilir dışında aşağıdaki, tüm alfasayısal: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.
> 
> 

Doğrudan yöntemleri zaman uyumludur ve ya da başarılı olması veya zaman aşımı süresinden sonra başarısız (varsayılan: 3600 saniye olarak ayarlanabilir yukarı 30 saniye). Doğrudan yöntemler, cihaz çevrimiçi ve alıcı komutları adıdır ve yalnızca, yapacak bir cihaz istediğiniz etkileşimli senaryolarda yararlıdır. Örneğin, bir ışık telefonda açmak. Bu senaryolarda, bulut hizmeti sonucuna olabildiğince çabuk davranabilir bir anında başarı veya başarısızlık görmek istiyorsunuz. Cihaz yöntemi sonucunda bazı ileti gövdesini döndürebilir, ancak bu yöntem Bunu yapmak gerekli değildir. Üzerinde sıralama garantisi veya yöntem çağrılarının tüm eşzamanlılık semantiği yoktur.

Doğrudan yöntemler yalnızca HTTPS bulut yan ve MQTT veya AMQP cihaz tarafındaki.

Yöntem istekleri ve yanıtları için bir JSON belgesi en fazla 128 KB yüktür.

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>Bir arka uç uygulamasından bir doğrudan yöntem çağırma
### <a name="method-invocation"></a>Yöntem çağırma
Bir cihazda doğrudan yöntem çağrılarını oluşturan HTTPS çağrıları vardır:

* *İstek URI'si* cihaz ile birlikte özel [API sürümü](/rest/api/iothub/service/invokedevicemethod):

    ```http
    https://fully-qualified-iothubname.azure-devices.net/twins/{deviceId}/methods?api-version=2018-06-30
    ```

* POST *yöntemi*
* *Üstbilgileri* yetkilendirme içerir, istek kimliği, içerik türü ve içerik kodlaması
* Saydam bir JSON *gövdesi* şu biçimde:

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

Zaman aşımı saniye cinsindendir. Zaman aşımı ayarlanmazsa, 30 saniye olarak varsayılan olarak.

#### <a name="example"></a>Örnek

Barebone kullanma örneği için aşağıya bakın `curl`. 

```bash
curl -X POST \
  https://iothubname.azure-devices.net/twins/myfirstdevice/methods?api-version=2018-06-30 \
  -H 'Authorization: SharedAccessSignature sr=iothubname.azure-devices.net&sig=x&se=x&skn=iothubowner' \
  -H 'Content-Type: application/json' \
  -d '{
    "methodName": "reboot",
    "responseTimeoutInSeconds": 200,
    "payload": {
        "input1": "someInput",
        "input2": "anotherInput"
    }
}'
```

### <a name="response"></a>Yanıt
Arka uç uygulaması oluşturan bir yanıt alır:

* *HTTP durum kodu*, şu anda bir 404 hatası cihazlar dahil olmak üzere bağlı olan IOT Hub'ından gelen hatalar için kullanılır
* *Üstbilgileri* içeren ETag, istek kimliği, içerik türü ve içerik kodlaması
* Bir JSON *gövdesi* şu biçimde:

    ```json
    {
        "status" : 201,
        "payload" : {...}
    }
    ```

    Her ikisi de `status` ve `body` cihaz tarafından sağlanan ve cihazın kendi durum kodu ve/veya açıklamanın ile yanıt vermek için kullanılır.

### <a name="method-invocation-for-iot-edge-modules"></a>Yöntem çağırma için IOT Edge modülleri
Bir modül kimliği desteklenen C# dilinde kullanarak çağrılıyor doğrudan yöntemler Önizleme SDK'sı (kullanılabilir [burada](https://www.nuget.org/packages/Microsoft.Azure.Devices/1.16.0-preview-004)).

Bu amaçla kullanabileceğiniz `ServiceClient.InvokeDeviceMethodAsync()` yöntemi ve geçişinde `deviceId` ve `moduleId` parametre olarak.

## <a name="handle-a-direct-method-on-a-device"></a>Tanıtıcı bir cihazda doğrudan yöntem
### <a name="mqtt"></a>MQTT
#### <a name="method-invocation"></a>Yöntem çağırma
Cihaz, MQTT konusunda doğrudan yöntem isteklerini alır: `$iothub/methods/POST/{method name}/?$rid={request id}`

Cihaz aldıktan gövdesi aşağıdaki biçimdedir:

```json
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

Yöntemi, QoS 0 isteklerdir.

#### <a name="response"></a>Yanıt
Cihaz yanıt gönderir `$iothub/methods/res/{status}/?$rid={request id}`burada:

* `status` Cihaz tarafından sağlanan yöntem yürütme durumunu bir özelliktir.
* `$rid` İstek kimliği: IOT Hub'ından alınan yöntemi çağrısının bir özelliktir.

Gövde, cihaz tarafından ayarlanır ve herhangi bir durum olabilir.

### <a name="amqp"></a>AMQP
#### <a name="method-invocation"></a>Yöntem çağırma
Adresinde alma bağlantı oluşturarak cihaz doğrudan yöntem isteklerini alır `amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound`

AMQP ileti yöntemi isteğinin temsil eden alma bağlantıyı ulaşır. Aşağıdakileri içerir:
* Karşılık gelen yöntemi yanıtı ile başarılı bir istek kimliği içeriyor. bağıntı kimliği özelliği
* Adlı bir uygulama özelliği `IoThub-methodname`, çağrılan yöntemin adını içerir
* JSON olarak yöntemi yükü içeren AMQP ileti gövdesi

#### <a name="response"></a>Yanıt
Cihaz yöntemi yanıt adresi döndürülecek gönderen bir bağlantı oluşturur. `amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound`

Yöntemin yanıtına gönderen bağlantıyı döndürülür ve şu şekilde yapılandırılmıştır:
* Yöntemin istek iletisinde geçirilen istek Kimliğini içeren bağıntı kimliği özelliği
* Adlı bir uygulama özelliği `IoThub-status`, kullanıcı içeren sağlanan yöntem durumu
* JSON olarak yöntemi yanıtı içeren AMQP ileti gövdesi

## <a name="additional-reference-material"></a>Ek başvuru malzemesi
IOT Hub Geliştirici Kılavuzu'nda olan diğer başvuru konularını içerir:

* [IOT Hub uç noktaları] [ lnk-endpoints] her IOT hub'ı ortaya koyan çalışma zamanı ve yönetim işlemleri için çeşitli uç noktaları açıklar.
* [Azaltma ve kotalar] [ lnk-quotas] uygulanan kotalar ve azaltma davranışı, IOT hub'ı kullandığınızda beklediğiniz açıklar.
* [Azure IOT cihaz ve hizmet SDK'ları] [ lnk-sdks] çeşitli dil IOT hub'ı ile etkileşim kuran hem cihaz hem de hizmet uygulamaları geliştirirken kullanabileceğiniz SDK'ları listeler.
* [Cihaz ikizleri, işler ve ileti yönlendirme için IOT Hub sorgu dili] [ lnk-query] IOT Hub'ından, cihaz ikizleri ve işler hakkında bilgi almak için kullanabileceğiniz IOT Hub sorgu dili açıklar.
* [IOT hub'ı MQTT desteği] [ lnk-devguide-mqtt] ve MQTT protokolünü için IOT hub'ı desteği hakkında daha fazla bilgi sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Doğrudan yöntemler kullanmayı öğrendiniz. artık aşağıdaki IOT Hub Geliştirici Kılavuzu makaleye de ilginizi çekebilir:

* [Birden fazla cihazda işleri zamanlama][lnk-devguide-jobs]

Bu makalede açıklanan kavramları bazıları denemek istiyorsanız, aşağıdaki IOT hub'ı öğreticide ilginizi çekebilir:

* [Doğrudan yöntemler kullanma][lnk-methods-tutorial]

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
