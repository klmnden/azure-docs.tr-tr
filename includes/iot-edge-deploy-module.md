Azure IoT Edge'in önemli özelliklerinden biri buluttan IoT Edge cihazlarınıza modüller dağıtabilmektir. IoT Edge modülü, kapsayıcı olarak uygulanan yürütülebilir bir pakettir. Bu bölümde, simülasyon cihazınız için telemetri oluşturacak bir modülün dağıtımı yapacaksınız. 

1. Azure portalında IoT Hub'ınıza gidin.
1. **IoT Edge (önizleme)** sayfasına gidip IoT Edge cihazınızı seçin.
1. **Modülleri Ayarlama**'yı seçin.
1. **IoT Edge Modülü Ekle**'yi seçin.
1. **Ad** alanına `tempSensor` girin. 
1. **Görüntü URI'si** alanına `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` girin. 
1. Diğer ayarları değiştirmeden bırakın ve **Kaydet**'i seçin.

   ![Adı ve görüntü URI'sini girdikten sonra IoT Edge modülünü kaydedin](./media/iot-edge-deploy-module/name-image.png)

1. **Modül ekleme** adımına dönüp **İleri**’yi seçin.
1. **Rota belirtme** adımında **İleri**'yi seçin.
1. **Şablonu gözden geçirme** adımında **Gönder**’i seçin.
1. Cihaz ayrıntıları sayfasına dönüp **Yenile**’yi seçin. Yeni tempSensor modülünün IoT Edge çalışma zamanıyla birlikte çalıştığını görmelisiniz. 

   ![Dağıtılan modüller listesinde tempSensor modülünü görüntüleme][1]

<!-- Images -->
[1]: ../articles/iot-edge/media/tutorial-simulate-device-windows/view-module.png