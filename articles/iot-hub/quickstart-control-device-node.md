---
title: Azure IoT Hub’dan bir cihazı denetleme hızlı başlangıcı (Node.js) | Microsoft Docs
description: Bu hızlı başlangıçta iki örnek Node.js uygulaması çalıştırırsınız. Bir uygulama, hub’ınıza bağlı cihazları uzaktan denetleyebilen bir arka uç uygulamasıdır. Diğer uygulama, uzaktan denetlenebilen hub’ınıza bağlanan bir cihazın simülasyonunu yapar.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: quickstart
ms.custom: mvc
ms.date: 06/21/2019
ms.openlocfilehash: 462b485b5dc5106ab18959ab2cdbfe1bfc0f94bf
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67330571"
---
# <a name="quickstart-control-a-device-connected-to-an-iot-hub-nodejs"></a>Hızlı Başlangıç: Bir IOT hub'ına (Node.js) bağlı cihazı denetleme

[!INCLUDE [iot-hub-quickstarts-2-selector](../../includes/iot-hub-quickstarts-2-selector.md)]

IoT Hub, IoT cihazlarınızdan buluta yüksek hacimlerde telemetri almanızı ve buluttan cihazlarınızı yönetmenizi sağlayan bir Azure hizmetidir. Bu hızlı başlangıçta, IoT hub’ınıza bağlı bir simülasyon cihazını denetlemek için *doğrudan yöntem* kullanırsınız. IoT hub’ınıza bağlı bir cihazın davranışını uzaktan değiştirmek için doğrudan yöntemler kullanabilirsiniz.

Hızlı başlangıçta, önceden yazılmış iki Node.js uygulaması kullanılır:

* Bir arka uç uygulamasından çağrılan doğrudan yöntemlere yanıt veren bir simülasyon cihazı uygulaması. Doğrudan yöntem çağrıları almak için bu uygulama, IoT hub’ınızda aygıta özgü bir uç noktaya bağlanır.

* Simülasyon cihazında doğrudan yöntemler çağıran bir arka uç uygulaması. Bir cihazda doğrudan yöntem çağırmak için bu uygulama, IoT hub’ınızda sunucu tarafı uç noktasına bağlanır.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta çalıştırdığınız iki örnek uygulama, Node.js kullanılarak yazılır. Geliştirme makinenize Node.js v10.x.x veya sonraki bir sürümü gerekir.

