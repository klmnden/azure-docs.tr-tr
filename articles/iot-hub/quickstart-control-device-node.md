---
title: Azure IoT Hub’dan bir cihazı denetleme hızlı başlangıcı (Node.js) | Microsoft Docs
description: Bu hızlı başlangıçta iki örnek Node.js uygulaması çalıştırırsınız. Bir uygulama, hub’ınıza bağlı cihazları uzaktan denetleyebilen bir arka uç uygulamasıdır. Diğer uygulama, uzaktan denetlenebilen hub’ınıza bağlanan bir cihazın simülasyonunu yapar.
services: iot-hub
author: dominicbetts
manager: timlt
editor: ''
ms.service: iot-hub
ms.devlang: node
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: ns
ms.date: 04/30/2018
ms.author: dobett
ms.openlocfilehash: a3f60409d6b337ef314963d2116e71f156d6dafd
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="quickstart-control-a-device-connected-to-an-iot-hub-nodejs"></a>Hızlı Başlangıç: IoT hub’a bağlı bir cihazı denetleme (Node.js)

[!INCLUDE [iot-hub-quickstarts-2-selector](../../includes/iot-hub-quickstarts-2-selector.md)]

IoT Hub, IoT cihazlarınızdan buluta yüksek hacimlerde telemetri almanızı ve buluttan cihazlarınızı yönetmenizi sağlayan bir Azure hizmetidir. Bu hızlı başlangıçta, IoT hub’ınıza bağlı bir simülasyon cihazını denetlemek için *doğrudan yöntem* kullanırsınız. IoT hub’ınıza bağlı bir cihazın davranışını uzaktan değiştirmek için doğrudan yöntemler kullanabilirsiniz.

Hızlı başlangıçta, önceden yazılmış iki Node.js uygulaması kullanılır:

* Bir arka uç uygulamasından çağrılan doğrudan yöntemlere yanıt veren bir simülasyon cihazı uygulaması. Doğrudan yöntem çağrıları almak için bu uygulama, IoT hub’ınızda aygıta özgü bir uç noktaya bağlanır.
* Simülasyon cihazında doğrudan yöntemler çağıran bir arka uç uygulaması. Bir cihazda doğrudan yöntem çağırmak için bu uygulama, IoT hub’ınızda sunucu tarafı uç noktasına bağlanır.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıçta çalıştırdığınız iki örnek uygulama, Node.js kullanılarak yazılır. Geliştirme makinenizde Node.js v4.x.x veya sonraki bir sürüm olması gerekir.

