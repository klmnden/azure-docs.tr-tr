> [!div class="op_single_selector"]
> * [Cihaz: Node.js Service: Node.js](../articles/iot-hub/iot-hub-node-node-device-management-get-started.md)
> * [Aygıt: C# hizmet: C#](../articles/iot-hub/iot-hub-csharp-csharp-device-management-get-started.md)
> * [Aygıt: Java hizmet: Java](../articles/iot-hub/iot-hub-java-java-device-management-getstarted.md)
> * [Aygıt: Python hizmet: Python](../articles/iot-hub/iot-hub-python-python-device-management-get-started.md)

Arka uç uygulamaları kullanabileceğiniz Azure IOT Hub temelleri gibi [cihaz çifti] [ lnk-devtwin] ve [doğrudan yöntemleri][lnk-c2dmethod], uzaktan Başlat ve cihaz izlemek için cihazda yönetim eylemleri. Bu öğretici bir arka uç uygulaması ve cihaz uygulaması birlikte nasıl başlatmak ve IOT hub'ı kullanarak bir uzak aygıt yeniden başlatma izlemek için çalışabileceğini gösterir.

[!INCLUDE [iot-hub-basic](iot-hub-basic-whole.md)]

Arka uç uygulama bulutta cihaz yönetim işlemleri (örneğin, yeniden başlatma, Fabrika sıfırlaması ve üretici yazılımı güncelleştirmesi) başlatmak için doğrudan bir yöntem kullanır. Cihaz sorumludur:

* IOT Hub'ından gönderilen yöntemi istek işleme.
* Cihaz karşılık gelen aygıta özgü eylemini başlatılıyor.
* Durum güncelleştirmeleri aracılığıyla sağlama *özellikleri bildirilen* IOT Hub'ına.

Aygıt Yönetimi eylemleri ilerlemesini bildirmek için cihaz çifti sorguları çalıştırmak için bulutta bir arka uç uygulaması kullanın.

[lnk-devtwin]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
