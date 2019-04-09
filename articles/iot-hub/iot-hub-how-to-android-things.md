---
title: Azure IOT SDK'larını kullanarak Android şeyler platformlar için geliştirin | Microsoft Docs
description: Geliştirici Kılavuzu - Azure IOT Hub SDK'larını kullanarak Android şeylere geliştirme hakkında bilgi edinin.
author: yzhong94
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: yizhon
ms.openlocfilehash: 8e36cee9857c00fcb618a8491595432fb0fd60fd
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59264582"
---
# <a name="develop-for-android-things-platform-using-azure-iot-sdks"></a>Azure IOT SDK'larını kullanarak Android şeyler platformlar için geliştirin

[Azure IOT Hub SDK'ları](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks) Windows, Linux, OSX, MBED ve Android ve iOS gibi mobil platformları gibi popüler platformlar için ilk katman desteği sağlar.  Büyük seçme hakkını ve esnekliği IOT dağıtımlarda etkinleştirmek için taahhüdümüzün bir parçası olarak, Java SDK'yı da destekler [Android şeyler](https://developer.android.com/things/) platform.  Geliştiriciler, kullanırken Android şeyler işletim sistemi, cihaz tarafında avantajlarından yararlanabilir [Azure IOT hub'ı](about-iot-hub.md) merkezi iletiyi aynı anda milyonlarca için ölçeklendirilen hub cihazları bağlı.

Bu öğreticide, Azure IOT Java SDK'sını kullanarak Android şey üzerinde bir cihaz tarafı uygulamayı oluşturmak için adımları açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

* Bir Android şeyler donanım Android şeyler çalışan işletim sistemi ile desteklenir.  İzleyebileceğiniz [Android şeyler belgeleri](https://developer.android.com/things/get-started/kits#flash-at) Android şeyler işletim sistemi flash konusunda.  Android şeyler Cihazınızı klavye, ekran ve fare bağlı gibi temel çevre ile İnternet'e bağlı olduğundan emin olun.  Bu öğreticide, Raspberry Pi 3 kullanılır.

* En son sürümünü [Android Studio](https://developer.android.com/studio/)

* En son sürümünü [Git](https://git-scm.com/)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure Cloud Shell kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. Aşağıdaki komutları Azure Cloud Shell'de çalıştırarak IoT Hub CLI uzantısını ekleyin ve cihaz kimliğini oluşturun.

   **YourIoTHubName** : Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adıyla değiştirin.

   **MyAndroidThingsDevice** : Bu, kayıtlı bir cihaz için verilen addır. MyAndroidThingsDevice gösterildiği gibi kullanın. Cihazınız için farklı bir ad seçerseniz bu makalenin geri kalan bölümünde aynı adı kullanmanız ve örnek uygulamaları çalıştırmadan önce bunlarda da cihaz adını güncelleştirmeniz gerekir.

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyAndroidThingsDevice
    ```

2. Azure Cloud Shell içinde almak için aşağıdaki komutları çalıştırın *cihaz bağlantı dizesini* yeni kaydettiğiniz cihazın. Değiştirin `YourIoTHubName` aşağıda adı ile IOT hub'ınız için seçin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyAndroidThingsDevice --output table
    ```

    Şu ifadeye benzer şekilde görünen cihaz bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyAndroidThingsDevice;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız.

## <a name="building-an-android-things-application"></a>Bir şeyler Android uygulaması oluşturma

1. Bir şeyler Android uygulaması oluşturmak için ilk adımı Android şeyler cihazlarınıza bağlanıyor. Android şeyler Cihazınızı bir ekrana bağlama ve internet'e bağlanın. Android şeyler sağlamak [belgeleri](https://developer.android.com/things/get-started/kits) WiFi bağlanma. İnternet'e bağlandıktan sonra bir ağlar altında listelenen IP adresini not edin.

2. Kullanım [adb](https://developer.android.com/studio/command-line/adb) yukarıda belirtilen IP adresi ile Android şeyler cihazınıza bağlanmak için aracı. Çift terminalinizi bu komutunu kullanarak bağlantıyı denetleyin. "Bağlı" listelenen cihazlarınızı görmeniz gerekir.

   ```
   adb devices
   ```

3. Örneğimizi Android/Android işlemler için bunu indirmek [depo](https://github.com/Azure-Samples/azure-iot-samples-java) veya Git'i kullanabilirsiniz.

   ```
   git clone https://github.com/Azure-Samples/azure-iot-samples-java.git
   ```

4. Android Studio'da bulunan "\azure-iot-samples-java\iot-hub\Samples\device\AndroidSample" Android projeyi açın.

5. Gradle.Properties dosyasını açın ve cihaz bağlantısı dizeniz ile "Device_connection_string daha önce not ettiğiniz" değiştirin.
 
6. Tıklayın çalıştırma - hata ayıklama ve bu kod, Android şeyler cihazlara dağıtmak için Cihazınızı seçin.

7. Uygulama başarıyla başlatıldıktan sonra Android şeyler Cihazınızda çalışan bir uygulama görebilirsiniz. Bu örnek uygulama, rastgele oluşturulan sıcaklık okumalar gönderir.

## <a name="read-the-telemetry-from-your-hub"></a>Hub’ınızdan telemetri okuma

Alınan verileri IOT hub'ınız görüntüleyebilirsiniz. IoT Hub uzantısı IoT Hub’ınızdaki bir hizmet tarafı **Olaylar** uç noktasına bağlanabilir. Uzantı, simülasyon cihazınızdan gönderilen cihazdan buluta iletileri alır. IoT Hub arka uç uygulaması genellikle cihazdan buluta iletileri alıp işlemek için bulutta çalışır.

Aşağıdaki komutları Azure Cloud Shell'de çalıştırın, `YourIoTHubName` yerine IoT hub'ınızın adını yazın:

```azurecli-interactive
az iot hub monitor-events --device-id MyAndroidThingsDevice --hub-name YourIoTHubName
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [bağlantı ve güvenilir Mesajlaşma yönetme](iot-hub-reliability-features-in-sdks.md) IOT Hub SDK'larını kullanarak.
* Hakkında bilgi [mobil platformlar için geliştirme](iot-hub-how-to-develop-for-mobile-devices.md) iOS ve Android gibi.
* [Azure IOT SDK'sı platform desteği](iot-hub-device-sdk-platform-support.md)
