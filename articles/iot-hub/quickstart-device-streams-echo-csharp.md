---
title: Bir cihaz uygulaması için iletişim C# aracılığıyla Azure IOT Hub cihaz akışları (Önizleme) | Microsoft Docs
description: Bu hızlı başlangıçta, iki örnek çalıştırma C# , IOT hub'ı aracılığıyla kurulan bir cihaz akışını aracılığıyla iletişim kuran uygulamaları.
author: rezasherafat
manager: briz
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: rezas
ms.openlocfilehash: 74a8fc40cff12070f7cea99981eb4e8321d7c1ef
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66735145"
---
# <a name="quickstart-communicate-to-a-device-application-in-c-via-iot-hub-device-streams-preview"></a>Hızlı Başlangıç: Bir cihaz uygulaması'na iletişim C# aracılığıyla IOT Hub cihaz akışları (Önizleme)

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

Azure IOT Hub cihaz akışları olarak şu anda destekleyen bir [önizleme özelliği](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[IOT Hub cihaz akışları](./iot-hub-device-streams-overview.md) güvenli ve güvenlik duvarı uyumlu bir şekilde iletişim kurmak hizmet ve cihaz uygulamalarınıza izin verin. Bu hızlı başlangıçta iki içerir C# sürekli veri (Yankı) göndermek için cihaz akışları yararlanarak uygulamalar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* Cihaz akışları önizlemesi, şu anda şu bölgelerde oluşturulan yalnızca IOT hub'ları için desteklenir:
  * Orta ABD
  * Orta ABD EUAP

* Bu hızlı başlangıçta çalışan iki örnek uygulamaları kullanılarak yazılan C#. Geliştirme makinenize .NET Core SDK'sını 2.1.0 veya sonraki bir sürümü gerekir.
  * İndirme [net'ten birden çok platform için .NET Core SDK](https://www.microsoft.com/net/download/all).
  * Geçerli sürümünü doğrulama C# geliştirme makinenizde aşağıdaki komutu kullanarak:

   ```
   dotnet --version
   ```

* Azure IOT uzantısı, Azure CLI için aşağıdaki komutu çalıştırarak Cloud Shell Örneğinize ekleyin. IOT Hub, IOT Edge ve IOT cihaz sağlama hizmeti (DPS) IOT uzantısını ekler-Azure CLI için özel komutları.

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    ```

* [Örneği indirmek C# proje](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip) ve ZIP arşivini ayıklayın. Hem cihaz tarafında hem de hizmet tarafı gerekir.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub-device-streams](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu bölümde, bir simülasyon cihazı kaydetmek için Azure Cloud Shell kullanın.

1. Cihaz kimliği oluşturma için Cloud Shell'de aşağıdaki komutu çalıştırın:

   > [!NOTE]
   > * Değiştirin *YourIoTHubName* yer tutucu IOT hub'ınız için seçtiğiniz ada sahip.
   > * Kullanım *Cihazım*gösterildiği gibi. Kayıtlı cihaz için verilen addır. Cihazınız için farklı bir ad seçerseniz, bu makalenin tamamında bu adı kullanın ve bunları çalıştırmadan önce örnek uygulamalar, cihaz adını güncelleştirin.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

1. Alınacak *cihaz bağlantı dizesini* yalnızca kayıtlı bir cihaz için Cloud Shell'de aşağıdaki komutu çalıştırın:

   > [!NOTE]
   > Değiştirin *YourIoTHubName* yer tutucu IOT hub'ınız için seçtiğiniz ada sahip.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyDevice --output table
    ```

    Bu hızlı başlangıçta daha sonra kullanmak için cihaz bağlantı dizesini not edin. Aşağıdaki örneğe benzer şekilde görünür:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDevice;SharedAccessKey={YourSharedAccessKey}`

3. Ayrıca gerekir *hizmet bağlantı dizesini* IOT hub'ınıza bağlanmak ve bir cihaz akışını kurmak Hizmet tarafı uygulamasını etkinleştirmek için IOT hub'ından. Aşağıdaki komut, IOT hub'ınız için bu değeri alır:

   > [!NOTE]
   > Değiştirin *YourIoTHubName* yer tutucu IOT hub'ınız için seçtiğiniz ada sahip.

    ```azurecli-interactive
    az iot hub show-connection-string --policy-name service --name YourIoTHubName
    ```

    Bu hızlı başlangıçta daha sonra kullanmak için döndürülen değer unutmayın. Aşağıdaki örneğe benzer şekilde görünür:

   `"HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"`

## <a name="communicate-between-the-device-and-the-service-via-device-streams"></a>Cihaz ve cihaz akışları aracılığıyla hizmeti arasında iletişim

Bu bölümde, hem cihaz tarafında uygulama hem de hizmet tarafı uygulamayı çalıştırın ve ikisi arasındaki iletişim.

### <a name="run-the-service-side-application"></a>Hizmet tarafı uygulamayı çalıştırın

Git *IOT hub/hızlı Başlangıçlar/cihaz akışları-Yankı/hizmet* sıkıştırması proje klasörünüzde dizin. Aşağıdaki bilgiler yararlı bulundurun:

| Parametre adı | Parametre değeri |
|----------------|-----------------|
| `ServiceConnectionString` | IOT hub'ınızın hizmeti bağlantı dizesini belirtin. |
| `DeviceId` | Daha önce oluşturduğunuz cihaz Kimliğini verin (örneğin, *Cihazım*). |

Derleyin ve kodun aşağıdaki gibi çalıştırın:

```
cd ./iot-hub/Quickstarts/device-streams-echo/service/

# Build the application
dotnet build

# Run the application
# In Linux or macOS
dotnet run "<ServiceConnectionString>" "<MyDevice>"

# In Windows
dotnet run <ServiceConnectionString> <MyDevice>
```

> [!NOTE]
> Aygıt tarafı uygulamayı zamanında yanıt vermezse, bir zaman aşımı meydana gelir.

### <a name="run-the-device-side-application"></a>Aygıt tarafı uygulamayı çalıştırın

Git *IOT hub/hızlı Başlangıçlar/cihaz-akışları-Yankı/cihaza* sıkıştırması proje klasörünüzde dizin. Aşağıdaki bilgiler yararlı bulundurun:

| Parametre adı | Parametre değeri |
|----------------|-----------------|
| `DeviceConnectionString` | IOT hub'ınızın cihaz bağlantı dizesini belirtin. |

Derleyin ve kodun aşağıdaki gibi çalıştırın:

```
cd ./iot-hub/Quickstarts/device-streams-echo/device/

# Build the application
dotnet build

# Run the application
# In Linux or macOS
dotnet run "<DeviceConnectionString>"

# In Windows
dotnet run <DeviceConnectionString>
```

Son adımın sonunda, hizmet tarafı uygulama, cihazınıza bir akış başlatır. Akış kurulduktan sonra uygulamayı bir dize arabelleğine hizmetine akış üzerinden gönderir. Bu örnekte, hizmet tarafı uygulamayı yalnızca geri iki uygulama arasındaki başarılı çift yönlü iletişim gösterir cihaz aynı verileri görüntülemektedir.

Konsol, cihaz tarafında çıktısı:

![Cihaz tarafında konsol çıktısı](./media/quickstart-device-streams-echo-csharp/device-console-output.png)

Konsol, hizmet tarafında çıktısı:

![Hizmet tarafında, konsol çıktısı](./media/quickstart-device-streams-echo-csharp/service-console-output.png)

Akış üzerinden gönderilen trafik IOT hub'ı aracılığıyla tünel yerine doğrudan gönderilir. Sağlanan avantajların ayrıntıları [cihaz akışları avantajları](./iot-hub-device-streams-overview.md#benefits).

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources-device-streams](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir IOT hub'ı ayarladınız, kayıtlı bir cihaz, cihaz akışı arasında kurulan C# cihaz ve hizmet tarafında, uygulamalar ve akış uygulamaları arasında sürekli veri göndermek için kullanılır.

Cihaz akışları hakkında daha fazla bilgi için bkz:

> [!div class="nextstepaction"]
> [Cihaz akışları genel bakış](./iot-hub-device-streams-overview.md)
