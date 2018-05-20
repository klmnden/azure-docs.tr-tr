---
title: Azure IoT Hub’a telemetri gönderme hızlı başlangıcı (Java) | Microsoft Docs
description: Bu hızlı başlangıçta bir IoT hub’a sanal telemetri göndermek ve bulutta işlemek üzere IoT hub’dan gelen telemetriyi okumak için iki örnek Java uygulaması çalıştırırsınız.
services: iot-hub
author: dominicbetts
manager: timlt
editor: ''
ms.service: iot-hub
ms.devlang: java
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: ns
ms.date: 04/30/2018
ms.author: dobett
ms.openlocfilehash: 36005988611e7ec3f16146919e3ab3f04755e7e5
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-and-read-the-telemetry-from-the-hub-with-a-back-end-application-java"></a>Hızlı Başlangıç: Bir cihazdan IoT hub’ına telemetri gönderme ve arka uç uygulaması ile hub’dan telemetriyi okuma (Java)

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

IoT Hub, IoT cihazlarınızdan buluta depolama veya işleme amacıyla yüksek hacimlerde telemetri almanızı sağlayan bir Azure hizmetidir. Bu hızlı başlangıçta, bir simülasyon cihazı uygulamasından bir arka uç uygulamasına işlenmek üzere IoT Hub aracılığıyla telemetri gönderirsiniz.

Hızlı başlangıçta, biri telemetriyi göndermek için, diğeri de hub’dan telemetriyi okumak için olmak üzere önceden yazılmış iki Java uygulaması kullanılır. Bu iki uygulamayı çalıştırmadan önce bir IoT hub oluşturur ve hub’a bir cihaz kaydedersiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıçta çalıştırdığınız iki örnek uygulama, Java kullanılarak yazılır. Geliştirme makinenizde Java SE 8 veya sonraki bir sürüm olması gerekir.

[Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html)’dan birden fazla platform için Java’yı indirebilirsiniz.

Aşağıdaki komutu kullanarak geliştirme makinenizde geçerli Java sürümünü doğrulayabilirsiniz:

```cmd/sh
java --version
```

