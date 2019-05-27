---
title: C cihaz uygulamasında Azure IOT Hub cihaz akışları (Önizleme) aracılığıyla kullanıcılara | Microsoft Docs
description: Bu hızlı başlangıçta, bir IOT cihazı ile iletişim kuran bir C Hizmet tarafı uygulama bir cihaza akış çalışacaktır.
author: rezasherafat
manager: briz
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: rezas
ms.openlocfilehash: f5e6128c1ecceda181f92b2d81e9ac06effbfce2
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65834075"
---
# <a name="quickstart-communicate-to-a-device-application-in-c-via-iot-hub-device-streams-preview"></a>Hızlı Başlangıç: C cihaz uygulama IOT Hub cihaz akışları (Önizleme) ile iletişim

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

Microsoft Azure IOT Hub cihaz akışları olarak şu anda destekleyen bir [önizleme özelliği](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[IOT Hub cihaz akışları](iot-hub-device-streams-overview.md) güvenli ve güvenlik duvarı uyumlu bir şekilde iletişim kurmak hizmet ve cihaz uygulamalarınıza izin verin. Genel Önizleme süresince C SDK'sı cihaz tarafında yalnızca cihaz akışlarını destekler. Sonuç olarak, bu hızlı başlangıçta, yalnızca cihaz tarafı uygulamayı çalıştırmak için yönergeleri kapsar. Aşağıdaki hızlı başlangıçlar, kullanılabilir bir eşlik eden bir hizmet tarafı uygulama çalıştırmanız gerekir:
 
   * [Cihaz uygulamaları için iletişim C# aracılığıyla IOT Hub cihaz akışları](./quickstart-device-streams-echo-csharp.md)

   * [Nodejs uygulamalarında cihaz IOT Hub cihaz akışları aracılığıyla iletişim kuran](./quickstart-device-streams-echo-nodejs.md).

Bu hızlı başlangıçta cihaz tarafı C uygulamada aşağıdaki işlevlere sahiptir:

* Bir cihaz akışını bir IOT cihazına kurun.

* Hizmet tarafı ve yankı geri gönderilen verileri alır.

Kod, veri göndermek ve almak için nasıl kullanılacağını yanı sıra, bir cihaz akış başlatma işlemi gösterilecektir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* Cihaz akışları şu anda önizlemesidir yalnızca IOT hub'ları aşağıdaki bölgelerde oluşturulan için desteklenir:

  * **Orta ABD**

  * **Orta ABD EUAP**

* [Visual Studio 2017](https://www.visualstudio.com/vs/)’yi ['C++ ile masaüstü geliştirme'](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) iş yükü etkinleştirilmiş şekilde yükleyin.

* En son [Git](https://git-scm.com/download/) sürümünü yükleyin.

* Microsoft Azure IOT uzantısı için Azure CLI Cloud Shell Örneğinize eklemek için aşağıdaki komutu çalıştırın. IOT uzantısı, Azure CLI için IOT Hub, IOT Edge ve IOT cihaz sağlama hizmeti (DPS) belirli komutları ekler.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

Bu hızlı başlangıçta, [C için Azure IoT cihaz SDK](iot-hub-device-sdk-c-intro.md)’sını kullanacaksınız. Kopyalama ve oluşturmak için kullanılan bir geliştirme ortamı hazırlar [Azure IOT C SDK'sı](https://github.com/Azure/azure-iot-sdk-c) github'dan. GitHub üzerindeki SDK, bu hızlı başlangıçta yer alan örnek kodu içerir.

1. İndirme [CMake derleme sistemini](https://cmake.org/download/).

    `CMake` yüklemesine başlamadan **önce** makinenizde Visual Studio önkoşullarının (Visual Studio ve "C++ ile masaüstü geliştirme" iş yükü) yüklenmiş olması önemlidir. Önkoşullar sağlandıktan ve indirme doğrulandıktan sonra, CMake derleme sistemini yükleyin.

2. Komut istemini veya Git Bash kabuğunu açın. Aşağıdaki komutu yürüterek [Azure IoT C SDK'sı](https://github.com/Azure/azure-iot-sdk-c) GitHub deposunu kopyalayın:

    ```
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive -b public-preview
    ```

    Bu işlem birkaç dakika beklemelisiniz.

3. Git deposunun kök dizininde bir `cmake` alt dizini oluşturun ve o klasöre gidin.

    ```
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

4. Aşağıdaki komutları çalıştırın `cmake` SDK geliştirme istemci platformunuza belirli bir sürümünü oluşturmak için dizin.

   * Linux'ta:

      ```bash
      cmake ..
      make -j
      ```

   * Windows Visual Studio 2015 veya 2017 için geliştirici Komut İstemi'nde aşağıdaki komutları çalıştırın. `cmake` dizininde simülasyon cihazı için bir Visual Studio çözümü de oluşturulur.

      ```cmd
      rem For VS2015
      cmake .. -G "Visual Studio 14 2015"

      rem Or for VS2017
      cmake .. -G "Visual Studio 15 2017"

      rem Then build the project
      cmake --build . -- /m /p:Configuration=Release
      ```

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub-device-streams](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu bölümde, bir simülasyon cihazını kaydetmek için [IoT uzantısı](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) ile Azure Cloud Shell'i kullanacaksınız.

1. Cihaz kimliği oluşturmak için Azure Cloud Shell'de aşağıdaki komutu çalıştırın.

   **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adıyla değiştirin.

   **Cihazım**: Bu, kayıtlı bir cihaz için verilen addır. Cihazım gösterildiği gibi kullanın. Cihazınız için farklı bir ad seçerseniz bu makalenin geri kalan bölümünde aynı adı kullanmanız ve örnek uygulamaları çalıştırmadan önce bunlarda da cihaz adını güncelleştirmeniz gerekir.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

2. Yeni kaydettiğiniz cihazın *cihaz bağlantı dizesini* almak için aşağıdaki komutları Azure Cloud Shell'de çalıştırın:

   **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adıyla değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyDevice --output table
    ```

    Aşağıdaki gibi görünen cihaz bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDevice;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız.

## <a name="communicate-between-device-and-service-via-device-streams"></a>Cihaz ve hizmet aracılığıyla cihaz akışları arasında iletişim

Bu bölümde, hem cihaz tarafında uygulama hem de hizmet tarafı uygulamayı çalıştırın ve ikisi arasındaki iletişim.

### <a name="run-the-device-side-application"></a>Aygıt tarafı uygulamayı çalıştırın

Aygıt tarafı uygulamayı çalıştırmak için aşağıdaki adımları gerçekleştirmeniz gerekir:

1. Kaynak dosyasını düzenleyerek cihaz kimlik bilgilerinizi sağlayın `iothub_client_c2d_streaming_sample.c` klasöründe `iothub_client/samples/iothub_client_c2d_streaming_sample` ve cihaz bağlantısı dizeniz sağlama.

   ```C
   /* Paste in your iothub connection string  */
   static const char* connectionString = "[device connection string]";
   ```

2. Kod şu şekilde derleyin:

   ```bash
   # In Linux
   # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_sample
   make -j
   ```

   ```cmd
   rem In Windows
   rem Go to the cmake folder at the root of repo
   cmake --build . -- /m /p:Configuration=Release
   ```

3. Derlenmiş bir program çalıştırın:

   ```bash
   # In Linux
   # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_sample
   ./iothub_client_c2d_streaming_sample
   ```

   ```cmd
   rem In Windows
   rem Go to the sample's release folder cmake\iothub_client\samples\iothub_client_c2d_streaming_sample\Release
   iothub_client_c2d_streaming_sample.exe
   ```

### <a name="run-the-service-side-application"></a>Hizmet tarafı uygulamayı çalıştırın

Daha önce belirtildiği gibi IOT Hub C SDK'sı cihaz tarafında yalnızca cihaz akışlarını destekler. Hizmet tarafı uygulaması derleme ve çalıştırma için şu hızlı başlangıçlarda birinde kullanılabilir adımları izleyin:

* [Bir cihaz uygulaması için iletişim C# aracılığıyla IOT Hub cihaz akışları](./quickstart-device-streams-echo-csharp.md)

* [Cihaz uygulamasında Node.js IOT Hub cihaz akışları aracılığıyla iletişim kurmak](./quickstart-device-streams-echo-nodejs.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources-device-streams](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir IOT hub'ı ayarladınız, kayıtlı bir cihaz, bir cihaz akışını cihaz üzerinde bir C uygulaması ve hizmet tarafında başka bir uygulama arasında kurulan ve akış uygulamaları arasında sürekli veri göndermek için kullanılan.

Cihaz akışları hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kullanın:

> [!div class="nextstepaction"]
> [Cihaz akışları genel bakış](./iot-hub-device-streams-overview.md)