[nodejs.org](https://nodejs.org) adresinden birden fazla platform için Node.js’yi indirebilirsiniz.

Aşağıdaki komutu kullanarak geliştirme makinenizde geçerli Node.js sürümünü doğrulayabilirsiniz:

```cmd/sh
node --version
```

Örnek Node.js projesini önceden indirmediyseniz https://github.com/Azure-Samples/azure-iot-samples-node/archive/master.zip adresinden indirip ZIP arşivini ayıklayın.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Önceki [Hızlı Başlangıç: Bir cihazdan IoT hub’a telemetri gönderme](quickstart-send-telemetry-node.md) öğreticisini tamamladıysanız bu adımı atlayabilirsiniz.

[!INCLUDE [iot-hub-quickstarts-create-hub](../../includes/iot-hub-quickstarts-create-hub.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Önceki [Hızlı Başlangıç: Bir cihazdan IoT hub’a telemetri gönderme](quickstart-send-telemetry-node.md) öğreticisini tamamladıysanız bu adımı atlayabilirsiniz.

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure CLI kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. IoT Hub CLI uzantısını ekleyin ve cihaz kimliğini oluşturun. `{YourIoTHubName}` değerini, IoT hub’ınızın adıyla değiştirin:

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name {YourIoTHubName}--device-id MyNodeDevice
    ```

1. Yeni kaydettiğiniz cihazın _cihaz bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın:

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id MyNodeDevice --output table
    ```

    `Hostname=...=` ifadesine benzer şekilde görünen cihaz bağlantı dizesini not edin. Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız.

1. Arka uç uygulamasının IoT hub’ınıza bağlanmasına ve iletileri almasına olanak sağlamak için bir _hizmet bağlantı dizesi_ de gerekir. Aşağıdaki komut, IoT hub'ınız için hizmeti bağlantı dizesini alır:

    ```azurecli-interactive
    az iot hub show-connection-string --hub-name {YourIoTHubName} --output table
    ```

    `Hostname=...=` ifadesine benzer şekilde görünen hizmet bağlantı dizesini not edin. Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız. Hizmet bağlantı dizesi, cihaz bağlantı dizesinden farklıdır.

## <a name="listen-for-direct-method-calls"></a>Doğrudan yöntem çağrılarını dinleme

Simülasyon cihazı, IoT hub’ınızdaki cihaza özgü bir uç noktaya bağlanır, sanal telemetri gönderir ve hub’ınızdan gelen doğrudan yöntem çağrılarını dinler. Bu hızlı başlangıçta, hub’dan gelen doğrudan yöntem çağrısı, telemetri gönderme aralığını değiştirmesini cihaza bildirir. Simülasyon cihazı, doğrudan yöntemi yürüttükten sonra hub’ınıza geri bir onay gönderir.

1. Terminal penceresinde, örnek Node.js projesinin kök klasörüne gidin. Ardından **Quickstarts\simulated-device-2** klasörüne gidin.

1. **SimulatedDevice.js** dosyasını, istediğiniz bir metin düzenleyicide açın.

    `connectionString` değişkeninin değerini, önceden not ettiğiniz cihaz bağlantı dizesiyle değiştirin. Daha sonra **SimulatedDevice.js** dosyasına değişikliklerinizi kaydedin.

1. Terminal penceresinde, aşağıdaki komutları çalıştırarak gerekli kitaplıkları yükleyin ve simülasyon cihazı uygulamasını çalıştırın:

    ```cmd/sh
    npm install
    node SimulatedDevice.js
    ```

    Aşağıdaki ekran görüntüsünde, simülasyon cihazı uygulaması, IoT hub’ınıza telemetri gönderdiğinde oluşan çıktı gösterilmektedir:

    ![Simülasyon cihazını çalıştırma](media/quickstart-control-device-node/SimulatedDevice-1.png)

## <a name="call-the-direct-method"></a>Doğrudan yöntem çağırma

Arka uç uygulaması, IoT Hub’ınızdaki bir hizmet tarafı uç noktasına bağlanır. Uygulama, IoT hub’ınız üzerinden bir cihaza doğrudan yöntem çağrıları yapar ve onayları dinler. IoT Hub arka uç uygulaması genellikle bulutta çalışır.

1. Başka bir terminal penceresinde, örnek Node.js projesinin kök klasörüne gidin. Ardından **Quickstarts\back-end-application** klasörüne gidin.

1. **BackEndApplication.js** dosyasını, istediğiniz bir metin düzenleyicide açın.

    `connectionString` değişkeninin değerini, önceden not ettiğiniz hizmet bağlantı dizesiyle değiştirin. Sonra **BackEndApplication.js** dosyasına değişikliklerinizi kaydedin.

1. Terminal penceresinde, aşağıdaki komutları çalıştırarak gerekli kitaplıkları yükleyin ve arka uç uygulamasını çalıştırın:

    ```cmd/sh
    npm install
    node BackEndApplication.js
    ```

    Aşağıdaki ekran görüntüsünde, uygulama cihaza bir doğrudan yöntem çağrısı yapıp onay aldığında elde edilen çıktı gösterilmektedir:

    ![Arka uç uygulamasını çalıştırma](media/quickstart-control-device-node/BackEndApplication.png)

    Arka uç uygulamasını çalıştırdıktan sonra, simülasyon cihazını çalıştıran konsol penceresinde bir ileti ve ileti değişikliklerini gönderdiği hızı görürsünüz:

    ![Sanal istemcide değişiklik](media/quickstart-control-device-node/SimulatedDevice-2.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticilere geçmeyi planlıyorsanız, kaynak grubunu ve IoT hub’ı değiştirmeden bırakın ve sonra bunları yeniden kullanın.

Artık gerekli değilse portaldan IoT hub’ı ve kaynak grubunu silin. Bunu yapmak için, IoT hub’ınızı içeren kaynak grubunu seçin ve **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir arka uç uygulamasındaki cihazda doğrudan yöntem çağırdınız ve simülasyon cihazı uygulamasında doğrudan yöntem çağrısına yanıt verdiniz.

Cihazdan buluta iletileri, buluttaki farklı hedeflere yönlendirmeyi öğrenmek için sonraki öğreticiyle devam edin.

> [!div class="nextstepaction"]
> [Öğretici: Telemetriyi işlenmek üzere farklı uç noktalara yönlendirme](iot-hub-node-node-process-d2c.md)