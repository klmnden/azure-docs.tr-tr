---
title: Azure IOT hub'ı (iOS) sahip bulut-cihaz iletilerini | Microsoft Docs
description: İOS için Azure IOT SDK'ları kullanılarak Azure IOT hub'dan bir aygıta bulut-cihaz iletilerini göndermek nasıl.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/19/2018
ms.author: kgremban
ms.openlocfilehash: 62647620f6bbeadecfa778f91855ef1eee5240dd
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34634290"
---
# <a name="send-cloud-to-device-messages-with-iot-hub-ios"></a>IOT hub'ı (iOS) sahip bulut-cihaz iletilerini gönder
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]


Azure IOT Hub, milyonlarca cihaza arasında güvenilir ve güvenli çift yönlü iletişimler sağlayan tam olarak yönetilen hizmet etkinleştirin ve bir çözüm arka ucu ' dir. [Telemetri bir CİHAZDAN bir IOT hub'ına Gönder] makale nasıl IOT hub oluşturma, bir cihaz kimliği, sağlama ve cihaz-bulut iletileri gönderen bir sanal cihaz uygulamasının kodu gösterir.

Bu makale size nasıl gösterir için:

* Çözüm arka ucunuz, tek bir cihaz IOT hub'ı aracılığıyla bulut cihaz ileti gönderebilir.
* Bir cihazda bulut-cihaz iletilerini alır.
* Çözüm arka ucunuz, teslimat alındısı iste (*geri bildirim*) için bir cihaz IOT Hub'ından gönderilen iletiler için.

Bulut cihaz iletileri hakkında daha fazla bilgi bulabilirsiniz [IOT Hub Geliştirici Kılavuzu][IoT Hub developer guide - C2D].

Bu makalenin sonunda iOS projeleri iki Swift çalıştırın:

* **Örnek aygıt**, oluşturulan aynı uygulama [Telemetri bir CİHAZDAN bir IOT hub'ına Gönder], IOT hub'ınıza bağlanır ve bulut-cihaz iletilerini alır.
* **Örnek hizmet**, IOT hub'ı aracılığıyla sanal cihaz uygulamasının bir bulut cihaz ileti gönderir ve teslimat alındısı alır.

> [!NOTE]
> IOT Hub SDK desteği birçok cihaz platformları ve Azure IOT cihaz SDK'ları aracılığıyla dilleri (C, Java ve Javascript dahil) sahiptir. Bu öğreticinin kod ve genellikle Azure IOT Hub Cihazınızı bağlamak hakkında adım adım yönergeler için bkz: [Azure IoT Geliştirici Merkezi].

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

- Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)
- Azure active IOT hub. 
- Kod örnekten [Azure örneklerinden](https://github.com/Azure-Samples/azure-iot-samples-ios/archive/master.zip) .
- En son iOS SDK sürümünü çalıştıran en yeni [XCode](https://developer.apple.com/xcode/) sürümü. Bu hızlı başlangıç XCode 9.3 ve iOS 11.3 ile test edilmiştir.
- [CocoaPods](https://guides.cocoapods.org/using/getting-started.html)'un en son sürümü.


## <a name="simulate-an-iot-device"></a>IOT cihaz benzetimi
Bu bölümde IOT hub'ından bulut cihaz iletileri almak için hızlı bir uygulama çalıştıran bir iOS cihazının benzetimini. 

Bu makalede, oluşturduğunuz örnek örnek cihazdır [Telemetri bir CİHAZDAN bir IOT hub'ına Gönder]. Çalıştıran zaten varsa, bu bölümü atlayabilirsiniz.

### <a name="install-cocoapods"></a>CocoaPods yükleme

CocoaPods, üçüncü taraf kitaplıklar kullanan iOS projeleri için bağımlılıkları yönetir.

Bir terminal penceresinde, önkoşulların bir parçası olarak indirdiğiniz Azure-IoT-Samples-iOS klasörüne gidin. Ardından örnek projeye gidin:

```sh
cd quickstart/sample-device
```

XCode’un kapalı olduğundan emin olun ve **podfile** dosyasında bildirilen CocoaPods’u yüklemek üzere aşağıdaki komutu çalıştırın:

```sh
pod install
```

Yükleme komutu, projeniz için gereken podları yüklemeye ek olarak bağımlılıklar için podları kullanacak şekilde önceden yapılandırılmış bir XCode çalışma alanı dosyası da oluşturmuştur. 

### <a name="run-the-sample-device-application"></a>Örnek cihaz uygulamayı çalıştırın 

1. Cihazınız için bağlantı dizesi alma. Cihaz ayrıntıları dikey penceresinde Azure portalından Bu dize kopyalama veya aşağıdaki CLI komutuyla alın: 

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id {YourDeviceID} --output table
    ```

1. Örnek çalışma alanını XCode'da açın.

   ```sh
   open "MQTT Client Sample.xcworkspace"
   ```

2. Genişletme **MQTT istemci örnek** proje ve klasörü aynı ada sahip.  
3. XCode’da düzenlemek üzere **ViewController.swift** dosyasını açın. 
4. Arama **connectionString** değişkeni ve değeri cihaz bağlantısı ile güncelleştirin, ilk adımda kopyaladığınız dize.
5. Yaptığınız değişiklikleri kaydedin. 
6. **Derle ve çalıştır** düğmesi ya da **command + r** tuş birleşimi ile projeyi cihaz öykünücüsünde çalıştırın. 

   ![Projeyi çalıştırma](media/quickstart-send-telemetry-ios/run-sample.png)


## <a name="simulate-a-service-device"></a>Bir hizmet cihazının benzetimini

Bu bölümde IOT hub'ı aracılığıyla bulut-cihaz iletilerini gönderir hızlı bir uygulama olan ikinci bir iOS cihaz benzetimi. Bu yapılandırma için IOT senaryolarını yararlıdır iOS cihazları bir iPhone veya iPad için diğer bir denetleyicisi olarak işlev olduğu bir IOT hub'ına bağlı. 

### <a name="install-cocoapods"></a>CocoaPods yükleme

CocoaPods, üçüncü taraf kitaplıklar kullanan iOS projeleri için bağımlılıkları yönetir.

Önkoşullar indirdiğiniz Azure IOT iOS örnekleri klasöre gidin. Ardından, örnek hizmet projesine gidin:

```sh
cd quickstart/sample-service
```

XCode’un kapalı olduğundan emin olun ve **podfile** dosyasında bildirilen CocoaPods’u yüklemek üzere aşağıdaki komutu çalıştırın:

```sh
pod install
```

Yükleme komutu, projeniz için gereken podları yüklemeye ek olarak bağımlılıklar için podları kullanacak şekilde önceden yapılandırılmış bir XCode çalışma alanı dosyası da oluşturmuştur.

### <a name="run-the-sample-service-application"></a>Örnek hizmet uygulamayı çalıştırın

1. IOT hub'ınız için hizmeti bağlantı dizesini alır. Bu dize Azure portalından kopyalayabilirsiniz **iothubowner** ilkesinde **paylaşılan erişim ilkeleri** dikey penceresinde veya aşağıdaki CLI komutuyla alınamıyor:  

    ```azurecli-interactive
    az iot hub show-connection-string --hub-name {YourIoTHubName} --output table
    ```

2. Örnek çalışma alanını XCode'da açın.

   ```sh
   open AzureIoTServiceSample.xcworkspace
   ```

3. Genişletme **AzureIoTServiceSample** proje ve sonra aynı ada sahip klasörünü genişletin.  
4. XCode’da düzenlemek üzere **ViewController.swift** dosyasını açın. 
5. Arama **connectionString** değişkeni ve değeri, daha önce kopyaladığınız hizmeti bağlantı dizesini ile güncelleştirin.
6. Yaptığınız değişiklikleri kaydedin. 
7. Xcode'da, farklı iOS cihazı IOT cihaz çalıştırmak için kullanılan daha öykünücüsü ayarlarını değiştirin. XCode aynı türde birden fazla Öykünücüler çalıştırılamıyor. 

   ![Öykünücü aygıt değiştirme](media/iot-hub-ios-swift-c2d/change-device.png)

8. Proje aygıt öykünücü ile çalıştırmanız **derleme ve çalıştırma** düğmesini veya tuş birleşimine **komutu + r**. 

   ![Projeyi çalıştırma](media/iot-hub-ios-swift-c2d/run-app.png)


## <a name="send-a-cloud-to-device-message"></a>Bulut cihaza ileti gönderme
Şimdi bulut cihaza ileti gönderme ve alma için iki uygulamaları kullanmak hazırsınız.

1. İçinde **iOS uygulaması örneği** benzetimli IOT cihazda çalışan uygulama tıklatın **Başlat**. Uygulama, cihaz-bulut iletileri göndermeye başlar, ancak aynı zamanda bulut-cihaz iletilerini dinlemeyi başlatır. 

   ![Örnek IOT cihaz uygulamayı görüntüle](media/iot-hub-ios-swift-c2d/view-d2c.png)

2. İçinde **Iothub hizmeti istemci örnek** benzetimli hizmet cihazda çalışan uygulama istediğiniz IOT cihaz kimliği girin bir ileti göndermek için. 
3. Düz metin bir ileti yazın ve ardından **Gönder**. 

Gönder'i hemen çeşitli eylemler gerçekleşir. Hizmet örneği, IOT hub'ı, ama uygulamanın erişim sağlanan hizmet bağlantı nedeniyle, dize için ileti gönderir. IOT hub'ınızı cihaz kimliği denetler, hedef aygıta ileti gönderir ve onay giriş kaynağı cihaza gönderir. Sanal IOT cihazınız üzerinde çalışan uygulama IOT hub'ı iletilerden denetler ve en son bir ekran üzerindeki metnin yazdırır.

Çıkışı aşağıdaki gibi görünmelidir:

   ![Bulut-cihaz iletilerini görüntüleme](media/iot-hub-ios-swift-c2d/view-c2d.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu öğretici kapsamında, bulut cihaza ileti gönderme ve alma öğrendiniz. 

IOT hub'ı kullanan tam uçtan uca çözümler örnekleri görmek için bkz: [Azure IOT Uzaktan izleme Çözüm Hızlandırıcısı].

IOT Hub ile çözümleri geliştirme hakkında daha fazla bilgi için bkz: [IOT Hub Geliştirici Kılavuzu].

<!-- Images -->
[img-simulated-device]: media/iot-hub-python-python-c2d/simulated-device.png
[img-send-command]:  media/iot-hub-python-python-c2d/send-command.png
[img-message-recieved]: media/iot-hub-python-python-c2d/message-recieved.png

<!-- Links -->
[Telemetri bir CİHAZDAN bir IOT hub'ına Gönder]: quickstart-send-telemetry-ios.md

[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
[IOT Hub Geliştirici Kılavuzu]: iot-hub-devguide.md
[Azure IoT Geliştirici Merkezi]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure portal]: https://portal.azure.com
[Azure IOT Uzaktan izleme Çözüm Hızlandırıcısı]: https://azure.microsoft.com/documentation/suites/iot-suite/
