---
title: Azure IoT Hub’a telemetri gönderme hızlı başlangıcı (C) | Microsoft Docs
description: Bu hızlı başlangıçta bir IoT hub’a sanal telemetri göndermek ve bulutta işlemek üzere IoT hub’dan gelen telemetriyi okumak için iki örnek C uygulaması çalıştırırsınız.
author: wesmc7777
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: quickstart
ms.custom: mvc
ms.date: 08/27/2018
ms.author: wesmc
ms.openlocfilehash: 1bb21f6decc725c47f135c9842a2ba6d8989f693
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2018
ms.locfileid: "51515180"
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-and-read-the-telemetry-from-the-hub-with-a-back-end-application-c"></a>Hızlı Başlangıç: Bir cihazdan IoT hub’ına telemetri gönderme ve arka uç uygulaması ile hub’dan telemetriyi okuma (C)

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

IoT Hub, IoT cihazlarınızdan buluta depolama veya işleme amacıyla yüksek hacimlerde telemetri almanızı sağlayan bir Azure hizmetidir. Bu hızlı başlangıçta, bir simülasyon cihazı uygulamasından bir arka uç uygulamasına işlenmek üzere IoT Hub aracılığıyla telemetri gönderirsiniz.

Bu hızlı başlangıçta, IoT hub’ına telemetri verileri göndermek için [C için Azure IoT cihaz SDK](iot-hub-device-sdk-c-intro.md)’sındaki bir C örnek uygulaması kullanılmaktadır. Azure IoT cihaz SDK’ları, taşınabilirlik ve geniş platform uyumluluğu için [ANSI C (C99)](https://wikipedia.org/wiki/C99) ile yazılır. Örnek kodu çalıştırmadan önce bir IoT hub’ı oluşturur ve simülasyon cihazını o hub’a kaydedersiniz.

Bu makale, Windows için yazılmıştır, ancak bu hızlı başlangıcı Linux üzerinde de tamamlayabilirsiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* [Visual Studio 2017](https://www.visualstudio.com/vs/)’yi ['C++ ile masaüstü geliştirme'](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) iş yükü etkinleştirilmiş şekilde yükleyin.
* En son [Git](https://git-scm.com/download/) sürümünü yükleyin.

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

Bu hızlı başlangıçta, [C için Azure IoT cihaz SDK](iot-hub-device-sdk-c-intro.md)’sını kullanacaksınız. 

Aşağıdaki ortamlar için paketleri ve kitaplıkları yükleyerek SDK’yı kullanabilirsiniz:

* **Linux**: apt-get paketleri; amd64, arm64, armhf ve i386 CPU mimarileri kullanılarak Ubuntu 16.04 ve 18.04 için kullanılabilir. Daha fazla bilgi için bkz. [Ubuntu’da C cihaz istemcisi oluşturmak için apt-get kullanma](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/ubuntu_apt-get_sample_setup.md).

* **mbed**: Mbed platformunda cihaz uygulamaları oluşturan geliştiriciler için, dakikalar içinde Azure IoT Hub’ı kullanmaya başlamanızı sağlayacak bir kitaplık ve örnekler yayımladık. Daha fazla bilgi için bkz. [Mbed kitaplığını kullanma](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/readme.md#mbed).

* **Arduino**: Arduino üzerinde geliştirme yapıyorsanız, Arduino IDE kitaplık yöneticisinde kullanılabilir olan Azure IoT kitaplığından yararlanabilirsiniz. Daha fazla bilgi için bkz. [Arduino için Azure IoT Hub kitaplığı](https://github.com/azure/azure-iot-arduino).

* **iOS**: IoT Hub Cihaz SDK’sı, Mac ve iOS cihaz geliştirmesi için CocoaPods olarak kullanılabilir. Daha fazla bilgi için bkz. [Microsoft Azure IoT için iOS Örnekleri](https://cocoapods.org/pods/AzureIoTHubClient).

Ancak bu bölümde, GitHub’dan [Azure IoT C SDK’sını](https://github.com/Azure/azure-iot-sdk-c) kopyalamak ve derlemek için kullanılan bir geliştirme ortamı hazırlayacaksınız. GitHub üzerindeki SDK, bu hızlı başlangıçta yer alan örnek kodu içerir. 


1. [CMake derleme sistemi](https://cmake.org/download/)'nin 3.11.4 sürümünü indirin. İlgili şifreleme karması değerini kullanarak indirilen ikili dağıtımı doğrulayın. Aşağıdaki örnekte, x64 MSI dağıtımı 3.11.4 sürümünün şifreleme karmasını doğrulamak için Windows PowerShell kullanılır:

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

    `CMake` yüklemesine başlamadan **önce** makinenizde Visual Studio önkoşullarının (Visual Studio ve "C++ ile masaüstü geliştirme" iş yükü) yüklenmiş olması önemlidir. Önkoşullar sağlandıktan ve indirme doğrulandıktan sonra, CMake derleme sistemini yükleyin.

2. Komut istemini veya Git Bash kabuğunu açın. Aşağıdaki komutu yürüterek [Azure IoT C SDK'sı](https://github.com/Azure/azure-iot-sdk-c) GitHub deposunu kopyalayın:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive
    ```
    Bu deponun boyutu şu anda 220 MB kadardır. Bu işlemin tamamlanması için birkaç dakika beklemeniz gerekebilir.


3. Git deposunun kök dizininde bir `cmake` alt dizini oluşturun ve o klasöre gidin. 

    ```cmd/sh
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

4. SDK’nın geliştirme istemci platformunuza ve özgü bir sürümünü derleyen aşağıdaki komutu çalıştırın. `cmake` dizininde simülasyon cihazı için bir Visual Studio çözümü de oluşturulur. 

    ```cmd
    cmake ..
    ```
    
    `cmake`, C++ derleyicinizi bulamazsa yukarıdaki komutu çalıştırırken derleme hatalarıyla karşılaşabilirsiniz. Bu durumda bu komutu [Visual Studio komut isteminde](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs) çalıştırmayı deneyin. 

    Derleme başarılı olduktan sonra, son birkaç çıkış satırı aşağıdaki çıkışa benzer olacaktır:

    ```cmd/sh
    $ cmake ..
    -- Building for: Visual Studio 15 2017
    -- Selecting Windows SDK version 10.0.16299.0 to target Windows 10.0.17134.
    -- The C compiler identification is MSVC 19.12.25835.0
    -- The CXX compiler identification is MSVC 19.12.25835.0

    ...

    -- Configuring done
    -- Generating done
    -- Build files have been written to: E:/IoT Testing/azure-iot-sdk-c/cmake
    ```


## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu bölümde, bir simülasyon cihazını kaydetmek için [IoT uzantısı](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) ile Azure Cloud Shell'i kullanacaksınız.

1. Aşağıdaki komutları Azure Cloud Shell'de çalıştırarak IoT Hub CLI uzantısını ekleyin ve cihaz kimliğini oluşturun. 

   **YourIoTHubName**: Bu yer tutucusunu IoT hub'ınız için seçtiğiniz adla değiştirin.

   **MyCDevice**: Kaydedilen cihaza verilen addır. Gösterilen MyCDevice değerini kullanın. Cihazınız için farklı bir ad seçerseniz bu makalenin geri kalan bölümünde aynı adı kullanmanız ve örnek uygulamaları çalıştırmadan önce bunlarda da cihaz adını güncelleştirmeniz gerekir.

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyCDevice
    ```

2. Yeni kaydettiğiniz cihazın _cihaz bağlantı dizesini_ almak için aşağıdaki komutları Azure Cloud Shell'de çalıştırın:

   **YourIoTHubName**: Bu yer tutucusunu IoT hub'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyCDevice --output table
    ```

    Şu ifadeye benzer şekilde görünen cihaz bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız.

## <a name="send-simulated-telemetry"></a>Sanal telemetri gönderme

Simülasyon cihazı uygulaması, IoT hub’ınız üzerindeki cihaza özgü bir uç noktaya bağlanır ve sanal telemetri olarak bir dize gönderir.

1. Bir metin düzenleyicisi kullanarak iothub_convenience_sample.c kaynak dosyasını açın ve telemetri verileri göndermek için örnek kodu gözden geçirin. Bu dosya, aşağıdaki konumda bulunur:

    ```
    \azure-iot-sdk-c\iothub_client\samples\iothub_convenience_sample\iothub_convenience_sample.c
    ```

2. `connectionString` sabitinin bildirimini bulun:

    ```C
    /* Paste in your device connection string  */
    static const char* connectionString = "[device connection string]";
    ```
    `connectionString` sabitinin değerini, önceden not ettiğiniz cihaz bağlantı dizesiyle değiştirin. Ardından **iothub_convenience_sample.c** üzerindeki değişikliklerinizi kaydedin.

3. Yerel terminal penceresinde, Azure IoT C SDK’sında oluşturduğunuz CMake dizininde yer alan *iothub_convenience_sample* proje dizinine gidin.

    ```
    cd /azure-iot-sdk-c/cmake/iothub_client/samples/iothub_convenience_sample
    ```

4. CMake öğesini yerel terminal pencerenizde çalıştırarak örneği güncelleştirilmiş `connectionString` değeriyle derleyin:

    ```cmd/sh
    cmake --build . --target iothub_convenience_sample --config Debug
    ```

5. Yerel terminal penceresinde, aşağıdaki komutları çalıştırarak simülasyon cihazı uygulamasını çalıştırın:

    ```cmd/sh
    Debug\iothub_convenience_sample.exe
    ```

    Aşağıdaki ekran görüntüsünde, simülasyon cihazı uygulaması, IoT hub’ınıza telemetri verilerini gönderdiğinde oluşan çıktı gösterilmektedir:

    ![Simülasyon cihazını çalıştırma](media/quickstart-send-telemetry-c/simulated-device-app.png)

## <a name="read-the-telemetry-from-your-hub"></a>Hub’ınızdan telemetri okuma


Bu bölümde, simülasyon cihazı tarafından gönderilen cihaz iletilerini izlemek için [IoT uzantısı](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) ile Azure Cloud Shell'i kullanacaksınız.

1. Azure Cloud Shell'i kullanarak, IoT hub’ınızdan gelen iletilere bağlanmak ve bu iletileri okumak için aşağıdaki komutu çalıştırın:

   **YourIoTHubName**: Bu yer tutucusunu IoT hub'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub monitor-events --hub-name YourIoTHubName --output table
    ```

    ![Azure CLI kullanarak cihaz iletilerini okuma](media/quickstart-send-telemetry-c/read-device-to-cloud-messages-app.png)

    

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir IoT hub’ını ayarladınız, bir cihazı kaydettiniz, C uygulamasını kullanarak hub’a sanal telemetri verileri gönderdiniz ve Azure Cloud Shell'i kullanarak hub’dan telemetri verilerini okudunuz.

Azure IoT Hub C SDK’sı ile geliştirme hakkında daha fazla bilgi edinmek için aşağıdaki Nasıl yapılır kılavuzuyla devam edin:

> [!div class="nextstepaction"]
> [Azure IoT Hub C SDK’sı kullanarak geliştirme](iot-hub-devguide-develop-for-constrained-devices.md)