Örnekleri oluşturmak için Maven 3 yüklemeniz gerekir. [Apache Maven](https://maven.apache.org/download.cgi)’den birden fazla platform için Maven’i indirebilirsiniz.

Aşağıdaki komutu kullanarak geliştirme makinenizde geçerli Maven sürümünü doğrulayabilirsiniz:

```cmd/sh
mvn --version
```

https://github.com/Azure-Samples/azure-iot-samples-java/archive/master.zip adresinden örnek Java projesini indirin ve ZIP arşivini ayıklayın.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-quickstarts-create-hub](../../includes/iot-hub-quickstarts-create-hub.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure CLI kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. IoT Hub CLI uzantısını ekleyin ve cihaz kimliğini oluşturun. `{YourIoTHubName}` değerini, IoT hub’ınız için seçtiğiniz adla değiştirin:

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id MyJavaDevice
    ```

    Cihazınız için farklı bir ad seçerseniz örnek uygulamaları çalıştırmadan önce bunlarda cihaz adını güncelleştirin.

1. Yeni kaydettiğiniz cihazın _cihaz bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın:

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id MyJavaDevice --output table
    ```

    `Hostname=...=` ifadesine benzer şekilde görünen cihaz bağlantı dizesini not edin. Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız.

1. Arka uç uygulamasının IoT hub’ınıza bağlanmasını ve iletileri almasını sağlamak için IoT hub’ınızdaki _Event Hubs uyumlu uç nokta_, _Event Hubs uyumlu yol_ ve _iothubowner birincil anahtarı_ da gerekir. Aşağıdaki komutlar, IoT hub’ınız için şu değerleri alır:

    ```azurecli-interactive
    az iot hub show --query properties.eventHubEndpoints.events.endpoint --name {YourIoTHubName}

    az iot hub show --query properties.eventHubEndpoints.events.path --name {YourIoTHubName}

    az iot hub policy show --name iothubowner --query primaryKey --hub-name {your IoT Hub name}
    ```

    Bu üç değeri not edin. Hızlı başlangıcın ilerleyen kısmında bunları kullanacaksınız.

## <a name="send-simulated-telemetry"></a>Sanal telemetri gönderme

Simülasyon cihazı uygulaması, IoT hub’ınız üzerindeki cihaza özgü bir uç noktaya bağlanır ve sanal sıcaklık ve nem telemetrisi gönderir.

1. Terminal penceresinde, örnek Java projesinin kök klasörüne gidin. Ardından **Quickstarts\simulated-device** klasörüne gidin.

1. **src/main/java/com/microsoft/docs/iothub/samples/SimulatedDevice.java** dosyasını istediğiniz bir metin düzenleyicide açın.

    `connString` değişkeninin değerini, önceden not ettiğiniz cihaz bağlantı dizesiyle değiştirin. Daha sonra **SimulatedDevice.java** dosyasına değişikliklerinizi kaydedin.

1. Terminal penceresinde, aşağıdaki komutları çalıştırarak gerekli kitaplıkları yükleyin ve simülasyon cihazı uygulamasını derleyin:

    ```cmd/sh
    mvn clean package
    ```

1. Terminal penceresinde, aşağıdaki komutları çalıştırarak simülasyon cihazı uygulamasını çalıştırın:

    ```cmd/sh
    java -jar target/simulated-device-1.0.0-with-deps.jar
    ```

    Aşağıdaki ekran görüntüsünde, simülasyon cihazı uygulaması, IoT hub’ınıza telemetri gönderdiğinde oluşan çıktı gösterilmektedir:

    ![Simülasyon cihazını çalıştırma](media/quickstart-send-telemetry-java/SimulatedDevice.png)

## <a name="read-the-telemetry-from-your-hub"></a>Hub’ınızdan telemetri okuma

Arka uç uygulaması, IoT Hub’ınızdaki bir hizmet tarafı **Olaylar** uç noktasına bağlanır. Uygulama, simülasyon cihazınızdan gönderilen cihazdan buluta iletileri alır. IoT Hub arka uç uygulaması genellikle cihazdan buluta iletileri alıp işlemek için bulutta çalışır.

1. Başka bir terminal penceresinde, örnek Java projesinin kök klasörüne gidin. Ardından **Quickstarts\read-d2c-messages** klasörüne gidin.

1. **src/main/java/com/microsoft/docs/iothub/samples/ReadDeviceToCloudMessages.java** dosyasını istediğiniz bir metin düzenleyicide açın.

    `eventHubsCompatibleEndpoint` değişkeninin değerini, önceden not ettiğiniz Event Hubs uyumlu uç noktayla değiştirin.

    `eventHubsCompatiblePath` değişkeninin değerini, önceden not ettiğiniz Event Hubs uyumlu yolla değiştirin.

    `iotHubSasKey` değişkeninin değerini, önceden not ettiğiniz iothubowner birincil anahtarıyla değiştirin. Ardından **ReadDeviceToCloudMessages.java** dosyasına değişikliklerinizi kaydedin.

1. Terminal penceresinde, aşağıdaki komutları çalıştırarak gerekli kitaplıkları yükleyin ve arka uç uygulamasını derleyin:

    ```cmd/sh
    mvn clean package
    ```

1. Terminal penceresinde, aşağıdaki komutları çalıştırarak arka uç uygulamasını çalıştırın:

    ```cmd/sh
    java -jar target/read-d2c-messages-1.0.0-with-deps.jar
    ```

    Aşağıdaki ekran görüntüsünde, arka uç uygulaması, simülasyon cihazı tarafından hub’a gönderilen telemetriyi aldığında oluşan çıktı gösterilmektedir:

    ![Arka uç uygulamasını çalıştırma](media/quickstart-send-telemetry-java/ReadDeviceToCloud.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki hızlı başlangıcı tamamlamayı planlıyorsanız, kaynak grubunu ve IoT hub’ı değiştirmeden bırakın ve sonra bunları yeniden kullanın.

Artık gerekli değilse portaldan IoT hub’ı ve kaynak grubunu silin. Bunu yapmak için, IoT hub’ınızı içeren **qs-iot-hub-rg** kaynak grubunu seçin ve **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir IoT hub’ını ayarladınız, bir cihazı kaydettiniz, Java uygulamasını kullanarak hub’a sanal telemetri gönderdiniz ve basit bir arka uç uygulamasını kullanarak hub’dan telemetriyi okudunuz.

Bir arka uç uygulamasından simülasyon cihazınızı denetlemeyi öğrenmek için sonraki hızlı başlangıçla devam edin.

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: IoT hub’a bağlı bir cihazı denetleme](quickstart-control-device-java.md)