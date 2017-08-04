# Genel Bakış
## [Azure ve IoT](iot-hub-what-is-azure-iot.md)
## [Azure IoT Hub nedir?](iot-hub-what-is-iot-hub.md)
## [Cihaz yönetimine genel bakış](iot-hub-device-management-overview.md)

# [Kullanmaya Başlama](iot-hub-get-started.md)

## Cihazınızı ayarlama
### [Bilgisayarınızda bir cihazı benzetme](iot-hub-get-started-simulated.md)
#### [.NET](iot-hub-csharp-csharp-getstarted.md)
#### [Java](iot-hub-java-java-getstarted.md)
#### [Node.js](iot-hub-node-node-getstarted.md)
#### [Python](iot-hub-python-getstarted.md)

### [Çevrimiçi simülatör kullanma](iot-hub-raspberry-pi-web-simulator-get-started.md)

### [Fiziksel cihaz kullanma](iot-hub-get-started-physical.md)
#### [Node.js ile Raspberry Pi](iot-hub-raspberry-pi-kit-node-get-started.md)
#### [C ile Raspberry Pi](iot-hub-raspberry-pi-kit-c-get-started.md)
#### [Python ile Raspberry Pi](iot-hub-raspberry-pi-kit-python-get-started.md)

#### [Node.js ile Intel Edison](iot-hub-intel-edison-kit-node-get-started.md)
#### [C ile Intel Edison](iot-hub-intel-edison-kit-c-get-started.md)

#### [Arduino IDE ile Adafruit Feather HUZZAH ESP8266](iot-hub-arduino-huzzah-esp8266-get-started.md)
#### [Arduino IDE ile Sparkfun ESP8266 Thing Dev](iot-hub-sparkfun-esp8266-thing-dev-get-started.md)
#### [Arduino IDE ile Adafruit Feather M0](iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md)

#### IoT Gateway Başlangıç Paketini kullanma
##### [Intel NUC cihazını ağ geçidi olarak ayarlama](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
##### [Ağ geçidini IoT Hub’a bağlama](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)
##### [Veri dönüştürme için ağ geçidi kullanma](iot-hub-gateway-kit-c-use-iot-gateway-for-data-conversion.md)

## Genişletilmiş IoT senaryoları
### [iothub-explorer ile bulut cihaz mesajlaşmasını yönetme](iot-hub-explorer-cloud-device-messaging.md)
### [IoT Hub iletilerini Azure veri depolamaya kaydetme](iot-hub-store-data-in-azure-table-storage.md)
### [Power BI’da Veri Görselleştirme](iot-hub-live-data-visualization-in-power-bi.md)
### [Web Apps ile Veri Görselleştirme](iot-hub-live-data-visualization-in-web-apps.md)
### [Azure Machine Learning kullanarak hava durumu tahmini](iot-hub-weather-forecast-machine-learning.md)
### [iothub-explorer ile cihaz yönetimi](iot-hub-device-management-iothub-explorer.md)
### [Logic Apps ile uzaktan izleme ve bildirimler](iot-hub-monitoring-notifications-with-azure-logic-apps.md)

