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
ms.date: 03/14/2019
ms.author: rezas
ms.openlocfilehash: 1468268e407eeac6196c8e8e4db0fc5a52ca09c7
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59501578"
---
# <a name="quickstart-sshrdp-over-iot-hub-device-streams-using-c-proxy-application-preview"></a>Hızlı Başlangıç: SSH/C Ara sunucu uygulamasını (Önizleme) kullanarak IOT Hub cihaz akışları üzerinden RDP

[!INCLUDE [iot-hub-quickstarts-4-selector](../../includes/iot-hub-quickstarts-4-selector.md)]

Microsoft Azure IOT Hub cihaz akışları olarak şu anda destekleyen bir [önizleme özelliği](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[IOT Hub cihaz akışları](./iot-hub-device-streams-overview.md) güvenli ve güvenlik duvarı uyumlu bir şekilde iletişim kurmak hizmet ve cihaz uygulamalarınıza izin verin. Bkz: [bu sayfayı](./iot-hub-device-streams-overview.md#local-proxy-sample-for-ssh-or-rdp) Kurulum genel bakış.

Bu belgede, cihaz akışları aracılığıyla SSH trafiği (22 numaralı bağlantı noktasını kullanarak) tünel Kurulumu açıklanmaktadır. RDP trafiği için Kurulum benzer ve basit bir yapılandırma değişikliği gerektiriyor. Cihaz akışlar, uygulama ve protokolü belirsiz olduğundan, mevcut Hızlı Başlangıç (iletişim bağlantı noktalarını değiştirerek) değiştirilebilir uygulama trafiği diğer türleri uyum sağlamak için.

## <a name="how-it-works"></a>Nasıl çalışır?

Aşağıdaki şekilde, cihaz ve hizmet yerel proxy programları SSH istemcisi ve SSH arka plan işlemleri arasında uçtan uca bağlantı nasıl etkinleştirir, Kurulum gösterilmektedir. Genel Önizleme süresince C SDK'sı cihaz tarafında yalnızca cihaz akışlarını destekler. Sonuç olarak, bu hızlı başlangıçta, yalnızca cihaz yerel ara sunucu uygulamasını çalıştırmak için yönergeleri kapsar. İçinde kullanılabilir olan eşlik eden bir hizmet yerel ara sunucu uygulamasını çalıştırmalısınız [ C# hızlı](./quickstart-device-streams-proxy-csharp.md) veya [Node.js Hızlı Başlangıç](./quickstart-device-streams-proxy-nodejs.md) Kılavuzlar.

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
    Bu işlemin tamamlanması için birkaç dakika beklemeniz gerekebilir.

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

## <a name="ssh-to-a-device-via-device-streams"></a>Bir cihazın cihaz akışları yoluyla SSH

### <a name="run-the-device-local-proxy-application"></a>Cihaz yerel ara sunucu uygulamasını çalıştırın

1. Kaynak dosyayı düzenlemek `iothub_client/samples/iothub_client_c2d_streaming_proxy_sample/iothub_client_c2d_streaming_proxy_sample.c` cihaz bağlantısı dizeniz, hedef cihaz IP/ana bilgisayar adı ve SSH bağlantı noktası 22 girin:

   ```C
   /* Paste in the your iothub connection string  */
   static const char* connectionString = "[Connection string of IoT Hub]";
   static const char* localHost = "[IP/Host of your target machine]"; // Address of the local server to connect to.
   static const size_t localPort = 22; // Port of the local server to connect to.
   ```

2. Örnek derleme:

   ```bash
    # In Linux
    # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_proxy_sample
    make -j
   ```

   ```cmd
    rem In Windows
    rem Go to cmake at root of repository
    cmake --build . -- /m /p:Configuration=Release
   ```

3. Derlenmiş programın cihazda çalıştırın:

   ```bash
    # In Linux
    # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_proxy_sample
    ./iothub_client_c2d_streaming_proxy_sample
   ```

   ```cmd
    rem In Windows
    rem Go to the sample's release folder cmake\iothub_client\samples\iothub_client_c2d_streaming_proxy_sample\Release
    iothub_client_c2d_streaming_proxy_sample.exe
   ```

### <a name="run-the-service-local-proxy-application"></a>Hizmet yerel ara sunucu uygulamasını çalıştırın

Açıklandığı gibi [önceden](#how-it-works), SSH trafiği tünel oluşturmak için bir uçtan uca stream oluşturulması için bir yerel ara her uçtaki (hem de hizmet ve cihaz) gerekir. Genel Önizleme sırasında IOT Hub C SDK'sı cihaz tarafında yalnızca cihaz akışlarını destekler. Derleme ve hizmet yerel proxy çalıştırmak için bulunan adımları [ C# hızlı](./quickstart-device-streams-proxy-csharp.md) veya [Node.js Hızlı Başlangıç](./quickstart-device-streams-proxy-nodejs.md).

### <a name="establish-an-ssh-session"></a>Bir SSH oturumu oluşturur

Cihaz ve hizmet yerel proxy'leri çalıştırıldıktan sonra SSH istemcisi programınız kullanın ve (yerine SSH arka plan programı doğrudan) 2222 numaralı bağlantı noktasında service-yerel ara sunucuya bağlanın.

```cmd/sh
ssh <username>@localhost -p 2222
```

Bu noktada kimlik bilgilerinizi girmeniz için SSH oturum açma istemi ile sunulur.

Konsol çıktısı SSH arka plan programı bağlanan cihazın yerel proxy'de `IP_address:22`: ![Alternatif metin](./media/quickstart-device-streams-proxy-c/device-console-output.PNG "cihaz yerel proxy çıkış")

Konsol çıktısı SSH istemcisi programının (SSH istemcisi iletişim kurar, yerel hizmet proxy dinlediği bağlantı noktası 22'yi bağlanarak SSH arka plan programı için): ![Alternatif metin](./media/quickstart-device-streams-proxy-csharp/ssh-console-output.png "SSH istemcisi çıkış")

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir IOT hub'ı ayarladınız, kayıtlı bir cihaz, cihaz - ve bir IOT Hub cihaz akışı oluşturmak için bir proxy hizmeti-yerel program dağıttıktan ve proxy'ler SSH trafiği tünel oluşturmak için kullanılır.

Cihaz akışları hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kullanın:

> [!div class="nextstepaction"]
> [Cihaz akışları genel bakış](./iot-hub-device-streams-overview.md)