[nodejs.org](https://nodejs.org) adresinden birden fazla platform için Node.js’yi indirebilirsiniz.

Aşağıdaki komutu kullanarak geliştirme makinenizde geçerli Node.js sürümünü doğrulayabilirsiniz:

```cmd/sh
node --version
```

Microsoft Azure IOT uzantısı için Azure CLI Cloud Shell Örneğinize eklemek için aşağıdaki komutu çalıştırın. IOT uzantısı, Azure CLI için IOT Hub, IOT Edge ve IOT cihaz sağlama hizmeti (DPS) belirli komutları ekler.

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

Örnek Node.js projesini önceden indirmediyseniz https://github.com/Azure-Samples/azure-iot-samples-node/archive/master.zip adresinden indirip ZIP arşivini ayıklayın.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Önceki tamamladıysanız [hızlı başlangıç: Bir IOT hub'ına bir CİHAZDAN telemetri gönderme](quickstart-send-telemetry-node.md), bu adımı atlayabilirsiniz.

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Önceki tamamladıysanız [hızlı başlangıç: Bir IOT hub'ına bir CİHAZDAN telemetri gönderme](quickstart-send-telemetry-node.md), bu adımı atlayabilirsiniz.

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure Cloud Shell kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. Cihaz kimliği oluşturmak için Azure Cloud Shell'de aşağıdaki komutu çalıştırın.

   **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adı ile değiştirin.

   **MyNodeDevice**: Kaydettirmekte cihazın adı. Kullanım **MyNodeDevice** gösterildiği gibi. Cihazınız için farklı bir ad seçerseniz, bu makalenin tamamında adı ve örnek uygulamalarda bunları çalıştırmadan önce cihaz adı gerekir.

    ```azurecli-interactive
    az iot hub device-identity create \
      --hub-name YourIoTHubName --device-id MyNodeDevice
    ```

2. Yeni kaydettiğiniz cihazın _cihaz bağlantı dizesini_ almak için aşağıdaki komutları Azure Cloud Shell'de çalıştırın:

    **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adı ile değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string \
      --hub-name YourIoTHubName \
      --device-id MyNodeDevice \
      --output table
    ```

    Şu ifadeye benzer şekilde görünen cihaz bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız.

3. Arka uç uygulamasının IoT hub’ınıza bağlanmasına ve iletileri almasına olanak sağlamak için bir _hizmet bağlantı dizesi_ de gerekir. Aşağıdaki komut, IoT hub'ınız için hizmeti bağlantı dizesini alır:

    **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adı ile değiştirin.

    ```azurecli-interactive
    az iot hub show-connection-string \
      --name YourIoTHubName --policy-name service --output table
    ```

    Şu ifadeye benzer şekilde görünen hizmet bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız. Hizmet bağlantı dizesi, cihaz bağlantı dizesinden farklıdır.

## <a name="listen-for-direct-method-calls"></a>Doğrudan yöntem çağrılarını dinleme

Simülasyon cihazı, IoT hub’ınızdaki cihaza özgü bir uç noktaya bağlanır, sanal telemetri gönderir ve hub’ınızdan gelen doğrudan yöntem çağrılarını dinler. Bu hızlı başlangıçta, hub’dan gelen doğrudan yöntem çağrısı, telemetri gönderme aralığını değiştirmesini cihaza bildirir. Simülasyon cihazı, doğrudan yöntemi yürüttükten sonra hub’ınıza geri bir onay gönderir.

1. Yerel terminal penceresinde örnek Node.js projesinin kök klasörüne gidin. Daha sonra **iot-hub\Quickstarts\simulated-device-2** klasörüne gidin.

2. **SimulatedDevice.js** dosyasını, istediğiniz bir metin düzenleyicide açın.

    `connectionString` değişkeninin değerini, önceden not ettiğiniz cihaz bağlantı dizesiyle değiştirin. Daha sonra **SimulatedDevice.js** dosyasına değişikliklerinizi kaydedin.

3. Yerel terminal penceresinde, aşağıdaki komutları çalıştırarak gerekli kitaplıkları yükleyin ve simülasyon cihazı uygulamasını çalıştırın:

    ```cmd/sh
    npm install
    node SimulatedDevice.js
    ```

    Aşağıdaki ekran görüntüsünde, simülasyon cihazı uygulaması, IoT hub’ınıza telemetri gönderdiğinde oluşan çıktı gösterilmektedir:

    ![Simülasyon cihazını çalıştırma](./media/quickstart-control-device-node/SimulatedDevice-1.png)

## <a name="call-the-direct-method"></a>Doğrudan yöntem çağırma

Arka uç uygulaması, IoT Hub’ınızdaki bir hizmet tarafı uç noktasına bağlanır. Uygulama, IoT hub’ınız üzerinden bir cihaza doğrudan yöntem çağrıları yapar ve onayları dinler. IoT Hub arka uç uygulaması genellikle bulutta çalışır.

1. Başka bir yerel terminal penceresinde örnek Node.js projesinin kök klasörüne gidin. Daha sonra **iot-hub\Quickstarts\back-end-application** klasörüne gidin.

2. **BackEndApplication.js** dosyasını, istediğiniz bir metin düzenleyicide açın.

    `connectionString` değişkeninin değerini, önceden not ettiğiniz hizmet bağlantı dizesiyle değiştirin. Sonra **BackEndApplication.js** dosyasına değişikliklerinizi kaydedin.

3. Yerel terminal penceresinde, aşağıdaki komutları çalıştırarak gerekli kitaplıkları yükleyin ve arka uç uygulamasını çalıştırın:

    ```cmd/sh
    npm install
    node BackEndApplication.js
    ```

    Aşağıdaki ekran görüntüsünde, uygulama cihaza bir doğrudan yöntem çağrısı yapıp onay aldığında elde edilen çıktı gösterilmektedir:

    ![Arka uç uygulamasını çalıştırma](./media/quickstart-control-device-node/BackEndApplication.png)

    Arka uç uygulamasını çalıştırdıktan sonra, simülasyon cihazını çalıştıran konsol penceresinde bir ileti ve ileti değişikliklerini gönderdiği hızı görürsünüz:

    ![Sanal istemcide değişiklik](./media/quickstart-control-device-node/SimulatedDevice-2.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir doğrudan yöntem bir cihazda bir arka uç uygulamasından çağrılır ve bir sanal cihaz uygulaması doğrudan yöntem çağrısında yanıt verdi.

Cihazdan buluta iletileri, buluttaki farklı hedeflere yönlendirmeyi öğrenmek için sonraki öğreticiyle devam edin.

> [!div class="nextstepaction"]
> [Öğretici: Rota telemetri işleme için farklı uç](tutorial-routing.md)