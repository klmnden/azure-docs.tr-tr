---
title: Bir cihaz uygulaması için iletişim C# aracılığıyla Azure IOT Hub cihaz akışları (Önizleme) | Microsoft Docs
description: Bu hızlı başlangıçta, iki örnek çalışacak C# , IOT hub'ı aracılığıyla kurulan bir cihaz akışını aracılığıyla iletişim kuran uygulamaları.
author: rezasherafat
manager: briz
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: rezas
ms.openlocfilehash: 8df57d3d36dcae851c9c0e23ea609e200a429605
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65832887"
---
# <a name="quickstart-communicate-to-a-device-application-in-c-via-iot-hub-device-streams-preview"></a>Hızlı Başlangıç: Bir cihaz uygulaması'na iletişim C# aracılığıyla IOT Hub cihaz akışları (Önizleme)

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

Microsoft Azure IOT Hub cihaz akışları olarak şu anda destekleyen bir [önizleme özelliği](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[IOT Hub cihaz akışları](./iot-hub-device-streams-overview.md) güvenli ve güvenlik duvarı uyumlu bir şekilde iletişim kurmak hizmet ve cihaz uygulamalarınıza izin verin. Bu hızlı başlangıçta iki içerir C# sürekli veri (Yankı) göndermek için cihaz akışları yararlanan programlar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

*  Cihaz akışları şu anda önizlemesidir yalnızca IOT hub'ları aşağıdaki bölgelerde oluşturulan için desteklenir:

   *  **Orta ABD**

   *  **Orta ABD EUAP**

Bu hızlı başlangıçta çalıştırdığınız iki örnek uygulama, C# kullanılarak yazılır. Geliştirme makinenizde .NET Core SDK 2.1.0 veya üzeri bir sürüm olması gerekir.

*  İndirme [net'ten birden çok platform için .NET Core SDK](https://www.microsoft.com/net/download/all).

Aşağıdaki komutu kullanarak geliştirme makinenizde geçerli C# sürümünü doğrulayabilirsiniz:

```
dotnet --version
```

*  Microsoft Azure IOT uzantısı için Azure CLI Cloud Shell Örneğinize eklemek için aşağıdaki komutu çalıştırın. IOT uzantısı, Azure CLI için IOT Hub, IOT Edge ve IOT cihaz sağlama hizmeti (DPS) belirli komutları ekler.

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    ```

* https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip adresinden örnek C# projesini indirin ve ZIP arşivini ayıklayın. Hem cihaz hem de hizmet tarafında ihtiyacınız.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub-device-streams](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure Cloud Shell kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. Cihaz kimliği oluşturmak için Azure Cloud Shell'de aşağıdaki komutu çalıştırın.

   **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adıyla değiştirin.

   **Cihazım**: Bu, kayıtlı bir cihaz için verilen addır. Cihazım gösterildiği gibi kullanın. Cihazınız için farklı bir ad seçerseniz bu makalenin geri kalan bölümünde aynı adı kullanmanız ve örnek uygulamaları çalıştırmadan önce bunlarda da cihaz adını güncelleştirmeniz gerekir.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

2. Yeni kaydettiğiniz cihazın _cihaz bağlantı dizesini_ almak için aşağıdaki komutları Azure Cloud Shell'de çalıştırın:

   **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adıyla değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyDevice --output table
    ```

    Aşağıdaki gibi görünen cihaz bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDevice;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız.

3. Ayrıca gerekir *hizmet bağlantı dizesini* IOT hub'ınıza bağlanmak ve bir cihaz akışını kurmak Hizmet tarafı uygulamasını etkinleştirmek için IOT hub'ından. Aşağıdaki komut, IOT hub'ınız için bu değeri alır:

   **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adıyla değiştirin.

    ```azurecli-interactive
    az iot hub show-connection-string --policy-name service --name YourIoTHubName
    ```

    Şuna benzer döndürülen değeri not edin:

   `"HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"`

## <a name="communicate-between-device-and-service-via-device-streams"></a>Cihaz ve hizmet aracılığıyla cihaz akışları arasında iletişim

Bu bölümde, hem cihaz tarafında uygulama hem de hizmet tarafı uygulamayı çalıştırın ve ikisi arasındaki iletişim.

### <a name="run-the-service-side-application"></a>Hizmet tarafı uygulamayı çalıştırın

Gidin `iot-hub/Quickstarts/device-streams-echo/service` sıkıştırması proje klasörünüzde. Aşağıdaki bilgiler yararlı gerekir:

| Parametre adı | Parametre değeri |
|----------------|-----------------|
| `ServiceConnectionString` | IOT hub'ınızın hizmeti bağlantı dizesini belirtin. |
| `DeviceId` | Örneğin, Cihazım daha önce oluşturulan cihaz kimliği sağlayın. |

Derleyin ve kodun aşağıdaki gibi çalıştırın:

```
cd ./iot-hub/Quickstarts/device-streams-echo/service/

# Build the application
dotnet build

# Run the application
# In Linux/MacOS
dotnet run "<ServiceConnectionString>" "<MyDevice>"

# In Windows
dotnet run <ServiceConnectionString> <MyDevice>
```

> [!NOTE]
> Aygıt tarafı uygulamayı zamanında yanıt vermezse, bir zaman aşımı meydana gelir.

### <a name="run-the-device-side-application"></a>Aygıt tarafı uygulamayı çalıştırın

Gidin `iot-hub/Quickstarts/device-streams-echo/device` sıkıştırması proje klasörünüzde dizin. Aşağıdaki bilgiler yararlı gerekir:

| Parametre adı | Parametre değeri |
|----------------|-----------------|
| `DeviceConnectionString` | IOT hub'ınızın cihaz bağlantı dizesini belirtin. |

Derleyin ve kodun aşağıdaki gibi çalıştırın:

```
cd ./iot-hub/Quickstarts/device-streams-echo/device/

# Build the application
dotnet build

# Run the application
# In Linux/MacOS
dotnet run "<DeviceConnectionString>"

# In Windows
dotnet run <DeviceConnectionString>
```

Son adımın sonunda, hizmet tarafı program cihazınıza ve kurulan sonra bir akışı başlatacak bir dize arabelleğine akış üzerinden hizmete gönderin. Bu örnekte, hizmet tarafı programı yalnızca geri başarılı çift yönlü iletişim iki uygulama arasındaki gösteren bir cihaza aynı verileri görüntülemektedir. Aşağıdaki şekle bakın.

Konsol, cihaz tarafında çıktısı:

![Aygıt tarafı konsol çıktısı](./media/quickstart-device-streams-echo-csharp/device-console-output.png)

Hizmet tarafı üzerinde çıkışını konsolu: ![Hizmet tarafında, konsol çıktısı](./media/quickstart-device-streams-echo-csharp/service-console-output.png )

Akış üzerinden gönderilen trafik, IOT hub'ı yerine doğrudan gönderilen tünel. Sağlanan avantajların ayrıntıları [cihaz akışları avantajları](./iot-hub-device-streams-overview.md#benefits).

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources-device-streams](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir IOT hub'ı ayarladınız, kayıtlı bir cihaz, cihaz akışı arasında kurulan C# cihaz ve hizmet tarafında, uygulamalar ve akış uygulamaları arasında sürekli veri göndermek için kullanılır.

Cihaz akışları hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kullanın:

> [!div class="nextstepaction"]
> [Cihaz akışları genel bakış](./iot-hub-device-streams-overview.md)
