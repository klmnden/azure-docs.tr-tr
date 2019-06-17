---
title: Azure IOT hub'ı doğrudan yöntemleri anlama | Microsoft Docs
description: Geliştirici Kılavuzu - kod cihazlarınızda bir hizmet uygulamasından çağırmak için doğrudan yöntemleri kullanın.
author: nberdy
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/17/2018
ms.author: nberdy
ms.openlocfilehash: d7c63ffe5a318507053f59bf3a18242ee8c327a0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61327763"
---
# <a name="understand-and-invoke-direct-methods-from-iot-hub"></a>Anlama ve IOT Hub'ından doğrudan metotları çağırma

IOT Hub buluttan cihazlar üzerinde doğrudan metotları çağırma yeteneği sağlar. Başarılı veya başarısız hemen (kullanıcı tarafından belirtilen zaman aşımından sonra) doğrudan yöntemler için HTTP çağrısı benzer bir cihazla bir istek-yanıt etkileşimi temsil eder. Bu yaklaşım, hemen işlem boyunca cihaz yanıt verebilmesi olmasına bağlı olarak farklı olduğu senaryolar için kullanışlıdır.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Her cihaz yöntemi tek bir cihaz hedefler. [İşleri birden fazla cihazda zamanlama](iot-hub-devguide-jobs.md) birden fazla cihazda doğrudan metotları çağırma ve yöntem çağırma bağlantısı kesilmiş cihazlar için zamanlamak için bir yolunu nasıl sağlayacağınızı gösterir.

Hiç kimseyle **hizmetini bağlama** IOT hub'ı izinlerini cihazda yöntem çağırma.

Doğrudan yöntemler istek-yanıt deseni izler ve bunların sonucunun anında onay gerektiren iletişimleri için tasarlanmıştır. Örneğin, etkileşimli denetim üzerinde fan kapatma gibi cihaz.

Başvurmak [bulut buluttan cihaza iletişim Kılavuzu](iot-hub-devguide-c2d-guidance.md) arasında istenen özellikleri kullanarak bu konuda şüpheleriniz varsa, yöntemler veya Bulut-cihaz iletilerini doğrudan.

## <a name="method-lifecycle"></a>Yöntem yaşam döngüsü

Doğrudan yöntemler cihazda uygulandı ve sıfır veya daha fazla giriş doğru örnekleyin için yöntemi yükünde gerektirebilir. Hizmet kullanıma yönelik bir URI aracılığıyla bir doğrudan yöntem çağırma (`{iot hub}/twins/{device id}/methods/`). Bir cihaz doğrudan bir cihaza özgü MQTT konu yöntemleri alır (`$iothub/methods/POST/{method name}/`) veya AMQP bağlantıları aracılığıyla ( `IoThub-methodname` ve `IoThub-status` uygulama özellikleri). 

> [!NOTE]
> Bir cihazda doğrudan yöntem çağırdığınızda, özellik adları ve değerleri yalnızca US-ASCII yazdırılabilir içerebilir alfasayısal karakterler, aşağıdaki, tüm hariç: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``
> 

Doğrudan yöntemleri zaman uyumludur ve ya da başarılı veya başarısız zaman aşımı süresinden sonra (varsayılan: 30 saniye, 300 saniye olarak ayarlanabilir yukarı). Doğrudan yöntemler, cihaz çevrimiçi ve alıcı komutları adıdır ve yalnızca, yapacak bir cihaz istediğiniz etkileşimli senaryolarda yararlıdır. Örneğin, bir ışık telefonda açmak. Bu senaryolarda, bulut hizmeti sonucuna olabildiğince çabuk davranabilir bir anında başarı veya başarısızlık görmek istiyorsunuz. Cihaz yöntemi sonucunda bazı ileti gövdesini döndürebilir, ancak bu yöntem Bunu yapmak gerekli değildir. Üzerinde sıralama garantisi veya yöntem çağrılarının tüm eşzamanlılık semantiği yoktur.

Doğrudan yöntemler yalnızca HTTPS bulut yan ve MQTT veya AMQP cihaz tarafındaki.

Yöntem istekleri ve yanıtları için bir JSON belgesi en fazla 128 KB yüktür.

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>Bir arka uç uygulamasından bir doğrudan yöntem çağırma

Artık, bir arka uç uygulamasından bir doğrudan yöntem çağırma.

### <a name="method-invocation"></a>Yöntem çağırma

Bir cihazda doğrudan yöntem çağrılarını şu öğelerden oluşur HTTPS çağrıları vardır:

* *İstek URI'si* cihaz ile birlikte özel [API sürümü](/rest/api/iothub/service/invokedevicemethod):

    ```http
    https://fully-qualified-iothubname.azure-devices.net/twins/{deviceId}/methods?api-version=2018-06-30
    ```

* POST *yöntemi*

* *Üstbilgileri* yetkilendirme içerir, istek kimliği, içerik türü ve içerik kodlaması.

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

Arka uç uygulaması, aşağıdaki öğelerden oluşan bir yanıt alır:

