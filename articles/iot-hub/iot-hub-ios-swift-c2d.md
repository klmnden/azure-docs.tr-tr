---
title: Azure IOT hub'ı (iOS) ile bulut-cihaz iletilerini | Microsoft Docs
description: İOS için Azure IOT SDK'larını kullanarak bir Azure IOT hub'ından bir cihaza bulut-cihaz iletilerini göndermek nasıl.
author: kgremban
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/19/2018
ms.author: kgremban
ms.openlocfilehash: 6bb95bf887837fffc4196bca8d761239ac430a1a
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67620187"
---
# <a name="send-cloud-to-device-messages-with-iot-hub-ios"></a>IOT hub'ı (iOS) ile bulut buluttan cihaza iletileri gönderme

[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

Azure IOT hub'ı yardımcı olan tam olarak yönetilen bir hizmet, milyonlarca cihaz arasında güvenilir ve güvenli çift yönlü iletişimi etkinleştirmek ve bir çözüm arka ucu ' dir. [Telemetri gönderir bir CİHAZDAN bir IOT hub'ına](quickstart-send-telemetry-ios.md) hızlı başlangıç, IOT hub oluşturma, bir cihaz kimliği da sağlamak ve CİHAZDAN buluta iletiler gönderen bir sanal cihaz uygulamasının kodu nasıl gösterir.

Bu öğretici şunların nasıl yapıldığını gösterir:

* Çözüm arka ucunuz, tek bir cihaz IOT hub'ı aracılığıyla bulut-cihaz iletilerini gönderin.

* Bir cihazda bulut-cihaz iletilerini alır.

* Teslim alındı bildirimi, çözüm arka ucu istek (*geri bildirim*) için bir cihaz IOT Hub'ından gönderilen iletileri.

Bulut-cihaz iletileri hakkında daha fazla bilgi bulabilirsiniz [bölümde IOT Hub Geliştirici kılavuzunun Mesajlaşma](iot-hub-devguide-messaging.md).

Bu makalenin sonunda, iki Swift iOS projeleri çalıştırın:

* **Örnek cihaz**, oluşturulan aynı uygulama [telemetri gönderir bir CİHAZDAN bir IOT hub'ına](quickstart-send-telemetry-ios.md), IOT hub'ınıza bağlanır ve bulut-cihaz iletilerini alır.

* **Örnek hizmeti**, IOT hub'ı aracılığıyla sanal cihaz uygulaması için bir bulut buluttan cihaza ileti gönderir ve sonra onun teslim alındı bildirimi alır.

> [!NOTE]
> IOT Hub SDK desteği birçok cihaz platformlarını ve Azure IOT cihaz SDK'ları aracılığıyla diller (C, Java ve Javascript dahil) sahiptir. Bu öğreticinin koda ve genellikle Azure IOT hub'a Cihazınızı bağlamak hakkında adım adım yönergeler için bkz. [Azure IOT Geliştirici Merkezi](https://www.azure.com/develop/iot).

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure hesabı. (Hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.)

* Azure'da etkin bir IOT hub.

* Kod örneğini [Azure örnekleri](https://github.com/Azure-Samples/azure-iot-samples-ios/archive/master.zip).

* En son iOS SDK sürümünü çalıştıran en yeni [XCode](https://developer.apple.com/xcode/) sürümü. Bu hızlı başlangıç XCode 9.3 ve iOS 11.3 ile test edilmiştir.

* [CocoaPods](https://guides.cocoapods.org/using/getting-started.html)'un en son sürümü.

## <a name="simulate-an-iot-device"></a>Bir IOT cihazının simülasyonunu gerçekleştirme

Bu bölümde, IOT hub'ından bulut-cihaz iletilerini almak için bir Swift uygulaması çalıştıran bir iOS cihazının benzetimini yapın. 

Bu makalede oluşturduğunuz örnek cihazdır [telemetri gönderir bir CİHAZDAN bir IOT hub'ına](quickstart-send-telemetry-ios.md). Çalıştıran zaten varsa, bu bölümü atlayabilirsiniz.

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

### <a name="run-the-sample-device-application"></a>Örnek cihaz uygulamasını çalıştırın

1. Cihazınız için bağlantı dizesini alın. Bu dizeden kopyalayabilirsiniz [Azure portalında](https://portal.azure.com) cihaz ayrıntıları dikey penceresinde aşağıdaki CLI komutunu ile alabilir:

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id {YourDeviceID} --output table
    ```

1. Örnek çalışma alanını XCode'da açın.

   ```sh
   open "MQTT Client Sample.xcworkspace"
   ```

2. Genişletin **MQTT istemci örneği** proje ve aynı ada sahip klasör.  

3. XCode’da düzenlemek üzere **ViewController.swift** dosyasını açın. 

4. Arama **connectionString** değişkeni güncelleştirilemiyor değeri cihaz bağlantı dizesi, ilk adımda kopyaladığınız.

5. Yaptığınız değişiklikleri kaydedin. 

6. **Derle ve çalıştır** düğmesi ya da **command + r** tuş birleşimi ile projeyi cihaz öykünücüsünde çalıştırın.

   ![Projeyi çalıştırma](media/iot-hub-ios-swift-c2d/run-sample.png)

## <a name="simulate-a-service-device"></a>Hizmet bir cihazın benzetimini gerçekleştirme

Bu bölümde, IOT hub'ı aracılığıyla bulut-cihaz iletilerini gönderen bir Swift uygulaması olan ikinci bir iOS cihaz benzetimi. Bu yapılandırma, IOT senaryoları için kullanışlı bir iPhone veya iPad için başka bir denetleyici çalışmasını olduğu iOS cihazlarını bir IOT hub'ına bağlı.

### <a name="install-cocoapods"></a>CocoaPods yükleme

CocoaPods, üçüncü taraf kitaplıklar kullanan iOS projeleri için bağımlılıkları yönetir.

Önkoşullarda indirdiğiniz Azure IOT iOS örnekler klasörüne gidin. Ardından, örnek hizmet projeye gidin:

```sh
cd quickstart/sample-service
```

XCode’un kapalı olduğundan emin olun ve **podfile** dosyasında bildirilen CocoaPods’u yüklemek üzere aşağıdaki komutu çalıştırın:

```sh
pod install
```

Yükleme komutu, projeniz için gereken podları yüklemeye ek olarak bağımlılıklar için podları kullanacak şekilde önceden yapılandırılmış bir XCode çalışma alanı dosyası da oluşturmuştur.

### <a name="run-the-sample-service-application"></a>Örnek hizmet uygulamayı çalıştırma

1. IOT hub'ınız için hizmeti bağlantı dizesini alın. Bu dizeden kopyalayabilirsiniz [Azure portalında](https://portal.azure.com) gelen **iothubowner** ilkesinde **paylaşılan erişim ilkeleri** dikey penceresinde aşağıdaki CLI komutunu ile alabilir:  

    ```azurecli-interactive
    az iot hub show-connection-string --name {YourIoTHubName} --output table
    ```

2. Örnek çalışma alanını XCode'da açın.

   ```sh
   open AzureIoTServiceSample.xcworkspace
   ```

3. Genişletin **AzureIoTServiceSample** proje ve ardından aynı ada sahip klasörü genişletin.  

4. XCode’da düzenlemek üzere **ViewController.swift** dosyasını açın. 

5. Arama **connectionString** değişkeni ve değeri daha önce kopyaladığınız hizmet bağlantı dizesiyle güncelleştirin.

6. Yaptığınız değişiklikleri kaydedin.

7. Xcode içinde bir başka bir iOS cihazı IOT cihaz çalıştırmak için kullanılan daha öykünücü ayarlarını değiştirin. XCode, aynı türde birden fazla öykünücüleri çalıştıramazsınız.

   ![Öykünücü cihaz değiştirme](media/iot-hub-ios-swift-c2d/change-device.png)

8. Projeyi cihaz öykünücüsünde ile çalıştırmak **derleme ve çalıştırma** düğme veya tuş birleşimi **Command + r**.

   ![Projeyi çalıştırma](media/iot-hub-ios-swift-c2d/run-app.png)

## <a name="send-a-cloud-to-device-message"></a>Bulut buluttan cihaza ileti gönderme

Artık iki uygulamanın göndermek ve bulut-cihaz iletilerini almak için kullanıma hazır.

1. İçinde **iOS uygulaması örneği** sanal IOT cihaz üzerinde çalışan bir uygulamaya tıklayarak **Başlat**. Uygulama, CİHAZDAN buluta iletileri göndermeye başlar ancak aynı zamanda bulut buluttan cihaza iletiler için dinleme başlatır.

   ![Görünüm örnek IOT cihaz uygulaması](media/iot-hub-ios-swift-c2d/view-d2c.png)

2. İçinde **IoTHub hizmeti istemci örneği** sanal hizmet cihazda çalışan uygulama için bir ileti göndermek istediğiniz IOT cihaz kimliği girin. 

3. Düz metin iletisi yazın ve ardından tıklayın **Gönder**.

    Çeşitli eylemler Gönder'i hemen sonra gerçekleşir. Hizmet örneği, IOT hub'ı, ama uygulamanın erişim sağlanan hizmet bağlantı nedeniyle, dize için bir ileti gönderir. IOT hub'ınızın cihaz kimliği denetler, hedef cihaza ileti gönderir ve onay giriş kaynağı cihaza gönderir. Uygulamayı çalıştıran sanal IOT Cihazınızı IOT Hub'ından iletiler denetler ve ekranında en son bir metin yazdırır.

    Çıkışınız aşağıdaki örnekteki gibi görünmelidir:

   ![Bulut-cihaz iletilerini görüntüleme](media/iot-hub-ios-swift-c2d/view-c2d.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bulut-cihaz iletilerini gönderip öğrendiniz.

IOT hub'ı kullanan tam uçtan uca çözümler örneklerini görmek için bkz: [Azure IOT Çözüm Hızlandırıcıları](https://azure.microsoft.com/documentation/suites/iot-suite/) belgeleri.

IOT Hub ile çözümleri geliştirme hakkında daha fazla bilgi için bkz. [IOT Hub Geliştirici kılavuzunun](iot-hub-devguide.md).
