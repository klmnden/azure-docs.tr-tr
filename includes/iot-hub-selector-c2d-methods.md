> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/quickstart-control-device-node.md)
> * [C#](../articles/iot-hub/quickstart-control-device-dotnet.md)
> * [Java](../articles/iot-hub/quickstart-control-device-java.md)
> * [Python](../articles/iot-hub/quickstart-control-device-python.md)

Azure IOT hub'ı güvenilir sağlayan tam olarak yönetilen bir hizmettir ve arka uç milyonlarca cihaz ve çözüm arasında güvenli çift yönlü iletişim. Önceki öğreticilerdeki ([IOT Hub ile çalışmaya başlama] ve [IOT Hub ile bulut buluttan cihaza iletileri gönderme]) IOT Hub'ın temel CİHAZDAN buluta ve bulut-cihaz Mesajlaşma işlevlerini göstermektedir. IOT Hub ayrıca cihazlara buluttan dayanıklı olmayan yöntemleri çağırma yeteneği sağlar. Doğrudan yöntemler başarılı olması veya çağrı durumunu bilmeniz izin vermek hemen (kullanıcı tarafından belirtilen zaman aşımından sonra) başarısız bir istek-yanıt etkileşimi benzer bir HTTPS yapılan bir cihazla temsil eder. Makaleyi [bir cihazda doğrudan yöntem çağırma] [ lnk-devguide-methods] doğrudan yöntemler daha ayrıntılı açıklanır ve bulut-cihaz iletilerini veya istenen özellikleri yerine doğrudan yöntemler kullanma konusunda rehberlik sağlar.

[!INCLUDE [iot-hub-basic](iot-hub-basic-whole.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* IOT hub oluşturma ve IOT hub'ınızda bir cihaz kimliği oluşturmak için Azure portalını kullanın.
* Bulut tarafından çağrılan doğrudan bir yöntemi olan bir sanal cihaz uygulaması oluşturma.
* Sanal cihaz uygulamasının, IOT hub'ı aracılığıyla doğrudan bir yöntemi çağıran bir konsol uygulaması oluşturacaksınız.


[lnk-devguide-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md

[IOT Hub ile bulut buluttan cihaza iletileri gönderme]: ../articles/iot-hub/iot-hub-csharp-csharp-c2d.md
[IOT Hub ile çalışmaya başlama]: ../articles/iot-hub/quickstart-send-telemetry-node.md