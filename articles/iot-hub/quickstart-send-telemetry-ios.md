---
title: Azure IoT Hub’a telemetri gönderme hızlı başlangıç kılavuzu | Microsoft Docs
description: Bu hızlı başlangıçta bir IoT hub’a sanal telemetri göndermek ve bulutta işlemek üzere IoT hub’dan telemetri okumak amacıyla örnek bir iOS uygulaması çalıştıracaksınız.
services: iot-hub
author: kgremban
manager: timlt
editor: ''
ms.service: iot-hub
ms.devlang: ''
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: ns
ms.date: 04/20//2018
ms.author: kgremban
ms.openlocfilehash: 8b95bb18f2e8941c10f7bcdf6a60e7fda6ab0ea5
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="send-telemetry-from-a-device-to-an-iot-hub-swift"></a>Bir cihazdan IoT hub’a telemetri gönderme (Swift)

IoT Hub, IoT cihazlarınızdan buluta depolama veya işleme amacıyla yüksek hacimlerde telemetri almanızı sağlayan bir Azure hizmetidir. Bu makalede, bir simülasyon cihazı uygulamasından IoT Hub’a telemetri göndereceksiniz. Daha sonra, bir arka uç uygulamasından verileri görüntüleyebilirsiniz. 

Bu makalede, telemetri göndermek için önceden yazılmış bir Swift uygulaması ve IoT Hub’dan telemetri okumak için bir CLI yardımcı programı kullanılır. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