* *HTTP durum kodu*, şu anda bir 404 hatası cihazlar dahil olmak üzere bağlı olan IOT Hub'ından gelen hatalar için kullanılır.

* *Üstbilgileri* içeren ETag, istek kimliği, içerik türü ve içerik kodlaması.

* Bir JSON *gövdesi* şu biçimde:

    ```json
    {
        "status" : 201,
        "payload" : {...}
    }
    ```

    Her ikisi de `status` ve `body` cihaz tarafından sağlanan ve cihazın kendi durum kodu ve/veya açıklamanın ile yanıt vermek için kullanılır.

### <a name="method-invocation-for-iot-edge-modules"></a>Yöntem çağırma için IOT Edge modülleri

Bir modül kimliği içerisinde desteklendiği kullanarak doğrudan metotları çağırma [IOT hizmeti istemci C# SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.Devices/).

Bu amaçla kullanabileceğiniz `ServiceClient.InvokeDeviceMethodAsync()` yöntemi ve geçişinde `deviceId` ve `moduleId` parametre olarak.

## <a name="handle-a-direct-method-on-a-device"></a>Tanıtıcı bir cihazda doğrudan yöntem

IOT cihaz üzerinde doğrudan bir yöntemi nasıl ele alınacağını göz atalım.

### <a name="mqtt"></a>MQTT

Aşağıdaki bölümde için MQTT protokolüdür.

#### <a name="method-invocation"></a>Yöntem çağırma

Cihaz, MQTT konusunda doğrudan yöntem isteklerini alır: `$iothub/methods/POST/{method name}/?$rid={request id}`. 5\. abonelik başına cihaz sayısı sınırlıdır. Bu nedenle, doğrudan her yöntem için tek tek abone değil önerilir. Bunun yerine abone düşünün `$iothub/methods/POST/#` ve ardından teslim edilen iletiler, istenen yöntem adlarına göre filtreleyin.

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

İçin AMQP protokolünü bölümüdür.

#### <a name="method-invocation"></a>Yöntem çağırma

Adresinde alma bağlantı oluşturarak cihaz doğrudan yöntem istekleri alan `amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound`.

AMQP ileti yöntemi isteğinin temsil eden alma bağlantıyı ulaşır. Bunu, aşağıdaki bölümleri içerir:

* Karşılık gelen yöntemi yanıtı ile başarılı bir istek kimliği içeriyor. bağıntı kimliği özelliği.

* Adlı bir uygulama özelliği `IoThub-methodname`, çağrılan yöntemin adını içerir.

* JSON olarak yöntemi yükü içeren AMQP ileti gövdesi.

#### <a name="response"></a>Yanıt

Cihaz yöntemi yanıt adresi döndürülecek gönderen bir bağlantı oluşturur `amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound`.

Yöntemin yanıtına gönderen bağlantıyı döndürülür ve şu şekilde yapılandırılmıştır:

* İstek Kimliği içeriyor. bağıntı kimliği özelliği yöntemin istek iletisinde geçirildi.

* Adlı bir uygulama özelliği `IoThub-status`, sağlanan yöntem durumu kullanıcı içerir.

* JSON olarak yöntemi yanıtı içeren AMQP ileti gövdesi.

## <a name="additional-reference-material"></a>Ek başvuru malzemesi

IOT Hub Geliştirici Kılavuzu'nda olan diğer başvuru konularını içerir:

* [IOT Hub uç noktaları](iot-hub-devguide-endpoints.md) her IOT hub'ı ortaya koyan çalışma zamanı ve yönetim işlemleri için çeşitli uç noktaları açıklar.

* [Azaltma ve kotalar](iot-hub-devguide-quotas-throttling.md) uygulanan kotalar ve azaltma davranışı, IOT hub'ı kullandığınızda beklediğiniz açıklar.

* [Azure IOT cihaz ve hizmet SDK'ları](iot-hub-devguide-sdks.md) çeşitli dil IOT hub'ı ile etkileşim kuran hem cihaz hem de hizmet uygulamaları geliştirirken kullanabileceğiniz SDK'ları listeler.

* [Cihaz ikizleri, işler ve ileti yönlendirme için IOT Hub sorgu dili](iot-hub-devguide-query-language.md) IOT Hub'ından, cihaz ikizleri ve işler hakkında bilgi almak için kullanabileceğiniz IOT Hub sorgu dili açıklar.

* [IOT hub'ı MQTT desteği](iot-hub-mqtt-support.md) ve MQTT protokolünü için IOT hub'ı desteği hakkında daha fazla bilgi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Doğrudan yöntemler kullanmayı öğrendiniz. artık aşağıdaki IOT Hub Geliştirici Kılavuzu makaleye de ilginizi çekebilir:

* [Birden fazla cihazda işleri zamanlama](iot-hub-devguide-jobs.md)

Bu makalede açıklanan kavramları bazıları denemek istiyorsanız, aşağıdaki IOT hub'ı öğreticide ilginizi çekebilir:

* [Doğrudan yöntemler kullanma](quickstart-control-device-node.md)
* [VS Code için Azure IOT araçları ile cihaz Yönetimi](iot-hub-device-management-iot-toolkit.md)
