---
title: Azure IOT Hub cihaz akışları C Hızlı Başlangıç için SSH/RDP (Önizleme) | Microsoft Docs
description: Bu hızlı başlangıçta, IOT Hub cihaz akışlar üzerinde SSH/RDP senaryoları etkinleştirmek için bir proxy görevi gören bir örnek C uygulama çalışacaktır.
author: rezasherafat
manager: briz
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: quickstart
ms.custom: mvc
ms.date: 01/15/2019
ms.author: rezas
ms.openlocfilehash: 300b42c9452fc58c857d075a7fd8c42fd6a1c409
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2019
ms.locfileid: "55731742"
---
# <a name="quickstart-sshrdp-over-iot-hub-device-streams-using-c-proxy-application-preview"></a>Hızlı Başlangıç: SSH/C Ara sunucu uygulamasını (Önizleme) kullanarak IOT Hub cihaz akışları üzerinden RDP

[!INCLUDE [iot-hub-quickstarts-4-selector](../../includes/iot-hub-quickstarts-4-selector.md)]

[IOT Hub cihaz akışları](./iot-hub-device-streams-overview.md) güvenli ve güvenlik duvarı uyumlu bir şekilde iletişim kurmak hizmet ve cihaz uygulamalarınıza izin verin. Bkz: [bu sayfayı](./iot-hub-device-streams-overview.md#local-proxy-sample-for-ssh-or-rdp) Kurulum genel bakış.

Bu belgede, cihaz akışları aracılığıyla SSH trafiği (22 numaralı bağlantı noktasını kullanarak) tünel Kurulumu açıklanmaktadır. RDP trafiği için Kurulum benzer ve basit bir yapılandırma değişikliği gerektiriyor. Cihaz akışlar, uygulama ve protokolü belirsiz olduğundan, mevcut Hızlı Başlangıç (iletişim bağlantı noktalarını değiştirerek) değiştirilebilir uygulama trafiği diğer türleri uyum sağlamak için.

## <a name="how-it-works"></a>Nasıl çalışır?
Aşağıdaki şekilde, cihaz ve hizmet yerel proxy programları SSH istemcisi SSH arka plan işlemleri arasında uçtan uca bağlantısı nasıl etkinleştirir, Kurulum gösterilmektedir. Genel Önizleme süresince C SDK'sı cihaz tarafında yalnızca cihaz akışlarını destekler. Sonuç olarak, bu hızlı başlangıçta, yalnızca cihaz yerel ara sunucu uygulamasını çalıştırmak için yönergeleri kapsar. İçinde kullanılabilir olan eşlik eden bir hizmet yerel ara sunucu uygulamasını çalıştırmalısınız [ C# hızlı](./quickstart-device-streams-proxy-csharp.md) veya [Node.js Hızlı Başlangıç](./quickstart-device-streams-proxy-nodejs.md) Kılavuzlar.

![Alternatif metin](./media/quickstart-device-streams-proxy-csharp/device-stream-proxy-diagram.svg "yerel ara Kurulumu")


1. Hizmet yerel proxy, IOT hub'ına bağlanır ve bir cihaz akışını hedef cihaza başlatır.

2. Cihaz yerel proxy akış başlatma el sıkışma işlemi tamamlandıktan ve IOT Hub'ın hizmet tarafına akış uç noktası aracılığıyla uçtan uca bir akış tüneli oluşturur.

3. Cihazda 22 numaralı bağlantı noktasında dinleme SSH arka plan programı (SSHD) cihaz yerel proxy bağlandığında (Bu açıklandığı gibi yapılandırılabilirdir [aşağıda](#run-the device-local-proxy-application)).

4. Kullanıcıdan yeni SSH bağlantıları için bekler, bu durumda 2222 numaralı bağlantı noktasına atanan bir bağlantı noktasında dinleme tarafından hizmet yerel proxy (Bu da açıklandığı gibi yapılandırılabilirdir [aşağıda](#run-the-device-local-proxy-application)). Kullanıcı SSH istemcisi bağlandığında, SSH istemcisi ve sunucusu programlar arasında aktarılmak SSH uygulama trafiği tüneli etkinleştirir.

> [!NOTE]
> Bir cihaz akış üzerinden gönderilen SSH trafiği doğrudan hizmet ve cihaz arasında gönderilen yerine IOT Hub'ın akış uç noktası aracılığıyla tünel. Bu sağlar [yararlar](./iot-hub-device-streams-overview.md#benefits). Ayrıca, aynı cihaza (veya makine) çalıştıran SSH arka plan programı şekilde cihaz yerel proxy olarak gösterilmektedir. Bu hızlı başlangıçta, SSH arka plan programı IP adresi sağlamak için cihaz yerel ara sunucu ve farklı makinelerde de çalıştırmak için arka plan programı sağlar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* [Visual Studio 2017](https://www.visualstudio.com/vs/)’yi ['C++ ile masaüstü geliştirme'](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) iş yükü etkinleştirilmiş şekilde yükleyin.
* En son [Git](https://git-scm.com/download/) sürümünü yükleyin.

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

Bu hızlı başlangıçta, [C için Azure IoT cihaz SDK](iot-hub-device-sdk-c-intro.md)’sını kullanacaksınız. Kopyalama ve oluşturmak için kullanılan bir geliştirme ortamı hazırlar [Azure IOT C SDK'sı](https://github.com/Azure/azure-iot-sdk-c) github'dan. GitHub üzerindeki SDK, bu hızlı başlangıçta yer alan örnek kodu içerir. 


1. ' % S'sürümü 3.11.4 olan [CMake derleme sistemini](https://cmake.org/download/) gelen [GitHub](https://github.com/Kitware/CMake/releases/tag/v3.11.4). İlgili şifreleme karması değerini kullanarak indirilen ikili dağıtımı doğrulayın. Aşağıdaki örnekte, x64 MSI dağıtımı 3.11.4 sürümünün şifreleme karmasını doğrulamak için Windows PowerShell kullanılır:

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
    
    ```
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive -b public-preview
    ```
    Bu deponun boyutu şu anda 220 MB kadardır. Bu işlemin tamamlanması için birkaç dakika beklemeniz gerekebilir.


3. Git deposunun kök dizininde bir `cmake` alt dizini oluşturun ve o klasöre gidin. 

    ```
    cd azure-iot-sdk-c
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
    rem In Windows
    rem For VS2015
    cmake .. -G "Visual Studio 15 2015"

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


## <a name="ssh-to-a-device-via-device-streams"></a>Bir cihazın cihaz akışları yoluyla SSH

### <a name="run-the-device-local-proxy-application"></a>Cihaz yerel ara sunucu uygulamasını çalıştırın

- Kaynak dosyayı düzenlemek `iothub_client/samples/iothub_client_c2d_streaming_proxy_sample/iothub_client_c2d_streaming_proxy_sample.c` ve RDP yanı sıra cihaz bağlantısı dizeniz, hedef cihaz IP/hostname bağlantı noktası 22 sağlayın:
```C
  /* Paste in the your iothub connection string  */
  static const char* connectionString = "[Connection string of IoT Hub]";
  static const char* localHost = "[IP/Host of your target machine]"; // Address of the local server to connect to.
  static const size_t localPort = 22; // Port of the local server to connect to.
```

- Örnek gibi derleme:

```
    # In Linux
    # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_proxy_sample
    make -j
```

```
    rem In Windows
    rem Go to cmake at root of repository
    cmake --build . -- /m /p:Configuration=Release
```

- Derlenmiş programın cihazda çalıştırın:
```
    # In Linux
    # Go to sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_proxy_sample
    ./iothub_client_c2d_streaming_proxy_sample
```

```
    rem In Windows
    rem Go to sample's release folder cmake\iothub_client\samples\iothub_client_c2d_streaming_proxy_sample\Release
    iothub_client_c2d_streaming_proxy_sample.exe
```

### <a name="run-the-service-local-proxy-application"></a>Hizmet yerel ara sunucu uygulamasını çalıştırın

Açıklandığı gibi [yukarıda](#how-it-works) SSH trafiği tünel oluşturmak için bir uçtan uca stream oluşturulması için her iki ucunda (yani, hizmet ve cihaz) yerel bir ara sunucu gerekir. Genel Önizleme sırasında IOT Hub C SDK'sı yalnızca cihaz akışlarını cihaz tarafında ancak destekler. Yerel Hizmet proxy'si için eşlik eden kılavuzları kullanın [ C# hızlı](./quickstart-device-streams-proxy-csharp.md) veya [Node.js Hızlı Başlangıç](./quickstart-device-streams-proxy-nodejs.md) yerine.


### <a name="establish-an-ssh-session"></a>Bir SSH oturumu oluşturur

Cihaz ve hizmet yerel Proxy çalıştıran varsayılarak, artık SSH istemcisi programınız kullanın ve yerel hizmet proxy'si (yerine SSH arka plan programı doğrudan) 2222 numaralı bağlantı noktasında bağlanın. 

```
ssh <username>@localhost -p 2222
```

Bu noktada kimlik bilgilerinizi girmeniz için SSH oturum açma istemi ile sunulur.


Konsol çıktısı SSH arka plan programı bağlanan cihazın yerel proxy'de `IP_address:22`: ![Alternatif metin](./media/quickstart-device-streams-proxy-c/device-console-output.PNG "cihaz yerel proxy çıkış")

Konsol çıktısı SSH istemcisi programının (SSH istemcisi iletişim kuran SSH arka plan programı için burada hizmeti-yerel proxy üzerinde dinleme bağlantı noktası 22 bağlanarak): ![Alternatif metin](./media/quickstart-device-streams-proxy-csharp/ssh-console-output.png "SSH istemcisi çıkış")

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir IOT hub'ı ayarladınız, kayıtlı bir cihaz, cihaz - ve bir IOT Hub cihaz akışı oluşturmak için bir proxy hizmeti-yerel program dağıttıktan ve proxy'ler SSH trafiği tünel oluşturmak için kullanılır.

Cihaz akışları hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kullanın:

> [!div class="nextstepaction"]
> [Cihaz akışları genel bakış](./iot-hub-device-streams-overview.md)
