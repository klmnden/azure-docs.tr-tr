---
title: Azure IoT Hub’a telemetri gönderme hızlı başlangıcı (Node.js) | Microsoft Docs
description: Bu hızlı başlangıçta bir IoT hub’a sanal telemetri göndermek ve bulutta işlemek üzere IoT hub’dan gelen telemetriyi okumak için iki örnek Node.js uygulaması çalıştırırsınız.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: node
ms.topic: quickstart
ms.custom: mvc
ms.date: 02/22/2019
ms.openlocfilehash: b99ed85495e00282c6a27f42b5817e46f4736720
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65597511"
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-and-read-it-with-a-back-end-application-nodejs"></a>Hızlı Başlangıç: Bir IOT hub'ına bir CİHAZDAN telemetri gönderme ve arka uç uygulaması ile (Node.js) okuyun

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

IoT Hub, IoT cihazlarınızdan buluta depolama veya işleme amacıyla yüksek hacimlerde telemetri almanızı sağlayan bir Azure hizmetidir. Bu hızlı başlangıçta, bir simülasyon cihazı uygulamasından bir arka uç uygulamasına işlenmek üzere IoT Hub aracılığıyla telemetri gönderirsiniz.

Hızlı başlangıçta, biri telemetriyi göndermek için, diğeri de hub’dan telemetriyi okumak için olmak üzere önceden yazılmış iki Node.js uygulaması kullanılır. Bu iki uygulamayı çalıştırmadan önce bir IoT hub oluşturur ve hub’a bir cihaz kaydedersiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı Başlangıçta Çalıştır iki örnek uygulama, node.js'de yazılmış. Geliştirme makinenize Node.js v10.x.x veya sonraki bir sürümü gerekir.

[nodejs.org](https://nodejs.org) adresinden birden fazla platform için Node.js’yi indirebilirsiniz.

Aşağıdaki komutu kullanarak geliştirme makinenizde geçerli Node.js sürümünü doğrulayabilirsiniz:

```cmd/sh
node --version
```

Microsoft Azure IOT uzantısı için Azure CLI Cloud Shell Örneğinize eklemek için aşağıdaki komutu çalıştırın. IOT uzantısı, Azure CLI için IOT Hub, IOT Edge ve IOT cihaz sağlama hizmeti (DPS) belirli komutları ekler.

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

https://github.com/Azure-Samples/azure-iot-samples-node/archive/master.zip adresinden örnek Node.js projesini indirin ve ZIP arşivini ayıklayın.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure Cloud Shell kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. Cihaz kimliği oluşturmak için Azure Cloud Shell'de aşağıdaki komutu çalıştırın.

   **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adıyla değiştirin.

   **MyNodeDevice**: Kaydettirmekte cihazın adı. Kullanım **MyNodeDevice** gösterildiği gibi. Cihazınız için farklı bir ad seçerseniz, bu makalenin tamamında adı ve örnek uygulamalarda bunları çalıştırmadan önce cihaz adı gerekir.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyNodeDevice
    ```

1. Yeni kaydettiğiniz cihazın _cihaz bağlantı dizesini_ almak için aşağıdaki komutları Azure Cloud Shell'de çalıştırın:

   **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adıyla değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyNodeDevice --output table
    ```

    Şu ifadeye benzer şekilde görünen cihaz bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız.

1. Arka uç uygulamasının IoT hub’ınıza bağlanmasına ve iletileri almasına olanak sağlamak için bir _hizmet bağlantı dizesi_ de gerekir. Aşağıdaki komut, IoT hub'ınız için hizmeti bağlantı dizesini alır:

   **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adıyla değiştirin.

    ```azurecli-interactive
    az iot hub show-connection-string --name YourIoTHubName --output table
    ```

    Şu ifadeye benzer şekilde görünen hizmet bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız. Hizmet bağlantı dizesi, cihaz bağlantı dizesinden farklıdır.

## <a name="send-simulated-telemetry"></a>Sanal telemetri gönderme

Simülasyon cihazı uygulaması, IoT hub’ınız üzerindeki cihaza özgü bir uç noktaya bağlanır ve sanal sıcaklık ve nem telemetrisi gönderir.

1. Yerel terminal pencerenizi açın ve örnek Node.js projesinin kök klasörüne gidin. Daha sonra **iot-hub\Quickstarts\simulated-device** klasörüne gidin.

1. **SimulatedDevice.js** dosyasını, istediğiniz bir metin düzenleyicide açın.

    `connectionString` değişkeninin değerini, önceden not ettiğiniz cihaz bağlantı dizesiyle değiştirin. Daha sonra **SimulatedDevice.js** dosyasına değişikliklerinizi kaydedin.

1. Yerel terminal penceresinde, aşağıdaki komutları çalıştırarak gerekli kitaplıkları yükleyin ve simülasyon cihazı uygulamasını çalıştırın:

    ```cmd/sh
    npm install
    node SimulatedDevice.js
    ```

    Aşağıdaki ekran görüntüsünde, simülasyon cihazı uygulaması, IoT hub’ınıza telemetri gönderdiğinde oluşan çıktı gösterilmektedir:

    ![Simülasyon cihazını çalıştırma](media/quickstart-send-telemetry-node/SimulatedDevice.png)

## <a name="read-the-telemetry-from-your-hub"></a>Hub’ınızdan telemetri okuma

Arka uç uygulaması, IoT Hub’ınızdaki bir hizmet tarafı **Olaylar** uç noktasına bağlanır. Uygulama, simülasyon cihazınızdan gönderilen cihazdan buluta iletileri alır. IoT Hub arka uç uygulaması genellikle cihazdan buluta iletileri alıp işlemek için bulutta çalışır.

1. Başka bir yerel terminal penceresi açın ve örnek Node.js projesinin kök klasörüne gidin. Daha sonra **iot-hub\Quickstarts\read-d2c-messages** klasörüne gidin.

1. **ReadDeviceToCloudMessages.js** dosyasını istediğiniz bir metin düzenleyicide açın.

    `connectionString` değişkeninin değerini, önceden not ettiğiniz hizmet bağlantı dizesiyle değiştirin. Ardından **ReadDeviceToCloudMessages.js** dosyasına değişikliklerinizi kaydedin.

1. Yerel terminal penceresinde, aşağıdaki komutları çalıştırarak gerekli kitaplıkları yükleyin ve arka uç uygulamasını çalıştırın:

    ```cmd/sh
    npm install
    node ReadDeviceToCloudMessages.js
    ```

    Aşağıdaki ekran görüntüsünde, arka uç uygulaması, simülasyon cihazı tarafından hub’a gönderilen telemetriyi aldığında oluşan çıktı gösterilmektedir:

    ![Arka uç uygulamasını çalıştırma](media/quickstart-send-telemetry-node/ReadDeviceToCloud.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir IoT hub’ını ayarladınız, bir cihazı kaydettiniz, Node.js uygulamasını kullanarak hub’a sanal telemetri gönderdiniz ve basit bir arka uç uygulamasını kullanarak hub’dan telemetriyi okudunuz.

Bir arka uç uygulamasından simülasyon cihazınızı denetlemeyi öğrenmek için sonraki hızlı başlangıçla devam edin.

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Bir IOT hub'ına bağlı cihazı denetleme](quickstart-control-device-node.md)
