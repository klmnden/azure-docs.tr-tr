---
title: Azure IoT Hub’dan bir cihazı denetleme hızlı başlangıcı (Java) | Microsoft Docs
description: Bu hızlı başlangıçta iki örnek Java uygulaması çalıştıracaksınız. Bir uygulama, hub’ınıza bağlı cihazları uzaktan denetleyebilen bir arka uç uygulamasıdır. Diğer uygulama, uzaktan denetlenebilen hub’ınıza bağlanan bir cihazın simülasyonunu yapar.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: quickstart
ms.custom: mvc
ms.date: 06/22/2018
ms.author: dobett
ms.openlocfilehash: 5da4248f0b0a72c3614b4c3e5ea042c4341f4e03
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38573509"
---
# <a name="quickstart-control-a-device-connected-to-an-iot-hub-java"></a>Hızlı Başlangıç: IoT hub’a bağlı bir cihazı denetleme (Java)

[!INCLUDE [iot-hub-quickstarts-2-selector](../../includes/iot-hub-quickstarts-2-selector.md)]

IoT Hub, IoT cihazlarınızdan buluta yüksek hacimlerde telemetri almanızı ve buluttan cihazlarınızı yönetmenizi sağlayan bir Azure hizmetidir. Bu hızlı başlangıçta, IoT hub’ınıza bağlı bir simülasyon cihazını denetlemek için *doğrudan yöntem* kullanırsınız. IoT hub’ınıza bağlı bir cihazın davranışını uzaktan değiştirmek için doğrudan yöntemler kullanabilirsiniz.

Hızlı başlangıçta, önceden yazılmış iki Java uygulaması kullanılır:

* Bir arka uç uygulamasından çağrılan doğrudan yöntemlere yanıt veren bir simülasyon cihazı uygulaması. Doğrudan yöntem çağrıları almak için bu uygulama, IoT hub’ınızda aygıta özgü bir uç noktaya bağlanır.
* Simülasyon cihazında doğrudan yöntemler çağıran bir arka uç uygulaması. Bir cihazda doğrudan yöntem çağırmak için bu uygulama, IoT hub’ınızda sunucu tarafı uç noktasına bağlanır.

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

Örnek Java projesini önceden indirmediyseniz https://github.com/Azure-Samples/azure-iot-samples-java/archive/master.zip adresinden indirip ZIP arşivini ayıklayın.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Önceki [Hızlı Başlangıç: Bir cihazdan IoT hub’a telemetri gönderme](quickstart-send-telemetry-java.md) öğreticisini tamamladıysanız bu adımı atlayabilirsiniz.

