---
title: Azure IOT Hub MQTT destek anlama | Microsoft Docs
description: Geliştirici Kılavuzu - MQTT protokolünü kullanarak bir IOT Hub cihaz'e yönelik uç noktasına bağlanan cihazlar için destek. Yerleşik MQTT destekleyen Azure IOT cihaz SDK'ları hakkında bilgi içerir.
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: ''
ms.assetid: 1d71c27c-b466-4a40-b95b-d6550cf85144
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2018
ms.author: elioda
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b058f716c2435b70244c9293b1c5d897ec215f7f
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="communicate-with-your-iot-hub-using-the-mqtt-protocol"></a>MQTT protokolünü kullanarak, IOT hub ile iletişim

IOT hub'ı kullanarak IOT Hub cihaz uç ile iletişim kurmak cihazları sağlar:

* [MQTT v3.1.1] [ lnk-mqtt-org] 8883 bağlantı noktası
* Bağlantı noktası 443 üzerinden WebSocket üzerinden MQTT v3.1.1.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

IOT Hub ile tüm cihaz iletişimi TLS/SSL ile güvenli hale getirilmelidir. Bu nedenle, IOT hub'ı 1883 bağlantı noktası üzerinden güvenli olmayan bağlantıları desteklemiyor.

## <a name="connecting-to-iot-hub"></a>IOT hub'ına bağlama

Bir aygıt, bir IOT hub'na bağlanmak için MQTT protokolünü kullanabilirsiniz:

