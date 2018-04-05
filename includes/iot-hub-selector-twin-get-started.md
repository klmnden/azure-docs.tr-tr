> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
> * [C#/Node.js](../articles/iot-hub/iot-hub-csharp-node-twin-getstarted.md)
> * [C#](../articles/iot-hub/iot-hub-csharp-csharp-twin-getstarted.md)
> * [Java](../articles/iot-hub/iot-hub-java-java-twin-getstarted.md)
> * [Python](../articles/iot-hub/iot-hub-python-twin-getstarted.md)

Cihaz çiftleri, cihaz durumu bilgilerini (meta veriler, yapılandırmalar ve koşullar) depolayan JSON belgelerdir. IOT Hub cihaz çifti ona bağlanan her aygıt için devam ettirir.

[!INCLUDE [iot-hub-basic](iot-hub-basic-whole.md)]

Cihaz çiftlerini kullanın:

* Çözüm arka ucunuz cihaz meta verilerini depolar.
* Kullanılabilir özellikler ve koşullar (örneğin, kullanılan bağlantı yöntemi) gibi geçerli durumu bilgileri, cihaz uygulamanızdan bildirin.
* Cihaz uygulama ve arka uç uygulama arasında uzun süre çalışan iş akışları (örneğin, bellenim ve yapılandırma güncelleştirmeleri) durumunu eşitleyin.
* Cihaz meta verilerini, yapılandırma veya durumu sorgu.

Cihaz çiftlerini cihaz yapılandırmalarını ve koşullar sorgulamak için ve eşitleme için tasarlanmıştır. Ne zaman cihaz çiftlerini kullanılacağı hakkında daha fazla bilgi bulunabilir [cihaz çiftlerini anlamak][lnk-twins].

Cihaz çiftlerini bir IOT hub ' depolanır ve içerir:

* *Etiketler*, cihaz meta verilerini yalnızca çözüm arka ucu tarafından; erişilebilir
* *Özellikler istenen*, JSON nesnelerinin çözümü tarafından değiştirilebilir, cihaz uygulaması tarafından; uç ve observable yedekleyin ve
* *Özellikler bildirilen*, JSON nesnelerini cihaz uygulaması tarafından değiştirilebilir ve çözüm arka ucu tarafından okunabilir. Etiketleri ve özellikleri diziler içeremez, ancak nesneleri iç içe olamaz.

![][img-twin]

Ayrıca, çözüm arka ucu yukarıdaki tüm verilere dayalı cihaz çiftlerini sorgulayabilirsiniz.
Başvurmak [cihaz çiftlerini anlamak] [ lnk-twins] ve cihaz çiftlerini hakkında daha fazla bilgi için [IOT hub'ı sorgu dili] [ lnk-query] için başvuru sorgulama.


Bu öğretici şunların nasıl yapıldığını gösterir:

* Ekler bir arka uç uygulaması oluşturma *etiketleri* cihaz çifti ve kendi bağlantı kanalı olarak raporları bir sanal cihaz uygulamasının bir *özelliği bildirilen* cihaz çifti üzerinde.
* Filtreler etiketler ve daha önce oluşturduğunuz özellikleri kullanarak arka uç uygulamanızı aygıtlardan sorgu.

<!-- images -->
[img-twin]: media/iot-hub-selector-twin-get-started/twin.png

<!-- links -->
[lnk-query]: ../articles/iot-hub/iot-hub-devguide-query-language.md
[lnk-twins]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-d2c]: ../articles/iot-hub/iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md