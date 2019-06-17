---
title: Cihaz verileri saydam ağ geçidi - Azure IOT Edge üzerinde Machine Learning aracılığıyla gönderme | Microsoft Docs
description: Geliştirme makinenizde, cihaz IOT Hub'ına saydam bir ağ geçidi olarak yapılandırılmış bir cihaz üzerinden giderek veri göndermek için sanal IOT Edge cihazı kullanın.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/13/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 12793ff28bf13f26bc2cc3d436b644601fc48ac8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67081388"
---
# <a name="tutorial-send-data-via-transparent-gateway"></a>Öğretici: Saydam bir ağ geçidi üzerinden veri gönderme

> [!NOTE]
> Bu makale bir serinin IOT Edge üzerinde Azure Machine Learning'i kullanma hakkında bir öğretici için parçasıdır. Bu makalede doğrudan gelmiş, başlangıç öneriyoruz [makaleyi](tutorial-machine-learning-edge-01-intro.md) serisindeki en iyi sonuçlar için.

Bu makalede, bir kez daha geliştirme makinenize bir sanal cihaz kullanıyoruz, ancak doğrudan IOT Hub'ına veri gönderen yerine cihaz verileri saydam bir ağ geçidi olarak yapılandırılmış IOT Edge cihazı gönderir.

Sanal cihazı veri gönderiyor olsa IOT Edge cihazı işlemi inşa ediyoruz. Cihaz tamamlandıktan sonra çalışan, verileri her şeyin beklendiği gibi çalıştığını doğrulamak için depolama hesabımız bakacağız.

Bu adım genellikle bir bulut ya da cihaz geliştirici tarafından gerçekleştirilir.

## <a name="review-device-harness"></a>Cihaz bandı gözden geçirin

Yeniden [DeviceHarness proje](tutorial-machine-learning-edge-03-generate-data.md) cihaz benzetimi aşağı akış (veya yaprak için). Saydam ağ geçidine bağlanma iki ek öğeleri gerektirir:

* Aşağı Akış cihazı hale getirmek için sertifikayı kaydetmek (Bu durumda geliştirme makinemizi) IOT Edge çalışma zamanı tarafından kullanılan sertifika yetkilisine güvenmesi.
* Edge ağ geçidi tam etki alanı adı (FQDN), cihaz bağlantı dizesi ekleyin.

Bu iki öğe nasıl uygulandığını görmek için koda bakın.

1. Geliştirme makinenizde Visual Studio Code'u açın.

2. Kullanım **dosya** > **Klasör Aç...**  C: açmak için\\kaynak\\IoTEdgeAndMlSample\\DeviceHarness.

3. Program.CS'de Webhostbuilder'a InstallCertificate() yöntemi bakın.

4. Kod sertifika yolu bulursa, bu makinede sertifikayı yüklemek için CertificateManager.InstallCACert yöntemi çağıran unutmayın.

5. Artık GetIotHubDevice yöntem TurbofanDevice sınıf üzerinde arayın.

6. Kullanıcının belirttiği FQDN kullanarak ağ geçidi, "-g" seçeneği, değer olarak cihaz bağlantı dizesi eklenmiş gatewayFqdn, bu yönteme geçirilen.

   ```csharp
   connectionString = $"{connectionString};GatewayHostName={gatewayFqdn.ToLower()}";
   ```

## <a name="build-and-run-leaf-device"></a>Yaprak cihaz derlemek ve çalıştırmak

1. DeviceHarness proje Visual Studio Code'da hala açıkken, projeyi derleyin (Ctrl + Shift + B ya da **Terminal** > **derleme görevi çalıştır...** ) seçip **derleme** iletişim.

2. IOT Edge cihaz sanal makinenize portalında giderek ve değeri kopyalama, sınır ağ geçidi tam etki alanı adını (FQDN) Bul **DNS adı** gelen genel bakış.

3. Visual Studio Code Terminali açın (**Terminal** > **yeni terminal**) ve aşağıdaki komut, değiştirme `<edge_device_fqdn>` sanal makineden kopyaladığınız DNS adına sahip:

   ```cmd
   dotnet run -- --gateway-host-name "<edge_device_fqdn>" --certificate C:\edgecertificates\certs\azure-iot-test-only.root.ca.cert.pem --max-devices 1
   ```

4. Uygulama geliştirme makinenize sertifikayı yüklemek çalışır. Yaptığında, güvenlik uyarısını kabul edin.