- [Azure örneklerinden](https://github.com/Azure-Samples/azure-iot-samples-ios/archive/master.zip) kod örneğini indirin 
- En son iOS SDK sürümünü çalıştıran en yeni [XCode](https://developer.apple.com/xcode/) sürümü. Bu hızlı başlangıç XCode 9.3 ve iOS 11.3 ile test edilmiştir.
- [CocoaPods](https://guides.cocoapods.org/using/getting-started.html)'un en son sürümü.
- IoT Hub’dan telemetri okuyan iothub-explorer CLI yardımcı programı. Yüklemek için öncelikle [Node.js](https://nodejs.org) v4.x.x veya üzerini yükleyin, ardından aşağıdaki komutu çalıştırın: 

   ```sh
   sudo npm install -g iothub-explorer
   ```

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Birinci adım, Azure portalını kullanarak aboneliğinizde bir IoT hub’ı oluşturmaktır. IoT hub’ı, çok sayıda cihazdan buluta yüksek hacimlerde telemetri almanızı sağlar. Hub daha sonra o telemetriyi okuyup işlemek üzere bulutta çalışan bir veya daha fazla arka uç hizmetini etkinleştirir.

1. [Azure Portal](http://portal.azure.com)’da oturum açın.

1. **Kaynak oluştur** > **Nesnelerin İnterneti** > **Iot Hub** seçeneğini belirleyin. 

   ![IoT Hub’ı yüklemek için seçin](media/quickstart-send-telemetry-ios/selectiothub.png)

1. IoT hub’ınızı oluşturmak için aşağıdaki tabloda bulunan değerleri kullanın:

    | Ayar | Değer |
    | ------- | ----- |
    | Adı | Hub’ınız için benzersiz bir ad |
    | Fiyatlandırma ve ölçek katmanı | F1 Ücretsiz |
    | IoT Hub birimleri | 1 |
    | Cihazdan buluta bölümler | 2 bölüm |
    | Abonelik | Azure aboneliğiniz. |
    | Kaynak grubu | Yeni oluşturun. Kaynak grubunuz için bir ad girin. |
    | Konum | Size en yakın konumu seçin. |
    | Panoya sabitle | Yes |

1. **Oluştur**’a tıklayın.  

   ![Hub ayarları](media/quickstart-send-telemetry-ios/hubdefinition.png)

1. IoT hub ve kaynak grubu adlarını not alın. Bu değerleri daha sonra bu hızlı başlangıçta kullanacaksınız.

## <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure CLI kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. IoT Hub CLI uzantısını ekleyin ve cihaz kimliğini oluşturun. `{YourIoTHubName}` değerini IoT hub’ınızın adıyla değiştirin:

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   az iot hub device-identity create --hub-name {YourIoTHubName} --device-id myiOSdevice
   ```

1. Yeni kaydettiğiniz cihazın _cihaz bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın:

   ```azurecli-interactive
   az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id myiOSdevice --output table
   ```

   `Hostname=...=` ifadesine benzer şekilde görünen cihaz bağlantı dizesini not edin. Bu değeri makalenin sonraki bölümlerinde kullanacaksınız.

1. IoT hub’ınıza bağlanacak arka uç uygulamalarını etkinleştirmek ve cihazdan buluta iletileri almak için bir _hizmet bağlantı dizesi_ de gereklidir. Aşağıdaki komut, IoT hub'ınız için hizmeti bağlantı dizesini alır:

   ```azurecli-interactive
   az iot hub show-connection-string --hub-name {YourIoTHubName} --output table
   ```

   `Hostname=...=` ifadesine benzer şekilde görünen hizmet bağlantı dizesini not edin. Bu değeri makalenin sonraki bölümlerinde kullanacaksınız.

## <a name="send-simulated-telemetry"></a>Sanal telemetri gönderme

Örnek uygulama, IoT hub’ınız üzerindeki cihaza özgü bir uç noktaya bağlanan ve sanal sıcaklık ile nem telemetrisini gönderen bir iOS cihazı üzerinde çalışır. 

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

XCode öykünücüsü üzerinde çalıştığınız örnek uygulama, cihazdan gönderilen iletilere ilişkin verileri gösterir. Alınan verileri IoT hub’ınız aracılığıyla da görüntüleyebilirsiniz. `iothub-explorer` CLI yardımcı programı, Iot Hub’ınız üzerinde sunucu tarafı **Olaylar** uç noktasına bağlanır. 

Yeni bir terminal penceresi açın. {hub hizmet bağlantı dizeniz} ifadesini bu makalenin başında aldığınız hizmet bağlantı dizesiyle değiştirerek aşağıdaki komutu çalıştırın:

```sh
iothub-explorer monitor-events myiOSdevice --login "{your hub service connection string}"
```

Aşağıdaki ekran görüntüsünde, terminal pencerenizde gördüğünüz telemetri türü gösterilmiştir:

![Telemetri görüntüleme](media/quickstart-send-telemetry-ios/view-telemetry.png)

iothub-explorer komutunu çalıştırdığınızda bir hata alıyorsanız, IoT cihazınızın *cihaz bağlantı dizesi* yerine IoT hub’ınıza ait *hizmet bağlantı dizesini* kullanıp kullanmadığınızı bir kez daha kontrol edin. Her iki bağlantı dizesi de **Hostname={iothubname}** ile başlar ancak hizmet bağlantı dizesi **SharedAccessKeyName** özelliğini, cihaz bağlantı dizesi ise **DeviceID** özelliğini içerir. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

IoT Hub’ınızı diğer makalelerle test etmeye devam etmeyi planlıyorsanız, kaynak grubu ve IoT hub’ınızdan ayrılıp daha sonra yeniden kullanabilirsiniz.

Artık gerekli değilse portaldan IoT hub’ı ve kaynak grubunu silin. Bunu yapmak için, IoT hub’ınızı içeren kaynak grubunu seçin ve **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede bir IoT hub’ı ayarladınız, bir cihaz kaydettiniz, bir iOS cihazından hub’a sanal telemetri gönderdiniz ve hub’dan telemetri okudunuz. 

iOS cihazlarının IoT Hub ile nasıl çalıştığı hakkında bilgi almaya devam etmek için bkz. [iOS ile buluttan cihaza iletiler gönderme (Swift)](iot-hub-ios-swift-c2d.md)

<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
