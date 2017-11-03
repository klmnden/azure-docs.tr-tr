> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-twin-how-to-configure.md)
> * [C#/node.js](../articles/iot-hub/iot-hub-csharp-node-twin-how-to-configure.md)
> * [C#](../articles/iot-hub/iot-hub-csharp-csharp-twin-how-to-configure.md)
> * [Java](../articles/iot-hub/iot-hub-java-java-twin-how-to-configure.md)
> 
> 

## <a name="introduction"></a>Giriş

İçinde [IOT Hub cihaz çiftlerini ile çalışmaya başlama][lnk-twin-tutorial], cihaz meta verilerini kullanarak, çözüm arka ucu ayarlama öğrendiniz *etiketleri*, rapor cihaz uygulaması cihaz koşulları kullanarak *özellikleri bildirilen*ve SQL benzeri bir dil kullanarak bu bilgileri sorgulayabilirsiniz.

Bu öğreticide, cihaz çifti 's kullanmayı öğrenin *özelliklerini istenen* ile birlikte *özellikleri bildirilen*, uzaktan cihaz uygulamalarını yapılandırmak için. Daha belirgin olarak Bu öğretici, bir cihaz çifti 's nasıl bildirilen gösterir ve istenen özellikleri cihaz uygulamasının çok adımlı yapılandırmasını etkinleştir ve bu işlemin durumunu çözüm arka ucu için görünürlük cihazlara sağlayın. Cihaz yapılandırmaları rolü ile ilgili daha fazla bilgi bulabilirsiniz [cihaz yönetimine genel bakış IOT Hub ile][lnk-dm-overview].

Yüksek bir düzeyde cihaz çiftlerini kullanarak belirli komutları göndermek yerine yönetilen cihazlar için istenen yapılandırmayı belirtmek çözüm arka ucu sağlar. Bu cihaz (burada belirli cihaz koşullar etkileyen hemen belirli komutları yürütmek olanağı IOT senaryolarda önemli), yapılandırmasını güncelleştirmek için en iyi şekilde ayarlama sorumlu koyar sürekli olarak çözüm arka ucuna raporlama oluştu Güncelleştirme işlemini olası hata koşulları ve geçerli durumu. Bu desen büyük kümeleri, cihazların yönetimi için enstrümental aynıdır çözüm arka ucu yapılandırma işleminin durumunu tam görünürlüğünü cihazlara sahip olmasını sağlar.

> [!NOTE]
> Burada aygıtları denetlenir daha etkileşimli bir şekilde (kullanıcı tarafından denetlenen bir uygulamadan fan etkinleştirin) senaryolarında kullanmayı [doğrudan yöntemleri][lnk-methods].
> 
> 

Bu öğreticide, bir hedef cihaz telemetri yapılandırmasını çözüm arka ucu değiştirir ve sonuç olarak, cihaz uygulaması yapılandırmasını güncelleştirme uygulamak için çok adımlı bir işlemi izler (örneğin, bir yazılım modülü yeniden, bu gerektiren Öğreticisi Basit bir gecikmeyle taklit eder).

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

> [!NOTE]
> Yapılandırmaları karmaşık nesneler olabileceği için benzersiz kimlikler atanan (karmaları veya [GUID'ler][lnk-guid]) kendi karşılaştırmaları basitleştirmek için.
> 
> 

Cihaz uygulaması istenen özelliği yansıtma geçerli yapılandırması raporları **telemetryConfig** bildirilen özellikleri:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Success",
                    }
                }
                ...
            }
        }

Not nasıl bildirilen **telemetryConfig** ek bir özelliğe sahiptir **durum**, yapılandırma güncelleştirme işleminin durumunu bildirmek için kullanılan.

Yeni bir istenen yapılandırma alındığında, cihaz uygulaması bekleyen yapılandırma bilgileri değiştirerek raporları:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Pending",
                        "pendingConfig": {
                            "changeId": "{id of the pending configuration}",
                            "sendFrequency": "{pending configuration}"
                        }
                    }
                }
                ...
            }
        }

Ardından, sonraki bir zamanda, cihaz uygulaması başarı veya başarısızlık bu işlemin yukarıdaki özellik güncelleştirerek bildirir.
Nasıl çözüm arka ucu cihazlara yapılandırma işleminin durumunu sorgulamak için istediğiniz zaman, bulabildiği unutmayın.

Bu öğretici şunların nasıl yapıldığını gösterir:

* Çözüm arka ucu yapılandırma güncelleştirmeleri alıp olarak birden çok güncelleştirme raporları bir sanal cihaz uygulaması oluşturma *özellikleri bildirilen* işlem yapılandırmasını güncelleştirin.
* Bir aygıt, istenen yapılandırma güncelleştirir ve daha sonra yapılandırma güncelleştirme işlemi sorgular bir arka uç uygulaması oluşturun.

<!-- links -->

[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: ../articles/iot-hub/iot-hub-device-management-overview.md
[lnk-twin-tutorial]: ../articles/iot-hub/iot-hub-node-node-twin-getstarted.md
[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier
