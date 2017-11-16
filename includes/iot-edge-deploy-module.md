Azure IOT kenar anahtar özelliklerini birini modülleri, IOT sınır cihazları buluttan dağıtamaz yapılıyor. Bir IOT kenar modülü, bir kapsayıcı olarak uygulanan bir yürütülebilir paketidir. Bu bölümde, sanal cihaz için telemetri oluşturması bir modül dağıtın. 

1. Azure portalında IOT hub'ına gidin.
1. Git **IOT kenar (Önizleme)** ve IOT sınır cihazı seçin.
1. Seçin **ayarlamak modülleri**.
1. Seçin **IOT kenar Modül Ekle**.
1. İçinde **adı** alanına, `tempSensor`. 
1. İçinde **görüntü URI** alanına, `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`. 
1. Diğer ayarları değiştirmeden bırakın ve seçin **kaydetmek**.
1. Geri **modülleri ekleme** adım, select **sonraki**.
1. İçinde **belirtin yolları** adım, select **sonraki**.
1. İçinde **gözden geçirme şablonu** adım, select **gönderme**.
1. Cihaz ayrıntıları sayfasına dönün ve seçin **yenileme**. IOT kenar çalışma zamanı çalıştıran yeni tempSensor modülü görmeniz gerekir. 

   ![Görünüm tempSensor dağıtılan modüller listesi][1]

<!-- Images -->
[1]: ../articles/iot-edge/media/tutorial-simulate-device-windows/view-module.png