5. IOT hub'ı bağlantı dizesi tıklayarak için üç nokta istendiğinde ( **...** ) Azure IOT Hub cihazları paneli ve seçin **kopyalama IOT Hub bağlantı dizesine**. Değeri, terminale yapıştırabilirsiniz.

6. Gibi bir çıktı görürsünüz:

   ```output
   Found existing device: Client_001
   Using device connection string: HostName=<your hub>.azure-devices.net;DeviceId=Client_001;SharedAccessKey=xxxxxxx; GatewayHostName=iotedge-xxxxxx.<region>.cloudapp.azure.com
   Device: 1 Message count: 50
   Device: 1 Message count: 100
   Device: 1 Message count: 150
   Device: 1 Message count: 200
   Device: 1 Message count: 250
   ```

   IOT Edge saydam ağ geçidi aracılığıyla IOT hub'ı aracılığıyla iletişim kurmak cihazın neden olan cihaz bağlantı dizesi "GatewayHostName" eklenmesi unutmayın.

## <a name="check-output"></a>Onay çıkış

### <a name="iot-edge-device-output"></a>IOT Edge cihaz çıktısı

AvroFileWriter modülü çıktısı IOT Edge cihazı bakarak kolayca gösterilebilir.

1. IOT Edge sanal makinenizi içine SSH.

2. Yazılan dosyaları aramak için disk.

   ```bash
   find /data/avrofiles -type f
   ```

3. Komut çıktısı, aşağıdaki örnekteki gibi görünür:

   ```output
   /data/avrofiles/2019/4/18/22/10.avro
   ```

   En fazla çalıştırma zamanlaması bağlı olarak tek bir dosya olabilir.

4. Zaman damgaları dikkat edin. Son değiştirme zamanı geçmişte 10 dakikadan fazla olduğunda avroFileWriter Modülü dosyaları buluta yükler. (DEĞİŞTİRİLEN bkz\_dosya\_avroFileWriter modülünde uploader.py zaman AŞIMI).

5. Modül, 10 dakika geçtikten sonra dosyaları yüklemeniz gerekir. Karşıya yükleme başarılı olursa, dosyayı diskten siler.

### <a name="azure-storage"></a>Azure Storage

Biz yönlendirilecek veriler nerede bekliyoruz depolama hesaplarını bakarak veri gönderen bizim yaprak cihaz sonuçlarını görebilirsiniz.

1. Geliştirme makinenizde Visual Studio Code'u açın.

2. Depolama hesabınız için ağaç İncele penceresinde "AZURE depolama" panelinde gidin.

3. Genişletin **Blob kapsayıcıları** düğümü.

4. Öğreticinin önceki bölümünde yaptığımız iş, bekliyoruz **ruldata** kapsayıcı RUL iletilerle içermelidir. Genişletin **ruldata** düğümü.

5. Gibi adlı bir veya daha fazla blob dosyalarını görürsünüz: `<IoT Hub Name>/<partition>/<year>/<month>/<day>/<hour>/<minute>`.

6. Sağ dosyalarından birine tıklayın ve seçin **Blob indirme** dosya geliştirme makinenize kaydedin.

7. Sonraki genişletin **uploadturbofanfiles** düğümü. Önceki makalede bu konum hedef olarak ayarladık avroFileWriter modülü tarafından karşıya yüklenen dosyalar için.

8. Dosya sağ tıklayın ve seçin **Blob indirme** geliştirme makinenize kaydedin.

### <a name="read-avro-file-contents"></a>Okuma Avro dosya içeriği

Avro dosya okuma ve dosyasında iletileri bir JSON dizisi döndürme için basit bir komut satırı yardımcı ekledik. Bu bölümde, biz yükleyebilir ve çalıştırabilirsiniz.

1. Visual Studio Code'da bir terminal açın (**Terminal** > **yeni terminal**).

2. Hubavroreader yükleyin:

   ```cmd
   pip install c:\source\IoTEdgeAndMlSample\HubAvroReader
   ```

3. Kaynağından indirdiğiniz Avro dosyayı okumak için hubavroreader kullanmak **ruldata**.

   ```cmd
   hubavroreader <avro file with ath> | more
   ```