* Her iki kitaplıklarda [Azure IOT SDK'ları][lnk-device-sdks].
* Veya doğrudan MQTT protokolü.

## <a name="using-the-device-sdks"></a>Cihaz SDK'ları kullanma

[Cihaz SDK'ları] [ lnk-device-sdks] destekleyen MQTT Protokolü Java, Node.js, C, C# ve Python için kullanılabilir. Cihaz SDK'ları, bir IOT hub bağlantı kurmak için standart IOT Hub bağlantı dizesine kullanın. MQTT protokolünü kullanmak için istemci protokolünü parametresi ayarlanmalıdır **MQTT**. Varsayılan olarak, cihaz SDK'ları bir IOT Hub ile bağlanmak **CleanSession** bayrağı ayarlanmış **0** ve **QoS 1** IOT hub ile ileti alışverişi için.

Bir cihaz IOT hub'a bağlandığında, cihaz SDK'ları cihazın IOT hub'ı ile ileti alışverişinde bulunmak yöntemleri sağlar.

Aşağıdaki tabloda, desteklenen her dil için kod örnekleri içerir ve IOT Hub'ın MQTT protokolünü kullanarak bir bağlantı kurmak için kullanılacak parametreyi belirtir.

| Dil | Protokol parametresi |
| --- | --- |
| [Node.js][lnk-sample-node] |azure-iot-device-mqtt |
| [Java][lnk-sample-java] |IotHubClientProtocol.MQTT |
| [C][lnk-sample-c] |MQTT_Protocol |
| [C#][lnk-sample-csharp] |TransportType.Mqtt |
| [Python][lnk-sample-python] |IoTHubTransportProvider.MQTT |

### <a name="migrating-a-device-app-from-amqp-to-mqtt"></a>Cihaz uygulaması MQTT için AMQP geçirme

Kullanıyorsanız [cihaz SDK'ları][lnk-device-sdks], MQTT için AMQP kullanarak geçiş gerektirir daha önce belirtildiği gibi istemci başlatması Protokolü parametresinde değiştirme.

Bunu yaparken, aşağıdaki öğeleri olduğundan emin olun:

* MQTT bağlantıyı sonlandırır sırada AMQP birçok koşulları için hatalar döndürür. Sonuç olarak, özel durum mantığı işleme bazı değişiklikler gerektirebilir.
* MQTT desteklemiyor *Reddet* alırken işlemleri [bulut-cihaz iletilerini][lnk-messaging]. Arka uç uygulamanızı aygıt uygulamasından bir yanıt gerekiyorsa kullanmayı [doğrudan yöntemleri][lnk-methods].

## <a name="using-the-mqtt-protocol-directly"></a>MQTT protokolü kullanarak doğrudan

Bir aygıt cihaz SDK'ları kullanamıyorsanız, bağlantı noktası 8883 MQTT protokolünü kullanarak ortak cihaz Uç noktalara hala bağlanabilir. İçinde **BAĞLAN** paket aygıt, aşağıdaki değerleri kullanmalıdır:

* İçin **ClientID** alanında, kullanmak **DeviceID**.

* İçin **kullanıcı adı** alanında, kullanmak `{iothubhostname}/{device_id}/api-version=2016-11-14`, burada `{iothubhostname}` IOT hub'ın tam CName değil.

    Örneğin, IOT hub'ınızın adını ise **contoso.azure devices.net** ve Cihazınızı adı ise **MyDevice01**, tam **kullanıcıadı** alan içermelidir:

    `contoso.azure-devices.net/MyDevice01/api-version=2016-11-14`

* İçin **parola** alan, bir SAS belirteci kullanın. SAS belirteci biçimi HTTPS ve AMQP protokolleri aynıdır:

  `SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`

  > [!NOTE]
  > X.509 sertifikası kimlik doğrulamasını kullanıyorsanız, SAS belirteci parolaları gerekli değildir. Daha fazla bilgi için bkz: [X.509 güvenlik Azure IOT Hub'ınızı ayarlayın][lnk-x509]

  SAS belirteci oluşturma hakkında daha fazla bilgi için aygıt bölümüne bakın [IOT Hub'ı kullanarak güvenlik belirteçleri][lnk-sas-tokens].

  Test edilirken de kullanabilirsiniz [aygıt explorer] [ lnk-device-explorer] aracı hızlı bir şekilde kendi koda kopyalayıp bir SAS belirteci oluşturmak için:

  1. Git **Yönetim** sekmesinde **aygıt Explorer**.
  2. Tıklatın **SAS belirteci** (sağ üst).
  3. Üzerinde **SASTokenForm**, Cihazınızı seçin **DeviceID** açılır. Ayarlama, **TTL**.
  4. Tıklatın **Generate** , belirteç oluşturmak için.

     Oluşturulan SAS belirteci aşağıdaki yapıya sahiptir:

     `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`

     Bir parçası olarak kullanmak üzere bu belirteci **parola** MQTT kullanarak bağlanmak için bir alandır:

     `SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`

İçin MQTT bağlanmak ve paketleri bağlantısını kesmek, IOT hub'ı üzerinde bir olay sorunları **işlemlerini izleme** kanal. Bu olay, bağlantı sorunları gidermenize yardımcı olabilecek ek bilgi yok.

Cihaz uygulaması belirtebilirsiniz bir **olacak** içinde ileti **BAĞLAN** paket. Cihaz uygulaması kullanması gereken `devices/{device_id}/messages/events/{property_bag}` veya `devices/{device_id}/messages/events/{property_bag}` olarak **olacak** tanımlamak için konu adı **olacak** bir telemetri iletisi olarak iletilecek iletileri. Bu durumda, ağ bağlantısı kapalı ise, ancak bir **Bağlantıyı Kes** paket daha önce aygıttan alınmadı ardından IOT hub'ı gönderen **olacak** ileti sağlanan **BAĞLAN** telemetri kanal paket. Telemetri kanal ya da varsayılan olabilir **olayları** uç noktası veya yönlendirme IOT Hub tarafından tanımlanan özel bir uç nokta. İleti **ıothub MessageType** özellik değerini **olacak** atanmış.

### <a name="tlsssl-configuration"></a>TLS/SSL yapılandırma

Kullanılacak MQTT protokol doğrudan istemci *gerekir* TLS/SSL üzerinden. Bu adımı atlayın girişimleri bağlantı hataları ile başarısız olur.

TLS bağlantı kurmak için karşıdan yükle ve DigiCert Baltimore kök sertifikasının başvuru gerekebilir. Bağlantının güvenliğini sağlamak için Azure kullanan bir sertifikadır. Bu sertifikada bulabilirsiniz [Azure-IOT-sdk-c] [ lnk-sdk-c-certs] deposu. Bu sertifikalar hakkında daha fazla bilgi bulunabilir [Digicert'ın Web sitesi][lnk-digicert-root-certs].

Bu Python sürümünü kullanarak uygulamak nasıl bir örnek [Paho MQTT Kitaplığı] [ lnk-paho] Eclipse Foundation tarafından aşağıdaki gibi görünebilir.

Önce komut satırı ortamınızdan Paho Kitaplığı yükleyin:

```cmd/sh
pip install paho-mqtt
```

Ardından, istemci bir Python komut dosyasında uygulayın. Yer tutucuları aşağıdaki gibi değiştirin:

* `<local path to digicert.cer>` DigiCert Baltimore kök sertifikasını içeren bir yerel dosya yoludur. Sertifika bilgileri kopyalayarak bu dosyayı oluşturabilirsiniz [certs.c](https://github.com/Azure/azure-iot-sdk-c/blob/master/certs/certs.c) satırları c için Azure IOT SDK'sı dahil `-----BEGIN CERTIFICATE-----` ve `-----END CERTIFICATE-----`, kaldırma `"` başında ve her satırın sonuna işaretleri ve kaldırma `\r\n` her satırın sonuna karakter.
* `<device id from device registry>` IOT hub'ınıza eklenen cihazı kimliğidir.
* `<generated SAS token>` Bu makalede daha önce açıklandığı şekilde oluşturulan cihaz için bir SAS belirteci olur.
* `<iot hub name>` IOT hub'ınızın adıdır.

```python
from paho.mqtt import client as mqtt
import ssl

path_to_root_cert = "<local path to digicert.cer>"
device_id = "<device id from device registry>"
sas_token = "<generated SAS token>"
iot_hub_name = "<iot hub name>"

def on_connect(client, userdata, flags, rc):
  print ("Device connected with result code: " + str(rc))
def on_disconnect(client, userdata, rc):
  print ("Device disconnected with result code: " + str(rc))
def on_publish(client, userdata, mid):
  print ("Device sent message")

client = mqtt.Client(client_id=device_id, protocol=mqtt.MQTTv311)

client.on_connect = on_connect
client.on_disconnect = on_disconnect
client.on_publish = on_publish

client.username_pw_set(username=iot_hub_name+".azure-devices.net/" + device_id, password=sas_token)

client.tls_set(ca_certs=path_to_root_cert, certfile=None, keyfile=None, cert_reqs=ssl.CERT_REQUIRED, tls_version=ssl.PROTOCOL_TLSv1, ciphers=None)
client.tls_insecure_set(False)

client.connect(iot_hub_name+".azure-devices.net", port=8883)

client.publish("devices/" + device_id + "/messages/events/", "{id=123}", qos=1)
client.loop_forever()
```

### <a name="sending-device-to-cloud-messages"></a>Cihaz bulut iletilerini gönderme

Başarılı bir bağlantı yaptıktan sonra bir cihaz IOT Hub'ına kullanarak iletileri gönderebilirsiniz `devices/{device_id}/messages/events/` veya `devices/{device_id}/messages/events/{property_bag}` olarak bir **konu adı**. `{property_bag}` Öğesi url kodlanmış bir biçimde ek özelliklerle iletileri göndermek aygıt sağlar. Örneğin:

```text
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [!NOTE]
> Bu `{property_bag}` öğesini kullanan sorgu dizeleri HTTPS protokolü için olduğu gibi aynı kodlama.

IOT hub'ı uygulamaya özel davranışları listesi aşağıdadır:

* IOT hub'ı QoS 2 iletileri desteklemez. Cihaz uygulaması içeren bir ileti yayımlarsa **QoS 2**, IOT hub'ı ağ bağlantıyı kapatır.
* IOT hub'ı tut iletileri kalmaz. Bir cihaz içeren bir ileti gönderirse **TUT** bayrağı, IOT hub'ı ekler 1 olarak ayarlayın **x-opt-korumak** iletisi uygulama özelliği. Bu durumda, tut ileti kalıcı yerine IOT hub'ı, arka uç uygulamaya geçirir.
* IOT Hub, yalnızca cihaz başına bir etkin MQTT bağlantısını destekler. Yeni bir MQTT bağlantı aynı cihaz kimliği adına, IOT Hub'ın var olan bağlantıyı kesmek neden olur.

Daha fazla bilgi için bkz: [Geliştirici Kılavuzu Mesajlaşma][lnk-messaging].

### <a name="receiving-cloud-to-device-messages"></a>Bulut-cihaz iletilerini alma

IOT Hub'ından iletileri almak için bir cihaz kullanarak abone olmalısınız `devices/{device_id}/messages/devicebound/#` olarak bir **konu filtre**. Birden çok düzeyli joker `#` konu filtresinde yalnızca ek özellikleri'nde konu adı almak cihaz sağlamak için kullanılır. IOT hub'ı kullanımını izin verme `#` veya `?` alt konularını filtrelemek için joker karakter. IOT hub'ı genel amaçlı pub-sub Mesajlaşma Aracısı olmadığından, yalnızca belgelenen konu adları ve konu filtreleri destekler.

Cihaz herhangi bir ileti almazsa, cihaza özel uç noktasında başarıyla abone kadar IOT Hub'ından tarafından temsil edilen `devices/{device_id}/messages/devicebound/#` konu filtre. Bir abonelik kurulduktan sonra cihaz için abonelik süresi sonra gönderilen bulut-cihaz iletilerini alır. Aygıt ile bağlanıyorsa **CleanSession** bayrağı ayarlanmış **0**, abonelik farklı oturumlarında kalıcıdır. Bu durumda, sonraki cihaz bağlandığında ile **CleanSession 0** kendisine bağlı değilken gönderilen tüm bekleyen iletileri alır. Aygıt kullanıyorsa **CleanSession** bayrağı ayarlanmış **1** kendi cihaz uç noktasına abone kadar yine de, tüm iletileri IOT Hub'ından almaz.

IOT hub'ı sunar iletilerle **konu adı** `devices/{device_id}/messages/devicebound/`, veya `devices/{device_id}/messages/devicebound/{property_bag}` ileti özellikleri olduğunda. `{property_bag}` İleti özelliklerini URL kodlanmış anahtar/değer çiftleri içerir. Yalnızca uygulama özellikleri ve kullanıcı ayarlanabilir Sistem Özellikleri (gibi **MessageID** veya **correlationıd değeri**) özellik paketindeki dahil edilir. Sistem özelliği adlara sahip önek **$**, uygulama özellikleri ile önek özgün özellik adı kullanın.

Bir konu ile cihaz uygulamasının ne zaman aboneliği **QoS 2**, IOT hub'ı verir maksimum QoS düzey 1 **SUBACK** paket. Bundan sonra IOT Hub aygıtına QoS 1 kullanarak iletileri sunar.

### <a name="retrieving-a-device-twins-properties"></a>Bir cihaz çifti'nın özelliklerini alma

Bir cihaz ilk olarak, abone `$iothub/twin/res/#`, işlem yanıtları almak için. Ardından, boş bir iletiyi konu başlığına gönderir `$iothub/twin/GET/?$rid={request id}`, için doldurulan bir değerle **istek kimliği**. Hizmeti daha sonra konuda cihaz çifti verileri içeren bir yanıt iletisi gönderir `$iothub/twin/res/{status}/?$rid={request id}`, aynı kullanarak **istek kimliği** istek olarak.

İstek Kimliği olarak başına bir ileti özellik değeri için geçerli bir değer olabilir [IOT Hub Geliştirici Kılavuzu Mesajlaşma][lnk-messaging], ve durumunu bir tamsayı olarak doğrulandı.

Yanıt gövdesi cihaz çiftinin özellikler bölümü içerir. Aşağıdaki kod parçacığında gövdesi sınırlı kimlik kayıt defteri girişi, örneğin "Özellikler" üyesine gösterir:

```json
{
    "properties": {
        "desired": {
            "telemetrySendFrequency": "5m",
            "$version": 12
        },
        "reported": {
            "telemetrySendFrequency": "5m",
            "batteryLevel": 55,
            "$version": 123
        }
    }
}
```

Olası durum kodları şunlardır:

|Durum | Açıklama |
| ----- | ----------- |
| 200 | Başarılı |
| 429 | (Kısıtlanan) çok sayıda istek, göre [IOT Hub'ın azaltma][lnk-quotas] |
| 5** | Sunucu hataları |

Daha fazla bilgi için bkz: [cihaz çiftlerini Geliştirici Kılavuzu][lnk-devguide-twin].

### <a name="update-device-twins-reported-properties"></a>Cihaz çifti'nın bildirilen özelliklerini güncelleştir

Aşağıdaki sırada, bir cihaz IOT Hub cihaz çiftine bildirilen özelliklerinde güncelleştirme biçimini açıklanmaktadır:

1. Bir cihaz ilk abone olmalısınız `$iothub/twin/res/#` IOT Hub'ından işlem yanıtları almak için konu.

1. Bir cihaz için cihaz çifti güncelleştirme içeren bir ileti gönderir `$iothub/twin/PATCH/properties/reported/?$rid={request id}` konu. Bu iletiyi içeren bir **istek kimliği** değeri.

1. Hizmeti daha sonra konuda bildirilen özellikleri koleksiyon için yeni ETag değeri içeren bir yanıt iletisi gönderir `$iothub/twin/res/{status}/?$rid={request id}`. Bu yanıt iletisiyle aynı kullanan **istek kimliği** istek olarak.

İstek ileti gövdesi bildirilen özellikler için yeni değerleri içeren bir JSON belgesi, içerir. JSON belgesini içindeki her üyenin güncelleştirir veya cihaz çifti'nın belgeye karşılık gelen üye ekleyin. Ayarlanmış bir üye `null`, üyeyi içeren nesneyi siler. Örneğin:

```json
{
    "telemetrySendFrequency": "35m",
    "batteryLevel": 60
}
```

Olası durum kodları şunlardır:

|Durum | Açıklama |
| ----- | ----------- |
| 200 | Başarılı |
| 400 | Hatalı istek. Hatalı biçimlendirilmiş JSON |
| 429 | (Kısıtlanan) çok sayıda istek, göre [IOT Hub'ın azaltma][lnk-quotas] |
| 5** | Sunucu hataları |

Daha fazla bilgi için bkz: [cihaz çiftlerini Geliştirici Kılavuzu][lnk-devguide-twin].

### <a name="receiving-desired-properties-update-notifications"></a>Alıcı istenen özellikler güncelleştirme bildirimlerini

Bir aygıt bağlandığında, IOT hub'ı bildirimleri konu başlığına gönderir. `$iothub/twin/PATCH/properties/desired/?$version={new version}`, çözüm arka ucu tarafından gerçekleştirilen güncelleştirme içeriğini içerir. Örneğin:

```json
{
    "telemetrySendFrequency": "5m",
    "route": null
}
```

Özelliği güncelleştirmeleri ettirilmesi `null` değerleri JSON nesnesi üye silinip silinmediğini anlamına gelir.

> [!IMPORTANT]
> Yalnızca aygıtları bağlandığınızda IOT hub'ı değişiklik bildirimleri oluşturur. Uygulanacak emin olun [aygıt yeniden bağlanmayı akış] [ lnk-devguide-twin-reconnection] IOT Hub ve cihaz uygulaması arasında eşitlenen istenen özellikleri tutmak için.

Daha fazla bilgi için bkz: [cihaz çiftlerini Geliştirici Kılavuzu][lnk-devguide-twin].

### <a name="respond-to-a-direct-method"></a>Doğrudan bir yönteme yanıt

İlk olarak, bir abone olmak aygıtın `$iothub/methods/POST/#`. IOT hub'ı konuya yöntemi istekleri gönderir `$iothub/methods/POST/{method name}/?$rid={request id}`, ya geçerli bir JSON ya da boş bir gövde ile.

Cihaz yanıt vermek için konu ile geçerli JSON ya da boş gövdesi içeren bir ileti gönderir `$iothub/methods/res/{status}/?$rid={request id}`. Bu iletideki **istek kimliği** istek iletisinde aynı olmalıdır ve **durum** bir tamsayı olmalıdır.

Daha fazla bilgi için bkz: [yöntemi Geliştirici Kılavuzu doğrudan][lnk-methods].

### <a name="additional-considerations"></a>Diğer konular

Son yaparken, bulut tarafındaki MQTT protokol davranışı özelleştirmeniz gerekiyorsa gözden geçirmeniz gereken [Azure IOT protokolü ağ geçidini][lnk-azure-protocol-gateway]. Bu yazılım, yüksek performanslı özel protokol ağ geçidi dağıtmak için IOT Hub ile doğrudan arabirim sağlar. Azure IOT protokolü ağ geçidini brownfield MQTT dağıtımları uyum sağlayacak şekilde cihaz protokolü veya diğer özel protokoller özelleştirmenize olanak tanır. Bu yaklaşım, ancak çalıştırın ve bir özel protokol ağ geçidi çalışmasını gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

MQTT protokolü hakkında daha fazla bilgi için bkz: [MQTT belgelerine][lnk-mqtt-docs].

IOT hub'ı dağıtımınızı planlama hakkında daha fazla bilgi için bkz:

* [IOT cihaz katalog için Azure Onaylandı][lnk-devices]
* [Destek ek protokoller][lnk-protocols]
* [Event Hubs ile karşılaştırın][lnk-compare]
* [Ölçeklendirme, HA ve DR][lnk-scaling]

Daha fazla IOT hub'ı özelliklerini keşfetmek için bkz:

* [IOT Hub Geliştirici Kılavuzu][lnk-devguide]
* [Azure IOT Edge ile sınır cihazlarına Al dağıtma][lnk-iotedge]

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-mqtt-org]: http://mqtt.org/
[lnk-mqtt-docs]: http://mqtt.org/documentation
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/sdk/iot/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt_dm
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/iothub/device/samples
[lnk-sample-python]: https://github.com/Azure/azure-iot-sdk-python/tree/master/device/samples
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer
[lnk-sas-tokens]: iot-hub-devguide-security.md#use-sas-tokens-in-a-device-app
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-devices]: https://catalog.azureiotsuite.com/
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-x509]: iot-hub-security-x509-get-started.md

[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-messaging]: iot-hub-devguide-messaging.md

[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-twin-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow
[lnk-devguide-twin]: iot-hub-devguide-device-twins.md
[lnk-sdk-c-certs]: https://github.com/Azure/azure-iot-sdk-c/blob/master/certs/certs.c
[lnk-digicert-root-certs]: https://www.digicert.com/digicert-root-certificates.htm
[lnk-paho]: https://pypi.python.org/pypi/paho-mqtt
