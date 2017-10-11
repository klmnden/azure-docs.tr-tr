## <a name="configure-the-nodejs-simulated-device"></a>Node.js sanal cihaz yapılandırma
1. Uzaktan izleme Panosu üzerinde tıklatın **+ bir cihaz ekleme** ve ardından ekleyin bir *özel cihaz*. IOT Hub ana bilgisayar adı, cihaz kimliği ve cihaz anahtarını Not. Remote_monitoring.js aygıt istemci uygulaması hazırlarken, bunları daha sonra Bu öğreticide gerekir.
2. Bu Node.js sürümünü olun 0.12.x sürümü veya daha sonra dağıtım makinenize yüklü. Çalıştırma `node --version` bir komut isteminde veya bir kabuk sürümünü denetleyin. Node.js Linux'ta yüklemek için bir paket Yöneticisi'ni kullanma hakkında daha fazla bilgi için bkz: [yükleme Node.js Paket Yöneticisi aracılığıyla][node-linux].
3. Node.js yüklendiğinde en son sürümünü kopyalama [azure IOT sdk düğüm] [ lnk-github-repo] geliştirme makinenizde depoya. Her zaman kullanmak **ana** en son sürümünü kitaplıklar ve örnekler için dal.
4. Yerel kopyasından [azure IOT sdk düğüm] [ lnk-github-repo] deposu, aşağıdaki iki dosyaların kopyalanacağı düğümü/aygıt/samples klasör boş bir klasörün geliştirme makinenizde:
   
   * Packages.JSON
   * remote_monitoring.js
5. Remote_monitoring.js dosyasını açın ve aşağıdaki değişken tanımı bakın:
   
    ```
    var connectionString = "[IoT Hub device connection string]";
    ```
6. Değiştir **[IOT Hub cihaz bağlantı dizesi]** cihaz bağlantı dizesine sahip. IOT Hub ana bilgisayar adı, cihaz kimliği ve 1. adımda not yapılan aygıt anahtarı için değerleri kullanın. Bir aygıt bağlantı dizesi, aşağıdaki biçime sahiptir:
   
    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```
   
    IOT Hub ana bilgisayar adına ise **contoso** ve cihaz kimliğinizi **mydevice**, bağlantı dizenizi aşağıdaki kod parçacığını gibi görünür:
   
    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```
7. Dosyayı kaydedin. Bir kabuk ya da gerekli paketleri yüklemek ve örnek uygulamayı çalıştırmak için bu dosyaları içeren klasör komut isteminde aşağıdaki komutları çalıştırın:
   
    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a>Eylem dinamik telemetri inceleyin
Pano varolan sanal aygıtlardan sıcaklık ve nem telemetrisi gösterir:

![Varsayılan Pano][image1]

Önceki bölümde çalıştırdığınız Node.js sanal cihaz seçerseniz, sıcaklık ve nem dış sıcaklığı telemetri bakın:

![Dış sıcaklığı için Pano ekleyin][image2]

Uzaktan izleme çözümü otomatik olarak ek dış sıcaklığı telemetri türünü algılar ve Pano grafiği ekler.

[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdk-node
[image1]: media/iot-suite-send-external-temperature/image1.png
[image2]: media/iot-suite-send-external-temperature/image2.png