# Nasıl yapılır?
## Planlama
### [IoT Hub ile Event Hubs karşılaştırması](iot-hub-compare-event-hubs.md)
### [Çözümünüzü ölçeklendirme](iot-hub-scaling.md)
### [Yüksek kullanılabilirlik ve olağanüstü durum kurtarma](iot-hub-ha-dr.md)
### [Ek protokollerin desteklenmesi](iot-hub-protocol-gateway.md)
## [Geliştirme](iot-hub-how-to.md)
### [Geliştirici kılavuzu](iot-hub-devguide.md)
#### [Cihazdan buluta özellik kılavuzu](iot-hub-devguide-d2c-guidance.md)
#### [Buluttan cihaza özellik kılavuzu](iot-hub-devguide-c2d-guidance.md)
#### [İleti alma ve gönderme](iot-hub-devguide-messaging.md)
##### [IoT Hub’a cihazdan buluta iletileri gönderme](iot-hub-devguide-messages-d2c.md)
##### [Cihazdan buluta iletilerini yerleşik uç noktadan okuma](iot-hub-devguide-messages-read-builtin.md)
##### [Cihazdan buluta iletileri için özel uç noktalar ve yönlendirme kuralları kullanma](iot-hub-devguide-messages-read-custom.md)
##### [IoT Hub’dan buluttan cihaza iletileri gönderme](iot-hub-devguide-messages-c2d.md)
##### [IoT Hub iletilerini oluşturma ve okuma](iot-hub-devguide-messages-construct.md)
##### [İletişim protokolü seçme](iot-hub-devguide-protocols.md)
#### [Bir cihazdan karşıya dosya yükleme](iot-hub-devguide-file-upload.md)
#### [Cihaz kimliklerini yönetme](iot-hub-devguide-identity-registry.md)
#### [IoT Hub’a erişimi denetleme](iot-hub-devguide-security.md)
#### [Cihaz ikizlerini anlama](iot-hub-devguide-device-twins.md)
#### [Bir cihazda doğrudan yöntemleri çağırma](iot-hub-devguide-direct-methods.md)
#### [Birden fazla cihazda işleri zamanlama](iot-hub-devguide-jobs.md)
#### [IoT Hub uç noktaları](iot-hub-devguide-endpoints.md)
#### [Sorgu dili](iot-hub-devguide-query-language.md)
#### [Kotalar ve azaltma](iot-hub-devguide-quotas-throttling.md)
#### [Fiyatlandırma örnekleri](iot-hub-devguide-pricing.md)
#### [Cihaz ve hizmet SDK’ları](iot-hub-devguide-sdks.md)
#### [MQTT desteği](iot-hub-mqtt-support.md)
#### [Sözlük](iot-hub-devguide-glossary.md)
### [C için IoT cihaz SDK'sını kullanma](iot-hub-device-sdk-c-intro.md)
#### [IoTHubClient kullanma](iot-hub-device-sdk-c-iothubclient.md)
#### [Seri hale getirici kullanma](iot-hub-device-sdk-c-serializer.md)
### Cihazdan buluta iletilerini işleme
#### [.NET](iot-hub-csharp-csharp-process-d2c.md)
#### [Java](iot-hub-java-java-process-d2c.md)
### Buluttan cihaza iletileri gönderme
#### [.NET](iot-hub-csharp-csharp-c2d.md)
#### [Java](iot-hub-java-java-c2d.md)
#### [Node.js](iot-hub-node-node-c2d.md)
### [Cihazlardan dosya yükleme](iot-hub-csharp-csharp-file-upload.md)
### Cihaz ikizlerini kullanmaya başlama
#### [Node.js arka ucu/Node.js cihazı](iot-hub-node-node-twin-getstarted.md)
#### [.NET arka ucu/Node.js cihazı](iot-hub-csharp-node-twin-getstarted.md)
#### [.NET arka ucu/.NET cihazı](iot-hub-csharp-csharp-twin-getstarted.md)
#### [Java arka ucu/Java cihazı](iot-hub-java-java-twin-getstarted.md)
### Doğrudan yöntemler kullanma
#### [Node.js arka ucu/Node.js cihazı](iot-hub-node-node-direct-methods.md)
#### [.NET arka ucu/Node.js cihazı](iot-hub-csharp-node-direct-methods.md)
#### [.NET arka ucu/.NET cihazı](iot-hub-csharp-csharp-direct-methods.md)
#### [Java arka ucu/Java cihazı](iot-hub-java-java-direct-methods.md)
### Cihaz yönetimini kullanmaya başlama
#### [Node.js arka ucu/Node.js cihazı](iot-hub-node-node-device-management-get-started.md)
#### [.NET arka ucu/Node.js cihazı](iot-hub-csharp-node-device-management-get-started.md)
#### [Java arka ucu/Java cihazı](iot-hub-java-java-device-management-getstarted.md)
### İkiz özelliklerini kullanma
#### [Node.js arka ucu/Node.js cihazı](iot-hub-node-node-twin-how-to-configure.md)
#### [.NET arka ucu/Node.js cihazı](iot-hub-csharp-node-twin-how-to-configure.md)
#### [.NET arka ucu/.NET cihazı](iot-hub-csharp-csharp-twin-how-to-configure.md)
### Cihaz üretici yazılımını güncelleştirmek için cihaz işlerini kullanma
#### [Node arka ucu/Node cihazı](iot-hub-node-node-firmware-update.md)
#### [.NET arka ucu/Node.js cihazı](iot-hub-csharp-node-firmware-update.md)
### İşleri zamanlama ve yayınlama
#### [Node.js arka ucu/Node.js cihazı](iot-hub-node-node-schedule-jobs.md)
#### [.NET arka ucu/Node.js cihazı](iot-hub-csharp-node-schedule-jobs.md)
#### [Java](iot-hub-java-java-schedule-jobs.md)
## Yönet
### IoT hub oluşturma 
#### [Portalı kullanma](iot-hub-create-through-portal.md)
#### [PowerShell kullanma](iot-hub-create-using-powershell.md)
#### [CLI 2.0’ı kullanma](iot-hub-create-using-cli.md)
#### [CLI’yi kullanma](iot-hub-create-using-cli-nodejs.md)
#### [REST API kullanma](iot-hub-rm-rest.md)
#### [PowerShell’den şablon kullanma](iot-hub-rm-template-powershell.md)
#### [.NET’ten şablon kullanma](iot-hub-rm-template.md)
### Karşıya dosya yüklemeyi yapılandırma
#### [Portalı kullanma](iot-hub-configure-file-upload.md)
#### [PowerShell kullanma](iot-hub-configure-file-upload-powershell.md)
#### [CLI 2.0’ı kullanma](iot-hub-configure-file-upload-cli.md)
### [IoT cihazlarını toplu yönetme](iot-hub-bulk-identity-mgmt.md)
### [Ölçümleri kullanma](iot-hub-metrics.md)
### [İşlemleri izleme](iot-hub-operations-monitoring.md)
### [IP filtrelemeyi yapılandırma](iot-hub-ip-filtering.md)
## Güvenlik
### [Baştan sona güvenlik](iot-hub-security-ground-up.md)
### [En iyi güvenlik uygulamaları](iot-hub-security-best-practices.md)
### [Güvenlik mimarisi](iot-hub-security-architecture.md)
### [IoT dağıtımınızın güvenliğini sağlama](iot-hub-security-deployment.md)
## Azure IoT Edge
### [Genel Bakış](iot-hub-iot-edge-overview.md)
### başlarken
#### [Linux](iot-hub-linux-iot-edge-get-started.md)
#### [Windows](iot-hub-windows-iot-edge-get-started.md)
### Cihazı benzetme
#### [Linux](iot-hub-linux-iot-edge-simulated-device.md)
#### [Windows](iot-hub-windows-iot-edge-simulated-device.md)
### [Gerçek cihaz kullanma](iot-hub-iot-edge-physical-device.md)
### Bir modül oluşturma
#### [Java](iot-hub-iot-edge-create-module-java.md)
#### [.NET Framework](https://github.com/Azure-Samples/iot-edge-samples#how-to-run-the-net-module-sample-windows-10)
#### [.NET Standard](iot-hub-iot-edge-create-module-dotnet-core.md)
#### [Node.js](iot-hub-iot-edge-create-module-js.md)
### Oluşturma
#### [.NET Framework](https://github.com/Azure/iot-edge/tree/master/samples/dotnet_binding_sample)
#### [.NET Core modülü](https://github.com/Azure/iot-edge/tree/master/samples/dotnet_core_module_sample)
#### [.NET Core yönetilen ağ geçidi](https://github.com/Azure/iot-edge/tree/master/samples/dotnet_core_managed_gateway)
#### [Java](https://github.com/Azure/iot-edge/tree/master/samples/java_sample)
#### [Node.js](https://github.com/Azure/iot-edge/tree/master/samples/nodejs_simple_sample)
#### [Modülü dinamik olarak ekleme](https://github.com/Azure/iot-edge/tree/master/samples/dynamically_add_module_sample)
#### [İşlem dışı ara sunucu modülü](https://github.com/Azure/iot-edge/tree/master/samples/proxy_sample)
#### [Yerel modül konağı](https://github.com/Azure/iot-edge/tree/master/samples/native_module_host_sample)

