---
title: Azure IoT Hub’dan bir cihazı denetleme hızlı başlangıcı (.NET) | Microsoft Docs
description: Bu hızlı başlangıçta iki örnek C# uygulaması çalıştırırsınız. Bir uygulama, hub’ınıza bağlı cihazları uzaktan denetleyebilen bir arka uç uygulamasıdır. Diğer uygulama, uzaktan denetlenebilen hub’ınıza bağlanan bir cihazın simülasyonunu yapar.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc
ms.date: 06/20/2018
ms.author: dobett
ms.openlocfilehash: c8ef958b2f39a9271b9fa344f61329d48eccdee4
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2018
ms.locfileid: "51514755"
---
# <a name="quickstart-control-a-device-connected-to-an-iot-hub-net"></a>Hızlı Başlangıç: IoT hub’a bağlı bir cihazı denetleme (.NET)

[!INCLUDE [iot-hub-quickstarts-2-selector](../../includes/iot-hub-quickstarts-2-selector.md)]

IoT Hub, IoT cihazlarınızdan buluta yüksek hacimlerde telemetri almanızı ve buluttan cihazlarınızı yönetmenizi sağlayan bir Azure hizmetidir. Bu hızlı başlangıçta, IoT hub’ınıza bağlı bir simülasyon cihazını denetlemek için *doğrudan yöntem* kullanırsınız. IoT hub’ınıza bağlı bir cihazın davranışını uzaktan değiştirmek için doğrudan yöntemler kullanabilirsiniz.

Hızlı başlangıçta, önceden yazılmış iki .NET uygulaması kullanılır:

* Bir arka uç uygulamasından çağrılan doğrudan yöntemlere yanıt veren bir simülasyon cihazı uygulaması. Doğrudan yöntem çağrıları almak için bu uygulama, IoT hub’ınızda aygıta özgü bir uç noktaya bağlanır.

* Simülasyon cihazında doğrudan yöntemler çağıran bir arka uç uygulaması. Bir cihazda doğrudan yöntem çağırmak için bu uygulama, IoT hub’ınızda sunucu tarafı uç noktasına bağlanır.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta çalıştırdığınız iki örnek uygulama, C# kullanılarak yazılır. Geliştirme makinenizde .NET Core SDK 2.1.0 veya üzeri bir sürüm olması gerekir.