[!INCLUDE [iot-hub-quickstarts-create-hub](../../includes/iot-hub-quickstarts-create-hub.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Önceki [Hızlı Başlangıç: Bir cihazdan IoT hub’a telemetri gönderme](quickstart-send-telemetry-java.md) öğreticisini tamamladıysanız bu adımı atlayabilirsiniz.

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure CLI kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. IoT Hub CLI uzantısını ekleyin ve cihaz kimliğini oluşturun. `{YourIoTHubName}` değerini, IoT hub’ınız için seçtiğiniz adla değiştirin:

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id MyJavaDevice
    ```

    Cihazınız için farklı bir ad seçerseniz örnek uygulamaları çalıştırmadan önce bunlarda cihaz adını güncelleştirin.

2. Yeni kaydettiğiniz cihazın _cihaz bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın:

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id MyJavaDevice --output table
    ```

    `Hostname=...=` ifadesine benzer şekilde görünen cihaz bağlantı dizesini not edin. Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız.

## <a name="retrieve-the-service-connection-string"></a>Hizmet bağlantı dizesini alma

Arka uç uygulamasının hub’a bağlanmasına ve iletileri almasına olanak sağlamak için IoT hub _hizmet bağlantı dizesi_ de gerekir. Aşağıdaki komut, IoT hub'ınız için hizmeti bağlantı dizesini alır:

```azurecli-interactive
az iot hub show-connection-string --hub-name {YourIoTHubName} --output table
```

`Hostname=...=` ifadesine benzer şekilde görünen hizmet bağlantı dizesini not edin. Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız.

## <a name="listen-for-direct-method-calls"></a>Doğrudan yöntem çağrılarını dinleme

Simülasyon cihazı, IoT hub’ınızdaki cihaza özgü bir uç noktaya bağlanır, sanal telemetri gönderir ve hub’ınızdan gelen doğrudan yöntem çağrılarını dinler. Bu hızlı başlangıçta, hub’dan gelen doğrudan yöntem çağrısı, telemetri gönderme aralığını değiştirmesini cihaza bildirir. Simülasyon cihazı, doğrudan yöntemi yürüttükten sonra hub’ınıza geri bir onay gönderir.

1. Terminal penceresinde, örnek Java projesinin kök klasörüne gidin. Daha sonra **iot-hub\Quickstarts\simulated-device-2** klasörüne gidin.

2. **src/main/java/com/microsoft/docs/iothub/samples/SimulatedDevice.java** dosyasını istediğiniz bir metin düzenleyicide açın.

    `connString` değişkeninin değerini, önceden not ettiğiniz cihaz bağlantı dizesiyle değiştirin. Daha sonra **SimulatedDevice.java** dosyasına değişikliklerinizi kaydedin.

3. Terminal penceresinde, aşağıdaki komutları çalıştırarak gerekli kitaplıkları yükleyin ve simülasyon cihazı uygulamasını derleyin:

    ```cmd/sh
    mvn clean package
    ```

4. Terminal penceresinde, aşağıdaki komutları çalıştırarak simülasyon cihazı uygulamasını çalıştırın:

    ```cmd/sh
    java -jar target/simulated-device-2-1.0.0-with-deps.jar
    ```

    Aşağıdaki ekran görüntüsünde, simülasyon cihazı uygulaması, IoT hub’ınıza telemetri gönderdiğinde oluşan çıktı gösterilmektedir:

    ![Simülasyon cihazını çalıştırma](media/quickstart-control-device-java/SimulatedDevice-1.png)

## <a name="call-the-direct-method"></a>Doğrudan yöntem çağırma

Arka uç uygulaması, IoT Hub’ınızdaki bir hizmet tarafı uç noktasına bağlanır. Uygulama, IoT hub’ınız üzerinden bir cihaza doğrudan yöntem çağrıları yapar ve onayları dinler. IoT Hub arka uç uygulaması genellikle bulutta çalışır.

1. Başka bir terminal penceresinde, örnek Java projesinin kök klasörüne gidin. Daha sonra **iot-hub\Quickstarts\back-end-application** klasörüne gidin.

2. **src/main/java/com/microsoft/docs/iothub/samples/BackEndApplication.java** dosyasını istediğiniz bir metin düzenleyicide açın.

    `iotHubConnectionString` değişkeninin değerini, önceden not ettiğiniz hizmet bağlantı dizesiyle değiştirin. Sonra **BackEndApplication.java** dosyasına değişikliklerinizi kaydedin.

3. Terminal penceresinde, aşağıdaki komutları çalıştırarak gerekli kitaplıkları yükleyin ve arka uç uygulamasını derleyin:

    ```cmd/sh
    mvn clean package
    ```

4. Terminal penceresinde, aşağıdaki komutları çalıştırarak arka uç uygulamasını çalıştırın:

    ```cmd/sh
    java -jar target/back-end-application-1.0.0-with-deps.jar
    ```

    Aşağıdaki ekran görüntüsünde, uygulama cihaza bir doğrudan yöntem çağrısı yapıp onay aldığında elde edilen çıktı gösterilmektedir:

    ![Arka uç uygulamasını çalıştırma](media/quickstart-control-device-java/BackEndApplication.png)

    Arka uç uygulamasını çalıştırdıktan sonra, simülasyon cihazını çalıştıran konsol penceresinde bir ileti ve ileti değişikliklerini gönderdiği hızı görürsünüz:

    ![Sanal istemcide değişiklik](media/quickstart-control-device-java/SimulatedDevice-2.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir arka uç uygulamasındaki cihazda doğrudan yöntem çağırdınız ve simülasyon cihazı uygulamasında doğrudan yöntem çağrısına yanıt verdiniz.

Cihazdan buluta iletileri, buluttaki farklı hedeflere yönlendirmeyi öğrenmek için sonraki öğreticiyle devam edin.

> [!div class="nextstepaction"]
> [Öğretici: Telemetriyi işlenmek üzere farklı uç noktalara yönlendirme](tutorial-routing.md)