# Başvuru
## [Kod örnekleri](https://azure.microsoft.com/en-us/resources/samples/?service=iot-hub)
## [Azure CLI](/cli/azure/iot)
## [.NET (Hizmet)](/dotnet/api/microsoft.azure.devices)
## [.NET (Cihazlar)](/dotnet/api/microsoft.azure.devices.client)
## [Java (Hizmet)](/java/api/com.microsoft.azure.sdk.iot.service)
## [Java (Cihazlar)](/java/api/com.microsoft.azure.sdk.iot.device)
## [Node.js SDK’ları](http://azure.github.io/azure-iot-sdk-node/)
## [C cihaz API’si](https://azure.github.io/azure-iot-sdk-c/index.html)
## [Azure IoT Edge](http://azure.github.io/iot-edge/)
## [REST (Kaynak Sağlayıcısı)](https://docs.microsoft.com/rest/api/iothub/iothubresource)
## [REST (Cihaz Kimlikleri)](https://docs.microsoft.com/rest/api/iothub/deviceapi)
## [REST (Cihaz İkizleri)](https://docs.microsoft.com/rest/api/iothub/devicetwinapi)
## [REST (Cihaz Mesajlaşması )](https://docs.microsoft.com/rest/api/iothub/httpruntime)
## [REST (İşler)](https://docs.microsoft.com/rest/api/iothub/jobapi)

# İlgili
## [Azure IoT Paketi](https://azure.microsoft.com/documentation/suites/iot-suite/)
## [Azure Event Hubs](https://azure.microsoft.com/documentation/services/event-hubs/)
## [Akış Analizi](https://azure.microsoft.com/documentation/services/stream-analytics/)
## [Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/)

# Kaynaklar
## [IoT için Azure Sertifikalı cihaz kataloğu](https://catalog.azureiotsuite.com/)
## [Azure IoT Geliştirici Merkezi](https://azure.microsoft.com/develop/iot/)
## [Azure Yol Haritası](https://azure.microsoft.com/roadmap/?category=internet-of-things)
## [DeviceExplorer aracı](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer)
## [iothub-diagnostics tool](https://github.com/Azure/iothub-diagnostics)
## [iothub-explorer tool](https://github.com/Azure/iothub-explorer)
## [Öğrenme yolu](https://azure.microsoft.com/documentation/learning-paths/iot-hub/)
## [MSDN forumu](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azureiothub)
## [Fiyatlandırma](https://azure.microsoft.com/pricing/details/iot-hub/)
## [Fiyatlandırma hesaplayıcı](https://azure.microsoft.com/pricing/calculator/)
## [Hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/?product=iot-hub)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-iot-hub)
## [Teknik örnek olay incelemeleri](https://microsoft.github.io/techcasestudies/#technology=IoT&sortBy=featured)
## [Videolar](https://azure.microsoft.com/documentation/videos/index/?services=iot-hub)