4. Cihaz kimliği ile beklenen ve RUL tahmin gibi iletisinin gövdesini göründüğünü unutmayın.

   ```json
   {
       "Body": {
           "ConnectionDeviceId": "Client_001",
           "CorrelationId": "3d0bc256-b996-455c-8930-99d89d351987",
           "CycleTime": 1.0,
           "PredictedRul": 170.1723693909444
       },
       "EnqueuedTimeUtc": "<time>",
       "Properties": {
           "ConnectionDeviceId": "Client_001",
           "CorrelationId": "3d0bc256-b996-455c-8930-99d89d351987",
           "CreationTimeUtc": "01/01/0001 00:00:00",
           "EnqueuedTimeUtc": "01/01/0001 00:00:00"
       },
       "SystemProperties": {
           "connectionAuthMethod": "{\"scope\":\"module\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
           "connectionDeviceGenerationId": "636857841798304970",
           "connectionDeviceId": "aaTurbofanEdgeDevice",
           "connectionModuleId": "turbofanRouter",
           "contentEncoding": "utf-8",
           "contentType": "application/json",
           "correlationId": "3d0bc256-b996-455c-8930-99d89d351987",
           "enqueuedTime": "<time>",
           "iotHubName": "mledgeiotwalkthroughhub"
       }
   }
   ```

5. Kaynağından indirdiğiniz Avro dosya geçirme aynı komutu çalıştırmak **uploadturbofanfiles**.

6. Bu iletiler, beklendiği gibi tüm sensör verilerini ve orijinal mesajın işletimsel ayarlarını içerir. Bu veriler, müşterilerimizin edge cihazında RUL modeli geliştirmek için kullanılabilir.

   ```json
   {
       "Body": {
           "CycleTime": 1.0,
           "OperationalSetting1": -0.0005000000237487257,
           "OperationalSetting2": 0.00039999998989515007,
           "OperationalSetting3": 100.0,
           "PredictedRul": 170.17236328125,
           "Sensor1": 518.6699829101562,
           "Sensor10": 1.2999999523162842,
           "Sensor11": 47.29999923706055,
           "Sensor12": 522.3099975585938,
           "Sensor13": 2388.010009765625,
           "Sensor14": 8145.31982421875,
           "Sensor15": 8.424599647521973,
           "Sensor16": 0.029999999329447746,
           "Sensor17": 391.0,
           "Sensor18": 2388.0,
           "Sensor19": 100.0,
           "Sensor2": 642.3599853515625,
           "Sensor20": 39.11000061035156,
           "Sensor21": 23.353700637817383,
           "Sensor3": 1583.22998046875,
           "Sensor4": 1396.8399658203125,
           "Sensor5": 14.619999885559082,
           "Sensor6": 21.610000610351562,
           "Sensor7": 553.969970703125,
           "Sensor8": 2387.9599609375,
           "Sensor9": 9062.169921875
       },
           "ConnectionDeviceId": "Client_001",
           "CorrelationId": "70df0c98-0958-4c8f-a422-77c2a599594f",
           "CreationTimeUtc": "0001-01-01T00:00:00+00:00",
           "EnqueuedTimeUtc": “<time>”
   }
   ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uçtan uca öğretici tarafından kullanılan kaynakları keşfetmeye devam etmeyi planlıyorsanız, oluşturduğunuz kaynakları temizlemek için bunlar tamamlanana kadar bekleyin. Devam etmeyi planlamıyorsanız, bunları silmek için aşağıdaki adımları kullanın:

1. Geliştirme VM, IOT Edge VM, IOT Hub, depolama hesabı, machine learning çalışma alanı hizmeti tutmak üzere oluşturulmuş kaynak gruplarını silin (ve kaynaklar: kapsayıcı kayıt defteri, application ınsights, anahtar kasası, depolama hesabı).

2. Machine learning projesi içinde Sil [Azure not defterleri](https://notebooks.azure.com).

3. Deponun yerel olarak, yakın yerel deponuza başvuran herhangi bir PowerShell veya VS Code penceresinin bir kopyasını, depo dizini silin.

4. Sertifikaları yerel olarak oluşturduğunuz klasörü c: silmeniz\\edgeCertificates.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir yaprak cihaz gönderen sensör ve işletimsel verileri için sunduğumuz edge cihazının simülasyonunu gerçekleştirme için sunduğumuz geliştirme makinesi kullanılır. Biz, cihazdaki modüller yönlendirilen, sınıflandırması, kalıcı ve verileri gerçek zamanlı işlem edge cihazı incelemek ve ardından dosyaları depolama hesabına yüklediniz bakarak önce karşıya olduğunu doğrulandı.

Aşağıdaki sayfalarda daha fazla bilgi bulunabilir:

* [Bir Azure IOT Edge ağ geçidi için bir aşağı akış cihazı bağlayın](how-to-connect-downstream-device.md)
* [IOT Edge (Önizleme) Azure Blob Depolama ile uçta veri Store](how-to-store-data-blob.md)