[.NET](https://www.microsoft.com/net/download/all)’ten birden fazla platform için .NET Core SDK’sını indirebilirsiniz.

Aşağıdaki komutu kullanarak geliştirme makinenizde geçerli C# sürümünü doğrulayabilirsiniz:

```cmd/sh
dotnet --version
```

Örnek C# projesini önceden indirmediyseniz https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip adresinden indirip ZIP arşivini ayıklayın.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Önceki [Hızlı Başlangıç: Bir cihazdan IoT hub’a telemetri gönderme](quickstart-send-telemetry-dotnet.md) öğreticisini tamamladıysanız bu adımı atlayabilirsiniz.

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Önceki [Hızlı Başlangıç: Bir cihazdan IoT hub’a telemetri gönderme](quickstart-send-telemetry-dotnet.md) öğreticisini tamamladıysanız bu adımı atlayabilirsiniz.

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure Cloud Shell kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. Aşağıdaki komutları Azure Cloud Shell'de çalıştırarak IoT Hub CLI uzantısını ekleyin ve cihaz kimliğini oluşturun. 

   **YourIoTHubName** : aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adı ile değiştirin.

   **MyDotnetDevice**: Kaydedilen cihaza verilen addır. Gösterilen MyDotnetDevice değerini kullanın. Cihazınız için farklı bir ad seçerseniz bu makalenin geri kalan bölümünde aynı adı kullanmanız ve örnek uygulamaları çalıştırmadan önce bunlarda da cihaz adını güncelleştirmeniz gerekir.

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create \
      --hub-name YourIoTHubName --device-id MyDotnetDevice
    ```

2. Yeni kaydettiğiniz cihazın _cihaz bağlantı dizesini_ almak için aşağıdaki komutları Azure Cloud Shell'de çalıştırın:

   **YourIoTHubName** : aşağıda bu yer tutucu adı ile değiştirin, coose IOT hub'ınız için.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string \
      --hub-name YourIoTHubName \
      --device-id MyDotnetDevice 
      --output table
    ```

    Şu ifadeye benzer şekilde görünen cihaz bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız.

## <a name="retrieve-the-service-connection-string"></a>Hizmet bağlantı dizesini alma

Arka uç uygulamasının hub’a bağlanmasına ve iletileri almasına olanak sağlamak için IoT hub _hizmet bağlantı dizesi_ de gerekir. Aşağıdaki komut, IoT hub'ınız için hizmeti bağlantı dizesini alır:

```azurecli-interactive
az iot hub show-connection-string --hub-name YourIoTHubName --output table
```

Şu ifadeye benzer şekilde görünen hizmet bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={YourSharedAccessKey}`

Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız. Hizmet bağlantı dizesi, cihaz bağlantı dizesinden farklıdır.  

## <a name="listen-for-direct-method-calls"></a>Doğrudan yöntem çağrılarını dinleme

Simülasyon cihazı, IoT hub’ınızdaki cihaza özgü bir uç noktaya bağlanır, sanal telemetri gönderir ve hub’ınızdan gelen doğrudan yöntem çağrılarını dinler. Bu hızlı başlangıçta, hub’dan gelen doğrudan yöntem çağrısı, telemetri gönderme aralığını değiştirmesini cihaza bildirir. Simülasyon cihazı, doğrudan yöntemi yürüttükten sonra hub’ınıza geri bir onay gönderir.

1. Yerel terminal penceresinde, örnek C# projesinin kök klasörüne gidin. Daha sonra **iot-hub\Quickstarts\simulated-device-2** klasörüne gidin.

2. **SimulatedDevice.cs** dosyasını, istediğiniz bir metin düzenleyicide açın.

    `s_connectionString` değişkeninin değerini, önceden not ettiğiniz cihaz bağlantı dizesiyle değiştirin. Daha sonra **SimulatedDevice.cs** dosyasına değişikliklerinizi kaydedin.

3. Yerel terminal penceresinde, aşağıdaki komutları çalıştırarak simülasyon cihazı uygulaması için gerekli paketleri yükleyin:

    ```cmd/sh
    dotnet restore
    ```

4. Yerel terminal penceresinde, aşağıdaki komutu çalıştırarak simülasyon cihazı uygulamasını derleyip çalıştırın:

    ```cmd/sh
    dotnet run
    ```

    Aşağıdaki ekran görüntüsünde, simülasyon cihazı uygulaması, IoT hub’ınıza telemetri gönderdiğinde oluşan çıktı gösterilmektedir:

    ![Simülasyon cihazını çalıştırma](./media/quickstart-control-device-dotnet/SimulatedDevice-1.png)

## <a name="call-the-direct-method"></a>Doğrudan yöntem çağırma

Arka uç uygulaması, IoT Hub’ınızdaki bir hizmet tarafı uç noktasına bağlanır. Uygulama, IoT hub’ınız üzerinden bir cihaza doğrudan yöntem çağrıları yapar ve onayları dinler. IoT Hub arka uç uygulaması genellikle bulutta çalışır.

1. Başka bir yerel terminal penceresinde, örnek C# projesinin kök klasörüne gidin. Daha sonra **iot-hub\Quickstarts\back-end-application** klasörüne gidin.

2. **BackEndApplication.cs** dosyasını, istediğiniz bir metin düzenleyicide açın.

    `s_connectionString` değişkeninin değerini, önceden not ettiğiniz hizmet bağlantı dizesiyle değiştirin. Sonra **BackEndApplication.cs** dosyasına değişikliklerinizi kaydedin.

3. Yerel terminal penceresinde, aşağıdaki komutları çalıştırarak arka uç uygulaması için gerekli kitaplıkları yükleyin:

    ```cmd/sh
    dotnet restore
    ```

4. Yerel terminal penceresinde, aşağıdaki komutları çalıştırarak arka uç uygulamasını derleyip çalıştırın:

    ```cmd/sh
    dotnet run
    ```

    Aşağıdaki ekran görüntüsünde, uygulama cihaza bir doğrudan yöntem çağrısı yapıp onay aldığında elde edilen çıktı gösterilmektedir:

    ![Arka uç uygulamasını çalıştırma](./media/quickstart-control-device-dotnet/BackEndApplication.png)

    Arka uç uygulamasını çalıştırdıktan sonra, simülasyon cihazını çalıştıran konsol penceresinde bir ileti ve ileti değişikliklerini gönderdiği hızı görürsünüz:

    ![Sanal istemcide değişiklik](./media/quickstart-control-device-dotnet/SimulatedDevice-2.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir doğrudan yöntem bir cihazda bir arka uç uygulamasından çağrılır ve bir sanal cihaz uygulaması doğrudan yöntem çağrısında yanıt verdi.

Cihazdan buluta iletileri, buluttaki farklı hedeflere yönlendirmeyi öğrenmek için sonraki öğreticiyle devam edin.

> [!div class="nextstepaction"]
> [Öğretici: Telemetriyi işlenmek üzere farklı uç noktalara yönlendirme](tutorial-routing.md)