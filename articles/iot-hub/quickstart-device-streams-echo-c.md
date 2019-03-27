---
title: Azure IOT Hub cihaz akışları C hızlı (Önizleme) | Microsoft Docs
description: Bu hızlı başlangıçta, bir cihaz akışı bir IOT cihazı ile iletişim kuran bir C Hizmet tarafı uygulamalar çalışır.
author: rezasherafat
manager: briz
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: rezas
ms.openlocfilehash: 6a0fd87c787108935430ca43310a662418833c96
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58480762"
---
# <a name="quickstart-communicate-to-a-device-application-in-c-via-iot-hub-device-streams-preview"></a>Hızlı Başlangıç: C cihaz uygulama IOT Hub cihaz akışları (Önizleme) ile iletişim

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

Microsoft Azure IOT Hub cihaz akışları olarak şu anda destekleyen bir [önizleme özelliği](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[IOT Hub cihaz akışları](./iot-hub-device-streams-overview.md) güvenli ve güvenlik duvarı uyumlu bir şekilde iletişim kurmak hizmet ve cihaz uygulamalarınıza izin verin. Genel Önizleme süresince C SDK'sı cihaz tarafında yalnızca cihaz akışlarını destekler. Sonuç olarak, bu hızlı başlangıçta, yalnızca cihaz tarafı uygulamayı çalıştırmak için yönergeleri kapsar. Kullanılabilir Hizmet tarafı eşlik eden bir uygulama, çalışması gerektiğini [ C# hızlı](./quickstart-device-streams-echo-csharp.md) veya [Node.js Hızlı Başlangıç](./quickstart-device-streams-echo-nodejs.md).

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

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

Bu hızlı başlangıçta, [C için Azure IoT cihaz SDK](iot-hub-device-sdk-c-intro.md)’sını kullanacaksınız. Kopyalama ve oluşturmak için kullanılan bir geliştirme ortamı hazırlar [Azure IOT C SDK'sı](https://github.com/Azure/azure-iot-sdk-c) github'dan. GitHub üzerindeki SDK, bu hızlı başlangıçta yer alan örnek kodu içerir. 

1. İndirme [CMake derleme sistemini](https://cmake.org/download/). İndirdiğiniz sürümüne karşılık gelen şifreleme karması değerini kullanarak indirilen ikili doğrulayın. Şifreleme karma değerlerini de zaten sağlanan CMake karşıdan yükleme bağlantısını yer alır.

    Aşağıdaki örnek, şifreleme karması x64 3.13.4 sürümü için doğrulamak için Windows PowerShell kullanılan MSI dağıtım:

    ```powershell
    PS C:\Downloads> $hash = get-filehash .\cmake-3.13.4-win64-x64.msi
    PS C:\Downloads> $hash.Hash -eq "64AC7DD5411B48C2717E15738B83EA0D4347CD51B940487DFF7F99A870656C09"
    True
    ```

    Aşağıdaki sürüm 3.13.4 karma değerlerini bu makalenin yazıldığı sırada CMake sitesinde listelenen:

    ```
    563a39e0a7c7368f81bfa1c3aff8b590a0617cdfe51177ddc808f66cc0866c76  cmake-3.13.4-Linux-x86_64.tar.gz
    7c37235ece6ce85aab2ce169106e0e729504ad64707d56e4dbfc982cb4263847  cmake-3.13.4-win32-x86.msi
    64ac7dd5411b48c2717e15738b83ea0d4347cd51b940487dff7f99a870656c09  cmake-3.13.4-win64-x64.msi
    ```

    `CMake` yüklemesine başlamadan **önce** makinenizde Visual Studio önkoşullarının (Visual Studio ve "C++ ile masaüstü geliştirme" iş yükü) yüklenmiş olması önemlidir. Önkoşullar sağlandıktan ve indirme doğrulandıktan sonra, CMake derleme sistemini yükleyin.

2. Komut istemini veya Git Bash kabuğunu açın. Aşağıdaki komutu yürüterek [Azure IoT C SDK'sı](https://github.com/Azure/azure-iot-sdk-c) GitHub deposunu kopyalayın:
    
    ```
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive -b public-preview
    ```

    Bu deponun boyutu şu anda 220 MB kadardır.

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

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu bölümde, bir simülasyon cihazını kaydetmek için [IoT uzantısı](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) ile Azure Cloud Shell'i kullanacaksınız.

1. Aşağıdaki komutları Azure Cloud Shell'de çalıştırarak IoT Hub CLI uzantısını ekleyin ve cihaz kimliğini oluşturun. 

   **YourIoTHubName**: Aşağıda bu yer tutucu IOT hub'ınız için seçtiğiniz adıyla değiştirin.

   **Cihazım**: Bu, kayıtlı bir cihaz için verilen addır. Cihazım gösterildiği gibi kullanın. Cihazınız için farklı bir ad seçerseniz bu makalenin geri kalan bölümünde aynı adı kullanmanız ve örnek uygulamaları çalıştırmadan önce bunlarda da cihaz adını güncelleştirmeniz gerekir.

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
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

## <a name="communicate-between-device-and-service-via-device-streams"></a>Cihaz ve hizmet aracılığıyla cihaz akışları arasında iletişim

### <a name="run-the-device-side-application"></a>Aygıt tarafı uygulamayı çalıştırın

Aygıt tarafı uygulamayı çalıştırmak için aşağıdaki adımları gerçekleştirmeniz gerekir:

1. Kaynak dosyasını düzenleyerek cihaz kimlik bilgilerinizi sağlayın `iothub_client/samples/iothub_client_c2d_streaming_sample/iothub_client_c2d_streaming_sample.c` ve cihaz bağlantısı dizeniz sağlama.

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

Daha önce belirtildiği gibi IOT Hub C SDK'sı cihaz tarafında yalnızca cihaz akışlarını destekler. Hizmet tarafı uygulaması derleme ve çalıştırma amacıyla bulunan adımları izleyin [ C# hızlı](./quickstart-device-streams-echo-csharp.md) veya [Node.js Hızlı Başlangıç](./quickstart-device-streams-echo-nodejs.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir IOT hub'ı ayarladınız, kayıtlı bir cihaz, bir cihaz akışını cihaz üzerinde bir C uygulaması ve hizmet tarafında başka bir uygulama arasında kurulan ve akış uygulamaları arasında sürekli veri göndermek için kullanılan.

Cihaz akışları hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kullanın:

> [!div class="nextstepaction"]
> [Cihaz akışları genel bakış](./iot-hub-device-streams-overview.md)