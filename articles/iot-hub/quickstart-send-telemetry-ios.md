---
title: Azure IoT Hub’a telemetri gönderme hızlı başlangıç kılavuzu | Microsoft Docs
description: Bu hızlı başlangıçta bir IoT hub’a sanal telemetri göndermek ve bulutta işlemek üzere IoT hub’dan telemetri okumak amacıyla örnek bir iOS uygulaması çalıştıracaksınız.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: quickstart
ms.custom: mvc
ms.date: 04/03/2019
ms.openlocfilehash: 03a0f285b2e8c74070a30bfbaac50e9bd9c58f65
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67051486"
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-ios"></a>Hızlı Başlangıç: Bir IOT hub'ına (iOS) bir CİHAZDAN telemetri gönderme

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

IoT Hub, IoT cihazlarınızdan buluta depolama veya işleme amacıyla yüksek hacimlerde telemetri almanızı sağlayan bir Azure hizmetidir. Bu makalede, bir simülasyon cihazı uygulamasından IoT Hub’a telemetri göndereceksiniz. Daha sonra, bir arka uç uygulamasından verileri görüntüleyebilirsiniz.

Bu makalede, telemetri göndermek için önceden yazılmış bir Swift uygulaması ve IoT Hub’dan telemetri okumak için bir CLI yardımcı programı kullanılır.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

- [Azure örneklerinden](https://github.com/Azure-Samples/azure-iot-samples-ios/archive/master.zip) kod örneğini indirin
- En son iOS SDK sürümünü çalıştıran en yeni [XCode](https://developer.apple.com/xcode/) sürümü. Bu hızlı başlangıç XCode 10.2 ve iOS 12.2 ile test edilmiştir.
- [CocoaPods](https://guides.cocoapods.org/using/getting-started.html)'un en son sürümü.
- Microsoft Azure IOT uzantısı için Azure CLI Cloud Shell Örneğinize eklemek için aşağıdaki komutu çalıştırın. IOT uzantısı, Azure CLI için IOT Hub, IOT Edge ve IOT cihaz sağlama hizmeti (DPS) belirli komutları ekler.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure Cloud Shell kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. Cihaz kimliği oluşturmak için Azure Cloud Shell'de aşağıdaki komutu çalıştırın.

   **YourIoTHubName** : Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adıyla değiştirin.

   **myiOSdevice** : Bu, kayıtlı bir cihaz için verilen addır. Gösterilen myiOSdevice değerini kullanın. Cihazınız için farklı bir ad seçerseniz bu makalenin geri kalan bölümünde aynı adı kullanmanız ve örnek uygulamaları çalıştırmadan önce bunlarda da cihaz adını güncelleştirmeniz gerekir.

   ```azurecli-interactive
   az iot hub device-identity create --hub-name YourIoTHubName --device-id myiOSdevice
   ```

1. Yeni kaydettiğiniz cihazın _cihaz bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın:

   ```azurecli-interactive
   az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id myiOSdevice --output table
   ```

   Şu ifadeye benzer şekilde görünen cihaz bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız.

## <a name="send-simulated-telemetry"></a>Sanal telemetri gönderme

Örnek uygulama, IoT hub’ınız üzerindeki cihaza özgü bir uç noktaya bağlanan ve sanal sıcaklık ile nem telemetrisini gönderen bir iOS cihazı üzerinde çalışır. 

### <a name="install-cocoapods"></a>CocoaPods yükleme

CocoaPods, üçüncü taraf kitaplıklar kullanan iOS projeleri için bağımlılıkları yönetir.

Yerel terminal penceresinde, önkoşulların bir parçası olarak indirdiğiniz Azure-IoT-Samples-iOS klasörüne gidin. Ardından örnek projeye gidin:

```sh
cd quickstart/sample-device
```

XCode’un kapalı olduğundan emin olun ve **podfile** dosyasında bildirilen CocoaPods’u yüklemek üzere aşağıdaki komutu çalıştırın:

```sh
pod install
```

Yükleme komutu, projeniz için gereken podları yüklemeye ek olarak bağımlılıklar için podları kullanacak şekilde önceden yapılandırılmış bir XCode çalışma alanı dosyası da oluşturmuştur. 

### <a name="run-the-sample-application"></a>Örnek uygulamayı çalıştırın 

1. Örnek çalışma alanını XCode'da açın.

   ```sh
   open "MQTT Client Sample.xcworkspace"
   ```

2. **MQTT İstemci Örneği** projesini genişletin ve sonra aynı ada sahip klasörü genişletin.  
3. XCode’da düzenlemek üzere **ViewController.swift** dosyasını açın. 
4. **connectionString** değişkenini arayın ve değeri daha önce not aldığınız cihaz bağlantı dizesiyle güncelleştirin.
5. Yaptığınız değişiklikleri kaydedin. 
6. **Derle ve çalıştır** düğmesi ya da **command + r** tuş birleşimi ile projeyi cihaz öykünücüsünde çalıştırın. 

   ![Projeyi çalıştırma](media/quickstart-send-telemetry-ios/run-sample.png)

7. Öykünücü açıldığında örnek uygulamada **Başlat**’ı seçin.

Aşağıdaki ekran görüntüsünde, uygulama IoT hub’ınıza sanal telemetri gönderdiğinde oluşan bazı örnek çıktılar gösterilmiştir:

   ![Simülasyon cihazını çalıştırma](media/quickstart-send-telemetry-ios/view-d2c.png)

## <a name="read-the-telemetry-from-your-hub"></a>Hub’ınızdan telemetri okuma

XCode öykünücüsü üzerinde çalıştığınız örnek uygulama, cihazdan gönderilen iletilere ilişkin verileri gösterir. Alınan verileri IoT hub’ınız aracılığıyla da görüntüleyebilirsiniz. IoT Hub uzantısı IoT Hub’ınızdaki bir hizmet tarafı **Olaylar** uç noktasına bağlanabilir. Uzantı, simülasyon cihazınızdan gönderilen cihazdan buluta iletileri alır. IoT Hub arka uç uygulaması genellikle cihazdan buluta iletileri alıp işlemek için bulutta çalışır.

Aşağıdaki komutları Azure Cloud Shell'de çalıştırın, `YourIoTHubName` yerine IoT hub'ınızın adını yazın:

```azurecli-interactive
az iot hub monitor-events --device-id myiOSdevice --hub-name YourIoTHubName
```

Aşağıdaki ekran görüntüsünde uzantı, simülasyon cihazı tarafından hub’a gönderilen telemetriyi aldığında oluşan çıktı gösterilmektedir:

Aşağıdaki ekran görüntüsünde, yerel terminal pencerenizde gördüğünüz telemetri türü gösterilmiştir:

![Telemetri görüntüleme](media/quickstart-send-telemetry-ios/view-telemetry.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede bir IoT hub’ı ayarladınız, bir cihaz kaydettiniz, bir iOS cihazından hub’a sanal telemetri gönderdiniz ve hub’dan telemetri okudunuz. 

Bir arka uç uygulamasından simülasyon cihazınızı denetlemeyi öğrenmek için sonraki hızlı başlangıçla devam edin.

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Bir IOT hub'ına bağlı cihazı denetleme](quickstart-control-device-node.md)
