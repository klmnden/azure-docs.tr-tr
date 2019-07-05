---
title: C cihaz uygulamasında Azure IOT Hub cihaz akışları (Önizleme) aracılığıyla kullanıcılara | Microsoft Docs
description: Bu hızlı başlangıçta, iletişim kuran bir C cihaz tarafında uygulama bir cihaza akış bir IOT cihazı ile çalıştırın.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: robinsh
ms.openlocfilehash: 4b6f987c68f9fe3ef95c82017b7d8be1d83083ea
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446123"
---
# <a name="quickstart-communicate-to-a-device-application-in-c-via-iot-hub-device-streams-preview"></a>Hızlı Başlangıç: C cihaz uygulama IOT Hub cihaz akışları (Önizleme) ile iletişim

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

Azure IOT Hub cihaz akışları olarak şu anda destekleyen bir [önizleme özelliği](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[IOT Hub cihaz akışları](iot-hub-device-streams-overview.md) güvenli ve güvenlik duvarı uyumlu bir şekilde iletişim kurmak hizmet ve cihaz uygulamalarınıza izin verin. Genel Önizleme süresince C SDK'sı cihaz tarafında yalnızca cihaz akışlarını destekler. Sonuç olarak, bu hızlı başlangıçta yalnızca cihaz tarafı uygulamayı çalıştırmak için yönergeler içerir. Eşlik eden bir hizmet tarafı uygulamayı çalıştırmak için bkz:
 
   * [Cihaz uygulamaları kullanıcılara C# aracılığıyla IOT Hub cihaz akışları](./quickstart-device-streams-echo-csharp.md)
   * [Node.js uygulamalarında cihaz IOT Hub cihaz akışları aracılığıyla iletişim kurar](./quickstart-device-streams-echo-nodejs.md)

Bu hızlı başlangıçta cihaz tarafı C uygulamada aşağıdaki işlevlere sahiptir:

* Bir cihaz akışını bir IOT cihazına kurun.
* Hizmet tarafı uygulama ve yankı geri gönderilen verileri alır.

Kod başlatma işlemi cihaz akışını yanı sıra, veri göndermek ve almak için nasıl kullanılacağını gösterir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* Cihaz akışları önizlemesi, şu anda şu bölgelerde oluşturulan yalnızca IOT hub'ları için desteklenir:

  * Orta ABD
  * Orta ABD EUAP

* Yükleme [Visual Studio 2017](https://www.visualstudio.com/vs/) ile [ile masaüstü geliştirme C++ ](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) iş yükünün etkinleştirilmiş.

* En son [Git](https://git-scm.com/download/) sürümünü yükleyin.

* Azure IOT uzantısı için Azure CLI Cloud Shell Örneğinize eklemek için aşağıdaki komutu çalıştırın. IOT Hub, IOT Edge ve IOT cihaz sağlama hizmeti (DPS) IOT uzantısını ekler-Azure CLI için özel komutları.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

Bu Hızlı Başlangıç için kullandığınız [C için Azure IOT cihaz SDK'sını](iot-hub-device-sdk-c-intro.md). Kopyalama ve oluşturmak için kullanılan bir geliştirme ortamı hazırlama [Azure IOT C SDK'sı](https://github.com/Azure/azure-iot-sdk-c) github'dan. GitHub üzerinde SDK'sı, bu hızlı başlangıçta kullanılan örnek kodu içerir.

1. İndirme [CMake derleme sistemini](https://cmake.org/download/).

    CMake yüklemesi başlamadan önce önemli olduğu, Visual Studio önkoşulları (Visual Studio ve *ile masaüstü geliştirme C++*  iş yükü), makinenizde yüklü. Önkoşulların yerinde olduğundan ve yüklemeyi doğruladıktan sonra CMake derleme sistemini yükleyebilirsiniz.

2. Komut istemini veya Git Bash kabuğunu açın. Aşağıdaki komutu yürüterek [Azure IoT C SDK'sı](https://github.com/Azure/azure-iot-sdk-c) GitHub deposunu kopyalayın:

    ```
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive -b public-preview
    ```

    Bu işlem birkaç dakika sürer.

3. Oluşturma bir *cmake* aşağıdaki komutta gösterildiği ve bu klasöre gidin, Git deposunun kök dizininde alt.

    ```
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

4. Aşağıdaki komutları çalıştırın *cmake* geliştirme istemci Platformunuza özgü SDK'sı sürümünü oluşturmak için dizin.

   * Linux'ta:

      ```bash
      cmake ..
      make -j
      ```

   * Windows Visual Studio 2015 veya 2017 için geliştirici Komut İstemi'nde aşağıdaki komutları çalıştırın. Sanal cihaz için bir Visual Studio çözümü içinde oluşturulacağı *cmake* dizin.

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

Bağlanabilmesi için önce bir cihaz IOT hub'ınıza kaydetmeniz gerekir. Bu bölümde, Azure Cloud Shell ile kullandığınız [IOT uzantısı](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) bir simülasyon cihazı kaydedeceksiniz.

1. Cihaz kimliği oluşturma için Cloud Shell'de aşağıdaki komutu çalıştırın:

   > [!NOTE]
   > * Değiştirin *YourIoTHubName* yer tutucu IOT hub'ınız için seçtiğiniz ada sahip.
   > * Kullanım *Cihazım*gösterildiği gibi. Kayıtlı cihaz için verilen addır. Cihazınız için farklı bir ad seçerseniz, bu makalenin tamamında bu adı kullanın ve bunları çalıştırmadan önce örnek uygulamalar, cihaz adını güncelleştirin.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

2. Alınacak *cihaz bağlantı dizesini* yalnızca kayıtlı bir cihaz için Cloud Shell'de aşağıdaki komutları çalıştırın:

   > [!NOTE]
   > Değiştirin *YourIoTHubName* yer tutucu IOT hub'ınız için seçtiğiniz ada sahip.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyDevice --output table
    ```

    Bu hızlı başlangıçta daha sonra kullanmak için cihaz bağlantı dizesini not edin. Aşağıdaki örneğe benzer şekilde görünür:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDevice;SharedAccessKey={YourSharedAccessKey}`

## <a name="communicate-between-the-device-and-the-service-via-device-streams"></a>Cihaz ve cihaz akışları aracılığıyla hizmeti arasında iletişim

Bu bölümde, hem cihaz tarafında uygulama hem de hizmet tarafı uygulamayı çalıştırın ve ikisi arasındaki iletişim.

### <a name="run-the-device-side-application"></a>Aygıt tarafı uygulamayı çalıştırın

Aygıt tarafı uygulamayı çalıştırmak için aşağıdakileri yapın:

1. Düzenleyerek cihaz kimlik bilgilerinizi sağlayın *iothub_client_c2d_streaming_sample.c* kaynak dosyada *iothub_client/samples/iothub_client_c2d_streaming_sample* klasörünü ve ardından sağlama cihaz bağlantı dizesi.

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

Daha önce belirtildiği gibi IOT Hub C SDK'sı cihaz tarafında yalnızca cihaz akışlarını destekler. Hizmet tarafı uygulaması derleme ve çalıştırma için şu hızlı başlangıçlarda birindeki yönergeleri izleyin:

* [Bir cihaz uygulaması için iletişim C# aracılığıyla IOT Hub cihaz akışları](./quickstart-device-streams-echo-csharp.md)
* [Node.js IOT Hub cihaz akışları aracılığıyla bir cihaz uygulaması için iletişim](./quickstart-device-streams-echo-nodejs.md)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources-device-streams](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir IOT hub'ı ayarladınız, kayıtlı bir cihaz, bir cihaz akışını cihaz üzerinde bir C uygulaması ve hizmet tarafında başka bir uygulama arasında kurulan ve akış uygulamaları arasında sürekli veri göndermek için kullanılan.

Cihaz akışları hakkında daha fazla bilgi için bkz:

> [!div class="nextstepaction"]
> [Cihaz akışları genel bakış](./iot-hub-device-streams-overview.md)
