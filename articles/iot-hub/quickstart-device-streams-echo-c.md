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
ms.date: 01/15/2019
ms.author: rezas
ms.openlocfilehash: 24b00a589454bfa8413cd98407c2022671cb92ce
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54887963"
---
# <a name="quickstart-communicate-to-a-device-application-in-c-via-iot-hub-device-streams-preview"></a>Hızlı Başlangıç: C cihaz uygulama IOT Hub cihaz akışları (Önizleme) ile iletişim

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

[IOT Hub cihaz akışları](./iot-hub-device-streams-overview.md) güvenli ve güvenlik duvarı uyumlu bir şekilde iletişim kurmak hizmet ve cihaz uygulamalarınıza izin verin. Genel Önizleme süresince C SDK'sı cihaz tarafında yalnızca cihaz akışlarını destekler. Sonuç olarak, bu hızlı başlangıçta, yalnızca cihaz tarafı uygulamayı çalıştırmak için yönergeleri kapsar. Kullanılabilir Hizmet tarafı eşlik eden bir uygulama, çalışması gerektiğini [ C# hızlı](./quickstart-device-streams-echo-csharp.md) veya [Node.js Hızlı Başlangıç](./quickstart-device-streams-echo-nodejs.md) Kılavuzlar.

Bu hızlı başlangıçta cihaz tarafı C uygulamada aşağıdaki işlevlere sahiptir:

* Bir cihaz akışını bir IOT cihazına kurun.

* Hizmet tarafı ve yankı geri gönderilen verileri alır.

Kod, veri göndermek ve almak için nasıl kullanılacağını yanı sıra, bir cihaz akış başlatma işlemi gösterilecektir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* [Visual Studio 2017](https://www.visualstudio.com/vs/)’yi ['C++ ile masaüstü geliştirme'](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) iş yükü etkinleştirilmiş şekilde yükleyin.
* En son [Git](https://git-scm.com/download/) sürümünü yükleyin.

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

Bu hızlı başlangıçta, [C için Azure IoT cihaz SDK](iot-hub-device-sdk-c-intro.md)’sını kullanacaksınız. Kopyalama ve oluşturmak için kullanılan bir geliştirme ortamı hazırlar [Azure IOT C SDK'sı](https://github.com/Azure/azure-iot-sdk-c) github'dan. GitHub üzerindeki SDK, bu hızlı başlangıçta yer alan örnek kodu içerir. 


1. 3.11.4 sürümünü indirin [CMake derleme sistemini](https://cmake.org/download/). İlgili şifreleme karması değerini kullanarak indirilen ikili dağıtımı doğrulayın. Aşağıdaki örnekte, x64 MSI dağıtımı 3.11.4 sürümünün şifreleme karmasını doğrulamak için Windows PowerShell kullanılır:

    ```PowerShell
    PS C:\Downloads> $hash = get-filehash .\cmake-3.11.4-win64-x64.msi
    PS C:\Downloads> $hash.Hash -eq "56e3605b8e49cd446f3487da88fcc38cb9c3e9e99a20f5d4bd63e54b7a35f869"
    True
    ```
    
    Bu metnin yazıldığı tarihte CMake sitesinde 3.11.4 sürümü için şu karma değerleri listeleniyordu:

    ```
    6dab016a6b82082b8bcd0f4d1e53418d6372015dd983d29367b9153f1a376435  cmake-3.11.4-Linux-x86_64.tar.gz
    72b3b82b6d2c2f3a375c0d2799c01819df8669dc55694c8b8daaf6232e873725  cmake-3.11.4-win32-x86.msi
    56e3605b8e49cd446f3487da88fcc38cb9c3e9e99a20f5d4bd63e54b7a35f869  cmake-3.11.4-win64-x64.msi
    ```

2. Komut istemini veya Git Bash kabuğunu açın. Aşağıdaki komutu yürüterek [Azure IoT C SDK'sı](https://github.com/Azure/azure-iot-sdk-c) GitHub deposunu kopyalayın:
    
    ```
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive
    ```

    Bu deponun boyutu şu anda 220 MB kadardır.

3. Git deposunun kök dizininde bir `cmake` alt dizini oluşturun ve o klasöre gidin. 

    ```
    cd azure-iot-sdk-c
    git checkout public-preview
    mkdir cmake
    cd cmake
    ```

4. SDK’nın geliştirme istemci platformunuza ve özgü bir sürümünü derleyen aşağıdaki komutu çalıştırın. Windows sanal cihaz için bir Visual Studio çözümü içinde oluşturulacağı `cmake` dizin. 

```
    # In Linux
    cmake ..
    make -j
```

Windows içinde Visual Studio 2015 veya 2017 istemi için geliştirici Komut İstemi'nde aşağıdaki komutları çalıştırın:

```
    # In Windows
    # For VS2015
    cmake .. -G "Visual Studio 15 2015"
    
    # Or for VS2017
    cmake .. -G "Visual Studio 15 2017

    # Then build the project
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
- Bu yönergeleri kullanarak geliştirme ortamınızı ayarlama [cihaz akışları hakkında daha fazla makale](https://github.com/Azure/azure-iot-sdk-c-tcpstreaming/blob/master/iothub_client/readme.md#compiling-the-device-sdk-for-c).

- Kaynak dosyasını düzenleyerek cihaz kimlik bilgilerinizi sağlayın `iothub_client/samples/iothub_client_c2d_streaming_sample/iothub_client_c2d_streaming_sample.c` ve cihaz bağlantısı dizeniz sağlayın.
```C
  /* Paste in the your iothub connection string  */
  static const char* connectionString = "[device connection string]";
```

- Kod şu şekilde derleyin:

```
  # In Linux
  # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_sample
  make -j


  # In Windows
  # Go to the cmake folder at the root of repo
  cmake --build . -- /m /p:Configuration=Release
```

- Derlenmiş bir program çalıştırın:

```
  # In Linux
  # Go to sample's folder
  cmake/iothub_client/samples/iothub_client_c2d_streaming_sample
  ./iothub_client_c2d_streaming_sample


  # In Windows
  # Go to sample's release folder
  cmake\iothub_client\samples\iothub_client_c2d_streaming_sample\Release
  iothub_client_c2d_streaming_sample.exe
```

### <a name="run-the-service-side-application"></a>Hizmet tarafı uygulamayı çalıştırın

Daha önce belirtildiği gibi IOT Hub C SDK'sı cihaz tarafında yalnızca cihaz akışlarını destekler. Hizmet tarafı, kullanılabilir eşlik eden hizmet programlarını kullanmanız [ C# hızlı](./quickstart-device-streams-echo-csharp.md) veya [Node.js Hızlı Başlangıç](./quickstart-device-streams-echo-nodejs.md) Kılavuzlar.


## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir IOT hub'ı ayarladınız, bir C uygulaması kullanarak hub'a sanal telemetri gönderilen, bir cihaz kaydetmiş ve Azure Cloud Shell'i kullanarak hub'dan telemetri okumak.

Cihaz akışları hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kullanın:

> [!div class="nextstepaction"]
> [Cihaz akışları genel bakış](./iot-hub-device-streams-overview.md)