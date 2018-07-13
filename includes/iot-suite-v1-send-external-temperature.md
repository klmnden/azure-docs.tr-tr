## <a name="configure-the-nodejs-simulated-device"></a>Node.js sanal cihaz yapılandırma
1. Uzaktan izleme panosunda **+ cihaz Ekle** ve ardından bir *özel cihaz*. IOT Hub ana bilgisayar adı, cihaz kimliği ve cihaz anahtarını not edin. Remote_monitoring.js cihazı istemci uygulaması hazırlanırken Bu öğreticinin ilerleyen bölümlerinde gerekir.
2. Bu Node.js sürümü olun 0.12.x sürümü veya üzeri geliştirme makinenizde yüklü. Çalıştırma `node --version` bir komut isteminde veya bir kabuk sürümü denetlemek için. Linux üzerinde Node.js yüklemek için bir paket Yöneticisi'ni kullanma hakkında daha fazla bilgi için bkz. [Paket Yöneticisi ile Node.js yükleme][node-linux].
3. Node.js yüklendiğinde, en son sürümünü kopyalama [azure IOT SDK'sı düğüm] [ lnk-github-repo] deposunu geliştirme makinenize. Her zaman **ana** dal en son sürümü kitaplıklar ve örnekler.
4. Yerel kopyasından [azure IOT SDK'sı düğüm] [ lnk-github-repo] depo, aşağıdaki iki dosyaların kopyalanacağı cihaz/düğüm/örnekler klasörüne boş bir klasör için geliştirme makinenizde:
   
   * Packages.JSON
   * remote_monitoring.js
5. Remote_monitoring.js dosyasını açın ve aşağıdaki değişken tanımını arayın:
   
    ```
    var connectionString = "[IoT Hub device connection string]";
    ```
6. Değiştirin **[IOT Hub cihaz bağlantı dizesini]** cihaz bağlantısı dizeniz ile. IOT Hub konak adına, cihaz kimliği ve Not 1. adımda oluşturduğunuz cihaz anahtarı için değerleri kullanın. Bir cihaz bağlantı dizesi aşağıdaki biçimdedir:
   
    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```
   
    IOT Hub ana bilgisayar **contoso** ve cihaz kimliğiniz **cihazım**, bağlantı dizeniz aşağıdaki kod parçacığı gibi görünür:
   
    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```
7. Dosyayı kaydedin. Kabuğu veya gerekli paketleri yükleyin ve ardından örnek uygulamayı çalıştırmak için bu dosyaları içeren klasöre komut isteminde aşağıdaki komutları çalıştırın:
   
    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a>Dinamik eylem telemetriyi inceleyin
Pano, mevcut sanal cihazlar sıcaklık ve nem telemetrisini gösterir:

![Varsayılan Pano][image1]

Önceki bölümde çalıştırdığınız Node.js sanal cihaz seçerseniz, dış ortam sıcaklığı telemetri sıcaklık ve nem bakın:

![Dış ortam sıcaklığı için Pano ekleyin][image2]

Uzaktan izleme çözümü, otomatik olarak ek dış ortam sıcaklığı telemetri türü algılar ve Pano grafiği ekler.

[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdk-node
[image1]: media/iot-suite-v1-send-external-temperature/image1.png
[image2]: media/iot-suite-v1-send-external-temperature/image2.png