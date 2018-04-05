> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-twin-how-to-configure.md)
> * [C#/Node.js](../articles/iot-hub/iot-hub-csharp-node-twin-how-to-configure.md)
> * [C#](../articles/iot-hub/iot-hub-csharp-csharp-twin-how-to-configure.md)
> * [Java](../articles/iot-hub/iot-hub-java-java-twin-how-to-configure.md)
> * [Python](../articles/iot-hub/iot-hub-python-python-twin-how-to-configure.md)
> 
> 

## <a name="introduction"></a>Giriş

İçinde [IOT Hub cihaz çiftlerini ile çalışmaya başlama][lnk-twin-tutorial], cihaz meta veriler kullanılarak ayarlanan öğrendiniz *etiketleri*. Kullanarak bir aygıtı uygulama aygıt koşullar alınan *özellikleri bildirilen*ve SQL benzeri bir dil kullanarak bu bilgileri sorgulanamadı.

Bu öğretici cihaz çifti kişinin kullanımını açıklar *özelliklerini istenen* ve *özellikleri bildirilen* uzaktan cihaz uygulamalarını yapılandırmak için. Bildirilen ve cihaz çifti istenen özelliklerinde bir cihaz uygulaması çok adımlı yapılandırılmasını sağlar ve bu işlemin durumunu görünürlüğünü cihazlara sunar. Cihaz yapılandırmaları rolü ile ilgili daha fazla bilgi bulabilirsiniz [cihaz yönetimine genel bakış IOT Hub ile][lnk-dm-overview].

[!INCLUDE [iot-hub-basic](iot-hub-basic-whole.md)]

Yüksek bir düzeyde cihaz çiftlerini kullanarak belirli komutları göndermek yerine yönetilen cihazlar için istenen yapılandırmayı belirtmek çözüm arka ucu sağlar. (Burada belirli cihaz koşullar etkileyen hemen belirli komutları yürütmek olanağı IOT senaryolarda önemli), yapılandırmasını güncelleştirmek için en iyi şekilde ayarlama sorumlu cihaz, geçerli durumu ve olası sürekli Raporlama sırasında Güncelleştirme işlemini hata koşulları. Bu desen büyük kümeleri, cihazların yönetimi için enstrümental aynıdır cihazlara yapılandırma işleminin durumunu çözüm arka uç tam görünürlüğünü sağlar.

> [!TIP]
> Burada aygıtları denetlenir (örneğin, kullanıcı tarafından denetlenen bir uygulamadan fan etkinleştirdikten) daha etkileşimli bir şekilde senaryolarda kullanmayı [doğrudan yöntemleri][lnk-methods].

Bu öğreticide, cihaz uygulaması bir yapılandırma güncelleştirmesi şekilde çözüm arka ucu bir hedef cihaz telemetri yapılandırmasını değiştirir. Örneğin, bir yapılandırma güncelleştirmesi Bu öğretici basit bir gecikmeyle taklit eden bir yazılım modülü yeniden gerektiren.

Çözüm arka ucu yapılandırmasını cihaz çifti'nın istenen özelliklerinde aşağıdaki şekilde depolar:

        {
            ...
            "properties": {
                ...
                "desired": {
                    "telemetryConfig": {
                        "configId": "{id of the configuration}",
                        "sendFrequency": "{config}"
                    }
                }
                ...
            }
            ...
        }

Yapılandırmaları karmaşık nesneler olabileceği için benzersiz kimlikler atanan (karmaları veya [GUID'ler][lnk-guid]).


Cihaz uygulaması istenen özelliği yansıtma geçerli yapılandırması raporları **telemetryConfig** bildirilen özellikleri:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "configId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Success",
                    }
                }
                ...
            }
        }

Not nasıl bildirilen **telemetryConfig** ek bir özelliğe sahiptir **durum**, yapılandırma güncelleştirme işleminin durumunu bildirmek için kullanılan.

Yeni istenen yapılandırma alındığında, cihaz uygulamasının durumunu değiştirerek bekleyen yapılandırma raporları:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "configId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Pending",
                        "pendingConfig": {
                            "configId": "{id of the pending configuration}",
                            "sendFrequency": "{pending configuration}"
                        }
                    }
                }
                ...
            }
        }

Ardından, sonraki bir zamanda, cihaz uygulaması başarı veya başarısızlık bu işlemin özelliği güncelleştirerek bildirir. Çözüm arka ucu, herhangi bir zamanda cihazlara yapılandırma işleminin durumunu sorgulayabilirsiniz.

Bu öğretici şunların nasıl yapıldığını gösterir:

* Çözüm arka ucu yapılandırma güncelleştirmeleri alıp olarak birden çok güncelleştirme raporları bir sanal cihaz uygulaması oluşturma *özellikleri bildirilen* işlem yapılandırmasını güncelleştirin.
* Bir aygıt, istenen yapılandırma güncelleştirir ve daha sonra yapılandırma güncelleştirme işlemi sorgular bir arka uç uygulaması oluşturun.

<!-- links -->

[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: ../articles/iot-hub/iot-hub-device-management-overview.md
[lnk-twin-tutorial]: ../articles/iot-hub/iot-hub-node-node-twin-getstarted.md
[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier
